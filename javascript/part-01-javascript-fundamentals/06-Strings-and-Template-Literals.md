# Chapter 06 — Strings and Template Literals

---

# Chapter Goal

پس از مطالعه این فصل انتظار می‌رود بتوانید:

- مفهوم **String** را به‌عنوان یکی از مهم‌ترین انواع داده در JavaScript توضیح دهید.
- تفاوت داده متنی (Text Data) و String را درک کنید.
- String Literal و روش‌های مختلف ایجاد String را توضیح دهید.
- کاربرد Escape Character و Escape Sequenceهای رایج را تحلیل کنید.
- String Concatenation و محدودیت‌های آن را درک کنید.
- Valueهای مختلف را در صورت نیاز به String تبدیل کنید.
- Template Literal و Expression Interpolation را به‌درستی به کار ببرید.
- متن‌های چندخطی و Dynamic Text را با Template Literal تولید کنید.
- Tagged Template را در سطح مقدماتی بشناسید.
- Best Practiceهای استفاده از String و Template Literal را در پروژه‌های واقعی تشخیص دهید.
- به پرسش‌های فنی مرتبط با Strings و Template Literals پاسخ دهید.

---

# Core Question

> **JavaScript چگونه داده‌های متنی را ذخیره، ترکیب و تولید می‌کند؟**

---

# Concept Flow

```text
Information
↓
Text Data
↓
String
↓
String Literal
↓
Escape Characters
↓
Concatenation
↓
String Conversion
↓
Template Literals
↓
Interpolation
↓
Multiline Strings
↓
Tagged Templates Introduction
↓
Dynamic Text
↓
Best Practices
```

---

# مقدمه

تقریباً تمام برنامه‌هایی که هر روز از آن‌ها استفاده می‌کنیم، حجم زیادی از **داده‌های متنی** را پردازش می‌کنند.

برای مثال:

- نام کاربران
- آدرس ایمیل
- پیام‌های خطا
- عنوان محصولات
- توضیحات کالاها
- پیام‌های شبکه‌های اجتماعی
- آدرس صفحات وب
- فایل‌های JSON
- کدهای HTML و CSS

تمام این اطلاعات در نهایت به‌صورت متن ذخیره و پردازش می‌شوند.

به همین دلیل، یکی از پرکاربردترین انواع داده در JavaScript، **String** است.

در نگاه اول ممکن است رشته‌ها بسیار ساده به نظر برسند؛ اما در عمل، بخش بزرگی از منطق بسیاری از برنامه‌های وب بر پایه پردازش رشته‌ها نوشته می‌شود.

---

# داده متنی (Text Data) چیست؟

پیش از آنکه درباره String صحبت کنیم، بهتر است ابتدا مفهوم **داده متنی** را بشناسیم.

هر اطلاعاتی که برای نمایش به انسان از کاراکترها استفاده کند، داده متنی محسوب می‌شود.

برای مثال:

```text
Omid
```

یا

```text
JavaScript
```

یا

```text
Welcome Back!
```

همگی نمونه‌هایی از داده متنی هستند.

کامپیوتر این داده‌ها را به شکل عددهای دودویی در حافظه ذخیره می‌کند، اما JavaScript آن‌ها را در قالب نوع داده‌ای به نام **String** در اختیار برنامه‌نویس قرار می‌دهد.

---

# String چیست؟

String نوع داده‌ای است که برای نگهداری متن استفاده می‌شود.

هر رشته، از کنار هم قرار گرفتن صفر یا چند **Character** تشکیل می‌شود.

برای مثال:

```javascript
'JavaScript'
```

این رشته از ده کاراکتر تشکیل شده است.

یا:

```javascript
'Hello'
```

پنج کاراکتر دارد.

حتی رشته زیر نیز یک String معتبر است:

```javascript
''
```

زیرا یک رشته می‌تواند هیچ کاراکتری نداشته باشد.

به چنین رشته‌ای **Empty String** گفته می‌شود.

---

## تعریف ساده

String مجموعه‌ای از کاراکترهاست که برای ذخیره و پردازش متن استفاده می‌شود.

---

## تعریف فنی

در JavaScript، **String** یکی از هفت نوع داده اولیه (**Primitive Data Type**) است که برای نمایش داده‌های متنی به کار می‌رود.

---

# Character چیست؟

هر حرف، عدد، فاصله یا نمادی که در یک رشته قرار می‌گیرد، یک **Character** محسوب می‌شود.

برای مثال:

```text
JavaScript
```

از کاراکترهای زیر تشکیل شده است:

```text
J
a
v
a
S
c
r
i
p
t
```

فاصله نیز یک Character محسوب می‌شود.

برای مثال:

```text
Front End
```

دارای ۹ حرف و یک فاصله است.

JavaScript همه آن‌ها را به‌عنوان Character ذخیره می‌کند.

