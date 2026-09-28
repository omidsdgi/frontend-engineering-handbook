Chapter 61 — Fetch API Communication
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

جایگاه Fetch API را در HTTP Communication توضیح دهید.
رابطه میان fetch()، Request و Response را تحلیل کنید.
توضیح دهید چرا fetch() نتیجه عملیات HTTP را به‌صورت Promise در اختیار برنامه قرار می‌دهد.
ساختار Response و Response Body را از یکدیگر تفکیک کنید.
JSON را از Response دریافت و به JavaScript Data تبدیل کنید.
تفاوت دریافت یک HTTP Response با موفقیت HTTP Request را تشخیص دهید.
نقش response.ok و response.status را توضیح دهید.
HTTP Error را از Network Error تفکیک کنید.
Fetch API را به‌عنوان مرز ارتباطی Application با API تحلیل کنید.
Core Question

چگونه با Fetch API به‌صورت مدرن با HTTP Resources کار کنیم؟

در فصل قبل دیدیم که Browser چگونه با Server ارتباط برقرار می‌کند.

یک Client برای دریافت یا ارسال Resource، یک HTTP Request ایجاد می‌کند. Request به Server می‌رسد و Server در پاسخ، یک HTTP Response ایجاد می‌کند.

در این جریان مفاهیمی مانند HTTP Methods، Request Headers، Response Headers و Status Codes مشخص می‌کنند Request و Response چه معنایی دارند.

مدل کلی ارتباط را می‌توان چنین دید:

Client
↓
Request
↓
HTTP
↓
Server
↓
Response

اما دانستن مدل HTTP هنوز یک مسئله عملی را حل نکرده است:

JavaScript چگونه این ارتباط را از داخل Application ایجاد می‌کند؟

فرض کنید یک Recipe Application داریم و می‌خواهیم Recipe شماره 42 را از API دریافت کنیم.

در سطح HTTP، Request ما چیزی شبیه این است:

GET /api/recipes/42

اما JavaScript برای ایجاد چنین ارتباطی به یک Browser API نیاز دارد.

این همان جایی است که Fetch API وارد می‌شود.

جریان این فصل از همین نیاز ساخته می‌شود:

HTTP
↓
fetch
↓
Promise
↓
Request
↓
Response
↓
Body
↓
JSON
↓
HTTP Errors
↓
API Integration

هدف این فصل حفظ کردن چند Method از Fetch API نیست.

هدف، ساختن یک مدل ذهنی صحیح از مسیر ارتباط Application با HTTP Resource است.

از HTTP به fetch()

HTTP مشخص می‌کند Client و Server چگونه با یکدیگر ارتباط برقرار کنند.

اما JavaScript برای شروع این ارتباط باید از یک API استفاده کند.

در Browser، Fetch API این امکان را فراهم می‌کند:

fetch('/api/recipes/42');

در اینجا fetch() یک HTTP Communication را آغاز می‌کند.

نکته مهم این است که Fetch API جای HTTP را نمی‌گیرد.

HTTP همچنان همان پروتکل ارتباطی است که در فصل قبل شناختیم.

Fetch API فقط Interfaceای است که JavaScript از طریق آن می‌تواند با HTTP Resources کار کند.

بنابراین:

JavaScript Application
↓
fetch()
↓
HTTP Request
↓
Server

در این Request، اگر Method مشخص نشود، Fetch API از GET استفاده می‌کند.

اگر بخواهیم Method دیگری را مشخص کنیم، می‌توانیم Request را پیکربندی کنیم:

fetch('/api/recipes', {
method: 'POST'
});

همان مفهومی که در فصل قبل درباره HTTP Methods آموختیم، اکنون از طریق Fetch API در اختیار JavaScript قرار می‌گیرد.

همین موضوع درباره Headers نیز صادق است:

fetch('/api/recipes', {
method: 'POST',
headers: {
'Content-Type': 'application/json'
}
});

در نتیجه Fetch API یک مدل جدید برای HTTP تعریف نمی‌کند؛ بلکه ابزار JavaScript برای استفاده از همان HTTP است.

اما هنوز یک مشکل وجود دارد.

وقتی fetch() اجرا می‌شود، Server هنوز Response را برنگردانده است.

پس نتیجه این عملیات در همان لحظه چه چیزی خواهد بود؟

