Chapter 49 — Intl and Internationalization
اهداف فصل

پس از مطالعه این فصل، خواننده باید بتواند:

مفهوم Locale و نقش آن در نمایش داده‌ها را توضیح دهد.
تفاوت Internationalization و Localization را تشخیص دهد.
نقش Intl را در JavaScript توضیح دهد.
Numberها را متناسب با Locale قالب‌بندی کند.
Currency و Percentage را با Formatter مناسب نمایش دهد.
Date و Time را بر اساس Locale و Time Zone قالب‌بندی کند.
Relative Time را به‌شکل Locale-aware نمایش دهد.
Text را بر اساس قواعد زبانی یک Locale مقایسه و مرتب کند.
داده خام Application را از نمایش Localized آن جدا نگه دارد.
Core Question

اگر یک Application قرار باشد برای کاربران کشورهای مختلف کار کند، چگونه باید Number، Date، Currency و سایر داده‌ها را به شکل مناسب برای هر کاربر نمایش دهیم؟

مقدمه

تا اینجا Number و Date را به‌عنوان داده‌هایی که JavaScript می‌تواند با آن‌ها کار کند بررسی کرده‌ایم. اما زمانی که این داده‌ها وارد رابط کاربری می‌شوند، یک مسئله جدید ظاهر می‌شود.

فرض کنید قیمت یک Recipe یا Product برابر با این مقدار باشد:

const price = 1234567.89;

برای JavaScript این فقط یک Number است.

اما کاربر قرار نیست 1234567.89 را به‌عنوان یک Number خام ببیند. Application باید تصمیم بگیرد این مقدار چگونه نمایش داده شود.

برای یک Locale ممکن است نتیجه شبیه این باشد:

1,234,567.89

و برای Locale دیگری:

1.234.567,89

مقدار تغییر نکرده است. فقط نحوه نمایش مقدار تغییر کرده است.

همین مسئله برای Date، Currency، Percentage، Unit و حتی ترتیب قرار گرفتن کلمات نیز وجود دارد.

بنابراین قبل از اینکه سراغ APIهای JavaScript برویم، باید مفهومی را بشناسیم که این تفاوت‌ها را توضیح می‌دهد: Locale.

Locale

Locale مجموعه‌ای از قواعد و ترجیحات مرتبط با یک زبان و منطقه است که نحوه نمایش و تفسیر برخی داده‌ها را مشخص می‌کند.

برای مثال:

en-US
en-GB
de-DE
fr-FR
fa-IR

هر کدام یک Locale هستند.

نکته مهم این است که Locale دقیقاً معادل Language نیست.

ممکن است دو Locale از یک زبان استفاده کنند، اما در نحوه نمایش Number، Date یا Currency با یکدیگر تفاوت داشته باشند.

بنابراین وقتی Application می‌خواهد چیزی را برای کاربر نمایش دهد، فقط دانستن زبان کافی نیست؛ باید بداند این داده برای کدام Locale نمایش داده می‌شود.

اینجا اولین بخش Concept Flow شکل می‌گیرد:

Locale
↓
Internationalization

اگر Application از ابتدا فقط برای یک Locale خاص نوشته شده باشد، بسیاری از قواعد نمایش ممکن است مستقیماً داخل Code قرار بگیرند. برای مثال:

const price = `$${amount}`;

این Code فرض کرده است که Symbol مربوط به Currency باید $ باشد و نحوه نمایش Number نیز همان قالب خاص است.

اما Application بین‌المللی نمی‌تواند چنین فرضی داشته باشد.

Internationalization

Internationalization که معمولاً به‌صورت i18n نوشته می‌شود، به طراحی Application به‌گونه‌ای اشاره دارد که بتواند برای Localeهای مختلف آماده باشد.

در اینجا هدف این نیست که تمام متن‌های Application را ترجمه کنیم.

هدف این است که Application به یک زبان یا منطقه خاص وابسته نباشد.

برای مثال، اگر Application قیمت را به‌صورت Number نگه دارد:

const price = 1234.5;

این داده مستقل از Locale است.

اما هنگام نمایش، Locale وارد مسئله می‌شود:

Price
↓
Locale
↓
Formatting Rules
↓
Displayed Value

این تفکیک اهمیت زیادی دارد، زیرا اگر مقدار اصلی را به شکل Localized ذخیره کنیم، Business Logic دیگر با یک داده واقعی و مستقل از Presentation سروکار ندارد.

