Chapter 69 — JavaScript Modules
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

توضیح دهید چرا با بزرگ شدن یک JavaScript Application، نگهداری تمام کد در یک Scope مناسب نیست.
مفهوم Module را به‌عنوان یک Boundary برای سازمان‌دهی Code توضیح دهید.
رابطه Module و Encapsulation را درک کنید.
ES Modules را به‌عنوان سیستم Module بومی JavaScript بشناسید.
نقش export و import را در ارتباط بین Moduleها توضیح دهید.
مفهوم Module Scope را از Global Scope تفکیک کنید.
توضیح دهید Dynamic import() چه مسئله‌ای را حل می‌کند.
Moduleها را بر اساس Responsibility و Boundary مناسب در یک Application سازمان‌دهی کنید.
Core Question

چگونه یک JavaScript Application بزرگ را به Moduleهای مستقل و دارای Boundary مشخص تقسیم کنیم؟

جریان این فصل:

Large Application
↓
Global Scope Problem
↓
Module
↓
Encapsulation
↓
ES Modules
↓
export
↓
import
↓
Module Scope
↓
Dynamic import
مقدمه

در فصل‌های قبل، JavaScript را در مقیاس کوچک بررسی کردیم.

Functionها برای تقسیم Logic استفاده شدند.

Objectها برای سازمان‌دهی State و Behavior به کار رفتند.

در OOP نیز دیدیم که می‌توان Responsibilities مرتبط را در ساختارهای مشخص قرار داد.

اما Application همیشه کوچک باقی نمی‌ماند.

فرض کنید یک Application فروشگاهی داریم.

در ابتدا ممکن است تمام Logic در یک فایل قرار داشته باشد:

const products = [];
const cart = [];
const currentUser = null;

function addProduct() {
// ...
}

function addToCart() {
// ...
}

function login() {
// ...
}

function renderProducts() {
// ...
}

در یک Application کوچک، این ساختار ممکن است قابل مدیریت باشد.

اما با افزایش قابلیت‌ها، تعداد Functionها و داده‌ها نیز افزایش پیدا می‌کند.

ممکن است Logic مربوط به User، Product، Cart، API و UI همگی در یک Scope قرار بگیرند.

در این شرایط مسئله دیگر فقط تعداد خطوط Code نیست.

مسئله این است که Boundary مشخصی میان بخش‌های مختلف Application وجود ندارد.

اگر هر بخش بتواند به همه چیز دسترسی داشته باشد، تغییر یک قسمت می‌تواند قسمت‌های دیگر را نیز تحت تأثیر قرار دهد.

پس با یک مسئله جدید روبه‌رو می‌شویم:

چگونه یک Application بزرگ را به بخش‌های مستقل‌تر تقسیم کنیم، بدون اینکه ارتباط میان آن‌ها از بین برود؟

پاسخ این مسئله، مفهوم Module است.

وقتی Application بزرگ می‌شود، Global Scope به مسئله تبدیل می‌شود

فرض کنید Application ما به چند بخش تقسیم شده است:

User
Product
Cart
API
UI

اگر تمام این Logicها در یک Scope مشترک قرار بگیرند، نام‌ها و داده‌ها می‌توانند با یکدیگر تداخل پیدا کنند.

برای مثال:

const user = {};

ممکن است در بخش دیگری نیز متغیری با همین نام وجود داشته باشد.

حتی اگر نام‌ها با یکدیگر تداخل نداشته باشند، هنوز یک مشکل مهم باقی است:

هر بخش از Application ممکن است بتواند به داده‌ها و Functionهای بخش‌های دیگر دسترسی داشته باشد.

در نتیجه مشخص نیست:

چه چیزی فقط برای یک بخش داخلی است؟
چه چیزی باید در اختیار بخش‌های دیگر قرار بگیرد؟
چه چیزی نباید از بیرون قابل دسترسی باشد؟

بنابراین مسئله فقط Global Scope Pollution نیست.

مسئله بزرگ‌تر، نبودن یک Boundary مشخص میان بخش‌های Application است.

اینجاست که Module معنا پیدا می‌کند.

Module چیست؟

