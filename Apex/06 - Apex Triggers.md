
Apex Triggers allow you to execute custom Apex code before or after Salesforce records are inserted, updated, deleted, or undeleted.

Triggers are commonly used for:

- Data validation
- Automation
- Cross-object updates
- Complex business logic
- Integration processing

---

# What is a Trigger?

A trigger is Apex code that executes automatically when a DML event occurs.

```apex
trigger AccountTrigger on Account (
    before insert
) {

    for(Account acc : Trigger.new) {
        acc.Description = 'New Account';
    }
}
```

---

# Trigger Syntax

```apex
trigger TriggerName on ObjectName (
    trigger_events
) {

    // Logic
}
```

Example:

```apex
trigger ContactTrigger on Contact (
    before insert,
    before update
) {

    // Business Logic
}
```

---

# 1. Trigger Events

Salesforce supports seven trigger events.

| Event | Description |
|----------|----------|
| before insert | Before record creation |
| after insert | After record creation |
| before update | Before record update |
| after update | After record update |
| before delete | Before record deletion |
| after delete | After record deletion |
| after undelete | After record restoration |

---

# Before Insert

Runs before records are saved.

```apex
trigger AccountTrigger on Account (
    before insert
) {

    for(Account acc : Trigger.new) {

        acc.Description =
            'Created Automatically';
    }
}
```

Use Cases:

- Populate default values
- Data validation
- Field formatting

---

# After Insert

Runs after record is committed to memory.

```apex
trigger AccountTrigger on Account (
    after insert
) {

    for(Account acc : Trigger.new) {

        System.debug(
            'Created Account: ' +
            acc.Id
        );
    }
}
```

Use Cases:

- Create related records
- Send notifications
- Callouts (using async processing)

---

# Before Update

Runs before updating records.

```apex
trigger AccountTrigger on Account (
    before update
) {

    for(Account acc : Trigger.new) {

        acc.Description =
            'Updated';
    }
}
```

---

# After Update

Runs after update operation.

```apex
trigger AccountTrigger on Account (
    after update
) {

    System.debug(
        'Records Updated'
    );
}
```

---

# Before Delete

Runs before deletion.

```apex
trigger AccountTrigger on Account (
    before delete
) {

    for(Account acc : Trigger.old) {

        System.debug(
            'Deleting: ' +
            acc.Name
        );
    }
}
```

---

# After Delete

Runs after deletion.

```apex
trigger AccountTrigger on Account (
    after delete
) {

    System.debug(
        'Deleted Records'
    );
}
```

---

# After Undelete

Runs after restoring records.

```apex
trigger AccountTrigger on Account (
    after undelete
) {

    System.debug(
        'Records Restored'
    );
}
```

---

# Trigger Event Summary

| Event | Trigger.new | Trigger.old |
|---------|---------|---------|
| before insert | ✅ | ❌ |
| after insert | ✅ | ❌ |
| before update | ✅ | ✅ |
| after update | ✅ | ✅ |
| before delete | ❌ | ✅ |
| after delete | ❌ | ✅ |
| after undelete | ✅ | ❌ |

---

# 2. Context Variables

Context variables provide information about the trigger execution.

Accessed using the `Trigger` class.

---

# Trigger.new

Contains new records.

```apex
for(Account acc : Trigger.new){

    System.debug(acc.Name);
}
```

Available In:

```text
before insert
after insert
before update
after update
after undelete
```

---

# Trigger.old

Contains old versions of records.

```apex
for(Account acc : Trigger.old){

    System.debug(acc.Name);
}
```

Available In:

```text
before update
after update
before delete
after delete
```

---

# Trigger.newMap

Map of new records.

```apex
Map<Id, Account> accMap =
    Trigger.newMap;
```

---

# Trigger.oldMap

Map of old records.

```apex
Map<Id, Account> oldMap =
    Trigger.oldMap;
```

---

# Detect Field Changes

```apex
trigger AccountTrigger on Account (
    before update
) {

    for(Account acc : Trigger.new){

        Account oldAcc =
            Trigger.oldMap.get(acc.Id);

        if(acc.Name != oldAcc.Name){

            System.debug(
                'Name Changed'
            );
        }
    }
}
```

---

# Trigger.size

Number of records.

```apex
System.debug(
    Trigger.size
);
```

---

# Trigger.isInsert

```apex
if(Trigger.isInsert){

}
```

---

# Trigger.isUpdate

```apex
if(Trigger.isUpdate){

}
```

---

# Trigger.isDelete

```apex
if(Trigger.isDelete){

}
```

---

# Trigger.isBefore

```apex
if(Trigger.isBefore){

}
```

---

# Trigger.isAfter

```apex
if(Trigger.isAfter){

}
```

---

# Trigger Operation Example

```apex
if(
    Trigger.isBefore &&
    Trigger.isInsert
){

    System.debug(
        'Before Insert'
    );
}
```

---

# Common Context Variables

| Variable | Description |
|------------|------------|
| Trigger.new | New records |
| Trigger.old | Old records |
| Trigger.newMap | New record map |
| Trigger.oldMap | Old record map |
| Trigger.size | Record count |
| Trigger.isInsert | Insert event |
| Trigger.isUpdate | Update event |
| Trigger.isDelete | Delete event |
| Trigger.isBefore | Before event |
| Trigger.isAfter | After event |

