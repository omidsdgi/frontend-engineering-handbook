Chapter 67 — Advanced Async Patterns
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

تفاوت اجرای Sequential و Parallel عملیات Async را توضیح دهید.
انتخاب بین اجرای ترتیبی و هم‌زمان را بر اساس وابستگی عملیات تحلیل کنید.
Loading State را در یک عملیات Async به‌درستی مدیریت کنید.
مفهوم Race Condition را در Applicationهای واقعی تشخیص دهید.
توضیح دهید چرا بعضی عملیات Async باید قابل Cancellation باشند.
نقش AbortController را در Cancellation تحلیل کنید.
اهمیت Resource Cleanup را در عملیات Async توضیح دهید.
الگوهای Async را در سناریوهای واقعی Application به‌صورت قابل کنترل طراحی کنید.
Core Question

چگونه Async Operations را در Applicationهای واقعی قابل کنترل و قابل اعتماد کنیم؟

تا اینجا با Promise، Promise Combinators، async/await، Runtime و Error Handling آشنا شده‌ایم.

اما دانستن این مفاهیم به‌تنهایی برای ساخت یک Application قابل اعتماد کافی نیست.

در یک Application واقعی، معمولاً فقط یک عملیات Async نداریم.

ممکن است چند Request داشته باشیم.

ممکن است بعضی Requestها به یکدیگر وابسته باشند و بعضی مستقل باشند.

ممکن است کاربر قبل از پایان Request، عملیات دیگری را شروع کند.

ممکن است نتیجه یک عملیات دیگر دیگر برای UI قابل استفاده نباشد.

در چنین شرایطی، مسئله فقط این نیست که:

«چگونه یک Promise را اجرا کنیم؟»

مسئله مهم‌تر این است:

چگونه جریان چند عملیات Async را کنترل کنیم؟

جریان این فصل:

Async Operations
↓
Sequential
↓
Parallel
↓
Loading State
↓
Race Condition
↓
Cancellation
↓
AbortController
↓
Resource Cleanup
↓
Real Applications
مقدمه

فرض کنید کاربر در یک Application روی یک Recipe کلیک می‌کند.

Application باید اطلاعات Recipe را از Server دریافت کند.

در ساده‌ترین حالت:

const recipe = await getRecipe(id);

کار تمام است.

اما Applicationهای واقعی معمولاً به این سادگی نیستند.

ممکن است ابتدا اطلاعات کاربر دریافت شود و سپس بر اساس آن اطلاعات، Request دیگری ارسال شود.

یا ممکن است هم‌زمان چند Resource مستقل مورد نیاز باشند.

از طرف دیگر، کاربر ممکن است قبل از تمام شدن Request، Recipe دیگری را انتخاب کند.

در این حالت Request اول هنوز در حال اجراست، اما UI دیگر به نتیجه آن نیاز ندارد.

بنابراین Async Programming فقط درباره منتظر ماندن برای نتیجه نیست.

باید بتوانیم:

ترتیب عملیات را کنترل کنیم.
عملیات مستقل را هم‌زمان اجرا کنیم.
وضعیت عملیات را به UI منتقل کنیم.
نتایج قدیمی را از نتایج جدید تشخیص دهیم.
عملیات غیرضروری را متوقف کنیم.
منابع مرتبط با عملیات را پاک‌سازی کنیم.

این مسائل، Async Code را از یک مجموعه Promise ساده به یک جریان قابل کنترل تبدیل می‌کنند.

Sequential Async Operations

اولین مسئله زمانی ایجاد می‌شود که چند عملیات Async داشته باشیم.

فرض کنید برای نمایش یک Order ابتدا باید اطلاعات Customer را دریافت کنیم و سپس بر اساس customerId اطلاعات Order را بگیریم.

در اینجا عملیات دوم به نتیجه عملیات اول وابسته است.

const customer = await getCustomer(customerId);
const order = await getOrder(customer.id);

