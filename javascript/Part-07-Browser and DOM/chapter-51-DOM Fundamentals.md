Chapter 51 — DOM Fundamentals
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مفهوم DOM را دقیق تعریف کنید.
تفاوت HTML Document و DOM را توضیح دهید.
نقش document را در دسترسی JavaScript به DOM تحلیل کنید.
مفهوم Node را در ساختار DOM درک کنید.
تفاوت Node و Element را تشخیص دهید.
ساختار یک Web Page را به‌صورت یک Tree مدل کنید.
رابطه Parent، Child و Sibling را در ساختار DOM توضیح دهید.
توضیح دهید چرا DOM بخشی از JavaScript Language نیست.
مرز میان JavaScript و Browser APIs را تشخیص دهید.
توضیح دهید چگونه Browser یک Document را به ساختاری قابل دسترسی برای JavaScript تبدیل می‌کند.
تفاوت مدل Document در HTML و مدل Objectهای JavaScript را در سطح مفهومی تحلیل کنید.
Core Question

JavaScript چگونه ساختار یک Web Page را به‌صورت قابل دستکاری مدل می‌کند؟

جریان این فصل:

Document
↓
DOM
↓
Node
↓
Element
↓
Tree
↓
Document Structure
↓
JavaScript / DOM Boundary

این فصل درباره مدل‌سازی Document در Browser است.

در اینجا هنوز وارد روش‌های انتخاب Elementها، تغییر Content، Attributes، ایجاد Nodeهای جدید یا Eventها نمی‌شویم. این مفاهیم زمانی معنا پیدا می‌کنند که ابتدا مدل ذهنی صحیحی از DOM داشته باشیم و در فصل‌های بعد بررسی خواهند شد.

مقدمه

تا اینجا بیشتر مفاهیمی که یاد گرفته‌ایم، در محدوده خود JavaScript قرار داشتند.

Valueها، Variableها، Functionها، Objectها، Arrayها و حتی Classها بخشی از مدل زبان JavaScript هستند.

اما وقتی JavaScript در Browser اجرا می‌شود، مسئله دیگری به وجود می‌آید.

فرض کنید یک صفحه Web چنین ساختاری دارد:

<body>
  <main>
    <h1>Recipe Hub</h1>
    <p>Search for a recipe.</p>
  </main>
</body>

این صفحه برای کاربر فقط مجموعه‌ای از متن‌ها و عناصر قابل مشاهده است.

اما JavaScript باید بتواند ساختار همین صفحه را نیز درک کند.

برای مثال، باید بتواند از نظر مفهومی بفهمد:

body
└── main
├── h1
└── p

سؤال این است:

Browser چگونه HTML را به ساختاری تبدیل می‌کند که JavaScript بتواند آن را ببیند و با آن کار کند؟

پاسخ این سؤال ما را به مفهوم DOM می‌رساند.

از Document به DOM

کلمه DOM مخفف:

Document Object Model

است.

برای درک این نام، ابتدا باید خود Document را بشناسیم.

در Browser، یک Web Page یک Document دارد.

Document نماینده صفحه‌ای است که Browser در حال نمایش و مدیریت آن است.

اما Document فقط یک فایل HTML خام نیست.

Browser هنگام پردازش HTML، ساختاری از آن ایجاد می‌کند که اجزای Document را به‌صورت Objectهایی در اختیار محیط Browser قرار می‌دهد.

این ساختار همان چیزی است که DOM نامیده می‌شود.

بنابراین می‌توانیم رابطه را چنین ببینیم:

HTML Document
↓
Browser Processing
↓
DOM
↓
JavaScript Access

در نتیجه DOM را نباید صرفاً «HTML صفحه» بدانیم.

HTML یک Markup Language برای توصیف ساختار Document است.

DOM یک Object Model است که همان ساختار را در محیط Browser قابل دسترسی و تعامل می‌کند.

این تفاوت بسیار مهم است.

DOM چیست؟
تعریف ساده

DOM مدلی درختی و Object-based از Document است که Browser در اختیار JavaScript و سایر APIهای Web قرار می‌دهد.

به زبان ساده:

DOM نسخه‌ای قابل دسترسی از ساختار Document در حافظه Browser است.

اگر HTML یک صفحه چنین باشد:

<main>
  <h1>Recipe Hub</h1>
  <p>Search for a recipe.</p>
</main>

DOM ساختار آن را به‌صورت مفهومی شبیه این مدل می‌کند:

Document
│
└── main
├── h1
└── p

در این مدل، main، h1 و p فقط متن HTML نیستند.

