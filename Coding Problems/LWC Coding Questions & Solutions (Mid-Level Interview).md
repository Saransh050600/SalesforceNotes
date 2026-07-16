
---

# 1. Parent → Child Communication (@api)

## Parent Component

### parent.html

```html
<template>
    <lightning-card title="Parent Component">
        <c-child account-name={accountName}></c-child>
    </lightning-card>
</template>
```

### parent.js

```javascript
import { LightningElement } from 'lwc';

export default class Parent extends LightningElement {
    accountName = 'Salesforce';
}
```

---

## Child Component

### child.html

```html
<template>
    <p>Account Name: {accountName}</p>
</template>
```

### child.js

```javascript
import { LightningElement, api } from 'lwc';

export default class Child extends LightningElement {
    @api accountName;
}
```

---

# 2. Child → Parent Communication (Custom Events)

## Child Component

### child.html

```html
<template>
    <lightning-button
        label="Send Data"
        onclick={sendData}>
    </lightning-button>
</template>
```

### child.js

```javascript
import { LightningElement } from 'lwc';

export default class Child extends LightningElement {

    sendData() {

        const event = new CustomEvent('accountselect', {
            detail: {
                accountId: '001XXXX'
            }
        });

        this.dispatchEvent(event);
    }
}
```

---

## Parent Component

### parent.html

```html
<template>
    <c-child
        onaccountselect={handleAccount}>
    </c-child>
</template>
```

### parent.js

```javascript
import { LightningElement } from 'lwc';

export default class Parent extends LightningElement {

    handleAccount(event) {
        console.log(event.detail.accountId);
    }
}
```

---

# 3. Display Accounts Using Wire Service

## Apex

```apex
public with sharing class AccountController {

    @AuraEnabled(cacheable=true)
    public static List<Account> getAccounts() {

        return [
            SELECT Id, Name, Industry
            FROM Account
            LIMIT 10
        ];
    }
}
```

---

## LWC

### accountList.js

```javascript
import { LightningElement, wire } from 'lwc';
import getAccounts from '@salesforce/apex/AccountController.getAccounts';

export default class AccountList extends LightningElement {

    accounts;
    error;

    @wire(getAccounts)
    wiredAccounts({ data, error }) {

        if(data) {
            this.accounts = data;
        }

        if(error) {
            this.error = error;
        }
    }
}
```

### accountList.html

```html
<template>

    <template if:true={accounts}>
        <template for:each={accounts}
                  for:item="acc">

            <p key={acc.Id}>
                {acc.Name}
            </p>

        </template>
    </template>

</template>
```

---

# 4. Imperative Apex Call

## Apex

```apex
@AuraEnabled
public static List<Contact> getContacts(Id accountId){

    return [
        SELECT Id, Name
        FROM Contact
        WHERE AccountId = :accountId
    ];
}
```

---

## JS

```javascript
import { LightningElement } from 'lwc';
import getContacts from '@salesforce/apex/ContactController.getContacts';

export default class ContactList extends LightningElement {

    contacts;

    loadContacts() {

        getContacts({
            accountId : '001XXXX'
        })
        .then(result => {
            this.contacts = result;
        })
        .catch(error => {
            console.error(error);
        });
    }
}
```

---

# 5. Search Accounts

## HTML

```html
<template>

    <lightning-input
        label="Search"
        onchange={handleSearch}>
    </lightning-input>

    <template for:each={filteredAccounts}
              for:item="acc">

        <p key={acc.Id}>
            {acc.Name}
        </p>

    </template>

</template>
```

---

## JS

```javascript
import { LightningElement } from 'lwc';

export default class SearchAccounts extends LightningElement {

    accounts = [
        { Id:'1', Name:'Google' },
        { Id:'2', Name:'Amazon' },
        { Id:'3', Name:'Salesforce' }
    ];

    filteredAccounts = [...this.accounts];

    handleSearch(event) {

        const keyword =
            event.target.value.toLowerCase();

        this.filteredAccounts =
            this.accounts.filter(acc =>
                acc.Name.toLowerCase()
                .includes(keyword)
            );
    }
}
```

---

# 6. Datatable with Row Actions

## JS

```javascript
columns = [

{
    label:'Name',
    fieldName:'Name'
},
{
    type:'button',
    typeAttributes:{
        label:'View',
        name:'view'
    }
}
];
```

---

## Handle Action

```javascript
handleRowAction(event) {

    const actionName =
        event.detail.action.name;

    const row =
        event.detail.row;

    if(actionName === 'view') {
        console.log(row.Id);
    }
}
```

---

# 7. Datatable Inline Edit

## Columns

```javascript
columns = [
{
    label:'Name',
    fieldName:'Name',
    editable:true
},
{
    label:'Phone',
    fieldName:'Phone',
    editable:true
}
];
```

---

## Save Handler

```javascript
handleSave(event) {

    const updatedFields =
        event.detail.draftValues;

    console.log(updatedFields);
}
```

---

# 8. NavigationMixin Example

## JS