چرا fetch() یک Promise برمی‌گرداند؟

به این کد توجه کنید:

const result = fetch('/api/recipes/42');

ممکن است تصور کنیم result همان Recipe است.

اما چنین نیست.

HTTP Communication یک عملیات فوری نیست.

Browser باید Request را ارسال کند، Server آن را پردازش کند و Response را برگرداند.

بنابراین JavaScript نمی‌تواند Data نهایی را همان لحظه در اختیار ما قرار دهد.

fetch() یک Promise برمی‌گرداند؛ یعنی نتیجه آینده عملیات HTTP را نمایندگی می‌کند.

مدل ساده این ارتباط:

fetch()
↓
Promise
↓
Future Response

برای دریافت Response می‌توانیم Promise را مصرف کنیم:

fetch('/api/recipes/42')
.then(response => {
console.log(response);
});

در اینجا response پس از تکمیل عملیات HTTP در اختیار Application قرار می‌گیرد.

در این فصل Promise فقط به‌اندازه‌ای معرفی می‌شود که رفتار fetch() قابل درک باشد.

ما هنوز وارد مدل کامل Promise، وضعیت‌های آن و روش‌های مدیریت آن نمی‌شویم؛ زیرا این مفاهیم موضوع فصل بعد هستند.

در اینجا تنها این رابطه را باید در ذهن داشته باشیم:

fetch() نتیجه آینده عملیات HTTP را به‌صورت Promise در اختیار JavaScript قرار می‌دهد.

اکنون می‌توانیم یک مرحله جلوتر برویم.

Promise در نهایت چه چیزی را به ما می‌رساند؟

Response.

Request؛ چیزی که به Server می‌فرستیم

تا اینجا می‌دانیم که:

fetch()
↓
Promise
↓
Response

اما Response نتیجه یک Request است.

Application ابتدا باید مشخص کند چه چیزی را از Server می‌خواهد.

ساده‌ترین Request:

fetch('/api/recipes/42');

در این حالت Browser برای Resource مشخص‌شده یک HTTP Request ایجاد می‌کند.

اگر بخواهیم Request را دقیق‌تر کنترل کنیم، می‌توانیم Method، Headers و Body را نیز مشخص کنیم:

fetch('/api/recipes', {
method: 'POST',
headers: {
'Content-Type': 'application/json'
},
body: JSON.stringify({
title: 'Pasta'
})
});

در اینجا:

method مشخص می‌کند چه نوع عملیاتی روی Resource درخواست شده است.
headers اطلاعاتی درباره Request منتقل می‌کنند.
body داده‌ای است که همراه Request ارسال می‌شود.

پس Fetch API اجازه می‌دهد ساختار HTTP Request را از داخل JavaScript تعریف کنیم:

Request
├── URL
├── Method
├── Headers
└── Body

این همان Requestی است که در فصل قبل در سطح HTTP شناختیم.

اکنون Request به سمت Server ارسال می‌شود.

اما Application برای ادامه کار به نتیجه آن نیاز دارد.

Response؛ چیزی که از Server دریافت می‌کنیم

پس از دریافت Request، Server آن را پردازش می‌کند و Response می‌فرستد.

جریان اکنون چنین است:

Application
↓
Request
↓
Server
↓
Response

در JavaScript می‌توانیم Response را دریافت کنیم:

fetch('/api/recipes/42')
.then(response => {
console.log(response);
});

اما response خود Recipe نیست.

این نکته بسیار مهم است.

Response نماینده HTTP Response است.

همان‌طور که در فصل قبل دیدیم، Response شامل اطلاعات مختلفی است:

Response
├── Status
├── Headers
└── Body

برای مثال Server ممکن است چنین پاسخی ایجاد کند:

HTTP/1.1 200 OK
Content-Type: application/json

در اینجا:

Status مشخص می‌کند Request از دید HTTP چه نتیجه‌ای داشته است.
Headers اطلاعاتی درباره Response ارائه می‌کنند.
Body محتوای اصلی Response را حمل می‌کند.

بنابراین وقتی fetch() یک Response در اختیار ما قرار می‌دهد، هنوز به Application Data نرسیده‌ایم.

ابتدا باید Response را بررسی کنیم و سپس در صورت مناسب بودن، Body را بخوانیم.

Response Body

فرض کنید Server برای Recipe شماره 42 چنین Bodyای ارسال کند:

{
"id": 42,
"title": "Pasta",
"publisher": "Omid Kitchen"
}

این داده در Response Body قرار دارد.

اما Response و Body یک مفهوم نیستند.

مدل ذهنی صحیح:

Response
↓
Body
↓
Data

اگر Application فقط خود Response را بررسی کند، هنوز به داده Recipe دسترسی ندارد.

برای رسیدن به Data باید Body را بخوانیم.

از آنجا که API ما داده را به شکل JSON ارسال کرده است، باید JSON را به یک JavaScript Value تبدیل کنیم.

اینجاست که response.json() وارد می‌شود.

JSON و response.json()

JSON یکی از Formatهای رایج برای انتقال Data میان Client و Server است.

فرض کنید Body چنین مقداری داشته باشد:

{
"id": 42,
"title": "Pasta",
"publisher": "Omid Kitchen"
}

Application نمی‌خواهد با متن خام JSON کار کند.

می‌خواهد بتواند با داده به شکل JavaScript Value کار کند:

recipe.title

Fetch API برای خواندن JSON Body یک Method در اختیار ما قرار می‌دهد:

response.json()

مثال:

fetch('/api/recipes/42')
.then(response => response.json())
.then(recipe => {
console.log(recipe.title);
});

اکنون مسیر ارتباط کامل‌تر شده است:

fetch()
↓
Promise
↓
Response
↓
Body
↓
JSON
↓
JavaScript Data

اما اینجا یک نکته فنی مهم وجود دارد.

response.json() مستقیماً JavaScript Object را در اختیار ما قرار نمی‌دهد.

چرا response.json() هم Promise است؟

Body باید خوانده و پردازش شود.

به همین دلیل:

response.json()

خودش یک Promise برمی‌گرداند.

پس این کد:

const data = response.json();

به این معنی نیست که data یک Recipe Object است.

data در این مرحله Promise است.

جریان واقعی:

Response
↓
response.json()
↓
Promise
↓
JavaScript Data

به همین دلیل می‌توانیم مرحله بعد را دریافت کنیم:

fetch('/api/recipes/42')
.then(response => response.json())
.then(recipe => {
console.log(recipe.title);
});

Callback اول با Response کار می‌کند:

response => response.json()

و Callback دوم با Data کار می‌کند:

recipe => {
console.log(recipe.title);
}

بنابراین دو مرحله متفاوت داریم:

HTTP Response
↓
Read Body
↓
Parse JSON
↓
Application Data

این تفکیک یکی از مهم‌ترین بخش‌های مدل ذهنی Fetch API است.

HTTP Status؛ آیا هر Response موفق است؟

اکنون فرض کنیم Resource موردنظر وجود ندارد:

fetch('/api/recipes/999');

Server ممکن است پاسخ دهد:

404 Not Found

ممکن است انتظار داشته باشیم fetch() این وضعیت را به‌صورت Error تشخیص دهد.

اما چنین برداشتی دقیق نیست.

fetch() در صورت دریافت HTTP Response، حتی اگر Status آن 404 یا 500 باشد، Response را در اختیار Application قرار می‌دهد.

بنابراین:

Request
↓
Server
↓
404 Response
↓
Response Object

اینجا یک نکته بسیار مهم شکل می‌گیرد:

دریافت Response لزوماً به معنی موفق بودن HTTP Request نیست.

این همان جایی است که Status Codes فصل قبل وارد رفتار Fetch API می‌شوند.

Application باید Response را بررسی کند تا مشخص شود HTTP Operation موفق بوده یا خیر.

response.ok و response.status

Fetch API اطلاعات Status را از طریق Properties مربوط به Response در اختیار Application قرار می‌دهد.

یکی از مهم‌ترین آن‌ها:

response.ok

است.

اگر Status در محدوده موفق HTTP باشد، response.ok مقدار true دارد؛ در غیر این صورت false است.

برای مثال:

fetch('/api/recipes/999')
.then(response => {
if (!response.ok) {
throw new Error(`HTTP Error: ${response.status}`);
}

    return response.json();
})
.then(recipe => {
console.log(recipe);
});

در اینجا ابتدا Response بررسی می‌شود.

اگر HTTP Status موفق نباشد، Application وارد مرحله پردازش Data نمی‌شود.

اگر موفق باشد، Body خوانده می‌شود.

