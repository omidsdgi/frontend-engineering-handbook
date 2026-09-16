Chapter 44 — Memory Management and References
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مفهوم Memory Management را در JavaScript توضیح دهید.
رابطه میان Value، Object و Reference را درک کنید.
توضیح دهید که JavaScript چگونه Memory موردنیاز Valueها را مدیریت می‌کند.
مفهوم Reachability را به‌عنوان معیار اصلی زنده‌بودن Objectها تحلیل کنید.
نقش Garbage Collector را در آزادسازی Memory توضیح دهید.
تفاوت بین از بین رفتن یک Reference و از بین رفتن خود Object را تشخیص دهید.
مفهوم Retained Reference را درک کنید.
توضیح دهید Memory Leak چگونه در یک Application ایجاد می‌شود.
برخی الگوهای رایج ایجاد Memory Leak را در Applicationهای واقعی تشخیص دهید.
بین مدیریت Memory توسط JavaScript و مدیریت مستقیم Memory در زبان‌های سطح پایین تفاوت قائل شوید.
Core Question

JavaScript چگونه Memory موردنیاز برنامه را مدیریت و Valueهای غیرقابل‌دسترسی را پاک‌سازی می‌کند؟

جریان این فصل:

Value
↓
Memory Allocation
↓
Reference
↓
Reachability
↓
Garbage Collection
↓
Memory Release
↓
Retained References
↓
Memory Leak

در فصل‌های قبل، Objectها، Referenceها، Constructorها، Prototypeها و Classها را بررسی کردیم.

اکنون یک سؤال طبیعی مطرح می‌شود:

وقتی Objectها در یک برنامه ایجاد می‌شوند، چه اتفاقی برای Memory مربوط به آن‌ها می‌افتد؟

اگر یک Object دیگر موردنیاز نباشد، آیا باید خودمان Memory آن را آزاد کنیم؟

و اگر نه، JavaScript چگونه تشخیص می‌دهد که یک Object دیگر موردنیاز نیست؟

پاسخ به این سؤال ما را به یکی از مهم‌ترین بخش‌های Runtime می‌رساند:

Memory Management.

مقدمه

هر برنامه برای اجرای Logic خود به داده نیاز دارد.

وقتی می‌نویسیم:

const user = {
name: 'Omid',
role: 'admin'
};

برنامه فقط یک Syntax را اجرا نمی‌کند.

یک Object ایجاد شده و برای نگهداری State آن به Memory نیاز است.

همین موضوع درباره Arrayها، Functionها و سایر Objectها نیز وجود دارد.

مثلاً:

const users = [
{ name: 'Omid' },
{ name: 'Sara' }
];

در اینجا Application با چند Value و Object مختلف کار می‌کند.

تا زمانی که این داده‌ها موردنیاز هستند، باید در دسترس باقی بمانند.

اما یک Application واقعی دائماً داده جدید ایجاد می‌کند.

Requestهای جدید دریافت می‌شوند.

Objectهای جدید ساخته می‌شوند.

Componentها ایجاد و حذف می‌شوند.

Event Listenerها اضافه می‌شوند.

Cacheها رشد می‌کنند.

اگر Memory مربوط به داده‌هایی که دیگر موردنیاز نیستند برای همیشه نگه داشته شود، مصرف Memory به‌مرور افزایش پیدا می‌کند.

بنابراین یک سیستم اجرایی باید بتواند بین دو وضعیت تفاوت بگذارد:

Object هنوز موردنیاز است

و:

Object دیگر قابل استفاده نیست

JavaScript برای این کار از مفهومی به نام Reachability استفاده می‌کند.

Memory در JavaScript

وقتی یک Value در برنامه ایجاد می‌شود، Runtime باید Memory مناسب برای نگهداری آن را مدیریت کند.

مثلاً:

const price = 1200;

یا:

const product = {
title: 'Laptop',
price: 1200
};

در هر دو حالت برنامه با Value کار می‌کند.

اما این دو Value از نظر مدل داده یکسان نیستند.

1200 یک Primitive Value است.

در حالی که Object مربوط به product یک Object است که دارای Identity مستقل است.

برای درک Memory Management لازم نیست فرض کنیم که تمام Primitiveها حتماً روی Stack و تمام Objectها حتماً روی Heap قرار می‌گیرند.

این نوع جزئیات، بخشی از مدل الزامی JavaScript نیستند و Implementationهای مختلف Engine می‌توانند از روش‌های متفاوتی استفاده کنند.

مدل مهم‌تر برای یک JavaScript Developer این است:

Runtime مسئول تخصیص و مدیریت Memory مربوط به Valueها و Objectهای موردنیاز برنامه است.

بنابراین تمرکز ما باید روی Lifetime داده‌ها باشد، نه روی یک مدل فیزیکی خاص از Memory.

Allocation و Lifetime

هنگامی که برنامه به داده‌ای نیاز دارد، Runtime باید امکان نگهداری آن را فراهم کند.

این فرآیند را می‌توان به‌صورت مفهومی Memory Allocation دانست.

مثلاً:

const user = {
name: 'Omid'
};

