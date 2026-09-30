Chapter 63 — Promise Combinators
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مسئله مدیریت چند Promise مستقل را تحلیل کنید.
توضیح دهید چرا یک Promise به‌تنهایی برای هماهنگ‌سازی چند عملیات Async کافی نیست.
رفتار Promise.all() را بر اساس موفقیت یا شکست Promiseهای ورودی تحلیل کنید.
تفاوت Promise.all() و Promise.allSettled() را در مدیریت Rejection توضیح دهید.
تفاوت Promise.race() و Promise.any() را بر اساس معیار انتخاب نتیجه تشخیص دهید.
مفهوم AggregateError را در ارتباط با Promise.any() توضیح دهید.
Combinator مناسب را بر اساس نیاز Application انتخاب کنید.
تفاوت هماهنگ‌سازی Async Operations با اجرای Parallel را درک کنید.
Core Question

چگونه چند Promise را بر اساس نیاز Application به یک جریان قابل مدیریت تبدیل کنیم؟

در فصل قبل دیدیم که Promise مسئله مدیریت نتیجه آینده یک عملیات Async را حل می‌کند.

یک Promise می‌تواند:

pending
↓
fulfilled

یا:

pending
↓
rejected

شود.

همچنین با then()، catch() و finally() می‌توانیم نتیجه آن را پردازش کنیم و با Chaining، چند مرحله وابسته را به یک جریان تبدیل کنیم.

اما Applicationهای واقعی معمولاً فقط یک عملیات Async ندارند.

فرض کنید یک Dashboard برای نمایش کامل خود به سه منبع داده نیاز دارد:

User
Recipes
Notifications

هر سه عملیات مستقل هستند و می‌توانند به‌صورت جداگانه آغاز شوند.

اکنون سؤال جدیدی شکل می‌گیرد:

اگر سه Promise داشته باشیم، Application چگونه باید تصمیم بگیرد که چه زمانی نتیجه نهایی آماده است؟

ممکن است پاسخ این باشد:

وقتی هر سه موفق شدند.

اما ممکن است Application فقط بخواهد بداند:

وضعیت هر سه عملیات چه بوده است؟

یا:

هرکدام زودتر تمام شد، همان نتیجه را استفاده کن.

یا حتی:

اگر یکی از منابع موفق شد، همان کافی است.

پس مسئله دیگر فقط مدیریت Promise نیست.

مسئله، تعریف رابطه میان چند Promise است.

این همان مسئله‌ای است که Promise Combinatorها برای آن طراحی شده‌اند.

از یک Promise به چند Promise

در فصل قبل، Promise را به‌عنوان نماینده نتیجه آینده یک عملیات Async دیدیم.

برای مثال:

const userPromise = fetch('/api/user');

در اینجا فقط یک سؤال داریم:

نتیجه این عملیات چه خواهد شد؟

اما حالا فرض کنید:

const userPromise = fetch('/api/user');
const recipesPromise = fetch('/api/recipes');
const settingsPromise = fetch('/api/settings');

سه عملیات مستقل داریم.

هر Promise وضعیت خودش را دارد، اما Application باید بتواند درباره مجموعه این عملیات تصمیم بگیرد.

اگر هر Promise را کاملاً جداگانه مدیریت کنیم، رابطه میان آن‌ها در Code بیان نشده است.

در حالی که Application ممکن است یک Policy مشخص داشته باشد:

همه باید موفق شوند.

یا:

وضعیت همه را می‌خواهم.

یا:

اولین نتیجه برایم کافی است.

یا:

اولین نتیجه موفق کافی است.

بنابراین به ابزاری نیاز داریم که بتواند چند Promise را بر اساس یک Policy مشخص ترکیب کند.

این ابزارها Promise Combinator نام دارند.

Promise Combinator چیست؟

Promise Combinator یک Method از Promise است که چند Promise یا Promise-like Value را دریافت می‌کند و یک Promise جدید برمی‌گرداند.

