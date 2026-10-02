Chapter 65 — Async JavaScript Behind the Scenes
اهداف فصل

پس از مطالعه این فصل، خواننده باید بتواند:

رابطه async Function و Promise را توضیح دهد.
توضیح دهد await هنگام اجرای یک async Function چه تغییری در جریان اجرا ایجاد می‌کند.
مفهوم Suspension را از Blocking شدن JavaScript تفکیک کند.
توضیح دهد Continuation چیست و چرا بعد از await به آن نیاز داریم.
رابطه Continuation و Microtask را تحلیل کند.
نقش Call Stack و Event Loop را در ادامه اجرای async Function توضیح دهد.
ترتیب اجرای Synchronous Code و کدهای بعد از await را تحلیل کند.
یک مدل ذهنی یکپارچه از async/await، Promise، Microtask، Call Stack و Event Loop داشته باشد.
Core Question

async/await و Promise در Runtime واقعاً چگونه اجرا می‌شوند؟

مقدمه

در فصل قبل یاد گرفتیم که async و await نوشتن Promise-based Code را خواناتر می‌کنند.

برای مثال:

async function loadRecipe() {
const response = await fetch('/api/recipe');
const recipe = await response.json();

return recipe;
}

از دید خواننده، این کد تقریباً شبیه یک جریان عادی به نظر می‌رسد:

fetch
↓
response
↓
json
↓
recipe

اما Runtime چنین برداشتی ندارد.

fetch یک عملیات Async است و نتیجه آن در همان لحظه آماده نیست. از طرف دیگر، JavaScript نباید اجرای کل برنامه را متوقف کند تا Server پاسخ دهد.

پس وقتی Runtime به این خط می‌رسد:

const response = await fetch('/api/recipe');

چه اتفاقی می‌افتد؟

آیا JavaScript متوقف می‌شود؟

اگر متوقف نمی‌شود، اجرای loadRecipe چه می‌شود؟

و وقتی Response آماده شد، Runtime از کجا می‌داند اجرای Function را ادامه دهد؟

برای پاسخ به این سؤال‌ها باید اجرای async Function را مرحله‌به‌مرحله دنبال کنیم.

از async Function شروع کنیم

اولین نکته‌ای که باید بدانیم این است که async فقط یک کلمه برای مشخص کردن Functionهای Asynchronous نیست.

یک async Function قرارداد مشخصی با Runtime دارد:

نتیجه اجرای یک async Function همیشه در قالب یک Promise ارائه می‌شود.

برای مثال:

async function getRecipe() {
return 'Pasta';
}

const result = getRecipe();

console.log(result);

result مقدار 'Pasta' نیست.

بلکه یک Promise است که در نهایت به 'Pasta' Fulfill می‌شود.

می‌توانیم نتیجه را با Promise API دریافت کنیم:

getRecipe().then(recipe => {
console.log(recipe);
});

بنابراین از همین ابتدا باید رابطه زیر را در ذهن داشته باشیم:

async Function
↓
Promise

این رابطه برای ادامه بحث مهم است؛ زیرا await نیز در همین مدل Promise-based معنا پیدا می‌کند.

چرا await لازم می‌شود؟

وقتی نتیجه یک Function یک Promise است، می‌توانیم از .then() استفاده کنیم:

getRecipe()
.then(recipe => {
console.log(recipe);
});

اما اگر چند مرحله وابسته به یکدیگر داشته باشیم، زنجیره Promise می‌تواند جریان منطقی برنامه را از شکل طبیعی آن دور کند.

برای مثال:

fetch('/api/recipe')
.then(response => response.json())
.then(recipe => {
console.log(recipe);
});

async/await اجازه می‌دهد همین جریان را با ساختاری نزدیک‌تر به ترتیب منطقی عملیات بنویسیم:

async function loadRecipe() {
const response = await fetch('/api/recipe');
const recipe = await response.json();

console.log(recipe);
}

اما این شباهت ظاهری نباید ما را به یک نتیجه اشتباه برساند.

