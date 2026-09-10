Chapter 33 — Array Methods
Chapter Goal

در فصل قبل دیدیم که Array چگونه چند Value را در یک Collection نگهداری می‌کند و چگونه با استفاده از Index و length به عناصر آن دسترسی پیدا می‌کنیم.

اما شناخت ساختار Array به‌تنهایی برای کار با داده‌ها کافی نیست. در یک Application واقعی باید بتوانیم Element جدید اضافه کنیم، Element موجود را حذف کنیم، داده‌ای را پیدا کنیم، بخشی از Array را جدا کنیم یا ترتیب عناصر را تغییر دهیم.

JavaScript برای این عملیات مجموعه‌ای از Array Methods را فراهم می‌کند.

هدف این فصل شناخت این Methodها به‌صورت حفظی نیست. مهم‌تر از دانستن نام هر Method، درک تفاوت رفتار آنهاست؛ به‌خصوص اینکه آیا Array اصلی را تغییر می‌دهند یا یک نتیجه جدید ایجاد می‌کنند.

Core Question

JavaScript چه ابزارهایی برای مدیریت و تغییر Arrayها فراهم می‌کند؟

مقدمه

فرض کنید در یک فروشگاه آنلاین، محصولات در یک Array نگهداری می‌شوند:

const products = ['Laptop', 'Phone', 'Tablet'];

تا زمانی که فقط بخواهیم این داده‌ها را نگهداری کنیم، ساختار Array کافی است. اما Application دائماً با تغییر داده‌ها سروکار دارد.

ممکن است محصول جدیدی اضافه شود. ممکن است آخرین محصول از لیست حذف شود. ممکن است بخواهیم بررسی کنیم محصول خاصی وجود دارد یا نه. گاهی نیز فقط به بخشی از Array نیاز داریم، بدون اینکه Collection اصلی تغییر کند.

بنابراین مسئله اصلی دیگر «چگونه یک Array بسازیم؟» نیست.

مسئله این است که چگونه Array موجود را مدیریت کنیم.

Array Methods دقیقاً برای همین منظور طراحی شده‌اند.

این Methodها رفتارهای متفاوتی دارند. بعضی از آنها Array را مستقیماً تغییر می‌دهند و بعضی دیگر بدون تغییر Array اصلی، نتیجه‌ای جدید برمی‌گردانند.

درک همین تفاوت، پایه استفاده حرفه‌ای از Array Methods است.

تغییر دادن Array

یکی از اولین نیازها هنگام کار با Collection، اضافه و حذف کردن Elementها است.

اگر محصول جدیدی وارد فروشگاه شود، ساده‌ترین راه این است که آن را به انتهای Array اضافه کنیم:

const products = ['Laptop', 'Phone'];

products.push('Tablet');

console.log(products);
// ['Laptop', 'Phone', 'Tablet']

push() یک یا چند Element را به انتهای Array اضافه می‌کند.

نکته مهم این است که push() Array جدیدی ایجاد نمی‌کند. همان Array موجود را تغییر می‌دهد.

به این تغییر مستقیم Collection، Mutation گفته می‌شود.

بنابراین پس از اجرای push()، متغیر products همچنان به همان Array اشاره می‌کند، اما محتوای آن تغییر کرده است.

مقدار بازگشتی push()

push() علاوه بر Mutation، یک Value نیز برمی‌گرداند. این Value برابر با length جدید Array است.

const products = ['Laptop', 'Phone'];

const length = products.push('Tablet');

console.log(length);
// 3

در این مثال دو اتفاق مستقل رخ داده است:

Array تغییر کرده و Tablet به آن اضافه شده است.
مقدار بازگشتی Method برابر 3 است.

این تفاوت مهم است؛ زیرا Return Value یک Method لزوماً همان Objectی نیست که Method روی آن کار کرده است.

حذف از انتهای Array

همان‌طور که push() برای اضافه کردن Element به انتهای Array استفاده می‌شود، pop() برای حذف آخرین Element به کار می‌رود.

const products = ['Laptop', 'Phone', 'Tablet'];

const product = products.pop();

console.log(products);
// ['Laptop', 'Phone']

console.log(product);
// 'Tablet'

pop() آخرین Element را از Array حذف می‌کند و همان Element حذف‌شده را برمی‌گرداند.

در نتیجه برخلاف push() که length جدید را برمی‌گرداند، pop() خود Element حذف‌شده را برمی‌گرداند.

