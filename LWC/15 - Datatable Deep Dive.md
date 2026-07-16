
`lightning-datatable` is one of the most commonly used and frequently asked LWC interview topics.

It is used to display Salesforce records in a tabular format with advanced features such as:

- Sorting
- Pagination
- Row Actions
- Inline Editing
- Custom Data Types
- Row Selection
- Dynamic Columns

---

# What is lightning-datatable?

Salesforce base component used to display data in table format.

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

---

### JavaScript

```javascript
columns = [
    {
        label: 'Name',
        fieldName: 'Name',
        type: 'text'
    },
    {
        label: 'Industry',
        fieldName: 'Industry',
        type: 'text'
    }
];
```

---

# Datatable Column Types

| Type | Description |
|--------|------------|
| text | Text Data |
| number | Numeric Data |
| currency | Currency |
| percent | Percentage |
| date | Date |
| phone | Phone Number |
| email | Email |
| url | Hyperlink |
| boolean | Checkbox |
| action | Row Actions |
| button | Custom Button |

---

# Sorting

Sorting allows users to sort rows by a selected column.

---

## Enable Sorting

### Column Definition

```javascript
columns = [
    {
        label: 'Name',
        fieldName: 'Name',
        sortable: true
    }
];
```

---

## Datatable

```html
<lightning-datatable
    data={accounts}
    columns={columns}
    sorted-by={sortedBy}
    sorted-direction={sortDirection}
    onsort={handleSort}>
</lightning-datatable>
```

---

## JavaScript

```javascript
sortedBy;
sortDirection;
```

---

## Sort Handler

```javascript
handleSort(event) {

    const {
        fieldName,
        sortDirection
    } = event.detail;

    this.sortedBy =
        fieldName;

    this.sortDirection =
        sortDirection;

    this.sortData(
        fieldName,
        sortDirection
    );
}
```

---

## Sorting Logic

```javascript
sortData(fieldName, direction) {

    let data =
        [...this.accounts];

    data.sort((a,b) => {

        let valueA =
            a[fieldName] || '';

        let valueB =
            b[fieldName] || '';

        return direction === 'asc'
            ? valueA.localeCompare(valueB)
            : valueB.localeCompare(valueA);

    });

    this.accounts = data;
}
```

---

# Pagination

Datatable does NOT provide built-in pagination.

Pagination must be implemented manually.

---

# Why Use Pagination?

Benefits:

- Better Performance
- Faster Rendering
- Improved UX
- Handles Large Data Sets

---

## Variables

```javascript
pageSize = 10;

currentPage = 1;

totalPages = 0;
```

---

## Paginated Data

```javascript
get paginatedData() {

    const start =
        (this.currentPage - 1)
        * this.pageSize;

    const end =
        start + this.pageSize;

    return this.accounts
        .slice(start,end);
}
```

---

## Previous Page

```javascript
handlePrevious() {

    if(this.currentPage > 1){

        this.currentPage--;
    }
}
```

---

## Next Page

```javascript
handleNext() {

    if(
        this.currentPage <
        this.totalPages
    ){
        this.currentPage++;
    }
}
```

---

## HTML Buttons

```html
<lightning-button
    label="Previous"
    onclick={handlePrevious}>
</lightning-button>

<lightning-button
    label="Next"
    onclick={handleNext}>
</lightning-button>
```

---

# Row Actions

Row Actions provide record-specific actions.

Example:

- Edit
- Delete
- Clone
- View

---

## Action Column

```javascript
{
    type: 'action',

    typeAttributes: {

        rowActions: [
            {
                label: 'Edit',
                name: 'edit'
            },

            {
                label: 'Delete',
                name: 'delete'
            }
        ]
    }
}
```

---

## Datatable

```html
<lightning-datatable
    columns={columns}
    data={accounts}
    onrowaction={handleRowAction}>
</lightning-datatable>
```

---

## Handle Actions

```javascript
handleRowAction(event) {

    const actionName =
        event.detail.action.name;

    const row =
        event.detail.row;

    switch(actionName){

        case 'edit':
            this.editRecord(row);
            break;

        case 'delete':
            this.deleteRecord(row);
            break;
    }
}
```

---

# Common Row Actions

| Action | Purpose |
|----------|----------|
| View | Open Record |
| Edit | Edit Record |
| Delete | Delete Record |
| Clone | Clone Record |

---

# Inline Editing

Allows editing directly inside the table.

Very common interview topic.

---

## Enable Inline Edit

### Column

```javascript
{
    label: 'Name',
    fieldName: 'Name',
    editable: true
}
```

---

## Datatable

```html
<lightning-datatable
    data={accounts}
    columns={columns}
    draft-values={draftValues}
    onsave={handleSave}>
</lightning-datatable>
```

---

# Draft Values

Stores modified rows.

```javascript
event.detail.draftValues
```

Example:

```javascript
[
    {
        Id: '001',
        Name: 'Updated Account'
    }
]
```

---

# Save Handler

```javascript
handleSave(event) {

    const draftValues =
        event.detail.draftValues;

    console.log(
        draftValues
    );
}
```

