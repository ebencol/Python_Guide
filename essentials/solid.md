# SOLID Principles

## Table of Contents

1. Introduction to SOLID
2. Why SOLID Matters
3. SOLID in Dynamic Languages Like Python
4. Single Responsibility Principle (SRP)
5. Open/Closed Principle (OCP)
6. Liskov Substitution Principle (LSP)
7. Interface Segregation Principle (ISP)
8. Dependency Inversion Principle (DIP)
9. Combining SOLID Principles
10. SOLID in Real-World Python Architectures
11. SOLID with FastAPI and Django
12. Common Anti-Patterns
13. Testing Benefits of SOLID
14. Refactoring Strategies
15. Performance and Tradeoffs
16. Best Practices
17. Summary

---

# 1. Introduction to SOLID

SOLID is a collection of five object-oriented design principles that improve:

- Maintainability
- Scalability
- Testability
- Extensibility
- Readability
- Team collaboration

The acronym stands for:

| Principle | Description |
|---|---|
| S | Single Responsibility Principle |
| O | Open/Closed Principle |
| L | Liskov Substitution Principle |
| I | Interface Segregation Principle |
| D | Dependency Inversion Principle |

SOLID originated from Robert C. Martin (Uncle Bob) and remains foundational in modern software architecture.

Although created for object-oriented programming, SOLID principles are equally useful in Python, especially when building:

- APIs
- Microservices
- Enterprise applications
- Frameworks
- Libraries
- Event-driven systems
- Data pipelines

---

# 2. Why SOLID Matters

Without SOLID principles, codebases typically become:

- Difficult to extend
- Fragile during refactoring
- Hard to test
- Highly coupled
- Filled with duplicated logic

Example of tightly coupled code:

```python
class UserService:
    def save_user(self, user_data):
        db = PostgreSQLDatabase()
        db.connect()
        db.insert(user_data)
```

Problems:

- Database implementation is hardcoded
- Cannot swap database easily
- Difficult to unit test
- Violates Dependency Inversion Principle

SOLID helps prevent these architectural issues.

---

# 3. SOLID in Dynamic Languages Like Python

Python differs from Java/C# because:

- It uses duck typing
- Interfaces are informal
- Multiple paradigms are supported
- Runtime flexibility is high

This means:

- SOLID should be applied pragmatically
- Overengineering should be avoided
- Simplicity remains important

Pythonic SOLID focuses on:

- Composition over inheritance
- Protocols and abstractions
- Dependency injection
- Small cohesive classes
- Testability

Modern Python tools useful for SOLID:

| Tool | Purpose |
|---|---|
| abc | Abstract base classes |
| typing.Protocol | Structural typing |
| dataclasses | Data modeling |
| dependency-injector | Dependency injection |
| pytest | Testing |
| FastAPI Depends | Dependency inversion |

---

# 4. Single Responsibility Principle (SRP)

## Definition

> A class should have only one reason to change.

A class should focus on one responsibility.

---

## Bad Example

```python
class Report:
    def generate(self):
        print("Generating report")

    def save_to_database(self):
        print("Saving to database")

    def send_email(self):
        print("Sending email")
```

Problems:

The class handles:

- Business logic
- Persistence
- Communication

Changes in email logic could break report generation.

---

## Better Design

```python
class ReportGenerator:
    def generate(self):
        return "report data"


class ReportRepository:
    def save(self, report):
        print(f"Saving {report}")


class EmailService:
    def send(self, report):
        print(f"Sending {report}")
```

Now responsibilities are separated.

---

## Real-World Example

### Bad Service Layer

```python
class UserService:
    def register_user(self, data):
        validate(data)
        save_to_db(data)
        send_welcome_email(data)
        publish_event(data)
```

### Better Separation

```python
class UserValidator:
    def validate(self, data):
        pass


class UserRepository:
    def save(self, data):
        pass


class NotificationService:
    def send_welcome_email(self, user):
        pass


class EventBus:
    def publish(self, event):
        pass


class UserRegistrationService:
    def __init__(
        self,
        validator,
        repository,
        notification_service,
        event_bus,
    ):
        self.validator = validator
        self.repository = repository
        self.notification_service = notification_service
        self.event_bus = event_bus

    def register(self, data):
        self.validator.validate(data)
        user = self.repository.save(data)
        self.notification_service.send_welcome_email(user)
        self.event_bus.publish("user_registered")
```

