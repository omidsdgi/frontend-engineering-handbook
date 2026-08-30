# Chapter 26 — OOP Fundamentals

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Object-Oriented Programming** را به‌صورت دقیق توضیح دهید.
* تفاوت **Programming Paradigm** و Syntax را درک کنید.
* توضیح دهید چرا OOP برای مدیریت Complexity در Software استفاده می‌شود.
* مفهوم **Object**، **Class** و **Instance** را از یکدیگر تشخیص دهید.
* نقش **Properties** و **Methods** را در یک Object توضیح دهید.
* مفهوم **Encapsulation** و **Abstraction** را در طراحی Object درک کنید.
* مفهوم **Inheritance** و **Polymorphism** را در سطح بنیادی توضیح دهید.
* جایگاه OOP را در JavaScript به‌عنوان یک زبان **Multi-Paradigm** درک کنید.
* تفاوت میان Object-Oriented Thinking و صرفاً استفاده از `class` را تشخیص دهید.
* برای یک مسئله ساده، Objects و Responsibilities مناسب را شناسایی کنید.
* پاسخ دقیق و مهندسی به پرسش‌های رایج مصاحبه درباره OOP ارائه دهید.

---

# Core Question

> **Object-Oriented Programming چگونه پیچیدگی نرم‌افزار را با Objects و Responsibilities مدیریت می‌کند؟**

---

# مقدمه

تا اینجا در بخش Objects یاد گرفتیم که Object می‌تواند داده و رفتار مرتبط را در یک ساختار واحد نگهداری کند.

برای مثال:

```javascript
const account = {
  owner: 'Omid',
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

در این Object:

```text
Data
↓
owner
balance

Behavior
↓
deposit()
```

این مدل برای بسیاری از مسائل ساده کافی است.

اما با بزرگ‌تر شدن Application، تعداد Objectها، رفتارها و ارتباط میان آن‌ها افزایش پیدا می‌کند.

فرض کنید یک سیستم فروشگاهی داریم که باید با این موارد کار کند:

```text
Customer
Product
Cart
Order
Payment
Shipping
```

اکنون سؤال فقط این نیست که:

> چگونه یک Object بسازیم؟

سؤال مهم‌تر این است:

> چگونه ساختار Application را طوری طراحی کنیم که هر بخش مسئولیت مشخصی داشته باشد و Complexity قابل کنترل باقی بماند؟

اینجا **Object-Oriented Programming** اهمیت پیدا می‌کند.

OOP بیش از آنکه مجموعه‌ای از Syntaxها باشد، یک روش برای **Thinking About Software** است.

در این روش، Software را می‌توان مجموعه‌ای از Objects در نظر گرفت که:

* State دارند.
* Behavior دارند.
* Responsibility دارند.
* با Objects دیگر تعامل می‌کنند.

بنابراین برای درک OOP نباید از `class` شروع کنیم.

ابتدا باید بفهمیم:

> **Object-Oriented Thinking چیست و چرا به آن نیاز داریم؟**

---

# Programming Paradigms

قبل از OOP باید مفهوم **Programming Paradigm** را بشناسیم.

Programming Paradigm یک شیوه کلی برای سازمان‌دهی و تفکر درباره Program است.

Paradigm به ما می‌گوید:

> چگونه مسئله را مدل کنیم و اجزای Program را سازمان‌دهی کنیم؟

برای مثال، در یک رویکرد Procedural ممکن است مسئله را بیشتر به شکل مجموعه‌ای از عملیات ببینیم:

```text
Input
↓
Process
↓
Process
↓
Output
```

در رویکرد Object-Oriented، تمرکز بیشتری روی Entityها و مسئولیت‌های آن‌ها قرار می‌گیرد:

```text
Customer
Product
Order
Payment
```

هر کدام State و Behavior مربوط به خود را دارند.

نکته مهم این است که Programming Paradigm به این معنا نیست که یک زبان فقط یک روش برنامه‌نویسی دارد.

JavaScript یک زبان **Multi-Paradigm** است.

یعنی می‌توان در آن از رویکردهای مختلف استفاده کرد.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

این کد بدون نیاز به Object-Oriented Design نیز قابل استفاده است.

در مقابل:

```javascript
const order = {
  price: 100,
  quantity: 2,

  getTotal() {
    return this.price * this.quantity;
  }
};
```

در اینجا Data و Behavior مرتبط در یک Object قرار گرفته‌اند.

بنابراین OOP یک اجبار در JavaScript نیست.

بلکه یک ابزار طراحی است که باید در جایی استفاده شود که برای مسئله مناسب باشد.

---

# چرا OOP؟

با افزایش اندازه Application، Complexity افزایش پیدا می‌کند.

Complexity فقط به تعداد خطوط Code مربوط نیست.

ارتباط میان بخش‌های مختلف نیز Complexity ایجاد می‌کند.

برای مثال، فرض کنید یک Order در Application باید:

* اطلاعات خود را نگهداری کند.
* Total را محاسبه کند.
* وضعیت خود را مدیریت کند.
* با بخش Payment ارتباط داشته باشد.

اگر مسئولیت‌های مختلف بدون ساختار مشخص در سراسر Application پخش شوند، تغییر یک بخش می‌تواند بخش‌های دیگر را نیز تحت تأثیر قرار دهد.

OOP تلاش می‌کند این Complexity را با سازمان‌دهی Software حول Objects و Responsibilities قابل کنترل‌تر کند.

مدل ذهنی ساده:

```text
Problem
↓
Identify Entities
↓
Define State
↓
Define Behavior
↓
Assign Responsibilities
↓
Objects
```

بنابراین هدف OOP صرفاً استفاده از `class` نیست.

هدف اصلی:

> **سازمان‌دهی Complexity با تقسیم مسئولیت‌ها میان Objects است.**

---

# Object-Oriented Thinking

Object-Oriented Thinking یعنی قبل از نوشتن Code، مسئله را از دید Objects و Responsibilities بررسی کنیم.

فرض کنید می‌خواهیم یک سیستم فروشگاه طراحی کنیم.

به‌جای اینکه از Functionهای پراکنده شروع کنیم، می‌توانیم ابتدا Entityهای اصلی را شناسایی کنیم:

```text
Customer
Product
Cart
Order
```

سپس برای هر Object سؤال می‌کنیم:

> چه اطلاعاتی باید داشته باشد؟

و:

> چه مسئولیتی باید انجام دهد؟

برای مثال:

```text
Product
├── name
├── price
└── stock
```

و:

```text
Cart
├── items
└── addItem()
```

در اینجا هر Object مسئولیت مشخصی دارد.

این شیوه تفکر به ما کمک می‌کند قبل از انتخاب Syntax، ساختار مسئله را مشخص کنیم.

---

# Object

در فصل‌های قبلی Object را به‌عنوان ساختاری برای نگهداری Propertyها و Methodها بررسی کردیم.

در OOP، همین مفهوم اهمیت بیشتری پیدا می‌کند.

Object می‌تواند نماینده یک Entity یا یک مفهوم در Application باشد.

مثلاً:

```javascript
const user = {
  name: 'Omid',
  email: 'omid@example.com',

  updateEmail(newEmail) {
    this.email = newEmail;
  }
};
```

این Object دارای:

```text
State
↓
name
email

