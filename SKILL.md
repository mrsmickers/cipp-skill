---
name: cipp
description: >
  CIPP (CyberDrain Improved Partner Portal) — comprehensive MSP management for Microsoft 365 and Azure tenants.
  Use when asked about: user management (create, edit, disable, reset password, offboard, MFA, licenses),
  group management (create, edit, list M365/security/distribution groups), device management (Intune, Autopilot),
  mailbox operations (permissions, forwarding, shared mailboxes, quarantine, transport rules, spam filters),
  tenant administration (onboarding, offboarding, domains, GDAP, alerts, audit logs),
  conditional access policies, security (incidents, alerts, Defender, Safe Links),
  standards & compliance (BPA, domain analyser, drift detection),
  Teams & SharePoint management, endpoint management (Intune policies, apps, scripts),
  CIPP settings & extensions (HaloPSA, NinjaOne, Hudu, Sherweb, Cloudflare, GitHub),
  CIPP API automation, n8n/Power Automate integrations with CIPP, or "can CIPP do X?" questions.
---

# CIPP Skill

CIPP is an open-source MSP management platform for Microsoft 365 / Azure tenant administration.
It runs as an Azure Function App (self-hosted or CyberDrain-hosted) with a React/Next.js frontend.

## Auth Pattern (Universal — No Hardcoded Credentials)

CIPP API uses **OAuth2 client_credentials** flow:

```
Token endpoint: https://login.microsoftonline.com/{TENANT_ID}/oauth2/v2.0/token
Scope:          api://{APPLICATION_ID}/.default
Grant type:     client_credentials
```

All API calls use `Authorization: Bearer {token}` header against `https://{CIPP_URL}/api/{endpoint}`.

**Before using API:** An API client must be configured in CIPP UI → Settings → Integrations → CIPP-API.

### Quick Auth (curl)

```bash
# Get token
TOKEN=$(curl -s -X POST "https://login.microsoftonline.com/${TENANT_ID}/oauth2/v2.0/token" \
  -d "client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=api://${CLIENT_ID}/.default&grant_type=client_credentials" \
  | jq -r '.access_token')

# Call API
curl -s -H "Authorization: Bearer $TOKEN" "https://${CIPP_URL}/api/ListUsers?TenantFilter=contoso.onmicrosoft.com"
```

### PowerShell Module

```powershell
Install-Module -Name CIPPAPIModule
Set-CIPPAPIDetails -CIPPClientID $ClientID -CIPPClientSecret $Secret -CIPPAPIUrl $URL -TenantID $TenantID
Get-CIPPLogs  # test connectivity
```

### Limits

- **Timeout:** 10 minutes max per API action
- **Rate limits:** None enforced, but heavy API use slows the frontend
- **Auth:** Bearer token (OAuth2), no API keys

## Core API Concepts

### TenantFilter Pattern

Nearly every endpoint requires a `TenantFilter` parameter — the target tenant's **default domain** (e.g. `contoso.onmicrosoft.com`) or `AllTenants` for cross-tenant operations.

- **GET requests:** Pass as query parameter: `?TenantFilter=contoso.onmicrosoft.com`
- **POST requests:** Include in JSON body: `{"TenantFilter": "contoso.onmicrosoft.com", ...}`

### Endpoint Naming Convention

| Prefix     | HTTP Method | Purpose                        |
|------------|-------------|--------------------------------|
| `List*`    | GET         | Read/list data                 |
| `Add*`     | POST        | Create new resources           |
| `Edit*`    | POST/PATCH  | Modify existing resources      |
| `Exec*`    | POST        | Execute actions                |
| `Remove*`  | POST/DELETE | Delete resources               |

### Response Pattern

```json
{ "Results": ["Successfully created user john@contoso.com", "License assigned"] }
```

Most write operations return `{ Results: string[] }` — an array of status messages.
List operations return arrays of objects directly.

### LabelValue Pattern

Many POST bodies use the `{ "label": "Display Name", "value": "internal-id" }` shape for dropdown/select fields. The backend unwraps via `$x.value ?? $x`.

## API Base URL

