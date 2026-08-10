# Python Cheatsheet — Objects and Object-Oriented Programming (OOP)

> **OOP = Object-Oriented Programming**
>
> OOP is a programming style where we organize code using **objects and classes**.

---

# 1. What is a Class?

A **class** is a blueprint/template for creating objects.

```python
class Student:
    pass
```

Think:

```text
Class
  ↓
Blueprint

Student
  ↓
Object
```

A class describes:

* What data an object has
* What actions an object can perform

---

# 2. What is an Object?

An **object** is an instance of a class.

```python
class Student:
    pass

student1 = Student()
student2 = Student()
```

Here:

```text
Student     → Class
student1    → Object
student2    → Object
```

Each object is created independently.

---

# 3. Basic Class and Object

```python
class Student:

    def study(self):
        print("Student is studying")


student1 = Student()

student1.study()
```

Output:

```text
Student is studying
```

### Understand the flow

```text
class Student
      ↓
Student()
      ↓
student1 object
      ↓
student1.study()
      ↓
study() executes
```

---

# 4. `self`

`self` represents the **current object**.

```python
class Student:

    def study(self):
        print("Studying")


student1 = Student()

student1.study()
```

Python internally treats:

```python
student1.study()
```

approximately as:

```python
Student.study(student1)
```

So:

```python
def study(self):
```

means:

> This method belongs to the current object.

---

# 5. Instance Variables

Instance variables store data belonging to a particular object.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Create objects:

```python
student1 = Student("Alok", 22)
student2 = Student("Rahul", 23)
```

Now:

```text
student1
 ├── name = "Alok"
 └── age = 22

student2
 ├── name = "Rahul"
 └── age = 23
```

Access them:

```python
print(student1.name)
print(student1.age)
```

---

# 6. `__init__()`

`__init__()` is the constructor-like initialization method used when an object is created.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

When you write:

```python
student = Student("Alok", 22)
```

Python calls:

```python
__init__()
```

to initialize the object.

---

# 7. Constructor Flow

```python
class Student:

    def __init__(self, name):
        self.name = name


student = Student("Alok")
```

Understand it like:

```text
Student("Alok")
      ↓
Create object
      ↓
__init__(self, "Alok")
      ↓
self.name = "Alok"
```

---

# 8. Instance Methods

A method that works with a particular object is an **instance method**.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print("My name is", self.name)
```

Usage:

```python
student = Student("Alok")

student.introduce()
```

Output:

```text
My name is Alok
```

---

# 9. Class Variables

A class variable is shared by objects of the class.

```python
class Student:

    school = "ABC School"

    def __init__(self, name):
        self.name = name
```

Objects:

```python
student1 = Student("Alok")
student2 = Student("Rahul")
```

Both can access:

```python
print(student1.school)
print(student2.school)
```

Output:

```text
ABC School
ABC School
```

---

# 10. Instance Variable vs Class Variable

```python
class Student:

    school = "ABC School"       # Class variable

    def __init__(self, name):
        self.name = name        # Instance variable
```

| Type              | Example     | Belongs to |
| ----------------- | ----------- | ---------- |
| Instance variable | `self.name` | Object     |
| Class variable    | `school`    | Class      |

Think:

```text
Class variable
     ↓
Shared

Instance variable
     ↓
Individual
```

---

# 11. Class Method

A class method works with the **class** rather than a particular object.

Use:

```python
@classmethod
```

Example:

```python
class Student:

    school = "ABC School"

    @classmethod
    def change_school(cls, new_school):
        cls.school = new_school
```

Call it:

```python
Student.change_school("XYZ School")
```

---

# 12. `cls`

`cls` represents the **class itself**.

```python
class Student:

    school = "ABC School"

    @classmethod
    def show_school(cls):
        print(cls.school)
```

Call:

```python
Student.show_school()
```

Think:

```text
self → current object

