# Endpoint — Autopilot Reference

## Autopilot Devices

### List Devices [API]

```
GET /api/ListAPDevices?TenantFilter={tenant}
```

Returns device objects: serialNumber, model, manufacturer, groupTag, deploymentProfileAssignmentStatus, etc.

**UI Path:** CIPP > Endpoint > Autopilot > Autopilot Devices

### Add Device [API + UI]

```
POST /api/AddAPDevice
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "autopilotData": "serial,hash\n1234567890,BASE64HASH...",
  "Groupname": "Autopilot Devices"
}
```

The `autopilotData` field accepts CSV format with serial number and hardware hash.

**UI Path:** CIPP > Endpoint > Autopilot > Add Autopilot Device

### Assign Device to User [API]

```
POST /api/ExecAssignAPDevice
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "device": "device-guid",
  "serialNumber": "1234567890",
  "user": "user@contoso.com"
}
```

### Rename Device [API]

```
POST /api/ExecRenameAPDevice
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "deviceId": "device-guid",
  "displayName": "LAPTOP-JOHN-001",
  "serialNumber": "1234567890"
}
```

### Set Group Tag [API]

```
POST /api/ExecSetAPDeviceGroupTag
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "deviceId": "device-guid",
  "groupTag": "London-Office",
  "serialNumber": "1234567890"
}
```

### Sync Autopilot Devices [API]

```
POST /api/ExecSyncAPDevices
{
  "tenantFilter": "contoso.onmicrosoft.com"
}
```

Also accepts `?tenantFilter=...` as query parameter.

### Remove Device [API]

```
POST /api/RemoveAPDevice
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "ID": "device-guid"
}
```

Also accepts `?tenantFilter=...&ID=...` as query parameters.

## Autopilot Profiles

### List Profiles [API]

```
GET /api/ListAutopilotconfig?TenantFilter={tenant}
```

Optional: `type` query parameter.

**UI Path:** CIPP > Endpoint > Autopilot > Profiles

### Add Profile [API + UI]

```
POST /api/AddAutopilotConfig
{
  "selectedTenants": "contoso.onmicrosoft.com",
  "DisplayName": "Standard Deployment",
  "Description": "Standard user deployment profile",
  "DeploymentMode": true,
  "CollectHash": true,
  "Autokeyboard": true,
  "HidePrivacy": true,
  "HideTerms": true,
  "HideChangeAccount": true,
  "NotLocalAdmin": true,
  "allowWhiteGlove": true,
  "DeviceNameTemplate": "PC-%SERIAL:7%",
  "languages": {"label": "English (United Kingdom)", "value": "en-GB"},
  "Assignto": true
}
```

**UI Path:** CIPP > Endpoint > Autopilot > Profiles > Add Profile

### Remove Profile [API]

```
POST /api/RemoveAutopilotConfig
{
  "tenantFilter": "contoso.onmicrosoft.com",
  "ID": "profile-guid",
  "displayName": "Old Profile",
  "assignments": ""
}
```

## Enrollment Status Pages

### List Status Pages [API]

Status pages are returned as part of `ListAutopilotconfig` with `type=ESP` or similar.

```
GET /api/ListAutopilotconfig?TenantFilter={tenant}&type=EnrollmentStatusPage
```

**UI Path:** CIPP > Endpoint > Autopilot > Status Pages

### Add Enrollment Status Page [API + UI]

```
POST /api/AddEnrollment
{
  "selectedTenants": "contoso.onmicrosoft.com",
  "ShowProgress": true,
  "blockDevice": true,
  "AllowReset": true,
  "AllowFail": false,
  "EnableLog": true,
  "InstallWindowsUpdates": true,
  "OBEEOnly": false,
  "TimeOutInMinutes": "60",
  "ErrorMessage": "Contact IT support if setup fails."
}
```

**UI Path:** CIPP > Endpoint > Autopilot > Add Status Page

## Common Workflows

### New Device Onboarding

1. Import device hash: `POST /api/AddAPDevice` with serial+hash CSV
2. Set group tag: `POST /api/ExecSetAPDeviceGroupTag`
3. Assign user: `POST /api/ExecAssignAPDevice`
4. Sync: `POST /api/ExecSyncAPDevices`
5. Device boots → Autopilot profile applies → ESP runs

### Bulk Device Import

```bash
# Prepare CSV: serial,hash
CSV="Serial Number,Hardware Hash
SN001,BASE64HASH1
SN002,BASE64HASH2"

curl -s -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  "${CIPP_URL}/api/AddAPDevice" \
  -d "{\"TenantFilter\":\"contoso.onmicrosoft.com\",\"autopilotData\":\"$CSV\",\"Groupname\":\"NewDevices\"}"
```

## DEP Sync (Apple) [API]

```
POST /api/ExecSyncDEP
{
  "TenantFilter": "contoso.onmicrosoft.com"
}
```

Triggers a sync of Apple DEP (Device Enrollment Program) devices.

## Reports

**UI Path:** CIPP > Endpoint > Reports > Autopilot Deployments
- Shows deployment status for all Autopilot-enrolled devices
- No dedicated API endpoint — use the UI or ListDevices with filters