آن‌ها Nodeهایی در ساختار DOM هستند که Browser آن‌ها را به‌صورت Objectهای قابل دسترسی مدل می‌کند.

بنابراین JavaScript مستقیماً با متن HTML کار نمی‌کند؛ بلکه از طریق DOM با مدل Object-based مربوط به Document تعامل دارد.

DOM یک Object واحد نیست

گاهی DOM را به‌گونه‌ای بیان می‌کنیم که انگار یک Object واحد است.

این بیان برای شروع مفید است، اما از نظر فنی ناقص است.

DOM در واقع یک ساختار از Nodeها است.

در رأس این ساختار، Document قرار دارد.

از آن Nodeهای دیگری منشعب می‌شوند و این ارتباط تا Elementها، Text Nodeها و سایر بخش‌های Document ادامه پیدا می‌کند.

مدل ذهنی بهتر این است:

DOM
│
├── Document
│
├── Element Nodes
│
├── Text Nodes
│
├── Comment Nodes
│
└── Other Node Types

بنابراین وقتی درباره DOM صحبت می‌کنیم، درباره یک Object منفرد صحبت نمی‌کنیم؛ بلکه درباره یک مدل ساختاری از Document صحبت می‌کنیم.

Node؛ واحدهای تشکیل‌دهنده DOM

اکنون یک سؤال طبیعی ایجاد می‌شود:

اگر DOM یک Tree از Objectها است، این Objectها چه هستند؟

پاسخ کلی:

Node

Node یک مفهوم عمومی در مدل DOM است.

انواع مختلفی از Nodeها می‌توانند در DOM وجود داشته باشند.

برای مثال:

Document
Element
Text
Comment

همه بخشی از مدل Node-based DOM هستند.

این نکته اهمیت زیادی دارد، زیرا هر چیزی که در DOM وجود دارد الزاماً یک HTML Element نیست.

برای مثال، در:

<p>Hello</p>

حداقل از دید مدل DOM، با ساختاری شامل:

p Element
└── "Hello" Text

مواجه هستیم.

p یک Element است.

اما "Hello" یک Text Node است.

هر دو در DOM حضور دارند، اما نوع آن‌ها یکسان نیست.

Element در برابر Node

این یکی از مهم‌ترین تفاوت‌های مفهومی این فصل است.

Element یک نوع خاص از Node است.

بنابراین رابطه را می‌توان چنین نمایش داد:

Node
│
├── Document
├── Element
├── Text
└── Comment

پس:

هر Element یک Node است، اما هر Node یک Element نیست.

برای مثال:

<h1>Recipe Hub</h1>

در این ساختار:

h1

یک Element است.

اما متن:

Recipe Hub

یک Text Node است.

هر دو Node هستند.

اما فقط h1 یک Element است.

این تفاوت در ادامه کتاب اهمیت زیادی پیدا می‌کند؛ زیرا بسیاری از APIهای DOM مشخصاً با Elementها کار می‌کنند، در حالی که برخی APIها با مفهوم عمومی‌تر Node سروکار دارند.

چرا DOM یک Tree است؟

تا اینجا می‌دانیم DOM از Nodeها تشکیل شده است.

اما چرا آن‌ها را به‌صورت Tree سازمان‌دهی می‌کند؟

پاسخ را باید در خود ساختار Document جست‌وجو کنیم.

HTML ذاتاً ساختاری تو در تو دارد.

برای مثال:

<body>
  <main>
    <section>
      <h1>Recipe Hub</h1>
    </section>
  </main>
</body>

در اینجا:

body
└── main
└── section
└── h1

هر Element می‌تواند Element دیگری را درون خود داشته باشد.

این رابطه تو در تو، به‌صورت طبیعی یک ساختار درختی ایجاد می‌کند.

بنابراین DOM این ساختار را حفظ می‌کند.

مدل ذهنی اصلی این فصل باید چنین باشد:

Document
↓
Tree
↓
Nodes
↓
Relationships

DOM فقط مجموعه‌ای از Elementهای مستقل نیست.

رابطه میان Nodeها بخش مهمی از مدل DOM است.

Parent و Child

وقتی DOM را به‌صورت Tree ببینیم، اصطلاحات Parent و Child معنا پیدا می‌کنند.

در این ساختار:

main
├── h1
└── p

main نسبت به h1 و p نقش Parent دارد.

و h1 و p نسبت به main نقش Child دارند.

اگر یک Node مستقیماً درون Node دیگری قرار گرفته باشد، رابطه Parent/Child میان آن‌ها وجود دارد.

برای مثال:

body
└── main
└── section
└── h1

در این ساختار:

body → Parent of main
main → Parent of section
section → Parent of h1

و در جهت عکس:

main → Child of body
section → Child of main
h1 → Child of section

این روابط بعداً امکان حرکت در ساختار DOM را فراهم می‌کنند.

اما فعلاً مهم‌ترین نکته این است:

DOM علاوه بر خود Nodeها، روابط میان آن‌ها را نیز مدل می‌کند.

Sibling Nodes

اگر دو Node یک Parent مشترک داشته باشند، نسبت به یکدیگر Sibling هستند.

برای مثال:

main
├── h1
└── p

در اینجا:

h1
p

Sibling هستند؛ زیرا هر دو Child یک main هستند.

این مفهوم از نظر مهندسی مهم است، زیرا DOM فقط یک مسیر عمودی از Parent به Child نیست.

یک Node می‌تواند:

Parent داشته باشد.
Child داشته باشد.
Sibling داشته باشد.

در نتیجه DOM یک ساختار ارتباطی کامل است، نه فقط یک لیست از Elementها.

Document Structure

اکنون می‌توانیم یک Web Page را در سطح مفهومی کامل‌تر ببینیم.

برای مثال:

<!DOCTYPE html>
<html>
  <head>
    <title>Recipe Hub</title>
  </head>
  <body>
    <main>
      <h1>Recipe Hub</h1>
      <p>Search for a recipe.</p>
    </main>
  </body>
</html>

مدل ذهنی ساده‌شده DOM آن می‌تواند چنین باشد:

Document
│
└── html
├── head
│    └── title
│         └── "Recipe Hub"
│
└── body
└── main
├── h1
│    └── "Recipe Hub"
│
└── p
└── "Search for a recipe."

این ساختار دقیقاً همان چیزی است که مفهوم Tree را در DOM معنا می‌کند.

درخت از یک نقطه شروع می‌شود و Nodeهای فرزند از آن منشعب می‌شوند.

document چیست؟

در محیط Browser، JavaScript برای دسترسی به Document یک Object مهم در اختیار دارد:

document

document نماینده Document جاری است.

بنابراین وقتی JavaScript در یک Web Page اجرا می‌شود، document نقطه مهمی برای ورود به مدل DOM است.

مدل ذهنی:

Browser
↓
Document
↓
DOM Tree
↓
Nodes / Elements

در نتیجه document را نباید با خود HTML Source Code یکی بدانیم.

document یک Object است که Browser برای نمایش و مدیریت Document در محیط Web در اختیار Script قرار می‌دهد.

DOM و JavaScript یک چیز نیستند

یکی از مهم‌ترین نکات این فصل، تشخیص مرز میان JavaScript Language و DOM است.

JavaScript یک Programming Language است.

DOM بخشی از Specification زبان ECMAScript نیست.

DOM یک Web Platform API است که توسط محیط Browser ارائه می‌شود.

به همین دلیل JavaScript می‌تواند خارج از Browser نیز اجرا شود.

مثلاً یک JavaScript Runtime می‌تواند بدون وجود یک Web Page اجرا شود.

در چنین محیطی الزاماً:

document

وجود ندارد.

این موضوع نشان می‌دهد که document بخشی از خود زبان JavaScript نیست.

مدل ذهنی صحیح:

JavaScript Language
│
│
↓
JavaScript Runtime
│
│
└──── Browser Host APIs
│
└──── DOM

بنابراین:

JavaScript می‌تواند با DOM کار کند، اما DOM خود JavaScript نیست.

این همان مرزی است که در فصل 50 میان Language، Runtime و Browser Host Environment ساخته شد.

Browser Host Environment و DOM

در فصل قبل دیدیم که Browser فقط یک JavaScript Engine نیست.

Browser علاوه بر Engine، مجموعه‌ای از قابلیت‌ها و APIهای مخصوص محیط Web در اختیار برنامه قرار می‌دهد.

DOM یکی از مهم‌ترین این APIها است.

پس وقتی می‌نویسیم:

document

در واقع از یک قابلیت فراهم‌شده توسط محیط Browser استفاده می‌کنیم.

به همین دلیل بهتر است معماری ذهنی خود را چنین نگه داریم:

JavaScript
│
↓
Engine
│
↓
Browser Runtime
│
├── DOM
├── Web APIs
└── Other Browser Capabilities

این تفکیک در توسعه Frontend بسیار مهم است.

زیرا در پروژه واقعی باید بدانیم کدام قابلیت متعلق به زبان است و کدام قابلیت توسط محیط اجرا فراهم می‌شود.

HTML Source و DOM Tree دقیقاً یکی نیستند

