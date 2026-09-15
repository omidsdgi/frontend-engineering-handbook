# Chapter 29 — ES Classes

## Chapter Goal

در پایان این فصل، خواننده باید بتواند:

* مفهوم `class` را در JavaScript توضیح دهد.
* یک Class برای ایجاد Instanceهای مشابه طراحی کند.
* نقش `constructor` را در Initialization یک Instance توضیح دهد.
* تفاوت Instance Methods و Instance Fields را درک کند.
* Private Fields را با Syntax مناسب پیاده‌سازی کند.
* از Getters و Setters برای Controlled Access استفاده کند.
* تفاوت Class Syntax و Prototype Mechanism را درک کند.
* تشخیص دهد که Class در JavaScript یک مدل کاملاً جدا از Prototype نیست.

---

# Core Question

> **Class Syntax چگونه Object-Oriented Programming را در JavaScript ساده‌تر می‌کند؟**

---

# مقدمه

در فصل 26 دیدیم که در Object-Oriented Programming، Object می‌تواند State و Behavior داشته باشد.

در فصل 27 با Constructor Functions یاد گرفتیم چگونه Objectهای مشابه ایجاد کنیم.

در فصل 28 نیز دیدیم که Prototype چگونه امکان Shared Behavior میان Instanceهای مختلف را فراهم می‌کند.

برای مثال، با Constructor Function می‌توانستیم بنویسیم:

```javascript
function User(name, email) {
  this.name = name;
  this.email = email;
}

User.prototype.login = function () {
  console.log(`${this.name} logged in`);
};

const user1 = new User('Omid', 'omid@example.com');
const user2 = new User('Sara', 'sara@example.com');
```

این روش کاملاً معتبر است.

اما Syntax آن در پروژه‌های بزرگ می‌تواند نسبتاً پراکنده شود:

```text
Constructor Function
        ↓
this
        ↓
prototype
        ↓
Shared Methods
```

JavaScript برای نوشتن همین مدل با Syntaxی منظم‌تر، `class` را ارائه می‌کند.

بنابراین سؤال اصلی این فصل این نیست که:

> آیا Class یک روش کاملاً جدید برای Object Creation است؟

بلکه سؤال مهم‌تر این است:

> Class چه چیزی را برای ما ساده‌تر می‌کند؟

پاسخ این است:

**Class یک Syntax منسجم برای تعریف Objectهای مشابه، Constructor، Methods و سایر Class Members فراهم می‌کند.**

و نکته بسیار مهم این است که Class در JavaScript جایگزین Prototype به‌عنوان مکانیزم زیربنایی نشده است.

در واقع:

```text
Class Syntax
     ↓
Prototype-based Mechanism
```

بنابراین ابتدا Syntax جدید را یاد می‌گیریم و سپس رابطه آن را با Prototypeهایی که در فصل قبل آموختیم بررسی می‌کنیم.

---

# Class چیست؟

## چرا به Class نیاز داریم؟

فرض کنید در یک Application فروشگاهی چندین Product داریم.

هر Product می‌تواند State مخصوص خودش را داشته باشد:

```text
title
price
```

و Behavior مشترکی مانند:

```text
discount()
```

ارائه کند.

با Constructor Function می‌توانستیم این مدل را بسازیم.

اما Class اجازه می‌دهد تمام اجزای مربوط به این Object Model را در یک ساختار واحد قرار دهیم:

```javascript
class Product {
  constructor(title, price) {
    this.title = title;
    this.price = price;
  }

  getPrice() {
    return this.price;
  }
}
```

در اینجا یک ساختار واحد داریم که مشخص می‌کند یک `Product` چگونه ساخته می‌شود و چه Behaviorهایی دارد.

به این ساختار **Class** می‌گوییم.

### Definition

**Class یک Syntax برای تعریف ساختار و رفتار Objectهای مشابه در JavaScript است.**

Class خودش Instance نیست.

بلکه الگویی است که از روی آن می‌توان Instance ایجاد کرد.

مدل ذهنی:

```text
Class
  ↓
Instance
  ↓
Object
```

مثلاً:

```javascript
const product1 = new Product('Laptop', 1200);
const product2 = new Product('Phone', 800);
```

اینجا:

```text
Product
   ↓
Class

product1
product2
   ↓
Instances
```

هر Instance State مستقل خودش را دارد.

---

# Class Declaration

برای تعریف یک Class از Keyword `class` استفاده می‌کنیم:

```javascript
class User {
  // class body
}
```

نام Class معمولاً با حرف بزرگ شروع می‌شود:

```javascript
class User {}
class Product {}
class ShoppingCart {}
```

این نام‌گذاری یک **Convention** است.

یعنی زبان JavaScript اجبار نمی‌کند که نام Class با حرف بزرگ باشد، اما این Convention به خواننده نشان می‌دهد که قرار است با Class کار کنیم.