جریان اکنون چنین است:

Response
↓
HTTP Status Check
↓
Successful?
├── Yes → Read Body
└── No  → Error

برای دسترسی به Status Code دقیق نیز می‌توانیم از:

response.status

استفاده کنیم.

برای مثال:

200
404
500

بنابراین:

response.ok
→ آیا HTTP Response موفق است؟

response.status
→ Status Code دقیق چیست؟

این دو Property نقش متفاوتی دارند، اما هر دو به تحلیل وضعیت HTTP Response کمک می‌کنند.

HTTP Error با Network Error یکی نیست

در این مرحله باید دو نوع Failure را از یکدیگر جدا کنیم.

فرض کنید Server پاسخ زیر را ارسال کند:

404 Not Found

در این حالت:

Request به Server رسیده است.
Server Response ایجاد کرده است.
Response دارای Status 404 است.

این یک HTTP Error است.

مدل آن:

Request
↓
Server
↓
Response
↓
404

اما ممکن است ارتباط شبکه به شکلی شکست بخورد که Response قابل دریافت نباشد.

در این حالت:

Request
↓
Communication Failure
↓
No usable Response

این وضعیت با HTTP Error متفاوت است.

بنابراین:

HTTP Error
≠
Network Error

این تفاوت در Fetch API اهمیت زیادی دارد.

برای Statusهایی مانند 404 و 500، fetch() به‌صورت خودکار Promise را Reject نمی‌کند؛ زیرا یک HTTP Response دریافت شده است.

Application باید Status را بررسی کند.

در مقابل، بعضی خطاهای ارتباطی باعث Reject شدن Promise مربوط به Fetch می‌شوند.

جزئیات کامل Promise Rejection را در فصل بعد بررسی خواهیم کرد.

از Fetch به API Integration

تا اینجا مسیر اصلی Fetch API را ساختیم:

HTTP
↓
fetch
↓
Promise
↓
Request
↓
Response
↓
Body
↓
JSON
↓
HTTP Errors

اما Application واقعی معمولاً فقط یک Request ندارد.

فرض کنید یک Recipe Application داریم.

کاربر ابتدا Recipe را انتخاب می‌کند و Application باید اطلاعات آن را از API دریافت کند.

می‌توانیم ارتباط با API را در یک Function متمرکز کنیم:

function getRecipe(id) {
return fetch(`/api/recipes/${id}`)
.then(response => {
if (!response.ok) {
throw new Error(
`Recipe request failed: ${response.status}`
);
}

      return response.json();
    });
}

اکنون بخش دیگری از Application فقط Data را مصرف می‌کند:

getRecipe(42)
.then(recipe => {
console.log(recipe.title);
});

در اینجا getRecipe() جزئیات HTTP Communication را در خود متمرکز کرده است:

getRecipe()
↓
fetch()
↓
Request
↓
Response
↓
Status Check
↓
JSON
↓
Recipe Data

Consumer دیگر لازم نیست بداند Request چگونه ساخته شده، Response چگونه بررسی شده یا JSON چگونه خوانده شده است.

این همان نقطه‌ای است که Fetch API از یک Browser API ساده به بخشی از Data Flow یک Application واقعی تبدیل می‌شود.

Fetch API و AJAX

در فصل قبل با AJAX و XMLHttpRequest آشنا شدیم.

AJAX یک الگوی ارتباطی بود که امکان تبادل Data با Server را بدون Reload کامل صفحه فراهم می‌کرد.

XMLHttpRequest یکی از APIهای Browser برای ایجاد چنین HTTP Communicationای بود.

Fetch API همان مسئله کلی را با Interface مدرن‌تری در اختیار JavaScript قرار می‌دهد.

پس مسیر تکامل مفهومی ما چنین است:

HTTP Communication
↓
AJAX
↓
XMLHttpRequest
↓
Modern Fetch

اما نباید تصور کنیم Fetch یک Protocol جدید است.

HTTP همچنان مسئول تعریف ارتباط میان Client و Server است.

چیزی که تغییر کرده، API مورد استفاده JavaScript برای ایجاد و مدیریت این ارتباط است.

بنابراین:

Fetch API یک Interface مدرن برای HTTP Communication در Browser است.

Fetch API به‌عنوان مرز ارتباطی Application

اکنون می‌توانیم جایگاه Fetch API را در Application بهتر ببینیم.

