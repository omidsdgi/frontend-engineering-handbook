Chapter 37 — Destructuring and Advanced Array Patterns
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مفهوم Destructuring را در Arrayها درک کنید.
مقادیر Array را مستقیماً در Variableهای جداگانه دریافت کنید.
از Rest برای جمع‌آوری بخش باقی‌مانده‌ی یک Array استفاده کنید.
تفاوت Rest و Spread را تشخیص دهید.
با Spread یک Array جدید ایجاد کنید.
Arrayها را بدون Mutation کردن Array اصلی Copy و Merge کنید.
ساختارهای Nested Array را با Destructuring مدیریت کنید.
از این Patternها برای Immutable Data Updates استفاده کنید.
بر اساس مسئله، بین Destructuring، Rest و Spread انتخاب درستی داشته باشید.
Core Question

چگونه Arrayها را با Syntax مدرن و الگوهای حرفه‌ای مدیریت کنیم؟

مقدمه

در فصل‌های قبل دیدیم که Array یکی از ساختارهای اصلی برای نگهداری مجموعه‌ای از داده‌ها است.

مثلاً:

const products = [
'Laptop',
'Phone',
'Monitor'
];

در بسیاری از مواقع فقط می‌خواهیم به عناصر این Array دسترسی داشته باشیم.

روش معمول این است:

const firstProduct = products[0];
const secondProduct = products[1];

این روش کاملاً درست است، اما وقتی هدف ما استخراج چند مقدار مشخص از یک Array باشد، Syntax می‌تواند مستقیم‌تر باشد.

از طرف دیگر، در Applicationهای واقعی مرتباً با موقعیت‌هایی مواجه می‌شویم که باید:

چند مقدار اول Array را جدا کنیم.
بقیه‌ی عناصر را نگه داریم.
دو Array را با هم ترکیب کنیم.
بدون تغییر Array اصلی، یک نسخه‌ی جدید بسازیم.
داده‌های Nested را از ساختار پیچیده خارج کنیم.

JavaScript برای این مسائل چند قابلیت مرتبط در اختیار ما قرار می‌دهد:

Array
↓
Destructuring
↓
Rest
↓
Spread
↓
Copying
↓
Merging
↓
Nested Data
↓
Immutable Updates

این قابلیت‌ها جدا از یکدیگر نیستند.

همه‌ی آن‌ها به یک مسئله‌ی مشترک مربوط می‌شوند:

چگونه داده‌های موجود در Array را به شکل ساده‌تر و قابل‌کنترل‌تری دریافت و مدیریت کنیم؟

استخراج مقادیر با Array Destructuring

فرض کنید:

const products = [
'Laptop',
'Phone',
'Monitor'
];

اگر بخواهیم سه عنصر اول را در Variableهای جداگانه قرار دهیم، می‌توانیم از Array Destructuring استفاده کنیم:

const [first, second, third] = products;

اکنون:

first  → 'Laptop'
second → 'Phone'
third  → 'Monitor'

در واقع ساختار سمت چپ مشخص می‌کند که هر مقدار از Array در کجا قرار بگیرد.

Array
↓
[Laptop, Phone, Monitor]

Destructuring
↓
first
second
third

بنابراین Destructuring روش جدیدی برای ایجاد داده نیست.

فقط به ما اجازه می‌دهد مقادیر موجود در یک ساختار را مستقیماً در Variableهای مختلف قرار دهیم.

ترتیب عناصر مهم است

در Array Destructuring، ترتیب اهمیت دارد.

const [first, second] = products;

مقدار اول Array در first قرار می‌گیرد و مقدار دوم در second.

اگر ترتیب را تغییر دهیم:

const [second, first] = products;

JavaScript نمی‌داند که ما از نظر معنایی چه نامی برای Variableها در نظر گرفته‌ایم.

فقط بر اساس Position عمل می‌کند:

Index 0 → first variable
Index 1 → second variable

بنابراین:

Array Destructuring بر اساس Position انجام می‌شود، نه نام Property.

این نکته آن را از Object Destructuring متمایز می‌کند.

رد کردن بعضی عناصر

