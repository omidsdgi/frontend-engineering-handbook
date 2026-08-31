# Chapter 27 — Constructor Functions

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Constructor Function** را دقیقاً تعریف کنید.
* توضیح دهید چرا Constructor Function برای ایجاد Objectهای مشابه استفاده می‌شود.
* تفاوت یک Function معمولی و Constructor Function را از نظر **نحوه Invocation** تشخیص دهید.
* نقش Operator به نام `new` را در ایجاد Object جدید توضیح دهید.
* رفتار `this` را هنگام Constructor Invocation تحلیل کنید.
* مفهوم **Instance** را درک کنید.
* چند Instance مستقل از یک Constructor Function ایجاد کنید.
* توضیح دهید چرا قرار دادن Methodها داخل Constructor می‌تواند باعث تکرار غیرضروری Functionها شود.
* توضیح دهید چرا این مشکل ما را به سمت مفهوم **Prototype** هدایت می‌کند.
* Constructor Function را در کدهای JavaScript قدیمی‌تر و APIهای مبتنی بر Constructor تشخیص دهید.

---

# Core Question

> **چگونه قبل از ES6 Class Syntax، Objectهای مشابه را با Constructor Functions ایجاد می‌کردیم؟**

جریان این فصل:

```text
Object Creation
↓
Constructor Function
↓
Constructor Invocation
↓
new
↓
this
↓
Instance
↓
Multiple Instances
↓
Shared Behavior Problem
↓
Need for Prototypes
```

این فصل روی **Constructor Function و فرآیند ایجاد Instance با `new`** تمرکز دارد.

جزئیات Prototype، `prototype` Property، Prototype Chain و Inheritance در فصل بعد بررسی خواهند شد.

---

# مقدمه

در فصل 26 دیدیم که یکی از ایده‌های اصلی Object-Oriented Programming این است که Objectها می‌توانند:

* State داشته باشند.
* Behavior داشته باشند.
* Responsibility مشخصی داشته باشند.

فرض کنید در یک Application فروشگاهی چند User داریم.

هر User می‌تواند اطلاعاتی مانند:

```text
name
email
role
```

داشته باشد و رفتارهایی مانند:

```text
login()
logout()
```

ارائه کند.

اگر فقط یک User داشته باشیم، می‌توانیم از Object Literal استفاده کنیم:

```javascript
const user = {
  name: 'Omid',
  email: 'omid@example.com',
  role: 'admin'
};
```

اما اگر بخواهیم ده‌ها یا صدها User مشابه ایجاد کنیم، ساختن هر Object به‌صورت دستی مناسب نیست.

مثلاً:

```javascript
const user1 = {
  name: 'Omid',
  email: 'omid@example.com',
  role: 'admin'
};

const user2 = {
  name: 'Sara',
  email: 'sara@example.com',
  role: 'user'
};
```

در اینجا ساختار Objectها مشابه است.

چیزی که تغییر می‌کند، Valueهای مربوط به هر User است.

پس به یک الگوی قابل تکرار برای **ساخت Objectهای مشابه** نیاز داریم.

یکی از روش‌های مهم JavaScript برای این کار، **Constructor Function** است.

---

# Constructor Function

## چرا Constructor Function به وجود آمد؟

مسئله اصلی این نیست که چگونه یک Object بسازیم.

ما قبلاً این کار را با Object Literal انجام داده‌ایم.

مسئله این است:

> چگونه یک الگوی مشخص تعریف کنیم که بتواند Objectهای متعدد و مشابه ایجاد کند؟

برای مثال، به جای اینکه هر بار ساختار User را تکرار کنیم، می‌توانیم یک Function داشته باشیم که مسئول ایجاد User باشد.

```javascript
function User(name, email, role) {
  this.name = name;
  this.email = email;
  this.role = role;
}
```

اکنون یک الگوی ساخت User داریم.

بعداً می‌توانیم از آن برای ایجاد Objectهای مختلف استفاده کنیم.

```javascript
const user1 = new User(
  'Omid',
  'omid@example.com',
  'admin'
);

const user2 = new User(
  'Sara',
  'sara@example.com',
  'user'
);
```

در اینجا:

```text
User
```

الگوی ایجاد Object است.

و:

```text
user1
user2
```

Objectهایی هستند که از آن الگو ایجاد شده‌اند.

---

## تعریف Constructor Function

### تعریف ساده

**Constructor Function** یک Function است که برای ایجاد و مقداردهی اولیه Objectهای مشابه استفاده می‌شود و معمولاً با Operator `new` فراخوانی می‌شود.

به بیان ساده:

> Constructor Function یک الگوی قابل استفاده مجدد برای ساخت Objectهای مشابه است.

---

### تعریف فنی

Constructor Function یک Function است که وقتی با `new` فراخوانی می‌شود، فرآیند ایجاد یک Object جدید را آغاز می‌کند و داخل آن Constructor، `this` به Object جدید مربوط می‌شود.

برای مثال:

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}

