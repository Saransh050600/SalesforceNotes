
Lightning Base Components are prebuilt Salesforce UI components that help developers build applications quickly without creating everything from scratch.

They:

- Follow Salesforce Lightning Design System (SLDS)
- Are responsive
- Support accessibility
- Require minimal custom code
- Are optimized by Salesforce

---

# What are Lightning Base Components?

Instead of writing:

```html
<input>
<button>
<select>
```

LWC provides:

```html
<lightning-input>
<lightning-button>
<lightning-combobox>
```

These components automatically follow Salesforce UI standards.

---

# Commonly Used Base Components

| Component | Purpose |
|------------|------------|
| lightning-input | User Input |
| lightning-combobox | Dropdown |
| lightning-button | Buttons |
| lightning-card | Card Layout |
| lightning-modal | Modal Popup |
| lightning-datatable | Data Tables |
| lightning-tree-grid | Hierarchical Data |

---

# lightning-input

Used to capture user input.

---

## Basic Example

```html
<lightning-input
    label="Name">
</lightning-input>
```

---

## Output

```text
Name
[____________]
```

---

## Input Types

### Text

```html
<lightning-input
    type="text"
    label="Name">
</lightning-input>
```

### Number

```html
<lightning-input
    type="number"
    label="Age">
</lightning-input>
```

### Email

```html
<lightning-input
    type="email"
    label="Email">
</lightning-input>
```

### Password

```html
<lightning-input
    type="password"
    label="Password">
</lightning-input>
```

### Date

```html
<lightning-input
    type="date"
    label="Date">
</lightning-input>
```

### Checkbox

```html
<lightning-input
    type="checkbox"
    label="Active">
</lightning-input>
```

---

## Reading Input Value

### HTML

```html
<lightning-input
    label="Name"
    onchange={handleChange}>
</lightning-input>
```

### JavaScript

```javascript
handleChange(event) {
    this.name = event.target.value;
}
```

---

## Required Field

```html
<lightning-input
    required
    label="Name">
</lightning-input>
```

---

## Validation

```javascript
const input =
    this.template.querySelector(
        'lightning-input'
    );

input.reportValidity();
```

---

# lightning-combobox

Used to display a dropdown list.

---

## Basic Example

### HTML

```html
<lightning-combobox
    label="Country"
    options={countryOptions}
    value={selectedCountry}
    onchange={handleChange}>
</lightning-combobox>
```

### JavaScript

```javascript
countryOptions = [
    {
        label: 'India',
        value: 'India'
    },
    {
        label: 'USA',
        value: 'USA'
    }
];
```

---

## Reading Selected Value

```javascript
handleChange(event) {
    this.selectedCountry =
        event.detail.value;
}
```

---

## Combobox Option Structure

```javascript
{
    label: 'India',
    value: 'India'
}
```

---

## Dynamic Combobox

Often populated from:

- Apex
- UI API
- Picklists

### Example

```javascript
@wire(getPicklistValues)
picklistValues;
```

---

# lightning-card

Used to group content visually.

One of the most commonly used components.

---

## Basic Example

```html
<lightning-card
    title="Account Information">

    <p>
        Account Details
    </p>

</lightning-card>
```

---

## Card with Icon

```html
<lightning-card
    title="Accounts"
    icon-name="standard:account">

</lightning-card>
```

---

## Card Slots

### Title

```html
title="Accounts"
```

### Icon

```html
icon-name="standard:account"
```

### Actions

```html
slot="actions"
```

---

## Example

```html
<lightning-card
    title="Accounts">

    <lightning-button
        slot="actions"
        label="Refresh">
    </lightning-button>

</lightning-card>
```

---

# lightning-button

Used for user actions.

---

## Basic Example

```html
<lightning-button
    label="Save">
</lightning-button>
```

---

## Click Event

```html
<lightning-button
    label="Save"
    onclick={handleSave}>
</lightning-button>
```

---

## Button Variants

### Neutral

```html
variant="neutral"
```

Default style.

### Brand

```html
variant="brand"
```

Primary Salesforce button.

### Destructive

```html
variant="destructive"
```

Used for delete actions.

### Success

```html
variant="success"
```

Used for successful actions.

---

## Example

```html
<lightning-button
    label="Save"
    variant="brand">
</lightning-button>
```

---

## Disable Button

```html
<lightning-button
    disabled>
</lightning-button>
```

---

## Icon Button

```html
<lightning-button-icon
    icon-name="utility:edit">
</lightning-button-icon>
```

