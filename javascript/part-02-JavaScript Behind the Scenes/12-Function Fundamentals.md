# Chapter 12 — Function Fundamentals

# اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Function** و نقش آن در طراحی نرم‌افزار را توضیح دهید.
* بدانید چرا Function یکی از Building Blockهای اصلی JavaScript است.
* ساختار **Function Declaration** را درک کنید.
* تفاوت **Parameter** و **Argument** را توضیح دهید.
* نحوه **Invocation** و اجرای یک Function را تحلیل کنید.
* داده را به Function وارد کنید و نتیجه را با `return` دریافت کنید.
* تفاوت **Function Output** و تغییر مستقیم وضعیت برنامه را در سطح مقدماتی تشخیص دهید.
* Functionهای کوچک، واضح و دارای نام معنادار طراحی کنید.
* از **Early Return** برای ساده‌تر کردن منطق Function استفاده کنید.
* مفهوم **Side Effect** را در حد لازم برای طراحی Functionهای قابل پیش‌بینی درک کنید.

---

# Core Question

> **Function چیست و چگونه JavaScript کد را قابل استفاده مجدد می‌کند؟**

---

# مقدمه

در فصل‌های قبل با **Value**، **Variable**، **Data Type**، **Operator** و ساختارهای کنترلی مانند `if` و Loopها آشنا شدیم.

با این مفاهیم می‌توانیم برنامه‌های ساده‌ای بنویسیم.

اما با بزرگ‌تر شدن یک برنامه، مسئله جدیدی ایجاد می‌شود.

فرض کنید در یک Application چندین بار باید قیمت نهایی یک محصول محاسبه شود.

بدون Function ممکن است منطق محاسبه را در چند قسمت مختلف تکرار کنیم:

```javascript
const finalPrice = price * quantity;
```

و چند خط یا چند فایل بعد دوباره:

```javascript
const total = price * quantity;
```

و در بخش دیگری:

```javascript
const amount = price * quantity;
```

اگر منطق محاسبه تغییر کند، باید تمام این قسمت‌ها را پیدا و اصلاح کنیم.

این مشکل فقط مربوط به محاسبه قیمت نیست.

در یک Application واقعی ممکن است منطق‌های زیر بارها مورد نیاز باشند:

* محاسبه قیمت سفارش
* اعتبارسنجی اطلاعات کاربر
* فرمت کردن نام کاربر
* محاسبه مالیات
* بررسی وضعیت سفارش
* تبدیل داده API
* محاسبه امتیاز
* ایجاد پیام مناسب برای کاربر

اینجاست که **Function** اهمیت پیدا می‌کند.

Function به ما اجازه می‌دهد یک منطق مشخص را یک‌بار تعریف کنیم و هر زمان که لازم بود آن را اجرا کنیم.

بنابراین Function فقط یک Syntax جدید نیست.

Function ابزاری برای **سازمان‌دهی و استفاده مجدد از Logic** است.

---

# Block 01 — Function Concept

## Function چیست؟

### تعریف ساده

**Function** یک واحد مستقل از Logic است که برای انجام یک وظیفه مشخص طراحی می‌شود و می‌توان آن را هر زمان که لازم باشد اجرا کرد.

به بیان ساده:

> Function بخشی از کد است که یک وظیفه مشخص را انجام می‌دهد و می‌تواند دوباره مورد استفاده قرار گیرد.

برای مثال:

```javascript
function calculateTotal() {
  // calculate order total
}
```

در اینجا یک Function برای محاسبه Total ایجاد شده است.

---

### تعریف فنی

در JavaScript، Function یک construct قابل فراخوانی (**Callable**) است که مجموعه‌ای از دستورها را در خود جای می‌دهد و می‌تواند در زمان Invocation اجرا شود.

یک Function می‌تواند:

* ورودی دریافت کند.
* Logic مشخصی را اجرا کند.
* خروجی تولید کند.
* یا صرفاً یک عملیات را انجام دهد.

در این فصل تمرکز ما بر **Function Declaration** و مدل پایه Input → Processing → Output است.

مباحثی مانند Function Expression، Arrow Function و Function به‌عنوان Value در فصل‌های بعدی بررسی خواهند شد.

---

## چرا Function مهم است؟

اگر یک Logic فقط یک بار استفاده شود، ممکن است قرار دادن آن Logic در همان نقطه کافی باشد.

اما اگر یک Logic چندین بار مورد نیاز باشد، تکرار آن باعث افزایش هزینه نگهداری می‌شود.

برای مثال:

```javascript
const price = 120;
const quantity = 3;

const total = price * quantity;
```

اگر همین Logic در بخش‌های مختلف Application تکرار شود، تغییر آن دشوار خواهد شد.

با Function:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

اکنون Logic یک‌بار تعریف شده است.

هرجا لازم باشد:

```javascript
calculateTotal(120, 3);
```

و:

```javascript
calculateTotal(50, 4);
```

می‌توان از همان Logic استفاده کرد.

---

## Reusability

یکی از مهم‌ترین اهداف Function، **Code Reusability** است.

Reusability یعنی یک Logic را بتوان بدون کپی کردن دوباره آن، در چند نقطه از برنامه استفاده کرد.

به جای:

```text
Logic
Logic
Logic
Logic
```

می‌توانیم داشته باشیم:

```text
        ┌──────────────┐
        │   Function   │
        └──────┬───────┘
               │
      ┌────────┼────────┐
      ↓        ↓        ↓
    Call     Call     Call
```

این مدل باعث می‌شود Logic در یک مکان متمرکز شود.

---

## Function به‌عنوان Building Block

در یک Application واقعی، Functionها مانند قطعات سازنده برنامه هستند.

یک برنامه بزرگ معمولاً از تعداد زیادی Logic کوچک‌تر تشکیل می‌شود.

برای مثال:

```text
Application
│
├── validateUser()
├── calculateTotal()
├── formatPrice()
├── createOrder()
└── showMessage()
```

