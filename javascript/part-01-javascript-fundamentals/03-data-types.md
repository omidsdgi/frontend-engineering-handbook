# Chapter 03

# Data Types

---

# Chapter Goal

پس از مطالعه این فصل، انتظار می‌رود بتوانید:

- مفهوم Data Type را توضیح دهید.
- رابطه میان Value و Type را در JavaScript درک کنید.
- دلیل وجود انواع داده را توضیح دهید.
- مفهوم Dynamic Typing را به‌درستی توضیح دهید.
- انواع داده‌های Primitive را نام ببرید.
- تفاوت Primitive و Object را در سطح مفهومی درک کنید.
- رفتار کلی Valueهای Primitive و Object را از یکدیگر تفکیک کنید.
- با عملگر `typeof` برای بررسی نوع Valueها کار کنید.
- محدودیت‌های `typeof` را بشناسید.
- تفاوت `null` و `undefined` را توضیح دهید.
- به پرسش‌های فنی مرتبط با Data Types پاسخ دهید.

---

# Core Question

> **JavaScript چگونه انواع مختلف Value را مدیریت می‌کند؟**

---

# Concept Flow

```text
Value
↓
Type
↓
Dynamic Typing
↓
Primitive Types
↓
Object
↓
Primitive vs Object
↓
typeof
↓
Type Checking
```

---

# مقدمه

در فصل قبل دیدیم که برنامه‌ها با **Value**ها کار می‌کنند.

یک Value می‌تواند یک عدد، متن، وضعیت منطقی یا ساختاری پیچیده‌تر باشد.

اما JavaScript باید بداند هر Value چه ماهیتی دارد؛ زیرا نوع Value روی نحوه پردازش و رفتار آن تأثیر می‌گذارد.

برای مثال:

```javascript
10 + 20
```

یک عملیات عددی است.

اما:

```javascript
"Hello" + " World"
```

به اتصال دو String منجر می‌شود.

پس برای درک رفتار JavaScript باید از یک سؤال بنیادی شروع کنیم:

> یک Value چه نوعی دارد و JavaScript چگونه این Type را مدیریت می‌کند؟

در این فصل ابتدا مفهوم Type را بررسی می‌کنیم، سپس Dynamic Typing و انواع Primitive را می‌شناسیم. بعد از آن به Object می‌رسیم و تفاوت مفهومی Primitive و Object را بررسی می‌کنیم. در پایان نیز با `typeof` و نقش آن در Type Checking آشنا خواهیم شد.

---

# Block 01 — Types

# Data Type چیست؟

**Data Type** مشخص می‌کند یک Value از چه نوعی است و چه رفتارهایی می‌توان روی آن انجام داد.

برای مثال:

```javascript
25
```

یک Value از نوع `Number` است.

```javascript
"Hello"
```

یک Value از نوع `String` است.

و:

```javascript
true
```

یک Value از نوع `Boolean` است.

بنابراین می‌توانیم رابطه ساده زیر را در نظر بگیریم:

```text
Value
↓
Type
```

هر Value دارای یک Type است و JavaScript بر اساس این Type رفتار مناسب را تعیین می‌کند.

---

# چرا Type مهم است؟

نوع یک Value فقط یک برچسب نیست.

Type بخشی از معنای آن Value و رفتارهایی است که زبان برای آن فراهم می‌کند.

برای مثال:

```javascript
10 + 20
```

نتیجه:

```text
30
```

اما:

```javascript
"10" + "20"
```

نتیجه:

```text
"1020"
```

در این دو مثال، Syntax مشابه است، اما Type Valueها متفاوت است و همین تفاوت روی رفتار Expression اثر می‌گذارد.

در فصل **Type Conversion and Coercion** قواعد تبدیل Typeها و رفتارهای پیچیده‌تر این عملیات را به‌صورت کامل بررسی خواهیم کرد.

---

# تعریف ساده

Type مشخص می‌کند یک Value چه نوع داده‌ای است.

برای مثال:

```text
42        → Number
"Hello"   → String
true      → Boolean
```

---

# تعریف فنی

