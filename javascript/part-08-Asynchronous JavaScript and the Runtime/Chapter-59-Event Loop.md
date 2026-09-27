Chapter 59 — Event Loop
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

توضیح دهید چرا برای اجرای Asynchronous Code به سازوکاری برای هماهنگی نیاز داریم.
نقش Call Stack را در اجرای JavaScript تحلیل کنید.
توضیح دهید Host APIs چه نقشی در اجرای عملیات خارج از Call Stack دارند.
مفهوم Task Queue را در مدل اجرای Asynchronous درک کنید.
تفاوت Task و Microtask را توضیح دهید.
توضیح دهید چرا Microtaskها در ترتیب اجرای Code اهمیت دارند.
نقش Event Loop را در هماهنگ کردن Call Stack و Queueها تحلیل کنید.
ترتیب اجرای Codeهای Synchronous، Task و Microtask را پیش‌بینی کنید.
رفتار نمونه‌هایی مانند setTimeout و Promise را از روی مدل Runtime تحلیل کنید.
از مدل Event Loop برای Debugging و تحلیل رفتار Asynchronous Code استفاده کنید.
Core Question

JavaScript چگونه Async Tasks را با Call Stack و Queues هماهنگ می‌کند؟

در فصل قبل دیدیم که JavaScript می‌تواند با عملیات زمان‌بر بدون متوقف کردن کامل اجرای برنامه کار کند.

اما این فقط بخشی از مسئله بود.

اگر یک عملیات Asynchronous شروع شود و نتیجه آن بعداً آماده شود، یک سؤال مهم باقی می‌ماند:

وقتی نتیجه آماده شد، چه چیزی تعیین می‌کند Callback آن چه زمانی و چگونه دوباره وارد اجرای JavaScript شود؟

برای پاسخ به این سؤال باید چند بخش از Runtime را در کنار هم ببینیم.

جریان اصلی این فصل چنین شکل می‌گیرد:

Call Stack
↓
Host APIs
↓
Task Queues
↓
Microtasks
↓
Event Loop
↓
Task Scheduling
↓
Execution Order

هدف فصل این نیست که این مفاهیم را به‌صورت اجزای جداگانه حفظ کنیم.

هدف این است که بفهمیم این اجزا چگونه یک سیستم واحد برای مدیریت اجرای Asynchronous JavaScript تشکیل می‌دهند.

مقدمه

فرض کنید در یک برنامه مرورگر چنین Codeای داریم:

console.log('Start');

setTimeout(() => {
console.log('Timer finished');
}, 0);

console.log('End');

ممکن است در نگاه اول انتظار داشته باشیم چون Timer مقدار 0 دارد، پیام آن بلافاصله بعد از Start اجرا شود.

اما خروجی چنین نیست:

Start
End
Timer finished

این سؤال ایجاد می‌شود:

اگر زمان Timer صفر است، چرا Callback آن قبل از End اجرا نشد؟

برای پاسخ باید بین دو مفهوم تفاوت بگذاریم:

Ready to execute

و:

Currently executing

آماده بودن یک Callback به این معنا نیست که JavaScript همان لحظه آن را اجرا می‌کند.

JavaScript باید ابتدا Code در حال اجرای خود را به پایان برساند و سپس Runtime مشخص کند کدام کار آماده اجرا باید وارد مسیر اجرای JavaScript شود.

در اینجا دقیقاً به مرز میان Execution و Scheduling می‌رسیم.

اجرای JavaScript از Call Stack شروع می‌شود

در فصل Execution Context و Call Stack دیدیم که JavaScript برای مدیریت اجرای Functionها از یک Stack استفاده می‌کند.

وقتی یک Function فراخوانی می‌شود، Execution Context مربوط به آن وارد Call Stack می‌شود.

برای مثال:

function calculateTotal(price, tax) {
return price + tax;
}

const total = calculateTotal(100, 10);

هنگام اجرای calculateTotal، یک Frame مربوط به آن Function روی Call Stack قرار می‌گیرد.

مدل ساده:

Call Stack

calculateTotal()
global()

Function اجرا می‌شود.

وقتی اجرای آن تمام شد، Frame مربوط به آن از Stack خارج می‌شود.

