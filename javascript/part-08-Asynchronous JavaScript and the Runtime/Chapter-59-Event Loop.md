Chapter 59 — Event Loop
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

توضیح دهید چرا Call Stack به‌تنهایی برای مدیریت Async Tasks کافی نیست.
نقش Host APIs را در مدل اجرای Asynchronous توضیح دهید.
تفاوت Task و Microtask را تشخیص دهید.
توضیح دهید چرا Microtaskها در Execution Order اهمیت دارند.
نقش Event Loop را در هماهنگی Call Stack و Queueها تحلیل کنید.
ترتیب اجرای Synchronous Code، Microtask و Task را از روی مدل Runtime استنتاج کنید.
رفتار Codeهای Asynchronous را بدون حفظ کردن خروجی آن‌ها تحلیل کنید.
Core Question

JavaScript چگونه Async Tasks را با Call Stack و Queues هماهنگ می‌کند؟

مقدمه

تا اینجا می‌دانیم که JavaScript Code در مسیر مشخصی اجرا می‌شود و Functionهای در حال اجرا در Call Stack قرار می‌گیرند.

این مدل برای Codeهای Synchronous کاملاً قابل درک است.

مثلاً در:

function calculateTotal(price, tax) {
return price + tax;
}

const total = calculateTotal(100, 10);

اجرای calculateTotal وارد Call Stack می‌شود، اجرا می‌شود و پس از پایان از Stack خارج می‌شود.

اما وقتی با یک عملیات زمان‌بر روبه‌رو می‌شویم، مسئله تغییر می‌کند.

فرض کنید Application باید یک HTTP Request ارسال کند.

اگر JavaScript تا زمان دریافت Response در Call Stack منتظر بماند، اجرای Code متوقف می‌شود.

در فصل قبل دیدیم که Asynchronous Programming برای جلوگیری از چنین Blockingای مطرح می‌شود.

اما حالا یک سؤال جدید داریم:

اگر عملیات Asynchronous در Call Stack اجرا نمی‌شود، پس چه چیزی آن را مدیریت می‌کند؟

و سؤال مهم‌تر:

وقتی عملیات تمام شد، Callback آن چگونه دوباره به اجرای JavaScript بازمی‌گردد؟

برای پاسخ، باید مدل Execution را که قبلاً ساخته‌ایم یک مرحله گسترش دهیم.

وقتی Call Stack کافی نیست

Call Stack برای یک کار اساسی طراحی شده است:

مدیریت Codeای که همین حالا در حال اجرای JavaScript است.

اگر Functionی فراخوانی شود، Execution Context آن وارد Stack می‌شود.

اگر Function دیگری را فراخوانی کند، Context جدید روی آن قرار می‌گیرد.

در نهایت Functionها به‌ترتیب مناسب از Stack خارج می‌شوند.

اما یک عملیات Asynchronous لزوماً نمی‌تواند همین مسیر را دنبال کند.

مثلاً:

console.log('Start');

setTimeout(() => {
console.log('Finished');
}, 1000);

console.log('End');

اگر Timer مستقیماً در Call Stack باقی بماند، باید اجرای JavaScript تا پایان یک ثانیه متوقف شود.

اما خروجی واقعی چنین نیست:

Start
End
Finished

پس Timer نباید اجرای خود را در Call Stack نگه دارد.

اینجا اولین نیاز شکل می‌گیرد:

بخشی از Runtime باید بتواند عملیات Asynchronous را خارج از مسیر مستقیم اجرای JavaScript مدیریت کند.

این نیاز ما را به Host APIs می‌رساند.

Host APIs؛ وقتی Runtime بخشی از کار را بر عهده می‌گیرد

JavaScript Language به‌تنهایی تمام قابلیت‌های محیط اجرا را تعریف نمی‌کند.

Browser محیطی است که JavaScript در آن اجرا می‌شود و APIهایی در اختیار آن قرار می‌دهد.

برای مثال:

setTimeout(() => {
console.log('Finished');
}, 1000);

در این مثال JavaScript درخواست یک Timer را ثبت می‌کند.

مدل ساده اجرای آن چنین است:

JavaScript
↓
Call Stack
↓
Host API
↓
Timer

نکته مهم این است که Timer در طول این یک ثانیه در Call Stack منتظر نمی‌ماند.

Host Environment مسئول مدیریت آن عملیات است.

به همین دلیل JavaScript می‌تواند بلافاصله ادامه دهد:

console.log('End');