cls  → current class
```

---

# 13. Static Method

A static method does not need `self` or `cls`.

Use:

```python
@staticmethod
```

Example:

```python
class Calculator:

    @staticmethod
    def add(a, b):
        return a + b
```

Call:

```python
result = Calculator.add(10, 20)

print(result)
```

Output:

```text
30
```

---

# 14. Instance vs Class vs Static Method

```python
class Example:

    def instance_method(self):
        pass

    @classmethod
    def class_method(cls):
        pass

    @staticmethod
    def static_method():
        pass
```

| Method          | First parameter | Works with          |
| --------------- | --------------- | ------------------- |
| Instance method | `self`          | Object              |
| Class method    | `cls`           | Class               |
| Static method   | None            | Independent utility |

### Easy memory trick

```text
self → object
cls  → class
static → neither
```

---

# 15. Four Pillars of OOP

The four major OOP principles are:

```text
1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism
```

Remember:

```text
E A I P
```

---

# 16. Encapsulation

Encapsulation means keeping data and related methods together inside a class and controlling access to internal data.

Example:

```python
class BankAccount:

    def __init__(self, balance):
        self.__balance = balance

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance
```

Usage:

```python
account = BankAccount(1000)

account.deposit(500)

print(account.get_balance())
```

Output:

```text
1500
```

---

# 17. Private Attribute

Double underscore:

```python
self.__balance
```

is used for name mangling/private-style access.

```python
class BankAccount:

    def __init__(self):
        self.__balance = 1000
```

You normally access it through methods:

```python
account.get_balance()
```

instead of directly accessing:

```python
account.__balance
```

---

# 18. Encapsulation Pattern

Common pattern:

```python
class Account:

    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance

    def deposit(self, amount):
        self.__balance += amount
```

Think:

```text
Private data
     ↓
__balance
     ↓
Controlled methods
     ↓
get_balance()
deposit()
```

---

# 19. Abstraction

Abstraction means exposing only the necessary details while hiding implementation details.

Python provides:

```python
from abc import ABC, abstractmethod
```

Example:

```python
from abc import ABC, abstractmethod


class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass
```

`Animal` defines what subclasses **must provide**.

---

# 20. Abstract Class

```python
from abc import ABC, abstractmethod


class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass
```

You generally cannot create:

```python
animal = Animal()
```

because `speak()` is abstract.

---

# 21. Implementing an Abstract Method

```python
class Dog(Animal):

    def speak(self):
        print("Bark")
```

Another class:

```python
class Cat(Animal):

    def speak(self):
        print("Meow")
```

Objects:

```python
dog = Dog()
cat = Cat()

dog.speak()
cat.speak()
```

Output:

```text
Bark
Meow
```

---

# 22. `@abstractmethod`

When you see:

```python
@abstractmethod
def speak(self):
    pass
```

Think:

> "Every concrete child class must implement this method."

---

# 23. Inheritance

Inheritance allows one class to inherit functionality from another class.

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):

    def bark(self):
        print("Barking")
```

Now:

```python
dog = Dog()

dog.eat()
dog.bark()
```

Output:

```text
Eating
Barking
```

Because:

```text
Animal
  ↑
Dog
```

`Dog` inherits from `Animal`.

---

# 24. Parent and Child Class

```python
class Animal:
    pass


class Dog(Animal):
    pass
```

Terminology:

```text
Animal → Parent / Base / Superclass

Dog → Child / Derived / Subclass
```

---

# 25. `super()`

`super()` is used to access the parent class implementation.

```python
class Animal:

    def __init__(self, name):
        self.name = name


class Dog(Animal):

    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
```

Create:

```python
dog = Dog("Tommy", "Labrador")
```

Now:

```python
print(dog.name)
print(dog.breed)
```

Output:

```text
Tommy
Labrador
```

---

# 26. Why Use `super()`?

Without repeating parent initialization:

```python
super().__init__(name)
```