به همین دلیل بهتر است:

1234.5

را داده Application در نظر بگیریم و نمایش‌هایی مانند:

1,234.50

را نتیجه Presentation بدانیم.

Internationalization در واقع همین نوع جداسازی و آماده‌سازی Application برای شرایط مختلف است.

اما وقتی بخواهیم یک داده را بر اساس Locale مشخص قالب‌بندی کنیم، به ابزار استاندارد JavaScript نیاز داریم.

Intl

JavaScript برای بسیاری از نیازهای Internationalization یک Namespace استاندارد به نام Intl در اختیار ما قرار می‌دهد.

Intl مجموعه‌ای از APIهای مرتبط با Locale-aware Formatting و مقایسه داده‌هاست.

مسیر اصلی فصل از اینجا روشن می‌شود:

Locale
↓
Internationalization
↓
Intl

به‌جای اینکه قواعد مربوط به زبان و منطقه را خودمان پیاده‌سازی کنیم، می‌توانیم این مسئولیت را به APIهای استاندارد Intl واگذار کنیم.

برای مثال، اگر هدف نمایش یک Number باشد، ابزار مناسب:

Intl.NumberFormat

است.

اگر هدف نمایش Date و Time باشد:

Intl.DateTimeFormat

و اگر هدف نمایش فاصله زمانی باشد:

Intl.RelativeTimeFormat

وجود دارد.

برای مقایسه و مرتب‌سازی Text نیز:

Intl.Collator

در اختیار ماست.

بنابراین Intl یک API واحد برای یک کار مشخص نیست؛ مجموعه‌ای از ابزارهاست که بخش‌های مختلف Presentation وابسته به Locale را پوشش می‌دهد.

Number Formatting

اولین مسئله‌ای که با آن روبه‌رو می‌شویم Number است.

فرض کنید Application یک قیمت را به‌صورت زیر دریافت کرده است:

const price = 1234567.89;

اگر آن را مستقیماً در UI قرار دهیم، هیچ Formatting خاصی روی آن انجام نشده است.

برای تبدیل این Number به یک نمایش Locale-aware از Intl.NumberFormat استفاده می‌کنیم:

const formatter = new Intl.NumberFormat('de-DE');

formatter.format(price);

در اینجا نکته مهم خود عدد نیست.

1234567.89 همچنان همان Number است.

Formatter فقط یک نمایش مناسب برای Locale مشخص تولید می‌کند.

این همان تفاوتی است که باید در Applicationهای واقعی حفظ شود:

Application Data
↓
Number
↓
Intl.NumberFormat
↓
Localized String

پس Intl.NumberFormat داده اصلی را تغییر نمی‌دهد؛ بلکه Representation مناسب آن را تولید می‌کند.

Currency Formatting

در Applicationهایی مانند فروشگاه، Booking System یا حتی یک Recipe Application که قیمت محصول یا سرویس را نمایش می‌دهد، Number معمولاً یک Currency است.

در این حالت فقط جدا کردن ارقام کافی نیست.

باید مشخص کنیم:

Currency چیست؟
چگونه نمایش داده شود؟
چند رقم اعشار داشته باشد؟
Symbol در کجا قرار گیرد؟
قواعد مربوط به Locale چه باشند؟

Intl.NumberFormat این اطلاعات را از طریق Options دریافت می‌کند:

const formatter = new Intl.NumberFormat('en-US', {
style: 'currency',
currency: 'USD'
});

formatter.format(1499.99);

در اینجا مقدار:

1499.99

داده Application است.

اما نتیجه format() یک String مناسب برای Presentation خواهد بود.

این موضوع نشان می‌دهد چرا ساختن String به‌صورت دستی راه‌حل مناسبی برای Applicationهای Internationalized نیست.

برای مثال:

`$${price}`

فقط یک قالب خاص را Hard-code می‌کند.

در مقابل:

Number
+
Currency
+
Locale
↓
Intl.NumberFormat

مسئولیت Formatting را به سیستم استاندارد Internationalization واگذار می‌کند.

همین Formatter می‌تواند برای Percentage و Unit نیز استفاده شود؛ بنابراین Intl.NumberFormat را نباید صرفاً یک ابزار مربوط به Currency در نظر گرفت.

Date Formatting

در فصل Dates، Date را از زاویه نگهداری و کار با یک مقدار زمانی بررسی کردیم.

اما همان‌طور که Number یک داده خام و یک نمایش قابل مشاهده دارد، Date نیز همین تفاوت را دارد.