Module یک واحد مستقل از Code است که بخشی از Functionality یک Application را در یک Boundary مشخص قرار می‌دهد.

برای مثال، به جای اینکه تمام Logic مربوط به Cart را در Application اصلی قرار دهیم، می‌توانیم آن را در یک Module سازمان‌دهی کنیم.

Application
│
├── User Module
├── Product Module
├── Cart Module
└── API Module

هر Module مسئول بخشی از Application است.

برای مثال:

Cart Module
→ مدیریت Cart
→ افزودن Product
→ حذف Product
→ محاسبه Total

هدف Module فقط تقسیم یک فایل بزرگ به چند فایل کوچک‌تر نیست.

هدف اصلی، تقسیم Responsibility و ایجاد Boundary است.

این تفاوت مهم است.

اگر فقط Code را بین چند فایل تقسیم کنیم، ممکن است همچنان تمام بخش‌ها به یکدیگر وابسته و درهم‌تنیده باشند.

Module زمانی ارزش واقعی پیدا می‌کند که مشخص کند:

این بخش از Code چه مسئولیتی دارد و چه چیزهایی را در اختیار بخش‌های دیگر قرار می‌دهد؟

این موضوع ما را به مفهوم مهم دیگری می‌رساند: Encapsulation.

Module و Encapsulation

در Encapsulation تلاش می‌کنیم جزئیات داخلی یک بخش را از بیرون پنهان کنیم و فقط بخش‌هایی را که برای استفاده دیگران لازم هستند در اختیار آن‌ها قرار دهیم.

فرض کنید Cart داخلی خود را دارد:

const cart = [];

function addToCart(product) {
cart.push(product);
}

ممکن است Application فقط به addToCart نیاز داشته باشد و نیازی نداشته باشد مستقیماً به ساختار داخلی cart دسترسی داشته باشد.

در این حالت Module می‌تواند یک Boundary ایجاد کند:

Cart Module
│
├── cart          ← Internal
├── addToCart()   ← Public
└── removeFromCart()

بنابراین Module کمک می‌کند میان Implementation داخلی و Interface قابل استفاده تفاوت ایجاد کنیم.

این همان جایی است که Module از یک تقسیم‌بندی ساده فایل‌ها فراتر می‌رود.

وقتی این Boundary ایجاد شد، سؤال بعدی این است:

JavaScript چگونه چنین Moduleهایی را به‌صورت استاندارد تعریف و بین آن‌ها ارتباط برقرار می‌کند؟

پاسخ، ES Modules است.

ES Modules

JavaScript برای تعریف Moduleها یک سیستم استاندارد به نام ECMAScript Modules یا ES Modules دارد.

ES Modules دو نیاز اصلی ما را برطرف می‌کند:

تعریف Boundary برای Code
ایجاد ارتباط کنترل‌شده میان Moduleها

دو ابزار اصلی این سیستم عبارت‌اند از:

export

و:

import

export مشخص می‌کند چه چیزی از یک Module در اختیار سایر Moduleها قرار گیرد.

import مشخص می‌کند یک Module از چه چیزی استفاده می‌کند.

بنابراین ارتباط میان Moduleها به‌صورت صریح تعریف می‌شود.

export؛ مشخص کردن چیزی که Module ارائه می‌دهد

فرض کنید Module مربوط به Cart یک Function دارد:

function addToCart(product) {
// ...
}

اگر این Function فقط در همان Module استفاده شود، نیازی نیست در اختیار بخش دیگری قرار گیرد.

اما اگر بخش دیگری از Application به آن نیاز داشته باشد، باید آن را Export کنیم:

export function addToCart(product) {
// ...
}

اکنون addToCart بخشی از Interface قابل استفاده این Module است.

به بیان ساده:

export مشخص می‌کند چه چیزی از داخل Module برای سایر Moduleها قابل استفاده باشد.

این موضوع به همان Boundary که در ابتدا به آن نیاز داشتیم برمی‌گردد.

Module همه جزئیات داخلی خود را در اختیار Application قرار نمی‌دهد.

فقط بخش‌هایی را که لازم است Export می‌کند.

Named Export و Default Export

