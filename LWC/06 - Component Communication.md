
Component Communication is the process of sharing data and events between Lightning Web Components.

There are multiple communication patterns in LWC depending on the relationship between components.

## Types of Component Communication

| Communication Type | Technique |
|------------|------------|
| Parent → Child | `@api` Property / Method |
| Child → Parent | Custom Events |
| Sibling Components | Lightning Message Service (LMS) |
| Unrelated Components | Lightning Message Service (LMS) |

---

# Parent → Child Communication

Parent components pass data to child components using public properties exposed with `@api`.

---

## How It Works

```text
Parent Component
        ↓
Passes Property
        ↓
Child Component
```

---

## Child Component

### childComponent.js

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {

    @api accountName;
}
```

---

### childComponent.html

```html
<template>
    <p>{accountName}</p>
</template>
```

---

## Parent Component

### parentComponent.js

```javascript
import { LightningElement } from 'lwc';

export default class ParentComponent extends LightningElement {

    accountName = 'Salesforce';
}
```

---

### parentComponent.html

```html
<template>

    <c-child-component
        account-name={accountName}>
    </c-child-component>

</template>
```

---

## Output

```text
Salesforce
```

---

# Passing Multiple Properties

### Child

```javascript
@api accountName;
@api accountType;
```

### Parent

```html
<c-child-component
    account-name={accountName}
    account-type={accountType}>
</c-child-component>
```

---

# Parent Calling Child Methods

A parent can invoke methods inside a child component using `@api`.

---

## Child Component

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {

    @api
    refreshData() {
        console.log('Refreshing Data');
    }
}
```

---

## Parent Component

```javascript
handleRefresh() {

    const child =
        this.template.querySelector(
            'c-child-component'
        );

    child.refreshData();
}
```

---

# Parent → Child Summary

### Uses

- Pass data
- Pass configuration
- Call child methods

### Technologies

```javascript
@api
querySelector()
```

---

# Child → Parent Communication

Children communicate with parents using Custom Events.

---

## How It Works

```text
Child Component
        ↓
Dispatch Event
        ↓
Parent Component
        ↓
Handle Event
```

---

# Custom Events

A custom event is created and dispatched by the child.

---

## Child Component

### childComponent.js

```javascript
import { LightningElement } from 'lwc';

export default class ChildComponent extends LightningElement {

    handleClick() {

        const event =
            new CustomEvent(
                'save'
            );

        this.dispatchEvent(event);
    }
}
```

---

### childComponent.html

```html
<template>

    <lightning-button
        label="Save"
        onclick={handleClick}>
    </lightning-button>

</template>
```

---

# Parent Component

### parentComponent.html

```html
<c-child-component
    onsave={handleSave}>
</c-child-component>
```

---

### parentComponent.js

```javascript
handleSave() {

    console.log('Event Received');
}
```

---

# Passing Data Through Events

Child components can pass data to parents using the `detail` property.

---

## Child Component

```javascript
handleClick() {

    const event =
        new CustomEvent(
            'save',
            {
                detail: {
                    accountId: '001XXXX'
                }
            }
        );

    this.dispatchEvent(event);
}
```

---

## Parent Component

```javascript
handleSave(event) {

    const accountId =
        event.detail.accountId;

    console.log(accountId);
}
```

---

# Event Object Structure

```javascript
event.detail
```

Contains custom data.

---

## Example

```javascript
event.detail.accountId
```

```javascript
event.detail.recordName
```

```javascript
event.detail.status
```

---

# Child → Parent Summary

### Uses

- Notify parent
- Send data
- Trigger actions

### Technology

```javascript
CustomEvent
dispatchEvent()
```

---

# Sibling Communication

Sibling components cannot communicate directly.

Example:

```text
Parent
├── Child A
└── Child B
```

Child A cannot directly access Child B.

---

## Recommended Solution

Use:

```text
Lightning Message Service (LMS)
```

or

```text
Parent Mediation Pattern
```

---

# Parent Mediation Pattern

```text
Child A
   ↓
Parent
   ↓
Child B
```

---

## Flow

1. Child A sends event
2. Parent receives event
3. Parent updates data
4. Parent passes data to Child B

---

### Example

```text
Child A → Custom Event
Parent → Update Variable
Parent → @api Property
Child B → Receives Data
```

---

# Lightning Message Service (LMS)

![[21 - Lightning Message Service (LMS)]]
# LMS Lifecycle Best Practice

Subscribe:

```javascript
connectedCallback()
```

Unsubscribe:

```javascript
disconnectedCallback()
```

---

## Unsubscribe Example

```javascript
import {
    unsubscribe
}
from 'lightning/messageService';
```

```javascript
disconnectedCallback() {

    unsubscribe(
        this.subscription
    );
}
```

---

# LMS Methods

| Method | Purpose |
|----------|----------|
| publish() | Send Message |
| subscribe() | Receive Message |
| unsubscribe() | Stop Listening |
| MessageContext | LMS Context |

---

# Communication Comparison

| Type | Technology |
|----------|----------|
| Parent → Child | @api |
| Parent → Child Method | @api Method |
| Child → Parent | Custom Event |
| Sibling Components | LMS |
| Unrelated Components | LMS |

---

# Real Interview Scenarios

## Scenario 1

Pass Account Id from Parent to Child.

### Solution

```javascript
@api accountId;
```

---

## Scenario 2

Notify Parent after Save.

### Solution

```javascript
CustomEvent
```

---

## Scenario 3

Pass Data Between Two Independent Components.

### Solution

```javascript
LMS
```

---

## Scenario 4

Parent Calls Child Validation Method.

### Solution

```javascript
@api validate()
```

---

# Interview Questions

## Q1: How does Parent communicate with Child?

**Answer**

Using:

```javascript
@api
```

---

## Q2: How does Child communicate with Parent?

**Answer**

Using:

```javascript
CustomEvent
```

---

## Q3: What is event.detail?

**Answer**

Contains data sent by the child.

```javascript
event.detail
```

---

## Q4: Can sibling components communicate directly?

**Answer**

No.

Use:

```text
LMS
```

or

```text
Parent Mediation
```

---

## Q5: What is LMS?

**Answer**

Lightning Message Service is Salesforce's messaging framework that enables communication between independent components.

---

## Q6: What are the main LMS methods?

**Answer**

```javascript
publish()
subscribe()
unsubscribe()
```

---

## Q7: When should LMS be used?

**Answer**

When components are unrelated or not in a direct parent-child hierarchy.

---

# Summary

## Parent → Child

- `@api` Properties
- `@api` Methods

---

## Child → Parent

- Custom Events
- `dispatchEvent()`
- `event.detail`

---

## Sibling Components

- Parent Mediation Pattern
- LMS

---

## Lightning Message Service

- Publisher
- Subscriber
- Message Channel
- `publish()`
- `subscribe()`
- `unsubscribe()`

Component Communication is one of the most frequently asked LWC interview topics because almost every real-world application requires components to exchange data and events efficiently.