const user = new User('Omid', 'omid@example.com');
```

در اینجا:

```text
User
```

Constructor Function است.

و:

```text
user
```

یک Instance ایجادشده از آن Constructor است.

---

# Constructor Function یک نوع Function جدید نیست

این نکته مهم است.

JavaScript Syntax خاصی به نام:

```javascript
constructor function
```

ندارد.

Constructor Function در اصل یک **Function معمولی** است که از آن با `new` برای ساخت Object استفاده می‌کنیم.

مثلاً:

```javascript
function User(name) {
  this.name = name;
}
```

از نظر Syntax، این همان Function معمولی است.

تفاوت اصلی در **نحوه استفاده** آن است:

```javascript
new User('Omid');
```

بنابراین بهتر است Constructor Function را یک **نقش برای یک Function** بدانیم، نه یک نوع مستقل از Function.

---

# یک Convention مهم

Constructor Functionها معمولاً با حرف بزرگ شروع می‌شوند:

```javascript
User
Product
Order
BankAccount
```

مثلاً:

```javascript
function User(name) {
  this.name = name;
}
```

این نام‌گذاری یک **Convention** است.

JavaScript به دلیل بزرگ بودن حرف اول، Function را Constructor نمی‌کند.

بلکه برنامه‌نویس با این Convention به خواننده اعلام می‌کند:

> این Function قرار است با `new` استفاده شود.

بنابراین:

```javascript
function createUser(name) {
  // ...
}
```

معمولاً نشان‌دهنده یک Function معمولی است.

در حالی که:

```javascript
function User(name) {
  // ...
}
```

معمولاً نشان‌دهنده Constructor Function است.

---

# Constructor Invocation

اکنون به بخش مهم‌تری می‌رسیم.

اگر Function را به‌صورت معمولی اجرا کنیم:

```javascript
User('Omid');
```

با:

```javascript
new User('Omid');
```

یکسان نیست.

`new` رفتار ویژه‌ای ایجاد می‌کند.

به همین دلیل باید Constructor Invocation را از Function Invocation معمولی جدا کنیم.

---

# `new` Operator

## چرا `new` لازم است؟

اگر Constructor Function فقط یک Function معمولی باشد، JavaScript از کجا بفهمد که:

> این Function باید برای ایجاد یک Object جدید استفاده شود؟

پاسخ:

```javascript
new
```

`new` به JavaScript اعلام می‌کند که این Function باید در قالب یک **Constructor Call** اجرا شود.

مثلاً:

```javascript
const user = new User('Omid', 'omid@example.com');
```

اینجا `new` مسئول شروع فرآیند ایجاد Instance است.

---

# مدل ذهنی `new`

برای درک `new` لازم نیست فعلاً وارد جزئیات داخلی Runtime شویم.

یک مدل ذهنی کاربردی این است:

```text
new User(...)
      ↓
Create a new Object
      ↓
Bind this to that Object
      ↓
Execute Constructor Function
      ↓
Initialize Properties
      ↓
Return the created Object
```

پس وقتی می‌نویسیم:

```javascript
const user = new User('Omid', 'omid@example.com');
```

می‌توانیم آن را مفهومی چنین ببینیم:

```text
Object جدید ساخته می‌شود
        ↓
this → Object جدید
        ↓
Constructor اجرا می‌شود
        ↓
Properties روی Object قرار می‌گیرند
        ↓
Object به user اختصاص داده می‌شود
```

---

# یک مثال کامل

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}

const user = new User(
  'Omid',
  'omid@example.com'
);

console.log(user.name);
console.log(user.email);
```

خروجی:

```text
Omid
omid@example.com
```

---

# `this` در Constructor Function

در فصل‌های قبل با مفهوم `this` آشنا شدیم.

در یک Constructor Call، رفتار `this` اهمیت ویژه‌ای پیدا می‌کند.

وقتی می‌نویسیم:

```javascript
const user = new User('Omid');
```

در زمان اجرای Constructor:

```javascript
function User(name) {
  this.name = name;
}
```

مقدار:

```javascript
this
```

به Instance جدید مربوط می‌شود.

بنابراین:

```javascript
this.name = name;
```

یعنی:

> Propertyای به نام `name` را روی Object جدید قرار بده.

---

# تحلیل مرحله‌به‌مرحله

کد:

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}

const user = new User(
  'Omid',
  'omid@example.com'
);
```

را مرحله‌ای بررسی کنیم.

### مرحله اول

Constructor فراخوانی می‌شود:

```javascript
new User(...)
```

### مرحله دوم

Object جدیدی برای Instance ایجاد می‌شود.

### مرحله سوم

`this` در Constructor به Object جدید مربوط می‌شود.

### مرحله چهارم

دستورات Constructor اجرا می‌شوند:

```javascript
this.name = name;
this.email = email;
```

در نتیجه Object جدید دارای Properties زیر می‌شود:

```javascript
{
  name: 'Omid',
  email: 'omid@example.com'
}
```

### مرحله پنجم

Object ایجادشده در نتیجه Expression قرار می‌گیرد:

```javascript
const user = ...
```

بنابراین:

```javascript
user
```

به Instance جدید اشاره می‌کند.

---

# Constructor Parameters

Constructor Function می‌تواند Parameters دریافت کند.

```javascript
function Product(title, price) {
  this.title = title;
  this.price = price;
}
```

هنگام ایجاد Instance:

```javascript
const laptop = new Product(
  'Laptop',
  1200
);
```

مقدارها وارد Constructor می‌شوند:

```text
title → "Laptop"
price → 1200
```

و سپس:

```javascript
this.title = title;
this.price = price;
```

روی Instance اعمال می‌شوند.

---

# Constructor برای Initialization است

یکی از بهترین مدل‌های ذهنی برای Constructor Function این است:

> Constructor وظیفه دارد Object جدید را در وضعیت اولیه مناسب قرار دهد.

مثلاً:

```javascript
function BankAccount(owner, balance) {
  this.owner = owner;
  this.balance = balance;
}
```

Constructor اطلاعات اولیه Account را تنظیم می‌کند.

```javascript
const account = new BankAccount(
  'Omid',
  5000
);
```

اکنون Instance دارای State اولیه است:

```text
owner   → Omid
balance → 5000
```

این همان مفهوم **Initialization** است.

---

# Instance

اکنون باید اصطلاح مهم دیگری را دقیق تعریف کنیم.

## Instance چیست؟

### تعریف ساده

**Instance** یک Object مشخص است که با استفاده از یک Constructor Function ایجاد شده است.

مثلاً:

```javascript
function User(name) {
  this.name = name;
}

