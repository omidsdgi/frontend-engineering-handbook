Chapter 34 — Array Iteration
Chapter Goal

خواننده بتواند روی عناصر یک Array به‌صورت کنترل‌شده، خوانا و متناسب با نیاز برنامه Iteration انجام دهد و تفاوت میان الگوهای مختلف Iteration را از نظر رفتار و کاربرد تشخیص دهد.

Core Question

چگونه روی عناصر Array به‌صورت کنترل‌شده و خوانا Iteration انجام دهیم؟

مقدمه

در فصل قبل دیدیم که Array مجموعه‌ای از Elementها را در اختیار برنامه قرار می‌دهد و با استفاده از Array Methods می‌توانیم این Collection را مدیریت کنیم.

اما مدیریت Array فقط به اضافه کردن، حذف کردن یا جست‌وجوی Elementها محدود نمی‌شود.

در یک Application واقعی معمولاً لازم است روی تمام عناصر یک Collection حرکت کنیم و برای هر Element عملی انجام دهیم.

برای مثال، فرض کنید محصولات یک فروشگاه در Array زیر قرار دارند:

const products = [
'Laptop',
'Phone',
'Tablet'
];

ممکن است بخواهیم نام تمام محصولات را نمایش دهیم، قیمت آنها را بررسی کنیم یا برای هر محصول عملیاتی انجام دهیم.

در چنین شرایطی یک سؤال جدید مطرح می‌شود:

چگونه از Element اول Array به Element بعدی برویم و این کار را تا پایان Collection ادامه دهیم؟

این فرآیند Iteration نام دارد.

Iteration یعنی اجرای یک عملیات به‌صورت تکرارشونده روی عناصر یک Collection.

Array یکی از رایج‌ترین ساختارهایی است که Iteration روی آن انجام می‌شود. JavaScript نیز چند روش برای این کار فراهم می‌کند.

در این فصل ابتدا با روش سنتی for شروع می‌کنیم. سپس می‌بینیم چگونه for...of همین مسئله را ساده‌تر می‌کند و در نهایت به forEach() می‌رسیم؛ روشی که Iteration را با استفاده از Callback Function بیان می‌کند.

هدف این نیست که یکی از این روش‌ها را همیشه «بهترین» بدانیم.

هدف این است که بدانیم هر الگو چه مسئله‌ای را بهتر حل می‌کند.

چرا به Iteration نیاز داریم؟

فرض کنید می‌خواهیم تمام محصولات موجود در یک Array را در Console نمایش دهیم.

یک راه این است که هر Element را جداگانه بنویسیم:

console.log(products[0]);
console.log(products[1]);
console.log(products[2]);

اما این روش فقط زمانی ممکن است که تعداد Elementها از قبل مشخص باشد.

اگر فردا محصول جدیدی به Array اضافه شود، کد باید تغییر کند.

مشکل اصلی این است که کد ما به تعداد Elementها وابسته شده است.

Iteration این وابستگی را از بین می‌برد.

به‌جای اینکه بگوییم:

Element اول را بخوان، سپس Element دوم را بخوان، سپس Element سوم را بخوان.

می‌گوییم:

روی تمام Elementهای این Array حرکت کن و این عملیات را برای هر Element انجام بده.

این تفاوت، Iteration را به یک الگوی بنیادی در کار با Collectionها تبدیل می‌کند.

Iteration با for

اولین راه‌حل، همان Loop سنتی for است که در مباحث اولیه JavaScript با آن آشنا شدیم.

const products = [
'Laptop',
'Phone',
'Tablet'
];

for (let i = 0; i < products.length; i++) {
console.log(products[i]);
}

در اینجا متغیر i نقش Counter را دارد.

Iteration از 0 شروع می‌شود؛ زیرا اولین Element Array در Index صفر قرار دارد.

در هر مرحله، i یک واحد افزایش پیدا می‌کند و تا زمانی که شرط i < products.length برقرار باشد، Loop ادامه پیدا می‌کند.

به این ترتیب i به‌ترتیب Indexهای Array را طی می‌کند:

0 → 1 → 2

و در هر مرحله با استفاده از products[i] به Element مربوطه دسترسی پیدا می‌کنیم.

در این مدل، برنامه‌نویس کنترل کاملی روی Iteration دارد.