مدل ذهنی آن:

Promise A ──┐
Promise B ──┼──→ Combinator ──→ New Promise
Promise C ──┘

اما همه Combinatorها یک کار انجام نمی‌دهند.

تفاوت آن‌ها در سؤالی است که درباره مجموعه Promiseها می‌پرسند.

برای مثال:

all
→ آیا همه موفق شدند؟

allSettled
→ وضعیت همه چه بود؟

race
→ کدام زودتر Settled شد؟

any
→ کدام زودتر Fulfilled شد؟

بنابراین به‌جای حفظ کردن چهار Method، بهتر است آن‌ها را چهار Policy متفاوت برای مدیریت چند Promise بدانیم.

وقتی همه نتایج ضروری هستند

فرض کنید یک صفحه Recipe برای نمایش کامل خود به سه داده نیاز دارد:

Recipe
Ingredients
Nutrition

ممکن است هر سه Request مستقل باشند.

اما صفحه زمانی کامل است که هر سه نتیجه آماده باشند.

در اینجا اگر یکی از Requestها شکست بخورد، نتیجه کامل قابل استفاده نیست.

بنابراین Policy ما چنین است:

همه عملیات باید موفق شوند.

این نیاز به:

Promise.all()

منجر می‌شود.

برای مثال:

const recipe = fetch('/api/recipe');
const ingredients = fetch('/api/ingredients');
const nutrition = fetch('/api/nutrition');

const result = Promise.all([
recipe,
ingredients,
nutrition
]);

Promise.all() زمانی Fulfill می‌شود که تمام Promiseهای ورودی Fulfilled شده باشند.

پس مدل ذهنی آن:

Recipe       ──┐
Ingredients   ──┼──→ Promise.all()
Nutrition     ──┘
↓
همه باید موفق شوند

اما اگر یکی از آن‌ها Reject شود:

Recipe       → fulfilled
Ingredients  → rejected
Nutrition    → fulfilled

Promise حاصل نیز Reject می‌شود.

بنابراین:

Promise.all() زمانی مناسب است که موفقیت همه عملیات برای ادامه کار ضروری باشد.

چرا ترتیب نتیجه در Promise.all مهم است؟

فرض کنید:

const first = new Promise(resolve => {
setTimeout(() => resolve('first'), 3000);
});

const second = new Promise(resolve => {
setTimeout(() => resolve('second'), 1000);

});

Promise.all([first, second])
.then(result => {
console.log(result);
});

second زودتر تمام می‌شود.

اما نتیجه:

["first", "second"]

است.

چرا؟

زیرا Promise.all() ترتیب نتایج را بر اساس ترتیب Promiseهای ورودی حفظ می‌کند، نه ترتیب تمام شدن آن‌ها.

پس دو مفهوم را باید از هم جدا کنیم:

Completion Order
≠
Result Order
یک نکته مهم درباره Promise.all()

در اینجا ممکن است یک سوءبرداشت ایجاد شود.

اگر یکی از Promiseها Reject شود، Promise.all() نیز Reject می‌شود.

اما آیا Promiseهای دیگر متوقف می‌شوند؟

خیر.

برای مثال:

const first = fetch('/api/first');
const second = fetch('/api/second');
const third = fetch('/api/third');

Promise.all([
first,
second,
third
]);

اگر second Reject شود، Promise حاصل از Promise.all() Reject خواهد شد.

اما این موضوع به‌معنای Cancel شدن خودکار first و third نیست.

بنابراین:

Rejection نتیجه ترکیبی با Cancellation عملیات Async یکی نیست.

در این فصل فقط رابطه Promiseها را مدیریت می‌کنیم؛ Cancellation موضوع مستقلی است که بعداً به آن نیاز خواهیم داشت.

اما اگر شکست یک عملیات نباید اطلاعات سایر عملیات را از بین ببرد چه؟

اکنون مسئله تغییر کرده است.

فرض کنید Application سه عملیات مستقل دارد:

Save Profile
Save Preferences
Save Notifications

ممکن است:

Profile       → موفق
Preferences   → شکست
Notifications → موفق

شود.

Application در این شرایط نمی‌خواهد فقط بگوید:

یکی شکست خورد.

بلکه می‌خواهد بداند:

وضعیت تک‌تک عملیات چه بوده است؟

اینجا Promise.all() دیگر Policy مناسبی نیست.

زیرا با Reject شدن یکی از Promiseها، Promise ترکیبی Reject می‌شود.

اما نیاز جدید ما این است:

صبر کن تا همه عملیات به وضعیت نهایی خود برسند و سپس وضعیت همه را گزارش کن.

این نیاز ما را به:

Promise.allSettled()

می‌رساند.

Promise.allSettled()

Promise.allSettled() منتظر می‌ماند تا تمام Promiseهای ورودی به یکی از دو وضعیت نهایی برسند:

fulfilled
rejected

سپس Promise حاصل Fulfill می‌شود و وضعیت هر Promise را گزارش می‌کند.

برای مثال:

const user = Promise.resolve('Omid');

const recipes = Promise.reject(
new Error('Failed to load recipes')
);

const settings = Promise.resolve({
theme: 'dark'
});

Promise.allSettled([
user,
recipes,
settings
]).then(result => {
console.log(result);
});

نتیجه مفهومی:

[
{
status: 'fulfilled',
value: 'Omid'
},
{
status: 'rejected',
reason: Error(...)
},
{
status: 'fulfilled',
value: {
theme: 'dark'
}
}
]

در اینجا Promise مربوط به recipes شکست خورده است، اما این شکست باعث Reject شدن Promise حاصل از allSettled() نمی‌شود.

زیرا هدف allSettled() این نیست که بپرسد:

آیا همه موفق شدند؟

بلکه می‌پرسد:

نتیجه نهایی همه چه بود؟

بنابراین تفاوت دو Policy روشن می‌شود:

Promise.all()
→ All must succeed

Promise.allSettled()
→ Tell me all outcomes
وقتی اولین نتیجه تعیین‌کننده است

اکنون یک نیاز کاملاً متفاوت داریم.

فرض کنید دو یا چند منبع داریم که می‌توانند یک نتیجه را برای Application فراهم کنند:

Server A
Server B
Server C

Application ممکن است بگوید:

هرکدام زودتر به نتیجه نهایی رسید، همان برای من کافی است.

در اینجا دیگر منتظر همه Promiseها نیستیم.

حتی نمی‌پرسیم که نتیجه موفق بوده یا شکست‌خورده.

فقط زمان اولین Settled شدن مهم است.

این نیاز به:

Promise.race()

منجر می‌شود.

برای مثال:

const first = new Promise(resolve => {
setTimeout(() => resolve('first'), 2000);
});

const second = new Promise(resolve => {
setTimeout(() => resolve('second'), 1000);
});

Promise.race([
first,
second
]).then(result => {
console.log(result);
});

خروجی:

second

است.

زیرا second زودتر Settled شده است.

چرا اسم آن race است؟

در یک Race، چند Promise برای رسیدن به وضعیت نهایی با یکدیگر رقابت می‌کنند.

اولین Promiseای که Settled شود، نتیجه ترکیبی را تعیین می‌کند.

نکته مهم این است که Settled شامل هر دو حالت است:

fulfilled
rejected

پس اگر:

A → rejected
B → fulfilled

و A زودتر Settled شود، Promise.race() نتیجه را Reject می‌کند.

بنابراین:

Promise.race() اولین Promise موفق را انتخاب نمی‌کند؛ اولین Promise Settled را انتخاب می‌کند.

مدل ذهنی:

A ──┐
B ──┼──→ race
C ──┘
↓
First Settled
اما اگر اولین شکست نباید تعیین‌کننده باشد چه؟

حالا یک مسئله دیگر شکل می‌گیرد.

