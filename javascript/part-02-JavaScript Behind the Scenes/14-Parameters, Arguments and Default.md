# Chapter 14 — Parameters, Arguments and Default Parameters

---

# اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* تفاوت **Parameter** و **Argument** را به‌صورت دقیق توضیح دهید.
* نحوه دریافت چند ورودی در یک Function را درک کنید.
* رفتار Function هنگام دریافت Arguments کمتر از Parameters را تحلیل کنید.
* مفهوم **Default Parameter** را درک و در کد واقعی استفاده کنید.
* نقش `undefined` را در Default Parameters توضیح دهید.
* تفاوت ارسال نشدن Argument و ارسال `undefined` را درک کنید.
* رفتار Function هنگام دریافت Arguments بیشتر از Parameters را بشناسید.
* ورودی‌های Function را با Defaults مناسب و قابل پیش‌بینی طراحی کنید.
* از Default Parameter برای بهبود خوانایی و قابلیت استفاده مجدد Function استفاده کنید.

---

# مقدمه

در فصل‌های قبل یاد گرفتیم که **Function** یکی از مهم‌ترین ابزارهای JavaScript برای سازمان‌دهی و استفاده مجدد از Logic است.

یک Function معمولاً برای انجام یک کار مشخص به داده نیاز دارد.

برای مثال، Function زیر مبلغ نهایی یک Order را محاسبه می‌کند:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

این Function بدون دریافت داده‌های مناسب نمی‌تواند نتیجه مورد انتظار را تولید کند.

پس یک سؤال مهم مطرح می‌شود:

> چگونه باید ورودی‌های Function را طراحی کنیم تا Function قابل استفاده، قابل پیش‌بینی و مقاوم در برابر ورودی‌های ناقص باشد؟

برای پاسخ به این سؤال باید ابتدا میان دو مفهوم بسیار نزدیک اما متفاوت تمایز قائل شویم:

**Parameters** و **Arguments**.

سپس بررسی می‌کنیم اگر یک Argument ارسال نشود چه اتفاقی رخ می‌دهد و چگونه می‌توان با **Default Parameters** رفتار Function را قابل اعتمادتر کرد.

---

# Block 01 — Parameters and Arguments

## Parameter چیست؟

### تعریف ساده

**Parameter** نامی است که در تعریف Function برای دریافت یک ورودی مشخص می‌کنیم.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

در این Function:

```javascript
price
quantity
```

Parameter هستند.

آن‌ها مشخص می‌کنند Function انتظار دارد چه ورودی‌هایی را دریافت کند.

---

### تعریف فنی

Parameter یک **Binding** در Function است که هنگام Invocation می‌تواند مقدار یک Argument را دریافت کند.

به بیان ساده‌تر، Parameter بخشی از تعریف Function است که برای دریافت ورودی در نظر گرفته می‌شود.

---

## چرا Parameter مهم است؟

Function بدون Parameter می‌تواند فقط روی داده‌های ثابت یا داده‌هایی که از Scope خود دریافت می‌کند کار کند.

اما Parameter باعث می‌شود یک Function بتواند با داده‌های مختلف دوباره استفاده شود.

برای مثال:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

اکنون همان Function می‌تواند برای Orderهای مختلف استفاده شود:

```javascript
calculateTotal(100, 2);
calculateTotal(250, 3);
calculateTotal(80, 5);
```

Function تغییر نکرده است.

تنها داده ورودی تغییر کرده است.

این همان چیزی است که **Reusability** را ممکن می‌کند.

---

## Argument چیست؟

### تعریف ساده

**Argument** مقداری است که هنگام فراخوانی Function به آن ارسال می‌کنیم.

برای مثال:

```javascript
calculateTotal(100, 2);
```

در اینجا:

```text
100
2
```

Argument هستند.

در حالی که:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

شامل Parameterهای:

```text
price
quantity
```

است.

---

### تعریف فنی

Argument یک مقدار یا Expression است که هنگام **Function Invocation** برای تأمین ورودی مورد انتظار Function ارائه می‌شود.

بنابراین:

```text
Parameter → در تعریف Function
Argument  → در فراخوانی Function
```

این تفاوت یکی از مهم‌ترین اصطلاحات پایه در کار با Functionها است.

---

## Parameter و Argument در کنار هم

به مثال زیر توجه کنید:

```javascript
function greetUser(name) {
  console.log(`Hello ${name}`);
}

greetUser('Omid');
```

در تعریف Function:

```javascript
name
```

یک Parameter است.

در زمان Invocation:

```javascript
'Omid'
```

یک Argument است.

می‌توان این رابطه را به شکل زیر تصور کرد:

```text
Function Definition
       ↓
   Parameter
       ↑
       |
    Argument
       |
Function Invocation
```

---

## تحلیل مهندسی

Parameter بخشی از **Interface** یک Function است.

وقتی می‌نویسیم:

```javascript
function createUser(name, role) {
  // ...
}
```

در واقع مشخص می‌کنیم Function برای انجام کار خود به دو ورودی نیاز دارد.

این موضوع در طراحی APIهای داخلی Application اهمیت زیادی دارد.

Function باید ورودی‌هایی داشته باشد که:

* برای انجام مسئولیت آن ضروری باشند.
* نام‌گذاری واضحی داشته باشند.
* رفتار قابل پیش‌بینی ایجاد کنند.

بنابراین طراحی Parameterها فقط یک موضوع Syntax نیست؛ بخشی از طراحی Interface Function است.

---

## Multiple Parameters

یک Function می‌تواند بیش از یک Parameter داشته باشد.

```javascript
function calculateDiscount(price, percentage) {
  return price * (1 - percentage / 100);
}
```

در اینجا:

```text
price
percentage
```

دو Parameter هستند.

هنگام Invocation می‌توان مقدار هر دو را ارسال کرد:

```javascript
calculateDiscount(1000, 20);
```

در این حالت:

```text
price      → 1000
percentage → 20
```

است.

---

## ترتیب Parameters اهمیت دارد

مقادیر Argumentها بر اساس موقعیت به Parameterها اختصاص داده می‌شوند.

```javascript
function createUser(name, role) {
  console.log(name);
  console.log(role);
}

createUser('Omid', 'admin');
```

نتیجه:

```text
Omid
admin
```

اما اگر ترتیب را تغییر دهیم:

```javascript
createUser('admin', 'Omid');
```

Function همچنان اجرا می‌شود، اما داده‌ها در Parameterهای اشتباه قرار می‌گیرند.

بنابراین ترتیب Parameters بخشی از قرارداد Function است.

---

## Common Mistakes

### اشتباه ۱: یکی دانستن Parameter و Argument

❌

> Parameter مقداری است که هنگام فراخوانی ارسال می‌شود.

✔

Parameter در تعریف Function قرار دارد و Argument هنگام Invocation ارسال می‌شود.

---

### اشتباه ۲: تصور اینکه Parameter باید حتماً مقدار داشته باشد

Parameter فقط مشخص می‌کند Function چگونه ورودی را دریافت کند.

ممکن است هنگام Invocation برای آن Argument ارسال نشود.

این حالت را در بخش Default Parameters بررسی خواهیم کرد.

---

### اشتباه ۳: نام‌گذاری نامفهوم Parameters

کد زیر:

```javascript
function calculate(a, b) {
  return a * b;
}
```

ممکن است اجرا شود، اما برای Functionی که قرار است در یک Application واقعی استفاده شود، اطلاعات کمی درباره نقش ورودی‌ها ارائه می‌دهد.

نسخه خواناتر:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

نام Parameter بخشی از مستندات Function است.

---

## نکات مهم

* Parameter در تعریف Function قرار دارد.
* Argument هنگام Invocation ارسال می‌شود.
* یک Function می‌تواند چند Parameter داشته باشد.
* Argumentها بر اساس ترتیب به Parameterها اختصاص داده می‌شوند.
* Parameterهای واضح، Interface Function را خواناتر می‌کنند.
* طراحی ورودی Function بخشی از API Design است.

---

# Block 02 — Default Parameters

## مشکل ورودی‌های ناقص

فرض کنید Function زیر برای ایجاد یک User استفاده می‌شود:

```javascript
function createUser(name, role) {
  return `${name} - ${role}`;
}
```

اگر هر دو Argument را ارسال کنیم:

```javascript
createUser('Omid', 'admin');
```

نتیجه مورد انتظار است:

```text
Omid - admin
```

اما اگر Role ارسال نشود:

```javascript
createUser('Omid');
```

چه اتفاقی می‌افتد؟

مقدار Parameter `role` برابر `undefined` خواهد بود.

در نتیجه:

```text
Omid - undefined
```

این رفتار ممکن است در برخی Functionها قابل قبول باشد، اما در بسیاری از APIها نتیجه مطلوبی نیست.

راه‌حل چیست؟

**Default Parameter**

---

## Default Parameter چیست؟

### تعریف ساده

Default Parameter مقداری است که برای یک Parameter تعیین می‌کنیم تا اگر Argument مربوط به آن ارسال نشد، Function از آن مقدار استفاده کند.

مثال:

```javascript
function createUser(name, role = 'user') {
  return `${name} - ${role}`;
}
```

اکنون:

```javascript
createUser('Omid');
```

نتیجه:

```text
Omid - user
```

است.

---

### تعریف فنی

Default Parameter مقداری است که هنگام ایجاد Binding مربوط به Parameter، در صورتی که Argument متناظر **`undefined`** باشد، به آن Parameter اختصاص داده می‌شود.

Syntax:

```javascript
function functionName(parameter = defaultValue) {
  // ...
}
```

---

## چرا Default Parameter مهم است؟

Default Parameter باعث می‌شود Function بتواند با ورودی‌های ناقص نیز رفتار مشخصی داشته باشد.

برای مثال:

```javascript
function loadProducts(limit = 10) {
  // ...
}
```

اگر Caller مقدار `limit` را ارسال نکند، Function همچنان یک رفتار مشخص دارد:

```text
limit → 10
```

این موضوع برای طراحی APIهای داخلی بسیار مفید است.

Function به جای اینکه با یک ورودی ناقص وارد وضعیت نامشخص شود، یک مقدار پیش‌فرض منطقی دارد.

