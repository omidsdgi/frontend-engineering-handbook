# Chapter 52 — JavaScript Engine and Runtime

---

# Chapter Goal

پس از مطالعه این فصل، انتظار می‌رود بتوانید:

* مفهوم **JavaScript Engine** را توضیح دهید.
* تفاوت میان **JavaScript Language** و **JavaScript Engine** را درک کنید.
* نقش Engine را در اجرای Source Code توضیح دهید.
* مفهوم **Parsing** و **Abstract Syntax Tree (AST)** را درک کنید.
* تفاوت کلی **Interpretation** و **Compilation** را توضیح دهید.
* بدانید چرا توصیف JavaScript به‌عنوان یک زبان صرفاً Interpreted دقیق نیست.
* مفهوم **Just-In-Time Compilation (JIT)** را در سطح مهندسی توضیح دهید.
* نقش **Optimization** و **Deoptimization** را در موتورهای مدرن JavaScript درک کنید.
* تفاوت **Engine** و **Runtime Environment** را تشخیص دهید.
* تفاوت Browser Runtime و Node.js Runtime را در سطح معماری توضیح دهید.
* جایگاه Web APIs، Event Loop و libuv را در حد مناسب این فصل بشناسید.
* بدانید چرا Runtimeهای مختلفی مانند Deno و Bun وجود دارند.
* ارتباط Engine و Runtime را با Performance و Debugging تحلیل کنید.
* به پرسش‌های فنی مرتبط با JavaScript Engine و Runtime پاسخ دهید.

---

# Core Question

> **JavaScript چگونه از Source Code به اجرای واقعی تبدیل می‌شود؟**

---

# Concept Flow

```text
Source Code
↓
JavaScript Engine
↓
Parsing
↓
AST
↓
Compilation
↓
Execution
↓
Optimization
↓
Runtime Environment
```

برای درک کامل‌تر این مسیر، در طول فصل آن را به چند مرحله تقسیم می‌کنیم:

```text
Source Code
      ↓
JavaScript Engine
      ↓
Parsing
      ↓
AST
      ↓
Execution Strategy
      ↓
Compilation / Interpretation
      ↓
JIT Optimization
      ↓
Execution
      ↓
Runtime Environment
      ↓
Application Behavior
```

---

# مقدمه

در فصل اول یاد گرفتیم که JavaScript یک **Programming Language** است و برای اجرای آن به یک **Runtime Environment** نیاز داریم.

همچنین دیدیم که مرورگر و Node.js محیط‌هایی هستند که JavaScript را اجرا می‌کنند.

اما اکنون پرسش عمیق‌تری مطرح می‌شود.

وقتی می‌نویسیم:

```javascript
const price = 120;

const quantity = 3;

const total = price * quantity;
```

چه اتفاقی می‌افتد؟

این Source Code چگونه از مجموعه‌ای از Characters و Syntax به عملیات واقعی تبدیل می‌شود؟

چه چیزی کد را می‌خواند؟

چه چیزی Syntax آن را بررسی می‌کند؟

چه چیزی آن را برای اجرا آماده می‌کند؟

و در نهایت چه چیزی باعث می‌شود CPU عملیات واقعی را انجام دهد؟

پاسخ این پرسش‌ها ما را به یکی از مهم‌ترین مفاهیم **JavaScript Behind the Scenes** می‌رساند:

**JavaScript Engine**

---

# چرا باید داخل JavaScript را بشناسیم؟

در Fundamentals بیشتر روی رفتار قابل مشاهده زبان تمرکز کردیم.

برای مثال یاد گرفتیم:

```javascript
const price = 120;

if (price < 200) {
  console.log('Affordable.');
}
```

اما اکنون باید یک لایه پایین‌تر برویم.

یک Developer حرفه‌ای تنها نمی‌پرسد:

> این Syntax چگونه نوشته می‌شود؟

بلکه می‌پرسد:

> این Syntax در Runtime چگونه پردازش و اجرا می‌شود؟

شناخت این لایه به ما کمک می‌کند رفتارهایی مانند:

* Performance
* Execution
* Errors
* Debugging
* Runtime Differences

را بهتر تحلیل کنیم.

هدف این فصل تبدیل شدن به یک متخصص Compiler Design نیست.

هدف، ساختن یک **Mental Model مهندسی** از مسیر اجرای JavaScript است.

---

# Abstraction در برنامه‌نویسی

یکی از دلایل اصلی که معمولاً درباره Engine صحبت نمی‌کنیم، مفهوم **Abstraction** است.

برنامه‌نویس می‌تواند بنویسد:

```javascript
const total = price * quantity;
```

بدون اینکه مجبور باشد بداند CPU دقیقاً چگونه عملیات ضرب را انجام می‌دهد.

این همان قدرت Abstraction است.

ما با یک مدل سطح بالا کار می‌کنیم و جزئیات پایین‌تر توسط سیستم‌های دیگری مدیریت می‌شوند.

اما Abstraction به معنای بی‌اهمیت بودن جزئیات نیست.

گاهی برای Debugging یا Performance باید بدانیم پشت این Abstraction چه اتفاقی رخ می‌دهد.

---

# JavaScript فقط یک زبان نیست

از یک دیدگاه حرفه‌ای، هنگام اجرای JavaScript چند لایه مختلف با یکدیگر همکاری می‌کنند.

```text
JavaScript Language
        ↓
JavaScript Engine
        ↓
Runtime Environment
        ↓
Host Capabilities
        ↓
Application
```

**Language** قواعد و Semantics زبان را مشخص می‌کند.

**Engine** Source Code را پردازش و اجرا می‌کند.

**Runtime** محیطی را فراهم می‌کند که Engine و برنامه در آن اجرا می‌شوند و قابلیت‌های Host را در اختیار برنامه قرار می‌دهد.

این تفکیک یکی از مهم‌ترین مدل‌های ذهنی این فصل است.

---

# مسیر اجرای یک برنامه JavaScript

به‌صورت ساده می‌توانیم مسیر را چنین تصور کنیم:

```text
Source Code
    ↓
JavaScript Engine
    ↓
Parsing
    ↓
AST
    ↓
Compilation / Execution
    ↓
Optimization
    ↓
Runtime
    ↓
Application Behavior
```

در ادامه هر مرحله را بررسی خواهیم کرد.

---

# Block 01 — Introduction to JavaScript Behind the Scenes

---

# Block 02 — JavaScript Engine چیست؟

## JavaScript Engine چیست؟

### تعریف ساده

**JavaScript Engine** نرم‌افزاری است که Source Code زبان JavaScript را دریافت، پردازش و اجرا می‌کند.

به بیان ساده:

> Engine بخشی از سیستم اجرای JavaScript است که کد JavaScript را به عملیات قابل اجرا تبدیل می‌کند.

برای مثال، وقتی مرورگر یک فایل JavaScript را دریافت می‌کند، Engine مرورگر مسئول پردازش و اجرای کد JavaScript است.

---

## تعریف فنی

JavaScript Engine یک پیاده‌سازی نرم‌افزاری از قابلیت‌های اجرای زبان JavaScript است که Source Code را پردازش کرده و بر اساس Semantics زبان، آن را اجرا می‌کند.

Engine می‌تواند شامل اجزایی برای:

* Parsing
* AST Construction
* Compilation
* Execution
* Optimization

باشد.

---

# چرا JavaScript به Engine نیاز دارد؟

Source Code مستقیماً برای CPU قابل اجرا نیست.

برای مثال:

```javascript
const total = price * quantity;
```

برای Developer معنای مشخصی دارد.

اما CPU این عبارت را به شکل Source Code نمی‌فهمد.

Engine در میان این دو سطح قرار می‌گیرد.

```text
Developer
   ↓
JavaScript Source Code
   ↓
JavaScript Engine
   ↓
Executable Representation
   ↓
CPU
```

Engine این فاصله را پر می‌کند.

---

# مسئولیت Engine چیست؟

Engine مسئول اجرای بخش زبانی JavaScript است.

در یک مدل ساده، می‌توان مسئولیت‌های آن را چنین در نظر گرفت:

```text
Source Code
    ↓
Parsing
    ↓
Understanding Program Structure
    ↓
Compilation / Execution
    ↓
Optimization
    ↓
Execution
```

جزئیات داخلی Engineها بسیار پیچیده‌تر هستند، اما این مدل برای درک معماری کلی کافی است.

---

# Engine در Browser

مرورگرها معمولاً یک JavaScript Engine درون خود دارند.

برای مثال:

```text
Browser
   ├── JavaScript Engine
   ├── Web APIs
   ├── Rendering System
   └── Other Browser Services
```

Engine مسئول JavaScript است.

اما Browser امکانات دیگری نیز دارد که خود JavaScript Language آن‌ها را تعریف نکرده است.

برای مثال:

```javascript
document.querySelector('.product');
```

یا:

```javascript
setTimeout(() => {
  console.log('Done');
}, 1000);
```

این قابلیت‌ها در سطح Runtime و Host Environment ارائه می‌شوند.

در این فصل فقط این تفکیک را می‌سازیم و جزئیات Browser APIs و Event Loop را در حد Preview نگه می‌داریم.

---

# Engine در Node.js

Node.js نیز JavaScript را اجرا می‌کند.

اما Node.js یک Browser نیست.

به‌طور ساده:

```text
Node.js
   ├── JavaScript Engine
   ├── Runtime APIs
   ├── libuv
   └── System Integration
```

Node.js از **V8** به‌عنوان JavaScript Engine استفاده می‌کند.

بنابراین می‌توانیم بگوییم:

```text
V8
   ↓
JavaScript Engine

Node.js
   ↓
Runtime Environment
```

این دو مفهوم یکسان نیستند.

---

# JavaScript Language vs JavaScript Engine

این دو مفهوم را نباید با یکدیگر یکی دانست.

### JavaScript Language

مشخص می‌کند:

* Syntax چیست.
* Valueها چگونه رفتار می‌کنند.
* Operatorها چگونه عمل می‌کنند.
* Functionها چگونه تعریف می‌شوند.
* Semantics زبان چیست.

### JavaScript Engine

مشخص می‌کند:

* Source Code چگونه پردازش شود.
* چگونه اجرا شود.
* چگونه بهینه شود.
* چگونه با Runtime همکاری کند.

بنابراین:

```text
JavaScript
    ↓
Language Rules

Engine
    ↓
Implementation + Execution
```

---

## مثال

فرض کنید دو محیط مختلف داریم:

```text
Chrome
Node.js
```

هر دو می‌توانند JavaScript اجرا کنند.

اما محیط اجرای آن‌ها یکسان نیست.

Chrome از V8 استفاده می‌کند و قابلیت‌های Browser را در اختیار برنامه قرار می‌دهد.

Node.js نیز از V8 استفاده می‌کند، اما Host Environment و APIهای متفاوتی دارد.

پس:

> یک Engine می‌تواند در Runtimeهای متفاوت استفاده شود.

---

# اشتباه رایج

❌ JavaScript همان V8 است.

✔ JavaScript یک Language است و V8 یک Engine است که JavaScript را اجرا می‌کند.

---

❌ Browser همان JavaScript Engine است.

✔ Browser یک Runtime Environment گسترده‌تر است که Engine یکی از اجزای آن است.

---

# نکات مهم

* JavaScript یک Language است.
* V8 یک JavaScript Engine است.
* Engine Source Code را پردازش و اجرا می‌کند.
* Runtime محیط اجرای برنامه را فراهم می‌کند.
* Browser و Node.js Runtimeهای متفاوتی هستند.
* یک Engine می‌تواند در بیش از یک Runtime استفاده شود.

---

# پاسخ کوتاه طلایی مصاحبه

**JavaScript Engine چیست؟**

JavaScript Engine نرم‌افزاری است که Source Code زبان JavaScript را پردازش و اجرا می‌کند. Engine مسئول اجرای Semantics زبان است، در حالی که Runtime قابلیت‌ها و محیط Host را در اختیار برنامه قرار می‌دهد.

---

# Block 03 — Popular JavaScript Engines

JavaScript یک Specification است و برای اجرا به Implementation نیاز دارد.

به همین دلیل Engineهای مختلفی برای اجرای JavaScript وجود دارند.

---

# V8 Engine

**V8** یکی از شناخته‌شده‌ترین JavaScript Engineها است.

V8 توسط Google توسعه داده شده و در محصولاتی مانند:

* Google Chrome
* Node.js

استفاده می‌شود.

V8 از تکنیک‌های مدرن Parsing، Compilation و Optimization برای اجرای JavaScript استفاده می‌کند.

---

# SpiderMonkey

**SpiderMonkey** JavaScript Engine توسعه‌یافته توسط Mozilla است.

این Engine در Firefox استفاده می‌شود.

در نتیجه:

```text
Firefox
   ↓
SpiderMonkey
   ↓
JavaScript Execution
```

---

# JavaScriptCore

**JavaScriptCore** Engine مورد استفاده در اکوسیستم WebKit و محصولات مرتبط با Apple است.

در Safari، JavaScript توسط JavaScriptCore اجرا می‌شود.

---

# Chakra

**Chakra** نام JavaScript Engine تاریخی Microsoft است که در Internet Explorer و نسخه‌های قدیمی Microsoft Edge استفاده می‌شد.

با تغییر معماری Edge، این Engine دیگر Engine اصلی مرورگر جدید Edge نیست.

این نام بیشتر برای درک تاریخی تکامل Engineهای JavaScript اهمیت دارد.

---

# چرا موتورهای مختلف وجود دارند؟

اگر JavaScript یک زبان است، چرا همه از یک Engine استفاده نمی‌کنند؟

پاسخ این است که Specification زبان، یک Implementation واحد را تحمیل نمی‌کند.

به‌عبارت دیگر:

```text
ECMAScript Specification
        ↓
Rules and Semantics
        ↓
Different Implementations
        ├── V8
        ├── SpiderMonkey
        └── JavaScriptCore
```

هر Engine باید رفتار مورد انتظار زبان را پیاده‌سازی کند، اما می‌تواند معماری داخلی و تکنیک‌های بهینه‌سازی متفاوتی داشته باشد.

---

# آیا Engineها دقیقاً یکسان رفتار می‌کنند؟

از نظر هدف، باید رفتار JavaScript را مطابق Specification پیاده‌سازی کنند.

اما ممکن است در:

* Performance
* Memory Usage
* Optimization Strategy
* Internal Architecture

تفاوت داشته باشند.

این تفاوت یکی از دلایل مهمی است که Performance یک برنامه JavaScript را نمی‌توان تنها با نگاه کردن به Source Code تحلیل کرد.

---

# مثال مهندسی

فرض کنید یک الگوریتم در دو Browser اجرا می‌شود:

```javascript
for (let i = 0; i < 1_000_000; i++) {
  // work
}
```

Source Code یکسان است.

اما Engineهای مختلف ممکن است این کد را با استراتژی‌های داخلی متفاوت اجرا و بهینه کنند.

بنابراین:

```text
Same Source Code
        ↓
Different Engines
        ↓
Potentially Different Performance
```

این به معنای متفاوت بودن Semantics زبان نیست.

---

# اشتباهات رایج

❌ V8 خود JavaScript است.

✔ V8 یکی از Engineهای JavaScript است.

---

❌ فقط Browserها JavaScript Engine دارند.

✔ Runtimeهای مختلف می‌توانند از JavaScript Engine استفاده کنند.

---

❌ وجود چند Engine یعنی JavaScript چند زبان متفاوت است.

✔ Engineها Implementationهای مختلف یک Language/Specification هستند.

---

# نکات مهم

* V8، SpiderMonkey و JavaScriptCore نمونه‌هایی از JavaScript Engine هستند.
* Engineها Implementationهای متفاوت JavaScript هستند.
* تفاوت Engineها می‌تواند روی Performance اثر بگذارد.
* JavaScript Language را نباید با یک Engine خاص یکی دانست.

---

# پاسخ کوتاه طلایی مصاحبه

**چرا چند JavaScript Engine وجود دارد؟**

چون JavaScript یک Specification/Language است و می‌تواند توسط Implementationهای مختلف پیاده‌سازی شود. Engineهای مختلف مانند V8، SpiderMonkey و JavaScriptCore قواعد زبان را پیاده‌سازی می‌کنند، اما ممکن است در معماری داخلی و Optimization متفاوت باشند.

---

# Block 04 — Parsing و Compilation

اکنون می‌دانیم Engine چیست.

اما هنوز یک سؤال مهم باقی مانده است:

> Engine چگونه Source Code را می‌فهمد؟

برای پاسخ، باید با **Parsing** آشنا شویم.

---

# Parsing چیست؟

### تعریف ساده

Parsing فرآیندی است که در آن Engine Source Code را بررسی می‌کند تا ساختار آن را مطابق Grammar زبان مشخص کند.

برای مثال:

```javascript
const total = price * quantity;
```

برای ما یک عبارت ساده است.

اما Engine باید ساختار این کد را تشخیص دهد.

باید بفهمد:

* `const` یک Declaration است.
* `total` یک Identifier است.
* `=` یک Assignment Operator است.
* `price * quantity` یک Expression است.

این فرآیند Parsing نام دارد.

---

## تعریف فنی

Parsing فرآیند تحلیل Source Code بر اساس Grammar زبان و تبدیل آن به یک ساختار قابل پردازش برای مراحل بعدی اجرای برنامه است.

یکی از مهم‌ترین خروجی‌های این فرآیند، **Abstract Syntax Tree** یا **AST** است.

---

# Abstract Syntax Tree چیست؟

AST یک نمایش ساختاری از Program است.

به جای آنکه Engine فقط یک رشته طولانی از Characters را ببیند، ساختار معنایی Syntax را در قالب یک Tree دریافت می‌کند.

برای مثال:

```javascript
const total = price * quantity;
```

به‌صورت مفهومی می‌توان آن را چنین دید:

```text
VariableDeclaration
       │
       └── total
             │
             └── BinaryExpression
                    ├── price
                    ├── *
                    └── quantity
```

این نمودار تنها یک مدل ساده آموزشی است.

AST واقعی جزئیات بیشتری دارد.

---

# چرا AST مهم است؟

AST باعث می‌شود ساختار Program برای ابزارهای نرم‌افزاری قابل پردازش شود.

برای مثال، Engine می‌تواند تشخیص دهد:

```javascript
price * quantity
```

یک Expression است و از دو Operand و یک Operator تشکیل شده است.

AST فقط مخصوص JavaScript Engine نیست.

ابزارهایی مانند:

* Linters
* Formatters
* Compilers
* Transpilers
* Code Analysis Tools

نیز می‌توانند از ساختارهای AST استفاده کنند.

---

# Syntax Analysis

یکی از وظایف Parsing، بررسی Syntax است.

برای مثال:

```javascript
const price = ;
```

ساختار Syntax صحیحی ندارد.

در چنین شرایطی Engine نمی‌تواند Program را مانند یک برنامه معتبر JavaScript پردازش کند.

این همان دلیلی است که Syntax Error می‌تواند پیش از اجرای عادی کد شناسایی شود.

---

# Parsing با اجرای کد یکی نیست

این دو مرحله را باید از یکدیگر جدا کنیم.

```text
Parsing
   ↓
Understanding Program Structure

Execution
   ↓
Running Program Behavior
```

Engine ابتدا باید بتواند ساختار کد را بفهمد.

سپس می‌تواند آن را وارد مراحل مناسب Execution کند.

---

# Compilation چیست؟

Compilation فرآیندی است که Source Code یا یک Representation از آن را به شکلی تبدیل می‌کند که برای اجرای کارآمدتر مناسب باشد.

در یک مدل ساده:

```text
Source Code
    ↓
Compilation
    ↓
Executable Representation
```

این Representation می‌تواند شامل Machine Code یا شکل‌های میانی دیگری باشد.

---

# Compilation در JavaScript

در گذشته معمولاً JavaScript را به‌صورت یک زبان Interpreted معرفی می‌کردند.

اما این مدل برای موتورهای مدرن کافی نیست.

Engineهای امروزی می‌توانند از:

* Parsing
* Interpretation
* Compilation
* JIT Compilation
* Optimization

به‌صورت ترکیبی استفاده کنند.

بنابراین بهتر است JavaScript را صرفاً با برچسب **Interpreted Language** توصیف نکنیم.

---

# اشتباه رایج

❌ Parsing یعنی اجرای کد.

✔ Parsing ساختار Source Code را تحلیل می‌کند؛ Execution مرحله اجرای Program است.

