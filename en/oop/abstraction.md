---
title: "Abstraction: Tell It Where to Go, Not How to Get There"
tags: [Abstraction, OOP, Abstract Class, LLD]
date: 2026-05-19
order: 5
read_time: "7 min"
description: "Alex tells Lyft where he's going and stares out the window. He doesn't know the GPS algorithm. He doesn't need to. That's Abstraction — hiding complexity, showing only what matters."
excerpt: "Alex commutes to NYU three different ways each week. Same destination, three different engines, three different routing systems. But his experience is always the same: tell it where to go, get there. That's Abstraction."
---

Alex commutes to NYU every morning.

Monday, he hails a Yellow Cab. The driver asks: "Where to?" Alex says: "Washington Square Park." The driver fires up the engine, reads the traffic, calculates the fare. Alex looks out the window.

Wednesday, the MTA bus. Which gear it's in, how the brakes work, what the injectors are doing — he has no idea. He swipes his MetroCard, says "NYU," and sits down.

Friday, Lyft. He pins the destination in the app. How the GPS optimizes the route, which traffic layer it's reading, what the surge algorithm is doing — none of it is visible. He just knows: he'll get there.

Three different vehicles. Three different engines, technologies, routing systems. But Alex's experience is always the same: tell it where to go, get there.

That idea has a name: **Abstraction.**

---

## 1. What is Abstraction?

Alex uses three vehicles but stays completely free from their internal complexity. How many horsepower the cab has, what hydraulic pressure the bus brakes use, which satellite Lyft's GPS locks onto — he doesn't need to know any of it. He only talks to the interface: "Where are we going?"

In programming, **Abstraction** does exactly this: it hides complex internal implementation and only exposes what the caller actually needs.

Simple formula: **Abstraction = Hide the complexity + Show a simple interface.**

{% include diagrams/abstraction/diagram-1.html %}

The biggest benefit of Abstraction is **separating "what" from "how."** Alex knows *what* will happen (he'll reach his destination), but he doesn't need to know *how*. That separation is what makes large software systems manageable.

---

## 2. Abstract Class: A Shared Blueprint

Think about Alex's three vehicles. The Yellow Cab, the MTA bus, and Lyft all share certain behaviors — they all calculate a fare, they all give a trip summary. But how each one gets you there is completely different.

That's exactly the situation **Abstract Classes** are built for.

An abstract class provides a shared blueprint: it implements the parts that work the same for everyone, and for the parts that differ, it says "you handle that yourself."

{% include code-blocks/abstraction/code-1.html %}

`trip_summary()` is the same for every vehicle, so it lives in the abstract class — written once. But `travel()` and `get_fare()` differ for each, so `@abstractmethod` marks them as "each subclass must define this itself."

```python
class YellowCab(Transport):
    def travel(self, destination: str):
        print(f"Cab weaving through Midtown traffic to {destination}...")

    def get_fare(self, duration_min: int) -> float:
        return 3.00 + (duration_min * 0.50)  # base + time

class MTABus(Transport):
    def travel(self, destination: str):
        print(f"Bus rolling along its fixed route to {destination}...")

    def get_fare(self, duration_min: int) -> float:
        return 2.90  # flat MetroCard fare

class Lyft(Transport):
    def travel(self, destination: str):
        print(f"Lyft driver following GPS route to {destination}...")

    def get_fare(self, duration_min: int) -> float:
        return 5.00 + (duration_min * 0.75)  # base + time
```

Now look at Alex's commute in code:

```python
def commute(vehicle: Transport, destination: str, duration: int):
    vehicle.travel(destination)
    vehicle.trip_summary(destination, duration)

# Monday
commute(YellowCab("NYC Cab 4782"), "Washington Square Park", 20)
# Cab weaving through Midtown traffic to Washington Square Park...
# Vehicle: NYC Cab 4782 | Destination: Washington Square Park | Fare: $13.00

# Wednesday
commute(MTABus("M15 Bus"), "Washington Square Park", 35)
# Bus rolling along its fixed route to Washington Square Park...
# Vehicle: M15 Bus | Destination: Washington Square Park | Fare: $2.90

# Friday
commute(Lyft("Lyft"), "Washington Square Park", 15)
# Lyft driver following GPS route to Washington Square Park...
# Vehicle: Lyft | Destination: Washington Square Park | Fare: $16.25
```

The `commute()` function doesn't know any specific vehicle. It only knows the `Transport` interface. If an e-scooter service launches tomorrow, just add a new class. Not a single line of `commute()` changes.

{% include diagrams/abstraction/diagram-2.html %}

---

## 3. Abstraction vs. Encapsulation

We covered Encapsulation in the last article. The two sound similar, but they look at the problem from different angles.

**Encapsulation** says: hide the data, don't let anyone touch it directly. It's about internal protection — keeping `__balance` private, requiring a PIN to get in.

**Abstraction** says: hide the complexity, show a simple interface. It's about the external view — Alex just calls `travel("NYU")`, he doesn't need to know anything about the engine.

Using the car analogy: the accelerator pedal is Abstraction (press it, the rest isn't your concern), and the sealed engine housing is Encapsulation (you can't reach inside).

| Aspect | Encapsulation | Abstraction |
|---|---|---|
| Goal | Protect data | Hide complexity |
| Perspective | Inside out | Outside in |
| Example | Keeping `__balance` private | Exposing only `travel()` |
| Question | "Who can see this?" | "Does the caller need to know this?" |

They work together. Encapsulation protects. Abstraction simplifies.

---

## Real-World Example: DoorDash Notification System

When DoorDash confirms an order, they need to notify the customer. Three channels: SMS, push notification, email. Each works completely differently internally — but the job is the same: deliver the message.

{% include code-blocks/abstraction/code-2.html %}

{% include code-blocks/abstraction/code-3.html %}

```python
# notify via SMS
service = OrderService(SMSSender("Twilio SMS"))
service.confirm_order("DD-9921", "212-555-0199")

# switch to push notification — OrderService unchanged
service = OrderService(PushSender("Firebase"))
service.confirm_order("DD-9921", "device_token_xyz")
```

`OrderService` has no idea how the notification is being sent. It just calls `send()`. If DoorDash adds WhatsApp tomorrow, `OrderService` doesn't change. `log_attempt()` is written once — every sender inherits it automatically.

---

## Summary

| In the story | In code |
|---|---|
| Alex just says "Washington Square Park" | Calling an abstract method |
| Cab, bus, Lyft each travel differently | Concrete class implementations |
| Trip summary is the same for all vehicles | Concrete method in the abstract class |
| Alex doesn't know the engine, doesn't need to | Implementation hiding |
| New vehicle added, Alex's experience unchanged | Open for extension, closed for modification |

**Abstraction means separating "what" from "how." The caller knows what will happen — not how.**

**An abstract class writes shared behavior once. Subclasses inherit it and implement only their own unique parts.**

**Encapsulation protects data. Abstraction hides complexity. They work side by side.**

---

> The next question: what if a class could inherit all the properties of another class — and add its own on top? That idea is called **Inheritance**.

*Next in the OOP series: Inheritance — when the child gets the parent's traits*