---

## Syntax

ساختار کلی:

```javascript
function functionName(parameter = defaultValue) {
  // ...
}
```

مثال:

```javascript
function greetUser(name = 'Guest') {
  return `Hello ${name}`;
}
```

اگر Argument ارسال نشود:

```javascript
greetUser();
```

خروجی:

```text
Hello Guest
```

اگر Argument ارسال شود:

```javascript
greetUser('Omid');
```

خروجی:

```text
Hello Omid
```

Argument ارسال‌شده جایگزین Default Value می‌شود.

---

## Default Parameter و `undefined`

یکی از مهم‌ترین نکات این فصل رابطه میان Default Parameter و `undefined` است.

فرض کنید:

```javascript
function greetUser(name = 'Guest') {
  return `Hello ${name}`;
}
```

اگر Function را بدون Argument فراخوانی کنیم:

```javascript
greetUser();
```

پارامتر `name` مقدار پیش‌فرض را دریافت می‌کند:

```text
Guest
```

اما اگر صراحتاً `undefined` ارسال کنیم:

```javascript
greetUser(undefined);
```

باز هم:

```text
Guest
```

دریافت می‌کنیم.

یعنی برای Default Parameter، این دو حالت از نظر نتیجه یکسان هستند:

```javascript
greetUser();
```

و:

```javascript
greetUser(undefined);
```

---

## چرا `undefined` مهم است؟

زیرا Default Parameter بر اساس `undefined` فعال می‌شود، نه صرفاً بر اساس «نبودن مقدار» به معنای عمومی.

برای مثال:

```javascript
function setLimit(limit = 10) {
  return limit;
}
```

این Function با:

```javascript
setLimit();
```

مقدار:

```text
10
```

را برمی‌گرداند.

همچنین:

```javascript
setLimit(undefined);
```

نیز:

```text
10
```

برمی‌گرداند.

اما:

```javascript
setLimit(null);
```

نتیجه:

```text
null
```

خواهد بود.

زیرا `null` برابر `undefined` نیست.

---

## Default Value فقط برای `undefined` فعال می‌شود

مثال:

```javascript
function setLimit(limit = 10) {
  return limit;
}
```

نتایج:

```javascript
setLimit();          // 10
setLimit(undefined); // 10
setLimit(null);      // null
setLimit(0);         // 0
setLimit(false);     // false
setLimit('');        // ''
```

این رفتار بسیار مهم است.

Default Parameter بر اساس **Truthy / Falsy** تصمیم نمی‌گیرد.

بلکه بررسی می‌کند آیا مقدار Parameter برابر `undefined` است یا خیر.

---

## Default Value می‌تواند Expression باشد

Default Value الزاماً یک مقدار ثابت نیست.

می‌توان از یک Expression نیز استفاده کرد.

```javascript
function createOrder(quantity = 1 + 1) {
  return quantity;
}
```

اگر Argument ارسال نشود:

```javascript
createOrder();
```

نتیجه:

```text
2
```

زیرا Expression ابتدا ارزیابی می‌شود.

---

## استفاده از Parameter دیگر

یک Default Parameter می‌تواند به Parameter قبلی نیز وابسته باشد.

```javascript
function createProduct(name, quantity = 1) {
  return {
    name,
    quantity
  };
}
```

در اینجا `quantity` مقدار پیش‌فرض دارد و Function می‌تواند بدون دریافت Quantity نیز کار کند.

این الگو در طراحی APIهای کوچک بسیار رایج است.

---

## Default Parameters و خوانایی

بدون Default Parameter ممکن است Logic مربوط به مقدار پیش‌فرض را داخل بدنه Function قرار دهیم:

```javascript
function loadProducts(limit) {
  if (limit === undefined) {
    limit = 10;
  }

  // ...
}
```

با Default Parameter:

```javascript
function loadProducts(limit = 10) {
  // ...
}
```

نسخه دوم واضح‌تر است.

خود Signature Function مشخص می‌کند که مقدار پیش‌فرض `limit` برابر `10` است.

این موضوع باعث می‌شود Interface Function سریع‌تر قابل درک باشد.

---

## Common Mistakes

### اشتباه ۱: تصور اینکه Default Parameter برای هر مقدار Falsy فعال می‌شود

❌

```javascript
function setLimit(limit = 10) {
  return limit;
}

setLimit(0);
```

نباید انتظار داشته باشیم `10` برگردد.

نتیجه:

```text
0
```

است.

---

### اشتباه ۲: یکی دانستن `null` و `undefined`

❌

```javascript
setLimit(null);
```

باعث فعال شدن Default Parameter نمی‌شود.

✔ Default Parameter زمانی فعال می‌شود که مقدار Parameter برابر `undefined` باشد.

---

### اشتباه ۳: استفاده از Default Value نامناسب

Default Value باید از نظر Domain منطقی باشد.

مثلاً:

```javascript
function createUser(role = 'admin') {
  // ...
}
```

اگر بیشتر کاربران معمولی هستند، این Default ممکن است یک طراحی خطرناک باشد.

Default Value باید رفتار مورد انتظار Application را نشان دهد.

---

## نکات مهم

