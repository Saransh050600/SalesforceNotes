
Template Directives are special HTML attributes provided by LWC that control how content is rendered in the UI.

They allow developers to:

- Show or hide content
- Render lists
- Iterate through collections
- Build dynamic user interfaces

---

# What are Template Directives?

Template directives are instructions added to HTML templates that control rendering behavior.

Example:

```html
<template lwc:if={showMessage}>
    <p>Hello World</p>
</template>
```

The content will only render when `showMessage` is true.

---

# Types of Template Directives

## Commonly Used Directives

| Directive | Purpose |
|------------|------------|
| `lwc:if` | Conditional Rendering |
| `lwc:elseif` | Alternate Condition |
| `lwc:else` | Default Condition |
| `for:each` | List Rendering |
| `iterator` | Advanced List Rendering |

---

# Conditional Rendering

Conditional Rendering allows displaying content based on a condition.

---

## lwc:if

### JavaScript

```javascript
showMessage = true;
```

### HTML

```html
<template lwc:if={showMessage}>
    <p>Welcome to Salesforce</p>
</template>
```

### Output

```text
Welcome to Salesforce
```

---

## Example

### JavaScript

```javascript
isLoggedIn = true;
```

### HTML

```html
<template lwc:if={isLoggedIn}>
    <p>User Logged In</p>
</template>
```

---

# lwc:if / lwc:elseif / lwc:else

Used for multiple conditions.

### JavaScript

```javascript
status = 'Approved';
```

### HTML

```html
<template lwc:if={isApproved}>
    Approved
</template>

<template lwc:elseif={isPending}>
    Pending
</template>

<template lwc:else>
    Rejected
</template>
```

---

## Example Using Getters

### JavaScript

```javascript
status = 'Approved';

get isApproved() {
    return this.status === 'Approved';
}

get isPending() {
    return this.status === 'Pending';
}
```

---

# Legacy Conditional Rendering

Older syntax still works but is not recommended.

### Example

```html
<template if:true={showMessage}>
    Visible
</template>
```

```html
<template if:false={showMessage}>
    Hidden
</template>
```

---

## Recommendation

Use:

```html
lwc:if
lwc:elseif
lwc:else
```

instead of:

```html
if:true
if:false
```

---

# List Rendering

List Rendering displays collections such as arrays.

---

## for:each Directive

### JavaScript

```javascript
accounts = [
    {
        Id: '1',
        Name: 'Salesforce'
    },
    {
        Id: '2',
        Name: 'Google'
    }
];
```

### HTML

```html
<template
    for:each={accounts}
    for:item="account">

    <p key={account.Id}>
        {account.Name}
    </p>

</template>
```

---

## Output

```text
Salesforce
Google
```

---

# Understanding for:each

### Syntax

```html
<template
    for:each={array}
    for:item="itemName">
```

---

## Important Rule

Every rendered item must have a unique key.

### Correct

```html
<p key={account.Id}>
```

### Incorrect

```html
<p>
```

---

## Why is key Required?

LWC uses keys to:

- Track DOM changes
- Improve rendering performance
- Minimize rerendering

---

# Rendering Simple Arrays

### JavaScript

```javascript
cities = [
    'Delhi',
    'Mumbai',
    'Bangalore'
];
```

### HTML

```html
<template
    for:each={cities}
    for:item="city">

    <p key={city}>
        {city}
    </p>

</template>
```

---

# Iterator Directive

Iterator provides additional metadata while looping.

Useful when you need to know:

- First record
- Last record
- Current value

---

## Syntax

```html
<template iterator:item={accounts}>
```

---

# Iterator Properties

| Property | Description |
|-----------|-----------|
| value | Current item |
| index | Current position |
| first | First item |
| last | Last item |

---

# Iterator Example

### JavaScript

```javascript
accounts = [
    {
        Id: '1',
        Name: 'Salesforce'
    },
    {
        Id: '2',
        Name: 'Google'
    },
    {
        Id: '3',
        Name: 'Amazon'
    }
];
```

### HTML

