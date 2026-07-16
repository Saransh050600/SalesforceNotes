
A complete roadmap for Salesforce Apex interviews, organized from beginner to advanced concepts.

---

# 1. Apex Fundamentals

## Topics

- Syntax
- Variables
- Data Types
- Operators
- Control Statements

## Key Concepts

### Variables

```apex
String name = 'Salesforce';
Integer age = 10;
Boolean active = true;
```

### Control Statements

```apex
if(age > 18){

}
else{

}
```

### Loops

```apex
for(Integer i=0;i<10;i++){

}
```

### Interview Focus

- Difference between Integer, Double, Decimal
- Primitive vs Non-Primitive Data Types
- If vs Switch
- For vs While Loop

---

# 2. Object-Oriented Programming (OOP)

## Topics

- Classes & Objects
- Constructors
- Inheritance
- Polymorphism
- Abstraction
- Encapsulation
- Interfaces

## Key Concepts

### Class

```apex
public class AccountService {

}
```

### Object

```apex
AccountService service =
new AccountService();
```

### Inheritance

```apex
public class Child
extends Parent {

}
```

### Interface

```apex
public interface Processor {

    void execute();
}
```

### Interview Focus

- Four pillars of OOP
- Interface vs Abstract Class
- Virtual vs Override
- Method Overloading vs Overriding

---

# 3. Collections

## Topics

- List
- Set
- Map
- Iterators

## Key Concepts

### List

```apex
List<Account> accounts =
new List<Account>();
```

### Set

```apex
Set<Id> accountIds =
new Set<Id>();
```

### Map

```apex
Map<Id, Account> accountMap =
new Map<Id, Account>();
```

### Interview Focus

- List vs Set vs Map
- Why Map is used in Triggers
- Iterator use cases

---

# 4. SOQL & SOSL

## Topics

- Basic Queries
- Relationship Queries
- Aggregate Queries
- Dynamic SOQL
- Search Operations

## Key Concepts

### SOQL

```apex
SELECT Id, Name
FROM Account
```

### Child → Parent

```apex
SELECT Id,
       Account.Name
FROM Contact
```

### Parent → Child

```apex
SELECT Name,
(
    SELECT LastName
    FROM Contacts
)
FROM Account
```

### Aggregate Query

```apex
SELECT Industry,
       COUNT(Id)
FROM Account
GROUP BY Industry
```

### Interview Focus

- SOQL vs SOSL
- AggregateResult
- Dynamic SOQL
- Query Optimization

---

# 5. DML Operations

## Topics

- Insert
- Update
- Upsert
- Delete
- Undelete
- Merge

## Key Concepts

### Insert

```apex
insert accountRecord;
```

### Update

```apex
update accountRecord;
```

### Upsert

```apex
upsert accountRecord;
```

### Database Methods

```apex
Database.insert(
    records,
    false
);
```

### Interview Focus

- Upsert vs Update
- DML vs Database Methods
- SaveResult
- Partial Success

---

# 6. Triggers

## Topics

- Trigger Events
- Context Variables
- Bulkification
- Trigger Frameworks

## Key Concepts

### Trigger

```apex
trigger AccountTrigger
on Account(before insert){

}
```

### Context Variables

```apex
Trigger.new
Trigger.old
Trigger.newMap
Trigger.oldMap
```

### Bulkification

```apex
Set<Id> ids =
new Set<Id>();
```

### Interview Focus

- Trigger Order of Execution
- Bulkification
- Recursion Handling
- Trigger Frameworks

---

# 7. Exception Handling

## Topics

- Try-Catch
- Custom Exceptions
- Throw Statements

## Key Concepts

### Try Catch

```apex
try{

}
catch(Exception ex){

}
```

### Custom Exception

```apex
public class MyException
extends Exception {

}
```

### Interview Focus

- DmlException
- QueryException
- AuraHandledException
- Custom Exceptions

---

# 8. Governor Limits

## Topics

- SOQL Limits
- DML Limits
- CPU Limits
- Heap Limits
- Callout Limits

## Important Limits

| Limit | Value |
|---------|---------|
| SOQL | 100 |
| DML | 150 |
| Query Rows | 50,000 |
| CPU Time | 10 sec |
| Heap | 6 MB |
| Callouts | 100 |

### Interview Focus

- Why Governor Limits Exist
- CPU Time Limit
- Heap Size Limit
- Best Practices

---

# 9. Apex Testing

## Topics

- Test Classes
- Test Methods
- Assertions
- Test Data Factory
- Test Coverage

## Key Concepts

### Test Class

```apex
@isTest
private class AccountTest {

}
```

### Assertion

```apex
System.assertEquals(
    expected,
    actual
);
```

