
Lightning Web Components can be integrated with Salesforce Flows to create powerful, interactive user experiences.

LWC Flow Integration is heavily used in enterprise Salesforce projects for:

- Custom Screen Components
- Dynamic User Interfaces
- Complex Validation
- Multi-Step Wizards
- Custom Navigation Logic

---

# What is LWC + Flow Integration?

Flow can host Lightning Web Components as Screen Components.

```text
Flow Screen
      ↓
LWC Component
      ↓
User Interaction
      ↓
Flow Variables Updated
```

---

# Common Use Cases

- Address Lookup
- Product Selection
- File Upload
- Dynamic Search
- Multi-Select Picklists
- Custom Validation
- Advanced UI Components

---

# Flow Screen Components

A Lightning Web Component can be exposed to Flow Screens.

---

## meta.xml Configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>

<LightningComponentBundle
    xmlns="http://soap.sforce.com/2006/04/metadata">

    <apiVersion>64.0</apiVersion>

    <isExposed>true</isExposed>

    <targets>

        <target>
            lightning__FlowScreen
        </target>

    </targets>

</LightningComponentBundle>
```

---

# Expose Properties to Flow

Use:

```javascript
@api
```

---

## Example

```javascript
import {
    LightningElement,
    api
}
from 'lwc';

export default class AccountSearch
extends LightningElement {

    @api accountName;
}
```

---

# Configure Property in meta.xml

```xml
<targetConfigs>

    <targetConfig
        targets="lightning__FlowScreen">

        <property
            name="accountName"
            type="String"
            label="Account Name"/>

    </targetConfig>

</targetConfigs>
```

---

# Input Variables

Flow passes data into LWC using:

```javascript
@api
```

---

## Flow

```text
Flow Variable
     ↓
LWC Property
```

---

## Example

```javascript
@api recordId;
```

---

Flow sends:

```text
001XXXXXXXXXXXX
```

to component.

---

# Output Variables

LWC sends values back to Flow.

---

## Example

```javascript
@api selectedAccountId;
```

---

## Update Value

```javascript
handleSelect(event){

    this.selectedAccountId =
        event.detail.value;
}
```

---

Flow automatically receives updated value.

---

# Input + Output Example

```javascript
@api accountName;
@api selectedAccountId;
```

---

```text
Flow
 ↓
accountName
 ↓
LWC

LWC
 ↓
selectedAccountId
 ↓
Flow
```

---

# Flow Navigation Events

Flow provides events to control navigation.

Very common interview topic.

---

# Import Events

```javascript
import {

    FlowNavigationNextEvent,
    FlowNavigationBackEvent,
    FlowNavigationPauseEvent,
    FlowNavigationFinishEvent

}

from 'lightning/flowSupport';
```

---

# Next Screen

```javascript
handleNext(){

    this.dispatchEvent(

        new FlowNavigationNextEvent()

    );
}
```

---

# Previous Screen

```javascript
handleBack(){

    this.dispatchEvent(

        new FlowNavigationBackEvent()

    );
}
```

---

# Pause Flow

```javascript
handlePause(){

    this.dispatchEvent(

        new FlowNavigationPauseEvent()

    );
}
```

---

# Finish Flow

```javascript
handleFinish(){

    this.dispatchEvent(

        new FlowNavigationFinishEvent()

    );
}
```

---

# Custom Next Button

### HTML

```html
<lightning-button
    label="Next"
    onclick={handleNext}>
</lightning-button>
```

---

### JavaScript

```javascript
handleNext(){

    this.dispatchEvent(
        new FlowNavigationNextEvent()
    );
}
```

---

# availableActions

Flow provides available navigation actions.

---

## Example

```javascript
@api availableActions;
```

---

## Check Next Availability

```javascript
get canGoNext(){

    return this.availableActions
        .includes('NEXT');
}
```

---

## Check Finish Availability

```javascript
get canFinish(){

    return this.availableActions
        .includes('FINISH');
}
```

---

# Conditional Navigation

```javascript
if(
    this.availableActions
        .includes('NEXT')
){

    this.dispatchEvent(
        new FlowNavigationNextEvent()
    );
}
```

---

# Validation in Flow

One of the most important Flow integration topics.

---

# Why Validation?

Prevent users from moving to next screen if data is invalid.

---

# Flow Validation Method

Flow automatically calls:

```javascript
validate()
```

if implemented.

---

# Example

```javascript
validate(){

    if(!this.accountName){

        return {

            isValid: false,

            errorMessage:
                'Account Name Required'

        };
    }

    return {

        isValid: true
    };
}
```

---

# Validation Response

### Success

```javascript
{
    isValid: true
}
```

---

### Failure

```javascript
{
    isValid: false,

    errorMessage:
        'Required Field'
}
```

---

# Example

```javascript
validate(){

    if(
        !this.selectedAccountId
    ){

        return {

            isValid: false,

            errorMessage:
                'Please select an account'

        };
    }

    return {

        isValid: true
    };
}
```

---

# Custom Input Validation

### HTML

```html
<lightning-input
    label="Account Name"
    onchange={handleChange}>
