
Apex Testing ensures your code works correctly and meets Salesforce deployment requirements.

Salesforce requires Apex tests because custom code runs in a shared multi-tenant environment and must be validated before deployment.

---

# Why Apex Testing?

Benefits:

- Validates business logic
- Prevents regressions
- Improves code quality
- Enables safe deployments
- Verifies governor limit behavior

---

# Deployment Requirement

Minimum code coverage required:

```text
75% Overall Apex Coverage
```

Requirements:

```text
Overall Coverage >= 75%
No Trigger Can Have 0% Coverage
```

---

# Apex Testing Components

1. Test Classes
2. Test Methods
3. Assertions
4. Test Data Factory
5. Test Coverage

---

# 1. Test Classes

A Test Class contains test methods used to validate Apex code.

---

## Basic Test Class

```apex
@isTest
private class AccountServiceTest {

}
```

---

## Benefits of @isTest

```apex
@isTest
```

- Excluded from org code size limit
- Used only during testing
- Cannot be invoked by users

---

## Test Class Naming Convention

```text
AccountService
AccountServiceTest
```

```text
ContactTriggerHandler
ContactTriggerHandlerTest
```

---

# Testing a Class

Production Code:

```apex
public class Calculator {

    public static Integer add(
        Integer a,
        Integer b
    ){
        return a + b;
    }
}
```

Test Class:

```apex
@isTest
private class CalculatorTest {

    @isTest
    static void testAdd(){

        Integer result =
            Calculator.add(10, 20);

        System.assertEquals(
            30,
            result
        );
    }
}
```

---

# 2. Test Methods

A Test Method validates a specific behavior.

---

## Syntax

```apex
@isTest
static void testMethodName(){

}
```

---

## Example

```apex
@isTest
static void testAccountInsert(){

    Account acc =
        new Account(
            Name='Test Account'
        );

    insert acc;

    System.assertNotEquals(
        null,
        acc.Id
    );
}
```

---

# Test Method Rules

- Must be static
- Takes no parameters
- Returns no value
- Executes independently

---

# Multiple Test Methods

```apex
@isTest
private class CalculatorTest {

    @isTest
    static void testAdd(){

    }

    @isTest
    static void testSubtract(){

    }

    @isTest
    static void testMultiply(){

    }
}
```

---

# Test Isolation

Tests do not see production data by default.

```text
SeeAllData = false
```

(default behavior)

---

## Bad Practice

```apex
@isTest(SeeAllData=true)
```

Avoid unless absolutely necessary.

---

# Test Setup Method

Creates reusable test data.

---

## Example

```apex
@isTest
private class AccountTest {

    @testSetup
    static void createData(){

        List<Account> accounts =
            new List<Account>();

        for(Integer i=0;i<5;i++){

            accounts.add(
                new Account(
                    Name='Account ' + i
                )
            );
        }

        insert accounts;
    }
}
```

---

## Use Setup Data

```apex
@isTest
static void testAccounts(){

    List<Account> accounts =
    [
        SELECT Id, Name
        FROM Account
    ];

    System.assertEquals(
        5,
        accounts.size()
    );
}
```

---

# Test.startTest() and Test.stopTest()

Resets governor limits and executes asynchronous jobs.

---

## Example

```apex
@isTest
static void testFutureMethod(){

    Test.startTest();

    MyFutureClass.process();

    Test.stopTest();
}
```

---

# Why Use StartTest/StopTest?

- Resets governor limits
- Executes async Apex
- Provides realistic testing

---

# Governor Limit Example

```apex
Test.startTest();

System.debug(
    Limits.getQueries()
);

Test.stopTest();
```

---

# 3. Assertions

Assertions verify expected outcomes.

Without assertions, tests may pass even when logic is wrong.

---

# System.assert()

Checks a condition.

```apex
System.assert(
    10 > 5
);
```

---

## Example

```apex
System.assert(
    acc.Id != null
);
```

---

# System.assertEquals()

Most commonly used assertion.

```apex
System.assertEquals(
    expectedValue,
    actualValue
);
```

---

## Example

```apex
System.assertEquals(
    5,
    accountList.size()
);
```

---

# System.assertNotEquals()

```apex
System.assertNotEquals(
    null,
    acc.Id
);
```

---

# Assertion Example

```apex
Account acc =
new Account(
    Name='Test'
);

insert acc;

System.assertNotEquals(
    null,
    acc.Id
);

System.assertEquals(
    'Test',
    acc.Name
);
```

---

# Testing Trigger Example

Trigger:

```apex
trigger AccountTrigger
on Account(before insert){

    for(Account acc : Trigger.new){

        acc.Description =
            'Created';
    }
}
```

Test:

```apex
@isTest
private class AccountTriggerTest {

    @isTest
    static void testTrigger(){

        Account acc =
            new Account(
                Name='Test'
            );

        insert acc;

        Account result =
        [
            SELECT Description
            FROM Account
            WHERE Id=:acc.Id
        ];

        System.assertEquals(
            'Created',
            result.Description
        );
    }
}
```

---

# 4. Test Data Factory