### Coverage

```text
75% Required
```

### Interview Focus

- @testSetup
- SeeAllData
- Test.startTest()
- Test.stopTest()

---

# 10. Asynchronous Apex

## Topics

- Future Methods
- Queueable Apex
- Batch Apex
- Scheduled Apex

## Key Concepts

### Future

```apex
@future
```

### Queueable

```apex
implements Queueable
```

### Batch

```apex
implements Database.Batchable
```

### Scheduled

```apex
implements Schedulable
```

### Interview Focus

- Future vs Queueable
- Batch Lifecycle
- Stateful Batch
- Cron Expressions

---

# 11. Integrations

## Topics

- REST API
- SOAP API
- HTTP Callouts
- Named Credentials
- JSON/XML Processing

## Key Concepts

### REST

```apex
@RestResource
```

### Callout

```apex
HttpRequest req =
new HttpRequest();
```

### JSON

```apex
JSON.serialize()
JSON.deserialize()
```

### Interview Focus

- REST vs SOAP
- Named Credentials
- WSDL2Apex
- Callout Handling

---

# 12. Security

## Topics

- CRUD
- FLS
- Sharing Rules
- With Sharing
- Without Sharing
- stripInaccessible()

## Key Concepts

### CRUD

```apex
isCreateable()
isUpdateable()
```

### FLS

```apex
isAccessible()
```

### Sharing

```apex
with sharing
```

### Interview Focus

- CRUD vs FLS
- With Sharing vs Without Sharing
- User Mode Operations
- stripInaccessible()

---

# 13. Dynamic Apex

## Topics

- Schema Methods
- Describe Information
- Dynamic SOQL
- Dynamic DML

## Key Concepts

### Schema

```apex
Schema.getGlobalDescribe()
```

### Dynamic Query

```apex
Database.query()
```

### Dynamic DML

```apex
record.put()
record.get()
```

### Interview Focus

- Reflection
- Describe Results
- Dynamic Frameworks
- SOQL Injection Prevention

---

# 14. Platform Features

## Topics

- Platform Events
- Change Data Capture (CDC)
- Custom Metadata
- Custom Settings
- Approval Processes

## Key Concepts

### Platform Event

```apex
EventBus.publish()
```

### CDC

```text
AccountChangeEvent
```

### Custom Metadata

```apex
MyConfig__mdt.getAll()
```

### Approval

```apex
Approval.process()
```

### Interview Focus

- CDC vs Platform Events
- Custom Metadata vs Custom Settings
- Approval APIs

---

# 15. Advanced Apex & Design Patterns

## Topics

- Invocable Methods
- Callable Interface
- Reflection
- Singleton Pattern
- Factory Pattern
- Service Layer
- Unit of Work
- Dependency Injection

## Key Concepts

### Invocable

```apex
@InvocableMethod
```

### Reflection

```apex
Type.forName()
```

### Factory

```apex
ProcessorFactory
```

### Dependency Injection

```apex
public Service(
    IRepository repo
)
```

### Interview Focus

- Enterprise Design Patterns
- Service Layer
- Dependency Injection
- Unit of Work

---

# Most Asked Interview Topics

## Beginner

- Collections
- SOQL
- DML
- Triggers
- OOP

## Intermediate

- Governor Limits
- Batch Apex
- Queueable Apex
- Security
- Dynamic Apex

## Advanced

- Platform Events
- CDC
- Design Patterns
- Integrations
- Enterprise Architecture

---

# Apex Interview Revision Checklist

## Core Apex

- [ ] OOP
- [ ] Collections
- [ ] SOQL
- [ ] DML
- [ ] Triggers
- [ ] Exceptions

## Platform

- [ ] Governor Limits
- [ ] Security
- [ ] Dynamic Apex
- [ ] Platform Events
- [ ] Approval Process

## Async

- [ ] Future
- [ ] Queueable
- [ ] Batch
- [ ] Scheduled

## Integration

- [ ] REST
- [ ] SOAP
- [ ] JSON
- [ ] Named Credentials

## Advanced

- [ ] Design Patterns
- [ ] Dependency Injection
- [ ] Unit of Work
- [ ] Reflection
- [ ] Callable Interface

---

# Interview Preparation Order

```text
1. Apex Fundamentals
2. OOP
3. Collections
4. SOQL/SOSL
5. DML
6. Triggers
7. Exception Handling
8. Governor Limits
9. Apex Testing
10. Async Apex
11. Security
12. Integrations
13. Dynamic Apex
14. Platform Features
15. Design Patterns
```

Following this sequence covers approximately 90–95% of Salesforce Apex interview questions from Developer, Senior Developer, and Technical Lead interviews.