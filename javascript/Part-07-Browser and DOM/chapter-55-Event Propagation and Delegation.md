Chapter 55 — Event Propagation and Delegation
Chapter Goal

خواننده بتواند مسیر حرکت Event در DOM را تحلیل کند، تفاوت Capture، Target و Bubble را درک کند و از همین رفتار برای طراحی Event Delegation در UIهای واقعی استفاده کند.

Core Question

Event چگونه در DOM حرکت می‌کند و چگونه می‌توان از این رفتار برای Event Delegation استفاده کرد؟

مقدمه

در فصل قبل دیدیم که Browser یک User Action مانند Click را به یک Event تبدیل می‌کند و JavaScript می‌تواند با addEventListener() به آن واکنش نشان دهد.

اما یک سؤال مهم هنوز باقی مانده است.

فرض کنید ساختار صفحه چنین باشد:

document
↓
body
↓
section
↓
button

کاربر روی button کلیک می‌کند.

واضح است که Button می‌تواند Event را دریافت کند. اما اگر روی section نیز یک Event Listener وجود داشته باشد چه اتفاقی می‌افتد؟

آیا Event فقط متعلق به Button است؟

اگر Event بتواند به section نیز برسد، این حرکت چگونه اتفاق می‌افتد؟

و اگر یک List شامل صدها Item داشته باشیم، آیا واقعاً لازم است برای تک‌تک Itemها Event Listener جداگانه ایجاد کنیم؟

برای پاسخ به این سؤال‌ها باید ابتدا رابطه میان DOM Tree و Event را درک کنیم.

از Event به Event Propagation

DOM یک مجموعه ساده از Elementهای مستقل نیست.

Elementها در یک ساختار درختی با یکدیگر ارتباط دارند:

document
↓
body
↓
section
↓
button

Button داخل Section قرار دارد و Section نیز داخل Body.

بنابراین وقتی Event روی Button رخ می‌دهد، Browser می‌تواند آن Event را در ارتباط با مسیر DOM پردازش کند.

این رفتار را Event Propagation می‌نامیم.

Event Propagation یعنی Event می‌تواند در مسیر DOM حرکت کند و در نقاط مختلف این مسیر به Event Listenerها برسد.

اما این حرکت یک جهت ساده ندارد.

برای فهم آن باید ابتدا مسیر Event را بررسی کنیم.

Event از کجا حرکت می‌کند؟

فرض کنید:

document
↓
body
↓
section
↓
button

و کاربر روی Button کلیک می‌کند.

Event از بالای مسیر به سمت Element هدف حرکت می‌کند:

document
↓
body
↓
section
↓
button

اما پس از رسیدن به Button، جریان می‌تواند در جهت مخالف ادامه پیدا کند:

button
↓
section
↓
body
↓
document

پس Event دو بخش مهم از مسیر را طی می‌کند:

بالا → پایین
پایین → بالا

این مشاهده، ما را به سه مرحله اصلی Event Propagation می‌رساند:

Capture
↓
Target
↓
Bubble
Capture Phase

در مرحله اول، Event از Ancestorهای بالاتر به سمت Element هدف حرکت می‌کند.

این مرحله Capture Phase نام دارد.

برای مثال:

document
↓
body
↓
section
↓
button

در این مرحله Event هنوز به Button نرسیده است.

Browser در حال عبور از مسیر Ancestorها به سمت Target است.

به همین دلیل اگر بخواهیم Listener را در Capture Phase اجرا کنیم، می‌توانیم هنگام ثبت Listener گزینه capture را فعال کنیم:

section.addEventListener('click', handler, {
capture: true
});

در مقابل، این کد:

section.addEventListener('click', handler);

به‌صورت پیش‌فرض Listener را برای Bubble Phase ثبت می‌کند.

پس capture یک مفهوم مربوط به زمان و مرحله اجرای Listener است، نه نوع جدیدی از Event.

Target Phase

Event پس از عبور از مسیر Capture به Elementی می‌رسد که Event روی آن ایجاد شده است.

این Element، Target است.

اگر کاربر روی Button کلیک کرده باشد:

document
↓
body
↓
section
↓
button

در اینجا:

button

Target Event است.

اکنون Event به نقطه‌ای رسیده که از ابتدا برای آن ایجاد شده بود.

