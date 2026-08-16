# Chapter 10 — Developer Tools and Debugging

---

# Chapter Goal

پس از مطالعه این فصل انتظار می‌رود بتوانید:

* مفهوم **Developer Tools** را به‌عنوان ابزار اصلی مشاهده و بررسی رفتار برنامه در Browser توضیح دهید.
* نقش **Console**، **Elements**، **Sources** و **Network** را در فرآیند توسعه و Debugging تشخیص دهید.
* پیام‌های Console و Errorهای JavaScript را به‌درستی تفسیر کنید.
* مفهوم **Stack Trace** را در سطح موردنیاز برای Debugging درک کنید.
* با `console.log()`، `console.warn()`، `console.error()` و `console.table()` برای مشاهده داده‌ها کار کنید.
* مفهوم **Breakpoint** را درک کرده و اجرای برنامه را مرحله‌به‌مرحله بررسی کنید.
* تفاوت **Step Over**، **Step Into**، **Step Out** و **Resume** را توضیح دهید.
* مقدار Variableها را در **Scope** و **Watch** بررسی کنید.
* تفاوت **Error** و **Bug** را تشخیص دهید.
* برای پیدا کردن خطا از یک Workflow منظم Debugging استفاده کنید.
* بدانید چه زمانی Console و چه زمانی Breakpoint ابزار مناسب‌تری است.
* از Debugging به‌عنوان یک فرآیند مهندسی، نه صرفاً چاپ کردن مقدار متغیرها، استفاده کنید.

---

# Core Question

> **چگونه می‌توان اجرای واقعی JavaScript را مشاهده، بررسی و Debug کرد؟**

---

# Concept Flow

```text
Browser
↓
Developer Tools
↓
Console
↓
Errors
↓
Sources
↓
Debugger
↓
Breakpoint
↓
Scope
↓
Call Stack
↓
Watch
↓
Network
↓
Debugging Workflow
```

---

# مقدمه

تا اینجا بیشتر مفاهیم JavaScript را از روی کد بررسی کرده‌ایم.

می‌دانیم Variable چیست.

می‌دانیم Operator چگونه کار می‌کند.

می‌توانیم با `if` تصمیم بگیریم و با Loopها اجرای تکراری ایجاد کنیم.

اما یک مسئله مهم باقی می‌ماند.

اگر برنامه برخلاف انتظار ما رفتار کند، چگونه بفهمیم مشکل دقیقاً کجاست؟

فرض کنید مقدار نهایی سبد خرید اشتباه است.

یا یک Button کلیک می‌شود اما نتیجه مورد انتظار ایجاد نمی‌شود.

یا برنامه بدون هیچ خروجی مشخصی متوقف می‌شود.

در چنین شرایطی نگاه کردن ساده به Source Code همیشه کافی نیست.

ما باید بتوانیم **رفتار واقعی برنامه هنگام اجرا** را مشاهده کنیم.

اینجاست که **Developer Tools** اهمیت پیدا می‌کند.

Developer Tools مجموعه‌ای از ابزارهای موجود در Browser است که به توسعه‌دهنده اجازه می‌دهد وضعیت برنامه، خروجی‌ها، Errorها، Source Code و در صورت نیاز Network Requests را هنگام اجرای واقعی بررسی کند.

بنابراین Debugging فقط پیدا کردن یک خط اشتباه نیست.

Debugging یعنی:

> **جمع‌آوری اطلاعات درباره رفتار واقعی برنامه و استفاده از آن اطلاعات برای پیدا کردن علت مشکل.**

---

# Block 01 — Developer Tools

## Developer Tools چیست؟

### تعریف ساده

**Developer Tools** یا به اختصار **DevTools** مجموعه ابزارهایی است که Browser برای توسعه، بررسی و Debug کردن Web Application در اختیار توسعه‌دهنده قرار می‌دهد.

این ابزارها به ما اجازه می‌دهند برنامه را فقط به‌عنوان یک فایل Source Code نبینیم؛ بلکه اجرای واقعی آن را نیز مشاهده کنیم.

---

### تعریف فنی

Developer Tools یک محیط توسعه و بررسی داخلی Browser است که مجموعه‌ای از قابلیت‌ها برای مشاهده و تحلیل Web Page و رفتار Application فراهم می‌کند.

در این فصل مهم‌ترین بخش‌های آن عبارت‌اند از:

* Console
* Elements
* Sources
* Network

هر بخش برای نوع خاصی از مسئله طراحی شده است.

---

## چرا Developer Tools مهم است؟

فرض کنید برنامه‌ای دارید که قیمت نهایی یک Order را محاسبه می‌کند.

```javascript
const price = 120;
const quantity = 2;

const total = price * quantity;

console.log(total);
```

اگر نتیجه اشتباه باشد، اولین قدم این نیست که کل برنامه را دوباره بنویسیم.

باید ابتدا ببینیم:

* `price` چه مقداری دارد؟
* `quantity` چه مقداری دارد؟
* Expression چگونه ارزیابی شده است؟
* آیا Error رخ داده است؟
* اجرای برنامه در کدام بخش با انتظار ما متفاوت شده است؟

DevTools امکان مشاهده همین اطلاعات را فراهم می‌کند.

---

## Console

**Console** یکی از پرکاربردترین بخش‌های Developer Tools است.

از Console می‌توان برای:

* مشاهده خروجی برنامه
* بررسی مقدار Variableها
* مشاهده Warningها
* مشاهده Errorها
* اجرای Expressionهای ساده JavaScript

استفاده کرد.

برای مثال:

```javascript
const cartTotal = 250;

console.log(cartTotal);
```

مقدار `250` در Console نمایش داده می‌شود.

---

## Elements

بخش **Elements** برای مشاهده ساختار HTML و وضعیت Elementهای صفحه استفاده می‌شود.

برای مثال، اگر صفحه شامل این HTML باشد:

```html
<button>Buy</button>
```

می‌توان Element مربوط به Button را در DevTools مشاهده کرد.

