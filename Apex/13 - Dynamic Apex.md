
Dynamic Apex allows developers to access metadata and manipulate objects, fields, and records at runtime without hardcoding object or field names.

Using Dynamic Apex, you can:

- Discover object metadata
- Retrieve field information
- Build dynamic SOQL queries
- Perform dynamic DML operations
- Create generic frameworks and utilities

---

# What is Dynamic Apex?

Normal Apex:

```apex
Account acc = new Account();
acc.Name = 'ABC Corp';
```

Object and field names are known at compile time.

---

Dynamic Apex:

```apex
SObject obj =
Schema.getGlobalDescribe()
      .get('Account')
      .newSObject();

obj.put('Name', 'ABC Corp');
```

Object name is determined at runtime.

---

# Why Use Dynamic Apex?

Common Use Cases:

- Generic data loaders
- Metadata-driven applications
- Framework development
- Dynamic integrations
- Admin-configurable solutions

---

# Dynamic Apex Components

1. Schema Methods
2. Describe Information
3. Dynamic SOQL
4. Dynamic DML

---

# 1. Schema Methods

The Schema class provides access to Salesforce metadata.

---

# Get All Objects

```apex
Map<String, Schema.SObjectType>
globalMap =
Schema.getGlobalDescribe();
```

---

# Example

```apex
Map<String, Schema.SObjectType>
objects =
Schema.getGlobalDescribe();

System.debug(
    objects.keySet()
);
```

Returns:

```text
Account
Contact
Opportunity
Case
Custom Objects
...
```

---

# Get Specific Object

```apex
Schema.SObjectType objType =
Schema.getGlobalDescribe()
      .get('Account');
```

---

# Create Dynamic Record

```apex
SObject account =
Schema.getGlobalDescribe()
      .get('Account')
      .newSObject();
```

---

# Check Object Exists

```apex
Boolean exists =
Schema.getGlobalDescribe()
      .containsKey('Account');
```

---

# Get Object Describe

```apex
Schema.DescribeSObjectResult
describeResult =
Account.SObjectType
       .getDescribe();
```

---

# Common Schema Methods

| Method | Purpose |
|----------|----------|
| getGlobalDescribe() | Get all objects |
| getDescribe() | Get object metadata |
| fields.getMap() | Get all fields |
| newSObject() | Create record dynamically |

---

# 2. Describe Information

Describe Information provides metadata about objects and fields.

---

# Object Describe

```apex
DescribeSObjectResult desc =
Account.SObjectType
       .getDescribe();
```

---

# Get Object Label

```apex
String label =
desc.getLabel();
```

Output:

```text
Account
```

---

# Get API Name

```apex
String apiName =
desc.getName();
```

Output:

```text
Account
```

---

# Is Custom Object?

```apex
Boolean customObj =
desc.isCustom();
```

---

# Is Createable?

```apex
Boolean canCreate =
desc.isCreateable();
```

---

# Is Updateable?

```apex
Boolean canUpdate =
desc.isUpdateable();
```

---

# Is Deletable?

```apex
Boolean canDelete =
desc.isDeletable();
```

---

# Get Record Type Information

```apex
Map<Id, Schema.RecordTypeInfo>
recordTypes =
desc.getRecordTypeInfosById();
```

---

# Get All Fields

```apex
Map<String,
    Schema.SObjectField>
fieldMap =
Account.SObjectType
       .getDescribe()
       .fields.getMap();
```

---

# Loop Through Fields

```apex
for(String fieldName :
    fieldMap.keySet()){

    System.debug(fieldName);
}
```

---

# Field Describe

```apex
Schema.DescribeFieldResult
fieldDesc =
Account.Name
       .getDescribe();
```

---

# Field Label

```apex
fieldDesc.getLabel();
```

---

# Field Type

```apex
fieldDesc.getType();
```

Example:

```text
STRING
DATE
DOUBLE
BOOLEAN
```

---

# Field Length

```apex
fieldDesc.getLength();
```

---

# Is Field Required?

```apex
fieldDesc.isNillable();
```

Returns:

```text
false → Required
true  → Optional
```

---

# Is Custom Field?

```apex
fieldDesc.isCustom();
```

---

# Describe Example

```apex
Schema.DescribeFieldResult desc =
Account.Name.getDescribe();

System.debug(
    desc.getLabel()
);

System.debug(
    desc.getType()
);
```

---

# Common Describe Methods

| Method | Description |
|----------|----------|
| getLabel() | Field label |
| getName() | API name |
| getType() | Data type |
| getLength() | Length |
| isAccessible() | Read access |
| isCreateable() | Create access |
| isUpdateable() | Update access |
| isCustom() | Custom field |

---

# 3. Dynamic SOQL

Dynamic SOQL builds queries at runtime.

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
'SELECT Id, Name ' +
'FROM Account';

List<SObject> records =
Database.query(query);
```

---

# Dynamic Object Query

```apex
String objectName =
'Account';

String query =
'SELECT Id, Name ' +
'FROM ' + objectName;

List<SObject> records =
Database.query(query);
```

---

# Dynamic WHERE Clause

```apex
String industry =
'Technology';

String query =
'SELECT Id, Name ' +
'FROM Account ' +
'WHERE Industry = \'' +
industry +
'\'';
```

---

# Execute Query

```apex
List<SObject> records =
Database.query(query);
```

---

# Dynamic Field Selection

```apex
String fieldName =
'Name';

String query =
'SELECT Id,' +
fieldName +
' FROM Account';
```

---

# Dynamic Query Example

```apex
String objectName =
'Contact';

