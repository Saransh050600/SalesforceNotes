
Apex Integration is one of the most important topics in LWC development.

LWC communicates with Salesforce server-side logic using Apex methods.

Common approaches:

- Wire Apex
- Imperative Apex
- Cacheable Methods
- refreshApex()
- Error Handling

---

# Why Use Apex?

Use Apex when:

- Querying Salesforce data
- Performing DML operations
- Calling external APIs
- Complex business logic
- Large data processing

---

# Exposing Apex to LWC

Apex methods must be:

```java
public static
```

and annotated with:

```java
@AuraEnabled
```

---

## Apex Class Example

```java
public with sharing class AccountController {

    @AuraEnabled
    public static List<Account> getAccounts() {

        return [
            SELECT Id, Name
            FROM Account
            LIMIT 10
        ];
    }
}
```

---

# Importing Apex in LWC

```javascript
import getAccounts
from '@salesforce/apex/AccountController.getAccounts';
```

---

# Imperative Calls

Imperative Apex gives complete control over when Apex executes.

Used when:

- Button Click
- Form Submission
- Create/Update/Delete
- Need Dynamic Parameters
- Non-cacheable Methods

---

## Apex

```java
@AuraEnabled
public static List<Account> getAccounts() {

    return [
        SELECT Id, Name
        FROM Account
    ];
}
```

---

## JavaScript

```javascript
import getAccounts
from '@salesforce/apex/AccountController.getAccounts';

loadAccounts() {

    getAccounts()

        .then(result => {

            this.accounts = result;

        })

        .catch(error => {

            console.error(error);

        });
}
```

---

## Button Example

### HTML

```html
<lightning-button
    label="Load Accounts"
    onclick={loadAccounts}>
</lightning-button>
```

---

## Passing Parameters

### Apex

```java
@AuraEnabled
public static Account getAccount(
    Id accountId
) {

    return [
        SELECT Id, Name
        FROM Account
        WHERE Id = :accountId
    ];
}
```

---

### JavaScript

```javascript
getAccount({

    accountId: this.recordId

})

.then(result => {

    this.account = result;

});
```

---

# Wire Apex

Wire Service automatically calls Apex whenever reactive parameters change.

Used when:

- Read-only data
- Automatic Refresh
- Reactive UI
- Better Performance

---

# Cacheable Requirement

Wire Apex requires:

```java
@AuraEnabled(cacheable=true)
```

---

## Apex

```java
@AuraEnabled(cacheable=true)
public static List<Account> getAccounts() {

    return [
        SELECT Id, Name
        FROM Account
    ];
}
```

---

## JavaScript

```javascript
@wire(getAccounts)

accounts;
```

---

## HTML

```html
<template
    for:each={accounts.data}
    for:item="acc">

    <p key={acc.Id}>
        {acc.Name}
    </p>

</template>
```

---

# Wire Function

Provides more control.

```javascript
@wire(getAccounts)

wiredAccounts(
    result
){

    this.wiredResult =
        result;

    if(result.data){

        this.accounts =
            result.data;

    }

    if(result.error){

        console.error(
            result.error
        );
    }
}
```

---

# Reactive Parameters

Wire automatically executes when parameter changes.

---

## Apex

```java
@AuraEnabled(cacheable=true)
public static List<Contact>
getContacts(
    Id accountId
){
    return [
        SELECT Id, Name
        FROM Contact
        WHERE AccountId = :accountId
    ];
}
```

---

## JavaScript

```javascript
@api recordId;

@wire(getContacts, {

    accountId: '$recordId'

})
contacts;
```

---

# Imperative vs Wire

| Feature | Imperative | Wire |
|----------|------------|-------|
| Manual Execution | ✅ | ❌ |
| Automatic Execution | ❌ | ✅ |
| DML Support | ✅ | ❌ |
| Cacheable Required | ❌ | ✅ |
| Reactive Parameters | ❌ | ✅ |

---

# Cacheable Methods

Used to improve performance.

Salesforce stores response in browser cache.

---

## Syntax

```java
@AuraEnabled(
    cacheable=true
)
```

---

## Example

```java
@AuraEnabled(cacheable=true)
public static List<Account>
getAccounts(){

    return [
        SELECT Id, Name
        FROM Account
    ];
}
```

---

# Benefits

- Faster UI
- Reduced Server Calls
- Better Performance
- Improved User Experience

---

# Cacheable Restrictions

Cannot perform:

```java
insert
update
delete
upsert
```

---

Cannot modify records.

Only read operations allowed.

---

# Error Handling

Always handle Apex errors.

---

## Imperative Error Handling

```javascript
getAccounts()

.then(result => {

    this.accounts =
        result;

})

.catch(error => {

    console.error(
        error.body.message
    );

});
```

