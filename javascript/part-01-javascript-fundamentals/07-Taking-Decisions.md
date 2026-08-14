# Chapter 07 — Taking Decisions

---

# Chapter Goal

پس از مطالعه این فصل، انتظار می‌رود بتوانید:

- مفهوم **Decision Making** را در برنامه‌نویسی توضیح دهید.
- نقش `Boolean` و **Condition** را در کنترل مسیر اجرای برنامه درک کنید.
- تفاوت `if`، `else if` و `else` را توضیح دهید.
- مفهوم **Truthy** و **Falsy** را در یک **Boolean Context** تحلیل کنید.
- تبدیل یک Value به Boolean را در تصمیم‌گیری درک کنید.
- تفاوت `===` و `==` را در شرایط واقعی تشخیص دهید.
- از `switch` در موقعیت مناسب استفاده کنید.
- ساختارهای شرطی را از نظر خوانایی و Maintainability ارزیابی کنید.
- الگوهای ساده‌ای مانند **Guard Clause** و **Conditional Operator** را در سطح مقدماتی بشناسید.
- ارتباط مفاهیم این فصل با Values، Variables، Data Types، Operators و Strings را درک کنید.
- به پرسش‌های فنی مرتبط با Decision Making پاسخ دهید.

---

# Core Question

> **JavaScript چگونه اجرای برنامه را بر اساس شرایط مختلف کنترل می‌کند؟**

---

# Concept Flow

```text
Boolean
↓
Condition
↓
if
↓
else
↓
else if
↓
Nested Conditions
↓
Truthy / Falsy
↓
Boolean Conversion
↓
Equality
↓
switch
↓
Conditional Patterns
↓
Best Practices
```

---

# مقدمه

در فصل‌های قبل، قدم‌به‌قدم با مفاهیمی آشنا شدیم که اکنون برای ساختن منطق واقعی برنامه به یکدیگر متصل می‌شوند.

در فصل دوم با **Value** و **Variable** آشنا شدیم.

در فصل سوم دیدیم که Valueها دارای **Type** هستند و یکی از این Typeها `Boolean` است.

در فصل چهارم با `let` و `const` یاد گرفتیم چگونه Variableها را تعریف و مدیریت کنیم.

در فصل پنجم با **Operators** و **Expressions** آشنا شدیم و دیدیم که Comparison Operatorها می‌توانند نتیجه‌ای از نوع Boolean تولید کنند.

در فصل ششم نیز با **String** و **Template Literal** آشنا شدیم و یاد گرفتیم چگونه داده‌های متنی را تولید کنیم.

اکنون یک سؤال جدید مطرح می‌شود:

> اگر یک Expression نتیجه‌ای مانند `true` یا `false` تولید کند، JavaScript چگونه از این نتیجه برای تغییر مسیر اجرای برنامه استفاده می‌کند؟

برای مثال:

```javascript
const age = 20;

if (age >= 18) {
  console.log('Access granted.');
}
```

در اینجا چند مفهوم قبلی به یکدیگر متصل شده‌اند:

```text
Variable
↓
Value
↓
Number
↓
Comparison Operator
↓
Boolean Result
↓
Condition
↓
Decision
```

بنابراین این فصل یک مفهوم کاملاً جدا از فصل‌های قبل نیست.

بلکه نقطه‌ای است که بسیاری از مفاهیم پایه JavaScript برای اولین بار در قالب **Control Flow** به یکدیگر متصل می‌شوند.

---

# Block 01 — Conditional Logic

## چرا برنامه‌ها باید تصمیم بگیرند؟

اگر تمام دستورات یک برنامه همیشه دقیقاً به یک ترتیب اجرا شوند، برنامه نمی‌تواند به وضعیت‌های مختلف واکنش مناسب نشان دهد.

یک Application واقعی باید بتواند بر اساس داده‌های موجود تصمیم بگیرد.

برای مثال:

```javascript
const isLoggedIn = true;

if (isLoggedIn) {
  console.log('Welcome back.');
}
```

یا:

```javascript
const stock = 0;

if (stock > 0) {
  console.log('Product is available.');
}
```

در هر دو مثال، برنامه یک وضعیت را بررسی می‌کند و بر اساس نتیجه، تصمیم می‌گیرد که آیا بخشی از کد اجرا شود یا خیر.

این مفهوم را **Decision Making** می‌نامیم.

---

# Boolean Review

در فصل Data Types دیدیم که `Boolean` فقط دو Value دارد:

```javascript
true
false
```

این دو Value برای نمایش وضعیت‌های منطقی بسیار مناسب‌اند.

برای مثال:

```javascript
const isLoggedIn = true;
const hasPermission = false;
```

در اینجا:

```text
isLoggedIn   → true
hasPermission → false
```

این Valueها می‌توانند مستقیماً در تصمیم‌گیری استفاده شوند.

```javascript
if (isLoggedIn) {
  console.log('Show dashboard.');
}
```

---

# Condition چیست؟

