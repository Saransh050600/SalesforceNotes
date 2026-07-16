
Lifecycle Hooks are special methods that execute automatically at different stages of a component's lifecycle.

They allow developers to:

- Initialize data
- Call Apex methods
- Subscribe to LMS
- Access the DOM
- Clean up resources
- Handle errors

---

# Component Lifecycle Flow

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

If an error occurs in a child component:

```text
errorCallback()
```

is invoked.

---

# Lifecycle Hook Order

```text
1. constructor()
2. connectedCallback()
3. render()
4. renderedCallback()
5. disconnectedCallback()
```

---

# constructor()

The constructor is called when the component instance is created.

It executes before the component is inserted into the DOM.

---

## Syntax

```javascript
constructor() {
    super();
}
```

---

## Example

```javascript
import { LightningElement } from 'lwc';

export default class Demo extends LightningElement {

    constructor() {

        super();

        console.log('Constructor Called');
    }
}
```

---

## Use Cases

- Initialize variables
- Setup default values
- Basic configuration

---

## Restrictions

### Cannot Access DOM

❌

```javascript
constructor() {

    super();

    this.template.querySelector('div');
}
```

This will fail because the DOM is not available yet.

---

### Must Call super()

```javascript
super();
```

must always be the first statement.

---

# connectedCallback()

Called when the component is inserted into the DOM.

This is one of the most commonly used lifecycle hooks.

---

## Syntax

```javascript
connectedCallback() {

}
```

---

## Example

```javascript
connectedCallback() {

    console.log(
        'Component Added'
    );
}
```

---

## Use Cases

### Call Apex

```javascript
connectedCallback() {

    this.loadAccounts();
}
```

---

### Subscribe to LMS

```javascript
connectedCallback() {

    this.subscribeChannel();
}
```

---

### Initialize Component Data

```javascript
connectedCallback() {

    this.status = 'Loaded';
}
```

---

## Important Notes

### DOM is Not Fully Rendered Yet

Avoid:

```javascript
this.template.querySelector();
```

inside `connectedCallback()`.

---

# renderedCallback()

Called after the component has finished rendering.

This hook may execute multiple times.

---

## Syntax

```javascript
renderedCallback() {

}
```

---

## Example

```javascript
renderedCallback() {

    console.log(
        'Rendered'
    );
}
```

---

# When Does renderedCallback Execute?

```text
Initial Render
      ↓
renderedCallback()

Property Changes
      ↓
Rerender
      ↓
renderedCallback()
```

---

# Use Cases

### DOM Manipulation

```javascript
renderedCallback() {

    const input =
        this.template.querySelector(
            'lightning-input'
        );
}
```

---

### Third-Party Libraries

```javascript
renderedCallback() {

    if(this.libraryLoaded) {
        return;
    }

    this.libraryLoaded = true;

    loadScript();
}
```

---

### Dynamic Styling

```javascript
renderedCallback() {

    const element =
        this.template.querySelector(
            '.box'
        );

    element.classList.add(
        'active'
    );
}
```

---

# Avoid Infinite Loops

Bad Example:

```javascript
renderedCallback() {

    this.counter++;
}
```

This causes:

```text
renderedCallback()
       ↓
Property Changes
       ↓
Rerender
       ↓
renderedCallback()
       ↓
Infinite Loop
```

---

## Recommended Pattern

```javascript
hasRendered = false;

renderedCallback() {

    if(this.hasRendered) {
        return;
    }

    this.hasRendered = true;

    console.log(
        'Run Once'
    );
}
```

---

# disconnectedCallback()

Called when the component is removed from the DOM.

---

## Syntax

```javascript
disconnectedCallback() {

}
```

---

## Example

```javascript
disconnectedCallback() {

    console.log(
        'Component Removed'
    );
}
```

---

# Use Cases

### LMS Cleanup

```javascript
disconnectedCallback() {

    unsubscribe(
        this.subscription
    );
}
```

---

### Remove Event Listeners

```javascript
window.removeEventListener(
    'resize',
    this.handleResize
);
```

---

### Clear Timers

```javascript
clearInterval(
    this.intervalId
);
```

---

# Why Cleanup Matters

Without cleanup:

```text
Memory Leaks
Duplicate Events
Performance Issues
```

may occur.

---

# errorCallback()

Called when a child component throws an error.

Acts like an error boundary.

---

## Syntax

```javascript
errorCallback(
    error,
    stack
) {

}
```

---

## Example

```javascript
errorCallback(
    error,
    stack
) {

    console.error(error);

    console.error(stack);
}
```

---

# Parameters

| Parameter | Description |
|------------|------------|
| error | Actual Error |
| stack | Stack Trace |

---

# Use Cases

- Error Logging
- Monitoring
- User Friendly Messages
- Fallback UI

---

## Example

```javascript
errorCallback(
    error,
    stack
) {

    this.hasError = true;
}
```

HTML:

```html
<template lwc:if={hasError}>
    Something went wrong.
</template>
```

