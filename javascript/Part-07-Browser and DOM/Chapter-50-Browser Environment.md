Chapter 50 — Browser Environment
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

تفاوت JavaScript Language، ECMAScript، JavaScript Engine و Browser Host Environment را توضیح دهید.
مشخص کنید کدام قابلیت‌ها بخشی از خود زبان JavaScript و کدام‌یک توسط Browser فراهم می‌شوند.
نقش JavaScript Engine را در اجرای کد JavaScript توضیح دهید.
مفهوم Runtime Environment را از خود زبان JavaScript تفکیک کنید.
Browser را به‌عنوان یک Host Environment برای اجرای JavaScript تحلیل کنید.
مفهوم Host API و Web API را در سطح مناسب درک کنید.
توضیح دهید چرا قابلیت‌هایی مانند document، fetch و localStorage بخشی از هسته زبان ECMAScript نیستند.
رابطه مفهومی میان JavaScript، Engine، Browser و Web APIs را تحلیل کنید.
مرز میان Language و Host Environment را در پروژه‌های واقعی تشخیص دهید.
درک کنید چرا رفتار JavaScript در Browser با خود Language یکی نیست.
Core Question

Browser چه قابلیت‌هایی را در اختیار JavaScript قرار می‌دهد که بخشی از خود زبان نیستند؟

مقدمه

تا اینجا بیشتر مفاهیمی که درباره JavaScript یاد گرفته‌ایم، متعلق به خود Language بوده‌اند.

ما با let و const، Functionها، Scope، Objectها، Arrayها، Classها و بسیاری از قابلیت‌های دیگر کار کرده‌ایم.

برای مثال:

const user = {
name: 'Omid'
};

function greet(name) {
return `Hello ${name}`;
}

این کد به Browser وابسته نیست.

همین JavaScript می‌تواند در محیط‌های مختلف اجرا شود.

اما به محض اینکه بخواهیم با محیط اطراف برنامه تعامل کنیم، وضعیت تغییر می‌کند.

مثلاً:

document.querySelector('.user');

یا:

fetch('/api/recipes');

یا:

localStorage.setItem('theme', 'dark');

این قابلیت‌ها از کجا آمده‌اند؟

آیا document بخشی از JavaScript است؟

آیا fetch یکی از قابلیت‌های Functionها در ECMAScript است؟

آیا localStorage را خود JavaScript Language تعریف کرده است؟

پاسخ این پرسش‌ها یک مفهوم مهم را روشن می‌کند:

JavaScript Language با محیطی که JavaScript در آن اجرا می‌شود یکسان نیست.

برای درک این تفاوت باید چند لایه را از هم جدا کنیم:

JavaScript Language
↓
ECMAScript
↓
JavaScript Engine
↓
Runtime Environment
↓
Browser
↓
Host APIs
↓
Web APIs

این فصل دقیقاً همین مرز را می‌سازد.

JavaScript Language و ECMAScript

اول باید مشخص کنیم وقتی می‌گوییم JavaScript، دقیقاً درباره چه چیزی صحبت می‌کنیم.

JavaScript یک Programming Language است.

این Language قواعدی برای مواردی مانند:

Syntax
Variables
Values
Types
Functions
Objects
Operators
Control Flow
Classes
Modules

تعریف می‌کند.

استاندارد رسمی این Language با نام ECMAScript شناخته می‌شود.

بنابراین بهتر است این دو مفهوم را یکی ندانیم.

می‌توانیم به‌صورت ساده چنین تصور کنیم:

JavaScript
↓
Language
↓
ECMAScript Specification

ECMAScript مشخص می‌کند Language چگونه باید رفتار کند.

برای مثال:

const numbers = [10, 20, 30];

const doubled = numbers.map(number => number * 2);

مفاهیمی مانند:

const
Array
map
Function
Arrow Function

در قلمرو JavaScript Language قرار دارند و رفتار آن‌ها توسط استاندارد ECMAScript تعریف می‌شود.

اما اینجا یک سؤال مهم ایجاد می‌شود.

اگر ECMAScript Language را تعریف می‌کند، چه چیزی این Language را واقعاً اجرا می‌کند؟

JavaScript Engine

کد JavaScript باید توسط یک سیستم نرم‌افزاری اجرا شود.

این وظیفه بر عهده JavaScript Engine است.

JavaScript Engine نرم‌افزاری است که کد JavaScript را دریافت می‌کند و آن را اجرا می‌کند.

نمونه‌هایی از JavaScript Engineها عبارت‌اند از:

V8
SpiderMonkey
JavaScriptCore

برای مثال:

Chrome و بسیاری از محیط‌های مبتنی بر Chromium از V8 استفاده می‌کنند.
Firefox از SpiderMonkey استفاده می‌کند.
Safari از JavaScriptCore استفاده می‌کند.