در اینجا Object ایجاد شده و Program باید بتواند به آن دسترسی داشته باشد.

اما مهم‌تر از Allocation، سؤال دیگری است:

این Object تا چه زمانی باید در Memory باقی بماند؟

پاسخ به این سؤال به Referenceهای موجود به آن Object وابسته است.

اگر هنوز بخشی از برنامه بتواند به Object دسترسی پیدا کند، حذف آن Object باعث خراب شدن رفتار برنامه خواهد شد.

اما اگر دیگر هیچ مسیر قابل دسترسی به آن وجود نداشته باشد، نگهداری آن در Memory ضرورتی ندارد.

اینجا مفهوم Reachability وارد می‌شود.

Reference و Object

برای درک Reachability ابتدا باید رابطه بین Variable و Object را دقیق‌تر ببینیم.

فرض کنید:

const user = {
name: 'Omid'
};

Variable user خود Object نیست.

user یک Binding است که Value مربوط به Object را در اختیار برنامه قرار می‌دهد.

می‌توان این وضعیت را به‌صورت مفهومی چنین نمایش داد:

user
│
▼
Object
{
name: 'Omid'
}

اکنون اگر Variable دیگری به همان Object اختصاص داده شود:

const admin = user;

چه اتفاقی افتاده است؟

Object جدیدی ایجاد نشده است.

اکنون دو Binding به همان Object مربوط هستند:

user ───┐
│
▼
Object
▲
│
admin ──┘

بنابراین:

admin.name = 'Ali';

console.log(user.name);

خروجی:

Ali

چرا؟

زیرا user و admin به همان Object مربوط هستند.

این همان دلیلی است که باید میان Copying a Value و Sharing an Object تفاوت قائل شویم.

Reference فقط یک آدرس ساده نیست

گاهی برای توضیح Objectها گفته می‌شود:

Variable آدرس Memory Object را نگه می‌دارد.

این توضیح برای یک مدل ذهنی ابتدایی می‌تواند مفید باشد، اما تعریف دقیقی از Semanticهای JavaScript نیست.

مدل بهتر این است:

وقتی با Objectها کار می‌کنیم، چند Variable می‌توانند به یک Object واحد دسترسی داشته باشند.

بنابراین مسئله اصلی برای Memory Management این نیست که دقیقاً یک آدرس عددی در کجا قرار گرفته است.

مسئله اصلی این است:

آیا هنوز Reference قابل دسترسی به Object وجود دارد یا نه؟

Referenceهای متعدد

فرض کنید:

const product = {
title: 'Laptop'
};

const selectedProduct = product;

اکنون:

product
│
├──────► Product Object
│
selectedProduct

اگر یکی از Referenceها حذف شود:

selectedProduct = null;

هنوز:

product ─────► Product Object

وجود دارد.

بنابراین Object همچنان قابل دسترسی است.

اما اگر آخرین Reference نیز از بین برود:

product = null;

در این وضعیت دیگر از طریق این Referenceها به Object دسترسی نداریم.

البته برای تعیین اینکه Object واقعاً قابل جمع‌آوری است، باید تمام مسیرهای قابل دسترسی در Runtime در نظر گرفته شوند.

این مفهوم، همان Reachability است.

Reachability
Object چه زمانی Reachable است؟

یک Object زمانی Reachable است که از مسیرهای قابل دسترسی Runtime بتوان به آن رسید.

برای مثال:

const user = {
name: 'Omid'
};

تا زمانی که user در دسترس باشد، Object نیز Reachable است.

می‌توان وضعیت را چنین نمایش داد:

Reachable Root
│
▼
user
│
▼
Object

اما اگر Reference دیگری به Object وجود داشته باشد، حذف یک Reference لزوماً Object را غیرقابل‌دسترسی نمی‌کند.

مثلاً:

const user = {
name: 'Omid'
};

const admin = user;

اگر:

user = null;

Object همچنان از طریق admin قابل دسترسی است:

admin
│
▼
Object

بنابراین هنوز Reachable است.

Reachability به‌عنوان معیار Lifetime

این مفهوم یکی از مهم‌ترین مدل‌های ذهنی این فصل است:

Lifetime یک Object به این وابسته است که آیا هنوز از مسیرهای قابل دسترسی می‌توان به آن رسید یا خیر.

بنابراین صرفاً این‌که یک Object دیگر در ظاهر مورد استفاده قرار نمی‌گیرد، برای قضاوت درباره Memory کافی نیست.

ممکن است یک Reference پنهان یا غیرمستقیم هنوز آن را نگه داشته باشد.

مثلاً:

const cache = [];

function addUser() {
const user = {
name: 'Omid'
};

cache.push(user);
}

پس از اجرای Function، ممکن است تصور کنیم user دیگر وجود ندارد.

Binding محلی user از بین می‌رود.

اما Object همچنان از طریق cache قابل دسترسی است:

cache
│
▼
Array
│
▼
User Object

بنابراین Object هنوز Reachable است.

Garbage Collection

اکنون مسئله اصلی مشخص شده است.

Runtime باید بتواند Objectهایی را که دیگر قابل دسترسی نیستند شناسایی کند.