**Condition** عبارتی است که نتیجه آن مشخص می‌کند کدام مسیر اجرای برنامه باید انتخاب شود.

برای مثال:

```javascript
age >= 18
```

یک Condition مناسب است.

اگر:

```javascript
const age = 20;
```

باشد، نتیجه:

```text
true
```

خواهد بود.

اگر:

```javascript
const age = 15;
```

باشد، نتیجه:

```text
false
```

خواهد بود.

بنابراین می‌توانیم مدل ذهنی زیر را در نظر بگیریم:

```text
Condition
↓
Boolean Result
↓
Decision
↓
Control Flow
```

---

# Boolean Expression

در فصل Operators دیدیم که Comparison Operatorها نتیجه‌ای از نوع Boolean تولید می‌کنند.

برای مثال:

```javascript
10 > 5;
```

نتیجه:

```text
true
```

یا:

```javascript
10 === 20;
```

نتیجه:

```text
false
```

چنین Expressionهایی می‌توانند به‌عنوان Condition استفاده شوند.

```javascript
const score = 75;

if (score >= 60) {
  console.log('Passed.');
}
```

روند منطقی این کد:

```text
score >= 60
↓
true
↓
اجرای Block
```

است.

این همان ارتباط مستقیم میان فصل پنجم و فصل حاضر است:

```text
Operator
↓
Expression
↓
Boolean Result
↓
Condition
↓
Decision
```

---

# if Statement

ساده‌ترین ساختار تصمیم‌گیری در JavaScript، `if` است.

```javascript
if (condition) {
  // code
}
```

اگر Condition برابر `true` باشد، Block مربوط به `if` اجرا می‌شود.

مثال:

```javascript
const age = 21;

if (age >= 18) {
  console.log('Access granted.');
}
```

در این مثال:

```javascript
age >= 18
```

Condition است.

چون نتیجه `true` است، کد داخل Block اجرا می‌شود.

---

# Code Block

دستورهای داخل `{}` یک **Block** را تشکیل می‌دهند.

```javascript
if (age >= 18) {
  console.log('Access granted.');
  console.log('Continue.');
}
```

هر دو دستور تنها زمانی اجرا می‌شوند که Condition برقرار باشد.

این مفهوم با **Block Scope** که در فصل `let`, `const and var` به‌صورت مقدماتی معرفی شد نیز ارتباط دارد؛ اما Scope به‌عنوان یک موضوع مستقل در فصل‌های بعدی به‌صورت کامل بررسی خواهد شد.

---

# چند Condition

یک برنامه می‌تواند چند تصمیم مستقل داشته باشد.

```javascript
const stock = 10;
const price = 120;

if (stock > 0) {
  console.log('Available.');
}

if (price < 200) {
  console.log('Affordable.');
}
```

در این مثال هر `if` یک تصمیم مستقل است.

---

# تحلیل مهندسی

`if` فقط یک Syntax نیست.

این ساختار به برنامه اجازه می‌دهد **Control Flow** خود را بر اساس وضعیت داده‌ها تغییر دهد.

بنابراین:

```text
Data
↓
Condition
↓
Decision
↓
Selected Path
```

یکی از پایه‌ای‌ترین الگوهای برنامه‌نویسی است.

---

# اشتباهات رایج

### اشتباه اول: تصور اینکه Condition باید حتماً یک Boolean Literal باشد

این کد کاملاً معتبر است:

```javascript
const age = 20;

if (age >= 18) {
  console.log('Adult.');
}
```

Condition یک Expression است که نتیجه آن در Boolean Context ارزیابی می‌شود.

---

### اشتباه دوم: پیچیده کردن Condition بدون نیاز

اگر منطق شرط ساده است، آن را ساده نگه دارید.

```javascript
if (age >= 18) {
  // ...
}
```

معمولاً خواناتر از ساختن Expressionهای غیرضروری است.

---

# نکات مهم

- `if` برای اجرای شرطی یک Block استفاده می‌شود.
- Condition مشخص می‌کند مسیر اجرای برنامه چگونه انتخاب شود.
- Comparison Operatorها معمولاً Boolean Result تولید می‌کنند.
- `Boolean` پلی میان نتیجه Expression و Decision Making ایجاد می‌کند.
- Decision Making ادامه منطقی مفاهیم فصل‌های قبلی است.

---

# Block 02 — Multiple Conditions

## else

گاهی برنامه باید برای هر دو حالت `true` و `false` رفتار مشخصی داشته باشد.

برای این کار از `else` استفاده می‌کنیم.

```javascript
const age = 16;

if (age >= 18) {
  console.log('Access granted.');
} else {
  console.log('Access denied.');
}
```

اگر Condition برابر `true` باشد، Block اول اجرا می‌شود.

اگر `false` باشد، Block مربوط به `else` اجرا خواهد شد.

---

# else if

گاهی تنها دو حالت نداریم.

برای مثال، وضعیت سفارش می‌تواند چند حالت مختلف داشته باشد:

```javascript
const status = 'shipped';

if (status === 'pending') {
  console.log('Order is pending.');
} else if (status === 'shipped') {
  console.log('Order is on the way.');
} else if (status === 'delivered') {
  console.log('Order delivered.');
} else {
  console.log('Unknown status.');
}
```

در این ساختار، Conditionها به ترتیب بررسی می‌شوند و اولین شرط برقرار، مسیر اجرای مربوط به خود را انتخاب می‌کند.

---

# Conditional Chain

ساختار زیر یک **Conditional Chain** ایجاد می‌کند:

```text
if
↓
else if
↓
else if
↓
else
```

این ساختار زمانی مناسب است که چند حالت مختلف وجود داشته باشد و هر حالت رفتار مشخصی داشته باشد.

---

# Nested Conditions

گاهی یک تصمیم تنها در صورتی معنا دارد که تصمیم دیگری قبلاً برقرار شده باشد.

در این شرایط می‌توان از Nested Condition استفاده کرد.

```javascript
const isLoggedIn = true;
const role = 'admin';

if (isLoggedIn) {
  if (role === 'admin') {
    console.log('Admin dashboard.');
  }
}
```

در اینجا شرط دوم تنها زمانی بررسی می‌شود که شرط اول برقرار باشد.

---

# آیا Nested if همیشه انتخاب خوبی است؟

خیر.

Nested Condition در صورت افزایش عمق می‌تواند خوانایی کد را کاهش دهد.

مثال:

```javascript
if (conditionA) {
  if (conditionB) {
    if (conditionC) {
      // ...
    }
  }
}
```

در چنین شرایطی بهتر است منطق شرط‌ها ساده‌تر یا به Conditionهای معنادار تقسیم شود.

هدف فقط درست بودن کد نیست.

**خوانایی و Maintainability نیز بخشی از طراحی حرفه‌ای هستند.**

---

# Truthy و Falsy

تا اینجا Conditionها را بیشتر با Boolean Expressionها نوشتیم:

```javascript
if (age >= 18) {
  // ...
}
```

اما JavaScript تنها `true` و `false` را در Conditionها قبول نمی‌کند.

Valueهای دیگر نیز می‌توانند در **Boolean Context** ارزیابی شوند.

برای مثال:

```javascript
const userName = 'Omid';

if (userName) {
  console.log('User name exists.');
}
```

رشته `'Omid'` یک Boolean Value نیست، اما در این Context به‌عنوان **Truthy** ارزیابی می‌شود.

---

# Truthy چیست؟

یک Value زمانی **Truthy** است که هنگام ارزیابی در Boolean Context، نتیجه آن `true` باشد.

برای مثال:

```javascript
Boolean('Omid');
```

نتیجه:

```text
true
```

بنابراین:

```javascript
if ('Omid') {
  console.log('Executed.');
}
```

اجرا می‌شود.

---

# Falsy چیست؟

یک Value زمانی **Falsy** است که در Boolean Context به `false` تبدیل شود.

مهم‌ترین Falsy Valueها عبارت‌اند از:

```text
false
0
-0
0n
""
null
undefined
NaN
```

برای مثال:

```javascript
Boolean('');
```

نتیجه:

```text
false
```

بنابراین:

```javascript
if ('') {
  console.log('Executed.');
}
```

اجرا نمی‌شود.

---

# Boolean Conversion

در فصل Data Types با Boolean آشنا شدیم.

در این فصل باید یک قدم جلوتر برویم:

JavaScript در Boolean Context می‌تواند Valueهای دیگر را به‌صورت منطقی ارزیابی کند.

برای مشاهده این تبدیل می‌توانیم از `Boolean()` استفاده کنیم:

```javascript
Boolean('Hello');
```

نتیجه:

```text
true
```

و:

```javascript
Boolean(0);
```

نتیجه:

```text
false
```

نکته مهم این است که `Boolean()` یک Value جدید از نوع Boolean تولید می‌کند، اما قرار گرفتن یک Value در Condition نیز باعث ارزیابی Boolean آن می‌شود.

---

# یک نکته بسیار مهم درباره Object و Array

خالی بودن Object یا Array به معنای Falsy بودن آن نیست.

```javascript
Boolean([]);
```

نتیجه:

```text
true
```

و:

```javascript
Boolean({});
```

نیز:

```text
true
```

بنابراین:

```javascript
if ([]) {
  console.log('Executed.');
}
```

اجرا می‌شود.

این موضوع یکی از خطاهای رایج در درک Truthy و Falsy است.

---

# String نیز رفتار مشخصی دارد

رشته خالی:

```javascript
''
```

Falsy است.

اما یک رشته غیرخالی:

```javascript
'JavaScript'
```

Truthy است.

```javascript
Boolean('');
// false

Boolean('JavaScript');
// true
```

---

# Truthy بودن به معنی Boolean بودن نیست

این دو مفهوم را نباید یکی بدانیم.

```javascript
const value = 'Hello';

typeof value;
```

نتیجه:

```text
string
```

اما:

```javascript
Boolean(value);
```

نتیجه:

```text
true
```

بنابراین:

```text
Truthy
```

یک رفتار در Boolean Context است، نه یک Data Type جدید.

---

# Jonas Perspective

یکی از نکات مهم در آموزش Decision Making این است که Truthy و Falsy را نباید صرفاً به‌صورت فهرستی از Valueها حفظ کرد.

بهتر است ابتدا مدل ذهنی زیر را درک کنیم:

```text
Value
↓
Boolean Context
↓
true / false
↓
Decision
```

این مدل ذهنی در ادامه کتاب، هنگام کار با Logical Operators و Conditional Patterns نیز اهمیت خواهد داشت.

---

# اشتباهات رایج

### هر Value غیر Boolean برابر false نیست

```javascript
if ('Hello') {
  console.log('Executed.');
}
```

اجرا می‌شود.

---

### Array خالی Falsy نیست

```javascript
Boolean([]);
// true
```

---

### Object خالی Falsy نیست

```javascript
Boolean({});
// true
```

---

### Truthy یک Type نیست

```javascript
typeof 'Hello';
// "string"
```

Truthy بودن تنها نتیجه ارزیابی Value در Boolean Context است.

---

# نکات مهم

- `if` و سایر ساختارهای شرطی از Boolean Context استفاده می‌کنند.
- Truthy یعنی Value در Boolean Context به `true` ارزیابی می‌شود.
- Falsy یعنی Value در Boolean Context به `false` ارزیابی می‌شود.
- `Boolean()` برای مشاهده تبدیل صریح به Boolean مفید است.
- `[]` و `{}` با وجود خالی بودن، Truthy هستند.
- Truthy و Falsy نوع داده جدیدی نیستند.

---

# Block 03 — Equality and switch

## Equality در Decision Making

در فصل Operators با Comparison و Equality آشنا شدیم.

اکنون می‌توانیم همان مفاهیم را در یک Decision واقعی به کار ببریم.

برای مثال:

```javascript
const score = 75;

if (score >= 60) {
  console.log('Passed.');
}
```

در اینجا Comparison Operator نتیجه‌ای از نوع Boolean تولید می‌کند و `if` از آن برای انتخاب مسیر اجرا استفاده می‌کند.

بنابراین:

```text
Comparison
↓
Boolean Result
↓
Condition
↓
Decision
```

---

# Strict Equality

عملگر:

```javascript
===
```

برای مقایسه Strict استفاده می‌شود.

برای مثال:

```javascript
5 === 5;
```

نتیجه:

```text
true
```

اما:

```javascript
5 === '5';
```

نتیجه:

```text
false
```

زیرا Type دو Value متفاوت است.

در Decision Making معمولاً این رفتار قابل پیش‌بینی‌تر است.

```javascript
const role = 'admin';

if (role === 'admin') {
  console.log('Admin access.');
}
```

---

# Strict Inequality

عملگر:

```javascript
!==
```

بررسی می‌کند که دو Value از نظر مقدار یا Type یکسان نباشند.

```javascript
5 !== '5';
```

نتیجه:

```text
true
```

---

# Loose Equality

عملگر:

```javascript
==
```

**Loose Equality** نام دارد.

این Operator در برخی مقایسه‌ها از **Type Coercion** استفاده می‌کند.

برای مثال:

```javascript
5 == '5';
```

نتیجه:

```text
true
```

در حالی که:

```javascript
5 === '5';
```

نتیجه:

```text
false
```

جزئیات کامل Type Coercion و قواعد تبدیل در فصل **Type Conversion and Coercion** بررسی خواهد شد.

در این فصل تنها باید بدانیم که این تفاوت می‌تواند مستقیماً روی نتیجه یک Decision اثر بگذارد.

---

# چرا معمولاً === توصیه می‌شود؟

در بسیاری از کدهای مدرن، استفاده از:

```javascript
===
```

و:

```javascript
!==
```

ترجیح داده می‌شود؛ زیرا Intent مقایسه را روشن‌تر و رفتار را قابل پیش‌بینی‌تر می‌کند.

مثال:

```javascript
if (userRole === 'admin') {
  // ...
}
```

از نظر خوانایی مشخص می‌کند که مقایسه دقیق میان Valueها انجام می‌شود.

---

# switch

وقتی یک Expression را با چند Value مشخص مقایسه می‌کنیم، `switch` می‌تواند ساختار خواناتری نسبت به چند `else if` ایجاد کند.

Syntax پایه:

```javascript
switch (expression) {
  case value1:
    // ...
    break;

  case value2:
    // ...
    break;

  default:
    // ...
}
```

---

# مثال

```javascript
const role = 'admin';

switch (role) {
  case 'admin':
    console.log('Admin dashboard.');
    break;

  case 'editor':
    console.log('Editor dashboard.');
    break;

  case 'user':
    console.log('User dashboard.');
    break;

  default:
    console.log('Unknown role.');
}
```

