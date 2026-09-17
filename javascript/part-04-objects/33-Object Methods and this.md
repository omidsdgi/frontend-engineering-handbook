# Chapter 20 — Object Methods and this

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* توضیح دهید چرا Object علاوه بر Data می‌تواند Behavior نیز داشته باشد.
* تفاوت Function Property و Method را درک کنید.
* Method را به‌درستی Invocation کنید.
* توضیح دهید چرا نحوه Invocation یک Function در تعیین `this` اهمیت دارد.
* مفهوم `this` را در یک Method توضیح دهید.
* Implicit Binding را تشخیص دهید.
* تفاوت رفتار Regular Function و Arrow Function را در رابطه با `this` توضیح دهید.
* تشخیص دهید چرا Arrow Function معمولاً انتخاب مناسبی برای Methodای که به `this` نیاز دارد نیست.
* Methodهایی طراحی کنید که Behavior مرتبط با Object را مدل کنند.
* مفهوم Method Chaining را در حد مقدماتی درک کنید.
* خطاهای رایج مربوط به `this` و Method Invocation را تشخیص دهید.

## Core Question

Object چگونه رفتار را از طریق Methods و `this` مدل می‌کند؟

برای پاسخ به این سؤال، از مفهومی که در فصل قبل معرفی کردیم شروع می‌کنیم:

Object می‌تواند یک Function را به‌عنوان Value یک Property نگهداری کند.

از اینجا یک سؤال طبیعی شکل می‌گیرد:

اگر این Function رفتار مرتبط با Object را نشان می‌دهد، چگونه باید آن را فراخوانی کنیم؟

پاسخ ما را به مفهوم **Method Invocation** می‌رساند.

سپس باید بفهمیم Function هنگام فراخوانی از طریق یک Object، چگونه خود Object را تشخیص می‌دهد.

اینجاست که `this` وارد مدل ذهنی ما می‌شود.

مسیر فصل چنین است:

```text
Object
↓
Function Property
↓
Method
↓
Method Invocation
↓
this
↓
Implicit Binding
↓
Arrow Function Difference
↓
Method Patterns
```

---

## مقدمه

در فصل قبل دیدیم که Object فقط برای نگهداری Data نیست.

یک Object می‌تواند Behavior مرتبط با آن Data را نیز در خود نگهداری کند.

برای مثال:

```js
const account = {
  owner: 'Omid',
  balance: 5000,

  deposit: function (amount) {
    console.log(amount);
  }
};
```

در این Object:

```text
account
├── owner   → 'Omid'
├── balance → 5000
└── deposit → Function
```

Propertyهای `owner` و `balance` داده را توصیف می‌کنند.

`deposit` رفتاری مرتبط با Account را نشان می‌دهد.

اما هنوز یک سؤال مهم باقی مانده است:

```text
وقتی deposit اجرا می‌شود،
از کجا می‌فهمد به کدام Account مربوط است؟
```

این سؤال، ما را به `this` می‌رساند.

در این فصل هدف ما حفظ کردن چند Rule پراکنده نیست.

می‌خواهیم یک مدل ذهنی ساده بسازیم:

> **در یک Method Call، Object سمت چپ Dot مشخص می‌کند `this` به کدام Object اشاره کند.**

این همان چیزی است که به آن **Implicit Binding** می‌گوییم.

---

# Function Property

در فصل 15 یاد گرفتیم که Function در JavaScript یک Value است.

بنابراین Function می‌تواند در یک Variable قرار بگیرد.

همچنین می‌تواند Value یک Property باشد:

```js
const user = {
  name: 'Omid',

  login: function () {
    console.log('User logged in');
  }
};
```

در اینجا:

```text
user
├── name  → 'Omid'
└── login → Function
```

از دید ساختاری، `login` یک Property است.

Value این Property یک Function است.

پس در ساده‌ترین مدل:

> **Function Property یعنی Propertyای که Value آن یک Function است.**

این مفهوم مستقیماً از مباحث First-Class Functions و Function as Value که قبلاً یاد گرفتیم به‌دست می‌آید.

---

## Function Property در عمل

فرض کنید یک Product داریم:

```js
const product = {
  name: 'Laptop',
  price: 1200,

  showPrice: function () {
    console.log(this.price);
  }
};
```

Object شامل دو نوع اطلاعات است:

```text
Product
├── Data
│   ├── name
│   └── price
│
└── Behavior
    └── showPrice
```

`showPrice` رفتاری مرتبط با Product است.

این همان نقطه‌ای است که Function Property را به **Method** تبدیل می‌کند.

---

# Method

وقتی یک Function به‌عنوان Property یک Object برای مدل کردن Behavior مرتبط با آن Object استفاده می‌شود، معمولاً آن را **Method** می‌نامیم.

برای مثال:

```js
const account = {
  owner: 'Omid',

  showOwner: function () {
    console.log(this.owner);
  }
};
```

`showOwner` یک Method است.

چرا؟

زیرا این Function رفتار مرتبط با `account` را مدل می‌کند.

مدل ذهنی:

```text
Account
├── owner
└── showOwner()
```

بنابراین:

> **Property می‌تواند Data را نگهداری کند یا Behavior را از طریق یک Function مدل کند.**

اما یک تفاوت مهم وجود دارد.

هر Functionای که داخل Object قرار گرفته، لزوماً از نظر طراحی یک Method معنادار نیست.

مثلاً:

```js
const config = {
  version: '1.0',
  log: console.log
};
```

از نظر ساختاری `log` یک Function Property است.

اما در طراحی Application، Method زمانی معنا پیدا می‌کند که Behavior آن با Entity یا مسئولیت Object مرتبط باشد.

---

## Function Property در مقابل Method

برای مثال:

```js
const account = {
  owner: 'Omid',

  deposit: function (amount) {
    this.balance += amount;
  },

  balance: 5000
};
```

اینجا:

```text
balance → Data
deposit → Behavior
```

پس `deposit` یک Method است.

Method به ما اجازه می‌دهد Data و Behavior مرتبط را در یک ساختار نگه داریم:

```text
Account
│
├── owner
├── balance
│
└── deposit()
```

این دقیقاً ادامه منطقی مفهوم Object از فصل قبل است.

در آن فصل Object را برای سازمان‌دهی Data معرفی کردیم.

اکنون Object می‌تواند Behavior مرتبط با همان Data را نیز مدل کند.

---

# Method Invocation

داشتن یک Method کافی نیست.

باید آن را اجرا کنیم.

برای مثال:

```js
const account = {
  owner: 'Omid',

  showOwner: function () {
    console.log(this.owner);
  }
};
```

برای فراخوانی Method می‌نویسیم:

```js
account.showOwner();
```

این عبارت یک **Method Invocation** است.

در اینجا Function از طریق Object فراخوانی شده است:

```text
account
   ↓
showOwner()
```

نکته مهم این است که:

```js
account.showOwner();
```

فقط یک Function Call معمولی نیست.

نحوه فراخوانی Function اطلاعات مهمی درباره `this` در اختیار JavaScript قرار می‌دهد.

---

## چرا نحوه Invocation مهم است؟

به این دو حالت توجه کنید:

```js
account.showOwner();
```

و:

```js
const showOwner = account.showOwner;

showOwner();
```

در حالت اول Function از طریق `account` فراخوانی شده است.

در حالت دوم Function را از Object جدا کرده‌ایم و سپس آن را به‌صورت مستقل فراخوانی کرده‌ایم.

بنابراین این دو Invocation از نظر رابطه با Object یکسان نیستند.

این تفاوت برای درک `this` بسیار مهم است.

مدل ذهنی:

```text
account.showOwner()
        ↑
Object در Invocation حضور دارد
```

اما:

```text
showOwner()
    ↑
Function مستقل فراخوانی شده است
```

پس نباید فقط به محل تعریف Function نگاه کنیم.

باید به **نحوه فراخوانی آن** نیز توجه کنیم.

---

# this

اکنون به مفهوم اصلی فصل می‌رسیم.

`this` یک Keyword است که در یک Function به یک Object یا Context مربوط به Invocation آن Function اشاره می‌کند.

اما برای سطح این فصل، یک Rule مهم‌تر داریم:

> **مقدار `this` برای یک Regular Function به نحوه فراخوانی Function وابسته است.**

به این مثال نگاه کنید:

```js
const account = {
  owner: 'Omid',

  showOwner: function () {
    console.log(this.owner);
  }
};

account.showOwner();
```

خروجی:

```text
Omid
```

چرا؟

چون Function به‌صورت:

```js
account.showOwner();
```

فراخوانی شده است.

در این Invocation، Object سمت چپ Dot یعنی:

```js
account
```

Object مرتبط با Method Call است.

بنابراین:

```js
this === account
```

در نتیجه:

```js
this.owner
```

معادل مفهومی:

```js
account.owner
```

است.

---

## مدل ذهنی `this`

بهتر است این Rule را به‌صورت تصویری ببینیم:

```text
account.showOwner()
        │
        └── Object مرتبط با Call
                 │
                 ↓
                this
```

بنابراین:

```js
this.owner
```

به:

```js
account.owner
```

می‌رسد.

این نکته بسیار مهم است:

> `this` در یک Method به Objectای اشاره می‌کند که Method از طریق آن فراخوانی شده است.

---

## `this` نام Object نیست

یکی از سوءبرداشت‌های رایج این است که تصور کنیم `this` همیشه نام Objectای است که Function داخل آن نوشته شده است.

اما `this` نام Object نیست.

برای مثال:

```js
const account = {
  owner: 'Omid',

  showOwner: function () {
    console.log(this.owner);
  }
};
```

اینجا نمی‌توانیم بگوییم:

> `this` یعنی account.

تعریف دقیق‌تر این است:

> در این Invocation خاص، `this` به `account` اشاره می‌کند.

زیرا Method به شکل زیر فراخوانی شده است:

```js
account.showOwner();
```