اما نکته مهم این است که Engine خودش Browser نیست.

این دو مفهوم را نباید یکی دانست.

مدل ذهنی مناسب‌تر چنین است:

JavaScript Code
↓
JavaScript Engine
↓
Execution

Engine مسئول اجرای Language است.

اما Application معمولاً فقط به اجرای خود Language نیاز ندارد.

یک Browser باید امکانات بسیار بیشتری ارائه کند.

مثلاً Browser باید بتواند:

Web Page را مدیریت کند.
با Network ارتباط برقرار کند.
اطلاعات را ذخیره کند.
به URL دسترسی داشته باشد.
به User Interaction پاسخ دهد.
وضعیت Browser را در اختیار Application قرار دهد.

این قابلیت‌ها از ECMAScript نمی‌آیند.

پس به یک لایه دیگر نیاز داریم.

Runtime Environment

وقتی یک Language اجرا می‌شود، فقط خود Engine وجود ندارد.

Engine در یک Runtime Environment قرار می‌گیرد.

Runtime Environment یعنی مجموعه محیط و قابلیت‌هایی که برای اجرای واقعی برنامه در اختیار آن قرار گرفته‌اند.

این محیط بسته به محل اجرای JavaScript متفاوت است.

برای مثال:

Browser
Server
Other JavaScript Hosts

می‌توانند Runtime Environmentهای متفاوتی فراهم کنند.

در نتیجه یک کد JavaScript ممکن است در دو محیط اجرا شود، اما قابلیت‌های محیط اطراف آن یکسان نباشد.

این تفاوت برای یک Frontend Developer بسیار مهم است.

مثلاً یک Browser API مانند:

document

را نمی‌توان صرفاً به این دلیل که با JavaScript نوشته شده است، بخشی از Language دانست.

document توسط محیط Browser فراهم می‌شود.

از طرف دیگر، محیط‌های Server-Side مانند Node.js نیز مجموعه APIهای مخصوص خود را دارند.

بنابراین:

JavaScript Language
↓
Engine
↓
Runtime Environment

یک مدل ذهنی دقیق‌تر از تصور «JavaScript = Browser» ایجاد می‌کند.

Browser به‌عنوان Host Environment

اکنون می‌توانیم Browser را دقیق‌تر ببینیم.

Browser فقط یک نرم‌افزار برای نمایش Web Page نیست.

از دید اجرای JavaScript، Browser یک Host Environment نیز محسوب می‌شود.

یعنی محیطی را فراهم می‌کند که JavaScript در آن اجرا شود و بتواند با امکانات Web تعامل داشته باشد.

مدل کلی چنین است:

JavaScript Code
↓
JavaScript Engine
↓
Browser Host Environment
↓
Browser APIs
↓
Web Platform

در نتیجه Browser دو نقش مرتبط دارد:

اول، محیطی برای اجرای JavaScript فراهم می‌کند.

دوم، مجموعه‌ای از قابلیت‌ها را در اختیار JavaScript قرار می‌دهد.

این همان نقطه‌ای است که تفاوت Language و Host Environment اهمیت پیدا می‌کند.

Host APIs

اگر خود JavaScript Language همه قابلیت‌های لازم برای یک Application را فراهم نمی‌کند، چگونه می‌تواند با محیط اطراف خود ارتباط برقرار کند؟

پاسخ، APIs provided by the host environment است.

API در اینجا یعنی Interfaceای که محیط اجرا در اختیار JavaScript قرار می‌دهد.

برای مثال Browser قابلیت‌هایی را برای کار با مواردی مانند:

Document
Network
Storage
Location
Timers
Browser Information

فراهم می‌کند.

بنابراین وقتی در یک برنامه Frontend می‌نویسیم:

fetch('/api/recipes');

Function fetch چیزی نیست که صرفاً به دلیل وجود JavaScript Language در اختیار ما قرار گرفته باشد.

این قابلیت از محیط Web می‌آید.

همین مسئله درباره مواردی مانند:

localStorage

و:

document

نیز صدق می‌کند.

بنابراین باید بین دو گروه تفاوت بگذاریم.

قابلیت‌های Language

مانند:

const
let
function
class
Array
Object
Map
Promise
قابلیت‌های Host / Web Platform

مانند:

document
fetch
localStorage
navigator
location

البته این تقسیم‌بندی در سطح APIهای استاندارد جزئیات بیشتری دارد؛ اما برای مدل ذهنی این فصل همین مرزبندی بنیادی اهمیت دارد.

Web APIs

در محیط Browser، بسیاری از قابلیت‌های مورد استفاده Frontend Developer در قالب Web APIs ارائه می‌شوند.

