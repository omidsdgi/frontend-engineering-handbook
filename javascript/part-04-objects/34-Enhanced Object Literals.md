# Chapter 21 — Enhanced Object Literals

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* توضیح دهید چرا JavaScript Syntaxهای مدرن‌تری برای ساخت Object Literal ارائه کرده است.
* Property Shorthand را زمانی که نام Variable و Property یکسان است به‌درستی به‌کار ببرید.
* Method Shorthand را از Function Property معمولی تشخیص دهید.
* Computed Property را برای ایجاد Propertyهای Dynamic استفاده کنید.
* توضیح دهید چرا Expression داخل `[]` می‌تواند Property Name را تعیین کند.
* تفاوت میان Property ثابت و Property پویا را تشخیص دهید.
* چند قابلیت Enhanced Object Literals را برای ساخت Objectهای واقعی Application ترکیب کنید.
* تشخیص دهید چه زمانی Syntax کوتاه‌تر باعث خوانایی بیشتر می‌شود و چه زمانی Mapping صریح مناسب‌تر است.

## Core Question

JavaScript چگونه Object Literals را برای نوشتن Objects مدرن‌تر و خواناتر کرده است؟

در فصل 19 با Object Literal آشنا شدیم و یاد گرفتیم چگونه Object را با Propertyهای مختلف ایجاد کنیم.

در فصل 20 نیز دیدیم که Object می‌تواند Behavior را از طریق Methodها مدل کند.

اکنون سؤال این است:

> آیا برای ساخت این Objectها همیشه باید تمام Syntax را به‌صورت کامل و تکراری بنویسیم؟

JavaScript در نسخه‌های مدرن Syntaxهایی ارائه کرده است که Object Construction را کوتاه‌تر، خواناتر و در بعضی موارد Dynamic می‌کنند.

مسیر این فصل چنین است:

```text
Object Literal
↓
Property Shorthand
↓
Method Shorthand
↓
Computed Properties
↓
Property Expressions
↓
Dynamic Object Construction
```

---

## مقدمه

فرض کنید اطلاعات یک Product در چند Variable قرار دارد:

```js
const name = 'Laptop';
const price = 1200;
```

برای ساخت Object می‌توانیم بنویسیم:

```js
const product = {
  name: name,
  price: price
};
```

این Code کاملاً صحیح است.

اما یک نکته وجود دارد.

در هر دو Property:

```text
name: name
price: price
```

نام Property و نام Variable یکسان است.

بنابراین بخشی از Syntax صرفاً تکرار همان اطلاعات است.

JavaScript برای این حالت Syntax کوتاه‌تری فراهم می‌کند:

```js
const product = {
  name,
  price
};
```

این Syntax بخشی از **Enhanced Object Literals** است.

هدف این قابلیت‌ها ایجاد نوع جدیدی از Object نیست.

Object همچنان همان Object است.

تفاوت در **نحوه بیان Object Construction** است.

---

# Property Shorthand

وقتی نام Variable و نام Property یکسان باشد، می‌توانیم از **Property Shorthand** استفاده کنیم.

مثال:

```js
const title = 'Pasta';
const servings = 4;

const recipe = {
  title,
  servings
};
```

این Code معادل است با:

```js
const recipe = {
  title: title,
  servings: servings
};
```

بنابراین:

```js
title
```

در Object Literal به این معناست:

```js
title: title
```

---

## چرا Property Shorthand وجود دارد؟

در Applicationهای واقعی معمولاً داده‌ها از قبل در Variableها قرار دارند.

برای مثال:

```js
const id = 42;
const title = 'Pasta';
const servings = 4;
```

سپس می‌خواهیم آن‌ها را در یک Object مدل کنیم:

```js
const recipe = {
  id,
  title,
  servings
};
```

بدون Shorthand باید بنویسیم:

```js
const recipe = {
  id: id,
  title: title,
  servings: servings
};
```

نسخه کوتاه‌تر یک تکرار غیرضروری را حذف می‌کند.

مدل ذهنی:

```text
Variable
   ↓
Property با همان نام
```

بنابراین Property Shorthand بیشتر از اینکه یک ترفند Syntaxی باشد، راهی برای بیان مستقیم‌تر رابطه میان داده و Property است.

---

## Shorthand چه چیزی را تغییر نمی‌دهد؟

Property Shorthand فقط Syntax را کوتاه می‌کند.