این تفاوت زمانی مهم می‌شود که همان Function را از Object جدا کنیم یا آن را از طریق Object دیگری فراخوانی کنیم.

---

# Implicit Binding

وقتی یک Method به شکل:

```js
object.method();
```

فراخوانی می‌شود، Object سمت چپ Dot به‌صورت ضمنی به‌عنوان `this` برای آن Call در نظر گرفته می‌شود.

این رفتار را **Implicit Binding** می‌نامیم.

مثال:

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(`Hello, ${this.name}`);
  }
};

user.greet();
```

در اینجا:

```js
this === user
```

بنابراین:

```js
this.name
```

به:

```js
user.name
```

دسترسی پیدا می‌کند.

خروجی:

```text
Hello, Omid
```

---

## چرا Implicit Binding مفید است؟

بدون `this` مجبور می‌شدیم Object را مستقیماً داخل Method نام ببریم:

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(`Hello, ${user.name}`);
  }
};
```

این طراحی یک وابستگی غیرضروری ایجاد می‌کند.

Method به نام مشخص Object وابسته می‌شود.

اما با `this`:

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(`Hello, ${this.name}`);
  }
};
```

Method به Objectای که آن را فراخوانی می‌کند وابسته می‌شود.

این موضوع قابلیت استفاده مجدد Method را بهتر می‌کند.

---

# یک Method، چند Object

این ایده زمانی واضح‌تر می‌شود که یک Function را میان Objectها به اشتراک بگذاریم.

فرض کنید:

```js
const showBalance = function () {
  console.log(this.balance);
};

const account1 = {
  balance: 5000,
  showBalance
};

const account2 = {
  balance: 12000,
  showBalance
};
```

اکنون:

```js
account1.showBalance();
```

خروجی:

```text
5000
```

و:

```js
account2.showBalance();
```

خروجی:

```text
12000
```

چرا همان Function دو نتیجه متفاوت دارد؟

چون Function یکی است، اما نحوه Invocation متفاوت است.

در Call اول:

```js
account1.showBalance();
```

داریم:

```js
this === account1
```

در Call دوم:

```js
account2.showBalance();
```

داریم:

```js
this === account2
```

مدل ذهنی:

```text
                showBalance
                   │
          ┌────────┴────────┐
          ↓                 ↓
   account1 Call      account2 Call
          │                 │
          ↓                 ↓
      this=account1     this=account2
```

این یکی از مهم‌ترین کاربردهای `this` است.

Behavior می‌تواند مشترک باشد، درحالی‌که Objectی که Behavior روی آن اجرا می‌شود متفاوت باشد.

---

# Arrow Functions و `this`

اکنون باید تفاوت مهمی را بررسی کنیم.

در Chapter 13 با Arrow Function آشنا شدیم:

```js
const greet = () => {
  console.log('Hello');
};
```

Arrow Function از نظر `this` مانند Regular Function رفتار نمی‌کند.

> **Arrow Function `this` مستقل خود را ندارد.**

در عوض، `this` را از محیط Lexical اطراف خود می‌گیرد.

این رفتار را می‌توان با عبارت:

**Lexical `this`**

توصیف کرد.

---

## چرا Arrow Function به‌عنوان Method متفاوت است؟

این Object را ببینید:

```js
const user = {
  name: 'Omid',

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

ممکن است انتظار داشته باشیم:

```text
Omid
```

اما این انتظار صحیح نیست.

چرا؟

زیرا Arrow Function برای Method Call، `this` جدیدی ایجاد نمی‌کند.

این:

```js
user.greet();
```

باعث نمی‌شود Arrow Function به‌صورت خودکار:

```js
this === user
```

داشته باشد.

Arrow Function `this` را از محیطی که در آن تعریف شده دریافت می‌کند.

بنابراین:

> **قرار دادن Arrow Function در یک Object باعث نمی‌شود `this` آن Object باشد.**

---

# Regular Function در مقابل Arrow Function

مقایسه مستقیم:

### Regular Function

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(this.name);
  }
};

user.greet();
```

در این Call:

```js
this === user
```

بنابراین:

```text
Omid
```

نمایش داده می‌شود.

### Arrow Function

```js
const user = {
  name: 'Omid',

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

اینجا Arrow Function `this` را از Invocation نمی‌گیرد.

`this` آن Lexical است.

پس اگر Method به `this` نیاز دارد، Arrow Function انتخاب مناسبی نیست.

---

# چرا Arrow Function این رفتار را دارد؟

Arrow Function برای داشتن یک `this` وابسته به Invocation طراحی نشده است.

به‌جای آن، `this` را از Scope اطراف خود دریافت می‌کند.

این ویژگی در موقعیت‌هایی که می‌خواهیم Function داخلی همان `this` بیرونی را حفظ کند، بسیار مفید است.

برای مثال:

```js
const user = {
  name: 'Omid',

  greet: function () {
    const show = () => {
      console.log(this.name);
    };

    show();
  }
};

user.greet();
```

در اینجا `show` یک Arrow Function است.

`show` `this` مستقل خود را ندارد.

بنابراین `this` را از Function بیرونی دریافت می‌کند.

مدل ذهنی:

```text
user.greet()
      │
      ↓
 this === user
      │
      ↓
   show()
      │
      ↓
Arrow Function
      │
      ↓
همان this بیرونی
```

این رفتار در کدهای واقعی بسیار کاربردی است.

اما باید یک تفاوت اساسی را به خاطر داشته باشیم:

> Arrow Function برای Methodای که باید `this` را از Method Call دریافت کند مناسب نیست؛ اما برای Function داخلی‌ای که باید `this` بیرونی را حفظ کند می‌تواند بسیار مناسب باشد.

---

# Method Invocation و جدا کردن Method

یکی از خطاهای مهم زمانی رخ می‌دهد که Method را از Object جدا کنیم.

مثال:

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(this.name);
  }
};

