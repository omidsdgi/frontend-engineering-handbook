# Chapter 11 — Coding Challenge

---

# اهداف فصل

پس از مطالعه این فصل، انتظار می‌رود بتوانید:

* یک مسئله نرم‌افزاری ساده را به Requirements قابل اجرا تبدیل کنید.
* مسئله را به بخش‌های کوچک‌تر و قابل مدیریت تقسیم کنید.
* پیش از نوشتن کد، یک Plan ساده برای حل مسئله ایجاد کنید.
* مفاهیم Fundamentals آموخته‌شده را در یک مسئله واقعی ترکیب کنید.
* از Variables، Values، Operators، Conditions، Loops و Template Literals در یک برنامه یکپارچه استفاده کنید.
* برای بررسی صحت برنامه، Test Caseهای مشخص طراحی کنید.
* با استفاده از Developer Tools یک Bug منطقی را پیدا و اصلاح کنید.
* تفاوت میان «کدی که کار می‌کند» و «کدی که طراحی مناسبی دارد» را درک کنید.
* یک راه‌حل اولیه را با استفاده از Refactoring ساده‌تر و خواناتر کنید.
* درباره تصمیم‌های طراحی یک راه‌حل در یک Code Review فنی صحبت کنید.

---

# Core Question

> **چگونه می‌توان مفاهیم Fundamentals را برای حل یک مسئله واقعی ترکیب کرد؟**

---

# Concept Flow

```text
Problem
↓
Requirements
↓
Decomposition
↓
Plan
↓
Implementation
↓
Testing
↓
Debugging
↓
Refactoring
↓
Review
```

این فصل قرار نیست مفهوم جدید بزرگی به JavaScript اضافه کند.

هدف آن، **ترکیب مفاهیمی است که تاکنون آموخته‌ایم**.

در فصل‌های قبل هر مفهوم را به‌صورت جداگانه بررسی کردیم:

```text
Variables
Operators
Strings
Conditions
Loops
Debugging
```

اکنون باید این مفاهیم را در یک مسئله واحد کنار یکدیگر قرار دهیم.

این همان نقطه‌ای است که یادگیری Syntax به سمت **Problem Solving** حرکت می‌کند.

---

# مقدمه

تا اینجا بیشتر مثال‌های کتاب یک مفهوم مشخص را توضیح می‌دادند.

برای مثال:

```javascript
const price = 100;
```

برای توضیح Variable استفاده می‌شد.

یا:

```javascript
if (price > 50) {
  console.log('Expensive');
}
```

برای توضیح Conditional Logic.

یا:

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

برای توضیح Loop.

اما در یک پروژه واقعی، معمولاً با یک مفهوم تنها مواجه نیستیم.

یک مسئله واقعی ممکن است هم‌زمان به:

* Variables
* Numbers
* Strings
* Operators
* Conditions
* Loops
* Template Literals
* Debugging

نیاز داشته باشد.

بنابراین توانایی واقعی برنامه‌نویس فقط این نیست که بتواند هر Syntax را به‌صورت جداگانه بنویسد.

باید بتواند آن‌ها را برای حل یک مسئله مشخص **ترکیب** کند.

---

# چرا Coding Challenge مهم است؟

دانستن Syntax به‌تنهایی به معنای توانایی حل مسئله نیست.

ممکن است برنامه‌نویسی بداند:

```javascript
if
```

چگونه نوشته می‌شود.

همچنین بداند:

```javascript
while
```

چگونه کار می‌کند.

و حتی Template Literal را نیز به‌خوبی بشناسد.

اما وقتی با یک مسئله واقعی مواجه می‌شود، ممکن است نداند:

* از کجا شروع کند؟
* مسئله را چگونه تقسیم کند؟
* چه Valueهایی نیاز دارد؟
* چه Conditionهایی لازم است؟
* Loop کجا قرار می‌گیرد؟
* چگونه بفهمد راه‌حل درست است؟
* اگر نتیجه اشتباه بود، چگونه Bug را پیدا کند؟
* آیا راه‌حل اولیه را می‌توان ساده‌تر کرد؟

Coding Challenge دقیقاً برای تمرین همین مهارت طراحی شده است.

---

# یک مدل ذهنی جدید

در فصل‌های قبلی بیشتر با این مسیر مواجه بودیم:

```text
Concept
↓
Syntax
↓
Example
```

اما در توسعه نرم‌افزار واقعی مسیر معمولاً برعکس است:

```text
Problem
↓
Understanding
↓
Plan
↓
Code
↓
Test
↓
Debug
↓
Refactor
↓
Review
```

این تغییر مدل ذهنی یکی از مهم‌ترین اهداف این فصل است.

> **در مهندسی نرم‌افزار، کدنویسی معمولاً اولین مرحله حل مسئله نیست.**

ابتدا باید مسئله را بفهمیم.

---

# Block 01 — Challenge Introduction

## مسئله چیست؟

فرض کنید در حال توسعه بخش ساده‌ای از یک فروشگاه اینترنتی هستیم.

کاربر یک محصول را انتخاب کرده است.

برنامه باید بتواند:

1. قیمت محصول را دریافت کند.
2. تعداد محصول را در نظر بگیرد.
3. Subtotal را محاسبه کند.
4. در صورت رسیدن سفارش به حد مشخص، Discount اعمال کند.
5. هزینه ارسال را محاسبه کند.
6. Total نهایی را به کاربر نمایش دهد.

در یک Application واقعی، این منطق احتمالاً بسیار پیچیده‌تر خواهد بود.

اما در این فصل عمداً مسئله را ساده نگه می‌داریم تا تمرکز روی **Problem Solving Process** باشد، نه روی پیچیدگی Application.

---

# Challenge Goal

هدف Challenge این است که برنامه‌ای بنویسیم که برای یک سفارش، مبلغ نهایی قابل پرداخت را محاسبه کند.

مدل ساده مسئله:

```text
Product
   ↓
Quantity
   ↓
Subtotal
   ↓
Discount
   ↓
Shipping
   ↓
Final Total
```

---

# Requirements

پیش از نوشتن حتی یک خط کد، Requirements را مشخص می‌کنیم.

برنامه باید:

* نام محصول را نگهداری کند.
* قیمت واحد محصول را نگهداری کند.
* تعداد محصول را نگهداری کند.
* قیمت کل قبل از Discount را محاسبه کند.
* اگر Subtotal حداقل `100` بود، `10%` Discount اعمال کند.
* اگر Subtotal حداقل `150` بود، هزینه ارسال صفر باشد.
* در غیر این صورت هزینه ارسال `10` باشد.
* Total نهایی را محاسبه کند.
* یک پیام خوانا برای نمایش نتیجه تولید کند.

---

# Concepts Used

این Challenge عمداً از مفاهیمی استفاده می‌کند که تاکنون آموزش داده‌ایم.

```text
Variables
↓
Values
↓
Data Types
↓
Operators
↓
Conditions
↓
Loops
↓
Template Literals
↓
Developer Tools
↓
Debugging
```

برای مثال:

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;
```

در این بخش از:

* `const`
* String
* Number
* Variable

استفاده کرده‌ایم.

سپس برای محاسبات از Operators و برای تصمیم‌گیری از Conditions استفاده خواهیم کرد.

---

# یک نکته مهم درباره Scope

در این Challenge عمداً از ساختارهایی مانند Function یا Object استفاده نمی‌کنیم.

دلیل آن ساده است.

این فصل قرار نیست مفاهیم فصل‌های آینده را زودتر آموزش دهد.

هدف آن است که نشان دهیم **با همان Fundamentals فعلی نیز می‌توان یک مسئله نسبتاً واقعی را حل کرد.**

بعداً با یادگیری Functionها و Objectها خواهیم دید که همین راه‌حل چگونه می‌تواند به ساختار حرفه‌ای‌تری تبدیل شود.

---

# Key Points

* Coding Challenge برای ترکیب مفاهیم قبلی است.
* قبل از کدنویسی باید مسئله را مشخص کنیم.
* Requirements مشخص می‌کنند برنامه چه رفتاری باید داشته باشد.
* در این فصل هدف، حل یک مسئله ساده مرتبط با Checkout است.
* مسئله عمداً بدون Function، Array و Object طراحی شده است.

---

# Block 02 — Problem Analysis

## Understanding Requirements

اکنون Requirements را به رفتارهای مشخص برنامه تبدیل می‌کنیم.

فرض کنید:

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;
```

قیمت هر Keyboard برابر `45` و تعداد آن `3` است.

پس:

```text
45 × 3 = 135
```

بنابراین:

```text
Subtotal = 135
```

از آنجا که:

```text
135 >= 100
```

Discount شامل سفارش می‌شود.

مقدار Discount:

```text
135 × 10% = 13.5
```

سپس:

```text
135 - 13.5 = 121.5
```

از آنجا که Subtotal کمتر از `150` است، هزینه ارسال:

```text
10
```

خواهد بود.

در نتیجه:

```text
121.5 + 10 = 131.5
```

Total نهایی:

```text
131.5
```

است.

---

# Breaking the Problem

اگر تمام مسئله را یک‌باره به کد تبدیل کنیم، احتمال اشتباه زیاد می‌شود.

بنابراین آن را به چند بخش تقسیم می‌کنیم.

```text
1. Product Information
↓
2. Subtotal
↓
3. Discount
↓
4. Shipping
↓
5. Final Total
↓
6. Output
```

این فرآیند را **Decomposition** می‌نامیم.

---

# Decomposition چیست؟

## تعریف ساده

Decomposition یعنی تقسیم یک مسئله بزرگ‌تر به چند مسئله کوچک‌تر.

به جای اینکه بپرسیم:

> چگونه Total سفارش را محاسبه کنم؟

مسئله را به سؤال‌های کوچک‌تر تبدیل می‌کنیم:

> قیمت واحد چیست؟

> تعداد چیست؟

> Subtotal چقدر است؟

> آیا Discount داریم؟

> هزینه ارسال چقدر است؟

> Total چقدر است؟

---

# چرا Decomposition مهم است؟

ذهن انسان در مواجهه با یک مسئله بزرگ، به‌راحتی دچار پیچیدگی می‌شود.

اما وقتی مسئله را به چند بخش کوچک تقسیم می‌کنیم، هر بخش قابل بررسی می‌شود.

برای مثال:

```text
Order Total
├── Subtotal
├── Discount
├── Shipping
└── Final Total
```

اکنون اگر نتیجه نهایی اشتباه باشد، می‌توانیم بررسی کنیم مشکل از کدام بخش است.

---

# Planning

پس از Decomposition، یک Plan ساده ایجاد می‌کنیم.

```text
Step 1 → Define product data
Step 2 → Calculate subtotal
Step 3 → Calculate discount
Step 4 → Calculate shipping
Step 5 → Calculate final total
Step 6 → Generate output
```

این Plan هنوز Code نیست.

اما مسیر تبدیل Requirement به Code را مشخص می‌کند.

---

# Algorithmic Thinking

در این مرحله باید بتوانیم مسئله را بدون وابستگی به Syntax توضیح دهیم.

مثلاً:

```text
شروع
↓
قیمت واحد را مشخص کن
↓
تعداد را مشخص کن
↓
Subtotal را محاسبه کن
↓
اگر Subtotal حداقل 100 بود
    Discount را محاسبه کن
در غیر این صورت
    Discount برابر صفر است
↓
اگر Subtotal حداقل 150 بود
    Shipping برابر صفر است
در غیر این صورت
    Shipping برابر 10 است
↓
Total را محاسبه کن
↓
نتیجه را نمایش بده
```

این یک **Algorithm ساده** است.

هنوز وارد الگوریتم‌های پیچیده نشده‌ایم.

در اینجا Algorithmic Thinking یعنی:

> تبدیل مسئله به مجموعه‌ای از مراحل منطقی و قابل اجرا.

---

# چرا قبل از کدنویسی Plan می‌نویسیم؟

اگر بدون Plan شروع کنیم، ممکن است:

```text
Code
↓
Error
↓
Change
↓
Another Error
↓
More Changes
```

ایجاد شود.

اما با یک Plan ساده:

```text
Problem
↓
Plan
↓
Implementation
```

کدنویسی هدفمندتر می‌شود.

Plan نباید طولانی باشد.

گاهی چند خط کافی است.

---

# Key Points

* Requirement مشخص می‌کند برنامه چه کاری باید انجام دهد.
* Decomposition مسئله را به بخش‌های کوچک‌تر تقسیم می‌کند.
* Plan مسیر اجرای راه‌حل را مشخص می‌کند.
* Algorithmic Thinking یعنی تبدیل مسئله به مراحل منطقی.
* Plan جایگزین Code نیست؛ Code نتیجه اجرای Plan است.

