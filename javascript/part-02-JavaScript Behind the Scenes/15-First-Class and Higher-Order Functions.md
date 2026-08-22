# Chapter 15 — First-Class and Higher-Order Functions

---

# اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* توضیح دهید چرا Function در JavaScript مانند یک Value قابل استفاده است.
* مفهوم **First-Class Function** را به‌صورت دقیق درک کنید.
* Function را در Variable، Object Property و Array Element ذخیره کنید.
* Function را به‌عنوان Argument به Function دیگری ارسال کنید.
* مفهوم اولیه **Callback Function** را درک کنید.
* Function را از یک Function دیگر برگردانید.
* مفهوم **Higher-Order Function** را از Callback تشخیص دهید.
* توضیح دهید چگونه Functionها می‌توانند برای ایجاد Abstraction و کاهش تکرار استفاده شوند.
* ارتباط First-Class Functions با الگوهای واقعی JavaScript را در سطح مناسب این فصل توضیح دهید.

---

# Core Question

> **چرا Function در JavaScript مانند یک Value قابل استفاده است؟**

---

# Concept Flow

```text
Function as Value
↓
First-Class Function
↓
Passing Functions
↓
Returning Functions
↓
Callback
↓
Higher-Order Function
↓
Abstraction
↓
Functional Thinking
```

---

# مقدمه

در فصل‌های قبل یاد گرفتیم که **Function** یک واحد قابل استفاده مجدد برای سازمان‌دهی منطق برنامه است.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

تا اینجا Function را بیشتر به‌عنوان چیزی می‌دیدیم که آن را **تعریف** و سپس **اجرا** می‌کنیم.

اما یک ویژگی مهم JavaScript وجود دارد که مدل ذهنی ما درباره Function را تغییر می‌دهد.

Function فقط چیزی نیست که بتوانیم آن را اجرا کنیم.

در JavaScript، خود Function نیز یک **Value** است.

یعنی همان‌طور که می‌توانیم یک String یا Number را در Variable ذخیره کنیم، می‌توانیم یک Function را نیز در Variable قرار دهیم.

```javascript
const calculateTotal = function (price, quantity) {
  return price * quantity;
};
```

اکنون Function مانند یک داده در اختیار برنامه قرار دارد.

این ویژگی پایه بسیاری از قابلیت‌های مهم JavaScript است.

از جمله:

* Passing Functions
* Callback Functions
* Higher-Order Functions
* Event Handlers
* بسیاری از Array APIs
* و بخش مهمی از سبک Functional Programming

بنابراین پرسش اصلی این فصل این نیست که:

> چگونه یک Function بنویسیم؟

بلکه این است:

> **چه اتفاقی می‌افتد وقتی Function را مانند یک Value با خودمان جابه‌جا کنیم؟**

---

# Block 01 — First-Class Functions

## Function as a Value

در فصل‌های قبل دیدیم که یک Value می‌تواند در Variable قرار بگیرد.

برای مثال:

```javascript
const productName = 'Laptop';
```

در اینجا:

```text
'Laptop'
```

یک Value است که در Variable زیر ذخیره شده است:

```text
productName
```

همین مفهوم درباره Function نیز صادق است.

```javascript
const calculatePrice = function (price) {
  return price * 1.2;
};
```

در اینجا Function نیز یک Value است و می‌توانیم آن را در Variable ذخیره کنیم.

---

## تعریف ساده

**First-Class Function** یعنی Function در زبان JavaScript مانند یک Value معمولی قابل استفاده است.

یعنی می‌توانیم Function را:

* در Variable ذخیره کنیم.
* به Function دیگری ارسال کنیم.
* از یک Function برگردانیم.
* در Object Property قرار دهیم.
* در Array Element ذخیره کنیم.

---

## تعریف فنی

در JavaScript، Function یک **First-Class Value** است.

این اصطلاح به این معناست که Function می‌تواند در موقعیت‌هایی که سایر Values قابل استفاده هستند، به‌عنوان یک Value مورد استفاده قرار گیرد.

First-Class بودن یک Function به معنای اجرای خودکار آن نیست.

Function تا زمانی که با `()` فراخوانی نشود، صرفاً یک Value است.

---

# ذخیره Function در Variable

یکی از ساده‌ترین نمونه‌های First-Class Function، قرار دادن Function در یک Variable است.

```javascript
const calculateDiscount = function (price) {
  return price * 0.9;
};
```

اکنون:

```javascript
calculateDiscount
```

به خود Function اشاره می‌کند.

اگر آن را اجرا کنیم:

```javascript
calculateDiscount(100);
```

نتیجه:

```text
90
```

اما اگر فقط بنویسیم:

```javascript
calculateDiscount
```

Function را اجرا نکرده‌ایم.

در این حالت خود Function به‌عنوان Value مورد استفاده قرار گرفته است.

---

# یک تفاوت مهم

این دو عبارت یکسان نیستند:

```javascript
calculateDiscount
```

و:

```javascript
calculateDiscount()
```

اولی خود Function را به‌عنوان Value مورد استفاده قرار می‌دهد.

دومی Function را **Invoke** می‌کند و نتیجه اجرای آن را به دست می‌آورد.

این تفاوت در هنگام Passing Functions بسیار مهم خواهد بود.

---

# Function به‌عنوان Object Property

Function را می‌توان به‌عنوان Property یک Object نیز ذخیره کرد.

```javascript
const cart = {
  total: 120,
  calculateTax: function () {
    return this.total * 0.1;
  }
};
```

در اینجا:

```javascript
calculateTax
```

یک Property است که Value آن یک Function است.

می‌توانیم آن را اجرا کنیم:

```javascript
cart.calculateTax();
```

در نتیجه Function بخشی از داده‌های Object شده است.

در فصل‌های بعد، مفهوم **Method** و رابطه آن با Object را به‌صورت کامل بررسی خواهیم کرد.

---

# Function به‌عنوان Array Element

Function حتی می‌تواند یکی از عناصر Array باشد.

```javascript
const operations = [
  function (price) {
    return price * 0.9;
  },
  function (price) {
    return price + 10;
  }
];
```

