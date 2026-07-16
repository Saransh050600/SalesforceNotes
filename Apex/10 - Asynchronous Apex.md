
Asynchronous Apex allows code to run in the background instead of executing immediately within the current transaction.

This helps:

- Process large volumes of data
- Perform callouts
- Avoid governor limits
- Improve user experience
- Execute long-running operations

---

# What is Asynchronous Processing?

## Synchronous Execution

```text
User Clicks Button
        ↓
Code Executes
        ↓
User Waits
        ↓
Response Returned
```

---

## Asynchronous Execution

```text
User Clicks Button
        ↓
Job Queued
        ↓
Immediate Response
        ↓
Background Processing
```

---

# Types of Asynchronous Apex

| Type | Purpose |
|---------|---------|
| Future Methods | Simple background processing |
| Queueable Apex | Advanced async processing |
| Batch Apex | Large data processing |
| Scheduled Apex | Time-based execution |

---

# 1. Future Methods

Future Methods execute asynchronously in a separate thread.

---

# Why Use Future Methods?

- Callouts
- Long-running operations
- Non-blocking processing

---

# Syntax

```apex
@future
public static void methodName(){

}
```

---

# Basic Example

```apex
public class FutureExample {

    @future
    public static void processAccounts(){

        System.debug(
            'Processing'
        );
    }
}
```

Call Method:

```apex
FutureExample.processAccounts();
```

---

# Future Method with Parameters

Only primitive types or collections of primitives are allowed.

```apex
public class FutureExample {

    @future
    public static void updateAccounts(
        List<Id> accountIds
    ){

        List<Account> accounts =
        [
            SELECT Id, Description
            FROM Account
            WHERE Id IN :accountIds
        ];

        for(Account acc : accounts){

            acc.Description =
                'Processed';
        }

        update accounts;
    }
}
```

---

# Future Callout

Use:

```apex
@future(callout=true)
```

---

## Example

```apex
public class IntegrationService {

    @future(callout=true)
    public static void sendData(){

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
    }
}
```

---

# Future Method Limitations

❌ Cannot return values

```apex
@future
public static String getData()
```

Invalid.

---

❌ Cannot accept sObjects

```apex
@future
public static void process(
    Account acc
)
```

Invalid.

---

❌ Cannot chain future methods

---

# Future Limits

```text
50 Future Calls
```

per transaction.

---

# 2. Queueable Apex

Queueable Apex is the modern replacement for Future Methods.

Provides:

- Complex objects
- Job monitoring
- Chaining
- Better flexibility

---

# Implement Queueable

```apex
public class AccountQueueable
implements Queueable {

    public void execute(
        QueueableContext qc
    ){

        System.debug(
            'Queue Running'
        );
    }
}
```

---

# Execute Queueable

```apex
System.enqueueJob(
    new AccountQueueable()
);
```

---

# Queueable with Constructor

```apex
public class AccountQueueable
implements Queueable {

    private List<Id> accountIds;

    public AccountQueueable(
        List<Id> accountIds
    ){
        this.accountIds =
            accountIds;
    }

    public void execute(
        QueueableContext qc
    ){

        List<Account> accounts =
        [
            SELECT Id, Name
            FROM Account
            WHERE Id IN :accountIds
        ];
    }
}
```

Execution:

```apex
System.enqueueJob(
    new AccountQueueable(
        accountIds
    )
);
```

---

# Queueable Chaining

One Queueable can start another.

---

## First Queue

```apex
public class QueueOne
implements Queueable {

    public void execute(
        QueueableContext qc
    ){

        System.enqueueJob(
            new QueueTwo()
        );
    }
}
```

---

## Second Queue

```apex
public class QueueTwo
implements Queueable {

    public void execute(
        QueueableContext qc
    ){

        System.debug(
            'Second Job'
        );
    }
}
```

---

# Queueable with Callout

```apex
public class CalloutQueue
implements Queueable,
Database.AllowsCallouts {

    public void execute(
        QueueableContext qc
    ){

        HttpRequest req =
            new HttpRequest();

        Http http =
            new Http();

        http.send(req);
    }
}
```

---

# Queueable Benefits

- Supports complex types
- Supports chaining
- Better monitoring
- Job ID available

---

# Queueable Limits

```text
50 Jobs Added Per Transaction
```

---

# Monitor Queueable Jobs

```apex
SELECT Id,
       Status,
       JobType
FROM AsyncApexJob
```

---

# Future vs Queueable

| Feature | Future | Queueable |
|----------|----------|----------|
| Primitive Params | ✅ | ✅ |
| Complex Objects | ❌ | ✅ |
| Chaining | ❌ | ✅ |
| Monitoring | Limited | Better |
| Recommended | ❌ | ✅ |

