---
title: "Polymorphism: রনি ভাইয়ের একটা নির্দেশ, পঞ্চাশটা আলাদা কাজ"
tags: [Polymorphism, OOP, "Method Overriding", "Method Overloading", LLD]
date: 2023-02-15
date_bn: "ফেব্রুয়ারি ২০২৩"
read_time: "৭ মিনিট"
excerpt: "একটাই নির্দেশ, কিন্তু প্রত্যেকে নিজের মতো করে পালন করে। Polymorphism-এর গল্প, রনি ভাইয়ের গার্মেন্টস ফ্যাক্টরি থেকে।"
---

# Polymorphism: রনি ভাইয়ের একটা নির্দেশ, পঞ্চাশটা আলাদা কাজ
## OOP সিরিজ

---

## ভূমিকা

প্রতিদিন সকাল আটটায় রনি ভাই ফ্লোরে আসেন।

বড় গলায় একটাই কথা বলেন: **"কাজ শুরু করো!"**

সেলাই মেশিনের অপারেটররা সুই-সুতো সামলে কাপড় সেলাই শুরু করে। কাটিং সেকশনের লোকেরা কাঁচি আর মেশিন নিয়ে কাপড় কাটতে বসে। QC টিম পাশে দাঁড়িয়ে পূর্ণ মনোযোগে প্রতিটি পিস পরীক্ষা করতে থাকে। প্যাকেজিং দলের ছেলেমেয়েরা বাক্স সাজিয়ে কাপড় ভাঁজ করতে শুরু করে।

একটাই নির্দেশ। কিন্তু পঞ্চাশটা আলাদা কাজ।

রনি ভাই কাউকে আলাদা করে বলেননি: "তুমি সেলাও, তুমি কাটো, তুমি দেখো।" তিনি শুধু বলেছেন "কাজ শুরু করো।" বাকিটা প্রত্যেকে নিজের ভূমিকা অনুযায়ী বুঝে নিয়েছে।

কোডের দুনিয়ায় এই ধারণাটার নাম **Polymorphism**।

---

## ১. Polymorphism কী এবং কেন দরকার?

Polymorphism মানে "অনেক রূপ।" গ্রিক শব্দ: *poly* (অনেক) + *morphe* (রূপ)।

প্রোগ্রামিংয়ে এর মানে হলো: একই নামের একটা method বা interface, কিন্তু ভিন্ন ভিন্ন class সেটাকে নিজের মতো করে implement করে।

রনি ভাইয়ের ফ্যাক্টরির কথাই ভাবো। ধরো তুমি একটা `Worker` class বানালে এবং তাতে একটা method রাখলে `start_work()`। কিন্তু সেলাইকর্মীর "কাজ" আর QC ইন্সপেক্টরের "কাজ" এক না। তাহলে?

Polymorphism ছাড়া তোমাকে আলাদা method লিখতে হতো প্রতিটার জন্য:

```python
def start_tailor_work(worker): ...
def start_cutting_work(worker): ...
def start_qc_work(worker): ...
```

এভাবে ম্যানেজার হিসেবে রনি ভাইকে প্রতিটা টাইপের কর্মীর জন্য আলাদা করে চিন্তা করতে হতো। কিন্তু Polymorphism দিয়ে তিনি শুধু বলেন: **"যে-ই হও, `start_work()` করো।"** বাকিটা সবাই নিজে বোঝে।

{% include diagrams/polymorphism/diagram-1.html %}

এই ধারণাটা দুটো উপায়ে কাজ করে: **Compile-time Polymorphism** আর **Runtime Polymorphism**। প্রথমটা সহজ, দ্বিতীয়টাই আসল জায়গা।

---

## ২. Compile-time Polymorphism: একই নাম, ভিন্ন কাজ

রনি ভাইয়ের ফ্যাক্টরিতে একটা `generate_report()` ফাংশন আছে। কিন্তু:
- শুধু তারিখ দিলে সেই দিনের রিপোর্ট
- তারিখ আর সেকশনের নাম দিলে নির্দিষ্ট সেকশনের রিপোর্ট
- মাস আর বছর দিলে সেই মাসের রিপোর্ট

একই নাম, কিন্তু parameter আলাদা। এটাকে বলে **Method Overloading।**

Java বা C++-এ সরাসরি লেখা যায়:

```java
class ReportService {
    void generateReport(String date) { ... }
    void generateReport(String date, String section) { ... }
    void generateReport(int month, int year) { ... }
}
```

Python-এ সরাসরি overloading নেই, কিন্তু default parameter দিয়ে কাছাকাছি করা যায়:

```python
def generate_report(date=None, section=None, month=None):
    if date and section:
        print(f"{date}-এর {section} সেকশনের রিপোর্ট")
    elif date:
        print(f"{date}-এর সম্পূর্ণ রিপোর্ট")
    elif month:
        print(f"{month} মাসের রিপোর্ট")
```

Compile-time Polymorphism বেশিরভাগ সময় কোড পরিষ্কার রাখার জন্য ব্যবহার হয়। কিন্তু OOP-এর আসল শক্তি দেখা যায় Runtime Polymorphism-এ।

---

## ৩. Runtime Polymorphism: আসল জায়গা

রনি ভাই প্রতিদিন সকালে সব কর্মীর তালিকা ধরে একবার করে ডাকেন: **"কাজ শুরু করো।"**

তিনি জানেন না এবং জানার দরকারও নেই কে সেলাইকর্মী, কে QC। তিনি শুধু জানেন যে সবাই `Worker`, আর প্রত্যেক `Worker`-এর একটা `start_work()` আছে।

