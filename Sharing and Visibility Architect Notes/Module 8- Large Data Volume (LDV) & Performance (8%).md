
**Salesforce Sharing & Visibility Architect Certification Notes**

> ⭐ **Core Exam Principle**
>
> Large Data Volume (LDV) architecture is about **maintaining performance, scalability, and security** when an org contains **millions of records**.
>
> Most Sharing & Visibility Architect scenario questions revolve around **sharing calculations**, **data skew**, and **query optimization**.

---

# 1. What is Large Data Volume (LDV)?

There is **no fixed Salesforce definition**, but an org is generally considered to have LDV when it contains:

- Millions of records
- Hundreds of thousands of users
- Large share tables
- Complex sharing models
- Large role hierarchies

Example

```
Accounts

15 Million

↓

Contacts

80 Million

↓

Cases

120 Million
```

At this scale,

design decisions directly impact performance.

---

# Why LDV Matters

As record count increases

↓

Sharing calculations increase

↓

Queries become slower

↓

Maintenance becomes expensive

↓

User experience degrades

Architects must design

For scale

Not just functionality.

---

# Typical LDV Challenges

- Slow record access
- Slow reports
- Long sharing recalculations
- Locking issues
- Ownership contention
- Query timeouts
- Long deployments

---

# 2. Sharing Calculations

Whenever record visibility changes,

Salesforce recalculates sharing.

Examples

- Owner changes
- OWD changes
- Role changes
- Sharing Rule updates
- Territory updates
- Group membership changes

---

# Sharing Calculation Flow

```
Record Change

↓

Evaluate Sharing

↓

Generate Share Records

↓

Update Share Tables

↓

Users Receive Access
```

---

# Why Sharing Calculations Become Slow

Example

```
10 Records

↓

Fast

-----------------

10 Million Records

↓

Millions of Share Records

↓

Slow
```

Every additional sharing mechanism increases

Calculation cost.

---

# 3. Ownership Skew

Ownership Skew occurs when

One user owns

A very large number of records.

Example

```
Integration User

↓

Owns

8 Million Accounts
```

---

# Problems

- Record locking
- Slow ownership transfer
- Slow sharing recalculation
- Long-running transactions

---

# Common Causes

- Data migration
- Integration users
- Automated imports
- Single service account

---

# Best Practices

- Distribute ownership.
- Avoid one integration user owning everything.
- Use multiple record owners.
- Plan ownership during migration.

---

# 4. Account Skew

Account Skew occurs when

One Account

Has

Too many child records.

Example

```
Account

Global Customer

↓

500,000 Contacts

↓

300,000 Cases

↓

1,000,000 Opportunities
```

---

# Problems

- Parent record locking
- Slow updates
- Long transactions
- Sharing delays

---

# Common Causes

- Generic "Master Account"
- Holding all records under one Account

---

# Best Practices

- Split large Accounts.
- Avoid "catch-all" Accounts.
- Partition customer data.
- Archive inactive child records.

---

# 5. Lookup Skew

Lookup Skew occurs when

Many records reference

The same lookup record.

Example

```
Invoice

↓

Region Lookup

↓

North America

Referenced

By

2 Million Records
```

---

# Problems

- Lock contention
- Slow updates
- Reduced concurrency

---

# Best Practices

- Distribute lookup values.
- Avoid one heavily referenced lookup record.
- Redesign data model if necessary.

---

# Data Skew Comparison

| Type | Cause | Impact |
|------|-------|--------|
| Ownership Skew | One owner | Sharing and locking |
| Account Skew | One Account | Parent locking |
| Lookup Skew | One lookup record | Lock contention |

---

# 6. Indexes

Indexes improve query performance.

Salesforce automatically indexes

- Record Id
- Name (many standard objects)
- OwnerId
- CreatedDate
- LastModifiedDate
- RecordTypeId
- External IDs
- Primary Keys
- Foreign Keys

---

# Custom Indexes

Salesforce Support can create

Custom indexes

For selective fields.

Examples

- Custom Text
- Number
- Date
- Lookup
- External ID

---

# Benefits

Indexes

↓

Reduce scanned records

↓

Faster SOQL

↓

Better reports

---

# Indexed Query Example

```sql
SELECT Id
FROM Account
WHERE AccountNumber = '10001'
```

If AccountNumber is indexed,

Query becomes significantly faster.

---

# 7. Selective Queries

One of the most tested LDV topics.

A query is **selective**

When Salesforce can use an index

To retrieve

A small percentage

Of total records.

---

# Good Query

```sql
SELECT Id

FROM Case

WHERE Status = 'New'

AND CreatedDate = THIS_MONTH
```

Using indexed fields

↓

Selective

---

# Poor Query

```sql
SELECT Id

FROM Case

WHERE Description LIKE '%error%'
```

No useful index

↓

Non-selective

↓

Full table scan

---

# Query Optimizer

Salesforce automatically decides

Whether to use an index.

Architects should design

Queries that

Can use indexes.

---

# Query Best Practices

Use

- Indexed fields
- Equality operators
- Date filters
- External IDs

Avoid

- Leading wildcards
- Negative filters (`!=`)
- Large OR conditions
- Unindexed formula fields

---

# 8. Skinny Tables

Skinny Tables are

Salesforce-managed

Physical database tables

Containing only frequently used fields.

---

# Why Skinny Tables Exist

Normal Table

```
Account

400 Fields
```

Application only uses

```
Name

Industry

Owner

Status
```