ترتیب اجرای این دو عملیات مهم است.

ابتدا:

getCustomer

باید نتیجه دهد.

سپس:

getOrder

می‌تواند اجرا شود.

این نوع اجرا را Sequential Execution می‌نامیم.

یعنی:

Operation A
↓
wait
↓
Operation B

در این مدل، عملیات بعدی تا زمانی که نتیجه عملیات قبلی مشخص نشود، شروع نمی‌شود.

چرا Sequential Execution همیشه بد نیست؟

گاهی ممکن است تصور کنیم اجرای هم‌زمان همیشه بهتر است.

اما این درست نیست.

اگر عملیات دوم به نتیجه عملیات اول نیاز داشته باشد، اجرای هم‌زمان امکان‌پذیر نیست.

مثلاً:

const user = await getUser();

const orders = await getOrders(user.id);

برای اجرای:

getOrders(user.id)

به user.id نیاز داریم.

پس رابطه این دو عملیات چنین است:

getUser
↓
user.id
↓
getOrders

بنابراین Sequential Execution در اینجا یک انتخاب منطقی است.

مسئله اصلی سرعت نیست.

مسئله اصلی Dependency است.

وقتی عملیات مستقل هستند

اکنون سناریوی دیگری را در نظر بگیریم.

Application برای نمایش Dashboard به دو Resource مستقل نیاز دارد:

User Statistics
Products

فرض کنید:

const statistics = await getStatistics();
const products = await getProducts();

در این کد، عملیات دوم بعد از پایان عملیات اول شروع می‌شود.

اما اگر:

getStatistics

به:

getProducts

وابسته نباشد، این انتظار غیرضروری است.

در چنین شرایطی سؤال جدیدی ایجاد می‌شود:

اگر دو عملیات به یکدیگر وابسته نیستند، چرا آن‌ها را هم‌زمان اجرا نکنیم؟

اینجاست که مفهوم Parallel Async Operations اهمیت پیدا می‌کند.

Parallel Async Operations

برای عملیات مستقل می‌توانیم Promiseها را قبل از await ایجاد کنیم:

const statisticsPromise = getStatistics();
const productsPromise = getProducts();

const statistics = await statisticsPromise;
const products = await productsPromise;

در این حالت هر دو عملیات شروع شده‌اند.

مدل ذهنی:

          ┌── getStatistics()
Start ────┤
└── getProducts()

           ↓

       wait for results

یا می‌توان از Promise.all استفاده کرد:

const [statistics, products] = await Promise.all([
getStatistics(),
getProducts()
]);

این الگو زمانی مناسب است که:

عملیات مستقل باشند.
برای ادامه کار به نتیجه همه آن‌ها نیاز داشته باشیم.

در این حالت:

Operation A ───┐
├──→ Continue
Operation B ───┘

Application منتظر نتیجه هر دو می‌ماند.

Sequential یا Parallel؟

اکنون می‌توانیم یک قاعده مهم ایجاد کنیم:

اگر عملیات به یکدیگر وابسته‌اند، اجرای Sequential لازم است.

و:

اگر عملیات مستقل هستند و نتیجه همه آن‌ها لازم است، Parallel Execution می‌تواند مناسب باشد.

بنابراین انتخاب بین Sequential و Parallel صرفاً یک انتخاب Syntax نیست.

این انتخاب به رابطه بین عملیات بستگی دارد.

Async Operation و UI

تا اینجا درباره خود عملیات صحبت کردیم.

اما در یک Application واقعی، کاربر نیز باید وضعیت عملیات را ببیند.

فرض کنید کاربر روی دکمه Search کلیک می‌کند.

Request ممکن است چند صد میلی‌ثانیه یا چند ثانیه طول بکشد.

در این مدت UI چه چیزی باید نمایش دهد؟

اگر هیچ تغییری در UI ایجاد نکنیم، کاربر ممکن است تصور کند:

دکمه کار نمی‌کند.
Application متوقف شده است.
Request ارسال نشده است.

بنابراین یک مفهوم جدید لازم است:

Loading State

Loading State

Loading State نشان می‌دهد یک عملیات Async در حال اجراست.

مدل ساده آن:

Idle
↓
Loading
↓
Success

یا در صورت خطا:

Idle
↓
Loading
↓
Error

مثلاً:

let isLoading = true;

const data = await fetchData();

isLoading = false;

در Application واقعی، UI می‌تواند بر اساس این وضعیت رفتار متفاوتی داشته باشد.

وقتی:

isLoading = true

است، می‌توان Loading Indicator نمایش داد.

پس Async Operation فقط یک Promise نیست.

برای UI، یک State Transition نیز ایجاد می‌کند.

چرا Loading State اهمیت دارد؟

بدون Loading State، Application ممکن است از نظر فنی Request را درست اجرا کند، اما از دید کاربر رفتار نامشخصی داشته باشد.

بنابراین باید میان این دو موضوع تفاوت قائل شویم:

Async Operation

و:

UI State caused by Async Operation

اولی درباره اجرای عملیات است.

دومی درباره وضعیت قابل مشاهده Application است.

این دو باید با یکدیگر هماهنگ باشند.

وقتی چند عملیات Async هم‌زمان می‌شوند

اکنون مسئله پیچیده‌تر می‌شود.

فرض کنید کاربر ابتدا Recipe شماره 1 را انتخاب می‌کند:

Request A → Recipe 1

قبل از اینکه Request تمام شود، کاربر Recipe شماره 2 را انتخاب می‌کند:

Request B → Recipe 2

اکنون دو عملیات هم‌زمان در حال اجرا هستند:

Request A ────────────────→ Recipe 1
Request B ───────→ Recipe 2

فرض کنید Request B سریع‌تر تمام شود.

نتیجه:

B finishes
↓
UI shows Recipe 2

تا اینجا همه چیز درست است.

اما اگر Request A بعداً تمام شود چه؟

A finishes
↓
UI shows Recipe 1

اکنون UI دوباره به داده قدیمی برگشته است.

این یک Race Condition است.

Race Condition

Race Condition زمانی رخ می‌دهد که نتیجه نهایی به ترتیب زمانی اجرای چند عملیات وابسته شود، در حالی که این ترتیب قابل پیش‌بینی نیست.

در مثال بالا:

Request A → Recipe 1
Request B → Recipe 2

ما نمی‌دانیم کدام Request زودتر تمام می‌شود.

اگر:

B → A

باشد، نتیجه ممکن است با:

A → B

متفاوت باشد.

بنابراین مشکل این نیست که Promiseها اشتباه اجرا شده‌اند.

هر دو Request می‌توانند کاملاً درست باشند.

مشکل این است که نتیجه قدیمی ممکن است بعد از نتیجه جدید به UI برسد.

چرا Race Condition مهم است؟

این مشکل در Applicationهای واقعی بسیار رایج است.

مثلاً در Search:

User types:
j
ja
jav
java

ممکن است چند Request ارسال شود:

Request 1 → "j"
Request 2 → "ja"
Request 3 → "jav"
Request 4 → "java"

اگر Request مربوط به "j" دیرتر از Request مربوط به "java" تمام شود، ممکن است UI نتیجه جستجوی قدیمی را نمایش دهد.

پس با یک سؤال جدید مواجه می‌شویم:

وقتی یک Async Operation دیگر مورد نیاز نیست، آیا باید اجازه دهیم همچنان ادامه پیدا کند؟

پاسخ در بسیاری از سناریوها این است:

خیر.

اینجاست که Cancellation مطرح می‌شود.

Cancellation

Cancellation یعنی متوقف کردن یک عملیات Async که دیگر مورد نیاز نیست.

برای مثال:

User starts Request A
↓
User starts Request B
↓
Request A is no longer needed
↓
Cancel Request A

هدف این نیست که هر عملیات Async را به‌صورت خودکار متوقف کنیم.

هدف این است که وقتی Application دیگر به یک عملیات نیاز ندارد، بتواند آن را کنترل کند.

Cancellation می‌تواند در سناریوهایی مانند این‌ها مفید باشد:

تغییر Search Query
تغییر صفحه
تغییر Resource
خروج کاربر از یک View
شروع Request جدیدی که Request قبلی را بی‌اعتبار می‌کند
AbortController

در Web Platform یکی از ابزارهای استاندارد برای Cancellation، AbortController است.

یک Controller ایجاد می‌کنیم:

const controller = new AbortController();

و Signal آن را در اختیار عملیاتی قرار می‌دهیم که باید قابل لغو باشد:

fetch(url, {
signal: controller.signal
});

اکنون Controller می‌تواند عملیات مرتبط با این Signal را Abort کند:

controller.abort();

مدل ذهنی:

AbortController
│
└── signal
│
↓
fetch()
│
↓
operation

و زمانی که:

controller.abort();

اجرا شود، Signal اعلام می‌کند که عملیات باید Abort شود.

چرا Signal جدا از Controller است؟

این تفکیک یک مدل ذهنی مهم ایجاد می‌کند.

AbortController مسئول صدور فرمان Abort است.

اما signal وسیله‌ای است که عملیات می‌تواند از طریق آن متوجه این فرمان شود.

بنابراین:

Controller
↓
abort()
↓
Signal
↓
Async Operation

این طراحی باعث می‌شود کنترل Cancellation از خود عملیات جدا باشد.

Cancellation در یک Search

فرض کنید با هر Search، یک Request جدید ایجاد می‌شود.

اگر Request قبلی هنوز در حال اجرا باشد، دیگر ممکن است به نتیجه آن نیاز نداشته باشیم.

می‌توانیم Controller مربوط به آن Request را نگه داریم:

let controller;

async function search(query) {
controller?.abort();

controller = new AbortController();

const response = await fetch(
`/api/search?q=${query}`,
{
signal: controller.signal
}
);

return response.json();
}

مدل ذهنی این کد:

New Search
↓
Abort previous request
↓
Create new controller
↓
Start new request

بنابراین Request جدید، Request قبلی را از چرخه خارج می‌کند.

Cancellation به معنی Success نیست

یک نکته مهم این است که Abort کردن یک عملیات به معنی موفقیت آن عملیات نیست.

وقتی عملیات Abort می‌شود، Application باید بتواند این وضعیت را از یک خطای معمولی تشخیص دهد.

مثلاً:

try {
const response = await fetch(url, {
signal: controller.signal
});

const data = await response.json();
} catch (error) {
// handle error or cancellation
}

در اینجا باید تصمیم بگیریم:

آیا Cancellation یک Error واقعی برای نمایش به کاربر است یا یک نتیجه مورد انتظار از رفتار Application؟

در بسیاری از سناریوهای UI، Cancellation بخشی طبیعی از جریان Application است.

مثلاً کاربر Search را تغییر داده است.

در این شرایط معمولاً لازم نیست پیام:

Something went wrong

به کاربر نمایش داده شود.

چون Request قبلی به دلیل تغییر نیاز کاربر متوقف شده است.

Resource Cleanup

اکنون به مرحله بعدی می‌رسیم.

فرض کنید یک Async Operation شروع شده است.

این عملیات ممکن است با منابع دیگری نیز در ارتباط باشد.

وقتی دیگر به آن نیاز نداریم، فقط نتیجه آن مهم نیست.

باید وضعیت و منابع مرتبط با آن را نیز مدیریت کنیم.

این مسئله ما را به مفهوم Resource Cleanup می‌رساند.

Cleanup یعنی:

بعد از پایان یا لغو یک عملیات، وضعیت و منابع مرتبط با آن را به حالت مناسب برگردانیم.

چرا Cleanup مهم است؟

فرض کنید Application یک عملیات Async را شروع می‌کند و در کنار آن وضعیت‌هایی مانند:

Loading
Timer
Event Listener
AbortController
Subscription

ایجاد می‌کند.

اگر عملیات تمام شود یا دیگر مورد نیاز نباشد، نباید منابع مرتبط برای همیشه باقی بمانند.

مدل ذهنی:

Start Operation
↓
Allocate / Register Resources
↓
Operation
↓
Success / Error / Cancellation
↓
Cleanup

نکته مهم این است که Cleanup فقط مربوط به Success نیست.

سه مسیر مهم داریم:

Success
Error
Cancellation

و هر سه می‌توانند نیازمند Cleanup باشند.

رابطه Cancellation و Cleanup

Cancellation و Cleanup یک مفهوم نیستند.

Cancellation یعنی:

عملیات دیگر نباید ادامه پیدا کند.

Cleanup یعنی:

آثار و منابع مرتبط با عملیات را مدیریت و پاک‌سازی کن.

ممکن است یک عملیات به پایان طبیعی برسد و به Cleanup نیاز داشته باشد.

ممکن است یک عملیات Cancel شود و سپس Cleanup انجام شود.

بنابراین:

Cancellation
↓
Operation stops
↓
Cleanup

اما:

Operation completes
↓
Cleanup

نیز ممکن است رخ دهد.

این تفکیک برای طراحی Async Code قابل اعتماد بسیار مهم است.

Async Operation به‌عنوان یک Lifecycle

اکنون می‌توانیم تمام مفاهیم فصل را در یک مدل واحد قرار دهیم.

یک Async Operation معمولاً از یک چرخه عبور می‌کند:

Start
↓
Loading
↓
Running
↓
┌───────────────┬───────────────┐
↓               ↓               ↓
Success         Error       Cancellation
└───────────────┴───────────────┘
↓
Cleanup

اگر چند عملیات داشته باشیم، ممکن است رابطه آن‌ها نیز مهم باشد:

Independent
↓
Parallel

Dependent
↓
Sequential

و اگر چند عملیات هم‌زمان روی یک UI کار کنند:

Multiple Async Operations
↓
Race Condition
↓
Need for Cancellation

به این ترتیب مفاهیم فصل جدا از یکدیگر نیستند.

هر مفهوم پاسخی به مسئله‌ای است که مفهوم قبلی ایجاد کرده است.

یک سناریوی واقعی

فرض کنید Application ما یک Search View دارد.

کاربر عبارت زیر را وارد می‌کند:

javascript

Application یک Request ارسال می‌کند.

Search
↓
Loading
↓
Request

در همین زمان کاربر عبارت را تغییر می‌دهد:

javascript array

Request جدید ایجاد می‌شود.

اگر Request قبلی هنوز فعال باشد، دو عملیات داریم:

Request A → "javascript"
Request B → "javascript array"

در اینجا:

عملیات جدیدتر باید اولویت داشته باشد.
Request قبلی دیگر مورد نیاز نیست.
احتمال Race Condition وجود دارد.
Cancellation می‌تواند Request قبلی را متوقف کند.
وضعیت Loading باید با Request جاری هماهنگ باشد.
بعد از پایان یا لغو عملیات، Cleanup باید انجام شود.

پس یک Search ساده می‌تواند تمام مفاهیم این فصل را درگیر کند.

انتخاب درست الگو

اکنون می‌توانیم چند سؤال مهندسی را هنگام طراحی Async Code مطرح کنیم.

آیا عملیات به یکدیگر وابسته‌اند؟

اگر بله:

Sequential

می‌تواند مناسب باشد.

آیا عملیات مستقل هستند؟

اگر بله:

Parallel

می‌تواند مناسب باشد.