---

❌ AST همان Source Code است.

✔ AST یک Representation ساختاری از Syntax Program است.

---

# نکات مهم

* Parsing Source Code را از نظر Grammar تحلیل می‌کند.
* AST ساختار Program را به‌صورت Tree نمایش می‌دهد.
* Syntax Analysis بخشی از فرآیند پردازش Source Code است.
* Compilation می‌تواند Code را به Representation مناسب‌تر برای Execution تبدیل کند.
* Parsing و Execution دو مرحله یکسان نیستند.

---

# پاسخ کوتاه طلایی مصاحبه

**AST چیست؟**

AST یا Abstract Syntax Tree نمایش ساختاری Syntax یک Program است که پس از Parsing ایجاد می‌شود. Engine و ابزارهای مختلف می‌توانند از این ساختار برای تحلیل و پردازش کد استفاده کنند.

---

# Block 05 — Interpretation vs Compilation

یکی از قدیمی‌ترین بحث‌ها درباره JavaScript این است:

> آیا JavaScript یک زبان Interpreted است یا Compiled؟

پاسخ حرفه‌ای این است که این تقسیم‌بندی ساده، برای موتورهای مدرن JavaScript کافی نیست.

---

# Interpretation چیست؟

در یک مدل ساده، **Interpreter** Source Code یا Representation آن را دریافت کرده و مراحل لازم برای اجرای آن را انجام می‌دهد.

مدل ذهنی ساده:

```text
Source Code
    ↓
Interpreter
    ↓
Execution
```

در این مدل تمرکز اصلی روی اجرای برنامه از طریق یک سیستم تفسیر است.

---

# Compilation چیست؟

در **Compilation**، Code پیش از Execution یا در طول فرآیند Execution به Representation مناسب‌تری تبدیل می‌شود.

مدل ساده:

```text
Source Code
    ↓
Compiler
    ↓
Executable Representation
    ↓
Execution
```

این Representation می‌تواند در نهایت برای اجرای مستقیم‌تر توسط CPU مناسب باشد.

---

# تفاوت مفهومی

می‌توانیم تفاوت را به‌صورت ساده چنین ببینیم:

```text
Interpretation
Code → Processing → Execution

Compilation
Code → Transformation → Executable Representation → Execution
```

اما Engineهای مدرن الزاماً فقط یکی از این دو مدل را انتخاب نمی‌کنند.

---

# مدل تاریخی JavaScript

در سال‌های ابتدایی JavaScript، مدل اجرای آن بسیار ساده‌تر از موتورهای امروزی بود.

با افزایش پیچیدگی Applicationها و نیاز به Performance بیشتر، Engineها به سمت تکنیک‌های پیشرفته‌تر حرکت کردند.

در نتیجه، معماری Engineهای مدرن ترکیبی و پیچیده‌تر شد.

---

# Modern JavaScript Execution

امروزه یک مدل ذهنی مناسب‌تر چنین است:

```text
Source Code
     ↓
Parsing
     ↓
AST
     ↓
Execution / Compilation
     ↓
Runtime Information
     ↓
Optimization
     ↓
Efficient Execution
```

بنابراین جمله زیر دقیق نیست:

> JavaScript فقط یک زبان Interpreted است.

بیان دقیق‌تر:

> JavaScript در Runtime توسط یک JavaScript Engine پردازش و اجرا می‌شود و موتورهای مدرن از ترکیبی از Interpretation، Compilation و تکنیک‌های Optimization استفاده می‌کنند.

---

# چرا این تفاوت برای Developer مهم است؟

زیرا برچسب «Interpreted» یا «Compiled» به‌تنهایی توضیح نمی‌دهد که یک Engine مدرن چگونه رفتار می‌کند.

برای مثال، اگر Application شما در یک مسیر خاص کند باشد، پاسخ این سؤال که:

> JavaScript Interpreted است یا Compiled؟

به‌تنهایی هیچ توضیح عملی درباره Performance نمی‌دهد.

برای تحلیل واقعی باید به مواردی مانند:

* Execution
* Runtime Behavior
* Optimization
* Memory
* Engine Strategy

توجه کنیم.

---

# اشتباهات رایج

❌ JavaScript همیشه خط‌به‌خط اجرا می‌شود.

✔ مدل اجرای Engineهای مدرن بسیار پیچیده‌تر از این توصیف ساده است.

---

❌ JavaScript هیچ Compilationای ندارد.

✔ موتورهای مدرن JavaScript از Compilation Techniques استفاده می‌کنند.

---

❌ اگر JavaScript Compiled باشد، پس دقیقاً مانند C++ اجرا می‌شود.

✔ Compilation در زبان‌ها و Runtimeهای مختلف می‌تواند مراحل و اهداف متفاوتی داشته باشد.

---

# نکات مهم

* Interpretation و Compilation دو مدل مفهومی برای پردازش Code هستند.
* JavaScript مدرن را نباید صرفاً Interpreted دانست.
* Engineهای مدرن از تکنیک‌های مختلف Execution و Compilation استفاده می‌کنند.
* برای درک Performance باید Runtime و Engine را نیز در نظر گرفت.

---

# پاسخ کوتاه طلایی مصاحبه

**آیا JavaScript Interpreted است یا Compiled؟**

این تقسیم‌بندی دوگانه برای JavaScript مدرن بیش از حد ساده است. Engineهای مدرن از ترکیبی از Parsing، Interpretation، Compilation و Optimization برای اجرای کد استفاده می‌کنند.

---

# Block 06 — Just-In-Time Compilation (JIT)

یکی از مهم‌ترین تکنیک‌هایی که برای درک Engineهای مدرن باید بشناسیم **JIT Compilation** است.

---

# JIT چیست؟

JIT مخفف:

**Just-In-Time Compilation**

است.

### تعریف ساده

JIT به فرآیندی اشاره می‌کند که در آن Compilation در زمان اجرای برنامه انجام می‌شود و Engine می‌تواند بر اساس اطلاعات واقعی Runtime، بخش‌هایی از Code را برای اجرای بهتر بهینه کند.

مدل ذهنی:

```text
Source Code
     ↓
Engine
     ↓
Execution
     ↓
Runtime Information
     ↓
JIT Optimization
     ↓
Efficient Execution
```

---

# چرا JIT مهم است؟

Engine هنگام اجرای واقعی Program اطلاعاتی درباره رفتار آن به دست می‌آورد.

برای مثال ممکن است یک مسیر از Code بارها اجرا شود.

Engine می‌تواند از این اطلاعات برای بهینه‌سازی بخش‌های پرتکرار استفاده کند.

بنابراین:

```text
Runtime Behavior
      ↓
Information
      ↓
Optimization Opportunity
      ↓
Faster Execution
```

---

# Optimization چیست؟

Optimization یعنی تغییر نحوه اجرای یک بخش از Program با هدف بهبود ویژگی‌هایی مانند Performance، بدون تغییر رفتار مورد انتظار Program.

Engine می‌تواند در شرایط مناسب برخی مسیرهای Code را برای Execution کارآمدتر آماده کند.

---

# یک مثال ساده

فرض کنید Functionای بارها با ورودی‌های سازگار اجرا می‌شود:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

Engine هنگام Execution می‌تواند اطلاعاتی درباره رفتار واقعی این Code جمع‌آوری کند.

اگر یک مسیر به‌صورت پایدار و پرتکرار اجرا شود، Engine ممکن است آن را برای Execution کارآمدتر بهینه کند.

هدف این مثال آموزش جزئیات داخلی JIT نیست.

هدف تنها این است:

> Engine می‌تواند هنگام Runtime از اطلاعات واقعی برنامه برای Optimization استفاده کند.

---

# Deoptimization چیست؟

Optimization همیشه دائمی و بدون شرط نیست.

فرض کنید Engine بر اساس رفتار مشاهده‌شده، یک فرض داخلی درباره Code داشته باشد.

اگر بعداً رفتار Program با آن فرض سازگار نباشد، Engine ممکن است Optimization انجام‌شده را کنار بگذارد یا مسیر اجرای دیگری را انتخاب کند.

این فرآیند را به‌صورت ساده **Deoptimization** می‌نامیم.

مدل ذهنی:

```text
Observed Behavior
      ↓
Optimization
      ↓
Assumption Changes
      ↓
Deoptimization
      ↓
Alternative Execution
```

جزئیات دقیق این فرآیند وابسته به معماری Engine است و در این فصل وارد جزئیات داخلی آن نمی‌شویم.

---

# JIT و Performance

یکی از دلایل مهم استفاده از JIT در Engineهای مدرن، دستیابی به Performance بهتر است.

اما نباید این برداشت را ایجاد کنیم که:

> JIT همیشه همه چیز را سریع‌تر می‌کند.

Optimization خود نیز هزینه دارد.

Engine باید:

* Code را تحلیل کند.
* رفتار آن را مشاهده کند.
* تصمیم Optimization بگیرد.
* در صورت نیاز Code را دوباره پردازش کند.

بنابراین JIT بخشی از یک سیستم پیچیده برای ایجاد تعادل میان:

```text
Startup Cost
+
Execution Cost
+
Optimization Cost
```

است.

---

# اشتباهات رایج

❌ JIT یعنی JavaScript همیشه قبل از اجرا کامپایل می‌شود.

✔ JIT بخشی از Compilation را در زمان Runtime انجام می‌دهد.

---

❌ JIT تضمین می‌کند تمام JavaScript سریع اجرا شود.

✔ JIT یک تکنیک Optimization است و نتیجه آن به Code، Runtime و Engine وابسته است.

---

❌ Deoptimization یعنی Program خراب شده است.

✔ Deoptimization یک رفتار داخلی Engine برای کنار گذاشتن یک Optimization نامناسب است.

---

# نکات مهم

* JIT مخفف Just-In-Time Compilation است.
* JIT در زمان Runtime می‌تواند Code را بهینه کند.
* Optimization بر اساس اطلاعات واقعی Runtime ممکن است انجام شود.
* Deoptimization می‌تواند زمانی رخ دهد که فرضیات Optimization دیگر معتبر نباشند.
* Performance نتیجه همکاری چندین بخش سیستم است، نه فقط JIT.

---

# پاسخ کوتاه طلایی مصاحبه

**JIT چیست؟**