All endpoints follow: `{CIPP_URL}/api/{EndpointName}`

Examples:
- `GET  /api/ListUsers?TenantFilter=contoso.onmicrosoft.com`
- `POST /api/AddUser` with JSON body
- `POST /api/ExecDisableUser` with JSON body
- `GET  /api/ListMailboxes?TenantFilter=contoso.onmicrosoft.com`

## Reference Files — When to Read Each

Load the relevant reference file based on the task at hand:

### Identity & Users
| File | Read when... |
|------|-------------|
| `references/auth-setup.md` | Setting up API access, troubleshooting auth, self-hosted API config |
| `references/identity-users.md` | User CRUD, password reset, MFA, disable/enable, offboarding, licenses, BEC remediation, sign-in logs |
| `references/identity-groups.md` | Group CRUD, templates, membership, Teams-enabling groups |
| `references/identity-devices.md` | Azure AD device management, deleted items, roles |

### Email & Exchange
| File | Read when... |
|------|-------------|
| `references/email-exchange.md` | Mailboxes, permissions, forwarding, shared mailboxes, contacts, quarantine, spam filters, transport rules, connectors, resources (rooms/equipment), mail flow, retention |

### Endpoint Management
| File | Read when... |
|------|-------------|
| `references/endpoint-intune.md` | Intune policies, compliance, app protection, device actions, scripts, reusable settings, assignment filters |
| `references/endpoint-autopilot.md` | Autopilot devices, profiles, enrollment status pages, deployment |

### Tenant Administration
| File | Read when... |
|------|-------------|
| `references/tenant-administration.md` | Tenant onboarding/offboarding, GDAP, domains, alerts, audit logs, secure score, app registrations, partner relationships |
| `references/tenant-conditional-access.md` | Conditional access policies, templates, named locations, CA testing |

### Security & Compliance
| File | Read when... |
|------|-------------|
| `references/security.md` | Security incidents, alerts, Defender status/deployment, vulnerabilities, Safe Links policies |
| `references/standards-compliance.md` | Standards, BPA, domain analyser, drift detection, templates |

### Teams, SharePoint & Other
| File | Read when... |
|------|-------------|
| `references/teams-sharepoint.md` | Teams management, SharePoint sites, voice, permissions |
| `references/extensions-integrations.md` | CIPP settings, extensions (HaloPSA, NinjaOne, Hudu, etc.), scheduler, custom roles, API clients |

## n8n / Automation Integration

CIPP's REST API works with any HTTP client. For n8n workflows:
1. Use an **HTTP Request** node with OAuth2 credentials (client_credentials grant)
2. Set the token URL, client ID, client secret, and scope as above
3. All CIPP endpoints are available — just call `{CIPP_URL}/api/{endpoint}`
4. Use the TenantFilter parameter to target specific tenants

## "Can CIPP Do X?" Decision Tree

1. Check the relevant reference file for API endpoints
2. If an API endpoint exists → automate it directly
3. If API endpoint exists but is complex → check the UI path for context
4. If no API endpoint → guide user through UI navigation: `CIPP > Section > Subsection > Action`
5. Some features are UI-only (e.g., setup wizard, some advanced settings)

## Complete Endpoint Index (by Category)

### Identity
- **Users:** ListUsers, AddUser, EditUser (PATCH), ExecDisableUser, ExecResetPass, ExecResetMFA, ExecRevokeSessions, ExecOffboardUser, ExecCreateTAP, ExecBECRemediate, ExecDismissRiskyUser, ExecPerUserMFA, ExecClrImmId, ExecRestoreDeleted, ExecPasswordNeverExpires, ExecSetUserPhoto, ExecReprocessUserLicenses, AddGuest, ListUserSigninLogs, ListUserDevices, ListUserGroups, ListUserMailboxDetails, ListUserMailboxRules, ListUserConditionalAccessPolicies, ListUserCounts, ListPerUserMFA, ListDeletedItems, ListUserSettings, ListUserPhoto
- **Groups:** ListGroups, AddGroup, EditGroup (PATCH), AddGroupTeam, AddGroupTemplate, ListGroupTemplates, RemoveGroupTemplate, ListGroupSenderAuthentication
- **Devices:** ExecDeviceDelete, ListDevices (reports)
- **Reports:** ListMFAUsers, ListSignIns, ListInactiveAccounts, ListBasicAuth, ListAzureADConnectStatus