اکنون هر عنصر Array یک Function است.

برای اجرای Function اول:

```javascript
operations[0](100);
```

نتیجه:

```text
90
```

در این مثال Array صرفاً مجموعه‌ای از Numbers یا Strings نیست.

می‌تواند مجموعه‌ای از Functionها نیز باشد.

---

# چرا این قابلیت مهم است؟

اگر Function فقط قابل اجرا بود، بسیاری از الگوهای مهم JavaScript امکان‌پذیر نبودند.

اما وقتی Function یک Value باشد، می‌توانیم رفتار را نیز مانند داده جابه‌جا کنیم.

برای مثال:

```javascript
const operation = function (price) {
  return price * 0.9;
};
```

اکنون می‌توانیم این رفتار را به بخش دیگری از برنامه ارسال کنیم.

این نقطه شروع **Higher-Order Functions** است.

---

# تحلیل مهندسی

First-Class Function یک قابلیت Syntax-level ساده نیست.

این قابلیت یک تغییر مهم در مدل طراحی برنامه ایجاد می‌کند.

در حالت سنتی ممکن است داده را به یک Function ارسال کنیم:

```text
Data → Function → Result
```

اما وقتی Function خودش Value باشد، می‌توانیم رفتار را نیز ارسال کنیم:

```text
Data + Behavior → Function → Result
```

این یعنی یک Function می‌تواند تعیین کند:

> «چه کاری انجام شود»

و Function دیگری تعیین کند:

> «چه زمانی یا در چه ساختاری آن کار اجرا شود.»

این مدل در APIهای مختلف JavaScript بسیار مهم است.

---

# اشتباهات رایج

### اشتباه اول: Function فقط زمانی Value است که اجرا شود

نادرست است.

خود Function نیز یک Value است.

```javascript
const greet = function () {
  console.log('Hello');
};
```

---

### اشتباه دوم: این دو عبارت یکی هستند

```javascript
greet
```

و:

```javascript
greet()
```

نادرست است.

اولی Function را به‌عنوان Value استفاده می‌کند.

دومی Function را اجرا می‌کند.

---

### اشتباه سوم: First-Class Function یعنی Function Object معمولی است

این بیان برای درک اولیه کافی نیست.

نکته اصلی First-Class بودن این است که Function در JavaScript مانند یک Value قابل استفاده است؛ از جمله ذخیره، ارسال و بازگرداندن.

---

# نکات مهم

* Function در JavaScript یک First-Class Value است.
* Function می‌تواند در Variable ذخیره شود.
* Function می‌تواند Object Property باشد.
* Function می‌تواند Array Element باشد.
* `functionName` با `functionName()` یکسان نیست.
* First-Class Functions پایه Passing و Returning Functions هستند.

---

# پاسخ کوتاه طلایی مصاحبه

**Function در JavaScript First-Class است؛ یعنی می‌توان آن را مانند یک Value ذخیره، ارسال و از Function دیگری برگرداند. همین ویژگی پایه Callback و Higher-Order Functions است.**

---

# Block 02 — Passing Functions

## ارسال Function به Function دیگر

اکنون که می‌دانیم Function یک Value است، می‌توانیم آن را مانند سایر Values به Function دیگری ارسال کنیم.

برای مثال:

```javascript
function applyDiscount(price, discount) {
  return discount(price);
}
```

در اینجا Parameter زیر:

```javascript
discount
```

قرار است یک Function دریافت کند.

اکنون می‌توانیم Function دیگری را ارسال کنیم:

```javascript
const tenPercentOff = function (price) {
  return price * 0.9;
};

const finalPrice = applyDiscount(100, tenPercentOff);
```

نتیجه:

```text
90
```

---

# چرا Function را به‌عنوان Argument ارسال کنیم؟

فرض کنید Function زیر فقط یک نوع تخفیف را پشتیبانی کند:

```javascript
function applyDiscount(price) {
  return price * 0.9;
}
```

اگر بخواهیم انواع مختلف تخفیف داشته باشیم، ممکن است مجبور شویم Functionهای زیادی ایجاد کنیم.

اما اگر منطق تخفیف را به‌عنوان یک Function دریافت کنیم:

```javascript
function applyDiscount(price, discount) {
  return discount(price);
}
```

Function اصلی دیگر لازم نیست بداند تخفیف دقیقاً چگونه محاسبه می‌شود.

این مسئولیت به Function دیگری واگذار شده است.

---

## مثال واقعی‌تر

```javascript
function calculateFinalPrice(price, rule) {
  return rule(price);
}

const memberPrice = price => price * 0.9;

const vipPrice = price => price * 0.8;

calculateFinalPrice(100, memberPrice);
calculateFinalPrice(100, vipPrice);
```

Function اصلی:

```javascript
calculateFinalPrice
```

نمی‌داند Rule دقیقاً چیست.

فقط می‌داند که یک Function دریافت می‌کند و آن را روی `price` اجرا می‌کند.

این اولین قدم به سمت **Abstraction** است.

---

# Callback Concept

وقتی یک Function را به Function دیگری ارسال می‌کنیم تا Function دریافت‌کننده بتواند آن را اجرا کند، Function ارسال‌شده را **Callback Function** می‌نامیم.

برای مثال:

```javascript
function processPrice(price, callback) {
  return callback(price);
}
```

و:

```javascript
const addTax = price => price * 1.1;

processPrice(100, addTax);
```

در این مثال:

```javascript
addTax
```

یک Callback است.

چون به Function دیگری ارسال شده تا آن Function بتواند آن را اجرا کند.

---

## نکته مهم

در این فصل فقط با **مفهوم اولیه Callback** آشنا می‌شویم.

Callback در فصل بعد به‌صورت مستقل بررسی خواهد شد.

در آن فصل تفاوت میان:

* Synchronous Callback
* Asynchronous Callback
* Event Callback
* Timer Callback

و مشکلات Callbackها بررسی می‌شود.

در این فصل تنها لازم است مدل پایه را درک کنیم:

```text
Function A
    ↓
receives
    ↓
Function B
```

---

# تحلیل مهندسی

Passing Function باعث می‌شود منطق یک Function از جزئیات یک رفتار خاص جدا شود.

برای مثال:

```javascript
function processPrice(price, rule) {
  return rule(price);
}
```

این Function مسئول **اجرای فرآیند** است.

اما:

```javascript
const memberPrice = price => price * 0.9;
```

مسئول **تعریف Rule** است.

این جداسازی می‌تواند باعث شود کد:

* قابل استفاده مجددتر شود.
* انعطاف‌پذیرتر شود.
* ساده‌تر تست شود.
* وابستگی میان بخش‌های مختلف کاهش پیدا کند.

---

# Common Mistakes

### اشتباه اول: ارسال Function با `()`

این کد:

```javascript
processPrice(100, addTax());
```

با این کد متفاوت است:

```javascript
processPrice(100, addTax);
```

در حالت اول، Function ابتدا اجرا می‌شود و **نتیجه اجرای آن** ارسال می‌شود.

در حالت دوم، خود Function ارسال می‌شود.

برای Passing Function معمولاً باید خود Function را ارسال کنیم:

```javascript
addTax
```

---

### اشتباه دوم: Callback و Higher-Order Function یکی هستند

این دو مفهوم مرتبط‌اند اما یکسان نیستند.

**Callback** نام Functionای است که به Function دیگری ارسال می‌شود.

**Higher-Order Function** نام Functionای است که یک Function دریافت می‌کند یا یک Function برمی‌گرداند.

یک Function می‌تواند هم‌زمان:

* Higher-Order Function باشد.
* Callback دریافت کند.

---

# نکات مهم

* Function می‌تواند به‌عنوان Argument ارسال شود.
* Function دریافت‌کننده می‌تواند آن را اجرا کند.
* Function ارسال‌شده در این نقش Callback نامیده می‌شود.
* `fn` و `fn()` دو مفهوم متفاوت دارند.
* Passing Functions امکان جدا کردن Behavior از Logic اصلی را فراهم می‌کند.

---

# پاسخ کوتاه طلایی مصاحبه

**وقتی یک Function را به‌عنوان Argument به Function دیگری ارسال می‌کنیم، Function دریافت‌شده می‌تواند آن را اجرا کند و در این نقش Callback نامیده می‌شود.**

---

# Block 03 — Returning Functions

## آیا Function می‌تواند Function دیگری را برگرداند؟

تا اینجا دیدیم که Function می‌تواند Function دیگری را دریافت کند.

اما First-Class بودن Function یک نتیجه مهم دیگر نیز دارد:

> Function می‌تواند Function دیگری را نیز برگرداند.

برای مثال:

```javascript
function createFormatter() {
  return function (name) {
    return `User: ${name}`;
  };
}
```

اکنون:

```javascript
const formatUser = createFormatter();
```

مقدار `formatUser` یک Function است.

می‌توانیم آن را اجرا کنیم:

```javascript
formatUser('Omid');
```

نتیجه:

```text
User: Omid
```

---

# چرا Function دیگری را برگردانیم؟

گاهی یک Function باید رفتار خاصی را برای استفاده بعدی ایجاد کند.

برای مثال، فرض کنید سیستم ما به Formatterهای مختلف نیاز دارد.

```javascript
function createFormatter(prefix) {
  return function (name) {
    return `${prefix}: ${name}`;
  };
}
```

اکنون:

```javascript
const userFormatter = createFormatter('User');
const adminFormatter = createFormatter('Admin');
```

می‌توانیم از آن‌ها استفاده کنیم:

```javascript
userFormatter('Omid');
```

نتیجه:

```text
User: Omid
```

و:

```javascript
adminFormatter('Sara');
```

نتیجه:

```text
Admin: Sara
```

---

# Function Factory

Functionای که Functionهای دیگر را ایجاد و برمی‌گرداند، می‌تواند نقش یک **Function Factory** را داشته باشد.

مدل ذهنی آن:

```text
Factory Function
      ↓
Creates
      ↓
Specialized Function
```

در مثال قبل:

```javascript
createFormatter
```

یک Function Factory است.

این الگو زمانی مفید است که چند Function رفتار مشابهی دارند اما در یک جزئیات با یکدیگر تفاوت دارند.

---

# یک مثال کاربردی

فرض کنید در یک Application برای بخش‌های مختلف Log تولید می‌کنیم.

```javascript
function createLogger(type) {
  return function (message) {
    return `[${type}] ${message}`;
  };
}
```

اکنون:

```javascript
const apiLogger = createLogger('API');
const uiLogger = createLogger('UI');
```

استفاده:

```javascript
apiLogger('Request completed');
```

نتیجه:

```text
[API] Request completed
```

و:

```javascript
uiLogger('Button clicked');
```

نتیجه:

```text
[UI] Button clicked
```

یک Function عمومی، Functionهای تخصصی ایجاد کرده است.

---

# یک نکته مهم درباره Scope

در مثال‌های Function Factory ممکن است Function داخلی از داده‌های Function بیرونی استفاده کند.

برای مثال:

```javascript
function createLogger(type) {
  return function (message) {
    return `[${type}] ${message}`;
  };
}
```

در اینجا Function داخلی به `type` دسترسی دارد.

این رفتار به **Lexical Scope** و در نهایت **Closure** مرتبط است.

اما Closure موضوع فصل بعدی این بخش نیست و در فصل مستقلی به‌صورت کامل بررسی خواهد شد.

در این فصل فقط باید بدانیم:

> یک Function می‌تواند Function دیگری را برگرداند و Function برگشتی می‌تواند برای ایجاد رفتار تخصصی استفاده شود.

---

# تحلیل مهندسی

Returning Functions به ما اجازه می‌دهد رفتار را نه‌تنها ارسال، بلکه **تولید** کنیم.

در Passing Function:

```text
Existing Function
       ↓
Passed to another Function
```

در Returning Function:

```text
Factory
   ↓
Creates
   ↓
New Function
```