const user1 = new User('Omid');
const user2 = new User('Sara');
```

در اینجا:

```text
user1 → Instance
user2 → Instance
```

و:

```text
User → Constructor Function
```

است.

---

## رابطه Constructor و Instance

مدل ذهنی:

```text
Constructor Function
        │
        ├──── new ────→ Instance 1
        │
        ├──── new ────→ Instance 2
        │
        └──── new ────→ Instance 3
```

Constructor یک Object مشخص نیست.

بلکه الگویی برای ایجاد Objectهای مشابه است.

Instance نیز خود الگو نیست.

بلکه یک Object واقعی و مستقل است که ایجاد شده است.

---

# Multiple Instances

یکی از مهم‌ترین مزایای Constructor Function این است که می‌توانیم از یک Constructor، Instanceهای متعددی ایجاد کنیم.

```javascript
function User(name, role) {
  this.name = name;
  this.role = role;
}

const admin = new User('Omid', 'admin');
const customer = new User('Sara', 'customer');
const manager = new User('Ali', 'manager');
```

اکنون سه Object مستقل داریم:

```text
admin
customer
manager
```

اما همه آن‌ها بر اساس یک الگوی مشترک ساخته شده‌اند:

```text
User
```

---

# Instanceها مستقل هستند

این استقلال بسیار مهم است.

```javascript
admin.name = 'Omid';
customer.name = 'Sara';
```

تغییر یکی از Instanceها دیگری را تغییر نمی‌دهد.

```javascript
console.log(admin.name);
console.log(customer.name);
```

خروجی:

```text
Omid
Sara
```

چرا؟

زیرا هر `new User(...)` یک Instance جدید ایجاد می‌کند.

---

# مدل ذهنی Multiple Instances

```text
User Constructor
       │
       ├──── new ───→ admin
       │
       ├──── new ───→ customer
       │
       └──── new ───→ manager
```

هر Instance State مخصوص خود را دارد.

مثلاً:

```text
admin
├── name: "Omid"
└── role: "admin"

customer
├── name: "Sara"
└── role: "customer"
```

Constructor فقط روش ایجاد این ساختار را تعریف می‌کند.

---

# Constructor و Object Literal

اکنون می‌توانیم دو رویکرد را مقایسه کنیم.

### Object Literal

```javascript
const user1 = {
  name: 'Omid',
  role: 'admin'
};

const user2 = {
  name: 'Sara',
  role: 'customer'
};
```

هر Object را باید به‌صورت جداگانه تعریف کنیم.

### Constructor Function

```javascript
function User(name, role) {
  this.name = name;
  this.role = role;
}

const user1 = new User('Omid', 'admin');
const user2 = new User('Sara', 'customer');
```

در اینجا ساختار ایجاد Object متمرکز شده است.

---

# چرا این تفاوت مهم است؟

اگر ساختار User تغییر کند، در رویکرد Constructor کافی است الگوی اصلی را تغییر دهیم.

مثلاً:

```javascript
function User(name, email, role) {
  this.name = name;
  this.email = email;
  this.role = role;
}
```

اکنون تمام Instanceهای جدید از همین ساختار استفاده می‌کنند.

این موضوع یکی از دلایل مهم استفاده از Constructor Function است:

> **Centralized Object Creation**

یعنی منطق ایجاد Object در یک نقطه مشخص قرار می‌گیرد.

---

# Constructor Function با Method

تا اینجا فقط State را ایجاد کردیم.

اما Object در OOP فقط State نیست.

می‌تواند Behavior نیز داشته باشد.

بنابراین ممکن است Constructor را این‌گونه بنویسیم:

```javascript
function BankAccount(owner, balance) {
  this.owner = owner;
  this.balance = balance;

  this.deposit = function (amount) {
    this.balance += amount;
  };
}
```

اکنون:

```javascript
const account = new BankAccount(
  'Omid',
  5000
);

account.deposit(1000);

console.log(account.balance);
```

خروجی:

```text
6000
```

در اینجا Instance هم State دارد:

```text
owner
balance
```

و هم Behavior:

```text
deposit()
```

---

# Shared Behavior Problem

در نگاه اول کد بالا مناسب به نظر می‌رسد.

اما اگر چند Account ایجاد کنیم:

```javascript
const account1 = new BankAccount('Omid', 5000);
const account2 = new BankAccount('Sara', 3000);
const account3 = new BankAccount('Ali', 7000);
```

هر بار Constructor اجرا می‌شود.

و در هر اجرای Constructor:

```javascript
this.deposit = function (amount) {
  this.balance += amount;
};
```

یک Function جدید ساخته می‌شود.

یعنی به‌صورت مفهومی:

```text
account1 → deposit Function A

account2 → deposit Function B

account3 → deposit Function C
```

در حالی که رفتار `deposit` در هر سه Account یکسان است.

---

# مشکل چیست؟

ما چند Function جداگانه داریم که منطق یکسانی را اجرا می‌کنند.

مثلاً:

```text
deposit A
deposit B
deposit C
```

همه در اصل همین کار را انجام می‌دهند:

```javascript
this.balance += amount;
```

اما برای هر Instance یک Function جدا ساخته شده است.

این می‌تواند در تعداد زیادی Instance باعث مصرف غیرضروری Memory شود.

---

# چرا Method را داخل Constructor قرار ندهیم؟

قرار دادن Propertyهای مربوط به State در Constructor کاملاً طبیعی است:

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}
```

چون هر Instance باید Valueهای مستقل خودش را داشته باشد.

اما برای Behavior مشترک، ایجاد Function جدید برای هر Instance همیشه بهینه نیست.

