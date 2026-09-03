# JavaScript Book --- Chapter Production & Review Contract

> **Purpose:** جلوگیری از تکرار مشکل سبک و جریان آموزشی در فصل‌های جدید و
> ایجاد یک استاندارد ثابت برای تولید و بازبینی Chapterها.

این سند مکمل `PROJECT_SPEC_FINAL.md` و `SERIES_BLUEPRINT_REVISED(4).md`
است و جایگزین هیچ‌کدام نیست.

`PROJECT_SPEC_FINAL.md` قانون نهایی محتواست و
`SERIES_BLUEPRINT_REVISED(4).md` نقشه راه و Concept Flow را تعیین می‌کند.
این سند فقط روش تبدیل آن‌ها به یک فصل آموزشی منسجم و روش بازبینی فصل را
مشخص می‌کند.

------------------------------------------------------------------------

## 1. Source of Truth

در تولید هر Chapter، ترتیب اعتبار مراجع چنین است:

1.  `PROJECT_SPEC_FINAL.md`
2.  `SERIES_BLUEPRINT_REVISED(4).md`
3.  فصل‌های نهایی و تأییدشده قبلی، به‌عنوان **Style Reference**
4.  دانش فنی عمومی، فقط برای تکمیل توضیح در محدوده‌ای که دو مرجع اصلی
    اجازه می‌دهند.

### Rule

هیچ Blueprint قدیمی، Writing Block قدیمی، Draft قبلی یا Template تولیدی
نباید بر نسخه Revised غلبه کند.

`SERIES_BLUEPRINT_REVISED(4).md` صراحتاً مدل تولید را از **Block-based
Writing** به **Narrative Flow** تغییر داده است.

------------------------------------------------------------------------

# 2. Concept Flow ≠ Chapter Structure

Concept Flow معماری یادگیری است؛ Headingهای فصل نیست.

برای مثال:

``` text
Object
↓
Prototype
↓
Shared Methods
↓
Property Lookup
↓
Prototype Chain
```

نباید به‌صورت خودکار به این ساختار تبدیل شود:

``` text
## Object
## Prototype
## Shared Methods
## Property Lookup
## Prototype Chain
```

Conceptها باید ابتدا به یک **روایت آموزشی** تبدیل شوند و سپس فقط در
جاهایی که یک مرز واقعی آموزشی وجود دارد Heading ایجاد شود.

### اصل

> **Concept Flow مشخص می‌کند چه چیزی و با چه وابستگی‌ای آموزش داده شود؛
> Narrative Flow مشخص می‌کند این مفاهیم چگونه برای خواننده روایت شوند.**

------------------------------------------------------------------------

# 3. Narrative First

پیش از نوشتن Markdown نهایی، باید برای فصل یک Narrative Skeleton ساخته
شود.

Narrative Skeleton نباید فهرست Headingها باشد.

باید مشخص کند:

``` text
Problem
↓
Need
↓
Concept
↓
Consequence
↓
New Question
↓
Next Concept
↓
New Problem
↓
Resolution
```

### مثال

به‌جای:

``` text
Array
Index
Element
length
Mutation
Reference
Iteration
```

روایت باید از یک نیاز واقعی شروع شود:

``` text
نیاز به نگهداری چند Value
↓
Collection
↓
Array به‌عنوان یک Collection مرتب
↓
چگونه هر Value را پیدا کنیم؟
↓
Index و Element
↓
چگونه اندازه Collection را بدانیم؟
↓
length
↓
اگر Array تغییر کند چه اتفاقی می‌افتد؟
↓
Mutation
↓
چرا دو Variable می‌توانند به همان Array اشاره کنند؟
↓
Reference
↓
چگونه عناصر را یکی‌یکی پردازش کنیم؟
↓
Iteration
```

------------------------------------------------------------------------

# 4. The Core Question Must Control the Chapter

هر فصل فقط باید به Core Question خودش پاسخ دهد.

در طول تولید باید دائماً بررسی شود:

> «آیا این توضیح برای پاسخ به سؤال محوری فصل لازم است؟»

اگر پاسخ منفی است، یکی از این کارها انجام شود:

-   حذف شود.
-   به Preview بسیار کوتاه تبدیل شود.
-   به فصل مناسب آینده منتقل شود.

این اصل از `Scope Rule` و `Zero Noise` پیروی می‌کند.

------------------------------------------------------------------------

# 5. Transition Rule

هر Concept مهم باید رابطه مشخصی با Concept بعدی داشته باشد.

قبل از عبور از یک مفهوم به مفهوم بعدی، یکی از این روابط باید وجود داشته
باشد:

-   سؤال طبیعی
-   مشکل عملی
-   محدودیت مفهوم قبلی
-   نتیجه منطقی
-   نیاز به ابزار یا مدل جدید

### ممنوع

