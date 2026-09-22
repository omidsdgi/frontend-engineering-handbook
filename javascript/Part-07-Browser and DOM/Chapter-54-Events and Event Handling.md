Chapter 54 — Events and Event Handling
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

Event را به‌عنوان مکانیزم ارتباط User Interaction با Application تحلیل کنید.
تفاوت Event، Event Target، Event Listener و Event Handler را توضیح دهید.
از addEventListener() برای واکنش به Eventها استفاده کنید.
Event Object و اطلاعات اصلی آن را تحلیل کنید.
تفاوت event.target و event.currentTarget را تشخیص دهید.
مفهوم Default Action را در Browser توضیح دهید.
از preventDefault() برای لغو رفتار پیش‌فرض قابل لغو استفاده کنید.
مرز میان Browser Event و Application Logic را در طراحی UI تشخیص دهید.
Core Question

Browser چگونه User Interaction را به JavaScript منتقل می‌کند؟

جریان اصلی این فصل:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Default Action
↓
preventDefault
مقدمه

در فصل‌های قبل، DOM را به‌عنوان مدلی از ساختار Document شناختیم و یاد گرفتیم چگونه Elementهای آن را پیدا و تغییر دهیم.

اما یک UI فقط ساختاری نیست که JavaScript آن را تغییر دهد.

کاربر باید بتواند با آن تعامل کند.

برای مثال، در یک Recipe Application کاربر ممکن است:

روی دکمه Search کلیک کند.
داخل Search Input تایپ کند.
یک Recipe را انتخاب کند.
یک Form را Submit کند.
روی یک Link کلیک کند.

در همه این موارد، Application باید متوجه شود که چه اتفاقی افتاده است.

مسئله اینجاست که JavaScript نمی‌تواند از قبل بداند کاربر دقیقاً چه زمانی روی Search کلیک خواهد کرد.

پس Application به مکانیزمی نیاز دارد که بتواند بگوید:

اگر یک اتفاق مشخص رخ داد، Logic مربوط به آن را اجرا کن.

این نیاز، ما را به Event می‌رساند.

وقتی Interaction به یک Event تبدیل می‌شود

فرض کنید کاربر روی دکمه Search کلیک می‌کند.

از دید کاربر، فقط یک عمل انجام شده است:

Click

اما برای Browser، این Click یک رخداد قابل شناسایی است.

Browser آن را به یک Event تبدیل می‌کند.

بنابراین:

Event یک رخداد قابل شناسایی در محیط Browser است که Application می‌تواند در پاسخ به آن Logic اجرا کند.

برای مثال:

click
input
change
submit
keydown

هرکدام نوع متفاوتی از Event را نشان می‌دهند.

در اینجا هنوز JavaScript هیچ کاری انجام نداده است.

Event فقط می‌گوید:

یک اتفاق مشخص رخ داده است.

این تفکیک مهم است؛ زیرا Event با Logic برنامه یکی نیست.

برای مثال، click فقط نشان می‌دهد Click اتفاق افتاده است. اینکه Application در پاسخ به این Click چه کاری انجام دهد، مسئله دیگری است.

Event-driven Programming

اکنون مسئله روشن‌تر شده است.

Application نمی‌خواهد دائماً بررسی کند:

آیا کاربر کلیک کرد؟
آیا کاربر کلیک کرد؟
آیا کاربر کلیک کرد؟
...

این مدل هم غیرضروری است و هم با ماهیت Browser سازگار نیست.

در عوض، Application به Browser اعلام می‌کند:

وقتی چنین Eventی رخ داد، Function مشخصی را اجرا کن.

بنابراین جریان به این شکل درمی‌آید:

Browser
↓
Wait for Event
↓
Event occurs
↓
Run Application Logic

این مدل را Event-driven Programming می‌نامیم.

در این مدل، وقوع Event آغازکننده اجرای بخشی از Logic برنامه است.

مثلاً:

User clicks Search
↓
click Event
↓
Search Logic

بنابراین Event یک Trigger برای Application Logic است.

اما هنوز یک سؤال مهم باقی می‌ماند:

JavaScript باید بداند این Event مربوط به کدام Element است.

Event Target

فرض کنید صفحه سه Button دارد:

<button>Search</button>
<button>Reset</button>
<button>Save</button>

کاربر روی Search کلیک می‌کند.

Browser باید بتواند مشخص کند Event مربوط به کدام Element است.

Element یا Objectی که Event به آن مربوط شده است، Event Target نام دارد.

در این مثال:

User clicks Search
↓
click Event
↓
Search Button
↓
Event Target

پس:

Event Target موجودیتی است که Event به آن مربوط شده است.

این مفهوم زمانی اهمیت بیشتری پیدا می‌کند که بخواهیم Event را در JavaScript دریافت و تحلیل کنیم.

اما اکنون هنوز یک مشکل داریم.

ما می‌دانیم Event رخ داده و Target آن را نیز می‌شناسیم.

ولی Browser از کجا باید بداند Application می‌خواهد به این Event واکنش نشان دهد؟

Event Listener

Application باید به Browser اعلام کند که به یک Event مشخص علاقه‌مند است.

این کار با Event Listener انجام می‌شود.

مثلاً:

const searchButton = document.querySelector('.search');

searchButton.addEventListener('click', handleSearch);

این کد به Browser می‌گوید:

برای Event نوع click روی این Button یک واکنش ثبت کن.

addEventListener() در اینجا Event را ایجاد نمی‌کند.

بلکه یک Listener ثبت می‌کند تا وقتی Event اتفاق افتاد، Browser بتواند Function مربوط را اجرا کند.

مدل ذهنی:

Element
↓
addEventListener()
↓
Listen for click
↓
Wait
↓
click occurs

این «Wait» به معنای توقف JavaScript نیست.

Browser Listener را ثبت می‌کند و Application می‌تواند به اجرای سایر بخش‌های خود ادامه دهد. هنگامی که Event رخ دهد، Browser Handler مربوط را فراخوانی می‌کند.

اما Functionای که در Listener ثبت می‌کنیم دقیقاً چه نقشی دارد؟

Event Handler

فرض کنید Function زیر را داریم:

function handleSearch() {
console.log('Search started');
}

و آن را به Event متصل می‌کنیم:

searchButton.addEventListener('click', handleSearch);

اینجا handleSearch یک Event Handler است.

Handler، Functionی است که Application برای پاسخ به Event اجرا می‌کند.

پس اکنون جریان کامل‌تر شده است:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler

تفاوت Listener و Handler را می‌توان این‌گونه دید:

Listener
↓
ثبت واکنش به Event

Handler
↓
Logic اجراشونده در پاسخ به Event

این دو مفهوم در گفتگوهای عملی گاهی با یکدیگر ترکیب می‌شوند، اما از نظر مدل ذهنی بهتر است نقش آن‌ها را جدا نگه داریم.

چرا Function را اجرا نمی‌کنیم؟

در اینجا یک نکته مهم درباره Callback Functions وجود دارد.

کد صحیح:

searchButton.addEventListener('click', handleSearch);

اما این کد رفتار متفاوتی دارد:

searchButton.addEventListener('click', handleSearch());

در حالت اول:

handleSearch

یک Reference به Function است.

Browser آن را نگه می‌دارد تا بعداً، هنگام وقوع click، آن را اجرا کند.

در حالت دوم:

handleSearch()

Function همان لحظه اجرا می‌شود.

پس مفهوم اصلی این است:

handleSearch
↓
Pass Function

handleSearch()
↓
Call Function Now

Event Handling یکی از کاربردهای مهم Callback Functionها در Browser است.

اما Handler معمولاً به اطلاعاتی درباره Event نیز نیاز دارد.

مثلاً ممکن است بخواهیم بدانیم:

کاربر دقیقاً روی کدام Element کلیک کرده است؟

برای پاسخ به این سؤال، Browser باید اطلاعات Event را به Handler منتقل کند.

Event Object

وقتی Event رخ می‌دهد، Browser اطلاعات مربوط به آن Event را در اختیار Handler قرار می‌دهد.

این اطلاعات در قالب یک Event Object ارائه می‌شوند.

مثلاً:

function handleSearch(event) {
console.log(event);
}

searchButton.addEventListener('click', handleSearch);

در اینجا event یک Object واقعی است که Browser هنگام اجرای Handler در اختیار Function قرار داده است.

