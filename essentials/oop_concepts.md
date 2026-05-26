# Architecture & OOP Concepts — Example Project

This tutorial demonstrates the following concepts in a single cohesive Python project:

- Encapsulation
- Abstraction
- Dependency Injection
- Polymorphism
- Dataclasses
- Composition
- SOLID Principles

The project simulates a small notification and order processing system.

---

# Project Structure

```text
project/
│
├── models.py
├── repositories.py
├── payment.py
├── notifications.py
├── services.py
├── app.py
└── tests/
```

The examples below are simplified into sections for readability.

---

# 1. Dataclasses

Python dataclasses reduce boilerplate when creating data objects.

## Why Dataclasses?

Without dataclasses:

```python
class User:
    def __init__(self, id: int, name: str, email: str):
        self.id = id
        self.name = name
        self.email = email
```

With dataclasses:

```python
from dataclasses import dataclass

@dataclass
class User:
    id: int
    name: str
    email: str
```

Benefits:

- Automatic `__init__`
- Automatic `__repr__`
- Automatic equality methods
- Cleaner code
- Better maintainability

---

# models.py

```python
from dataclasses import dataclass, field
from typing import List
from uuid import uuid4


@dataclass(slots=True)
class Product:
    id: str
    name: str
    price: float


@dataclass(slots=True)
class OrderItem:
    product: Product
    quantity: int

    @property
    def total_price(self) -> float:
        return self.product.price * self.quantity


@dataclass(slots=True)
class Order:
    customer_email: str
    items: List[OrderItem] = field(default_factory=list)
    id: str = field(default_factory=lambda: str(uuid4()))

    @property
    def total_amount(self) -> float:
        return sum(item.total_price for item in self.items)
```

---

# 2. Encapsulation

Encapsulation means:

- Keeping internal state protected
- Exposing behavior through methods
- Preventing invalid state changes

Python does not enforce strict private access like Java or C++, but conventions exist.

---

# Example: Encapsulation

```python
class BankAccount:
    def __init__(self, owner: str, balance: float = 0):
        self.owner = owner
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount: float):
        if amount <= 0:
            raise ValueError("Deposit must be positive")

        self._balance += amount

    def withdraw(self, amount: float):
        if amount > self._balance:
            raise ValueError("Insufficient funds")

        self._balance -= amount
```

---

## Why This Is Encapsulation

The user cannot safely manipulate balance directly:

```python
account._balance = -1000000
```

Instead, balance modifications happen through controlled methods:

```python
account.deposit(100)
account.withdraw(50)
```

This protects business rules.

---

# 3. Abstraction

Abstraction means:

- Hiding implementation details
- Exposing only required behavior
- Defining contracts/interfaces

Python commonly uses:

- Abstract Base Classes (ABC)
- Protocols

---

# payment.py

```python
from abc import ABC, abstractmethod


class PaymentProcessor(ABC):

    @abstractmethod
    def process_payment(self, amount: float) -> None:
        pass
```

This defines a contract.

Any payment processor must implement:

```python
process_payment()
```

---

# Concrete Implementations

```python
class StripePaymentProcessor(PaymentProcessor):

    def process_payment(self, amount: float) -> None:
        print(f"Processing Stripe payment: ${amount:.2f}")


class PayPalPaymentProcessor(PaymentProcessor):

    def process_payment(self, amount: float) -> None:
        print(f"Processing PayPal payment: ${amount:.2f}")
```

---

## Why This Is Abstraction

The application depends on:

```python
PaymentProcessor
```

NOT on:

```python
StripePaymentProcessor
```

The caller does not care how payment works internally.

---

# 4. Polymorphism

Polymorphism means:

> Different objects can be treated through the same interface.

---

# Example

```python
processors = [
    StripePaymentProcessor(),
    PayPalPaymentProcessor(),
]

for processor in processors:
    processor.process_payment(100)
```

Output:

```text
Processing Stripe payment: $100.00
Processing PayPal payment: $100.00
```