در JavaScript، Type بخشی از مدل معنایی زبان است که مشخص می‌کند یک Value چه نوعی دارد و عملیات زبان چگونه باید با آن Value رفتار کنند.

JavaScript دارای Typeهای مشخص است، اما برخلاف زبان‌های Statically Typed، Type متغیرها را هنگام نوشتن کد به‌صورت ثابت تعیین نمی‌کند.

این موضوع ما را به مفهوم **Dynamic Typing** می‌رساند.

---

# Dynamic Typing

JavaScript یک زبان **Dynamically Typed** است.

این عبارت به این معنا نیست که JavaScript فاقد Type است.

برعکس، JavaScript Typeهای مشخصی دارد؛ اما Type به **Value** مربوط است و محدودیت Type متغیرها مانند زبان‌های Statically Typed در زمان نوشتن کد اعمال نمی‌شود.

برای مثال:

```javascript
let value = 20;
```

در این لحظه:

```text
value → 20 (Number)
```

بعد:

```javascript
value = "Hello";
```

اکنون:

```text
value → "Hello" (String)
```

Identifier و Variable همان نقش قبلی را دارند، اما Value جدید Type متفاوتی دارد.

بنابراین مدل ذهنی دقیق‌تر این است:

```text
Variable / Binding
        ↓
      Value
        ↓
       Type
```

نه اینکه تصور کنیم Variable خودش یک Type ثابت دارد و سپس Type آن تغییر می‌کند.

---

# چه زمانی Type مشخص می‌شود؟

Type Value در زمان اجرای برنامه قابل تعیین است.

برای مثال:

```javascript
let score;

console.log(typeof score);
```

خروجی:

```text
undefined
```

بعد:

```javascript
score = 100;
```

اکنون Value موجود در Binding از نوع `Number` است.

بنابراین Dynamic Typing به این معناست که Type سیستم در Runtime با Valueهای واقعی برنامه سروکار دارد.

---

# Dynamic Typing چه مزیتی دارد؟

یکی از مزایای مهم Dynamic Typing، انعطاف‌پذیری بیشتر در نوشتن کد است.

برای مثال:

```javascript
function printValue(value) {
  console.log(value);
}

printValue("Hello");
printValue(100);
printValue(true);
```

لازم نیست هنگام تعریف Function مشخص کنیم که `value` فقط یک Type خاص را دریافت می‌کند.

این انعطاف‌پذیری می‌تواند سرعت توسعه را افزایش دهد.

---

# Dynamic Typing چه هزینه‌ای دارد؟

انعطاف بیشتر می‌تواند به معنای افزایش احتمال برخی خطاهای Runtime نیز باشد.

برای مثال:

```javascript
function add(a, b) {
  return a + b;
}

add(10, "20");
```

نتیجه:

```text
"1020"
```

خواهد بود.

JavaScript از اجرای این Expression جلوگیری نمی‌کند؛ بلکه بر اساس قواعد زبان آن را ارزیابی می‌کند.

جزئیات Type Conversion و Type Coercion در فصل **Type Conversion and Coercion** بررسی خواهد شد.

---

# یک تفکیک مهم

**Dynamic Typing** و **Dynamic Language** یک مفهوم نیستند.

Dynamic Typing به سیستم Type مربوط است.

Dynamic Language مفهومی گسترده‌تر است و به ویژگی‌هایی از زبان اشاره می‌کند که در Runtime انعطاف‌پذیری بیشتری ایجاد می‌کنند.

در این فصل تمرکز ما روی Dynamic Typing است.

---

# TypeScript چه تغییری ایجاد می‌کند؟

TypeScript یک سیستم Static Type Checking را در مرحله توسعه فراهم می‌کند.

برای مثال:

```typescript
let age: number = 20;
```

TypeScript می‌تواند هنگام توسعه بررسی کند که:

```typescript
age = "Hello";
```

با Type اعلام‌شده سازگار نیست.

اما TypeScript در نهایت به JavaScript تبدیل می‌شود و رفتار Type سیستم JavaScript در Runtime همچنان بر اساس مدل خود JavaScript است.

در این کتاب، جزئیات TypeScript خارج از Scope این فصل است.

---