```html
<template iterator:acc={accounts}>

    <div key={acc.value.Id}>

        <template lwc:if={acc.first}>
            <p>First Record</p>
        </template>

        <p>{acc.value.Name}</p>

        <template lwc:if={acc.last}>
            <p>Last Record</p>
        </template>

    </div>

</template>
```

---

## Output

```text
First Record
Salesforce

Google

Amazon
Last Record
```

---

# for:each vs Iterator

| Feature | for:each | iterator |
|-----------|-----------|-----------|
| Loop Records | Yes | Yes |
| Access Current Item | Yes | Yes |
| Access Index | No | Yes |
| Access First Record | No | Yes |
| Access Last Record | No | Yes |
| Performance | Faster | Slightly More Processing |

---

## When to Use Which?

Use:

```html
for:each
```

for normal list rendering.

Use:

```html
iterator
```

when you need:

- first
- last
- index

---

# Dynamic UI Rendering

Dynamic UI Rendering means changing the user interface based on data or conditions.

---

## Example 1: Show/Hide Section

### JavaScript

```javascript
showDetails = true;
```

### HTML

```html
<template lwc:if={showDetails}>
    <p>Account Details</p>
</template>
```

---

## Example 2: User Role Based Rendering

### JavaScript

```javascript
role = 'Admin';

get isAdmin() {
    return this.role === 'Admin';
}
```

### HTML

```html
<template lwc:if={isAdmin}>
    <lightning-button
        label="Delete">
    </lightning-button>
</template>
```

---

## Example 3: Loading Spinner

### JavaScript

```javascript
isLoading = true;
```

### HTML

```html
<template lwc:if={isLoading}>
    <lightning-spinner>
    </lightning-spinner>
</template>
```

---

## Example 4: Dynamic Datatable

### JavaScript

```javascript
showTable = true;
```

### HTML

```html
<template lwc:if={showTable}>
    <lightning-datatable>
    </lightning-datatable>
</template>
```

---

# Combining Directives

Multiple directives are often used together.

### JavaScript

```javascript
showAccounts = true;

accounts = [
    {
        Id: '1',
        Name: 'Salesforce'
    }
];
```

### HTML

```html
<template lwc:if={showAccounts}>

    <template
        for:each={accounts}
        for:item="account">

        <p key={account.Id}>
            {account.Name}
        </p>

    </template>

</template>
```

---

# Best Practices

## Use lwc:if Instead of if:true

Preferred:

```html
<template lwc:if={showData}>
```

Avoid:

```html
<template if:true={showData}>
```

---

## Always Use Unique Keys

Preferred:

```html
key={record.Id}
```

Avoid:

```html
key={index}
```

---

## Use Getters for Complex Conditions

Instead of:

```html
{score > 50}
```

Use:

```javascript
get isPassed() {
    return this.score > 50;
}
```

---

# Interview Questions

## Q1: What are Template Directives?

**Answer:**

Special HTML attributes used to control rendering behavior.

---

## Q2: What directive is used for conditional rendering?

**Answer:**

```html
lwc:if
lwc:elseif
lwc:else
```

---

## Q3: What directive is used for list rendering?

**Answer:**

```html
for:each
```

---

## Q4: Why is key required in loops?

**Answer:**

To uniquely identify DOM elements and improve rendering performance.

---

## Q5: When should you use iterator instead of for:each?

**Answer:**

When access to:

- first
- last
- index

is required.

---

## Q6: Which is the modern conditional rendering syntax?

**Answer:**

```html
lwc:if
lwc:elseif
lwc:else
```

---

# Summary

## Conditional Rendering

- `lwc:if`
- `lwc:elseif`
- `lwc:else`

Used to show or hide content.

---

## List Rendering

- `for:each`
- Requires unique key

Used to display collections.

---

## Iterator

Provides:

- value
- index
- first
- last

Useful for advanced list rendering.

---

## Dynamic UI Rendering

Allows UI to change based on:

- User actions
- Permissions
- Data values
- Loading states
- Business logic

Template Directives are one of the most frequently used and most commonly asked topics in Lightning Web Component interviews because they directly control how the UI behaves and renders.