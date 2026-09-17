Chapter 19 — Objects Fundamentals
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

توضیح دهید چرا JavaScript به Objectها نیاز دارد.
Object را به‌عنوان ساختاری برای مدل‌سازی داده‌های مرتبط درک کنید.
رابطه میان Object، Property، Key و Value را توضیح دهید.
با Object Literal یک Object ایجاد کنید.
Propertyهای یک Object را با Dot Notation و Bracket Notation بخوانید.
تفاوت Property Access معمولی و Dynamic Property Access را تشخیص دهید.
Propertyهای Object را اضافه، به‌روزرسانی و حذف کنید.
تفاوت Mutation و Reassignment را توضیح دهید.
Method را در حد Function Property بشناسید.
رفتار Objectها به‌عنوان Reference Value را درک کنید.
توضیح دهید چرا Assignment یک Object مستقل ایجاد نمی‌کند.
Core Question

Object چگونه داده و رفتار مرتبط را در یک ساختار واحد سازمان‌دهی می‌کند؟

برای پاسخ به این سؤال، مسیر فصل را از خود مفهوم Object شروع می‌کنیم و مرحله‌به‌مرحله به Property، ساخت Object، دسترسی و تغییر آن، Method و در نهایت Reference می‌رسیم. این همان Concept Flow تعیین‌شده برای این فصل است.

مقدمه

تا اینجا بیشتر با Valueهای مستقل کار کرده‌ایم.

برای مثال:

const productName = 'Laptop';
const price = 1200;
const stock = 15;

این کد کاملاً معتبر است.

اما یک مسئله مهم وجود دارد.

این سه Value در دنیای واقعی متعلق به یک Entity هستند:

Product
├── name
├── price
└── stock

اگر Application ما فقط یک Product داشته باشد، نگهداری این Valueها در Variableهای جداگانه شاید مشکل بزرگی ایجاد نکند.

اما یک Application واقعی معمولاً با Entityهایی مانند:

User
Product
Order
Recipe
Account

کار می‌کند.

در این شرایط بهتر است داده‌های مرتبط را نیز در یک ساختار مرتبط نگهداری کنیم:

const product = {
name: 'Laptop',
price: 1200,
stock: 15
};

اکنون به‌جای سه Variable مستقل، یک Entity به نام product داریم.

این دقیقاً نقطه‌ای است که Object وارد مدل ذهنی ما می‌شود.

Object
چرا Object؟

Object قبل از اینکه یک Syntax باشد، یک مدل برای سازمان‌دهی داده است.

فرض کنید اطلاعات یک User را داریم:

const name = 'Omid';
const email = 'omid@example.com';
const role = 'admin';

این Variableها جدا هستند، درحالی‌که هر سه متعلق به یک User هستند.

با Object می‌توانیم این ارتباط را در ساختار داده نیز نشان دهیم:

const user = {
name: 'Omid',
email: 'omid@example.com',
role: 'admin'
};

اکنون:

user
├── name
├── email
└── role

داریم.

تعریف

Object ساختاری در JavaScript است که مجموعه‌ای از Propertyها را برای سازمان‌دهی و مدل‌سازی داده‌های مرتبط نگهداری می‌کند.

بنابراین Object را می‌توان به‌عنوان یک Entity در Application در نظر گرفت.

برای مثال:

const account = {
owner: 'Omid',
balance: 5000
};

این Object یک Account را مدل می‌کند.

Data Modeling با Object

یکی از مهم‌ترین کاربردهای Object، Data Modeling است.

یعنی داده‌های Application را مطابق Entityهای واقعی سازمان‌دهی کنیم.

برای مثال در یک Recipe Application:

const recipe = {
title: 'Pasta',
servings: 4,
cookingTime: 25
};

این Object یک Recipe را مدل می‌کند.

مدل ذهنی ما اکنون چنین است:

Recipe
│
├── title
├── servings
└── cookingTime

این نگاه به Object در پروژه‌های واقعی بسیار مهم‌تر از حفظ کردن Syntax آن است.

Property

اکنون یک Object داریم:

const user = {
name: 'Omid',
email: 'omid@example.com',
role: 'admin'
};

اما اجزای تشکیل‌دهنده این Object چه هستند؟

name، email و role را Property می‌نامیم.

تعریف

Property یک ویژگی نام‌گذاری‌شده از یک Object است که یک Value را نگهداری می‌کند.

در مثال بالا:

user
├── name
├── email
└── role

سه Property داریم.

هر Property دارای دو بخش اصلی است:

Key → Value
Key و Value

به این Object توجه کنید:

const product = {
name: 'Laptop',
price: 1200
};

در Property اول:

name → 'Laptop'

name یک Key است و 'Laptop' یک Value.

در Property دوم:

price → 1200

price Key و 1200 Value است.

بنابراین می‌توان Object را این‌گونه مدل کرد:

product
│
├── name  → 'Laptop'
│
└── price → 1200

این رابطه پایه‌ای Object است:

Object از Propertyها تشکیل می‌شود و هر Property یک Key و یک Value دارد.

Value می‌تواند چه چیزی باشد؟

Value یک Property می‌تواند Typeهای مختلفی داشته باشد:

const product = {
name: 'Laptop',
price: 1200,
available: true
};

در اینجا:

name      → String
price     → Number
available → Boolean

بنابراین Object محدود به یک نوع Value نیست.

حتی یک Property می‌تواند Function را به‌عنوان Value نگهداری کند:

const user = {
name: 'Omid',
login: function () {
console.log('Logged in');
}
};

در اینجا login یک Property است و Value آن یک Function است.

این ایده ما را به مفهوم Method می‌رساند که کمی بعد معرفی می‌کنیم.

Object Literal

اکنون که می‌دانیم Object چیست، باید ببینیم چگونه آن را ایجاد کنیم.

یکی از رایج‌ترین روش‌ها استفاده از Object Literal است.

تعریف

Object Literal Syntaxای برای ایجاد مستقیم یک Object با استفاده از {} است.

const product = {
name: 'Laptop',
price: 1200,
stock: 15
};

قسمت:

{
name: 'Laptop',
price: 1200,
stock: 15
}

Object Literal است.

ساختار کلی:

const objectName = {
key: value
};
چند Property

یک Object می‌تواند چند Property داشته باشد:

const order = {
id: 101,
customer: 'Omid',
total: 250,
paid: true
};

مدل ذهنی:

order
├── id
├── customer
├── total
└── paid

هر Property بخشی از همان Entity را توصیف می‌کند.

Object خالی

Object می‌تواند در ابتدا هیچ Propertyای نداشته باشد:

const product = {};

این یک Object معتبر است.

Propertyها می‌توانند بعداً به آن اضافه شوند:

product.name = 'Laptop';

این رفتار را در بخش Mutation بررسی خواهیم کرد.

Property Access

داشتن داده داخل Object کافی نیست.

باید بتوانیم به Propertyهای آن دسترسی پیدا کنیم.

JavaScript دو روش اصلی برای Property Access در اختیار ما قرار می‌دهد:

Dot Notation
Bracket Notation
Dot Notation
تعریف

در Dot Notation، نام Property بعد از . قرار می‌گیرد:

object.property

برای مثال:

const product = {
name: 'Laptop',
price: 1200
};

console.log(product.name);

خروجی:

Laptop

در:

product.name

JavaScript Propertyای به نام name را از Object product درخواست می‌کند.

مدل ذهنی:

product
↓
name
↓
'Laptop'
Bracket Notation

روش دوم Bracket Notation است:

object['property']

مثال:

const product = {
name: 'Laptop',
price: 1200
};

console.log(product['price']);

خروجی:

1200

برای Property ثابت، این دو روش می‌توانند نتیجه یکسانی داشته باشند:

product.name;

و:

product['name'];
Dynamic Property Access

تفاوت مهم Bracket Notation زمانی مشخص می‌شود که نام Property را در یک Variable داشته باشیم.

const product = {
name: 'Laptop',
price: 1200,
stock: 15
};

const field = 'price';

console.log(product[field]);

خروجی:

1200

چرا؟

چون مقدار field برابر 'price' است.

بنابراین:

product[field];

عملاً به این تبدیل مفهومی می‌شود:

product['price'];
تفاوت مهم

این دو کد را مقایسه کنید:

product.field;

و:

product[field];

در اولی JavaScript به دنبال Propertyای با نام دقیق field می‌گردد.

در دومی مقدار Variable به نام field بررسی می‌شود.

مثال:

const field = 'price';

console.log(product.field);
console.log(product[field]);

اگر Object Propertyای به نام field نداشته باشد، نتیجه اول undefined خواهد بود.

اما نتیجه دوم:

1200

خواهد بود.

قاعده عملی

Property ثابت → Dot Notation

product.price;

Property پویا → Bracket Notation

product[field];

این تفاوت یکی از مهم‌ترین نکات پایه‌ای Object Access است.