---

# 3. Batch Apex

Batch Apex processes large datasets in manageable chunks.

---

# Why Batch Apex?

Governor limits prevent processing huge datasets in one transaction.

Batch Apex solves this.

---

# Processing Flow

```text
Start()
   ↓
Execute()
   ↓
Execute()
   ↓
Execute()
   ↓
Finish()
```

---

# Batch Interface

```apex
Database.Batchable<SObject>
```

---

# Basic Batch Class

```apex
global class AccountBatch
implements Database.Batchable<SObject> {

    global Database.QueryLocator
    start(
        Database.BatchableContext bc
    ){

        return Database.getQueryLocator(
            'SELECT Id, Name FROM Account'
        );
    }

    global void execute(
        Database.BatchableContext bc,
        List<Account> scope
    ){

    }

    global void finish(
        Database.BatchableContext bc
    ){

    }
}
```

---

# Execute Batch

```apex
Database.executeBatch(
    new AccountBatch()
);
```

---

# Batch Scope Size

Default:

```text
200 Records
```

Custom:

```apex
Database.executeBatch(
    new AccountBatch(),
    100
);
```

---

# Example Batch Update

```apex
global class AccountBatch
implements Database.Batchable<SObject>{

    global Database.QueryLocator
    start(
        Database.BatchableContext bc
    ){

        return Database.getQueryLocator(
            'SELECT Id, Description FROM Account'
        );
    }

    global void execute(
        Database.BatchableContext bc,
        List<Account> scope
    ){

        for(Account acc : scope){

            acc.Description =
                'Batch Updated';
        }

        update scope;
    }

    global void finish(
        Database.BatchableContext bc
    ){

        System.debug(
            'Batch Complete'
        );
    }
}
```

---

# Stateful Batch

Retains values between execute calls.

---

## Example

```apex
global class AccountBatch
implements Database.Batchable<SObject>,
           Database.Stateful {

    Integer totalRecords = 0;

    global void execute(
        Database.BatchableContext bc,
        List<Account> scope
    ){

        totalRecords += scope.size();
    }
}
```

---

# Batch with Callouts

```apex
implements Database.AllowsCallouts
```

---

# Monitor Batch Jobs

```apex
SELECT Id,
       Status,
       NumberOfErrors
FROM AsyncApexJob
```

---

# Batch Limits

```text
50 Million Records
```

via QueryLocator.

---

# Batch for Multiple Inputs

Database.Batchable` does not support method parameters in the `start()`, `execute()`, or `finish()` methods. To pass data into a Batch Apex job, use the batch class constructor and store values in instance variables.  
  
This is a common interview topic and is frequently used in real-world projects when batches need dynamic filtering or configuration.  
  
---  
  
# Method 1: Constructor Parameters  
  
### Batch Class  
  
```apex  
public class AccountBatch implements Database.Batchable<SObject> {  
  
private String industry;  
private Integer minEmployees;  
  
public AccountBatch(String industry, Integer minEmployees) {  
this.industry = industry;  
this.minEmployees = minEmployees;  
}  
  
public Database.QueryLocator start(Database.BatchableContext bc) {  
  
return Database.getQueryLocator([  
SELECT Id, Name, Industry, NumberOfEmployees  
FROM Account  
WHERE Industry = :industry  
AND NumberOfEmployees >= :minEmployees  
]);  
}  
  
public void execute(Database.BatchableContext bc, List<Account> scope) {  
// Processing Logic  
}  
  
public void finish(Database.BatchableContext bc) {  
System.debug('Batch Completed');  
}  
}  
```  
  
### Execute Batch  
  
```apex  
Database.executeBatch(  
new AccountBatch('Technology', 100),  
200  
);  
```  
  
### When to Use  
  
- Small number of parameters  
- Simple filtering logic  
- Quick implementation  
  
---  
  
# Method 2: Wrapper Class (Recommended)  
  
When multiple parameters are required, use a wrapper class instead of adding many constructor arguments.  
  
### Wrapper Class  
  
```apex  
public class BatchInputWrapper {  
  
public String industry;  
public Integer minEmployees;  
public Date createdAfter;  
  
public BatchInputWrapper(  
String industry,  
Integer minEmployees,  
Date createdAfter  
) {  
this.industry = industry;  
this.minEmployees = minEmployees;  
this.createdAfter = createdAfter;  
}  
}  
```  
  
### Batch Class  
  
```apex  
public class AccountBatch implements Database.Batchable<SObject> {  
  
private BatchInputWrapper input;  
  
public AccountBatch(BatchInputWrapper input) {  
this.input = input;  
}  
  
public Database.QueryLocator start(Database.BatchableContext bc) {  
  
return Database.getQueryLocator([  
SELECT Id, Name  
FROM Account  
WHERE Industry = :input.industry  
AND NumberOfEmployees >= :input.minEmployees  
AND CreatedDate >= :input.createdAfter  
]);  
}  
  
public void execute(Database.BatchableContext bc, List<Account> scope) {  
// Processing Logic  
}  
  
public void finish(Database.BatchableContext bc) {  
}  
}  
```  
  
### Execute Batch  
  
```apex  
BatchInputWrapper input =  
new BatchInputWrapper(  
'Technology',  
500,  
Date.today().addMonths(-6)  
);  
  