Elements برای بررسی مشکلات مربوط به ساختار صفحه و رفتار ظاهری آن بسیار مفید است.

در این فصل تمرکز اصلی ما روی JavaScript Debugging است؛ بنابراین جزئیات DOM و تغییر ساختار Page را به مباحث مربوط به Browser و DOM موکول می‌کنیم.

---

## Sources

بخش **Sources** محلی است که می‌توان Source Fileهای Application را مشاهده و اجرای JavaScript را Debug کرد.

در این بخش می‌توان:

* فایل JavaScript را مشاهده کرد.
* Breakpoint قرار داد.
* اجرای برنامه را متوقف کرد.
* مقدار Variableها را بررسی کرد.
* اجرای کد را مرحله‌به‌مرحله دنبال کرد.

بنابراین اگر Console برای مشاهده خروجی مناسب باشد، **Sources محیط اصلی Debugging تعاملی** است.

---

## Network

بخش **Network** برای مشاهده ارتباطات Networkی Page استفاده می‌شود.

برای مثال می‌توان بررسی کرد که آیا Browser درخواستی به یک Server ارسال کرده است یا خیر.

در این مرحله فقط باید نقش کلی Network را بشناسیم:

> Network به ما کمک می‌کند ببینیم Browser چه Requestهایی ارسال و چه Responseهایی دریافت می‌کند.

جزئیات HTTP، Request، Response و Fetch در بخش‌های بعدی کتاب بررسی خواهند شد.

---

## یک مدل ذهنی ساده

می‌توان DevTools را مانند مجموعه‌ای از ابزارهای بررسی در نظر گرفت:

```text
Console
→ چه چیزی در برنامه اتفاق افتاده است؟

Sources
→ کد در کجا و چگونه اجرا می‌شود؟

Elements
→ ساختار صفحه چگونه است؟

Network
→ Browser با چه منابع یا Serverهایی ارتباط برقرار می‌کند؟
```

این نگاه باعث می‌شود به جای باز کردن تصادفی DevTools، ابتدا نوع مشکل را مشخص کنیم و سپس ابزار مناسب را انتخاب کنیم.

---

## اشتباهات رایج

❌ تصور اینکه DevTools فقط برای نمایش Error است.

✔ DevTools برای مشاهده، بررسی و Debug کردن رفتار Application استفاده می‌شود.

---

❌ استفاده از Console برای تمام مشکلات.

✔ Console ابزار بسیار مفیدی است، اما برای بررسی جریان اجرای کد، Breakpointها معمولاً اطلاعات بیشتری ارائه می‌کنند.

---

❌ ورود زودهنگام به جزئیات Network در یک Bug معمولی JavaScript.

✔ ابتدا نوع مشکل را مشخص کنید و سپس ابزار مناسب را انتخاب کنید.

---

## نکات مهم

* DevTools یکی از مهم‌ترین ابزارهای روزمره Frontend Developer است.
* Console برای مشاهده خروجی و پیام‌های برنامه کاربرد دارد.
* Sources محیط اصلی Debugging کد JavaScript است.
* Elements برای بررسی ساختار Page کاربرد دارد.
* Network برای مشاهده ارتباطات Networkی استفاده می‌شود.
* انتخاب ابزار مناسب، بخشی از مهارت Debugging است.

---

# Block 02 — Console

Console معمولاً اولین جایی است که هنگام بررسی یک مشکل JavaScript به آن مراجعه می‌کنیم.

اما استفاده حرفه‌ای از Console فقط به `console.log()` محدود نمی‌شود.

برای Debugging مؤثر باید بدانیم:

* چه چیزی را چاپ کنیم؟
* چرا آن را چاپ می‌کنیم؟
* پیام چه اطلاعاتی به ما می‌دهد؟
* آیا پیام واقعاً علت مشکل را نشان می‌دهد یا فقط یک علامت از آن است؟

---

# `console.log()`

### تعریف ساده

`console.log()` برای نمایش اطلاعات در Console استفاده می‌شود.

```javascript
const product = 'Laptop';

console.log(product);
```

خروجی:

```text
Laptop
```

---

### تعریف فنی

`console.log()` متدی از **Console API** است که مقدار یا مقادیر ارسال‌شده را برای مشاهده در Developer Console ثبت می‌کند.

---

## چرا مهم است؟

گاهی یک Variable در Source Code مقدار مشخصی دارد، اما هنگام اجرای برنامه مقدار دیگری دریافت می‌کند.

در این حالت می‌توان مقدار واقعی آن را مشاهده کرد.

```javascript
const quantity = 3;

console.log('quantity:', quantity);
```

خروجی می‌تواند چیزی شبیه این باشد:

```text
quantity: 3
```

در این مثال، علاوه بر مقدار، نام Variable نیز نمایش داده شده است.

این کار در Debugging بسیار مفیدتر از چاپ یک مقدار بدون Context است.

---

## مثال عملی

فرض کنید قیمت نهایی اشتباه محاسبه شده است:

```javascript
const price = 80;
const quantity = 3;

const total = price + quantity;

console.log('price:', price);
console.log('quantity:', quantity);
console.log('total:', total);
```

با مشاهده Console متوجه می‌شویم:

```text
price: 80
quantity: 3
total: 83
```

مشکل اکنون قابل مشاهده است.

ما انتظار داشتیم قیمت در تعداد ضرب شود، اما از `+` استفاده کرده‌ایم.

Debugging در اینجا باعث شد به جای حدس زدن، وضعیت واقعی برنامه را مشاهده کنیم.

---

# `console.warn()`

برای نمایش یک Warning می‌توان از:

```javascript
console.warn()
```

استفاده کرد.

```javascript
const stock = 2;

if (stock < 5) {
  console.warn('Low stock');
}
```

این پیام نشان می‌دهد وضعیت برنامه لزوماً Error نیست، اما شرایطی وجود دارد که ممکن است نیاز به توجه داشته باشد.

---

# `console.error()`

برای ثبت یک Error می‌توان از:

```javascript
console.error()
```

استفاده کرد.

```javascript
const user = null;

if (!user) {
  console.error('User data is missing');
}
```

این روش برای برجسته کردن یک وضعیت خطا در Console مفید است.

البته باید توجه کنیم:

> `console.error()` به‌خودی‌خود مشکل برنامه را حل نمی‌کند.

فقط اطلاعاتی درباره وضعیت مشکل ثبت می‌کند.

---

# `console.table()`

هنگامی که داده ساختاریافته‌ای مانند Array از Objectها داریم، `console.table()` می‌تواند خوانایی بیشتری ایجاد کند.

```javascript
const products = [
  { name: 'Laptop', price: 1200 },
  { name: 'Mouse', price: 40 },
];

console.table(products);
```

Browser داده‌ها را به شکلی جدولی نمایش می‌دهد.

این روش برای بررسی مجموعه‌ای از داده‌ها معمولاً از چندین `console.log()` خواناتر است.

---

# Error Messages

یکی از مهم‌ترین مهارت‌های Debugging، **خواندن Error Message** است.

فرض کنید کد زیر اجرا شود:

```javascript
const user = null;

console.log(user.name);
```

Browser ممکن است Errorای مانند این نمایش دهد:

```text
TypeError: Cannot read properties of null
```

پیام Error به ما می‌گوید که برنامه در زمان اجرا تلاش کرده است Propertyای را از یک مقدار `null` بخواند.

بنابراین Error Message فقط یک پیام ترسناک نیست.

آن پیام یک **سرنخ فنی** درباره علت مشکل است.

---

# Stack Trace

در بسیاری از Errorها Browser اطلاعات بیشتری درباره محل وقوع مشکل ارائه می‌کند.

به این اطلاعات **Stack Trace** گفته می‌شود.

Stack Trace معمولاً نشان می‌دهد:

* Error در کجا رخ داده است.
* کدام فایل درگیر بوده است.
* شماره خط یا موقعیت مربوطه چیست.
* مسیر اجرای Functionها چگونه به این نقطه رسیده است.

برای مثال:

```text
TypeError: Cannot read properties of null
    at calculateTotal (cart.js:12)
    at updateCart (cart.js:28)
```

در اینجا می‌توانیم ببینیم Error در `calculateTotal` و در خط مشخصی از فایل رخ داده است.

در فصل‌های آینده، **Call Stack** به‌صورت مستقل بررسی خواهد شد.

در این فصل تنها کافی است بدانیم Stack Trace یکی از مهم‌ترین سرنخ‌های Debugging است.

---

# چگونه یک Error را بخوانیم؟

به جای اینکه فقط به اولین خط پیام نگاه کنیم، اطلاعات آن را به بخش‌های کوچک‌تر تقسیم کنید.

مثلاً:

```text
TypeError
```

نوع Error را مشخص می‌کند.

```text
Cannot read properties of null
```

مشکل احتمالی را توضیح می‌دهد.

```text
cart.js:12
```

محل وقوع مشکل را مشخص می‌کند.

بنابراین می‌توانیم از این الگو استفاده کنیم:

```text
What happened?
↓
Why?
↓
Where?
```

یعنی:

```text
چه نوع خطایی رخ داده؟
↓
چه چیزی اشتباه است؟
↓
در کجا رخ داده است؟
```

---

# Console همیشه علت اصلی Bug را نشان نمی‌دهد

این نکته بسیار مهم است.

ممکن است برنامه هیچ Errorای ایجاد نکند اما رفتار آن اشتباه باشد.

مثلاً:

```javascript
const price = 100;
const quantity = 2;

const total = price + quantity;

console.log(total);
```

این کد ممکن است بدون Error اجرا شود.

اما:

```text
102
```

به‌جای:

```text
200
```

تولید می‌کند.

در اینجا **Runtime Error نداریم**.

اما **Bug داریم**.

بنابراین:

> نبودن Error به معنای درست بودن برنامه نیست.

---

## اشتباهات رایج

❌ تصور اینکه `console.log()` ابزار کامل Debugging است.

✔ Console فقط یکی از ابزارهای Debugging است.

---

❌ حذف کردن Error Message بدون خواندن آن.

✔ Error Message و Stack Trace را به‌عنوان سرنخ بررسی کنید.

---

❌ تصور اینکه هر Bug باید Error ایجاد کند.

✔ Logical Bug ممکن است بدون هیچ Errorای برنامه را به نتیجه اشتباه برساند.

---

❌ چاپ کردن ده‌ها مقدار بدون مشخص کردن Context.

✔ پیام‌های Debugging باید مشخص و هدفمند باشند.

---

## نکات مهم

* `console.log()` برای مشاهده داده‌ها استفاده می‌شود.
* `console.warn()` برای Warningها مناسب است.
* `console.error()` برای نمایش وضعیت‌های خطا کاربرد دارد.
* `console.table()` برای داده‌های ساختاریافته خواناتر است.
* Error Message یک سرنخ مهم برای Debugging است.
* Stack Trace محل و مسیر رسیدن به Error را بهتر نشان می‌دهد.
* نبودن Error به معنای نبودن Bug نیست.

---

# Block 03 — Debugger

گاهی Console برای فهمیدن مشکل کافی نیست.

فرض کنید Function زیر چند مرحله محاسباتی دارد:

```javascript
const calculateTotal = function (price, quantity, discount) {
  const subtotal = price * quantity;
  const finalPrice = subtotal - discount;

  return finalPrice;
};
```

اگر خروجی اشتباه باشد، می‌توانیم همه Variableها را با `console.log()` چاپ کنیم.

اما راه بهتری نیز وجود دارد.

می‌توانیم اجرای برنامه را **متوقف کنیم** و وضعیت آن را دقیقاً در یک نقطه خاص بررسی کنیم.

این همان کاری است که **Debugger** انجام می‌دهد.

---

# Breakpoint چیست؟

### تعریف ساده

**Breakpoint** نقطه‌ای در Source Code است که در آن Browser اجرای JavaScript را موقتاً متوقف می‌کند.