ممکن است این تصور ایجاد شود که DOM فقط یک کپی مستقیم از متن HTML است.

این تصور دقیق نیست.

HTML Source یک متن است.

DOM یک ساختار Object-based است.

برای مثال:

HTML
↓
Textual Markup

در مقابل:

DOM
↓
Object Model
↓
Tree of Nodes

Browser HTML را Parse می‌کند و بر اساس آن ساختار DOM را ایجاد می‌کند.

پس رابطه را بهتر است این‌گونه ببینیم:

HTML Source
↓
Parse
↓
DOM Tree

این نکته یک تفاوت بنیادی ایجاد می‌کند:

HTML چیزی است که Browser آن را پردازش می‌کند.

DOM چیزی است که Browser در نتیجه این پردازش برای مدل‌سازی Document در اختیار محیط Web قرار می‌دهد.

DOM یک Snapshot ساده از HTML نیست

DOM را نباید مانند یک فایل HTML تصور کنیم که فقط یک بار خوانده شده است.

DOM یک مدل زنده از Document در محیط Browser است.

اگر ساختار Document در طول اجرای برنامه تغییر کند، مدل DOM نیز می‌تواند تغییر کند.

این ویژگی یکی از دلایل اصلی وجود DOM است.

اگر DOM فقط یک تصویر ثابت از HTML اولیه بود، JavaScript نمی‌توانست UI را در طول اجرای Application با وضعیت جدید هماهنگ کند.

در این فصل هنوز روش انجام این تغییرات را بررسی نمی‌کنیم.

فقط باید مسئله را بشناسیم:

Document
↓
DOM Model
↓
JavaScript
↓
Interaction with Document

روش‌های انتخاب و تغییر Nodeها در فصل‌های بعد بررسی خواهند شد.

چرا Browser به DOM نیاز دارد؟

اکنون می‌توانیم به سؤال اصلی فصل برگردیم.

اگر Browser فقط HTML را نمایش می‌داد، شاید به چنین مدل پیچیده‌ای نیاز نبود.

اما یک Web Application مدرن فقط یک Document ثابت نیست.

رابط کاربری می‌تواند بر اساس وضعیت Application تغییر کند.

برای مثال:

Search
↓
Result
↓
User Interaction
↓
UI Update

JavaScript برای هماهنگ کردن UI با وضعیت برنامه باید بتواند ساختار Document را بشناسد.

DOM این مدل را فراهم می‌کند.

بنابراین مسئله اصلی DOM این نیست که فقط HTML را «نمایش» دهد.

مسئله این است که:

ساختار Document را به مدلی تبدیل کند که برنامه بتواند آن را در محیط Browser درک و با آن تعامل کند.

این همان دلیل وجود DOM در معماری Web است.

DOM به‌عنوان یک Boundary

اکنون می‌توانیم یک مدل مهندسی مهم‌تر بسازیم.

تا اینجا بیشتر با JavaScript و Object Model خود زبان کار کرده‌ایم.

اما در Browser به یک مرز می‌رسیم:

JavaScript Application
│
│
↓
Browser API
│
↓
DOM
│
↓
Web Document

JavaScript از طریق APIهای Browser به Document دسترسی پیدا می‌کند.

این مرز از نظر معماری مهم است.

برای مثال، Array یک مفهوم زبان JavaScript است.

اما document یک قابلیت Web Platform است.

هر دو در کد JavaScript قابل استفاده‌اند، اما منشأ یکسانی ندارند.

این تفاوت به ما کمک می‌کند از یک اشتباه رایج جلوگیری کنیم:

هر چیزی که با JavaScript نوشته می‌شود، الزاماً بخشی از JavaScript Language نیست.

DOM Tree و Object Model

اکنون سه مفهوم اصلی را کنار هم قرار دهیم:

Document
↓
DOM
↓
Tree of Nodes

Document همان Web Page مورد نظر است.

DOM مدل Object-based آن Document است.

Tree ساختار ارتباط Nodeهای DOM را نشان می‌دهد.

و Node واحدهای مختلف این ساختار هستند.

بنابراین اگر بخواهیم یک مدل ذهنی واحد داشته باشیم:

Web Page
↓
Document
↓
DOM
↓
Tree
↓
Nodes
↓
Elements / Text / Comments / ...

این مدل پایه تمام کارهای بعدی با DOM است.

یک مثال کامل

فرض کنید صفحه‌ای از یک Recipe Application داریم:

<body>
  <main>
    <h1>Recipe Hub</h1>

    <section>
      <h2>Search Results</h2>
      <p>12 recipes found.</p>
    </section>
  </main>
