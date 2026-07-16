
**Salesforce Sharing & Visibility Architect Certification Notes**

---

# 1. Salesforce Security Model

Salesforce security is implemented in **multiple layers**. Every user must pass through each layer before gaining access to data.

```
Authentication
      ↓
Authorization
      ↓
Object-Level Security
      ↓
Field-Level Security
      ↓
Record-Level Security
      ↓
Sharing & Visibility
      ↓
UI & Apex Enforcement
```

Think of it like a building:

- Authentication → Enter the building
- Authorization → Allowed floor
- Object Security → Allowed room
- Field Security → Allowed drawer
- Record Security → Allowed document

---

# 2. Authentication vs Authorization

## Authentication

Authentication answers:

> **Who are you?**

It verifies user identity.

Examples

- Username + Password
- Multi-Factor Authentication (MFA)
- Single Sign-On (SSO)
- OAuth Login
- Login IP Restrictions
- Login Hours

Example

```
Saransh logs into Salesforce.

Salesforce verifies:

Username ✔
Password ✔
MFA ✔

Authentication Successful
```

Authentication **does NOT decide what data you can access.**

---

## Authorization

Authorization answers:

> **What are you allowed to do?**

Examples

- Read Accounts
- Edit Opportunities
- Delete Cases
- View Salary field
- Access specific records

Authorization is controlled by

- Profiles
- Permission Sets
- Permission Set Groups
- Sharing Rules
- Roles
- Teams
- Territory Management

Example

```
Two users login successfully.

Admin
    Can edit everything

Sales Rep
    Can edit only own Accounts
```

Same authentication.

Different authorization.

---

# Authentication vs Authorization

| Authentication | Authorization |
|---------------|---------------|
| Verifies identity | Determines permissions |
| Login process | Access control |
| Username/Password | Profiles/Permission Sets |
| Happens first | Happens after login |
| Who are you? | What can you do? |

---

# 3. Salesforce Security Layers

Salesforce security consists of several layers.

## Layer 1 — Organization Access

Controls who can enter the org.

Includes

- Username
- Password
- MFA
- SSO
- Login Hours
- Login IP Range
- Trusted IP

---

## Layer 2 — Object Security

Controls whether a user can access an object.

Example

```
Account

Read ✔
Create ✔
Edit ✔
Delete ✖
```

Configured using

- Profiles
- Permission Sets

---

## Layer 3 — Field Security

Controls access to fields.

Example

Salary__c

```
HR User

Visible ✔
Editable ✔

Sales User

Visible ✖
```

Configured using

- Field-Level Security (FLS)

---

## Layer 4 — Record Security

Controls which individual records users can access.

Example

Two Account records

```
ABC Ltd

Owner:
John

XYZ Ltd

Owner:
Sarah
```

If OWD = Private

John cannot see Sarah's Account unless sharing grants access.

---

## Layer 5 — Apex Security

Custom code must enforce security.

Tools

- WITH SECURITY_ENFORCED
- Security.stripInaccessible()
- User Mode Database Operations
- UserInfo
- sharing keywords

---

## Layer 6 — UI Security

Lightning Experience only shows what the user can access.

Examples

- Hidden fields
- Hidden buttons
- Hidden tabs
- Dynamic Forms
- Dynamic Actions

---

# Complete Security Flow

```
User Login
     ↓
Authentication
     ↓
Authorization
     ↓
Object Access
     ↓
Field Access
     ↓
Record Access
     ↓
Sharing Rules
     ↓
UI Displays Allowed Data
```

---

# 4. Object Security

Determines access to an object.

Permissions

- Read
- Create
- Edit
- Delete
- View All
- Modify All

Configured using

- Profile
- Permission Set

Example

```
Case

Read ✔
Create ✔
Edit ✖
Delete ✖
```

User can create cases but cannot modify them.

---

## View All vs Modify All

### View All

Can read every record regardless of sharing.

Cannot edit unless Edit permission exists.

### Modify All

Full control.

Ignores sharing.

Can

- Read
- Edit
- Delete
- Transfer ownership

---

# Exam Tip

**View All ≠ View All Data**

- View All → Object level
- View All Data → Entire org

---

# 5. Field-Level Security (FLS)

Determines whether a field is

- Visible
- Editable

Example

```
Employee

Salary

HR
Visible ✔

Sales
Hidden ✖
```

Configured using

- Profile
- Permission Set

---

## Field Access Matrix

| Access | Result |
|----------|---------|
| Hidden | Cannot see |
| Read Only | Can view |
| Editable | Can edit |

---

# 6. Record-Level Security

Object access says

> Can you access Accounts?

Record access says

> Which Accounts?

Controlled by

- OWD
- Role Hierarchy
- Sharing Rules
- Manual Sharing
- Teams
- Territories
- Apex Sharing

Example

```
OWD = Private

John owns Account A

Sarah owns Account B

John cannot see Sarah's Account.
```

---

# 7. Apex Security