این دو Object از نظر Structure یکسان هستند:

```js
const name = 'Laptop';

const productA = {
  name
};
```

و:

```js
const name = 'Laptop';

const productB = {
  name: name
};
```

در هر دو حالت Object دارای Propertyای به نام `name` است.

Shorthand رفتار Object را تغییر نمی‌دهد.

فقط Syntax را ساده‌تر می‌کند.

---

## وقتی نام‌ها متفاوت هستند

Shorthand فقط زمانی قابل استفاده است که نام Variable و Property یکسان باشد.

برای مثال:

```js
const productName = 'Laptop';
```

اگر Property باید `name` باشد، نمی‌توانیم بنویسیم:

```js
const product = {
  name
};
```

زیرا Variableای به نام `name` وجود ندارد.

باید Mapping را صریح بنویسیم:

```js
const product = {
  name: productName
};
```

اینجا:

```text
productName
    ↓
   name
```

نام Variable و Property متفاوت است.

بنابراین Syntax کامل نه‌تنها مجاز، بلکه خواناتر و دقیق‌تر است.

---

# Method Shorthand

در فصل 20 یاد گرفتیم که Function می‌تواند به‌عنوان Property یک Object قرار گیرد و در صورت مدل کردن Behavior مرتبط، به‌عنوان Method استفاده شود.

Syntax قدیمی‌تر:

```js
const account = {
  balance: 5000,

  deposit: function (amount) {
    this.balance += amount;
  }
};
```

JavaScript Syntax کوتاه‌تری برای تعریف چنین Methodهایی فراهم می‌کند:

```js
const account = {
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

این Syntax را **Method Shorthand** می‌نامیم.

---

## Method Shorthand چگونه کار می‌کند؟

در Syntax معمولی Function Property داریم:

```js
deposit: function (amount) {
  this.balance += amount;
}
```

در Syntax مدرن:

```js
deposit(amount) {
  this.balance += amount;
}
```

نام Method مستقیماً قبل از Parameter List قرار می‌گیرد.

ساختار کلی:

```js
const object = {
  methodName(parameter) {
    // behavior
  }
};
```

مثال:

```js
const cart = {
  total: 0,

  add(price) {
    this.total += price;
  }
};
```

اکنون:

```js
cart.add(100);

console.log(cart.total);
```

خروجی:

```text
100
```

---

## تفاوت Method Shorthand و Function Property

این دو Syntax را مقایسه کنید:

```js
const account = {
  deposit: function (amount) {
    this.balance += amount;
  }
};
```

و:

```js
const account = {
  deposit(amount) {
    this.balance += amount;
  }
};
```

هر دو برای تعریف Behavior در Object Literal استفاده می‌شوند.

تفاوت اصلی مورد بحث این فصل در **Syntax تعریف** است.

مفهوم Method قبلاً در Chapter 20 بررسی شده است.

در نتیجه، Method Shorthand مفهوم جدیدی از Method ایجاد نمی‌کند.

این فقط Syntax مدرن‌تر و فشرده‌تری برای تعریف Method در Object Literal است.

---

## چرا Method Shorthand مفید است؟

در Code واقعی Methodها بخش مهمی از Objectهای مدل‌کننده Entity هستند:

```js
const account = {
  owner: 'Omid',
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  },

  withdraw(amount) {
    this.balance -= amount;
  }
};
```

ساختار Object به‌سرعت قابل مشاهده است:

```text
Account
├── owner
├── balance
├── deposit()
└── withdraw()
```

Syntax کوتاه‌تر باعث می‌شود تمرکز خواننده بیشتر روی **نام و Behavior Method** باشد تا روی جزئیات تکراری `function`.

---

# Computed Properties

تا اینجا Property Nameها را مستقیماً در Object Literal نوشته‌ایم:

```js
const product = {
  name: 'Laptop',
  price: 1200
};
```

اما گاهی نام Property از قبل مشخص نیست.

ممکن است نام Property از داده‌ای که در اختیار Application است به‌دست آید.

برای مثال:

```js
const field = 'price';
```

اگر بخواهیم مقدار `field` را به‌عنوان Property Name استفاده کنیم، می‌توانیم از **Computed Property** استفاده کنیم:

```js
const product = {
  [field]: 1200
};
```

نتیجه:

```js
{
  price: 1200
}
```

---

## چرا `[]` مهم است؟

این دو Code را مقایسه کنید:

```js
const field = 'price';