# پاسخ کوتاه طلایی مصاحبه

**سؤال**

Dynamic Typing در JavaScript چیست؟

**پاسخ**

JavaScript یک زبان Dynamically Typed است؛ یعنی Type به Value مربوط است و Type سیستم در Runtime با Valueهای واقعی برنامه سروکار دارد. در نتیجه یک Variable می‌تواند در طول اجرای برنامه Valueهایی با Typeهای متفاوت داشته باشد.

---

# Block 02 — Primitive Types

# Primitive Data Types

JavaScript دارای هفت نوع داده Primitive است:

| Type | مثال |
|---|---|
| Number | `10` |
| String | `"Hello"` |
| Boolean | `true` |
| Undefined | `undefined` |
| Null | `null` |
| Symbol | `Symbol()` |
| BigInt | `123n` |

Primitiveها Valueهای بنیادی زبان هستند.

در این فصل ابتدا پنج Type رایج را بررسی می‌کنیم و سپس `Symbol` و `BigInt` را به‌صورت کوتاه معرفی خواهیم کرد.

---

# Number

`Number` برای نمایش اعداد استفاده می‌شود.

اعداد صحیح و اعشاری معمولی هر دو از نوع `Number` هستند.

```javascript
25

3.14

-10
```

بنابراین JavaScript برای این Valueها Type جداگانه‌ای مانند `Integer` و `Float` ندارد.

برای مثال:

```javascript
typeof 25;
```

نتیجه:

```text
"number"
```

جزئیات مدل عددی، Floating Point، Precision و محدودیت‌های Number در فصل **Working with Numbers** بررسی خواهد شد.

---

# String

`String` برای نمایش داده‌های متنی استفاده می‌شود.

برای ایجاد String می‌توان از Quoteهای مختلف استفاده کرد:

```javascript
"JavaScript"

'JavaScript'
```

String نیز مانند سایر Valueها دارای Type مشخصی است:

```javascript
typeof "JavaScript";
```

نتیجه:

```text
"string"
```

مباحث مربوط به String، Concatenation و Template Literals در فصل **Strings and Template Literals** به‌صورت کامل بررسی خواهند شد.

---

# Boolean

`Boolean` فقط دو Value دارد:

```javascript
true

false
```

Boolean معمولاً برای نمایش یک وضعیت منطقی استفاده می‌شود.

برای مثال:

```javascript
const isLoggedIn = true;
```

در اینجا:

```text
isLoggedIn → true → Boolean
```

Boolean نقش مهمی در تصمیم‌گیری‌های برنامه دارد که در فصل **Taking Decisions** بررسی خواهد شد.

---

# Undefined

`undefined` یک Primitive Value است.

یکی از حالت‌های رایج آن زمانی است که یک Variable ایجاد شده اما هنوز Valueای به آن Assignment نشده است:

```javascript
let age;
```

در این حالت:

```javascript
console.log(age);
```

نتیجه:

```text
undefined
```

است.

بنابراین `undefined` می‌تواند نشان‌دهنده نبودن یک Value مشخص در یک وضعیت خاص باشد.

---

# Null

`null` نیز یک Primitive Value است.

تفاوت مفهومی مهم آن با `undefined` این است که `null` معمولاً به‌صورت آگاهانه برای نشان دادن نبودن یک Value قرار داده می‌شود.

برای مثال:

```javascript
const selectedUser = null;
```

در اینجا برنامه‌نویس عمداً مشخص کرده است که در حال حاضر User انتخاب‌شده‌ای وجود ندارد.

به‌صورت ساده:

```text
undefined → Value مشخصی تعیین نشده است
null      → عمداً نبودن Value را نشان می‌دهیم
```

این دو Value یکسان نیستند.

---

# تفاوت null و undefined

مقایسه زیر را به‌عنوان یک مدل ذهنی اولیه در نظر بگیرید:

```javascript
let user;

const selectedUser = null;
```

در مورد اول:

```text
user → undefined
```

در مورد دوم:

```text
selectedUser → null
```

هر دو بیانگر نبودن یک Value کاربردی هستند، اما معنای آن‌ها در برنامه یکسان نیست.