اما هنوز یک سؤال باقی می‌ماند:

آیا Event در همین‌جا متوقف می‌شود؟

خیر.

اگر Event قابلیت Bubbling داشته باشد، اکنون حرکت آن در جهت مخالف ادامه پیدا می‌کند.

Bubble Phase

پس از رسیدن Event به Target، Event می‌تواند از Target به سمت Ancestorهای آن حرکت کند:

button
↓
section
↓
body
↓
document

این مرحله Bubble Phase نام دارد.

اینجا یک رفتار مهم ظاهر می‌شود.

اگر روی Button یک Listener داشته باشیم و روی Section نیز Listener دیگری وجود داشته باشد، کلیک روی Button می‌تواند باعث اجرای هر دو Handler شود.

چرا؟

چون Event ابتدا به Button می‌رسد و سپس در مسیر Bubble به Section منتقل می‌شود.

بنابراین:

Click on Button
↓
Target: Button
↓
Bubble
↓
Section Listener

این رفتار در نگاه اول ممکن است غیرمنتظره باشد، اما دقیقاً همان رفتاری است که بعداً امکان Event Delegation را فراهم می‌کند.

یک مثال ساده از Bubbling

فرض کنید:

<div class="card">
  <button>Delete</button>
</div>

برای هر دو Element Listener قرار می‌دهیم:

const card = document.querySelector('.card');
const button = document.querySelector('button');

card.addEventListener('click', () => {
console.log('card');
});

button.addEventListener('click', () => {
console.log('button');
});

اگر روی Button کلیک کنیم، ابتدا Handler مربوط به Button اجرا می‌شود.

سپس Event در مسیر Bubble به Card می‌رسد و Handler مربوط به Card نیز اجرا می‌شود.

در نتیجه مفهوم اصلی این است:

Button
↓
Card

نه اینکه کاربر واقعاً دو بار کلیک کرده باشد.

فقط یک Event وجود دارد که در مسیر DOM حرکت کرده است.

یک Event، چند Handler

اکنون می‌توانیم مسئله را دقیق‌تر ببینیم.

Event یک Object مستقل است، اما در طول Propagation ممکن است توسط Listenerهای مختلف مشاهده و پردازش شود.

مثلاً:

document
↓
body
↓
card
↓
button

اگر روی همه این Elementها Listener داشته باشیم، یک Click می‌تواند در مسیر خود چند Handler را فعال کند.

این موضوع یک سؤال مهم ایجاد می‌کند:

اگر Handler مربوط به Card در حال اجراست، چگونه بفهمیم Event در ابتدا روی چه Elementی رخ داده است؟

برای پاسخ به این سؤال باید دو Property مهم Event Object را از یکدیگر جدا کنیم:

event.target

و:

event.currentTarget
event.target

event.target به Elementی اشاره می‌کند که Event روی آن شروع شده است.

در مثال قبلی، اگر کاربر روی Button کلیک کند:

event.target

به Button اشاره می‌کند.

حتی زمانی که Event به Card رسیده و Handler مربوط به Card در حال اجرا است، Target همچنان Button است.

یعنی:

Button Click
↓
target = Button
↓
Bubble to Card
↓
target = still Button

Target در طول Bubble شدن Event تغییر نمی‌کند.

event.currentTarget

اما currentTarget سؤال دیگری را پاسخ می‌دهد:

اکنون Handler مربوط به کدام Element در حال اجراست؟

اگر Listener روی Card باشد:

card.addEventListener('click', event => {
console.log(event.currentTarget);
});

در زمان اجرای Handler:

event.currentTarget

به Card اشاره می‌کند.

پس اگر روی Button کلیک کرده باشیم:

event.target
→ button

event.currentTarget
→ card

این تفاوت بسیار مهم است.

می‌توان آن را با دو سؤال به خاطر سپرد:

target
→ Event از کجا شروع شد؟

currentTarget
→ Handler فعلی متعلق به کدام Element است؟

این دو Property در Event Delegation نقش اساسی خواهند داشت.

چرا به کنترل Propagation نیاز داریم؟

تا اینجا دیدیم که Bubbling باعث می‌شود Event از Child به Parent منتقل شود.

اما همیشه نمی‌خواهیم این اتفاق ادامه پیدا کند.

فرض کنید یک Card Listener دارد:

Card
↓
Button

