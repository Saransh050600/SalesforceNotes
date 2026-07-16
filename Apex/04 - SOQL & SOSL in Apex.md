
Salesforce provides two query languages for retrieving data:

| Query Language | Purpose |
|---------------|----------|
| SOQL (Salesforce Object Query Language) | Retrieve records from a specific object |
| SOSL (Salesforce Object Search Language) | Search text across multiple objects |

---

# SOQL (Salesforce Object Query Language)

SOQL is used to query records from Salesforce objects.

Similar to SQL but designed specifically for Salesforce.

---

# 1. Basic Queries

## Select All Required Fields

```apex
List<Account> accounts =
[
    SELECT Id, Name, Industry
    FROM Account
];
```

---

## Single Record Query

```apex
Account acc =
[
    SELECT Id, Name
    FROM Account
    LIMIT 1
];
```

---

## Using WHERE Clause

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    WHERE Industry = 'Technology'
];
```

---

## Multiple Conditions

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    WHERE Industry = 'Technology'
    AND AnnualRevenue > 1000000
];
```

---

## OR Condition

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    WHERE Industry = 'Technology'
    OR Industry = 'Banking'
];
```

---

## LIMIT

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    LIMIT 10
];
```

---

## ORDER BY

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    ORDER BY Name
];
```

Descending:

```apex
ORDER BY CreatedDate DESC
```

---

## NULL Conditions

```apex
SELECT Id, Name
FROM Contact
WHERE AccountId = NULL
```

```apex
SELECT Id, Name
FROM Contact
WHERE AccountId != NULL
```

---

## IN Operator

```apex
Set<Id> accountIds = new Set<Id>();

SELECT Id, Name
FROM Account
WHERE Id IN :accountIds
```

---

## LIKE Operator

```apex
SELECT Id, Name
FROM Account
WHERE Name LIKE 'A%'
```

Examples:

```sql
'A%'     Starts with A
'%A'     Ends with A
'%A%'    Contains A
```

---

# Querying Custom Objects

Custom object names end with `__c`.

```apex
SELECT Id, Name
FROM Project__c
```

---

# Querying Custom Fields

```apex
SELECT Name,
       Status__c,
       Budget__c
FROM Project__c
```

---

# Bind Variables

Use Apex variables inside SOQL.

```apex
String industry = 'Technology';

List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    WHERE Industry = :industry
];
```

---

# FOR UPDATE

Locks records.

```apex
SELECT Id, Name
FROM Account
WHERE Id = :accId
FOR UPDATE
```

---

# SOQL Query Result Types

## Multiple Records

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
];
```

---

## Single Record

```apex
Account acc =
[
    SELECT Id, Name
    FROM Account
    LIMIT 1
];
```

---

# 2. Relationship Queries

Salesforce supports:

1. Child → Parent
2. Parent → Child

---

# Child to Parent Query

Uses dot notation.

Contact → Account

```apex
SELECT Id,
       FirstName,
       LastName,
       Account.Name
FROM Contact
```

---

## Multiple Parent Levels

```apex
SELECT Id,
       Opportunity.Account.Owner.Name
FROM Opportunity
```

---

# Parent to Child Query

Uses subquery.

Account → Contacts

```apex
SELECT Id,
       Name,
       (
           SELECT Id,
                  FirstName,
                  LastName
           FROM Contacts
       )
FROM Account
```

---

## Loop Through Child Records

```apex
List<Account> accounts =
[
    SELECT Name,
           (
               SELECT LastName
               FROM Contacts
           )
    FROM Account
];

for(Account acc : accounts){

    for(Contact con : acc.Contacts){

        System.debug(con.LastName);
    }
}
```

---

# Standard Relationship Names

| Parent | Child Relationship |
|----------|----------|
| Account | Contacts |
| Account | Opportunities |
| Account | Cases |
| Opportunity | OpportunityLineItems |

---

# Custom Relationship Query

```apex
SELECT Id,
       Name,
       (
           SELECT Id,
                  Name
           FROM Projects__r
       )
FROM Client__c
```

---

# Find Child Relationship Name

```apex
Schema.describeSObjects()
```

Or check:

```text
Object Manager
→ Child Relationship Name
```

---

# 3. Aggregate Queries

Aggregate functions perform calculations on data.

---

## COUNT()

```apex
SELECT COUNT()
FROM Account
```

---

## COUNT(Field)

```apex
SELECT COUNT(Id)
FROM Account
```

---

## SUM()

```apex
SELECT SUM(Amount)
FROM Opportunity
```

---

## AVG()

```apex
SELECT AVG(Amount)
FROM Opportunity
```

---

## MIN()

```apex
SELECT MIN(Amount)
FROM Opportunity
```

---

## MAX()

```apex
SELECT MAX(Amount)
FROM Opportunity
```

---

# GROUP BY

```apex
SELECT Industry,
       COUNT(Id)
FROM Account
GROUP BY Industry
```

---

# AggregateResult

Aggregate queries return AggregateResult.

```apex
AggregateResult[] results =
[
    SELECT Industry,
           COUNT(Id) total
    FROM Account
    GROUP BY Industry
];
```

Access values:

```apex
for(AggregateResult ar : results){

    String industry =
        (String)ar.get('Industry');

    Integer total =
        (Integer)ar.get('total');

    System.debug(industry + ' : ' + total);
}
```

---

# GROUP BY HAVING

Filter grouped records.

```apex
SELECT Industry,
       COUNT(Id)
FROM Account
GROUP BY Industry
HAVING COUNT(Id) > 5
```

