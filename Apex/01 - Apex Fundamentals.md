
Apex is Salesforce's strongly typed, object-oriented programming language used to execute flow and transaction control statements on the Salesforce platform.

---

# 1. Syntax

Every Apex statement ends with a semicolon (`;`).

```apex
public class HelloWorld {
    public static void printMessage() {
        System.debug('Hello World');
    }
}
```

### Key Rules

- Case-insensitive language
- Statements end with `;`
- Classes are defined using `class`
- Methods are defined inside classes
- Curly braces `{}` define code blocks

```apex
public class SampleClass {
    public static void executeMethod() {
        System.debug('Executing Apex');
    }
}
```

---

# 2. Variables

Variables store data values.

### Declaration

```apex
String name;
Integer age;
```

### Initialization

```apex
String name = 'John';
Integer age = 25;
```

### Multiple Variables

```apex
Integer x = 10;
Integer y = 20;
Integer z = x + y;
```

### Final Variable

Cannot be reassigned after initialization.

```apex
final Integer MAX_LIMIT = 100;
```

---

# 3. Data Types

## Primitive Data Types

### Integer

Stores whole numbers.

```apex
Integer count = 100;
```

### Long

Stores large integers.

```apex
Long population = 9000000000L;
```

### Double

Stores decimal values.

```apex
Double salary = 50000.75;
```

### Decimal

High-precision decimal numbers.

```apex
Decimal amount = 9999.99;
```

### Boolean

Stores true or false.

```apex
Boolean isActive = true;
```

### String

Stores text.

```apex
String company = 'Salesforce';
```

### Date

Stores date only.

```apex
Date today = Date.today();
```

### Datetime

Stores date and time.

```apex
Datetime nowTime = Datetime.now();
```

### Time

Stores time only.

```apex
Time currentTime = Time.newInstance(14, 30, 0, 0);
```

### ID

Stores Salesforce Record IDs.

```apex
Id accountId = '001XXXXXXXXXXXXXXX';
```

---

## Collection Data Types

### List

Ordered collection.

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};
```

### Set

Unique values only.

```apex
Set<String> cities = new Set<String>{
    'Delhi',
    'Mumbai',
    'Delhi'
};
```

### Map

Key-value pair collection.

```apex
Map<Id, Account> accountMap = new Map<Id, Account>();
```

---

## sObject Data Type

Represents Salesforce records.

```apex
Account acc = new Account();
acc.Name = 'ABC Corp';
```

---

# 4. Operators

## Arithmetic Operators

| Operator | Description |
|----------|-------------|
| + | Addition |
| - | Subtraction |
| * | Multiplication |
| / | Division |
| % | Modulus |

```apex
Integer a = 10;
Integer b = 5;

System.debug(a + b);
System.debug(a - b);
System.debug(a * b);
System.debug(a / b);
```

---

## Assignment Operators

| Operator | Description |
|----------|-------------|
| = | Assign |
| += | Add and assign |
| -= | Subtract and assign |
| *= | Multiply and assign |
| /= | Divide and assign |

```apex
Integer score = 10;

score += 5;
score -= 2;
```

---

## Comparison Operators

| Operator | Description |
|----------|-------------|
| == | Equal |
| != | Not Equal |
| > | Greater Than |
| < | Less Than |
| >= | Greater Than Equal |
| <= | Less Than Equal |

```apex
Integer age = 18;

if(age >= 18) {
    System.debug('Adult');
}
```

---

## Logical Operators

| Operator | Description |
|----------|-------------|
| && | AND |
| \|\| | OR |
| ! | NOT |

```apex
Boolean isAdmin = true;
Boolean isActive = true;

if(isAdmin && isActive) {
    System.debug('Access Granted');
}
```

---

## Increment / Decrement Operators

```apex
Integer count = 5;

count++;
count--;
```

---

# 5. Control Statements

Control statements determine the flow of execution.

---

## If Statement

```apex
Integer age = 20;

if(age >= 18) {
    System.debug('Eligible');
}
```

---

## If-Else Statement

```apex
Integer marks = 40;

if(marks >= 35) {
    System.debug('Pass');
}
else {
    System.debug('Fail');
}
```

---

## If-Else If Ladder

```apex
Integer score = 85;

if(score >= 90) {
    System.debug('Grade A');
}
else if(score >= 75) {
    System.debug('Grade B');
}
else {
    System.debug('Grade C');
}
```

---

## Switch Statement

```apex
String status = 'Open';

switch on status {
    when 'Open' {
        System.debug('Case Open');
    }
    when 'Closed' {
        System.debug('Case Closed');
    }
    when else {
        System.debug('Unknown Status');
    }
}
```

---

## For Loop

```apex
for(Integer i = 1; i <= 5; i++) {
    System.debug(i);
}
```

Output:

```
1
2
3
4
5
```

---

## Enhanced For Loop

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};

for(String name : names) {
    System.debug(name);
}
```

---

## While Loop

```apex
Integer count = 1;

while(count <= 5) {
    System.debug(count);
    count++;
}
```

---

## Do-While Loop

```apex
Integer count = 1;

do {
    System.debug(count);
    count++;
}
while(count <= 5);
```

---

## Break Statement

Stops loop execution.

```apex
for(Integer i = 1; i <= 10; i++) {

    if(i == 5) {
        break;
    }

    System.debug(i);
}
```

Output:

```
1
2
3
4
```

---

## Continue Statement

Skips current iteration.

```apex
for(Integer i = 1; i <= 5; i++) {

    if(i == 3) {
        continue;
    }

    System.debug(i);
}
```

Output:

```
1
2
4
5
```

---

# Interview Questions

### What is Apex?

Apex is Salesforce's strongly typed, object-oriented programming language used for custom business logic.

### Difference between List, Set, and Map?

| Collection | Description |
|------------|-------------|
| List | Ordered, allows duplicates |
| Set | Unordered, unique values |
| Map | Key-value pairs |

### Difference between Integer, Double, and Decimal?

| Type | Usage |
|--------|--------|
| Integer | Whole numbers |
| Double | Floating-point values |
| Decimal | High-precision financial calculations |

### Which control statement is best for multiple conditions?

`switch` statement when evaluating a single variable against multiple values.

### What is an sObject?

An sObject represents a Salesforce record such as Account, Contact, Opportunity, etc.

```apex
Account acc = new Account(Name='ABC Corp');
```