Button نیز رفتار خاص خودش را دارد.

اگر کاربر روی Button کلیک کند، ممکن است بخواهیم فقط رفتار Button اجرا شود و Event به Card نرسد.

پس به راهی برای متوقف کردن حرکت Event نیاز داریم.

اینجاست که:

event.stopPropagation();

وارد می‌شود.

stopPropagation()

stopPropagation() ادامه Propagation Event را متوقف می‌کند.

مثلاً:

button.addEventListener('click', event => {
event.stopPropagation();

console.log('button');
});

اگر Event در مسیر Bubble باشد، پس از رسیدن به این نقطه دیگر به Ancestorهای بعدی منتقل نمی‌شود.

مدل ذهنی:

button
X
card
X
container

در نتیجه Listenerهای Parent که قرار بود Event را در ادامه مسیر دریافت کنند، دیگر Event را دریافت نمی‌کنند.

اما باید یک تفاوت مهم را در ذهن نگه داریم.

Propagation با Default Action متفاوت است

فرض کنید کاربر روی یک Link کلیک می‌کند:

<a href="/profile">Profile</a>

اگر بنویسیم:

event.stopPropagation();

این دستور به Browser نمی‌گوید که Navigation را متوقف کند.

چون دو مسئله متفاوت داریم:

Event Propagation

و:

Browser Default Action

برای کنترل اول:

event.stopPropagation();

برای کنترل دوم:

event.preventDefault();

استفاده می‌کنیم.

پس:

stopPropagation()
→ Event Flow

preventDefault()
→ Default Browser Behavior

این دو Method را نباید به‌جای یکدیگر استفاده کنیم.

از Bubbling به Event Delegation

اکنون به مسئله‌ای برمی‌گردیم که در ابتدای فصل مطرح کردیم.

فرض کنید یک List داریم:

Product List
├── Product
├── Product
├── Product
├── Product
└── Product

همه Productها باید هنگام Click یک رفتار مشترک داشته باشند.

راه ساده این است که برای هر Product یک Listener ایجاد کنیم:

Product 1 → Listener
Product 2 → Listener
Product 3 → Listener
Product 4 → Listener
Product 5 → Listener

اما این طراحی یک سؤال ایجاد می‌کند:

اگر همه Eventها در نهایت به Parent می‌رسند، چرا Listener را روی خود Parent قرار ندهیم؟

این دقیقاً نقطه‌ای است که Event Delegation از دل Event Bubbling به وجود می‌آید.

Event Delegation

Event Delegation یک Pattern برای مدیریت Event است که در آن به‌جای ثبت Listener روی تعداد زیادی Child Element، Listener را روی یک Parent مشترک قرار می‌دهیم.

ساختار به این شکل تبدیل می‌شود:

Product 1 ──┐
Product 2 ──┤
Product 3 ──┼──→ Product List Listener
Product 4 ──┤
Product 5 ──┘

اکنون تمام Productها Event خود را به Parent منتقل می‌کنند.

Parent یک Listener دارد و از event.target برای تشخیص Element مربوط به Event استفاده می‌کند.

بنابراین Delegation از سه مفهوم قبلی استفاده می‌کند:

Event Bubbling
↓
Parent Listener
↓
event.target
↓
Handle Child Event

این نکته مهم است:

Event Delegation یک رفتار جدید Browser نیست؛ یک Pattern طراحی است که از رفتار موجود Event Propagation استفاده می‌کند.

اولین Delegation واقعی

فرض کنید:

<ul class="products">
  <li>Product A</li>
  <li>Product B</li>
  <li>Product C</li>
</ul>

به‌جای ثبت Listener روی هر li، Listener را روی ul قرار می‌دهیم:

const products = document.querySelector('.products');

products.addEventListener('click', event => {
console.log(event.target);
});

اگر کاربر روی Product B کلیک کند، Event ابتدا روی li رخ می‌دهد و سپس Bubble می‌شود.

در نتیجه Listener مربوط به ul اجرا می‌شود.

در این Handler:

event.target

همچنان همان li مربوط به Product B است.

در حالی که:

event.currentTarget

به ul اشاره می‌کند.

پس:

target
→ Product B

currentTarget
→ Product List

این دقیقاً همان اطلاعاتی است که Delegation به آن نیاز دارد.