---

# lightning-modal

Used for popup dialogs.

Introduced as a standard modal framework.

---

## Why Use Modal?

Common use cases:

- Create Record
- Edit Record
- Confirmation Dialog
- Warning Messages

---

## Create Modal Component

### modalComponent.js

```javascript
import LightningModal
from 'lightning/modal';

export default class DemoModal
extends LightningModal {

}
```

---

## Modal HTML

```html
<template>

    Modal Content

</template>
```

---

## Open Modal

```javascript
import DemoModal
from 'c/demoModal';

async openModal() {

    await DemoModal.open({
        size: 'small'
    });
}
```

---

## Modal Sizes

```javascript
small
medium
large
full
```

---

## Passing Data to Modal

```javascript
await DemoModal.open({
    accountId: this.recordId
});
```

---

## Returning Data from Modal

```javascript
this.close({
    status: 'Success'
});
```

---

## Parent Receives

```javascript
const result =
    await DemoModal.open();
```

---

# lightning-datatable

One of the most important Lightning Base Components.

Used to display tabular data.

---

## Basic Example

### HTML

```html
<lightning-datatable
    key-field="Id"
    data={accounts}
    columns={columns}>
</lightning-datatable>
```

### JavaScript

```javascript
columns = [
    {
        label: 'Name',
        fieldName: 'Name'
    },
    {
        label: 'Industry',
        fieldName: 'Industry'
    }
];
```

---

## Data Example

```javascript
accounts = [
    {
        Id: '1',
        Name: 'Salesforce',
        Industry: 'Technology'
    }
];
```

---

## Datatable Column Types

### Text

```javascript
type: 'text'
```

### Number

```javascript
type: 'number'
```

### Currency

```javascript
type: 'currency'
```

### Date

```javascript
type: 'date'
```

### Boolean

```javascript
type: 'boolean'
```

### URL

```javascript
type: 'url'
```

---

## Sorting

```html
onsort={handleSort}
```

### Example Column

```javascript
{
    label: 'Name',
    fieldName: 'Name',
    sortable: true
}
```

---

## Row Actions

### Column

```javascript
{
    type: 'action',
    typeAttributes: {
        rowActions: actions
    }
}
```

### Actions

```javascript
actions = [
    {
        label: 'Edit',
        name: 'edit'
    },
    {
        label: 'Delete',
        name: 'delete'
    }
];
```

---

## Handle Row Action

```javascript
handleRowAction(event) {

    const actionName =
        event.detail.action.name;

    const row =
        event.detail.row;
}
```

---

## Inline Editing

### Column

```javascript
{
    label: 'Name',
    fieldName: 'Name',
    editable: true
}
```

### Datatable

```html
<lightning-datatable
    draft-values={draftValues}
    onsave={handleSave}>
</lightning-datatable>
```

---

## Selected Rows

### HTML

```html
onrowselection={handleSelection}
```

### JavaScript

```javascript
handleSelection(event) {

    const rows =
        event.detail.selectedRows;
}
```

---

## Common Datatable Features

- Sorting
- Pagination
- Row Actions
- Inline Editing
- Search
- Selection
- Refresh

---

# lightning-tree-grid

Used to display hierarchical parent-child data.

Common examples:

- Accounts → Contacts
- Accounts → Opportunities
- Territories → Accounts
- Parent Records → Child Records

---

## Basic Example

### HTML

```html
<lightning-tree-grid
    columns={columns}
    data={gridData}
    key-field="Id">
</lightning-tree-grid>
```

---

## Columns

```javascript
columns = [
    {
        label: 'Name',
        fieldName: 'Name',
        type: 'text'
    },
    {
        label: 'Type',
        fieldName: 'Type',
        type: 'text'
    }
];
```

---

## Data Structure

Tree Grid requires child records inside:

```javascript
_children
```

### Example

```javascript
gridData = [
    {
        Id: '1',
        Name: 'Account A',
        Type: 'Customer',

        _children: [
            {
                Id: '11',
                Name: 'Contact 1',
                Type: 'Primary Contact'
            },
            {
                Id: '12',
                Name: 'Contact 2',
                Type: 'Secondary Contact'
            }
        ]
    }
];
```

---

## Expand All Rows

```javascript
this.template
    .querySelector('lightning-tree-grid')
    .expandAll();
```

---

## Collapse All Rows

```javascript
this.template
    .querySelector('lightning-tree-grid')
    .collapseAll();
```

