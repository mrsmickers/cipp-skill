# Identity — Groups Reference

## List Groups [API]

```
GET /api/ListGroups?TenantFilter={tenant}
```

Optional: `GroupID` for specific group.

Returns array of group objects: displayName, id, groupTypes, mailEnabled, securityEnabled, membershipRule, etc.

```bash
curl -s -H "Authorization: Bearer $TOKEN" \
  "${CIPP_URL}/api/ListGroups?TenantFilter=contoso.onmicrosoft.com"
```

**UI Path:** CIPP > Identity > Administration > Groups

## Add Group [API + UI]

```
POST /api/AddGroup
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "IT Team",
  "Description": "IT department group",
  "GroupType": "Security",
  "MailNickname": "itteam",
  "MembershipRuleProcessingState": null,
  "MembershipRule": null,
  "Members": [
    {"label": "John Smith", "value": "user-guid-1"},
    {"label": "Jane Doe", "value": "user-guid-2"}
  ],
  "Owners": [
    {"label": "Admin User", "value": "owner-guid"}
  ]
}
```

GroupType options: `Security`, `Microsoft 365`, `Distribution`, `Mail-enabled Security`, `Dynamic Security`, `Dynamic Microsoft 365`

**UI Path:** CIPP > Identity > Administration > Groups > Add Group

## Edit Group [API]

```
PATCH /api/EditGroup
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GroupID": "group-guid",
  "DisplayName": "Updated Group Name",
  "Description": "Updated description",
  "AddMembers": [{"label":"New User","value":"user-guid"}],
  "RemoveMembers": [{"label":"Old User","value":"user-guid"}],
  "AddOwners": [{"label":"New Owner","value":"user-guid"}],
  "RemoveOwners": []
}
```

**UI Path:** CIPP > Identity > Administration > Groups > click group > Edit

## View Group Details [UI]

**UI Path:** CIPP > Identity > Administration > Groups > click group name
- Shows members, owners, group type, mail settings, dynamic membership rules

## Add Teams to Group [API]

Converts an existing Microsoft 365 group to a Teams-enabled group:

```
POST /api/AddGroupTeam
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GroupID": "group-guid",
  "DisplayName": "IT Team"
}
```

## Group Sender Authentication [API]

```
GET /api/ListGroupSenderAuthentication?TenantFilter={tenant}
```

Lists groups with their external sender authentication settings.

## Group Templates [API + UI]

### List Templates

```
GET /api/ListGroupTemplates
```

### Add Template

```
POST /api/AddGroupTemplate
{
  "TemplateName": "Standard Security Group",
  "GroupType": "Security",
  "Description": "Template for standard security groups",
  "MembershipRuleProcessingState": null,
  "allowExternal": false
}
```

### Deploy Template

**UI Path:** CIPP > Identity > Administration > Group Templates > Deploy
- Select template → Select tenant(s) → Deploy

### Remove Template

```
POST /api/RemoveGroupTemplate
{
  "ID": "template-guid"
}
```

**UI Path:** CIPP > Identity > Administration > Group Templates

## Group Actions via Exchange [API]

These endpoints manage Exchange-specific group properties:

### Delete Group

```
POST /api/ExecGroupsDelete
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GroupID": "group-guid",
  "GroupType": "Distribution"
}
```

### Hide Group from GAL

```
POST /api/ExecGroupsHideFromGAL
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GroupID": "group-guid",
  "HideFromGAL": true
}
```

### Delivery Management

```
POST /api/ExecGroupsDeliveryManagement
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "GroupID": "group-guid",
  "OnlyAllowInternal": true
}
```

## Roles [API + UI]

```
GET /api/ListRoles?TenantFilter={tenant}
```

Lists all directory roles with their members.

**UI Path:** CIPP > Identity > Administration > Roles

## Deleted Items [API + UI]

```
GET /api/ListDeletedItems?TenantFilter={tenant}
```

Lists deleted users, groups, and applications that can be restored.

**UI Path:** CIPP > Identity > Administration > Deleted Items