JIT یا Just-In-Time Compilation روشی است که در زمان Runtime بخش‌هایی از Code را برای اجرای کارآمدتر Compilation و Optimization می‌کند. Engine می‌تواند بر اساس رفتار واقعی برنامه تصمیم بگیرد کدام مسیرها ارزش Optimization دارند.

---

# Block 07 — Execution Phase

پس از بررسی Parsing، Compilation و Optimization، اکنون به مرحله‌ای می‌رسیم که Program واقعاً رفتار مورد انتظار خود را ایجاد می‌کند:

**Execution**

---

# Execution چیست؟

### تعریف ساده

Execution یعنی اجرای عملی دستورات Program بر اساس Semantics زبان.

برای مثال:

```javascript
const price = 100;
const quantity = 2;

const total = price * quantity;
```

در Execution، Expressionها ارزیابی می‌شوند و عملیات مورد نیاز انجام می‌شود.

---

# Execution با Compilation یکی نیست

این دو مفهوم را نباید یکی دانست.

```text
Compilation
↓
Preparing Code for Execution

Execution
↓
Running the Program
```

Compilation می‌تواند بخشی از مسیر آماده‌سازی Code باشد.

Execution مرحله‌ای است که رفتار Program واقعاً اتفاق می‌افتد.

---

# Memory Creation

برای اجرای Program، Runtime و Engine باید داده‌ها و وضعیت‌های مورد نیاز Execution را مدیریت کنند.

برای مثال:

```javascript
const price = 100;
const quantity = 2;
```

Engine باید وضعیت مربوط به این Bindingها و Valueها را در فرآیند Execution مدیریت کند.

اما در این مرحله نباید وارد جزئیات:

* Execution Context
* Call Stack
* Scope Chain
* Memory Management

شویم.

این مفاهیم در فصل‌های بعدی به‌صورت مستقل بررسی خواهند شد.

---

# Execution Context؛ یک Preview

در فصل بعد با **Execution Context** آشنا خواهیم شد.

در این فصل تنها یک مدل ذهنی اولیه کافی است:

> JavaScript برای اجرای Code به یک محیط اجرایی نیاز دارد که اطلاعات و وضعیت لازم برای Execution را مدیریت کند.

این موضوع در Chapter 13 به‌صورت کامل بررسی خواهد شد.

---

# Runtime Behavior

هنگامی که Program اجرا می‌شود، رفتار آن تنها به Source Code وابسته نیست.

محیط اجرای Program نیز اهمیت دارد.

برای مثال:

```javascript
setTimeout(() => {
  console.log('Done');
}, 1000);
```

رفتار چنین Codeای به Runtime و Host APIs نیز وابسته است.

به همین دلیل باید میان:

```text
Language
Engine
Runtime
Host Environment
```

تفکیک قائل شویم.

---

# اشتباهات رایج

❌ Execution یعنی CPU مستقیماً Source Code را اجرا می‌کند.

✔ Engine Source Code را پردازش کرده و آن را از طریق سازوکارهای داخلی خود به Execution می‌رساند.

---

❌ تمام جزئیات Execution در Engine خلاصه می‌شود.

✔ Runtime و Host Environment نیز در رفتار واقعی Application نقش دارند.

---

# نکات مهم

* Execution مرحله اجرای Program است.
* Compilation و Execution یک مفهوم نیستند.
* Execution نیازمند مدیریت وضعیت و Environment است.
* Execution Context در فصل بعد به‌صورت کامل بررسی خواهد شد.
* Runtime می‌تواند رفتار Program را تحت تأثیر قرار دهد.

---

# پاسخ کوتاه طلایی مصاحبه

**Execution چیست؟**

Execution مرحله‌ای است که Program بر اساس Semantics زبان اجرا می‌شود و رفتار واقعی آن شکل می‌گیرد. Engine مسئول اجرای JavaScript است، اما Execution در یک Runtime Environment اتفاق می‌افتد.

---

# Block 08 — JavaScript Runtime

اکنون به یکی از مهم‌ترین تفکیک‌های این فصل می‌رسیم:

**Engine vs Runtime**

---

# Runtime چیست؟

### تعریف ساده

Runtime محیطی است که Program در آن اجرا می‌شود و علاوه بر Engine، امکانات و سرویس‌های مورد نیاز Application را فراهم می‌کند.

به بیان ساده:

> Engine کد JavaScript را اجرا می‌کند؛ Runtime محیط اجرای آن را فراهم می‌کند.

---

# Engine vs Runtime

مدل ذهنی مهم:

```text
Runtime
│
├── JavaScript Engine
│
├── Host APIs
│
├── Runtime Services
│
└── System Integration
```

بنابراین Engine تنها یک بخش از Runtime است.

---

# مثال Browser

در یک Browser می‌توانیم یک مدل ساده مانند زیر داشته باشیم:

```text
Browser Runtime
│
├── JavaScript Engine
├── Web APIs
├── Rendering
├── Browser Services
└── Event Coordination
```

Engine JavaScript را اجرا می‌کند.

Browser قابلیت‌های دیگری را نیز فراهم می‌کند.

---

# مثال Node.js

در Node.js:

```text
Node.js Runtime
│
├── V8
├── Node APIs
├── libuv
└── Operating System Integration
```

Node.js از V8 برای اجرای JavaScript استفاده می‌کند.

اما خود Node.js فقط V8 نیست.

---

# چرا این تفکیک مهم است؟

فرض کنید در Browser بنویسیم:

```javascript
document.querySelector('.product');
```

`document` بخشی از Core JavaScript Language نیست.

این قابلیت توسط Browser Runtime در اختیار JavaScript قرار می‌گیرد.

در Node.js نیز APIهایی مانند:

```javascript
fs
```

برای کار با File System در اختیار برنامه قرار می‌گیرند.

بنابراین:

```text
JavaScript Language
        +
Host Environment
        ↓
Runtime Capabilities
```

---

# Browser Runtime

Browser Runtime برای اجرای JavaScript در محیط وب طراحی شده است.

این محیط علاوه بر Engine، قابلیت‌هایی مانند:

* DOM APIs
* Timers
* Network APIs
* Browser Storage
* Rendering-related APIs

را در اختیار Application قرار می‌دهد.

در این فصل فقط جایگاه این قابلیت‌ها را مشخص می‌کنیم.

جزئیات DOM در Part مربوط به Browser JavaScript بررسی خواهد شد.

---

# Node.js Runtime

Node.js Runtime برای اجرای JavaScript خارج از Browser طراحی شده است.

در نتیجه، APIهای آن برای سناریوهایی مانند:

* File System
* Server Applications
* Networking
* Process Management

طراحی شده‌اند.

بنابراین Browser و Node.js هر دو JavaScript اجرا می‌کنند، اما Runtime آن‌ها یکسان نیست.

---

# اشتباهات رایج

❌ Node.js یک JavaScript Engine است.

✔ Node.js یک Runtime Environment است که از V8 استفاده می‌کند.

---

❌ `document` بخشی از JavaScript Language است.

✔ `document` یک Browser API است.

---

❌ اگر یک API در Browser وجود دارد، در Node.js هم وجود دارد.

✔ APIهای Host وابسته به Runtime هستند.

---

# نکات مهم

* Runtime محیط اجرای Application است.
* Engine یکی از اجزای Runtime است.
* Browser و Node.js Runtimeهای متفاوتی هستند.
* APIهای Host می‌توانند میان Runtimeها متفاوت باشند.
* همین تفاوت یکی از دلایل اصلی تفاوت رفتار JavaScript در محیط‌های مختلف است.

---

# پاسخ کوتاه طلایی مصاحبه

**تفاوت Engine و Runtime چیست؟**

Engine مسئول پردازش و اجرای JavaScript است، در حالی که Runtime محیط گسترده‌تری است که Engine را همراه با Host APIs و سرویس‌های مورد نیاز Application در اختیار برنامه قرار می‌دهد.

---

# Block 09 — Browser Runtime Components

اکنون می‌توانیم Browser Runtime را کمی دقیق‌تر ببینیم.

در این مرحله فقط یک **Preview** می‌سازیم؛ زیرا Event Loop و Asynchronous JavaScript در فصل‌های آینده به‌صورت مستقل بررسی خواهند شد.

---

# Browser Runtime Architecture

یک مدل ساده:

```text
Browser Runtime
       │
       ├── JavaScript Engine
       │
       ├── Web APIs
       │
       ├── Queues
       │
       └── Event Loop
```

هر بخش نقش متفاوتی دارد.

---

# JavaScript Engine

Engine مسئول اجرای JavaScript است.

برای مثال:

```javascript
const price = 100;

console.log(price);
```

کد JavaScript توسط Engine پردازش و اجرا می‌شود.

---

# Web APIs

Browser قابلیت‌هایی را ارائه می‌دهد که بخشی از Core JavaScript Language نیستند.

برای مثال:

```javascript
document.querySelector('.product');
```

یا:

```javascript
setTimeout(() => {
  console.log('Done');
}, 1000);
```

این APIها توسط Browser Runtime فراهم می‌شوند.

---

# Callback Queue

برخی عملیات Runtime می‌توانند پس از تکمیل، کاری را برای اجرای بعدی آماده کنند.

برای مثال:

```javascript
setTimeout(() => {
  console.log('Done');
}, 1000);
```

Callback مربوط به Timer بخشی از یک سازوکار Runtime برای زمان‌بندی اجرای بعدی است.

جزئیات Queue و نحوه هماهنگی آن با Call Stack در فصل **Event Loop** بررسی خواهد شد.

---

# Event Loop؛ Preview

**Event Loop** سازوکاری است که به Runtime کمک می‌کند اجرای JavaScript و کارهای زمان‌بندی‌شده را هماهنگ کند.

در این فصل تنها این مدل را نگه می‌داریم:

```text
JavaScript Engine
       ↓
Execution
       ↓
Runtime Coordination
       ↓
Event Loop
       ↓
Scheduled Work
```

جزئیات:

* Task Queue
* Microtask Queue
* Promise Jobs
* Execution Order

در بخش Asynchronous JavaScript آموزش داده خواهند شد.

---

# چرا نباید Event Loop را در این فصل کامل کنیم؟

زیرا Event Loop به مفاهیمی مانند:

* Call Stack
* Function Execution
* Promise
* Queue

وابسته است.

