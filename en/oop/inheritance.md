---
title: "Inheritance: Dad's a GP, Son's a Cardiologist"
tags: [Inheritance, OOP, LLD, "Code Reuse"]
date: 2026-05-20
date_en: "May 2026"
read_time: "7 min"
excerpt: "Dr. Frank Miller trained two residents at Mount Sinai. One went into cardiology, the other into surgery. Both took everything he taught them — then added their own expertise on top. That's Inheritance."
---

Dr. Frank Miller runs a practice near Mount Sinai, Upper East Side.

Thirty years in medicine. General practitioner. Whoever walks in, he sees them: checks vitals, diagnoses the problem, writes the prescription.

His former resident James Chen trained under him. Spent years watching how Frank worked — how to read a patient, how to measure blood pressure, how to write a prescription. Then James specialized in cardiology. Now he runs his own clinic on the West Side. He can read ECGs, interpret angiograms. Those skills didn't come from Dr. Miller — James built them himself.

His other resident, Sarah Rodriguez, learned the same fundamentals. Vitals, diagnosis, prescription. Then she went into surgery. Now she's the one in charge of the OR at NYU Langone. She can suture wounds, perform appendectomies. Skills she developed on her own.

Three doctors. All three know the same core work. James and Sarah took everything Frank taught them and layered their own specializations on top.

That concept is called **Inheritance.**

---

## 1. What Is Inheritance and Why Does It Matter?

If Dr. Miller had to teach James and Sarah completely separately — duplicating everything from vitals to prescriptions — imagine the wasted time. And if he ever found a mistake in how he taught the basics, he'd have to correct it in two places.

In programming, **Inheritance** solves exactly this problem. Whatever a class (parent or superclass) knows, another class (child or subclass) gets automatically. The child then adds its own specialized behavior.

Inheritance delivers four major benefits.

First, **code reuse**: write common logic once, every child class gets it. Second, **natural hierarchy**: the real-world relationship "Cardiologist is-a Doctor" shows up directly in the code. Third, **easier maintenance**: fix a bug in the parent and it's fixed everywhere. Fourth, **the foundation for Polymorphism**: we'll see that in the next article.

{% include diagrams/inheritance/diagram-1.html %}

---

## 2. How It Works in Code

Here's the `Doctor` parent class:

{% include code-blocks/inheritance/code-1.html %}

Now both James and Sarah inherit from `Doctor` and add their own specialties:

{% include code-blocks/inheritance/code-2.html %}

```python
james = Cardiologist("Dr. James Chen")
sarah = Surgeon("Dr. Sarah Rodriguez", "OR Suite 7")

# Methods inherited from Dr. Miller
james.check_vitals("Mr. Thompson")
james.prescribe("Mr. Thompson", "Aspirin")

# James's own method
james.read_ecg("Mr. Thompson")

# Sarah got the parent methods too
sarah.check_vitals("Ms. Garcia")
sarah.perform_surgery("Ms. Garcia", "Appendectomy")
```

`super().__init__()` is how the child calls the parent's constructor. It's saying: "I'm taking everything the parent had first."

A child can also **override** a parent method. Say James handles diagnosis differently for cardiac cases:

{% include code-blocks/inheritance/code-3.html %}

Now when you call `james.diagnose("chest pain")`, it runs James's version — not the parent's.

---

## 3. Types of Inheritance

**Single Inheritance** is the simplest: one child, one parent. `Cardiologist extends Doctor`. Supported in every language.

**Hierarchical Inheritance** means multiple children from the same parent. Both James and Sarah inherit from Dr. Miller. This is the most common pattern.

**Multi-level Inheritance** is a chain: parent → child → grandchild. Dr. Miller trains James, James later trains a fellow who becomes an Interventional Cardiologist.

{% include diagrams/inheritance/diagram-2.html %}

**Multiple Inheritance** (one child, multiple parents) is possible in Python but dangerous. Say James wants to inherit from both `Doctor` and `Researcher`. If both have a `publish()` method, which one does James get? That's called the **Diamond Problem**. This is why Java and C# don't let you extend multiple classes — but you can implement multiple interfaces.