---

# Constructor

بعد از تعریف Class، معمولاً اولین چیزی که برای ایجاد Instanceهای قابل تنظیم نیاز داریم، `constructor` است.

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}
```

`constructor` یک Method ویژه در Class Syntax است که هنگام ایجاد Instance با `new` اجرا می‌شود.

مثلاً:

```javascript
const user = new User(
  'Omid',
  'omid@example.com'
);
```

در اینجا:

```text
new User(...)
      ↓
constructor(...)
      ↓
Instance Initialization
```

مقدارهای ورودی به Constructor منتقل می‌شوند:

```javascript
constructor(name, email)
```

و سپس روی Instance قرار می‌گیرند:

```javascript
this.name = name;
this.email = email;
```

در نتیجه:

```javascript
console.log(user.name);
// Omid

console.log(user.email);
// omid@example.com
```

### نکته مهم

`constructor` وظیفه اصلی‌اش **Initialization** کردن Instance است.

یعنی معمولاً باید State اولیه Object را آماده کند.

---

# ایجاد Instance با new

Class برای ایجاد Instance مستقیماً مانند یک Function معمولی فراخوانی نمی‌شود.

باید از `new` استفاده کنیم:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user = new User('Omid');
```

اینجا:

```text
User
 ↓
new
 ↓
constructor
 ↓
Instance
```

`new` همان Constructor Call را ایجاد می‌کند که در فصل قبل با Constructor Functions بررسی کردیم.

بنابراین دانشی که درباره `new` در فصل قبل به دست آوردیم، مستقیماً در Class Syntax قابل استفاده است.

---

# this در Constructor

در فصل Constructor Functions دیدیم که `this` به Instance مربوط می‌شود.

همین مدل در Class نیز وجود دارد:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}
```

وقتی می‌نویسیم:

```javascript
const user = new User('Omid');
```

در زمان اجرای Constructor:

```javascript
this
```

به Instance در حال ساخته‌شدن اشاره می‌کند.

بنابراین:

```javascript
this.name = name;
```

یعنی:

> Property به نام `name` را روی Instance فعلی قرار بده.

برای دو Instance مختلف:

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

داریم:

```text
user1 → { name: 'Omid' }
user2 → { name: 'Sara' }
```

State آنها مستقل است.

---

# Methods در Class

یکی از مهم‌ترین مزیت‌های Syntax مربوط به Class این است که Methodها مستقیماً در بدنه Class نوشته می‌شوند.

مثلاً:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}
```

سپس:

```javascript
const user = new User('Omid');

user.login();
```

در اینجا `login` Behavior مربوط به User را مشخص می‌کند.

Syntax بسیار خواناتر از حالتی است که Constructor و Prototype Method جدا از هم نوشته شوند.

در Constructor Function باید می‌نوشتیم:

```javascript
function User(name) {
  this.name = name;
}

User.prototype.login = function () {
  console.log(`${this.name} logged in`);
};
```

در Class:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}
```

Class Syntax این ارتباط را در یک ساختار واحد نشان می‌دهد.

---

# Methodها کجا قرار می‌گیرند؟

اینجا یکی از مهم‌ترین نکات فنی فصل قرار دارد.

ممکن است تصور کنیم:

> چون `login()` داخل Class نوشته شده است، پس برای هر Instance یک Function جدید ساخته می‌شود.

این تصور صحیح نیست.

در حالت معمول، Instance Methodهای Class در Prototype قرار می‌گیرند و Instanceها از همان Behavior مشترک استفاده می‌کنند.

مدل ذهنی:

```text
User Class
    ↓
User.prototype
    ↓
login()

user1 ──────┐
            ├──→ shared login()
user2 ──────┘
```

پس:

```javascript
user1.login === user2.login
```

نتیجه:

```javascript
true
```

این دقیقاً با چیزی که در Chapter 28 درباره Shared Behavior و Prototype آموختیم سازگار است.

Class Syntax این مکانیزم را حذف نکرده است.

فقط نوشتن آن را ساده‌تر کرده است.

---

# Fields

تا اینجا State را داخل Constructor ایجاد کردیم:

```javascript
class Product {
  constructor(title, price) {
    this.title = title;
    this.price = price;
  }
}
```

JavaScript همچنین Public Fields را فراهم می‌کند.

مثلاً:

```javascript
class Product {
  category = 'general';

  constructor(title, price) {
    this.title = title;
    this.price = price;
  }
}
```

در اینجا:

```javascript
category = 'general';
```

یک **Public Instance Field** است.

هر Instance مقدار مخصوص خودش را برای این Field خواهد داشت.

```javascript
const product1 = new Product('Laptop', 1200);
const product2 = new Product('Phone', 800);

console.log(product1.category);
// general