خودمان مشخص می‌کنیم:

Iteration از کجا شروع شود.
چه زمانی ادامه پیدا کند.
در هر مرحله چه اتفاقی بیفتد.
Counter چگونه تغییر کند.

این کنترل یکی از مهم‌ترین ویژگی‌های for است.

اما همین کنترل زیاد می‌تواند باعث افزایش جزئیات کد شود.

ما باید Counter ایجاد کنیم، شرط بنویسیم، آن را افزایش دهیم و سپس Element را با Index دریافت کنیم.

برای یک Array ساده، این جزئیات همیشه ضروری نیستند.

همین مسئله ما را به سؤال بعدی می‌رساند:

اگر فقط می‌خواهیم Elementهای Array را یکی‌یکی دریافت کنیم، آیا واقعاً به مدیریت مستقیم Index نیاز داریم؟

Iteration با for...of

در بسیاری از موارد، هدف ما کار کردن با خود Elementها است، نه با Index آنها.

برای چنین شرایطی JavaScript syntax ساده‌تری ارائه می‌کند:

const products = [
'Laptop',
'Phone',
'Tablet'
];

for (const product of products) {
console.log(product);
}

در اینجا دیگر Counter و دسترسی مستقیم به Index وجود ندارد.

for...of در هر Iteration، مقدار Element فعلی را در متغیر product قرار می‌دهد.

بنابراین می‌توانیم به‌جای فکر کردن درباره حرکت Indexها، مستقیماً روی Valueها تمرکز کنیم.

این تفاوت کوچک از نظر خوانایی اهمیت زیادی دارد.

در روش قبلی باید ذهن خود را با این منطق درگیر کنیم:

Index → Array[Index] → Element

اما در for...of مستقیماً داریم:

Element → Operation

به همین دلیل وقتی هدف فقط پیمایش Elementهای Array است، for...of معمولاً بیان واضح‌تری از Intent کد ارائه می‌کند.

چه زمانی Index مهم است؟

سادگی for...of به این معنا نیست که for دیگر کاربردی ندارد.

گاهی Index بخشی از مسئله است.

فرض کنید می‌خواهیم در کنار نام محصول، موقعیت آن را نیز نمایش دهیم:

const products = [
'Laptop',
'Phone',
'Tablet'
];

for (let i = 0; i < products.length; i++) {
console.log(`${i}: ${products[i]}`);
}

در اینجا Index بخشی از منطق برنامه است.

بنابراین for هنوز انتخاب مناسبی است؛ زیرا کنترل مستقیم روی Index را در اختیار داریم.

این مثال یک اصل مهم را نشان می‌دهد:

انتخاب Loop باید بر اساس اطلاعاتی باشد که در Iteration به آنها نیاز داریم.

اگر فقط Element لازم است، for...of ساده‌تر است.

اگر Index یا کنترل دقیق Iteration لازم باشد، for انعطاف بیشتری دارد.

for...of و مفهوم Iterable

تا اینجا for...of را روی Array استفاده کردیم.

اما دلیل اینکه این Syntax روی Array کار می‌کند این است که Array یک Iterable است.

Iterable به ساختاری گفته می‌شود که JavaScript می‌تواند مقادیر آن را به‌ترتیب یکی‌یکی در اختیار for...of قرار دهد.

در این فصل لازم نیست وارد جزئیات Protocolهای مربوط به Iterable شویم. نکته مهم برای مدل ذهنی این است که for...of برای پیمایش Valueهای یک Iterable طراحی شده است.

به همین دلیل for...of فقط محدود به Array نیست.

اما در این فصل تمرکز ما روی Array است؛ زیرا Iteration روی Array زمینه ورود به Array Methods و Callback-based processing در فصل‌های بعدی است.

از Loop به Callback

تا اینجا دو روش برای Iteration داریم.

با for خودمان تمام مراحل کنترل Loop را مدیریت می‌کنیم.

با for...of بخشی از این جزئیات حذف می‌شود و مستقیماً با Elementها کار می‌کنیم.

اما هنوز یک سؤال دیگر وجود دارد.

فرض کنید هدف ما همیشه یکسان است:

برای هر Element یک Function اجرا کن.

در این حالت می‌توانیم عملیات مورد نظر را به شکل یک Function تعریف کنیم و اجرای آن را به مکانیزم Iteration واگذار کنیم.

