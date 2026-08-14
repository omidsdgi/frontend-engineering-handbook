# Chapter 09 — Type Conversion and Coercion

---

# Chapter Goal

پس از مطالعه این فصل انتظار می‌رود بتوانید:

* مفهوم **Type Conversion** را به‌عنوان تغییر Type یک Value توضیح دهید.
* تفاوت **Explicit Conversion** و **Implicit Coercion** را درک کنید.
* Valueها را به‌صورت صریح به `String`، `Number` و `Boolean` تبدیل کنید.
* رفتار JavaScript هنگام تبدیل ضمنی Valueها را تحلیل کنید.
* تفاوت **Conversion** و **Coercion** را از نظر مهندسی توضیح دهید.
* رفتار عملگر `+` را هنگام برخورد با String و Number تحلیل کنید.
* تفاوت `==` و `===` را در ارتباط با Type Conversion توضیح دهید.
* از قواعد رایج Coercion در کدهای واقعی آگاه باشید.
* از تبدیل‌های ضمنی غیرضروری که خوانایی کد را کاهش می‌دهند، جلوگیری کنید.
* به پرسش‌های فنی مرتبط با Type Conversion و Type Coercion پاسخ دهید.

---

# Core Question

> **JavaScript چه زمانی و چگونه Values را بین Types تبدیل می‌کند؟**

---

# Concept Flow

```text
Value
↓
Type Conversion
↓
Explicit Conversion
↓
Implicit Coercion
↓
To String
↓
To Number
↓
To Boolean
↓
Equality
↓
Common Coercion Rules
↓
Best Practices
```

---

# مقدمه

در فصل سوم یاد گرفتیم که هر **Value** در JavaScript دارای یک **Type** است.

برای مثال:

```javascript
const userName = 'Omid';
const age = 30;
const isActive = true;
```

در این مثال سه Value از سه Type متفاوت داریم:

```text
'Omid'  → String
30      → Number
true    → Boolean
```

اما در برنامه‌های واقعی، Valueها همیشه در همان Type اولیه خود باقی نمی‌مانند.

فرض کنید مقدار یک سن از یک فرم دریافت شده است:

```javascript
const age = '30';
```

از دید انسان، مقدار `30` است.

اما از دید JavaScript، این Value یک **String** است، نه Number.

اگر بخواهیم با آن محاسبه انجام دهیم، ممکن است لازم باشد آن را به Number تبدیل کنیم.

اینجاست که مفهوم **Type Conversion** اهمیت پیدا می‌کند.

از طرف دیگر، JavaScript در برخی عملیات‌ها خودش تصمیم می‌گیرد Type یک Value را تغییر دهد.

برای مثال:

```javascript
'30' + 5
```

نتیجه:

```text
305
```

در اینجا ما صریحاً نگفتیم که `5` به String تبدیل شود.

JavaScript در جریان ارزیابی Expression، تبدیل لازم را انجام داده است.

این رفتار **Type Coercion** نام دارد.

بنابراین در این فصل دو سؤال اساسی را بررسی می‌کنیم:

1. چگونه خودمان یک Value را به Type دیگری تبدیل کنیم؟
2. JavaScript چه زمانی و چرا یک Value را به‌صورت ضمنی تبدیل می‌کند؟

---

# 1. Type Conversion چیست؟

## تعریف ساده

**Type Conversion** یعنی تبدیل یک Value از یک Type به Type دیگر.

برای مثال:

```javascript
const age = '30';

const numericAge = Number(age);
```

در اینجا Value اولیه:

```text
'30'
```

از نوع `String` است.

پس از تبدیل:

```text
30
```

از نوع `Number` خواهد بود.

---

## تعریف فنی

Type Conversion فرایندی است که طی آن یک Value به Valueای با Type دیگر تبدیل می‌شود.

این تبدیل می‌تواند:

* به‌صورت **صریح (Explicit)** توسط برنامه‌نویس انجام شود.
* یا در برخی عملیات‌ها به‌صورت **ضمنی (Implicit)** توسط JavaScript انجام شود.

در حالت دوم معمولاً از اصطلاح **Type Coercion** استفاده می‌کنیم.

---

## چرا این مفهوم مهم است؟

داده‌هایی که از منابع مختلف وارد برنامه می‌شوند، الزاماً Type مورد انتظار ما را ندارند.

برای مثال، داده‌های یک فرم، URL یا برخی منابع خارجی ممکن است به‌صورت String در اختیار برنامه قرار بگیرند.

اگر Type واقعی Value را نادیده بگیریم، ممکن است عملیاتی انجام دهیم که نتیجه آن چیزی غیر از انتظار ما باشد.

برای مثال:

```javascript
const quantity = '2';
const price = 10;

const total = quantity + price;
```

نتیجه:

```text
210
```

است.

اگر هدف ما محاسبه قیمت باشد، این نتیجه اشتباه است.

---

## مثال

```javascript
const quantity = '2';

const numericQuantity = Number(quantity);

const total = numericQuantity * 10;

console.log(total);
```

خروجی:

```text
20
```

---

## تحلیل مهندسی

نکته مهم این است که **Type یک Value را باید قبل از استفاده در یک عملیات در نظر بگیریم**.

در این مثال:

```javascript
const quantity = '2';
```

نام متغیر ممکن است باعث شود تصور کنیم `quantity` یک Number است.