این مفاهیم هنوز در مسیر آموزشی خود به‌صورت کامل بررسی نشده‌اند.

بنابراین در این فصل فقط نقش کلی Event Loop را معرفی می‌کنیم.

---

# اشتباهات رایج

❌ Event Loop بخشی از JavaScript Language است.

✔ Event Loop بخشی از سازوکار Runtime برای هماهنگ کردن Execution و کارهای زمان‌بندی‌شده است.

---

❌ Web APIs توسط V8 تعریف شده‌اند.

✔ Web APIs توسط Browser Environment ارائه می‌شوند.

---

# نکات مهم

* Browser Runtime فقط Engine نیست.
* Web APIs بخشی از Host Environment هستند.
* Event Loop با Runtime Coordination ارتباط دارد.
* جزئیات Event Loop در فصل آینده‌دار مربوط به Async JavaScript بررسی خواهد شد.

---

# پاسخ کوتاه طلایی مصاحبه

**آیا Event Loop بخشی از JavaScript Engine است؟**

خیر، Event Loop بخشی از مدل Runtime است و برای هماهنگی اجرای JavaScript با کارهای زمان‌بندی‌شده و Queueها استفاده می‌شود. Engine مسئول اجرای JavaScript است، در حالی که Runtime سازوکارهای گسترده‌تری فراهم می‌کند.

---

# Block 10 — Server Runtime

JavaScript تنها در Browser اجرا نمی‌شود.

یکی از مهم‌ترین Runtimeهای خارج از Browser:

**Node.js**

است.

---

# Node.js Runtime

Node.js محیطی برای اجرای JavaScript خارج از Browser است.

یک مدل ساده:

```text
Node.js
   │
   ├── V8
   ├── Node APIs
   ├── libuv
   └── OS Integration
```

---

# V8 در Node.js

V8 مسئول اجرای JavaScript است.

برای مثال:

```javascript
const total = 100 + 50;

console.log(total);
```

V8 این Code را پردازش و اجرا می‌کند.

اما برای کارهایی مانند File System، V8 به‌تنهایی کافی نیست.

Node.js قابلیت‌های دیگری در اختیار برنامه قرار می‌دهد.

---

# libuv چیست؟

**libuv** یک Library مهم در معماری Node.js است که برای مدیریت بخش‌هایی از عملیات Asynchronous و تعامل با سیستم‌عامل استفاده می‌شود.

در مدل بسیار ساده:

```text
JavaScript
    ↓
Node.js
    ↓
V8 + libuv + Node APIs
    ↓
Operating System
```

libuv به Node.js کمک می‌کند برخی عملیات خارج از اجرای مستقیم JavaScript را مدیریت کند.

---

# چرا libuv مهم است؟

فرض کنید یک Application Server باید با File System یا Network کار کند.

این عملیات ممکن است نیازمند تعامل با سیستم‌عامل باشند.

Node.js می‌تواند این قابلیت‌ها را از طریق Runtime خود مدیریت کند، در حالی که V8 همچنان مسئول اجرای JavaScript است.

این تفکیک نشان می‌دهد:

> V8 و Node.js یک چیز نیستند.

---

# Backend JavaScript

به کمک Runtimeهایی مانند Node.js، JavaScript می‌تواند خارج از Browser نیز اجرا شود.

برای مثال:

```javascript
console.log('Server started.');
```

این Code می‌تواند در یک محیط Server اجرا شود.

در این حالت:

```text
Browser
   ↓
Frontend JavaScript

Node.js
   ↓
Backend JavaScript
```

Language یکسان است، اما Runtime متفاوت است.

---

# Runtime Environment

برای درک حرفه‌ای‌تر Node.js باید این مدل را حفظ کنیم:

```text
JavaScript Language
        ↓
V8 Engine
        ↓
Node.js Runtime
        ↓
Operating System
```

هر لایه مسئولیت متفاوتی دارد.

---

# اشتباهات رایج

❌ Node.js جایگزین JavaScript است.

✔ Node.js Runtimeای برای اجرای JavaScript است.

---

❌ V8 مسئول File System است.

✔ V8 JavaScript را اجرا می‌کند؛ Node.js APIها و زیرساخت لازم برای تعامل با سیستم را فراهم می‌کند.

---

❌ JavaScript فقط برای Frontend است.

✔ JavaScript می‌تواند در Runtimeهای مختلف، از جمله Runtimeهای Server-Side، اجرا شود.

---

# نکات مهم

* Node.js یک Runtime Environment است.
* V8 Engine بخشی از Node.js است.
* libuv بخشی از زیرساخت Runtime Node.js است.
* JavaScript در Node.js می‌تواند برای Backend استفاده شود.
* Language و Runtime دو مفهوم متفاوت هستند.

---

# پاسخ کوتاه طلایی مصاحبه

**نقش V8 و libuv در Node.js چیست؟**

V8 مسئول پردازش و اجرای JavaScript است، در حالی که libuv بخشی از زیرساخت Node.js برای مدیریت عملیات و تعاملات Runtime، به‌ویژه در سناریوهای Asynchronous و سیستم‌عامل، است.

---

# Block 11 — Modern JavaScript Runtimes

Node.js تنها Runtime مدرن JavaScript نیست.

در سال‌های اخیر Runtimeهای دیگری نیز ایجاد شده‌اند.

دو نمونه مهم:

* Deno
* Bun

---

# Deno

**Deno** یک Runtime مدرن برای اجرای JavaScript و TypeScript است.

Deno با هدف ارائه محیطی مدرن‌تر برای توسعه Server-Side JavaScript و TypeScript طراحی شده است.

---

# Bun

**Bun** نیز یک Runtime مدرن JavaScript است که روی Performance و یکپارچه‌سازی ابزارهای توسعه تمرکز دارد.

Bun تلاش می‌کند علاوه بر Runtime، بخش‌هایی از Tooling مورد نیاز پروژه‌های JavaScript را نیز در یک محیط واحد ارائه کند.

---

# چرا Runtimeهای جدید ایجاد می‌شوند؟

اگر Node.js وجود دارد، چرا Runtimeهای جدید ساخته می‌شوند؟

زیرا Runtime تنها مسئله اجرای JavaScript نیست.

یک Runtime می‌تواند درباره موضوعاتی مانند:

* Performance
* Developer Experience
* Security Model
* Tooling
* Module Support
* Package Management
* Web Compatibility

تصمیم‌های متفاوتی بگیرد.

بنابراین Runtimeهای مختلف می‌توانند Trade-offهای متفاوتی داشته باشند.

---

# یک مدل ذهنی مهم

نباید تصور کنیم:

```text
JavaScript
   ↓
Node.js
```

بلکه مدل بهتر:

```text
JavaScript
     ↓
Different Runtime Implementations
     ├── Browser Runtimes
     ├── Node.js
     ├── Deno
     └── Bun
```

هر Runtime محیط و قابلیت‌های خاص خود را فراهم می‌کند.

---

# آیا Code در همه Runtimeها یکسان اجرا می‌شود؟

نه لزوماً.

اگر Code فقط از JavaScript Language Features استفاده کند، قابلیت حمل آن معمولاً بیشتر است.

اما اگر به Host API خاصی وابسته باشد، Portability کاهش می‌یابد.

برای مثال:

```javascript
document.querySelector('.product');
```

به Browser وابسته است.

در مقابل:

```javascript
const total = 10 + 20;
```

به Host API خاصی نیاز ندارد.

---

# اشتباه رایج

❌ Runtimeهای مختلف زبان‌های مختلفی هستند.

✔ Runtimeها محیط‌های متفاوتی برای اجرای JavaScript فراهم می‌کنند.

---

❌ Deno یا Bun نسخه‌های جدید JavaScript هستند.

✔ آن‌ها Runtime هستند، نه نسخه‌های جدید Language.

---

# نکات مهم

* Node.js، Deno و Bun Runtime هستند.
* Runtimeها می‌توانند APIها و Trade-offهای متفاوتی داشته باشند.
* وابستگی به Host APIs قابلیت حمل Code را کاهش می‌دهد.
* JavaScript Language مستقل از یک Runtime خاص است.

---

# پاسخ کوتاه طلایی مصاحبه

**چرا Runtimeهای مختلف JavaScript وجود دارند؟**

زیرا یک Runtime علاوه بر اجرای JavaScript، درباره APIها، Security، Performance، Tooling و تعامل با Host Environment تصمیم می‌گیرد. Runtimeهای مختلف می‌توانند برای نیازها و Trade-offهای متفاوت طراحی شوند.

---

# Block 12 — Jonas Perspective

## چرا Jonas مباحث Behind the Scenes را آموزش می‌دهد؟

یکی از ویژگی‌های مهم رویکرد آموزشی Jonas Schmedtmann این است که JavaScript تنها به‌عنوان مجموعه‌ای از Syntaxها آموزش داده نمی‌شود.

هدف، ساختن یک مدل ذهنی از نحوه رفتار زبان است.

این رویکرد به‌ویژه در مباحث Behind the Scenes اهمیت دارد.

---

# ارتباط Runtime با Debugging

در فصل Developer Tools یاد گرفتیم که Debugging یعنی مشاهده و تحلیل رفتار واقعی Program.

اکنون می‌توانیم یک لایه عمیق‌تر به آن نگاه کنیم.

```text
Source Code
    ↓
Engine
    ↓
Runtime
    ↓
Actual Behavior
    ↓
Developer Tools
    ↓
Evidence
```

وقتی یک Bug رخ می‌دهد، آنچه در Source Code نوشته‌ایم تنها بخشی از مسئله است.

باید بدانیم Program در Runtime واقعاً چه رفتاری داشته است.

به همین دلیل شناخت Engine و Runtime، مدل ذهنی Debugging را تقویت می‌کند.

---

# ارتباط Engine با Performance

Performance نیز فقط به تعداد خطوط Code وابسته نیست.

دو قطعه Code با رفتار مشابه می‌توانند در شرایط مختلف Performance متفاوتی داشته باشند.

زیرا:

```text
Source Code
      ↓
Engine
      ↓
Optimization
      ↓
Runtime
      ↓
Performance
```

Engine می‌تواند Code را Optimization کند.

Runtime نیز می‌تواند محدودیت‌ها و هزینه‌های خاص خود را داشته باشد.

بنابراین یک Developer حرفه‌ای Performance را فقط با نگاه کردن به Syntax تحلیل نمی‌کند.

