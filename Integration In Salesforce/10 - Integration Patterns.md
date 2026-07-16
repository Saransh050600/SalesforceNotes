
## Overview

Integration Patterns are standard approaches used to connect Salesforce with external systems.

Choosing the correct pattern is one of the most common Salesforce integration interview topics.

---

# Why Integration Patterns Matter?

Different business requirements need different integration approaches.

Example:

```text
Get Customer Credit Score
→ Real-Time

Load 5 Million Records
→ Batch

Notify ERP About New Order
→ Event Driven
```

Using the wrong pattern can cause:

- Performance Issues
- API Limits
- Data Inconsistencies
- Scalability Problems

---

# Salesforce Integration Pattern Categories

```text
1. Remote Process Invocation (Request-Reply)
2. Remote Process Invocation (Fire-and-Forget)
3. Batch Data Synchronization
4. Remote Call-In
5. UI Update Based on Data Changes
6. Data Virtualization
7. Event-Driven Architecture
```

---

# 1. Remote Process Invocation (Request-Reply)

## Definition

Salesforce sends a request and waits for an immediate response.

```text
Salesforce
     |
     | Request
     v
External System
     |
     | Response
     v
Salesforce
```

---

## Examples

```text
Credit Check

Tax Calculation

Address Validation

Payment Verification
```

---

## Implementation

```text
Apex Callout

REST API

SOAP API
```

---

## Example

User clicks:

```text
Calculate Tax
```

Salesforce:

```text
Call External Tax Service
Receive Tax Amount
Display Result
```

Immediately.

---

## Characteristics

| Feature | Value |
|----------|----------|
| Real-Time | Yes |
| Response Required | Yes |
| Synchronous | Yes |

---

## Interview Question

When would you use Request-Reply?

```text
When Salesforce needs an immediate response.
```

---

# 2. Remote Process Invocation (Fire-and-Forget)

## Definition

Salesforce sends a request but does not wait for a response.

```text
Salesforce
     |
     v
External System

No Response Needed
```

---

## Examples

```text
Send Notification

Create ERP Order

Audit Logging

Email Service
```

---

## Implementation

```text
Platform Events

Outbound Messages

Queueable Callouts
```

---

## Example

Order Created:

```text
Salesforce
      |
      v
ERP System

Continue Processing
```

No response expected.

---

## Characteristics

| Feature | Value |
|----------|----------|
| Real-Time | Yes |
| Response Required | No |
| Asynchronous | Yes |

---

## Interview Question

When would you use Fire-and-Forget?

```text
When confirmation is not immediately required.
```

---

# 3. Batch Data Synchronization

## Definition

Large volumes of data exchanged at scheduled intervals.

```text
Salesforce
      |
      v
Batch Process
      |
      v
External System
```

---

## Examples

```text
Nightly Customer Sync

Product Catalog Updates

ERP Data Loads

Data Warehouse Updates
```

---

## Implementation

```text
Bulk API

Batch Apex

Middleware
```

---

## Example

Every night:

```text
10 PM

100,000 Customers
     |
     v
Salesforce
```

---

## Characteristics

| Feature | Value |
|----------|----------|
| Real-Time | No |
| Scheduled | Yes |
| High Volume | Yes |

---

## Interview Question

When would you use Batch Synchronization?

```text
Large data transfers that do not require real-time processing.
```

---

# 4. Remote Call-In

## Definition

External systems call Salesforce APIs.

```text
External System
      |
      v
Salesforce
```

---

## Examples

```text
ERP Updates Salesforce

Website Creates Leads

Mobile App Creates Cases
```

---

## Implementation

```text
REST API

SOAP API

Bulk API

Composite API
```

---

## Example

Website:

```text
Customer Registers
      |
      v
Lead Created In Salesforce
```

---

## Characteristics

| Feature | Value |
|----------|----------|
| Direction | External → Salesforce |
| API Driven | Yes |

---

# 5. UI Update Based on Data Changes

## Definition

User interface updates when data changes.

```text
Record Updated
      |
      v
UI Refresh
```

---

## Examples

```text
Live Dashboard

Service Console Updates

Case Monitoring
```

---

## Implementation

```text
Change Data Capture

Platform Events

Streaming API

EMP API
```

---

## Example

Case Status Changed:

```text
Open
     ↓
Closed
```

Agent screen updates automatically.

---

## Characteristics

| Feature | Value |
|----------|----------|
| Real-Time | Yes |
| Event Driven | Yes |

---

# 6. Data Virtualization