```javascript
import { NavigationMixin }
from 'lightning/navigation';

export default class NavigateRecord
extends NavigationMixin(
    LightningElement
) {

    navigateToRecord() {

        this[NavigationMixin.Navigate]({
            type:'standard__recordPage',
            attributes:{
                recordId:'001XXXX',
                actionName:'view'
            }
        });
    }
}
```

---

# 9. Toast Message

## JS

```javascript
import { ShowToastEvent }
from 'lightning/platformShowToastEvent';

showSuccess() {

    this.dispatchEvent(
        new ShowToastEvent({
            title:'Success',
            message:'Record Saved',
            variant:'success'
        })
    );
}
```

---

# 10. Spinner During Loading

## HTML

```html
<template>

    <template if:true={isLoading}>
        <lightning-spinner>
        </lightning-spinner>
    </template>

</template>
```

---

## JS

```javascript
isLoading = true;

loadData() {

    this.isLoading = true;

    getAccounts()
    .then(result => {
        this.accounts = result;
    })
    .finally(() => {
        this.isLoading = false;
    });
}
```

---

# 11. Lifecycle Hooks

## JS

```javascript
import { LightningElement }
from 'lwc';

export default class Demo
extends LightningElement {

    constructor() {
        super();
        console.log('Constructor');
    }

    connectedCallback() {
        console.log('Connected');
    }

    renderedCallback() {
        console.log('Rendered');
    }

    disconnectedCallback() {
        console.log('Disconnected');
    }
}
```

---

# 12. Pub/Sub Using LMS

## Message Channel Import

```javascript
import ACCOUNT_CHANNEL
from '@salesforce/messageChannel/AccountChannel__c';
```

---

## Publish

```javascript
import {
    publish,
    MessageContext
}
from 'lightning/messageService';

@wire(MessageContext)
messageContext;

publishMessage() {

    publish(
        this.messageContext,
        ACCOUNT_CHANNEL,
        {
            recordId:'001XXXX'
        }
    );
}
```

---

## Subscribe

```javascript
import {
    subscribe,
    MessageContext
}
from 'lightning/messageService';

subscription;

@wire(MessageContext)
messageContext;

connectedCallback() {

    this.subscription =
    subscribe(
        this.messageContext,
        ACCOUNT_CHANNEL,
        message => {

            console.log(
                message.recordId
            );
        }
    );
}
```

---

# 13. Getter Example

## JS

```javascript
firstName = 'Saransh';
lastName = 'Rai';

get fullName() {

    return `${this.firstName}
            ${this.lastName}`;
}
```

---

## HTML

```html
<p>{fullName}</p>
```

---

# 14. Conditional Rendering

## HTML

```html
<template>

    <template if:true={showData}>
        Data Available
    </template>

    <template if:false={showData}>
        No Data
    </template>

</template>
```

---

# 15. Looping Records

## HTML

```html
<template>

    <template
        for:each={accounts}
        for:item="acc">

        <div key={acc.Id}>
            {acc.Name}
        </div>

    </template>

</template>
```

---

# 16. Calling Apex on Button Click

## JS

```javascript
handleLoad() {

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

# 17. Dynamic CSS Class

## JS

```javascript
isError = true;

get cardClass() {

    return this.isError
        ? 'error'
        : 'success';
}
```

---

## HTML

```html
<div class={cardClass}>
    Status
</div>
```

---

# 18. Modal Popup

## HTML

```html
<template if:true={showModal}>

<section
role="dialog"
class="slds-modal slds-fade-in-open">

<div class="slds-modal__container">

<header class="slds-modal__header">
    Modal Header
</header>

<div class="slds-modal__content">
    Modal Content
</div>

</div>

</section>

</template>
```

---

# 19. Refresh Apex

## JS

```javascript
import {
    refreshApex
}
from '@salesforce/apex';

refreshApex(this.wiredAccountsResult);
```

---

# 20. File Upload Component

## HTML

```html
<lightning-file-upload
    record-id={recordId}
    accept=".pdf,.png"
    multiple>
</lightning-file-upload>
```

---

# Most Asked Mid-Level LWC Coding Questions

### Data Handling
1. Parent to Child Communication
2. Child to Parent Communication
3. LMS Communication
4. Wire Service
5. Imperative Apex
6. Refresh Apex

### UI Development
7. Datatable
8. Inline Edit
9. Row Actions
10. Modal Popup
11. Toast Messages
12. Spinners

### Navigation
13. Record Page Navigation
14. Object Page Navigation
15. List View Navigation

### Lifecycle
16. Constructor
17. Connected Callback
18. Rendered Callback
19. Disconnected Callback

### Advanced
20. LMS
21. Dynamic Components
22. Static Resources
23. Pagination
24. Debouncing
25. Lazy Loading
26. Custom Datatable
27. Tree Grid
28. File Upload
29. Platform Events Integration
30. Reusable Components

# Interview Tip

For every coding question, explain:

- Component Communication
- Lifecycle Hooks
- Reactive Properties
- Wire vs Imperative
- Error Handling
- Loading State
- Governor Limits
- Reusability
- Security (CRUD/FLS)
- Performance Optimization

These points often matter more to interviewers than the code itself.