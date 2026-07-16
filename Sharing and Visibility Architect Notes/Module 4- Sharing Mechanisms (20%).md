
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **This is the largest and most important section of the Sharing & Visibility Architect exam.**
>
> Remember the golden rule:
>
> **OWD defines the baseline. Sharing mechanisms only OPEN access—they never restrict access.**

---

# Sharing Architecture

```
OWD (Baseline)

        ↓

Role Hierarchy

        ↓

Sharing Rules

        ↓

Manual Sharing

        ↓

Team Sharing

        ↓

Enterprise Territory Management

        ↓

Apex Managed Sharing
```

---

# 1. Sharing Rules

Sharing Rules automatically grant record access to users who do not own the record.

They are used when:

- Users in different branches need access
- Access isn't provided by Role Hierarchy
- Sharing must happen automatically

Sharing Rules **only grant additional access**.

They **cannot**

- Restrict access
- Override CRUD
- Override Field-Level Security

---

## Types of Sharing Rules

Salesforce supports:

- Owner-Based Sharing Rules
- Criteria-Based Sharing Rules
- Guest User Sharing Rules

---

# 2. Owner-Based Sharing Rules

Owner-Based Sharing shares records based on **who owns them**.

Example

```
Owner

Sales Team East

↓

Share With

Support Team
```

All records owned by East Sales become visible to Support.

---

## Example

```
OWD

Private

↓

Accounts owned by

North Sales

↓

Shared Read Only

South Sales
```

Result

South Sales can now view North Sales Accounts.

---

## When to Use

Choose Owner-Based Sharing when

- Access depends on the owner
- Departments collaborate
- Regional teams share records

Examples

- Sales → Support
- Sales → Finance
- Region A → Region B

---

# 3. Criteria-Based Sharing Rules

Shares records based on **field values**, regardless of owner.

Example

```
Opportunity

Stage = Closed Won

↓

Share Read Only

Finance Team
```

Ownership doesn't matter.

Only the record values matter.

---

## Example

```
Case

Priority = High

↓

Share

Executive Support
```

All High Priority Cases become visible.

---

## When to Use

Choose Criteria-Based Sharing when

Access depends on

- Status
- Country
- Business Unit
- Amount
- Type
- Custom fields

---

# Owner-Based vs Criteria-Based

| Owner-Based | Criteria-Based |
|--------------|----------------|
| Based on owner | Based on field values |
| Ownership changes may affect sharing | Field updates may affect sharing |
| Good for departments | Good for business rules |

---

# 4. Guest User Sharing Rules

Used for **Experience Cloud Guest Users**.

Guest users have extremely limited access.

Guest User Sharing Rules explicitly grant access to records.

Example

```
Public Website

↓

Guest User

↓

Knowledge Article
```

---

## Best Practices

- Share only required records.
- Never expose sensitive data.
- Prefer read-only access.

---

# 5. Manual Sharing

Manual Sharing allows record owners (or authorized users) to share an individual record.

Example

```
Opportunity

Owner

John

↓

Share

Sarah

Read Only
```

Only that record is shared.

---

## Characteristics

- Record-by-record
- Manual process
- Owner controlled
- Not scalable

---

## When to Use

Suitable for

- One-time collaboration
- Temporary access
- Exceptional business cases

Avoid for enterprise-wide sharing.

---

# Manual Sharing Limitations

- Cannot share thousands of records efficiently.
- Ownership transfer may remove manual sharing.
- Difficult to maintain.
- Not suitable for automation.

---

# 6. Standard Object Sharing

Examples of standard share tables:

| Object | Share Table |
|---------|-------------|
| Account | AccountShare |
| Opportunity | OpportunityShare |
| Case | CaseShare |
| Lead | LeadShare |
| Contact | ContactShare |

Salesforce automatically manages these tables.

---

# 7. Custom Object Sharing

Every custom object has a corresponding Share object.

Example

```
Invoice__c

↓

Invoice__Share
```

Contains

- ParentId
- UserOrGroupId
- AccessLevel
- RowCause