console.log(product2.category);
// general
```

Fieldها برای تعریف State اولیه‌ای که مستقیماً به Instance مربوط است، مفید هستند.

---

# Constructor یا Field؟

هر دو می‌توانند برای تعریف State استفاده شوند.

مثلاً:

```javascript
class Product {
  category = 'general';

  constructor(title, price) {
    this.title = title;
    this.price = price;
  }
}
```

در اینجا:

* `category` مقدار پیش‌فرض دارد.
* `title` و `price` از ورودی دریافت می‌شوند.

این تفکیک از نظر طراحی نیز خوانا است:

```text
Default State
    ↓
Fields

Input-dependent State
    ↓
Constructor
```

اگر مقدار یک Property از ورودی Instance وابسته باشد، Constructor معمولاً محل طبیعی مقداردهی آن است.

---

# Public Fields چه تفاوتی با Methods دارند؟

تفاوت مهمی میان State و Behavior وجود دارد.

Field:

```javascript
class Product {
  category = 'general';
}
```

به Instance مربوط است.

اما Method:

```javascript
class Product {
  getPrice() {
    return this.price;
  }
}
```

Behavior مشترک را تعریف می‌کند.

به‌صورت مفهومی:

```text
Instance
 ├── State
 │    ├── title
 │    └── price
 │
 └── Behavior
      └── getPrice()
```

این همان مدل Object-Oriented است که در فصل 26 معرفی کردیم.

---

# Private Fields

تا اینجا Propertyهای Class عمومی بودند.

یعنی خارج از Object نیز قابل دسترسی‌اند:

```javascript
class BankAccount {
  constructor(balance) {
    this.balance = balance;
  }
}

const account = new BankAccount(1000);

console.log(account.balance);
// 1000
```

هر کدی که به `account` دسترسی داشته باشد می‌تواند `balance` را بخواند یا تغییر دهد:

```javascript
account.balance = -500;
```

اما در بعضی طراحی‌ها نمی‌خواهیم State داخلی Object مستقیماً از بیرون قابل دستکاری باشد.

JavaScript برای این هدف **Private Fields** را ارائه می‌کند.

---

# Syntax مربوط به Private Fields

Private Field با `#` شروع می‌شود:

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }
}
```

اکنون:

```javascript
const account = new BankAccount(1000);
```

اما این دسترسی مجاز نیست:

```javascript
account.#balance;
```

Private Field فقط از داخل Class قابل دسترسی است.

مثلاً:

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }

  getBalance() {
    return this.#balance;
  }
}
```

اکنون:

```javascript
const account = new BankAccount(1000);

console.log(account.getBalance());
// 1000
```

اما:

```javascript
console.log(account.#balance);
```

خطای Syntax ایجاد می‌کند.

---

# چرا Private Fields مهم هستند؟

هدف Private Field صرفاً مخفی کردن یک Property نیست.

مسئله اصلی **کنترل دسترسی به State داخلی** است.

فرض کنید یک Account داریم:

```text
balance
```

اگر این State کاملاً Public باشد، هر بخش Application می‌تواند آن را تغییر دهد.

اما اگر Private باشد:

```text
BankAccount
     │
     ├── Public API
     │      └── getBalance()
     │
     └── Private State
            └── #balance
```

Object می‌تواند تصمیم بگیرد چه چیزی را در اختیار کد بیرونی قرار دهد.

این ایده با مفهوم Encapsulation ارتباط دارد.

اما بررسی کامل Encapsulation در Chapter 31 انجام خواهد شد.

در این فصل فقط مکانیزم Private Fields را می‌شناسیم.

---

# Private Field فقط یک Convention نیست

تفاوت مهمی میان:

```javascript
_balance
```

و:

```javascript
#balance
```

وجود دارد.

این:

```javascript
_balance
```

فقط یک Convention است.

یعنی برنامه‌نویسان معمولاً از `_` برای نشان دادن Property داخلی استفاده می‌کنند، اما JavaScript همچنان آن را Public می‌داند.

مثلاً:

```javascript
class User {
  constructor() {
    this._password = 'secret';
  }
}
```

هنوز امکان دسترسی وجود دارد:

```javascript
user._password;
```

اما:

```javascript
#password
```

یک Private Field واقعی در Class Syntax است.

---

# Getters and Setters

گاهی نمی‌خواهیم Property مستقیماً در اختیار مصرف‌کننده باشد.

اما از طرف دیگر نمی‌خواهیم دسترسی به آن را کاملاً حذف کنیم.

فرض کنید:

```javascript
class Product {
  constructor(price) {
    this.price = price;
  }
}
```

هر کسی می‌تواند بنویسد:

```javascript
product.price = -100;
```

در یک Application واقعی این می‌تواند State نامعتبر ایجاد کند.

یکی از راه‌حل‌ها استفاده از **Getter و Setter** است.

---

# Getter

