# Endpoint — Intune / MEM Reference

## Devices

### List Devices [API]

```
GET /api/ListDevices?TenantFilter={tenant}
```

Returns Intune-managed device objects: deviceName, managementState, complianceState, osVersion, lastSyncDateTime, etc.

**UI Path:** CIPP > Endpoint > Device Management > Devices

### View Device Details [API]

```
GET /api/ListDeviceDetails?TenantFilter={tenant}&DeviceID={deviceId}
```

**UI Path:** CIPP > Endpoint > Device Management > Devices > click device

### Device Actions [API]

```
POST /api/ExecDeviceAction
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GUID": "device-guid",
  "Action": "syncDevice"
}
```

Available actions: `syncDevice`, `rebootNow`, `locateDevice`, `WindowsDefenderScan` (quick/full), `cleanWindowsDevice`, `wipe`, `retire`, `remoteLock`, `disableLostMode`, `rotateFileVaultKey`, `rotateLocalAdminPassword`, `shutDown`, `restartDevice`

### Device Passcode Actions [API]

```
POST /api/ExecDevicePasscodeAction
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GUID": "device-guid",
  "Action": "resetPasscode"
}
```

### Get BitLocker Recovery Key [API]

```
POST /api/ExecGetRecoveryKey
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GUID": "device-guid"
}
```

### Get Local Admin Password (LAPS) [API]

```
POST /api/ExecGetLocalAdminPassword
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GUID": "device-guid"
}
```

## Configuration Policies

### List Policies [API]

```
GET /api/ListIntunePolicy?TenantFilter={tenant}
```

**UI Path:** CIPP > Endpoint > Device Management > Configuration Policies

### Add Policy [API + UI]

```
POST /api/AddPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyType": "deviceConfiguration",
  "DisplayName": "WiFi Profile",
  "Description": "Corporate WiFi",
  "PolicyData": { ...policy JSON... }
}
```

**UI Path:** CIPP > Endpoint > Device Management > Apply Policy

### Edit Policy [API]

```
POST /api/EditPolicy
POST /api/EditIntunePolicy
```

### Assign Policy [API]

```
POST /api/ExecAssignPolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid",
  "Assignments": [{"label":"All Users","value":"all-users-guid"}]
}
```

### Remove Policy [API]

```
POST /api/RemovePolicy
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PolicyID": "policy-guid"
}
```

## Compliance Policies [API]

```
GET /api/ListCompliancePolicies?TenantFilter={tenant}
```

**UI Path:** CIPP > Endpoint > Device Management > Compliance Policies

## App Protection Policies [API]

```
GET /api/ListAppProtectionPolicies?TenantFilter={tenant}
```

**UI Path:** CIPP > Endpoint > Device Management > App Policies

## Policy Templates [API + UI]

```
GET  /api/ListIntuneTemplates
POST /api/AddIntuneTemplate
{
  "DisplayName": "Standard Config",
  "Description": "Standard device configuration",
  "TemplateType": "deviceConfiguration",
  "TemplateData": { ...JSON config... }
}
POST /api/RemoveIntuneTemplate
```

### Edit Template
```
POST /api/ExecEditTemplate (clone/modify)
```

**UI Path:** CIPP > Endpoint > Device Management > Policy Templates

## Scripts [API + UI]

```
GET /api/ListIntuneScript?TenantFilter={tenant}

PATCH /api/EditIntuneScript
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ScriptID": "script-guid",
  "DisplayName": "Updated Script",
  "ScriptContent": "base64-encoded-ps1"
}

POST /api/RemoveIntuneScript
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ScriptID": "script-guid"
}
```

**UI Path:** CIPP > Endpoint > Device Management > Scripts

## Reusable Settings [API + UI]

```
GET  /api/ListIntuneReusableSettings?TenantFilter={tenant}
POST /api/AddIntuneReusableSetting
POST /api/RemoveIntuneReusableSetting

# Templates
GET  /api/ListIntuneReusableSettingTemplates
POST /api/AddIntuneReusableSettingTemplate
POST /api/RemoveIntuneReusableSettingTemplate
```

**UI Path:** CIPP > Endpoint > Device Management > Reusable Settings / Reusable Settings Templates

## Assignment Filters [API + UI]

```
GET  /api/ListAssignmentFilters?TenantFilter={tenant}
POST /api/AddAssignmentFilter
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Windows 11 Only",
  "Description": "Filter for W11 devices",
  "Platform": "windows10AndLater",
  "Rule": "(device.osVersion -startsWith \"10.0.22\")"
}
POST /api/EditAssignmentFilter
DELETE /api/ExecAssignmentFilter

# Templates
GET  /api/ListAssignmentFilterTemplates
POST /api/AddAssignmentFilterTemplate
POST /api/RemoveAssignmentFilterTemplate
```

**UI Path:** CIPP > Endpoint > Device Management > Assignment Filters / Templates

## Defender [API + UI]

```
# Defender status
GET /api/ListDefenderState?TenantFilter={tenant}

# Deploy Defender
POST /api/AddDefenderDeployment
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DeployTo": "AllDevices"
}

# Threat & Vulnerability Management
GET /api/ListDefenderTVM?TenantFilter={tenant}
```

**UI Path:** CIPP > Security > Defender > [Defender Status | Deployment | Vulnerabilities]

## DEP Sync [API]

```
POST /api/ExecSyncDEP
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

## Applications

### List Apps [API]

```
GET /api/ListApps?TenantFilter={tenant}
```

**UI Path:** CIPP > Endpoint > Applications > Applications

### Add Applications [API + UI]

```
# Chocolatey app
POST /api/AddChocoApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PackageName": "googlechrome",
  "AssignTo": "AllDevices"
}

# Microsoft Store app
POST /api/AddStoreApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "PackageIdentifier": "Microsoft.VisualStudioCode",
  "AssignTo": "AllDevices"
}

# Office app
POST /api/AddOfficeApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Apps": ["Word","Excel","Outlook","Teams"],
  "Channel": "Current",
  "Architecture": "64",
  "AssignTo": "AllDevices"
}

# MSP/RMM app
POST /api/AddMSPApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ApplicationName": "RMM Agent",
  "InstallerUrl": "https://...",
  "InstallCommand": "msiexec /i agent.msi /qn",
  "UninstallCommand": "msiexec /x {GUID} /qn"
}

# Custom Win32 app
POST /api/AddWin32ScriptApp
```

### Assign App [API]

```
POST /api/ExecAssignApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "AppID": "app-guid",
  "AssignTo": "AllDevices",
  "Intent": "required"  // required | available | uninstall
}
```

### Application Queue [API]

```
GET /api/ListApplicationQueue
POST /api/RemoveQueuedApp
```

### VPP Sync [API]

```
POST /api/ExecSyncVPP
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

### Remove App [API]

```
POST /api/RemoveApp
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "AppID": "app-guid"
}
```

**UI Path:** CIPP > Endpoint > Applications > [Add Application | Queue]

## Intune Intents [API]

```
GET /api/ListIntuneIntents?TenantFilter={tenant}
```

## Detected / Discovered Apps [API + UI]

```
GET /api/ListDetectedApps?TenantFilter={tenant}
GET /api/ListDetectedAppDevices?TenantFilter={tenant}&AppID={appId}
```

**UI Path:** CIPP > Endpoint > Reports > Discovered Apps

## Reports [API]

```
GET /api/ListAllTenantDeviceCompliance   # Cross-tenant compliance
```

**UI Path:** CIPP > Endpoint > Reports > [Analytics Device Score | Work from Anywhere | Autopilot Deployments]
