
Lightning Data Service (LDS) allows LWC components to interact with Salesforce records without writing Apex.

It uses Salesforce UI APIs behind the scenes and automatically handles:

- CRUD Operations
- Field-Level Security (FLS)
- Sharing Rules
- Record Caching
- Data Synchronization

---

# Why Use Lightning Data Service?

Instead of:

```text
LWC
 ↓
Apex
 ↓
Database
```

You can use:

```text
LWC
 ↓
Lightning Data Service
 ↓
Salesforce UI API
 ↓
Database
```

Benefits:

- No Apex Required
- Better Performance
- Automatic Caching
- Security Enforcement
- Reduced Code

---

# LDS vs Apex

| Feature | LDS | Apex |
|----------|------|------|
| No Apex Required | ✅ | ❌ |
| Supports CRUD | ✅ | ✅ |
| Automatic Caching | ✅ | ❌ |
| FLS Enforcement | ✅ | Manual |
| Sharing Enforcement | ✅ | Depends |
| Complex Logic | ❌ | ✅ |

---

# Common UI API Functions

| Function | Purpose |
|------------|------------|
| getRecord | Read Record |
| createRecord | Create Record |
| updateRecord | Update Record |
| deleteRecord | Delete Record |
| getFieldValue | Extract Field Values |

---

# getRecord

Used to retrieve Salesforce records.

Most commonly used LDS function.

---

## Import

```javascript
import { getRecord }
from 'lightning/uiRecordApi';
```

---

## Import Fields

```javascript
import NAME_FIELD
from '@salesforce/schema/Account.Name';

import INDUSTRY_FIELD
from '@salesforce/schema/Account.Industry';
```

---

## Wire Example

```javascript
@api recordId;

@wire(getRecord, {

    recordId: '$recordId',

    fields: [
        NAME_FIELD,
        INDUSTRY_FIELD
    ]

})

account;
```

---

## Access Data

```javascript
this.account.data
```

---

## HTML Example

```html
<template if:true={account.data}>

    Account Loaded

</template>
```

---

# getFieldValue()

Used to extract field values from getRecord response.

Recommended approach.

---

## Import

```javascript
import { getFieldValue }
from 'lightning/uiRecordApi';
```

---

## Example

```javascript
get accountName() {

    return getFieldValue(

        this.account.data,

        NAME_FIELD

    );
}
```

---

## HTML

```html
<p>{accountName}</p>
```

---

# Why Use getFieldValue?

Instead of:

```javascript
this.account.data.fields.Name.value
```

Use:

```javascript
getFieldValue()
```

Benefits:

- Cleaner
- Safer
- Readable
- Handles Undefined Values

---

# Multiple Field Example

```javascript
get accountIndustry() {

    return getFieldValue(
        this.account.data,
        INDUSTRY_FIELD
    );
}
```

---

# createRecord()

Creates Salesforce records without Apex.

---

## Import

```javascript
import { createRecord }
from 'lightning/uiRecordApi';
```

---

## Example

```javascript
const fields = {

    Name: 'Test Account',

    Industry: 'Technology'

};

const recordInput = {

    apiName: 'Account',

    fields
};

createRecord(recordInput)

.then(result => {

    console.log(result.id);

})

.catch(error => {

    console.error(error);

});
```

---

# Dynamic Example

```javascript
const fields = {};

fields.Name = this.name;
fields.Phone = this.phone;
```

---

# Success Response

```javascript
result.id
```

Returns newly created record Id.

---

# updateRecord()

Updates existing records.

---

## Import

```javascript
import { updateRecord }
from 'lightning/uiRecordApi';
```

---

## Example

```javascript
const fields = {

    Id: this.recordId,

    Name: 'Updated Account'

};

const recordInput = {

    fields
};

updateRecord(recordInput)

.then(() => {

    console.log(
        'Updated'
    );

});
```

---

# Dynamic Update Example

```javascript
const fields = {};

fields.Id = this.recordId;
fields.Name = this.accountName;
```

---

# deleteRecord()

Deletes Salesforce records.

---

## Import

```javascript
import { deleteRecord }
from 'lightning/uiRecordApi';
```

---

## Example

```javascript
deleteRecord(
    this.recordId
)

.then(() => {

    console.log(
        'Deleted'
    );

})

.catch(error => {

    console.error(
        error
    );

});
```

---

# CRUD Operations Summary

| Operation | Function |
|------------|------------|
| Create | createRecord() |
| Read | getRecord() |
| Update | updateRecord() |
| Delete | deleteRecord() |

---

# Using Toast Messages

Always show success/error feedback.

---

## Import