در اینجا نیز Array اصلی تغییر کرده است:

قبل:
['Laptop', 'Phone', 'Tablet']

بعد:
['Laptop', 'Phone']

پس pop() نیز یک Mutating Method است.

این دو Method یک جفت منطقی تشکیل می‌دهند:

push() → اضافه کردن به انتها
pop() → حذف از انتها

وقتی این رابطه را درک کنیم، به‌جای حفظ کردن دو نام، رفتار آنها قابل پیش‌بینی می‌شود.

اضافه و حذف از ابتدای Array

گاهی مسئله متفاوت است. ممکن است بخواهیم Element جدیدی را در ابتدای Array قرار دهیم.

برای این کار از unshift() استفاده می‌کنیم:

const products = ['Phone', 'Tablet'];

products.unshift('Laptop');

console.log(products);
// ['Laptop', 'Phone', 'Tablet']

unshift() یک یا چند Element را به ابتدای Array اضافه می‌کند و مانند push()، خود Array را تغییر می‌دهد.

در مقابل، اگر بخواهیم اولین Element را حذف کنیم، از shift() استفاده می‌کنیم:

const products = ['Laptop', 'Phone', 'Tablet'];

const product = products.shift();

console.log(products);
// ['Phone', 'Tablet']

console.log(product);
// 'Laptop'

shift() اولین Element را حذف می‌کند و Element حذف‌شده را برمی‌گرداند.

بنابراین اکنون چهار عملیات اصلی اضافه و حذف را در اختیار داریم:

Method	عملیات	محل عملیات	Array اصلی
push()	اضافه کردن	انتها	تغییر می‌کند
pop()	حذف کردن	انتها	تغییر می‌کند
unshift()	اضافه کردن	ابتدا	تغییر می‌کند
shift()	حذف کردن	ابتدا	تغییر می‌کند

این چهار Method بخش مهمی از عملیات پایه Collection را پوشش می‌دهند.

چرا Mutation اهمیت دارد؟

در نگاه اول، تغییر دادن Array ممکن است کاملاً طبیعی باشد. اگر داده‌ای باید تغییر کند، چرا همان Array را تغییر ندهیم؟

در JavaScript این تصمیم همیشه ساده نیست.

Array یک Object است و همان‌طور که در فصل‌های قبلی دیدیم، چند Variable می‌توانند به یک Object واحد اشاره کنند.

const products = ['Laptop', 'Phone'];
const cart = products;

cart.push('Tablet');

console.log(products);
// ['Laptop', 'Phone', 'Tablet']

در اینجا cart و products دو Array مستقل نیستند. هر دو به یک Array اشاره می‌کنند.

بنابراین Mutation از طریق cart در products نیز دیده می‌شود.

این رفتار لزوماً اشتباه نیست، اما باید آگاهانه انجام شود.

به همین دلیل هنگام انتخاب Array Method فقط نباید بپرسیم:

«این Method چه کاری انجام می‌دهد؟»

باید بپرسیم:

«آیا این Method Array موجود را تغییر می‌دهد؟»

این سؤال در ادامه فصل اهمیت بیشتری پیدا می‌کند.

جست‌وجو در Array

بعد از اضافه و حذف داده‌ها، نیاز رایج دیگری مطرح می‌شود: آیا یک Value مشخص در Array وجود دارد؟

فرض کنید می‌خواهیم بررسی کنیم آیا محصول Phone در فهرست محصولات وجود دارد یا خیر.

برای چنین کاری includes() انتخاب مناسبی است:

const products = ['Laptop', 'Phone', 'Tablet'];

console.log(products.includes('Phone'));
// true

console.log(products.includes('Monitor'));
// false

includes() به‌دنبال یک Value در Array می‌گردد و نتیجه را به‌صورت Boolean برمی‌گرداند.

بنابراین اگر هدف فقط پاسخ به این سؤال باشد که:

آیا این Value وجود دارد؟

includes() مستقیماً همین اطلاعات را ارائه می‌کند.

پیدا کردن Index با indexOf()

گاهی دانستن وجود یک Element کافی نیست.

ممکن است بخواهیم بدانیم Element در کدام Index قرار دارد.

در این حالت indexOf() مناسب است:

const products = ['Laptop', 'Phone', 'Tablet'];

console.log(products.indexOf('Phone'));
// 1

