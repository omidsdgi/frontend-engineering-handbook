# Chapter 24 — Short Circuiting and Logical Patterns

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* رفتار `&&`، `||` و `??` را در Expressionها توضیح دهید.
* تفاوت **Boolean Logic** و **Value Evaluation** را در Logical Operators تشخیص دهید.
* مفهوم **Short-Circuit Evaluation** را دقیق توضیح دهید.
* توضیح دهید چرا Logical Operators همیشه `true` یا `false` برنمی‌گردانند.
* نقش **Truthy / Falsy** را در `&&` و `||` تحلیل کنید.
* از `&&` برای **Conditional Execution** استفاده کنید.
* از `||` و `??` برای ساخت **Default Values** استفاده کنید.
* تفاوت `||` و `??` را در طراحی Default Value تشخیص دهید.
* از Logical Operators برای ساخت الگوهای ساده و خوانای شرطی استفاده کنید.
* از انتخاب نادرست Logical Operator و ایجاد رفتار ناخواسته جلوگیری کنید.

---

# Core Question

> **Logical Operators چگونه می‌توانند علاوه بر Boolean Logic جریان ارزیابی Expression را کنترل کنند؟**

در فصل‌های قبل با Logical Operators و مفاهیمی مانند Truthy و Falsy آشنا شدیم.

اکنون می‌خواهیم یک گام جلوتر برویم.

نکته مهم این است که `&&` و `||` صرفاً برای تولید یک Boolean Value استفاده نمی‌شوند.

آن‌ها می‌توانند مشخص کنند:

* آیا بخش بعدی Expression اصلاً ارزیابی شود؟
* کدام Value از یک Expression برگردانده شود؟
* آیا یک Action فقط در صورت برقرار بودن یک Condition اجرا شود؟
* اگر یک Value وجود نداشت، چه Value جایگزینی استفاده شود؟

بنابراین مدل ذهنی این فصل چنین است:

```text
Logical Operator
      ↓
Evaluation Order
      ↓
Truthy / Falsy
      ↓
Short Circuit
      ↓
Returned Value
      ↓
Practical Pattern
```

---

# مقدمه

فرض کنید در یک Application فقط زمانی می‌خواهیم پیام موفقیت را نمایش دهیم که عملیات با موفقیت انجام شده باشد.

یک روش استفاده از `if` است:

```javascript
if (isSuccess) {
  showSuccessMessage();
}
```

اما در JavaScript می‌توان همین الگو را با یک Logical Expression نیز نوشت:

```javascript
isSuccess && showSuccessMessage();
```

در اینجا `&&` فقط یک عملگر منطقی نیست.

این Operator تعیین می‌کند آیا Expression سمت راست اصلاً ارزیابی شود یا خیر.

مثال دیگری را در نظر بگیرید.

فرض کنید یک User ممکن است نام کاربری نداشته باشد:

```javascript
const displayName = username || 'Guest';
```

اینجا `||` برای انتخاب یک **Default Value** استفاده شده است.

یا در یک Application ممکن است مقدار `timeout` برابر `0` باشد و `0` یک مقدار معتبر محسوب شود:

```javascript
const timeout = config.timeout ?? 5000;
```

در اینجا `??` انتخاب مناسب‌تری است.

پس یک سؤال مهم مطرح می‌شود:

> چرا Logical Operators می‌توانند چنین رفتارهایی داشته باشند؟

پاسخ در نحوه **ارزیابی Expression** نهفته است.

---

# Logical Evaluation

## Logical Operators فقط Boolean تولید نمی‌کنند

در منطق کلاسیک ممکن است انتظار داشته باشیم:

```text
true && true  → true
true || false → true
```

این رفتار درست است.

اما در JavaScript، Logical Operators می‌توانند **خود Valueهای موجود در Expression** را نیز برگردانند.

مثلاً:

```javascript
const result = 'Hello' && 'World';

console.log(result);
```

خروجی:

```text
World
```

یا:

```javascript
const result = '' || 'Guest';

console.log(result);
```

خروجی:

```text
Guest
```

پس نتیجه Logical Expression الزاماً `true` یا `false` نیست.

این نکته، پایه اصلی درک Short Circuiting است.

---

# Evaluation Order

برای تحلیل Logical Operators ابتدا باید بدانیم Expressionها چگونه ارزیابی می‌شوند.

در Expression زیر:

```javascript
const result = a && b;
```

JavaScript ابتدا `a` را ارزیابی می‌کند.

سپس بر اساس نتیجه آن تصمیم می‌گیرد آیا لازم است `b` را نیز ارزیابی کند یا خیر.

همین رفتار برای `||` نیز وجود دارد.

بنابراین یک مدل ذهنی مناسب این است:

```text
Left Operand
     ↓
Evaluate
     ↓
Can the result already be determined?
     ↓
Yes → Stop
No  → Evaluate Right Operand
```

این رفتار همان **Short-Circuit Evaluation** است.

---

# Truthy / Falsy

برای درک Short Circuiting باید نقش Truthy و Falsy را به یاد داشته باشیم.

در JavaScript بعضی Valueها در Context بولی به‌صورت `false` ارزیابی می‌شوند.

مهم‌ترین Falsy Values عبارت‌اند از:

```javascript
false
0
-0
0n
''
null
undefined
NaN
```

بسیاری از Valueهای دیگر در Context بولی Truthy هستند.

مثلاً:

```javascript
'hello'
42
[]
{}
```

Truthy هستند.

نکته مهم این است که Truthy یا Falsy بودن یک Value به این معنا نیست که Type آن Boolean است.

مثلاً:

```javascript
const value = 'hello';

console.log(typeof value);
```

خروجی:

```text
string
```

اما:

```javascript
Boolean(value);
```

نتیجه:

```text
true
```

است.

در Short Circuiting، JavaScript از همین رفتار برای تصمیم‌گیری درباره ادامه ارزیابی استفاده می‌کند.

---

# Short-Circuit Evaluation

