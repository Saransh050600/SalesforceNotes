
---

# 1. Count Total Contacts on Account

### Scenario
Whenever a Contact is inserted, updated, deleted, or undeleted, update the Account field `Total_Contacts__c`.

### Trigger

```apex
trigger ContactTrigger on Contact (
    after insert,
    after update,
    after delete,
    after undelete
) {
    Set<Id> accountIds = new Set<Id>();

    if(Trigger.isInsert || Trigger.isUpdate || Trigger.isUndelete){
        for(Contact con : Trigger.new){
            if(con.AccountId != null){
                accountIds.add(con.AccountId);
            }
        }
    }

    if(Trigger.isDelete){
        for(Contact con : Trigger.old){
            if(con.AccountId != null){
                accountIds.add(con.AccountId);
            }
        }
    }

    ContactTriggerHandler.updateContactCount(accountIds);
}
```

### Handler

```apex
public class ContactTriggerHandler {

    public static void updateContactCount(Set<Id> accountIds){

        Map<Id,Integer> contactCountMap = new Map<Id,Integer>();

        for(AggregateResult ar : [
            SELECT AccountId accId,
                   COUNT(Id) total
            FROM Contact
            WHERE AccountId IN :accountIds
            GROUP BY AccountId
        ]){
            contactCountMap.put(
                (Id)ar.get('accId'),
                (Integer)ar.get('total')
            );
        }

        List<Account> accountsToUpdate = new List<Account>();

        for(Id accId : accountIds){
            accountsToUpdate.add(
                new Account(
                    Id = accId,
                    Total_Contacts__c =
                        contactCountMap.containsKey(accId)
                        ? contactCountMap.get(accId)
                        : 0
                )
            );
        }

        update accountsToUpdate;
    }
}
```

---

# 2. Find Duplicate Contacts Based on Email

### Problem

Return all duplicate Contacts using Email.

### Solution

```apex
Map<String,List<Contact>> duplicateMap = new Map<String,List<Contact>>();

for(Contact con : [
    SELECT Id, Name, Email
    FROM Contact
    WHERE Email != NULL
]){

    if(!duplicateMap.containsKey(con.Email)){
        duplicateMap.put(con.Email,new List<Contact>());
    }

    duplicateMap.get(con.Email).add(con);
}

for(String email : duplicateMap.keySet()){
    if(duplicateMap.get(email).size() > 1){
        System.debug('Duplicate Email => ' + email);
    }
}
```

---

# 3. Highest Salary Employee

### Scenario

Find employee with maximum salary.

### Solution

```apex
Employee__c emp = [
    SELECT Id, Name, Salary__c
    FROM Employee__c
    ORDER BY Salary__c DESC
    LIMIT 1
];

System.debug(emp.Name);
```

---

# 4. Find Second Highest Salary

### Solution

```apex
List<Employee__c> employees = [
    SELECT Name, Salary__c
    FROM Employee__c
    ORDER BY Salary__c DESC
    LIMIT 2
];

if(employees.size() > 1){
    System.debug(employees[1].Name);
}
```

---

# 5. Account with Maximum Contacts

### Solution

```apex
AggregateResult result = [
    SELECT AccountId accId,
           COUNT(Id) total
    FROM Contact
    GROUP BY AccountId
    ORDER BY COUNT(Id) DESC
    LIMIT 1
];

System.debug(result.get('accId'));
System.debug(result.get('total'));
```

---

# 6. Remove Duplicate Strings from List

### Input

```apex
List<String> names =
new List<String>{'John','Mike','John','Sam'};
```

### Solution

```apex
Set<String> uniqueNames = new Set<String>(names);

List<String> finalList =
new List<String>(uniqueNames);

System.debug(finalList);
```

---

# 7. Reverse a String

### Solution

```apex
String str = 'Salesforce';
String reverseStr = '';

for(Integer i = str.length()-1; i >= 0; i--){
    reverseStr += str.substring(i,i+1);
}

System.debug(reverseStr);
```

---

# 8. Check Palindrome

### Input

```
MADAM
```

### Solution

```apex
String str = 'MADAM';
String reverseStr = '';

for(Integer i=str.length()-1;i>=0;i--){
    reverseStr += str.substring(i,i+1);
}

if(str == reverseStr){
    System.debug('Palindrome');
}
```

---

# 9. Count Occurrence of Characters

### Input

```
Salesforce
```

### Solution

