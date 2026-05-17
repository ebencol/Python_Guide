# Python OOP

## Table of Contents

1. Introduction to Object-Oriented Programming
2. Classes and Objects
3. Instance Variables vs Class Variables
4. Methods in Python
5. Constructors and Destructors
6. Encapsulation
7. Inheritance
8. Polymorphism
9. Abstraction
10. Magic (Dunder) Methods
11. Composition vs Inheritance
12. Dataclasses
13. Properties and Descriptors
14. Abstract Base Classes (ABC)
15. Multiple Inheritance and MRO
16. Metaclasses
17. Design Patterns in Python
18. SOLID Principles
19. OOP Best Practices
20. Real-World Use Cases
21. Performance Considerations
22. Common Pitfalls
23. Final Project Example
24. Conclusion

---

# 1. Introduction to Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm centered around objects, which bundle together data and behavior.

Python is a multi-paradigm language, but OOP is one of its strongest features.

## Why Use OOP?

- Code reusability
- Better organization
- Scalability
- Encapsulation of logic
- Easier maintenance
- Modeling real-world entities

## Core OOP Concepts

| Concept | Description |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| Encapsulation | Hiding internal implementation |
| Inheritance | Reusing behavior from another class |
| Polymorphism | Same interface, different behavior |
| Abstraction | Exposing only essential functionality |

---

# 2. Classes and Objects

## Creating a Class

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def start(self):
        print(f"{self.brand} {self.model} is starting")
```

## Creating Objects

```python
car1 = Car("Tesla", "Model S")
car2 = Car("BMW", "i8")

car1.start()
```

## Output

```python
Tesla Model S is starting
```

## Use Case

Classes are useful when modeling entities such as:

- Users
- Products
- Vehicles
- API clients
- Database records
- Game characters

---

# 3. Instance Variables vs Class Variables

## Instance Variables

Belong to individual objects.

```python
class User:
    def __init__(self, username):
        self.username = username
```

## Class Variables

Shared across all instances.

```python
class User:
    user_count = 0

    def __init__(self, username):
        self.username = username
        User.user_count += 1
```

## Example

```python
u1 = User("alice")
u2 = User("bob")

print(User.user_count)
```

## Output

```python
2
```

## Use Case

Class variables are useful for:

- Global counters
- Shared configuration
- Caching
- Constants

---

# 4. Methods in Python

## Instance Methods

```python
class Dog:
    def bark(self):
        print("Woof")
```

## Class Methods

Operate on the class itself.

```python
class Database:
    connection_count = 0

    @classmethod
    def increment_connections(cls):
        cls.connection_count += 1
```

## Static Methods

Utility functions related to a class.

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
```

## Use Cases

| Method Type | Best Use |
|---|---|
| Instance Method | Access instance data |
| Class Method | Factory methods |
| Static Method | Utility/helper logic |

---

# 5. Constructors and Destructors

## Constructor

```python
class Employee:
    def __init__(self, name):
        self.name = name
```

## Destructor

```python
class FileHandler:
    def __del__(self):
        print("Cleaning up resources")
```

## Important Notes

Python uses garbage collection.

Destructors are:

- Non-deterministic
- Rarely needed
- Not ideal for resource management

Instead, prefer:

```python
with open("data.txt") as file:
    content = file.read()
```

---

# 6. Encapsulation

Encapsulation means hiding internal details and exposing controlled access.

## Public Attributes

```python
class Person:
    def __init__(self, name):
        self.name = name
```

## Protected Attributes

Convention only.

```python
class Person:
    def __init__(self):
        self._age = 30
```

## Private Attributes

Name mangling.

```python
class BankAccount:
    def __init__(self):
        self.__balance = 0

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance
```

## Use Case

Useful in:

- Banking systems
- Security-sensitive applications
- APIs
- Internal state management

---

# 7. Inheritance

Inheritance allows one class to reuse another class.

## Basic Inheritance

```python
class Animal:
    def speak(self):
        print("Animal sound")

class Cat(Animal):
    def speak(self):
        print("Meow")
```

## Using super()

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

class ElectricCar(Vehicle):
    def __init__(self, brand, battery):
        super().__init__(brand)
        self.battery = battery
```

## Use Cases

- GUI frameworks
- Web frameworks
- Game engines
- API client hierarchies

---

# 8. Polymorphism

Polymorphism allows different objects to share the same interface.

## Example

```python
class Bird:
    def move(self):
        print("Flying")

class Fish:
    def move(self):
        print("Swimming")

for animal in [Bird(), Fish()]:
    animal.move()
```

## Output

```python
Flying
Swimming
```

## Duck Typing

Python emphasizes behavior over type.

```python
class Duck:
    def quack(self):
        print("Quack")

