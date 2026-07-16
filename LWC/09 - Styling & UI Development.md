
Styling is an important part of Lightning Web Components (LWC) because it controls the appearance, layout, and user experience of components.

LWC provides several ways to style components:

- CSS
- Scoped CSS
- Salesforce Lightning Design System (SLDS)
- Styling Hooks
- Static Resources

---

# CSS in LWC

CSS (Cascading Style Sheets) is used to control the visual appearance of a component.

Each LWC can have its own CSS file.

---

# Component Structure

```text
accountCard/
│
├── accountCard.html
├── accountCard.js
├── accountCard.css
└── accountCard.js-meta.xml
```

---

# Basic CSS Example

## HTML

```html
<template>

    <h1 class="title">
        Welcome
    </h1>

</template>
```

---

## CSS

```css
.title {

    color: blue;
    font-size: 24px;

}
```

---

## Output

```text
Welcome
```

Displayed in blue with larger text.

---

# Applying Multiple Classes

## HTML

```html
<div class="card active">
    Account Details
</div>
```

---

## CSS

```css
.card {

    padding: 10px;

}

.active {

    border: 1px solid black;

}
```

---

# CSS Selectors

---

## Class Selector

```css
.title {

}
```

---

## Multiple Classes

```css
.title,
.subtitle {

}
```

---

## Descendant Selector

```css
.card p {

}
```

---

## Attribute Selector

```css
[data-id="name"] {

}
```

---

# Scoped CSS

One of the biggest advantages of LWC styling.

---

# What is Scoped CSS?

CSS defined in a component only affects that component.

---

## Example

### Component A CSS

```css
.title {

    color: red;

}
```

---

### Component A HTML

```html
<h1 class="title">
    Account
</h1>
```

---

### Component B HTML

```html
<h1 class="title">
    Contact
</h1>
```

Component B will NOT be affected.

---

# Why Scoped CSS?

Benefits:

- No CSS conflicts
- Better maintainability
- Safer component development
- Easier debugging

---

# CSS Isolation

```text
Component A CSS
      ↓
Only Component A

Component B CSS
      ↓
Only Component B
```

---

# :host Selector

Styles the root element of the component.

---

## Example

```css
:host {

    display: block;

}
```

---

# Example

```css
:host {

    background-color: lightgray;
    padding: 10px;

}
```

Affects the component container itself.

---

# Conditional Host Styling

```css
:host(.active) {

    border: 2px solid blue;

}
```

---

# Styling Child Elements

```css
:host {

    display: block;

}

.title {

    color: red;

}
```

---

# Salesforce Lightning Design System (SLDS)

SLDS is Salesforce's official design framework.

It provides prebuilt styling classes and design patterns.

---

# Why Use SLDS?

Benefits:

- Salesforce look and feel
- Responsive design
- Accessibility support
- Consistent UI

---

# Example

```html
<div class="slds-box">

    Account Information

</div>
```

---

# Common SLDS Utility Classes

---

## Margin

```html
<div class="slds-m-around_medium">
```

---

## Padding

```html
<div class="slds-p-around_medium">
```

---

## Text Alignment

```html
<div class="slds-text-align_center">
```

---

## Font Weight

```html
<div class="slds-text-title_bold">
```

---

## Grid Layout

```html
<div class="slds-grid">
```

---

# SLDS Card Example

```html
<div class="slds-card">

    <div class="slds-card__body">

        Account Details

    </div>

</div>
```

---

# SLDS Grid System

---

## Two Columns

```html
<div class="slds-grid">

    <div class="slds-col">
        Column 1
    </div>

    <div class="slds-col">
        Column 2
    </div>

</div>
```

---

# Responsive Layout

```html
<div class="slds-size_1-of-2">
```

50% width.

---

# Commonly Used SLDS Classes

| Class | Purpose |
|---------|---------|
| slds-box | Container |
| slds-card | Card Layout |
| slds-grid | Grid Layout |
| slds-col | Grid Column |
| slds-text-heading_medium | Heading |
| slds-p-around_medium | Padding |
| slds-m-around_medium | Margin |
| slds-align_absolute-center | Center Content |

---

# Styling Hooks

Styling Hooks allow customization of Lightning Base Components.

---

# Why Styling Hooks?

Normally:

```html
<lightning-button>
```

cannot be styled internally.

Styling Hooks provide supported customization.

---

# Syntax

```css
--slds-c-button-brand-color-background
```

---

# Example

## HTML

```html
<lightning-button
    variant="brand"
    label="Save">
</lightning-button>
```

---

## CSS

```css
:host {

    --slds-c-button-brand-color-background: green;

}
```

---

# Common Styling Hooks

---

## Brand Button Background

```css
--slds-c-button-brand-color-background
```

---

## Brand Button Border

```css
--slds-c-button-brand-color-border
```

---

## Input Border

```css
--slds-c-input-color-border
```

---

## Input Background