فرض کنید:

const date = new Date();

این Date یک مقدار زمانی است.

اما اینکه آن را چگونه به کاربر نمایش دهیم، مسئله دیگری است.

برای این کار از Intl.DateTimeFormat استفاده می‌کنیم:

const formatter = new Intl.DateTimeFormat('en-US');

formatter.format(date);

اینجا دوباره همان الگو را می‌بینیم:

Date
+
Locale
↓
Intl.DateTimeFormat
↓
Localized Date

بنابراین Date و Date Formatting دو مفهوم متفاوت هستند.

Date با داده زمانی سروکار دارد؛ Intl.DateTimeFormat با نحوه نمایش آن داده.

Date و Time Formatting

گاهی فقط Date کافی نیست و Application باید Time را نیز نمایش دهد.

در این حالت Options مشخص می‌کنند چه بخش‌هایی از مقدار زمانی نمایش داده شوند:

const formatter = new Intl.DateTimeFormat('en-US', {
dateStyle: 'medium',
timeStyle: 'short'
});

formatter.format(new Date());

در اینجا دو تصمیم از یکدیگر جدا هستند:

Locale
→ چگونه نمایش داده شود؟

Options
→ چه چیزی نمایش داده شود؟

این تفکیک باعث می‌شود Formatter قابل کنترل‌تر و قابل استفاده مجدد باشد.

اما هنگام کار با Time، یک مفهوم دیگر نیز وارد مسئله می‌شود: Time Zone.

Locale و Time Zone

Locale و Time Zone نباید با یکدیگر اشتباه گرفته شوند.

Locale درباره قواعد Presentation صحبت می‌کند.

Time Zone درباره منطقه زمانی صحبت می‌کند.

برای مثال، ممکن است Application بخواهد یک زمان را برای Locale انگلیسی نمایش دهد، اما Time Zone آن زمان مربوط به Tallinn باشد.

در این شرایط هر دو اطلاعات لازم هستند:

Date
+
Locale
+
Time Zone
↓
Localized Date/Time

برای تعیین Time Zone می‌توان آن را در Options مشخص کرد:

const formatter = new Intl.DateTimeFormat('en-US', {
dateStyle: 'medium',
timeStyle: 'short',
timeZone: 'Europe/Tallinn'
});

بنابراین این دو سؤال متفاوت هستند:

زمان چگونه نمایش داده شود؟

Locale به این سؤال پاسخ می‌دهد.

زمان بر اساس کدام منطقه زمانی نمایش داده شود؟

Time Zone به این سؤال پاسخ می‌دهد.

این تفکیک در Applicationهایی که کاربران در مناطق زمانی مختلف دارند اهمیت زیادی پیدا می‌کند.

Relative Formatting

نمایش Date همیشه به معنای نمایش تاریخ کامل نیست.

در بسیاری از UIها کاربر بیشتر به فاصله زمانی علاقه دارد.

برای مثال، در یک Application ممکن است به‌جای نمایش یک Timestamp کامل، چیزی مانند این نمایش داده شود:

2 days ago

یا:

tomorrow

این نوع نمایش را Relative Time می‌نامیم.

JavaScript برای این کار Intl.RelativeTimeFormat را ارائه می‌کند.

const formatter = new Intl.RelativeTimeFormat('en', {
numeric: 'auto'
});

formatter.format(-1, 'day');

در اینجا Formatter سه قطعه اطلاعات دریافت می‌کند:

Locale
+
Numeric Value
+
Unit

و آن‌ها را به یک Representation مناسب برای کاربر تبدیل می‌کند.

بنابراین:

Time Difference
↓
Intl.RelativeTimeFormat
↓
Localized Relative Time

این موضوع یک بار دیگر مرز میان Logic و Presentation را نشان می‌دهد.

Application ممکن است بداند که مقدار برابر با -1 day است؛ اما اینکه این مقدار چگونه به کاربر نمایش داده شود، مسئولیت Formatter است.

Text و Collation

تا اینجا Number و Date را بررسی کردیم، اما Internationalization فقط به اعداد و زمان محدود نمی‌شود.

فرض کنید Application فهرستی از نام‌ها دارد و باید آن‌ها را مرتب کند.

ممکن است اولین راه‌حل این باشد:

names.sort();

اما مقایسه Text یک مسئله زبانی است.

حروف و قواعد ترتیب در زبان‌های مختلف الزاماً یکسان نیستند.