class Person:
    def quack(self):
        print("I can imitate a duck")
```

---

# 9. Abstraction

Abstraction hides implementation details.

## Using Abstract Base Classes

```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass

class PayPalProcessor(PaymentProcessor):
    def process_payment(self, amount):
        print(f"Processing ${amount} via PayPal")
```

## Use Cases

- Payment systems
- Plugin architectures
- Framework APIs
- Device drivers

---

# 10. Magic (Dunder) Methods

Magic methods customize object behavior.

## Common Dunder Methods

| Method | Purpose |
|---|---|
| __init__ | Constructor |
| __str__ | String representation |
| __repr__ | Developer representation |
| __len__ | Length |
| __eq__ | Equality |
| __add__ | Addition |
| __iter__ | Iteration |

## Example

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __str__(self):
        return f"Vector({self.x}, {self.y})"
```

## Example Usage

```python
v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1 + v2)
```

## Output

```python
Vector(4, 6)
```

---

# 11. Composition vs Inheritance

## Composition

Composition means building objects using other objects.

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
```

## Why Composition Often Wins

Composition:

- Reduces tight coupling
- Improves flexibility
- Simplifies testing
- Avoids deep inheritance chains

## Real-World Use Cases

- Service architectures
- Dependency injection
- Microservices
- Game systems

---

# 12. Dataclasses

Dataclasses reduce boilerplate code.

## Basic Example

```python
from dataclasses import dataclass

@dataclass
class Product:
    name: str
    price: float
```

## Example Usage

```python
p = Product("Laptop", 1499.99)
print(p)
```

## Advanced Dataclass

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class ShoppingCart:
    items: List[str] = field(default_factory=list)
```

## Benefits

- Automatic __init__
- Automatic __repr__
- Automatic comparisons
- Cleaner code

---

# 13. Properties and Descriptors

## Properties

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Invalid temperature")
        self._celsius = value
```

## Use Case

Properties are useful for:

- Validation
- Lazy loading
- Computed values
- Backward compatibility

## Descriptors

Advanced reusable attribute management.

```python
class PositiveNumber:
    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, instance, owner):
        return instance.__dict__.get(self.name)

    def __set__(self, instance, value):
        if value < 0:
            raise ValueError("Must be positive")
        instance.__dict__[self.name] = value

class Product:
    price = PositiveNumber()
```

---

# 14. Abstract Base Classes (ABC)

ABCs define required interfaces.

```python
from abc import ABC, abstractmethod

class Storage(ABC):
    @abstractmethod
    def save(self, data):
        pass
```

## Implementations

```python
class FileStorage(Storage):
    def save(self, data):
        print("Saving to file")

class CloudStorage(Storage):
    def save(self, data):
        print("Saving to cloud")
```

## Use Cases

- Framework development
- Plugin systems
- API standardization

---

# 15. Multiple Inheritance and MRO

Python supports multiple inheritance.

## Example

```python
class A:
    def hello(self):
        print("A")

class B:
    def hello(self):
        print("B")

class C(A, B):
    pass

c = C()
c.hello()
```

## Output

```python
A
```

## Method Resolution Order (MRO)

```python
print(C.__mro__)
```

## Best Practices

- Avoid deep multiple inheritance
- Prefer composition when possible
- Use mixins carefully

## Mixin Example

```python
class LoggerMixin:
    def log(self, message):
        print(f"LOG: {message}")

class Service(LoggerMixin):
    pass
```

---

# 16. Metaclasses

Metaclasses define how classes themselves are created.

## Basic Example

```python
class Meta(type):
    def __new__(cls, name, bases, attrs):
        attrs['version'] = '1.0'
        return super().__new__(cls, name, bases, attrs)

class App(metaclass=Meta):
    pass

print(App.version)
```

## Use Cases

- ORMs
- Framework internals
- Plugin registration
- Validation systems
- Singleton enforcement

## Important

Metaclasses are advanced.

Avoid them unless:

- Simpler solutions fail
- Framework-level behavior is needed

---

# 17. Design Patterns in Python

## Singleton Pattern

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

## Factory Pattern

```python
class Dog:
    pass

class Cat:
    pass

class AnimalFactory:
    @staticmethod
    def create(animal_type):
        if animal_type == "dog":
            return Dog()
        return Cat()
```

## Observer Pattern

```python
class Subject:
    def __init__(self):
        self.observers = []

    def subscribe(self, observer):
        self.observers.append(observer)

    def notify(self, message):
        for observer in self.observers:
            observer.update(message)
```

## Strategy Pattern

```python
class PaymentStrategy:
    def pay(self, amount):
        pass

class CreditCardPayment(PaymentStrategy):
    def pay(self, amount):
        print(f"Paid ${amount} using credit card")