## تعریف

**Short-Circuit Evaluation** یعنی JavaScript در یک Logical Expression، اگر بتواند نتیجه نهایی را بدون ارزیابی بخش باقی‌مانده مشخص کند، ارزیابی را متوقف می‌کند.

به بیان ساده:

> اگر نتیجه از قبل مشخص شده باشد، JavaScript ادامه Expression را ارزیابی نمی‌کند.

این رفتار هم برای `&&` و هم برای `||` وجود دارد، اما شرط توقف در آن‌ها متفاوت است.

---

# `&&` و Short Circuiting

## چرا `&&` متوقف می‌شود؟

در Expression زیر:

```javascript
a && b
```

برای اینکه نتیجه کل Expression Truthy باشد، هر دو Operand باید Truthy باشند.

اگر `a` Falsy باشد، دیگر مهم نیست `b` چه مقداری دارد.

مثلاً:

```javascript
false && something();
```

نتیجه از قبل مشخص است.

چون:

```text
false && anything → false
```

بنابراین `something()` اصلاً اجرا نمی‌شود.

---

## مثال

```javascript
const isLoggedIn = false;

isLoggedIn && showDashboard();
```

اگر:

```javascript
isLoggedIn
```

برابر `false` باشد، بخش سمت راست:

```javascript
showDashboard();
```

اجرا نمی‌شود.

این همان **Conditional Execution** است.

---

# `&&` فقط `false` برنمی‌گرداند

نکته مهم‌تر این است که `&&` الزاماً `true` یا `false` برنمی‌گرداند.

مثال:

```javascript
const result = 'admin' && 'dashboard';

console.log(result);
```

خروجی:

```text
dashboard
```

چرا؟

چون Operand اول:

```javascript
'admin'
```

Truthy است.

بنابراین JavaScript مجبور است Operand دوم را نیز ارزیابی کند.

نتیجه نهایی Value سمت راست است:

```javascript
'dashboard'
```

---

## اگر Operand اول Falsy باشد

```javascript
const result = '' && 'dashboard';

console.log(result);
```

خروجی:

```text
```

در این حالت JavaScript همان Value سمت چپ را برمی‌گرداند.

بنابراین می‌توان رفتار `&&` را به‌صورت مفهومی چنین خلاصه کرد:

```text
A && B

A is Falsy
    ↓
Return A

A is Truthy
    ↓
Evaluate B
    ↓
Return B
```

این یکی از مهم‌ترین مدل‌های ذهنی این فصل است.

---

# مثال واقعی

فرض کنید فقط در صورت وجود یک User می‌خواهیم نام او را نمایش دهیم:

```javascript
const user = {
  name: 'Omid'
};

user && console.log(user.name);
```

اگر `user` یک Value Truthy باشد، Expression سمت راست ارزیابی می‌شود.

اما اگر:

```javascript
const user = null;
```

باشد:

```javascript
user && console.log(user.name);
```

`console.log` اجرا نمی‌شود.

در نتیجه دسترسی به:

```javascript
user.name
```

نیز انجام نمی‌شود.

این الگو قبل از معرفی Optional Chaining کاربرد زیادی داشت.

برای دسترسی ایمن به داده‌های تو‌در‌تو، Optional Chaining در فصل قبل بررسی شده است و در این فصل دوباره آموزش داده نمی‌شود.

---

# `||` و Short Circuiting

اکنون به Operator دوم می‌رسیم:

```javascript
||
```

در Expression زیر:

```javascript
a || b
```

اگر `a` Truthy باشد، دیگر برای تعیین نتیجه نیازی به `b` نیست.

زیرا:

```text
true || anything → true
```

بنابراین JavaScript ارزیابی را متوقف می‌کند.

---

## مثال

```javascript
const username = 'Omid';

const displayName = username || 'Guest';

console.log(displayName);
```

خروجی:

```text
Omid
```

چون:

```javascript
username
```

Truthy است.

پس:

```javascript
' Omid ' || 'Guest'
```

نیازی به ارزیابی Operand دوم ندارد.

---

## اگر Operand اول Falsy باشد

```javascript
const username = '';

const displayName = username || 'Guest';

console.log(displayName);
```

خروجی:

```text
Guest
```

چون مقدار سمت چپ Falsy است.

پس JavaScript به سمت راست می‌رود و مقدار آن را برمی‌گرداند.

مدل ذهنی:

```text
A || B

A is Truthy
    ↓
Return A

A is Falsy
    ↓
Evaluate B
    ↓
Return B
```

---

# تفاوت اصلی `&&` و `||`

می‌توان رفتار این دو Operator را به شکل زیر خلاصه کرد:

| Expression | اگر سمت چپ ... باشد | نتیجه |        |     |
| ---------- | ------------------- | ----- | ------ | --- |
| `A && B`   | Falsy               | `A`   |        |     |
| `A && B`   | Truthy              | `B`   |        |     |
| `A         |                     | B`    | Truthy | `A` |
| `A         |                     | B`    | Falsy  | `B` |

نکته مهم این جدول:

**`&&` و `||` الزاماً Boolean برنمی‌گردانند.**

آن‌ها یکی از Operandهای خود را برمی‌گردانند.

---

# چرا Returned Value مهم است؟

فرض کنید:

```javascript
const user = {
  name: 'Omid'
};

const result = user && user.name;

console.log(result);
```

خروجی:

```text
Omid
```

در اینجا:

```javascript
user && user.name
```

یک Boolean Expression ساده نیست.

Expression از Valueهای واقعی Application استفاده می‌کند.

این رفتار امکان ایجاد الگوهای کوتاه و کاربردی را فراهم می‌کند.

---

# Conditional Execution

یکی از رایج‌ترین کاربردهای `&&` اجرای شرطی یک Function است.

مثلاً:

```javascript
const hasPermission = true;

hasPermission && showAdminPanel();
```

اگر Permission وجود داشته باشد:

```javascript
showAdminPanel();
```

