Chapter 60 — AJAX and HTTP Communication
اهداف فصل

پس از مطالعه این فصل، خواننده باید بتواند:

Client و Server را به‌عنوان دو نقش در یک ارتباط Web توضیح دهد.
جریان HTTP Request و HTTP Response را تحلیل کند.
نقش HTTP را در ارتباط میان Browser و Server توضیح دهد.
مفهوم Status در یک HTTP Response را درک کند.
نقش JSON را در انتقال داده‌های ساختاریافته توضیح دهد.
مفهوم AJAX را از HTTP و Browser APIها تفکیک کند.
نقش XMLHttpRequest را در برقراری ارتباط با Server توضیح دهد.
جایگاه Fetch API را بدون ورود به جزئیات فصل بعد، در این مسیر تشخیص دهد.
Core Question

Browser چگونه با Server ارتباط برقرار می‌کند؟

مقدمه

تا اینجا بیشتر مسئله‌هایی که در JavaScript بررسی کرده‌ایم، در محدوده خود Application یا Browser قرار داشتند.

داده‌ای داشتیم و آن را با JavaScript پردازش می‌کردیم.

Object می‌ساختیم، Array را تغییر می‌دادیم، Function اجرا می‌کردیم یا با DOM تعامل داشتیم.

اما یک Application واقعی همیشه تمام داده موردنیاز خود را در اختیار ندارد.

فرض کنید کاربر در یک Recipe Application عبارت زیر را جستجو می‌کند:

pasta

اطلاعات مربوط به Recipeهای موردنظر ممکن است در Browser وجود نداشته باشد.

Application باید بتواند این اطلاعات را از یک Server دریافت کند.

پس برای اولین بار با مسئله‌ای روبه‌رو می‌شویم که در آن JavaScript باید با سیستمی خارج از محیط فعلی خود ارتباط برقرار کند.

مدل ساده مسئله چنین است:

Browser
↓
اطلاعات موردنیاز
↓
Server

اما Browser چگونه باید به Server بگوید چه چیزی می‌خواهد؟

و Server چگونه باید نتیجه را برگرداند؟

برای پاسخ به این سؤال، ابتدا باید دو طرف این ارتباط را مشخص کنیم.

وقتی Browser دیگر به‌تنهایی کافی نیست

در یک Web Application، Browser معمولاً در نقش Client قرار دارد.

Client سیستمی است که برای دریافت یک Resource یا انجام یک عملیات، درخواست ارسال می‌کند.

در طرف دیگر، Server قرار دارد که درخواست را دریافت و پردازش می‌کند.

بنابراین ارتباط اولیه بسیار ساده است:

Client
↓
Server

اما این ارتباط هنوز کامل نیست.

Client باید چیزی برای Server ارسال کند.

Server نیز باید چیزی را برگرداند.

در نتیجه:

Client
↓
Request
↓
Server
↓
Response
↓
Client

اکنون مسئله روشن‌تر شده است.

Browser نمی‌تواند صرفاً بگوید:

«من اطلاعات Recipe را می‌خواهم.»

باید این درخواست را در قالبی مشخص ارسال کند تا Server بتواند آن را بفهمد.

این نیاز ما را به مفهوم بعدی می‌رساند: Request.

Client باید خواسته خود را بیان کند

وقتی Client به یک Resource نیاز دارد، باید یک Request ایجاد کند.

Request را می‌توان یک پیام از طرف Client به Server در نظر گرفت که مشخص می‌کند Client چه چیزی می‌خواهد.

مثلاً Application ممکن است بخواهد اطلاعات Recipe شماره 123 را دریافت کند.

در سطح مفهومی:

Client
↓
Request for Recipe 123
↓
Server

Request در یک ارتباط واقعی اطلاعات بیشتری دارد، اما در این مرحله مهم‌ترین نکته این است که Client باید درخواست خود را به شکلی ساختاریافته ارسال کند.

برای مثال:

GET /recipes/123

در اینجا Client مشخص کرده است که Resource مربوط به مسیر /recipes/123 را می‌خواهد.

اما هنوز یک مشکل وجود دارد.

