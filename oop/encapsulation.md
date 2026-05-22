---
title: "Encapsulation: তোমার bKash balance কেউ সরাসরি ছুঁতে পারে না"
tags: [Encapsulation, OOP, "Access Modifiers", "Data Hiding"]
date: 2022-11-15
date_bn: "নভেম্বর ২০২২"
read_time: "৬ মিনিট"
description: "bKash-এ টাকা আছে, কিন্তু সেই balance variable-এ সরাসরি হাত দেওয়া যায় না। এটাই Encapsulation — data hiding আর controlled access বাংলায়।"
excerpt: "bKash-এ টাকা আছে, কিন্তু সেই balance variable-এ সরাসরি হাত দেওয়া যায় না। PIN লাগবে, নির্দিষ্ট পথ লাগবে। এটাই Encapsulation , data hiding আর controlled access।"
---

তোমার bKash-এ টাকা আছে।

কিন্তু ভাবো একটু। সেই টাকাটা আসলে কোথায় আছে? কোনো server-এ। কোনো database-এ। একটা variable-এ। হয়তো `balance = 5420.50`।

তুমি কি সেই variable-এ সরাসরি হাত দিতে পারো? গিয়ে লিখতে পারো `balance = 999999`?

পারো না।

তোমাকে নির্দিষ্ট পথ দিয়ে আসতে হবে। টাকা পাঠাতে হলে "Send Money" তে যাও। বের করতে হলে "Cash Out"। জানতে হলে "Check Balance"। PIN ছাড়া কিছুই হবে না। আর ভেতরে কীভাবে সব হচ্ছে সেটা তুমি জানো না, জানার দরকারও নেই।

এই ব্যবস্থার নাম **Encapsulation।**

---

## ১. Encapsulation কী?

সহজ করে বললে: **Encapsulation = Data Hiding + Controlled Access।**

একটা class-এর ভেতরে data (variables) আর behavior (methods) একসাথে রাখা, এবং সেই data-তে সরাসরি access না দিয়ে শুধু নির্দিষ্ট পথ দিয়ে access দেওয়া।

bKash-এর ATM analogy ভাবো। তুমি সরাসরি bank-এর vault-এ ঢুকে balance বদলাতে পারো না। ATM-এর মাধ্যমে যাও। ATM তোমাকে তিনটাই দেয়: `deposit()`, `withdraw()`, `get_balance()`। ভেতরে কী হচ্ছে? সেটা তোমার ব্যাপার না।

{% include diagrams/encapsulation/diagram-1.html %}

**Encapsulation কেন দরকার?**

প্রথমত, sensitive data লুকিয়ে রাখা যায়। Balance, password, card number সরাসরি দেখা যাবে না।

দ্বিতীয়ত, controlled validation। কেউ `-500` টাকা deposit করতে চাইলে আটকানো যাবে। Method-এর ভেতরে rule থাকবে।

তৃতীয়ত, maintainability। ভেতরের implementation বদলালেও বাইরের code ভাঙবে না। bKash চাইলে তাদের balance `integer` থেকে `float`-এ বদলাতে পারে, তুমি টেরও পাবে না।

---

## ২. Access Modifiers: কতটুকু দেখাবে?

Encapsulation implement করার মূল হাতিয়ার হলো **Access Modifiers।** এগুলো নিয়ন্ত্রণ করে class-এর কোন অংশ বাইরে থেকে দেখা যাবে।

তিনটা মূল modifier:

**`private`:** শুধু ওই class-এর ভেতরে। সবচেয়ে বেশি ব্যবহার হয় data লুকাতে। বাইরে থেকে কেউ ছুঁতে পারবে না।

**`protected`:** ওই class আর তার subclass-এ। Inheritance-এ কাজে লাগে।

**`public`:** সবাই দেখতে পারে। এটাই controlled interface।

সহজ নিয়ম: **সব কিছু default-এ private রাখো, দরকার হলে public করো।**

{% include code-blocks/encapsulation/code-1.html %}

Python-এ `__` দিয়ে private করলে সরাসরি access block হয়ে যায়। দেখো কী হয়:

{% include code-blocks/encapsulation/code-2.html %}

---

## ৩. Getters এবং Setters: নিয়ন্ত্রিত দরজা

Private data পড়তে এবং লিখতে যে public methods ব্যবহার হয় সেগুলোকে বলে **Getter** আর **Setter।**

**Getter:** শুধু পড়তে দেয়। `get_balance()` balance দেখায়, কিন্তু বদলাতে দেয় না।

**Setter:** লিখতে দেয়, কিন্তু validation সহ। Invalid value ঢুকতে পারে না।

{% include code-blocks/encapsulation/code-3.html %}

Setter-এর কারণে `account_holder = ""` বা `balance = -500` কখনো set হবে না।

---

## বাস্তব উদাহরণ: Payment Processor

এই example-টা সবচেয়ে শক্তিশালী।

তুমি একটা payment system বানাচ্ছ। Customer-এর credit card number system-এ রাখতে হবে। কিন্তু পুরো number কখনো store করা যাবে না, এটা security risk। শুধু masked version রাখতে হবে: `****-****-****-1234`।

Encapsulation এই সমস্যার perfect সমাধান:

{% include code-blocks/encapsulation/code-4.html %}

{% include diagrams/encapsulation/diagram-2.html %}

{% include code-blocks/encapsulation/code-5.html %}

এই design-এর সৌন্দর্য কোথায়? Raw card number object-এ ঢোকার সাথে সাথে mask হয়ে যায়। এরপর যদি কেউ object inspect করে, log করে, বা debug করে, শুধু masked version দেখবে। Original number চিরতরে চলে গেছে।

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| bKash balance সরাসরি বদলানো যায় না | Private variable |
| ATM-এর তিনটা button | Public methods (controlled interface) |
| PIN ছাড়া কিছু হয় না | Access modifier: private |
| Cashier নিজে জানে কিন্তু বলে না | Getter without setter |
| Invalid amount deposit ঠেকানো | Setter with validation |
| Card number mask করে রাখা | Encapsulation for security |

**Data private রাখো, access দাও শুধু method-এর মাধ্যমে।**

**Method-এর ভেতরে validation রাখলে invalid state কখনো তৈরি হয় না।**

**Implementation বদলালেও বাইরের code ভাঙবে না, এটাই maintainability।**

---

> পরবর্তী প্রশ্ন: Encapsulation detail লুকায়। কিন্তু একটা class-এর পুরো complexity যদি লুকিয়ে, শুধু essential অংশটুকু দেখাতে পারতাম? সেই ধারণার নাম **Abstraction।**

*OOP সিরিজের পরবর্তী পর্ব: Abstraction, জটিলতা লুকানোর শিল্প*
