Chapter 57 — Forms and User Input
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

نقش Form را در دریافت اطلاعات از کاربر توضیح دهید.
تفاوت میان Form، Input و User Data را درک کنید.
مقدار داده‌های واردشده توسط کاربر را دریافت و پردازش کنید.
submit Event را مدیریت کنید.
رفتار پیش‌فرض Browser هنگام Submit را تحلیل کنید.
از preventDefault() برای کنترل Submission Flow استفاده کنید.
داده‌های Form را با FormData جمع‌آوری کنید.
نقش name در Form Controlها را توضیح دهید.
داده‌های User را قبل از Processing اعتبارسنجی کنید.
تفاوت Constraint Validation و Validation منطقی را درک کنید.
Error State را در جریان تعامل با Form مدیریت کنید.
یک Submission Flow ساده و قابل نگهداری طراحی کنید.
Core Question

چگونه Input کاربر را از Form دریافت، بررسی و پردازش کنیم؟

جریان این فصل:

Form
↓
Input
↓
User Data
↓
Submit Event
↓
FormData
↓
Validation
↓
Error State
↓
Submission Flow
مقدمه

تا اینجا یاد گرفته‌ایم که Browser چگونه Document را به شکل DOM در اختیار JavaScript قرار می‌دهد، چگونه Elementها را پیدا و تغییر دهیم و چگونه User Interaction به Event تبدیل می‌شود.

اما هنوز یک مسئله مهم باقی مانده است.

فرض کنید یک Application فروشگاهی داریم.

کاربر می‌خواهد یک Product را جستجو کند.

در صفحه چیزی شبیه این وجود دارد:

<form>
  <input type="search">
  <button type="submit">Search</button>
</form>

کاربر عبارت:

laptop

را وارد می‌کند.

از دید کاربر، کار ساده است:

Type
↓
Click Search
↓
See Results

اما از دید JavaScript، اتفاقات بیشتری رخ داده است.

Application باید بفهمد:

کاربر چه چیزی وارد کرده است؟
چه زمانی می‌خواهد این اطلاعات پردازش شود؟
داده چگونه از UI به JavaScript منتقل شود؟
آیا داده معتبر است؟
اگر معتبر نیست، چه چیزی به کاربر نشان داده شود؟
و اگر معتبر است، مرحله بعدی چیست؟

بنابراین مسئله فقط «خواندن مقدار یک Input» نیست.

مسئله، مدیریت یک Data Flow از User Interface به Application Logic است.

برای ساختن این جریان، ابتدا باید جایی داشته باشیم که Inputهای کاربر را در یک عملیات مشخص کنار هم قرار دهد.

این همان جایی است که Form وارد می‌شود.

Form؛ نقطه شروع User Input

وقتی یک Application از کاربر اطلاعاتی دریافت می‌کند، معمولاً این اطلاعات بخشی از یک عملیات مشخص هستند.

مثلاً:

Search
Login
Sign Up
Checkout
Create Order

در همه این موارد چند Input ممکن است بخشی از یک عملیات واحد باشند.

مثلاً Login:

email
password

یا Checkout:

name
address
postalCode

اگر هر Input را کاملاً مستقل از بقیه در نظر بگیریم، باید خودمان رابطه میان آن‌ها را در JavaScript ایجاد کنیم.

HTML برای این مسئله یک مفهوم معنایی در اختیار ما قرار می‌دهد:

<form>
  ...
</form>

Form مجموعه‌ای از Form Controlها را در قالب یک Submission Unit قرار می‌دهد.

بنابراین Form صرفاً یک Container برای ظاهر UI نیست.

Form بیان می‌کند:

این Inputها در کنار یکدیگر بخشی از یک عملیات کاربر هستند.

برای مثال:

<form>
  <input type="email" name="email">
  <input type="password" name="password">

  <button type="submit">
    Login
  </button>
</form>

در اینجا:

Form
├── email
├── password
└── submit

یک جریان منطقی را تشکیل می‌دهند.

اما هنوز یک سؤال باقی می‌ماند:

خود داده‌ای که کاربر در Input وارد می‌کند کجاست؟

Input؛ محل دریافت داده

Input همان نقطه‌ای است که User Data وارد UI می‌شود.

مثلاً:

<input type="text" name="username">

در این Element چند مفهوم متفاوت وجود دارد.

type مشخص می‌کند Control چه نوع Inputای را انتظار دارد.

name نام Field را مشخص می‌کند.

و value مقدار فعلی Input را نگه می‌دارد.

فرض کنید کاربر بنویسد:

Omid

اکنون می‌توانیم وضعیت Input را به شکل زیر تصور کنیم:

Input
├── name  → "username"
└── value → "Omid"

این دو Property را نباید با یکدیگر اشتباه گرفت.

name مشخص می‌کند داده با چه نامی شناخته شود.

value داده فعلی را نشان می‌دهد.

پس اگر:

<input
type="text"
name="username"
>

داشته باشیم و کاربر بنویسد:

Omid

داده مفهومی ما این است:

username → "Omid"

اما JavaScript هنوز این داده را دریافت نکرده است.

پس سؤال بعدی طبیعی است:

چگونه مقدار Input را از DOM به JavaScript منتقل کنیم؟

دریافت Value از Input

اگر فقط یک Input داشته باشیم، ساده‌ترین راه دسترسی به داده، خواندن value است.

const input = document.querySelector(
'input[name="username"]'
);

console.log(input.value);

اگر کاربر نوشته باشد:

Omid

خروجی:

Omid

خواهد بود.

این روش برای یک Input کاملاً مناسب است.

اما تصور کنید یک Form ثبت‌نام داریم:

name
email
password

اکنون باید سه Element را پیدا کنیم و مقدار هرکدام را بخوانیم.

مثلاً:

const name = nameInput.value;
const email = emailInput.value;
const password = passwordInput.value;

برای یک Form کوچک مشکلی ندارد.

اما با افزایش تعداد Fieldها، Application باید بیشتر و بیشتر درباره ساختار داخلی UI بداند.

در اینجا یک نیاز جدید ایجاد می‌شود:

اگر Form خودش مجموعه‌ای از داده‌هاست، آیا راهی وجود ندارد که به جای خواندن تک‌تک Inputها، داده‌های Form را یکجا دریافت کنیم؟

پاسخ این نیاز، FormData است.

اما قبل از رسیدن به آن، هنوز یک سؤال مهم‌تر داریم:

چه زمانی باید این داده‌ها را دریافت کنیم؟

Submit؛ لحظه‌ای که User آماده پردازش است

کاربر ممکن است در طول چند ثانیه چندین بار مقدار Input را تغییر دهد.

مثلاً:

l
la
lap
lapt
lapto
laptop

اگر Application قرار باشد فقط یک Search معمولی انجام دهد، احتمالاً نمی‌خواهیم با هر تغییر کوچک، عملیات نهایی Form را اجرا کنیم.

ما به نقطه‌ای نیاز داریم که User عملاً اعلام کند:

من Input خود را کامل کرده‌ام و می‌خواهم این Form پردازش شود.

این همان Submit است.

در HTML معمولاً Submit با Button زیر مشخص می‌شود:

<button type="submit">
  Search
</button>

اما نکته مهم این است که Submit متعلق به Form است، نه صرفاً Button.

برای مثال کاربر ممکن است به جای کلیک روی Button، در Input کلید Enter را فشار دهد.

بنابراین اگر هدف ما مدیریت Submission است، بهتر است به خود Form گوش دهیم:

const form = document.querySelector('form');

form.addEventListener('submit', function (event) {
// ...
});

اکنون JavaScript به یک مفهوم مشخص متصل شده است:

User wants to submit the Form
↓
submit Event
↓
JavaScript

این همان نقطه‌ای است که User Interaction وارد Application Logic می‌شود.

Submit Event و Default Action

اکنون Form را به submit Event متصل کرده‌ایم.

اما یک رفتار مهم دیگر وجود دارد.

Browser خودش برای Form Submission رفتار پیش‌فرض دارد.

بنابراین وقتی Form Submit می‌شود، صرفاً Handler ما اجرا نمی‌شود؛ Browser نیز ممکن است Default Action مربوط به Form را انجام دهد.

در Applicationهای ساده HTML ممکن است همین رفتار پیش‌فرض دقیقاً همان چیزی باشد که می‌خواهیم.

اما در یک Application که JavaScript مسئول پردازش داده است، معمولاً می‌خواهیم ابتدا:

Receive
↓
Validate
↓
Process

را انجام دهیم.

پس باید کنترل Submission را در اختیار JavaScript قرار دهیم.

اینجاست که preventDefault() وارد می‌شود:

form.addEventListener('submit', function (event) {
event.preventDefault();
});

این دستور به Browser می‌گوید:

Default Action این Event را انجام نده.

نکته مهم این است که preventDefault() خود Event را حذف نمی‌کند.

Event همچنان ایجاد شده و Handler نیز اجرا شده است.

