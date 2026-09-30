Chapter 62 — Promises
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مسئله‌ای را که در مدیریت چند Callback تو‌در‌تو ایجاد می‌شود، تحلیل کنید.
Promise را به‌عنوان مدل مدیریت نتیجه آینده یک عملیات Async تعریف کنید.
سه وضعیت اصلی Promise یعنی pending، fulfilled و rejected را توضیح دهید.
تفاوت Promise موفق و Promise ردشده را تحلیل کنید.
با then() نتیجه موفق Promise را مدیریت کنید.
با catch() خطای Promise را مدیریت کنید.
نقش finally() را در پایان یک جریان Promise توضیح دهید.
Promise Chaining را تحلیل کنید.
توضیح دهید چرا then() یک Promise جدید برمی‌گرداند.
تفاوت Error Handling در زنجیره Promiseها را درک کنید.
Promise را در سناریوهای واقعی مانند دریافت داده از API به‌کار ببرید.
Core Question

Promise چگونه نتیجه یک عملیات Async را به‌صورت قابل ترکیب مدیریت می‌کند؟

در فصل 16 دیدیم که Callback می‌تواند اجرای یک Function را به زمان یا سیستم دیگری واگذار کند. وقتی فقط یک عملیات Async داریم، این روش می‌تواند ساده باشد. اما با افزایش وابستگی میان چند عملیات، Callbackها می‌توانند به ساختاری تو‌در‌تو و دشوار برای مدیریت تبدیل شوند.

در چنین شرایطی، مسئله فقط «اجرای Async» نیست.

مسئله این است:

چگونه نتیجه‌ای را که هنوز آماده نیست، به شکلی مدیریت کنیم که بعداً بتوانیم ادامه منطقی برنامه را بر اساس آن نتیجه بسازیم؟

برای پاسخ به این سؤال، JavaScript مفهومی به نام Promise در اختیار ما قرار می‌دهد.

جریان این فصل از همین نیاز شکل می‌گیرد:

Callback Problem
↓
Promise
↓
Pending
↓
Fulfilled
↓
Rejected
↓
then
↓
catch
↓
finally
↓
Chaining

این ترتیب همان Concept Flow تعیین‌شده برای این فصل است.

مقدمه

فرض کنید Application باید اطلاعات یک Recipe را از Server دریافت کند.

در ساده‌ترین حالت، می‌توانیم از یک Callback استفاده کنیم:

getRecipe('pizza', function (recipe) {
console.log(recipe);
});

تا زمانی که Server پاسخ نداده است، نتیجه نهایی در اختیار برنامه نیست.

اما Application معمولاً به یک عملیات محدود نمی‌شود.

ممکن است بعد از دریافت Recipe لازم باشد:

اطلاعات آن را پردازش کنیم.
اطلاعات دیگری را دریافت کنیم.
نتیجه را در UI نمایش دهیم.

در این حالت، اگر هر مرحله Callback مخصوص خود را دریافت کند، ساختار برنامه می‌تواند به‌صورت تو‌در‌تو رشد کند.

مشکل اصلی این نیست که Callback نمی‌تواند عملیات Async را مدیریت کند.

Callback می‌تواند.

مشکل زمانی ایجاد می‌شود که نتیجه یک عملیات، ورودی عملیات بعدی باشد و تعداد این وابستگی‌ها افزایش پیدا کند.

در چنین شرایطی، ساختار کد به جای اینکه جریان منطقی عملیات را نشان دهد، ممکن است ساختار تو‌در‌توی Callbackها را نشان دهد.

این همان مسئله‌ای است که در فصل 16 با عنوان Callback Hell به آن رسیدیم.

اکنون به سؤالی طبیعی می‌رسیم:

آیا می‌توان نتیجه یک عملیات Async را به‌عنوان یک Value آینده مدل کرد تا بتوانیم مراحل بعدی را مستقل و قابل ترکیب تعریف کنیم؟

پاسخ، Promise است.

Promise
چرا Promise به وجود آمد؟

فرض کنید عملیاتی داریم که نتیجه آن همین الآن آماده نیست:

Request
↓
?
↓
Response

در زمان ارسال Request، نمی‌توانیم مقدار Response را در اختیار داشته باشیم.

اما برنامه باید بتواند بگوید:

وقتی نتیجه آماده شد، این کار را انجام بده.

و همچنین:

اگر عملیات شکست خورد، این مسیر را اجرا کن.

Callback یکی از روش‌های انجام این کار است.

Promise مدل دیگری ارائه می‌کند.

به جای اینکه Function را مستقیماً به‌عنوان Callback در اختیار یک API قرار دهیم، می‌توانیم یک Promise Object داشته باشیم که نشان‌دهنده نتیجه آینده عملیات است.

بنابراین Promise در اصل به یک مسئله مشخص پاسخ می‌دهد:

چگونه وضعیت و نتیجه آینده یک عملیات را به‌صورت یک Object قابل مدیریت نمایش دهیم؟

