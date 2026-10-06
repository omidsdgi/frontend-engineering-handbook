Chapter 72 — JavaScript Build Process
اهداف فصل

در پایان این فصل، خواننده باید بتواند:

توضیح دهد چرا Source Code یک پروژه مدرن الزاماً همان چیزی نیست که در Production اجرا می‌شود.
نقش Dependencies و Modules را در Build Process توضیح دهد.
دلیل نیاز به Transformation را درک کند.
مفهوم Bundling را توضیح دهد.
هدف Optimization را از Transformation و Bundling تفکیک کند.
مفهوم Production Build را به‌عنوان خروجی نهایی فرآیند Build تحلیل کند.
Core Question

چرا پروژه‌های JavaScript مدرن به Build Process نیاز دارند؟

مقدمه

در فصل‌های قبل، JavaScript Application را از نظر ساختار کد بررسی کردیم.

Application می‌تواند به Moduleهای مختلف تقسیم شود، هر Module مسئولیت مشخصی داشته باشد و Dependencyهای موردنیاز آن نیز توسط Package Manager مدیریت شوند.

اما یک سؤال مهم باقی می‌ماند.

فرض کنید یک پروژه JavaScript داریم که شامل چندین Module، Package و فایل مختلف است.

آیا Browser باید دقیقاً همین ساختار توسعه را دریافت و اجرا کند؟

پاسخ همیشه مثبت نیست.

کدی که Developer برای توسعه Application می‌نویسد، معمولاً برای خوانایی، توسعه و نگهداری سازمان‌دهی شده است.

در مقابل، خروجی Production باید برای اجرا و تحویل به کاربر آماده باشد.

بنابراین بین این دو نقطه فاصله‌ای وجود دارد:

Source Code → Production

Build Process فرآیندی است که این فاصله را مدیریت می‌کند.

اما برای درک Build Process نباید آن را یک دستور یا یک ابزار خاص در نظر بگیریم.

ابتدا باید ببینیم چه چیزی وارد این فرآیند می‌شود و چرا هر مرحله به مرحله بعدی نیاز پیدا می‌کند.

Source Code

هر Build Process از Source Code شروع می‌شود.

Source Code همان کدی است که Developer برای ساخت Application می‌نویسد.

برای مثال، یک Application می‌تواند چند Module داشته باشد:

src/
├── index.js
├── recipe.js
├── search.js
└── api.js

این ساختار برای Developer مناسب است؛ زیرا هر بخش Application مسئولیت مشخصی دارد.

اما همین ساختار هنوز یک Development Structure است.

وقتی Application بزرگ‌تر می‌شود، تعداد فایل‌ها، Moduleها و Dependencyها نیز افزایش پیدا می‌کند.

در نتیجه یک سؤال ایجاد می‌شود:

اگر Source Code از بخش‌های مختلف تشکیل شده باشد، سیستم چگونه می‌داند هر بخش به چه چیزهایی وابسته است؟

اینجاست که مفهوم بعدی اهمیت پیدا می‌کند.

Dependencies

یک Module معمولاً به تنهایی کار نمی‌کند.

ممکن است یک Module، Module دیگری را Import کند یا Application به یک Package خارجی نیاز داشته باشد.

برای مثال:

import { getRecipe } from './api.js';

در اینجا index.js برای اجرای بخشی از منطق خود به api.js نیاز دارد.

این رابطه، Dependency ایجاد می‌کند.

Dependency یعنی یک بخش از Application برای انجام وظیفه خود به بخش دیگری نیاز داشته باشد.

این وابستگی‌ها بخشی از ساختار واقعی Application هستند.

بنابراین Build Process نمی‌تواند فقط فایل‌ها را جداگانه ببیند؛ باید بداند Source Code چگونه به Moduleها و Dependencyهای خود متصل است.

اما هنوز یک مسئله باقی مانده است.

Application ما از Moduleهای مختلف ساخته شده است. پس Build Process باید بتواند این Moduleها را به‌عنوان بخش‌های مرتبط یک Application مدیریت کند.

Modules

Moduleها به ما اجازه دادند Application را به بخش‌های مستقل‌تر تقسیم کنیم.

در فصل Modules دیدیم که یک Application بزرگ می‌تواند به Moduleهای مختلف تقسیم شود و هر Module مرز مشخصی داشته باشد.

در زمان توسعه، این جداسازی یک مزیت مهم است.

برای مثال، در یک Recipe Application ممکن است:

یک Module مسئول API باشد.
یک Module مسئول Search باشد.
یک Module مسئول نمایش Recipe باشد.

