# Email & Exchange Reference

## Mailboxes

### List Mailboxes [API]

```
GET /api/ListMailboxes?TenantFilter={tenant}
```

Returns mailbox objects: displayName, UPN, recipientType, primarySmtpAddress, etc.

**UI Path:** CIPP > Email & Exchange > Administration > Mailboxes

### Add Shared Mailbox [API + UI]

```
POST /api/AddSharedMailbox
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "Info Mailbox",
  "UserName": "info",
  "Domain": "contoso.com"
}
```

**UI Path:** CIPP > Email & Exchange > Administration > Mailboxes > Add Shared Mailbox

### Convert Mailbox Type [API]

```
POST /api/ExecConvertMailbox
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "user@contoso.com",
  "ConvertToType": "SharedMailbox"  // SharedMailbox | UserMailbox
}
```

### Mailbox Permissions [API]

```
# List permissions
GET /api/ListmailboxPermissions?TenantFilter={tenant}&UserID={userId}

# Edit permissions (Full Access, Send As, Send on Behalf)
POST /api/ExecEditMailboxPermissions
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "mailbox@contoso.com",
  "AddFullAccess": [{"label":"User","value":"user@contoso.com"}],
  "RemoveFullAccess": [],
  "AddSendAs": [],
  "RemoveSendAs": [],
  "AddSendOnBehalf": [],
  "RemoveSendOnBehalf": []
}

# Modify mailbox permissions (alternative)
POST /api/ExecModifyMBPerms
```

### Email Forwarding [API]

```
POST /api/ExecEmailForward
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "ForwardTo": "other@contoso.com",
  "KeepCopy": true,
  "DisableForwarding": false
}
```

### Set Mailbox Quota [API]

```
POST /api/ExecSetMailboxQuota
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "ProhibitSendQuota": "49GB",
  "ProhibitSendReceiveQuota": "50GB",
  "IssueWarningQuota": "48GB"
}
```

### Set Mailbox Email Size [API]

```
POST /api/ExecSetMailboxEmailSize
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "MaxSendSize": "35MB",
  "MaxReceiveSize": "36MB"
}
```

### Set Out of Office [API]

```
POST /api/ExecSetOoO
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "AutoReplyState": "Enabled",
  "InternalMessage": "I'm out of office.",
  "ExternalMessage": "I'm out of office. Contact support.",
  "StartTime": "2024-12-20T00:00:00Z",
  "EndTime": "2025-01-02T00:00:00Z"
}

# Get OoO status
GET /api/ListOoO?TenantFilter={tenant}&UserID={userId}
```

### Hide from GAL [API]

```
POST /api/ExecHideFromGAL
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "HideFromGAL": true
}
```

### Copy for Sent [API]

```
POST /api/ExecCopyForSent
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "shared@contoso.com",
  "MessageCopyForSentAsEnabled": true,
  "MessageCopyForSendOnBehalfEnabled": true
}
```

### Litigation Hold [API]

```
POST /api/ExecSetLitigationHold
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "LitigationHoldEnabled": true,
  "LitigationHoldDuration": 365
}
```

### Archive Mailbox [API]

```
POST /api/ExecEnableArchive
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}

# Auto-expanding archive
POST /api/ExecEnableAutoExpandingArchive
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

### Retention Hold [API]

```
POST /api/ExecSetRetentionHold
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "RetentionHoldEnabled": true
}
```

### Set Mailbox Locale [API]

```
POST /api/ExecSetMailboxLocale
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "Language": "en-GB",
  "DateFormat": "dd/MM/yyyy",
  "TimeFormat": "HH:mm",
  "TimeZone": "GMT Standard Time"
}
```

### Recipient Limits [API]

```
POST /api/ExecSetRecipientLimits
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "RecipientLimits": 500
}
```

### High Volume Email (HVE) User [API]

```
POST /api/ExecHVEUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

### Mailbox Rules [API]