جزئیات رفتار آن‌ها در Conversion، Equality و سایر عملیات در فصل‌های بعد بررسی خواهد شد.

---

# Symbol

`Symbol` یک Primitive Type است که برای ایجاد Valueهای Symbolic و یکتا استفاده می‌شود.

برای مثال:

```javascript
const id = Symbol("id");
```

دو Symbol با Description یکسان نیز Value یکسانی نیستند:

```javascript
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second);
```

نتیجه:

```text
false
```

`Symbol` در سناریوهای خاص، مانند ایجاد کلیدهای یکتا، کاربرد دارد.

در این فصل فقط مدل اولیه آن را می‌شناسیم.

---

# BigInt

`BigInt` برای نمایش Integerهای بسیار بزرگ استفاده می‌شود؛ زمانی که محدوده امن `Number` برای یک کاربرد کافی نیست.

برای ایجاد یک BigInt می‌توان از پسوند `n` استفاده کرد:

```javascript
12345678901234567890n
```

در این مثال:

```text
12345678901234567890n → BigInt
```

مباحث مربوط به محدودیت‌های Number و BigInt در فصل **BigInt** به‌صورت کامل بررسی خواهند شد.

---

# نکته مهم

هفت Primitive Type عبارت‌اند از:

```text
Number
String
Boolean
Undefined
Null
Symbol
BigInt
```

درک این فهرست مهم است، اما هدف اصلی این فصل حفظ کردن آن نیست.

هدف این است که بدانیم JavaScript برای Valueهای مختلف Typeهای متفاوتی دارد و این Typeها بخشی از رفتار زبان را تعیین می‌کنند.

---

# Block 03 — Modern Primitive Types

# چرا Symbol و BigInt جداگانه معرفی می‌شوند؟

`Number`، `String` و `Boolean` در برنامه‌های روزمره بسیار رایج‌اند.

در مقابل، `Symbol` و `BigInt` در سناریوهای تخصصی‌تر استفاده می‌شوند.

اما هر دو بخشی از مدل Typeهای Primitive JavaScript هستند و شناخت آن‌ها برای داشتن یک تصویر کامل از Data Types ضروری است.

در ادامه مسیر کتاب، هرجا کاربرد واقعی این Typeها مهم باشد، جزئیات بیشتری ارائه خواهد شد.

---

# Block 04 — Objects

# Object چیست؟

علاوه بر Primitiveها، JavaScript با **Object**ها نیز کار می‌کند.

Object می‌تواند مجموعه‌ای از Properties و Valueهای مرتبط را در یک ساختار واحد سازمان‌دهی کند.

برای مثال:

```javascript
const user = {
  firstName: "Omid",
  age: 30
};
```

در این مثال، `user` یک Object Value است.

Objectها فقط برای نگهداری چند Value نیستند؛ آن‌ها می‌توانند داده و رفتار مرتبط را نیز در یک ساختار سازمان‌دهی کنند.

جزئیات Properties، Methods، Mutation و Reference در فصل‌های Objects بررسی خواهند شد.

---

# Primitive و Object

یکی از مهم‌ترین تمایزهای مفهومی در Data Types این است که Valueها را در سطح مقدماتی به دو گروه اصلی تقسیم کنیم:

```text
Primitive Values
        vs
Object Values
```

Primitiveها شامل:

```text
Number
String
Boolean
Undefined
Null
Symbol
BigInt
```

و Objectها شامل Valueهایی هستند که ساختار Object دارند.

برای مثال:

```javascript
42
```

یک Primitive Value است.

اما:

```javascript
{
  name: "Omid"
}
```

یک Object Value است.

---

# چرا این تفاوت مهم است؟

Primitive و Object فقط دو نام برای دو گروه از Typeها نیستند.

رفتار آن‌ها در برخی عملیات‌های زبان متفاوت است.

برای مثال، Objectها می‌توانند Properties داشته باشند:

```javascript
const user = {
  name: "Omid"
};

console.log(user.name);
```

اما یک Number مانند:

```javascript
42
```

به همان معنای Object دارای مجموعه‌ای از Properties قابل تعریف توسط برنامه‌نویس نیست.