const productA = {
  field: 1200
};
```

و:

```js
const productB = {
  [field]: 1200
};
```

در Object اول:

```js
field: 1200
```

Propertyای با نام literal یعنی:

```text
field
```

ایجاد می‌شود.

اما در Object دوم:

```js
[field]: 1200
```

Expression داخل `[]` ارزیابی می‌شود.

مقدار آن:

```text
'price'
```

است.

بنابراین Property Name برابر می‌شود با:

```text
price
```

مدل ذهنی:

```text
field
 ↓
'price'
 ↓
Property Name
 ↓
price
```

---

## Definition

**Computed Property** روشی برای تعیین Property Name بر اساس نتیجه یک Expression در Object Literal است.

Syntax کلی:

```js
const object = {
  [expression]: value
};
```

Expression داخل `[]` ارزیابی می‌شود و نتیجه آن به‌عنوان Property Name استفاده می‌شود.

---

# Property Expressions

Computed Property فقط به یک Variable ساده محدود نیست.

Expression داخل `[]` می‌تواند از یک محاسبه یا ترکیب چند Value تشکیل شود.

برای مثال:

```js
const prefix = 'user';

const user = {
  [`${prefix}Name`]: 'Omid'
};
```

در اینجا Expression:

```js
`${prefix}Name`
```

ارزیابی می‌شود.

نتیجه:

```text
userName
```

است.

بنابراین Object شامل:

```js
{
  userName: 'Omid'
}
```

خواهد بود.

این همان مفهوم **Property Expressions** در Concept Flow این فصل است.

---

## Expression می‌تواند Dynamic باشد

فرض کنید Application بر اساس نوع داده، نام Property را تولید می‌کند:

```js
const prefix = 'user';
const field = 'role';

const user = {
  [`${prefix}${field}`]: 'admin'
};
```

Expression:

```js
`${prefix}${field}`
```

به:

```text
userrole
```

تبدیل می‌شود.

در نتیجه Property Name همان مقدار خواهد بود.

اگر نام‌ها با قالب مناسب ساخته شوند:

```js
const prefix = 'user';
const field = 'Role';

const user = {
  [`${prefix}${field}`]: 'admin'
};
```

نتیجه:

```js
{
  userRole: 'admin'
}
```

نکته مهم این است که Property Name در این حالت مستقیماً در Syntax نوشته نشده است.

Property Name از داده و Expression ساخته می‌شود.

---

# Dynamic Object Construction

اکنون سه قابلیت اصلی را داریم:

```text
Property Shorthand
Method Shorthand
Computed Properties
```

می‌توانیم این قابلیت‌ها را هنگام ساخت Objectهای واقعی Application با یکدیگر ترکیب کنیم.

فرض کنید اطلاعات یک Product از بخش دیگری از Application آمده است:

```js
const name = 'Laptop';
const price = 1200;

const stockField = 'stock';
const stock = 15;
```

می‌توانیم Object را چنین بسازیم:

```js
const product = {
  name,
  price,
  [stockField]: stock,

  increaseStock(amount) {
    this.stock += amount;
  }
};
```

Object حاصل:

```text
Product
├── name
├── price
├── stock
└── increaseStock()
```

در این مثال:

```text
name
price
→ Property Shorthand

[stockField]
→ Computed Property

increaseStock()
→ Method Shorthand
```

این ترکیب هدف اصلی Enhanced Object Literals را نشان می‌دهد:

> Object را می‌توان مستقیماً از داده‌های موجود، Propertyهای Dynamic و Behaviorهای مرتبط ساخت؛ بدون اینکه Syntax غیرضروری تکرار شود.

---

## ساخت Object از داده‌های Application

فرض کنید Application اطلاعات یک Recipe را در اختیار دارد:

```js
const id = 42;
const title = 'Pasta';
const servings = 4;
```

ساخت Object بسیار مستقیم است:

```js
const recipe = {
  id,
  title,
  servings
};
```

اکنون فرض کنید یک Property نیز بر اساس داده تعیین می‌شود:

```js
const field = 'rating';
const value = 4.8;
```

می‌توانیم همان Object را توسعه دهیم:

```js
const recipe = {
  id,
  title,
  servings,
  [field]: value
};
```

نتیجه:

```js
{
  id: 42,
  title: 'Pasta',
  servings: 4,
  rating: 4.8
}
```

در اینجا:

```text
id
title
servings
→ Shorthand

