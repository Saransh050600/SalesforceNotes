
**Salesforce Sharing & Visibility Architect Certification Notes**

---

# 1. Introduction

Role Hierarchy is one of the **core record-sharing mechanisms** in Salesforce.

It provides **vertical access** to records based on an organization's reporting structure.

A Role Hierarchy answers the question:

> **Who should automatically gain access because they manage someone?**

It does **not** grant object permissions or field permissions—it only expands **record-level access**.

---

# 2. Record Ownership

Every Salesforce record has an **Owner**.

The owner is the user (or queue for supported objects) responsible for that record.

Owner examples:

- User
- Queue (supported objects like Case, Lead, Custom Objects)

Example

```
Account

ABC Corporation

Owner

John Smith
```

John automatically has access to the Account.

---

# Ownership Determines

Ownership influences

- Default record access
- Sharing calculations
- Role hierarchy visibility
- Reports
- Dashboards
- Assignment Rules
- Escalation Rules (supported objects)

---

# Record Owner Capabilities

Normally the owner can

- Read
- Edit
- Delete
- Share (if object supports manual sharing)

Example

```
Opportunity

Owner

Sarah

Sarah

✔ Read
✔ Edit
✔ Delete
```

---

# Ownership vs Object Permission

Example

```
User owns Opportunity

But Profile

Read Only
```

Result

The user **cannot edit** the record.

Ownership **does not override** object permissions.

Both are required.

---

# Ownership Flow

```
Record Created

↓

Owner Assigned

↓

OWD Applied

↓

Role Hierarchy Evaluated

↓

Sharing Rules

↓

Additional Sharing
```

---

# 3. Owner-Based Access

Owner-based access means

The record owner always receives access according to their object permissions.

Example

```
OWD

Private

Owner

John

Result

John still has access.
```

Even when OWD is Private,

owners keep access.

---

# Ownership Transfer

Changing ownership may trigger

- Sharing recalculation
- Territory recalculation
- Team updates
- Apex sharing recalculation

Large ownership changes can significantly affect performance.

---

# Ownership Best Practices

- Avoid frequent mass ownership changes.
- Use Assignment Rules when possible.
- Plan ownership during data migration.
- Minimize unnecessary transfers.

---

# 4. Role Hierarchy

Role Hierarchy provides **automatic record access upward**.

Users higher in the hierarchy automatically receive access to records owned by users below them.

Example

```
CEO

↓

VP Sales

↓

Sales Manager

↓

Sales Rep
```

Sales Rep owns an Opportunity.

Automatically accessible by

- Sales Manager
- VP Sales
- CEO

---

# Important Rule

Role Hierarchy

```
Grants access upward

NOT downward
```

Managers see subordinate records.

Employees do **not** see manager records automatically.

---

# Example

```
CEO

↓

Manager

↓

John
```

John owns

```
Account A
```

Access

| User | Access |
|------|---------|
| John | ✔ |
| Manager | ✔ |
| CEO | ✔ |

John cannot automatically see CEO records.

---

# How Access Rolls Upward

```
CEO

↓

Director

↓

Manager

↓

Sales Rep

↓

Opportunity
```

Opportunity owned by Sales Rep.

Automatically visible to

- Manager
- Director
- CEO

---

# Role Hierarchy Requirements

Role hierarchy only provides access when

- OWD is Private
- OWD is Public Read Only

If OWD is Public Read/Write,

everyone already has access.

Role hierarchy becomes less relevant.

---

# Grant Access Using Hierarchies

Standard Objects

Always enabled.

Cannot be disabled.

Custom Objects

Can be disabled.

Example

```
☑ Grant Access Using Hierarchies
```

or

```
☒ Grant Access Using Hierarchies
```

When disabled,

Managers no longer inherit access automatically.

---

# Example

Custom Object

```
Payroll__c
```

Grant Access Using Hierarchies

Disabled

Manager

Cannot automatically access employee payroll.

---

# 5. CEO Role

Every role hierarchy begins with a top role.

Typically

```
CEO

↓

VP

↓

Director

↓

Manager

↓

Employee
```

CEO receives access to all subordinate records.

Important

CEO role

≠

System Administrator

Role hierarchy only grants record visibility.

It does not grant permissions.

---

# CEO Role Example

CEO Profile

Read Only on Opportunities.

Even though CEO sees every Opportunity,

they still cannot edit unless object permissions allow it.

---

# 6. Internal Roles

Internal roles represent employees.

Example

```
CEO

↓

VP Sales

↓

Regional Manager

↓

Sales Representative
```

Typical Characteristics

- Used for employees
- Supports managerial reporting
- Grants upward record access

---

# Internal Role Best Practices

- Mirror reporting structure only where useful.
- Avoid unnecessary role levels.
- Keep hierarchy simple.

---

# 7. External Roles

Experience Cloud users have a separate role hierarchy.

External users never share the same role tree as employees.

Example

```
Internal Roles

CEO

↓

Sales Manager

↓

Sales Rep

----------------

External Roles

Partner Executive

↓

Partner Manager

↓

Partner User
```

