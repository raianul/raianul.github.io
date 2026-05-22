---
title: "Enum: রিনা আপার অর্ডার খাতার বিশৃঙ্খলা"
tags: [Enum, OOP, Constants]
date: 2022-09-15
date_bn: "সেপ্টেম্বর ২০২২"
read_time: "৬ মিনিট"
description: "তিনজন হেল্পার, তিনরকম লেখা, একই অর্ডারের তিনটা নাম। Enum কীভাবে এই বিশৃঙ্খলা থামায় — বাংলায় OOP Enum টিউটোরিয়াল।"
excerpt: "তিনজন হেল্পার, তিনরকম লেখা, একই অর্ডারের তিনটা নাম। মাস শেষে রিপোর্ট বানাতে গিয়ে মাথায় হাত। Enum কীভাবে এই বিশৃঙ্খলা থামায়।"
---

রিনা আপার একটা ছোট্ট অনলাইন শপ আছে।

ফেসবুকে পেজ, হাতে বানানো কাপড়ের গয়না, মাসে ৫০-৬০টা অর্ডার। ব্যবসা ভালোই চলছে।

কিন্তু অর্ডার ট্র্যাক করতে গিয়ে সমস্যা।

রিনা আপা নিজে লেখেন: "পেন্ডিং", "রেডি", "পাঠানো হয়েছে", "পৌঁছেছে"। তার হেল্পার সুমাইয়া লেখে: "wait", "ready to ship", "shipped", "delivered"। আরেকজন হেল্পার কামাল লেখে: "অপেক্ষায়", "তৈরি", "রাস্তায়", "পাইছে"।

মাস শেষে রিপোর্ট বানাতে গিয়ে মাথায় হাত। কতটা অর্ডার এখনো পেন্ডিং? কতটা shipped হয়েছে? কেউ বলতে পারছে না কারণ একই অবস্থার তিনটা আলাদা নাম।

এই সমস্যার নাম **magic strings** আর সমাধানের নাম **Enum।**

---

## ১. Enum কী?

রিনা আপা যদি সবাইকে বলতেন: "অর্ডারের অবস্থা লিখতে হলে শুধু এই পাঁচটা শব্দ ব্যবহার করবে: PENDING, CONFIRMED, SHIPPED, DELIVERED, CANCELLED। এর বাইরে কোনো শব্দ চলবে না।"

এই পাঁচটা নির্দিষ্ট শব্দের তালিকাটাই হলো **Enum (Enumeration)**।

Enum হলো একটা বিশেষ data type যেটা একটা fixed, predefined set of named constants ধরে রাখে। একবার define করলে সেই set-এর বাইরে কোনো value সম্ভব না।

{% include code-blocks/enums/code-1.html %}

এখন কেউ `OrderStatus.SHIPED` লিখলে Python সাথে সাথে error দেবে। Typo করার সুযোগ নেই। "raste ache" লেখার সুযোগ নেই। শুধু পাঁচটা নির্দিষ্ট option।

{% include diagrams/enums/diagram-1.html %}

Enum use না করলে এই flow-টা maintain করা কঠিন। যে কেউ যেকোনো string লিখতে পারে, ভুল state-ও set করতে পারে।

---

## ২. Simple Enum: শুধু নাম দরকার

সবচেয়ে সহজ Enum-এ শুধু names থাকে। কোনো extra value দরকার নেই।

Pathao ride-এর payment method ভাবো। হয় CASH, নয়তো CARD, নয়তো DIGITAL_WALLET (bKash)। তিনটার বাইরে কিছু নেই।

{% include code-blocks/enums/code-2.html %}

Simple Enum-এ value হিসেবে integer ব্যবহার করলে কাজ চলে। কিন্তু কখনো কখনো আরেকটু বেশি দরকার হয়।

---

## ৩. Enum with Properties and Methods: Enum-এ বুদ্ধি

রিনা আপার অর্ডার status-এর সাথে যদি আরও তথ্য রাখা দরকার হয়? যেমন, প্রতিটা status-এর একটা বাংলা label আর একটা description?

Enum-এ properties আর methods যোগ করা যায়।

{% include code-blocks/enums/code-3.html %}

এখন Enum শুধু নাম না, পুরো information ধরে রাখছে:

```python
status = OrderStatus.SHIPPED

print(status.value)        # shipped
print(status.label)        # Shipped
print(status.description)  # Order handed to courier
print(status.is_active())  # True
print(status.can_cancel()) # False (shipped হয়ে গেলে আর cancel হয় না)
```

{% include diagrams/enums/diagram-2.html %}

প্রতিটা Enum member এখন নিজেই জানে সে cancel করা যাবে কিনা, সে active কিনা। এই logic Enum-এর ভেতরেই থাকায় বাইরে বারবার লিখতে হয় না।

---

## বাস্তব উদাহরণ: Daraz-এর Order Processing

Daraz-এ প্রতিদিন লক্ষ লক্ষ অর্ডার হয়। প্রতিটা অর্ডার বিভিন্ন state-এর মধ্য দিয়ে যায়।

Enum ছাড়া এই system-এ কী সমস্যা হতো:

```python
# Enum ছাড়া: বিপদ
order["status"] = "shiped"    # typo, কেউ ধরতে পারবে না
order["status"] = "Delivered" # capital D, আলাদা string
order["status"] = "রাস্তায়"  # বাংলায় লিখলে?

if order["status"] == "shipped":  # এই check কাজ করবে না!
    notify_customer()
```

Enum দিয়ে:

{% include code-blocks/enums/code-4.html %}

```python
order = Order("DRZ-001", "কাপড়ের গয়না")
order.confirm()
order.ship()    # status PACKED না হওয়ায় কিছু হবে না
order.cancel()  # CONFIRMED হওয়ার পর ship না হলে cancel হবে না
```

Enum ব্যবহার করায় invalid state transition সম্ভব না। কেউ ভুলেও `order.status = "delvrd"` লিখতে পারবে না।

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| রিনা আপার পাঁচটা নির্দিষ্ট status শব্দ | Enum |
| প্রতিটা নির্দিষ্ট শব্দ | Enum member |
| শব্দের সাথে বাংলা label রাখা | Enum property |
| "shipped হলে cancel হবে না" নিয়ম | Enum method |
| যে কেউ যা খুশি লিখতে পারত | Magic strings (সমস্যা) |

**Enum ব্যবহার করলে invalid value set করা compiler বা runtime-এই আটকে যায়।**

**Enum members-এ property আর method রাখা যায়, তাই related logic একজায়গায় থাকে।**

**Code পড়তে সহজ হয়: `OrderStatus.SHIPPED` দেখলেই বোঝা যায়, `2` দেখলে বোঝা যায় না।**

---

> পরবর্তী প্রশ্ন: Class-এ যদি কিছু জিনিস লুকিয়ে রাখা যেত, শুধু দরকারি অংশটুকুই বাইরে দেখাত? সেই ধারণার নাম **Encapsulation।**

*OOP সিরিজের পরবর্তী পর্ব: Encapsulation, যা সবাইকে দেখানো যায় না*
