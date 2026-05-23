# Advanced Design Patterns in Python

## Table of Contents

1. Introduction to Design Patterns
2. Creational Patterns
   - Singleton
   - Factory Method
   - Abstract Factory
   - Builder
   - Prototype
3. Structural Patterns
   - Adapter
   - Decorator
   - Facade
   - Proxy
   - Composite
4. Behavioral Patterns
   - Observer
   - Strategy
   - Command
   - State
   - Iterator
   - Template Method
5. Real-World Architecture Examples
6. Best Practices and Anti-Patterns
7. Conclusion

---

# 1. Introduction to Design Patterns

Design patterns are reusable solutions to recurring software design problems. They are not finished code but templates and architectural ideas that improve:

- Maintainability
- Scalability
- Readability
- Flexibility
- Testability

Python’s dynamic nature makes many design patterns easier to implement compared to statically typed languages.

## Categories of Design Patterns

| Category | Purpose |
|---|---|
| Creational | Object creation mechanisms |
| Structural | Composition of classes and objects |
| Behavioral | Communication between objects |

---

# 2. Creational Patterns

# Singleton Pattern

## Purpose

Ensure a class has only one instance and provide global access to it.

## Common Use Cases

- Database connections
- Logging systems
- Configuration managers
- Cache managers
- Thread pools

## Basic Implementation

```python
class Singleton:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance


obj1 = Singleton()
obj2 = Singleton()

print(obj1 is obj2)  # True
```

## Thread-Safe Singleton

```python
from threading import Lock


class DatabaseConnection:
    _instance = None
    _lock = Lock()

    def __new__(cls):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
                cls._instance.initialize()
        return cls._instance

    def initialize(self):
        self.connection = "Connected"


conn1 = DatabaseConnection()
conn2 = DatabaseConnection()

print(conn1 is conn2)
```

## Real-World Example: Application Config Manager

```python
import json


class ConfigManager:
    _instance = None

    def __new__(cls, path="config.json"):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.load(path)
        return cls._instance

    def load(self, path):
        self.settings = {
            "debug": True,
            "database": "postgresql"
        }


config1 = ConfigManager()
config2 = ConfigManager()

print(config1.settings)
print(config1 is config2)
```

## Advantages

- Controlled access to shared resources
- Reduces memory usage
- Centralized configuration

## Disadvantages

- Harder to test
- Hidden dependencies
- Can behave like a global variable

---

# Factory Method Pattern

## Purpose

Define an interface for object creation while allowing subclasses or logic to determine which object to instantiate.

## Use Cases

- Payment systems
- Notification services
- File parsers
- Cloud providers
- Plugin systems

## Example: Notification System

```python
from abc import ABC, abstractmethod


class Notification(ABC):
    @abstractmethod
    def send(self, message):
        pass


class EmailNotification(Notification):
    def send(self, message):
        print(f"Sending EMAIL: {message}")


class SMSNotification(Notification):
    def send(self, message):
        print(f"Sending SMS: {message}")


class NotificationFactory:
    @staticmethod
    def create_notification(notification_type):
        if notification_type == "email":
            return EmailNotification()
        elif notification_type == "sms":
            return SMSNotification()
        else:
            raise ValueError("Unsupported notification type")


notifier = NotificationFactory.create_notification("email")
notifier.send("Welcome!")
```

## Real-World Example: Database Drivers

```python
class MySQLConnection:
    def connect(self):
        return "Connected to MySQL"


class PostgreSQLConnection:
    def connect(self):
        return "Connected to PostgreSQL"


class DatabaseFactory:
    @staticmethod
    def get_database(db_type):
        databases = {
            "mysql": MySQLConnection,
            "postgresql": PostgreSQLConnection,
        }

        if db_type not in databases:
            raise ValueError("Invalid database")

        return databases[db_type]()


connection = DatabaseFactory.get_database("postgresql")
print(connection.connect())
```

## Benefits

- Open/Closed Principle
- Removes complex conditionals
- Easier extensibility

---

# Abstract Factory Pattern

## Purpose

Create families of related objects without specifying their concrete classes.

## Use Cases

- Cross-platform UI systems
- Multi-theme applications
- Game engines
- Multi-cloud deployment systems

## Example: GUI Toolkit

```python
from abc import ABC, abstractmethod


class Button(ABC):
    @abstractmethod
    def paint(self):
        pass


class WindowsButton(Button):
    def paint(self):
        return "Windows Button"


class MacButton(Button):
    def paint(self):
        return "Mac Button"


class GUIFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass


class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()


class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()


factory = WindowsFactory()
button = factory.create_button()
print(button.paint())
```

