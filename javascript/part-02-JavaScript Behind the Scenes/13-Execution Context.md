## Chapter 13 — Execution Context

---

# Chapter Goal

پس از مطالعه این فصل، انتظار می‌رود بتوانید:

* مفهوم **Execution Context** را توضیح دهید.
* بدانید چرا JavaScript برای اجرای Code به یک Context نیاز دارد.
* تفاوت **Global Execution Context** و **Function Execution Context** را توضیح دهید.
* بدانید هنگام فراخوانی یک Function چه اتفاق مفهومی رخ می‌دهد.
* مفهوم **Creation Phase** و **Execution Phase** را درک کنید.
* بدانید JavaScript پیش از اجرای Code چه Environmentی را آماده می‌کند.
* رابطه Execution Context با Variable و Binding را درک کنید.
* نقش اولیه **Scope** و `this` را در Execution Context بشناسید.
* Lifecycle یک Execution Context را از Creation تا Removal توضیح دهید.
* ارتباط Execution Context با مباحث آینده مانند Call Stack، Scope و Hoisting را درک کنید.
* به پرسش‌های فنی مرتبط با Execution Context در سطوح Junior، Mid-Level و Senior پاسخ دهید.

---

# Core Question

> **هنگام اجرای JavaScript چه محیطی برای اجرای Code ایجاد می‌شود؟**

---

# Concept Flow

```text
Program Execution
↓
Execution Context
↓
Global Context
↓
Function Context
↓
Creation Phase
↓
Execution Phase
↓
Environment
↓
Scope
↓
this
↓
Context Lifecycle
```

---

# مقدمه

در فصل قبل بررسی کردیم که JavaScript چگونه از **Source Code** به اجرای واقعی می‌رسد.

با JavaScript Engine، Parsing، AST، Compilation و Runtime آشنا شدیم.

اما هنوز یک سؤال مهم باقی مانده است:

> وقتی Engine تصمیم می‌گیرد Code را اجرا کند، دقیقاً Code در چه محیطی اجرا می‌شود؟

برای مثال:

```javascript
const product = 'Laptop';

console.log(product);
```

Engine باید بتواند:

* `product` را مدیریت کند.
* مقدار آن را در اختیار Code قرار دهد.
* `console.log()` را اجرا کند.
* وضعیت لازم برای اجرای Program را نگهداری کند.

همین نیاز ما را به مفهوم **Execution Context** می‌رساند.

Execution Context را می‌توان محیطی مفهومی دانست که JavaScript هنگام اجرای Code برای مدیریت وضعیت لازم آن Execution ایجاد می‌کند.

---

# Block 01 — Execution Context

## Execution چیست؟

پیش از تعریف Execution Context، ابتدا باید خود واژه **Execution** را درک کنیم.

Execution یعنی:

> اجرای دستورها و محاسبه Expressionهای موجود در Program.

برای مثال:

```javascript
const price = 120;
const quantity = 2;

const total = price * quantity;
```

JavaScript باید این Code را اجرا کند.

در جریان Execution:

```text
price
↓
120

quantity
↓
2

price * quantity
↓
240
```

در نتیجه Execution صرفاً خواندن Source Code نیست.

Execution یعنی JavaScript واقعاً عملیات تعریف‌شده توسط Program را انجام دهد.

---

## Context چیست؟

واژه **Context** در اینجا به محیط یا شرایطی اشاره دارد که Execution در آن انجام می‌شود.

برای اجرای Code، JavaScript باید اطلاعاتی درباره وضعیت فعلی Execution داشته باشد.

برای مثال:

* چه Variableهایی در دسترس هستند؟
* چه Bindingهایی ایجاد شده‌اند؟
* Code در چه Environmentی اجرا می‌شود؟
* مقدار `this` چیست؟

این اطلاعات بخشی از Context اجرای Code را تشکیل می‌دهند.

---

## Execution Context چیست؟

### تعریف ساده

**Execution Context محیطی مفهومی است که JavaScript هنگام اجرای Code ایجاد می‌کند تا وضعیت لازم برای اجرای آن Code را مدیریت کند.**

به بیان ساده‌تر:

> Execution Context مشخص می‌کند Code در چه محیطی و با چه وضعیت اجرایی در حال اجرا است.

---

## تعریف فنی

Execution Context یک ساختار مفهومی در مدل اجرای ECMAScript است که وضعیت لازم برای اجرای یک قطعه Code را مشخص می‌کند.

این Context شامل اطلاعاتی مرتبط با Environment اجرای Code، Bindingها، Scope و `this` است.

در این فصل از یک مدل آموزشی ساده برای درک Execution Context استفاده می‌کنیم.

جزئیات دقیق‌تر Environmentها و نحوه Resolution کردن Identifierها در فصل‌های **Scope** و **Scope Chain** بررسی خواهند شد.

---

## چرا Execution Context مهم است؟

اگر Execution Context را نشناسیم، بسیاری از رفتارهای JavaScript جدا از هم و غیرقابل‌پیش‌بینی به نظر می‌رسند.

برای مثال، ممکن است بپرسیم:

> Variable هنگام اجرای Function کجا قرار می‌گیرد؟

یا:

> چرا یک Function می‌تواند Variableهای محلی خودش را داشته باشد؟

یا:

> چرا هنگام اجرای یک Function، وضعیت اجرای آن با بخش اصلی Program متفاوت است؟

Execution Context یک مدل ذهنی برای پاسخ به این پرسش‌ها فراهم می‌کند.

---

## مثال

```javascript
const product = 'Laptop';

console.log(product);
```

برای اجرای این Program، JavaScript باید محیطی برای اجرای Code اصلی ایجاد کند.

در این محیط، Binding مربوط به:

```text
product
```

وجود خواهد داشت و Code می‌تواند از آن استفاده کند.

در سطح مفهومی:

```text
Program
   ↓
Execution Context
   ↓
product = 'Laptop'
   ↓
console.log(product)
```

---

## تحلیل مهندسی

Execution Context را نباید با یک Object معمولی JavaScript اشتباه گرفت.

این مفهوم بخشی از **Execution Model** زبان است و برای توضیح چگونگی اجرای Code استفاده می‌شود.

همچنین Execution Context همان **Runtime Environment** نیست.

