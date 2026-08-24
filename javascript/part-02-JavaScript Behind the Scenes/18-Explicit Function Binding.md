# Chapter 18 — Explicit Function Binding

# اهداف فصل

پس از مطالعه این فصل انتظار می‌رود بتوانید:

* مفهوم **Explicit Function Binding** را توضیح دهید.
* بدانید چرا گاهی لازم است مقدار `this` را به‌صورت صریح کنترل کنیم.
* تفاوت **Implicit Binding** و **Explicit Binding** را درک کنید.
* متد `call()` را برای اجرای Function با `this` مشخص به‌کار ببرید.
* متد `apply()` را بشناسید و تفاوت آن را با `call()` توضیح دهید.
* متد `bind()` را برای ایجاد یک Function جدید با `this` مشخص استفاده کنید.
* تفاوت `call()`، `apply()` و `bind()` را از نظر زمان اجرا و نحوه دریافت Arguments تحلیل کنید.
* مفهوم **Function Borrowing** را درک کنید.
* مفهوم **Partial Application** را با استفاده از `bind()` توضیح دهید.
* خطاهای رایج هنگام استفاده از Explicit Binding را تشخیص دهید.
* به پرسش‌های فنی مرتبط با Explicit Function Binding در سطوح Junior، Mid-Level و Senior پاسخ دهید.

---

# Core Question

> **چگونه مقدار `this` را به‌صورت صریح کنترل کنیم؟**

---

# Concept Flow

```text
this
↓
Implicit Binding
↓
Explicit Binding
↓
call()
↓
apply()
↓
bind()
↓
Function Borrowing
↓
Partial Application
↓
Common Mistakes
↓
Best Practices
```

---
# مقدمه

تا اینجا با Functionها، نحوه تعریف و فراخوانی آن‌ها، Parameters و Arguments و همچنین Callback و Closure آشنا شدیم.

در این فصل با مفهومی روبه‌رو می‌شویم که هنگام اجرای بعضی Functionها اهمیت پیدا می‌کند:

this
this چیست؟

this یک Keyword ویژه در JavaScript است که در زمان اجرای یک Regular Function به یک Value اشاره می‌کند.

نکته مهم این است که مقدار this هنگام تعریف Function تعیین نمی‌شود؛ بلکه به نحوه فراخوانی Function وابسته است.

برای مثال:

function showName() {
console.log(this.name);
}

در اینجا Function از this استفاده می‌کند، اما هنوز نمی‌توانیم فقط از روی تعریف Function مشخص کنیم:

this → ?

مقدار this زمانی مشخص می‌شود که Function را اجرا کنیم.

بنابراین می‌توانیم فعلاً این مدل ذهنی ساده را داشته باشیم:

Function Definition
↓
Function Invocation
↓
this

یعنی نحوه Invocation می‌تواند مشخص کند this هنگام اجرای Function به چه Valueای اشاره کند.

در این فصل فقط همین مقدار از this را نیاز داریم. قواعد کامل this و انواع Invocation در فصل‌های بعدی بررسی خواهند شد.

اکنون مسئله اصلی این فصل شکل می‌گیرد:

اگر بخواهیم خودمان مشخص کنیم this هنگام اجرای یک Function به چه Valueای اشاره کند، چه کار باید کنیم؟

JavaScript برای این کار متدهایی را در اختیار Functionها قرار می‌دهد:

call()
apply()
bind()

در ادامه، ابتدا Explicit Binding و دلیل نیاز به آن را بررسی می‌کنیم.



# Block 01 — Explicit Binding

## مشکل چیست؟

فرض کنید یک Function داریم که از `this` استفاده می‌کند.

```javascript
const user = {
  name: 'Omid',

  showName() {
    console.log(this.name);
  }
};

user.showName();
```

در این حالت:

```javascript
this
```

به `user` مربوط است.

اما فرض کنید Function را در یک Variable قرار دهیم:

```javascript
const showName = user.showName;
```

اکنون دیگر آن را به شکل زیر فراخوانی نمی‌کنیم:

```javascript
user.showName();
```

بلکه:

```javascript
showName();
```

در نتیجه دیگر نمی‌توانیم صرفاً با نگاه کردن به خود Function انتظار داشته باشیم `this` همان Object قبلی باشد.

این موضوع یک مسئله مهم ایجاد می‌کند:

> اگر Function از `this` استفاده کند، چگونه می‌توانیم خودمان مشخص کنیم `this` به چه Objectی اشاره کند؟

پاسخ:

**Explicit Binding**

---

## Explicit Binding چیست؟

### تعریف ساده

**Explicit Binding** یعنی هنگام فراخوانی یا آماده‌سازی یک Function، به‌صورت صریح مشخص کنیم `this` باید به چه Valueای اشاره کند.

JavaScript برای این کار سه متد اصلی در اختیار ما قرار می‌دهد:

```javascript
call()
apply()
bind()
```

هر سه از Functionها در برابر یک مسئله مشابه استفاده می‌کنند:

```text
چه چیزی باید this باشد؟
```

اما روش و زمان استفاده از آن‌ها متفاوت است.

---

## تعریف فنی

`call()`، `apply()` و `bind()` متدهایی هستند که روی Functionها در دسترس‌اند و امکان تعیین صریح **this value** را فراهم می‌کنند.

تفاوت اصلی آن‌ها در این است که:

* `call()` Function را **بلافاصله اجرا** می‌کند.
* `apply()` نیز Function را **بلافاصله اجرا** می‌کند.
* `bind()` Function را **اجرا نمی‌کند**؛ بلکه Function جدیدی ایجاد می‌کند که `this` آن از قبل تعیین شده است.

این تفاوت، هسته اصلی فصل است.

---

# چرا Explicit Binding مهم است؟

در پروژه‌های واقعی، Functionها همیشه به یک Context ثابت وابسته نیستند.

ممکن است یک Function:

* برای چند Object استفاده شود.
* به‌عنوان یک Callback منتقل شود.
* در زمان دیگری اجرا شود.
* بخشی از یک API قابل استفاده مجدد باشد.
* نیاز داشته باشد برخی Arguments از قبل مشخص شوند.

اگر Function به `this` وابسته باشد، جدا شدن آن از نحوه فراخوانی اصلی می‌تواند رفتار آن را تغییر دهد.

Explicit Binding این امکان را فراهم می‌کند که این وابستگی را به‌صورت کنترل‌شده مدیریت کنیم.

---

## مثال

فرض کنید یک Function برای نمایش نام داریم:

```javascript
function showName() {
  console.log(this.name);
}
```

اکنون دو Object داریم:

```javascript
const user = {
  name: 'Omid'
};

const admin = {
  name: 'Sara'
};
```

خود Function مشخص نمی‌کند `this` باید `user` باشد یا `admin`.

ما می‌توانیم هنگام استفاده، آن را مشخص کنیم.

```javascript
showName.call(user);
showName.call(admin);
```

خروجی:

```text
Omid
Sara
```

همان Function برای دو Object مختلف استفاده شده است.

---

## تحلیل مهندسی

در این مثال Function:

```javascript
showName
```

Reusable است.

Function به Object خاصی وابسته نشده است.

در عوض، Object موردنظر هنگام فراخوانی مشخص می‌شود.

این الگو پایه بسیاری از کاربردهای **Function Reuse** در JavaScript است.

---

## اشتباهات رایج

❌ تصور اینکه `this` همیشه به Objectی اشاره می‌کند که Function در آن تعریف شده است.

✔ مقدار `this` در Functionهای Regular به نحوه فراخوانی Function وابسته است.

---

❌ تصور اینکه `call()` فقط برای تغییر مقدار `this` است و Function را اجرا نمی‌کند.

✔ `call()` علاوه بر تعیین `this`، Function را بلافاصله اجرا می‌کند.

---

## نکات مهم

* Explicit Binding یعنی تعیین صریح `this`.
* ابزارهای اصلی آن `call()`، `apply()` و `bind()` هستند.
* `call()` و `apply()` اجرای Function را فوراً انجام می‌دهند.
* `bind()` Function جدید ایجاد می‌کند و آن را فوراً اجرا نمی‌کند.
* Explicit Binding امکان Reuse کردن Functionها را افزایش می‌دهد.

---

# Block 02 — `call()`

## `call()` چیست؟

### تعریف ساده

`call()` یک Function را **بلافاصله اجرا** می‌کند و اجازه می‌دهد مقدار `this` و Arguments آن را مشخص کنیم.

Syntax:

```javascript
functionName.call(thisArg, arg1, arg2, ...);
```

مهم‌ترین بخش Syntax این است:

```javascript
thisArg
```

این Value همان چیزی است که برای `this` در نظر گرفته می‌شود.

---

## مثال ساده

```javascript
function introduce() {
  console.log(`My name is ${this.name}`);
}

const user = {
  name: 'Omid'
};

introduce.call(user);
```

خروجی:

```text
My name is Omid
```

---

## چه اتفاقی افتاد؟

ابتدا Function را تعریف کردیم:

```javascript
function introduce() {
  console.log(`My name is ${this.name}`);
}
```

در خود Function نگفتیم:

```javascript
this = user
```

چنین Assignment ای برای `this` انجام نمی‌دهیم.

سپس هنگام فراخوانی گفتیم:

```javascript
introduce.call(user);
```

یعنی:

> Function را اجرا کن و برای این اجرای Function، `this` را `user` در نظر بگیر.

در نتیجه:

```javascript
this.name
```

به:

```javascript
user.name
```

دسترسی پیدا می‌کند.

---

# ارسال Arguments با `call()`

`call()` علاوه بر `thisArg` می‌تواند Arguments را نیز دریافت کند.

برای مثال:

```javascript
function introduce(role) {
  console.log(`${this.name} is a ${role}`);
}

const user = {
  name: 'Omid'
};

introduce.call(user, 'developer');
```

خروجی:

```text
Omid is a developer
```

در اینجا:

```javascript
user
```

برای `this` استفاده شده است.

و:

```javascript
'developer'
```

به Parameter به نام `role` منتقل شده است.

---

## مدل ذهنی `call()`

می‌توانیم `call()` را این‌گونه تصور کنیم:

```text
Function
   │
   ├── this → Object
   │
   └── arguments → Parameters
```

یعنی:

```javascript
introduce.call(user, 'developer');
```

تقریباً از نظر مفهومی می‌گوید:

```text
this = user
role = 'developer'
Function را همین حالا اجرا کن
```

---

## `call()` و Invocation

نکته مهم این است که `call()` خودش یک Invocation ایجاد می‌کند.

مثلاً:

```javascript
introduce.call(user);
```

Function را اجرا می‌کند.

بنابراین:

```javascript
const result = introduce.call(user);
```

متغیر `result` مقدار **Return Value** همان Function را دریافت خواهد کرد.

مثال:

```javascript
function getName() {
  return this.name;
}

const user = {
  name: 'Omid'
};

const name = getName.call(user);

console.log(name);
```

خروجی:

```text
Omid
```

---

## مثال واقعی‌تر

فرض کنید یک Function برای نمایش اطلاعات Account داریم:

```javascript
function showAccount(role) {
  return `${this.name} - ${role}`;
}
```

دو Account داریم:

```javascript
const admin = {
  name: 'Sara'
};

const customer = {
  name: 'Omid'
};
```

اکنون می‌توانیم همان Function را برای هر دو استفاده کنیم:

```javascript
console.log(showAccount.call(admin, 'Admin'));

console.log(showAccount.call(customer, 'Customer'));
```

خروجی:

```text
Sara - Admin
Omid - Customer
```

یک Function داریم، اما Context را هنگام Invocation مشخص کرده‌ایم.

---

## تحلیل مهندسی

مزیت اصلی این روش این است که Logic مربوط به Function را از Data مربوط به Object جدا می‌کند.

Function:

```javascript
showAccount
```