---

# یک مدل ذهنی حرفه‌ای

تا اینجا می‌توانیم کل فصل را در یک مدل واحد خلاصه کنیم:

```text
JavaScript Source Code
          ↓
   JavaScript Engine
          ↓
       Parsing
          ↓
         AST
          ↓
 Compilation / Execution
          ↓
      JIT / Optimization
          ↓
       Execution
          ↓
    Runtime Environment
          ↓
   Host Capabilities
          ↓
 Application Behavior
```

این مدل، پایه‌ای برای فصل‌های بعدی خواهد بود.

در فصل ۱۳ وارد **Execution Context** می‌شویم.

سپس در فصل ۱۴، **Call Stack** را بررسی خواهیم کرد.

بعد از آن Scope، Scope Chain، Hoisting، `this` و Memory Management را بررسی می‌کنیم.

بنابراین این فصل قرار نیست همه جزئیات Execution را توضیح دهد.

وظیفه آن ساختن **نقشه معماری اجرای JavaScript** است.

---

# اشتباهات رایج

### اشتباه اول: Engine و Runtime را یکی دانستن

Engine فقط بخشی از Runtime است.

---

### اشتباه دوم: تصور اینکه JavaScript مستقیماً توسط CPU اجرا می‌شود

Source Code ابتدا توسط Engine پردازش می‌شود.

---

### اشتباه سوم: تصور اینکه JavaScript فقط Interpreted است

Engineهای مدرن از Compilation و Optimization نیز استفاده می‌کنند.

---

### اشتباه چهارم: وارد کردن Event Loop در این فصل به‌صورت کامل

Event Loop موضوعی مستقل است و به مفاهیم دیگری مانند Call Stack و Asynchronous Programming وابسته است.

---

# نکات مهم

* Engine و Runtime دو مفهوم متفاوت هستند.
* JavaScript Language با Engine یکی نیست.
* Browser و Node.js Runtimeهای متفاوتی هستند.
* Parsing و AST بخشی از مسیر پردازش Source Code هستند.
* Modern JavaScript Execution ترکیبی از تکنیک‌های مختلف است.
* JIT می‌تواند در Runtime به Optimization کمک کند.
* Execution Context و Call Stack در فصل‌های بعدی بررسی خواهند شد.

---

# پاسخ کوتاه طلایی مصاحبه

**چرا شناخت Engine و Runtime برای یک Frontend Developer مهم است؟**

زیرا رفتار واقعی JavaScript فقط از Syntax مشخص نمی‌شود و Engine و Runtime در Execution، Performance و دسترسی به Host APIs نقش دارند. شناخت این لایه‌ها به Developer کمک می‌کند رفتار Program و مشکلات Runtime را دقیق‌تر تحلیل کند.

---
Block 13 — Putting It All Together: How Chrome Executes JavaScript

تا اینجا چند مفهوم مهم را به‌صورت جداگانه بررسی کردیم:

JavaScript Engine
V8
Parsing
AST
Compilation
Execution
JIT Optimization
Browser Runtime
Web APIs

اکنون بهتر است همه این مفاهیم را در یک مثال ساده کنار هم قرار دهیم.

هدف این بخش معرفی مفهوم جدید نیست؛ بلکه می‌خواهیم ببینیم مفاهیمی که در این فصل یاد گرفتیم، هنگام اجرای واقعی یک برنامه JavaScript در Google Chrome چگونه با یکدیگر ارتباط پیدا می‌کنند.

یک مثال ساده

فرض کنید فایل HTML زیر را در Chrome باز کرده‌ایم:

<!DOCTYPE html>
<html>
  <body>
    <script>
      const price = 100;
      const quantity = 2;


      const total = price * quantity;


      console.log(total);


      setTimeout(() => {
        console.log('Done');
      }, 1000);
    </script>
  </body>
</html>

کد JavaScript شامل دو بخش ساده است:

const total = price * quantity;


console.log(total);

و:

setTimeout(() => {
console.log('Done');
}, 1000);

حالا ببینیم Chrome چگونه با این Code برخورد می‌کند.

مرحله 1 — Chrome فایل را دریافت می‌کند

Chrome ابتدا Document را بارگذاری می‌کند.

هنگامی که به بخش:

<script>
  ...
</script>

می‌رسد، JavaScript Source Code را برای اجرا در اختیار JavaScript Engine قرار می‌دهد.

در Chrome، این Engine، V8 است.

مدل ساده:

HTML Document
↓
Chrome Browser
↓
JavaScript Source Code
↓
V8 Engine
مرحله 2 — V8 کد را Parse می‌کند

V8 نمی‌تواند Source Code را صرفاً به‌عنوان یک رشته Characters اجرا کند.

ابتدا باید ساختار Syntax آن را تحلیل کند.

برای مثال:

const total = price * quantity;

Engine باید تشخیص دهد که این Code شامل:

یک Variable Declaration
یک Identifier به نام total
یک Assignment
یک Expression
یک Multiplication Operation

است.

این فرآیند Parsing نام دارد.

مدل ساده:

JavaScript Source Code
↓
Parsing
↓
Program Structure
مرحله 3 — ایجاد AST

نتیجه Parsing را می‌توان به‌صورت یک Abstract Syntax Tree (AST) تصور کرد.

برای مثال، ساختار:

const total = price * quantity;

به‌صورت مفهومی شبیه این است:

VariableDeclaration
│
└── total
│
└── BinaryExpression
├── price
├── *
└── quantity

این Tree به Engine کمک می‌کند ساختار Program را به‌صورت قابل پردازش در اختیار داشته باشد.

این نمایش فقط یک مدل آموزشی ساده است و AST واقعی جزئیات بیشتری دارد.

مرحله 4 — آماده‌سازی برای Execution

پس از Parsing، Engine وارد مراحل مربوط به اجرای Code می‌شود.

در این مرحله Engine می‌تواند از تکنیک‌های مختلفی مانند:

Interpretation
Compilation
JIT Compilation
Optimization

استفاده کند.

بنابراین نباید تصور کنیم که V8 فقط Source Code را خط‌به‌خط می‌خواند و اجرا می‌کند.

مدل مناسب‌تر:

Source Code
↓
Parsing
↓
AST
↓
Execution / Compilation
↓
Optimization when applicable
↓
Execution
مرحله 5 — اجرای Expression

اکنون این Code را در نظر بگیرید:

const price = 100;
const quantity = 2;


const total = price * quantity;

در زمان Execution، Expression زیر ارزیابی می‌شود:

price * quantity

که نتیجه آن:

100 * 2
↓
200

است.

در نتیجه total مقدار 200 خواهد داشت.

مرحله 6 — اجرای console.log

سپس Code زیر اجرا می‌شود:

console.log(total);

نتیجه:

200

در Chrome DevTools Console مشاهده می‌شود.

اینجا یک نکته مهم وجود دارد.

console را نباید با خود JavaScript Language یکی بدانیم.

این قابلیت در محیط Browser در اختیار JavaScript قرار گرفته است.

بنابراین مدل ساده چنین است:

JavaScript Code
↓
V8 Engine
↓
Execution
↓
Browser-provided Capability
↓
DevTools Console
↓
200
مرحله 7 — رسیدن به setTimeout

اکنون Engine به این Code می‌رسد:

setTimeout(() => {
console.log('Done');
}, 1000);

در اینجا یک تفاوت مهم با Code قبلی وجود دارد.

setTimeout یک قابلیت ارائه‌شده توسط Browser Environment است.

یعنی:

setTimeout
↓
Browser Runtime

نه اینکه setTimeout بخشی از Syntax اصلی JavaScript باشد.

مرحله 8 — Browser Timer

هنگامی که این Code اجرا می‌شود:

setTimeout(() => {
console.log('Done');
}, 1000);

Browser Runtime اطلاعات Timer را دریافت می‌کند.

مقدار:

1000 ms

یعنی Callback نباید زودتر از زمان تعیین‌شده آماده ادامه فرآیند شود.

Callback این مثال:

() => {
console.log('Done');
}

است.

در این مرحله لازم نیست وارد جزئیات Queue و Call Stack شویم.

فقط کافی است بدانیم:

Timer توسط Browser Runtime مدیریت می‌شود، نه توسط خود JavaScript Engine به‌تنهایی.

مرحله 9 — پایان اجرای اولیه Code

پس از رسیدن به setTimeout، اجرای اولیه Source Code به پایان می‌رسد.

مدل ساده:

JavaScript Source
↓
V8
↓
Initial Execution
↓
setTimeout registered
↓
Initial Code Finished

اما Application هنوز کاملاً تمام نشده است.

Browser Runtime همچنان Timer را مدیریت می‌کند.

مرحله 10 — آماده شدن Callback

پس از گذشت زمان تعیین‌شده، Browser Runtime Callback را برای اجرای بعدی آماده می‌کند.

Callback:

() => {
console.log('Done');
}

است.

در این مرحله وارد قلمرو Asynchronous JavaScript می‌شویم.

بنابراین فعلاً فقط مدل کلی را حفظ می‌کنیم:

Browser Runtime
↓
Timer
↓
Callback becomes ready
↓
JavaScript Execution

جزئیات اینکه Callback دقیقاً چگونه وارد Queue می‌شود و چه زمانی توسط Engine اجرا می‌شود، در مباحث مربوط به Event Loop و Asynchronous JavaScript بررسی خواهد شد.

مرحله 11 — اجرای Callback

هنگامی که Runtime شرایط اجرای Callback را فراهم می‌کند، JavaScript Engine دوباره Code مربوط به Callback را اجرا می‌کند:

console.log('Done');

در نتیجه در Console مشاهده خواهیم کرد:

200
Done
کل مسیر در یک نگاه

اکنون می‌توانیم کل مثال را در یک نمودار واحد ببینیم:

HTML Document
↓
Chrome Browser
↓
JavaScript Source Code
↓
V8 Engine
↓
Parsing
↓
AST
↓
Compilation / Execution
↓
Initial Execution
│
├── price * quantity
│       ↓
│      200
│
├── console.log(200)
│       ↓
│   DevTools Console
│
└── setTimeout(...)
↓
Browser Runtime
↓
Timer
↓
Callback becomes ready
↓
JavaScript Execution
↓
console.log('Done')
↓
DevTools Console
Engine و Runtime در این مثال