Runtime محیط بزرگ‌تری است که Engine و قابلیت‌های Host را در بر می‌گیرد.

Execution Context در داخل فرآیند اجرای Code توسط Engine معنا پیدا می‌کند.

---

## اشتباهات رایج

### اشتباه اول: Execution Context همان Scope است.

خیر.

Scope درباره محدوده دسترسی به Identifierها صحبت می‌کند.

Execution Context مدل گسترده‌تری برای وضعیت اجرای Code است.

در فصل Scope این تفاوت دقیق‌تر بررسی خواهد شد.

---

### اشتباه دوم: Execution Context یک Object قابل دسترسی در JavaScript است.

خیر.

Execution Context یک مفهوم داخلی در مدل اجرای JavaScript است.

ما نمی‌توانیم مستقیماً بنویسیم:

```javascript
console.log(executionContext);
```

---

### اشتباه سوم: Execution Context همان Call Stack است.

خیر.

Execution Context و Call Stack دو مفهوم متفاوت هستند.

Execution Context وضعیت اجرای Code را توصیف می‌کند.

Call Stack نحوه مدیریت ترتیب اجرای Contextها را بررسی می‌کند.

Call Stack در فصل بعد آموزش داده خواهد شد.

---

## نکات مهم

* Execution یعنی اجرای واقعی Code.
* Context یعنی محیط و وضعیت لازم برای Execution.
* Execution Context محیط مفهومی اجرای Code است.
* Execution Context با Scope و Call Stack یکسان نیست.
* جزئیات Scope و Call Stack در فصل‌های بعد بررسی خواهند شد.

---

## پاسخ کوتاه طلایی مصاحبه

> **Execution Context محیط مفهومی‌ای است که JavaScript برای اجرای Code ایجاد می‌کند و اطلاعات لازم مانند Environment، Bindingها و ****`this`**** را در اختیار Execution قرار می‌دهد.**

---

# Global Execution Context

اولین Codeی که JavaScript اجرا می‌کند، در یک Context ویژه قرار می‌گیرد که **Global Execution Context** نام دارد.

---

## Global Execution Context چیست؟

### تعریف ساده

Global Execution Context محیط اجرای Code اصلی Program است.

هنگامی که JavaScript اجرای یک Script را آغاز می‌کند، ابتدا Context مربوط به اجرای Global Code ایجاد می‌شود.

---

## چرا Global Context وجود دارد؟

Program باید از یک نقطه مشخص شروع شود.

برای مثال:

```javascript
const appName = 'Recipe Hub';

console.log(appName);
```

این Code متعلق به یک Function خاص نیست.

بنابراین JavaScript باید محیطی برای اجرای Code اصلی ایجاد کند.

این محیط:

**Global Execution Context**

است.

---

## مثال

```javascript
const appName = 'Recipe Hub';
const version = 1;

console.log(appName);
console.log(version);
```

در سطح مفهومی:

```text
Global Execution Context
│
├── appName
├── version
│
└── Execute Program
```

---

## Global Object

در محیط‌های JavaScript یک **Global Object** نیز وجود دارد.

در Browser، این Object با:

```javascript
window
```

شناخته می‌شود.

برای مثال:

```javascript
console.log(window);
```

در Browser، `window` نماینده Global Object است.

اما باید دقت کنیم که **Global Object** و **Global Execution Context** یک مفهوم نیستند.

Global Object یکی از اجزای محیط Global است؛ در حالی که Execution Context مفهوم گسترده‌تری برای توصیف وضعیت اجرای Code است.

---

## Global Scope

در این مرحله تنها به‌صورت مقدماتی با **Global Scope** آشنا می‌شویم.

وقتی یک Identifier در سطح Global تعریف می‌شود، ممکن است در محدوده Global قرار گیرد.

برای مثال:

```javascript
const appName = 'Recipe Hub';
```

در اینجا `appName` در سطح بالای Script تعریف شده است.

اما اینکه دقیقاً Scope چگونه تعیین می‌شود و Identifier چگونه در Scopeهای مختلف پیدا می‌شود، موضوع فصل‌های بعد است.

---

## Global Execution Phase

پس از آماده شدن Global Environment، JavaScript Code مربوط به Program را اجرا می‌کند.

برای مثال:

```javascript
const price = 100;
const quantity = 3;

const total = price * quantity;

console.log(total);
```

در سطح مفهومی:

```text
Global Context
      ↓
Prepare Environment
      ↓
Execute Code
      ↓
total = 300
      ↓
console.log(300)
```

---

## تحلیل مهندسی

Global Execution Context نقطه شروع اجرای Script است.

هر Program باید بتواند از یک Context اولیه شروع شود.

بعداً خواهیم دید که وقتی یک Function فراخوانی می‌شود، JavaScript برای آن Function نیز Context مخصوصی ایجاد می‌کند.

---

## اشتباهات رایج

❌ Global Execution Context را همان `window` بدانیم.

✔ `window` در Browser یک Global Object است؛ Execution Context مفهوم دیگری است.

---

❌ تصور کنیم تمام Executionها فقط در Global Context اتفاق می‌افتند.

✔ Functionها Contextهای مخصوص خودشان را ایجاد می‌کنند.

---

## نکات مهم

* اجرای Global Code با Global Execution Context مرتبط است.
* Global Context نقطه شروع اجرای Script است.
* Global Object بخشی از محیط Global است.
* Global Object و Global Execution Context یکسان نیستند.
* جزئیات دقیق Global Scope در فصل Scope بررسی خواهد شد.

---

## پاسخ کوتاه طلایی مصاحبه

> **Global Execution Context محیطی است که JavaScript برای اجرای Global Code ایجاد می‌کند و نقطه شروع اجرای یک Script محسوب می‌شود.**

---

# Block 02 — Function Execution Context

تا اینجا دیدیم که JavaScript برای اجرای Program یک Global Execution Context ایجاد می‌کند.

اما Programها فقط شامل Codeهای سطح Global نیستند.

