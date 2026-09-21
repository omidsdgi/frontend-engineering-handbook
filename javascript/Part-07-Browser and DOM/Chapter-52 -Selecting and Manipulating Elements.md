Chapter 52 — Selecting and Manipulating Elements
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

DOM Element را از Document انتخاب کنید.
تفاوت میان Selection و داشتن Reference به یک Element را درک کنید.
از CSS Selectorها برای Query کردن DOM استفاده کنید.
تفاوت querySelector() و querySelectorAll() را توضیح دهید.
Content یک Element را تغییر دهید.
تفاوت textContent و innerHTML را تحلیل کنید.
تفاوت HTML Attribute و DOM Property را توضیح دهید.
Attributeها و Propertyهای یک Element را مدیریت کنید.
Classهای یک Element را با classList تغییر دهید.
Styleهای Inline را از طریق DOM تغییر دهید.
برای هر نوع تغییر، DOM API مناسب را انتخاب کنید.
Core Question

چگونه Elementهای موجود در صفحه را پیدا کنیم و آن‌ها را به‌صورت صحیح تغییر دهیم؟

مقدمه

تا اینجا بیشتر با خود زبان JavaScript و مدل‌های داده‌ای آن کار کرده‌ایم.

اما در Browser، JavaScript فقط با داده‌ها و Functionها سروکار ندارد. یکی از مهم‌ترین وظایف آن این است که با صفحه‌ای که کاربر می‌بیند تعامل داشته باشد.

فرض کنید یک Recipe Application داریم.

اطلاعات Recipe از یک API دریافت شده و اکنون باید عنوان آن روی صفحه نمایش داده شود.

داده را در اختیار داریم:

const recipe = {
title: 'Pasta Carbonara'
};

اما داشتن این Object به‌تنهایی چیزی را روی صفحه تغییر نمی‌دهد.

عنوانی که کاربر می‌بیند بخشی از DOM است:

<h1 class="recipe-title">Pasta</h1>

پس JavaScript باید ابتدا این Element را پیدا کند.

بعد باید بتواند محتوای آن را تغییر دهد.

در نتیجه مسئله واقعی دو مرحله دارد:

DOM
↓
Find the Element
↓
Change the Element

اما خیلی زود متوجه می‌شویم که «تغییر Element» خودش یک مفهوم کلی است.

گاهی می‌خواهیم متن آن را تغییر دهیم.

گاهی یک Attribute را.

گاهی یک Property را.

گاهی Class آن را.

و گاهی Style آن را.

بنابراین سؤال اصلی این فصل فقط این نیست که:

چگونه یک Element را پیدا کنیم؟

بلکه این است:

پس از پیدا کردن Element، چگونه تشخیص دهیم چه چیزی را باید تغییر دهیم و از کدام DOM API استفاده کنیم؟

پیدا کردن چیزی که می‌خواهیم تغییر دهیم

DOM را می‌توان به‌صورت یک درخت از Nodeها تصور کرد. در این درخت، Elementهای HTML بخش مهمی از ساختار صفحه هستند.

برای مثال:

document
└── body
└── main
├── h1.recipe-title
├── p.recipe-description
└── button.favorite

اگر بخواهیم عنوان Recipe را تغییر دهیم، ابتدا باید به h1 دسترسی پیدا کنیم.

Browser برای این کار APIهای مختلفی در اختیار JavaScript قرار می‌دهد.

یکی از عمومی‌ترین آن‌ها querySelector() است:

const title = document.querySelector('.recipe-title');

در اینجا .recipe-title یک CSS Selector است.

Browser در DOM جستجو می‌کند و اولین Element مطابق این Selector را برمی‌گرداند.

اکنون Variable title به خود Element اشاره می‌کند.

این تفاوت مهم است.

قبل از Query، فقط یک Selector داشتیم:

.recipe-title

بعد از Query، یک Reference به Element داریم:

title
↓
<h1 class="recipe-title">

پس Selection صرفاً «پیدا کردن یک نام» نیست.

Selection فرآیندی است که به JavaScript امکان می‌دهد از ساختار DOM به یک Element واقعی دسترسی پیدا کند.

وقتی یک Element کافی نیست

فرض کنید صفحه فقط یک عنوان ندارد.