بنابراین:

Event occurs
↓
Browser invokes Handler
↓
Browser passes Event Object
↓
Handler receives event

Event Object بسته به نوع Event می‌تواند اطلاعات متفاوتی داشته باشد.

برای مثال، در یک Keyboard Event اطلاعاتی درباره Key وجود دارد و در یک Mouse Event اطلاعات مربوط به تعامل Pointer در دسترس است.

پس Event Object را می‌توان رابطی دانست که اطلاعات رخداد را از Browser به Application Logic منتقل می‌کند.

پیدا کردن منبع Event با event.target

اکنون به مسئله‌ای که هنگام معرفی Event Target داشتیم برمی‌گردیم.

فرض کنید:

searchButton.addEventListener('click', function (event) {
console.log(event.target);
});

در اینجا:

event.target

به Element مربوط به Event اشاره می‌کند.

پس اگر کاربر روی Search Button کلیک کرده باشد، Target همان Button خواهد بود.

این اطلاعات زمانی اهمیت پیدا می‌کند که یک Handler باید بر اساس Element مربوط به Event تصمیم بگیرد چه کاری انجام دهد.

بنابراین:

Event Object
↓
target
↓
Element related to the Event

اما یک سؤال ظریف‌تر وجود دارد.

اگر Event از یک Element شروع شود و در ساختار DOM حرکت کند، آیا همیشه Element مربوط به Event همان Elementی است که Listener روی آن قرار گرفته است؟

خیر.

برای همین Browser مفهوم دیگری به نام currentTarget دارد.

target در برابر currentTarget

فرض کنید Listener روی یک Element ثبت شده است:

container.addEventListener('click', function (event) {
console.log(event.target);
console.log(event.currentTarget);
});

دو Property دو سؤال متفاوت را پاسخ می‌دهند.

event.target

می‌پرسد:

Event از کدام Element آمده است؟

در حالی که:

event.currentTarget

می‌پرسد:

Listener فعلی روی کدام Element در حال اجرا است؟

در ساده‌ترین حالت، اگر Listener مستقیماً روی همان Elementی باشد که کاربر روی آن کلیک کرده است، این دو می‌توانند یکسان باشند.

اما از نظر مفهومی یکی نیستند.

این تفاوت در فصل بعد، هنگام بررسی Event Propagation و Event Delegation اهمیت بیشتری پیدا می‌کند.

در این فصل فقط باید مرز مفهومی آن‌ها را حفظ کنیم:

target
↓
Element مربوط به Event

currentTarget
↓
Element مربوط به Listener فعلی
Eventهای مختلف، اطلاعات متفاوت

تا اینجا بیشتر از click استفاده کردیم.

اما Browser Eventهای مختلفی را در اختیار Application قرار می‌دهد.

برای مثال:

click
input
change
submit
keydown
keyup
focus
blur

انتخاب Event باید بر اساس Interaction موردنظر انجام شود.

مثلاً اگر می‌خواهیم هنگام تایپ کاربر واکنش نشان دهیم، input با نیاز ما سازگار است.

اگر می‌خواهیم هنگام ارسال Form واکنش نشان دهیم، submit مفهوم مناسب‌تری است.

اگر می‌خواهیم فشرده‌شدن یک Key را تشخیص دهیم، keydown کاربرد دارد.

بنابراین Event Type فقط یک String نیست که به‌صورت تصادفی انتخاب شود.

نوع Event بخشی از مدل Interaction است:

User Interaction
↓
Appropriate Event Type
↓
Handler

این انتخاب باید بر اساس زمان و نوع واکنش موردنیاز Application انجام شود.

از Event به Application Logic

اکنون می‌توانیم یک مثال واقعی‌تر را ببینیم.

فرض کنید در Recipe Application کاربر روی Search کلیک می‌کند.

Handler ممکن است چنین کاری انجام دهد:

function handleSearch(event) {
const query = searchInput.value;

if (!query) return;

searchRecipes(query);
}

searchButton.addEventListener('click', handleSearch);

در اینجا Event فقط آغاز جریان است.

click خودش Search انجام نمی‌دهد.

جریان واقعی:

User clicks
↓
click Event
↓
Listener
↓
handleSearch
↓
Read Input
↓
Search Logic

