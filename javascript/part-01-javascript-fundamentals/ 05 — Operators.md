# Chapter 05 — Operators and Expressions

# اهداف فصل

پس از پایان این فصل انتظار می‌رود بتوانید:

* مفهوم **Expression**، **Operand** و **Operator** را توضیح دهید.
* تفاوت **Expression** و **Statement** را تشخیص دهید.
* عملگرهای حسابی را در Expressionهای ساده و واقعی به‌کار ببرید.
* تفاوت **Assignment** و **Comparison** را توضیح دهید.
* تفاوت `==` و `===` و نقش **Type Coercion** در آن‌ها را درک کنید.
* عملگرهای منطقی `&&`، `||` و `!` را در سطح موردنیاز این فصل به‌کار ببرید.
* مفهوم **Unary Operator** و نمونه‌هایی مانند `typeof` را درک کنید.
* **Ternary Operator** را به‌عنوان یک Expression شرطی استفاده کنید.
* مفهوم **Operator Precedence** را درک کرده و در صورت نیاز با Parentheses ترتیب ارزیابی را صریح کنید.
* برای یک مسئله مشخص، Operator مناسب را با توجه به خوانایی و هدف کد انتخاب کنید.

---

# مقدمه

در فصل‌های قبل با **Value**، **Variable** و **Data Type** آشنا شدیم.

اکنون می‌دانیم داده چیست و چگونه می‌توانیم آن را در یک Variable نگهداری کنیم.

اما یک برنامه فقط داده‌ها را نگهداری نمی‌کند.

برنامه باید بتواند روی داده‌ها عملیات انجام دهد:

* قیمت چند محصول را محاسبه کند.
* موجودی حساب را تغییر دهد.
* سن کاربر را بررسی کند.
* دو مقدار را مقایسه کند.
* چند شرط را با یکدیگر ترکیب کند.
* بر اساس یک شرط، یکی از دو مقدار را انتخاب کند.

این عملیات با استفاده از **Operators** انجام می‌شوند.

اما برای درک Operatorها نباید از خود Syntax شروع کنیم.

ابتدا باید بدانیم یک Operation در JavaScript از چه اجزایی تشکیل شده است.

جریان مفهومی این فصل چنین است:

```text
Expression
↓
Operand
↓
Operator
↓
Arithmetic
↓
Assignment
↓
Comparison
↓
Logical
↓
Unary
↓
Ternary
↓
Precedence
```

---

# Block 01 — Expressions and Arithmetic

# Expression چیست؟

## تعریف ساده

**Expression** قطعه‌ای از کد است که هنگام ارزیابی، یک Value تولید می‌کند.

برای مثال:

```javascript
10 + 5
```

این Expression در نهایت مقدار:

```text
15
```

را تولید می‌کند.

یک Value ساده نیز می‌تواند Expression باشد:

```javascript
42
```

یا:

```javascript
'JavaScript'
```

یا:

```javascript
true
```

---

## تعریف فنی

Expression ساختاری از JavaScript است که می‌تواند ارزیابی شود و یک مقدار نتیجه تولید کند.

Expression می‌تواند از یک Value ساده، Variable، Operator یا ترکیبی از چند Expression تشکیل شود.

برای مثال:

```javascript
price * quantity
```

یک Expression است.

در اینجا ابتدا مقدار `price` و `quantity` مشخص می‌شود و سپس نتیجه Operation تولید می‌شود.

---

## چرا Expression مهم است؟

بخش بزرگی از JavaScript بر پایه Expressionها ساخته شده است.

وقتی می‌نویسیم:

```javascript
const total = price * quantity;
```

سمت راست Assignment یک Expression است.

وقتی می‌نویسیم:

```javascript
age >= 18
```

نیز با یک Expression روبه‌رو هستیم.

بنابراین Expression یکی از واحدهای اصلی تشکیل‌دهنده Code در JavaScript است.

---

## مثال

فرض کنید یک فروشگاه قیمت و تعداد یک محصول را در اختیار دارد:

```javascript
const price = 25;
const quantity = 3;

const total = price * quantity;
```

Expression:

```javascript
price * quantity
```

مقدار:

```text
75
```

را تولید می‌کند.

---

## تحلیل مهندسی

در این مثال، Operator به‌تنهایی مسئله را حل نمی‌کند.

سه جزء با یکدیگر همکاری می‌کنند:

```text
price
   ↓
Operand

*
   ↓
Operator

quantity
   ↓
Operand
```

نتیجه این Operation نیز یک Value جدید است.

این مدل ذهنی پایه فهم سایر Operatorها خواهد بود.

---

## اشتباهات رایج

### اشتباه

فکر کنیم فقط عباراتی که شامل Operator هستند Expression محسوب می‌شوند.

### صحیح

یک Value ساده نیز می‌تواند Expression باشد:

```javascript
42
```

---

## نکات مهم

* Expression هنگام ارزیابی یک Value تولید می‌کند.
* Expression می‌تواند ساده یا ترکیبی باشد.
* بسیاری از بخش‌های JavaScript از Expressionها تشکیل می‌شوند.

---

## پاسخ کوتاه طلایی مصاحبه

**Expression چیست؟**

Expression ساختاری از JavaScript است که هنگام ارزیابی یک Value تولید می‌کند.

---

# Operand چیست؟

## تعریف ساده

**Operand** مقداری است که Operator روی آن عملیات انجام می‌دهد.

در مثال:

```javascript
10 + 5
```

دو Operand داریم:

```text
10
5
```

و Operator:

```text
+
```

است.

---

## تعریف فنی

Operand بخشی از Expression است که توسط یک Operator مورد استفاده یا پردازش قرار می‌گیرد.

Operand می‌تواند یک Literal، Variable یا Expression دیگری باشد.

برای مثال:

```javascript
price * quantity
```

هر دو Variable در این Expression Operand هستند.

---

## چرا Operand مهم است؟