means:

> Call the parent class's `__init__()`.

Flow:

```text
Dog.__init__()
      ↓
super().__init__()
      ↓
Animal.__init__()
      ↓
self.name = name
```

---

# 27. Polymorphism

Polymorphism means:

> Same interface/method call, different behavior.

Example:

```python
class Dog:

    def speak(self):
        print("Bark")


class Cat:

    def speak(self):
        print("Meow")
```

Now:

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

Output:

```text
Bark
Meow
```

The same:

```python
animal.speak()
```

produces different behavior.

---

# 28. Polymorphism with Inheritance

```python
from abc import ABC, abstractmethod


class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass


class Dog(Animal):

    def speak(self):
        print("Bark")


class Cat(Animal):

    def speak(self):
        print("Meow")
```

Then:

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

This combines:

```text
Abstraction
     +
Inheritance
     +
Polymorphism
```

---

# 29. Method Overriding

When a child class provides its own implementation of a parent method.

```python
class Animal:

    def speak(self):
        print("Animal sound")


class Dog(Animal):

    def speak(self):
        print("Bark")
```

Here `Dog` overrides:

```python
speak()
```

---

# 30. `isinstance()`

Checks whether an object belongs to a class.

```python
class Dog:
    pass


dog = Dog()

print(isinstance(dog, Dog))
```

Output:

```text
True
```

With inheritance:

```python
class Animal:
    pass


class Dog(Animal):
    pass


dog = Dog()

print(isinstance(dog, Animal))
```

Output:

```text
True
```

---

# 31. `issubclass()`

Checks whether a class inherits from another class.

```python
class Animal:
    pass


class Dog(Animal):
    pass


print(issubclass(Dog, Animal))
```

Output:

```text
True
```

---

# 32. Object Attributes

You can access object attributes using `.`.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Alok", 22)

print(student.name)
print(student.age)
```

Syntax:

```python
object.attribute
```

---

# 33. Object Method Calls

Call methods using:

```python
object.method()
```

Example:

```python
student.introduce()
```

Think:

```text
student
   ↓
object
   ↓
.
   ↓
introduce()
   ↓
method call
```

---

# 34. `__str__()`

Controls the human-readable string representation of an object.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Student: {self.name}"
```

Now:

```python
student = Student("Alok")

print(student)
```

Output:

```text
Student: Alok
```

---

# 35. Common Dunder Methods

Dunder means:

```text
Double Underscore
```

Examples:

```python
__init__
__str__
__len__
__eq__
```

These are also called **magic methods** or **special methods**.

---

# 36. `__len__()`

Allows an object to work with `len()`.

```python
class Team:

    def __init__(self, players):
        self.players = players

    def __len__(self):
        return len(self.players)
```

Usage:

```python
team = Team(["A", "B", "C"])

print(len(team))
```

Output:

```text
3
```

---

# 37. `__eq__()`

Defines how objects are compared using `==`.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name
```

Usage:

```python
student1 = Student("Alok")
student2 = Student("Alok")

print(student1 == student2)
```

Output:

```text
True
```

---

# 38. Composition

Composition means one object **contains** another object.

Example:

```python
class Engine:

    def start(self):
        print("Engine started")


class Car:

    def __init__(self):
        self.engine = Engine()
```

Now:

```python
car = Car()

car.engine.start()
```

Relationship:

```text
Car
 ↓
contains
 ↓
Engine
```

Think:

> **Has-a relationship**

Car **has an** Engine.

---

# 39. Inheritance vs Composition

### Inheritance

```python
class Dog(Animal):
    pass
```

Relationship:

```text
Dog IS-A Animal
```

### Composition

```python
class Car:

    def __init__(self):
        self.engine = Engine()
```

Relationship:

```text
Car HAS-A Engine
```

### Remember

```text
Inheritance  → IS-A