Getter اجازه می‌دهد یک Method را مانند Property بخوانیم.

مثلاً:

```javascript
class Product {
  constructor(price) {
    this._price = price;
  }

  get price() {
    return this._price;
  }
}
```

اکنون:

```javascript
const product = new Product(100);

console.log(product.price);
```

به جای:

```javascript
product.price();
```

می‌نویسیم:

```javascript
product.price;
```

Getter با Keyword `get` تعریف می‌شود:

```javascript
get price() {
  return this._price;
}
```

از دید مصرف‌کننده، `price` مانند یک Property به نظر می‌رسد.

اما پشت آن یک Getter اجرا می‌شود.

---

# Setter

Setter برای کنترل مقداردهی به یک Property استفاده می‌شود.

```javascript
class Product {
  constructor(price) {
    this.price = price;
  }

  get price() {
    return this._price;
  }

  set price(value) {
    if (value < 0) {
      throw new Error('Price cannot be negative');
    }

    this._price = value;
  }
}
```

اکنون:

```javascript
const product = new Product(100);

product.price = 150;

console.log(product.price);
// 150
```

اما:

```javascript
product.price = -50;
```

باعث خطا می‌شود.

در اینجا Setter به‌عنوان یک نقطه کنترل برای تغییر State عمل می‌کند.

مدل ذهنی:

```text
External Code
      ↓
   product.price
      ↓
    Setter
      ↓
Validation
      ↓
Internal State
```

و برای خواندن:

```text
Internal State
      ↓
    Getter
      ↓
External Code
```

---

# چرا Getter و Setter به شکل Property استفاده می‌شوند؟

این طراحی باعث می‌شود Interface Object ساده باقی بماند.

مصرف‌کننده می‌نویسد:

```javascript
product.price
```

نه:

```javascript
product.getPrice()
```

و برای تغییر:

```javascript
product.price = 150;
```

نه:

```javascript
product.setPrice(150);
```

در عین حال، Class می‌تواند پشت این Syntax منطق اضافی داشته باشد.

بنابراین Getter و Setter میان دو هدف تعادل ایجاد می‌کنند:

```text
Simple Interface
      +
Controlled Access
```

---

# یک مثال کامل

اکنون می‌توانیم چند Concept فصل را کنار هم قرار دهیم:

```javascript
class User {
  #email;

  constructor(name, email) {
    this.name = name;
    this.#email = email;
  }

  get email() {
    return this.#email;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}

const user = new User(
  'Omid',
  'omid@example.com'
);

console.log(user.name);
// Omid

console.log(user.email);
// omid@example.com

user.login();
```

در این مثال:

```text
User
 │
 ├── constructor()
 │
 ├── Public Field
 │     └── name
 │
 ├── Private Field
 │     └── #email
 │
 ├── Getter
 │     └── email
 │
 └── Method
       └── login()
```

این Class یک Object Model نسبتاً ساده و واقعی برای Application ایجاد می‌کند.

---

# Class و Prototype

اکنون به مهم‌ترین ارتباط این فصل با فصل قبل می‌رسیم.

ممکن است پس از یادگیری Class تصور کنیم:

> از اینجا به بعد دیگر Prototype اهمیتی ندارد.

این تصور اشتباه است.

Class Syntax و Prototype Mechanism دو مفهوم متفاوت هستند.

Class به ما Syntax خواناتر می‌دهد:

```javascript
class User {
  login() {}
}
```

اما JavaScript همچنان برای Instance Methods از Prototype استفاده می‌کند.

می‌توانیم رابطه را به شکل زیر ببینیم:

```text
class User
     ↓
User.prototype
     ↓
login()

      ↑
      │
   lookup
      │

user instance
```

مثلاً:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log('Logged in');
  }
}

const user = new User('Omid');
```

در اینجا `login` یک Instance Method است و به‌صورت معمول در Prototype مربوط به Class قرار می‌گیرد.

بنابراین:

```javascript
user.login();
```

با همان مدل Property Lookup که در فصل Prototype یاد گرفتیم قابل توضیح است.

---

# Class Syntax در برابر Constructor Function

حالا می‌توانیم دو روش را کنار هم قرار دهیم.

### Constructor Function

```javascript
function User(name) {
  this.name = name;
}

User.prototype.login = function () {
  console.log(`${this.name} logged in`);
};

const user = new User('Omid');
```

### Class

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}

const user = new User('Omid');
```

در هر دو مدل:

```text
Create Instance
       ↓
new
       ↓
Instance State
       ↓
Shared Behavior
```

تفاوت اصلی در Syntax و نحوه بیان این مدل است.

Class Syntax رابطه میان Constructor، Instance Methods و Object Model را بسیار مستقیم‌تر نشان می‌دهد.

---

# Class یک Object نیست

یک سوءبرداشت رایج این است که:

> Class همان Object است.

