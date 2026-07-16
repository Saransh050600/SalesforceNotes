
Advanced Apex patterns help build scalable, maintainable, testable, and enterprise-grade Salesforce applications.

These patterns are commonly used in:

- Large Salesforce implementations
- Enterprise applications
- Managed packages
- Complex integrations
- Multi-team development

---

# Topics Covered

1. Invocable Methods
2. Callable Interface
3. Reflection
4. Singleton Pattern
5. Factory Pattern
6. Service Layer
7. Unit of Work
8. Dependency Injection

---

# 1. Invocable Methods

Invocable Methods allow Flow and Process Builder to call Apex methods.

---

# Why Use Invocable Methods?

```text
Flow
   ↓
Invocable Apex
   ↓
Complex Business Logic
```

Useful when Flow cannot handle complex requirements.

---

# Syntax

```apex
@InvocableMethod
public static void methodName(
    List<InputType> inputs
){

}
```

---

# Simple Example

```apex
public class AccountInvoker {

    @InvocableMethod(
        label='Update Accounts'
    )
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
                'Updated by Flow';
        }

        update accounts;
    }
}
```

---

# Invocable Variable

Used for Flow input/output.

```apex
public class Request {

    @InvocableVariable
    public Id accountId;
}
```

---

# Flow-Friendly Example

```apex
public class AccountAction {

    public class Request {

        @InvocableVariable(
            required=true
        )
        public Id accountId;
    }

    @InvocableMethod
    public static void process(
        List<Request> requests
    ){

    }
}
```

---

# Invocable Method Rules

- Must be static
- Must be public/global
- One parameter only
- Parameter must be List
- Return type optional

---

# Use Cases

- Flow actions
- Reusable business logic
- Approval processing
- External integrations

---

# 2. Callable Interface

Allows dynamic invocation of methods.

Useful for:

- Frameworks
- Plugin architectures
- Generic processing

---

# Interface

```apex
public interface Callable {

    Object call(
        String action,
        Map<String,Object> args
    );
}
```

---

# Implementation

```apex
public class AccountCallable
implements Callable {

    public Object call(
        String action,
        Map<String,Object> args
    ){

        if(action == 'create'){

            return createAccount(
                args
            );
        }

        return null;
    }

    private Account createAccount(
        Map<String,Object> args
    ){

        Account acc =
            new Account(
                Name =
                (String)args.get('name')
            );

        insert acc;

        return acc;
    }
}
```

---

# Dynamic Execution

```apex
Callable service =
new AccountCallable();

service.call(
    'create',
    new Map<String,Object>{
        'name' => 'ABC Corp'
    }
);
```

---

# Use Cases

- Managed packages
- Dynamic plugins
- Framework design

---

# 3. Reflection

Reflection allows discovering and instantiating classes dynamically.

---

# Type Class

```apex
Type t =
Type.forName(
    'AccountService'
);
```

---

# Create Instance Dynamically

```apex
Object obj =
t.newInstance();
```

---

# Example

```apex
Type t =
Type.forName(
    'AccountProcessor'
);

Object instance =
t.newInstance();
```

---

# Reflection + Interface

```apex
public interface Processor {

    void execute();
}
```

---

```apex
public class AccountProcessor
implements Processor {

    public void execute(){

        System.debug(
            'Processing'
        );
    }
}
```

---

```apex
Processor p =
(Processor)
Type.forName(
    'AccountProcessor'
).newInstance();

p.execute();
```

---

# Benefits

- Loose coupling
- Dynamic configuration
- Plugin frameworks

---

# 4. Singleton Pattern

Ensures only one instance of a class exists.

---

# Why Singleton?

Use when:

```text
One Shared Instance
```

is required.

Examples:

- Cache Manager
- Configuration Manager
- Logging Service

---

# Singleton Implementation

```apex
public class ConfigManager {

    private static
    ConfigManager instance;

    private ConfigManager(){

    }

    public static
    ConfigManager getInstance(){

        if(instance == null){

            instance =
                new ConfigManager();
        }

        return instance;
    }
}
```

---

# Usage

```apex
ConfigManager config =
ConfigManager.getInstance();
```

---

# Characteristics

```text
Single Instance
Global Access
Lazy Loading
```

---

# 5. Factory Pattern

Factory Pattern creates objects without exposing creation logic.

---

# Problem

```apex
if(type == 'Account'){

}

if(type == 'Contact'){

}
```

Messy and hard to maintain.

---

# Factory Solution

---

## Interface

```apex
public interface Processor {

    void process();
}
```

---

## Implementations

```apex
public class AccountProcessor
implements Processor {

    public void process(){

    }
}
```

---

```apex
public class ContactProcessor
implements Processor {

    public void process(){

    }
}
```

---

## Factory

```apex
public class ProcessorFactory {

    public static Processor
    getProcessor(
        String type
    ){

        if(type == 'Account'){

            return
            new AccountProcessor();
        }

        if(type == 'Contact'){

            return
            new ContactProcessor();
        }

        return null;
    }
}
```

---

## Usage

```apex
Processor processor =
ProcessorFactory
.getProcessor(
    'Account'
);

processor.process();
```

---

# Benefits

- Extensible
- Loose coupling
- Cleaner code

