
This section contains the most frequently asked Lightning Web Components interview questions along with concise, practical answers.

---

# LWC Fundamentals

## Q1: What is Lightning Web Components (LWC)?

### Answer

Lightning Web Components is Salesforce's modern UI framework built on:

- Web Components
- ES6+ JavaScript
- Shadow DOM
- Custom Elements

Used to build fast, reusable Salesforce applications.

---

## Q2: Difference between Aura and LWC?

| Aura | LWC |
|--------|--------|
| Older Framework | Modern Framework |
| Proprietary Model | Web Standards |
| Slower Performance | Faster Performance |
| More Boilerplate | Less Code |
| Event Based | Standard DOM Events |

---

## Q3: Why is LWC faster than Aura?

### Answer

Because LWC:

- Uses native browser APIs
- Uses Shadow DOM
- Uses modern JavaScript
- Requires less framework overhead

---

# Component Lifecycle

## Q4: Lifecycle Hooks in LWC?

### Answer

```javascript
constructor()
connectedCallback()
renderedCallback()
disconnectedCallback()
errorCallback()
```

---

## Q5: When is connectedCallback() executed?

### Answer

When component is inserted into DOM.

Used for:

- Initial Setup
- Apex Calls
- LMS Subscription

---

## Q6: When is renderedCallback() executed?

### Answer

After component renders.

Used for:

- DOM Manipulation
- Third-Party Libraries

---

## Q7: Difference between connectedCallback and renderedCallback?

| connectedCallback | renderedCallback |
|-------------------|------------------|
| Runs Once | Runs After Every Render |
| DOM Not Ready | DOM Available |
| Initialization | DOM Operations |

---

# Data Binding

## Q8: What is Reactive Property?

### Answer

A property that automatically updates UI when value changes.

```javascript
name = 'Salesforce';
```

---

## Q9: What happened to @track?

### Answer

Modern LWC automatically tracks primitive properties.

Use `@track` only for complex nested object changes.

---

# Decorators

## Q10: What are LWC Decorators?

### Answer

```javascript
@api
@wire
@track
```

---

## Q11: Purpose of @api?

### Answer

Makes property or method public.

Allows:

- Parent → Child Communication
- Flow Inputs
- Public Methods

---

## Q12: Purpose of @wire?

### Answer

Connects LWC to:

- Apex
- UI APIs
- LDS

Automatically reacts to data changes.

---

# Communication

## Q13: Parent to Child Communication?

### Answer

Using:

```javascript
@api
```

---

### Parent

```html
<c-child
    name={accountName}>
</c-child>
```

---

### Child

```javascript
@api name;
```

---

## Q14: Child to Parent Communication?

### Answer

Using Custom Events.

```javascript
this.dispatchEvent(
    new CustomEvent('save')
);
```

---

## Q15: Unrelated Components Communication?

### Answer

Using:

```javascript
Lightning Message Service
```

---

# Apex Integration

## Q16: Difference Between Wire and Imperative Apex?

| Wire | Imperative |
|--------|------------|
| Automatic | Manual |
| Reactive | Non-Reactive |
| Cacheable Required | Not Required |
| Read Operations | Read + DML |

---

## Q17: When should Imperative Apex be used?

### Answer

- Create
- Update
- Delete
- Button Click
- Full Control

---

## Q18: What is refreshApex()?

### Answer

Refreshes cached wire data.

```javascript
refreshApex(
    wiredResult
);
```

---

# LDS & UI APIs

## Q19: What is Lightning Data Service?

### Answer

A Salesforce data layer that allows CRUD operations without Apex.

---

## Q20: Difference Between LDS and Apex?

| LDS | Apex |
|------|------|
| No Code | Custom Logic |
| Built-In Security | Manual Security |
| CRUD Only | Complex Logic |

---

## Q21: Common LDS Methods?

### Answer

```javascript
getRecord()
createRecord()
updateRecord()
deleteRecord()
getFieldValue()
```

---

# Forms

## Q22: Difference Between record-form and record-edit-form?

| record-form | record-edit-form |
|------------|------------------|
| Simple | Flexible |
| Less Customization | Full Customization |

---

## Q23: How do you perform custom validation?

### Answer

```javascript
setCustomValidity()
reportValidity()
```

---

# Navigation

## Q24: What is NavigationMixin?

### Answer

Used for navigating between Salesforce pages.

---

## Q25: Navigation Types?

### Answer

```javascript
standard__recordPage
standard__objectPage
standard__webPage
standard__recordRelationshipPage
```

---

# Datatable

## Q26: How do you make a column sortable?

### Answer

```javascript
sortable: true
```

---

## Q27: How do you enable inline editing?

### Answer

```javascript
editable: true
```

---

## Q28: What are draftValues?

### Answer

Stores unsaved changes.

```javascript
event.detail.draftValues
```

---

## Q29: Does Datatable support pagination?

### Answer

No.

Must be implemented manually.