فرض کنید چند Server می‌توانند یک Resource را فراهم کنند:

Server A
Server B
Server C

Server A ممکن است Down باشد.

Server B ممکن است کمی دیرتر پاسخ دهد.

Server C نیز ممکن است در دسترس باشد.

Application در اینجا نمی‌خواهد بگوید:

اولین Serverی که هر اتفاقی برایش افتاد، برنده است.

بلکه می‌خواهد بگوید:

اولین Serverی که موفق شد کافی است.

در این شرایط Promise.race() مناسب نیست.

زیرا یک Rejection سریع می‌تواند Race را تمام کند.

ما به Policy دیگری نیاز داریم:

Rejectionها را تا زمانی که یک Fulfillment پیدا شود نادیده بگیر.

این دقیقاً مسئله Promise.any() است.

Promise.any()

Promise.any() منتظر اولین Promiseای می‌ماند که fulfilled شود.

برای مثال:

const primary = Promise.reject(
new Error('Primary failed')
);

const backup = new Promise(resolve => {
setTimeout(() => resolve('Backup'), 1000);
});

const cache = new Promise(resolve => {
setTimeout(() => resolve('Cache'), 2000);
});

Promise.any([
primary,
backup,
cache
]).then(result => {
console.log(result);
});

نتیجه:

Backup

است.

زیرا:

Primary → rejected
Backup  → fulfilled
Cache   → fulfilled

و Backup اولین Promiseای است که Fulfilled شده است.

بنابراین:

Promise.any()
→ First Fulfilled
تفاوت Promise.race() و Promise.any()

اکنون دو Combinator داریم که در ظاهر بسیار شبیه هستند.

هر دو با «اولین نتیجه» ارتباط دارند.

اما سؤال آن‌ها متفاوت است.

Promise.race() می‌پرسد:

کدام Promise زودتر Settled شد؟

Promise.any() می‌پرسد:

کدام Promise زودتر Fulfilled شد؟

فرض کنید:

A → rejected
B → fulfilled
C → fulfilled

و A زودتر از بقیه Settled شود.

در این شرایط:

race → rejected
any  → B

زیرا:

race → First Settled
any  → First Fulfilled

این یکی از مهم‌ترین تفاوت‌های Promise Combinatorها است.

اگر Promise.any() هیچ نتیجه موفقی پیدا نکند چه؟

اکنون فرض کنید:

const first = Promise.reject(
new Error('Server A failed')
);

const second = Promise.reject(
new Error('Server B failed')
);

Promise.any([
first,
second
])
.then(result => {
console.log(result);
})
.catch(error => {
console.log(error);
});

هیچ Promiseای Fulfilled نشده است.

پس Promise.any() نمی‌تواند نتیجه موفقی تولید کند.

در این شرایط Promise حاصل Reject می‌شود و خطای آن از نوع:

AggregateError

است.

AggregateError امکان نگهداری خطاهای چند Promise را فراهم می‌کند.

بنابراین رفتار any() را می‌توان چنین خلاصه کرد:

At least one fulfills
↓
First fulfillment

اما:

All reject
↓
AggregateError
چهار Policy، یک مسئله

اکنون مسئله‌ای که در ابتدای فصل داشتیم را دوباره ببینیم.

ما چند Promise داشتیم:

A
B
C

و باید مشخص می‌کردیم Application چه رابطه‌ای میان آن‌ها می‌خواهد.

اکنون چهار پاسخ داریم.

اگر:

همه باید موفق شوند.

Promise.all([A, B, C])

اگر:

وضعیت همه را می‌خواهم.

Promise.allSettled([A, B, C])

اگر:

اولین Promise که به هر وضعیت نهایی رسید برای من تعیین‌کننده است.

Promise.race([A, B, C])

اگر:

اولین Promise موفق برای من کافی است.

Promise.any([A, B, C])

پس چهار Method را نباید به‌عنوان چهار API جداگانه حفظ کنیم.