console.log(products.indexOf('Monitor'));
// -1

اگر Value پیدا شود، indexOf() Index آن را برمی‌گرداند.

اگر Value در Array وجود نداشته باشد، مقدار -1 برگردانده می‌شود.

این تفاوت باعث می‌شود includes() و indexOf() با وجود شباهت، برای یک هدف دقیقاً یکسان استفاده نشوند:

includes() → آیا Value وجود دارد؟
indexOf() → Value در چه Indexی قرار دارد؟

در نتیجه انتخاب Method باید بر اساس نوع اطلاعات مورد نیاز انجام شود، نه صرفاً بر اساس اینکه هر دو می‌توانند وجود یک Value را بررسی کنند.

استخراج بخشی از Array

فرض کنید Array زیر فهرستی از محصولات فروشگاه است:

const products = [
'Laptop',
'Phone',
'Tablet',
'Monitor'
];

گاهی به کل Array نیاز نداریم. ممکن است فقط بخواهیم دو محصول اول را در یک بخش دیگر Application استفاده کنیم.

در اینجا مسئله ما حذف کردن Elementها نیست.

ما فقط به یک بخش از داده‌ها نیاز داریم.

برای چنین کاری slice() استفاده می‌شود:

const products = [
'Laptop',
'Phone',
'Tablet',
'Monitor'
];

const featuredProducts = products.slice(0, 2);

console.log(featuredProducts);
// ['Laptop', 'Phone']

slice() بخشی از Array را استخراج می‌کند و یک Array جدید برمی‌گرداند.

نکته مهم این است که Array اصلی تغییر نمی‌کند:

console.log(products);
// ['Laptop', 'Phone', 'Tablet', 'Monitor']

پس slice() با Methodهایی مانند pop() و shift() رفتار متفاوتی دارد.

pop() و shift() برای تغییر Collection موجود استفاده می‌شوند، در حالی که slice() برای گرفتن بخشی از Collection بدون تغییر آن مناسب است.

slice() و مفهوم Range

در مثال قبلی:

products.slice(0, 2);

عدد 0 نقطه شروع و 2 نقطه پایان است.

اما Element موجود در Index 2 در نتیجه قرار نمی‌گیرد.

یعنی Range به شکل زیر در نظر گرفته می‌شود:

0 ─── 1 ─── 2
│     │     │
Laptop Phone Tablet

slice(0, 2)
└── پایان قبل از Index 2

نتیجه بنابراین شامل Indexهای 0 و 1 است.

این الگوی start inclusive / end exclusive در بسیاری از APIهای JavaScript دیده می‌شود و درک آن از حفظ کردن مثال‌ها مهم‌تر است.

وقتی هدف استخراج نیست، بلکه تغییر است

گاهی نمی‌خواهیم فقط بخشی از Array را بخوانیم.

ممکن است بخواهیم Elementهایی را حذف کنیم، Element جدیدی در میان آنها قرار دهیم یا چند Element را هم‌زمان جایگزین کنیم.

اینجا مسئله تغییر ساختار Array است.

برای این کار splice() استفاده می‌شود.

const products = [
'Laptop',
'Phone',
'Tablet',
'Monitor'
];

products.splice(1, 1);

console.log(products);
// ['Laptop', 'Tablet', 'Monitor']

در این مثال، splice() از Index 1 شروع کرده و یک Element را حذف کرده است.

بنابراین Phone از Array خارج شده است.

برخلاف slice()، این بار Array اصلی تغییر کرده است.

اضافه کردن و جایگزین کردن با splice()

قدرت splice() فقط در حذف نیست.

می‌توانیم با استفاده از آن Element جدیدی نیز در یک موقعیت مشخص قرار دهیم:

const products = [
'Laptop',
'Phone',
'Monitor'
];

products.splice(2, 0, 'Tablet');

console.log(products);
// ['Laptop', 'Phone', 'Tablet', 'Monitor']

در اینجا:

2 محل شروع عملیات است.
0 یعنی هیچ Elementی حذف نشود.
'Tablet' Elementی است که باید اضافه شود.

از آنجا که تعداد حذف صفر است، Array فقط Element جدید را در موقعیت مشخص دریافت می‌کند.

splice() همچنین می‌تواند Elementهای موجود را جایگزین کند:

const products = [
'Laptop',
'Phone',
'Monitor'
];

products.splice(1, 1, 'Tablet');