[field]
→ Computed Property
```

این الگو در Applicationهایی که بخشی از Structure داده بر اساس اطلاعات Runtime تعیین می‌شود، مفید است.

---

## API Data

فرض کنید یک API اطلاعات Product را در اختیار Application قرار می‌دهد و بخشی از داده باید به یک Object داخلی منتقل شود.

مثلاً:

```js
const id = 101;
const name = 'Laptop';
const price = 1200;
```

می‌توانیم مدل داخلی را بسازیم:

```js
const product = {
  id,
  name,
  price
};
```

اگر یک Property خاص بر اساس Configuration انتخاب شود:

```js
const extraField = 'stock';
const extraValue = 15;
```

می‌توانیم آن را نیز اضافه کنیم:

```js
const product = {
  id,
  name,
  price,
  [extraField]: extraValue
};
```

این مثال نشان می‌دهد که Enhanced Object Literals می‌توانند فاصله میان داده‌های موجود و Object Model برنامه را کاهش دهند.

---

## Configuration Objects

این Syntax فقط برای Data Modelها نیست.

Configuration Objectها نیز معمولاً از Variableهای موجود ساخته می‌شوند:

```js
const timeout = 5000;
const retries = 3;
const debug = true;

const config = {
  timeout,
  retries,
  debug
};
```

اگر یک Configuration Key به‌صورت Dynamic تعیین شود:

```js
const environment = 'production';
const value = true;

const config = {
  debug: false,
  [environment]: value
};
```

در اینجا Property Name از مقدار Variable به‌دست می‌آید.

هدف این Syntaxها همچنان همان است:

```text
Existing Data
     ↓
Object Construction
```

---

# Best Practices

## 1. زمانی که نام Variable و Property یکسان است از Property Shorthand استفاده کنید

به‌جای:

```js
const title = 'Pasta';

const recipe = {
  title: title
};
```

بنویسید:

```js
const recipe = {
  title
};
```

این Syntax تکرار غیرضروری را حذف می‌کند.

---

## 2. برای Methodها از Method Shorthand استفاده کنید

به‌جای:

```js
const account = {
  deposit: function (amount) {
    this.balance += amount;
  }
};
```

بنویسید:

```js
const account = {
  deposit(amount) {
    this.balance += amount;
  }
};
```

این شکل Intent Code را بهتر نشان می‌دهد.

---

## 3. Computed Property را برای Propertyهای واقعاً Dynamic استفاده کنید

مناسب:

```js
const field = 'price';

const product = {
  [field]: 1200
};
```

اما برای Property ثابت:

```js
const product = {
  ['price']: 1200
};
```

دلیلی برای استفاده از Computed Property وجود ندارد.

شکل ساده‌تر:

```js
const product = {
  price: 1200
};
```

خواناتر است.

---

## 4. اگر نام Variable و Property متفاوت است، Mapping را صریح نگه دارید

برای مثال:

```js
const productName = 'Laptop';

const product = {
  name: productName
};
```

تلاش برای کوتاه کردن این Code لزوماً آن را بهتر نمی‌کند.

در اینجا Mapping صریح بخشی از معنای Code است:

```text
productName
    ↓
   name
```

---

## 5. Dynamic Object Construction باید دلیل مشخصی داشته باشد

اگر Structure Object از قبل مشخص است:

```js
const user = {
  name,
  email,
  role
};
```

از Syntax ساده استفاده کنید.

اگر Property Name واقعاً بر اساس داده تعیین می‌شود:

```js
const field = getFieldName();

const user = {
  [field]: value
};
```

Computed Property انتخاب مناسبی است.

---

# اشتباهات رایج

### اشتباه ۱: تصور اینکه `field` و `[field]` یکسان هستند

```js
const field = 'price';

const product = {
  field: 1200
};
```

Propertyای به نام:

```text
field
```

ایجاد می‌شود.

اما:

```js
const product = {
  [field]: 1200
};
```

Propertyای به نام:

```text
price
```

ایجاد می‌شود.

---

### اشتباه ۲: استفاده از Shorthand برای نام‌های متفاوت

این Code معتبر نیست:

```js
const productName = 'Laptop';