هر Function مسئول یک بخش مشخص از Logic است.

این تقسیم‌بندی باعث می‌شود برنامه را بتوان به بخش‌های کوچک‌تر تحلیل کرد.

---

## مثال

فرض کنید Application ما باید قیمت نهایی یک Order را محاسبه کند.

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(120, 3);

console.log(total);
```

خروجی:

```text
360
```

در این مثال Function یک وظیفه مشخص دارد:

> دریافت Price و Quantity و محاسبه Total.

---

## تحلیل مهندسی

نکته مهم این است که Function باعث حذف Logic نمی‌شود.

Logic همچنان وجود دارد:

```javascript
price * quantity
```

اما اکنون این Logic در یک واحد مشخص قرار گرفته است.

در نتیجه:

* Logic متمرکز شده است.
* استفاده مجدد ساده شده است.
* تغییر Logic آسان‌تر شده است.
* هدف Function مشخص است.

این همان تفاوت میان **نوشتن کد** و **سازمان‌دهی کد** است.

---

## اشتباهات رایج

### ❌ Function فقط برای جلوگیری از تکرار کد است.

این یکی از مهم‌ترین کاربردهای Function است، اما تنها کاربرد آن نیست.

Function برای جدا کردن مسئولیت‌ها و ساختن واحدهای قابل فهم Logic نیز استفاده می‌شود.

### ❌ هر چند خط کد باید حتماً Function شود.

Function زمانی مفید است که یک واحد منطقی مشخص ایجاد کند.

تقسیم بیش از حد کد می‌تواند خوانایی را کاهش دهد.

### ❌ Function باید همیشه خروجی داشته باشد.

خیر.

برخی Functionها یک عملیات انجام می‌دهند و خروجی قابل استفاده‌ای تولید نمی‌کنند.

---

## نکات مهم

* Function یک واحد مستقل از Logic است.
* Function می‌تواند ورودی دریافت کند.
* Function می‌تواند خروجی تولید کند.
* Function می‌تواند چندین بار اجرا شود.
* Reusability یکی از مهم‌ترین مزایای Function است.
* Functionها Building Blockهای مهم Application هستند.
* Function باید یک وظیفه منطقی و قابل تشخیص داشته باشد.

---

# Block 02 — Function Declaration

## Function Declaration چیست؟

تا اینجا مفهوم Function را شناختیم.

اکنون باید ببینیم چگونه یک Function را در JavaScript تعریف کنیم.

یکی از روش‌های اصلی تعریف Function، **Function Declaration** است.

Syntax پایه آن:

```javascript
function functionName() {
  // statements
}
```

برای مثال:

```javascript
function showWelcomeMessage() {
  console.log('Welcome to the application');
}
```

در اینجا Functionای با نام:

```text
showWelcomeMessage
```

تعریف شده است.

---

## ساختار Function Declaration

یک Function Declaration معمولاً شامل بخش‌های زیر است:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

اجزای آن:

```text
function
   ↓
function name
   ↓
parameters
   ↓
function body
   ↓
return
```

### `function`

کلمه کلیدی `function` به JavaScript اعلام می‌کند که در حال تعریف یک Function هستیم.

### Function Name

نام Function مشخص می‌کند این Function چه وظیفه‌ای دارد.

```javascript
calculateTotal
```

### Parameters

مقادیر ورودی مورد انتظار Function در بخش Parameter قرار می‌گیرند:

```javascript
(price, quantity)
```

### Function Body

دستورهای داخل `{}` بدنه Function را تشکیل می‌دهند:

```javascript
{
  return price * quantity;
}
```

### `return`

در صورت نیاز، Function نتیجه‌ای را با `return` به محل فراخوانی برمی‌گرداند.

---

# Parameters چیست؟

### تعریف ساده

**Parameter** نامی است که در تعریف Function برای دریافت یک ورودی استفاده می‌شود.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

در اینجا:

```javascript
price
quantity
```

Parameter هستند.

Parameter را می‌توان مانند یک Variable ورودی در نظر گرفت که Function از آن برای اجرای Logic خود استفاده می‌کند.

---

### تعریف فنی

Parameter یک Identifier در Function Definition است که برای دریافت مقدار ورودی هنگام Invocation استفاده می‌شود.

برای مثال:

```javascript
function greetUser(name) {
  console.log(`Hello ${name}`);
}
```

در اینجا `name` یک Parameter است.

---

## مثال

```javascript
function calculateDiscount(price, discount) {
  return price - discount;
}
```

در این Function دو Parameter داریم:

```text
price
discount
```

Function به جای وابسته بودن به یک Price مشخص، می‌تواند با مقادیر مختلف کار کند.

---

## چرا Parameter مهم است؟

بدون Parameter، Function مجبور می‌شود روی داده‌های مشخص و از پیش تعیین‌شده کار کند.

برای مثال:

```javascript
function calculateTotal() {
  const price = 100;
  const quantity = 2;

  return price * quantity;
}
```

این Function فقط با همین دو مقدار کار می‌کند.

اما:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

قابل استفاده مجدد است.

اکنون:

```javascript
calculateTotal(100, 2);
calculateTotal(200, 3);
calculateTotal(50, 10);
```

همگی از همان Logic استفاده می‌کنند.

---

# Arguments چیست؟

### تعریف ساده

**Argument** مقداری است که هنگام فراخوانی Function به آن ارسال می‌کنیم.

برای مثال:

```javascript
calculateTotal(120, 3);
```

در اینجا:

```text
120
3
```

Argument هستند.

در مقابل:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

در اینجا:

```text
price
quantity
```

Parameter هستند.

---

## Parameter vs Argument

این تفاوت را باید دقیق بدانید:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

`price` و `quantity`:

> Parameters

و:

```javascript
calculateTotal(120, 3);
```

`120` و `3`:

> Arguments

پس:

```text
Definition
   ↓
Parameters

Invocation
   ↓