تعریف Promise
تعریف ساده

Promise یک Object است که نشان‌دهنده نتیجه نهایی یک عملیات است که ممکن است در آینده مشخص شود.

این نتیجه می‌تواند:

موفق باشد.
یا با شکست مواجه شود.

تا زمانی که نتیجه مشخص نشده است، Promise در وضعیت انتظار قرار دارد.

بنابراین Promise را می‌توان یک قرارداد برای نتیجه آینده دانست:

Promise
↓
Future Result

Promise خودش نتیجه نهایی نیست.

بلکه نماینده نتیجه‌ای است که هنوز مشخص نشده یا در حال مشخص شدن است.

تعریف فنی

Promise یک Object استاندارد JavaScript است که وضعیت نهایی یک عملیات را نمایش می‌دهد و امکان ثبت Handlerهایی برای واکنش به موفقیت یا شکست آن عملیات را فراهم می‌کند.

به همین دلیل، Promise دو بخش مهم را از هم جدا می‌کند:

Operation
↓
Promise
↓
Future Result

عملیات می‌تواند در یک زمان نتیجه تولید کند، اما کدی که به نتیجه نیاز دارد می‌تواند از قبل مشخص کند که در صورت موفقیت یا شکست چه کاری باید انجام شود.

این جداسازی، پایه Promise-based Programming است.

Promise یک Value معمولی نیست

فرض کنید:

const result = getRecipe();

اگر getRecipe() یک Promise برگرداند، مقدار result لزوماً Recipe نیست.

ممکن است:

result
↓
Promise
↓
Future Recipe

باشد.

بنابراین این تصور اشتباه است:

Promise همان نتیجه عملیات است.

Promise نماینده نتیجه آینده عملیات است.

برای دریافت و مدیریت نتیجه باید به Promise Handler متصل شویم؛ مفهومی که با then() آغاز می‌شود.

اما پیش از آن باید بدانیم Promise در چه وضعیتی قرار دارد.

وضعیت Promise

یک Promise در طول عمر خود سه وضعیت اصلی دارد:

pending
fulfilled
rejected

این سه وضعیت به یک مسئله مهم پاسخ می‌دهند:

اکنون نتیجه عملیات در چه وضعیتی قرار دارد؟

Pending

در ابتدا ممکن است نتیجه عملیات هنوز مشخص نشده باشد.

در این حالت Promise در وضعیت:

pending

قرار دارد.

مثلاً:

const promise = fetch('/api/recipes');

در اینجا عملیات درخواست آغاز شده است، اما در لحظه ایجاد Promise هنوز نمی‌دانیم Response نهایی چه خواهد بود.

مدل ذهنی:

Promise
↓
pending
↓
Waiting for result

pending به معنی خطا نیست.

فقط یعنی:

نتیجه نهایی هنوز مشخص نشده است.

Fulfilled

اگر عملیات با موفقیت به نتیجه برسد، Promise به وضعیت:

fulfilled

تغییر می‌کند.

مثلاً اگر Request بتواند Response دریافت کند، Promise مربوط به آن Request می‌تواند fulfilled شود.

مدل ذهنی:

pending
↓
fulfilled
↓
Success

در این وضعیت Promise یک نتیجه موفق دارد.

مثلاً:

fulfilled
↓
Response
Rejected

اگر عملیات با شکست مواجه شود، Promise به وضعیت:

rejected

می‌رسد.

مدل ذهنی:

pending
↓
rejected
↓
Failure

در این حالت Promise دارای یک Reason برای شکست است.

این Reason معمولاً یک Error یا Value مرتبط با شکست عملیات است.

Promise فقط یک‌بار تعیین تکلیف می‌شود

یکی از ویژگی‌های مهم Promise این است که وضعیت نهایی آن برگشت‌پذیر نیست.

یعنی:

pending
↓
fulfilled

یا:

pending
↓
rejected

اما Promise نمی‌تواند:

fulfilled
↓
pending

شود.

یا:

fulfilled
↓
rejected

شود.

پس Promise یک مسیر یک‌طرفه دارد:

pending
↙     ↘
fulfilled  rejected

به محض اینکه Promise به fulfilled یا rejected برسد، وضعیت آن نهایی است.

این ویژگی باعث می‌شود نتیجه Promise قابل اعتمادتر و قابل پیش‌بینی‌تر باشد.

ساختن یک Promise

تا اینجا Promise را به‌عنوان مدلی برای نتیجه آینده شناختیم.

اکنون می‌توانیم یک Promise ایجاد کنیم:

const promise = new Promise(function (resolve, reject) {
// asynchronous operation
});

تابعی که به Promise داده می‌شود، معمولاً Executor Function نام دارد.

دو Function در اختیار آن قرار می‌گیرد:

resolve
reject

resolve برای اعلام موفقیت و reject برای اعلام شکست استفاده می‌شود.

مثلاً:

const promise = new Promise(function (resolve, reject) {
const success = true;

if (success) {
resolve('Recipe loaded');
} else {
reject(new Error('Failed to load recipe'));
}
});