در پروژه‌های واقعی، Functionها بخش مهمی از Code را تشکیل می‌دهند.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}
```

وقتی این Function اجرا شود، JavaScript باید محیطی برای اجرای Code داخل Function داشته باشد.

اینجاست که **Function Execution Context** وارد می‌شود.

---

## Function Execution Context چیست؟

### تعریف ساده

وقتی یک Function فراخوانی و اجرا می‌شود، JavaScript یک Execution Context مخصوص آن Function ایجاد می‌کند.

این Context وضعیت اجرای همان Function را مدیریت می‌کند.

---

## Function Invocation

صرف تعریف Function به معنای اجرای آن نیست.

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}
```

در این مرحله Function فقط تعریف شده است.

برای اجرای آن باید Function را فراخوانی کنیم:

```javascript
calculateTotal(100, 2);
```

با Invocation، JavaScript باید Context مربوط به اجرای Function را آماده کند.

---

## مثال

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}

const result = calculateTotal(100, 2);
```

در زمان اجرای Function:

```text
Global Execution Context
        │
        └── calculateTotal()
                 ↓
        Function Execution Context
                 │
                 ├── price
                 ├── quantity
                 └── total
```

در این مدل ساده، Variableهای مورد نیاز Function در Context مربوط به همان Function مدیریت می‌شوند.

---

## Local Variables

یکی از ویژگی‌های مهم Function Execution Context، مدیریت وضعیت محلی Function است.

در مثال:

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}
```

این Identifierها مربوط به اجرای Function هستند:

```text
price
quantity
total
```

به همین دلیل آن‌ها را در این مرحله می‌توان به‌عنوان **Local Data** در نظر گرفت.

---

## Arguments

هنگام Invocation می‌توان Valueهایی را به Function ارسال کرد.

```javascript
calculateTotal(100, 2);
```

در اینجا:

```text
100
2
```

Valueهایی هستند که به Function ارسال شده‌اند.

Function Context باید این اطلاعات را در اختیار اجرای Function قرار دهد.

به همین دلیل Arguments بخشی از مدل Function Execution هستند.

---

## تحلیل مهندسی

Function Execution Context باعث می‌شود هر Invocation بتواند وضعیت اجرای مربوط به خودش را داشته باشد.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const first = calculateTotal(100, 2);
const second = calculateTotal(50, 4);
```

هر Invocation باید با Valueهای مربوط به همان Invocation اجرا شود.

در سطح مفهومی:

```text
calculateTotal(100, 2)
        ↓
Context مخصوص این Invocation

calculateTotal(50, 4)
        ↓
Context مخصوص این Invocation
```

این مدل ذهنی برای درک رفتار Functionها در فصل‌های آینده بسیار مهم است.

---

## اشتباهات رایج

### اشتباه اول: یک Function همیشه یک Execution Context دارد.

دقیق‌تر این است که بگوییم:

> هر بار که Function اجرا می‌شود، Execution Context مربوط به آن Invocation ایجاد می‌شود.

---

### اشتباه دوم: Function Declaration خودش Execution Context ایجاد می‌کند.

صرف تعریف Function به معنای اجرای آن نیست.

Execution Context مربوط به Function هنگام **Invocation** ایجاد می‌شود.

---

### اشتباه سوم: Local Variableها Global هستند.

برای مثال:

```javascript
function createUser() {
  const username = 'Omid';
}
```

`username` بخشی از وضعیت اجرای Function است و در سطح Global قرار ندارد.

جزئیات دقیق Accessibility در فصل Scope بررسی خواهد شد.

---

## نکات مهم

* Function هنگام Invocation اجرا می‌شود.
* اجرای Function با Function Execution Context مرتبط است.
* Local Variableها در Context مربوط به Function مدیریت می‌شوند.
* Arguments نیز بخشی از وضعیت اجرای Function هستند.
* هر Invocation می‌تواند Context اجرایی مخصوص خود را داشته باشد.

---

## پاسخ کوتاه طلایی مصاحبه

> **وقتی یک Function فراخوانی می‌شود، JavaScript یک Function Execution Context برای اجرای آن ایجاد می‌کند که وضعیت محلی مانند Parameters، Local Bindings و سایر اطلاعات لازم برای اجرای Function را مدیریت می‌کند.**

---

# Block 03 — Creation Phase

اکنون یک سؤال مهم مطرح می‌شود:

> آیا JavaScript بلافاصله بعد از ایجاد Execution Context شروع به اجرای خط اول Code می‌کند؟

خیر.

پیش از اجرای Code، محیط لازم برای Execution آماده می‌شود.

این مرحله را به‌صورت آموزشی **Creation Phase** می‌نامیم.

---

## Creation Phase چیست؟

### تعریف ساده

Creation Phase مرحله‌ای است که JavaScript محیط و Bindingهای لازم برای اجرای Code را آماده می‌کند.

در این مرحله هنوز هدف اصلی، اجرای دستورهای Program نیست.

هدف، آماده کردن شرایط لازم برای Execution است.

---

## چرا Creation Phase لازم است؟

فرض کنید Function زیر را داریم:

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}
```

برای اجرای این Function، JavaScript باید بداند:

* `price` چیست؟
* `quantity` چیست؟
* `total` چگونه مدیریت خواهد شد؟
* Environment اجرای Function چگونه است؟

بنابراین پیش از اجرای Code، Environment باید آماده شود.

---

## Environment

واژه **Environment** در این فصل به مجموعه اطلاعات و Bindingهایی اشاره دارد که JavaScript برای مدیریت Identifierها و وضعیت اجرای Code نیاز دارد.

برای مثال:

```javascript
const price = 100;
```

در اینجا JavaScript باید Binding مربوط به:

```text
price
```

را در Environment مربوط به Execution مدیریت کند.

در این مرحله لازم نیست وارد جزئیات دقیق **Lexical Environment** و **Variable Environment** شویم.

این مفاهیم در فصل‌های Scope و Hoisting با جزئیات بیشتری بررسی خواهند شد.

---

## Binding چیست؟

در فصل‌های قبل درباره Variable و Identifier صحبت کردیم.

برای مثال:

```javascript
const price = 100;
```

در اینجا:

```text
price → Identifier
100   → Value
```

Execution Model نیاز دارد رابطه میان نام و Value را مدیریت کند.

به این رابطه در سطح مفهومی **Binding** گفته می‌شود.

بنابراین:

> Binding ارتباط میان یک Identifier و وضعیت یا Value مرتبط با آن Identifier است.

---

## Creation Phase و Binding

در Creation Phase، JavaScript محیط لازم برای مدیریت Bindingها را آماده می‌کند.

برای مثال:

```javascript
const price = 100;
```

در سطح مفهومی:

```text
Creation Phase
      ↓
Environment آماده می‌شود
      ↓
Binding مربوط به price
      ↓
Execution Phase
```

جزئیات اینکه `var`، `let` و `const` در این مرحله دقیقاً چه رفتاری دارند، در فصل **Hoisting and Temporal Dead Zone** بررسی خواهد شد.

---

## ارتباط با Hoisting

یکی از دلایلی که Creation Phase اهمیت دارد، توضیح برخی رفتارهایی است که معمولاً با واژه **Hoisting** شناخته می‌شوند.

برای مثال:

```javascript
console.log(user);

var user = 'Omid';
```

این رفتار بدون داشتن مدل Creation Phase ممکن است عجیب به نظر برسد.

اما در این فصل فقط ارتباط مفهومی را معرفی می‌کنیم:

```text
Execution Context
       ↓
Creation Phase
       ↓
Environment / Bindings
       ↓
Execution Phase
```

رفتار دقیق `var`، `let`، `const` و Function Declaration در Creation Phase موضوع فصل Hoisting خواهد بود.

---

## چرا Hoisting را کامل آموزش نمی‌دهیم؟

زیرا Concept Flow کتاب ابتدا باید:

```text
Execution Context
↓
Scope
↓
Scope Chain
↓
Hoisting
```

را طی کند.

اگر Hoisting را در این فصل به‌صورت کامل بررسی کنیم، مفاهیمی مانند Scope و Environment هنوز برای خواننده به اندازه کافی تثبیت نشده‌اند.

بنابراین در اینجا فقط مدل ذهنی مورد نیاز را ایجاد می‌کنیم.

---

## مثال

```javascript
function createOrder() {
  const orderId = 101;

  console.log(orderId);
}

createOrder();
```

در سطح مفهومی:

```text
Function Invocation
        ↓
Create Execution Context
        ↓
Prepare Environment
        ↓
Create/Prepare Bindings
        ↓
Execute Code
```

این ترتیب، پایه درک رفتارهای پیچیده‌تر JavaScript در فصل‌های آینده است.

---

## تحلیل مهندسی

Creation Phase نشان می‌دهد که Execution یک فرآیند کاملاً لحظه‌ای و بدون آماده‌سازی نیست.

JavaScript پیش از اجرای Code باید وضعیت لازم برای آن Execution را آماده کند.

این مدل ذهنی در آینده برای درک:

* Hoisting
* TDZ
* Scope
* Function Execution
* `this`

اهمیت خواهد داشت.

اما هر یک از این مباحث در جایگاه مخصوص خود بررسی خواهند شد.

---

## اشتباهات رایج

### اشتباه اول: Creation Phase یعنی Memory Allocation ساده.

این توضیح بیش از حد ساده است.

Creation Phase بخشی از فرآیند آماده‌سازی Execution Context و Environment است.

---

### اشتباه دوم: Hoisting یعنی Code واقعاً به بالای فایل منتقل می‌شود.

این مدل ذهنی نادرست است.

Creation Phase به رفتارهایی منجر می‌شود که ما معمولاً آن‌ها را با Hoisting توضیح می‌دهیم، اما Source Code واقعاً جابه‌جا نمی‌شود.

جزئیات این موضوع در فصل Hoisting بررسی خواهد شد.

---

### اشتباه سوم: Creation Phase یعنی هیچ Codeای اجرا نمی‌شود و هیچ کاری انجام نمی‌شود.

در این مرحله هدف اصلی آماده‌سازی Execution Environment است، نه اجرای معمول Statementهای Program.

---

## نکات مهم

* Creation Phase پیش از Execution Phase قرار دارد.
* Environment اجرای Code در این مرحله آماده می‌شود.
* Bindingهای مورد نیاز Execution مدیریت می‌شوند.
* Creation Phase پایه‌ای برای درک Hoisting است.
* جزئیات Hoisting و TDZ به فصل بعدتر واگذار شده است.

---

## پاسخ کوتاه طلایی مصاحبه

> **Creation Phase مرحله‌ای از آماده‌سازی Execution Context است که در آن Environment و Bindingهای لازم برای اجرای Code آماده می‌شوند؛ سپس Execution Phase آغاز می‌شود.**

---

# Block 04 — Execution Phase

پس از آماده شدن Execution Context، JavaScript وارد مرحله اجرای Code می‌شود.

این مرحله را **Execution Phase** می‌نامیم.

---

## Execution Phase چیست؟

### تعریف ساده

Execution Phase مرحله‌ای است که JavaScript Statementها و Expressionهای Program را اجرا و ارزیابی می‌کند.

اگر Creation Phase مرحله آماده‌سازی باشد، Execution Phase مرحله انجام واقعی عملیات Program است.

---

## مثال

```javascript
const price = 100;
const quantity = 3;

const total = price * quantity;

console.log(total);
```

در Execution Phase، Code به‌ترتیب اجرا می‌شود:

```text
price = 100
      ↓
quantity = 3
      ↓
total = price * quantity
      ↓
total = 300
      ↓
console.log(total)
```

---

## Assignment

یکی از عملیات مهم در Execution Phase، Assignment است.

برای مثال:

```javascript
let status = 'pending';

status = 'completed';
```

در Execution Phase، Assignment دوم باعث می‌شود وضعیت مربوط به `status` تغییر کند.

```text
Initial
status → 'pending'

Execution
status = 'completed'

Result
status → 'completed'
```

---

## Function Execution

اگر Execution به Function Invocation برسد، Function نیز باید اجرا شود.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(100, 2);
```

در سطح مفهومی:

```text
Global Execution
      ↓
calculateTotal(100, 2)
      ↓
Function Execution Context
      ↓
Function Code Executes
      ↓
return 200
      ↓
Global Execution Continues
```

در این فصل فقط این ارتباط را در سطح Execution Context بررسی می‌کنیم.

نحوه مدیریت چند Context در کنار یکدیگر، موضوع **Call Stack** در فصل بعد است.

---

## Context Lifecycle

یک Execution Context چرخه مشخصی دارد.

به‌صورت ساده:

```text
Creation
   ↓
