---
title: "Interface: Pathao জানে না bKash কীভাবে কাজ করে, তবু পেমেন্ট হয়"
tags: [Interface, OOP, Abstraction]
date: 2026-05-17
date_bn: "মে ২০২৬"
read_time: "৫ মিনিট"
description: "bKash, Nagad, Rocket তিনটা আলাদা system। Pathao কীভাবে সবগুলো সামলায় একটা অক্ষরও না বদলে? বাংলায় OOP Interface শেখো।"
excerpt: "bKash, Nagad, Rocket তিনটা আলাদা system। Pathao কীভাবে সবগুলো সামলায় একটা অক্ষরও না বদলে? এটাই Interface-এর জাদু।"
---

Pathao-এ ride নিলে শেষে payment দিতে হয়।

bKash, Nagad, Rocket, Credit Card, যেটা খুশি বেছে নাও। Pathao-এর app-এ সব কটা option আছে।

এখন ভাবো: Pathao-এর developer-রা কি bKash-এর পুরো system জানে? Nagad কীভাবে তাদের server-এ transaction process করে, সেটা কি Pathao-এর code-এ লেখা আছে?

না।

Pathao শুধু একটাই কথা জানে: "আমাকে `processPayment(amount)` বলো, বাকিটা তুমি সামলাও।" bKash সেটা নিজের মতো করে। Nagad নিজের মতো করে। Pathao-এর কিছু যায় আসে না।

এই চুক্তিটার নাম **Interface।**

---

## ১. Interface কী?

ধরো তুমি একটা নতুন রেস্টুরেন্টে কাজ করতে এসেছ। মালিক বললেন: "তোমাকে তিনটা কাজ করতে হবে: অর্ডার নাও, খাবার দাও, বিল নাও। কীভাবে করবে সেটা তোমার ব্যাপার।"

এই তিনটা কাজের তালিকাটাই হলো Interface। মালিক বলে দিচ্ছেন **কী করতে হবে**, কিন্তু **কীভাবে করতে হবে** সেটা বলছেন না।

প্রোগ্রামিং-এ **Interface** হলো একটা contract বা চুক্তি। এটা define করে কোনো class-কে কী কী method implement করতে হবে, কিন্তু সেই method-এর ভেতরে কী হবে সেটা Interface-এর কাজ না।

{% include code-blocks/interfaces/code-1.html %}

`PaymentGateway` Interface বলছে: "যে class আমাকে implement করবে, তাকে অবশ্যই `process_payment`, `refund`, আর `get_transaction_status` রাখতে হবে।" ভেতরে কী হবে? সেটা implementer-এর স্বাধীনতা।

{% include diagrams/interfaces/diagram-1.html %}

---

## ২. Interface-এর মূল বৈশিষ্ট্য

**শুধু কী, কীভাবে না।** Interface-এ method-এর signature থাকে, implementation থাকে না। রেস্টুরেন্টের মেনু যেমন বলে "বিরিয়ানি আছে", রান্নার recipe দেয় না।

**একটা class একাধিক Interface implement করতে পারে।** একজন waiter যেমন অর্ডার নেওয়া, বিল দেওয়া, টেবিল সাজানো সব কাজই করতে পারে। একটা class-ও একাধিক চুক্তি মানতে পারে।

**Interface নিজে object তৈরি করতে পারে না।** মেনু দেখে খাবার আসে না, রাঁধুনি রান্না করে। Interface থেকে directly object বানানো যায় না।

**Loose coupling তৈরি হয়।** Pathao-এর code bKash-এর উপর depend করে না, করে `PaymentGateway` Interface-এর উপর। bKash বদলে গেলেও Pathao-এর core code ছুঁতে হয় না।

---

## ৩. Code Example: Payment Gateway

{% include code-blocks/interfaces/code-2.html %}

এখন দেখো `RideCheckout` class কতটা স্বাধীন:

```python
# bKash দিয়ে
checkout = RideCheckout(WalletPayment())
checkout.complete_ride(85.0)
# Wallet: processing payment of 85.0...
# Ride complete! Payment processed.

# Nagad দিয়ে
checkout = RideCheckout(DirectDebitPayment())
checkout.complete_ride(85.0)
# Direct debit: processing payment of 85.0...
# Ride complete! Payment processed.
```

`RideCheckout`-এর একটা অক্ষরও বদলাতে হয়নি। কাল যদি Rocket যোগ হয়, `RocketPayment` class বানাও, `RideCheckout` ছুঁতে হবে না।

---

## বাস্তব উদাহরণ: Notification Service

Shohoz Food-এ একটা order confirm হলে customer-কে notify করতে হয়। SMS দিয়ে, Email দিয়ে, আর app-এ Push notification দিয়ে।

তিনটা আলাদা service, তিনটা আলাদা company-র API। কিন্তু কাজ একটাই: "এই message-টা user-কে পাঠাও।"

{% include code-blocks/interfaces/code-3.html %}

{% include diagrams/interfaces/diagram-2.html %}

```python
processor = OrderProcessor([
    SmsNotification(),
    EmailNotification(),
    PushNotification()
])

processor.confirm_order("01711-XXXXX", "SHZ-4521")
# SMS to 01711-XXXXX: Order #SHZ-4521 confirmed!
# Email to 01711-XXXXX: Order #SHZ-4521 confirmed!
# Push notification to 01711-XXXXX: Order #SHZ-4521 confirmed!
```

কাল যদি WhatsApp notification যোগ করতে হয়, শুধু `WhatsAppNotification` class বানাও। `OrderProcessor`-এর কিছু বদলাতে হবে না।

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| রেস্টুরেন্টের মেনু | Interface |
| মেনুতে লেখা তিনটা কাজ | Abstract methods |
| ওয়েটার যে সেই কাজ করে | Implementing class |
| Pathao-এর payment চুক্তি | PaymentGateway interface |
| bKash, Nagad নিজের মতো করে | Concrete implementations |
| নতুন payment method যোগ করা সহজ | Loose coupling |

**Interface বলে কী করতে হবে, কীভাবে করতে হবে সেটা implementing class-এর কাজ।**

**Interface-এর কারণে নতুন implementation যোগ করতে পুরনো code ছুঁতে হয় না।**

**একটা class একাধিক Interface implement করতে পারে, flexibility অনেক বেশি।**

---

> পরবর্তী প্রশ্ন: Object-এর ভেতরের data কি সবার জন্য উন্মুক্ত থাকা উচিত? কিছু জিনিস কি লুকিয়ে রাখা উচিত না? সেই ধারণার নাম **Encapsulation।**

*OOP সিরিজের পরবর্তী পর্ব: Encapsulation, যা সবাইকে দেখানো যায় না*