---

# String Literal چیست؟

وقتی یک رشته را مستقیماً داخل کد می‌نویسیم، در واقع یک **String Literal** ایجاد کرده‌ایم.

برای مثال:

```javascript
'Hello World'
```

یا

```javascript
"JavaScript"
```

هر دو نمونه، String Literal هستند.

واژه **Literal** به این معناست که مقدار مستقیماً داخل کد نوشته شده است، نه اینکه در زمان اجرا محاسبه شود.

برای مثال:

```javascript
const language = 'JavaScript';
```

رشته:

```javascript
'JavaScript'
```

یک String Literal است.

---

# روش‌های ایجاد String

JavaScript دو روش اصلی برای ایجاد رشته در اختیار ما قرار می‌دهد.

## Single Quote

```javascript
'JavaScript'
```

---

## Double Quote

```javascript
"JavaScript"
```

هر دو کاملاً معتبر هستند.

```javascript
const first = 'Hello';

const second = "Hello";
```

نتیجه هر دو دقیقاً یکسان است.

---

# آیا تفاوتی میان Single Quote و Double Quote وجود دارد؟

از نظر موتور JavaScript، خیر.

هر دو یک نوع داده تولید می‌کنند.

```javascript
typeof 'Hello';
```

نتیجه:

```text
string
```

و

```javascript
typeof "Hello";
```

نیز دقیقاً همان خروجی را تولید می‌کند.

```text
string
```

در نتیجه، تفاوت آن‌ها تنها در **سبک نگارش (Coding Style)** است.

---

# کدام روش بهتر است؟

از نظر فنی، هیچ برتری میان Single Quote و Double Quote وجود ندارد.

اما تقریباً تمام پروژه‌های حرفه‌ای، یک استاندارد مشخص را انتخاب می‌کنند و در سراسر پروژه همان روش را ادامه می‌دهند.

برای مثال، در بسیاری از مثال‌های Jonas Schmedtmann از:

```javascript
'Hello'
```

استفاده شده است.

اما برخی پروژه‌ها نیز استفاده از Double Quote را استاندارد خود قرار می‌دهند.

مهم‌ترین اصل این است که در یک پروژه، از یک سبک ثابت استفاده شود.

---

# Jonas Perspective

Jonas در دوره آموزشی خود تأکید می‌کند که انتخاب میان Single Quote و Double Quote یک موضوع سلیقه‌ای نیست، بلکه یک موضوع مربوط به **Consistency** است.

به اعتقاد او، مهم نیست کدام روش را انتخاب می‌کنید؛ مهم این است که تمام اعضای تیم از یک استاندارد یکسان پیروی کنند.

---

# اشتباهات رایج

❌ تصور اینکه Single Quote و Double Quote نوع داده متفاوتی تولید می‌کنند.

✔ هر دو از نوع:

```text
string
```

هستند.

---

❌ تصور اینکه رشته باید حتماً دارای چند حرف باشد.

✔ حتی رشته خالی نیز یک String معتبر است.

```javascript
''
```

---

❌ تصور اینکه String فقط شامل حروف است.

✔ هر رشته می‌تواند شامل:

- حروف
- اعداد
- فاصله
- علامت‌ها
- Emoji
- یا ترکیبی از همه آن‌ها باشد.

---

# نکات مهم

- String یکی از Primitive Data Typeهای JavaScript است.
- رشته برای نگهداری داده‌های متنی استفاده می‌شود.
- هر رشته از صفر یا چند Character تشکیل شده است.
- رشته خالی نیز یک String معتبر است.
- Single Quote و Double Quote از نظر عملکرد تفاوتی ندارند.
- استفاده از یک Coding Style ثابت، مهم‌تر از انتخاب نوع Quote است.

---
---

# Escape Characters

در بخش قبل دیدیم که یک String می‌تواند با استفاده از Single Quote یا Double Quote ایجاد شود.

اما گاهی لازم است کاراکترهایی را داخل رشته قرار دهیم که خود JavaScript آن‌ها را به‌عنوان بخشی از سینتکس زبان تفسیر می‌کند.

برای مثال:

```text
'
```

یا

```text
"
```

یا

```text
\
```

یا حتی رفتن به خط بعد.

در چنین شرایطی از **Escape Character** استفاده می‌کنیم.

---

# Escape Character چیست؟

Escape Character روشی است که به موتور JavaScript اعلام می‌کند:

> «کاراکتر بعدی را به‌عنوان بخشی از متن در نظر بگیر، نه بخشی از سینتکس زبان.»

در JavaScript، Escape Character همان بک‌اسلش است.

```javascript
\
```

هرگاه موتور JavaScript این علامت را مشاهده کند، رفتار عادی کاراکتر بعدی را تغییر می‌دهد.

---

## تعریف ساده