```
# List rules
GET /api/ListMailboxRules?TenantFilter={tenant}&UserID={userId}

# Set rule
POST /api/ExecSetMailboxRule
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "RuleName": "Delete spam",
  "Conditions": {...},
  "Actions": {...}
}

# Remove rule
POST /api/ExecRemoveMailboxRule
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "RuleID": "rule-guid"
}
```

### Calendar Processing [API]

```
POST /api/ExecSetCalendarProcessing
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "room@contoso.com",
  "AutomateProcessing": "AutoAccept",
  "AllowConflicts": false,
  "BookingWindowInDays": 180
}
```

### Calendar Permissions [API]

```
GET /api/ListCalendarPermissions?TenantFilter={tenant}&UserID={userId}

POST /api/ExecEditCalendarPermissions
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com",
  "Permissions": [{"User":"other@contoso.com","AccessRights":"Editor"}]
}

POST /api/ExecModifyCalPerms
```

### Contact Permissions [API]

```
GET /api/ListContactPermissions?TenantFilter={tenant}&UserID={userId}

POST /api/ExecModifyContactPerms
```

### Restricted Users [API]

```
GET /api/ListRestrictedUsers?TenantFilter={tenant}

POST /api/ExecRemoveRestrictedUser
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

### Start Managed Folder Assistant [API]

```
POST /api/ExecStartManagedFolderAssistant
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}
```

### Mailbox Restore [API]

```
POST /api/ExecMailboxRestore
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "UserID": "user@contoso.com"
}

GET /api/ListMailboxRestores?TenantFilter={tenant}
```

### Shared Mailbox Statistics [API]

```
GET /api/ListSharedMailboxStatistics?TenantFilter={tenant}
```

## Contacts [API + UI]

```
# List contacts
GET /api/ListContacts?TenantFilter={tenant}

# Add contact
POST /api/AddContact
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "DisplayName": "External Contact",
  "ExternalEmailAddress": "ext@external.com",
  "FirstName": "External",
  "LastName": "Contact"
}

# Edit contact
POST /api/EditContact
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ContactID": "contact-guid",
  "DisplayName": "Updated Contact"
}

# Remove contact
POST /api/RemoveContact
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ContactID": "contact-guid"
}
```

### Contact Templates [API]

```
GET  /api/ListContactTemplates
POST /api/AddContactTemplates
POST /api/EditContactTemplates
POST /api/DeployContactTemplates
POST /api/RemoveContactTemplates
```

**UI Path:** CIPP > Email & Exchange > Administration > Contacts / Contact Templates

## Transport Rules [API + UI]

```
# List transport rules
GET /api/ListTransportRules?TenantFilter={tenant}

# Add transport rule
POST /api/AddTransportRule
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Name": "Block External Auto-Forward",
  "Priority": 0,
  "State": "Enabled",
  ...rule conditions and actions...
}

# Edit transport rule
POST /api/EditTransportRule

# Remove transport rule
POST /api/RemoveTransportRule
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "RuleID": "rule-guid"
}
```

### Transport Templates [API]

```
GET  /api/ListTransportRulesTemplates
POST /api/AddTransportTemplate
POST /api/RemoveTransportRuleTemplate
```

**UI Path:** CIPP > Email & Exchange > Transport > Transport Rules / Templates

## Exchange Connectors [API + UI]

```
GET  /api/ListExchangeConnectors?TenantFilter={tenant}
POST /api/AddExConnector
POST /api/EditExConnector
POST /api/RemoveExConnector

# Connector templates
GET  /api/ListExConnectorTemplates
POST /api/AddExConnectorTemplate
POST /api/RemoveExConnectorTemplate
```

**UI Path:** CIPP > Email & Exchange > Transport > Connectors / Connector Templates

## Spam Filters [API + UI]

```
# List spam filters
GET /api/ListSpamfilter?TenantFilter={tenant}

# Add/edit spam filter
POST /api/AddSpamFilter
POST /api/EditSpamFilter

# Templates
GET  /api/ListSpamFilterTemplates
POST /api/AddSpamFilterTemplate
POST /api/RemoveSpamfilterTemplate

