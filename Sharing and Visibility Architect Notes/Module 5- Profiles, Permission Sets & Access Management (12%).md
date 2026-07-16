
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **Key Exam Principle**
>
> **Profiles and Permission Sets control WHAT a user can do.**
>
> They **do not determine WHICH records a user can access** (that's handled by OWD, Role Hierarchy, Sharing Rules, etc.).

---

# 1. Access Management Overview

Salesforce access management consists of several layers.

```
Authentication
        ↓
Profiles
        ↓
Permission Sets
        ↓
Permission Set Groups
        ↓
Feature Licenses
        ↓
Sharing Model
```

Remember

```
Profiles

↓

Permission Sets

↓

Permission Set Groups

Control

"What can the user do?"

NOT

"What records can the user see?"
```

---

# 2. Profiles

Every user must have **exactly one Profile**.

A Profile defines the **minimum permissions** required for a user's job.

Examples

- System Administrator
- Standard User
- Read Only
- Custom Sales Profile

---

## What a Profile Controls

Profiles can control

- Object Permissions (CRUD)
- Field-Level Security
- Login Hours
- Login IP Ranges
- Tabs
- Apps
- Record Types
- Page Layout Assignments
- Apex Class Access
- Visualforce Page Access
- Flow Access
- User Permissions

---

# CRUD Permissions

CRUD stands for

| Permission | Description |
|------------|-------------|
| Create | Create records |
| Read | View records |
| Update (Edit) | Modify records |
| Delete | Delete records |

Example

```
Opportunity

Read ✔

Create ✔

Edit ✔

Delete ✖
```

The user cannot delete Opportunities even if they own the record.

---

# View All vs Modify All

## View All

- Read every record
- Ignores sharing
- Cannot edit without Edit permission

---

## Modify All

- Read
- Edit
- Delete
- Transfer ownership
- Ignores sharing

---

# Object Permission Flow

```
User

↓

Profile

↓

Permission Set

↓

Effective Permission

↓

Record Access
```

Object permission is checked **before** record sharing.

---

# Login Hours

Profiles can restrict when users can log in.

Example

```
Monday-Friday

09:00

↓

18:00
```

Outside these hours

Login denied.

---

# Login IP Ranges

Restrict login locations.

Example

```
Corporate Office

192.168.x.x

Allowed

Home Network

Blocked
```

Useful for

- Banking
- Government
- High-security organizations

---

# Tabs

Profiles determine

- Default On
- Default Off
- Hidden

Example

```
Cases

Visible

Campaigns

Hidden
```

---

# Apps

Profiles determine

Which Lightning Apps users can access.

Example

Sales App

✔

Service Console

✖

---

# Record Types

Profiles determine

Which Record Types users can create and use.

Example

Opportunity

Record Types

- Direct Sales ✔
- Partner Sales ✔
- Government Sales ✖

---

# Page Layout Assignment

Profiles assign

Different layouts

For different Record Types.

Example

```
Sales Profile

↓

Sales Layout

Support Profile

↓

Support Layout
```

---

# 3. Permission Sets

Permission Sets grant **additional permissions**.

Think of them as

```
Profile

+

Extra Permissions
```

Users may have

```
One Profile

+

Many Permission Sets
```

---

# Permission Set Benefits

- No profile cloning
- Flexible security
- Easier maintenance
- Supports least privilege

Salesforce recommends

Prefer Permission Sets over creating many Profiles.

---

# Object Permissions

Grant

- CRUD
- View All
- Modify All

Without modifying the Profile.

---

# Field Permissions

Grant visibility or edit access

To individual fields.

Example

```
Salary__c

Profile

Hidden

↓

Permission Set

Visible
```

Effective access

Visible.

---

# Apex Class Access

Controls whether users can execute Apex classes.

Example

```
InvoiceController

Permission Set

Granted
```

Without Apex access,

users cannot invoke the class.

---

# Visualforce Access

Permission Sets grant access

To Visualforce Pages.

Example

```
InvoicePage

Accessible

Only Finance Team
```

---

# Flow Access

Controls

Who can run Flows.

Examples

- Screen Flow
- Autolaunched Flow
- Flow Orchestration

---

# Custom Permissions

Custom Permissions allow developers

To control functionality.

Example

```
Custom Permission

Approve_Discount
```

Used in

- Apex
- Validation Rules
- Flows
- Lightning Components

Example Validation Rule

```
IF(
NOT($Permission.Approve_Discount),
TRUE,
FALSE
)
```

---

# Permission Evaluation

```
Profile

Read

↓

Permission Set

Edit

↓

Effective Permission

Edit
```

Permissions are cumulative.

Permission Sets only **add** permissions.

They never remove them.

---

# 4. Permission Set Groups

Permission Set Groups combine

Multiple Permission Sets

Into one assignable unit.

Example

```
Sales Group

↓

Accounts

↓

Opportunities

↓

Reports

↓

Forecasts
```

Instead of assigning

Four Permission Sets,

Assign one group.

---

# Benefits

- Easier administration
- Cleaner security model
- Better scalability
- Job-based access

---

# Permission Set Group Example

```
Sales Representative

↓

Sales Group

Contains

Opportunity Access

Report Access

Forecast Access

Quote Access
```

---

# 5. Muting Permission Sets

Muting removes permissions

Inside a Permission Set Group.

Important

Muting

Only works

Inside Permission Set Groups.

---

## Example

Permission Set

```
Delete Opportunity

Granted
```

Muted

```
Delete Opportunity

Removed
```

Effective permission

No Delete.

---

# Why Muting Exists

Without muting

You would need many similar Permission Sets.

Muting reduces duplication.

---

# Combined Permissions

Effective permission equals

```
Profile

+

Permission Sets

+

Permission Set Groups

-

Muted Permissions
```

Salesforce evaluates

Highest effective permission.

---

# Permission Resolution

```
Profile

Read

↓

Permission Set

Edit

↓

Permission Set Group

Delete

↓

Mute Delete

↓

Final

Read + Edit
```

---

# 6. Delegated Administration

Delegated Administration allows

Non-admin users

To perform limited administrative tasks.

Examples

- Reset passwords
- Unlock users
- Manage users in assigned roles
- Assign selected Permission Sets

---

# Typical Use Cases

- Regional IT
- Department Admins
- HR administrators
- Business support teams

---

# Delegated Admin Cannot

- Modify organization settings
- Change Profiles (unless delegated)
- Customize Salesforce
- Deploy metadata

---

# 7. Feature Licenses

Some Salesforce features require

Feature Licenses.

Examples

- Knowledge
- CRM Analytics
- Inbox
- CPQ
- Maps

Having object permission alone is not enough.

User must also have

Appropriate Feature License.

---

# License Relationship

```
User License

↓

Feature License

↓

Permission Set

↓

Feature Access
```

---

# Example

CPQ

Requires

```
Salesforce License

+

CPQ License

+

CPQ Permission Set
```

---

# 8. Session Settings

Session Settings improve security.

Examples

- Session Timeout
- Lock Sessions to IP
- Lock Sessions to Domain
- Require Reauthentication
- High Assurance Sessions

---

# High Assurance Sessions

Some operations require

Stronger authentication.

Example

```
Login

↓

MFA

↓

High Assurance

↓

Modify Security Settings
```

---

# Session Security Best Practices

- Enable MFA.
- Use reasonable session timeouts.
- Restrict sessions by IP where appropriate.
- Require reauthentication for sensitive actions.

---

# Profiles vs Permission Sets

| Profile | Permission Set |
|----------|----------------|
| One per user | Many per user |
| Required | Optional |
| Baseline permissions | Additional permissions |
| Defines minimum access | Adds extra access |
| Cannot be removed | Can be assigned/removed anytime |

---

# Permission Set vs Permission Set Group

| Permission Set | Permission Set Group |
|----------------|----------------------|
| Single permission collection | Multiple Permission Sets |
| Assigned individually | Assigned as one unit |
| No muting | Supports muting |

---

# Profiles vs Sharing

| Profiles | Sharing |
|-----------|----------|
| CRUD | Record visibility |
| FLS | OWD |
| Login Hours | Role Hierarchy |
| Tabs | Sharing Rules |
| Apps | Apex Sharing |

Profiles answer

**What can you do?**

Sharing answers

**Which records can you access?**

---

# Architect Best Practices

- Keep the number of Profiles as low as possible.
- Use Profiles only for **baseline access**.
- Grant additional permissions through **Permission Sets**.
- Bundle job-based permissions with **Permission Set Groups**.
- Use **Muting Permission Sets** to create exceptions instead of cloning permission sets.
- Implement the **Principle of Least Privilege**.
- Use **Custom Permissions** instead of hardcoding profile names in Apex, Flows, or Validation Rules.
- Regularly audit user permissions and remove unused access.
- Enable **MFA** and appropriate session controls for sensitive environments.

---

# Common Exam Scenarios

### Requirement

```
Finance users

Need temporary access

To Reports
```

Best Solution

✅ Permission Set

---

### Requirement

```
Sales Users

Need

Opportunity

Quote

Forecast

Reports
```

Best Solution

✅ Permission Set Group

---

### Requirement

```
Remove Delete

From one job role

Inside a Permission Set Group
```

Best Solution

✅ Muting Permission Set

---

### Requirement

```
Business support team

Should reset passwords

Without System Administrator access
```

Best Solution

✅ Delegated Administration

---

### Requirement

```
Restrict user logins

Outside office hours
```

Best Solution

✅ Profile Login Hours

---

# Exam Tips ⭐⭐⭐⭐⭐

- Every user has **exactly one Profile** but can have **multiple Permission Sets**.
- **Profiles provide baseline permissions**; **Permission Sets only add permissions**—they never remove them.
- **Permission Set Groups** simplify administration by combining multiple Permission Sets into one assignment.
- **Muting Permission Sets** can only be used within a **Permission Set Group** to suppress specific permissions.
- Profiles and Permission Sets control **CRUD, FLS, Apex, Flows, Visualforce, Tabs, Apps, Login Hours, and Login IPs**.
- **Profiles do not grant record access**. Record visibility is controlled by OWD, Role Hierarchy, Sharing Rules, Teams, Territories, and Apex Sharing.
- Use **Custom Permissions** instead of checking Profile names in automation or Apex.
- **Delegated Administration** is ideal for limited user-management tasks without granting full administrator rights.
- Many Salesforce features require both a **Feature License** and the appropriate **Permission Set**.
- Salesforce best practice is **fewer Profiles, more Permission Sets**.