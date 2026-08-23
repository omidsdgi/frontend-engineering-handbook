# Chapter 16 — Callback Functions

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Callback Function** را دقیقاً تعریف کنید.
* توضیح دهید چرا Callbackها در JavaScript به وجود آمده‌اند.
* تفاوت **Synchronous Callback** و **Asynchronous Callback** را درک کنید.
* جریان اجرای Callback را در یک Function تحلیل کنید.
* Callbackها را در Array Methods، Sorting و APIهای سفارشی تشخیص دهید.
* نقش Callback را در `setTimeout` و Event Handling در سطح این فصل توضیح دهید.
* مشکل **Nested Callbacks** و **Callback Hell** را تحلیل کنید.
* توضیح دهید چرا Promiseها به‌عنوان راهکاری برای برخی مشکلات Callbackها مطرح شدند.
* تفاوت نقش Callback و Higher-Order Function را تشخیص دهید.

---

# مقدمه

در فصل قبل یاد گرفتیم که Function در JavaScript فقط مجموعه‌ای از دستورها نیست.

Function می‌تواند مانند یک **Value** در برنامه استفاده شود.

می‌توانیم آن را:

* داخل یک Variable قرار دهیم.
* به‌عنوان Argument به Function دیگری ارسال کنیم.
* داخل یک Object قرار دهیم.
* داخل یک Array ذخیره کنیم.
* از یک Function دیگر برگردانیم.

این قابلیت، پایه یکی از مهم‌ترین الگوهای JavaScript را ایجاد می‌کند:

**Callback Function**

فرض کنید یک Function داریم که باید بعد از انجام یک کار، Function دیگری را اجرا کند.

مثلاً:

```javascript
function processOrder(order, callback) {
  console.log(`Processing order ${order.id}`);

  callback(order);
}
```

در اینجا `processOrder` خودش می‌داند که چه کاری انجام دهد، اما تصمیم گرفته است اجرای بخش دیگری از منطق را به Function دریافت‌شده واگذار کند.

```javascript
processOrder(
  { id: 101 },
  order => console.log(`Order ${order.id} completed`)
);
```

در این مثال، Function دوم یک **Callback** است.

اما یک سؤال مهم‌تر وجود دارد:

> چرا باید اجرای یک Function را به Function دیگری واگذار کنیم؟

پاسخ به این سؤال، نقطه شروع درک Callbackهاست.

---

# Core Question

> **Callback چگونه اجرای یک Function را به Function یا سیستم دیگری واگذار می‌کند؟**

جریان این فصل مطابق Blueprint از این مسیر پیروی می‌کند:

```text
Function as Value
↓
Callback
↓
Synchronous Callback
↓
Asynchronous Callback
↓
Event / Timer
↓
Nested Callbacks
↓
Callback Hell
↓
Promises Motivation
```

این فصل روی **خود Callback و الگوی استفاده از آن** تمرکز دارد.

مباحث داخلی Runtime مانند **Call Stack، Event Loop، Task Queue و Microtask Queue** در فصل‌های مربوط به Runtime و Async JavaScript به‌صورت کامل بررسی خواهند شد.

---

# Block 01 — Callback

## چرا Callback به وجود آمده است؟

فرض کنید یک Function مسئول پردازش یک Order است.

```javascript
function processOrder(order) {
  console.log(`Processing order ${order.id}`);
}
```

این Function می‌تواند Order را پردازش کند، اما شاید بعد از آن بخواهیم کارهای مختلفی انجام دهیم:

* پیام موفقیت نمایش دهیم.
* اطلاعات را ذخیره کنیم.
* یک گزارش ایجاد کنیم.
* UI را به‌روزرسانی کنیم.

اگر همه این رفتارها را داخل Function قرار دهیم:

```javascript
function processOrder(order) {
  console.log(`Processing order ${order.id}`);
  // save order
  // update UI
  // send notification
  // create report
}
```

Function به تدریج به منطق‌های مختلف وابسته می‌شود.

راه دیگری این است که Function تصمیم بگیرد **چه زمانی** باید مرحله بعد اجرا شود، اما تعیین کند **چه کاری** در آن مرحله انجام شود، بر عهده کد فراخواننده باشد.

اینجا Callback وارد می‌شود.

---

## Callback چیست؟

### تعریف ساده

**Callback Function** تابعی است که به‌عنوان یک Value به Function یا API دیگری داده می‌شود تا آن Function یا API در زمان مناسب آن را اجرا کند.

به بیان ساده:

> Callback تابعی است که اجرای آن به Function دیگری واگذار می‌شود.

---

### تعریف فنی

در JavaScript، Callback یک Function است که به‌عنوان Argument به یک Function یا API دیگر ارسال می‌شود تا توسط آن دریافت‌کننده در نقطه‌ای از جریان اجرای خود فراخوانی شود.

بنابراین Callback یک نوع Function جدید نیست.

Callback همان Function است، اما **نقش آن در یک فراخوانی خاص** Callback است.

---

# یک نکته بسیار مهم

این دو کد را با هم مقایسه کنید.

```javascript
processOrder(order, completeOrder);
```

و:

```javascript
processOrder(order, completeOrder());
```

این دو کاملاً متفاوت هستند.

در حالت اول:

```javascript
completeOrder
```

خود Function به‌عنوان Value ارسال می‌شود.

اما در حالت دوم:

```javascript
completeOrder()
```

Function بلافاصله اجرا می‌شود و **نتیجه اجرای آن** ارسال می‌شود.

بنابراین هنگام ارسال Callback معمولاً نباید Function را با `()` اجرا کنیم.

---

## مثال

```javascript
function completeOrder(order) {
  console.log(`Order ${order.id} completed`);
}

function processOrder(order, callback) {
  console.log(`Processing order ${order.id}`);

  callback(order);
}

processOrder({ id: 101 }, completeOrder);
```

خروجی:

```text
Processing order 101
Order 101 completed
```

---

## تحلیل مثال

در این کد:

```javascript
completeOrder
```

به‌عنوان یک Function Value به `processOrder` ارسال شده است.

