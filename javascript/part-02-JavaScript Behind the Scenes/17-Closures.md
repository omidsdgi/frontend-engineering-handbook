# Chapter 17 — Closures

## Core Question

چگونه یک Function می‌تواند Scope بیرونی خود را بعد از پایان اجرای آن حفظ کند؟

---

# مقدمه

تا اینجا آموخته‌ایم که Function فقط مجموعه‌ای از دستورات نیست. Function می‌تواند:

- یک Value باشد.
- به‌عنوان Argument به Function دیگری ارسال شود.
- از یک Function دیگر Return شود.
- منطق قابل استفاده مجدد ایجاد کند.

```javascript
function createGreeting(name) {
  return function () {
    return `Hello, ${name}`;
  };
}
```

در اینجا یک Function داخل Function دیگری قرار گرفته است. اما اتفاق جالب زمانی رخ می‌دهد که Function بیرونی تمام شود:

```javascript
const greet = createGreeting('Omid');
```

Function `createGreeting` اجرا شده و به پایان رسیده است. با این حال:

```javascript
greet();
```

هنوز می‌تواند به `name` دسترسی داشته باشد.

چگونه؟

متغیر `name` متعلق به اجرای Function بیرونی بود. پس چرا هنوز در دسترس است؟

برای پاسخ باید ابتدا رابطه میان **Nested Function**، **Lexical Scope** و **Scope Chain** را درک کنیم.

---

# Closure چیست؟

## تعریف ساده

Closure زمانی شکل می‌گیرد که یک Function به Variables موجود در Scope بیرونی خود دسترسی داشته باشد و این دسترسی حتی پس از پایان اجرای Scope بیرونی نیز حفظ شود.

به بیان ساده:

> Function فقط خودش را همراه ندارد؛ بلکه می‌تواند دسترسی به Environment موردنیاز خود را نیز حفظ کند.

---

## تعریف فنی

Closure یک رابطه میان یک Function و Lexical Environmentای است که Function در آن ایجاد شده است.

این رابطه باعث می‌شود Function بتواند به Identifierهای موجود در Scopeهای بیرونی خود دسترسی داشته باشد، حتی اگر اجرای Function بیرونی به پایان رسیده باشد.

---

# چرا Closure مهم است؟

در نگاه اول ممکن است Closure یک رفتار عجیب زبان به نظر برسد. اما Closure در JavaScript یک ابزار مهم برای طراحی نرم‌افزار است.

Closure به ما اجازه می‌دهد Stateای ایجاد کنیم که:

- مستقیماً از بیرون قابل دسترسی نباشد.
- فقط از طریق Functionهای مشخصی تغییر کند.
- بین چند Invocation حفظ شود.
- بدون استفاده از Global Variable نگهداری شود.