این مفهوم را Collation می‌نامیم.

برای Locale-aware Text Comparison می‌توان از Intl.Collator استفاده کرد:

const collator = new Intl.Collator('en');

names.sort(collator.compare);

در اینجا Application از قواعد مقایسه مناسب Locale استفاده می‌کند.

بنابراین مسیر Concept Flow به اینجا می‌رسد:

Locale
↓
Internationalization
↓
Intl
↓
Number Formatting
↓
Date Formatting
↓
Relative Formatting
↓
Collation

در این مرحله Intl دیگر مجموعه‌ای از APIهای جدا از هم به نظر نمی‌رسد؛ بلکه یک ایده واحد پشت همه آن‌ها دیده می‌شود:

داده را مستقل از Presentation نگه می‌داریم و هنگام نمایش، قواعد Locale را وارد می‌کنیم.

Localized UI Data

اکنون می‌توانیم مسئله را از دید Application جمع‌بندی کنیم.

فرض کنید Application این داده‌ها را دارد:

Price
Date
Time Difference
Names

این‌ها داده‌های Application هستند.

اما UI باید نسخه‌ای مناسب برای کاربر تولید کند:

Price
↓
NumberFormat

Date
↓
DateTimeFormat

Time Difference
↓
RelativeTimeFormat

Names
↓
Collator

در نتیجه معماری مفهومی به این شکل است:

Application Data
↓
Locale
↓
Intl
↓
Localized Presentation

این همان نقطه‌ای است که Internationalization از یک مجموعه API به یک اصل طراحی تبدیل می‌شود.

برای مثال، بهتر است Product چنین داده‌ای داشته باشد:

const product = {
name: 'Laptop',
price: 1499.99,
currency: 'USD'
};

نه اینکه Price از ابتدا به شکل یک String نمایشی ذخیره شود:

price: '$1,499.99'

زیرا در حالت اول Application هنوز یک داده قابل پردازش در اختیار دارد و می‌تواند همان داده را برای Localeهای مختلف نمایش دهد.

Intl و Translation یکی نیستند

در اینجا باید یک سوءبرداشت مهم را برطرف کنیم.

استفاده از Intl به این معنی نیست که Application کاملاً Multilingual شده است.

Intl برای بسیاری از مسائل مرتبط با Formatting و Locale-sensitive Operations مناسب است.

اما Translation یک مسئله جداست.

برای مثال:

$1,499.99

یک مسئله Currency Formatting است و Intl.NumberFormat می‌تواند آن را مدیریت کند.

اما:

Add to Cart

یک متن UI است و ترجمه آن مسئله دیگری است.

بنابراین:

Internationalization
├── Number Formatting
├── Date Formatting
├── Currency
├── Relative Time
├── Collation
└── Translation

و Intl بخش مهمی از این مجموعه را پوشش می‌دهد، نه تمام آن را.

Locale در Application

وقتی Application از Localeهای مختلف پشتیبانی می‌کند، بهتر است Locale در بخش‌های مختلف Code به‌صورت پراکنده Hard-code نشود.

برای مثال، اگر در چندین فایل داشته باشیم:

new Intl.NumberFormat('en-US');

و در بخش‌های دیگر:

new Intl.NumberFormat('de-DE');

تغییر Locale به یک کار پراکنده و پرخطا تبدیل می‌شود.

مدل بهتر این است که Application ابتدا Locale موردنظر را مشخص کند:

Application
↓
Resolved Locale
↓
Formatting Layer
↓
Components

در این مدل Component لازم نیست بداند Locale از کجا آمده است.

فقط باید داده را برای نمایش دریافت کند.

این همان جداسازی‌ای است که پیش‌تر در مورد Application Data و Presentation دیدیم.

Best Practices
داده خام را از Presentation جدا کنید

Number، Date و سایر داده‌های Domain را به شکل مناسب برای Logic نگه دارید و Formatting را هنگام Presentation انجام دهید.

از APIهای استاندارد Intl استفاده کنید

برای Number، Currency، Date، Relative Time و Text Comparison، قواعد Locale را دستی بازسازی نکنید.

Locale را متمرکز مدیریت کنید

Locale نباید بدون دلیل در Componentها و Utilityهای مختلف Hard-code شود.

Locale و Time Zone را جدا نگه دارید

این دو پاسخ دو سؤال متفاوت هستند: نحوه نمایش و Context زمانی.

Formatterهای قابل استفاده مجدد ایجاد کنید