اما نام متغیر Type آن را تعیین نمی‌کند.

Value واقعی یک String است.

بنابراین Conversion به Number باید پیش از محاسبه انجام شود.

---

## اشتباهات رایج

❌ تصور اینکه وجود عدد درون یک String به این معناست که Value از نوع Number است.

```javascript
'30'
```

✔ این Value یک String است.

---

❌ اعتماد به نام متغیر برای تشخیص Type.

```javascript
const age = '30';
```

✔ Type را باید از Value و رفتار زبان تشخیص داد.

---

## نکات مهم

* Conversion یعنی تغییر Type یک Value.
* Conversion می‌تواند Explicit یا Implicit باشد.
* نام متغیر Type آن را تعیین نمی‌کند.
* Type صحیح برای انجام عملیات صحیح اهمیت دارد.

---

## پاسخ کوتاه طلایی مصاحبه

**Type Conversion چیست؟**

Type Conversion فرایند تبدیل یک Value از یک Type به Type دیگر است. این تبدیل می‌تواند به‌صورت صریح توسط برنامه‌نویس یا به‌صورت ضمنی در جریان اجرای یک عملیات انجام شود.

---

# 2. Explicit Conversion

## تعریف ساده

وقتی خود برنامه‌نویس به‌صورت مشخص درخواست تبدیل یک Value را می‌دهد، با **Explicit Conversion** روبه‌رو هستیم.

برای مثال:

```javascript
const age = '30';

const numericAge = Number(age);
```

در اینجا تصمیم تبدیل توسط ما گرفته شده است.

---

## تعریف فنی

Explicit Conversion تبدیلی است که برنامه‌نویس با استفاده از یک API، Constructor یا عملیات مشخص، Type مورد نظر Value را تعیین می‌کند.

سه تبدیل پایه‌ای که در این فصل بررسی می‌کنیم عبارت‌اند از:

```text
String()
Number()
Boolean()
```

---

## چرا Explicit Conversion مهم است؟

Explicit Conversion رفتار کد را آشکار می‌کند.

برای مثال:

```javascript
const quantity = Number(inputValue);
```

برای خواننده کد مشخص است که:

> این Value باید به Number تبدیل شود.

این موضوع در پروژه‌های واقعی اهمیت زیادی دارد، زیرا کد فقط باید برای موتور JavaScript قابل اجرا نباشد؛ باید برای سایر توسعه‌دهندگان نیز قابل فهم باشد.

---

## مثال

```javascript
const quantity = '3';

const totalItems = Number(quantity) + 2;

console.log(totalItems);
```

خروجی:

```text
5
```

---

## تحلیل مهندسی

در اینجا Conversion پیش از انجام عملیات انجام شده است:

```text
'3'
↓
Number('3')
↓
3
↓
3 + 2
↓
5
```

این ترتیب باعث می‌شود رفتار Expression قابل پیش‌بینی باشد.

---

## اشتباهات رایج

❌ تبدیل کردن Value فقط به این دلیل که امکان آن وجود دارد.

✔ Conversion باید با نیاز واقعی برنامه انجام شود.

---

❌ تصور اینکه هر String را می‌توان بدون مشکل به Number تبدیل کرد.

```javascript
Number('hello')
```

خروجی:

```text
NaN
```

این حالت را باید در طراحی برنامه در نظر گرفت.

---

## نکات مهم

* Explicit Conversion توسط برنامه‌نویس کنترل می‌شود.
* `String()`، `Number()` و `Boolean()` ابزارهای اصلی این فصل هستند.
* تبدیل صریح معمولاً خوانایی و قابلیت پیش‌بینی کد را افزایش می‌دهد.
* Conversion موفقیت‌آمیز بودن خود را تضمین نمی‌کند.

---

## پاسخ کوتاه طلایی مصاحبه

**Explicit Conversion چیست؟**

Explicit Conversion زمانی رخ می‌دهد که برنامه‌نویس به‌صورت مستقیم Value را به Type دیگری تبدیل کند؛ مانند `Number('30')`. این روش معمولاً رفتار مورد انتظار برنامه را واضح‌تر می‌کند.

---

# 3. تبدیل به String

## تعریف ساده

هرگاه بخواهیم یک Value را به یک String تبدیل کنیم، می‌توانیم از:

```javascript
String()
```

استفاده کنیم.

---

## تعریف فنی

`String()` یک Value را دریافت می‌کند و نمایش String متناظر با آن را تولید می‌کند.

برای مثال:

```javascript
String(42);
```

نتیجه:

```text
'42'
```

---

## چرا این مفهوم مهم است؟

در برنامه‌های واقعی، گاهی لازم است یک Value را برای نمایش، ساخت متن یا ارسال در قالب متنی آماده کنیم.

برای مثال:

```javascript
const orderId = 245;

const message = 'Order #' + String(orderId);
```

در اینجا تبدیل صریح مشخص می‌کند که `orderId` قرار است به‌عنوان بخشی از متن استفاده شود.

---

## مثال

```javascript
const orderId = 245;

const orderLabel = `Order #${String(orderId)}`;

console.log(orderLabel);
```

خروجی:

```text
Order #245
```

---

## تحلیل مهندسی

تبدیل به String به این معنا نیست که Value اصلی در همه جای برنامه تغییر می‌کند.

برای مثال:

```javascript
const orderId = 245;