پس اکنون یک مشکل را حل کرده‌ایم:

عملیات Asynchronous لازم نیست تا پایان خود، Call Stack را اشغال کند.

اما هنوز مشکل اصلی حل نشده است.

وقتی عملیات تمام می‌شود، چه اتفاقی می‌افتد؟

فرض کنید Timer تمام شده است.

Callback زیر آماده اجرا است:

() => {
console.log('Finished');
}

اما یک سؤال مهم وجود دارد:

آیا این Callback می‌تواند مستقیماً وارد Call Stack شود؟

خیر.

ممکن است JavaScript هنوز در حال اجرای Code دیگری باشد.

برای مثال:

setTimeout(() => {
console.log('Timer');
}, 0);

for (let i = 0; i < 1_000_000_000; i++) {
// long-running synchronous work
}

حتی اگر Timer خیلی زود آماده شود، JavaScript هنوز مشغول اجرای Loop است.

پس Callback باید جایی منتظر بماند تا شرایط اجرای آن فراهم شود.

این نیاز، مفهوم Task Queue را ایجاد می‌کند.

Task Queue؛ کار آماده است، اما هنوز نوبت اجرا نیست

وقتی یک عملیات Asynchronous مانند Timer آماده می‌شود، Callback آن می‌تواند به‌عنوان یک Task برای اجرای JavaScript در نظر گرفته شود.

به‌صورت ساده:

Host API
↓
Operation completes
↓
Task
↓
Task Queue

Task Queue محلی برای انتظار Taskهایی است که آماده ادامه اجرای JavaScript هستند.

اما یک نکته بسیار مهم وجود دارد:

قرار گرفتن یک Callback در Queue به معنای اجرای آن نیست.

این دو مفهوم باید کاملاً از هم جدا شوند:

Ready
≠
Executing

برای مثال:

setTimeout(() => {
console.log('Timer');
}, 0);

عدد 0 به این معنا نیست که Callback دقیقاً در همان لحظه اجرا می‌شود.

Callback باید:

توسط Timer آماده شود.
در مسیر مناسب قرار گیرد.
منتظر فراهم شدن فرصت اجرای JavaScript بماند.
سپس وارد Call Stack شود.

پس:

setTimeout(..., 0) یک زمان دقیق برای اجرای Callback تعیین نمی‌کند.

این تفاوت یکی از پایه‌های درک Event Loop است.

اما همه کارهای Asynchronous یکسان نیستند

اکنون مدل ما چنین است:

Call Stack
↑
Task Queue
↑
Host APIs

اما JavaScript فقط با Timerها کار نمی‌کند.

برای مثال Promise نیز می‌تواند ادامه‌ای از Code را برای زمانی قرار دهد که نتیجه آن آماده شده است:

Promise.resolve().then(() => {
console.log('Done');
});

Callback مربوط به then مانند Timer معمولی در Task Queue قرار نمی‌گیرد.

این نوع کار یک Microtask است.

در نتیجه Runtime به مسیر دیگری نیز نیاز دارد:

Microtask
↓
Microtask Queue

اکنون دو نوع کار آماده داریم:

Task Queue
Microtask Queue

و اینجا مسئله مهم‌تری شکل می‌گیرد:

اگر هم Task و هم Microtask آماده باشند، کدام‌یک باید زودتر اجرا شود؟

Microtask؛ چرا Promise مسیر متفاوتی دارد؟

Microtask نوعی کار Asynchronous با Scheduling مشخص در Runtime است.

Promise Reactionها، مانند Callback مربوط به then، نمونه مهمی از Microtask هستند.

مثلاً:

Promise.resolve().then(() => {
console.log('Promise');
});

پس از آماده شدن Promise، Callback آن در Microtask Queue قرار می‌گیرد.

اکنون اگر Code زیر را اجرا کنیم:

console.log('Start');

setTimeout(() => {
console.log('Timer');
}, 0);

Promise.resolve().then(() => {
console.log('Promise');
});

console.log('End');

ابتدا Codeهای Synchronous اجرا می‌شوند:

Start
End

سپس Promise Callback آماده است و Timer نیز Task خود را دارد.

اگر Runtime صرفاً بین Queueها به‌صورت تصادفی انتخاب می‌کرد، Execution Order قابل پیش‌بینی نبود.

اما چنین نیست.

Microtaskها در Scheduling جایگاه مشخصی دارند.

نتیجه:

Start
End
Promise
Timer

پس اکنون می‌توانیم علت این ترتیب را توضیح دهیم:

Synchronous Code
↓
Microtasks
↓
Tasks

این ترتیب چیزی نیست که باید حفظ شود.

باید آن را از مدل Runtime استنتاج کنیم.

چرا هنوز به Event Loop نیاز داریم؟

اکنون اجزای اصلی را داریم:

Call Stack
Host APIs
Task Queue
Microtask Queue

اما هنوز یک سؤال بی‌پاسخ داریم:

چه چیزی بررسی می‌کند Call Stack چه زمانی برای اجرای کار جدید آماده است؟

و:

چه چیزی هماهنگ می‌کند Task و Microtask چگونه دوباره وارد مسیر اجرای JavaScript شوند؟

اینجاست که Event Loop وارد مدل می‌شود.

Event Loop را می‌توان یک سازوکار هماهنگ‌کننده در Runtime دانست که وضعیت اجرای JavaScript و کارهای آماده را دنبال می‌کند.

مدل ذهنی ساده:

Host APIs
↓
Queues
↓
Event Loop
↓
Call Stack

Event Loop محل اجرای Callback نیست.

بلکه بخشی از سازوکاری است که تعیین می‌کند کار آماده چه زمانی می‌تواند وارد مسیر اجرای JavaScript شود.

Event Loop چگونه مسئله را حل می‌کند؟

اکنون همان مثال را دوباره ببینیم:

console.log('Start');

setTimeout(() => {
console.log('Timer');
}, 0);

Promise.resolve().then(() => {
console.log('Promise');
});

console.log('End');

در ابتدا:

console.log('Start');

اجرا می‌شود.

سپس Timer توسط Host Environment مدیریت می‌شود.

Promise نیز Microtask مربوط به خود را آماده می‌کند.

بعد:

console.log('End');

اجرا می‌شود.

در این مرحله Code اصلی Synchronous تمام شده است.

اکنون Runtime باید کارهای آماده را مدیریت کند.

Microtask مربوط به Promise آماده است:

Promise

پس اجرا می‌شود.

سپس Task مربوط به Timer اجرا می‌شود:

Timer

در نتیجه:

Start
End
Promise
Timer

اکنون دیگر فقط خروجی را نمی‌بینیم.

می‌توانیم علت خروجی را نیز توضیح دهیم.

از Call Stack تا Event Loop

اکنون می‌توانیم تمام مسیر را در یک مدل واحد ببینیم.

وقتی JavaScript یک عملیات Asynchronous را آغاز می‌کند:

Call Stack
↓
Host API

Host Environment عملیات را مدیریت می‌کند.

وقتی نتیجه آماده شد، کار مربوطه وارد مسیر Queue می‌شود:

Host API
↓
Task Queue

یا:

Host API
↓
Microtask Queue

سپس Runtime باید این کار آماده را در زمان مناسب وارد اجرای JavaScript کند.

اینجا Event Loop نقش هماهنگ‌کننده دارد:

Call Stack
↑
Event Loop
↑
Queues
↑
Host APIs

این همان مدلی است که باید در ذهن باقی بماند.

Task Scheduling؛ مسئله فقط «چه چیزی آماده است» نیست

ممکن است تصور کنیم Event Loop فقط یک کار ساده انجام می‌دهد:

اگر Stack خالی بود، چیزی از Queue بردار.

اما برای تحلیل دقیق‌تر، مسئله Scheduling اهمیت پیدا می‌کند.

فرض کنید:

setTimeout(() => {
console.log('Timer');
}, 0);

console.log('Start');

for (let i = 0; i < 1_000_000_000; i++) {
// synchronous work
}

Timer ممکن است آماده شود، اما Loop طولانی هنوز در حال اجرا است.

در نتیجه:

Timer ready
↓
Call Stack busy
↓
Timer waits
↓
Synchronous work finishes
↓
Execution can continue

پس آماده بودن یک Task کافی نیست.

وضعیت Call Stack نیز اهمیت دارد.

این همان جایی است که Task Scheduling از یک مفهوم ساده Queue فراتر می‌رود.

Execution Order را چگونه پیش‌بینی کنیم؟

وقتی Code Asynchronous می‌بینیم، نباید فقط خطوط را از بالا به پایین بخوانیم.

باید مسیر Runtime را بازسازی کنیم.

برای هر عملیات این سؤال‌ها را بپرسید:

۱. آیا این Code Synchronous است؟

اگر بله، مستقیماً در مسیر اجرای JavaScript قرار می‌گیرد.

