
Testing, Debugging, and Deployment are critical parts of the LWC development lifecycle.

A successful LWC developer should be able to:

- Debug components efficiently
- Analyze browser behavior
- Write unit tests
- Deploy safely to higher environments

---

# Topics Covered

| Topic | Purpose |
|---------|---------|
| Console Debugging | Troubleshoot LWC Issues |
| Chrome DevTools | Browser Analysis |
| Jest Testing | Unit Testing |
| Deployment Best Practices | Safe Production Releases |

---

# Console Debugging

The simplest and most commonly used debugging technique.

---

# Why Use Console Debugging?

Helps identify:

- Variable Values
- Event Data
- API Responses
- Apex Responses
- JavaScript Errors

---

# Basic Example

```javascript
console.log('Component Loaded');
```

---

# Logging Variables

```javascript
console.log(this.accounts);
```

---

# Logging Objects

```javascript
console.log(
    JSON.stringify(
        this.accounts
    )
);
```

---

# Logging Events

```javascript
handleClick(event){

    console.log(event);
}
```

---

# Logging Apex Response

```javascript
getAccounts()

.then(result => {

    console.log(result);

})

.catch(error => {

    console.error(error);

});
```

---

# Console Methods

| Method | Purpose |
|----------|----------|
| console.log() | General Output |
| console.error() | Errors |
| console.warn() | Warnings |
| console.table() | Display Arrays |
| console.group() | Group Logs |

---

# Example

```javascript
console.table(
    this.accounts
);
```

---

# Best Practice

Use meaningful logs.

Bad:

```javascript
console.log('1');
```

---

Good:

```javascript
console.log(
    'Accounts Loaded',
    this.accounts
);
```

---

# Chrome DevTools

Most powerful debugging tool for LWC.

Open:

```text
F12
```

or

```text
Right Click
 ↓
Inspect
```

---

# DevTools Tabs

| Tab | Purpose |
|--------|---------|
| Elements | DOM Inspection |
| Console | Logs & Errors |
| Sources | JavaScript Debugging |
| Network | API Requests |
| Performance | Performance Analysis |
| Application | Storage Inspection |

---

# Elements Tab

Inspect component DOM.

---

## Example

```html
<c-account-list>
```

Can inspect:

- HTML Structure
- CSS
- SLDS Classes
- Rendered Components

---

# Console Tab

Displays:

```javascript
console.log()
```

outputs.

Also shows:

- JavaScript Errors
- LWC Runtime Errors
- Apex Errors

---

# Sources Tab

Used for breakpoints.

---

## Add Breakpoint

```javascript
handleSave(){

    debugger;

}
```

---

When executed:

```text
Execution Stops
```

allowing variable inspection.

---

# Using debugger

```javascript
loadAccounts(){

    debugger;

    console.log(
        this.accounts
    );
}
```

---

# Network Tab

Most useful for:

- Apex Calls
- UI API Calls
- Static Resources

---

# Analyze Requests

Shows:

```text
Request URL
Response
Status Code
Time Taken
```

---

# Common Status Codes

| Code | Meaning |
|---------|---------|
| 200 | Success |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 500 | Server Error |

---

# Performance Tab

Used to identify:

- Slow Rendering
- Long Scripts
- DOM Bottlenecks

---

# Common Debugging Workflow

```text
Error Appears
      ↓
Console
      ↓
Network
      ↓
Sources
      ↓
Fix Code
```

---

# Jest Testing

Official LWC Unit Testing Framework.

Frequently asked interview topic.

---

# Why Jest?

Allows testing:

- JavaScript Logic
- User Interaction
- Event Handling
- Component Rendering

Without Salesforce Server.

---

# Installation

```bash
npm install
```

---

# Run Tests

```bash
npm run test:unit
```

---

# Test Structure

```text
lwc
 ├── accountList
 │    ├── accountList.js
 │    ├── accountList.html
 │    ├── accountList.test.js
```

---

# Basic Test

### accountList.test.js

```javascript
import {

    createElement

}

from 'lwc';

import AccountList

from 'c/accountList';

describe(

    'c-account-list',

    () => {

        afterEach(() => {

            while(
                document.body
                    .firstChild
            ){

                document.body
                    .removeChild(
                        document.body
                            .firstChild
                    );
            }
        });

        it(

            'renders component',

            () => {

                const element =

                    createElement(

                        'c-account-list',

                        {
                            is:
                            AccountList
                        }

                    );

                document.body
                    .appendChild(
                        element
                    );

                expect(
                    element
                )
                .not
                .toBeNull();

            }

        );

    }

);
```

---

# Test Button Click

### Component

```html
<lightning-button
    label="Save"
    onclick={handleSave}>
</lightning-button>
```

---

### Test