---

### تعریف فنی

Breakpoint یک نقطه توقف در فرآیند Debugging است که باعث می‌شود اجرای برنامه هنگام رسیدن به آن نقطه متوقف شود تا وضعیت اجرای فعلی بررسی شود.

---

## چرا Breakpoint مهم است؟

فرض کنید:

```javascript
const calculateTotal = function (price, quantity) {
  const subtotal = price * quantity;
  const total = subtotal * 1.2;

  return total;
};
```

اگر Breakpoint را روی خط محاسبه `total` قرار دهیم، اجرای برنامه در همان نقطه متوقف می‌شود.

اکنون می‌توانیم بررسی کنیم:

```text
price
quantity
subtotal
```

چه مقادیری دارند.

به جای حدس زدن، **State برنامه در همان لحظه واقعی** را مشاهده می‌کنیم.

---

# قرار دادن Breakpoint

در DevTools معمولاً می‌توان در کنار شماره خط Source Code کلیک کرد تا Breakpoint ایجاد شود.

برای مثال:

```javascript
const subtotal = price * quantity;
const total = subtotal * 1.2;
```

اگر روی خط دوم Breakpoint قرار دهیم، اجرای برنامه هنگام رسیدن به آن خط متوقف می‌شود.

---

# Resume

پس از متوقف شدن برنامه، می‌توان اجرای آن را ادامه داد.

این عمل با **Resume** انجام می‌شود.

مدل ذهنی آن ساده است:

```text
Run
↓
Breakpoint
↓
Pause
↓
Inspect
↓
Resume
```

---

# Step Over

**Step Over** اجرای برنامه را به خط بعدی منتقل می‌کند، بدون اینکه وارد جزئیات اجرای Function فراخوانی‌شده شود.

فرض کنید:

```javascript
const total = calculateTotal(price, quantity);
```

اگر روی این خط Step Over انجام دهیم، Debugger اجرای Function را به‌صورت داخلی دنبال نمی‌کند و پس از پایان آن به خط بعدی می‌رود.

این گزینه زمانی مفید است که:

> Function موردنظر برای مسئله فعلی مهم نیست و فقط می‌خواهیم اجرای برنامه را ادامه دهیم.

---

# Step Into

**Step Into** به Debugger اجازه می‌دهد وارد Functionای شود که در خط فعلی فراخوانی شده است.

برای مثال:

```javascript
const total = calculateTotal(price, quantity);
```

با Step Into می‌توانیم وارد:

```javascript
calculateTotal()
```

شویم و اجرای داخلی آن را خط‌به‌خط بررسی کنیم.

---

# Step Out

اگر داخل یک Function هستیم اما متوجه شویم جزئیات آن برای مسئله فعلی لازم نیست، می‌توانیم با **Step Out** از Function خارج شویم.

به این ترتیب اجرای باقی‌مانده Function انجام شده و Debugger به سطح قبلی بازمی‌گردد.

---

# تفاوت Step Over، Step Into و Step Out

می‌توان آن‌ها را این‌گونه به خاطر سپرد:

```text
Step Over
→ از Function عبور کن.

Step Into
→ وارد Function شو.

Step Out
→ از Function فعلی خارج شو.
```

و:

```text
Resume
→ اجرای برنامه را ادامه بده.
```

---

# Scope

هنگامی که اجرای برنامه متوقف شده است، یکی از مهم‌ترین اطلاعاتی که Debugger نمایش می‌دهد **Scope** است.

در این مرحله Scope را فقط از دید Debugging بررسی می‌کنیم.

فرض کنید:

```javascript
const calculateTotal = function (price, quantity) {
  const subtotal = price * quantity;

  return subtotal;
};
```

هنگامی که برنامه داخل Function متوقف شده است، Debugger می‌تواند Variableهایی را که در محدوده فعلی قابل دسترسی هستند نمایش دهد.

برای مثال:

```text
price
quantity
subtotal
```

این اطلاعات به ما اجازه می‌دهد وضعیت واقعی Variableها را در لحظه اجرای برنامه مشاهده کنیم.

مبحث Scope به‌عنوان یکی از مفاهیم اصلی JavaScript در فصل‌های آینده به‌صورت کامل بررسی خواهد شد.

---

# Call Stack

هنگامی که Debugger متوقف شده است، معمولاً اطلاعاتی درباره **Call Stack** نیز در اختیار ما قرار می‌دهد.

در این فصل لازم نیست سازوکار Call Stack را به‌صورت عمیق بررسی کنیم.

تنها مدل ذهنی زیر کافی است:

> Call Stack نشان می‌دهد اجرای فعلی از چه مسیر Functionهایی به نقطه‌ای که اکنون در آن متوقف شده‌ایم رسیده است.

فرض کنید:

```javascript
loadCart();
```

داخل آن:

```javascript
calculateTotal();
```

و داخل آن:

```javascript
formatPrice();
```

فراخوانی شود.

در زمان Debugging ممکن است مسیر اجرا به شکل مفهومی چنین باشد:

```text
formatPrice
↓
calculateTotal
↓
loadCart
```

این اطلاعات هنگام بررسی Errorهای پیچیده بسیار مفید است.

Call Stack در فصل مستقلی بعداً به‌صورت دقیق بررسی خواهد شد.

---

# Watch

گاهی یک Variable خاص برای ما اهمیت زیادی دارد.

در این حالت می‌توانیم آن Expression یا مقدار را تحت نظر بگیریم.

این کار با **Watch** انجام می‌شود.

برای مثال، اگر در حال بررسی محاسبه Order باشیم، ممکن است بخواهیم همیشه این Expression را بررسی کنیم:

```javascript
subtotal
```

یا:

```javascript
price * quantity
```

با Watch می‌توانیم مقدار موردنظر را در طول فرآیند Debugging تحت نظر داشته باشیم.

---

# Console در برابر Breakpoint

اکنون می‌توانیم دو ابزار مهم را مقایسه کنیم.