فقط رفتار پیش‌فرض Browser لغو شده است.

بنابراین:

submit Event
↓
Handler executes
↓
preventDefault()
↓
Default Action cancelled
↓
JavaScript controls the flow

اکنون JavaScript می‌تواند داده Form را قبل از هر Processing دیگری بررسی کند.

و این ما را دوباره به مسئله‌ای که قبلاً داشتیم برمی‌گرداند:

چگونه تمام داده‌های Form را یکجا دریافت کنیم؟

FormData؛ جمع‌آوری داده Form

فرض کنید Form ثبت‌نام ما چنین باشد:

<form id="signup-form">
  <input
    type="text"
    name="name"
  >

<input
type="email"
name="email"
>

<input
type="password"
name="password"
>

  <button type="submit">
    Create Account
  </button>
</form>

کاربر سه مقدار وارد می‌کند.

ما می‌توانیم هر Input را جداگانه پیدا کنیم، اما اکنون می‌دانیم که Form خودش یک واحد Submission است.

پس بهتر است داده را در همان سطح دریافت کنیم.

JavaScript Web API به نام FormData دقیقاً برای کار با داده‌های Form طراحی شده است:

const formData = new FormData(form);

اکنون:

Form
↓
FormData

یعنی وضعیت داده‌های Form در این نقطه در اختیار JavaScript قرار گرفته است.

می‌توانیم مقدار یک Field را با get() دریافت کنیم:

const name = formData.get('name');
const email = formData.get('email');
const password = formData.get('password');

اکنون:

name
email
password

از Form استخراج شده‌اند.

این دقیقاً همان مسئله‌ای را حل می‌کند که هنگام افزایش تعداد Inputها ایجاد شد.

چرا name اهمیت پیدا می‌کند؟

اکنون نقش name که قبلاً معرفی کردیم روشن‌تر می‌شود.

وقتی می‌نویسیم:

<input
type="email"
name="email"
>

name فقط برای توصیف HTML نیست.

این نام Field را در داده Form مشخص می‌کند.

بنابراین:

formData.get('email');

به دنبال Fieldای با نام:

email

می‌گردد.

اگر Input را چنین بنویسیم:

<input type="email">

دیگر Field نام مشخصی برای دسترسی از طریق FormData.get() ندارد.

پس یک رابطه مستقیم داریم:

name
↓
Form Field Identity
↓
FormData.get(name)

به همین دلیل طراحی صحیح Form Controlها از همان ابتدا اهمیت دارد.

FormData و وضعیت Form

اکنون می‌توانیم یک تفاوت مهم را روشن کنیم.

وقتی می‌نویسیم:

input.value

مقدار یک Input مشخص را می‌خوانیم.

اما وقتی می‌نویسیم:

new FormData(form)

در حال جمع‌آوری داده‌های Form به‌عنوان یک مجموعه هستیم.

پس:

input.value
→ یک Control

FormData
→ داده‌های Form

این دو روش رقیب یکدیگر نیستند.

هرکدام برای سطح متفاوتی از مسئله مناسب هستند.

اگر فقط به Value یک Input نیاز داریم، value کافی است.

اگر در حال مدیریت Submission یک Form هستیم، FormData مدل مناسب‌تری برای جمع‌آوری داده است.

وقتی داده را داریم، مشکل جدید شروع می‌شود

اکنون Submission Flow ما تا اینجا کامل‌تر شده است:

User
↓
Input
↓
Submit
↓
FormData
↓
Data

اما یک مشکل اساسی وجود دارد.

هر داده‌ای که User وارد کند، الزاماً داده معتبر نیست.

فرض کنید Form ثبت‌نام را داریم.

کاربر ممکن است Email را خالی بگذارد:

email → ""

یا مقدار زیر را وارد کند:

hello

در حالی که Application انتظار یک Email معتبر دارد.

از دید JavaScript هر دو Value می‌توانند String باشند.

اما از دید Application، ممکن است هیچ‌کدام داده قابل قبول نباشند.

پس اکنون سؤال جدیدی داریم:

چگونه قبل از Processing بفهمیم داده User معتبر است؟

اینجا Validation وارد جریان می‌شود.

Validation؛ مرز میان Input و Application

Validation یعنی بررسی اینکه داده دریافت‌شده با قواعد مورد انتظار سازگار است یا خیر.

بنابراین Submission Flow ما دیگر:

Submit
↓
Process

نیست.

بلکه:

Submit
↓
Collect Data
↓
Validate
↓
Process

است.