* Default Parameter با `=` تعریف می‌شود.
* اگر Argument مربوطه `undefined` باشد، Default Value استفاده می‌شود.
* `null` باعث فعال شدن Default Parameter نمی‌شود.
* `0`، `false` و `''` نیز Default را فعال نمی‌کنند.
* Default Value می‌تواند یک Expression باشد.
* Default Parameters می‌توانند خوانایی Interface Function را افزایش دهند.

---

# Block 03 — Parameter Patterns

## Missing Arguments

JavaScript الزام نمی‌کند که Caller دقیقاً به تعداد Parameters موجود Argument ارسال کند.

برای مثال:

```javascript
function createUser(name, role) {
  return `${name} - ${role}`;
}

createUser('Omid');
```

Function اجرا می‌شود.

اما چون Argument دوم ارسال نشده است:

```text
role → undefined
```

خواهد بود.

بنابراین:

```text
تعداد Parameters
```

و:

```text
تعداد Arguments
```

لزومی ندارد همیشه برابر باشد.

---

## اگر Arguments کمتر باشند چه می‌شود؟

فرض کنید:

```javascript
function createOrder(product, quantity, discount) {
  // ...
}
```

و فقط دو Argument ارسال کنیم:

```javascript
createOrder('Laptop', 2);
```

در این حالت:

```text
product  → 'Laptop'
quantity → 2
discount → undefined
```

است.

اگر Parameter سوم Default داشته باشد:

```javascript
function createOrder(
  product,
  quantity,
  discount = 0
) {
  // ...
}
```

آنگاه:

```text
discount → 0
```

خواهد بود.

این یکی از کاربردهای اصلی Default Parameters است.

---

## Extra Arguments

حال حالت برعکس را بررسی کنیم.

اگر Function دو Parameter داشته باشد:

```javascript
function createUser(name, role) {
  return `${name} - ${role}`;
}
```

اما سه Argument ارسال کنیم:

```javascript
createUser('Omid', 'admin', 'extra');
```

Function به‌طور معمول به‌دلیل وجود Argument اضافه خطا نمی‌دهد.

دو Parameter اول مقدار خود را دریافت می‌کنند و Argument اضافی در این Signature مورد استفاده قرار نمی‌گیرد.

به بیان ساده:

```text
name → 'Omid'
role → 'admin'
extra → بدون Parameter متناظر
```

---

## چرا این رفتار مهم است؟

این رفتار نشان می‌دهد که JavaScript برای Invocation الزام نمی‌کند تعداد Arguments دقیقاً با تعداد Parameters برابر باشد.

بنابراین هنگام طراحی Function باید بدانیم:

* چه ورودی‌هایی ضروری هستند؟
* چه ورودی‌هایی اختیاری هستند؟
* برای ورودی اختیاری چه Default Value منطقی است؟
* اگر ورودی ارسال نشد چه اتفاقی باید رخ دهد؟

این پرسش‌ها بخشی از طراحی API Function هستند.

---

## Missing Argument و Default Parameter

ترکیب این دو مفهوم بسیار کاربردی است.

```javascript
function fetchProducts(page = 1, limit = 20) {
  return `Page: ${page}, Limit: ${limit}`;
}
```

اکنون می‌توان Function را با ورودی‌های مختلف استفاده کرد:

```javascript
fetchProducts();
```

نتیجه:

```text
Page: 1, Limit: 20
```

یا:

```javascript
fetchProducts(2);
```

نتیجه:

```text
Page: 2, Limit: 20
```

یا:

```javascript
fetchProducts(2, 50);
```

نتیجه:

```text
Page: 2, Limit: 50
```

این الگو برای Functionهایی که ورودی‌های اختیاری دارند بسیار مناسب است.

---

## ترتیب Parameters با Default

در طراحی Function معمولاً بهتر است Parameterهای ضروری قبل از Parameterهای دارای Default قرار بگیرند.

برای مثال:

```javascript
function createUser(name, role = 'user') {
  // ...
}
```

این ساختار طبیعی است.

اما اگر یک Parameter دارای Default قبل از Parameter ضروری قرار گیرد:

```javascript
function createUser(role = 'user', name) {
  // ...
}
```

Caller برای ارسال `name` باید موقعیت اول را نیز مدیریت کند.

در چنین شرایطی API Function می‌تواند کمتر خوانا شود.

بنابراین در طراحی Signature باید ترتیب Parameters را نیز در نظر گرفت.

---

## Function Design

یک Function خوب باید Interface مشخصی داشته باشد.

برای مثال:

```javascript
function calculateShipping(total, freeShippingLimit = 100) {
  // ...
}
```

این Signature اطلاعات مفیدی در اختیار برنامه‌نویس قرار می‌دهد:

* `total` ورودی اصلی است.
* `freeShippingLimit` قابل تنظیم است.
* مقدار پیش‌فرض آن `100` است.

در نتیجه بخشی از رفتار Function بدون مطالعه بدنه آن قابل فهم است.

---

## Common Mistakes

### اشتباه ۱: فرض اینکه تعداد Arguments باید دقیقاً برابر Parameters باشد

در JavaScript چنین الزامی وجود ندارد.