آیا UI باید وضعیت عملیات را نمایش دهد؟

اگر بله:

Loading State

باید بخشی از طراحی باشد.

آیا چند عملیات می‌توانند هم‌زمان روی یک نتیجه اثر بگذارند؟

اگر بله:

Race Condition

باید بررسی شود.

آیا عملیات قبلی ممکن است دیگر مورد نیاز نباشد؟

اگر بله:

Cancellation

باید در نظر گرفته شود.

آیا عملیات منابع یا State موقتی ایجاد می‌کند؟

اگر بله:

Cleanup

باید بخشی از Lifecycle باشد.

این پرسش‌ها از حفظ کردن APIها مهم‌تر هستند.

Best Practices
1. وابستگی عملیات را قبل از انتخاب الگو مشخص کنید

فقط به دلیل وجود چند Promise، همه آن‌ها را Parallel اجرا نکنید.

ابتدا بررسی کنید آیا عملیات به یکدیگر وابسته‌اند یا خیر.

2. عملیات مستقل را بی‌دلیل Sequential نکنید

اگر دو Request کاملاً مستقل هستند، اجرای ترتیبی آن‌ها می‌تواند زمان انتظار غیرضروری ایجاد کند.

3. Loading State را بخشی از طراحی بدانید

Async Operation فقط Data نیست.

UI باید بداند عملیات در چه وضعیتی قرار دارد.

4. Race Condition را در عملیات وابسته به User Input بررسی کنید

Search، Navigation و تغییر Resource از سناریوهای رایج Race Condition هستند.

5. عملیات غیرضروری را Cancel کنید

اگر نتیجه یک Request دیگر برای Application ارزش ندارد، بررسی کنید آیا امکان Cancellation وجود دارد یا خیر.

6. Cancellation را با Error معمولی یکی ندانید

لغو شدن یک Request همیشه به معنی Failure واقعی Application نیست.

معنای Cancellation باید بر اساس نیاز Application مشخص شود.

7. Cleanup را بخشی از Lifecycle بدانید

برای Async Operation فقط مسیر Success را طراحی نکنید.

مسیرهای:

Success
Error
Cancellation

را نیز در نظر بگیرید.

Common Mistakes
اجرای همه عملیات به‌صورت Sequential

این کار زمانی مشکل‌ساز می‌شود که عملیات مستقل باشند.

استفاده از Parallel بدون بررسی Dependency

Parallel بودن همیشه صحیح نیست.

اگر عملیات دوم به نتیجه عملیات اول وابسته باشد، باید رابطه Dependency حفظ شود.

نادیده گرفتن Race Condition

ممکن است هر Request به‌تنهایی درست باشد، اما ترتیب رسیدن نتایج باعث نمایش داده قدیمی شود.

نمایش Error برای هر Cancellation

Cancellation ممکن است نتیجه طبیعی تغییر نیاز کاربر باشد و لزوماً نباید به‌عنوان خطای قابل نمایش در نظر گرفته شود.

Cleanup فقط در مسیر Success

اگر Cleanup فقط بعد از Success انجام شود، مسیر Error یا Cancellation ممکن است منابع یا State نامناسب باقی بگذارد.

اشتباه گرفتن Cancellation با Cleanup

Cancellation عملیات را متوقف می‌کند.

Cleanup آثار و منابع مرتبط با عملیات را مدیریت می‌کند.

این دو نقش متفاوت دارند.

Summary

Async Programming در Application واقعی فقط اجرای یک Promise نیست.

وقتی چند عملیات Async داریم، ابتدا باید رابطه آن‌ها را مشخص کنیم.

اگر عملیات وابسته باشند، اجرای Sequential مناسب است:

A → B

اگر عملیات مستقل باشند، می‌توان آن‌ها را Parallel اجرا کرد:

A ─┐
├→ Continue
B ─┘