Arguments
```

این دو مفهوم مرتبط‌اند، اما یکسان نیستند.

---

## مثال واقعی

فرض کنید Function وظیفه محاسبه مبلغ سفارش را دارد:

```javascript
function calculateOrderTotal(price, quantity) {
  return price * quantity;
}
```

اکنون:

```javascript
const total = calculateOrderTotal(25, 4);
```

در اینجا:

```text
price    → 25
quantity → 4
```

و Function نتیجه زیر را تولید می‌کند:

```text
100
```

---

# `return` چیست؟

### تعریف ساده

`return` برای پایان دادن به اجرای Function و برگرداندن یک Value به محل فراخوانی استفاده می‌شود.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

وقتی Function اجرا شود، نتیجه Expression زیر:

```javascript
price * quantity
```

به محل فراخوانی برگردانده می‌شود.

---

## مثال

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(120, 3);

console.log(total);
```

خروجی:

```text
360
```

می‌توان این جریان را به شکل زیر دید:

```text
Arguments
   ↓
Function
   ↓
Processing
   ↓
return
   ↓
Output
```

---

# چرا `return` مهم است؟

اگر Function فقط مقدار را داخل خودش محاسبه کند، بخش دیگری از برنامه لزوماً به آن مقدار دسترسی ندارد.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;
}
```

اینجا مقدار محاسبه شده است، اما به محل فراخوانی برگردانده نشده است.

اما:

```javascript
function calculateTotal(price, quantity) {
  const total = price * quantity;

  return total;
}
```

اکنون می‌توان نتیجه را دریافت کرد:

```javascript
const total = calculateTotal(120, 3);
```

---

## Return یک Value است

مقدار برگشتی Function می‌تواند مستقیماً در یک Expression استفاده شود.

```javascript
const total = calculateTotal(120, 3);
```

یا:

```javascript
console.log(calculateTotal(120, 3));
```

یا:

```javascript
const finalPrice = calculateTotal(120, 3) + 50;
```

بنابراین Function می‌تواند مانند یک بخش محاسباتی از برنامه عمل کند.

---

## Function بدون `return`

Function الزاماً نیاز به `return` ندارد.

برای مثال:

```javascript
function showWelcomeMessage(name) {
  console.log(`Welcome ${name}`);
}
```

این Function یک پیام نمایش می‌دهد، اما Value مشخصی را با `return` برنمی‌گرداند.

این نوع Function برای انجام یک **Action** مناسب است.

در مقابل:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

یک **Result** تولید می‌کند.

---

## تحلیل مهندسی

در طراحی Function باید مشخص باشد که Function قرار است:

> **Result تولید کند**

یا:

> **Action انجام دهد**

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

هدف اصلی تولید Result است.

اما:

```javascript
function showMessage(message) {
  console.log(message);
}
```

هدف اصلی انجام یک Action است.

این تمایز در طراحی Functionهای قابل فهم اهمیت دارد.

---

## اشتباهات رایج

### ❌ اشتباه گرفتن Parameter و Argument

```javascript
function greet(name) {}
```

`name` Parameter است.

```javascript
greet('Omid');
```

`'Omid'` Argument است.

---

### ❌ تصور اینکه `return` فقط برای نمایش نتیجه است.

`return` مقدار را به محل فراخوانی برمی‌گرداند.

نمایش نتیجه وظیفه `console.log()` است.

---

### ❌ نوشتن `console.log()` به جای `return`

این دو رفتار یکسان ندارند.

```javascript
function calculateTotal(price, quantity) {
  console.log(price * quantity);
}
```

با:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

متفاوت است.

در حالت اول نتیجه فقط به Console ارسال می‌شود.

در حالت دوم نتیجه به Caller برگردانده می‌شود.

---

## نکات مهم

* Function Declaration با `function` تعریف می‌شود.
* Parameter در زمان Definition مشخص می‌شود.
* Argument در زمان Invocation ارسال می‌شود.
* `return` مقدار را به محل فراخوانی برمی‌گرداند.
* Function می‌تواند بدون `return` نیز معتبر باشد.
* `console.log()` جایگزین `return` نیست.
* Function باید Input و Output خود را تا حد امکان واضح تعریف کند.

---

# Block 03 — Invocation

## Function Invocation چیست؟

تعریف Function به معنای اجرای آن نیست.

این تفاوت بسیار مهم است.

وقتی می‌نویسیم:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

فقط Function را **تعریف** کرده‌ایم.

هنوز Logic داخل آن اجرا نشده است.

برای اجرای Function باید آن را **Invoke** یا **Call** کنیم.

---

## Calling a Function

برای Invocation از نام Function به همراه پرانتز استفاده می‌کنیم:

```javascript
calculateTotal(120, 3);
```

این دستور Function را اجرا می‌کند.

---

## Definition vs Invocation

این دو را نباید با یکدیگر اشتباه گرفت.

### Definition

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

یعنی:

> این Function را تعریف کن.

### Invocation

```javascript
calculateTotal(120, 3);
```

یعنی:

> این Function را با این Input اجرا کن.

مدل ذهنی:

```text
Definition
   ↓
Create the Function

Invocation
   ↓
Execute the Function
```

---

# جریان اجرای یک Function

فرض کنید:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const total = calculateTotal(120, 3);
```

می‌توان جریان را این‌گونه تصور کرد:

```text
calculateTotal(120, 3)
        ↓
price = 120
quantity = 3
        ↓
120 * 3
        ↓
360
        ↓
return 360
        ↓
total = 360
```

این مدل ذهنی برای درک Function بسیار مهم است.

---

# Input → Processing → Output

یک Function ساده را می‌توان با سه بخش اصلی تحلیل کرد:

```text
Input
  ↓
Processing
  ↓
Output
```

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

### Input

```text
price
quantity
```

### Processing

```javascript
price * quantity
```

### Output

```text
result
```

این مدل در بسیاری از Functionهای واقعی قابل استفاده است.

---

## مثال واقعی

