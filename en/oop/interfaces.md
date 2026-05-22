---
title: "Interfaces: The Contract You Must Keep"
tags: [Interface, OOP, Abstraction]
date: 2026-05-17
order: 3
date_en: "May 2026"
read_time: "5 min"
description: "Uber doesn't know how Venmo works internally. Stripe doesn't know how Chase processes transactions. Yet payments happen seamlessly. That's the power of Interfaces."
excerpt: "Uber doesn't know how Venmo works. Stripe doesn't know Chase's internals. Yet checkout works every time. That's the Interface contract — what to do, not how."
---

When you finish an Uber ride in New York, you pay.

Venmo, Zelle, credit card, Apple Pay — pick whatever you have. The app handles all of them.

Now think: does Uber's engineering team know how Venmo's servers process a transaction? Is Chase's internal payment logic written somewhere in Uber's codebase?

No.

Uber knows exactly one thing: "Tell me `processPayment(amount)` and I'll handle the rest." Venmo does it its way. Chase does it its way. Uber doesn't care.

That contract is called an **Interface.**

---

## 1. What Is an Interface?

Imagine you're starting a new job at a restaurant in the West Village. Your manager says: "You need to do three things: take orders, deliver food, collect payment. How you do it is up to you."

That list of three things is an Interface. The manager is telling you **what** to do — not **how** to do it.

In programming, an **Interface** is a contract. It defines what methods a class must implement, but says nothing about what happens inside those methods.

{% include code-blocks/interfaces/code-1.html %}

`PaymentGateway` is saying: "Any class that implements me must have `process_payment`, `refund`, and `get_transaction_status`." What they do inside? That's the implementer's business.

{% include diagrams/interfaces/diagram-1.html %}

---

## 2. Core Properties of an Interface

**What, not how.** An interface holds method signatures, not implementations. Like a restaurant menu — it tells you what's available, not how it's cooked.

**A class can implement multiple interfaces.** A waiter can take orders, handle bills, and set tables. A class can honor multiple contracts at the same time.

**You can't instantiate an interface directly.** The menu doesn't cook the food — the chef does. You can't create an object from an interface alone.

**Loose coupling.** Uber's code doesn't depend on Venmo. It depends on the `PaymentGateway` interface. If Venmo changes its internals tomorrow, Uber's core code doesn't need to change at all.

---

## 3. Code Example: Payment Gateway

{% include code-blocks/interfaces/code-2.html %}

Now look at how independent `RideCheckout` is:

```python
# With Venmo
checkout = RideCheckout(WalletPayment())
checkout.complete_ride(12.50)
# Wallet: processing payment of 12.5...
# Ride complete! Payment processed.

# With Zelle
checkout = RideCheckout(DirectDebitPayment())
checkout.complete_ride(12.50)
# Direct debit: processing payment of 12.5...
# Ride complete! Payment processed.
```

Not a single line of `RideCheckout` changed. Tomorrow if Apple Pay gets added, build an `ApplePayPayment` class. `RideCheckout` stays untouched.

---

## Real-World Example: Notification Service

When an order is confirmed on DoorDash, the customer gets notified — SMS, email, and a push notification, all at once.

Three separate services. Three different company APIs. But the job is always the same: "Send this message to this user."

{% include code-blocks/interfaces/code-3.html %}

{% include diagrams/interfaces/diagram-2.html %}

```python
processor = OrderProcessor([
    SmsNotification(),
    EmailNotification(),
    PushNotification()
])

processor.confirm_order("+1-646-555-0192", "DD-8821")
# SMS to +1-646-555-0192: Order #DD-8821 confirmed!
# Email to +1-646-555-0192: Order #DD-8821 confirmed!
# Push notification to +1-646-555-0192: Order #DD-8821 confirmed!
```

Tomorrow if WhatsApp notifications get added, build a `WhatsAppNotification` class. `OrderProcessor` doesn't change.

---

## Summary

| In the story | In code |
|---|---|
| The restaurant manager's job list | Interface |
| The three tasks on that list | Abstract methods |
| The waiter who does those tasks | Implementing class |
| Uber's payment contract | PaymentGateway interface |
| Venmo and Chase doing it their own way | Concrete implementations |
| Adding a new payment method easily | Loose coupling |

**An interface says what to do. How to do it is the implementing class's job.**

**Interfaces let you add new implementations without touching existing code.**

**A class can implement multiple interfaces — far more flexible than deep inheritance.**

---

> Next question: Should an object's internal data be open to everyone? Shouldn't some things stay hidden? That idea is called **Encapsulation.**

*Next in the OOP series: Encapsulation — what doesn't need to be seen, shouldn't be*