در این مثال، Promise ابتدا:

pending

است.

سپس بر اساس نتیجه عملیات یکی از این مسیرها را طی می‌کند:

resolve()
↓
fulfilled

یا:

reject()
↓
rejected

نکته مهم این است که resolve و reject نتیجه Promise را اعلام می‌کنند؛ کدی که می‌خواهد با آن نتیجه کار کند، باید از Handlerهای Promise استفاده کند.

Promise و عملیات واقعی

در Application واقعی معمولاً خودمان Promise را برای عملیات شبکه از صفر ایجاد نمی‌کنیم.

APIهایی مانند:

fetch()

خودشان Promise برمی‌گردانند.

مثلاً:

const promise = fetch('/api/recipes');

در اینجا:

fetch()
↓
Promise

قرار دارد.

Promise نشان می‌دهد که نتیجه Request در آینده مشخص خواهد شد.

این دقیقاً همان چیزی است که در فصل قبل هنگام بررسی Fetch API با آن مواجه شدیم: fetch() یک Promise برمی‌گرداند.

بنابراین اکنون می‌توانیم مفهوم Promise را به چیزی که قبلاً دیده‌ایم متصل کنیم:

HTTP Request
↓
fetch()
↓
Promise
↓
Future Response
مدیریت نتیجه با then()

اکنون یک مسئله جدید داریم.

Promise نتیجه آینده را نمایش می‌دهد، اما:

وقتی نتیجه آماده شد، چگونه با آن کار کنیم؟

برای این کار از:

then()

استفاده می‌کنیم.

مثلاً:

fetch('/api/recipes')
.then(function (response) {
console.log(response);
});

اینجا به Promise می‌گوییم:

اگر عملیات با موفقیت به نتیجه رسید، این Function را اجرا کن.

پس then() محل تعریف Success Handler است.

مدل ذهنی:

Promise
↓
fulfilled
↓
then()
↓
Success Handler
then() نتیجه Promise را دریافت می‌کند

فرض کنید Promise ما مقدار مشخصی را Resolve کند:

const promise = new Promise(function (resolve) {
resolve('Recipe loaded');
});

اکنون:

promise.then(function (result) {
console.log(result);
});

مقدار:

Recipe loaded

در اختیار Handler قرار می‌گیرد.

پس:

resolve(value)
↓
fulfilled
↓
then(handler)
↓
handler(value)

این ارتباط، یکی از مهم‌ترین مدل‌های ذهنی Promise است.

Promise و Callback چه تفاوتی دارند؟

در ظاهر، then() ممکن است شبیه Callback به نظر برسد.

مثلاً:

promise.then(function (result) {
console.log(result);
});

در اینجا نیز یک Function در اختیار سیستم قرار داده‌ایم.

پس Promise قرار نیست Callback را حذف کند.

بلکه مسئله را در سطح بالاتری مدل می‌کند.

در Callback:

Operation
↓
Callback
↓
Result

اما در Promise:

Operation
↓
Promise
↓
then()
↓
Result

این تفاوت باعث می‌شود Promise بتواند نتیجه عملیات را به‌صورت یک Object مستقل مدل کند.

همین Object می‌تواند مبنای اتصال عملیات بعدی قرار گیرد.

و این موضوع ما را به مفهوم مهم‌تری می‌رساند: Chaining.

اما قبل از آن، باید شکست عملیات را نیز مدیریت کنیم.

مدیریت شکست با catch()

موفقیت تنها حالت ممکن نیست.

در یک Application واقعی ممکن است:

Server در دسترس نباشد.
Request شکست بخورد.
داده نامعتبر باشد.
یک عملیات داخلی با Error مواجه شود.

Promise این وضعیت را با:

rejected

مدل می‌کند.

برای مدیریت این وضعیت از:

catch()

استفاده می‌کنیم.

مثلاً:

fetch('/api/recipes')
.then(function (response) {
console.log(response);
})
.catch(function (error) {
console.error(error);
});

مدل ذهنی:

Promise
↓
rejected
↓
catch()
↓
Error Handler

بنابراین:

then()  → success
catch() → failure

این تفکیک باعث می‌شود مسیر موفقیت و مسیر شکست صریح‌تر باشند.

catch() و خطاهای زنجیره

نکته مهم‌تر این است که catch() فقط برای خطای اولیه نیست.

فرض کنید:

fetch('/api/recipes')
.then(function (response) {
return response.json();
})
.then(function (data) {
console.log(data);
})
.catch(function (error) {
console.error(error);
});

اگر یکی از Promiseهای این زنجیره Reject شود، یا Handlerها خطایی ایجاد کنند، می‌توانیم خطا را در انتهای زنجیره مدیریت کنیم.

بنابراین catch() می‌تواند نقش یک مسیر Error Handling برای جریان Promise داشته باشد.

مدل ذهنی:

Promise
↓
then()
↓
then()
↓
catch()