---

## 4. When to Use Inheritance

One simple rule: **"is-a" relationship → use inheritance. "has-a" relationship → don't.**

Cardiologist **is-a** Doctor. ✅ Inheritance makes sense.
Car **has-a** Engine. ❌ Use composition instead.

| Use It | Avoid It |
|---|---|
| Clear "is-a" relationship | "has-a" or "uses-a" relationship |
| Need to share common behavior | Need to swap behavior at runtime |
| Shallow hierarchy (2–3 levels) | Deep hierarchy (5+ levels) |
| All parent behavior is valid in the child | Child breaks or ignores parent methods |

When in doubt, start with composition. Moving to inheritance later is easy. But unwinding a deep inheritance tree back into composition is painful.

---

## Real-World Example: Brooklyn Manufacturing Plant Staff System

A factory in Brooklyn has different types of staff. Everyone is an employee — everyone punches in and out, everyone gets paid. But a Line Worker, a Quality Inspector, and a Floor Supervisor all do different jobs.

{% include code-blocks/inheritance/code-4.html %}

```python
class LineWorker(Employee):
    def __init__(self, name: str, employee_id: str, salary: float, station: int):
        super().__init__(name, employee_id, salary)
        self.station = station

    def operate_machine(self):
        print(f"{self.name} running station #{self.station}")

    def report_output(self, units: int):
        print(f"{self.name}: produced {units} units today")


class QualityInspector(Employee):
    def __init__(self, name: str, employee_id: str, salary: float):
        super().__init__(name, employee_id, salary)

    def inspect_batch(self, batch_id: str) -> bool:
        print(f"{self.name}: inspecting batch #{batch_id}")
        return True

    def reject_item(self, item_id: str, reason: str):
        print(f"{self.name}: rejected item #{item_id} — {reason}")


class FloorSupervisor(LineWorker):   # multi-level: FloorSupervisor is-a LineWorker is-a Employee
    def __init__(self, name: str, employee_id: str, salary: float, station: int, team_size: int):
        super().__init__(name, employee_id, salary, station)
        self.team_size = team_size

    def approve_leave(self, worker_name: str):
        print(f"{self.name}: approved leave for {worker_name}")

    def daily_report(self, total_output: int):
        print(f"{self.name}'s team: {total_output} units today, {self.team_size} workers")
```

```python
marcus = LineWorker("Marcus Johnson", "W-101", 52000, station=5)
priya  = QualityInspector("Priya Patel", "Q-203", 61000)
derek  = FloorSupervisor("Derek Williams", "S-301", 85000, station=1, team_size=12)

# Everyone gets Employee methods
marcus.clock_in()
priya.clock_in()
derek.clock_in()

# Their own methods
marcus.operate_machine()
priya.inspect_batch("B-4421")
derek.approve_leave("Marcus Johnson")

# FloorSupervisor is-a LineWorker, so this works too
derek.operate_machine()
derek.daily_report(480)
```

When a new role comes along — say `AccountsClerk` — just extend `Employee`. All existing code stays untouched.

---

## Summary

| In the story | In code |
|---|---|
| Learning the basics from Dr. Miller | Inheriting methods from the parent class |
| James specializing in cardiology | Adding new methods in the child class |
| James diagnosing his own way | Overriding a method |
| James "is-a" Doctor | The "is-a" relationship |
| Both James and Sarah trained under Dr. Miller | Hierarchical inheritance |
| James's fellow becomes a sub-specialist | Multi-level inheritance |

**Inheritance means taking everything from the parent and adding your own specialization on top.**

**Use inheritance for "is-a" relationships, composition for "has-a".**

**Keep hierarchies shallow (2–3 levels) — deep trees become hard to understand and maintain.**

---

> Next question: If you hand Dr. Miller, James, and Sarah to a single method — will each one behave in their own way? That idea is called **Polymorphism.**

*Next in the OOP series: Polymorphism — one call, different behaviors*