مثلاً چند Recipe Card داریم و هرکدام یک Ingredient List دارند:

<li class="ingredient">Pasta</li>
<li class="ingredient">Eggs</li>
<li class="ingredient">Cheese</li>

این بار مسئله متفاوت است.

ما دیگر یک Element نمی‌خواهیم؛ تمام Elementهای مطابق Selector را می‌خواهیم.

برای این کار از querySelectorAll() استفاده می‌کنیم:

const ingredients =
document.querySelectorAll('.ingredient');

اکنون نتیجه یک Element واحد نیست، بلکه مجموعه‌ای از Elementهای مطابق Selector است.

این تفاوت باید در مدل ذهنی ما باقی بماند:

querySelector()
↓
First matching Element

querySelectorAll()
↓
All matching Elements

به همین دلیل اگر بخواهیم روی تمام Ingredientها عملیاتی انجام دهیم، باید با آن‌ها به‌عنوان یک مجموعه برخورد کنیم:

ingredients.forEach(ingredient => {
ingredient.classList.add('visible');
});

بنابراین انتخاب Method مناسب به سؤال ما بستگی دارد.

اگر سؤال این باشد:

اولین Element مطابق این Selector کدام است؟

از querySelector() استفاده می‌کنیم.

اگر سؤال این باشد:

تمام Elementهای مطابق این Selector کدام‌اند؟

از querySelectorAll() استفاده می‌کنیم.

جستجو همیشه از کل Document شروع نمی‌شود

تا اینجا Query را روی document انجام دادیم.

اما در Applicationهای واقعی، اغلب می‌دانیم Element موردنظر داخل یک بخش مشخص از صفحه قرار دارد.

مثلاً:

<article class="recipe-card">
  <h2 class="recipe-title">Pasta</h2>
</article>

<article class="recipe-card">
  <h2 class="recipe-title">Pizza</h2>
</article>

اگر بنویسیم:

document.querySelector('.recipe-title');

اولین عنوان در کل Document انتخاب می‌شود.

اما اگر ابتدا Card موردنظر را پیدا کنیم:

const card = document.querySelector('.recipe-card');

می‌توانیم جستجو را به همان محدوده محدود کنیم:

const title = card.querySelector('.recipe-title');

اکنون Query از خود card شروع شده است، نه از کل Document.

این الگو اهمیت زیادی دارد، زیرا DOM یک ساختار سلسله‌مراتبی است و Selection نیز می‌تواند همین ساختار را دنبال کند:

Document
↓
Parent Element
↓
Child Element

محدود کردن Scope جستجو نه‌تنها Intent کد را واضح‌تر می‌کند، بلکه در ساختارهای پیچیده DOM نیز کنترل بیشتری در اختیار ما قرار می‌دهد.

وقتی Element پیدا نمی‌شود

تا اینجا فرض کردیم Selector موردنظر همیشه در DOM وجود دارد.

اما این فرض همیشه درست نیست.

اگر بنویسیم:

const title =
document.querySelector('.unknown-title');

و چنین Elementای در DOM وجود نداشته باشد، نتیجه null خواهد بود.

این رفتار منطقی است؛ Browser نمی‌تواند Referenceای به Elementی بدهد که وجود ندارد.

بنابراین این کد:

title.textContent = 'New Recipe';

در چنین شرایطی مشکل ایجاد می‌کند.

در نتیجه یکی از نکات مهم در کار با DOM این است:

Selection ممکن است هیچ Elementی پیدا نکند.

اگر وجود Element قطعی نیست، باید این وضعیت را در Logic برنامه در نظر بگیریم:

const title =
document.querySelector('.recipe-title');

if (title) {
title.textContent = 'New Recipe';
}

این موضوع در UIهای واقعی بسیار رایج است، زیرا یک Element ممکن است فقط در یک State خاص از Application وجود داشته باشد.

از پیدا کردن Element تا تغییر آن

اکنون به نقطه مهم‌تری رسیده‌ایم.

ما Element را پیدا کرده‌ایم:

const title =
document.querySelector('.recipe-title');

اما هنوز مشخص نکرده‌ایم چه چیزی را باید تغییر دهیم.

این سؤال مهم‌تر از خود Selection است.

یک Element فقط یک «قطعه HTML» نیست.

