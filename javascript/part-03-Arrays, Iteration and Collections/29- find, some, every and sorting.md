

Chapter 36 — find, some, every and sorting
Chapter Goal

خواننده بتواند از Array برای پیدا کردن یک Element، پیدا کردن Index، بررسی وجود یک شرط، اعتبارسنجی تمام عناصر و مرتب‌سازی داده‌ها استفاده کند و تفاوت میان این عملیات را از نظر نتیجه، رفتار Iteration و Mutation تشخیص دهد. این فصل بر Concept Flow تعیین‌شده برای Chapter 36 و اصول Narrative Flow کتاب بنا شده است.

Core Question

چگونه Array را برای جست‌وجو، اعتبارسنجی و مرتب‌سازی حرفه‌ای پردازش کنیم؟

مقدمه

در فصل قبل دیدیم که map()، filter() و reduce() ابزارهای مهمی برای Data Processing هستند.

با map() می‌توانیم هر Element را به یک Value جدید تبدیل کنیم.

با filter() می‌توانیم مجموعه‌ای از عناصر را انتخاب کنیم.

با reduce() می‌توانیم چند Value را به یک نتیجه نهایی تبدیل کنیم.

برای مثال:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

اگر بخواهیم فقط محصولات گران‌تر از 700 را داشته باشیم، filter() انتخاب مناسبی است:

const expensiveProducts = products.filter(
product => product.price > 700
);

اما همیشه هدف ما ساختن یک Array جدید نیست.

گاهی فقط می‌خواهیم یک Element مشخص را پیدا کنیم.

مثلاً:

اولین محصولی که قیمت آن بیشتر از 1000 است کدام است؟

گاهی خود Element برای ما مهم نیست و فقط می‌خواهیم بدانیم:

Index این محصول چیست؟

گاهی حتی Index نیز برای ما مهم نیست.

فقط می‌خواهیم بدانیم:

آیا حداقل یک محصول با این ویژگی وجود دارد؟

و در بعضی مسائل، سؤال برعکس است:

آیا تمام محصولات این شرط را دارند؟

این تفاوت‌ها باعث شده JavaScript چند Array Method تخصصی در اختیار ما قرار دهد:

find()
findIndex()
some()
every()

این Methodها همگی با Callback کار می‌کنند، اما هدف و نتیجه متفاوتی دارند.

در ادامه مسئله دیگری نیز مطرح می‌شود.

فرض کنید محصولات را پیدا کرده‌ایم و فیلتر کرده‌ایم، اما اکنون می‌خواهیم آنها را بر اساس قیمت یا نام مرتب کنیم.

در اینجا مسئله دیگر Search یا Validation نیست.

مسئله:

Order

است.

و این ما را به sort() و مفهوم مهم Comparator می‌رساند.

از Array Processing به Search

در filter() یک سؤال داریم:

کدام عناصر این شرط را دارند؟

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const expensiveProducts = products.filter(
product => product.price > 700
);

نتیجه:

[
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 }
]

filter() تمام عناصر مناسب را جمع می‌کند و یک Array جدید می‌سازد.

اما فرض کنید فقط اولین محصول گران‌تر از 700 برای ما مهم است.

در اینجا ساختن Array جدید غیرضروری است.

ما به یک سؤال کوچک‌تر نیاز داریم:

اولین Element که شرط را برقرار می‌کند کدام است؟

این دقیقاً مسئله‌ای است که find() حل می‌کند.

پیدا کردن یک Element با find()

find() برای پیدا کردن اولین Element در Array استفاده می‌شود که Callback برای آن مقدار true یا یک مقدار Truthy برگرداند.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const product = products.find(
product => product.price > 700
);

console.log(product);

خروجی:

{ name: 'Laptop', price: 1200 }

نکته مهم این است که find() همه عناصر مناسب را برنمی‌گرداند.

فقط اولین Element مطابق شرط را برمی‌گرداند.

پس:

product => product.price > 700

شرط Search است.

و:

find()

اولین نتیجه مطابق آن شرط را پیدا می‌کند.

find() چه چیزی برمی‌گرداند؟

اگر Element مناسب پیدا شود، خود Element برگردانده می‌شود.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const product = products.find(
product => product.name === 'Phone'
);

console.log(product.name);
console.log(product.price);

خروجی:

Phone
800

بنابراین مدل ذهنی find() این است:

Array
↓
Check Elements
↓
First Match
↓
Return Element
find() فقط اولین Match را پیدا می‌کند

فرض کنید چند محصول شرایط یکسانی دارند:

const products = [
{ name: 'Laptop', category: 'computer' },
{ name: 'Phone', category: 'mobile' },
{ name: 'Tablet', category: 'mobile' }
];

اگر بنویسیم:

const product = products.find(
product => product.category === 'mobile'
);

خروجی:

{ name: 'Phone', category: 'mobile' }

Tablet نیز شرط را دارد، اما برگردانده نمی‌شود.

چرا؟

زیرا find() به دنبال:

First Matching Element

است.

برای دریافت تمام عناصر Matching باید از filter() استفاده کنیم.

بنابراین تفاوت اصلی:

find()
↓
First Match
↓
One Element

در مقابل:

filter()
↓
All Matches
↓
New Array

این تفاوت در طراحی Logic بسیار مهم است.

find() اجرای خود را زود متوقف می‌کند

یکی از ویژگی‌های مهم find() این است که وقتی اولین Element مطابق شرط پیدا شد، دیگر لازم نیست عناصر بعدی بررسی شوند.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const product = products.find(product => {
console.log(product.name);
return product.price > 700;
});

خروجی:

Laptop

Callback فقط برای Laptop اجرا شده است.

چون شرط برای اولین Element برقرار شده است.

این رفتار را می‌توان به‌صورت زیر تصور کرد:

Laptop
↓
condition → true
↓
return Laptop
↓
STOP

بنابراین find() علاوه بر اینکه نتیجه متفاوتی نسبت به filter() دارد، از نظر کنترل Iteration نیز متفاوت است.

اگر چیزی پیدا نشود چه می‌شود؟

ممکن است هیچ Elementی شرط را برقرار نکند:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const product = products.find(
product => product.price > 2000
);

