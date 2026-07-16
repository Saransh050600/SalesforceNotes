
Performance Optimization is critical in Lightning Web Components because users expect fast and responsive applications.

Poorly optimized components can cause:

- Slow page loads
- Excessive server calls
- Poor user experience
- Browser memory issues

Key topics:

- Caching
- Lazy Loading
- Debouncing
- Efficient Rendering
- Best Practices

---

# Why Performance Matters

```text
Fast Components
      ↓
Better User Experience
      ↓
Higher Productivity
      ↓
Better Adoption
```

---

# Performance Optimization Areas

| Area | Goal |
|---------|---------|
| Caching | Reduce Server Calls |
| Lazy Loading | Load Only When Needed |
| Debouncing | Reduce Frequent Requests |
| Efficient Rendering | Minimize DOM Updates |
| Best Practices | Overall Optimization |

---

# Caching

Caching stores data temporarily so repeated requests do not hit the server.

---

# Why Use Caching?

Without Cache:

```text
User Opens Page
       ↓
Apex Call
       ↓
Database Query
```

Repeated every time.

---

With Cache:

```text
User Opens Page
       ↓
Cache Available
       ↓
Instant Response
```

---

# Cacheable Apex

Most common caching mechanism.

---

## Example

```java
@AuraEnabled(cacheable=true)
public static List<Account>
getAccounts(){

    return [
        SELECT Id,
               Name
        FROM Account
    ];
}
```

---

# Benefits

- Faster UI
- Reduced SOQL
- Fewer Server Calls
- Better Scalability

---

# Wire + Cache

```javascript
@wire(getAccounts)
accounts;
```

Wire automatically uses cacheable responses.

---

# refreshApex

Refreshes cached data.

```javascript
import { refreshApex }
from '@salesforce/apex';
```

---

## Example

```javascript
refreshApex(
    this.wiredResult
);
```

---

# Lightning Data Service Cache

LDS automatically caches records.

Example:

```javascript
getRecord()
```

```javascript
updateRecord()
```

```javascript
createRecord()
```

---

Benefits:

- Shared Cache
- Better Performance
- Less Apex Needed

---

# Lazy Loading

Load data only when required.

Very common optimization strategy.

---

# Without Lazy Loading

```text
Load Page
      ↓
Load Everything
      ↓
Slow UI
```

---

# With Lazy Loading

```text
Load Page
      ↓
Load Essential Data
      ↓
Load Remaining Data Later
```

---

# Example

Load contacts only when button clicked.

```javascript
handleLoadContacts(){

    getContacts()

    .then(result => {

        this.contacts =
            result;

    });
}
```

---

# Lazy Loading Tabs

Load data only when tab becomes active.

```html
<lightning-tab
    onactive={loadData}>
</lightning-tab>
```

---

# Infinite Scrolling

Load records as user scrolls.

```html
enable-infinite-loading
```

---

## Datatable Example

```html
<lightning-datatable
    enable-infinite-loading
    onloadmore={loadMoreData}>
</lightning-datatable>
```

---

# Debouncing

Debouncing prevents repeated execution of a function.

Common interview topic.

---

# Why Use Debouncing?

Without Debouncing:

```text
S
Sa
Sal
Sale
Sales
```

5 Apex Calls

---

With Debouncing:

```text
S
Sa
Sal
Sale
Sales
      ↓
1 Apex Call
```

---

# Search Example

### HTML

```html
<lightning-input
    onchange={handleSearch}>
</lightning-input>
```

---

### JavaScript

```javascript
delayTimeout;
```

---

```javascript
handleSearch(event){

    window.clearTimeout(
        this.delayTimeout
    );

    const searchKey =
        event.target.value;

    this.delayTimeout =
        setTimeout(() => {

            this.searchAccounts(
                searchKey
            );

        },300);
}
```

---

# Benefits

- Reduced Server Calls
- Better UX
- Lower Governor Limits
- Faster Applications

---

# Typical Delay

```javascript
300ms
```

to

```javascript
500ms
```

---

# Efficient Rendering

Rendering affects UI responsiveness.

---

# What Causes Slow Rendering?

- Large Arrays
- Unnecessary Re-renders
- Complex DOM
- Excessive Loops

---

# Use Key Attribute

Always use:

```html
key
```

inside loops.

---

## Example

```html
<template
    for:each={accounts}
    for:item="account">

    <div
        key={account.Id}>

        {account.Name}

    </div>

</template>
```

---

# Why Key Matters

Without:

```html
key
```

LWC may re-render entire lists.

---

