Chapter 35 — map, filter and reduce
Chapter Goal

خواننده بتواند داده‌های یک Array را با استفاده از map()، filter() و reduce() به‌صورت Declarative پردازش کند و تفاوت میان Transformation، Selection و Accumulation را در مسائل واقعی تشخیص دهد.

Core Question

چگونه داده‌های Array را به‌صورت Declarative تبدیل، فیلتر و خلاصه کنیم؟

مقدمه

در فصل قبل دیدیم که Iteration چگونه امکان پیمایش عناصر یک Array را فراهم می‌کند.

فرض کنید داده‌های یک فروشگاه را در اختیار داریم:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

ممکن است بخواهیم فقط نام محصولات را استخراج کنیم.

یا فقط محصولاتی را انتخاب کنیم که قیمت آنها بیشتر از 700 است.

یا مجموع قیمت تمام محصولات را محاسبه کنیم.

هر سه مسئله روی یک Array انجام می‌شوند، اما هدف آنها یکسان نیست.

در مسئله اول، می‌خواهیم داده را به شکل دیگری تبدیل کنیم.

در مسئله دوم، می‌خواهیم بخشی از داده را انتخاب کنیم.

در مسئله سوم، می‌خواهیم چند مقدار را به یک نتیجه نهایی تبدیل کنیم.

اگر برای همه این مسائل از یک Loop معمولی استفاده کنیم، باید خودمان State و منطق پردازش را مدیریت کنیم.

اما JavaScript برای این نوع پردازش سه Array Method مهم در اختیار ما قرار می‌دهد:

map() برای Transformation
filter() برای Selection
reduce() برای Accumulation

تفاوت این سه Method، در واقع تفاوت میان سه نوع مسئله است.

از Iteration به Data Processing

در فصل قبل، هدف اصلی Iteration این بود که روی عناصر Array حرکت کنیم و برای هر Element عملی انجام دهیم.

برای مثال:

const products = ['Laptop', 'Phone', 'Tablet'];

products.forEach(product => {
console.log(product);
});

اما در بسیاری از مسائل، فقط اجرای یک عملیات کافی نیست.

ممکن است نتیجه عملیات برای ما مهم باشد.

فرض کنید می‌خواهیم از Array محصولات، Array دیگری بسازیم که فقط نام محصولات را نگهداری کند.

در اینجا دیگر صرفاً نمی‌خواهیم روی Array حرکت کنیم.

می‌خواهیم:

از یک Array، Array دیگری با داده‌ای تبدیل‌شده بسازیم.

این تفاوت، نقطه شروع map() است.

تبدیل داده با map()

map() زمانی استفاده می‌شود که بخواهیم هر Element یک Array را به یک Value جدید تبدیل کنیم و نتیجه را در قالب یک Array جدید دریافت کنیم.

برای مثال:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const names = products.map(product => product.name);

console.log(names);
// ['Laptop', 'Phone', 'Tablet']

در اینجا Array اصلی همچنان شامل Objectهای محصول است.

اما map() برای هر Product یک Value جدید تولید کرده است.

بنابراین:

Input Array
↓
Transformation
↓
Output Array

سه Element ورودی داریم و سه Element خروجی دریافت می‌کنیم.

این ویژگی یکی از مهم‌ترین مشخصه‌های map() است:

map() ساختار یک Array را حفظ می‌کند، اما Valueهای آن را به شکل دیگری تبدیل می‌کند.

Callback در map()

همان‌طور که در فصل قبل دیدیم، Array Methodهایی مانند forEach() می‌توانند یک Callback Function دریافت کنند.

map() نیز همین مدل را دنبال می‌کند.

const prices = [100, 200, 300];

const discountedPrices = prices.map(price => price * 0.9);

console.log(discountedPrices);
// [90, 180, 270]

Callback مشخص می‌کند:

هر Element چگونه باید تبدیل شود؟

در این مثال، هر price در 0.9 ضرب می‌شود.

خود map() مسئول پیمایش Array است.

Callback نیز مسئول تعیین Transformation است.

این جداسازی باعث می‌شود Intent کد بسیار واضح باشد:

map
↓
برای هر Element
↓
این Transformation را انجام بده
↓
نتیجه‌ها را در یک Array جدید قرار بده
Return Value در map()

یکی از مهم‌ترین نکات map() این است که Callback باید Valueای را برگرداند که قرار است در Array جدید قرار گیرد.

برای مثال:

const prices = [100, 200, 300];

const result = prices.map(price => {
return price * 2;
});

console.log(result);
// [200, 400, 600]

اگر Callback چیزی را return نکند:

const result = prices.map(price => {
price * 2;
});

نتیجه شامل undefined خواهد بود:

console.log(result);
// [undefined, undefined, undefined]

زیرا map() نمی‌داند Transformation مورد نظر چیست.

این Method فقط نتیجه‌ای را که Callback برمی‌گرداند در Array جدید قرار می‌دهد.

بنابراین می‌توان گفت:

در map()، return همان چیزی است که مشخص می‌کند هر Element ورودی به چه Elementی در خروجی تبدیل شود.

map() و اندازه Array

یکی از ویژگی‌های مهم map() این است که Array خروجی معمولاً همان تعداد Elementهای Array ورودی را دارد.

اگر:

const prices = [100, 200, 300];

را به map() بدهیم، نتیجه نیز سه Element خواهد داشت:

const doubled = prices.map(price => price * 2);

// [200, 400, 600]

به همین دلیل map() ابزار مناسبی برای تبدیل یک‌به‌یک Elementها است.

اگر هدف ما حذف بعضی Elementها باشد، map() ابزار مناسبی نیست.

این سؤال ما را به مفهوم بعدی می‌رساند.

وقتی نمی‌خواهیم همه Elementها را نگه داریم

فرض کنید می‌خواهیم از Array محصولات فقط محصولاتی را انتخاب کنیم که قیمت آنها بیشتر از 700 باشد.

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

اگر از map() استفاده کنیم، برای هر محصول باید یک نتیجه تولید کنیم.

اما مسئله ما چنین نیست.

ما نمی‌خواهیم هر Product را به Product دیگری تبدیل کنیم.

می‌خواهیم بعضی Productها را نگه داریم و بعضی دیگر را کنار بگذاریم.

این همان مسئله‌ای است که filter() برای آن طراحی شده است.

انتخاب داده با filter()

filter() روی عناصر Array حرکت می‌کند و فقط Elementهایی را در Array جدید قرار می‌دهد که شرط مشخص‌شده را قبول کنند.

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const expensiveProducts = products.filter(product => {
return product.price > 700;
});

console.log(expensiveProducts);

نتیجه شامل Laptop و Phone خواهد بود.

در اینجا Callback به‌جای تولید یک Value جدید، در واقع به یک سؤال پاسخ می‌دهد:

آیا این Element باید در نتیجه باقی بماند؟

اگر نتیجه true باشد، Element وارد Array جدید می‌شود.

اگر نتیجه false باشد، Element کنار گذاشته می‌شود.

بنابراین مدل ذهنی filter() چنین است:

Input Array
↓
Condition
↓
Keep / Reject
↓
Output Array
Predicate چیست؟

Functionای که در filter() برای تصمیم‌گیری استفاده می‌شود، یک Predicate Function است.

Predicate Function تابعی است که برای یک ورودی، یک نتیجه منطقی تولید می‌کند تا مشخص شود آیا آن Value شرایط مورد نظر را دارد یا خیر.

برای مثال:

const isExpensive = product => product.price > 700;

اکنون می‌توانیم آن را به filter() بدهیم:

const expensiveProducts = products.filter(isExpensive);

این ساختار Intent کد را روشن‌تر می‌کند:

محصولاتی را فیلتر کن که isExpensive باشند.

در اینجا filter() مسئول Iteration است و Predicate مسئول تصمیم‌گیری.

Return Value در filter()

در filter() مقدار بازگشتی Callback باید به‌عنوان یک شرط ارزیابی شود.

const numbers = [10, 20, 30, 40];

const largeNumbers = numbers.filter(number => number > 20);

console.log(largeNumbers);
// [30, 40]

در این مثال:

10 → false → حذف
20 → false → حذف
30 → true  → نگه‌داری
40 → true  → نگه‌داری

نکته مهم این است که filter() خود Array اصلی را تغییر نمی‌دهد.

نتیجه یک Array جدید است.

بنابراین برخلاف splice() که در فصل قبل دیدیم، filter() برای Selection بدون Mutation مناسب است.

تفاوت اصلی map() و filter()

این دو Method هر دو Array جدید تولید می‌کنند، اما مسئله‌ای که حل می‌کنند متفاوت است.

map() می‌پرسد:

هر Element به چه چیزی تبدیل شود؟

filter() می‌پرسد:

کدام Elementها باید باقی بمانند؟

برای مثال:

const prices = [100, 200, 300];

با map() می‌توانیم قیمت‌ها را دو برابر کنیم:

const doubled = prices.map(price => price * 2);

نتیجه:

[200, 400, 600]

اما با filter() می‌توانیم فقط قیمت‌های بیشتر از 150 را نگه داریم:

const largePrices = prices.filter(price => price > 150);

نتیجه:

[200, 300]

در اولی Transformation اتفاق افتاده است.

در دومی Selection.

این تفاوت، مهم‌تر از Syntax دو Method است.

وقتی Array دیگر نتیجه نهایی نیست

تا اینجا دو نوع پردازش را می‌شناسیم.

گاهی می‌خواهیم:

Array → Array جدید

و گاهی:

Array → Array انتخاب‌شده

اما مسئله همیشه تولید یک Array نیست.

فرض کنید می‌خواهیم مجموع قیمت محصولات را محاسبه کنیم:

const prices = [1200, 800, 500];

نتیجه مورد نظر یک Array نیست.

ما به یک Value نهایی نیاز داریم:

1200 + 800 + 500 = 2500

اکنون مسئله تغییر می‌کند.

باید روی چند Element حرکت کنیم و نتیجه هر مرحله را در یک مقدار مشترک جمع کنیم.

این مفهوم Accumulation است.

و ابزار اصلی JavaScript برای آن reduce() است.

جمع کردن داده با reduce()

reduce() روی عناصر Array حرکت می‌کند و در هر مرحله یک نتیجه تجمعی را به مرحله بعد منتقل می‌کند.

برای مثال:

const prices = [1200, 800, 500];

const total = prices.reduce((total, price) => {
return total + price;
}, 0);

console.log(total);
// 2500

در اینجا total نقش Accumulator را دارد.

Accumulator مقداری است که نتیجه پردازش تا آن مرحله را نگه می‌دارد.

فرآیند به‌صورت مفهومی چنین است:

شروع
total = 0

1200 → total = 1200
800  → total = 2000
500  → total = 2500

در پایان، reduce() مقدار نهایی Accumulator را برمی‌گرداند.

بنابراین برخلاف map() و filter()، نتیجه reduce() الزاماً Array نیست.

می‌تواند یک Number، String، Object، Array یا هر Value دیگری باشد.

Accumulator و Current Value

برای درک درست reduce() باید دو مفهوم را از هم جدا کنیم:

Accumulator

نتیجه‌ای که تا این مرحله جمع شده است.

Current Value

Element فعلی Array که در مرحله جاری پردازش می‌شود.

در مثال:

const total = prices.reduce((total, price) => {
return total + price;
}, 0);

total همان Accumulator و price همان Current Value است.

هر بار Callback اجرا می‌شود، باید مشخص کند Accumulator جدید چه مقداری باشد.

به همین دلیل return در reduce() اهمیت اساسی دارد.

Accumulator قبلی
+
Current Value
↓
Accumulator جدید

Accumulator جدید وارد مرحله بعد می‌شود.

Initial Value

در مثال قبلی، مقدار اولیه Accumulator را با 0 مشخص کردیم:

prices.reduce((total, price) => {
return total + price;
}, 0);

این مقدار را Initial Value می‌نامیم.

وجود Initial Value باعث می‌شود شروع محاسبه کاملاً مشخص باشد.

برای مثال، اگر هدف محاسبه مجموع باشد، 0 نقطه شروع طبیعی است.

اگر بخواهیم حاصل‌ضرب اعداد را محاسبه کنیم، مقدار اولیه می‌تواند 1 باشد:

const numbers = [2, 3, 4];

const result = numbers.reduce((total, number) => {
return total * number;
}, 1);

console.log(result);
// 24

فرآیند:

1 × 2 = 2
2 × 3 = 6
6 × 4 = 24

Initial Value فقط یک مقدار ابتدایی نیست؛ بخشی از طراحی الگوریتم reduce() است.

چرا Initial Value مهم است؟

ممکن است reduce() بدون Initial Value نیز استفاده شود:

const numbers = [10, 20, 30];

const total = numbers.reduce((sum, number) => {
return sum + number;
});

در این حالت، اولین Element Array به‌عنوان Initial Accumulator در نظر گرفته می‌شود و Iteration از Element دوم ادامه پیدا می‌کند.

این رفتار می‌تواند در برخی موارد درست باشد، اما یک مشکل مهم ایجاد می‌کند:

اگر Array خالی باشد:

const numbers = [];

numbers.reduce((sum, number) => sum + number);

reduce() نمی‌تواند Accumulator اولیه‌ای پیدا کند و یک TypeError ایجاد می‌شود.

در مقابل، اگر Initial Value مشخص کنیم:

const numbers = [];

const total = numbers.reduce((sum, number) => {
return sum + number;
}, 0);

console.log(total);
// 0

رفتار برای Array خالی نیز مشخص است.

به همین دلیل در بسیاری از کاربردهای واقعی، تعیین Initial Value انتخاب روشن‌تر و ایمن‌تری است؛ به‌خصوص زمانی که برای مسئله مقدار شروع طبیعی داریم.

reduce() فقط برای جمع کردن نیست

نام reduce() ممکن است این تصور را ایجاد کند که این Method فقط برای محاسبه مجموع اعداد استفاده می‌شود.

اما مفهوم اصلی آن Accumulation است.

برای مثال می‌توانیم تعداد محصولات را محاسبه کنیم:

const products = [
{ name: 'Laptop', stock: 4 },
{ name: 'Phone', stock: 8 },
{ name: 'Tablet', stock: 3 }
];

const totalStock = products.reduce((total, product) => {
return total + product.stock;
}, 0);

در اینجا Accumulator یک Number است.

اما می‌توانیم Accumulator را یک Object نیز قرار دهیم.

برای مثال، می‌توانیم محصولات را بر اساس دسته‌بندی جمع‌آوری کنیم:

const products = [
{ name: 'Laptop', category: 'computer' },
{ name: 'Phone', category: 'mobile' },
{ name: 'Tablet', category: 'mobile' }
];

const grouped = products.reduce((result, product) => {
const category = product.category;

if (!result[category]) {
result[category] = [];
}

result[category].push(product);

return result;
}, {});

در اینجا نتیجه نهایی یک Object است.

بنابراین:

reduce() درباره نوع خاصی از خروجی نیست؛ درباره تبدیل چند مرحله از پردازش به یک Accumulated Result است.

تفاوت reduce() و map()

ممکن است هر دو Method روی تمام عناصر Array حرکت کنند، اما هدف آنها متفاوت است.

map() برای Transformation یک‌به‌یک طراحی شده است.

اگر پنج Element داشته باشیم، انتظار داریم پنج نتیجه داشته باشیم.

5 Elements
↓ map
5 Elements

اما reduce() می‌تواند چند Element را در یک نتیجه نهایی جمع کند:

5 Elements
↓ reduce
1 Result

این نتیجه می‌تواند Number، Object، Array یا Value دیگری باشد.

بنابراین اگر مسئله این است:

«هر Element را به چه چیزی تبدیل کنم؟»

map() را در نظر بگیرید.

اگر مسئله این است:

«چگونه چند Element را در یک نتیجه نهایی جمع کنم؟»

reduce() مناسب‌تر است.

ترکیب filter() و map()

در مسائل واقعی، معمولاً یک عملیات تنها کافی نیست.

فرض کنید می‌خواهیم نام محصولاتی را به دست آوریم که قیمت آنها بیشتر از 700 است.

ابتدا باید محصولات مناسب را انتخاب کنیم.

سپس نام آنها را استخراج کنیم.

این دو مرحله را می‌توان با filter() و map() ترکیب کرد:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const names = products
.filter(product => product.price > 700)
.map(product => product.name);

console.log(names);
// ['Laptop', 'Phone']

در اینجا عملیات به دو مرحله تقسیم شده است:

Products
↓
filter
↓
Expensive Products
↓
map
↓
Product Names

این ترتیب اهمیت دارد.

ابتدا Selection انجام می‌شود.

سپس Transformation.

اگر ترتیب را تغییر دهیم:

products
.map(product => product.name)
.filter(name => ...);

دیگر به Object محصول و price دسترسی نداریم؛ زیرا map() داده را به نام تبدیل کرده است.

بنابراین در زنجیره کردن Methodها باید به شکل داده‌ای که از هر مرحله خارج می‌شود توجه کنیم.

ترکیب filter() و reduce()

گاهی فقط Elementهای خاصی باید در محاسبه نهایی شرکت کنند.

فرض کنید می‌خواهیم مجموع قیمت محصولاتی را محاسبه کنیم که قیمت آنها بیشتر از 700 است.

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const total = products
.filter(product => product.price > 700)
.reduce((sum, product) => sum + product.price, 0);