console.log(product);

خروجی:

undefined

پس مدل کامل find():

Match found
↓
Element

No match
↓
undefined

این موضوع هنگام استفاده از نتیجه مهم است.

مثلاً این کد ممکن است خطا ایجاد کند:

const product = products.find(
product => product.price > 2000
);

console.log(product.name);

زیرا اگر محصولی پیدا نشود:

product === undefined

و undefined دارای Property به نام name نیست.

در چنین شرایطی باید نتیجه Search را در نظر بگیریم.

مثلاً:

const product = products.find(
product => product.price > 2000
);

if (product) {
console.log(product.name);
}

یا در شرایط مناسب می‌توان از Optional Chaining استفاده کرد:

console.log(product?.name);
وقتی Element می‌خواهیم، find()؛ وقتی Index می‌خواهیم، findIndex()

گاهی خود Object برای ما مهم نیست.

فرض کنید:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

می‌خواهیم بدانیم:

محصول Phone در چه Indexای قرار دارد؟

find() در اینجا نتیجه‌ای بیش از نیاز ما برمی‌گرداند.

چیزی که نیاز داریم:

Index

است.

برای این مسئله JavaScript Method دیگری دارد:

findIndex()
پیدا کردن Index با findIndex()

findIndex() مانند find() روی عناصر حرکت می‌کند و اولین Elementی را پیدا می‌کند که شرط را برقرار کند.

اما به‌جای Element، Index آن Element را برمی‌گرداند.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const index = products.findIndex(
product => product.name === 'Phone'
);

console.log(index);

خروجی:

1

زیرا:

Laptop → 0
Phone  → 1
Tablet → 2

مدل ذهنی:

find()
↓
Element

findIndex()
↓
Index
اگر findIndex() چیزی پیدا نکند

اگر هیچ Elementی شرط را برقرار نکند:

const index = products.findIndex(
product => product.name === 'Monitor'
);

console.log(index);

خروجی:

-1

پس:

find()
Match → Element
No Match → undefined

findIndex()
Match → Index
No Match → -1

این تفاوت باید به‌صورت ذهنی کاملاً روشن باشد.

find() و findIndex() چه زمانی استفاده می‌شوند؟

بهتر است Method را بر اساس نوع نتیجه موردنیاز انتخاب کنیم.

اگر می‌خواهیم با خود Object کار کنیم:

const product = products.find(
product => product.name === 'Phone'
);

product.price = 900;

find() انتخاب طبیعی است.

اما اگر به Index نیاز داریم:

const index = products.findIndex(
product => product.name === 'Phone'
);

findIndex() مناسب‌تر است.

بنابراین سؤال اصلی این نیست که:

کدام Method را حفظ کنم؟

بلکه:

نتیجه‌ای که Logic من نیاز دارد Element است یا Index؟

Search در برابر Selection

اکنون می‌توانیم find() و filter() را دقیق‌تر مقایسه کنیم.

filter()
↓
All matching elements
↓
New Array

در مقابل:

find()
↓
First matching element
↓
Element

و:

findIndex()
↓
First matching element
↓
Index

پس سه سؤال متفاوت داریم:

کدام عناصر؟
→ filter()

اولین Element کدام است؟
→ find()

اولین Element در چه Indexای است؟
→ findIndex()

این تفاوت یکی از مهم‌ترین مدل‌های ذهنی این فصل است.

گاهی فقط یک سؤال Boolean داریم

تا اینجا درباره Search صحبت کردیم.

اما همیشه نمی‌خواهیم چیزی را پیدا کنیم.

فرض کنید:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

سؤال ما این است:

آیا حداقل یک محصول با قیمت بیشتر از 1000 وجود دارد؟

ما به خود Product نیاز نداریم.

Index هم برای ما مهم نیست.

فقط به پاسخ:

true

یا:

false

نیاز داریم.

این مسئله با some() حل می‌شود.

بررسی وجود حداقل یک شرط با some()

some() بررسی می‌کند که آیا حداقل یک Element در Array وجود دارد که Callback برای آن مقدار Truthy برگرداند یا خیر.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const hasExpensiveProduct = products.some(
product => product.price > 1000
);

console.log(hasExpensiveProduct);

خروجی:

true

زیرا حداقل یک محصول قیمت بیشتر از 1000 دارد.

مدل ذهنی:

Array
↓
Does at least one element match?
↓
true / false
some() برای وجود یک وضعیت مناسب است

some() زمانی بسیار مناسب است که سؤال ما با عبارت‌هایی مانند این شروع شود:

آیا حداقل یک ... وجود دارد؟

مثلاً:

const users = [
{ name: 'Omid', role: 'admin' },
{ name: 'Sara', role: 'user' },
{ name: 'Ali', role: 'user' }
];

const hasAdmin = users.some(
user => user.role === 'admin'
);

console.log(hasAdmin);

خروجی:

true

یا:

const hasInactiveUser = users.some(
user => user.active === false
);

در اینجا نیز نتیجه فقط Boolean است.

some() نیز زود متوقف می‌شود

اگر some() اولین Element مناسب را پیدا کند، نتیجه مشخص شده است.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const result = products.some(product => {
console.log(product.name);
return product.price > 1000;
});

خروجی:

Laptop

بعد از پیدا شدن یک Match، ادامه Iteration لازم نیست.

مدل ذهنی:

Check
↓
true?
↓
YES
↓
true
↓
STOP

اگر هیچ Elementی شرط را برقرار نکند، some() پس از بررسی عناصر به:

false

می‌رسد.

تفاوت find() و some()

هر دو می‌توانند یک Search را خیلی زود متوقف کنند.

اما نتیجه آنها متفاوت است.

const product = products.find(
product => product.price > 1000
);

نتیجه:

Product Object

در حالی که:

const hasProduct = products.some(
product => product.price > 1000
);

نتیجه:

true

بنابراین:

find()
"Which element?"

some()
"Does at least one exist?"

این تفاوت کوچک در ظاهر، در طراحی Logic بسیار مهم است.

از «حداقل یک» به «همه»

اکنون یک سؤال دیگر داریم.

فرض کنید می‌خواهیم بررسی کنیم:

آیا تمام محصولات قیمت بیشتر از 300 دارند؟

