
Forms are used to create, edit, and save Salesforce records while validation ensures users enter correct data before submission.

LWC provides several built-in form components that significantly reduce development effort.

---

# Form Components Overview

| Component | Purpose |
|------------|------------|
| lightning-record-form | Quick Create/Edit Form |
| lightning-record-edit-form | Customizable Form |
| lightning-record-view-form | Read-Only Display |
| lightning-input-field | Salesforce Field |
| lightning-input | Custom Input |
| lightning-messages | Display Validation Errors |

---

# lightning-record-form

Simplest way to create forms.

Provides:

- Create Record
- Edit Record
- View Record
- Automatic Validation
- Automatic Save

No Apex required.

---

## Create Record Example

```html
<lightning-record-form
    object-api-name="Account"
    fields={fields}>
</lightning-record-form>
```

---

## JavaScript

```javascript
import NAME_FIELD
from '@salesforce/schema/Account.Name';

import INDUSTRY_FIELD
from '@salesforce/schema/Account.Industry';

fields = [
    NAME_FIELD,
    INDUSTRY_FIELD
];
```

---

## Edit Existing Record

```html
<lightning-record-form
    record-id={recordId}
    object-api-name="Account"
    fields={fields}>
</lightning-record-form>
```

---

## Modes

### View Mode

```html
mode="view"
```

---

### Edit Mode

```html
mode="edit"
```

---

### Read Only

```html
mode="readonly"
```

---

## Benefits

- Minimal code
- Automatic validation
- Automatic save
- Uses standard Salesforce UI
- Supports FLS and Sharing

---

## Limitations

- Limited customization
- Fixed layout behavior
- Cannot easily control field rendering

---

# lightning-record-edit-form

Provides complete control over form layout.

Most commonly used in real projects.

---

## Basic Example

### HTML

```html
<lightning-record-edit-form
    object-api-name="Account">

    <lightning-input-field
        field-name="Name">
    </lightning-input-field>

    <lightning-input-field
        field-name="Industry">
    </lightning-input-field>

    <lightning-button
        type="submit"
        label="Save">
    </lightning-button>

</lightning-record-edit-form>
```

---

## Why Use It?

Allows:

- Custom Layout
- Custom Buttons
- Custom Validation
- Event Handling
- Dynamic Forms

---

# Form Events

---

## onload

Triggered when form loads.

```html
onload={handleLoad}
```

```javascript
handleLoad(event) {
    console.log('Loaded');
}
```

---

## onsubmit

Triggered before record save.

```html
onsubmit={handleSubmit}
```

```javascript
handleSubmit(event) {

    event.preventDefault();

    const fields =
        event.detail.fields;

    this.template
        .querySelector(
            'lightning-record-edit-form'
        )
        .submit(fields);
}
```

---

## onsuccess

Triggered after successful save.

```html
onsuccess={handleSuccess}
```

```javascript
handleSuccess(event) {

    console.log(
        event.detail.id
    );
}
```

---

## onerror

Triggered when save fails.

```html
onerror={handleError}
```

```javascript
handleError(event) {

    console.error(
        event.detail
    );
}
```

---

# lightning-input-field

Used inside:

```html
<lightning-record-edit-form>
```

Automatically handles:

- Validation Rules
- Required Fields
- Picklists
- Lookup Fields
- Field Security

---

## Example

```html
<lightning-input-field
    field-name="Name">
</lightning-input-field>
```

---

# Input Validation

Validation ensures data entered is correct before save.

---

## Required Field

```html
<lightning-input
    label="Email"
    required>
</lightning-input>
```

---

## Check Validity

```javascript
const input =
    this.template.querySelector(
        'lightning-input'
    );

input.checkValidity();
```

Returns:

```javascript
true
false
```

---

## Show Validation Message

```javascript
input.reportValidity();
```

Displays Salesforce validation message.

---

## Example

```javascript
const allValid =
[
    ...this.template.querySelectorAll(
        'lightning-input'
    )
].reduce((valid,input) => {

    input.reportValidity();

    return valid &&
           input.checkValidity();

}, true);
```

---

# Validating Multiple Inputs

```javascript
validateInputs() {

    return [
        ...this.template.querySelectorAll(
            'lightning-input'
        )
    ].every(input => {

        input.reportValidity();

        return input.checkValidity();

    });
}
```

---

# Custom Validation

Used when standard validation is not enough.