Database.executeBatch(  
new AccountBatch(input),  
200  
);  
```  
  
### Advantages  
  
- Clean code  
- Easy maintenance  
- Easily extensible  
- Preferred in enterprise projects  
  
---  
  
# Method 3: Passing List or Set of IDs  
  
Useful when specific records need processing.  
  
### Batch Class  
  
```apex  
public class ContactBatch implements Database.Batchable<SObject> {  
  
private Set<Id> accountIds;  
  
public ContactBatch(Set<Id> accountIds) {  
this.accountIds = accountIds;  
}  
  
public Database.QueryLocator start(Database.BatchableContext bc) {  
  
return Database.getQueryLocator([  
SELECT Id, Name, AccountId  
FROM Contact  
WHERE AccountId IN :accountIds  
]);  
}  
  
public void execute(Database.BatchableContext bc, List<Contact> scope) {  
// Processing Logic  
}  
  
public void finish(Database.BatchableContext bc) {  
}  
}  
```  
  
### Execute Batch  
  
```apex  
Set<Id> accountIds = new Set<Id>{  
'001XXXXXXXXXXXXAAA',  
'001XXXXXXXXXXXXBBB'  
};  
  
Database.executeBatch(  
new ContactBatch(accountIds),  
100  
);  
```  
  
### Use Cases  
  
- Process selected records  
- Retry failed records  
- Batch from UI selections  
  
---  
  
# Method 4: Passing a Map of Filters  
  
Useful for dynamic filtering.  
  
### Batch Class  
  
```apex  
public class DynamicBatch implements Database.Batchable<SObject> {  
  
private Map<String, Object> filters;  
  
public DynamicBatch(Map<String, Object> filters) {  
this.filters = filters;  
}  
  
public Database.QueryLocator start(Database.BatchableContext bc) {  
  
String industry = (String) filters.get('industry');  
Integer size = (Integer) filters.get('size');  
  
String query =  
'SELECT Id, Name ' +  
'FROM Account ' +  
'WHERE Industry = :industry ' +  
'AND NumberOfEmployees >= :size';  
  
return Database.getQueryLocator(query);  
}  
  
public void execute(Database.BatchableContext bc, List<Account> scope) {  
}  
  
public void finish(Database.BatchableContext bc) {  
}  
}  
```  
  
### Execute Batch  
  
```apex  
Map<String, Object> filters =  
new Map<String, Object>{  
'industry' => 'Technology',  
'size' => 500  
};  
  