Function می‌تواند Arguments کمتر یا بیشتر دریافت کند.

---

### اشتباه ۲: استفاده از Default برای جایگزینی تمام Validationها

Default Parameter برای تعیین مقدار پیش‌فرض مناسب است.

اما به‌تنهایی تضمین نمی‌کند که ورودی معتبر است.

برای مثال:

```javascript
function setPage(page = 1) {
  // ...
}
```

این Function همچنان می‌تواند مقدار نامناسبی مانند:

```javascript
setPage(-10);
```

دریافت کند.

Validation موضوع متفاوتی است و نباید با Default Parameter اشتباه گرفته شود.

---

### اشتباه ۳: Default Value غیرمنطقی

اگر Default Value با نیاز Domain سازگار نباشد، Function به‌جای مقاوم‌تر شدن می‌تواند رفتار اشتباه ایجاد کند.

بنابراین:

> Default باید یک رفتار معتبر و مورد انتظار باشد، نه صرفاً یک مقدار دلخواه.

---

## نکات مهم

* تعداد Arguments می‌تواند کمتر از Parameters باشد.
* Parameter بدون Argument معمولاً مقدار `undefined` دریافت می‌کند.
* Default Parameter می‌تواند این وضعیت را مدیریت کند.
* ارسال Arguments بیشتر از Parameters معمولاً باعث خطا نمی‌شود.
* ترتیب Parameters بخشی از طراحی Interface Function است.
* Default Value با Validation یکسان نیست.

---

# Block 04 — Practical API Design

## Function به‌عنوان یک API کوچک

یک Function را می‌توان مانند یک API کوچک در نظر گرفت.

Caller تنها باید بداند:

* چه ورودی‌هایی لازم است؟
* چه ورودی‌هایی اختیاری هستند؟
* اگر ورودی اختیاری ارسال نشود چه اتفاقی می‌افتد؟
* Function چه خروجی‌ای تولید می‌کند؟

برای مثال:

```javascript
function calculateShipping(
  orderTotal,
  freeShippingLimit = 100
) {
  return orderTotal >= freeShippingLimit ? 0 : 10;
}
```

در اینجا Signature Function بخشی از قرارداد آن است.

---

## Defensive Defaults

گاهی Function برای کار کردن به مقدار پیش‌فرض مناسبی نیاز دارد.

برای مثال:

```javascript
function paginate(page = 1, limit = 20) {
  // ...
}
```

اگر Caller هیچ مقداری ارسال نکند، Function همچنان رفتار مشخصی دارد.

این نوع Default را می‌توان **Defensive Default** در نظر گرفت؛ یعنی طراحی Function به‌گونه‌ای که در برابر نبودن ورودی اختیاری، رفتار منطقی خود را حفظ کند.

البته Default نباید جای Validation را بگیرد.

---

## Readability

یکی از مهم‌ترین مزایای Default Parameters، افزایش خوانایی Signature Function است.

روش قدیمی:

```javascript
function searchProducts(query, limit) {
  if (limit === undefined) {
    limit = 20;
  }

  // ...
}
```

روش مستقیم‌تر:

```javascript
function searchProducts(query, limit = 20) {
  // ...
}
```

در نسخه دوم، قرارداد Function از همان ابتدا مشخص است.

خواننده بدون بررسی Body متوجه می‌شود:

```text
limit → optional
default → 20
```

---

## Practical Example

فرض کنید در یک Application برای نمایش محصولات Function زیر را داریم:

```javascript
function getProducts(page = 1, limit = 20) {
  console.log(`Page: ${page}, Limit: ${limit}`);
}
```

اکنون سه حالت داریم.

### بدون Argument

```javascript
getProducts();
```

نتیجه:

```text
Page: 1, Limit: 20
```

### فقط Page

```javascript
getProducts(2);
```

نتیجه:

```text
Page: 2, Limit: 20
```

### هر دو مقدار

```javascript
getProducts(2, 50);
```

نتیجه:

```text
Page: 2, Limit: 50
```

این Function یک Interface ساده و قابل پیش‌بینی دارد.

---

## چه زمانی Default Parameter انتخاب مناسبی است؟

Default Parameter زمانی مناسب است که:

* یک ورودی واقعاً اختیاری باشد.
* مقدار پیش‌فرض مشخص و معناداری وجود داشته باشد.
* نبودن ورودی نباید باعث شکست Function شود.
* مقدار پیش‌فرض بخشی از رفتار طبیعی Function باشد.

مثال مناسب:

```javascript
function loadProducts(limit = 20) {
  // ...
}
```

---

## چه زمانی Default Parameter مناسب نیست؟

اگر یک مقدار برای Function ضروری باشد، بهتر است صرفاً برای جلوگیری از خطا Default مصنوعی تعیین نکنیم.

مثلاً:

```javascript
function createAccount(email = 'unknown') {
  // ...
}
```

اگر `email` برای ساخت Account ضروری است، قرار دادن یک Default ساختگی ممکن است خطای واقعی را پنهان کند.

در چنین شرایطی بهتر است Function نبودن ورودی ضروری را به‌عنوان یک مسئله مستقل مدیریت کند.

---

