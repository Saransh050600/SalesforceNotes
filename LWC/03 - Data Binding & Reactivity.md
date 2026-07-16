
Data Binding and Reactivity are core concepts in Lightning Web Components (LWC). They allow the UI to automatically update whenever the underlying data changes.

---
# What is Data Binding?

Data Binding is the process of connecting JavaScript data to the HTML template.

When a property value changes in JavaScript, the UI automatically reflects the updated value.

## Example

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class DemoComponent extends LightningElement {
    message = 'Hello Salesforce';
}
```

### HTML

```html
<template>
    <p>{message}</p>
</template>
```

### Output

```text
Hello Salesforce
```

The value of `message` is bound to the HTML template.

---

# Property Binding

Property Binding allows JavaScript properties to be displayed in the HTML template.

## Syntax

```html
{propertyName}
```

---

## Example

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class Employee extends LightningElement {
    employeeName = 'John';
}
```

### HTML

```html
<template>
    <p>{employeeName}</p>
</template>
```

### Output

```text
John
```

---

## Multiple Property Binding

### JavaScript

```javascript
firstName = 'John';
lastName = 'Doe';
```

### HTML

```html
<template>
    <p>{firstName}</p>
    <p>{lastName}</p>
</template>
```

### Output

```text
John
Doe
```

---

## Binding with Lightning Components

```html
<lightning-input
    label="Name"
    value={employeeName}>
</lightning-input>
```

The input value is bound to the JavaScript property.

---

# Reactivity

## What is Reactivity?

Reactivity means the component automatically rerenders when reactive data changes.

LWC tracks changes in properties used in the template.

When those values change:

```text
Property Changes
        ↓
Component Rerenders
        ↓
UI Updates Automatically
```

---

## Example

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class Counter extends LightningElement {

    count = 0;

    increaseCount() {
        this.count++;
    }
}
```

### HTML

```html
<template>
    <p>{count}</p>

    <lightning-button
        label="Increase"
        onclick={increaseCount}>
    </lightning-button>
</template>
```

### Result

```text
0 → 1 → 2 → 3
```

UI updates automatically.

---

# Reactive Properties

## Primitive Data Types

Primitive values are reactive by default.

### Supported Types

```javascript
string
number
boolean
null
undefined
```

---

## Example

```javascript
name = 'John';
```

Later:

```javascript
this.name = 'David';
```

The UI automatically updates.

---

## Example

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class Demo extends LightningElement {

    companyName = 'Salesforce';

    changeCompany() {
        this.companyName = 'OpenAI';
    }
}
```

### HTML

```html
<template>
    <p>{companyName}</p>

    <lightning-button
        label="Change"
        onclick={changeCompany}>
    </lightning-button>
</template>
```

### Output

```text
Salesforce
```

After button click:

```text
OpenAI
```

---

# Reactivity with Objects

## Problem

Changing a nested property directly may not trigger rerendering.

### Example

```javascript
employee = {
    name: 'John',
    age: 25
};
```

Updating:

```javascript
this.employee.name = 'David';
```

May not rerender the UI.

---

## Recommended Solution

Create a new object.

```javascript
this.employee = {
    ...this.employee,
    name: 'David'
};
```

This triggers reactivity.

---

# Reactivity with Arrays

## Incorrect Approach

```javascript
this.accounts.push(newAccount);
```

Sometimes UI may not rerender.

---

## Recommended Approach

```javascript
this.accounts = [
    ...this.accounts,
    newAccount
];
```

Creates a new array reference.

---

## Example

### JavaScript

```javascript
accounts = ['Google', 'Amazon'];

addAccount() {

    this.accounts = [
        ...this.accounts,
        'Salesforce'
    ];
}
```

### HTML

```html
<template for:each={accounts}
          for:item="account">

    <p key={account}>
        {account}
    </p>

</template>
```

---

# @track Decorator

## Historical Concept

Before modern LWC reactivity improvements:

```javascript
@track employee;
```

was required for nested objects.

---

## Current Recommendation

Usually not needed.

Modern LWC automatically tracks most property changes.

---

## Interview Question

**Q: Is @track still required?**

**Answer:**

For most use cases, no.

It may still be useful in some complex object mutation scenarios, but generally creating a new object/array is the preferred approach.

---

# Getters

## What is a Getter?

A Getter is a computed property that dynamically returns a value.