برای فهم رفتار Operator باید بدانیم Operation روی چه داده‌ای انجام می‌شود.

مثلاً:

```javascript
price * quantity
```

با:

```javascript
price + quantity
```

از یک Operator متفاوت استفاده می‌کند، اما Operandها همان داده‌های اصلی هستند.

---

## مثال

```javascript
const price = 100;
const discount = 20;

price - discount;
```

در این Expression:

* `price` یک Operand است.
* `discount` یک Operand است.
* `-` یک Operator است.

---

## تحلیل مهندسی

Operator و Operand را باید به‌صورت یک واحد مفهومی ببینیم:

```text
Operand → Operator → Operand
```

این مدل بعداً برای Comparison و Logical Operators نیز کاربرد خواهد داشت.

---

## اشتباهات رایج

Operand را فقط عدد در نظر نگیرید.

در:

```javascript
price * quantity
```

متغیرها نیز Operand هستند.

---

## نکات مهم

* Operand داده‌ای است که Operator روی آن عمل می‌کند.
* Operand می‌تواند Variable یا Expression باشد.
* تعداد Operandها به نوع Operator بستگی دارد.

---

## پاسخ کوتاه طلایی مصاحبه

**Operand چیست؟**

Operand مقداری یا Expressionای است که یک Operator روی آن عملیات انجام می‌دهد.

---

# Operator چیست؟

## تعریف ساده

**Operator** نماد یا کلمه‌ای است که یک Operation مشخص را روی یک یا چند Operand انجام می‌دهد.

مثلاً:

```javascript
price * quantity
```

علامت `*` مشخص می‌کند که باید دو Operand با یکدیگر ضرب شوند.

---

## تعریف فنی

Operator بخشی از Syntax زبان است که نوع و رفتار یک Operation را مشخص می‌کند.

Operatorها می‌توانند تعداد Operandهای متفاوتی داشته باشند.

### Binary Operator

دو Operand دارد:

```javascript
price * quantity
```

### Unary Operator

یک Operand دارد:

```javascript
typeof price
```

### Conditional Operator

سه بخش دارد:

```javascript
age >= 18 ? 'Adult' : 'Minor'
```

در ادامه فصل این تفاوت‌ها را بررسی خواهیم کرد.

---

## چرا Operator مهم است؟

Operatorها روش اصلی انجام Operation روی Values هستند.

بدون آن‌ها نمی‌توانیم به شکل مستقیم:

* محاسبه کنیم.
* مقدار اختصاص دهیم.
* مقایسه کنیم.
* شروط را ترکیب کنیم.
* مقدار مناسب را انتخاب کنیم.

---

## مثال

```javascript
const subtotal = price * quantity;
const isAvailable = stock > 0;
```

در Expression اول، `*` یک Arithmetic Operator است.

در Expression دوم، `>` یک Comparison Operator است.

---

## تحلیل مهندسی

Operator فقط یک علامت ظاهری نیست.

هر Operator بخشی از قواعد زبان JavaScript را فعال می‌کند.

بنابراین برای استفاده حرفه‌ای از Operatorها باید رفتار آن‌ها را بشناسیم، نه اینکه فقط Syntax آن‌ها را حفظ کنیم.

---

## اشتباهات رایج

همه Operatorها را Mathematical Operator در نظر نگیرید.

برای مثال:

```javascript
=
```

برای Assignment استفاده می‌شود.

و:

```javascript
===
```

برای Comparison.

---

## نکات مهم

* Operator نوع Operation را مشخص می‌کند.
* Operator می‌تواند Unary، Binary یا Conditional باشد.
* رفتار هر Operator توسط قواعد JavaScript تعریف می‌شود.

---

## پاسخ کوتاه طلایی مصاحبه

**Operator چیست؟**

Operator بخشی از Syntax JavaScript است که یک Operation مشخص را روی یک یا چند Operand انجام می‌دهد.

---

# Arithmetic Operators

## تعریف ساده

Arithmetic Operators برای انجام عملیات محاسباتی استفاده می‌شوند.

مهم‌ترین آن‌ها:

| Operator | عملیات     |
| -------- | ---------- |
| `+`      | جمع        |
| `-`      | تفریق      |
| `*`      | ضرب        |
| `/`      | تقسیم      |
| `%`      | باقی‌مانده |
| `**`     | توان       |

---

## چرا Arithmetic Operators مهم هستند؟

بسیاری از محاسبات Application مستقیماً به عملیات عددی وابسته‌اند.

برای مثال:

```javascript
const total = price * quantity;
```

یا:

```javascript
const remaining = stock - sold;
```

---

## مثال

```javascript
const price = 40;
const quantity = 3;

const subtotal = price * quantity;
const average = subtotal / quantity;
const remainder = subtotal % 7;
```

---

## تحلیل مهندسی

هر Arithmetic Operator یک Operation متفاوت را بیان می‌کند.

```javascript
price * quantity
```

برای محاسبه مجموع قیمت مناسب است.

اما:

```javascript
price + quantity
```

از نظر Syntax صحیح است، ولی از نظر مدل مسئله ممکن است معنای درستی نداشته باشد.

بنابراین انتخاب Operator بخشی از **Problem Modeling** است.

---

# عملگر باقی‌مانده `%`

## تعریف ساده

Operator `%` باقی‌مانده تقسیم دو عدد را تولید می‌کند.

مثلاً:

```javascript
10 % 3
```

نتیجه:

```text
1
```

است.

---

## چرا این مفهوم مهم است؟

در برنامه‌های واقعی می‌توان از `%` برای الگوهایی مانند:

* تشخیص زوج یا فرد بودن عدد
* چرخه‌های تکرارشونده
* تقسیم‌بندی داده‌ها
* محاسبات دوره‌ای

استفاده کرد.

---

## مثال

```javascript
const orderCount = 7;

const isEven = orderCount % 2 === 0;
```

در اینجا نتیجه `isEven` مشخص می‌کند تعداد سفارش‌ها زوج است یا خیر.