## Architecture Benefit

You can swap entire application families without changing client code.

---

# Builder Pattern

## Purpose

Separate construction of a complex object from its representation.

## Use Cases

- SQL query builders
- HTML builders
- API request builders
- Docker configuration generators
- Machine learning pipelines

## Example: SQL Query Builder

```python
class QueryBuilder:
    def __init__(self):
        self.query = ""

    def select(self, table):
        self.query += f"SELECT * FROM {table} "
        return self

    def where(self, condition):
        self.query += f"WHERE {condition} "
        return self

    def order_by(self, field):
        self.query += f"ORDER BY {field} "
        return self

    def build(self):
        return self.query.strip()


query = (
    QueryBuilder()
    .select("users")
    .where("age > 18")
    .order_by("name")
    .build()
)

print(query)
```

## Fluent Interface Advantage

Builder patterns often support fluent APIs for readability.

---

# Prototype Pattern

## Purpose

Create objects by cloning existing instances.

## Use Cases

- Game object duplication
- Deep copy templates
- Expensive object initialization
- Machine learning model presets

## Example

```python
import copy


class Character:
    def __init__(self, name, inventory):
        self.name = name
        self.inventory = inventory

    def clone(self):
        return copy.deepcopy(self)


hero = Character("Knight", ["Sword", "Shield"])
clone = hero.clone()

clone.name = "Mage"
clone.inventory.append("Spellbook")

print(hero.inventory)
print(clone.inventory)
```

---

# 3. Structural Patterns

# Adapter Pattern

## Purpose

Convert one interface into another expected by clients.

## Use Cases

- Legacy integration
- Third-party APIs
- Payment gateway compatibility
- File format converters

## Example: Payment Gateway Adapter

```python
class StripeAPI:
    def make_payment(self, amount):
        return f"Stripe processed ${amount}"


class StripeAdapter:
    def __init__(self, stripe_api):
        self.stripe_api = stripe_api

    def pay(self, amount):
        return self.stripe_api.make_payment(amount)


stripe = StripeAdapter(StripeAPI())
print(stripe.pay(100))
```

## Key Insight

Adapters allow incompatible systems to work together without modifying source code.

---

# Decorator Pattern

## Purpose

Add behavior dynamically to objects.

## Use Cases

- Logging
- Authentication
- Rate limiting
- Metrics collection
- Retry logic
- Caching

## Function Decorator Example

```python
from functools import wraps
import time


def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"Execution Time: {end - start:.4f}s")
        return result

    return wrapper


@timer
def compute():
    sum(range(10_000_000))


compute()
```

## Class-Based Decorator

```python
class LoggingDecorator:
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print(f"Calling {self.func.__name__}")
        return self.func(*args, **kwargs)


@LoggingDecorator
def greet(name):
    return f"Hello {name}"


print(greet("Alice"))
```

## Real-World Example: Authentication Middleware

```python
from functools import wraps


def requires_auth(func):
    @wraps(func)
    def wrapper(user, *args, **kwargs):
        if not user.get("authenticated"):
            raise PermissionError("Unauthorized")
        return func(user, *args, **kwargs)

    return wrapper


@requires_auth
def dashboard(user):
    return "Admin Dashboard"


user = {"authenticated": True}
print(dashboard(user))
```

---

# Facade Pattern

## Purpose

Provide a simplified interface to a complex subsystem.

## Use Cases

- Cloud SDKs
- Deployment systems
- Media processing
- Payment workflows

## Example: Video Conversion System

```python
class VideoDecoder:
    def decode(self):
        print("Decoding video")


class VideoCompressor:
    def compress(self):
        print("Compressing video")


class VideoUploader:
    def upload(self):
        print("Uploading video")


class VideoProcessingFacade:
    def __init__(self):
        self.decoder = VideoDecoder()
        self.compressor = VideoCompressor()
        self.uploader = VideoUploader()

    def process(self):
        self.decoder.decode()
        self.compressor.compress()
        self.uploader.upload()


processor = VideoProcessingFacade()
processor.process()
```

---

# Proxy Pattern

## Purpose

Provide a placeholder or intermediary for another object.

## Use Cases

- Lazy loading
- Access control
- API throttling
- Distributed systems
- Smart references

## Example: Lazy Database Loader

```python
class Database:
    def connect(self):
        print("Connecting to database...")


class DatabaseProxy:
    def __init__(self):
        self.database = None

    def connect(self):
        if self.database is None:
            self.database = Database()
        self.database.connect()


proxy = DatabaseProxy()
proxy.connect()
```