این تغییر کوچک، از نظر مهندسی بسیار مهم است.

زیرا Application نباید داده خام User را مستقیماً وارد Logic اصلی کند.

بین این دو باید یک مرز وجود داشته باشد:

User Input
↓
Validation
↓
Application Data
Constraint Validation

برخی قوانین عمومی را می‌توان مستقیماً در HTML مشخص کرد.

برای مثال:

<input
type="email"
name="email"
required
>

در اینجا:

required

می‌گوید این Field نباید خالی باشد.

و:

type="email"

به Browser می‌گوید Input باید با Constraint مربوط به Email سازگار باشد.

Browser می‌تواند این Constraintها را بررسی کند.

برای مثال:

if (!form.checkValidity()) {
return;
}

در اینجا Application از Browser می‌پرسد:

آیا Form بر اساس Constraintهای تعریف‌شده معتبر است؟

نتیجه می‌تواند به شکل مفهومی چنین باشد:

Form
↓
Constraint Validation
↓
Valid / Invalid

این Validation برای تجربه کاربر بسیار مفید است.

اما هنوز یک مسئله باقی مانده است.

Validation فقط Constraintهای HTML نیست

فرض کنید یک Application فروشگاهی داریم.

کاربر مقدار زیر را وارد می‌کند:

quantity → 10

از نظر HTML ممکن است مقدار کاملاً معتبر باشد.

اما فرض کنید Product موردنظر فقط 3 عدد موجودی دارد.

اکنون:

quantity = 10

از نظر Input معتبر است، اما از نظر Application معتبر نیست.

این دیگر یک Constraint عمومی HTML نیست.

این یک Business Rule است.

بنابراین Validation می‌تواند در دو سطح اتفاق بیفتد:

Input Constraints
↓
Basic Validity

و:

Application Rules
↓
Business Validity

این تفاوت مهم است.

مثلاً:

required

می‌تواند خالی نبودن Field را بررسی کند.

اما نمی‌تواند به‌تنهایی تعیین کند که:

quantity <= inventory

باشد.

این نوع Rule به منطق Application مربوط است.

User Input قابل اعتماد نیست

از اینجا یک اصل مهندسی مهم به دست می‌آوریم:

User Input را نباید صرفاً به دلیل عبور از UI Validation معتبر و قابل اعتماد فرض کرد.

Validation سمت Browser برای جلوگیری از خطاهای رایج و ارائه Feedback سریع به کاربر بسیار ارزشمند است.

اما Browser در اختیار کاربر است.

بنابراین اگر داده قرار است به یک Server یا سیستم قابل اعتماد ارسال شود، Validation در آن مرز نیز باید انجام شود.

مدل کلی:

User Input
↓
Client Validation
↓
Application Processing
↓
Server Validation
↓
Trusted Processing

بنابراین Client Validation بخشی از تجربه و منطق UI است، نه جایگزین Validation سمت Server.

وقتی Validation شکست می‌خورد چه اتفاقی می‌افتد؟

فرض کنید کاربر Email را وارد نکرده است.

Application متوجه می‌شود:

Invalid

اکنون یک سؤال UX ایجاد می‌شود:

کاربر چگونه باید بفهمد چه چیزی اشتباه است؟

صرفاً این کد:

if (!email) {
return;
}

برای Application کافی نیست.

چون Logic برنامه می‌داند داده نامعتبر است، اما User چیزی درباره دلیل آن نمی‌داند.

بنابراین Invalid بودن داده باید به UI منتقل شود.

اینجا Error State شکل می‌گیرد.

Error State

Error State یعنی UI وضعیت فعلی خود را بر اساس یک خطای شناسایی‌شده تغییر دهد.

مثلاً:

Email is required.

یا:

Please enter a valid email address.

جریان اکنون چنین است:

User Input
↓
Validation
↓
Invalid
↓
Error State
↓
User Corrects Input
↓
Validation Again

این نکته مهم است:

Error بخشی از جریان عادی Form است.

در Formهای واقعی، Invalid Input یک اتفاق غیرعادی نیست.

کاربر ممکن است:

Field را خالی بگذارد.
مقدار اشتباه وارد کند.
فرمت نامناسب وارد کند.
Rule مربوط به Application را رعایت نکند.

بنابراین Application باید برای این وضعیت از ابتدا طراحی شده باشد.

Error باید قابل اصلاح باشد

Error State خوب فقط نمی‌گوید:

Invalid

بلکه تا حد امکان به User کمک می‌کند بفهمد چه چیزی باید اصلاح شود.