এটাই **Method Overriding** আর Runtime Polymorphism।

{% include code-blocks/polymorphism/code-1.html %}

এখন রনি ভাইয়ের ফ্লোর ম্যানেজমেন্ট কোড দেখো:

```python
floor = [
    Tailor("জামাল"),
    Cutter("সালমা"),
    QCInspector("হাসান"),
    Tailor("মিতু"),
]

for worker in floor:
    worker.start_work()  # একই call, চারটা আলাদা ফলাফল
```

রনি ভাই একবার `start_work()` লিখেছেন। Python runtime-এ নিজে বুঝে নিচ্ছে কোন object-এর কোন version চালাতে হবে। এটাই **Dynamic Dispatch**, Runtime Polymorphism-এর মূল কৌশল।

{% include diagrams/polymorphism/diagram-2.html %}

---

## ৪. Polymorphism with Abstract Class এবং Interface

আগের [Abstraction](abstraction.md) পর্বে দেখেছিলে Abstract Class কীভাবে কাজ করে। Polymorphism আর Abstract Class একসাথে ব্যবহার হলে জিনিসটা আরও পরিষ্কার হয়।

Abstract Class দিয়ে তুমি বলতে পারো: **"প্রতিটা `Worker`-এর অবশ্যই `start_work()` থাকতে হবে। কিন্তু কী করবে সেটা নিজে ঠিক করো।"**

{% include code-blocks/polymorphism/code-2.html %}

এখন কেউ `Worker` class-এর direct object বানাতে পারবে না। আর `start_work()` implement না করলে error আসবে। Polymorphism-এর সাথে Abstract Class এই গ্যারান্টিটা দেয়।

**Interface দিয়ে Polymorphism** একটু আলাদাভাবে ভাবো। মনে করো, ফ্যাক্টরির কিছু কর্মী **Trainable**, মানে নতুন স্কিল নিতে পারে। সব কর্মী না, শুধু কেউ কেউ।

```python
class Trainable:
    def take_training(self):
        raise NotImplementedError


class Tailor(Worker, Trainable):
    def start_work(self):
        print(f"{self.name}: stitching started.")

    def take_training(self):
        print(f"{self.name} learning new machine operation.")
```

এখন একটা `trainable_list` তৈরি করে সবাইকে `take_training()` বলা যাচ্ছে, Polymorphism থাকছে, কিন্তু ট্রেনিং শুধু যোগ্যদের জন্য।

---

## বাস্তব উদাহরণ: Daraz-এর প্রাইসিং সিস্টেম

Daraz-এ তিন ধরনের product আছে: সাধারণ product, discounted product, আর flash sale product। তিনটার দাম হিসেব করার নিয়ম আলাদা। কিন্তু checkout page-এ সবার জন্য একটাই call: **"দাম বলো।"**

{% include code-blocks/polymorphism/code-3.html %}

```python
cart = [
    RegularProduct("শার্ট", 850),
    DiscountedProduct("জুতা", 1200, 20),   # 20% ছাড়
    FlashSaleProduct("ব্যাগ", 2000, 999),  # flash sale price
]

total = 0
for product in cart:
    product.display()
    total += product.final_price()

print(f"\nমোট: ৳{total}")
```

Checkout code জানে না কোনটা regular, কোনটা flash sale। সে শুধু জানে সবাই `Product`, আর প্রতিটার `final_price()` আছে। নতুন product type যোগ করতে হলে শুধু নতুন class বানাও, checkout code একটুও বদলাবে না। এটাই Polymorphism-এর আসল সুবিধা।

---

## সারসংক্ষেপ

| গল্পের ভাষায় | প্রযুক্তির ভাষায় |
|---|---|
| রনি ভাইয়ের "কাজ শুরু করো" | Polymorphic method call |
| প্রত্যেক কর্মী নিজের মতো বোঝে | Method Overriding |
| একই নামে ভিন্ন parameter | Method Overloading |
| রনি ভাই কে কী করে জানেন না | Dynamic Dispatch |
| সব `Worker`-এর `start_work()` বাধ্যতামূলক | Abstract Method |
| Daraz checkout একটাই loop | Polymorphic interface |

**মূল শিক্ষা:**

**Polymorphism মানে caller-এর চিন্তা কমানো।** রনি ভাই বা Daraz checkout, কেউ জানতে চায় না ভেতরে কী হচ্ছে, শুধু জানে কাকে কী বলতে হবে।

**Method Overriding হলো Runtime Polymorphism।** Parent class-এর method child class নিজের মতো করে লেখে, আর Python runtime-এ সঠিক version বেছে নেয়।

**Abstract class আর Polymorphism একসাথে শক্তিশালী।** Abstract method guarantee দেয় সব subclass implement করবে, Polymorphism guarantee দেয় সবাইকে একই চোখে দেখা যাবে।

**নতুন type যোগ করা সহজ হয়।** `FlashSaleProduct` যোগ করতে checkout code বদলাতে হয়নি। এটাই Open/Closed Principle-এর ভিত্তি।

---

> পরবর্তী প্রশ্ন: class আর object বানাতে পারি, inherit করতে পারি, polymorphism বুঝলাম। কিন্তু object তৈরি করার সময় যদি নিয়ম থাকত যে কীভাবে বানাতে হবে? সেই ধারণার নাম **Design Patterns**।

*OOP সিরিজের পরবর্তী পর্ব: Design Patterns: একই সমস্যা, পরীক্ষিত সমাধান*