Developer می‌خواهد این بخش‌ها جدا از یکدیگر قابل فهم و نگهداری باشند.

اما Build Process باید از این ساختار توسعه عبور کند و از آن یک خروجی قابل استفاده ایجاد کند.

برای انجام این کار، ابتدا باید Source Code را به شکلی آماده کند که برای مراحل بعدی Build قابل پردازش باشد.

اینجا به مرحله Transformation می‌رسیم.

Transformation

Source Code همیشه در همان شکلی که Developer آن را نوشته است، بهترین شکل برای مراحل بعدی Build نیست.

گاهی لازم است Source Code تغییر شکل داده شود تا برای محیط هدف یا مرحله بعدی فرآیند مناسب‌تر باشد.

این کار را Transformation می‌نامیم.

Transformation یعنی تبدیل Source Code از یک شکل به شکل دیگری، بدون اینکه هدف اصلی Application تغییر کند.

برای مثال، ممکن است ساختار یا Syntax کد نیاز به تبدیل داشته باشد تا خروجی برای محیط موردنظر مناسب‌تر شود.

نکته مهم این است که Transformation با تغییر منطق Application یکی نیست.

هدف آن این نیست که رفتار مورد انتظار Application را عوض کند.

هدف این است که Source Code به شکلی تبدیل شود که بتوان از آن در ادامه Build Process استفاده کرد.

اما حتی بعد از Transformation هنوز یک مسئله داریم.

Application ما همچنان از چندین Module تشکیل شده است.

اگر این Moduleها و Dependencyها جدا از یکدیگر باقی بمانند، Build Process هنوز خروجی نهایی را آماده نکرده است.

بنابراین باید راهی برای سازمان‌دهی این بخش‌های مختلف در خروجی پیدا کنیم.

Bundling

تا اینجا Source Code را داریم، Dependencyها را می‌شناسیم، Moduleها را داریم و در صورت نیاز Source Code را Transform کرده‌ایم.

اکنون باید این بخش‌های مرتبط را برای خروجی Application سازمان‌دهی کنیم.

این مرحله Bundling نام دارد.

Bundling یعنی جمع‌آوری و سازمان‌دهی Moduleها و Dependencyهای موردنیاز Application در خروجی‌های مناسب برای اجرا.

فرض کنید Application ما از چند Module تشکیل شده است:

index.js
recipe.js
search.js
api.js

در محیط توسعه، جدا بودن این فایل‌ها برای Developer مفید است.

اما Build Process می‌تواند این ساختار توسعه را به خروجی‌ای تبدیل کند که برای تحویل Application مناسب‌تر باشد.

بنابراین Bundling را نباید صرفاً «یکی کردن فایل‌ها» بدانیم.

هدف اصلی آن این است که ساختار Moduleها و Dependencyهای موردنیاز Application به شکل مناسب برای خروجی Build سازمان‌دهی شود.

اما هنوز یک سؤال مهم باقی مانده است:

آیا خروجی‌ای که فقط آماده اجرا شده، بهترین خروجی ممکن برای Production است؟

لزوماً نه.

ممکن است در Source Code فاصله‌ها، بخش‌های غیرضروری یا ساختاری وجود داشته باشد که برای Developer مفید هستند اما برای خروجی نهایی ضروری نیستند.

پس مرحله دیگری موردنیاز است.

Optimization

بعد از اینکه خروجی Application آماده شد، می‌توان آن را برای Production بهینه کرد.

این مرحله Optimization است.

Optimization یعنی آماده‌سازی خروجی برای استفاده کارآمدتر، بدون تغییر رفتار مورد انتظار Application.

برای مثال، خروجی Production می‌تواند به شکلی آماده شود که حجم غیرضروری نداشته باشد یا ساختار آن برای تحویل مناسب‌تر باشد.

نکته مهم این است که Optimization را با Transformation یکی ندانیم.

Transformation بر تبدیل Source Code به شکل مناسب‌تر تمرکز دارد.

Optimization بر بهبود خروجی برای استفاده نهایی تمرکز دارد.

همچنین Optimization هدف متفاوتی از Bundling دارد.

Bundling ساختار Moduleها و Dependencyها را برای خروجی سازمان‌دهی می‌کند؛ Optimization خروجی حاصل را برای Production مناسب‌تر می‌کند.

اکنون تمام مراحل اصلی را پشت سر گذاشته‌ایم.

اما حاصل نهایی این فرآیند چیست؟

Production Build

پس از عبور Source Code از مراحل موردنیاز، به خروجی‌ای می‌رسیم که برای استفاده در محیط Production آماده شده است.

