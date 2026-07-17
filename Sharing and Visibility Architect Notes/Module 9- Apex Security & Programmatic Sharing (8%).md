
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **Core Exam Principle**
>
> Apex runs in **System Mode by default**, meaning it can bypass user permissions unless security is explicitly enforced.
>
> Architects must ensure that custom code respects:
>
> - Record-Level Security (Sharing)
> - Object-Level Security (CRUD)
> - Field-Level Security (FLS)

---

# 1. Apex Security Overview

Salesforce security has three major layers that Apex must consider.

```
Record-Level Security

↓

Object-Level Security (CRUD)

↓

Field-Level Security (FLS)
```

Apex **does NOT automatically enforce all three**.

Developers are responsible for enforcing them appropriately.

---

# Apex Execution Context

```
User clicks button

↓

Apex executes

↓

System Mode

↓

May bypass user permissions
```

Unless security is enforced,

users may access data they shouldn't.

---

# Security Layers in Apex

| Security Layer | Automatically Enforced? |
|---------------|-------------------------|
| Record Sharing | Depends on sharing keyword |
| CRUD | No |
| FLS | No |

---

# 2. Sharing Keywords

Salesforce provides three sharing keywords.

- with sharing
- without sharing
- inherited sharing

---

# with sharing

Enforces

Record-Level Sharing Rules.

Example

```apex
public with sharing class AccountController {

}
```

The class respects

- OWD
- Role Hierarchy
- Sharing Rules
- Manual Sharing
- Apex Sharing

---

## Example

```
OWD

Private

↓

User owns

Only one Account
```

Query

```apex
SELECT Name FROM Account
```

Result

Returns

Only records

Visible to the user.

---

# without sharing

Ignores record sharing.

Example

```apex
public without sharing class AdminUtility {

}
```

The class can access

All records

Regardless of sharing.

---

## Use Cases

- Administrative jobs
- Scheduled Apex
- Data cleanup
- Migration utilities

Avoid for user-facing functionality.

---

# inherited sharing

Introduced as a safer default.

The class inherits

The sharing context

From the caller.

Example

```apex
public inherited sharing class InvoiceService {

}
```

---

## Behavior

Called from

```
with sharing

↓

Runs

with sharing
```

Called from

```
without sharing

↓

Runs

without sharing
```

---

# Comparison

| Keyword | Sharing Enforced |
|-----------|-----------------|
| with sharing | Yes |
| without sharing | No |
| inherited sharing | Inherits caller |

---

# Exam Tip ⭐

Salesforce recommends

Using

```
inherited sharing
```

For service classes

Unless another behavior is explicitly required.

---

# 3. CRUD Security

CRUD determines

Whether users may

- Create
- Read
- Update
- Delete

objects.

---

# Apex Does NOT Automatically Check CRUD

Example

```apex
insert account;
```

Will execute

Even if the user lacks Create permission

unless explicitly enforced.

---

# CRUD Check

```apex
if(Schema.sObjectType.Account.isCreateable()){

    insert acc;

}
```

---

# Useful Schema Methods

| Method | Checks |
|----------|---------|
| isCreateable() | Create |
| isUpdateable() | Update |
| isDeletable() | Delete |
| isAccessible() | Read |

---

# CRUD Example

```apex
if(Account.SObjectType.getDescribe().isAccessible()){

    List<Account> accs =
        [SELECT Name FROM Account];

}
```

---

# 4. Field-Level Security (FLS)

FLS controls

Whether a field

Is visible

Or editable.

Apex

Does not enforce it automatically.

---

# Schema Describe Methods

Example

```apex
Schema.DescribeFieldResult d =
Account.AnnualRevenue.getDescribe();

Boolean canRead =
d.isAccessible();
```

---

# Common Field Methods

| Method | Description |
|----------|-------------|
| isAccessible() | Read |
| isCreateable() | Create |
| isUpdateable() | Edit |

---

# Drawback

Schema checks become verbose

When many fields exist.

---

# 5. Security.stripInaccessible()

Recommended approach

For enforcing FLS.

Introduced to simplify field security.

---

## Example

