# Extensions, Integrations & CIPP Settings Reference

## CIPP Settings

### Application Settings [UI + API]

**UI Path:** CIPP > Settings > Application Settings

#### Permissions
```
GET  /api/ExecAPIPermissionList
POST /api/ExecAccessChecks
POST /api/ExecCPVPermissions
```

**UI Path:** CIPP > Settings > Permissions

#### Tenants Management
```
POST /api/ExecExcludeTenant
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ExcludeFromList": true
}
```

**UI Path:** CIPP > Settings > Tenants

#### Backend
```
GET /api/ExecBackendURLs
```

**UI Path:** CIPP > Settings > Backend

#### Notifications
```
POST /api/ExecNotificationConfig
{
  "Email": "admin@company.com",
  "Webhook": "https://webhook.site/...",
  "OnePerTenant": false,
  "SendToIntegration": false
}
```

**UI Path:** CIPP > Settings > Notifications

#### Licenses (Exclude from reports)
```
POST /api/ExecExcludeLicenses
{
  "ExcludedLicenses": ["FLOW_FREE", "TEAMS_EXPLORATORY"]
}

GET /api/ListExcludedLicenses
```

**UI Path:** CIPP > Settings > Licenses

#### Backup
```
GET  /api/ExecRunBackup
POST /api/ExecRestoreBackup
POST /api/ExecSetCIPPAutoBackup
POST /api/ExecBackupRetentionConfig
```

**UI Path:** CIPP > Settings > CIPP Backup

#### Features (Feature Flags)
```
GET  /api/ListFeatureFlags
POST /api/ExecFeatureFlag
{
  "Feature": "featureName",
  "Enabled": true
}
```

**UI Path:** CIPP > Settings > Features

#### SIEM
**UI Path:** CIPP > Settings > SIEM

#### Password Configuration
```
POST /api/ExecPasswordConfig
{
  "Length": 16,
  "IncludeSpecialCharacters": true,
  "IncludeNumbers": true
}
```

**UI Path:** CIPP > Settings > Password Configuration

#### DNS Configuration
```
POST /api/ExecDnsConfig
```

#### Branding
```
POST /api/ExecBrandingSettings
```

#### Log Retention
```
POST /api/ExecLogRetentionConfig
```

## Logbook [API + UI]

```
GET /api/ListLogs
```

Returns CIPP activity logs — all actions taken through the platform.

**UI Path:** CIPP > Logbook

## Version [API]

```
GET /api/GetVersion
```

Returns current CIPP version information.

## Integrations

### CIPP-API (API Clients) [UI + API]

```
DELETE /api/ExecApiClient  (remove API client)
```

**UI Path:** CIPP > Settings > Integrations > CIPP-API
- Create/manage API clients for external access
- Copy Application ID, Secret, Scope
- Assign roles/permissions

### Extension Configuration [API]

```
GET  /api/ListExtensionsConfig
POST /api/ExecExtensionsConfig
{
  "Extension": "HaloPSA",
  "Config": {
    "ApiUrl": "https://halo.example.com",
    "ClientId": "...",
    "ClientSecret": "...",
    "Tenant": "..."
  }
}
```

### Extension Mapping [API]

```
GET /api/ExecExtensionMapping?Extension={name}&TenantFilter={tenant}
```

Maps CIPP tenants to extension entities (e.g., HaloPSA clients, NinjaOne organisations).

### Extension Sync [API]

```
GET /api/ExecExtensionSync?Extension={name}
GET /api/ListExtensionSync
```

### Extension Test [API]

```
GET /api/ExecExtensionTest?Extension={name}
```

### Supported Extensions

| Extension | Type | Purpose |
|-----------|------|---------|
| **HaloPSA** | PSA | Ticketing, client sync |
| **NinjaOne** | RMM | Device sync, documentation |
| **Hudu** | Documentation | Password/doc sync |
| **Sherweb** | Licensing | CSP license management |
| **Cloudflare** | DNS | Domain management |
| **GitHub** | DevOps | Community repos, actions |
| **Password Pusher** | Security | Secure password delivery |
| **Have I Been Pwned** | Security | Breach monitoring |
| **Gradient** | MSP | Compliance and security |

**UI Path:** CIPP > Settings > Integrations > [Extension Name]

### Integration Sync [UI]

**UI Path:** CIPP > Settings > Integrations > Integration Sync
- Manually trigger sync between CIPP and extensions
- View sync status and last sync time

## Scheduler [API + UI]

### Add Scheduled Item [API]

```
POST /api/AddScheduledItem
{
  "Name": "Weekly BPA Run",
  "Command": {"label": "BPA", "value": "ExecBPA"},
  "Parameters": {"TenantFilter": "AllTenants"},
  "Schedule": "Weekly",
  "StartTime": "2024-01-01T09:00:00Z"
}
```

### List Scheduled Items [API]

```
POST /api/ListScheduledItems
POST /api/ListScheduledItemDetails
```