برای مثال، می‌توانیم یک Counter ایجاد کنیم که مقدار داخلی خود را حفظ می‌کند.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter());
console.log(counter());
console.log(counter());
```

خروجی:

```text
1
2
3
```

در اینجا `createCounter()` فقط یک بار اجرا شده است.

اما مقدار `count` بین فراخوانی‌های مختلف `counter()` حفظ می‌شود.

در واقع، در زمان اجرای:

```javascript
const counter = createCounter();
```

Function بیرونی اجرا می‌شود. داخل آن:

```javascript
let count = 0;
```

ایجاد می‌شود.

سپس Function داخلی Return می‌شود:

```javascript
return function () {
  count++;
  return count;
};
```

پس از پایان اجرای `createCounter`، ممکن است انتظار داشته باشیم `count` دیگر قابل دسترسی نباشد.

اما Function داخلی هنوز به `count` نیاز دارد. به همین دلیل دسترسی لازم برای Function داخلی حفظ می‌شود.

در نتیجه:

```javascript
counter();
```

می‌تواند همان `count` را پیدا کند.

برای درک واقعی Closure نباید از خود Closure شروع کنیم.

ابتدا باید بدانیم Function در چه محیطی ایجاد شده و چگونه Identifierهای بیرونی را پیدا می‌کند.

سه مفهوم در اینجا اهمیت دارند:

```text
Scope Chain → Lexical Scope → Function
```

---

# Nested Function

Nested Function تابعی است که داخل Function یا Scope دیگری تعریف شده است.

```javascript
function createMessage() {
  const message = 'Welcome';

  function showMessage() {
    console.log(message);
  }

  showMessage();
}
```

در این مثال:

```text
showMessage
```

یک Nested Function است.

---

## چرا Nested Function مهم است؟

زیرا Function داخلی در محیطی ایجاد شده است که Variable مربوط به Function بیرونی را می‌شناسد.

در مثال بالا:

```javascript
const message = 'Welcome';
```

در Scope بیرونی‌تر قرار دارد.

Function داخلی:

```javascript
showMessage();
```

می‌تواند آن را پیدا کند.

---

# Lexical Scope

Lexical Scope یعنی محدوده دسترسی به یک Identifier بر اساس محل قرار گرفتن کد در ساختار برنامه تعیین می‌شود.

به بیان ساده:

> Function کجا نوشته شده است؟

JavaScript براساس محل نوشته شدن کد مشخص می‌کند یک Function به چه Scopeهایی دسترسی دارد.

در مثال فوق، Function `showMessage` در محیطی نوشته شده است که `message` در Scope بیرونی آن قرار دارد. بنابراین Function می‌تواند به آن دسترسی داشته باشد.

---

## مدل ذهنی

Scope را می‌توان مانند مجموعه‌ای از محیط‌های تو‌در‌تو تصور کرد:

```text
Outer Scope
└── Inner Scope
    └── Nested Function
```

Function داخلی می‌تواند به Scope خود و Scopeهای بیرونی دسترسی داشته باشد.

---

# Scope Chain

اگر JavaScript یک Identifier را در Scope فعلی پیدا نکند، برای پیدا کردن آن به Scope بیرونی مراجعه می‌کند.

این زنجیره را **Scope Chain** می‌نامیم.

به بیان ساده:

> اگر اینجا پیدا نکردم، کجا بگردم؟

Scope Chain مشخص می‌کند JavaScript برای پیدا کردن یک Identifier از کجا شروع کند و به چه Scopeهای بیرونی برود.

---

## مثال

```javascript
const appName = 'Recipe Hub';

function createApp() {
  const version = '1.0';

  function showInfo() {
    console.log(appName);
    console.log(version);
  }

  showInfo();
}
```

وقتی `showInfo` اجرا می‌شود، JavaScript ابتدا Identifier را در Scope خودش جست‌وجو می‌کند.

اگر پیدا نشد، به Scope بیرونی می‌رود.

در نتیجه مسیر جست‌وجو به‌صورت مفهومی چنین است:

```text
Global Scope
↓
createApp Scope
↓
showInfo Scope
```

### برای `version`

جست‌وجو در Scope خود `showInfo` نتیجه‌ای ندارد.

سپس JavaScript به Scope `createApp` می‌رود و `version` را پیدا می‌کند.

### برای `appName`

پس از بررسی Scopeهای نزدیک‌تر، JavaScript به Global Scope می‌رسد و آن را پیدا می‌کند.

Lexical Scope می‌گوید:

> چون `showInfo` اینجا نوشته شده، به `createApp` دسترسی دارد.

Scope Chain می‌گوید:

> اگر `version` را در `showInfo` پیدا نکردم، در `createApp` دنبالش می‌گردم.

---

## جمله طلایی

> Lexical Scope می‌گوید Function به کدام Scopeها دسترسی دارد؛
>
> Scope Chain مسیر پیدا کردن Variable در آن Scopeهاست؛
>
> و Closure باعث می‌شود این دسترسی Lexical برای Function حتی بعد از پایان اجرای Scope بیرونی نیز حفظ شود.

این سه مفهوم را به‌صورت یک زنجیره علت و معلولی ببینید، اما یکسان نیستند.

```text
Lexical Scope
      ↓