const product = {
  name
};
```

زیرا Variableای به نام `name` وجود ندارد.

باید بنویسیم:

```js
const product = {
  name: productName
};
```

---

### اشتباه ۳: استفاده غیرضروری از Computed Property

این Code معتبر است:

```js
const product = {
  ['price']: 1200
};
```

اما Property Name کاملاً ثابت است.

بنابراین:

```js
const product = {
  price: 1200
};
```

خواناتر است.

---

### اشتباه ۴: اشتباه گرفتن Method Shorthand با مفهوم جدید Method

در Chapter 20 مفهوم Method را یاد گرفتیم.

در این فصل:

```js
const account = {
  deposit(amount) {
    this.balance += amount;
  }
};
```

چیز جدیدی درباره مفهوم Method اضافه نمی‌کنیم.

چیزی که جدید است Syntax تعریف Method در Object Literal است.

---

### اشتباه ۵: تصور اینکه Shorthand نام Property را تغییر می‌دهد

در:

```js
const title = 'Pasta';

const recipe = {
  title
};
```

Property Name برابر:

```text
title
```

است.

اگر Property باید `name` باشد:

```js
const recipe = {
  name: title
};
```

باید Mapping را صریح بنویسیم.

Shorthand فقط زمانی قابل استفاده است که نام Property و Variable یکسان باشند.

---

### اشتباه ۶: استفاده از Computed Property بدون نیاز

اگر می‌دانیم Property Name ثابت است:

```js
const product = {
  price: 1200
};
```

استفاده از:

```js
const product = {
  ['price']: 1200
};
```

هیچ مزیت واقعی ایجاد نمی‌کند.

Syntax باید Intent واقعی Code را نشان دهد.

---

## Summary

در این فصل از Object Literal معمولی شروع کردیم.

ابتدا دیدیم که وقتی نام Property و Variable یکسان هستند، می‌توانیم از Property Shorthand استفاده کنیم:

```js
const title = 'Pasta';

const recipe = {
  title
};
```

که معادل:

```js
const recipe = {
  title: title
};
```

است.

سپس Method Shorthand را بررسی کردیم:

```js
const account = {
  deposit(amount) {
    this.balance += amount;
  }
};
```

این Syntax شکل کوتاه‌تر تعریف Method در Object Literal است.

بعد به Computed Properties رسیدیم:

```js
const field = 'price';

const product = {
  [field]: 1200
};
```

در اینجا Expression داخل `[]` ارزیابی می‌شود و نتیجه آن Property Name را مشخص می‌کند.

در ادامه دیدیم که Expression می‌تواند از ترکیب چند Value نیز ساخته شود:

```js
const prefix = 'user';

const user = {
  [`${prefix}Name`]: 'Omid'
};
```

در نهایت این Syntaxها را در ساخت Objectهای واقعی ترکیب کردیم:

```js
const name = 'Laptop';
const price = 1200;
const field = 'stock';
const stock = 15;

const product = {
  name,
  price,
  [field]: stock,

  increaseStock(amount) {
    this.stock += amount;
  }
};
```

بنابراین Concept Flow این فصل به این شکل کامل می‌شود:

```text
Object Literal
↓
Property Shorthand
↓
Method Shorthand
↓
Computed Properties
↓
Property Expressions
↓
Dynamic Object Construction
```

Enhanced Object Literals مفهوم جدیدی از Object ایجاد نمی‌کنند.

آن‌ها نحوه بیان Object Construction را بهبود می‌دهند.

---

## Key Takeaways

* Enhanced Object Literals برای نوشتن Objectهای خواناتر و انعطاف‌پذیرتر استفاده می‌شوند.
* Property Shorthand تکرار `property: variable` را زمانی که نام‌ها یکسان هستند حذف می‌کند.
* Method Shorthand روش کوتاه‌تری برای تعریف Method در Object Literal است.
* `field` و `[field]` در Object Literal رفتار یکسانی ندارند.
* Computed Property با `[]` نوشته می‌شود.
* Expression داخل Computed Property ارزیابی می‌شود.
* نتیجه Expression به‌عنوان Property Name استفاده می‌شود.
* Computed Properties برای Dynamic Keys مناسب هستند.
* Property Shorthand و Method Shorthand برای خوانایی Object Construction مفید هستند.
* Computed Properties نباید برای Propertyهای کاملاً ثابت به‌صورت غیرضروری استفاده شوند.
* اگر نام Variable و Property متفاوت باشد، Mapping صریح خواناتر است.
* Enhanced Object Literals را می‌توان برای ساخت Objectهای واقعی Application ترکیب کرد.
* این فصل Object را از نظر **نحوه ساخت** توسعه می‌دهد؛ Destructuring در فصل بعد بررسی خواهد شد.

---

# Technical Interview

## Junior

### 1. Property Shorthand چیست؟

Property Shorthand زمانی استفاده می‌شود که نام Property و نام Variable یکسان باشد:

```js
const name = 'Omid';

