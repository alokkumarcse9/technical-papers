# 🧠 OOPs & SOLID in Python — Cheatsheet

> A practical revision guide with concise explanations and Python code samples.

---

# Part 1: OOPs in Python

## 1. What is OOP?

**Object-Oriented Programming (OOP)** is a programming approach where we organize code around **objects**.

An object contains:

- **Attributes** → Data / State
- **Methods** → Behavior / Actions

```text
Student
├── Attributes
│   ├── name
│   └── marks
│
└── Methods
    ├── add_marks()
    └── calculate_average()
```

---

# 2. Class

A **class** is a blueprint for creating objects.

```python
class Student:
    pass
```

Think:

```text
Class = Blueprint
Object = Real thing created from blueprint
```

---

# 3. Object

An **object** is an instance of a class.

```python
class Student:
    pass


student1 = Student()
student2 = Student()
```

```text
Student  → Class
student1 → Object
student2 → Object
```

---

# 4. Constructor — `__init__()`

`__init__()` is used to initialize an object's data when the object is created.

```python
class Student:

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks


student = Student("Alok", 90)

print(student.name)
print(student.marks)
```

### Flow

```text
Student("Alok", 90)
        ↓
__init__(self, "Alok", 90)
        ↓
self.name = "Alok"
self.marks = 90
```

---

# 5. `self`

`self` refers to the **current object**.

```python
class Student:

    def __init__(self, name):
        self.name = name
```

When:

```python
s1 = Student("Alok")
```

`self` refers to `s1`.

When:

```python
s2 = Student("Rahul")
```

`self` refers to `s2`.

Therefore:

```text
s1.name → "Alok"
s2.name → "Rahul"
```

---

# 6. Attributes / Instance Variables

Instance variables belong to individual objects.

```python
class Student:

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
```

```python
s1 = Student("Alok", 90)
s2 = Student("Rahul", 80)

print(s1.name)
# Alok

print(s2.name)
# Rahul
```

Each object has its own values.

---

# 7. Instance Methods

Instance methods work with object data and normally receive `self`.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")
```

```python
student = Student("Alok")

student.introduce()
# My name is Alok
```

---

# 8. Class Variables

A class variable belongs to the class and can be shared by instances.

```python
class Student:

    school = "ABC School"

    def __init__(self, name):
        self.name = name
```

```python
s1 = Student("Alok")
s2 = Student("Rahul")

print(s1.school)
print(s2.school)
```

Both objects can access `school`.

---

# 9. Class Method

A class method receives the class as `cls`.

Use `@classmethod`.

```python
class Student:

    school = "ABC School"

    @classmethod
    def change_school(cls, name):
        cls.school = name
```

Usage:

```python
Student.change_school("XYZ School")

print(Student.school)
```

---

# 10. Static Method

A static method does not automatically receive `self` or `cls`.

Use `@staticmethod`.

```python
class Calculator:

    @staticmethod
    def add(a, b):
        return a + b
```

Usage:

```python
result = Calculator.add(10, 20)

print(result)
# 30
```

---

# 11. Method Types

| Method | First Parameter | Purpose |
|---|---|---|
| Instance Method | `self` | Works with object data |
| Class Method | `cls` | Works with class-level data |
| Static Method | None | Utility/helper logic |

---

# 12. Four Pillars of OOP

```text
                 OOP
                  │
       ┌──────────┼──────────┐
       │          │          │
 Encapsulation Inheritance Polymorphism
                  │
             Abstraction
```

---

## 12.1 Encapsulation

Encapsulation means keeping data and related methods together and controlling access to internal data.

```python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        self._balance += amount

    def get_balance(self):
        return self._balance
```

Usage:

```python
account = BankAccount(1000)

account.deposit(500)

print(account.get_balance())
# 1500
```

### Python Naming Convention

```text
name        → Public
_name       → Internal / protected convention
__name      → Name mangling / strongly non-public convention
```

---

## 12.2 Inheritance

Inheritance allows a child class to reuse or extend a parent class.

```python
class Animal:

    def speak(self):
        print("Animal speaks")


class Dog(Animal):

    def bark(self):
        print("Dog barks")
```

Usage:

```python
dog = Dog()

dog.speak()
dog.bark()
```

```text
Animal
   ↑
  Dog
```

---

## 12.3 Polymorphism

Polymorphism means the same interface can produce different behavior depending on the object.

```python
class Dog:

    def speak(self):
        print("Woof")


class Cat:

    def speak(self):
        print("Meow")