مثلاً:

Password must contain at least 8 characters.

از:

Invalid password.

اطلاعات بیشتری در اختیار User قرار می‌دهد.

پس Error State بخشی از Communication میان Application و User است.

مدل کامل‌تر:

Input
↓
Validation
↓
Invalid
↓
Explain Problem
↓
User Correction
↓
Validation

در نتیجه Error State پایان جریان نیست.

بخشی از یک Loop تعاملی است.

یک Form کوچک، یک Submission Flow کامل

اکنون می‌توانیم تمام Conceptهای قبلی را در یک مثال کوچک کنار هم قرار دهیم.

فرض کنید Search Form داریم:

<form id="search-form">
  <input
    type="search"
    name="query"
    required
  >

  <button type="submit">
    Search
  </button>
</form>

<p id="error"></p>

در JavaScript:

const form = document.querySelector('#search-form');
const errorElement = document.querySelector('#error');

form.addEventListener('submit', function (event) {
event.preventDefault();

const formData = new FormData(form);
const query = formData.get('query');

if (!query) {
errorElement.textContent =
'Please enter a search query.';

    return;
}

errorElement.textContent = '';

console.log(query);
});

اکنون می‌توانیم این کد را نه به‌عنوان مجموعه‌ای از دستورها، بلکه به‌عنوان یک جریان بخوانیم:

User enters data
↓
Form
↓
submit Event
↓
preventDefault()
↓
FormData
↓
Read query
↓
Validate
↓
Invalid? ────── Yes → Error State
│
No
↓
Process Data

این همان Submission Flow است.

Submission Flow؛ ترکیب همه Conceptها

اکنون می‌توانیم کل فصل را در یک مدل ذهنی واحد قرار دهیم.

ابتدا User با Form تعامل می‌کند:

Form

Form شامل Controlهایی است که داده دریافت می‌کنند:

Input

User مقدار خود را وارد می‌کند:

User Data

وقتی User آماده پردازش است:

Submit Event

JavaScript در صورت نیاز Default Action را لغو می‌کند:

preventDefault()

داده‌های Form جمع‌آوری می‌شوند:

FormData

سپس داده اعتبارسنجی می‌شود:

Validation

اگر داده معتبر نباشد:

Error State

و اگر معتبر باشد:

Submission Flow

ادامه پیدا می‌کند.

بنابراین Concept Flow فصل اکنون یک توالی صرفاً آموزشی نیست؛ بلکه یک جریان واقعی داده در Application است:

Form
↓
Input
↓
User Data
↓
Submit Event
↓
FormData
↓
Validation
↓
Error State
↓
Submission Flow
input Event در برابر submit Event

در اینجا ممکن است یک سؤال مهم مطرح شود.

اگر هدف ما دریافت User Input است، چرا از input Event استفاده نکنیم؟

input Event زمانی مفید است که بخواهیم به تغییر Value در هنگام تعامل User واکنش نشان دهیم.

مثلاً:

input.addEventListener('input', function () {
console.log(input.value);
});

این Event برای سناریوهایی مانند موارد زیر مناسب است:

Live Search
Character Counter
Instant Feedback
Live Validation

اما submit مفهوم دیگری دارد.

submit زمانی مهم است که User بخواهد Form را وارد مرحله Processing کند.

بنابراین:

input
↓
Value is changing

در مقابل:

submit
↓
Form should be processed

این دو Event را نباید با یکدیگر جایگزین کنیم.

اگر Search Application قرار است فقط پس از Submit جستجو را انجام دهد، اجرای Submission Logic در هر input Event ضرورتی ندارد.

چرا Snapshot داده مهم است؟

فرض کنید User ابتدا بنویسد:

lap

و سپس آن را به:

laptop

تغییر دهد.

اگر در هر لحظه مقدار Input را بخوانیم، داده دائماً در حال تغییر است.

اما وقتی User Form را Submit می‌کند، Application یک لحظه مشخص دارد که می‌تواند داده را جمع‌آوری کند:

Current Form State
↓
FormData
↓
Validation
↓
Processing

در این معنا FormData نمایی از داده Form در زمان مشخص Submission است.

این مدل ذهنی کمک می‌کند UI State را از Application Data جدا کنیم.

Form بخشی از UI است.

FormData داده‌ای است که Application از وضعیت فعلی Form دریافت کرده است.

تبدیل FormData به Object

در بسیاری از Applicationها ممکن است بخواهیم داده Form را به شکل یک Object معمولی در اختیار Logic برنامه قرار دهیم.