گاهی به مقدار اول نیاز نداریم، اما مقدار دوم برای ما مهم است.

مثلاً:

const [, secondProduct] = products;

در اینجا جای خالی یعنی مقدار اول نادیده گرفته شود.

نتیجه:

secondProduct → 'Phone'

این Pattern زمانی مفید است که ساختار Array را می‌شناسیم، اما فقط بعضی از Positionها برای ما اهمیت دارند.

مقدار پیش‌فرض در Destructuring

ممکن است Array به اندازه‌ی کافی Element نداشته باشد.

const products = ['Laptop'];

const [first, second] = products;

در این حالت:

first  → 'Laptop'
second → undefined

اگر مقدار پیش‌فرض بخواهیم، می‌توانیم آن را در Destructuring مشخص کنیم:

const [first, second = 'Unknown'] = products;

اکنون:

first  → 'Laptop'
second → 'Unknown'

مقدار پیش‌فرض فقط زمانی استفاده می‌شود که مقدار مربوطه undefined باشد.

وقتی تعداد عناصر مشخص نیست: Rest

تا اینجا می‌دانستیم چند مقدار اول را می‌خواهیم.

اما فرض کنید Array می‌تواند تعداد زیادی Element داشته باشد و ما فقط Element اول را می‌خواهیم و باید تمام عناصر باقی‌مانده را نیز نگه داریم.

مثلاً:

const products = [
'Laptop',
'Phone',
'Monitor',
'Tablet'
];

می‌توانیم بنویسیم:

const [first, ...others] = products;

اکنون:

first
→ 'Laptop'

others
→ ['Phone', 'Monitor', 'Tablet']

اینجا ...others یک Rest Pattern است.

Rest یعنی:

مقادیر باقی‌مانده را جمع‌آوری کن.

مدل ذهنی:

Array
↓
First Element + Remaining Elements
↓
first + ...others
Rest فقط در انتهای Pattern قرار می‌گیرد

Rest برای جمع‌آوری عناصر باقی‌مانده استفاده می‌شود.

بنابراین:

const [first, ...others] = products;

صحیح است.

اما نمی‌توانیم به شکل دلخواه آن را در وسط Pattern قرار دهیم.

دلیل آن ساده است.

Rest باید بداند:

از این نقطه به بعد، تمام عناصر باقی‌مانده را جمع‌آوری کن.

پس Rest پایان Pattern را مشخص می‌کند.

Rest در کنار Destructuring

ترکیب Destructuring و Rest یکی از Patternهای مهم در JavaScript است.

فرض کنید می‌خواهیم اولین محصول را جدا کنیم و بقیه را در Array دیگری داشته باشیم:

const [firstProduct, ...remainingProducts] = products;

این کار بدون تغییر Array اصلی انجام می‌شود.

Array اولیه همچنان همان Array است.

products
↓
['Laptop', 'Phone', 'Monitor', 'Tablet']

Destructuring
↓
firstProduct
remainingProducts

این Pattern در پروژه‌های واقعی، مخصوصاً هنگام کار با Dataهای ترتیبی، بسیار کاربردی است.

Spread: باز کردن عناصر Array

تا اینجا با Rest آشنا شدیم.

اما همان Syntax ... در یک موقعیت دیگر رفتار متفاوتی دارد.

فرض کنید دو Array داریم:

const frontend = ['HTML', 'CSS'];
const backend = ['Node.js', 'SQL'];

اگر بخواهیم یک Array جدید شامل تمام عناصر این دو Array بسازیم:

const skills = [
...frontend,
...backend
];

نتیجه:

['HTML', 'CSS', 'Node.js', 'SQL']

اینجا ... دیگر Rest نیست.

این بار Spread Syntax است.

Spread یعنی:

عناصر یک Iterable را در ساختار جدید باز کن.

مدل ذهنی:

frontend
↓
...frontend
↓
HTML, CSS

و:

backend
↓
...backend
↓
Node.js, SQL

سپس این عناصر در Array جدید قرار می‌گیرند.

تفاوت Rest و Spread

هر دو از ... استفاده می‌کنند، اما جهت عمل آن‌ها متفاوت است.