اجرا می‌شود.

اگر وجود نداشته باشد، اجرا نمی‌شود.

این الگو مخصوصاً برای یک Action کوتاه مفید است.

---

## مثال Application

```javascript
const cartItems = ['Laptop', 'Mouse'];

cartItems.length > 0 && showCart();
```

اگر Cart دارای Item باشد:

```javascript
showCart();
```

اجرا می‌شود.

اگر Cart خالی باشد، Function اجرا نمی‌شود.

---

# `&&` در برابر `if`

هر دو می‌توانند Conditional Execution ایجاد کنند.

با `if`:

```javascript
if (isLoggedIn) {
  showDashboard();
}
```

با `&&`:

```javascript
isLoggedIn && showDashboard();
```

هر دو ممکن است درست باشند.

اما این به معنای آن نیست که همیشه باید `if` را با `&&` جایگزین کنیم.

اگر منطق شرطی پیچیده شود، `if` معمولاً خواناتر است:

```javascript
if (isLoggedIn && hasPermission) {
  showDashboard();
}
```

در مقابل:

```javascript
isLoggedIn && hasPermission && showDashboard();
```

ممکن است برای یک Action ساده قابل قبول باشد، اما با افزایش پیچیدگی خوانایی کاهش می‌یابد.

هدف Logical Pattern باید **خوانایی** باشد، نه کوتاه‌ترین کد ممکن.

---

# Default Values با `||`

یکی از کاربردهای بسیار رایج `||` انتخاب Default Value است.

مثلاً:

```javascript
const username = '';

const displayName = username || 'Guest';
```

در اینجا اگر `username` Falsy باشد، مقدار:

```javascript
'Guest'
```

استفاده می‌شود.

این الگو زمانی مناسب است که تمام Valueهای Falsy برای Application شما به معنای «مقدار قابل استفاده نیست» باشند.

اما این شرط همیشه برقرار نیست.

---

# مشکل Default Value با `||`

فرض کنید:

```javascript
const settings = {
  timeout: 0
};
```

و `0` یک مقدار کاملاً معتبر برای Application است.

اگر بنویسیم:

```javascript
const timeout = settings.timeout || 5000;
```

نتیجه:

```javascript
5000
```

خواهد بود.

چرا؟

زیرا:

```javascript
0
```

Falsy است.

اما ما نمی‌خواستیم `0` را به‌عنوان مقدار نامعتبر در نظر بگیریم.

اینجاست که `??` اهمیت پیدا می‌کند.

---

# Nullish Coalescing — `??`

## چرا `??` به وجود آمده است؟

گاهی فقط می‌خواهیم زمانی Default Value استفاده شود که مقدار واقعاً **وجود ندارد**.

در JavaScript دو Value اصلی برای نشان دادن نبود مقدار عبارت‌اند از:

```javascript
null
undefined
```

Operator زیر:

```javascript
??
```

برای همین سناریو طراحی شده است.

---

## رفتار `??`

در Expression زیر:

```javascript
A ?? B
```

اگر `A` برابر `null` یا `undefined` نباشد، همان `A` برگردانده می‌شود.

اگر `A` برابر `null` یا `undefined` باشد، `B` ارزیابی و برگردانده می‌شود.

مدل ذهنی:

```text
A ?? B

A is null or undefined
        ↓
Evaluate B
        ↓
Return B

A is anything else
        ↓
Return A
```

---

## مثال

```javascript
const timeout = 0;

const finalTimeout = timeout ?? 5000;

console.log(finalTimeout);
```

خروجی:

```text
0
```

زیرا:

```javascript
0
```

نه `null` است و نه `undefined`.

بنابراین `??` مقدار معتبر `0` را حفظ می‌کند.

---

# مقایسه `||` و `??`

این تفاوت یکی از مهم‌ترین نکات این فصل است.

```javascript
const value = 0;

console.log(value || 100);
console.log(value ?? 100);
```

خروجی:

```text
100
0
```

چرا؟

در:

```javascript
value || 100
```

مقدار `0` Falsy است.

بنابراین `100` انتخاب می‌شود.

اما در:

```javascript
value ?? 100
```

مقدار `0` Nullish نیست.

پس خود `0` برگردانده می‌شود.

---

## جدول مقایسه

| Value سمت چپ | `value || 'default'` | `value ?? 'default'` |
|---|---|---|
| `false` | `'default'` | `false` |
| `0` | `'default'` | `0` |
| `''` | `'default'` | `''` |
| `null` | `'default'` | `'default'` |
| `undefined` | `'default'` | `'default'` |
| `'hello'` | `'hello'` | `'hello'` |

بنابراین انتخاب Operator باید بر اساس **معنای داده** انجام شود.

---

# یک قانون عملی

اگر منظور شما این است:

> «اگر Value Truthy نبود، مقدار جایگزین را استفاده کن.»

از:

```javascript
||
```

استفاده کنید.

اگر منظور شما این است:

> «اگر Value فقط `null` یا `undefined` بود، مقدار جایگزین را استفاده کن.»

از:

```javascript
??
```

استفاده کنید.

این تفاوت کوچک از بسیاری از Bugهای منطقی جلوگیری می‌کند.

---

# Guard Patterns

یکی دیگر از کاربردهای Logical Operators ساختن Guardهای ساده است.

Guard یعنی قبل از انجام یک عملیات، یک شرط را بررسی کنیم.

مثلاً:

```javascript
user && user.isActive && loadDashboard();
```

جریان مفهومی:

```text
user exists?
    ↓ yes
user is active?
    ↓ yes
loadDashboard()
```

اگر یکی از Conditions مقدار Falsy داشته باشد، ادامه Expression متوقف می‌شود.

این الگو را می‌توان نوعی **Guard Pattern** دانست.

---

## Guard برای Permission

```javascript
isLoggedIn && hasPermission && showAdminPanel();
```

در اینجا:

```text
isLoggedIn
↓
hasPermission
↓
showAdminPanel()
```