Client و Server باید بر سر قالب این ارتباط توافق داشته باشند.

اگر هر Client درخواست را به شکل متفاوتی ارسال کند، Server نمی‌تواند یک روش مشخص برای دریافت و پردازش آن داشته باشد.

پس به یک قرارداد مشترک نیاز داریم.

Request به یک قرارداد نیاز دارد

این قرارداد در Web معمولاً HTTP است.

HTTP مخفف Hypertext Transfer Protocol است و پروتکلی برای ارتباط میان Client و Server محسوب می‌شود.

پس اکنون ارتباط را می‌توان دقیق‌تر دید:

Client
↓
HTTP Request
↓
Server

HTTP مشخص می‌کند Request و Response چگونه در یک ارتباط Web ساختاربندی شوند.

به همین دلیل HTTP را نباید با JavaScript یکی بدانیم.

JavaScript زبان برنامه‌نویسی است.

HTTP پروتکل ارتباطی است.

Browser نیز محیطی است که امکاناتی برای استفاده از این ارتباط در اختیار JavaScript قرار می‌دهد.

پس:

JavaScript
↓
Application Logic

HTTP
↓
Communication Protocol

Browser
↓
Host Environment

این تفکیک اهمیت زیادی دارد.

وقتی بعداً با APIهایی مانند XMLHttpRequest یا fetch() کار می‌کنیم، باید بدانیم این APIها خود HTTP نیستند.

آن‌ها ابزارهایی هستند که Browser برای برقراری ارتباط HTTP در اختیار JavaScript قرار می‌دهد.

اما حالا Request به Server رسیده است.

Server باید با آن چه کار کند؟

Server باید به Request پاسخ دهد

Server پس از دریافت Request آن را پردازش می‌کند.

برای مثال، اگر Request مربوط به دریافت یک Recipe باشد، Server می‌تواند Resource موردنظر را پیدا کند و نتیجه را آماده کند.

جریان اکنون چنین شده است:

Client
↓
Request
↓
HTTP
↓
Server
↓
?

علامت سؤال نشان می‌دهد که هنوز بخشی از ارتباط را نمی‌دانیم.

Server نمی‌تواند فقط Request را دریافت کند و ارتباط را تمام کند.

Client باید نتیجه را دریافت کند.

بنابراین Server باید یک Response ایجاد کند.

Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Client

Response پاسخ Server به Request است.

اما Response فقط به این معنا نیست که Server مقداری داده را برگردانده است.

Client باید بتواند بفهمد Request چه نتیجه‌ای داشته است.

آیا Resource پیدا شده است؟

آیا Request معتبر بوده است؟

آیا Server توانسته آن را پردازش کند؟

پس Response به اطلاعاتی درباره وضعیت Request نیز نیاز دارد.

Response فقط داده نیست

فرض کنید Client درخواست یک Recipe را ارسال کرده است.

اگر Server بتواند آن را پیدا کند، یک وضعیت موفقیت‌آمیز به Client اعلام می‌شود.

اما اگر Recipe موردنظر وجود نداشته باشد، نتیجه متفاوت خواهد بود.

بنابراین Client باید بتواند بین این دو وضعیت تفاوت بگذارد.

برای همین HTTP Response دارای Status Code است.

برای مثال:

200

معمولاً نشان‌دهنده موفقیت Request است.

در مقابل:

404

نشان می‌دهد Resource موردنظر پیدا نشده است.

و:

500

نشان‌دهنده وقوع یک خطا در سمت Server است.

پس Response را بهتر است چنین تصور کنیم:

HTTP Response
├── Status
└── Data

Status به Client می‌گوید Request چه وضعیتی داشته است.

Data، در صورت وجود، اطلاعاتی را در اختیار Application قرار می‌دهد که می‌تواند برای ادامه کار استفاده شود.

اکنون یک سؤال طبیعی ایجاد می‌شود:

اگر Server بخواهد داده واقعی Application را برای Client ارسال کند، این داده با چه ساختاری منتقل می‌شود؟

داده باید قابل انتقال باشد

فرض کنید Server اطلاعات Recipe را پیدا کرده است.