Rest مقادیر را جمع می‌کند:

const [first, ...remaining] = products;

در اینجا چند Element باقی‌مانده در یک Array جمع می‌شوند.

اما Spread عناصر را باز می‌کند:

const allProducts = [
...products
];

در اینجا عناصر Array در Array جدید قرار می‌گیرند.

پس:

Rest
→ Collect remaining values

Spread
→ Expand values

این تفاوت یکی از مهم‌ترین نکات این فصل است.

ایجاد Copy با Spread

فرض کنید:

const products = [
'Laptop',
'Phone',
'Monitor'
];

اگر بخواهیم یک Array جدید با همان عناصر ایجاد کنیم:

const copiedProducts = [
...products
];

اکنون دو Array داریم:

products
↓
['Laptop', 'Phone', 'Monitor']

copiedProducts
↓
['Laptop', 'Phone', 'Monitor']

اما این دو Array یک Object یکسان نیستند.

console.log(products === copiedProducts);

نتیجه:

false

این موضوع اهمیت زیادی دارد.

Spread در اینجا یک Array جدید ایجاد کرده است.

چرا Copy کردن اهمیت دارد؟

در JavaScript، Array یک Object است و Variable معمولاً Reference آن Object را نگه می‌دارد.

بنابراین اگر فقط Reference را منتقل کنیم:

const copiedProducts = products;

دو Variable به همان Array اشاره می‌کنند.

products ─────┐
↓
Array
↑
copiedProducts

پس تغییر از طریق یکی می‌تواند روی دیگری اثر بگذارد.

اما با Spread:

const copiedProducts = [...products];

یک Array جدید ایجاد می‌کنیم:

products
↓
Array A

copiedProducts
↓
Array B

این همان دلیلی است که Spread در Immutable Data Handling بسیار مهم می‌شود.

Merging Arrayها

Spread فقط برای Copy کردن نیست.

می‌توانیم چند Array را نیز با هم ترکیب کنیم.

const featured = ['Laptop', 'Phone'];
const newProducts = ['Tablet', 'Monitor'];

const products = [
...featured,
...newProducts
];

نتیجه:

['Laptop', 'Phone', 'Tablet', 'Monitor']

مزیت این روش این است که Arrayهای اولیه تغییر نمی‌کنند.

featured
↘
new Array
↗
newProducts

بنابراین Spread هم برای Copying و هم برای Merging یک Pattern مهم است.

ترکیب Array و مقدار جدید

Spread فقط برای ترکیب دو Array استفاده نمی‌شود.

می‌توانیم Element جدیدی را نیز هم‌زمان اضافه کنیم.

const products = [
'Laptop',
'Phone'
];

const updatedProducts = [
...products,
'Monitor'
];

نتیجه:

['Laptop', 'Phone', 'Monitor']

Array اصلی تغییر نکرده است.

این Pattern برای ایجاد نسخه‌ی جدید از یک Collection بسیار رایج است.

حذف یک بخش از Array با Rest

Rest نیز می‌تواند برای ساختن نسخه‌ای متفاوت از یک Array استفاده شود.

فرض کنید:

const products = [
'Laptop',
'Phone',
'Monitor'
];

اگر بخواهیم اولین Element را جدا کنیم:

const [first, ...remaining] = products;

اکنون:

first
→ 'Laptop'

remaining
→ ['Phone', 'Monitor']

Array اصلی همچنان بدون تغییر باقی مانده است.

در اینجا Rest به ما کمک می‌کند ساختار Array را به بخش‌های کوچک‌تر تقسیم کنیم.

Nested Arrays

در پروژه‌های واقعی همیشه با Arrayهای ساده روبه‌رو نیستیم.

ممکن است یک Array شامل Arrayهای دیگر باشد:

const coordinates = [
[10, 20],
[30, 40]
];

با Destructuring می‌توانیم ساختار را مستقیماً باز کنیم:

const [[x1, y1], [x2, y2]] = coordinates;

اکنون:

x1 → 10
y1 → 20
x2 → 30
y2 → 40

در اینجا Destructuring در دو سطح انجام شده است.

سطح اول:

coordinates
↓
[point1, point2]