این تفاوت پایه‌ای است و در ادامه کتاب، هنگام ورود به Objects و References اهمیت بیشتری پیدا می‌کند.

---

# Primitive Values و رفتار آن‌ها

Primitive Valueها Valueهای بنیادی زبان هستند.

برای مثال:

```javascript
let age = 30;
```

در اینجا `30` یک Primitive Value از نوع `Number` است.

اگر Variable دیگری همان Value را دریافت کند:

```javascript
let age = 30;
let anotherAge = age;
```

هر دو Variable اکنون Value عددی `30` دارند.

در این سطح، نیازی نیست وارد جزئیات Memory Allocation یا Copying شویم.

هدف فقط این است که Primitive Value را از Object Value به‌عنوان دو مفهوم متفاوت تشخیص دهیم.

---

# Object Values و رفتار آن‌ها

Objectها ساختار پیچیده‌تری دارند.

برای مثال:

```javascript
const user = {
  name: "Omid"
};
```

در اینجا `user` به یک Object Value اشاره می‌کند که دارای Property است.

اگر Object دیگری در اختیار برنامه قرار بگیرد، نحوه مدیریت آن با Primitive Valueها یکسان نیست.

برای مثال:

```javascript
const user = {
  name: "Omid"
};

const anotherUser = user;
```

در این مرحله فقط باید بدانیم که Objectها **Reference Values** هستند و این موضوع باعث می‌شود رفتار آن‌ها در Assignment و Mutation با Primitiveها متفاوت باشد.

جزئیات دقیق References، Copying و Mutation در فصل **Objects Fundamentals** و سپس در فصل **Memory Management** بررسی خواهند شد.

---

# یک مرز مهم: Memory

برای درک تفاوت Primitive و Object، لازم است یک مدل ذهنی اولیه از رفتار آن‌ها داشته باشیم.

اما این به معنای آموزش کامل Memory Model نیست.

در این فصل فقط می‌خواهیم بدانیم:

```text
Primitive
↓
Value-oriented behavior

Object
↓
Reference-oriented behavior
```

جزئیات اینکه Reference دقیقاً چگونه مدیریت می‌شود، Valueها چگونه در Memory قرار می‌گیرند، Reachability چگونه شکل می‌گیرد و Garbage Collection چگونه عمل می‌کند، خارج از Scope این فصل است.

این موضوعات در فصل **Memory Management** بررسی خواهند شد.

---

# Common Mistakes

## تصور اینکه JavaScript بدون Type است

این تصور اشتباه است.

JavaScript دارای Typeهای مشخص است.

Dynamic Typing به این معناست که Type سیستم به‌صورت پویا در Runtime با Valueها کار می‌کند؛ نه اینکه Valueها بدون Type باشند.

---

## تصور اینکه Variable خودش Type دارد

عبارت:

> «Type متغیر تغییر کرد»

برای توضیح ساده قابل استفاده است، اما مدل ذهنی دقیق‌تر این است:

```text
Variable / Binding
↓
Value
↓
Type
```

برای مثال:

```javascript
let data = 42;

data = "JavaScript";
```

Value جدید Type متفاوتی دارد.

---

## تصور اینکه null و undefined یکسان هستند

این دو Value متفاوت‌اند.

```text
undefined → نبودن Value مشخص در یک وضعیت
null      → نمایش آگاهانه نبودن Value
```

---

## تصور اینکه Number و Integer دو Type جدا هستند

در JavaScript، اعداد معمولی از Type `Number` هستند.

```javascript
typeof 10;
typeof 10.5;
```

هر دو:

```text
"number"
```

هستند.

---

## تصور اینکه typeof همیشه Type دقیق را برمی‌گرداند

`typeof` ابزار مفیدی برای Type Checking است، اما برای همه موارد نتیجه‌ای که از نام Type انتظار داریم ارائه نمی‌کند.

مهم‌ترین مثال:

```javascript
typeof null;
```

نتیجه:

```text
"object"
```

خواهد بود.

این رفتار تاریخی زبان است و نباید آن را به این معنا تفسیر کرد که Type واقعی `null` برابر Object است.