Behavior
↓
updateEmail()
```

است.

---

## State چیست؟

State مجموعه اطلاعاتی است که وضعیت فعلی Object را توصیف می‌کند.

در مثال بالا:

```javascript
name
email
```

بخشی از State هستند.

اگر Email تغییر کند:

```javascript
user.updateEmail('new@example.com');
```

State Object تغییر کرده است.

---

## Behavior چیست؟

Behavior مشخص می‌کند Object چه کاری می‌تواند انجام دهد.

در مثال بالا:

```javascript
updateEmail()
```

یک Behavior است.

بنابراین یک مدل ذهنی مفید برای Object این است:

```text
Object
├── State
└── Behavior
```

---

# Properties و Methods

در JavaScript، State معمولاً با Propertyها و Behavior معمولاً با Methodها نمایش داده می‌شود.

مثلاً:

```javascript
const cart = {
  items: [],

  addItem(product) {
    this.items.push(product);
  }
};
```

در اینجا:

```javascript
items
```

State را نگهداری می‌کند.

و:

```javascript
addItem()
```

Behavior مربوط به Cart است.

این ارتباط در OOP اهمیت زیادی دارد:

> Object فقط داده نیست؛ می‌تواند مسئول انجام Behavior مرتبط با همان داده نیز باشد.

---

# Class

وقتی تعداد Objectهای مشابه افزایش پیدا می‌کند، ایجاد و سازمان‌دهی آن‌ها به‌صورت جداگانه دشوار می‌شود.

فرض کنید Application ما هزار Customer دارد.

نمی‌خواهیم ساختار هر Customer را از ابتدا به‌صورت دستی طراحی کنیم.

اینجا مفهوم **Class** مطرح می‌شود.

### تعریف ساده

Class یک الگو برای تعریف ساختار و Behavior مشترک Objectهای مشابه است.

به بیان ساده:

> Class مشخص می‌کند Instanceهای یک نوع Object چه State و Behaviorهایی داشته باشند.

برای مثال، می‌توانیم مفهوم Customer را به‌صورت یک Class مدل کنیم:

```javascript
class Customer {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  updateEmail(newEmail) {
    this.email = newEmail;
  }
}
```

در اینجا `Customer` یک Class است.

Class مشخص می‌کند Customerها دارای:

```text
name
email
updateEmail()
```

باشند.

نکته مهم این فصل این است که Class را به‌عنوان یک **Modeling Tool** درک کنیم.

جزئیات نحوه کار `class`، `constructor`، `new`، Prototype و Methodها در فصل‌های بعدی بررسی خواهند شد.

---

# Instance

اگر Class الگوی Object باشد، **Instance** یک Object واقعی ساخته‌شده بر اساس آن Class است.

مثلاً:

```javascript
const customer1 = new Customer('Omid', 'omid@example.com');
const customer2 = new Customer('Sara', 'sara@example.com');
```

در اینجا:

```text
Customer
   ↓
Class

customer1
   ↓
Instance

customer2
   ↓
Instance
```

هر دو Object از یک Class ساخته شده‌اند.

اما State آن‌ها مستقل است.

```javascript
customer1.name = 'Omid';
customer2.name = 'Sara';
```

تغییر State یک Instance نباید State Instance دیگر را تغییر دهد.

---

# Class و Instance

تفاوت اصلی را می‌توان این‌گونه دید:

| مفهوم    | نقش                                  |
| -------- | ------------------------------------ |
| Class    | الگوی تعریف Objectهای مشابه          |
| Instance | Object واقعی ساخته‌شده بر اساس Class |
| Property | بخشی از State                        |
| Method   | بخشی از Behavior                     |

مدل ذهنی:

```text
Class
  ↓
Defines Structure + Behavior
  ↓
Instance
  ↓
Actual Object
```

---

# چرا Class به وجود می‌آید؟

فرض کنید بدون یک الگوی مشترک، چند Customer ایجاد کنیم:

```javascript
const customer1 = {
  name: 'Omid',
  email: 'omid@example.com'
};

