---
layout: post
title: "SOLID: ভাইয়ের রান্নাঘরে পাঁচটা নিয়ম"
description: "একটা রেস্টুরেন্ট, পাঁচটা সমস্যা, পাঁচটা সমাধান। SOLID Principles শেখো পুরান ঢাকার ভাইয়ের রান্নাঘরের গল্পে।"
series: "OOP সিরিজ"
series_label: "OOP"
category: oop
tags: [SOLID, SRP, OCP, LSP, ISP, DIP, OOP, LLD]
date: 2026-05-22
date_bn: "মে ২০২৬"
read_time: "১২ মিনিট"
order: 9
excerpt: "একটা রেস্টুরেন্ট, পাঁচটা সমস্যা, পাঁচটা সমাধান। SOLID Principles শেখো পুরান ঢাকার ভাইয়ের রান্নাঘরের গল্পে।"
---

পুরান ঢাকার পুরনো গলিতে "ভাইয়ের রান্নাঘর"।

দশ বছর ধরে চলছে। হাজী বিরিয়ানির পাশের গলিতে, নীল টিনের চাল। জামান ভাই নিজে শুরু করেছিলেন। প্রথমে ছোট ছিল। পাঁচটা টেবিল, দশটা item। এখন double হয়েছে।

কিন্তু সমস্যাও double হয়েছে।

সকালে ঢুকলে দেখবে জব্বার একা সব করছে: রান্না করছে, order নিচ্ছে, cash নিচ্ছে, আবার delivery-ও দিচ্ছে।

শুক্রবার lunch-এ order ভুল হলো। কাচ্চির জায়গায় তেহারি গেল। জামান ভাই জিজ্ঞেস করলেন: "কার ভুল?"

বলা গেল না। কারণ জব্বার একসাথে চারটা কাজ করছিল।

Software-এও এই সমস্যা হয়। একটা class যখন অনেক কিছু করে, bug হলে কোথায় সমস্যা বোঝা যায় না। এই পাঁচটা নিয়ম মিলে একটা নাম: **SOLID।**

পাঁচটা নিয়ম। পাঁচটা আলাদা সমস্যার সমাধান।

---

## ১. S: Single Responsibility Principle

জব্বার রান্না করে, order নেয়, cash নেয়, deliver করে। চারটা কাজ, একজন মানুষ।

Order ভুল হলো। কে দায়ী? রাঁধুনি জব্বার? Order-নেওয়া জব্বার? Cash-নেওয়া জব্বার? ধরার উপায় নেই।

জামান ভাই সিদ্ধান্ত নিলেন: ভাগ করে দিতে হবে।

রফিক শুধু রান্না করবে। মামুন শুধু order নেবে। সালমা শুধু cash নেবে। রিপন শুধু deliver করবে।

এখন কাচ্চির জায়গায় তেহারি গেলে: মামুনের ভুল। Cash কম পড়লে: সালমার ভুল। স্পষ্ট।

এই নিয়মের নাম **Single Responsibility Principle (SRP):** একটা class-এর পরিবর্তন হওয়ার একটাই কারণ থাকবে।

{% include code-blocks/solid/code-1.html %}

```python
chef     = Chef()
taker    = OrderTaker()
cashier  = Cashier()
delivery = DeliveryBoy()

chef.cook("kacchi")
taker.take_order("Jabbar")
cashier.calculate_bill("kacchi")
delivery.deliver("Islampur Road")
```

`KitchenWorker`-এ bug হলে চারটা জায়গায় খুঁজতে হবে। `Chef`, `OrderTaker`, `Cashier`, `DeliveryBoy` আলাদা করলে bug আছে কোথায় এক নজরে বোঝা যাবে।

{% include diagrams/solid/diagram-1.html %}

---

## ২. O: Open/Closed Principle

রমজান মাসে জামান ভাই ঠিক করলেন menu-তে বিরিয়ানি যোগ করবেন।

কিন্তু kitchen-এর system এমনভাবে লেখা যে নতুন item যোগ করতে গেলে পুরনো `prepare_item` function-এ হাত দিতে হবে। সেখানে আগে থেকে কাজ করা code আছে। একটু বেঠিক হলে কাচ্চি আর তেহারিও ভেঙে যাবে।

সমাধান: প্রতিটা item আলাদা class। নতুন item যোগ করতে হলে নতুন class বানাও। পুরনো কিছু ছুঁয়ো না।

এই নিয়মের নাম **Open/Closed Principle (OCP):** class পরিবর্তনের জন্য বন্ধ, কিন্তু extension-এর জন্য খোলা।

{% include code-blocks/solid/code-2.html %}

```python
kitchen = Kitchen()

kitchen.serve(Kacchi())
# Serving: slow-cooked kacchi biryani — $350.00

kitchen.serve(Tehari())
# Serving: tehari with beef — $200.00

kitchen.serve(Biryani())  # নতুন item, পুরনো code অপরিবর্তিত
# Serving: chicken biryani — $280.00
```

`Biryani` class যোগ করতে `Kitchen`-এ একটা line-ও বদলাতে হয়নি। **বাড়ানো গেছে, ভাঙা হয়নি।**

---

## ৩. L: Liskov Substitution Principle

জব্বার ছুটিতে গেল। রহিম ভাই এলেন substitute হিসেবে।

রহিম রান্না করতে পারেন, order নিতে পারেন। কিন্তু জব্বারের বিশেষ kacchi recipe জানেন না। শুক্রবারে regular customer এসে জিজ্ঞেস করল: "ভাইয়ের special kacchi আছে?"

রহিম বললেন: "আমি পারব না।"

Customer হতাশ হয়ে চলে গেল। Substitute কাজ করেনি।