کد بعد از await واقعاً مثل یک خط عادی از کد Synchronous اجرا نمی‌شود.

برای فهم علت، باید ببینیم هنگام رسیدن به await چه اتفاقی برای Function می‌افتد.

وقتی اجرای Function به await می‌رسد

مثال ساده‌تری در نظر بگیریم:

async function loadRecipe() {
console.log('Start');

const recipe = await getRecipe();

console.log(recipe);
}

اجرای Function از ابتدای آن شروع می‌شود:

loadRecipe()
↓
Start

تا اینجا چیزی متفاوت از اجرای یک Function معمولی نمی‌بینیم.

اما بعد به این قسمت می‌رسیم:

await getRecipe();

getRecipe() یک Promise در اختیار await قرار می‌دهد.

فرض کنیم نتیجه هنوز آماده نیست.

در این لحظه Runtime نمی‌تواند این خط را کامل کند:

const recipe = ...

زیرا مقدار recipe هنوز مشخص نشده است.

اما آیا باید اجرای JavaScript را متوقف کند؟

خیر.

اگر JavaScript واقعاً در این نقطه متوقف می‌شد، عملیات دیگری که برنامه باید انجام دهد نیز نمی‌توانست اجرا شود.

پس Runtime راه دیگری دارد:

اجرای همان async Function را در این نقطه موقتاً Suspend می‌کند.

Suspension

Suspension یعنی اجرای فعلی async Function در نقطه await موقتاً متوقف می‌شود تا Promise مورد انتظار به نتیجه برسد.

این توقف را نباید با توقف JavaScript یا Blocking شدن Thread یکی بدانیم.

مثال:

async function loadRecipe() {
console.log('Start');

await getRecipe();

console.log('Recipe loaded');
}

loadRecipe();

console.log('Application continues');

وقتی loadRecipe() اجرا می‌شود:

Start

نمایش داده می‌شود.

سپس Function به await می‌رسد و Suspend می‌شود.

اما این به معنای آن نیست که برنامه کاملاً متوقف شده است.

کد بیرونی می‌تواند ادامه پیدا کند:

Start
Application continues

بنابراین:

await
↓
Suspend current async Function
↓
JavaScript can continue other work

این یکی از مهم‌ترین تفاوت‌های Async JavaScript با Blocking Execution است.

await نمی‌گوید:

«کل JavaScript صبر کن.»

بلکه می‌گوید:

«ادامه اجرای این Function را فعلاً نگه دار.»

اکنون سؤال بعدی طبیعی است:

وقتی Promise آماده شد، Runtime باید اجرای Function را از کجا ادامه دهد؟

Continuation

فرض کنید Function زیر را داریم:

async function loadRecipe() {
const response = await fetch('/api/recipe');

console.log(response);
}

وقتی اجرای Function در await Suspend می‌شود، بخشی از Function هنوز باید در آینده اجرا شود:

console.log(response);

Runtime باید بتواند این ادامه اجرای Function را حفظ کند.

به این بخش از اجرای آینده، Continuation می‌گوییم.

پس جریان اکنون چنین شده است:

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation

Continuation در واقع همان ادامه Logic است که باید بعد از آماده شدن نتیجه await اجرا شود.

اما هنوز یک مسئله باقی مانده است.

فرض کنید Promise آماده شد.

آیا Runtime باید همان لحظه وسط اجرای برنامه Continuation را اجرا کند؟

خیر.

اینجاست که Microtask وارد جریان می‌شود.

Continuation چگونه دوباره اجرا می‌شود؟

فرض کنیم Promise مورد انتظار Fulfilled شده است.

اکنون Runtime می‌داند که ادامه Function می‌تواند اجرا شود.

اما اجرای Continuation نباید به‌صورت مستقیم و ناگهانی وارد جریان فعلی شود.

ادامه اجرای async Function در مسیر Microtask قرار می‌گیرد.

مدل مفهومی:

Promise settles
↓
Continuation
↓
Microtask

بنابراین await فقط یک «منتظر ماندن» ساده نیست.