این ساختار یکی از دلایل مهم قابل ترکیب بودن Promiseها است.

finally()

اکنون دو مسیر اصلی را داریم:

Success → then()
Failure → catch()

اما بعضی عملیات‌ها به نتیجه موفق یا ناموفق وابسته نیستند.

مثلاً فرض کنید هنگام ارسال Request یک Loading Indicator نمایش داده‌ایم.

چه Request موفق شود و چه شکست بخورد، در پایان باید Loading Indicator حذف شود.

در چنین شرایطی از:

finally()

استفاده می‌کنیم.

مثلاً:

fetch('/api/recipes')
.then(function (response) {
console.log(response);
})
.catch(function (error) {
console.error(error);
})
.finally(function () {
console.log('Request finished');
});

در اینجا finally() برای کدی مناسب است که باید در پایان جریان Promise، مستقل از نتیجه نهایی، اجرا شود.

مدل ذهنی:

              ┌→ then()
Promise ──────┤
└→ catch()
↓
finally()
finally() نتیجه را دریافت نمی‌کند

یک تفاوت مهم وجود دارد.

در:

then(function (result) {
// ...
});

Handler به نتیجه موفق دسترسی دارد.

در:

catch(function (error) {
// ...
});

Handler به Reason شکست دسترسی دارد.

اما:

finally(function () {
// ...
});

برای دریافت نتیجه موفق یا Error طراحی نشده است.

چون هدف آن اجرای Cleanup یا Logic مشترک در پایان جریان است.

مثلاً:

showLoading();

fetch('/api/recipes')
.then(function (response) {
return response.json();
})
.catch(function (error) {
console.error(error);
})
.finally(function () {
hideLoading();
});

hideLoading() به این موضوع وابسته نیست که Request موفق بوده یا شکست خورده است.

چرا Promiseها قابل ترکیب هستند؟

تا اینجا Promise را می‌شناسیم و می‌توانیم:

Promise
↓
then()
catch()
finally()

را روی آن قرار دهیم.

اما یک ویژگی هنوز باقی مانده است که Promise را برای مدیریت جریان‌های Async بسیار قدرتمند می‌کند.

فرض کنید بعد از دریافت Recipe باید داده را پردازش کنیم.

fetch('/api/recipes')
.then(function (response) {
return response.json();
});

در اینجا response.json() خودش یک Promise برمی‌گرداند.

بنابراین نتیجه یک مرحله می‌تواند وارد مرحله بعد شود.

اینجاست که Promise از یک Handler ساده به یک جریان قابل ترکیب تبدیل می‌شود.

Promise Chaining

فرض کنید یک عملیات Async داریم:

fetch('/api/recipes')

و بعد باید Response را به JSON تبدیل کنیم:

response.json()

می‌توانیم این دو عملیات را به‌صورت زنجیره‌ای بنویسیم:

fetch('/api/recipes')
.then(function (response) {
return response.json();
})
.then(function (data) {
console.log(data);
});

مدل ذهنی:

fetch()
↓
Promise
↓
then()
↓
response.json()
↓
Promise
↓
then()
↓
data

این همان Promise Chaining است.

چرا then() یک Promise جدید برمی‌گرداند؟

این بخش از Promise بسیار مهم است.

وقتی می‌نویسیم:

promise.then(...)

نتیجه then() خودش یک Promise جدید است.

به همین دلیل می‌توانیم بنویسیم:

promise
.then(...)
.then(...)
.then(...);

یعنی:

Promise A
↓
then()
↓
Promise B
↓
then()
↓
Promise C

هر مرحله می‌تواند نتیجه‌ای برای مرحله بعد ایجاد کند.

Return در Promise Chain

فرض کنید:

const promise = Promise.resolve(10);

promise
.then(function (value) {
return value * 2;
})
.then(function (value) {
console.log(value);
});

در Handler اول:

return value * 2;

اجرا می‌شود.

مقدار:

20

به Promise مرحله بعد منتقل می‌شود.

بنابراین:

10
↓
value * 2
↓
20
↓
then()

این همان چیزی است که باعث می‌شود مراحل مختلف یک عملیات بتوانند به‌صورت خطی به یکدیگر متصل شوند.

اگر Handler یک Promise برگرداند چه می‌شود؟

این بخش قدرت اصلی Chaining را نشان می‌دهد.

فرض کنید:

fetch('/api/recipes')
.then(function (response) {
return response.json();
})
.then(function (data) {
return saveRecipe(data);
})
.then(function () {
console.log('Recipe saved');
});

در مرحله دوم:

return saveRecipe(data);

اگر saveRecipe() یک Promise برگرداند، Promise مرحله بعد منتظر نتیجه آن خواهد بود.

بنابراین جریان مفهومی چنین است:

Request
↓
Promise
↓
then()
↓
Response
↓
Promise
↓
then()
↓
Save Operation
↓
Promise
↓
then()

به این ترتیب، هر مرحله می‌تواند مرحله بعدی را تغذیه کند.

این همان چیزی است که در طراحی Async Flow اهمیت دارد:

نتیجه هر مرحله می‌تواند Promise مرحله بعد را تعیین کند.

Promise Chaining و Callback Hell

اکنون می‌توانیم مسئله‌ای را که در ابتدای فصل مطرح کردیم دوباره ببینیم.

در Callbackهای تو‌در‌تو، ممکن است ساختار به چنین شکلی تبدیل شود:

Operation A
↓
Callback
↓
Operation B
↓
Callback
↓
Operation C
↓
Callback

با Promise می‌توانیم جریان را به شکل زنجیره‌ای بیان کنیم:

operationA()
.then(function (resultA) {
return operationB(resultA);
})
.then(function (resultB) {
return operationC(resultB);
});

ساختار منطقی عملیات اکنون واضح‌تر است:

Operation A
↓
Operation B
↓
Operation C

بنابراین ارزش Promise فقط این نیست که Callback را با then() جایگزین می‌کند.

ارزش اصلی آن در مدل‌سازی و ترکیب نتیجه‌های آینده است.

Promise و کنترل جریان

در یک جریان Promise، هر مرحله می‌تواند:

یک Value برگرداند.
یک Promise برگرداند.
Error ایجاد کند.

این سه حالت رفتار مرحله بعد را تعیین می‌کنند.

مثلاً:

promise
.then(function (value) {
return value * 2;
})
.then(function (value) {
console.log(value);
});

یک Value معمولی باعث می‌شود مرحله بعد با یک Promise Fulfilled ادامه پیدا کند.

اگر Promise برگردانیم:

return anotherPromise();

مرحله بعد به نتیجه آن Promise وابسته می‌شود.

و اگر Error ایجاد شود:

throw new Error('Something went wrong');

جریان وارد مسیر Rejection می‌شود و catch() می‌تواند آن را مدیریت کند.

مدل ذهنی:

then()
├── return Value
│      ↓
│   next then()
│
├── return Promise
│      ↓
│   wait for that Promise
│
└── throw Error
↓
catch()

این مدل، اساس Promise Chaining است.

یک مثال واقعی‌تر

فرض کنید Application یک Recipe را دریافت می‌کند، سپس JSON را استخراج می‌کند و در نهایت آن را نمایش می‌دهد:

fetch('/api/recipes/42')
.then(function (response) {
return response.json();
})
.then(function (recipe) {
console.log(recipe);
})
.catch(function (error) {
console.error(error);
})
.finally(function () {
console.log('Request finished');
});

جریان این کد را می‌توان چنین تحلیل کرد:

fetch()
↓
Promise
↓
then()
↓
response.json()
↓
Promise
↓
then()
↓
recipe
↓
catch()       ← failure path
↓
finally()     ← completion/cleanup

در این مثال، هر بخش مسئولیت مشخصی دارد:

fetch()
→ شروع عملیات

then()
→ پردازش نتیجه

then()
→ استفاده از داده

catch()
→ مدیریت خطا

finally()
→ عملیات پایانی

این همان تفکر مهندسی موردنیاز برای طراحی جریان‌های Async است.

Promise یک Thread جدید ایجاد نمی‌کند

یک سوءبرداشت رایج این است:

Promise باعث می‌شود JavaScript یک Thread جدید ایجاد کند.

این تعریف صحیح نیست.

Promise یک مدل برای مدیریت نتیجه آینده است.

Promise خودش Thread ایجاد نمی‌کند.

همچنین Promise به‌تنهایی توضیح نمی‌دهد که عملیات Async دقیقاً چگونه توسط Runtime یا Host Environment اجرا می‌شود.

در این فصل فقط به این اندازه نیاز داریم که بدانیم:

Operation
↓
Promise
↓
Future Result

جزئیات Runtime، Microtask و Event Loop در فصل‌های بعدی بررسی خواهند شد.

این تفکیک مهم است، زیرا Promise یک Abstraction برای مدیریت نتیجه است؛ نه توضیح کامل مکانیزم اجرای Async در Runtime.

Promise و Fetch

در فصل 61 با Fetch API دیدیم:

fetch(url)

یک Promise برمی‌گرداند.

اکنون می‌توانیم این رابطه را کامل‌تر تحلیل کنیم:

HTTP Request
↓
fetch()
↓
Promise
↓
pending
↓
┌────┴────┐
↓         ↓
fulfilled rejected
↓         ↓
then()    catch()
\   /
finally()

این مدل نشان می‌دهد که Fetch و Promise دو مفهوم متفاوت هستند.

fetch() یک API برای انجام HTTP Request است.

Promise مدلی است که نتیجه آینده این عملیات را نمایش می‌دهد.

بنابراین:

Fetch
→ performs HTTP-related operation

Promise
→ represents the future result
Promise و Callback یک رقابت ساده نیستند

نباید Promise را صرفاً به‌عنوان «نسخه بهتر Callback» در نظر گرفت.

Callback یک الگوی واگذاری اجرای یک Function است.