---

## Benefits of SRP

- Easier testing
- Better modularity
- Reduced coupling
- Easier refactoring
- Improved readability

---

# 5. Open/Closed Principle (OCP)

## Definition

> Software entities should be open for extension but closed for modification.

You should add new behavior without changing existing code.

---

## Bad Example

```python
class DiscountCalculator:
    def calculate(self, customer_type, amount):
        if customer_type == "regular":
            return amount * 0.95

        if customer_type == "vip":
            return amount * 0.80

        if customer_type == "employee":
            return amount * 0.70
```

Every new discount type requires modifying the class.

---

## Better Design with Polymorphism

```python
from abc import ABC, abstractmethod


class DiscountStrategy(ABC):
    @abstractmethod
    def apply_discount(self, amount):
        pass


class RegularDiscount(DiscountStrategy):
    def apply_discount(self, amount):
        return amount * 0.95


class VIPDiscount(DiscountStrategy):
    def apply_discount(self, amount):
        return amount * 0.80


class EmployeeDiscount(DiscountStrategy):
    def apply_discount(self, amount):
        return amount * 0.70


class DiscountCalculator:
    def calculate(self, strategy: DiscountStrategy, amount):
        return strategy.apply_discount(amount)
```

---

## Extending Without Modifying

```python
class BlackFridayDiscount(DiscountStrategy):
    def apply_discount(self, amount):
        return amount * 0.50
```

No existing code changes are needed.

---

## Plugin Architecture Example

```python
class PaymentProcessor(ABC):
    @abstractmethod
    def process(self, amount):
        pass


class StripeProcessor(PaymentProcessor):
    def process(self, amount):
        print(f"Stripe charged {amount}")


class PayPalProcessor(PaymentProcessor):
    def process(self, amount):
        print(f"PayPal charged {amount}")
```

New payment providers can be added independently.

---

## OCP Patterns

Common patterns supporting OCP:

| Pattern | Purpose |
|---|---|
| Strategy | Replace algorithms dynamically |
| Factory | Create extensible object creation |
| Plugin systems | Runtime extension |
| Dependency injection | Runtime configuration |
| Event systems | Decoupled extensibility |

---

# 6. Liskov Substitution Principle (LSP)

## Definition

> Subtypes must be substitutable for their base types.

Derived classes should behave correctly when used in place of parent classes.

---

## Classic Violation Example

```python
class Bird:
    def fly(self):
        print("Flying")


class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins cannot fly")
```

This breaks expectations.

---

## Better Design

```python
class Bird:
    pass


class FlyingBird(Bird):
    def fly(self):
        print("Flying")


class Sparrow(FlyingBird):
    pass


class Penguin(Bird):
    def swim(self):
        print("Swimming")
```

---

## Behavioral Compatibility

LSP is not only about method signatures.

It also involves:

- Expected behavior
- Preconditions
- Postconditions
- Side effects

---

## Bad Banking Example

```python
class Account:
    def withdraw(self, amount):
        pass


class FixedDepositAccount(Account):
    def withdraw(self, amount):
        raise Exception("Withdrawals not allowed")
```

This violates substitutability.

---

## Better Design

```python
class Account:
    pass


class WithdrawableAccount(Account):
    def withdraw(self, amount):
        pass


class SavingsAccount(WithdrawableAccount):
    def withdraw(self, amount):
        print(f"Withdraw {amount}")


class FixedDepositAccount(Account):
    pass
```

---

## LSP Checklist

A subclass should:

- Preserve behavior contracts
- Not strengthen input requirements
- Not weaken output guarantees
- Not introduce unexpected exceptions

---

# 7. Interface Segregation Principle (ISP)

## Definition

> Clients should not be forced to depend on interfaces they do not use.

Large interfaces should be split into focused interfaces.

---

## Bad Example

```python
from abc import ABC, abstractmethod


class Worker(ABC):
    @abstractmethod
    def work(self):
        pass

    @abstractmethod
    def eat(self):
        pass


class RobotWorker(Worker):
    def work(self):
        print("Working")

    def eat(self):
        raise Exception("Robots do not eat")
```

---

## Better Design

```python
class Workable(ABC):
    @abstractmethod
    def work(self):
        pass


class Eatable(ABC):
    @abstractmethod
    def eat(self):
        pass


class HumanWorker(Workable, Eatable):
    def work(self):
        print("Human working")

    def eat(self):
        print("Human eating")


class RobotWorker(Workable):
    def work(self):
        print("Robot working")
```