چرا Dynamic Elements مسئله مهمی هستند؟

تا اینجا کاهش تعداد Listenerها یک مزیت بود.

اما Delegation یک مزیت مهم‌تر نیز دارد.

فرض کنید List در ابتدا سه Item دارد:

Product A
Product B
Product C

و Listener روی خود Itemها ثبت شده است.

بعداً Application یک Item جدید اضافه می‌کند:

Product D

اکنون باید برای Product D نیز Listener ایجاد کنیم.

اما اگر Listener روی Parent قرار داشته باشد:

Product A ──┐
Product B ──┤
Product C ──┼──→ Parent Listener
Product D ──┤
Product E ──┘

Itemهای جدید نیز می‌توانند Event خود را به همان Parent منتقل کنند.

پس Delegation مخصوصاً زمانی ارزشمند می‌شود که UI دارای Dynamic Content باشد.

این نتیجه مستقیم همان زنجیره‌ای است که از ابتدای فصل دنبال کردیم:

DOM Tree
↓
Event Propagation
↓
Capture
↓
Target
↓
Bubble
↓
target / currentTarget
↓
Event Delegation
↓
Dynamic Elements
Delegation و Elementهای تو‌در‌تو

اکنون یک مشکل واقعی‌تر ایجاد می‌شود.

فرض کنید Button ما چنین ساختاری دارد:

<button class="delete">
  <span>Delete</span>
</button>

و Listener روی Parent قرار گرفته است.

کاربر ممکن است روی خود Button کلیک کند یا روی span.

اگر روی span کلیک کند:

event.target

به span اشاره خواهد کرد.

اما منطق Application ممکن است به Button نیاز داشته باشد.

بنابراین در Delegation نباید تصور کنیم:

event.target همیشه همان Elementی است که منطق Application باید روی آن اجرا شود.

target فقط می‌گوید Event از کجا شروع شده است.

این تفاوت مهم است.

Target واقعی یا Element منطقی؟

فرض کنید UI چنین ساختاری دارد:

button.delete
↓
span

اگر Click روی span رخ دهد:

event.target
→ span

اما Elementی که رفتار Delete را تعریف می‌کند:

button.delete

است.

پس Delegation در UIهای تو‌در‌تو معمولاً به یک مرحله دیگر نیاز پیدا می‌کند:

Event Target
↓
Find Relevant Element
↓
Handle Action

ابزارهایی مانند closest() و matches() برای چنین موقعیت‌هایی بسیار مفید هستند.

اما استفاده کامل از آن‌ها متعلق به فصل بعد، یعنی DOM Traversing است. بنابراین در این فصل فقط باید مسئله را بشناسیم، نه اینکه آن را به‌طور کامل وارد بحث کنیم.

این مرزبندی مهم است؛ زیرا هدف Chapter 55 فهم Event Flow و Delegation است، در حالی که Chapter 56 به حرکت میان Parent، Child، Sibling و ابزارهایی مانند closest() و matches() اختصاص دارد.

Delegation در Listها

List یکی از طبیعی‌ترین کاربردهای Event Delegation است.

فرض کنید Todo List داریم:

Todo List
├── Learn JavaScript
├── Build Project
├── Review Code
└── Push to Git

همه Itemها ممکن است رفتار مشترکی داشته باشند.

به‌جای اینکه برای هر Item Listener جداگانه داشته باشیم، Container می‌تواند Listener را مدیریت کند.

مدل ذهنی:

Todo Item
↓
Bubble
↓
Todo List
↓
Delegated Handler

این طراحی زمانی ارزش بیشتری پیدا می‌کند که Itemها به‌صورت Dynamic ایجاد یا حذف شوند.

Delegation در Table

Table نیز ساختار مناسبی برای Delegation دارد.

فرض کنید:

Table
↓
Row
↓
Delete Button

اگر Table شامل صدها Row باشد، ایجاد Listener برای تک‌تک Buttonها همیشه ضروری نیست.

می‌توان Listener را روی Table قرار داد و Event را از طریق target تحلیل کرد.

در اینجا همان مدل قبلی دوباره ظاهر می‌شود:

Button
↓
Row
↓
Table
↓
Delegated Handler

این مثال نشان می‌دهد که Delegation وابسته به یک نوع UI خاص نیست.