String query =
'SELECT Id, LastName ' +
'FROM ' + objectName;

List<SObject> results =
Database.query(query);
```

---

# Access Dynamic Results

```apex
for(SObject record : results){

    System.debug(
        record.get('LastName')
    );
}
```

---

# Prevent SOQL Injection

---

## Unsafe

```apex
String query =
'SELECT Id ' +
'FROM Account ' +
'WHERE Name = \'' +
userInput +
'\'';
```

---

## Safe

```apex
String safeInput =
String.escapeSingleQuotes(
    userInput
);
```

---

```apex
String query =
'SELECT Id ' +
'FROM Account ' +
'WHERE Name = \'' +
safeInput +
'\'';
```

---

# Dynamic Query with Bind Variables

```apex
String industry =
'Technology';

List<SObject> records =
Database.query(
'SELECT Id, Name ' +
'FROM Account ' +
'WHERE Industry = :industry'
);
```

---

# Dynamic Query Builder Example

```apex
public static List<SObject>
getRecords(
    String objectName,
    String fieldName
){

    String query =
        'SELECT Id,' +
        fieldName +
        ' FROM ' +
        objectName;

    return Database.query(
        query
    );
}
```

---

# 4. Dynamic DML

Dynamic DML performs record operations without knowing object types at compile time.

---

# Create Dynamic Record

```apex
SObject obj =
Schema.getGlobalDescribe()
      .get('Account')
      .newSObject();
```

---

# Set Field Values

```apex
obj.put(
    'Name',
    'ABC Corporation'
);
```

---

# Insert Dynamic Record

```apex
insert obj;
```

---

# Full Example

```apex
SObject account =
Schema.getGlobalDescribe()
      .get('Account')
      .newSObject();

account.put(
    'Name',
    'ABC Corporation'
);

insert account;
```

---

# Read Field Values

```apex
String name =
(String)
account.get('Name');
```

---

# Update Dynamic Record

```apex
account.put(
    'Phone',
    '9999999999'
);

update account;
```

---

# Delete Dynamic Record

```apex
delete account;
```

---

# Dynamic Upsert

```apex
upsert account;
```

---

# Dynamic DML Using Database Class

```apex
Database.insert(
    account
);
```

---

# Generic Insert Utility

```apex
public static void
createRecord(
    String objectName,
    Map<String,Object> values
){

    SObject record =
    Schema.getGlobalDescribe()
          .get(objectName)
          .newSObject();

    for(String field :
        values.keySet()){

        record.put(
            field,
            values.get(field)
        );
    }

    insert record;
}
```

Usage:

```apex
Map<String,Object> values =
new Map<String,Object>();

values.put(
    'Name',
    'Dynamic Account'
);

createRecord(
    'Account',
    values
);
```

---

# Dynamic Field Access

---

## Get Value

```apex
String name =
(String)
record.get('Name');
```

---

## Set Value

```apex
record.put(
    'Name',
    'Updated Name'
);
```

---

# Dynamic Apex Security

Always validate:

```apex
CRUD
FLS
```

before dynamic operations.

---

## Check Object Access

```apex
Schema.SObjectType objType =
Schema.getGlobalDescribe()
      .get(objectName);

if(
    objType.getDescribe()
           .isCreateable()
){

}
```

---

## Check Field Access

```apex
fieldMap.get(fieldName)
        .getDescribe()
        .isAccessible();
```

---

# Real-World Use Cases

## Generic Data Loader

```text
CSV
 ↓
Dynamic Mapping
 ↓
Any Salesforce Object
```

---

## Metadata-Driven Framework

```text
Admin Configures Object
 ↓
Dynamic Query
 ↓
Display Records
```

---

## Dynamic Integration

```text
External JSON
 ↓
Dynamic Object Mapping
 ↓
Insert Records
```

---

# Interview Questions

### What is Dynamic Apex?

Dynamic Apex allows metadata access and runtime manipulation of objects, fields, and records.

---

### What is Schema.getGlobalDescribe()?

Returns all Salesforce objects as a map.

```apex
Map<String, Schema.SObjectType>
```

---

### Difference Between Static and Dynamic SOQL?

| Static SOQL | Dynamic SOQL |
|-------------|-------------|
| Compile-time query | Runtime query |
| Better performance | More flexible |
| Fixed object names | Dynamic object names |

---

### What Method Executes Dynamic SOQL?

```apex
Database.query()
```

---

### How Do You Set a Field Dynamically?

```apex
record.put(
    'Name',
    'ABC'
);
```

---

### How Do You Read a Field Dynamically?

```apex
record.get(
    'Name'
);
```

---

### How Do You Create an Object Dynamically?

```apex
Schema.getGlobalDescribe()
      .get('Account')
      .newSObject();
```

---

### What Security Concern Exists in Dynamic SOQL?

```text
SOQL Injection
```

Prevent using:

```apex
String.escapeSingleQuotes()
```

and proper input validation.

---

### Common Dynamic Apex Methods

| Method | Purpose |
|----------|----------|
| Schema.getGlobalDescribe() | All objects |
| getDescribe() | Metadata |
| fields.getMap() | Fields |
| Database.query() | Dynamic SOQL |
| get() | Dynamic field read |
| put() | Dynamic field write |
| newSObject() | Dynamic record creation |

---

**Next Topic:** Advanced Apex Features (Custom Metadata, Custom Settings, Platform Cache, Transaction Finalizers, Platform Events, Continuations).