Scope Chain
      ↓
Closure
```

---

# Preserved Environment

اکنون به بخش اصلی Closure می‌رسیم.

سؤال این است:

> اگر Function بیرونی اجرا شود و به پایان برسد، چرا Variableهای آن هنوز برای Function داخلی قابل دسترسی هستند؟

برای پاسخ باید چرخه اجرای Function را بررسی کنیم.

---

# Outer Variables

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}
```

در اینجا:

```text
count
```

یک **Outer Variable** برای Function داخلی است.

Function داخلی:

```javascript
function () {
  count++;
  return count;
}
```

خودش `count` را تعریف نکرده است. اما به آن نیاز دارد.

---

# Execution Lifecycle

زمانی که:

```javascript
createCounter();
```

اجرا می‌شود، Function شروع به اجرای دستورات خود می‌کند.

در طول اجرای آن:

```javascript
let count = 0;
```

ایجاد می‌شود.

سپس Function داخلی ساخته و Return می‌شود:

```javascript
return function () {
  count++;
  return count;
};
```

اکنون اجرای Function بیرونی تمام می‌شود.

اما Function داخلی هنوز به `count` وابسته است.

بنابراین دسترسی لازم برای آن Function حفظ می‌شود.

---

## مدل ذهنی صحیح

بهتر است این فرایند را به شکل زیر تصور کنیم:

```text
createCounter()
├── count = 0
│
└── return inner Function
                │
                └── needs count
```

بعد از پایان اجرای `createCounter`:

```text
Inner Function
      │
      └── preserved access
                │
                └── count
```

بنابراین Function داخلی همچنان می‌تواند:

```javascript
count++;
```

را اجرا کند.

---

# نکته مهم درباره حفظ Scope

گاهی گفته می‌شود:

> Closure باعث می‌شود Scope بیرونی زنده بماند.

این جمله برای ساختن مدل ذهنی اولیه مفید است، اما از نظر فنی باید دقیق‌تر صحبت کنیم.

JavaScript الزاماً کل Scope یا تمام Variables آن را بدون دلیل نگه نمی‌دارد.

Function به Environment موردنیاز خود دسترسی دارد و داده‌ای که هنوز از طریق Function قابل دسترسی و موردنیاز است، می‌تواند در دسترس باقی بماند.

بنابراین بهتر است بگوییم:

> Closure دسترسی Function به Environment بیرونی موردنیاز آن را حفظ می‌کند.

نکته مهم‌تر این است که Function داخلی فقط مقدار نهایی را به‌صورت یک Copy دریافت نکرده است.

بلکه Function همچنان به Environment مربوط به Variable دسترسی دارد.

به همین دلیل اگر Variable قابل تغییر باشد، تغییر آن نیز برای Function داخلی قابل مشاهده است.

---

# Practical Closures

اکنون که Mechanism Closure را شناختیم، می‌توانیم از آن برای حل مسائل واقعی استفاده کنیم.

سه کاربرد مهم در این مرحله عبارت‌اند از:

- Private Variables
- Counter
- Function Factory

---

# Private Variables

Closure می‌تواند داده‌ای ایجاد کند که مستقیماً از بیرون قابل دسترسی نباشد.

```javascript
function createAccount() {
  let balance = 0;

  return {
    deposit(amount) {
      balance += amount;
    },

    getBalance() {
      return balance;
    }
  };
}

const account = createAccount();

account.deposit(100);

console.log(account.getBalance());
```

خروجی:

```text
100
```

اما:

```javascript
console.log(account.balance);
```

خروجی:

```text
undefined
```

---

## تحلیل مهندسی

`balance` مستقیماً روی Object قرار ندارد.

بنابراین کد بیرونی نمی‌تواند به‌صورت مستقیم آن را تغییر دهد.

دسترسی به آن فقط از طریق Functionهایی انجام می‌شود که داخل همان Closure قرار دارند.

این الگو نوعی **Encapsulation** ایجاد می‌کند.

---

# چند Closure، چند State