With:

```html
key={Id}
```

Only changed records are updated.

---

# Conditional Rendering

Avoid rendering unnecessary components.

---

## Example

```html
<template if:true={showTable}>

    <lightning-datatable>

    </lightning-datatable>

</template>
```

---

# Getter Optimization

Avoid expensive calculations inside getters.

---

## Bad Example

```javascript
get totalRevenue(){

    return this.accounts
        .reduce(...);
}
```

Executed every render.

---

## Better Approach

```javascript
calculateRevenue(){

    this.totalRevenue =
        ...
}
```

---

# Avoid Large DOM Trees

Bad:

```html
1000+
Rows
```

---

Better:

```text
Pagination
```

or

```text
Infinite Scroll
```

---

# Efficient Event Handling

Avoid adding unnecessary event listeners.

---

## Better

```html
onclick={handleClick}
```

---

Instead of:

```javascript
addEventListener()
```

when unnecessary.

---

# Best Practices

---

## Use LDS Before Apex

Preferred:

```javascript
getRecord()
```

instead of Apex when possible.

---

## Cache Read Operations

```java
@AuraEnabled(
    cacheable=true
)
```

---

## Use Pagination

Instead of:

```javascript
5000 records
```

load:

```javascript
50 records
```

---

## Use Debouncing

For:

- Search
- Filters
- Autocomplete

---

## Lazy Load Components

Load only when users need them.

---

## Avoid Excessive SOQL

Bad:

```text
Every Button Click
↓
New Query
```

---

Better:

```text
Cache Results
```

---

## Use refreshApex Only When Needed

Avoid unnecessary refreshes.

---

# Common Performance Problems

| Problem | Solution |
|----------|------------|
| Slow Search | Debouncing |
| Too Many Queries | Caching |
| Large Datatables | Pagination |
| Heavy Initial Load | Lazy Loading |
| Frequent Re-render | Proper Keys |
| Multiple Server Calls | LDS |

---

# Real Interview Scenarios

## Scenario 1

Search Accounts while typing.

### Solution

```javascript
Debouncing
```

---

## Scenario 2

Need records only when tab opens.

### Solution

```javascript
Lazy Loading
```

---

## Scenario 3

Need faster repeated queries.

### Solution

```java
@AuraEnabled(cacheable=true)
```

---

## Scenario 4

Need latest cached data.

### Solution

```javascript
refreshApex()
```

---

## Scenario 5

Need to display 20,000 records.

### Solution

```javascript
Pagination
```

or

```javascript
Infinite Loading
```

---

# Interview Questions

## Q1: What is caching?

### Answer

Storing data temporarily to avoid repeated server requests.

---

## Q2: How do you cache Apex responses?

### Answer

```java
@AuraEnabled(cacheable=true)
```

---

## Q3: What is lazy loading?

### Answer

Loading data only when it is needed.

---

## Q4: What is debouncing?

### Answer

Delaying execution until user stops performing an action.

Commonly used in search functionality.

---

## Q5: Why use debouncing?

### Answer

Reduces:

- Apex Calls
- Server Load
- Governor Limit Consumption

---

## Q6: What is the typical debounce delay?

### Answer

```javascript
300ms - 500ms
```

---

## Q7: Why is key important in loops?

### Answer

Allows LWC to efficiently update only changed DOM elements.

---

## Q8: Which is better for record retrieval?

### Answer

```javascript
Lightning Data Service
```

before Apex whenever possible.

---

## Q9: What causes unnecessary re-rendering?

### Answer

- Missing key
- Large DOM trees
- Expensive getters
- Excessive state changes

---

## Q10: How do you optimize large datatables?

### Answer

- Pagination
- Infinite Scroll
- Lazy Loading
- Cached Queries

---

# Summary

## Caching

```java
@AuraEnabled(cacheable=true)
```

- Faster Queries
- Reduced Server Calls

---

## Lazy Loading

- Load When Needed
- Better Initial Performance

---

## Debouncing

```javascript
setTimeout()
clearTimeout()
```

- Search Optimization
- Reduced API Calls

---

## Efficient Rendering

- Use key
- Conditional Rendering
- Avoid Expensive Getters
- Smaller DOM

---

## Best Practices

- Use LDS First
- Cache Read Operations
- Use Pagination
- Lazy Load Data
- Debounce Search
- Avoid Unnecessary Re-renders

Performance Optimization is a favorite LWC interview topic because enterprise Salesforce applications often deal with large datasets, multiple integrations, and high user traffic where efficient components significantly improve scalability and user experience.