Execution
   ↓
Removal
```

---

## Creation

ابتدا Context ایجاد می‌شود.

در این مرحله Environment و Bindingهای لازم آماده می‌شوند.

---

## Execution

سپس Code اجرا می‌شود.

Statementها و Expressionها ارزیابی می‌شوند و Functionها در صورت Invocation اجرا می‌شوند.

---

## Removal

پس از پایان Execution مربوط به Context، آن Context دیگر Context فعال اجرای آن Code نیست و می‌تواند از چرخه Execution خارج شود.

برای Functionها این اتفاق پس از پایان اجرای Function رخ می‌دهد.

---

## مثال

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}

const result = calculateTotal(100, 2);
```

مدل ذهنی:

```text
Global Context
      ↓
Create
      ↓
Execute
      ↓
Function Invocation
      ↓
Create Function Context
      ↓
Execute Function
      ↓
Return
      ↓
Function Context Removed
      ↓
Global Execution Continues
```

این مدل پایه‌ای بعداً برای درک Call Stack استفاده خواهد شد.

---

# `this` در Execution Context

یکی دیگر از مفاهیمی که با Execution Context ارتباط دارد، `this` است.

در این مرحله فقط باید بدانیم:

> Execution Context شامل اطلاعاتی مرتبط با `this` است.

برای مثال، در یک Function:

```javascript
function showUser() {
  console.log(this);
}
```

مقدار `this` بخشی از وضعیت Execution آن Function محسوب می‌شود.

اما پاسخ به سؤال:

> دقیقاً `this` چه مقداری دارد؟

به نحوه Invocation و نوع Function وابسته است.

این موضوع در فصل **The ****`this`**** Keyword** به‌صورت کامل بررسی خواهد شد.

---

## چرا `this` را فقط معرفی می‌کنیم؟

اگر در این فصل تمام Rules مربوط به `this` را آموزش دهیم، وارد مباحث فصل آینده می‌شویم.

Concept Flow کتاب به این صورت طراحی شده است:

```text
Execution Context
        ↓
this Introduction
        ↓
The this Keyword
```

بنابراین در اینجا تنها ارتباط مفهومی را می‌سازیم.

---

## تحلیل مهندسی

Execution Phase همان مرحله‌ای است که Program از یک ساختار آماده به یک رفتار واقعی تبدیل می‌شود.

برای مهندس Frontend، این مدل ذهنی هنگام تحلیل Bug اهمیت زیادی دارد.

وقتی Program رفتار غیرمنتظره‌ای دارد، باید بتوانیم بپرسیم:

* چه Codeای اجرا شده؟
* در چه Contextی اجرا شده؟
* چه Bindingهایی در آن Environment وجود داشته‌اند؟
* Execution چه زمانی وارد Function شده است؟
* Context چه زمانی پایان یافته است؟

این پرسش‌ها پایه Debugging عمیق‌تر JavaScript هستند.

---

## اشتباهات رایج

### اشتباه اول: Creation Phase و Execution Phase کاملاً جدا از هم و مستقل هستند.

این دو مرحله بخش‌هایی از چرخه آماده‌سازی و اجرای یک Context هستند.

---

### اشتباه دوم: Assignment در زمان Declaration انجام نمی‌شود.

این موضوع به نوع Declaration و Execution Model بستگی دارد و جزئیات آن در Hoisting بررسی خواهد شد.

---

### اشتباه سوم: `this` همیشه به همان Objectای اشاره می‌کند که Function داخل آن نوشته شده است.

این یک مدل ذهنی نادرست است.

مقدار `this` به Rules مربوط به Invocation وابسته است و در فصل مخصوص خود بررسی خواهد شد.

---

## نکات مهم

* Execution Phase مرحله اجرای واقعی Code است.
* Assignment و Evaluation در این مرحله انجام می‌شوند.
* Function Invocation می‌تواند باعث ایجاد Function Execution Context شود.
* Context پس از پایان Execution از چرخه فعال خارج می‌شود.
* `this` بخشی از مدل Execution Context است، اما Rules آن در فصل آینده بررسی می‌شود.

---

## پاسخ کوتاه طلایی مصاحبه

> **در Execution Phase، JavaScript Code را اجرا می‌کند، Valueها را محاسبه و Assignmentها را انجام می‌دهد و در صورت Function Invocation، Execution مربوط به Function را آغاز می‌کند.**

---

# Execution Context Lifecycle

اکنون می‌توانیم کل مفهوم فصل را در یک مدل واحد قرار دهیم.

```text
                Execution Context
                       │
                       ▼
                  Creation Phase
                       │
          ┌────────────┴────────────┐
          │                         │
      Environment                Bindings
          │
          ▼
                  Execution Phase
                       │
          ┌────────────┼────────────┐
          │            │            │
       Evaluate     Assign      Invoke
       Code         Values      Function
                                    │
                                    ▼
                          Function Context
                                    │
                               Execution
                                    │
                                  Return
                                    │
                                    ▼
                              Context Ends
```

این مدل ذهنی، ارتباط میان مفاهیم اصلی فصل را نشان می‌دهد.

---

# Global Context در برابر Function Context

اکنون می‌توانیم دو Context اصلی را مقایسه کنیم.

| ویژگی      | Global Execution Context                | Function Execution Context      |
| ---------- | --------------------------------------- | ------------------------------- |
| هدف        | اجرای Global Code                       | اجرای Function                  |
| زمان ایجاد | هنگام آغاز اجرای Script                 | هنگام Function Invocation       |
| Local Data | مربوط به Global Environment             | مربوط به Function               |
| Arguments  | ندارد به معنای Function Arguments       | مربوط به Invocation             |
| Lifecycle  | تا پایان اجرای Global Code              | تا پایان اجرای Function         |
| `this`     | دارای Context مربوط به Global Execution | وابسته به Function و Invocation |

این جدول یک مدل آموزشی است و نباید آن را با جزئیات کامل Specification یکسان دانست.

---

# Execution Context و Runtime چه تفاوتی دارند؟

در فصل قبل با **Runtime** آشنا شدیم.