اطلاعات می‌تواند شامل مواردی مانند این باشد:

title
publisher
rating
ingredients

Client باید این داده را به شکلی دریافت کند که بتواند آن را پردازش کند.

یکی از قالب‌های رایج برای چنین داده‌ای JSON است.

برای مثال، Server می‌تواند داده‌ای شبیه این برگرداند:

{
"title": "Pasta",
"publisher": "Jonas",
"rating": 4.8
}

JSON مخفف JavaScript Object Notation است و قالبی متنی برای نمایش داده‌های ساختاریافته محسوب می‌شود.

اما یک نکته مهم وجود دارد.

JSON خود HTTP نیست.

در این ارتباط:

HTTP
↓
قواعد ارتباط

JSON
↓
قالب نمایش داده

HTTP مشخص می‌کند Client و Server چگونه با یکدیگر ارتباط برقرار کنند.

JSON می‌تواند قالب داده‌ای باشد که در این ارتباط منتقل می‌شود.

بنابراین:

Client
↓
HTTP Request
↓
Server
↓
HTTP Response
├── Status
└── JSON Data

اکنون تقریباً تمام اجزای ارتباط را داریم.

اما هنوز یک مشکل مهم باقی مانده است.

وقتی دریافت داده نباید به معنی Reload صفحه باشد

فرض کنید کاربر در یک Recipe Application عبارت pasta را جستجو می‌کند.

Application باید اطلاعات جدید را از Server دریافت کند.

یک روش ساده این است که Browser یک Request کامل برای یک Document جدید ارسال کند:

User Action
↓
Request
↓
Server
↓
New Document
↓
Page Reload

این روش برای بعضی سناریوها مناسب است.

اما تصور کنید کاربر فقط می‌خواهد نتایج جستجو در یک بخش از صفحه تغییر کند.

در این حالت، دریافت دوباره کل صفحه ضروری نیست.

Application فقط به داده جدید نیاز دارد.

پس سؤال جدیدی ایجاد می‌شود:

آیا JavaScript می‌تواند با Server ارتباط برقرار کند و فقط داده موردنیاز را دریافت کند، بدون اینکه کل صفحه Reload شود؟

این همان نیازی است که مفهوم AJAX به آن پاسخ می‌دهد.

AJAX؛ ارتباط با Server بدون Reload کامل صفحه

AJAX مخفف:

Asynchronous JavaScript and XML

است.

اما برای درک AJAX نباید روی کلمه XML تمرکز کنیم.

ایده اصلی AJAX این است که JavaScript بتواند در Browser با Server ارتباط برقرار کند، داده دریافت یا ارسال کند و نتیجه را در Application استفاده کند، بدون اینکه برای هر ارتباط مجبور باشیم کل صفحه را دوباره بارگذاری کنیم.

جریان Application اکنون می‌تواند چنین باشد:

User Action
↓
JavaScript
↓
HTTP Request
↓
Server
↓
HTTP Response
↓
JavaScript
↓
Update UI

مثلاً در یک Search:

User searches "pasta"
↓
JavaScript sends Request
↓
Server returns data
↓
JavaScript receives Response
↓
Search Results update

در این مدل، Document موجود همچنان در Browser باقی می‌ماند.

JavaScript فقط داده جدید را دریافت می‌کند و بخش موردنیاز Application را تغییر می‌دهد.

به همین دلیل AJAX را نباید یک Protocol جدید یا یک Syntax جدید JavaScript بدانیم.

AJAX یک رویکرد ارتباطی است.

اما JavaScript برای اجرای این رویکرد به یک API نیاز دارد.

AJAX به یک Browser API نیاز دارد

تا اینجا می‌دانیم چه می‌خواهیم:

JavaScript
↓
HTTP Request
↓
Server
↓
HTTP Response

اما JavaScript به‌تنهایی یک API مخصوص HTTP نیست.

Browser باید ابزاری در اختیار JavaScript قرار دهد تا بتواند چنین Requestهایی را ایجاد کند.

یکی از APIهای مهمی که برای این منظور در Web Platform وجود دارد:

XMLHttpRequest