### Console

وقتی می‌خواهیم:

* مقدار مشخصی را مشاهده کنیم.
* یک وضعیت را ثبت کنیم.
* اطلاعات ساده‌ای از برنامه بگیریم.

مناسب است.

### Breakpoint

وقتی می‌خواهیم:

* اجرای برنامه را متوقف کنیم.
* State فعلی را بررسی کنیم.
* چند Variable را هم‌زمان ببینیم.
* جریان اجرای Functionها را دنبال کنیم.

مناسب‌تر است.

---

# یک مثال واقعی

فرض کنید:

```javascript
const calculateOrderTotal = function (price, quantity, discount) {
  const subtotal = price * quantity;
  const total = subtotal - discount;

  return total;
};

const result = calculateOrderTotal(100, 2, 30);

console.log(result);
```

خروجی:

```text
170
```

فرض کنید برنامه باید مقدار دیگری تولید کند.

به جای اضافه کردن `console.log()`های متعدد، می‌توانیم روی خط زیر Breakpoint قرار دهیم:

```javascript
const total = subtotal - discount;
```

سپس وضعیت را بررسی کنیم:

```text
price
100

quantity
2

discount
30

subtotal
200
```

اکنون می‌توانیم محاسبه را مستقیماً تحلیل کنیم.

این دقیقاً مزیت اصلی Debugger است:

> **به جای مشاهده فقط نتیجه، وضعیت برنامه را در لحظه اجرای آن مشاهده می‌کنیم.**

---

## اشتباهات رایج

❌ قرار دادن Breakpoint روی خطوط تصادفی.

✔ Breakpoint باید برای پاسخ دادن به یک سؤال مشخص استفاده شود.

---

❌ استفاده از Step Into برای تمام Functionها.

✔ فقط زمانی وارد Function شوید که منطق داخلی آن برای Bug مهم است.

---

❌ فراموش کردن اینکه برنامه در حالت Pause قرار دارد.

✔ پس از بررسی State، با Resume اجرای برنامه را ادامه دهید.

---

❌ بررسی یک Variable بدون توجه به Scope فعلی.

✔ ابتدا محدوده اجرای فعلی را در نظر بگیرید.

---

## نکات مهم

* Breakpoint اجرای برنامه را در یک نقطه مشخص متوقف می‌کند.
* Step Over از Function عبور می‌کند.
* Step Into وارد Function می‌شود.
* Step Out از Function فعلی خارج می‌شود.
* Resume اجرای برنامه را ادامه می‌دهد.
* Scope Variableهای قابل دسترسی در نقطه فعلی را نشان می‌دهد.
* Call Stack مسیر Functionهای فعال را نشان می‌دهد.
* Watch برای تحت نظر گرفتن Expressionهای مهم استفاده می‌شود.

---

# Block 04 — Debugging Workflow

تا اینجا با ابزارهای مختلف آشنا شدیم.

اما داشتن ابزار به‌تنهایی Debugger حرفه‌ای نمی‌سازد.

مشکل اصلی در بسیاری از موارد این نیست که توسعه‌دهنده ابزار Debugging را نمی‌شناسد.

مشکل این است که **بدون Workflow مشخص Debug می‌کند.**

---

# Error چیست؟

**Error** وضعیتی است که JavaScript یا محیط اجرای آن آن را به‌عنوان یک وضعیت خطا گزارش می‌کند.

برای مثال:

```javascript
const user = null;

console.log(user.name);
```

این کد هنگام اجرا Error ایجاد می‌کند.

در چنین شرایطی Browser اطلاعاتی مانند نوع Error و محل وقوع آن را در اختیار ما قرار می‌دهد.

---

# Bug چیست؟

**Bug** رفتار نادرست یا ناخواسته در برنامه است.

یک Bug الزاماً باعث Error نمی‌شود.

مثلاً:

```javascript
const price = 100;
const quantity = 2;

const total = price + quantity;
```

برنامه اجرا می‌شود.

اما اگر هدف ما محاسبه قیمت کل باشد، نتیجه:

```text
102
```

اشتباه است.

پس:

```text
Error
≠
Bug
```

ممکن است یک Bug باعث Error شود.

اما ممکن است Bug بدون هیچ Errorای نیز وجود داشته باشد.

---

# سه سؤال اصلی هنگام Debugging

هنگامی که برنامه درست کار نمی‌کند، ابتدا سه سؤال بپرسید:

### 1. چه چیزی اشتباه است؟

مثلاً:

```text
Total price is wrong.
```

### 2. کجا رفتار اشتباه رخ می‌دهد؟

مثلاً:

```text
calculateOrderTotal()
```

### 3. چرا این رفتار رخ می‌دهد؟

مثلاً:

```text
+ به جای * استفاده شده است.
```

این روش بهتر از این است که بدون هدف، بخش‌های مختلف کد را تغییر دهیم.

---

# Debugging از Result به Cause

یک اشتباه رایج این است که از همان ابتدا به دنبال خطی بگردیم که فکر می‌کنیم اشتباه است.

روش حرفه‌ای‌تر این است:

```text
Observed Behavior
↓
Evidence
↓
Possible Cause
↓
Verification
↓
Fix
```

ابتدا رفتار واقعی را مشاهده می‌کنیم.

سپس شواهد جمع می‌کنیم.

بعد علت احتمالی را مطرح می‌کنیم.

آن علت را بررسی می‌کنیم.

و در نهایت اصلاح را انجام می‌دهیم.

---

# Console یا Breakpoint؟

یک قانون ساده:

اگر سؤال شما این است:

> «این مقدار الان چیست؟»

Console معمولاً کافی است.

اگر سؤال شما این است:

> «برنامه چگونه از این خط به این وضعیت رسید؟»

Breakpoint و Debugger مناسب‌تر هستند.

برای مثال:

```javascript
console.log(total);
```

برای مشاهده مقدار `total` مناسب است.