نکته بسیار مهم این است که هر Invocation از Function Factory می‌تواند State جداگانه‌ای داشته باشد.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counterA = createCounter();
const counterB = createCounter();

console.log(counterA());
console.log(counterA());
console.log(counterB());
```

خروجی:

```text
1
2
1
```

چرا؟

زیرا:

```text
counterA
```

و:

```text
counterB
```

به دو Environment مستقل مربوط هستند.

مدل ذهنی:

```text
counterA → count = 2
counterB → count = 1
```

این یکی از مهم‌ترین ویژگی‌های Closure است.

---

# Function Factory

Function Factory تابعی است که Functionهای جدید تولید می‌کند.

Closure باعث می‌شود هر Function تولید شده بتواند State یا Configuration مربوط به زمان ایجاد خود را حفظ کند.

---

## مثال

فرض کنید می‌خواهیم برای کاربران مختلف Greeting Function ایجاد کنیم:

```javascript
function createGreeting(name) {
  return function () {
    return `Hello, ${name}`;
  };
}

const greetOmid = createGreeting('Omid');
console.log(greetOmid());
// Hello, Omid

const greetSara = createGreeting('Sara');
console.log(greetSara());
// Hello, Sara
```

هر Function اطلاعات مربوط به زمان ایجاد خود را حفظ کرده است.

---

## تحلیل مهندسی

این الگو در طراحی APIهای کوچک و قابل تنظیم بسیار مفید است.

به جای اینکه هر بار Configuration را دوباره ارسال کنیم، می‌توانیم یک Function بسازیم که Configuration لازم را در Closure نگه دارد.

```text
Configuration
      ↓
Function Factory
      ↓
Configured Function
```

این الگو در بسیاری از کتابخانه‌ها و Applicationها دیده می‌شود.

---

# Closure در Event Handlers

فرض کنید برای یک Button یک Event Handler تعریف کنیم:

```javascript
function setupButton(buttonId) {
  const message = 'Button clicked';

  const button = document.querySelector(buttonId);

  button.addEventListener('click', function () {
    console.log(message);
  });
}

setupButton('#save');
```

Function مربوط به Event Handler به `message` دسترسی دارد.

حتی زمانی که `setupButton` اجرای خود را به پایان رسانده است، Event Handler همچنان می‌تواند `message` را استفاده کند.

این یک کاربرد واقعی Closure است.

---

# Closure و Timer

Closure در Timerها نیز بسیار رایج است.

```javascript
function startTimer(name) {
  setTimeout(function () {
    console.log(`Timer for ${name} finished`);
  }, 1000);
}

startTimer('Save operation');
```

Callback مربوط به `setTimeout` به `name` دسترسی دارد.

این دسترسی از Scope بیرونی Function ایجاد شده است.

---

# Closure و State

یکی از مهم‌ترین کاربردهای Closure، نگهداری State است.

فرض کنید یک Application باید تعداد دفعات یک عملیات را ثبت کند:

```javascript
function createTracker() {
  let executions = 0;

  return function () {
    executions++;
    console.log(`Executions: ${executions}`);
  };
}

const track = createTracker();