const textId = String(orderId);
```

اکنون دو Value داریم:

```text
orderId → Number
textId  → String
```

Conversion یک Value جدید ایجاد می‌کند و Binding اولیه همچنان همان مقدار را نگه می‌دارد.

---

## اشتباهات رایج

❌ تصور اینکه:

```javascript
String(245)
```

متغیر اولیه را برای همیشه به String تبدیل می‌کند.

✔ Conversion نتیجه جدیدی تولید می‌کند.

---

## نکات مهم

* `String()` برای تبدیل صریح به String استفاده می‌شود.
* `String(245)` نتیجه `'245'` تولید می‌کند.
* Conversion Type یک Value را تغییر می‌دهد، نه معنای داده را.

---

## پاسخ کوتاه طلایی مصاحبه

**چرا ممکن است یک Value را به‌صورت صریح با `String()` تبدیل کنیم؟**

برای اینکه Type مورد انتظار را به‌صورت واضح مشخص کنیم؛ مثلاً وقتی یک Number باید به‌عنوان بخشی از یک متن استفاده شود.

---

# 4. تبدیل به Number

## تعریف ساده

اگر یک Value قابل تبدیل به Number باشد، می‌توانیم از:

```javascript
Number()
```

استفاده کنیم.

برای مثال:

```javascript
Number('30');
```

نتیجه:

```text
30
```

---

## تعریف فنی

`Number()` یک Value را دریافت می‌کند و تلاش می‌کند یک Number متناظر با آن ایجاد کند.

---

## چرا این مفهوم مهم است؟

یکی از رایج‌ترین موارد Conversion در برنامه‌های واقعی، تبدیل داده متنی به Number است.

برای مثال:

```javascript
const quantity = '4';

const total = Number(quantity) * 25;

console.log(total);
```

خروجی:

```text
100
```

---

## مثال

```javascript
const price = '49.99';

const numericPrice = Number(price);

console.log(numericPrice);
```

خروجی:

```text
49.99
```

---

## وقتی Conversion موفق نیست

همه Stringها را نمی‌توان به Number معتبر تبدیل کرد.

```javascript
const value = Number('hello');

console.log(value);
```

نتیجه:

```text
NaN
```

`NaN` مخفف **Not-a-Number** است و نشان می‌دهد نتیجه عملیات عددی معتبر نیست.

در این فصل فقط رفتار Conversion را بررسی می‌کنیم. روش‌های دقیق بررسی `NaN` در فصل مربوط به Numberها به‌صورت کامل بررسی خواهند شد.

---

## تحلیل مهندسی

Conversion به Number یک مرحله مهم در مرز ورود داده به برنامه است.

برای مثال، اگر مقدار تعداد کالا از یک ورودی متنی دریافت شود، بهتر است پیش از محاسبه مشخص کنیم که برنامه با String کار می‌کند یا Number.

```text
Input
↓
'4'
↓
Number()
↓
4
↓
Calculation
```

این مدل ذهنی از بسیاری از خطاهای محاسباتی جلوگیری می‌کند.

---

## اشتباهات رایج

❌ تصور اینکه هر String عددی است.

```javascript
'100'
```

و:

```javascript
'one hundred'
```

هر دو String هستند، اما فقط اولی قابلیت تبدیل مستقیم به Number معتبر را دارد.

---

❌ انجام محاسبات روی داده‌ای که Type آن مشخص نیست.

✔ ابتدا Type مورد نیاز عملیات را مشخص کنید.

---

## نکات مهم

* `Number()` برای تبدیل صریح به Number استفاده می‌شود.
* Stringهای عددی معمولاً به Number تبدیل می‌شوند.
* Conversion نامعتبر می‌تواند `NaN` تولید کند.
* داده‌های ورودی را نباید صرفاً بر اساس ظاهرشان Number فرض کرد.

---

## پاسخ کوتاه طلایی مصاحبه

**چرا `Number()` در برنامه‌های واقعی مهم است؟**

زیرا بسیاری از داده‌های ورودی به‌صورت String در اختیار برنامه قرار می‌گیرند، در حالی که منطق برنامه به Number نیاز دارد. `Number()` این تبدیل را به‌صورت صریح و قابل مشاهده انجام می‌دهد.

---

# 5. تبدیل به Boolean

## تعریف ساده

برای تبدیل یک Value به Boolean می‌توانیم از:

```javascript
Boolean()
```

استفاده کنیم.

```javascript
Boolean(1);
```

نتیجه:

```text
true
```

و:

```javascript
Boolean(0);
```

نتیجه:

```text
false
```

---

## تعریف فنی

`Boolean()` یک Value را به یکی از دو Boolean Value زیر تبدیل می‌کند:

```text
true
false
```

این تبدیل بر اساس قواعد **Truthy** و **Falsy** انجام می‌شود.

---

## چرا این مفهوم مهم است؟

Boolean Conversion در منطق شرطی بسیار مهم است.

در فصل هفتم با Truthy و Falsy آشنا شدیم و دیدیم که JavaScript می‌تواند Valueهای مختلف را در contextهای شرطی به‌صورت Boolean ارزیابی کند.

اکنون می‌توانیم همان مفهوم را به‌صورت صریح نیز انجام دهیم.

---

## مثال

```javascript
const cartItems = 3;

const hasItems = Boolean(cartItems);

