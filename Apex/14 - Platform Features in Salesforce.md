
Salesforce provides several platform-level features that help build scalable, configurable, event-driven, and enterprise-grade applications.

Key Platform Features:

1. Platform Events
2. Change Data Capture (CDC)
3. Custom Metadata
4. Custom Settings
5. Approval Processes

---

# 1. Platform Events

Platform Events enable event-driven architecture in Salesforce.

Instead of systems directly calling each other:

```text
System A
   ↓
Platform Event
   ↓
System B
```

Systems communicate through events.

---

# What are Platform Events?

Platform Events are special Salesforce objects used for asynchronous communication.

Examples:

- Order Created
- Payment Processed
- Invoice Generated
- Customer Registered

---

# Platform Event Architecture

```text
Publisher
    ↓
Platform Event
    ↓
Subscriber
```

---

# Create Platform Event

```text
Setup
→ Platform Events
→ New Platform Event
```

Example:

```text
Order_Event__e
```

Fields:

```text
Order_Number__c
Amount__c
Status__c
```

---

# Publish Platform Event

```apex
Order_Event__e eventMsg =
new Order_Event__e();

eventMsg.Order_Number__c =
'ORD-1001';

eventMsg.Amount__c =
1000;

eventMsg.Status__c =
'Created';

EventBus.publish(
    eventMsg
);
```

---

# Publish Result

```apex
Database.SaveResult result =
EventBus.publish(
    eventMsg
);
```

---

# Trigger on Platform Event

```apex
trigger OrderEventTrigger
on Order_Event__e
(after insert){

    for(Order_Event__e evt :
        Trigger.New){

        System.debug(
            evt.Order_Number__c
        );
    }
}
```

---

# Platform Event Subscriber

Subscribers can be:

- Apex Triggers
- Flow
- LWC
- External Systems
- Middleware

---

# Platform Event Limits

High Volume Events:

```text
Millions of Events Per Day
```

(depending on edition/licensing)

---

# Platform Event Use Cases

- System integrations
- Event-driven architecture
- Real-time notifications
- Microservices communication

---

# Standard Platform Events

Examples:

```text
BatchApexErrorEvent
LoginEvent
LogoutEvent
```

---

# 2. Change Data Capture (CDC)

CDC captures record changes automatically.

Instead of writing triggers:

```text
Record Changed
      ↓
CDC Event Published
      ↓
Subscribers Notified
```

---

# What CDC Tracks

```text
Insert
Update
Delete
Undelete
```

---

# Enable CDC

```text
Setup
→ Change Data Capture
→ Select Object
```

Example:

```text
Account
Contact
Opportunity
```

---

# CDC Event Example

Object:

```text
Account
```

Event:

```text
AccountChangeEvent
```

---

# Sample Change Event Fields

```text
Record Id
Changed Fields
Change Type
Replay Id
Commit User
```

---

# Trigger on CDC

```apex
trigger AccountCDCTrigger
on AccountChangeEvent
(after insert){

    for(AccountChangeEvent evt :
        Trigger.New){

            System.debug(
                evt.ChangeEventHeader
                   .changeType
            );
    }
}
```

---

# Change Types

```text
CREATE
UPDATE
DELETE
UNDELETE
```

---

# CDC vs Platform Events

| Feature | CDC | Platform Events |
|----------|----------|----------|
| Auto Generated | ✅ | ❌ |
| Custom Event | ❌ | ✅ |
| Record Changes | ✅ | ❌ |
| Business Events | ❌ | ✅ |

---

# CDC Use Cases

- Data synchronization
- Real-time integrations
- Data lake updates
- ERP synchronization

---

# 3. Custom Metadata

Custom Metadata stores configuration data that can be deployed between orgs.

---

# Why Custom Metadata?

Avoid hardcoding values.

❌

```apex
String endpoint =
'https://api.erp.com';
```

---

✅

```text
Store in Custom Metadata
```

---

# Create Custom Metadata

```text
Setup
→ Custom Metadata Types
→ New Type
```

Example:

```text
ERP_Config__mdt
```

Fields:

```text
Endpoint_URL__c
API_Key__c
```

---

# Create Metadata Record

```text
Production_Config
```

---

# Query Custom Metadata

```apex
ERP_Config__mdt config =
[
    SELECT Endpoint_URL__c
    FROM ERP_Config__mdt
    LIMIT 1
];
```

---

# Get All Metadata Records

```apex
Map<String,
    ERP_Config__mdt>
configs =
ERP_Config__mdt
.getAll();
```

---

# Get Specific Record

```apex
ERP_Config__mdt config =
ERP_Config__mdt
.getInstance(
    'Production_Config'
);
```

---

# Benefits

- Deployable via Metadata API
- No DML required
- Faster access
- Environment configuration

---

# Custom Metadata Use Cases

- API endpoints
- Feature flags
- Business rules
- Country configurations
- Validation settings

---

# 4. Custom Settings

Custom Settings store application configuration data.

---

# Types of Custom Settings

| Type | Description |
|---------|---------|
| List Custom Setting | Organization-wide |
| Hierarchy Custom Setting | User/Profile-specific |

---

# List Custom Setting

Example:

```text
Country_Config__c
```

Fields:

```text
Country_Code__c
Currency__c
```