Code-এও এই সমস্যা হয়। `SubstituteCook` যদি `Cook`-এর জায়গায় ব্যবহার করলে program ভেঙে পড়ে, তাহলে সে সত্যিকারের substitute নয়।

এই নিয়মের নাম **Liskov Substitution Principle (LSP):** subclass-কে parent class-এর জায়গায় ব্যবহার করলে program-এর behavior বদলাবে না।

{% include code-blocks/solid/code-3.html %}

```python
def start_shift(cook: Cook):
    cook.cook_standard_menu()
    cook.take_new_order("kacchi")

head = HeadCook("Jabbar")
sub  = SubstituteCook("Rahim")

start_shift(head)
# Jabbar: cooking full standard menu
# Jabbar: accepted order for kacchi

start_shift(sub)   # LSP satisfied — behavior identical
# Rahim: cooking standard menu items
# Rahim: accepted order for kacchi
```

`start_shift` function জানে না কে আসছে। `HeadCook` হোক বা `SubstituteCook`, দুজনেই একই কাজ করতে পারে। **Parent-এর promise, child পূরণ করবেই।**

---

## ৪. I: Interface Segregation Principle

রিপন নতুন delivery boy। জামান ভাই বললেন: "রান্নাঘরের সবাইকে একটা চুক্তি sign করতে হয়।"

চুক্তিতে লেখা: রান্না করতে জানতে হবে, order নিতে হবে, cash handle করতে হবে, আবার deliver-ও করতে হবে।

রিপন শুধু deliver করে। সে রান্না জানে না। কিন্তু চুক্তিতে আছে।

এটা অন্যায়। রিপনকে এমন কিছুর প্রতিশ্রুতি দিতে বলা হচ্ছে যেটা তার কাজের অংশই না।

এই নিয়মের নাম **Interface Segregation Principle (ISP):** client-কে এমন method implement করতে বাধ্য করা যাবে না যেটা সে ব্যবহার করে না।

{% include code-blocks/solid/code-4.html %}

```python
chef  = Chef()
ripon = DeliveryBoy()

chef.cook()
# Chef: cooking

chef.take_order()
# Chef: taking order

ripon.deliver()
# DeliveryBoy: out for delivery
```

`DeliveryBoy`-কে `cook()` বা `handle_billing()` implement করতে হয়নি। **যার যতটুকু দরকার, ততটুকুই চুক্তি।**

---

## ৫. D: Dependency Inversion Principle

রান্নাঘরের সব মশলা আসে "করিমের মশলার দোকান" থেকে। জামান ভাই সরাসরি করিমের নম্বরে call করেন।

একদিন করিম দোকান বন্ধ করে গ্রামে গেলেন। কোনো notice নেই। সেদিন রান্না বন্ধ।

সমস্যা: রান্নাঘর একটা নির্দিষ্ট দোকানের উপর নির্ভর করছে। দোকানের ধারণার উপর নয়।

সমাধান: "Supplier" ধারণার উপর নির্ভর করো। যে কেউ সেই ধারণা পূরণ করলে রান্নাঘর চলবে।

এই নিয়মের নাম **Dependency Inversion Principle (DIP):** high-level module abstract-এর উপর নির্ভর করবে, নির্দিষ্ট implementation-এর উপর নয়।

{% include code-blocks/solid/code-5.html %}

```python
karim_shop = KarimShop()
wholesale  = WholesaleMarket()

kitchen1 = Kitchen(supplier=karim_shop)
kitchen2 = Kitchen(supplier=wholesale)

kitchen1.cook()
# Cooking with: ['turmeric', 'cumin', 'cardamom']

kitchen2.cook()
# Cooking with: ['turmeric', 'cumin', 'cardamom', 'saffron']
```

`Kitchen` এখন যেকোনো `IngredientSupplier`-এর সাথে কাজ করতে পারে। করিমের দোকান বন্ধ হলে? নতুন supplier pass করো। **Kitchen-এর code একটা line-ও বদলাতে হবে না।**

{% include diagrams/solid/diagram-2.html %}

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| জব্বার একা সব করলে ভুল ধরা যায় না | SRP: একটা class-এর একটাই দায়িত্ব |
| বিরিয়ানি যোগে পুরো রান্নাঘর বদলাতে হয় | OCP: extend করো, modify করো না |
| substitute রাঁধুনি signature dish জানে না | LSP: subclass parent-এর জায়গায় কাজ করবে |
| delivery boy-কে রান্না শিখতে বাধ্য করা | ISP: শুধু যা দরকার, সেটুকুই implement করবে |
| নির্দিষ্ট মশলার দোকানের উপর নির্ভর | DIP: abstract-এ নির্ভর করো, concrete-এ নয় |

**SRP: একটা class-এর পরিবর্তন হওয়ার একটাই কারণ থাকবে।**

**OCP: নতুন feature মানে নতুন class, পুরনো class-এ হাত দেওয়া নয়।**

**LSP: subclass সবসময় parent class-এর জায়গায় ব্যবহারযোগ্য হবে, behavior অপরিবর্তিত।**

**ISP: বড় interface ভেঙে ছোট করো। যার যতটুকু দরকার, ততটুকুই দাও।**

**DIP: high-level module কখনো low-level module-এর উপর সরাসরি নির্ভর করবে না, abstraction-এর উপর করবে।**

---

> পরবর্তী প্রশ্ন: SOLID মানলে code ভালো হয়, কিন্তু বারবার আসা সমস্যাগুলোর জন্য কি কোনো ready-made সমাধান আছে? সেই সমাধানগুলোর নাম **Design Patterns।**

*OOP সিরিজের পরবর্তী পর্ব: Design Patterns — বারবার আসা সমস্যার চিরন্তন সমাধান*