آن‌ها چهار پاسخ به یک مسئله واحد هستند:

چگونه چند Async Operation را بر اساس نیاز Application هماهنگ کنیم؟

Promise Combinators و استقلال عملیات

در اینجا یک تفاوت مهم با Promise Chaining آشکار می‌شود.

در Chaining، خروجی یک مرحله معمولاً ورودی مرحله بعد است:

Get User
↓
Get Orders
↓
Calculate Summary

بنابراین مراحل به یکدیگر وابسته‌اند.

اما در Combinatorها معمولاً با عملیات مستقل سروکار داریم:

Get User
Get Recipes
Get Settings

هیچ‌کدام برای آغاز شدن به نتیجه دیگری نیاز ندارند.

در چنین شرایطی می‌توان آن‌ها را مستقل آغاز کرد و سپس رابطه موردنیاز را با یک Combinator بیان کرد.

بنابراین یک مدل ذهنی مفید داریم:

Dependent Async Operations
↓
Promise Chaining

و:

Independent Async Operations
↓
Promise Combinators

این بدان معنا نیست که Combinatorها فقط برای عملیات کاملاً مستقل کاربرد دارند؛ بلکه نکته اصلی این است که نوع وابستگی میان عملیات باید قبل از انتخاب الگو مشخص شود.

Concurrency؛ مفهومی که از اینجا شکل می‌گیرد

وقتی چند Async Operation را به‌صورت مستقل آغاز می‌کنیم، دیگر با یک جریان کاملاً ترتیبی مواجه نیستیم.

برای مثال:

User Request ──────────┐
│
Recipes Request ───────┼──→ Combined Result
│
Settings Request ──────┘

سپس Combinator تعیین می‌کند که چه زمانی و با چه Policyای نتیجه ترکیبی تولید شود.

در نتیجه Promise Combinatorها ما را با یک مفهوم مهم‌تر آشنا می‌کنند:

Concurrency

در اینجا منظور این نیست که JavaScript چند محاسبه CPU-bound را هم‌زمان روی یک Thread اجرا می‌کند.

بحث درباره هماهنگ‌سازی چند Async Operation است که می‌توانند در یک بازه زمانی با یکدیگر هم‌پوشانی داشته باشند.

بنابراین:

Promise
→ یک Async Result

Promise Combinator
→ رابطه میان چند Async Result

این تغییر در مدل ذهنی مهم‌تر از حفظ Syntax چهار Method است.

انتخاب Combinator بر اساس نیاز

اکنون اگر در یک Application با چند Promise مواجه شدیم، بهتر است قبل از نوشتن Code چهار سؤال بپرسیم:

آیا همه نتایج برای ادامه کار لازم هستند؟
Yes
↓
Promise.all()
آیا می‌خواهیم وضعیت تمام عملیات را بدانیم؟
Yes
↓
Promise.allSettled()
آیا اولین Promise Settled تعیین‌کننده است؟
Yes
↓
Promise.race()
آیا اولین Promise Fulfilled کافی است؟
Yes
↓
Promise.any()

بنابراین انتخاب Combinator از Syntax شروع نمی‌شود.

از نیاز Application شروع می‌شود.

یک مقایسه نهایی
Combinator	سؤال اصلی	چه زمانی Promise حاصل تعیین می‌شود؟	Rejection چه نقشی دارد؟
Promise.all()	آیا همه موفق شدند؟	وقتی همه Fulfilled شوند	یک Rejection باعث Reject شدن نتیجه می‌شود
Promise.allSettled()	وضعیت همه چه بود؟	وقتی همه Settled شوند	بخشی از نتیجه است
Promise.race()	چه کسی زودتر Settled شد؟	با اولین Settlement	می‌تواند برنده Race باشد
Promise.any()	چه کسی زودتر موفق شد؟	با اولین Fulfillment	تا زمانی که همه Reject نشده‌اند نادیده گرفته می‌شود