---

# Try-Catch in Apex

```java
@AuraEnabled
public static String saveData() {

    try {

        return 'Success';

    } catch(Exception ex) {

        throw new AuraHandledException(
            ex.getMessage()
        );
    }
}
```

---

# AuraHandledException

Best practice for sending custom errors to LWC.

```java
throw new AuraHandledException(
    'Custom Error Message'
);
```

---

# Wire Error Handling

```javascript
@wire(getAccounts)

wiredAccounts(result){

    if(result.data){

        this.accounts =
            result.data;
    }

    if(result.error){

        console.error(
            result.error
        );
    }
}
```

---

# Showing Errors with Toast

```javascript
import { ShowToastEvent }
from 'lightning/platformShowToastEvent';
```

---

## Example

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

# refreshApex()

Used to refresh cached wire data.

One of the most asked interview topics.

---

# Why refreshApex?

Wire Apex caches data.

After updating records:

```text
Database Updated
↓
Wire Still Shows Old Data
↓
refreshApex()
```

---

# Import

```javascript
import { refreshApex }
from '@salesforce/apex';
```

---

# Store Wire Result

```javascript
wiredAccountsResult;

@wire(getAccounts)

wiredAccounts(result){

    this.wiredAccountsResult =
        result;

    if(result.data){

        this.accounts =
            result.data;
    }
}
```

---

# Refresh Data

```javascript
refreshApex(
    this.wiredAccountsResult
);
```

---

# Example After Delete

```javascript
deleteAccount({

    accountId: id

})

.then(() => {

    return refreshApex(
        this.wiredAccountsResult
    );

});
```

---

# Real Project Flow

```text
Load Data Using Wire
           ↓
User Updates Record
           ↓
Imperative Apex Call
           ↓
Database Updated
           ↓
refreshApex()
           ↓
UI Automatically Refreshes
```

---

# Common Interview Scenarios

## Scenario 1

Load accounts when component opens.

### Solution

```javascript
@wire(getAccounts)
```

---

## Scenario 2

Load data after button click.

### Solution

```javascript
Imperative Apex
```

---

## Scenario 3

Insert Account.

### Solution

```javascript
Imperative Apex
```

because DML is involved.

---

## Scenario 4

Refresh records after update.

### Solution

```javascript
refreshApex()
```

---

## Scenario 5

Show custom Apex error.

### Solution

```java
AuraHandledException
```

---

# Best Practices

## Use Wire For

- Read-only Data
- UI Display
- Cacheable Operations

---

## Use Imperative For

- Create
- Update
- Delete
- Button Click Events
- Manual Execution

---

## Always

- Handle Errors
- Use AuraHandledException
- Use refreshApex after DML
- Cache Read Operations

---

# Interview Questions

## Q1: Difference between Wire and Imperative Apex?

| Wire | Imperative |
|--------|------------|
| Automatic | Manual |
| Read Only | Read + DML |
| Cacheable Required | Not Required |
| Reactive | Non-Reactive |

---

## Q2: Can Wire Apex call a non-cacheable method?

### Answer

No.

```java
@AuraEnabled(cacheable=true)
```

is mandatory.

---

## Q3: When should you use Imperative Apex?

### Answer

When:

- Button Click
- Create Record
- Update Record
- Delete Record
- Need Full Control

---

## Q4: What is refreshApex()?

### Answer

Refreshes cached wire data and fetches latest records from Salesforce.

---

## Q5: Why store the wire result?

### Answer

Because refreshApex() requires the original wire response object.

```javascript
this.wiredResult = result;
```

---

## Q6: What is AuraHandledException?

### Answer

A custom Apex exception used to send meaningful error messages to LWC.

---

## Q7: Can cacheable Apex perform DML?

### Answer

No.

Cacheable methods must be read-only.

---

## Q8: Which is better for loading records on page load?

### Answer

```javascript
@wire
```

because it supports caching and automatic refresh.

---

# Summary

## Wire Apex

- Automatic execution
- Reactive
- Cacheable required
- Best for read operations

---

## Imperative Apex

- Manual execution
- Supports DML
- Used for button actions

---

## Cacheable Methods

```java
@AuraEnabled(cacheable=true)
```

- Faster performance
- Read-only operations

---

## Error Handling

```java
AuraHandledException
```

```javascript
.catch()
```

```javascript
ShowToastEvent
```

---

## refreshApex()

```javascript
refreshApex()
```

- Refreshes cached wire data
- Used after insert/update/delete
- Frequently asked in interviews

Apex Integration is one of the most important LWC interview topics because almost every enterprise application uses Apex to retrieve, manipulate, and refresh Salesforce data.