اکنون باید این دو مفهوم را از هم جدا کنیم.

### Runtime

Runtime محیطی است که JavaScript در آن اجرا می‌شود و می‌تواند شامل مواردی مانند:

* JavaScript Engine
* Web APIs
* Host Capabilities
* سایر اجزای محیط اجرا

باشد.

### Execution Context

Execution Context محیط مفهومی مربوط به اجرای یک قطعه Code است.

بنابراین:

```text
Runtime
   │
   └── JavaScript Engine
           │
           └── Execution Context
```

این نمودار فقط یک مدل ذهنی ساده است و قصد ندارد Architecture دقیق Browser Runtime را نمایش دهد.

---

# Execution Context و Scope

Execution Context و Scope ارتباط نزدیکی دارند، اما یکی نیستند.

به‌صورت مقدماتی:

```text
Execution Context
        ↓
Environment
        ↓
Identifier Accessibility
        ↓
Scope
```

در این فصل تنها همین رابطه را می‌شناسیم.

در فصل **Scope** بررسی خواهیم کرد که Scope چیست، Global Scope و Function Scope چگونه کار می‌کنند و Accessibility چگونه تعیین می‌شود.

---

# Execution Context و Hoisting

Creation Phase توضیح می‌دهد که چرا پیش از اجرای Code، Environment و Bindingها آماده می‌شوند.

همین موضوع پایه‌ای برای درک Hoisting است.

اما:

> **Execution Context فقط Hoisting نیست.**

Hoisting یکی از رفتارهایی است که با آماده‌سازی Environment در Execution ارتباط دارد.

رفتار دقیق `var`، `let`، `const`، Function Declaration و TDZ در فصل **Hoisting and Temporal Dead Zone** بررسی خواهد شد.

---

# Execution Context و Call Stack

در این فصل دیدیم که Function Invocation می‌تواند یک Function Execution Context ایجاد کند.

اما هنوز نگفتیم:

> چند Execution Context چگونه مدیریت می‌شوند؟

یا:

> وقتی یک Function، Function دیگری را فراخوانی می‌کند چه اتفاقی می‌افتد؟

پاسخ این پرسش‌ها در فصل بعد یعنی **Call Stack** بررسی خواهد شد.

در اینجا تنها رابطه را به خاطر بسپارید:

```text
Function Call
      ↓
Execution Context
      ↓
Call Stack
```

---

# دیدگاه Jonas

Jonas Schmedtmann در آموزش JavaScript، مباحث **Behind the Scenes** را برای ساختن یک مدل ذهنی از اجرای Code مطرح می‌کند.

Execution Context یکی از مفاهیم کلیدی این مدل است.

نکته مهم در این دیدگاه این است که برنامه‌نویس نباید فقط Syntax را ببیند.

برای مثال، هنگام مشاهده:

```javascript
calculateTotal(100, 2);
```

باید بتواند فراتر از Syntax فکر کند:

```text
Function Invocation
        ↓
Execution Context
        ↓
Local Environment
        ↓
Code Execution
        ↓
Return
```

این مدل ذهنی بعدها برای درک Call Stack، Scope، Closures و رفتار `this` اهمیت بیشتری پیدا می‌کند.

---

# اشتباهات رایج

## اشتباه اول: Execution Context همان Memory است.

خیر.

Memory یکی از بخش‌های مورد نیاز برای اجرای Program است.

Execution Context مدل گسترده‌تری برای توصیف وضعیت Execution است.

---

## اشتباه دوم: هر Statement یک Execution Context ایجاد می‌کند.

خیر.

Execution Context در ارتباط با نوع Code و Execution آن ایجاد می‌شود.

برای مثال، Function Invocation باعث ایجاد Function Execution Context می‌شود.

---

## اشتباه سوم: Function فقط یک بار Context ایجاد می‌کند.

خیر.

هر Invocation می‌تواند Execution Context مربوط به همان Invocation را ایجاد کند.

---

## اشتباه چهارم: Creation Phase یعنی JavaScript کل Code را اجرا می‌کند.

خیر.

Creation Phase مرحله آماده‌سازی Environment و Bindingهای لازم است.

اجرای معمول Code در Execution Phase انجام می‌شود.

---

## اشتباه پنجم: Scope و Execution Context یکی هستند.

خیر.

این دو مفهوم به هم مرتبط‌اند اما یکسان نیستند.

Scope درباره Accessibility است.

Execution Context وضعیت اجرای Code را مدل می‌کند.

---

## اشتباه ششم: Execution Context همان Call Stack است.

خیر.

Execution Context محیط اجرای Code را توصیف می‌کند.

Call Stack ساختار مدیریت Contextهای فعال را در زمان اجرای Functionها بررسی می‌کند.

---

# Key Points

* Execution Context مدل مفهومی وضعیت اجرای Code است.
* JavaScript برای اجرای Global Code یک Global Execution Context دارد.
* Function Invocation با ایجاد Function Execution Context همراه است.
* Creation Phase محیط و Bindingهای لازم را آماده می‌کند.
* Execution Phase Code را اجرا می‌کند.
* Execution Context یک Lifecycle دارد:

  ```text
  Creation → Execution → Removal
  ```
* Environment و Bindingها بخشی از مدل Execution Context هستند.
* Scope با Execution Context مرتبط است اما با آن یکسان نیست.
* `this` با Execution Context ارتباط دارد، اما Rules آن در فصل اختصاصی خودش بررسی می‌شود.
* Call Stack در فصل بعد توضیح داده خواهد شد.
* Hoisting در فصل بعدتر به‌صورت مستقل بررسی خواهد شد.

---

# خلاصه فصل

در فصل قبل دیدیم که JavaScript Engine چگونه Source Code را برای Execution آماده می‌کند.

در این فصل یک قدم جلوتر رفتیم و بررسی کردیم که **Code هنگام اجرا در چه محیطی قرار می‌گیرد.**

با مفهوم **Execution Context** آشنا شدیم.

Execution Context یک مدل مفهومی برای توضیح وضعیت لازم جهت اجرای Code است.

ابتدا **Global Execution Context** را بررسی کردیم.

این Context محیط اجرای Global Code است و نقطه شروع اجرای یک Script محسوب می‌شود.

