# Python Classes

## Table of Contents

1. Introduction
2. What Is a Class?
3. Objects and Instances
4. Defining Your First Class
5. Instance Attributes
6. Methods and `self`
7. Constructors with `__init__`
8. Class Attributes vs Instance Attributes
9. String Representations: `__str__` and `__repr__`
10. Encapsulation
11. Properties and Validation
12. Class Methods
13. Static Methods
14. Inheritance
15. Method Overriding
16. Multiple Inheritance
17. Composition vs Inheritance
18. Abstract Base Classes
19. Duck Typing
20. Magic Methods (Dunder Methods)
21. Dataclasses
22. Slots and Memory Optimization
23. Metaclasses (Advanced)
24. Common Design Patterns with Classes
25. Best Practices
26. Common Mistakes
27. Real-World Example Project

---

# 1. Introduction

Python is an object-oriented programming (OOP) language.  
Classes are one of the core building blocks used to model data and behavior together.

Classes allow you to:

- Organize code
- Reuse functionality
- Model real-world systems
- Encapsulate state and behavior
- Build scalable applications

---

# 2. What Is a Class?

A class is a blueprint for creating objects.

Example analogy:

| Concept | Example |
|---|---|
| Class | Car blueprint |
| Object | Actual car |

---

# 3. Objects and Instances

An object is an instance of a class.

```python
class Dog:
    pass


dog1 = Dog()
dog2 = Dog()

print(type(dog1))
```

Output:

```python
<class '__main__.Dog'>
```

---

# 4. Defining Your First Class

```python
class User:
    pass
```

Creating instances:

```python
user1 = User()
user2 = User()
```

---

# 5. Instance Attributes

Attributes store object-specific data.

```python
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email


user = User("alice", "alice@example.com")

print(user.username)
print(user.email)
```

---

# 6. Methods and `self`

Methods define behavior.

```python
class User:
    def __init__(self, username):
        self.username = username

    def greet(self):
        return f"Hello, {self.username}!"
```

---

# 7. Constructors with `__init__`

`__init__` initializes new objects.

```python
class DatabaseConnection:
    def __init__(self, host, port):
        self.host = host
        self.port = port
```

---

# 8. Class Attributes vs Instance Attributes

## Instance Attributes

Unique per object.

```python
class User:
    def __init__(self, name):
        self.name = name
```

## Class Attributes

Shared across all instances.

```python
class User:
    platform = "Python Academy"

    def __init__(self, name):
        self.name = name
```

---

# 9. String Representations

## `__str__`

Human-readable representation.

```python
class User:
    def __init__(self, username):
        self.username = username

    def __str__(self):
        return f"User({self.username})"
```

---

# 10. Encapsulation

Encapsulation means hiding internal implementation details.

```python
class BankAccount:
    def __init__(self):
        self.__balance = 0

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance
```

---

# 11. Properties and Validation

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value
```

---

# 12. Class Methods

```python
class User:
    user_count = 0

    def __init__(self, name):
        self.name = name
        User.user_count += 1

    @classmethod
    def get_user_count(cls):
        return cls.user_count
```

---

# 13. Static Methods

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
```

---

# 14. Inheritance

```python
class Animal:
    def speak(self):
        print("Some sound")


class Dog(Animal):
    pass
```

---

# 15. Method Overriding

```python
class Dog(Animal):
    def speak(self):
        print("Woof")
```

---

# 16. Multiple Inheritance

```python
class Flyable:
    def fly(self):
        print("Flying")


class Swimmable:
    def swim(self):
        print("Swimming")


class Duck(Flyable, Swimmable):
    pass
```

---

# 17. Composition vs Inheritance

```python
class Engine:
    pass


class Car:
    def __init__(self):
        self.engine = Engine()
```

---

# 18. Abstract Base Classes

```python
from abc import ABC, abstractmethod


class PaymentProcessor(ABC):

    @abstractmethod
    def process_payment(self, amount):
        pass
```

---

# 19. Duck Typing

```python
class Duck:
    def speak(self):
        print("Quack")
```

---

# 20. Magic Methods (Dunder Methods)

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(
            self.x + other.x,
            self.y + other.y
        )
```

---

# 21. Dataclasses

```python
from dataclasses import dataclass


@dataclass
class User:
    username: str
    email: str
```

---

# 22. Slots and Memory Optimization

```python
class User:
    __slots__ = ("username", "email")
```

---

# 23. Metaclasses (Advanced)

```python
class MyMeta(type):
    pass
```

---

# 24. Common Design Patterns with Classes

## Factory Pattern

```python
class AnimalFactory:
    @staticmethod
    def create(animal_type):
        if animal_type == "dog":
            return Dog()
```

---

# 25. Best Practices

- Keep classes focused
- Prefer composition over inheritance
- Use dataclasses for data containers
- Use type hints
- Avoid excessive magic

---

# 26. Common Mistakes

## Mutable Default Arguments

Bad:

```python
class User:
    def __init__(self, tags=[]):
        self.tags = tags
```

Correct:

```python
class User:
    def __init__(self, tags=None):
        self.tags = tags or []
```

---

# 27. Real-World Example Project

```python
from dataclasses import dataclass
from abc import ABC, abstractmethod


@dataclass
class Product:
    name: str
    price: float


class PaymentProcessor(ABC):

    @abstractmethod
    def pay(self, amount: float):
        pass
```

