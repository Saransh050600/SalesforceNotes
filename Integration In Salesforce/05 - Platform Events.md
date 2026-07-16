
## Overview

Platform Events enable event-driven communication between Salesforce and external systems.

Instead of one system directly calling another, systems communicate through events.

```text
Publisher
    |
    v
Platform Event
    |
    v
Event Bus
    |
    v
Subscribers
```

Examples:

- Order Created
- Payment Received
- Invoice Generated
- Shipment Delivered

---

# Why Platform Events?

Traditional Integration:

```text
System A
   |
   v
System B
```

Problems:

- Tight Coupling
- Difficult Maintenance
- Scalability Issues

---

Event-Driven Integration:

```text
System A
   |
   v
Platform Event
   |
   v
Multiple Subscribers
```

Benefits:

- Loose Coupling
- Scalability
- Asynchronous Processing
- Real-Time Communication

---

# Key Components

## Publisher

Creates and publishes events.

Examples:

- Apex
- Flow
- External System

---

## Event Bus

Stores and distributes events.

Managed by Salesforce.

---

## Subscriber

Consumes events.

Examples:

- Apex Trigger
- Flow
- LWC (EMP API)
- External System

---

# Platform Event Lifecycle

```text
Publisher
    |
    v
Publish Event
    |
    v
Event Bus
    |
    v
Subscriber Receives Event
    |
    v
Process Data
```

---

# Creating a Platform Event

Navigate:

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
Order_Id__c
Amount__c
Status__c
```

---

# Naming Convention

Platform Event API names end with:

```text
__e
```

Example:

```text
Order_Event__e
Payment_Event__e
Shipment_Event__e
```

---

# Publishing Platform Events

## Apex Example

```apex
Order_Event__e eventObj =
    new Order_Event__e(
        Order_Id__c = 'ORD-1001',
        Amount__c = 5000,
        Status__c = 'Created'
    );

Database.SaveResult result =
    EventBus.publish(eventObj);
```

---

# Publishing Multiple Events

```apex
List<Order_Event__e> events =
    new List<Order_Event__e>();

events.add(
    new Order_Event__e(
        Order_Id__c = 'ORD-1001'
    )
);

events.add(
    new Order_Event__e(
        Order_Id__c = 'ORD-1002'
    )
);

EventBus.publish(events);
```

---

# Publish Result

```apex
Database.SaveResult result =
    EventBus.publish(eventObj);

if(result.isSuccess()){

    System.debug('Published');

}
```

---

# Subscribing with Apex Trigger

Platform Events support triggers.

```apex
trigger OrderEventTrigger
on Order_Event__e
(after insert){

    for(
        Order_Event__e eventObj :
        Trigger.New
    ){

        System.debug(
            eventObj.Order_Id__c
        );

    }
}
```

---

# Trigger Characteristics

Platform Event Triggers:

```text
Asynchronous
After Insert Only
Batch Processing
```

Supported Event:

```text
after insert
```

Only.

---

# Platform Event Trigger Example

```apex
trigger PaymentEventTrigger
on Payment_Event__e
(after insert){

    for(
        Payment_Event__e pe :
        Trigger.New
    ){

        Account acc =
            new Account(
                Name='Payment Received'
            );

        insert acc;
    }
}
```

---

# Publishing from Flow

Flow can publish events without code.

```text
Record Triggered Flow
        |
        v