---

## Q30: How do you create custom datatable types?

### Answer

Extend:

```javascript
LightningDatatable
```

---

# Security

## Q31: Difference Between CRUD, FLS and Sharing?

| Security Layer | Controls |
|---------------|-----------|
| CRUD | Object Access |
| FLS | Field Access |
| Sharing | Record Access |

---

## Q32: What is with sharing?

### Answer

Enforces record-level security.

```java
public with sharing class
```

---

## Q33: What is stripInaccessible()?

### Answer

Removes fields users cannot access.

---

## Q34: What is Lightning Web Security?

### Answer

Modern Salesforce client-side security architecture.

Replaced:

```text
Locker Service
```

---

# Performance

## Q35: What is Debouncing?

### Answer

Delays execution until user stops typing.

Commonly used in search.

---

## Q36: What is Lazy Loading?

### Answer

Load data only when needed.

---

## Q37: How do you optimize LWC performance?

### Answer

- Cache Data
- Use LDS
- Debounce Searches
- Pagination
- Lazy Loading

---

# LMS

## Q38: What is Lightning Message Service?

### Answer

Allows communication between unrelated components.

---

## Q39: LMS Components?

### Answer

```javascript
Message Channel
Publisher
Subscriber
```

---

# Flow Integration

## Q40: How do you expose an LWC to Flow?

### Answer

```xml
lightning__FlowScreen
```

---

## Q41: How do you navigate Flow screens?

### Answer

```javascript
FlowNavigationNextEvent
FlowNavigationBackEvent
FlowNavigationFinishEvent
```

---

## Q42: How do you validate an LWC inside Flow?

### Answer

Implement:

```javascript
validate()
```

---

# Advanced Concepts

## Q43: What are Slots?

### Answer

Allow parent components to inject content into child components.

---

## Q44: What are Dynamic Components?

### Answer

Components loaded at runtime.

```javascript
import()
```

---

## Q45: What are Custom Labels?

### Answer

Reusable text values that support translations.

---

## Q46: What are Custom Permissions?

### Answer

Permissions used to control feature access.

---

## Q47: What are Platform Events?

### Answer

Event-driven communication mechanism in Salesforce.

---

# Testing

## Q48: Which testing framework is used for LWC?

### Answer

```javascript
Jest
```

---

## Q49: How do you mock Apex in Jest?

### Answer

```javascript
jest.mock()
```

---

## Q50: How do you run Jest tests?

### Answer

```bash
npm run test:unit
```

---

# Scenario-Based Questions

## Q51: Need Account Data on Load?

### Answer

```javascript
@wire
```

---

## Q52: Need Create Record?

### Answer

```javascript
createRecord()
```

or

Imperative Apex.

---

## Q53: Need Parent → Child Communication?

### Answer

```javascript
@api
```

---

## Q54: Need Child → Parent Communication?

### Answer

```javascript
CustomEvent
```

---

## Q55: Need Unrelated Components Communication?

### Answer

```javascript
LMS
```

---

## Q56: Need Refresh Data After Save?

### Answer

```javascript
refreshApex()
```

---

## Q57: Need Security Enforcement?

### Answer

```java
with sharing
stripInaccessible()
CRUD Checks
```

---

## Q58: Need Loading Indicator?

### Answer

```html
<lightning-spinner>
```

---

## Q59: Need User Notification?

### Answer

```javascript
ShowToastEvent
```

---

## Q60: Need Runtime Component Loading?

### Answer

```javascript
Dynamic Components
```

---

# Senior-Level Questions

## Q61: Why prefer LDS over Apex?

### Answer

- Less Code
- Better Security
- Automatic Caching
- Better Performance

---

## Q62: What causes unnecessary rerendering?

### Answer

- Missing key attribute
- Expensive getters
- Frequent state updates
- Large DOM trees

---

## Q63: How does LWC achieve reactivity?

### Answer

By tracking property changes and rerendering affected DOM portions.

---

## Q64: Explain LWC Rendering Lifecycle.

### Answer

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

## Q65: Most Important Topics for Interviews?

### Answer

- Lifecycle Hooks
- @api, @wire
- Parent-Child Communication
- LMS
- Apex Integration
- LDS
- Datatable
- NavigationMixin
- Security
- Flow Integration
- Jest Testing
- Performance Optimization

---

# Final Interview Revision Checklist

## Must Know

- Lifecycle Hooks
- Decorators
- Events
- LMS
- Apex Integration
- Wire vs Imperative
- LDS
- Forms & Validation
- NavigationMixin
- Datatable
- Security
- Performance
- Flow Integration
- Platform Events
- Jest Testing

---

# Golden Interview Tip

If you remember only five topics before an interview, focus on:

1. Lifecycle Hooks
2. Component Communication
3. Apex Integration (Wire vs Imperative)
4. LDS vs Apex
5. Datatable + Security

These five areas alone cover a large percentage of real-world LWC interview questions.