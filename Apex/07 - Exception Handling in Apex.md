
Exception Handling is used to manage runtime errors gracefully and prevent application crashes.

Without exception handling, Apex code stops execution when an error occurs.

With exception handling, errors can be caught, logged, and handled properly.

---

# What is an Exception?

An exception is an unexpected error that occurs during program execution.

Examples:

- DML failures
- SOQL query errors
- Null Pointer exceptions
- List index errors
- Division by zero
- Callout failures

---

# Why Use Exception Handling?

Benefits:

- Prevents application crashes
- Provides meaningful error messages
- Improves debugging
- Maintains transaction control
- Enhances user experience

---

# Exception Handling Syntax

```apex
try {

    // Risky code

} catch(Exception ex) {

    // Error handling

}
```

---

# 1. Try-Catch

The `try` block contains code that may throw an exception.

The `catch` block handles the exception.

---

## Basic Example

```apex
try {

    Integer result = 10 / 0;

} catch(Exception ex) {

    System.debug(
        ex.getMessage()
    );
}
```

Output:

```text
Divide by 0
```

---

## DML Exception Example

```apex
try {

    Account acc =
        new Account();

    insert acc;

} catch(Exception ex) {

    System.debug(
        ex.getMessage()
    );
}
```

Output:

```text
Required fields are missing
```

---

## Multiple Catch Blocks

Specific exceptions should be caught before generic exceptions.

```apex
try {

    insert new Account();

} catch(DmlException ex) {

    System.debug(
        'DML Error'
    );

} catch(Exception ex) {

    System.debug(
        'General Error'
    );
}
```

---

# Exception Hierarchy

```text
Exception
│
├── DmlException
├── QueryException
├── NullPointerException
├── ListException
├── MathException
├── SObjectException
├── JSONException
├── CalloutException
└── StringException
```

---

# Common Apex Exceptions

---

## DmlException

Occurs during DML operations.