در نتیجه Parameter زیر:

```javascript
callback
```

به همان Function اشاره می‌کند.

به‌صورت مفهومی:

```text
completeOrder
      ↓
   callback
      ↓
callback(order)
```

پس وقتی این دستور اجرا می‌شود:

```javascript
callback(order);
```

در واقع Function اصلی اجرا می‌شود:

```javascript
completeOrder(order);
```

---

## چرا این الگو مهم است؟

Function `processOrder` دیگر لازم نیست بداند بعد از پردازش Order چه کاری باید انجام شود.

فقط می‌داند که یک Callback دریافت کرده و باید آن را در نقطه مناسب اجرا کند.

در نتیجه، منطق پردازش از منطق مرحله بعد جدا می‌شود.

مثلاً:

```javascript
processOrder(order, showSuccessMessage);
```

یا:

```javascript
processOrder(order, saveOrder);
```

یا:

```javascript
processOrder(order, sendNotification);
```

یک Function می‌تواند با Callbackهای مختلف رفتارهای متفاوتی داشته باشد.

این همان چیزی است که Callback را به یک ابزار مهم برای **Reuse** و **Separation of Concerns** تبدیل می‌کند.

---

# Callback یک مفهوم مستقل از زمان است

یک اشتباه رایج این است که Callback را با **Asynchronous Programming** یکی بدانیم.

این تصور صحیح نیست.

Callback می‌تواند:

* Synchronous باشد.
* Asynchronous باشد.

Callback فقط به این معناست که یک Function به Function دیگری داده شده تا توسط آن اجرا شود.

اگر آن Function بلافاصله Callback را اجرا کند، Callback **Synchronous** است.

اگر Callback را برای اجرای بعدی نگه دارد، می‌تواند **Asynchronous** باشد.

پس:

> هر Asynchronous Callback یک Callback است، اما هر Callback لزوماً Asynchronous نیست.

---

# جریان اجرای Callback

برای درک بهتر، این مثال را بررسی کنید:

```javascript
function greet(name, callback) {
  const message = `Hello ${name}`;

  callback(message);
}

function showMessage(message) {
  console.log(message);
}

greet('Omid', showMessage);
```

جریان مفهومی:

```text
greet()
  ↓
message ساخته می‌شود
  ↓
callback دریافت می‌شود
  ↓
callback(message)
  ↓
showMessage()
```

در اینجا `showMessage` به‌عنوان Callback استفاده شده است.

---

## Callback با Anonymous Function

Callback لازم نیست حتماً یک Function دارای نام باشد.

می‌توانیم آن را مستقیماً نیز ارسال کنیم:

```javascript
processOrder(
  { id: 101 },
  order => console.log(`Order ${order.id} completed`)
);
```

در این حالت Arrow Function مستقیماً به‌عنوان Callback استفاده شده است.

این الگو در JavaScript بسیار رایج است.

---

# Common Mistakes

### اشتباه اول: اجرای Callback هنگام ارسال

❌

```javascript
processOrder(order, completeOrder());
```

✔

```javascript
processOrder(order, completeOrder);
```

در حالت اول Function اجرا می‌شود.

در حالت دوم Function به‌عنوان Value ارسال می‌شود.

---

### اشتباه دوم: تصور اینکه Callback نوع خاصی از Function است

Callback یک نوع Function نیست.

یک Function به دلیل **نحوه استفاده از آن** نقش Callback پیدا می‌کند.

---

### اشتباه سوم: یکی دانستن Callback و Asynchronous

Callback می‌تواند Synchronous یا Asynchronous باشد.

این دو مفهوم یکی نیستند.

---

# نکات مهم

* Callback یک Function است که به Function یا API دیگری ارسال می‌شود.
* Callback توسط دریافت‌کننده در زمان مناسب اجرا می‌شود.
* Callback معمولاً بدون `()` ارسال می‌شود.
* Callback می‌تواند Named یا Anonymous باشد.
* Callback می‌تواند Synchronous یا Asynchronous باشد.
* Callback باعث جدا شدن منطق اصلی از رفتار بعدی می‌شود.

---

# Block 02 — Synchronous Callbacks

اکنون که مفهوم Callback را شناختیم، باید با اولین نوع مهم آن آشنا شویم:

**Synchronous Callback**

---

## Synchronous Callback چیست؟

### تعریف ساده

Synchronous Callback تابعی است که توسط Function دریافت‌کننده، در همان جریان اجرای فعلی و بدون به تعویق انداختن اجرای آن، فراخوانی می‌شود.

به بیان ساده:

> Function اصلی Callback را دریافت می‌کند و قبل از ادامه اجرای خود آن را اجرا می‌کند.

---

## مثال ساده

```javascript
function processProduct(product, callback) {
  console.log(`Processing ${product.name}`);

  callback(product);

  console.log('Finished');
}

processProduct(
  { name: 'Laptop' },
  product => console.log(`Validated ${product.name}`)
);
```

خروجی:

```text
Processing Laptop
Validated Laptop
Finished
```

ترتیب اجرا مهم است.

ابتدا:

```javascript
console.log(`Processing ${product.name}`);
```

سپس Callback:

```javascript
callback(product);
```

و بعد:

```javascript
console.log('Finished');
```

اجرا می‌شود.

---

# Callback در Array Methods

یکی از رایج‌ترین کاربردهای Synchronous Callback در JavaScript، APIهای مربوط به Array است.

برای مثال:

```javascript
const prices = [100, 200, 300];

prices.forEach(price => {
  console.log(price);
});
```

Function زیر:

```javascript
price => {
  console.log(price);
}
```

به `forEach` داده شده است.

`forEach` مسئول پیمایش عناصر Array است و Callback را برای هر عنصر اجرا می‌کند.

در این فصل هدف ما آموزش کامل `forEach` یا سایر Array Methods نیست.

آن‌ها در بخش Arrays کتاب به‌صورت مستقل و عمیق بررسی خواهند شد.

در اینجا فقط از آن‌ها برای مشاهده یک کاربرد واقعی Callback استفاده می‌کنیم.