Web APIها رابط‌هایی هستند که Web Platform برای تعامل Application با قابلیت‌های Browser فراهم می‌کند.

برای مثال:

fetch('/api/recipes');

به Application اجازه می‌دهد با منابع شبکه کار کند.

یا:

localStorage.setItem('theme', 'dark');

برای ذخیره‌سازی سمت Client استفاده می‌شود.

یا:

document.querySelector('.recipe');

برای دسترسی به Document استفاده می‌شود.

این مثال‌ها یک نکته بسیار مهم دارند:

JavaScript برای انجام بسیاری از کارهای واقعی در Browser، از قابلیت‌های Host Environment استفاده می‌کند.

بنابراین بهتر است Browser را به شکل یک جعبه ساده که «JavaScript را اجرا می‌کند» نبینیم.

مدل دقیق‌تر این است:

                Browser
        ┌─────────────────────┐
        │                     │
        │   JavaScript Engine │
        │          ↓          │
        │   JavaScript Code   │
        │                     │
        │   Browser APIs      │
        │                     │
        │   Web Platform      │
        │                     │
        └─────────────────────┘

JavaScript Engine اجرای Language را بر عهده دارد و Browser قابلیت‌های Host را فراهم می‌کند.

یک مثال واقعی

فرض کنید در یک Application فروشگاهی می‌خواهیم اطلاعات یک Product را از Server دریافت کنیم.

کد ممکن است چنین باشد:

const response = await fetch('/api/products/10');
const product = await response.json();

در اینجا چند مفهوم مختلف کنار هم قرار گرفته‌اند.

const و await بخشی از Language هستند.

اما:

fetch()

یک API مربوط به Web Platform است.

پس نباید همه اجزای این کد را یک چیز واحد تصور کنیم.

به‌صورت مفهومی:

JavaScript Language
↓
const / await
│
└─────────────┐
↓
Web API
↓
fetch

این تفکیک در پروژه‌های واقعی بسیار مهم است.

زیرا وقتی بدانیم یک قابلیت متعلق به Language است یا Host Environment، بهتر می‌توانیم رفتار آن را تحلیل کنیم.

window و Browser Global Environment

اکنون با یکی از مهم‌ترین اشیای محیط Browser روبه‌رو می‌شویم:

window

در Browser، window نماینده محیط Window مربوط به یک Document و محیط Browsing Context است.

برای مثال:

window.location

به اطلاعات Location مربوط به صفحه دسترسی دارد.

یا:

window.localStorage

به Storage مربوط به همان محیط دسترسی می‌دهد.

این نکته یک تفاوت مهم را روشن می‌کند.

window بخشی از ECMAScript Language نیست.

بلکه Browser آن را به‌عنوان بخشی از محیط خود در اختیار JavaScript قرار می‌دهد.

بنابراین:

ECMAScript
↓
Language Objects

Browser
↓
window
↓
Browser APIs

این یکی از واضح‌ترین مثال‌ها برای تفاوت JavaScript Language و Browser Environment است.

Global Object و Browser Environment

در فصل‌های قبل با مفهوم Global Environment و Global Scope آشنا شدیم.

در Browser، محیط Global با امکانات Browser گره می‌خورد.

به همین دلیل ممکن است در کدهای Frontend مستقیماً بنویسیم:

console.log(window);

یا:

console.log(location);

بدون اینکه قبل از آن Object دیگری تعریف کرده باشیم.

این موضوع نباید باعث شود تصور کنیم:

همه این قابلیت‌ها بخشی از ECMAScript هستند.

آنچه اتفاق افتاده این است که Browser محیط اجرای JavaScript را با قابلیت‌های مخصوص خود گسترش داده است.

این همان مرزی است که باید در ذهن داشته باشیم:

Language-defined capabilities
+
Host-provided capabilities
↓
Browser Runtime
document کجای این مدل قرار می‌گیرد؟

در اینجا ممکن است یک سؤال طبیعی ایجاد شود:

پس document چیست؟

document یکی از مهم‌ترین Interfaceهای Browser برای کار با Web Page است.

اما جزئیات آن موضوع فصل بعد است.

در این فصل فقط باید بدانیم:

document

بخشی از ECMAScript Language نیست.

Browser آن را برای تعامل JavaScript با Document در اختیار برنامه قرار می‌دهد.

بنابراین:

JavaScript
↓
Browser Environment
↓
document
↓
DOM

در فصل بعد مفهوم DOM را به‌صورت مستقل بررسی خواهیم کرد.

فعلاً همین مرزبندی کافی است.

JavaScript بدون Browser

برای درک بهتر این موضوع، Browser را از JavaScript جدا کنیم.

فرض کنید یک JavaScript Engine در محیطی غیر از Browser اجرا شود.

