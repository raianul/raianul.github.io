---
title: "Encapsulation: তোমার bKash balance কেউ সরাসরি ছুঁতে পারে না"
tags: [Encapsulation, OOP, "Access Modifiers", "Data Hiding"]
date: 2026-05-18
date_bn: "মে ২০২৬"
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

bKash-এর ATM analogy ভাবো। তুমি সরাসরি bank-এর vault-এ ঢুকে balance বদলাতে পারো না। ATM-এর মাধ্যমে যাও। ATM তোমাকে তিনটাই দেয়: `deposit()`, `withdraw()`, `checkBalance()`। ভেতরে কী হচ্ছে? সেটা তোমার ব্যাপার না।

{% include diagrams/encapsulation/diagram-1.html %}

**Encapsulation কেন দরকার?**

প্রথমত, sensitive data লুকিয়ে রাখা যায়। Balance, password, card number সরাসরি দেখা যাবে না।

দ্বিতীয়ত, controlled validation। কেউ `-500` টাকা deposit করতে চাইলে আটকানো যাবে। Method-এর ভেতরে rule থাকবে।

তৃতীয়ত, maintainability। ভেতরের implementation বদলালেও বাইরের code ভাঙবে না। bKash চাইলে তাদের balance integer থেকে double-এ বদলাতে পারে, তুমি টেরও পাবে না।

---

## ২. Access Modifiers: কতটুকু দেখাবে?

Encapsulation implement করার মূল হাতিয়ার হলো **Access Modifiers।** এগুলো নিয়ন্ত্রণ করে class-এর কোন অংশ বাইরে থেকে দেখা যাবে।

তিনটা মূল modifier:

**`private`:** শুধু ওই class-এর ভেতরে। সবচেয়ে বেশি ব্যবহার হয় data লুকাতে। বাইরে থেকে কেউ ছুঁতে পারবে না।

**`protected`:** ওই class আর তার subclass-এ। Inheritance-এ কাজে লাগে।

**`public`:** সবাই দেখতে পারে। এটাই controlled interface।

সহজ নিয়ম: **সব কিছু default-এ private রাখো, দরকার হলে public করো।**

```python
class BkashWallet:
    def __init__(self, owner, initial_balance):
        self.__owner = owner           # private, বাইরে দেখা যাবে না
        self.__balance = initial_balance  # private, সরাসরি বদলানো যাবে না
        self.__pin = None              # private

    def get_balance(self):             # public, শুধু পড়া যাবে
        return self.__balance

    def deposit(self, amount):         # public, controlled access
        if amount <= 0:
            print("Invalid amount")
            return False
        self.__balance += amount
        return True

    def withdraw(self, amount):        # public, controlled access
        if amount <= 0:
            print("Invalid amount")
            return False
        if amount > self.__balance:
            print("Insufficient balance")
            return False
        self.__balance -= amount
        return True
```

এখন দেখো কী হয়:

```python
wallet = BkashWallet("Raian", 1000)

# সঠিক পথ
print(wallet.get_balance())   # 1000
wallet.deposit(500)
print(wallet.get_balance())   # 1500

# সরাসরি access করার চেষ্টা
wallet.__balance = 999999     # এটা কাজ করবে না!
print(wallet.get_balance())   # এখনো 1500
```

Python-এ `__` দিয়ে private করলে সরাসরি access block হয়ে যায়।

---

## ৩. Getters এবং Setters: নিয়ন্ত্রিত দরজা

Private data পড়তে এবং লিখতে যে public methods ব্যবহার হয় সেগুলোকে বলে **Getter** আর **Setter।**

**Getter:** শুধু পড়তে দেয়। `getBalance()` balance দেখায়, কিন্তু বদলাতে দেয় না।

**Setter:** লিখতে দেয়, কিন্তু validation সহ। Invalid value ঢুকতে পারে না।

```python
class BankAccount:
    def __init__(self):
        self.__balance = 0
        self.__account_holder = ""

    # Getter
    def get_balance(self):
        return self.__balance

    # Getter
    def get_account_holder(self):
        return self.__account_holder

    # Setter with validation
    def set_account_holder(self, name):
        if not name or len(name.strip()) == 0:
            raise ValueError("Account holder name cannot be empty")
        self.__account_holder = name.strip()

    # Business method with validation
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        self.__balance += amount
        print(f"{amount} টাকা জমা হয়েছে। নতুন balance: {self.__balance}")

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")
        if amount > self.__balance:
            raise ValueError("Insufficient funds")
        self.__balance -= amount
        print(f"{amount} টাকা তোলা হয়েছে। নতুন balance: {self.__balance}")
```

Setter-এর কারণে `account_holder = ""` বা `balance = -500` কখনো set হবে না।

---

## বাস্তব উদাহরণ: Payment Processor

এই example-টা সবচেয়ে শক্তিশালী।

তুমি একটা payment system বানাচ্ছ। Customer-এর credit card number system-এ রাখতে হবে। কিন্তু পুরো number কখনো store করা যাবে না, এটা security risk। শুধু masked version রাখতে হবে: `****-****-****-1234`।

Encapsulation এই সমস্যার perfect সমাধান:

```python
class PaymentProcessor:
    def __init__(self, card_number: str, amount: float):
        # Raw card number কখনো store হয় না
        # Constructor-এই mask করে ফেলা হচ্ছে
        self.__masked_card = self.__mask_card(card_number)
        self.__amount = amount
        self.__is_processed = False

    def __mask_card(self, card_number: str) -> str:
        # private method: বাইরে থেকে call করা যাবে না
        digits_only = card_number.replace("-", "").replace(" ", "")
        return f"****-****-****-{digits_only[-4:]}"

    def process_payment(self) -> bool:
        if self.__is_processed:
            print("এই payment ইতিমধ্যে process হয়েছে")
            return False

        # Payment gateway-এ পাঠানোর logic
        print(f"Card {self.__masked_card} দিয়ে {self.__amount} টাকা process হচ্ছে...")
        self.__is_processed = True
        print("Payment সফল!")
        return True

    def get_masked_card(self) -> str:
        return self.__masked_card

    def get_amount(self) -> float:
        return self.__amount
```

{% include diagrams/encapsulation/diagram-2.html %}

```python
processor = PaymentProcessor("4111-1111-1111-1234", 500)

# সঠিক পথ
processor.process_payment()
# Card ****-****-****-1234 দিয়ে 500 টাকা process হচ্ছে...
# Payment সফল!

print(processor.get_masked_card())  # ****-****-****-1234

# Raw card number কখনো পাবে না
# processor.__mask_card() কাজ করবে না (private method)
```

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
