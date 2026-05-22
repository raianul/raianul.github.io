---
title: "Enums: When Options Must Be Fixed"
tags: [Enum, OOP, Constants]
date: 2026-05-16
order: 2
date_en: "May 2026"
read_time: "6 min"
description: "Three employees, three different spellings, one order status — three names. Enums are how you stop that chaos before it starts."
excerpt: "Three employees, three different spellings, one order status. Month-end reporting becomes a nightmare. Enums fix this — explained through a Brooklyn Etsy shop story."
---

Maria runs a small Etsy shop out of her apartment in Williamsburg, Brooklyn.

Handmade ceramic jewelry, Instagram DMs, 50–60 orders a month. Business is good.

But tracking orders is a mess.

Maria herself writes: "pending", "ready", "sent out", "arrived". Her helper Jasmine writes: "wait", "ready to ship", "shipped", "delivered". Another helper, Carlos, writes: "waiting", "prepped", "in transit", "got it".

Come month-end, nobody can answer: how many orders are still pending? How many shipped? Nobody knows — because the same status has three different names.

This problem is called **magic strings** and the solution is called **Enum.**

---

## 1. What Is an Enum?

If Maria had told everyone upfront: "For order status, use only these five words: PENDING, CONFIRMED, SHIPPED, DELIVERED, CANCELLED. Nothing else."

That fixed list of five words is an **Enum (Enumeration)**.

An Enum is a special data type that holds a fixed, predefined set of named constants. Once defined, no value outside that set is possible.

{% include code-blocks/enums/code-1.html %}

Now if anyone types `OrderStatus.SHIPED`, Python throws an error immediately. No typos. No "in transit" vs "sent out". Just five options.

{% include diagrams/enums/diagram-1.html %}

Without Enum, maintaining this flow is hard — anyone can write any string, and invalid states slip through silently.

---

## 2. Simple Enum: When You Just Need Names

The simplest Enum just holds names. No extra data needed.

Think of a ride-share payment method — either CASH, CARD, or DIGITAL_WALLET (Venmo). Nothing outside those three.

{% include code-blocks/enums/code-2.html %}

Enum members can be compared, looped over, and checked easily. Simple, readable, no surprises.

---

## 3. Enum with Properties and Methods: Smarter Enums

What if Maria's order status needs to carry more information — a display label and a description alongside the code?

You can add properties and methods directly to an Enum.

{% include code-blocks/enums/code-3.html %}

Now the Enum holds full information:

```python
status = OrderStatus.SHIPPED

print(status.value)        # shipped
print(status.label)        # Shipped
print(status.description)  # Order handed to courier
print(status.is_active())  # True
print(status.can_cancel()) # False — can't cancel once it's shipped
```

{% include diagrams/enums/diagram-2.html %}

Each Enum member now knows for itself whether it can be cancelled or whether it's still active. That logic lives inside the Enum — you don't have to repeat it everywhere else.

---

## Real-World Example: Amazon Order Processing

Amazon processes millions of orders a day. Each one moves through multiple states.

Without Enum, this system breaks fast:

```python
# Without Enum: danger zone
order["status"] = "shiped"    # typo — nobody catches it
order["status"] = "Delivered" # capital D — different string
order["status"] = "on the way" # someone went off-script

if order["status"] == "shipped":  # this check will never pass!
    notify_customer()
```

With Enum:

{% include code-blocks/enums/code-4.html %}

```python
order = Order("AMZ-001", "Ceramic earrings")
order.confirm()
order.ship()    # status isn't PACKED yet — nothing happens
order.cancel()  # can't cancel after it's shipped
```

With Enum, invalid state transitions are impossible. No one can accidentally write `order.status = "delvrd"` and have it silently accepted.

---

## Summary

| In the story | In code |
|---|---|
| Maria's five fixed status words | Enum |
| Each specific word | Enum member |
| Storing a display label with each status | Enum property |
| "Can't cancel once shipped" rule | Enum method |
| Anyone writing whatever they wanted | Magic strings (the problem) |

**Using Enum stops invalid values at the compiler or runtime — before they cause bugs.**

**Enum members can carry properties and methods, keeping related logic in one place.**

**Code becomes self-documenting: `OrderStatus.SHIPPED` is instantly readable. `2` is not.**

---

> Next question: What if a class could hide some of its internals and only expose what outside callers need? That idea is called **Encapsulation.**

*Next in the OOP series: Encapsulation — what doesn't need to be seen, shouldn't be*
