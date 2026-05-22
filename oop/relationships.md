---
layout: post
title: "Association, Aggregation, Composition: Object-রা কীভাবে একে অপরকে চেনে"
description: "সব object আলাদা দ্বীপে থাকে না। কেউ শুধু চেনে, কেউ ধরে রাখে, কেউ ছাড়া বাঁচতেই পারে না। পাঁচটা সম্পর্ক, একটা গল্পে।"
series: "OOP সিরিজ"
series_label: "OOP"
category: oop
tags: [Association, Aggregation, Composition, Dependency, Realization, OOP, LLD]
date: 2026-05-22
date_bn: "মে ২০২৬"
read_time: "৮ মিনিট"
order: 8
excerpt: "সব object আলাদা দ্বীপে থাকে না। কেউ শুধু চেনে, কেউ সাথে রাখে, কেউ ছাড়া বাঁচতেই পারে না। Association, Aggregation, Composition, Dependency আর Realization — পাঁচটা সম্পর্ক।"
---

নারায়ণগঞ্জের একটা পুরনো গলি। "কামাল টেইলার্স" — দরজার উপর সাইনবোর্ডে লেখা ১৯৮৯ সাল থেকে।

বুধবার সকালে ঢুকলে একসাথে দেখবে: সেলাই মেশিনে কাপড় যাচ্ছে, কাটিং টেবিলে মাপ নেওয়া হচ্ছে, পাশের গদি দোকান থেকে কাপড়ের থান আসছে, একজন দর্জি কাউন্টার থেকে মাপের খাতা তুলে নিচ্ছে, দেওয়ালে BGMEA সদস্যপদের সার্টিফিকেট ঝুলছে।

একসাথে পাঁচটা ঘটনা। পাঁচ রকম সম্পর্ক।

Object-oriented programming-এও ঠিক এইভাবে object-রা একে অপরের সাথে সম্পর্ক রাখে। কেউ শুধু চেনে। কেউ ধরে রাখে। কেউ তৈরি করে। কেউ ক্ষণিকের জন্য ধার নেয়। কেউ চুক্তি মেনে চলে।

এই পাঁচটা সম্পর্কের নাম: **Association**, **Aggregation**, **Composition**, **Dependency**, আর **Realization**।

---

## ১. Association: চেনা-জানা, কিন্তু আলাদা

কামাল ভাই চেনেন সেলিম ভাইকে। পাশের গলির গদি দোকানের মালিক।

যখনই কাপড়ের থান দরকার হয়, কামাল ভাই ফোন করেন। সেলিম ভাই মাপ অনুযায়ী কাপড় আলাদা করে রাখেন, লোক পাঠিয়ে নিয়ে আসা হয়। কিন্তু কামাল ভাই চাইলে মিটফোর্ড থেকেও কাপড় আনাতে পারেন। সেলিম ভাইয়েরও আরো দুইশো কাস্টমার আছে।

দুইজন দুইজনকে চেনেন, যোগাযোগ করেন। কিন্তু কেউ কাউকে তৈরি করেননি। কেউ কারো উপর নির্ভর করে বেঁচে নেই।

এই সম্পর্কের নাম **Association**।

দুটো class যখন একে অপরকে চেনে, একে অপরের reference রাখে — কিন্তু একজন আরেকজনকে তৈরি করেনি, lifecycle-ও আলাদা — সেটা Association।

{% include code-blocks/relationships/code-1.html %}

```python
selim = Supplier("সেলিম ভাই")
sumon = Worker("সুমন", supplier=selim)

sumon.request_item("সুতি কাপড়")
# সেলিম ভাই: preparing সুতি কাপড়
# সুমন: received সুতি কাপড়
```

`sumon` চলে গেলেও `selim` থাকবেন। **দুজনের lifecycle আলাদা** — এটাই Association-এর মূল কথা।

---

## ২. Aggregation: দোকানে আছে, কিন্তু দোকানের না

দোকানে তিনজন দর্জি: সুমন, বাবুল, আর মতিন।

কামাল ভাই তাদের বেতন দেন, শিফট ঠিক করেন। কিন্তু আজকে দোকান বন্ধ হয়ে গেলে? তিনজনই অন্য দোকানে কাজ পাবেন। তারা কামাল ভাইয়ের দোকানে আসার আগেও ছিলেন। চলে গেলেও থাকবেন।