const user = {
  name
};
```

این Code معادل:

```js
const user = {
  name: name
};
```

است.

---

### 2. Method Shorthand چیست؟

Method Shorthand Syntax کوتاه‌تری برای تعریف Method در Object Literal است:

```js
const user = {
  login() {
    console.log('Logged in');
  }
};
```

به‌جای:

```js
const user = {
  login: function () {
    console.log('Logged in');
  }
};
```

استفاده می‌شود.

---

### 3. Computed Property چیست؟

Computed Property روشی برای تعیین Property Name بر اساس نتیجه یک Expression است:

```js
const field = 'price';

const product = {
  [field]: 1200
};
```

در اینجا مقدار `field` یعنی `price` به‌عنوان Property Name استفاده می‌شود.

---

### 4. تفاوت `field` و `[field]` چیست؟

در:

```js
const field = 'price';

const product = {
  field: 1200
};
```

Propertyای با نام `field` ساخته می‌شود.

اما در:

```js
const product = {
  [field]: 1200
};
```

Expression داخل `[]` ارزیابی می‌شود و Propertyای با نام `price` ساخته می‌شود.

---

### 5. Property Shorthand چه زمانی قابل استفاده است؟

زمانی که نام Variable و Property یکسان باشد:

```js
const title = 'Pasta';

const recipe = {
  title
};
```

اگر نام‌ها متفاوت باشند، باید Mapping صریح بنویسیم:

```js
const recipe = {
  name: title
};
```

---

## Mid-Level

### 6. چرا Property Shorthand برای Object Construction مفید است؟

زیرا وقتی داده‌ها از قبل در Variableهایی با نام مناسب قرار دارند، تکرار غیرضروری را حذف می‌کند و رابطه میان Variable و Property را مستقیم‌تر نشان می‌دهد:

```js
const id = 42;
const title = 'Pasta';

const recipe = {
  id,
  title
};
```

این Code همان Structure نسخه کامل `id: id` و `title: title` را ایجاد می‌کند، اما خواناتر است.

---

### 7. آیا Method Shorthand رفتار جدیدی به Method اضافه می‌کند؟

خیر.

Method Shorthand بیشتر یک Syntax مدرن برای تعریف Method در Object Literal است.

مفهوم Method و نحوه Invocation آن قبلاً بررسی شده است.

در این فصل تمرکز بر نحوه نوشتن Method در Object Literal است:

```js
const cart = {
  add(price) {
    this.total += price;
  }
};
```

---

### 8. چرا Computed Properties برای Dynamic Object Construction مهم هستند؟

زیرا Property Name همیشه از قبل ثابت نیست.

برای مثال:

```js
const field = 'rating';
const value = 4.8;

const recipe = {
  [field]: value
};
```

اگر `field` تغییر کند، Property Name Object نیز تغییر می‌کند.

بنابراین Structure Object می‌تواند بر اساس داده موجود ساخته شود.

---

### 9. آیا Computed Property همیشه انتخاب بهتری است؟

خیر.

اگر Property Name ثابت باشد:

```js
const product = {
  price: 1200
};
```

خواناتر از:

```js
const product = {
  ['price']: 1200
};
```

است.

Computed Property زمانی ارزش دارد که Property Name واقعاً Dynamic باشد.

---

### 10. Expression در Computed Property چه نقشی دارد؟

Expression داخل `[]` ارزیابی می‌شود و نتیجه آن Property Name را مشخص می‌کند.

مثلاً:

```js
const prefix = 'user';