```javascript
function calculateDiscountedPrice(price, discount) {
  return price - discount;
}
```

جریان:

```text
price + discount
        ↓
Calculation
        ↓
discounted price
```

Invocation:

```javascript
const finalPrice = calculateDiscountedPrice(500, 50);
```

نتیجه:

```text
450
```

---

# یک Function می‌تواند چند بار Invoke شود

یکی از دلایل اصلی استفاده از Function، امکان اجرای مجدد همان Logic است.

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

const firstOrder = calculateTotal(100, 2);
const secondOrder = calculateTotal(250, 3);
const thirdOrder = calculateTotal(80, 5);
```

هر Invocation یک اجرای مستقل از همان Logic ایجاد می‌کند.

در این مثال:

```text
100 × 2 = 200
250 × 3 = 750
80 × 5  = 400
```

اما Logic محاسبه فقط یک‌بار نوشته شده است.

---

# Function Execution

هنگامی که Function Invoke می‌شود:

1. Inputهای آن مشخص می‌شوند.
2. Function Body اجرا می‌شود.
3. دستورها به ترتیب اجرا می‌شوند.
4. در صورت وجود `return`، مقدار برگشتی تولید می‌شود.
5. اجرای Function پایان می‌یابد.

برای مثال:

```javascript
function calculateOrderTotal(price, quantity) {
  const subtotal = price * quantity;
  const shipping = 20;

  return subtotal + shipping;
}
```

Invocation:

```javascript
const total = calculateOrderTotal(100, 2);
```

جریان:

```text
price = 100
quantity = 2
       ↓
subtotal = 200
       ↓
shipping = 20
       ↓
return 220
```

در این فصل فقط رفتار منطقی Function را بررسی می‌کنیم.

جزئیات Runtime مانند **Execution Context** و **Call Stack** در بخش مربوط به JavaScript Runtime بررسی خواهند شد.

---

# Return و پایان Function

وقتی JavaScript به `return` می‌رسد، اجرای Function همان‌جا پایان می‌یابد.

برای مثال:

```javascript
function getStatus(isActive) {
  return isActive ? 'Active' : 'Inactive';

  console.log('This will not run');
}
```

دستور بعد از `return` اجرا نمی‌شود.

این ویژگی پایه‌ای برای طراحی **Early Return** نیز هست.

---

# Early Return

### تعریف ساده

**Early Return** یعنی Function قبل از رسیدن به انتهای معمول Logic، در صورت برقرار بودن یک شرط، نتیجه را برگرداند و اجرای خود را تمام کند.

برای مثال:

```javascript
function getUserStatus(isActive) {
  if (!isActive) {
    return 'Inactive';
  }

  return 'Active';
}
```

در اینجا اگر:

```javascript
isActive === false
```

باشد، Function فوراً:

```text
Inactive
```

را برمی‌گرداند.

---

## چرا Early Return مهم است؟

Early Return می‌تواند Logic را ساده‌تر کند.

بدون Early Return ممکن است کد به شکل زیر نوشته شود:

```javascript
function getUserStatus(isActive) {
  let status;

  if (isActive) {
    status = 'Active';
  } else {
    status = 'Inactive';
  }

  return status;
}
```

اما:

```javascript
function getUserStatus(isActive) {
  if (!isActive) {
    return 'Inactive';
  }

  return 'Active';
}
```

مستقیم‌تر است.

---

## مثال واقعی

فرض کنید فقط کاربران فعال اجازه ادامه یک عملیات را دارند:

```javascript
function processOrder(isActive) {
  if (!isActive) {
    return 'User is not active';
  }

  return 'Order processed';
}
```

در اینجا شرط نامعتبر در ابتدای Function بررسی می‌شود و در صورت برقرار بودن، Function سریعاً خارج می‌شود.

---

## اشتباهات رایج

### ❌ تصور اینکه تعریف Function باعث اجرای آن می‌شود.

```javascript
function calculateTotal() {
  return 100;
}
```

این کد فقط Function را تعریف می‌کند.

برای اجرا:

```javascript
calculateTotal();
```

لازم است.

---

### ❌ تصور اینکه `return` بعد از اجرای Function انجام می‌شود.

`return` بخشی از Function Body است و هنگام رسیدن Execution به آن اجرا می‌شود.

---

### ❌ قرار دادن Logic غیرضروری بعد از `return`

```javascript
function getPrice() {
  return 100;

  console.log('unreachable');
}
```

این دستور هیچ‌گاه اجرا نمی‌شود.

---

## نکات مهم

* Definition با Invocation متفاوت است.
* Function فقط هنگام Invocation اجرا می‌شود.
* Invocation می‌تواند Argument دریافت کند.
* Function می‌تواند چندین بار Invoke شود.
* `return` اجرای Function را پایان می‌دهد.
* Early Return می‌تواند Logic شرطی را ساده‌تر کند.
* جزئیات Execution Context و Call Stack در این فصل آموزش داده نمی‌شوند.

---

# Block 04 — Function Design

اکنون می‌دانیم چگونه Function تعریف و Invoke می‌شود.

اما دانستن Syntax به‌تنهایی برای نوشتن Function خوب کافی نیست.

ممکن است یک Function کاملاً معتبر از نظر Syntax باشد، اما طراحی مناسبی نداشته باشد.

برای مثال:

```javascript
function processEverything() {
  // validate user
  // calculate order
  // format price
  // save data
  // show message
}
```

این Function ممکن است کار کند، اما مسئولیت‌های زیادی دارد.

در این بخش روی چند اصل پایه برای طراحی Function تمرکز می‌کنیم.

---

# Small Functions

یک Function بهتر است یک Logic مشخص را انجام دهد.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

این Function کوچک است و هدف آن کاملاً مشخص است.

در مقابل:

```javascript
function processOrder() {
  // validate user
  // calculate price
  // update UI
  // save order
  // send message
}
```

مسئولیت‌های زیادی دارد.

---

## چرا Functionهای کوچک مفید هستند؟

Function کوچک معمولاً:

* ساده‌تر خوانده می‌شود.
* ساده‌تر تست می‌شود.
* ساده‌تر تغییر می‌کند.
* ساده‌تر Debug می‌شود.
* قابلیت استفاده مجدد بیشتری دارد.

البته کوچک بودن به معنای کم بودن تعداد خطوط به هر قیمت نیست.

هدف، **یک Logic مشخص** است.

---

# Naming Functions

نام Function باید نشان دهد Function چه کاری انجام می‌دهد.

نام خوب:

```javascript
calculateTotal();
```

یا:

```javascript
validateUser();
```

یا:

```javascript
formatPrice();
```

این نام‌ها Intent را منتقل می‌کنند.

نام ضعیف:

```javascript
doStuff();
```

یا:

```javascript
handleData();
```

این نام‌ها اطلاعات کافی درباره وظیفه Function ارائه نمی‌کنند.

---

## Function Name و Intent

نام Function باید به خواننده کمک کند بدون خواندن جزئیات Implementation، هدف آن را بفهمد.

برای مثال:

```javascript
calculateOrderTotal();
```

از:

```javascript
process();
```

معنادارتر است.

این موضوع بخشی از **Readable Code** است.

---

# Function Responsibility

یک Function بهتر است یک مسئولیت مشخص داشته باشد.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

مسئولیت آن:

> محاسبه Total

است.

نه:

> محاسبه Total + نمایش پیام + تغییر UI + ذخیره اطلاعات.

این اصل در طراحی نرم‌افزار اهمیت زیادی دارد.

---

# Side Effects

### تعریف ساده

**Side Effect** یعنی Function علاوه بر تولید یک Result، تغییری خارج از محاسبه داخلی خود ایجاد کند.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

این Function فقط یک Result تولید می‌کند.

اما:

```javascript
function showMessage(message) {
  console.log(message);
}
```

یک اثر بیرونی دارد:

```text
Console
```

Function دیگری ممکن است:

* داده‌ای را در یک منبع خارجی تغییر دهد.
* UI را تغییر دهد.
* وضعیت Application را تغییر دهد.

در این فصل وارد جزئیات Browser API یا State Management نمی‌شویم.

تنها باید بدانیم که Functionها می‌توانند علاوه بر تولید Output، اثر دیگری نیز روی محیط برنامه داشته باشند.

---

# چرا Side Effect مهم است؟

Functionهایی که فقط بر اساس Input خود Output تولید می‌کنند، معمولاً ساده‌تر قابل درک هستند.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

با دریافت یک Input مشخص، Result مشخصی تولید می‌کند.

اما Functionی که هم محاسبه انجام دهد و هم چند بخش دیگر Application را تغییر دهد، برای درک و Debug کردن پیچیده‌تر می‌شود.

به همین دلیل، هنگام طراحی Function بهتر است مسئولیت و اثر آن مشخص باشد.

---

# مثال طراحی نامناسب

```javascript
function processOrder(price, quantity) {
  const total = price * quantity;

  console.log(`Total: ${total}`);

  return total;
}
```

این Function هم:

1. Total را محاسبه می‌کند.
2. پیام را نمایش می‌دهد.
3. Result را برمی‌گرداند.

در برخی شرایط این طراحی قابل قبول است، اما اگر این Logic در Application بزرگ شود، بهتر است مسئولیت‌ها جدا شوند.

مثلاً:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

function showTotal(total) {
  console.log(`Total: ${total}`);
}
```

