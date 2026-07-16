
Event Handling is the mechanism through which components respond to user actions and communicate with other components.

Examples of events:

- Button Click
- Input Change
- Form Submit
- Custom Component Events

Event handling is one of the most important concepts in LWC because it drives user interaction and component communication.

---

# What is an Event?

An event is an action or occurrence detected by the browser.

Examples:

```text
Click
Change
Focus
Blur
Submit
Key Press
```

When an event occurs, JavaScript can execute a handler method.

---

# Event Flow

```text
User Action
      ↓
Event Generated
      ↓
Event Handler Executes
      ↓
Business Logic Runs
```

---

# Standard Events

Standard Events are browser or Lightning Base Component events that are already available.

---

## Common Standard Events

| Event | Description |
|----------|----------|
| onclick | Button Click |
| onchange | Value Changed |
| onfocus | Input Focus |
| onblur | Input Lost Focus |
| onsubmit | Form Submission |
| onkeyup | Key Released |
| onkeydown | Key Pressed |

---

# Click Event

## HTML

```html
<lightning-button
    label="Click Me"
    onclick={handleClick}>
</lightning-button>
```

---

## JavaScript

```javascript
handleClick() {

    console.log(
        'Button Clicked'
    );
}
```

---

# Change Event

Triggered when an input value changes.

---

## HTML

```html
<lightning-input
    label="Name"
    onchange={handleChange}>
</lightning-input>
```

---

## JavaScript

```javascript
handleChange(event) {

    console.log(
        event.target.value
    );
}
```

---

# Input Example

### HTML

```html
<lightning-input
    label="Account Name"
    onchange={handleNameChange}>
</lightning-input>
```

---

### JavaScript

```javascript
accountName;

handleNameChange(event) {

    this.accountName =
        event.target.value;
}
```

---

# Event Object

Every event handler receives an event object.

---

## Example

```javascript
handleClick(event) {

}
```

---

# Common Event Properties

| Property | Description |
|------------|------------|
| target | Element That Triggered Event |
| currentTarget | Element Handling Event |
| detail | Custom Data |
| type | Event Type |

---

# Event Object Example

```javascript
handleClick(event) {

    console.log(event.type);
}
```

Output:

```text
click
```

---

# Custom Events

Custom Events are events created by developers.

Used primarily for:

```text
Child → Parent Communication
```

---

# Why Custom Events?

A child component cannot directly modify parent data.

Instead:

```text
Child
   ↓
Dispatch Event
   ↓
Parent
   ↓
Handle Event
```

---

# Creating a Custom Event

---

## Child Component

### JavaScript

```javascript
handleSave() {

    const saveEvent =
        new CustomEvent(
            'save'
        );

    this.dispatchEvent(
        saveEvent
    );
}
```

---

### HTML

```html
<lightning-button
    label="Save"
    onclick={handleSave}>
</lightning-button>
```

---

# Parent Component

### HTML

```html
<c-child-component
    onsave={handleSave}>
</c-child-component>
```

---

### JavaScript

```javascript
handleSave() {

    console.log(
        'Save Received'
    );
}
```

---

# Passing Data in Custom Events

Use:

```javascript
detail
```

property.

---

## Child Component

```javascript
handleSave() {

    const saveEvent =
        new CustomEvent(
            'save',
            {
                detail: {
                    accountId: '001ABC',
                    accountName: 'Salesforce'
                }
            }
        );

    this.dispatchEvent(
        saveEvent
    );
}
```

---

# Parent Component

```javascript
handleSave(event) {

    console.log(
        event.detail.accountId
    );

    console.log(
        event.detail.accountName
    );
}
```

---

# Output

```text
001ABC
Salesforce
```

---

# Event Bubbling

Event Bubbling is the process where an event travels upward through the component hierarchy.

---

# Bubbling Flow

```text
Child
   ↑
Parent
   ↑
Grandparent
```

Event moves upward.

---

# Example

```text
Button Click
      ↑
Child Component
      ↑
Parent Component
      ↑
Grandparent Component
```

---

# bubbles Property

Controls whether an event bubbles upward.

---

## Example

```javascript
const event =
    new CustomEvent(
        'save',
        {
            bubbles: true
        }
    );
```

---

# Without Bubbling

```javascript
bubbles: false
```

(default)

```text
Child Only
```

---

# With Bubbling

```javascript
bubbles: true
```

```text
Child
 ↑
Parent
 ↑
Grandparent
```

---

# Event Propagation

Event Propagation defines how events travel through the DOM.

---

# Phases of Event Propagation

```text
1. Capturing Phase
2. Target Phase
3. Bubbling Phase
```

---

## Capturing Phase

Event travels downward.

```text
Window
 ↓
Parent
 ↓
Child
```

---

## Target Phase

Event reaches the source element.

```text
Button Clicked
```

---

## Bubbling Phase