track();
track();
track();
```

خروجی:

```text
Executions: 1
Executions: 2
Executions: 3
```

در اینجا State:

```text
executions
```

داخل Closure قرار دارد.

---

# Memory Considerations

Closure می‌تواند باعث شود داده‌ای که Function به آن دسترسی دارد، برای مدت بیشتری قابل دسترسی باقی بماند.

این رفتار معمولاً بخشی طبیعی از طراحی برنامه است.

اما اگر Closureها به‌صورت غیرضروری نگه داشته شوند و به داده‌های بزرگی دسترسی داشته باشند، می‌توانند در مدیریت Memory اهمیت پیدا کنند.

برای مثال، Event Handler یا Timerای که دیگر لازم نیست اما همچنان Reference آن حفظ شده است، می‌تواند باعث شود داده‌های مرتبط نیز قابل دسترسی باقی بمانند.

بنابراین در Applicationهای بزرگ، مدیریت Lifecycle آن اهمیت دارد.

---

# اشتباهات رایج

## اشتباه اول

❌ Closure یعنی یک Function برای همیشه در Memory باقی می‌ماند.

✔ Closure یعنی Function می‌تواند دسترسی مورد نیاز خود به Environment بیرونی را حفظ کند. طول عمر واقعی داده‌ها به Reachability و مدیریت حافظه وابسته است.

---

## اشتباه دوم

❌ Closure فقط زمانی وجود دارد که Function از Function دیگری Return شود.

✔ Return کردن Function یکی از رایج‌ترین روش‌های مشاهده Closure است، اما Closure به Return شدن Function محدود نیست.

---

## اشتباه سوم

❌ Scope Chain فقط هنگام اجرای Function ساخته می‌شود.

✔ Scope و روابط Lexical آن بر اساس ساختار کد شکل می‌گیرند و هنگام اجرای Function در Variable Lookup مورد استفاده قرار می‌گیرند.

---

## اشتباه چهارم

❌ Function داخلی فقط می‌تواند Variables مستقیم والد خود را ببیند.

✔ Function می‌تواند از طریق Scope Chain به Scopeهای بیرونی‌تر نیز دسترسی پیدا کند.

---

## اشتباه پنجم

❌ Closure یک Scope جدید است.

✔ Closure یک رابطه میان Function و Environment مربوط به محل ایجاد آن است.

---

## اشتباه ششم

❌ Function داخلی مقدار Variable بیرونی را فقط یک بار Copy می‌کند.

✔ Function داخلی به Environment مربوط به Variable دسترسی دارد.

---

## اشتباه هفتم

❌ با پایان اجرای Function بیرونی، تمام داده‌های آن بلافاصله از بین می‌روند.

✔ اگر داده هنوز از طریق یک Closure قابل دسترسی باشد، می‌تواند قابل دسترسی باقی بماند.

---

## اشتباه هشتم

❌ هر بار که Function Factory اجرا می‌شود، همه Closureها State مشترک دارند.

✔ هر Invocation می‌تواند Environment مستقل خود را ایجاد کند.

---

## اشتباه نهم

❌ Private Variable یعنی Variable در زبان JavaScript واقعاً یک Keyword به نام `private` دارد.

✔ در اینجا Private بودن نتیجه طراحی Closure است، نه یک Keyword خاص.

---

## اشتباه دهم

❌ Closure فقط در Function Factory استفاده می‌شود.

✔ Event Handlerها، Timerها و State Management نیز می‌توانند از Closure استفاده کنند.

---

## اشتباه یازدهم

❌ هر Closure باعث Memory Leak می‌شود.

✔ Closure به‌خودی‌خود Memory Leak نیست. مشکل زمانی ایجاد می‌شود که داده‌هایی بدون نیاز واقعی همچنان قابل دسترسی باقی بمانند.

---

## اشتباه دوازدهم

❌ با حذف Variable بیرونی، داده حتماً بلافاصله آزاد می‌شود.

✔ اگر Closure هنوز به آن داده دسترسی داشته باشد، داده ممکن است همچنان Reachable باشد.

---

# Summary

در فصل‌های قبل یاد گرفتیم که Function در JavaScript یک Value است و می‌تواند:

- به Variable اختصاص داده شود.
- به Function دیگری ارسال شود.
- از یک Function دیگر Return شود.

این ویژگی‌ها زمینه لازم برای درک Closure را فراهم کردند.

در این فصل ابتدا با Nested Function آشنا شدیم.

سپس دیدیم که Function داخلی بر اساس Lexical Scope می‌تواند به Scopeهای بیرونی دسترسی داشته باشد.

این دسترسی از طریق Scope Chain انجام می‌شود.

اما زمانی که Function داخلی از محیط خود خارج شود و Function بیرونی اجرای خود را تمام کند، این دسترسی همچنان می‌تواند حفظ شود.

این رابطه را Closure می‌نامیم.

Closure به Function اجازه می‌دهد Environment موردنیاز خود را حفظ کند.

به همین دلیل می‌توان با Closure:

- Private State ایجاد کرد.
- Counter ساخت.
- Function Factory ایجاد کرد.
- State را بین Invocationها حفظ کرد.
- در Event Handlerها و Timerها به داده‌های بیرونی دسترسی داشت.

همچنین دیدیم که Closure می‌تواند بر Lifetime داده‌ها در Memory تأثیر بگذارد؛ اما بررسی عمیق این موضوع به فصل Memory Management مربوط است.

---

# نکات مهم

- Closure به رابطه Function با Scope بیرونی آن مربوط است.
- Closure از مفهوم Lexical Scope ناشی می‌شود.
- Closure می‌تواند State بیرونی را برای Function حفظ کند.
- Closure یکی از ابزارهای مهم Encapsulation در JavaScript است.
- Closure فقط یک Syntax خاص نیست؛ نتیجه رفتار Scope و Functionهاست.
- Nested Function درون Scope دیگری تعریف می‌شود.
- Lexical Scope به محل قرارگیری Function در کد مربوط است.
- Scope Chain مسیر دسترسی به Scopeهای بیرونی را فراهم می‌کند.
- Closure بر پایه همین دسترسی Lexical شکل می‌گیرد.
- Variableهای Function بیرونی می‌توانند توسط Function داخلی استفاده شوند.
- پایان اجرای Function بیرونی لزوماً به معنای پایان دسترسی Closure به داده‌های موردنیاز نیست.
- Closure باعث حفظ دسترسی Lexical به Environment بیرونی می‌شود.
- همین ویژگی پایه ساخت Private State است.
- Closure می‌تواند Private State ایجاد کند.
- Counter یکی از ساده‌ترین مثال‌های Closure است.
- Function Factory می‌تواند Functionهای دارای Configuration متفاوت تولید کند.
- هر Invocation از Factory می‌تواند Closure مستقل خود را داشته باشد.
- Event Handlerها نمونه مهمی از Closure در Browser هستند.
- Timer Callbackها نیز می‌توانند Closure ایجاد کنند.
- Closure برای نگهداری State داخلی مفید است.
- Closure می‌تواند بر Lifetime داده‌ها در Memory تأثیر بگذارد.
- بررسی عمیق Memory Management خارج از Scope این فصل است.

---

# Technical Interview

## سطح Junior

### ⭐ Closure چیست؟

Closure رابطه‌ای میان یک Function و Lexical Environment مربوط به محل ایجاد آن است که به Function اجازه می‌دهد حتی پس از پایان اجرای Function بیرونی به Variableهای موردنیاز خود دسترسی داشته باشد.

---

### ⭐ چرا Closure ایجاد می‌شود؟

Closure نتیجه طبیعی ترکیب Functionها، Lexical Scope و Scope Chain است.

وقتی یک Function به محیط Lexical بیرونی خود وابسته باشد، این دسترسی می‌تواند همراه Function حفظ شود.

---

### ⭐ یک مثال ساده از Closure بزنید.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    return ++count;
  };
}

const counter = createCounter();

console.log(counter());
console.log(counter());
```