اکنون هر Function مسئولیت مشخص‌تری دارد.

---

# Early Return در طراحی Function

Early Return علاوه بر پایان دادن به Function، می‌تواند Structure آن را ساده کند.

برای مثال:

```javascript
function getDiscount(isMember) {
  if (!isMember) {
    return 0;
  }

  return 20;
}
```

این ساختار از ایجاد Nesting غیرضروری جلوگیری می‌کند.

در Functionهای بزرگ‌تر، این الگو می‌تواند خوانایی را به‌طور محسوسی افزایش دهد.

---

# Function Design در پروژه واقعی

فرض کنید یک Application سفارش غذا داریم.

به جای یک Function بزرگ:

```javascript
function processOrder() {
  // validate
  // calculate
  // format
  // display
}
```

می‌توان Logic را به Functionهای مشخص تقسیم کرد:

```javascript
function validateOrder(order) {
  // validation
}

function calculateTotal(order) {
  // calculation
}

function formatPrice(price) {
  // formatting
}

function showOrderSummary(order) {
  // display
}
```

اکنون هر Function یک وظیفه مشخص دارد.

این ساختار زمینه را برای مفاهیم پیشرفته‌تر مانند **Abstraction** و **Higher-Order Functions** در فصل‌های آینده آماده می‌کند، اما آن مفاهیم در این فصل آموزش داده نمی‌شوند.

---

## تحلیل مهندسی

Function خوب فقط Function کوتاه نیست.

یک Function خوب باید:

* هدف مشخص داشته باشد.
* نام معنادار داشته باشد.
* Input مشخصی دریافت کند.
* در صورت نیاز Output مشخصی تولید کند.
* مسئولیت‌های نامرتبط را با هم ترکیب نکند.
* Side Effectهای آن قابل تشخیص باشد.

این ویژگی‌ها باعث می‌شوند Function به یک واحد قابل فهم در Architecture برنامه تبدیل شود.

---

## اشتباهات رایج

### ❌ Function بزرگ را فقط با حذف چند خط کوچک کنید.

هدف Small Function کم کردن تعداد خطوط نیست.

هدف، جدا کردن Logicهای مستقل است.

---

### ❌ نام Function را بیش از حد عمومی انتخاب کنید.

```javascript
process();
```

به‌تنهایی Intent مشخصی ندارد.

---

### ❌ چند مسئولیت نامرتبط را داخل یک Function قرار دهید.