---

# typeof

برای بررسی Type یک Value می‌توان از Operator زیر استفاده کرد:

```javascript
#typeof
```

برای مثال:

```javascript
typeof 20;
```

نتیجه:

```text
"number"
```

نمونه‌های دیگر:

```javascript
typeof "Hello";
// "string"

typeof true;
// "boolean"

typeof undefined;
// "undefined"
```

---

# typeof null

یکی از معروف‌ترین رفتارهای خاص JavaScript:

```javascript
typeof null;
```

نتیجه:

```text
"object"
```

است.

این نتیجه با Type واقعی `null` یکسان نیست.

`null` یک Primitive Value است، اما `typeof null` مقدار `"object"` را برمی‌گرداند.

این رفتار تاریخی برای حفظ سازگاری با کدهای قدیمی زبان باقی مانده است.

بنابراین:

```text
typeof null === "object"
```

را باید به‌عنوان یک رفتار خاص `typeof` بشناسیم، نه به‌عنوان تعریف Type `null`.

---

# Type Checking

**Type Checking** به فرایند بررسی Type یک Value گفته می‌شود.

`typeof` یکی از ابزارهای Type Checking در JavaScript است.

برای مثال:

```javascript
const age = 30;

console.log(typeof age);
```

خروجی:

```text
"number"
```

اما باید توجه داشته باشیم که `typeof` تنها یک ابزار برای بررسی Type است و محدودیت‌هایی نیز دارد.

در این فصل هدف، شناخت همین ابزار و ایجاد مدل ذهنی اولیه Type Checking است.

روش‌های پیشرفته‌تر بررسی Type و تشخیص Objectهای خاص در مباحث بعدی و در صورت نیاز معرفی خواهند شد.

---

# پاسخ کوتاه طلایی مصاحبه

**سؤال**

`typeof` چه کاری انجام می‌دهد؟

**پاسخ**

`typeof` یک Operator برای بررسی Type یک Value است و یک String مانند `"number"`، `"string"` یا `"object"` برمی‌گرداند. با این حال، در مواردی مانند `typeof null` محدودیت‌های تاریخی دارد.

---

# Summary

در این فصل بررسی کردیم که JavaScript چگونه انواع مختلف Value را مدیریت می‌کند.

ابتدا رابطه میان Value و Type را شناختیم.

سپس Dynamic Typing را بررسی کردیم و دیدیم که JavaScript دارای Typeهای مشخص است، اما Type به Value مربوط است و Variableها به یک Type ثابت محدود نیستند.

بعد از آن هفت Primitive Type را شناختیم:

```text
Number
String
Boolean
Undefined
Null
Symbol
BigInt
```

در ادامه Object را معرفی کردیم و تفاوت مفهومی Primitive و Object را بررسی کردیم.

در پایان نیز با `typeof` و مفهوم Type Checking آشنا شدیم.

---

# Key Takeaways

- **Data Type** مشخص می‌کند یک Value چه نوعی دارد و چه رفتارهایی روی آن قابل انجام است.
- JavaScript دارای Typeهای مشخص است و Dynamic Typing به معنای بدون Type بودن زبان نیست.
- در JavaScript، Type به Value مربوط است و Variableها به یک Type ثابت محدود نیستند.
- JavaScript دارای هفت Primitive Type است:
  - `Number`
  - `String`
  - `Boolean`
  - `Undefined`
  - `Null`
  - `Symbol`
  - `BigInt`
- `null` و `undefined` یکسان نیستند.
- `Object` نوعی Value ساختاریافته است که می‌تواند Properties و رفتار مرتبط را سازمان‌دهی کند.
- Primitive و Object در برخی رفتارهای زبان تفاوت دارند.
- Objectها Reference Values هستند؛ جزئیات References و Memory Management در فصل‌های تخصصی‌تر بررسی خواهند شد.
- `typeof` یکی از ابزارهای Type Checking است.
- `typeof null` مقدار `"object"` را برمی‌گرداند و این یک رفتار تاریخی زبان است.
- Type Conversion و Type Coercion در فصل بعدی مربوط به این موضوع به‌صورت کامل بررسی خواهند شد.