---

# 3. Bulkification

Bulkification means writing code that works efficiently for one record or thousands of records.

This is critical because Salesforce executes triggers in batches.

---

# Why Bulkify?

A trigger can receive:

```text
1 Record
10 Records
200 Records
```

Your code must work for all.

---

# Non-Bulkified Trigger (Bad)

```apex
trigger ContactTrigger
on Contact (after insert) {

    for(Contact con : Trigger.new){

        Account acc =
        [
            SELECT Id, Name
            FROM Account
            WHERE Id = :con.AccountId
        ];
    }
}
```

Problem:

```text
SOQL inside loop
Governor Limit Violation
```

Error:

```text
Too Many SOQL Queries: 101
```

---

# Bulkified Trigger (Good)

```apex
trigger ContactTrigger
on Contact (after insert) {

    Set<Id> accountIds =
        new Set<Id>();

    for(Contact con : Trigger.new){

        if(con.AccountId != null){

            accountIds.add(
                con.AccountId
            );
        }
    }

    Map<Id, Account> accMap =
        new Map<Id, Account>(
            [
                SELECT Id, Name
                FROM Account
                WHERE Id IN :accountIds
            ]
        );

    for(Contact con : Trigger.new){

        Account acc =
            accMap.get(
                con.AccountId
            );
    }
}
```

---

# Bulkification Rules

## Never SOQL Inside Loops

❌ Bad

```apex
for(Account acc : Trigger.new){

    [SELECT Id FROM Contact];
}
```

---

## Never DML Inside Loops

❌ Bad

```apex
for(Account acc : Trigger.new){

    update acc;
}
```

---

## Perform DML Once

✅ Good

```apex
update accountsToUpdate;
```

---

## Use Collections

```apex
List<Account>
Set<Id>
Map<Id, Account>
```

---

# Bulk Trigger Pattern

```apex
Set<Id> accountIds =
new Set<Id>();

for(Contact con : Trigger.new){

    if(con.AccountId != null){

        accountIds.add(
            con.AccountId
        );
    }
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

# 4. Trigger Frameworks

As projects grow, triggers become difficult to maintain.

Best practice:

```text
One Trigger Per Object
Logic in Handler Classes
```

---

# Bad Practice

```apex
trigger AccountTrigger
on Account (
    before insert,
    before update,
    after update
){

    // 500+ lines of code
}
```

---

# Good Practice

```apex
trigger AccountTrigger
on Account (
    before insert,
    before update
){

    AccountTriggerHandler.run();
}
```

---

# Trigger Handler Class

```apex
public class AccountTriggerHandler {

    public static void run(){

        if(
            Trigger.isBefore &&
            Trigger.isInsert
        ){

            beforeInsert();
        }

        if(
            Trigger.isBefore &&
            Trigger.isUpdate
        ){

            beforeUpdate();
        }
    }

    private static void beforeInsert(){

    }

    private static void beforeUpdate(){

    }
}
```

---

# One Trigger Per Object

✅ Recommended

```apex
AccountTrigger
```

Only one trigger should exist per object.

---

# Benefits of Trigger Framework

- Easy maintenance
- Better readability
- Reusable logic
- Unit test friendly
- Supports large applications

---

# Sample Enterprise Structure

```text
triggers/
│
├── AccountTrigger.trigger

classes/
│
├── AccountTriggerHandler.cls
├── AccountService.cls
├── AccountSelector.cls
├── AccountDomain.cls
```

---

# Recursion Prevention

A trigger updating the same object may execute repeatedly.

---

## Static Boolean Pattern

```apex
public class TriggerControl {

    public static Boolean
    isRunning = false;
}
```

```apex
if(TriggerControl.isRunning){

    return;
}

TriggerControl.isRunning = true;
```

---

# Trigger vs Flow

| Feature | Trigger | Flow |
|----------|----------|----------|
| Complex Logic | ✅ | Limited |
| Large Data Volumes | ✅ | Limited |
| UI Based | ❌ | ✅ |
| Custom Processing | ✅ | Limited |
| Callouts | ✅ | Limited |

---

# Trigger Order of Execution

```text
1. System Validation
2. Before Trigger
3. Validation Rules
4. Duplicate Rules
5. Record Saved
6. After Trigger
7. Assignment Rules
8. Auto Response Rules
9. Workflow Rules
10. Processes/Flows
11. Rollup Summary
12. Commit
```

---

# Trigger Interview Questions

### What is Trigger.new?

Contains the new version of records being processed.

---

### Difference Between Trigger.new and Trigger.old?

| Trigger.new | Trigger.old |
|------------|------------|
| New Values | Old Values |
| Insert/Update | Update/Delete |

---

### Why Should SOQL Not Be Used Inside Loops?

It may exceed governor limits and throw:

```text
Too Many SOQL Queries: 101
```

---

### What is Bulkification?

Writing trigger code that can process multiple records efficiently in a single transaction.

---

### What is a Trigger Framework?

A design pattern that moves trigger logic into handler classes for maintainability and scalability.

---

### How Many Triggers Should Exist Per Object?

```text
One Trigger Per Object
```

Using a Trigger Handler Framework.

---

### What is the Maximum Batch Size for Trigger Execution?

```text
200 Records
```

per trigger execution context.

---
