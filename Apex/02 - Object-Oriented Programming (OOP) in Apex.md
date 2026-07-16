
Object-Oriented Programming (OOP) is a programming paradigm that organizes code into classes and objects. Apex supports all major OOP concepts, making code reusable, maintainable, and scalable.

---

# 1. Classes & Objects

## Class

A class is a blueprint for creating objects.

```apex
public class AccountManager {

    public String accountName;

    public void displayName() {
        System.debug(accountName);
    }
}
```

---

## Object

An object is an instance of a class.

```apex
AccountManager accObj = new AccountManager();

accObj.accountName = 'ABC Corporation';
accObj.displayName();
```

Output:

```
ABC Corporation
```

---

## Real-World Example

```apex
public class Employee {

    public String name;
    public Integer age;

    public void displayDetails() {
        System.debug('Name: ' + name);
        System.debug('Age: ' + age);
    }
}
```

```apex
Employee emp = new Employee();

emp.name = 'John';
emp.age = 30;

emp.displayDetails();
```

---

# Constructor

A constructor is a special method that executes automatically when an object is created.

## Default Constructor

```apex
public class Student {

    public Student() {
        System.debug('Constructor Executed');
    }
}
```

```apex
Student s = new Student();
```

---

## Parameterized Constructor

```apex
public class Student {

    public String name;

    public Student(String studentName) {
        name = studentName;
    }
}
```

```apex
Student s = new Student('Rahul');

System.debug(s.name);
```

---

# this Keyword

Refers to the current object.

```apex
public class Employee {

    public String name;

    public Employee(String name) {
        this.name = name;
    }
}
```

---

# 2. Inheritance

Inheritance allows one class to acquire properties and methods of another class.

---

## Parent Class

```apex
public virtual class Animal {

    public void sound() {
        System.debug('Animal Sound');
    }
}
```

---

## Child Class

```apex
public class Dog extends Animal {

}
```

```apex
Dog d = new Dog();

d.sound();
```

Output:

```
Animal Sound
```

---

## Method Overriding

Child class provides its own implementation.

```apex
public virtual class Animal {

    public virtual void sound() {
        System.debug('Animal Sound');
    }
}
```

```apex
public class Dog extends Animal {

    public override void sound() {
        System.debug('Bark');
    }
}
```

```apex
Dog d = new Dog();
d.sound();
```

Output:

```
Bark
```

---

# virtual vs override

| Keyword | Purpose |
|-----------|-----------|
| virtual | Allows method/class to be overridden |
| override | Replaces parent implementation |

```apex
public virtual class ParentClass {

    public virtual void display() {
        System.debug('Parent');
    }
}
```

```apex
public class ChildClass extends ParentClass {

    public override void display() {
        System.debug('Child');
    }
}
```

---

# 3. Polymorphism

Polymorphism means "many forms."

A parent reference can point to child objects.

---

## Runtime Polymorphism

```apex
public virtual class Vehicle {

    public virtual void startEngine() {
        System.debug('Vehicle Engine');
    }
}
```

```apex
public class Car extends Vehicle {

    public override void startEngine() {
        System.debug('Car Engine');
    }
}
```

```apex
Vehicle v = new Car();

v.startEngine();
```

Output:

```
Car Engine
```

---

## Benefits

- Flexible code
- Loose coupling
- Easier maintenance
- Better scalability

---

# 4. Abstraction

Abstraction hides implementation details and shows only essential functionality.

In Apex, abstraction is achieved using:

- Abstract Classes
- Interfaces

---

# Abstract Class

Cannot be instantiated directly.

```apex
public abstract class Payment {

    public abstract void processPayment();
}
```

---

## Child Class

```apex
public class CreditCardPayment extends Payment {

    public override void processPayment() {
        System.debug('Credit Card Payment Processed');
    }
}
```

```apex
Payment p = new CreditCardPayment();

p.processPayment();
```

---

## Abstract Class Rules

- Can have abstract methods
- Can have concrete methods
- Cannot create object directly
- Child class must implement abstract methods