Escape Character باعث می‌شود بتوانیم کاراکترهای ویژه را داخل رشته بنویسیم.

---

## تعریف فنی

Escape Sequence ترکیبی از بک‌اسلش (`\`) و یک کاراکتر دیگر است که معنی ویژه‌ای برای موتور JavaScript ایجاد می‌کند.

---

# نوشتن علامت نقل‌قول داخل String

فرض کنید بخواهیم رشته زیر را نمایش دهیم.

```text
I'm learning JavaScript
```

اگر بنویسیم:

```javascript
'I'm learning JavaScript'
```

JavaScript تصور می‌کند رشته بعد از:

```text
I'
```

به پایان رسیده است.

در نتیجه خطای Syntax Error ایجاد می‌شود.

راه‌حل استفاده از Escape Character است.

```javascript
'I\'m learning JavaScript'
```

اکنون خروجی صحیح خواهد بود.

---

همین موضوع درباره Double Quote نیز وجود دارد.

```javascript
"He said: \"Hello\""
```

خروجی:

```text
He said: "Hello"
```

---

# Backslash داخل String

اگر خود علامت بک‌اسلش را بخواهیم نمایش دهیم نیز باید آن را Escape کنیم.

```javascript
"C:\\Program Files\\Google"
```

خروجی:

```text
C:\Program Files\Google
```

زیرا:

```javascript
\\
```

به معنای یک بک‌اسلش واقعی است.

---

# رفتن به خط بعد

گاهی لازم است متن در چند خط نمایش داده شود.

برای این کار از:

```javascript
\n
```

استفاده می‌کنیم.

حرف `n` مخفف **New Line** است.

مثال:

```javascript
console.log('HTML\nCSS\nJavaScript');
```

خروجی:

```text
HTML
CSS
JavaScript
```

---

# Tab Character

گاهی لازم است بین دو بخش متن فاصله‌ای معادل یک Tab قرار گیرد.

برای این کار از:

```javascript
\t
```

استفاده می‌شود.

مثال:

```javascript
console.log('Name\tAge');
```

خروجی:

```text
Name    Age
```

---

# رایج‌ترین Escape Sequenceها

| Escape | Description |
|---------|-------------|
| `\'` | Single Quote |
| `\"` | Double Quote |
| `\\` | Backslash |
| `\n` | New Line |
| `\t` | Tab |

---

# آیا هنوز به Escape Character نیاز داریم؟

پاسخ:

بله.

اما نسبت به گذشته بسیار کمتر.

دلیل آن این است که از زمان معرفی **Template Literal** در ES6، بسیاری از مشکلات مربوط به رشته‌های چندخطی و ترکیب متن برطرف شده است.

در بلوک بعد با Template Literal آشنا خواهیم شد.

---

# String Concatenation

پیش از معرفی Template Literal، برنامه‌نویسان برای ترکیب چند رشته از عملگر `+` استفاده می‌کردند.

به این روش:

**String Concatenation**

گفته می‌شود.

---

## مثال

```javascript
const firstName = 'Omid';
const lastName = 'Sadeghi';

const fullName = firstName + ' ' + lastName;

console.log(fullName);
```

خروجی:

```text
Omid Sadeghi
```

در این مثال:

```javascript
+
```

به جای جمع کردن عددها، دو رشته را به یکدیگر متصل کرده است.

---

# عملگر + همیشه جمع انجام نمی‌دهد

یکی از ویژگی‌های جالب JavaScript این است که رفتار عملگر `+` به نوع داده‌ها بستگی دارد.

اگر دو Operand عدد باشند:

```javascript
10 + 5
```

نتیجه:

```text
15
```

اما اگر یکی از Operandها رشته باشد:

```javascript
'10' + 5
```

نتیجه:

```text
105
```

خواهد بود.

زیرا JavaScript رشته‌ها را به یکدیگر متصل می‌کند.

این رفتار یکی از اولین نمونه‌های **Type Coercion** است که در فصل مربوط به Type Conversion به‌صورت کامل بررسی خواهد شد.

---

# مشکلات String Concatenation

اگر رشته‌های کمی داشته باشیم، Concatenation کاملاً مناسب است.

اما فرض کنید بخواهیم جمله زیر را تولید کنیم.

```text
My name is Omid and I am 30 years old.
```

با روش قدیمی باید بنویسیم:

```javascript
const message =
  'My name is ' +
  firstName +
  ' and I am ' +
  age +
  ' years old.';
```

با بزرگ‌تر شدن متن:

- خوانایی کاهش پیدا می‌کند.
- احتمال فراموش کردن فاصله‌ها بیشتر می‌شود.
- نگهداری کد دشوارتر خواهد شد.

همین مشکل یکی از مهم‌ترین دلایل معرفی **Template Literal** در ES6 بود.

---

# String Conversion

در بسیاری از موقعیت‌ها لازم است یک Value را به یک String تبدیل کنیم تا بتوانیم آن را در یک متن قرار دهیم یا به‌عنوان داده متنی با آن کار کنیم.

یکی از روش‌های روشن و صریح برای این کار استفاده از `String()` است.

```javascript
const productId = 42;
const textId = String(productId);

console.log(textId);
console.log(typeof textId);
```

خروجی:

```text
42
string
```

در این مثال Value اولیه از نوع `Number` است، اما نتیجه `String(productId)` یک String جدید است.

## چرا String Conversion مهم است؟

در Applicationهای واقعی، داده‌ها همیشه از ابتدا به شکل String در اختیار ما نیستند. ممکن است یک API یک Number برگرداند، اما هنگام ساخت یک پیام متنی به نمایش متنی آن نیاز داشته باشیم.

برای مثال:

```javascript
const orderId = 125;
const message = `Order #${String(orderId)} is ready.`;
```

در بسیاری از Template Literalها تبدیل مقدار به String در فرایند تولید متن به‌صورت ضمنی انجام می‌شود؛ بااین‌حال شناخت **Explicit Conversion** برای زمانی که می‌خواهیم Intent خود را روشن بیان کنیم اهمیت دارد. جزئیات Type Conversion و Type Coercion در فصل **Type Conversion and Coercion** بررسی خواهد شد.

---

# Common Patterns

Stringها در Applicationهای واقعی معمولاً برای ساخت پیام، نمایش داده و ترکیب بخش‌های مختلف متن استفاده می‌شوند. چند الگوی ساده و پرکاربرد عبارت‌اند از:

### ساخت پیام از داده‌های برنامه

```javascript
const userName = 'Omid';
const orderId = 125;

const message = `User ${userName} created order #${orderId}.`;
```

### ترکیب متن ثابت و Dynamic Data

```javascript
const productName = 'Laptop';
const price = 1200;

const label = `${productName} - $${price}`;
```

### استفاده از Expression برای تولید متن

```javascript
const quantity = 3;
const price = 120;

const total = `Total: $${quantity * price}`;
```

این الگوها نشان می‌دهند که String فقط برای نگهداری متن ثابت استفاده نمی‌شود؛ بلکه می‌تواند خروجی قابل نمایش را از ترکیب داده‌های مختلف Application تولید کند.

---

# Jonas Perspective

Jonas توضیح می‌دهد که پیش از ES6 تقریباً تمام پروژه‌های JavaScript با String Concatenation نوشته می‌شدند.

اما پس از معرفی Template Literal، این روش تقریباً به‌طور کامل کنار گذاشته شد.

او توصیه می‌کند:

> امروزه تنها برای درک کدهای قدیمی باید Concatenation را بشناسید؛ در پروژه‌های جدید، Template Literal انتخاب استاندارد است.

---

# اشتباهات رایج

❌ تصور اینکه عملگر `+` همیشه عملیات جمع انجام می‌دهد.

✔ اگر یکی از Operandها String باشد، نتیجه معمولاً Concatenation خواهد بود.

---

❌ استفاده بیش از حد از Escape Character.

✔ در بسیاری از موارد، Template Literal راه‌حل خواناتری ارائه می‌دهد.

---

❌ نوشتن رشته‌های طولانی با تعداد زیادی `+`

✔ این روش خوانایی کد را کاهش می‌دهد و نگهداری آن را دشوار می‌کند.

---

# نکات مهم

- Escape Character با استفاده از بک‌اسلش (`\`) ایجاد می‌شود.
- `\n` برای رفتن به خط بعد استفاده می‌شود.
- `\t` فاصله Tab ایجاد می‌کند.
- `\\` یک بک‌اسلش واقعی تولید می‌کند.
- String Concatenation با استفاده از عملگر `+` انجام می‌شود.
- Concatenation در پروژه‌های مدرن جای خود را تا حد زیادی به Template Literal داده است.

---
---

# Template Literals

در بلوک قبل دیدیم که برای ترکیب چند رشته معمولاً از عملگر `+` استفاده می‌شود.

اگرچه این روش سال‌ها استاندارد JavaScript بود، اما مشکلاتی مانند کاهش خوانایی، مدیریت دشوار فاصله‌ها و سخت شدن نگهداری کد را به همراه داشت.

در سال ۲۰۱5 و هم‌زمان با معرفی **ECMAScript 6 (ES6)**، ویژگی جدیدی به JavaScript اضافه شد که امروزه تقریباً در تمام پروژه‌های مدرن استفاده می‌شود.

این ویژگی **Template Literals** نام دارد.

---

# Template Literal چیست؟

Template Literal روشی جدید برای ایجاد رشته‌ها است که امکانات بسیار بیشتری نسبت به String Literalهای معمولی در اختیار برنامه‌نویس قرار می‌دهد.

برخلاف رشته‌های معمولی که با Single Quote یا Double Quote ایجاد می‌شوند، Template Literal با استفاده از **Backtick** نوشته می‌شود.

```javascript
`Hello JavaScript`
```

---

## تعریف ساده

Template Literal نوعی رشته است که امکان قرار دادن Expressionها و نوشتن متن چندخطی را فراهم می‌کند.

---

## تعریف فنی

Template Literal نوعی String Literal است که با استفاده از کاراکتر Backtick (`) ایجاد می‌شود و از قابلیت‌هایی مانند **Expression Interpolation** و **Multiline Strings** پشتیبانی می‌کند.