---

# Lifecycle Hook Comparison

| Hook | Purpose |
|---------|---------|
| constructor | Component Initialization |
| connectedCallback | Component Added to DOM |
| renderedCallback | Component Rendered |
| disconnectedCallback | Component Removed |
| errorCallback | Child Error Handling |

---

# DOM Access

DOM Access means interacting with HTML elements from JavaScript.

---

# querySelector()

Returns the first matching element.

---

## Syntax

```javascript
this.template.querySelector(
    'selector'
);
```

---

## Example

### HTML

```html
<template>

    <lightning-input
        label="Name">
    </lightning-input>

</template>
```

### JavaScript

```javascript
const input =
    this.template.querySelector(
        'lightning-input'
    );
```

---

# Access Using Class

### HTML

```html
<div class="box">
</div>
```

### JavaScript

```javascript
const box =
    this.template.querySelector(
        '.box'
    );
```

---

# Access Using Data Attributes

### HTML

```html
<lightning-input
    data-id="name">
</lightning-input>
```

### JavaScript

```javascript
const input =
    this.template.querySelector(
        '[data-id="name"]'
    );
```

---

# querySelectorAll()

Returns all matching elements.

---

## Example

### HTML

```html
<lightning-input></lightning-input>
<lightning-input></lightning-input>
<lightning-input></lightning-input>
```

### JavaScript

```javascript
const inputs =
    this.template.querySelectorAll(
        'lightning-input'
    );
```

---

# Iterating Elements

```javascript
inputs.forEach(input => {

    input.reportValidity();

});
```

---

# Common Validation Example

### HTML

```html
<lightning-input
    required>
</lightning-input>

<lightning-input
    required>
</lightning-input>
```

---

### JavaScript

```javascript
validate() {

    const inputs =
        this.template.querySelectorAll(
            'lightning-input'
        );

    let isValid = true;

    inputs.forEach(input => {

        if(!input.reportValidity()) {
            isValid = false;
        }

    });

    return isValid;
}
```

---

# When Can querySelector Be Used?

| Lifecycle Hook | Safe? |
|----------------|--------|
| constructor | ❌ |
| connectedCallback | ⚠️ Usually No |
| renderedCallback | ✅ |
| Event Handlers | ✅ |
| Public Methods | ✅ |

---

# Best Practices

## Use renderedCallback for DOM Access

```javascript
renderedCallback() {

    const element =
        this.template.querySelector(
            '.box'
        );
}
```

---

## Use Data Attributes

Preferred:

```html
data-id="accountName"
```

Over:

```html
class="randomClass"
```

---

## Avoid DOM Manipulation When Possible

Prefer:

```html
lwc:if
```

and reactive properties over direct DOM changes.

---

# Real Interview Scenarios

## Scenario 1

Need to call Apex when component loads.

### Solution

```javascript
connectedCallback()
```

---

## Scenario 2

Need to initialize ChartJS.

### Solution

```javascript
renderedCallback()
```

---

## Scenario 3

Need to unsubscribe LMS.

### Solution

```javascript
disconnectedCallback()
```

---

## Scenario 4

Need to validate all inputs.

### Solution

```javascript
querySelectorAll()
```

---

## Scenario 5

Need to catch child component errors.

### Solution

```javascript
errorCallback()
```

---

# Interview Questions

## Q1: What is the lifecycle hook execution order?

**Answer**

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

## Q2: Which hook is best for Apex calls?

**Answer**

```javascript
connectedCallback()
```

---

## Q3: Which hook is best for DOM manipulation?

**Answer**

```javascript
renderedCallback()
```

---

## Q4: Can querySelector be used inside constructor?

**Answer**

No.

DOM is not available yet.

---

## Q5: Why does renderedCallback execute multiple times?

**Answer**

Because every rerender triggers it.

---

## Q6: What causes infinite loops in renderedCallback?

**Answer**

Updating reactive properties inside the hook.

---

## Q7: What is the purpose of disconnectedCallback?

**Answer**

Cleanup logic:

- LMS unsubscribe
- Event listener removal
- Timer cleanup

---

## Q8: What is errorCallback?

**Answer**

A lifecycle hook that catches errors from child components.

---

# Summary

## Lifecycle Hooks

### constructor()

- Component creation
- Initialize variables

### connectedCallback()

- Component inserted into DOM
- Apex calls
- LMS subscriptions

### renderedCallback()

- DOM available
- DOM manipulation
- Third-party libraries

### disconnectedCallback()

- Cleanup
- Unsubscribe LMS
- Remove listeners

### errorCallback()

- Child component error handling

---

## DOM Access

### querySelector()

Returns first matching element.

### querySelectorAll()

Returns all matching elements.

### Best Place to Use

- renderedCallback()
- Event Handlers
- Public Methods

Lifecycle Hooks and DOM Access are among the most commonly asked LWC interview topics because they control component initialization, rendering, cleanup, and interaction with the UI.