---

## تحلیل مهندسی

`%` صرفاً یک Operator ریاضی نیست.

گاهی یک Operation ریاضی مستقیماً به یک مفهوم منطقی در Application تبدیل می‌شود.

---

## اشتباهات رایج

`%` را با درصد اشتباه نگیرید.

در JavaScript:

```javascript
10 % 3
```

به معنای «۱۰ درصد ۳» نیست.

---

## نکات مهم

* `%` باقی‌مانده تقسیم را تولید می‌کند.
* برای تشخیص زوج و فرد کاربرد زیادی دارد.
* می‌تواند در ساخت الگوهای تکرارشونده استفاده شود.

---

## پاسخ کوتاه طلایی مصاحبه

**کاربرد `%` چیست؟**

`%` باقی‌مانده تقسیم را تولید می‌کند و علاوه بر محاسبات، در الگوریتم‌هایی مانند تشخیص زوج و فرد و الگوهای دوره‌ای کاربرد دارد.

---

# Block 02 — Assignment and Comparison

# Assignment چیست؟

## تعریف ساده

Assignment یعنی **اختصاص دادن نتیجه یک Expression به یک Binding**.

مثلاً:

```javascript
let total;

total = 250;
```

در اینجا مقدار `250` به Binding مربوط به `total` اختصاص داده می‌شود.

---

## تعریف فنی

Assignment یک Operation است که مقدار حاصل از ارزیابی سمت راست را به هدف قابل انتساب در سمت چپ اختصاص می‌دهد.

ساده‌ترین Assignment Operator:

```javascript
=
```

است.

---

## چرا Assignment مهم است؟

تا اینجا Expressionها می‌توانستند Value تولید کنند.

اما Application باید بتواند State موجود در Variableها را نیز تغییر دهد.

مثلاً:

```javascript
let balance = 100;

balance = 150;
```

مقدار جدید برای `balance` ثبت می‌شود.

---

## مثال

```javascript
let cartTotal = 120;

cartTotal = 150;
```

بعد از Assignment:

```text
cartTotal → 150
```

---

## تحلیل مهندسی

مدل ذهنی مناسب برای Assignment این است:

```text
Evaluate Right Side
        ↓
Obtain Result
        ↓
Assign Result to Left Side
```

بنابراین:

```javascript
x = x + 1;
```

ابتدا سمت راست ارزیابی می‌شود:

```text
x + 1
```

سپس نتیجه در `x` قرار می‌گیرد.

---

## اشتباهات رایج

### اشتباه

فکر کنیم:

```javascript
x = x + 1;
```

یک معادله ریاضی است.

### صحیح

در JavaScript این یک Assignment است.

---

### اشتباه

فکر کنیم `=` عملگر مقایسه است.

### صحیح

`=` برای Assignment استفاده می‌شود.

---

## نکات مهم

* `=` برای Assignment است.
* سمت راست ابتدا ارزیابی می‌شود.
* نتیجه سپس به هدف سمت چپ اختصاص داده می‌شود.

---

## پاسخ کوتاه طلایی مصاحبه

**Assignment چیست؟**

Assignment نتیجه یک Expression را به یک Binding قابل انتساب اختصاص می‌دهد. عملگر پایه آن `=` است.

---

# Compound Assignment Operators

## تعریف ساده

JavaScript برای برخی Assignmentهای رایج Syntax کوتاه‌تری ارائه می‌کند.

برای مثال:

```javascript
score = score + 10;
```

را می‌توان به شکل زیر نوشت:

```javascript
score += 10;
```

---

## مهم‌ترین موارد

| Operator | معادل مفهومی     |
| -------- | ---------------- |
| `+=`     | `x = x + value`  |
| `-=`     | `x = x - value`  |
| `*=`     | `x = x * value`  |
| `/=`     | `x = x / value`  |
| `%=`     | `x = x % value`  |
| `**=`    | `x = x ** value` |

---

## مثال

```javascript
let balance = 100;

balance += 50;
balance -= 20;
```

در پایان:

```text
130
```

داریم.

---

## تحلیل مهندسی

Compound Assignment زمانی مفید است که هدف ما تغییر مقدار موجود باشد.

مثلاً:

```javascript
cartTotal += itemPrice;
```

از نظر Intent بسیار واضح است:

> قیمت این Item را به مجموع سبد اضافه کن.

---

## اشتباهات رایج

Compound Assignment را با Comparison اشتباه نگیرید.

```javascript
score += 10;
```

مقدار را تغییر می‌دهد.

اما:

```javascript
score >= 10
```

فقط یک Comparison انجام می‌دهد.

---

## نکات مهم

* Compound Assignment شکل کوتاه برخی Assignmentها است.
* این Syntax معمولاً Intent کد را واضح‌تر می‌کند.
* `=` همچنان Operator اصلی Assignment است.

---

## پاسخ کوتاه طلایی مصاحبه

**Compound Assignment چیست؟**

Syntax کوتاه‌تری برای ترکیب یک Operation با Assignment است؛ مانند `+=` به‌جای `x = x + value`.

---

# Increment و Decrement

JavaScript دو Update Operator دارد:

```javascript
++
--
```

این Operatorها مقدار یک Variable را به‌ترتیب یک واحد افزایش یا کاهش می‌دهند.

---

## مثال

```javascript
let page = 1;

page++;
```

اکنون:

```text
page → 2
```

است.

---

## نکته فنی مهم

`++` و `--` را نباید **Assignment Operator** نامید.

این دو در JavaScript در دسته **Update Expressions** قرار می‌گیرند.

---

## مثال دیگر

```javascript
let remainingItems = 5;

remainingItems--;
```

اکنون:

```text
remainingItems → 4
```

است.

---

## تحلیل مهندسی

اگر هدف فقط افزایش یک واحدی باشد، `++` بسیار فشرده است.

اما در کدهایی که خوانایی و صراحت مهم‌تر است، گاهی:

```javascript
page += 1;
```