این تفکیک از نظر مهندسی مهم است.

Event Layer وظیفه تشخیص Interaction را بر عهده دارد.

Application Logic مسئول تصمیم‌گیری درباره کاری است که باید انجام شود.

به همین دلیل بهتر است Event Handler را با تمام Business Logic Application یکی ندانیم.

وقتی Browser خودش هم کاری انجام می‌دهد

تا اینجا فرض کردیم Event فقط باعث اجرای Handler می‌شود.

اما Browser در بعضی Eventها خودش نیز رفتار مشخصی دارد.

برای مثال:

<a href="/recipes">Recipes</a>

وقتی کاربر روی Link کلیک می‌کند، click Event ایجاد می‌شود.

اما Browser علاوه بر Event، یک رفتار پیش‌فرض نیز دارد:

Click
↓
click Event
↓
Navigation

این Navigation، بخشی از Logicی نیست که ما در Handler نوشته‌ایم.

این رفتار را Default Action می‌نامیم.

پس اکنون باید دو چیز را از یکدیگر جدا کنیم:

Event
↓
یک رخداد اتفاق افتاده است

Default Action
↓
Browser در پاسخ به آن رخداد چه رفتار پیش‌فرضی انجام می‌دهد

این تفکیک در Formها اهمیت بیشتری پیدا می‌کند.

مسئله Form Submission

فرض کنید Search Application یک Form دارد:

<form class="search-form">
  <input class="search-input" name="query">
  <button type="submit">Search</button>
</form>

کاربر Form را Submit می‌کند.

Browser یک submit Event ایجاد می‌کند.

اما Submit یک رفتار پیش‌فرض نیز دارد.

در Applicationهای مدرن ممکن است بخواهیم فرآیند Submit را خودمان کنترل کنیم:

Submit
↓
Read Input
↓
Validate
↓
Send Request
↓
Update UI

اگر Browser هم‌زمان Default Action خودش را انجام دهد، ممکن است با جریان موردنظر Application تداخل ایجاد شود.

پس این بار مشکل فقط دریافت Event نیست.

ما باید بتوانیم رفتار پیش‌فرض Browser را نیز کنترل کنیم.

لغو Default Action با preventDefault()

Event Object متدی در اختیار ما قرار می‌دهد:

event.preventDefault();

این متد برای لغو Default Action قابل لغو Event استفاده می‌شود.

مثلاً:

function handleSubmit(event) {
event.preventDefault();

const query = searchInput.value;

if (!query) return;

searchRecipes(query);
}

searchForm.addEventListener('submit', handleSubmit);

اکنون جریان به این شکل است:

User submits form
↓
submit Event
↓
Handler
↓
preventDefault()
↓
Cancel Default Action
↓
Application handles submission

نکته مهم این است که preventDefault() خود Event را حذف نمی‌کند.

Event اتفاق افتاده است.

Handler نیز اجرا شده است.

فقط Default Action مربوط به آن لغو شده است.

preventDefault() چه چیزی را متوقف نمی‌کند؟

یک سوءبرداشت رایج این است که:

preventDefault() Event را متوقف می‌کند.

این تعریف دقیق نیست.

preventDefault() وظیفه مشخصی دارد:

لغو Default Action قابل لغو Event.

این متد مسئول Event Propagation نیست.

بنابراین:

preventDefault()
↓
Default Action

با:

Event Propagation
↓
Capture / Target / Bubble

دو مسئله متفاوت هستند.

Event Propagation در فصل بعد بررسی خواهد شد.

آیا هر Event قابل لغو است؟

از نظر فنی، هر Event الزاماً Default Action قابل لغو ندارد.

برای بررسی این موضوع می‌توان از:

event.cancelable

استفاده کرد.

این Property مشخص می‌کند آیا Event دارای Default Action قابل لغو است یا خیر.

بنابراین استفاده صحیح از preventDefault() بر یک مدل مشخص استوار است:

Event
↓
Cancelable Default Action?
↓
Yes
↓
preventDefault()
↓
Cancel Default Action

در نتیجه، preventDefault() نباید به یک دستور عادت‌وار در تمام Event Handlerها تبدیل شود.