Developers must explicitly enforce security.

Important keywords

```
with sharing
without sharing
inherited sharing
```

Security APIs

```
WITH SECURITY_ENFORCED

Security.stripInaccessible()

Database.insert(record, AccessLevel.USER_MODE)
```

---

## WITH SHARING

```
public with sharing class AccountController
```

Respects sharing rules.

---

## WITHOUT SHARING

```
public without sharing class AdminClass
```

Ignores sharing.

Dangerous.

---

## INHERITED SHARING

Uses caller's sharing context.

Recommended by Salesforce.

---

# 8. UI Security

Salesforce hides components users cannot access.

Examples

- Hidden Tabs
- Hidden Fields
- Hidden Buttons
- Dynamic Forms
- Dynamic Actions

Remember

UI security alone is **not sufficient**.

Always enforce security in Apex.

---

# 9. Principle of Least Privilege

One of the most important architect principles.

Definition

Users receive only the minimum permissions required to perform their job.

Benefits

- Better security
- Reduced risk
- Easier auditing
- Better compliance

Example

Finance User

Needs

- Read Accounts
- Edit Invoices

Should NOT receive

- Modify All Data
- Customize Application

---

# 10. Need-to-Know Access

Users should access information only if required.

Example

Sales

Needs

- Accounts
- Opportunities

Should not access

- Payroll
- Employee Salary

---

# Least Privilege vs Need-to-Know

| Least Privilege | Need-to-Know |
|-----------------|-------------|
| Minimum permissions | Minimum information |
| Focus on actions | Focus on data |
| Security principle | Data privacy principle |

---

# 11. Data Ownership

Every record has an owner.

Owner can be

- User
- Queue (supported objects)

Owner determines

- Default access
- Sharing
- Reports
- Role hierarchy visibility

Example

```
Opportunity

Owner

John
```

John automatically has access.

---

# Record Owner Capabilities

Usually owners can

- Read
- Edit
- Delete
- Share (if allowed)

---

# 12. Data Isolation

Large enterprises often require data separation.

Examples

- Country-specific data
- Business Unit isolation
- Subsidiary separation
- Government regulations

Implemented using

- OWD
- Role Hierarchy
- Restriction Rules
- Territory Management
- Sharing Rules
- Enterprise Territory Management
- Apex Sharing

---

# Example

```
USA Sales

Can access USA Accounts

India Sales

Cannot access USA Accounts
```

---

# 13. Enterprise Sharing Strategy

Architects design a sharing strategy before implementation.

Typical approach

```
OWD

↓

Role Hierarchy

↓

Sharing Rules

↓

Teams

↓

Territories

↓

Apex Sharing
```

Salesforce Best Practice

Start restrictive.

Open access only when necessary.

> "Private first, then grant access."

---

# Questions an Architect Should Ask

- Who owns the data?
- Who needs access?
- Read only or Edit?
- Temporary or permanent?
- Internal or external users?
- Regulatory requirements?
- Performance impact?

---

# 14. Security vs Sharing

This is one of the most frequently tested topics.

## Security

Controls

**Can the user perform an action?**

Examples

- Login
- Read object
- Edit field

Tools

- Profiles
- Permission Sets
- MFA
- FLS

---

## Sharing

Controls

**Which records can the user access?**

Tools

- OWD
- Role Hierarchy
- Sharing Rules
- Manual Sharing
- Teams
- Territories
- Apex Sharing

---

# Security vs Sharing Comparison

| Security | Sharing |
|-----------|----------|
| Authentication | Record access |
| Authorization | Visibility |
| Profiles | OWD |
| Permission Sets | Sharing Rules |
| Field Security | Role Hierarchy |
| Object Permissions | Apex Sharing |

---

# 15. Architect Best Practices

- Start with **Private OWD** whenever possible.
- Follow the **Principle of Least Privilege**.
- Grant access through **Permission Sets**, not multiple Profiles.
- Use **Permission Set Groups** for job-based access.
- Use **Sharing Rules** instead of manual sharing whenever possible.
- Avoid **Modify All Data** unless absolutely necessary.
- Use **inherited sharing** for Apex classes where appropriate.
- Enforce FLS and CRUD in Apex using `WITH SECURITY_ENFORCED`, `Security.stripInaccessible()`, or User Mode operations.
- Design for **scalability** and **maintainability**, not just immediate requirements.

---

# Exam Tips (High Priority ⭐⭐⭐⭐⭐)

- **Authentication** = Who are you?
- **Authorization** = What can you do?
- **Object Security** = Which objects?
- **Field Security** = Which fields?
- **Record Security** = Which records?
- **Sharing** never grants object permissions—it only grants access to records.
- **OWD** is the baseline for record access.
- **Profiles** and **Permission Sets** do **not** grant record access by themselves.
- **View All** is object-level; **View All Data** is org-wide.
- **Modify All** ignores sharing for that object.
- **Always enforce security in Apex**—UI restrictions alone are not enough.
```