</body>

مدل ساده‌شده آن:

Document
└── body
└── main
├── h1
│    └── "Recipe Hub"
│
└── section
├── h2
│    └── "Search Results"
│
└── p
└── "12 recipes found."

اکنون می‌توانیم چند مفهوم را هم‌زمان ببینیم.

main یک Element است.

section یک Element است.

متن "Search Results" یک Text Node است.

main والد section است.

h2 و p فرزندان section هستند.

و h2 و p نسبت به یکدیگر Sibling هستند.

این دقیقاً همان ساختاری است که DOM برای Document مدل می‌کند.

DOM و Rendering یکی نیستند

یک سوءبرداشت دیگر این است که DOM همان چیزی است که کاربر روی صفحه می‌بیند.

این دو مفهوم به هم مرتبط‌اند، اما یکی نیستند.

DOM ساختار Document را مدل می‌کند.

Rendering فرآیند Browser برای تبدیل ساختار و اطلاعات صفحه به خروجی قابل نمایش است.

بنابراین:

HTML / DOM
↓
Browser Rendering Process
↓
Visual Web Page

DOM یک Tree از Nodeها است.

آنچه کاربر می‌بیند نتیجه فرآیندهای Rendering Browser است.

این تفاوت در مباحث Performance و Rendering در سطح پیشرفته اهمیت بیشتری پیدا می‌کند، اما برای این فصل همین تفکیک کافی است.

DOM یک Abstraction است

DOM را می‌توان یک Abstraction نیز در نظر گرفت.

JavaScript لازم نیست مستقیماً با جزئیات داخلی Browser برای نمایش یک Document کار کند.

به‌جای آن، Browser یک مدل استاندارد ارائه می‌کند:

Document
Node
Element
Tree

JavaScript از این مدل استفاده می‌کند.

بنابراین DOM یک لایه Abstraction میان Application و ساختار داخلی Document در Browser ایجاد می‌کند.

مدل ذهنی:

Application Code
↓
DOM API
↓
DOM Object Model
↓
Browser Document

این Abstraction یکی از دلایل مهم قابل استفاده بودن JavaScript در محیط Web است.

آنچه DOM نیست

برای جلوگیری از شکل‌گیری مدل ذهنی اشتباه، چند مرز را روشن کنیم.

DOM:

خود HTML Source Code نیست.
خود JavaScript نیست.
فقط مجموعه‌ای از Elementها نیست.
فقط ظاهر صفحه نیست.
فقط یک Object منفرد نیست.
فقط یک Snapshot ثابت از Document نیست.

DOM یک Object Model درختی از Document است که Browser آن را در محیط Web فراهم می‌کند.

این تعریف، هم ساده است و هم برای ادامه کتاب کافی است.

DOM و Object Model جاوااسکریپت

در فصل‌های Objects و OOP، Object را به‌عنوان ساختاری برای نگهداری State و Behavior شناختیم.

DOM نیز از Objectها استفاده می‌کند، اما باید مراقب یک سوءبرداشت باشیم.

DOM یک Extension از Object Model زبان JavaScript نیست.

به بیان دقیق‌تر، Browser APIهای DOM را در اختیار JavaScript قرار می‌دهد و این APIها از Objectها و Interfaceهای مشخصی استفاده می‌کنند.

پس این دو مفهوم را جدا نگه داریم:

JavaScript Object Model

و:

DOM Object Model

هر دو با Objectها کار می‌کنند، اما یکی مدل داخلی زبان و دیگری یک Web Platform API است.

چرا تفاوت Node و Element در پروژه واقعی مهم است؟

فرض کنید در یک Application با DOM کار می‌کنیم.

اگر تصور کنیم تمام Nodeها Element هستند، ممکن است در زمان تحلیل ساختار Document دچار خطا شویم.

مثلاً:

<p>Hello <strong>Omid</strong></p>

ساختار ساده‌شده:

p
├── "Hello "
└── strong
└── "Omid"

در اینجا:

p       → Element
"Hello" → Text Node
strong  → Element
"Omid"  → Text Node

پس Tree فقط از Tagها ساخته نشده است.

Text نیز بخشی از Tree است.

این مدل ذهنی در فصل‌های بعد، هنگام کار با Content و Traversal اهمیت پیدا می‌کند.

یک مرز مهم: DOM در Browser

چون DOM یک Web API است، وجود آن به محیط اجرا وابسته است.

در Browser:

document

یک نقطه دسترسی استاندارد به Document جاری است.

اما در یک محیط JavaScript که Browser APIs را فراهم نمی‌کند، الزاماً چنین Objectی وجود ندارد.