بنابراین Call Stack را می‌توانیم نقطه‌ای بدانیم که JavaScript در آن Code در حال اجرای خود را مدیریت می‌کند.

اما همین موضوع یک محدودیت مهم ایجاد می‌کند.

محدودیت Call Stack

فرض کنید Function زیر مدت زیادی CPU را درگیر کند:

function calculate() {
for (let i = 0; i < 1_000_000_000; i++) {
// Long-running work
}
}

calculate();

تا زمانی که این Function در Call Stack در حال اجرا است، JavaScript نمی‌تواند Function دیگری را روی همان مسیر اجرا کند.

مدل ذهنی ساده:

Call Stack
↓
Long-running JavaScript
↓
Stack remains busy
↓
Other JavaScript waits

این همان Blocking است که در فصل قبل درباره آن صحبت کردیم.

پس اگر قرار باشد JavaScript هم‌زمان با عملیات‌هایی مانند Timer، Network یا User Interaction کار کند، نمی‌توانیم همه این عملیات را مستقیماً داخل Call Stack اجرا کنیم.

اینجاست که Runtime به بخش دیگری نیاز دارد.

Host APIs

مرورگر فقط یک JavaScript Engine نیست.

Browser یک Host Environment است که امکانات مختلفی برای برنامه فراهم می‌کند.

برای مثال:

setTimeout(...)

یک قابلیت مربوط به محیط اجرا است.

همین موضوع درباره بسیاری از APIهای مرورگر نیز صدق می‌کند.

وقتی JavaScript یک عملیات Asynchronous را درخواست می‌کند، لازم نیست آن عملیات در همان لحظه داخل Call Stack باقی بماند.

برای مثال:

setTimeout(() => {
console.log('Done');
}, 1000);

هنگام اجرای این Code، JavaScript درخواست Timer را ثبت می‌کند.

سپس اجرای Synchronous Code خود را ادامه می‌دهد.

به‌صورت ساده:

JavaScript
↓
Call Stack
↓
Request async operation
↓
Host APIs

Host Environment مسئول مدیریت آن عملیات است.

در مورد Timer، محیط اجرا زمان موردنظر را پیگیری می‌کند.

در مورد عملیات دیگری مانند Network یا User Interaction نیز Host Environment مسئولیت مربوط به آن عملیات را بر عهده می‌گیرد.

نکته مهم این است که:

Host APIs محل اجرای معمول JavaScript Code نیستند.

آن‌ها امکاناتی را فراهم می‌کنند که JavaScript از طریق آن‌ها می‌تواند با محیط اجرا تعامل کند.

از Host API تا Queue

اکنون مسئله دیگری ایجاد می‌شود.

فرض کنید Timer تمام شده است.

آیا Callback باید همان لحظه وارد Call Stack شود؟

خیر.

چون ممکن است JavaScript هنوز در حال اجرای Code دیگری باشد.

مثلاً:

console.log('Start');

setTimeout(() => {
console.log('Timer finished');
}, 0);

console.log('End');

هنگامی که Timer آماده می‌شود، Callback آن باید در جایی منتظر بماند تا زمان مناسب اجرای آن فرا برسد.

این نقطه، ما را به مفهوم Task Queue می‌رساند.

Task Queue

Task Queue محلی است که Taskهای آماده اجرای JavaScript می‌توانند در آن منتظر بمانند.

مدل ساده:

Host API
↓
Task becomes ready
↓
Task Queue
↓
Wait for execution opportunity

در مثال Timer:

setTimeout(() => {
console.log('Timer finished');
}, 0);

پس از اینکه Timer آماده شد، Callback آن برای اجرای JavaScript در مسیر Queue قرار می‌گیرد.

اما هنوز یک شرط وجود دارد:

Call Stack باید برای اجرای آن آماده باشد.

اگر Call Stack هنوز مشغول اجرای Code دیگری باشد، Task منتظر می‌ماند.

Task با Execution یکی نیست

این تفاوت یکی از مهم‌ترین نکات Event Loop است.

فرض کنید Timer تمام شده است.

این اتفاق فقط یعنی:

Timer is ready

نه:

Callback is executing

بین این دو مرحله فاصله وجود دارد:

Timer completes
↓
Callback becomes ready
↓
Task Queue
↓
Execution opportunity
↓
Call Stack
↓
Callback executes