---

## مدل ذهنی

می‌توانیم این رابطه را به شکل زیر تصور کنیم:

```text
Array Method
     ↓
Element
     ↓
Callback
     ↓
Result of Callback
```

برای مثال:

```javascript
prices.forEach(price => {
  console.log(price);
});
```

در هر مرحله، `forEach` Callback را با مقدار مناسب اجرا می‌کند.

---

# Callback در Sorting

Callback در Sorting نیز کاربرد مهمی دارد.

مثلاً:

```javascript
const products = [
  { name: 'Laptop', price: 1200 },
  { name: 'Mouse', price: 50 },
  { name: 'Keyboard', price: 100 }
];

products.sort((a, b) => a.price - b.price);
```

Function زیر:

```javascript
(a, b) => a.price - b.price
```

یک Callback است.

`sort` از این Function برای مقایسه عناصر استفاده می‌کند.

ما در اینجا وارد جزئیات الگوریتم Sorting نمی‌شویم.

نکته مهم برای این فصل این است که:

> API مربوط به Sorting، Function مقایسه را دریافت می‌کند و خودش تصمیم می‌گیرد چه زمانی آن را اجرا کند.

این دقیقاً همان الگوی Callback است.

---

# Callback در Custom API

Callback فقط در APIهای داخلی JavaScript استفاده نمی‌شود.

ما نیز می‌توانیم APIهایی طراحی کنیم که Callback دریافت کنند.

مثال:

```javascript
function calculatePrice(price, callback) {
  const result = price * 1.2;

  callback(result);
}

calculatePrice(100, result => {
  console.log(`Final price: ${result}`);
});
```

در اینجا:

```javascript
result => {
  console.log(`Final price: ${result}`);
}
```

Callback است.

Function `calculatePrice` مسئول محاسبه است.

اما تصمیم درباره نحوه استفاده از نتیجه را به Callback واگذار می‌کند.

---

# چرا Custom Callback مفید است؟

فرض کنید یک Function عمومی داریم:

```javascript
function calculatePrice(price, callback) {
  const result = price * 1.2;

  callback(result);
}
```

می‌توانیم از آن با رفتارهای مختلف استفاده کنیم.

```javascript
calculatePrice(100, result => {
  console.log(result);
});
```

یا:

```javascript
calculatePrice(100, result => {
  savePrice(result);
});
```

یا:

```javascript
calculatePrice(100, result => {
  updateUI(result);
});
```

Function اصلی نیازی ندارد همه این جزئیات را بداند.

این همان مزیت مهم Callback است:

**واگذاری بخشی از رفتار به کد فراخواننده.**

---

# Synchronous Callback در یک جمله

در Synchronous Callback، Function دریافت‌کننده Callback را در همان جریان اجرای خود فراخوانی می‌کند.

---

# Common Mistakes

### اشتباه اول

تصور کنیم Callback همیشه بعداً اجرا می‌شود.

خیر.

```javascript
function run(callback) {
  callback();
  console.log('Done');
}
```

در اینجا Callback قبل از `Done` اجرا می‌شود و Synchronous است.

---

### اشتباه دوم

تصور کنیم هر Function که Argument می‌گیرد، Callback دارد.

خیر.

وجود Callback زمانی مطرح است که یکی از Argumentها یک Function باشد و دریافت‌کننده آن Function را اجرا کند.

---

# نکات مهم

* Synchronous Callback در همان جریان اجرای Function اجرا می‌شود.
* `forEach` و `sort` نمونه‌هایی از APIهای رایج دارای Callback هستند.
* Custom Functionها نیز می‌توانند Callback دریافت کنند.
* Callback باعث جداسازی منطق اصلی از رفتار بعدی می‌شود.
* Callback ذاتاً Asynchronous نیست.

---

# Block 03 — Asynchronous Callbacks

تا اینجا Callbackها را در یک جریان کاملاً Synchronous بررسی کردیم.

اما یکی از مهم‌ترین کاربردهای Callback زمانی ظاهر می‌شود که نتیجه یک عملیات **همین حالا آماده نیست**.

برای مثال:

* یک Timer باید منتظر بماند.
* کاربر بعداً روی یک دکمه کلیک می‌کند.
* یک عملیات مربوط به Browser در آینده کامل می‌شود.

در این شرایط، Function دریافت‌کننده نمی‌تواند Callback را همان لحظه اجرا کند.

در نتیجه Callback برای اجرای بعدی نگهداری می‌شود.

---

## Asynchronous Callback چیست؟

### تعریف ساده

Asynchronous Callback تابعی است که برای اجرا در زمان آینده به یک API یا سیستم دیگر داده می‌شود.

---

### تعریف فنی

Asynchronous Callback تابعی است که اجرای آن به زمان تکمیل یا وقوع یک عملیات دیگر وابسته است و بنابراین Function دریافت‌کننده آن را در همان لحظه اجرا نمی‌کند.

نکته مهم:

> خود Callback باعث Asynchronous شدن برنامه نمی‌شود.

این API یا محیط اجراست که تعیین می‌کند Callback چه زمانی فراخوانی شود.

---

# Timer و `setTimeout`

یکی از ساده‌ترین مثال‌ها:

```javascript
setTimeout(() => {
  console.log('Order reminder');
}, 2000);
```

Function زیر:

```javascript
() => {
  console.log('Order reminder');
}
```

یک Callback است.

`setTimeout` آن را دریافت می‌کند و درخواست می‌کند که Callback پس از حداقل تأخیر مشخص‌شده قابل اجرا شود.

در اینجا:

```javascript
2000
```

به معنای ۲۰۰۰ میلی‌ثانیه، یعنی حدود ۲ ثانیه است.

---

## چرا Callback بلافاصله اجرا نمی‌شود؟

زیرا `setTimeout` برای اجرای Callback در زمان آینده طراحی شده است.

پس:

```javascript
setTimeout(() => {
  console.log('Later');
}, 2000);

console.log('Now');
```

خروجی معمول:

```text
Now
Later
```

است.