این همان نقطه‌ای است که مفهوم Callback اهمیت پیدا می‌کند.

در JavaScript، Function یک Value است و می‌توان آن را به Function دیگری به‌عنوان Argument ارسال کرد.

function printProduct(product) {
console.log(product);
}

اکنون می‌توانیم این Function را در اختیار مکانیزمی قرار دهیم که برای هر Element آن را اجرا کند.

Array Methodی که برای این کار طراحی شده است forEach() نام دارد.

Iteration با forEach()

forEach() روی Elementهای Array حرکت می‌کند و برای هر Element، Callback Function مشخص‌شده را اجرا می‌کند.

const products = [
'Laptop',
'Phone',
'Tablet'
];

products.forEach(function (product) {
console.log(product);
});

در اینجا ما دیگر Loop را مستقیماً کنترل نمی‌کنیم.

به forEach() می‌گوییم:

این Array را پیمایش کن و برای هر Element این Function را اجرا کن.

در نتیجه مسئولیت Iteration از کد اصلی ما به forEach() واگذار شده است.

همین موضوع باعث می‌شود ساختار کد بر چیزی که می‌خواهیم انجام دهیم تمرکز کند، نه بر جزئیات حرکت روی Array.

Callback در forEach()

Functionای که به forEach() ارسال می‌کنیم، یک Callback Function است.

products.forEach(function (product) {
console.log(product);
});

forEach() خودش این Function را تعریف نکرده است.

ما Function را در اختیار آن قرار داده‌ایم و forEach() در طول Iteration آن را برای هر Element فراخوانی می‌کند.

این همان مفهوم Callback است که در فصل‌های مربوط به Functionها بررسی کردیم:

یک Function را به Function یا سیستم دیگری می‌دهیم تا در زمان مناسب اجرا شود.

در forEach()، زمان مناسب بسیار مشخص است:

هر بار که Iteration به یک Element می‌رسد.

Argumentهای Callback

Callback مربوط به forEach() می‌تواند اطلاعات بیشتری درباره Iteration دریافت کند.

سه مقدار اصلی در اختیار Callback قرار می‌گیرد:

currentValue
index
array

برای مثال:

const products = [
'Laptop',
'Phone',
'Tablet'
];

products.forEach(function (product, index) {
console.log(index, product);
});

در اینجا:

product مقدار Element فعلی است.
index موقعیت آن Element در Array است.

بنابراین خروجی مفهومی چنین خواهد بود:

0 Laptop
1 Phone
2 Tablet

در صورت نیاز می‌توانیم خود Array را نیز به‌عنوان Argument سوم دریافت کنیم:

products.forEach(function (product, index, array) {
console.log(product, index, array);
});

اما در بسیاری از کاربردهای واقعی فقط currentValue کافی است.

اصل مهم این است که Callback باید فقط اطلاعاتی را دریافت کند که واقعاً به آن نیاز دارد.

Arrow Function و خوانایی forEach()

از آنجا که Callback یک Function است، می‌توانیم از Arrow Function نیز استفاده کنیم:

products.forEach(product => {
console.log(product);
});

اگر عملیات کوتاه باشد، این شکل معمولاً خوانایی بیشتری دارد.

مثلاً:

products.forEach(product => console.log(product));

اما کوتاه‌تر بودن همیشه به معنی بهتر بودن نیست.

اگر Callback منطق بیشتری داشته باشد، استفاده از Block Body می‌تواند خوانایی را حفظ کند:

products.forEach(product => {
const message = `Product: ${product}`;

console.log(message);
});

هدف Syntax کوتاه‌تر، کاهش نویز است؛ نه حذف ساختار منطقی کد.

کنترل Iteration

تا اینجا تفاوت مهمی میان for و forEach() شکل گرفته است.

در for می‌توانیم کنترل مستقیم روی Loop داشته باشیم.

مثلاً می‌توانیم با break از Loop خارج شویم:

const products = [
'Laptop',
'Phone',
'Tablet',
'Monitor'
];

for (const product of products) {
if (product === 'Tablet') {
break;
}

console.log(product);
}

در این مثال Iteration هنگام رسیدن به Tablet متوقف می‌شود.