```

---

# 18. SOLID Principles

## S — Single Responsibility Principle

A class should have only one reason to change.

## O — Open/Closed Principle

Software should be open for extension but closed for modification.

## L — Liskov Substitution Principle

Derived classes should be replaceable with base classes.

## I — Interface Segregation Principle

Clients should not depend on unused interfaces.

## D — Dependency Inversion Principle

Depend on abstractions, not concrete implementations.

## Example

```python
from abc import ABC, abstractmethod

class NotificationService(ABC):
    @abstractmethod
    def send(self, message):
        pass

class EmailService(NotificationService):
    def send(self, message):
        print(f"Email: {message}")
```

---

# 19. OOP Best Practices

## Keep Classes Small

Large classes become difficult to maintain.

## Prefer Composition Over Inheritance

Composition is often more flexible.

## Use Meaningful Names

Bad:

```python
class Data:
    pass
```

Good:

```python
class CustomerRepository:
    pass
```

## Avoid God Objects

One class should not manage everything.

## Write Unit Tests

OOP structures work best with testing.

---

# 20. Real-World Use Cases

## Web Development

```python
class UserService:
    def authenticate(self, username, password):
        pass
```

### Frameworks

- entity["software","Django","Python web framework"]
- entity["software","FastAPI","Python web framework"]
- entity["software","Flask","Python web framework"]

## Game Development

```python
class Player:
    def move(self):
        pass

class Enemy:
    def attack(self):
        pass
```

## Data Engineering

```python
class DataPipeline:
    def extract(self):
        pass

    def transform(self):
        pass

    def load(self):
        pass
```

## Machine Learning

```python
class ModelTrainer:
    def train(self):
        pass
```

### Libraries

- entity["software","TensorFlow","Machine learning framework"]
- entity["software","PyTorch","Machine learning framework"]
- entity["software","scikit-learn","Machine learning library"]

---

# 21. Performance Considerations

## Using __slots__

```python
class Point:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y
```

## Benefits

- Reduced memory usage
- Faster attribute access

## Tradeoffs

- Less flexibility
- No dynamic attributes

## Avoid Excessive Abstraction

Too many layers can:

- Reduce readability
- Increase complexity
- Hurt performance

---

# 22. Common Pitfalls

## Mutable Default Arguments

Bad:

```python
class Cart:
    def __init__(self, items=[]):
        self.items = items
```

Good:

```python
class Cart:
    def __init__(self, items=None):
        self.items = items or []
```

## Tight Coupling

Avoid hard dependencies.

Use dependency injection.

## Overusing Inheritance

Deep hierarchies are difficult to maintain.

## Ignoring Encapsulation

Directly exposing internal state can create bugs.

---

# 23. Final Project Example

## E-Commerce System

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass

@dataclass
class Product:
    name: str
    price: float

class PaymentProcessor(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class StripeProcessor(PaymentProcessor):
    def pay(self, amount):
        print(f"Paid ${amount} using Stripe")

class ShoppingCart:
    def __init__(self):
        self.products = []

    def add_product(self, product):
        self.products.append(product)

    def total(self):
        return sum(product.price for product in self.products)

class OrderService:
    def __init__(self, payment_processor):
        self.payment_processor = payment_processor

    def checkout(self, cart):
        total = cart.total()
        self.payment_processor.pay(total)
```

## Example Usage

```python
cart = ShoppingCart()
cart.add_product(Product("Keyboard", 99.99))
cart.add_product(Product("Mouse", 49.99))

processor = StripeProcessor()
order_service = OrderService(processor)

order_service.checkout(cart)
```

## Concepts Demonstrated

- Encapsulation
- Abstraction
- Dependency injection
- Polymorphism
- Dataclasses
- Composition
- SOLID principles

---

# 24. Conclusion

Advanced Python OOP enables developers to build:

- Scalable applications
- Reusable architectures
- Maintainable systems
- Frameworks and libraries
- Enterprise-grade software

## Key Takeaways

- Prefer composition over deep inheritance
- Use abstraction to define clean interfaces
- Keep classes focused and small
- Leverage dataclasses for cleaner models
- Apply SOLID principles consistently
- Use metaclasses sparingly
- Write testable, loosely coupled code

---

# Additional Resources

## Official Documentation

- [Python Official Documentation](https://docs.python.org/3/)
- [Python Data Model Documentation](https://docs.python.org/3/reference/datamodel.html)
- [Python abc Module Documentation](https://docs.python.org/3/library/abc.html)
- [Python dataclasses Documentation](https://docs.python.org/3/library/dataclasses.html)

## Recommended Practice Projects

1. Banking system
2. Task management application
3. Inventory management system
4. Chat application
5. REST API backend
6. Game engine prototype
7. Plugin-based application
8. File synchronization tool