console.log(hasItems);
```

خروجی:

```text
true
```

---

## چند Value رایج

برخی Valueهای مهم که به `false` تبدیل می‌شوند:

```javascript
Boolean(false);      // false
Boolean(0);          // false
Boolean('');         // false
Boolean(null);       // false
Boolean(undefined);  // false
NaN;                 // false when converted to Boolean
```

در مقابل:

```javascript
Boolean('0');        // true
Boolean('false');    // true
Boolean(1);          // true
Boolean([]);         // true
Boolean({});         // true
```

نکته مهم این است که:

```javascript
'0'
```

یک String خالی نیست.

بنابراین Truthy است.

---

## تحلیل مهندسی

این موضوع یکی از نقاطی است که تفاوت میان **Value** و **ظاهر Value** اهمیت پیدا می‌کند.

ممکن است:

```text
0
```

از نظر معنایی یک عدد صفر باشد و Falsy محسوب شود.

اما:

```text
'0'
```

یک String غیرخالی است و Truthy محسوب می‌شود.

بنابراین Type Conversion همیشه باید همراه با شناخت Type و مقدار واقعی Value تحلیل شود.

---

## اشتباهات رایج

❌ تصور اینکه String `'false'` به Boolean `false` تبدیل می‌شود.

```javascript
Boolean('false');
```

نتیجه:

```text
true
```

زیرا String غیرخالی است.

---

❌ تصور اینکه `0` و `'0'` رفتار Boolean یکسانی دارند.

✔ `0` Falsy است، اما `'0'` Truthy است.

---

## نکات مهم

* `Boolean()` تبدیل صریح به Boolean انجام می‌دهد.
* Truthy و Falsy اساس این تبدیل را تشکیل می‌دهند.
* String غیرخالی Truthy است.
* `0` Falsy است.
* `'0'` Truthy است.

---

## پاسخ کوتاه طلایی مصاحبه

**چرا `Boolean('false')` برابر `true` است؟**

زیرا JavaScript محتوای معنایی String را به‌عنوان کلمه `false` تفسیر نمی‌کند. هر String غیرخالی هنگام Boolean Conversion، Truthy است.

---

# 6. Type Coercion چیست؟

## تعریف ساده

گاهی برنامه‌نویس هیچ Conversion صریحی انجام نمی‌دهد، اما JavaScript برای انجام یک عملیات، Valueها را به Type دیگری تبدیل می‌کند.

این رفتار **Type Coercion** نام دارد.

برای مثال:

```javascript
const quantity = '2';

const total = quantity * 5;
```

نتیجه:

```text
10
```

در اینجا `quantity` همچنان String است، اما JavaScript هنگام انجام `*` آن را برای انجام عملیات عددی به Number تبدیل می‌کند.

---

## تعریف فنی

Type Coercion تبدیل ضمنی Valueها توسط قواعد زبان در جریان ارزیابی یک Expression است.

تفاوت اصلی با Explicit Conversion این است که:

```text
Explicit Conversion
→ برنامه‌نویس درخواست Conversion می‌کند.

Implicit Coercion
→ خود JavaScript Conversion لازم را انجام می‌دهد.
```

---

## چرا این مفهوم مهم است؟

Coercion یکی از ویژگی‌های مهم JavaScript است.

اما اگر رفتار آن را نشناسیم، می‌تواند باعث ایجاد نتایج غیرمنتظره شود.

برای مثال:

```javascript
'10' + 5
```

و:

```javascript
'10' - 5
```

رفتار یکسانی ندارند.

```text
'105'
```

در برابر:

```text
5
```

این تفاوت از قواعد Coercion ناشی می‌شود.

---

## مثال

```javascript
const quantity = '3';

const total = quantity * 20;

console.log(total);
```

خروجی:

```text
60
```

---

## تحلیل مهندسی

در اینجا JavaScript برای انجام عملیات ضرب، String را به Number تبدیل می‌کند:

```text
'3'
↓
Number-like coercion
↓
3
↓
3 × 20
↓
60
```

اما این بدان معنا نیست که:

```javascript
quantity
```

برای همیشه Number شده است.

```javascript
typeof quantity;
```

همچنان:

```text
string
```

است.

---

## اشتباهات رایج

❌ تصور اینکه Coercion Type متغیر را برای همیشه تغییر می‌دهد.

✔ Coercion مربوط به همان عملیات و ارزیابی Expression است.

---

❌ تصور اینکه همه Operatorها یک نوع Coercion انجام می‌دهند.

✔ قواعد تبدیل به Operator و context وابسته هستند.

---

## نکات مهم

* Coercion معمولاً ضمنی است.
* JavaScript هنگام انجام برخی عملیات Valueها را تبدیل می‌کند.
* Coercion Type متغیر را به‌صورت دائمی تغییر نمی‌دهد.
* شناخت قواعد Coercion برای تحلیل رفتار JavaScript ضروری است.

---

## پاسخ کوتاه طلایی مصاحبه

**تفاوت Conversion و Coercion چیست؟**

Conversion معمولاً به تبدیل صریح یک Value توسط برنامه‌نویس اشاره دارد، در حالی که Coercion تبدیل ضمنی Valueها توسط JavaScript در جریان ارزیابی Expression است.

---

# 7. Arithmetic Coercion

## تعریف ساده

برخی Operatorهای حسابی برای انجام عملیات عددی به Number نیاز دارند.

در چنین شرایطی JavaScript ممکن است Valueهای غیرعددی را به Number تبدیل کند.

برای مثال:

```javascript
'10' - 3
```

نتیجه:

```text
7
```

---

## چرا این مفهوم مهم است؟

اگر رفتار Operatorها را نشناسیم، ممکن است نتیجه یک Expression را اشتباه پیش‌بینی کنیم.

مثلاً:

```javascript
'10' * 2
```

نتیجه:

```text
20
```

اما:

```javascript
'10' + 2
```

نتیجه:

```text
102
```

است.

---

## مثال

```javascript
const price = '50';
const quantity = 2;