const customer2 = {
  name: 'Sara',
  email: 'sara@example.com'
};
```

در تعداد کم مشکلی ایجاد نمی‌کند.

اما در Application واقعی، Objectهای مشابه زیاد هستند.

Class اجازه می‌دهد ساختار مشترک را یک‌بار تعریف کنیم و از آن برای ایجاد Instanceهای متعدد استفاده کنیم.

این موضوع باعث:

* Consistency
* Reusability
* Organization

می‌شود.

اما Class به‌تنهایی Design خوب را تضمین نمی‌کند.

یک Class بزرگ و دارای مسئولیت‌های متعدد می‌تواند Complexity را بیشتر کند.

بنابراین مسئله اصلی همچنان **Responsibility** است.

---

# Encapsulation

اکنون به یکی از اصول اصلی OOP می‌رسیم:

**Encapsulation**

فرض کنید یک Account داریم:

```javascript
const account = {
  balance: 1000
};
```

اگر هر بخش Application بتواند مستقیماً مقدار `balance` را تغییر دهد:

```javascript
account.balance = -500000;
```

کنترل State دشوار می‌شود.

در طراحی بهتر، باید مشخص کنیم:

> چه چیزی از بیرون قابل دسترسی است و چه چیزی باید تحت کنترل Object باشد؟

این ایده پایه Encapsulation است.

---

## تعریف ساده

Encapsulation یعنی جمع‌کردن State و Behavior مرتبط در یک واحد و کنترل نحوه دسترسی و تغییر State.

به بیان ساده:

> Object باید مسئول مدیریت State خودش باشد.

---

## مثال

به‌جای اینکه هر کدی بتواند Balance را مستقیماً تغییر دهد:

```javascript
account.balance = -500;
```

می‌توانیم Behavior مشخصی برای تغییر آن داشته باشیم:

```javascript
account.deposit(500);
```

در این حالت Object مسئول کنترل عملیات است.

مدل ذهنی:

```text
External Code
     ↓
Public Behavior
     ↓
Object State
```

نه:

```text
External Code
     ↓
Direct State Manipulation
```

---

## چرا Encapsulation مهم است؟

فرض کنید قانون Application این است:

> موجودی حساب نباید منفی شود.

اگر State آزادانه قابل تغییر باشد، هر بخش Application باید این قانون را رعایت کند.

اما اگر Object مسئول کنترل State باشد، منطق مربوط به آن در یک مکان متمرکز می‌شود.

این باعث کاهش احتمال Inconsistent State می‌شود.

---

# Information Hiding

یکی از ایده‌های مرتبط با Encapsulation، **Information Hiding** است.

Information Hiding یعنی جزئیات داخلی Implementation تا حد امکان از مصرف‌کننده Object پنهان بماند.

مثلاً مصرف‌کننده ممکن است فقط بداند:

```javascript
account.deposit(500);
```

اما لازم نباشد بداند Deposit دقیقاً چگونه محاسبه یا ثبت می‌شود.

بنابراین:

```text
Public Interface
       ↓
     Object
       ↓
Internal Details
```

این جداسازی باعث می‌شود Implementation داخلی بتواند تغییر کند، بدون اینکه تمام کدهای استفاده‌کننده مجبور به تغییر شوند.

جزئیات Private Fields، Private Methods و کنترل دقیق Access در فصل‌های بعدی OOP بررسی خواهند شد.

---

# Abstraction

Encapsulation ما را به مفهوم دیگری می‌رساند:

**Abstraction**

### تعریف ساده

Abstraction یعنی نمایش بخش‌های مهم یک سیستم و پنهان کردن جزئیات غیرضروری از مصرف‌کننده.

به بیان ساده:

> مصرف‌کننده باید بداند چگونه از یک قابلیت استفاده کند، بدون اینکه مجبور باشد تمام جزئیات داخلی آن را بداند.

---

## مثال واقعی

فرض کنید در یک Application:

```javascript
payment.process();
```

را داریم.

کدی که این Method را استفاده می‌کند ممکن است لازم نباشد بداند:

```text
Validation
↓
Payment Provider
↓
Request
↓
Response Handling
↓
Internal State
```

چگونه انجام می‌شوند.

مصرف‌کننده فقط با Interface موردنیاز کار می‌کند:

```javascript
payment.process();
```

این Abstraction است.

---

# Encapsulation و Abstraction

این دو مفهوم نزدیک هستند، اما یکسان نیستند.

**Encapsulation** بیشتر درباره سازمان‌دهی و کنترل State و Behavior و محدود کردن دسترسی به جزئیات داخلی است.

**Abstraction** بیشتر درباره ارائه Interface ضروری و پنهان کردن Complexity غیرضروری از مصرف‌کننده است.

می‌توانیم این تفاوت را این‌گونه ببینیم:

```text
Encapsulation
↓
Control Internal State and Access
```

در مقابل:

```text
Abstraction
↓
Expose What Consumer Needs
Hide Unnecessary Complexity
```

این دو در طراحی OOP معمولاً با یکدیگر همکاری می‌کنند.

---

# Inheritance

تا اینجا Objects مستقل را بررسی کردیم.

اما گاهی چند Object یا Model دارای Behavior مشترک هستند.

فرض کنید Application ما چند نوع User دارد:

```text
User
├── Admin
└── Customer
```

ممکن است Admin و Customer هر دو اطلاعات و رفتارهای مشترکی داشته باشند.

برای مثال:

```text
User
├── name
├── email
└── login()
```

و هر نوع User رفتارهای خاص خود را نیز داشته باشد.

Inheritance یکی از روش‌های مدل‌کردن چنین رابطه‌ای است.

---

## تعریف ساده

Inheritance یعنی ایجاد یک نوع جدید بر اساس Behavior و ویژگی‌های نوع موجود.

به بیان ساده:

> نوع جدید می‌تواند بخش‌هایی از مدل موجود را به ارث ببرد و Behavior خاص خود را اضافه یا تغییر دهد.

مدل مفهومی:

```text
Parent
  ↓
Shared Behavior
  ↓
Child
  ↓