اما اگر می‌خواهیم بفهمیم `total` چگونه از چند مرحله محاسباتی به این مقدار رسیده است، Breakpoint اطلاعات بیشتری ارائه می‌کند.

---

# Debugging Workflow پیشنهادی

یک Workflow ساده و قابل استفاده در پروژه‌های واقعی:

```text
1. Reproduce the Problem
↓
2. Observe the Behavior
↓
3. Read Errors
↓
4. Locate the Problem
↓
5. Inspect State
↓
6. Form a Hypothesis
↓
7. Verify the Hypothesis
↓
8. Fix the Cause
↓
9. Run Again
```

---

## 1. Reproduce the Problem

ابتدا باید بتوانید مشکل را دوباره ایجاد کنید.

اگر نمی‌دانیم Bug در چه شرایطی رخ می‌دهد، Debugging بسیار دشوارتر می‌شود.

---

## 2. Observe the Behavior

قبل از تغییر کد، ببینید دقیقاً چه اتفاقی می‌افتد.

برای مثال:

```text
Expected:
Total = 200

Actual:
Total = 102
```

این اطلاعات نقطه شروع Debugging است.

---

## 3. Read Errors

اگر Error وجود دارد:

* نوع Error را بخوانید.
* Message را بررسی کنید.
* محل وقوع را پیدا کنید.
* Stack Trace را بررسی کنید.

---

## 4. Locate the Problem

سعی کنید محدوده مشکل را کوچک کنید.

اگر Application صدها خط کد دارد، لازم نیست همه آن را بررسی کنید.

ممکن است بتوانید مشکل را به یک Function یا یک بخش کوچک از Logic محدود کنید.

---

## 5. Inspect State

اکنون مقدار Variableهای مهم را بررسی کنید.

می‌توانید از:

```javascript
console.log()
```

یا:

```text
Breakpoint
```

استفاده کنید.

---

## 6. Form a Hypothesis

بر اساس شواهد یک فرضیه بسازید.

مثلاً:

> احتمالاً `quantity` مقدار اشتباهی دریافت کرده است.

---

## 7. Verify the Hypothesis

اکنون آن فرضیه را بررسی کنید.

اگر:

```text
quantity = 2
```

باشد، فرضیه رد می‌شود.

اگر:

```text
quantity = undefined
```

باشد، احتمالاً به علت اصلی نزدیک شده‌ایم.

---

## 8. Fix the Cause

هدف Debugging حذف علامت مشکل نیست.

باید علت اصلی را اصلاح کنیم.

---

## 9. Run Again

پس از اصلاح، برنامه را دوباره اجرا کنید و مطمئن شوید:

* Bug برطرف شده است.
* رفتار مورد انتظار برقرار شده است.
* اصلاح جدید مشکل دیگری ایجاد نکرده است.

---

# Debugging حرفه‌ای یعنی جمع‌آوری Evidence

یکی از مهم‌ترین تغییرات ذهنی در Debugging این است:

> **حدس نزن؛ مشاهده کن.**

به جای اینکه بگوییم:

> «احتمالاً این Variable مشکل دارد.»

ابتدا مقدار آن را بررسی می‌کنیم.

به جای اینکه بگوییم:

> «احتمالاً Function درست اجرا نمی‌شود.»

اجرای آن را با Breakpoint بررسی می‌کنیم.

به جای اینکه بگوییم:

> «احتمالاً Browser درخواست را ارسال نکرده است.»

در صورت مرتبط بودن مشکل، Network را بررسی می‌کنیم.

Debugging در واقع فرآیند تبدیل:

```text
Assumption
```

به:

```text
Evidence
```

است.

---

# Common Mistakes

## تغییر دادن چند بخش کد به‌صورت هم‌زمان

اگر چند قسمت را هم‌زمان تغییر دهیم، مشخص نخواهد بود کدام تغییر مشکل را حل کرده است.

بهتر است:

```text
One hypothesis
↓
One change
↓
One verification
```

را دنبال کنیم.

---

## استفاده بیش از حد از `console.log()`

Console بسیار مفید است.

اما صدها پیام Debugging باعث می‌شود اطلاعات مهم در میان خروجی‌های زیاد گم شوند.

پیام‌های Debugging باید هدفمند باشند.

---

## حذف Error به جای حل علت

گاهی توسعه‌دهنده فقط بخشی از کد را تغییر می‌دهد تا Error دیگر نمایش داده نشود.

اما اگر رفتار برنامه همچنان اشتباه باشد، Bug حل نشده است.

هدف Debugging:

> **رفع علت مشکل، نه فقط حذف علامت آن.**

---

## Debugging بدون Reproduce

اگر نمی‌دانیم Bug چگونه ایجاد می‌شود، بررسی علت دشوار است.

بنابراین اولین قدم باید ایجاد دوباره مشکل باشد.

---

## استفاده از Breakpoint بدون سؤال مشخص

Breakpoint زمانی بیشترین ارزش را دارد که بدانیم:

> «می‌خواهم در این نقطه چه چیزی را بررسی کنم؟»

---

# Best Practices

### 1. ابتدا مشکل را تعریف کنید.

نگویید:

> «برنامه کار نمی‌کند.»

بگویید:

> «Total Order در Checkout اشتباه محاسبه می‌شود.»

---

### 2. Error Message را کامل بخوانید.

گاهی پاسخ اصلی همان‌جایی است که ابتدا نادیده گرفته می‌شود.

---

### 3. از کوچک‌ترین محدوده شروع کنید.

ابتدا Function یا Expression مرتبط را بررسی کنید.

---

### 4. از Evidence استفاده کنید.

مقدار Variableها و مسیر اجرای برنامه را بررسی کنید.

---

### 5. فرضیه خود را آزمایش کنید.

Debugging نباید تبدیل به حدس‌زدن شود.

---

### 6. علت را اصلاح کنید.

صرفاً Error Message را پنهان نکنید.

---

### 7. پس از اصلاح دوباره رفتار برنامه را بررسی کنید.

Fix بدون Verification کامل نیست.

---

# دیدگاه Jonas