const user = {
  [`${prefix}Name`]: 'Omid'
};
```

Expression:

```js
`${prefix}Name`
```

ارزیابی شده و نتیجه آن:

```text
userName
```

به‌عنوان Property Name استفاده می‌شود.

---

### 11. Enhanced Object Literals چه چیزی را تغییر می‌دهند؟

آن‌ها مدل Object را تغییر نمی‌دهند.

Object همچنان همان ساختار Property/Value و Methodهای قبلی را دارد.

این قابلیت‌ها عمدتاً **Syntax و نحوه بیان Object Construction** را بهبود می‌دهند:

* Property Shorthand
* Method Shorthand
* Computed Properties

---

## Senior

### 12. چرا Enhanced Object Literals را باید بیشتر یک قابلیت برای بیان Intent دانست تا یک قابلیت جدید Object؟

زیرا Enhanced Object Literals رفتار بنیادی Object را تغییر نمی‌دهند.

برای مثال:

```js
const name = 'Laptop';

const product = {
  name
};
```

از نظر نتیجه همان Objectی را ایجاد می‌کند که نسخه کامل:

```js
const product = {
  name: name
};
```

ایجاد می‌کند.

ارزش اصلی Syntax مدرن در کاهش تکرار، افزایش خوانایی و بیان مستقیم‌تر رابطه میان داده‌های موجود و Object Model است.

---

### 13. چه زمانی Mapping صریح بهتر از Shorthand است؟

زمانی که نام Variable و Property متفاوت است یا Mapping بخشی از معنای Domain Model است.

برای مثال:

```js
const productName = 'Laptop';

const product = {
  name: productName
};
```

اگر بخواهیم فقط برای کوتاه‌تر شدن Code نام‌ها را تغییر دهیم، ممکن است Intent را مبهم کنیم.

در این حالت:

```text
productName
    ↓
   name
```

یک Mapping معنادار است و بهتر است صریح باقی بماند.

---

### 14. چگونه می‌توان چند قابلیت Enhanced Object Literals را در یک Object واقعی ترکیب کرد؟

برای مثال:

```js
const name = 'Laptop';
const price = 1200;

const field = 'stock';
const stock = 15;

const product = {
  name,
  price,
  [field]: stock,

  increaseStock(amount) {
    this.stock += amount;
  }
};
```

در این Object:

```text
name
price
→ Property Shorthand

[field]
→ Computed Property

increaseStock()
→ Method Shorthand
```

هر Syntax مسئول یک بخش مشخص از Object Construction است.

---

### 15. چرا استفاده بیش از حد از Computed Properties می‌تواند کیفیت Code را کاهش دهد؟

زیرا Syntax باید Structure و Intent واقعی Object را منعکس کند.

اگر Property ثابت باشد:

```js
const user = {
  name: 'Omid'
};
```

مستقیماً نشان می‌دهد که Object دارای Propertyای به نام `name` است.

اما:

```js
const user = {
  ['name']: 'Omid'
};
```

یک لایه Syntax اضافه ایجاد می‌کند، بدون اینکه Dynamic بودن واقعی وجود داشته باشد.

بنابراین Computed Property باید زمانی استفاده شود که Dynamic بودن Property بخشی از مسئله باشد.

---

### 16. تفاوت Property Shorthand و Computed Property از نظر مسئله‌ای که حل می‌کنند چیست؟

Property Shorthand یک مسئله **تکرار Syntax** را حل می‌کند:

```js
const name = 'Omid';

const user = {
  name
};
```

درحالی‌که Computed Property یک مسئله **Dynamic Property Naming** را حل می‌کند:

```js
const field = 'name';

const user = {
  [field]: 'Omid'
};
```

بنابراین:

```text
Shorthand
→ کاهش تکرار