const total = price * quantity;

console.log(total);
```

خروجی:

```text
100
```

---

## تحلیل مهندسی

عملگر `*` یک عملیات عددی انجام می‌دهد.

بنابراین JavaScript برای اجرای آن باید Operandها را به شکل عددی ارزیابی کند.

اما `+` رفتار ویژه‌ای دارد و در صورت حضور String می‌تواند به Concatenation منجر شود.

این تفاوت یکی از مهم‌ترین نکات Type Coercion در JavaScript است.

---

## اشتباهات رایج

❌ فرض اینکه `+` و `*` از نظر Coercion یک رفتار دارند.

✔ `+` در حضور String می‌تواند Concatenation انجام دهد.

---

## نکات مهم

* Operatorهای حسابی می‌توانند باعث Coercion شوند.
* `*`، `/` و `-` معمولاً برای عملیات عددی Valueها را به Number تبدیل می‌کنند.
* `+` رفتار متفاوتی دارد و باید با دقت تحلیل شود.

---

## پاسخ کوتاه طلایی مصاحبه

**چرا `'10' - 5` برابر `5` است ولی `'10' + 5` برابر `'105'`؟**

چون `-` یک عملیات عددی است و Operandها را به Number تبدیل می‌کند، اما `+` در حضور String می‌تواند به String Concatenation تبدیل شود.

---

# 8. Comparison Coercion

## تعریف ساده

در مقایسه‌ها نیز JavaScript ممکن است برای مقایسه دو Value، Type Conversion انجام دهد.

این موضوع به‌خصوص هنگام استفاده از `==` اهمیت دارد.

برای مثال:

```javascript
'5' == 5
```

نتیجه:

```text
true
```

اما:

```javascript
'5' === 5
```

نتیجه:

```text
false
```

---

## چرا این مفهوم مهم است؟

مقایسه یکی از رایج‌ترین عملیات در برنامه‌های JavaScript است.

اگر ندانیم Operator مورد استفاده چگونه Typeها را بررسی می‌کند، ممکن است شرط‌هایی بنویسیم که نتیجه آن‌ها برخلاف انتظار باشد.

---

## مثال

```javascript
const userId = '42';

if (userId == 42) {
  console.log('User found');
}
```

این شرط `true` خواهد بود، زیرا `==` در این حالت اجازه Conversion را می‌دهد.

اما با:

```javascript
if (userId === 42) {
  console.log('User found');
}
```

شرط `false` خواهد بود.

---

## تحلیل مهندسی

در `==`، Typeهای متفاوت می‌توانند باعث انجام Conversion شوند.

در `===`، Typeها نیز بخشی از مقایسه هستند.

بنابراین:

```text
'42'
```

و:

```text
42
```

با `===` برابر نیستند.

این تفاوت دلیل اصلی توصیه گسترده به استفاده از **Strict Equality** در کدهای مدرن است.

---

## اشتباهات رایج

❌ تصور اینکه `==` و `===` فقط از نظر Syntax متفاوت‌اند.

✔ رفتار آن‌ها در Type Conversion متفاوت است.

---

❌ تصور اینکه `==` همیشه بد است.

✔ `==` بخشی معتبر از زبان است، اما رفتار Conversion آن باید آگاهانه درک شود.

---

## نکات مهم

* `==` می‌تواند باعث Type Coercion شود.
* `===` بدون Type Conversion ضمنی معمول مقایسه را انجام می‌دهد.
* در کدهای حرفه‌ای، `===` معمولاً انتخاب پیش‌فرض مناسب‌تری است.
* درک `==` برای تحلیل کدهای موجود و مصاحبه‌های فنی ضروری است.

---

## پاسخ کوتاه طلایی مصاحبه

**تفاوت `==` و `===` چیست؟**

`==` در صورت نیاز می‌تواند قبل از مقایسه Type Conversion انجام دهد، اما `===` هم Type و هم Value را بدون این نوع Coercion مقایسه می‌کند.

---

# 9. `==` و `===` در عمل

## یک مدل ذهنی ساده

هنگام مواجهه با:

```javascript
a == b
```

از خود بپرسید:

> آیا JavaScript برای قابل‌مقایسه کردن این دو Value نیاز به Conversion دارد؟

اما در:

```javascript
a === b
```

سؤال ساده‌تر است:

> آیا Type و Value هر دو برابر هستند؟

---

## مثال

```javascript
const inputId = '100';