Composition  → HAS-A
```

---

# 40. Multiple Inheritance

Python allows a class to inherit from multiple classes.

```python
class A:

    def method_a(self):
        print("A")


class B:

    def method_b(self):
        print("B")


class C(A, B):
    pass
```

Now:

```python
obj = C()

obj.method_a()
obj.method_b()
```

---

# 41. Method Resolution Order — MRO

Python uses MRO to determine which method should be called.

```python
class A:
    pass


class B(A):
    pass


class C(B):
    pass
```

Check:

```python
print(C.mro())
```

Conceptually:

```text
C
↓
B
↓
A
↓
object
```

---

# 42. `object`

All normal Python classes ultimately inherit from:

```python
object
```

For example:

```python
class Student:
    pass
```

is conceptually related to:

```python
class Student(object):
    pass
```

---

# 43. Property

`@property` allows a method to be accessed like an attribute.

```python
class Student:

    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name
```

Usage:

```python
student = Student("Alok")

print(student.name)
```

Notice:

```python
student.name
```

not:

```python
student.name()
```

---

# 44. Setter with `@property`

```python
class Student:

    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        self._name = value
```

Usage:

```python
student = Student("Alok")

student.name = "Rahul"

print(student.name)
```

Output:

```text
Rahul
```

---

# 45. OOP Code Reading Strategy

When you see an OOP program, read it in this order:

```text
1. Find the classes
        ↓
2. Find inheritance
        ↓
3. Find __init__()
        ↓
4. Find instance variables
        ↓
5. Find methods
        ↓
6. Find @staticmethod
        ↓
7. Find @classmethod
        ↓
8. Find @abstractmethod
        ↓
9. Find objects
        ↓
10. Follow method calls
```

---

# 46. How to Identify `self`

When you see:

```python
def something(self):
```

Ask:

> Is this method working with object data?

If yes, look for:

```python
self.name
self.age
self.balance
self.value
```

Example:

```python
class Student:

    def __init__(self, name):
        self.name = name

    def show_name(self):
        print(self.name)
```

Here:

```text
self.name
   ↓
Current object's name
```

---

# 47. How to Identify `cls`

When you see:

```python
@classmethod
def something(cls):
```

look for:

```python
cls.variable
```

Example:

```python
class Student:

    school = "ABC"

    @classmethod
    def show_school(cls):
        print(cls.school)
```

Think:

```text
cls → class
```

---

# 48. How to Identify Static Method

When you see:

```python
@staticmethod
```

there is no:

```python
self
```

and no:

```python
cls
```

Example:

```python
class Calculator:

    @staticmethod
    def add(a, b):
        return a + b
```

Think:

> This method doesn't need object or class state.

---

# 49. How to Identify Abstraction

Look for:

```python
from abc import ABC, abstractmethod
```

Then:

```python
class Animal(ABC):
```

and:

```python
@abstractmethod
```

Example:

```python
class Animal(ABC):

    @abstractmethod
    def speak(self):
        pass
```

Think:

```text
abstractmethod
      ↓
Child class must implement it
```

---

# 50. How to Identify Inheritance

Look inside:

```python
class Child(Parent):
```

Example:

```python
class Dog(Animal):
```

The parentheses indicate inheritance.

```text
Dog
 ↓
Animal
```

---

# 51. How to Identify Polymorphism

Look for the same method being implemented differently.

```python
class Dog:

    def speak(self):
        print("Bark")


class Cat:

    def speak(self):
        print("Meow")
```

Then:

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

This is a classic polymorphism pattern.

---

# 52. How to Identify Encapsulation

Look for data being controlled inside a class.

Common signs:

```python
self.__balance
```

and methods such as:

```python
get_balance()
deposit()
withdraw()
```

Think:

```text
Data
 ↓
Protected/controlled
 ↓