اما:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}
```

`User` یک Class است.

در مقابل:

```javascript
const user = new User('Omid');
```

`user` یک Instance است که یک Object محسوب می‌شود.

مدل:

```text
User
 ↓
Class

user
 ↓
Instance / Object
```

بنابراین این دو را نباید یکی بدانیم.

---

# Class یک Instance نیست

این نیز مهم است که Class را با Instance اشتباه نگیریم.

وقتی می‌نویسیم:

```javascript
const user = new User('Omid');
```

یک Instance ایجاد شده است.

اگر دوباره بنویسیم:

```javascript
const anotherUser = new User('Sara');
```

یک Instance جدید ایجاد می‌شود.

اما Class همچنان همان Class است:

```text
             User Class
             /         \
            /           \
        user          anotherUser
       Instance         Instance
```

هر Instance State مستقل خود را دارد.

Behavior مشترک نیز از مکانیزم Prototype استفاده می‌کند.

---

# Static Members — Preview

Classها علاوه بر Instance Members می‌توانند Members مربوط به خود Class نیز داشته باشند.

این Members با Keyword `static` تعریف می‌شوند.

مثلاً:

```javascript
class MathHelper {
  static double(value) {
    return value * 2;
  }
}
```

این Method متعلق به خود Class است:

```javascript
MathHelper.double(10);
```

نه به Instance:

```javascript
const helper = new MathHelper();

helper.double(10);
```

این تفاوت را فعلاً فقط در حد مدل ذهنی نگه می‌داریم:

```text
Instance Member
      ↓
instance.method()

Static Member
      ↓
Class.method()
```

جزئیات Static Methods و Static Properties در Chapter 31 بررسی خواهد شد.

---

# Class Declaration و Scope

Class Declaration را باید مانند یک Declaration واقعی JavaScript در نظر گرفت، نه چیزی که بتوان قبل از تعریف مانند یک Function Declaration از آن استفاده کرد.

مثلاً:

```javascript
const user = new User('Omid');

class User {
  constructor(name) {
    this.name = name;
  }
}
```

این کد قابل استفاده نیست.

بنابراین یک Rule عملی مهم این است:

> Class را پیش از استفاده از آن تعریف کنید.

جزئیات فنی رفتار Class Declaration در ارتباط با Environment، Hoisting و TDZ در بخش Runtime کتاب بررسی خواهد شد.

---

# Common Mistakes

## اشتباه اول: تصور اینکه Class یک نوع جدید از Object است

❌

> Class همان Object است.

✔

Class الگویی برای تعریف ساختار و رفتار است.

Instanceای که از Class ساخته می‌شود یک Object است.

```text
Class
 ↓
Instance
 ↓
Object
```

---

## اشتباه دوم: تصور اینکه Class جای Prototype را گرفته است

❌

> بعد از ES Classes دیگر Prototype وجود ندارد.

✔

Class Syntax همچنان از Prototype-based mechanism استفاده می‌کند.

Class روش ساده‌تر و منظم‌تری برای بیان این مدل فراهم می‌کند.

---

## اشتباه سوم: تصور اینکه Method داخل Class برای هر Instance کپی می‌شود

❌

```text
user1 → login()
user2 → login()
user3 → login()
```

به این معنا که سه Function مستقل ساخته شده‌اند.

✔

Instance Methods معمولاً از Prototype به‌صورت مشترک استفاده می‌کنند.

```text
User.prototype
      ↓
   login()
   ↑    ↑