از دید DOM، Element یک Object است که بخش‌های مختلفی دارد:

Element
├── Content
├── Attributes
├── Properties
├── Classes
└── Styles

بنابراین وقتی می‌گوییم:

«این Element را تغییر بده.»

از نظر فنی هنوز مشخص نکرده‌ایم چه تغییری می‌خواهیم.

ممکن است هدف ما Text باشد.

ممکن است Attribute باشد.

ممکن است Property باشد.

یا شاید فقط بخواهیم وضعیت ظاهری Element را تغییر دهیم.

از اینجا به بعد، نوع تغییر مشخص می‌کند که از کدام API استفاده کنیم.

وقتی فقط متن مهم است

در مثال Recipe، شاید هدف ما این باشد که عنوان:

Pasta

به:

Mushroom Pasta

تبدیل شود.

در اینجا HTML Structure قرار نیست تغییر کند.

فقط Text تغییر می‌کند.

برای این کار از textContent استفاده می‌کنیم:

title.textContent = 'Mushroom Pasta';

این دقیقاً Intent ما را بیان می‌کند:

محتوای متنی این Element را تغییر بده.

textContent برای خواندن Text نیز قابل استفاده است:

const titleText = title.textContent;

پس وقتی مسئله ما Text است، textContent API طبیعی DOM برای این کار است.

وقتی Content دیگر فقط Text نیست

اکنون فرض کنید می‌خواهیم داخل یک Element، HTML قرار دهیم.

مثلاً:

<div class="message"></div>

و می‌خواهیم نتیجه چنین ساختاری باشد:

<strong>Success</strong>

در این حالت textContent مناسب نیست، زیرا HTML را به‌عنوان Text در نظر می‌گیرد:

message.textContent =
'<strong>Success</strong>';

کاربر در این حالت خود عبارت <strong>Success</strong> را به‌صورت متن خواهد دید.

اگر واقعاً هدف ما قرار دادن HTML Markup باشد، innerHTML برای این کار طراحی شده است:

message.innerHTML =
'<strong>Success</strong>';

تفاوت اصلی چنین است:

textContent
↓
Text

innerHTML
↓
HTML Markup

اما اینجا یک ملاحظه مهم مهندسی وجود دارد.

innerHTML باعث می‌شود رشته به‌عنوان HTML تفسیر شود. بنابراین اگر محتوای آن از منبع غیرقابل اعتماد مانند Input کاربر یا داده‌ای بدون اعتبارسنجی مناسب بیاید، می‌تواند خطر امنیتی ایجاد کند.

به همین دلیل، اگر فقط می‌خواهیم Text نمایش دهیم، انتخاب textContent هم از نظر Intent و هم از نظر ایمنی انتخاب مناسب‌تری است.

این تفاوت یک قاعده ساده به ما می‌دهد:

Text را به‌عنوان Text مدیریت کنید و فقط زمانی HTML تولید کنید که واقعاً به HTML نیاز دارید.

وقتی Content مسئله نیست

گاهی چیزی که می‌خواهیم تغییر دهیم Text داخل Element نیست.

تصویر Recipe را در نظر بگیرید:

<img
class="recipe-image"
src="pasta.jpg"
alt="Pasta"
>

ممکن است بخواهیم تصویر را تغییر دهیم.

در اینجا با Attributeهای HTML مواجه هستیم:

src
alt

JavaScript می‌تواند Attributeهای یک Element را بخواند و تغییر دهد.

برای خواندن یک Attribute:

const source =
image.getAttribute('src');

و برای تغییر آن:

image.setAttribute(
'src',
'pizza.jpg'
);

در اینجا دیگر با Content کار نمی‌کنیم.

با Attribute کار می‌کنیم.

این تمایز ساده، بخش مهمی از مدل DOM است:

Text
→ textContent

HTML Attribute
→ getAttribute / setAttribute
اما Attribute تنها چیزی نیست که DOM در اختیار ما قرار می‌دهد

در اینجا ممکن است یک سؤال ایجاد شود.

اگر src یک Attribute است، چرا گاهی می‌توانیم بنویسیم:

image.src = 'pizza.jpg';

؟

پاسخ به تفاوت میان Attribute و Property مربوط است.