مسئول Logic است.

Object:

```javascript
admin
customer
```

داده را فراهم می‌کنند.

و `call()` این دو را هنگام اجرای Function به یکدیگر متصل می‌کند.

---

## اشتباهات رایج

### اشتباه اول: فراموش کردن Invocation

این کد:

```javascript
showAccount.call;
```

Function را اجرا نمی‌کند.

در اینجا فقط به خود متد `call` دسترسی پیدا کرده‌ایم.

برای اجرای Function باید بنویسیم:

```javascript
showAccount.call(customer);
```

---

### اشتباه دوم: قرار دادن Arguments در یک Array

برای `call()` Arguments به‌صورت جداگانه ارسال می‌شوند:

```javascript
showAccount.call(customer, 'Customer');
```

نه:

```javascript
showAccount.call(customer, ['Customer']);
```

مگر اینکه خود Function انتظار یک Array داشته باشد.

---

## نکات مهم

* Syntax اصلی:

```javascript
fn.call(thisArg, arg1, arg2);
```

* `call()` Function را فوراً اجرا می‌کند.
* اولین Argument مربوط به `this` است.
* Arguments بعدی به Parameters Function منتقل می‌شوند.
* Return Value همان Function برگردانده می‌شود.

---

# Block 03 — `apply()`

## `apply()` چیست؟

### تعریف ساده

`apply()` مانند `call()` Function را با یک `thisArg` مشخص **بلافاصله اجرا می‌کند**.

تفاوت اصلی این است که Arguments را به‌صورت یک **Array-like Object** دریافت می‌کند.

Syntax:

```javascript
functionName.apply(thisArg, argsArray);
```

---

## مثال

همان Function قبلی را در نظر بگیرید:

```javascript
function introduce(role, department) {
  console.log(`${this.name} - ${role} - ${department}`);
}

const user = {
  name: 'Omid'
};
```

با `call()` می‌توانیم بنویسیم:

```javascript
introduce.call(user, 'Developer', 'Frontend');
```

اما با `apply()`:

```javascript
introduce.apply(user, ['Developer', 'Frontend']);
```

خروجی هر دو یکی است:

```text
Omid - Developer - Frontend
```

---

# تفاوت `call()` و `apply()`

از نظر تعیین `this` و اجرای Function، تفاوتی میان آن‌ها وجود ندارد.

تفاوت اصلی در نحوه ارسال Arguments است.

### `call()`

```javascript
fn.call(object, arg1, arg2, arg3);
```

### `apply()`

```javascript
fn.apply(object, [arg1, arg2, arg3]);
```

بنابراین:

```text
call  → Arguments جداگانه
apply → یک مجموعه Arguments
```

---

## مدل ذهنی

فرض کنید Function به سه ورودی نیاز دارد:

```javascript
function createReport(title, author, format) {
  // ...
}
```

با `call()`:

```javascript
createReport.call(report, 'Sales', 'Omid', 'PDF');
```

با `apply()`:

```javascript
createReport.apply(report, ['Sales', 'Omid', 'PDF']);
```

در هر دو حالت:

```text
this → report
```

اما شکل ارسال Arguments متفاوت است.

---

# چه زمانی `apply()` مفید است؟

در گذشته، زمانی که Arguments از قبل در قالب یک Array یا Array-like Object قرار داشتند، `apply()` کاربرد بیشتری داشت.

برای مثال:

```javascript
const values = [10, 20, 30];

someFunction.apply(context, values);
```

در این حالت نیازی نبود Arguments را یکی‌یکی استخراج کنیم.

امروزه Syntaxهای جدید JavaScript مانند **Spread Syntax** بسیاری از این موارد را ساده‌تر کرده‌اند.

در نتیجه، `apply()` را بیشتر باید به‌عنوان بخشی از مدل ذهنی Function Binding و هنگام کار با کدهای موجود بشناسیم.

---

# `call()` یا `apply()`؟

قاعده ساده:

اگر Arguments از قبل به‌صورت جداگانه دارید:

```javascript
fn.call(object, value1, value2);
```

اگر Arguments در یک Collection قرار دارند:

```javascript
fn.apply(object, values);
```

در هر دو حالت، Function بلافاصله اجرا می‌شود.

---

## تحلیل مهندسی

تفاوت `call()` و `apply()` مربوط به **نحوه انتقال Arguments** است، نه نوع Binding.

هر دو:

1. `this` را تعیین می‌کنند.
2. Function را اجرا می‌کنند.
3. Return Value Function را برمی‌گردانند.

تنها تفاوت اصلی:

```text
call  → arg1, arg2, arg3
apply → [arg1, arg2, arg3]
```

---

## اشتباهات رایج

❌ تصور اینکه `apply()` Function را اجرا نمی‌کند.

✔ `apply()` مانند `call()` Function را فوراً اجرا می‌کند.

---

❌ تصور اینکه `apply()` همیشه بهتر از `call()` است.

✔ تفاوت آن‌ها عمدتاً در شکل ارسال Arguments است.

---

❌ تصور اینکه `apply()` و `bind()` یک کار انجام می‌دهند.

✔ `apply()` Function را اجرا می‌کند؛ `bind()` Function جدید ایجاد می‌کند.

---

## نکات مهم

* `apply()` Explicit Binding انجام می‌دهد.
* Function را بلافاصله اجرا می‌کند.
* اولین Argument، `thisArg` است.
* Argument دوم مجموعه Arguments است.
* تفاوت اصلی `call()` و `apply()` در شکل ارسال Arguments است.

---

# Block 04 — `bind()`

## مشکل جدید

تا اینجا دیدیم:

```javascript
call()
apply()
```

هر دو Function را بلافاصله اجرا می‌کنند.

اما همیشه نمی‌خواهیم Function همین حالا اجرا شود.

گاهی می‌خواهیم:

> یک Function جدید بسازیم که از قبل بداند `this` چه مقداری است و بعداً آن را اجرا کنیم.

برای این نیاز از:

```javascript
bind()
```

