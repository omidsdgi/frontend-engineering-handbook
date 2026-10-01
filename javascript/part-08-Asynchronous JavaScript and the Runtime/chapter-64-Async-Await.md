Chapter 64 — Async/Await
اهداف فصل

پس از مطالعه این فصل، خواننده باید بتواند:

توضیح دهد چرا async/await بر پایه Promise ساخته شده است.
رفتار یک async Function و مقدار Return آن را تحلیل کند.
توضیح دهد await چه نقشی در مصرف نتیجه Promise دارد.
تفاوت میان Sequential و Parallel Async Flow را تشخیص دهد.
از try/catch و finally در جریان Async استفاده کند.
تشخیص دهد چه زمانی awaitهای پشت‌سرهم مناسب و چه زمانی نامناسب هستند.
رابطه async/await با Promise را بدون اشتباه گرفتن آن با Synchronous Execution توضیح دهد.
Core Question

چگونه Promise-based Code را با Syntax خواناتر مدیریت کنیم؟

در فصل 62 دیدیم که Promise راهی برای مدیریت نتیجه آینده یک عملیات Async است.

سپس در فصل 63 دیدیم که وقتی چند Promise داریم، می‌توانیم بر اساس رابطه میان آن‌ها از Combinatorهای مختلف استفاده کنیم.

اما هنوز یک مسئله باقی می‌ماند.

فرض کنید یک عملیات Asynchronous چند مرحله دارد:

getRecipe(id)
.then(recipe => getIngredients(recipe))
.then(ingredients => {
console.log(ingredients);
})
.catch(error => {
console.error(error);
});

این کد مشکلی ندارد.

اما هرچه تعداد مراحل بیشتر شود، جریان اصلی برنامه در میان .then()ها و Callbackهای مربوط به آن‌ها پخش می‌شود.

ما می‌خواهیم همان Promise-based Flow را داشته باشیم، اما بتوانیم Dependency میان مراحل را به شکلی مستقیم‌تر بیان کنیم.

پس سؤال جدید این است:

آیا می‌توان Promise را همچنان حفظ کرد، اما جریان Asynchronous را خواناتر نوشت؟

پاسخ، async/await است.

از Promise به async

برای درک async/await ابتدا باید رابطه آن را با Promise روشن کنیم.

فرض کنید Functionای داریم که نتیجه یک عملیات Asynchronous را برمی‌گرداند:

function getRecipe() {
return fetch('/api/recipe');
}

همان‌طور که در فصل‌های قبل دیدیم، fetch() یک Promise برمی‌گرداند.

بنابراین:

const result = getRecipe();

مقدار result خود Recipe نیست.

بلکه یک Promise است که نتیجه Recipe را در آینده فراهم می‌کند.

اکنون اگر بخواهیم Functionای داشته باشیم که خودش بخشی از همین Promise-based Flow باشد، می‌توانیم آن را با async تعریف کنیم:

async function getRecipe() {
return fetch('/api/recipe');
}

اما async فقط یک علامت برای نشان‌دادن Asynchronous بودن Function نیست.

یک ویژگی مهم‌تر دارد:

یک async Function همیشه یک Promise برمی‌گرداند.

مثلاً:

async function getNumber() {
return 42;
}

ممکن است در نگاه اول تصور کنیم:

const result = getNumber();

مقدار result برابر 42 است.

اما این‌طور نیست.

result یک Promise است که در نهایت با مقدار 42 Fulfill می‌شود.

پس:

return 42
↓
async Function
↓
Promise
↓
42

این اولین نکته مهم در مدل ذهنی async/await است:

async Promise را حذف نمی‌کند؛ بلکه Function را وارد Promise-based Flow می‌کند.

چرا async به‌تنهایی کافی نیست؟

اکنون یک Async Function داریم:

async function loadRecipe() {
const response = fetch('/api/recipe');

return response;
}

اما هنوز با همان مسئله قبلی روبه‌رو هستیم.

fetch() یک Promise برمی‌گرداند و ما می‌خواهیم نتیجه آن Promise را در جریان همین Function در اختیار داشته باشیم.

می‌توانیم همچنان از Promise Chain استفاده کنیم:

function loadRecipe() {
return fetch('/api/recipe')
.then(response => {
console.log(response);
});
}