## طراحی حرفه‌ای Function

یک Function خوب باید مشخص کند:

```text
Required Input
      +
Optional Input
      +
Default Behavior
      ↓
Predictable Function
```

برای مثال:

```javascript
function createOrder(productId, quantity = 1) {
  // ...
}
```

در اینجا:

```text
productId → Required
quantity  → Optional
default   → 1
```

این طراحی ساده‌تر از Functionی است که Caller مجبور باشد همیشه Quantity را ارسال کند.

---

## Common Mistakes

### استفاده از Default برای پنهان کردن خطا

❌

```javascript
function createOrder(productId = 0) {
  // ...
}
```

اگر `productId` واقعاً ضروری است، این Default ممکن است خطای Caller را پنهان کند.

---

### Defaultهای بیش از حد

Function زیر:

```javascript
function createOrder(
  productId = 0,
  quantity = 1,
  discount = 0,
  currency = 'USD',
  shipping = 10
) {
  // ...
}
```

ممکن است بیش از حد مسئولیت و Configuration را وارد Signature کند.

Default Parameter ابزار مفیدی است، اما تعداد زیاد Parameters می‌تواند نشانه‌ای باشد که Interface Function نیاز به طراحی مجدد دارد.

در چنین شرایطی در فصل‌های آینده با الگوهای دیگری برای سازمان‌دهی داده‌ها آشنا خواهیم شد.

---

### تصور اینکه Default Parameter ورودی را Validate می‌کند

این کد:

```javascript
function setQuantity(quantity = 1) {
  // ...
}
```

به این معنا نیست که Quantity همیشه معتبر است.

مثلاً:

```javascript
setQuantity(-5);
```

هنوز مقدار `-5` را دریافت می‌کند.

Default فقط زمانی استفاده می‌شود که مقدار `undefined` باشد.

---

## نکات مهم

* Function باید Interface مشخص و قابل پیش‌بینی داشته باشد.
* Default Parameters برای ورودی‌های واقعاً اختیاری مناسب هستند.
* Default Value باید از نظر Domain منطقی باشد.
* Default Parameter جایگزین Validation نیست.
* Defaultهای زیاد می‌توانند نشانه Interface پیچیده باشند.
* Signature خوانا بخشی از کیفیت طراحی Function است.

---

# خلاصه فصل

در این فصل بررسی کردیم که Function چگونه ورودی‌های خود را دریافت و مدیریت می‌کند.

ابتدا میان **Parameter** و **Argument** تمایز ایجاد کردیم.

Parameter در تعریف Function قرار دارد:

```javascript
function calculateTotal(price, quantity) {
  // ...
}
```

در حالی که Argument هنگام Invocation ارسال می‌شود:

```javascript
calculateTotal(100, 2);
```

سپس دیدیم که تعداد Arguments الزاماً نباید با تعداد Parameters برابر باشد.

اگر Argument مربوط به یک Parameter ارسال نشود، مقدار آن Parameter می‌تواند `undefined` باشد.

برای مدیریت این وضعیت از **Default Parameter** استفاده می‌کنیم:

```javascript
function calculateTotal(price, quantity = 1) {
  // ...
}
```

یکی از نکات مهم این بود که Default Parameter فقط زمانی فعال می‌شود که مقدار Parameter برابر `undefined` باشد.

بنابراین:

```javascript
function test(value = 10) {
  return value;
}
```

رفتار زیر را دارد:

```javascript
test();           // 10
test(undefined);  // 10
test(null);       // null
test(0);          // 0
```

در نهایت دیدیم که Default Parameters تنها یک قابلیت Syntax نیستند.

آن‌ها بخشی از **Function API Design** هستند و می‌توانند Interface Function را خواناتر، قابل پیش‌بینی‌تر و مناسب‌تر برای استفاده مجدد کنند.

---

# Key Takeaways

* **Parameter** در تعریف Function قرار دارد.
* **Argument** هنگام Invocation ارسال می‌شود.
* Arguments بر اساس ترتیب به Parameters اختصاص داده می‌شوند.
* تعداد Arguments می‌تواند کمتر یا بیشتر از تعداد Parameters باشد.
* Parameter بدون Argument معمولاً مقدار `undefined` دریافت می‌کند.
* Default Parameter با Syntax زیر تعریف می‌شود:

```javascript
function example(value = defaultValue) {
  // ...
}
```

* Default Parameter زمانی استفاده می‌شود که مقدار Parameter `undefined` باشد.
* `null` باعث فعال شدن Default Parameter نمی‌شود.
* `0`، `false` و `''` نیز باعث فعال شدن Default نمی‌شوند.
* Default Value می‌تواند یک Expression باشد.
* Default Parameter برای ورودی‌های اختیاری مناسب است.
* Default Parameter جایگزین Validation نیست.
* طراحی Parameterها بخشی از API Design Function است.
* Defaultهای مناسب می‌توانند Function را قابل پیش‌بینی‌تر کنند.
* Defaultهای بیش از حد می‌توانند Interface Function را پیچیده کنند.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Parameter و Argument چه تفاوتی دارند؟

### پاسخ

