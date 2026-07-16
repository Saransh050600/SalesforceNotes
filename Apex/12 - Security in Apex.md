
Security is one of the most important aspects of Apex development.

By default, Apex runs in **System Mode**, meaning it ignores many user permissions unless explicitly enforced.

As a Salesforce developer, you must ensure that users can only access data they are authorized to view or modify.

---

# Why Security Matters?

Without proper security:

```text
User
    ↓
Apex Class
    ↓
Sensitive Data Exposed
```

Potential issues:

- Unauthorized record access
- Unauthorized field access
- Data leakage
- Compliance violations

---

# Security Layers in Salesforce

```text
Organization Security
        ↓
Object Security (CRUD)
        ↓
Field Security (FLS)
        ↓
Record Security (Sharing)
        ↓
Apex Security Enforcement
```

---

# Security Concepts Covered

1. CRUD
2. FLS
3. Sharing Rules
4. With Sharing
5. Without Sharing
6. Security.stripInaccessible()

---

# 1. CRUD

CRUD stands for:

| Operation | Meaning |
|------------|------------|
| Create | Create records |
| Read | View records |
| Update | Edit records |
| Delete | Delete records |

CRUD controls access at the **object level**.

---

# Example

User has:

```text
Read Account
Update Account
```

User does NOT have:

```text
Delete Account
```

Apex should respect these permissions.

---

# Check CRUD Permissions

Use Schema methods.

---

## Create Permission

```apex
if(
    Schema.sObjectType.Account
    .isCreateable()
){

}
```

---

## Read Permission

```apex
if(
    Schema.sObjectType.Account
    .isAccessible()
){

}
```

---

## Update Permission

```apex
if(
    Schema.sObjectType.Account
    .isUpdateable()
){

}
```

---

## Delete Permission

```apex
if(
    Schema.sObjectType.Account
    .isDeletable()
){

}
```

---

# Example

```apex
if(
    Schema.sObjectType.Account
    .isCreateable()
){

    insert accountRecord;
}
```

---

# CRUD Helper Example

```apex
public static Boolean canCreateAccount(){

    return Schema.sObjectType.Account
        .isCreateable();
}
```

---

# 2. FLS (Field-Level Security)

FLS controls access to individual fields.

---

# Example

User may access:

```text
Account.Name
```

But not:

```text
Account.AnnualRevenue
```

---

# Field Access Checks

---

## Read Access

```apex
if(
    Schema.sObjectType.Account
        .fields.Name
        .isAccessible()
){

}
```

---

## Update Access

```apex
if(
    Schema.sObjectType.Account
        .fields.Name
        .isUpdateable()
){

}
```

---

## Create Access

```apex
if(
    Schema.sObjectType.Account
        .fields.Name
        .isCreateable()
){

}
```

---

# Example

```apex
if(
    Account.Name
    .getDescribe()
    .isAccessible()
){

    System.debug(
        account.Name
    );
}
```

---

# CRUD vs FLS

| CRUD | FLS |
|--------|--------|
| Object Level | Field Level |
| Account | Account.Name |
| Contact | Contact.Email |

---

# Example

User can:

```text
Read Account
```

But cannot:

```text
Read AnnualRevenue
```

Meaning:

```text
CRUD = Yes
FLS = No
```

---

# 3. Sharing Rules

Sharing Rules control **record-level access**.

---

# Record Access Layers

```text
OWD
     ↓
Role Hierarchy
     ↓
Sharing Rules
     ↓
Manual Sharing
     ↓
Teams
```

---

# Organization-Wide Defaults (OWD)

Base level record access.

Examples:

```text
Private
Public Read Only
Public Read/Write
Controlled by Parent
```

---

# Example

OWD:

```text
Account = Private
```

User A owns:

```text
Account A
```

User B cannot see it unless access is granted.

---

# Sharing Rule

Automatically grants access.

Example:

```text
Share All Accounts
With Sales Team
```

---

# Manual Sharing

User manually shares a record.

```text
Account
↓
Sharing Button
↓
Grant Access
```

---

# Apex Managed Sharing

Programmatic sharing.

Example:

```apex
AccountShare share =
new AccountShare();

share.AccountId =
    accountId;

share.UserOrGroupId =
    userId;

share.AccountAccessLevel =
    'Read';

insert share;
```

---

# 4. With Sharing

Enforces record-level security.

---

# Example

```apex
public with sharing class
AccountService {

}
```

---

# Behavior

If user can access:

```text
10 Accounts
```

Query returns:

```text
10 Accounts
```

Even if database contains:

```text
1000 Accounts
```

---

# Example

```apex
public with sharing class
AccountService {

    public static List<Account>
    getAccounts(){

        return
        [
            SELECT Id, Name
            FROM Account
        ];
    }
}
```

Only accessible records returned.

---

# Best Practice

✅ Most business classes should use:

```apex
with sharing
```

---

# 5. Without Sharing

Ignores record-level security.

---

# Example

```apex
public without sharing class
AccountAdminService {

}
```

---

# Behavior

User sees:

```text
All Records
```

even if sharing model is private.

---

# Example

```apex
public without sharing class
AdminUtility {

    public static List<Account>
    getAllAccounts(){

        return
        [
            SELECT Id, Name
            FROM Account
        ];
    }
}
```

---

# Use Cases

- Administrative utilities
- Background jobs
- System-level processing

Use carefully.

---

# Inherited Sharing

Recommended modern approach.

---

## Example

```apex
public inherited sharing class
AccountService {

}
```

Behavior:

```text
Caller Has Sharing
↓
Class Uses Sharing

Caller Without Sharing
↓
Class Uses Without Sharing
```

---

# Sharing Keyword Comparison

| Keyword | Record Security |
|-----------|-----------|
| with sharing | Enforced |
| without sharing | Ignored |
| inherited sharing | Inherited |

---

# Important Note

Sharing keywords affect:

```text
Record Access
```

ONLY.

They do NOT enforce:

```text
CRUD
FLS
```

---

# 6. Security.stripInaccessible()

Modern Salesforce security enforcement method.

Automatically removes inaccessible fields.

---

# Why Use stripInaccessible?

Before:

```apex
Hundreds of Manual FLS Checks
```

After:

```apex
Single Method Call
```

---

# Query Example

```apex
List<Account> accounts =
[
    SELECT Name,
           AnnualRevenue
    FROM Account
];
```

---

# Secure Records

```apex
SObjectAccessDecision decision =
Security.stripInaccessible(
    AccessType.READABLE,
    accounts
);

List<Account> secureAccounts =
(List<Account>)
decision.getRecords();
```

---

# Insert Example

```apex
Account acc =
new Account();

acc.Name = 'ABC';

acc.AnnualRevenue = 100000;
```

---

## Remove Non-Creatable Fields

```apex
SObjectAccessDecision decision =
Security.stripInaccessible(
    AccessType.CREATABLE,
    new List<Account>{acc}
);

insert decision.getRecords();
```

---

# Update Example

```apex
SObjectAccessDecision decision =
Security.stripInaccessible(
    AccessType.UPDATABLE,
    accountList
);

update decision.getRecords();
```

---

# Access Types

| Type | Purpose |
|--------|--------|
| READABLE | Query Security |
| CREATABLE | Insert Security |
| UPDATABLE | Update Security |
| UPSERTABLE | Upsert Security |

---

# Secure Query Example

```apex
List<Account> accounts =
[
    SELECT Name,
           AnnualRevenue
    FROM Account
];

accounts =
(List<Account>)
Security.stripInaccessible(
    AccessType.READABLE,
    accounts
).getRecords();
```

---

# Secure Insert Example

```apex
List<Account> accounts =
new List<Account>();

accounts.add(
    new Account(
        Name='Test'
    )
);

accounts =
(List<Account>)
Security.stripInaccessible(
    AccessType.CREATABLE,
    accounts
).getRecords();

insert accounts;
```

---

# User Mode Operations (Modern Security)

Salesforce now supports User Mode operations.

---