همچنین continue اجازه می‌دهد Iteration فعلی را نادیده بگیریم و به مرحله بعد برویم:

for (const product of products) {
if (product === 'Phone') {
continue;
}

console.log(product);
}

اما forEach() چنین کنترل مستقیمی را ارائه نمی‌کند.

نمی‌توانیم با break یا continue از Callback مربوط به forEach() خارج شویم.

این تفاوت یکی از مهم‌ترین معیارهای انتخاب بین Loopها است.

چرا return در forEach() جایگزین break نیست؟

یک سوءبرداشت رایج این است که می‌توانیم داخل Callback از return استفاده کنیم تا کل Iteration متوقف شود.

products.forEach(product => {
if (product === 'Tablet') {
return;
}

console.log(product);
});

در اینجا return فقط اجرای همان Callback فعلی را پایان می‌دهد.

forEach() به Iteration خود ادامه می‌دهد و Elementهای بعدی نیز پردازش می‌شوند.

بنابراین:

return
↓
پایان Callback فعلی

break
↓
پایان Loop

این دو رفتار یکسان نیستند.

اگر واقعاً نیاز داریم Iteration را متوقف کنیم، for یا for...of انتخاب مناسب‌تری است.

مقایسه for، for...of و forEach()

اکنون سه الگوی اصلی این فصل را داریم.

for

for بیشترین کنترل را در اختیار برنامه‌نویس قرار می‌دهد.

می‌توانیم Index را مدیریت کنیم و از break و continue استفاده کنیم.

این روش زمانی مفید است که منطق Iteration به کنترل دقیق Loop وابسته باشد.

for...of

for...of زمانی مناسب است که بخواهیم مستقیماً روی Valueهای Array حرکت کنیم.

کد معمولاً ساده‌تر از for است و همچنان امکان استفاده از break و continue را داریم.

forEach()

forEach() زمانی مناسب است که می‌خواهیم برای هر Element یک عملیات مشخص انجام دهیم و نیازی به توقف یا کنترل مستقیم Loop نداریم.

در این روش Iteration به Array Method واگذار می‌شود و عملیات مورد نظر در قالب Callback بیان می‌شود.

ویژگی	for	for...of	forEach()
دسترسی مستقیم به Index	بله	خیر	بله، از طریق Callback
دسترسی مستقیم به Element	از طریق Index	بله	بله
break	بله	بله	خیر
continue	بله	بله	خیر
Callback	خیر	خیر	بله
کنترل مستقیم Loop	زیاد	زیاد	محدود
خوانایی برای Iteration ساده	متوسط	بالا	بالا

این جدول نباید به یک قانون مطلق تبدیل شود.

انتخاب Loop همیشه به نیاز واقعی کد بستگی دارد.

انتخاب الگوی مناسب

اکنون می‌توانیم به Core Question فصل پاسخ دقیق‌تری بدهیم.

اگر به کنترل Index و ساختار Loop نیاز داریم، for انتخاب مناسبی است.

اگر فقط می‌خواهیم روی Valueهای یک Array حرکت کنیم و امکان break یا continue نیز برایمان مهم است، for...of معمولاً انتخاب ساده و خوانایی است.

اگر هدف این است که یک عملیات مشخص برای تمام Elementها اجرا شود و کنترل مستقیم Loop لازم نیست، forEach() می‌تواند انتخاب مناسبی باشد.

در نتیجه نباید صرفاً به دلیل جدیدتر یا کوتاه‌تر بودن یک Syntax آن را انتخاب کنیم.

سؤال درست این است:

در این Iteration چه مقدار کنترل نیاز دارم؟

این سؤال معمولاً انتخاب مناسب را مشخص می‌کند.

یک الگوی واقعی

فرض کنید اطلاعات سفارش‌های یک فروشگاه در اختیار ما قرار گرفته است:

const orders = [
'Order #1001',
'Order #1002',
'Order #1003'
];

اگر فقط بخواهیم همه سفارش‌ها را پردازش کنیم:

orders.forEach(order => {
console.log(`Processing ${order}`);
});

اما اگر بخواهیم هنگام رسیدن به یک سفارش خاص Iteration را متوقف کنیم، for...of مناسب‌تر است:

for (const order of orders) {
if (order === 'Order #1002') {
break;
}

console.log(`Processing ${order}`);
}