---

# Backtick چیست؟

Backtick کاراکتری است که معمولاً در سمت چپ عدد 1 روی صفحه‌کلید قرار دارد.

نماد آن:

```text
`
```

است.

برای ایجاد Template Literal باید از همین کاراکتر استفاده کنیم.

```javascript
const language = `JavaScript`;
```

خروجی:

```text
JavaScript
```

از نظر نوع داده نیز تفاوتی با رشته‌های معمولی ندارد.

```javascript
typeof `JavaScript`;
```

خروجی:

```text
string
```

---

# چرا Template Literal معرفی شد؟

هدف اصلی از معرفی Template Literal حل سه مشکل بزرگ بود.

- خوانایی پایین String Concatenation
- مدیریت دشوار رشته‌های چندخطی
- قرار دادن متغیرها و Expressionها داخل متن

---

# Expression Interpolation

مهم‌ترین قابلیت Template Literal، **Interpolation** است.

Interpolation یعنی قرار دادن یک **Expression** داخل رشته.

برای این کار از ساختار زیر استفاده می‌شود.

```javascript
${expression}
```

JavaScript ابتدا Expression را محاسبه می‌کند و سپس نتیجه را داخل رشته قرار می‌دهد.

---

## مثال

```javascript
const firstName = 'Omid';