در واقع جریان Function را به دو قسمت تقسیم می‌کند:

Before await
↓
Suspension
↓
Continuation
↓
Microtask
↓
After await

به همین دلیل کدی که بعد از await نوشته شده است، بخشی از اجرای بعدی Function خواهد بود.

چرا حتی Promise آماده هم باعث ادامه فوری نمی‌شود؟

برای مشاهده این رفتار، لازم نیست حتی یک Request واقعی داشته باشیم.

می‌توانیم از Promiseای استفاده کنیم که از قبل Fulfilled است:

async function test() {
console.log('A');

await Promise.resolve();

console.log('B');
}

test();

console.log('C');

ممکن است در نگاه اول انتظار داشته باشیم:

A
B
C

اما خروجی واقعی:

A
C
B

است.

چرا؟

ابتدا test() اجرا می‌شود:

A

سپس به await می‌رسد.

حتی اگر:

Promise.resolve()

از قبل Fulfilled باشد، اجرای قسمت بعد از await همان لحظه ادامه پیدا نمی‌کند.

Continuation برای اجرای Async بعدی در قالب Microtask قرار می‌گیرد.

بنابراین کد Synchronous بیرونی ادامه پیدا می‌کند:

C

پس از آن Microtask اجرا می‌شود:

B

جریان واقعی:

A
↓
await
↓
Suspend
↓
C
↓
Microtask
↓
B

اکنون رابطه await و Microtask برایمان روشن‌تر شده است.

اما هنوز یک سؤال مهم باقی مانده است:

Microtask در نهایت کجا اجرا می‌شود؟

برای پاسخ باید به Call Stack برگردیم.

بازگشت به Call Stack

ما از فصل‌های قبلی می‌دانیم که JavaScript Code برای اجرا در Call Stack قرار می‌گیرد.

پس زمانی که loadRecipe() در حال اجرای بخش اول خود است، Function در مسیر اجرای JavaScript قرار دارد.

اما هنگام رسیدن به await:

Call Stack
↓
async Function
↓
await
↓
Suspension

ادامه Function دیگر در همان اجرای فعلی ادامه پیدا نمی‌کند.

بعد از آماده شدن Promise، Continuation به‌صورت Microtask آماده می‌شود:

Promise settles
↓
Continuation
↓
Microtask

اما Microtask برای اینکه واقعاً اجرا شود، باید دوباره وارد مسیر اجرای JavaScript شود.

یعنی در نهایت:

Microtask
↓
Call Stack
↓
Execute Continuation

در نتیجه، اجرای بخش بعد از await دوباره در Call Stack اتفاق می‌افتد.

Event Loop وارد جریان می‌شود

اکنون تقریباً تمام قطعات لازم را داریم.

می‌دانیم:

async Function با Promise کار می‌کند.
await اجرای Function را Suspend می‌کند.
ادامه Function یک Continuation دارد.
Continuation در مسیر Microtask قرار می‌گیرد.
اجرای JavaScript در Call Stack انجام می‌شود.

اما چه چیزی این جریان را با هم هماهنگ می‌کند؟

اینجا Event Loop وارد مدل می‌شود.

Event Loop مسئول هماهنگ کردن کارهای آماده‌شده با مسیر اجرای JavaScript است.

در مدل این فصل، جریان را می‌توان چنین دید:

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution

در اینجا Event Loop را نباید به‌عنوان محلی برای اجرای مستقل JavaScript تصور کنیم.

اجرای JavaScript در Call Stack اتفاق می‌افتد.

Event Loop در هماهنگی زمان‌بندی و بازگرداندن کارهای آماده‌شده به مسیر اجرای JavaScript نقش دارد.

به این ترتیب، Function می‌تواند از نقطه‌ای که Suspend شده بود دوباره ادامه پیدا کند.

Resume Execution

اکنون به آخرین مرحله می‌رسیم.

وقتی Promise آماده شده و Continuation از مسیر Microtask به اجرای JavaScript بازمی‌گردد، Function از نقطه بعد از await ادامه پیدا می‌کند.

برای مثال:

async function loadRecipe() {
console.log('Start');

const recipe = await getRecipe();

console.log(recipe);
}

جریان اولیه:

loadRecipe()
↓
Start
↓
await getRecipe()

در اینجا Function Suspend می‌شود.

بعد از آماده شدن Promise:

Promise settles
↓
Continuation
↓
Microtask

سپس Continuation دوباره وارد مسیر اجرای JavaScript می‌شود:

Call Stack
↓
Resume

و اجرای Function از این قسمت ادامه پیدا می‌کند:

console.log(recipe);

پس تمام جریان را می‌توان چنین خلاصه کرد:

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution

این همان Concept Flow تعیین‌شده برای این فصل است و هر Concept اکنون از مسئله‌ای که Concept قبلی ایجاد کرده است به وجود آمده است.

یک مثال کامل‌تر

اکنون این مدل را روی یک جریان واقعی‌تر بررسی کنیم:

async function loadRecipe() {
console.log('Loading...');

const response = await fetch('/api/recipe');

console.log('Recipe loaded');

return response;
}

loadRecipe();

console.log('Application continues');

ابتدا loadRecipe() فراخوانی می‌شود.

Function وارد جریان اجرای JavaScript می‌شود و:

Loading...

را چاپ می‌کند.

سپس به:

await fetch('/api/recipe');

می‌رسد.

fetch() یک Promise فراهم می‌کند، اما Response هنوز آماده نیست.

پس:

await
↓
Suspension

اجرای loadRecipe در این نقطه ادامه پیدا نمی‌کند.

اما برنامه متوقف نشده است.

بنابراین:

console.log('Application continues');

اجرا می‌شود:

Loading...
Application continues

بعداً Response آماده می‌شود.

Promise به نتیجه می‌رسد و ادامه Function باید اجرا شود:

Promise settles
↓
Continuation
↓
Microtask

سپس Continuation دوباره وارد مسیر اجرای JavaScript می‌شود و Function Resume می‌شود:

Recipe loaded

بنابراین کدی که از بیرون شبیه یک جریان خطی است، در Runtime به چند مرحله تقسیم شده است.

این دقیقاً دلیل اهمیت مدل Runtime است.

چرا await را نباید «صبر کردن» بدانیم؟

در گفتار روزمره، ممکن است بگوییم:

await منتظر Promise می‌ماند.

این جمله برای شروع یادگیری قابل استفاده است، اما اگر همین جمله را مدل دقیق Runtime بدانیم، باعث سوءبرداشت می‌شود.

چون ممکن است تصور کنیم:

await
↓
JavaScript stops
↓
Promise finishes
↓
JavaScript continues

مدل دقیق‌تر این است:

await
↓
Suspend current async Function
↓
Other JavaScript work can continue
↓
Promise settles
↓
Continuation
↓
Microtask
↓
Resume

پس چیزی که Suspend می‌شود جریان همان Function است، نه اجرای کل JavaScript.

یک مثال برای تحلیل ترتیب اجرا

کد زیر را بررسی کنیم:

console.log('A');

async function test() {
console.log('B');

await Promise.resolve();

console.log('C');
}

test();

console.log('D');

برای تحلیل، ابتدا کد Synchronous را دنبال می‌کنیم.

اول:

A

سپس test() اجرا می‌شود:

B

بعد Function به await می‌رسد و Suspend می‌شود.

بنابراین اجرای test() در این نقطه ادامه پیدا نمی‌کند.

کد بیرونی ادامه می‌یابد:

D

در نهایت Continuation مربوط به await اجرا می‌شود:

C

پس خروجی:

A
B
D
C

است.

این نتیجه را نباید با حفظ کردن یک ترتیب خاص به خاطر سپرد.

اگر مدل زیر را در ذهن داشته باشیم، نتیجه قابل استنتاج است:

Synchronous Execution
↓
await
↓
Suspension
↓
Microtask
↓
Resume

این همان نوع Engineering Thinking است که هدف آن استنتاج رفتار Runtime به‌جای حفظ کردن خروجی مثال‌هاست.