---

# Block 03 — Implementation

## Step 1 — Product Data

اکنون Plan را به Code تبدیل می‌کنیم.

ابتدا داده‌های سفارش را تعریف می‌کنیم:

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;
```

در اینجا مقدارهای ثابت را با `const` تعریف کرده‌ایم.

چون در طول اجرای این بخش از برنامه انتظار نداریم:

```javascript
productName
```

یا:

```javascript
unitPrice
```

تغییر کنند.

---

# Step 2 — Calculate Subtotal

اکنون باید قیمت کل قبل از Discount را محاسبه کنیم.

یک روش مستقیم:

```javascript
const subtotal = unitPrice * quantity;

console.log(subtotal);
```

خروجی:

```text
135
```

این ساده‌ترین راه برای محاسبه Subtotal است.

اما در این Challenge یک هدف آموزشی دیگر نیز داریم.

می‌خواهیم ببینیم چگونه می‌توان یک مسئله تکراری را با Loop حل کرد.

---

# استفاده از Loop برای محاسبه Subtotal

اگر بخواهیم قیمت محصول را برای هر واحد به‌صورت جداگانه به Subtotal اضافه کنیم، می‌توانیم بنویسیم:

```javascript
let subtotal = 0;
let i = 1;

while (i <= quantity) {
  subtotal += unitPrice;
  i++;
}

console.log(subtotal);
```

خروجی:

```text
135
```

روند اجرای Loop:

```text
subtotal = 0
i = 1
↓
45 اضافه می‌شود
↓
subtotal = 45
i = 2
↓
45 اضافه می‌شود
↓
subtotal = 90
i = 3
↓
45 اضافه می‌شود
↓
subtotal = 135
i = 4
↓
Condition = false
↓
Loop پایان می‌یابد
```

این مثال نشان می‌دهد که Loop چگونه می‌تواند یک عملیات تکراری را کنترل کند.

---

# چرا از `let` استفاده کردیم؟

در اینجا:

```javascript
let subtotal = 0;
```

از `let` استفاده کرده‌ایم.

زیرا مقدار `subtotal` در طول اجرای Loop تغییر می‌کند:

```javascript
subtotal += unitPrice;
```

همچنین:

```javascript
let i = 1;
```

نیز باید تغییر کند:

```javascript
i++;
```

بنابراین استفاده از `let` در این دو مورد با قاعده‌ای که در فصل 04 آموختیم سازگار است:

> اگر مقدار باید تغییر کند، از `let` استفاده کنید.

---

# Step 3 — Calculate Discount

اکنون باید بررسی کنیم که آیا سفارش شامل Discount می‌شود یا خیر.

Requirement می‌گوید:

```text
Subtotal >= 100
```

پس:

```javascript
let discount = 0;

if (subtotal >= 100) {
  discount = subtotal * 0.10;
}
```

اگر:

```text
subtotal = 135
```

باشد:

```text
discount = 13.5
```

و اگر:

```text
subtotal = 90
```

باشد، Condition برقرار نیست و:

```text
discount = 0
```

باقی می‌ماند.

---

# چرا مقدار اولیه `discount` صفر است؟

این تصمیم به ما اجازه می‌دهد هر دو حالت را با یک Variable مدیریت کنیم.

ابتدا:

```javascript
let discount = 0;
```

سپس فقط اگر شرط برقرار بود:

```javascript
discount = subtotal * 0.10;
```

مقدار را تغییر می‌دهیم.

بنابراین در پایان همیشه یک Value معتبر برای Discount داریم.

---

# Step 4 — Calculate Shipping

اکنون هزینه ارسال را مشخص می‌کنیم.

Requirement:

```text
Subtotal >= 150 → Free Shipping
Subtotal < 150 → Shipping = 10
```

می‌توانیم از `if...else` استفاده کنیم:

```javascript
let shipping;

if (subtotal >= 150) {
  shipping = 0;
} else {
  shipping = 10;
}
```

اکنون مقدار `shipping` بر اساس Condition تعیین می‌شود.

---

# استفاده از Ternary Operator

از آنجا که این تصمیم بسیار ساده است، می‌توانیم از Ternary Operator نیز استفاده کنیم:

```javascript
const shipping = subtotal >= 150 ? 0 : 10;
```

معنای آن:

```text
اگر subtotal >= 150
    shipping = 0
در غیر این صورت
    shipping = 10
```

این مثال یکی از کاربردهای مناسب Ternary Operator است.

Condition ساده است و هر دو نتیجه نیز مشخص هستند.

---

# Step 5 — Calculate Final Total

اکنون سه مقدار اصلی را داریم:

```text
Subtotal
Discount
Shipping
```

پس:

```javascript
const total = subtotal - discount + shipping;
```

برای مثال:

```text
135 - 13.5 + 10
```

نتیجه:

```text
131.5
```

است.

---

# Step 6 — Generate Output

اکنون باید نتیجه را به شکل قابل فهم نمایش دهیم.

برای این کار از Template Literal استفاده می‌کنیم:

```javascript
const message = `
Product: ${productName}
Quantity: ${quantity}
Subtotal: ${subtotal}
Discount: ${discount}
Shipping: ${shipping}
Total: ${total}
`;

console.log(message);
```

خروجی:

```text
Product: Keyboard
Quantity: 3
Subtotal: 135
Discount: 13.5
Shipping: 10
Total: 131.5
```

این همان کاربردی است که در فصل Strings و Template Literals درباره Dynamic Text بررسی کردیم.

---

# نسخه اولیه Challenge

اکنون می‌توانیم کل راه‌حل را کنار هم قرار دهیم:

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;

let subtotal = 0;
let i = 1;

while (i <= quantity) {
  subtotal += unitPrice;
  i++;
}

let discount = 0;

if (subtotal >= 100) {
  discount = subtotal * 0.10;
}

const shipping = subtotal >= 150 ? 0 : 10;

const total = subtotal - discount + shipping;

const message = `
Product: ${productName}
Quantity: ${quantity}
Subtotal: ${subtotal}
Discount: ${discount}
Shipping: ${shipping}
Total: ${total}
`;

console.log(message);
```

---

# تحلیل راه‌حل

اکنون به جای تمرکز روی هر Syntax، جریان کلی برنامه را بررسی کنیم.

```text
Product Data
↓
Subtotal
↓
Discount
↓
Shipping
↓
Total
↓
Output
```