Specialized Behavior
```

در این مرحله فقط مفهوم Inheritance را بررسی می‌کنیم.

Syntaxهایی مانند:

```javascript
extends
```

و:

```javascript
super
```

در فصل **Inheritance and Polymorphism** به‌صورت مستقل آموزش داده خواهند شد.

---

# چرا Inheritance؟

فرض کنید دو نوع Object رفتار مشترکی دارند:

```text
Admin
Customer
```

اگر Behavior مشترک را در هر دو به‌صورت مستقل تکرار کنیم، Duplication ایجاد می‌شود.

Inheritance می‌تواند برای مدل‌کردن رابطه‌ای مانند:

```text
Admin is a User
Customer is a User
```

استفاده شود.

اما این نکته بسیار مهم است:

> هر شباهتی به معنای وجود رابطه Inheritance نیست.

اگر دو Object فقط از یک قابلیت مشترک استفاده می‌کنند، ممکن است مدل دیگری مناسب‌تر باشد.

انتخاب میان Inheritance و Composition در فصل‌های بعدی با جزئیات بیشتری بررسی خواهد شد.

---

# Polymorphism

Inheritance ما را به مفهوم مهم دیگری می‌رساند:

**Polymorphism**

واژه Polymorphism به معنای «چندشکلی» است.

در OOP، ایده اصلی این است که یک Interface یا رفتار مشترک می‌تواند توسط Objectهای مختلف با Implementations متفاوت ارائه شود.

فرض کنید چند نوع Notification داریم:

```text
EmailNotification
SMSNotification
PushNotification
```

همه آن‌ها ممکن است مفهوم مشترکی داشته باشند:

```text
send()
```

اما نحوه انجام `send()` در هر نوع متفاوت است.

مدل مفهومی:

```text
Notification
    ↓
   send()
    ↓
 ┌───────────────┐
 ↓       ↓       ↓
Email   SMS     Push
```

مصرف‌کننده می‌تواند با مفهوم مشترک کار کند، در حالی که هر Object Behavior مناسب خود را ارائه می‌دهد.

---

## مثال مفهومی

فرض کنید Application یک Function دارد که Notification را ارسال می‌کند:

```javascript
function sendNotification(notification) {
  notification.send();
}
```

اکنون Objectهای مختلف می‌توانند Behavior متفاوتی داشته باشند:

```text
EmailNotification → send() → Email
SMSNotification   → send() → SMS
PushNotification  → send() → Push
```

Function اصلی لازم نیست برای هر نوع Notification منطق جداگانه‌ای داشته باشد.

این ایده، هسته Polymorphism است:

> **یک Interface مشترک، چند Behavior متفاوت.**

---

# رابطه Inheritance و Polymorphism

این دو مفهوم ارتباط نزدیکی دارند، اما یکی نیستند.

Inheritance درباره **رابطه و Reuse میان Typeها** است.

Polymorphism درباره **استفاده از Interface یا قرارداد مشترک برای رفتارهای متفاوت** است.

مدل ساده:

```text
Inheritance
↓
Shared Structure / Behavior
```

و:

```text
Polymorphism
↓
Common Interface
↓
Different Implementations
```

Polymorphism می‌تواند از طریق Inheritance پیاده‌سازی شود، اما ایده Polymorphism به یک Syntax یا یک روش خاص محدود نیست.

---

# Object-Oriented Design

اکنون می‌توانیم تمام مفاهیم اصلی را کنار یکدیگر قرار دهیم.

فرض کنید یک سیستم سفارش داریم.

ابتدا Entityها را شناسایی می‌کنیم:

```text
Customer
Product
Order
```

سپس State و Behavior را مشخص می‌کنیم.

### Customer

```text
State
↓
name
email

Behavior
↓
updateProfile()
```

### Product

```text
State
↓
name
price
stock

Behavior
↓
updateStock()
```

### Order

```text
State
↓
items
status

Behavior
↓
calculateTotal()
submit()
```

اکنون Object-Oriented Thinking به ما کمک می‌کند Responsibilityها را نیز مشخص کنیم.

برای مثال:

> چه Objectی باید Total سفارش را محاسبه کند؟

اگر پاسخ منطقی `Order` باشد، این Behavior در `Order` قرار می‌گیرد.

```text
Order
 ├── items
 ├── status
 └── calculateTotal()
```

به‌جای اینکه هر بخش Application خودش منطق محاسبه Order را دوباره پیاده‌سازی کند.

---

# Responsibility

یکی از مهم‌ترین مفاهیم مهندسی در OOP، **Responsibility** است.

Object باید مسئولیت‌هایی داشته باشد که به State و نقش آن در سیستم مرتبط هستند.

مثلاً:

```text
Order
↓
Order-related behavior
```

و:

```text
Payment
↓
Payment-related behavior
```

اگر یک `Order` مسئول:

```text
Payment
Email
Shipping
Database
UI
Logging
```

نیز باشد، Class یا Object به‌شدت پیچیده و Coupled می‌شود.

پس OOP فقط درباره ساختن Objectهای بیشتر نیست.

هدف:

> **قرار دادن Behavior در جای مناسب و مشخص کردن Responsibility هر Object است.**

---

# OOP در JavaScript

JavaScript را نباید یک زبان صرفاً Object-Oriented بدانیم.

JavaScript یک زبان **Multi-Paradigm** است.

می‌توان در آن از:

* Procedural Programming
* Functional Programming
* Object-Oriented Programming

و ترکیب‌هایی از آن‌ها استفاده کرد.

بنابراین این دو دیدگاه اشتباه هستند:

> «برای JavaScript حتماً باید همه چیز را با Class بنویسیم.»

و:

> «JavaScript چون Prototype-Based است، پس OOP نیست.»

هر دو برداشت نادرست هستند.

JavaScript امکانات لازم برای طراحی Object-Oriented را فراهم می‌کند.

اما OOP در JavaScript دقیقاً همان مدل Class-Based زبان‌هایی مانند Java یا C++ نیست.

JavaScript از نظر مدل Object، **Prototype-Based** است.

---

# JavaScript و Prototype-Based Nature

در JavaScript، Objectها در نهایت بر اساس رابطه‌های Prototype با یکدیگر رفتار و Properties را به اشتراک می‌گذارند.

این موضوع پایه Mechanism مربوط به Object Delegation در JavaScript است.

اما در JavaScript مدرن می‌توان از:

```javascript
class User {
  // ...
}
```

نیز استفاده کرد.

نکته مهم این است که `class` را نباید با یک مدل کاملاً مستقل از Prototypeها اشتباه گرفت.

برای این فصل کافی است این مدل ذهنی را داشته باشیم:

```text
JavaScript
    ↓