user.greet();
```

این یک Method Call است.

اما:

```js
const greet = user.greet;

greet();
```

دیگر Call به شکل:

```js
user.greet();
```

وجود ندارد.

Function از Object جدا شده است.

بنابراین نمی‌توانیم همان Implicit Binding قبلی را انتظار داشته باشیم.

مدل ذهنی:

```text
user.greet()
      ↓
Implicit Binding
      ↓
this → user
```

در مقابل:

```text
const greet = user.greet;

greet()
  ↓
Object سمت چپ Dot وجود ندارد
  ↓
Implicit Binding قبلی وجود ندارد
```

این یکی از مهم‌ترین دلایل خطاهای مربوط به `this` در JavaScript است.

---

# Method Patterns

اکنون که Method و `this` را می‌شناسیم، می‌توانیم از آن‌ها برای مدل کردن Behavior واقعی استفاده کنیم.

## Object Behavior

فرض کنید یک Shopping Cart داریم:

```js
const cart = {
  total: 0,

  add(price) {
    this.total += price;
  }
};
```

برای مثال:

```js
cart.add(100);
cart.add(50);

console.log(cart.total);
```

خروجی:

```text
150
```

اینجا Method:

```js
add()
```

Behavior مربوط به Cart را مدل می‌کند.

و:

```js
this.total
```

به State همان Cart اشاره می‌کند.

مدل ذهنی:

```text
Cart
│
├── total → State
│
└── add() → Behavior
             │
             ↓
           this
             │
             ↓
           Cart
