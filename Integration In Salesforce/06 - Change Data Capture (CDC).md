
## Overview

Change Data Capture (CDC) is a Salesforce event-driven feature that automatically publishes events whenever records are created, updated, deleted, or undeleted.

Unlike Platform Events, you do not create or publish CDC events manually.

```text
Record Change
      |
      v
Change Event
      |
      v
Event Bus
      |
      v
Subscribers
```

---

# Why CDC?

Without CDC:

```text
External System
      |
      v
Poll Salesforce Every Few Minutes
      |
      v
Check For Changes
```

Problems:

- Many API Calls
- Performance Overhead
- Delayed Updates

---

With CDC:

```text
Record Changes
      |
      v
Salesforce Publishes Event
      |
      v
External System Receives Event
```

Benefits:

- Real-Time Updates
- Fewer API Calls
- Event-Driven Architecture
- Automatic Change Tracking

---

# What Changes Are Captured?

CDC automatically tracks:

```text
Create
Update
Delete
Undelete
```

Example:

```text
Account Updated
      |
      v
AccountChangeEvent
```

---

# CDC Architecture

```text
Salesforce Record
        |
        v
Change Event
        |
        v
Event Bus
        |
        v
Subscribers
```

Subscribers:

```text
External Systems
LWC
Middleware
CometD Clients
Pub/Sub API
```

---

# Enabling CDC

Navigate:

```text
Setup
→ Change Data Capture
```

Select Objects:

```text
Account
Contact
Opportunity
Case
Custom Objects
```

Save.

Salesforce starts publishing events automatically.

---

# CDC Event Naming

Format:

```text
ObjectNameChangeEvent
```

Examples:

```text
AccountChangeEvent

ContactChangeEvent

OpportunityChangeEvent

CaseChangeEvent
```

Custom Object:

```text
Project__ChangeEvent
```

---

# CDC Event Structure

Example:

```json
{
  "ChangeEventHeader": {
      "changeType": "UPDATE"
  },
  "Name": "Acme Corp"
}
```

Contains:

- Record Data
- Changed Fields
- Change Metadata

---

# ChangeEventHeader

Most important CDC field.

Example:

```json
{
  "ChangeEventHeader": {
      "changeType": "UPDATE"
  }
}
```

---

# Useful Header Fields

## changeType

Type of change.

Values:

```text
CREATE
UPDATE
DELETE
UNDELETE
GAP_CREATE
```

---

## recordIds

Affected records.

Example:

```json
"recordIds": [
  "001XXXXXXXXXXXX"
]
```

---

## changedFields

Fields modified during update.

Example:

```json
"changedFields": [
  "Name",
  "Industry"
]
```

---

## entityName

Object name.

Example:

```json
"entityName":
"Account"
```

---

# CDC Event Example

Account Updated:

```text
Account:
Name = Acme Updated
```

Published Event:

```json
{
  "ChangeEventHeader": {
      "changeType":"UPDATE",
      "entityName":"Account"
  },
  "Name":"Acme Updated"
}
```

---

# CDC Subscribers

## External Systems

Most common use case.

```text
ERP
SAP
Oracle
MuleSoft
Boomi
```

---

## LWC

Using EMP API.

```javascript
import {
    subscribe
}
from 'lightning/empApi';
```

Channel:

```javascript
/data/AccountChangeEvent
```

Example:

```javascript
subscribe(
    '/data/AccountChangeEvent',
    -1,
    response => {

        console.log(response);

    }
);
```

---

## Pub/Sub API

Modern approach.

Recommended for large-scale integrations.

```text
gRPC Based
High Performance
```

---

# Replay ID

Every CDC event contains:

```text
ReplayId
```

Used to recover missed events.

---

Replay Values

| Value | Meaning |
|---------|----------|
| -1 | New Events |
| -2 | All Stored Events |
| Replay ID | Resume From Event |

---

# Event Retention

CDC events retained for:

```text
72 Hours
```

Replay available during this period.

---

# CDC vs Platform Events