```javascript
function handleEverything() {
  // many unrelated responsibilities
}
```

این طراحی نگهداری Function را دشوار می‌کند.

---

### ❌ Side Effect را پنهان کنید.

اگر Function علاوه بر Result، تغییری در بیرون ایجاد می‌کند، بهتر است رفتار آن برای خواننده قابل تشخیص باشد.

---

## نکات مهم

* Function کوچک باید یک Logic مشخص داشته باشد.
* نام Function باید Intent را منتقل کند.
* Functionهای دارای مسئولیت مشخص ساده‌تر نگهداری می‌شوند.
* Side Effect یعنی Function علاوه بر Logic داخلی، اثر بیرونی ایجاد کند.
* Early Return می‌تواند Conditional Logic را ساده‌تر کند.
* کوچک بودن Function به‌تنهایی معیار کیفیت نیست.
* هدف اصلی Function Design، افزایش خوانایی و Maintainability است.

---

# خلاصه فصل

در این فصل با **Function** به‌عنوان یکی از مهم‌ترین Building Blockهای JavaScript آشنا شدیم.

ابتدا دیدیم که Function بخشی مستقل از Logic است که می‌تواند یک وظیفه مشخص را انجام دهد و چندین بار مورد استفاده قرار گیرد.

سپس با **Function Declaration** آشنا شدیم:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

در این ساختار:

* `calculateTotal` نام Function است.
* `price` و `quantity` Parameter هستند.
* `return` نتیجه را برمی‌گرداند.

در ادامه تفاوت **Parameter** و **Argument** را بررسی کردیم.

Parameter در زمان Definition مشخص می‌شود:

```javascript
function calculateTotal(price, quantity) {}
```

و Argument هنگام Invocation ارسال می‌شود:

```javascript
calculateTotal(120, 3);
```

سپس مفهوم **Invocation** را بررسی کردیم و دیدیم که Definition به معنای اجرای Function نیست.

Function تنها زمانی اجرا می‌شود که آن را Call کنیم.

همچنین مدل ذهنی:

```text
Input
  ↓
Processing
  ↓
Output
```

را برای تحلیل Function معرفی کردیم.

در بخش پایانی، از Syntax فراتر رفتیم و اصول پایه **Function Design** را بررسی کردیم:

* Small Functions
* Meaningful Naming
* Clear Responsibility
* Side Effects
* Early Return

در نتیجه Function را دیگر صرفاً به‌عنوان یک Syntax جدید نمی‌بینیم.

Function یک ابزار مهندسی برای **جدا کردن Logic، استفاده مجدد از Code و ساختن واحدهای قابل فهم نرم‌افزار** است.

---

# Key Takeaways

* Function یک واحد مستقل از Logic است.
* Function می‌تواند چندین بار مورد استفاده قرار گیرد.
* Reusability یکی از اهداف اصلی Function است.
* Function Declaration با کلمه کلیدی `function` ایجاد می‌شود.
* Parameter در Function Definition قرار دارد.
* Argument هنگام Function Invocation ارسال می‌شود.
* Definition با Invocation متفاوت است.
* Function فقط هنگام Invocation اجرا می‌شود.
* `return` یک Value را به محل فراخوانی برمی‌گرداند.
* `console.log()` با `return` یکسان نیست.
* Function می‌تواند بدون `return` نیز استفاده شود.
* Early Return اجرای Function را زودتر پایان می‌دهد.
* Functionهای کوچک و دارای مسئولیت مشخص معمولاً خواناتر و قابل نگهداری‌تر هستند.
* نام Function باید Intent آن را منتقل کند.
* Side Effect یعنی Function علاوه بر Result، اثر دیگری خارج از محاسبه داخلی خود ایجاد کند.
* کیفیت Function فقط با تعداد خطوط آن سنجیده نمی‌شود.
* Function یکی از مهم‌ترین Building Blockهای طراحی نرم‌افزار است.

---

# Technical Interview

## سطح Junior

### سؤال ۱ — Function چیست و چرا در JavaScript استفاده می‌شود؟

### سؤال ۲ — Function Declaration چیست؟

### سؤال ۳ — تفاوت Parameter و Argument چیست؟

### سؤال ۴ — Function چگونه Invoke می‌شود؟

### سؤال ۵ — تفاوت Function Definition و Function Invocation چیست؟

### سؤال ۶ — `return` در Function چه کاری انجام می‌دهد؟

### سؤال ۷ — آیا یک Function می‌تواند بدون `return` باشد؟

### سؤال ۸ — تفاوت `return` و `console.log()` چیست؟

### سؤال ۹ — چرا Functionها باعث Reusability می‌شوند؟

### سؤال ۱۰ — Early Return چیست؟

---

## سطح Mid-Level

### سؤال ۱۱ — چرا بهتر است Function یک مسئولیت مشخص داشته باشد؟

### سؤال ۱۲ — چه تفاوتی میان Input، Processing و Output در یک Function وجود دارد؟

### سؤال ۱۳ — چرا Functionهای کوچک معمولاً قابل نگهداری‌تر هستند؟

### سؤال ۱۴ — چرا نام‌گذاری Function اهمیت دارد؟

### سؤال ۱۵ — Side Effect چیست؟

### سؤال ۱۶ — آیا Function همیشه باید Value برگرداند؟

### سؤال ۱۷ — چرا استفاده از `console.log()` به جای `return` می‌تواند طراحی Function را محدود کند؟

### سؤال ۱۸ — چگونه Early Return می‌تواند خوانایی Function را افزایش دهد؟

### سؤال ۱۹ — آیا یک Function بزرگ لزوماً یک Function بد است؟

### سؤال ۲۰ — چگونه تشخیص می‌دهید دو بخش Logic باید در دو Function جدا قرار بگیرند؟

---

## سطح Senior

### سؤال ۲۱ — Reusability تنها دلیل استفاده از Function است؟

### سؤال ۲۲ — از دید مهندسی نرم‌افزار، یک Function خوب چه ویژگی‌هایی دارد؟