user1 user2
```

---

## اشتباه چهارم: اشتباه گرفتن Field با Method

Field:

```javascript
class Product {
  price = 100;
}
```

Method:

```javascript
class Product {
  getPrice() {
    return this.price;
  }
}
```

اولی State است.

دومی Behavior است.

---

## اشتباه پنجم: تصور اینکه `_property` واقعاً Private است

❌

```javascript
this._password = 'secret';
```

`_password` همچنان Public است.

✔

برای Private Field واقعی:

```javascript
this.#password = 'secret';
```

---

## اشتباه ششم: استفاده نادرست از Getter

Getter مانند Method فراخوانی نمی‌شود.

❌

```javascript
user.email();
```

اگر `email` Getter باشد.

✔

```javascript
user.email;
```

---

## اشتباه هفتم: استفاده نادرست از Setter

Setter نیز مانند یک Function فراخوانی نمی‌شود.

❌

```javascript
user.email('new@example.com');
```

✔

```javascript
user.email = 'new@example.com';
```

---

## اشتباه هشتم: قرار دادن همه State در Private Fields بدون نیاز

Private Fields ابزار Encapsulation هستند، نه چیزی که باید برای تمام Propertyها به‌صورت خودکار استفاده شود.

اگر یک Property واقعاً بخشی از Public State Object است، Public بودن آن می‌تواند کاملاً مناسب باشد.

هدف باید طراحی Interface مناسب باشد، نه Private کردن همه چیز.

---

# Best Practices

## 1. Class را حول یک Responsibility مشخص طراحی کنید

یک Class بهتر است یک مفهوم مشخص در Application را مدل کند.

مثلاً:

```javascript
class ShoppingCart {
  // ...
}
```

از ترکیب کردن مسئولیت‌های کاملاً متفاوت در یک Class خودداری کنید.

---

## 2. Constructor را برای Initialization نگه دارید

Constructor معمولاً باید State اولیه را آماده کند:

```javascript
class Product {
  constructor(title, price) {
    this.title = title;
    this.price = price;
  }
}
```

Logic پیچیده و غیرمرتبط را بدون دلیل داخل Constructor قرار ندهید.

---

## 3. Behavior مشترک را به شکل Instance Method تعریف کنید

به‌جای ایجاد Function جدید برای هر Instance:

```javascript
class User {
  login() {
    // shared behavior
  }
}
```

Class Syntax به‌صورت طبیعی این مدل را ایجاد می‌کند.

---

## 4. Private Fields را زمانی استفاده کنید که واقعاً State داخلی است

اگر یک مقدار بخشی از Implementation داخلی Object است و نباید مستقیماً تغییر کند:

```javascript
class BankAccount {
  #balance;
}
```

Private Field انتخاب مناسبی است.

---

## 5. Getter و Setter را برای Controlled Access استفاده کنید

اگر خواندن یا تغییر یک مقدار نیازمند کنترل یا Validation است:

```javascript
get price() {
  return this._price;
}

set price(value) {
  // validation
}
```

این ابزارها می‌توانند Interface Object را ساده نگه دارند.

---

## 6. Class را فقط به دلیل استفاده از OOP ایجاد نکنید

وجود `class` به‌تنهایی به معنی طراحی خوب نیست.

اگر یک Object ساده با Object Literal یا Function به‌خوبی مدل می‌شود، الزاماً نیازی به Class نیست.

Class زمانی ارزش بیشتری دارد که:

* چند Instance مشابه داریم.
* State و Behavior مشخصی داریم.
* Object دارای Responsibility مشخصی است.
* Shared Behavior اهمیت دارد.

---

# دیدگاه Jonas

یکی از ایده‌های مهم در رویکرد آموزشی Jonas Schmedtmann این است که هنگام یادگیری OOP باید ابتدا مدل Object و رابطه میان State و Behavior را درک کنیم و سپس Syntaxهایی مانند Class را یاد بگیریم.

این موضوع با مسیر این کتاب نیز هماهنگ است:

```text
Object
 ↓
OOP
 ↓
Constructor Function
 ↓
Prototype
 ↓
Class
```

بنابراین Class نباید به‌عنوان یک Syntax مستقل و جدا از مباحث قبلی دیده شود.

خواننده‌ای که Prototype و Constructor Function را درک کرده باشد، راحت‌تر می‌تواند بفهمد Class در JavaScript چه مسئله‌ای را حل می‌کند.

---

# Summary

در این فصل دیدیم که JavaScript برای تعریف Objectهای مشابه، Syntax مربوط به `class` را ارائه می‌کند.

یک Class می‌تواند شامل Constructor، Instance Methods و Fields باشد.

با:

```javascript
new
```

از روی Class یک Instance ایجاد می‌کنیم.

Constructor هنگام ایجاد Instance برای Initialization استفاده می‌شود:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}
```

Instance Methods در Class Syntax به شکل ساده و مستقیم نوشته می‌شوند:

```javascript
class User {
  login() {
    // ...
  }
}
```

این Methodها در مدل معمول Class روی Prototype قرار می‌گیرند و میان Instanceها Shared هستند.

Public Fields برای تعریف State مربوط به Instance استفاده می‌شوند.

Private Fields با `#` امکان ایجاد State واقعاً خصوصی را فراهم می‌کنند:

```javascript
#balance
```

Getter و Setter نیز امکان Controlled Access به State را فراهم می‌کنند.

در نهایت، مهم‌ترین مدل ذهنی این فصل این است:

```text
Class
  ↓
Constructor
  ↓
Instance
  ↓
Methods
  ↓
Prototype-based Sharing
```

بنابراین Class Syntax را نباید به‌عنوان جایگزینی برای Prototype Mechanism در نظر گرفت.

---

# Key Takeaways