استفاده می‌کنیم.

---

# `bind()` چیست؟

### تعریف ساده

`bind()` یک **Function جدید** ایجاد می‌کند که `this` آن از قبل مشخص شده است.

برخلاف `call()` و `apply()`، `bind()` Function اصلی را بلافاصله اجرا نمی‌کند.

Syntax:

```javascript
const newFunction = originalFunction.bind(thisArg);
```

---

## مثال

```javascript
function showName() {
  console.log(this.name);
}

const user = {
  name: 'Omid'
};

const showUserName = showName.bind(user);
```

در اینجا Function اجرا نشده است.

تنها یک Function جدید ساخته‌ایم:

```javascript
showUserName
```

اکنون می‌توانیم بعداً آن را اجرا کنیم:

```javascript
showUserName();
```

خروجی:

```text
Omid
```

---

# تفاوت اصلی `bind()` با `call()`

این دو کد را مقایسه کنید:

```javascript
showName.call(user);
```

و:

```javascript
const showUserName = showName.bind(user);
```

در حالت اول:

```text
Function → همین حالا اجرا می‌شود
```

در حالت دوم:

```text
Function → Function جدید ساخته می‌شود
```

و سپس:

```javascript
showUserName();
```

باعث اجرای آن Function جدید می‌شود.

---

## مدل ذهنی `bind()`

می‌توانیم `bind()` را به‌صورت یک Function آماده تصور کنیم:

```text
Original Function
       │
       │ bind(user)
       ▼
Bound Function
       │
       │ later
       ▼
Invocation
```

یعنی `bind()` اجرای Function را به آینده موکول می‌کند.

---

# چرا این قابلیت مهم است؟

فرض کنید Functionی داریم که باید در آینده اجرا شود، اما می‌خواهیم `this` آن از همین حالا مشخص باشد.

برای مثال:

```javascript
function showUser() {
  console.log(this.name);
}

const user = {
  name: 'Omid'
};

const callback = showUser.bind(user);
```

اکنون می‌توانیم `callback` را به بخشی از برنامه بدهیم تا بعداً آن را اجرا کند.

Function از قبل می‌داند:

```text
this → user
```

این ویژگی یکی از دلایل مهم استفاده از `bind()` در APIها و Callbackها است.

---

# `bind()` و Arguments

`bind()` تنها برای تعیین `this` نیست.

می‌توانیم برخی Arguments را نیز هنگام ساخت Function جدید مشخص کنیم.

برای مثال:

```javascript
function introduce(role, company) {
  console.log(`${this.name} is a ${role} at ${company}`);
}

const user = {
  name: 'Omid'
};

const introduceAsDeveloper =
  introduce.bind(user, 'Developer');
```

اکنون:

```javascript
introduceAsDeveloper('Acme');
```

خروجی:

```text
Omid is a Developer at Acme
```

در اینجا:

```javascript
user
```

برای `this` مشخص شده است.

و:

```javascript
'Developer'
```

نیز از قبل به Parameter اول داده شده است.

تنها مقدار باقی‌مانده:

```javascript
'Acme'
```

هنگام اجرای Function ارسال می‌شود.

---

# Partial Application

این الگو یک مفهوم مهم‌تر را نیز نشان می‌دهد:

**Partial Application**

### تعریف ساده

Partial Application یعنی بخشی از Arguments یک Function را از قبل مشخص کنیم و Function جدیدی بسازیم که فقط Arguments باقی‌مانده را دریافت کند.

مثال:

```javascript
function calculatePrice(price, tax) {
  return price + price * tax;
}
```

فرض کنید نرخ مالیات همیشه:

```text
0.09
```

است.

می‌توانیم آن را از قبل مشخص کنیم:

```javascript
const calculateWithTax =
  calculatePrice.bind(null, undefined, 0.09);
```

اما این مثال از نظر طراحی چندان خوانا نیست.

یک الگوی ساده‌تر برای درک مفهوم:

```javascript
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
```

اکنون:

```javascript
double(5);
```

خروجی:

```text
10
```

زیرا:

```text
a = 2
b = 5
```

Function جدید فقط مقدار `b` را دریافت می‌کند.

---

## نکته مهم درباره `bind()`

در مثال:

```javascript
multiply.bind(null, 2);
```

ما `this` را نیز مشخص کرده‌ایم:

```javascript
null
```

اما Function به `this` نیاز ندارد.

بنابراین مقدار `this` در این مثال نقش مهمی ندارد.

آنچه برای Partial Application اهمیت دارد، این بخش است:

```javascript
2
```

که به‌عنوان اولین Argument از قبل ثبت شده است.

---

# Function Borrowing

یکی از کاربردهای مهم Explicit Binding، **Function Borrowing** است.

### تعریف ساده

Function Borrowing یعنی یک Object از Functionای که در Object دیگری قرار دارد استفاده کند، بدون اینکه آن Function را دوباره بنویسیم.

فرض کنید:

```javascript
const user = {
  name: 'Omid',

  introduce() {
    console.log(`My name is ${this.name}`);
  }
};
```

اکنون Object دیگری داریم:

```javascript
const admin = {
  name: 'Sara'
};
```

به جای نوشتن دوباره `introduce()` می‌توانیم همان Function را Borrow کنیم.

```javascript
user.introduce.call(admin);
```

خروجی:

```text
My name is Sara
```

Function متعلق به `user` بود، اما هنگام اجرای آن:

```text
this → admin
```

قرار گرفت.

بنابراین `admin` از رفتار Function استفاده کرد.

---

## چرا Function Borrowing مفید است؟

فرض کنید Logic یک Function عمومی است و Objectهای مختلف به همان رفتار نیاز دارند.

نوشتن چند نسخه از یک Function باعث:

* تکرار کد
* افزایش هزینه نگهداری
* احتمال ناسازگاری Logicها

می‌شود.

Function Borrowing می‌تواند در چنین شرایطی از Reuse کردن Logic پشتیبانی کند.

---

# مقایسه سه متد