در این فصل لازم نیست بدانیم Browser یا Runtime دقیقاً با **Call Stack، Queue و Event Loop** چگونه این کار را انجام می‌دهد.

آن سازوکار در فصل‌های آینده بررسی خواهد شد.

در اینجا فقط باید مدل زیر را داشته باشیم:

```text
Register Callback
       ↓
Wait for the relevant condition
       ↓
Callback becomes executable
       ↓
Callback runs
```

---

# Event Callback

یکی دیگر از کاربردهای مهم Callback در Event Handling است.

برای مثال:

```javascript
button.addEventListener('click', () => {
  console.log('Button clicked');
});
```

Function زیر:

```javascript
() => {
  console.log('Button clicked');
}
```

یک Callback است.

Browser آن را دریافت می‌کند و زمانی که Event مربوط به `click` رخ دهد، Callback را اجرا می‌کند.

در اینجا نیز Function را خودمان مستقیماً اجرا نمی‌کنیم.

ما فقط Function را در اختیار سیستم قرار می‌دهیم.

---

# تفاوت Timer و Event

در Timer:

```javascript
setTimeout(callback, 2000);
```

شرط اجرای Callback بر اساس زمان تعیین می‌شود.

در Event:

```javascript
button.addEventListener('click', callback);
```

شرط اجرای Callback وقوع یک Event است.

اما الگوی اصلی یکسان است:

```text
Give Function
      ↓
System waits
      ↓
Condition occurs
      ↓
System executes Function
```

---

# Browser APIs

در Browser، بسیاری از APIها به Callback متکی هستند.

برای مثال:

* Timer APIs
* Event APIs
* برخی APIهای قدیمی Browser

در این موارد JavaScript Function را در اختیار Browser قرار می‌دهد و Browser در زمان مناسب آن را فراخوانی می‌کند.

این موضوع یکی از نقاط اتصال مهم میان **JavaScript Language** و **Host Environment** است.

اما جزئیات Runtime و Event Loop در این فصل آموزش داده نمی‌شود.

---

# Synchronous vs Asynchronous Callback

| ویژگی                     | Synchronous Callback  | Asynchronous Callback          |
| ------------------------- | --------------------- | ------------------------------ |
| زمان اجرا                 | در همان جریان فعلی    | در زمان آینده                  |
| انتظار برای رویداد/عملیات | ندارد                 | دارد                           |
| مثال                      | `forEach`             | `setTimeout`                   |
| اجرا توسط                 | Function دریافت‌کننده | API / Host Environment         |
| مفهوم اصلی                | ترتیب اجرای فوری      | اجرای وابسته به زمان یا رویداد |

نکته مهم:

**Callback بودن** و **Asynchronous بودن** دو ویژگی متفاوت هستند.

---

# Common Mistakes

### اشتباه اول: `setTimeout` دقیقاً بعد از ۲ ثانیه اجرا می‌شود

این برداشت دقیق نیست.

عدد Timeout یک **حداقل تأخیر** برای قابل اجرا شدن Callback را مشخص می‌کند، نه تضمین اجرای دقیق در همان لحظه.

جزئیات این رفتار در مباحث Event Loop بررسی خواهد شد.

---

### اشتباه دوم: Callback خودش Thread جدید ایجاد می‌کند

خیر.

Callback صرفاً یک Function است.

نحوه زمان‌بندی اجرای آن به API و Runtime مربوط است.

---

### اشتباه سوم: تمام Callbackها Asynchronous هستند

خیر.

برای مثال:

```javascript
[1, 2, 3].forEach(value => {
  console.log(value);
});
```

Callback این مثال Synchronous است.

---

# نکات مهم

* Asynchronous Callback برای اجرای آینده در اختیار API یا سیستم قرار می‌گیرد.
* `setTimeout` نمونه‌ای ساده از این الگو است.
* Event Handler نیز یک Callback است.
* Browser می‌تواند Callback را در واکنش به Event اجرا کند.
* جزئیات اجرای Async Callback به Runtime و Host Environment وابسته است.
* Event Loop در فصل‌های آینده به‌صورت کامل بررسی خواهد شد.

---

# Block 04 — Callback Problems

Callbackها مشکل مهمی را حل می‌کنند.

اما استفاده نادرست یا بیش از حد از Callbackها می‌تواند مشکل جدیدی ایجاد کند.

مهم‌ترین مشکل زمانی ظاهر می‌شود که یک Callback، Callback دیگری ایجاد کند و این ساختار چندین بار تکرار شود.

---

# Nested Callbacks

فرض کنید چند عملیات باید به ترتیب انجام شوند.

```text
Create Order
↓
Save Order
↓
Send Notification
↓
Update UI
```

اگر هر عملیات بعد از پایان عملیات قبلی اجرا شود، ممکن است ساختاری شبیه این ایجاد شود:

```javascript
createOrder(order, createdOrder => {
  saveOrder(createdOrder, savedOrder => {
    sendNotification(savedOrder, notification => {
      updateUI(notification);
    });
  });
});
```

در اینجا یک Callback داخل Callback دیگر قرار گرفته است.

به این ساختار **Nested Callback** گفته می‌شود.

---

# چرا Nesting مشکل ایجاد می‌کند؟

در مثال کوچک بالا شاید مشکل چندان جدی به نظر نرسد.

اما با افزایش تعداد مراحل، عمق تو در تو بودن Callbackها بیشتر می‌شود.

برای مثال:

```javascript
stepOne(data, result1 => {
  stepTwo(result1, result2 => {
    stepThree(result2, result3 => {
      stepFour(result3, result4 => {
        stepFive(result4, finalResult => {
          // ...
        });
      });
    });
  });
});
```

اکنون ساختار کد از نظر بصری نیز پیچیده شده است.

خواننده باید برای درک جریان برنامه، از چندین سطح Callback عبور کند.

---

# Callback Hell

### تعریف ساده

**Callback Hell** وضعیتی است که در آن تعداد زیادی Callback به‌صورت تو در تو قرار می‌گیرند و ساختار کد را دشوار و پیچیده می‌کنند.