```javascript
it(

    'calls click event',

    () => {

        const button =

            element.shadowRoot
                .querySelector(
                    'lightning-button'
                );

        button.click();

    }

);
```

---

# Testing Apex Calls

Mock Apex.

---

## Mock

```javascript
jest.mock(

'@salesforce/apex/AccountController.getAccounts',

() => {

    return {

        default:
            jest.fn()

    };

},

{
    virtual:true
}

);
```

---

# Example

```javascript
getAccounts.mockResolvedValue(
    mockAccounts
);
```

---

# Common Jest Functions

| Function | Purpose |
|-----------|----------|
| describe() | Test Group |
| it() | Test Case |
| expect() | Assertion |
| jest.fn() | Mock Function |
| mockResolvedValue() | Mock Success |
| mockRejectedValue() | Mock Error |

---

# Deployment Best Practices

Deployment is the process of moving code between orgs.

---

# Deployment Flow

```text
Developer Sandbox
       ↓
Integration Sandbox
       ↓
UAT
       ↓
Production
```

---

# Deployment Tools

| Tool | Purpose |
|---------|---------|
| Change Sets | Admin Deployment |
| Salesforce CLI | Developer Deployment |
| VS Code | Development |
| DevOps Center | Modern CI/CD |
| GitHub | Source Control |

---

# Salesforce CLI Deployment

Deploy component:

```bash
sf project deploy start
```

---

Deploy specific metadata:

```bash
sf project deploy start

--metadata

LightningComponentBundle:accountList
```

---

# Validate Deployment

```bash
sf project deploy validate
```

---

# Retrieve Metadata

```bash
sf project retrieve start
```

---

# Best Practices Before Deployment

---

## Run Apex Tests

```bash
Run All Tests
```

---

## Run Jest Tests

```bash
npm run test:unit
```

---

## Check LWC Errors

```text
Browser Console
```

---

## Verify Profiles

Ensure:

- Permissions
- Object Access
- Field Access

are included.

---

## Use Source Control

Always commit code.

```bash
git commit
```

---

# Deployment Checklist

| Item | Verify |
|---------|---------|
| Apex Tests | ✅ |
| Jest Tests | ✅ |
| LWC Functionality | ✅ |
| Profiles | ✅ |
| Permission Sets | ✅ |
| Custom Labels | ✅ |
| Static Resources | ✅ |

---

# Common Interview Scenarios

## Scenario 1

Need to inspect Apex response.

### Solution

```javascript
console.log(result);
```

---

## Scenario 2

Need to debug JavaScript execution.

### Solution

```javascript
debugger;
```

---

## Scenario 3

Need to inspect API requests.

### Solution

```text
Network Tab
```

---

## Scenario 4

Need unit testing for LWC.

### Solution

```javascript
Jest
```

---

## Scenario 5

Need to mock Apex in tests.

### Solution

```javascript
jest.mock()
```

---

# Interview Questions

## Q1: What is the official testing framework for LWC?

### Answer

```javascript
Jest
```

---

## Q2: How do you run Jest tests?

### Answer

```bash
npm run test:unit
```

---

## Q3: Which Chrome DevTools tab is used for API requests?

### Answer

```text
Network
```

---

## Q4: Which tab is used for DOM inspection?

### Answer

```text
Elements
```

---

## Q5: How do you pause JavaScript execution?

### Answer

```javascript
debugger;
```

---

## Q6: How do you mock Apex methods in Jest?

### Answer

```javascript
jest.mock()
```

---

## Q7: Why use console.table()?

### Answer

Displays arrays and objects in a structured table format.

---

## Q8: What should be tested before deployment?

### Answer

- Apex Tests
- Jest Tests
- Permissions
- UI Functionality
- Integrations

---

## Q9: Which deployment tool is most commonly used by developers?

### Answer

```text
Salesforce CLI
```

and

```text
VS Code
```

---

## Q10: Why is source control important?

### Answer

- Version History
- Team Collaboration
- Rollback Support
- CI/CD Integration

---

# Summary

## Console Debugging

```javascript
console.log()
console.error()
console.table()
```

- Fast Troubleshooting
- Runtime Analysis

---

## Chrome DevTools

```text
Elements
Console
Sources
Network
Performance
```

- Browser Debugging
- Performance Analysis

---

## Jest Testing

```javascript
describe()
it()
expect()
jest.mock()
```

- Unit Testing
- Apex Mocking
- Event Testing

---

## Deployment Best Practices

- Run Apex Tests
- Run Jest Tests
- Validate Permissions
- Use Source Control
- Validate Before Deploying

Testing, Debugging, and Deployment are essential skills for every Salesforce developer because writing code is only part of the job—ensuring that code is reliable, maintainable, and safely deployed to production is equally important.