اگر این جدول را به چهار جمله تبدیل کنیم:

all
→ All must fulfill.

allSettled
→ Tell me every outcome.

race
→ First settled wins.

any
→ First fulfilled wins.

این چهار جمله باید بخشی از مدل ذهنی شما باشند، نه صرفاً چهار عبارت برای حفظ کردن.

Promise Combinators و Cancellation

یک نکته مهم باقی می‌ماند.

فرض کنید:

Promise.race([
slowRequest,
fastRequest
]);

و fastRequest برنده شود.

آیا slowRequest خودکار متوقف می‌شود؟

خیر.

Combinator فقط مشخص می‌کند Promise ترکیبی چگونه رفتار کند.

بنابراین:

Winner selected
≠
Other operation cancelled

همین موضوع درباره Promise.all() نیز صادق است.

اگر به Cancellation نیاز داشته باشیم، باید سازوکار Cancellation را به‌صورت جداگانه مدیریت کنیم.

این تمایز در Applicationهای واقعی اهمیت زیادی دارد، زیرا یک Request ممکن است حتی پس از اینکه دیگر برای UI موردنیاز نیست، همچنان در حال اجرا باشد.

Best Practices
ابتدا Policy را مشخص کنید، سپس Combinator را انتخاب کنید

به‌جای اینکه بگوییم:

برای چند Promise از Promise.all() استفاده می‌کنم.

ابتدا بپرسید:

موفقیت همه لازم است یا فقط یکی کافی است؟

عملیات مستقل را بی‌دلیل ترتیبی نکنید

اگر چند Async Operation وابستگی ندارند، لازم نیست همیشه یکی پس از دیگری منتظر بمانند.

می‌توان آن‌ها را مستقل آغاز و سپس با Combinator مناسب هماهنگ کرد.

Rejection را با Cancellation یکی ندانید

Reject شدن Promise ترکیبی به‌معنای توقف خودکار عملیات دیگر نیست.

race() و any() را بر اساس Settlement و Fulfillment تشخیص دهید

قاعده ساده:

race → settled
any  → fulfilled
allSettled() را زمانی انتخاب کنید که Failure نیز بخشی از اطلاعات موردنیاز است

اگر Application باید بداند کدام عملیات موفق و کدام عملیات شکست‌خورده است، allSettled() این وضعیت را به‌صورت صریح گزارش می‌کند.

اشتباهات رایج
تصور اینکه Promise.all() اولین نتیجه را برمی‌گرداند

خیر. منتظر تمام Promiseهای موردنیاز می‌ماند و نتایج را بر اساس ترتیب ورودی ارائه می‌کند.

تصور اینکه Promise.race() اولین نتیجه موفق را انتخاب می‌کند

خیر. اولین fulfilled یا rejected می‌تواند برنده باشد.

تصور اینکه Promise.any() با اولین Rejection Reject می‌شود

خیر. Rejectionهای اولیه مانع ادامه any() نیستند.

تصور اینکه Promise.allSettled() در صورت وجود Rejection Reject می‌شود

خیر. هدف آن گزارش وضعیت نهایی همه Promiseها است.

تصور اینکه Combinatorها Promiseهای دیگر را Cancel می‌کنند

خیر. ترکیب نتیجه با Cancellation دو مسئله متفاوت هستند.

Summary

در فصل قبل، Promise امکان مدیریت نتیجه آینده یک Async Operation را فراهم کرد.

اما با افزایش تعداد عملیات، مسئله جدیدی ایجاد شد:

چگونه رابطه میان چند Promise را تعریف کنیم؟

اگر Application به موفقیت همه عملیات نیاز داشته باشد، Promise.all() این Policy را بیان می‌کند.

اگر Application بخواهد وضعیت تمام عملیات را بداند، Promise.allSettled() مناسب است.

اگر اولین Promiseای که به هر وضعیت نهایی برسد تعیین‌کننده باشد، Promise.race() مورد استفاده قرار می‌گیرد.

