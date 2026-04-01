# Security Reference

## Incidents & Alerts

### List Security Incidents [API]

```
GET /api/ExecIncidentsList?TenantFilter={tenant}
```

Returns Microsoft 365 security incidents with severity, status, investigation state.

**UI Path:** CIPP > Security & Compliance > Incidents & Alerts > Incidents

### List Security Alerts [API]

```
GET /api/ExecAlertsList?TenantFilter={tenant}
```

**UI Path:** CIPP > Security & Compliance > Incidents & Alerts > Alerts

### List MDO (Microsoft Defender for Office) Alerts [API]

```
GET /api/ExecMdoAlertsList?TenantFilter={tenant}
```

**UI Path:** CIPP > Security & Compliance > Incidents & Alerts > MDO Alerts

### Set Security Alert Status [API]

```
POST /api/ExecSetSecurityAlert
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "AlertID": "alert-guid",
  "Status": "resolved",
  "Comment": "False positive - confirmed legitimate"
}
```

### Set Security Incident Status [API]

```
POST /api/ExecSetSecurityIncident
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "IncidentID": "incident-guid",
  "Status": "resolved",
  "Classification": "falsePositive"
}
```

### Set MDO Alert [API]

```
POST /api/ExecSetMdoAlert
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "AlertID": "alert-guid",
  "Status": "resolved"
}
```

### Check Alerts (Extension Alerts) [API]

```
GET /api/ListCheckExtAlerts
```

**UI Path:** CIPP > Security & Compliance > Incidents & Alerts > Check Alerts

## Defender

### Defender Status [API]

```
GET /api/ListDefenderState?TenantFilter={tenant}
```

Returns Windows Defender deployment status across devices.

**UI Path:** CIPP > Security & Compliance > Defender > Defender Status

### Deploy Defender [API + UI]

```
POST /api/AddDefenderDeployment
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyName": "Defender Deployment",
  "AssignTo": "AllDevices"
}
```

**UI Path:** CIPP > Security & Compliance > Defender > Defender Deployment

### Threat & Vulnerability Management [API]

```
GET /api/ListDefenderTVM?TenantFilter={tenant}
```

Returns vulnerability assessments, software inventory, and recommendations.

**UI Path:** CIPP > Security & Compliance > Defender > Vulnerabilities

## Safe Links Policies

### List Safe Links Policies [API]

```
GET /api/ListSafeLinksPolicy?TenantFilter={tenant}
```

**UI Path:** CIPP > Security & Compliance > Safe Links Policies

### Get Policy Details [API]

```
POST /api/ListSafeLinksPolicyDetails
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid"
}
```

### Create Safe Links Policy [API + UI]

```
POST /api/ExecNewSafeLinksPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Name": "Standard Safe Links",
  "IsEnabled": true,
  "DoNotAllowClickThrough": true,
  "DoNotTrackUserClicks": false,
  "EnableForInternalSenders": true,
  "ScanUrls": true,
  "DeliverMessageAfterScan": true,
  "DisableUrlRewrite": false,
  "EnableOrganizationBranding": false
}
```

**UI Path:** CIPP > Security & Compliance > Safe Links Policies > Add Safe Links Policy

### Edit Safe Links Policy [API]

```
POST /api/EditSafeLinksPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid",
  "Name": "Updated Safe Links",
  "IsEnabled": true,
  ...
}
```

### Delete Safe Links Policy [API]

```
POST /api/ExecDeleteSafeLinksPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid"
}
```

## Safe Links Templates [API + UI]

```
# List templates
GET /api/ListSafeLinksPolicyTemplates

# Get template details
POST /api/ListSafeLinksPolicyTemplateDetails

# Create template from existing policy
POST /api/CreateSafeLinksPolicyTemplate
{
  "TemplateName": "Standard Safe Links Template",
  "PolicyID": "policy-guid",
  "TenantFilter": "contoso.onmicrosoft.com"
}

# Deploy template to tenant
POST /api/AddSafeLinksPolicyFromTemplate
{
  "TenantFilter": "target-tenant.onmicrosoft.com",
  "TemplateID": "template-guid"
}

# Edit template
POST /api/EditSafeLinksPolicyTemplate

# Remove template
POST /api/RemoveSafeLinksPolicyTemplate
```

**UI Path:** CIPP > Security & Compliance > Safe Links Templates > [Create | Deploy | Edit]

## Device Compliance Report [API]

```
GET /api/ListAllTenantDeviceCompliance
```

Cross-tenant device compliance overview.

**UI Path:** CIPP > Security & Compliance > Reports > Device Compliance

## BEC (Business Email Compromise) — see identity-users.md

For BEC check and remediation, refer to `identity-users.md` > BEC section.

## Phishing Check [API]

```
POST /api/PublicPhishingCheck
{
  "URL": "https://suspicious-link.example.com"
}
```

Public endpoint (no auth required) for checking URLs against known phishing databases.

## Known IP Database [API]

```
GET /api/ListKnownIPDb
```

Lists known/trusted IP addresses used for security analysis.

## GeoIP Lookup [API]

```
POST /api/ExecGeoIPLookup
{
  "IP": "203.0.113.1"
}
```

Returns geolocation data for an IP address.

## Risky Users — see identity-users.md

For dismissing risky users, refer to `identity-users.md` > Dismiss Risky User section.