console.log(total);
// 2000

در اینجا ابتدا Selection انجام می‌شود:

1200 → نگه‌داری
800  → نگه‌داری
500  → حذف

سپس Accumulation انجام می‌شود:

0 + 1200 + 800 = 2000

این مثال نشان می‌دهد که Array Methods زمانی قدرت واقعی خود را نشان می‌دهند که بتوانیم آنها را بر اساس مراحل منطقی پردازش داده ترکیب کنیم.

Declarative Data Processing

تا اینجا سه Method اصلی این فصل را بررسی کردیم.

اما یک مفهوم مهم پشت همه آنها قرار دارد: Declarative Programming.

در روش Imperative معمولاً مراحل انجام کار را خودمان به‌صورت جزئی مشخص می‌کنیم:

const names = [];

for (let i = 0; i < products.length; i++) {
names.push(products[i].name);
}

در اینجا خودمان مدیریت می‌کنیم:

Index
شرط Loop
دسترسی به Element
ایجاد Array نتیجه
اضافه کردن نتیجه

اما با map() می‌توانیم Intent را مستقیم‌تر بیان کنیم:

const names = products.map(product => product.name);

در اینجا دیگر درباره نحوه پیمایش صحبت نمی‌کنیم.

می‌گوییم:

از هر Product، Name را به دست بیاور.

این همان تفاوت مهم میان How و What است.

در سبک Declarative، تمرکز بیشتر روی چیزی است که می‌خواهیم به دست آوریم، نه جزئیات اجرای آن.

map()، filter() و reduce() این مدل فکر کردن را در پردازش Arrayها تقویت می‌کنند.

انتخاب درست بین سه Method

اکنون می‌توانیم سه مسئله اصلی را از یکدیگر جدا کنیم.

اگر می‌خواهیم هر Element را تبدیل کنیم

از map() استفاده می‌کنیم.

const names = products.map(product => product.name);
اگر می‌خواهیم فقط بعضی Elementها باقی بمانند

از filter() استفاده می‌کنیم.

const expensive = products.filter(product => product.price > 700);
اگر می‌خواهیم چند Element را به یک نتیجه نهایی تبدیل کنیم

از reduce() استفاده می‌کنیم.

const total = products.reduce(
(sum, product) => sum + product.price,
0
);

مدل ذهنی فصل را می‌توان چنین خلاصه کرد:

Array
│
├── Transformation → map()
│
├── Selection      → filter()
│
└── Accumulation   → reduce()

این سه مفهوم پایه استفاده صحیح از این Methodها هستند.

Best Practices
1. Method را بر اساس نوع مسئله انتخاب کنید

قبل از نوشتن کد مشخص کنید مسئله شما:

Transformation است؟
Selection است؟
یا Accumulation؟

سپس Method مناسب را انتخاب کنید.

2. از map() برای Transformation استفاده کنید

اگر برای هر Element یک Value جدید تولید می‌کنید، map() معمولاً Intent مناسب‌تری نسبت به forEach() دارد.

3. از filter() برای Selection استفاده کنید

اگر هدف نگه‌داشتن Elementهایی است که شرط خاصی را دارند، filter() را انتخاب کنید.

4. برای reduce() Initial Value را آگاهانه انتخاب کنید

Initial Value بخشی از منطق Accumulation است.

مقدار مناسب باید بر اساس نوع نتیجه و منطق مسئله انتخاب شود.

5. Callbackها را ساده و متمرکز نگه دارید

Callback باید تا حد امکان فقط مسئول همان Transformation، Selection یا Accumulation مورد نظر باشد.

اگر منطق بیش از حد پیچیده شود، خوانایی زنجیره کاهش پیدا می‌کند.

6. قبل از Chain کردن، خروجی هر مرحله را تصور کنید

در:

products
.filter(...)
.map(...)

خروجی filter() ورودی map() است.

بنابراین باید بدانیم هر مرحله چه داده‌ای تولید می‌کند.

7. Declarative بودن را با کوتاه بودن اشتباه نگیرید

هدف map()، filter() و reduce() صرفاً کاهش تعداد خطوط کد نیست.

هدف اصلی، بیان واضح‌تر Intent پردازش داده است.

Common Mistakes
اشتباه اول: استفاده از forEach() برای ساخت Array جدید

اگر هدف ساخت یک Array جدید از نتیجه Transformation است، map() ابزار مناسب‌تری است.