console.log(`Hello ${firstName}`);
```

خروجی:

```text
Hello Omid
```

در این مثال:

```javascript
firstName
```

یک Expression محسوب می‌شود.

---

# هر Expression مجاز است

داخل `${}` فقط متغیر قرار نمی‌گیرد.

هر Expression معتبر JavaScript قابل استفاده است.

برای مثال:

```javascript
console.log(`2 + 3 = ${2 + 3}`);
```

خروجی:

```text
2 + 3 = 5
```

یا:

```javascript
const birthYear = 1995;

console.log(`Age: ${2030 - birthYear}`);
```

خروجی:

```text
Age: 35
```

---

# Template Literal چگونه کار می‌کند؟

هنگامی که موتور JavaScript به عبارت زیر می‌رسد:

```javascript
`Age: ${2030 - birthYear}`
```

ابتدا Expression را محاسبه می‌کند.

```javascript
2030 - birthYear
```

فرض کنید نتیجه:

```text
35
```

باشد.

سپس رشته نهایی را ایجاد می‌کند.

```text
Age: 35
```

به همین دلیل گفته می‌شود:

> Template Literal یک رشته پویا (Dynamic String) تولید می‌کند.

---

# مزیت نسبت به Concatenation

روش قدیمی:

```javascript
const message =
  'My name is ' +
  firstName +
  ' and I am ' +
  age +
  ' years old.';
```

روش جدید:

```javascript
const message =
  `My name is ${firstName} and I am ${age} years old.`;
```

نسخه دوم:

- کوتاه‌تر است.
- خواناتر است.
- نگهداری آن بسیار ساده‌تر است.

---

# Multiline Strings

یکی دیگر از قابلیت‌های مهم Template Literal، پشتیبانی از متن چندخطی است.

در رشته‌های معمولی مجبور بودیم از:

```javascript
\n
```

استفاده کنیم.

اما در Template Literal کافی است Enter بزنیم.

---

## مثال

```javascript
const html = `
<header>
  <h1>JavaScript</h1>
</header>
`;
```

خروجی دقیقاً همان ساختار چندخطی را حفظ می‌کند.

این قابلیت باعث شده Template Literal برای تولید HTML، Email و JSON بسیار مناسب باشد.

---

# آیا هنوز به Escape Character نیاز داریم؟

در بسیاری از موارد خیر.

برای مثال:

به جای:

```javascript
'Line 1\nLine 2'
```

می‌توان نوشت:

```javascript
`
Line 1
Line 2
`
```

کد خواناتر و طبیعی‌تر خواهد بود.

البته Escape Character همچنان برای برخی کاراکترهای خاص کاربرد دارد.

---

# Jonas Perspective

Jonas Schmedtmann تقریباً در تمام پروژه‌های مدرن خود از Template Literal استفاده می‌کند.

او معتقد است:

> Template Literal یکی از بهترین قابلیت‌های ES6 است، زیرا هم خوانایی کد را افزایش می‌دهد و هم احتمال بروز خطا را کاهش می‌دهد.

به همین دلیل، در تمام مثال‌های دوره، تقریباً هیچ String Concatenation طولانی مشاهده نمی‌شود.

---

# Dynamic Text

در Applicationهای واقعی، متن معمولاً ثابت نیست. بخشی از آن از داده‌های کاربر، API، وضعیت برنامه یا نتیجه یک محاسبه به دست می‌آید.

برای مثال، در یک فروشگاه اینترنتی ممکن است پیام زیر بر اساس داده‌های واقعی ساخته شود:

```javascript
const userName = 'Omid';
const cartCount = 3;