این خروجی را Production Build می‌نامیم.

Production Build نسخه‌ای از Application است که برای انتشار و استفاده نهایی آماده شده است.

بنابراین می‌توانیم کل فرآیند را به شکل زیر ببینیم:

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

این ترتیب صرفاً فهرستی از اصطلاحات نیست.

هر مرحله پاسخی به مسئله‌ای است که مرحله قبل ایجاد کرده است:

Source Code ساختار Application را در اختیار ما قرار می‌دهد.

از آنجا که بخش‌های مختلف به یکدیگر وابسته‌اند، باید Dependencies را در نظر بگیریم.

از آنجا که Application با ساختار Moduleها سازمان‌دهی شده است، Build Process باید Modules را مدیریت کند.

از آنجا که Source Code ممکن است به شکل دیگری برای مرحله Build یا محیط هدف نیاز داشته باشد، Transformation لازم می‌شود.

از آنجا که Moduleها و Dependencyها باید به خروجی قابل استفاده تبدیل شوند، Bundling مطرح می‌شود.

از آنجا که خروجی اولیه هنوز می‌تواند برای Production بهبود پیدا کند، Optimization انجام می‌شود.

و در نهایت، نتیجه این فرآیند Production Build است.

این همان مدل ذهنی اصلی Build Process است.

Build Process به‌عنوان یک فرآیند

اکنون می‌توانیم Build Process را دقیق‌تر تعریف کنیم.

Build Process مجموعه‌ای از مراحل است که Source Code و Dependencyهای یک Application را دریافت می‌کند و پس از پردازش و آماده‌سازی، خروجی مناسب برای استفاده نهایی ایجاد می‌کند.

بنابراین Build Process یک ابزار خاص نیست.

Build Process یک مفهوم است.

ابزارهایی که در فصل‌های بعد بررسی خواهیم کرد، این فرآیند را اجرا و مدیریت می‌کنند.

این تفکیک مهم است؛ زیرا اگر Build Process را با یک ابزار خاص یکی بدانیم، با تغییر ابزار، مدل ذهنی ما نیز از بین می‌رود.

اما اگر فرآیند را بشناسیم، می‌توانیم نقش هر Build Tool را در آن تحلیل کنیم.

Development و Production

Build Process فقط برای زمانی نیست که Application منتشر می‌شود.

در یک پروژه واقعی، Developer در طول Development نیز با فرآیندهایی مانند پردازش فایل‌ها، مدیریت Moduleها و آماده‌سازی Application سروکار دارد.

اما هدف Development و Production یکسان نیست.

در Development، اولویت اصلی معمولاً سرعت توسعه و بازخورد سریع Developer است.

در Production، خروجی باید برای تحویل نهایی Application آماده باشد.

بنابراین یک Build Process حرفه‌ای می‌تواند بسته به هدف، رفتار متفاوتی داشته باشد؛ اما مدل اصلی آن همچنان همان زنجیره‌ای است که بررسی کردیم.

یک مدل ذهنی ساده

برای اینکه Build Process را به یک دستور حفظی تبدیل نکنیم، آن را مانند یک مسیر در نظر بگیرید.

Developer ابتدا Application را به شکل قابل فهم و قابل نگهداری می‌سازد.

Source Code

Application به بخش‌ها و Dependencyهای مختلف تقسیم شده است.

Dependencies
Modules

Build Process این Source Code را برای مراحل بعدی آماده می‌کند.

Transformation

سپس بخش‌های موردنیاز را برای خروجی سازمان‌دهی می‌کند.

Bundling

بعد خروجی را برای استفاده نهایی مناسب‌تر می‌کند.

Optimization

و در نهایت چیزی ایجاد می‌شود که برای انتشار Application آماده است.

Production Build

بنابراین Build Process را می‌توان یک پل میان Development Structure و Production Output دانست.

Best Practices
1. Build Process را با Build Tool یکی ندانید

Build Process یک فرآیند است؛ Build Tool ابزاری است که این فرآیند را اجرا یا مدیریت می‌کند.

2. Source Code و Production Build را یکسان فرض نکنید

ساختار مناسب برای Developer الزاماً همان ساختار مناسب برای Production نیست.

3. Transformation، Bundling و Optimization را از یکدیگر تفکیک کنید

این سه مرحله هدف یکسانی ندارند.

4. ابزار را بعد از فرآیند یاد بگیرید

ابتدا باید بدانید Build Process چه مسئله‌ای را حل می‌کند؛ سپس می‌توان نقش یک ابزار مشخص را تحلیل کرد.

5. Build را بخشی از Application Lifecycle بدانید