اما هدف ما این بود که جریان را خواناتر کنیم.

اینجا نیاز به Concept بعدی شکل می‌گیرد:

اگر async Function را وارد Promise-based Flow می‌کند، چگونه نتیجه یک Promise را داخل همان Function دریافت کنیم؟

پاسخ await است.

await

می‌توانیم کد را به این شکل بنویسیم:

async function loadRecipe() {
const response = await fetch('/api/recipe');

console.log(response);
}

در اینجا await به JavaScript می‌گوید که ادامه اجرای این Async Function به نتیجه Promise وابسته است.

پس:

const response = await fetch('/api/recipe');

از نظر منطقی یعنی:

fetch()
↓
Promise
↓
await
↓
Response
↓
ادامه Function

در نتیجه دیگر لازم نیست نتیجه Promise را با یک Callback داخل .then() دنبال کنیم.

می‌توانیم آن را در همان جریان منطقی Function دریافت کنیم.

await چه چیزی را متوقف می‌کند؟

ظاهر کد ممکن است چنین برداشتی ایجاد کند:

const response = await fetch(url);
console.log(response);

انگار JavaScript اجرای خود را متوقف کرده و منتظر fetch() مانده است.

اما این برداشت دقیق نیست.

await کل JavaScript را متوقف نمی‌کند.

آنچه به نتیجه Promise وابسته می‌شود، ادامه اجرای همان Async Function است.

به همین دلیل بهتر است await را این‌گونه در ذهن مدل کنیم:

Async Function
↓
await
↓
Promise هنوز آماده نیست
↓
ادامه Function فعلاً اجرا نمی‌شود
↓
Promise نتیجه می‌دهد
↓
ادامه Function می‌تواند ادامه پیدا کند

جزئیات اینکه این Resume شدن در Runtime چگونه انجام می‌شود، موضوع Chapter 65 است و در این فصل وارد آن نمی‌شویم. Roadmap نیز Chapter 65 را به‌طور مشخص برای Suspension، Continuation، Microtask و Event Loop در نظر گرفته است.

در این فصل فقط باید یک نکته را حفظ کنیم:

await اجرای کل برنامه را متوقف نمی‌کند؛ بلکه ادامه همان Async Function را به نتیجه Promise وابسته می‌کند.

از یک Promise به یک جریان چندمرحله‌ای

حالا مزیت await زمانی روشن‌تر می‌شود که چند مرحله به یکدیگر وابسته باشند.

فرض کنید برای دریافت Ingredients ابتدا باید Recipe را دریافت کنیم:

getRecipe(id)
.then(recipe => getIngredients(recipe))
.then(ingredients => {
console.log(ingredients);
});

در اینجا رابطه مشخصی وجود دارد:

getRecipe()
↓
recipe
↓
getIngredients(recipe)
↓
ingredients

عملیات دوم بدون نتیجه عملیات اول قابل انجام نیست.

با async/await می‌توانیم همین Dependency را مستقیماً در ترتیب کد نشان دهیم:

async function loadRecipe(id) {
const recipe = await getRecipe(id);
const ingredients = await getIngredients(recipe);

console.log(ingredients);
}

اکنون روایت کد بسیار مستقیم‌تر شده است:

getRecipe
↓
recipe
↓
getIngredients
↓
ingredients

این اولین نتیجه مهم async/await است:

Dependency میان عملیات‌های Asynchronous را می‌توان با Syntaxای خطی و خوانا بیان کرد.

Sequential Async Flow

در مثال قبلی، عملیات دوم به نتیجه عملیات اول وابسته بود.

به همین دلیل ترتیب اجرای Logic اهمیت دارد:

const user = await getUser();
const orders = await getOrders(user.id);

ابتدا user لازم است تا user.id در اختیار ما قرار بگیرد.

سپس می‌توانیم getOrders() را اجرا کنیم.

بنابراین:

getUser()
↓
user
↓
user.id
↓
getOrders()
↓
orders

این یک Sequential Async Flow است.

Sequential بودن در اینجا به معنی Synchronous شدن JavaScript نیست.

هر دو عملیات همچنان Asynchronous هستند.

فقط عملیات دوم به نتیجه عملیات اول وابسته است.