مثلاً:

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    console.log(`${this.name} logged in`);
  };
}
```

اگر 1000 User ایجاد کنیم، 1000 Function جداگانه برای `login` ایجاد خواهد شد.

در حالی که منطق آن‌ها یکسان است.

---

# یک نکته مهم

مشکل این نیست که Constructor Function نمی‌تواند Method داشته باشد.

می‌تواند.

مثلاً:

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    console.log(`${this.name} logged in`);
  };
}
```

از نظر عملکردی کاملاً معتبر است.

مشکل زمانی مطرح می‌شود که:

> تعداد زیادی Instance داریم و Behaviorهای مشترک را برای هر Instance دوباره ایجاد می‌کنیم.

پس مسئله اصلی **Shared Behavior** است.

---

# چرا به Prototype نیاز پیدا می‌کنیم؟

اکنون به آخرین مرحله Concept Flow رسیده‌ایم.

ما یک Constructor Function داریم:

```javascript
function User(name) {
  this.name = name;
}
```

و می‌توانیم Instanceهای متعدد ایجاد کنیم:

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

اما اگر بخواهیم یک Method مشترک داشته باشیم، قرار دادن آن داخل Constructor باعث ایجاد Function جداگانه برای هر Instance می‌شود:

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    console.log(`${this.name} logged in`);
  };
}
```

ما به راهی نیاز داریم که:

```text
user1
user2
user3
...
```

بتوانند یک Behavior مشترک را استفاده کنند، بدون اینکه برای هر Instance یک Function جدید ساخته شود.

این نیاز، ما را به مفهوم:

**Prototype**

می‌رساند.

---

# Prototype فقط یک Preview

در این فصل وارد سازوکار Prototype نمی‌شویم.

فقط باید بدانیم:

> JavaScript سازوکاری دارد که اجازه می‌دهد Behavior مشترک بین Objectهای مختلف قرار گیرد و Instanceها بتوانند از آن استفاده کنند.

این سازوکار **Prototype** نام دارد.

در فصل بعد بررسی خواهیم کرد:

* Prototype چیست.
* رابطه Object با Prototype چیست.
* `prototype` Property چیست.
* چگونه Method مشترک ایجاد می‌شود.
* Property Lookup چگونه کار می‌کند.
* Prototype Chain چیست.

بنابراین در این مرحله کافی است مشکل را بشناسیم:

```text
Multiple Instances
        ↓
Shared Behavior
        ↓
Repeated Functions
        ↓
Unnecessary Duplication
        ↓
Need for Shared Behavior
        ↓
Prototype
```

این دقیقاً نقطه‌ای است که Constructor Functions به مفهوم Prototype متصل می‌شوند.

---

# Constructor Function و Prototype یک مفهوم نیستند

یک سوءبرداشت رایج این است که:

> Constructor Function همان Prototype است.

خیر.

این دو نقش متفاوت دارند.

### Constructor Function

مسئول ایجاد و Initialization Instance است.

```javascript
function User(name) {
  this.name = name;
}
```

### Prototype

مکانیزمی برای اشتراک Behavior و Properties میان Objectها فراهم می‌کند.

جزئیات آن در فصل بعد بررسی می‌شود.

پس:

```text
Constructor Function
        ↓
Creates / Initializes Instances

Prototype
        ↓
Enables Shared Behavior
```

---

# Constructor Function و `new`

برای استفاده صحیح از Constructor Function باید ارتباط این سه مفهوم را به خاطر بسپاریم:

```text
Constructor Function
        ↓
new
        ↓
Instance
```

مثلاً:

```javascript
function Product(title, price) {
  this.title = title;
  this.price = price;
}

const laptop = new Product(
  'Laptop',
  1200
);
```

در این مثال:

```text
Product → Constructor Function
new     → Constructor Invocation
laptop  → Instance
```

و در طول اجرای Constructor:

```javascript
this
```

به Instance جدید مربوط می‌شود.

---

# اگر `new` را حذف کنیم چه می‌شود؟

این یکی از مهم‌ترین اشتباهات است.

```javascript
function User(name) {
  this.name = name;
}

const user = User('Omid');
```

اینجا Constructor با `new` فراخوانی نشده است.

بنابراین این کد دیگر یک Constructor Call نیست.

در نتیجه:

```javascript
this
```

رفتار Constructor-specific خود را نخواهد داشت.

در JavaScript مدرن و به‌خصوص در Strict Mode، چنین استفاده‌ای می‌تواند باعث خطا شود.

پس Constructor Functionی که برای استفاده با `new` طراحی شده است، باید به‌صورت صحیح فراخوانی شود:

```javascript
const user = new User('Omid');
```

---

# یک Constructor Function باید با `new` استفاده شود

Convention حرف اول بزرگ:

```javascript
function User(name) {
  this.name = name;
}
```

به برنامه‌نویس می‌گوید:

```text
Use with new
```

بنابراین این:

```javascript
new User('Omid');
```

صحیح است.

اما این:

```javascript
User('Omid');
```

استفاده صحیح از Constructor Function نیست.

---

# Constructor Return Behavior

یک نکته فنی مهم دیگر درباره Constructor Call وجود دارد.

در حالت معمول، Constructor Object جدیدی را که با `new` ساخته شده است به نتیجه Expression تبدیل می‌کند.

مثلاً:

```javascript
function User(name) {
  this.name = name;
}