گاهی این ساختار به شکل یک هرم یا مثلث دیده می‌شود:

```text
stepOne(
  stepTwo(
    stepThree(
      stepFour(
        stepFive()
      )
    )
  )
)
```

به همین دلیل اصطلاح **Pyramid of Doom** نیز برای توصیف این الگو استفاده شده است.

---

# مشکل اصلی Callback Hell چیست؟

Callback Hell فقط مشکل زیبایی کد نیست.

مشکلات مهم‌تری ایجاد می‌کند.

### 1. کاهش خوانایی

درک جریان اجرای برنامه سخت‌تر می‌شود.

---

### 2. افزایش Coupling

مراحل مختلف بیش از حد به یکدیگر وابسته می‌شوند.

---

### 3. دشوار شدن Error Handling

وقتی چند عملیات تو در تو باشند، مدیریت خطا در هر مرحله پیچیده‌تر می‌شود.

---

### 4. دشوار شدن Maintenance

تغییر یک مرحله ممکن است نیازمند تغییر چندین Callback تو در تو باشد.

---

### 5. دشوار شدن Testing

هرچه وابستگی میان مراحل بیشتر شود، تست کردن هر بخش به‌صورت مستقل دشوارتر می‌شود.

---

# آیا Callback بد است؟

خیر.

این نتیجه‌گیری اشتباه است.

Callback یکی از ابزارهای بنیادی JavaScript است و هنوز هم کاربردهای بسیار زیادی دارد.

مشکل از خود Callback نیست.

مشکل زمانی ایجاد می‌شود که:

> تعداد زیادی عملیات وابسته را با Nesting عمیق Callbackها مدل کنیم.

حتی در JavaScript مدرن نیز Callbackها همچنان در Event Handling و بسیاری از APIها استفاده می‌شوند.

---

# یک مدل ذهنی بهتر

Callback را به‌عنوان یک **قرارداد اجرایی** در نظر بگیرید.

Function اصلی می‌گوید:

> «من این کار را انجام می‌دهم؛ وقتی به نقطه مشخصی رسیدم، این Function را اجرا می‌کنم.»

در ساختار ساده:

```text
Operation
   ↓
Callback
```

در ساختار پیچیده:

```text
Operation
   ↓
Callback
   ↓
Operation
   ↓
Callback
   ↓
Operation
   ↓
Callback
```

مشکل از زمانی شروع می‌شود که این زنجیره بیش از حد عمیق شود.

---

# Maintainability

کدی که Callbackهای زیادی دارد ممکن است کاملاً درست اجرا شود، اما همچنان از نظر مهندسی کیفیت پایینی داشته باشد.

**Correctness** به این معنا نیست که کد حتماً **Maintainable** است.

در طراحی نرم‌افزار، باید علاوه بر اجرای صحیح، به این موارد نیز توجه کنیم:

* خوانایی
* جداسازی مسئولیت‌ها
* تست‌پذیری
* قابلیت تغییر
* مدیریت خطا

Callback Hell می‌تواند روی همه این موارد تأثیر منفی بگذارد.

---

# Common Mistakes

### اشتباه اول: هر Nested Callback را Callback Hell بدانیم

هر Nesting ساده‌ای Callback Hell نیست.

مشکل زمانی جدی می‌شود که Nesting عمیق و مدیریت جریان دشوار شود.

---

### اشتباه دوم: حذف کامل Callbackها

Callbackها بخش مهمی از JavaScript هستند.

هدف، حذف Callback نیست.

هدف، استفاده مناسب از آن‌ها و انتخاب Abstraction مناسب برای جریان‌های پیچیده است.

---

### اشتباه سوم: تصور اینکه Callback Hell فقط در کد Asynchronous اتفاق می‌افتد

Nesting از نظر ساختاری می‌تواند در هر نوع Callback رخ دهد.

اما این مشکل در عملیات Asynchronous که چند مرحله وابسته دارند، بسیار محسوس‌تر است.

---

# نکات مهم

* Callback می‌تواند باعث Nesting شود.
* Nesting عمیق باعث کاهش خوانایی و Maintainability می‌شود.
* Callback Hell یک مشکل طراحی و نگهداری است، نه یک خطای Syntax.
* Callback ذاتاً بد نیست.
* مشکل اصلی، مدیریت جریان پیچیده با Nesting زیاد است.

---

# Block 05 — Modern Alternatives

Callbackها برای مدت زیادی ابزار اصلی مدیریت عملیات وابسته و Asynchronous در JavaScript بودند.

اما با پیچیده‌تر شدن Applicationها، محدودیت‌های Callback-based Code بیشتر آشکار شدند.

به‌خصوص زمانی که:

* چند عملیات باید پشت سر هم انجام شوند.
* هر مرحله به نتیجه مرحله قبل وابسته باشد.
* خطاها باید مدیریت شوند.
* تعداد Callbackهای تو در تو افزایش پیدا کند.

در چنین شرایطی، JavaScript راهکارهای دیگری نیز در اختیار ما قرار می‌دهد.

دو مفهوم مهم عبارت‌اند از:

* **Promise**
* **async/await**

---

# Promise چیست؟

در این فصل Promise را آموزش نمی‌دهیم.

فقط باید بدانیم که Promise یک Abstraction برای نمایش نتیجه یک عملیات Asynchronous است.

به‌صورت مفهومی:

```text
Async Operation
       ↓
    Promise
       ↓
 Future Result
```

Promise به ما اجازه می‌دهد جریان عملیات Asynchronous را به شکلی ساختاریافته‌تر مدل کنیم.

برای مثال، به‌جای ساختاری مانند:

```javascript
operationA(data, resultA => {
  operationB(resultA, resultB => {
    operationC(resultB, resultC => {
      // ...
    });
  });
});
```

در آینده می‌توان چنین جریان‌هایی را با Promiseها به شکل قابل ترکیب‌تری مدیریت کرد.

جزئیات Promise در فصل مربوط به **Promises** آموزش داده خواهد شد.

---

# async/await

بعد از Promiseها، JavaScript Syntax دیگری در اختیار ما قرار می‌دهد:

```javascript
async
await
```

هدف این Syntax ساده‌تر کردن خوانایی کدهای Promise-based است.

برای مثال، جریان مفهومی:

```text
Operation A
↓
Operation B
↓
Operation C
```

می‌تواند از نظر ظاهری به کدی نزدیک شود که خواندن آن مانند یک جریان معمولی Synchronous ساده‌تر است.

اما:

> `async/await` هنوز در این فصل آموزش داده نمی‌شود.

این مفهوم در بخش Async JavaScript و پس از آموزش Promiseها بررسی خواهد شد.

---

# آیا Promise و async/await Callback را حذف می‌کنند؟

خیر.

این یک تصور رایج اما نادرست است.

Callback همچنان در JavaScript کاربرد دارد.

برای مثال:

```javascript
button.addEventListener('click', handleClick);
```

همچنان یک Callback-based API است.

Promise و `async/await` بیشتر برای مدیریت و سازمان‌دهی جریان‌های Asynchronous پیچیده‌تر اهمیت پیدا می‌کنند.

---

# چرا Promiseها مطرح شدند؟

در سطح این فصل، انگیزه اصلی را می‌توان این‌گونه خلاصه کرد:

```text
Callback
↓
Nested Callbacks
↓
Callback Hell
↓
Harder Maintenance
↓
Need for Better Abstraction
↓
Promise
```

این یک **Preview** است.

در فصل Promiseها، مفهوم Promise، Stateها، `then`، `catch`، `finally` و Chaining به‌صورت کامل بررسی خواهند شد.

---

# Common Mistakes

### اشتباه اول: Promise را نوع جدیدی از Callback بدانیم

Promise و Callback دو مفهوم متفاوت هستند.

---

### اشتباه دوم: فکر کنیم async/await جایگزین کامل Promise است

`async/await` بر پایه Promiseها کار می‌کند.

جزئیات آن در فصل‌های آینده بررسی خواهد شد.

---

### اشتباه سوم: فکر کنیم Callback دیگر در JavaScript مدرن استفاده نمی‌شود

Callback همچنان بخش مهمی از بسیاری از APIهای JavaScript و Browser است.

---

# نکات مهم

* Callbackها همچنان در JavaScript مدرن کاربرد دارند.
* Promiseها برای مدیریت بهتر عملیات Asynchronous معرفی شده‌اند.
* `async/await` Syntax خواناتری برای کار با Promiseها فراهم می‌کند.
* Promise و `async/await` در این فصل فقط Preview هستند.
* جزئیات Promise در فصل آینده مربوط به Async Programming بررسی خواهد شد.

---

# Summary

در این فصل با یکی از مهم‌ترین الگوهای JavaScript یعنی **Callback Function** آشنا شدیم.

در فصل قبل یاد گرفتیم که Function در JavaScript مانند یک Value قابل استفاده است.

Callback مستقیماً بر همین قابلیت بنا شده است.

وقتی یک Function را به Function یا API دیگری ارسال می‌کنیم تا دریافت‌کننده آن Function را در زمان مناسب اجرا کند، آن Function در آن موقعیت یک **Callback** است.

سپس دیدیم که Callback دو شکل مهم دارد.

در **Synchronous Callback**، Function دریافت‌کننده Callback را در همان جریان اجرای خود فراخوانی می‌کند.

برای مثال:

```javascript
[1, 2, 3].forEach(value => {
  console.log(value);
});
```

در مقابل، در **Asynchronous Callback**، Callback برای اجرای آینده در اختیار API یا سیستم قرار می‌گیرد.

برای مثال:

```javascript
setTimeout(() => {
  console.log('Later');
}, 2000);
```

همچنین Event Handlerها نمونه دیگری از Callbackهای مورد استفاده در Browser هستند.

در ادامه، با **Nested Callbacks** آشنا شدیم.

وقتی Callbackها به شکل عمیق و متوالی درون یکدیگر قرار بگیرند، خوانایی، Maintainability و مدیریت خطا دشوارتر می‌شود.

این وضعیت را **Callback Hell** می‌نامیم.

در پایان دیدیم که این مشکلات یکی از انگیزه‌های شکل‌گیری Abstractionهای مدرن‌تری مانند **Promise** بوده‌اند.

در این فصل Promise و `async/await` فقط در حد Preview مطرح شدند و آموزش کامل آن‌ها به فصل‌های آینده واگذار می‌شود.

---

# Key Takeaways

در پایان این فصل باید بتوانید:

* Callback یک Function است که به Function یا API دیگری ارسال می‌شود.
* Callback معمولاً بدون `()` ارسال می‌شود.
* Callback یک نوع خاص از Function نیست؛ بلکه یک **نقش** برای Function است.
* Callback می‌تواند Named یا Anonymous باشد.
* Callback می‌تواند Synchronous یا Asynchronous باشد.
* Synchronous Callback در همان جریان اجرای Function دریافت‌کننده اجرا می‌شود.
* `forEach` و `sort` نمونه‌هایی از APIهای دارای Synchronous Callback هستند.
* `setTimeout` نمونه‌ای از API دارای Asynchronous Callback است.
* Event Handlerها نیز از Callback استفاده می‌کنند.
* Asynchronous بودن ویژگی خود Callback نیست؛ نحوه زمان‌بندی اجرای آن اهمیت دارد.
* Nested Callback می‌تواند باعث افزایش پیچیدگی شود.
* Callback Hell نتیجه Nesting عمیق و دشوارشدن مدیریت جریان است.
* Callback ذاتاً بد نیست و همچنان در JavaScript کاربرد دارد.
* Promiseها برای مدیریت ساختاریافته‌تر برخی جریان‌های Asynchronous معرفی شده‌اند.
* `async/await` در آینده برای خواناتر کردن Promise-based Code بررسی خواهد شد.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Callback Function چیست؟

### پاسخ

Callback تابعی است که به‌عنوان Argument به Function یا API دیگری ارسال می‌شود تا دریافت‌کننده آن را در زمان مناسب اجرا کند.

---

