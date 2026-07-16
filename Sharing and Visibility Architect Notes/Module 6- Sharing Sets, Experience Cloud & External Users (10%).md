
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **Core Exam Principle**
>
> External users (Experience Cloud users) follow a **different sharing model** than internal users.
>
> Architects must choose the correct sharing mechanism based on the **license type** and **business requirement**.

---

# 1. Experience Cloud Overview

Experience Cloud (formerly Community Cloud) allows external users to securely access Salesforce data.

Typical users include

- Customers
- Partners
- Dealers
- Suppliers
- Franchisees
- Vendors

Examples

- Customer Support Portal
- Partner Sales Portal
- Supplier Portal
- Dealer Portal

---

# Internal vs External Users

```
Internal Users

↓

Employees

Sales

Support

Admins

----------------------------

External Users

↓

Customers

Partners

Guests
```

External users generally require **more restrictive** access.

---

# Experience Cloud Security Model

```
Authentication

↓

Profile / Permission Set

↓

External OWD

↓

Sharing Sets

↓

Share Groups

↓

Role Hierarchy (if supported)

↓

Sharing Rules

↓

Apex Sharing
```

---

# 2. Types of External Users

Salesforce supports three major categories.

---

# Customer Users

Typical examples

- Customers
- Consumers
- End Users

Characteristics

- Limited access
- Usually access only their own records
- Lowest collaboration needs

Examples

- View Cases
- Update Support Tickets
- Download Knowledge Articles

---

# Partner Users

Partner users represent external organizations.

Examples

- Resellers
- Dealers
- Distributors
- Business Partners

Characteristics

- Collaborate on Opportunities
- Manage Leads
- View Accounts
- Work with Cases

Partner users receive

- More permissions
- Partner Roles
- Role hierarchy support

---

# Guest Users

Guest users

- Anonymous
- No login required

Examples

- Public website
- Public form
- Event registration

Characteristics

- Extremely restricted
- Read-only in most scenarios
- Explicit sharing required

---

# External User Comparison

| User Type | Login | Role Hierarchy | Typical Access |
|------------|-------|----------------|----------------|
| Guest User | No | No | Public records only |
| Customer User | Yes | Depends on license | Own records |
| Partner User | Yes | Yes | Collaborative access |

---

# 3. Sharing Sets

Sharing Sets are unique to Experience Cloud.

They grant access based on **matching fields** rather than ownership.

Example

```
Contact.AccountId

=

Case.AccountId
```

If the values match,

the customer can access the Case.

---

# How Sharing Sets Work

```
Logged-in Customer

↓

Contact

↓

Account

↓

Matching AccountId

↓

Access Granted
```

No sharing rules required.

---

# Example

Customer

```
ABC Company
```

Account

```
AccountId = A100
```

Case

```
AccountId = A100
```

Sharing Set

```
AccountId

=

AccountId
```

Result

Customer automatically sees the Case.

---

# Common Sharing Set Relationships

Examples

```
User.Contact.AccountId

↓

Case.AccountId

--------------------

User.Contact.AccountId

↓

Asset.AccountId

--------------------

User.Contact.AccountId

↓

Custom_Object__c.Account__c
```

---

# When to Use Sharing Sets

Ideal for

- Customer portals
- Self-service portals
- High-volume users
- Account-based access

---

# Advantages

- No Apex required
- Easy to configure
- High performance
- Scalable
- Designed for external users

---

# Limitations

- Only available for certain Experience Cloud licenses.
- Uses matching fields only.
- Cannot implement complex business logic.
- Cannot share arbitrary records.

---

# 4. Share Groups

Share Groups allow records owned by

High-Volume Users

to be shared with

Internal users.

---

# Why Share Groups Exist

High-volume users

Do not participate in

- Role Hierarchy
- Standard Sharing

Therefore,

Salesforce provides Share Groups.

---

# Example

```
Customer Portal User

↓

Creates Case

↓

Share Group

↓

Support Team
```

Support agents can access the Case.

---

# When to Use Share Groups

- Customer-created Cases
- Customer-created custom object records
- External collaboration with internal employees

---

# 5. Super User Access

Partner Super Users receive additional visibility.

They can view

Records owned by users

within their partner account.

---

# Example

Partner Company

```
Executive

↓

Manager

↓

User
```

Super User

Can view records owned by

- Executive
- Manager
- User

Without additional sharing rules.

---

# Benefits

- Simplifies partner collaboration
- Reduces sharing rule complexity
- Better user experience

---

# 6. External OWD

Salesforce supports

Two separate OWD models.

```
Internal OWD

↓

Employees

----------------

External OWD

↓

Experience Cloud Users
```

---

# Why Separate Models?

Employees usually need broader collaboration.

Customers should see

Only their own records.

---

# Example

```
Internal

Public Read Only

↓

Employees collaborate

----------------------

External

Private

↓

Customers only see own Cases
```

---

# Architect Best Practice

External OWD should generally be

More restrictive

Than Internal OWD.

---

# 7. External Role Hierarchy

Not all external users support roles.

---

## Partner Users

Support

Partner Role Hierarchy

Example