---

# 6. Service Layer Pattern

Business logic should not be inside:

- Triggers
- Controllers
- Flows

---

# Layered Architecture

```text
Trigger
    ↓
Service Layer
    ↓
Domain Logic
    ↓
Database
```

---

# Service Class Example

```apex
public class AccountService {

    public static void
    createAccount(
        String name
    ){

        Account acc =
        new Account(
            Name = name
        );

        insert acc;
    }
}
```

---

# Trigger

```apex
trigger AccountTrigger
on Account(before insert){

    AccountService
        .processAccounts(
            Trigger.new
        );
}
```

---

# Benefits

- Reusable logic
- Easier testing
- Better separation

---

# Enterprise Structure

```text
AccountTrigger
        ↓
AccountService
        ↓
AccountDomain
        ↓
Database
```

---

# 7. Unit of Work Pattern

Manages multiple DML operations as one transaction.

Popularized by:

```text
fflib Apex Common
```

---

# Problem

```apex
insert accounts;
update contacts;
delete cases;
```

Multiple DML operations spread everywhere.

---

# Unit of Work Solution

```text
Register Changes
       ↓
Commit Once
```

---

# Concept

```apex
uow.registerNew(account);
uow.registerDirty(contact);
uow.registerDeleted(caseRecord);

uow.commitWork();
```

---

# Benefits

- Centralized DML
- Better transaction management
- Easier rollback strategy
- Enterprise scalability

---

# Example Concept

```apex
public class UnitOfWork {

    List<SObject> inserts =
        new List<SObject>();

    public void registerNew(
        SObject record
    ){

        inserts.add(record);
    }

    public void commitWork(){

        insert inserts;
    }
}
```

---

# Commonly Used With

```text
fflib
Enterprise Patterns
```

---

# 8. Dependency Injection (DI)

Dependency Injection provides dependencies from outside instead of creating them internally.

---

# Bad Practice

```apex
public class AccountService {

    private EmailService emailService =
        new EmailService();
}
```

Tightly coupled.

---

# Good Practice

```apex
public class AccountService {

    private IEmailService
        emailService;

    public AccountService(
        IEmailService emailService
    ){

        this.emailService =
            emailService;
    }
}
```

---

# Interface

```apex
public interface IEmailService {

    void sendEmail(
        String email
    );
}
```

---

# Implementation

```apex
public class EmailService
implements IEmailService {

    public void sendEmail(
        String email
    ){

    }
}
```

---

# Usage

```apex
AccountService service =
new AccountService(
    new EmailService()
);
```

---

# Testing Benefit

Mock implementation:

```apex
public class MockEmailService
implements IEmailService {

    public void sendEmail(
        String email
    ){

    }
}
```

---

# Dependency Injection Benefits

- Loose coupling
- Easy testing
- Mocking support
- Flexible architecture

---

# Pattern Comparison

| Pattern | Purpose |
|----------|----------|
| Singleton | Single instance |
| Factory | Object creation |
| Service Layer | Business logic layer |
| Unit of Work | Transaction management |
| Dependency Injection | Loose coupling |
| Reflection | Dynamic class loading |
| Callable | Dynamic execution |
| Invocable Method | Flow integration |

---

# Enterprise Architecture Example

```text
Flow
 ↓
Invocable Method
 ↓
Service Layer
 ↓
Factory
 ↓
Domain Service
 ↓
Unit Of Work
 ↓
Database
```

---

# Real-World Salesforce Stack

```text
Trigger
   ↓
Handler
   ↓
Service Layer
   ↓
Factory
   ↓
Repository / Selector
   ↓
Unit Of Work
   ↓
Database
```

---

# Interview Questions

### What is an Invocable Method?

An Apex method exposed to Flow using:

```apex
@InvocableMethod
```

---

### What is the Callable Interface?

An interface that enables dynamic method execution through:

```apex
call(
    String action,
    Map<String,Object> args
)
```

---

### What is Reflection in Apex?

Creating and loading classes dynamically using:

```apex
Type.forName()
```

---

### Why Use Singleton Pattern?

To ensure only one instance of a class exists during execution.

---

### What Problem Does Factory Pattern Solve?

Object creation logic is centralized and decoupled from consumers.

---

### Why Use a Service Layer?

To keep business logic separate from triggers, controllers, and UI layers.

---

### What is Unit of Work?

A pattern that groups DML operations and commits them together.

---

### What is Dependency Injection?

A design pattern where dependencies are supplied externally rather than instantiated internally.

---

### Which Design Patterns Are Most Common in Enterprise Salesforce Projects?

```text
Trigger Handler
Service Layer
Factory Pattern
Dependency Injection
Unit of Work
Singleton
```

These form the foundation of most scalable Salesforce enterprise architectures and frameworks such as the :contentReference[oaicite:0]{index=0}.

---

# Salesforce Enterprise Pattern Flow

```text
UI / API / Flow
        ↓
Invocable Method
        ↓
Trigger Handler
        ↓
Service Layer
        ↓
Factory
        ↓
Domain Layer
        ↓
Unit Of Work
        ↓
Database
```

This architecture provides:

- Scalability
- Testability
- Maintainability
- Reusability
- Enterprise-grade design

---