JavaScript امکان Export کردن یک یا چند مقدار را فراهم می‌کند.

برای مثال:

export const tax = 0.09;

export function calculateTotal(price) {
return price + price * tax;
}

در این حالت، نام Exportها بخشی از Interface Module است.

JavaScript همچنین امکان default export را فراهم می‌کند:

export default function calculateTotal(price) {
return price;
}

تفاوت اصلی این دو در نحوه معرفی و Import کردن آن‌هاست.

اما نکته مهم‌تر برای درک Module این است:

Export کردن یعنی مشخص کردن بخشی از Module که سایر بخش‌های Application اجازه استفاده از آن را دارند.

بنابراین export ابزار ایجاد یک Public Interface برای Module است.

اکنون باید بتوانیم از این Interface در Module دیگری استفاده کنیم.

import؛ استفاده از قابلیت یک Module دیگر

فرض کنید addToCart در Module مربوط به Cart Export شده است.

Module دیگری مانند UI می‌تواند آن را Import کند:

import { addToCart } from './cart.js';

اکنون UI می‌تواند از قابلیت ارائه‌شده توسط Cart Module استفاده کند.

رابطه میان دو Module را می‌توان این‌گونه دید:

Cart Module
│
│ export
↓
addToCart
│
│ import
↓
UI Module

این ارتباط مهم است، زیرا Dependency میان دو بخش Application را صریح می‌کند.

UI دیگر لازم نیست بداند Cart چگونه کار می‌کند.

فقط می‌داند Cart Module قابلیت مشخصی به نام addToCart ارائه می‌دهد.

بنابراین:

import { addToCart } from './cart.js';

فقط یک Syntax نیست.

این خط بیان می‌کند:

این Module برای انجام مسئولیت خود به قابلیت مشخصی از Module دیگری وابسته است.

این وابستگی صریح، یکی از ویژگی‌های مهم معماری Module-based است.

Module Scope؛ هر Module یک Boundary دارد

تا اینجا یک مسئله هنوز باقی مانده است.

اگر Module فقط یک فایل جدا باشد، آیا Variableها و Functionهای آن همچنان در Global Scope قرار می‌گیرند؟

خیر.

یکی از ویژگی‌های مهم ES Modules این است که Code داخل Module دارای Module Scope است.

برای مثال:

const cart = [];

function addToCart(product) {
cart.push(product);
}

export { addToCart };

در اینجا cart بخشی از Implementation داخلی Module است.

Module دیگر نمی‌تواند صرفاً با نام:

cart

به آن دسترسی پیدا کند.

برای دسترسی به قابلیت‌های Module باید آن‌ها به‌صورت مشخص Export و سپس Import شوند.

در نتیجه Module Scope به ایجاد همان Boundary کمک می‌کند که در ابتدای فصل به آن نیاز داشتیم.

می‌توانیم مدل ذهنی زیر را در نظر بگیریم:

Application
│
├── Module A
│   ├── Private Implementation
│   └── Exported Interface
│
├── Module B
│   ├── Private Implementation
│   └── Exported Interface
│
└── Module C
├── Private Implementation
└── Exported Interface

هر Module یک فضای مستقل برای Code خود دارد.

ارتباط میان Moduleها نیز از طریق Interface مشخص انجام می‌شود.

بنابراین Module سه مسئله‌ای را که در ابتدای فصل داشتیم به هم متصل می‌کند:

Large Application
↓
Global Scope Problem
↓
Module Boundary
↓
Encapsulation
↓
Controlled Communication

اما هنوز یک سؤال باقی می‌ماند.

آیا تمام Moduleها باید از ابتدای اجرای Application بارگذاری شوند؟

در یک Application بزرگ، ممکن است همیشه چنین چیزی مطلوب نباشد.

مثلاً ممکن است یک بخش از Application فقط زمانی مورد نیاز باشد که کاربر وارد یک صفحه خاص شود.

در این شرایط به روش دیگری برای بارگذاری Module نیاز داریم.

اینجاست که Dynamic import وارد می‌شود.

Dynamic import؛ وقتی Module را در زمان نیاز می‌خواهیم