const user = new User('Omid');
```

در اینجا:

```javascript
user
```

به Object جدید اشاره می‌کند.

Constructor لازم نیست بنویسد:

```javascript
return this;
```

یعنی:

```javascript
function User(name) {
  this.name = name;
}
```

کافی است.

---

# آیا Constructor می‌تواند `return` داشته باشد؟

بله، اما رفتار `return` در Constructor Call قواعد خاصی دارد.

در این مرحله برای مدل ذهنی اصلی کافی است بدانیم که Constructorهای معمولی باید روی Initialization Object تمرکز کنند و معمولاً چیزی را به‌صورت صریح Return نمی‌کنند.

مثلاً:

```javascript
function User(name) {
  this.name = name;
}
```

الگوی استاندارد و خوانایی است.

جزئیات رفتار `return` در Constructor Call موضوعی تخصصی‌تر است و برای استفاده معمول Constructor Function ضروری نیست.

---

# یک مثال واقعی: Order

فرض کنید یک فروشگاه آنلاین داریم.

هر Order دارای:

```text
id
customer
total
status
```

است.

می‌توانیم Constructor ایجاد کنیم:

```javascript
function Order(id, customer, total) {
  this.id = id;
  this.customer = customer;
  this.total = total;
  this.status = 'pending';
}
```

اکنون:

```javascript
const order1 = new Order(
  101,
  'Omid',
  250
);

const order2 = new Order(
  102,
  'Sara',
  450
);
```

هر دو Order از یک الگو ایجاد شده‌اند.

اما State آن‌ها مستقل است.

```javascript
console.log(order1.status);
console.log(order2.status);
```

خروجی:

```text
pending
pending
```

اگر:

```javascript
order1.status = 'completed';
```

را اجرا کنیم:

```javascript
console.log(order1.status);
console.log(order2.status);
```

خروجی:

```text
completed
pending
```

زیرا هر Instance State مستقل خود را دارد.

---

# یک مثال واقعی: Product

```javascript
function Product(title, price) {
  this.title = title;
  this.price = price;
}

const laptop = new Product('Laptop', 1200);
const mouse = new Product('Mouse', 50);
const keyboard = new Product('Keyboard', 100);
```

اکنون سه Product داریم:

```text
laptop
mouse
keyboard
```

اما همه آن‌ها با یک Constructor Function ایجاد شده‌اند.

---

# Constructor Function به‌عنوان Object Factory

از یک دید مهندسی می‌توان Constructor Function را یک روش برای **Object Creation** در نظر گرفت.

```text
Input
 ↓
Constructor
 ↓
Initialization
 ↓
Instance
```

برای مثال:

```javascript
new Product('Laptop', 1200);
```

ورودی:

```text
Laptop
1200
```

و خروجی:

```text
Product Instance
```

این دیدگاه کمک می‌کند Constructor را صرفاً به‌عنوان یک Syntax خاص نبینیم.

Constructor بخشی از طراحی Object Creation است.

---

# Constructor Function در کدهای قدیمی‌تر JavaScript

پیش از معرفی `class` در ES6، Constructor Function یکی از الگوهای اصلی برای پیاده‌سازی Object-Oriented Programming در JavaScript بود.

برای مثال:

```javascript
function User(name) {
  this.name = name;
}

const user = new User('Omid');
```

بعدها `class` Syntax معرفی شد:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}
```

اما مقایسه کامل این دو Syntax در فصل **ES Classes** انجام خواهد شد.

در این فصل فقط باید بدانیم که Constructor Functions یکی از روش‌های مهم تاریخی و همچنان قابل مشاهده برای ساخت Objectهای مشابه در JavaScript هستند.

---

# Best Practices

## 1. نام Constructor را با حرف بزرگ شروع کنید

```javascript
function User(name) {
  this.name = name;
}
```

این یک Convention رایج است و مشخص می‌کند Function برای Constructor Call طراحی شده است.

---

## 2. State مربوط به Instance را داخل Constructor مقداردهی کنید

```javascript
function Product(title, price) {
  this.title = title;
  this.price = price;
}
```

هر Instance Valueهای مستقل خود را دریافت می‌کند.

---

## 3. Constructor را ساده نگه دارید

Constructor بهتر است عمدتاً مسئول Initialization باشد.

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}
```

از قرار دادن Logic پیچیده و غیرمرتبط با Initialization در Constructor خودداری کنید.

---

## 4. Constructor را با `new` فراخوانی کنید

```javascript
const user = new User('Omid');
```

نه:

```javascript
const user = User('Omid');
```

---

## 5. Behaviorهای مشترک را آگاهانه طراحی کنید

اگر تعداد زیادی Instance دارید، ایجاد Functionهای مشابه داخل هر Constructor می‌تواند باعث تکرار غیرضروری شود.

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    // ...
  };
}
```

در چنین شرایطی باید به مکانیزم Shared Behavior یعنی Prototype توجه کنیم که در فصل بعد بررسی می‌شود.

---

# Common Mistakes

## اشتباه اول: تصور اینکه Constructor Function نوع خاصی از Function است

❌

> Constructor Function یک نوع جداگانه از Function است.

✔

Constructor Function یک Function معمولی است که برای Constructor Call، معمولاً با `new`، استفاده می‌شود.

---

## اشتباه دوم: تصور اینکه نام Function آن را Constructor می‌کند

❌

```javascript
function User() {}
```

فقط به دلیل نام `User` یک Constructor نیست.

✔

این Function زمانی به‌عنوان Constructor استفاده می‌شود که با Constructor Call مناسب، معمولاً:

```javascript
new User();
```

فراخوانی شود.

---

## اشتباه سوم: فراموش کردن `new`

❌

```javascript
const user = User('Omid');
```

✔

```javascript
const user = new User('Omid');
```

`new` رفتار مخصوص Constructor Invocation را فعال می‌کند.

---

## اشتباه چهارم: یکی دانستن Constructor و Instance

Constructor:

```javascript
User
```

الگوی ایجاد Object است.

Instance:

```javascript
const user = new User('Omid');
```

Object مشخصی است که ایجاد شده است.

---

## اشتباه پنجم: تصور اینکه `this` در Constructor همیشه به Global Object اشاره می‌کند