هر شرط مانند یک Gate عمل می‌کند.

اگر یکی از Gateها عبور نکند، Function نهایی اجرا نمی‌شود.

---

# Guard Pattern و خوانایی

Guard Pattern برای شرایط ساده مفید است.

اما نباید Expression را بیش از حد طولانی کنیم.

این کد:

```javascript
isLoggedIn && hasPermission && !isBlocked && showAdminPanel();
```

هنوز قابل فهم است.

اما اگر شرط‌ها پیچیده شوند:

```javascript
isLoggedIn &&
hasPermission &&
user.role === 'admin' &&
user.settings &&
user.settings.enabled &&
showAdminPanel();
```

ممکن است `if` انتخاب خواناتری باشد:

```javascript
if (
  isLoggedIn &&
  hasPermission &&
  user.role === 'admin' &&
  user.settings &&
  user.settings.enabled
) {
  showAdminPanel();
}
```

هدف استفاده از Short Circuiting، کاهش بی‌دلیل تعداد خطوط نیست.

هدف اصلی، بیان روشن منطق است.

---

# ترکیب `&&` و `||`

می‌توان Logical Operators را با یکدیگر ترکیب کرد.

مثلاً:

```javascript
const label = isAdmin && 'Admin' || 'User';
```

اگر:

```javascript
isAdmin
```

Truthy باشد:

```javascript
isAdmin && 'Admin'
```

به `'Admin'` می‌رسد.

سپس:

```javascript
'Admin' || 'User'
```

همان `'Admin'` را برمی‌گرداند.

اگر `isAdmin` Falsy باشد، قسمت اول نتیجه Falsy خواهد بود و `||` به `'User'` می‌رسد.

این الگو از نظر فنی قابل اجرا است، اما معمولاً بهترین انتخاب نیست.

برای انتخاب میان دو Value بر اساس یک Condition، **Conditional Operator** که در فصل‌های قبل معرفی شد، اغلب واضح‌تر است:

```javascript
const label = isAdmin ? 'Admin' : 'User';
```

بنابراین:

> هر Expression کوتاه‌تر، الزاماً Expression بهتری نیست.

---

# `??` و Short Circuiting

`??` نیز دارای رفتار Short-Circuit است.

مثلاً:

```javascript
const name = 'Omid';

const result = name ?? getDefaultName();
```

چون:

```javascript
name
```

نه `null` است و نه `undefined`، Function سمت راست اجرا نمی‌شود.

بنابراین:

```javascript
getDefaultName()
```

فراخوانی نمی‌شود.

اگر:

```javascript
const name = null;
```

باشد، سمت راست ارزیابی می‌شود:

```javascript
const result = name ?? getDefaultName();
```

این رفتار از نظر ساختاری شبیه Short Circuiting در `||` است، اما شرط آن متفاوت است.

---

# `||`، `&&` و `??` در کنار هم

می‌توانیم سه Operator اصلی این فصل را چنین مقایسه کنیم:

| Operator | چه زمانی سمت راست را ارزیابی می‌کند؟ | معمولاً چه زمانی متوقف می‌شود؟ |                     |                      |
| -------- | ------------------------------------ | ------------------------------ | ------------------- | -------------------- |
| `A && B` | وقتی `A` Truthy باشد                 | وقتی `A` Falsy باشد            |                     |                      |
| `A       |                                      | B`                             | وقتی `A` Falsy باشد | وقتی `A` Truthy باشد |
| `A ?? B` | وقتی `A` `null` یا `undefined` باشد  | وقتی `A` Nullish نباشد         |                     |                      |

این جدول، یکی از بهترین خلاصه‌های رفتاری این سه Operator است.

---

# Choosing the Correct Operator

انتخاب Operator باید از **معنای Business Logic** شروع شود.

فرض کنید:

```javascript
const retryCount = 0;
```

اگر `0` مقدار معتبر باشد:

```javascript
const count = retryCount ?? 3;
```

درست‌تر است.

اما اگر منطق Application بگوید:

> هر مقدار Falsy یعنی مقدار معتبر در دسترس نیست.

آنگاه:

```javascript
const count = retryCount || 3;
```

می‌تواند مناسب باشد.

بنابراین سؤال اصلی این نیست:

> کدام Syntax کوتاه‌تر است؟

بلکه:

> چه Valueهایی باید به‌عنوان «وجود ندارد» یا «نامعتبر» در نظر گرفته شوند؟

این همان نگاه مهندسی به Logical Patterns است.

---

# ترکیب `??` با `||`

در JavaScript نمی‌توان `??` را بدون پرانتز مستقیماً با `||` یا `&&` در یک Expression ترکیب کرد.

مثلاً این Expression معتبر نیست:

```javascript
a ?? b || c;
```

برای مشخص کردن ترتیب موردنظر باید از Parentheses استفاده کنیم:

```javascript
(a ?? b) || c;
```

یا:

```javascript
a ?? (b || c);
```

این دو Expression الزاماً رفتار یکسانی ندارند.

بنابراین وقتی چند Logical Operator را ترکیب می‌کنیم، باید منطق Expression را به‌صورت صریح مشخص کنیم.

---

# ارتباط با Optional Chaining

در فصل قبل با **Optional Chaining** آشنا شدیم.

Optional Chaining نیز از ایده Short Circuiting استفاده می‌کند تا در شرایط مشخص ادامه دسترسی انجام نشود.

مثلاً:

```javascript
const city = user?.address?.city;
```

اگر یکی از بخش‌های زنجیره `null` یا `undefined` باشد، ادامه دسترسی متوقف می‌شود.

در این فصل هدف ما آموزش مجدد `?.` نیست.

نکته‌ای که باید به خاطر داشته باشیم این است که:

```text
Optional Chaining
        +
Nullish Coalescing
```

می‌توانند در Applicationهای واقعی برای دسترسی ایمن و تعیین Default Value در کنار یکدیگر استفاده شوند.