Multi-Paradigm
    ↓
Supports OOP
    ↓
Object Model is Prototype-Based
    ↓
class provides a convenient syntax
```

جزئیات Prototype، Prototype Chain و رابطه آن‌ها با `class` در فصل‌های بعدی بررسی خواهند شد.

---

# Class-Based Thinking vs Object-Oriented Thinking

این دو را نباید یکی بدانیم.

ممکن است برنامه‌ای از تعداد زیادی `class` تشکیل شده باشد، اما Design خوبی نداشته باشد.

مثلاً:

```text
HugeApplicationClass
├── Users
├── Products
├── Payments
├── Emails
├── Reports
└── Database
```

استفاده از `class` به‌تنهایی این Design را خوب نمی‌کند.

OOP باید به ما کمک کند:

* مسئولیت‌ها را جدا کنیم.
* State را سازمان‌دهی کنیم.
* Behavior را در جای مناسب قرار دهیم.
* وابستگی‌های غیرضروری را کاهش دهیم.
* Complexity را مدیریت کنیم.

بنابراین:

> **OOP یک Design Approach است، نه صرفاً استفاده از `class`.**

---

# Composition vs Inheritance

در طراحی Object-Oriented گاهی می‌توان یک Behavior را با Inheritance به اشتراک گذاشت.

اما همیشه Inheritance بهترین انتخاب نیست.

فرض کنید یک Object به چند قابلیت نیاز دارد:

```text
Logging
Caching
Validation
```

ممکن است بهتر باشد این قابلیت‌ها به‌صورت اجزای مستقل طراحی شوند و Object آن‌ها را Composition کند.

مدل مفهومی:

```text
Object
├── Logging capability
├── Caching capability
└── Validation capability
```

در مقابل Inheritance بیشتر یک رابطه Type است:

```text
Admin
  is a
User
```

این تفاوت در طراحی مهم است.

در این فصل فقط باید این اصل را به‌عنوان یک مدل ذهنی اولیه بدانیم:

> Inheritance برای رابطه‌های واقعی Type و اشتراک Behavior مناسب است؛ Composition اغلب برای ترکیب قابلیت‌های مستقل انعطاف بیشتری دارد.

جزئیات Design Trade-offهای Inheritance و Composition در فصل‌های آینده بررسی خواهد شد.

---

# یک مدل ذهنی یکپارچه از OOP

اکنون می‌توان مسیر اصلی OOP را به‌صورت زیر خلاصه کرد:

```text
Programming Paradigm
        ↓
Object-Oriented Thinking
        ↓
Identify Objects
        ↓
Define State + Behavior
        ↓
Assign Responsibilities
        ↓
Class
        ↓
Instances
        ↓
Encapsulation
        ↓
Abstraction
        ↓
Inheritance
        ↓
Polymorphism
```

این زنجیره را نباید صرفاً به‌صورت فهرستی از اصطلاحات حفظ کرد.

هر مفهوم پاسخی به یک مسئله طراحی است.

```text
Object
→ What exists?

Class
→ What structure do similar Objects share?

Instance
→ What is the actual Object?

Encapsulation
→ Who controls the State?

Abstraction
→ What does the consumer need to know?

Inheritance
→ What behavior is shared through a Type relationship?

