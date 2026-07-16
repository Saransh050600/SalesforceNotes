
Security is one of the most important topics in Salesforce and LWC interviews.

Every Salesforce application must ensure users can only access data they are authorized to view or modify.

Key security concepts:

- CRUD
- FLS
- Sharing
- Lightning Web Security (LWS)
- Content Security Policy (CSP)

---

# Security Layers in Salesforce

```text
Organization
     ↓
Object Security (CRUD)
     ↓
Field Security (FLS)
     ↓
Record Security (Sharing)
     ↓
LWC Security (LWS)
     ↓
Browser Security (CSP)
```

---

# CRUD

CRUD stands for:

| Operation | Meaning |
|------------|------------|
| Create | Insert Records |
| Read | View Records |
| Update | Modify Records |
| Delete | Remove Records |

---

# Why CRUD Matters

Example:

A user may have:

```text
Read Account
✓

Edit Account
✗
```

Without CRUD enforcement:

```text
User could modify records
```

which becomes a security violation.

---

# Checking CRUD in Apex

## Object Access

```java
if(Account.SObjectType
    .getDescribe()
    .isCreateable()){

}
```

---

## Read Access

```java
if(Account.SObjectType
    .getDescribe()
    .isAccessible()){

}
```

---

## Update Access

```java
if(Account.SObjectType
    .getDescribe()
    .isUpdateable()){

}
```

---

## Delete Access

```java
if(Account.SObjectType
    .getDescribe()
    .isDeletable()){

}
```

---

# Example

```java
if(
    Account.SObjectType
        .getDescribe()
        .isAccessible()
){

    return [
        SELECT Id, Name
        FROM Account
    ];
}
```

---

# FLS (Field-Level Security)

FLS controls access to specific fields.

Example:

| User | Salary__c |
|--------|------------|
| HR | Visible |
| Sales Rep | Hidden |

---

# Why FLS Matters

Even if user can access Account:

```text
Account Access
✓
```

they may not have access to:

```text
Salary__c
✗
```

---

# Check FLS

## Read Access

```java
Account.Salary__c
    .getDescribe()
    .isAccessible()
```

---

## Update Access

```java
Account.Salary__c
    .getDescribe()
    .isUpdateable()
```

---

# Example

```java
if(
    Account.Salary__c
        .getDescribe()
        .isAccessible()
){

    System.debug(
        account.Salary__c
    );
}
```

---

# stripInaccessible()

Recommended Salesforce approach.

Automatically removes fields user cannot access.

---

## Example

```java
List<Account> accounts =
[
    SELECT Id,
           Name,
           AnnualRevenue
    FROM Account
];
```

---

```java
SObjectAccessDecision
decision =

Security.stripInaccessible(

    AccessType.READABLE,

    accounts

);
```

---

## Result

```java
decision.getRecords();
```

contains only accessible fields.

---

# Sharing

Sharing controls record-level access.

Example:

```text
User A
can see Account A

User B
cannot see Account A
```

---

# Types of Sharing

| Type | Purpose |
|---------|---------|
| OWD | Base Access |
| Role Hierarchy | Manager Access |
| Sharing Rules | Additional Access |
| Manual Sharing | Individual Access |
| Apex Sharing | Programmatic Access |

---

# with sharing

Most important Apex interview topic.

---

## Example

```java
public with sharing class
AccountController {

}
```

---

# Purpose

Enforces record-level security.

Without:

```java
without sharing
```

users may see records they should not access.

---

# with sharing vs without sharing

| Feature | with sharing | without sharing |
|----------|------------|------------|
| Respects Sharing | ✅ | ❌ |
| Record Security | ✅ | ❌ |
| Recommended | ✅ | ❌ |

---

# inherited sharing

Recommended modern approach.

```java
public inherited sharing
class AccountController {

}
```

---

# Benefits

Uses sharing context of caller.

More secure.

---

# Lightning Data Service Security

LDS automatically enforces:

- CRUD
- FLS
- Sharing

---

# Example

```javascript
getRecord()
createRecord()
updateRecord()
deleteRecord()
```

All automatically respect security.

---

# Lightning Web Security (LWS)

Lightning Web Security is Salesforce's modern JavaScript security architecture.

Replaced:

```text
Locker Service
```

---

# Purpose

Protects:

- Components
- Browser Environment
- User Data

---

# Without LWS

A component might:

```javascript
window.document
```

access another component's DOM.

---

# With LWS

Each component runs in an isolated sandbox.

```text
Component A
cannot directly manipulate

Component B
```