---

# Query List Custom Setting

```apex
Country_Config__c config =
Country_Config__c
.getInstance(
    'India'
);
```

---

# Hierarchy Custom Setting

Supports:

```text
Organization
Profile
User
```

levels.

---

# Example

```text
Discount_Settings__c
```

---

# Get Hierarchy Settings

```apex
Discount_Settings__c settings =
Discount_Settings__c
.getInstance();
```

---

# Custom Metadata vs Custom Settings

| Feature | Custom Metadata | Custom Settings |
|----------|----------|----------|
| Deployable | ✅ | ❌ |
| Metadata Package Support | ✅ | ❌ |
| Runtime Updates | Limited | ✅ |
| Recommended Today | ✅ | ⚠️ Legacy |

---

# Which Should You Use?

Modern Salesforce recommendation:

```text
Custom Metadata
```

unless runtime updates are required.

---

# 5. Approval Processes

Approval Processes automate record approval workflows.

---

# Approval Flow

```text
Submit Record
      ↓
Manager Approval
      ↓
Approved / Rejected
```

---

# Common Use Cases

- Expense Approvals
- Discount Approvals
- Contract Approvals
- Purchase Orders
- Loan Applications

---

# Create Approval Process

```text
Setup
→ Approval Processes
→ New
```

---

# Approval Components

| Component | Purpose |
|------------|------------|
| Entry Criteria | Determines submission |
| Approver | Reviewer |
| Approval Steps | Approval sequence |
| Final Approval Actions | Success actions |
| Final Rejection Actions | Rejection actions |

---

# Approval Example

```text
Opportunity
Amount > 100000
```

Flow:

```text
Sales Rep
     ↓
Manager Approval
     ↓
Approved
```

---

# Submit Record for Approval

```apex
Approval.ProcessSubmitRequest req =
new Approval.ProcessSubmitRequest();

req.setObjectId(
    opp.Id
);

Approval.ProcessResult result =
Approval.process(req);
```

---

# Check Approval Result

```apex
System.debug(
    result.isSuccess()
);
```

---

# Approve Request Programmatically

```apex
Approval.ProcessWorkitemRequest req =
new Approval.ProcessWorkitemRequest();

req.setComments(
    'Approved'
);

req.setAction(
    'Approve'
);
```

---

# Reject Request

```apex
req.setAction(
    'Reject'
);
```

---

# Approval Objects

| Object | Purpose |
|----------|----------|
| ProcessInstance | Approval Instance |
| ProcessInstanceStep | Approval Step |
| ProcessInstanceWorkitem | Pending Approval |

---

# Query Approval History

```apex
SELECT Id,
       Status
FROM ProcessInstance
WHERE TargetObjectId = :recordId
```

---

# Approval Status Values

```text
Pending
Approved
Rejected
Removed
```

---

# Platform Feature Comparison

| Feature | Purpose |
|----------|----------|
| Platform Events | Custom event messaging |
| CDC | Record change events |
| Custom Metadata | Deployable configuration |
| Custom Settings | Runtime configuration |
| Approval Process | Record approval workflow |

---

# Real-World Architecture

```text
Order Created
      ↓
Platform Event
      ↓
ERP Integration

Account Updated
      ↓
CDC Event
      ↓
Data Warehouse

API Endpoint
      ↓
Custom Metadata

User Preferences
      ↓
Hierarchy Custom Setting

Contract
      ↓
Approval Process
```

---

# Best Practices

## Use Platform Events

For:

```text
System-to-System Communication
```

---

## Use CDC

For:

```text
Real-Time Data Sync
```

---

## Use Custom Metadata

For:

```text
Configuration Data
```

instead of hardcoding values.

---

## Use Approval Processes

For:

```text
Business Approvals
```

instead of custom code where possible.

---

## Avoid New Custom Settings

Prefer:

```text
Custom Metadata
```

for new implementations.

---

# Interview Questions

### What is a Platform Event?

A special Salesforce event object used for asynchronous event-driven communication.

---

### Difference Between Platform Events and CDC?

| Platform Events | CDC |
|---------------|---------------|
| Custom business events | Automatic record change events |
| Published manually | Published automatically |

---

### What is Change Data Capture?

A feature that publishes events whenever records are created, updated, deleted, or undeleted.

---

### Difference Between Custom Metadata and Custom Settings?

| Custom Metadata | Custom Settings |
|---------------|---------------|
| Deployable | Not deployable |
| Preferred | Legacy |
| Metadata | Data |

---

### Why Use Custom Metadata?

To store configurable values such as API endpoints, feature flags, and business rules without hardcoding them.

---

### What is an Approval Process?

A Salesforce automation that routes records through approval and rejection workflows.

---

### How Do You Submit a Record for Approval in Apex?

```apex
Approval.ProcessSubmitRequest req =
new Approval.ProcessSubmitRequest();

req.setObjectId(recordId);

Approval.process(req);
```

---

### Which Platform Feature is Most Common in Enterprise Integrations?

```text
Platform Events
CDC
Custom Metadata
```

These three are heavily used in modern Salesforce enterprise architectures.

---

**Next Topic:** Lightning Web Components (LWC) for Apex Developers (Apex Integration, @AuraEnabled, Wire Service, Imperative Calls, LDS, LMS).