Database.executeBatch(  
new DynamicBatch(filters),  
200  
);  
```  
  
---  
  
# Real-World Example  
  
### Batch to Process Accounts for a Specific Region  
  
```apex  
public class RegionAccountBatch implements Database.Batchable<SObject> {  
  
private String region;  
private Date startDate;  
  
public RegionAccountBatch(  
String region,  
Date startDate  
) {  
this.region = region;  
this.startDate = startDate;  
}  
  
public Database.QueryLocator start(Database.BatchableContext bc) {  
  
return Database.getQueryLocator([  
SELECT Id, Name  
FROM Account  
WHERE Region__c = :region  
AND CreatedDate >= :startDate  
]);  
}  
  
public void execute(  
Database.BatchableContext bc,  
List<Account> scope  
) {  
for(Account acc : scope) {  
acc.Status__c = 'Processed';  
}  
  
update scope;  
}  
  
public void finish(Database.BatchableContext bc) {  
System.debug('Region Processing Completed');  
}  
}  
```  
  
### Execute  
  
```apex  
Database.executeBatch(  
new RegionAccountBatch(  
'APAC',  
Date.today().addMonths(-3)  
),  
200  
);  
```  
  
---  
  
# Interview Questions  
  
### Q1: How can you pass multiple parameters to a Batch Apex class?  
  
Pass them through the constructor and store them in instance variables.  
  
---  
  
### Q2: What is the best approach when many parameters are required?  
  
Use a Wrapper Class.  
  
---  
  
### Q3: Can we pass a List<Id> to a Batch Apex class?  
  
Yes. Collections such as List<Id> and Set<Id> can be passed through constructors.  
  
---  
  
### Q4: Can Batch Apex methods accept custom parameters?  
  
No. Only the constructor can receive custom inputs.  
  
---  
  
### Q5: Why use a Wrapper Class?  
  
- Better readability  
- Easier maintenance  
- Easier future enhancements  
- Reduces constructor complexity  
  
---  
  
# Best Practices  
  
✅ Use Wrapper Classes for multiple inputs.  
  
✅ Keep constructor logic simple.  
  
✅ Pass only required data.  
  
✅ Avoid passing very large collections.  
  
✅ Store configuration in Custom Metadata when possible.  
  
✅ Use Database.Stateful only when state persistence is required.  
  
---  
  
# Interview Summary  
  
Batch Apex supports multiple inputs through constructor parameters. For simple use cases, primitive values can be passed directly. For enterprise-level implementations, a Wrapper Class is the preferred approach because it improves maintainability, scalability, and readability.
# 4. Scheduled Apex

Scheduled Apex executes code at a specific time.

---

# Implement Schedulable

```apex
global class DailyJob
implements Schedulable {

    global void execute(
        SchedulableContext sc
    ){

        System.debug(
            'Job Running'
        );
    }
}
```

---

# Schedule Job

```apex
String cron =
'0 0 2 * * ?';

System.schedule(
    'Daily Job',
    cron,
    new DailyJob()
);
```

---

# Cron Expression Format

```text
Seconds
Minutes
Hours
Day of Month
Month
Day of Week
Optional Year
```

---

# Example Cron Expressions

Run Daily at 2 AM

```text
0 0 2 * * ?
```

---

Run Every Hour

```text
0 0 * * * ?
```

---

Run Every Monday

```text
0 0 8 ? * MON
```

---

# Schedule Batch Apex

```apex
global class ScheduleBatch
implements Schedulable {

    global void execute(
        SchedulableContext sc
    ){

        Database.executeBatch(
            new AccountBatch()
        );
    }
}
```

---

# Monitor Scheduled Jobs

```apex
SELECT Id,
       CronExpression,
       State
FROM CronTrigger
```

---

# Async Apex Monitoring Objects

## AsyncApexJob

Tracks:

- Batch Jobs
- Queueable Jobs
- Future Jobs

```apex
SELECT Id,
       Status,
       JobType
FROM AsyncApexJob
```

---

## CronTrigger

Tracks scheduled jobs.

```apex
SELECT Id,
       State,
       NextFireTime
FROM CronTrigger
```

---

# Comparison

| Feature | Future | Queueable | Batch | Scheduled |
|----------|----------|----------|----------|----------|
| Background Processing | ✅ | ✅ | ✅ | ✅ |
| Complex Objects | ❌ | ✅ | ✅ | ✅ |
| Chaining | ❌ | ✅ | Limited | N/A |
| Callouts | ✅ | ✅ | ✅ | Via Async |
| Large Data Volume | ❌ | ❌ | ✅ | Via Batch |
| Scheduling | ❌ | ❌ | ❌ | ✅ |

---

# Best Practices

## Use Queueable Instead of Future

✅ Preferred

```apex
System.enqueueJob()
```

---

## Use Batch for Large Data

```text
Thousands/Millions of Records
```

---

## Use Scheduled Apex for Automation

```text
Daily
Weekly
Monthly Jobs
```

---

## Monitor Async Jobs

```apex
AsyncApexJob
CronTrigger
```

---

## Avoid Excessive Chaining

Keep Queueable chains manageable.

---

# Interview Questions

### What is Asynchronous Apex?

Apex code that runs in the background without blocking the current transaction.

---

### Difference Between Future and Queueable?

| Future | Queueable |
|----------|----------|
| Primitive Params Only | Supports Complex Types |
| No Chaining | Supports Chaining |
| Limited Monitoring | Better Monitoring |

---

### When Should Batch Apex Be Used?

When processing large data volumes that exceed normal governor limits.

---

### What is the Default Batch Size?

```text
200 Records
```

---

### What is Database.Stateful?

Allows variables to retain values across batch executions.

---

### What is the Maximum Number of Records a Batch Can Process?

```text
50 Million Records
```

using `Database.getQueryLocator()`.

---

### What is Scheduled Apex?

Apex code that runs automatically at a specified time using a CRON expression.

---

### Which Async Type is Most Recommended Today?

```text
Queueable Apex
```

because it supports complex objects, chaining, monitoring, and callouts.