پس یک قاعده مهم شکل می‌گیرد:

وقتی Dependency میان عملیات‌ها وجود دارد، ترتیب awaitها بخشی از Logic برنامه است.

وقتی مسیر موفقیت تنها مسیر ممکن نیست

تا اینجا همه چیز بر اساس موفقیت عملیات پیش رفت.

اما یک Promise ممکن است Reject شود.

مثلاً:

async function loadRecipe(id) {
const recipe = await getRecipe(id);
const ingredients = await getIngredients(recipe);

return ingredients;
}

اگر getRecipe() یا getIngredients() با Failure مواجه شود، مسیر عادی Function دیگر ادامه پیدا نمی‌کند.

بنابراین اکنون یک سؤال طبیعی ایجاد می‌شود:

اگر یکی از Promiseهایی که با await منتظر آن هستیم Reject شود، چگونه مسیر خطا را مدیریت کنیم؟

در Promise Chain این کار را با .catch() می‌شناختیم:

getRecipe(id)
.then(...)
.catch(error => {
console.error(error);
});

در async/await می‌توانیم همان مسیر خطا را در ساختاری آشنا‌تر قرار دهیم:

async function loadRecipe(id) {
try {
const recipe = await getRecipe(id);
const ingredients = await getIngredients(recipe);

    return ingredients;
} catch (error) {
console.error(error);
}
}

اکنون جریان به دو مسیر تقسیم می‌شود:

             ┌── Success → ادامه Function
await Promise
└── Failure → catch

این همان نقطه‌ای است که try/catch به‌صورت طبیعی وارد روایت می‌شود.

try/catch Concept جدیدی برای JavaScript نیست؛ اما async/await اجازه می‌دهد Failure مربوط به Promiseهای awaitشده را در همان ساختار کنترل خطای Function قرار دهیم.

finally

اکنون مسیر Success و Failure را داریم.

اما ممکن است Logicای وجود داشته باشد که نتیجه عملیات برای آن اهمیتی نداشته باشد.

مثلاً می‌خواهیم بعد از پایان عملیات، چه موفق شده باشد و چه شکست خورده باشد، یک وضعیت موقت را تغییر دهیم.

در این شرایط finally مناسب است:

async function loadRecipe(id) {
try {
const recipe = await getRecipe(id);

    return recipe;
} catch (error) {
console.error(error);
} finally {
console.log('Request finished');
}
}

جریان اکنون این شکل را دارد:

             ┌── Success ──┐
await        │             ↓
Promise ─────┤          finally
│             ↑
└── Failure → catch

بنابراین finally برای Logicای مناسب است که نباید به موفقیت یا شکست عملیات وابسته باشد.

تا اینجا جریان ما کامل به نظر می‌رسد:

Promise
↓
async
↓
await
↓
Sequential Flow
↓
try / catch
↓
finally

اما یک مسئله مهم دیگر باقی مانده است.

آیا همیشه باید awaitها را پشت سر هم بنویسیم؟

فرض کنید Application باید دو منبع مستقل را دریافت کند:

async function loadData() {
const user = await getUser();
const categories = await getCategories();

return { user, categories };
}

در این مثال، آیا getCategories() واقعاً به user نیاز دارد؟

اگر پاسخ منفی باشد، دو عملیات مستقل هستند.

اما کدی که نوشته‌ایم آن‌ها را به ترتیب قرار داده است:

getUser()
↓
user
↓
getCategories()
↓
categories

یعنی شروع عملیات دوم را به پایان عملیات اول وابسته کرده‌ایم، در حالی که چنین Dependencyای در مسئله وجود ندارد.

اینجا یک سؤال جدید شکل می‌گیرد:

اگر دو عملیات مستقل باشند، چرا باید اجرای یکی منتظر پایان دیگری بماند؟

Parallel Async

اگر عملیات‌ها مستقل باشند، می‌توانیم Promiseهای آن‌ها را بدون انتظار برای نتیجه هرکدام ایجاد کنیم:

async function loadData() {
const userPromise = getUser();
const categoriesPromise = getCategories();

const [user, categories] = await Promise.all([
userPromise,
categoriesPromise
]);

return { user, categories };
}

