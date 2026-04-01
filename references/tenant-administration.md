# Tenant Administration Reference

## Tenant Management

### List Tenants [API]

```
POST /api/ListTenants
```

Returns all managed tenants. No TenantFilter needed — returns the full list.

```bash
curl -s -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  "${CIPP_URL}/api/ListTenants"
```

### Tenant Details [API]

```
GET /api/ListTenantDetails?TenantFilter={tenant}
```

Returns detailed info: displayName, defaultDomain, tenantId, verifiedDomains, etc.

**UI Path:** CIPP > Tenant > Administration > Tenants

### Add/Onboard Tenant [API + UI]

```
POST /api/AddTenant
{
  "TenantFilter": "newtenant.onmicrosoft.com"
}

POST /api/ExecOnboardTenant
{
  "TenantFilter": "newtenant.onmicrosoft.com",
  "GDAPRoles": [...],
  "Standards": [...]
}
```

**UI Path:** CIPP > Tenant > GDAP Management > Onboarding

### Edit Tenant [API + UI]

```
POST /api/EditTenant
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Updated Name",
  "DefaultDomain": "contoso.com"
}
```

**UI Path:** CIPP > Tenant > Manage > Edit Tenant

### Offboard Tenant [API + UI]

```
PATCH /api/ExecOffboardTenant
{
  "TenantFilter": "oldtenant.onmicrosoft.com"
}
```

**UI Path:** CIPP > Tenant > GDAP Management > Offboarding

### Remove Tenant (Exclude) [API]

```
POST /api/ExecRemoveTenant
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

### Tenant Groups [UI primarily]

**UI Path:** CIPP > Tenant > Administration > Tenants > Groups
- Create logical groups of tenants for bulk operations
- Dynamic rules for automatic tenant grouping

```
DELETE /api/ExecTenantGroupManagement  (delete group)
POST   /api/ExecTenantGroupManagement  (create/edit)
POST   /api/ListTenantGroups
POST   /api/ExecTenantGroupDynamicRules  (run dynamic rules)
```

### Remove Tenant Capabilities Cache [API]

```
GET /api/RemoveTenantCapabilitiesCache?TenantFilter={tenant}
```

## Domains

### List Domains [API]

```
GET /api/ListDomains?TenantFilter={tenant}
```

**UI Path:** CIPP > Tenant > Administration > Domains

### Add Domain [API]

```
POST /api/AddDomain
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DomainName": "newdomain.com"
}
```

### Domain Actions [API]

```
DELETE /api/ExecDomainAction?TenantFilter={tenant}&DomainName={domain}&Action={action}
```

## Alerts & Audit Logs

### Add Alert Rule [API + UI]

```
POST /api/AddAlert
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "command": {"label": "New User Created", "value": "Add user"},
  "recurrence": {"label": "Every 15 minutes", "value": "15m"},
  "actions": ["email", "webhook"],
  "conditions": "...",
  "AlertComment": "Alert on new user creation"
}
```

**UI Path:** CIPP > Tenant > Administration > Alert Configuration > Add Alert

### List Alerts Queue [API]

```
GET /api/ListAlertsQueue
```

### Remove Queued Alert [API]

```
POST /api/RemoveQueuedAlert
{
  "RowKey": "alert-row-key"
}
```

### Audit Logs [API + UI]

```
# List audit logs
GET /api/ListAuditLogs?TenantFilter={tenant}&StartDate=2024-01-01&EndDate=2024-01-31

# Search audit logs
POST /api/ExecAuditLogSearch
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "StartTime": "2024-01-01",
  "EndTime": "2024-01-31",
  "Action": "Add user"
}

# List saved searches
GET /api/ListAuditLogSearches?TenantFilter={tenant}
```

**UI Path:** CIPP > Tenant > Administration > Audit Logs > [View | Searches | Directory Audits]

### Webhook Alerts [API]

```
GET  /api/ListWebhookAlert
POST /api/PublicWebhooks  (incoming webhook handler)
```

## Secure Score [API + UI]

```
POST /api/ExecUpdateSecureScore
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

**UI Path:** CIPP > Tenant > Administration > Secure Score > [Table View]

## Applications & App Registrations [UI + API]

### App Consent Requests [API]

```
GET /api/ListAppConsentRequests?TenantFilter={tenant}
```

### Enterprise Apps / App Approval [API]

