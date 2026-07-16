
**Salesforce Sharing & Visibility Architect Certification Notes**

---

# 1. What are Organization-Wide Defaults (OWD)?

Organization-Wide Defaults (OWD) define the **baseline (most restrictive)** level of record access for users in Salesforce.

OWD answers the question:

> **If no sharing exists, who can access this record?**

Think of OWD as the **starting point** of record-level security.

Additional access can then be granted through:

- Role Hierarchy
- Sharing Rules
- Manual Sharing
- Teams
- Enterprise Territory Management
- Apex Managed Sharing

> **Important:** OWD only controls **record-level access**, not object or field permissions.

---

# Security Hierarchy

```
Authentication
        ↓
Authorization
        ↓
Object-Level Security
        ↓
Field-Level Security
        ↓
Organization-Wide Defaults (OWD)
        ↓
Role Hierarchy
        ↓
Sharing Rules
        ↓
Manual/Apex Sharing
```

---

# 2. Why OWD Exists

Imagine an organization with 10,000 Accounts.

Without OWD:

- Every user could see every Account.
- Sensitive customer data would be exposed.

With OWD:

- Salesforce starts with a restricted baseline.
- Additional access is granted only where needed.

**Architect Principle**

> Start restrictive, then open access.

---

# 3. Types of OWD

Salesforce supports different default sharing models depending on the object.

---

# Private

```
Owner
    ✔ Read
    ✔ Edit

Others
    ✖ No Access
```

Only the owner (and privileged users) can access the record unless sharing grants additional access.

### Example

Sales Rep A owns:

```
ABC Corporation
```

Sales Rep B

Cannot see it.

Unless

- Role Hierarchy
- Sharing Rule
- Manual Sharing
- Team Sharing
- Apex Sharing

provides access.

### Best Use Cases

- Banking
- Healthcare
- Government
- HR
- Finance
- Confidential Sales

### Advantages

- Highest security
- Best data isolation
- Supports least privilege

### Disadvantages

- Requires additional sharing
- More sharing calculations

---

# Public Read Only

Everyone can read.

Only owner can edit.

```
Owner
    Read ✔
    Edit ✔

Others
    Read ✔
    Edit ✖
```

### Example

Product Catalog

Everyone should view products.

Only Product Managers modify them.

### Best Use Cases

- Product Catalog
- Knowledge Articles
- Price Lists
- Marketing Assets

---

# Public Read/Write

Everyone can read and edit every record.

```
Everyone

Read ✔

Edit ✔
```

### Example

Small startup

All employees collaborate on Accounts.

### Best Use Cases

- Small organizations
- Internal collaboration
- Shared project tracking

### Risks

- Data can be modified accidentally.
- No ownership protection.

---

# Public Full Access

Available only on certain objects (primarily custom objects).

Includes:

- Read
- Edit
- Delete
- Transfer
- Share

Everyone has complete control.

### Use Cases

Rare.

Mostly collaboration objects.

---

# Controlled by Parent

Child record inherits access from parent.

```
Account

↓

Contact

↓

Case

↓

Asset
```

If user can access parent,

they automatically access child.

### Example

```
Account

ABC Ltd

↓

Contact

John Smith
```

If Account is shared,

Contact is automatically shared.

### Advantages

- Simplifies sharing
- Reduces maintenance
- Better performance

### Common Objects

- Contact
- Opportunity (optional depending on settings)
- Case
- Custom Master-Detail children

---

# Grant Access Using Hierarchies

Determines whether users higher in the Role Hierarchy automatically receive access.

## Standard Objects

Always enabled.

Cannot be disabled.

Example

```
VP Sales

↓

Sales Manager

↓

Sales Rep
```

Sales Manager automatically sees Sales Rep records.

---

## Custom Objects

Can be disabled.

```
☑ Grant Access Using Hierarchies
```

or

```
☐ Grant Access Using Hierarchies
```

### When Disabled

Managers do NOT automatically receive access.

Useful for:

- HR
- Payroll
- Legal
- Compliance

---

# Internal vs External Sharing Model

Introduced for Experience Cloud.

Salesforce maintains **two independent sharing models**.

---

## Internal OWD

Applies to:

- Internal Users

Examples

- Employees
- Admins
- Sales
- Support

---

## External OWD

Applies to:

- Experience Cloud Users
- Partner Users
- Customer Users

External users usually require much more restrictive access.

Example

```
Internal

Public Read Only

External

Private
```

---

# Why Separate Models?

Employees often need collaboration.

Partners or customers should only see their own records.

Example

```
Employees

Can view all Cases.

Customers

Only own Cases.
```

---

# Internal vs External Example

| User | Access |
|--------|---------|
| Internal Sales | Public Read Only |
| Customer Portal | Private |
| Partner | Private |
| Support Employee | Public Read Only |

---

# Choosing the Right OWD

## Private

Choose when