و سپس هر Point نیز Destructure می‌شود:

point
↓
[x, y]

این Pattern زمانی مفید است که ساختار داده از قبل مشخص باشد و بخواهیم مقادیر مشخصی را مستقیماً استخراج کنیم.

Destructuring در داده‌های واقعی

فرض کنید نتیجه‌ی یک API شامل اطلاعات یک Recipe باشد:

const recipe = [
'Pasta',
20,
['Tomato', 'Garlic']
];

می‌توانیم اطلاعات را مستقیماً استخراج کنیم:

const [
title,
cookingTime,
[firstIngredient, secondIngredient]
] = recipe;

اکنون:

title
→ 'Pasta'

cookingTime
→ 20

firstIngredient
→ 'Tomato'

secondIngredient
→ 'Garlic'

مزیت اصلی این Pattern زمانی مشخص می‌شود که ساختار داده مشخص باشد و بخواهیم چند مقدار مشخص را در یک مرحله استخراج کنیم.

Immutable Updates با Spread

تا اینجا دیدیم که Spread می‌تواند یک Copy از Array ایجاد کند.

اما کاربرد مهم‌تر آن در Applicationهای واقعی، Immutable Update است.

فرض کنید:

const products = [
'Laptop',
'Phone',
'Monitor'
];

می‌خواهیم محصول جدیدی اضافه کنیم، بدون اینکه Array اصلی را تغییر دهیم.

می‌توانیم بنویسیم:

const updatedProducts = [
...products,
'Tablet'
];

اکنون:

products
→ ['Laptop', 'Phone', 'Monitor']

updatedProducts
→ ['Laptop', 'Phone', 'Monitor', 'Tablet']

Array اصلی دست‌نخورده باقی مانده است.

این روش در State Management و UI Development اهمیت زیادی دارد؛ زیرا به‌جای تغییر مستقیم Data موجود، یک نسخه‌ی جدید ایجاد می‌کنیم.

Immutable Update در یک Position مشخص

گاهی می‌خواهیم یک Element را با مقدار دیگری جایگزین کنیم.

مثلاً:

const products = [
'Laptop',
'Phone',
'Monitor'
];

اگر بخواهیم Phone را با Tablet جایگزین کنیم، می‌توانیم از ترکیب slice() و Spread استفاده کنیم:

const updatedProducts = [
...products.slice(0, 1),
'Tablet',
...products.slice(2)
];

نتیجه:

['Laptop', 'Tablet', 'Monitor']

Array اصلی تغییر نکرده است.

این مثال نشان می‌دهد که Immutable Update معمولاً به معنی استفاده از یک Syntax خاص نیست.

بلکه یک اصل است:

به‌جای Mutation کردن Data موجود، نسخه‌ی جدیدی از آن ایجاد کن.

ترتیب Spread در ساختن Array جدید مهم است

Spread به‌تنهایی ترتیب را تغییر نمی‌دهد.

مثلاً:

const first = ['A', 'B'];
const second = ['C', 'D'];

const result = [
...first,
...second
];

نتیجه:

['A', 'B', 'C', 'D']

اما:

const result = [
...second,
...first
];

نتیجه:

['C', 'D', 'A', 'B']

پس Spread فقط عناصر را باز می‌کند.

ترتیب نهایی را ساختاری که عناصر را در آن قرار می‌دهیم تعیین می‌کند.

یک نکته‌ی مهم درباره Copy

Spread یک Shallow Copy ایجاد می‌کند.

یعنی فقط خود Array جدید است.

اگر Array شامل Objectهای داخلی باشد، Objectهای داخلی Copy عمیق نمی‌شوند.

مثلاً:

const products = [
{ name: 'Laptop' },
{ name: 'Phone' }
];

const copiedProducts = [...products];

اکنون Array جدید است، اما Objectهای داخل آن همان Referenceهای قبلی هستند.

مدل ذهنی:

products
↓
Array A
↓
Object A

copiedProducts
↓
Array B
↓
Object A

بنابراین:

Spread برای Copy کردن خود Array مناسب است، اما به‌تنهایی Deep Copy ایجاد نمی‌کند.