---

# Composite Pattern

## Purpose

Treat groups of objects the same way as individual objects.

## Use Cases

- File systems
- GUI components
- Organization charts
- Scene graphs

## Example: File System

```python
from abc import ABC, abstractmethod


class Component(ABC):
    @abstractmethod
    def show(self):
        pass


class File(Component):
    def __init__(self, name):
        self.name = name

    def show(self):
        print(self.name)


class Folder(Component):
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, component):
        self.children.append(component)

    def show(self):
        print(self.name)
        for child in self.children:
            child.show()


root = Folder("root")
root.add(File("a.txt"))
root.add(File("b.txt"))

root.show()
```

---

# 4. Behavioral Patterns

# Observer Pattern

## Purpose

Define one-to-many dependency relationships.

## Use Cases

- Event systems
- Notification systems
- GUI frameworks
- Real-time dashboards
- Message brokers

## Example

```python
class Subject:
    def __init__(self):
        self.observers = []

    def subscribe(self, observer):
        self.observers.append(observer)

    def notify(self, message):
        for observer in self.observers:
            observer.update(message)


class Observer:
    def update(self, message):
        print(f"Received: {message}")


subject = Subject()
observer1 = Observer()
observer2 = Observer()

subject.subscribe(observer1)
subject.subscribe(observer2)

subject.notify("New Event")
```

## Real-World Example: Stock Market Alerts

```python
class StockMarket:
    def __init__(self):
        self.subscribers = []

    def subscribe(self, trader):
        self.subscribers.append(trader)

    def update_price(self, stock, price):
        for trader in self.subscribers:
            trader.notify(stock, price)


class Trader:
    def __init__(self, name):
        self.name = name

    def notify(self, stock, price):
        print(f"{self.name}: {stock} -> ${price}")
```

---

# Strategy Pattern

## Purpose

Define interchangeable algorithms.

## Use Cases

- Payment methods
- Compression algorithms
- Sorting strategies
- AI behavior systems
- Recommendation engines

## Example: Payment Processing

```python
from abc import ABC, abstractmethod


class PaymentStrategy(ABC):
    @abstractmethod
    def pay(self, amount):
        pass


class CreditCardPayment(PaymentStrategy):
    def pay(self, amount):
        print(f"Paid ${amount} using Credit Card")


class PayPalPayment(PaymentStrategy):
    def pay(self, amount):
        print(f"Paid ${amount} using PayPal")


class ShoppingCart:
    def __init__(self, payment_strategy):
        self.payment_strategy = payment_strategy

    def checkout(self, amount):
        self.payment_strategy.pay(amount)


cart = ShoppingCart(PayPalPayment())
cart.checkout(200)
```

---

# Command Pattern

## Purpose

Encapsulate requests as objects.

## Use Cases

- Undo/redo systems
- Queues
- Job schedulers
- Transaction processing
- Remote command execution

## Example

```python
class Light:
    def on(self):
        print("Light ON")

    def off(self):
        print("Light OFF")


class LightOnCommand:
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.on()


class RemoteControl:
    def submit(self, command):
        command.execute()


light = Light()
command = LightOnCommand(light)

remote = RemoteControl()
remote.submit(command)
```

---

# State Pattern

## Purpose

Allow objects to alter behavior when internal state changes.

## Use Cases

- Media players
- Workflow engines
- Game characters
- Traffic lights
- Order management systems

## Example: Traffic Light

```python
class RedState:
    def next(self):
        return GreenState()

    def display(self):
        return "RED"


class GreenState:
    def next(self):
        return YellowState()

    def display(self):
        return "GREEN"


class YellowState:
    def next(self):
        return RedState()

    def display(self):
        return "YELLOW"


class TrafficLight:
    def __init__(self):
        self.state = RedState()

    def change(self):
        self.state = self.state.next()

    def show(self):
        print(self.state.display())


light = TrafficLight()
light.show()
light.change()
light.show()
```

---

# Iterator Pattern

## Purpose

Provide sequential access to elements without exposing structure.

## Example

```python
class NumberCollection:
    def __init__(self, numbers):
        self.numbers = numbers

    def __iter__(self):
        for number in self.numbers:
            yield number


collection = NumberCollection([1, 2, 3, 4])

for num in collection:
    print(num)
```

## Python Insight

Generators make Iterator implementations elegant and memory efficient.

---

# Template Method Pattern

## Purpose

Define an algorithm skeleton while allowing subclasses to redefine steps.