HTML Elementها در Browser به Objectهای DOM تبدیل می‌شوند.

بنابراین Element علاوه بر Attributeهای HTML، Propertyهایی نیز دارد.

برای مثال:

image.src

یک DOM Property است.

یا در یک Input:

input.value

یک Property است.

بنابراین باید این دو مفهوم را از یکدیگر جدا کنیم:

HTML
↓
Attribute

DOM Object
↓
Property

این دو به هم مرتبط‌اند، اما یکی نیستند.

چرا تفاوت Attribute و Property مهم است؟

یک Input را در نظر بگیرید:

<input
class="search-input"
value="pizza"
>

در HTML، value="pizza" یک Attribute است.

اما وقتی کاربر مقدار Input را تغییر می‌دهد، Property مربوط به value وضعیت فعلی Input را نشان می‌دهد:

input.value

به همین دلیل ممکن است مقدار اولیه تعریف‌شده در HTML و مقدار فعلی Element یکسان نباشند.

این موضوع نشان می‌دهد که Attribute بیشتر به Markup و اطلاعات تعریف‌شده برای Element مربوط است، در حالی که Property بخشی از Object DOM است و می‌تواند وضعیت فعلی Element را منعکس کند.

بنابراین هنگام کار با DOM باید ابتدا مشخص کنیم:

آیا با HTML Attribute سروکار داریم یا با وضعیت DOM Object؟

برای Attribute می‌توانیم از:

getAttribute()
setAttribute()

استفاده کنیم.

برای وضعیت Element، در بسیاری از موارد Property مربوطه API طبیعی‌تری است:

input.value
image.src
link.href

این تمایز در Formها اهمیت ویژه‌ای پیدا می‌کند، زیرا وضعیت فعلی کنترل‌های Form اغلب از مقدار اولیه تعریف‌شده در Markup متفاوت است.

وقتی مسئله ظاهر Element است

فرض کنید Recipe موردعلاقه کاربر است و می‌خواهیم Button مربوط به Favorite ظاهر متفاوتی پیدا کند.

HTML:

<button class="favorite-btn">
  Favorite
</button>

و CSS:

.favorite-btn--active {
/* active state */
}

در اینجا لازم نیست Text یا Attribute را تغییر دهیم.

آنچه تغییر می‌کند State ظاهری Element است.

DOM برای مدیریت Classها API مشخصی دارد:

classList

برای اضافه کردن Class:

button.classList.add(
'favorite-btn--active'
);

برای حذف:

button.classList.remove(
'favorite-btn--active'
);

برای تغییر وضعیت:

button.classList.toggle(
'favorite-btn--active'
);

و برای بررسی وجود Class:

button.classList.contains(
'favorite-btn--active'
);

اکنون یک الگوی مهم شکل گرفته است:

Content
→ textContent

Attribute
→ getAttribute / setAttribute

Class
→ classList

این APIها صرفاً Methodهای مختلف نیستند.

هرکدام برای نوع خاصی از تغییر طراحی شده‌اند.

چرا Class را مستقیماً جایگزین نکنیم؟

می‌توانیم کل مقدار class را نیز تغییر دهیم:

element.className = 'active';

اما این کار ممکن است Classهای قبلی را از بین ببرد.

فرض کنید:

<div class="modal dark-theme rounded"></div>

اگر بنویسیم:

element.className = 'active';

Classهای قبلی حذف می‌شوند.

در مقابل:

element.classList.add('active');

فقط Class جدید را اضافه می‌کند.

به همین دلیل classList برای تغییرات تدریجی و هدفمند Classها API مناسب‌تری است.

در UI معمولاً چند Class مختلف ممکن است مسئول Layout، Theme، State و Styling باشند. تغییر یک Class نباید به‌صورت ناخواسته بقیه را حذف کند.

وقتی لازم است Style را مستقیماً تغییر دهیم

گاهی تغییر موردنظر مستقیماً به Style مربوط است.

DOM این امکان را از طریق style در اختیار ما قرار می‌دهد:

title.style.fontSize = '2rem';

یا:

title.style.marginBottom = '1rem';

در اینجا JavaScript مستقیماً یک CSS Property را روی همان Element تنظیم می‌کند.

اما این ابزار باید با درک مسئولیت‌ها استفاده شود.