وقتی Async Operation با UI در ارتباط است، باید وضعیت‌هایی مانند Loading را نیز مدیریت کنیم.

وقتی چند عملیات هم‌زمان روی یک نتیجه اثر می‌گذارند، Race Condition ممکن است باعث شود نتیجه قدیمی جای نتیجه جدید را بگیرد.

اگر عملیاتی دیگر مورد نیاز نباشد، Cancellation می‌تواند آن را متوقف کند.

AbortController یکی از ابزارهای استاندارد Web Platform برای کنترل Cancellation است.

در نهایت، چه عملیات با موفقیت تمام شود، چه با خطا مواجه شود و چه Cancel شود، باید Lifecycle و Cleanup آن را در نظر بگیریم.

مدل نهایی:

Async Operations
↓
Dependency?
↙       ↘
Yes        No
↓          ↓
Sequential Parallel
↓
UI State
↓
Multiple Operations
↓
Race Condition
↓
Cancellation
↓
AbortController
↓
Resource Cleanup
Key Takeaways
Sequential Execution برای عملیات وابسته مناسب است.
Parallel Execution برای عملیات مستقل مناسب است.
Promise.all یکی از ابزارهای رایج برای انتظار هم‌زمان چند Promise است.
Loading State وضعیت Async Operation را به UI منتقل می‌کند.
Race Condition می‌تواند باعث نمایش نتیجه قدیمی شود.
Cancellation برای عملیات‌هایی مفید است که دیگر مورد نیاز نیستند.
AbortController امکان Abort کردن عملیات‌هایی مانند fetch را فراهم می‌کند.
Cancellation و Cleanup یک مفهوم نیستند.
Cleanup باید Success، Error و Cancellation را در نظر بگیرد.
طراحی Async Code باید بر اساس Lifecycle و Dependency انجام شود، نه صرفاً Syntax.
Technical Interview
Junior-Level
سؤال 1

تفاوت Sequential و Parallel Async Execution چیست؟

Golden Answer

در Sequential Execution، عملیات بعدی پس از عملیات قبلی اجرا می‌شود و معمولاً زمانی لازم است که عملیات‌ها به یکدیگر وابسته باشند. در Parallel Execution، عملیات مستقل می‌توانند هم‌زمان شروع شوند و در صورت نیاز با Promise.all منتظر نتیجه همه آن‌ها بمانیم.

سؤال 2

Loading State چه کاربردی دارد؟

Golden Answer

Loading State وضعیت اجرای یک Async Operation را به UI منتقل می‌کند تا Application بتواند هنگام انتظار، وضعیت مناسبی مانند Spinner یا پیام Loading نمایش دهد.

سؤال 3

AbortController چیست؟

Golden Answer

AbortController یک API استاندارد Web Platform برای ایجاد یک Signal و ارسال فرمان Abort به عملیات‌هایی است که از آن Signal پشتیبانی می‌کنند، مانند fetch.

Mid-Level
سؤال 4

چه زمانی نباید دو عملیات Async را Parallel اجرا کنیم؟

Golden Answer

وقتی عملیات‌ها به یکدیگر وابسته باشند. اگر عملیات دوم برای اجرا یا تولید ورودی خود به نتیجه عملیات اول نیاز داشته باشد، باید Dependency حفظ شود و عملیات‌ها معمولاً به‌صورت Sequential اجرا شوند.

سؤال 5

Race Condition در یک Search UI چگونه ایجاد می‌شود؟

Golden Answer

اگر کاربر چند Query متوالی وارد کند، ممکن است چند Request هم‌زمان اجرا شوند. چون زمان پاسخ آن‌ها قابل پیش‌بینی نیست، Request قدیمی ممکن است بعد از Request جدید تمام شود و نتیجه قدیمی را روی UI قرار دهد.

سؤال 6

تفاوت Cancellation و Cleanup چیست؟

Golden Answer