### سؤال ۲

آیا Callback یک نوع خاص از Function است؟

### پاسخ

خیر. Callback یک نوع Function نیست؛ هر Function می‌تواند بسته به نحوه استفاده، نقش Callback داشته باشد.

---

### سؤال ۳

تفاوت این دو چیست؟

```javascript
run(callback);
```

و:

```javascript
run(callback());
```

### پاسخ

در حالت اول خود Function ارسال می‌شود، اما در حالت دوم Function ابتدا اجرا شده و نتیجه اجرای آن ارسال می‌شود.

---

### سؤال ۴

Synchronous Callback چیست؟

### پاسخ

Callbackای است که Function دریافت‌کننده آن را در همان جریان اجرای فعلی و بدون به تعویق انداختن اجرا می‌کند.

---

### سؤال ۵

آیا همه Callbackها Asynchronous هستند؟

### پاسخ

خیر. Callback می‌تواند Synchronous یا Asynchronous باشد؛ `forEach` نمونه‌ای از Synchronous Callback و `setTimeout` نمونه‌ای از Asynchronous Callback است.

---

### سؤال ۶

چرا از Callback استفاده می‌کنیم؟

### پاسخ

برای اینکه بتوانیم بخشی از رفتار یک Function را به کد دیگری واگذار کنیم و منطق اصلی را از رفتار بعدی جدا کنیم.

---

## سطح Mid-Level

### سؤال ۷

تفاوت Synchronous Callback و Asynchronous Callback چیست؟

### پاسخ

در Synchronous Callback، Function دریافت‌کننده Callback را در همان جریان اجرای خود فراخوانی می‌کند؛ در Asynchronous Callback، اجرای Callback به وقوع یک رویداد یا تکمیل یک عملیات در آینده وابسته است.

---

### سؤال ۸

چرا Callback باعث افزایش Reusability می‌شود؟

### پاسخ

زیرا Function اصلی می‌تواند منطق عمومی خود را حفظ کند و رفتار متغیر را از طریق Callback دریافت کند. در نتیجه یک Function می‌تواند با Callbackهای مختلف رفتارهای متفاوتی داشته باشد.

---

### سؤال ۹

Callback Hell چیست؟

### پاسخ

Callback Hell وضعیتی است که چندین Callback به‌صورت عمیق و تو در تو قرار می‌گیرند و در نتیجه خوانایی، Maintainability و مدیریت جریان و خطا دشوار می‌شود.

---

### سؤال ۱۰

آیا Callback Hell به این معناست که Callbackها بد هستند؟

### پاسخ

خیر. Callback یک الگوی بنیادی در JavaScript است. مشکل زمانی ایجاد می‌شود که جریان‌های پیچیده با Nesting زیاد Callbackها مدیریت شوند.

---

### سؤال ۱۱

چرا Promiseها به‌عنوان جایگزین Callback-based Code مطرح شدند؟

### پاسخ

Promiseها Abstraction ساختاریافته‌تری برای نمایش و ترکیب نتایج عملیات Asynchronous فراهم می‌کنند و می‌توانند بخشی از مشکلات Nesting و مدیریت جریان در Callback-based Code را کاهش دهند.

---

### سؤال ۱۲

آیا `async/await` جایگزین Callback است؟

### پاسخ

خیر. `async/await` برای کار با Promiseها استفاده می‌شود و Callbackها همچنان در APIهایی مانند Event Handling کاربرد دارند.

---

### سؤال ۱۳

آیا `setTimeout` خودش Callback را ایجاد می‌کند؟

### پاسخ

خیر. ما یک Function را به `setTimeout` می‌دهیم و `setTimeout` مسئول زمان‌بندی اجرای آن Function است.

---

## سطح Senior

### سؤال ۱۴

آیا Callback بودن یک Function به این معناست که آن Function بعداً اجرا خواهد شد؟

### پاسخ

خیر. Callback فقط به نحوه استفاده از Function اشاره دارد. Callback می‌تواند همان لحظه به‌صورت Synchronous اجرا شود یا توسط یک API برای زمان آینده نگهداری شود.

---

### سؤال ۱۵

چرا Asynchronous بودن را نباید ویژگی ذاتی Callback بدانیم؟

### پاسخ

زیرا Callback صرفاً یک Function Value است که برای اجرای آن توسط Function یا API دیگری ارسال شده است. زمان اجرای Callback به رفتار دریافت‌کننده و محیط اجرا بستگی دارد، نه به خود Function.

---

### سؤال ۱۶

مشکل اصلی Callback Hell از دیدگاه مهندسی چیست؟

### پاسخ

مشکل اصلی فقط ظاهر تو در توی کد نیست؛ Nesting عمیق باعث افزایش Coupling، کاهش خوانایی، دشوار شدن مدیریت خطا، سخت‌تر شدن Testing و کاهش Maintainability می‌شود.

---

### سؤال ۱۷

چگونه می‌توان یک Callback-based API را بهتر طراحی کرد؟

### پاسخ

باید مسئولیت‌ها را مشخص نگه داشت، از Nesting غیرضروری جلوگیری کرد، رفتار متغیر را از طریق Callback دریافت کرد و در جریان‌های پیچیده از Abstractionهای مناسب‌تری مانند Promise استفاده کرد.

---

### سؤال ۱۸

آیا Event Handler یک Callback است؟

### پاسخ

بله. وقتی Function را به Event API مانند `addEventListener` می‌دهیم تا هنگام وقوع Event اجرا شود، آن Function در این نقش یک Callback محسوب می‌شود.

---

### سؤال ۱۹

آیا یک Callback می‌تواند خودش Callback دیگری دریافت کند؟

### پاسخ

بله. از نظر زبان JavaScript هیچ محدودیتی وجود ندارد، اما Nesting زیاد می‌تواند ساختار برنامه را پیچیده و نگهداری آن را دشوار کند.

---

### سؤال ۲۰

مهم‌ترین تفاوت Callback و Higher-Order Function چیست؟

### پاسخ