این قابلیت یکی از پایه‌های مهم طراحی Functional در JavaScript است.

---

# Common Mistakes

### اشتباه اول: Return کردن نتیجه Function به جای خود Function

این دو متفاوت هستند:

```javascript
return formatUser(name);
```

و:

```javascript
return formatUser;
```

اولی نتیجه اجرای Function را برمی‌گرداند.

دومی خود Function را برمی‌گرداند.

---

### اشتباه دوم: Function Factory را با Closure یکی دانستن

Function Factory یک **الگوی استفاده** از Function است.

Closure یک **رفتار مربوط به Scope و Environment** است.

ممکن است Function Factory از Closure استفاده کند، اما این دو مفهوم یکسان نیستند.

---

# نکات مهم

* Function می‌تواند Function دیگری را Return کند.
* Functionای که Function تولید می‌کند می‌تواند Function Factory باشد.
* Returning Functions امکان ایجاد Behavior تخصصی را فراهم می‌کند.
* `return fn` با `return fn()` متفاوت است.
* رابطه Functionهای برگشتی با Closure در فصل آینده بررسی خواهد شد.

---

# پاسخ کوتاه طلایی مصاحبه

**از آنجا که Function یک First-Class Value است، می‌توان آن را از Function دیگری Return کرد. این قابلیت برای ساخت Functionهای تخصصی و الگوهایی مانند Function Factory استفاده می‌شود.**

---

# Block 04 — Higher-Order Functions

## Higher-Order Function چیست؟

اکنون دو قابلیت اصلی را می‌شناسیم:

1. Function می‌تواند Function دریافت کند.
2. Function می‌تواند Function برگرداند.

Functionای که یکی از این دو کار را انجام دهد، یک **Higher-Order Function** است.

---

## تعریف ساده

Higher-Order Function تابعی است که:

* یک Function را به‌عنوان ورودی دریافت می‌کند،
* یا یک Function را به‌عنوان خروجی برمی‌گرداند،
* یا هر دو.

---

## تعریف فنی

در JavaScript، Higher-Order Function اصطلاحی برای Functionای است که با Functionها به‌عنوان Value کار می‌کند؛ یعنی یک Function را دریافت می‌کند یا یک Function برمی‌گرداند.

این مفهوم نتیجه مستقیم First-Class بودن Functionهاست.

---

# مثال اول: دریافت Function

```javascript
function calculate(price, operation) {
  return operation(price);
}
```

این Function یک Function دریافت می‌کند.

بنابراین:

```javascript
calculate
```

یک Higher-Order Function است.

---

# مثال دوم: برگرداندن Function

```javascript
function createMultiplier(factor) {
  return function (value) {
    return value * factor;
  };
}
```

این Function یک Function برمی‌گرداند.

بنابراین آن نیز یک Higher-Order Function است.

---

# مثال سوم: هر دو

```javascript
function createProcessor(operation) {
  return function (value) {
    return operation(value);
  };
}
```

این Function:

* یک Function دریافت می‌کند.
* یک Function برمی‌گرداند.

بنابراین Higher-Order Function است.

---

# تفاوت Callback و Higher-Order Function

این تفاوت بسیار مهم است.

فرض کنید:

```javascript
function processPrice(price, operation) {
  return operation(price);
}
```

و:

```javascript
const addTax = price => price * 1.1;
```

وقتی می‌نویسیم:

```javascript
processPrice(100, addTax);
```

در اینجا:

```text
processPrice → Higher-Order Function
addTax       → Callback
```

بنابراین:

> **Callback نقش Function ارسال‌شده را توصیف می‌کند.**

در حالی که:

> **Higher-Order Function نقش Function دریافت‌کننده یا برگرداننده Function را توصیف می‌کند.**

این دو اصطلاح از دو زاویه متفاوت به یک تعامل نگاه می‌کنند.

---

# Abstraction با Higher-Order Functions

یکی از مهم‌ترین کاربردهای Higher-Order Functions، ایجاد **Abstraction** است.

فرض کنید چند بخش از برنامه باید یک فرآیند مشابه را انجام دهند.

بدون Abstraction ممکن است کد تکراری ایجاد کنیم:

```javascript
const discountedPrice = price * 0.9;
const discountedShipping = shipping * 0.9;
```

اما می‌توانیم منطق عمومی را جدا کنیم:

```javascript
function applyDiscount(value, rule) {
  return rule(value);
}
```

اکنون Rule را می‌توانیم جداگانه تعریف کنیم:

```javascript
const tenPercentOff = value => value * 0.9;
```

و سپس:

```javascript
applyDiscount(100, tenPercentOff);
```

این طراحی باعث می‌شود:

* Logic عمومی از Rule خاص جدا شود.
* رفتار قابل تعویض باشد.
* Duplication کاهش پیدا کند.
* Functionها Reusableتر شوند.

---

# چرا Abstraction مهم است؟

هدف Abstraction این نیست که کد را پیچیده‌تر کنیم.

هدف این است که جزئیات غیرضروری از بخش اصلی Logic جدا شوند.

برای مثال:

```javascript
function processPrice(price, rule) {
  return rule(price);
}
```

این Function درباره جزئیات Rule چیزی نمی‌داند.

تنها می‌داند:

> یک Price دریافت کن و Rule مربوطه را روی آن اعمال کن.

این جداسازی مسئولیت یکی از اصول مهم طراحی نرم‌افزار است.

---

# Functional Thinking

First-Class Functions فقط یک قابلیت Syntax نیستند.

آن‌ها یک مدل فکری جدید برای طراحی Logic ایجاد می‌کنند.

در رویکرد معمول ممکن است بیشتر روی این سؤال تمرکز کنیم:

> چه داده‌ای دارم و چه دستوری باید روی آن اجرا کنم؟

اما با First-Class Functions می‌توانیم سؤال دیگری نیز مطرح کنیم:

> چه رفتاری را می‌توانم به این بخش از برنامه منتقل کنم؟

برای مثال:

```javascript
processData(data, transform);
```

در اینجا:

```text
data
```