## Query in User Mode

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    WITH USER_MODE
];
```

Enforces:

```text
CRUD
FLS
Sharing
```

---

## DML in User Mode

```apex
insert as user accountRecord;
```

---

## Update in User Mode

```apex
update as user accountRecord;
```

---

# Security Enforcement Strategy

Modern recommended approach:

```text
1. inherited sharing
2. WITH USER_MODE
3. stripInaccessible()
```

---

# Common Security Mistakes

---

## Missing Sharing Declaration

❌

```apex
public class AccountService
```

---

✅

```apex
public inherited sharing class
AccountService
```

---

## Ignoring FLS

❌

```apex
SELECT AnnualRevenue
FROM Account
```

---

✅

```apex
Security.stripInaccessible()
```

---

## Ignoring CRUD

❌

```apex
insert accountRecord;
```

---

✅

```apex
if(
    Schema.sObjectType.Account
    .isCreateable()
){
    insert accountRecord;
}
```

---

# Security Best Practices

### Use inherited sharing

```apex
public inherited sharing class
```

---

### Use WITH USER_MODE

```apex
SELECT Id FROM Account
WITH USER_MODE
```

---

### Use stripInaccessible()

```apex
Security.stripInaccessible()
```

---

### Validate CRUD Before DML

```apex
isCreateable()
isUpdateable()
isDeletable()
```

---

### Never Assume User Access

Always verify access before reading or modifying data.

---

# Interview Questions

### What is CRUD?

Object-level permissions controlling Create, Read, Update, and Delete access.

---

### What is FLS?

Field-Level Security controls access to individual fields.

---

### Difference Between CRUD and FLS?

| CRUD | FLS |
|--------|--------|
| Object Security | Field Security |
| Account | Account.Name |

---

### Difference Between With Sharing and Without Sharing?

| With Sharing | Without Sharing |
|--------------|----------------|
| Enforces record access | Ignores record access |
| Respects sharing rules | Returns all records |

---

### Does With Sharing Enforce CRUD and FLS?

❌ No

It only enforces:

```text
Record-Level Security
```

---

### What Does Security.stripInaccessible() Do?

Removes fields the current user cannot access based on FLS.

---

### What Is the Recommended Modern Security Pattern?

```apex
public inherited sharing class ServiceClass {

    public static List<Account> getAccounts(){

        return [
            SELECT Id, Name
            FROM Account
            WITH USER_MODE
        ];
    }
}
```

This enforces:

```text
Sharing
CRUD
FLS
```

and represents Salesforce's current security best practice.

---

# Apex Security Cheat Sheet

## Security Types

| Security Mechanism | Record-Level Security | Object-Level Security (CRUD) | Field-Level Security (FLS) | Throws Exception | Use Case |
|-------------------|----------------------|-----------------------------|----------------------------|-----------------|----------|
| `with sharing` | ✅ | ❌ | ❌ | ❌ | Enforce sharing rules |
| `without sharing` | ❌ | ❌ | ❌ | ❌ | Ignore sharing rules |
| `inherited sharing` | Depends on caller | ❌ | ❌ | ❌ | Recommended for service classes |
| `WITH USER_MODE` | ✅ | ✅ | ✅ | ✅ | Modern secure SOQL |
| `WITH SECURITY_ENFORCED` | ❌ | ✅ | ✅ | ✅ | Legacy secure SOQL |
| `Security.stripInaccessible()` | ❌ | ❌ | ✅ | ❌ | Remove inaccessible fields |
| `insert/update/delete as user` | ✅ | ✅ | ✅ | ✅ | Modern secure DML |
| `Database.*(..., AccessLevel.USER_MODE)` | ✅ | ✅ | ✅ | ✅ | Modern secure DML |
| `insert/update/delete` | Depends on sharing keyword | ❌ | ❌ | ❌ | System mode DML |
| `Schema.Describe` Methods | ❌ | ✅ | ✅ | ❌ | Manual CRUD/FLS checks |

---

# Record-Level Security

## with sharing

```apex
public with sharing class AccountService {

}
```

### Enforces

- Organization-Wide Defaults (OWD)
- Role Hierarchy
- Sharing Rules
- Manual Sharing
- Teams/Territories

### Does NOT Enforce

- CRUD
- FLS

---

## without sharing

```apex
public without sharing class AccountService {

}
```

Ignores sharing rules and runs with full record access.

---

## inherited sharing

```apex
public inherited sharing class AccountService {

}
```

Uses the sharing mode of the caller.

Recommended by Salesforce for service-layer classes.

---

# Object-Level & Field-Level Security

## WITH USER_MODE (Recommended)

```apex
List<Account> accounts = [
    SELECT Id, Name
    FROM Account
    WITH USER_MODE
];
```

### Enforces

- Sharing
- CRUD
- FLS

### Behavior

Throws exception if access is denied.

---

## WITH SECURITY_ENFORCED (Legacy)

```apex
List<Account> accounts = [
    SELECT Id, Name
    FROM Account
    WITH SECURITY_ENFORCED
];
```

### Enforces

- CRUD
- FLS

### Does NOT Enforce

- Sharing

### Behavior

Throws exception if access is denied.

---

# Security.stripInaccessible()

Used to remove fields that the user cannot access.

```apex
accounts =
(List<Account>)
Security.stripInaccessible(
    AccessType.CREATABLE,
    accounts
).getRecords();
```

### Enforces

- FLS

### Does NOT Enforce

- CRUD
- Sharing

### Behavior

Removes restricted fields instead of throwing an exception.

---

# AccessType Values

| AccessType | Usage |
|------------|--------|
| `READABLE` | Before reading records |
| `CREATABLE` | Before insert |
| `UPDATABLE` | Before update |
| `UPSERTABLE` | Before upsert |

Example:

```apex
Security.stripInaccessible(
    AccessType.UPDATABLE,
    accounts
);
```

---

# User Mode DML (Recommended)

## Insert

```apex
insert as user account;
```

or

```apex
Database.insert(
    account,
    AccessLevel.USER_MODE
);
```

## Update

```apex
update as user account;
```

## Delete

```apex
delete as user account;
```

### Enforces

- Sharing
- CRUD
- FLS

### Behavior

Throws exception if access is denied.

---

# System Mode DML

```apex
insert account;
update account;
delete account;
```

### Enforces

- Depends on sharing keyword

### Does NOT Enforce

- CRUD
- FLS

Runs in system mode.

---

# Manual CRUD/FLS Checks

## Object Permission

```apex
if(Schema.sObjectType.Account.isCreateable()) {

}
```

## Read Permission

```apex
if(Schema.sObjectType.Account.isAccessible()) {

}
```

## Update Permission

```apex
if(Schema.sObjectType.Account.isUpdateable()) {

}
```

## Delete Permission

```apex
if(Schema.sObjectType.Account.isDeletable()) {

}
```

## Field Permission

```apex
if(Account.Name.getDescribe().isAccessible()) {

}
```

```apex
if(Account.Name.getDescribe().isUpdateable()) {

}
```

```apex
if(Account.Name.getDescribe().isCreateable()) {

}
```

---

# Modern Security Recommendation

| Operation | Recommended Approach |
|------------|---------------------|
| SOQL | `WITH USER_MODE` |
| DML | `insert/update/delete as user` |
| Service Classes | `inherited sharing` |
| Remove Restricted Fields | `Security.stripInaccessible()` |
| Manual Checks | `Schema.Describe` Methods |

---

# Interview Summary

| Requirement | Best Solution |
|-------------|--------------|
| Record Access | `with sharing` / `inherited sharing` |
| Secure SOQL | `WITH USER_MODE` |
| Secure DML | `insert as user` |
| Remove Restricted Fields | `Security.stripInaccessible()` |
| Manual CRUD/FLS Validation | `Schema.Describe` |
| Legacy SOQL Security | `WITH SECURITY_ENFORCED` |

---

# Interview One-Liner

> Apex security consists of three layers: Record-Level Security (Sharing), Object-Level Security (CRUD), and Field-Level Security (FLS). Modern Salesforce development uses `inherited sharing`, `WITH USER_MODE`, and User Mode DML (`insert as user`) to automatically enforce all three layers. `Security.stripInaccessible()` is used when inaccessible fields should be removed instead of throwing exceptions.