console.log(products);
// ['Laptop', 'Tablet', 'Monitor']

در این مثال Phone حذف و Tablet جایگزین آن شده است.

بنابراین splice() را می‌توان به‌عنوان Methodی برای تغییر ساختار Array از یک نقطه مشخص در نظر گرفت.

تفاوت slice() و splice()

این دو Method به دلیل شباهت نام، یکی از نقاط رایج خطا برای افراد تازه‌کار هستند.

اما منطق آنها کاملاً متفاوت است.

slice() برای استخراج بخشی از Array استفاده می‌شود و Array اصلی را تغییر نمی‌دهد.

splice() برای تغییر خود Array استفاده می‌شود و می‌تواند Elementها را حذف، اضافه یا جایگزین کند.

یک مدل ذهنی ساده:

slice()
Array ───────────────► Array جدید
بدون Mutation

splice()
Array ───────────────► همان Array تغییر یافته
با Mutation

بنابراین اگر فقط بخشی از داده را می‌خواهیم، slice() معمولاً انتخاب طبیعی است.

اگر هدف تغییر ساختار Array موجود باشد، splice() ابزار مناسب‌تری است.

تبدیل Array به String

تا اینجا بیشتر Methodها مستقیماً با ساختار Array سروکار داشتند.

اما گاهی داده‌ها باید برای نمایش یا ارسال به یک سیستم دیگر به شکل String تبدیل شوند.

فرض کنید چند Ingredient در یک Array قرار دارند:

const ingredients = [
'Tomato',
'Cheese',
'Basil'
];

اگر بخواهیم آنها را به شکل یک متن نمایش دهیم، join() می‌تواند عناصر Array را به یک String تبدیل کند:

const ingredients = [
'Tomato',
'Cheese',
'Basil'
];

const text = ingredients.join(', ');

console.log(text);
// Tomato, Cheese, Basil

join() Array اصلی را تغییر نمی‌دهد.

این Method عناصر را با یک Separator مشخص به هم متصل می‌کند و یک String جدید برمی‌گرداند.

اگر Separator مشخص نکنیم، از comma به‌عنوان جداکننده استفاده می‌شود:

const ingredients = ['Tomato', 'Cheese', 'Basil'];

console.log(ingredients.join());
// Tomato,Cheese,Basil

در Applicationهای واقعی، join() زمانی مفید است که داده ساختاریافته داخل Array باید برای نمایش یا تولید یک متن تبدیل شود.

تغییر ترتیب عناصر

گاهی ترتیب Elementهای Array اهمیت دارد و لازم است ترتیب موجود برعکس شود.

برای این کار reverse() استفاده می‌شود:

const products = [
'Laptop',
'Phone',
'Tablet'
];

products.reverse();

console.log(products);
// ['Tablet', 'Phone', 'Laptop']

reverse() ترتیب عناصر Array را برعکس می‌کند.

اما نکته مهم این است که reverse() نیز Mutating Method است.

یعنی همان Array موجود تغییر می‌کند.

این موضوع با join() و slice() تفاوت دارد که Array اصلی را تغییر نمی‌دهند.

در نتیجه حتی Methodی که ظاهراً فقط یک عملیات ساده مانند «برعکس کردن» انجام می‌دهد، باید از نظر Mutation بررسی شود.

یک نگاه یکپارچه به Array Methods

اکنون می‌توانیم Methodهای این فصل را نه به‌صورت مجموعه‌ای از نام‌ها، بلکه به‌عنوان پاسخ به چند نیاز اصلی ببینیم.

اگر بخواهیم Element اضافه یا حذف کنیم، Methodهای زیر را داریم:

push()     → اضافه از انتها
pop()      → حذف از انتها
unshift()  → اضافه از ابتدا
shift()    → حذف از ابتدا

اگر بخواهیم درباره وجود یک Value یا موقعیت آن اطلاعات بگیریم:

includes() → بررسی وجود Value
indexOf()  → پیدا کردن Index

اگر بخواهیم با بخشی از Array کار کنیم:

slice()    → استخراج بدون Mutation
splice()   → تغییر Array

و برای تبدیل یا تغییر ترتیب:

join()     → Array به String
reverse()  → برعکس کردن ترتیب

اما دسته‌بندی مهم‌تر از نام Methodها، رفتار آنها نسبت به Array اصلی است.

Mutating Methods

این Methodها Array موجود را تغییر می‌دهند:

push()
pop()
shift()
unshift()
splice()
reverse()
Non-Mutating Methods

این Methodها Array اصلی را تغییر نمی‌دهند:

includes()
indexOf()
slice()
join()

این تقسیم‌بندی یک مدل ذهنی بسیار مهم ایجاد می‌کند.

وقتی Method جدیدی از Arrayها یاد می‌گیریم، یکی از اولین سؤال‌های مهندسی باید این باشد:

آیا این Method Collection موجود را تغییر می‌دهد؟

انتخاب Method مناسب

در Code واقعی معمولاً چند Method می‌توانند ظاهراً یک مسئله را حل کنند، اما انتخاب صحیح به هدف ما بستگی دارد.

اگر فقط می‌خواهیم وجود یک محصول را بررسی کنیم:

products.includes('Phone');

اگر Index محصول را لازم داریم:

products.indexOf('Phone');

اگر فقط بخشی از Array را لازم داریم و نمی‌خواهیم Collection اصلی تغییر کند:

products.slice(0, 3);

اگر می‌خواهیم خود Array را از نقطه‌ای مشخص تغییر دهیم:

products.splice(1, 2);

این نوع انتخاب، همان چیزی است که استفاده از Array Methods را از حفظ کردن Syntax جدا می‌کند.

Best Practices
1. قبل از استفاده از Method، هدف عملیات را مشخص کنید

ابتدا مشخص کنید که می‌خواهید:

Element اضافه کنید.
Element حذف کنید.
Value پیدا کنید.
بخشی از Array را استخراج کنید.
Array را تغییر دهید.
Array را به String تبدیل کنید.

سپس Method مناسب را انتخاب کنید.

2. Mutation را آگاهانه انجام دهید

Methodهایی مانند push()، pop()، splice() و reverse() Array اصلی را تغییر می‌دهند.

قبل از استفاده از آنها بررسی کنید که Mutation با طراحی Application سازگار است.

3. بین slice() و splice() تمایز قائل شوید

اگر هدف استخراج است، slice() را در نظر بگیرید.

اگر هدف تغییر Array است، splice() ابزار مربوط به این کار است.

4. Return Value را با Array اصلی اشتباه نگیرید

هر Method قرارداد بازگشت متفاوتی دارد.

برای مثال:

push() → length جدید
pop() → Element حذف‌شده
shift() → Element حذف‌شده
includes() → Boolean
indexOf() → Index یا -1
slice() → Array جدید
join() → String

دانستن Return Value برای استفاده صحیح از Method ضروری است.

5. Method را بر اساس نیاز انتخاب کنید، نه بر اساس شباهت Syntax

includes() و indexOf() هر دو می‌توانند وجود یک Value را بررسی کنند، اما یکی Boolean و دیگری Index برمی‌گرداند.

انتخاب Method باید بر اساس خروجی مورد نیاز انجام شود.

Common Mistakes
اشتباه اول: تصور اینکه push() یک Array جدید ایجاد می‌کند
const products = ['Laptop', 'Phone'];

const result = products.push('Tablet');

console.log(result);
// 3

result Array نیست؛ مقدار length جدید است.

خود Array در products تغییر کرده است.

اشتباه دوم: اشتباه گرفتن slice() و splice()
products.slice(1, 3);

Array اصلی را تغییر نمی‌دهد.

در مقابل:

products.splice(1, 2);

Array اصلی را تغییر می‌دهد.

شباهت نام این دو Method نباید باعث یکسان در نظر گرفتن رفتار آنها شود.

اشتباه سوم: تصور اینکه reverse() یک Array جدید برمی‌گرداند
const products = ['Laptop', 'Phone', 'Tablet'];

const result = products.reverse();

console.log(products);
// ['Tablet', 'Phone', 'Laptop']

reverse() خود Array را تغییر می‌دهد.

اشتباه چهارم: استفاده از indexOf() فقط برای بررسی وجود Value

اگر فقط به true یا false نیاز داریم، includes() بیان دقیق‌تری از Intent کد ارائه می‌کند:

products.includes('Phone');

در مقابل، indexOf() زمانی مناسب‌تر است که Index مورد نیاز باشد.

اشتباه پنجم: نادیده گرفتن Reference

اگر دو Variable به یک Array اشاره کنند، Mutation از طریق یکی از آنها روی دیگری نیز قابل مشاهده خواهد بود.