Promise و async/await دو Runtime جدا نیستند

اکنون یک نتیجه مهم از این فصل به دست می‌آوریم.

ممکن است تصور کنیم Promise یک سیستم است و async/await سیستم دیگری.

اما چنین نیست.

فصل ۶۲ Promise را به‌عنوان مدل مدیریت نتیجه آینده یک عملیات Async معرفی کرد و فصل ۶۴ async/await را به‌عنوان Syntax خواناتری برای مدیریت Promise-based Code قرار داد.

بنابراین فصل ۶۵ قرار نیست یک Async Model جدید معرفی کند.

وظیفه این فصل این است که قطعاتی را که قبلاً ساخته شده‌اند در سطح Runtime به یکدیگر متصل کند:

Promise
↓
async
↓
await
↓
Runtime Execution

به همین دلیل فصل ۶۵ ادامه طبیعی فصل ۶۴ است، نه یک موضوع مستقل.

یک مدل ذهنی یکپارچه

اکنون اگر یک async Function را در یک Application واقعی ببینیم، می‌توانیم آن را در چند مرحله تحلیل کنیم.

مثلاً:

async function getRecipe() {
const response = await fetch('/api/recipe');

return response;
}

به‌جای اینکه فقط Syntax را ببینیم، Runtime را دنبال می‌کنیم:

مرحله اول — async Function

Function با مدل Promise-based اجرا می‌شود.

async Function
↓
Promise
مرحله دوم — await

Function به Promise می‌رسد:

Promise
↓
await
مرحله سوم — Suspension

نتیجه هنوز آماده نیست، بنابراین اجرای همان Function Suspend می‌شود:

await
↓
Suspension
مرحله چهارم — Continuation

Runtime باید بداند پس از آماده شدن نتیجه، اجرای Function از کجا ادامه پیدا کند:

Suspension
↓
Continuation
مرحله پنجم — Microtask

Continuation در مسیر Microtask قرار می‌گیرد:

Continuation
↓
Microtask
مرحله ششم — Call Stack

برای اجرای واقعی JavaScript، Continuation باید وارد مسیر اجرای JavaScript شود:

Microtask
↓
Call Stack
مرحله هفتم — Event Loop

Event Loop در هماهنگی این Scheduling با اجرای JavaScript نقش دارد:

Call Stack
↓
Event Loop
مرحله هشتم — Resume

در نهایت Function از نقطه بعد از await ادامه پیدا می‌کند:

Event Loop
↓
Resume Execution

بنابراین مدل کامل:

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution
یک سوءبرداشت مهم: await برابر با Blocking نیست

در Application واقعی ممکن است چنین کدی داشته باشیم:

async function loadRecipe() {
const response = await fetch('/api/recipe');

return response;
}

اگر fetch() چند صد میلی‌ثانیه یا حتی بیشتر طول بکشد، این به معنای آن نیست که JavaScript برای این مدت نمی‌تواند هیچ کار دیگری انجام دهد.

آنچه Suspend شده است:

loadRecipe()

است.

نه کل Runtime.

به همین دلیل Async JavaScript می‌تواند در Applicationهایی که عملیات I/O زیادی دارند، بدون آنکه هر عملیات Async کل جریان اجرای JavaScript را متوقف کند، به کار خود ادامه دهد.

رابطه فصل با Runtimeای که قبلاً ساخته‌ایم

در ابتدای کتاب Runtime یک‌باره آموزش داده نشد.

ابتدا Function و Invocation را شناختیم.

سپس Execution Context و Call Stack ساخته شد.

بعد Scope و Variable Lookup به این مدل اضافه شدند.

در مراحل بعد Browser Runtime و Host Environment معرفی شدند.

اکنون در بخش Async JavaScript، این مدل به Async Runtime می‌رسد. نقشه راه صراحتاً Runtime را به‌صورت لایه‌ای تعریف می‌کند و Async Runtime را بعد از Engine/Runtime Environment و Browser Host Layer قرار می‌دهد.