দোকানের সাথে তাদের সম্পর্ক আছে, কিন্তু দোকান তাদের "মালিক" না।

এই সম্পর্কের নাম **Aggregation**।

Aggregation মানে: "whole"-এর কাছে "part" আছে, কিন্তু "part" স্বাধীনভাবে বাঁচতে পারে। "Whole" মরলেও "part" মরে না।

{% include code-blocks/relationships/code-2.html %}

```python
sumon  = Worker("সুমন")
babul  = Worker("বাবুল")
matin  = Worker("মতিন")

shop = Shop("কামাল টেইলার্স", workers=[sumon, babul, matin])
shop.open()
# সুমন is working
# বাবুল is working
# মতিন is working
```

`workers` list বাইরে থেকে pass করা হয়েছে — `Shop` তাদের তৈরি করেনি। `shop` object চলে গেলেও `sumon`, `babul`, `matin` memory-তে থাকবে।

কিন্তু দোকানের কাটিং টেবিলগুলো? সেগুলো অন্য গল্প।

---

## ৩. Composition: দোকানের অংশ, দোকান ছাড়া অস্তিত্ব নেই

দোকানে চারটা কাটিং টেবিল। কামাল ভাই নিজে বানিয়েছিলেন, ঘরের ভেতরে মাপ নিয়ে তৈরি করা।

আজকে দোকান বন্ধ হয়ে গেলে এই টেবিলগুলো কোথায় যাবে? অন্য দোকানে গিয়ে কাজ করবে না। দোকানের মেঝের সাথে গাঁথা। দোকান উঠে গেলে এগুলোও যাবে।

এই সম্পর্কের নাম **Composition**।

দেখতে Aggregation-এর মতোই: "whole"-এর কাছে "part" আছে। কিন্তু এখানে "whole" নিজেই "part" তৈরি করে। আর "whole" মরলে "part"-ও মরে।

{% include code-blocks/relationships/code-3.html %}

```python
shop = Shop("কামাল টেইলার্স")
shop.accept_job("ORDER-0042")
# Station #1: working on ORDER-0042
```

`WorkStation` object তৈরি হয়েছে `Shop`-এর constructor-এর ভেতরে। `shop` চলে গেলে এই station object-গুলোও চলে যাবে।

**Aggregation আর Composition-এর পার্থক্য বোঝার এক প্রশ্ন:** "part"-টা কি "whole"-এর ভেতরে তৈরি হয়েছে, নাকি বাইরে থেকে এসেছে?

{% include diagrams/relationships/diagram-1.html %}

---

## ৪. Dependency: ধার নিলাম, ব্যবহার করলাম, ফেরত দিলাম

একটা অর্ডার এলো। সুমন উঠল, কাউন্টারের উপর থেকে মাপের খাতাটা নিল।

খাতায় কাস্টমারের মাপ আছে: বুক, কোমর, কাঁধ। সুমন মাপ দেখল, কাপড় কাটল, খাতা ফেরত রেখে দিল।

সুমন খাতাটা নিজের কাছে রাখেনি। শুধু একটা কাজের জন্য, কয়েক মিনিটের জন্য ব্যবহার করল।

এই সম্পর্কের নাম **Dependency**।

পাঁচটার মধ্যে সবচেয়ে হালকা সম্পর্ক। এক class আরেক class-কে শুধু একটা method-এর ভেতরে ব্যবহার করে। Object-এ store করে না। Lifecycle share করে না। Method শেষ হলে সম্পর্কও শেষ।

{% include code-blocks/relationships/code-4.html %}

```python
spec  = Specification()
sumon = Worker("সুমন")

sumon.process_order("ORDER-0042", spec)
# Looking up spec for ORDER-0042...
# সুমন: processing order ORDER-0042
#   measurement_a: 40
#   measurement_b: 34
#   measurement_c: 16
```

`Specification` class-টা `Worker`-এর `__init__`-এ নেই। `process_order` method-এর parameter হিসেবে আসে, ব্যবহার হয়, শেষ। **Method call-এর সময়টুকুর জন্যই এই সম্পর্ক।**

UML diagram-এ এটা dashed arrow দিয়ে আঁকা হয়। সবচেয়ে হালকা স্পর্শ।

---

## ৫. Realization: চুক্তি করেছি, পালন করতেই হবে

দেওয়ালে একটা সার্টিফিকেট। BGMEA সদস্যপদ।