بنابراین اگر کدی مانند:

document.querySelector(...)

را ببینیم، باید تشخیص دهیم که:

querySelector

در اینجا یک قابلیت مربوط به DOM است، نه یک ویژگی عمومی تمام محیط‌های JavaScript.

جزئیات Selection در فصل بعد بررسی خواهد شد.

در این فصل فقط مرز مفهومی آن را می‌شناسیم.

Best Practices
1. DOM را با HTML یکی ندانید

HTML یک Markup Language است.

DOM مدل Object-based همان Document در محیط Browser است.

2. Node و Element را از یکدیگر تفکیک کنید

Element نوعی Node است.

اما Text، Comment و Document نیز Node هستند.

پس:

Element ⊂ Node
3. DOM را به‌صورت Tree تصور کنید

به‌جای تصور DOM به‌عنوان مجموعه‌ای از Elementهای مستقل، همیشه روابط میان Nodeها را نیز در نظر بگیرید:

Parent
Child
Sibling

این مدل ذهنی برای کار با DOM ضروری است.

4. JavaScript و Browser APIs را از هم تفکیک کنید

هر APIای که در JavaScript قابل استفاده است، لزوماً بخشی از ECMAScript نیست.

DOM نمونه مهمی از یک Web Platform API است.

5. document را به‌عنوان نقطه ورود به Document بشناسید

در Browser، document نماینده Document جاری است و یکی از مهم‌ترین نقاط دسترسی به DOM محسوب می‌شود.

6. قبل از Manipulation، ساختار DOM را بفهمید

انتخاب، تغییر و ایجاد Nodeها بدون درک Tree Model معمولاً باعث استفاده مکانیکی از APIها می‌شود.

ابتدا ساختار را بفهمید.

سپس API مناسب را انتخاب کنید.

Common Mistakes
اشتباه اول: تصور اینکه DOM همان HTML است

HTML متن Markup است.

DOM مدل Object-based و درختی Document است.

اشتباه دوم: تصور اینکه DOM فقط شامل Elementها است

Elementها فقط یکی از انواع Node هستند.

Text Node و سایر Nodeها نیز می‌توانند بخشی از DOM باشند.

اشتباه سوم: یکی دانستن Node و Element

این گزاره:

هر Node یک Element است.

نادرست است.

گزاره صحیح:

هر Element یک Node است، اما هر Node الزاماً Element نیست.

اشتباه چهارم: تصور اینکه DOM بخشی از JavaScript Language است

DOM یک Web Platform API است که Browser در اختیار JavaScript قرار می‌دهد.

اشتباه پنجم: تصور اینکه document یک Variable معمولی است که توسط JavaScript ساخته شده است

document نماینده Document جاری در محیط Browser است.

وجود آن به Browser Host Environment مربوط است.

اشتباه ششم: تصور اینکه DOM فقط یک Snapshot ثابت از HTML اولیه است

DOM مدل Document در محیط Browser است و می‌تواند در طول عمر صفحه تغییر کند.

جزئیات این تغییرات در فصل‌های بعد بررسی خواهند شد.

اشتباه هفتم: تصور اینکه DOM همان چیزی است که کاربر می‌بیند

DOM ساختار Document را مدل می‌کند.

آنچه کاربر می‌بیند حاصل فرآیند Rendering Browser است.

اشتباه هشتم: ورود زودهنگام به Selection و Manipulation

در این فصل هنوز لازم نیست جزئیات APIهایی مانند:

document.querySelector(...)

یا:

element.textContent = ...

را یاد بگیریم.

ابتدا باید مدل DOM را درک کنیم.

Selection و Manipulation در فصل‌های بعد بررسی خواهند شد.

Summary

مسئله این فصل از یک سؤال ساده شروع شد:

JavaScript چگونه می‌تواند ساختار یک Web Page را درک کند؟

HTML به‌تنهایی یک متن Markup است.

Browser این Document را پردازش می‌کند و یک مدل Object-based از آن ایجاد می‌کند که DOM نام دارد.

DOM یک Object منفرد نیست.

بلکه ساختاری از Nodeها است که به‌صورت یک Tree سازمان‌دهی شده‌اند.

در این Tree، Nodeها می‌توانند رابطه‌هایی مانند:

Parent
Child
Sibling

داشته باشند.

Element یکی از انواع Node است.

بنابراین:

Element → Node

اما:

Node → Element

همیشه صحیح نیست.

Document نماینده Document جاری است و در Browser نقطه مهمی برای دسترسی به DOM محسوب می‌شود.