اگر چندین مقدار با Configuration یکسان Format می‌شوند، Formatter را می‌توان یک بار ایجاد و دوباره استفاده کرد.

Localized String را به‌عنوان داده اصلی ذخیره نکنید

String نهایی UI معمولاً Presentation Data است، نه Domain Data.

Common Mistakes
Hard-code کردن Currency
`$${price}`

فقط یک قالب خاص را نمایش می‌دهد و برای Localeهای مختلف مناسب نیست.

ذخیره کردن Price به شکل String
price: '$1,499.99'

کار با داده را برای محاسبات و Formatting مجدد دشوار می‌کند.

یکی دانستن Locale و Language

Locale می‌تواند اطلاعات منطقه‌ای بیشتری از صرفاً نام زبان داشته باشد.

یکی دانستن Locale و Time Zone

Locale قواعد Presentation را تعیین می‌کند؛ Time Zone Context زمانی را.

استفاده از Sorting ساده برای همه زبان‌ها

ترتیب Text می‌تواند به قواعد زبانی وابسته باشد؛ در چنین مواردی Intl.Collator ابزار مناسب‌تری است.

تصور اینکه Intl سیستم Translation است

Intl ابزار Internationalization است، اما Translation یک مسئله جداگانه است.

Summary

Internationalization از یک مسئله ساده شروع می‌شود: یک داده واحد ممکن است برای کاربران مختلف به شکل‌های متفاوتی نمایش داده شود.

این تفاوت‌ها با مفهوم Locale توضیح داده می‌شوند.

Application نباید داده را از ابتدا به شکل Localized String نگه دارد. داده باید تا حد امکان مستقل از Presentation باقی بماند و هنگام نمایش بر اساس Locale قالب‌بندی شود.

JavaScript برای این کار Intl را فراهم می‌کند.

Intl.NumberFormat برای Number و مواردی مانند Currency و Percentage استفاده می‌شود.

Intl.DateTimeFormat برای Date و Time Formatting مناسب است و می‌تواند Time Zone را نیز در نظر بگیرد.

Intl.RelativeTimeFormat برای نمایش فاصله زمانی به شکل طبیعی‌تر استفاده می‌شود.

Intl.Collator نیز قواعد مناسب برای مقایسه و مرتب‌سازی Text را فراهم می‌کند.

در نهایت تمام این مفاهیم به یک الگوی واحد می‌رسند:

Application Data
↓
Locale
↓
Intl
↓
Localized Presentation
Key Takeaways
Locale فقط Language نیست؛ مجموعه‌ای از قواعد مرتبط با زبان و منطقه است.
Internationalization یعنی Application را برای Localeهای مختلف آماده کنیم.
Intl ابزار استاندارد JavaScript برای بسیاری از نیازهای Internationalization است.
Intl.NumberFormat برای Number، Currency، Percentage و Unit Formatting استفاده می‌شود.
Intl.DateTimeFormat برای Date و Time Formatting استفاده می‌شود.
Locale و Time Zone دو مفهوم مستقل هستند.
Intl.RelativeTimeFormat برای Relative Time استفاده می‌شود.
Intl.Collator برای Locale-aware Text Comparison و Sorting کاربرد دارد.
Localized String را با Application Data یکی نکنید.
Intl جایگزین سیستم Translation نیست.
Formatting بهتر است در مرز Presentation انجام شود، نه در Business Logic.
Technical Interview
Junior
Intl چیست؟

Intl Namespace استاندارد JavaScript برای قابلیت‌های Internationalization است و APIهایی برای Locale-aware Formatting و مقایسه داده‌ها فراهم می‌کند.

Locale چیست؟

Locale مجموعه‌ای از قواعد مرتبط با زبان و منطقه است که می‌تواند بر نحوه نمایش Number، Date، Currency و Text اثر بگذارد.

Intl.NumberFormat چه کاری انجام می‌دهد؟

یک Number را بر اساس Locale و Options مشخص به یک String مناسب برای Presentation تبدیل می‌کند.

Intl.DateTimeFormat چه کاری انجام می‌دهد؟

Date و Time را بر اساس Locale، Options و در صورت نیاز Time Zone قالب‌بندی می‌کند.

Mid-Level
چرا نباید Currency را دستی Format کنیم؟

زیرا نحوه نمایش Currency فقط Symbol نیست؛ جای Symbol، جداکننده‌ها، تعداد اعشار و سایر قواعد می‌توانند به Locale وابسته باشند.