بنابراین مقدار 0 در:

setTimeout(callback, 0);

به معنای:

Callback را دقیقاً بعد از صفر میلی‌ثانیه اجرا کن.

نیست.

بلکه مفهوم آن به این نزدیک‌تر است:

Timer را با تأخیر صفر ثبت کن و Callback را پس از فراهم شدن شرایط مناسب برای اجرای آن قرار بده.

این تفاوت برای تحلیل دقیق Asynchronous JavaScript بسیار مهم است.

چرا Queue به تنهایی کافی نیست؟

اکنون یک سیستم داریم:

Call Stack
↑
Task Queue
↑
Host APIs

اما JavaScript فقط با Timerها کار نمی‌کند.

Promiseها و برخی عملیات دیگر نیازمند نوع دیگری از کارهای Asynchronous هستند که باید با اولویت متفاوتی نسبت به Taskهای معمول مدیریت شوند.

برای مثال:

Promise.resolve().then(() => {
console.log('Promise');
});

Callback مربوط به Promise مانند Timer معمولی در Task Queue قرار نمی‌گیرد.

این نوع کار به Microtask مربوط می‌شود.

Microtasks

Microtaskها نوع خاصی از کارهای آماده اجرای JavaScript هستند.

Promise Reactionها یکی از نمونه‌های مهم آن‌ها هستند.

برای مثال:

Promise.resolve().then(() => {
console.log('Promise');
});

Callback مربوط به then پس از آماده شدن Promise در مسیر Microtask قرار می‌گیرد.

مدل ساده:

Promise settles
↓
Microtask
↓
Microtask Queue

بنابراین اکنون Runtime حداقل دو مسیر مهم برای کارهای آماده اجرای JavaScript دارد:

Task Queue
Microtask Queue

اما تفاوت آن‌ها فقط در نام نیست.

ترتیب رسیدگی به آن‌ها اهمیت اساسی دارد.

چرا Microtaskها مهم هستند؟

فرض کنید Code زیر را داریم:

console.log('Start');

setTimeout(() => {
console.log('Timer');
}, 0);

Promise.resolve().then(() => {
console.log('Promise');
});

console.log('End');

خروجی:

Start
End
Promise
Timer

ممکن است سؤال ایجاد شود:

چرا Timer قبل از Promise اجرا نشد، در حالی که هر دو Asynchronous هستند؟

پاسخ در مدل Scheduling قرار دارد.

ابتدا Codeهای Synchronous اجرا می‌شوند:

Start
End

سپس Runtime به Microtaskها می‌رسد.

بنابراین:

Promise

اجرا می‌شود.

بعد نوبت Task مربوط به Timer می‌رسد:

Timer

در نتیجه:

Synchronous Code
↓
Microtasks
↓
Tasks

این ترتیب، بخش مهمی از مدل Event Loop است.

Event Loop

اکنون تمام قطعات موردنیاز را داریم:

Call Stack
Host APIs
Task Queue
Microtask Queue

اما هنوز یک سؤال مهم باقی مانده است:

چه چیزی بررسی می‌کند که Call Stack چه زمانی خالی شده و کدام کار باید وارد آن شود؟

پاسخ:

Event Loop

Event Loop یک Loop هماهنگ‌کننده در Runtime است که وضعیت اجرای JavaScript و کارهای آماده را بررسی می‌کند و تعیین می‌کند چه زمانی کار بعدی می‌تواند برای اجرا انتخاب شود.

مدل ذهنی ساده:

             ┌──────────────┐
             │   Host APIs  │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │    Queues    │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │  Event Loop  │
             └──────┬───────┘
                    ↓
             ┌──────────────┐
             │  Call Stack  │
             └──────────────┘

Event Loop خودش محل اجرای Callback نیست.

وظیفه اصلی آن هماهنگ کردن مسیر ورود کارهای آماده به اجرای JavaScript است.

مدل ذهنی کامل Event Loop

اکنون می‌توانیم کل جریان را یکجا ببینیم.

فرض کنید:

console.log('Start');

setTimeout(() => {
console.log('Timer');
}, 0);

Promise.resolve().then(() => {
console.log('Promise');
});

console.log('End');

Runtime را می‌توان به‌صورت مفهومی چنین دنبال کرد:

مرحله اول: اجرای Synchronous Code