از طرف دیگر، DOM بخشی از خود JavaScript Language نیست.

DOM یک Web Platform API است که Browser در اختیار JavaScript قرار می‌دهد.

پس مدل ذهنی نهایی این فصل:

HTML Document
↓
Browser
↓
DOM
↓
Tree of Nodes
↓
Element / Text / ...
↓
JavaScript Access

اکنون می‌دانیم DOM چیست و چرا وجود دارد.

در فصل بعد، سؤال طبیعی این خواهد بود:

وقتی DOM را به‌عنوان یک Tree داریم، چگونه Element موردنظر خود را پیدا کنیم و آن را تغییر دهیم؟

Key Takeaways
DOM مخفف Document Object Model است.
DOM مدل Object-based و ساختاریافته‌ای از Document است.
DOM توسط Browser به‌عنوان بخشی از Web Platform در اختیار JavaScript قرار می‌گیرد.
HTML و DOM یک مفهوم نیستند.
HTML یک Markup Language است.
DOM یک Object Model از Document است.
DOM ساختاری Tree-like دارد.
Node واحد عمومی در ساختار DOM است.
Element یک نوع خاص از Node است.
هر Element یک Node است، اما هر Node یک Element نیست.
Text نیز می‌تواند به‌صورت Text Node در DOM وجود داشته باشد.
Nodeها می‌توانند روابط Parent، Child و Sibling داشته باشند.
document نماینده Document جاری در Browser است.
DOM بخشی از ECMAScript Language نیست.
Browser Host Environment APIهای DOM را فراهم می‌کند.
DOM و Rendering یک مفهوم نیستند.
DOM ساختار Document را مدل می‌کند.
DOM را نباید فقط مجموعه‌ای از Elementها تصور کرد.
درک Tree Model پیش‌نیاز کار صحیح با DOM APIها است.
Selection و Manipulation بر پایه همین مدل در فصل‌های بعد ساخته می‌شوند.
Technical Interview
سطح Junior
1. DOM چیست؟

DOM مخفف Document Object Model است و یک مدل Object-based و درختی از Document است که Browser در اختیار JavaScript قرار می‌دهد.

2. آیا DOM بخشی از JavaScript است؟

خیر.

DOM بخشی از Web Platform است که Browser آن را فراهم می‌کند. JavaScript از طریق DOM API با Document تعامل می‌کند.

3. تفاوت HTML و DOM چیست؟

HTML یک Markup Language و متن ساختاریافته است، در حالی که DOM یک Object Model درختی است که Browser از Document ایجاد می‌کند.

4. Node چیست؟

Node یک واحد عمومی در ساختار DOM است. Document، Element، Text و Comment نمونه‌هایی از Node هستند.

5. Element چیست؟

Element نوعی Node است که یک عنصر Document را مدل می‌کند؛ مانند div، p یا button.

6. آیا هر Node یک Element است؟

خیر.

هر Element یک Node است، اما Nodeهای دیگری مانند Document و Text نیز وجود دارند.

7. چرا DOM را Tree می‌نامیم؟

زیرا Nodeهای Document بر اساس روابط ساختاری مانند Parent و Child به‌صورت سلسله‌مراتبی سازمان‌دهی می‌شوند.

سطح Mid-Level
8. نقش document در DOM چیست؟

document نماینده Document جاری در Browser است و نقطه اصلی دسترسی JavaScript به مدل DOM آن Document محسوب می‌شود.

9. چرا DOM را نباید با HTML Source یکی دانست؟

HTML Source یک متن Markup است، اما DOM یک ساختار Object-based است که Browser پس از پردازش Document ایجاد می‌کند.

10. رابطه Node و Element چیست؟

Element یک نوع خاص از Node است. بنابراین مجموعه Elementها زیرمجموعه‌ای از مجموعه Nodeها هستند.

11. چرا Text Node اهمیت دارد؟

زیرا محتوای متنی Elementها نیز در مدل DOM به‌عنوان Node مدل می‌شود. بنابراین DOM فقط از Elementها تشکیل نشده است.

12. DOM چه مسئله‌ای را برای JavaScript حل می‌کند؟

DOM ساختار Document را به یک مدل قابل دسترسی تبدیل می‌کند تا JavaScript بتواند Document را در محیط Browser شناسایی و با آن تعامل کند.

13. آیا DOM فقط ساختار اولیه HTML را نشان می‌دهد؟

خیر.

DOM مدل Document در محیط Browser است و می‌تواند در طول اجرای Application تغییر کند.

سطح Senior
14. چرا می‌گوییم DOM یک Web Platform API است و نه بخشی از JavaScript Language؟

