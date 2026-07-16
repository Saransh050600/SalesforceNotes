
Decorators in Lightning Web Components (LWC) are special annotations that add functionality to properties and methods.

LWC provides three primary decorators:

- `@api`
- `@wire`
- `@track`

Decorators help control:

- Component communication
- Data retrieval
- Reactivity

---

# What is a Decorator?

A decorator is a special construct that modifies the behavior of a property or method.

Example:

```javascript
@api recordId;
```

Here, `@api` makes the property publicly accessible.

---

# @api Decorator

The `@api` decorator exposes a property or method to parent components.

It is used when:

- Parent passes data to child
- Parent calls child methods
- Component is configured from App Builder
- Component is used in Flow

---

# Public Properties

## Child Component

### childComponent.js

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {

    @api accountName;
}
```

### childComponent.html

```html
<template>
    <p>{accountName}</p>
</template>
```

---

## Parent Component

### parentComponent.html

```html
<c-child-component
    account-name="Salesforce">
</c-child-component>
```

### Output

```text
Salesforce
```

---

# Passing Dynamic Values

### Parent JS

```javascript
accountName = 'Google';
```

### Parent HTML

```html
<c-child-component
    account-name={accountName}>
</c-child-component>
```

---

# Public Methods

Methods can also be exposed using `@api`.

---

## Child Component

### childComponent.js

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {

    @api
    showMessage() {
        alert('Hello from Child');
    }
}
```

---

## Parent Component

### parentComponent.js

```javascript
handleClick() {

    const child =
        this.template.querySelector(
            'c-child-component'
        );

    child.showMessage();
}
```

---

# Common Use Cases of @api

- Parent → Child Communication
- Exposing methods
- Record Id on Record Pages
- Flow Variables
- App Builder Properties

---

# @api with Getters and Setters

Used when validation or transformation is needed.

```javascript
import { LightningElement, api } from 'lwc';

export default class Demo extends LightningElement {

    _name;

    @api
    set name(value) {
        this._name = value.toUpperCase();
    }

    get name() {
        return this._name;
    }
}
```

---

# @wire Decorator

The `@wire` decorator is used to retrieve Salesforce data reactively.

It connects a component to:

- Apex Methods
- LDS Adapters
- UI APIs

---

# Why Use @wire?

Benefits:

- Reactive
- Automatic refresh
- Built-in caching
- Less code

---

# Wire Apex Method

### Apex

```apex
public with sharing class AccountController {

    @AuraEnabled(cacheable=true)
    public static List<Account> getAccounts() {
        return [
            SELECT Id, Name
            FROM Account
        ];
    }
}
```

---

### LWC

```javascript
import { LightningElement, wire }
from 'lwc';

import getAccounts
from '@salesforce/apex/AccountController.getAccounts';

export default class Demo
extends LightningElement {

    @wire(getAccounts)
    accounts;
}
```

---

# Property Wire

Stores result directly.

```javascript
@wire(getAccounts)
accounts;
```

Access data:

```javascript
this.accounts.data
```

Access errors:

```javascript
this.accounts.error
```

---

# Function Wire

Provides more control.

```javascript
@wire(getAccounts)
wiredAccounts({ data, error }) {

    if(data) {
        this.accounts = data;
    }

    if(error) {
        console.error(error);
    }
}
```

---

# Reactive Parameters

Wire automatically refreshes when parameter changes.

---

### Apex

```apex
@AuraEnabled(cacheable=true)
public static Account getAccount(Id accountId)
```

---

### LWC

```javascript
@api recordId;

@wire(getAccount,
{
    accountId: '$recordId'
})
account;
```

---

## Important

Reactive variables use:

```javascript
'$recordId'
```

not:

```javascript
recordId
```

---

# Wire Adapters

Commonly used adapters:

---

## getRecord

```javascript
import { getRecord }
from 'lightning/uiRecordApi';
```

```javascript
@wire(getRecord, {
    recordId: '$recordId',
    fields: FIELDS
})
account;
```

---

## getObjectInfo

```javascript
import { getObjectInfo }
from 'lightning/uiObjectInfoApi';
```