هر جا چند Child یک رفتار مشترک داشته باشند و Event بتواند به Parent برسد، Delegation می‌تواند یک گزینه طراحی باشد.

Delegation و تعداد Listenerها

فرض کنید یک UI شامل 1000 Item است.

روش مستقیم:

1000 Items
↓
1000 Event Listeners

روش Delegation:

1000 Items
↓
1 Parent Listener

این کاهش می‌تواند مدیریت Event را ساده‌تر کند.

اما نباید از این نتیجه بگیریم:

هرچه تعداد Listener کمتر باشد، طراحی همیشه بهتر است.

Event Delegation یک ابزار است، نه یک قانون مطلق.

اگر فقط یک Button مستقل داریم، این کد:

button.addEventListener('click', handler);

کاملاً طبیعی است.

استفاده از Parent فقط برای اینکه Listener کمتری داشته باشیم، ممکن است پیچیدگی غیرضروری ایجاد کند.

بنابراین سؤال درست این نیست:

چگونه تعداد Listenerها را به صفر برسانیم؟

بلکه:

کدام Element مناسب‌ترین نقطه برای مدیریت این رفتار است؟

Bubbling و Delegation یکی نیستند

اکنون می‌توانیم یک تمایز مهم را دقیق بیان کنیم.

Bubbling بخشی از رفتار Event System در DOM است.

Child
↓
Parent
↓
Ancestor

اما Delegation یک Pattern طراحی است.

Parent Listener
+
Bubbling
+
Target Identification

بنابراین:

Bubbling
→ Browser Behavior

Delegation
→ Developer Pattern

Bubbling می‌تواند بدون Delegation وجود داشته باشد.

اما Delegation معمولاً از Bubbling استفاده می‌کند.

Capturing و Delegation

در این فصل تمرکز اصلی Delegation بر Bubble Phase است، زیرا Event Listenerها به‌صورت پیش‌فرض در این Phase کار می‌کنند.

اما از نظر مدل Event Propagation، Capture Phase نیز وجود دارد و می‌توان Listener را به‌صورت Capture ثبت کرد:

container.addEventListener('click', handler, {
capture: true
});

بنابراین باید میان این دو مفهوم تفاوت بگذاریم:

Event Propagation
→ Capture + Target + Bubble

در مقابل:

Common Event Delegation
→ معمولاً بر پایه Bubble

این تفکیک باعث می‌شود Delegation را با Capture یا Bubble اشتباه نگیریم.

stopPropagation() و Delegation

اکنون یک پیامد مهم از بحث Propagation را می‌توانیم ببینیم.

فرض کنید Delegation روی Parent قرار دارد:

Button
↓
Card
↓
Container

و Container Listener دارد.

اگر Button در Handler خود بنویسد:

event.stopPropagation();

Event دیگر به Container نمی‌رسد.

در نتیجه Delegated Handler اجرا نمی‌شود.

پس اگر Delegation درست به نظر می‌رسد اما Handler اجرا نمی‌شود، یکی از مواردی که باید بررسی کنیم این است که آیا Propagation در مسیر متوقف شده است یا خیر.

این موضوع نشان می‌دهد که Delegation بدون درک Event Propagation قابل تحلیل دقیق نیست.

آیا همیشه باید از stopPropagation() استفاده کنیم؟

خیر.

در واقع استفاده بی‌دلیل از آن می‌تواند طراحی Event را پیچیده کند.

فرض کنید:

Child
↓
Parent
↓
Application Container

ممکن است هر سه سطح Listener داشته باشند.

اگر Child بدون دلیل Propagation را متوقف کند:

event.stopPropagation();

Parent دیگر Event را دریافت نمی‌کند.

بنابراین ممکن است یک بخش از Application به دلیل تصمیم یک Child، Event موردنیاز خود را از دست بدهد.

قاعده بهتر این است:

Propagation را فقط زمانی متوقف کنید که واقعاً می‌خواهید Event به Ancestorهای بعدی نرسد.

Common Mistakes
1. اشتباه گرفتن target و currentTarget

اشتباه:

currentTarget همان Elementی است که کاربر روی آن کلیک کرده است.

درست:

target
→ Element شروع‌کننده Event

currentTarget
→ Element صاحب Handler فعلی
2. تصور اینکه stopPropagation() رفتار Browser را لغو می‌کند

