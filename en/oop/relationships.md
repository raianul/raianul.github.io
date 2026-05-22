---
title: "Association, Aggregation, Composition: How Objects Know Each Other"
tags: [Association, Aggregation, Composition, Dependency, Realization, OOP, LLD]
description: "Not every object lives on its own island — some know each other, some hold each other, some can't survive without each other. Five relationships, one story."
date: 2026-05-22
order: 8
date_en: "May 2026"
read_time: "8 min"
excerpt: "Not every object lives on its own island. Some just know each other, some hold each other close, some can't exist without the other. Association, Aggregation, Composition, Dependency, and Realization — five relationships explained through a story."
lang: en
---

A side street in Flatbush, Brooklyn. Mike's Auto Repair. The kind of shop with the garage door always open and a coffee maker that never stops.

Walk in on a Tuesday and you'll see it all at once: a Honda up on the lift, two mechanics talking under the hood, the AutoZone rep dropping off parts, a diagnostic scanner getting passed across the floor, a blue-and-white AAA plaque hanging on the wall.

Five things happening at once. Five different kinds of relationships.

---

## 1. Association

Mike and Sarah have been doing business for three years.

Sarah runs the counter at the AutoZone three blocks down. Whenever Mike needs a part — alternator, water pump, brake pads — he calls her. She pulls it. Someone walks over to pick it up.

Neither of them created the other. Mike could order from another supplier tomorrow. Sarah has a hundred other accounts. But they know each other. They interact. The relationship is real.

In code, this is called **Association**.

Two classes are associated when one holds a reference to the other. Neither creates the other. Neither is responsible for what happens to the other when things end.

{% include code-blocks/relationships/code-1.html %}

```python
sarah = Supplier("Sarah")
james = Worker("James", supplier=sarah)

james.request_item("brake pads")
# Sarah: preparing brake pads
# James: received brake pads
```

`james` holds a reference to `sarah`. But if `james` ceases to exist, `sarah` doesn't disappear with him. That's the key: **independent lifecycles.**

---

## 2. Aggregation

The shop has three mechanics: James, Carlos, and Derek.

Mike schedules their shifts. Signs their checks. But if Mike closed up tomorrow — all three would still be mechanics. They'd find work at another shop. They existed before Mike hired them. They'll exist after.

This is **Aggregation**.

The whole (the `Shop`) has parts (the `Worker`s). But those parts can survive on their own. They aren't born inside the shop and they don't die with it.

{% include code-blocks/relationships/code-2.html %}

```python
james  = Worker("James")
carlos = Worker("Carlos")
derek  = Worker("Derek")

mikes = Shop("Mike's Auto Repair", workers=[james, carlos, derek])
mikes.open()
# James is working
# Carlos is working
# Derek is working
```

The `workers` list is passed in from outside. `Shop` didn't create those objects. If `mikes` goes away, James, Carlos, and Derek are still standing. **The whole holds the parts, but the parts survive on their own.**

But the service bays built into the garage floor? That's a different story.

---

## 3. Composition

The garage has four service bays.

Bay 1 through Bay 4. Each has a lift, a drain pan, a tool rack, overhead lights. They're physically built into the building.

If Mike sold the shop and someone tore the building down, those bays don't just relocate. They don't exist independently. The moment the shop is gone, the bays are gone too.

This is **Composition**.

Same "has-a" shape as Aggregation on the surface. But now the whole creates the parts. The whole owns the parts. The whole's death is the parts' death.

{% include code-blocks/relationships/code-3.html %}

```python
mikes = Shop("Mike's Auto Repair")
mikes.accept_job("Honda-NYC-4782")
# Station #1: working on Honda-NYC-4782
```

`Shop` builds the `WorkStation` objects inside its own constructor. When the `Shop` is gone, those station objects go with it.

**Aggregation vs Composition — one question decides it:** did the whole create the part, or was the part brought in from outside?

{% include diagrams/relationships/diagram-1.html %}

---

## 4. Dependency

A customer pulls in driving a beat-up Camry. Carlos takes the keys.

First thing he does: grabs the diagnostic spec sheet from the front desk. Checks the error codes, maps them to the repair needed, writes it up. Puts the sheet back.

He doesn't own it. Doesn't store it anywhere. Just borrowed it long enough to do the job.

In code, this is called **Dependency**.

It's the weakest relationship of the five. One class uses another temporarily — as a method parameter, or a local variable. No reference stored on the object. No shared lifecycle. Pick it up, use it, let it go.

{% include code-blocks/relationships/code-4.html %}

```python
spec   = Specification()
carlos = Worker("Carlos")

carlos.process_order("Camry-NYC-9812", spec)
# Looking up spec for Camry-NYC-9812...
# Carlos: processing order Camry-NYC-9812
#   measurement_a: 40
#   measurement_b: 34
#   measurement_c: 16
```

`Specification` doesn't appear anywhere in `Worker.__init__`. It shows up only inside `process_order` — passed in, used, done. **The dependency exists only for the duration of that method call.**

In UML, this is drawn as a dashed arrow. The lightest touch.

---

## 5. Realization

On the wall near the entrance: a blue-and-white plaque. **AAA Approved Auto Repair.**

Getting that plaque wasn't free. Mike signed an agreement. A contract. Transparency in written estimates. OEM-quality parts. ASE-certified technicians. A loaner vehicle policy.

The plaque is a statement: "We have fulfilled every item on that list."

In code, this is **Realization**.

A class **realizes** an interface — it signs the contract. The interface says what must be done. The class decides how. No implementation lives in the interface. Just the commitment.

{% include code-blocks/relationships/code-5.html %}

```python
mikes = Workshop()
mikes.safe_environment()
mikes.fair_wages("James")
mikes.quality_output("Honda-NYC-4782")
# Fire exits clear, safety equipment in place
# James: paid on time
# Order Honda-NYC-4782: completed to standard
```

Miss even one method and Python raises a `TypeError` at instantiation — the contract isn't fulfilled. In Java it's called `implements`. In Swift it's a protocol. Same idea everywhere: **a class that realizes an interface is making a promise it cannot break.**

---

## All Five at Once

{% include diagrams/relationships/diagram-2.html %}

Five arrows. Five different weights.

**Solid line:** Association. **Hollow diamond:** Aggregation. **Filled diamond:** Composition. **Dashed arrow:** Dependency. **Dashed arrow with hollow triangle:** Realization.

The diamond tells you ownership. The fill tells you how deep that ownership goes. The dashes tell you it's temporary.

---

## Summary

| In the story | In code |
|---|---|
| Mike knowing Sarah at AutoZone | Association — one class holds a reference to another |
| James, Carlos, Derek working at the shop | Aggregation — whole holds parts that exist independently |
| Service bays built into the building | Composition — whole creates and owns its parts |
| Carlos borrowing the spec sheet | Dependency — one class uses another only inside a method |
| The AAA certification plaque | Realization — a class implements an interface's contract |
| Shop closes, mechanics move on | Aggregation: parts survive the whole |
| Shop closes, bays disappear | Composition: parts die with the whole |

**Association is the loosest bond — two objects know each other, but neither owns the other.**

**Aggregation and Composition both say "has-a" — the difference is who created the part and whether it can live on its own.**

**Dependency is temporary — it exists only inside a method call, never stored on the object.**

**Realization is a promise — the class commits to every method in the interface, no exceptions.**

---

> Next question: now that objects have relationships — how do we structure the code that wires them all together, without things falling apart every time something changes? That idea lives in the **SOLID Principles.**

*Next in the OOP series: SOLID — five rules for code that holds up*