ابتدا باید بدانیم Browser چه رفتار پیش‌فرضی دارد و چرا می‌خواهیم آن را لغو کنیم.

Event Handling در یک جریان واقعی

اکنون تمام مفاهیم فصل را در یک سناریوی Search Form کنار هم قرار دهیم.

HTML:

<form class="search-form">
  <input class="search-input" name="query">
  <button type="submit">Search</button>
</form>

JavaScript:

const searchForm = document.querySelector('.search-form');
const searchInput = document.querySelector('.search-input');

function handleSearch(event) {
event.preventDefault();

const query = searchInput.value;

if (!query) return;

searchRecipes(query);
}

searchForm.addEventListener('submit', handleSearch);

حالا جریان اجرا را مرحله‌به‌مرحله ببینیم.

کاربر یک Action انجام می‌دهد:

User submits form

Browser Event ایجاد می‌کند:

submit Event

Event به Form مربوط است:

Event Target
↓
searchForm

Application قبلاً Listener ثبت کرده است:

searchForm.addEventListener('submit', handleSearch);

بنابراین Browser Handler را اجرا می‌کند:

handleSearch

و Event Object را به آن می‌دهد:

function handleSearch(event)

Handler Default Action را لغو می‌کند:

event.preventDefault();

سپس Application Logic اجرا می‌شود:

Read Input
↓
Validate Query
↓
Search

بنابراین Concept Flow فصل اکنون به یک جریان واقعی تبدیل شده است:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Default Action
↓
preventDefault

این Flow مجموعه‌ای از اصطلاحات مستقل نیست.

هر Concept پاسخی به مسئله Concept قبلی است.

مرز Browser و Application Logic

در اینجا می‌توانیم یک نتیجه مهندسی مهم بگیریم.

Event Handling در مرز میان Browser و Application قرار دارد.

Browser مسئول تشخیص Interaction است:

User
↓
Browser
↓
Event

Application مسئول واکنش به آن Interaction است:

Event
↓
Handler
↓
Application Logic

بنابراین Event Handler را می‌توان Boundary میان این دو بخش در نظر گرفت.

این Boundary کمک می‌کند Application Logic را از جزئیات Interaction جدا نگه داریم.

برای مثال:

function handleSearch(event) {
event.preventDefault();

const query = searchInput.value;

searchRecipes(query);
}

در اینجا Handler Interaction را دریافت می‌کند و سپس Logic Application را فعال می‌کند.

با بزرگ‌تر شدن Application، بهتر است Logic اصلی Search از جزئیات Event جدا شود:

Browser Event
↓
Event Handler
↓
Application Logic

این همان جایی است که Event Handling از یک Syntax ساده به یک موضوع مهندسی تبدیل می‌شود.

چرا addEventListener() بخش مهمی از این مدل است؟

ممکن است به‌جای آن از Propertyهایی مانند:

button.onclick = handleClick;

استفاده کنیم.

این روش می‌تواند کار کند.

اما addEventListener() مدل عمومی‌تر و انعطاف‌پذیرتری برای ثبت Listener فراهم می‌کند.

برای مثال، می‌توان Listenerهای مختلفی برای یک Event ثبت کرد:

button.addEventListener('click', handleSearch);
button.addEventListener('click', trackSearch);

در نتیجه یک Click می‌تواند چند واکنش مستقل داشته باشد.

این موضوع با یک اصل مهندسی مهم سازگار است:

یک Interaction الزاماً فقط یک مسئولیت ندارد.

برای مثال، یک Search ممکن است هم Logic جست‌وجو را اجرا کند و هم یک Event مربوط به Analytics را ثبت کند.

با این حال، هر Handler باید مسئولیت مشخصی داشته باشد.

یک مدل ذهنی واحد برای Event Handling

اکنون دیگر نیازی نیست Event، Listener، Handler و Event Object را به‌صورت اصطلاحات جداگانه حفظ کنیم.

همه آن‌ها بخشی از یک جریان هستند.

کاربر Interaction انجام می‌دهد:

User Action

Browser آن Interaction را به یک رخداد تبدیل می‌کند:

Event

رخداد به یک موجودیت مشخص مربوط است:

Event Target

Application قبلاً اعلام کرده است که به آن Event علاقه‌مند است:

Event Listener

Browser Function مربوط را اجرا می‌کند:

Event Handler

اطلاعات رخداد در اختیار Handler قرار می‌گیرد:

Event Object

Browser ممکن است علاوه بر اجرای Handler، رفتار پیش‌فرضی داشته باشد:

Default Action

و Application در صورت نیاز می‌تواند آن رفتار قابل لغو را کنترل کند:

preventDefault()

بنابراین مدل ذهنی نهایی:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Default Action
↓
preventDefault()

نکته مهم این است که این Flow پایان Event System نیست.

در این فصل یاد گرفتیم Event چگونه به Handler می‌رسد و چگونه می‌توان Default Action را کنترل کرد.

اما هنوز یک سؤال باقی مانده است:

اگر Event در یک Element رخ دهد، چگونه در ساختار DOM حرکت می‌کند و چگونه می‌توان از این رفتار برای مدیریت تعداد زیادی Element استفاده کرد؟

پاسخ این سؤال در Event Propagation و Event Delegation قرار دارد که در فصل بعد بررسی خواهد شد.

Best Practices
1. Handler را از Registration جدا نگه دارید

بهتر است Handler مستقل باشد:

function handleSearch(event) {
// ...
}

searchForm.addEventListener('submit', handleSearch);

این ساختار مسئولیت Event Registration و Application Logic را واضح‌تر می‌کند.

2. Event Type را بر اساس Interaction انتخاب کنید

Event باید با نیاز Application هماهنگ باشد:

click  → Click interaction
input  → Input changes
submit → Form submission
keydown → Keyboard interaction

انتخاب اشتباه Event می‌تواند باعث اجرای Logic در زمان نامناسب شود.

3. Event Object را زمانی دریافت کنید که به آن نیاز دارید

اگر Handler به Event Object نیاز ندارد:

function handleSearch() {
search();
}

کافی است.

اگر نیاز دارید Default Action را لغو کنید:

function handleSearch(event) {
event.preventDefault();
}

Event Object را دریافت کنید.

4. preventDefault() را هدفمند استفاده کنید

قبل از استفاده از آن مشخص کنید:

Default Action چیست؟
آیا قابل لغو است؟
چرا باید لغو شود؟

preventDefault() نباید صرفاً به یک عادت در تمام Handlerها تبدیل شود.

5. Event Handler را با تمام Application Logic یکی نکنید

Handler بهتر است نقطه ورود Interaction باشد:

Event
↓
Handler
↓
Application Logic

با افزایش پیچیدگی Application، جداسازی این لایه‌ها Maintainability را افزایش می‌دهد.

اشتباهات رایج
Event را با Event Handler یکی ندانید

Event یک رخداد است.

Handler Functionی است که در پاسخ به آن رخداد اجرا می‌شود.

Event
↓
Handler
handleClick و handleClick() را اشتباه نگیرید
button.addEventListener('click', handleClick);

Function را برای اجرای آینده ثبت می‌کند.

اما:

button.addEventListener('click', handleClick());

Function را همان لحظه اجرا می‌کند.

target و currentTarget را یکی ندانید
event.target

به Element مربوط به Event اشاره می‌کند.

در حالی که:

event.currentTarget

به Element مربوط به Listener فعلی اشاره می‌کند.

preventDefault() را با توقف Event اشتباه نگیرید

preventDefault() Default Action را لغو می‌کند.

این متد به معنای توقف Event Propagation نیست.

preventDefault() را بدون دلیل استفاده نکنید

ابتدا رفتار پیش‌فرض Browser را شناسایی کنید.

سپس اگر Application باید آن رفتار را کنترل کند، آن را لغو کنید.

Summary

Event Handling پاسخی به یک مسئله بنیادی در Browser است:

Application چگونه بفهمد User چه زمانی با UI تعامل کرده است؟

کاربر یک Action انجام می‌دهد.

Browser آن Action را به یک Event تبدیل می‌کند.

Event به یک Event Target مربوط است.

Application با addEventListener() برای آن Event یک Listener ثبت می‌کند.

هنگام وقوع Event، Browser Event Handler را اجرا می‌کند و Event Object را در اختیار آن قرار می‌دهد.