### Remove Scheduled Item [API]

```
POST /api/RemoveScheduledItem
{
  "ID": "scheduled-item-guid"
}
```

**UI Path:** CIPP > Scheduler

## Advanced Settings (Super Admin)

**UI Path:** CIPP > Advanced (requires super admin role)

### Custom Roles [API + UI]

```
GET    /api/ListCustomRole
POST   /api/ExecCustomRole  (create/edit — via UI fields)
DELETE /api/ExecCustomRole
```

**UI Path:** CIPP > Advanced > Super Admin > CIPP Roles

### SAM App Roles [API]

```
POST /api/ExecSAMRoles
```

**UI Path:** CIPP > Advanced > Super Admin > SAM App Roles

### SAM App Permissions [API]

```
POST /api/ExecSAMAppPermissions
GET  /api/ExecSAMAppPermissionsUpdate  (update permissions)
```

**UI Path:** CIPP > Advanced > Super Admin > SAM App Permissions

### Tenant Mode [UI]

**UI Path:** CIPP > Advanced > Super Admin > Tenant Mode
- Switch between MSP mode (multi-tenant) and single-tenant mode

### Partner Mode [API]

```
POST /api/ExecPartnerMode
```

### Function Offloading [API + UI]

```
POST /api/ExecOffloadFunctions
```

Deploy multiple Azure Function Apps for increased performance.

**UI Path:** CIPP > Advanced > Super Admin > Function Offloading

### Time Settings [API]

```
POST /api/ExecTimeSettings
```

**UI Path:** CIPP > Advanced > Super Admin > Time Settings

### JIT Admin Settings [API]

```
POST /api/ExecJITAdminSettings
```

### Exchange Role Repair [API]

```
POST /api/ExecExchangeRoleRepair
```

**UI Path:** CIPP > Advanced > Exchange Cmdlets

### Trusted IPs [API]

```
POST /api/ExecAddTrustedIP
{
  "IP": "203.0.113.0/24",
  "Description": "Office network"
}

GET /api/ListIPWhitelist
```

### Webhook Subscriptions [API]

```
DELETE /api/ExecWebhookSubscriptions
```

### Maintenance Scripts [API]

```
GET /api/ExecMaintenanceScripts
```

### Durable Functions [API]

```
GET /api/ExecDurableFunctions
```

### Diagnostics [UI]

**UI Path:** CIPP > Advanced > Diagnostics

### Table Maintenance [UI]

**UI Path:** CIPP > Advanced > Table Maintenance

### Timers [UI]

**UI Path:** CIPP > Advanced > Timers

## Custom Data [API + UI]

### Custom Variables [API]

```
GET /api/ListCustomVariables
POST /api/ExecCustomData
```

### Custom Data Mappings [API]

```
GET /api/ListCustomDataMappings
```

**UI Path:** CIPP > Custom Data > [Directory Extensions | Schema Extensions | Mappings]

## CIPP Replace Map [API]

```
DELETE /api/ExecCippReplacemap
```

## Tenant Default Groups [API]

```
GET /api/ExecCreateDefaultTenantGroups
```

## Graph Explorer (Tools) [API]

```
GET    /api/ListGraphExplorerPresets
DELETE /api/ExecGraphExplorerPreset
GET    /api/ListGraphRequest?TenantFilter={tenant}&Endpoint={graphEndpoint}
POST   /api/ListGraphBulkRequest
POST   /api/ListDirectoryObjects
```

### BPA Template (Tools) [API]

```
POST /api/AddBPATemplate
```

## Setup Endpoints [API]

These are typically used during initial CIPP setup:

```
POST /api/ExecCombinedSetup
POST /api/ExecCreateSAMApp
GET  /api/ExecDeviceCodeLogon
POST /api/ExecSAMSetup
POST /api/ExecTokenExchange
POST /api/ExecUpdateRefreshToken
POST /api/ExecAddTenant  (setup context)
```

## GitHub Integration [API]

```
GET    /api/ListCommunityRepos      # List community repos
POST   /api/InvokeGitHubAction      # Trigger GitHub Action
DELETE /api/ExecCommunityRepo       # Remove repo
GET    /api/ListGitHubReleaseNotes  # Release notes
```

**UI Path:** CIPP > Settings > Integrations > GitHub

## App Status & Function Stats [API]

```
GET /api/ListAppStatus
GET /api/ListFunctionStats
GET /api/ListFunctionParameters
```

## Send Org Message [API]

```
GET /api/ExecSendOrgMessage?TenantFilter={tenant}
```

## Admin Portal Licenses [API]

```
GET /api/ListAdminPortalLicenses
```

## Ping (Health Check) [API]

```
GET /api/PublicPing
```

No auth required. Returns basic health status.

## CIPP Alerts [API]

```
GET /api/GetCippAlerts
```

Returns CIPP platform alerts (not tenant security alerts).