زیرا DOM توسط محیط Host یعنی Browser فراهم می‌شود، در حالی که ECMAScript خود زبان JavaScript را تعریف می‌کند. بنابراین وجود APIهایی مانند document به Browser Environment وابسته است.

15. چرا تفکیک JavaScript و DOM از نظر معماری مهم است؟

زیرا مشخص می‌کند کدام قابلیت متعلق به خود زبان است و کدام قابلیت توسط Host Environment ارائه می‌شود. این تفکیک هنگام انتقال Code میان محیط‌های مختلف JavaScript اهمیت پیدا می‌کند.

16. چرا DOM را یک Abstraction می‌دانیم؟

زیرا Application لازم نیست با ساختار داخلی Browser برای مدیریت Document مستقیماً کار کند. Browser یک مدل استاندارد از Document را از طریق Nodeها، Elementها و روابط Tree در اختیار Application قرار می‌دهد.

17. چرا DOM را نمی‌توان صرفاً یک Tree از HTML Tagها دانست؟

زیرا DOM علاوه بر Element Nodeها، انواع دیگری مانند Document و Text Node را نیز مدل می‌کند. بنابراین Tree مربوط به DOM، مدل ساختاری کامل‌تری از Document است.

18. تفاوت DOM و Rendering چیست؟

DOM ساختار Document را مدل می‌کند، در حالی که Rendering فرآیندی است که Browser برای تولید خروجی بصری صفحه انجام می‌دهد. DOM یکی از ورودی‌های مهم این فرآیند است، اما خود خروجی بصری نیست.

19. چرا درک DOM Tree قبل از یادگیری DOM Manipulation مهم است؟

زیرا APIهای DOM بر اساس ساختار و روابط Nodeها عمل می‌کنند. بدون درک Tree، استفاده از APIها به حفظ کردن Methodها تبدیل می‌شود و تحلیل رفتار آن‌ها دشوار خواهد شد.

20. مهم‌ترین مدل ذهنی که باید از DOM داشته باشیم چیست؟

DOM را باید یک Object-based Tree Model of a Document بدانیم:

Document
↓
DOM
↓
Tree
↓
Nodes
↓
Elements / Text / Other Nodes

JavaScript از طریق Browser APIs با این مدل تعامل می‌کند.

Golden Answers
DOM چیست؟

DOM یک مدل Object-based و درختی از Document است که Browser ایجاد می‌کند و از طریق Web APIs در اختیار JavaScript قرار می‌دهد.

آیا DOM بخشی از JavaScript است؟

خیر. DOM یک Web Platform API است، نه بخشی از ECMAScript Language.

تفاوت Node و Element چیست؟

Element یک نوع Node است، اما همه Nodeها Element نیستند. Document و Text نیز نمونه‌هایی از Node هستند.

چرا DOM به‌صورت Tree مدل می‌شود؟

زیرا ساختار Document به‌صورت سلسله‌مراتبی و تو در تو است و DOM این روابط را با Parent، Child و Sibling مدل می‌کند.

document چیست؟

document نماینده Document جاری در Browser و یکی از نقاط اصلی دسترسی JavaScript به DOM است.

چرا HTML و DOM یکی نیستند؟

HTML متن Markup است؛ DOM مدل Object-based و درختی همان Document در محیط Browser است.

مهم‌ترین مرز مفهومی این فصل چیست؟

JavaScript یک Language است، Browser یک Host Environment است و DOM یکی از APIهایی است که Browser برای تعامل با Document در اختیار JavaScript قرار می‌دهد.

Conclusion

تا اینجا مسئله از یک Web Page ساده شروع شد.

HTML ساختار Document را توصیف می‌کند، اما JavaScript برای کار با آن به یک مدل قابل دسترسی نیاز دارد.

Browser این مدل را به‌صورت DOM فراهم می‌کند.

DOM یک ساختار Tree از Nodeها است.

در این ساختار، Elementها تنها یکی از انواع Node هستند و روابط Parent، Child و Sibling ساختار Document را شکل می‌دهند.

در نتیجه، وقتی JavaScript در Browser با صفحه Web کار می‌کند، مستقیماً با متن HTML تعامل نمی‌کند؛ بلکه از طریق APIهای Browser با DOM Object Model کار می‌کند.

اکنون پایه مفهومی لازم ساخته شده است.

در فصل بعد از همین نقطه حرکت می‌کنیم و بررسی خواهیم کرد که چگونه Elementهای این Tree را پیدا کنیم و Properties، Attributes، Content، Classes و Styles آن‌ها را تغییر دهیم.