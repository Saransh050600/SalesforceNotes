

Every Lightning Web Component (LWC) consists of a folder containing mandatory and optional files.

## Typical Component Structure

```text
accountCard/
│
├── accountCard.html
├── accountCard.js
├── accountCard.js-meta.xml
└── accountCard.css
```

### Required Files

- HTML Template (`.html`)
- JavaScript Controller (`.js`)
- Configuration File (`.js-meta.xml`)

### Optional Files

- CSS File (`.css`)
- SVG Icon (`.svg`)
- Jest Test File (`.test.js`)

---

# HTML File

The HTML file defines the User Interface (UI) of the component.

## Purpose

- Display data
- Render UI elements
- Handle template directives
- Bind JavaScript properties

## Example

### accountCard.html

```html
<template>
    <lightning-card title="Account Information">
        <p>{accountName}</p>
    </lightning-card>
</template>
```

## Important Rules

### Root Element

Every LWC HTML file must have a root `<template>` tag.

```html
<template>
</template>
```

### Data Binding

```html
<p>{accountName}</p>
```

### Event Binding

```html
<lightning-button
    label="Save"
    onclick={handleSave}>
</lightning-button>
```

### Conditional Rendering

```html
<template lwc:if={showData}>
    <p>Visible</p>
</template>
```

### List Rendering

```html
<template for:each={accounts} for:item="acc">
    <p key={acc.Id}>{acc.Name}</p>
</template>
```

---

# JavaScript File

The JavaScript file contains the component logic.

## Purpose

- Business logic
- Event handling
- Data manipulation
- Apex integration
- Lifecycle management

## Basic Structure

### accountCard.js

```javascript
import { LightningElement } from 'lwc';

export default class AccountCard extends LightningElement {

}
```

---

## Import Statements

### Import LightningElement

```javascript
import { LightningElement } from 'lwc';
```

### Import Decorators

```javascript
import { LightningElement, api, wire } from 'lwc';
```

### Import Apex

```javascript
import getAccounts from
'@salesforce/apex/AccountController.getAccounts';
```

### Import Labels

```javascript
import MY_LABEL from
'@salesforce/label/c.MyLabel';
```

---

## Properties

```javascript
accountName = 'Salesforce';
```

Used for storing data.

---

## Methods

```javascript
handleClick() {
    console.log('Clicked');
}
```

Used for implementing business logic.

---

## Decorators

[[05 - Decorators]]
### @api

Public property or method.

```javascript
@api recordId;
```

### @wire

Reactive data retrieval.

```javascript
@wire(getAccounts)
accounts;
```

### @track (Legacy)

Tracks nested object changes.

```javascript
@track employee;
```

---

## Lifecycle Hooks

```javascript
connectedCallback() {

}

renderedCallback() {

}

disconnectedCallback() {

}
```

---

# CSS File

The CSS file controls component styling.

## Purpose

- Component-specific styling
- UI customization
- Layout control

---

## Example

### accountCard.css

```css
.title {
    color: blue;
    font-size: 18px;
}
```

### HTML

```html
<template>
    <h1 class="title">Account</h1>
</template>
```

---

## Scoped CSS

LWC CSS is scoped to the component.

```css
.title {
    color: red;
}
```

Will only affect elements inside that component.

---

## Host Selector

Used to style the component container.

```css
:host {
    display: block;
}
```

---

## Multiple Selectors

```css
.title,
.subtitle {
    font-weight: bold;
}
```

---

## SLDS Usage

Recommended approach.

```html
<div class="slds-p-around_medium">
    Content
</div>
```

---

# Meta XML File

The meta XML file controls where and how the component can be used.

## File Name

```text
componentName.js-meta.xml
```

Example:

```text
accountCard.js-meta.xml
```

---

## Purpose

- Expose component
- Define targets
- Configure component properties
- Control deployment visibility

---

## Basic Structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle
    xmlns="http://soap.sforce.com/2006/04/metadata">

    <apiVersion>65.0</apiVersion>

    <isExposed>true</isExposed>