Computed Property
→ تعیین Dynamic Property Name
```

---

# Golden Answers

## Junior — Golden Answer

### Property Shorthand چیست؟

> Property Shorthand Syntax کوتاه‌تری برای Object Literal است که وقتی نام Variable و Property یکسان هستند، اجازه می‌دهد به‌جای `property: variable` فقط نام Variable را بنویسیم. برای مثال `const user = { name }` معادل `const user = { name: name }` است.

---

### Computed Property چیست؟

> Computed Property اجازه می‌دهد Property Name را بر اساس نتیجه یک Expression تعیین کنیم. Expression داخل `[]` ارزیابی می‌شود و نتیجه آن به‌عنوان Property Name استفاده می‌شود؛ مانند `{ [field]: value }`.

---

## Mid-Level — Golden Answer

### چرا Enhanced Object Literals مهم هستند؟

> Enhanced Object Literals مدل جدیدی از Object ایجاد نمی‌کنند؛ بلکه نحوه ساخت Object را خواناتر و انعطاف‌پذیرتر می‌کنند. Property Shorthand تکرار غیرضروری را حذف می‌کند، Method Shorthand تعریف Behavior را ساده‌تر می‌کند و Computed Properties امکان ایجاد Dynamic Keys را فراهم می‌کنند.

---

### چه زمانی از Computed Property استفاده می‌کنید؟

> زمانی که Property Name واقعاً از یک Variable یا Expression به‌دست می‌آید و در زمان نوشتن Code کاملاً ثابت نیست. برای Propertyهای ثابت بهتر است از Syntax معمولی استفاده شود، زیرا Intent را واضح‌تر بیان می‌کند.

---

## Senior — Golden Answer

### Enhanced Object Literals چه مسئله مهندسی را حل می‌کنند؟

> مسئله اصلی آن‌ها نحوه بیان Object Construction است، نه تغییر مدل Object. در Applicationهای واقعی معمولاً داده‌ها از قبل در Variableها قرار دارند و گاهی Property Name نیز از داده یا Configuration به‌صورت Dynamic تعیین می‌شود. Property Shorthand رابطه مستقیم میان داده و Property را بدون تکرار بیان می‌کند، Method Shorthand Behavior را خواناتر تعریف می‌کند و Computed Properties اجازه می‌دهند Structure Object بر اساس Runtime Data ساخته شود. در نتیجه Object Construction هم concise و هم expressive می‌شود، بدون اینکه مفهوم بنیادی Object تغییر کند.

---

### چگونه تشخیص می‌دهید Syntax کوتاه‌تر واقعاً بهتر است؟

> کوتاه‌تر بودن به‌تنهایی معیار کافی نیست. اگر Shorthand همان Intent را واضح‌تر بیان کند، مانند `name` برای `name: name`، انتخاب مناسبی است. اما اگر نام Variable و Property متفاوت باشد، Mapping صریح مانند `name: productName` بخشی از معنای Code است و نباید صرفاً برای کوتاه‌تر شدن حذف شود. همین اصل درباره Computed Properties نیز صدق می‌کند: Dynamic Syntax زمانی مناسب است که Dynamic بودن واقعاً بخشی از مسئله باشد.

---

# Conclusion

در Chapter 19 یاد گرفتیم چگونه Object را ایجاد کنیم و Propertyهای آن را مدیریت کنیم.

در Chapter 20 یاد گرفتیم که Object می‌تواند Behavior را از طریق Methodها مدل کند.

اکنون در Chapter 21 همان Object Literal را از زاویه دیگری بررسی کردیم:

**چگونه می‌توان Object را با Syntax خواناتر و انعطاف‌پذیرتر ساخت؟**

مسیر فصل چنین بود:

```text
Object Literal
↓
Property Shorthand
↓
Method Shorthand
↓
Computed Properties
↓
Property Expressions
↓
Dynamic Object Construction
```

ابتدا دیدیم وقتی نام Variable و Property یکسان است:

```js
const title = 'Pasta';

const recipe = {
  title
};
```

می‌توانیم از Property Shorthand استفاده کنیم.

سپس Method Shorthand را دیدیم:

```js
const recipe = {
  save() {
    console.log('Recipe saved');
  }
};
```

و در ادامه Computed Properties را بررسی کردیم:

```js
const field = 'rating';

const recipe = {
  [field]: 4.8
};
```

در اینجا Property Name دیگر لزوماً در زمان نوشتن Object ثابت نیست؛ بلکه می‌تواند از یک Expression به‌دست آید.

در پایان، این قابلیت‌ها را کنار هم قرار دادیم تا ببینیم Objectهای واقعی Application چگونه می‌توانند مستقیماً از داده‌های موجود ساخته شوند:

```js
const title = 'Pasta';
const field = 'rating';
const rating = 4.8;

const recipe = {
  title,
  [field]: rating,

  save() {
    console.log('Recipe saved');
  }
};
```

بنابراین Enhanced Object Literals مفهوم جدیدی از Object ایجاد نمی‌کنند.

آن‌ها **نحوه بیان Object Construction** را بهتر می‌کنند.

در فصل بعد، مسیر از **ساخت Object** به **استخراج داده از Object** تغییر می‌کند.