هر بخش مسئول یک مرحله از مسئله است.

---

# Applying Fundamentals

در این Challenge چند فصل قبلی را هم‌زمان به کار بردیم.

### Variables

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;
```

---

### Operators

```javascript
subtotal += unitPrice;
```

و:

```javascript
subtotal - discount + shipping
```

---

### Conditions

```javascript
if (subtotal >= 100) {
  discount = subtotal * 0.10;
}
```

---

### Loop

```javascript
while (i <= quantity) {
  subtotal += unitPrice;
  i++;
}
```

---

### Ternary Operator

```javascript
const shipping = subtotal >= 150 ? 0 : 10;
```

---

### Template Literal

```javascript
const message = `
Product: ${productName}
...
`;
```

---

# Testing the Solution

نوشتن Code به معنای تمام شدن کار نیست.

اکنون باید بررسی کنیم آیا برنامه واقعاً Requirements را اجرا می‌کند یا خیر.

این کار را با **Test Case** انجام می‌دهیم.

---

# Test Case 1 — Discount Applied

Input:

```text
unitPrice = 45
quantity = 3
```

محاسبه:

```text
Subtotal = 135
Discount = 13.5
Shipping = 10
Total = 131.5
```

Expected Result:

```text
Total = 131.5
```

---

# Test Case 2 — No Discount

Input:

```text
unitPrice = 45
quantity = 2
```

محاسبه:

```text
Subtotal = 90
Discount = 0
Shipping = 10
Total = 100
```

Expected Result:

```text
Total = 100
```

این Test Case بررسی می‌کند که Condition مربوط به Discount به‌درستی عمل می‌کند.

---

# Test Case 3 — Free Shipping

Input:

```text
unitPrice = 50
quantity = 3
```

محاسبه:

```text
Subtotal = 150
Discount = 15
Shipping = 0
Total = 135
```

این Test Case مرز Free Shipping را بررسی می‌کند.

---

# چرا Boundaryها مهم هستند؟

به‌جای اینکه فقط یک مقدار معمولی را آزمایش کنیم، باید مقادیری را نیز بررسی کنیم که نزدیک به مرز Condition هستند.

برای مثال:

```javascript
subtotal >= 100
```

دو مقدار بسیار مهم:

```text
99
100
```

هستند.

زیرا در:

```text
99
```

Discount نباید اعمال شود.

اما در:

```text
100
```

باید اعمال شود.

همین موضوع درباره:

```javascript
subtotal >= 150
```

نیز وجود دارد.

مقادیر مهم:

```text
149
150
```

هستند.

این نوع Test Caseها می‌توانند Bugهای منطقی را سریع‌تر آشکار کنند.

---

# Testing Checklist

برای این Challenge حداقل باید موارد زیر را بررسی کنیم:

```text
□ Subtotal کمتر از 100
□ Subtotal برابر 100
□ Subtotal بین 100 و 149
□ Subtotal برابر 150
□ Subtotal بیشتر از 150
```

هدف این نیست که فقط «یک بار» برنامه را اجرا کنیم.

هدف این است که رفتار برنامه را در شرایط مهم بررسی کنیم.

---

# Common Mistakes

### اشتباه اول — شروع مستقیم با Code

برنامه‌نویس بلافاصله شروع به نوشتن کد می‌کند بدون اینکه Requirements را مشخص کند.

مشکل:

```text
Problem
↓
Code
```

به‌جای:

```text
Problem
↓
Requirements
↓
Plan
↓
Code
```

---

### اشتباه دوم — نادیده گرفتن Boundaryها

ممکن است برنامه برای:

```text
135
```

درست کار کند، اما برای:

```text
100
```

رفتار اشتباه داشته باشد.

---

### اشتباه سوم — تغییر چند بخش هم‌زمان

اگر برنامه نتیجه اشتباه داد، نباید بدون تحلیل چند خط مختلف را هم‌زمان تغییر دهیم.

ابتدا باید مشخص کنیم:

```text
کدام مقدار اشتباه است؟
↓
کدام مرحله آن را تولید کرده؟
↓
کدام Condition یا Calculation مسئول است؟
```

---

### اشتباه چهارم — تصور اینکه اجرای موفق یعنی برنامه صحیح است

ممکن است برنامه بدون هیچ Runtime Error اجرا شود اما نتیجه اشتباه باشد.

برای مثال:

```javascript
const total = subtotal + discount + shipping;
```

از نظر Syntax معتبر است.

اما از نظر منطق کسب‌وکار اشتباه است.

زیرا Discount باید از Subtotal کم شود.

این یک **Logical Bug** است.

---

# Key Points

* Implementation بعد از Analysis و Planning انجام می‌شود.
* یک برنامه ممکن است بدون Error اجرا شود اما هنوز Bug داشته باشد.
* Testing باید با Test Caseهای مشخص انجام شود.
* Boundary Valueها برای بررسی Conditions اهمیت زیادی دارند.
* اجرای موفق Code به‌تنهایی صحت Logic را اثبات نمی‌کند.

---

# Block 04 — Code Review

## Debugging

اکنون فرض کنید برنامه در ظاهر اجرا می‌شود، اما Total اشتباه است.

برای مثال، برنامه چنین خروجی‌ای می‌دهد:

```text
Product: Keyboard
Quantity: 3
Subtotal: 135
Discount: 13.5
Shipping: 10
Total: 158.5
```

اما ما انتظار داشتیم:

```text
Total: 131.5
```

در اینجا Runtime Error نداریم.

برنامه اجرا شده است.

اما نتیجه اشتباه است.

پس با یک **Bug منطقی** مواجه هستیم.

---

# Finding the Bug

ابتدا مقدارهای مهم را بررسی می‌کنیم:

```javascript
console.log(subtotal);
console.log(discount);
console.log(shipping);
console.log(total);
```

اگر خروجی:

```text
135
13.5
10
158.5
```

باشد، مشخص است که:

```text
subtotal
```

درست است.

```text
discount
```

درست است.

```text
shipping
```

نیز درست است.

پس باید Calculation مربوط به Total را بررسی کنیم.

---

# Bug

فرض کنید Code به اشتباه چنین نوشته شده باشد:

```javascript
const total = subtotal + discount + shipping;
```

اکنون با جایگذاری Valueها:

```text
135 + 13.5 + 10
```

به:

```text
158.5
```

می‌رسیم.

اما Requirement می‌گوید Discount باید از قیمت کم شود.

بنابراین Calculation صحیح:

```javascript
const total = subtotal - discount + shipping;
```

است.

---

# Debugging با Breakpoint

اگر بخواهیم وضعیت برنامه را دقیق‌تر مشاهده کنیم، می‌توانیم از Breakpoint استفاده کنیم.

مثلاً:

```javascript
const shipping = subtotal >= 150 ? 0 : 10;

