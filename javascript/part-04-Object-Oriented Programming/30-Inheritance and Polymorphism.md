# Chapter 30 — Inheritance and Polymorphism

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Inheritance** را در OOP توضیح دهید.
* توضیح دهید چرا یک Class ممکن است بخشی از Behavior و State یک Class دیگر را نیاز داشته باشد.
* نقش `extends` را در ایجاد رابطه بین Parent Class و Child Class توضیح دهید.
* تفاوت Parent Class و Child Class را تشخیص دهید.
* نقش `super` را هنگام اجرای Constructor و Methodهای Parent توضیح دهید.
* مفهوم **Method Overriding** را درک کنید.
* توضیح دهید چگونه Overriding به **Polymorphism** منجر می‌شود.
* یک مثال ساده از Polymorphism را تحلیل کنید.
* مزایا و محدودیت‌های Inheritance را در طراحی نرم‌افزار تشخیص دهید.
* تفاوت کلی **Inheritance** و **Composition** را توضیح دهید.
* در یک مسئله واقعی تشخیص دهید که آیا رابطه‌ی Inheritance مناسب است یا Composition.

---

# Core Question

> **چگونه Classها رفتار مشترک را به ارث می‌برند و رفتار متفاوت ارائه می‌کنند؟**

جریان این فصل:

```text
Inheritance
↓
extends
↓
Parent Class
↓
Child Class
↓
super
↓
Method Overriding
↓
Polymorphism
↓
Composition
```

در فصل 29 با `class`، Constructor، Instance، Method و Field آشنا شدیم.

اکنون یک سؤال طبیعی شکل می‌گیرد:

> اگر چند Class بخشی از Behavior مشترک داشته باشند، آیا باید آن Behavior را در همه Classها تکرار کنیم؟

Inheritance یکی از پاسخ‌های JavaScript به این مسئله است.

---

# مقدمه

فرض کنید در یک Application فروشگاهی چند نوع User داریم.

برای مثال:

```text
User
Admin
Customer
```

همه این Objectها ممکن است اطلاعات مشترکی داشته باشند:

```text
name
email
```

و Behavior مشترکی مانند:

```text
login()
logout()
```

داشته باشند.

اما Admin ممکن است Behavior خاص خود را نیز داشته باشد:

```text
deleteUser()
```

و Customer ممکن است Behavior متفاوتی داشته باشد:

```text
placeOrder()
```

اگر برای هر Class تمام Behaviorهای مشترک را دوباره بنویسیم، کد تکراری ایجاد می‌شود.

Inheritance به ما اجازه می‌دهد رابطه‌ای میان Classها ایجاد کنیم:

```text
User
 ↓
Admin
Customer
```

در این مدل، `Admin` و `Customer` می‌توانند Behaviorهای عمومی `User` را دریافت کنند و در صورت نیاز Behavior مخصوص خود را اضافه یا جایگزین کنند.

این همان نقطه‌ای است که Inheritance وارد مدل ذهنی OOP می‌شود.

---

# Inheritance

## چرا به Inheritance نیاز داریم؟

فرض کنید یک Class عمومی برای User داریم:

```javascript
class User {
  login() {
    console.log('User logged in');
  }

  logout() {
    console.log('User logged out');
  }
}
```

اکنون می‌خواهیم Admin داشته باشیم:

```javascript
class Admin {
  login() {
    console.log('User logged in');
  }

  logout() {
    console.log('User logged out');
  }

  deleteUser() {
    console.log('User deleted');
  }
}
```

کد کار می‌کند.

اما دو Method زیر تکراری هستند:

```javascript
login()
logout()
```

اگر این Behaviorها در `User` قرار دارند، بهتر است Admin به جای تکرار آن‌ها، از آن‌ها استفاده کند.

اینجاست که Inheritance مفید می‌شود.

---

## تعریف Inheritance

### تعریف ساده

**Inheritance** مکانیزمی است که به یک Class اجازه می‌دهد Behavior و قابلیت‌های Class دیگری را دریافت کند و در صورت نیاز آن‌ها را گسترش یا تغییر دهد.

به بیان ساده:

> Child Class می‌تواند از Parent Class استفاده مجدد کند.

مدل ذهنی:

```text
Parent Class
     ↓
Shared Behavior
     ↓
Child Class
     ↓
Reuse + Extension
```

---

## تعریف فنی

در JavaScript، Class Inheritance با استفاده از `extends` ایجاد می‌شود.

یک Child Class با `extends` رابطه‌ی ارث‌بری با Parent Class برقرار می‌کند.

مثلاً:

```javascript
class Admin extends User {
}
```

در اینجا:

```text
User  → Parent Class
Admin → Child Class
```

است.

`Admin` می‌تواند از قابلیت‌های قابل دسترس `User` استفاده کند.

---

# `extends`

اکنون Syntax اصلی را ببینیم.

```javascript
class User {
  login() {
    console.log('User logged in');
  }
}

class Admin extends User {
}
```

کلمه‌ی:

```javascript
extends
```

رابطه‌ی Inheritance را ایجاد می‌کند.

اکنون:

```javascript
const admin = new Admin();

admin.login();
```

خروجی:

```text
User logged in
```

نکته مهم این است که `login()` داخل `Admin` تعریف نشده است.

Behavior از رابطه‌ی Inheritance به دست آمده است.

---

# Parent Class و Child Class

در مثال:

```javascript
class User {
  login() {
    console.log('User logged in');
  }
}

class Admin extends User {
}
```

`User` را **Parent Class** می‌نامیم.

و:

```text
Admin
```

را **Child Class** می‌نامیم.

مدل ذهنی:

```text
User
 │
 │ extends
 ↓
Admin
```

Parent معمولاً Behavior عمومی‌تر را تعریف می‌کند.

Child می‌تواند آن Behavior را استفاده کند و قابلیت‌های بیشتری اضافه کند.

---

# یک مثال واقعی‌تر

فرض کنید در یک Application دو نوع User داریم:

```text
User
Admin
```

User عمومی:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  login() {
    console.log(`${this.name} logged in`);
  }
}
```

اکنون Admin:

```javascript
class Admin extends User {
  deleteUser() {
    console.log('User deleted');
  }
}
```

می‌توانیم Instance ایجاد کنیم:

```javascript
const admin = new Admin(
  'Omid',
  'omid@example.com'
);
```

و هر دو Behavior را استفاده کنیم:

```javascript
admin.login();
admin.deleteUser();
```

خروجی:

```text
Omid logged in
User deleted
```

`login()` از Parent آمده است.

`deleteUser()` مخصوص Child است.

---

# Inheritance یعنی Reuse، نه Copy

یک سوءبرداشت مهم این است که تصور کنیم:

> `extends` تمام کد Parent را داخل Child کپی می‌کند.

مدل ذهنی صحیح این نیست.

Inheritance یک **رابطه** بین Classها ایجاد می‌کند.

در JavaScript این رابطه در نهایت با مکانیزم Prototypeها پیاده‌سازی می‌شود که در فصل‌های قبل بررسی شده است.

بنابراین:

```javascript
class Admin extends User {
}
```

به معنی:

> Admin را در رابطه‌ی ارث‌بری با User قرار بده.

است، نه اینکه Source Code مربوط به `User` را داخل `Admin` کپی کن.

---

# Child Class می‌تواند Behavior جدید اضافه کند

Inheritance فقط برای استفاده از Behavior موجود نیست.

Child می‌تواند Behavior جدید نیز داشته باشد.

```javascript
class User {
  login() {
    console.log('User logged in');
  }
}