داده را مشخص می‌کند.

و:

```text
transform
```

رفتار را مشخص می‌کند.

این جداسازی یکی از پایه‌های تفکر Functional است.

---

# Functional Thinking به چه معناست؟

در این سطح، Functional Thinking را نباید با آموزش کامل **Functional Programming** یکی بدانیم.

در این فصل تنها یک ایده مهم را دنبال می‌کنیم:

> **Function می‌تواند مانند داده جابه‌جا شود و رفتار را به بخش‌های مختلف برنامه منتقل کند.**

این مدل بعدها در:

* Array APIs
* Event Handlers
* Async Programming

نقش مهمی خواهد داشت.

---

# Common Mistakes

### اشتباه اول: هر Function یک Higher-Order Function است

خیر.

این Function:

```javascript
function add(a, b) {
  return a + b;
}
```

یک Function معمولی است.

اما این Function:

```javascript
function apply(operation, value) {
  return operation(value);
}
```

Higher-Order Function است، زیرا یک Function دریافت می‌کند.

---

### اشتباه دوم: Callback و Higher-Order Function یکی هستند

خیر.

```text
Callback
= Function دریافت‌شده

Higher-Order Function
= Function دریافت‌کننده یا برگرداننده Function
```

---

### اشتباه سوم: Higher-Order Function همیشه باید Function را Return کند

خیر.

کافی است Function دیگری را دریافت کند.

```javascript
function apply(value, operation) {
  return operation(value);
}
```

این Function Higher-Order است، حتی اگر خروجی آن یک Number باشد.

---

### اشتباه چهارم: Abstraction یعنی هرچه Function بیشتر، کد بهتر

خیر.

Abstraction زمانی ارزشمند است که پیچیدگی واقعی را مدیریت کند.

Abstraction بیش از حد می‌تواند:

* خوانایی را کاهش دهد.
* مسیر اجرای کد را پیچیده کند.
* Debugging را دشوارتر کند.

---

# نکات مهم

* Higher-Order Function با Functionها به‌عنوان Value کار می‌کند.
* دریافت Function یکی از راه‌های Higher-Order بودن است.
* Return کردن Function نیز یکی از راه‌های Higher-Order بودن است.
* Callback و Higher-Order Function دو مفهوم یکسان نیستند.
* Higher-Order Functions می‌توانند Abstraction ایجاد کنند.
* Abstraction باید پیچیدگی را کاهش دهد، نه افزایش.

---

# پاسخ کوتاه طلایی مصاحبه

**Higher-Order Function تابعی است که یک Function را دریافت می‌کند یا یک Function برمی‌گرداند. Callback معمولاً Functionای است که به Higher-Order Function ارسال می‌شود.**

---

# Block 05 — Practical Applications

First-Class Functions فقط یک مفهوم تئوری نیستند.

بخش بزرگی از APIهای JavaScript بر پایه همین قابلیت طراحی شده‌اند.

در این بخش فقط ارتباط آن‌ها را با چند کاربرد مهم می‌بینیم.

جزئیات این کاربردها در فصل‌های بعدی بررسی خواهند شد.

---

# Array APIs — Preview

بسیاری از Array Methods مدرن یک Function دریافت می‌کنند.

برای مثال:

```javascript
const prices = [100, 200, 300];

prices.map(price => price * 0.9);
```

در این مثال:

```javascript
price => price * 0.9
```

به `map` ارسال شده است.

این دقیقاً نتیجه First-Class بودن Function است.

اما خود `map`، نحوه Iteration و Callbackهای آن در فصل‌های مربوط به Arrayها و Callback Functions به‌صورت کامل بررسی خواهند شد.

در این فصل فقط باید مدل زیر را ببینیم:

```text
Array Method
     ↓
Function
     ↓
Process Each Value
```

---

# Event Handlers — Preview

در محیط Browser نیز Functionها می‌توانند به‌عنوان رفتار به APIهای دیگر ارسال شوند.

برای مثال:

```javascript
button.addEventListener('click', handleClick);
```

در اینجا:

```javascript
handleClick
```

به‌عنوان یک Function به API ارسال شده است.

Browser بعداً می‌تواند این Function را در زمان مناسب اجرا کند.

جزئیات Event، Event Handler و اجرای Callback در فصل‌های مربوط به Browser و Events بررسی خواهد شد.

---

# Asynchronous Programming — Preview

First-Class Functions در برنامه‌نویسی Asynchronous نیز نقش مهمی دارند.

برای مثال:

```javascript
setTimeout(showMessage, 1000);
```

در اینجا:

```javascript
showMessage
```

به API ارسال شده است.

ایده اصلی همان است:

```text
Function
↓
Passed as Value
↓
Used Later
```

اما اینکه چه چیزی باعث اجرای Function در زمان دیگری می‌شود و Runtime چگونه این فرآیند را مدیریت می‌کند، خارج از Scope این فصل است.

---

# Function Wrappers

یک کاربرد دیگر Higher-Order Functions، ساخت Functionهایی است که رفتار موجود را در یک لایه جدید قرار می‌دهند.

برای مثال:

```javascript
function withLogging(operation) {
  return function (value) {
    console.log('Processing:', value);
    return operation(value);
  };
}
```

اکنون می‌توانیم:

```javascript
const calculatePrice = price => price * 1.1;

const loggedPrice = withLogging(calculatePrice);
```

و سپس:

```javascript
loggedPrice(100);
```

در اینجا Higher-Order Function یک رفتار اضافی، یعنی Logging، به Function موجود اضافه کرده است.

این الگو نمونه‌ای ساده از **Function Wrapping** است.

---

# چرا Function Wrapping مفید است؟

فرض کنید چند Function مختلف داریم که باید Logging مشابهی داشته باشند.

به‌جای اینکه Logging را داخل همه آن‌ها تکرار کنیم، می‌توانیم Logic مشترک را در یک Function جدا قرار دهیم.

```text
Original Function
       ↓
Wrapper
       ↓
Additional Behavior
```

این همان جایی است که First-Class Functions به یک ابزار واقعی برای طراحی نرم‌افزار تبدیل می‌شوند.