اگر تغییر موردنظر یک وضعیت UI است، معمولاً بهتر است State را با Class بیان کنیم:

modal.classList.add('modal--open');

و Presentation را در CSS نگه داریم.

در این مدل:

JavaScript
↓
State / Behavior

CSS
↓
Presentation

این جداسازی باعث می‌شود JavaScript کمتر به جزئیات Presentation وابسته شود.

بنابراین style ابزار نامناسبی نیست؛ بلکه برای تغییرات مستقیم و موردی مفید است. مسئله زمانی ایجاد می‌شود که تمام منطق Presentation را به JavaScript منتقل کنیم.

یک مسئله، چند نوع تغییر

اکنون می‌توانیم یک Recipe Card را به‌عنوان یک مسئله کامل ببینیم:

<article class="recipe-card">
  <h2 class="recipe-title">Pasta</h2>

<img
class="recipe-image"
src="pasta.jpg"
alt="Pasta"
>

  <button class="favorite-btn">
    Favorite
  </button>
</article>

ابتدا Card را پیدا می‌کنیم:

const card =
document.querySelector('.recipe-card');

سپس Elementهای موردنیاز را در محدوده همان Card پیدا می‌کنیم:

const title =
card.querySelector('.recipe-title');

const image =
card.querySelector('.recipe-image');

const button =
card.querySelector('.favorite-btn');

حالا سه نیاز متفاوت داریم.

عنوان باید تغییر کند:

title.textContent = 'Mushroom Pasta';

تصویر باید تغییر کند:

image.setAttribute(
'src',
'mushroom-pasta.jpg'
);

و وضعیت Favorite باید فعال شود:

button.classList.add(
'favorite-btn--active'
);

سه تغییر داریم، اما سه نوع تغییر متفاوت:

Title
↓
Content

Image
↓
Attribute

Button
↓
Class

این همان نکته اصلی فصل است.

ابتدا Element را پیدا می‌کنیم؛ سپس ماهیت تغییری را که می‌خواهیم ایجاد کنیم تشخیص می‌دهیم؛ بعد API متناسب با آن را انتخاب می‌کنیم.

Selection و Manipulation دو مرحله متفاوت‌اند

اکنون می‌توانیم فرآیند کامل را دقیق‌تر بیان کنیم:

DOM
↓
Selection
↓
Element Reference
↓
Identify the Target
↓
Choose the DOM API
↓
Manipulation

برای مثال:

const title =
document.querySelector('.recipe-title');

title.textContent = recipe.title;

خط اول Selection است.

خط دوم Manipulation است.

این تفکیک فقط برای آموزش نیست.

در Debugging نیز بسیار مفید است.

اگر UI تغییر نکرد، ابتدا باید بپرسیم:

آیا Element درست انتخاب شده است؟

اگر پاسخ مثبت است، سؤال بعدی:

آیا تغییر درست روی آن اعمال شده است؟

با جدا کردن این دو مرحله، پیدا کردن خطا نیز ساده‌تر می‌شود.

وقتی چند Element داریم

یک تفاوت دیگر نیز باید در ذهن باقی بماند.

querySelector() یک Element را برمی‌گرداند:

const button =
document.querySelector('.favorite-btn');

اما querySelectorAll() مجموعه‌ای از Elementها را برمی‌گرداند:

const buttons =
document.querySelectorAll('.favorite-btn');

بنابراین عملیات باید متناسب با نوع نتیجه باشد:

buttons.forEach(button => {
button.classList.add('visible');
});

این موضوع ارتباط مستقیمی با Iteration دارد.

querySelectorAll() به ما مجموعه‌ای از Elementها می‌دهد و سپس می‌توانیم از مفاهیم Iteration که پیش‌تر آموخته‌ایم برای پردازش آن‌ها استفاده کنیم.

به این ترتیب، DOM APIها جدا از مفاهیم قبلی JavaScript نیستند؛ بلکه روی همان مفاهیم ساخته می‌شوند.

یک مدل ذهنی برای کار با DOM

اکنون تمام مسیر فصل را می‌توان در یک مدل ساده خلاصه کرد.

وقتی قرار است بخشی از UI را تغییر دهیم، این چهار سؤال را به‌ترتیب می‌پرسیم:

اول: Element موردنظر کجاست؟

document.querySelector(...)

دوم: یک Element می‌خواهم یا چند Element؟

One
→ querySelector()

Many
→ querySelectorAll()

سوم: دقیقاً چه چیزی باید تغییر کند؟

Text
Attribute
Property
Class
Style

چهارم: API مناسب آن چیست؟

Text
→ textContent

HTML
→ innerHTML

Attribute
→ getAttribute / setAttribute

Property
→ DOM Property

Class
→ classList

Style
→ style

بنابراین به‌جای حفظ کردن مجموعه‌ای از Methodها، می‌توانیم از یک فرآیند تصمیم‌گیری استفاده کنیم:

Find
↓
Identify
↓
Choose
↓
Change

این مدل ذهنی در کار با DOM از حفظ کردن Syntax چند API مهم‌تر است.

Best Practices
Element را قبل از Manipulation پیدا کنید

Selection و Manipulation را از نظر ذهنی دو مرحله جدا در نظر بگیرید.

const title =
document.querySelector('.recipe-title');

title.textContent = recipe.title;

این ساختار خواناتر و قابل Debugتر است.

برای Text از textContent استفاده کنید

اگر هدف فقط نمایش Text است، از HTML Parsing استفاده نکنید.

element.textContent = message;
Attribute و Property را با یکدیگر اشتباه نگیرید

قبل از تغییر Element مشخص کنید آیا با Markup کار دارید یا وضعیت فعلی DOM Object.

برای UI State از classList استفاده کنید

وقتی هدف تغییر وضعیت ظاهری است، Class معمولاً انتخاب مناسبی است:

element.classList.toggle('active');
Scope جستجو را محدود کنید

اگر Element موردنظر داخل یک Parent مشخص قرار دارد، Query را از همان Parent شروع کنید.

const card =
document.querySelector('.recipe-card');

const title =
card.querySelector('.recipe-title');
innerHTML را فقط زمانی استفاده کنید که واقعاً HTML لازم است

برای Text ساده، textContent انتخاب صریح‌تر و امن‌تری است.

امکان null را در Selection در نظر بگیرید

هر Query الزاماً موفق نیست.

const title =
document.querySelector('.recipe-title');

if (title) {
// ...
}
اشتباهات رایج
تصور اینکه querySelector() همه Matchها را برمی‌گرداند

querySelector() فقط اولین Match را برمی‌گرداند.

برای تمام Matchها باید از querySelectorAll() استفاده شود.

استفاده از innerHTML برای Text ساده

اگر هدف نمایش Text است، textContent انتخاب مناسب‌تری است.

یکی دانستن Attribute و Property

این دو مفهوم مرتبط‌اند، اما یکسان نیستند.

Attribute بخشی از HTML Markup است؛ Property بخشی از DOM Object است.

جایگزین کردن کامل className

تغییر className می‌تواند Classهای موجود را حذف کند.

در بسیاری از سناریوها classList انتخاب دقیق‌تری است.

تغییر مستقیم Style برای تمام وضعیت‌های UI

style برای تغییر مستقیم CSS مفید است، اما Stateهای UI را می‌توان با Class بهتر از Presentation جدا کرد.

فرض کردن اینکه Element همیشه وجود دارد

Query ممکن است null برگرداند.

پس وجود Element باید در Logic مناسب در نظر گرفته شود.

Summary

کار با DOM از Manipulation شروع نمی‌شود.

ابتدا باید Element موردنظر را پیدا کنیم.

برای Selection، querySelector() اولین Element مطابق CSS Selector را پیدا می‌کند و querySelectorAll() تمام Matchها را برمی‌گرداند. در صورت نیاز نیز می‌توان از getElementById() برای Selection بر اساس id استفاده کرد.

اما بعد از Selection، مسئله مهم‌تری مطرح می‌شود:

چه چیزی از Element باید تغییر کند؟

اگر Text موردنظر باشد، textContent ابزار اصلی است.

اگر واقعاً HTML Markup لازم باشد، innerHTML امکان تغییر HTML داخلی را فراهم می‌کند.

اگر با HTML Attribute کار کنیم، getAttribute() و setAttribute() در اختیار ما هستند.