---

## API Client Example

### Bad Interface

```python
class CloudProvider:
    def create_vm(self):
        pass

    def create_bucket(self):
        pass

    def create_database(self):
        pass
```

Not all providers support all services.

---

## Better Segregation

```python
class VMProvider:
    def create_vm(self):
        pass


class StorageProvider:
    def create_bucket(self):
        pass


class DatabaseProvider:
    def create_database(self):
        pass
```

---

## ISP with Python Protocols

Python supports structural typing via protocols.

```python
from typing import Protocol


class Writer(Protocol):
    def write(self, data: str) -> None:
        pass
```

Any compatible object satisfies the interface.

---

# 8. Dependency Inversion Principle (DIP)

## Definition

> High-level modules should not depend on low-level modules.
> Both should depend on abstractions.

---

## Bad Example

```python
class MySQLDatabase:
    def save(self, data):
        print("Saving to MySQL")


class UserService:
    def __init__(self):
        self.db = MySQLDatabase()

    def register(self, user):
        self.db.save(user)
```

Problems:

- Tight coupling
- Hard to test
- Hard to replace database

---

## Better Design

```python
from abc import ABC, abstractmethod


class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass


class MySQLDatabase(Database):
    def save(self, data):
        print("Saving to MySQL")


class PostgreSQLDatabase(Database):
    def save(self, data):
        print("Saving to PostgreSQL")


class UserService:
    def __init__(self, database: Database):
        self.database = database

    def register(self, user):
        self.database.save(user)
```

---

## Usage

```python
postgres = PostgreSQLDatabase()
service = UserService(postgres)
service.register({"name": "Alice"})
```

---

## Dependency Injection

DIP usually works together with dependency injection.

Types of injection:

| Type | Description |
|---|---|
| Constructor Injection | Dependencies passed in constructor |
| Method Injection | Dependencies passed into methods |
| Property Injection | Dependencies assigned to fields |

---

## Testing Advantage

```python
class FakeDatabase(Database):
    def save(self, data):
        print("Fake save")


fake_db = FakeDatabase()
service = UserService(fake_db)
```

Now testing is easy.

---

# 9. Combining SOLID Principles

Real systems combine all principles.

---

## Example: Notification System

```python
from abc import ABC, abstractmethod


class MessageSender(ABC):
    @abstractmethod
    def send(self, message: str):
        pass


class EmailSender(MessageSender):
    def send(self, message: str):
        print(f"Email: {message}")


class SMSSender(MessageSender):
    def send(self, message: str):
        print(f"SMS: {message}")


class NotificationService:
    def __init__(self, sender: MessageSender):
        self.sender = sender

    def notify(self, message: str):
        self.sender.send(message)
```

### Principles Used

| Principle | Application |
|---|---|
| SRP | Sender only sends messages |
| OCP | Add new sender types easily |
| LSP | All senders interchangeable |
| ISP | Minimal sender interface |
| DIP | Service depends on abstraction |

---

# 10. SOLID in Real-World Python Architectures

## Layered Architecture

Typical layers:

```text
Presentation Layer
        ↓
Service Layer
        ↓
Repository Layer
        ↓
Database
```

SOLID improves separation between layers.

---

## Repository Pattern

```python
from abc import ABC, abstractmethod


class UserRepository(ABC):
    @abstractmethod
    def get_by_id(self, user_id):
        pass


class SQLUserRepository(UserRepository):
    def get_by_id(self, user_id):
        return {"id": user_id}
```

---

## Service Layer

```python
class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository

    def get_user(self, user_id):
        return self.repository.get_by_id(user_id)
```

---

## Hexagonal Architecture

SOLID aligns naturally with:

- Ports and adapters
- Clean architecture
- Onion architecture
- Domain-driven design

Core business logic depends only on abstractions.

---

# 11. SOLID with FastAPI and Django

# FastAPI Example

FastAPI strongly encourages dependency injection.