---

# Example: Total Opportunity Amount

```apex
AggregateResult ar =
[
    SELECT SUM(Amount) total
    FROM Opportunity
];

Decimal totalAmount =
    (Decimal)ar.get('total');
```

---

# 4. Dynamic SOQL

Dynamic SOQL allows query creation at runtime.

---

# Static SOQL

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
];
```

---

# Dynamic SOQL

```apex
String query =
'SELECT Id, Name FROM Account';

List<Account> accounts =
Database.query(query);
```

---

# Dynamic WHERE Clause

```apex
String industry = 'Technology';

String query =
'SELECT Id, Name ' +
'FROM Account ' +
'WHERE Industry = \'' +
industry +
'\'';

List<Account> accounts =
Database.query(query);
```

---

# Dynamic Field Selection

```apex
String fieldName = 'Name';

String query =
'SELECT Id,' +
fieldName +
' FROM Account';

List<SObject> records =
Database.query(query);
```

---

# Prevent SOQL Injection

❌ Unsafe:

```apex
String query =
'SELECT Id FROM Account ' +
'WHERE Name = \'' +
userInput + '\'';
```

---

✅ Safe:

```apex
String safeValue =
String.escapeSingleQuotes(
    userInput
);
```

```apex
String query =
'SELECT Id FROM Account ' +
'WHERE Name = \'' +
safeValue + '\'';
```

---

# Dynamic Query with Bind Variables

```apex
String industry = 'Technology';

List<Account> accounts =
Database.query(
    'SELECT Id, Name ' +
    'FROM Account ' +
    'WHERE Industry = :industry'
);
```

---

# 5. SOSL (Salesforce Object Search Language)

SOSL searches text across multiple objects.

Use SOSL when:

- Exact object is unknown
- Searching keywords
- Searching across many objects

---

# Basic SOSL Syntax

```apex
FIND 'John'
IN ALL FIELDS
RETURNING Contact
```

---

# Search Contacts

```apex
List<List<SObject>> results =
[
    FIND 'John'
    IN ALL FIELDS
    RETURNING Contact(
        Id,
        FirstName,
        LastName
    )
];
```

---

# Access Results

```apex
List<Contact> contacts =
(List<Contact>)results[0];
```

---

# Search Multiple Objects

```apex
List<List<SObject>> results =
[
    FIND 'Acme'
    IN ALL FIELDS
    RETURNING
        Account(Id, Name),
        Contact(Id, FirstName)
];
```

---

# Search Specific Fields

```apex
FIND 'John'
IN NAME FIELDS
RETURNING Contact
```

Options:

```text
ALL FIELDS
NAME FIELDS
EMAIL FIELDS
PHONE FIELDS
SIDEBAR FIELDS
```

---

# LIMIT Results

```apex
FIND 'John'
IN ALL FIELDS
RETURNING Contact(Id, Name LIMIT 10)
```

---

# SOSL with Wildcards

```apex
FIND 'John*'
IN ALL FIELDS
RETURNING Contact
```

Matches:

```text
John
Johnson
Johnny
```

---

# SOQL vs SOSL

| Feature | SOQL | SOSL |
|----------|----------|----------|
| Search Specific Object | ✅ | ❌ |
| Search Multiple Objects | ❌ | ✅ |
| Relationship Queries | ✅ | ❌ |
| Aggregate Functions | ✅ | ❌ |
| Structured Query | ✅ | ❌ |
| Text Search | Limited | Excellent |

---

# Governor Limits Related to Queries

## SOQL Queries Per Transaction

```text
100 Synchronous
200 Asynchronous
```

---

## Records Retrieved

```text
50,000 Records
```

---

## SOSL Queries Per Transaction

```text
20 Queries
```

---

# Query Optimization Best Practices

## Query Only Needed Fields

❌

```apex
SELECT Id, Name, Industry,
       Phone, Website
FROM Account
```

✅

```apex
SELECT Id, Name
FROM Account
```

---

## Avoid SOQL in Loops

❌

```apex
for(Contact con : contacts){

    Account acc =
    [
        SELECT Id, Name
        FROM Account
        WHERE Id = :con.AccountId
    ];
}
```

---

✅

```apex
Set<Id> accountIds = new Set<Id>();

for(Contact con : contacts){
    accountIds.add(con.AccountId);
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

## Use Selective Filters

Good:

```apex
WHERE Id = :recordId
```

```apex
WHERE CreatedDate = TODAY
```

Avoid:

```apex
WHERE Name LIKE '%ABC%'
```

on very large datasets.

---

# Interview Questions

### Difference between SOQL and SOSL?

SOQL retrieves records from specific objects, while SOSL searches text across multiple objects.

---

### What is an AggregateResult?

A special object used to hold results from aggregate queries such as COUNT, SUM, AVG, MIN, and MAX.

---

### Difference between Child-to-Parent and Parent-to-Child Query?

Child-to-Parent:

```apex
SELECT Account.Name
FROM Contact
```

Parent-to-Child:

```apex
SELECT Name,
(
    SELECT LastName
    FROM Contacts
)
FROM Account
```

---

### What is SOQL Injection?

A security vulnerability where user input alters a dynamic query. Prevent it using:

```apex
String.escapeSingleQuotes()
```

---

### Why should SOQL not be placed inside loops?

It can exceed governor limits and cause runtime exceptions.

---

### What is the maximum number of SOQL queries in a synchronous transaction?

```text
100 SOQL Queries
```

---