JavaScript برای این کار از Garbage Collection استفاده می‌کند.

تعریف ساده

Garbage Collection فرآیندی است که در آن JavaScript Engine Memory مربوط به داده‌هایی را که دیگر قابل دسترسی نیستند، می‌تواند بازیابی کند.

به Objectی که دیگر از مسیرهای قابل دسترسی به آن نمی‌رسیم، از دید Garbage Collector نیازی نیست همچنان در Memory نگه داشته شود.

مدل ذهنی ساده:

Object Created
↓
Reachable
↓
Still Needed
↓
Reference Removed
↓
Unreachable
↓
Garbage Collection
↓
Memory Reclaimed
Garbage Collector چگونه تصمیم می‌گیرد؟

نباید Garbage Collection را به شکل:

«JavaScript Objectهایی را که دیگر استفاده نشده‌اند پیدا می‌کند»

تعریف کنیم.

این تعریف بیش از حد ساده است.

ممکن است Objectی در یک بخش از Application ظاهراً دیگر استفاده نشود، اما هنوز Referenceای به آن وجود داشته باشد.

معیار مهم‌تر:

آیا Object هنوز Reachable است؟

به همین دلیل Garbage Collection با مفهوم Reachability ارتباط مستقیم دارد.

به‌صورت مفهومی:

Reachable Object
↓
Keep

Unreachable Object
↓
Eligible for Collection

البته عبارت Eligible for Collection مهم است.

این به معنای آن نیست که Object دقیقاً همان لحظه حذف می‌شود.

زمان و نحوه اجرای Garbage Collection به JavaScript Engine و شرایط Runtime وابسته است.

Garbage Collection فوری نیست

فرض کنید:

let user = {
name: 'Omid'
};

user = null;

اکنون Reference موجود در user از بین رفته است.

اگر Reference دیگری به Object وجود نداشته باشد، Object دیگر Reachable نیست.

اما نباید تصور کنیم:

user = null
↓
Object immediately deleted

مدل صحیح‌تر:

user = null
↓
Object becomes unreachable
↓
Object becomes eligible for garbage collection
↓
Garbage Collector may reclaim its memory

بنابراین Garbage Collection یک فرآیند مدیریت‌شده توسط Engine است، نه عملیاتی که برنامه‌نویس در هر لحظه زمان آن را تعیین کند.

Garbage Collection و null

قرار دادن null در یک Variable گاهی باعث می‌شود Reference به Object حذف شود:

let user = {
name: 'Omid'
};

user = null;

اما null خودش یک دستور برای Garbage Collector نیست.

این کد صرفاً Binding user را از Object قبلی جدا می‌کند.

اگر Reference دیگری وجود داشته باشد:

let user = {
name: 'Omid'
};

const admin = user;

user = null;

Object همچنان Reachable است:

admin ─────► Object

بنابراین:

null کردن یک Reference، Object را به‌صورت خودکار Garbage نمی‌کند.

آنچه اهمیت دارد این است که آیا آخرین مسیر Reachable به Object از بین رفته است یا خیر.

Memory Release

هنگامی که Object دیگر Reachable نباشد، Garbage Collector می‌تواند Memory مربوط به آن را در فرآیند خود بازیابی کند.

این موضوع باعث می‌شود Developer معمولاً مجبور نباشد Memory مربوط به هر Object را به‌صورت دستی آزاد کند.

در زبان‌هایی که Manual Memory Management دارند، Developer ممکن است مسئولیت بیشتری برای آزاد کردن Memory داشته باشد.

اما JavaScript دارای Automatic Garbage Collection است.

بنابراین مدل کلی چنین است:

Application creates Object
↓
Object becomes Reachable
↓
Application uses Object
↓
References disappear
↓
Object becomes Unreachable
↓
Garbage Collector
↓
Memory can be reclaimed

این یکی از تفاوت‌های مهم JavaScript با زبان‌هایی است که مدیریت Memory در آن‌ها به‌صورت دستی انجام می‌شود.

آیا Garbage Collection یعنی Memory Leak غیرممکن است؟

خیر.

این یکی از مهم‌ترین سوءبرداشت‌ها درباره JavaScript است.

ممکن است JavaScript دارای Garbage Collector باشد و همچنان Application دچار Memory Leak شود.

چرا؟

زیرا Garbage Collector نمی‌تواند Objectی را که هنوز Reachable است، صرفاً به دلیل اینکه Application دیگر واقعاً به آن نیاز ندارد، حذف کند.

فرض کنید:

const cache = [];

function addUser() {
const user = {
name: 'Omid'
};

cache.push(user);
}

هر بار که addUser() اجرا شود، یک Object جدید ایجاد می‌شود.

اگر cache دائماً رشد کند:

cache
↓
User 1
User 2
User 3
User 4
...

همه این Objectها از طریق cache Reachable هستند.

Garbage Collector نمی‌تواند آن‌ها را حذف کند، حتی اگر Application دیگر واقعاً به آن‌ها نیاز نداشته باشد.

این همان جایی است که Retained References اهمیت پیدا می‌کنند.

Retained References