سپس **Function Execution Context** را بررسی کردیم.

هرگاه یک Function فراخوانی شود، JavaScript باید محیط لازم برای اجرای همان Invocation را آماده کند.

در ادامه دیدیم که Execution Context را می‌توان از طریق دو مرحله اصلی درک کرد:

```text
Creation Phase
↓
Execution Phase
```

در Creation Phase، Environment و Bindingهای لازم برای Execution آماده می‌شوند.

در Execution Phase، JavaScript Code را اجرا می‌کند، Expressionها را ارزیابی می‌کند و Assignmentها را انجام می‌دهد.

همچنین Lifecycle یک Context را به شکل زیر مدل کردیم:

```text
Creation
↓
Execution
↓
Removal
```

در پایان، رابطه Execution Context را با مفاهیمی که در فصل‌های بعدی خواهیم خواند بررسی کردیم:

```text
Execution Context
        ↓
Call Stack
        ↓
Scope
        ↓
Scope Chain
        ↓
Hoisting
        ↓
this
```

اما هر کدام از این مفاهیم در جایگاه خودشان آموزش داده خواهند شد.

---

# Key Takeaways

در پایان این فصل باید بتوانید:

* Execution Context را به‌عنوان محیط مفهومی اجرای Code تعریف کنید.
* تفاوت Global Execution Context و Function Execution Context را توضیح دهید.
* بدانید Function Context هنگام Invocation ایجاد می‌شود.
* Creation Phase را مرحله آماده‌سازی Environment و Bindingها بدانید.
* Execution Phase را مرحله اجرای واقعی Code بدانید.
* Lifecycle یک Context را توضیح دهید.
* Execution Context را با Runtime، Scope و Call Stack اشتباه نگیرید.
* بدانید `this` با Execution Context ارتباط دارد، اما Rules آن هنوز موضوع این فصل نیست.
* بدانید Creation Phase پایه‌ای برای درک Hoisting است.
* ارتباط Execution Context و Call Stack را در حد Preview توضیح دهید.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Execution Context چیست؟

### پاسخ

Execution Context محیط مفهومی‌ای است که JavaScript برای اجرای یک قطعه Code ایجاد می‌کند و وضعیت لازم برای آن Execution را مدیریت می‌کند.

---

### سؤال ۲

Global Execution Context چیست؟

### پاسخ

Global Execution Context محیط اجرای Global Code است و هنگام آغاز اجرای یک Script ایجاد می‌شود.

---

### سؤال ۳

چه زمانی Function Execution Context ایجاد می‌شود؟

### پاسخ

هنگامی که یک Function فراخوانی و اجرا می‌شود، JavaScript Execution Context مربوط به آن Invocation را ایجاد می‌کند.

---

### سؤال ۴

Creation Phase چیست؟

### پاسخ

Creation Phase مرحله‌ای است که JavaScript Environment و Bindingهای لازم برای اجرای Code را آماده می‌کند.

---

### سؤال ۵

Execution Phase چیست؟

### پاسخ

Execution Phase مرحله‌ای است که JavaScript Statementها و Expressionهای Code را واقعاً اجرا و ارزیابی می‌کند.

---

### سؤال ۶

آیا Execution Context همان Scope است؟

### پاسخ

خیر. Scope درباره محدوده دسترسی به Identifierها است، در حالی که Execution Context وضعیت لازم برای اجرای Code را مدل می‌کند.

---

### سؤال ۷

آیا Execution Context همان Runtime است؟

### پاسخ

خیر. Runtime محیط اجرای JavaScript است، در حالی که Execution Context وضعیت اجرای یک قطعه Code را توصیف می‌کند.

---

## سطح Mid-Level

### سؤال ۸

چرا JavaScript به Execution Context نیاز دارد؟

### پاسخ

زیرا هنگام اجرای Code باید وضعیت لازم مانند Environment، Bindingها و اطلاعات مرتبط با Execution را مدیریت کند. Execution Context یک مدل مشخص برای سازمان‌دهی این وضعیت فراهم می‌کند.

---

### سؤال ۹

تفاوت Global Execution Context و Function Execution Context چیست؟

### پاسخ

Global Execution Context برای اجرای Global Code ایجاد می‌شود، در حالی که Function Execution Context هنگام Invocation یک Function ایجاد می‌شود و وضعیت اجرای همان Function را مدیریت می‌کند.

---

### سؤال ۱۰

چرا Function Invocation باعث ایجاد Execution Context می‌شود؟

### پاسخ

زیرا هر Function باید با وضعیت اجرای مخصوص همان Invocation، از جمله Parameters و Local Bindings، اجرا شود. بنابراین Engine باید Context مناسب آن Execution را فراهم کند.

---

### سؤال ۱۱

Creation Phase چه ارتباطی با Hoisting دارد؟

### پاسخ

در Creation Phase، Environment و Bindingهای لازم پیش از اجرای معمول Code آماده می‌شوند. این فرآیند پایه‌ای برای درک رفتارهایی است که با مفهوم Hoisting توضیح داده می‌شوند، اما جزئیات Hoisting در فصل اختصاصی آن بررسی می‌شود.

---

### سؤال ۱۲

چرا Execution Context را نباید با Memory یکی دانست؟

### پاسخ

Memory فقط بخشی از منابع مورد نیاز برای اجرای Program است. Execution Context مدل گسترده‌تری برای توصیف وضعیت Execution و Environment مربوط به آن است.

---

### سؤال ۱۳

چرا Execution Context را نباید با Call Stack یکی دانست؟

### پاسخ

Execution Context وضعیت اجرای Code را توصیف می‌کند، در حالی که Call Stack نحوه مدیریت Contextهای فعال هنگام اجرای Functionها را مدل می‌کند.

---

### سؤال ۱۴

چرا هر Invocation یک Context مستقل دارد؟

### پاسخ

زیرا هر Invocation باید وضعیت اجرایی مخصوص خودش را داشته باشد؛ برای مثال Arguments و Local Bindings آن Invocation نباید با Invocation دیگری اشتباه شوند.

---

## سطح Senior

### سؤال ۱۵

چرا Execution Context یک مدل ذهنی مهم برای مهندس JavaScript است؟

### پاسخ