خواناتر است.

انتخاب بین آن‌ها باید با توجه به Context و Style پروژه انجام شود.

---

## نکات مهم

* `++` یک واحد اضافه می‌کند.
* `--` یک واحد کم می‌کند.
* این دو Update Operator هستند، نه Assignment Operator.
* Prefix و Postfix نیز رفتار متفاوتی در مقدار Expression دارند و در صورت نیاز باید جداگانه بررسی شوند.

---

# Comparison Operators

## تعریف ساده

Comparison Operator رابطه میان دو Value را بررسی می‌کند.

نتیجه Comparison Operatorهای معرفی‌شده در این بخش یک Boolean Value است:

```javascript
true
```

یا:

```javascript
false
```

---

## تعریف فنی

Comparison Operator یک رابطه میان Operandها را ارزیابی کرده و یک Boolean Result تولید می‌کند.

---

## مهم‌ترین Comparison Operators

| Operator | مفهوم             |
| -------- | ----------------- |
| `>`      | بزرگ‌تر از        |
| `<`      | کوچک‌تر از        |
| `>=`     | بزرگ‌تر یا مساوی  |
| `<=`     | کوچک‌تر یا مساوی  |
| `==`     | Loose Equality    |
| `===`    | Strict Equality   |
| `!=`     | Loose Inequality  |
| `!==`    | Strict Inequality |

---

## چرا Comparison مهم است؟

Application دائماً باید درباره Values تصمیم بگیرد.

مثلاً:

```javascript
const isAvailable = stock > 0;
```

اینجا Comparison نتیجه‌ای تولید می‌کند که بعداً می‌تواند در Logic برنامه استفاده شود.

---

## مثال

```javascript
const stock = 12;

const isAvailable = stock > 0;
```

نتیجه:

```text
true
```

است.

---

## تحلیل مهندسی

Comparison یک پل مهم میان Data و Decision Making است:

```text
Value
↓
Comparison
↓
Boolean Result
↓
Decision
```

در فصل بعدی درباره Decision Making به‌صورت کامل‌تر از این Boolean Resultها استفاده خواهیم کرد.

---

## اشتباهات رایج

`=` را با `==` و `===` اشتباه نگیرید.

```javascript
=
```

برای Assignment است.

```javascript
==
```

و:

```javascript
===
```

برای Equality Comparison هستند.

---

## نکات مهم

* Comparison نتیجه Boolean تولید می‌کند.
* Comparison پایه بسیاری از تصمیم‌های برنامه است.
* `>`، `<`، `>=` و `<=` برای مقایسه رابطه‌ای استفاده می‌شوند.

---

## پاسخ کوتاه طلایی مصاحبه

**Comparison Operator چه کاری انجام می‌دهد؟**

رابطه میان دو Operand را بررسی می‌کند و یک Boolean Result تولید می‌کند.

---

# Equality

JavaScript دو نوع Equality اصلی دارد:

```javascript
==
```

و:

```javascript
===
```

تفاوت آن‌ها در نحوه مقایسه Values و Types است.

---

# Loose Equality — `==`

## تعریف ساده

`==` دو Value را با استفاده از قواعد **Loose Equality** مقایسه می‌کند.

در برخی شرایط، این مقایسه شامل **Type Coercion** نیز می‌شود.

مثلاً:

```javascript
5 == '5'
```

نتیجه:

```text
true
```

است.

---

## تعریف فنی

Loose Equality مطابق الگوریتم تعریف‌شده برای `==` مقادیر را مقایسه می‌کند و در برخی حالت‌ها پیش از مقایسه، تبدیل نوع انجام می‌شود.

---

## چرا این موضوع مهم است؟

اگر Type Coercion را در نظر نگیریم، رفتار `==` می‌تواند برای برنامه‌نویس غیرمنتظره باشد.

برای همین در Codebaseهای مدرن معمولاً استفاده از Strict Equality ترجیح داده می‌شود.

---

## مثال

```javascript
const input = '18';

input == 18;
```

در این حالت نتیجه:

```text
true
```

است.

اما:

```javascript
input === 18;
```

نتیجه:

```text
false
```

خواهد بود.

---

## تحلیل مهندسی

نکته مهم این نیست که `==` همیشه بد است.

نکته این است که `==` قواعد تبدیل بیشتری را وارد Comparison می‌کند.

در نتیجه، اگر قصد شما مقایسه بدون Type Coercion است، `===` انتخاب واضح‌تری است.

---

## اشتباهات رایج

نگویید:

> `==` فقط مقدار را مقایسه می‌کند.

این بیان ناقص است.

`==` از قواعد Loose Equality و در برخی موارد Type Coercion استفاده می‌کند.

---

## نکات مهم

* `==` Loose Equality است.
* ممکن است Type Coercion انجام شود.
* رفتار آن از `===` پیچیده‌تر است.

---

## پاسخ کوتاه طلایی مصاحبه

**`==` چیست؟**

`==` برای Loose Equality استفاده می‌شود و در برخی مقایسه‌ها Type Coercion انجام می‌دهد.

---

# Strict Equality — `===`

## تعریف ساده

`===` برای مقایسه Strict استفاده می‌شود.

برای برابر بودن دو Value، Type و Value باید با قواعد Strict Equality سازگار باشند.

مثلاً:

```javascript
5 === 5
```

نتیجه:

```text
true
```

اما:

```javascript
5 === '5'
```

نتیجه:

```text
false
```

است.

---

## تعریف فنی

Strict Equality بدون اعمال Type Coercion بین دو Operand مقایسه انجام می‌دهد.

اگر Typeهای دو Operand متفاوت باشند، نتیجه `false` خواهد بود.

---

## چرا `===` مهم است؟

استفاده از Strict Equality معمولاً رفتار مقایسه را قابل پیش‌بینی‌تر می‌کند.

به همین دلیل در بسیاری از Codebaseهای مدرن:

```javascript
===
```

به انتخاب پیش‌فرض تبدیل شده است.