Polymorphism
→ How can different Objects provide different Behavior through a common interface?
```

این مدل ذهنی، پایه ورود به مباحث فنی‌تر OOP در فصل‌های بعدی است.

---

# Best Practices

## 1. ابتدا Responsibility را مشخص کنید

قبل از ایجاد Class بپرسید:

> این Object دقیقاً مسئول چه کاری است؟

Class بدون Responsibility مشخص معمولاً به یک Container بزرگ برای کد تبدیل می‌شود.

---

## 2. Object را صرفاً برای داده ایجاد نکنید

اگر Behavior مشخصی به State مربوط است، قرار دادن آن Behavior در Object می‌تواند Design را واضح‌تر کند.

---

## 3. از Class برای هر چیزی استفاده نکنید

JavaScript Multi-Paradigm است.

یک Function ساده گاهی از یک Class مناسب‌تر است.

مثلاً:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

برای چنین منطق ساده‌ای الزاماً نیازی به Class وجود ندارد.

---

## 4. Encapsulation را جدی بگیرید

State نباید بدون دلیل از بخش‌های مختلف Application قابل تغییر باشد.

Behavior مناسب می‌تواند کنترل تغییر State را در یک مکان متمرکز کند.

---

## 5. Abstraction را با پنهان‌کردن همه چیز اشتباه نگیرید

هدف Abstraction این نیست که هیچ جزئیاتی در دسترس نباشد.

هدف این است که Consumer فقط با جزئیات ضروری تعامل کند.

---

## 6. Inheritance را فقط به دلیل شباهت استفاده نکنید

دو Object ممکن است Behavior مشترک داشته باشند، بدون اینکه رابطه منطقی Parent/Child میان آن‌ها وجود داشته باشد.

---

## 7. Composition را در ذهن داشته باشید

اگر هدف شما فقط ترکیب چند قابلیت مستقل است، Composition ممکن است انتخاب مناسب‌تری باشد.

---

## 8. Class بزرگ نسازید

اگر یک Class مسئولیت‌های متعدد و نامرتبط دارد، احتمالاً باید Responsibilities آن دوباره بررسی شوند.

---

# اشتباهات رایج

## 1. OOP را مساوی `class` دانستن

OOP یک رویکرد طراحی است.

`class` فقط یکی از Syntaxها و ابزارهای موجود برای پیاده‌سازی برخی مدل‌های Object-Oriented در JavaScript است.

---

## 2. تصور اینکه JavaScript فقط Object-Oriented است

JavaScript یک زبان Multi-Paradigm است.

استفاده از OOP به مسئله و Design بستگی دارد.

---

## 3. تصور اینکه Object فقط Data است

Object می‌تواند هم State و هم Behavior داشته باشد.

---

## 4. یکی دانستن Class و Instance

Class الگو است.

Instance یک Object واقعی ساخته‌شده بر اساس آن الگو است.

---

## 5. یکی دانستن Encapsulation و Abstraction

این دو مرتبط هستند اما یک مفهوم نیستند.

Encapsulation بیشتر روی کنترل State و Access تمرکز دارد.

Abstraction روی ارائه Interface ضروری و پنهان‌کردن Complexity غیرضروری تمرکز دارد.

---

## 6. استفاده از Inheritance برای هر نوع Reuse

Reuse به‌تنهایی دلیل کافی برای Inheritance نیست.

ابتدا باید بررسی شود آیا رابطه واقعی Type وجود دارد یا خیر.

---

## 7. تصور اینکه Inheritance همیشه بهتر از Composition است

هیچ‌کدام همیشه بهتر نیستند.

انتخاب به Relationship و Design مسئله بستگی دارد.

---

## 8. ایجاد Classهای بسیار بزرگ

اگر یک Class مسئولیت‌های متعدد و نامرتبط داشته باشد، Complexity و Coupling افزایش پیدا می‌کند.

---

## 9. انتقال مستقیم تمام Data به بیرون

اگر State بدون کنترل در اختیار تمام بخش‌های Application باشد، حفظ Invariants دشوارتر می‌شود.

---

## 10. تمرکز روی Syntax به‌جای Design

دانستن:

```javascript
class
```

به‌تنهایی به معنای دانستن OOP نیست.

مهم‌تر از Syntax این است که بدانیم:

> چه Objectهایی داریم و هر کدام چه Responsibilityای دارند؟

---

# Summary

Object-Oriented Programming یک روش برای سازمان‌دهی Software حول Objects و Responsibilities است.

OOP از یک Programming Paradigm شروع می‌شود، اما هدف آن صرفاً انتخاب Syntax خاصی نیست.

در Object-Oriented Thinking، ابتدا مسئله را به Entityها و Responsibilityهای آن‌ها تقسیم می‌کنیم.

Object می‌تواند:

* State داشته باشد.
* Behavior داشته باشد.
* مسئولیت مشخصی داشته باشد.
* با Objectهای دیگر تعامل کند.

Class الگویی برای تعریف ساختار و Behavior مشترک Objectهای مشابه است.

Instance یک Object واقعی است که بر اساس آن Class ایجاد شده است.

Encapsulation به سازمان‌دهی State و Behavior و کنترل دسترسی به جزئیات داخلی کمک می‌کند.

Abstraction به ما اجازه می‌دهد Interface موردنیاز را ارائه کنیم و Complexity غیرضروری را از Consumer پنهان کنیم.

Inheritance امکان مدل‌کردن رابطه میان Typeها و اشتراک Behavior را فراهم می‌کند.

Polymorphism اجازه می‌دهد Objectهای مختلف از طریق یک Interface یا قرارداد مشترک، Behavior متفاوت ارائه دهند.

JavaScript یک زبان Multi-Paradigm است و OOP یکی از رویکردهای قابل استفاده در آن است.

همچنین Object Model در JavaScript بر پایه Prototypeهاست و `class` یک Syntax مدرن برای کار با این مدل ارائه می‌کند. جزئیات Prototype و Class در فصل‌های بعدی بررسی می‌شوند.

مهم‌ترین نکته این فصل این است:

> **OOP درباره ساختن Classهای بیشتر نیست؛ درباره سازمان‌دهی Complexity با Objects، State، Behavior و Responsibilities است.**

---

# Key Takeaways

* **Programming Paradigm** یک روش کلی برای سازمان‌دهی و تفکر درباره Program است.
* JavaScript یک زبان **Multi-Paradigm** است.
* OOP یکی از رویکردهای قابل استفاده در JavaScript است.
* Object می‌تواند State و Behavior را در یک واحد سازمان‌دهی کند.
* Property معمولاً بخشی از State Object را نشان می‌دهد.
* Method معمولاً Behavior مرتبط با Object را نشان می‌دهد.
* **Class** الگویی برای تعریف ساختار و Behavior مشترک Objectهای مشابه است.
* **Instance** یک Object واقعی ساخته‌شده بر اساس Class است.
* Class و Instance یک مفهوم نیستند.
* OOP باید از Object-Oriented Thinking شروع شود، نه از Syntax.
* Object-Oriented Thinking بر شناسایی Objects و Responsibilities تمرکز دارد.
* **Encapsulation** به سازمان‌دهی و کنترل State و Behavior کمک می‌کند.
* **Information Hiding** جزئیات داخلی Implementation را از Consumer پنهان می‌کند.
* **Abstraction** Interface ضروری را ارائه و Complexity غیرضروری را پنهان می‌کند.
* Encapsulation و Abstraction مرتبط‌اند اما یکسان نیستند.
* **Inheritance** برای مدل‌کردن رابطه میان Typeها و اشتراک Behavior استفاده می‌شود.
* هر نوع Code Reuse الزاماً به Inheritance نیاز ندارد.
* **Polymorphism** امکان ارائه Behaviorهای متفاوت از طریق یک Interface یا قرارداد مشترک را فراهم می‌کند.
* Inheritance و Polymorphism مفاهیم مرتبط اما متفاوت هستند.
* JavaScript از نظر Object Model، Prototype-Based است.
* `class` را نباید با کل مفهوم OOP یکسان دانست.
* OOP یک Design Approach است، نه صرفاً استفاده از `class`.
* Responsibility باید پیش از طراحی Class مشخص شود.
* Classهای بزرگ و دارای مسئولیت‌های متعدد معمولاً نشانه Design نامناسب هستند.
* Composition در برخی طراحی‌ها می‌تواند نسبت به Inheritance مناسب‌تر باشد.
* انتخاب OOP باید بر اساس مسئله و Complexity سیستم انجام شود.

---

# Technical Interview

## Junior

### 1. OOP چیست؟

OOP یک Programming Paradigm است که Software را حول Objects، State، Behavior و Responsibilities سازمان‌دهی می‌کند.

---

### 2. چرا از OOP استفاده می‌کنیم؟

برای مدیریت Complexity، سازمان‌دهی State و Behavior و تقسیم Responsibility میان اجزای مختلف Software.

---

### 3. Object چیست؟

Object ساختاری است که می‌تواند State و Behavior مرتبط را در یک واحد نگهداری کند.

---

### 4. تفاوت Property و Method چیست؟

Property معمولاً Data یا State Object را نگهداری می‌کند، در حالی که Method Behavior مرتبط با Object را تعریف می‌کند.

---

### 5. Class چیست؟

Class الگویی برای تعریف ساختار و Behavior مشترک Objectهای مشابه است.

---

### 6. Instance چیست؟

Instance یک Object واقعی است که بر اساس یک Class ساخته شده است.

---

### 7. تفاوت Class و Instance چیست؟

Class الگو یا تعریف مشترک است؛ Instance نمونه واقعی ساخته‌شده بر اساس آن تعریف است.

---

### 8. Encapsulation چیست؟

Encapsulation یعنی سازمان‌دهی State و Behavior مرتبط در یک واحد و کنترل نحوه دسترسی و تغییر State.

---

### 9. Abstraction چیست؟

Abstraction یعنی ارائه Interface ضروری به Consumer و پنهان‌کردن جزئیات غیرضروری Implementation.

---

### 10. Inheritance چیست؟

Inheritance روشی برای ایجاد یک Type بر اساس Type موجود و استفاده مجدد از ویژگی‌ها یا Behaviorهای مشترک است.

---

### 11. Polymorphism چیست؟

Polymorphism یعنی Objectهای مختلف بتوانند از یک Interface یا قرارداد مشترک، Behaviorهای متفاوت ارائه دهند.

---

### 12. آیا JavaScript یک زبان Object-Oriented است؟

JavaScript یک زبان Multi-Paradigm است و از OOP پشتیبانی می‌کند، اما به OOP محدود نیست.

---

## Mid-Level

### 13. چرا OOP را نباید مساوی Class دانست؟

زیرا OOP یک رویکرد طراحی است که بر Objects، Responsibilities، State، Behavior و تعامل میان آن‌ها تمرکز دارد. `class` فقط یکی از ابزارهای Syntax در JavaScript برای بیان مدل‌های Object-Oriented است.

---

### 14. Object-Oriented Thinking چیست؟

روشی برای تحلیل مسئله که در آن ابتدا Entityها، State، Behavior و Responsibilityهای آن‌ها شناسایی می‌شوند و سپس ساختار Code بر اساس این مدل طراحی می‌شود.

---

### 15. چرا Responsibility در OOP مهم است؟

زیرا هر Object باید مسئولیت مشخص و مرتبطی داشته باشد. توزیع مناسب Responsibility باعث کاهش Complexity، Coupling و Duplication می‌شود.

---

### 16. تفاوت Encapsulation و Abstraction چیست؟

Encapsulation بیشتر روی سازمان‌دهی و کنترل State و Access تمرکز دارد؛ Abstraction روی ارائه Interface ضروری و پنهان‌کردن Complexity غیرضروری تمرکز می‌کند.

---

### 17. آیا Inheritance همیشه بهترین روش برای Reuse است؟

خیر. اگر رابطه واقعی Type میان Objects وجود نداشته باشد، Composition ممکن است انتخاب مناسب‌تری باشد.

---

### 18. Composition و Inheritance چه تفاوت مفهومی دارند؟

Inheritance بیشتر رابطه‌ای از نوع:

```text
is-a
```

را مدل می‌کند.

Composition بیشتر برای ترکیب قابلیت‌ها یا اجزای مستقل در یک Object استفاده می‌شود.

---

### 19. چرا Class بزرگ می‌تواند مشکل‌ساز باشد؟

زیرا مسئولیت‌های متعدد و نامرتبط را در یک واحد قرار می‌دهد و باعث افزایش Complexity و Coupling می‌شود.

---

### 20. آیا هر Object باید Class داشته باشد؟

خیر. JavaScript اجازه می‌دهد از Object Literals، Functions و سایر الگوها نیز استفاده کنیم. انتخاب ابزار باید بر اساس مسئله باشد.

---

### 21. آیا JavaScript یک Class-Based Language است؟

خیر. Object Model جاوااسکریپت Prototype-Based است. `class` یک Syntax مدرن برای کار با Objectها و مدل‌های Object-Oriented ارائه می‌کند.

---

## Senior

### 22. OOP چگونه Complexity را مدیریت می‌کند؟

OOP Complexity را با تقسیم System به Objects دارای State، Behavior و Responsibilities مشخص مدیریت می‌کند. هر Object بخشی از Complexity را مالک می‌شود و از طریق Interfaceهای مشخص با سایر Objects تعامل می‌کند.

---

### 23. آیا استفاده از OOP به‌تنهایی باعث Maintainability می‌شود؟

خیر. OOP صرفاً یک ابزار طراحی است. اگر Responsibilities به‌درستی تقسیم نشوند یا Classها بیش از حد بزرگ و Coupled باشند، استفاده از OOP حتی می‌تواند Complexity را افزایش دهد.

---

### 24. رابطه Encapsulation و Maintainability چیست؟

Encapsulation باعث می‌شود جزئیات داخلی و تغییرات State در محدوده مشخصی کنترل شوند. در نتیجه تغییر Implementation داخلی می‌تواند با Impact کمتری روی Consumerها انجام شود.

---

### 25. Abstraction چه نقشی در کاهش Complexity دارد؟

Abstraction Complexity داخلی را پشت یک Interface ساده‌تر قرار می‌دهد. Consumer فقط با Contract موردنیاز کار می‌کند و لازم نیست جزئیات Implementation را بشناسد.

---

### 26. آیا Inheritance و Polymorphism یک مفهوم هستند؟

خیر. Inheritance یک رابطه میان Typeها برای اشتراک ساختار یا Behavior است، در حالی که Polymorphism امکان ارائه Behaviorهای متفاوت از طریق یک Interface یا Contract مشترک را توصیف می‌کند.

---

### 27. چرا Inheritance می‌تواند Design را پیچیده کند؟

زیرا ایجاد وابستگی میان Parent و Child می‌تواند تغییرات Parent را به Childها منتقل کند. همچنین Hierarchyهای عمیق می‌توانند فهم و تغییر سیستم را دشوار کنند.

---

### 28. چه زمانی Composition می‌تواند مناسب‌تر از Inheritance باشد؟

وقتی هدف اصلی ترکیب چند قابلیت مستقل باشد و رابطه منطقی Parent/Child وجود نداشته باشد، Composition معمولاً انعطاف بیشتری برای تغییر و توسعه فراهم می‌کند.

---

### 29. آیا JavaScript برای OOP به Class نیاز دارد؟

خیر. JavaScript پیش از `class` نیز قابلیت‌های Object-Oriented داشت. Objectها و Prototypeها بخش اصلی Object Model زبان هستند و `class` Syntax مدرن‌تری برای بیان برخی الگوهای OOP فراهم می‌کند.

---

### 30. مهم‌ترین اشتباه در یادگیری OOP چیست؟

تمرکز بیش از حد روی Syntaxهایی مانند `class`، `extends` و `super` بدون درک Object-Oriented Thinking، Responsibility، Encapsulation، Abstraction و Design Trade-offها.

---

# Golden Answers

## Junior Golden Answer

> OOP یک Programming Paradigm است که Software را حول Objects سازمان‌دهی می‌کند. Objectها State و Behavior دارند و هر Object مسئولیت مشخصی بر عهده می‌گیرد. مفاهیمی مانند Class، Encapsulation، Abstraction، Inheritance و Polymorphism ابزارهای اصلی این رویکرد هستند.

---

## Mid-Level Golden Answer

> OOP یک روش طراحی برای مدیریت Complexity است که Software را به Objects دارای State، Behavior و Responsibilities تقسیم می‌کند. Classها می‌توانند ساختار مشترک Objectهای مشابه را تعریف کنند، Encapsulation کنترل State را بهتر می‌کند، Abstraction Complexity غیرضروری را پنهان می‌کند و Inheritance و Polymorphism امکان مدل‌کردن روابط و Behaviorهای مشترک یا متفاوت را فراهم می‌کنند. در JavaScript، OOP فقط به Class محدود نیست، زیرا JavaScript یک زبان Multi-Paradigm با Object Model مبتنی بر Prototype است.

---

## Senior Golden Answer

> OOP را نباید مجموعه‌ای از Syntaxهای مربوط به Class دانست؛ بلکه یک رویکرد برای مدیریت Complexity و طراحی Responsibilityهاست. یک Design خوب باید State و Behavior را در مرزهای منطقی قرار دهد، Interfaceهای مناسب ارائه کند و Coupling را کنترل کند. Encapsulation و Abstraction به کنترل Complexity کمک می‌کنند، در حالی که Inheritance و Polymorphism برای مدل‌کردن روابط و رفتارهای مشترک یا متفاوت به کار می‌روند. با این حال، Inheritance همیشه انتخاب مناسبی نیست و در بسیاری از موارد Composition انعطاف بیشتری دارد. در JavaScript نیز باید میان OOP به‌عنوان یک Design Paradigm و Prototype-Based Object Model زبان تفاوت قائل شویم.

---

# دیدگاه Jonas

دیدگاه مهم در آموزش OOP این است که OOP را صرفاً به‌عنوان مجموعه‌ای از Syntaxها نبینیم.

نقطه اصلی، یادگیری نحوه **Model کردن Application با Objects و Responsibilities** است.

در این نگاه، خواننده ابتدا باید بفهمد:

```text
What are the Objects?
↓
What State do they own?
↓
What Behavior do they provide?
↓
What are their Responsibilities?
↓
How do they interact?
```

سپس Syntax و Mechanism زبان برای پیاده‌سازی این Design بررسی می‌شود.

این ترتیب با هدف این فصل نیز هماهنگ است:

> ابتدا Object-Oriented Thinking، سپس ابزارهای زبان.

---

# Conclusion

OOP را نباید با یک Keyword یا Syntax خاص تعریف کرد.

`class`، `extends` و سایر Syntaxها ابزارهایی برای پیاده‌سازی برخی مدل‌های Object-Oriented هستند.

اما پایه OOP جای دیگری است:

```text
Problem
↓
Objects
↓
State
↓
Behavior
↓
Responsibilities
↓
Interaction
```

از این مدل ذهنی، مفاهیم اصلی OOP شکل می‌گیرند:

```text
Object
↓
Class
↓
Instance
↓
Encapsulation
↓
Abstraction
↓
Inheritance
↓
Polymorphism
```

در JavaScript، این مفاهیم در کنار یک Object Model مبتنی بر Prototype قرار می‌گیرند.

بنابراین در ادامه کتاب، لازم است Mechanism واقعی JavaScript را بررسی کنیم.

فصل بعد به **Prototype و Prototype Chain** می‌پردازد و نشان می‌دهد JavaScript چگونه Properties و Behavior را میان Objects به اشتراک می‌گذارد.