اکنون جریان متفاوت است:

              ┌── getUser()
Start ────────┤
└── getCategories()
↓
Promise.all
↓
Results

در اینجا دو عملیات مستقل از یکدیگر شروع شده‌اند و Promise.all نتیجه مجموعه را در اختیار ما قرار می‌دهد.

async/await در اینجا جایگزین Promise Combinator نشده است.

بلکه:

Multiple Promises
↓
Promise.all
↓
Combined Promise
↓
await
↓
Results

است.

این دقیقاً همان چیزی است که از فصل قبل به فصل حاضر منتقل می‌شود: Chapter 63 نحوه ترکیب چند Promise را ساخته است و Chapter 64 اکنون نشان می‌دهد چگونه همان الگو را در یک Async Function با await مصرف کنیم.

Sequential یا Parallel؟

اکنون می‌توانیم تفاوت دو Flow را دقیق‌تر ببینیم.

اگر عملیات دوم به نتیجه عملیات اول نیاز داشته باشد:

const user = await getUser();
const orders = await getOrders(user.id);

Sequential بودن بخشی از مسئله است.

getUser
↓
user.id
↓
getOrders

اما اگر عملیات‌ها مستقل باشند:

const [user, products] = await Promise.all([
getUser(),
getProducts()
]);

می‌توان آن‌ها را در یک Parallel Flow قرار داد:

getUser      ──┐
├──→ Promise.all → Results
getProducts ───┘

بنابراین انتخاب بین Sequential و Parallel نباید بر اساس ظاهر کد انجام شود.

قاعده ساده‌تر این است:

اگر Dependency وجود دارد، ترتیب را حفظ کنید؛ اگر Dependency وجود ندارد، عملیات‌های مستقل را بی‌دلیل Sequential نکنید.

این یکی از مهم‌ترین کاربردهای مهندسی async/await است.

async/await چه چیزی را تغییر نمی‌دهد؟

در این مرحله ممکن است به نظر برسد که async/await یک مدل کاملاً جدید برای Asynchronous JavaScript ساخته است.

اما چنین نیست.

ما هنوز همان Promiseها را داریم.

مثلاً:

async function loadRecipe() {
const recipe = await getRecipe();

return recipe;
}

در اینجا:

getRecipe()
↓
Promise
↓
await
↓
recipe

و خود loadRecipe() نیز یک Promise برمی‌گرداند.

بنابراین:

Promise
↓
async / await
↓
خوانایی بیشتر

مدل Promise حذف نشده است.

فقط شیوه بیان آن تغییر کرده است.

به همین دلیل async/await را نباید جایگزین Promise دانست.

async/await Syntax خواناتری برای کار با Promise-based Flow فراهم می‌کند.

یک مثال کامل

اکنون می‌توانیم تمام مسیر فصل را در یک مثال واحد ببینیم.

فرض کنید می‌خواهیم اطلاعات یک Recipe را دریافت کنیم:

async function loadRecipe(id) {
try {
const response = await fetch(`/api/recipes/${id}`);

    if (!response.ok) {
      throw new Error('Failed to load recipe');
    }

    const recipe = await response.json();

    return recipe;
} catch (error) {
console.error(error);
} finally {
console.log('Request finished');
}
}

در این مثال، هر بخش نتیجه یک نیاز قبلی است.

ابتدا Function باید بخشی از Promise-based Flow باشد:

async function loadRecipe(id) {

پس async وارد می‌شود.

سپس باید نتیجه fetch() را در همان جریان Function مصرف کنیم:

const response = await fetch(...);

پس await وارد می‌شود.

بعد باید Body پاسخ را پردازش کنیم:

const recipe = await response.json();

پس یک مرحله Sequential دیگر داریم، زیرا برای response.json() ابتدا به Response نیاز داریم.

سپس باید مسیر Failure را کنترل کنیم:

try {
...
} catch (error) {
...
}

و در نهایت Logic مستقلی که باید در پایان عملیات اجرا شود:

finally {
...
}

بنابراین مثال ما صرفاً مجموعه‌ای از Syntaxها نیست.

بلکه همان Concept Flow فصل را در یک مسئله واقعی نشان می‌دهد:

Promise
↓
async
↓
await
↓
Sequential Async Flow
↓
try / catch
↓
finally
یک نکته درباره Error Handling

در مثال قبلی یک نکته مهم وجود دارد.

async/await خودش مشخص نمی‌کند که یک Failure از نظر Application چه معنایی دارد.

نقش آن این است که Promise-based Flow را قابل مدیریت‌تر کند.

مثلاً ممکن است Application بخواهد:

خطا را ثبت کند.
پیام مناسب نمایش دهد.
مقدار پیش‌فرض برگرداند.
خطا را به لایه دیگری منتقل کند.

اما اینکه چگونه یک Strategy کامل برای Error Handling طراحی کنیم، موضوع مستقلی است و در Chapter 66 بررسی خواهد شد. Roadmap این فصل را مشخصاً برای Error، throw، try/catch/finally، Promise Rejection، Async Error و Recovery در نظر گرفته است.

در این فصل فقط تا جایی پیش می‌رویم که بدانیم async/await چگونه Failure را در جریان Promise-based خود قرار می‌دهد.

async/await و خوانایی کد

اکنون تفاوت اصلی را می‌توانیم در یک نگاه ببینیم.

Promise Chain:

getUser()
.then(user => getOrders(user.id))
.then(orders => {
console.log(orders);
})
.catch(error => {
console.error(error);
});

Async/Await:

async function loadData() {
try {
const user = await getUser();
const orders = await getOrders(user.id);

    console.log(orders);
} catch (error) {
console.error(error);
}
}

هر دو بر اساس Promise کار می‌کنند.

اما در نسخه دوم، Dependency میان مراحل مستقیماً در ترتیب کد دیده می‌شود:

user
↓
user.id
↓
orders

به همین دلیل async/await را می‌توان ابزاری برای بیان خواناتر جریان Asynchronous دانست، نه یک مدل مستقل از Promise.

Best Practices
async را بخشی از Promise-based API بدانید

وقتی Function قرار است نتیجه Asynchronous ارائه کند، async باعث می‌شود خروجی آن Promise باشد.

async function loadUser() {
return user;
}
await را برای بیان Dependency استفاده کنید

اگر عملیات دوم به نتیجه عملیات اول نیاز دارد:

const user = await getUser();
const orders = await getOrders(user.id);

این ترتیب باید حفظ شود.

عملیات مستقل را بی‌دلیل Sequential نکنید

اگر دو عملیات مستقل هستند:

const [user, products] = await Promise.all([
getUser(),
getProducts()
]);

استفاده از Promise Combinator مناسب‌تر از awaitهای پشت‌سرهم است.

ظاهر خطی کد را با Synchronous بودن اشتباه نگیرید

این:

const data = await getData();

فقط جریان کد را خطی‌تر نشان می‌دهد.

مدل اجرای آن همچنان Promise-based و Asynchronous است.

Error Path را فراموش نکنید

وقتی یک Async Operation بخشی مهم از Application است، فقط مسیر Success را طراحی نکنید.

مسیر Failure نیز باید مشخص باشد:

try {
await loadData();
} catch (error) {
// handle failure
}
Common Mistakes
تصور اینکه async Function مقدار معمولی برمی‌گرداند
async function getUser() {
return user;
}

const user = getUser();

در اینجا user یک Promise است، نه خود Object.

تصور اینکه await کل برنامه را متوقف می‌کند

await فقط ادامه اجرای همان Async Function را به نتیجه Promise وابسته می‌کند.

تصور اینکه async/await جای Promise را گرفته است

async/await بر پایه Promise کار می‌کند.

استفاده از awaitهای پشت‌سرهم برای عملیات مستقل
const user = await getUser();
const products = await getProducts();

اگر getProducts() به user وابسته نباشد، این ترتیب لزوماً بیان‌کننده نیاز واقعی برنامه نیست.

استفاده از try/catch بدون درک مسیر Failure

catch صرفاً برای نمایش یک پیام نیست.

باید مشخص باشد Application پس از Failure چه رفتاری دارد.

Summary

در فصل‌های قبل Promise به‌عنوان مدل مدیریت نتیجه آینده یک عملیات Async ساخته شد و سپس Promise Combinatorها برای مدیریت چند Promise معرفی شدند.

اما استفاده از Promise Chain در Flowهای چندمرحله‌ای می‌تواند خوانایی Dependency میان عملیات‌ها را کاهش دهد.

از همین نیاز، async/await وارد می‌شود.

async یک Function را به Async Function تبدیل می‌کند و Async Function همیشه Promise برمی‌گرداند.

سپس await اجازه می‌دهد نتیجه Promise را در همان جریان Async Function مصرف کنیم.

وقتی عملیات‌ها به یکدیگر وابسته باشند، awaitهای پشت‌سرهم می‌توانند یک Sequential Async Flow را بیان کنند.

اگر Promiseای Reject شود، می‌توان مسیر Failure را با try/catch مدیریت کرد و برای Logic مستقلی که باید در پایان هر دو مسیر اجرا شود، از finally استفاده کرد.

اما همه عملیات‌ها وابسته نیستند.

اگر چند عملیات مستقل باشند، بهتر است آن‌ها را بی‌دلیل Sequential نکنیم و از Promise Combinatorهایی مانند Promise.all استفاده کنیم.

پس مدل ذهنی این فصل چنین است:

Promise
↓
async
↓
await
↓
Sequential Async Flow
↓
try / catch
↓
finally
↓
Parallel Async
↓
Error Handling

این دقیقاً همان جریان Concept تعیین‌شده برای Chapter 64 است.

اما اکنون یک سؤال عمیق‌تر ایجاد شده است.

ما می‌دانیم:

const data = await getData();

چه معنایی در سطح کد دارد.

اما هنوز نمی‌دانیم JavaScript در Runtime با این کد چه می‌کند.

Key Takeaways
async Function همیشه Promise برمی‌گرداند.
async Promise را حذف نمی‌کند؛ Function را وارد Promise-based Flow می‌کند.
await نتیجه Promise را در جریان Async Function مصرف می‌کند.
await کل JavaScript را متوقف نمی‌کند.
Sequential Async Flow زمانی معنا دارد که میان عملیات‌ها Dependency وجود داشته باشد.
try/catch امکان مدیریت Failure مربوط به Promiseهای awaitشده را فراهم می‌کند.
finally برای Logic مستقلی است که باید پس از پایان Flow اجرا شود.
عملیات مستقل را می‌توان با Promise Combinatorهایی مانند Promise.all به‌صورت Parallel مدیریت کرد.
async/await جایگزین Promise نیست.
ظاهر خطی async/await به معنی Synchronous شدن Runtime نیست.
جزئیات اجرای await در Runtime موضوع فصل بعد است.
Technical Interview
Junior
async Function چیست؟

Functionای است که با async تعریف شده و همیشه یک Promise برمی‌گرداند.

await چه کاری انجام می‌دهد؟

نتیجه یک Promise را درون Async Function مصرف می‌کند و ادامه همان Function را به نتیجه آن Promise وابسته می‌کند.

آیا await کل برنامه را متوقف می‌کند؟

خیر. فقط ادامه اجرای همان Async Function تا مشخص‌شدن نتیجه Promise متوقف می‌ماند.

آیا async/await جایگزین Promise است؟

خیر. async/await بر پایه Promise ساخته شده و Syntax خواناتری برای کار با Promise ارائه می‌دهد.

Mid-Level
چه زمانی از Sequential Async Flow استفاده می‌کنیم؟

وقتی عملیات بعدی به نتیجه عملیات قبلی وابسته باشد.

const user = await getUser();
const orders = await getOrders(user.id);
چه زمانی عملیات‌ها را Parallel مدیریت می‌کنیم؟

وقتی عملیات‌ها مستقل باشند و به نتیجه یکدیگر نیاز نداشته باشند.

const [user, products] = await Promise.all([
getUser(),
getProducts()
]);
نقش try/catch در async/await چیست؟

برای مدیریت Failure مربوط به Promiseهایی که در جریان try با await مصرف می‌شوند.

چرا awaitهای پشت‌سرهم همیشه مناسب نیستند؟

زیرا اگر عملیات‌ها مستقل باشند، awaitهای پشت‌سرهم آن‌ها را Sequential می‌کنند، در حالی که می‌توانستند مستقل از یکدیگر پیش بروند.

Senior
آیا async/await مدل Asynchronous JavaScript را تغییر می‌دهد؟

خیر. async/await همچنان بر پایه Promise است. async خروجی Function را Promise-based می‌کند و await نحوه مصرف نتیجه Promise را خواناتر می‌سازد.

معیار انتخاب Sequential یا Parallel چیست؟

معیار اصلی Dependency میان عملیات‌هاست. اگر عملیات B به نتیجه A نیاز داشته باشد، Sequential Flow لازم است. اگر مستقل باشند، می‌توان آن‌ها را با Promise Combinatorهایی مانند Promise.all مدیریت کرد.

چرا ظاهر async/await نباید با Synchronous Execution اشتباه گرفته شود؟

زیرا Syntax خطی فقط نحوه بیان Flow را ساده می‌کند. عملیات همچنان Asynchronous و Promise-based است.

رابطه async/await با Promise چیست؟

async/await یک مدل مستقل از Promise نیست. async Function Promise برمی‌گرداند و await برای مصرف نتیجه Promise درون آن Function استفاده می‌شود.

Golden Answers
Junior

async/await Syntax خواناتری برای کار با Promise است. async باعث می‌شود Function یک Promise برگرداند و await امکان مصرف نتیجه Promise را درون آن Function فراهم می‌کند.

Mid-Level

وقتی عملیات‌ها به یکدیگر وابسته باشند، از awaitهای Sequential استفاده می‌کنم؛ اما اگر مستقل باشند، آن‌ها را بی‌دلیل پشت سر هم await نمی‌کنم و در صورت نیاز از Promise.all برای مدیریت Parallel استفاده می‌کنم.

Senior

async/await مدل Asynchronous JavaScript را تغییر نمی‌دهد؛ بلکه نحوه بیان Promise-based Flow را ساده‌تر می‌کند. Async Function همچنان Promise برمی‌گرداند و await ادامه همان Function را به نتیجه Promise وابسته می‌کند. بنابراین Syntax خطی‌تر شده، اما مدل Promise-based Runtime همچنان پابرجاست.

Conclusion

در این فصل، از Promise به async/await رسیدیم؛ نه به‌عنوان یک Concept مستقل، بلکه به‌عنوان پاسخ طبیعی به مسئله خوانایی Promise-based Code.

Promise به ما امکان داد نتیجه آینده یک عملیات را مدیریت کنیم.

اما وقتی این نتیجه بخشی از یک Flow چندمرحله‌ای شد، نیاز داشتیم Dependency میان مراحل را واضح‌تر بیان کنیم.

async Function را وارد Promise-based Flow کرد.

await امکان داد نتیجه Promise را در همان جریان Function مصرف کنیم.

از اینجا Sequential Async Flow شکل گرفت؛ جایی که ترتیب عملیات از Dependency میان آن‌ها ناشی می‌شود.

سپس Failure وارد روایت شد و try/catch مسیر خطا را در همان Async Flow قرار داد.

finally نیز زمانی وارد شد که به Logicای نیاز داشتیم که مستقل از Success یا Failure اجرا شود.

بعد مشخص شد که همه عملیات‌ها به یکدیگر وابسته نیستند. این مسئله ما را به Parallel Async و استفاده از Promise Combinatorها رساند.

بنابراین async/await را نباید جایگزین Promise دانست.

این Syntax فقط به ما کمک می‌کند همان Promise-based Flow را به شکلی نزدیک‌تر به منطق طبیعی برنامه بیان کنیم.

اما اکنون یک سؤال اساسی باقی مانده است:

وقتی اجرای Async Function به await می‌رسد، JavaScript در Runtime دقیقاً چه اتفاقی ایجاد می‌کند؟

اگر Function ادامه اجرای خود را متوقف می‌کند، این توقف چگونه اتفاق می‌افتد؟

وقتی Promise آماده شد، ادامه Function چگونه برمی‌گردد؟

Microtask چه نقشی در این میان دارد؟

و Event Loop چگونه این قطعات را در کنار Call Stack قرار می‌دهد؟

این پرسش‌ها ما را از Syntax به Runtime می‌برند.

و فصل بعد دقیقاً از همین نقطه آغاز می‌شود:

Chapter 65 — Async JavaScript Behind the Scenes