---

## Tree Grid Features

- Hierarchical display
- Expand/Collapse rows
- Parent-child relationship visualization
- Row selection
- Custom columns
- Sorting support

---

## Tree Grid Limitations

- No inline editing
- More complex data structure
- Requires `_children` property

---

## Tree Grid vs Datatable

| Feature | Datatable | Tree Grid |
|----------|------------|------------|
| Flat Data | ✅ | ❌ |
| Parent-Child Data | ❌ | ✅ |
| Expand/Collapse | ❌ | ✅ |
| Inline Editing | ✅ | ❌ |
| Row Actions | ✅ | ✅ |
| Sorting | ✅ | Limited |

---

## Real Interview Scenario

### Scenario

Display Accounts and their Contacts in a single component.

### Solution

```html
<lightning-tree-grid>
```

because parent-child data is required.

---

# Component Comparison

| Component | Purpose |
|------------|------------|
| lightning-input | User Input |
| lightning-combobox | Dropdown |
| lightning-card | Layout Container |
| lightning-button | User Actions |
| lightning-modal | Popup Dialog |
| lightning-datatable | Flat Table/Grid |
| lightning-tree-grid | Hierarchical Grid |

---

# Real Interview Scenarios

## Scenario 1

Need user to enter email.

### Solution

```html
<lightning-input
    type="email">
</lightning-input>
```

---

## Scenario 2

Need country dropdown.

### Solution

```html
<lightning-combobox>
```

---

## Scenario 3

Need Salesforce-style container.

### Solution

```html
<lightning-card>
```

---

## Scenario 4

Need confirmation popup.

### Solution

```html
<lightning-modal>
```

---

## Scenario 5

Need account list.

### Solution

```html
<lightning-datatable>
```

---

## Scenario 6

Need to display Accounts and Contacts in a hierarchy.

### Solution

```html
<lightning-tree-grid>
```

---

# Interview Questions

## Q1: What are Lightning Base Components?

### Answer

Prebuilt Salesforce UI components that follow SLDS standards.

---

## Q2: How do you read a lightning-input value?

### Answer

```javascript
event.target.value
```

---

## Q3: How do you read a combobox value?

### Answer

```javascript
event.detail.value
```

---

## Q4: Which component is used for popups?

### Answer

```html
<lightning-modal>
```

---

## Q5: Which component is used to display records in a table?

### Answer

```html
<lightning-datatable>
```

---

## Q6: How do you enable inline editing in a datatable?

### Answer

```javascript
editable: true
```

---

## Q7: How do you add row actions?

### Answer

Use:

```javascript
type: 'action'
```

and

```javascript
rowActions
```

---

## Q8: Difference between lightning-input and lightning-combobox?

| lightning-input | lightning-combobox |
|----------------|-------------------|
| Free Text Input | Dropdown Selection |
| Multiple Types | Option Based |
| User Enters Value | User Selects Value |

---

## Q9: What is the difference between lightning-datatable and lightning-tree-grid?

### Answer

| Datatable | Tree Grid |
|------------|------------|
| Flat Records | Parent-Child Records |
| Inline Edit Supported | Inline Edit Not Supported |
| No Expand/Collapse | Supports Expand/Collapse |

---

## Q10: Which property is required for child rows in Tree Grid?

### Answer

```javascript
_children
```

---

## Q11: When should you use lightning-tree-grid?

### Answer

When records have a hierarchical relationship such as:

- Account → Contacts
- Account → Opportunities
- Parent → Child Objects

---

# Summary

## lightning-input

- Text
- Number
- Email
- Password
- Checkbox
- Validation

---

## lightning-combobox

- Dropdown
- Uses label/value pairs
- Reads value via `event.detail.value`

---

## lightning-card

- Container component
- Supports title, icon, actions

---

## lightning-button

- User actions
- Multiple variants
- Supports click events

---

## lightning-modal

- Popup dialog
- Supports data passing and return values

---

## lightning-datatable

- Record display
- Sorting
- Row actions
- Inline editing
- Selection

---

## lightning-tree-grid

- Hierarchical Data
- Parent-Child Records
- Expand/Collapse
- Uses `_children`
- Alternative to Datatable for nested records
- Commonly used for Account-Contact and Account-Opportunity hierarchies
- Frequently asked in LWC interviews

Lightning Base Components are heavily used in real Salesforce projects and are among the most frequently asked topics in LWC interviews because they form the building blocks of almost every UI implementation.