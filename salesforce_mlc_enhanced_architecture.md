# Enhanced Multiple Lead Converter (MLC+) — Architecture Design
> Built on top of Wissen Infotech's AppExchange MLC (a0N3A00000EJdWcUAL)
> Version: 2.0 | June 2026

---

## 1. Overview

The **Enhanced MLC+** extends the base Multiple Lead Converter with **7 additional enterprise-grade features**:

| # | Feature | Category |
|---|---|---|
| 1 | Scheduled / Auto Conversion | Automation |
| 2 | Email Notification after Conversion | Communication |
| 3 | Rollback / Undo Conversion | Data Safety |
| 4 | Custom Field Mapping UI | Configuration |
| 5 | Conversion History & Audit Log | Visibility |
| 6 | Bulk Re-assignment before Conversion | Productivity |
| 7 | External CRM Sync (HubSpot / Marketo) | Integration |

---

## 2. High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          Salesforce Org — MLC+ App                           │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                    Lightning App "MLC+"  (LWC)                         │  │
│  │                                                                        │  │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐  │  │
│  │  │  Lead        │ │  Field       │ │  Conversion  │ │  Rollback    │  │  │
│  │  │  Datatable   │ │  Mapping UI  │ │  History     │ │  Manager     │  │  │
│  │  │  + Bulk      │ │  (Config)    │ │  & Audit Log │ │  UI          │  │  │
│  │  │  Reassign    │ │              │ │              │ │              │  │  │
│  │  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘ └──────┬───────┘  │  │
│  └─────────┼────────────────┼────────────────┼────────────────┼───────────┘  │
│            │                │                │                │              │
│  ┌─────────▼────────────────▼────────────────▼────────────────▼───────────┐  │
│  │                      Apex Controller Layer                              │  │
│  │  LeadConverterController  │  FieldMappingController  │  AuditController │  │
│  └─────────────────────────────────────┬───────────────────────────────────┘  │
│                                        │                                      │
│  ┌─────────────────────────────────────▼───────────────────────────────────┐  │
│  │                         Apex Service Layer                               │  │
│  │                                                                          │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐  │  │
│  │  │ Lead Fetch   │  │ Duplicate    │  │ Conversion   │  │ Rollback    │  │  │
│  │  │ + Paginate   │  │ Detection    │  │ Service      │  │ Service     │  │  │
│  │  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘  │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐  │  │
│  │  │ Bulk         │  │ Email        │  │ Field        │  │ Audit Log   │  │  │
│  │  │ Reassign     │  │ Notification │  │ Mapping      │  │ Service     │  │  │
│  │  │ Service      │  │ Service      │  │ Service      │  │             │  │  │
│  │  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘  │  │
│  └─────────────────────────────────────┬───────────────────────────────────┘  │
│                                        │                                      │
│  ┌─────────────────────────────────────▼───────────────────────────────────┐  │
│  │               Salesforce Platform Layer                                  │  │
│  │  Lead │ Account │ Contact │ Opportunity │ Task │ EmailMessage            │  │
│  │  MLC_Audit_Log__c │ MLC_Field_Mapping__c │ MLC_Rollback_Snapshot__c      │  │
│  └─────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌─────────────────┐   ┌─────────────────┐   ┌──────────────────────────┐   │
│  │  Scheduled Flow │   │  Platform       │   │  Named Credentials       │   │
│  │  (Auto Convert) │   │  Events         │   │  HubSpot / Marketo API   │   │
│  └─────────────────┘   └─────────────────┘   └──────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────▼───────────────┐
                    │      External Systems          │
                    │  ┌──────────┐  ┌───────────┐  │
                    │  │ HubSpot  │  │  Marketo  │  │
                    │  │  CRM     │  │  MAP      │  │
                    │  └──────────┘  └───────────┘  │
                    └───────────────────────────────┘
```

---

## 3. Base Features (from Original MLC)

These remain unchanged from the base app:

- Paginated Lead datatable with keyword search and status filter
- Multi-select (individual / page-all / view-selected)
- Bulk conversion → Account + Contact (auto) + Opportunity (optional checkbox)
- Duplicate detection → merge into existing Account/Contact
- `Database.convertLead()` with `allOrNone = false` (partial success)
- 3 standard reports: Lead Converter, Converted Leads, Not Converted Leads

---

## 4. Additional Feature 1 — Scheduled / Auto Conversion

### Purpose
Automatically convert Leads that meet predefined criteria (score, status, age, source) on a schedule — no manual user action needed.

### Architecture

```
Scheduled Flow (Daily/Weekly)
        │
        ▼