- Sensitive data
- Regulatory requirements
- Large enterprise
- Least privilege

Examples

- Opportunities
- Cases
- Financial Records

---

## Public Read Only

Choose when

Everyone should view data.

Few users edit.

Examples

- Products
- Price Books
- Knowledge

---

## Public Read/Write

Choose when

Everyone collaborates equally.

Examples

- Internal Projects
- Startup CRM
- Shared Activities

---

## Controlled by Parent

Choose when

Child should always inherit parent visibility.

Examples

- Contact
- Opportunity Line Item
- Case Comment
- Custom Detail Objects

---

# OWD Decision Tree

```
Should everyone edit?

        │
      Yes
        │
Public Read/Write

        │
      No
        │
Should everyone read?

        │
      Yes
        │
Public Read Only

        │
      No
        │
Private
```

---

# Performance Implications

One of the most important architect topics.

---

## Public Read/Write

Fastest.

Reason

No sharing calculations required.

Everyone already has access.

---

## Public Read Only

Minimal sharing.

Good performance.

---

## Private

Most expensive.

Requires

- Sharing Rules
- Role Hierarchy
- Teams
- Territories
- Apex Sharing

Each creates share table entries.

---

# OWD Performance Ranking

```
Fastest

Public Read/Write

↓

Public Read Only

↓

Private

Slowest
```

---

# Share Tables

When OWD is restrictive,

Salesforce creates share records.

Examples

```
AccountShare

CaseShare

OpportunityShare

CustomObject__Share
```

More sharing

↓

More share records

↓

Larger calculations

↓

Lower performance

---

# Sharing Recalculation

Whenever OWD changes,

Salesforce recalculates sharing.

Example

```
Public Read Only

↓

Private
```

Salesforce must

- Rebuild share tables
- Update millions of records
- Recalculate visibility

This can take minutes or hours in large orgs.

---

# Events That Trigger Recalculation

- OWD changed
- Role moved
- Sharing Rule modified
- Territory assignment changed
- Owner changed (depending on sharing model)
- Group membership changes

---

# Sharing Calculation Process

```
OWD Changed

↓

Calculate New Access

↓

Generate Share Records

↓

Update Share Tables

↓

Users Receive New Access
```

---

# Large Data Volume (LDV) Considerations

LDV typically means:

- Millions of records
- Hundreds of thousands of users
- Large role hierarchy
- Complex sharing

Sharing becomes one of the biggest performance bottlenecks.

---

# Best Practices for LDV

### Keep OWD as Simple as Possible

Avoid unnecessary sharing.

---

### Minimize Sharing Rules

Too many rules increase recalculation time.

---

### Keep Role Hierarchy Shallow

Avoid very deep hierarchies.

Recommended:

5–10 levels maximum.

---

### Prefer Controlled by Parent

Inheritance reduces share records.

---

### Reduce Ownership Changes

Changing owners often triggers recalculation.

---

### Avoid Unnecessary Apex Sharing

Every Apex share creates additional records.

---

### Use Public Groups

Instead of sharing individually with thousands of users.

---

### Plan Data Model Early

Changing OWD later in production can be extremely expensive.

---

# Common Architect Scenario

Company Requirement

```
Sales

Should only see own Opportunities.

Managers

See team Opportunities.

Executives

See everything.
```

Recommended Design

```
OWD

Private

↓

Role Hierarchy

Managers gain access

↓

Executives

Highest Role
```

---

# Another Scenario

Requirement

Customers should only view their own Cases.

Employees should view every Case.

Solution

```
Internal OWD

Public Read Only

External OWD

Private
```

---

# OWD Summary Table

| OWD | Read | Edit | Typical Use |
|------|------|------|-------------|
| Private | Owner only | Owner only | Sensitive data |
| Public Read Only | Everyone | Owner only | Catalogs, Knowledge |
| Public Read/Write | Everyone | Everyone | Collaboration |
| Public Full Access | Everyone | Everyone + Delete/Transfer | Rare custom object scenarios |
| Controlled by Parent | Inherits parent | Inherits parent | Child objects |

---

# Exam Tips ⭐⭐⭐⭐⭐

- OWD is the **foundation** of record-level security.
- OWD is always the **most restrictive baseline**.
- **Sharing Rules only open access**; they never restrict it.
- **Private** is the recommended starting point for enterprise designs.
- **Public Read/Write** provides the best performance because no sharing calculations are needed.
- **Grant Access Using Hierarchies** cannot be disabled for standard objects but **can** be disabled for custom objects.
- **Controlled by Parent** is ideal for child objects that should always inherit parent visibility.
- **Internal and External OWD** allow separate sharing models for employees and Experience Cloud users.
- Changing OWD in a production org with **Large Data Volumes (LDV)** can trigger expensive sharing recalculations.
- Architects should always consider the impact of OWD choices on **share table size**, **recalculation time**, and **overall system performance**.