const names = products.map(product => product.name);

forEach() برای اجرای Callback روی عناصر طراحی شده و خودش Array حاصل از Return Valueها تولید نمی‌کند.

اشتباه دوم: فراموش کردن return در map()
const names = products.map(product => {
product.name;
});

در این حالت Callback چیزی Return نمی‌کند و نتیجه شامل undefined خواهد بود.

باید بنویسیم:

const names = products.map(product => {
return product.name;
});
اشتباه سوم: استفاده از map() برای حذف Elementها

map() برای Transformation است و تعداد عناصر خروجی را به‌طور معمول با ورودی حفظ می‌کند.

اگر هدف حذف Elementها بر اساس شرط است، filter() انتخاب مناسب‌تری است.

اشتباه چهارم: فراموش کردن return در reduce()

Accumulator جدید باید از Callback برگردانده شود:

const total = prices.reduce((sum, price) => {
return sum + price;
}, 0);

اگر return فراموش شود، مقدار Accumulator در مرحله بعد آن چیزی نخواهد بود که انتظار داریم.

اشتباه پنجم: نداشتن Initial Value بدون توجه به Array خالی

استفاده از reduce() بدون Initial Value روی Array خالی باعث TypeError می‌شود.

اگر مسئله مقدار شروع مشخصی دارد، بهتر است آن را صریحاً ارائه کنیم.

اشتباه ششم: اشتباه گرفتن Accumulator و Current Value

در:

numbers.reduce((sum, number) => {
return sum + number;
}, 0);

sum نتیجه تجمعی تا این مرحله است، در حالی که number Element فعلی است.

این دو نقش یکسان نیستند.

اشتباه هفتم: Chain کردن بدون توجه به نوع داده

در:

products
.filter(...)
.map(...)

خروجی filter() ورودی map() است.

اگر filter() یا map() شکل داده را تغییر دهد، باید بررسی کنیم مرحله بعد با آن داده سازگار باشد.

اشتباه هشتم: استفاده از reduce() برای هر مسئله‌ای

reduce() بسیار قدرتمند است، اما قدرت بیشتر همیشه به معنی انتخاب بهتر نیست.

اگر مسئله ساده‌ای با map() یا filter() به‌وضوح بیان می‌شود، استفاده از reduce() معمولاً فقط پیچیدگی غیرضروری ایجاد می‌کند.

Summary

map()، filter() و reduce() سه Array Method مهم برای پردازش Declarative داده‌ها هستند.

map() برای Transformation استفاده می‌شود. این Method روی عناصر Array حرکت می‌کند و از Return Value Callback یک Array جدید می‌سازد.

filter() برای Selection استفاده می‌شود. Callback آن مشخص می‌کند کدام Elementها باید در Array جدید باقی بمانند.

reduce() برای Accumulation استفاده می‌شود. این Method نتیجه هر مرحله را در یک Accumulator نگه می‌دارد و در پایان یک نتیجه نهایی برمی‌گرداند.

map() معمولاً تعداد عناصر را حفظ می‌کند.

filter() می‌تواند تعداد عناصر را کاهش دهد.

reduce() می‌تواند کل Array را به یک Value نهایی تبدیل کند.

در reduce()، تفاوت میان Accumulator، Current Value و Initial Value برای درک رفتار Method ضروری است.

این Methodها را می‌توان برای پردازش‌های چندمرحله‌ای با یکدیگر ترکیب کرد. در این حالت باید به ترتیب عملیات و شکل داده‌ای که از هر مرحله خارج می‌شود توجه داشته باشیم.

در نهایت، ارزش اصلی این Methodها فقط در کوتاه‌تر شدن کد نیست. آنها به ما اجازه می‌دهند Intent پردازش داده را مستقیم‌تر بیان کنیم.

Key Takeaways
map() برای Transformation است.
filter() برای Selection است.
reduce() برای Accumulation است.
map() یک Array جدید ایجاد می‌کند.
filter() یک Array جدید از Elementهای پذیرفته‌شده ایجاد می‌کند.
reduce() الزاماً Array برنمی‌گرداند.
Return Value در map() مشخص می‌کند Element جدید چه باشد.
Callback در filter() مشخص می‌کند Element نگه داشته شود یا نه.
reduce() با استفاده از Accumulator نتیجه را مرحله‌به‌مرحله تولید می‌کند.
Initial Value نقطه شروع Accumulation را مشخص می‌کند.
در reduce()، Accumulator و Current Value دو نقش متفاوت دارند.
map() برای حذف Elementها طراحی نشده است.
filter() برای Transformation یک‌به‌یک طراحی نشده است.
reduce() ابزار عمومی‌تری است، اما نباید برای مسائل ساده بیش از حد استفاده شود.
ترکیب Methodها باید بر اساس جریان واقعی داده انجام شود.
Declarative Code بیشتر روی What تمرکز می‌کند تا جزئیات How.
Technical Interview
Junior
1. map() چه کاری انجام می‌دهد؟