و اگر اولین Promise موفق کافی باشد، Promise.any() این نیاز را بیان می‌کند.

بنابراین:

Multiple Promises
↓
Need a Policy
↓
┌───────────────┬────────────────────┐
│               │                    │
all        allSettled             race / any
│               │                    │
All succeed   All outcomes      First result
│
┌───────┴───────┐
│               │
race             any
│               │
First settled   First fulfilled

در نتیجه Promise Combinatorها صرفاً چند Method نیستند.

آن‌ها زبان بیان رابطه میان چند Async Operation هستند.

Key Takeaways
Promise یک Async Result را مدل می‌کند؛ Combinator رابطه میان چند Promise را مدیریت می‌کند.
Promise.all() به موفقیت همه Promiseها نیاز دارد.
Promise.allSettled() وضعیت تمام Promiseها را گزارش می‌کند.
Promise.race() اولین Promise Settled را تعیین‌کننده می‌کند.
Promise.any() اولین Promise Fulfilled را تعیین‌کننده می‌کند.
race() و any() فقط در ظاهر مشابه‌اند؛ معیار انتخاب آن‌ها متفاوت است.
در Promise.any() اگر همه Promiseها Reject شوند، نتیجه با AggregateError Reject می‌شود.
Rejection یک Promise با Cancellation آن یکی نیست.
Combinatorها ابزار هماهنگ‌سازی Async Operations هستند.
انتخاب Combinator باید از نیاز Application آغاز شود.
عملیات وابسته معمولاً با Chaining مدل می‌شوند و عملیات مستقل را می‌توان با Combinatorها هماهنگ کرد.
Technical Interview
Junior Level
Promise.all() چه کاری انجام می‌دهد؟

Promise.all() چند Promise را ترکیب می‌کند و زمانی Fulfill می‌شود که همه Promiseهای ورودی Fulfilled شده باشند. اگر یکی Reject شود، Promise حاصل Reject می‌شود.

تفاوت Promise.all() و Promise.allSettled() چیست؟

Promise.all() به موفقیت همه Promiseها نیاز دارد؛ اما Promise.allSettled() منتظر تمام Promiseها می‌ماند و وضعیت هرکدام را گزارش می‌کند.

Promise.race() چه چیزی را انتخاب می‌کند؟

اولین Promiseای که Settled شود؛ یعنی اولین Promiseای که Fulfilled یا Rejected شود.

Promise.any() چه چیزی را انتخاب می‌کند؟

اولین Promiseای که Fulfilled شود.

Mid-Level
چرا Promise.all() برای چند عملیات مستقل مفید است؟

زیرا عملیات مستقل می‌توانند بدون وابستگی ترتیبی آغاز شوند و سپس Application منتظر نتیجه همه آن‌ها بماند.

آیا Reject شدن Promise.all() باعث Cancel شدن Promiseهای دیگر می‌شود؟

خیر. Promise حاصل Reject می‌شود، اما Promiseهای دیگر به‌صورت خودکار Cancel نمی‌شوند.

چه زمانی Promise.allSettled() مناسب‌تر است؟

وقتی Application نیاز دارد نتیجه نهایی و وضعیت هر عملیات را، چه موفق و چه ناموفق، دریافت کند.

تفاوت اصلی Promise.race() و Promise.any() چیست؟

race() اولین Settled را انتخاب می‌کند؛ any() اولین Fulfilled را.

Senior Level
چرا Promise Combinatorها را نباید صرفاً APIهای مختلف Promise دانست؟

زیرا هر Combinator یک Concurrency Policy متفاوت را بیان می‌کند؛ بنابراین انتخاب آن باید از نیاز Application و رابطه میان Async Operations ناشی شود.

آیا Promise Combinatorها Parallelism ایجاد می‌کنند؟

خیر. آن‌ها چند Async Operation را هماهنگ می‌کنند. Combinator به‌خودی‌خود JavaScript را به اجرای موازی CPU-bound تبدیل نمی‌کند.

