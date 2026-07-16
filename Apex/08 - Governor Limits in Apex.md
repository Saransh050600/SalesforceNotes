
Governor Limits are runtime limits enforced by Salesforce to ensure that no single transaction consumes excessive shared resources in the multi-tenant environment.

Think of Salesforce as an apartment building:

- Many customers share the same infrastructure.
- Governor Limits prevent one customer from using all resources.
- They ensure platform stability and fair resource allocation.

---

# Why Governor Limits Exist?

Salesforce is a multi-tenant platform.

```text
One Infrastructure
Many Organizations
Shared Resources
```

Without limits:

```text
One Organization
Could Consume All Resources
```

Governor Limits prevent:

- Excessive CPU usage
- Too many database queries
- Memory overconsumption
- Excessive API usage

---

# Types of Governor Limits

| Type | Description |
|--------|--------|
| Per Transaction Limits | Most common Apex limits |
| Static Limits | Platform-wide limits |
| Size-Specific Limits | Heap, code size, payload size |
| Lightning Platform Limits | API and metadata limits |
| Certified Managed Package Limits | Separate limits for managed packages |

---

# Access Governor Limits

Use the `Limits` class.

Example:

```apex
System.debug(
    Limits.getQueries()
);
```

---

## Check SOQL Usage

```apex
System.debug(
    Limits.getQueries()
);

System.debug(
    Limits.getLimitQueries()
);
```

---

## Check DML Usage

```apex
System.debug(
    Limits.getDMLStatements()
);

System.debug(
    Limits.getLimitDMLStatements()
);
```

---

# 1. SOQL Limits

SOQL queries consume database resources.

Salesforce limits how many queries can be executed.

---

# Maximum SOQL Queries

| Context | Limit |
|----------|----------|
| Synchronous Apex | 100 |
| Asynchronous Apex | 200 |

---

## Example

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
];
```

Counts as:

```text
1 SOQL Query
```

---

# Bad Example

```apex
for(Contact con : contacts){

    Account acc =
    [
        SELECT Id
        FROM Account
        WHERE Id = :con.AccountId
    ];
}
```

Error:

```text
Too many SOQL queries: 101
```

---

# Good Example

```apex
Set<Id> accountIds =
new Set<Id>();

for(Contact con : contacts){

    accountIds.add(
        con.AccountId
    );
}

Map<Id, Account> accountMap =
new Map<Id, Account>(
[
    SELECT Id, Name
    FROM Account
    WHERE Id IN :accountIds
]);
```

---

# Records Retrieved Limit

Maximum records returned by SOQL:

```text
50,000 Records
```

---

## Example Error

```text
Too many query rows: 50001
```

---

# QueryLocator Exception

Batch Apex supports:

```text
50 Million Records
```

using:

```apex
Database.getQueryLocator()
```

---

# SOSL Limits

Maximum SOSL queries:

```text
20 SOSL Queries
```

per transaction.

---

# 2. DML Limits

DML operations consume database write resources.

---

# Maximum DML Statements

```text
150 DML Statements
```

per transaction.

---

## Example

```apex
insert acc;
update acc;
delete acc;
```

Total:

```text
3 DML Statements
```

---

# Bad Example

```apex
for(Account acc : accounts){

    update acc;
}
```

Error:

```text
Too many DML statements: 151
```

---

# Good Example

```apex
update accounts;
```

Single DML statement.

---

# Records Processed Limit

Maximum records processed:

```text
10,000 Records
```

per transaction.

---

## Example Error

```text
Too many DML rows: 10001
```

---

# 3. CPU Limits

CPU time measures the amount of processing time consumed by Apex.

Includes:

- Apex execution
- Workflow processing
- Flow processing
- Formula evaluation
- Validation rules

---

# CPU Time Limits

| Context | Limit |
|----------|----------|
| Synchronous | 10 Seconds |
| Asynchronous | 60 Seconds |

---

# Example Error

```text
Apex CPU Time Limit Exceeded
```

---

# Bad Example

Nested loops.

```apex
for(Account acc : accounts){

    for(Contact con : contacts){

        if(acc.Id ==
           con.AccountId){

        }
    }
}
```

---

# Better Example

Use Maps.

```apex
Map<Id, List<Contact>>
contactMap =
new Map<Id, List<Contact>>();
```

Lookup becomes:

```text
O(1)
```

instead of:

```text
O(n²)
```

---

# CPU Usage Monitoring

```apex
System.debug(
    Limits.getCpuTime()
);
```

---

## CPU Limit

```apex
System.debug(
    Limits.getLimitCpuTime()
);
```

---

# 4. Heap Limits

Heap memory stores variables, collections, objects, JSON data, and query results during execution.

---

# Heap Size Limits

| Context | Limit |
|----------|----------|
| Synchronous | 6 MB |
| Asynchronous | 12 MB |

---

# Example

```apex
List<Account> accounts =
[
    SELECT Id,
           Name,
           Description
    FROM Account
];
```

Large queries consume heap.

---

# Example Error

```text
Apex Heap Size Too Large
```

---

# Check Heap Usage

```apex
System.debug(
    Limits.getHeapSize()
);
```

---

## Heap Limit

```apex
System.debug(
    Limits.getLimitHeapSize()
);
```

---

# Reduce Heap Usage

---

## Query Only Needed Fields

❌

```apex
SELECT *
```

(Not supported in Salesforce, but illustrates over-fetching.)

---

✅

```apex
SELECT Id, Name
FROM Account
```

---

## Clear Collections

```apex
accounts.clear();
```

---

## Process Data in Chunks

Use:

```text
Batch Apex
```

for large datasets.

---

# 5. Callout Limits

Callouts are requests sent from Salesforce to external systems.

Examples:

- REST APIs
- SOAP APIs
- Third-party integrations

---

# Maximum Callouts

```text
100 Callouts
```

per transaction.

---

## HTTP Callout Example

```apex
HttpRequest req =
new HttpRequest();