در رویکرد آموزشی Jonas Schmedtmann، Debugging بخشی طبیعی از فرآیند برنامه‌نویسی است، نه نشانه شکست در نوشتن کد.

نگاه حرفه‌ای این است که وقتی برنامه رفتار مورد انتظار را ندارد، به جای حدس زدن، وضعیت برنامه را بررسی کنیم.

در عمل، ابزارهایی مانند Console و Debugger باعث می‌شوند بتوانیم این بررسی را سریع‌تر و دقیق‌تر انجام دهیم.

---

# اشتباهات رایج

❌ تصور اینکه Error و Bug یک مفهوم هستند.

✔ Error یک وضعیت خطا گزارش‌شده است؛ Bug رفتار نادرست برنامه است و ممکن است بدون Error رخ دهد.

---

❌ شروع Debugging با تغییر تصادفی کد.

✔ ابتدا مشکل را مشاهده و محدود کنید.

---

❌ اعتماد به حدس بدون بررسی State.

✔ مقدار Variableها و مسیر اجرای برنامه را بررسی کنید.

---

❌ تصور اینکه حذف Error یعنی Bug حل شده است.

✔ رفتار واقعی Application باید بعد از اصلاح دوباره بررسی شود.

---

# نکات مهم

* Debugging یک فرآیند حل مسئله است.
* Error و Bug یکسان نیستند.
* نبودن Error به معنای نبودن Bug نیست.
* Console برای مشاهده اطلاعات بسیار مفید است.
* Breakpoint برای بررسی اجرای واقعی برنامه مناسب است.
* Error Message و Stack Trace باید به‌عنوان Evidence بررسی شوند.
* Debugging حرفه‌ای بر مشاهده و Verification متکی است.
* هدف اصلی، پیدا کردن و اصلاح علت مشکل است.

---

# Summary

در این فصل با **Developer Tools** و نقش آن‌ها در Debugging JavaScript آشنا شدیم.

ابتدا دیدیم که Developer Tools مجموعه‌ای از ابزارهای Browser برای مشاهده و بررسی رفتار واقعی Application است.

سپس مهم‌ترین بخش‌های آن را بررسی کردیم:

* **Console**
* **Elements**
* **Sources**
* **Network**

در ادامه Console را دقیق‌تر بررسی کردیم و با ابزارهایی مانند:

```javascript
console.log()
console.warn()
console.error()
console.table()
```

آشنا شدیم.

همچنین یاد گرفتیم که Error Message و Stack Trace می‌توانند اطلاعات ارزشمندی درباره محل و نوع مشکل ارائه کنند.

در بخش Debugger، مفهوم **Breakpoint** را بررسی کردیم و دیدیم چگونه می‌توان اجرای برنامه را متوقف و وضعیت آن را در یک لحظه مشخص مشاهده کرد.

سپس با:

* Step Over
* Step Into
* Step Out
* Resume
* Scope
* Call Stack
* Watch

آشنا شدیم.

در پایان نیز Debugging را به‌عنوان یک **Workflow مهندسی** بررسی کردیم.

مهم‌ترین ایده این فصل این بود:

> **Debugging یعنی مشاهده رفتار واقعی برنامه، جمع‌آوری Evidence و پیدا کردن علت مشکل؛ نه حدس زدن و تغییر تصادفی کد.**

---

# Key Takeaways

در پایان این فصل باید بتوانید:

* Developer Tools را به‌عنوان ابزار اصلی بررسی Web Application توضیح دهید.
* تفاوت کاربرد Console، Sources، Elements و Network را بیان کنید.
* از `console.log()` برای بررسی مقدار Variableها استفاده کنید.
* از `console.warn()` و `console.error()` برای پیام‌های مناسب استفاده کنید.
* از `console.table()` برای مشاهده داده‌های ساختاریافته استفاده کنید.
* Error Message و Stack Trace را به‌عنوان سرنخ Debugging تحلیل کنید.
* تفاوت Error و Bug را توضیح دهید.
* Breakpoint قرار دهید و اجرای برنامه را متوقف کنید.
* تفاوت Step Over، Step Into و Step Out را توضیح دهید.
* اجرای برنامه را با Resume ادامه دهید.
* Variableهای موجود در Scope فعلی را بررسی کنید.
* Call Stack را در حد Debugging تفسیر کنید.
* Expressionهای مهم را با Watch تحت نظر بگیرید.
* برای Debugging یک Workflow مشخص داشته باشید.
* به جای حدس زدن، از Evidence برای پیدا کردن علت مشکل استفاده کنید.

---

# Technical Interview

## سطح Junior

### سؤال ۱

Developer Tools چیست و چرا برای Frontend Developer اهمیت دارد؟

---

### سؤال ۲

تفاوت Console و Sources چیست؟

---

### سؤال ۳

`console.log()` چه کاربردی دارد؟

---

### سؤال ۴

تفاوت `console.warn()` و `console.error()` چیست؟

---

### سؤال ۵

Breakpoint چیست؟

---

### سؤال ۶

تفاوت Step Over و Step Into چیست؟

---

### سؤال ۷

آیا هر Bug باعث ایجاد Error می‌شود؟

---

## سطح Mid-Level

### سؤال ۸

چرا Error Message و Stack Trace در Debugging اهمیت دارند؟

---

### سؤال ۹

چه زمانی استفاده از Breakpoint نسبت به `console.log()` مناسب‌تر است؟

---

### سؤال ۱۰

Scope در Debugger چه اطلاعاتی در اختیار توسعه‌دهنده قرار می‌دهد؟

---

### سؤال ۱۱

Call Stack در زمان Debugging چه چیزی را نشان می‌دهد؟

---

### سؤال ۱۲

چرا Debugging نباید بر اساس حدس و تغییر تصادفی کد انجام شود؟

---

### سؤال ۱۳

یک Workflow مناسب برای Debugging یک Bug منطقی چیست؟

---

## سطح Senior

### سؤال ۱۴

تفاوت Error و Bug را از دید مهندسی نرم‌افزار توضیح دهید.