اکنون می‌توانیم تفاوت سه ابزار اصلی را در یک جدول ببینیم.

| Method    | تعیین `this` | اجرای فوری | ایجاد Function جدید | دریافت Arguments         |
| --------- | ------------ | ---------- | ------------------- | ------------------------ |
| `call()`  | بله          | بله        | خیر                 | جداگانه                  |
| `apply()` | بله          | بله        | خیر                 | به‌صورت یک Collection    |
| `bind()`  | بله          | خیر        | بله                 | جداگانه و قابل پیش‌تعیین |

مدل ذهنی ساده:

```text
call()
→ this را تعیین کن
→ Arguments را بده
→ همین حالا اجرا کن

apply()
→ this را تعیین کن
→ Arguments را به‌صورت Collection بده
→ همین حالا اجرا کن

bind()
→ this را تعیین کن
→ Function جدید بساز
→ بعداً اجرا کن
```

---

# یک مثال برای مقایسه

```javascript
function showUser(role) {
  return `${this.name} - ${role}`;
}

const user = {
  name: 'Omid'
};
```

### `call()`

```javascript
showUser.call(user, 'Developer');
```

Function بلافاصله اجرا می‌شود.

---

### `apply()`

```javascript
showUser.apply(user, ['Developer']);
```

Function بلافاصله اجرا می‌شود.

---

### `bind()`

```javascript
const showDeveloper = showUser.bind(user, 'Developer');

showDeveloper();
```

ابتدا Function جدید ساخته می‌شود و بعداً اجرا می‌شود.

---

# تحلیل مهندسی

در هر سه حالت یک اصل مشترک وجود دارد:

```text
Function Logic
      +
Explicit Context
```

اما زمان اتصال متفاوت است.

`call()` و `apply()`:

```text
Binding + Invocation
```

را هم‌زمان انجام می‌دهند.

`bind()`:

```text
Binding
```

را انجام می‌دهد و `Invocation` را به زمان دیگری موکول می‌کند.

این تفاوت برای طراحی APIهای قابل استفاده مجدد بسیار مهم است.

---

## اشتباهات رایج

### اشتباه اول: انتظار اجرای `bind()`

این کد:

```javascript
showName.bind(user);
```

Function را اجرا نمی‌کند.

برای اجرای Function جدید باید آن را فراخوانی کنیم:

```javascript
showName.bind(user)();
```

یا بهتر:

```javascript
const boundShowName = showName.bind(user);

boundShowName();
```

---

### اشتباه دوم: تصور اینکه `bind()` Function اصلی را تغییر می‌دهد

```javascript
const boundShowName = showName.bind(user);
```

Function اصلی:

```javascript
showName
```

تغییر نمی‌کند.

یک Function جدید ساخته می‌شود:

```javascript
boundShowName
```

---

### اشتباه سوم: اشتباه گرفتن `bind()` با Call

❌

```javascript
const result = showName.bind(user);
```

`result` نتیجه اجرای `showName` نیست.

`result` یک Function جدید است.

---

## نکات مهم

* `bind()` Function را اجرا نمی‌کند.
* `bind()` یک Function جدید ایجاد می‌کند.
* `this` در Function جدید از قبل مشخص می‌شود.
* می‌توان Arguments را نیز از قبل مشخص کرد.
* Partial Application یکی از کاربردهای مهم `bind()` است.

---

# Block 05 — Practical Patterns

## Function Borrowing در یک مثال واقعی

فرض کنید یک Function برای ساختن پیام Account داریم:

```javascript
const account = {
  owner: 'Omid',

  getSummary() {
    return `${this.owner}'s account`;
  }
};
```

Object دیگری نیز داریم:

```javascript
const anotherAccount = {
  owner: 'Sara'
};
```

می‌توانیم همان Method را برای Object دوم استفاده کنیم:

```javascript
const summary =
  account.getSummary.call(anotherAccount);

console.log(summary);
```

خروجی:

```text
Sara's account
```

در اینجا Logic فقط یک بار نوشته شده است.

---

# Object Reuse

Explicit Binding می‌تواند به جدا کردن Behavior از Object خاص کمک کند.

به جای:

```javascript
const admin = {
  name: 'Sara',

  showName() {
    console.log(this.name);
  }
};

const customer = {
  name: 'Omid',

  showName() {
    console.log(this.name);
  }
};
```

می‌توان Logic مشترک را یک بار تعریف کرد:

```javascript
function showName() {
  console.log(this.name);
}
```

و سپس:

```javascript
showName.call(admin);
showName.call(customer);
```

این الگو زمانی ارزشمند است که Function واقعاً Logic مشترکی داشته باشد.

هدف Explicit Binding ایجاد پیچیدگی مصنوعی نیست.

---

# Event Handler Pattern

در بعضی APIهای رویدادمحور، یک Function به‌عنوان Callback در اختیار سیستم دیگری قرار می‌گیرد.

اگر Function به `this` وابسته باشد، ممکن است لازم باشد Context آن را از قبل مشخص کنیم.

`bind()` برای چنین سناریویی می‌تواند مفید باشد:

```javascript
const controller = {
  name: 'Recipe Controller',

  handle() {
    console.log(this.name);
  }
};

