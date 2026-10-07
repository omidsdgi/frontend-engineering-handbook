Chapter 73 — Parcel
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

نقش یک Build Tool را در یک پروژه JavaScript توضیح دهید.
توضیح دهید Parcel چگونه یک Project را از Entry Point دریافت می‌کند.
نقش Parcel را در Development و Production تحلیل کنید.
توضیح دهید Assetهای پروژه چگونه در فرآیند Build پردازش می‌شوند.
مفهوم Bundling را در جایگاه صحیح آن در Build Process درک کنید.
تفاوت فرآیند Development و Production Build را تشخیص دهید.
توضیح دهید Optimization چرا بخشی از Production Build است.
جایگاه Parcel را در Build Process یک پروژه JavaScript مشخص کنید.
Core Question

Parcel چگونه فرآیند Development و Production Build را برای یک پروژه JavaScript مدیریت می‌کند؟

در فصل 72 دیدیم که Build Process می‌تواند به‌صورت کلی چنین مسیری داشته باشد:

Source Code
↓
Dependencies
↓
Modules
↓
Transformation
↓
Bundling
↓
Optimization
↓
Production Build

اما این سؤال باقی می‌ماند:

چه ابزاری این مراحل را در یک پروژه واقعی اجرا و هماهنگ می‌کند؟

در این فصل با Parcel به‌عنوان یک Build Tool این فرآیند را از دید عملی دنبال می‌کنیم.

جریان فصل:

Project
↓
Entry
↓
Parcel
↓
Development Server
↓
Asset Processing
↓
Bundling
↓
Optimization
↓
Production Build
مقدمه

فرض کنید یک پروژه JavaScript داریم.

پروژه ممکن است شامل چندین فایل باشد:

src/
├── index.html
├── js/
│   ├── app.js
│   └── utils.js
├── css/
│   └── style.css
└── images/
└── logo.png

در نگاه اول، می‌توانیم فایل index.html را باز کنیم و JavaScript را اجرا کنیم.

اما یک پروژه واقعی معمولاً فقط شامل یک فایل JavaScript ساده نیست.

فایل‌های مختلف به یکدیگر وابسته‌اند. CSS، JavaScript، Image و سایر Assetها باید در کنار هم مدیریت شوند و نسخه‌ای مناسب برای Development یا Production آماده شود.

در فصل قبل دیدیم که این فرآیند را Build Process می‌نامیم.

اکنون مسئله مشخص‌تر می‌شود:

چه چیزی این فرآیند را برای ما اجرا می‌کند؟

اینجاست که Build Tool وارد می‌شود.

یکی از Build Toolهایی که می‌توان برای این کار استفاده کرد Parcel است.

از Project به Entry Point

Parcel نمی‌تواند بدون دانستن نقطه شروع پروژه فرآیند Build را آغاز کند.

ابتدا باید مشخص شود که Build از کجا شروع می‌شود.

این نقطه را Entry یا Entry Point می‌نامیم.

برای یک Web Application ساده، ممکن است Entry این فایل باشد:

src/index.html

مثلاً می‌توانیم Parcel را با این Entry اجرا کنیم:

npx parcel src/index.html

در اینجا:

src/index.html

نقطه شروع Build است.

Parcel از این نقطه شروع می‌کند و ارتباط فایل‌های موردنیاز پروژه را دنبال می‌کند.

بنابراین اولین رابطه مهم این است:

Project
↓
Entry
↓
Parcel

اما Parcel با دریافت Entry دقیقاً چه کاری انجام می‌دهد؟

Parcel؛ Build Process در عمل

در فصل قبل Build Process را به‌عنوان مجموعه‌ای از مراحل دیدیم.

اکنون Parcel همان ایده را در یک پروژه واقعی اجرا می‌کند.

Parcel مسئولیت‌های مختلفی را در فرآیند Build بر عهده می‌گیرد؛ از جمله:

اجرای Development Server
پردازش Assetها
ایجاد Bundleها
آماده‌سازی Production Build
انجام Optimizationهای لازم برای Production