این تفاوت در کار با داده‌های Nested اهمیت زیادی دارد.

Destructuring، Rest و Spread در کنار یکدیگر

اکنون می‌توانیم سه مفهوم اصلی این فصل را کنار هم قرار دهیم.

فرض کنید:

const products = [
'Laptop',
'Phone',
'Monitor',
'Tablet'
];

با Destructuring می‌توانیم اولین عنصر را دریافت کنیم:

const [firstProduct, ...remainingProducts] = products;

اینجا:

Destructuring
→ extract values

Rest
→ collect remaining values

و اگر بخواهیم Array جدیدی با یک محصول اضافه ایجاد کنیم:

const updatedProducts = [
...products,
'Keyboard'
];

اینجا:

Spread
→ expand existing values

پس سه مفهوم در یک مدل ذهنی:

Destructuring
→ Take values out

Rest
→ Collect remaining values

Spread
→ Put values into a new structure

این مدل ذهنی از حفظ کردن شکل ... مهم‌تر است.

Best Practices
از Destructuring برای استخراج مستقیم مقادیر استفاده کنید

اگر ساختار Array مشخص است و چند مقدار مشخص لازم دارید:

const [first, second] = products;

از این روش استفاده کنید.

Rest را برای Remaining Values به کار ببرید

وقتی یک بخش از Array را جدا می‌کنید و می‌خواهید بقیه‌ی عناصر را حفظ کنید:

const [first, ...remaining] = products;
از Spread برای Copy و Merge استفاده کنید

برای ایجاد Array جدید:

const copy = [...products];

و برای ترکیب:

const allProducts = [
...featured,
...newProducts
];
Mutation را آگاهانه انجام دهید

اگر حفظ Array اصلی مهم است، به‌جای تغییر مستقیم آن، Array جدید بسازید.

const updatedProducts = [
...products,
newProduct
];
به Shallow Copy توجه کنید

Spread فقط ساختار سطح اول Array را Copy می‌کند.

اگر داده‌ها Nested هستند، باید Referenceهای داخلی را نیز در طراحی Update در نظر بگیرید.

Common Mistakes
اشتباه اول: اشتباه گرفتن Rest و Spread

هر دو از ... استفاده می‌کنند، اما کاربردشان متفاوت است.

const [first, ...remaining] = products;

اینجا Rest است.

اما:

const copy = [...products];

اینجا Spread است.

مدل ذهنی:

Rest   → collect
Spread → expand
اشتباه دوم: تصور اینکه Assignment یک Copy ایجاد می‌کند

این کد:

const copy = products;

Copy ایجاد نمی‌کند.

هر دو Variable به همان Array اشاره می‌کنند.

برای یک Shallow Copy:

const copy = [...products];

مناسب است.

اشتباه سوم: تصور اینکه Spread Deep Copy ایجاد می‌کند

در:

const copy = [...products];

خود Array جدید است، اما Objectهای داخل آن همچنان Referenceهای قبلی هستند.

بنابراین Spread را نباید به‌عنوان راه‌حل عمومی Deep Copy در نظر گرفت.

اشتباه چهارم: استفاده‌ی غیرضروری از Destructuring

Destructuring زمانی مفید است که ساختار داده و مقادیر موردنیاز مشخص باشند.

اگر خوانایی کد با Destructuring بیش از حد کاهش پیدا کند، دسترسی مستقیم به Index ممکن است واضح‌تر باشد.

هدف Syntax مدرن، افزایش خوانایی و بیان بهتر Intent است؛ نه استفاده از Syntax در همه‌ی موقعیت‌ها.

Summary

در این فصل دیدیم که مدیریت Array فقط به Array Methodها محدود نمی‌شود.

با Destructuring می‌توانیم مقادیر Array را مستقیماً در Variableهای مختلف قرار دهیم:

const [first, second] = products;

با Rest می‌توانیم عناصر باقی‌مانده را جمع‌آوری کنیم:

const [first, ...remaining] = products;

و با Spread می‌توانیم عناصر یک Array را در ساختار جدید باز کنیم:

const copy = [...products];