---

# Update Records Using LDS

```javascript
import {
    updateRecord
}
from 'lightning/uiRecordApi';
```

---

## Example

```javascript
const recordInputs =
    draftValues.map(
        draft => {

            return {
                fields: draft
            };

        }
    );

Promise.all(

    recordInputs.map(
        record =>
            updateRecord(record)
    )

);
```

---

# Clear Draft Values

```javascript
this.draftValues = [];
```

---

# Custom Data Types

Used when standard datatable types are not sufficient.

Examples:

- Image Column
- Progress Bar
- Rating Stars
- Custom Buttons
- Status Badges

---

# Why Custom Types?

Standard types:

```javascript
text
currency
date
email
```

may not meet business requirements.

---

# Create Custom Datatable

Extend:

```javascript
LightningDatatable
```

---

## customDatatable.js

```javascript
import LightningDatatable
from 'lightning/datatable';

import customBadgeTemplate
from './customBadge.html';

export default class CustomDatatable
extends LightningDatatable {

    static customTypes = {

        badge: {

            template:
                customBadgeTemplate,

            standardCellLayout:
                true,

            typeAttributes: [
                'label'
            ]
        }
    };
}
```

---

## Custom Template

### customBadge.html

```html
<template>

    <lightning-badge
        label={typeAttributes.label}>
    </lightning-badge>

</template>
```

---

## Column Definition

```javascript
{
    label: 'Status',

    fieldName: 'Status',

    type: 'badge',

    typeAttributes: {

        label: {
            fieldName:
                'Status'
        }
    }
}
```

---

# Dynamic Columns

Columns can be created dynamically.

```javascript
this.columns =
    response.columns;
```

---

# Row Selection

Enable record selection.

---

## Datatable

```html
<lightning-datatable
    onrowselection=
        {handleSelection}>
</lightning-datatable>
```

---

## JavaScript

```javascript
handleSelection(event){

    const selectedRows =
        event.detail.selectedRows;
}
```

---

# Selected Row Example

```javascript
const ids =
    selectedRows.map(
        row => row.Id
    );
```

---

# Datatable Performance Tips

## Use Pagination

Avoid loading thousands of records.

---

## Use Lazy Loading

Load data only when needed.

---

## Avoid Excessive Columns

Keep tables readable.

---

## Use Cacheable Apex

```java
@AuraEnabled(cacheable=true)
```

---

# Common Interview Scenarios

## Scenario 1

Need sortable account list.

### Solution

```javascript
sortable: true
```

and

```javascript
onsort
```

---

## Scenario 2

Need account-specific actions.

### Solution

```javascript
type: 'action'
```

---

## Scenario 3

Need inline editing.

### Solution

```javascript
editable: true
```

---

## Scenario 4

Need status badge.

### Solution

```javascript
Custom Data Type
```

---

## Scenario 5

Need to handle 10,000 records.

### Solution

```javascript
Pagination
```

---

# Datatable vs Tree Grid

| Feature | Datatable | Tree Grid |
|----------|------------|------------|
| Flat Data | ✅ | ❌ |
| Hierarchical Data | ❌ | ✅ |
| Inline Edit | ✅ | ❌ |
| Row Actions | ✅ | ✅ |
| Sorting | ✅ | Limited |
| Pagination | Manual | Manual |

---

# Interview Questions

## Q1: Does lightning-datatable support built-in pagination?

### Answer

No.

Pagination must be implemented manually.

---

## Q2: How do you make a column sortable?

### Answer

```javascript
sortable: true
```

---

## Q3: Which event handles sorting?

### Answer

```javascript
onsort
```

---

## Q4: What are draftValues?

### Answer

Stores unsaved inline edits.

```javascript
event.detail.draftValues
```

---

## Q5: Which event is fired when inline edits are saved?

### Answer

```javascript
onsave
```

---

## Q6: How do you add row actions?

### Answer

```javascript
type: 'action'
```

---

## Q7: What class is extended for custom data types?

### Answer

```javascript
LightningDatatable
```

---

## Q8: How do you get selected rows?

### Answer

```javascript
event.detail.selectedRows
```

---

## Q9: How are records updated after inline editing?

### Answer

Typically using:

```javascript
updateRecord()
```

or Apex.

---

## Q10: What are examples of custom data types?

### Answer

- Images
- Badges
- Rating Stars
- Progress Bars
- Custom Buttons

---

# Summary

## Sorting

```javascript
sortable: true
onsort
```

- User-controlled sorting
- Ascending/Descending

---

## Pagination

- Manual implementation
- Better performance
- Handles large datasets

---

## Row Actions

```javascript
type: 'action'
```

- Edit
- Delete
- View
- Clone

---

## Inline Editing

```javascript
editable: true
```

- Uses draftValues
- Saved with onsave

---

## Custom Data Types

```javascript
extends LightningDatatable
```

- Images
- Badges
- Progress Bars
- Custom Components

Datatable Deep Dive is one of the highest-priority LWC interview topics because nearly every Salesforce application uses datatables for displaying and managing records.