```apex
try {

    insert new Account();

}
catch(DmlException ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## QueryException

Occurs when SOQL returns unexpected results.

```apex
try {

    Account acc =
    [
        SELECT Id
        FROM Account
        WHERE Name='ABC'
    ];

}
catch(QueryException ex){

    System.debug(
        ex.getMessage()
    );
}
```

Example:

```text
List has no rows for assignment
```

---

## NullPointerException

Occurs when accessing a null object.

```apex
try {

    Account acc = null;

    System.debug(
        acc.Name
    );

}
catch(NullPointerException ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## ListException

Occurs due to invalid list operations.

```apex
try {

    List<String> names =
        new List<String>();

    System.debug(
        names[0]
    );

}
catch(ListException ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## MathException

Occurs during invalid math operations.

```apex
try {

    Integer result =
        10 / 0;

}
catch(MathException ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## SObjectException

Occurs when accessing fields not queried.

```apex
try {

    Account acc =
    [
        SELECT Id
        FROM Account
        LIMIT 1
    ];

    System.debug(
        acc.Name
    );

}
catch(SObjectException ex){

    System.debug(
        ex.getMessage()
    );
}
```

Output:

```text
SObject row was retrieved without querying field
```

---

# Exception Methods

Every exception inherits methods from the Exception class.

---

## getMessage()

Returns error message.

```apex
catch(Exception ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

## getTypeName()

Returns exception type.

```apex
catch(Exception ex){

    System.debug(
        ex.getTypeName()
    );
}
```

Output:

```text
System.DmlException
```

---

## getLineNumber()

Returns error line number.

```apex
catch(Exception ex){

    System.debug(
        ex.getLineNumber()
    );
}
```

---

## getStackTraceString()

Returns stack trace.

```apex
catch(Exception ex){

    System.debug(
        ex.getStackTraceString()
    );
}
```

---

## Example

```apex
catch(Exception ex){

    System.debug(
        ex.getMessage()
    );

    System.debug(
        ex.getLineNumber()
    );

    System.debug(
        ex.getTypeName()
    );
}
```

---

# Finally Block

Executes regardless of success or failure.

```apex
try {

    Integer x = 10;

}
catch(Exception ex){

    System.debug(
        ex.getMessage()
    );
}
finally {

    System.debug(
        'Always Executes'
    );
}
```

---

# 2. Custom Exceptions

Custom exceptions allow creation of business-specific error types.

---

# Create Custom Exception

```apex
public class InvalidAmountException
extends Exception {

}
```

---

# Throw Custom Exception

```apex
public class PaymentService {

    public static void processAmount(
        Decimal amount
    ){

        if(amount <= 0){

            throw new
            InvalidAmountException(
                'Amount must be greater than zero'
            );
        }
    }
}
```

---

# Handle Custom Exception

```apex
try {

    PaymentService.processAmount(
        -100
    );

}
catch(
    InvalidAmountException ex
){

    System.debug(
        ex.getMessage()
    );
}
```

Output:

```text
Amount must be greater than zero
```

---

# Business Validation Example

```apex
public class AgeValidator {

    public static void validate(
        Integer age
    ){

        if(age < 18){

            throw new Exception(
                'Age must be 18+'
            );
        }
    }
}
```

---

# 3. Throw Statements

The `throw` keyword is used to explicitly raise an exception.

---

# Syntax

```apex
throw new Exception(
    'Error Message'
);
```

---

# Example

```apex
if(totalAmount <= 0){

    throw new Exception(
        'Invalid Amount'
    );
}
```

---

# Throw Custom Exception

```apex
throw new InvalidAmountException(
    'Invalid Payment Amount'
);
```

---

# Rethrow Exception

Catch an exception and throw it again.

```apex
try {

    insert acc;

}
catch(Exception ex){

    System.debug(
        ex.getMessage()
    );

    throw ex;
}
```

---

# AuraHandledException

Used in Lightning Components and LWC to display user-friendly errors.

---

## Example

```apex
public static void validateAmount(
    Decimal amount
){

    if(amount <= 0){

        throw new AuraHandledException(
            'Amount must be positive'
        );
    }
}
```

---

# LWC Error Handling Example

Apex:

```apex
@AuraEnabled
public static void saveAccount(){

    throw new AuraHandledException(
        'Unable to save record'
    );
}
```

LWC:

```javascript
.catch(error => {

    console.log(
        error.body.message
    );
});
```

---

# Best Practices

## Catch Specific Exceptions

✅ Good

```apex
catch(DmlException ex)
```

❌ Bad

```apex
catch(Exception ex)
```

only.

---

## Log Detailed Errors

```apex
System.debug(
    ex.getStackTraceString()
);
```

---

## Avoid Empty Catch Blocks

❌

```apex
catch(Exception ex){

}
```

---

## Use Custom Exceptions

```apex
InvalidAmountException
```

instead of generic exceptions.

---

## Validate Before DML

```apex
if(acc.Name == null){

    throw new Exception(
        'Name Required'
    );
}
```

---

# Database Methods vs Exceptions

---

## DML Statement

```apex
insert accounts;
```

Throws:

```text
DmlException
```

if any record fails.

---

## Database Method

```apex
Database.insert(
    accounts,
    false
);
```

Returns:

```apex
Database.SaveResult
```

instead of throwing an exception for individual record failures.

---

# Real-World Example

```apex
public class AccountService {

    public static void createAccount(
        String name
    ){

        try {

            Account acc =
                new Account(
                    Name = name
                );

            insert acc;

        }
        catch(DmlException ex){

            System.debug(
                ex.getMessage()
            );

            throw new AuraHandledException(
                'Unable to create Account'
            );
        }
    }
}
```

---

# Interview Questions

### What is Exception Handling?

A mechanism to detect, catch, and handle runtime errors without crashing the application.

---

### Difference Between Try and Catch?

| Try | Catch |
|------|------|
| Contains risky code | Handles exception |
| Executes first | Executes if error occurs |

---

### What is a Custom Exception?

A user-defined exception class extending the Exception class.

```apex
public class MyException
extends Exception {}
```

---

### What is the Purpose of Throw?

Used to explicitly generate an exception.

```apex
throw new Exception(
    'Error'
);
```

---

### What is AuraHandledException?

A special exception used to send user-friendly error messages from Apex to Aura Components and LWC.

---

### Difference Between DmlException and Database.SaveResult?

| DmlException | SaveResult |
|-------------|------------|
| Throws exception | Returns status object |
| All-or-none behavior | Supports partial success |

---

### What is the Benefit of Finally Block?

It always executes regardless of whether an exception occurs.

---