Spread امکان Merge کردن Arrayها را نیز فراهم می‌کند:

const allProducts = [
...featured,
...newProducts
];

همچنین دیدیم که Spread یک Shallow Copy ایجاد می‌کند و برای Immutable Updateهای سطح Array بسیار کاربردی است.

در نهایت، Nested Destructuring به ما اجازه می‌دهد داده‌های تو در تو را نیز مستقیماً استخراج کنیم.

بنابراین:

Destructuring
→ Extract

Rest
→ Collect

Spread
→ Expand

Spread + New Array
→ Copy / Merge

Copy + Update
→ Immutable Pattern
Key Takeaways
Array Destructuring بر اساس Position کار می‌کند.
می‌توان با Destructuring برخی عناصر را نادیده گرفت.
می‌توان برای مقادیر undefined مقدار پیش‌فرض تعیین کرد.
Rest عناصر باقی‌مانده را جمع‌آوری می‌کند.
Spread عناصر را در یک ساختار جدید باز می‌کند.
Rest و Spread از یک Syntax استفاده می‌کنند، اما کاربرد متفاوتی دارند.
Spread برای Copy و Merge کردن Arrayها بسیار کاربردی است.
Spread یک Shallow Copy ایجاد می‌کند.
Copy کردن Array با Spread باعث ایجاد Array جدید می‌شود، اما Objectهای Nested همچنان Reference مشترک دارند.
Spread یکی از ابزارهای مهم برای Immutable Array Updates است.
Destructuring و Rest برای Extract کردن Data و Spread برای ساختن Structure جدید مناسب است.
Technical Interview
Junior
1. Array Destructuring چیست؟

روشی برای استخراج مقادیر Array و قرار دادن آن‌ها در Variableهای مختلف بر اساس Position است.

2. Rest در Array Destructuring چه کاری انجام می‌دهد؟

تمام عناصر باقی‌مانده‌ی Array را جمع‌آوری کرده و در یک Array جدید قرار می‌دهد.

3. Spread در Array چه کاری انجام می‌دهد؟

عناصر یک Array را در یک ساختار جدید باز می‌کند و امکان Copy یا Merge کردن Arrayها را فراهم می‌کند.

4. تفاوت Rest و Spread چیست؟

Rest مقادیر باقی‌مانده را جمع می‌کند، در حالی که Spread عناصر یک Structure را باز می‌کند.

5. چگونه یک Shallow Copy از Array ایجاد می‌کنیم؟

با Spread:

const copy = [...products];
Mid-Level
6. چرا const copy = products یک Copy واقعی ایجاد نمی‌کند؟

زیرا Array یک Object است و این Assignment فقط Reference همان Array را در Variable جدید قرار می‌دهد.

7. چرا Spread برای Immutable Update مناسب است؟

زیرا می‌توان با آن یک Array جدید ایجاد کرد و تغییر موردنظر را روی نسخه‌ی جدید اعمال کرد، بدون اینکه Array اصلی Mutation شود.

8. آیا Spread یک Deep Copy ایجاد می‌کند؟

خیر. Spread یک Shallow Copy ایجاد می‌کند. اگر Array شامل Objectهای Nested باشد، Objectهای داخلی همچنان Referenceهای قبلی هستند.

9. آیا ترتیب در Array Destructuring اهمیت دارد؟

بله. Array Destructuring بر اساس Position انجام می‌شود، بنابراین ترتیب عناصر و Variableها اهمیت دارد.

10. چه زمانی از filter() و چه زمانی از Rest استفاده می‌کنیم؟

filter() برای انتخاب عناصر بر اساس یک شرط استفاده می‌شود، در حالی که Rest برای جمع‌آوری عناصر باقی‌مانده در یک Destructuring Pattern به کار می‌رود.

Senior
11. چرا Rest و Spread با وجود Syntax یکسان، دو مفهوم متفاوت هستند؟

زیرا Context استفاده مشخص می‌کند که ... چه کاری انجام می‌دهد. در Destructuring، ... معمولاً برای جمع‌آوری مقادیر باقی‌مانده یعنی Rest استفاده می‌شود؛ در ساخت یک Structure جدید، ... عناصر موجود را باز می‌کند و Spread است.