---

## getPicklistValues

```javascript
import { getPicklistValues }
from 'lightning/uiObjectInfoApi';
```

---

# refreshApex()

Used to refresh wire data.

```javascript
import { refreshApex }
from '@salesforce/apex';
```

```javascript
refreshApex(this.wiredResult);
```

---

# @track Decorator

The `@track` decorator was originally introduced to make object and array changes reactive.

---

# Historical Usage

Before reactivity enhancements:

```javascript
@track employee = {
    name: 'John'
};
```

Updating:

```javascript
this.employee.name = 'David';
```

would rerender the component.

---

# Current LWC Behavior

Most use cases no longer require `@track`.

Modern LWC automatically tracks:

- Primitive properties
- Object references
- Array references

---

# Recommended Pattern

Instead of:

```javascript
this.employee.name = 'David';
```

Use:

```javascript
this.employee = {
    ...this.employee,
    name: 'David'
};
```

---

# Array Example

Instead of:

```javascript
this.accounts.push(account);
```

Use:

```javascript
this.accounts = [
    ...this.accounts,
    account
];
```

---

# Should We Still Use @track?

### Interview Answer

For most scenarios:

```text
No
```

Modern LWC handles reactivity automatically.

However, you should understand it because it is still frequently asked in interviews.

---

# Public Methods

Public methods are methods exposed using `@api`.

They allow a parent component to invoke functionality inside a child component.

---

# Example

## Child Component

```javascript
import { LightningElement, api }
from 'lwc';

export default class ChildComponent
extends LightningElement {

    @api
    refreshData() {

        console.log('Refreshing...');
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

# Public Properties vs Public Methods

| Feature | Public Property | Public Method |
|----------|----------|----------|
| Receives Data | Yes | No |
| Executes Logic | No | Yes |
| Uses @api | Yes | Yes |
| Parent Access | Yes | Yes |

---

# Decorator Comparison

| Decorator | Purpose |
|------------|------------|
| @api | Expose property/method |
| @wire | Retrieve reactive data |
| @track | Legacy object tracking |

---

# Real Interview Scenario

### Parent to Child

```javascript
@api accountId;
```

Parent passes data.

---

### Parent Calls Child

```javascript
@api refreshData() {

}
```

Parent executes logic.

---

### Retrieve Data

```javascript
@wire(getAccounts)
accounts;
```

Wire fetches data.

---

### Reactivity

```javascript
this.accounts = [
    ...this.accounts,
    newAccount
];
```

Modern alternative to `@track`.

---

# Interview Questions

## Q1: What are the decorators available in LWC?

**Answer**

- `@api`
- `@wire`
- `@track`

---

## Q2: What is @api used for?

**Answer**

To expose properties and methods to parent components.

---

## Q3: Can @api be used on methods?

**Answer**

Yes.

```javascript
@api
refreshData() {

}
```

---

## Q4: What is @wire used for?

**Answer**

To retrieve Salesforce data reactively.

---

## Q5: Difference between Property Wire and Function Wire?

| Property Wire | Function Wire |
|---------------|---------------|
| Simpler | More control |
| Auto stores data | Custom processing |
| Less code | More flexible |

---

## Q6: What is a Reactive Parameter?

**Answer**

A parameter prefixed with `$`.

```javascript
'$recordId'
```

When value changes, wire executes again.

---

## Q7: Is @track still required?

**Answer**

Generally no.

Modern LWC reactivity handles most scenarios automatically.

---

## Q8: How does a parent call a child method?

**Answer**

Using:

```javascript
this.template.querySelector()
```

and an `@api` method.

---

# Summary

## @api

- Public properties
- Public methods
- Parent → Child communication
- Flow/App Builder integration

## @wire

- Reactive data retrieval
- Apex integration
- LDS integration
- UI API integration
- Supports caching

## @track

- Legacy reactivity decorator
- Rarely required today
- Still important for interviews

## Public Methods

- Exposed using `@api`
- Parent can invoke child logic
- Used for refresh, validation, reset, and custom actions

Decorators are among the most important LWC interview topics because they control component communication, data access, and reactivity.