Function داخلی به `count` دسترسی دارد و مقدار آن بین فراخوانی‌ها حفظ می‌شود.

---

### ⭐ چرا `counter()` در مثال بالا هر بار مقدار جدیدی برمی‌گرداند؟

زیرا Function داخلی به همان `count` موجود در Environment بیرونی دسترسی دارد و مقدار آن را در هر Invocation تغییر می‌دهد.

---

### ⭐ Closure چه کاربردی دارد؟

مهم‌ترین کاربردهای آن شامل ایجاد Private State، ساخت Counter، Function Factory، Event Handler و نگهداری Configuration یا State برای Callbackها است.

---

### ⭐ آیا Closure فقط زمانی ایجاد می‌شود که Function از Function دیگری Return شود؟

خیر.

Return کردن Function یکی از رایج‌ترین موارد مشاهده Closure است. هر Function می‌تواند به Variableهای موجود در Scopeهای Lexical بیرونی خود دسترسی داشته باشد.

---

### ⭐ تفاوت Scope Chain و Closure چیست؟

Scope Chain مسیر جست‌وجوی Identifierها در Scope فعلی و Scopeهای بیرونی است.

Closure رابطه Function با Environment بیرونی خود را حفظ می‌کند و به همین دلیل این دسترسی می‌تواند پس از پایان اجرای Function بیرونی ادامه پیدا کند.