برای این کار می‌توان از:

const data = Object.fromEntries(
new FormData(form)
);

استفاده کرد.

در یک Form ساده، نتیجه می‌تواند مفهومی مشابه این باشد:

{
name: 'Omid',
email: 'omid@example.com'
}

اکنون داده Form از Web API مخصوص Form به یک Object معمولی تبدیل شده است.

این کار زمانی مفید است که Logic بعدی Application با Objectها کار می‌کند.

اما باید یک نکته را فراموش نکنیم:

تبدیل ساختار داده، نوع واقعی Valueها را به‌طور خودکار مطابق نیاز Business Logic تغییر نمی‌دهد.

برای مثال یک مقدار عددی دریافت‌شده از Form ممکن است همچنان به‌صورت String در اختیار Application باشد.

اگر Business Logic به Number نیاز دارد، تبدیل Type باید به‌صورت آگاهانه انجام شود.

این همان مرحله‌ای است که User Input به Application Data تبدیل می‌شود.

Form Submission به‌عنوان یک مرز معماری

اکنون می‌توانیم به یک نگاه مهندسی‌تر برسیم.

Form را می‌توان یک Boundary میان UI و Application Logic در نظر گرفت.

در سمت UI:

User
↓
Input
↓
Form

در مرز Submission:

Submit
↓
Collect
↓
Validate

و در سمت Application:

Valid Data
↓
Business Logic
↓
Result

بنابراین Form صرفاً HTML نیست.

Form یکی از نقاطی است که در آن داده از دنیای User وارد دنیای Application می‌شود.

به همین دلیل طراحی Form، Validation و Error Handling در Applicationهای واقعی اهمیت زیادی دارد.

Best Practices
Form را به‌عنوان واحد Submission مدیریت کنید

به جای اینکه Logic اصلی را فقط به Button وابسته کنیم، Submission را روی Form مدیریت کنیم:

form.addEventListener('submit', handler);

این کار با مدل معنایی Form سازگارتر است.

برای Form Controlها name مشخص کنید

اگر Field بخشی از Form Data است، name آن باید به‌درستی تعریف شود:

<input name="email">
Data Collection را از Validation جدا کنید

جریان را ذهنی و در صورت نیاز در کد نیز به مراحل مشخص تقسیم کنید:

Collect
↓
Validate
↓
Process
Error State را بخشی از UI بدانید

Invalid Input باید به شکلی قابل فهم به User منتقل شود.

Client Validation را جایگزین Server Validation نکنید

Browser Validation برای UX مهم است، اما داده‌ای که وارد مرز قابل اعتماد سیستم می‌شود باید در آن مرز نیز اعتبارسنجی شود.

فقط زمانی که لازم است از preventDefault() استفاده کنید

preventDefault() ابزار کنترل Default Action است، نه یک دستور عمومی برای تمام Eventها.

اشتباهات رایج
مدیریت Submission فقط با click

این طراحی:

button.addEventListener('click', handler);

ممکن است برای یک تعامل خاص کار کند، اما مفهوم Submission متعلق به Form است.

برای مدیریت Form بهتر است submit Event را روی Form دریافت کنیم.

فراموش کردن preventDefault()

اگر Application می‌خواهد خودش Submission Flow را کنترل کند، باید Default Action Browser را نیز آگاهانه مدیریت کند.

فراموش کردن name

ممکن است Input از نظر UI کاملاً کار کند اما هنگام جمع‌آوری Form Data، به دلیل نبود name داده مورد انتظار در دسترس نباشد.

یکی دانستن Value و Validity

اینکه Input دارای Value است:

Has Value

به معنی معتبر بودن آن نیست:

Has Value ≠ Valid Data
اجرای Business Logic روی داده خام

نباید داده‌ای که مستقیماً از User دریافت شده است بدون بررسی وارد Logic اصلی Application شود.

استفاده از input برای تمام Submission Logic

input Event برای تغییرات Value مناسب است.

submit Event برای مدیریت Submission مناسب است.

هرکدام باید بر اساس نیاز واقعی Application استفاده شوند.

نمایش Error بدون مسیر اصلاح

نمایش:

Invalid input

معمولاً اطلاعات کافی در اختیار User قرار نمی‌دهد.

Error State باید تا حد امکان مسیر اصلاح داده را روشن کند.

Summary

Form نقطه‌ای است که چند User Input را در یک Submission Unit قرار می‌دهد.

Input داده را از User دریافت می‌کند و value مقدار فعلی آن را نشان می‌دهد.