Handler می‌تواند از اطلاعات Event Object، مانند target، برای تحلیل Interaction استفاده کند.

در بعضی Eventها Browser علاوه بر اجرای Handler دارای یک Default Action نیز هست.

اگر این رفتار قابل لغو باشد، Application می‌تواند با:

event.preventDefault();

آن را لغو کند.

در نتیجه Event Handling را می‌توان به‌صورت یک جریان واحد دید:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Default Action
↓
preventDefault()
Key Takeaways
Event یک رخداد قابل شناسایی در Browser است.
Event-driven Programming اجرای Logic را به وقوع Eventها وابسته می‌کند.
Event Target موجودیتی است که Event به آن مربوط شده است.
addEventListener() برای ثبت Listener استفاده می‌شود.
Event Handler Functionی است که در پاسخ به Event اجرا می‌شود.
Event Handler معمولاً یک Callback است که Browser آن را هنگام وقوع Event فراخوانی می‌کند.
Event Object اطلاعات مربوط به Event را در اختیار Handler قرار می‌دهد.
event.target به Element مربوط به Event اشاره می‌کند.
event.currentTarget به Element مربوط به Listener فعلی اشاره می‌کند.
target و currentTarget همیشه یک مفهوم نیستند.
Event Type باید بر اساس نوع Interaction انتخاب شود.
بعضی Eventها Default Action دارند.
preventDefault() برای لغو Default Action قابل لغو استفاده می‌شود.
preventDefault() Event Propagation را متوقف نمی‌کند.
Event Handling مرز مهمی میان Browser Interaction و Application Logic ایجاد می‌کند.
Event Propagation و Event Delegation موضوع فصل بعد هستند.
Technical Interview
Junior
1. Event چیست؟

Event یک رخداد قابل شناسایی در Browser است که Application می‌تواند در پاسخ به آن Logic اجرا کند؛ مانند click، input و submit.

2. Event Listener چیست؟

Event Listener مکانیزمی برای ثبت واکنش به یک Event مشخص روی یک EventTarget است.

3. Event Handler چیست؟

Event Handler Functionی است که هنگام وقوع Event اجرا می‌شود و معمولاً Application Logic مربوط به آن Interaction را آغاز می‌کند.

4. addEventListener() چه کاری انجام می‌دهد؟

یک Listener برای Event مشخص روی یک EventTarget ثبت می‌کند.

5. Event Object چیست؟

Objectی است که Browser هنگام اجرای Handler در اختیار آن قرار می‌دهد و اطلاعات مربوط به Event را فراهم می‌کند.

6. event.target چیست؟

Element یا Objectی است که Event به آن مربوط شده است.

Mid-Level
7. تفاوت Event و Event Handler چیست؟

Event یک رخداد است؛ Event Handler Functionی است که در پاسخ به آن رخداد اجرا می‌شود.

8. تفاوت target و currentTarget چیست؟

target به Element مربوط به Event اشاره می‌کند، در حالی که currentTarget به Element مربوط به Listener فعلی اشاره دارد.

9. چرا handleClick با handleClick() متفاوت است؟

handleClick Reference مربوط به Function را منتقل می‌کند تا بعداً توسط Browser اجرا شود؛ handleClick() Function را همان لحظه اجرا می‌کند.

10. preventDefault() چه کاری انجام می‌دهد؟

Default Action قابل لغو Event را لغو می‌کند.

11. آیا preventDefault() Event را متوقف می‌کند؟

خیر. این متد Default Action را لغو می‌کند و با Event Propagation تفاوت دارد.

12. چرا addEventListener() برای Event Handling مناسب است؟

زیرا Event Registration را از Logic جدا می‌کند و امکان ثبت Listenerهای مستقل برای یک Event را فراهم می‌سازد.

Senior
13. Event Handling چه نقشی در معماری Frontend دارد؟

Event Handling مرز میان User Interaction در Browser و Application Logic است:

Browser Event
↓
Event Handler
↓
Application Logic

Handler می‌تواند Interaction را دریافت کرده و Logic مناسب Application را فعال کند.

14. چرا Event Handler نباید محل تجمع تمام Application Logic باشد؟