```apex
List<Account> cleaned =

(List<Account>)
Security.stripInaccessible(

AccessType.READABLE,

accounts

).getRecords();
```

---

# Benefits

Automatically removes

Fields

The user

Cannot access.

Works for

- Query results
- DML
- Serialization

---

# Example

Account

```
Name

Visible

Salary__c

Hidden
```

Query

```
SELECT Name,

Salary__c

FROM Account
```

After

```
stripInaccessible()
```

Salary__c

Is removed.

---

# Advantages

- Easy to use
- Bulk-safe
- Cleaner code
- Salesforce recommended

---

# 6. WITH SECURITY_ENFORCED

Automatically enforces

CRUD

and

FLS

during SOQL.

---

# Example

```apex
SELECT

Name,

AnnualRevenue

FROM Account

WITH SECURITY_ENFORCED
```

---

# Behavior

If the user

Cannot access

AnnualRevenue

↓

Salesforce throws

```
QueryException
```

---

# Advantages

- Very simple
- Automatic enforcement
- Less code

---

# Limitations

Only applies

To SOQL.

Does not protect

DML.

---

# CRUD/FLS Comparison

| Method | CRUD | FLS | SOQL | DML |
|----------|------|-----|------|-----|
| Schema Methods | ✔ | ✔ | ✔ | ✔ |
| stripInaccessible() | ✔* | ✔ | ✔ | ✔ |
| WITH SECURITY_ENFORCED | ✔ | ✔ | ✔ | ✖ |

> **Note:** `Security.stripInaccessible()` primarily sanitizes field access. For complete object-level security, combine it with CRUD checks or User Mode operations where appropriate.

---

# 7. User Mode Database Operations (Modern Apex)

Recent Salesforce releases introduced **User Mode** DML and SOQL.

Example

```apex
Database.insert(account,

AccessLevel.USER_MODE);
```

Benefits

- Enforces CRUD
- Enforces FLS
- Respects sharing

Salesforce increasingly recommends

User Mode

For new development.

---

# 8. Programmatic Sharing

Programmatic Sharing

Creates

Share records

Using Apex.

---

# When to Use

Only when

Declarative sharing

Cannot satisfy

Business requirements.

---

# Flow

```
Business Logic

↓

Apex

↓

Share Object

↓

Additional Access
```

---

# Share Objects

Examples

| Object | Share Object |
|----------|--------------|
| Account | AccountShare |
| Case | CaseShare |
| Opportunity | OpportunityShare |
| Custom Object | CustomObject__Share |

---

# Important Share Fields

| Field | Purpose |
|----------|----------|
| ParentId | Record being shared |
| UserOrGroupId | Recipient |
| AccessLevel | Read/Edit |
| RowCause | Reason |

---

# Example

```apex
AccountShare share =
new AccountShare();

share.AccountId = acc.Id;

share.UserOrGroupId = userId;

share.AccountAccessLevel = 'Edit';

share.RowCause =
Schema.AccountShare.RowCause.Manual;

insert share;
```

---

# 9. RowCause

Indicates

Why

A share exists.

---

# Common Values

| RowCause | Meaning |
|-----------|---------|
| Owner | Owner access |
| Manual | Manual share |
| Rule | Sharing Rule |
| Team | Team sharing |
| Territory | Territory assignment |
| ImplicitParent | Parent sharing |
| ImplicitChild | Child sharing |
| Custom Apex Sharing Reason | Programmatic sharing |

---

# 10. Custom Apex Sharing Reasons

Available

Only

For Custom Objects.

---

## Example

```
Invoice__Share

↓

Project_Manager__c
```

Benefits

- Easier maintenance
- Prevent accidental deletion
- Better reporting
- Better debugging

---

# Best Practice

Always use

Custom Apex Sharing Reasons

For

Custom Object Sharing.

---

# 11. Sharing Recalculation

Whenever

Sharing changes

Salesforce recalculates

Visibility.

Triggers

- OWD changes
- Owner changes
- Role changes
- Territory changes
- Share record changes

---

# Recalculation Flow

```
Insert Share

↓

Update Share Table

↓

Recalculate

↓

Users Gain Access
```

---

# Apex Sharing Best Practices