---

## setCustomValidity()

Adds custom error message.

```javascript
input.setCustomValidity(
    'Age must be above 18'
);
```

---

## Clear Custom Error

```javascript
input.setCustomValidity('');
```

---

## Display Error

```javascript
input.reportValidity();
```

---

# Example

### HTML

```html
<lightning-input
    type="number"
    label="Age"
    onchange={handleAge}>
</lightning-input>
```

---

### JavaScript

```javascript
handleAge(event) {

    const age =
        event.target.value;

    if(age < 18){

        event.target
            .setCustomValidity(
                'Age must be at least 18'
            );

    } else {

        event.target
            .setCustomValidity('');
    }

    event.target
        .reportValidity();
}
```

---

# Email Validation Example

```javascript
validateEmail(event) {

    const email =
        event.target.value;

    const regex =
        /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

    if(!regex.test(email)) {

        event.target
            .setCustomValidity(
                'Invalid Email'
            );

    } else {

        event.target
            .setCustomValidity('');
    }

    event.target
        .reportValidity();
}
```

---

# Form Submission with Validation

```javascript
handleSave() {

    const isValid =
        this.validateInputs();

    if(!isValid) {

        return;
    }

    // Continue Save Logic
}
```

---

# lightning-messages

Displays validation and DML errors.

---

## Example

```html
<lightning-record-edit-form
    object-api-name="Account">

    <lightning-messages>
    </lightning-messages>

    <lightning-input-field
        field-name="Name">
    </lightning-input-field>

</lightning-record-edit-form>
```

---

# record-form vs record-edit-form

| Feature | record-form | record-edit-form |
|----------|-------------|------------------|
| Quick Setup | ✅ | ❌ |
| Custom Layout | ❌ | ✅ |
| Custom Validation | ❌ | ✅ |
| Event Handling | Limited | Full |
| Production Usage | Medium | Very High |

---

# Real Interview Scenarios

## Scenario 1

Need a quick Account creation form.

### Solution

```html
<lightning-record-form>
```

---

## Scenario 2

Need custom layout with validation.

### Solution

```html
<lightning-record-edit-form>
```

---

## Scenario 3

Need age validation.

### Solution

```javascript
setCustomValidity()
```

---

## Scenario 4

Need to validate all inputs before save.

### Solution

```javascript
checkValidity()
reportValidity()
```

---

## Scenario 5

Need Salesforce validation errors displayed automatically.

### Solution

```html
<lightning-messages>
```

---

# Interview Questions

## Q1: Difference between lightning-record-form and lightning-record-edit-form?

| lightning-record-form | lightning-record-edit-form |
|----------------------|----------------------------|
| Quick Setup | Custom Layout |
| Minimal Code | More Flexible |
| Limited Control | Full Control |

---

## Q2: Which form component is most commonly used in real projects?

### Answer

```html
<lightning-record-edit-form>
```

Because it provides complete control over UI and validation.

---

## Q3: What is the purpose of lightning-input-field?

### Answer

Automatically renders Salesforce fields and supports:

- Validation Rules
- Required Fields
- Picklists
- Lookups
- FLS

---

## Q4: What is the difference between checkValidity() and reportValidity()?

### Answer

```javascript
checkValidity()
```

Returns:

```javascript
true / false
```

---

```javascript
reportValidity()
```

Displays error messages to users.

---

## Q5: What does setCustomValidity() do?

### Answer

Sets a custom validation error message.

```javascript
input.setCustomValidity(
    'Custom Error'
);
```

---

## Q6: How do you clear a custom validation error?

### Answer

```javascript
input.setCustomValidity('');
```

---

## Q7: Which event is fired after a successful save?

### Answer

```html
onsuccess
```

---

## Q8: Which event is fired before save?

### Answer

```html
onsubmit
```

---

# Summary

## lightning-record-form

- Quick form creation
- Automatic save
- Automatic validation
- Less customization

---

## lightning-record-edit-form

- Most used in projects
- Custom layouts
- Event handling
- Custom validation

---

## Validation Methods

```javascript
checkValidity()
reportValidity()
setCustomValidity()
```

---

## Important Events

```html
onload
onsubmit
onsuccess
onerror
```

---

## Interview Focus Areas

- record-form vs record-edit-form
- lightning-input-field
- checkValidity()
- reportValidity()
- setCustomValidity()
- form events
- custom validation
- lightning-messages