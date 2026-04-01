# Identity — Devices Reference

## List Devices (Identity) [API]

Azure AD / Entra ID registered devices. For Intune-managed devices, see `endpoint-intune.md`.

```
GET /api/ListDevices?TenantFilter={tenant}
```

Returns device objects with: displayName, deviceId, operatingSystem, operatingSystemVersion, trustType, isCompliant, isManaged, registeredOwners, etc.

**UI Path:** CIPP > Identity > Administration > Devices

## Delete Device [API]

```
POST /api/ExecDeviceDelete
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "device-guid"
}
```

**UI Path:** CIPP > Identity > Administration > Devices > click device > Delete

## User's Devices [API]

```
GET /api/ListUserDevices?TenantFilter={tenant}&UserID={userId}
```

Returns devices associated with a specific user.

## Deleted Items (Including Devices) [API]

```
GET /api/ListDeletedItems?TenantFilter={tenant}
```

Lists deleted users, groups, applications — can include device-related objects.

**UI Path:** CIPP > Identity > Administration > Deleted Items

## Restore Deleted Object [API]

```
POST /api/ExecRestoreDeleted
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "object-guid"
}
```

## Permanently Remove Deleted Object [API]

```
POST /api/RemoveDeletedObject
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "object-guid",
  "Type": "Device"
}
```

## Roles [API + UI]

While primarily an identity concept, device-related roles (e.g. Cloud Device Administrator) can be listed:

```
GET /api/ListRoles?TenantFilter={tenant}
```

**UI Path:** CIPP > Identity > Administration > Roles
- Shows all Azure AD roles and their current members
- Helps identify who has device management permissions

## Mailbox Mobile Devices [API]

Lists mobile devices connected to a user's mailbox (Exchange ActiveSync):

```
GET /api/ExecMailboxMobileDevices?TenantFilter={tenant}&UserID={userId}
```

Also available:
```
GET /api/ListMailboxMobileDevices?TenantFilter={tenant}&UserID={userId}
```

## Notes

- Azure AD devices vs Intune devices: Azure AD tracks device registration/join status. Intune (see `endpoint-intune.md`) tracks management, compliance, and policies.
- For device compliance reports across all tenants: `GET /api/ListAllTenantDeviceCompliance`
- For Intune device actions (wipe, retire, sync): see `endpoint-intune.md`