تا اینجا با import معمولی، Dependency میان Moduleها را به‌صورت مشخص تعریف کردیم:

import { addToCart } from './cart.js';

اما گاهی یک قابلیت فقط در شرایط خاص مورد نیاز است.

برای مثال، فرض کنید Application یک بخش گزارش‌گیری دارد.

ممکن است کاربر اصلاً وارد صفحه Reports نشود.

در چنین شرایطی می‌توان Module مربوط به Reports را فقط هنگام نیاز بارگذاری کرد:

const module = await import('./reports.js');

import() یک Promise برمی‌گرداند.

به همین دلیل می‌توان نتیجه آن را به‌صورت Async دریافت کرد.

const reports = await import('./reports.js');

تفاوت مهم اینجا این است که Module دیگر صرفاً در مسیر ثابت ابتدای Code قرار ندارد.

Application می‌تواند تصمیم بگیرد:

این قابلیت اکنون مورد نیاز است؛ Module مربوط به آن را بارگذاری کن.

این قابلیت به‌خصوص در Applicationهای بزرگ می‌تواند به Lazy Loading و Code Splitting کمک کند.

اما نکته اصلی این فصل همچنان همان مسئله اولیه است:

Dynamic import نیز یک روش برای مدیریت Boundaryهای Application است؛ با این تفاوت که زمان دسترسی به یک Module را نیز کنترل می‌کنیم.

جزئیات مربوط به Build Process و اینکه ابزارهای مدرن چگونه Moduleها را به Bundle یا Chunk تبدیل می‌کنند، موضوع این فصل نیست و در بخش Tooling بررسی خواهد شد.

یک مدل ذهنی کامل از Module

اکنون می‌توانیم تمام مسیر فصل را کنار هم قرار دهیم.

ابتدا Application بزرگ شد.

در نتیجه قرار دادن تمام Code در یک Scope مشترک باعث افزایش وابستگی و کاهش مرزبندی شد.

برای حل این مسئله، Application را به Moduleهای مستقل تقسیم کردیم.

هر Module Responsibility مشخصی دارد.

برای کنترل ارتباط میان Moduleها، Encapsulation اهمیت پیدا کرد.

ES Modules این ساختار را به‌صورت استاندارد در JavaScript فراهم می‌کند.

export مشخص می‌کند Module چه چیزی ارائه می‌دهد.

import مشخص می‌کند Module به چه قابلیت‌هایی نیاز دارد.

Module Scope از انتشار مستقیم Implementation داخلی جلوگیری می‌کند.

و Dynamic import() زمانی مفید است که بخواهیم یک Module را فقط در زمان نیاز بارگذاری کنیم.

بنابراین Concept Flow فصل به این شکل کامل می‌شود:

Large Application
↓
Global Scope Problem
↓
Module
↓
Encapsulation
↓
ES Modules
↓
export
↓
import
↓
Module Scope
↓
Dynamic import

Module را نباید صرفاً به‌عنوان «یک فایل JavaScript» در نظر گرفت.

مدل ذهنی دقیق‌تر این است:

Module یک Boundary برای سازمان‌دهی Responsibility، پنهان کردن Implementation داخلی و تعریف ارتباط مشخص میان بخش‌های Application است.

Best Practices
1. Module را بر اساس Responsibility طراحی کنید

هر Module باید مسئولیت مشخصی داشته باشد.

مثلاً:

user.js
cart.js
product.js
api.js

نام فایل به‌تنهایی معماری مناسب ایجاد نمی‌کند؛ Responsibility پشت آن مهم است.

2. فقط Interface مورد نیاز را Export کنید

هر چیزی که داخل Module قرار دارد نباید الزاماً Public باشد.

Implementation داخلی را تا حد امکان داخل Boundary نگه دارید.

3. Dependencyها را صریح نگه دارید

اگر Module به قابلیت Module دیگری نیاز دارد، آن Dependency را با import مشخص کنید.

این کار رابطه میان بخش‌های Application را قابل مشاهده‌تر می‌کند.

4. از Module برای تقسیم واقعی Responsibility استفاده کنید