```javascript
import { ShowToastEvent }
from 'lightning/platformShowToastEvent';
```

---

## Success Toast

```javascript
this.dispatchEvent(

    new ShowToastEvent({

        title: 'Success',

        message:
            'Record Created',

        variant: 'success'

    })

);
```

---

## Error Toast

```javascript
this.dispatchEvent(

    new ShowToastEvent({

        title: 'Error',

        message:
            error.body.message,

        variant: 'error'

    })

);
```

---

# Common Real-World Example

## Load Account

```javascript
@wire(getRecord, {

    recordId: '$recordId',

    fields: [
        NAME_FIELD
    ]

})

account;
```

---

## Show Name

```javascript
get accountName() {

    return getFieldValue(

        this.account.data,

        NAME_FIELD

    );
}
```

---

## Update Name

```javascript
const fields = {

    Id: this.recordId,

    Name: this.accountName

};

updateRecord({
    fields
});
```

---

# LDS Caching

LDS automatically caches records.

Benefits:

- Faster UI
- Fewer Server Calls
- Shared Record Cache
- Better User Experience

---

# Security Features

LDS automatically respects:

- CRUD Permissions
- Field-Level Security
- Sharing Rules

Unlike Apex, no extra coding required.

---

# When to Use LDS

Use LDS when:

- Simple CRUD
- Single Record Operations
- Standard Salesforce Objects
- No Complex Business Logic

---

# When to Use Apex Instead

Use Apex when:

- Complex SOQL
- Multiple Objects
- Business Logic
- External APIs
- Bulk Processing

---

# LDS vs Wire Apex

| Feature | LDS | Wire Apex |
|----------|----------|----------|
| No Apex Needed | ✅ | ❌ |
| Automatic CRUD | ✅ | ❌ |
| Complex Queries | ❌ | ✅ |
| FLS Handling | ✅ | Manual |
| Caching | ✅ | Cacheable Only |

---

# Real Interview Scenarios

## Scenario 1

Need Account Name on Record Page.

### Solution

```javascript
getRecord()
```

---

## Scenario 2

Need to display field value.

### Solution

```javascript
getFieldValue()
```

---

## Scenario 3

Need to create Account without Apex.

### Solution

```javascript
createRecord()
```

---

## Scenario 4

Need to update Account.

### Solution

```javascript
updateRecord()
```

---

## Scenario 5

Need to delete Contact.

### Solution

```javascript
deleteRecord()
```

---

## Scenario 6

Need CRUD + Security without Apex.

### Solution

```javascript
Lightning Data Service
```

---

# Interview Questions

## Q1: What is Lightning Data Service?

### Answer

A Salesforce data layer that allows LWC components to perform CRUD operations without Apex.

---

## Q2: What is getRecord()?

### Answer

Retrieves Salesforce record data using UI API.

---

## Q3: Why use getFieldValue()?

### Answer

Safely extracts field values from a getRecord response.

```javascript
getFieldValue()
```

---

## Q4: Which method creates records?

### Answer

```javascript
createRecord()
```

---

## Q5: Which method updates records?

### Answer

```javascript
updateRecord()
```

---

## Q6: Which method deletes records?

### Answer

```javascript
deleteRecord()
```

---

## Q7: Does Lightning Data Service require Apex?

### Answer

No.

LDS works directly with Salesforce UI APIs.

---

## Q8: What security features does LDS provide?

### Answer

Automatically respects:

- CRUD Permissions
- Field-Level Security
- Sharing Rules

---

## Q9: When should you use Apex instead of LDS?

### Answer

When:

- Complex Queries
- Multiple Object Operations
- Business Logic
- External Integrations

---

## Q10: What is the advantage of LDS caching?

### Answer

- Faster Performance
- Reduced Server Calls
- Better User Experience

---

# Summary

## getRecord()

```javascript
getRecord()
```

- Read Records
- Uses Wire Service
- Supports Caching

---

## getFieldValue()

```javascript
getFieldValue()
```

- Extracts Field Values
- Safer than direct access

---

## createRecord()

```javascript
createRecord()
```

- Creates Records
- No Apex Needed

---

## updateRecord()

```javascript
updateRecord()
```

- Updates Existing Records

---

## deleteRecord()

```javascript
deleteRecord()
```

- Deletes Records

---

## Lightning Data Service

- No Apex Required
- Automatic Caching
- CRUD Operations
- FLS Enforcement
- Sharing Enforcement
- Frequently Asked in Interviews

Lightning Data Service is preferred whenever simple CRUD operations can be performed without Apex because it reduces code, improves performance, and automatically handles Salesforce security.