اکنون می‌توانیم تفاوت این دو را کاملاً ملموس ببینیم.

V8 — JavaScript Engine

در این مثال V8 مسئول بخش‌هایی مانند:

Parsing
ساخت Representationهایی مانند AST
Compilation
Execution
Optimization در صورت امکان

است.

Chrome Browser Runtime

Chrome قابلیت‌هایی مانند:

setTimeout
console
DOM APIs
سایر Browser APIs

را در اختیار JavaScript قرار می‌دهد.

بنابراین:

Chrome Browser
│
├── V8
│    └── JavaScript Execution
│
└── Browser Runtime
├── setTimeout
├── console
├── DOM APIs
└── Other Browser Capabilities
یک نکته بسیار مهم

وقتی می‌گوییم:

«Chrome JavaScript را اجرا می‌کند»

این جمله از نظر آموزشی درست است، اما از نظر معماری بهتر است بدانیم Chrome این کار را با همکاری چند بخش انجام می‌دهد.

مدل دقیق‌تر:

Chrome
↓
Browser Runtime
├── V8
├── Web APIs
└── Other Browser Services

V8 مسئول JavaScript Execution است و Browser Environment قابلیت‌های Host را فراهم می‌کند.

چرا setTimeout مثال خوبی است؟

اگر فقط این Code را داشتیم:

const total = 100 * 2;


console.log(total);

تقریباً تمام اتفاقات داخل مسیر:

JavaScript
↓
V8
↓
Execution

قرار می‌گرفت.

اما setTimeout یک مرز مهم را نشان می‌دهد:

JavaScript Engine
↕
Browser Runtime

بنابراین این مثال به ما کمک می‌کند بفهمیم که:

اجرای JavaScript فقط Engine نیست؛ Engine و Runtime با یکدیگر همکاری می‌کنند.

Common Mistakes
اشتباه ۱ — setTimeout بخشی از JavaScript Language است

خیر.

setTimeout یک Browser API است که توسط محیط Browser در اختیار JavaScript قرار می‌گیرد.

اشتباه ۲ — V8 مسئول Timer است

در این مدل آموزشی، Timer را باید بخشی از Browser Runtime در نظر گرفت، نه مسئولیت مستقیم JavaScript Engine.

اشتباه ۳ — setTimeout(..., 1000) یعنی Callback دقیقاً بعد از یک ثانیه اجرا می‌شود

نه.

عدد 1000 زمان Timer را مشخص می‌کند؛ اینکه Callback دقیقاً چه زمانی بتواند اجرا شود به سازوکارهای Runtime و Event Loop نیز وابسته است.

جزئیات این موضوع در مباحث Asynchronous JavaScript بررسی خواهد شد.

اشتباه ۴ — AST همان چیزی است که CPU اجرا می‌کند

خیر.

AST یک Representation ساختاری از Syntax Program است و فقط یکی از مراحل پردازش Source Code محسوب می‌شود.

اشتباه ۵ — همه مراحل اجرای JavaScript را می‌توان فقط با «خط‌به‌خط اجرا شدن» توضیح داد

خیر.

Engineهای مدرن از Parsing، Compilation، Execution و Optimization استفاده می‌کنند و معماری آن‌ها بسیار پیچیده‌تر از یک Interpreter ساده است.

Key Points
Chrome برای اجرای JavaScript از V8 استفاده می‌کند.
V8 یک JavaScript Engine است.
Source Code ابتدا توسط Engine پردازش می‌شود.
Parsing ساختار Source Code را تحلیل می‌کند.
AST نمایش ساختاری Syntax Program است.
Engine می‌تواند از Compilation و Optimization استفاده کند.
setTimeout یک Browser-provided API است.
Timer توسط Browser Runtime مدیریت می‌شود.
اجرای Callback به سازوکارهای Runtime وابسته است.
جزئیات Event Loop و Queueها در مباحث آینده بررسی می‌شوند.
این مثال نشان می‌دهد که Engine و Runtime دو مفهوم متفاوت اما مرتبط هستند.
Final Mental Model

اگر بخواهیم کل Chapter 12 را در یک تصویر ذهنی نهایی خلاصه کنیم، هنگام اجرای مثال بالا در Chrome باید چنین فکر کنیم:

                CHROME
┌─────────────────────────────────────┐
│                                     │
│       Browser Runtime               │
│                                     │
│   ┌─────────────────────────────┐   │
│   │            V8               │   │
│   │                             │   │
│   │  Parse → AST → Execute      │   │
│   │          ↓                  │   │
│   │      Optimization           │   │
│   └─────────────────────────────┘   │
│                ↕                    │
│         Browser APIs               │
│         ├── console                │
│         └── setTimeout             │
│                                     │
└─────────────────────────────────────┘

پس وقتی می‌نویسیم:

setTimeout(() => {
console.log('Done');
}, 1000);

نباید آن را فقط به شکل:

JavaScript → اجرا

تصور کنیم.

مدل ذهنی بهتر این است:

JavaScript Source
↓
V8 Engine
↓
Execution
↓
Browser Runtime
↓
Timer
↓
Callback
↓
JavaScript Execution

این همان ارتباطی است که تمام مفاهیم Chapter 12 را از حالت انتزاعی خارج کرده و به یک سناریوی واقعی در Chrome متصل می‌کند.

# Chapter Review

---

# Summary

در این فصل وارد بخش **JavaScript Behind the Scenes** شدیم و بررسی کردیم که Source Code چگونه به اجرای واقعی تبدیل می‌شود.

ابتدا مفهوم **JavaScript Engine** را بررسی کردیم.

Engine نرم‌افزاری است که Source Code JavaScript را پردازش و اجرا می‌کند.

سپس میان:

```text
JavaScript Language
```

و:

```text
JavaScript Engine
```

تفاوت گذاشتیم.

JavaScript Language قواعد و Semantics را تعریف می‌کند، در حالی که Engine یک Implementation برای پردازش و اجرای آن قواعد است.

در ادامه Engineهای معروف مانند:

* V8
* SpiderMonkey
* JavaScriptCore
* Chakra

را بررسی کردیم.

سپس وارد فرآیند **Parsing** شدیم و دیدیم که Engine Source Code را تحلیل کرده و ساختاری مانند **AST** ایجاد می‌کند.

بعد از آن تفاوت مفهومی **Interpretation** و **Compilation** را بررسی کردیم و به این نتیجه رسیدیم که توصیف JavaScript به‌عنوان یک زبان صرفاً Interpreted، برای موتورهای مدرن کافی نیست.

سپس با **JIT Compilation** آشنا شدیم.

JIT به Engine اجازه می‌دهد در زمان Runtime بر اساس اطلاعات واقعی Program، بخش‌هایی از Code را Compilation و Optimization کند.

همچنین با مفهوم **Deoptimization** آشنا شدیم؛ یعنی کنار گذاشتن یک Optimization زمانی که فرضیات آن دیگر معتبر نیستند.

در ادامه به تفاوت اصلی:

```text
Engine
vs
Runtime
```

رسیدیم.

Browser و Node.js هر دو JavaScript اجرا می‌کنند، اما Runtime آن‌ها متفاوت است.

در Browser، Engine در کنار Web APIs و سایر Browser Services قرار دارد.

در Node.js، V8 در کنار Node APIs و libuv بخشی از Runtime را تشکیل می‌دهد.

همچنین Runtimeهای مدرن دیگری مانند Deno و Bun را بررسی کردیم و دیدیم که Runtimeهای مختلف می‌توانند Trade-offهای متفاوتی در Performance، Security، Tooling و Host Integration داشته باشند.

در نهایت ارتباط Engine و Runtime با:

* Debugging
* Performance

را بررسی کردیم.

---

# Key Takeaways

در پایان این فصل باید بتوانید:

* **JavaScript Language** را از **JavaScript Engine** تفکیک کنید.
* بدانید V8 یک Engine است، نه خود JavaScript.
* بدانید Browser و Node.js Runtime هستند، نه صرفاً Engine.
* نقش Parsing را در پردازش Source Code توضیح دهید.
* AST را به‌عنوان نمایش ساختاری Syntax یک Program توضیح دهید.
* Parsing را از Execution تفکیک کنید.
* تفاوت مفهومی Interpretation و Compilation را بیان کنید.
* بدانید چرا عبارت «JavaScript یک زبان Interpreted است» بیش از حد ساده است.
* JIT Compilation را در سطح مفهومی توضیح دهید.
* نقش Optimization و Deoptimization را درک کنید.
* تفاوت Engine و Runtime را توضیح دهید.
* جایگاه Web APIs را در Browser Runtime بشناسید.
* جایگاه V8 و libuv را در Node.js Runtime در سطح کلی توضیح دهید.
* بدانید Event Loop در این فصل فقط در حد Preview معرفی شده است.
* بدانید Execution Context و Call Stack در فصل‌های بعدی بررسی می‌شوند.
* بتوانید ارتباط Engine و Runtime با Performance و Debugging را تحلیل کنید.

---

# Technical Interview

## سطح پایه — Junior

### سؤال ۱

JavaScript Engine چیست و چه کاری انجام می‌دهد؟

### پاسخ

JavaScript Engine نرم‌افزاری است که Source Code زبان JavaScript را پردازش و اجرا می‌کند. Parsing، Compilation، Execution و Optimization از بخش‌های مهم فرآیند اجرای آن هستند.

---

### سؤال ۲

V8 چیست؟

### پاسخ

V8 یک JavaScript Engine است که توسط Google توسعه داده شده و در Chrome و Node.js استفاده می‌شود.

---

### سؤال ۳

آیا JavaScript و V8 یک چیز هستند؟

### پاسخ

خیر. JavaScript یک Language است، در حالی که V8 یک Engine برای اجرای JavaScript است.

---

### سؤال ۴

Parsing چیست؟

### پاسخ

Parsing فرآیند تحلیل Source Code بر اساس Grammar زبان و تبدیل آن به ساختاری قابل پردازش برای مراحل بعدی است.

---

### سؤال ۵

AST چیست؟

### پاسخ

AST یا Abstract Syntax Tree نمایش ساختاری Syntax یک Program است که پس از Parsing ایجاد می‌شود.

---

### سؤال ۶

Runtime چیست؟

### پاسخ