---

# 8. Share Objects

Share objects physically store sharing information.

Example

```
AccountShare

AccountId

↓

005 User

↓

Read

↓

Manual
```

---

## Common Fields

| Field | Purpose |
|--------|----------|
| ParentId | Record being shared |
| UserOrGroupId | User or Group receiving access |
| AccessLevel | Read or Edit |
| RowCause | Why the share exists |

---

# 9. RowCause

`RowCause` identifies **why** a share record exists.

Examples

```
Owner

Manual

Rule

Territory

Team

ImplicitParent

ImplicitChild

Apex Sharing Reason
```

---

## Common RowCause Values

| RowCause | Created By |
|-----------|------------|
| Owner | Record ownership |
| Manual | Manual sharing |
| Rule | Sharing Rule |
| Territory | Territory assignment |
| Team | Team sharing |
| ImplicitParent | Parent access |
| ImplicitChild | Child inheritance |
| Custom Apex Sharing Reason | Apex Managed Sharing |

---

# 10. Apex Managed Sharing

Apex Managed Sharing allows developers to create sharing programmatically.

Used when standard sharing cannot satisfy business requirements.

---

## Typical Flow

```
OWD

Private

↓

Business Logic

↓

Apex

↓

Insert Share Record

↓

Additional Access
```

---

# Example

```apex
AccountShare share = new AccountShare();

share.AccountId = accountId;
share.UserOrGroupId = userId;
share.AccountAccessLevel = 'Edit';
share.RowCause = Schema.AccountShare.RowCause.Manual;

insert share;
```

---

# When to Use Apex Managed Sharing

Choose Apex Sharing when

- Sharing logic is too complex
- Multiple conditions exist
- Dynamic business rules
- Integration-driven access
- Custom algorithms

---

# Apex Sharing Reasons

Custom objects support **Apex Sharing Reasons**.

Example

```
Invoice__Share

RowCause

Project_Manager__c
```

Benefits

- Easier maintenance
- Prevent accidental deletion
- Better reporting

---

# Insert/Delete Share Records

Developers can

Insert

```apex
insert Invoice__Share;
```

Delete

```apex
delete Invoice__Share;
```

Always bulkify sharing operations.

---

# Apex Sharing Best Practices

- Bulkify all sharing logic.
- Avoid DML inside loops.
- Use custom Apex Sharing Reasons.
- Remove obsolete shares.
- Handle ownership changes.
- Minimize unnecessary share records.

---

# 11. Sharing Recalculation

Salesforce recalculates sharing whenever visibility changes.

Common triggers

- OWD changes
- Owner changes
- Role changes
- Sharing Rule changes
- Territory changes
- Group membership changes

---

# Recalculation Flow

```
Change

↓

Evaluate

↓

Generate Share Records

↓

Update Share Tables

↓

Users Receive Access
```

---

# Sharing Maintenance

Architect responsibilities

- Remove obsolete shares.
- Minimize duplicate sharing.
- Monitor share table growth.
- Review ownership regularly.
- Simplify sharing model.

---

# 12. Team Sharing

Team Sharing grants access through predefined teams.

Supports collaboration without changing ownership.

---

# Account Teams

Allows multiple users to collaborate on an Account.

Example

```
Account

ABC Ltd

↓

Owner

John

↓

Account Team

Sarah

Mike

David
```

Each member receives configurable access.

---

## Account Team Access

Can control

- Account
- Opportunity
- Contact
- Case

Access levels are configurable.

---

# Opportunity Teams

Multiple salespeople collaborate on one Opportunity.

Example

```
Opportunity

↓

Sales Rep

↓

Sales Engineer

↓

Sales Manager

↓

Partner Rep
```

---

## Benefits

- Collaboration
- Forecasting
- Reporting
- Flexible access

---

# Case Teams

Support users collaborate on Cases.

Example

```
Case

↓

Support Agent

↓

Manager

↓

Technical Expert

↓

Escalation Team
```

---

# Team Sharing Summary