بنابراین Parcel را نباید صرفاً یک ابزار برای اجرای JavaScript بدانیم.

نقش آن گسترده‌تر است:

Parcel یک Build Tool است که بخش‌های مختلف فرآیند Build را از دریافت Entry تا تولید خروجی قابل استفاده در Development و Production هماهنگ می‌کند.

اما در زمان توسعه، هنوز یک نیاز مهم وجود دارد.

ما نمی‌خواهیم بعد از هر تغییر، پروژه را به‌صورت دستی Build کنیم.

پس سؤال بعدی شکل می‌گیرد:

هنگام Development چگونه نتیجه تغییرات را سریع مشاهده کنیم؟

Development Server

در زمان توسعه، Developer دائماً کد را تغییر می‌دهد.

اگر برای مشاهده هر تغییر مجبور باشیم یک Production Build کامل ایجاد کنیم، فرآیند توسعه کند و غیرضروری خواهد شد.

Parcel برای این مرحله می‌تواند Development Server را اجرا کند.

مثلاً:

npx parcel src/index.html

Parcel یک Server توسعه اجرا می‌کند و Application را از طریق آن در اختیار Developer قرار می‌دهد.

در این حالت Developer می‌تواند:

Edit Code
↓
Save
↓
Development Server
↓
Updated Application

را تجربه کند.

Parcel در Development می‌تواند تغییرات را سریع‌تر در اختیار Developer قرار دهد و فرآیند توسعه را از Production Build جدا کند.

اما Application فقط شامل JavaScript نیست.

در پروژه واقعی Assetهای دیگری نیز وجود دارند.

وقتی Project فقط JavaScript نیست

فرض کنید index.html به یک فایل CSS و یک Image وابسته است:

<link rel="stylesheet" href="./style.css">

و:

<img src="./logo.png" alt="Logo">

در این حالت پروژه شامل چند نوع Asset است:

HTML
CSS
JavaScript
Image

Build Tool باید بتواند این Assetها را در فرآیند Build در نظر بگیرد.

اینجاست که Asset Processing اهمیت پیدا می‌کند.

Parcel Entry را دریافت می‌کند و وابستگی‌های مرتبط با آن را بررسی می‌کند.

در نتیجه فرآیند از یک فایل منفرد فراتر می‌رود:

Entry
↓
Referenced Assets
↓
Asset Processing

هر Asset باید به شکلی مناسب برای ادامه Build آماده شود.

اما هنوز یک مسئله باقی مانده است.

اگر Application از چندین Module و Asset تشکیل شده باشد، آیا باید تمام این فایل‌ها را همان‌طور که هستند به Browser تحویل دهیم؟

معمولاً پاسخ این است که نه.

از Assetهای متعدد به Bundle

یک Application واقعی می‌تواند شامل تعداد زیادی Module و Asset باشد.

برای مثال:

app.js
↓
utils.js
↓
api.js

اگر این ساختار را بدون هیچ مرحله‌ای مستقیماً برای Production ارسال کنیم، مدیریت خروجی و بهینه‌سازی آن دشوارتر خواهد شد.

Build Process بنابراین نیاز دارد خروجی مناسب‌تری ایجاد کند.

اینجاست که Bundling وارد می‌شود.

Bundling یعنی Build Tool منابع موردنیاز Application را در خروجی‌های مناسب برای اجرا سازمان‌دهی کند.

در نتیجه مسیر به این شکل ادامه پیدا می‌کند:

Entry
↓
Asset Processing
↓
Bundling

Parcel این فرآیند را برای Application انجام می‌دهد.

نکته مهم این است که Bundling هدف نهایی Build نیست.

پس از ایجاد Bundle، هنوز باید مشخص کنیم این خروجی قرار است برای Development استفاده شود یا Production.

در Development سرعت و تجربه Developer اهمیت زیادی دارد.

اما در Production اولویت تغییر می‌کند.

Production نیاز متفاوتی دارد

وقتی Application آماده انتشار است، دیگر هدف اصلی سرعت نوشتن و آزمایش کد نیست.

هدف این است که خروجی نهایی تا حد امکان برای اجرای واقعی مناسب باشد.