اگر با وضعیت Object مربوط به Element کار کنیم، Propertyهای DOM مانند value، src و href قابل استفاده‌اند.

برای مدیریت Classها، classList امکان Add، Remove، Toggle و بررسی Class را فراهم می‌کند.

و برای تغییر مستقیم CSS می‌توان از style استفاده کرد.

در نتیجه، مدل کامل فصل چنین است:

DOM
↓
Selection
↓
Element Reference
↓
Identify the Target
↓
Content / Attribute / Property / Class / Style
↓
Choose the Appropriate API
↓
Manipulation

این مدل ذهنی باعث می‌شود DOM APIها به مجموعه‌ای از Methodهای پراکنده تبدیل نشوند.

هر API پاسخی به یک نیاز مشخص است.

Key Takeaways
DOM ساختار صفحه را در قالب Nodeها و Elementها در اختیار JavaScript قرار می‌دهد.
Selection یعنی پیدا کردن Element موردنظر و به‌دست‌آوردن Reference آن.
querySelector() اولین Element مطابق Selector را برمی‌گرداند.
querySelectorAll() تمام Elementهای مطابق Selector را برمی‌گرداند.
Query را می‌توان از یک Parent Element نیز انجام داد.
اگر Element پیدا نشود، Selection معمولاً null برمی‌گرداند.
textContent برای مدیریت Text مناسب است.
innerHTML برای HTML Markup است و باید با دقت استفاده شود.
HTML Attribute و DOM Property یک مفهوم واحد نیستند.
getAttribute() و setAttribute() برای مدیریت Attributeها استفاده می‌شوند.
Propertyهای DOM می‌توانند وضعیت فعلی Element را نشان دهند.
classList برای مدیریت Classهای Element طراحی شده است.
style امکان تغییر مستقیم CSS را فراهم می‌کند.
Selection و Manipulation دو مرحله متفاوت‌اند.
نوع تغییر باید تعیین کند از کدام DOM API استفاده شود.
هدف اصلی حفظ کردن APIها نیست؛ هدف ساختن یک مدل تصمیم‌گیری برای کار با DOM است.
Technical Interview
Junior Level
سؤال ۱: querySelector() چیست؟

پاسخ:

querySelector() یک CSS Selector دریافت می‌کند و اولین Element مطابق آن را برمی‌گرداند. اگر Match وجود نداشته باشد، null برمی‌گرداند.

سؤال ۲: تفاوت querySelector() و querySelectorAll() چیست؟

پاسخ:

querySelector() اولین Match را برمی‌گرداند، در حالی که querySelectorAll() تمام Matchها را برمی‌گرداند.

سؤال ۳: چگونه Text یک Element را تغییر می‌دهید؟

پاسخ:

با استفاده از textContent:

element.textContent = 'New title';
سؤال ۴: چگونه یک Class را به Element اضافه می‌کنید؟

پاسخ:

با classList.add():

element.classList.add('active');
Mid-Level
سؤال ۵: تفاوت Attribute و Property چیست؟

پاسخ:

Attribute بخشی از HTML Markup است، در حالی که Property بخشی از DOM Object مربوط به Element است و می‌تواند وضعیت فعلی آن Element را نشان دهد.

سؤال ۶: تفاوت textContent و innerHTML چیست؟

پاسخ:

textContent با Text کار می‌کند، در حالی که innerHTML محتوای HTML داخلی Element را به‌عنوان Markup پردازش می‌کند.

سؤال ۷: اگر querySelector() هیچ Elementی پیدا نکند چه اتفاقی می‌افتد؟

پاسخ:

null برمی‌گرداند. بنابراین دسترسی مستقیم به Propertyهای نتیجه بدون بررسی می‌تواند باعث خطا شود.

سؤال ۸: چرا classList معمولاً برای تغییر Class مناسب‌تر از className است؟

پاسخ:

زیرا می‌توان یک Class را بدون جایگزین کردن کل مقدار class اضافه، حذف یا Toggle کرد و در نتیجه Classهای موجود را حفظ کرد.

Senior Level
سؤال ۹: چگونه تصمیم می‌گیرید از textContent، Attribute، Property، classList یا style استفاده کنید؟

پاسخ:

ابتدا نوع State یا داده‌ای را که باید تغییر کند مشخص می‌کنم. برای Text از textContent، برای HTML داخلی در صورت نیاز از innerHTML، برای HTML Attribute از getAttribute() و setAttribute()، برای وضعیت DOM از Propertyهای Element، برای Stateهای ظاهری از classList و برای تغییر مستقیم CSS از style استفاده می‌کنم.

سؤال ۱۰: چرا Selection و Manipulation را از یکدیگر جدا می‌کنیم؟

پاسخ:

زیرا دو مسئولیت متفاوت هستند: Selection مشخص می‌کند با کدام Element کار می‌کنیم و Manipulation مشخص می‌کند چه تغییری روی آن انجام می‌دهیم. این جداسازی خوانایی و Debugging را بهتر می‌کند.

سؤال ۱۱: چرا استفاده از Class برای UI State معمولاً از تغییر مستقیم Style مناسب‌تر است؟

پاسخ:

زیرا JavaScript را از جزئیات Presentation جدا می‌کند. JavaScript State یا Behavior را تغییر می‌دهد و CSS مسئول Presentation باقی می‌ماند.

سؤال ۱۲: چرا querySelectorAll() با یک Element واحد متفاوت است؟

پاسخ:

زیرا querySelectorAll() مجموعه‌ای از Elementهای مطابق Selector را برمی‌گرداند. بنابراین برای اعمال عملیات روی اعضای آن باید با مجموعه تعامل کرد، مثلاً با forEach().

Golden Answers
DOM Selection چیست؟

فرآیند پیدا کردن یک یا چند Element از ساختار DOM و به‌دست‌آوردن Reference آن‌ها برای خواندن یا تغییر است.

تفاوت querySelector() و querySelectorAll() چیست؟

querySelector() اولین Match را برمی‌گرداند؛ querySelectorAll() تمام Matchها را برمی‌گرداند.

Attribute و Property چه تفاوتی دارند؟

Attribute بخشی از HTML Markup است؛ Property بخشی از DOM Object مربوط به Element است و می‌تواند وضعیت فعلی آن را نشان دهد.

چه زمانی از textContent استفاده می‌کنیم؟

زمانی که هدف خواندن یا تغییر Text است و نیازی به تفسیر HTML نداریم.

چرا classList اهمیت دارد؟

زیرا اجازه می‌دهد Classهای یک Element را به‌صورت مستقل اضافه، حذف، Toggle یا بررسی کنیم بدون اینکه مجبور باشیم کل مقدار class را جایگزین کنیم.

مدل ذهنی اصلی این فصل چیست؟

ابتدا Element را پیدا کن، سپس مشخص کن چه چیزی باید تغییر کند و در نهایت DOM API متناسب با همان نوع تغییر را انتخاب کن.

Conclusion

وقتی JavaScript در Browser اجرا می‌شود، یکی از مهم‌ترین نیازهای آن تعامل با صفحه است.

اما این تعامل از تغییر دادن صفحه آغاز نمی‌شود.

ابتدا باید بدانیم با کدام Element قرار است کار کنیم.

Selection این ارتباط را ایجاد می‌کند.

پس از آن باید بدانیم چه چیزی از Element باید تغییر کند.

ممکن است Text باشد، Attribute باشد، Property باشد، Class باشد یا Style.

هرکدام از این نیازها API مناسب خود را دارند.

بنابراین کار حرفه‌ای با DOM به حفظ کردن مجموعه‌ای از Methodها خلاصه نمی‌شود.

مسیر درست این است:

Find
↓
Identify
↓
Choose
↓
Manipulate

این مدل ذهنی به ما اجازه می‌دهد DOM APIها را بر اساس مسئله انتخاب کنیم، نه بر اساس حفظ کردن Syntax.

اما هنوز یک سؤال مهم باقی مانده است.

تا اینجا تمام Elementهایی که با آن‌ها کار کردیم از قبل در DOM وجود داشتند.

اگر Application بخواهد یک Recipe Card جدید ایجاد کند چه؟

اگر بخواهیم یک Element جدید بسازیم، محتوای آن را تنظیم کنیم و سپس آن را به Document اضافه کنیم، دیگر فقط با Selecting و Manipulating Elementهای موجود سروکار نداریم.

این مسئله، موضوع فصل بعد است:

Creating and Modifying DOM Elements