---

## مثال

```javascript
const userId = 42;
const requestedId = 42;

const isSameUser = userId === requestedId;
```

این Comparison مستقیماً Intent را نشان می‌دهد:

> آیا این دو ID از نظر Strict Equality برابر هستند؟

---

## تحلیل مهندسی

Strict Equality به ما کمک می‌کند Comparison را بدون وارد کردن Type Coercion انجام دهیم.

این موضوع مخصوصاً هنگام کار با داده‌هایی که از API، Form یا Storage دریافت می‌شوند اهمیت دارد.

---

## اشتباهات رایج

`===` را به معنای «فقط مقدار برابر است» تعریف نکنید.

Strict Equality علاوه بر مقدار، Type را نیز در مقایسه در نظر می‌گیرد.

---

## نکات مهم

* `===` Type Coercion انجام نمی‌دهد.
* Type متفاوت معمولاً نتیجه `false` می‌دهد.
* برای مقایسه‌های قابل پیش‌بینی انتخاب مناسبی است.

---

## پاسخ کوتاه طلایی مصاحبه

**چرا `===` را ترجیح می‌دهید؟**

زیرا Strict Equality بدون Type Coercion مقایسه می‌کند و رفتار آن معمولاً قابل پیش‌بینی‌تر است.

---

# Inequality Operators

برای بررسی نابرابری نیز دو Operator داریم:

```javascript
!=
```

و:

```javascript
!==
```

مانند Equality:

```text
!=  → Loose Inequality
!== → Strict Inequality
```

---

## مثال

```javascript
const status = 'active';

status !== 'inactive';
```

نتیجه:

```text
true
```

است.

---

## نکات مهم

در Codebaseهای مدرن معمولاً:

```javascript
!==
```

به‌جای:

```javascript
!=
```

ترجیح داده می‌شود.

---

# Block 03 — Logical and Conditional Operators

# Logical Operators

پس از Comparison، معمولاً یک Boolean Result داریم.

مثلاً:

```javascript
stock > 0
```

اما در Applicationهای واقعی اغلب یک شرط به‌تنهایی کافی نیست.

ممکن است بخواهیم بررسی کنیم:

* موجودی وجود دارد **و** محصول فعال است.
* کاربر Admin است **یا** Owner.
* یک شرط **نیست**.

برای این کار از Logical Operators استفاده می‌کنیم.

---

# AND — `&&`

## تعریف ساده

`&&` برای ترکیب دو Logical Condition با رابطه **AND** استفاده می‌شود.

```javascript
isLoggedIn && isAdmin
```

مفهوم آن:

> کاربر هم وارد شده باشد و هم Admin باشد.

---

## مثال

```javascript
const isLoggedIn = true;
const isAdmin = true;

const canManageUsers = isLoggedIn && isAdmin;
```

نتیجه:

```text
true
```

---

## چرا مهم است؟

در Applicationهای واقعی معمولاً دسترسی یا اعتبار یک عملیات به چند شرط وابسته است.

---

## تحلیل مهندسی

مدل ذهنی پایه:

```text
Condition A
    AND
Condition B
    ↓
Result
```

---

## پاسخ کوتاه طلایی مصاحبه

**`&&` چه کاری انجام می‌دهد؟**

دو شرط منطقی را با رابطه AND ترکیب می‌کند؛ نتیجه در حالت Boolean Logic زمانی `true` است که هر دو شرط `true` باشند.

---

# OR — `||`

## تعریف ساده

`||` برای ترکیب شرط‌ها با رابطه OR استفاده می‌شود.

```javascript
isAdmin || isOwner
```

یعنی اگر کاربر Admin **یا** Owner باشد، شرط برقرار است.

---

## مثال

```javascript
const isAdmin = false;
const isOwner = true;

const canEdit = isAdmin || isOwner;
```

نتیجه:

```text
true
```

است.

---

## تحلیل مهندسی

OR زمانی مناسب است که چند مسیر مختلف بتوانند یک Requirement را برآورده کنند.

---

## پاسخ کوتاه طلایی مصاحبه

**`||` چه کاری انجام می‌دهد؟**

دو شرط را با رابطه OR ترکیب می‌کند؛ در Boolean Logic اگر حداقل یکی از آن‌ها `true` باشد، نتیجه `true` است.

---

# NOT — `!`

## تعریف ساده

`!` نتیجه منطقی یک Expression را معکوس می‌کند.

```javascript
!true
```

نتیجه:

```text
false
```

---

## مثال

```javascript
const isAuthenticated = false;

const isGuest = !isAuthenticated;
```

نتیجه:

```text
true
```

است.

---

## تحلیل مهندسی

NOT برای بیان شرط‌های معکوس مفید است.

```javascript
!isLoading
```

می‌تواند به‌صورت مفهومی یعنی:

> در حال Loading نیست.

---

## پاسخ کوتاه طلایی مصاحبه

**`!` چیست؟**

یک Unary Logical Operator است که نتیجه منطقی Expression خود را معکوس می‌کند.

---

# Unary Operators

## تعریف ساده

Unary Operator فقط با **یک Operand** کار می‌کند.

برای مثال:

```javascript
typeof value
```

یا:

```javascript
-value
```

---

## تعریف فنی

Unary Operator عملیاتی را روی یک Operand انجام می‌دهد.

این Operatorها معمولاً پیش از Operand قرار می‌گیرند.

---

# `typeof`

یکی از مهم‌ترین Unary Operatorهای JavaScript:

```javascript
typeof
```

است.

`typeof` نوع یک Expression را به‌صورت یک String گزارش می‌کند.

---

## مثال

```javascript
const price = 100;

typeof price;
```

نتیجه:

```text
"number"
```

---

## مثال دیگر

```javascript
const userName = 'Omid';

typeof userName;
```

نتیجه:

```text
"string"
```

---

## چرا `typeof` مهم است؟

در JavaScript Type Checking گاهی برای بررسی نوع یک Value به `typeof` نیاز داریم.