const total = subtotal - discount + shipping;
```

می‌توانیم روی خط محاسبه Total یک Breakpoint قرار دهیم.

سپس هنگام توقف اجرای برنامه، مقدارهای:

```text
subtotal
discount
shipping
```

را بررسی کنیم.

این دقیقاً همان نوع استفاده‌ای است که در Chapter 10 برای Breakpoint و مشاهده وضعیت Runtime بررسی کردیم.

---

# Debugging Workflow

برای این Bug می‌توانیم Workflow زیر را اجرا کنیم:

```text
Observed Result
↓
Expected Result
↓
Compare
↓
Identify Wrong Value
↓
Trace Calculation
↓
Inspect Variables
↓
Find Faulty Logic
↓
Fix
↓
Test Again
```

این فرآیند بسیار مهم‌تر از این است که صرفاً یک خط را تغییر دهیم تا برنامه «ظاهراً» درست شود.

---

# Refactoring

اکنون فرض کنید همه Test Caseها موفق شده‌اند.

آیا کار تمام است؟

هنوز نه.

اکنون باید Code را Review کنیم.

در نسخه اولیه برای محاسبه Subtotal از Loop استفاده کردیم:

```javascript
let subtotal = 0;
let i = 1;

while (i <= quantity) {
  subtotal += unitPrice;
  i++;
}
```

این Code صحیح است.

اما آیا بهترین راه برای این مسئله است؟

---

# آیا Loop واقعاً لازم است؟

مسئله می‌گوید:

```text
Subtotal = Unit Price × Quantity
```

پس می‌توانیم مستقیماً بنویسیم:

```javascript
const subtotal = unitPrice * quantity;
```

این نسخه:

* کوتاه‌تر است.
* خواناتر است.
* State کمتری دارد.
* احتمال خطای کمتری دارد.
* Intent را مستقیم‌تر نشان می‌دهد.

بنابراین Loop از نظر فنی می‌تواند کار کند، اما برای این مسئله **ضروری نیست**.

---

# این Refactoring چه چیزی به ما یاد می‌دهد؟

یک نکته مهم مهندسی:

> **استفاده از یک Feature به‌خودی‌خود نشانه طراحی بهتر نیست.**

ما در فصل Loops یاد گرفتیم چگونه عملیات تکراری را انجام دهیم.

اما اکنون باید بتوانیم تشخیص دهیم:

> آیا واقعاً به Loop نیاز داریم؟

اگر یک Operator ساده مسئله را بهتر حل کند، استفاده از Loop فقط به دلیل اینکه آن را بلدیم، تصمیم مناسبی نیست.

---

# نسخه Refactored

نسخه ساده‌تر برنامه:

```javascript
const productName = 'Keyboard';
const unitPrice = 45;
const quantity = 3;

const subtotal = unitPrice * quantity;

let discount = 0;

if (subtotal >= 100) {
  discount = subtotal * 0.10;
}

const shipping = subtotal >= 150 ? 0 : 10;

const total = subtotal - discount + shipping;

const message = `
Product: ${productName}
Quantity: ${quantity}
Subtotal: ${subtotal}
Discount: ${discount}
Shipping: ${shipping}
Total: ${total}
`;

console.log(message);
```

---

# مقایسه دو راه‌حل

### راه‌حل اول

```javascript
let subtotal = 0;
let i = 1;

while (i <= quantity) {
  subtotal += unitPrice;
  i++;
}
```

مزیت:

* تمرین خوبی برای Loop است.

محدودیت:

* برای مسئله فعلی پیچیده‌تر از نیاز واقعی است.

---

### راه‌حل دوم

```javascript
const subtotal = unitPrice * quantity;
```

مزیت:

* مستقیم است.
* خواناتر است.
* State کمتری دارد.
* Intent را واضح‌تر نشان می‌دهد.

برای Application واقعی، راه‌حل دوم مناسب‌تر است.

---

# Alternative Solutions

گاهی یک مسئله بیش از یک راه‌حل معتبر دارد.

برای مثال، Shipping را می‌توان با `if...else` نوشت:

```javascript
let shipping;