است.

این API معمولاً با نام کوتاه XHR نیز شناخته می‌شود.

بنابراین اکنون رابطه مفاهیم روشن‌تر شده است:

AJAX
↓
ارتباط JavaScript با Server
↓
XMLHttpRequest
↓
HTTP Request / Response

در نتیجه:

AJAX و XMLHttpRequest یک مفهوم نیستند.

AJAX یک تکنیک یا الگوی ارتباطی است.

XMLHttpRequest یک Browser API است که می‌تواند برای برقراری چنین ارتباطی استفاده شود.

XMLHttpRequest چگونه وارد این جریان می‌شود؟

فرض کنید می‌خواهیم اطلاعات Recipeها را از Server دریافت کنیم.

با XMLHttpRequest می‌توان یک Request ایجاد کرد:

const xhr = new XMLHttpRequest();

xhr.open('GET', '/api/recipes');

xhr.send();

در اینجا هنوز هدف ما حفظ کردن این Syntax نیست.

هدف، دیدن جایگاه XHR در کل جریان است.

ابتدا یک Object از XMLHttpRequest ایجاد می‌کنیم:

const xhr = new XMLHttpRequest();

سپس نوع Request و Resource موردنظر را مشخص می‌کنیم:

xhr.open('GET', '/api/recipes');

و در نهایت Request را ارسال می‌کنیم:

xhr.send();

پس:

XMLHttpRequest
↓
Configure Request
↓
Send Request
↓
Server
↓
Response

این همان ارتباطی است که در ابتدای فصل به‌صورت مفهومی ساختیم.

اکنون Browser API مشخصی داریم که می‌تواند آن را اجرا کند.

اما یک ویژگی مهم این ارتباط را نباید فراموش کنیم.

Request ممکن است فوراً تمام نشود

وقتی JavaScript Request را ارسال می‌کند، Server ممکن است بلافاصله Response را برنگرداند.

Network Communication زمان می‌برد.

مثلاً:

JavaScript
↓
Request
↓
Network
↓
Server
↓
Processing
↓
Response

بنابراین Application نباید تصور کند که Response در همان لحظه‌ای که Request ارسال شد، آماده است.

این همان نقطه‌ای است که مفهوم Asynchronous اهمیت پیدا می‌کند.

در فصل‌های قبل دیدیم که عملیات زمان‌بر نباید الزاماً اجرای Application را متوقف کنند.

اکنون یک نمونه واقعی از همان مسئله داریم:

Start Request
↓
Wait for Response
↓
Continue Application
↓
Response Arrives
↓
Process Result

پس ارتباط با Server یکی از مهم‌ترین نمونه‌های واقعی برای درک Asynchronous Programming در Browser است.

در این فصل لازم نیست سازوکار Event Loop را دوباره بررسی کنیم؛ آن مدل قبلاً ساخته شده است.

اکنون فقط باید بدانیم که Network Request یک عملیات فوری و همزمان با اجرای معمول JavaScript نیست.

از Request تا Update UI

اکنون می‌توانیم کل مسئله را از ابتدا تا انتها دنبال کنیم.

فرض کنید کاربر در یک Recipe Application جستجو می‌کند.

ابتدا User Interaction رخ می‌دهد:

Search: pasta

JavaScript متوجه این Interaction می‌شود و تصمیم می‌گیرد داده جدیدی از Server دریافت کند.

سپس:

JavaScript
↓
XMLHttpRequest
↓
HTTP Request
↓
Server

Server Request را پردازش می‌کند:

Server
↓
Find Data
↓
HTTP Response

Response به Browser برمی‌گردد:

HTTP Response
├── Status
└── JSON Data

JavaScript نتیجه را دریافت می‌کند و می‌تواند UI را به‌روزرسانی کند:

JSON Data
↓
JavaScript
↓
Update UI

پس کل جریان اکنون چنین است:

User
↓
JavaScript
↓
Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Status + JSON
↓
JavaScript
↓
UI

اگر این ارتباط بدون Reload کامل صفحه انجام شود، در محدوده مفهوم AJAX قرار داریم.