Mutation

تا اینجا Object را ایجاد کردیم و Propertyهای آن را خواندیم.

اما Object در یک Application معمولاً ثابت نمی‌ماند.

برای مثال:

قیمت Product تغییر می‌کند.
موجودی تغییر می‌کند.
وضعیت Order تغییر می‌کند.
اطلاعات User به‌روزرسانی می‌شود.

پس باید بتوانیم Object را تغییر دهیم.

این تغییر را Mutation می‌نامیم.

تعریف

Mutation یعنی تغییر مستقیم Object موجود، مانند اضافه کردن، به‌روزرسانی یا حذف Property.

افزودن Property

اگر Property وجود نداشته باشد، Assignment می‌تواند آن را ایجاد کند:

const product = {
name: 'Laptop'
};

product.price = 1200;

اکنون:

product
├── name  → 'Laptop'
└── price → 1200

داریم.

به‌روزرسانی Property

اگر Property از قبل وجود داشته باشد، Assignment مقدار آن را تغییر می‌دهد:

const product = {
name: 'Laptop',
price: 1200
};

product.price = 1100;

اکنون:

price → 1100

است.

حذف Property

برای حذف Property از delete استفاده می‌کنیم:

const product = {
name: 'Laptop',
price: 1200,
stock: 15
};

delete product.stock;

اکنون Property stock دیگر در Object وجود ندارد.

Mutation و const

یکی از نکات مهم این فصل ارتباط Object Mutation با const است.

این کد کاملاً معتبر است:

const product = {
name: 'Laptop',
price: 1200
};

product.price = 1100;

اما این کد معتبر نیست:

const product = {
name: 'Laptop'
};

product = {};

چرا؟

زیرا این دو عملیات متفاوت هستند.

در:

product.price = 1100;

ما Object را Mutation کرده‌ایم.

اما در:

product = {};

می‌خواهیم Variable product را به Object دیگری Reassign کنیم.

پس:

Mutation
→ تغییر Object

Reassignment
→ تغییر Value مربوط به Binding

این تفاوت با بحث const در فصل 04 مرتبط است؛ آنجا const را به‌عنوان Binding غیرقابل Reassign بررسی کردیم.

Method

Object می‌تواند علاوه بر Data، یک Behavior را نیز نگهداری کند.

قبلاً دیدیم که Property می‌تواند یک Function باشد:

const account = {
owner: 'Omid',

deposit: function () {
console.log('Deposit completed');
}
};

مدل ذهنی:

account
├── owner   → 'Omid'
└── deposit → Function

در اینجا deposit یک Property است که Function را به‌عنوان Value نگهداری می‌کند.

چنین Functionای در زمینه Object یک Method نامیده می‌شود.

تعریف

Method رفتاری مرتبط با یک Object است که در سطح پایه می‌توان آن را Function موجود در یک Property دانست.

بنابراین Object می‌تواند هم Data و هم Behavior مرتبط با آن Entity را در خود مدل کند:

Account
├── owner   → Data
├── balance → Data
└── deposit → Behavior
نکته مهم

در این فصل فقط مفهوم Method را معرفی می‌کنیم.

موضوعاتی مانند:

Method Invocation
this
Implicit Binding
تفاوت Arrow Function و Method

در Chapter 20 بررسی می‌شوند؛ زیرا Concept Flow آن فصل از Function Property به Method Invocation و سپس this می‌رسد.

Reference

اکنون به یکی از مهم‌ترین رفتارهای Objectها می‌رسیم.

این مثال را ببینید:

const user = {
name: 'Omid'
};

const anotherUser = user;

آیا اکنون دو Object داریم؟

خیر.

user و anotherUser به همان Object اشاره می‌کنند.

مدل ذهنی:

user ──────────┐
↓
┌──────────┐
│  Object  │
│   name   │
└──────────┘
↑
│
anotherUser ───┘
Object به‌عنوان Reference Value

در سطح این فصل، می‌توانیم Object را به‌عنوان یک Reference Value در نظر بگیریم.

وقتی می‌نویسیم:

const anotherUser = user;

Object موجود Copy نمی‌شود.

Variable جدید به همان Object اشاره می‌کند.

نتیجه Mutation
const user = {
name: 'Omid'
};

const anotherUser = user;

anotherUser.name = 'Ali';

console.log(user.name);

خروجی:

Ali

چرا؟

چون:

user ──────────┐
↓
Same Object
↑
anotherUser ───┘