برای مثال:

```javascript
const value = 42;

if (typeof value === 'number') {
  // ...
}
```

---

## تحلیل مهندسی

`typeof` یک نمونه خوب برای درک تفاوت Unary و Binary است:

```text
typeof → یک Operand
```

در مقابل:

```javascript
price > 100
```

دارای دو Operand است.

---

## اشتباهات رایج

`typeof` خود Value را برنمی‌گرداند.

مثلاً:

```javascript
typeof 42
```

برمی‌گرداند:

```text
"number"
```

نه:

```text
42
```

---

## نکات مهم

* `typeof` یک Unary Operator است.
* نتیجه آن یک String است.
* برای بررسی نوع Value کاربرد دارد.

---

## پاسخ کوتاه طلایی مصاحبه

**`typeof` چیست؟**

`typeof` یک Unary Operator است که نوع یک Expression را به‌صورت String برمی‌گرداند.

---

# Unary Plus و Unary Minus

دو Unary Operator دیگر:

```javascript
+
-
```

هستند.

برای مثال:

```javascript
const temperature = -5;
```

علامت `-` در اینجا مقدار را منفی می‌کند.

همچنین Unary Plus می‌تواند در برخی شرایط مقدار را به Number تبدیل کند:

```javascript
const value = +'42';
```

نتیجه:

```text
42
```

است.

این رفتار باید با Type Conversion که در فصل 09 به‌صورت کامل بررسی می‌شود، مرتبط دانسته شود و در اینجا بیشتر از این گسترش داده نمی‌شود.

---

# Ternary Operator

## تعریف ساده

Ternary Operator یک Expression شرطی است که بر اساس یک Condition یکی از دو Expression را انتخاب می‌کند.

Syntax:

```javascript
condition ? valueIfTrue : valueIfFalse
```

---

## مثال

```javascript
const age = 20;

const status = age >= 18 ? 'adult' : 'minor';
```

اگر شرط `true` باشد:

```text
adult
```

و اگر `false` باشد:

```text
minor
```

انتخاب می‌شود.

---

## چرا Ternary مهم است؟

گاهی برای یک انتخاب ساده، نوشتن یک `if/else` کامل ضروری نیست.

Ternary می‌تواند چنین انتخابی را در قالب یک Expression بیان کند.

---

## تحلیل مهندسی

Ternary بخشی از Concept Flow ما را تکمیل می‌کند:

```text
Condition
↓
Boolean Result
↓
Choose One of Two Expressions
↓
New Value
```

به همین دلیل Ternary یک **Expression** است و می‌تواند در مکان‌هایی استفاده شود که یک Value مورد انتظار است.

---

## مثال واقعی

```javascript
const stock = 4;

const availability = stock > 0
  ? 'In stock'
  : 'Out of stock';
```

---

## اشتباهات رایج

از Ternary برای Logicهای بسیار پیچیده استفاده نکنید.

مثلاً Ternaryهای تو در تو معمولاً خوانایی را کاهش می‌دهند.

در چنین شرایطی یک ساختار واضح‌تر مانند `if/else` مناسب‌تر است.

---

## نکات مهم

* Ternary یک Operator شرطی است.
* سه بخش دارد.
* یک Expression تولید می‌کند.
* برای انتخاب ساده میان دو نتیجه مناسب است.

---

## پاسخ کوتاه طلایی مصاحبه

**Ternary Operator چیست؟**

یک Conditional Operator است که بر اساس یک Condition یکی از دو Expression را انتخاب می‌کند و خودش یک Expression محسوب می‌شود.

---

# Operator Precedence

## تعریف ساده

وقتی یک Expression چند Operator داشته باشد، JavaScript باید مشخص کند کدام Operation زودتر ارزیابی شود.

این ترتیب را **Operator Precedence** می‌نامیم.

---

## مثال

```javascript
const result = 2 + 3 * 4;
```

ضرب پیش از جمع انجام می‌شود.

بنابراین نتیجه:

```text
14
```

است.

نه:

```text
20
```

---

## چرا Precedence مهم است؟

اگر ترتیب ارزیابی را ندانیم، ممکن است نتیجه Expression را اشتباه پیش‌بینی کنیم.

---

## Parentheses

برای واضح کردن Intent می‌توان از Parentheses استفاده کرد:

```javascript
const result = (2 + 3) * 4;
```

اکنون ابتدا:

```text
2 + 3
```

محاسبه می‌شود.

نتیجه:

```text
20
```

است.

---

## تحلیل مهندسی

Parentheses فقط برای تغییر نتیجه نیستند.

آن‌ها می‌توانند **Intent برنامه‌نویس** را نیز واضح‌تر کنند.

مثلاً:

```javascript
const total = price * quantity + shippingCost;
```

ممکن است کاملاً واضح باشد.

اما در Expressionهای پیچیده‌تر، Parentheses می‌تواند فهم کد را آسان‌تر کند.

---

## اشتباهات رایج

### اشتباه

فرض کنیم همه Operatorها از چپ به راست و با یک اولویت اجرا می‌شوند.

### صحیح

Operatorهای مختلف Precedence و Associativity متفاوتی دارند.

---

## نکات مهم

* Precedence ترتیب اولویت Operatorها را مشخص می‌کند.
* Parentheses می‌تواند ترتیب ارزیابی را تغییر دهد.
* در Expressionهای پیچیده، خوانایی باید بر کوتاه بودن کد اولویت داشته باشد.

---

## پاسخ کوتاه طلایی مصاحبه

**Operator Precedence چیست؟**

مجموعه قواعدی است که مشخص می‌کند در Expressionهای دارای چند Operator، کدام Operation ابتدا ارزیابی شود.

---

# Block 04 — Professional Usage

# Choosing Operators

انتخاب Operator فقط مسئله Syntax نیست.

یک Developer باید Operator را بر اساس Intent و خوانایی انتخاب کند.

---