این همان زنجیره‌ای است که باید در ذهن باقی بماند.

AJAX یک لایه روی HTTP است

اکنون می‌توانیم یکی از سوءبرداشت‌های رایج را برطرف کنیم.

گاهی ممکن است تصور شود AJAX جایگزین HTTP شده است.

چنین چیزی درست نیست.

AJAX روی ارتباط HTTP بنا می‌شود.

به بیان ساده:

HTTP
↓
Communication Protocol

AJAX
↓
Technique for using JavaScript
to communicate with Server

همچنین XMLHttpRequest نیز جایگزین HTTP نیست.

XHR یک Browser API است که امکان ایجاد HTTP Request را برای JavaScript فراهم می‌کند.

بنابراین:

HTTP
↑
│
XMLHttpRequest
↑
│
JavaScript

و AJAX مفهومی است که این نوع استفاده از JavaScript برای ارتباط با Server و به‌روزرسانی Application بدون Reload کامل صفحه را توصیف می‌کند.

چرا JSON در این جریان مهم است؟

در این مرحله ممکن است سؤال دیگری ایجاد شود.

AJAX نامی قدیمی دارد که در آن XML دیده می‌شود، اما در مثال ما JSON استفاده شد.

دلیل آن این است که AJAX الزاماً به XML وابسته نیست.

XML یکی از قالب‌هایی بود که در ارتباطات قدیمی Web استفاده می‌شد.

اما Applicationهای JavaScript معمولاً با داده‌های ساختاریافته‌ای مانند Objectها و Arrayها کار می‌کنند و JSON برای نمایش چنین داده‌هایی بسیار مناسب است.

بنابراین یک جریان رایج در Applicationهای Web می‌تواند چنین باشد:

JavaScript
↓
HTTP Request
↓
Server
↓
HTTP Response
↓
JSON
↓
JavaScript
↓
UI

در اینجا:

HTTP مسئول ارتباط است.
JSON مسئول نمایش داده است.
JavaScript مسئول منطق Application است.
Browser محیطی است که این ارتباط را در اختیار Application قرار می‌دهد.

این تفکیک از حفظ کردن نام‌ها مهم‌تر است.

جایگاه XMLHttpRequest در مدل ذهنی

اکنون می‌توانیم جایگاه XHR را دقیق‌تر مشخص کنیم.

اگر مسئله را از بالا ببینیم:

Application Need
↓
Communicate with Server
↓
HTTP
↓
Browser API
↓
XMLHttpRequest

XMLHttpRequest در اینجا خود هدف نیست.

هدف Application، ارتباط با Server است.

HTTP قرارداد این ارتباط است.

XHR یکی از ابزارهای Browser برای انجام آن است.

این نگاه باعث می‌شود اگر API دیگری برای همین نیاز وجود داشته باشد، مدل ذهنی ما از بین نرود.

و دقیقاً همین‌جا به مفهوم بعدی می‌رسیم.

وقتی ابزار ارتباطی تغییر می‌کند

XMLHttpRequest API مهمی برای ارتباط Client و Server در Browser بوده است.

اما کار با آن نیازمند مدیریت جزئیاتی است که در Applicationهای مدرن می‌توان آن‌ها را با API مناسب‌تری مدیریت کرد.

Web Platform در ادامه Fetch API را ارائه کرده است.

در این فصل قرار نیست Fetch را آموزش دهیم.

فقط باید جایگاه آن را در مسیر مفهومی تشخیص دهیم:

HTTP
↓
AJAX Communication
↓
XMLHttpRequest
↓
Modern Fetch API

این تغییر به این معنا نیست که HTTP تغییر کرده است.

HTTP همچنان همان پروتکل ارتباطی است.

آنچه تغییر کرده، APIای است که JavaScript در Browser برای کار با این ارتباط استفاده می‌کند.

پس سؤال فصل بعد دیگر این نیست که:

Browser چگونه با Server ارتباط برقرار می‌کند؟

این مسئله را اکنون می‌دانیم.

سؤال بعدی دقیق‌تر است:

چگونه این ارتباط را با API مدرن Fetch در JavaScript پیاده‌سازی کنیم؟