Promise یک مدل برای نمایش و مدیریت نتیجه آینده یک عملیات است.

در Promise همچنان Functionهایی مانند:

then(handler)
catch(handler)

در اختیار سیستم قرار می‌دهیم.

بنابراین Callback هنوز در زیرساخت Promise نقش دارد.

تفاوت اصلی در سطح Abstraction است.

در Callback تمرکز بیشتر روی این است:

وقتی اتفاق افتاد، این Function را اجرا کن.

در Promise تمرکز روی این است:

این Object نتیجه آینده عملیات را نمایش می‌دهد و می‌توانم جریان موفقیت، شکست و مراحل بعدی را روی آن تعریف کنم.

این تفاوت، مدل ذهنی دقیق‌تری از Promise ایجاد می‌کند.

Promise Chaining به‌عنوان یک جریان داده

یکی از مهم‌ترین مدل‌های ذهنی این فصل را می‌توان چنین بیان کرد:

Promise
↓
Result
↓
Transformation
↓
New Promise
↓
Next Result
↓
Transformation

مثلاً:

getUser()
.then(function (user) {
return getOrders(user.id);
})
.then(function (orders) {
return calculateTotal(orders);
})
.then(function (total) {
console.log(total);
})
.catch(function (error) {
console.error(error);
});

اینجا داده در یک مسیر حرکت می‌کند:

User
↓
Orders
↓
Total

هر مرحله نتیجه مرحله قبلی را دریافت می‌کند و می‌تواند Promise مرحله بعد را تولید کند.

این ساختار به ما اجازه می‌دهد عملیات Async وابسته را بدون تو‌در‌تو کردن Callbackها بیان کنیم.

اشتباه مهم: تصور اینکه Promise همیشه Async است

این عبارت دقیق نیست:

Promise همیشه به‌صورت Async اجرا می‌شود.

Promise Object می‌تواند با:

Promise.resolve(...)

یا:

Promise.reject(...)

نیز ایجاد شود.

همچنین Executor مربوط به:

new Promise(...)

به‌صورت معمول هنگام ایجاد Promise اجرا می‌شود.

بنابراین نباید Promise را با «اجرای Async» یکی بدانیم.

مدل دقیق‌تر این است:

Promise ابزاری برای نمایش و مدیریت نتیجه آینده یک عملیات است و می‌تواند برای عملیات Async نیز استفاده شود.

این تفاوت میان مدل نتیجه و مکانیزم اجرای عملیات بسیار مهم است.

اشتباه مهم: Promise فقط برای HTTP نیست

چون در فصل‌های اخیر Promise را با fetch() دیده‌ایم، ممکن است تصور شود Promise فقط برای HTTP Request کاربرد دارد.

این تصور صحیح نیست.

Promise می‌تواند نتیجه آینده انواع مختلفی از عملیات را مدل کند.

مثلاً:

Network Request
File Operation
Timer-based Operation
Database Operation
User-related Async Operation

مهم این نیست که عملیات دقیقاً چه نوعی است.

مهم این است که:

نتیجه عملیات اکنون در دسترس نیست و باید در آینده مشخص شود.

Best Practices
جریان Promise را بر اساس مسئولیت‌ها طراحی کنید

بهتر است هر مرحله مسئولیت مشخصی داشته باشد:

fetch('/api/recipes')
.then(response => response.json())
.then(data => renderRecipes(data))
.catch(error => showError(error))
.finally(() => hideLoading());

در این ساختار:

Request
→ Parse
→ Render
→ Error Handling
→ Cleanup

جریان قابل خواندن است.

Promiseها را بی‌دلیل تو‌در‌تو نکنید

این ساختار:

promise.then(function (value) {
return anotherPromise.then(function (result) {
return anotherPromiseAgain.then(function () {
// ...
});
});
});

اغلب نشان می‌دهد که از قابلیت Chaining به‌درستی استفاده نشده است.

در بسیاری از موارد می‌توان آن را به شکل خطی نوشت:

promise
.then(function (value) {
return anotherPromise(value);
})
.then(function (result) {
return anotherPromiseAgain(result);
});
خطا را در جریان Promise مدیریت کنید

اگر یک عملیات می‌تواند شکست بخورد، مسیر خطا را نیز طراحی کنید:

operation()
.then(handleSuccess)
.catch(handleError);

Async Flow بدون Error Handling یک جریان کامل نیست.

finally() را برای Cleanup استفاده کنید

کارهایی مانند:

Hide Loading
Release UI State
Cleanup
Reset Temporary State

معمولاً به نتیجه موفق یا ناموفق وابسته نیستند.

در چنین شرایطی:

finally()

انتخاب طبیعی‌تری است.

اشتباهات رایج
1. Promise را با نتیجه نهایی اشتباه گرفتن

این تصور:

Promise === Result

صحیح نیست.

Promise نماینده نتیجه آینده است.

2. تصور اینکه then() فقط برای Promiseهای موفق است