console.log(inputId == 100);
console.log(inputId === 100);
```

خروجی:

```text
true
false
```

در خط اول Coercion اجازه می‌دهد مقایسه انجام شود.

در خط دوم Type متفاوت است:

```text
String !== Number
```

پس نتیجه `false` است.

---

## تحلیل مهندسی

در پروژه واقعی، داده‌های ورودی ممکن است از Type متفاوتی نسبت به داده‌های داخلی برنامه برخوردار باشند.

به‌جای اینکه همیشه اجازه دهیم `==` این تفاوت را پنهان کند، بهتر است در مرز ورود داده، Type مورد انتظار را مشخص کنیم.

برای مثال:

```javascript
const inputId = '100';

const id = Number(inputId);

if (id === 100) {
  // ...
}
```

اکنون منطق برنامه شفاف‌تر است.

```text
External Data
↓
Conversion
↓
Expected Type
↓
Strict Comparison
```

این الگو معمولاً از تکیه بر Coercion پنهان خواناتر است.

---

# 10. Common Coercion Rules

برای تحلیل رفتار JavaScript لازم نیست تمام جزئیات الگوریتم‌های استاندارد را در این فصل حفظ کنیم.

اما چند قاعده عملی بسیار مهم هستند.

### Rule 1 — String + Value

اگر `+` در یک Expression با String روبه‌رو شود، ممکن است نتیجه به‌صورت String Concatenation تولید شود.

```javascript
'5' + 2;
```

نتیجه:

```text
'52'
```

---

### Rule 2 — Numeric Operators

Operatorهایی مانند:

```javascript
-
*
/
```

برای عملیات عددی به Number نیاز دارند و می‌توانند باعث Coercion شوند.

```javascript
'10' - 2; // 8
```

---

### Rule 3 — Boolean Context

Valueها در contextهای Boolean می‌توانند به Truthy یا Falsy تبدیل شوند.

```javascript
if ('hello') {
  console.log('User has a name');
}
```

رشته غیرخالی Truthy است.

---

### Rule 4 — `==`

Loose Equality می‌تواند Conversion انجام دهد.

```javascript
'10' == 10;
```

نتیجه:

```text
true
```

---

### Rule 5 — `===`

Strict Equality به Type متفاوت اجازه نمی‌دهد که صرفاً با Conversion برابر شوند.

```javascript
'10' === 10;
```

نتیجه:

```text
false
```

---

# 11. Explicit Conversion در برابر Implicit Coercion

اکنون می‌توانیم تفاوت این دو مفهوم را در یک مثال ببینیم.

### Explicit

```javascript
const quantity = '3';

const numericQuantity = Number(quantity);

const total = numericQuantity * 20;
```

Conversion کاملاً آشکار است.

---

### Implicit

```javascript
const quantity = '3';

const total = quantity * 20;
```

JavaScript برای انجام `*` Conversion لازم را به‌صورت ضمنی انجام می‌دهد.

---

## تحلیل مهندسی

هر دو کد می‌توانند نتیجه مشابهی داشته باشند.

اما تفاوت مهمی در **قابل‌فهم بودن Intent** وجود دارد.

در نسخه Explicit:

```javascript
Number(quantity)
```

به خواننده اعلام می‌شود که Conversion بخشی از منطق برنامه است.

این موضوع در پروژه‌های بزرگ اهمیت زیادی دارد.

---

# 12. Common Surprises

Type Coercion زمانی مشکل‌ساز می‌شود که برنامه‌نویس از قواعد آن اطلاع نداشته باشد.

برای مثال:

```javascript
'0' == false
```

می‌تواند نتیجه‌ای متفاوت از چیزی باشد که در نگاه اول انتظار داریم.

یا:

```javascript
'' == 0
```

نیز ممکن است باعث تعجب شود.

هدف این فصل حفظ کردن تمام این موارد نیست.

مدل ذهنی مهم‌تر این است:

> **هر زمان از `==` یا Operatorهایی که Typeها را با هم ترکیب می‌کنند استفاده می‌کنید، احتمال Coercion را در نظر بگیرید.**

---

# دیدگاه Jonas

در رویکرد آموزشی Jonas Schmedtmann، شناخت Type Coercion برای فهم رفتار JavaScript ضروری است، اما توصیه اصلی در کدنویسی روزمره، تکیه نکردن بر رفتارهای مبهم و غیرضروری زبان است.

رویکرد عملی این است:

* Type مورد انتظار را مشخص کنید.
* در صورت نیاز Conversion را صریح انجام دهید.
* برای مقایسه‌ها معمولاً از `===` استفاده کنید.
* رفتارهای Coercion را بشناسید تا بتوانید کد JavaScript را تحلیل و Debug کنید.

این دیدگاه با هدف این فصل نیز هماهنگ است: **شناخت Coercion برای تحلیل رفتار زبان، نه استفاده بی‌دلیل از آن.**

---

# 13. Professional Practices

## قانون اول: Type مورد انتظار را مشخص کنید

به‌جای اینکه اجازه دهیم Conversion در چند نقطه مختلف اتفاق بیفتد، بهتر است در محل مناسب Type داده را مشخص کنیم.

```javascript
const quantity = Number(inputValue);
```

سپس:

```javascript
const total = quantity * price;
```

---

## قانون دوم: Conversion را در مرز داده انجام دهید

اگر داده از یک منبع خارجی وارد برنامه می‌شود، بهتر است Type آن را در همان نقطه مشخص کنیم.

```text
External Data
↓
Validation / Conversion
↓
Application Logic
```

این الگو باعث می‌شود منطق داخلی برنامه قابل پیش‌بینی‌تر باشد.

---

## قانون سوم: از Coercion ناخواسته دوری کنید

این کد:

```javascript
const total = '5' * 20;
```

ممکن است کار کند.

اما اگر `5` قرار است در منطق برنامه یک Quantity باشد، بهتر است Type آن از ابتدا مشخص باشد.

```javascript
const quantity = Number(inputValue);