The loop does not care about the concrete type.

Each object responds differently to the same method call.

This is polymorphism.

---

# 5. Composition

Composition means:

> Building complex objects from smaller objects.

This is often preferred over inheritance.

---

# Example: Order Contains OrderItems

```python
@dataclass
class Order:
    customer_email: str
    items: list[OrderItem]
```

An order is composed of:

- multiple order items
- each item contains a product

Relationship:

```text
Order
 ├── OrderItem
 │     └── Product
 ├── OrderItem
 │     └── Product
```

---

# Another Example: Notification Service Composition

# notifications.py

```python
from abc import ABC, abstractmethod


class NotificationSender(ABC):

    @abstractmethod
    def send(self, recipient: str, message: str) -> None:
        pass


class EmailSender(NotificationSender):

    def send(self, recipient: str, message: str) -> None:
        print(f"Sending EMAIL to {recipient}: {message}")


class SmsSender(NotificationSender):

    def send(self, recipient: str, message: str) -> None:
        print(f"Sending SMS to {recipient}: {message}")
```

---

# services.py

```python
class NotificationService:

    def __init__(self, sender: NotificationSender):
        self.sender = sender

    def notify(self, recipient: str, message: str):
        self.sender.send(recipient, message)
```

`NotificationService` is composed with a sender.

---

# 6. Dependency Injection

Dependency Injection (DI) means:

> Providing dependencies from the outside instead of creating them internally.

---

# Bad Example (Tight Coupling)

```python
class OrderService:

    def __init__(self):
        self.payment_processor = StripePaymentProcessor()
```

Problems:

- Hard to test
- Hard to replace
- Strong coupling
- Violates SOLID principles

---

# Good Example (Dependency Injection)

```python
class OrderService:

    def __init__(
        self,
        payment_processor: PaymentProcessor,
        notification_service: NotificationService,
    ):
        self.payment_processor = payment_processor
        self.notification_service = notification_service

    def checkout(self, order: Order):
        self.payment_processor.process_payment(order.total_amount)

        self.notification_service.notify(
            order.customer_email,
            f"Your order total was ${order.total_amount:.2f}",
        )
```

Dependencies are injected from outside.

---

# app.py

```python
stripe_processor = StripePaymentProcessor()
email_sender = EmailSender()
notification_service = NotificationService(email_sender)

service = OrderService(
    payment_processor=stripe_processor,
    notification_service=notification_service,
)
```

This architecture is:

- flexible
- testable
- loosely coupled

---

# Why Dependency Injection Matters

Testing becomes easy.

---

# Example Test Double

```python
class FakePaymentProcessor(PaymentProcessor):

    def process_payment(self, amount: float) -> None:
        print("Fake payment")
```

Now tests can inject fake implementations.

---

# 7. SOLID Principles

SOLID is a set of object-oriented design principles.

---

# S — Single Responsibility Principle (SRP)

> A class should have only one reason to change.

---

## Bad Example

```python
class UserManager:

    def create_user(self):
        pass

    def send_email(self):
        pass

    def generate_report(self):
        pass
```

This class does too many things.

---

## Good Example

```python
class UserService:

    def create_user(self):
        pass


class EmailService:

    def send_email(self):
        pass


class ReportService:

    def generate_report(self):
        pass
```

Each class has one responsibility.

---

# O — Open/Closed Principle (OCP)

> Software entities should be open for extension but closed for modification.

---

Our payment abstraction follows OCP.

We can add:

```python
class CryptoPaymentProcessor(PaymentProcessor):

    def process_payment(self, amount: float) -> None:
        print(f"Crypto payment: ${amount:.2f}")
```

WITHOUT modifying existing code.

---

# L — Liskov Substitution Principle (LSP)

> Subtypes must be replaceable for their base types.

---

All payment processors must behave correctly:

```python
processor: PaymentProcessor = StripePaymentProcessor()
processor.process_payment(100)
```