```

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

Output:

```text
Woof
Meow
```

---

## 12.4 Abstraction

Abstraction means exposing essential behavior while hiding implementation details.

Python commonly uses the `abc` module for abstract base classes.

```python
from abc import ABC, abstractmethod


class Payment(ABC):

    @abstractmethod
    def pay(self, amount):
        pass
```

Implementation:

```python
class CreditCardPayment(Payment):

    def pay(self, amount):
        print(f"Paid {amount} using credit card")
```

---

# 13. Method Overriding

A child class can provide its own implementation of a parent method.

```python
class Animal:

    def speak(self):
        print("Animal speaks")


class Dog(Animal):

    def speak(self):
        print("Dog barks")
```

```python
dog = Dog()

dog.speak()
# Dog barks
```

---

# 14. `super()`

`super()` is used to access functionality from a parent class.

```python
class Animal:

    def __init__(self, name):
        self.name = name


class Dog(Animal):

    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
```

```python
dog = Dog("Bruno", "Labrador")

print(dog.name)
print(dog.breed)
```

---

# 15. Composition

Composition means building a class using objects of other classes.

```python
class Engine:

    def start(self):
        print("Engine started")


class Car:

    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car started")
```

```python
car = Car()
car.start()
```

Relationship:

```text
Car
 │
 └── Engine
```

---

# OOP Quick Revision

```text
Class
  ↓
Blueprint

Object
  ↓
Instance of a class

__init__()
  ↓
Initialize object

self
  ↓
Current object

Four Pillars
  ↓
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

# Part 2: SOLID Principles

## 1. What is SOLID?

SOLID is a set of five object-oriented design principles that help make software:

- Maintainable
- Flexible
- Testable
- Scalable
- Reusable
- Easier to modify

```text
S → Single Responsibility Principle
O → Open/Closed Principle
L → Liskov Substitution Principle
I → Interface Segregation Principle
D → Dependency Inversion Principle
```

---

# 2. SOLID at a Glance

| Principle | Full Form | Main Idea |
|---|---|---|
| **S** | Single Responsibility Principle | One class should have one responsibility |
| **O** | Open/Closed Principle | Open for extension, closed for modification |
| **L** | Liskov Substitution Principle | Child objects should properly substitute parent objects |
| **I** | Interface Segregation Principle | Prefer small, specific interfaces |
| **D** | Dependency Inversion Principle | Depend on abstractions, not concrete implementations |

---

# 3. 🔹 SRP — Single Responsibility Principle

> **A class should have only one reason to change.**

A class should ideally focus on one responsibility.

## ❌ Bad Example

```python
class Employee:

    def calculate_salary(self):
        pass

    def save_to_database(self):
        pass

    def send_email(self):
        pass
```

This class has three responsibilities:

```text
Employee
├── Calculate Salary
├── Save Database
└── Send Email
```

## ✅ Better Example

```python
class SalaryCalculator:

    def calculate(self, employee):
        pass


class EmployeeRepository:

    def save(self, employee):
        pass


class EmailService:

    def send(self, employee):
        pass
```

Now each class has one primary responsibility.

---

# 4. 🔹 OCP — Open/Closed Principle

> **Software entities should be open for extension but closed for modification.**

We should be able to add new behavior without repeatedly modifying stable existing code.

## ❌ Bad Example

```python
class Payment:

    def pay(self, payment_type):

        if payment_type == "card":
            print("Paying using card")

        elif payment_type == "upi":
            print("Paying using UPI")

        elif payment_type == "cash":
            print("Paying using cash")
```

Adding a new payment type requires modifying `Payment`.

## ✅ Better Example

```python
class Payment:

    def pay(self):
        pass


class CardPayment(Payment):

    def pay(self):
        print("Paying using card")


class UPIPayment(Payment):

    def pay(self):
        print("Paying using UPI")


class CashPayment(Payment):

    def pay(self):
        print("Paying using cash")
```

Now a new payment implementation can be added without changing the existing implementations.

---

# 5. 🔹 LSP — Liskov Substitution Principle

> **A child object should be usable wherever its parent object is expected without breaking the program.**

## ❌ Bad Example

```python
class Bird:

    def fly(self):
        print("Flying")


class Penguin(Bird):

    def fly(self):
        raise Exception("Penguins cannot fly")
```

The `Penguin` class cannot satisfy the behavior promised by this `Bird` abstraction.

## ✅ Better Example

```python
class Bird:
    pass


class FlyingBird(Bird):

    def fly(self):
        print("Flying")


class Sparrow(FlyingBird):

    def fly(self):
        print("Sparrow is flying")


class Penguin(Bird):

    def swim(self):
        print("Penguin is swimming")
```

