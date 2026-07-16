
DML (Data Manipulation Language) operations are used to manipulate records in Salesforce.

Using DML, you can:

- Create records
- Modify records
- Delete records
- Restore deleted records
- Merge duplicate records

---

# What is DML?

DML statements perform database operations on Salesforce records (sObjects).

```apex
Account acc = new Account(
    Name = 'ABC Corporation'
);

insert acc;
```

---

# Types of DML Operations

| Operation | Purpose |
|------------|------------|
| Insert | Create records |
| Update | Modify records |
| Upsert | Insert or Update |
| Delete | Remove records |
| Undelete | Restore deleted records |
| Merge | Combine duplicate records |

---

# 1. Insert

Creates new records.

---

## Insert Single Record

```apex
Account acc = new Account();

acc.Name = 'ABC Corporation';
acc.Phone = '1234567890';

insert acc;
```

---

## Insert Using Constructor

```apex
Account acc = new Account(
    Name = 'ABC Corporation',
    Industry = 'Technology'
);

insert acc;
```

---

## Insert Multiple Records

```apex
List<Account> accounts =
new List<Account>();

accounts.add(
    new Account(Name='Account A')
);

accounts.add(
    new Account(Name='Account B')
);

insert accounts;
```

---

## Access Generated Record Id

```apex
Account acc = new Account(
    Name='ABC Corporation'
);

insert acc;

System.debug(acc.Id);
```

---

# Insert Considerations

- Required fields must be populated
- Validation Rules are executed
- Triggers execute
- Flows execute
- Assignment Rules may execute

---

# 2. Update

Updates existing records.

---

## Update Single Record

```apex
Account acc =
[
    SELECT Id, Name
    FROM Account
    LIMIT 1
];

acc.Name = 'Updated Name';

update acc;
```

---

## Update Multiple Records

```apex
List<Account> accounts =
[
    SELECT Id, Name
    FROM Account
    LIMIT 5
];

for(Account acc : accounts){
    acc.Name += ' Updated';
}

update accounts;
```

---

## Partial Update

Only modified fields are updated.

```apex
Account acc = new Account(
    Id='001XXXXXXXXXXXXXXX'
);

acc.Phone = '9999999999';

update acc;
```

---

# 3. Upsert

Upsert = Update + Insert

If record exists → Update

If record does not exist → Insert

---

## Upsert by Salesforce Id

```apex
Account acc = new Account();

acc.Id = '001XXXXXXXXXXXXXXX';
acc.Name = 'Updated Account';

upsert acc;
```

---

## Upsert New Record

```apex
Account acc = new Account(
    Name='New Account'
);

upsert acc;
```

Record does not exist, so Salesforce inserts it.

---

# External Id Upsert

Very common in integrations.

---

## Create External ID Field

```text
ERP_Id__c
```

Mark field as:

```text
External ID
Unique
```

---

## Upsert Using External ID

```apex
Account acc = new Account();

acc.ERP_Id__c = 'ERP1001';
acc.Name = 'ABC Corporation';

upsert acc ERP_Id__c;
```

Salesforce checks:

```text
ERP1001 Exists?
YES → Update
NO  → Insert
```

---

# Bulk Upsert

```apex
List<Account> accounts =
new List<Account>();

upsert accounts ERP_Id__c;
```

---

# Why Use Upsert?

- Data migration
- Integrations
- ETL tools
- Prevent duplicates

---

# 4. Delete

Removes records.

Deleted records go to Recycle Bin.

---

## Delete Single Record

```apex
Account acc =
[
    SELECT Id
    FROM Account
    LIMIT 1
];

delete acc;
```

---

## Delete Multiple Records

```apex
List<Account> accounts =
[
    SELECT Id
    FROM Account
    LIMIT 5
];

delete accounts;
```

---

## Delete Using Id

```apex
Account acc =
new Account(
    Id='001XXXXXXXXXXXXXXX'
);

delete acc;
```

---

# Delete Considerations

- Child records may be affected
- Validation may occur
- Triggers execute
- Recycle Bin stores deleted record

---

# Hard Delete vs Soft Delete

| Type | Description |
|--------|--------|
| Soft Delete | Recycle Bin |
| Hard Delete | Permanently removed |

Standard DML performs soft delete.

---

# 5. Undelete

Restores records from Recycle Bin.

---

## Undelete Single Record

```apex
Account acc =
[
    SELECT Id
    FROM Account
    WHERE IsDeleted = TRUE
    ALL ROWS
    LIMIT 1
];

undelete acc;
```