class Admin extends User {
  deleteUser() {
    console.log('User deleted');
  }
}
```

اکنون:

```text
User
├── login()

Admin
├── login()       ← inherited
└── deleteUser()  ← own behavior
```

این یکی از الگوهای اصلی Inheritance است:

```text
Parent
↓
Common Behavior

Child
↓
Common Behavior + Specialized Behavior
```

---

# Child Class و Constructor

اکنون به بخش مهم‌تری می‌رسیم.

فرض کنید Parent دارای Constructor باشد:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}
```

و Admin از آن ارث‌بری کند:

```javascript
class Admin extends User {
}
```

اکنون:

```javascript
const admin = new Admin(
  'Omid',
  'omid@example.com'
);
```

در این حالت Child Constructor تعریف نکرده است.

پس Constructor Parent برای ساخت Instance استفاده می‌شود.

بنابراین:

```javascript
admin.name
```

دارای مقدار:

```text
Omid
```

خواهد بود.

---

# وقتی Child Constructor داشته باشد

فرض کنید Admin به State بیشتری نیاز دارد:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class Admin extends User {
  constructor(name, email, permissions) {
    this.permissions = permissions;
  }
}
```

اکنون یک مشکل داریم.

Child Constructor قبل از استفاده از `this` باید Parent Constructor را فراخوانی کند.

برای این کار از:

```javascript
super()
```

استفاده می‌کنیم.

---

# `super`

## چرا `super` لازم است؟

در یک Child Class که Constructor دارد، JavaScript باید ابتدا بخش مربوط به Parent را آماده کند.

برای این کار:

```javascript
super(...)
```

Parent Constructor را فراخوانی می‌کند.

مثلاً:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class Admin extends User {
  constructor(name, email, permissions) {
    super(name, email);

    this.permissions = permissions;
  }
}
```

اکنون:

```javascript
const admin = new Admin(
  'Omid',
  'omid@example.com',
  ['delete-user']
);
```

Instance دارای:

```text
name
email
permissions
```

است.

---

# تحلیل `super()`

کد:

```javascript
constructor(name, email, permissions) {
  super(name, email);

  this.permissions = permissions;
}
```

را مرحله‌ای ببینیم.

ابتدا:

```javascript
super(name, email);
```

Parent Constructor را با داده‌های مربوط به Parent اجرا می‌کند.

Parent:

```javascript
constructor(name, email) {
  this.name = name;
  this.email = email;
}
```

State عمومی User را مقداردهی می‌کند.

سپس:

```javascript
this.permissions = permissions;
```

State مخصوص Admin را اضافه می‌کند.

مدل ذهنی:

```text
Admin Constructor
       ↓
super()
       ↓
User Constructor
       ↓
name + email
       ↓
Admin-specific initialization
       ↓
permissions
```

---

# قانون مهم `super()`

در Child Constructor نمی‌توانیم قبل از `super()` از `this` استفاده کنیم.

❌

```javascript
class Admin extends User {
  constructor(name) {
    this.name = name;

    super(name);
  }
}
```

این ترتیب صحیح نیست.

✔

```javascript
class Admin extends User {
  constructor(name) {
    super(name);

    this.role = 'admin';
  }
}
```

ابتدا:

```javascript
super()
```

و سپس استفاده از:

```javascript
this
```

---

# `super` فقط برای Constructor نیست

`super` در Methodهای Child نیز کاربرد دارد.

فرض کنید Parent دارای Method زیر است:

```javascript
class User {
  login() {
    console.log('User logged in');
  }
}
```

Child می‌تواند همان Method را Override کند:

```javascript
class Admin extends User {
  login() {
    super.login();
    console.log('Admin logged in');
  }
}
```

اکنون:

```javascript
const admin = new Admin();

admin.login();
```

خروجی:

```text
User logged in
Admin logged in
```

اینجا:

```javascript
super.login();
```

Method مربوط به Parent را فراخوانی می‌کند.

---

# Method Overriding

اکنون به مرحله بعدی Concept Flow می‌رسیم.

گاهی Child نمی‌خواهد دقیقاً همان Behavior Parent را داشته باشد.

مثلاً:

```text
User login
Admin login
```

ممکن است رفتار متفاوتی داشته باشند.

در این حالت Child می‌تواند Method مربوط به Parent را با تعریف Methodی با همان نام **Override** کند.

مثلاً:

```javascript
class User {
  login() {
    console.log('Logging in as user');
  }
}

class Admin extends User {
  login() {
    console.log('Logging in as admin');
  }
}
```

اکنون:

```javascript
const user = new User();
const admin = new Admin();

user.login();
admin.login();
```

خروجی:

```text
Logging in as user
Logging in as admin
```

---

# تعریف Method Overriding

**Method Overriding** یعنی Child Class یک Method موجود در Parent را با همان نام تعریف کند تا Behavior متفاوتی ارائه دهد.

مدل ذهنی:

```text
Parent
login()
  ↓
Child
login()
  ↓
Different Behavior
```

نکته مهم:

Child Method، Parent Method را حذف نمی‌کند.

بلکه هنگام استفاده از Instance مربوط به Child، Method مخصوص Child انتخاب می‌شود.

---

# Overriding بدون `super`

مثلاً:

```javascript
class User {
  login() {
    console.log('User login');
  }
}

class Admin extends User {
  login() {
    console.log('Admin login');
  }
}
```

در اینجا Child کاملاً Behavior جدیدی ارائه کرده است.

```javascript
const admin = new Admin();

admin.login();
```

خروجی:

```text
Admin login
```

Parent Method در این Call اجرا نمی‌شود.

---

# Overriding همراه با `super`

گاهی نمی‌خواهیم Behavior Parent را حذف کنیم.

می‌خواهیم آن را گسترش دهیم.

در این حالت:

```javascript
class User {
  login() {
    console.log('User login');
  }
}

class Admin extends User {
  login() {
    super.login();
    console.log('Checking admin permissions');
  }
}
```

اکنون:

```javascript
const admin = new Admin();

admin.login();
```

خروجی:

```text
User login
Checking admin permissions
```