در این حالت هنوز می‌توانیم از بسیاری از قابلیت‌های Language استفاده کنیم.

برای مثال:

const numbers = [10, 20, 30];

const result = numbers.map(number => number * 2);

console.log(result);

این کد برای منطق JavaScript به Browser نیاز ندارد.

اما اگر بنویسیم:

document.querySelector('.recipe');

دیگر صرفاً به JavaScript Language متکی نیستیم.

به محیطی نیاز داریم که document را فراهم کند.

این تفاوت بسیار مهم است:

JavaScript Language
↓
Can execute independently of Browser

Browser APIs
↓
Require a compatible Host Environment

بنابراین JavaScript ذاتاً مساوی Browser نیست.

Browser فقط یکی از محیط‌هایی است که JavaScript را Host می‌کند.

یک مقایسه مهندسی

فرض کنید یک Function برای محاسبه قیمت نهایی داریم:

function calculateTotal(price, quantity) {
return price * quantity;
}

این Function به Browser وابسته نیست.

می‌توانیم آن را در محیط‌های مختلف اجرا کنیم.

اما Function زیر:

function getRecipe() {
return document.querySelector('.recipe');
}

به یک قابلیت Browser وابسته است.

بنابراین این دو Function از نظر Dependency یکسان نیستند.

اولی:

Pure Language Logic

دومی:

Browser-dependent Logic

این تفکیک در معماری Application اهمیت زیادی دارد.

هرچه Logic بیشتری مستقیماً به Browser APIs وابسته شود، وابستگی آن Logic به محیط اجرا بیشتر خواهد شد.

چرا این مرزبندی در Frontend مهم است؟

در یک پروژه واقعی، معمولاً دو نوع Logic را کنار هم داریم.

یک بخش درباره خود داده و Business Logic صحبت می‌کند:

function calculateDiscount(price, percentage) {
return price - price * percentage;
}

و بخش دیگری با Browser تعامل می‌کند:

document.querySelector('.price').textContent = price;

اولی به Language وابسته است.

دومی به Browser Environment وابسته است.

این تفاوت به ما کمک می‌کند Responsibilityها را بهتر تشخیص دهیم.

برای مثال اگر یک Function فقط محاسبات انجام می‌دهد، معمولاً نیازی ندارد به:

window
document
localStorage

وابسته باشد.

اما اگر مسئول تغییر UI باشد، طبیعی است که با Browser APIs تعامل کند.

پس شناخت Browser Environment فقط یک موضوع تئوری نیست.

مستقیماً به طراحی کد و مدیریت Dependencyها مربوط می‌شود.

Browser Runtime را چگونه مدل کنیم؟

اکنون می‌توانیم تمام مفاهیم را کنار هم قرار دهیم.

وقتی یک JavaScript Application در Browser اجرا می‌شود، بهتر است آن را چنین تصور کنیم:

JavaScript Code
↓
ECMAScript Language
↓
JavaScript Engine
↓
Browser Runtime Environment
↓
Browser Host APIs
↓
Web Platform

هر لایه مسئولیت متفاوتی دارد.

ECMAScript قواعد و رفتار Language را تعریف می‌کند.

JavaScript Engine کد Language را اجرا می‌کند.

Browser یک Host Environment فراهم می‌کند.

Web APIs قابلیت‌هایی را در اختیار Application قرار می‌دهند که خارج از هسته Language هستند.

این مدل ذهنی باعث می‌شود یک اشتباه رایج را کنار بگذاریم:

Browser، JavaScript Language نیست.

Browser محیطی است که JavaScript را اجرا می‌کند و قابلیت‌های بیشتری در اختیار آن قرار می‌دهد.

آیا هر چیزی که در Browser وجود دارد Web API است؟

خیر.

این عبارت را نباید بیش از حد ساده کنیم.

Browser یک سیستم بزرگ است که اجزای مختلفی دارد.

برای مثال:

Rendering
Networking
Storage
Document
Events
JavaScript Execution
Security

همگی بخشی از تجربه Web هستند، اما از نظر استاندارد و معماری دقیقاً یک مفهوم واحد نیستند.

همچنین برخی قابلیت‌ها توسط ECMAScript تعریف می‌شوند و برخی توسط استانداردهای Web مانند WHATWG و سایر استانداردهای مرتبط.

بنابراین هنگام صحبت درباره APIها بهتر است دقیق بگوییم:

این قابلیت توسط JavaScript Language تعریف شده است.

یا:

این قابلیت توسط Browser / Web Platform در اختیار JavaScript قرار گرفته است.

این دقت از ایجاد مدل ذهنی اشتباه جلوگیری می‌کند.

یک سوءبرداشت مهم درباره console

ممکن است در مثال‌های JavaScript بارها نوشته باشیم:

console.log('Hello');

و به همین دلیل تصور کنیم console نیز بخشی از هسته ECMAScript است.

اما این نتیجه‌گیری صحیح نیست.

console نیز از قابلیت‌های محیط اجراست و مشخصات و پیاده‌سازی آن را باید در چارچوب Host / Web Platform و محیط اجرای مربوطه بررسی کرد.

این مثال اهمیت یک اصل را نشان می‌دهد:

صرف اینکه یک API در تمام محیط‌های رایج JavaScript در دسترس است، به این معنی نیست که بخشی از ECMAScript Language است.

این اصل هنگام مقایسه Browser و Node.js اهمیت بیشتری پیدا می‌کند.

Browser و Server یکسان نیستند

اگر JavaScript را فقط در Browser دیده باشیم، ممکن است تصور کنیم تمام قابلیت‌های JavaScript در همه محیط‌ها یکسان هستند.

اما چنین نیست.

مثلاً Browser قابلیت‌هایی مانند:

document
window
localStorage

را فراهم می‌کند.

در محیط‌های Server-Side، چنین Browser Objectهایی به‌صورت پیش‌فرض وجود ندارند.

در عوض آن محیط می‌تواند APIهای مخصوص خودش را ارائه کند.

پس:

Same Language
↓
Different Host Environment
↓
Different Available APIs

این یکی از مهم‌ترین نتایج این فصل است.

خود JavaScript Language می‌تواند مشترک باشد، اما محیط اجرا می‌تواند متفاوت باشد.

چرا این موضوع برای Frontend Developer ضروری است؟

در توسعه Frontend، تقریباً همیشه با سه لایه سروکار داریم:

Language
↓
Runtime
↓
Browser

وقتی خطایی مانند:

document is not defined

می‌بینیم، شناخت این مدل به ما کمک می‌کند سؤال درست را مطرح کنیم.

مسئله الزاماً Syntax JavaScript نیست.

ممکن است کدی که نوشته‌ایم به یک Browser API وابسته باشد، اما در محیطی اجرا شود که Browser Environment وجود ندارد.

همین مدل ذهنی در Frameworkهایی مانند Next.js نیز اهمیت دارد؛ زیرا بخشی از کد ممکن است در محیطی غیر از Browser اجرا شود.

در چنین شرایطی دانستن اینکه یک API متعلق به Language است یا Browser Environment، مستقیماً در Debugging و Architecture به کار می‌آید.

مرز این فصل با فصل‌های بعد

تا اینجا فقط باید مرز اصلی را ساخته باشیم.

در این فصل وارد جزئیات:

DOM Tree
Event Propagation
Event Loop
Task Queue
Microtask Queue
Fetch Internals

نمی‌شویم.

این مفاهیم در فصل‌های بعدی جایگاه مشخص خود را دارند.

در فصل بعد، document و DOM را بررسی خواهیم کرد.

سپس Selection و Manipulation را یاد می‌گیریم.

بعد Eventها و Event Handling مطرح می‌شوند.

در بخش Async نیز رابطه Browser Runtime با Event Loop و اجرای غیرهمزمان بررسی خواهد شد.

بنابراین هدف این فصل ساختن Foundation است:

JavaScript
↓
Engine
↓
Runtime
↓
Browser
↓
Web APIs

بدون این Foundation، بسیاری از مفاهیم Browser ممکن است به شکل مجموعه‌ای از APIهای پراکنده به نظر برسند.

Best Practices
1. Language را از Host API جدا کنید

وقتی API جدیدی می‌بینید، مشخص کنید آیا بخشی از JavaScript Language است یا توسط محیط اجرا فراهم شده است.

مثلاً:

Array

با:

document

از یک جنس نیستند.

2. Browser را با JavaScript یکی ندانید

JavaScript یک Language است.

Browser یک Host Environment است که JavaScript را اجرا می‌کند و Web APIs را در اختیار آن قرار می‌دهد.

3. Dependencyهای Browser را آگاهانه مدیریت کنید

اگر Functionی مستقیماً از:

window
document
localStorage

استفاده می‌کند، آن Function به Browser Environment وابسته است.

این Dependency باید در طراحی Application در نظر گرفته شود.

4. هنگام Debugging ابتدا محیط اجرا را بررسی کنید

اگر APIای مانند:

document
window
localStorage

در دسترس نیست، قبل از بررسی Syntax از خود بپرسید:

آیا این کد واقعاً در Browser اجرا می‌شود؟

5. API را فقط به دلیل نام JavaScript به ECMAScript نسبت ندهید

وجود یک API در کد JavaScript به این معنی نیست که آن API توسط ECMAScript تعریف شده است.

همیشه بین:

Language API

و:

Host / Web API

تفاوت بگذارید.

Common Mistakes
اشتباه اول: JavaScript را همان Browser دانستن

❌

JavaScript همان محیطی است که Browser فراهم می‌کند.

✔

JavaScript یک Programming Language است و Browser یکی از Host Environmentهایی است که می‌تواند آن را اجرا کند.

اشتباه دوم: تصور اینکه document بخشی از JavaScript Language است

❌

document یکی از Objectهای اصلی JavaScript است.

✔

document توسط Browser برای تعامل با Document در اختیار JavaScript قرار می‌گیرد.

جزئیات آن در فصل DOM بررسی خواهد شد.

اشتباه سوم: تصور اینکه JavaScript Engine همان Browser است

❌

Chrome همان JavaScript Engine است.

✔

Chrome یک Browser است و از V8 به‌عنوان JavaScript Engine استفاده می‌کند.

Engine وظیفه اجرای JavaScript را بر عهده دارد، در حالی که Browser قابلیت‌های بسیار بیشتری فراهم می‌کند.

اشتباه چهارم: تصور اینکه همه JavaScript APIها در همه محیط‌ها وجود دارند

❌

اگر چیزی با JavaScript نوشته شده باشد، در هر Runtime در دسترس است.

✔

Language مشترک می‌تواند در Runtimeهای مختلف اجرا شود، اما Host Environmentها APIهای متفاوتی فراهم می‌کنند.

اشتباه پنجم: یکی دانستن Web API و ECMAScript API

❌

fetch یک قابلیت ذاتی ECMAScript است.

✔

fetch یک Web API است که توسط Web Platform تعریف شده است.

اشتباه ششم: ورود زودهنگام به Event Loop

در این فصل لازم نیست سازوکار:

Task Queue
Microtask Queue
Event Loop

را توضیح دهیم.

این مفاهیم در بخش Async Runtime بررسی خواهند شد.

در اینجا فقط باید بدانیم Browser یک Runtime Environment است و اجرای JavaScript در آن با قابلیت‌های Host همراه می‌شود.

Summary

در ابتدای فصل یک سؤال داشتیم:

Browser چه چیزی به JavaScript اضافه می‌کند؟

برای پاسخ باید ابتدا JavaScript Language را از محیط اجرای آن جدا کنیم.

JavaScript یک Programming Language است و ECMAScript استاندارد اصلی رفتار این Language را تعریف می‌کند.

کد JavaScript برای اجرا به یک JavaScript Engine نیاز دارد.

Engine وظیفه اجرای Language را بر عهده دارد.

اما Engine به‌تنهایی محیط کامل Application نیست.

برای اجرای واقعی JavaScript، یک Runtime Environment وجود دارد که بسته به Host می‌تواند قابلیت‌های متفاوتی ارائه کند.

Browser یکی از مهم‌ترین Host Environmentهای JavaScript است.

Browser علاوه بر Engine، مجموعه‌ای از قابلیت‌های Web Platform را در اختیار Application قرار می‌دهد.

این قابلیت‌ها شامل APIهایی برای کار با بخش‌هایی از محیط Web هستند.

بنابراین مدل ذهنی اصلی این فصل چنین است:

JavaScript Language
↓
ECMAScript
↓
JavaScript Engine
↓
Runtime Environment
↓
Browser
↓
Host APIs
↓
Web APIs
↓
Browser Runtime

در نتیجه وقتی می‌نویسیم:

document.querySelector(...)

نباید تصور کنیم document بخشی از Syntax یا هسته ECMAScript است.

و وقتی می‌نویسیم:

fetch(...)

نباید آن را صرفاً یکی دیگر از قابلیت‌های ذاتی Language بدانیم.

این APIها در چارچوب Browser و Web Platform در اختیار JavaScript قرار گرفته‌اند.

از اینجا یک مدل ذهنی مهم شکل می‌گیرد:

JavaScript Language چیزی است که اجرا می‌شود؛ Browser محیطی است که علاوه بر اجرای JavaScript، قابلیت‌های Web را نیز در اختیار آن قرار می‌دهد.