```css
--slds-c-input-color-background
```

---

# Example

```css
:host {

    --slds-c-input-color-border: red;

}
```

---

# Advantages of Styling Hooks

- Official Salesforce support
- Upgrade safe
- No DOM hacking
- Works with base components

---

# Static Resources

Static Resources allow storing external files inside Salesforce.

---

# Common Static Resources

```text
CSS Files
JavaScript Files
Images
Fonts
Zip Files
Libraries
```

---

# Example

```text
logo.png
chartjs.zip
custom.css
```

---

# Creating a Static Resource

```text
Setup
    ↓
Static Resources
    ↓
New
```

Upload file.

---

# Importing Static Resources

---

## Image Example

```javascript
import LOGO
from '@salesforce/resourceUrl/logo';
```

---

## HTML

```html
<img src={logoUrl}>
```

---

## JavaScript

```javascript
logoUrl = LOGO;
```

---

# JavaScript Library Example

```javascript
import CHARTJS
from '@salesforce/resourceUrl/chartjs';
```

---

# loadScript()

Used for loading external JavaScript files.

---

## Example

```javascript
import {
    loadScript
}
from 'lightning/platformResourceLoader';
```

---

```javascript
connectedCallback() {

    loadScript(
        this,
        CHARTJS
    )
    .then(() => {

        console.log(
            'Loaded'
        );

    });
}
```

---

# loadStyle()

Used for loading CSS files.

---

## Example

```javascript
import {
    loadStyle
}
from 'lightning/platformResourceLoader';
```

---

```javascript
loadStyle(
    this,
    MY_CSS
);
```

---

# Loading Multiple Resources

```javascript
Promise.all([

    loadScript(
        this,
        LIBRARY_ONE
    ),

    loadStyle(
        this,
        STYLE_ONE
    )

]);
```

---

# Static Resource Use Cases

---

## Images

```html
Logo
Icons
Banners
```

---

## Libraries

```text
ChartJS
MomentJS
D3JS
```

---

## Custom Styles

```text
Fonts
Theme CSS
Animations
```

---

# Styling Best Practices

---

## Prefer SLDS

Recommended:

```html
slds-card
slds-box
slds-grid
```

Instead of custom CSS whenever possible.

---

## Use Scoped CSS

Keep styles component-specific.

---

## Use Styling Hooks

For Lightning Base Components.

Avoid unsupported CSS overrides.

---

## Load Large Libraries Through Static Resources

Examples:

```text
ChartJS
D3JS
FullCalendar
```

---

## Use :host for Container Styling

```css
:host {

    display: block;

}
```

---

# Real Interview Scenarios

## Scenario 1

Need Salesforce-style UI.

### Solution

```text
SLDS
```

---

## Scenario 2

Need custom button colors.

### Solution

```text
Styling Hooks
```

---

## Scenario 3

Need ChartJS.

### Solution

```text
Static Resource
+
loadScript()
```

---

## Scenario 4

Need component-specific styling.

### Solution

```text
Scoped CSS
```

---

## Scenario 5

Need styling on root component.

### Solution

```css
:host
```

---

# Interview Questions

## Q1: What is Scoped CSS?

**Answer**

CSS that only affects the component where it is defined.

---

## Q2: What is the purpose of :host?

**Answer**

Styles the root element of the component.

---

## Q3: What is SLDS?

**Answer**

Salesforce Lightning Design System, the official Salesforce design framework.

---

## Q4: Why should SLDS be preferred?

**Answer**

Provides:

- Consistency
- Responsiveness
- Accessibility
- Salesforce look and feel

---

## Q5: What are Styling Hooks?

**Answer**

Supported CSS variables used to customize Lightning Base Components.

---

## Q6: What is a Static Resource?

**Answer**

A Salesforce repository for storing:

- Images
- CSS
- JavaScript
- Third-party libraries

---

## Q7: Difference between loadScript and loadStyle?

| Method | Purpose |
|----------|----------|
| loadScript() | Load JavaScript |
| loadStyle() | Load CSS |

---

## Q8: How do you load ChartJS in LWC?

**Answer**

1. Upload as Static Resource.
2. Import using `@salesforce/resourceUrl`.
3. Load using `loadScript()`.

---

# Summary

## CSS

- Component styling
- Class selectors
- Attribute selectors

---

## Scoped CSS

- Component isolation
- Prevents CSS conflicts

---

## SLDS

- Salesforce design framework
- Responsive UI
- Accessibility support

---

## Styling Hooks

- Customize base components
- Supported by Salesforce
- Upgrade safe

---

## Static Resources

- Images
- JavaScript libraries
- CSS files
- Fonts

---

## Important APIs

```javascript
loadScript()
loadStyle()
```

---

## Important CSS Features

```css
:host
```

```css
--slds-c-*
```

Styling & UI Development is a critical LWC topic because every Salesforce application requires consistent, responsive, and maintainable user interfaces.