- Bulkify share record creation.
- Avoid DML inside loops.
- Delete obsolete shares.
- Minimize unnecessary sharing.
- Use custom RowCause values.
- Prefer declarative sharing first.

---

# 12. Security Review Best Practices

For AppExchange

Salesforce performs

A mandatory

Security Review.

---

# Common Requirements

- Enforce CRUD
- Enforce FLS
- Respect sharing
- Prevent SOQL Injection
- Prevent XSS
- Prevent CSRF
- Avoid hardcoded IDs
- Validate user input

---

# Secure Coding Checklist

✅ Use

```
with sharing

or

inherited sharing
```

---

✅ Check CRUD

Before DML

---

✅ Check FLS

Before reading

Or writing fields

---

✅ Use

```
Security.stripInaccessible()
```

For field sanitization

---

✅ Use

```
WITH SECURITY_ENFORCED
```

For SOQL where appropriate

---

✅ Prefer

```
Database.insert(...,

AccessLevel.USER_MODE)
```

For modern Apex

---

# Common Architect Scenarios

### Scenario 1

Requirement

```
Users

Should only see

Records

Allowed by OWD
```

Solution

```
with sharing
```

---

### Scenario 2

Requirement

```
Background batch

Needs

All records
```

Solution

```
without sharing
```

(only when justified)

---

### Scenario 3

Requirement

```
Service class

Should follow

Calling context
```

Solution

```
inherited sharing
```

---

### Scenario 4

Requirement

```
Dynamic business rule

Cannot use

Sharing Rules
```

Solution

```
Apex Managed Sharing
```

---

### Scenario 5

Requirement

```
Protect field security

For queried records
```

Solution

```
Security.stripInaccessible()

or

WITH SECURITY_ENFORCED
```

---

# Architect Best Practices

- Default to **`inherited sharing`** for reusable service classes.
- Use **`with sharing`** for user-facing controllers and services.
- Avoid **`without sharing`** unless there is a documented business need.
- Enforce **CRUD**, **FLS**, and **record-level sharing** independently.
- Prefer **User Mode Database Operations** for new Apex development.
- Use **`Security.stripInaccessible()`** to sanitize queried and DML-bound records.
- Use **`WITH SECURITY_ENFORCED`** for SOQL when automatic CRUD/FLS validation is desired.
- Prefer declarative sharing before implementing **Apex Managed Sharing**.
- Bulkify all share record operations and monitor sharing recalculation impacts.

---

# Quick Decision Matrix

| Requirement | Best Solution |
|-------------|---------------|
| Respect record sharing | `with sharing` |
| Follow caller's sharing context | `inherited sharing` |
| Ignore sharing (admin utility) | `without sharing` |
| Check CRUD | Schema Describe / User Mode |
| Enforce FLS on queried records | `Security.stripInaccessible()` |
| Enforce CRUD/FLS in SOQL | `WITH SECURITY_ENFORCED` |
| Enforce CRUD/FLS in DML | User Mode DML |
| Complex record sharing | Apex Managed Sharing |

---

# Exam Tips ⭐⭐⭐⭐⭐

- **Apex runs in System Mode by default**, so security must be enforced explicitly.
- **`with sharing`** enforces record-level sharing; **`without sharing`** bypasses it; **`inherited sharing`** follows the caller's context.
- **Sharing keywords do not enforce CRUD or FLS**.
- Use **Schema Describe**, **`WITH SECURITY_ENFORCED`**, **`Security.stripInaccessible()`**, or **User Mode** to enforce object and field security.
- **`WITH SECURITY_ENFORCED`** only applies to **SOQL** and throws an exception when access is missing.
- **`Security.stripInaccessible()`** removes inaccessible fields from records and is recommended for secure Apex.
- **User Mode Database Operations** provide a modern way to enforce sharing, CRUD, and FLS for DML.
- Use **Apex Managed Sharing** only when declarative sharing mechanisms cannot satisfy the requirement.
- Always use **Custom Apex Sharing Reasons** for custom object programmatic sharing.
- Security Review expects proper enforcement of **sharing**, **CRUD**, **FLS**, and protection against common vulnerabilities such as SOQL injection.