در Constructor Call:

```javascript
new User('Omid');
```

`this` به Instance جدید مربوط می‌شود.

---

## اشتباه ششم: تصور اینکه همه Instanceها State مشترک دارند

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

Stateهای Instance معمولاً مستقل هستند.

تغییر:

```javascript
user1.name = 'Ali';
```

باعث تغییر:

```javascript
user2.name
```

نمی‌شود.

---

## اشتباه هفتم: ایجاد Methodهای مشترک داخل هر Instance بدون توجه به هزینه آن

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    // ...
  };
}
```

این کد معتبر است.

اما هر Instance یک Function جداگانه دریافت می‌کند.

در تعداد زیادی Instance، این طراحی می‌تواند باعث تکرار غیرضروری شود.

---

## اشتباه هشتم: تصور اینکه Constructor خودش Prototype است

Constructor Function و Prototype دو مفهوم متفاوت هستند.

Constructor برای Object Creation و Initialization استفاده می‌شود.

Prototype مکانیزمی برای Shared Behavior فراهم می‌کند.

---

## اشتباه نهم: ورود زودهنگام به Prototype

در این فصل لازم نیست جزئیات زیر را یاد بگیریم:

```javascript
User.prototype
```

یا:

```javascript
Object.getPrototypeOf(...)
```

این مفاهیم در فصل بعد به‌صورت مستقل بررسی خواهند شد.

---

# Summary

تا اینجا مسئله اصلی این فصل را از Object Creation شروع کردیم.

وقتی فقط یک Object داریم، Object Literal کافی است:

```javascript
const user = {
  name: 'Omid'
};
```

اما وقتی تعداد زیادی Object مشابه داریم، تکرار ساختار Objectها مناسب نیست.

Constructor Function یک Function معمولی است که برای ایجاد Objectهای مشابه، معمولاً با `new`، استفاده می‌شود.

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}
```

با:

```javascript
const user = new User(
  'Omid',
  'omid@example.com'
);
```

یک Constructor Call انجام می‌شود.

Operator `new` فرآیند ایجاد Instance را آغاز می‌کند.

در زمان اجرای Constructor:

```javascript
this
```

به Instance جدید مربوط می‌شود.

بنابراین:

```javascript
this.name = name;
```

Property مربوط به Instance را مقداردهی می‌کند.

از یک Constructor Function می‌توان Instanceهای متعددی ایجاد کرد:

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

هر Instance State مستقل خود را دارد.

اما وقتی Behaviorهای مشترک را مستقیماً داخل Constructor قرار می‌دهیم، برای هر Instance یک Function جدید ایجاد می‌شود.

این موضوع در تعداد زیادی Instance می‌تواند باعث تکرار غیرضروری Behavior شود.

در نتیجه به مکانیزمی برای **Shared Behavior** نیاز پیدا می‌کنیم.

این نیاز ما را به مفهوم **Prototype** هدایت می‌کند.

Prototype و Prototype Chain در فصل بعد بررسی خواهند شد.

---

# Key Takeaways

* Constructor Function یک Function معمولی است که برای ایجاد Objectهای مشابه استفاده می‌شود.
* Constructor Function معمولاً با `new` فراخوانی می‌شود.
* `new` یک Constructor Call ایجاد می‌کند.
* Constructor Call فرآیند ایجاد یک Object جدید را آغاز می‌کند.
* `this` در Constructor Call به Instance جدید مربوط می‌شود.
* Constructor معمولاً مسئول Initialization اولیه Instance است.
* Parameterهای Constructor برای دریافت داده‌های اولیه استفاده می‌شوند.
* Instance یک Object مشخص است که با Constructor Function ایجاد شده است.
* از یک Constructor Function می‌توان Instanceهای متعدد ایجاد کرد.
* هر Instance State مستقل خود را دارد.
* Constructor Function یک نوع مستقل از Function نیست.
* بزرگ بودن حرف اول نام Constructor یک Convention است، نه یک قانون زبان.
* حذف `new` باعث می‌شود Function دیگر در قالب Constructor Call اجرا نشود.
* Object Creation یکی از کاربردهای اصلی Constructor Function است.
* Constructor Function می‌تواند State و Behavior ایجاد کند.
* قرار دادن Method داخل Constructor باعث ایجاد یک Function جداگانه برای هر Instance می‌شود.
* تکرار Behavior مشترک می‌تواند در تعداد زیادی Instance هزینه اضافی ایجاد کند.
* این مشکل نیاز به مکانیزمی برای Shared Behavior را ایجاد می‌کند.
* Prototype راهکاری است که JavaScript برای Shared Behavior فراهم می‌کند.
* جزئیات Prototype و Prototype Chain در فصل بعد بررسی می‌شوند.
* Constructor Function و Prototype دو مفهوم متفاوت هستند.

---

# Technical Interview

## سطح Junior

### 1. Constructor Function چیست؟

Constructor Function یک Function است که معمولاً با `new` فراخوانی می‌شود و برای ایجاد و مقداردهی اولیه Objectهای مشابه استفاده می‌شود.

```javascript
function User(name) {
  this.name = name;
}

const user = new User('Omid');
```

---

### 2. آیا Constructor Function یک نوع خاص از Function است؟

خیر.

Constructor Function در اصل یک Function معمولی است که در یک Constructor Call، معمولاً با `new`، استفاده می‌شود.

---

### 3. `new` چه کاری انجام می‌دهد؟

`new` یک Constructor Call ایجاد می‌کند. در این فرآیند یک Object جدید ایجاد می‌شود، `this` به آن Object مربوط می‌شود، Constructor اجرا می‌شود و Object ایجادشده به‌عنوان نتیجه در اختیار برنامه قرار می‌گیرد.

---