مثلاً:

```javascript
const city = user?.address?.city ?? 'Unknown';
```

در این Expression:

```text
user?.address?.city
        ↓
Safe Access
        ↓
??
Default Value
```

جزئیات Optional Chaining در فصل مربوط به آن بررسی شده است.

---

# Practical Patterns

اکنون می‌توانیم الگوهای اصلی این فصل را در یک Application واقعی ببینیم.

## Conditional Action

```javascript
isLoggedIn && loadDashboard();
```

معنا:

> فقط اگر User وارد شده باشد، Dashboard را Load کن.

---

## Default Value

```javascript
const username = inputName || 'Guest';
```

معنا:

> اگر `inputName` Falsy بود، `Guest` را استفاده کن.

---

## Preserve Valid Falsy Value

```javascript
const page = query.page ?? 1;
```

اگر:

```javascript
query.page = 0;
```

باشد، مقدار `0` حفظ می‌شود.

---

## Guard Chain

```javascript
user && user.isActive && openAccount();
```

معنا:

> اگر User وجود دارد و Active است، Account را باز کن.

---

## Safe Access + Default

```javascript
const role = user?.profile?.role ?? 'user';
```

معنا:

> Role را بخوان؛ اگر مسیر یا مقدار نهایی Nullish بود، `'user'` را استفاده کن.

---

# Best Practices

## 1. Operator را بر اساس Meaning انتخاب کنید

به جای انتخاب Operator صرفاً به دلیل کوتاه بودن Syntax، مشخص کنید:

* Falsy بودن چه معنایی دارد؟
* `0` معتبر است؟
* `''` معتبر است؟
* `false` معتبر است؟
* فقط `null` و `undefined` باید Default ایجاد کنند؟

سپس Operator را انتخاب کنید.

---

## 2. از `??` برای داده‌هایی که Falsy معتبر دارند استفاده کنید

مثلاً:

```javascript
const limit = config.limit ?? 10;
```

اگر:

```javascript
limit = 0;
```

باشد، مقدار معتبر `0` حفظ می‌شود.

---

## 3. از `&&` برای Actionهای ساده استفاده کنید

مثلاً:

```javascript
isReady && startApp();
```

این الگو زمانی خواناست که شرط و Action ساده باشند.

---

## 4. از Logical Operators برای مخفی کردن Logic پیچیده استفاده نکنید

اگر Expression برای درک شدن نیاز به توضیح طولانی دارد، احتمالاً `if` یا یک Variable میانی انتخاب بهتری است.

---

## 5. تفاوت `||` و `??` را آگاهانه انتخاب کنید

این دو Operator قابل جایگزینی کورکورانه نیستند.

```javascript
value || fallback
```

و:

```javascript
value ?? fallback
```

معنای یکسانی ندارند.

---

## 6. در ترکیب چند Operator از Parentheses استفاده کنید

Parentheses می‌تواند Intent را واضح‌تر کند.

مثلاً:

```javascript
const result = (a ?? b) || c;
```

خواننده می‌تواند ترتیب منطقی Expression را سریع‌تر تشخیص دهد.

---

# Common Mistakes

## اشتباه اول: تصور اینکه `&&` و `||` همیشه Boolean برمی‌گردانند

❌

```javascript
const result = 'Admin' || 'User';

console.log(result); // true
```

✔

```javascript
const result = 'Admin' || 'User';

console.log(result); // "Admin"
```

Logical Operators در JavaScript می‌توانند یکی از Operandها را برگردانند.

---

## اشتباه دوم: تصور اینکه `||` فقط برای Boolean Logic است

`||` می‌تواند برای انتخاب Default Value نیز استفاده شود:

```javascript
const name = username || 'Guest';
```

---

## اشتباه سوم: استفاده از `||` وقتی `0` معتبر است

❌

```javascript
const timeout = config.timeout || 5000;
```

اگر:

```javascript
config.timeout === 0
```

باشد، `5000` انتخاب می‌شود.

✔

```javascript
const timeout = config.timeout ?? 5000;
```

---

## اشتباه چهارم: تصور اینکه `??` برای تمام Falsy Values فعال می‌شود

❌

```javascript
0 ?? 10
```

نتیجه:

```text
0
```

و:

```javascript
'' ?? 'Guest'
```

نتیجه:

```text
''
```

فقط:

```javascript
null
undefined
```

باعث استفاده از سمت راست `??` می‌شوند.

---

## اشتباه پنجم: اجرای Function در سمتی که قرار نیست اجرا شود

در:

```javascript
isReady && startApp();
```

اگر:

```javascript
isReady
```

Falsy باشد، `startApp()` اجرا نمی‌شود.

این یکی از مزایای اصلی Short Circuiting است.

---

## اشتباه ششم: استفاده بیش از حد از Chainهای Logical

این کد:

```javascript
a && b && c && d && e && doSomething();
```

ممکن است از نظر Syntax معتبر باشد، اما اگر منطق پیچیده شود، خوانایی کاهش می‌یابد.

کد کوتاه الزاماً کد بهتر نیست.

---

## اشتباه هفتم: استفاده از `||` به جای Conditional Operator

مثلاً:

```javascript
const label = isAdmin && 'Admin' || 'User';
```

ممکن است کار کند، اما:

```javascript
const label = isAdmin ? 'Admin' : 'User';
```

برای انتخاب میان دو Value بر اساس یک Condition، معمولاً Intent واضح‌تری دارد.

---

## اشتباه هشتم: ترکیب `??` و `||` بدون Parentheses

❌

```javascript
const value = a ?? b || c;
```

✔

```javascript
const value = (a ?? b) || c;
```

یا:

```javascript
const value = a ?? (b || c);
```

بسته به منطق موردنظر.

---

# Summary

Logical Operators در JavaScript فقط ابزارهایی برای محاسبه `true` و `false` نیستند.

آن‌ها می‌توانند جریان ارزیابی یک Expression را نیز کنترل کنند.