در اینجا مقدار `role` با Caseهای مختلف مقایسه می‌شود.

---

# case

هر `case` یک حالت مشخص را تعریف می‌کند.

```javascript
case 'admin':
```

یعنی اگر مقدار Expression مربوط به `switch` با این Case مطابقت داشته باشد، دستورات آن بخش اجرا می‌شوند.

---

# break

`break` اجرای `switch` را پس از Case مربوطه متوقف می‌کند.

```javascript
case 'admin':
  console.log('Admin.');
  break;
```

اگر `break` حذف شود، ممکن است اجرای برنامه وارد Case بعدی شود.

این رفتار **Fall-through** نام دارد.

---

# default

`default` زمانی اجرا می‌شود که هیچ Caseای Match نشده باشد.

```javascript
switch (role) {
  case 'admin':
    console.log('Admin.');
    break;

  default:
    console.log('Unknown role.');
}
```

---

# انتخاب بین if و switch

به‌صورت کلی:

```text
if / else if
```

برای Conditionهای منطقی و متنوع مناسب است.

برای مثال:

```javascript
if (score >= 90) {
  console.log('A');
} else if (score >= 60) {
  console.log('B');
} else {
  console.log('C');
}
```

اما:

```text
switch
```

زمانی می‌تواند خواناتر باشد که یک Expression را با چند Value مشخص مقایسه کنیم.

```javascript
switch (status) {
  case 'pending':
    // ...
    break;

  case 'shipped':
    // ...
    break;

  case 'delivered':
    // ...
    break;
}
```

معیار اصلی انتخاب، کوتاه‌تر بودن نیست؛ **تناسب ساختار با مسئله و خوانایی کد** است.

---

# نکات مهم

- `===` مقایسه Strict انجام می‌دهد.
- `!==` برای Strict Inequality استفاده می‌شود.
- `==` می‌تواند Type Coercion را وارد مقایسه کند.
- جزئیات Type Coercion در فصل بعدی بررسی خواهد شد.
- `switch` برای مقایسه یک Expression با چند Case مشخص مناسب است.
- `break` از Fall-through ناخواسته جلوگیری می‌کند.
- `default` مسیر پیش‌فرض زمانی است که هیچ Caseای Match نشده باشد.

---

# Block 04 — Practical Patterns

## Conditional Design

نوشتن Condition فقط به درست بودن Syntax محدود نمی‌شود.

یک Condition حرفه‌ای باید:

- قابل فهم باشد.
- Intent را منتقل کند.
- تا حد امکان ساده باشد.
- در صورت رشد پروژه، قابل نگهداری باقی بماند.

برای مثال، به‌جای:

```javascript
if (
  isAdmin &&
  isActive &&
  !isBlocked &&
  hasPermission
) {
  console.log('Access granted.');
}
```

در صورت نیاز می‌توان بخشی از منطق را با یک نام معنادار بیان کرد:

```javascript
const canAccess = isAdmin && isActive && !isBlocked && hasPermission;

if (canAccess) {
  console.log('Access granted.');
}
```

در این حالت، نام `canAccess` بخشی از منطق را برای خواننده توضیح می‌دهد.

---

# Guard Clause — Introduction

**Guard Clause** یک الگوی ساده برای خارج کردن سریع برنامه از یک مسیر نامعتبر یا غیرقابل ادامه است.

در سطح فعلی کتاب، فقط مدل ذهنی آن را معرفی می‌کنیم.

به‌صورت مفهومی:

```text
Invalid Condition
↓
Stop Current Path
```

برای مثال، اگر در یک فرآیند شرط اولیه برقرار نباشد، بهتر است به‌جای افزایش عمق Nested Conditionها، همان مسیر را زودتر متوقف کنیم.

پیاده‌سازی کامل Guard Clause معمولاً در Functionها معنا پیدا می‌کند و در فصل‌های مربوط به Functions با `return` کاربرد آن را بهتر خواهیم دید.

بنابراین در این فصل هدف، شناخت **Conditional Design Pattern** است، نه آموزش Function یا `return`.

---

# Conditional Operator

در فصل Operators با **Conditional Operator** یا **Ternary Operator** آشنا شدیم.

Syntax آن:

```javascript
condition ? valueIfTrue : valueIfFalse
```

است.

اکنون می‌توانیم آن را در Decision Making به‌صورت کاربردی‌تر ببینیم.

مثال:

```javascript
const age = 20;

const message =
  age >= 18 ? 'Access granted.' : 'Access denied.';
```

در اینجا یک Decision ساده برای تولید یک Value انجام شده است.

---

# چه زمانی از Ternary استفاده کنیم؟

Ternary برای Conditionهای ساده مناسب است.

مثلاً:

```javascript
const label = isLoggedIn ? 'Logout' : 'Login';
```

اما اگر منطق پیچیده شود، استفاده از `if / else` معمولاً خواناتر است.

مثال نامناسب:

```javascript
const result =
  conditionA
    ? conditionB
      ? 'A'
      : 'B'
    : conditionC
      ? 'C'
      : 'D';
```

در چنین شرایطی، کوتاه‌تر بودن کد به معنای بهتر بودن آن نیست.

---

# ارتباط با فصل پنجم

Conditional Operator در فصل پنجم به‌عنوان یکی از Operatorها معرفی شد.

اکنون کاربرد آن را در یک مسئله واقعی‌تر می‌بینیم.

این یک نمونه مهم از **Concept Flow** کتاب است:

```text
Operator
↓
Expression
↓
Boolean Condition
↓
Decision
↓
Application Logic
```

بنابراین فصل حاضر قرار نیست Ternary Operator را دوباره از ابتدا آموزش دهد؛ بلکه جایگاه آن را در Decision Making روشن می‌کند.

---

# یک مثال ترکیبی

```javascript
const age = 25;
const role = 'admin';
const isActive = true;

const isAdult = age >= 18;
const isAuthorized = role === 'admin' && isActive;

if (isAdult && isAuthorized) {
  console.log(`Access granted for ${role}.`);
} else {
  console.log('Access denied.');
}
```

در این مثال مفاهیم فصل‌های قبل با یکدیگر ترکیب شده‌اند:

```text
Variables
↓
Values
↓
Number / String / Boolean
↓
Comparison Operators
↓
Logical Operators
↓
Boolean Expressions
↓
Decision Making
↓
Template Literal
```

این همان نقطه‌ای است که دانش Syntax به **Engineering Thinking** تبدیل می‌شود.

---

# Common Mistakes

## استفاده بی‌دلیل از `==`

به‌جای:

```javascript
if (age == 18) {
  // ...
}
```

در بیشتر شرایط بهتر است از:

```javascript
if (age === 18) {
  // ...
}
```

استفاده شود.

---

## Conditionهای بیش از حد پیچیده

اگر یک Condition به‌سرعت قابل خواندن نیست، آن را به بخش‌های معنادار تقسیم کنید.

```javascript
const hasRole = isAdmin || isOwner;
const canAccess = hasRole && isActive && !isBlocked;

if (canAccess) {
  // ...
}
```

هدف این نیست که همیشه Condition را کوتاه کنیم.

هدف این است که **منطق آن برای انسان قابل فهم باشد.**

---

## استفاده بیش از حد از Nested Conditions

Nested Conditionهای عمیق معمولاً خوانایی را کاهش می‌دهند.

اگر امکان ساده‌سازی یا استفاده از الگوهای مناسب وجود دارد، از افزایش غیرضروری عمق جلوگیری کنید.

---

## استفاده بیش از حد از Ternary

Ternary برای Decisionهای ساده مناسب است.

برای منطق پیچیده، `if / else` معمولاً انتخاب بهتری است.

---

## فراموش کردن `break` در switch

```javascript
switch (role) {
  case 'admin':
    console.log('Admin.');

  case 'user':
    console.log('User.');
}
```

در این حالت ممکن است Fall-through رخ دهد.

اگر این رفتار عمدی نیست، `break` را فراموش نکنید.

---

# Jonas Perspective

یکی از نکات مهم در سبک آموزش Jonas این است که ساختارهای شرطی باید در خدمت **خوانایی منطق برنامه** باشند.

هدف استفاده از `if`، `switch` یا Ternary صرفاً نوشتن کد کوتاه‌تر نیست.

انتخاب ساختار مناسب باید بر اساس مسئله انجام شود.

به‌طور عملی:

- برای یک Condition ساده، `if` مناسب است.
- برای چند مسیر منطقی، `if / else if / else` می‌تواند مناسب باشد.
- برای مقایسه یک Expression با چند Value مشخص، `switch` می‌تواند خواناتر باشد.
- برای انتخاب ساده بین دو Value، Ternary مناسب است.
- برای جلوگیری از Nested Conditionهای غیرضروری، Guard Clause می‌تواند یک الگوی مفید باشد.

---

# Decision Making در یک برنامه واقعی

در یک برنامه واقعی، معمولاً چند مفهوم با هم ترکیب می‌شوند.

```javascript
const age = 25;
const role = 'admin';
const isActive = true;

if (age >= 18 && isActive) {
  if (role === 'admin') {
    console.log('Admin access.');
  } else {
    console.log('User access.');
  }
} else {
  console.log('Access denied.');
}
```

در این مثال:

```text
Variables
↓
Values
↓
Comparison Operators
↓
Boolean Expressions
↓
Logical Operators
↓
Condition
↓
if / else
↓
Control Flow
```

این همان نقطه‌ای است که مفاهیم پایه فصل‌های قبل در یک برنامه واقعی به یکدیگر متصل می‌شوند.

---

# Best Practices

برای نوشتن Conditional Logic حرفه‌ای:

- Condition را تا حد امکان واضح نگه دارید.
- از نام‌های معنادار برای Conditionهای پیچیده استفاده کنید.
- در مقایسه‌های معمول، `===` و `!==` را ترجیح دهید.
- از Nested Conditionهای عمیق اجتناب کنید.
- Ternary را برای Decisionهای ساده نگه دارید.
- `switch` را زمانی استفاده کنید که ساختار آن واقعاً خواناتر است.
- منطق Type Coercion را در Conditionها بدون دلیل وارد نکنید.
- کوتاه بودن کد را با خوانایی اشتباه نگیرید.

---

# Summary

در این فصل دیدیم که برنامه‌ها برای واکنش به وضعیت‌های مختلف به **Decision Making** نیاز دارند.

ابتدا رابطه میان `Boolean`، `Condition` و `if` را بررسی کردیم و دیدیم که Comparison Expressionها چگونه نتیجه‌ای تولید می‌کنند که می‌تواند مسیر اجرای برنامه را تعیین کند.

سپس با `else`، `else if` و Nested Conditions آشنا شدیم و دیدیم چگونه می‌توان برای چند وضعیت مختلف مسیرهای متفاوتی ایجاد کرد.

در ادامه مفهوم **Truthy** و **Falsy** را بررسی کردیم و آموختیم که JavaScript در Boolean Context فقط به Valueهای واقعی `true` و `false` محدود نیست.

سپس Equality را در Decision Making بررسی کردیم و تفاوت `===` و `==` را شناختیم. همچنین دیدیم که Type Coercion می‌تواند روی نتیجه Loose Equality اثر بگذارد و جزئیات آن در فصل **Type Conversion and Coercion** بررسی خواهد شد.

در ادامه `switch` را شناختیم و یاد گرفتیم چه زمانی می‌تواند نسبت به Conditional Chain خواناتر باشد.

در پایان نیز با Conditional Design، Guard Clause در سطح مقدماتی و Conditional Operator آشنا شدیم و تأکید کردیم که هدف اصلی، انتخاب ساختار شرطی مناسب و خوانا است.

---

# Key Takeaways

- Decision Making مسیر اجرای برنامه را بر اساس وضعیت داده‌ها کنترل می‌کند.
- `Boolean` یکی از پایه‌های اصلی Conditional Logic است.
- Condition می‌تواند یک Boolean Expression یا Valueای باشد که در Boolean Context ارزیابی می‌شود.
- `if` برای اجرای شرطی یک Block استفاده می‌شود.
- `else if` و `else` امکان ایجاد مسیرهای جایگزین را فراهم می‌کنند.
- Nested Conditions باید با دقت استفاده شوند تا پیچیدگی افزایش پیدا نکند.
- Truthy و Falsy رفتار Valueها در Boolean Context را توصیف می‌کنند و Type جدیدی نیستند.
- `[]` و `{}` با وجود خالی بودن، Truthy هستند.
- `===` و `!==` معمولاً انتخاب‌های قابل پیش‌بینی‌تری برای مقایسه هستند.
- `==` می‌تواند Type Coercion را وارد Equality کند.
- `switch` برای مقایسه یک Expression با چند Value مشخص مناسب است.
- `break` از Fall-through ناخواسته در `switch` جلوگیری می‌کند.
- Ternary برای Decisionهای ساده مناسب است.
- Guard Clause در این فصل فقط به‌عنوان یک Conditional Pattern معرفی شد.
- خوانایی و Maintainability باید در انتخاب ساختار شرطی در نظر گرفته شوند.

---

# Technical Interview

## سطح پایه (Junior)

### سؤال ۱

Decision Making در JavaScript چیست؟

### سؤال ۲

Condition چیست؟

### سؤال ۳

تفاوت `if` و `else` چیست؟

### سؤال ۴

`else if` چه زمانی استفاده می‌شود؟

### سؤال ۵

Truthy و Falsy چیستند؟

### سؤال ۶

آیا `[]` و `{}` در JavaScript Falsy هستند؟

### سؤال ۷

تفاوت `===` و `==` چیست؟

### سؤال ۸

`switch` چه کاربردی دارد؟

---

## سطح متوسط (Mid-Level)

### سؤال ۹

Boolean Context چیست؟

### سؤال ۱۰

چرا Truthy بودن یک Value به معنی Boolean بودن آن Value نیست؟

### سؤال ۱۱

چرا در Conditionهای معمول استفاده از `===` ترجیح داده می‌شود؟

### سؤال ۱۲

اگر `break` را در `switch` فراموش کنیم چه اتفاقی ممکن است رخ دهد؟

### سؤال ۱۳

چه زمانی `switch` نسبت به `if / else if` انتخاب خواناتری است؟

### سؤال ۱۴

چرا Nested Conditions زیاد می‌توانند Maintainability را کاهش دهند؟

### سؤال ۱۵

Conditional Operator چه زمانی انتخاب مناسبی است؟

---

## سطح پیشرفته (Senior)

### سؤال ۱۶

رابطه میان Comparison Operator، Boolean Expression و Decision Making چیست؟

### سؤال ۱۷