و این همان مسئله‌ای است که فصل بعد بررسی خواهد کرد.

یک مدل ذهنی نهایی

در پایان این فصل، بهتر است کل Concept Flow را به‌صورت یک زنجیره واحد ببینیم:

Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Status
↓
JSON
↓
AJAX
↓
XMLHttpRequest
↓
Modern Fetch

اما این زنجیره را نباید صرفاً به‌صورت فهرستی از اصطلاحات حفظ کرد.

هر Concept پاسخی به یک نیاز Concept قبلی است:

Client
↓
باید چیزی درخواست کند
↓
Request
↓
به یک قرارداد ارتباطی نیاز دارد
↓
HTTP
↓
باید درخواست را دریافت کند
↓
Server
↓
باید نتیجه را برگرداند
↓
Response
↓
باید وضعیت نتیجه مشخص باشد
↓
Status
↓
داده باید قابل انتقال باشد
↓
JSON
↓
نباید برای هر داده جدید کل صفحه Reload شود
↓
AJAX
↓
JavaScript به Browser API نیاز دارد
↓
XMLHttpRequest
↓
API مدرن‌تر موردنیاز است
↓
Fetch

این زنجیره، مدل ذهنی اصلی این فصل است.

Best Practices
ارتباط را از ابزار جدا کنید

ابتدا مسئله را شناسایی کنید:

Application باید با Server ارتباط برقرار کند.

سپس پروتکل را مشخص کنید:

HTTP.

بعد ابزار Browser را در نظر بگیرید:

XMLHttpRequest یا Fetch.

این ترتیب از حفظ کردن APIها بدون درک مسئله جلوگیری می‌کند.

HTTP را با JSON اشتباه نگیرید

HTTP پروتکل ارتباطی است.

JSON قالب داده است.

HTTP
↓
How communication happens

JSON
↓
How data is represented
AJAX را با XMLHttpRequest یکی ندانید

AJAX یک تکنیک ارتباطی است.

XMLHttpRequest یک Browser API است.

AJAX
↓
Communication Approach

XMLHttpRequest
↓
Browser API
Status را نادیده نگیرید

وجود داده در Response به‌تنهایی کافی نیست.

Application باید بداند Request با چه وضعیتی تمام شده است.

بنابراین Response را فقط به‌عنوان Data در نظر نگیرید.

ابتدا نیاز را درک کنید، سپس API را انتخاب کنید

دانستن Syntax یک API به‌تنهایی نشان‌دهنده درک HTTP Communication نیست.

قبل از استفاده از API باید بدانید:

چه کسی؟
↓
چه چیزی را؟
↓
از کجا؟
↓
با چه پروتکلی؟
↓
با چه نتیجه‌ای؟
اشتباهات رایج
اشتباه اول: AJAX را یک Protocol بدانیم

AJAX Protocol نیست.

AJAX یک تکنیک برای ارتباط JavaScript با Server و دریافت یا ارسال داده بدون Reload کامل صفحه است.

HTTP پروتکل ارتباطی این جریان است.

اشتباه دوم: AJAX را همان XMLHttpRequest بدانیم

این دو یکی نیستند.

XMLHttpRequest یک Browser API است.

AJAX مفهوم گسترده‌تری برای این نوع ارتباط Client و Server است.

اشتباه سوم: JSON را جایگزین HTTP بدانیم

JSON فقط قالبی برای نمایش داده است.

HTTP مسئول ارتباط Request و Response است.

اشتباه چهارم: Response را فقط Data بدانیم

Response علاوه بر داده می‌تواند شامل اطلاعاتی درباره وضعیت Request باشد.

برای همین Status در تحلیل Response اهمیت دارد.

اشتباه پنجم: تصور کنیم AJAX یعنی XML

وجود XML در نام AJAX به این معنا نیست که AJAX الزاماً باید XML دریافت کند.

JSON نیز می‌تواند داده مورد استفاده در چنین ارتباطی باشد.

اشتباه ششم: Fetch را یک Protocol جدید بدانیم

Fetch یک Browser API است.