Evaluate Lead Criteria (Filter Conditions)
  e.g. Lead_Score__c >= 80
       Status = 'Qualified'
       CreatedDate <= TODAY - 7
        │
        ▼
Invoke Apex Action: AutoConversionService.runScheduled()
        │
        ├─▶ Fetch matching unconverted Leads (SOQL)
        ├─▶ Run Duplicate Detection
        ├─▶ Database.convertLead() in Queueable (chunked)
        ├─▶ Write to MLC_Audit_Log__c
        └─▶ Fire Email Notification (→ Feature 2)
```

### Custom Metadata — `MLC_Auto_Convert_Rule__mdt`

| Field | Type | Example |
|---|---|---|
| Rule_Name__c | Text | High Score Qualified Leads |
| Field_API_Name__c | Text | Lead_Score__c |
| Operator__c | Picklist | GreaterThanOrEqual |
| Value__c | Text | 80 |
| Create_Opportunity__c | Checkbox | true |
| Is_Active__c | Checkbox | true |
| Schedule_Frequency__c | Picklist | Daily / Weekly |

### Apex

```apex
public class AutoConversionService implements Schedulable {
    public void execute(SchedulableContext sc) {
        List<Lead> eligibleLeads = fetchEligibleLeads();
        LeadConversionService.convertAll(
            new Map<Id,Lead>(eligibleLeads).keySet(), true, null, null
        );
    }

    private List<Lead> fetchEligibleLeads() {
        // Dynamically build SOQL from MLC_Auto_Convert_Rule__mdt records
    }
}
```

**Scheduling via Apex:**
```apex
String cronExp = '0 0 6 * * ?'; // Daily at 6 AM
System.schedule('MLC Auto Convert Job', cronExp, new AutoConversionService());
```

---

## 5. Additional Feature 2 — Email Notification after Conversion

### Purpose
Send automated emails to the Lead owner, their manager, or the Lead (contact) after conversion completes — for both manual and scheduled conversions.

### Architecture

```
Conversion completes (manual or scheduled)
        │
        ▼
EmailNotificationService.sendConversionEmails(results)
        │
        ├─▶ Load Email Template (MLC_Conversion_Notification template)
        ├─▶ Resolve recipients per Notification Rule:
        │     • Lead Owner
        │     • Owner's Manager (User.ManagerId)
        │     • Converted Contact (email from Lead)
        │
        ├─▶ Messaging.SingleEmailMessage[] — bulk send
        └─▶ Log sent emails to EmailMessage (Activity History)
```

### Custom Metadata — `MLC_Notification_Rule__mdt`

| Field | Type | Description |
|---|---|---|
| Notify_Owner__c | Checkbox | Send to Lead owner |
| Notify_Manager__c | Checkbox | Send to owner's manager |
| Notify_Contact__c | Checkbox | Send to converted Contact email |
| Email_Template_Id__c | Text | API name of EmailTemplate |
| Trigger_On__c | Picklist | Manual / Scheduled / Both |

### Email Template (Salesforce Classic / Lightning)

```
Subject: Lead Converted — {!Lead.Name}

Hi {!User.FirstName},

The following Lead has been successfully converted:

Lead Name   : {!Lead.Name}
Company     : {!Lead.Company}
Account     : {!Account.Name}
Contact     : {!Contact.Name}
Opportunity : {!Opportunity.Name}
Converted By: {!$User.FirstName} {!$User.LastName}
Date        : {!NOW()}

View Record: [Account Link]
```

### Apex

```apex
public class EmailNotificationService {
    public static void sendConversionEmails(List<ConversionResult> results) {
        List<Messaging.SingleEmailMessage> emails = new List<Messaging.SingleEmailMessage>();
        MLC_Notification_Rule__mdt rule = loadRule();

        for (ConversionResult r : results) {
            if (rule.Notify_Owner__c)
                emails.add(buildEmail(r.ownerId, r, rule.Email_Template_Id__c));
            if (rule.Notify_Manager__c && r.managerId != null)
                emails.add(buildEmail(r.managerId, r, rule.Email_Template_Id__c));
            if (rule.Notify_Contact__c && r.contactEmail != null)
                emails.add(buildEmailToAddress(r.contactEmail, r));
        }

        Messaging.sendEmail(emails, false); // allOrNone = false
    }
}
```

---

## 6. Additional Feature 3 — Rollback / Undo Conversion

### Purpose
Allow users to "undo" a Lead conversion — reopen the Lead and optionally delete the Account, Contact, and Opportunity that were created during conversion.

### Architecture

```
User opens Conversion History tab
        │
        ▼