ابتدا:

console.log('Start');

اجرا می‌شود.

خروجی:

Start

سپس setTimeout ثبت می‌شود و Timer توسط Host Environment مدیریت می‌شود.

بعد Promise آماده می‌شود و Reaction مربوط به آن به Microtask Queue می‌رود.

در نهایت:

console.log('End');

اجرا می‌شود.

اکنون خروجی:

Start
End

است.

مرحله دوم: بررسی Microtasks

Code اصلی به پایان رسیده و Call Stack آماده است.

Microtask مربوط به Promise آماده اجرا است.

پس:

console.log('Promise');

اجرا می‌شود.

خروجی:

Start
End
Promise
مرحله سوم: Task

اکنون نوبت Task مربوط به Timer است.

Callback آن وارد مسیر اجرای JavaScript می‌شود:

console.log('Timer');

خروجی نهایی:

Start
End
Promise
Timer

این ترتیب تصادفی نیست.

نتیجه مستقیم Scheduling Runtime است.

Event Loop و Task Scheduling

اکنون مفهوم مهم‌تری شکل می‌گیرد.

Event Loop فقط نمی‌گوید:

آیا چیزی در Queue وجود دارد؟

بلکه Runtime باید تعیین کند:

چه کاری در چه زمانی اجازه ورود به مسیر اجرای JavaScript را دارد؟

این همان Task Scheduling است.

مدل ذهنی ساده:

Call Stack busy?
│
├── Yes → Wait
│
└── No
↓
Check pending work
↓
Process Microtasks
↓
Select next Task
↓
Execute

در این مدل، خالی شدن Call Stack شرط مهمی برای ادامه اجرای کارهای آماده است.

اما خالی بودن Stack به‌تنهایی به معنای انتخاب تصادفی یک Callback نیست.

Runtime باید قواعد Scheduling خود را رعایت کند.

چرا Event Loop به Call Stack وابسته است؟

فرض کنید Code زیر اجرا می‌شود:

function heavyWork() {
for (let i = 0; i < 1_000_000_000; i++) {}
}

setTimeout(() => {
console.log('Timer');
}, 0);

heavyWork();

Timer ممکن است خیلی زود آماده شود.

اما تا زمانی که:

heavyWork();

در حال اجرا است، Call Stack آزاد نیست.

پس Callback Timer نمی‌تواند وسط اجرای آن وارد JavaScript شود.

مدل:

heavyWork()
↓
Call Stack Busy
↓
Timer becomes ready
↓
Task waits
↓
heavyWork finishes
↓
Call Stack becomes available
↓
Task can execute

این مثال نشان می‌دهد:

Asynchronous بودن یک عملیات به معنای اجرای هم‌زمان JavaScript نیست.

JavaScript همچنان Code خود را از طریق مسیر اجرای مشخصی پردازش می‌کند.

Execution Order

اکنون می‌توانیم مهم‌ترین نتیجه عملی Event Loop را بررسی کنیم:

برای پیش‌بینی خروجی Asynchronous Code، باید ترتیب ورود و اجرای کارها را تحلیل کنیم.

به‌جای اینکه Code را فقط از بالا به پایین بخوانیم، باید بپرسیم:

کدام بخش Synchronous است؟
کدام عملیات به Host Environment واگذار می‌شود؟
نتیجه آن عملیات چگونه به Queue بازمی‌گردد؟
آیا Callback یک Task است یا Microtask؟
Call Stack چه زمانی آزاد می‌شود؟
Runtime در آن نقطه کدام کار را انتخاب می‌کند؟

این روش تحلیل بسیار دقیق‌تر از این است که بگوییم:

«این Function چون Async است، بعداً اجرا می‌شود.»

کلمه «بعداً» برای تحلیل حرفه‌ای کافی نیست.

ما باید بدانیم:

بعد از چه چیزی؟
در کدام Queue؟
با چه Scheduling Rule؟
در چه نقطه‌ای از اجرای Stack؟
یک مثال تحلیلی

مثال زیر را در نظر بگیرید:

console.log('A');

setTimeout(() => {
console.log('B');
}, 0);

Promise.resolve().then(() => {
console.log('C');
});

console.log('D');

ابتدا فقط Code Synchronous را اجرا می‌کنیم:

A
D

Timer یک Task ایجاد می‌کند.

Promise یک Microtask ایجاد می‌کند.

پس از پایان Code اصلی:

Microtask
↓
Promise callback

قبل از Task Timer قرار می‌گیرد.

در نتیجه:

A
D
C
B

نکته مهم این است که این خروجی را نباید حفظ کنیم.

باید آن را از مدل ذهنی استخراج کنیم:

Synchronous
↓
Microtasks
↓
Tasks

این دقیقاً همان نوع دانشی است که در Debugging واقعی اهمیت دارد.

Microtask Queue می‌تواند اجرای Task را عقب بیندازد

Microtaskها یک ویژگی مهم دارند.

پس از پایان یک Task، Runtime Microtaskهای آماده را پردازش می‌کند و Microtaskها می‌توانند Microtaskهای جدید ایجاد کنند.

مثلاً:

Promise.resolve().then(() => {
console.log('A');

Promise.resolve().then(() => {
console.log('B');
});
});

Microtask دوم در نتیجه اجرای Microtask اول ایجاد می‌شود.

بنابراین:

Microtask A
↓
creates
↓
Microtask B

در مدل اجرای Microtaskها، Runtime باید Microtaskهای آماده را تا رسیدن به نقطه مناسب پردازش کند.

از دید مهندسی، این موضوع یک هشدار مهم ایجاد می‌کند:

ایجاد زنجیره بسیار طولانی Microtaskها می‌تواند رسیدن Runtime به Taskهای دیگر را به تأخیر بیندازد.

بنابراین Microtaskها «فقط Promiseهای کوچک» نیستند؛ آن‌ها بخشی از Scheduling Runtime هستند.

Event Loop و Browser Rendering

در Browser یک مسئله دیگر نیز وجود دارد.

Browser فقط JavaScript اجرا نمی‌کند.

باید UI را نیز به‌روزرسانی کند.

برای مثال:

تغییرات DOM باید نمایش داده شوند.
صفحه باید دوباره Paint شود.
تعامل کاربر باید پاسخ داده شود.

به همین دلیل Scheduling در Browser فقط به اجرای JavaScript محدود نیست.

Runtime و Browser باید فرصت‌هایی برای رسیدگی به سایر کارهای محیط اجرا نیز داشته باشند.

این موضوع به ما کمک می‌کند بفهمیم چرا یک JavaScript Application با Codeهای طولانی یا Microtaskهای بیش از حد می‌تواند باعث کند شدن پاسخ‌گویی UI شود.

اما نکته اصلی این فصل همچنان همان است:

JavaScript Execution
+
Runtime Scheduling
+
Browser Responsibilities

Event Loop بخشی از این هماهنگی را شکل می‌دهد.

Event Loop یک Thread جدید ایجاد نمی‌کند

یک سوءبرداشت رایج این است:

Event Loop باعث می‌شود JavaScript چند کار را هم‌زمان اجرا کند.

این مدل ذهنی دقیق نیست.

Event Loop خودش Thread جدیدی برای اجرای JavaScript ایجاد نمی‌کند.

JavaScript همچنان Code خود را در مسیر اجرای JavaScript پردازش می‌کند.

آنچه Asynchronous Programming را ممکن می‌کند، همکاری چند بخش است:

JavaScript Execution
↓
Host Environment
↓
Queues
↓
Event Loop
↓
JavaScript Execution

بنابراین باید بین:

Concurrency

و:

Parallel Execution

تفاوت بگذاریم.

Event Loop به Runtime اجازه می‌دهد کارهای مختلف را بدون قرار دادن همه آن‌ها در یک اجرای طولانی Synchronous هماهنگ کند.

این به معنای اجرای هم‌زمان چند قطعه JavaScript روی یک Call Stack نیست.

Event Loop را چگونه در Debugging استفاده کنیم؟

وقتی رفتار Asynchronous یک Application غیرمنتظره است، به‌جای حدس زدن، باید مسیر اجرا را بازسازی کنیم.

برای مثال اگر انتظار داریم:

A
B
C

اما خروجی:

A
C
B

است، سؤال درست این نیست:

چرا JavaScript ترتیب Code را رعایت نکرد؟

بلکه باید بپرسیم:

A → Synchronous؟

B → Task یا Microtask؟