``` text
Array چیست؟

length چیست؟

Mutation چیست؟
```

### مطلوب

``` text
وقتی چند Value را در Array قرار دادیم، باید بتوانیم بدانیم
هر Value در کجا قرار دارد.

پس Index مطرح می‌شود.

...

حالا که می‌توانیم Elementها را پیدا کنیم،
یک سؤال دیگر داریم:
از کجا بدانیم Array چند Element دارد؟

اینجاست که length وارد می‌شود.
```

------------------------------------------------------------------------

# 6. Heading Rule

Heading فقط وقتی ایجاد شود که یکی از این موارد رخ داده باشد:

-   یک مفهوم مستقل و مهم معرفی می‌شود.
-   یک سؤال جدید ایجاد شده است.
-   یک تغییر مهم در مدل ذهنی رخ می‌دهد.
-   یک بخش عملی مستقل شکل گرفته است.
-   یک مرز مشخص بین دو موضوع وجود دارد.

### ممنوع

ساختن Heading برای هر اصطلاح صرفاً به دلیل وجود آن در Concept Flow.

### نتیجه

تعداد Headingها باید **از روایت حاصل شود**، نه از تعداد Concepts.

------------------------------------------------------------------------

# 7. Definition Rule

تعریف باید زمانی ارائه شود که خواننده به آن نیاز دارد.

از تعریف‌های دایره‌ای و فرهنگ‌لغت‌گونه اجتناب شود.

### الگوی ترجیحی

``` text
Need
↓
Observation
↓
Definition
↓
Example
↓
Explanation
```

نه:

``` text
Definition
↓
Definition
↓
Definition
↓
Example
```

------------------------------------------------------------------------

# 8. Example Rule

هر Example باید حداقل یکی از این کارها را انجام دهد:

-   یک مفهوم جدید را روشن کند.
-   یک رفتار JavaScript را نشان دهد.
-   یک تفاوت مهم را آشکار کند.
-   یک اشتباه رایج را قابل مشاهده کند.
-   یک کاربرد واقعی را نشان دهد.

Example نباید صرفاً برای پر کردن فصل اضافه شود.

### ویژگی Example

-   کوتاه
-   Runnable
-   واقع‌گرایانه
-   مرتبط با Application Development
-   هم‌سطح با Concept فعلی

------------------------------------------------------------------------

# 9. Code Explanation Rule

کد نباید بدون توضیح رها شود.

اما توضیح نیز نباید خط‌به‌خط و مکانیکی شود.

تمرکز روی **رفتار مهم کد** باشد:

``` text
What happens?
Why does it happen?
What mental model explains it?
```

------------------------------------------------------------------------

# 10. Mental Model Rule

هر فصل باید در نهایت یک مدل ذهنی قابل بازسازی ایجاد کند.

خواننده نباید فقط بداند:

``` text
Array has length.
```

باید بتواند توضیح دهد:

``` text
Array
→ indexed collection
→ elements are accessed through indexes
→ length represents the array's current length
→ arrays are mutable objects
→ variables can reference the same array
→ iteration processes its elements
```

مدل ذهنی مهم‌تر از حفظ Syntax است.

------------------------------------------------------------------------

# 11. No Future Leakage

مفهومی که هنوز نوبتش نرسیده نباید به‌صورت کامل آموزش داده شود.

اگر Concept آینده برای توضیح فعلی ضروری نیست:

> حذف شود.

اگر فقط برای جلوگیری از ابهام لازم است:

> یک Preview کوتاه کافی است.

### مثال

اگر Chapter 32 فقط Fundamentals است، نباید بدون نیاز وارد آموزش کامل:

``` text
map
filter
reduce
find
sort
splice
```

شود، زیرا این‌ها در Chapterهای بعدی Concept Flow مستقل دارند.

------------------------------------------------------------------------

# 12. Scope Boundary Between Chapters

در پایان هر Chapter باید بررسی شود:

### آیا این فصل چیزی را آموزش داده که Chapter آینده قرار است آموزش دهد؟

اگر بله:

-   توضیح اضافی حذف شود.
-   یا به سطح مورد نیاز فصل محدود شود.

### آیا Chapter فعلی برای درک فصل بعدی بیش از حد کم توضیح داده است؟

اگر بله:

-   فقط پیش‌نیاز لازم اضافه شود.

هدف، ایجاد **Dependency Continuity** است.

------------------------------------------------------------------------

# 13. Style Anchor

برای ارزیابی سبک، Chapter 27 به‌عنوان Style Anchor استفاده شود.

منظور از Style Anchor تقلید جمله‌ها یا ساختار Headingها نیست.

باید این ویژگی‌ها بررسی شوند:

-   روایت پیوسته
-   انتقال طبیعی بین مفاهیم
-   مثال در خدمت روایت
-   توضیح مهندسی و قابل استنتاج
-   عدم پرگویی
-   عدم تبدیل Concepts به فهرست
-   ارتباط روشن با مفاهیم قبلی
-   ایجاد نیاز برای مفهوم بعدی
-   پایان‌بندی طبیعی

### اصل مهم

> **Chapter 27 باید Style Reference باشد، نه Template.**

------------------------------------------------------------------------

# 14. Chapter Architecture

هر Chapter باید حداقل این مسیر را داشته باشد:

``` text
Chapter Title
↓
Learning Goal / Objectives
↓
Core Question
↓
Introduction / Motivation
↓
Narrative Body
↓
Review / Summary
↓
Key Takeaways
↓
Technical Interview
↓
Golden Answers
↓
Conclusion
```

اما بخش‌های داخل Narrative Body نباید Template ثابت داشته باشند.

ممکن است یک Concept با Example شروع شود و Concept دیگر با یک سؤال یا یک
مسئله.

------------------------------------------------------------------------

# 15. Interview Rule

Technical Interview باید حاصل طبیعی فصل باشد.

Interview نباید به یک فصل مستقل تبدیل شود.

سه سطح حفظ شود:

``` text
Junior
Mid-Level
Senior
```

سؤالات باید:

-   مستقیماً از Conceptهای فصل باشند.
-   نیازمند فهم باشند، نه حفظ Syntax.
-   تکرار یکدیگر نباشند.

Golden Answers باید پاسخ کامل و مهندسی ارائه کنند، اما نباید مطالب جدید
خارج از Scope فصل آموزش دهند.

------------------------------------------------------------------------

# 16. Zero Noise Audit

برای هر پاراگراف این سؤال مطرح شود:

> «اگر این پاراگراف حذف شود، آیا مدل ذهنی خواننده آسیب می‌بیند؟»

اگر خیر:

``` text
REMOVE
```

اگر فقط بخشی از آن لازم است:

``` text
SHORTEN
```

اگر لازم است:

``` text
KEEP
```

------------------------------------------------------------------------

# 17. Completeness Audit

پس از حذف Noise باید بررسی شود که فصل هنوز به Core Question پاسخ کامل
می‌دهد.

این دو اصل باید هم‌زمان برقرار باشند:

``` text
Complete
+
Concise
```

نه:

``` text
Complete = Long
```

------------------------------------------------------------------------

# 18. Technical Audit

قبل از تأیید نهایی:

### بررسی شود:

-   Syntax صحیح است.
-   رفتار JavaScript دقیق است.
-   مثال‌ها Runnable هستند.
-   اصطلاحات consistent هستند.
-   تفاوت مفاهیم مشابه روشن است.
-   ادعاهای فنی بدون دلیل مطرح نشده‌اند.
-   توضیح با Runtime یا Mechanism اشتباه مخلوط نشده است.
-   Conceptهای آینده بیش از حد آموزش داده نشده‌اند.

------------------------------------------------------------------------

# 19. Narrative Audit

بعد از Technical Audit، فصل از دید خواننده بررسی شود.

### پنج سؤال اصلی

#### 1. آیا فصل با یک نیاز یا سؤال شروع می‌شود؟

#### 2. آیا هر Concept به Concept بعدی منتهی می‌شود؟

#### 3. آیا خواننده می‌فهمد «چرا» مفهوم بعدی لازم شده است؟

#### 4. آیا Headingها حاصل روایت هستند؟

#### 5. آیا متن بدون Headingهایش نیز یک جریان منطقی دارد؟

اگر پاسخ سؤال پنجم «خیر» باشد، احتمالاً فصل بیش از حد Heading-driven است.

------------------------------------------------------------------------

# 20. Style Audit

فصل با Style Anchor مقایسه شود.

بررسی شود:

  معیار         سؤال
  ------------- ------------------------------------------------
  Narrative     آیا متن داستان آموزشی دارد؟
  Flow          آیا انتقال‌ها طبیعی هستند؟
  Clarity       آیا مفهوم بدون پیچیدگی اضافی روشن است؟
  Density       آیا هر بخش بیش از حد فشرده یا پراکنده نیست؟
  Examples      آیا مثال‌ها در خدمت مفهوم هستند؟
  Noise         آیا توضیح اضافی وجود دارد؟
  Consistency   آیا زبان و اصطلاحات با فصل‌های قبلی هماهنگ است؟
  Scope         آیا فصل از مرز خود خارج نشده است؟

------------------------------------------------------------------------

# 21. Final Production Pipeline

از این پس تولید Chapter باید دقیقاً از این Pipeline عبور کند:

``` text
PROJECT_SPEC_FINAL.md
        ↓
SERIES_BLUEPRINT_REVISED(4).md
        ↓
Chapter Goal
        ↓
Core Question
        ↓
Concept Flow
        ↓
Dependency Check
        ↓
Narrative Skeleton
        ↓
Heading Extraction
        ↓
Draft
        ↓
Concept Audit
        ↓
Technical Audit
        ↓
Narrative Audit
        ↓
Style Audit
        ↓
Zero Noise Audit
        ↓
Interview Audit
        ↓
Final Chapter
```

------------------------------------------------------------------------

# 22. ممنوعیت تولید مستقیم از Concept Flow

از این پس این روش ممنوع است:

``` text
Read Concept Flow
↓
Turn every Concept into a Section
↓
Add Definition
↓
Add Example
↓
Add Analysis
↓
Add Summary
```

این روش عامل اصلی تبدیل فصل‌ها به Documentation است.

روش صحیح:

``` text
Read Concept Flow
↓
Understand Dependencies
↓
Find the Teaching Problem
↓
Build Narrative
↓
Let Concepts emerge naturally
↓
Create only necessary Headings
↓
Write
↓
Audit
```

------------------------------------------------------------------------

# 23. Review of Existing Chapters

برای فصل‌های اخیر، ابتدا بازنویسی مستقیم انجام نشود.

هر فصل ابتدا با سه وضعیت ارزیابی شود:

``` text
KEEP
```

فصل از نظر Narrative و Style قابل قبول است.

``` text
REFACTOR
```

Concept Flow و محتوای اصلی درست است، اما روایت، Headingها یا حجم توضیح
نیاز به اصلاح دارد.

``` text
REWRITE
```

ساختار روایت از ابتدا بر اساس Template ساخته شده و اصلاح موضعی نتیجه
مطلوب نمی‌دهد.

------------------------------------------------------------------------

# 24. Recommended Review Order

فصل‌های اخیر باید از آخرین فصل به سمت Style Anchor بازبینی شوند:

``` text
Chapter 32
↓
Chapter 31
↓
Chapter 30
↓
Chapter 29
↓
Chapter 28
↓
Chapter 27
```

Chapter 27 مرجع مقایسه است، نه اینکه الزاماً بدون بررسی تغییرناپذیر فرض
شود.

در هر فصل ابتدا:

``` text
Concept Audit
```

سپس:

``` text
Narrative Audit
```

و در پایان:

``` text
Style Audit
```

انجام شود.

------------------------------------------------------------------------

# 25. Golden Rule

> **Concept Flow tells us what the reader must learn and in what
> dependency order.**
>
> **Narrative Flow tells us how the reader should discover and
> understand it.**
>
> **Heading structure must emerge from Narrative Flow, not from Concept
> Flow.**

و مهم‌تر:

> **هر فصل باید یک مسیر فکری داشته باشد، نه مجموعه‌ای از توضیحات درباره
> چند Concept.**

------------------------------------------------------------------------

# 26. Definition of Done

یک Chapter فقط زمانی Final محسوب می‌شود که:

-   [ ] با `PROJECT_SPEC_FINAL.md` سازگار باشد.
-   [ ] با `SERIES_BLUEPRINT_REVISED(4).md` سازگار باشد.
-   [ ] Core Question را کامل پاسخ دهد.
-   [ ] تمام Conceptهای لازم را پوشش دهد.
-   [ ] Concept Flow را حفظ کند.
-   [ ] Concept Flow را به Blockهای مصنوعی تبدیل نکرده باشد.
-   [ ] Narrative Flow طبیعی داشته باشد.
-   [ ] Headingها از روایت حاصل شده باشند.
-   [ ] هیچ Concept آینده را زودتر آموزش نداده باشد.
-   [ ] مثال‌ها Runnable و کاربردی باشند.
-   [ ] Zero Noise رعایت شده باشد.
-   [ ] Complete but Concise باشد.
-   [ ] Technical Audit را گذرانده باشد.
-   [ ] Narrative Audit را گذرانده باشد.
-   [ ] Style Audit را گذرانده باشد.
-   [ ] Technical Interview مکمل فصل باشد.
-   [ ] Golden Answers خارج از Scope فصل نروند.
-   [ ] فصل در مقایسه با Style Anchor از نظر کیفیت روایت افت نکرده باشد.

------------------------------------------------------------------------

## Final Principle

این پروژه دیگر نباید با این سؤال تولید شود:

> «Conceptهای این فصل چیستند؟»

بلکه باید با این سؤال تولید شود:

> **«خواننده از کجا شروع می‌کند، چه مسئله‌ای دارد، چه چیزی را می‌فهمد، چرا
> به مفهوم بعدی نیاز پیدا می‌کند، و در پایان چه مدل ذهنی قابل استفاده‌ای
> با خود می‌برد؟»**

Concept Flow نقشه است.

Narrative Flow مسیر حرکت خواننده روی آن نقشه است.

فصل نهایی باید حاصل هر دو باشد.