پس دو الگو داریم:

### جایگزینی کامل

```javascript
login() {
  // new behavior
}
```

### گسترش Behavior Parent

```javascript
login() {
  super.login();

  // additional behavior
}
```

---

# Polymorphism

اکنون به مفهوم اصلی بعدی می‌رسیم.

اگر فقط Inheritance داشته باشیم، هنوز به سؤال مهمی پاسخ نداده‌ایم:

> چرا می‌توانیم Objectهای متفاوت را از طریق یک Interface رفتاری مشابه استفاده کنیم؟

اینجاست که **Polymorphism** اهمیت پیدا می‌کند.

---

## تعریف Polymorphism

### تعریف ساده

**Polymorphism** یعنی یک Interface یا Method مشترک بتواند در Objectهای مختلف، Behavior متفاوتی داشته باشد.

به بیان ساده:

> یک Method مشترک، بسته به نوع Object می‌تواند رفتار متفاوتی ارائه کند.

---

## مثال

فرض کنید یک سیستم Notification داریم.

سه نوع Notification:

```text
EmailNotification
SMSNotification
PushNotification
```

همه می‌توانند Method مشترکی به نام:

```javascript
send()
```

داشته باشند.

Parent:

```javascript
class Notification {
  send() {
    console.log('Sending notification');
  }
}
```

Childها:

```javascript
class EmailNotification extends Notification {
  send() {
    console.log('Sending email');
  }
}

class SMSNotification extends Notification {
  send() {
    console.log('Sending SMS');
  }
}
```

اکنون:

```javascript
const email = new EmailNotification();
const sms = new SMSNotification();

email.send();
sms.send();
```

خروجی:

```text
Sending email
Sending SMS
```

Method مشترک:

```javascript
send()
```

است.

اما Behavior بر اساس Object متفاوت است.

این یک نمونه ساده از Polymorphism است.

---

# چرا Polymorphism مفید است؟

فرض کنید تابعی داریم که فقط به Behavior موردنظر اهمیت می‌دهد:

```javascript
function notify(notification) {
  notification.send();
}
```

اکنون:

```javascript
notify(email);
notify(sms);
```

بدون اینکه Function بداند Object دقیقاً از چه Classی ساخته شده است، Method مناسب اجرا می‌شود.

مدل ذهنی:

```text
notify()
   ↓
send()
   ↓
EmailNotification → Email behavior
SMSNotification   → SMS behavior
```

این موضوع باعث می‌شود کد بتواند با Objectهای مختلف کار کند، بدون اینکه برای هر نوع Object منطق جداگانه‌ای در Caller بنویسد.

---

# Inheritance و Polymorphism چه رابطه‌ای دارند؟

این دو مفهوم یکسان نیستند.

**Inheritance** یک رابطه میان Classها ایجاد می‌کند.

```text
Parent
  ↓
Child
```

**Polymorphism** درباره این است که یک Interface یا Method مشترک می‌تواند در Objectهای مختلف رفتار متفاوتی داشته باشد.

```text
send()
 ↓
Email → email behavior
SMS   → SMS behavior
```

Inheritance می‌تواند یکی از راه‌های ایجاد Polymorphic Behavior باشد.

اما:

> Polymorphism مفهوم گسترده‌تری از صرفاً Inheritance است.

در این فصل تمرکز ما روی Polymorphism در چارچوب Class Inheritance و Method Overriding است.

---

# یک مثال کامل‌تر

فرض کنید در یک Application پرداخت داریم.

Parent:

```javascript
class Payment {
  process() {
    console.log('Processing payment');
  }
}
```

Child:

```javascript
class CardPayment extends Payment {
  process() {
    console.log('Processing card payment');
  }
}

class CashPayment extends Payment {
  process() {
    console.log('Processing cash payment');
  }
}
```

اکنون:

```javascript
const cardPayment = new CardPayment();
const cashPayment = new CashPayment();
```

هر دو یک Method دارند:

```javascript
process()
```

اما Behavior متفاوت است.

```javascript
cardPayment.process();
cashPayment.process();
```

خروجی:

```text
Processing card payment
Processing cash payment
```

---

# Polymorphism در یک Function

اکنون می‌توانیم یک Function عمومی بنویسیم:

```javascript
function processPayment(payment) {
  payment.process();
}
```

و:

```javascript
processPayment(cardPayment);
processPayment(cashPayment);
```

Function فقط به این قرارداد ساده نیاز دارد:

```text
payment.process()
```

اما Implementation واقعی را Object مشخص می‌کند.

این مدل ذهنی بسیار مهم است:

```text
Common Interface
       ↓
process()
       ↓
Different Implementations
       ↓
Card / Cash
```

---

# Polymorphism و کاهش شرط‌ها

بدون Polymorphism ممکن است کد به سمت چنین الگویی برود:

```javascript
function processPayment(type) {
  if (type === 'card') {
    // card logic
  } else if (type === 'cash') {
    // cash logic
  }
}
```

با مدل Polymorphic می‌توان Behavior را به Objectهای مربوط منتقل کرد:

```javascript
function processPayment(payment) {
  payment.process();
}
```

و هر Class مسئول Behavior خودش است.

این می‌تواند باعث شود مسئولیت‌ها بهتر جدا شوند.

البته این به معنی آن نیست که هر `if` یا `switch` باید با Inheritance جایگزین شود.

انتخاب طراحی باید بر اساس مسئله واقعی انجام شود.

---

# Inheritance و رابطه «is-a»

یکی از مدل‌های ذهنی مفید برای تشخیص Inheritance، رابطه‌ی:

```text
is-a
```

است.

مثلاً:

```text
Admin is a User
```

یا:

```text
CardPayment is a Payment
```

اگر Child واقعاً نوع خاصی از Parent باشد، Inheritance می‌تواند منطقی باشد.

مثلاً:

```javascript
class Admin extends User {
}
```

از نظر مدل دامنه:

```text
Admin is a User
```

اما باید دقت کنیم که صرفاً شباهت چند Property یا Method دلیل کافی برای Inheritance نیست.

---

# Inheritance همیشه بهترین انتخاب نیست

فرض کنید:

```text
Car
Engine
```

آیا:

```text
Car is an Engine
```

است؟

خیر.

Car دارای Engine است.

رابطه اینجا:

```text
Car has an Engine
```

است.

این تفاوت ما را به مفهوم دیگری می‌رساند:

**Composition**

---

# Composition

## چرا Composition؟

Inheritance برای رابطه‌ای مانند:

```text
Admin is a User
```

مناسب است.

اما بسیاری از روابط نرم‌افزاری از نوع:

```text
Object has a Component
```

هستند.

در این حالت به جای اینکه یک Class از Class دیگر ارث‌بری کند، می‌توانیم Objectهای مختلف را با ترکیب کردن Behaviorها بسازیم.