* `class` یک Syntax برای تعریف Object Model است.
* Class خودش Instance نیست.
* Instance با `new` ایجاد می‌شود.
* `constructor` برای Initialization Instance استفاده می‌شود.
* `this` در Constructor به Instance در حال ایجاد مربوط است.
* Instance Methods معمولاً روی Prototype قرار می‌گیرند.
* Methodهای مشترک میان Instanceها از Prototype استفاده می‌کنند.
* Public Fields بخشی از State مربوط به Instance هستند.
* Private Fields با `#` واقعاً Private هستند.
* `_property` فقط یک Convention است و Private واقعی نیست.
* Getter برای Controlled Reading و Setter برای Controlled Writing مفید است.
* Class Syntax مکانیزم Prototype را حذف نکرده است.
* Static Members متعلق به Class هستند، نه Instanceها.
* Inheritance و Polymorphism موضوع فصل بعد هستند.
* Encapsulation و Static Members به‌صورت عمیق در فصل اختصاصی خود بررسی خواهند شد.

---

# Technical Interview

## Junior Level

### 1. Class در JavaScript چیست؟

**پاسخ:**

Class یک Syntax برای تعریف ساختار و رفتار Objectهای مشابه است. از روی Class می‌توان با `new` Instance ایجاد کرد.

---

### 2. `constructor` چه کاری انجام می‌دهد؟

**پاسخ:**

`constructor` هنگام ایجاد Instance اجرا می‌شود و معمولاً برای Initialization کردن State اولیه Instance استفاده می‌شود.

---

### 3. چگونه از یک Class یک Instance ایجاد می‌کنیم؟

**پاسخ:**

با استفاده از `new`:

```javascript
const user = new User('Omid');
```

---

### 4. تفاوت Class و Instance چیست؟

**پاسخ:**

Class الگویی برای تعریف ساختار و رفتار است، در حالی که Instance یک Object واقعی است که از آن Class ایجاد شده است.

---

### 5. Private Field چگونه تعریف می‌شود؟

**پاسخ:**

با `#`:

```javascript
class User {
  #password;
}
```

این Field فقط از داخل Class قابل دسترسی است.

---

## Mid-Level

### 6. آیا Class در JavaScript یک مکانیزم کاملاً متفاوت از Prototype است؟

**پاسخ:**

خیر. Class یک Syntax سطح بالاتر برای تعریف Object Model است و Instance Methods در مدل معمول روی Prototype قرار می‌گیرند. بنابراین Class Syntax همچنان با Prototype mechanism ارتباط دارد.

---

### 7. چرا Methodهای Class برای هر Instance یک Function جدا ایجاد نمی‌کنند؟

**پاسخ:**

چون Instance Methods معمولاً روی Prototype قرار می‌گیرند و Instanceها از همان Method مشترک استفاده می‌کنند.

---

### 8. تفاوت Public Field و Instance Method چیست؟

**پاسخ:**

Public Field بخشی از State مربوط به Instance است، در حالی که Instance Method رفتار Object را تعریف می‌کند و در مدل معمول Class از Prototype به‌صورت مشترک استفاده می‌شود.

---

### 9. Getter و Setter چه مسئله‌ای را حل می‌کنند؟

**پاسخ:**

آنها امکان Controlled Access به Propertyها را فراهم می‌کنند. Getter خواندن را کنترل می‌کند و Setter می‌تواند هنگام تغییر مقدار، Validation یا منطق دیگری اجرا کند.

---

### 10. تفاوت `_property` و `#property` چیست؟

**پاسخ:**

`_property` فقط یک Convention برای نشان دادن یک Property داخلی است و همچنان Public است.

اما `#property` یک Private Field واقعی است که خارج از Class قابل دسترسی نیست.

---

### 11. چرا Constructor را نباید محل مناسبی برای تمام Logicهای Class در نظر گرفت؟

**پاسخ:**

وظیفه اصلی Constructor Initialization است. قرار دادن Logic پیچیده و غیرمرتبط در آن می‌تواند مسئولیت Constructor را افزایش دهد و طراحی Class را پیچیده‌تر کند.

---

## Senior Level

### 12. آیا می‌توان گفت ES Classes فقط Syntactic Sugar برای Constructor Functions هستند؟

**پاسخ:**

این بیان برای یک مدل مقدماتی مفید است، اما از نظر فنی بیش از حد ساده‌سازی شده است. Class Syntax روی همان خانواده Prototype-based Object Model قرار دارد، اما Semantics مربوط به Classها با Constructor Functionهای معمول کاملاً یکسان نیست. بنابراین بهتر است بگوییم Class Syntax یک مدل Class-oriented و منسجم برای کار با Objectها ارائه می‌کند که در JavaScript همچنان بر Prototype mechanism تکیه دارد.

---

### 13. وقتی یک Method داخل Class نوشته می‌شود، چرا Instance می‌تواند آن را پیدا کند؟

**پاسخ:**

Instance در هنگام Property Lookup ابتدا خودش بررسی می‌شود و سپس در Prototype مربوط به آن جست‌وجو ادامه پیدا می‌کند. Instance Methodهای Class در Prototype قرار دارند، بنابراین Instance می‌تواند آنها را از طریق Prototype relationship پیدا کند.

---

### 14. چرا Private Fields با `_property` متفاوت هستند؟