Selects a conversion record → clicks "Rollback"
        │
        ▼
Confirmation modal: "Delete created records? Yes / No"
        │
        ▼
RollbackService.rollback(auditLogId, deleteCreatedRecords)
        │
        ├─▶ Load MLC_Rollback_Snapshot__c (pre-conversion lead field values)
        ├─▶ Restore Lead fields from snapshot
        ├─▶ Set Lead.IsConverted = false (via LeadStatus update)
        ├─▶ If deleteCreatedRecords = true:
        │     ├─▶ Delete Opportunity (if created by this conversion)
        │     ├─▶ Delete Contact (if created by this conversion)
        │     └─▶ Delete Account (if created by this conversion & no other contacts)
        └─▶ Update MLC_Audit_Log__c: Status = 'Rolled Back'
```

### Custom Object — `MLC_Rollback_Snapshot__c`

| Field | Type | Description |
|---|---|---|
| Audit_Log__c | Lookup(MLC_Audit_Log__c) | Parent conversion record |
| Lead_Id__c | Text | Original Lead Id |
| Snapshot_Data__c | Long Text Area | JSON blob of Lead field values pre-conversion |
| Account_Created__c | Lookup(Account) | Account to delete on rollback |
| Contact_Created__c | Lookup(Contact) | Contact to delete on rollback |
| Opportunity_Created__c | Lookup(Opportunity) | Opportunity to delete on rollback |
| Is_New_Account__c | Checkbox | False if merged into existing — don't delete |
| Is_New_Contact__c | Checkbox | False if merged into existing — don't delete |

### Apex

```apex
public class RollbackService {
    public static void rollback(Id auditLogId, Boolean deleteCreated) {
        MLC_Rollback_Snapshot__c snap = [
            SELECT Snapshot_Data__c, Lead_Id__c,
                   Account_Created__c, Contact_Created__c, Opportunity_Created__c,
                   Is_New_Account__c, Is_New_Contact__c
            FROM MLC_Rollback_Snapshot__c
            WHERE Audit_Log__c = :auditLogId LIMIT 1
        ];

        // 1. Restore Lead from JSON snapshot
        Map<String, Object> snapData =
            (Map<String, Object>) JSON.deserializeUntyped(snap.Snapshot_Data__c);
        Lead l = new Lead(Id = snap.Lead_Id__c);
        // Dynamically set fields from snapshot...
        l.IsConverted = false;  // Reopen lead
        update l;

        // 2. Conditionally delete created records
        if (deleteCreated) {
            if (snap.Opportunity_Created__c != null)
                delete new Opportunity(Id = snap.Opportunity_Created__c);
            if (snap.Contact_Created__c != null && snap.Is_New_Contact__c)
                delete new Contact(Id = snap.Contact_Created__c);
            if (snap.Account_Created__c != null && snap.Is_New_Account__c) {
                Integer contactCount = [SELECT COUNT() FROM Contact
                                        WHERE AccountId = :snap.Account_Created__c];
                if (contactCount == 0)
                    delete new Account(Id = snap.Account_Created__c);
            }
        }

        // 3. Update Audit Log
        update new MLC_Audit_Log__c(Id = auditLogId, Status__c = 'Rolled Back');
    }
}
```

> ⚠ **Constraint:** Salesforce does not natively support setting `IsConverted = false` via standard DML. A workaround using `Database.update` with the `IsConverted` field on Lead requires special org permissions or a custom implementation using anonymous Apex / metadata API tricks. Thorough testing required.

---

## 7. Additional Feature 4 — Custom Field Mapping UI

### Purpose
Give admins a visual drag-and-drop style UI to configure which Lead fields map to which Account / Contact / Opportunity fields — without code changes.

### Architecture

```
Admin opens "Field Mapping" tab in MLC+ App
        │
        ▼
LWC: mlcFieldMappingConfig
  ├─▶ Left Panel: Lead fields (fetched via Schema.SObjectType.Lead.fields.getMap())
  ├─▶ Right Panel: Target Object selector (Account / Contact / Opportunity)
  │               + Target field dropdown
  ├─▶ Mapping rows: Source Field → Target Object → Target Field → Override Existing
  └─▶ Save button → upsert MLC_Field_Mapping__mdt records