در اینجا انتخاب Method بر اساس نیاز رفتاری انجام شده است.

کد اول بر اجرای یکسان عملیات روی تمام Elementها تمرکز دارد.

کد دوم به کنترل جریان Iteration نیاز دارد.

آیا forEach() همیشه خواناتر است؟

خیر.

خوانایی به هدف کد وابسته است.

این کد:

products.forEach(product => {
console.log(product);
});

برای یک عملیات ساده کاملاً خوانا است.

اما اگر منطق Iteration به چند شرط و کنترل جریان وابسته شود، استفاده از for...of ممکن است Intent را واضح‌تر کند:

for (const product of products) {
if (!product) {
continue;
}

if (product === 'Tablet') {
break;
}

console.log(product);
}

در اینجا for...of ساختار کنترل را به‌صورت مستقیم نشان می‌دهد.

بنابراین هدف حرفه‌ای، استفاده از یک Syntax خاص نیست.

هدف انتخاب واضح‌ترین ابزار برای مسئله موجود است.

Best Practices
1. ابتدا نیاز Iteration را مشخص کنید

قبل از انتخاب Loop مشخص کنید که آیا به:

Element
Index
کنترل Loop
توقف Iteration
یا اجرای یک Callback برای هر Element

نیاز دارید.

سپس Syntax مناسب را انتخاب کنید.

2. برای Iteration ساده روی Valueها، for...of را در نظر بگیرید

اگر فقط Elementهای Array را یکی‌یکی می‌خواهید، for...of معمولاً خوانایی خوبی دارد.

3. forEach() را برای اجرای عملیات روی هر Element استفاده کنید

وقتی قصد دارید یک عملیات مشخص را برای تمام Elementها اجرا کنید و کنترل مستقیم Loop لازم نیست، forEach() انتخاب مناسبی است.

4. از for یا for...of برای کنترل جریان استفاده کنید

اگر break یا continue بخشی از منطق مسئله است، Loopهایی مانند for و for...of مناسب‌تر هستند.

5. Callback را تا حد امکان ساده نگه دارید

اگر Callback بسیار طولانی شود، خوانایی کاهش پیدا می‌کند.

در چنین شرایطی می‌توان منطق را به یک Function مستقل منتقل کرد.

6. فقط Argumentهای مورد نیاز Callback را دریافت کنید

اگر فقط Value فعلی لازم است:

products.forEach(product => {
console.log(product);
});

نیازی نیست index و array را نیز دریافت کنیم.

Common Mistakes
اشتباه اول: استفاده از forEach() زمانی که نیاز به break داریم

forEach() امکان استفاده مستقیم از break برای توقف Iteration را ندارد.

اگر توقف بخشی از منطق برنامه است، از for یا for...of استفاده کنید.

اشتباه دوم: تصور اینکه return در forEach() کل Iteration را متوقف می‌کند

return فقط Callback فعلی را تمام می‌کند.

Iteration توسط forEach() ادامه پیدا می‌کند.

اشتباه سوم: استفاده از for بدون نیاز به Index

این کد:

for (let i = 0; i < products.length; i++) {
console.log(products[i]);
}

کاملاً صحیح است، اما اگر هیچ نیازی به Index نداریم، for...of معمولاً Intent را واضح‌تر بیان می‌کند:

for (const product of products) {
console.log(product);
}
اشتباه چهارم: تصور اینکه forEach() یک Loop قابل توقف معمولی است

forEach() یک Array Method است که Callback را برای هر Element اجرا می‌کند.

مدل ذهنی آن با for متفاوت است و نباید انتظار کنترل مشابهی از آن داشته باشیم.

اشتباه پنجم: انتخاب Syntax بر اساس کوتاه بودن

کوتاه‌ترین کد همیشه خواناترین کد نیست.

Loop باید بر اساس نیاز رفتاری انتخاب شود، نه صرفاً تعداد خطوط.

Summary

Iteration فرآیند حرکت کنترل‌شده روی عناصر یک Collection است.

در Arrayها می‌توان با روش‌های مختلفی این کار را انجام داد.

for کنترل مستقیم روی Counter، شرط و جریان Loop را فراهم می‌کند.

for...of بسیاری از جزئیات مربوط به Index را حذف می‌کند و اجازه می‌دهد مستقیماً روی Valueهای Iterable حرکت کنیم.