A reusable class that generates test data.

Used to avoid duplicate test code.

---

# Basic Factory

```apex
@isTest
public class TestDataFactory {

    public static Account
    createAccount(){

        Account acc =
            new Account(
                Name='Test Account'
            );

        insert acc;

        return acc;
    }
}
```

---

# Using Factory

```apex
@isTest
static void testAccount(){

    Account acc =
        TestDataFactory
        .createAccount();

    System.assertNotEquals(
        null,
        acc.Id
    );
}
```

---

# Advanced Factory

```apex
@isTest
public class TestDataFactory {

    public static List<Account>
    createAccounts(Integer count){

        List<Account> accounts =
            new List<Account>();

        for(Integer i=0;i<count;i++){

            accounts.add(
                new Account(
                    Name='Account ' + i
                )
            );
        }

        insert accounts;

        return accounts;
    }
}
```

---

# Factory with Related Records

```apex
@isTest
public class TestDataFactory {

    public static Contact
    createContact(){

        Account acc =
            new Account(
                Name='Parent'
            );

        insert acc;

        Contact con =
            new Contact(
                LastName='Smith',
                AccountId=acc.Id
            );

        insert con;

        return con;
    }
}
```

---

# Benefits of Test Data Factory

- Reusability
- Cleaner test classes
- Consistent test data
- Easier maintenance

---

# 5. Test Coverage

Coverage measures how much code is executed by tests.

---

# Coverage Formula

:contentReference[oaicite:0]{index=0}

Example:

```text
80 Lines Executed
100 Total Lines
```

Coverage:

```text
80%
```

---

# Salesforce Requirement

```text
75% Minimum Overall Coverage
```

---

# Check Coverage

```text
Developer Console
→ Test
→ New Run
```

or

```text
Setup
→ Apex Test Execution
```

---

# Coverage Example

```apex
public class DiscountService {

    public static Decimal
    getDiscount(Integer score){

        if(score > 80){

            return 20;
        }

        return 10;
    }
}
```

Test:

```apex
@isTest
private class DiscountServiceTest {

    @isTest
    static void testDiscount(){

        Decimal result =
        DiscountService
        .getDiscount(90);

        System.assertEquals(
            20,
            result
        );
    }
}
```

---

# Improve Coverage

Test all branches.

```apex
@isTest
static void testHighScore(){

    System.assertEquals(
        20,
        DiscountService
        .getDiscount(90)
    );
}
```

```apex
@isTest
static void testLowScore(){

    System.assertEquals(
        10,
        DiscountService
        .getDiscount(50)
    );
}
```

---

# Code Coverage vs Quality

High coverage does not guarantee quality.

❌ Bad Test

```apex
MyClass.execute();
```

No assertions.

---

✅ Good Test

```apex
System.assertEquals(
    expected,
    actual
);
```

Verifies behavior.

---

# Testing Best Practices

## Use SeeAllData=false

```apex
@isTest
```

Default and recommended.

---

## Test Positive and Negative Scenarios

```apex
Valid Data
Invalid Data
Boundary Values
```

---

## Use Assertions

Every test should verify outcomes.

---

## Use Test Data Factory

Avoid duplicated test setup code.

---

## Use Test.startTest()

```apex
Test.startTest();
Test.stopTest();
```

Especially for:

- Future Methods
- Queueables
- Batch Apex
- Scheduled Apex

---

## Test Bulk Operations

```apex
200 Records
```

to validate trigger bulkification.

---

# Real-World Trigger Test

```apex
@isTest
private class ContactTriggerTest {

    @isTest
    static void testBulkInsert(){

        Account acc =
        new Account(
            Name='Parent'
        );

        insert acc;

        List<Contact> contacts =
        new List<Contact>();

        for(Integer i=0;i<200;i++){

            contacts.add(
                new Contact(
                    LastName='Contact ' + i,
                    AccountId=acc.Id
                )
            );
        }

        Test.startTest();

        insert contacts;

        Test.stopTest();

        System.assertEquals(
            200,
            contacts.size()
        );
    }
}
```

---

# Interview Questions

### Why Are Test Classes Required?

Salesforce requires tests to validate code quality and ensure deployments are safe.

---

### What Is the Minimum Deployment Coverage?

```text
75%
```

overall Apex code coverage.

---

### What Does @isTest Do?

Marks a class or method as a test and excludes it from organization code size limits.

---

### Difference Between @testSetup and TestDataFactory?

| @testSetup | TestDataFactory |
|------------|------------|
| Shared data within one test class | Reusable across multiple test classes |
| Runs once per test class | Called whenever needed |

---

### Why Use Test.startTest() and Test.stopTest()?

To reset governor limits and execute asynchronous code.

---

### Difference Between assert() and assertEquals()?

| Method | Purpose |
|----------|----------|
| assert() | Verify condition |
| assertEquals() | Verify expected vs actual |
| assertNotEquals() | Verify values differ |

---

### Does 100% Coverage Mean Bug-Free Code?

No. Coverage only measures execution, not correctness. Assertions and scenario testing are equally important.
