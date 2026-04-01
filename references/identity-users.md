# Identity — Users Reference

## List Users [API]

```
GET /api/ListUsers?TenantFilter={tenant}
```

Optional query params: `UserID` (specific user), `graphFilter` (OData filter)

Returns array of user objects with properties like displayName, userPrincipalName, id, accountEnabled, assignedLicenses, etc.

```bash
# List all users
curl -s -H "Authorization: Bearer $TOKEN" \
  "${CIPP_URL}/api/ListUsers?TenantFilter=contoso.onmicrosoft.com"

# Get specific user
curl -s -H "Authorization: Bearer $TOKEN" \
  "${CIPP_URL}/api/ListUsers?TenantFilter=contoso.onmicrosoft.com&UserID=john@contoso.com"
```

**UI Path:** CIPP > Identity > Administration > Users

## Create User [API + UI]

```
POST /api/AddUser
Content-Type: application/json

{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "John Smith",
  "UserName": "john",
  "Domain": "contoso.onmicrosoft.com",
  "FirstName": "John",
  "LastName": "Smith",
  "Password": "TempP@ss123!",
  "MustChangePass": true,
  "usageLocation": "GB",
  "Licenses": [{"label": "Microsoft 365 Business Premium", "value": "SPB"}],
  "CopyFrom": ""
}
```

**UI Path:** CIPP > Identity > Administration > Users > Add User button (top right)

### Bulk Create Users (CSV) [API]

```
POST /api/AddUser  (with CSV import format)
```

Upload CSV with columns matching user fields. UI provides a template download.

**UI Path:** CIPP > Identity > Administration > Users > Bulk Create

## Edit User [API]

```
PATCH /api/EditUser
Content-Type: application/json

{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid-here",
  "DisplayName": "Updated Name",
  "JobTitle": "Manager",
  "Department": "IT",
  "City": "London",
  "Country": "GB",
  "CompanyName": "Contoso Ltd"
}
```

**UI Path:** CIPP > Identity > Administration > Users > click user > Edit User

### Edit User Properties Wizard [UI]

**UI Path:** CIPP > Identity > Administration > Users > click user > Edit Properties
- Allows editing all directory attributes
- Supports custom attributes and extension properties

## Edit User Aliases [API]

```
POST /api/EditUserAliases
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "john@contoso.com",
  "AddedAliases": "john.smith@contoso.com",
  "RemoveAliases": "old@contoso.com"
}
```

## Disable User [API]

```
POST /api/ExecDisableUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Reset Password [API]

```
POST /api/ExecResetPass
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid",
  "MustChange": true
}
```

Returns the new temporary password in Results.

## Reset MFA [API]

```
POST /api/ExecResetMFA
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Revoke Sessions [API]

```
POST /api/ExecRevokeSessions
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Create Temporary Access Pass (TAP) [API]

```
POST /api/ExecCreateTAP
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

Returns TAP code in Results.

## Per-User MFA [API]

```
# List per-user MFA status
GET /api/ListPerUserMFA?TenantFilter={tenant}

# Set per-user MFA
POST /api/ExecPerUserMFA
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "userId": "user-guid",
  "State": "Enabled"  // Enabled | Disabled | Enforced
}
```

## Password Never Expires [API]

```
POST /api/ExecPasswordNeverExpires
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid",
  "PasswordNeverExpires": true
}
```

## Clear Immutable ID [API]

```
POST /api/ExecClrImmId
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Send Push Notification [API]

```
POST /api/ExecSendPush
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserEmail": "user@contoso.com"
}
```

## Set User Photo [API]

```
POST /api/ExecSetUserPhoto
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "input": "base64-encoded-image-data"
}
```

## Get User Photo [API]

```
GET /api/ListUserPhoto?TenantFilter={tenant}&UserID={userPrincipalName}
```

## OneDrive Provisioning [API]

```
POST /api/ExecOnedriveProvision
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

## OneDrive Shortcut [API]

```
POST /api/ExecOneDriveShortCut
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "siteUrl": "https://contoso.sharepoint.com/sites/shared",
  "listName": "Documents",
  "folderName": "Shared Docs"
}
```

## Reprocess Licenses [API]

```
POST /api/ExecReprocessUserLicenses
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Offboard User [API + UI]

```
POST /api/ExecOffboardUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid",
  "RevokeSessions": true,
  "ResetPassword": true,
  "RemoveMFA": true,
  "DisableUser": true,
  "ConvertToShared": false,
  "HideFromGAL": true,
  "RemoveGroups": true,
  "RemoveLicenses": true,
  "SetForwarding": "",
  "SetOoO": "This user has left the company.",
  "RemoveRules": true,
  "RemoveMobileDevices": true,
  "RemovePermissions": true
}
```

**UI Path:** CIPP > Identity > Administration > Offboarding Wizard
- Provides a step-by-step wizard with checkboxes for each offboarding action
- Can queue offboarding jobs for later

### Offboarding Defaults [API]

```
POST /api/EditTenantOffboardingDefaults
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "RevokeSessions": true,
  "ResetPassword": true,
  ...
}
```

## BEC (Business Email Compromise) [API + UI]

```
# Check for compromise indicators
GET /api/ExecBECCheck?TenantFilter={tenant}&UserID={userId}