## `=` یا `===`؟

ابتدا باید مشخص کنیم هدف چیست.

اگر می‌خواهیم مقدار اختصاص دهیم:

```javascript
status = 'active';
```

اگر می‌خواهیم Equality را بررسی کنیم:

```javascript
status === 'active';
```

---

## `==` یا `===`؟

در حالت معمول:

```javascript
===
```

انتخاب مناسب‌تری است، زیرا Type Coercion انجام نمی‌دهد.

استفاده از `==` باید آگاهانه و بر اساس رفتار موردنیاز باشد.

---

## Ternary یا if؟

برای انتخاب ساده:

```javascript
const label = isActive ? 'Active' : 'Inactive';
```

خوانا است.

اما اگر Logic پیچیده شود، `if/else` معمولاً مناسب‌تر است.

---

## `++` یا `+= 1`؟

هر دو می‌توانند مقدار را یک واحد افزایش دهند:

```javascript
count++;
```

و:

```javascript
count += 1;
```

انتخاب باید بر اساس Context و Convention پروژه انجام شود.

هیچ قاعده عمومی زبان JavaScript وجود ندارد که یکی را در تمام شرایط بر دیگری برتر بداند.

---

# Common Mistakes

## 1. اشتباه گرفتن Assignment و Equality

```javascript
=
```

با:

```javascript
===
```

یکسان نیستند.

---

## 2. فرض کردن اینکه `==` و `===` یک رفتار دارند

این دو Operator از قواعد متفاوتی استفاده می‌کنند.

---

## 3. استفاده از Ternaryهای پیچیده

Ternary باید خوانایی را افزایش دهد، نه اینکه جایگزین اجباری `if/else` شود.

---

## 4. بی‌توجهی به Precedence

در Expressionهای چندبخشی، اگر ترتیب ارزیابی برای خواننده واضح نیست، از Parentheses استفاده کنید.

---

## 5. طبقه‌بندی اشتباه `++`

`++` و `--` را Assignment Operator ندانید.

آن‌ها **Update Operators** هستند.

---

# دیدگاه Jonas

در آموزش Jonas، Operatorها صرفاً Syntaxهایی برای حفظ کردن نیستند.

آن‌ها ابزارهایی برای ساخت Expression و پیاده‌سازی Logic برنامه هستند.

به همین دلیل در یادگیری آن‌ها، درک رفتار Operator و کاربرد آن در یک مسئله واقعی از حفظ کردن جدول Syntax مهم‌تر است.

این دیدگاه با هدف این کتاب نیز هماهنگ است:

> ابتدا مدل ذهنی و دلیل وجود یک قابلیت را درک کنیم، سپس Syntax آن را به‌کار ببریم.

---

# Block 05 — Chapter Review

# خلاصه فصل

در این فصل بررسی کردیم که JavaScript چگونه با استفاده از Operatorها روی Values عملیات انجام می‌دهد.

ابتدا مفهوم **Expression** را شناختیم و دیدیم که Expression هنگام ارزیابی یک Value تولید می‌کند.

سپس با **Operand** و **Operator** آشنا شدیم و دیدیم که یک Operation چگونه از این اجزا تشکیل می‌شود.

در ادامه Arithmetic Operators را بررسی کردیم و سپس به **Assignment** رسیدیم؛ جایی که نتیجه یک Expression به یک Binding اختصاص داده می‌شود.

بعد از آن Comparison Operators را بررسی کردیم و تفاوت مهم میان:

```javascript
==
```

و:

```javascript
===
```

را شناختیم.

در ادامه Logical Operators را برای ترکیب Conditions، Unary Operators را برای عملیات روی یک Operand و Ternary Operator را برای انتخاب میان دو Expression بررسی کردیم.

در پایان نیز با **Operator Precedence** آشنا شدیم و دیدیم که JavaScript چگونه ترتیب ارزیابی Operationها را مشخص می‌کند.

---

# Key Takeaways

* Expression ساختاری است که هنگام ارزیابی یک Value تولید می‌کند.
* Operand مقداری است که Operator روی آن عمل می‌کند.
* Operator نوع Operation را مشخص می‌کند.
* Arithmetic Operators برای محاسبات استفاده می‌شوند.
* `%` باقی‌مانده تقسیم را تولید می‌کند.
* Assignment نتیجه سمت راست را به هدف سمت چپ اختصاص می‌دهد.
* `=` برای Assignment است، نه Equality.
* `++` و `--` Update Operators هستند.
* Comparison Operators نتیجه Boolean تولید می‌کنند.
* `==` از Loose Equality استفاده می‌کند و ممکن است Type Coercion انجام دهد.
* `===` Strict Equality است و Type Coercion انجام نمی‌دهد.
* `&&`، `||` و `!` برای Logical Operations استفاده می‌شوند.
* `typeof` یک Unary Operator است.
* Ternary یک Conditional Expression است.
* Operator Precedence ترتیب اولویت Operationها را مشخص می‌کند.
* Parentheses می‌تواند ترتیب ارزیابی و Intent کد را واضح‌تر کند.
* انتخاب Operator باید بر اساس Intent، خوانایی و رفتار موردنیاز انجام شود.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Expression چیست؟

### سؤال ۲

Operand چیست؟

### سؤال ۳

Operator چیست؟

### سؤال ۴

تفاوت Assignment و Comparison چیست؟

### سؤال ۵

تفاوت `=` و `===` چیست؟

### سؤال ۶

Comparison Operator چه نوع نتیجه‌ای تولید می‌کند؟

### سؤال ۷

تفاوت `==` و `===` چیست؟

### سؤال ۸

Ternary Operator چیست؟

---

## سطح Mid-Level

### سؤال ۹

چرا در Codebaseهای مدرن معمولاً `===` نسبت به `==` ترجیح داده می‌شود؟

### سؤال ۱۰

Type Coercion چه ارتباطی با `==` دارد؟

### سؤال ۱۱