بنابراین Chapter 65 نباید به‌عنوان «فصل دیگری درباره Event Loop» خوانده شود.

این فصل یک هدف مشخص دارد:

نشان دادن اینکه async/await چگونه روی Runtime قبلی قرار می‌گیرد و چگونه Promise، Microtask، Call Stack و Event Loop را به یک جریان واحد متصل می‌کند.

Best Practices
مدل ذهنی خود را بر اساس Syntax نسازید

به‌جای اینکه فقط به:

await promise;

به‌عنوان یک دستور نگاه کنید، آن را بخشی از یک جریان Runtime ببینید:

await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Resume
await را با Blocking اشتباه نگیرید

اگر Function به await رسید، فقط اجرای همان Function Suspend می‌شود.

این تفاوت در طراحی Applicationهای Async اهمیت زیادی دارد.

برای تحلیل ترتیب اجرا، ابتدا Synchronous Code را جدا کنید

وقتی خروجی یک Async Program نامشخص است، ابتدا بخش Synchronous را مشخص کنید.

سپس نقاط await و Continuationهای مربوط به آن‌ها را بررسی کنید.

این روش بسیار قابل اعتمادتر از حدس زدن ترتیب اجرا از روی ظاهر کد است.

Promise و async/await را جدا از هم مدل نکنید

async/await ادامه همان Promise-based Model است.

پس برای تحلیل رفتار آن باید Promise را بخشی از مدل ذهنی خود بدانیم.

اشتباهات رایج
۱. «await کل JavaScript را متوقف می‌کند.»

خیر.

await اجرای همان async Function را Suspend می‌کند.

۲. «وقتی Promise Fulfilled شد، کد بعد از await همان لحظه اجرا می‌شود.»

خیر.

ادامه Function در مسیر Microtask Scheduling قرار می‌گیرد و بعداً Resume می‌شود.

۳. «async فقط باعث می‌شود Function Asynchronous به نظر برسد.»

خیر.

async Function رفتار مشخص Promise-based دارد و نتیجه آن یک Promise است.

۴. «Promise و Microtask یک چیز هستند.»

خیر.

Promise مدل مدیریت نتیجه آینده یک عملیات است؛ Microtask بخشی از سازوکار Scheduling اجرای JavaScript است.

۵. «Event Loop محل اجرای JavaScript است.»

خیر.

اجرای JavaScript در Call Stack انجام می‌شود و Event Loop در هماهنگی Scheduling نقش دارد.

۶. «کد بعد از await همان ادامه مستقیم اجرای فعلی Function است.»

از نظر مدل Runtime، این نگاه دقیق نیست.

بخش بعد از await بخشی از Continuation است که بعداً Resume می‌شود.

Summary

در این فصل، مسئله اصلی این بود که بفهمیم پشت Syntax ساده‌ای مانند:

const recipe = await getRecipe();

چه اتفاقی می‌افتد.

ابتدا دیدیم که async Function نتیجه خود را در قالب Promise ارائه می‌کند.

سپس دیدیم که await برای مدیریت جریان Promise-based Code استفاده می‌شود.

اما وقتی اجرای Function به await می‌رسد، JavaScript کل برنامه را متوقف نمی‌کند.

اجرای همان Function Suspend می‌شود.

از آنجا که Function باید بعداً ادامه پیدا کند، بخش باقی‌مانده Logic به‌عنوان Continuation در نظر گرفته می‌شود.

پس از آماده شدن Promise، Continuation در مسیر Microtask قرار می‌گیرد.

Microtask باید دوباره وارد مسیر اجرای JavaScript شود و اجرای آن در Call Stack اتفاق می‌افتد.

Event Loop در هماهنگی این Scheduling با مسیر اجرای JavaScript نقش دارد.

در نهایت Function از نقطه بعد از await Resume می‌شود.

بنابراین:

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution

این فصل لایه Async Runtime را روی لایه‌های Runtime قبلی بنا می‌کند؛ همان رویکردی که معماری کتاب برای آموزش Runtime به‌صورت تدریجی تعیین کرده است.