HTTP همچنان پروتکل ارتباط میان Client و Server است.

Summary

یک Web Application واقعی معمولاً نمی‌تواند تمام داده موردنیاز خود را از ابتدا در Browser داشته باشد.

در چنین شرایطی Browser در نقش Client قرار می‌گیرد و برای دریافت Resource یا انجام یک عملیات، یک Request ایجاد می‌کند.

این Request از طریق HTTP به Server ارسال می‌شود.

Server Request را پردازش کرده و یک Response برمی‌گرداند.

Response می‌تواند شامل Status برای بیان نتیجه Request و داده‌ای مانند JSON باشد.

اما در Applicationهای تعاملی، دریافت داده جدید نباید الزاماً به معنی Reload کامل صفحه باشد.

اینجا مفهوم AJAX وارد می‌شود.

AJAX روشی برای استفاده از JavaScript جهت ارتباط با Server و دریافت یا ارسال داده بدون Reload کامل صفحه است.

یکی از Browser APIهای مهم برای این کار XMLHttpRequest است.

در ادامه مسیر، Fetch API به‌عنوان API مدرن‌تری برای کار با HTTP Resources مطرح می‌شود.

بنابراین هدف اصلی این فصل حفظ چند نام نیست.

هدف، ساختن این مدل ذهنی است:

Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Status + Data
↓
JSON
↓
AJAX
↓
Browser API
↓
XMLHttpRequest
↓
Modern Fetch
Key Takeaways
Browser معمولاً در نقش Client با Server ارتباط برقرار می‌کند.
Client برای دریافت Resource یا انجام عملیات، Request ارسال می‌کند.
HTTP پروتکل ارتباط میان Client و Server است.
Server Request را پردازش کرده و Response برمی‌گرداند.
Response می‌تواند شامل Status و Data باشد.
JSON یکی از قالب‌های رایج برای نمایش داده‌های ساختاریافته است.
AJAX یک تکنیک ارتباطی است، نه یک Protocol.
XMLHttpRequest یک Browser API برای برقراری HTTP Communication است.
AJAX و XMLHttpRequest یک مفهوم نیستند.
Fetch API ابزار مدرن‌تری برای کار با HTTP Resources در Browser است.
مهم‌ترین مدل ذهنی این فصل، جریان کامل Request و Response میان Client و Server است.
Technical Interview
سطح Junior
1. HTTP چیست؟

HTTP یک پروتکل ارتباطی برای انتقال Request و Response میان Client و Server است.

2. Client و Server چه تفاوتی دارند؟

Client درخواست‌کننده Resource یا عملیات است و Server درخواست را دریافت و پردازش می‌کند.

3. Request چیست؟

Request پیامی است که Client برای درخواست یک Resource یا انجام یک عملیات به Server ارسال می‌کند.

4. Response چیست؟

Response پاسخی است که Server در مقابل Request برای Client ارسال می‌کند.

5. JSON چیست؟

JSON یک قالب متنی برای نمایش و انتقال داده‌های ساختاریافته است.

سطح Mid-Level
6. AJAX چیست؟

AJAX روشی برای استفاده از JavaScript جهت ارتباط با Server و دریافت یا ارسال داده بدون Reload کامل صفحه است.

7. آیا AJAX یک API است؟

خیر. AJAX یک تکنیک یا رویکرد ارتباطی است؛ XMLHttpRequest و Fetch API ابزارهای Browser برای انجام چنین ارتباطی هستند.

8. تفاوت HTTP و JSON چیست؟

HTTP پروتکل ارتباط میان Client و Server است، در حالی که JSON قالبی برای نمایش داده‌ای است که می‌تواند در این ارتباط منتقل شود.

9. چرا Status Code در HTTP مهم است؟

زیرا Client باید بتواند وضعیت Request را تشخیص دهد و بر اساس نتیجه آن رفتار مناسب Application را انتخاب کند.

10. XMLHttpRequest چیست؟

XMLHttpRequest یک Browser API است که JavaScript می‌تواند از آن برای ایجاد و ارسال HTTP Request و دریافت Response استفاده کند.

سطح Senior
11. تفاوت AJAX و XMLHttpRequest چیست؟

