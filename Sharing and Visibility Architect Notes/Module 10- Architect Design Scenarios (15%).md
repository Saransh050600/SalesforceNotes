
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **This is the most important module for the Sharing & Visibility Architect exam.**
>
> Nearly every question presents a business scenario and asks:
>
> **"What is the BEST architecture?"**
>
> The exam rarely tests definitions—it tests **design decisions and trade-offs.**

---

# 1. Architect Mindset

Before selecting any solution, ask these questions in order.

```
1. Who owns the data?

↓

2. Who should access it?

↓

3. Is access permanent or temporary?

↓

4. Is access based on management?

↓

5. Is access based on geography?

↓

6. Is access based on business rules?

↓

7. Can declarative sharing solve it?

↓

8. Only then consider Apex.
```

---

# Salesforce Sharing Decision Framework

```
Object Security?

↓

Profiles / Permission Sets

------------------------

Record Security?

↓

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

---

# Golden Architect Principles

✅ Private by Default

✅ Least Privilege

✅ Declarative First

✅ Apex Last

✅ Simplicity Wins

---

# 2. Global Company

### Scenario

```
Company

Operations

Worldwide

Sales teams

Need local visibility

Executives

Need global visibility
```

---

## Recommended Architecture

```
OWD

Private

↓

Role Hierarchy

↓

Regional Managers

↓

Global Executives
```

---

## Why?

- Strong security
- Easy maintenance
- Supports enterprise reporting

---

# 3. Multiple Countries

### Scenario

```
USA

India

Germany

Japan

Need country isolation
```

---

## Best Solution

```
OWD

Private

↓

Territory Management

or

Criteria Sharing Rules
```

---

## Why?

Countries are

Business territories

Not management hierarchy.

---

# 4. Multiple Business Units

Example

```
Healthcare

Manufacturing

Automotive

Finance
```

Business units

Should not access

Each other's data.

---

## Recommended Design

```
OWD

Private

↓

Criteria-Based Sharing

↓

Public Groups
```

If sales coverage differs by business unit,

consider **Enterprise Territory Management (ETM)**.

---

# 5. Mergers & Acquisitions

Scenario

```
Company A

+

Company B

Need separation

Initially

Later collaboration
```

---

## Best Solution

```
OWD

Private

↓

Separate Roles

↓

Sharing Rules

As integration progresses
```

Avoid giving broad access too early.

---

# 6. Separate Visibility

Requirement

```
HR

Finance

Legal

Must not

See each other's records
```

---

## Best Solution

```
Private OWD

↓

No Sharing Rules

↓

Separate Roles

↓

Permission Sets
```

For custom objects,

consider disabling **Grant Access Using Hierarchies** if managers should not inherit access.

---

# 7. Temporary Sharing

Scenario

```
Employee

Needs access

For one week
```

---

## Best Solutions

Depending on scale:

- Manual Sharing (single record)
- Temporary Team membership
- Temporary Permission Set (for object access)
- Apex Managed Sharing with expiration logic (large-scale automation)

---

# 8. Confidential Data

Examples

- Executive compensation
- Payroll
- Legal investigations
- Compliance

---

## Recommended Architecture

```
Private OWD

↓

Minimal Sharing

↓

Restricted Profiles

↓

Permission Sets

↓

Custom Object

Grant Access Using Hierarchies

Disabled (if needed)
```

---

# 9. HR Organization

Requirements

```
Payroll

Confidential

Managers

Should NOT

Automatically inherit
```

---

## Solution

```
Private OWD

↓

Custom Object

↓

Grant Access Using Hierarchies

Disabled

↓

Criteria Sharing

if required
```

---

# 10. Finance Organization

Requirements

```
Invoices

Read

Many users

Edit

Finance only
```

---

## Solution

```
OWD

Public Read Only

↓

Finance

Edit

Via Profile / Permission Set
```

If invoices are sensitive,

use **Private OWD** with sharing rules instead.

---

# 11. Legal Organization

Requirements

```
Legal Cases

Highly confidential
```

---

## Solution

```
Private OWD

↓

No Role Sharing (custom objects if appropriate)

↓

Minimal Sharing Rules

↓