---

# 8. Partner Roles

Partner users automatically receive partner roles.

Typical hierarchy

```
Partner Executive

↓

Partner Manager

↓

Partner User
```

Partner hierarchy allows

Managers within partner organizations

↓

See subordinate partner records.

Partner users generally cannot access unrelated partner accounts.

---

# Example

Partner Company A

```
Executive

↓

Manager

↓

User
```

Manager automatically sees

User-owned Opportunities.

---

# 9. Customer Roles

Customer users usually have

Much simpler role hierarchy.

Example

```
Customer Manager

↓

Customer User
```

Customer Manager may view subordinate customer records,

depending on Experience Cloud configuration.

---

# Internal vs External Roles

| Internal | External |
|-----------|----------|
| Employees | Experience Cloud Users |
| Enterprise reporting | Partner/Customer reporting |
| Larger hierarchy | Smaller hierarchy |
| Organization-wide | Account-specific |

---

# Role Hierarchy Limitations

Role Hierarchy

Does NOT

- Grant object permissions
- Grant field permissions
- Override CRUD
- Override FLS
- Restrict access

It only **adds record visibility**.

---

# Common Misconception

Role Hierarchy

❌ Gives edit permission

Incorrect.

Edit still requires

- Edit object permission
- Record access

---

# Role Design Strategies

Architects should design roles based on

Business reporting structure,

not necessarily departments.

---

# Recommended Design

```
CEO

↓

Sales VP

↓

Sales Manager

↓

Sales Rep
```

Avoid

```
CEO

↓

Country

↓

Region

↓

State

↓

City

↓

District

↓

Area

↓

Branch

↓

Employee
```

Too many levels reduce performance and increase maintenance.

---

# Keep Role Count Low

Salesforce recommends

Only create roles when they provide meaningful record access.

Not every employee requires a unique role.

---

# Flat vs Deep Hierarchy

Flat

```
CEO

↓

Manager

↓

Employee
```

Advantages

- Faster calculations
- Easier maintenance

---

Deep

```
CEO

↓

VP

↓

Director

↓

Senior Manager

↓

Manager

↓

Lead

↓

Employee
```

Disadvantages

- More sharing calculations
- Slower recalculation
- Harder administration

---

# Role Optimization

## Keep Hierarchy Shallow

Recommended

5–10 levels or fewer where possible.

---

## Minimize Total Roles

Thousands of unnecessary roles increase

- Sharing calculations
- Maintenance
- Complexity

---

## Align with Reporting Structure

Roles should represent

Who manages whom,

not every organizational attribute.

---

## Use Public Groups for Sharing

Don't create roles simply for sharing.

Instead

Use

- Public Groups
- Sharing Rules

---

## Separate Reporting from Security

Departments

```
Finance

Sales

Marketing
```

Do not always require separate role branches.

Use Permission Sets for permissions.

Use Roles for visibility.

---

# Performance Considerations

Role Hierarchy affects

- Sharing calculations
- Ownership changes
- Role moves
- User moves
- OWD recalculations

Large role hierarchies increase

- Share table size
- Recalculation time

---

# Example Interview Scenario

Requirement

```
Sales Representatives

See only own Opportunities.

Managers

See team Opportunities.

CEO

See everything.
```

Solution

```
OWD

Private

↓

Role Hierarchy

CEO

↓

Sales Manager

↓

Sales Rep
```

No sharing rules required.

---

# Another Scenario

Requirement

```
HR records

Should NOT automatically roll up.

```

Solution

Custom Object

```
Grant Access Using Hierarchies

Disabled
```

---

# Ownership vs Role Hierarchy

| Ownership | Role Hierarchy |
|------------|----------------|
| Assigned to one owner | Based on reporting structure |
| Determines primary access | Adds upward access |
| Every record has one owner | Users may or may not have a role |
| Supports sharing | Supports sharing calculations |

---

# Role Hierarchy vs Sharing Rules

| Role Hierarchy | Sharing Rules |
|----------------|---------------|
| Vertical sharing | Horizontal sharing |
| Manager → Employee | Peer → Peer / Group |
| Automatic | Rule-based |
| Based on roles | Based on ownership or criteria |

---

# Exam Tips ⭐⭐⭐⭐⭐

- Every record has exactly **one owner** (user or queue for supported objects).
- **Role Hierarchy grants record access upward only**, never downward.
- Role Hierarchy **does not grant CRUD or FLS**.
- The **CEO role is not equivalent to System Administrator**.
- **Grant Access Using Hierarchies** is mandatory for standard objects but optional for custom objects.
- Design role hierarchies to match the **business reporting structure**, not every department or geography.
- Keep the hierarchy **as shallow and simple as possible** to improve sharing performance.
- Use **Public Groups and Sharing Rules** instead of creating unnecessary roles solely for sharing purposes.
- Internal and Experience Cloud users have **separate role hierarchies**.
- Large, deep role hierarchies increase **sharing recalculation time** and can impact performance in Large Data Volume (LDV) environments.