Key Takeaways
JavaScript یک Programming Language است.
ECMAScript استاندارد اصلی تعریف رفتار JavaScript Language است.
JavaScript Engine مسئول اجرای کد JavaScript است.
Engine و Browser یک مفهوم نیستند.
Browser یک Host Environment برای اجرای JavaScript است.
Runtime Environment مجموعه محیط و قابلیت‌های لازم برای اجرای Application را فراهم می‌کند.
Host Environment می‌تواند APIهای مخصوص خود را در اختیار JavaScript قرار دهد.
Web APIs بخشی از قابلیت‌های Web Platform هستند.
document بخشی از ECMAScript Language نیست.
fetch یک Web API است و بخشی از هسته ECMAScript نیست.
window از مفاهیم مهم Browser Environment است.
یک JavaScript Language مشترک می‌تواند در Runtimeهای مختلف اجرا شود.
Host Environmentهای مختلف می‌توانند APIهای متفاوتی ارائه کنند.
Browser را نباید با JavaScript یکی دانست.
JavaScript Engine را نیز نباید با Browser یکی دانست.
تشخیص مرز Language و Host Environment در Debugging و Architecture اهمیت دارد.
وابستگی مستقیم به window، document و سایر Browser APIs یعنی کد به Browser Environment وابسته است.
DOM و جزئیات document در فصل بعد بررسی می‌شوند.
Event Loop و Async Runtime در بخش Async JavaScript بررسی خواهند شد.
Technical Interview
سطح Junior
1. JavaScript چیست؟

JavaScript یک Programming Language است که رفتار و قابلیت‌های اصلی آن در استاندارد ECMAScript تعریف می‌شود.

2. ECMAScript چیست؟

ECMAScript استانداردی است که Syntax، Semantics و بسیاری از قابلیت‌های اصلی JavaScript Language را تعریف می‌کند.

3. JavaScript Engine چیست؟

JavaScript Engine نرم‌افزاری است که کد JavaScript را اجرا می‌کند؛ مانند V8، SpiderMonkey و JavaScriptCore.

4. آیا Browser همان JavaScript Engine است؟

خیر.

Browser یک Host Environment کامل است و JavaScript Engine تنها یکی از اجزای آن برای اجرای JavaScript است.

5. Web API چیست؟

Web API مجموعه‌ای از Interfaceها و قابلیت‌هایی است که Web Platform برای تعامل JavaScript با محیط Web در اختیار Application قرار می‌دهد.

6. آیا document بخشی از ECMAScript است؟

خیر.

document توسط Browser برای تعامل با Document در اختیار JavaScript قرار می‌گیرد.

سطح Mid-Level
7. تفاوت JavaScript Language و Browser Environment چیست؟

JavaScript Language قواعد و رفتار خود Language را تعریف می‌کند، در حالی که Browser Environment محیطی است که JavaScript را اجرا می‌کند و قابلیت‌های Web Platform را نیز در اختیار آن قرار می‌دهد.

8. تفاوت Engine و Runtime Environment چیست؟

Engine وظیفه اجرای JavaScript Language را بر عهده دارد؛ Runtime Environment محیط کامل‌تری است که Engine را همراه با قابلیت‌های Host موردنیاز Application در بر می‌گیرد.

9. چرا fetch را نباید بخشی از JavaScript Language بدانیم؟

زیرا fetch توسط Web Platform تعریف شده و برای ارتباط با Network Resource در محیط Web استفاده می‌شود؛ بنابراین یک Web API است، نه قابلیت ذاتی ECMAScript.

10. چرا یک کد JavaScript ممکن است در Browser اجرا شود اما در محیط دیگری خطا دهد؟

زیرا Host Environmentها APIهای متفاوتی فراهم می‌کنند.

برای مثال:

document.querySelector('.recipe');

به Browser Environment وابسته است و در محیطی که document را فراهم نمی‌کند، قابل استفاده نیست.

11. چرا شناخت Browser Environment در معماری Frontend اهمیت دارد؟

زیرا مشخص می‌کند کدام بخش از کد به Browser وابسته است و کدام بخش فقط به JavaScript Language متکی است. این تفکیک به مدیریت Dependency و طراحی قابل‌تست‌تر کمک می‌کند.

12. آیا وجود یک API در محیط JavaScript به این معنی است که آن API بخشی از ECMAScript است؟

خیر.

ممکن است API توسط Host Environment یا Web Platform فراهم شده باشد.

سطح Senior
13. چرا باید بین Language و Host Environment مرز مشخصی وجود داشته باشد؟

زیرا Language و Host مسئولیت‌های متفاوتی دارند. Language قواعد اجرای Program را تعریف می‌کند، در حالی که Host امکانات مخصوص محیط اجرا را فراهم می‌کند. این تفکیک امکان اجرای یک Language مشترک در محیط‌های مختلف را فراهم می‌کند.

14. اگر JavaScript Language یکسان باشد، چرا رفتار Application در Browser و Server می‌تواند متفاوت باشد؟

زیرا بخش Language می‌تواند مشترک باشد، اما Host Environmentها APIها و قابلیت‌های متفاوتی ارائه می‌کنند.

به‌صورت مفهومی:

Same Language
↓
Different Host
↓
Different APIs
15. چرا تشخیص Browser Dependency برای طراحی Application اهمیت دارد؟