```apex
String text = 'Salesforce';

Map<String,Integer> countMap =
new Map<String,Integer>();

for(Integer i=0;i<text.length();i++){

    String ch = text.substring(i,i+1);

    countMap.put(
        ch,
        countMap.containsKey(ch)
        ? countMap.get(ch)+1
        : 1
    );
}

System.debug(countMap);
```

---

# 10. Sort Contacts Alphabetically

### Solution

```apex
List<Contact> contacts = [
    SELECT Id, Name
    FROM Contact
    ORDER BY Name ASC
];
```

---

# 11. Merge Two Lists Without Duplicates

### Solution

```apex
List<String> list1 =
new List<String>{'A','B','C'};

List<String> list2 =
new List<String>{'B','C','D'};

Set<String> result =
new Set<String>();

result.addAll(list1);
result.addAll(list2);

System.debug(result);
```

---

# 12. Find Missing Numbers

### Input

```
1,2,3,5,7,10
```

### Output

```
4,6,8,9
```

### Solution

```apex
List<Integer> numbers =
new List<Integer>{1,2,3,5,7,10};

Set<Integer> numSet =
new Set<Integer>(numbers);

for(Integer i=1;i<=10;i++){
    if(!numSet.contains(i)){
        System.debug(i);
    }
}
```

---

# 13. Bulkify Trigger

### Bad Practice

```apex
for(Account acc : Trigger.new){
    Contact con = [
        SELECT Id
        FROM Contact
        WHERE AccountId = :acc.Id
    ];
}
```

### Good Practice

```apex
Set<Id> accountIds = new Set<Id>();

for(Account acc : Trigger.new){
    accountIds.add(acc.Id);
}

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accountIds
];
```

---

# 14. Create Child Contacts for New Accounts

### Trigger

```apex
trigger AccountTrigger on Account(after insert){

    List<Contact> contacts =
    new List<Contact>();

    for(Account acc : Trigger.new){

        contacts.add(
            new Contact(
                LastName='Default Contact',
                AccountId=acc.Id
            )
        );
    }

    insert contacts;
}
```

---

# 15. Account Contact Wrapper

### Wrapper Class

```apex
public class AccountWrapper {

    public Account accountRec;
    public List<Contact> contacts;

    public AccountWrapper(
        Account acc,
        List<Contact> cons
    ){
        accountRec = acc;
        contacts = cons;
    }
}
```

---

# 16. Dynamic SOQL

### Scenario

User enters object name.

### Solution

```apex
String objectName = 'Account';

String query =
'SELECT Id, Name FROM ' + objectName;

List<SObject> records =
Database.query(query);
```

---

# 17. Dynamic Field Access

### Solution

```apex
Account acc = [
    SELECT Id, Name
    FROM Account
    LIMIT 1
];

String fieldName = 'Name';

System.debug(
    acc.get(fieldName)
);
```

---

# 18. Batch Apex Example

### Batch Class

```apex
global class AccountBatch
implements Database.Batchable<SObject>{

    global Database.QueryLocator start(
        Database.BatchableContext bc){

        return Database.getQueryLocator(
            'SELECT Id FROM Account'
        );
    }

    global void execute(
        Database.BatchableContext bc,
        List<Account> scope){

        for(Account acc : scope){
            acc.Description = 'Updated';
        }

        update scope;
    }

    global void finish(
        Database.BatchableContext bc){
    }
}
```

---

# 19. Queueable Apex

### Solution

```apex
public class AccountQueueable
implements Queueable{

    public void execute(
        QueueableContext qc){

        List<Account> accounts =
        [SELECT Id FROM Account LIMIT 100];

        for(Account acc : accounts){
            acc.Description='Queueable';
        }

        update accounts;
    }
}
```

Execute:

```apex
System.enqueueJob(
    new AccountQueueable()
);
```

---

# 20. Future Method

### Solution

```apex
public class FutureClass {

    @future
    public static void processAccounts(
        List<Id> accountIds){

        List<Account> accounts =
        [SELECT Id FROM Account
         WHERE Id IN :accountIds];

        update accounts;
    }
}
```

---

# Most Asked Mid-Level Apex Interview Topics

1. Trigger Framework
2. Bulkification
3. Collections
4. Maps & Sets
5. Wrapper Classes
6. Dynamic Apex
7. SOQL Optimization
8. Governor Limits
9. Queueable Apex
10. Batch Apex
11. Future Methods
12. Scheduled Apex
13. Platform Events
14. Exception Handling
15. Test Classes
16. Integration Callouts
17. Custom Metadata
18. Sharing & Security
19. Aggregate Queries
20. Design Patterns