وقتی از طریق anotherUser Object را تغییر دادیم، همان Objectی تغییر کرد که user نیز به آن اشاره می‌کند.

Reference در مقایسه با Primitive Values

برای درک بهتر، Primitive را مقایسه کنیم:

let score = 100;

let anotherScore = score;

anotherScore = 200;

console.log(score);

خروجی:

100

اما:

const user = {
score: 100
};

const anotherUser = user;

anotherUser.score = 200;

console.log(user.score);

خروجی:

200

در مثال Object، هر دو Variable به همان Object اشاره می‌کنند.

این تفاوت در کار با State و داده‌های Application اهمیت زیادی دارد.

Copying Preview

اگر بنویسیم:

const copy = original;

یک Copy مستقل ایجاد نمی‌کنیم.

برای ایجاد یک Object مستقل به تکنیک‌های Copying نیاز داریم.

یکی از Syntaxهای مدرن که در آینده با آن کار خواهیم کرد:

const copy = { ...original };

است.

اما Spread و مفهوم Shallow Copy در فصل مربوط به Rest and Spread Syntax بررسی خواهند شد؛ بنابراین در این فصل وارد جزئیات آن نمی‌شویم.

در این مرحله فقط این اصل را حفظ کنید:

Assignment یک Object را Copy نمی‌کند؛ یک Reference دیگر به همان Object ایجاد می‌کند.

Best Practices
1. Object را برای مدل‌سازی Entityهای مرتبط استفاده کنید

به‌جای پراکنده کردن داده‌های یک Entity در Variableهای مختلف:

const productName = 'Laptop';
const productPrice = 1200;
const productStock = 15;

داده مرتبط را در یک Object سازمان‌دهی کنید:

const product = {
name: 'Laptop',
price: 1200,
stock: 15
};
2. Property Nameهای معنادار انتخاب کنید

ترجیح دهید:

const product = {
price: 1200
};

به‌جای:

const product = {
x: 1200
};

نام Property باید معنای داده را مشخص کند.

3. برای Property ثابت از Dot Notation استفاده کنید
   product.price;

این Syntax معمولاً خواناتر است.

4. برای Property پویا از Bracket Notation استفاده کنید
   const field = 'price';

product[field];

وقتی نام Property در یک Variable یا Expression قرار دارد، Bracket Notation انتخاب مناسب است.

5. Mutation را آگاهانه انجام دهید

اگر چند بخش Application به یک Object دسترسی دارند، Mutation آن Object می‌تواند روی تمام Referenceهای دیگر نیز اثر بگذارد.

بنابراین قبل از تغییر Object باید بدانیم آیا Object به‌صورت مشترک استفاده می‌شود یا خیر.

6. تفاوت Mutation و Reassignment را همیشه تشخیص دهید
   product.price = 1000;

با:

product = {};

یکسان نیست.

اولی Object را تغییر می‌دهد؛ دومی Binding را تغییر می‌دهد.

اشتباهات رایج
اشتباه ۱: تصور اینکه Key و Value یک چیز هستند

در:

const product = {
price: 1200
};

price Key است و 1200 Value.

اشتباه ۲: اشتباه گرفتن Dot و Bracket در Dynamic Access

این:

const field = 'price';

product.field;

به Propertyای با نام field دسترسی دارد.

اما:

product[field];

از مقدار field استفاده می‌کند.

اشتباه ۳: تصور اینکه const Object را Immutable می‌کند

این کد معتبر است:

const user = {
name: 'Omid'
};

user.name = 'Ali';

const مانع Mutation نمی‌شود.

اشتباه ۴: تصور اینکه Assignment یک Object را Copy می‌کند
const copy = original;

Copy مستقل ایجاد نمی‌کند.

اشتباه ۵: یکی دانستن Mutation و Reassignment
product.price = 1000;

Mutation است.

product = {};

Reassignment است.

اشتباه ۶: وارد کردن this در همین مرحله

دیدن Function داخل Object ممکن است این سؤال را ایجاد کند:

this در این Function به چه چیزی اشاره می‌کند؟

این سؤال مهم است، اما پاسخ آن در Scope این فصل نیست.

در Chapter 20، Method Invocation و this به‌صورت مستقل بررسی خواهند شد.

Summary

در این فصل Object را به‌عنوان یکی از مهم‌ترین ابزارهای Data Modeling در JavaScript بررسی کردیم.

ابتدا دیدیم که Applicationها با Entityهایی مانند User، Product و Order کار می‌کنند و Object امکان سازمان‌دهی داده‌های مرتبط با این Entityها را فراهم می‌کند.