then() برای ثبت Handler موفقیت استفاده می‌شود، اما خودش بخشی از زنجیره Promise است و می‌تواند Promise جدید تولید کند.

3. استفاده نکردن از return در Chain

این کد:

promise
.then(function (value) {
anotherPromise(value);
})
.then(function (result) {
console.log(result);
});

با این کد یکسان نیست:

promise
.then(function (value) {
return anotherPromise(value);
})
.then(function (result) {
console.log(result);
});

در حالت دوم Promise مرحله اول به Promise مرحله بعد متصل شده است.

4. تصور اینکه finally() نتیجه را دریافت می‌کند

finally() برای دسترسی به Result یا Error طراحی نشده است.

هدف اصلی آن اجرای Logic مشترک در پایان جریان است.

5. یکی دانستن Promise با Event Loop

Promise و Event Loop مفاهیم مرتبطی هستند، اما یک مفهوم نیستند.

Promise مدل مدیریت نتیجه است.

Event Loop بخشی از مدل اجرای Async JavaScript است که در فصل‌های بعدی بررسی خواهد شد.

Summary

Callback می‌تواند اجرای Logic را به زمان دیگری واگذار کند، اما وقتی چند عملیات Async به یکدیگر وابسته می‌شوند، ساختار Callbackهای تو‌در‌تو می‌تواند مدیریت جریان و خطا را دشوار کند.

Promise برای مدل‌سازی همین مسئله وارد می‌شود.

Promise یک Object است که نتیجه آینده یک عملیات را نمایش می‌دهد.

یک Promise در ابتدا در وضعیت:

pending

قرار دارد و در نهایت به یکی از دو وضعیت:

fulfilled
rejected

می‌رسد.

برای مدیریت این وضعیت‌ها از:

then()
catch()
finally()

استفاده می‌کنیم.

اما ویژگی مهم‌تر Promise این است که then() یک Promise جدید برمی‌گرداند. به همین دلیل می‌توان چند مرحله را به‌صورت زنجیره‌ای به یکدیگر متصل کرد.

در نتیجه:

Operation
↓
Promise
↓
Result
↓
Next Operation
↓
New Promise

به یک مدل قابل ترکیب برای مدیریت Async Flow تبدیل می‌شود.

Promise بنابراین فقط یک جایگزین برای Callback نیست؛ بلکه مدلی برای نمایش، انتقال و ترکیب نتیجه‌های آینده است.

Key Takeaways
Promise نماینده نتیجه آینده یک عملیات است.
Promise در ابتدا pending است.
Promise در نهایت یا fulfilled می‌شود یا rejected.
وضعیت نهایی Promise برگشت‌پذیر نیست.
then() برای مدیریت مسیر موفقیت استفاده می‌شود.
catch() برای مدیریت مسیر شکست استفاده می‌شود.
finally() برای Logic مشترک در پایان جریان مناسب است.
then() یک Promise جدید برمی‌گرداند.
بازگرداندن Promise در یک Handler، مراحل Promise Chain را به یکدیگر متصل می‌کند.
Promise Chaining امکان ساخت جریان خطی و قابل ترکیب برای عملیات وابسته را فراهم می‌کند.
Promise خودش Thread یا Runtime جدید ایجاد نمی‌کند.
Promise فقط مخصوص HTTP یا fetch() نیست.
Promise و Event Loop یک مفهوم نیستند.
Promise مدل مدیریت نتیجه آینده است؛ Runtime تعیین می‌کند این عملیات چگونه اجرا شود.
Technical Interview
Junior Level
Promise چیست؟

Promise یک Object است که نتیجه آینده یک عملیات را نمایش می‌دهد. این نتیجه می‌تواند در نهایت موفق یا ناموفق باشد.

Promise چه وضعیت‌هایی دارد؟

سه وضعیت اصلی دارد:

pending
fulfilled
rejected
تفاوت then() و catch() چیست؟

then() برای مدیریت نتیجه موفق و catch() برای مدیریت شکست یا Rejection در جریان Promise استفاده می‌شود.

finally() چه کاربردی دارد؟

برای اجرای Logic مشترک در پایان جریان Promise استفاده می‌شود؛ بدون اینکه به موفقیت یا شکست عملیات وابسته باشد.

Mid-Level
چرا Promise نسبت به Callback برای مدیریت چند عملیات Async قابل ترکیب‌تر است؟

زیرا Promise نتیجه آینده عملیات را به‌صورت یک Object مدل می‌کند و then() با برگرداندن Promise جدید امکان ایجاد یک Chain از عملیات وابسته را فراهم می‌کند.

چرا then() یک Promise جدید برمی‌گرداند؟

زیرا نتیجه Handler مرحله فعلی باید بتواند به مرحله بعدی منتقل شود. اگر Handler یک Value یا Promise برگرداند، این نتیجه مبنای Promise مرحله بعد قرار می‌گیرد.

اگر یک Handler در Promise Chain یک Promise برگرداند چه اتفاقی می‌افتد؟