এটা পেতে কামাল ভাইকে একটা চুক্তি সই করতে হয়েছিল। নির্দিষ্ট মান মেনে কাপড় তৈরি করতে হবে। কাজের পরিবেশ নিরাপদ রাখতে হবে। সঠিক মজুরি দিতে হবে।

চুক্তিতে লেখা ছিল কী করতে হবে। কীভাবে করতে হবে সেটা কামাল ভাইয়ের ব্যাপার। কিন্তু "কী করতে হবে" সেটার প্রতিটা পয়েন্ট পূরণ করতেই হবে। একটাও বাদ দেওয়া যাবে না।

এই সম্পর্কের নাম **Realization**।

একটা class যখন একটা interface-কে implement করে, সে সেই interface-এর "চুক্তি" পূরণ করার প্রতিশ্রুতি দেয়। Interface বলে কী করতে হবে। Class বলে কীভাবে করবে।

{% include code-blocks/relationships/code-5.html %}

```python
shop = Workshop()
shop.safe_environment()
shop.fair_wages("সুমন")
shop.quality_output("ORDER-0042")
# Fire exits clear, safety equipment in place
# সুমন: paid on time
# Order ORDER-0042: completed to standard
```

Interface বলে দেয় কী কী method থাকতে হবে। `Workshop` সেই প্রতিটা method implement করে। একটাও বাদ দিলে Python `TypeError` দেবে: চুক্তি অসম্পূর্ণ।

Java-তে এটাকে বলে `implements`। Swift-এ বলে protocol। সব জায়গায় একই কথা: **যে class interface realize করে, সে প্রতিটা শর্ত পূরণ করতে বাধ্য।**

---

## পাঁচটা সম্পর্ক একসাথে

{% include diagrams/relationships/diagram-2.html %}

পাঁচটা তীর। পাঁচটা আলাদা ওজন।

**Solid line:** Association। **Hollow diamond:** Aggregation। **Filled diamond:** Composition। **Dashed arrow:** Dependency। **Dashed arrow with hollow triangle:** Realization।

Diamond দেখলে বুঝবে ownership। Fill দেখলে বুঝবে কতটা গভীর। Dashes দেখলে বুঝবে সম্পর্কটা ক্ষণিকের।

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| কামাল ভাই আর সেলিম ভাই পরস্পরকে চেনেন | Association: দুটো class একে অপরের reference রাখে |
| দর্জিরা দোকানে কাজ করেন, চলে গেলেও টিকে থাকেন | Aggregation: whole-এর কাছে part আছে, কিন্তু part স্বাধীন |
| কাটিং টেবিল দোকানের ভেতরে তৈরি, দোকান উঠলে যাবে | Composition: whole নিজেই part তৈরি করে, lifecycle একসাথে |
| সুমন খাতা তুলল, কাজ শেষে ফেরত রাখল | Dependency: method-এর ভেতরে ব্যবহার, store করা হয় না |
| BGMEA চুক্তির প্রতিটা শর্ত পূরণ | Realization: class-টা interface-এর প্রতিটা method implement করে |
| দোকান বন্ধ হলে দর্জিরা অন্য জায়গায় যান | Aggregation: whole মরলে part বাঁচে |
| দোকান বন্ধ হলে টেবিল আর নেই | Composition: whole মরলে part-ও মরে |

**Association সবচেয়ে আলগা: দুটো object পরস্পরকে চেনে, কিন্তু কেউ কাউকে তৈরি করেনি।**

**Aggregation আর Composition দুটোই "has-a" — পার্থক্য হলো part কি whole-এর ভেতরে তৈরি হয়েছে কিনা।**

**Dependency সবচেয়ে হালকা: একটা method call-এর জন্য ধার, শেষ হলে সম্পর্কও শেষ।**

**Realization একটা প্রতিশ্রুতি: interface-এর কোনো একটা method বাদ দেওয়ার সুযোগ নেই।**

---

> পরবর্তী প্রশ্ন: এত সম্পর্ক, এত dependency — কীভাবে লিখলে code টেকসই হয়, পরিবর্তন করতে গেলে ভেঙে না পড়ে? সেই পাঁচটা নিয়মের নাম **SOLID Principles।**

*OOP সিরিজের পরবর্তী পর্ব: SOLID — ভালো code লেখার পাঁচটা নিয়ম*
