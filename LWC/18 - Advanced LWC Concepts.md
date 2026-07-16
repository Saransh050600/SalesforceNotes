
Advanced LWC concepts are commonly asked in senior-level Salesforce interviews because they demonstrate deeper platform knowledge and enterprise application development skills.

Topics covered:

- Slots
- Dynamic Components
- Mixins
- Custom Labels
- Custom Permissions
- Platform Events

---

# Advanced Concepts Overview

| Concept | Purpose |
|----------|----------|
| Slots | Content Injection |
| Dynamic Components | Runtime Component Creation |
| Mixins | Reusable Functionality |
| Custom Labels | Multi-language Support |
| Custom Permissions | Permission-Based Access |
| Platform Events | Event-Driven Architecture |

---

# Slots

Slots allow parent components to pass markup into child components.

Similar to:

```html
<slot>
```

in Web Components.

---

# Why Use Slots?

Without Slots:

```text
Child controls all UI
```

With Slots:

```text
Parent injects content
```

More reusable.

---

# Child Component

### child.html

```html
<template>

    <lightning-card>

        <slot>
        </slot>

    </lightning-card>

</template>
```

---

# Parent Component

```html
<c-child>

    <p>
        Passed From Parent
    </p>

</c-child>
```

---

# Output

```text
+----------------+
| Passed From    |
| Parent         |
+----------------+
```

---

# Named Slots

Multiple content areas.

---

## Child Component

```html
<template>

    <header>

        <slot
            name="header">
        </slot>

    </header>

    <main>

        <slot>
        </slot>

    </main>

</template>
```

---

## Parent Component

```html
<c-child>

    <h1 slot="header">
        Account Header
    </h1>

    <p>
        Account Content
    </p>

</c-child>
```

---

# Dynamic Components

Allows components to be loaded at runtime.

Advanced topic.

Useful when:

- Component Type Unknown
- Metadata-Driven UI
- Dynamic Forms
- Configurable Applications

---

# Enable Dynamic Components

### meta.xml

```xml
<capability>
    lightning__dynamicComponent
</capability>
```

---

# Import Component Dynamically

```javascript
const component =
await import(
    'c/accountDetails'
);
```

---

# Example

```javascript
async loadComponent(){

    const component =
        await import(
            'c/accountDetails'
        );

    this.dynamicComponent =
        component.default;
}
```

---

# Benefits

- Reduced Initial Load
- Flexible UI
- Runtime Decisions

---

# Mixins

Mixins allow reusable functionality across multiple components.

Salesforce itself uses Mixins.

Example:

```javascript
NavigationMixin
```

---

# What is a Mixin?

A JavaScript pattern that extends functionality.

---

# Example

```javascript
import {
    NavigationMixin
}
from 'lightning/navigation';
```

---

## Usage

```javascript
export default class Demo
extends NavigationMixin(
    LightningElement
){

}
```

---

# Common Salesforce Mixins

| Mixin | Purpose |
|----------|----------|
| NavigationMixin | Navigation |
| LightningModal | Modal Support |
| RefreshView | Refresh Views |

---

# Custom Mixins

### reusableMixin.js

```javascript
export const LoggingMixin =
    (Base) =>

class extends Base {

    log(message){

        console.log(
            message
        );
    }
};
```

---

## Component Usage

```javascript
export default class Demo

extends LoggingMixin(
    LightningElement
){

}
```

---

# Custom Labels

Used for:

- Multi-language Support
- Reusable Text
- Centralized Messages

---

# Why Use Custom Labels?

Bad:

```javascript
'Account Created'
```

Hardcoded.

---

Better:

```javascript
Label.AccountCreated
```

Reusable.

---

# Create Label

```text
Setup
 ↓
Custom Labels
 ↓
New
```

---

# Import Label

```javascript
import ACCOUNT_CREATED

from '@salesforce/label/c.AccountCreated';
```

---

# Usage

```javascript
label = {

    ACCOUNT_CREATED

};
```

---

# HTML

```html
{label.ACCOUNT_CREATED}
```

---

# Example

```javascript
showToast(){

    this.dispatchEvent(

        new ShowToastEvent({

            title:
                ACCOUNT_CREATED

        })

    );
}
```

---

# Benefits

- Translation Support
- Easy Maintenance
- Centralized Text

---

# Custom Permissions

Used to control access to functionality.

More flexible than Profiles.

---

# Example Use Cases

- Show Admin Button
- Hide Features
- Restrict Actions
- Feature Toggles

---

# Create Permission

```text
Setup
 ↓
Custom Permissions
 ↓
New
```

---

# Assign Permission

```text
Permission Set
 ↓
Custom Permission
```

---

# Import Permission

```javascript
import HAS_ACCESS

from '@salesforce/customPermission/ManageAccounts';
```

---

# Example

```javascript
if(HAS_ACCESS){

    // Show Feature
}
```