The abstraction no longer requires every bird to fly.

---

# 6. 🔹 ISP — Interface Segregation Principle

> **Clients should not be forced to depend on methods they do not need.**

Python does not have Java-style interfaces as a built-in language feature, but the principle can be implemented using abstract base classes or protocols.

## ❌ Bad Example

```python
class Machine:

    def print(self):
        pass

    def scan(self):
        pass

    def fax(self):
        pass
```

A simple printer may not need scanning or faxing.

## ✅ Better Example

```python
from abc import ABC, abstractmethod


class Printer(ABC):

    @abstractmethod
    def print(self):
        pass


class Scanner(ABC):

    @abstractmethod
    def scan(self):
        pass


class SimplePrinter(Printer):

    def print(self):
        print("Printing...")
```

`SimplePrinter` only depends on the functionality it needs.

---

# 7. 🔹 DIP — Dependency Inversion Principle

> **High-level modules should not depend directly on low-level modules. Both should depend on abstractions.**

## ❌ Bad Example

```python
class MySQLDatabase:

    def save(self, data):
        print("Saving to MySQL")


class UserService:

    def __init__(self):
        self.database = MySQLDatabase()

    def save_user(self, user):
        self.database.save(user)
```

`UserService` is tightly coupled to `MySQLDatabase`.

## ✅ Better Example

```python
from abc import ABC, abstractmethod


class Database(ABC):

    @abstractmethod
    def save(self, data):
        pass


class MySQLDatabase(Database):

    def save(self, data):
        print("Saving to MySQL")


class UserService:

    def __init__(self, database):
        self.database = database

    def save_user(self, user):
        self.database.save(user)
```

Usage:

```python
database = MySQLDatabase()

service = UserService(database)

service.save_user("Alok")
```

Now the dependency is injected from outside.

```text
UserService
     │
     ↓
 Database (Abstraction)
     ↑
     │
MySQLDatabase
```

---

# 8. SOLID Practical Example

Imagine an e-commerce application:

```text
Customer
   ↓
Order
   ↓
Payment
   ↓
Invoice
   ↓
Notification
   ↓
Database
```

A poor design might put everything into one class:

```python
class OrderService:

    def create_order(self):
        pass

    def process_payment(self):
        pass

    def generate_invoice(self):
        pass

    def send_notification(self):
        pass

    def save_to_database(self):
        pass
```

This creates multiple responsibilities and tight coupling.

A better design separates responsibilities:

```python
class OrderService:
    pass


class PaymentService:
    pass


class InvoiceService:
    pass


class NotificationService:
    pass


class OrderRepository:
    pass
```

This makes the system easier to:

- Test
- Extend
- Replace
- Maintain
- Debug

---

# 🧠 SOLID Mental Model

```text
                         SOLID
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        S                  O                  L
        │                  │                  │
  One Responsibility   Extend Easily     Safe Substitution
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                    ┌──────┴──────┐
                    │             │
                    I             D
                    │             │
             Small Interfaces   Abstractions
```

---

# ⚡ SOLID Quick Revision

```text
S → Single Responsibility
    One class → One primary responsibility

O → Open/Closed
    Extend behavior → Avoid unnecessary modification

L → Liskov Substitution
    Child → Should properly substitute parent

I → Interface Segregation
    Small, focused interfaces

D → Dependency Inversion
    Depend on abstractions → Not concrete implementations
```

---

# OOP vs SOLID

| OOP | SOLID |
|---|---|
| Programming paradigm | Design principles |
| Classes and objects | Better class design |
| Encapsulation | SRP |
| Inheritance | LSP / OCP |
| Polymorphism | OCP / LSP |
| Abstraction | DIP / ISP |
| Focuses on building objects | Focuses on designing maintainable systems |

---

# Final Revision Map

```text
                    PYTHON OOP
                        │
          ┌─────────────┼─────────────┐
          │             │             │
       Classes       Objects       Methods
          │             │             │
          └─────────────┼─────────────┘
                        │
                 Four Pillars
                        │
       ┌────────────────┼────────────────┐
       │                │                │
 Encapsulation      Inheritance     Polymorphism
                        │
                   Abstraction
                        │
                        ↓
                      SOLID
                        │
       ┌────────────────┼────────────────┐
       │                │                │
       S                O                L
       │                │                │
 Responsibility      Extension      Substitution
       │                │                │
       └────────────────┼────────────────┘
                        │
                    ┌───┴───┐
                    I       D
                    │       │
                Interfaces  Dependencies
```