Parameter در تعریف Function قرار دارد و مشخص می‌کند Function چه ورودی‌ای دریافت می‌کند. Argument مقداری است که هنگام فراخوانی Function به آن ارسال می‌شود.

---

### سؤال ۲

Default Parameter چیست؟

### پاسخ

Default Parameter مقداری پیش‌فرض برای یک Parameter است که زمانی استفاده می‌شود که Argument متناظر `undefined` باشد.

---

### سؤال ۳

در مثال زیر `name` چه چیزی است و `'Omid'` چه چیزی؟

```javascript
function greet(name) {
  return `Hello ${name}`;
}

greet('Omid');
```

### پاسخ

`name` یک Parameter و `'Omid'` یک Argument است.

---

### سؤال ۴

اگر Function سه Parameter داشته باشد اما فقط دو Argument دریافت کند چه اتفاقی می‌افتد؟

### پاسخ

Parameter سوم مقدار `undefined` دریافت می‌کند، مگر اینکه برای آن Default Parameter تعریف شده باشد.

---

### سؤال ۵

آیا تعداد Arguments باید دقیقاً با تعداد Parameters برابر باشد؟

### پاسخ

خیر. JavaScript اجازه می‌دهد Function با Arguments کمتر یا بیشتر از تعداد Parameters فراخوانی شود.

---

### سؤال ۶

چه چیزی باعث فعال شدن Default Parameter می‌شود؟

### پاسخ

زمانی که مقدار Parameter برابر `undefined` باشد، Default Value استفاده می‌شود.

---

## سطح Mid-Level

### سؤال ۷

تفاوت رفتار `undefined` و `null` در Default Parameters چیست؟

### پاسخ

`undefined` باعث فعال شدن Default Parameter می‌شود، اما `null` یک مقدار واقعی است و Default را فعال نمی‌کند.

---

### سؤال ۸

خروجی کد زیر چیست؟

```javascript
function test(value = 10) {
  return value;
}

console.log(test(0));
```

### پاسخ

خروجی `0` است، زیرا Default Parameter فقط برای `undefined` فعال می‌شود و `0` یک مقدار معتبر است.

---

### سؤال ۹

خروجی کد زیر چیست؟

```javascript
function test(value = 10) {
  return value;
}

console.log(test(null));
```

### پاسخ

خروجی `null` است، زیرا `null` برابر `undefined` نیست و Default Parameter را فعال نمی‌کند.

---

### سؤال ۱۰

چرا Default Parameters برای API Design مفید هستند؟

### پاسخ

زیرا اجازه می‌دهند ورودی‌های اختیاری رفتار پیش‌فرض مشخصی داشته باشند. در نتیجه Interface Function خواناتر و رفتار آن در برابر ورودی‌های ناقص قابل پیش‌بینی‌تر می‌شود.

---

### سؤال ۱۱

آیا Default Parameter جایگزین Validation است؟

### پاسخ

خیر. Default Parameter فقط نبودن مقدار یا `undefined` را مدیریت می‌کند و معتبر بودن مقداری که Caller ارسال کرده است را بررسی نمی‌کند.

---

### سؤال ۱۲

اگر Function دو Parameter داشته باشد و سه Argument دریافت کند چه اتفاقی می‌افتد؟

### پاسخ

دو Argument اول به دو Parameter اختصاص داده می‌شوند و Argument اضافی در Signature معمول Function مورد استفاده قرار نمی‌گیرد.

---

### سؤال ۱۳

چرا بهتر است Parameterهای ضروری معمولاً قبل از Parameterهای دارای Default قرار بگیرند؟

### پاسخ

زیرا این ترتیب Signature را خواناتر می‌کند و باعث می‌شود Caller بتواند ورودی‌های اصلی را بدون ایجاد الگوی پیچیده برای رد کردن Parameterهای اختیاری ارسال کند.

---

## سطح Senior

### سؤال ۱۴

Default Parameter چگونه به طراحی یک Function API بهتر کمک می‌کند؟

### پاسخ

Default Parameter بخشی از قرارداد Function را در Signature مشخص می‌کند و رفتار ورودی‌های اختیاری را به‌صورت مستقیم بیان می‌کند. این کار وابستگی به Logic اضافی داخل Body را کاهش می‌دهد و Function را قابل پیش‌بینی‌تر می‌کند.

---

### سؤال ۱۵

چرا این دو Invocation از نظر Default Parameter رفتار یکسانی دارند؟

```javascript
getProducts();
```

و:

```javascript
getProducts(undefined);
```

### پاسخ

زیرا در هر دو حالت Parameter مربوطه مقدار `undefined` دارد. Default Parameter هنگام `undefined` بودن مقدار Parameter فعال می‌شود.

---

### سؤال ۱۶

چرا این کد Default را فعال نمی‌کند؟

```javascript
function setLimit(limit = 20) {
  return limit;
}

setLimit(0);
```

### پاسخ

زیرا Default Parameter بر اساس Truthy یا Falsy بودن مقدار تصمیم نمی‌گیرد. شرط فعال شدن Default این است که مقدار Parameter `undefined` باشد؛ `0` یک مقدار واقعی است.

---

### سؤال ۱۷