در `&&`، اگر Operand سمت چپ Falsy باشد، JavaScript دیگر نیازی به ارزیابی سمت راست ندارد.

در `||`، اگر Operand سمت چپ Truthy باشد، ارزیابی متوقف می‌شود.

در `??`، اگر Operand سمت چپ نه `null` و نه `undefined` باشد، Operand سمت راست ارزیابی نمی‌شود.

این رفتار **Short-Circuit Evaluation** نام دارد.

همچنین دیدیم که Logical Operators در JavaScript الزاماً Boolean Value برنمی‌گردانند.

آن‌ها می‌توانند یکی از Operandهای خود را به‌عنوان نتیجه Expression برگردانند.

به همین دلیل می‌توان از آن‌ها برای ساخت الگوهای عملی استفاده کرد:

```javascript
condition && action();
```

برای Conditional Execution،

```javascript
value || defaultValue;
```

برای Default Value بر اساس Truthiness،

و:

```javascript
value ?? defaultValue;
```

برای Default Value فقط در صورت `null` یا `undefined`.

تفاوت `||` و `??` بسیار مهم است.

`||` تمام Falsy Values را در تصمیم خود در نظر می‌گیرد، در حالی که `??` فقط `null` و `undefined` را به‌عنوان نبود مقدار در نظر می‌گیرد.

در نهایت، هدف استفاده از Logical Patterns صرفاً کوتاه کردن کد نیست.

هدف، بیان روشن و دقیق منطق Application است.

---

# Key Takeaways

* Logical Operators در JavaScript فقط Boolean تولید نمی‌کنند.
* `&&`، `||` و `??` می‌توانند یکی از Operandهای خود را برگردانند.
* **Short Circuit Evaluation** یعنی وقتی نتیجه Expression از قبل مشخص باشد، ادامه ارزیابی انجام نمی‌شود.
* `&&` در صورت Falsy بودن Operand سمت چپ متوقف می‌شود.
* `&&` در صورت Truthy بودن Operand سمت چپ، Operand سمت راست را ارزیابی می‌کند.
* `||` در صورت Truthy بودن Operand سمت چپ متوقف می‌شود.
* `||` در صورت Falsy بودن Operand سمت چپ، Operand سمت راست را ارزیابی می‌کند.
* `??` فقط در صورت `null` یا `undefined` بودن Operand سمت چپ به سمت راست می‌رود.
* Truthy و Falsy بودن با Type `boolean` یکسان نیست.
* `&&` برای Conditional Execution و Guardهای ساده کاربرد دارد.
* `||` می‌تواند برای Default Value استفاده شود.
* `??` برای Default Valueهایی مناسب است که فقط `null` و `undefined` باید باعث فعال شدن آن‌ها شوند.
* اگر `0`، `false` یا `''` مقدار معتبر باشند، استفاده از `||` ممکن است نتیجه ناخواسته ایجاد کند.
* `??` برای حفظ Falsy Valueهای معتبر مناسب است.
* کوتاه‌تر بودن Logical Expression به‌تنهایی معیار بهتر بودن آن نیست.
* برای منطق پیچیده، `if` معمولاً از Chainهای طولانی Logical خواناتر است.
* در ترکیب `??` با `&&` یا `||` باید از Parentheses برای مشخص کردن ترتیب موردنظر استفاده کرد.
* Optional Chaining نیز از ایده توقف زودهنگام در دسترسی استفاده می‌کند.
* انتخاب Logical Operator باید بر اساس معنای داده و Business Logic انجام شود.

---

# Technical Interview

## Junior

### 1. Short Circuiting چیست؟

**پاسخ:**

Short Circuiting رفتاری است که در آن JavaScript وقتی بتواند نتیجه یک Logical Expression را بدون ارزیابی بخش باقی‌مانده مشخص کند، ادامه Expression را ارزیابی نمی‌کند.

---

### 2. رفتار `&&` در Short Circuiting چگونه است؟

**پاسخ:**

در `A && B`، اگر `A` Falsy باشد، `B` ارزیابی نمی‌شود و `A` برگردانده می‌شود. اگر `A` Truthy باشد، `B` ارزیابی شده و نتیجه آن برگردانده می‌شود.

---

### 3. رفتار `||` چگونه است؟

**پاسخ:**

در `A || B`، اگر `A` Truthy باشد، `B` ارزیابی نمی‌شود و `A` برگردانده می‌شود. اگر `A` Falsy باشد، `B` ارزیابی شده و نتیجه آن برگردانده می‌شود.

---

### 4. آیا `&&` و `||` همیشه `true` یا `false` برمی‌گردانند؟

**پاسخ:**

خیر. در JavaScript آن‌ها می‌توانند یکی از Operandهای خود را برگردانند.

مثلاً:

```javascript
const result = 'Hello' && 'World';

console.log(result); // "World"
```

---

### 5. `??` چه تفاوتی با `||` دارد؟

**پاسخ:**

`||` برای تصمیم‌گیری تمام Falsy Values را در نظر می‌گیرد، اما `??` فقط `null` و `undefined` را Nullish در نظر می‌گیرد.

```javascript
0 || 10; // 10
0 ?? 10; // 0
```

---

### 6. چرا از `&&` برای Conditional Execution استفاده می‌کنیم؟

**پاسخ:**

زیرا اگر شرط سمت چپ Falsy باشد، Expression سمت راست اصلاً ارزیابی نمی‌شود.

```javascript
isLoggedIn && showDashboard();
```

در نتیجه `showDashboard()` فقط زمانی اجرا می‌شود که `isLoggedIn` Truthy باشد.

---

### 7. یک کاربرد واقعی `||` چیست؟

**پاسخ:**

استفاده از Default Value زمانی که هر Value Falsy باید به‌عنوان مقدار نامعتبر یا موجودنبودن در نظر گرفته شود.

```javascript
const name = inputName || 'Guest';
```

---

### 8. یک کاربرد واقعی `??` چیست؟