زیرا Function یا Moduleای که مستقیماً به Browser APIs وابسته است، دیگر صرفاً به Language وابسته نیست.

برای مثال:

function updateTitle(title) {
document.title = title;
}

به Browser وابسته است.

در مقابل:

function formatTitle(title) {
return title.trim().toUpperCase();
}

به Browser API نیاز ندارد.

این تفکیک می‌تواند در طراحی، Testing و Reuse کد اهمیت داشته باشد.

16. آیا می‌توان گفت Browser فقط JavaScript Engine به‌علاوه چند API است؟

این مدل برای شروع بیش از حد ساده است.

Browser یک محیط پیچیده شامل Engine و مجموعه‌ای از قابلیت‌های Web Platform، Rendering، Networking، Storage، Security و سایر اجزاست.

اما برای درک رابطه JavaScript و Browser، مدل:

JavaScript
↓
Engine
↓
Browser Host
↓
Web APIs

یک مدل ذهنی مناسب و کاربردی است.

17. چرا وجود console.log() نباید باعث شود console را بخشی از ECMAScript فرض کنیم؟

زیرا در دسترس بودن یک API در محیط اجرای JavaScript لزوماً به معنی تعریف شدن آن در ECMAScript نیست.

console نیز در چارچوب Host / Web Platform و محیط اجرا تعریف می‌شود.

بنابراین باید منشأ API را از صرفاً نحوه استفاده از آن تشخیص داد.

Golden Answers
Junior — Golden Answer

تفاوت JavaScript و Browser چیست؟

JavaScript یک Programming Language است و Browser یک Host Environment برای اجرای آن است. Browser علاوه بر JavaScript Engine، قابلیت‌هایی مانند Web APIs را در اختیار JavaScript قرار می‌دهد.

JavaScript
↓
Engine
↓
Browser
↓
Web APIs
Mid-Level — Golden Answer

آیا document و fetch بخشی از JavaScript هستند؟

آن‌ها در کد JavaScript استفاده می‌شوند، اما بخشی از هسته ECMAScript Language نیستند. این قابلیت‌ها توسط Browser و Web Platform در اختیار JavaScript قرار می‌گیرند.

بنابراین:

ECMAScript
↓
Language Features

Browser / Web Platform
↓
document / fetch / other Web APIs
Senior — Golden Answer

چرا باید JavaScript Language را از Browser Runtime جدا کنیم؟

زیرا JavaScript Language و Host Environment مسئولیت‌های متفاوتی دارند. ECMAScript رفتار Language را تعریف می‌کند و JavaScript Engine آن را اجرا می‌کند؛ اما Browser به‌عنوان Host Environment قابلیت‌های مخصوص Web را از طریق APIهای خود در اختیار برنامه قرار می‌دهد.

در نتیجه یک Language مشترک می‌تواند در Runtimeهای مختلف اجرا شود، در حالی که APIهای Host می‌توانند متفاوت باشند.

این تفکیک در معماری، Debugging، Testing و تشخیص Dependencyهای محیطی اهمیت دارد.

Conclusion

درک Browser Environment از یک سؤال ساده شروع شد:

وقتی JavaScript در Browser اجرا می‌شود، چه چیزی فراتر از خود Language در اختیار آن قرار می‌گیرد؟

پاسخ، تفاوت میان Language و Host Environment است.

JavaScript Language توسط ECMAScript تعریف می‌شود.

JavaScript Engine مسئول اجرای آن است.

Browser یک Host Environment است که علاوه بر Engine، مجموعه‌ای از قابلیت‌های Web Platform را فراهم می‌کند.

در نتیجه وقتی با APIهایی مانند:

document
fetch
localStorage
window

کار می‌کنیم، باید بدانیم که در حال استفاده از قابلیت‌های محیط Browser هستیم، نه صرفاً Syntax و قابلیت‌های هسته JavaScript.

مدل ذهنی نهایی این فصل چنین است:

JavaScript Language
↓
ECMAScript
↓
JavaScript Engine
↓
Runtime Environment
↓
Browser Host
↓
Web APIs
↓
Browser Application

اکنون مرز اصلی روشن شده است.

اما این مرز سؤال بعدی را ایجاد می‌کند:

اگر Browser قابلیت document را در اختیار JavaScript قرار می‌دهد، document دقیقاً چه چیزی را مدل می‌کند و JavaScript چگونه ساختار یک Web Page را به‌صورت قابل دستکاری می‌بیند؟

پاسخ این سؤال ما را به DOM می‌رساند؛ موضوع فصل بعد.

Browser Environment
↓
document
↓
DOM
↓
Document Tree
↓
DOM Manipulation

و این دقیقاً نقطه‌ای است که JavaScript از اجرای Logic وارد تعامل مستقیم با ساختار Web Page می‌شود.