map() روی عناصر Array Iteration انجام می‌دهد، Callback را برای هر Element اجرا می‌کند و Return Value هر Callback را در یک Array جدید قرار می‌دهد.

2. تفاوت map() و filter() چیست؟

map() هر Element را به یک نتیجه جدید تبدیل می‌کند، در حالی که filter() تصمیم می‌گیرد کدام Elementها باید در Array خروجی باقی بمانند.

3. filter() چه چیزی از Callback انتظار دارد؟

Callback باید نتیجه‌ای ارائه کند که به‌عنوان شرط ارزیابی شود. Elementهایی که شرط آنها true باشد در Array خروجی قرار می‌گیرند.

4. reduce() چه کاری انجام می‌دهد؟

reduce() عناصر Array را به‌صورت مرحله‌ای پردازش می‌کند و آنها را در یک Accumulator جمع می‌کند تا در نهایت یک نتیجه نهایی تولید شود.

5. map() و filter() آیا Array اصلی را تغییر می‌دهند؟

خیر. هر دو Array جدید تولید می‌کنند و Array اصلی را Mutation نمی‌کنند.

Mid-Level
6. چرا map() برای ساخت Array جدید مناسب‌تر از forEach() است؟

زیرا هدف map() دقیقاً Transformation عناصر و تولید یک Array جدید از Return Valueهای Callback است، در حالی که forEach() صرفاً Callback را برای هر Element اجرا می‌کند.

7. Accumulator در reduce() چیست؟

Accumulator مقداری است که نتیجه پردازش تا مرحله فعلی را نگهداری می‌کند و مقدار جدید آن به مرحله بعد منتقل می‌شود.

8. Initial Value در reduce() چه نقشی دارد؟

Initial Value مقدار اولیه Accumulator را مشخص می‌کند و تعیین می‌کند Accumulation از چه Valueای آغاز شود.

9. چرا reduce() بدون Initial Value روی Array خالی مشکل ایجاد می‌کند؟

زیرا در این حالت reduce() باید اولین Element Array را به‌عنوان Initial Accumulator استفاده کند، اما Array خالی Elementی ندارد و در نتیجه TypeError ایجاد می‌شود.

10. چه زمانی filter() و map() را با هم ترکیب می‌کنیم؟

وقتی ابتدا باید مجموعه‌ای از Elementها را بر اساس یک شرط انتخاب کنیم و سپس Elementهای انتخاب‌شده را به شکل دیگری تبدیل کنیم.

Senior
11. تفاوت مفهومی map()، filter() و reduce() چیست؟

map() یک Transformation یک‌به‌یک انجام می‌دهد، filter() Selection انجام می‌دهد و reduce() چند Element را در یک Accumulated Result خلاصه می‌کند.

12. چرا map() و filter() را می‌توان Declarative دانست؟

زیرا به‌جای مدیریت مستقیم جزئیات Iteration، Intent پردازش را بیان می‌کنند؛ map() مشخص می‌کند هر Element چگونه تبدیل شود و filter() مشخص می‌کند چه Elementهایی باید باقی بمانند.

13. چرا reduce() یک Method عمومی‌تر از map() و filter() محسوب می‌شود؟

زیرا نوع نتیجه آن محدود به Array نیست و می‌تواند با Accumulator مناسب، Number، String، Object، Array یا Value دیگری تولید کند.

14. مهم‌ترین نکته در طراحی یک reduce() چیست؟

باید نقش Accumulator، Current Value و Initial Value به‌صورت دقیق مشخص باشد و هر مرحله مقدار جدید Accumulator را به مرحله بعد برگرداند.

15. در یک Chain مانند filter().map().reduce() چه نکته‌ای اهمیت دارد؟

خروجی هر مرحله ورودی مرحله بعد است. بنابراین باید هم ترتیب عملیات و هم شکل داده‌ای که هر Method تولید می‌کند با نیاز مرحله بعد سازگار باشد.