### 4. `this` در Constructor Function به چه چیزی اشاره می‌کند؟

وقتی Function با `new` فراخوانی شود، `this` در طول اجرای Constructor به Instance جدید مربوط می‌شود.

---

### 5. Instance چیست؟

Instance یک Object مشخص است که با استفاده از Constructor Function ایجاد شده است.

```javascript
const user = new User('Omid');
```

در اینجا `user` یک Instance از `User` است.

---

### 6. چرا از Constructor Function استفاده می‌کنیم؟

برای اینکه بتوانیم Objectهای متعدد و مشابه را بر اساس یک الگوی مشترک ایجاد و مقداردهی اولیه کنیم.

---

### 7. آیا Instanceهای مختلف State مشترک دارند؟

خیر. Propertiesای که با `this` در Constructor مقداردهی می‌شوند، متعلق به Instance مربوطه هستند.

---

### 8. چرا نام Constructor Function معمولاً با حرف بزرگ شروع می‌شود؟

این یک Convention است تا به برنامه‌نویس اعلام کند Function برای استفاده با `new` طراحی شده است.

---

## سطح Mid-Level

### 9. تفاوت Function Invocation و Constructor Invocation چیست؟

در Function Invocation معمولی:

```javascript
User('Omid');
```

Function به‌صورت معمولی اجرا می‌شود.

در Constructor Invocation:

```javascript
new User('Omid');
```

`new` فرآیند ایجاد Instance را فعال می‌کند و `this` به Object جدید مربوط می‌شود.

---

### 10. این کد چه مشکلی دارد؟

```javascript
function User(name) {
  this.name = name;
}

const user = User('Omid');
```

Function با `new` فراخوانی نشده است؛ بنابراین Constructor Call اتفاق نیفتاده است.

برای استفاده به‌عنوان Constructor باید بنویسیم:

```javascript
const user = new User('Omid');
```

---

### 11. چرا Constructor Function برای ایجاد چند Object مناسب است؟

زیرا منطق ایجاد و Initialization در یک نقطه متمرکز می‌شود:

```javascript
function Product(title, price) {
  this.title = title;
  this.price = price;
}
```

سپس می‌توان Instanceهای متعدد ایجاد کرد:

```javascript
const laptop = new Product('Laptop', 1200);
const mouse = new Product('Mouse', 50);
```

در نتیجه ساختار ایجاد Object تکرار نمی‌شود.

---

### 12. چرا قرار دادن Method داخل Constructor می‌تواند مشکل‌ساز باشد؟

مثلاً:

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    console.log(`${this.name} logged in`);
  };
}
```

هر بار که `new User()` اجرا می‌شود، یک Function جدید برای `login` ایجاد می‌شود.

اگر تعداد Instanceها زیاد باشد، این Behavior مشترک به‌صورت تکراری ایجاد می‌شود.

---

### 13. آیا قرار دادن Method داخل Constructor اشتباه است؟

خیر.

از نظر زبان کاملاً معتبر است.

مسئله این است که اگر Behavior برای تمام Instanceها مشترک باشد، ایجاد یک Function جداگانه برای هر Instance می‌تواند طراحی غیرضروری و پرهزینه‌ای باشد.

این مسئله ما را به سمت Prototype هدایت می‌کند.

---

### 14. رابطه Constructor Function و Instance چیست؟

Constructor Function الگوی ایجاد و Initialization است.

Instance یک Object مشخص است که از آن الگو ایجاد شده است.

```text
Constructor
    ↓ new
Instance
```

---

### 15. آیا هر بار استفاده از `new` یک Instance جدید ایجاد می‌کند؟

بله، در یک Constructor Call معمولی هر اجرای:

```javascript
new User(...)
```

یک Object جدید را ایجاد و مقداردهی اولیه می‌کند.

بنابراین:

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

دو Instance مستقل ایجاد می‌کند.

---

### 16. چرا Constructor Function را یک Object Factory می‌دانیم؟

زیرا یک الگوی تکرارپذیر برای ایجاد Objectهای مشابه فراهم می‌کند:

```text
Input
↓
Constructor
↓
Initialization
↓
Instance
```

---

## سطح Senior

### 17. از دید مهندسی، مهم‌ترین محدودیت Constructor Function چیست؟

Constructor Function ایجاد و Initialization Object را به‌خوبی حل می‌کند، اما اگر Behaviorهای مشترک را مستقیماً روی `this` تعریف کنیم، برای هر Instance Functionهای جداگانه ساخته می‌شوند.

مثلاً:

```javascript
function User(name) {
  this.name = name;

  this.login = function () {
    // shared behavior
  };
}
```

در تعداد زیاد Instanceها این موضوع باعث تکرار Behavior و مصرف بیشتر Memory می‌شود.

این محدودیت نیاز به Shared Behavior را مطرح می‌کند که در JavaScript با Prototypeها حل می‌شود.

---

### 18. چرا Constructor Function را نباید با Prototype یکی دانست؟

این دو مسئولیت متفاوت دارند.

Constructor Function روی **Object Creation و Initialization** تمرکز دارد.

Prototype مکانیزمی برای **Shared Behavior و Property Lookup** فراهم می‌کند.

رابطه آن‌ها در JavaScript مهم است، اما یکی نیستند.

---

### 19. چرا `new` فقط یک Syntax برای کوتاه‌تر نوشتن Object Creation نیست؟

زیرا `new` یک Constructor Call ایجاد می‌کند و مجموعه‌ای از رفتارهای مشخص را فعال می‌کند.

مدل ذهنی آن:

```text
new Constructor(...)
        ↓
Create Instance
        ↓
Bind this
        ↓
Execute Constructor
        ↓