const boundHandler = controller.handle.bind(controller);
```

اکنون:

```javascript
boundHandler();
```

همیشه با Context مشخص‌شده ساخته شده است.

در این فصل وارد API مربوط به Eventها نمی‌شویم؛ این مثال تنها الگوی استفاده از `bind()` را نشان می‌دهد.

---

# چه زمانی از Explicit Binding استفاده نکنیم؟

Explicit Binding ابزار قدرتمندی است، اما نباید صرفاً به دلیل وجود آن از آن استفاده کنیم.

اگر Function به `this` نیاز ندارد:

```javascript
function add(a, b) {
  return a + b;
}
```

استفاده از:

```javascript
add.call(someObject, 2, 3);
```

معنای مفیدی ایجاد نمی‌کند.

همچنین اگر یک Function فقط برای یک Object خاص نوشته شده و Method Call کاملاً مناسب است، Explicit Binding ممکن است پیچیدگی غیرضروری ایجاد کند.

قاعده مهندسی:

> ابتدا ساده‌ترین Invocation را انتخاب کنید و فقط زمانی از Explicit Binding استفاده کنید که کنترل صریح Context واقعاً مسئله را حل کند.

---

# Best Practice

برای انتخاب میان این سه متد، ابتدا این سؤال را بپرسید:

### آیا Function باید همین حالا اجرا شود؟

اگر بله:

```javascript
call()
```

یا:

```javascript
apply()
```

### آیا Arguments جداگانه هستند؟

از:

```javascript
call()
```

استفاده کنید.

### آیا Arguments در یک Collection قرار دارند؟

`apply()` می‌تواند مناسب باشد.

### آیا Function باید بعداً اجرا شود؟

از:

```javascript
bind()
```

استفاده کنید.

### آیا بخشی از Arguments باید از قبل مشخص شود؟

`bind()` می‌تواند برای **Partial Application** مناسب باشد.

---

# اشتباهات رایج

## اشتباه اول: استفاده از `bind()` برای اجرای فوری

```javascript
showName.bind(user);
```

این Function را اجرا نمی‌کند.

---

## اشتباه دوم: تصور اینکه `call()` و `apply()` Function جدید می‌سازند

آن‌ها Function را همان لحظه اجرا می‌کنند.

---

## اشتباه سوم: تصور اینکه `bind()` Function اصلی را تغییر می‌دهد

`bind()` یک Function جدید ایجاد می‌کند.

---

## اشتباه چهارم: استفاده بی‌دلیل از Explicit Binding

اگر Function به `this` وابسته نیست، Explicit Binding معمولاً ارزش اضافه‌ای ایجاد نمی‌کند.

---

## اشتباه پنجم: اشتباه گرفتن `thisArg` با Argument اول Function

در:

```javascript
showUser.call(user, 'Developer');
```

این دو مقدار نقش متفاوتی دارند:

```text
user
↓
thisArg

'Developer'
↓
Function Argument
```

---

# نکات مهم

* `call()` و `apply()` برای Invocation فوری هستند.
* `bind()` برای ساختن Function جدید است.
* `call()` Arguments را جداگانه می‌گیرد.
* `apply()` Arguments را به‌صورت یک Collection دریافت می‌کند.
* `bind()` می‌تواند Arguments را نیز از قبل مشخص کند.
* Function Borrowing یکی از کاربردهای مهم Explicit Binding است.
* Partial Application می‌تواند با `bind()` پیاده‌سازی شود.
* Explicit Binding باید برای حل یک مسئله واقعی استفاده شود، نه صرفاً برای پیچیده‌تر کردن کد.

---

# Jonas Perspective

در رویکرد آموزشی Jonas Schmedtmann، `this` یکی از بخش‌هایی است که باید بر اساس **نحوه فراخوانی Function** درک شود، نه با حفظ کردن یک مقدار ثابت برای آن.

در همین چارچوب، `call()`، `apply()` و `bind()` ابزارهایی هستند که به برنامه‌نویس اجازه می‌دهند Context مربوط به `this` را هنگام استفاده از Function کنترل کند.

قاعده عملی مهم این است:

* `call()` برای اجرای فوری با Arguments جداگانه
* `apply()` برای اجرای فوری با مجموعه Arguments
* `bind()` برای ساختن Function جدید و اجرای آن در آینده

بنابراین بهتر است به‌جای حفظ کردن سه Syntax، ابتدا این سؤال را بپرسیم:

> **آیا می‌خواهم Function را همین حالا اجرا کنم یا Function جدیدی برای اجرای بعدی بسازم؟**

این سؤال معمولاً انتخاب میان `call()`، `apply()` و `bind()` را ساده می‌کند.

---

# Summary

در این فصل بررسی کردیم که گاهی نحوه عادی فراخوانی Function برای تعیین Context موردنظر کافی نیست.

در چنین شرایطی می‌توان از **Explicit Binding** استفاده کرد.

ابتدا `call()` را بررسی کردیم.

```javascript
fn.call(object, arg1, arg2);
```

`call()` مقدار `this` را مشخص می‌کند و Function را بلافاصله اجرا می‌کند.

سپس `apply()` را بررسی کردیم.

```javascript
fn.apply(object, [arg1, arg2]);
```

رفتار آن از نظر Binding و Invocation مانند `call()` است، اما Arguments را به‌صورت یک Collection دریافت می‌کند.

در ادامه به `bind()` رسیدیم.

```javascript
const newFn = fn.bind(object);
```

`bind()` Function را اجرا نمی‌کند؛ بلکه Function جدیدی ایجاد می‌کند که `this` آن از قبل مشخص شده است.

همچنین دیدیم که `bind()` می‌تواند بخشی از Arguments را نیز از قبل تعیین کند. این الگو **Partial Application** نام دارد.

در نهایت با **Function Borrowing** آشنا شدیم؛ یعنی استفاده از Logic یک Function برای Context یا Object دیگری بدون بازنویسی همان Logic.

---

# Key Takeaways

* **Explicit Binding** یعنی تعیین صریح `this` برای استفاده از یک Function.
* `call()` Function را بلافاصله اجرا می‌کند.
* `apply()` نیز Function را بلافاصله اجرا می‌کند.
* تفاوت اصلی `call()` و `apply()` در نحوه ارسال Arguments است.
* `call()` Arguments را جداگانه دریافت می‌کند.
* `apply()` Arguments را به‌صورت یک Collection دریافت می‌کند.
* `bind()` Function را اجرا نمی‌کند.
* `bind()` یک Function جدید ایجاد می‌کند.
* Function جدید ایجادشده با `bind()`، `this` مشخص‌شده را حفظ می‌کند.
* `bind()` می‌تواند برای **Partial Application** نیز استفاده شود.
* **Function Borrowing** یعنی استفاده مجدد از یک Function برای Context دیگری.
* Explicit Binding زمانی ارزشمند است که کنترل صریح `this` یک نیاز واقعی باشد.
* انتخاب میان `call()`، `apply()` و `bind()` بیشتر به **زمان Invocation** و **نحوه انتقال Arguments** بستگی دارد.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Explicit Binding چیست؟

### پاسخ

Explicit Binding یعنی تعیین صریح مقدار `this` هنگام استفاده از یک Function. در JavaScript این کار معمولاً با `call()`، `apply()` یا `bind()` انجام می‌شود.

---

### سؤال ۲

`call()` چه کاری انجام می‌دهد؟

### پاسخ

`call()` مقدار `this` را تعیین می‌کند و Function را بلافاصله اجرا می‌کند. Arguments نیز به‌صورت جداگانه بعد از `thisArg` ارسال می‌شوند.

---

### سؤال ۳

`apply()` چه تفاوتی با `call()` دارد؟

### پاسخ

هر دو `this` را مشخص کرده و Function را بلافاصله اجرا می‌کنند. تفاوت اصلی این است که `call()` Arguments را جداگانه می‌گیرد، اما `apply()` آن‌ها را به‌صورت یک Collection دریافت می‌کند.

---

### سؤال ۴

`bind()` چه تفاوتی با `call()` دارد؟

### پاسخ

`call()` Function را بلافاصله اجرا می‌کند، اما `bind()` یک Function جدید ایجاد می‌کند و اجرای آن را به زمان دیگری موکول می‌کند.

---

### سؤال ۵

آیا `bind()` Function اصلی را تغییر می‌دهد؟

### پاسخ

خیر. `bind()` یک Function جدید ایجاد می‌کند و Function اصلی را تغییر نمی‌دهد.

---

### سؤال ۶

در کد زیر چه اتفاقی می‌افتد؟

```javascript
function showName() {
  console.log(this.name);
}