16. آیا استفاده از reduce() همیشه انتخاب حرفه‌ای‌تری نسبت به map() یا filter() است؟

خیر. حرفه‌ای بودن به انتخاب ابزار متناسب با مسئله بستگی دارد. اگر مسئله Transformation یا Selection است، map() یا filter() معمولاً Intent را واضح‌تر از reduce() بیان می‌کنند.

Golden Answers
map() چیست؟

map() یک Array Method برای Transformation است که Callback را برای هر Element اجرا می‌کند و Return Valueهای Callback را در یک Array جدید قرار می‌دهد.

filter() چیست؟

filter() یک Array Method برای Selection است که فقط Elementهایی را در یک Array جدید قرار می‌دهد که Predicate آنها نتیجه‌ای true داشته باشد.

reduce() چیست؟

reduce() یک Array Method برای Accumulation است که نتیجه پردازش را در یک Accumulator نگه می‌دارد و در پایان یک نتیجه نهایی برمی‌گرداند.

تفاوت map() و filter() چیست؟

map() Value هر Element را به Value جدیدی تبدیل می‌کند، اما filter() تصمیم می‌گیرد Element در نتیجه باقی بماند یا حذف شود.

تفاوت map() و reduce() چیست؟

map() معمولاً یک Array را به Array دیگری با همان تعداد Element تبدیل می‌کند، در حالی که reduce() چند Element را در یک نتیجه نهایی جمع می‌کند و نوع خروجی آن الزاماً Array نیست.

Accumulator چیست؟

Accumulator مقداری است که نتیجه تجمعی پردازش تا مرحله فعلی را نگه می‌دارد و در هر مرحله به مقدار جدیدی تبدیل می‌شود.

Initial Value چیست؟

Initial Value مقدار اولیه‌ای است که به‌عنوان نقطه شروع Accumulator در reduce() تعیین می‌شود.

چرا reduce() را نباید برای هر مسئله‌ای استفاده کرد؟

زیرا reduce() انعطاف زیادی دارد، اما اگر مسئله صرفاً Transformation یا Selection باشد، map() یا filter() معمولاً Intent را واضح‌تر و کد را قابل‌فهم‌تر می‌کنند.

Declarative Data Processing چیست؟

یعنی کد بیشتر بر نتیجه یا Intent مورد نظر تمرکز کند و جزئیات اجرای Iteration را به ابزار مورد استفاده واگذار کند. map()، filter() و reduce() نمونه‌های مهمی از این رویکرد در پردازش Array هستند.

Conclusion

در فصل قبل یاد گرفتیم چگونه روی عناصر Array حرکت کنیم.

اما Iteration به‌تنهایی هدف نهایی نیست.

در Applicationهای واقعی، معمولاً از Iteration برای پردازش داده استفاده می‌کنیم.

گاهی می‌خواهیم هر Element را به شکل دیگری تبدیل کنیم.

گاهی فقط بخشی از Elementها برای ما اهمیت دارند.

و گاهی لازم است چندین Value را در یک نتیجه نهایی خلاصه کنیم.

سه Array Method این فصل دقیقاً این سه نیاز را از یکدیگر جدا می‌کنند.

map() برای Transformation است.

filter() برای Selection است.

reduce() برای Accumulation است.

وقتی این سه مفهوم را به‌درستی از هم تشخیص دهیم، دیگر انتخاب Method بر اساس حفظ کردن Syntax انجام نمی‌شود.

ابتدا مسئله را می‌بینیم.

سپس می‌پرسیم:

آیا می‌خواهم داده را تبدیل کنم؟

اگر بله، map().

آیا می‌خواهم فقط داده‌های مناسب را انتخاب کنم؟

اگر بله، filter().

آیا می‌خواهم چند Value را در یک نتیجه تجمیع کنم؟

اگر بله، reduce().

و زمانی که مسئله چند مرحله دارد، می‌توان این عملیات را در یک Data Processing Pipeline ترکیب کرد؛ به شرط آنکه ترتیب مراحل و شکل داده‌ها را به‌درستی در نظر بگیریم.

در نهایت، ارزش اصلی این Methodها در این است که به ما کمک می‌کنند به‌جای تمرکز بر جزئیات پیمایش، منطق پردازش داده را به زبان خود مسئله بیان کنیم.

این همان نقطه‌ای است که Iteration ساده، به Data Processing حرفه‌ای تبدیل می‌شود.