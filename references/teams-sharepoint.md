# Teams & SharePoint Reference

## Teams

### List Teams [API]

```
GET /api/ListTeams?TenantFilter={tenant}
```

Returns all Teams with: displayName, id, description, visibility, memberCount, etc.

**UI Path:** CIPP > Teams & SharePoint > Teams

### Add Team [API + UI]

```
POST /api/AddTeam
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Project Alpha",
  "Description": "Team for Project Alpha",
  "Visibility": "Private",
  "Owners": [{"label": "Admin", "value": "user-guid"}],
  "Members": [{"label": "User1", "value": "user-guid"}]
}
```

**UI Path:** CIPP > Teams & SharePoint > Teams > Add Team

### Teams Activity [API]

```
GET /api/ListTeamsActivity?TenantFilter={tenant}
```

Returns Teams usage activity reports.

### Convert Group to Team [API]

See `identity-groups.md` — `POST /api/AddGroupTeam`

## Teams Voice

### List Voice Users [API]

```
GET /api/ListTeamsVoice?TenantFilter={tenant}
```

Returns users with Teams voice/phone system assignments.

**UI Path:** CIPP > Teams & SharePoint > Teams Voice

### Assign Phone Number [API]

```
POST /api/ExecTeamsVoicePhoneNumberAssignment
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "PhoneNumber": "+441234567890",
  "PhoneNumberType": "DirectRouting"
}
```

### Remove Phone Number [API]

```
POST /api/ExecRemoveTeamsVoicePhoneNumberAssignment
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

### LIS Locations [API]

```
GET /api/ListTeamsLisLocation?TenantFilter={tenant}
```

Returns Location Information Service (LIS) locations for E911 compliance.

## SharePoint

### List Sites [API]

```
GET /api/ListSites?TenantFilter={tenant}
```

Returns SharePoint sites with: url, title, storageUsed, storageQuota, lastModified, etc.

**UI Path:** CIPP > Teams & SharePoint > SharePoint Sites

### Add Site [API + UI]

```
POST /api/AddSite
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "SiteName": "Project Documents",
  "SiteUrl": "ProjectDocs",
  "Template": "STS#3",
  "Owner": "admin@contoso.com",
  "StorageQuota": 1024
}
```

### Bulk Add Sites [API]

```
POST /api/AddSiteBulk
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Sites": [
    {"SiteName": "Site1", "SiteUrl": "site1", "Template": "STS#3", "Owner": "admin@contoso.com"},
    {"SiteName": "Site2", "SiteUrl": "site2", "Template": "STS#3", "Owner": "admin@contoso.com"}
  ]
}
```

### Delete Site [API]

```
POST /api/DeleteSharepointSite
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "SiteUrl": "https://contoso.sharepoint.com/sites/oldsite"
}
```

### SharePoint Admin URL [API]

```
GET /api/ListSharepointAdminUrl?TenantFilter={tenant}
```

Returns the SharePoint admin centre URL for the tenant.

### SharePoint Quota [API]

```
GET /api/ListSharepointQuota?TenantFilter={tenant}
```

Returns storage quota information across all sites.

### SharePoint Settings [API]

```
GET /api/ListSharepointSettings?TenantFilter={tenant}
```

Returns tenant-level SharePoint settings (sharing, external access, etc.).

### SharePoint Permissions [API]

```
POST /api/ExecSharePointPerms
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "SiteUrl": "https://contoso.sharepoint.com/sites/project",
  "UserID": "user@contoso.com",
  "Role": "Owner"
}
```

### Set SharePoint Member [API]

```
POST /api/ExecSetSharePointMember
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "SiteUrl": "https://contoso.sharepoint.com/sites/project",
  "UserID": "user@contoso.com",
  "Action": "Add",
  "Role": "Member"
}
```

## OneDrive — see identity-users.md

OneDrive provisioning and shortcut creation are managed via user endpoints:
- `POST /api/ExecOnedriveProvision`
- `POST /api/ExecOneDriveShortCut`

## Common Workflows

### Create Team with SharePoint Site

Teams automatically creates an associated SharePoint site. Use `AddTeam` and the site is provisioned automatically.

### Migrate SharePoint Permissions

1. List current permissions: `GET /api/ListSites` + check per-site
2. Set new permissions: `POST /api/ExecSharePointPerms`
3. Remove old permissions: `POST /api/ExecSetSharePointMember` with `Action: "Remove"`