چه زمانی استفاده از Default Parameter می‌تواند طراحی Function را بدتر کند؟

### پاسخ

زمانی که برای یک ورودی ضروری Default مصنوعی تعیین کنیم و در نتیجه خطای Caller را پنهان کنیم، یا تعداد زیادی Parameter اختیاری ایجاد کنیم که Interface Function را پیچیده و دشوار برای استفاده کنند.

---

### سؤال ۱۸

آیا Default Parameter تضمین می‌کند Function همیشه ورودی معتبر دریافت می‌کند؟

### پاسخ

خیر. Default Parameter فقط نبودن مقدار یا `undefined` را پوشش می‌دهد. اعتبارسنجی مقدار ارسال‌شده مسئله‌ای جداگانه از Default Value است.

---

### سؤال ۱۹

چرا طراحی Parameterها را می‌توان بخشی از API Design دانست؟

### پاسخ

زیرا Parameterها مشخص می‌کنند Function چه داده‌ای نیاز دارد، کدام ورودی اختیاری است و در نبود آن چه رفتار پیش‌فرضی دارد. بنابراین Signature Function بخشی از قرارداد استفاده از آن است.

---

### سؤال ۲۰

اگر Function تعداد زیادی Parameter اختیاری با Default داشته باشد، چه مسئله‌ای ممکن است ایجاد شود؟

### پاسخ

Signature Function می‌تواند بیش از حد پیچیده شود و استفاده از آن دشوار شود. این وضعیت ممکن است نشان دهد که مسئولیت Function یا روش انتقال Configuration نیاز به بازطراحی دارد.

---

# Golden Answers

## Parameter و Argument چه تفاوتی دارند؟

**Parameter** نامی است که در تعریف Function برای دریافت ورودی مشخص می‌شود، در حالی که **Argument** مقداری است که هنگام Invocation به Function ارسال می‌شود.

---

## Default Parameter چیست؟

Default Parameter مقداری است که در صورت `undefined` بودن Argument متناظر، به Parameter اختصاص داده می‌شود.

---

## آیا Default Parameter برای تمام مقادیر Falsy فعال می‌شود؟

خیر. Default Parameter فقط در صورت `undefined` بودن مقدار فعال می‌شود؛ بنابراین `0`، `false`، `''` و `null` باعث استفاده از Default نمی‌شوند.

---

## آیا تعداد Arguments باید با Parameters برابر باشد؟

خیر. JavaScript اجازه می‌دهد Function با Arguments کمتر یا بیشتر از Parameters فراخوانی شود.

---

## آیا Default Parameter Validation انجام می‌دهد؟

خیر. Default Parameter فقط مقدار پیش‌فرض تعیین می‌کند و تضمین نمی‌کند مقدار دریافت‌شده معتبر باشد.

---

## چرا Default Parameters برای API Design مهم هستند؟

زیرا رفتار ورودی‌های اختیاری را مستقیماً در Signature Function مشخص می‌کنند و باعث می‌شوند Function خواناتر و قابل پیش‌بینی‌تر باشد.

---

## پاسخ کوتاه طلایی مصاحبه

**سؤال:**
Parameter و Argument چه تفاوتی دارند و Default Parameter چه مشکلی را حل می‌کند؟

**پاسخ:**
Parameter در تعریف Function قرار دارد، در حالی که Argument هنگام Invocation ارسال می‌شود. Default Parameter زمانی که Argument مربوطه `undefined` باشد یک مقدار پیش‌فرض فراهم می‌کند و به Function اجازه می‌دهد با ورودی اختیاری رفتار قابل پیش‌بینی داشته باشد.

---

# جمع‌بندی فصل

طراحی ورودی Function فقط به نوشتن چند نام داخل پرانتز محدود نمی‌شود.

یک Function خوب باید مشخص کند:

* چه داده‌ای لازم دارد.
* چه داده‌ای اختیاری است.
* اگر داده اختیاری ارسال نشد چه اتفاقی رخ می‌دهد.
* و چگونه باید رفتار خود را در برابر ورودی‌های ناقص حفظ کند.

در این فصل یاد گرفتیم که **Parameter** بخشی از تعریف Function و **Argument** مقدار ارسالی هنگام Invocation است.

سپس با **Default Parameters** آشنا شدیم و دیدیم که چگونه می‌توان برای ورودی‌های اختیاری رفتار پیش‌فرض تعریف کرد.

مهم‌ترین مدل ذهنی این فصل این است:

```text
Function API
     ↓
Parameters
     ↓
Arguments
     ↓
Missing Input
     ↓
undefined
     ↓
Default Parameter
     ↓
Predictable Behavior
```

در نتیجه، Default Parameter را نباید فقط یک Syntax کوتاه‌تر در نظر گرفت.

این قابلیت ابزاری برای **طراحی بهتر Interface Function** است.

در فصل بعد، نگاه خود را یک مرحله گسترده‌تر خواهیم کرد و بررسی می‌کنیم که چرا Function در JavaScript می‌تواند مانند یک **Value** مورد استفاده قرار گیرد و چگونه این ویژگی زمینه شکل‌گیری First-Class Functions و Higher-Order Functions را فراهم می‌کند.