Key Takeaways
async Function یک Promise برمی‌گرداند.
async/await بر پایه Promise است.
await اجرای همان async Function را Suspend می‌کند.
Suspension به معنای Blocking شدن کل JavaScript نیست.
کد بعد از await بخشی از Continuation است.
Continuation پس از آماده شدن Promise در مسیر Microtask قرار می‌گیرد.
Microtask و Promise یک مفهوم نیستند.
اجرای واقعی JavaScript در Call Stack اتفاق می‌افتد.
Event Loop در هماهنگی Scheduling نقش دارد.
Function پس از آماده شدن Continuation از نقطه بعد از await Resume می‌شود.
ترتیب ظاهری کد همیشه به‌تنهایی ترتیب واقعی اجرای Async Code را مشخص نمی‌کند.
مدل Runtime باید به‌جای حفظ کردن خروجی مثال‌ها، امکان استنتاج رفتار برنامه را فراهم کند.
Chapter 65 قرار نیست Promise یا async/await را دوباره آموزش دهد؛ هدف آن اتصال این مفاهیم به Runtime است.
Technical Interview
Junior
۱. async Function چیست؟

async Function تابعی است که نتیجه اجرای آن در قالب Promise ارائه می‌شود.

۲. await چه کاری انجام می‌دهد؟

await اجرای async Function را در نقطه مشخصی Suspend می‌کند تا Promise مورد انتظار به نتیجه برسد و سپس اجرای Function ادامه پیدا کند.

۳. آیا await باعث Blocking شدن JavaScript می‌شود؟

خیر. اجرای همان async Function Suspend می‌شود، نه کل JavaScript.

۴. آیا Promise و Microtask یکی هستند؟

خیر. Promise مدل مدیریت نتیجه آینده یک عملیات است؛ Microtask بخشی از سازوکار Scheduling اجرای JavaScript است.

Mid-Level
۵. وقتی اجرای async Function به await می‌رسد چه اتفاقی می‌افتد؟

Function در آن نقطه Suspend می‌شود. وقتی Promise به نتیجه برسد، ادامه اجرای Function به‌صورت Continuation در مسیر Microtask قرار می‌گیرد و بعداً از نقطه بعد از await Resume می‌شود.

۶. چرا این کد ابتدا C را چاپ می‌کند؟
async function test() {
console.log('A');

await Promise.resolve();

console.log('B');
}

test();

console.log('C');

چون اجرای test() در await Suspend می‌شود. کد Synchronous بیرونی ادامه پیدا می‌کند و C چاپ می‌شود. سپس Continuation مربوط به await در مسیر Microtask اجرا شده و B چاپ می‌شود.

خروجی:

A
C
B
۷. Continuation چیست؟

Continuation همان بخش Logicای از async Function است که باید پس از آماده شدن نتیجه await ادامه پیدا کند.

۸. Event Loop در این جریان چه نقشی دارد؟

Event Loop در هماهنگی کارهای آماده‌شده با مسیر اجرای JavaScript نقش دارد تا Continuation بتواند در زمان مناسب وارد مسیر اجرای JavaScript شود.

Senior
۹. چرا await را نباید به‌عنوان «توقف Thread» مدل کرد؟

زیرا چیزی که Suspend می‌شود اجرای همان async Function است، نه کل JavaScript.

مدل صحیح:

await
↓
Suspension
↓
Other JavaScript work
↓
Promise settles
↓
Continuation
↓
Microtask
↓
Resume
۱۰. چرا حتی await Promise.resolve() نیز باعث می‌شود کد بعد از آن بلافاصله اجرا نشود؟

زیرا await ادامه اجرای Function را به Continuation تبدیل می‌کند و ادامه آن در جریان Microtask Scheduling اجرا می‌شود؛ حتی اگر Promise از قبل Fulfilled باشد.

۱۱. رابطه Promise، await و Microtask چیست؟

Promise نتیجه آینده یک عملیات را مدل می‌کند. await اجرای async Function را تا مشخص شدن آن نتیجه Suspend می‌کند و Continuation مربوط به ادامه Function در مسیر Microtask Scheduling قرار می‌گیرد.