مثلاً:

```text
Order
 ├── Payment
 └── Shipping
```

Order یک Payment نیست.

Order یک Shipping نیست.

Order از این قابلیت‌ها **استفاده می‌کند**.

---

# یک مثال ساده از Composition

```javascript
class Logger {
  log(message) {
    console.log(message);
  }
}

class Order {
  constructor(logger) {
    this.logger = logger;
  }

  create() {
    this.logger.log('Order created');
  }
}
```

اکنون:

```javascript
const logger = new Logger();
const order = new Order(logger);

order.create();
```

خروجی:

```text
Order created
```

در اینجا:

```text
Order
 ↓
uses
 ↓
Logger
```

رابطه Inheritance نداریم.

```javascript
class Order extends Logger
```

ننوشته‌ایم.

بلکه Logger را به Order داده‌ایم.

این یک نمونه ساده از Composition است.

---

# Inheritance در برابر Composition

مدل ذهنی ساده:

### Inheritance

```text
Admin
  ↓ is-a
User
```

### Composition

```text
Order
  ↓ has-a / uses-a
Logger
```

بنابراین قبل از استفاده از `extends` باید از خود بپرسیم:

> آیا واقعاً رابطه‌ی «is-a» وجود دارد؟

اگر پاسخ منفی است، Composition ممکن است انتخاب مناسب‌تری باشد.

---

# چرا Inheritance می‌تواند مشکل‌ساز شود؟

Inheritance در صورت استفاده درست مفید است.

اما Hierarchyهای پیچیده می‌توانند نگهداری کد را دشوار کنند.

مثلاً:

```text
User
 ↓
Employee
 ↓
Manager
 ↓
SeniorManager
 ↓
RegionalManager
```

اکنون Behavior یک Class ممکن است به چند سطح Parent وابسته باشد.

تغییر در Parent می‌تواند روی چند Child اثر بگذارد.

بنابراین Inheritance نباید صرفاً برای حذف چند خط کد استفاده شود.

هدف باید ایجاد یک رابطه‌ی منطقی و پایدار میان مدل‌ها باشد.

---

# یک معیار مهندسی مهم

قبل از استفاده از:

```javascript
extends
```

چند سؤال بپرسید:

1. آیا Child واقعاً یک نوع از Parent است؟
2. آیا Behavior مشترک واقعاً بخشی از مدل Parent است؟
3. آیا این رابطه در آینده نیز منطقی باقی می‌ماند؟
4. آیا Child باید بتواند Contract رفتاری Parent را حفظ کند؟
5. آیا Composition مدل ساده‌تر و انعطاف‌پذیرتری ایجاد نمی‌کند؟

اگر فقط هدف:

> Code Reuse

باشد، Inheritance لزوماً بهترین ابزار نیست.

Code Reuse به‌تنهایی دلیل کافی برای ایجاد رابطه‌ی Parent/Child نیست.

---

# یک مثال مقایسه‌ای

فرض کنید چند Object باید Logging داشته باشند.

راه اول:

```javascript
class Logger {
  log(message) {
    console.log(message);
  }
}

class Order extends Logger {
}
```

اما این رابطه منطقی نیست:

```text
Order is a Logger
```

راه بهتر:

```javascript
class Order {
  constructor(logger) {
    this.logger = logger;
  }

  create() {
    this.logger.log('Order created');
  }
}
```

اکنون رابطه منطقی است:

```text
Order uses Logger
```

این همان تفکر مهندسی پشت Composition است.

---

# Inheritance و Prototype

در فصل‌های 27 و 28 دیدیم که JavaScript بر پایه‌ی Prototypeها کار می‌کند.

Class Syntax مکانیزم جداگانه‌ای برای Object Model ایجاد نمی‌کند.

وقتی می‌نویسیم:

```javascript
class Admin extends User {
}
```

JavaScript یک رابطه‌ی Prototype-based برای این Inheritance ایجاد می‌کند.

بنابراین مدل ذهنی کلی همچنان با آنچه در فصل‌های قبلی آموختیم سازگار است:

```text
Class Syntax
     ↓
Prototype-based mechanism
```

در این فصل نیازی نیست جزئیات Prototype Chain را دوباره بررسی کنیم؛ چون مکانیزم Prototype و Prototype Chain قبلاً آموزش داده شده است.

هدف فعلی این است که بدانیم `extends` و `super` چگونه روی این مدل، یک Syntax مناسب برای Class Inheritance فراهم می‌کنند.

---

# یک مثال نهایی از کل Concept Flow

اکنون تمام مفاهیم فصل را در یک مثال ترکیب کنیم.

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

Child:

```javascript
class Admin extends User {
  constructor(name, permissions) {
    super(name);

    this.permissions = permissions;
  }

  login() {
    super.login();
    console.log('Admin permissions checked');
  }

  deleteUser() {
    console.log('User deleted');
  }
}
```

ایجاد Instance:

```javascript
const admin = new Admin(
  'Omid',
  ['delete-user']
);
```

اکنون:

```javascript
admin.login();
```

خروجی:

```text
Omid logged in
Admin permissions checked
```

و:

```javascript
admin.deleteUser();
```

خروجی:

```text
User deleted
```

در این مثال:

```text
User
 ↓
Parent Class

Admin
 ↓
Child Class

extends
 ↓
Inheritance

super()
 ↓
Parent Constructor

login()
 ↓
Method Overriding

super.login()
 ↓
Parent Method

admin.login()
 ↓
Polymorphic Behavior
```

این همان مسیر مفهومی فصل است.

---

# Best Practices

## 1. برای هر Inheritance رابطه‌ی منطقی ایجاد کنید

قبل از:

```javascript
class Admin extends User
```

مطمئن شوید:

```text
Admin is a User
```

از نظر مدل دامنه درست است.

---

## 2. فقط برای Code Reuse از Inheritance استفاده نکنید

این دلیل:

> «چون دو Class Method مشترک دارند»

به‌تنهایی کافی نیست.

Shared Behavior می‌تواند با Composition یا روش‌های دیگر نیز طراحی شود.

---

## 3. Constructor Child را ساده نگه دارید

```javascript
class Admin extends User {
  constructor(name, permissions) {
    super(name);
    this.permissions = permissions;
  }
}
```

ابتدا Parent را مقداردهی کنید و سپس State مخصوص Child را تنظیم کنید.

---

## 4. از `super` آگاهانه استفاده کنید

اگر قصد گسترش Behavior Parent را دارید:

```javascript
login() {
  super.login();

  // additional behavior
}
```

اما اگر قصد جایگزینی کامل دارید، لازم نیست `super` را فراخوانی کنید.

---

## 5. از Hierarchyهای عمیق اجتناب کنید

این ساختار:

```text
A
 ↓
B
 ↓
C
 ↓
D
 ↓
E
```