C → Task یا Microtask؟

چه چیزی قبل از B وارد Queue شد؟

Call Stack چه زمانی خالی شد؟

این روش، Debugging Asynchronous Code را از حد آزمون و خطا خارج می‌کند.

یک مدل ذهنی مهندسی

تا اینجا می‌توانیم Event Loop را با یک مدل واحد توضیح دهیم:

                 JavaScript Code
                       │
                       ▼
                 ┌───────────┐
                 │ Call Stack│
                 └─────┬─────┘
                       │
             Async Operation
                       │
                       ▼
                 ┌───────────┐
                 │ Host APIs │
                 └─────┬─────┘
                       │
              Operation completes
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
        Task Queue         Microtask Queue
             │                   │
             └─────────┬─────────┘
                       ▼
                 ┌───────────┐
                 │ Event Loop│
                 └─────┬─────┘
                       ▼
                 ┌───────────┐
                 │ Call Stack│
                 └───────────┘

این Diagram یک مدل ساده‌شده آموزشی است.

هدف آن نمایش تمام جزئیات داخلی Browser نیست.

هدف این است که رابطه علت و معلولی مفاهیم را ببینیم:

Call Stack
↓
JavaScript Execution

Host APIs
↓
Async Work

Queues
↓
Waiting Work

Event Loop
↓
Coordination

Execution Order
↓
Observable Behavior
Best Practices
1. Asynchronous بودن را با اجرای هم‌زمان اشتباه نگیرید

یک عملیات Asynchronous الزاماً به این معنا نیست که JavaScript آن را هم‌زمان با Code فعلی اجرا می‌کند.

2. به setTimeout(..., 0) به‌عنوان اجرای فوری نگاه نکنید

صفر بودن Delay به معنای اجرای فوری Callback نیست.

Callback باید وارد مسیر Scheduling شود و منتظر فرصت مناسب اجرای JavaScript بماند.

3. Execution Order را از روی مدل Runtime تحلیل کنید

به‌جای حفظ کردن خروجی مثال‌ها، مسیر زیر را بررسی کنید:

Synchronous Code
↓
Microtasks
↓
Tasks

و سپس شرایط واقعی مثال را تحلیل کنید.

4. Microtaskها را دست‌کم نگیرید

Microtaskهای زیاد یا زنجیره‌ای می‌توانند رسیدگی به Taskهای دیگر را به تأخیر بیندازند.

5. برای Debugging، Queue را فراموش نکنید

وقتی یک Callback «زودتر» یا «دیرتر» از انتظار اجرا می‌شود، فقط خود Callback را بررسی نکنید.

بپرسید:

این Callback از کدام مسیر به Execution رسیده است؟

اشتباهات رایج
اشتباه اول: Event Loop را محل اجرای JavaScript بدانیم

Event Loop خودش محل اجرای Callback نیست.

وظیفه آن هماهنگی بین اجرای JavaScript و کارهای آماده است.

اشتباه دوم: setTimeout(..., 0) را اجرای فوری بدانیم

صفر بودن Delay فقط باعث نمی‌شود Callback بلافاصله اجرا شود.

Call Stack و Scheduling همچنان باید در نظر گرفته شوند.

اشتباه سوم: Promise Callback را مانند Timer بدانیم

Promise Reaction در مسیر Microtask قرار می‌گیرد و Scheduling متفاوتی با Taskهای معمول دارد.

اشتباه چهارم: خالی شدن Timer را مساوی اجرای Callback بدانیم

آماده شدن یک عملیات با اجرای Callback آن یکی نیست.

Ready
≠
Executing
اشتباه پنجم: Event Loop را Thread جدید بدانیم

Event Loop برای هماهنگی Runtime است، نه ایجاد یک مسیر مستقل برای اجرای هم‌زمان JavaScript.

اشتباه ششم: خروجی مثال‌های Asynchronous را حفظ کنیم

اگر فقط خروجی:

A
C
B

را حفظ کنیم، در اولین مثال متفاوت دچار مشکل می‌شویم.

باید دلیل این ترتیب را بدانیم.

Summary

JavaScript برای اجرای Code از Call Stack استفاده می‌کند.

وقتی یک عملیات Asynchronous به Host Environment واگذار می‌شود، نتیجه آن لزوماً مستقیماً وارد Call Stack نمی‌شود.