Callback به Functionای گفته می‌شود که به Function یا API دیگری داده می‌شود تا توسط آن اجرا شود؛ Higher-Order Function به Functionای گفته می‌شود که یک یا چند Function را دریافت می‌کند یا یک Function برمی‌گرداند. بنابراین یک Function می‌تواند هم Higher-Order Function باشد و هم Function دیگری را به‌عنوان Callback دریافت کند.

---

# Golden Answers

## Callback Function چیست؟

Callback یک Function است که به Function یا API دیگری به‌عنوان Argument داده می‌شود تا آن Function یا API در زمان مناسب آن را اجرا کند.

---

## آیا Callback همیشه Asynchronous است؟

خیر. Callback می‌تواند Synchronous یا Asynchronous باشد. تفاوت به زمان و نحوه اجرای Callback توسط دریافت‌کننده آن مربوط است.

---

## چرا از Callback استفاده می‌کنیم؟

Callback به ما اجازه می‌دهد بخشی از رفتار یک Function را به کد فراخواننده واگذار کنیم و منطق عمومی را از رفتار متغیر جدا کنیم.

---

## Callback Hell چیست؟

Callback Hell وضعیتی است که در آن Callbackهای متعدد به‌صورت عمیق تو در تو قرار می‌گیرند و در نتیجه خوانایی، Maintainability، Testing و مدیریت خطا دشوارتر می‌شود.

---

## آیا Callbackها منسوخ شده‌اند؟

خیر. Callbackها همچنان در JavaScript و Browser APIs کاربرد دارند. Promise و `async/await` بیشتر برای مدیریت بهتر جریان‌های پیچیده Asynchronous به کار می‌روند.

---

## تفاوت Callback و Higher-Order Function چیست؟

Callback Functionای است که به Function دیگری داده می‌شود تا اجرا شود؛ Higher-Order Function تابعی است که Function دریافت می‌کند یا Function برمی‌گرداند. این دو مفهوم می‌توانند هم‌زمان در یک طراحی وجود داشته باشند.

---

# پاسخ کوتاه طلایی مصاحبه

### سؤال

Callback چیست؟

### پاسخ

Callback یک Function است که به Function یا API دیگری داده می‌شود تا آن را در زمان مناسب اجرا کند. Callback می‌تواند Synchronous یا Asynchronous باشد و برای واگذاری بخشی از رفتار به کد دیگر استفاده می‌شود.

---

### سؤال

آیا Callback و Asynchronous Programming یک مفهوم هستند؟

### پاسخ

خیر. Callback یک Function است که برای اجرا توسط Function یا API دیگری ارسال می‌شود؛ اما Asynchronous بودن به زمان و نحوه اجرای آن Function مربوط است.

---

### سؤال

Callback Hell چرا مشکل‌ساز است؟

### پاسخ

زیرا Nesting عمیق Callbackها باعث افزایش پیچیدگی، کاهش خوانایی و دشوار شدن مدیریت خطا و نگهداری کد می‌شود. Promiseها یکی از Abstractionهایی هستند که برای مدیریت بهتر چنین جریان‌هایی معرفی شدند.

---

# اشتباهات رایج

❌ Callback را نوع خاصی از Function بدانیم.

✔ Callback یک نقش برای Function است.

---

❌ تصور کنیم Callback همیشه Asynchronous است.

✔ Callback می‌تواند Synchronous یا Asynchronous باشد.

---

❌ هنگام ارسال Callback آن را اجرا کنیم.

❌

```javascript
run(callback());
```

✔

```javascript
run(callback);
```

---

❌ تصور کنیم `setTimeout` دقیقاً در زمان مشخص‌شده Callback را اجرا می‌کند.

✔ زمان مشخص‌شده یک تأخیر حداقلی است و زمان‌بندی دقیق اجرای Callback به Runtime وابسته است.

---

❌ تصور کنیم Callback Hell به معنای خراب بودن Callback است.

✔ مشکل، Nesting عمیق و پیچیدگی حاصل از آن است.

---

❌ تصور کنیم Promise و `async/await` تمام Callbackها را حذف کرده‌اند.

✔ Callbackها همچنان بخش مهمی از APIهای JavaScript و Browser هستند.

---

# Conclusion

Callback یکی از مفاهیمی است که از قابلیت **Function as a Value** به وجود می‌آید.

وقتی یک Function را به Function یا API دیگری می‌دهیم تا اجرای آن را در نقطه مناسب مدیریت کند، آن Function نقش Callback پیدا می‌کند.

این الگو در JavaScript بسیار گسترده است.

در APIهای Synchronous مانند برخی Array Methods، Callback در همان جریان اجرا می‌شود.

در APIهای Asynchronous مانند Timerها و Event Handling، Callback برای اجرا در زمان آینده در اختیار سیستم قرار می‌گیرد.

اما استفاده از Callback برای جریان‌های پیچیده می‌تواند به Nested Callbacks و در موارد شدیدتر به Callback Hell منجر شود.

به همین دلیل، شناخت Callback فقط به معنای دانستن Syntax آن نیست.

مدل ذهنی صحیح این است:

```text
Function as Value
        ↓
     Callback
        ↓
 Function/API receives it
        ↓
  Executes it when appropriate
```

و در جریان‌های پیچیده‌تر:

```text
Callbacks
    ↓
Nested Callbacks
    ↓
Callback Hell
    ↓
Need for Better Abstraction
    ↓
Promises
```

Promise و `async/await` در فصل‌های آینده این مسیر را ادامه خواهند داد.

در این فصل فقط انگیزه و جایگاه آن‌ها را شناختیم؛ آموزش سازوکار آن‌ها در فصل‌های مربوط به **Asynchronous JavaScript** انجام خواهد شد.

از اینجا به بعد، یک سؤال مهم‌تر مطرح می‌شود:

> اگر یک Function بعد از پایان اجرای Function بیرونی هنوز بتواند به داده‌های محیط بیرونی خود دسترسی داشته باشد، این داده‌ها چگونه حفظ می‌شوند؟

پاسخ این سؤال ما را به مفهوم **Closure** در فصل بعد می‌رساند.