stopPropagation() فقط جریان Event را کنترل می‌کند.

برای Default Action باید از:

event.preventDefault();

استفاده کرد.

3. ثبت Listener برای همه Childها بدون بررسی Delegation

وقتی تعداد زیادی Child رفتار مشترک دارند، بهتر است بررسی کنیم آیا Parent مشترک می‌تواند Eventها را مدیریت کند یا خیر.

4. فرض اینکه event.target همیشه Element موردنظر Application است

در UIهای تو‌در‌تو ممکن است Target یک span یا Element داخلی دیگر باشد.

پس:

Event Target
≠
Always Logical UI Element
5. استفاده افراطی از Delegation

Delegation برای یک Button مستقل ضرورتی ندارد.

هدف، انتخاب نقطه مناسب برای Event Handling است، نه صرفاً کاهش تعداد Listenerها.

6. متوقف کردن Propagation بدون دلیل

stopPropagation() می‌تواند Event را از Parentهایی که به آن نیاز دارند پنهان کند.

Best Practices
Event Flow را قبل از کدنویسی تحلیل کنید

ابتدا مشخص کنید:

Event کجا رخ می‌دهد؟
↓
Target چیست؟
↓
Event چه مسیری را طی می‌کند؟
↓
کدام Handler باید اجرا شود؟
target و currentTarget را با سؤال درست انتخاب کنید

اگر می‌خواهید بدانید Event از کجا آمده است:

event.target

اگر می‌خواهید بدانید Handler فعلی متعلق به کدام Element است:

event.currentTarget
برای Childهای متعدد، Delegation را بررسی کنید

اگر چند Child رفتار مشترکی دارند و Parent مشترکی وجود دارد، Delegation می‌تواند طراحی مناسب‌تری باشد.

Dynamic Content را در نظر بگیرید

اگر Childها بعداً ایجاد می‌شوند، Listener روی Parent می‌تواند بدون نیاز به ثبت Listener جدید برای هر Child، Eventهای آن‌ها را مدیریت کند.

Propagation را بدون دلیل متوقف نکنید

stopPropagation() باید یک تصمیم آگاهانه درباره Event Flow باشد.

Event Handling را بر اساس ساختار UI طراحی کنید

Listener را صرفاً بر اساس تعداد Elementها انتخاب نکنید.

ساختار DOM، نوع Event، رفتار Childها و Dynamic بودن UI همگی در تصمیم نقش دارند.

Summary

Event در DOM در ارتباط با ساختار درختی آن Propagate می‌شود.

مدل اصلی این حرکت:

Capture
↓
Target
↓
Bubble

در Capture، Event از Ancestorها به سمت Target حرکت می‌کند.

در Target، Event به Elementی می‌رسد که Event روی آن رخ داده است.

در Bubble، Event می‌تواند از Target به سمت Ancestorها حرکت کند.

در این مسیر دو Property بسیار مهم داریم:

event.target

که Event origin را مشخص می‌کند و:

event.currentTarget

که Element صاحب Handler فعلی را مشخص می‌کند.

اگر لازم باشد ادامه حرکت Event متوقف شود:

event.stopPropagation();

به‌کار می‌رود.

اما برای لغو رفتار پیش‌فرض Browser باید از:

event.preventDefault();

استفاده کرد.

در نهایت، Bubbling یک امکان مهم در اختیار Developer قرار می‌دهد.

به‌جای اینکه برای هر Child Listener جداگانه ایجاد کنیم، می‌توانیم Listener را روی Parent قرار دهیم و Eventهای Child را از طریق event.target مدیریت کنیم.

این Pattern:

Event Delegation

نام دارد.

بنابراین کل مدل ذهنی فصل را می‌توان در یک زنجیره خلاصه کرد:

DOM Tree
↓
Event Propagation
↓
Capture
↓
Target
↓
Bubble
↓
target / currentTarget
↓
Event Delegation
↓
Dynamic Elements
Key Takeaways
DOM یک Tree است و Eventها در ارتباط با این Tree Propagate می‌شوند.

Event Propagation سه مرحله اصلی دارد:

Capture → Target → Bubble
Capture حرکت Event از Ancestor به سمت Target است.
Bubble حرکت Event از Target به سمت Ancestor است.
event.target Element شروع‌کننده Event را مشخص می‌کند.
event.currentTarget Element صاحب Handler در حال اجرا را مشخص می‌کند.
stopPropagation() ادامه Propagation را متوقف می‌کند.
preventDefault() رفتار پیش‌فرض Browser را لغو می‌کند.
Bubbling و Delegation یک مفهوم نیستند.
Bubbling یک رفتار Event System است.
Delegation یک Pattern برای استفاده از این رفتار است.
Delegation برای Listها، Tableها و Dynamic Content کاربرد زیادی دارد.
event.target ممکن است یک Nested Element باشد.
کاهش تعداد Listenerها تنها دلیل استفاده از Delegation نیست.
stopPropagation() باید آگاهانه استفاده شود.
Technical Interview
Junior
Event Propagation چیست؟

پاسخ طلایی:

Event Propagation فرآیندی است که طی آن Event در مسیر DOM حرکت می‌کند و شامل Capture، Target و در Eventهای قابل Bubble، Bubble Phase است.

Event Bubbling چیست؟

پاسخ طلایی:

Bubbling مرحله‌ای از Event Propagation است که در آن Event از Target به سمت Ancestorهای DOM حرکت می‌کند.

تفاوت event.target و event.currentTarget چیست؟

پاسخ طلایی:

event.target Elementی است که Event روی آن شروع شده است، در حالی که event.currentTarget Elementی است که Handler فعلی به آن تعلق دارد.

stopPropagation() چه کاری انجام می‌دهد؟

پاسخ طلایی:

ادامه Propagation Event را در مسیر DOM متوقف می‌کند، اما رفتار پیش‌فرض Browser را لغو نمی‌کند.

Mid-Level
Event Delegation چیست؟

پاسخ طلایی:

Event Delegation الگویی است که در آن Listener روی یک Parent قرار می‌گیرد و Eventهای Childهای متعدد از طریق Propagation به Parent می‌رسند. سپس با استفاده از اطلاعات Event، Child مربوط به Event شناسایی می‌شود.

چرا Bubbling برای Delegation مهم است؟

پاسخ طلایی:

چون Event می‌تواند از Child به Parent Bubble شود و بنابراین Parent می‌تواند Eventهای چند Child را با یک Listener مدیریت کند.

چرا Delegation برای Dynamic Elements مناسب است؟

پاسخ طلایی:

چون Listener روی Parent قرار دارد و به Childهای موجود در زمان ثبت Listener وابسته نیست. بنابراین Childهای جدید نیز می‌توانند Event خود را به Parent منتقل کنند.

آیا stopPropagation() و preventDefault() یک کار انجام می‌دهند؟

پاسخ طلایی:

خیر. stopPropagation() جریان Event در DOM را کنترل می‌کند، در حالی که preventDefault() رفتار پیش‌فرض Browser را لغو می‌کند.

آیا Event Delegation همیشه بهتر از Listener مستقیم است؟

پاسخ طلایی:

خیر. Delegation زمانی مفیدتر است که Childهای متعدد رفتار مشترک داشته باشند، Dynamic باشند یا Parent مناسبی برای مدیریت Event وجود داشته باشد. برای یک Element مستقل، Listener مستقیم می‌تواند ساده‌تر باشد.

Senior
Event Delegation از نظر طراحی چه مسئله‌ای را حل می‌کند؟

پاسخ طلایی:

Delegation مدیریت Eventهای Childهای متعدد را در یک نقطه مشترک متمرکز می‌کند. این کار می‌تواند تعداد Listenerهای تکراری را کاهش دهد و برای UIهای Dynamic، مدیریت Event را ساده‌تر کند.

چرا استفاده مستقیم از event.target در Delegation می‌تواند مشکل‌ساز باشد؟

پاسخ طلایی:

چون event.target دقیقاً Nodeای است که Event روی آن شروع شده و ممکن است یک Nested Element باشد، نه Element منطقی موردنظر Application. بنابراین در UIهای تو‌در‌تو باید Target نسبت به ساختار DOM تحلیل شود.

اگر Delegated Handler اجرا نشود، چه چیزهایی را بررسی می‌کنید؟

پاسخ طلایی:

ابتدا بررسی می‌کنم Event روی Child رخ داده و Event موردنظر قابلیت Bubbling دارد. سپس Listener و Parent را بررسی می‌کنم، مقدار target و currentTarget را مشاهده می‌کنم و بررسی می‌کنم آیا stopPropagation() در مسیر Event مانع رسیدن آن به Parent شده است یا خیر.

