---
title: "SOLID: Five Rules From Marco's Kitchen"
description: "One restaurant, five problems, five solutions. SOLID Principles explained through Marco's Kitchen in Carroll Gardens, Brooklyn."
tags: [SOLID, SRP, OCP, LSP, ISP, DIP, OOP, LLD]
date: 2026-05-22
order: 9
date_en: "May 2026"
read_time: "12 min"
excerpt: "One restaurant, five problems, five solutions. SOLID Principles explained through Marco's Kitchen in Carroll Gardens, Brooklyn."
lang: en
---

Carroll Gardens, Brooklyn. Marco's Kitchen on Smith Street.

Ten years running. The kind of place with handwritten specials on a chalkboard and a line out the door on Friday nights. Marco started it himself. Five tables, ten items. Now it's doubled.

So have the problems.

Walk in on any given morning and Tony is doing everything: cooking, taking orders, running the register, and handling deliveries. When a Friday order got mixed up, nobody could say which "Tony" made the mistake.

Software has the same problem. When one class does too many things, bugs get lost. These five principles together are called **SOLID.**

Five rules. Five different problems. Five solutions.

---

## 1. S: Single Responsibility Principle

Tony cooks, takes orders, handles cash, and delivers. Four jobs, one person.

An order gets mixed up. Who's responsible? Cook Tony? Order-taker Tony? Cashier Tony? There's no way to tell.

Marco makes a decision: split it up.

Rafael only cooks. Carlos only takes orders. Maria only handles the register. Luis only delivers.

Now when an order goes wrong: Carlos's mistake. Short on cash: Maria's mistake. Clear.

This principle is called the **Single Responsibility Principle (SRP):** a class should have only one reason to change.

{% include code-blocks/solid/code-1.html %}

```python
chef     = Chef()
taker    = OrderTaker()
cashier  = Cashier()
delivery = DeliveryBoy()

chef.cook("kacchi")
taker.take_order("James")
cashier.calculate_bill("kacchi")
delivery.deliver("Smith Street")
```

With `KitchenWorker`, a bug could hide in four places. Split into `Chef`, `OrderTaker`, `Cashier`, `DeliveryBoy` — you know exactly where to look.

{% include diagrams/solid/diagram-1.html %}

---

## 2. O: Open/Closed Principle

Marco wants to add weekend brunch to the menu.

But the kitchen system is written so tightly that adding eggs benedict means touching the existing `prepare_item` function. That function already handles the lunch menu. Touch it wrong and the whole lunch service breaks.

The fix: each menu item is its own class. Adding something new means adding a new class. Nothing existing gets touched.

This principle is called the **Open/Closed Principle (OCP):** open for extension, closed for modification.

{% include code-blocks/solid/code-2.html %}

```python
kitchen = Kitchen()

kitchen.serve(Kacchi())
# Serving: slow-cooked kacchi biryani — $350.00

kitchen.serve(Tehari())
# Serving: tehari with beef — $200.00

kitchen.serve(Biryani())  # new item — zero existing code changed
# Serving: chicken biryani — $280.00
```

Adding `Biryani` didn't touch a single line inside `Kitchen`. **Extended without breaking.**

---

## 3. L: Liskov Substitution Principle

Tony calls in sick. Marco brings in Luis as a substitute.

Luis can cook the standard menu. But he doesn't know Tony's Friday pasta special — the one regulars order every week. A customer asks for it. Luis says he can't make it.

The customer leaves. The substitute didn't fully substitute.

The same failure happens in code. If a `SubstituteCook` breaks the program when used in place of `Cook`, it isn't really a substitute.

This principle is called the **Liskov Substitution Principle (LSP):** a subclass must be usable in place of its parent class without changing the program's behavior.

{% include code-blocks/solid/code-3.html %}

```python
def start_shift(cook: Cook):
    cook.cook_standard_menu()
    cook.take_new_order("pasta")

head = HeadCook("Tony")
sub  = SubstituteCook("Luis")

start_shift(head)
# Tony: cooking full standard menu
# Tony: accepted order for pasta

start_shift(sub)   # LSP satisfied — behavior identical
# Luis: cooking standard menu items
# Luis: accepted order for pasta
```

`start_shift` doesn't care who shows up. `HeadCook` or `SubstituteCook`, both deliver. **Whatever the parent promises, the child keeps.**

---

## 4. I: Interface Segregation Principle

Marco hires a new delivery driver. To be on the team, the driver has to sign the "kitchen staff contract."

The contract says: must know how to cook, take orders, handle billing, and deliver.

The driver just drives. He doesn't know how to cook. But the contract requires it.

That's not fair. He's being forced to commit to things that have nothing to do with his job.

This principle is called the **Interface Segregation Principle (ISP):** a client should not be forced to implement methods it doesn't use.

{% include code-blocks/solid/code-4.html %}

```python
chef  = Chef()
carlos = DeliveryBoy()

chef.cook()
# Chef: cooking

chef.take_order()
# Chef: taking order

carlos.deliver()
# DeliveryBoy: out for delivery
```

`DeliveryBoy` never had to implement `cook()` or `handle_billing()`. **Each role signs only the contract it needs.**

---

## 5. D: Dependency Inversion Principle

Every morning Marco calls Vinnie's Produce directly for vegetables and herbs.

One morning Vinnie retires. No call, no notice. Marco has no produce. Kitchen closes for the day.

The problem: Marco's kitchen depends on a specific shop, not on the idea of a supplier.

The fix: depend on the concept of a supplier. Whoever fulfills that role can step in.

This principle is called the **Dependency Inversion Principle (DIP):** high-level modules should depend on abstractions, not on concrete implementations.

{% include code-blocks/solid/code-5.html %}

```python
vinnies   = KarimShop()       # same interface, different name
wholesale = WholesaleMarket()

kitchen1 = Kitchen(supplier=vinnies)
kitchen2 = Kitchen(supplier=wholesale)

kitchen1.cook()
# Cooking with: ['turmeric', 'cumin', 'cardamom']

kitchen2.cook()
# Cooking with: ['turmeric', 'cumin', 'cardamom', 'saffron']
```

`Kitchen` works with any `IngredientSupplier`. Vinnie retires? Pass in a new supplier. **Not one line of `Kitchen` changes.**

{% include diagrams/solid/diagram-2.html %}

---

## Summary

| In the story | In code |
|---|---|
| Tony doing everything means no one's accountable | SRP: one class, one responsibility |
| Adding biryani means rewriting the order flow | OCP: extend, don't modify |
| The substitute doesn't know the Friday special | LSP: subclass must work wherever the parent works |
| The driver forced to learn cooking | ISP: implement only what you need |
| Depending on one specific produce shop | DIP: depend on abstractions, not concrete classes |

**SRP: a class should have only one reason to change.**

**OCP: new features mean new classes, not edits to existing ones.**

**LSP: a subclass must always be substitutable for its parent without breaking behavior.**

**ISP: break large interfaces into small ones. Each role takes only what it needs.**

**DIP: high-level modules depend on abstractions, never directly on low-level implementations.**

---

> Next question: SOLID keeps code clean, but when the same problems keep coming up across different projects — is there a catalog of ready-made solutions? Those solutions are called **Design Patterns.**

*Next in the OOP series: Design Patterns — proven solutions to recurring problems*