در این مرحله Build Tool می‌تواند فرآیندهایی مانند Optimization را انجام دهد.

برای مثال، خروجی Production ممکن است نیاز داشته باشد:

حجم کمتری داشته باشد.
کدهای غیرضروری حذف شوند.
فایل‌ها به شکل مناسب‌تری خروجی داده شوند.
منابع برای تحویل به Browser آماده‌تر باشند.

بنابراین مسیر Build اکنون کامل‌تر می‌شود:

Asset Processing
↓
Bundling
↓
Optimization

Optimization یک مرحله تزئینی نیست.

دلیل وجود آن به تفاوت میان Source Code و Production Output برمی‌گردد.

Source Code برای انسان نوشته می‌شود تا خوانا و قابل نگهداری باشد.

Production Output برای اجرا و انتقال کارآمدتر به کاربر آماده می‌شود.

به همین دلیل Build Tool نباید صرفاً Source Code را کپی کند؛ بلکه باید خروجی مناسب Production تولید کند.

Production Build

اکنون می‌توانیم تفاوت دو مسیر را بهتر ببینیم.

در Development:

Project
↓
Entry
↓
Parcel
↓
Development Server
↓
Fast Development

اما در Production:

Project
↓
Entry
↓
Parcel
↓
Asset Processing
↓
Bundling
↓
Optimization
↓
Production Build

برای مثال، Parcel می‌تواند با دستور build یک Production Build ایجاد کند:

npx parcel build src/index.html

در این حالت هدف دیگر اجرای سریع Development نیست.

هدف تولید خروجی نهایی Application است.

بنابراین یک Build Tool باید بتواند دو نیاز متفاوت را پاسخ دهد:

Development
→ Fast Feedback

Production
→ Optimized Output

این تفاوت یکی از مهم‌ترین نکاتی است که هنگام کار با Build Tool باید در ذهن داشته باشیم.

Parcel در یک نگاه

اکنون اگر مسیر فصل را از ابتدا دنبال کنیم، نقش Parcel واضح‌تر می‌شود.

ما یک Project داریم.

برای Build باید یک Entry مشخص شود.

Parcel از Entry شروع می‌کند و منابع موردنیاز را در فرآیند Build وارد می‌کند.

در Development، Development Server امکان اجرای سریع Application و مشاهده تغییرات را فراهم می‌کند.

در ادامه، Assetها پردازش می‌شوند و منابع پروژه در قالب Bundleهای مناسب سازمان‌دهی می‌شوند.

در Production، این خروجی‌ها باید Optimization شوند تا Production Build ایجاد شود.

بنابراین:

Project
↓
Entry
↓
Parcel
↓
Development Server
↓
Asset Processing
↓
Bundling
↓
Optimization
↓
Production Build

این دقیقاً همان Build Process فصل قبل است، اما اکنون آن را با یک ابزار واقعی مشاهده می‌کنیم.

Parcel جایگزین Build Process نیست

یک سوءبرداشت رایج این است که:

Parcel همان Build Process است.

خیر.

Build Process مفهومی است که مراحل آماده‌سازی Application را توصیف می‌کند.

Parcel ابزاری است که بخشی از این فرآیند را برای ما اجرا و هماهنگ می‌کند.

به بیان ساده:

Build Process
↓
Concept / Process
↓
Parcel
↓
Tool that performs and coordinates the process

این تفاوت مهم است.

اگر فردا به‌جای Parcel از Build Tool دیگری استفاده کنیم، مفهوم Build Process همچنان وجود دارد.

ابزار تغییر می‌کند؛ نیاز مهندسی ثابت می‌ماند.

Parcel چه مشکلی را حل کرد؟

در این فصل از یک مسئله ساده شروع کردیم:

یک Project واقعی فقط یک فایل JavaScript نیست.

Project شامل Entry، Moduleها و Assetهای مختلف است.

برای توسعه به یک محیط مناسب نیاز داریم.

برای انتشار نیز به یک Production Output مناسب نیاز داریم.

Parcel این مراحل را در قالب یک Build Tool به هم متصل می‌کند:

Project
↓
Entry
↓
Development
↓
Processing
↓
Bundling
↓
Optimization
↓
Production

بنابراین ارزش اصلی Parcel در یک دستور یا یک فایل Configuration خلاصه نمی‌شود.

ارزش آن در هماهنگ کردن مراحل مختلف Build Process است.

اما اکنون یک سؤال مهم‌تر باقی مانده است.

Parcel می‌تواند منابع پروژه را پردازش و Bundle کند، اما اگر Source Code ما از Syntaxهای مدرن JavaScript استفاده کند و محیط هدف همه آن Syntaxها را پشتیبانی نکند، چه اتفاقی می‌افتد؟

این مسئله ما را به مفهوم دیگری می‌رساند:

چگونه Syntax مدرن JavaScript را برای محیط‌های هدف مختلف Transform کنیم؟

پاسخ این سؤال در فصل بعد بررسی خواهد شد:

Chapter 74 — Babel

Best Practices
Build Tool را با Build Process یکی ندانید

Build Process یک مفهوم مهندسی است و Parcel یکی از ابزارهایی است که می‌تواند این فرآیند را اجرا کند.

Entry Point را نقطه شروع Build بدانید

قبل از تحلیل رفتار Parcel، مشخص کنید Build از کدام فایل یا Entry شروع می‌شود.

Development و Production را از یکدیگر جدا کنید

هدف Development، Feedback سریع است؛ در حالی که Production به خروجی مناسب و Optimized نیاز دارد.

Source Code را با Production Output یکی ندانید

Source Code برای توسعه و نگهداری نوشته می‌شود.

Production Output برای اجرای نهایی Application آماده می‌شود.

Common Mistakes
تصور اینکه Parcel فقط JavaScript را اجرا می‌کند

Parcel در فرآیند Build با انواع مختلف Assetهای پروژه سروکار دارد.

تصور اینکه Bundling همان کل Build Process است

Bundling فقط یکی از مراحل Build Process است.

تصور اینکه Development Build همان Production Build است

این دو برای اهداف متفاوتی ایجاد می‌شوند و نیازهای متفاوتی دارند.

تصور اینکه Parcel خود مفهوم Build Process است

Build Process یک فرآیند است؛ Parcel یک ابزار برای اجرای و هماهنگ کردن بخش‌های این فرآیند است.

Summary

در یک پروژه واقعی، Source Code از فایل‌ها و Assetهای متعددی تشکیل شده است.

برای تبدیل این Project به خروجی قابل اجرا و قابل انتشار، به Build Process نیاز داریم.

Parcel به‌عنوان یک Build Tool این فرآیند را از یک Entry Point آغاز می‌کند.

در Development می‌تواند Development Server را فراهم کند.

سپس Assetهای پروژه را پردازش می‌کند و آن‌ها را در قالب Bundleهای مناسب سازمان‌دهی می‌کند.

برای Production، خروجی باید Optimization شود و در نهایت Production Build ایجاد شود.

بنابراین مدل ذهنی این فصل چنین است:

Project
↓
Entry
↓
Parcel
↓
Development Server
↓
Asset Processing
↓
Bundling
↓
Optimization
↓
Production Build
Key Takeaways
Build Tool ابزاری برای اجرای و هماهنگ کردن مراحل Build Process است.
Parcel یکی از Build Toolهای JavaScript است.
Build معمولاً از یک Entry Point آغاز می‌شود.
Parcel می‌تواند در Development یک Development Server فراهم کند.
پروژه واقعی فقط JavaScript نیست و Assetهای مختلف باید در Build پردازش شوند.
Bundling یکی از مراحل Build Process است، نه تمام آن.
Production به Optimization و خروجی مناسب انتشار نیاز دارد.
Development و Production اهداف متفاوتی دارند.
Parcel جایگزین مفهوم Build Process نیست؛ بلکه ابزاری برای اجرای آن است.
Technical Interview
Junior
1. Parcel چیست؟

Parcel یک Build Tool است که می‌تواند فرآیند Build یک پروژه JavaScript را از Entry Point آغاز کرده و مراحل مختلفی مانند Asset Processing، Bundling و Production Build را مدیریت کند.

