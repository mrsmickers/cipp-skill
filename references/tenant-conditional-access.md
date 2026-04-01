# Tenant — Conditional Access Reference

## CA Policies

### List Policies [API]

```
GET /api/ListConditionalAccessPolicies?TenantFilter={tenant}
```

Returns all CA policies with state, conditions, grant controls, session controls.

**UI Path:** CIPP > Tenant > Conditional Access > CA Policies

### Add CA Policy [API + UI]

```
POST /api/AddCAPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Block Legacy Auth",
  "State": "enabled",
  "Conditions": {
    "ClientAppTypes": ["exchangeActiveSync", "other"],
    "Users": {
      "IncludeUsers": ["All"]
    }
  },
  "GrantControls": {
    "BuiltInControls": ["block"],
    "Operator": "OR"
  }
}
```

**UI Path:** CIPP > Tenant > Conditional Access > CA Policies > Add Policy (via templates)

### Edit CA Policy [API]

```
POST /api/EditCAPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid",
  "State": "disabled",
  "DisplayName": "Updated Policy Name"
}
```

### Remove CA Policy [API]

```
POST /api/RemoveCAPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid"
}
```

### CA Policy Changes [API]

```
GET /api/ListConditionalAccessPolicyChanges?TenantFilter={tenant}
```

Lists recent changes/modifications to CA policies.

## CA Check (What-If) [API + UI]

Test conditional access policies against a user without enforcing:

```
POST /api/ExecCACheck
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "IncludeApplications": "All",
  "ClientAppType": "browser"
}
```

Returns which policies would apply, allow, or block.

**UI Path:** CIPP > Identity > Administration > Users > click user > Conditional Access

## CA Exclusion Management [API]

```
POST /api/ExecCAExclusion
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid",
  "ExcludeUsers": ["user-guid"],
  "ExcludeGroups": ["group-guid"]
}
```

### Service Account Exclusions [API]

```
POST /api/ExecCAServiceExclusion
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid",
  "ServiceAccount": "service@contoso.com"
}
```

## CA Templates

### List Templates [API]

```
GET /api/ListCAtemplates
```

Returns saved CA policy templates.

**UI Path:** CIPP > Tenant > Conditional Access > CA Templates

### Add Template [API + UI]

```
POST /api/AddCATemplate
{
  "TemplateName": "Block Legacy Auth Template",
  "DisplayName": "Block Legacy Auth",
  "State": "enabled",
  "Conditions": {...},
  "GrantControls": {...}
}
```

### Remove Template [API]

```
POST /api/RemoveCATemplate
{
  "ID": "template-guid"
}
```

### Edit Template [UI]

**UI Path:** CIPP > Tenant > Conditional Access > CA Templates > click template > Edit

## Named Locations

### List Named Locations [API]

```
GET /api/ListNamedLocations?TenantFilter={tenant}
```

**UI Path:** CIPP > Tenant > Conditional Access > Named Locations

### Add Named Location [API + UI]

```
POST /api/AddNamedLocation
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "UK Office IPs",
  "Type": "ipNamedLocation",
  "IsTrusted": true,
  "IPRanges": ["203.0.113.0/24", "198.51.100.0/24"]
}
```

For country-based locations:
```
POST /api/AddNamedLocation
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Allowed Countries",
  "Type": "countryNamedLocation",
  "CountriesAndRegions": ["GB", "US", "DE"],
  "IncludeUnknownCountriesAndRegions": false
}
```

### Edit Named Location [API]

```
POST /api/ExecNamedLocation
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "location-guid",
  "DisplayName": "Updated Location",
  "IsTrusted": true,
  "IPRanges": ["203.0.113.0/24"]
}
```

**UI Path:** CIPP > Tenant > Conditional Access > Named Locations > Add Named Location

## Vacation Mode (CA Bypass) [UI primarily]

Temporarily exclude users from CA policies for vacation/travel:

**UI Path:** CIPP > Identity > Administration > CA Vacation Mode
- Select user → Set start/end dates → Choose policies to bypass
- Automatically re-enables policies after the end date

```
# Add vacation schedule (via UI or scheduled task)
```

**UI Path:** CIPP > Identity > Administration > CA Vacation Mode > Add Vacation Schedule

## User's CA Policies [API]

View which CA policies apply to a specific user:

```
GET /api/ListUserConditionalAccessPolicies?TenantFilter={tenant}&UserID={userId}
```

## Common CA Policy Patterns

### Block Legacy Authentication
- Conditions: Client apps = Exchange ActiveSync, Other clients
- Grant: Block access
- Users: All users (exclude break-glass accounts)

### Require MFA for All Users
- Conditions: All cloud apps
- Grant: Require multi-factor authentication
- Users: All users (exclude service accounts)

### Block Access from Untrusted Locations
- Conditions: All cloud apps, Exclude trusted named locations
- Grant: Block access

### Require Compliant Device
- Conditions: All cloud apps
- Grant: Require device to be marked as compliant
- Users: All users