const products = ['Laptop', 'Phone'];
const cart = products;

cart.push('Tablet');

console.log(products);
// ['Laptop', 'Phone', 'Tablet']

بنابراین Mutation فقط یک جزئیات مربوط به Method نیست؛ با مفهوم Reference در Objectها نیز ارتباط مستقیم دارد.

Summary

Array Methods مجموعه‌ای از ابزارهای آماده JavaScript برای کار با Collectionهای نوع Array هستند.

برای اضافه و حذف Elementها می‌توان از push()، pop()، unshift() و shift() استفاده کرد. این Methodها Array اصلی را تغییر می‌دهند.

برای جست‌وجو، includes() وجود یک Value را بررسی می‌کند و indexOf() موقعیت آن را مشخص می‌کند.

slice() برای استخراج بخشی از Array بدون تغییر Array اصلی استفاده می‌شود، در حالی که splice() می‌تواند ساختار Array موجود را تغییر دهد و Elementها را حذف، اضافه یا جایگزین کند.

join() عناصر Array را به یک String تبدیل می‌کند و reverse() ترتیب عناصر را برعکس می‌کند. reverse() برخلاف join() یک Mutating Method است.

در نهایت، مهم‌ترین نکته این فصل حفظ کردن فهرست Methodها نیست. باید بتوانیم بر اساس مسئله تشخیص دهیم:

چه عملی می‌خواهیم انجام دهیم؟

و مهم‌تر:

آیا می‌خواهیم Array موجود تغییر کند یا فقط نتیجه‌ای جدید لازم داریم؟

Key Takeaways
push() و unshift() برای اضافه کردن Element استفاده می‌شوند.
pop() و shift() برای حذف Element استفاده می‌شوند.
push() مقدار length جدید را برمی‌گرداند.
pop() و shift() Element حذف‌شده را برمی‌گردانند.
includes() یک Boolean برمی‌گرداند.
indexOf() Index مربوط به Value را برمی‌گرداند و در صورت نبودن آن -1 می‌دهد.
slice() Array اصلی را تغییر نمی‌دهد.
splice() Array اصلی را تغییر می‌دهد.
slice() برای استخراج و splice() برای تغییر ساختار Array مناسب است.
join() Array را به String تبدیل می‌کند.
reverse() ترتیب Array را تغییر می‌دهد.
Mutation باید آگاهانه انجام شود.
Return Value هر Method بخشی از قرارداد آن Method است.
انتخاب Method باید بر اساس Intent و نتیجه مورد نیاز انجام شود.
Technical Interview
Junior
1. push() و pop() چه تفاوتی دارند؟

push() یک یا چند Element را به انتهای Array اضافه می‌کند و length جدید را برمی‌گرداند. pop() آخرین Element را حذف می‌کند و همان Element حذف‌شده را برمی‌گرداند. هر دو Array اصلی را تغییر می‌دهند.

2. تفاوت shift() و unshift() چیست؟

unshift() Element را به ابتدای Array اضافه می‌کند، در حالی که shift() اولین Element را حذف می‌کند. هر دو Array اصلی را تغییر می‌دهند.

3. includes() چه چیزی برمی‌گرداند؟

یک Boolean که مشخص می‌کند Value مورد نظر در Array وجود دارد یا خیر.

4. اگر indexOf() نتواند یک Value را پیدا کند چه چیزی برمی‌گرداند؟

مقدار -1.

Mid-Level
5. تفاوت اصلی slice() و splice() چیست؟

slice() بخشی از Array را استخراج می‌کند و یک Array جدید برمی‌گرداند، بدون اینکه Array اصلی را تغییر دهد. splice() خود Array را تغییر می‌دهد و می‌تواند Elementها را حذف، اضافه یا جایگزین کند.

6. کدام Array Methods در این فصل Mutating هستند؟

push()، pop()، shift()، unshift()، splice() و reverse().

7. چرا دانستن Mutation در کار با Arrayها مهم است؟

زیرا Array یک Object است و ممکن است چند Variable به همان Array اشاره کنند. Mutation از طریق یکی از Referenceها می‌تواند روی داده‌ای که بخش دیگری از Application استفاده می‌کند نیز اثر بگذارد.

8. چه زمانی includes() را به indexOf() ترجیح می‌دهید؟

وقتی فقط می‌خواهیم بدانیم Value وجود دارد یا خیر و به Index آن نیازی نداریم. includes() Intent کد را نیز مستقیم‌تر بیان می‌کند.