۲. اگر Asynchronous است، چه چیزی آن را مدیریت می‌کند؟

ممکن است Host Environment مسئول آن باشد.

۳. پس از آماده شدن، کار در کدام مسیر قرار می‌گیرد؟

Task یا Microtask؟

۴. Call Stack چه زمانی آزاد می‌شود؟

تا زمانی که اجرای Synchronous ادامه دارد، Callback نمی‌تواند وسط آن وارد اجرای JavaScript شود.

۵. وقتی اجرای فعلی تمام شد، Scheduling چگونه ادامه پیدا می‌کند؟

در این مرحله تفاوت Microtask و Task اهمیت پیدا می‌کند.

این روش باعث می‌شود Execution Order را استنتاج کنیم، نه حفظ.

یک تحلیل کوتاه

Code زیر را در نظر بگیرید:

console.log('A');

setTimeout(() => {
console.log('B');
}, 0);

Promise.resolve().then(() => {
console.log('C');
});

console.log('D');

برای تحلیل آن:

ابتدا:

A
D

چون این دو Synchronous هستند.

سپس دو کار آماده داریم:

Microtask → C
Task → B

Microtask پیش از Task پردازش می‌شود.

پس:

C

و سپس:

B

خروجی نهایی:

A
D
C
B

نکته مهم این نیست که این چهار حرف را حفظ کنیم.

نکته این است که بتوانیم برای هر مثال مشابه، همین فرآیند تحلیل را تکرار کنیم.

Microtaskها و زنجیره اجرا

Microtask می‌تواند Microtask دیگری ایجاد کند.

برای مثال:

Promise.resolve().then(() => {
console.log('A');

Promise.resolve().then(() => {
console.log('B');
});
});

اجرای Microtask اول باعث ایجاد Microtask دوم می‌شود.

پس Runtime باید Microtaskهای آماده را طبق قواعد Scheduling خود پردازش کند.

این رفتار اهمیت عملی دارد.

اگر Application تعداد بسیار زیادی Microtask پشت سر هم ایجاد کند، رسیدگی به Taskهای دیگر می‌تواند به تأخیر بیفتد.

بنابراین Microtask فقط یک اصطلاح مربوط به Promise نیست.

Microtask بخشی از مدل Scheduling است و می‌تواند روی رفتار قابل مشاهده Application اثر بگذارد.

مدل ذهنی نهایی

اکنون می‌توانیم کل فصل را در یک جریان واحد خلاصه کنیم:

JavaScript Execution
↓
Call Stack
↓
Async Operation
↓
Host APIs
↓
┌──────┴──────┐
↓             ↓
Task       Microtask
Queue        Queue
└──────┬──────┘
↓
Event Loop
↓
Task Scheduling
↓
Call Stack
↓
Execution Order

این Diagram قرار نیست تمام جزئیات داخلی Browser را مدل کند.

هدف آن ساختن یک Mental Model قابل استفاده برای تحلیل رفتار Asynchronous JavaScript است.

از این مدل می‌توان برای پاسخ به سؤال‌هایی مانند این استفاده کرد:

چرا این Callback زودتر اجرا شد؟

چرا setTimeout(..., 0) فوراً اجرا نشد؟

چرا Promise Callback قبل از Timer اجرا شد؟

چرا یک عملیات Synchronous طولانی باعث تأخیر در Callback شد؟

پاسخ این سؤال‌ها دیگر حدس نیست.

آن‌ها از رابطه میان:

Call Stack
Host APIs
Queues
Event Loop
Scheduling

به‌دست می‌آیند.

Best Practices
رفتار Async را از روی مدل تحلیل کنید

به‌جای حفظ کردن خروجی مثال‌ها، مسیر اجرای آن‌ها را دنبال کنید.

Synchronous
→ Microtask
→ Task
setTimeout(..., 0) را اجرای فوری تصور نکنید

صفر بودن Delay به معنای اجرای فوری Callback نیست.

Callback باید وارد مسیر Scheduling شود.

Task و Microtask را از هم جدا نگه دارید

این دو اصطلاح فقط نام دو Queue نیستند؛ تفاوت آن‌ها روی Execution Order اثر می‌گذارد.

Call Stack را در تحلیل Async فراموش نکنید

حتی اگر یک Callback آماده باشد، اجرای Synchronous فعلی باید مسیر خود را طی کند.

Event Loop را با Call Stack یکی ندانید

Call Stack محل مدیریت Execution است.

Event Loop بخشی از سازوکار هماهنگی Runtime است.

