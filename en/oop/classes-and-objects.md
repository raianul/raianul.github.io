---
title: "Classes and Objects: One Mold, Thousand Pots"
tags: [Class, Object, OOP, LLD]
date: 2026-05-15
order: 1
read_time: "7 min"
description: "In a small pottery studio in Brooklyn, one wooden mold shapes thousands of clay pots. That mold is a Class. Each pot is an Object. OOP explained through a story."
excerpt: "In a small pottery studio in Brooklyn, one wooden mold shapes thousands of clay pots. That mold is a Class. Each pot is an Object."
---

On the corner of a Brooklyn side street, Mike runs a small pottery studio.

He makes clay pots. Has been for years.

In the back of the studio sits an old wooden mold. That mold is everything. It dictates how wide the mouth of each pot will be, how round its belly, how thick its base. The mold itself is not a pot. But without it, not a single pot gets made.

Every day, Mike uses that same mold to make many pots. One he glazes red, another slate gray. One small, one large. One goes to a kitchen in Manhattan, another becomes a flower planter on a brownstone stoop in Harlem.

Every pot is different. But every pot was born from the same mold.

That mold is a **Class**. Each pot is an **Object**.

---

## 1. What is a Class?

Take a closer look at Mike's mold.

The mold carries the definition of what every pot must have. A color. A height. A price. And what every pot can do: hold water, be sold.

In programming, a **Class** does exactly this. It is a blueprint — a mold — that defines what data a thing will have (called **attributes** or **fields**) and what it can do (called **methods**).

A Class is not a real thing itself. It is just a design. The way Mike's mold is not a pot, a Class is not a concrete object either.

{% include code-blocks/classes-and-objects/code-1.html %}

Here, `Pot` is the Class. `color`, `height`, `price` are its attributes — the pot's identity. `describe()` and `sell()` are its methods — what the pot can do.

But no pot exists yet. We have only built the mold. Now we need the actual pots.

---

## 2. What is an Object?

One morning, Mike sits down with his mold. The first pot gets a red glaze, 8 inches tall, priced at $45. The second gets slate gray, 6 inches, $30.

Two pots. Same mold. But completely different from each other.

In programming, each of these pots is an **Object** — also called an **Instance**. The act of creating an object from a class is called **Instantiation**.

{% include code-blocks/classes-and-objects/code-2.html %}

`pot1` and `pot2` are two separate objects. Changing the color of one doesn't affect the other. Each has its own data, its own existence.

{% include diagrams/classes-and-objects/diagram-1.html %}

You can create as many Objects from one Class as you want. Just as Mike can make a thousand pots from the same mold.

---

## 3. Attributes and Methods: What Makes a Pot a Pot

Every pot has two sides to it.

One is *who it is* — its color, its height, its price. These are its identity. In programming, these are called **attributes** or **instance variables**. Each object can have different values for the same attributes.

The other is *what it can do* — hold water, be sold, be broken. These are its capabilities. In programming, these are called **methods**. Methods are functions defined inside a Class that objects can call.

{% include code-blocks/classes-and-objects/code-3.html %}

`__init__` is a special method called the **constructor**. It runs automatically the moment an object is created. When Mike pours clay into the mold, the mold gives the pot its shape — that is exactly what the constructor does.

One thing worth remembering: `self`. It means "me, myself." Every time an object calls a method, it passes itself as `self`. So when you write `pot1.describe()`, `pot1` is saying: *"Run this with my own color, my own height, my own price."*

Once you understand this relationship between Class and Object, a natural question follows: can almost everything in the real world be modeled this way?

---

## Real-World Example: Drivers on Uber

Think about Uber. Millions of drivers on their platform. Each driver has a name, a phone number, a license plate, a rating. And each driver can accept a trip, cancel one, reach a destination.

Uber's engineers wrote one `Driver` Class. From that single Class, millions of Driver objects were created.

{% include code-blocks/classes-and-objects/code-4.html %}

```python
# Millions of driver objects from one class
driver1 = Driver("Mike Johnson", "212-555-0101", "NYC-4782")
driver2 = Driver("Sarah Chen", "917-555-0188", "NYC-9341", rating=4.8)

driver1.start_trip("Times Square")
# → Mike Johnson is now heading to Times Square.

driver1.end_trip()
driver1.update_rating(5.0)
# → Mike Johnson's new rating: 5.0
```

{% include diagrams/classes-and-objects/diagram-2.html %}

Uber's entire driver management system rests on this one idea. One Class, millions of Objects.

---

## Summary

| In the story | In code |
|---|---|
| Mike's clay pot mold in Brooklyn | Class |
| Each pot made from the mold | Object / Instance |
| Color, height, price of the pot | Attributes / Fields |
| What the pot can do | Methods |
| The moment a new pot is created | Instantiation |
| Pouring clay into the mold | Constructor (`__init__`) |
| "Me, myself" | `self` |

**A Class is a blueprint. It is not a real thing — just a design.**

**An Object is the real thing built from that blueprint. Each one has its own data.**

**You can create as many Objects from one Class as you want. Each is completely independent.**

**The constructor (`__init__`) runs at the moment of creation and sets up the initial data.**

---

> The next question is: should the data inside an object be open to everyone? Or is it wiser to keep some things hidden? That story is called **Encapsulation**.

*Next in the OOP series: Encapsulation — what not everyone gets to see*