Senior
9. چرا تشخیص Mutating و Non-Mutating Methods از حفظ کردن Syntax مهم‌تر است؟

زیرا Mutation روی State موجود اثر می‌گذارد و می‌تواند در بخش‌های دیگری از Application که همان Reference را دارند قابل مشاهده باشد. بنابراین این ویژگی مستقیماً بر Predictability و مدیریت State اثر می‌گذارد.

10. چگونه بین slice() و splice() از نظر طراحی API تصمیم می‌گیرید؟

اگر نیاز فقط به استخراج بخشی از Collection باشد و حفظ Array اصلی اهمیت داشته باشد، slice() انتخاب مناسب است. اگر عملیات مورد نظر مستقیماً تغییر ساختار Array موجود باشد، splice() مناسب است.

11. آیا Return Value یک Array Method الزاماً Array است؟

خیر. Return Value بسته به Method متفاوت است. push() عدد length جدید، pop() یک Element، includes() یک Boolean، indexOf() یک Index و join() یک String برمی‌گرداند.

12. چرا Mutation یک موضوع مهم مهندسی است، نه صرفاً یک ویژگی Syntax؟

زیرا Arrayها Reference Value هستند و تغییر یک Object می‌تواند از طریق Referenceهای دیگر نیز مشاهده شود. بنابراین تصمیم درباره Mutation بخشی از طراحی رفتار State در Application است.

Golden Answers
push() چیست؟

push() یک یا چند Element را به انتهای Array اضافه می‌کند. این Method Array اصلی را تغییر می‌دهد و length جدید Array را برمی‌گرداند.

pop() چیست؟

pop() آخرین Element Array را حذف می‌کند. Array اصلی را تغییر می‌دهد و Element حذف‌شده را برمی‌گرداند.

تفاوت slice() و splice() چیست؟

slice() بخشی از Array را بدون تغییر Array اصلی استخراج می‌کند، در حالی که splice() خود Array را تغییر می‌دهد و برای حذف، اضافه یا جایگزینی Elementها استفاده می‌شود.

تفاوت includes() و indexOf() چیست؟

includes() بررسی می‌کند Value در Array وجود دارد یا خیر و Boolean برمی‌گرداند. indexOf() موقعیت Value را برمی‌گرداند و در صورت نبودن آن -1 می‌دهد.

Mutation در Array چیست؟

Mutation یعنی تغییر مستقیم Array موجود. Methodهایی مانند push()، pop()، splice() و reverse() نمونه‌هایی از Methodهای Mutating هستند.

چرا Mutation باید آگاهانه انجام شود؟

زیرا Array یک Object است و چند Reference می‌توانند به یک Array اشاره کنند. تغییر Array از طریق یک Reference می‌تواند نتیجه را از طریق Referenceهای دیگر نیز تغییر دهد.

Conclusion

Array زمانی ارزش واقعی خود را در یک Application نشان می‌دهد که بتوانیم Collection را متناسب با نیاز برنامه مدیریت کنیم.

JavaScript برای این کار Methodهای مختلفی در اختیار ما قرار می‌دهد؛ از اضافه و حذف Elementها گرفته تا جست‌وجو، استخراج، تغییر ساختار و تبدیل داده.

اما استفاده حرفه‌ای از این Methodها با حفظ کردن نام آنها شکل نمی‌گیرد.

مدل ذهنی درست این است که ابتدا نوع عملیات را مشخص کنیم و سپس رفتار Method را در نظر بگیریم.

آیا می‌خواهیم Array موجود را تغییر دهیم؟

یا فقط به نتیجه‌ای جدید نیاز داریم؟

آیا به وجود یک Value نیاز داریم یا Index آن را می‌خواهیم؟

آیا قصد استخراج بخشی از Array را داریم یا می‌خواهیم ساختار آن را تغییر دهیم؟

وقتی این تفاوت‌ها روشن باشند، انتخاب Array Method دیگر مجموعه‌ای از قواعد حفظی نخواهد بود؛ بلکه نتیجه طبیعی درک رفتار Collection خواهد بود.

در فصل بعد، تمرکز از مدیریت ساختار Array به Iteration روی عناصر آن منتقل می‌شود؛ جایی که باید بتوانیم روی Collection حرکت کنیم و هر Element را به‌صورت کنترل‌شده پردازش کنیم.