```
GET  /api/ExecAppApproval?TenantFilter={tenant}
POST /api/ExecAddMultiTenantApp
PATCH /api/ExecApplication

# Templates
GET    /api/ListAppApprovalTemplates
POST   /api/ExecCreateAppTemplate
DELETE /api/ExecAppApprovalTemplate
DELETE /api/ExecAppPermissionTemplate
```

**UI Path:** CIPP > Tenant > Administration > [Applications | App Registrations | App Consent Requests | Permission Sets | Application Templates]

### Service Principals [API]

```
GET /api/ExecServicePrincipals?TenantFilter={tenant}

GET /api/ExecAddSPN?TenantFilter={tenant}&AppID={appId}
```

## Authentication Methods [API + UI]

```
POST /api/SetAuthMethod
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Method": "microsoftAuthenticator",
  "State": "enabled"
}
```

**UI Path:** CIPP > Tenant > Administration > Authentication Methods

## Partner Relationships [API]

```
GET /api/ListPartnerRelationships?TenantFilter={tenant}
```

**UI Path:** CIPP > Tenant > Administration > Partner Relationships

## GDAP (Granular Delegated Admin Privileges)

### Relationships [API + UI]

```
# List GDAP roles
GET /api/ListGDAPRoles

# Add role
POST /api/ExecAddGDAPRole
{
  "RoleName": "Security Reader",
  "RoleDefinitionId": "role-guid"
}

# Delete relationship
POST /api/ExecDeleteGDAPRelationship
{
  "RelationshipId": "relationship-guid"
}

# Auto-extend GDAP
POST /api/ExecAutoExtendGDAP
{
  "RelationshipId": "relationship-guid"
}
```

**UI Path:** CIPP > Tenant > GDAP Management > Relationships

### Access Assignments [API]

```
GET   /api/ListGDAPAccessAssignments?TenantFilter={tenant}
PATCH /api/ExecGDAPAccessAssignment
```

### Role Mappings [API + UI]

```
POST /api/ExecDeleteGDAPRoleMapping
DELETE /api/ExecGDAPRoleTemplate
```

**UI Path:** CIPP > Tenant > GDAP Management > Role Mappings / Role Templates

### Invites [API + UI]

```
GET    /api/ListGDAPInvite
DELETE /api/ExecGDAPInvite
GET    /api/ExecGDAPInviteApproved
```

**UI Path:** CIPP > Tenant > GDAP Management > Invites > New Invite

### Remove GA Role [API]

```
POST /api/ExecGDAPRemoveGArole
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

## Tenant Onboarding Status [API]

```
GET /api/ListTenantOnboarding
```

## Reports [API]

```
GET /api/ListLicenses?TenantFilter={tenant}       # Licence report
GET /api/ListOAuthApps?TenantFilter={tenant}       # Consented applications
GET /api/ListServiceHealth?TenantFilter={tenant}   # Service health

# CSP Licenses
GET  /api/ExecCSPLicense?TenantFilter={tenant}
POST /api/ExecCSPLicense
GET  /api/ListCSPLicenses?TenantFilter={tenant}
GET  /api/ListCSPsku?TenantFilter={tenant}
```

**UI Path:** CIPP > Tenant > Reports > [Licence Report | Consented Applications | Service Health]

## External Tenant Info [API]

```
GET /api/ListExternalTenantInfo?TenantFilter={domain}
```

Looks up public info for any M365 tenant domain.

## Universal Search [API]

```
GET /api/ExecUniversalSearch?SearchString={query}
GET /api/ExecUniversalSearchV2?SearchString={query}
```

Searches across all tenants for users, devices, groups.

## Breach Search [API]

```
POST /api/ExecBreachSearch
{
  "Domain": "contoso.com"
}

GET /api/ListBreachesAccount?TenantFilter={tenant}&UserID={userId}
GET /api/ListBreachesTenant?TenantFilter={tenant}
```

## Organisation Info [API]

```
GET /api/ListOrg?TenantFilter={tenant}
```

## Configuration Backup [API + UI]

```
GET  /api/ExecRunBackup
POST /api/ExecRestoreBackup
GET  /api/ExecListBackup
POST /api/ExecSetCIPPAutoBackup
POST /api/ExecBackupRetentionConfig
```

**UI Path:** CIPP > Tenant > Manage > Configuration Backup