const total = quantity * 20;
```

---

## قانون چهارم: `===` را انتخاب پیش‌فرض قرار دهید

در بیشتر مقایسه‌های کاربردی:

```javascript
if (userId === currentUserId) {
  // ...
}
```

شفاف‌تر از تکیه بر Conversion ضمنی است.

---

# Common Mistakes

### اشتباه ۱

```javascript
const quantity = '10';

console.log(quantity + 5);
```

تصور می‌کنیم نتیجه:

```text
15
```

است.

اما نتیجه:

```text
'105'
```

است.

---

### اشتباه ۲

```javascript
Boolean('false');
```

تصور می‌کنیم:

```text
false
```

است.

اما نتیجه:

```text
true
```

است.

---

### اشتباه ۳

```javascript
'10' == 10
```

تصور می‌کنیم چون Typeها متفاوت‌اند، نتیجه `false` است.

اما `==` می‌تواند Coercion انجام دهد و نتیجه `true` شود.

---

### اشتباه ۴

```javascript
const id = '42';

if (id === 42) {
  // ...
}
```

در اینجا مشکل الزاماً Operator نیست.

ممکن است داده ورودی Type مورد انتظار برنامه را نداشته باشد.

---

# خلاصه فصل

در فصل‌های قبل یاد گرفتیم که هر Value در JavaScript دارای یک Type است.

در این فصل دیدیم که Type یک Value همیشه ثابت نمی‌ماند و JavaScript در شرایط مختلف می‌تواند Valueها را بین Typeهای مختلف تبدیل کند.

ابتدا مفهوم **Type Conversion** را بررسی کردیم.

سپس میان دو شکل اصلی آن تفاوت گذاشتیم:

```text
Explicit Conversion
↓
توسط برنامه‌نویس