اشتباهات رایج
۱. «Async یعنی هم‌زمان»

Asynchronous بودن به معنای اجرای هم‌زمان چند قطعه JavaScript در یک Call Stack نیست.

۲. «Timer صفر یعنی اجرای فوری»

0 به معنای اجرای فوری Callback نیست.

Callback هنوز تابع Scheduling Runtime است.

۳. «Callback آماده یعنی Callback در حال اجرا»

آماده بودن و اجرا شدن دو مرحله متفاوت هستند:

Ready
≠
Executing
۴. «همه Callbackها در یک Queue قرار می‌گیرند»

برای تحلیل Event Loop باید حداقل تفاوت Task و Microtask را در نظر گرفت.

۵. «خروجی مثال‌ها را حفظ می‌کنم»

حفظ کردن:

A
D
C
B

دانش قابل اتکایی ایجاد نمی‌کند.

باید بتوانید توضیح دهید چرا این ترتیب ایجاد شده است.

Summary

JavaScript Code در مسیر اجرای خود از Call Stack استفاده می‌کند.

اما عملیات Asynchronous نباید لزوماً Call Stack را تا زمان پایان خود اشغال کند.

به همین دلیل Host Environment بخشی از این عملیات را مدیریت می‌کند.

وقتی عملیات آماده ادامه می‌شود، Callback آن مستقیماً و بدون قاعده وارد Call Stack نمی‌شود؛ بلکه باید در مسیر Scheduling مناسب قرار گیرد.

در اینجا Task Queue و Microtask Queue وارد مدل می‌شوند.

Task و Microtask از نظر Scheduling یکسان نیستند و همین تفاوت می‌تواند Execution Order را تغییر دهد.

در نهایت Event Loop سازوکاری است که اجرای JavaScript را با کارهای آماده در Queueها هماهنگ می‌کند.

پس مدل اصلی فصل چنین است:

Call Stack
↓
Host APIs
↓
Task Queue
↓
Microtask Queue
↓
Event Loop
↓
Scheduling
↓
Execution Order

این مدل به ما اجازه می‌دهد رفتار Asynchronous JavaScript را از روی علت‌ها تحلیل کنیم، نه اینکه خروجی مثال‌ها را حفظ کنیم.

Key Takeaways
Call Stack مسیر اصلی اجرای JavaScript Code است.
عملیات Asynchronous می‌تواند برای مدیریت شدن به Host Environment واگذار شود.
آماده شدن یک Async Operation به معنای اجرای فوری Callback آن نیست.
Taskهای آماده می‌توانند در Task Queue منتظر بمانند.
Promise Reactionها نمونه‌ای از Microtask هستند.
Microtask و Task Scheduling یکسانی ندارند.
Call Stack باید در تحلیل Execution Order همیشه در نظر گرفته شود.
Event Loop وظیفه هماهنگ کردن اجرای JavaScript با کارهای آماده Runtime را بر عهده دارد.
setTimeout(..., 0) به معنای اجرای فوری Callback نیست.
Asynchronous بودن با اجرای هم‌زمان چند قطعه JavaScript یکسان نیست.
Execution Order باید از مدل Runtime استنتاج شود، نه حفظ شود.
Technical Interview
Junior
سؤال ۱: Event Loop چیست؟

پاسخ:

Event Loop سازوکاری در Runtime است که اجرای JavaScript را با کارهای آماده در Queueها هماهنگ می‌کند.

سؤال ۲: چرا setTimeout(fn, 0) بلافاصله اجرا نمی‌شود؟

پاسخ:

چون Callback باید پس از آماده شدن وارد مسیر Scheduling شود و اجرای آن به وضعیت Call Stack و Runtime Scheduling وابسته است.

سؤال ۳: Call Stack چه نقشی در Async JavaScript دارد؟

پاسخ:

Call Stack مسیر اجرای JavaScript را مدیریت می‌کند. تا زمانی که Code Synchronous در حال اجرا باشد، Callback جدید نمی‌تواند در همان مسیر وارد اجرا شود.

Mid-Level
سؤال ۴: تفاوت Task و Microtask چیست؟

پاسخ:

هر دو کار آماده اجرای JavaScript هستند، اما Scheduling آن‌ها متفاوت است. Microtaskها مانند Promise Reactionها در ترتیب اجرای خود پیش از Task بعدی پردازش می‌شوند.

سؤال ۵: خروجی Code زیر چیست؟
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