12. مهم‌ترین محدودیت Spread برای Copy کردن Array چیست؟

Spread فقط یک Shallow Copy ایجاد می‌کند. بنابراین اگر Array شامل Object یا Arrayهای داخلی باشد، Reference آن داده‌های Nested حفظ می‌شود.

13. چرا Immutable Update در Applicationهای مدرن اهمیت دارد؟

زیرا به‌جای تغییر مستقیم Data موجود، نسخه‌ی جدیدی از آن ایجاد می‌شود. این الگو تشخیص تغییرات و مدیریت State را ساده‌تر و قابل‌پیش‌بینی‌تر می‌کند.

14. آیا Destructuring باعث ایجاد Copy از داده‌ها می‌شود؟

خود Destructuring صرفاً مقدار را از ساختار استخراج می‌کند. اگر مقدار استخراج‌شده یک Object یا Array باشد، Reference آن منتقل می‌شود و Deep Copy ایجاد نمی‌شود.

15. چه زمانی استفاده از Destructuring می‌تواند به خوانایی آسیب بزند؟

وقتی Pattern بیش از حد پیچیده و Nested شود و ساختار آن از خود مسئله دشوارتر شود. Destructuring باید برای بیان واضح Intent استفاده شود، نه صرفاً برای استفاده از Syntax مدرن.

Golden Answers

Array Destructuring چیست؟

روشی برای استخراج مقادیر Array بر اساس Position و قرار دادن آن‌ها در Variableهای مختلف است.

Rest چیست؟

Rest عناصر باقی‌مانده را جمع‌آوری می‌کند و آن‌ها را در یک Array قرار می‌دهد.

Spread چیست؟

Spread عناصر یک Iterable مانند Array را در یک Structure جدید باز می‌کند.

تفاوت Rest و Spread چیست؟

Rest مقادیر را جمع می‌کند؛ Spread مقادیر را باز می‌کند.

آیا Spread Deep Copy ایجاد می‌کند؟

خیر. Spread یک Shallow Copy ایجاد می‌کند و Reference داده‌های Nested را حفظ می‌کند.

چگونه Array را بدون Mutation کردن آن Copy کنیم؟

با Spread یک Array جدید ایجاد می‌کنیم: const copy = [...array].

چگونه دو Array را بدون تغییر Arrayهای اصلی Merge کنیم؟

با قرار دادن Spread هر دو Array در یک Array جدید:

const merged = [
...first,
...second
];

چرا Spread برای Immutable Update مهم است؟

چون امکان ایجاد یک Array جدید بر اساس داده‌ی قبلی را فراهم می‌کند و اجازه می‌دهد Update بدون Mutation کردن Array اصلی انجام شود.

Conclusion

در فصل‌های 35 و 36 یاد گرفتیم که چگونه Array را برای Data Processing، Search، Validation و Sorting به کار ببریم.

اما مدیریت حرفه‌ای Array فقط به Methodها محدود نمی‌شود.

گاهی مسئله این است که:

چگونه چند مقدار را از Array استخراج کنیم؟

Destructuring پاسخ این سؤال است.

گاهی می‌خواهیم:

Elementهای باقی‌مانده را نگه داریم.

اینجا Rest وارد می‌شود.

و گاهی هدف ما این است:

چگونه از داده‌ی موجود یک Array جدید بسازیم یا چند Array را ترکیب کنیم؟

در اینجا Spread کاربرد دارد.

مدل ذهنی نهایی این فصل:

Need values from Array
↓
Destructuring
Need remaining values
↓
Rest
Need to build a new Array
↓
Spread

و هنگامی که Spread را با ساخت یک Array جدید ترکیب می‌کنیم:

Existing Array
↓
Spread
↓
New Array
↓
Copy / Merge / Update

به این ترتیب Destructuring، Rest و Spread دیگر سه Syntax جدا از هم نیستند.

آن‌ها ابزارهایی برای Extract، Collect و Compose کردن داده‌های Array هستند و در کنار هم بخش مهمی از الگوهای مدرن مدیریت داده در JavaScript را تشکیل می‌دهند.