Salesforce creates

Skinny Table

Containing only

Those frequently accessed fields.

---

# Benefits

- Faster queries
- Better reporting
- Reduced joins
- Improved read performance

---

# Limitations

- Created only by Salesforce Support
- Cannot contain all field types
- Automatically maintained by Salesforce
- Not available in all scenarios

---

# 9. Sharing Recalculation

Every sharing model change

Triggers recalculation.

Examples

- OWD changes
- Role changes
- Territory changes
- Sharing Rule changes
- Group membership updates

---

# Large Recalculation Example

```
10 Million Accounts

↓

OWD

Public Read Only

↓

Private

↓

Entire Sharing Model Rebuilt
```

This can take hours

Depending on org size.

---

# Best Practices

- Avoid unnecessary OWD changes.
- Minimize sharing rules.
- Schedule major changes during maintenance windows.

---

# 10. Deferred Sharing Calculation

Deferred Sharing temporarily postpones

Sharing recalculation.

---

# Normal Process

```
Change

↓

Recalculate

↓

Change

↓

Recalculate

↓

Slow
```

---

# Deferred Process

```
Many Changes

↓

No Recalculation

↓

Final Calculation

↓

Much Faster
```

---

# Best Use Cases

- Data migration
- Territory redesign
- Role restructuring
- Bulk ownership transfer
- Massive deployments

---

# Benefits

- Faster deployment
- Reduced CPU usage
- Lower system load
- One final recalculation

---

# 11. Data Partitioning

Data Partitioning divides

Large datasets

Into logical groups.

Examples

- Region
- Country
- Business Unit
- Fiscal Year
- Product Line

---

# Example

```
Global Accounts

↓

North America

↓

Europe

↓

Asia
```

Benefits

- Smaller queries
- Better reports
- Faster processing

---

# Common Partitioning Methods

- Record Types
- Custom fields
- Territories
- Business Units
- Divisions (where applicable)

---

# 12. Archiving

Old records

Do not always need to remain active.

Example

```
Cases

Older than

7 Years

↓

Archive
```

---

# Benefits

- Smaller active dataset
- Faster reports
- Better queries
- Reduced sharing calculations

---

# Typical Archive Candidates

- Closed Cases
- Completed Work Orders
- Old Opportunities
- Historical Tasks
- Historical Events

---

# Soft Delete vs Archive

| Soft Delete | Archive |
|-------------|----------|
| In Recycle Bin | Stored separately |
| Can be restored | Usually read-only |
| Still affects storage | Reduces active data volume |

---

# LDV Optimization Strategy

```
Private OWD

↓

Simple Role Hierarchy

↓

Few Sharing Rules

↓

Indexed Fields

↓

Selective Queries

↓

Deferred Sharing

↓

Archive Old Data
```

---

# Performance Optimization Checklist

✅ Keep OWD as simple as possible.

✅ Minimize share table growth.

✅ Keep role hierarchy shallow.

✅ Reduce Public Group nesting.

✅ Use selective SOQL queries.

✅ Filter using indexed fields.

✅ Archive historical data.

✅ Enable Deferred Sharing during large changes.

✅ Avoid ownership, account, and lookup skew.

---

# Common Architect Scenarios

### Scenario 1

Requirement

```
Integration User

Owns

5 Million Accounts
```

Problem

Ownership Skew

Best Solution

Distribute ownership across multiple users.

---

### Scenario 2

Requirement

```
Single Account

Contains

700,000 Contacts
```

Problem

Account Skew

Best Solution

Split into multiple Accounts.

---

### Scenario 3

Requirement

```
Weekend Migration

50 Million Records
```

Best Solution

Enable Deferred Sharing Calculation.

---

### Scenario 4

Requirement

```
Slow SOQL

Searching

20 Million Cases
```

Best Solution

Use indexed fields and rewrite as a selective query.

---

### Scenario 5

Requirement

```
Historical Cases

10 Years Old

Rarely Accessed
```

Best Solution

Archive inactive records.

---

# Architect Best Practices

- Design for **Large Data Volumes (LDV)** from the beginning.
- Use **Private OWD** with minimal sharing complexity.
- Avoid **Ownership Skew**, **Account Skew**, and **Lookup Skew**.
- Write **selective SOQL queries** that leverage indexed fields.
- Use **Custom Indexes** only when standard indexes are insufficient and after validating query selectivity.
- Consider **Skinny Tables** for frequently queried fields in very large orgs.
- Enable **Deferred Sharing Calculation** before major administrative operations.
- Archive or purge old data to reduce active record counts.
- Monitor query performance with the **Query Plan Tool** and optimize before production deployments.

---

# Exam Tips ⭐⭐⭐⭐⭐

- **Large Data Volume (LDV)** is about scalability, not just record count.
- **Ownership Skew** = one user owns too many records.
- **Account Skew** = one Account has too many child records.
- **Lookup Skew** = many records reference the same lookup record.
- **Selective queries** use indexes and return a small subset of records; they are critical for LDV performance.
- **Skinny Tables** are created by Salesforce Support to improve read performance on frequently accessed fields.
- **Deferred Sharing Calculation** should be used during large migrations, role changes, territory changes, or ownership updates.
- **Archiving** reduces the active dataset and improves reporting, querying, and sharing performance.
- Avoid unnecessary sharing rules and deep role hierarchies in LDV environments.
- In architect scenarios, always optimize **sharing calculations**, **query selectivity**, and **data distribution** before adding custom solutions.