۱۲. رابطه Call Stack و Event Loop در این مدل چیست؟

Call Stack محل اجرای JavaScript است. Event Loop در هماهنگی Scheduling کارهای آماده‌شده با مسیر اجرای JavaScript نقش دارد. در مورد await، Continuation پس از طی مسیر Microtask Scheduling دوباره در مسیر اجرای JavaScript اجرا می‌شود.

Golden Answers
Junior

await چه می‌کند؟

await اجرای async Function را Suspend می‌کند تا نتیجه Promise مشخص شود. پس از آن، ادامه Function در مسیر Async اجرا می‌شود. await کل JavaScript را Blocking نمی‌کند.

Mid-Level

وقتی async Function به await می‌رسد چه اتفاقی می‌افتد؟

اجرای همان Function Suspend می‌شود. پس از آماده شدن Promise، Continuation مربوط به بخش بعد از await در مسیر Microtask قرار می‌گیرد و سپس Function از همان نقطه Resume می‌شود.

await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Resume
Senior

async/await در Runtime چگونه اجرا می‌شود؟

async Function نتیجه خود را به‌صورت Promise ارائه می‌کند. هنگام رسیدن اجرای Function به await، اجرای همان Function Suspend می‌شود. بخش باقی‌مانده به‌عنوان Continuation در نظر گرفته می‌شود. پس از آماده شدن Promise، Continuation در مسیر Microtask Scheduling قرار می‌گیرد و سپس از طریق مسیر اجرای JavaScript در Call Stack اجرا می‌شود. Event Loop در هماهنگی این Scheduling نقش دارد و Function در نهایت از نقطه بعد از await Resume می‌شود.

async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution
Conclusion

تا اینجا می‌دانستیم:

const recipe = await getRecipe();

چه کاری برای برنامه‌نویس انجام می‌دهد.

اما اکنون می‌توانیم توضیح دهیم Runtime برای اجرای همین یک خط چه مسیری را طی می‌کند.

async Function با Promise کار می‌کند.

await اجرای Function را Suspend می‌کند.

ادامه Logic به Continuation تبدیل می‌شود.

با آماده شدن Promise، Continuation در مسیر Microtask قرار می‌گیرد.

سپس این ادامه باید دوباره وارد مسیر اجرای JavaScript شود؛ جایی که Call Stack مسئول اجرای واقعی آن است و Event Loop در هماهنگی Scheduling نقش دارد.

در نهایت Function از همان نقطه‌ای که بعد از await قرار داشت Resume می‌شود.

بنابراین async/await یک سیستم جدا از Promise و Runtime نیست.

بلکه لایه‌ای است که روی همان Runtime موجود قرار می‌گیرد و به ما اجازه می‌دهد جریان Promise-based Code را با Syntax خواناتر بنویسیم.

اکنون یک مسئله جدید شکل می‌گیرد.

در مثال‌های این فصل فقط یک Async Operation داشتیم و توانستیم مسیر آن را از شروع تا Resume دنبال کنیم.

اما Application واقعی معمولاً فقط یک عملیات Async ندارد.

ممکن است چند Request مستقل داشته باشیم، چند عملیات وابسته به یکدیگر اجرا شوند، یا چند عملیات هم‌زمان برای رسیدن به یک نتیجه واحد مورد نیاز باشند.

در این شرایط سؤال بعدی طبیعی است:

چگونه چند Async Operation را به‌صورت قابل کنترل و قابل اعتماد مدیریت کنیم؟

این سؤال، ما را به فصل بعد یعنی Chapter 66 — Error Handling نمی‌برد؛ بلکه ابتدا باید خطاهایی را که ممکن است در همین جریان Async رخ دهند مدیریت کنیم. بنابراین قدم طبیعی بعدی، بررسی تفاوت خطای Synchronous و Asynchronous و نحوه مدیریت Promise Rejection است؛ موضوعی که Chapter 66 دقیقاً برای آن طراحی شده است.