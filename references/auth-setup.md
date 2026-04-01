# Auth & Setup Reference

## Prerequisites

Before using the CIPP API, you must:
1. Have a running CIPP instance (self-hosted or CyberDrain-hosted)
2. Configure an API client in: **CIPP > Settings > Integrations > CIPP-API**
3. Note your Application ID, Client Secret, and Tenant ID from the integration page

## OAuth2 Client Credentials Flow

### Step 1: Get Access Token

```bash
curl -s -X POST "https://login.microsoftonline.com/${TENANT_ID}/oauth2/v2.0/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=${CLIENT_ID}" \
  -d "client_secret=${CLIENT_SECRET}" \
  -d "scope=api://${CLIENT_ID}/.default" \
  -d "grant_type=client_credentials"
```

Response:
```json
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAi..."
}
```

### Step 2: Call API

```bash
curl -s -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -H "Content-Type: application/json" \
  "https://${CIPP_URL}/api/ListUsers?TenantFilter=contoso.onmicrosoft.com"
```

### Step 3: Token Refresh

Tokens expire after ~1 hour. Request a new token before expiry. There is no refresh_token in client_credentials flow — just request a new access_token.

## PowerShell Authentication

```powershell
# Install module
Install-Module -Name CIPPAPIModule

# Configure (one-time)
Set-CIPPAPIDetails -CIPPClientID "your-client-id" `
  -CIPPClientSecret "your-secret" `
  -CIPPAPIUrl "https://your-cipp-url" `
  -TenantID "your-tenant-id"

# Test
Get-CIPPLogs

# Generic endpoint call
Invoke-CIPPRestMethod -Endpoint "/api/ListUsers" -Method GET -Params @{ TenantFilter = "contoso.onmicrosoft.com" }
```

## Shell Script Helper Pattern

```bash
#!/bin/bash
# cipp-api.sh — reusable CIPP API helper
# Usage: source cipp-api.sh && cipp_get "ListUsers" "TenantFilter=contoso.onmicrosoft.com"

CIPP_URL="${CIPP_URL:?Set CIPP_URL}"
CIPP_TENANT_ID="${CIPP_TENANT_ID:?Set CIPP_TENANT_ID}"
CIPP_CLIENT_ID="${CIPP_CLIENT_ID:?Set CIPP_CLIENT_ID}"
CIPP_CLIENT_SECRET="${CIPP_CLIENT_SECRET:?Set CIPP_CLIENT_SECRET}"

_cipp_token=""
_cipp_token_expiry=0

cipp_auth() {
  local now=$(date +%s)
  if [ "$now" -lt "$_cipp_token_expiry" ] && [ -n "$_cipp_token" ]; then return; fi
  local resp=$(curl -s -X POST "https://login.microsoftonline.com/${CIPP_TENANT_ID}/oauth2/v2.0/token" \
    -d "client_id=${CIPP_CLIENT_ID}&client_secret=${CIPP_CLIENT_SECRET}&scope=api://${CIPP_CLIENT_ID}/.default&grant_type=client_credentials")
  _cipp_token=$(echo "$resp" | jq -r '.access_token')
  local exp=$(echo "$resp" | jq -r '.expires_in')
  _cipp_token_expiry=$((now + exp - 60))
}

cipp_get() {
  cipp_auth
  curl -s -H "Authorization: Bearer $_cipp_token" "${CIPP_URL}/api/$1?$2"
}

cipp_post() {
  cipp_auth
  curl -s -X POST -H "Authorization: Bearer $_cipp_token" -H "Content-Type: application/json" \
    "${CIPP_URL}/api/$1" -d "$2"
}
```

## Self-Hosted API Setup (v7.1+)

For self-hosted instances deployed **before v7.1**, additional setup is needed:

1. Navigate to **CIPP > Settings > Integrations > CIPP-API**
2. Click **Create API Client**
3. Fill in client name and permissions
4. Note the Application ID and generate a Client Secret
5. The API scope is shown on the integration page — copy it for OAuth config

**UI Path:** CIPP > Settings > Integrations > CIPP-API > Create API Client

### Permissions / Roles

API clients can have custom roles assigned. Default roles:
- **Reader** — List* endpoints only
- **Editor** — List*, Add*, Edit* endpoints
- **Admin** — All endpoints including Exec* and Remove*

## n8n Integration Pattern

### HTTP Request Node Setup

1. Create an **OAuth2** credential in n8n:
   - Grant Type: `Client Credentials`
   - Access Token URL: `https://login.microsoftonline.com/{TENANT_ID}/oauth2/v2.0/token`
   - Client ID: your CIPP API client ID
   - Client Secret: your CIPP API client secret
   - Scope: `api://{CLIENT_ID}/.default`
   - Authentication: `Body`

2. Use **HTTP Request** nodes:
   - URL: `https://{CIPP_URL}/api/{endpoint}`
   - Authentication: select the OAuth2 credential above
   - For GET: pass TenantFilter as query parameter
   - For POST: pass TenantFilter in JSON body

### Example n8n Workflow: List All Users

```
HTTP Request Node:
  Method: GET
  URL: https://cipp.example.com/api/ListUsers
  Query Parameters: TenantFilter = contoso.onmicrosoft.com
  Authentication: OAuth2 (configured above)
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| 401 Unauthorized | Check client_id, secret, tenant_id. Ensure API client exists in CIPP. |
| 403 Forbidden | API client lacks required role. Check CIPP > Settings > Integrations > CIPP-API > client roles. |
| Token request fails | Verify tenant ID is correct. Ensure the app registration exists in Azure AD. |
| Slow responses | Heavy API usage slows frontend. Space out bulk operations. |
| 10 min timeout | Long-running operations may timeout. Check CIPP logs for status. |
| Self-hosted 404 | Ensure Azure Functions app is running. Check backend URLs in CIPP > Settings > Backend. |

## API Scope

The API scope follows the pattern: `api://{APPLICATION_ID}/.default`

To find your scope:
1. Go to **CIPP > Settings > Integrations > CIPP-API**
2. Find your API client row
3. Click the three-dot Actions menu → **Copy API Scope**

## Third-Party OAuth Connections

When connecting CIPP to external services that need OAuth:
- Use the copyable fields on the CIPP-API integration page (indicated by blue outlines)
- You'll need the API Scope from the CIPP-API Clients table (Actions → Copy API Scope)