Cancellation یعنی متوقف کردن یک عملیات Async که دیگر مورد نیاز نیست. Cleanup یعنی پاک‌سازی State، منابع یا آثار موقتی مرتبط با عملیات. Cancellation و Cleanup می‌توانند در یک Lifecycle پشت سر هم قرار بگیرند، اما یک مفهوم نیستند.

Senior-Level
سؤال 7

چگونه در یک Application تشخیص می‌دهید یک Async Operation باید Sequential یا Parallel باشد؟

Golden Answer

ابتدا Dependency Graph عملیات را بررسی می‌کنم. اگر نتیجه یک عملیات ورودی عملیات دیگر باشد، Dependency وجود دارد و اجرای Sequential لازم است. اگر عملیات مستقل باشند و نتیجه همه آن‌ها مورد نیاز باشد، Parallel Execution می‌تواند مناسب‌تر باشد. بنابراین تصمیم بر اساس Dependency است، نه صرفاً Performance.

سؤال 8

چرا Race Condition یک مشکل طراحی است، نه الزاماً مشکل Promise؟

Golden Answer

Promiseها ممکن است کاملاً درست کار کنند و هر Request نیز پاسخ صحیحی برگرداند. مشکل زمانی ایجاد می‌شود که چند عملیات مستقل روی یک State مشترک اثر بگذارند و ترتیب تکمیل آن‌ها با ترتیب مورد انتظار Application متفاوت باشد. بنابراین باید Lifecycle و اعتبار نتیجه را مدیریت کنیم.

سؤال 9

چرا Cancellation در Applicationهای تعاملی اهمیت دارد؟

Golden Answer

زیرا نیاز کاربر می‌تواند قبل از پایان عملیات تغییر کند. اگر Request قبلی دیگر مورد نیاز نباشد، ادامه آن می‌تواند منابع مصرف کند و حتی نتیجه قدیمی را وارد UI کند. Cancellation امکان خارج کردن عملیات منسوخ از جریان Application را فراهم می‌کند.

سؤال 10

چرا Cleanup باید بخشی از طراحی Async Operation باشد؟

Golden Answer

زیرا Async Operation ممکن است State یا منابع موقتی ایجاد کند و پایان آن فقط با Success اتفاق نمی‌افتد. Operation ممکن است با Error یا Cancellation نیز پایان یابد. بنابراین Lifecycle باید طوری طراحی شود که منابع و State مرتبط در تمام مسیرهای پایان مدیریت شوند.

Conclusion

در فصل‌های قبلی یاد گرفتیم چگونه Promiseها را ایجاد، ترکیب و با async/await مدیریت کنیم و چگونه خطاهای Async را کنترل کنیم.

اما در Application واقعی، مسئله بزرگ‌تر است.

چند عملیات ممکن است هم‌زمان وجود داشته باشند.

برخی به یکدیگر وابسته‌اند و برخی مستقل هستند.

برخی عملیات ممکن است دیگر مورد نیاز نباشند.

برخی نتایج ممکن است بعد از تغییر State، دیگر معتبر نباشند.

بنابراین Async Programming در سطح Application به یک مدل مدیریتی نیاز دارد:

Dependency
↓
Execution Strategy
↓
UI State
↓
Concurrency
↓
Cancellation
↓
Cleanup

این مدل به ما اجازه می‌دهد Async Operations را نه فقط اجرا، بلکه طراحی و کنترل کنیم.

اکنون که JavaScript را از نظر Syntax، Objects، Runtime، Browser و Async Programming ساخته‌ایم، سؤال بعدی این است:

وقتی JavaScript مدرن قابلیت‌های متعددی را در اختیار ما قرار می‌دهد، چگونه این قابلیت‌ها را در کنار یکدیگر برای نوشتن کدی خوانا، انعطاف‌پذیر و قابل نگهداری به کار ببریم؟

این سؤال ما را به Chapter 68 — Modern JavaScript Development می‌رساند.