### سؤال ۲۳ — چرا ترکیب چند Responsibility در یک Function می‌تواند Maintainability را کاهش دهد؟

### سؤال ۲۴ — Side Effect چه تأثیری بر Predictability یک Function دارد؟

### سؤال ۲۵ — چگونه بین Small Function و Over-Fragmentation تعادل برقرار می‌کنید؟

### سؤال ۲۶ — چرا Function را می‌توان یک واحد مهم برای Decomposition یک Problem دانست؟

### سؤال ۲۷ — اگر دو Function کد مشابهی داشته باشند، آیا همیشه باید آن‌ها را به یک Function تبدیل کرد؟

### سؤال ۲۸ — در یک Code Review چگونه کیفیت طراحی Function را ارزیابی می‌کنید؟

---

# Golden Answers

## سؤال ۱ — Function چیست و چرا در JavaScript استفاده می‌شود؟

**Junior**

Function یک واحد مستقل از Logic است که می‌تواند برای انجام یک وظیفه مشخص چندین بار اجرا شود. هدف اصلی آن سازمان‌دهی Logic و جلوگیری از تکرار غیرضروری Code است.

---

## سؤال ۲ — Function Declaration چیست؟

**Junior**

Function Declaration روشی برای تعریف Function با استفاده از کلمه کلیدی `function` است؛ مانند:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

---

## سؤال ۳ — تفاوت Parameter و Argument چیست؟

**Junior**

Parameter نام ورودی در زمان تعریف Function است، در حالی که Argument مقدار واقعی‌ای است که هنگام Invocation به Function ارسال می‌شود.

```javascript
function greet(name) {} // name → Parameter

greet('Omid');          // 'Omid' → Argument
```

---

## سؤال ۴ — Function چگونه Invoke می‌شود؟

**Junior**

با نوشتن نام Function به همراه پرانتز آن را Invoke می‌کنیم:

```javascript
calculateTotal(120, 3);
```

Argumentها داخل پرانتز قرار می‌گیرند.

---

## سؤال ۵ — تفاوت Definition و Invocation چیست؟

**Junior**

Definition ساختار و Logic Function را ایجاد می‌کند، اما Invocation باعث اجرای آن Logic می‌شود.

```javascript
function calculateTotal() {} // Definition

calculateTotal();            // Invocation
```

---

## سؤال ۶ — `return` چه کاری انجام می‌دهد؟

**Junior**

`return` یک Value را از Function به محل فراخوانی برمی‌گرداند و اجرای Function را در همان نقطه پایان می‌دهد.

---

## سؤال ۷ — آیا Function می‌تواند بدون `return` باشد؟

**Junior**

بله. Function می‌تواند صرفاً یک Action انجام دهد و Value مشخصی را برنگرداند.

```javascript
function showMessage(message) {
  console.log(message);
}
```

---

## سؤال ۸ — تفاوت `return` و `console.log()` چیست؟

**Junior**

`console.log()` مقدار را برای مشاهده در Console نمایش می‌دهد، اما `return` مقدار را به Caller برمی‌گرداند تا بتوان از آن در ادامه برنامه استفاده کرد.

---

## سؤال ۹ — چرا Function باعث Reusability می‌شود؟

**Junior**

زیرا Logic یک‌بار در Function تعریف می‌شود و می‌توان آن را با Inputهای مختلف چندین بار Invoke کرد. در نتیجه نیازی به کپی کردن همان Logic در نقاط مختلف برنامه نیست.

---

## سؤال ۱۰ — Early Return چیست؟

**Junior**

Early Return یعنی Function در صورت برقرار بودن یک شرط، قبل از رسیدن به انتهای معمول Logic مقدار موردنظر را برگرداند و اجرا را پایان دهد. این الگو معمولاً برای کاهش پیچیدگی Conditional Logic استفاده می‌شود.

---

## سؤال ۱۱ — چرا Function باید مسئولیت مشخصی داشته باشد؟

**Mid-Level**

زیرا Function با مسئولیت مشخص ساده‌تر فهمیده، تست، Debug و تغییر داده می‌شود. ترکیب چند مسئولیت نامرتبط باعث افزایش پیچیدگی و کاهش Maintainability می‌شود.

---

## سؤال ۱۲ — Input، Processing و Output چیست؟

**Mid-Level**

Input داده‌ای است که Function دریافت می‌کند، Processing Logic داخلی Function است و Output نتیجه‌ای است که Function تولید و در صورت نیاز با `return` برمی‌گرداند.

```text
Input → Processing → Output
```

---

## سؤال ۱۳ — چرا Functionهای کوچک معمولاً قابل نگهداری‌تر هستند؟

**Mid-Level**

زیرا معمولاً یک Logic محدودتر دارند و درک رفتار آن‌ها ساده‌تر است. در نتیجه تغییر، Debug و استفاده مجدد از آن‌ها آسان‌تر می‌شود.

---

## سؤال ۱۴ — چرا نام Function اهمیت دارد؟

**Mid-Level**

نام Function باید Intent آن را منتقل کند. نامی مانند `calculateTotal` بدون خواندن Implementation نیز هدف Function را مشخص می‌کند، در حالی که نامی مانند `process` اطلاعات کمی ارائه می‌دهد.

---

## سؤال ۱۵ — Side Effect چیست؟

**Mid-Level**

Side Effect زمانی رخ می‌دهد که Function علاوه بر محاسبه یا تولید Output، تغییری خارج از محاسبه داخلی خود ایجاد کند؛ مانند تغییر یک وضعیت یا ارسال داده به یک محیط بیرونی.

---

## سؤال ۱۶ — آیا Function همیشه باید Value برگرداند؟

**Mid-Level**

خیر. برخی Functionها برای انجام یک Action طراحی می‌شوند و خروجی قابل استفاده‌ای تولید نمی‌کنند. مهم این است که رفتار Function با Responsibility آن سازگار باشد.