---

# Preview: Array Methods

در فصل‌های آینده با Array Methods مختلفی مانند:

```javascript
map()
filter()
reduce()
```

کار خواهیم کرد.

این Methods نمونه‌های مهمی از Higher-Order Functions هستند، زیرا Function دریافت می‌کنند.

در این فصل فقط باید بدانیم که این APIها بر پایه همان قابلیت First-Class Functions ساخته شده‌اند.

---

# Preview: Event Handlers

Event Handler نیز نمونه دیگری از Passing Functions است.

```javascript
element.addEventListener('click', handleClick);
```

Function:

```javascript
handleClick
```

به API ارسال می‌شود تا سیستم بتواند در زمان مناسب آن را اجرا کند.

جزئیات Event System بعداً بررسی خواهد شد.

---

# Preview: Async

در APIهای Asynchronous نیز Functionها می‌توانند به سیستم دیگری سپرده شوند.

```javascript
setTimeout(handleTimeout, 1000);
```

اما این مثال را نباید به‌عنوان آموزش Asynchronous Programming در نظر گرفت.

در اینجا فقط یک کاربرد First-Class Function را مشاهده می‌کنیم.

---

# تحلیل مهندسی

سه کاربرد زیر در ظاهر متفاوت‌اند:

```javascript
array.map(transform);
```

```javascript
element.addEventListener('click', handleClick);
```

```javascript
setTimeout(handleTimeout, 1000);
```

اما از یک ایده مشترک استفاده می‌کنند:

```text
Behavior as a Value
```

یعنی Function می‌تواند مانند داده از یک بخش برنامه به بخش دیگری منتقل شود.

این یکی از مهم‌ترین تغییرات مدل ذهنی در این فصل است.

---

# Common Mistakes

### اشتباه اول: اجرای Function هنگام ارسال

نادرست:

```javascript
setTimeout(handleTimeout(), 1000);
```

صحیح:

```javascript
setTimeout(handleTimeout, 1000);
```

در حالت صحیح، خود Function ارسال می‌شود.

---

### اشتباه دوم: تصور اینکه Callback همیشه Asynchronous است

خیر.

Callback می‌تواند Synchronous یا Asynchronous باشد.

این تفاوت در فصل بعد بررسی خواهد شد.

---

### اشتباه سوم: تصور اینکه Higher-Order Functions فقط برای Arrayها هستند

خیر.

Array Methods تنها یکی از کاربردهای آن‌ها هستند.

Higher-Order Functions می‌توانند برای:

* Abstraction
* Function Wrapping
* Event Handling
* API Design
* و بسیاری از الگوهای دیگر

استفاده شوند.

---

# نکات مهم

* Array APIs از First-Class Functions استفاده می‌کنند.
* Event Handlers نمونه‌ای از Passing Functions هستند.
* Async APIs نیز می‌توانند Function دریافت کنند.
* Function Wrapping می‌تواند رفتار مشترک را به Functionهای مختلف اضافه کند.
* جزئیات Array، Event و Async در فصل‌های آینده آموزش داده خواهند شد.

---

# دیدگاه Jonas

در رویکرد آموزشی Jonas Schmedtmann، First-Class Functions یکی از پایه‌های مهم درک JavaScript مدرن هستند.

نکته کلیدی این دیدگاه این است که Function را فقط به‌عنوان چیزی که اجرا می‌شود نبینیم.

Function می‌تواند:

* ذخیره شود.
* ارسال شود.
* برگردانده شود.
* و به‌عنوان Behavior در طراحی برنامه استفاده شود.

این مدل ذهنی بعداً در Callback، Array Methods، Closures و بسیاری از الگوهای JavaScript اهمیت بیشتری پیدا می‌کند.

---

# Block 06 — Chapter Review

# Summary

در این فصل مدل ذهنی خود را درباره Function تغییر دادیم.

در فصل‌های قبلی Function را بیشتر به‌عنوان واحدی برای اجرای منطق برنامه می‌شناختیم.

اکنون دیدیم که Function در JavaScript خودش یک **Value** است.

به همین دلیل می‌توانیم Function را در Variable ذخیره کنیم:

```javascript
const calculate = function (value) {
  return value * 2;
};
```

یا آن را در Object قرار دهیم:

```javascript
const calculator = {
  calculate
};
```

یا در Array ذخیره کنیم:

```javascript
const operations = [calculate];
```

سپس دیدیم که Function می‌تواند به‌عنوان Argument به Function دیگری ارسال شود.

در این حالت Function ارسال‌شده می‌تواند نقش **Callback** داشته باشد.

```javascript
process(data, callback);
```

همچنین Function می‌تواند Function دیگری را Return کند.

```javascript
function createOperation() {
  return function (value) {
    return value * 2;
  };
}
```

از ترکیب این قابلیت‌ها به مفهوم **Higher-Order Function** رسیدیم.

Higher-Order Function تابعی است که Function دریافت می‌کند یا Function برمی‌گرداند.

در نهایت دیدیم که این قابلیت امکان ایجاد **Abstraction** و انتقال Behavior را فراهم می‌کند.

مدل ذهنی اصلی این فصل را می‌توان به شکل زیر خلاصه کرد:

```text
Function
   ↓
Value
   ↓
Store
   ↓
Pass
   ↓
Return
   ↓
Compose Behavior
   ↓
Abstraction
```

---

# Key Takeaways

در پایان این فصل باید بتوانید نکات زیر را به‌صورت دقیق توضیح دهید:

* Function در JavaScript یک **First-Class Value** است.
* Function می‌تواند در Variable ذخیره شود.
* Function می‌تواند Object Property باشد.
* Function می‌تواند Array Element باشد.
* Function می‌تواند به‌عنوان Argument ارسال شود.
* Function ارسال‌شده می‌تواند نقش Callback داشته باشد.
* Function می‌تواند Function دیگری را Return کند.
* Function Factory الگویی برای ایجاد Functionهای تخصصی است.
* Higher-Order Function تابعی است که Function دریافت می‌کند یا Function برمی‌گرداند.
* Callback و Higher-Order Function دو مفهوم یکسان نیستند.
* Higher-Order Functions می‌توانند برای ایجاد Abstraction استفاده شوند.
* First-Class Functions پایه بسیاری از APIها و الگوهای مدرن JavaScript هستند.
* Array APIs، Event Handlers و Async APIs نمونه‌هایی از کاربرد این مدل هستند.
* جزئیات Callback، Array Methods، Events و Async در فصل‌های بعدی بررسی خواهند شد.

---

# Technical Interview

## سطح Junior

### سؤال ۱

First-Class Function در JavaScript چیست؟

### پاسخ

Function در JavaScript یک First-Class Value است؛ یعنی می‌توان آن را مانند سایر Values در Variable ذخیره کرد، به Function دیگری ارسال کرد و از یک Function برگرداند.

---

### سؤال ۲

تفاوت `fn` و `fn()` چیست؟

### پاسخ

`fn` خود Function را به‌عنوان Value نشان می‌دهد، در حالی که `fn()` Function را Invoke می‌کند و نتیجه اجرای آن را برمی‌گرداند.

---

### سؤال ۳

آیا می‌توان Function را داخل Object ذخیره کرد؟

### پاسخ

بله. Function می‌تواند به‌عنوان Object Property ذخیره شود و در این حالت معمولاً به آن Method گفته می‌شود.

---

### سؤال ۴

آیا می‌توان Function را داخل Array قرار داد؟

### پاسخ

بله. از آنجا که Function یک First-Class Value است، می‌توان آن را مانند هر Value دیگری به‌عنوان Array Element ذخیره کرد.

---

### سؤال ۵

Callback چیست؟

### پاسخ

Callback Functionای است که به Function یا API دیگری ارسال می‌شود تا آن سیستم بتواند آن را اجرا کند.

---

## سطح Mid-Level

### سؤال ۶

Higher-Order Function چیست؟

### پاسخ

Higher-Order Function تابعی است که یک Function را دریافت می‌کند یا یک Function برمی‌گرداند. این مفهوم مستقیماً بر پایه First-Class بودن Functionها در JavaScript قرار دارد.

---

### سؤال ۷

تفاوت Callback و Higher-Order Function چیست؟

### پاسخ

Callback به Function ارسال‌شده اشاره دارد، در حالی که Higher-Order Function به Function دریافت‌کننده یا برگرداننده Function اشاره می‌کند. بنابراین این دو اصطلاح نقش‌های متفاوتی را توصیف می‌کنند.

---

### سؤال ۸

چرا Passing Functions می‌تواند باعث کاهش Code Duplication شود؟

### پاسخ

زیرا می‌توان Logic عمومی را یک‌بار پیاده‌سازی کرد و Behavior متفاوت را به‌صورت Function به آن ارسال کرد. در نتیجه Logic اصلی از جزئیات Behavior جدا می‌شود.

---

### سؤال ۹

Function Factory چیست؟

### پاسخ

Function Factory تابعی است که Functionهای دیگر را ایجاد و Return می‌کند. این الگو برای تولید Functionهایی با رفتار مشابه اما تنظیمات یا رفتار تخصصی متفاوت استفاده می‌شود.

---

### سؤال ۱۰

چرا Higher-Order Functions برای Abstraction مناسب هستند؟

### پاسخ

زیرا می‌توانند Logic عمومی را از Behavior خاص جدا کنند. Function اصلی فرآیند را مدیریت می‌کند و Function دریافت‌شده یا تولیدشده جزئیات Behavior را مشخص می‌کند.

---

### سؤال ۱۱

آیا یک Higher-Order Function حتماً باید Function برگرداند؟

### پاسخ

خیر. کافی است یک Function دریافت کند. برای مثال Functionای که یک Callback دریافت و نتیجه اجرای آن را Return می‌کند، Higher-Order Function است.

---

## سطح Senior

### سؤال ۱۲

چرا First-Class بودن Function یک ویژگی مهم در طراحی زبان است؟

### پاسخ

زیرا اجازه می‌دهد Behavior مانند Data جابه‌جا شود. در نتیجه می‌توان Logic را از Behavior جدا کرد، Abstraction ایجاد کرد و APIهای انعطاف‌پذیرتری طراحی کرد.

---

### سؤال ۱۳

چرا `fn()` را نباید هنگام Passing یک Function با `fn` اشتباه گرفت؟

### پاسخ

`fn` خود Function را منتقل می‌کند، اما `fn()` آن را همان لحظه اجرا می‌کند و نتیجه اجرا را منتقل می‌کند. API دریافت‌کننده معمولاً به خود Function نیاز دارد تا زمان اجرای آن را کنترل کند.

---

### سؤال ۱۴

چگونه Higher-Order Functions می‌توانند Coupling را کاهش دهند؟

### پاسخ

با جدا کردن Logic عمومی از Behavior خاص. Function اصلی به‌جای وابستگی مستقیم به یک پیاده‌سازی مشخص، یک Function را دریافت می‌کند و فقط قرارداد موردنیاز آن Behavior را مصرف می‌کند.

---

### سؤال ۱۵

آیا استفاده بیشتر از Higher-Order Functions همیشه باعث بهتر شدن معماری می‌شود؟

### پاسخ

خیر. Abstraction باید پیچیدگی واقعی را کاهش دهد. استفاده بیش از حد می‌تواند مسیر اجرای برنامه را پیچیده‌تر و Debugging و Maintenance را دشوارتر کند.

---

### سؤال ۱۶

چرا Array Methods، Event Handlers و برخی APIهای Asynchronous به First-Class Functions وابسته‌اند؟

### پاسخ

زیرا این APIها می‌توانند Behavior را به‌صورت Function دریافت کنند و زمان یا نحوه اجرای آن را خودشان مدیریت کنند. این امکان مستقیماً از First-Class بودن Functionها ناشی می‌شود.

---

### سؤال ۱۷

اگر Function یک First-Class Value است، آیا از نظر رفتار دقیقاً مانند Number و String است؟