Build یک عملیات تصادفی در انتهای پروژه نیست؛ بخشی از مسیر تبدیل Source Code به Application قابل انتشار است.

Common Mistakes
اشتباه اول: Build Process یعنی Bundle کردن فایل‌ها

Bundling فقط یکی از مراحل Build Process است.

اشتباه دوم: Build Tool همان Build Process است

ابزار فقط اجرای فرآیند را بر عهده می‌گیرد؛ خود فرآیند مجموعه‌ای از مراحل و اهداف است.

اشتباه سوم: Transformation و Optimization یک مفهوم هستند

Transformation شکل Source Code را تغییر می‌دهد؛ Optimization خروجی را برای استفاده نهایی بهبود می‌دهد.

اشتباه چهارم: Source Code همان Production Code است

Source Code برای توسعه نوشته می‌شود؛ Production Build برای انتشار آماده می‌شود.

اشتباه پنجم: Build فقط برای Applicationهای بزرگ است

حتی پروژه‌های کوچک نیز ممکن است از Moduleها، Dependencyها و فرآیندهای تبدیل و آماده‌سازی استفاده کنند.

Summary

یک JavaScript Application مدرن معمولاً از Source Code، Moduleها و Dependencyهای مختلف تشکیل شده است.

این ساختار برای توسعه مناسب است، اما الزاماً همان ساختاری نیست که باید به کاربر نهایی تحویل داده شود.

Build Process این فاصله را مدیریت می‌کند.

فرآیند از Source Code آغاز می‌شود.

سپس Dependencies و Modules در نظر گرفته می‌شوند.

در صورت نیاز، Source Code دچار Transformation می‌شود.

Moduleها و Dependencyهای موردنیاز برای خروجی با Bundling سازمان‌دهی می‌شوند.

سپس خروجی با Optimization برای Production آماده‌تر می‌شود.

در نهایت Production Build ایجاد می‌شود.

مدل ذهنی نهایی:

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
Key Takeaways
Build Process پلی میان Source Code و Production Build است.
Source Code برای توسعه و نگهداری نوشته می‌شود.
Dependencies روابط موردنیاز میان بخش‌های مختلف Application را مشخص می‌کنند.
Modules ساختار Application را به بخش‌های مستقل‌تر تقسیم می‌کنند.
Transformation Source Code را به شکل مناسب‌تری تبدیل می‌کند.
Bundling Moduleها و Dependencyهای موردنیاز را برای خروجی سازمان‌دهی می‌کند.
Optimization خروجی را برای استفاده نهایی مناسب‌تر می‌کند.
Production Build خروجی آماده انتشار Application است.
Build Process یک مفهوم است، نه نام یک ابزار خاص.
Technical Interview
Junior Level
سؤال:

Build Process چیست؟

Golden Answer:

Build Process مجموعه‌ای از مراحل است که Source Code و Dependencyهای یک Application را پردازش می‌کند تا خروجی مناسب برای استفاده و انتشار ایجاد شود.

Mid-Level
سؤال:

تفاوت Transformation، Bundling و Optimization چیست؟

Golden Answer:

Transformation Source Code را به شکل مناسب‌تری تبدیل می‌کند، Bundling Moduleها و Dependencyهای موردنیاز را برای خروجی سازمان‌دهی می‌کند و Optimization خروجی را برای استفاده نهایی بهینه‌تر می‌کند.

Senior Level
سؤال:

چرا Source Code یک Application را مستقیماً به‌عنوان Production Output تحویل نمی‌دهیم؟

Golden Answer:

زیرا ساختار Source Code برای توسعه، خوانایی و نگهداری طراحی شده است و الزاماً برای اجرای نهایی و تحویل بهینه نیست. Build Process با مدیریت Dependencyها و Moduleها، انجام Transformation، Bundling و Optimization، Source Code را به Production Build تبدیل می‌کند.

Conclusion

تا اینجا مسئله اصلی را از دید مفهومی حل کردیم.

می‌دانیم که یک Application از Source Code و Moduleها تشکیل شده و Dependencyهای مختلفی دارد. همچنین می‌دانیم که Source Code برای رسیدن به Production باید از چند مرحله عبور کند.

اما هنوز یک سؤال عملی باقی مانده است:

چه ابزاری این مراحل را در یک پروژه واقعی انجام می‌دهد؟

اکنون زمان آن رسیده است که از مفهوم Build Process به یک Build Tool واقعی برسیم.

در فصل بعد، با Parcel بررسی خواهیم کرد که چگونه یک Build Tool می‌تواند این فرآیند را در Development و Production اجرا و مدیریت کند