این سؤال با some() حل نمی‌شود.

some() می‌پرسد:

آیا حداقل یکی؟

اما ما می‌پرسیم:

آیا همه؟

برای این مسئله از:

every()

استفاده می‌کنیم.

بررسی تمام عناصر با every()

every() بررسی می‌کند که آیا تمام عناصر Array شرط Callback را برقرار می‌کنند یا خیر.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const allProductsAreExpensive = products.every(
product => product.price > 300
);

console.log(allProductsAreExpensive);

خروجی:

true

زیرا تمام محصولات قیمت بیشتر از 300 دارند.

اما:

const allProductsAreExpensive = products.every(
product => product.price > 700
);

console.log(allProductsAreExpensive);

خروجی:

false

زیرا Tablet قیمت 500 دارد.

مدل ذهنی every()

برای every() سؤال این است:

Do all elements match?

و نتیجه:

All match
↓
true

At least one fails
↓
false

این نکته مهم است:

every() لازم نیست تمام عناصر را بررسی کند.

اگر یک Element شرط را نقض کند، نتیجه از همان لحظه مشخص است.

مثلاً:

const result = products.every(product => {
console.log(product.name);
return product.price > 700;
});

خروجی:

Laptop
Phone
Tablet

وقتی Tablet بررسی می‌شود:

500 > 700

برابر با:

false

است.

پس every() می‌تواند همان‌جا متوقف شود.

some() و every() دو منطق مکمل هستند

می‌توانیم آنها را کنار هم قرار دهیم:

some()
حداقل یک عنصر شرط را دارد؟

در مقابل:

every()
همه عناصر شرط را دارند؟

مثلاً:

const scores = [80, 90, 75, 92];

حداقل یک نمره بالاتر از 90:

const hasHighScore = scores.some(
score => score > 90
);

تمام نمره‌ها بالاتر از 70:

const allPassed = scores.every(
score => score > 70
);

این دو Method برای Validation بسیار کاربردی هستند.

Validation با some() و every()

فرض کنید یک فرم ثبت سفارش داریم و سفارش شامل چند محصول است:

const cart = [
{ name: 'Laptop', stock: 5 },
{ name: 'Phone', stock: 0 },
{ name: 'Tablet', stock: 3 }
];

می‌خواهیم بدانیم:

آیا حداقل یک محصول ناموجود داریم؟

const hasUnavailableProduct = cart.some(
product => product.stock === 0
);

console.log(hasUnavailableProduct);

خروجی:

true

و اگر بخواهیم بدانیم:

آیا تمام محصولات قابل خرید هستند؟

می‌توانیم بنویسیم:

const allProductsAvailable = cart.every(
product => product.stock > 0
);

console.log(allProductsAvailable);

خروجی:

false

در اینجا Methodها مستقیماً مسئله Business Logic را بیان می‌کنند.

به جای نوشتن Loop و مدیریت دستی یک Boolean:

let hasUnavailableProduct = false;

for (const product of cart) {
if (product.stock === 0) {
hasUnavailableProduct = true;
break;
}
}

می‌توانیم مستقیماً هدف Logic را بیان کنیم:

const hasUnavailableProduct = cart.some(
product => product.stock === 0
);

این همان مزیت Data Processing به شکل Declarative است که در فصل قبل آغاز کردیم.

find، findIndex، some و every یک خانواده هستند

اکنون می‌توانیم چهار Method را در یک مدل واحد قرار دهیم.

همه آنها:

روی Array Iteration انجام می‌دهند.
Callback دریافت می‌کنند.
می‌توانند زودتر از پایان Array متوقف شوند.
اما هدف و خروجی متفاوتی دارند.
find()
↓
First matching Element
findIndex()
↓
Index of first matching Element
some()
↓
Does at least one match?
↓
Boolean
every()
↓
Do all elements match?
↓
Boolean

بنابراین انتخاب Method باید بر اساس نوع سؤال انجام شود.

یک مسئله متفاوت: ترتیب عناصر مهم است

تا اینجا درباره این سؤال‌ها صحبت کردیم:

چه عنصری؟
چه Indexای؟
آیا حداقل یکی؟
آیا همه؟

اما فرض کنید Array ما این است:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

اکنون می‌خواهیم محصولات را بر اساس قیمت از کم به زیاد مرتب کنیم.

اینجا دیگر Search نداریم.

می‌خواهیم:

Order عناصر را تغییر دهیم.

این مسئله با sort() حل می‌شود.

مرتب‌سازی با sort()

sort() عناصر یک Array را مرتب می‌کند.

مثلاً:

const names = ['Tablet', 'Laptop', 'Phone'];

names.sort();

console.log(names);

خروجی:

['Laptop', 'Phone', 'Tablet']

در نگاه اول ممکن است تصور کنیم sort() همیشه مانند مرتب‌سازی عددی عمل می‌کند.

اما این تصور درست نیست.

رفتار پیش‌فرض sort() بر اساس String Conversion و ترتیب Lexicographic است.

این موضوع برای Numberها بسیار مهم می‌شود.

مشکل مرتب‌سازی Numberها

فرض کنید:

const prices = [100, 5, 20, 40];

prices.sort();

console.log(prices);

ممکن است انتظار داشته باشیم:

[5, 20, 40, 100]

اما نتیجه چنین نیست.

خروجی:

[100, 20, 40, 5]

چرا؟

زیرا sort() در حالت پیش‌فرض مقادیر را مانند Stringها با یکدیگر مقایسه می‌کند.

به‌صورت مفهومی:

100 → "100"
5   → "5"
20  → "20"
40  → "40"

و ترتیب Lexicographic به این صورت شکل می‌گیرد:

"100"
"20"
"40"
"5"

پس برای Numberها باید روش مقایسه را مشخص کنیم.

اینجاست که مفهوم مهم:

Comparator

وارد می‌شود.

Comparator چیست؟

sort() می‌تواند یک Function دریافت کند که مشخص می‌کند دو Element نسبت به یکدیگر چه ترتیبی دارند.

این Function را Comparator می‌نامیم.

ساختار کلی:

array.sort((a, b) => {
// comparison
});

Comparator باید یک Number برگرداند.