Promise جدید حاصل از then() تا تعیین تکلیف Promise بازگردانده‌شده، نتیجه نهایی خود را مشخص نمی‌کند و جریان به نتیجه آن Promise وابسته می‌شود.

تفاوت Promise با Callback چیست؟

Callback یک Function است که برای اجرای Logic در زمان یا شرایط مشخص واگذار می‌شود؛ Promise یک Object است که نتیجه آینده یک عملیات را مدل می‌کند و امکان ترکیب مراحل موفقیت و شکست را فراهم می‌کند.

Senior Level
چرا Promise را نباید صرفاً «نسخه بهتر Callback» دانست؟

زیرا تفاوت اصلی در Abstraction است. Callback بر واگذاری اجرای یک Function تمرکز دارد، در حالی که Promise نتیجه آینده یک عملیات را به‌عنوان یک Object قابل ترکیب مدل می‌کند. این مدل اجازه می‌دهد Success، Failure و مراحل بعدی به‌صورت زنجیره‌ای تعریف شوند.

چرا Return کردن Promise در یک then() اهمیت دارد؟

چون Promise Chain بر اساس Promise بازگشتی مرحله قبل ساخته می‌شود. اگر Handler یک Promise را بدون return ایجاد کند، آن Promise بخشی از Chain اصلی نخواهد شد. اما با return، Promise جدید به نتیجه مرحله فعلی متصل می‌شود.

آیا Promise خودش باعث Async شدن کد می‌شود؟

خیر. Promise یک مدل برای مدیریت نتیجه عملیات است، نه مکانیزم ایجاد Thread یا اجرای Async. Promise می‌تواند نتیجه یک عملیات Async را نمایش دهد، اما سازوکار اجرای آن عملیات به Runtime و Host Environment مربوط است.

رابطه Promise با Event Loop چیست؟

Promise یک Abstraction برای مدیریت نتیجه عملیات است و Event Loop بخشی از مدل اجرای Async JavaScript است. نحوه زمان‌بندی ادامه اجرای Promiseها به Runtime مربوط می‌شود و جزئیات آن در لایه Runtime بررسی می‌شود، نه در تعریف بنیادی Promise.

Golden Answers

Promise چیست؟

Promise یک Object است که نتیجه آینده یک عملیات را نمایش می‌دهد و امکان مدیریت موفقیت، شکست و ادامه جریان را فراهم می‌کند.

Promise چه وضعیت‌هایی دارد؟

Promise ابتدا pending است و سپس به یکی از وضعیت‌های نهایی fulfilled یا rejected می‌رسد.

چرا Promise قابل Chain شدن است؟

چون then() یک Promise جدید برمی‌گرداند و نتیجه هر مرحله می‌تواند مبنای مرحله بعدی قرار گیرد.

تفاوت then() و catch() چیست؟

then() مسیر موفقیت را مدیریت می‌کند، در حالی که catch() مسیر Rejection و خطا را مدیریت می‌کند.

نقش finally() چیست؟

finally() برای اجرای Logic مشترک در پایان جریان استفاده می‌شود؛ چه Promise موفق شده باشد و چه شکست خورده باشد.

چرا Promise از Callback Hell جلوگیری می‌کند؟

Promise با مدل‌سازی نتیجه آینده و فراهم کردن Chaining، عملیات وابسته را می‌تواند به‌صورت یک جریان خطی و قابل ترکیب بیان کند، به جای اینکه Callbackها به شکل تو‌در‌تو قرار بگیرند.

Conclusion

در فصل 16 مسئله‌ای را دیدیم که با افزایش تعداد Callbackها ایجاد می‌شود: جریان Async می‌تواند به ساختاری تو‌در‌تو و دشوار برای ترکیب و مدیریت خطا تبدیل شود.

Promise پاسخ JavaScript به این نیاز است.

اما مهم‌ترین چیزی که باید از Promise به خاطر سپرد، Syntax آن نیست.

مدل ذهنی آن است:

Operation
↓
Promise
↓
Future Result
↓
Success / Failure
↓
Next Operation

Promise نتیجه‌ای را که هنوز آماده نیست به یک Object قابل مدیریت تبدیل می‌کند. سپس then()، catch() و finally() امکان تعریف رفتارهای مربوط به نتیجه، شکست و پایان جریان را فراهم می‌کنند.

مهم‌تر از همه، Promiseها می‌توانند به یکدیگر متصل شوند:

Promise
↓
then()
↓
Promise
↓
then()
↓
Promise

در نتیجه می‌توانیم یک جریان Async را به‌صورت مرحله‌ای و قابل ترکیب مدل کنیم.

اما اینجا یک سؤال جدید شکل می‌گیرد:

اگر چند Promise مستقل داشته باشیم، چگونه آن‌ها را با یکدیگر ترکیب کنیم تا مثلاً چند عملیات به‌صورت هم‌زمان اجرا شوند و نتیجه آن‌ها را به‌عنوان یک جریان واحد مدیریت کنیم؟

این سؤال ما را به Promise Combinators می‌رساند.