---

# HTML Example

```html
<template if:true={hasAccess}>

    <lightning-button
        label="Admin Action">
    </lightning-button>

</template>
```

---

# JavaScript

```javascript
hasAccess =
    HAS_ACCESS;
```

---

# Benefits

- Better Security
- Feature Control
- Easier Administration

---

# Platform Events

Platform Events enable event-driven communication.

One of the most advanced Salesforce topics.

---

# What are Platform Events?

Custom event messages published inside Salesforce.

Used for:

- Real-Time Updates
- Integrations
- Asynchronous Processing
- Event-Driven Architectures

---

# Architecture

```text
Publisher
     ↓
Platform Event
     ↓
Subscriber
```

---

# Example

```text
Order Created
     ↓
Publish Event
     ↓
ERP System Updates
```

---

# Create Event

```text
Setup
 ↓
Platform Events
 ↓
New
```

---

# Event Example

```text
Order_Processed__e
```

---

# Publish Event

### Apex

```java
Order_Processed__e eventObj =

new Order_Processed__e(

    Order_Id__c = orderId

);

EventBus.publish(
    eventObj
);
```

---

# Subscribe Using Apex Trigger

```java
trigger OrderEventTrigger

on Order_Processed__e
(after insert){

    for(
        Order_Processed__e evt :
        Trigger.New
    ){

        System.debug(
            evt.Order_Id__c
        );
    }
}
```

---

# Subscribe in LWC

Uses:

```javascript
lightning/empApi
```

---

# Import

```javascript
import {

    subscribe

}

from 'lightning/empApi';
```

---

# Subscribe Example

```javascript
connectedCallback(){

    subscribe(

        '/event/Order_Processed__e',

        -1,

        response => {

            console.log(
                response
            );

        }

    );
}
```

---

# Use Cases

- Real-Time Notifications
- Live Dashboards
- External Integrations
- Cross-System Communication

---

# Common Interview Scenarios

## Scenario 1

Need reusable component layout.

### Solution

```html
<slot>
```

---

## Scenario 2

Need runtime component loading.

### Solution

```javascript
Dynamic Components
```

---

## Scenario 3

Need navigation support.

### Solution

```javascript
NavigationMixin
```

---

## Scenario 4

Need multilingual support.

### Solution

```javascript
Custom Labels
```

---

## Scenario 5

Need feature access based on permissions.

### Solution

```javascript
Custom Permissions
```

---

## Scenario 6

Need real-time communication.

### Solution

```javascript
Platform Events
```

---

# Interview Questions

## Q1: What are Slots in LWC?

### Answer

Slots allow parent components to inject content into child components.

---

## Q2: What are Named Slots?

### Answer

Multiple content placeholders identified using:

```html
slot="header"
```

---

## Q3: What is a Dynamic Component?

### Answer

A component loaded at runtime instead of compile time.

---

## Q4: What is a Mixin?

### Answer

A reusable JavaScript pattern that adds functionality to components.

Example:

```javascript
NavigationMixin
```

---

## Q5: Why use Custom Labels?

### Answer

- Reusability
- Translation Support
- Centralized Text Management

---

## Q6: How do you import a Custom Label?

### Answer

```javascript
import LABEL_NAME

from '@salesforce/label/c.LabelName';
```

---

## Q7: What are Custom Permissions?

### Answer

Permissions used to control feature access independently of profiles.

---

## Q8: How do you check Custom Permissions in LWC?

### Answer

```javascript
import HAS_ACCESS

from '@salesforce/customPermission/MyPermission';
```

---

## Q9: What are Platform Events?

### Answer

Salesforce event messages used for asynchronous, event-driven communication.

---

## Q10: How do you subscribe to Platform Events in LWC?

### Answer

Using:

```javascript
lightning/empApi
```

---

## Q11: Difference Between Platform Events and LMS?

| Platform Events | Lightning Message Service |
|----------------|--------------------------|
| Server Based | Client Side |
| Cross-System | Same Salesforce UI |
| Persistent | Temporary |
| Asynchronous | Real-Time UI Messaging |

---

# Summary

## Slots

```html
<slot>
```

- Content Injection
- Reusable Components
- Named Slots Supported

---

## Dynamic Components

```javascript
import()
```

- Runtime Loading
- Metadata Driven UI

---

## Mixins

```javascript
NavigationMixin
```

- Reusable Functionality
- Extends Components

---

## Custom Labels

```javascript
@salesforce/label
```

- Multi-language Support
- Centralized Text

---

## Custom Permissions

```javascript
@salesforce/customPermission
```

- Feature Control
- Security

---

## Platform Events

```javascript
lightning/empApi
```

- Event-Driven Architecture
- Real-Time Updates
- System Integrations

Advanced LWC concepts are frequently discussed in senior Salesforce interviews because they demonstrate knowledge of scalable, reusable, enterprise-grade application design patterns.