می‌تواند وابستگی زیادی ایجاد کند.

Hierarchy ساده‌تر معمولاً قابل فهم‌تر و قابل نگهداری‌تر است.

---

## 6. Composition را همیشه به‌عنوان گزینه بررسی کنید

اگر رابطه:

```text
has-a
```

یا:

```text
uses-a
```

است، Composition معمولاً مدل طبیعی‌تری نسبت به Inheritance است.

---

# Common Mistakes

## اشتباه اول: تصور اینکه `extends` کد Parent را کپی می‌کند

❌

> Child تمام کد Parent را Copy می‌کند.

✔

`extends` یک رابطه‌ی Inheritance میان Classها ایجاد می‌کند و JavaScript این رابطه را با Prototype-based mechanism پیاده می‌کند.

---

## اشتباه دوم: یکی دانستن Inheritance و Polymorphism

❌

> Inheritance و Polymorphism یک مفهوم هستند.

✔

Inheritance رابطه‌ی Parent/Child است.

Polymorphism امکان ارائه‌ی Behavior متفاوت از طریق یک Interface یا Method مشترک را بیان می‌کند.

---

## اشتباه سوم: فراموش کردن `super()` در Child Constructor

❌

```javascript
class Admin extends User {
  constructor(name) {
    this.name = name;
  }
}
```

✔

```javascript
class Admin extends User {
  constructor(name) {
    super(name);
  }
}
```

در Child Constructor باید Parent Constructor با `super()` مقداردهی شود، پیش از آنکه از `this` استفاده شود.

---

## اشتباه چهارم: استفاده از `this` قبل از `super()`

❌

```javascript
constructor(name) {
  this.name = name;
  super(name);
}
```

✔

```javascript
constructor(name) {
  super(name);
  this.role = 'admin';
}
```

---

## اشتباه پنجم: تصور اینکه Override کردن Method Parent را حذف می‌کند

وقتی Child می‌نویسد:

```javascript
login() {
  console.log('Admin login');
}
```

Parent Method از بین نمی‌رود.

Child فقط Behavior خود را برای همان Method ارائه می‌کند.

در صورت نیاز می‌توان Parent Method را با:

```javascript
super.login();
```

فراخوانی کرد.

---

## اشتباه ششم: استفاده از Inheritance فقط برای حذف تکرار

دو Class ممکن است Method مشترک داشته باشند، اما این الزاماً به معنی رابطه‌ی Parent/Child نیست.

ابتدا رابطه‌ی مفهومی را بررسی کنید.

---

## اشتباه هفتم: تصور اینکه Composition نوع دیگری از `extends` است

Composition Inheritance نیست.

در Composition، Objectها با داشتن یا استفاده کردن از Objectهای دیگر ساخته می‌شوند.

مثلاً:

```javascript
class Order {
  constructor(logger) {
    this.logger = logger;
  }
}
```

اینجا Order از Logger ارث‌بری نکرده است.

---

## اشتباه هشتم: ساخت Hierarchyهای بیش از حد عمیق

Inheritance زیاد می‌تواند وابستگی میان Classها را افزایش دهد و تغییرات را دشوارتر کند.

---

## اشتباه نهم: تصور اینکه Polymorphism فقط زمانی وجود دارد که چند Child Class داشته باشیم

چند Child Class یک الگوی رایج برای نشان دادن Polymorphism است، اما اصل Polymorphism مربوط به توانایی استفاده از یک Interface یا Behavior مشترک با Implementationهای متفاوت است.

---

# Summary

Inheritance زمانی مطرح می‌شود که چند Class دارای رابطه‌ی منطقی Parent/Child باشند.

با:

```javascript
extends
```

می‌توان این رابطه را میان دو Class ایجاد کرد.

مثلاً:

```javascript
class Admin extends User {
}
```

در اینجا:

```text
User  → Parent
Admin → Child
```

Child می‌تواند Behaviorهای Parent را استفاده کند و Behaviorهای جدید اضافه کند.

اگر Child Constructor داشته باشد، برای فراخوانی Parent Constructor از:

```javascript
super()
```

استفاده می‌کنیم.

مثلاً:

```javascript
class Admin extends User {
  constructor(name, permissions) {
    super(name);

    this.permissions = permissions;
  }
}
```

در Child Constructor نباید قبل از `super()` از `this` استفاده کنیم.

`super` در Methodهای Child نیز می‌تواند برای فراخوانی Method Parent استفاده شود:

```javascript
super.login();
```

Child می‌تواند Method Parent را با تعریف Methodی با همان نام **Override** کند.

```javascript
class Admin extends User {
  login() {
    console.log('Admin login');
  }
}
```

اگر Child بخواهد Behavior Parent را نیز حفظ و گسترش دهد، می‌تواند از:

```javascript
super.login();
```

استفاده کند.

این Behaviorهای متفاوت از یک Method مشترک، ما را به مفهوم **Polymorphism** می‌رسانند.

مثلاً:

```text
Payment
   ↓
process()

CardPayment
   ↓
card behavior

CashPayment
   ↓
cash behavior
```

Caller می‌تواند فقط به:

```javascript
payment.process()
```

تکیه کند و لازم نباشد Implementation داخلی هر نوع Payment را بشناسد.

در نهایت، Inheritance همیشه بهترین انتخاب برای Code Reuse نیست.

اگر رابطه از نوع:

```text
is-a
```

باشد، Inheritance می‌تواند مناسب باشد.

اگر رابطه بیشتر از نوع:

```text
has-a
uses-a
```

باشد، **Composition** ممکن است مدل مناسب‌تری باشد.

هدف اصلی طراحی خوب، استفاده از `extends` نیست.

هدف، ایجاد رابطه‌ای است که مدل نرم‌افزار را ساده‌تر، قابل فهم‌تر و قابل نگهداری‌تر کند.

---

# Key Takeaways

* **Inheritance** رابطه‌ای میان Parent Class و Child Class ایجاد می‌کند.
* `extends` برای ایجاد Class Inheritance استفاده می‌شود.
* Parent Class معمولاً Behavior عمومی‌تر را ارائه می‌کند.
* Child Class می‌تواند Behavior Parent را استفاده کند.
* Child Class می‌تواند Behavior جدید اضافه کند.
* Inheritance به معنی Copy شدن Source Code Parent نیست.
* JavaScript Class Inheritance بر پایه‌ی Prototype mechanism عمل می‌کند.
* `super()` برای فراخوانی Parent Constructor استفاده می‌شود.
* در Child Constructor باید قبل از استفاده از `this`، `super()` اجرا شود.
* `super.method()` برای فراخوانی Method مربوط به Parent استفاده می‌شود.
* **Method Overriding** یعنی Child Behavior متفاوتی برای Method Parent ارائه کند.
* Override می‌تواند Behavior Parent را کاملاً جایگزین کند یا با `super` آن را گسترش دهد.
* **Polymorphism** اجازه می‌دهد یک Interface یا Method مشترک، Behaviorهای متفاوتی ارائه کند.
* Inheritance و Polymorphism یک مفهوم نیستند.
* Inheritance معمولاً با رابطه‌ی `is-a` توجیه می‌شود.
* Composition بیشتر برای روابط `has-a` یا `uses-a` مناسب است.
* Code Reuse به‌تنهایی دلیل کافی برای استفاده از Inheritance نیست.
* Hierarchyهای عمیق می‌توانند وابستگی و پیچیدگی را افزایش دهند.
* انتخاب میان Inheritance و Composition یک تصمیم طراحی است، نه صرفاً یک تصمیم Syntax.