```
Partner Executive

↓

Partner Manager

↓

Partner User
```

Managers receive upward access.

---

## Customer Users

Depends on license.

Many customer licenses

Do not include role hierarchy.

---

## Guest Users

No role hierarchy.

---

# External Role Comparison

| User Type | Roles |
|------------|-------|
| Internal | Yes |
| Partner | Yes |
| Customer Community Plus | Yes |
| Customer Community | No |
| Guest | No |

---

# 8. Internal vs External Sharing

| Internal Users | External Users |
|----------------|----------------|
| Employees | Customers / Partners |
| Internal OWD | External OWD |
| Role Hierarchy | Partner Roles (license dependent) |
| Sharing Rules | Sharing Sets |
| Public Groups | Share Groups |

---

# 9. Portal Record Access

Typical customer portal access

```
Customer

↓

Own Account

↓

Own Cases

↓

Own Assets

↓

Own Custom Records
```

Usually implemented using

- Sharing Sets
- External OWD

---

# Partner Portal Access

Partners often need

```
Accounts

↓

Opportunities

↓

Leads

↓

Cases
```

Implemented using

- Partner Roles
- Sharing Rules
- Teams
- Territory Management

---

# 10. High-Volume Users (HVUs)

High-Volume Users are designed for

Large customer communities.

Examples

- Millions of customers
- Public portals
- Consumer websites

---

# Characteristics

High-Volume Users

Do NOT have

- Roles
- Role Hierarchy
- Manual Sharing

They use

- Sharing Sets
- Share Groups

---

# Why?

Role hierarchy for millions of users

Would be too expensive.

Salesforce uses

Sharing Sets

instead.

---

# High-Volume User Comparison

| Feature | High-Volume User |
|----------|------------------|
| Role | No |
| Role Hierarchy | No |
| Sharing Sets | Yes |
| Share Groups | Yes |
| Manual Sharing | No |

---

# 11. Customer Community Plus

Customer Community Plus license provides

More capabilities than standard Customer Community.

---

# Features

Supports

- Roles
- Role Hierarchy
- Sharing Rules
- Reports
- Dashboards
- Advanced Sharing

Suitable for

Business customers

rather than consumers.

---

# Customer Community vs Customer Community Plus

| Feature | Customer Community | Customer Community Plus |
|----------|--------------------|-------------------------|
| Roles | No | Yes |
| Sharing Sets | Yes | Yes |
| Role Hierarchy | No | Yes |
| Sharing Rules | Limited | Yes |
| Reports | Limited | Yes |

---

# Choosing the Right Sharing Mechanism

| Requirement | Best Choice |
|-------------|-------------|
| Customer sees own Cases | Sharing Set |
| Partner manager sees partner team records | Partner Role Hierarchy |
| Public website access | Guest User Sharing Rule |
| Internal users access customer-owned records | Share Group |
| Customer business users require roles | Customer Community Plus |
| Consumer portal with millions of users | High-Volume Users + Sharing Sets |

---

# Architect Best Practices

- Use **External OWD** independently from Internal OWD.
- Keep external access **more restrictive** than internal access.
- Prefer **Sharing Sets** for customer self-service portals.
- Use **Partner Roles** when external collaboration requires managerial visibility.
- Avoid Apex sharing unless declarative sharing cannot meet the requirement.
- Design for **High-Volume Users** by avoiding role-based solutions.
- Use **Customer Community Plus** when business users need role hierarchy and advanced sharing.
- Never expose sensitive data to **Guest Users** unless explicitly required and carefully secured.

---

# Common Exam Scenarios

### Scenario 1

Requirement

```
Customer

Should only see

Own Cases
```

Best Solution

✅ Sharing Set

---

### Scenario 2

Requirement

```
Partner Manager

Needs access

To partner team Opportunities
```

Best Solution

✅ Partner Role Hierarchy

---

### Scenario 3

Requirement

```
Public website

Displays Knowledge Articles
```

Best Solution

✅ Guest User Sharing Rule

---

### Scenario 4

Requirement

```
Millions of portal users

Need access

To their own records
```

Best Solution

✅ High-Volume Users + Sharing Sets

---

### Scenario 5

Requirement

```
Business customers

Need reports

Role hierarchy

Advanced sharing
```

Best Solution

✅ Customer Community Plus License

---

# Exam Tips ⭐⭐⭐⭐⭐

- **Internal OWD** and **External OWD** are separate sharing models.
- **Sharing Sets** grant access based on **matching fields**, not ownership.
- **Share Groups** allow internal users to access records owned by **High-Volume Users**.
- **Partner Users** support role hierarchy; **Guest Users** do not.
- Standard **Customer Community** users generally do **not** have roles, while **Customer Community Plus** users do.
- **High-Volume Users (HVUs)** cannot use Role Hierarchy or Manual Sharing—they rely on **Sharing Sets**.
- **Guest Users** require explicit sharing and should have the minimum possible access.
- For self-service customer portals, **Sharing Sets** are usually the first choice.
- For B2B partner collaboration, use **Partner Roles** and declarative sharing before considering Apex.
- On the exam, always identify the **user license type first**, as it determines which sharing mechanisms are available.