forEach() Iteration را به Array Method واگذار می‌کند و برای هر Element یک Callback را اجرا می‌کند.

این Callback می‌تواند currentValue، index و خود array را دریافت کند.

تفاوت مهم دیگر به Iteration Control مربوط است.

for و for...of امکان استفاده از break و continue را فراهم می‌کنند، اما forEach() چنین کنترل مستقیمی ندارد.

در نتیجه انتخاب Loop نباید بر اساس یک قانون ثابت انجام شود.

اگر کنترل دقیق لازم است، for انتخاب قدرتمندی است.

اگر تمرکز روی Valueها است، for...of معمولاً خوانایی خوبی دارد.

اگر یک عملیات باید برای تمام Elementها اجرا شود و کنترل مستقیم Loop لازم نیست، forEach() می‌تواند انتخاب مناسبی باشد.

Key Takeaways
Iteration یعنی حرکت کنترل‌شده روی عناصر یک Collection.
for کنترل مستقیم روی Loop و Index را فراهم می‌کند.
for...of مستقیماً Valueهای Iterable را در اختیار برنامه قرار می‌دهد.
Array یک Iterable است و بنابراین با for...of قابل پیمایش است.
forEach() برای هر Element یک Callback را اجرا می‌کند.
Callback مربوط به forEach() می‌تواند currentValue، index و array را دریافت کند.
for و for...of از break و continue پشتیبانی می‌کنند.
forEach() امکان استفاده مستقیم از break و continue را ندارد.
return در Callback مربوط به forEach() فقط همان Callback را پایان می‌دهد.
انتخاب Loop باید بر اساس نیاز واقعی Iteration انجام شود.
کوتاه‌تر بودن Syntax به‌تنهایی معیار مناسبی برای انتخاب نیست.
خوانایی زمانی ایجاد می‌شود که ساختار Loop، Intent برنامه را به‌وضوح نشان دهد.
Technical Interview
Junior
1. Iteration چیست؟

Iteration فرآیند پیمایش عناصر یک Collection و اجرای یک عملیات روی آنها به‌صورت تکرارشونده است.

2. تفاوت for و for...of چیست؟

for کنترل مستقیم روی Index و ساختار Loop دارد، در حالی که for...of مستقیماً Valueهای Iterable را در اختیار برنامه قرار می‌دهد.

3. forEach() چه کاری انجام می‌دهد؟

forEach() روی عناصر Array حرکت می‌کند و Callback مشخص‌شده را برای هر Element اجرا می‌کند.

4. آیا forEach() از break پشتیبانی می‌کند؟

خیر. break نمی‌تواند Iteration مربوط به forEach() را متوقف کند.

Mid-Level
5. چه زمانی for...of را به for ترجیح می‌دهید؟

وقتی به Index یا کنترل پیچیده Counter نیاز نداریم و هدف اصلی پیمایش مستقیم Valueهای Array است.

6. Callback مربوط به forEach() چه Argumentهایی دریافت می‌کند؟

به‌ترتیب currentValue، index و array.

7. تفاوت return در Callback مربوط به forEach() با break در for چیست؟

return فقط اجرای Callback فعلی را پایان می‌دهد، در حالی که break کل Loop را متوقف می‌کند.

8. چرا forEach() همیشه جایگزین مناسبی برای for نیست؟

زیرا forEach() کنترل مستقیمی مانند break و continue ارائه نمی‌دهد و در شرایطی که منطق Iteration به کنترل جریان نیاز دارد، for یا for...of مناسب‌تر هستند.

Senior
9. معیار اصلی انتخاب بین for، for...of و forEach() چیست؟

معیار اصلی، نیاز رفتاری Iteration است؛ مانند نیاز به Index، کنترل جریان، توقف Iteration یا اجرای یک Callback برای هر Element. انتخاب نباید صرفاً بر اساس کوتاهی Syntax باشد.

10. چرا for...of می‌تواند Intent کد را بهتر از for بیان کند؟

زیرا زمانی که هدف فقط پیمایش Valueها است، جزئیات غیرضروری مانند Counter و دسترسی دستی به Index حذف می‌شوند و کد مستقیماً روی Element تمرکز می‌کند.