---

# Technical Interview

## سطح پایه (Junior)

### سؤال ۱

Data Type چیست؟

### سؤال ۲

چرا Type در JavaScript اهمیت دارد؟

### سؤال ۳

JavaScript چند Primitive Type دارد؟

### سؤال ۴

Primitive Typeهای JavaScript را نام ببرید.

### سؤال ۵

تفاوت `null` و `undefined` چیست؟

### سؤال ۶

`typeof` چه کاری انجام می‌دهد؟

### سؤال ۷

آیا Object یک Data Type در JavaScript است؟

---

## سطح متوسط (Mid-Level)

### سؤال ۸

Dynamic Typing در JavaScript چیست؟

### سؤال ۹

آیا خود Variable در JavaScript Type دارد یا Value؟

### سؤال ۱۰

تفاوت Primitive Value و Object Value چیست؟

### سؤال ۱۱

چرا `typeof null` مقدار `"object"` را برمی‌گرداند؟

### سؤال ۱۲

آیا Dynamic Typing به این معناست که JavaScript فاقد Type است؟

### سؤال ۱۳

Type Checking چیست و `typeof` چه نقشی در آن دارد؟

### سؤال ۱۴

چرا `Number` و `Integer` در JavaScript دو Type جداگانه نیستند؟

---

## سطح پیشرفته (Senior)

### سؤال ۱۵

چرا بهتر است به‌جای اینکه بگوییم «Type متغیر تغییر کرد»، درباره Type Value صحبت کنیم؟

### سؤال ۱۶

رابطه مفهومی زیر را توضیح دهید:

```text
Variable
↓
Value
↓
Type
```

### سؤال ۱۷

چرا Dynamic Typing با Dynamic Language یک مفهوم یکسان نیست؟

### سؤال ۱۸

چرا تفاوت Primitive و Object برای درک مباحث بعدی JavaScript اهمیت دارد؟

### سؤال ۱۹

چرا نباید نتیجه `typeof null` را به‌عنوان Type واقعی `null` تفسیر کرد؟

### سؤال ۲۰

چرا جزئیات Reference، Copying و Memory Management نباید در فصل Data Types به‌صورت کامل آموزش داده شوند؟

---

# Golden Answers

## Data Type چیست؟

Data Type مشخص می‌کند یک Value چه نوعی دارد و زبان چگونه باید با آن Value رفتار کند.

---

## Dynamic Typing چیست؟

JavaScript یک زبان Dynamically Typed است؛ یعنی Type به Value مربوط است و Type سیستم در Runtime با Valueهای واقعی برنامه سروکار دارد. بنابراین یک Variable می‌تواند در طول اجرای برنامه Valueهایی با Typeهای متفاوت داشته باشد.

---

## Primitive Typeهای JavaScript کدام‌اند؟

هفت Primitive Type عبارت‌اند از:

```text
Number
String
Boolean
Undefined
Null
Symbol
BigInt
```

---

## تفاوت null و undefined چیست؟

`undefined` معمولاً نشان می‌دهد Value مشخصی در یک وضعیت تعیین نشده است، در حالی که `null` معمولاً به‌صورت آگاهانه برای نمایش نبودن یک Value استفاده می‌شود.

---

## آیا Variable خودش Type دارد؟

مدل ذهنی دقیق‌تر این است که Type به Value مربوط است، نه اینکه Variable را به یک Type ثابت محدود کنیم.

برای مثال:

```javascript
let data = 42;

data = "JavaScript";
```

در اینجا Value تغییر کرده و Value جدید Type متفاوتی دارد.

---

## تفاوت Primitive و Object چیست؟

Primitiveها Valueهای بنیادی زبان هستند، در حالی که Objectها Valueهای ساختاریافته‌ای هستند که می‌توانند Properties و رفتار مرتبط را سازمان‌دهی کنند. Objectها همچنین Reference Values هستند و به همین دلیل رفتار آن‌ها در برخی عملیات با Primitiveها متفاوت است.

---

## چرا typeof null برابر object است؟

