
## What is LWC?

Lightning Web Components (LWC) is Salesforce's modern UI framework for building reusable, fast, and lightweight components on the Salesforce platform.

LWC is built on modern web standards such as:

- HTML
- JavaScript (ES6+)
- CSS
- Custom Elements
- Shadow DOM
- Modules

Instead of using a proprietary framework, Salesforce leverages native browser capabilities to improve performance and developer experience.

### Key Features

- Component-based architecture
- Better performance than Aura
- Reusable components
- Modern JavaScript support
- Built-in security
- Seamless Salesforce integration

### Example

**helloWorld.html**

```html
<template>
    <h1>Hello World</h1>
</template>
```

**helloWorld.js**

```javascript
import { LightningElement } from 'lwc';

export default class HelloWorld extends LightningElement {}
```

---

# Aura vs LWC

## Aura Framework

Aura was Salesforce's first component framework.

Example:

```javascript
({
    doInit : function(component, event, helper) {
        console.log('Aura Component');
    }
})
```

### Advantages

- Mature framework
- Supports legacy implementations
- Works with older Salesforce applications

### Disadvantages

- Slower performance
- Proprietary framework
- More complex syntax
- Larger bundle size

---

## Lightning Web Components (LWC)

Example:

```javascript
import { LightningElement } from 'lwc';

export default class Demo extends LightningElement {}
```

### Advantages

- Faster rendering
- Uses modern web standards
- Smaller footprint
- Better maintainability
- Improved developer experience

### Disadvantages

- Some Aura features require workarounds
- Learning modern JavaScript is necessary

---

## Aura vs LWC Comparison

| Feature | Aura | LWC |
|----------|----------|----------|
| Performance | Slower | Faster |
| Framework Type | Proprietary | Web Standards |
| Learning Curve | Higher | Lower |
| Rendering | Framework Managed | Browser Optimized |
| JavaScript Support | Limited | ES6+ |
| Development Experience | Complex | Simpler |
| Future Investment | Limited | Salesforce Recommended |

### Interview Question

**Q: Which framework should be used for new development?**

**Answer:** Salesforce recommends LWC for all new development unless a specific Aura-only feature is required.

---

# Web Components

## What are Web Components?

Web Components are a set of browser standards used to create reusable UI elements.

LWC is built on these standards.

### Main Pillars

1. Custom Elements
2. Shadow DOM
3. HTML Templates
4. ES Modules

---

## Custom Elements

Custom HTML tags created by developers.

Example:

```html
<c-account-list></c-account-list>
```

Here:

```html
<c-account-list>
```

represents a custom component.

---

## HTML Templates

Templates define the component UI.

Example:

```html
<template>
    <p>Account Name</p>
</template>
```

---

## ES Modules

Used to import and export functionality.

Example:

```javascript
import { LightningElement } from 'lwc';
```

---

## Benefits of Web Components

- Reusability
- Encapsulation
- Maintainability
- Better performance
- Native browser support

---

# Shadow DOM

## What is Shadow DOM?

Shadow DOM provides encapsulation by isolating a component's DOM and styles from the rest of the page.

Each LWC component gets its own DOM boundary.

### Benefits

- CSS isolation
- DOM encapsulation
- Prevents style conflicts
- Improves maintainability

---

## Example

Component CSS:

```css
.title {
    color: red;
}
```

Component HTML:

```html
<template>
    <h1 class="title">Welcome</h1>
</template>
```

The CSS applies only to that component.

Other components cannot accidentally override it.

---

## Shadow DOM Types

### Native Shadow DOM

Provided directly by browsers.

### Synthetic Shadow DOM

Salesforce implementation used for compatibility across browsers.

---

## Interview Question

**Q: Why can't we directly access another component's DOM?**

**Answer:** Because Shadow DOM enforces component encapsulation and isolation.

---

# Component Lifecycle 
[[07 - Lifecycle Hooks & DOM Access]]

Lifecycle hooks allow execution of code at specific stages of a component's existence.

---

## Lifecycle Flow

```text
constructor()
      ↓
connectedCallback()
      ↓
render()
      ↓
renderedCallback()
      ↓
disconnectedCallback()
```

---

## constructor()

Called when component instance is created.

### Example

```javascript
constructor() {
    super();
    console.log('Constructor');
}
```

### Use Cases

- Initialize variables
- Setup default values

### Restrictions

- Cannot access DOM

---

## connectedCallback()

Called when component is inserted into the DOM.

### Example

```javascript
connectedCallback() {
    console.log('Component Loaded');
}
```

### Common Uses

- Call Apex
- Subscribe to LMS
- Initialize data

---

## renderedCallback()

Called after rendering completes.

### Example

```javascript
renderedCallback() {
    console.log('Rendered');
}
```

### Common Uses

- DOM manipulation
- Third-party library initialization

### Important

Avoid updating tracked properties inside this method to prevent infinite rerendering.

---

## disconnectedCallback()

Called when component is removed from DOM.

### Example

```javascript
disconnectedCallback() {
    console.log('Removed');
}
```

### Common Uses

- Cleanup logic
- Unsubscribe LMS
- Remove event listeners

---

## errorCallback()

Captures errors from child components.

### Example

```javascript
errorCallback(error, stack) {
    console.error(error);
}
```

### Use Cases

- Error handling
- Logging
- Display fallback UI

---

# Lifecycle Interview Questions

## Q1: Which hook is called first?

**Answer:**

```text
constructor()
```

---

## Q2: Which hook is used to call Apex?

**Answer:**

```text
connectedCallback()
```

---

## Q3: Which hook is used for DOM manipulation?

**Answer:**

```text
renderedCallback()
```

---

## Q4: Which hook is used for cleanup?

**Answer:**

```text
disconnectedCallback()
```

---

## Q5: Can we access DOM inside constructor?

**Answer:**

No.

DOM is not available during constructor execution.

---

# Summary

## LWC

- Modern Salesforce UI framework
- Built on web standards
- Component-based architecture

## Aura vs LWC

- LWC is faster and recommended
- Aura mainly exists for legacy implementations

## Web Components

- Custom Elements
- Shadow DOM
- Templates
- Modules

## Shadow DOM

- Encapsulation
- Style isolation
- DOM isolation

## Lifecycle Hooks

- constructor()
- connectedCallback()
- renderedCallback()
- disconnectedCallback()
- errorCallback()

These concepts form the foundation of all Lightning Web Component development and are among the most frequently asked topics in Salesforce interviews.