---
Technical Interview

سطح پایه (Junior)

سؤال ۱

Inheritance چیست؟

پاسخ:

Inheritance مکانیزمی برای ایجاد رابطه‌ی Parent/Child میان Classهاست
که به Child اجازه می‌دهد Behaviorهای Parent را استفاده کند و در صورت نیاز
Behaviorهای جدید اضافه یا Behaviorهای موجود را Override کند.

سؤال ۲

extends چه کاری انجام می‌دهد؟

پاسخ:

extends یک رابطه‌ی Inheritance میان دو Class ایجاد می‌کند و Child را به
Parent متصل می‌سازد.

class Admin extends User {
}

در اینجا Admin، Child و User، Parent است.

سؤال ۳

Parent Class و Child Class چیستند؟

پاسخ:

Parent Class Class عمومی‌تری است که Behavior یا State مشترک را تعریف
می‌کند.

Child Class Class تخصصی‌تری است که از Parent ارث‌بری می‌کند و می‌تواند
Behaviorهای Parent را استفاده کند، Behavior جدید اضافه کند یا Behavior
موجود را Override کند.

سؤال ۴

super() چه کاری انجام می‌دهد؟

پاسخ:

super() در Child Constructor، Constructor مربوط به Parent را فراخوانی
می‌کند تا بخش مربوط به Parent از Initialization انجام شود.

class Admin extends User {
constructor(name) {
super(name);
}
}

همچنین super.method() برای فراخوانی Method مربوط به Parent استفاده
می‌شود.

سؤال ۵

چرا در Child Constructor باید قبل از this از super() استفاده کنیم؟

پاسخ:

در یک Derived Class، قبل از اجرای super() هنوز this برای Child
آماده نشده است. بنابراین استفاده از this قبل از super() باعث
ReferenceError می‌شود.

class Admin extends User {
constructor(name) {
super(name);

    this.role = 'admin';
}
}

ابتدا Parent Constructor با super() اجرا می‌شود و سپس Child می‌تواند از
this استفاده کند.

سؤال ۶

Method Overriding چیست؟

پاسخ:

Method Overriding یعنی Child Class یک Method موجود در Parent را با
همان نام تعریف کند تا Behavior متفاوتی برای آن ارائه دهد.

class User {
login() {
console.log('User login');
}
}

class Admin extends User {
login() {
console.log('Admin login');
}
}

در این حالت admin.login()، Behavior تعریف‌شده در Admin را اجرا می‌کند.

سؤال ۷

Polymorphism چیست؟

پاسخ:

Polymorphism یعنی یک Interface یا Method مشترک بتواند در Objectهای
مختلف، Behavior متفاوتی داشته باشد.

class CardPayment extends Payment {
process() {
console.log('Processing card payment');
}
}

class CashPayment extends Payment {
process() {
console.log('Processing cash payment');
}
}

هر دو Object از Method مشترک process() استفاده می‌کنند، اما
Implementation آن‌ها متفاوت است.

سؤال ۸

تفاوت Inheritance و Composition چیست؟

پاسخ:

در Inheritance یک رابطه‌ی Parent/Child یا معمولاً is-a ایجاد می‌شود.

Admin is a User

در Composition یک Object از Object یا Component دیگری استفاده می‌کند
و رابطه معمولاً has-a یا uses-a است.

Order uses Logger

Inheritance برای مدل‌کردن رابطه‌ی واقعی میان انواع Objects مناسب است؛
Composition برای ترکیب قابلیت‌های مستقل معمولاً انعطاف‌پذیرتر است.

سطح متوسط (Mid-Level)

سؤال ۹

چرا ممکن است به جای تکرار Behavior در چند Class از Inheritance استفاده
کنیم؟

پاسخ:

اگر چند Class واقعاً Behavior مشترکی داشته باشند و رابطه‌ی منطقی
Parent/Child میان آن‌ها وجود داشته باشد، Inheritance اجازه می‌دهد Behavior
مشترک در یک Parent تعریف شود و Childها آن را استفاده کنند.

در نتیجه Behavior مشترک در چند Class تکرار نمی‌شود و تغییر آن نیز می‌تواند
در یک نقطه انجام شود.

اما هدف نباید صرفاً کاهش خطوط کد باشد؛ رابطه‌ی Inheritance باید از نظر مدل
دامنه نیز منطقی باشد.

سؤال ۱۰

تفاوت Override کردن یک Method با فراخوانی Parent Method با super چیست؟

پاسخ:

در Override، Child یک Method با همان نام تعریف می‌کند و Behavior
مربوط به Child هنگام فراخوانی استفاده می‌شود.

class Admin extends User {
login() {
console.log('Admin login');
}
}

اما با:

super.login();

Child صراحتاً Method مربوط به Parent را فراخوانی می‌کند.

بنابراین Override می‌تواند Behavior Parent را جایگزین کند، در حالی که
super امکان استفاده از Behavior Parent را در Child فراهم می‌کند.

سؤال ۱۱

چگونه یک Child Class می‌تواند Behavior Parent را گسترش دهد؟

پاسخ:

Child می‌تواند Method Parent را Override کند و داخل آن، ابتدا Behavior
Parent را با super.method() اجرا کند و سپس Behavior جدید خود را اضافه
کند.

class Admin extends User {
login() {
super.login();
console.log('Checking admin permissions');
}
}

در این حالت Behavior Parent حذف نشده است؛ بلکه Child آن را گسترش داده
است.

سؤال ۱۲

چرا Code Reuse به‌تنهایی دلیل مناسبی برای استفاده از Inheritance نیست؟

پاسخ:

زیرا Inheritance فقط Code Reuse ایجاد نمی‌کند؛ بلکه یک رابطه‌ی ساختاری و
وابستگی میان Classها ایجاد می‌کند.

اگر دو Class صرفاً چند Method مشترک داشته باشند، این شباهت الزاماً به معنی
رابطه‌ی is-a نیست.

استفاده از Inheritance فقط برای Reuse می‌تواند Hierarchy نامناسب،
Coupling بیشتر و وابستگی‌های سخت برای تغییر ایجاد کند.