# Remediate compromise
POST /api/ExecBECRemediate
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid",
  "ResetPassword": true,
  "RevokeSessions": true,
  "RemoveRules": true,
  "DisableForwarding": true
}
```

**UI Path:** CIPP > Identity > Administration > Users > click user > Compromise Remediation (BEC)

## Dismiss Risky User [API]

```
POST /api/ExecDismissRiskyUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid"
}
```

**UI Path:** CIPP > Identity > Administration > Risky Users

## Restore Deleted User [API]

```
POST /api/ExecRestoreDeleted
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Delete User [API]

```
POST /api/RemoveUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user-guid"
}
```

## Permanently Delete Deleted Object [API]

```
POST /api/RemoveDeletedObject
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "object-guid",
  "Type": "User"
}
```

## Add Guest User [API]

```
POST /api/AddGuest
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Email": "guest@external.com",
  "DisplayName": "External Guest",
  "RedirectUrl": "https://myapps.microsoft.com",
  "SendInvite": true
}
```

## User Lookup Queries [API]

```bash
# Sign-in logs
GET /api/ListUserSigninLogs?TenantFilter={tenant}&UserID={userId}

# User's devices
GET /api/ListUserDevices?TenantFilter={tenant}&UserID={userId}

# User's groups
GET /api/ListUserGroups?TenantFilter={tenant}&UserID={userId}

# User's mailbox details
GET /api/ListUserMailboxDetails?TenantFilter={tenant}&UserID={userId}

# User's mailbox rules
GET /api/ListUserMailboxRules?TenantFilter={tenant}&UserID={userId}

# User's CA policies
GET /api/ListUserConditionalAccessPolicies?TenantFilter={tenant}&UserID={userId}

# User settings
GET /api/ListUserSettings?TenantFilter={tenant}&UserID={userId}

# Trusted/blocked senders
GET /api/ListUserTrustedBlockedSenders?TenantFilter={tenant}&UserID={userId}

# User counts (per tenant)
GET /api/ListUserCounts?TenantFilter={tenant}
```

## Bulk License Operations [API]

```
GET /api/ExecBulkLicense?TenantFilter={tenant}
```

## JIT (Just-In-Time) Admin [API + UI]

```
# Create JIT admin session
POST /api/ExecJITAdmin
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "admin@contoso.com",
  "Roles": [{"label": "Global Admin", "value": "role-guid"}],
  "Expiration": "2024-01-01T12:00:00Z"
}

# List active JIT sessions
GET /api/ListJITAdmin?TenantFilter={tenant}

# Add JIT template
POST /api/AddJITAdminTemplate
{ "TemplateName": "Emergency Admin", "Roles": [...], "DefaultExpiration": "4h" }

# List JIT templates
POST /api/ListJITAdminTemplates
```

**UI Path:** CIPP > Identity > Administration > JIT Admin

## User Defaults / Templates [API + UI]

```
# Add user defaults (template for new users)
POST /api/AddUserDefaults
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "TemplateName": "Standard User",
  "usageLocation": "GB",
  "Licenses": [{"label":"M365 BP","value":"SPB"}],
  "DefaultDomain": "contoso.com"
}

# List user default templates
POST /api/ListNewUserDefaults
```

**UI Path:** CIPP > Tenant > Manage > User Defaults

## Reports [API]

```bash
# MFA status report
GET /api/ListMFAUsers?TenantFilter={tenant}

# Sign-in report
GET /api/ListSignIns?TenantFilter={tenant}

# Inactive accounts
GET /api/ListInactiveAccounts?TenantFilter={tenant}

# Basic auth usage
GET /api/ListBasicAuth?TenantFilter={tenant}

# Azure AD Connect status
GET /api/ListAzureADConnectStatus?TenantFilter={tenant}

# Deleted items
GET /api/ListDeletedItems?TenantFilter={tenant}
```

**UI Path:** CIPP > Identity > Reports > [MFA Report | Inactive Users | Sign-in Report | AAD Connect Report | Risk Detections]

## Remove Trusted/Blocked Sender [API]

```
POST /api/RemoveTrustedBlockedSender
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid",
  "SenderAddress": "spam@example.com",
  "ListType": "BlockedSenders"
}
```

## Set Cloud Managed [API]

```
POST /api/ExecSetCloudManaged
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user-guid"
}
```