```

### Custom Metadata — `MLC_Field_Mapping__mdt`

| Field | Type | Description |
|---|---|---|
| Source_Field__c | Text | Lead field API name (e.g. `LeadSource`) |
| Target_Object__c | Picklist | Account / Contact / Opportunity |
| Target_Field__c | Text | Target field API name (e.g. `Lead_Source__c`) |
| Override_Existing__c | Checkbox | Overwrite value if target record exists |
| Is_Active__c | Checkbox | Enable/disable this mapping |
| Mapping_Order__c | Number | Priority when multiple mappings exist |

### Apex — Field Mapping Service

```apex
public class FieldMappingService {

    // Load all active mappings
    public static List<MLC_Field_Mapping__mdt> getActiveMappings() {
        return [SELECT Source_Field__c, Target_Object__c, Target_Field__c,
                       Override_Existing__c
                FROM MLC_Field_Mapping__mdt
                WHERE Is_Active__c = true
                ORDER BY Mapping_Order__c ASC];
    }

    // Apply mappings when building LeadConvert object
    public static void applyMappings(Lead lead, Account acc, Contact con, Opportunity opp) {
        for (MLC_Field_Mapping__mdt mapping : getActiveMappings()) {
            Object value = lead.get(mapping.Source_Field__c);
            if (value == null) continue;

            if (mapping.Target_Object__c == 'Account' &&
               (mapping.Override_Existing__c || acc.get(mapping.Target_Field__c) == null))
                acc.put(mapping.Target_Field__c, value);

            if (mapping.Target_Object__c == 'Contact' &&
               (mapping.Override_Existing__c || con.get(mapping.Target_Field__c) == null))
                con.put(mapping.Target_Field__c, value);

            if (mapping.Target_Object__c == 'Opportunity' && opp != null &&
               (mapping.Override_Existing__c || opp.get(mapping.Target_Field__c) == null))
                opp.put(mapping.Target_Field__c, value);
        }
    }

    // Save mappings from UI (upsert Custom Metadata via Metadata API)
    @AuraEnabled
    public static void saveMappings(List<MappingDTO> mappings) {
        // Use Metadata.DeployContainer to upsert Custom Metadata records
    }
}
```

### LWC UI Behaviour

```
Lead Fields (Left)          Target Object     Target Field (Right)
─────────────────────       ─────────────     ──────────────────────
☐ LeadSource           →   Account       →   Lead_Source__c         [Override ☑]
☐ Industry             →   Account       →   Industry               [Override ☐]
☐ Title                →   Contact       →   Title                  [Override ☑]
☐ AnnualRevenue        →   Opportunity   →   Amount                 [Override ☐]

[ + Add Mapping Row ]                                  [ Save Mappings ]
```

---

## 8. Additional Feature 5 — Conversion History & Audit Log

### Purpose
Full traceable record of every conversion — who did it, when, which leads, what was created, success/failure, and rollback status.

### Custom Object — `MLC_Audit_Log__c`

| Field | Type | Description |
|---|---|---|
| Lead__c | Lookup(Lead) | Source Lead |
| Lead_Name__c | Text | Lead name (preserved post-conversion) |
| Converted_By__c | Lookup(User) | User who triggered conversion |
| Conversion_Type__c | Picklist | Manual / Scheduled / Rollback |
| Status__c | Picklist | Success / Failed / Partial / Rolled Back |
| Account_Created__c | Lookup(Account) | Created/linked Account |
| Contact_Created__c | Lookup(Contact) | Created/linked Contact |
| Opportunity_Created__c | Lookup(Opportunity) | Created Opportunity |
| Was_Duplicate__c | Checkbox | Duplicate detected during conversion |
| Error_Message__c | Long Text | Failure reason if Status = Failed |
| Conversion_Timestamp__c | DateTime | When conversion ran |
| Batch_Id__c | Text | Groups leads converted in same click/job |
| Notification_Sent__c | Checkbox | Email notification fired |
| Rollback_Available__c | Formula(Checkbox) | True if Status = Success & snapshot exists |

### Audit Log UI Tab — `mlcAuditLog` LWC

```
┌───────────────────────────────────────────────────────────────────┐
│  Conversion History                         [ Export CSV ]        │
│                                                                   │
│  Filter: [ Date Range ] [ Converted By ] [ Status ] [ Type ]     │
│                                                                   │
│  Lead Name    │ Converted By │ Date       │ Status  │ Actions     │
│  ─────────────┼──────────────┼────────────┼─────────┼──────────── │
│  John Smith   │ Amy Lee      │ 2026-06-28 │ Success │ [Rollback]  │
│  Acme Corp    │ Bob Ray      │ 2026-06-27 │ Failed  │ [View Error]│
│  Jane Doe     │ Scheduled    │ 2026-06-26 │ Success │ [Rollback]  │
└───────────────────────────────────────────────────────────────────┘
```

### Apex — Audit Log Service

```apex
public class AuditLogService {