**پاسخ:**

زمانی که فقط `null` و `undefined` باید باعث استفاده از Default Value شوند.

```javascript
const timeout = config.timeout ?? 5000;
```

---

## Mid-Level

### 9. چرا این دو کد رفتار متفاوتی دارند؟

```javascript
const value = 0;

const a = value || 10;
const b = value ?? 10;
```

**پاسخ:**

چون `0` Falsy است، `||` آن را کنار می‌گذارد و `10` را برمی‌گرداند.

اما `0` Nullish نیست، بنابراین `??` خود `0` را حفظ می‌کند.

```text
value || 10 → 10
value ?? 10 → 0
```

---

### 10. در Expression زیر چه اتفاقی می‌افتد؟

```javascript
const result = user && user.name;
```

**پاسخ:**

اگر `user` Falsy باشد، همان `user` برگردانده می‌شود و `user.name` ارزیابی نمی‌شود.

اگر `user` Truthy باشد، `user.name` ارزیابی شده و نتیجه آن برگردانده می‌شود.

---

### 11. چرا Short Circuiting فقط یک ویژگی Performance نیست؟

**پاسخ:**

زیرا Short Circuiting مستقیماً رفتار برنامه را تغییر می‌دهد.

برای مثال:

```javascript
isReady && startApp();
```

در اینجا توقف ارزیابی باعث می‌شود `startApp()` اصلاً اجرا نشود.

بنابراین Short Circuiting بخشی از **Control Flow یک Expression** است، نه صرفاً یک Optimization.

---

### 12. چه زمانی `&&` بهتر است و چه زمانی `if`؟

**پاسخ:**

برای یک شرط و Action ساده، `&&` می‌تواند خوانا باشد:

```javascript
isLoggedIn && showDashboard();
```

اما وقتی شرط‌ها یا Actionها پیچیده می‌شوند، `if` معمولاً Intent را واضح‌تر بیان می‌کند.

---

### 13. چرا این الگو می‌تواند مشکل‌ساز باشد؟

```javascript
const label = isAdmin && 'Admin' || 'User';
```

**پاسخ:**

این الگو به Truthiness Value میانی وابسته است.

اگر Value مربوط به بخش اول Falsy باشد، `||` به مقدار بعدی می‌رود.

برای انتخاب مستقیم میان دو Value بر اساس یک Condition، Conditional Operator معمولاً واضح‌تر است:

```javascript
const label = isAdmin ? 'Admin' : 'User';
```

---

### 14. آیا `??` نیز Short Circuit می‌کند؟

**پاسخ:**

بله.

در:

```javascript
const result = value ?? getDefault();
```

اگر `value` نه `null` باشد و نه `undefined`، `getDefault()` ارزیابی نمی‌شود.

---

### 15. چرا ترکیب `??` و `||` نیازمند Parentheses است؟

**پاسخ:**

زیرا JavaScript ترکیب مستقیم آن‌ها را بدون مشخص کردن گروه‌بندی اجازه نمی‌دهد.

بنابراین باید Intent را صریح کنیم:

```javascript
(a ?? b) || c;
```

یا:

```javascript
a ?? (b || c);
```

این دو Expression می‌توانند نتایج متفاوتی داشته باشند.

---

### 16. آیا `&&` و `||` فقط برای Boolean Logic طراحی شده‌اند؟

**پاسخ:**

خیر.

در JavaScript علاوه بر Boolean Logic، از آن‌ها برای کنترل ارزیابی Expression، Conditional Execution، Guard Pattern و انتخاب Value نیز استفاده می‌شود.

---

## Senior

### 17. مدل ذهنی دقیق برای `&&`، `||` و `??` چیست؟

**پاسخ:**

هر سه Operator ابتدا Operand سمت چپ را ارزیابی می‌کنند و سپس بر اساس نوع Operator تصمیم می‌گیرند آیا Operand سمت راست باید ارزیابی شود یا خیر.

```text
A && B
A Falsy     → A
A Truthy    → B

A || B
A Truthy    → A
A Falsy     → B

A ?? B
A non-nullish → A
A nullish     → B
```

بنابراین Logical Operators را باید به‌عنوان **Value-producing Operators با Short-Circuit Evaluation** در نظر گرفت، نه صرفاً Operatorsی که Boolean تولید می‌کنند.

---

### 18. چرا تفاوت `||` و `??` از نظر طراحی Application مهم است؟

**پاسخ:**

زیرا این دو Operator درباره مفهوم «وجود داشتن Value» فرض متفاوتی دارند.

`||` می‌گوید:

> اگر Value در Boolean Context قابل قبول نیست، از Default استفاده کن.

`??` می‌گوید:

> فقط اگر Value واقعاً `null` یا `undefined` است، از Default استفاده کن.

بنابراین اگر Valueهایی مانند `0`، `false` یا `''` از نظر Domain معتبر باشند، `??` معمولاً انتخاب دقیق‌تری است.

---

### 19. آیا Short Circuiting را می‌توان Control Flow دانست؟

**پاسخ:**

بله، در سطح Expression می‌تواند بخشی از Control Flow باشد.

مثلاً:

```javascript
condition && action();
```

با تعیین اینکه آیا سمت راست ارزیابی شود یا خیر، مستقیماً بر اجرای برنامه اثر می‌گذارد.

اما این رفتار را نباید با ساختارهای مستقل Control Flow مانند `if` یکسان دانست؛ Logical Operators همچنان Expression تولید می‌کنند.

---

### 20. چرا این کد از نظر مهندسی همیشه مناسب نیست؟

```javascript
a && b && c && d && execute();
```

**پاسخ:**

چون اگر تعداد شروط زیاد شود یا هر شرط منطق پیچیده‌ای داشته باشد، خوانایی و قابلیت Debug کاهش پیدا می‌کند.

Short Circuiting باید برای بیان واضح یک الگوی ساده استفاده شود، نه برای فشرده کردن منطق پیچیده.

