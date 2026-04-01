# Standards & Compliance Reference

## Standards

Standards ensure consistent configuration across Microsoft 365 tenants. CIPP can enforce settings, detect drift, and remediate non-compliance.

### List Applied Standards [API]

```
GET /api/ListStandards?TenantFilter={tenant}
```

Returns all standards applied to a tenant with their current state.

### Deploy Standards [API + UI]

```
POST /api/AddStandardsDeploy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Standards": {
    "PasswordExpireDisabled": true,
    "OAuthAppConsent": true,
    "DisableBasicAuth": true,
    "UnifiedAuditLog": true,
    "ModernAuth": true,
    "SecurityDefaults": false,
    "AuditLog": true,
    "DisableSharedMailboxSignIn": true,
    "ActivityBasedTimeout": true,
    ...
  }
}
```

**UI Path:** CIPP > Tenant > Standards & Drift > Standards & Drift Alignment

### Run Standards [API]

```
GET /api/ExecStandardsRun?TenantFilter={tenant}
GET /api/CIPPStandardsRun
```

Triggers an immediate standards compliance check and remediation.

### Remove Standard [API]

```
GET /api/RemoveStandard?TenantFilter={tenant}&StandardName={name}
```

### Convert Standard Format [API]

```
GET /api/ExecStandardConvert
```

## Standards Templates [API + UI]

```
# List templates
GET /api/listStandardTemplates

# Add template
POST /api/AddStandardsTemplate
{
  "TemplateName": "MSP Baseline",
  "Standards": {
    "PasswordExpireDisabled": true,
    "OAuthAppConsent": true,
    ...
  }
}

# Remove template
POST /api/RemoveStandardTemplate
```

**UI Path:** CIPP > Tenant > Standards & Drift > Templates

### Available Standards Categories

Standards cover these areas (partial list):
- **Authentication:** Disable basic auth, enable modern auth, security defaults, password policies
- **Mail:** Disable shared mailbox sign-in, auto-forwarding, anonymous calendar sharing
- **Security:** Enable unified audit log, disable self-service purchases, OAuth app consent
- **Compliance:** Activity-based timeout, DLP policies, retention policies
- **Exchange:** Block consumer storage, set sharing policies
- **Azure AD:** Require admin consent for apps, enable risk-based CA

**UI Path:** CIPP > Tenant > Standards & Drift > Templates > Available Standards (full list)

## Standards Compare [API]

```
GET /api/ListStandardsCompare?TenantFilter={tenant}
```

Compares current tenant settings against applied standards.

## Tenant Alignment [API]

```
GET /api/ListTenantAlignment?TenantFilter={tenant}
```

Shows how well a tenant aligns with applied standards.

## Drift Detection & Management

### List Tenant Drift [API]

```
GET /api/ListTenantDrift?TenantFilter={tenant}
```

Shows configuration drift — settings that have changed from the standard baseline.

**UI Path:** CIPP > Tenant > Manage > Manage Drift

### Update Drift Deviation [API]

```
POST /api/ExecUpdateDriftDeviation
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DriftID": "drift-guid",
  "Action": "accept"  // accept = acknowledge deviation, remediate = fix it
}
```

### Clone Drift Configuration [API]

```
POST /api/ExecDriftClone
{
  "SourceTenant": "template-tenant.onmicrosoft.com",
  "TargetTenant": "contoso.onmicrosoft.com"
}
```

## Best Practice Analyser (BPA)

### Run BPA [API + UI]

```
POST /api/ExecBPA
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

**UI Path:** CIPP > Tenant > Standards & Drift > Best Practice Analyser

### List BPA Results [API]

```
GET /api/ListBPA?TenantFilter={tenant}
GET /api/BestPracticeAnalyser_List
```

### BPA Templates [API + UI]

```
GET  /api/ListBPATemplates
POST /api/AddBPATemplate
POST /api/RemoveBPATemplate
```

**UI Path:** CIPP > Tenant > Standards & Drift > Best Practice Analyser > Best Practice Templates

### Custom BPA Reports [UI]

**UI Path:** CIPP > Tenant > Standards & Drift > Best Practice Analyser > Custom Reports
- Build custom compliance reports
- Define custom checks

## Domain Analyser

### Run Domain Analysis [API]

```
GET /api/ExecDomainAnalyser?TenantFilter={tenant}
```

Analyses domain configuration: SPF, DKIM, DMARC, MX records, DNSSEC, etc.

### List Domain Analysis Results [API]

```
GET /api/ListDomainAnalyser
GET /api/ListDomainHealth?TenantFilter={tenant}&Domain={domain}
```

**UI Path:** CIPP > Tenant > Standards & Drift > Domains Analyser

### Domain Analyser Notes

- Results don't update instantly — CIPP caches domain analysis data
- New domains may not appear immediately after being added
- Deleted domains may persist in the analyser until the cache refreshes
- Force refresh by running `ExecDomainAnalyser` again

## Applied Standards Report [UI + API]

**UI Path:** CIPP > Tenant > Manage > Applied Standards Report
- Shows all standards applied to a specific tenant
- Displays compliance status for each standard

## Policies & Settings Deployed [UI]

**UI Path:** CIPP > Tenant > Manage > Policies and Settings Deployed
- Overview of all Intune policies, CA policies, and Exchange settings pushed to a tenant

## Configuration History [UI]

**UI Path:** CIPP > Tenant > Manage > History
- Tracks changes made through CIPP over time