---

## سؤال ۱۷ — چرا `console.log()` جایگزین `return` نیست؟

**Mid-Level**

زیرا `console.log()` فقط مقدار را نمایش می‌دهد، اما `return` مقدار را در اختیار کدی قرار می‌دهد که Function را Invoke کرده است. بنابراین Result قابل استفاده مجدد نیست مگر اینکه با `return` برگردانده شود.

---

## سؤال ۱۸ — Early Return چگونه خوانایی را افزایش می‌دهد؟

**Mid-Level**

با پایان دادن زودهنگام مسیرهای نامعتبر یا استثنایی، از ایجاد Nesting غیرضروری جلوگیری می‌کند و مسیر اصلی Logic را واضح‌تر نشان می‌دهد.

---

## سؤال ۱۹ — آیا Function بزرگ لزوماً بد است؟

**Mid-Level**

خیر. اندازه به‌تنهایی معیار کیفیت نیست. مشکل زمانی ایجاد می‌شود که Function مسئولیت‌های متعدد و نامرتبط داشته باشد یا درک و تغییر آن دشوار شود.

---

## سؤال ۲۰ — چه زمانی دو Logic باید در دو Function جدا قرار گیرند؟

**Mid-Level**

وقتی دو بخش Logic مسئولیت‌های متفاوتی دارند، مستقل قابل استفاده هستند یا تغییر یکی نباید مستقیماً روی دیگری اثر بگذارد، جدا کردن آن‌ها معمولاً طراحی بهتری ایجاد می‌کند.

---

## سؤال ۲۱ — Reusability تنها دلیل استفاده از Function است؟

**Senior**

خیر. Function علاوه بر Reusability، برای **Decomposition، Separation of Responsibilities، Readability و Maintainability** استفاده می‌شود. بنابراین حتی Logicای که فقط یک‌بار استفاده می‌شود نیز ممکن است برای ساختار بهتر برنامه داخل یک Function قرار گیرد.

---

## سؤال ۲۲ — یک Function خوب چه ویژگی‌هایی دارد؟

**Senior**

یک Function خوب Responsibility مشخص، نام معنادار، Input و Output قابل فهم و رفتار قابل پیش‌بینی دارد. همچنین باید تا حد امکان از ترکیب Logicهای نامرتبط و Side Effectهای غیرضروری جلوگیری کند.

---

## سؤال ۲۳ — چرا چند Responsibility در یک Function مشکل‌ساز است؟

**Senior**

زیرا تغییر یک بخش از Function ممکن است بخش‌های دیگر را تحت تأثیر قرار دهد. این وابستگی داخلی باعث افزایش پیچیدگی، دشوارتر شدن Testing و کاهش Maintainability می‌شود.

---

## سؤال ۲۴ — Side Effect چه تأثیری بر Predictability دارد؟

**Senior**

هرچه Function علاوه بر Output خود تغییرات بیشتری در محیط ایجاد کند، تحلیل رفتار آن دشوارتر می‌شود. Functionی که رفتار آن عمدتاً از Input به Output محدود باشد، معمولاً Predictableتر است.

---

## سؤال ۲۵ — چگونه بین Small Function و Over-Fragmentation تعادل برقرار می‌کنید؟

**Senior**

هدف کوچک کردن Function به‌صورت مکانیکی نیست؛ هدف ایجاد واحدهای منطقی مستقل است. اگر تقسیم Function باعث شود خواننده برای فهم یک Logic ساده دائماً بین Functionهای متعدد جابه‌جا شود، احتمالاً بیش از حد Fragment شده است.

---

## سؤال ۲۶ — چرا Function برای Problem Decomposition مهم است؟

**Senior**

زیرا یک Problem بزرگ را می‌توان به چند مسئولیت کوچک‌تر تقسیم کرد و هر مسئولیت را در یک Function مستقل پیاده کرد. این کار Complexity را به واحدهای قابل مدیریت‌تر تبدیل می‌کند.

---

## سؤال ۲۷ — آیا کد مشابه همیشه باید به یک Function تبدیل شود؟

**Senior**

خیر. شباهت ظاهری کافی نیست. باید بررسی شود که آیا دو Logic واقعاً یک Responsibility و یک مفهوم مشترک دارند یا فقط در بخشی از Implementation مشابه‌اند. Abstraction نادرست می‌تواند Coupling و پیچیدگی بیشتری ایجاد کند.

---

## سؤال ۲۸ — در Code Review چگونه کیفیت Function را ارزیابی می‌کنید؟

**Senior**

ابتدا Responsibility و Intent آن را بررسی می‌کنم، سپس نام‌گذاری، Input و Output، پیچیدگی Logic، Side Effectها و میزان وابستگی آن به بخش‌های دیگر را ارزیابی می‌کنم. هدف این است که Function برای خواننده قابل فهم، قابل تغییر و دارای رفتار قابل پیش‌بینی باشد.

---

# Conclusion

Function یکی از بنیادی‌ترین ابزارهای طراحی در JavaScript است.

اما درک Function نباید به حفظ Syntax زیر محدود شود:

```javascript
function name(parameters) {
  // logic
  return result;
}
```

مدل ذهنی مهم‌تر این است:

```text
Function
   ↓
Defined Logic
   ↓
Invocation
   ↓
Input
   ↓
Processing
   ↓
Output
```

Function به ما کمک می‌کند Logic را از محل استفاده آن جدا کنیم، آن را دوباره استفاده کنیم و یک Application را به واحدهای کوچک‌تر و قابل فهم تقسیم کنیم.

در این فصل فقط با **Function Fundamentals** آشنا شدیم.

در فصل بعد، نگاه خود را یک مرحله عمیق‌تر می‌کنیم و بررسی خواهیم کرد که Function چگونه می‌تواند به‌عنوان یک **Value** مورد استفاده قرار گیرد و چه تفاوتی میان **Function Declaration، Function Expression و Arrow Function** وجود دارد.