</lightning-input>
```

---

### JavaScript

```javascript
handleChange(event){

    this.accountName =
        event.target.value;
}
```

---

### Flow Validation

```javascript
validate(){

    const input =

        this.template.querySelector(
            'lightning-input'
        );

    input.reportValidity();

    return {

        isValid:
            input.checkValidity()
    };
}
```

---

# FlowAttributeChangeEvent

Used to notify Flow when a property value changes.

Very frequently asked interview topic.

---

# Import

```javascript
import {

    FlowAttributeChangeEvent

}

from 'lightning/flowSupport';
```

---

# Example

```javascript
handleChange(event){

    this.accountName =
        event.target.value;

    this.dispatchEvent(

        new FlowAttributeChangeEvent(

            'accountName',

            this.accountName

        )

    );
}
```

---

# Why Use FlowAttributeChangeEvent?

Without it:

```text
LWC Changes Value
      ↓
Flow Doesn't Know
```

---

With it:

```text
LWC Changes Value
      ↓
Flow Variable Updated
```

---

# Real Project Example

## Product Selector

```text
Flow Screen
      ↓
LWC Product Search
      ↓
User Selects Product
      ↓
Flow Variable Updated
      ↓
Next Screen
```

---

# Best Practices

## Always

Use:

```javascript
FlowAttributeChangeEvent
```

for output variables.

---

## Validate Before Navigation

Use:

```javascript
validate()
```

---

## Check Navigation Availability

Use:

```javascript
availableActions
```

---

## Keep Flow Variables Simple

Prefer:

```text
String
Boolean
Number
Record Id
```

---

## Use Custom LWC Only When Needed

Prefer standard Flow components when possible.

---

# Common Interview Scenarios

## Scenario 1

Need custom address lookup inside Flow.

### Solution

```text
Flow Screen Component
```

---

## Scenario 2

Need to move to next screen programmatically.

### Solution

```javascript
FlowNavigationNextEvent
```

---

## Scenario 3

Need to prevent user from continuing.

### Solution

```javascript
validate()
```

---

## Scenario 4

Need Flow variable updated immediately.

### Solution

```javascript
FlowAttributeChangeEvent
```

---

## Scenario 5

Need custom Finish button.

### Solution

```javascript
FlowNavigationFinishEvent
```

---

# Interview Questions

## Q1: How do you expose an LWC to Flow?

### Answer

```xml
<target>
    lightning__FlowScreen
</target>
```

---

## Q2: How does Flow pass data into LWC?

### Answer

Using:

```javascript
@api
```

properties.

---

## Q3: How does LWC return values to Flow?

### Answer

Using:

```javascript
@api
```

and

```javascript
FlowAttributeChangeEvent
```

---

## Q4: Which event moves Flow to the next screen?

### Answer

```javascript
FlowNavigationNextEvent
```

---

## Q5: Which event moves Flow to the previous screen?

### Answer

```javascript
FlowNavigationBackEvent
```

---

## Q6: How do you finish a Flow from LWC?

### Answer

```javascript
FlowNavigationFinishEvent
```

---

## Q7: What is availableActions?

### Answer

A Flow-provided array containing supported navigation actions.

Example:

```javascript
['NEXT','BACK']
```

---

## Q8: How do you validate an LWC inside Flow?

### Answer

Implement:

```javascript
validate()
```

---

## Q9: What does validate() return?

### Answer

```javascript
{
    isValid: true
}
```

or

```javascript
{
    isValid: false,
    errorMessage: 'Error'
}
```

---

## Q10: What is FlowAttributeChangeEvent?

### Answer

Updates Flow variables when LWC values change.

---

# Summary

## Flow Screen Components

```xml
lightning__FlowScreen
```

- Embed LWC in Flow

---

## Input/Output Variables

```javascript
@api
```

- Flow → LWC
- LWC → Flow

---

## Navigation Events

```javascript
FlowNavigationNextEvent
FlowNavigationBackEvent
FlowNavigationPauseEvent
FlowNavigationFinishEvent
```

- Programmatic Navigation

---

## Validation

```javascript
validate()
```

- Prevent Invalid Navigation
- Return Error Messages

---

## FlowAttributeChangeEvent

```javascript
FlowAttributeChangeEvent
```

- Sync LWC Values with Flow Variables

LWC + Flow Integration is one of the most valuable Salesforce development skills because it combines Flow's declarative power with LWC's flexibility, enabling highly interactive and scalable business processes.