const message = `Hello ${userName}, you have ${cartCount} items in your cart.`;
```

در اینجا ساختار متن ثابت است، اما مقدار `userName` و `cartCount` در زمان اجرای برنامه تعیین می‌شود. این همان جایی است که **Interpolation** Template Literal را برای ساخت UI Text، پیام‌های وضعیت و خروجی‌های Dynamic بسیار مناسب می‌کند.

نکته مهم این است که Template Literal ابزار تولید متن است، نه جایگزینی برای منطق برنامه. اگر محاسبات یا منطق پیچیده‌ای برای تولید یک مقدار وجود دارد، بهتر است آن منطق خارج از رشته و در یک Expression یا Function مناسب قرار گیرد.

---

# Best Practices

برای استفاده حرفه‌ای از String و Template Literal بهتر است چند اصل ساده را رعایت کنیم.

- برای متن‌های ساده از یک Coding Style ثابت در Quoteها استفاده کنید.
- برای متن‌هایی که شامل چند Value یا Expression هستند، معمولاً Template Literal را به Concatenation ترجیح دهید.
- برای تبدیل صریح یک Value به String، در صورت نیاز از `String()` استفاده کنید تا Intent کد روشن باشد.
- از Concatenationهای طولانی با `+` که خوانایی را کاهش می‌دهند، پرهیز کنید.
- برای متن‌های چندخطی از Template Literal استفاده کنید، مگر اینکه Escape Sequence برای سناریوی خاصی مناسب‌تر باشد.
- از قرار دادن منطق پیچیده داخل `${}` پرهیز کنید و Expression را تا حد امکان ساده نگه دارید.
- هنگام تولید متن از داده‌های خارجی، صرفاً به String بودن داده اکتفا نکنید و الزامات امنیتی و Context مربوط به خروجی را نیز در نظر بگیرید.

این اصول باعث می‌شوند Stringها فقط قابل اجرا نباشند، بلکه خوانا، قابل نگهداری و مناسب Applicationهای واقعی باشند.

---

# Tagged Templates (Introduction)

Template Literal قابلیت پیشرفته‌تری نیز دارد که **Tagged Template** نامیده می‌شود.

در این حالت، Template Literal مستقیماً به یک تابع ارسال می‌شود.

برای مثال:

```javascript
tag`Hello ${name}`
```

این قابلیت در کتابخانه‌هایی مانند:

- styled-components
- lit-html

کاربرد فراوانی دارد.

از آنجا که هنوز با Functionها آشنا نشده‌ایم، بررسی کامل Tagged Template را به فصل توابع موکول می‌کنیم.

در این مرحله تنها کافی است بدانیم که Template Literal صرفاً یک روش جدید برای نوشتن رشته‌ها نیست، بلکه قابلیت‌های پیشرفته‌تری نیز در اختیار JavaScript قرار می‌دهد.

---

# اشتباهات رایج

❌ تصور اینکه داخل `${}` فقط متغیر قرار می‌گیرد.

✔ هر Expression معتبر JavaScript قابل استفاده است.

---

❌ استفاده از Concatenation برای تولید متن‌های طولانی.

✔ در پروژه‌های مدرن از Template Literal استفاده می‌شود.

---

❌ تصور اینکه Template Literal نوع داده جدیدی ایجاد می‌کند.

✔ خروجی آن همچنان از نوع:

```text
string
```

است.

---

# نکات مهم

- Template Literal با Backtick ایجاد می‌شود.
- مهم‌ترین قابلیت آن Interpolation است.
- داخل `${}` هر Expression معتبر قابل استفاده است.
- Template Literal از متن چندخطی پشتیبانی می‌کند.
- امروزه Template Literal جایگزین اصلی String Concatenation محسوب می‌شود.
- Tagged Template یکی از قابلیت‌های پیشرفته Template Literal است که بعداً بررسی خواهد شد.

---
---

# خلاصه فصل

در این فصل با یکی از پرکاربردترین انواع داده در JavaScript یعنی **String** آشنا شدیم.

ابتدا دیدیم که رشته‌ها برای نگهداری داده‌های متنی استفاده می‌شوند و هر رشته مجموعه‌ای از صفر یا چند **Character** است.

سپس مفهوم **String Literal** را بررسی کردیم و آموختیم که JavaScript دو روش اصلی برای ایجاد رشته‌ها، یعنی **Single Quote** و **Double Quote** را در اختیار ما قرار می‌دهد.

در ادامه با **Escape Character** آشنا شدیم و دیدیم که چگونه می‌توان کاراکترهای ویژه، نقل‌قول‌ها، بک‌اسلش و خطوط جدید را داخل رشته‌ها قرار داد.

سپس روش سنتی **String Concatenation** را بررسی کردیم و محدودیت‌های آن را شناختیم.

در نهایت با **Template Literals** آشنا شدیم؛ قابلیتی که یکی از مهم‌ترین ویژگی‌های ES6 محسوب می‌شود و امروزه تقریباً در تمام پروژه‌های حرفه‌ای JavaScript مورد استفاده قرار می‌گیرد.

همچنین یاد گرفتیم که چگونه با استفاده از **Interpolation**، مقادیر و Expressionها را به‌صورت مستقیم داخل متن قرار دهیم و بدون استفاده از Escape Character رشته‌های چندخطی تولید کنیم.

---

# Key Takeaways

در پایان این فصل باید بتوانید:

- تفاوت Text Data و String را توضیح دهید.
- مفهوم Character را بیان کنید.
- String Literal را تعریف کنید.
- تفاوت Single Quote و Double Quote را توضیح دهید.
- کاربرد Escape Character را بدانید.
- مهم‌ترین Escape Sequenceهای JavaScript را بشناسید.
- String Concatenation را پیاده‌سازی کنید.
- محدودیت‌های Concatenation را تحلیل کنید.
- Template Literal را ایجاد کنید.
- از `${}` برای Interpolation استفاده کنید.
- متن‌های چندخطی را با Template Literal تولید کنید.
- تفاوت Template Literal و Concatenation را در پروژه‌های واقعی توضیح دهید.
- مفهوم String Conversion و کاربرد `String()` را توضیح دهید.
- الگوهای رایج ساخت Dynamic Text را با Template Literal به کار ببرید.
- اصول Best Practice برای ساخت و نگهداری متن در Applicationهای واقعی را تشخیص دهید.

---

# Technical Interview

## سطح پایه (Junior)

### سؤال ۱

String چیست؟

---

### سؤال ۲

Character چیست؟

---

### سؤال ۳

String Literal چیست؟

---

### سؤال ۴

تفاوت Single Quote و Double Quote چیست؟

---

### سؤال ۵

Escape Character چیست؟

---

### سؤال ۶

کاربرد `\n` چیست؟

---

### سؤال ۷

Template Literal چیست؟

---

### سؤال ۸

Interpolation چیست؟

---

## سطح متوسط (Mid-Level)

### سؤال ۹

چرا Template Literal نسبت به String Concatenation برتری دارد؟

---

### سؤال ۱۰

داخل `${}` چه چیزی می‌توان قرار داد؟

---

### سؤال ۱۱

چرا Template Literal خوانایی کد را افزایش می‌دهد؟

---

### سؤال ۱۲

چه زمانی همچنان به Escape Character نیاز خواهیم داشت؟

---

### سؤال ۱۳

چرا Template Literal یکی از مهم‌ترین قابلیت‌های ES6 محسوب می‌شود؟

---

## سطح متوسط (Mid-Level) — تکمیلی

### سؤال ۱۴

چرا ممکن است یک Value را به‌صورت صریح با `String()` تبدیل کنیم؟

---

### سؤال ۱۵

تفاوت String Conversion صریح با تبدیل ضمنی در زمان ساخت یک متن چیست؟

---

## سطح پیشرفته (Senior)

### سؤال ۱۶

آیا Template Literal نوع داده جدیدی ایجاد می‌کند؟

---

### سؤال ۱۷

Tagged Template چیست و چه کاربردی دارد؟

---

### سؤال ۱۸

چرا کتابخانه‌هایی مانند **styled-components** بر پایه Tagged Template طراحی شده‌اند؟

---

# Golden Answers

## String چیست؟

String یکی از Primitive Data Typeهای JavaScript است که برای ذخیره و پردازش داده‌های متنی استفاده می‌شود.

---

## Character چیست؟

کوچک‌ترین واحد تشکیل‌دهنده یک رشته که می‌تواند حرف، عدد، فاصله یا هر نماد دیگری باشد.

---

## Escape Character چیست؟

Escape Character با استفاده از بک‌اسلش (`\`) ایجاد می‌شود و به JavaScript اعلام می‌کند که کاراکتر بعدی را به‌عنوان بخشی از متن تفسیر کند.

---

## Template Literal چیست؟

Template Literal نوعی String Literal است که با استفاده از Backtick ایجاد می‌شود و از قابلیت‌هایی مانند Interpolation و Multiline String پشتیبانی می‌کند.

---

## Interpolation چیست؟

Interpolation فرآیندی است که در آن نتیجه یک Expression با استفاده از `${}` داخل یک رشته قرار می‌گیرد.

---

### String Conversion چیست؟

String Conversion تبدیل یک Value به یک String است. برای تبدیل صریح می‌توان از `String(value)` استفاده کرد و جزئیات سایر قواعد تبدیل در فصل **Type Conversion and Coercion** بررسی خواهد شد.

---

## چرا Template Literal برای Dynamic Text مناسب است؟

زیرا می‌تواند متن ثابت را با Valueها و Expressionهای محاسبه‌شده در یک ساختار خوانا ترکیب کند و نیاز به Concatenationهای طولانی را کاهش دهد.

---

## Best Practice مهم در ساخت متن چیست؟

برای متن‌های ساده از یک Style ثابت استفاده کنید و برای متن‌های Dynamic و چندبخشی معمولاً Template Literal را به Concatenation ترجیح دهید. منطق پیچیده را نیز بهتر است خارج از Template Literal نگه دارید.

---

# پاسخ کوتاه طلایی مصاحبه

### سؤال

چرا امروزه Template Literal جایگزین String Concatenation شده است؟

### پاسخ

زیرا Template Literal خوانایی کد را افزایش می‌دهد، از متن‌های چندخطی پشتیبانی می‌کند و امکان قرار دادن مستقیم Expressionها را بدون استفاده از عملگر `+` فراهم می‌سازد.

---

# گفت‌وگوی فنی

## مدیر فنی

Template Literal دقیقاً چه مشکلی را حل کرد؟

---

## داوطلب

مهم‌ترین مشکل، کاهش خوانایی String Concatenation بود.

در پروژه‌های بزرگ، استفاده مکرر از `+` باعث می‌شد کدها طولانی، شلوغ و مستعد خطا شوند.

---

## مدیر فنی

آیا Template Literal فقط یک روش جدید برای نوشتن String است؟

---

## داوطلب

خیر.

Template Literal علاوه بر تولید رشته، امکاناتی مانند:

- Interpolation
- Multiline String
- Tagged Template

را نیز فراهم می‌کند.

---

## مدیر فنی

داخل `${}` چه چیزی قرار می‌گیرد؟

---

## داوطلب

هر **Expression** معتبر JavaScript.

نه فقط متغیر.

برای مثال:

```javascript
`${2 + 3}`
```

کاملاً معتبر است.

---

## مدیر فنی

آیا خروجی Template Literal با String معمولی تفاوت دارد؟

---

## داوطلب

خیر.

هر دو از نوع:

```text
string
```

هستند.

Template Literal تنها روش تولید رشته را تغییر داده است، نه نوع داده را.

---

## مدیر فنی

چرا Jonas تقریباً همیشه از Template Literal استفاده می‌کند؟

---

## داوطلب

زیرا خوانایی کد را افزایش می‌دهد، نگهداری آن را ساده‌تر می‌کند و احتمال خطاهای ناشی از Concatenation را کاهش می‌دهد.

---

# اشتباهات رایج

❌ تصور اینکه Template Literal نوع داده جدیدی ایجاد می‌کند.

✔ خروجی آن همچنان یک String است.

---

❌ تصور اینکه داخل `${}` فقط متغیر قرار می‌گیرد.

✔ هر Expression معتبر JavaScript قابل استفاده است.

---

❌ استفاده از Concatenation برای تولید رشته‌های طولانی.

✔ در پروژه‌های مدرن Template Literal استاندارد اصلی است.

---

❌ استفاده بی‌دلیل از Escape Character برای متن‌های چندخطی.

✔ Template Literal این مشکل را به‌صورت طبیعی حل کرده است.

---

# جمع‌بندی فصل

پردازش داده‌های متنی یکی از رایج‌ترین فعالیت‌ها در توسعه نرم‌افزار است و به همین دلیل، آشنایی عمیق با Stringها برای هر برنامه‌نویس JavaScript ضروری است.

در این فصل دیدیم که چگونه JavaScript رشته‌ها را ایجاد و مدیریت می‌کند، چگونه کاراکترهای ویژه را با Escape Character کنترل می‌کند و چرا Template Literal به استاندارد اصلی تولید متن در JavaScript مدرن تبدیل شده است.

از این فصل به بعد، تقریباً در تمام مثال‌های کتاب از Template Literal برای تولید پیام‌ها، ساخت HTML و نمایش اطلاعات استفاده خواهیم کرد.

در فصل بعد، یاد خواهیم گرفت که چگونه JavaScript با استفاده از ساختارهای شرطی مسیر اجرای برنامه را تغییر می‌دهد و بر اساس شرایط مختلف تصمیم‌گیری می‌کند.

---