### Email & Exchange
- **Mailboxes:** ListMailboxes, AddSharedMailbox, ExecConvertMailbox, ExecEditMailboxPermissions, ExecEmailForward, ExecSetMailboxQuota, ExecSetOoO, ExecSetMailboxRule, ExecRemoveMailboxRule, ExecHideFromGAL, ExecSetLitigationHold, ExecEnableArchive, ExecSetMailboxLocale, ExecSetCalendarProcessing, ListMailboxRules, ListMailboxPermissions, ListCalendarPermissions, ListOoO, ListRestrictedUsers
- **Contacts:** ListContacts, AddContact, EditContact, AddContactTemplates, DeployContactTemplates
- **Transport:** ListTransportRules, AddTransportRule, EditTransportRule, ListExchangeConnectors, AddExConnector, EditExConnector
- **Spam:** ListSpamfilter, AddSpamFilter, EditSpamFilter, ListMailQuarantine, ExecQuarantineManagement, AddTenantAllowBlockList
- **Resources:** ListRooms, ListEquipment, ListRoomLists, AddRoomMailbox, AddEquipmentMailbox

### Endpoint
- **Intune/MEM:** ListIntunePolicy, AddPolicy, EditPolicy, ExecAssignPolicy, ExecDeviceAction, ListCompliancePolicies, ListAppProtectionPolicies, ListIntuneScript, ListIntuneTemplates, AddIntuneTemplate, ListDefenderState, ListDefenderTVM, ExecGetRecoveryKey, ExecGetLocalAdminPassword
- **Autopilot:** ListAPDevices, AddAPDevice, ExecAssignAPDevice, ExecRenameAPDevice, ExecSyncAPDevices, RemoveAPDevice, ListAutopilotconfig, AddAutopilotConfig, AddEnrollment
- **Apps:** ListApps, AddChocoApp, AddStoreApp, AddOfficeApp, AddMSPApp, ExecAssignApp

### Tenant
- **Admin:** ListTenants, ListTenantDetails, AddTenant, EditTenant, ExecOnboardTenant, ExecOffboardTenant, ListDomains, AddDomain, ListPartnerRelationships, ListAppConsentRequests, ExecUpdateSecureScore
- **Alerts:** AddAlert, ListAlertsQueue, ListAuditLogs, ExecAuditLogSearch, ListWebhookAlert
- **GDAP:** ListGDAPRoles, ExecAddGDAPRole, ExecDeleteGDAPRelationship, ListGDAPInvite, ListGDAPAccessAssignments
- **Conditional:** ListConditionalAccessPolicies, AddCAPolicy, EditCAPolicy, ExecCACheck, ListCAtemplates, AddCATemplate, ListNamedLocations, AddNamedLocation

### Security
- **Incidents:** ExecAlertsList, ExecIncidentsList, ExecSetSecurityAlert, ExecSetSecurityIncident
- **Defender:** ListDefenderState, AddDefenderDeployment, ListDefenderTVM
- **Safe Links:** ListSafeLinksPolicy, ExecNewSafeLinksPolicy, EditSafeLinksPolicy, ListSafeLinksPolicyTemplates

### Standards
- ListStandards, AddStandardsDeploy, ListBPA, ExecBPA, ListDomainAnalyser, ExecDomainAnalyser, ListTenantDrift, ListTenantAlignment

### Teams & SharePoint
- ListTeams, AddTeam, ListSites, AddSite, ListSharepointSettings, ExecSharePointPerms, ListTeamsVoice, ExecTeamsVoicePhoneNumberAssignment

### CIPP Settings
- ListLogs, ListExtensionsConfig, ExecExtensionsConfig, AddScheduledItem, ListScheduledItems, ExecAccessChecks, ExecNotificationConfig, ExecRunBackup, ExecRestoreBackup, GetVersion