Runtime محیطی است که Program در آن اجرا می‌شود و علاوه بر Engine، Host APIs و سرویس‌های مورد نیاز Application را فراهم می‌کند.

---

## سطح متوسط — Mid-Level

### سؤال ۷

تفاوت JavaScript Engine و Runtime چیست؟

### پاسخ

Engine مسئول پردازش و اجرای JavaScript است، در حالی که Runtime محیط گسترده‌تری است که Engine را همراه با Host APIs و Runtime Services در اختیار Application قرار می‌دهد.

---

### سؤال ۸

چرا JavaScript را نمی‌توان صرفاً یک زبان Interpreted دانست؟

### پاسخ

زیرا موتورهای مدرن JavaScript از تکنیک‌های مختلفی مانند Compilation، JIT و Optimization استفاده می‌کنند. بنابراین مدل «فقط Interpretation» تصویر دقیقی از اجرای مدرن JavaScript ارائه نمی‌دهد.

---

### سؤال ۹

JIT Compilation چیست؟

### پاسخ

JIT یا Just-In-Time Compilation روشی است که در زمان Runtime بخش‌هایی از Code را Compilation و Optimization می‌کند. Engine می‌تواند از اطلاعات واقعی Execution برای انتخاب مسیرهای مناسب Optimization استفاده کند.

---

### سؤال ۱۰

Deoptimization چیست؟

### پاسخ

Deoptimization زمانی رخ می‌دهد که یک Optimization یا فرض داخلی Engine دیگر معتبر نباشد و Engine مجبور شود مسیر بهینه‌شده را کنار بگذارد یا روش دیگری برای Execution انتخاب کند.

---

### سؤال ۱۱

چرا Browser APIها بخشی از Core JavaScript Language محسوب نمی‌شوند؟

### پاسخ

زیرا این APIها توسط Host Environment مانند Browser فراهم می‌شوند، نه توسط خود زبان JavaScript. به همین دلیل APIهای موجود در Browser الزاماً در Node.js یا Runtimeهای دیگر وجود ندارند.

---

### سؤال ۱۲

Node.js چه رابطه‌ای با V8 دارد؟

### پاسخ

Node.js یک Runtime Environment است که از V8 برای اجرای JavaScript استفاده می‌کند. Node.js علاوه بر V8، APIها و زیرساخت‌هایی مانند libuv را نیز فراهم می‌کند.

---

### سؤال ۱۳

چرا یک Source Code یکسان ممکن است در Runtimeهای مختلف رفتار متفاوتی داشته باشد؟

### پاسخ

زیرا JavaScript Language یکسان است، اما Host APIs و Runtime Capabilities می‌توانند متفاوت باشند. Code وابسته به Browser APIs در Node.js لزوماً قابل اجرا نیست.

---

## سطح پیشرفته — Senior

### سؤال ۱۴

چرا تفکیک Language، Engine و Runtime برای یک Developer حرفه‌ای مهم است؟

### پاسخ

زیرا هر کدام مسئولیت متفاوتی دارند: Language قواعد و Semantics را تعریف می‌کند، Engine آن‌ها را اجرا می‌کند و Runtime محیط و Host Capabilities را فراهم می‌کند. این تفکیک برای تحلیل Portability، Debugging و Performance ضروری است.

---

### سؤال ۱۵

چرا وجود چند JavaScript Engine با وجود یک Specification واحد ممکن است؟

### پاسخ

Specification رفتار مورد انتظار زبان را تعریف می‌کند، اما یک Implementation واحد را تحمیل نمی‌کند. Engineهای مختلف می‌توانند همان Semantics را با معماری و تکنیک‌های Optimization متفاوت پیاده‌سازی کنند.

---

### سؤال ۱۶

چرا JIT را نمی‌توان به‌سادگی «کامپایل کامل JavaScript قبل از اجرا» دانست؟

### پاسخ

زیرا JIT Compilation در زمان Runtime انجام می‌شود و می‌تواند بر اساس اطلاعات واقعی Execution تصمیم بگیرد چه بخش‌هایی ارزش Optimization دارند. بنابراین بخشی از فرآیند Compilation به رفتار واقعی Program وابسته است.

---

### سؤال ۱۷

چرا Optimization می‌تواند به Deoptimization منجر شود؟

### پاسخ

زیرا Optimization ممکن است بر اساس فرض‌هایی درباره رفتار Code انجام شود. اگر Runtime نشان دهد این فرض‌ها دیگر معتبر نیستند، Engine باید Optimization را کنار بگذارد تا رفتار صحیح Program حفظ شود.

---

### سؤال ۱۸

چرا Performance یک Application JavaScript فقط با بررسی Source Code قابل پیش‌بینی نیست؟

### پاسخ

زیرا Execution به Engine، Runtime، Optimization Strategy، Memory Behavior و Host Environment نیز وابسته است. Source Code تنها یکی از عوامل مؤثر بر Performance است.

---

### سؤال ۱۹

چرا Event Loop در این فصل فقط به‌صورت Preview مطرح شد؟

### پاسخ

زیرا درک کامل Event Loop به مفاهیمی مانند Call Stack، Queues و Asynchronous Programming وابسته است. این مفاهیم در مسیر آموزشی خود در فصل‌های بعدی آموزش داده خواهند شد.

---

### سؤال ۲۰

چرا آموزش Execution Context در این فصل نباید کامل شود؟

### پاسخ

زیرا Execution Context یک مفهوم مستقل در معماری Execution است و فصل بعدی به‌طور اختصاصی به آن می‌پردازد. در این فصل تنها برای ساختن مدل کلی Execution به آن اشاره می‌کنیم.

---

# Golden Answers

## JavaScript Engine چیست؟

JavaScript Engine نرم‌افزاری است که Source Code زبان JavaScript را پردازش و اجرا می‌کند. Engineهایی مانند V8، SpiderMonkey و JavaScriptCore Implementationهای مختلف JavaScript هستند.

---

## تفاوت JavaScript و JavaScript Engine چیست؟

JavaScript یک Language با قواعد و Semantics مشخص است، در حالی که JavaScript Engine نرم‌افزاری است که آن Language را پیاده‌سازی و اجرا می‌کند.

---

## AST چیست؟

AST یا Abstract Syntax Tree نمایش ساختاری Syntax یک Program است که Engine پس از Parsing می‌تواند از آن برای مراحل بعدی پردازش استفاده کند.

---

## آیا JavaScript Interpreted است یا Compiled؟

این تقسیم‌بندی دوگانه برای JavaScript مدرن بیش از حد ساده است. Engineهای مدرن از ترکیبی از Interpretation، Compilation، JIT و Optimization برای اجرای JavaScript استفاده می‌کنند.

---

## JIT چیست؟

JIT یا Just-In-Time Compilation روشی است که در زمان Runtime امکان Compilation و Optimization بخش‌هایی از Code را بر اساس رفتار واقعی Program فراهم می‌کند.

---

## تفاوت Engine و Runtime چیست؟

Engine مسئول اجرای JavaScript است؛ Runtime محیطی است که Engine را همراه با Host APIs و سرویس‌های لازم برای Application فراهم می‌کند.

---

## آیا Node.js همان V8 است؟

خیر.

V8 یک JavaScript Engine است و Node.js یک Runtime Environment است که از V8 برای اجرای JavaScript استفاده می‌کند.

---

## آیا `document` بخشی از JavaScript است؟

خیر.

`document` یک Browser API است که توسط Browser Runtime در اختیار JavaScript قرار می‌گیرد.

---

## چرا چند Runtime برای JavaScript وجود دارد؟

زیرا Runtimeها می‌توانند نیازها و Trade-offهای متفاوتی در زمینه‌هایی مانند Performance، Security، Tooling و Host Integration داشته باشند. JavaScript می‌تواند در Runtimeهای مختلف اجرا شود.

---

## پاسخ طلایی نهایی

**سؤال: JavaScript چگونه از Source Code به اجرای واقعی تبدیل می‌شود؟**

**پاسخ:**

Source Code توسط JavaScript Engine ابتدا Parsing می‌شود و ساختاری مانند AST از آن ایجاد می‌شود. سپس Engine با استفاده از تکنیک‌هایی مانند Compilation، Execution و JIT Optimization کد را اجرا می‌کند؛ این فرآیند درون یک Runtime Environment مانند Browser یا Node.js انجام می‌شود که قابلیت‌های Host مورد نیاز Application را نیز فراهم می‌کند.

---

# Conclusion

در Fundamentals بیشتر روی این تمرکز داشتیم که JavaScript **چه رفتاری دارد**.

در این فصل یک قدم به داخل سیستم رفتیم و بررسی کردیم که این رفتار چگونه به اجرای واقعی تبدیل می‌شود.

مدل ذهنی اصلی این فصل چنین است:

```text
Source Code
     ↓
JavaScript Engine
     ↓
Parsing
     ↓
AST
     ↓
Compilation / Execution
     ↓
JIT / Optimization
     ↓
Execution
     ↓
Runtime Environment
     ↓
Host Capabilities
     ↓
Application Behavior
```

اکنون می‌دانیم که JavaScript فقط مجموعه‌ای از Syntaxها نیست.

برای اجرای یک Application، چند لایه مختلف با یکدیگر همکاری می‌کنند.

در مرکز این فرآیند، **JavaScript Engine** قرار دارد.

Engine Source Code را پردازش و اجرا می‌کند.

اما Engine به‌تنهایی Runtime نیست.

Browser و Node.js محیط‌هایی هستند که Engine را همراه با قابلیت‌های Host در اختیار Application قرار می‌دهند.

همچنین یاد گرفتیم که اجرای JavaScript مدرن را نمی‌توان صرفاً با عبارت «Interpreted Language» توضیح داد.

Engineهای مدرن از Compilation، JIT و Optimization استفاده می‌کنند تا بتوانند Code را در Runtime به شکل کارآمدتری اجرا کنند.

این فصل عمداً وارد جزئیات Execution Context، Call Stack، Scope و Event Loop نشد.

این مفاهیم به مراحل بعدی Concept Flow تعلق دارند.

در فصل بعد، از همین نقطه حرکت خواهیم کرد و به این سؤال پاسخ خواهیم داد:

> **هنگام اجرای JavaScript چه محیطی برای اجرای Code ایجاد می‌شود؟**

و این‌گونه وارد مفهوم **Execution Context** خواهیم شد.