یک Reference زمانی مشکل‌ساز می‌شود که Object را بیشتر از مدت موردنیاز زنده نگه دارد.

مثلاً:

const cache = [];

function saveUser(name) {
const user = { name };

cache.push(user);
}

اگر هدف Cache کردن داده‌ها باشد، نگهداری Objectها می‌تواند کاملاً منطقی باشد.

اما اگر cache بدون محدودیت رشد کند و هیچ‌وقت پاک‌سازی نشود، Referenceهای موجود در آن باعث نگهداری Objectها می‌شوند.

در این وضعیت:

cache
↓
User Object
↓
Still Reachable
↓
Cannot be collected

بنابراین:

Memory Leak اغلب نتیجه Retained Reference است، نه شکست Garbage Collector.

Memory Leak
تعریف ساده

Memory Leak زمانی رخ می‌دهد که برنامه Memory مربوط به داده‌هایی را که دیگر واقعاً موردنیاز نیستند، به دلیل باقی ماندن Referenceها نگه می‌دارد.

به زبان ساده:

برنامه Object را دیگر نمی‌خواهد، اما هنوز راهی برای دسترسی به آن وجود دارد.

این دو مفهوم باید از یکدیگر جدا شوند:

Application no longer needs Object

و:

Object is unreachable

این دو الزاماً یکی نیستند.

ممکن است Application دیگر Object را لازم نداشته باشد، اما یک Reference همچنان آن را Reachable نگه دارد.

یک مثال واقعی‌تر

فرض کنید Application برای نگهداری داده‌های موقت یک Array دارد:

const cache = [];

function addData(data) {
cache.push(data);
}

اگر Application دائماً داده اضافه کند:

cache
├── data
├── data
├── data
├── data
└── ...

و هیچ سیاستی برای حذف داده‌های قدیمی وجود نداشته باشد، Memory مصرفی می‌تواند به‌مرور افزایش پیدا کند.

مشکل در اینجا این نیست که Garbage Collector کار نمی‌کند.

Garbage Collector کاملاً درست عمل می‌کند.

اما از دید Runtime:

cache → data

هنوز یک مسیر Reachable وجود دارد.
نسخه بهتر:

let user = {
name: 'Omid',
age: 58,
};

const cache = [];

cache.push(user);

// وقتی دیگر به user نیاز نداریم
cache.length = 0;
user = null;

حالا هیچ Referenceای به شیء باقی نمانده است و Garbage Collector می‌تواند آن را در صورت نیاز از حافظه خارج کند.
پس حذف Object از Memory از نظر Garbage Collector صحیح نیست.

Memory Leak در Closure

Closureها یکی دیگر از مواردی هستند که باید با دقت در نظر گرفته شوند.

در فصل Closure دیدیم که Function می‌تواند به Environment بیرونی خود دسترسی داشته باشد.

مثلاً:

function createCounter() {
let count = 0;

return function () {
count++;
return count;
};
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2

در این مثال، تابع داخلی یک Closure ایجاد کرده و به count دسترسی دارد. بنابراین count تا زمانی که counter به تابع داخلی Reference دارد، در دسترس باقی می‌ماند.

این Memory Leak نیست؛ زیرا این Reference هنوز مورد نیاز است.

اما اگر دیگر به counter نیاز نداشته باشیم:

let counter = createCounter();

console.log(counter()); // 1

counter = null;

اکنون اگر Reference دیگری به تابع داخلی وجود نداشته باشد، Closure و متغیر count نیز دیگر قابل دسترسی نیستند و می‌توانند توسط Garbage Collector آزاد شوند.

نکته مهم:

Closure زمانی می‌تواند در ایجاد مشکل حافظه نقش داشته باشد که یک Reference غیرضروری باعث شود Closure و داده‌های captured شده، بیشتر از Lifetime موردنیازشان در حافظه باقی بمانند.

بنابراین:

Closure ≠ Memory Leak

بلکه نگه‌داشتن Reference غیرضروری به Closure می‌تواند باعث نگه‌داشتن داده‌های بیشتری در حافظه شود.
Event Listener و Memory

در Applicationهای Browser، Event Listenerها نیز می‌توانند در Retaining Objectها نقش داشته باشند.

مثلاً:

const button = document.querySelector('#save');

function handleSave() {
// ...
}

button.addEventListener('click', handleSave);

اگر Lifecycle مربوط به یک بخش از Application تمام شود، باید بررسی کنیم که Listenerها و Referenceهای مرتبط نیز در زمان مناسب مدیریت شوند.

این مسئله در Applicationهای بزرگ، Componentهای دارای Lifecycle و سیستم‌هایی که به‌صورت پویا Elementها را ایجاد و حذف می‌کنند، اهمیت بیشتری پیدا می‌کند.

نکته اصلی این نیست که:

Event Listener همیشه Memory Leak ایجاد می‌کند.

بلکه:

Referenceهای باقی‌مانده می‌توانند Objectها یا بخش‌هایی از Application را بیشتر از Lifecycle موردنیازشان نگه دارند.

Memory Leak و Cache

Cache یکی از مهم‌ترین نمونه‌های واقعی است.

Cache اساساً برای نگهداری داده‌های قابل استفاده مجدد ایجاد می‌شود.

بنابراین وجود Reference در Cache ذاتاً مشکل نیست.

مشکل زمانی ایجاد می‌شود که Cache:

بدون محدودیت رشد کند.![img.png](img.png)
داده‌های قدیمی را حذف نکند.
Lifecycle مشخصی نداشته باشد.
Objectهای بسیار بزرگ را برای مدت نامحدود نگه دارد.

بنابراین طراحی Cache باید علاوه بر سرعت، Memory Cost را نیز در نظر بگیرد.

راهکار ساده: تعیین حداکثر اندازه Cache

const cache = new Map();

const MAX_CACHE_SIZE = 100;

function addToCache(id, user) {
if (cache.size >= MAX_CACHE_SIZE) {
const oldestKey = cache.keys().next().value;

    cache.delete(oldestKey);
}

cache.set(id, user);
}

در این مثال، Cache حداکثر 100 آیتم نگه می‌دارد. وقتی ظرفیت پر شود، یک آیتم قدیمی حذف می‌شود تا Referenceهای غیرضروری در حافظه باقی نمانند.

نکته مهم:
Cache باید یک چرخه عمر (lifecycle) مشخص داشته باشد؛ یعنی مشخص کنیم چه زمانی داده ذخیره شود، چه زمانی حذف شود و حداکثر چه مقدار داده می‌تواند در حافظه باقی بماند.
Memory Leak و Global State

Referenceهای Global نیز باید با دقت مدیریت شوند.

مثلاً:
const globalState = {
users: [],
};

function addUser(user) {
globalState.users.push(user);
}

addUser({ name: 'Ali' });
addUser({ name: 'Sara' });
addUser({ name: 'Reza' });

در این مثال، globalState در محدوده سراسری قرار دارد و Reference مربوط به تمام Objectهای users را نگه می‌دارد. اگر داده‌ها مرتباً اضافه شوند و دیگر مورد نیاز نباشند، آرایه می‌تواند بدون کنترل رشد کند.

راهکار بهتر:

اگر داده فقط برای یک عملیات موقت مورد نیاز است، آن را به‌صورت Local نگه دارید:

function processUsers() {
const users = [
{ name: 'Ali' },
{ name: 'Sara' },
{ name: 'Reza' },
];

// پردازش users
}

processUsers();

در این حالت، users یک Local Variable است. پس از پایان اجرای processUsers، اگر Reference دیگری به Objectها وجود نداشته باشد، آن‌ها دیگر از طریق این آرایه قابل دسترسی نیستند و می‌توانند توسط Garbage Collector جمع‌آوری شوند.

اصل کلی:

داده‌ای را که فقط در یک محدوده مشخص مورد نیاز است، به Global State منتقل نکنید؛ زیرا Global Reference می‌تواند طول عمر داده را بیش از نیاز واقعی آن افزایش دهد.
Reference Lifetime مهم‌تر از Reference Count

ممکن است در نگاه اول تصور کنیم اگر تعداد Referenceهای یک Object به صفر برسد، آن Object فوراً حذف می‌شود.

این مدل در سیستم‌های ساده مفید به نظر می‌رسد، اما Garbage Collection مدرن JavaScript بر اساس چنین مدل ساده‌ای تعریف نمی‌شود.

مثلاً ممکن است Referenceها به‌صورت چرخه‌ای به یکدیگر متصل باشند.

const a = {};
const b = {};

a.other = b;
b.other = a;

اگر Referenceهای بیرونی به این دو Object از بین بروند، این دو Object ممکن است به یکدیگر Reference داشته باشند.

مدل ساده Reference Counting نمی‌تواند به‌تنهایی این وضعیت را به‌درستی مدیریت کند.

Garbage Collectorهای مدرن از مدل‌های Reachability و الگوریتم‌های پیشرفته‌تر استفاده می‌کنند تا Objectهایی را که از Rootهای قابل دسترسی جدا شده‌اند، شناسایی کنند.

بنابراین مفهوم مهم‌تر:

Reachability است، نه صرفاً تعداد Referenceها.

Circular References

یک Circular Reference زمانی ایجاد می‌شود که Objectها به‌صورت چرخه‌ای به یکدیگر Reference داشته باشند.

مثلاً:

const user = {};
const profile = {};

user.profile = profile;
profile.user = user;

رابطه:

user
↓
profile
↓
user

ایجاد شده است.

اما Circular Reference به‌تنهایی Memory Leak نیست.

اگر هیچ Reference قابل دسترسی از خارج این چرخه وجود نداشته باشد، Garbage Collector می‌تواند تشخیص دهد که این مجموعه Objectها از Reachable Roots جدا شده است.

بنابراین:

Circular Reference ذاتاً Memory Leak نیست.

Memory Leak زمانی مطرح می‌شود که یک Reference از بخش Reachable برنامه، Objectهای دیگر را برای مدت نامناسب زنده نگه دارد.

Garbage Collection یک Abstraction است

Developer معمولاً نباید برنامه را بر اساس فرض‌هایی مانند:

Object is deleted exactly here

طراحی کند.

JavaScript Language تضمین نمی‌کند که Garbage Collection دقیقاً چه زمانی اجرا شود.

این موضوع به Engine و شرایط Runtime مربوط است.

آنچه برای Developer اهمیت دارد:

Referenceهای غیرضروری ایجاد نکند.
Objectهای غیرضروری را در Scopeهای طولانی‌مدت نگه ندارد.
Cacheها را کنترل کند.
Lifecycle مربوط به Resourceها را مدیریت کند.
در Applicationهای بزرگ Retained References را بررسی کند.

بنابراین:

Developer باید Object Lifetime را طراحی کند، نه زمان اجرای Garbage Collector را.

یک مدل ذهنی کامل

اکنون می‌توانیم کل فرآیند را کنار هم قرار دهیم.

فرض کنید:

let user = {
name: 'Omid'
};

ابتدا Object ایجاد می‌شود:

Memory Allocation
↓
User Object

Variable به Object دسترسی دارد:

user
↓
Object

پس Object Reachable است:

Reachable

اگر:

user = null;

و Reference دیگری وجود نداشته باشد:

No Reachable Reference
↓
Unreachable Object

Object اکنون برای Garbage Collection واجد شرایط است:

Eligible for Collection

و Engine می‌تواند Memory مربوط به آن را بازیابی کند:

Memory Reclaimed

اما اگر Reference دیگری وجود داشته باشد:

user ─────┐
▼
Object
▲
│
cache ────┘

با حذف user:

cache ───► Object

Object همچنان Reachable است.

پس Memory آن همچنان می‌تواند حفظ شود.

Memory Management در Application واقعی

در Applicationهای کوچک ممکن است Memory Management مسئله‌ای دور از ذهن به نظر برسد.

اما در Applicationهای واقعی، Lifetime داده‌ها می‌تواند بسیار متفاوت باشد.

برای مثال:

Temporary Data
↓
Request Data
↓
Component State
↓
Cache
↓
Application State
↓
Global State

هرچه Lifetime یک Data بیشتر باشد، احتمال اینکه Referenceهای آن مدت طولانی‌تری باقی بمانند بیشتر است.

بنابراین در طراحی Application باید فقط از خودمان نپرسیم:

این Object را کجا ایجاد کنم؟

بلکه باید بپرسیم:

این Object چه زمانی دیگر نباید وجود داشته باشد؟

این سؤال، یکی از مهم‌ترین پرسش‌های مهندسی در مدیریت Memory است.

Best Practices
Referenceهای غیرضروری را نگه ندارید

اگر داده دیگر موردنیاز نیست، آن را از Collection یا ساختاری که آن را نگه داشته است حذف کنید.

Cache را بدون محدودیت رشد ندهید

Cache باید Policy مشخصی برای Lifetime و حذف داده داشته باشد.

Global State را کنترل کنید

هرچه یک Reference طولانی‌تر در دسترس باشد، Objectهای بیشتری ممکن است از طریق آن Reachable باقی بمانند.

Lifecycle را در نظر بگیرید

اگر یک Object یا Resource متعلق به یک Lifecycle مشخص است، باید پایان همان Lifecycle را نیز در طراحی در نظر بگیرید.

Closure را با Memory Leak اشتباه نگیرید

Closure می‌تواند Referenceهای موردنیاز خود را حفظ کند.

این رفتار طبیعی است.

Memory Leak زمانی مطرح می‌شود که Referenceها بیش از زمان موردنیاز باقی بمانند.

به Garbage Collector تکیه کنید، اما آن را جایگزین طراحی ندانید

Garbage Collector Objectهای غیرقابل‌دسترسی را مدیریت می‌کند.

اما نمی‌تواند Objectی را که هنوز Reachable است، فقط به دلیل اینکه Application دیگر به آن نیاز ندارد حذف کند.

اشتباهات رایج
اشتباه اول: Object بلافاصله بعد از null شدن حذف می‌شود

خیر.

null شدن یک Reference فقط آن Reference را از Object جدا می‌کند.

اگر Reference دیگری وجود داشته باشد، Object همچنان Reachable است.

اشتباه دوم: Garbage Collector هر Object بلااستفاده‌ای را حذف می‌کند

Garbage Collector بر اساس Reachability عمل می‌کند، نه بر اساس تشخیص Intent برنامه‌نویس.

اگر Object هنوز Reachable باشد، ممکن است جمع‌آوری نشود.

اشتباه سوم: JavaScript نمی‌تواند Memory Leak داشته باشد

Garbage Collection مانع تمام Memory Leakها نمی‌شود.

Referenceهای غیرضروری می‌توانند Objectها را Reachable نگه دارند.

اشتباه چهارم: Circular Reference همیشه Memory Leak است

خیر.

Circular Reference زمانی مشکل‌ساز است که از طریق Referenceهای Reachable به Objectها دسترسی وجود داشته باشد.

یک چرخه کاملاً جدا از Reachable Roots می‌تواند توسط Garbage Collector جمع‌آوری شود.

اشتباه پنجم: Heap و Stack را بخشی از Semantic زبان بدانیم

Engineهای JavaScript در سطح Implementation می‌توانند Memory را به روش‌های مختلف مدیریت کنند.

برای Developer، مفهوم مهم‌تر Reachability و Object Lifetime است.

اشتباه ششم: null کردن همه Referenceها همیشه لازم است

قرار دادن null روی Referenceها به‌صورت خودکار یک Best Practice عمومی نیست.

اگر Variable قرار است از Scope خارج شود، خود Scope می‌تواند باعث پایان Lifetime آن Binding شود.

باید Referenceهای غیرضروری را در Context واقعی Application مدیریت کرد، نه اینکه به‌صورت مکانیکی همه چیز را null کنیم.

Summary

JavaScript برای اجرای برنامه به Memory نیاز دارد و Runtime مسئول مدیریت این Memory است.

Objectها دارای Identity مستقل هستند و چند Variable می‌توانند به یک Object واحد دسترسی داشته باشند.

این رابطه باعث می‌شود مفهوم Reference برای Memory Management اهمیت زیادی پیدا کند.

اما وجود یک Object در Memory صرفاً به ایجاد آن وابسته نیست.

مهم‌تر این است که آیا Object هنوز Reachable است یا خیر.

اگر Object از مسیرهای قابل دسترسی دیگر قابل دستیابی نباشد، می‌تواند Eligible for Garbage Collection شود.

Garbage Collector مسئول شناسایی و بازیابی Memory مربوط به Objectهای غیرقابل‌دسترسی است.

با این حال، Garbage Collection به معنای غیرممکن بودن Memory Leak نیست.

اگر یک Reference غیرضروری همچنان Object را Reachable نگه دارد، Garbage Collector نمی‌تواند آن Object را حذف کند.

به همین دلیل، در Applicationهای واقعی باید به Retained References، Cacheها، Global State، Closureها و Lifecycle داده‌ها توجه کرد.

Key Takeaways
JavaScript دارای Automatic Garbage Collection است.
Runtime Memory موردنیاز Valueها و Objectها را مدیریت می‌کند.
چند Variable می‌توانند به یک Object واحد دسترسی داشته باشند.
null کردن یک Reference لزوماً Object را حذف نمی‌کند.
معیار مهم برای Garbage Collection، Reachability است.
Object غیرقابل‌دسترسی می‌تواند برای Garbage Collection واجد شرایط شود.
زمان دقیق Garbage Collection تحت کنترل Developer نیست.
Circular Reference به‌تنهایی Memory Leak نیست.
Memory Leak می‌تواند در نتیجه Retained References ایجاد شود.
Cacheهای بدون محدودیت یکی از منابع رایج افزایش مصرف Memory هستند.
Closure ذاتاً Memory Leak نیست.
Developer باید Object Lifetime و Reference Lifetime را در طراحی Application در نظر بگیرد.
هدف اصلی Memory Management در سطح Application، جلوگیری از نگهداری غیرضروری Objectهای Reachable است.
Technical Interview
Junior-Level
1. Garbage Collection چیست؟

Garbage Collection فرآیندی در JavaScript Engine است که Memory مربوط به Objectهایی را که دیگر Reachable نیستند، می‌تواند بازیابی کند.

2. Reachability یعنی چه؟

Reachability یعنی اینکه آیا هنوز از مسیرهای قابل دسترسی Runtime می‌توان به یک Object رسید یا خیر.

3. آیا null کردن یک Variable باعث حذف Object می‌شود؟

خیر. فقط Reference موجود در آن Variable از Object جدا می‌شود. اگر Reference دیگری وجود داشته باشد، Object همچنان Reachable است.

4. آیا JavaScript به Developer اجازه می‌دهد Garbage Collection را مستقیماً کنترل کند؟

به‌صورت عمومی خیر. زمان و نحوه اجرای Garbage Collection تحت کنترل JavaScript Engine است.

Mid-Level
5. چرا JavaScript با وجود Garbage Collection می‌تواند Memory Leak داشته باشد؟

زیرا Garbage Collector Objectهای Reachable را حذف نمی‌کند. اگر Reference غیرضروری Object را Reachable نگه دارد، Memory آن نیز می‌تواند حفظ شود.

6. تفاوت بین Unreachable Object و Unused Object چیست؟

Unused به این معناست که Application دیگر واقعاً به Object نیاز ندارد؛ اما ممکن است Object همچنان Reference قابل دسترسی داشته باشد.

Unreachable یعنی دیگر هیچ مسیر قابل دسترسی به Object وجود ندارد.

Garbage Collection بر اساس Reachability عمل می‌کند، نه Intent برنامه.

7. آیا Circular Reference باعث Memory Leak می‌شود؟

نه لزوماً. اگر چرخه از Reachable Roots جدا شده باشد، Garbage Collector می‌تواند آن Objectها را جمع‌آوری کند.

8. چرا Cache می‌تواند باعث Memory Leak شود؟

اگر Cache Reference به Objectهایی را برای مدت نامحدود نگه دارد و Objectهای قدیمی حذف نشوند، آن Objectها همچنان Reachable باقی می‌مانند و Memory آن‌ها آزاد نمی‌شود.

Senior-Level
9. چرا Reference Counting به‌تنهایی برای Garbage Collection کافی نیست؟

زیرا Reference Counting نمی‌تواند چرخه‌های Reference را به‌تنهایی به‌درستی مدیریت کند. چند Object ممکن است فقط به یکدیگر Reference داشته باشند، در حالی که دیگر از خارج Reachable نباشند.

به همین دلیل Garbage Collectorهای مدرن از مدل‌های مبتنی بر Reachability و الگوریتم‌های پیشرفته‌تر استفاده می‌کنند.

10. آیا JavaScript تضمین می‌کند یک Object دقیقاً چه زمانی از Memory حذف شود؟

خیر. JavaScript مشخص نمی‌کند Garbage Collection دقیقاً چه زمانی اجرا شود. Object فقط زمانی که Unreachable شود، واجد شرایط جمع‌آوری خواهد بود و زمان واقعی Reclamation به Engine وابسته است.

11. چگونه یک Developer باید Memory Management را در Application طراحی کند؟

Developer نباید زمان اجرای Garbage Collector را مدیریت کند؛ بلکه باید Lifetime داده‌ها و Referenceها را طراحی کند.

این شامل کنترل Cacheها، جلوگیری از Referenceهای طولانی‌مدت غیرضروری، مدیریت Lifecycle و توجه به Global State و سایر ساختارهای نگهدارنده Reference است.

12. آیا Closure می‌تواند باعث Memory Leak شود؟

Closure خودش Memory Leak نیست. Closure به‌صورت طبیعی Environment موردنیاز خود را حفظ می‌کند.

اما اگر یک Closure برای مدت غیرضروری Reachable باقی بماند و Objectهای بزرگی را از طریق Environment خود نگه دارد، می‌تواند در ایجاد Retained References و در نتیجه Memory Leak نقش داشته باشد.

Golden Answers
اگر در مصاحبه پرسیدند: JavaScript چگونه Memory را مدیریت می‌کند؟

JavaScript از Automatic Memory Management و Garbage Collection استفاده می‌کند. Objectها تا زمانی که Reachable باشند در دسترس باقی می‌مانند. وقتی Object دیگر از مسیرهای قابل دسترسی قابل دستیابی نباشد، می‌تواند توسط Garbage Collector جمع‌آوری شود و Memory آن بازیابی شود.

اگر پرسیدند: Memory Leak در JavaScript چگونه ایجاد می‌شود؟

Memory Leak زمانی رخ می‌دهد که Application دیگر واقعاً به یک Object نیاز ندارد، اما یک Reference غیرضروری همچنان آن Object را Reachable نگه می‌دارد. در نتیجه Garbage Collector نمی‌تواند آن را جمع‌آوری کند. Cacheهای بدون محدودیت، Global State نامناسب و برخی Referenceهای طولانی‌مدت نمونه‌های رایج هستند.

اگر پرسیدند: آیا Circular Reference مشکل Memory ایجاد می‌کند؟

Circular Reference به‌تنهایی Memory Leak نیست. اگر چرخه از Reachable Roots جدا شده باشد، Garbage Collector می‌تواند آن را شناسایی و جمع‌آوری کند. مشکل زمانی ایجاد می‌شود که چرخه یا Objectهای آن همچنان از طریق Referenceهای Reachable قابل دسترسی باشند.

اگر پرسیدند: آیا null کردن Reference باعث Free شدن Memory می‌شود؟

نه به‌صورت مستقیم. null کردن Reference فقط آن مسیر دسترسی به Object را حذف می‌کند. اگر Reference دیگری وجود نداشته باشد، Object Unreachable می‌شود و می‌تواند توسط Garbage Collector جمع‌آوری شود. زمان واقعی آزادسازی Memory توسط Engine تعیین می‌شود.

Conclusion

Memory Management در JavaScript بیشتر از آنکه درباره محل فیزیکی قرار گرفتن داده‌ها باشد، درباره Lifetime و Reachability است.

برنامه Object ایجاد می‌کند، Referenceهایی به آن‌ها شکل می‌گیرد و تا زمانی که Objectها Reachable هستند، Runtime باید آن‌ها را حفظ کند.

وقتی آخرین مسیر قابل دسترسی به یک Object از بین برود، Object می‌تواند Unreachable شود و Garbage Collector در زمان مناسب Memory آن را بازیابی کند.

اما همین مدل یک نکته مهم مهندسی را آشکار می‌کند:

Garbage Collector فقط Objectهایی را می‌تواند جمع‌آوری کند که دیگر Reachable نیستند.

بنابراین وجود Garbage Collection به این معنا نیست که Developer دیگر مسئولیتی در قبال Memory ندارد.

مسئولیت Developer، طراحی صحیح Lifetime داده‌ها و جلوگیری از Referenceهای غیرضروری است.

در یک Application حرفه‌ای، سؤال مهم فقط این نیست که:

«این Object را کجا ایجاد کنیم؟»

بلکه باید بپرسیم:

«این Object تا چه زمانی باید Reachable باقی بماند؟»

همین تغییر در مدل ذهنی، تفاوت میان استفاده ساده از Objectها و درک مهندسی Memory در JavaScript است.