```python
from fastapi import Depends, FastAPI

app = FastAPI()


class UserRepository:
    def get_user(self, user_id):
        return {"id": user_id}


class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository

    def get_user(self, user_id):
        return self.repository.get_user(user_id)



def get_user_repository():
    return UserRepository()



def get_user_service(
    repository: UserRepository = Depends(get_user_repository),
):
    return UserService(repository)


@app.get("/users/{user_id}")
def get_user(
    user_id: int,
    service: UserService = Depends(get_user_service),
):
    return service.get_user(user_id)
```

---

# Django Example

Django applications often violate SOLID when business logic is placed inside models.

### Bad Example

```python
class Order(models.Model):
    def complete_order(self):
        self.save()
        send_email()
        create_invoice()
        notify_shipping()
```

---

## Better Service Layer

```python
class OrderService:
    def complete_order(self, order):
        order.complete()
        self.send_notifications(order)
```

---

# 12. Common Anti-Patterns

## God Objects

Large classes doing everything.

```python
class ApplicationManager:
    pass
```

Symptoms:

- Thousands of lines
- Too many methods
- High coupling

---

## Deep Inheritance Trees

```text
Animal
 └── Mammal
      └── Dog
           └── WorkingDog
                └── PoliceDog
```

Prefer composition.

---

## Interface Pollution

Creating huge interfaces with many unused methods.

---

## Overengineering

Do not create abstractions prematurely.

Bad:

```python
class IUserFactoryBuilderProviderManager:
    pass
```

Keep designs simple.

---

# 13. Testing Benefits of SOLID

SOLID dramatically improves testing.

---

## Without SOLID

```python
class PaymentService:
    def pay(self):
        stripe = StripeAPI()
        stripe.charge()
```

Hard to mock.

---

## With DIP

```python
class PaymentGateway(ABC):
    @abstractmethod
    def charge(self):
        pass
```

Now tests become easy.

---

## Pytest Example

```python
class FakeGateway(PaymentGateway):
    def charge(self):
        return True



def test_payment():
    gateway = FakeGateway()
    assert gateway.charge() is True
```

---

# 14. Refactoring Strategies

## Step 1: Identify Responsibilities

Questions:

- What changes this class?
- How many reasons to change exist?

---

## Step 2: Extract Interfaces

Create abstractions around:

- Databases
- APIs
- External services
- Message queues

---

## Step 3: Use Composition

Prefer:

```python
class Car:
    def __init__(self, engine):
        self.engine = engine
```

Instead of:

```python
class Car(Engine):
    pass
```

---

## Step 4: Introduce Dependency Injection

Pass dependencies externally.

---

## Step 5: Write Tests During Refactoring

Tests protect against regressions.

---

# 15. Performance and Tradeoffs

SOLID introduces:

- More abstractions
- More classes
- More indirection

Tradeoffs:

| Benefit | Cost |
|---|---|
| Maintainability | Slight complexity |
| Testability | More abstractions |
| Extensibility | More files/classes |
| Scalability | More architecture |

For small scripts, SOLID may be unnecessary.

For medium and large systems, SOLID becomes extremely valuable.

---

# 16. Best Practices

## Prefer Composition Over Inheritance

Good:

```python
class Car:
    def __init__(self, engine):
        self.engine = engine
```

---

## Keep Interfaces Small

Focused abstractions are easier to maintain.

---

## Depend on Abstractions

Avoid hardcoded implementations.

---

## Use Protocols in Modern Python

```python
from typing import Protocol


class Serializer(Protocol):
    def serialize(self, data) -> str:
        pass
```

---

## Avoid Premature Abstraction

Start simple.
Refactor when patterns emerge.

---

## Prefer Immutable Data

Using dataclasses:

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class User:
    id: int
    name: str
```

---

# 17. Summary

SOLID principles improve software quality by promoting:

- Modularity
- Extensibility
- Maintainability
- Testability
- Scalability

## Quick Recap

| Principle | Core Idea |
|---|---|
| SRP | One responsibility per class |
| OCP | Extend without modifying |
| LSP | Subtypes must be replaceable |
| ISP | Small focused interfaces |
| DIP | Depend on abstractions |

---

# Final Thoughts

SOLID is not about creating excessive abstractions.

The real goal is:

- Flexible architecture
- Easier maintenance
- Safer refactoring
- Better collaboration

In Python specifically:

- Use SOLID pragmatically
- Favor readability
- Prefer composition
- Use protocols and dependency injection
- Avoid unnecessary complexity

When applied correctly, SOLID principles help transform fragile codebases into scalable, maintainable systems suitable for professional software development.