Application با یک Resource خارجی ارتباط برقرار می‌کند:

Application
↓
Fetch API
↓
HTTP
↓
Server

Response در جهت مخالف حرکت می‌کند:

Server
↓
HTTP Response
↓
Fetch API
↓
Application

و در نهایت:

Response
↓
Body
↓
JSON
↓
Application Data

پس Fetch API را بهتر است فقط «تابعی برای گرفتن اطلاعات» ندانیم.

Fetch API Interfaceای است که مرز میان Application و HTTP Resources را مدیریت می‌کند.

در Application واقعی، این مرز می‌تواند در Functionها یا API Layerهای مشخص قرار گیرد:

UI / Application Logic
↓
API Function
↓
fetch()
↓
HTTP
↓
Server

این جداسازی باعث می‌شود بخش‌های مختلف Application به‌جای درگیر شدن با جزئیات HTTP، روی Data موردنیاز خود تمرکز کنند.

یک مثال کامل

اکنون تمام جریان را در یک مثال کوچک کنار هم قرار دهیم:

function getRecipe(id) {
return fetch(`/api/recipes/${id}`)
.then(response => {
if (!response.ok) {
throw new Error(
`Request failed: ${response.status}`
);
}

      return response.json();
    });
}

getRecipe(42)
.then(recipe => {
console.log(recipe.title);
});

بیایید این کد را از ابتدا دنبال کنیم.

ابتدا:

fetch(`/api/recipes/${id}`)

یک HTTP Request را آغاز می‌کند.

چون نتیجه هنوز آماده نیست، fetch() یک Promise برمی‌گرداند.

پس از دریافت Response:

response

در اختیار Application قرار می‌گیرد.

Response هنوز Recipe نیست.

ابتدا Status بررسی می‌شود:

if (!response.ok)

اگر HTTP Request ناموفق باشد، Application آن را به‌عنوان Error مدیریت می‌کند.

اگر موفق باشد، Body خوانده می‌شود:

response.json()

این مرحله نیز یک Promise ایجاد می‌کند و در نهایت JavaScript Data در اختیار Application قرار می‌گیرد:

recipe

بنابراین کل جریان:

Application
↓
getRecipe()
↓
fetch()
↓
Promise
↓
HTTP Request
↓
Server
↓
Response
↓
Status Check
↓
Body
↓
JSON
↓
JavaScript Data
↓
Application

این همان مدل ذهنی اصلی فصل است.

Best Practices
Response را با Data یکی ندانید

این دو مرحله متفاوت‌اند:

Response
↓
Body
↓
Data

fetch() ابتدا Response را در اختیار Application قرار می‌دهد، نه Data نهایی.

قبل از پردازش Body، Status را بررسی کنید

به دریافت Response اکتفا نکنید.

در صورت نیاز ابتدا:

if (!response.ok) {
throw new Error(`HTTP Error: ${response.status}`);
}

را بررسی کنید و سپس Body را پردازش کنید.

response.json() را با Object اشتباه نگیرید

این:

response.json()

نتیجه نهایی Data نیست.

این Method Promise مربوط به خواندن و Parse کردن JSON Body را برمی‌گرداند.

HTTP Error و Network Error را جدا نگه دارید

این دو Failure یکسان نیستند:

HTTP Error
→ Response دریافت شده
→ Status ناموفق

Network Error
→ ارتباط با مشکل مواجه شده
→ ممکن است Response دریافت نشده باشد
API Communication را از Application Logic جدا کنید

ارتباط با API را می‌توان در Function یا Layer مشخصی قرار داد تا بخش‌های دیگر Application فقط Data را مصرف کنند.

Common Mistakes
تصور اینکه fetch() مستقیماً Data را برمی‌گرداند
const recipe = fetch('/api/recipes/42');

recipe در اینجا Recipe نیست؛ Promise است.

تصور اینکه Response همان JSON است
fetch('/api/recipes/42')
.then(response => {
console.log(response);
});

response یک Response Object است.

JSON معمولاً در Body آن قرار دارد.

تصور اینکه response.json() مستقیماً Object می‌دهد
const data = response.json();

data در این مرحله Promise است.

تصور اینکه 404 باعث Reject شدن fetch() می‌شود

404 یک HTTP Response است.

بنابراین باید Status بررسی شود:

if (!response.ok) {
throw new Error(`HTTP Error: ${response.status}`);
}
یکی دانستن HTTP Error و Network Error
HTTP Error
→ Response دریافت شده
→ Status ناموفق

Network Error
→ ارتباط با Resource با مشکل مواجه شده
→ ممکن است Response دریافت نشده باشد
Summary

Fetch API یک Web API برای برقراری HTTP Communication در Browser است.

HTTP مشخص می‌کند Client و Server چگونه ارتباط برقرار کنند و Fetch API امکان استفاده از این ارتباط را از داخل JavaScript فراهم می‌کند.

fetch() عملیات HTTP را آغاز می‌کند و نتیجه آینده آن را به‌صورت Promise در اختیار Application قرار می‌دهد.

پس از دریافت Response، Application ابتدا با یک Response Object مواجه است، نه با Data نهایی.

Response شامل Status، Headers و Body است.

اگر Body شامل JSON باشد، می‌توان از:

response.json()

برای خواندن آن استفاده کرد.

در نتیجه مسیر اصلی Data چنین است:

fetch()
↓
Promise
↓
Response
↓
Body
↓
JSON
↓
JavaScript Data

اما دریافت Response به معنی موفقیت HTTP نیست.

Statusهایی مانند 404 و 500 نیز می‌توانند به‌صورت Response دریافت شوند.

بنابراین Application باید Status را بررسی کند:

response.ok

یا:

response.status

در نهایت Fetch API مرز ارتباطی Application با HTTP Resources را شکل می‌دهد:

Application
↓
Fetch API
↓
HTTP
↓
Server
↓
HTTP Response
↓
Application Data

و در این میان AJAX و XMLHttpRequest زمینه تاریخی و فنی رسیدن به API مدرن Fetch را تشکیل می‌دهند.

Key Takeaways
Fetch API رابط مدرن JavaScript برای HTTP Communication در Browser است.
HTTP Protocol و Fetch API یک مفهوم نیستند.
fetch() یک HTTP Request را آغاز می‌کند.
fetch() نتیجه آینده عملیات را به‌صورت Promise برمی‌گرداند.
نتیجه Fetch یک Response است، نه Data نهایی.
Response شامل Status، Headers و Body است.
JSON معمولاً در Body قرار دارد.
response.json() Body JSON را به JavaScript Value تبدیل می‌کند.
response.json() نیز Promise برمی‌گرداند.
دریافت Response لزوماً به معنی موفقیت HTTP نیست.
404 و 500 باعث Reject شدن خودکار fetch() نمی‌شوند.
response.ok برای بررسی موفقیت HTTP مفید است.
response.status Status Code دقیق را مشخص می‌کند.
HTTP Error و Network Error یک مفهوم نیستند.
Fetch API می‌تواند مرز مشخصی میان API Communication و Application Logic ایجاد کند.
Technical Interview
Junior
Fetch API چیست؟

Fetch API یک Web API در Browser است که به JavaScript امکان ایجاد HTTP Request و دریافت HTTP Response را می‌دهد.

fetch() چه چیزی برمی‌گرداند؟

یک Promise که در صورت دریافت Response، نتیجه آن به یک Response Object منتهی می‌شود.

آیا Response همان JSON است؟

خیر.

Response نماینده HTTP Response است و JSON معمولاً در Body آن قرار دارد.

چگونه JSON را از Response می‌خوانیم؟

با:

response.json()

که Promise مربوط به JavaScript Value حاصل از خواندن و Parse کردن JSON Body را برمی‌گرداند.

Mid-Level
چرا fetch() مستقیماً Data را برنمی‌گرداند؟

زیرا HTTP Communication یک عملیات فوری نیست. fetch() نتیجه آینده عملیات را به‌صورت Promise در اختیار Application قرار می‌دهد. پس از دریافت Response، Body باید خوانده شود و در صورت JSON بودن Parse شود.

fetch()
↓
Promise
↓
Response
↓
response.json()
↓
Data
چرا باید response.ok را بررسی کنیم؟

زیرا دریافت Response لزوماً به معنی موفقیت HTTP نیست.

برای مثال 404 و 500 می‌توانند Responseهای معتبر HTTP باشند و Promise مربوط به fetch() را خودکار Reject نکنند.

تفاوت HTTP Error و Network Error چیست؟