در چنین شرایطی، `if` یا استخراج Conditionها به Variableهای معنادار می‌تواند انتخاب بهتری باشد.

---

### 21. چرا Logical Operators می‌توانند Value برگردانند؟

**پاسخ:**

زیرا در JavaScript، `&&`، `||` و `??` برای تعیین نتیجه خود الزاماً به تبدیل نهایی Operandها به Boolean نیاز ندارند.

آن‌ها از وضعیت Operand برای تصمیم‌گیری درباره ادامه ارزیابی استفاده می‌کنند و سپس یکی از Operandها را به‌عنوان نتیجه Expression برمی‌گردانند.

---

### 22. رابطه Truthy/Falsy و Short Circuiting چیست؟

**پاسخ:**

Truthy/Falsy بودن مشخص می‌کند Operand در یک Boolean Context چگونه ارزیابی شود.

Short Circuiting از همین نتیجه برای تصمیم‌گیری درباره ادامه ارزیابی استفاده می‌کند.

برای `&&`، Falsy بودن سمت چپ باعث توقف می‌شود.

برای `||`، Truthy بودن سمت چپ باعث توقف می‌شود.

برای `??`، معیار `null` یا `undefined` بودن است، نه Truthy/Falsy بودن.

---

### 23. چرا `0 ?? 10` برابر `0` است اما `0 || 10` برابر `10`؟

**پاسخ:**

زیرا:

```javascript
0
```

Falsy است، اما Nullish نیست.

بنابراین:

```javascript
0 || 10
```

به سمت راست می‌رود.

اما:

```javascript
0 ?? 10
```

چون `0` نه `null` است و نه `undefined`، همان `0` را برمی‌گرداند.

---

### 24. در یک Code Review چگونه استفاده مناسب از Short Circuiting را ارزیابی می‌کنید؟

**پاسخ:**

سه سؤال اصلی می‌پرسم:

1. آیا منطق Expression بلافاصله قابل فهم است؟
2. آیا Operator با معنای Domain داده سازگار است؟
3. آیا کوتاه‌نویسی باعث کاهش خوانایی یا افزایش احتمال Bug شده است؟

اگر پاسخ منفی باشد، ساختار واضح‌تری مانند `if`، Conditional Operator یا Variableهای میانی انتخاب می‌کنم.

---

# Golden Answers

## Junior Golden Answer

> **Short Circuiting چیست؟**

**پاسخ طلایی:**

Short Circuiting یعنی JavaScript وقتی نتیجه یک Logical Expression را بتواند بدون ارزیابی بخش باقی‌مانده مشخص کند، ارزیابی را متوقف می‌کند. در `&&`، Falsy بودن سمت چپ باعث توقف و در `||`، Truthy بودن سمت چپ باعث توقف می‌شود.

---

## Mid-Level Golden Answer

> **تفاوت `||` و `??` چیست؟**

**پاسخ طلایی:**

`||` تمام Falsy Values را در تصمیم‌گیری خود در نظر می‌گیرد، در حالی که `??` فقط `null` و `undefined` را به‌عنوان نبود مقدار در نظر می‌گیرد.

```javascript
0 || 10; // 10
0 ?? 10; // 0
```

بنابراین اگر `0`، `false` یا `''` مقدار معتبر باشند، `??` معمولاً انتخاب مناسب‌تری برای Default Value است.

---

## Senior Golden Answer

> **Logical Operators در JavaScript چگونه جریان ارزیابی Expression را کنترل می‌کنند؟**

**پاسخ طلایی:**

در JavaScript، `&&`، `||` و `??` علاوه بر Boolean Logic، **Short-Circuit Evaluation** انجام می‌دهند. ابتدا Operand سمت چپ ارزیابی می‌شود و بر اساس نتیجه، مشخص می‌شود آیا Operand سمت راست باید ارزیابی شود یا خیر.

در `A && B`، اگر `A` Falsy باشد، `A` نتیجه Expression است و `B` اصلاً ارزیابی نمی‌شود.

در `A || B`، اگر `A` Truthy باشد، `A` نتیجه Expression است و `B` ارزیابی نمی‌شود.

در `A ?? B`، اگر `A` نه `null` باشد و نه `undefined`، همان `A` برگردانده می‌شود؛ در غیر این صورت `B` ارزیابی می‌شود.

بنابراین این Operators را نباید صرفاً Boolean Operators دانست؛ آن‌ها **Value-producing Expressions با Short-Circuit Evaluation** هستند و می‌توانند برای Conditional Execution، Guard Patterns و Default Values استفاده شوند.

---

# Conclusion

در این فصل یک تفاوت مهم میان **Boolean Logic** و **Logical Evaluation** را یاد گرفتیم.

Logical Operators در JavaScript فقط درباره `true` و `false` تصمیم نمی‌گیرند.

آن‌ها درباره **ادامه یا توقف ارزیابی Expression** نیز تصمیم می‌گیرند.

مدل ذهنی اصلی این فصل چنین است:

```text
Operand
   ↓
Evaluate
   ↓
Can result be determined?
   ↓
Yes → Stop
No  → Continue
```

و سه Operator اصلی این فصل قواعد متفاوتی دارند:

```text
&&  → stop on Falsy
||  → stop on Truthy
??  → stop on non-nullish
```

از این رفتار می‌توان برای ساخت الگوهای واقعی استفاده کرد:

```javascript
isReady && startApp();
```

```javascript
const name = username || 'Guest';
```

```javascript
const timeout = config.timeout ?? 5000;
```

اما استفاده حرفه‌ای از این Operators به کوتاه کردن کد محدود نمی‌شود.

مهندس خوب ابتدا معنای داده و منطق Application را مشخص می‌کند و سپس Operator مناسب را انتخاب می‌کند.

در نتیجه:

> **Short Circuiting یک ترفند Syntax نیست؛ یک مدل ارزیابی Expression است که می‌تواند بخشی از منطق برنامه را کنترل کند.**