Return Instance
```

بنابراین `new` بخشی از Semantics مربوط به Constructor Invocation است، نه صرفاً Syntax.

---

### 20. اگر یک Constructor Function با `new` فراخوانی نشود، چه اتفاقی می‌افتد؟

دیگر Constructor Invocation اتفاق نمی‌افتد.

در نتیجه رفتار `this` نیز مانند Constructor Call نخواهد بود.

برای مثال:

```javascript
function User(name) {
  this.name = name;
}

User('Omid');
```

این Function Call معمولی است، نه Constructor Call.

رفتار دقیق `this` به Mode اجرای کد نیز وابسته است و در Strict Mode می‌تواند باعث خطا شود.

---

### 21. مهم‌ترین دلیل طراحی Prototype در کنار Constructor Function چیست؟

Constructor Function به ما اجازه می‌دهد Instanceهای متعدد ایجاد کنیم.

اما اگر Behavior مشترک را داخل Constructor تعریف کنیم، هر Instance Function مخصوص خود را دریافت می‌کند.

پس مسئله به این شکل تبدیل می‌شود:

```text
Many Instances
      ↓
Same Behavior
      ↓
Repeated Functions
      ↓
Unnecessary Duplication
      ↓
Shared Behavior Mechanism
      ↓
Prototype
```

Prototype این مشکل را با فراهم کردن مکانیزمی برای اشتراک Behavior میان Objectها حل می‌کند.

جزئیات این مکانیزم موضوع فصل بعد است.

---

### 22. اگر Constructor Function برای ایجاد Objectهای مشابه است، چرا Object Literal کافی نیست؟

Object Literal برای ایجاد یک Object مشخص بسیار مناسب است:

```javascript
const user = {
  name: 'Omid'
};
```

اما اگر تعداد زیادی Object با ساختار مشابه داشته باشیم، تکرار ساختار Objectها مناسب نیست.

Constructor Function اجازه می‌دهد الگوی Object Creation یک بار تعریف شود و بارها مورد استفاده قرار گیرد.

---

# Golden Answers

## Junior — Golden Answer

> **Constructor Function چیست؟**

Constructor Function یک Function معمولی است که معمولاً با `new` فراخوانی می‌شود و برای ایجاد و مقداردهی اولیه Objectهای مشابه استفاده می‌شود. در زمان Constructor Call، `this` به Instance جدید مربوط می‌شود.

مثال:

```javascript
function User(name) {
  this.name = name;
}

const user = new User('Omid');
```

---

## Mid-Level — Golden Answer

> **`new` در Constructor Function چه کاری انجام می‌دهد؟**

`new` یک Constructor Call ایجاد می‌کند. در این فرآیند Object جدیدی ایجاد می‌شود، `this` در Constructor به آن Object مربوط می‌شود، Constructor برای Initialization اجرا می‌شود و Object ایجادشده به‌عنوان نتیجه در اختیار برنامه قرار می‌گیرد.

به‌صورت مفهومی:

```text
new
↓
New Object
↓
this Binding
↓
Constructor Execution
↓
Instance
```

---

## Senior — Golden Answer

> **محدودیت اصلی Constructor Function چیست و چرا Prototype مطرح می‌شود؟**

Constructor Function برای ایجاد و Initialization چندین Instance مناسب است، اما اگر Behavior مشترک را مستقیماً روی `this` تعریف کنیم، هر Constructor Call یک Function جدید ایجاد می‌کند. بنابراین با افزایش تعداد Instanceها، Behaviorهای یکسان به‌صورت تکراری ایجاد می‌شوند.

برای جلوگیری از این duplication به مکانیزمی برای Shared Behavior نیاز داریم. JavaScript این مسئله را با Prototypeها حل می‌کند؛ یعنی Behavior مشترک می‌تواند به‌جای قرار گرفتن روی هر Instance، در یک مکان مشترک قرار گیرد و Objectها از آن استفاده کنند.

جزئیات Prototype و Property Lookup در فصل بعد بررسی می‌شود.

---

# Conclusion

Constructor Function یکی از پاسخ‌های مهم JavaScript به مسئله **Object Creation** است.

مسئله از جایی شروع می‌شود که Application به تعداد زیادی Object مشابه نیاز دارد.

به‌جای تعریف دستی هر Object، یک Constructor Function ایجاد می‌کنیم:

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}
```

سپس با `new` می‌توانیم Instanceهای متعدد ایجاد کنیم:

```javascript
const user1 = new User(
  'Omid',
  'omid@example.com'
);

const user2 = new User(
  'Sara',
  'sara@example.com'
);
```

در Constructor Call، `this` به Instance جدید مربوط می‌شود و Constructor وظیفه Initialization آن را انجام می‌دهد.

بنابراین:

```text
Constructor Function
        ↓
new
        ↓
this
        ↓
Instance
```

این مدل برای ایجاد Objectهای متعدد بسیار مفید است.

اما وقتی Behavior مشترک را داخل Constructor قرار می‌دهیم، برای هر Instance یک Function جدا ایجاد می‌شود:

```text
Instance 1 → Method A
Instance 2 → Method B
Instance 3 → Method C
```

در حالی که Behavior هر سه یکسان است.

این مسئله ما را به یک سؤال طبیعی می‌رساند:

> چگونه می‌توانیم Behavior را بین Instanceهای مختلف به اشتراک بگذاریم؟

پاسخ این سؤال **Prototype** است.

و این دقیقاً نقطه شروع فصل بعد خواهد بود.

```text
Constructor Function
        ↓
Multiple Instances
        ↓
Shared Behavior Problem
        ↓
Need for Prototypes
```

در فصل بعد بررسی خواهیم کرد که Prototype چیست، چگونه با Constructor Function ارتباط دارد و JavaScript چگونه از Prototypeها برای Shared Behavior و Property Lookup استفاده می‌کند.