---

### ⭐ آیا Closure باعث می‌شود Variableهای بیرونی Global شوند؟

خیر.

Variable همچنان متعلق به Scope خودش است و فقط Function دارای Closure می‌تواند به آن دسترسی داشته باشد.

---

# سطح Mid-Level

### ⭐ چرا Closure برای ایجاد Private State مناسب است؟

می‌توان Variable را داخل Function بیرونی قرار داد و فقط Functionهای داخلی را که Closure تشکیل داده‌اند برای دسترسی به آن Return کرد.

در نتیجه State مستقیماً در Scope بیرونی قابل دسترسی نیست.

---

### ⭐ چرا دو Function ساخته‌شده توسط یک Function Factory می‌توانند State متفاوت داشته باشند؟

زیرا هر Invocation از Function Factory می‌تواند Environment مستقل خود را ایجاد کند.

هر Function برگشتی، Closure مربوط به همان Environment را حفظ می‌کند.

---

### ⭐ خروجی کد زیر چیست و چرا؟

```javascript
function createCounter() {
  let count = 0;

  return () => ++count;
}

const a = createCounter();
const b = createCounter();

console.log(a());
console.log(a());
console.log(b());
```

خروجی:

```text
1
2
1
```

زیرا `a` و `b` به دو Environment مستقل و در نتیجه به دو `count` مستقل دسترسی دارند.

---

### ⭐ آیا Closure مقدار Variable بیرونی را Copy می‌کند؟

خیر.

مدل صحیح این است که Function به Environment مربوط به Variable دسترسی دارد.

بنابراین اگر Variable قابل تغییر باشد، تغییرات آن در Invocationهای بعدی قابل مشاهده است.

---

### ⭐ چرا Closure در Event Handlerها کاربرد دارد؟

زیرا Event Handler معمولاً بعداً اجرا می‌شود، اما ممکن است به داده‌هایی نیاز داشته باشد که هنگام ایجاد Handler در Scope بیرونی قرار داشته‌اند.

Closure این دسترسی Lexical را حفظ می‌کند.

---

### ⭐ آیا هر Closure باعث Memory Leak می‌شود؟

خیر.

Closure یک قابلیت عادی JavaScript است.

Memory Leak زمانی مطرح می‌شود که Referenceهای غیرضروری باعث شوند داده‌هایی که دیگر مورد نیاز نیستند همچنان Reachable باقی بمانند.

---

### ⭐ چرا Closure را نباید یک Syntax مستقل دانست؟

Closure نتیجه تعامل چند قابلیت زبان است، نه یک Keyword یا Syntax خاص.

Function، Lexical Scope، Scope Chain و First-Class Functions زمینه ایجاد آن را فراهم می‌کنند.

---

# سطح Senior

### ⭐ از نظر مدل اجرای زبان، چرا Function داخلی می‌تواند بعد از پایان Function بیرونی به Variableهای آن دسترسی داشته باشد؟

زیرا Function بر اساس محل Lexical ایجاد خود با Environment مربوط به آن Scope ارتباط دارد.

اگر Function همچنان Reachable باشد و به داده‌ای از آن Environment نیاز داشته باشد، دسترسی لازم برای Variableهای موردنیاز حفظ می‌شود؛ بنابراین پایان اجرای Function بیرونی به‌تنهایی باعث از بین رفتن آن داده‌ها نمی‌شود.

---

### ⭐ Closure چگونه می‌تواند برای Encapsulation استفاده شود؟