رابطه Bubbling و Delegation چیست؟

پاسخ طلایی:

Bubbling یک رفتار طبیعی Event System در DOM است، در حالی که Delegation یک Pattern طراحی است که معمولاً از همین Bubbling برای مدیریت Eventهای چند Child با یک Parent Listener استفاده می‌کند.

آیا کاهش تعداد Listenerها به‌تنهایی دلیل کافی برای استفاده از Delegation است؟

پاسخ طلایی:

خیر. ساختار DOM، Dynamic بودن Content، نوع Event، رفتار مشترک Childها و پیچیدگی Handler نیز باید در تصمیم‌گیری در نظر گرفته شوند.

Golden Answers

Event Propagation چیست؟

حرکت Event در مسیر DOM که شامل Capture، Target و در Eventهای قابل Bubble، Bubble Phase است.

Capture چیست؟

مرحله‌ای که Event از Ancestorهای بالاتر به سمت Target حرکت می‌کند.

Bubbling چیست؟

مرحله‌ای که Event از Target به سمت Ancestorهای DOM حرکت می‌کند.

event.target چیست؟

Elementی که Event روی آن شروع شده است.

event.currentTarget چیست؟

Elementی که Handler در حال اجرای آن قرار دارد.

stopPropagation() چیست؟

روشی برای متوقف کردن ادامه Propagation Event در مسیر DOM.

preventDefault() چیست؟

روشی برای لغو رفتار پیش‌فرض Browser.

Event Delegation چیست؟

Patternی که در آن Listener روی Parent قرار می‌گیرد تا Eventهای Childهای متعدد را با استفاده از Propagation و Target Identification مدیریت کند.

چرا Delegation برای Dynamic Elements مناسب است؟

چون Listener روی Parent باقی می‌ماند و Childهای جدید نیز می‌توانند Event خود را به همان Parent Bubble کنند.

Conclusion

در فصل قبل Event را از دید User Interaction بررسی کردیم:

User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler

اما برای فهم Eventهای واقعی در DOM، این مدل کافی نیست.

DOM یک Tree است و Event نیز در ارتباط با همین Tree حرکت می‌کند.

از همین‌جا Concept جدیدی شکل گرفت:

DOM Tree
↓
Event Propagation

برای تحلیل این Propagation، سه مرحله اصلی را شناختیم:

Capture
↓
Target
↓
Bubble

در Capture، Event به سمت Target حرکت می‌کند.

در Target، به Elementی می‌رسد که Event روی آن شروع شده است.

در Bubble، Event می‌تواند از Target به سمت Ancestorهای آن حرکت کند.

اما همین حرکت یک مسئله جدید ایجاد کرد:

اگر Handler مربوط به Parent اجرا شود، چگونه بفهمیم Event در ابتدا روی کدام Child رخ داده است؟

اینجا تفاوت میان:

event.target

و:

event.currentTarget

اهمیت پیدا می‌کند.

target مبدأ Event را مشخص می‌کند و currentTarget Element صاحب Handler فعلی را.

اکنون می‌توانیم یک مرحله جلوتر برویم.

اگر Eventهای Child به Parent می‌رسند، همیشه لازم نیست برای هر Child یک Listener مستقل ایجاد کنیم.

می‌توانیم Listener را روی Parent قرار دهیم و Eventهای Child را در همان نقطه مدیریت کنیم.

این همان چیزی است که:

Event Delegation

نام دارد.

بنابراین Delegation یک تکنیک جدا از Event System نیست؛ نتیجه طبیعی درک درست Propagation است:

DOM Tree
↓
Event Propagation
↓
Capture
↓
Target
↓
Bubble
↓
target / currentTarget
↓
Delegation
↓
Dynamic Elements

اکنون که می‌دانیم Event چگونه در DOM حرکت می‌کند، یک سؤال طبیعی باقی می‌ماند:

وقتی Event به یک Element رسید، چگونه می‌توانیم از آن Element به Parent، Child یا Siblingهای مرتبط حرکت کنیم و Element درست را پیدا کنیم؟

این سؤال، ما را مستقیماً به DOM Traversing در فصل بعد می‌رساند.