معنای نتیجه:

negative
↓
a قبل از b

positive
↓
a بعد از b

zero
↓
ترتیب نسبی a و b تغییر نمی‌کند

نکته مهم:

Comparator لازم نیست دقیقاً -1، 0 یا 1 برگرداند.

هر مقدار منفی، صفر یا مثبت برای مشخص کردن رابطه کافی است.

مرتب‌سازی عددی صعودی

برای مرتب کردن Numberها از کوچک به بزرگ:

const prices = [100, 5, 20, 40];

prices.sort((a, b) => a - b);

console.log(prices);

خروجی:

[5, 20, 40, 100]

چرا؟

Comparator برای دو مقدار:

a
b

این مقدار را محاسبه می‌کند:

a - b

اگر:

a < b

باشد، نتیجه منفی است.

پس:

a قبل از b

اگر:

a > b

باشد، نتیجه مثبت است.

پس:

a بعد از b

و اگر:

a === b

باشد:

0

برمی‌گردد.

بنابراین:

(a, b) => a - b

یک Comparator مناسب برای مرتب‌سازی عددی صعودی است.

مرتب‌سازی عددی نزولی

برای ترتیب بزرگ به کوچک، کافی است جهت مقایسه را برعکس کنیم:

const prices = [100, 5, 20, 40];

prices.sort((a, b) => b - a);

console.log(prices);

خروجی:

[100, 40, 20, 5]

مدل ذهنی:

a - b
↓
Ascending

b - a
↓
Descending

این الگو یکی از الگوهای بسیار پرکاربرد در JavaScript است.

Comparator روی Objectها

در Applicationهای واقعی معمولاً با Arrayای از Objectها کار می‌کنیم.

مثلاً:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

اگر بخواهیم بر اساس price مرتب کنیم:

products.sort(
(a, b) => a.price - b.price
);

اکنون:

console.log(products);

خروجی:

[
{ name: 'Tablet', price: 500 },
{ name: 'Phone', price: 800 },
{ name: 'Laptop', price: 1200 }
]

Comparator در واقع Property موردنظر را استخراج و مقایسه می‌کند.

مرتب‌سازی Objectها بر اساس String

Comparator فقط برای Numberها نیست.

فرض کنید:

const products = [
{ name: 'Tablet', price: 500 },
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 }
];

برای مرتب‌سازی بر اساس name می‌توانیم بنویسیم:

products.sort(
(a, b) => a.name.localeCompare(b.name)
);

اکنون ترتیب بر اساس مقایسه مناسب Stringها تعیین می‌شود.

localeCompare() برای مقایسه Stringها بر اساس قواعد زبانی و Locale طراحی شده است و در بسیاری از موارد انتخاب مناسب‌تری نسبت به مقایسه دستی با < و > است.

مثلاً:

const products = [
{ name: 'Tablet', price: 500 },
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 }
];

products.sort(
(a, b) => a.name.localeCompare(b.name)
);

console.log(products);

خروجی:

[
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
]
Comparator در واقع یک قرارداد است

بهتر است Comparator را فقط به شکل:

(a, b) => a - b

حفظ نکنیم.

مفهوم اصلی این است:

Comparator به sort() می‌گوید دو Element نسبت به یکدیگر چه ترتیبی دارند.

مثلاً:

(a, b) => a.price - b.price

یعنی:

ترتیب را بر اساس Price تعیین کن.

و:

(a, b) => b.price - a.price

یعنی:

همان Price را به ترتیب معکوس مرتب کن.

پس Comparator در واقع Rule مربوط به Order است.

sort() چه چیزی را تغییر می‌دهد؟

اکنون به بخش بسیار مهم Concept Flow می‌رسیم:

sort
↓
Comparator
↓
Mutation

برخلاف map() و filter() که Array جدید تولید می‌کنند، sort() خود Array را تغییر می‌دهد.

مثلاً:

const prices = [100, 5, 20, 40];

const sortedPrices = prices.sort(
(a, b) => a - b
);

console.log(prices);
console.log(sortedPrices);

خروجی:

[5, 20, 40, 100]
[5, 20, 40, 100]

هر دو به همان Array مرتب‌شده اشاره می‌کنند.

پس:

sortedPrices === prices

برابر است با:

true
Mutation در sort()

این ویژگی sort() بسیار مهم است.

فرض کنید:

const prices = [100, 5, 20, 40];

const sortedPrices = prices.sort(
(a, b) => a - b
);

ممکن است از نام:

sortedPrices

تصور کنیم یک نسخه جدید ساخته شده است.

اما چنین چیزی اتفاق نیفتاده است.

خود:

prices

تغییر کرده است.

مدل ذهنی:

prices
↓
sort()
↓
Same Array
↓
Reordered Elements

بنابراین sort() یک Mutating Method است.

چرا Mutation مهم است؟

فرض کنید یک Array را در چند بخش Application استفاده می‌کنیم:

const prices = [100, 5, 20, 40];

اگر آن را مرتب کنیم:

prices.sort((a, b) => a - b);

هر بخشی از برنامه که به همان Array دسترسی دارد، Array مرتب‌شده را خواهد دید.

این موضوع می‌تواند کاملاً مطلوب باشد یا باعث Bug شود.

مثلاً:

const prices = [100, 5, 20, 40];

const originalPrices = prices;

prices.sort((a, b) => a - b);

console.log(originalPrices);

خروجی:

[5, 20, 40, 100]

نام:

originalPrices

باعث نشده یک Copy ایجاد شود.

هر دو Variable به یک Array اشاره می‌کنند.

این همان مفهوم Reference است که در فصل‌های قبلی بررسی کردیم.

اگر نخواهیم Array اصلی تغییر کند

گاهی مرتب‌سازی لازم است، اما نمی‌خواهیم Array اصلی Mutation شود.

در JavaScript مدرن می‌توان از:

toSorted()

استفاده کرد.

مثلاً:

const prices = [100, 5, 20, 40];

const sortedPrices = prices.toSorted(
(a, b) => a - b
);

console.log(prices);
console.log(sortedPrices);

خروجی:

[100, 5, 20, 40]
[5, 20, 40, 100]

در این حالت:

prices
↓
Original Order

sortedPrices
↓
New Sorted Array

پس تفاوت اصلی:

sort()
↓
Mutates the original Array

و:

toSorted()
↓
Returns a new sorted Array

است.

در این فصل تمرکز اصلی روی sort() است، زیرا مفهوم Mutation یکی از بخش‌های اصلی رفتار این Method است. الگوهای پیشرفته‌تر Immutable Updates در فصل‌های بعدی با جزئیات بیشتری بررسی خواهند شد.

یک مثال کامل از Data Processing

اکنون چند Method این فصل را در یک مسئله واقعی ترکیب کنیم.

فرض کنید یک فروشگاه این محصولات را دارد:

const products = [
{ name: 'Laptop', category: 'computer', price: 1200 },
{ name: 'Phone', category: 'mobile', price: 800 },
{ name: 'Tablet', category: 'mobile', price: 500 },
{ name: 'Monitor', category: 'computer', price: 300 }
];
پیدا کردن یک محصول
const product = products.find(
product => product.name === 'Phone'
);

console.log(product);

خروجی:

{ name: 'Phone', category: 'mobile', price: 800 }
پیدا کردن Index
const index = products.findIndex(
product => product.name === 'Phone'
);

console.log(index);

خروجی:

1
بررسی وجود حداقل یک محصول گران
const hasExpensiveProduct = products.some(
product => product.price > 1000
);

console.log(hasExpensiveProduct);

خروجی:

true
بررسی اینکه همه محصولات قیمت معتبر دارند
const allPricesValid = products.every(
product => product.price > 0
);

console.log(allPricesValid);

خروجی:

true
مرتب‌سازی بر اساس قیمت
const sortedProducts = products.toSorted(
(a, b) => a.price - b.price
);

console.log(sortedProducts);

خروجی:

[
{ name: 'Monitor', category: 'computer', price: 300 },
{ name: 'Tablet', category: 'mobile', price: 500 },
{ name: 'Phone', category: 'mobile', price: 800 },
{ name: 'Laptop', category: 'computer', price: 1200 }
]

این مثال نشان می‌دهد که Array Processing فقط به Transformation محدود نیست.

گاهی:

Search

گاهی:

Validation

و گاهی:

Ordering

مسئله اصلی را تشکیل می‌دهد.

انتخاب Method بر اساس سؤال

اکنون می‌توانیم تمام Methodهای مهم این فصل را در یک مدل ذهنی واحد قرار دهیم:

سؤال	Method	نتیجه
اولین Element مطابق شرط چیست؟	find()	Element / undefined
Index اولین Element مطابق شرط چیست؟	findIndex()	Index / -1
آیا حداقل یک Element مطابق شرط وجود دارد؟	some()	Boolean
آیا تمام عناصر مطابق شرط هستند؟	every()	Boolean
چگونه ترتیب عناصر را تغییر دهیم؟	sort()	همان Array مرتب‌شده

این جدول برای حفظ کردن نیست.

هدف آن نشان دادن رابطه مفهومی Methodها است.

اگر بدانیم سؤال ما چیست، انتخاب Method تقریباً قابل استنتاج است.

تفاوت filter() و find() در یک مسئله واقعی

فرض کنید:

const users = [
{ name: 'Omid', role: 'admin' },
{ name: 'Sara', role: 'user' },
{ name: 'Ali', role: 'admin' }
];

اگر بخواهیم تمام Adminها را داشته باشیم:

const admins = users.filter(
user => user.role === 'admin'
);

نتیجه:

[
{ name: 'Omid', role: 'admin' },
{ name: 'Ali', role: 'admin' }
]

اما اگر فقط اولین Admin را بخواهیم:

const admin = users.find(
user => user.role === 'admin'
);

نتیجه:

{ name: 'Omid', role: 'admin' }

پس استفاده از filter() برای مسئله دوم، کار می‌کند اما دقیقاً بیانگر Intent ما نیست.

در طراحی حرفه‌ای Code، بهتر است Method با Intent هماهنگ باشد.

تفاوت find() و some() در یک مسئله واقعی

فرض کنید:

const users = [
{ name: 'Omid', active: true },
{ name: 'Sara', active: false },
{ name: 'Ali', active: true }
];

اگر بخواهیم اولین User غیرفعال را پیدا کنیم:

const inactiveUser = users.find(
user => !user.active
);

نتیجه:

{ name: 'Sara', active: false }

اما اگر فقط بخواهیم بدانیم User غیرفعالی وجود دارد یا خیر:

const hasInactiveUser = users.some(
user => !user.active
);

نتیجه:

true

در حالت دوم، گرفتن Object اضافی است.

بنابراین:

Need the Element?
→ find()

Need only existence?
→ some()
تفاوت some() و every() در Validation

فرض کنید:

const scores = [80, 90, 65, 75];

اگر سؤال این باشد:

آیا حداقل یک نمره کمتر از 70 وجود دارد؟

const hasFailedScore = scores.some(
score => score < 70
);

نتیجه:

true

اما اگر سؤال این باشد:

آیا تمام نمره‌ها حداقل 60 هستند؟

const allPassed = scores.every(
score => score >= 60
);

نتیجه:

true

پس تفاوت آنها در یک عبارت ساده:

some()
At least one

every()
All
Callback Parameters

Methodهای این فصل Callback دریافت می‌کنند و Callback می‌تواند Parameters مختلفی دریافت کند.

مثلاً:

const products = ['Laptop', 'Phone', 'Tablet'];

products.find((product, index, array) => {
console.log(product);
console.log(index);
console.log(array);

return product === 'Phone';
});

Callback می‌تواند به‌صورت مفهومی این اطلاعات را دریافت کند:

element
index
array

در بیشتر موارد فقط Element موردنیاز است:

products.find(
product => product === 'Phone'
);

اما در صورت نیاز می‌توان از Index نیز استفاده کرد.

یک نکته مهم درباره findIndex()

نباید findIndex() را با indexOf() یکی بدانیم.

indexOf() برای پیدا کردن یک Value مشخص استفاده می‌شود:

const products = ['Laptop', 'Phone', 'Tablet'];

const index = products.indexOf('Phone');

console.log(index);