    public static void logConversions(
        List<Database.LeadConvertResult> results,
        List<Lead> leads,
        String batchId,
        String conversionType
    ) {
        List<MLC_Audit_Log__c> logs = new List<MLC_Audit_Log__c>();
        Map<Id, Lead> leadMap = new Map<Id, Lead>(leads);

        for (Database.LeadConvertResult r : results) {
            Lead l = leadMap.get(r.getLeadId());
            logs.add(new MLC_Audit_Log__c(
                Lead__c               = r.getLeadId(),
                Lead_Name__c          = l.Name,
                Converted_By__c       = UserInfo.getUserId(),
                Conversion_Type__c    = conversionType,
                Status__c             = r.isSuccess() ? 'Success' : 'Failed',
                Account_Created__c    = r.getAccountId(),
                Contact_Created__c    = r.getContactId(),
                Opportunity_Created__c = r.getOpportunityId(),
                Error_Message__c      = r.isSuccess() ? null :
                                        r.getErrors()[0].getMessage(),
                Conversion_Timestamp__c = DateTime.now(),
                Batch_Id__c           = batchId
            ));
        }
        insert logs;

        // Also create Rollback Snapshots for successful conversions
        RollbackSnapshotService.createSnapshots(logs, leadMap);
    }
}
```

---

## 9. Additional Feature 6 — Bulk Re-assignment before Conversion

### Purpose
Allow users to change the **owner** of selected leads (in bulk) before converting — useful when leads were imported with wrong owners or when redistributing to the right sales rep.

### UI Flow

```
Lead Datatable
        │
User selects leads → clicks [ Re-assign Owner ]
        │
        ▼
Modal: "Assign selected leads to:"
  [ User Lookup Field ]    ← search Salesforce users
  [ ☐ Also update Opportunity Owner after conversion ]
  [ Assign ]  [ Cancel ]
        │
        ▼
BulkReassignService.reassign(leadIds, newOwnerId, updateOpportunity)
        │
        ├─▶ update Lead records with new OwnerId
        ├─▶ Log reassignment in MLC_Audit_Log__c (pre-conversion note)
        └─▶ Leads remain in datatable; user proceeds to Convert
```

### Apex

```apex
public class BulkReassignService {

    @AuraEnabled
    public static void reassign(
        List<Id> leadIds,
        Id newOwnerId,
        Boolean updateFutureOpportunity
    ) {
        List<Lead> leads = new List<Lead>();
        for (Id lid : leadIds)
            leads.add(new Lead(Id = lid, OwnerId = newOwnerId));

        update leads;

        // Store flag so ConversionService knows to set Opportunity.OwnerId too
        if (updateFutureOpportunity)
            storeOpportunityOwnerPreference(leadIds, newOwnerId);
    }
}
```

### Integration with Conversion

When `LeadConversionService` runs, it checks if a pre-conversion owner preference exists and sets `Opportunity.OwnerId` accordingly after the `Database.convertLead()` call.

---

## 10. Additional Feature 7 — External CRM Sync (HubSpot / Marketo)

### Purpose
After conversion, push the created Account, Contact, and Opportunity records to external systems (HubSpot CRM or Marketo MAP) via REST API — keeping all systems in sync.

### Architecture

```
Conversion completes
        │
        ▼
Platform Event fired: MLC_Conversion_Complete__e
  { accountId, contactId, opportunityId, leadId, syncTargets: ['hubspot','marketo'] }
        │
        ▼