AJAX یک الگوی ارتباطی برای تعامل JavaScript با Server بدون Reload کامل صفحه است، در حالی که XMLHttpRequest یک Browser API برای پیاده‌سازی HTTP Communication است.

12. چرا HTTP و Fetch را نباید یک مفهوم بدانیم؟

HTTP یک Protocol است، اما Fetch یک Browser API است که JavaScript از طریق آن می‌تواند با HTTP Resources ارتباط برقرار کند.

13. نقش JSON در HTTP Communication چیست؟

JSON یک قالب نمایش داده است که می‌تواند در Body یک HTTP Request یا Response استفاده شود؛ خود JSON مسئول تعریف ارتباط HTTP نیست.

14. چرا AJAX به Asynchronous JavaScript مرتبط است؟

زیرا Network Request ممکن است زمان ببرد و Application نباید برای انجام آن الزاماً کل اجرای خود را متوقف کند. JavaScript می‌تواند پس از آماده شدن Response، نتیجه را پردازش کند.

15. مدل ذهنی صحیح ارتباط Frontend با Server چیست؟
    User
    ↓
    JavaScript
    ↓
    Client
    ↓
    HTTP Request
    ↓
    Server
    ↓
    HTTP Response
    ↓
    Status + Data
    ↓
    JavaScript
    ↓
    UI
    Golden Answers
    HTTP چیست؟

HTTP پروتکل ارتباطی میان Client و Server است که ساختار Request و Response را تعریف می‌کند.

AJAX چیست؟

AJAX تکنیکی برای استفاده از JavaScript جهت ارتباط با Server و دریافت یا ارسال داده بدون Reload کامل صفحه است.

AJAX و XMLHttpRequest چه تفاوتی دارند؟

AJAX یک تکنیک ارتباطی است، اما XMLHttpRequest یک Browser API برای ایجاد و مدیریت HTTP Request است.

JSON چیست؟

JSON یک قالب متنی برای نمایش داده‌های ساختاریافته است که می‌تواند در HTTP Communication منتقل شود.

Status Code چه نقشی دارد؟

Status Code وضعیت پردازش Request را به Client اعلام می‌کند و به Application کمک می‌کند نتیجه ارتباط را تحلیل کند.

HTTP و Fetch چه تفاوتی دارند؟

HTTP پروتکل ارتباطی است و Fetch یک Browser API برای برقراری ارتباط با HTTP Resources است.

Conclusion

مسئله این فصل از یک نیاز ساده شروع شد:

Browser به داده‌ای نیاز دارد که در اختیارش نیست.

برای دریافت آن، Browser در نقش Client یک Request ایجاد می‌کند.

Request باید با یک قرارداد مشخص ارسال شود؛ این قرارداد HTTP است.

Request به Server می‌رسد و Server آن را پردازش می‌کند.

نتیجه پردازش در قالب Response به Client برمی‌گردد.

Client برای تحلیل Response به Status نیاز دارد و داده واقعی Application می‌تواند در قالبی مانند JSON منتقل شود.

اما هنوز یک نیاز مهم وجود دارد.

اگر هر بار دریافت داده جدید باعث Reload کامل صفحه شود، بسیاری از تعاملات Application غیرضروری و سنگین خواهند شد.

پس JavaScript باید بتواند با Server ارتباط برقرار کند و فقط داده موردنیاز را دریافت کند.

این نیاز به مفهوم AJAX منجر شد.

برای اجرای این نوع ارتباط، Browser APIهایی مانند XMLHttpRequest در اختیار JavaScript قرار می‌دهد.

اکنون مسئله اصلی دیگر ناشناخته نیست.

ما می‌دانیم:

Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Status
↓
JSON
↓
AJAX
↓
XMLHttpRequest

اما هنوز یک سؤال عملی باقی مانده است:

چگونه این HTTP Communication را با API مدرن Fetch در JavaScript پیاده‌سازی کنیم و Response را به داده قابل استفاده در Application تبدیل کنیم؟

این سؤال طبیعی، نقطه شروع Chapter 61 — Fetch API است.