### پاسخ

نه. First-Class بودن به این معناست که Function می‌تواند در بسیاری از موقعیت‌ها مانند یک Value استفاده شود، نه اینکه Function از نظر رفتار و semantics با Primitiveها یکسان باشد.

---

# Golden Answers

## First-Class Function چیست؟

Function در JavaScript یک First-Class Value است؛ یعنی می‌توان آن را ذخیره، ارسال و از Function دیگری برگرداند.

---

## Higher-Order Function چیست؟

Higher-Order Function تابعی است که یک Function دریافت می‌کند یا یک Function برمی‌گرداند.

---

## Callback چیست؟

Callback تابعی است که به Function یا API دیگری ارسال می‌شود تا آن Function یا سیستم آن را اجرا کند.

---

## تفاوت Callback و Higher-Order Function چیست؟

Callback، Function ارسال‌شده است؛ Higher-Order Function، Function دریافت‌کننده یا برگرداننده Function است.

---

## چرا Function را به‌عنوان Argument ارسال می‌کنیم؟

برای انتقال Behavior و جدا کردن Logic عمومی از جزئیات یک رفتار خاص.

---

## چرا Function دیگری را Return می‌کنیم؟

برای ایجاد Behavior تخصصی و ساخت الگوهایی مانند Function Factory.

---

## آیا Higher-Order Function حتماً Function برمی‌گرداند؟

خیر. دریافت یک Function نیز برای Higher-Order بودن کافی است.

---

## چرا `fn` با `fn()` متفاوت است؟

`fn` خود Function را نشان می‌دهد، اما `fn()` آن Function را اجرا می‌کند و نتیجه اجرای آن را برمی‌گرداند.

---

## Function Factory چیست؟

Functionای است که Functionهای دیگر را ایجاد و Return می‌کند و معمولاً برای ساخت Behaviorهای تخصصی استفاده می‌شود.

---

## آیا Callback همیشه Asynchronous است؟

خیر. Callback می‌تواند Synchronous یا Asynchronous باشد. این تفاوت در فصل Callback Functions بررسی خواهد شد.

---

# پاسخ کوتاه طلایی مصاحبه

**سؤال:** چرا Function در JavaScript مانند یک Value قابل استفاده است؟

**پاسخ:**

زیرا Function در JavaScript یک **First-Class Value** است؛ بنابراین می‌توان آن را ذخیره کرد، به Function دیگری ارسال کرد و از Function دیگری برگرداند. این ویژگی پایه Callbackها، Higher-Order Functions و بسیاری از الگوهای Functional در JavaScript است.

---

# اشتباهات رایج فصل

### ۱. اجرای Function به‌جای ارسال آن

نادرست:

```javascript
process(data, transform());
```

صحیح:

```javascript
process(data, transform);
```

---

### ۲. یکی دانستن Callback و Higher-Order Function

Callback Function دریافت‌شده است.

Higher-Order Function Function دریافت‌کننده یا برگرداننده است.

---

### ۳. تصور اینکه Higher-Order Function باید Function برگرداند

دریافت Function نیز کافی است.

---

### ۴. تصور اینکه First-Class Function یعنی Function فقط یک Object است

First-Class بودن درباره نحوه استفاده از Function به‌عنوان Value است، نه صرفاً یکسان دانستن آن با Objectهای معمولی.

---

### ۵. استفاده بیش از حد از Abstraction

هر Abstraction الزاماً طراحی بهتری ایجاد نمی‌کند.

اگر Abstraction فهم کد را سخت‌تر کند، احتمالاً بیش از نیاز پروژه استفاده شده است.

---

### ۶. ورود زودهنگام به Closure

Function Factory ممکن است با Closure ارتباط پیدا کند، اما Closure مفهوم مستقلی است که در فصل مربوط به خود بررسی خواهد شد.

---

# جمع‌بندی مهندسی

مهم‌ترین تغییر ذهنی این فصل این است که Function را فقط به‌عنوان **کدی که اجرا می‌شود** نبینیم.

Function در JavaScript می‌تواند **Behavior قابل انتقال** باشد.

می‌توانیم آن را:

```text
Store
↓
Pass
↓
Return
↓
Compose
```

کنیم.

از این قابلیت، مفاهیمی مانند:

```text
First-Class Function
        ↓
Callback
        ↓
Higher-Order Function
        ↓
Abstraction
```

شکل می‌گیرند.

این مدل ذهنی در ادامه کتاب اهمیت بیشتری پیدا خواهد کرد.

در فصل بعد، مفهوم **Callback Function** به‌صورت مستقل بررسی می‌شود و تفاوت Callbackهای Synchronous و Asynchronous، نحوه اجرای آن‌ها و مشکلاتی مانند Callback Hell بررسی خواهد شد.

---

# Conclusion

Function در JavaScript فقط یک بلوک کد قابل اجرا نیست.

Function خودش یک Value است.

این ویژگی به JavaScript اجازه می‌دهد Behavior را مانند Data در برنامه جابه‌جا کند.

از یک طرف می‌توان Function را به Function دیگری ارسال کرد:

```javascript
process(data, callback);
```

و از طرف دیگر می‌توان Function دیگری را برگرداند:

```javascript
function createProcessor() {
  return function (data) {
    return data;
  };
}
```

وقتی Functionی Function دریافت می‌کند یا Function برمی‌گرداند، با مفهوم **Higher-Order Function** روبه‌رو هستیم.

این قابلیت امکان ایجاد Abstraction، کاهش تکرار و طراحی APIهای انعطاف‌پذیرتر را فراهم می‌کند.

بنابراین مدل ذهنی نهایی این فصل باید این باشد:

> **در JavaScript، Function فقط چیزی نیست که اجرا می‌شود؛ Function می‌تواند خودش یک Value باشد و به‌عنوان Behavior در بخش‌های مختلف برنامه منتقل، ذخیره و تولید شود.**

این مفهوم، پایه درک صحیح **Callback Functions** و بسیاری از APIهای مدرن JavaScript در فصل‌های بعدی است.