پس از آماده شدن نتیجه، Callback می‌تواند در Queue مناسب قرار گیرد.

در اینجا دو مسیر مهم داریم:

Task Queue
Microtask Queue

Microtaskها برای برخی عملیات Asynchronous مانند Promise Reactions استفاده می‌شوند.

Event Loop وضعیت Call Stack و کارهای آماده را هماهنگ می‌کند تا کار مناسب در زمان مناسب وارد مسیر اجرای JavaScript شود.

در نتیجه ترتیب اجرای Asynchronous Code را نمی‌توان فقط با ترتیب نوشته شدن خطوط پیش‌بینی کرد.

باید Runtime را نیز در نظر گرفت:

Call Stack
↓
Host APIs
↓
Task Queues
↓
Microtasks
↓
Event Loop
↓
Task Scheduling
↓
Execution Order

این مدل ذهنی به ما اجازه می‌دهد رفتار Asynchronous JavaScript را تحلیل کنیم، نه اینکه خروجی مثال‌ها را حفظ کنیم.

Key Takeaways
Call Stack مسیر اجرای JavaScript Code را مدیریت می‌کند.
Host APIs امکانات محیط اجرا برای مدیریت عملیات خارج از اجرای مستقیم JavaScript را فراهم می‌کنند.
آماده شدن یک عملیات Asynchronous به معنای اجرای فوری Callback آن نیست.
Callbackهای آماده باید از مسیر Queue و Scheduling وارد اجرای JavaScript شوند.
Task Queue محل انتظار Taskهای آماده اجرای JavaScript است.
Microtask نوع دیگری از کارهای Asynchronous است که Scheduling متفاوتی دارد.
Promise Reactionها نمونه مهمی از Microtaskها هستند.
Microtaskها در ترتیب اجرای Code نسبت به Taskهای معمول اهمیت دارند.
Event Loop هماهنگ‌کننده بین Call Stack و کارهای آماده Runtime است.
setTimeout(..., 0) به معنای اجرای فوری Callback نیست.
Event Loop به معنای اجرای هم‌زمان چند قطعه JavaScript روی یک Call Stack نیست.
برای تحلیل حرفه‌ای Asynchronous Code باید Execution Order را از روی Runtime Model استنتاج کرد.
Technical Interview
Junior
سؤال 1: Event Loop چیست؟

پاسخ:

Event Loop سازوکاری در Runtime است که اجرای JavaScript را با کارهای آماده در Queueها هماهنگ می‌کند و زمانی که شرایط اجرای کار جدید فراهم باشد، آن را وارد مسیر اجرای JavaScript می‌کند.

سؤال 2: چرا setTimeout(fn, 0) فوراً اجرا نمی‌شود؟

پاسخ:

زیرا صفر بودن Delay فقط زمان آماده شدن Timer را مشخص می‌کند. Callback باید بعد از آماده شدن در مسیر Queue قرار گیرد و زمانی اجرا شود که Call Stack و Scheduling Runtime اجازه دهند.

سؤال 3: Call Stack چه نقشی دارد؟

پاسخ:

Call Stack محل مدیریت Execution Contextهای مربوط به Code در حال اجرای JavaScript است. تا زمانی که اجرای جاری Stack تمام نشده باشد، Callback جدید نمی‌تواند در همان مسیر اجرای JavaScript اجرا شود.

Mid-Level
سؤال 4: تفاوت Task و Microtask چیست؟

پاسخ:

هر دو نماینده کارهای آماده اجرای Asynchronous هستند، اما در Scheduling یکسان نیستند. Microtaskها، مانند Promise Reactionها، در نقطه‌ای پیش از رسیدگی به Task بعدی پردازش می‌شوند و بنابراین می‌توانند روی Execution Order اثر بگذارند.

سؤال 5: خروجی Code زیر چیست و چرا؟
console.log('A');

setTimeout(() => {
console.log('B');
}, 0);

Promise.resolve().then(() => {
console.log('C');
});

console.log('D');

پاسخ:

A
D
C
B

زیرا A و D به‌صورت Synchronous اجرا می‌شوند. Callback مربوط به Promise یک Microtask است و قبل از Task مربوط به Timer پردازش می‌شود.