Platform Event Trigger → ExternalSyncService (future/queueable)
        │
        ├─▶ HubSpot Sync
        │     ├─▶ Named Credential: NC_HubSpot
        │     ├─▶ POST /crm/v3/objects/contacts  (Contact data)
        │     ├─▶ POST /crm/v3/objects/companies (Account data)
        │     ├─▶ POST /crm/v3/objects/deals     (Opportunity data)
        │     └─▶ Associate contact → company → deal
        │
        └─▶ Marketo Sync
              ├─▶ Named Credential: NC_Marketo
              ├─▶ POST /rest/v1/leads.json        (upsert Lead/Contact)
              ├─▶ POST /rest/v1/opportunities.json (sync Opportunity)
              └─▶ Associate lead → opportunity
```

### Platform Event — `MLC_Conversion_Complete__e`

| Field | Type | Description |
|---|---|---|
| Account_Id__c | Text | Salesforce Account Id |
| Contact_Id__c | Text | Salesforce Contact Id |
| Opportunity_Id__c | Text | Salesforce Opportunity Id |
| Lead_Id__c | Text | Original Lead Id |
| Sync_Targets__c | Text | Comma-separated: hubspot,marketo |
| Conversion_Timestamp__c | DateTime | When conversion occurred |

### Named Credentials

```
Setup → Named Credentials
  ├── NC_HubSpot
  │     URL: https://api.hubapi.com
  │     Auth: OAuth 2.0 (HubSpot Private App Token)
  │
  └── NC_Marketo
        URL: https://<instance>.mktorest.com
        Auth: OAuth 2.0 (Marketo Client Credentials)
```

### Apex — External Sync Service

```apex
public class ExternalSyncService implements Queueable, Database.AllowsCallouts {

    private MLC_Conversion_Complete__e event;

    public ExternalSyncService(MLC_Conversion_Complete__e evt) {
        this.event = evt;
    }

    public void execute(QueueableContext ctx) {
        List<String> targets = event.Sync_Targets__c.split(',');

        if (targets.contains('hubspot'))
            syncToHubSpot();

        if (targets.contains('marketo'))
            syncToMarketo();
    }

    private void syncToHubSpot() {
        // Fetch SF records
        Contact con = [SELECT Id, FirstName, LastName, Email, Phone
                       FROM Contact WHERE Id = :event.Contact_Id__c];
        Account acc = [SELECT Id, Name, Industry FROM Account WHERE Id = :event.Account_Id__c];

        // POST to HubSpot Contacts API
        HttpRequest req = new HttpRequest();
        req.setEndpoint('callout:NC_HubSpot/crm/v3/objects/contacts');
        req.setMethod('POST');
        req.setHeader('Content-Type', 'application/json');
        req.setBody(JSON.serialize(new Map<String, Object>{
            'properties' => new Map<String, Object>{
                'firstname' => con.FirstName,
                'lastname'  => con.LastName,
                'email'     => con.Email,
                'phone'     => con.Phone
            }
        }));
        new Http().send(req);
        // Repeat for Company, Deal; then associate
    }

    private void syncToMarketo() {
        // POST to Marketo REST API /rest/v1/leads.json
    }
}
```

### Sync Status Tracking — `MLC_Sync_Log__c`

| Field | Type | Description |
|---|---|---|
| Audit_Log__c | Lookup(MLC_Audit_Log__c) | Parent conversion |
| Target_System__c | Picklist | HubSpot / Marketo |
| Sync_Status__c | Picklist | Success / Failed / Pending |
| External_Id__c | Text | ID returned from external system |
| Error_Message__c | Long Text | API error details |
| Synced_At__c | DateTime | Timestamp of successful sync |

---

## 11. Complete Data Model

```
MLC_Audit_Log__c
    ├── MLC_Rollback_Snapshot__c  (1:1)
    └── MLC_Sync_Log__c           (1:Many — one per external system)

MLC_Field_Mapping__mdt            (Custom Metadata — admin configurable)
MLC_Auto_Convert_Rule__mdt        (Custom Metadata — scheduler criteria)
MLC_Notification_Rule__mdt        (Custom Metadata — email rules)