---

## Undelete Multiple Records

```apex
List<Account> accounts =
[
    SELECT Id
    FROM Account
    WHERE IsDeleted = TRUE
    ALL ROWS
];

undelete accounts;
```

---

# ALL ROWS Keyword

Required for querying deleted records.

```apex
SELECT Id, Name
FROM Account
ALL ROWS
```

---

# Example

```apex
Account acc =
new Account(
    Name='Test Account'
);

insert acc;

delete acc;

undelete acc;
```

---

# 6. Merge

Combines duplicate records.

Available only for:

- Account
- Contact
- Lead
- Person Account

---

# Merge Two Accounts

```apex
Account master =
[
    SELECT Id, Name
    FROM Account
    WHERE Name='Account A'
    LIMIT 1
];

Account duplicate =
[
    SELECT Id, Name
    FROM Account
    WHERE Name='Account B'
    LIMIT 1
];

merge master duplicate;
```

---

# Merge Three Accounts

```apex
merge master dup1 dup2;
```

Maximum:

```text
1 Master + 2 Duplicates
```

---

# What Happens During Merge?

```text
Master Record Survives
Duplicate Records Deleted
Related Records Reparented
Activities Moved
Notes Moved
Attachments Moved
```

---

# Merge Example

Before:

```text
Account A
Account B
Account C
```

After:

```text
Account A (Master)
```

B and C are merged into A.

---

# DML Statements vs Database Methods

## DML Statement

```apex
insert accounts;
```

If one record fails:

```text
Entire operation fails
```

---

## Database Method

```apex
Database.insert(
    accounts,
    false
);
```

If one record fails:

```text
Other records succeed
```

---

# AllOrNone Behavior

## Default

```apex
insert accounts;
```

Equivalent to:

```apex
Database.insert(
    accounts,
    true
);
```

---

## Partial Success

```apex
Database.insert(
    accounts,
    false
);
```

---

# SaveResult Example

```apex
Database.SaveResult[] results =
Database.insert(
    accounts,
    false
);

for(Database.SaveResult sr : results){

    if(sr.isSuccess()){

        System.debug(
            'Inserted: ' +
            sr.getId()
        );

    } else {

        for(Database.Error err :
            sr.getErrors()){

            System.debug(
                err.getMessage()
            );
        }
    }
}
```

---

# DML Governor Limits

## DML Statements

```text
150 DML Statements
```

Examples:

```apex
insert acc;
update acc;
delete acc;
```

Each counts as one DML statement.

---

## Records Processed

```text
10,000 Records
```

Maximum records processed in a transaction.

---

# Bulkification Example

❌ Bad

```apex
for(Account acc : accounts){

    insert acc;
}
```

May hit:

```text
Too Many DML Statements: 151
```

---

✅ Good

```apex
insert accounts;
```

Single DML statement.

---

# DML Execution Order

When DML occurs:

```text
1. Validation Rules
2. Before Triggers
3. Duplicate Rules
4. Record Saved
5. After Triggers
6. Assignment Rules
7. Auto Response Rules
8. Workflow Rules
9. Processes/Flows
10. Roll-Up Summary
11. Commit
```

---

# Common Exceptions

## DmlException

```apex
try{

    insert acc;

}catch(DmlException ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## Example

```apex
Account acc =
new Account();

insert acc;
```

Error:

```text
REQUIRED_FIELD_MISSING
```

---

# Interview Questions

### Difference Between Insert and Upsert?

| Insert | Upsert |
|----------|----------|
| Only creates | Creates or updates |
| Fails on duplicate Id | Updates existing record |

---

### Why Use External ID with Upsert?

To uniquely identify records during integrations and migrations.

---

### Difference Between Delete and Undelete?

| Delete | Undelete |
|----------|----------|
| Moves to Recycle Bin | Restores record |

---

### What Objects Support Merge?

- Account
- Contact
- Lead
- Person Account

---

### Difference Between DML Statements and Database Methods?

| DML Statement | Database Method |
|---------------|---------------|
| All-or-none | Supports partial success |
| Throws exception | Returns SaveResult |

---

### What Is the DML Governor Limit?

```text
150 DML Statements
10,000 Records Processed
```

---

### Why Should DML Not Be Inside Loops?

To avoid governor limit violations and improve performance.

```apex
insert records;
```

is preferred over:

```apex
for(Account acc : records){
    insert acc;
}
```