Methods
```

---

# 53. Four Pillars — Quick Table

| Pillar        | Meaning                            | Common Python Signs      |
| ------------- | ---------------------------------- | ------------------------ |
| Encapsulation | Control access to data             | `__variable`, methods    |
| Abstraction   | Hide implementation details        | `ABC`, `@abstractmethod` |
| Inheritance   | Reuse parent functionality         | `class Dog(Animal)`      |
| Polymorphism  | Same interface, different behavior | Method overriding        |

---

# 54. Complete OOP Example

```python
from abc import ABC, abstractmethod


class Animal(ABC):

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def speak(self):
        pass

    def introduce(self):
        print(f"My name is {self.name}")


class Dog(Animal):

    def speak(self):
        print("Bark")


class Cat(Animal):

    def speak(self):
        print("Meow")


animals = [
    Dog("Tommy"),
    Cat("Kitty")
]

for animal in animals:
    animal.introduce()
    animal.speak()
```

Output:

```text
My name is Tommy
Bark
My name is Kitty
Meow
```

### What is happening?

```text
Animal
  │
  ├── ABC
  ├── __init__()
  ├── name
  ├── abstract speak()
  └── introduce()
       │
       ├───────────────┐
       ↓               ↓
     Dog              Cat
       │               │
   speak()           speak()
      ↓                 ↓
    Bark              Meow
```

This single example contains:

```text
Abstraction
Inheritance
Polymorphism
Encapsulation
Constructor
Instance variables
Instance methods
Method overriding
Objects
```

---

# 55. OOP Quick Reference

```python
# Class
class Student:
    pass


# Object
student = Student()


# Constructor
def __init__(self):
    pass


# Instance variable
self.name = "Alok"


# Instance method
def study(self):
    pass


# Class variable
school = "ABC"


# Class method
@classmethod
def method(cls):
    pass


# Static method
@staticmethod
def method():
    pass


# Inheritance
class Dog(Animal):
    pass


# Parent method
super().method()


# Abstract class
class Animal(ABC):
    pass


# Abstract method
@abstractmethod
def speak(self):
    pass


# Property
@property
def name(self):
    return self._name
```

---

# ⭐ Final OOP Memory Map

```text
                    OOP
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Class        Object       Methods
        │            │
        │            └── Instance
        │
        ├── Variables
        │     ├── Instance
        │     └── Class
        │
        └── Methods
              ├── Instance → self
              ├── Class → cls
              └── Static → neither


              FOUR PILLARS
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓           ↓
 Encapsulation  Abstraction  Inheritance  Polymorphism
       │           │           │           │
      __x       ABC/abstract  Parent      Same method
      methods   method        → Child     → different behavior
```

# 🚀 Must-Memorize OOP Toolkit

```text
CLASS
    class Student:

OBJECT
    student = Student()

CONSTRUCTOR
    __init__()

OBJECT DATA
    self.name

INSTANCE METHOD
    def method(self):

CLASS METHOD
    @classmethod
    def method(cls):

STATIC METHOD
    @staticmethod
    def method():

INHERITANCE
    class Dog(Animal):

PARENT
    super()

ABSTRACTION
    ABC
    @abstractmethod

ENCAPSULATION
    __variable

POLYMORPHISM
    same method
    different implementation

COMPOSITION
    object contains another object

PROPERTY
    @property

TYPE CHECKING
    isinstance()
    issubclass()
```

# 🧠 The Most Important Mental Model

Whenever you see OOP code, ask these questions:

```text
1. What is the CLASS?
       ↓
2. What OBJECTS are created?
       ↓
3. What does __init__ store?
       ↓
4. What is stored in self?
       ↓
5. Which methods belong to objects?
       ↓
6. Is there @classmethod?
       ↓
7. Is there @staticmethod?
       ↓
8. Is there inheritance?
       ↓
9. Is there @abstractmethod?
       ↓
10. Which method is actually called?
```

If you can answer these 10 questions, you will be able to read most beginner/intermediate Python OOP code much more confidently.
