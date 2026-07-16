
Navigation and User Experience are critical aspects of LWC development.

Salesforce provides built-in APIs and components that allow users to:

- Navigate between pages
- Open records
- Open list views
- Create records
- Display notifications
- Show loading indicators

---

# Topics Covered

| Topic | Purpose |
|---------|---------|
| NavigationMixin | Navigate between pages |
| Record Pages | Open specific records |
| Object Pages | Open object home pages |
| List Views | Open filtered lists |
| Toast Messages | User notifications |
| Spinners | Loading indicators |

---

# NavigationMixin

NavigationMixin is used to navigate users to different Salesforce pages.

---

## Import

```javascript
import { NavigationMixin }
from 'lightning/navigation';
```

---

## Extend Component

```javascript
export default class DemoComponent
extends NavigationMixin(
    LightningElement
) {
}
```

---

# Why Use NavigationMixin?

Instead of hardcoding URLs:

```javascript
window.location.href
```

Use:

```javascript
NavigationMixin
```

Benefits:

- Works across Salesforce environments
- Supports Mobile App
- Supports Lightning Experience
- URL-independent

---

# Record Pages

Used to navigate to a specific Salesforce record.

---

## Open Record Page

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__recordPage',

    attributes: {

        recordId:
            this.recordId,

        objectApiName:
            'Account',

        actionName:
            'view'
    }

});
```

---

## Record Actions

### View Record

```javascript
actionName: 'view'
```

---

### Edit Record

```javascript
actionName: 'edit'
```

---

### Clone Record

```javascript
actionName: 'clone'
```

---

# Example

```html
<lightning-button
    label="View Account"
    onclick={navigateToRecord}>
</lightning-button>
```

```javascript
navigateToRecord() {

    this[NavigationMixin.Navigate]({

        type:
            'standard__recordPage',

        attributes: {

            recordId:
                this.recordId,

            objectApiName:
                'Account',

            actionName:
                'view'
        }

    });
}
```

---

# Object Pages

Used to open an object's home page.

---

## Navigate to Account Home

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__objectPage',

    attributes: {

        objectApiName:
            'Account',

        actionName:
            'home'
    }

});
```

---

# Create New Record Page

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__objectPage',

    attributes: {

        objectApiName:
            'Account',

        actionName:
            'new'
    }

});
```

---

# Object Page Actions

| Action | Purpose |
|----------|----------|
| home | Object Home |
| new | Create Record |

---

# List Views

Used to open specific object list views.

---

## Open Recently Viewed Accounts

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__objectPage',

    attributes: {

        objectApiName:
            'Account',

        actionName:
            'list'
    },

    state: {

        filterName:
            'Recent'
    }

});
```

---

# Open All Accounts

```javascript
filterName:
    'AllAccounts'
```

---

# Common List View Filters

| Filter Name | Description |
|------------|-------------|
| Recent | Recently Viewed |
| AllAccounts | All Accounts |
| MyAccounts | My Accounts |
| NewThisWeek | New This Week |

---

# Navigation to Related Lists

Example:

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__recordRelationshipPage',

    attributes: {

        recordId:
            this.recordId,

        objectApiName:
            'Account',

        relationshipApiName:
            'Contacts',

        actionName:
            'view'
    }

});
```

---

# Navigation to Web Page

```javascript
this[NavigationMixin.Navigate]({

    type:
        'standard__webPage',

    attributes: {

        url:
            'https://www.salesforce.com'
    }

});
```

---

# Generate URL

Instead of navigating immediately:

```javascript
this[NavigationMixin.GenerateUrl]({

    type:
        'standard__recordPage',

    attributes: {

        recordId:
            this.recordId,

        actionName:
            'view'
    }

})
.then(url => {

    console.log(url);

});
```

---

# Toast Messages

Toast messages provide feedback to users.

One of the most frequently used UX features.

---

# Import

```javascript
import { ShowToastEvent }
from 'lightning/platformShowToastEvent';
```

---

# Success Toast

```javascript
this.dispatchEvent(

    new ShowToastEvent({

        title:
            'Success',

        message:
            'Account Created',

        variant:
            'success'

    })

);
```

---

# Error Toast

```javascript
this.dispatchEvent(

    new ShowToastEvent({

        title:
            'Error',

        message:
            'Something went wrong',

        variant:
            'error'

    })

);
```

---

# Warning Toast

```javascript
variant:
    'warning'
```

---

# Info Toast

```javascript
variant:
    'info'