</LightningComponentBundle>
```

---

## Important Tags

### apiVersion

Defines API version.

```xml
<apiVersion>65.0</apiVersion>
```

---

### isExposed

Controls visibility.

```xml
<isExposed>true</isExposed>
```

If false:

```xml
<isExposed>false</isExposed>
```

Component cannot be added to pages.

---

# Targets

Targets specify where the component can be used.

## Example

```xml
<targets>
    <target>lightning__AppPage</target>
</targets>
```

---

## Common Targets

### App Page

```xml
<target>lightning__AppPage</target>
```

Allows use on Lightning App Pages.

---

### Record Page

```xml
<target>lightning__RecordPage</target>
```

Allows use on Record Pages.

---

### Home Page

```xml
<target>lightning__HomePage</target>
```

Allows use on Home Pages.

---

### Flow Screen

```xml
<target>lightning__FlowScreen</target>
```

Allows use inside Screen Flows.

---

### Utility Bar

```xml
<target>lightning__UtilityBar</target>
```

Allows use in Utility Bar.

---

### Quick Action

```xml
<target>lightning__RecordAction</target>
```

Allows use as Quick Action.

---

## Multiple Targets

```xml
<targets>
    <target>lightning__AppPage</target>
    <target>lightning__RecordPage</target>
    <target>lightning__HomePage</target>
</targets>
```

---

# Target Configs

Target Configs define configuration for specific targets.

---

## Example

```xml
<targetConfigs>

    <targetConfig
        targets="lightning__RecordPage">

        <property
            name="message"
            type="String"
            label="Message" />

    </targetConfig>

</targetConfigs>
```

---

## Purpose

Allows Admins to configure component properties from Lightning App Builder.

---

## Property Example

### Meta XML

```xml
<property
    name="title"
    type="String"
    label="Card Title"/>
```

### JavaScript

```javascript
@api title;
```

### HTML

```html
<h1>{title}</h1>
```

Admin can now provide the title from App Builder.

---

## Supported Property Types

### String

```xml
type="String"
```

### Integer

```xml
type="Integer"
```

### Boolean

```xml
type="Boolean"
```

### Object

```xml
type="Object"
```

---

# Complete Example

## accountCard.js-meta.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle
    xmlns="http://soap.sforce.com/2006/04/metadata">

    <apiVersion>65.0</apiVersion>

    <isExposed>true</isExposed>

    <targets>
        <target>lightning__AppPage</target>
        <target>lightning__RecordPage</target>
        <target>lightning__HomePage</target>
    </targets>

    <targetConfigs>

        <targetConfig
            targets="lightning__AppPage,
                     lightning__RecordPage">

            <property
                name="title"
                type="String"
                label="Card Title"/>

        </targetConfig>

    </targetConfigs>

</LightningComponentBundle>
```

---

# Interview Questions

## Q1: Which files are mandatory in an LWC?

### Answer

- HTML
- JavaScript
- Meta XML

---

## Q2: What is the purpose of the Meta XML file?

### Answer

Controls component visibility, targets, and configuration.

---

## Q3: What does `isExposed=true` mean?

### Answer

The component becomes available in Lightning App Builder, Flow Builder, or other supported targets.

---

## Q4: What is the difference between Targets and Target Configs?

### Answer

| Targets | Target Configs |
|----------|----------|
| Define where a component can be used | Define properties for those targets |
| App Page, Record Page, Flow, etc. | Configurable attributes |
| Controls placement | Controls behavior |

---

## Q5: Can a component have multiple targets?

### Answer

Yes.

```xml
<targets>
    <target>lightning__AppPage</target>
    <target>lightning__RecordPage</target>
</targets>
```

---

# Summary

## HTML

- Defines UI
- Uses template syntax
- Supports directives and events

## JavaScript

- Contains component logic
- Handles events
- Calls Apex
- Uses decorators

## CSS

- Component-scoped styling
- Supports SLDS
- Uses `:host`

## Meta XML

- Controls exposure
- Defines targets
- Configures properties

## Targets

- App Page
- Record Page
- Home Page
- Flow Screen
- Utility Bar
- Quick Action

## Target Configs

- Define configurable properties
- Used in Lightning App Builder
- Exposed through `@api`

Understanding component structure and configuration is essential because every LWC starts with these four files and their configuration.