تفاوت Logical Operatorهای `&&` و `||` چیست؟

### سؤال ۱۲

چرا `typeof` یک Unary Operator محسوب می‌شود؟

### سؤال ۱۳

چرا `++` را نباید Assignment Operator نامید؟

### سؤال ۱۴

Operator Precedence چیست و چرا مهم است؟

### سؤال ۱۵

چه زمانی Ternary Operator را به `if/else` ترجیح می‌دهید؟

---

## سطح Senior

### سؤال ۱۶

چرا Expression بودن Ternary از نظر طراحی زبان اهمیت دارد؟

### سؤال ۱۷

چگونه انتخاب Operator می‌تواند بر Readability و Maintainability کد تأثیر بگذارد؟

### سؤال ۱۸

چرا اتکا به Type Coercion ضمنی می‌تواند در طراحی Logic برنامه مشکل‌ساز شود؟

### سؤال ۱۹

چه زمانی استفاده از Parentheses حتی اگر از نظر Precedence ضروری نباشد، تصمیم مهندسی مناسبی است؟

### سؤال ۲۰

چگونه می‌توان بین کوتاه بودن Expression و خوانایی آن تعادل برقرار کرد؟

---

# Golden Answers

## Expression چیست؟

Expression ساختاری از JavaScript است که هنگام ارزیابی یک Value تولید می‌کند.

---

## Operand چیست؟

Operand مقداری یا Expressionای است که یک Operator روی آن عملیات انجام می‌دهد.

---

## Operator چیست؟

Operator بخشی از Syntax زبان است که نوع و رفتار یک Operation را روی یک یا چند Operand مشخص می‌کند.

---

## Assignment و Comparison چه تفاوتی دارند؟

Assignment نتیجه یک Expression را به یک Binding اختصاص می‌دهد.

Comparison رابطه میان Values را بررسی کرده و یک Boolean Result تولید می‌کند.

---

## تفاوت `==` و `===` چیست؟

`==` از Loose Equality استفاده می‌کند و در برخی شرایط Type Coercion انجام می‌دهد.

`===` از Strict Equality استفاده می‌کند و Type Coercion انجام نمی‌دهد.

---

## چرا `===` معمولاً ترجیح داده می‌شود؟

زیرا رفتار Comparison را قابل پیش‌بینی‌تر می‌کند و از Type Coercion ضمنی جلوگیری می‌کند.

---

## `++` چیست؟

`++` یک Update Operator است که مقدار یک واحد افزایش می‌دهد.

نباید آن را Assignment Operator نامید.

---

## `typeof` چیست؟

`typeof` یک Unary Operator است که Type یک Expression را به‌صورت String گزارش می‌کند.

---

## Ternary Operator چیست؟

Ternary یک Conditional Operator است که بر اساس یک Condition یکی از دو Expression را انتخاب می‌کند.

---

## Operator Precedence چیست؟

Precedence قواعدی است که ترتیب اولویت Operationها را در Expressionهای دارای چند Operator مشخص می‌کند.

---

## پاسخ کوتاه طلایی مصاحبه

**در پروژه واقعی `==` یا `===`؟**

در حالت معمول `===` را انتخاب می‌کنم، چون Strict Equality است و Type Coercion انجام نمی‌دهد. این موضوع رفتار Comparison را قابل پیش‌بینی‌تر می‌کند.

---

# اشتباهات رایج فصل

### اشتباه ۱

```javascript
=
```

را با Equality اشتباه بگیریم.

### صحیح

`=` برای Assignment است.

---

### اشتباه ۲

تصور کنیم:

```javascript
5 == '5'
```

و:

```javascript
5 === '5'
```

رفتار یکسانی دارند.

### صحیح

اولی Loose Equality و دومی Strict Equality است.

---

### اشتباه ۳

`++` را Assignment Operator بدانیم.

### صحیح

`++` یک Update Operator است.

---

### اشتباه ۴

از Ternary برای هر نوع Conditional Logic استفاده کنیم.

### صحیح

Ternary برای انتخاب‌های ساده و Expressionهای کوتاه مناسب است.

---

### اشتباه ۵

Precedence را نادیده بگیریم.

### صحیح

در Expressionهای پیچیده، Precedence را در نظر بگیرید و در صورت نیاز از Parentheses استفاده کنید.

---

# جمع‌بندی فصل

در این فصل یک مدل ذهنی مهم برای کار با Values ساختیم:

```text
Value
↓
Expression
↓
Operand
↓
Operator
↓
Operation
↓
Result
```

سپس دیدیم که این مدل چگونه به گروه‌های مختلف Operator گسترش پیدا می‌کند:

```text
Arithmetic
↓
Assignment
↓
Comparison
↓
Logical
↓
Unary
↓
Ternary
↓
Precedence
```

این ترتیب تصادفی نیست.

ابتدا یاد گرفتیم چگونه روی Values عملیات انجام دهیم.

سپس یاد گرفتیم چگونه نتیجه را ذخیره کنیم.

بعد روابط میان Values را بررسی کردیم.

در ادامه Conditions را ترکیب کردیم، روی یک Operand عملیات انجام دادیم و در نهایت با استفاده از Ternary یک Expression شرطی ساختیم.

در پایان نیز یاد گرفتیم که وقتی چند Operator در یک Expression حضور دارند، JavaScript با استفاده از Precedence ترتیب ارزیابی آن‌ها را تعیین می‌کند.

بنابراین Operatorها را نباید مجموعه‌ای از علامت‌های مستقل برای حفظ کردن در نظر گرفت.

آن‌ها ابزارهایی هستند که با کمک Expressionها به ما اجازه می‌دهند **Values را پردازش کنیم، State را تغییر دهیم، روابط را بررسی کنیم و Logic برنامه را بیان کنیم.**

در فصل بعد با **Strings and Template Literals** آشنا می‌شویم و بررسی می‌کنیم JavaScript چگونه داده‌های متنی را ذخیره، ترکیب و تولید می‌کند.