Any subclass should work interchangeably.

---

# Bad LSP Example

```python
class BrokenProcessor(PaymentProcessor):

    def process_payment(self, amount: float):
        raise Exception("Always fails")
```

This violates expectations.

---

# I — Interface Segregation Principle (ISP)

> Clients should not depend on methods they do not use.

---

## Bad Example

```python
class Worker(ABC):

    @abstractmethod
    def work(self):
        pass

    @abstractmethod
    def eat(self):
        pass
```

A robot worker may not eat.

---

## Better Example

```python
class Workable(ABC):

    @abstractmethod
    def work(self):
        pass


class Eatable(ABC):

    @abstractmethod
    def eat(self):
        pass
```

Smaller interfaces are better.

---

# D — Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules.

Instead:

> Both should depend on abstractions.

---

Our `OrderService` depends on:

```python
PaymentProcessor
NotificationSender
```

NOT concrete classes.

This is DIP.

---

# Full Example

# app.py

```python
from models import Product, OrderItem, Order
from payment import StripePaymentProcessor
from notifications import EmailSender
from services import NotificationService, OrderService


laptop = Product(
    id="1",
    name="Laptop",
    price=1500,
)

mouse = Product(
    id="2",
    name="Mouse",
    price=50,
)

order = Order(
    customer_email="alice@example.com",
    items=[
        OrderItem(product=laptop, quantity=1),
        OrderItem(product=mouse, quantity=2),
    ],
)

payment_processor = StripePaymentProcessor()
email_sender = EmailSender()
notification_service = NotificationService(email_sender)

order_service = OrderService(
    payment_processor=payment_processor,
    notification_service=notification_service,
)

order_service.checkout(order)
```

---

# Output

```text
Processing Stripe payment: $1600.00
Sending EMAIL to alice@example.com: Your order total was $1600.00
```

---

# How All Concepts Work Together

| Concept | Example |
|---|---|
| Encapsulation | BankAccount protects balance |
| Abstraction | PaymentProcessor interface |
| Dependency Injection | Injecting payment processor |
| Polymorphism | Different processors same method |
| Dataclasses | Product, Order, OrderItem |
| Composition | Order contains OrderItems |
| SOLID | Entire architecture |

---

# Why These Concepts Matter in Real Projects

These concepts help create software that is:

- maintainable
- scalable
- testable
- reusable
- extensible
- easier to reason about

Large systems become manageable when responsibilities are clearly separated.

---

# Common Interview Questions

## Difference Between Abstraction and Encapsulation

| Abstraction | Encapsulation |
|---|---|
| Hides complexity | Protects internal state |
| Focuses on what object does | Focuses on how data is protected |
| Implemented via interfaces/ABCs | Implemented via access control |

---

## Composition vs Inheritance

Prefer composition when:

- objects collaborate
- behavior should be replaceable
- flexibility is important

Use inheritance when:

- there is a true “is-a” relationship
- polymorphism is required

---

# Advanced Improvements

In production systems you could further add:

- database repositories
- caching layers
- async processing
- event-driven architecture
- message queues
- dependency injection containers
- structured logging
- unit tests
- integration tests
- domain-driven design

---

# Example Unit Test

```python
class FakeNotifier(NotificationSender):

    def __init__(self):
        self.messages = []

    def send(self, recipient: str, message: str):
        self.messages.append((recipient, message))


def test_notification_service():
    fake = FakeNotifier()

    service = NotificationService(fake)

    service.notify("test@example.com", "hello")

    assert len(fake.messages) == 1
```

Dependency injection makes this extremely easy.

---

# Final Takeaways

## Encapsulation

Protect object state and enforce rules.

## Abstraction

Define contracts and hide implementation details.

## Dependency Injection

Inject dependencies instead of creating them internally.

## Polymorphism

Different objects respond to the same interface.

## Dataclasses

Simplify data models.

## Composition

Build systems from smaller reusable components.

## SOLID

Design maintainable and extensible software.