State را می‌توان در Scope یک Function قرار داد و تنها Functionهای داخلی را که Closure تشکیل داده‌اند برای دسترسی یا تغییر آن State در اختیار کد بیرونی قرار داد.

در نتیجه Interface قابل دسترسی از Implementation داخلی جدا می‌شود.

---

### ⭐ چرا Function Factory یک الگوی مهم مبتنی بر Closure است؟

زیرا می‌تواند هنگام ایجاد Function، Configuration یا State خاصی را در Environment خود قرار دهد.

Function برگشتی آن Environment را حفظ می‌کند و در نتیجه یک Function مستقل و از قبل پیکربندی‌شده ایجاد می‌شود.

---

### ⭐ Closure چه ارتباطی با Memory Management دارد؟

Closure می‌تواند باعث شود داده‌هایی که Function به آن‌ها دسترسی دارد تا زمانی که Closure قابل دسترسی است، Reachable باقی بمانند.

بنابراین Closure می‌تواند روی Lifetime داده‌ها تأثیر بگذارد.

---

### ⭐ آیا می‌توان گفت «Closure، Scope بیرونی را ذخیره می‌کند»؟

این عبارت برای آموزش اولیه مفید است، اما از نظر فنی ساده‌سازی شده است.

مدل دقیق‌تر این است که Function ارتباط Lexical خود با Environment را حفظ می‌کند و فقط داده‌های موردنیاز می‌توانند همچنان قابل دسترسی باشند.

---

### ⭐ چرا Closure در طراحی Applicationهای واقعی ارزشمند است؟

چون امکان ساخت State خصوصی، Functionهای Configured، Callbackهای وابسته به Context و رفتارهای قابل استفاده مجدد را بدون Global State فراهم می‌کند.

این ویژگی به کاهش Coupling و کنترل بهتر دسترسی به داده کمک می‌کند.

---

### ⭐ اگر یک Closure به داده بزرگی دسترسی داشته باشد، چه نگرانی مهندسی ایجاد می‌شود؟

اگر Closure یا Reference مرتبط با آن بیش از زمان لازم زنده بماند، داده بزرگ نیز ممکن است Reachable باقی بماند و مصرف Memory افزایش پیدا کند.

بنابراین باید Lifecycle Event Handlerها، Timerها و سایر Referenceهای مرتبط را در Applicationهای بزرگ مدیریت کرد.

---

# جمع‌بندی فصل

Closure یکی از مفاهیمی است که در ابتدا ممکن است پیچیده به نظر برسد، اما زمانی که آن را از مسیر درست دنبال کنیم، رفتار آن قابل پیش‌بینی می‌شود.

ابتدا باید Nested Function را داشته باشیم.

Function داخلی بر اساس Lexical Scope می‌داند که در چه محیطی ایجاد شده است.

سپس هنگام جست‌وجوی Identifierها، Scope Chain امکان حرکت از Scope فعلی به Scopeهای بیرونی را فراهم می‌کند.

اگر Function داخلی از محیط خود خارج شود، مثلاً به‌عنوان یک Value از Function بیرونی Return شود، رابطه آن با Environment موردنیاز خود همچنان اهمیت پیدا می‌کند.

این رابطه همان چیزی است که به آن Closure می‌گوییم.

مدل ذهنی نهایی این فصل:

```text
Nested Function
      ↓
Lexical Scope
      ↓
Scope Chain
      ↓
Closure
      ↓
Preserved Environment
      ↓
Private State
      ↓
Function Factory
      ↓
Real Applications
```

بنابراین Closure را نباید یک تکنیک عجیب یا یک Syntax خاص در JavaScript بدانیم.

Closure نتیجه طبیعی این واقعیت است که Functionها Value هستند و در یک Lexical Environment ایجاد می‌شوند.

در فصل بعد، مسیر مفهومی وارد **Explicit Function Binding** می‌شود و بررسی خواهیم کرد که چگونه می‌توان مقدار `this` را به‌صورت صریح کنترل کرد.