---

# Benefits of LWS

- Better Performance
- Improved Security
- Modern JavaScript Support
- Better Third-Party Library Support

---

# LWS Restrictions

Avoid:

```javascript
document.querySelector()
```

Use:

```javascript
this.template.querySelector()
```

---

Avoid:

```javascript
window.document
```

Use component DOM instead.

---

# Example

### Incorrect

```javascript
document.querySelector(
    '.btn'
);
```

---

### Correct

```javascript
this.template.querySelector(
    '.btn'
);
```

---

# Content Security Policy (CSP)

Browser-level security mechanism.

Controls:

- Scripts
- Images
- Frames
- External Resources

---

# Why CSP?

Prevents:

- XSS Attacks
- Malicious Scripts
- Unauthorized Resources

---

# Example

Blocked:

```html
<script>
alert('Hello');
</script>
```

---

Inline scripts are not allowed.

---

# External JavaScript

Use:

```text
Static Resources
```

instead of CDN links.

---

# Example

### Incorrect

```html
<script src=
"https://cdn.com/lib.js">
</script>
```

---

### Correct

```javascript
import LIBRARY
from '@salesforce/resourceUrl/lib';
```

---

# Loading Third-Party Libraries

```javascript
import {
    loadScript
}
from 'lightning/platformResourceLoader';
```

---

## Example

```javascript
loadScript(
    this,
    LIBRARY
);
```

---

# Security Best Practices

## Always

Use:

```java
with sharing
```

or

```java
inherited sharing
```

---

## Enforce

- CRUD
- FLS
- Sharing

---

## Use

```java
Security.stripInaccessible()
```

---

## Prefer

```javascript
Lightning Data Service
```

for standard CRUD operations.

---

## Avoid

```javascript
document.querySelector()
window.document
```

---

## Store Libraries

As:

```text
Static Resources
```

---

# Common Interview Scenarios

## Scenario 1

Need record-level security.

### Solution

```java
with sharing
```

---

## Scenario 2

Need field-level security.

### Solution

```java
stripInaccessible()
```

---

## Scenario 3

Need object-level permission checks.

### Solution

```java
isAccessible()
isCreateable()
isUpdateable()
isDeletable()
```

---

## Scenario 4

Need secure CRUD operations without Apex.

### Solution

```javascript
Lightning Data Service
```

---

## Scenario 5

Need third-party JavaScript library.

### Solution

```text
Static Resource
```

and

```javascript
loadScript()
```

---

# Interview Questions

## Q1: What is CRUD?

### Answer

Object-level permissions:

- Create
- Read
- Update
- Delete

---

## Q2: What is FLS?

### Answer

Field-Level Security controls visibility and edit access to individual fields.

---

## Q3: What is Sharing?

### Answer

Record-level access control.

Determines which records users can see.

---

## Q4: Difference between CRUD and FLS?

| CRUD | FLS |
|--------|--------|
| Object Level | Field Level |
| Account Access | Account.Name Access |

---

## Q5: Difference between FLS and Sharing?

| FLS | Sharing |
|------|---------|
| Field Access | Record Access |
| Name Field | Account Record |

---

## Q6: What does with sharing do?

### Answer

Enforces record-level security in Apex.

---

## Q7: What is stripInaccessible()?

### Answer

Removes fields user cannot access.

Recommended Salesforce security practice.

---

## Q8: What is Lightning Web Security?

### Answer

Modern Salesforce client-side security architecture that isolates components and protects browser resources.

---

## Q9: What replaced Locker Service?

### Answer

```text
Lightning Web Security (LWS)
```

---

## Q10: What is CSP?

### Answer

Content Security Policy.

Prevents execution of unauthorized scripts and resources.

---

## Q11: Why use Static Resources for libraries?

### Answer

CSP blocks direct CDN script loading.

Static Resources are trusted by Salesforce.

---

# Summary

## CRUD

```java
isAccessible()
isCreateable()
isUpdateable()
isDeletable()
```

- Object-Level Security

---

## FLS

```java
stripInaccessible()
```

- Field-Level Security

---

## Sharing

```java
with sharing
inherited sharing
```

- Record-Level Security

---

## Lightning Web Security

- Replaces Locker Service
- Component Isolation
- Secure DOM Access

---

## CSP

- Blocks Unauthorized Scripts
- Prevents XSS Attacks
- Requires Static Resources

Security is one of the most frequently asked Salesforce interview topics because every Apex class, LWC component, integration, and data operation must respect Salesforce security architecture.