چرا Truthy/Falsy را بهتر است به‌عنوان Boolean Context درک کنیم و نه یک فهرست حفظی؟

### سؤال ۱۸

چرا کوتاه‌تر بودن Conditional Logic الزاماً به معنای بهتر بودن آن نیست؟

### سؤال ۱۹

Guard Clause چه مشکلی را در طراحی Conditional Logic هدف قرار می‌دهد؟

### سؤال ۲۰

چگونه می‌توان یک Condition پیچیده را بدون تغییر رفتار برنامه خواناتر کرد؟

---

# Golden Answers

## Decision Making چیست؟

Decision Making فرآیندی است که در آن برنامه بر اساس نتیجه یک Condition، مسیر مناسب اجرای کد را انتخاب می‌کند. ساختارهایی مانند `if`، `else` و `switch` ابزارهای اصلی این کار هستند.

---

## Condition چیست؟

Condition عبارتی است که نتیجه آن برای انتخاب مسیر اجرای برنامه استفاده می‌شود. این نتیجه معمولاً یک Boolean است یا در یک Boolean Context به `true` یا `false` ارزیابی می‌شود.

---

## Truthy و Falsy چیستند؟

Truthy و Falsy رفتار یک Value را هنگام ارزیابی در Boolean Context توصیف می‌کنند. Valueهای Truthy به `true` و Valueهای Falsy به `false` ارزیابی می‌شوند.

---

## چرا `===` معمولاً ترجیح داده می‌شود؟

زیرا مقایسه Strict انجام می‌دهد و Type Coercion ضمنی را وارد مقایسه نمی‌کند. در نتیجه Intent و رفتار مقایسه معمولاً قابل پیش‌بینی‌تر است.

---

## تفاوت `switch` و `if / else if` چیست؟

`if / else if` برای Conditionهای منطقی و متنوع انعطاف بیشتری دارد. `switch` زمانی می‌تواند خواناتر باشد که یک Expression را با چند Value مشخص مقایسه کنیم.

---

## Guard Clause چیست؟

Guard Clause یک الگوی طراحی است که با بررسی یک Condition نامعتبر یا غیرقابل ادامه، مسیر فعلی را زودتر متوقف می‌کند و از افزایش غیرضروری Nested Conditions جلوگیری می‌کند. کاربرد کامل آن معمولاً در Functionها و همراه با `return` معنا پیدا می‌کند.

---

## Conditional Operator چه زمانی مناسب است؟

برای Decisionهای ساده که باید یکی از دو Value انتخاب شود، Ternary معمولاً مناسب است. وقتی منطق شرط پیچیده شود، `if / else` معمولاً خواناتر خواهد بود.

---

## پاسخ کوتاه طلایی مصاحبه

**سؤال**

رابطه میان Comparison، Boolean و Decision Making چیست؟

**پاسخ**

Comparison Operator یک Expression تولید می‌کند که معمولاً نتیجه‌ای از نوع Boolean دارد. این نتیجه می‌تواند به‌عنوان Condition استفاده شود و Condition مسیر اجرای برنامه را در ساختارهایی مانند `if` یا `switch` تعیین می‌کند.

---

# اشتباهات رایج

❌ تصور اینکه فقط `true` و `false` می‌توانند در `if` استفاده شوند.

✔ Valueهای دیگر نیز در Boolean Context به Truthy یا Falsy ارزیابی می‌شوند.

---

❌ تصور اینکه `[]` و `{}` چون خالی هستند Falsy هستند.

✔ هر دو Truthy هستند.

---

❌ استفاده از `==` بدون درک Type Coercion.

✔ در مقایسه‌های معمول، `===` و `!==` انتخاب‌های قابل پیش‌بینی‌تری هستند.

---

❌ استفاده از Ternaryهای تو در تو برای منطق پیچیده.

✔ Ternary را برای Decisionهای ساده نگه دارید.

---

❌ افزایش عمق Nested Conditions برای هر وضعیت جدید.

✔ منطق را ساده کنید و در صورت نیاز از Conditionهای معنادار یا الگوهای مناسب استفاده کنید.

---

# Conclusion

Decision Making یکی از اولین نقاطی است که در آن JavaScript از مجموعه‌ای از Syntaxها به ابزاری برای **کنترل رفتار برنامه** تبدیل می‌شود.

در این فصل دیدیم که این قابلیت به‌تنهایی عمل نمی‌کند.

```text
Values
↓
Variables
↓
Types
↓
Operators
↓
Boolean Expressions
↓
Conditions
↓
Decision Making
```

این جریان مفهومی، ارتباط فصل هفتم با شش فصل قبل را نشان می‌دهد.

در فصل بعد، همین ایده یک قدم جلوتر خواهد رفت.

در آنجا به‌جای انتخاب میان چند مسیر، با مسئله **تکرار اجرای یک مسیر** روبه‌رو خواهیم شد و بررسی می‌کنیم JavaScript چگونه با استفاده از **Loops** اجرای تکراری را مدیریت می‌کند.