2. Entry Point در Parcel چیست؟

فایلی است که Build از آن شروع می‌شود و Parcel از طریق آن منابع و وابستگی‌های موردنیاز پروژه را دنبال می‌کند.

3. تفاوت Development و Production Build چیست؟

Development برای Feedback سریع و توسعه راحت‌تر طراحی می‌شود؛ Production برای تولید خروجی نهایی و بهینه‌شده Application است.

Mid-Level
4. چرا Build Tool به Development Server نیاز دارد؟

چون در زمان توسعه Developer مرتباً Source Code را تغییر می‌دهد و نیاز دارد نتیجه تغییرات را سریع مشاهده کند، بدون اینکه هر بار یک Production Build کامل ایجاد شود.

5. Bundling چه نقشی در Build Process دارد؟

Bundling منابع و Moduleهای موردنیاز Application را در خروجی‌های مناسب برای اجرای Application سازمان‌دهی می‌کند.

6. چرا Asset Processing بخشی از Build Process است؟

چون یک Application واقعی فقط شامل JavaScript نیست و منابعی مانند HTML، CSS و Image نیز باید در فرآیند Build مدیریت شوند.

Senior
7. آیا Parcel همان Build Process است؟

خیر. Build Process یک فرآیند مفهومی و مهندسی برای تبدیل Source Code به Production Output است؛ Parcel ابزاری است که می‌تواند بخش‌های مختلف این فرآیند را اجرا و هماهنگ کند.

8. چرا نباید Development و Production را یک فرآیند یکسان در نظر گرفت؟

زیرا معیار موفقیت آن‌ها متفاوت است. Development بر Feedback سریع و تجربه Developer تمرکز دارد، در حالی که Production بر خروجی مناسب، کارآمد و آماده انتشار تمرکز می‌کند.

9. رابطه Parcel با Build Process چیست؟

Parcel یک لایه اجرایی روی Build Process است. Project و Entry را دریافت می‌کند و مراحلی مانند Asset Processing، Bundling و Optimization را برای تولید خروجی Development یا Production مدیریت می‌کند.

Golden Answers
Parcel چیست؟

Parcel یک Build Tool است که فرآیند Build یک پروژه JavaScript را از Entry Point آغاز می‌کند و مراحلی مانند Asset Processing، Bundling و Optimization را برای Development و Production مدیریت می‌کند.

Bundling چیست؟

Bundling فرآیندی است که منابع و Moduleهای موردنیاز Application را در خروجی‌های مناسب برای اجرای Application سازمان‌دهی می‌کند.

تفاوت Development و Production Build چیست؟

Development برای Feedback سریع در زمان توسعه طراحی می‌شود، در حالی که Production Build برای تولید خروجی نهایی و Optimized جهت انتشار Application ایجاد می‌شود.

آیا Parcel همان Build Process است؟

خیر. Build Process یک فرآیند مهندسی است و Parcel یک ابزار برای اجرا و هماهنگ کردن بخش‌های مختلف آن فرآیند است.

Conclusion

در فصل 72، Build Process را به‌عنوان یک فرآیند شناختیم:

Source Code
↓
Dependencies
↓
Modules
↓
Transformation
↓
Bundling
↓
Optimization
↓
Production Build

در این فصل دیدیم که چگونه یک Build Tool مانند Parcel این فرآیند را در یک پروژه واقعی مدیریت می‌کند.

از Project و Entry شروع کردیم، به Development Server رسیدیم، سپس Asset Processing و Bundling را دیدیم و در نهایت به Optimization و Production Build رسیدیم.

اما Parcel یک سؤال جدید ایجاد می‌کند.

اگر پروژه از Modern JavaScript Syntax استفاده کند، آیا تمام محیط‌های هدف می‌توانند این Syntax را اجرا کنند؟

اگر پاسخ منفی باشد، قبل از رسیدن به Production باید Source Code را برای محیط هدف Transform کنیم.

این نیاز، ما را به مفهوم بعدی می‌رساند:

Babel.