Implicit Coercion
↓
توسط JavaScript
```

در ادامه با تبدیل صریح Valueها به سه Type مهم آشنا شدیم:

```javascript
String()
Number()
Boolean()
```

سپس دیدیم که JavaScript در برخی عملیات‌ها مانند:

```javascript
'10' - 5
```

به‌صورت ضمنی Conversion انجام می‌دهد.

همچنین بررسی کردیم که `+` در حضور String می‌تواند رفتار متفاوتی داشته باشد:

```javascript
'10' + 5
```

و در نهایت ارتباط Type Coercion با Equality را بررسی کردیم:

```javascript
== 
===
```

مهم‌ترین نتیجه فصل این است که **Coercion بخشی واقعی از رفتار JavaScript است و باید آن را شناخت؛ اما در کدنویسی حرفه‌ای بهتر است Typeهای مورد انتظار را تا حد امکان به‌صورت واضح مشخص کنیم.**

---

# Key Takeaways

در پایان این فصل باید بتوانید:

* Type Conversion را تعریف کنید.
* تفاوت Explicit Conversion و Implicit Coercion را توضیح دهید.
* از `String()` برای تبدیل صریح به String استفاده کنید.
* از `Number()` برای تبدیل صریح به Number استفاده کنید.
* از `Boolean()` برای تبدیل صریح به Boolean استفاده کنید.
* بدانید Conversion نامعتبر به Number می‌تواند `NaN` تولید کند.
* تفاوت `'0'` و `0` را از نظر Boolean Conversion توضیح دهید.
* رفتار متفاوت `+` با String را تحلیل کنید.
* بدانید Operatorهایی مانند `-` و `*` می‌توانند باعث Numeric Coercion شوند.
* تفاوت `==` و `===` را از نظر Coercion توضیح دهید.
* بدانید Coercion Type متغیر را به‌صورت دائمی تغییر نمی‌دهد.
* در کدهای حرفه‌ای، Conversion را در نقاط مشخص و قابل مشاهده انجام دهید.
* `===` را در بیشتر مقایسه‌های روزمره به‌عنوان انتخاب پیش‌فرض در نظر بگیرید.

---

# Technical Interview

## Junior

### سؤال ۱

Type Conversion چیست؟

### سؤال ۲

تفاوت Explicit Conversion و Implicit Coercion چیست؟

### سؤال ۳

چگونه یک String را به Number تبدیل می‌کنیم؟

### سؤال ۴

نتیجه این Expression چیست؟

```javascript
'10' + 5
```

### سؤال ۵

چرا نتیجه این Expression عددی است؟

```javascript
'10' - 5
```

### سؤال ۶

چرا نتیجه زیر `true` است؟

```javascript
Boolean('false')
```

---

## Mid-Level

### سؤال ۷

چرا `==` می‌تواند باعث نتایج غیرمنتظره شود؟

### سؤال ۸

تفاوت `==` و `===` از نظر Type Conversion چیست؟

### سؤال ۹

آیا Type Coercion مقدار متغیر را برای همیشه تغییر می‌دهد؟

### سؤال ۱۰

چرا بهتر است داده‌های ورودی را پیش از استفاده در منطق برنامه تبدیل کنیم؟

### سؤال ۱۱

تفاوت این دو چیست؟

```javascript
Number('10') + 5
```

و:

```javascript
'10' + 5
```

### سؤال ۱۲

چرا `0` و `'0'` در Boolean Conversion رفتار یکسانی ندارند؟

---

## Senior

### سؤال ۱۳

چرا JavaScript اصلاً Type Coercion را در طراحی زبان خود دارد و این ویژگی چه Trade-offی ایجاد می‌کند؟

### سؤال ۱۴

چگونه می‌توان در یک Application بزرگ اثرات منفی Implicit Coercion را کاهش داد؟

### سؤال ۱۵

چرا Conversion در مرز ورود داده می‌تواند معماری داخلی برنامه را قابل‌اعتمادتر کند؟

### سؤال ۱۶

آیا استفاده از `==` همیشه یک خطای مهندسی است؟ استدلال کنید.

### سؤال ۱۷

چگونه تفاوت Type Conversion و Type Coercion به Debugging یک Bug کمک می‌کند؟

---

# Golden Answers

## Type Conversion چیست؟

Type Conversion یعنی تبدیل یک Value از یک Type به Type دیگر. این تبدیل می‌تواند به‌صورت صریح توسط برنامه‌نویس یا به‌صورت ضمنی توسط JavaScript انجام شود.

---

## تفاوت Conversion و Coercion چیست؟

Conversion معمولاً به تبدیل آگاهانه یک Value به Type دیگر اشاره دارد. Coercion زمانی رخ می‌دهد که JavaScript برای ارزیابی یک Expression یا انجام یک عملیات، Conversion را به‌صورت ضمنی انجام دهد.

---

## چرا `'10' + 5` برابر `'105'` است؟

زیرا `+` در حضور String می‌تواند به Concatenation تبدیل شود. در نتیجه `5` نیز در این عملیات به نمایش متنی تبدیل می‌شود.

---

## چرا `'10' - 5` برابر `5` است؟

زیرا `-` یک عملیات عددی است و JavaScript برای انجام آن Operandهای لازم را به Number تبدیل می‌کند.

---

## چرا `Boolean('false')` برابر `true` است؟

زیرا JavaScript معنای لغوی String را بررسی نمی‌کند. هر String غیرخالی هنگام Boolean Conversion، Truthy است.

---

## تفاوت `==` و `===` چیست؟

`==` می‌تواند پیش از مقایسه Type Conversion انجام دهد، اما `===` Type و Value را بدون چنین Coercion معمولی مقایسه می‌کند. به همین دلیل `===` در بیشتر کدهای کاربردی انتخاب شفاف‌تری است.

---

## چرا Conversion در مرز ورود داده مفید است؟

زیرا Type داده را قبل از ورود به منطق اصلی برنامه مشخص می‌کند. در نتیجه بخش داخلی Application می‌تواند با فرض‌های Type مشخص و قابل پیش‌بینی کار کند.

---

## آیا `==` همیشه اشتباه است؟

خیر. `==` بخشی معتبر از زبان است و در برخی شرایط می‌تواند عمداً استفاده شود. بااین‌حال، به دلیل قواعد Coercion، استفاده از `===` در بیشتر کدهای کاربردی رفتار شفاف‌تر و قابل پیش‌بینی‌تری ایجاد می‌کند.

---

# Conclusion

Type Conversion یکی از نقاطی است که در آن مدل ذهنی صحیح درباره **Value** و **Type** اهمیت خود را نشان می‌دهد.

JavaScript زبانی Dynamic است و در بسیاری از عملیات‌ها می‌تواند Typeها را به‌صورت ضمنی تبدیل کند.

اگر این رفتار را نشناسیم، Expressionهایی مانند:

```javascript
'10' + 5
```

یا:

```javascript
'10' == 10
```

می‌توانند غیرقابل پیش‌بینی به نظر برسند.

اما وقتی تفاوت میان:

```text
Value
↓
Type
↓
Conversion
↓
Coercion
```

را درک کنیم، این رفتارها دیگر تصادفی نیستند.

در کدنویسی حرفه‌ای، هدف حذف کامل Coercion از JavaScript نیست.

هدف این است که:

* بدانیم چه زمانی رخ می‌دهد.
* نتیجه آن را بتوانیم پیش‌بینی کنیم.
* در صورت نیاز Conversion را صریح انجام دهیم.
* و از رفتارهای ضمنی غیرضروری در منطق اصلی برنامه دوری کنیم.

این مدل ذهنی در فصل‌های بعدی، هنگام کار با Functions، Objects و Runtime، پایه مهمی برای تحلیل رفتار JavaScript خواهد بود.

---

## مرجع مفهومی فصل

جریان این فصل را می‌توان در یک مدل ذهنی خلاصه کرد:

```text
Value
   ↓
Type
   ↓
Need another Type?
   ↓
 ┌─────────────────────┐
 │                     │
Explicit           Implicit
Conversion         Coercion
 │                     │
 ↓                     ↓
String()           Operator Rules
Number()           Comparison
Boolean()          Boolean Context
 │                     │
 └──────────┬──────────┘
            ↓
      Predictable Code
```

**اصل مهندسی فصل:**

> **Typeها را بشناس، Conversion را آگاهانه انجام بده و Coercion را همیشه بتوانی توضیح دهی.**