---

### سؤال ۱۵

چرا «نبودن Runtime Error» نمی‌تواند نشان‌دهنده صحت یک Application باشد؟

---

### سؤال ۱۶

چرا جمع‌آوری Evidence قبل از تغییر Source Code اهمیت دارد؟

---

### سؤال ۱۷

چگونه تشخیص می‌دهید که یک Bug باید با Console بررسی شود یا با Breakpoint؟

---

### سؤال ۱۸

چرا اصلاح علامت یک مشکل بدون پیدا کردن Root Cause یک Debugging کامل محسوب نمی‌شود؟

---

### سؤال ۱۹

چگونه می‌توان Debugging را به یک فرآیند قابل تکرار و مهندسی‌شده تبدیل کرد؟

---

# Golden Answers

## Developer Tools چیست؟

Developer Tools مجموعه ابزارهای Browser برای مشاهده، بررسی و Debug کردن Web Application است. Console، Sources، Elements و Network هرکدام برای نوع متفاوتی از بررسی استفاده می‌شوند.

---

## Breakpoint چیست؟

Breakpoint نقطه‌ای در Source Code است که Browser هنگام رسیدن به آن اجرای JavaScript را متوقف می‌کند. این توقف به توسعه‌دهنده اجازه می‌دهد State و مسیر اجرای برنامه را بررسی کند.

---

## تفاوت Console و Breakpoint چیست؟

Console بیشتر برای مشاهده و ثبت اطلاعات مناسب است. Breakpoint اجرای برنامه را متوقف می‌کند و امکان بررسی Interactive وضعیت Variableها و جریان اجرای کد را فراهم می‌کند.

---

## Step Over و Step Into چه تفاوتی دارند؟

Step Over از Function فراخوانی‌شده عبور می‌کند و اجرای برنامه را به خط بعد می‌برد. Step Into وارد Function می‌شود تا اجرای داخلی آن را بررسی کنیم.

---

## Step Out چیست؟

Step Out از Function فعلی خارج می‌شود و Debugger را به سطح اجرای قبلی بازمی‌گرداند.

---

## Call Stack چه کاربردی دارد؟

Call Stack مسیر Functionهای فعال را در نقطه فعلی اجرای برنامه نشان می‌دهد. در Debugging کمک می‌کند بفهمیم برنامه چگونه به نقطه‌ای که در آن متوقف شده‌ایم رسیده است.

---

## Error و Bug چه تفاوتی دارند؟

Error یک وضعیت خطا است که JavaScript یا محیط اجرا آن را گزارش می‌کند. Bug رفتار نادرست Application است و ممکن است بدون ایجاد Error نیز رخ دهد.

---

## آیا نبودن Error به معنای درست بودن برنامه است؟

خیر.

یک برنامه ممکن است بدون هیچ Runtime Errorای اجرا شود اما نتیجه نادرست تولید کند. این نوع مشکل می‌تواند یک Logical Bug باشد.

---

## چرا Stack Trace مهم است؟

Stack Trace اطلاعاتی درباره محل وقوع Error و مسیر Functionهایی که به آن نقطه منتهی شده‌اند ارائه می‌کند. بنابراین یکی از مهم‌ترین منابع Evidence برای پیدا کردن علت مشکل است.

---

## Debugging حرفه‌ای چیست؟

Debugging حرفه‌ای یعنی مشاهده رفتار واقعی برنامه، جمع‌آوری Evidence، تشکیل یک فرضیه درباره علت مشکل، آزمایش آن فرضیه و سپس اصلاح Root Cause. تغییر تصادفی کد بدون Verification، Debugging قابل اتکا نیست.

---

## پاسخ کوتاه طلایی مصاحبه

**سؤال:** اگر یک Application خروجی اشتباه تولید کند اما هیچ Errorای نشان ندهد، چگونه Debug می‌کنید؟

**پاسخ:**

ابتدا رفتار مورد انتظار و رفتار واقعی را مشخص می‌کنم، سپس State مربوط به بخش مشکل‌دار را با Console یا Breakpoint بررسی می‌کنم. بعد بر اساس Evidence یک فرضیه درباره علت Bug ایجاد و آن را قبل از اعمال Fix بررسی می‌کنم.

---

# Conclusion

تا اینجا یاد گرفتیم که نوشتن JavaScript فقط به تولید Source Code محدود نمی‌شود.

یک Developer حرفه‌ای باید بتواند **رفتار واقعی کد هنگام اجرا** را نیز مشاهده و تحلیل کند.

Developer Tools این امکان را فراهم می‌کند.

Console برای مشاهده اطلاعات و Errorها مفید است.

Sources و Debugger امکان توقف و بررسی اجرای واقعی برنامه را فراهم می‌کنند.

Breakpoint اجازه می‌دهد برنامه را در نقطه‌ای مشخص متوقف کنیم.

Scope وضعیت Variableهای قابل دسترسی را نشان می‌دهد.

Call Stack مسیر اجرای Functionها را آشکار می‌کند.

Watch امکان بررسی مداوم Expressionهای مهم را فراهم می‌کند.

Network نیز در بررسی ارتباطات Application با منابع خارجی کاربرد دارد.

اما مهم‌تر از خود ابزارها، **روش استفاده از آن‌ها** است.

Debugging حرفه‌ای از این الگو پیروی می‌کند:

```text
Observe
↓
Collect Evidence
↓
Locate
↓
Hypothesize
↓
Verify
↓
Fix
↓
Re-test
```

بنابراین هدف Debugging این نیست که صرفاً Error را ناپدید کنیم.

هدف این است که بفهمیم:

> **برنامه چه کاری انجام می‌دهد، چرا این رفتار رخ داده است و چگونه می‌توان علت آن را اصلاح کرد.**

در فصل بعد، این مهارت را در قالب یک **Coding Challenge** به کار خواهیم گرفت و مفاهیم Fundamentals را برای حل یک مسئله واقعی ترکیب خواهیم کرد.