A و D Synchronous هستند. Callback مربوط به Promise یک Microtask است و پیش از Task مربوط به Timer پردازش می‌شود.

سؤال ۶: آیا setTimeout(..., 0) تضمین می‌کند Callback بعد از صفر میلی‌ثانیه اجرا شود؟

پاسخ:

خیر. صفر بودن Delay به معنای اجرای فوری Callback نیست. Callback پس از آماده شدن باید در مسیر Scheduling قرار گیرد و اجرای آن به شرایط Runtime وابسته است.

Senior
سؤال ۷: آیا Event Loop باعث اجرای هم‌زمان JavaScript می‌شود؟

پاسخ:

خیر. Event Loop وظیفه هماهنگی کارهای آماده با مسیر اجرای JavaScript را دارد. خودش مسیر مستقلی برای اجرای هم‌زمان JavaScript ایجاد نمی‌کند.

سؤال ۸: چرا یک عملیات Synchronous طولانی می‌تواند اجرای Timer را به تأخیر بیندازد؟

پاسخ:

زیرا تا زمانی که Code Synchronous در Call Stack در حال اجرا است، Callback Timer نمی‌تواند وارد مسیر اجرای JavaScript شود؛ حتی اگر Timer از قبل آماده شده باشد.

سؤال ۹: برای تحلیل Execution Order یک Code Asynchronous چه مراحلی را بررسی می‌کنید؟

پاسخ:

ابتدا Code Synchronous را مشخص می‌کنم، سپس عملیات واگذارشده به Host Environment را بررسی می‌کنم، بعد مشخص می‌کنم Callback در Task یا Microtask قرار می‌گیرد و در نهایت وضعیت Call Stack و Scheduling را برای تعیین Execution Order تحلیل می‌کنم.

Golden Answers
Event Loop چیست؟

Event Loop سازوکاری برای هماهنگ کردن اجرای JavaScript با کارهای آماده در Queueهای Runtime است.

چرا setTimeout(..., 0) فوری اجرا نمی‌شود؟

زیرا آماده شدن Timer با اجرای Callback یکی نیست؛ Callback باید وارد مسیر Scheduling شود و منتظر فرصت اجرای JavaScript بماند.

چرا Promise Callback می‌تواند قبل از Timer اجرا شود؟

زیرا Promise Reaction یک Microtask است و Microtaskها در Scheduling پیش از Task بعدی پردازش می‌شوند.

مهم‌ترین مدل ذهنی فصل چیست؟

عملیات Async از مسیر مستقیم Call Stack خارج می‌شود، Host Environment آن را مدیریت می‌کند، نتیجه در مسیر Queue مناسب قرار می‌گیرد و Event Loop ورود آن به اجرای JavaScript را هماهنگ می‌کند.

Conclusion

درک Event Loop زمانی ساده می‌شود که آن را به‌عنوان یک مفهوم مستقل حفظ نکنیم.

ما از Call Stack شروع کردیم؛ زیرا JavaScript برای اجرای Code به یک مسیر مشخص نیاز دارد.

اما Call Stack نمی‌تواند درگیر یک عملیات زمان‌بر بماند و هم‌زمان انتظار داشته باشیم Application پاسخ‌گو باقی بماند.

پس بخشی از کار به Host APIs واگذار می‌شود.

وقتی عملیات تمام می‌شود، Callback نمی‌تواند بدون توجه به وضعیت اجرای JavaScript وارد Call Stack شود.

پس به Queues نیاز داریم.

اما همه کارها یکسان نیستند.

Promise Reactionها در قالب Microtask وارد مدل می‌شوند و همین موضوع باعث می‌شود Scheduling آن‌ها با Taskهای معمول متفاوت باشد.

در نهایت Event Loop این اجزا را در یک مدل هماهنگ قرار می‌دهد.

بنابراین وقتی در یک Application با رفتار غیرمنتظره‌ای مانند این مواجه می‌شویم:

Why did this run before that?

نباید پاسخ را در حد «چون Async است» نگه داریم.

باید مسیر Runtime را دنبال کنیم:

Call Stack
↓
Host APIs
↓
Queues
↓
Event Loop
↓
Scheduling
↓
Execution Order

اکنون یک سؤال طبیعی باقی می‌ماند:

اگر Browser می‌تواند یک عملیات Asynchronous را به Host Environment واگذار کند، در یک HTTP Request دقیقاً چه چیزی بین Browser و Server اتفاق می‌افتد؟

این سؤال ما را به فصل بعد می‌رساند:

AJAX and HTTP Communication.