```

این دقیقاً همان ترکیب Data و Behavior است که Object برای ما فراهم می‌کند.

---

## Method برای تغییر State

یک Account را در نظر بگیرید:

```js
const account = {
  owner: 'Omid',
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

اکنون:

```js
account.deposit(1000);

console.log(account.balance);
```

خروجی:

```text
6000
```

Method:

```js
deposit()
```

Behavior مربوط به Account را مدل می‌کند.

و:

```js
this.balance
```

به State همان Account دسترسی دارد.

---

## Method برای خواندن State

Method الزاماً نباید State را تغییر دهد.

برای مثال:

```js
const product = {
  name: 'Laptop',
  price: 1200,

  getPrice() {
    return this.price;
  }
};
```

اکنون:

```js
console.log(product.getPrice());
```

خروجی:

```text
1200
```

Method می‌تواند فقط Behavior مربوط به خواندن یا محاسبه Data را مدل کند.

---

# Method Chaining — مقدمه

یک Pattern رایج دیگر، **Method Chaining** است.

در این Pattern، یک Method بعد از انجام کار خود همان Object را برمی‌گرداند تا بتوانیم Method دیگری را بلافاصله فراخوانی کنیم.

برای مثال:

```js
const cart = {
  total: 0,

  add(price) {
    this.total += price;
    return this;
  }
};
```

اکنون:

```js
cart
  .add(100)
  .add(50)
  .add(25);
```

در اولین Call:

```js
cart.add(100)
```

Method مقدار:

```js
this
```

را برمی‌گرداند.

در این Call:

```js
this === cart
```

بنابراین نتیجه Method دوباره همان Object است.

سپس:

```js
.add(50)
```

روی همان Object اجرا می‌شود.

مدل ذهنی:

```text
cart
 ↓
add(100)
 ↓
return this
 ↓
cart
 ↓
add(50)
 ↓
return this
 ↓
cart
```

در نهایت:

```js
console.log(cart.total);
```

خروجی:

```text
175
```

در این فصل فقط مفهوم Method Chaining را معرفی می‌کنیم.

هدف ما در اینجا طراحی APIهای پیچیده نیست؛ فقط باید درک کنیم که یک Method می‌تواند Object مربوط به خود را برگرداند و امکان Chain کردن Callها را فراهم کند.

---

# Best Practices

## 1. Behavior مرتبط را در Method قرار دهید

اگر Behavior مستقیماً به یک Entity مربوط است، قرار دادن آن در Object می‌تواند مدل داده را خواناتر کند.

به‌جای پراکنده کردن Logic:

```js
function deposit(account, amount) {
  account.balance += amount;
}
```

در موارد مناسب می‌توان Behavior را بخشی از Object مدل کرد:

```js
const account = {
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

هدف، صرفاً کوتاه‌تر شدن Code نیست.

هدف این است که Data و Behavior مرتبط در یک مدل مشخص قرار بگیرند.

---

## 2. وقتی Method به Object وابسته است از `this` استفاده کنید

اگر Method باید روی State همان Object کار کند:

```js
const account = {
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

استفاده از `this` باعث می‌شود Method به Objectی که آن را فراخوانی می‌کند متصل باشد.

---

## 3. Arrow Function را بدون توجه به `this` به‌عنوان Method انتخاب نکنید

این Pattern می‌تواند مشکل‌ساز باشد:

```js
const user = {
  name: 'Omid',

  greet: () => {
    console.log(this.name);
  }
};
```

اگر Method به `this` نیاز دارد، Regular Function انتخاب مناسب‌تری است:

```js
const user = {
  name: 'Omid',

  greet: function () {
    console.log(this.name);
  }
};
```

یا Syntaxهای Method که در فصل بعد بررسی خواهند شد.

---

## 4. به Invocation توجه کنید، نه فقط محل تعریف Function

این:

```js
user.greet();
```

با این:

```js
const greet = user.greet;

greet();
```

از نظر `this` یکسان نیست.

پس هنگام Debug کردن `this` فقط به Definition نگاه نکنید.

نحوه Invocation را نیز بررسی کنید.

---

## 5. Method را به مسئولیت Object مرتبط کنید

این:

```js
const account = {
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

از نظر طراحی معنای مشخصی دارد.

Method باید Behavior مرتبط با مسئولیت Object را مدل کند.

---

## 6. Method Chaining را فقط زمانی استفاده کنید که API را خواناتر کند

Chaining می‌تواند خوانایی را افزایش دهد:

```js
cart
  .add(100)
  .add(50)
  .add(25);
```

اما اگر Chain بسیار طولانی یا مبهم شود، ممکن است خوانایی کاهش پیدا کند.

هدف Chaining باید طراحی API خوانا باشد، نه صرفاً کوتاه کردن Code.

---

# اشتباهات رایج

## اشتباه ۱: تصور اینکه `this` همیشه به Object محل تعریف Function اشاره می‌کند

این تصور:

```text
Function داخل user تعریف شده
↓
پس this همیشه user است
```

صحیح نیست.

برای Regular Function، `this` به Invocation وابسته است.

در:

```js
user.greet();
```

`this` به `user` مربوط می‌شود.

اما اگر Function از Object جدا شود، همان Binding را نخواهد داشت.

---

## اشتباه ۲: تصور اینکه `this` نام Object است

`this` نام Object نیست.

`this` یک Keyword است که مقدار آن با توجه به نحوه اجرای Function تعیین می‌شود.

در یک Method Call مشخص ممکن است:

```js
this === user
```

باشد، اما این به معنای آن نیست که `this` همیشه `user` است.

---

## اشتباه ۳: تصور اینکه Arrow Function به‌صورت خودکار `this` را از Object می‌گیرد

این کد:

```js
const user = {
  name: 'Omid',

  greet: () => {
    console.log(this.name);
  }
};
```

به این معنا نیست که:

```js
this === user
```

Arrow Function `this` مستقل ندارد و `this` را به‌صورت Lexical دریافت می‌کند.

---

## اشتباه ۴: جدا کردن Method و انتظار همان `this`

این:

```js
user.greet();
```

با:

```js
const greet = user.greet;

greet();
```

یکسان نیست.

در حالت دوم Method دیگر از طریق `user` فراخوانی نشده است.

---

## اشتباه ۵: تصور اینکه نام Property تعیین‌کننده `this` است

در:

```js
user.greet();
```

`this` به دلیل نام `greet` تعیین نمی‌شود.

رابطه مهم این است:

```text
user.greet()
↑
Object سمت چپ Dot
```

---

## اشتباه ۶: استفاده از Arrow Function برای Methodای که به `this` نیاز دارد

اگر Method قرار است:

```js
this.balance
```

را بخواند یا تغییر دهد، Arrow Function معمولاً انتخاب مناسبی نیست.

مثال مشکل‌دار:

```js
const account = {
  balance: 5000,

  deposit: (amount) => {
    this.balance += amount;
  }
};
```

برای چنین Behaviorای Regular Function مناسب‌تر است:

```js
const account = {
  balance: 5000,

  deposit: function (amount) {
    this.balance += amount;
  }
};
```

---

## اشتباه ۷: تصور اینکه `this` با Scope یکی است

`this` و Scope دو مفهوم متفاوت هستند.

Scope تعیین می‌کند Identifierها از کجا قابل دسترسی هستند.

`this` در یک Function به Invocation و نوع Function وابسته است.

این دو مفهوم را نباید یکی دانست.

---

# Summary

در این فصل از Function Property شروع کردیم.

در فصل قبل دیدیم که Function می‌تواند Value یک Property باشد:

```js
const account = {
  deposit: function () {}
};
```

وقتی چنین Functionای Behavior مرتبط با Object را مدل کند، آن را Method می‌نامیم.

سپس Method Invocation را بررسی کردیم:

```js
account.deposit();
```

در این نوع Call، Object سمت چپ Dot نقش مهمی در تعیین `this` دارد.

سپس `this` را بررسی کردیم.

برای Regular Function:

> مقدار `this` به نحوه Invocation Function وابسته است.

در یک Method Call مانند:

```js
account.deposit();
```

داریم:

```js
this === account
```

این رفتار **Implicit Binding** نام دارد.

سپس دیدیم که یک Function می‌تواند میان Objectهای مختلف استفاده شود:

```js
account1.showBalance();
account2.showBalance();
```

در هر Call، `this` به Object مربوط به همان Invocation اشاره می‌کند.

بعد تفاوت Arrow Function را بررسی کردیم.

Arrow Function `this` مستقل ندارد و `this` را از محیط Lexical اطراف خود دریافت می‌کند.

بنابراین این Pattern:

```js
const user = {
  greet: () => {}
};
```

باعث نمی‌شود `this` داخل Arrow Function به `user` اشاره کند.

در نهایت Method Pattern و Method Chaining را معرفی کردیم و دیدیم که Method می‌تواند Behavior Object را مدل کند و در صورت نیاز با:

```js
return this;
```

امکان Chain کردن Methodها را فراهم کند.

---

# Key Takeaways

* Function می‌تواند Value یک Property باشد.
* Function Property می‌تواند Behavior مرتبط با Object را مدل کند.
* Function Propertyای که Behavior Object را مدل می‌کند، Method نامیده می‌شود.
* Method با استفاده از Object Invocation می‌شود.
* در Method Call، نحوه Invocation برای تعیین `this` اهمیت دارد.
* در `object.method()`، Object سمت چپ Dot به‌صورت ضمنی به Method Call مرتبط می‌شود.
* این رفتار Implicit Binding نام دارد.
* `this` نام Object نیست.
* مقدار `this` برای Regular Function به نحوه Invocation وابسته است.
* یک Function می‌تواند در Objectهای مختلف استفاده شود و در هر Call `this` متفاوتی داشته باشد.
* جدا کردن Method از Object می‌تواند رابطه Implicit Binding را از بین ببرد.
* Arrow Function `this` مستقل ندارد.
* Arrow Function، `this` را به‌صورت Lexical از محیط اطراف دریافت می‌کند.
* Arrow Function معمولاً برای Methodای که به `this` نیاز دارد انتخاب مناسبی نیست.
* Arrow Function برای Functionهای داخلی که باید `this` بیرونی را حفظ کنند می‌تواند مفید باشد.
* Method می‌تواند State Object را بخواند یا تغییر دهد.
* Method Chaining می‌تواند با برگرداندن `this` ایجاد شود.
* `this` و Scope دو مفهوم متفاوت هستند.

---

# Technical Interview

## Junior

### 1. Method در JavaScript چیست؟

Method در ساده‌ترین تعریف Functionای است که به‌عنوان Property یک Object قرار گرفته و Behavior مرتبط با آن Object را مدل می‌کند.

---

### 2. چگونه یک Method را فراخوانی می‌کنیم؟

با قرار دادن Object و Property Function در یک Call:

```js
object.method();
```

---

### 3. `this` در یک Method چیست؟

در یک Method Call معمولی، `this` به Objectای اشاره می‌کند که Method از طریق آن فراخوانی شده است.

برای مثال:

```js
const user = {
  name: 'Omid',

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

در این Call:

```js
this === user
```

---

### 4. Implicit Binding چیست؟

وقتی یک Regular Function به شکل:

```js
object.method();
```

فراخوانی می‌شود، Object سمت چپ Dot به‌صورت ضمنی به‌عنوان `this` برای آن Call در نظر گرفته می‌شود.

---

### 5. آیا `this` همیشه به Object محل تعریف Function اشاره می‌کند؟

خیر.

برای Regular Function، `this` به نحوه Invocation وابسته است، نه صرفاً محل تعریف Function.

---

### 6. تفاوت Regular Function و Arrow Function در رابطه با `this` چیست؟

Regular Function می‌تواند بر اساس نحوه Invocation مقدار `this` دریافت کند.

Arrow Function `this` مستقل ندارد و `this` را از محیط Lexical اطراف خود دریافت می‌کند.

---

### 7. چرا Arrow Function معمولاً برای Method مناسب نیست؟

زیرا Method Call باعث نمی‌شود Arrow Function `this` را از Object دریافت کند.

اگر Method به:

```js
this.property
```

نیاز داشته باشد، Arrow Function معمولاً انتخاب مناسبی نیست.

---

## Mid-Level

### 8. چرا این دو Call از نظر `this` متفاوت هستند؟

```js
user.greet();
```

و:

```js
const greet = user.greet;
greet();
```

در Call اول Function از طریق `user` فراخوانی می‌شود؛ بنابراین Implicit Binding برقرار است.

در Call دوم Function از Object جدا شده و به‌صورت مستقل فراخوانی می‌شود؛ بنابراین دیگر همان Implicit Binding وجود ندارد.

---

### 9. آیا یک Function می‌تواند به‌عنوان Method چند Object استفاده شود؟

بله.

برای مثال:

```js
const showBalance = function () {
  console.log(this.balance);
};

const account1 = {
  balance: 5000,
  showBalance
};

const account2 = {
  balance: 10000,
  showBalance
};
```

سپس:

```js
account1.showBalance();
```

و:

```js
account2.showBalance();
```

به‌ترتیب `this` متفاوتی خواهند داشت.

---

### 10. چرا استفاده از `this` از اشاره مستقیم به نام Object بهتر است؟

زیرا Method را به نام خاص Object وابسته نمی‌کند.

به‌جای:

```js
console.log(user.name);
```

می‌توان نوشت:

```js
console.log(this.name);
```

در نتیجه Behavior می‌تواند در Objectهای مشابه نیز استفاده شود.

---

### 11. چرا این کد مشکل دارد؟

```js
const user = {
  name: 'Omid',

  greet: () => {
    console.log(this.name);
  }
};
```

زیرا Arrow Function `this` مستقل ندارد.

`this` آن از محیط Lexical اطراف دریافت می‌شود و `user` از طریق Method Call به آن Binding نمی‌شود.

---

### 12. چرا Arrow Function در Function داخلی یک Method می‌تواند مفید باشد؟

زیرا Arrow Function `this` مستقل ندارد و می‌تواند `this` مربوط به Function بیرونی را حفظ کند:

```js
const user = {
  name: 'Omid',

  greet: function () {
    const show = () => {
      console.log(this.name);
    };

    show();
  }
};
```

در اینجا Arrow Function داخلی همان `this` مربوط به `greet` را استفاده می‌کند.

---

### 13. Method Chaining چگونه کار می‌کند؟

یک Method می‌تواند Object فعلی را با:

```js
return this;
```

برگرداند.

برای مثال:

```js
const cart = {
  total: 0,

  add(price) {
    this.total += price;
    return this;
  }
};
```

در نتیجه:

```js
cart.add(100).add(50);
```

ممکن می‌شود.

---

### 14. آیا `this` و Scope یک مفهوم هستند؟

خیر.

Scope مربوط به دسترسی به Identifierها است.

`this` مربوط به Context مربوط به Function Invocation است و در Regular Function بر اساس نحوه Call تعیین می‌شود.

---

## Senior

### 15. چرا `this` در JavaScript را بهتر است Invocation-Based بدانیم؟

زیرا برای Regular Function، محل تعریف Function به‌تنهایی تعیین‌کننده `this` نیست.

این نحوه فراخوانی Function است که Binding مربوط به `this` را مشخص می‌کند.

برای مثال:

```js
object.method();
```

و:

```js
const method = object.method;
method();
```

به دلیل تفاوت در Invocation، رفتار `this` یکسانی ندارند.

---

### 16. چرا `this` برای طراحی Object Behavior اهمیت دارد؟

زیرا اجازه می‌دهد یک Behavior نسبت به Objectای که آن را اجرا می‌کند عمل کند.

برای مثال:

```js
const account = {
  balance: 5000,

  deposit(amount) {
    this.balance += amount;
  }
};
```

Method نیازی ندارد نام `account` را مستقیماً بداند.

Behavior به Object مربوط به Invocation متصل می‌شود.

---

### 17. چگونه یک Method می‌تواند بین چند Object قابل استفاده باشد؟

Function را می‌توان به‌عنوان Property در چند Object قرار داد:

```js
const showBalance = function () {
  console.log(this.balance);
};
```

سپس:

```js
const account1 = {
  balance: 5000,
  showBalance
};

const account2 = {
  balance: 10000,
  showBalance
};
```

در هر Invocation، Implicit Binding `this` را بر اساس Object مربوط به Call تعیین می‌کند.

بنابراین Behavior مشترک است اما Receiver متفاوت است.

---

### 18. چرا قرار دادن Arrow Function به‌عنوان Method با مدل Implicit Binding سازگار نیست؟

زیرا Implicit Binding یک رفتار مربوط به Invocation یک Regular Function است.

Arrow Function `this` مستقل ندارد.

در نتیجه:

```js
object.method();
```

نمی‌تواند `this` یک Arrow Function را مانند Regular Function به `object` تغییر دهد.

Arrow Function `this` را از محیط Lexical خود می‌گیرد.

---

### 19. چرا جدا کردن Method از Object یک مسئله مهم مهندسی است؟

زیرا در Applicationهای واقعی ممکن است یک Method را به Variable دیگری منتقل کنیم یا آن را به Function دیگری واگذار کنیم.

برای مثال:

```js
const handler = user.greet;
```

از این لحظه دیگر نباید فرض کنیم:

```js
handler();
```

همان `this` مربوط به:

```js
user.greet();
```

را خواهد داشت.

بنابراین در طراحی Callbackها، Event Handlerها و APIهای مبتنی بر Function باید رفتار `this` را آگاهانه در نظر گرفت.

جزئیات Binding صریح در فصل مربوط به Explicit Function Binding بررسی شده است.

---

### 20. چرا Method Chaining به `this` وابسته است؟

زیرا برای ادامه Chain باید نتیجه Method دوباره همان Object قابل استفاده باشد:

```js
add(price) {
  this.total += price;
  return this;
}
```

در نتیجه:

```js
cart.add(100)
    .add(50)
    .add(25);
```

هر Call Object را برمی‌گرداند و Call بعدی روی همان Object انجام می‌شود.

---

# Golden Answers

### Method چیست؟

Method Functionای است که به‌عنوان Property یک Object قرار گرفته و Behavior مرتبط با آن Object را مدل می‌کند.

### Method Invocation چیست؟

فراخوانی یک Method از طریق Object، مانند:

```js
object.method();
```

است.

### `this` چیست؟

`this` یک Keyword است که در یک Function مقدار آن به نوع Function و نحوه Invocation وابسته است. در یک Method Call معمولی، `this` به Object مرتبط با Call اشاره می‌کند.

### Implicit Binding چیست؟

وقتی یک Regular Function به شکل:

```js
object.method();
```

فراخوانی می‌شود، Object سمت چپ Dot به‌صورت ضمنی به‌عنوان `this` برای آن Call تعیین می‌شود.

### آیا `this` همیشه Object محل تعریف Function است؟

خیر.

در Regular Function، `this` بر اساس نحوه Invocation تعیین می‌شود.

### چرا `user.greet()` با `const greet = user.greet; greet()` متفاوت است؟

زیرا در حالت اول Function از طریق `user` فراخوانی شده و Implicit Binding برقرار است.

در حالت دوم Function از Object جدا شده و به‌صورت مستقل فراخوانی شده است.

### Arrow Function چه تفاوتی با Regular Function در رابطه با `this` دارد؟

Arrow Function `this` مستقل ندارد و `this` را از محیط Lexical اطراف خود دریافت می‌کند؛ بنابراین Method Call باعث نمی‌شود `this` آن به Object متصل شود.

### چرا Arrow Function معمولاً برای Methodای که به `this` نیاز دارد مناسب نیست؟

زیرا Arrow Function `this` را از Method Invocation دریافت نمی‌کند.

### چرا Arrow Function در Function داخلی یک Method می‌تواند مفید باشد؟

زیرا `this` مستقل ندارد و می‌تواند `this` مربوط به Function بیرونی را حفظ کند.

### Method Chaining چیست؟

Patternای است که در آن Method پس از انجام کار، Object مربوط به خود را برمی‌گرداند تا Method دیگری روی همان Object فراخوانی شود.

برای مثال:

```js
return this;
```

---

# Conclusion

در فصل قبل Object را به‌عنوان ساختاری برای سازمان‌دهی Data شناختیم.

اکنون یک مرحله جلوتر رفتیم.

Object فقط Data را نگهداری نمی‌کند.

می‌تواند Behavior مرتبط با همان Data را نیز مدل کند:

```text
Object
│
├── Data
│   ├── Property
│   └── Value
│
└── Behavior
    └── Method
```

از Function Property شروع کردیم:

```js
const account = {
  deposit: function () {}
};
```

سپس دیدیم که وقتی این Function Behavior مرتبط با Object را مدل می‌کند، Method نامیده می‌شود.

بعد به Method Invocation رسیدیم:

```js
account.deposit();
```

و متوجه شدیم که نحوه Invocation برای `this` اهمیت دارد.

در یک Method Call معمولی:

```js
account.deposit();
```

داریم:

```js
this === account
```

این همان **Implicit Binding** است.

سپس دیدیم که یک Function می‌تواند میان چند Object استفاده شود و هر Invocation می‌تواند `this` متفاوتی داشته باشد.

در ادامه تفاوت مهم Arrow Function را بررسی کردیم:

```text
Regular Function
↓
this بر اساس Invocation

Arrow Function
↓
this Lexical
```

بنابراین قرار دادن Arrow Function داخل Object به‌تنهایی باعث نمی‌شود `this` به آن Object اشاره کند.

در پایان نیز دیدیم که Methodها می‌توانند Behavior و State را در یک Object ترکیب کنند و در الگوهایی مانند Method Chaining، با برگرداندن `this` امکان ادامه زنجیره‌ای Callها را فراهم کنند.

اکنون مدل ذهنی ما چنین است:

```text
Object
   ↓
Function Property
   ↓
Method
   ↓
Method Invocation
   ↓
this
   ↓
Implicit Binding
   ↓
Arrow Function Difference
   ↓
Object Behavior
```

این پایه برای درک رفتار Objectها در JavaScript ضروری است.

در فصل بعد، همین Object Literal را از زاویه Syntaxهای مدرن بررسی می‌کنیم و می‌بینیم JavaScript چگونه Property Shorthand، Method Syntax و Computed Properties را برای ساخت Objectهای خواناتر و پویاتر فراهم می‌کند.