if (subtotal >= 150) {
  shipping = 0;
} else {
  shipping = 10;
}
```

یا:

```javascript
const shipping = subtotal >= 150 ? 0 : 10;
```

هر دو رفتار یکسانی دارند.

اما انتخاب میان آن‌ها باید بر اساس خوانایی باشد.

---

# چه زمانی `if...else` بهتر است؟

اگر Logic پیچیده شود:

```javascript
if (subtotal >= 500) {
  shipping = 0;
} else if (subtotal >= 300) {
  shipping = 5;
} else if (subtotal >= 150) {
  shipping = 10;
} else {
  shipping = 20;
}
```

در چنین شرایطی `if...else` خواناتر است.

---

# چه زمانی Ternary بهتر است؟

برای تصمیم‌های بسیار ساده:

```javascript
const shipping = subtotal >= 150 ? 0 : 10;
```

Ternary می‌تواند مناسب باشد.

بنابراین هدف:

> کوتاه‌ترین Code

نیست.

هدف:

> **خواناترین Code برای مسئله موردنظر**

است.

---

# Clean Code در این سطح

در این مرحله هنوز وارد اصول پیشرفته Clean Code نشده‌ایم.

اما چند اصل ساده را می‌توانیم رعایت کنیم.

### نام‌های معنادار

به‌جای:

```javascript
const x = 45;
```

بنویسیم:

```javascript
const unitPrice = 45;
```

نام Variable بخشی از توضیح برنامه است.

---

### یکدست بودن منطق

بهتر است مراحل محاسبه به ترتیب منطقی نوشته شوند:

```text
Subtotal
↓
Discount
↓
Shipping
↓
Total
```

این ترتیب Code را با مدل ذهنی مسئله هماهنگ می‌کند.

---

### پرهیز از پیچیدگی غیرضروری

اگر:

```javascript
const subtotal = unitPrice * quantity;
```

مسئله را حل می‌کند، استفاده از Loop برای همین کار پیچیدگی اضافی ایجاد می‌کند.

---

# Code Review Questions

در یک Review فنی می‌توانیم این سؤال‌ها را مطرح کنیم:

### آیا Code درست است؟

آیا تمام Requirements اجرا می‌شوند؟

### آیا Code خوانا است؟

آیا یک برنامه‌نویس دیگر می‌تواند Logic را سریع درک کند؟

### آیا پیچیدگی غیرضروری وجود دارد؟

آیا جایی از Loop یا Condition اضافه استفاده شده است؟

### آیا Boundaryها بررسی شده‌اند؟

آیا:

```text
100
150
```

به‌درستی آزمایش شده‌اند؟

### آیا نام‌ها معنادار هستند؟

آیا:

```javascript
subtotal
discount
shipping
total
```

رفتار Variableها را توضیح می‌دهند؟

---

# Common Mistakes

### اشتباه اول — مساوی دانستن Short Code و Clean Code

کوتاه بودن Code به‌تنهایی معیار کیفیت نیست.

گاهی Code کوتاه‌تر، خوانایی کمتری دارد.

---

### اشتباه دوم — استفاده از Loop فقط برای نشان دادن دانش Loop

دانستن یک Feature به معنای استفاده از آن در همه مسائل نیست.

---

### اشتباه سوم — Refactoring بدون Test

نباید ابتدا Code را تغییر دهیم و سپس ببینیم چه اتفاقی افتاده است.

Workflow بهتر:

```text
Working Code
↓
Tests
↓
Refactor
↓
Run Tests Again
```

---

### اشتباه چهارم — تغییر رفتار هنگام Refactoring

هدف Refactoring این است که ساختار Code بهتر شود، بدون اینکه رفتار مورد انتظار تغییر کند.

در این مثال:

```javascript
const subtotal = unitPrice * quantity;
```

جایگزین Loop شد، اما نتیجه باید همچنان همان باشد.

---

# Alternative Thinking

یک برنامه‌نویس حرفه‌ای فقط نمی‌پرسد:

> آیا این Code کار می‌کند؟

بلکه سؤال‌های بیشتری می‌پرسد:

> آیا این Code ساده‌ترین راه مناسب برای این مسئله است؟

> آیا رفتار آن قابل پیش‌بینی است؟

> آیا اگر Requirement تغییر کند، فهم آن آسان خواهد بود؟

> آیا شخص دیگری می‌تواند آن را به‌راحتی بررسی کند؟

این تغییر نوع سؤال پرسیدن، بخشی از **Engineering Thinking** است.

---

# Key Points

* Debugging فقط برای Runtime Error نیست؛ Logical Bug نیز باید Debug شود.
* Breakpoint به ما اجازه می‌دهد وضعیت برنامه را در یک نقطه مشخص بررسی کنیم.
* Refactoring باید رفتار مورد انتظار برنامه را حفظ کند.
* راه‌حل صحیح همیشه بهترین طراحی نیست.
* Featureها باید بر اساس نیاز مسئله انتخاب شوند، نه صرفاً بر اساس اینکه آن‌ها را بلد هستیم.
* خوانایی و سادگی از معیارهای مهم Code Review هستند.

---

# Final Review

## مسیر کامل Challenge

اکنون کل فرآیند را از ابتدا تا انتها مرور کنیم.

```text
Problem
↓
Checkout Total Calculator
```

ابتدا Requirements را مشخص کردیم:

```text
Product
Quantity
Discount
Shipping
Total
```

سپس مسئله را Decompose کردیم:

```text
Product Data
↓
Subtotal
↓
Discount
↓
Shipping
↓
Total
↓
Output
```

سپس Plan ایجاد کردیم.

بعد Plan را به Code تبدیل کردیم.

سپس Test Case طراحی کردیم.

در ادامه یک Logical Bug را بررسی کردیم.

در نهایت Code را Review و Refactor کردیم.

این همان چرخه‌ای است که باید در ذهن برنامه‌نویس شکل بگیرد:

```text
Problem
↓
Understand
↓
Plan
↓
Implement
↓
Test
↓
Debug
↓
Refactor
↓
Review
```

---

# Concepts Covered

در این Challenge مفاهیم زیر را به‌صورت یکپارچه به کار بردیم:

| Concept              | کاربرد در Challenge       |
| -------------------- | ------------------------- |
| Variable             | نگهداری داده‌های سفارش    |
| String               | نام محصول و خروجی         |
| Number               | قیمت، تعداد و محاسبات     |
| `const`              | مقادیر ثابت               |
| `let`                | مقادیر قابل تغییر         |
| Arithmetic Operators | محاسبه قیمت               |
| Comparison Operators | بررسی شرایط               |
| `if`                 | اعمال Discount            |
| Ternary Operator     | تعیین Shipping            |
| `while`              | پیاده‌سازی اولیه Subtotal |
| Template Literal     | تولید گزارش سفارش         |
| Console              | مشاهده نتیجه              |
| Breakpoint           | بررسی وضعیت Runtime       |
| Debugging            | پیدا کردن Logical Bug     |
| Testing              | بررسی رفتار برنامه        |
| Refactoring          | ساده‌سازی راه‌حل          |

این دقیقاً هدف Coding Challenge است:

> **مفاهیم جداگانه را به یک سیستم کوچک اما واقعی تبدیل کنیم.**

---

# یک نکته مهم درباره مرز این فصل

در این Challenge عمداً برنامه را با چند Variable و ساختارهای پایه پیاده‌سازی کردیم.

در یک Application واقعی، احتمالاً اطلاعات چندین Product را باید مدیریت کنیم.

در آن شرایط، ساختارهایی مانند:

```text
Array
Object
Function
```

می‌توانند طراحی بهتری ایجاد کنند.

اما این فصل هنوز زمان آموزش آن مفاهیم نیست.

در نتیجه، آن‌ها را وارد Implementation نمی‌کنیم.

این محدودیت بخشی از طراحی آموزشی کتاب است:

> **هر مفهوم باید در جایگاه مناسب خود آموزش داده شود.**

در فصل‌های بعدی، همین مسئله‌ها را می‌توان با ابزارهای قدرتمندتر و ساختارهای مناسب‌تر بازطراحی کرد.

---

# مدل ذهنی نهایی

پس از پایان Fundamentals نباید مدل ذهنی ما این باشد:

```text
I know variables.
I know loops.
I know conditions.
```

مدل ذهنی بهتر این است:

```text
I have a problem.
        ↓