## Use Cases

- Data pipelines
- Machine learning workflows
- Build systems
- ETL jobs

## Example

```python
from abc import ABC, abstractmethod


class DataProcessor(ABC):
    def process(self):
        self.load()
        self.transform()
        self.save()

    @abstractmethod
    def load(self):
        pass

    @abstractmethod
    def transform(self):
        pass

    @abstractmethod
    def save(self):
        pass


class CSVProcessor(DataProcessor):
    def load(self):
        print("Loading CSV")

    def transform(self):
        print("Transforming CSV")

    def save(self):
        print("Saving CSV")


processor = CSVProcessor()
processor.process()
```

---

# 5. Real-World Architecture Examples

# Example: Web Framework Architecture

| Feature | Pattern Used |
|---|---|
| Routing | Strategy |
| Middleware | Decorator |
| ORM Connection | Singleton |
| Plugin System | Factory |
| Event Hooks | Observer |
| HTTP Request Pipeline | Template Method |

---

# Example: E-Commerce Platform

| Component | Design Pattern |
|---|---|
| Payment Methods | Strategy |
| Product Creation | Factory |
| Logging | Decorator |
| Cart Commands | Command |
| Notification Service | Observer |
| Inventory Cache | Proxy |

---

# Example: Machine Learning Platform

| Component | Pattern |
|---|---|
| Model Pipelines | Builder |
| Training Workflow | Template Method |
| Hyperparameter Search | Strategy |
| Experiment Logging | Decorator |
| Dataset Loading | Proxy |

---

# 6. Best Practices and Anti-Patterns

# When to Use Design Patterns

Use patterns when:

- Problems repeat consistently
- Code becomes difficult to extend
- Complexity grows rapidly
- Multiple teams collaborate
- Architecture stability matters

Avoid patterns when:

- Simpler solutions exist
- Requirements are small
- Abstractions become unnecessary
- Overengineering appears

---

# Common Anti-Patterns

## God Object

A single class handling too many responsibilities.

## Overusing Singleton

Leads to hidden global state.

## Deep Inheritance Trees

Favor composition over inheritance.

## Pattern Mania

Using patterns simply because they exist.

---

# SOLID Principles Connection

| SOLID Principle | Related Patterns |
|---|---|
| Single Responsibility | Decorator |
| Open/Closed | Strategy, Factory |
| Liskov Substitution | Template Method |
| Interface Segregation | Adapter |
| Dependency Inversion | Abstract Factory |

---

# Performance Considerations

## Patterns That Improve Performance

- Flyweight
- Proxy
- Object Pool
- Lazy Initialization

## Patterns That Add Overhead

- Observer with many listeners
- Excessive decorators
- Deep abstraction layers

---

# Testing Design Patterns

## Example: Testing Strategy Pattern

```python
import unittest


class TestPayment(unittest.TestCase):
    def test_credit_card_payment(self):
        strategy = CreditCardPayment()
        self.assertIsNone(strategy.pay(100))


if __name__ == "__main__":
    unittest.main()
```

## Dependency Injection Advantage

Patterns become easier to test when dependencies are injected rather than hardcoded.

---

# 7. Conclusion

Design patterns are architectural tools that help developers:

- Write maintainable code
- Build scalable systems
- Improve extensibility
- Reduce coupling
- Increase flexibility

Python simplifies many traditional implementations through:

- First-class functions
- Duck typing
- Decorators
- Generators
- Dynamic attributes
- Context managers

The most important skill is not memorizing patterns but recognizing recurring problems and applying the appropriate solution.

---

# Recommended Next Topics

- Dependency Injection
- Clean Architecture
- Domain-Driven Design (DDD)
- CQRS and Event Sourcing
- Microservice Patterns
- Async Design Patterns
- Reactive Systems
- Distributed System Patterns

---

# Quick Pattern Selection Guide

| Problem | Recommended Pattern |
|---|---|
| Multiple algorithms | Strategy |
| Complex object creation | Builder |
| One shared instance | Singleton |
| Add runtime behavior | Decorator |
| Simplify subsystem | Facade |
| Event broadcasting | Observer |
| Undo/Redo | Command |
| Interchangeable families | Abstract Factory |
| Lazy loading | Proxy |
| State-dependent behavior | State |

---

# Final Advice

Mastering design patterns requires:

1. Building real projects
2. Refactoring existing systems
3. Studying open-source frameworks
4. Understanding trade-offs
5. Avoiding unnecessary abstraction

The best engineers know when NOT to use a design pattern.