const user = {
  name: 'Omid'
};

showName.call(user);
```

### پاسخ

Function بلافاصله اجرا می‌شود و برای این Invocation، `this` برابر با `user` خواهد بود؛ بنابراین `Omid` چاپ می‌شود.

---

## سطح Mid-Level

### سؤال ۷

چرا `call()` و `apply()` را Explicit Binding می‌نامیم؟

### پاسخ

زیرا هنگام Invocation به‌صورت صریح مشخص می‌کنیم `this` باید به چه Valueای اشاره کند. این مقدار از نحوه معمول Invocation مستقل تعیین می‌شود.

---

### سؤال ۸

چرا `bind()` برای Callbackها مفید است؟

### پاسخ

زیرا می‌توانیم Function جدیدی ایجاد کنیم که `this` آن از قبل مشخص شده باشد و سپس آن Function را برای اجرای بعدی در اختیار سیستم یا API دیگری قرار دهیم.

---

### سؤال ۹

Function Borrowing چیست؟

### پاسخ

Function Borrowing یعنی استفاده از یک Function برای Object یا Context دیگری بدون بازنویسی Function. `call()` و `apply()` ابزارهای رایجی برای این الگو هستند.

---

### سؤال ۱۰

Partial Application چیست؟

### پاسخ

Partial Application یعنی بخشی از Arguments یک Function را از قبل مشخص کنیم و Function جدیدی بسازیم که فقط Arguments باقی‌مانده را دریافت کند. `bind()` می‌تواند این الگو را پیاده‌سازی کند.

---

### سؤال ۱۱

اگر Function به `this` نیاز نداشته باشد، آیا استفاده از `call()` یا `bind()` منطقی است؟

### پاسخ

معمولاً خیر. Explicit Binding زمانی ارزش دارد که کنترل `this` یا آماده‌سازی بخشی از Arguments یک نیاز واقعی باشد.

---

### سؤال ۱۲

آیا `call()` و `apply()` Function جدید ایجاد می‌کنند؟

### پاسخ

خیر. آن‌ها Function موجود را همان لحظه اجرا می‌کنند و Return Value آن را برمی‌گردانند.

---

### سؤال ۱۳

در این کد چه چیزی در Variable به نام `boundFn` قرار می‌گیرد؟

```javascript
const boundFn = fn.bind(user);
```

### پاسخ

`boundFn` نتیجه اجرای `fn` نیست؛ بلکه یک Function جدید است که `this` آن از قبل به `user` متصل شده است.

---

## سطح Senior

### سؤال ۱۴

تفاوت مفهومی `call()`، `apply()` و `bind()` را بدون اشاره به Syntax توضیح دهید.

### پاسخ

`call()` و `apply()` برای Invocation فوری یک Function با Context مشخص هستند و فقط در نحوه دریافت Arguments تفاوت دارند. `bind()` به‌جای Invocation فوری، یک Function جدید با Context از پیش تعیین‌شده ایجاد می‌کند.

---

### سؤال ۱۵

چرا `bind()` را می‌توان یک ابزار برای کنترل زمان Invocation دانست؟

### پاسخ

زیرا `bind()` Binding را در یک مرحله انجام می‌دهد اما Invocation را انجام نمی‌دهد. Function جدید بعداً و در زمان دلخواه اجرا می‌شود، در حالی که `this` موردنظر از قبل مشخص شده است.

---

### سؤال ۱۶

چرا Function Borrowing می‌تواند به کاهش Duplication کمک کند؟

### پاسخ

زیرا Logic یک Function را می‌توان برای چند Context یا Object مختلف استفاده کرد، بدون اینکه همان Logic دوباره نوشته شود. Context موردنظر هنگام Invocation مشخص می‌شود.

---

### سؤال ۱۷

اگر Functionی با `bind()` ساخته شده باشد، چرا ممکن است تغییر نحوه Invocation آن `this` را تغییر ندهد؟

### پاسخ

زیرا Function حاصل از `bind()` یک Bound Function است و `this` آن از زمان Binding تعیین شده است. بنابراین Invocation معمولی بعدی آن Context تعیین‌شده را جایگزین نمی‌کند.

---

### سؤال ۱۸

چه زمانی استفاده بیش از حد از Explicit Binding می‌تواند نشانه طراحی نامناسب باشد؟

### پاسخ

وقتی Functionها بدون نیاز واقعی به `this` دائماً با `call()`، `apply()` یا `bind()` اجرا شوند. در چنین شرایطی ممکن است API یا طراحی Function بیش از حد به Context وابسته شده باشد و پیچیدگی غیرضروری ایجاد کند.

---

### سؤال ۱۹

چرا `call()` و `apply()` از نظر Binding تفاوت مفهومی ندارند؟

### پاسخ

زیرا هر دو مقدار `this` را به‌صورت صریح تعیین کرده و Function را بلافاصله اجرا می‌کنند. تنها تفاوت اصلی آن‌ها در نحوه انتقال Arguments است.

---

### سؤال ۲۰

چگونه `bind()` می‌تواند هم Explicit Binding و هم Partial Application را انجام دهد؟

### پاسخ

`bind()` می‌تواند `this` را به‌عنوان اولین مقدار تعیین کند و سپس یک یا چند Argument را نیز از قبل ثبت کند. Function جدید در زمان اجرا فقط Arguments باقی‌مانده را دریافت می‌کند.

---

# Golden Answers

## Explicit Binding چیست؟

Explicit Binding یعنی هنگام استفاده از Function، مقدار `this` را به‌صورت صریح تعیین کنیم. ابزارهای اصلی آن `call()`، `apply()` و `bind()` هستند.

---

## تفاوت `call()` و `apply()` چیست؟

هر دو Function را بلافاصله با `this` مشخص اجرا می‌کنند. تفاوت اصلی این است که `call()` Arguments را جداگانه و `apply()` آن‌ها را به‌صورت یک Collection دریافت می‌کند.

---

## تفاوت `call()` و `bind()` چیست؟

`call()` Function را فوراً اجرا می‌کند، اما `bind()` Function جدیدی ایجاد می‌کند که بعداً قابل اجرا است.

---

## `bind()` چه چیزی برمی‌گرداند؟

`bind()` یک Function جدید برمی‌گرداند که `this` آن از قبل تعیین شده است و می‌تواند Arguments مشخصی را نیز از قبل دریافت کرده باشد.

---

## Function Borrowing چیست؟

Function Borrowing یعنی استفاده از Logic یک Function برای Context دیگری بدون بازنویسی آن Function. Explicit Binding می‌تواند این کار را ممکن کند.

---

## Partial Application چیست؟

Partial Application یعنی بخشی از Arguments یک Function را از قبل مشخص کنیم و Function جدیدی بسازیم که فقط Arguments باقی‌مانده را دریافت کند.

---

## آیا `bind()` Function را اجرا می‌کند؟

خیر. `bind()` Function جدید ایجاد می‌کند؛ Invocation آن Function جدید باید بعداً انجام شود.

---

## چرا `bind()` برای Callbackها مفید است؟

زیرا می‌توان Context موردنظر را پیش از ارسال Function مشخص کرد و سپس Function جدید را برای اجرای بعدی در اختیار API یا سیستم دیگری قرار داد.

---

## قانون طلایی انتخاب متد چیست؟

اگر اجرای فوری می‌خواهیم از `call()` یا `apply()` استفاده می‌کنیم؛ اگر Function جدیدی برای اجرای بعدی می‌خواهیم، از `bind()` استفاده می‌کنیم.

---

# جمع‌بندی فصل

در این فصل یک مشکل مهم را بررسی کردیم:

> اگر Function به `this` وابسته باشد، چگونه می‌توانیم Context آن را به‌صورت صریح کنترل کنیم؟

پاسخ این سؤال **Explicit Function Binding** است.

سه ابزار اصلی این کار عبارت‌اند از:

```javascript
call()
apply()
bind()
```

`call()` و `apply()` هر دو Function را بلافاصله اجرا می‌کنند.

```javascript
fn.call(object, arg1, arg2);