تفاوت Locale و Time Zone چیست؟

Locale قواعد نمایش را تعیین می‌کند، در حالی که Time Zone مشخص می‌کند زمان بر اساس کدام منطقه زمانی نمایش داده شود.

Intl.RelativeTimeFormat چه مسئله‌ای را حل می‌کند؟

فاصله زمانی را به یک Representation قابل نمایش و Locale-aware تبدیل می‌کند؛ مانند نمایش زمان گذشته یا آینده نسبت به یک نقطه زمانی.

Intl.Collator چه کاربردی دارد؟

برای مقایسه و مرتب‌سازی Text بر اساس قواعد Collation یک Locale استفاده می‌شود.

Senior
چرا باید Application Data را از Localized Presentation جدا کنیم؟

زیرا Application Data باید مستقل از Locale و Presentation قابل استفاده باشد. Localized String به Locale وابسته است و متعلق به لایه Presentation است.

Internationalization چگونه بر طراحی Application اثر می‌گذارد؟

Application باید از Hard-code کردن قواعد مربوط به زبان، Currency، Date و سایر Presentation Rules جلوگیری کند و این قواعد را بر اساس Locale در لایه مناسب اعمال کند.

آیا Intl برای ساخت یک Application کاملاً Multilingual کافی است؟

خیر. Intl بخش مهمی از Locale-aware Formatting و برخی عملیات زبانی را فراهم می‌کند، اما Translation و سایر اجزای سیستم چندزبانه مسائل مستقلی هستند.

Golden Answers

Locale چیست؟

Locale مجموعه‌ای از قواعد زبانی و منطقه‌ای است که بر نحوه نمایش داده‌ها برای کاربر اثر می‌گذارد.

Internationalization چیست؟

Internationalization یعنی Application را به‌گونه‌ای طراحی کنیم که بتواند بدون وابستگی سخت به یک زبان یا منطقه، از Localeهای مختلف پشتیبانی کند.

Intl چیست؟

Intl مجموعه APIهای استاندارد JavaScript برای Internationalization است که امکاناتی مانند Number، Date، Relative Time Formatting و Locale-aware Text Comparison را فراهم می‌کند.

چرا داده خام و Localized Presentation باید جدا باشند؟

چون داده خام بخشی از Application Logic است، اما Localized Presentation به Locale و UI وابسته است.

Locale و Time Zone چه تفاوتی دارند؟

Locale تعیین می‌کند داده چگونه نمایش داده شود؛ Time Zone تعیین می‌کند زمان بر اساس کدام منطقه زمانی تفسیر و نمایش داده شود.

چرا Intl بهتر از Formatting دستی است؟

چون قواعد پیچیده و Locale-dependent را به API استاندارد JavaScript واگذار می‌کند و از Hard-code کردن این قواعد در Application جلوگیری می‌شود.

Conclusion

وقتی Application فقط برای یک کاربر و یک Locale طراحی می‌شود، بسیاری از تفاوت‌های مربوط به نمایش داده‌ها دیده نمی‌شوند.

اما به‌محض اینکه Application باید برای کاربران مختلف کار کند، Number، Date، Currency، Time و حتی ترتیب Text دیگر یک قالب ثابت ندارند.

راه‌حل این نیست که برای هر Locale مجموعه‌ای از Stringها و قواعد دستی ایجاد کنیم.

راه‌حل این است که داده را از نحوه نمایش آن جدا کنیم و Locale را به‌عنوان بخشی از Presentation Context در نظر بگیریم.

در JavaScript این مسیر با Intl ساخته می‌شود:

Locale
↓
Intl
↓
Formatting / Comparison
↓
Localized Presentation

در نتیجه، Number همان Number باقی می‌ماند، Date همان Date باقی می‌ماند و داده Application به یک String وابسته به زبان تبدیل نمی‌شود.

تنها چیزی که تغییر می‌کند، Representation مناسب برای کاربر است.

این همان ایده‌ای است که Internationalization را از مجموعه‌ای از APIها به یک اصل مهم در طراحی Application تبدیل می‌کند:

داده را مستقل نگه دارید؛ نمایش را بر اساس Locale تولید کنید.

این رویکرد با جایگاه فصل در معماری کتاب نیز هماهنگ است؛ جایی که مسیر یادگیری از Numbers → Dates / Intl → Browser Runtime حرکت می‌کند و Internationalization پیش از ورود به Browser Runtime تکمیل می‌شود.