Platform Event: MLC_Conversion_Complete__e
Named Credentials: NC_HubSpot, NC_Marketo
```

---

## 12. Complete Package Structure

```
MLC+ Enhanced Package
│
├── Lightning App
│   └── MLC_Plus.app
│
├── Lightning Web Components
│   ├── mlcLeadDatatable/          ← Base: lead list, search, filter, pagination
│   ├── mlcConversionPanel/        ← Base: convert button, opportunity toggle
│   ├── mlcBulkReassign/           ← NEW: owner reassignment modal
│   ├── mlcFieldMappingConfig/     ← NEW: admin field mapping UI
│   ├── mlcAuditLog/               ← NEW: history & audit log tab
│   └── mlcRollbackManager/        ← NEW: rollback UI
│
├── Apex Classes
│   ├── LeadConverterController.cls
│   ├── FieldMappingController.cls
│   ├── AuditController.cls
│   ├── LeadFetchService.cls
│   ├── LeadDuplicateService.cls
│   ├── LeadConversionService.cls
│   ├── AutoConversionService.cls       ← NEW
│   ├── EmailNotificationService.cls    ← NEW
│   ├── RollbackService.cls             ← NEW
│   ├── RollbackSnapshotService.cls     ← NEW
│   ├── FieldMappingService.cls         ← NEW
│   ├── AuditLogService.cls             ← NEW
│   ├── BulkReassignService.cls         ← NEW
│   ├── ExternalSyncService.cls         ← NEW
│   └── *_Test.cls (for all above)
│
├── Custom Objects
│   ├── MLC_Audit_Log__c
│   ├── MLC_Rollback_Snapshot__c
│   └── MLC_Sync_Log__c
│
├── Custom Metadata Types
│   ├── MLC_Field_Mapping__mdt
│   ├── MLC_Auto_Convert_Rule__mdt
│   └── MLC_Notification_Rule__mdt
│
├── Platform Events
│   └── MLC_Conversion_Complete__e
│
├── Named Credentials
│   ├── NC_HubSpot
│   └── NC_Marketo
│
├── Email Templates
│   └── MLC_Conversion_Notification
│
├── Scheduled Jobs
│   └── AutoConversionService (Schedulable)
│
└── Permission Sets
    ├── MLC_User_Access          ← Standard users
    └── MLC_Admin_Access         ← Field mapping + audit log access
```

---

## 13. End-to-End User Journey

```
① Admin configures Field Mappings via UI tab
② Admin sets Auto Conversion rules & schedule
③ Admin configures Email Notification rules
④ Admin sets up Named Credentials for HubSpot/Marketo

── MANUAL CONVERSION PATH ──────────────────────────────────────
⑤ User opens MLC+ App → Lead Datatable loads (paginated)
⑥ User searches/filters leads → selects multiple
⑦ User optionally clicks "Re-assign Owner" → assigns to correct rep
⑧ User reviews Duplicate preview
⑨ User checks "Create Opportunity" toggle
⑩ User clicks Convert
⑪ Conversion runs → Account + Contact (+ Opportunity) created
⑫ Audit Log entry written + Rollback Snapshot saved
⑬ Email Notifications sent to owner / manager / contact
⑭ Platform Event fires → External Sync to HubSpot/Marketo
⑮ User sees result summary with links to created records

── SCHEDULED CONVERSION PATH ───────────────────────────────────
⑤ Scheduled Job fires (e.g. daily 6AM)
⑥ AutoConversionService fetches leads matching rules
⑦ Batch conversion runs
⑧ Audit Log written, Emails sent, External Sync fired
```

---

## 14. Security Model

| Layer | Implementation |
|---|---|
| App Access | `MLC_User_Access` permission set |
| Admin Features | `MLC_Admin_Access` permission set (field mapping, audit, rules) |
| Apex Sharing | All controllers `with sharing` |
| CRUD/FLS | `Security.stripInaccessible()` before all DML |
| External API Keys | Stored in Named Credentials — never hardcoded |
| Audit Log | Read-only for standard users; full access for admins |
| Rollback | Restricted to `MLC_Admin_Access` or record owner |

---

## 15. Governor Limit Strategy

| Feature | Concern | Solution |
|---|---|---|
| Bulk Conversion | DML limits | Max 100 leads per Queueable chunk |
| Email Notifications | 10 emails/day limit (SingleEmailMessage) | Use `OrgWideEmailAddress` + Mass Email methods |
| External Sync | Callout limits (100/tx) | Platform Event → Queueable (callouts allowed) |
| Auto Conversion | CPU / heap | Scheduled Apex → Queueable chain |
| Audit Logging | DML per transaction | Collect all logs → single insert after conversion |
| Field Mapping | SOQL per mapping | Cache metadata in static map within transaction |

---

*Document Version: 2.0 | MLC+ Enhanced Architecture | June 2026*