Create Platform Event
```

Common interview topic.

---

# Subscribing in Flow

Flow can subscribe to events.

```text
Platform Event Triggered Flow
```

Available:

```text
Start Element
→ Platform Event
```

---

# Subscribing in LWC

Use EMP API.

Import:

```javascript
import {
    subscribe
}
from 'lightning/empApi';
```

Subscribe:

```javascript
subscribe(
    '/event/Order_Event__e',
    -1,
    message => {

        console.log(message);

    }
);
```

---

# Replay ID

Every Platform Event has:

```text
ReplayId
```

Used for:

```text
Event Recovery
Replay Events
```

---

Replay Options

| Value | Meaning |
|---------|----------|
| -1 | New Events Only |
| -2 | All Stored Events |
| Replay ID | Resume From Specific Event |

---

# Publish Behavior

## Publish Immediately

Event is published instantly.

```text
Does not wait for transaction commit.
```

---

## Publish After Commit

Event publishes only after successful transaction commit.

Recommended for most business scenarios.

---

# Event Retention

Platform Events are retained temporarily.

Standard Platform Events:

```text
72 Hours
```

Consumers can replay during this period.

---

# Platform Events vs CDC

| Feature | Platform Events | CDC |
|----------|----------|----------|
| Custom Events | Yes | No |
| Record Changes | Optional | Automatic |
| Custom Fields | Yes | No |
| Business Events | Yes | No |
| Data Change Tracking | No | Yes |

---

# Platform Events vs Queueable

| Platform Event | Queueable |
|---------------|-----------|
| Event Driven | Job Based |
| Multiple Subscribers | Single Process |
| Decoupled | Tightly Coupled |
| Integration Friendly | Internal Processing |

---

# Event Bus Limits

Interview-level understanding:

```text
Daily Event Limits

Hourly Delivery Limits

Subscriber Limits
```

Varies by Salesforce Edition and Licenses.

---

# Error Handling

Use:

```apex
Database.SaveResult
```

Example:

```apex
Database.SaveResult result =
    EventBus.publish(eventObj);

if(!result.isSuccess()){

    System.debug(
        result.getErrors()
    );

}
```

---

# Use Cases

## ERP Integration

```text
Order Created
        |
        v
Platform Event
        |
        v
ERP System
```

---

## Payment Processing

```text
Payment Received
        |
        v
Platform Event
        |
        v
Accounting System
```

---

## Microservices

```text
Salesforce
      |
      v
Platform Event
      |
      v
Multiple Services
```

---

## Real-Time Notifications

```text
Case Created

Order Shipped

Invoice Generated
```

---

# Best Practices

## Keep Events Lightweight

Publish only required data.

---

## Use After Commit

Avoid publishing events that may roll back.

---

## Handle Failures

Check:

```apex
Database.SaveResult
```

---

## Design for Replay

Use Replay IDs when required.

---

## Use Meaningful Event Names

```text
Order_Created__e

Payment_Received__e

Shipment_Delivered__e
```

---

# Common Interview Questions

## What is a Platform Event?

A custom event message used for asynchronous, event-driven communication between Salesforce and other systems.

---

## What suffix is used for Platform Events?

```text
__e
```

---

## How do you publish a Platform Event?

```apex
EventBus.publish(eventObj);
```

---

## How do you subscribe to Platform Events?

Using:

```text
Apex Trigger

Flow

LWC EMP API

External Subscriber
```

---

## Which Trigger Event is Supported?

```text
after insert
```

Only.

---

## What is Replay ID?

A unique identifier used to replay missed events.

---

## Platform Events vs CDC?

```text
Platform Events
→ Business Events

CDC
→ Record Changes
```

---

## Are Platform Events Synchronous?

```text
No
```

They are asynchronous.

---

## Can Flow Publish Platform Events?

```text
Yes
```

---

## Can LWC Subscribe to Platform Events?

```text
Yes

Using lightning/empApi
```

---

# Interview Summary

```text
Platform Events
=
Event-Driven Architecture

Suffix:
__e

Publish:
EventBus.publish()

Subscribe:
- Apex Trigger
- Flow
- LWC EMP API
- External System

Trigger Event:
after insert

Replay ID:
Recover Missed Events

Retention:
72 Hours

Platform Events
=
Business Events

CDC
=
Record Change Events

Best Use Cases:
- ERP Integration
- Real-Time Notifications
- Microservices
- Decoupled Systems
```