11. چرا forEach() را نمی‌توان از نظر کنترل Iteration معادل for دانست؟

زیرا forEach() اجرای Iteration را مدیریت می‌کند و Callback را برای هر Element فراخوانی می‌کند. برنامه‌نویس کنترل مستقیمی مانند break و continue روی این Iteration ندارد.

12. آیا استفاده از forEach() همیشه از نظر خوانایی بهتر است؟

خیر. خوانایی به Intent کد بستگی دارد. برای عملیات ساده روی تمام Elementها، forEach() بسیار خوانا است؛ اما وقتی کنترل جریان و توقف Iteration اهمیت دارد، for یا for...of ممکن است ساختار منطقی را واضح‌تر نشان دهند.

Golden Answers
Iteration چیست؟

Iteration فرآیند پیمایش عناصر یک Collection و اجرای یک عملیات برای هر Element است.

تفاوت for و for...of چیست؟

for کنترل مستقیم روی Index، Counter و جریان Loop را فراهم می‌کند. for...of مستقیماً Valueهای یک Iterable را پیمایش می‌کند و برای Iteration ساده روی عناصر خواناتر است.

forEach() چیست؟

forEach() یک Array Method است که Callback مشخص‌شده را برای هر Element Array اجرا می‌کند.

Callback در forEach() چه اطلاعاتی دریافت می‌کند؟

Callback می‌تواند currentValue، index و array را به‌ترتیب دریافت کند.

آیا می‌توان forEach() را با break متوقف کرد؟

خیر. forEach() کنترل مستقیم برای break ندارد. اگر توقف Iteration لازم باشد، for یا for...of انتخاب مناسب‌تری است.

return در forEach() چه کاری انجام می‌دهد؟

return فقط اجرای Callback فعلی را پایان می‌دهد و باعث توقف کل Iteration نمی‌شود.

چه زمانی for...of انتخاب مناسبی است؟

وقتی هدف پیمایش مستقیم Valueهای یک Iterable است و به کنترل Index نیاز نداریم، اما همچنان ممکن است به break یا continue نیاز داشته باشیم.

چگونه Loop مناسب را انتخاب کنیم؟

ابتدا نیاز Iteration را مشخص می‌کنیم. اگر کنترل دقیق Index و Loop لازم است، for مناسب است. اگر پیمایش مستقیم Valueها هدف اصلی است، for...of گزینه مناسبی است. اگر فقط اجرای یک Callback برای هر Element مورد نیاز است و کنترل مستقیم Loop لازم نیست، forEach() می‌تواند انتخاب مناسبی باشد.

Conclusion

Iteration زمانی مطرح می‌شود که Collection دیگر فقط داده‌ای برای نگهداری نیست، بلکه باید روی عناصر آن عملیاتی انجام شود.

در Array می‌توان این عملیات را با روش‌های مختلف انجام داد.

for بیشترین کنترل را در اختیار ما قرار می‌دهد. for...of این کنترل را ساده‌تر می‌کند و تمرکز را از Index به Value منتقل می‌کند. forEach() یک گام دیگر پیش می‌رود و اجرای عملیات را در قالب یک Callback به Array Method واگذار می‌کند.

هیچ‌کدام از این روش‌ها به‌صورت مطلق بهترین نیستند.

یک برنامه‌نویس حرفه‌ای ابتدا مسئله را بررسی می‌کند و سپس ابزار مناسب را انتخاب می‌کند.

اگر Index و کنترل جریان اهمیت دارد، for انتخاب طبیعی است.

اگر هدف پیمایش مستقیم Valueها است، for...of معمولاً انتخاب خواناتری است.

اگر هدف اجرای یک عملیات برای تمام Elementها است و توقف Iteration مطرح نیست، forEach() می‌تواند Intent را به‌خوبی بیان کند.

در نتیجه، مدل ذهنی مهم این فصل چنین است:

Iteration فقط تکرار کردن نیست؛ انتخاب روشی است که میزان کنترل مورد نیاز و Intent واقعی کد را به‌درستی بیان کند.

در فصل بعد، همین مفهوم Callback به سطح بالاتری منتقل می‌شود. به‌جای اینکه فقط برای هر Element یک عملیات اجرا کنیم، خواهیم دید چگونه Array را با map()، filter() و reduce() به‌صورت Declarative پردازش کنیم.