`typeof null` یک رفتار تاریخی JavaScript است که برای حفظ سازگاری با کدهای قدیمی زبان باقی مانده است. بنابراین نتیجه `"object"` نباید به‌عنوان Type واقعی `null` تفسیر شود.

---

## Type Checking چیست؟

Type Checking فرایند بررسی Type یک Value است. `typeof` یکی از ابزارهای ساده Type Checking در JavaScript است، اما محدودیت‌هایی دارد و برای همه سناریوها Type دقیق موردنظر را مشخص نمی‌کند.

---

## چرا Type Conversion و Type Coercion را در این فصل کامل بررسی نکردیم؟

زیرا سؤال اصلی این فصل شناخت Type و نحوه مدیریت آن در JavaScript است. قواعد تبدیل Typeها و Coercion موضوع فصل **Type Conversion and Coercion** هستند و آموزش کامل آن‌ها در این فصل باعث خروج از Scope می‌شود.

---

## چرا Memory و References را در این فصل فقط مقدماتی معرفی کردیم؟

زیرا تفاوت Primitive و Object برای ساخت مدل ذهنی صحیح ضروری است، اما جزئیات Reference، Memory Allocation، Reachability و Garbage Collection به مفاهیم بیشتری نیاز دارند و در فصل‌های تخصصی‌تر بررسی خواهند شد.

---

# Conclusion

شناخت Data Types فقط به حفظ کردن نام چند Type محدود نمی‌شود.

مدل ذهنی درست از این رابطه آغاز می‌شود:

```text
Value
↓
Type
```

سپس باید بدانیم JavaScript یک زبان Dynamically Typed است:

```text
Variable
↓
Value
↓
Type
```

بعد می‌توانیم Valueها را در سطح پایه به دو گروه مهم Primitive و Object تقسیم کنیم و تفاوت رفتاری آن‌ها را بشناسیم.

در نهایت، `typeof` ابزاری برای Type Checking در اختیار ما قرار می‌دهد، اما مانند هر ابزار دیگری محدودیت‌هایی دارد.

اکنون پایه مفهومی لازم برای ورود به مبحث **Type Conversion and Coercion** را داریم؛ جایی که بررسی خواهیم کرد JavaScript چه زمانی و چگونه Valueها را بین Typeهای مختلف تبدیل می‌کند.

---

# تمرین‌ها

## مرور مفاهیم

1. Data Type چیست؟
2. چرا Type برای JavaScript اهمیت دارد؟
3. Primitive Data Type چیست؟
4. هفت Primitive Type را نام ببرید.
5. تفاوت `null` و `undefined` چیست؟
6. Dynamic Typing چیست؟
7. تفاوت Primitive و Object چیست؟
8. `typeof` چه کاری انجام می‌دهد؟
9. چرا `typeof null` برابر `"object"` است؟
10. Type Checking چیست؟

---

## تحلیل

نوع داده Valueهای زیر را مشخص کنید:

```javascript
42

"Front-End"

false

undefined

null

123n
```

---

## تحلیل دوم

کد زیر را بررسی کنید:

```javascript
let value = 42;

value = "JavaScript";
```

پاسخ دهید:

- Variable یا Binding چیست؟
- Value اول چه Typeای دارد؟
- Value دوم چه Typeای دارد؟
- آیا بهتر است بگوییم «Variable Type تغییر کرد» یا «Value جدید Type متفاوتی دارد»؟

---

## تحلیل سوم

خروجی کدهای زیر را پیش‌بینی کنید:

```javascript
typeof 42

typeof "JavaScript"

typeof true

typeof undefined

typeof null
```

سپس دلیل خروجی `typeof null` را توضیح دهید.

---

## پیاده‌سازی

پنج Variable ایجاد کنید که هر کدام یکی از Typeهای زیر را نگهداری کنند:

- Number
- String
- Boolean
- Undefined
- Null

سپس Type هر Value را با استفاده از `typeof` در Console نمایش دهید.

در مرحله بعد یک Object ایجاد کنید و Type آن را نیز بررسی کنید:

```javascript
const user = {
  name: "Omid",
  age: 30
};
```

تفاوت نتیجه `typeof` برای Primitive Valueها و Object را توضیح دهید.