| Team Type | Purpose |
|------------|----------|
| Account Team | Account collaboration |
| Opportunity Team | Sales collaboration |
| Case Team | Service collaboration |

---

# 13. Enterprise Territory Management (ETM)

Enterprise Territory Management provides record access based on **geography, market, product, or business model**.

Unlike Role Hierarchy,

Territories do **not** depend on management structure.

---

# Example

```
CEO

↓

Manager

↓

Sales Rep

Role Hierarchy

--------------------

North America

Europe

Asia

Territories
```

One user can belong to multiple territories.

---

# Territory Hierarchy

Example

```
Global

↓

North America

↓

USA

↓

California
```

Records assigned to California automatically roll up.

---

# Territory Assignment

Records can be assigned

- Manually
- Automatically
- Using Assignment Rules
- Via Apex

---

# When to Use ETM

Ideal when access depends on

- Geography
- Product Line
- Customer Segment
- Industry
- Market

Not on reporting structure.

---

# Role Hierarchy vs Territory Management

| Role Hierarchy | Territory Management |
|----------------|----------------------|
| Based on managers | Based on business territories |
| One role per user | Multiple territories per user |
| Reporting structure | Sales coverage model |

---

# Choosing the Right Sharing Mechanism

| Requirement | Best Choice |
|-------------|-------------|
| Managers see team records | Role Hierarchy |
| Department collaboration | Owner-Based Sharing Rule |
| Share based on field values | Criteria-Based Sharing Rule |
| Temporary one-record access | Manual Sharing |
| Complex business logic | Apex Managed Sharing |
| Sales collaboration | Opportunity Team |
| Account collaboration | Account Team |
| Service collaboration | Case Team |
| Geographic sales model | Enterprise Territory Management |
| Public website guest access | Guest User Sharing Rule |

---

# Scalability

Best to Worst

```
Role Hierarchy

↓

Sharing Rules

↓

Team Sharing

↓

Territory Management

↓

Apex Managed Sharing

↓

Manual Sharing
```

Reason

Programmatic and manual sharing create additional share records that require maintenance.

---

# Maintenance Comparison

| Mechanism | Maintenance |
|------------|-------------|
| Role Hierarchy | Low |
| Sharing Rules | Low |
| Teams | Medium |
| Territory Management | Medium |
| Apex Sharing | High |
| Manual Sharing | Very High |

---

# Architect Best Practices

- Start with **Private OWD** and open access only as needed.
- Prefer **Role Hierarchy** for manager-subordinate access.
- Use **Owner-Based Sharing Rules** for ownership-driven collaboration.
- Use **Criteria-Based Sharing Rules** when access depends on business data.
- Use **Teams** for collaborative work instead of changing ownership.
- Use **Enterprise Territory Management** when access is based on geography or markets rather than reporting lines.
- Reserve **Apex Managed Sharing** for requirements that declarative sharing cannot solve.
- Avoid excessive manual sharing—it does not scale.
- Minimize share table growth to improve Large Data Volume (LDV) performance.
- Review sharing models periodically to remove unnecessary complexity.

---

# Exam Tips ⭐⭐⭐⭐⭐

- **Sharing mechanisms only grant additional record access—they never restrict it.**
- **Owner-Based Sharing Rules** depend on record ownership.
- **Criteria-Based Sharing Rules** depend on field values.
- **Manual Sharing** is intended for individual records and is not scalable.
- **Apex Managed Sharing** creates and removes share records programmatically and is ideal for complex business logic.
- Every share record contains a **RowCause** that identifies why the access exists.
- Use **custom Apex Sharing Reasons** for custom objects whenever implementing Apex Managed Sharing.
- **Role Hierarchy** is based on management structure; **Enterprise Territory Management** is based on business coverage (geography, product, market, etc.).
- **Account Teams**, **Opportunity Teams**, and **Case Teams** enable collaboration without changing record ownership.
- On the exam, always choose the **simplest declarative sharing mechanism** that satisfies the requirement before considering Apex.