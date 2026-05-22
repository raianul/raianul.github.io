---
title: "Polymorphism: One Command, Fifty Different Jobs"
tags: [Polymorphism, OOP, "Method Overriding", "Method Overloading", LLD]
description: "Ray says one thing every morning: let's get to work. The sewers sew, the cutters cut, the packers pack. One command, fifty different jobs. That's Polymorphism."
date: 2026-05-21
date_en: "May 2026"
read_time: "7 min"
excerpt: "One command — and every person on the floor does something completely different. The story of Polymorphism, from a Brooklyn manufacturing plant."
---

Every morning at 8 AM, Ray walks onto the floor.

One loud announcement: **"Let's get to work!"**

The sewing machine operators thread their needles and start stitching. The cutting crew picks up shears and gets to the fabric. The QC team lines up by the conveyor belt, eyeing every piece. The packing crew unfolds boxes and starts folding garments.

One command. Fifty different jobs.

Ray didn't tell each person individually: "You stitch, you cut, you inspect." He just said "get to work." Everyone figured out the rest based on their own role.

In code, that idea is called **Polymorphism.**

---

## 1. What Is Polymorphism and Why Does It Matter?

Polymorphism means "many forms." Greek roots: *poly* (many) + *morphe* (form).

In programming it means: one method name or interface, but different classes implement it their own way.

Think about Ray's floor. You create a `Worker` class with a `start_work()` method. But a tailor's "work" and a QC inspector's "work" are completely different. So what do you do?

Without Polymorphism, you'd write separate methods for each type:

```python
def start_tailor_work(worker): ...
def start_cutting_work(worker): ...
def start_qc_work(worker): ...
```

As floor manager, Ray would have to think about every worker type separately. But with Polymorphism, he just says: **"Whoever you are, call `start_work()`."** Everyone figures out the rest themselves.

{% include diagrams/polymorphism/diagram-1.html %}

This idea works in two ways: **Compile-time Polymorphism** and **Runtime Polymorphism**. The first is simple, the second is where the real power lives.

---

## 2. Compile-time Polymorphism: Same Name, Different Input

Ray's factory has a `generate_report()` function. But:
- Pass just a date → get that day's full report
- Pass a date and section name → get that section's report
- Pass a month and year → get the monthly summary

Same name, different parameters. This is called **Method Overloading.**

In Java or C++ you write it directly:

```java
class ReportService {
    void generateReport(String date) { ... }
    void generateReport(String date, String section) { ... }
    void generateReport(int month, int year) { ... }
}
```

Python doesn't have native overloading, but default parameters get you there:

```python
def generate_report(date=None, section=None, month=None):
    if date and section:
        print(f"Report for {section} section on {date}")
    elif date:
        print(f"Full report for {date}")
    elif month:
        print(f"Monthly report for {month}")
```

Compile-time Polymorphism is mostly about keeping code clean. But OOP's real power shows up in Runtime Polymorphism.

---

## 3. Runtime Polymorphism: Where It Gets Real

Every morning Ray walks the floor roster and calls out once: **"Get to work."**

He doesn't know — and doesn't need to know — who's a tailor and who's QC. He just knows everyone is a `Worker`, and every `Worker` has a `start_work()`.

That's **Method Overriding** and Runtime Polymorphism.

{% include code-blocks/polymorphism/code-1.html %}

Now look at Ray's floor management code:

```python
floor = [
    Tailor("Marcus"),
    Cutter("Priya"),
    QCInspector("Derek"),
    Tailor("Sofia"),
]

for worker in floor:
    worker.start_work()  # one call, four different results
```

Output:
```
Marcus: machine on, stitching started
Priya: scissors up, cutting fabric
Derek: inspecting pieces for quality
Sofia: machine on, stitching started
```

Ray wrote `start_work()` once. Python figures out at runtime which version to run for each object. That's called **Dynamic Dispatch** — the core mechanism behind Runtime Polymorphism.

{% include diagrams/polymorphism/diagram-2.html %}

---

## 4. Polymorphism with Abstract Classes and Interfaces

Back in the [Abstraction](/en/oop/abstraction/) article, you saw how Abstract Classes work. Combine them with Polymorphism and things get even cleaner.

An Abstract Class lets you say: **"Every `Worker` must have `start_work()`. But what it does is up to you."**

{% include code-blocks/polymorphism/code-2.html %}

Now nobody can instantiate `Worker` directly. And if a subclass skips implementing `start_work()`, Python throws an error immediately. Abstract Class plus Polymorphism gives you that guarantee.

**Interface-style Polymorphism** works a bit differently. Say some workers on the floor are **Trainable** — they can pick up new skills. Not everyone, just some.

```python
class Trainable:
    def take_training(self):
        raise NotImplementedError


class Tailor(Worker, Trainable):
    def start_work(self):
        print(f"{self.name}: stitching started.")

    def take_training(self):
        print(f"{self.name} learning new machine operation.")
```

Now you can build a `trainable_list` and call `take_training()` on everyone in it — Polymorphism intact, training scoped only to those who qualify.

---

## Real-World Example: Amazon's Pricing System

Amazon carries three types of products: regular, discounted, and flash sale. Each calculates its price differently. But at checkout, there's a single call for all of them: **"What's the price?"**

{% include code-blocks/polymorphism/code-3.html %}

```python
cart = [
    RegularProduct("Merino wool scarf", 45.00),
    DiscountedProduct("Running shoes", 120.00, 20),   # 20% off
    FlashSaleProduct("Leather bag", 180.00, 79.99),   # flash sale price
]

total = 0
for product in cart:
    product.display()
    total += product.final_price()

print(f"\nTotal: ${total:.2f}")
```

Output:
```
Merino wool scarf: $45.00
Running shoes: $96.00
Leather bag: $79.99

Total: $220.99
```

The checkout code has no idea which product is regular and which is a flash sale. It just knows they're all `Product`, and every `Product` has a `final_price()`. Add a new product type — just create a new class. The checkout loop doesn't change at all. That's Polymorphism's real payoff.

---

## Summary

| In the story | In code |
|---|---|
| Ray's "get to work" | Polymorphic method call |
| Each worker interprets it their own way | Method Overriding |
| Same name, different parameters | Method Overloading |
| Ray doesn't know who does what | Dynamic Dispatch |
| Every `Worker` must have `start_work()` | Abstract Method |
| Amazon checkout — one loop | Polymorphic interface |

**Polymorphism reduces what the caller has to think about.** Ray and the Amazon checkout — neither wants to know what's happening inside. They just know what to call.

**Method Overriding is Runtime Polymorphism.** A child class rewrites the parent's method its own way, and Python picks the right version at runtime.

**Abstract classes and Polymorphism together are powerful.** Abstract methods guarantee every subclass implements the contract. Polymorphism guarantees you can treat them all the same way.

**Adding new types stays easy.** Adding `FlashSaleProduct` didn't touch the checkout code. That's the foundation of the Open/Closed Principle.

---

> Next question: We can build classes and objects, inherit, and use polymorphism. But what if there were proven, reusable patterns for solving common design problems? That idea is called **Design Patterns.**

*Next in the OOP series: Design Patterns — same problems, battle-tested solutions*