خروجی:

1

اما findIndex() زمانی قدرتمندتر است که معیار Search یک شرط باشد:

const products = [
{ name: 'Laptop', price: 1200 },
{ name: 'Phone', price: 800 },
{ name: 'Tablet', price: 500 }
];

const index = products.findIndex(
product => product.price > 700
);

console.log(index);

خروجی:

0

پس:

indexOf()
Search by value

findIndex()
Search by condition

این دو Method برای مسائل متفاوتی طراحی شده‌اند.

یک نکته مهم درباره some() و includes()

includes() نیز می‌تواند بررسی کند که آیا یک Value در Array وجود دارد:

const roles = ['admin', 'user', 'editor'];

console.log(roles.includes('admin'));

خروجی:

true

اما some() زمانی مناسب است که شرط Search پیچیده‌تر باشد:

const users = [
{ name: 'Omid', role: 'admin' },
{ name: 'Sara', role: 'user' }
];

const hasAdmin = users.some(
user => user.role === 'admin'
);

بنابراین:

includes()
آیا این Value وجود دارد؟

some()
آیا حداقل یک Element این شرط را دارد؟

این تفاوت به انتخاب صحیح ابزار کمک می‌کند.

مرتب‌سازی Stringها و ترتیب Lexicographic

در مرتب‌سازی Stringها، باید مفهوم Lexicographic Order را بشناسیم.

مثلاً:

const names = ['Sara', 'Ali', 'Omid'];

names.sort();

console.log(names);

خروجی:

['Ali', 'Omid', 'Sara']

اما مرتب‌سازی Stringها همیشه به معنی:

ترتیب ساده حروف انگلیسی

نیست.

ترتیب می‌تواند تحت تأثیر قواعد Unicode و Locale قرار بگیرد.

برای مقایسه دقیق‌تر Stringها معمولاً می‌توان از:

localeCompare()

استفاده کرد.

مثلاً:

names.sort(
(a, b) => a.localeCompare(b)
);

در Applicationهایی که داده‌های چندزبانه دارند، انتخاب روش مقایسه اهمیت بیشتری پیدا می‌کند.

مرتب‌سازی بر اساس چند معیار

گاهی یک Comparator فقط یک معیار ندارد.

فرض کنید محصولات را ابتدا بر اساس Category و سپس Price مرتب کنیم:

const products = [
{ name: 'Laptop', category: 'computer', price: 1200 },
{ name: 'Phone', category: 'mobile', price: 800 },
{ name: 'Monitor', category: 'computer', price: 300 },
{ name: 'Tablet', category: 'mobile', price: 500 }
];

می‌توانیم Comparator را مرحله‌ای طراحی کنیم:

products.sort((a, b) => {
const categoryComparison =
a.category.localeCompare(b.category);

if (categoryComparison !== 0) {
return categoryComparison;
}

return a.price - b.price;
});

منطق این Comparator:

Compare category
↓
Different?
├── Yes → use category order
└── No
↓
Compare price

این مثال نشان می‌دهد Comparator فقط یک Function کوچک برای a - b نیست.

Comparator می‌تواند Rule مربوط به Order در یک Domain واقعی را پیاده کند.

Best Practices
۱. Method را بر اساس Intent انتخاب کنید

اگر فقط اولین Element را می‌خواهید:

find()

اگر Index را می‌خواهید:

findIndex()

اگر فقط وجود حداقل یک Match مهم است:

some()

اگر باید تمام عناصر شرط را داشته باشند:

every()

اگر ترتیب عناصر مهم است:

sort()
۲. برای گرفتن اولین Match از filter() استفاده نکنید

به جای:

const products = products.filter(
product => product.price > 1000
);

const product = products[0];

اگر فقط اولین Match موردنیاز است:

const product = products.find(
product => product.price > 1000
);

Intent واضح‌تر است و Array غیرضروری ساخته نمی‌شود.

۳. برای Boolean Logic از some() و every() استفاده کنید

اگر نتیجه موردنیاز فقط:

true / false

است، بهتر است Method متناسب با این Intent را انتخاب کنیم.

مثلاً:

const hasAdmin = users.some(
user => user.role === 'admin'
);

بهتر از ساختن یک نتیجه میانی فقط برای بررسی وجود Admin است.

۴. برای Numberها Comparator صریح بنویسید

این:

prices.sort();

برای مرتب‌سازی عددی مناسب نیست.

بهتر است:

prices.sort((a, b) => a - b);

یا:

prices.sort((a, b) => b - a);
۵. Mutation مربوط به sort() را آگاهانه مدیریت کنید

اگر تغییر Array اصلی مطلوب است:

products.sort(
(a, b) => a.price - b.price
);

اما اگر باید Array اصلی حفظ شود، از روش غیرMutating مانند:

products.toSorted(
(a, b) => a.price - b.price
);

استفاده کنید.

۶. نتیجه Search را بررسی کنید

find() ممکن است:

undefined

برگرداند.

بنابراین نباید بدون توجه به این حالت بنویسیم:

const product = products.find(...);

console.log(product.name);

ابتدا باید مشخص کنیم اگر Match وجود نداشت، Application چه رفتاری باید داشته باشد.

Common Mistakes
اشتباه اول: تصور اینکه find() همه Matchها را برمی‌گرداند

این اشتباه است:

const products = products.find(
product => product.category === 'mobile'
);

find() فقط اولین Match را برمی‌گرداند.

برای تمام Matchها:

const products = products.filter(
product => product.category === 'mobile'
);
اشتباه دوم: انتظار null از find()

در حالت نبودن Match:

const product = products.find(...);

نتیجه:

undefined

است، نه:

null
اشتباه سوم: انتظار -1 از find()

-1 نتیجه find() نیست.

در:

find()

عدم وجود Match:

undefined

است.

در:

findIndex()

عدم وجود Match:

-1

است.

اشتباه چهارم: استفاده از some() برای دریافت Element

این:

const product = products.some(
product => product.price > 1000
);

Product را برنمی‌گرداند.

نتیجه:

true

یا:

false

است.

اگر خود Element لازم است:

const product = products.find(
product => product.price > 1000
);
اشتباه پنجم: اشتباه گرفتن some() و every()