```

---

# Toast Variants

| Variant | Purpose |
|----------|----------|
| success | Success Message |
| error | Error Message |
| warning | Warning Message |
| info | Information Message |

---

# Toast Modes

---

## Dismissible

Default.

```javascript
mode:
'dismissible'
```

---

## Sticky

User must close manually.

```javascript
mode:
'sticky'
```

---

## Pester

Auto-closes.

```javascript
mode:
'pester'
```

---

# Spinner

Used to indicate loading.

Improves user experience.

---

# Why Use Spinner?

Without spinner:

```text
Button Clicked
↓
Nothing Happens
↓
User Confused
```

With spinner:

```text
Button Clicked
↓
Spinner Appears
↓
User Knows Processing
```

---

# Basic Spinner

```html
<lightning-spinner
    alternative-text="Loading">
</lightning-spinner>
```

---

# Conditional Spinner

### HTML

```html
<template if:true={isLoading}>

    <lightning-spinner
        alternative-text="Loading">
    </lightning-spinner>

</template>
```

---

### JavaScript

```javascript
isLoading = false;
```

---

# Spinner with Apex

```javascript
loadAccounts() {

    this.isLoading = true;

    getAccounts()

    .then(result => {

        this.accounts =
            result;

    })

    .catch(error => {

        console.error(
            error
        );

    })

    .finally(() => {

        this.isLoading =
            false;

    });
}
```

---

# Spinner Sizes

```html
size="small"
```

```html
size="medium"
```

```html
size="large"
```

---

# Spinner Example

```html
<lightning-spinner
    size="large"
    alternative-text="Loading">
</lightning-spinner>
```

---

# Real Project Example

```javascript
handleSave() {

    this.isLoading = true;

    saveAccount()

    .then(() => {

        this.showToast(
            'Success',
            'Account Saved',
            'success'
        );

    })

    .finally(() => {

        this.isLoading = false;

    });
}
```

---

# Common Interview Scenarios

## Scenario 1

Need to open Account Record.

### Solution

```javascript
NavigationMixin
```

---

## Scenario 2

Need to create Account page.

### Solution

```javascript
standard__objectPage
```

with:

```javascript
actionName:'new'
```

---

## Scenario 3

Need to show success after save.

### Solution

```javascript
ShowToastEvent
```

---

## Scenario 4

Need loading indicator during Apex call.

### Solution

```html
<lightning-spinner>
```

---

## Scenario 5

Need to open Accounts List View.

### Solution

```javascript
actionName:'list'
```

---

# Navigation Types

| Type | Purpose |
|---------|---------|
| standard__recordPage | Record Page |
| standard__objectPage | Object Page |
| standard__recordRelationshipPage | Related List |
| standard__webPage | External URL |

---

# Best Practices

## Navigation

- Always use NavigationMixin
- Avoid hardcoded URLs
- Use GenerateUrl when needed

---

## Toasts

- Show success messages after save
- Show error messages on failures
- Use appropriate variants

---

## Spinners

- Show during long-running operations
- Hide in finally()
- Prevent duplicate clicks

---

# Interview Questions

## Q1: What is NavigationMixin?

### Answer

A Salesforce navigation service used to navigate between pages without hardcoded URLs.

---

## Q2: How do you navigate to a record page?

### Answer

```javascript
type:
'standard__recordPage'
```

---

## Q3: Which action opens a record in edit mode?

### Answer

```javascript
actionName:
'edit'
```

---

## Q4: Which event displays toast notifications?

### Answer

```javascript
ShowToastEvent
```

---

## Q5: What are the toast variants?

### Answer

- success
- error
- warning
- info

---

## Q6: Why use a spinner?

### Answer

To indicate processing and improve user experience.

---

## Q7: Which Navigation Type opens an object home page?

### Answer

```javascript
standard__objectPage
```

---

## Q8: How do you navigate to a list view?

### Answer

```javascript
actionName:
'list'
```

---

## Q9: How do you open a related list?

### Answer

```javascript
standard__recordRelationshipPage
```

---

## Q10: What is GenerateUrl used for?

### Answer

Generates a Salesforce URL without immediately navigating.

---

# Summary

## NavigationMixin

- Navigate to pages
- Avoid hardcoded URLs
- Supports Mobile & Lightning Experience

---

## Record Pages

```javascript
standard__recordPage
```

- View
- Edit
- Clone

---

## Object Pages

```javascript
standard__objectPage
```

- Home
- New
- List Views

---

## Toast Messages

```javascript
ShowToastEvent
```

- Success
- Error
- Warning
- Info

---

## Spinner

```html
<lightning-spinner>
```

- Loading Indicator
- Better UX
- Used during Apex/UI API operations

Navigation and User Experience topics are frequently asked in LWC interviews because almost every production component requires navigation, notifications, and loading indicators to create a smooth user experience.