# Remove spam filter
POST /api/RemoveSpamfilter
```

### Connection Filters [API]

```
GET  /api/ListConnectionFilter?TenantFilter={tenant}
POST /api/AddConnectionFilter
GET  /api/ListConnectionFilterTemplates
POST /api/AddConnectionFilterTemplate
POST /api/RemoveConnectionfilterTemplate
```

### Anti-Phishing / Malware Filters [API]

```
POST /api/EditAntiPhishingFilter
POST /api/EditMalwareFilter
POST /api/EditSafeAttachmentsFilter

# Reports
GET /api/ListAntiPhishingFilters?TenantFilter={tenant}
GET /api/ListMalwareFilters?TenantFilter={tenant}
GET /api/ListSafeAttachmentsFilters?TenantFilter={tenant}
```

**UI Path:** CIPP > Email & Exchange > Spamfilter

## Mail Quarantine [API + UI]

```
GET /api/ListMailQuarantine?TenantFilter={tenant}
GET /api/ListMailQuarantineMessage?TenantFilter={tenant}&MessageID={id}

POST /api/ExecQuarantineManagement
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ID": "message-id",
  "Action": "Release"  // Release | Delete | Deny
}
```

### Quarantine Policies [API]

```
POST /api/ListQuarantinePolicy
POST /api/AddQuarantinePolicy
POST /api/EditQuarantinePolicy
POST /api/RemoveQuarantinePolicy
```

**UI Path:** CIPP > Email & Exchange > Administration > Quarantine

## Tenant Allow/Block List [API + UI]

```
POST /api/AddTenantAllowBlockList
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "ListType": "Sender",
  "Action": "Block",
  "Entries": ["spam@example.com"],
  "Notes": "Known phishing sender"
}

POST /api/RemoveTenantAllowBlockList
```

**UI Path:** CIPP > Email & Exchange > Administration > Tenant Allow/Block Lists

## Retention Policies & Tags [API]

```
DELETE /api/ExecManageRetentionPolicies
DELETE /api/ExecManageRetentionTags
POST  /api/ExecSetMailboxRetentionPolicies
```

**UI Path:** CIPP > Email & Exchange > Administration > Retention Policies & Tags

## Resource Management [API + UI]

### Rooms
```
GET  /api/ListRooms?TenantFilter={tenant}
POST /api/AddRoomMailbox
POST /api/EditRoomMailbox
```

### Equipment
```
GET  /api/ListEquipment?TenantFilter={tenant}
POST /api/AddEquipmentMailbox
POST /api/EditEquipmentMailbox
```

### Room Lists
```
GET  /api/ListRoomLists?TenantFilter={tenant}
POST /api/AddRoomList
POST /api/EditRoomList
```

**UI Path:** CIPP > Email & Exchange > Resource Management > [Rooms | Equipment | Room Lists]

## Exchange Tools [API]

### Message Trace
```
POST /api/ListMessageTrace
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "SenderAddress": "user@contoso.com",
  "StartDate": "2024-01-01",
  "EndDate": "2024-01-31"
}
```

### Mail Test
```
GET /api/ExecMailTest?TenantFilter={tenant}&Email={email}
```

### Exchange Online Request (raw EXO cmdlet)
```
POST /api/ListExoRequest
{
  "TenantFilter": "contoso.onmicrosoft.com",
  "Cmdlet": "Get-Mailbox",
  "Parameters": { "Identity": "user@contoso.com" }
}
```

**UI Path:** CIPP > Email & Exchange > Tools

## Reports [API]

```bash
GET /api/ListMailboxCAS?TenantFilter={tenant}           # Mailbox Client Access Settings
GET /api/ListSharedMailboxAccountEnabled?TenantFilter={tenant}  # Shared mailboxes with sign-in enabled
GET /api/ListGlobalAddressList?TenantFilter={tenant}     # Global Address List
```

**UI Path:** CIPP > Email & Exchange > Reports