این دو سؤال متفاوت‌اند:

some()
حداقل یکی؟

every()
همه؟

اگر فقط یک Element شرط را نقض کند:

every()

می‌تواند false شود.

اما:

some()

ممکن است همچنان true باشد، اگر حداقل یک Element دیگر شرط را برقرار کند.

اشتباه ششم: مرتب‌سازی Numberها بدون Comparator

این:

const numbers = [100, 5, 20];

numbers.sort();

مرتب‌سازی عددی مورد انتظار را تضمین نمی‌کند.

برای ترتیب عددی:

numbers.sort(
(a, b) => a - b
);
اشتباه هفتم: فراموش کردن Mutation در sort()

این:

const sorted = products.sort(
(a, b) => a.price - b.price
);

یک Array مستقل ایجاد نمی‌کند.

Array اصلی:

products

نیز مرتب شده است.

اشتباه هشتم: فرض اینکه Comparator باید فقط -1، 0 یا 1 برگرداند

این تصور دقیق نیست.

Comparator باید رابطه Ordering را مشخص کند:

negative → a قبل از b
zero     → برابر از نظر ترتیب
positive → a بعد از b

بنابراین:

(a, b) => a.price - b.price

کاملاً معتبر است، حتی اگر نتیجه مثلاً:

-700

باشد.

Summary

در فصل قبل، Array Processing را با:

map
filter
reduce

بررسی کردیم.

اما همه مسائل مربوط به Array به Transformation، Selection یا Accumulation محدود نمی‌شوند.

گاهی فقط یک Element لازم داریم.

برای این مسئله:

find()

اولین Element مطابق شرط را برمی‌گرداند.

اگر خود Element موردنیاز نباشد و فقط Index آن را بخواهیم:

findIndex()

استفاده می‌شود.

گاهی حتی Index نیز لازم نیست و فقط می‌خواهیم بدانیم آیا حداقل یک Element شرطی را برقرار می‌کند:

some()

در مقابل، وقتی باید تمام عناصر شرط را برقرار کنند:

every()

استفاده می‌شود.

سپس با مسئله متفاوتی روبه‌رو شدیم:

ترتیب عناصر باید تغییر کند.

برای این مسئله:

sort()

استفاده می‌شود.

اما sort() برای Numberها به Comparator نیاز دارد:

(a, b) => a - b

و باید به خاطر داشته باشیم که sort() Array اصلی را Mutation می‌کند.

در نتیجه Concept Flow این فصل را می‌توان به شکل زیر خلاصه کرد:

Search
↓
find
↓
findIndex
↓
some
↓
every
↓
Sorting
↓
sort
↓
Comparator
↓
Mutation
↓
Practical Patterns
Key Takeaways
find() اولین Element مطابق شرط را برمی‌گرداند.
اگر find() چیزی پیدا نکند، undefined برمی‌گرداند.
findIndex() Index اولین Match را برمی‌گرداند.
اگر findIndex() چیزی پیدا نکند، -1 برمی‌گرداند.
some() بررسی می‌کند که آیا حداقل یک Element شرط را دارد.
every() بررسی می‌کند که آیا تمام عناصر شرط را دارند.
find() و some() می‌توانند قبل از پایان Array متوقف شوند.
every() نیز با پیدا کردن اولین Element ناموفق می‌تواند متوقف شود.
sort() ترتیب عناصر Array را تغییر می‌دهد.
sort() در حالت پیش‌فرض برای مقایسه، رفتار String-like دارد.
برای مرتب‌سازی Numberها معمولاً از a - b یا b - a استفاده می‌شود.
Comparator یک Rule برای تعیین ترتیب دو Element است.
Comparator می‌تواند برای Objectها بر اساس Propertyهای مختلف طراحی شود.
sort() یک Mutating Method است.
در صورت نیاز به حفظ Array اصلی، toSorted() گزینه غیرMutating مدرن‌تری است.
انتخاب Method باید بر اساس Intent و نوع نتیجه موردنیاز انجام شود.
Technical Interview
Junior
1. تفاوت find() و filter() چیست؟

find() اولین Element مطابق شرط را برمی‌گرداند و در صورت نبودن Match مقدار undefined می‌دهد.

filter() تمام عناصر مطابق شرط را در یک Array جدید قرار می‌دهد.

2. اگر find() چیزی پیدا نکند چه اتفاقی می‌افتد؟

find() مقدار undefined برمی‌گرداند.

3. findIndex() در صورت پیدا نشدن Element چه چیزی برمی‌گرداند؟

مقدار -1.

4. some() چه کاری انجام می‌دهد؟

بررسی می‌کند که آیا حداقل یک Element شرط Callback را برقرار می‌کند یا خیر و نتیجه آن Boolean است.

5. every() چه کاری انجام می‌دهد؟

بررسی می‌کند که آیا تمام عناصر Array شرط Callback را برقرار می‌کنند یا خیر.

6. تفاوت some() و every() چیست؟

some() برای حداقل یک Match و every() برای تمام Matchها استفاده می‌شود.

7. چرا sort() برای Numberها ممکن است نتیجه غیرمنتظره ایجاد کند؟

زیرا sort() در حالت پیش‌فرض مقادیر را بر اساس ترتیب String-like مقایسه می‌کند، نه ترتیب عددی.

Mid-Level
8. Comparator در sort() چیست؟

Comparator تابعی است که رابطه ترتیب میان دو Element را مشخص می‌کند. مقدار منفی یعنی a قبل از b، مقدار مثبت یعنی a بعد از b و صفر یعنی از نظر ترتیب برابر هستند.

9. چرا (a, b) => a - b باعث مرتب‌سازی صعودی Numberها می‌شود؟

اگر a < b باشد، a - b منفی است و a باید قبل از b قرار گیرد.

اگر a > b باشد، نتیجه مثبت است و a بعد از b قرار می‌گیرد.

10. آیا sort() یک Array جدید ایجاد می‌کند؟

خیر. sort() خود Array را Mutation می‌کند و همان Array مرتب‌شده را برمی‌گرداند.

11. تفاوت sort() و toSorted() چیست؟

sort() Array اصلی را تغییر می‌دهد.