چرا Rejection و Cancellation دو مفهوم متفاوت هستند؟

Rejection بیانگر شکست یا ناموفق بودن Promise است؛ Cancellation مربوط به متوقف کردن یا بی‌اثر کردن خود عملیات Async است. Reject شدن Promise ترکیبی به‌طور خودکار عملیات دیگر را Cancel نمی‌کند.

چه زمانی Promise.any() از Promise.race() مناسب‌تر است؟

زمانی که چند مسیر می‌توانند یک نتیجه را تولید کنند و شکست بعضی مسیرها نباید مانع ادامه کار شود؛ در این شرایط اولین Fulfillment برای Application کافی است.

Golden Answers
Promise.all()

Promise.all() زمانی مناسب است که چند Async Operation مستقل داریم و موفقیت همه آن‌ها برای نتیجه نهایی ضروری است. Promise حاصل با Fulfillment همه ورودی‌ها Fulfilled و با Rejection یکی از آن‌ها Rejected می‌شود.

Promise.allSettled()

Promise.allSettled() زمانی استفاده می‌شود که می‌خواهیم وضعیت نهایی تمام Promiseها را بدانیم، بدون اینکه Rejection یکی از آن‌ها باعث Reject شدن Promise ترکیبی شود.

Promise.race()

Promise.race() اولین Promiseای را که Settled شود، تعیین‌کننده نتیجه قرار می‌دهد؛ بنابراین هم Fulfillment و هم Rejection می‌تواند برنده باشد.

Promise.any()

Promise.any() اولین Promise Fulfilled را انتخاب می‌کند و Rejectionهای اولیه را نادیده می‌گیرد. اگر تمام Promiseها Reject شوند، Promise حاصل با AggregateError Reject می‌شود.

تفاوت race() و any()

race() بر اساس اولین Settlement تصمیم می‌گیرد، در حالی که any() بر اساس اولین Fulfillment تصمیم می‌گیرد.

Promise Combinators

Promise Combinatorها ابزارهایی برای هماهنگ‌سازی چند Promise بر اساس یک Policy مشخص هستند؛ بنابراین انتخاب آن‌ها باید از نیاز Application آغاز شود.

Conclusion

در فصل 62، Promise به ما کمک کرد نتیجه آینده یک Async Operation را مدیریت کنیم.

اما اکنون با چند عملیات Async مواجه شدیم.

در اینجا دیگر یک سؤال ساده نداریم:

«نتیجه این Promise چه زمانی آماده می‌شود؟»

بلکه سؤال بزرگ‌تری داریم:

«رابطه میان این Promiseها چیست و Application چه زمانی می‌تواند نتیجه آن‌ها را قابل استفاده بداند؟»

اگر همه باید موفق شوند:

Promise.all()

اگر وضعیت همه مهم است:

Promise.allSettled()

اگر اولین نتیجه نهایی تعیین‌کننده است:

Promise.race()

و اگر اولین نتیجه موفق کافی است:

Promise.any()

بنابراین مسیر یادگیری ما اکنون چنین شده است:

Promise
↓
Promise Chaining
↓
Multiple Promises
↓
Promise Combinators
↓
Concurrency Policy

ما اکنون می‌دانیم چگونه چند Promise را بر اساس نیاز Application هماهنگ کنیم.

اما هنوز یک مسئله باقی مانده است.

تا اینجا برای کار با Promiseها از then() و catch() استفاده کرده‌ایم. با افزایش تعداد عملیات و ترکیب جریان‌های Async، این Syntax می‌تواند خوانایی Code را دشوارتر کند.

پس سؤال طبیعی بعدی این است:

آیا می‌توان همین Promise-based Flow را با Syntaxای خواناتر و شبیه جریان عادی اجرای کد نوشت؟

این سؤال ما را به async و await می‌رساند؛ مفهومی که در فصل بعد بررسی خواهیم کرد.