زیرا Handler مستقیماً به UI Event وابسته است. اگر تمام Validation، Data Processing، State Management و Rendering در آن قرار گیرد، Coupling افزایش می‌یابد و نگهداری و Test کردن Logic دشوارتر می‌شود.

15. چرا preventDefault() با Event Propagation متفاوت است؟

preventDefault() رفتار پیش‌فرض قابل لغو Browser را کنترل می‌کند؛ Event Propagation مربوط به حرکت Event در ساختار DOM است. این دو مکانیزم مسئولیت‌های متفاوتی دارند.

16. چرا target و currentTarget از نظر مهندسی مهم هستند؟

زیرا به دو سؤال متفاوت پاسخ می‌دهند:

target
↓
Event مربوط به کدام Element است؟

currentTarget
↓
Listener فعلی روی کدام Element اجرا می‌شود؟

این تفاوت در Event Propagation و Event Delegation اهمیت بیشتری پیدا می‌کند.

17. چرا Event Handling را بخشی از Browser Platform می‌دانیم، نه صرفاً Syntax زبان JavaScript؟

زیرا click، submit و سایر DOM Events بخشی از Web Platform هستند. Browser Interaction را تشخیص می‌دهد و از طریق Event System اطلاعات آن را به JavaScript منتقل می‌کند:

User
↓
Browser
↓
Web Platform
↓
Event System
↓
JavaScript
Golden Answers
Junior — Golden Answer

Event چیست؟

Event یک رخداد قابل شناسایی در Browser است که Application می‌تواند در پاسخ به آن Logic اجرا کند؛ مانند click، input یا submit.

Mid-Level — Golden Answer

Event Handling چگونه کار می‌کند؟

Browser یک User Interaction را به Event تبدیل می‌کند. Event به یک Target مربوط می‌شود. اگر برای آن Event Listener ثبت شده باشد، Browser Handler را اجرا می‌کند و Event Object را در اختیار آن قرار می‌دهد. اگر Event دارای Default Action قابل لغو باشد، Handler می‌تواند با preventDefault() آن رفتار را لغو کند.

User Action
↓
Event
↓
Target
↓
Listener
↓
Handler
↓
Event Object
↓
Default Action
↓
preventDefault()
Senior — Golden Answer

preventDefault() چه تفاوتی با توقف Event دارد؟

preventDefault() برای لغو Default Action یک Event قابل لغو استفاده می‌شود. این متد خود Event یا مسیر حرکت آن در DOM را متوقف نمی‌کند. Event Propagation مکانیزم جداگانه‌ای است که در آن Event میان بخش‌های مختلف DOM حرکت می‌کند.

Conclusion

در Browser، User Interaction نقطه آغاز بسیاری از عملیات Application است.

اما JavaScript نمی‌تواند زمان این Interaction را از قبل بداند.

به همین دلیل Browser یک Event System فراهم می‌کند.

User Action به Event تبدیل می‌شود.

Event به یک Target مربوط می‌شود.

Application با addEventListener() برای آن Event Listener ثبت می‌کند.

وقتی Event رخ می‌دهد، Browser Handler را اجرا می‌کند و Event Object را در اختیار آن قرار می‌دهد.

Handler سپس می‌تواند Application Logic را اجرا کند.

اگر Browser برای Event رفتار پیش‌فرضی داشته باشد، Application در صورت نیاز می‌تواند آن رفتار قابل لغو را با preventDefault() کنترل کند.

بنابراین Event Handling را نباید مجموعه‌ای از Methodها و Propertyهای جداگانه حفظ کرد.

مدل صحیح این است:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Application Logic
↓
Default Action
↓
preventDefault()

در این فصل یاد گرفتیم Event چگونه از User Interaction به Application Logic می‌رسد.

اما هنوز یک بخش مهم از Event System باقی مانده است.

وقتی Event در یک Element رخ می‌دهد، این Event فقط یک نقطه ثابت ندارد؛ بلکه می‌تواند در ساختار DOM حرکت کند.

اکنون سؤال طبیعی این است:

Event چگونه در DOM حرکت می‌کند و چگونه می‌توان از این رفتار برای مدیریت چندین Element با یک Listener استفاده کرد؟

این سؤال نقطه ورود به Event Propagation و Event Delegation در Chapter 55 است.