در HTTP Error، ارتباط با Server برقرار شده و یک Response با Status ناموفق دریافت شده است.

در Network Error، ارتباط ممکن است اصلاً به دریافت Response منتهی نشود.

Senior
چرا موفقیت Promise مربوط به fetch() با موفقیت HTTP یکی نیست؟

زیرا fetch() می‌تواند برای HTTP Responseهای ناموفق مانند 404 یا 500 نیز یک Response معتبر در اختیار Application قرار دهد.

بنابراین دو سؤال متفاوت داریم:

آیا Response دریافت شد؟
↓
آیا HTTP Status موفق است؟

Application باید این دو را از یکدیگر تفکیک کند.

چرا Response و Application Data باید از یکدیگر جدا شوند؟

Response متعلق به لایه HTTP است، در حالی که Application Data نتیجه پردازش Body برای استفاده در منطق Application است.

HTTP
↓
Response
↓
Body
↓
Parsing
↓
Application Data

این تفکیک Boundary میان Communication Layer و Application Logic را روشن نگه می‌دارد.

Fetch API چه نقشی در Application دارد؟

Fetch API Interface ارتباط Application با HTTP Resources را فراهم می‌کند. Application Request را ایجاد می‌کند، Response را دریافت می‌کند، Status را بررسی می‌کند و Body را به Data قابل استفاده تبدیل می‌کند.

Golden Answers
Junior — Golden Answer

Fetch API چیست؟

Fetch API یک Web API برای ایجاد HTTP Request و دریافت HTTP Response در Browser است. fetch() یک Promise برمی‌گرداند که نتیجه آن در صورت دریافت Response، به‌صورت یک Response Object در اختیار Application قرار می‌گیرد.

Mid-Level — Golden Answer

مسیر دریافت Data با Fetch چیست؟

ابتدا fetch() یک HTTP Request را آغاز می‌کند و Promise نتیجه آینده را برمی‌گرداند. پس از دریافت Response، Application باید وضعیت HTTP را بررسی کند. سپس Body خوانده می‌شود و اگر JSON باشد، با response.json() به JavaScript Value تبدیل می‌شود.

fetch()
↓
Promise
↓
Response
↓
Status Check
↓
Body
↓
JSON
↓
Data
Senior — Golden Answer

مهم‌ترین نکته مهندسی هنگام استفاده از Fetch API چیست؟

نباید دریافت Response را با موفقیت HTTP یکی دانست. fetch() می‌تواند برای Statusهایی مانند 404 و 500 نیز Response دریافت کند. بنابراین HTTP Status باید جداگانه بررسی شود و سپس Body پردازش شود. این تفکیک باعث می‌شود HTTP Communication، Application Data و Application Logic از یکدیگر جدا باقی بمانند.

Conclusion

در فصل قبل، Browser، Client، Server، HTTP Request و Response را به‌عنوان اجزای یک جریان ارتباطی شناختیم.

اکنون همان جریان را از داخل JavaScript دنبال کردیم.

Application برای ایجاد HTTP Communication از Fetch API استفاده می‌کند:

Application
↓
fetch()
↓
Request
↓
Server

نتیجه آینده این ارتباط به‌صورت Promise در اختیار Application قرار می‌گیرد.

پس از دریافت Response، هنوز به Data نهایی نرسیده‌ایم:

Response
↓
Body
↓
JSON
↓
Application Data

در همین مسیر مشخص شد که دریافت Response لزوماً به معنی موفقیت HTTP نیست.

ممکن است Server یک 404 یا 500 برگرداند و همچنان یک Response معتبر دریافت شود.

بنابراین:

Response Received
≠
HTTP Success

Application باید Status را بررسی کند و سپس Body را پردازش کند.

اکنون کل جریان را می‌توان در یک مدل واحد دید:

Application
↓
fetch()
↓
Promise
↓
Request
↓
Server
↓
Response
↓
Status Check
↓
Body
↓
JSON
↓
Application Data

اما در این فصل Promise را فقط به اندازه‌ای دیدیم که رفتار fetch() قابل فهم باشد.

اکنون یک سؤال طبیعی باقی می‌ماند:

Promise دقیقاً چیست و چگونه نتیجه یک عملیات Async را به‌صورت قابل ترکیب مدیریت می‌کند؟

این سؤال، نقطه شروع فصل بعد است.