req.setEndpoint(
    'https://api.example.com'
);

req.setMethod('GET');

Http http =
new Http();

HttpResponse res =
http.send(req);
```

---

# Example Error

```text
Too many callouts: 101
```

---

# Maximum Timeout

```text
120 Seconds
```

per callout.

---

# Callout Response Size

Maximum response size:

```text
6 MB
```

---

# Callout Restrictions

Cannot perform callout after uncommitted DML.

---

## Incorrect

```apex
insert acc;

http.send(request);
```

Error:

```text
You have uncommitted work pending
```

---

## Correct Approaches

Use:

- Future Method
- Queueable Apex
- Batch Apex

---

# Common Governor Limits

| Limit | Value |
|---------|---------|
| SOQL Queries | 100 |
| SOSL Queries | 20 |
| DML Statements | 150 |
| DML Rows | 10,000 |
| Query Rows | 50,000 |
| CPU Time Sync | 10 Seconds |
| CPU Time Async | 60 Seconds |
| Heap Sync | 6 MB |
| Heap Async | 12 MB |
| Callouts | 100 |
| Future Calls | 50 |
| Queueable Jobs Added | 50 |

---

# Limits Class Methods

---

## SOQL

```apex
Limits.getQueries()
Limits.getLimitQueries()
```

---

## DML

```apex
Limits.getDMLStatements()
Limits.getLimitDMLStatements()
```

---

## CPU

```apex
Limits.getCpuTime()
Limits.getLimitCpuTime()
```

---

## Heap

```apex
Limits.getHeapSize()
Limits.getLimitHeapSize()
```

---

## Callouts

```apex
Limits.getCallouts()
Limits.getLimitCallouts()
```

---

# Best Practices to Avoid Governor Limits

---

## Avoid SOQL in Loops

❌

```apex
for(Account acc : accounts){

    [SELECT Id FROM Contact];
}
```

---

## Avoid DML in Loops

❌

```apex
for(Account acc : accounts){

    update acc;
}
```

---

## Use Collections

```apex
List
Set
Map
```

---

## Query Only Required Fields

```apex
SELECT Id, Name
```

instead of unnecessary fields.

---

## Use Batch Apex for Large Data

```apex
Database.Batchable
```

for millions of records.

---

## Replace Nested Loops with Maps

```apex
Map<Id, Account>
```

to reduce CPU usage.

---

## Use Asynchronous Processing

```apex
@future
Queueable
Batch Apex
Scheduled Apex
```

when heavy processing is required.

---

# Real Trigger Example

❌ Not Bulkified

```apex
for(Contact con : Trigger.new){

    Account acc =
    [
        SELECT Id
        FROM Account
        WHERE Id = :con.AccountId
    ];

    update acc;
}
```

Possible Errors:

```text
Too many SOQL queries: 101
Too many DML statements: 151
```

---

✅ Bulkified

```apex
Set<Id> accountIds =
new Set<Id>();

for(Contact con : Trigger.new){

    accountIds.add(
        con.AccountId
    );
}

Map<Id, Account> accMap =
new Map<Id, Account>(
[
    SELECT Id
    FROM Account
    WHERE Id IN :accountIds
]);

List<Account> accountsToUpdate =
new List<Account>();

for(Account acc :
    accMap.values()){

    acc.Description =
        'Updated';

    accountsToUpdate.add(acc);
}

update accountsToUpdate;
```

---

# Interview Questions

### What are Governor Limits?

Runtime limits enforced by Salesforce to ensure fair resource usage in a multi-tenant environment.

---

### What is the SOQL Query Limit?

```text
100 Synchronous
200 Asynchronous
```

---

### What is the DML Statement Limit?

```text
150 DML Statements
```

---

### What is the Maximum CPU Time?

```text
10 Seconds (Sync)
60 Seconds (Async)
```

---

### What is the Heap Size Limit?

```text
6 MB (Sync)
12 MB (Async)
```

---

### What is the Maximum Number of Callouts?

```text
100 Callouts
```

---

### Why Should SOQL and DML Not Be Inside Loops?

They can exceed governor limits and cause runtime exceptions.

---

### How Can Large Volumes of Data Be Processed?

Using:

```apex
Batch Apex
Queueable Apex
Future Methods
```

---

### Which Governor Limit Causes the Most Production Issues?

Typically:

```text
SOQL Query Limits
CPU Time Limits
DML Limits
```

especially in poorly bulkified triggers.