وقتی User آماده پردازش Form است، submit Event ایجاد می‌شود.

JavaScript می‌تواند Submission را مدیریت کند و در صورت نیاز با preventDefault() رفتار پیش‌فرض Browser را لغو کند.

پس از آن می‌توان داده‌های Form را با FormData جمع‌آوری کرد.

name هر Form Control مشخص می‌کند Field با چه نامی در داده Form شناخته شود.

اما دریافت داده پایان کار نیست.

User Input باید Validation شود.

Validation می‌تواند شامل Constraintهای HTML و همچنین Rules مربوط به Application باشد.

اگر داده نامعتبر باشد، Application باید Error State مناسبی ایجاد کند.

اگر داده معتبر باشد، Submission Flow می‌تواند ادامه پیدا کند.

بنابراین:

Form
↓
Input
↓
User Data
↓
Submit Event
↓
FormData
↓
Validation
↓
Error State
↓
Submission Flow

یک مدل ذهنی برای مدیریت User Input در Applicationهای واقعی فراهم می‌کند.

Key Takeaways
Form یک Submission Unit برای User Inputها است.
Input محل دریافت داده از User است.
value مقدار فعلی Input را نشان می‌دهد.
submit Event نقطه طبیعی شروع Submission Flow است.
Submission متعلق به Form است، نه صرفاً Submit Button.
preventDefault() Default Action مربوط به Event را لغو می‌کند.
FormData داده‌های Form را به‌صورت یک مجموعه جمع‌آوری می‌کند.
name برای شناسایی Fieldهای Form اهمیت دارد.
داشتن Value به معنی معتبر بودن داده نیست.
Validation باید پیش از Processing انجام شود.
Constraint Validation و Business Validation یکسان نیستند.
Error State بخشی از تعامل طبیعی User با Form است.
input Event برای تغییرات Value و submit Event برای Submission مناسب است.
Client Validation برای UX ضروری است، اما جایگزین Server Validation نیست.
Submission Flow بهتر است به مراحل مشخصی مانند Collect، Validate و Process تقسیم شود.
Technical Interview
سطح Junior
1. Form چیست؟

Form ساختاری در HTML برای گروه‌بندی User Inputها و مدیریت یک عملیات Submission است.

2. submit Event چیست؟

Eventای است که هنگام درخواست Submission یک Form ایجاد می‌شود و به JavaScript اجازه می‌دهد Submission Flow را مدیریت کند.

3. preventDefault() چه کاری انجام می‌دهد؟

Default Action مرتبط با Event را لغو می‌کند؛ خود Event و اجرای Handler را متوقف نمی‌کند.

4. FormData چیست؟

یک Web API برای جمع‌آوری داده‌های Form Controlها و کار با آن‌ها به‌عنوان داده Form است.

5. چرا name در Input اهمیت دارد؟

زیرا name هویت Field را در Form Data مشخص می‌کند و برای دسترسی به داده از طریق FormData.get() استفاده می‌شود.

6. چگونه مقدار مستقیم یک Input را بخوانیم؟

با استفاده از Property به نام value:

const value = input.value;
سطح Mid-Level
7. چرا Submission را روی Form مدیریت می‌کنیم، نه فقط Button؟

زیرا Submission رفتار مربوط به Form است و ممکن است بدون کلیک مستقیم روی Button نیز اتفاق بیفتد؛ بنابراین Form نقطه مناسب‌تری برای مدیریت این جریان است.

8. تفاوت input.value و FormData چیست؟

input.value مقدار یک Control مشخص را می‌خواند، در حالی که FormData داده‌های Form را به‌عنوان یک مجموعه جمع‌آوری می‌کند.

9. چرا User Input باید Validation شود؟

زیرا داده‌ای که User وارد می‌کند الزاماً با قواعد مورد انتظار Application سازگار نیست و نباید مستقیماً وارد Business Logic شود.

10. تفاوت Constraint Validation و Business Validation چیست؟

Constraint Validation قواعد عمومی Form مانند required یا نوع Email را بررسی می‌کند؛ Business Validation قواعد خاص Application مانند محدودیت موجودی Product را بررسی می‌کند.

11. چرا input Event و submit Event را نباید یکی دانست؟

input برای واکنش به تغییر Value در جریان تعامل User است، در حالی که submit نشان‌دهنده درخواست پردازش Form است.

12. چرا Error State اهمیت دارد؟

زیرا Invalid بودن داده باید به User منتقل شود تا بتواند Input خود را اصلاح کند.