زیرا به جای نگاه کردن صرف به Syntax، وضعیت واقعی Execution را مدل می‌کند. این مدل ذهنی پایه درک رفتارهایی مانند Function Execution، Scope، Hoisting، `this` و Call Stack است.

---

### سؤال ۱۶

اگر یک Function دو بار فراخوانی شود، چرا نباید وضعیت Local آن را یک Execution مشترک در نظر بگیریم؟

### پاسخ

زیرا هر Invocation یک Execution مستقل دارد و باید Parameters و Local Bindings مربوط به همان Invocation را مدیریت کند. بنابراین وضعیت اجرای دو Invocation نباید به‌صورت یک Context مشترک مدل شود.

---

### سؤال ۱۷

Creation Phase چه مشکل مفهومی را برای ما حل می‌کند؟

### پاسخ

Creation Phase توضیح می‌دهد که Execution قبل از اجرای معمول Code نیاز به آماده‌سازی Environment و Bindingها دارد. بدون این مدل، رفتارهایی مانند Hoisting و برخی تفاوت‌های Declarationها دشوارتر قابل توضیح هستند.

---

### سؤال ۱۸

چرا نباید Execution Context را صرفاً «محل قرار گرفتن Variableها در Memory» تعریف کنیم؟

### پاسخ

زیرا این تعریف بیش از حد محدود است. Execution Context فقط درباره Variable Storage نیست و اطلاعات و Environment لازم برای اجرای Code را نیز در بر می‌گیرد.

---

### سؤال ۱۹

Execution Context چه ارتباطی با Scope دارد، بدون اینکه این دو مفهوم یکی باشند؟

### پاسخ

Execution Context محیط اجرای Code را مدل می‌کند و Environment مرتبط با آن در تعیین نحوه دسترسی به Identifierها نقش دارد. Scope خود مفهوم مستقلی برای تعیین Accessibility است که در فصل‌های بعد دقیق‌تر بررسی می‌شود.

---

### سؤال ۲۰

چرا آموزش کامل `this` در این فصل از نظر Concept Flow مناسب نیست؟

### پاسخ

زیرا Execution Context فقط زمینه معرفی `this` را فراهم می‌کند، اما مقدار `this` به Rules مربوط به Invocation وابسته است. بنابراین آموزش کامل آن باید بعد از تثبیت Execution Context و در فصل اختصاصی `this` انجام شود.

---

# Golden Answers

## Execution Context چیست؟

> Execution Context محیط مفهومی‌ای است که JavaScript برای اجرای Code ایجاد می‌کند و اطلاعات و وضعیت لازم برای آن Execution را مدیریت می‌کند.

---

## Global Execution Context چیست؟

> Global Execution Context محیط اجرای Global Code است و هنگام شروع اجرای Script ایجاد می‌شود.

---

## Function Execution Context چیست؟

> Function Execution Context هنگام Invocation یک Function ایجاد می‌شود و وضعیت لازم برای اجرای همان Function، مانند Parameters و Local Bindings، را مدیریت می‌کند.

---

## Creation Phase چیست؟

> Creation Phase مرحله آماده‌سازی Execution Context است که در آن Environment و Bindingهای لازم برای اجرای Code آماده می‌شوند.

---

## Execution Phase چیست؟

> Execution Phase مرحله اجرای واقعی Code است که در آن Statementها و Expressionها اجرا و ارزیابی می‌شوند.

---

## Lifecycle یک Execution Context چیست؟

> به‌صورت ساده می‌توان Lifecycle را به سه مرحله تقسیم کرد: **Creation، Execution و Removal**.

---

## Execution Context چه تفاوتی با Scope دارد؟

> Scope محدوده‌ای را مشخص می‌کند که Identifier در آن قابل دسترسی است، اما Execution Context وضعیت لازم برای اجرای Code را مدل می‌کند. این دو مرتبط هستند اما یک مفهوم نیستند.

---

## Execution Context چه تفاوتی با Call Stack دارد؟

> Execution Context وضعیت اجرای Code را توصیف می‌کند، در حالی که Call Stack ساختاری برای مدیریت Contextهای فعال هنگام اجرای Functionهاست.

---

## چرا Creation Phase برای درک Hoisting مهم است؟

> زیرا در Creation Phase محیط و Bindingهای لازم پیش از اجرای معمول Code آماده می‌شوند. رفتارهای مرتبط با Hoisting بر پایه همین مدل قابل درک‌تر هستند.

---

## چرا Execution Context برای Debugging مهم است؟

> زیرا به ما کمک می‌کند هنگام تحلیل رفتار Code بدانیم چه Environment و چه وضعیت اجرایی در زمان اجرای آن Code وجود داشته است.

---

## چرا Function Invocation مهم است؟

> زیرا Invocation نقطه‌ای است که Function واقعاً اجرا می‌شود و JavaScript باید Execution Context مخصوص آن Invocation را ایجاد کند.

---

## چرا `this` فقط در این فصل معرفی شد؟

> زیرا `this` با Execution Context ارتباط دارد، اما مقدار آن به Rules مربوط به Invocation وابسته است. بنابراین تحلیل کامل آن باید در فصل اختصاصی `this` انجام شود.

---

# جمع‌بندی فصل

Execution Context یکی از مفاهیم بنیادی برای عبور از سطح Syntax به سطح **Execution Model** در JavaScript است.

در سطح Syntax ممکن است فقط این Code را ببینیم:

```javascript
const result = calculateTotal(100, 2);
```

اما یک مهندس JavaScript باید بتواند پشت این Syntax را نیز تصور کند:

```text
Program Execution
       ↓
Execution Context
       ↓
Creation Phase
       ↓
Environment
       ↓
Execution Phase
       ↓
Function Invocation
       ↓
Function Execution Context
       ↓
Function Execution
       ↓
Return
       ↓
Context Lifecycle Ends
```

این مدل ذهنی به ما کمک می‌کند رفتار JavaScript را به‌صورت سیستماتیک تحلیل کنیم.

در فصل بعد، همین Execution Contextها را از دید **Call Stack** بررسی خواهیم کرد و خواهیم دید JavaScript چگونه Contextهای فعال را هنگام Function Call، Nested Call و Return مدیریت می‌کند.