I understand the requirements.
        ↓
I break the problem down.
        ↓
I create a plan.
        ↓
I implement the plan.
        ↓
I test the result.
        ↓
I debug incorrect behavior.
        ↓
I refactor the solution.
        ↓
I review the final code.
```

این تغییر، یکی از مهم‌ترین اهداف بخش Fundamentals است.

---

# Summary

در این فصل برای اولین‌بار به‌جای بررسی یک Syntax یا Feature مستقل، یک مسئله کامل را از ابتدا تا انتها حل کردیم.

مسئله ما یک **Checkout Total Calculator** ساده بود.

ابتدا Requirementهای مسئله را مشخص کردیم و سپس آن را به بخش‌های کوچک‌تر تقسیم کردیم.

بعد از آن یک Plan ایجاد کردیم و Plan را به Code تبدیل کردیم.

در Implementation از مفاهیمی مانند Variables، Operators، Conditions، Loops و Template Literals استفاده کردیم.

سپس با Test Caseهای مختلف رفتار برنامه را بررسی کردیم.

در ادامه دیدیم که یک برنامه می‌تواند بدون Runtime Error اجرا شود اما همچنان دارای Logical Bug باشد.

برای پیدا کردن Bug از بررسی Valueها و Debugging استفاده کردیم.

در پایان نیز Code را Review کردیم و متوجه شدیم که اگرچه Loop برای محاسبه Subtotal صحیح است، اما برای این مسئله ضرورتی ندارد و:

```javascript
const subtotal = unitPrice * quantity;
```

راه‌حل ساده‌تر و خواناتری است.

بنابراین Coding Challenge فقط درباره نوشتن Code نیست.

بلکه درباره **فرآیند تبدیل یک Problem به یک Solution قابل اعتماد** است.

---

# Key Takeaways

* حل مسئله با Code شروع نمی‌شود؛ با فهم Problem شروع می‌شود.
* Requirements مشخص می‌کنند برنامه چه رفتاری باید داشته باشد.
* Decomposition یک مسئله را به بخش‌های کوچک‌تر تقسیم می‌کند.
* قبل از Implementation بهتر است یک Plan ساده داشته باشیم.
* یک راه‌حل می‌تواند از نظر Syntax صحیح باشد اما از نظر Logic اشتباه باشد.
* Testing باید بر اساس Test Caseهای مشخص انجام شود.
* Boundary Valueها برای بررسی Conditions اهمیت زیادی دارند.
* Debugging بخشی طبیعی از فرآیند توسعه نرم‌افزار است.
* Refactoring باید بدون تغییر رفتار مورد انتظار انجام شود.
* کوتاه‌ترین Code الزاماً بهترین Code نیست.
* استفاده از Featureها باید بر اساس نیاز مسئله باشد.
* خوانایی، سادگی و قابلیت بررسی از معیارهای مهم Code Review هستند.
* Coding Challenge نقطه اتصال مفاهیم Fundamentals به Problem Solving است.

---

# Technical Interview

## سطح Junior

### سؤال ۱

چرا قبل از نوشتن Code باید Requirements را مشخص کنیم؟

### پاسخ

زیرا بدون مشخص کردن رفتار مورد انتظار، نمی‌دانیم Code باید دقیقاً چه مسئله‌ای را حل کند و چگونه صحت آن را بررسی کنیم.

---

### سؤال ۲

Decomposition در Problem Solving چیست؟

### پاسخ

Decomposition یعنی تقسیم یک مسئله بزرگ‌تر به چند مسئله کوچک‌تر و قابل مدیریت تا بتوان هر بخش را جداگانه تحلیل و پیاده‌سازی کرد.

---

### سؤال ۳

آیا اجرای بدون Error به معنای صحیح بودن برنامه است؟

### پاسخ

خیر. یک برنامه ممکن است بدون Syntax یا Runtime Error اجرا شود اما به دلیل Logic اشتباه، نتیجه نادرست تولید کند.

---

### سؤال ۴

چرا Test Case طراحی می‌کنیم؟

### پاسخ

برای بررسی اینکه برنامه در شرایط مشخص همان رفتاری را دارد که Requirementها تعیین کرده‌اند.

---

### سؤال ۵

چرا Boundary Valueها مهم هستند؟

### پاسخ

زیرا بسیاری از خطاهای منطقی در مرز Conditionها رخ می‌دهند؛ مثلاً تفاوت رفتار برای `99` و `100` زمانی که شرط `>= 100` است.

---

### سؤال ۶

Refactoring چیست؟

### پاسخ

Refactoring یعنی بهبود ساختار داخلی Code بدون تغییر رفتار مورد انتظار آن.

---

## سطح Mid-Level

### سؤال ۷

چرا نباید مستقیماً از Problem به Implementation برویم؟

### پاسخ

زیرا بدون Analysis و Planning معمولاً مسئله به‌درستی شکسته نمی‌شود و احتمال ایجاد پیچیدگی، خطا و تغییرات پراکنده بیشتر می‌شود.

---

### سؤال ۸

تفاوت Testing و Debugging چیست؟

### پاسخ

Testing بررسی می‌کند که آیا برنامه رفتار مورد انتظار را دارد یا خیر. Debugging فرآیند پیدا کردن علت رفتار نادرست و اصلاح آن است.

---

### سؤال ۹

اگر یک Program بدون Runtime Error اجرا شود اما نتیجه اشتباه باشد، چگونه آن را بررسی می‌کنید؟

### پاسخ

ابتدا Expected Result را با Actual Result مقایسه می‌کنم، سپس Valueهای میانی را بررسی می‌کنم تا مشخص شود خطا در کدام مرحله از Logic ایجاد شده است. در صورت نیاز از Console یا Breakpoint برای مشاهده وضعیت Runtime استفاده می‌کنم.

---

### سؤال ۱۰

چرا استفاده از Loop برای محاسبه Subtotal در Challenge الزاماً بهترین راه‌حل نیست؟

### پاسخ

زیرا مسئله مستقیماً با رابطه `unitPrice × quantity` قابل حل است. Loop رفتار صحیحی دارد، اما State و Complexity غیرضروری ایجاد می‌کند و Intent کد را کمتر مستقیم نشان می‌دهد.

---

### سؤال ۱۱

چه زمانی Ternary Operator نسبت به `if...else` مناسب‌تر است؟

### پاسخ

زمانی که Condition ساده باشد و تنها دو نتیجه مشخص داشته باشیم. اگر Logic پیچیده یا چندمرحله‌ای شود، `if...else` معمولاً خواناتر است.

---

### سؤال ۱۲

چرا Refactoring باید بعد از Test انجام شود؟

### پاسخ

زیرا ابتدا باید رفتار فعلی را با Test Caseها مشخص کنیم تا پس از تغییر ساختار بتوانیم بررسی کنیم که رفتار برنامه تغییر نکرده است.

---

## سطح Senior

### سؤال ۱۳

آیا یک راه‌حل صحیح لزوماً یک راه‌حل مهندسی خوب است؟

### پاسخ

خیر. Correctness شرط لازم است، اما کیفیت طراحی به عواملی مانند سادگی، خوانایی، قابلیت نگهداری و تناسب راه‌حل با مسئله نیز وابسته است.

---

### سؤال ۱۴

چرا استفاده از یک Feature فقط برای نشان دادن دانش آن Feature تصمیم مناسبی نیست؟

### پاسخ

زیرا ابزار باید بر اساس نیاز Problem انتخاب شود. استفاده غیرضروری از Feature می‌تواند Complexity و Cognitive Load را افزایش دهد، حتی اگر Code از نظر فنی معتبر باشد.

---

### سؤال ۱۵

چگونه تشخیص می‌دهید که یک Code واقعاً نیاز به Refactoring دارد؟

### پاسخ

اگر Code رفتار صحیح دارد اما دارای Complexity غیرضروری، تکرار، نام‌گذاری ضعیف یا ساختاری باشد که فهم و تغییر آن دشوار است، می‌توان آن را Candidate برای Refactoring دانست.

---

### سؤال ۱۶

چرا Testing فقط بررسی یک خروجی معمولی کافی نیست؟

### پاسخ

زیرا یک برنامه ممکن است در حالت عادی درست باشد اما در مرزها یا شرایط خاص رفتار نادرست داشته باشد. بنابراین باید Test Caseهایی طراحی شوند که حالت‌های عادی، مرزی و مهم Requirement را پوشش دهند.

---

### سؤال ۱۷

رابطه میان Problem Decomposition و Debugging چیست؟

### پاسخ

وقتی Problem به مراحل مشخص تقسیم شده باشد، می‌توانیم هر مرحله را جداگانه بررسی کنیم و محل تولید Value اشتباه را سریع‌تر پیدا کنیم. بنابراین Decomposition نه‌تنها برای Implementation، بلکه برای Debugging نیز مفید است.

---

### سؤال ۱۸

یک Engineer چگونه بین دو راه‌حل صحیح انتخاب می‌کند؟

### پاسخ

با مقایسه عواملی مانند Readability، Complexity، Maintainability، احتمال خطا و تناسب راه‌حل با Requirement. هدف صرفاً کمترین تعداد خطوط Code نیست، بلکه انتخاب راه‌حلی است که رفتار مورد نیاز را با پیچیدگی مناسب بیان کند.

---

# Golden Answers

## چرا Problem Solving قبل از Coding قرار می‌گیرد؟

زیرا Code باید نتیجه یک درک مشخص از Problem باشد. ابتدا Requirementها و مراحل حل را مشخص می‌کنیم و سپس آن‌ها را به Syntax تبدیل می‌کنیم.

---

## Decomposition چیست؟

Decomposition یعنی شکستن یک Problem به بخش‌های کوچک‌تر و قابل مدیریت تا بتوان هر بخش را مستقل‌تر تحلیل، پیاده‌سازی و آزمایش کرد.

---

## آیا اجرای بدون Error یعنی برنامه صحیح است؟

خیر. Error و Bug یکسان نیستند. برنامه می‌تواند بدون Runtime Error اجرا شود اما به دلیل Logic اشتباه، نتیجه نادرست تولید کند.

---

## Testing و Debugging چه تفاوتی دارند؟

Testing مشخص می‌کند آیا رفتار برنامه با Expected Result مطابقت دارد یا خیر. Debugging علت رفتار نادرست را پیدا و اصلاح می‌کند.

---

## Refactoring چیست؟

Refactoring بهبود ساختار داخلی Code بدون تغییر رفتار مورد انتظار آن است.

---

## آیا کوتاه‌ترین راه‌حل بهترین راه‌حل است؟

خیر. معیار اصلی، تناسب با مسئله، Readability، سادگی، قابلیت نگهداری و عدم ایجاد Complexity غیرضروری است.

---

## پاسخ کوتاه طلایی مصاحبه

**سؤال:** هنگام حل یک مسئله نرم‌افزاری از کجا شروع می‌کنید؟

**پاسخ:** ابتدا Problem و Requirements را مشخص می‌کنم، سپس آن را به بخش‌های کوچک‌تر تقسیم و یک Plan ایجاد می‌کنم. بعد Implementation، Testing، Debugging و در نهایت Refactoring و Review را انجام می‌دهم.

---

# Conclusion

تا اینجا JavaScript را بیشتر از زاویه قابلیت‌های زبان بررسی کردیم.

اکنون یک قدم مهم‌تر برداشته‌ایم.

یاد گرفتیم که این قابلیت‌ها به‌صورت جداگانه ارزش محدودی دارند؛ ارزش واقعی آن‌ها زمانی آشکار می‌شود که بتوانیم از آن‌ها برای حل یک Problem استفاده کنیم.

در این Challenge:

```text
Variables
+
Operators
+
Conditions
+
Loops
+
Strings
+
Template Literals
+
Debugging
```

به یک Solution واحد تبدیل شدند.

اما مهم‌تر از خود Code، فرآیندی بود که برای رسیدن به آن طی کردیم:

```text
Problem
↓
Requirements
↓
Decomposition
↓
Plan
↓
Implementation
↓
Testing
↓
Debugging
↓
Refactoring
↓
Review
```

این فرآیند، پایان Fundamentals و آغاز یک مدل ذهنی مهندسی‌تر برای یادگیری JavaScript است.

در بخش بعدی کتاب، از سطح Syntax و Problem Solving به درون JavaScript حرکت خواهیم کرد و بررسی خواهیم کرد که **Source Code چگونه توسط JavaScript Engine پردازش و اجرا می‌شود.**

این موضوع در Chapter 12 بررسی خواهد شد.