---

### Example

```apex
public abstract class Shape {

    public abstract Decimal area();

    public void display() {
        System.debug('Shape Area');
    }
}
```

```apex
public class Circle extends Shape {

    public override Decimal area() {
        return 100;
    }
}
```

---

# 5. Encapsulation

Encapsulation means hiding data and exposing it through controlled methods.

---

## Private Variables

```apex
public class Employee {

    private Decimal salary;
}
```

External classes cannot access salary directly.

---

## Getter and Setter

```apex
public class Employee {

    private Decimal salary;

    public void setSalary(Decimal amount) {
        salary = amount;
    }

    public Decimal getSalary() {
        return salary;
    }
}
```

```apex
Employee emp = new Employee();

emp.setSalary(50000);

System.debug(emp.getSalary());
```

---

## Property Syntax

Apex provides automatic getters and setters.

```apex
public class Employee {

    public Decimal salary {get; set;}
}
```

```apex
Employee emp = new Employee();

emp.salary = 50000;

System.debug(emp.salary);
```

---

## Read-Only Property

```apex
public String companyName {get; private set;}
```

---

## Why Encapsulation?

- Data security
- Better validation
- Controlled access
- Easier maintenance

---

# 6. Interfaces

An interface defines a contract that classes must implement.

---

## Interface Definition

```apex
public interface PaymentProcessor {

    void processPayment();
}
```

---

## Implementation

```apex
public class PayPalProcessor implements PaymentProcessor {

    public void processPayment() {
        System.debug('PayPal Payment');
    }
}
```

```apex
PaymentProcessor p = new PayPalProcessor();

p.processPayment();
```

Output:

```
PayPal Payment
```

---

# Multiple Interfaces

Apex supports implementing multiple interfaces.

```apex
public interface Printable {
    void print();
}
```

```apex
public interface Exportable {
    void export();
}
```

```apex
public class ReportGenerator
    implements Printable, Exportable {

    public void print() {
        System.debug('Printing');
    }

    public void export() {
        System.debug('Exporting');
    }
}
```

---

# Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|----------|----------|----------|
| Methods with body | ✅ Yes | ❌ No |
| Abstract methods | ✅ Yes | ✅ Yes |
| Variables | ✅ Yes | ❌ No |
| Constructor | ✅ Yes | ❌ No |
| Multiple inheritance | ❌ No | ✅ Yes |

---

# Common Salesforce OOP Examples

## Batch Apex

Implements interface:

```apex
Database.Batchable<sObject>
```

```apex
global class AccountBatch
implements Database.Batchable<sObject> {

    global Database.QueryLocator start(
        Database.BatchableContext bc
    ) {
        return Database.getQueryLocator(
            'SELECT Id FROM Account'
        );
    }

    global void execute(
        Database.BatchableContext bc,
        List<Account> scope
    ) {

    }

    global void finish(
        Database.BatchableContext bc
    ) {

    }
}
```

---

## Schedulable Apex

```apex
global class DailyJob
implements Schedulable {

    global void execute(
        SchedulableContext sc
    ) {
        System.debug('Scheduled Job');
    }
}
```

---

# OOP Interview Questions

### What are the four pillars of OOP?

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

---

### Difference between Interface and Abstract Class?

| Interface | Abstract Class |
|------------|------------|
| Only method declarations | Can contain implementations |
| Multiple interfaces allowed | Single inheritance only |
| No constructor | Constructor allowed |

---

### Why use Polymorphism?

Allows parent references to handle multiple child objects, making code flexible and extensible.

---

### What is Method Overriding?

Providing a different implementation of a parent class method in the child class.

```apex
public override void display() {
    System.debug('Child Implementation');
}
```

---

### What is Encapsulation?

Wrapping data and methods together while restricting direct access to internal data.

---

### What is the difference between Class and Object?

| Class | Object |
|---------|---------|
| Blueprint | Instance |
| Defines structure | Holds actual data |
| Created once | Can create many |