toSorted() یک Array جدید با ترتیب مرتب‌شده ایجاد می‌کند و Array اصلی را تغییر نمی‌دهد.

12. چه زمانی some() نسبت به find() انتخاب مناسب‌تری است؟

وقتی فقط می‌خواهیم بدانیم آیا حداقل یک Element مطابق شرط وجود دارد و خود Element برای ما اهمیتی ندارد.

13. چه زمانی findIndex() نسبت به find() مناسب‌تر است؟

وقتی به Position یا Index Element نیاز داریم، نه خود Element.

Senior
14. چرا انتخاب بین find()، filter() و some() فقط یک انتخاب Syntax نیست؟

زیرا هر Method Intent متفاوتی را بیان می‌کند:

find()   → obtain one element
filter() → obtain all matching elements
some()   → test existence of a match

انتخاب درست Method باعث می‌شود Code مستقیماً منطق مسئله را بیان کند و از پردازش یا ساخت داده غیرضروری جلوگیری شود.

15. چرا find() و some() می‌توانند از نظر پردازش مناسب‌تر از روش‌های Loop-based باشند؟

هر دو Method می‌توانند پس از مشخص شدن نتیجه، Iteration را متوقف کنند.

find() پس از اولین Match متوقف می‌شود.

some() پس از اولین Match که نتیجه Boolean را true می‌کند متوقف می‌شود.

بنابراین لازم نیست برای تعیین نتیجه، الزاماً کل Array پیمایش شود.

16. چرا Mutation در sort() می‌تواند یک مسئله مهندسی ایجاد کند؟

زیرا ممکن است Array در بخش‌های مختلف Application با همان Reference استفاده شده باشد.

وقتی sort() اجرا می‌شود، همه Referenceهایی که به همان Array اشاره می‌کنند، ترتیب جدید را مشاهده خواهند کرد.

بنابراین باید تصمیم بگیریم که Mutation موردنظر است یا باید یک Array جدید تولید شود.

17. چرا Comparator را باید بخشی از Business Logic در نظر گرفت؟

زیرا Comparator مشخص می‌کند مفهوم Order در یک Domain چیست.

مثلاً:

(a, b) => a.price - b.price

می‌گوید Order بر اساس Price صعودی است.

اما در یک Application واقعی ممکن است Order بر اساس:

price
rating
date
priority
name
category

یا ترکیبی از آنها تعریف شود.

بنابراین Comparator صرفاً یک Function فنی برای sort() نیست؛ بلکه می‌تواند Rule مربوط به ترتیب داده‌ها را بیان کند.

Golden Answers
find() چیست؟

find() یک Array Method برای پیدا کردن اولین Element مطابق یک شرط است. اگر Match پیدا نشود، undefined برمی‌گرداند.

findIndex() چیست؟

findIndex() مانند find() اولین Match را جست‌وجو می‌کند، اما به‌جای Element، Index آن را برمی‌گرداند و در صورت نبودن Match مقدار -1 می‌دهد.

some() چیست؟

some() بررسی می‌کند که آیا حداقل یک Element شرط مشخص‌شده را برقرار می‌کند یا خیر و نتیجه آن Boolean است.

every() چیست؟

every() بررسی می‌کند که آیا تمام عناصر شرط مشخص‌شده را برقرار می‌کنند یا خیر و نتیجه آن Boolean است.

تفاوت find() و filter() چیست؟

find() فقط اولین Match را به‌صورت یک Element برمی‌گرداند، در حالی که filter() تمام Matchها را در یک Array جدید جمع می‌کند.

تفاوت some() و every() چیست؟

some() می‌پرسد:

آیا حداقل یکی؟

و every() می‌پرسد:

آیا همه؟

Comparator چیست؟

Comparator تابعی است که به sort() می‌گوید دو Element نسبت به یکدیگر چه ترتیبی دارند. نتیجه منفی، صفر یا مثبت به‌ترتیب رابطه Ordering را مشخص می‌کند.

چرا برای Numberها از a - b استفاده می‌کنیم؟

زیرا نتیجه مقایسه را مستقیماً به سه حالت Ordering تبدیل می‌کند:

a < b → negative
a = b → zero
a > b → positive

بنابراین a - b ترتیب صعودی و b - a ترتیب نزولی را ایجاد می‌کند.

آیا sort() Array را Mutation می‌کند؟

بله. sort() عناصر همان Array را جابه‌جا می‌کند و Array اصلی را تغییر می‌دهد.

اگر نخواهیم Array اصلی تغییر کند چه کنیم؟

در JavaScript مدرن می‌توان از toSorted() استفاده کرد که یک Array جدید با ترتیب مرتب‌شده برمی‌گرداند و Array اصلی را Mutation نمی‌کند.

Conclusion

در فصل قبل یاد گرفتیم که چگونه Array را تبدیل، فیلتر و خلاصه کنیم.

اما پردازش Array همیشه به ساخت یک نتیجه جدید ختم نمی‌شود.

گاهی فقط یک Element لازم داریم:

find()

گاهی Position آن Element مهم است:

findIndex()

گاهی فقط می‌خواهیم وجود یک وضعیت را بررسی کنیم:

some()

و گاهی باید تمام داده‌ها یک شرط را رعایت کنند:

every()

سپس وارد مسئله‌ای متفاوت شدیم.

گاهی داده درست است، اما ترتیب آن درست نیست.

در این حالت:

sort()

ترتیب عناصر را تغییر می‌دهد و Comparator مشخص می‌کند این ترتیب دقیقاً چگونه باید باشد.

در نهایت یک نکته مهندسی مهم باقی می‌ماند:

انتخاب Array Method باید از روی سؤال مسئله انجام شود، نه از روی Syntax.

اگر یک Element می‌خواهیم، find().

اگر Index می‌خواهیم، findIndex().

اگر فقط وجود Match مهم است، some().

اگر همه عناصر باید شرط را داشته باشند، every().

و اگر مسئله درباره Order است، sort() و یک Comparator مناسب.

این مدل ذهنی، Array Methods را از مجموعه‌ای از Methodهای قابل حفظ کردن به مجموعه‌ای از ابزارهای قابل استنتاج برای حل مسئله تبدیل می‌کند.