تقسیم یک فایل بزرگ به چند فایل کوچک، بدون ایجاد Boundary منطقی، به‌تنهایی معماری Module-based ایجاد نمی‌کند.

5. Dynamic import را برای موارد مناسب استفاده کنید

Dynamic import زمانی ارزشمند است که بارگذاری یک قابلیت بتواند به زمان نیاز به آن قابلیت موکول شود.

Common Mistakes
اشتباه اول: Module را فقط یک فایل بدانیم

هر فایل JavaScript الزاماً از نظر معماری یک Module خوب نیست.

Module باید Boundary و Responsibility مشخص داشته باشد.

اشتباه دوم: همه چیز را Export کنیم

اگر تمام Implementation داخلی را Export کنیم، Boundary Module ضعیف می‌شود.

هدف Encapsulation این است که فقط Interface لازم در اختیار دیگران قرار گیرد.

اشتباه سوم: import را فقط یک Syntax بدانیم

import علاوه بر Syntax، یک Dependency صریح میان Moduleها ایجاد می‌کند.

اشتباه چهارم: Dynamic import را جایگزین همه Importها بدانیم

Dynamic import برای بارگذاری در زمان نیاز است.

برای Dependencyهایی که Application از ابتدا به آن‌ها نیاز دارد، Import معمولی مناسب‌تر است.

اشتباه پنجم: Module را با Build Tool اشتباه بگیریم

ES Modules بخشی از قابلیت استاندارد JavaScript هستند.

Bundlerها و Build Toolها موضوع متفاوتی هستند و در مراحل بعدی بررسی می‌شوند.

Summary

یک Application بزرگ نمی‌تواند برای همیشه تمام Logic خود را در یک Scope مشترک نگه دارد.

با افزایش Code، مسئله اصلی فقط تعداد خطوط نیست؛ بلکه نبودن Boundary میان بخش‌های مختلف Application است.

Module این امکان را فراهم می‌کند که Application را به واحدهای مستقل‌تر با Responsibility مشخص تقسیم کنیم.

Encapsulation کمک می‌کند Implementation داخلی یک Module از Interface آن جدا شود.

ES Modules این معماری را با export و import در اختیار JavaScript قرار می‌دهد.

export مشخص می‌کند Module چه چیزی ارائه می‌دهد.

import مشخص می‌کند Module از چه قابلیت‌هایی استفاده می‌کند.

Module Scope باعث می‌شود Code هر Module در Boundary خودش قرار داشته باشد.

در نهایت، Dynamic import() امکان بارگذاری یک Module را در زمان نیاز فراهم می‌کند.

Key Takeaways
Module یک Boundary برای سازمان‌دهی Code است، نه صرفاً یک فایل.
هدف Module کاهش وابستگی و مشخص کردن Responsibility است.
Encapsulation میان Implementation داخلی و Interface قابل استفاده تفاوت ایجاد می‌کند.
ES Modules سیستم استاندارد JavaScript برای Moduleها است.
export قابلیت‌های قابل دسترس یک Module را مشخص می‌کند.
import Dependency میان Moduleها را صریح می‌کند.
Module دارای Scope مستقل از Global Scope است.
Dynamic import() امکان بارگذاری Module در زمان نیاز را فراهم می‌کند.
یک Application خوب Moduleهای خود را بر اساس Responsibility و Boundary طراحی می‌کند.
Technical Interview
Junior Level
سؤال 1

Module چیست؟

پاسخ:

Module یک واحد مستقل از Code است که Responsibility و Boundary مشخصی دارد و می‌تواند بخشی از قابلیت‌های خود را برای استفاده سایر بخش‌های Application ارائه کند.

سؤال 2

تفاوت export و import چیست؟

پاسخ:

export مشخص می‌کند چه چیزی از یک Module در اختیار سایر Moduleها قرار گیرد و import مشخص می‌کند یک Module از چه قابلیت‌هایی استفاده می‌کند.

سؤال 3

آیا Module همان فایل JavaScript است؟

پاسخ:

خیر. فایل یک واحد فیزیکی است، اما Module یک Boundary منطقی برای سازمان‌دهی Code و ارتباط میان بخش‌های Application است.