سپس Object را از طریق مفهوم Property بررسی کردیم.

هر Property دارای:

Key → Value

است.

بعد با Object Literal آشنا شدیم:

const product = {
name: 'Laptop',
price: 1200
};

سپس Property Access را بررسی کردیم.

برای Propertyهای ثابت:

product.price;

و برای دسترسی پویا:

product[field];

را به‌کار می‌بریم.

در ادامه Mutation را بررسی کردیم و دیدیم که می‌توان Property را:

اضافه کرد،
به‌روزرسانی کرد،
حذف کرد.

همچنین تفاوت Mutation و Reassignment را بررسی کردیم و دیدیم که const مانع تغییر Propertyهای Object نمی‌شود.

سپس Method را در سطح مقدماتی معرفی کردیم:

const account = {
deposit: function () {}
};

و در نهایت به Reference رسیدیم.

وقتی می‌نویسیم:

const anotherUser = user;

Object جدیدی ایجاد نمی‌شود؛ هر دو Variable به همان Object اشاره می‌کنند.

Key Takeaways
Object برای مدل‌سازی و سازمان‌دهی داده‌های مرتبط استفاده می‌شود.
Object از Propertyها تشکیل می‌شود.
هر Property دارای Key و Value است.
Object Literal با {} ایجاد می‌شود.
Dot Notation برای Property Access مستقیم مناسب است.
Bracket Notation امکان Dynamic Property Access را فراهم می‌کند.
Propertyها می‌توانند اضافه، به‌روزرسانی و حذف شوند.
Mutation با Reassignment یکسان نیست.
const مانع Mutation Object نمی‌شود.
Property می‌تواند یک Function باشد.
Function موجود در Property می‌تواند به‌عنوان Method شناخته شود.
جزئیات this و Method Invocation در فصل بعد است.
Assignment یک Object را Copy نمی‌کند.
چند Variable می‌توانند به یک Object واحد Reference داشته باشند.
Mutation از طریق یک Reference روی همان Object مشترک اثر می‌گذارد.
Technical Interview
Junior
1. Object چیست؟

Object ساختاری در JavaScript برای سازمان‌دهی مجموعه‌ای از Propertyهای مرتبط است و معمولاً برای مدل‌سازی Entityهای Application استفاده می‌شود.

2. Property چیست؟

Property یک ویژگی نام‌گذاری‌شده از Object است که یک Value را نگهداری می‌کند و دارای Key و Value است.

3. Object Literal چیست؟

Object Literal Syntaxای برای ایجاد مستقیم Object با استفاده از {} است.

4. تفاوت Dot Notation و Bracket Notation چیست؟

Dot Notation برای دسترسی مستقیم به یک Property استفاده می‌شود:

product.price;

Bracket Notation می‌تواند نام Property را به‌صورت پویا مشخص کند:

product[field];
5. چگونه Property جدید اضافه می‌کنیم؟

با Assignment:

product.stock = 15;
6. چگونه Property را حذف می‌کنیم؟

با delete:

delete product.stock;
7. آیا Object تعریف‌شده با const قابل تغییر است؟

بله. const مانع Mutation Object نمی‌شود.

Mid-Level
8. چرا product[field] با product.field متفاوت است؟

زیرا در product.field نام Property مستقیماً field است، اما در product[field] مقدار Expression داخل Bracket به‌عنوان نام Property استفاده می‌شود.

9. Mutation چیست؟

Mutation تغییر مستقیم Object موجود است؛ مانند اضافه کردن، Update کردن یا حذف Property.

10. آیا این کد Object را Copy می‌کند؟
    const copy = original;

خیر. Variable copy به همان Object اشاره می‌کند.

11. چرا تغییر copy روی original نیز اثر می‌گذارد؟

زیرا هر دو Variable به همان Object Reference دارند.

12. تفاوت Mutation و Reassignment چیست؟

Mutation محتوای Object را تغییر می‌دهد:

user.name = 'Ali';

Reassignment Variable را به Value دیگری متصل می‌کند:

user = {};
13. Method در این فصل چگونه تعریف می‌شود؟

Method در سطح پایه Functionای است که به‌عنوان Value یک Property در Object قرار گرفته و رفتار مرتبط با Object را مدل می‌کند.

Senior
14. چرا Reference Semantics در طراحی Application مهم است؟