سؤال ۱۳

چگونه تشخیص می‌دهید یک رابطه برای Inheritance مناسب است؟

پاسخ:

ابتدا بررسی می‌کنم آیا Child واقعاً یک نوع تخصصی از Parent است یا خیر:

Child is a Parent

سپس بررسی می‌کنم Behavior مشترک واقعاً بخشی از مدل Parent باشد، رابطه در
آینده نیز منطقی بماند و Child بتواند Contract رفتاری Parent را حفظ کند.

اگر رابطه بیشتر از نوع has-a یا uses-a باشد، Composition معمولاً
گزینه‌ی مناسب‌تری است.

سؤال ۱۴

Polymorphism چگونه می‌تواند تعداد شرط‌های موجود در Code را کاهش دهد؟

پاسخ:

به جای اینکه یک Function نوع Object را بررسی کند و برای هر نوع شرط
جداگانه داشته باشد:

if (type === 'card') {
// ...
} else if (type === 'cash') {
// ...
}

می‌توان Behavior مشترکی مانند process() تعریف کرد و Implementation هر
نوع Object را به خودش سپرد:

function processPayment(payment) {
payment.process();
}

در این حالت Caller فقط به Interface رفتاری مشترک وابسته است و لازم نیست
نوع دقیق Object را بررسی کند.

سؤال ۱۵

چرا Composition ممکن است از Inheritance انعطاف‌پذیرتر باشد؟

پاسخ:

Composition به جای ایجاد یک Hierarchy ثابت، قابلیت‌ها را از طریق ترکیب
Objectها در کنار یکدیگر قرار می‌دهد.

یک Object می‌تواند از چند Component مستقل استفاده کند، بدون اینکه مجبور
باشد در یک زنجیره‌ی Parent/Child قرار بگیرد.

این مدل معمولاً Coupling کمتری ایجاد می‌کند و تغییر یا جایگزینی یک
Component را ساده‌تر می‌سازد.

سطح پیشرفته (Senior)

سؤال ۱۶

آیا extends در JavaScript به معنی Copy شدن Methodهای Parent داخل Child
است؟

پاسخ:

خیر.

extends Source Code یا Methodهای Parent را داخل Child کپی نمی‌کند؛ بلکه
یک رابطه‌ی Inheritance ایجاد می‌کند.

در Class Syntax، این رابطه بر پایه‌ی Prototype mechanism زبان
JavaScript شکل می‌گیرد و Methodهای Inherited از طریق زنجیره‌ی Prototype
قابل دسترسی هستند.

بنابراین Inheritance را باید یک رابطه دانست، نه یک عملیات Copy.

سؤال ۱۷

رابطه‌ی Inheritance در Class Syntax چگونه با Prototype mechanism ارتباط
دارد؟

پاسخ:

class و extends Syntax سطح بالاتری برای کار با Object Model
جاوااسکریپت ارائه می‌کنند، اما مکانیزم زیربنایی Objectها همچنان
Prototype-based است.

وقتی می‌نویسیم:

class Admin extends User {
}

JavaScript رابطه‌ای Prototype-based میان Classها و Instanceهای آن‌ها ایجاد
می‌کند.

بنابراین Class Syntax یک Object Model مستقل از Prototypeها ایجاد نمی‌کند؛
بلکه Syntax خواناتری برای کار با همان مکانیزم زبان فراهم می‌کند.

سؤال ۱۸

چرا Hierarchyهای عمیق Inheritance می‌توانند مشکل طراحی ایجاد کنند؟

پاسخ:

در Hierarchy عمیق، Behavior یک Child ممکن است به چند سطح Parent وابسته
شود.

User
↓
Employee
↓
Manager
↓
SeniorManager
↓
RegionalManager

در چنین ساختاری تغییر در یک Parent می‌تواند روی چند Child اثر بگذارد.

همچنین فهمیدن اینکه یک Behavior دقیقاً از کدام سطح به ارث رسیده است
دشوارتر می‌شود.

در نتیجه Coupling افزایش می‌یابد و تغییر، Debugging و نگهداری سیستم
پیچیده‌تر می‌شود.

سؤال ۱۹

چه زمانی Composition را به Inheritance ترجیح می‌دهید؟

پاسخ:

زمانی که رابطه‌ی واقعی میان Objects از نوع has-a یا uses-a باشد، یا
زمانی که یک قابلیت می‌تواند مستقل از Hierarchy اصلی تغییر یا جایگزین شود،
Composition را ترجیح می‌دهم.

مثلاً اگر Order به Logger نیاز داشته باشد، منطقی‌تر است:

class Order {
constructor(logger) {
this.logger = logger;
}
}

تا اینکه:

class Order extends Logger {
}

در حالت اول، Order از Logger استفاده می‌کند؛ در حالت دوم به‌اشتباه ادعا
می‌کنیم Order یک Logger است.

سؤال ۲۰

Polymorphism چه ارزش مهندسی‌ای برای طراحی نرم‌افزار دارد؟

پاسخ:

Polymorphism اجازه می‌دهد Caller به یک Interface یا Behavior مشترک وابسته
باشد، در حالی که Implementation واقعی توسط Object مشخص می‌شود.

برای مثال:

function processPayment(payment) {
payment.process();
}

این Function می‌تواند با انواع مختلف Payment کار کند، بدون اینکه منطق
داخلی هر نوع را بشناسد.

نتیجه، کاهش وابستگی به Typeهای مشخص، کاهش شرط‌های مربوط به نوع Object و
امکان اضافه‌کردن Implementationهای جدید با تغییر کمتر در Caller است.

بنابراین ارزش اصلی Polymorphism در Separation of Concerns، کاهش
Coupling و افزایش قابلیت توسعه است.
---

# Golden Answers

## Inheritance چیست؟

Inheritance مکانیزمی برای ایجاد رابطه‌ی Parent/Child میان Classهاست که به Child اجازه می‌دهد Behaviorهای Parent را استفاده کرده و در صورت نیاز آن‌ها را گسترش یا Override کند.

---

## `extends` چه کاری انجام می‌دهد؟

`extends` یک رابطه‌ی Inheritance میان دو Class ایجاد می‌کند و Child را به Parent متصل می‌سازد.

مثلاً:

```javascript
class Admin extends User {}
```

در اینجا `Admin` Child و `User` Parent است.

---

## Parent Class و Child Class چیستند؟

Parent Class Class عمومی‌تر است که Behavior یا State مشترک را تعریف می‌کند.

Child Class Class تخصصی‌تری است که از Parent ارث‌بری می‌کند و می‌تواند Behaviorهای جدید اضافه یا Behaviorهای موجود را Override کند.

---

## `super()` چه کاری انجام می‌دهد؟

`super()` در Child Constructor، Parent Constructor را فراخوانی می‌کند.