Mid-Level
سؤال 1

چرا Module به Encapsulation کمک می‌کند؟

پاسخ:

زیرا Code داخل Module دارای Boundary و Scope مستقل است و فقط قابلیت‌هایی که Export شده‌اند می‌توانند به‌صورت مشخص در اختیار Moduleهای دیگر قرار گیرند.

سؤال 2

چرا import برای معماری Application مهم است؟

پاسخ:

زیرا Dependency میان Moduleها را به‌صورت صریح مشخص می‌کند و نشان می‌دهد یک بخش از Application برای انجام Responsibility خود به چه قابلیت‌هایی وابسته است.

سؤال 3

Dynamic import چه تفاوتی با Import معمولی دارد؟

پاسخ:

Import معمولی برای Dependencyهایی استفاده می‌شود که در ساختار Module مشخص هستند، در حالی که Dynamic import() امکان بارگذاری Module را در زمان نیاز فراهم می‌کند و یک Promise برمی‌گرداند.

Senior Level
سؤال 1

آیا تقسیم Application به Moduleهای بیشتر همیشه معماری بهتری ایجاد می‌کند؟

پاسخ:

خیر. هدف Module ایجاد Boundary و جداسازی Responsibility است، نه افزایش تعداد فایل‌ها. Moduleهای بیش از حد کوچک می‌توانند Dependency و پیچیدگی ارتباطات را افزایش دهند.

سؤال 2

چرا Module Boundary برای Maintainability مهم است؟

پاسخ:

زیرا Boundary مشخص می‌کند هر بخش چه مسئولیتی دارد، چه Implementationی داخلی است و چه Interfaceی در اختیار سایر بخش‌ها قرار می‌گیرد. این جداسازی اثر تغییرات را محدودتر و ساختار Application را قابل مدیریت‌تر می‌کند.

سؤال 3

Dynamic import در معماری Application چه نقشی دارد؟

پاسخ:

Dynamic import امکان به‌تعویق انداختن بارگذاری یک Module تا زمان نیاز را فراهم می‌کند. این قابلیت برای قابلیت‌های غیرضروری در Initial Load و سناریوهایی مانند Lazy Loading مفید است.

Golden Answers

Module چیست؟

Module یک Boundary برای سازمان‌دهی Responsibility و کنترل ارتباط میان بخش‌های مختلف Application است.

چرا Module به Encapsulation کمک می‌کند؟

چون Implementation داخلی را در یک Scope مستقل قرار می‌دهد و فقط Interface مورد نیاز را در اختیار سایر Moduleها قرار می‌دهد.

نقش export چیست؟

export مشخص می‌کند چه قابلیت‌هایی از یک Module Public Interface آن باشند.

نقش import چیست؟

import Dependency میان Moduleها را صریح می‌کند و امکان استفاده از قابلیت‌های Export شده را فراهم می‌کند.

Dynamic import چیست؟

Dynamic import() روشی برای بارگذاری یک Module در زمان نیاز است و نتیجه آن یک Promise است.

Conclusion

در Applicationهای کوچک ممکن است قرار دادن Logic در یک Scope مشترک قابل تحمل باشد.

اما با رشد Application، مسئله اصلی دیگر فقط مقدار Code نیست.

بخش‌های مختلف Application باید Responsibility، Boundary و Interface مشخص داشته باشند.

Module این ساختار را ایجاد می‌کند.

ES Modules نیز ابزار استاندارد JavaScript برای تعریف این ارتباط هستند:

Module
↓
Encapsulation
↓
export
↓
import
↓
Module Scope
↓
Dynamic import

با این حال، ES Modules تنها سیستم Moduleای نیستند که در اکوسیستم JavaScript با آن روبه‌رو می‌شویم.

در پروژه‌های مختلف، به‌خصوص در محیط‌های Server و پروژه‌های قدیمی‌تر، ممکن است با سیستم دیگری به نام CommonJS مواجه شویم.

پس سؤال طبیعی بعدی این است:

ES Modules و CommonJS چه تفاوتی دارند و چرا هر دو در اکوسیستم JavaScript وجود دارند؟