سؤال 6: آیا setTimeout(..., 0) تضمین می‌کند Callback بعد از صفر میلی‌ثانیه اجرا شود؟

پاسخ:

خیر. 0 حداقل Delay مربوط به Timer را مشخص می‌کند، نه زمان دقیق اجرای Callback. اجرای Callback به وضعیت Call Stack و Scheduling Runtime نیز وابسته است.

Senior
سؤال 7: آیا Event Loop باعث اجرای هم‌زمان JavaScript می‌شود؟

پاسخ:

خیر. Event Loop وظیفه هماهنگ کردن کارهای آماده با مسیر اجرای JavaScript را بر عهده دارد. اجرای JavaScript در یک Call Stack انجام می‌شود و Event Loop به‌تنهایی Thread جدیدی برای اجرای هم‌زمان JavaScript ایجاد نمی‌کند.

سؤال 8: چرا Microtaskها می‌توانند اجرای Task بعدی را به تأخیر بیندازند؟

پاسخ:

زیرا Runtime قبل از رسیدگی به Task بعدی، Microtaskهای آماده را پردازش می‌کند. اگر اجرای یک Microtask باعث ایجاد Microtaskهای بیشتری شود، این زنجیره می‌تواند ادامه پیدا کند و رسیدن Runtime به Taskهای دیگر را عقب بیندازد.

سؤال 9: برای تحلیل یک رفتار غیرمنتظره در Asynchronous JavaScript چه مدل ذهنی استفاده می‌کنید؟

پاسخ:

ابتدا Code Synchronous را مشخص می‌کنم، سپس بررسی می‌کنم عملیات Asynchronous به کدام Host API واگذار شده و Callback آن در کدام Queue قرار می‌گیرد. سپس وضعیت Call Stack و Scheduling Microtaskها و Taskها را بررسی می‌کنم تا Execution Order را از روی Runtime Model استنتاج کنم.

Golden Answers
Event Loop در یک جمله چیست؟

Event Loop سازوکاری برای هماهنگ کردن اجرای JavaScript با کارهای آماده در Queueهای Runtime است.

چرا Timer صفر بلافاصله اجرا نمی‌شود؟

چون آماده شدن Timer با اجرای Callback یکسان نیست؛ Callback باید وارد مسیر Queue و Scheduling شود و منتظر فرصت اجرای JavaScript بماند.

چرا Promise معمولاً قبل از Timer اجرا می‌شود؟

زیرا Promise Reaction در مسیر Microtask قرار می‌گیرد و Microtaskها پیش از Task بعدی پردازش می‌شوند.

مهم‌ترین مدل ذهنی این فصل چیست؟

Asynchronous Code مستقیماً Call Stack را کنترل نمی‌کند؛ عملیات به Runtime واگذار می‌شود، نتیجه وارد Queue مناسب می‌شود و Event Loop ورود آن کار به اجرای JavaScript را هماهنگ می‌کند.

Conclusion

در این فصل، Event Loop را به‌عنوان یک مفهوم منفرد بررسی نکردیم.

از Call Stack شروع کردیم؛ زیرا JavaScript باید محلی برای اجرای Code داشته باشد.

سپس به Host APIs رسیدیم؛ زیرا عملیات Asynchronous نباید اجرای مستقیم JavaScript را برای تمام مدت خود متوقف کند.

بعد Task Queues را دیدیم؛ زیرا نتیجه یک عملیات آماده نمی‌تواند بدون توجه به وضعیت اجرای فعلی وارد Call Stack شود.

سپس Microtasks را وارد مدل کردیم؛ زیرا همه کارهای Asynchronous از یک مسیر Scheduling عبور نمی‌کنند.

در نهایت Event Loop را دیدیم؛ سازوکاری که این اجزا را در یک سیستم Scheduling به هم متصل می‌کند.

اکنون می‌توانیم یک سؤال مهم‌تر مطرح کنیم.

اگر Browser عملیات Network را نیز به Host Environment واگذار می‌کند، نتیجه این عملیات چگونه از Server به Application بازمی‌گردد و JavaScript چگونه با یک Server ارتباط برقرار می‌کند؟

این سؤال ما را به مفهوم بعدی می‌رساند:

Browser چگونه با Server ارتباط برقرار می‌کند؟