زیرا چند بخش Application می‌توانند به یک Object واحد دسترسی داشته باشند. Mutation از یک مسیر می‌تواند State مشاهده‌شده در مسیر دیگر را نیز تغییر دهد. بنابراین هنگام طراحی State و اشتراک داده باید مالکیت و Mutation را آگاهانه مدیریت کرد.

15. چرا Assignment یک Object را Copy نمی‌کند؟

چون Assignment در این حالت Reference به Object موجود را در Variable دیگر قرار می‌دهد؛ در نتیجه دو Variable می‌توانند به یک Object واحد اشاره کنند.

16. تفاوت این دو چیست؟
    const a = object;

و:

const a = { ...object };

در اولی a به همان Object اشاره می‌کند.

در دومی یک Object جدید با استفاده از Spread ساخته می‌شود. جزئیات Shallow Copy و Spread در فصل Rest and Spread Syntax بررسی خواهد شد.

17. چرا Dynamic Property Access برای Applicationهای واقعی مهم است؟

زیرا در بسیاری از سناریوها نام Property در زمان نوشتن کد ثابت نیست و می‌تواند در یک Variable قرار داشته باشد؛ برای مثال زمانی که Field موردنظر بر اساس Configuration یا ورودی برنامه انتخاب می‌شود.

18. چرا Object را نباید فقط یک Syntax بدانیم؟

زیرا Object بخشی از Data Model برنامه است. انتخاب Object و Propertyهای آن مشخص می‌کند Entityهای Application چگونه در Code نمایش داده و سازمان‌دهی شوند.

19. اگر دو Variable به یک Object اشاره کنند، آیا دو Object داریم؟

خیر. دو Variable داریم اما یک Object مشترک داریم:

a ─────┐
↓
Object
↑
b ─────┘
20. چرا const با Mutation Object تناقض ندارد؟

زیرا const Binding را غیرقابل Reassign می‌کند، نه اینکه Object مورد اشاره را Immutable کند.

Golden Answers
Object چیست؟

Object ساختاری برای سازمان‌دهی Propertyهای مرتبط و مدل‌سازی Entityهای Application است.

Property چیست؟

Property یک ویژگی نام‌گذاری‌شده Object است که یک Key و یک Value دارد.

Dot و Bracket Notation چه تفاوتی دارند؟

Dot Notation برای دسترسی مستقیم به Property استفاده می‌شود، درحالی‌که Bracket Notation امکان Dynamic Property Access را فراهم می‌کند.

Mutation چیست؟

Mutation یعنی تغییر مستقیم Object موجود، مانند Add، Update یا Delete کردن Property.

آیا const Object را Immutable می‌کند؟

خیر. const فقط Reassignment Binding را محدود می‌کند و مانع Mutation Propertyهای Object نمی‌شود.

آیا Assignment Object را Copy می‌کند؟

خیر. Assignment یک Reference به همان Object را در Variable دیگر قرار می‌دهد.

Method چیست؟

Method در سطح پایه Functionای است که به‌عنوان Value یک Property در Object قرار گرفته و رفتار مرتبط با Object را مدل می‌کند.

چرا Reference مهم است؟

چون چند Variable می‌توانند به یک Object واحد اشاره کنند و Mutation از طریق یکی از آن‌ها روی همان Object مشترک اثر می‌گذارد.

Conclusion

در این فصل از یک مسئله ساده شروع کردیم:

چگونه داده‌های مرتبط یک Entity را در یک ساختار واحد سازمان‌دهی کنیم؟

پاسخ ما Object بود.

سپس این مفهوم را مرحله‌به‌مرحله گسترش دادیم:

Object
↓
Property
↓
Key / Value
↓
Object Literal
↓
Property Access
↓
Mutation
↓
Method
↓
Reference

این مسیر دقیقاً همان Concept Flow تعیین‌شده برای Chapter 19 است.

در پایان فصل، Object دیگر صرفاً مجموعه‌ای از {} و Propertyها نیست.

اکنون باید آن را به‌عنوان یک Entity قابل مدل‌سازی ببینید:

Object
├── Data
│   ├── Property
│   ├── Key
│   └── Value
│
├── Access
│
├── Mutation
│
├── Behavior
│   └── Method
│
└── Reference

و این دقیقاً نقطه‌ای است که فصل بعدی از آن ادامه پیدا می‌کند.

در Chapter 20 — Object Methods and this، مفهوم Method را از Function Property یک مرحله جلوتر می‌بریم و بررسی می‌کنیم که Method چگونه فراخوانی می‌شود، this چگونه تعیین می‌شود و چرا نوع Function می‌تواند رفتار متفاوتی ایجاد کند.