Instead of storing data, it calculates data when requested.

---

## Syntax

```javascript
get propertyName() {

}
```

---

## Example

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class Employee extends LightningElement {

    firstName = 'John';
    lastName = 'Doe';

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}
```

### HTML

```html
<template>
    <p>{fullName}</p>
</template>
```

### Output

```text
John Doe
```

---

# Why Use Getters?

Instead of:

```javascript
fullName = this.firstName + this.lastName;
```

Use:

```javascript
get fullName() {
    return `${this.firstName} ${this.lastName}`;
}
```

The value is always up to date.

---

# Getter Example with Conditions

### JavaScript

```javascript
score = 80;

get isPassed() {
    return this.score >= 35;
}
```

### HTML

```html
<template>
    <p>{isPassed}</p>
</template>
```

### Output

```text
true
```

---

# Setters

## What is a Setter?

A Setter runs whenever a value is assigned to a property.

Useful for:

- Validation
- Data transformation
- Logging
- Preprocessing

---

## Syntax

```javascript
set propertyName(value) {

}
```

---

## Example

```javascript
_name;

set name(value) {
    this._name = value.toUpperCase();
}

get name() {
    return this._name;
}
```

---

## Usage

```javascript
this.name = 'john';
```

Stored value:

```text
JOHN
```

---

# Getter and Setter Together

### JavaScript

```javascript
import { LightningElement } from 'lwc';

export default class Employee extends LightningElement {

    _salary;

    set salary(value) {
        this._salary = value;
    }

    get salary() {
        return this._salary;
    }
}
```

---

# @api with Getter and Setter

Common interview topic.

### Example

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {

    _recordId;

    @api
    set recordId(value) {
        this._recordId = value;
    }

    get recordId() {
        return this._recordId;
    }
}
```

Used when receiving values from a parent component.

---

# Template Expressions

Template Expressions allow displaying JavaScript properties in HTML.

---

## Valid Expression

```html
<p>{accountName}</p>
```

```html
<p>{fullName}</p>
```

```html
<p>{totalRecords}</p>
```

---

## Using Getter in Template

### JavaScript

```javascript
firstName = 'John';
lastName = 'Doe';

get fullName() {
    return `${this.firstName} ${this.lastName}`;
}
```

### HTML

```html
<p>{fullName}</p>
```

---

# Unsupported Template Expressions

The following are NOT allowed directly in HTML.

### Function Calls

❌

```html
<p>{getName()}</p>
```

---

### Arithmetic Operations

❌

```html
<p>{count + 1}</p>
```

---

### Ternary Operators

❌

```html
<p>{isAdmin ? 'Admin' : 'User'}</p>
```

---

## Correct Approach

Use a Getter.

### JavaScript

```javascript
get userRole() {
    return this.isAdmin ? 'Admin' : 'User';
}
```

### HTML

```html
<p>{userRole}</p>
```

---

# Interview Questions

## Q1: What is Data Binding?

**Answer:**

The process of connecting JavaScript properties to HTML elements.

---

## Q2: What is Reactivity?

**Answer:**

The automatic rerendering of the UI when reactive properties change.

---

## Q3: Are primitive values reactive?

**Answer:**

Yes.

```javascript
name = 'John';
```

is reactive by default.

---

## Q4: How do you update an object reactively?

**Answer:**

Create a new object.

```javascript
this.employee = {
    ...this.employee,
    name: 'David'
};
```

---

## Q5: Why use Getters?

**Answer:**

To compute values dynamically instead of storing redundant data.

---

## Q6: Can we call functions directly in HTML?

**Answer:**

No.

Use a Getter instead.

---

## Q7: Is @track still commonly required?

**Answer:**

No.

Modern LWC handles most reactivity automatically.

---

# Summary

## Property Binding

- Connects JavaScript properties to HTML
- Uses `{propertyName}` syntax

## Reactive Properties

- Automatically trigger rerendering
- Primitive values are reactive by default

## Getters

- Computed properties
- Dynamic values
- Used heavily in templates

## Setters

- Execute logic when values are assigned
- Useful for validation and transformation

## Template Expressions

- Display properties and getters
- Cannot execute functions or complex expressions directly

Data Binding and Reactivity form the foundation of dynamic user interfaces in Lightning Web Components and are among the most frequently asked LWC interview topics.