Permission Sets
```

---

# 12. Sales Organization

Classic Salesforce scenario.

Requirements

```
Sales Rep

Own Opportunities

↓

Manager

Team Opportunities

↓

VP

Regional Opportunities

↓

CEO

Everything
```

---

## Solution

```
OWD

Private

↓

Role Hierarchy
```

Sharing Rules usually unnecessary.

---

# 13. Territory Model

Requirements

```
Sales

Organized

By geography
```

Example

```
North America

Europe

Asia
```

---

## Best Solution

```
Enterprise Territory Management
```

Reason

Sales coverage

≠

Management hierarchy.

---

# ETM vs Role Hierarchy

| Requirement | Best Solution |
|-------------|---------------|
| Manager visibility | Role Hierarchy |
| Geographic sales | ETM |
| Product sales | ETM |
| Reporting chain | Role Hierarchy |

---

# 14. Matrix Organization

Scenario

```
Employee

Reports to

Manager A

Works with

Manager B
```

---

## Solution

```
Role Hierarchy

+

Public Groups

+

Sharing Rules

or

Territories
```

One user can only have **one role**, so Role Hierarchy alone is insufficient.

---

# 15. Service Organization

Requirements

```
Support Team

Shared Cases

Queue-based work
```

---

## Solution

```
Queues

↓

Case Teams

↓

Escalation Rules
```

---

# 16. Queues

Use Queues

When records

Need shared ownership.

Example

```
Support Queue

↓

Cases

↓

Agent claims ownership
```

Supported objects include

- Cases
- Leads
- Some custom objects

---

# 17. Escalation

Requirement

```
Case

Not resolved

Within 24 Hours
```

---

## Solution

```
Escalation Rules

↓

Manager

↓

Case Team
```

---

# 18. Teams

Use Teams

For collaboration

Without changing ownership.

---

## Account Teams

```
Account

↓

Sales

↓

Support

↓

Consultant
```

---

## Opportunity Teams

```
Sales Rep

↓

Sales Engineer

↓

Partner
```

---

## Case Teams

```
Support

↓

Escalation

↓

Technical Expert
```

---

# When to Use Teams

| Requirement | Team Type |
|-------------|-----------|
| Account collaboration | Account Team |
| Sales collaboration | Opportunity Team |
| Service collaboration | Case Team |

---

# 19. Partner Collaboration

Scenario

```
Partners

Need

Opportunity access
```

---

## Solution

```
Partner Roles

↓

Sharing Rules

↓

Opportunity Teams
```

---

# 20. External Sharing

Requirement

```
Customers

Only own Cases
```

---

## Solution

```
External OWD

Private

↓

Sharing Sets
```

---

# 21. Experience Cloud

Decision Matrix

| Requirement | Best Solution |
|-------------|---------------|
| Customer self-service | Sharing Sets |
| Partner collaboration | Partner Roles |
| Public website | Guest User Sharing Rules |
| Business customers needing reports and roles | Customer Community Plus |

---

# 22. Large Enterprise

Typical Scenario

```
100 Million Records

100,000 Users
```

---

## Architect Priorities

- Performance
- Scalability
- Sharing calculations
- Maintainability

---

## Recommended Design

```
Private OWD

↓

Simple Role Hierarchy

↓

Few Sharing Rules

↓

Public Groups

↓

Deferred Sharing

↓

Selective Queries
```

---

# 23. Performance Considerations

Always evaluate

### Sharing Complexity

Too many

- Roles
- Sharing Rules
- Groups
- Territories
- Apex Shares

↓

Slow calculations.

---

### Data Skew

Avoid

- Ownership Skew
- Account Skew
- Lookup Skew

---

### Query Optimization

Use

- Indexed fields
- Selective SOQL
- Query Plan Tool

---

### Sharing Recalculation

Large changes

↓

Enable

Deferred Sharing Calculation.

---

# Security Trade-Offs

| Choice | Benefit | Trade-Off |
|---------|----------|-----------|
| Private OWD | Maximum security | More sharing complexity |
| Public Read Only | Easy collaboration | Reduced confidentiality |
| Public Read/Write | Fastest performance | Lowest security |
| Apex Sharing | Maximum flexibility | Highest maintenance |
| Territory Management | Flexible sales coverage | Additional complexity |
| Teams | Collaboration | Manual administration |

---

# Architect Decision Tree

```
Need Object Permission?