## Definition

Access external data without storing it in Salesforce.

```text
Salesforce
      |
      v
External Database
```

---

## Examples

```text
ERP Customer Data

Financial Data

Legacy Systems
```

---

## Implementation

```text
Salesforce Connect

External Objects

OData
```

---

## Example

```text
Customer__x
```

Data remains in ERP.

---

## Characteristics

| Feature | Value |
|----------|----------|
| Data Stored In Salesforce | No |
| Real-Time Access | Yes |

---

## Interview Question

Which pattern uses Salesforce Connect?

```text
Data Virtualization
```

---

# 7. Event-Driven Architecture

## Definition

Systems communicate using events instead of direct calls.

```text
Publisher
     |
     v
Event Bus
     |
     v
Subscribers
```

---

## Examples

```text
Order Created

Payment Received

Shipment Delivered
```

---

## Implementation

```text
Platform Events

CDC

Pub/Sub API
```

---

## Example

```text
Order Created
      |
      v
Platform Event
      |
      v
ERP
Warehouse
Billing System
```

---

## Characteristics

| Feature | Value |
|----------|----------|
| Loosely Coupled | Yes |
| Asynchronous | Yes |
| Scalable | Yes |

---

## Interview Question

When would you use Event-Driven Architecture?

```text
When multiple systems need to react to the same event.
```

---

# Pattern Selection Guide

| Requirement | Pattern |
|------------|----------|
| Need Immediate Response | Request-Reply |
| No Response Needed | Fire-and-Forget |
| Large Scheduled Data Load | Batch Synchronization |
| External System Updates Salesforce | Remote Call-In |
| Real-Time Screen Updates | UI Update Based on Data Changes |
| Access External Data | Data Virtualization |
| Publish Business Events | Event-Driven Architecture |

---

# Most Common Salesforce Mapping

| Requirement | Technology |
|-------------|------------|
| Real-Time Validation | Apex Callout |
| ERP Notification | Platform Event |
| Large Data Migration | Bulk API |
| Website Creates Lead | REST API |
| Live UI Updates | CDC |
| External Data Access | Salesforce Connect |
| Business Event Processing | Platform Events |

---

# Real Interview Scenarios

## Scenario 1

```text
User enters Address
Need Validation Immediately
```

Answer:

```text
Request-Reply
(Apex Callout)
```

---

## Scenario 2

```text
Order Created
Notify ERP
No Immediate Response Needed
```

Answer:

```text
Fire-and-Forget
(Platform Event)
```

---

## Scenario 3

```text
Load 10 Million Products
Every Night
```

Answer:

```text
Batch Data Synchronization
(Bulk API)
```

---

## Scenario 4

```text
ERP Customers Should Be Visible
Without Storing Data
```

Answer:

```text
Data Virtualization
(Salesforce Connect)
```

---

## Scenario 5

```text
Order Created

ERP
Billing
Warehouse

All Need Notification
```

Answer:

```text
Event-Driven Architecture
(Platform Events)
```

---

# Common Interview Questions

## What are Salesforce Integration Patterns?

Standard approaches for integrating Salesforce with external systems.

---

## Which Pattern Requires Immediate Response?

```text
Remote Process Invocation
(Request-Reply)
```

---

## Which Pattern Is Best For Large Data Loads?

```text
Batch Data Synchronization
```

---

## Which Pattern Uses Salesforce Connect?

```text
Data Virtualization
```

---

## Which Pattern Uses Platform Events?

```text
Event-Driven Architecture
```

---

## Difference Between Request-Reply and Fire-and-Forget?

| Request-Reply | Fire-and-Forget |
|--------------|----------------|
| Waits For Response | No Response |
| Synchronous | Asynchronous |
| Immediate Result | Background Processing |

---

# Mid-Level Interview Focus

### Must Know

```text
Request-Reply

Fire-and-Forget

Batch Synchronization

Remote Call-In

Data Virtualization

Event-Driven Architecture

Platform Events

CDC

Bulk API

Salesforce Connect
```

---

# Interview Summary

```text
Request-Reply
→ Immediate Response
→ Apex Callout

Fire-and-Forget
→ No Response
→ Platform Event

Batch Synchronization
→ Large Data Loads
→ Bulk API

Remote Call-In
→ External → Salesforce
→ REST API

Data Virtualization
→ Salesforce Connect

UI Update
→ CDC

Event-Driven Architecture
→ Platform Events
→ CDC
→ Pub/Sub API

Most Asked:

Which pattern would you choose and why?
```