fn.apply(object, [arg1, arg2]);
```

تفاوت آن‌ها در نحوه انتقال Arguments است.

در مقابل:

```javascript
const boundFn = fn.bind(object);
```

Function را اجرا نمی‌کند.

بلکه یک Function جدید ایجاد می‌کند که `this` آن از قبل مشخص شده است.

همچنین دیدیم که `bind()` می‌تواند بخشی از Arguments را نیز از قبل مشخص کند؛ قابلیتی که پایه **Partial Application** است.

در نهایت با **Function Borrowing** آشنا شدیم؛ الگویی که به ما اجازه می‌دهد Logic یک Function را برای Contextهای مختلف دوباره استفاده کنیم.

مدل ذهنی نهایی این فصل باید ساده باشد:

```text
call()
→ Bind + Invoke Now

apply()
→ Bind + Invoke Now
→ Arguments as Collection

bind()
→ Bind Now
→ Invoke Later
```

این تفاوت، مهم‌تر از حفظ کردن Syntax هر سه متد است.

---

# Conclusion

Explicit Binding یکی از ابزارهای مهم JavaScript برای کنترل رفتار Functionهایی است که به `this` وابسته‌اند.

هدف اصلی آن این نیست که هر Function را با `call()`، `apply()` یا `bind()` اجرا کنیم.

هدف، **کنترل آگاهانه Context در جایی است که Invocation معمولی پاسخگوی نیاز برنامه نیست.**

اگر Function باید همین حالا با Context مشخص اجرا شود، `call()` یا `apply()` انتخاب مناسب است.

اگر Function باید بعداً اجرا شود و می‌خواهیم Context آن از قبل مشخص باشد، `bind()` انتخاب مناسب‌تری است.

درک این تفاوت باعث می‌شود `call()`، `apply()` و `bind()` را به‌عنوان سه Syntax جداگانه حفظ نکنیم؛ بلکه آن‌ها را به‌عنوان سه ابزار برای یک مسئله مشترک ببینیم:

> **کنترل صریح Context و نحوه استفاده از یک Function.**