| Feature | CDC | Platform Events |
|----------|----------|----------|
| Automatic | Yes | No |
| Publish Custom Events | No | Yes |
| Track Record Changes | Yes | No |
| Custom Event Schema | No | Yes |
| Business Events | No | Yes |
| Data Synchronization | Yes | No |

---

# Example Comparison

## CDC

```text
Account Updated
      |
      v
AccountChangeEvent
```

Automatic.

---

## Platform Event

```text
Order Approved
      |
      v
Order_Approved__e
```

Custom Business Event.

---

# CDC vs Outbound Messages

| CDC | Outbound Message |
|------|------|
| Event Driven | SOAP Message |
| Modern | Legacy |
| Real-Time | Near Real-Time |
| Multiple Subscribers | Single Endpoint |
| Replay Support | No Replay |

---

# CDC vs API Polling

| CDC | Polling |
|------|------|
| Real-Time | Delayed |
| Fewer API Calls | Many API Calls |
| Event Driven | Query Driven |
| Scalable | Less Efficient |

---

# Common Use Cases

## Data Synchronization

```text
Salesforce
     |
     v
CDC
     |
     v
ERP
```

---

## Data Warehouse Updates

```text
Salesforce
      |
      v
CDC
      |
      v
Snowflake
```

---

## Real-Time Customer Sync

```text
Account Updated
      |
      v
External CRM Updated
```

---

## Middleware Integrations

```text
Salesforce
      |
      v
CDC
      |
      v
MuleSoft
      |
      v
Other Systems
```

---

# Benefits

## Automatic Publishing

No Apex required.

---

## Real-Time

Changes available immediately.

---

## Reduced API Usage

No polling.

---

## Scalable

Suitable for enterprise integrations.

---

## Replay Support

Recover missed events.

---

# Limitations

## Not for Business Events

CDC tracks data changes only.

Example:

```text
Order Approved
```

Use Platform Events instead.

---

## Limited Retention

```text
72 Hours
```

---

## Event Delivery Limits

Depends on licenses and edition.

---

# Best Practices

## Use CDC for Data Synchronization

Ideal use case.

---

## Use Platform Events for Business Processes

Example:

```text
Invoice Generated

Order Approved

Shipment Delivered
```

---

## Monitor Replay IDs

Avoid missing events.

---

## Use Pub/Sub API

For modern enterprise integrations.

---

# Common Interview Questions

## What is CDC?

A Salesforce event-driven feature that automatically publishes events when records are created, updated, deleted, or undeleted.

---

## Do We Need Apex to Publish CDC Events?

```text
No
```

Salesforce publishes them automatically.

---

## What Changes Are Tracked?

```text
CREATE
UPDATE
DELETE
UNDELETE
```

---

## What is ChangeEventHeader?

Metadata describing the change event.

Contains:

```text
changeType
recordIds
changedFields
entityName
```

---

## CDC vs Platform Events?

| CDC | Platform Events |
|------|------|
| Data Changes | Business Events |
| Automatic | Manual Publish |
| Synchronization | Process Events |

---

## CDC vs Polling?

CDC is:

```text
Real-Time
Event Driven
More Efficient
```

---

## How Long Are CDC Events Retained?

```text
72 Hours
```

---

## Can LWC Subscribe to CDC Events?

```text
Yes

Using lightning/empApi
```

Channel Example:

```javascript
/data/AccountChangeEvent
```

---

## What is Replay ID?

A unique identifier used to replay missed CDC events.

---

# Interview Summary

```text
CDC
=
Automatic Record Change Events

Tracks:
- Create
- Update
- Delete
- Undelete

Enable:
Setup → Change Data Capture

Event Names:
AccountChangeEvent
ContactChangeEvent

Important Field:
ChangeEventHeader

Subscribers:
- External Systems
- LWC
- Middleware
- Pub/Sub API

Retention:
72 Hours

CDC
=
Data Synchronization

Platform Events
=
Business Events

No Apex Required
Automatic Publishing
Real-Time Updates
```