**پاسخ:**

Underscore فقط یک قرارداد نام‌گذاری است و هیچ محدودیت دسترسی توسط زبان ایجاد نمی‌کند.

اما Private Field با `#` بخشی از Syntax و Semantics زبان است و دسترسی مستقیم از خارج Class به آن مجاز نیست.

---

### 15. آیا استفاده از Class همیشه از Object Literal یا Function بهتر است؟

**پاسخ:**

خیر.

Class زمانی ارزش بیشتری دارد که بخواهیم یک Object Model با Instanceهای متعدد، State مشخص و Behavior مرتبط طراحی کنیم. استفاده از Class صرفاً به دلیل OOP بودن، به‌تنهایی نشانه طراحی بهتر نیست.

---

### 16. تفاوت Instance Member و Static Member چیست؟

**پاسخ:**

Instance Member متعلق به هر Instance است و از طریق Instance قابل دسترسی است:

```javascript
user.login();
```

Static Member متعلق به خود Class است:

```javascript
User.create();
```

جزئیات Static Members در فصل اختصاصی آن بررسی می‌شود.

---

### 17. چرا درک Prototype برای فهم ES Classes همچنان ضروری است؟

**پاسخ:**

زیرا Class Syntax نباید به‌عنوان جایگزین Prototype mechanism در نظر گرفته شود. Instance Methods، Property Lookup و Shared Behavior همچنان با Prototype relationship قابل توضیح هستند.

بنابراین کسی که فقط Syntax `class` را حفظ کرده باشد، اما Prototype را نفهمیده باشد، مدل کامل و دقیقی از Object Model در JavaScript ندارد.

---

# Golden Answers

### Golden Answer — Class

> **Class یک Syntax برای تعریف ساختار و رفتار Objectهای مشابه در JavaScript است.**

---

### Golden Answer — Constructor

> **Constructor متدی ویژه است که هنگام ایجاد Instance با `new` اجرا می‌شود و معمولاً مسئول Initialization کردن State اولیه Instance است.**

---

### Golden Answer — Instance Method

> **Instance Method رفتاری است که در Class تعریف می‌شود و در مدل معمول Class روی Prototype قرار می‌گیرد تا Instanceها بتوانند از Behavior مشترک استفاده کنند.**

---

### Golden Answer — Private Field

> **Private Field با `#` تعریف می‌شود و برخلاف Conventionهایی مانند `_property`، واقعاً از دسترسی مستقیم خارج از Class محافظت می‌شود.**

---

### Golden Answer — Getter / Setter

> **Getter و Setter یک Interface شبیه Property فراهم می‌کنند، اما اجازه می‌دهند خواندن یا تغییر State تحت کنترل Class انجام شود.**

---

### Golden Answer — Class vs Prototype

> **Class Syntax روش خواناتری برای تعریف Object Model ارائه می‌کند، اما Prototype mechanism همچنان بخش مهمی از مدل Objectهای JavaScript است.**

---

### Golden Answer — Class vs Instance

> **Class تعریف‌کننده ساختار و رفتار است؛ Instance یک Object واقعی است که با `new` از روی آن ایجاد می‌شود.**

---

### Golden Answer — Static Member

> **Static Member متعلق به خود Class است، نه Instance؛ بنابراین از طریق نام Class استفاده می‌شود، نه از طریق Instance.**

---

# Conclusion

در فصل‌های قبل مسیر مهمی را طی کردیم.

ابتدا Object را شناختیم.

سپس برای ایجاد Objectهای مشابه به Constructor Functions رسیدیم.

بعد Prototype را برای Shared Behavior و Property Lookup بررسی کردیم.

اکنون Class Syntax را می‌توانیم در جایگاه واقعی خود قرار دهیم:

```text
Object
  ↓
Constructor Function
  ↓
new
  ↓
Instance
  ↓
Prototype
  ↓
Shared Behavior
  ↓
Class Syntax
```

Class این مفاهیم را حذف نمی‌کند.

بلکه آنها را در یک Syntax منسجم‌تر قرار می‌دهد.

بنابراین وقتی می‌نویسیم:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}
```

نباید فقط Syntax زیر را ببینیم:

```text
class
constructor
method
```

بلکه باید مدل پشت آن را نیز ببینیم:

```text
User Class
    ↓
new User(...)
    ↓
Instance
    ↓
Instance State
    ↓
Prototype-based Shared Behavior
```

این مدل ذهنی برای درک مباحث بعدی OOP ضروری است.

در فصل بعد، این Classها را وارد مرحله بعدی می‌کنیم و بررسی خواهیم کرد که چگونه Classها می‌توانند از یکدیگر **Inheritance** داشته باشند، چگونه `extends` و `super` کار می‌کنند و چگونه **Polymorphism** در این مدل شکل می‌گیرد.