↓

Profiles / Permission Sets

----------------------------

Need Record Access?

↓

Private OWD

↓

Manager?

↓

Role Hierarchy

↓

Department?

↓

Owner Sharing Rule

↓

Field Value?

↓

Criteria Sharing Rule

↓

Collaboration?

↓

Teams

↓

Geography?

↓

Territory Management

↓

Complex Logic?

↓

Apex Sharing
```

---

# Common Exam Scenarios

### Scenario 1

```
Managers

Need subordinate records
```

✅ Role Hierarchy

---

### Scenario 2

```
Sales by Geography
```

✅ Enterprise Territory Management

---

### Scenario 3

```
Finance

Read-only

Across company
```

✅ Public Read Only OWD (if appropriate) or Private OWD + Sharing Rule, depending on confidentiality

---

### Scenario 4

```
Partner

Needs Opportunities
```

✅ Partner Roles + Sharing Rules

---

### Scenario 5

```
Customer

Own Cases only
```

✅ Sharing Sets

---

### Scenario 6

```
Temporary

Single record access
```

✅ Manual Sharing

---

### Scenario 7

```
Complex dynamic sharing
```

✅ Apex Managed Sharing

---

### Scenario 8

```
Support Cases

Shared ownership
```

✅ Queue

---

### Scenario 9

```
Collaborative sales
```

✅ Opportunity Teams

---

### Scenario 10

```
100 Million Records
```

✅ Private OWD + Simple Role Hierarchy + Minimal Sharing + Deferred Sharing

---

# Architect Best Practices

- Start with **Private OWD** and open access only when required.
- Prefer **Role Hierarchy** for manager-subordinate visibility.
- Use **Sharing Rules** for peer or cross-functional access.
- Use **Enterprise Territory Management** when access is based on geography, products, or markets.
- Use **Teams** to support collaboration without changing ownership.
- Prefer declarative sharing over Apex; implement **Apex Managed Sharing** only for requirements that cannot be met declaratively.
- Keep role hierarchies shallow and sharing rules to a minimum in LDV environments.
- Consider **External OWD**, **Sharing Sets**, and partner roles separately when designing Experience Cloud solutions.
- Evaluate both **security** and **performance** impacts before choosing a sharing model.

---

# Architect Design Cheat Sheet

| Business Requirement | Best Solution |
|----------------------|---------------|
| Managers see team records | Role Hierarchy |
| Cross-department sharing | Owner-Based Sharing Rule |
| Share based on record values | Criteria-Based Sharing Rule |
| Temporary single-record access | Manual Sharing |
| Sales collaboration | Opportunity Team |
| Account collaboration | Account Team |
| Service collaboration | Case Team |
| Geographic sales model | Enterprise Territory Management |
| Customer portal access | Sharing Sets |
| Partner collaboration | Partner Roles + Sharing Rules |
| Dynamic complex sharing | Apex Managed Sharing |
| High-volume customer portal | Sharing Sets + External OWD |
| Large enterprise (100M+ records) | Private OWD + Simple Hierarchy + Minimal Sharing + LDV optimization |

---

# Final Exam Strategy ⭐⭐⭐⭐⭐

When answering architect scenario questions, follow this sequence:

1. **Determine object access** → Profiles & Permission Sets
2. **Choose the most restrictive OWD** that meets the requirement
3. **Use Role Hierarchy** for management-based visibility
4. **Use Sharing Rules** for peer or cross-functional access
5. **Use Teams** for collaboration
6. **Use Enterprise Territory Management** for geography/product-based access
7. **Use Sharing Sets** for Experience Cloud customer access
8. **Choose Apex Managed Sharing only when declarative options cannot satisfy the requirement**
9. **Evaluate performance** (LDV, sharing recalculation, skew, indexes)
10. **Select the simplest, most scalable solution** that satisfies both security and business requirements.

> **Architect Exam Golden Rule:**  
> **Choose the simplest declarative solution that satisfies the business requirement while maintaining security, scalability, and performance.**