Event travels upward.

```text
Child
 ↑
Parent
 ↑
Grandparent
```

---

# LWC Focus

For interviews, focus on:

```text
Target Phase
Bubbling Phase
```

---

# composed Property

Controls whether an event can cross Shadow DOM boundaries.

---

## Example

```javascript
const event =
    new CustomEvent(
        'save',
        {
            bubbles: true,
            composed: true
        }
    );
```

---

# Common Interview Combination

```javascript
new CustomEvent(
    'save',
    {
        detail: data,
        bubbles: true,
        composed: true
    }
);
```

---

# stopPropagation()

Prevents an event from continuing upward.

---

## Example

```javascript
handleClick(event) {

    event.stopPropagation();
}
```

---

# Flow

Without:

```text
Child
 ↑
Parent
 ↑
Grandparent
```

With:

```text
Child
```

Event stops.

---

# Event Target vs CurrentTarget

One of the most commonly asked interview questions.

---

# event.target

Represents the element that actually triggered the event.

---

## Example

```html
<div onclick={handleClick}>

    <button>
        Save
    </button>

</div>
```

User clicks:

```html
<button>
```

---

### JavaScript

```javascript
handleClick(event) {

    console.log(
        event.target
    );
}
```

Output:

```text
button
```

---

# event.currentTarget

Represents the element that owns the event handler.

---

### Same Example

```html
<div onclick={handleClick}>

    <button>
        Save
    </button>

</div>
```

---

### JavaScript

```javascript
handleClick(event) {

    console.log(
        event.currentTarget
    );
}
```

Output:

```text
div
```

---

# Visual Difference

```text
<div onclick={handleClick}>

    <button>
        Save
    </button>

</div>
```

User clicks button.

---

## target

```text
button
```

---

## currentTarget

```text
div
```

---

# Data Attributes with currentTarget

Common LWC pattern.

---

## HTML

```html
<lightning-button
    data-id="001ABC"
    onclick={handleClick}>
</lightning-button>
```

---

## JavaScript

```javascript
handleClick(event) {

    const id =
        event.currentTarget.dataset.id;

    console.log(id);
}
```

---

# Event Handling Best Practices

---

## Use Custom Events for Child → Parent

```javascript
dispatchEvent()
```

---

## Pass Data Through detail

```javascript
detail: {
    accountId: '001'
}
```

---

## Use currentTarget for Dataset Access

```javascript
event.currentTarget.dataset.id
```

---

## Enable Bubbling Only When Needed

```javascript
bubbles: true
```

---

## Use composed for Cross Component Communication

```javascript
composed: true
```

---

# Real Interview Scenarios

## Scenario 1

Button clicked.

### Solution

```javascript
onclick
```

---

## Scenario 2

Pass Account Id to Parent.

### Solution

```javascript
CustomEvent
+
detail
```

---

## Scenario 3

Allow Parent and Grandparent to receive event.

### Solution

```javascript
bubbles: true
```

---

## Scenario 4

Cross Shadow DOM Boundary.

### Solution

```javascript
composed: true
```

---

## Scenario 5

Get clicked button record id.

### Solution

```javascript
event.currentTarget.dataset.id
```

---

# Interview Questions

## Q1: What are Standard Events?

**Answer**

Built-in browser or Lightning events.

Examples:

```text
onclick
onchange
onfocus
onblur
```

---

## Q2: What are Custom Events?

**Answer**

Developer-created events used primarily for Child → Parent communication.

---

## Q3: How do you pass data in a Custom Event?

**Answer**

Using:

```javascript
detail
```

---

## Q4: What is Event Bubbling?

**Answer**

The process where an event travels upward through the component hierarchy.

---

## Q5: What does bubbles:true do?

**Answer**

Allows the event to move upward to parent components.

---

## Q6: What does composed:true do?

**Answer**

Allows the event to cross Shadow DOM boundaries.

---

## Q7: Difference between target and currentTarget?

**Answer**

| target | currentTarget |
|----------|----------|
| Element Clicked | Element Handling Event |
| Actual Source | Event Listener Owner |

---

## Q8: How do you stop event propagation?

**Answer**

```javascript
event.stopPropagation();
```

---

# Summary

## Standard Events

- onclick
- onchange
- onfocus
- onblur
- onsubmit

---

## Custom Events

- Child → Parent Communication
- dispatchEvent()
- detail

---

## Event Bubbling

- Event travels upward
- Controlled by `bubbles:true`

---

## Event Propagation

- Capturing
- Target
- Bubbling

---

## Event Target vs CurrentTarget

### target

Actual clicked element.

### currentTarget

Element containing event handler.

---

## Important Properties

```javascript
event.target
event.currentTarget
event.detail
event.type
```

Event Handling is one of the most frequently asked LWC interview topics because it forms the foundation of user interaction and component communication in Salesforce applications.