مثلاً:

```javascript
class Admin extends User {
  constructor(name) {
    super(name);
  }
}
```

---

## چرا قبل از `this` باید `super()` اجرا شود؟

در Derived Class، Instance باید ابتدا از مسیر Parent initialization شود. بنابراین استفاده از `this` در Child Constructor پیش از اجرای `super()` مجاز نیست.

---

## Method Overriding چیست؟

Method Overriding زمانی رخ می‌دهد که Child Class Methodی با همان نام Method Parent تعریف کند و Behavior متفاوتی ارائه دهد.

---

## Polymorphism چیست؟

Polymorphism یعنی یک Interface یا Method مشترک بتواند در Objectهای مختلف Behavior متفاوتی داشته باشد.

مثلاً:

```javascript
payment.process();
```

می‌تواند برای انواع مختلف Payment، Implementation متفاوتی اجرا کند.

---

## تفاوت Inheritance و Composition چیست؟

Inheritance رابطه‌ی:

```text
is-a
```

را مدل می‌کند.

Composition رابطه‌هایی مانند:

```text
has-a
uses-a
```

را مدل می‌کند.

مثلاً:

```text
Admin is a User
```

می‌تواند Inheritance باشد.

اما:

```text
Order uses Logger
```

بیشتر با Composition مدل می‌شود.

---

## چرا Code Reuse به‌تنهایی دلیل مناسبی برای Inheritance نیست؟

زیرا Inheritance فقط Code Reuse ایجاد نمی‌کند؛ یک وابستگی و رابطه‌ی Parent/Child نیز ایجاد می‌کند.

اگر رابطه‌ی مفهومی صحیح نباشد، صرفاً برای حذف Code Duplication ایجاد کردن Inheritance می‌تواند Coupling و پیچیدگی آینده را افزایش دهد.

---

## چگونه Behavior Parent را در Child گسترش می‌دهیم؟

با Override کردن Method و سپس فراخوانی Parent Method با `super`:

```javascript
class Admin extends User {
  login() {
    super.login();
    console.log('Admin permissions checked');
  }
}
```

---

## Polymorphism چگونه می‌تواند شرط‌ها را کاهش دهد؟

به جای بررسی نوع Object:

```javascript
if (type === 'card') {
  // ...
} else if (type === 'cash') {
  // ...
}
```

می‌توان Behavior را به Class مربوط منتقل کرد:

```javascript
function processPayment(payment) {
  payment.process();
}
```

هر Implementation، `process()` مخصوص خود را ارائه می‌کند.

---

## آیا `extends` باعث Copy شدن Methodهای Parent می‌شود؟

خیر.

`extends` یک رابطه‌ی Inheritance ایجاد می‌کند.

Methodهای Class در Prototypeهای مربوط به Class قرار می‌گیرند و JavaScript از Prototype-based mechanism برای این رابطه استفاده می‌کند.

بنابراین `extends` را نباید مانند Copy/Paste کد Parent در Child تصور کرد.

---

## رابطه‌ی Class Inheritance با Prototype چیست؟

Class Syntax یک لایه‌ی Syntax برای کار با Object Model جاوااسکریپت فراهم می‌کند.

در Inheritance، رابطه‌ی Classها در نهایت با Prototype mechanism پیاده می‌شود.

بنابراین:

```javascript
class Admin extends User {}
```

مستقل از Prototype model نیست.

---

## چرا Hierarchyهای عمیق مشکل‌ساز هستند؟

زیرا Child به Parentهای خود وابسته می‌شود.

در یک Hierarchy عمیق:

```text
A
 ↓
B
 ↓
C
 ↓
D
```

تغییر در یک سطح می‌تواند Behavior سطوح پایین‌تر را تحت تأثیر قرار دهد.

همچنین درک منشأ یک Behavior دشوارتر می‌شود.

---

## چه زمانی Composition را به Inheritance ترجیح می‌دهید؟

وقتی رابطه‌ی واقعی میان Objects بیشتر از نوع:

```text
has-a
```

یا:

```text
uses-a
```

باشد.

همچنین زمانی که می‌خواهیم قابلیت‌ها را مستقل‌تر و انعطاف‌پذیرتر ترکیب کنیم، Composition می‌تواند انتخاب مناسب‌تری باشد.

---

## Polymorphism چه ارزش مهندسی‌ای دارد؟

Polymorphism اجازه می‌دهد Caller به یک Behavior مشترک وابسته باشد، نه به Implementation مشخص.

مثلاً:

```javascript
function processPayment(payment) {
  payment.process();
}
```

در این مدل، Function نیازی ندارد بداند Payment از چه نوعی است.

این موضوع می‌تواند Coupling را کاهش داده و طراحی را قابل توسعه‌تر کند.

---

# Conclusion

در فصل‌های قبلی، ابتدا Objectها را شناختیم، سپس Prototype و Prototype Chain را بررسی کردیم، Constructor Function را برای ایجاد Instanceهای مشابه آموختیم و در نهایت با ES Classes یک Syntax مدرن برای مدل‌سازی Objectها به دست آوردیم.

اکنون یک مرحله جلوتر رفتیم.

با:

```javascript
extends
```

یک Child Class می‌تواند Behavior مشترک Parent را استفاده کند.

با:

```javascript
super()
```

می‌توان Parent Constructor را فراخوانی کرد.

با:

```javascript
super.method()
```

می‌توان Behavior Parent را در Child گسترش داد.

با **Method Overriding**، Child می‌تواند Behavior متفاوتی برای یک Method مشترک ارائه کند.

و زمانی که Objectهای مختلف از یک Method مشترک، Behaviorهای متفاوتی ارائه می‌کنند، به مفهوم **Polymorphism** می‌رسیم.

اما مهم‌ترین نتیجه این فصل صرفاً یادگیری:

```javascript
extends
super
```

نیست.

نکته‌ی مهندسی مهم‌تر این است که **Inheritance یک ابزار طراحی است، نه صرفاً یک ابزار برای جلوگیری از تکرار کد.**

اگر رابطه‌ی واقعی:

```text
is-a
```

باشد، Inheritance می‌تواند انتخاب مناسبی باشد.

اما اگر رابطه:

```text
has-a
uses-a
```

باشد، Composition ممکن است مدل بهتری ارائه کند.

بنابراین هنگام طراحی Classها، سؤال اصلی این نیست که:

> «چگونه می‌توانم با `extends` کد کمتری بنویسم؟»

بلکه سؤال مهندسی این است:

> **«چه رابطه‌ای میان این Objects واقعاً وجود دارد و کدام مدل، آن رابطه را بهتر بیان می‌کند؟»**

این دیدگاه، Inheritance و Polymorphism را از یک Syntax ساده به یک ابزار واقعی برای طراحی نرم‌افزار تبدیل می‌کند.