سطح Senior
13. Submission Flow مناسب برای یک Form چیست؟
    Collect
    ↓
    Validate
    ↓
    Handle Errors
    ↓
    Process

این تفکیک باعث می‌شود دریافت داده، اعتبارسنجی و Logic اصلی مسئولیت‌های مشخصی داشته باشند.

14. چرا Client Validation به‌تنهایی کافی نیست؟

زیرا Browser در اختیار User است و نباید داده‌ای که از Client دریافت می‌شود صرفاً بر اساس Validation UI قابل اعتماد فرض شود. Validation در مرز قابل اعتماد سیستم نیز ضروری است.

15. چرا Form را می‌توان یک Boundary میان UI و Application Logic دانست؟

زیرا User Data ابتدا در UI ایجاد می‌شود، سپس از طریق Form Submission جمع‌آوری و Validation شده و در نهایت به Application Logic منتقل می‌شود.

16. چرا FormData برای Formهای چندField مناسب‌تر از خواندن دستی تمام Inputها است؟

زیرا FormData داده‌های Form را در سطح خود Form جمع‌آوری می‌کند و Application را از مدیریت دستی تعداد زیادی DOM Element برای Data Collection جدا می‌کند.

17. آیا FormData همان Object معمولی JavaScript است؟

خیر. FormData یک Web API مخصوص کار با Form Data است. در صورت نیاز می‌توان داده آن را به یک Object معمولی تبدیل کرد.

18. آیا وجود Value به معنی معتبر بودن Input است؟

خیر.

Has Value
≠
Valid Data

Value فقط نشان می‌دهد داده‌ای وجود دارد؛ Validation مشخص می‌کند آیا آن داده با قواعد مورد انتظار سازگار است یا خیر.

Golden Answers
Form چه مسئله‌ای را حل می‌کند؟

Form چند User Input را در قالب یک Submission Unit سازمان‌دهی می‌کند و یک نقطه مشخص برای مدیریت Submission در اختیار Application قرار می‌دهد.

چرا submit نقطه مهمی در Form است؟

زیرا Submit لحظه‌ای است که User درخواست می‌کند داده Form از حالت صرفاً UI خارج شده و وارد مرحله Processing شود.

FormData چه مسئله‌ای را حل می‌کند؟

FormData نیاز به جمع‌آوری دستی تمام Fieldهای Form را کاهش می‌دهد و داده‌های Form را در یک ساختار استاندارد برای پردازش در اختیار JavaScript قرار می‌دهد.

چرا Validation باید قبل از Processing انجام شود؟

زیرا User Input خام است و Application نباید داده‌ای را که هنوز اعتبار آن مشخص نشده مستقیماً وارد Business Logic کند.

Error State چه نقشی دارد؟

Error State نتیجه Validation ناموفق را به UI منتقل می‌کند و به User اجازه می‌دهد مشکل Input را شناسایی و اصلاح کند.

Submission Flow چیست؟

Submission Flow مسیر تبدیل User Input به داده قابل پردازش است:

User Input
↓
Submit
↓
Collect
↓
Validate
↓
Error or Process
Conclusion

مدیریت Form در نگاه اول ممکن است فقط به خواندن مقدار یک Input و واکنش به کلیک یک Button محدود به نظر برسد.

اما یک Application واقعی با مسئله بزرگ‌تری روبه‌رو است.

User ابتدا داده را وارد می‌کند.

Form این Inputها را در یک عملیات مشخص قرار می‌دهد.

Submit نقطه‌ای ایجاد می‌کند که User آماده پردازش داده است.

JavaScript Submission را دریافت می‌کند و در صورت نیاز Default Action مرورگر را کنترل می‌کند.

FormData داده‌های Form را جمع‌آوری می‌کند.

Validation مشخص می‌کند داده برای Application قابل قبول است یا خیر.

اگر داده نامعتبر باشد، Error State شکل می‌گیرد و User باید فرصت اصلاح آن را داشته باشد.

اگر داده معتبر باشد، Application می‌تواند آن را وارد مرحله بعدی Processing کند.

بنابراین Form را نباید صرفاً مجموعه‌ای از Inputها و Buttonها دید.

Form یک مسیر انتقال داده از User Interface به Application Logic ایجاد می‌کند:

Form
↓
Input
↓
User Data
↓
Submit Event
↓
FormData
↓
Validation
↓
Error State
↓
Submission Flow

و این دقیقاً همان چیزی است که مدیریت User Input در یک JavaScript Application را از خواندن ساده input.value به یک Data Flow قابل طراحی و کنترل تبدیل می‌کند.