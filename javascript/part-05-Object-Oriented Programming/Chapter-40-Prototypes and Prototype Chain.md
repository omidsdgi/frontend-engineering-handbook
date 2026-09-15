Chapter 28 — Prototypes and Prototype Chain

اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

مفهوم Prototype را دقیقاً توضیح دهید.

توضیح دهید چرا JavaScript برای Objects از Prototype استفاده می‌کند.

تفاوت prototype و [[Prototype]] را تشخیص دهید.

رابطه میان یک Object و Prototype آن را توضیح دهید.

Shared Methods را روی Prototype قرار دهید.

فرآیند Property Lookup را مرحله‌به‌مرحله تحلیل کنید.

مفهوم Prototype Chain را توضیح دهید.

Own و Inherited Properties را از یکدیگر تشخیص دهید.

نقش Prototypeهای Built-in مانند Object.prototype، Array.prototype و String.prototype را درک کنید.

Core Question

JavaScript چگونه از طریق Prototypeها رفتار و Properties را بین Objects به اشتراک می‌گذارد؟

مقدمه

در فصل قبل دیدیم که می‌توانیم با Constructor Functions چند Object مشابه ایجاد کنیم.

برای مثال:

function User(name) {
this.name = name;
}

const user1 = new User('Omid');
const user2 = new User('Sara');

اکنون دو Instance داریم:

user1
└── name: "Omid"

user2
└── name: "Sara"

اما فرض کنید هر User باید بتواند Login کند.

یک راه این است که Method را داخل Constructor قرار دهیم:

function User(name) {
this.name = name;

this.login = function () {
console.log(`${this.name} logged in`);
};
}

در این حالت هر بار که Constructor اجرا می‌شود، یک Function جدید برای login ایجاد می‌شود.

برای دو User:

user1
├── name
└── login → Function A

user2
├── name
└── login → Function B

اما رفتار login برای هر دو User یکسان است.

پس سؤال مهمی ایجاد می‌شود:

آیا لازم است هر Instance نسخه جداگانه‌ای از یک Behavior مشترک داشته باشد؟

JavaScript برای چنین مسئله‌ای مکانیزم Prototype را در اختیار ما قرار می‌دهد.

ایده اصلی ساده است:

Instance
↓
Shared Prototype
↓
Shared Behavior

به‌جای اینکه Behavior مشترک را داخل هر Object تکرار کنیم، می‌توانیم آن را در یک Object مشترک قرار دهیم.

این نقطه شروع فهم Prototype است.

Prototype

چرا به Prototype نیاز داریم؟

فرض کنید یک Application فروشگاهی داریم و هزار Product ایجاد می‌کنیم.

همه Productها ممکن است Behavior مشترکی مانند getPrice() داشته باشند.

اگر این Method برای هر Instance به‌صورت جداگانه ساخته شود، همان منطق بارها تکرار می‌شود.

اما اگر Behavior مشترک را در یک Object قرار دهیم، همه Instanceها می‌توانند از همان Behavior استفاده کنند.

آن Object همان Prototype است.

تعریف ساده

Prototype یک Object است که می‌تواند منبع Properties و Methods برای Object دیگری باشد.

اگر JavaScript Property موردنظر را روی خود Object پیدا نکند، می‌تواند Prototype آن را بررسی کند.

تعریف فنی

هر Object در JavaScript می‌تواند یک رابطه داخلی با Object دیگری داشته باشد که در مدل زبان با Internal Slot زیر نمایش داده می‌شود:

[[Prototype]]

این Internal Slot رابطه Prototype را مشخص می‌کند.

برای مشاهده Prototype یک Object از:

Object.getPrototypeOf(object);

استفاده می‌کنیم.

یک مثال ساده

const userPrototype = {
login() {
console.log(`${this.name} logged in`);
}
};

const user = Object.create(userPrototype);

user.name = 'Omid';

اکنون رابطه مفهومی چنین است:

user
├── name: "Omid"
│
└── [[Prototype]]
↓
userPrototype
└── login()

login مستقیماً روی user قرار ندارد.

اما:

user.login();

می‌تواند آن را پیدا و اجرا کند.

این اولین نکته مهم Prototype است:

Object می‌تواند به Behaviorای دسترسی داشته باشد که مستقیماً روی خودش قرار ندارد.

Prototype Relationship

اکنون باید دقیق‌تر ببینیم Object چگونه به Prototype خود مرتبط می‌شود.

Object.getPrototypeOf(user);

Prototype مربوط به user را برمی‌گرداند.

بنابراین:

Object.getPrototypeOf(user) === userPrototype;

نتیجه:

true

رابطه را می‌توان این‌گونه تصور کرد:

user
│
│ [[Prototype]]
↓
userPrototype

این رابطه Prototype Relationship است.

نکته مهم این است که Prototype یک Copy از Object نیست.

user همچنان Object خودش است و userPrototype نیز Object جداگانه‌ای است.

prototype در Constructor Functions

تا اینجا Prototype را به‌صورت مستقیم با Object.create() دیدیم.

اما در فصل قبل با Constructor Functions کار کردیم.

در اینجا یک مفهوم مهم دیگر ظاهر می‌شود:

User.prototype

فرض کنید:

function User(name) {
this.name = name;
}

Function User دارای Propertyای به نام:

User.prototype

است.

این Property یک Object است که در Constructor Pattern می‌تواند Prototype Instanceهایی باشد که با new User() ساخته می‌شوند.

پس در یک مثال معمولی:

const user = new User('Omid');

رابطه مفهومی چنین است:

User
└── prototype
↓
Prototype Object
↑
│
user.[[Prototype]]

بنابراین:

Object.getPrototypeOf(user) === User.prototype;

نتیجه:

true

یک تفاوت بسیار مهم

اکنون دو مفهوم شبیه به هم داریم:

User.prototype

و:

Object.getPrototypeOf(user)

این دو را نباید یکی بدانیم.

User.prototype یک Property روی Function به نام User است.

در مقابل:

Object.getPrototypeOf(user)

Prototype مربوط به Object user را مشاهده می‌کند.

در Constructor Pattern معمولی، این دو می‌توانند به همان Object اشاره کنند:

User.prototype
↑
│
user.[[Prototype]]

اما نقش مفهومی آن‌ها متفاوت است.

Shared Methods

اکنون می‌توانیم مشکل ابتدای فصل را حل کنیم.

به‌جای:

function User(name) {
this.name = name;

this.login = function () {
console.log(`${this.name} logged in`);
};
}

می‌توانیم Method مشترک را روی Prototype قرار دهیم:

function User(name) {
this.name = name;
}

User.prototype.login = function () {
console.log(`${this.name} logged in`);
};

اکنون:

const user1 = new User('Omid');
const user2 = new User('Sara');

ساختار مفهومی:

user1
├── name: "Omid"
└── [[Prototype]]
↓
User.prototype
└── login()


user2
├── name: "Sara"
└── [[Prototype]]
↓
User.prototype
└── login()

هر دو User به یک Method مشترک دسترسی دارند.

بررسی عملی

می‌توانیم این موضوع را با مقایسه Referenceها ببینیم:

console.log(user1.login === user2.login);

نتیجه:

true

زیرا Lookup برای هر دو Instance در نهایت به همان Function در User.prototype می‌رسد.

پس:

Shared Method روی Prototype قرار می‌گیرد و Instanceها از طریق Prototype Relationship به آن دسترسی پیدا می‌کنند.

چرا this همچنان درست کار می‌کند؟

ممکن است این سؤال ایجاد شود:

اگر login روی User.prototype قرار دارد، چرا:

this.name

برای هر User مقدار متفاوتی دارد؟

به Invocation توجه کنید:

user1.login();

Method از Prototype پیدا شده است، اما Invocation از طریق user1 انجام شده است.

بنابراین در این حالت:

this → user1

و:

this.name

به:

user1.name

اشاره می‌کند.

برای:

user2.login();

همین منطق برقرار است:

this → user2

بنابراین یک Method مشترک می‌تواند با State مربوط به Instanceهای مختلف کار کند.

Property Lookup

اکنون سؤال مهم‌تری داریم:

وقتی می‌نویسیم:

user1.login();

JavaScript دقیقاً login را کجا پیدا می‌کند؟

این همان Property Lookup است.

فرآیند ساده:

1. Object را بررسی کن
   ↓
2. اگر پیدا نشد، Prototype را بررسی کن
   ↓
3. اگر پیدا نشد، Prototype بعدی را بررسی کن
   ↓
4. تا پیدا شدن Property یا رسیدن به null ادامه بده

مثال

function User(name) {
this.name = name;
}

User.prototype.login = function () {
console.log(`${this.name} logged in`);
};

const user = new User('Omid');

اکنون:

user.name;

در خود user پیدا می‌شود.

اما:

user.login;

در خود user وجود ندارد.

JavaScript Prototype مربوط به user را بررسی می‌کند:

User.prototype

و login را پیدا می‌کند.

پس:

user.login();

می‌تواند اجرا شود.

Property Lookup فقط برای Method نیست

این مکانیزم فقط برای Functionها نیست.

مثلاً اگر:

const productPrototype = {
category: 'General'
};

const laptop = Object.create(productPrototype);

laptop.name = 'Laptop';

اکنون:

laptop.category;

نیز Property Lookup انجام می‌دهد.

JavaScript ابتدا laptop را بررسی می‌کند.

category وجود ندارد.

سپس Prototype را بررسی می‌کند.

در Prototype:

category: "General"

پیدا می‌شود.

پس مقدار برگردانده می‌شود.

این نکته مهم است:

Prototype Chain بخشی از مکانیزم عمومی Property Lookup است؛ نه فقط Lookup برای Methodها.

Prototype Chain

تا اینجا یک رابطه داشتیم:

user
↓
User.prototype

اما Prototype خودش نیز یک Object است.

بنابراین آن Object نیز می‌تواند Prototype داشته باشد.

برای مثال:

user
↓
User.prototype
↓
Object.prototype
↓
null

این زنجیره را Prototype Chain می‌نامیم.

چرا Prototype Chain لازم است؟

فرض کنید:

user.login();

login روی user نیست.

JavaScript به:

User.prototype

می‌رود و آنجا login را پیدا می‌کند.

اما فرض کنید:

user.toString();

را اجرا کنیم.

ممکن است toString روی:

user

وجود نداشته باشد.

روی:

User.prototype

نیز ممکن است وجود نداشته باشد.

JavaScript Lookup را ادامه می‌دهد:

user
↓
User.prototype
↓
Object.prototype

در Object.prototype رفتار مربوط به toString را پیدا می‌کند.

بنابراین Method می‌تواند از Prototype بالاتری در Chain در دسترس باشد.

مدل ذهنی Property Lookup

از اینجا به بعد می‌توانیم Property Access را به شکل زیر تصور کنیم:

user.someProperty
↓
user
│
پیدا شد؟
/    \
Yes     No
↓        ↓
Result   Prototype
│
پیدا شد؟
/    \
Yes     No
↓        ↓
Result   Next Prototype
│
↓
...
│
↓
null

اگر Property در هیچ‌کدام از Objectهای Chain پیدا نشود، Lookup موفق نیست.

برای Property Access معمولی نتیجه می‌تواند:

undefined

باشد.

Own Properties و Inherited Properties

اکنون دو مفهوم مهم را می‌توانیم از هم جدا کنیم.

Own Property

Propertyای که مستقیماً روی خود Object قرار دارد، Own Property است.

در مثال:

const user = new User('Omid');

Property:

user.name

یک Own Property است.

می‌توانیم آن را بررسی کنیم:

user.hasOwnProperty('name');

نتیجه:

true

Inherited Property

اگر Property روی خود Object نباشد اما از طریق Prototype Chain قابل دسترسی باشد، آن Property را Inherited Property در نظر می‌گیریم.

در مثال:

user.login();

login روی:

User.prototype

قرار دارد، نه روی user.

بنابراین:

user.hasOwnProperty('login');

نتیجه:

false

اما:

user.login();

همچنان کار می‌کند.

این دقیقاً تفاوت میان:

وجود داشتن Property

و:

Own بودن Property

است.

Shadowing

فرض کنید Prototype یک Property دارد:

const productPrototype = {
category: 'General'
};

const laptop = Object.create(productPrototype);

اکنون:

laptop.category;

مقدار:

General

دارد.

اما اگر روی خود Object Propertyای با همان نام ایجاد کنیم:

laptop.category = 'Electronics';

اکنون:

laptop.category;

مقدار:

Electronics

دارد.

چرا؟

چون Lookup ابتدا خود laptop را بررسی می‌کند:

laptop
↓
category پیدا شد

در نتیجه دیگر به Prototype نمی‌رود.

Prototype همچنان مقدار قبلی را دارد:

productPrototype.category;

نتیجه:

General

این وضعیت را Property Shadowing می‌نامیم.

Built-in Prototypes

Prototype فقط برای Objectهایی که خودمان طراحی می‌کنیم نیست.

JavaScript برای بسیاری از Built-in Objectها نیز Prototypeهای مخصوص دارد.

سه نمونه مهم:

Object.prototype
Array.prototype
String.prototype

Object.prototype

یک Object معمولی مانند:

const user = {
name: 'Omid'
};

در حالت معمول Prototype مربوط به:

Object.prototype

است.

می‌توانیم بررسی کنیم:

Object.getPrototypeOf(user) === Object.prototype;

نتیجه:

true

بنابراین:

user
↓
Object.prototype
↓
null

بخشی از رفتار مشترک Objectها از این Prototype در دسترس قرار می‌گیرد.

Array.prototype

فرض کنید:

const products = ['Laptop', 'Mouse'];

Array دارای Prototype مخصوص خود است:

Array.prototype

بنابراین Arrayها می‌توانند از Behaviorهای مشترک Array استفاده کنند.

به‌صورت مفهومی:

products
↓
Array.prototype
↓
Object.prototype
↓
null

برای مثال:

products.map(product => product.toUpperCase());

map یک Behavior مربوط به Array است و از طریق Prototype در دسترس Arrayها قرار می‌گیرد.

String.prototype

Stringها نیز Prototype مخصوص خود را دارند.

مثلاً:

const title = 'JavaScript';

و:

title.toUpperCase();

Behaviorهای مشترک String در:

String.prototype

قرار دارند.

مدل مفهومی:

String
↓
String.prototype
↓
Object.prototype
↓
null

Prototype Chain در Built-in Objects

اکنون می‌توانیم تصویر کامل‌تری داشته باشیم.

برای یک Array معمولی:

array
↓
Array.prototype
↓
Object.prototype
↓
null

برای یک Object معمولی:

object
↓
Object.prototype
↓
null

این نشان می‌دهد Prototype Chain فقط بخشی از Constructor Pattern نیست.

این مکانیزم در مدل Objectهای JavaScript به‌صورت گسترده وجود دارد.

یک مثال کامل

اکنون تمام مفاهیم اصلی فصل را در یک مثال ترکیب کنیم:

function Product(name, price) {
this.name = name;
this.price = price;
}

Product.prototype.getPrice = function () {
return this.price;
};

Product.prototype.describe = function () {
return `${this.name}: $${this.price}`;
};

const laptop = new Product('Laptop', 1200);
const mouse = new Product('Mouse', 50);

ساختار مفهومی:

laptop
├── name: "Laptop"
├── price: 1200
└── [[Prototype]]
↓
Product.prototype
├── getPrice()
└── describe()
↓
Object.prototype
↓
null


mouse
├── name: "Mouse"
├── price: 50
└── [[Prototype]]
↓
Product.prototype
├── getPrice()
└── describe()
↓
Object.prototype
↓
null

اکنون:

laptop.getPrice();

فرآیند Lookup:

laptop
↓
getPrice پیدا نشد
↓
Product.prototype
↓
getPrice پیدا شد

و:

laptop.toString();

فرآیند Lookup:

laptop
↓
Product.prototype
↓
Object.prototype
↓
toString پیدا شد

این دقیقاً همان Prototype Chain است.

Best Practices

1. Shared Behavior را روی Prototype قرار دهید

اگر Behavior میان Instanceها مشترک است، قرار دادن آن روی Prototype از ایجاد نسخه‌های مستقل جلوگیری می‌کند.

Product.prototype.getPrice = function () {
return this.price;
};

2. prototype و [[Prototype]] را جدا نگه دارید

این دو مفهوم را در ذهن یکی نکنید:

Product.prototype

در برابر:

Object.getPrototypeOf(product)

3. برای مشاهده Prototype از API مناسب استفاده کنید

برای بررسی Prototype:

Object.getPrototypeOf(object);

را به‌عنوان API استاندارد و خوانا در نظر بگیرید.

4. Own و Inherited Properties را در Debugging تشخیص دهید

اگر Object رفتاری دارد که مستقیماً روی آن تعریف نشده است، Prototype Chain را بررسی کنید.

5. Prototype را بدون دلیل تغییر ندهید

Prototype Relationship بهتر است بخشی روشن و پایدار از طراحی Object باشد.

تغییر مکرر Prototype می‌تواند کد را پیچیده‌تر کند و نگهداری آن را دشوار سازد.

Common Mistakes

اشتباه اول: یکی دانستن prototype و [[Prototype]]

این دو یکی نیستند.

User.prototype

یک Property روی Function است.

در حالی که:

Object.getPrototypeOf(user)

Prototype مربوط به Object user را مشاهده می‌کند.

اشتباه دوم: تصور اینکه Prototype یک Copy از Object است

Prototype یک Object جداگانه است.

Instance به Prototype خود مرتبط است و Property Lookup می‌تواند در آن ادامه پیدا کند.

اشتباه سوم: تصور اینکه Methodهای Prototype روی هر Instance کپی می‌شوند

در حالت معمول Method کپی نمی‌شود.

Instance از طریق Prototype Chain به همان Method دسترسی پیدا می‌کند.

اشتباه چهارم: تصور اینکه Prototype فقط برای Methodهاست

Prototype می‌تواند Data Property نیز داشته باشد.

مثلاً:

const defaults = {
currency: 'USD'
};

اشتباه پنجم: تصور اینکه Callback یا Function بودن Method، Prototype را تغییر می‌دهد

Prototype به محل و رابطه Object مربوط است.

Function بودن Value باعث نمی‌شود Prototype مفهوم متفاوتی پیدا کند.

اشتباه ششم: تصور اینکه Property حتماً باید Own باشد تا قابل استفاده باشد

Property می‌تواند از Prototype Chain پیدا شود و همچنان قابل استفاده باشد.

user.toString();

اشتباه هفتم: تصور اینکه Prototype Chain فقط یک مرحله دارد

ممکن است Lookup از چند Prototype عبور کند:

Object
↓
Prototype
↓
Prototype
↓
null

Summary

در فصل قبل دیدیم که Constructor Function می‌تواند چند Instance مشابه ایجاد کند.

اما یک مشکل وجود داشت:

Behavior مشترک را کجا قرار دهیم؟

Prototype پاسخ این مسئله است.

Prototype یک Object است که می‌تواند منبع Properties و Methods برای Object دیگری باشد.

هر Object می‌تواند رابطه‌ای داخلی با Prototype خود داشته باشد که با:

[[Prototype]]

مدل می‌شود.

در Constructor Pattern، Function دارای Propertyای به نام:

Constructor.prototype

است.

Instanceهایی که با new ساخته می‌شوند، در حالت معمول به آن Prototype متصل می‌شوند.

بنابراین:

Instance
↓
Constructor.prototype

رابطه‌ای اساسی در این الگو است.

وقتی Behavior مشترکی داریم، می‌توانیم آن را روی Prototype قرار دهیم:

Product.prototype.getPrice = function () {
return this.price;
};

در این حالت Instanceها Function را به‌صورت مستقل دریافت نمی‌کنند؛ بلکه هنگام Property Lookup آن را از Prototype پیدا می‌کنند.

Property Lookup ابتدا Object را بررسی می‌کند.

اگر Property پیدا نشود، Prototype بررسی می‌شود.

اگر آنجا نیز پیدا نشود، Lookup در Prototype بعدی ادامه پیدا می‌کند.

این زنجیره:

Object
↓
Prototype
↓
Prototype
↓
null

همان Prototype Chain است.

در این مدل باید بین:

Own Property

و:

Inherited Property

تفاوت قائل شویم.

Own Property مستقیماً روی Object قرار دارد.

Inherited Property از طریق Prototype Chain قابل دسترسی است.

همچنین اگر Object Propertyای با همان نام Prototype ایجاد کند، Property روی Object در Lookup اولویت پیدا می‌کند و Prototype Property را Shadow می‌کند.

این مدل فقط برای Objectهای ساخته‌شده توسط ما نیست.

Built-in Objectها نیز Prototype دارند:

Array.prototype
String.prototype
Object.prototype

بنابراین Prototype بخشی بنیادی از مدل Object در JavaScript است.

Key Takeaways

Prototype یک Object است.

Object می‌تواند از طریق [[Prototype]] به Prototype خود مرتبط باشد.

[[Prototype]] یک Internal Slot است.

prototype یک Property معمولی روی Function است.

prototype و [[Prototype]] یک مفهوم نیستند.

Object.getPrototypeOf() برای مشاهده Prototype استفاده می‌شود.

Constructor Function دارای Propertyای به نام prototype است.

Instance ساخته‌شده با new در حالت معمول به Constructor.prototype متصل می‌شود.

Shared Methods را می‌توان روی Prototype قرار داد.

Methodهای Prototype معمولاً برای هر Instance کپی نمی‌شوند.

Property Lookup ابتدا Object را بررسی می‌کند.

اگر Property پیدا نشود، Prototype بررسی می‌شود.

Lookup می‌تواند در چند Prototype ادامه پیدا کند.

این زنجیره Prototype Chain نام دارد.

Prototype Chain در نهایت به null می‌رسد.

Own Property مستقیماً روی Object قرار دارد.

Inherited Property از Prototype Chain قابل دسترسی است.

Property روی Object می‌تواند Property موجود در Prototype را Shadow کند.

Prototype فقط برای Methods نیست و می‌تواند Data Properties نیز داشته باشد.

Object.prototype، Array.prototype و String.prototype نمونه‌هایی از Built-in Prototypes هستند.

Prototype بخشی بنیادی از Object Model و Property Lookup در JavaScript است.

Technical Interview

Junior

1. Prototype چیست؟

Prototype یک Object است که می‌تواند منبع Properties و Methods برای Object دیگری باشد. اگر Property روی Object پیدا نشود، JavaScript می‌تواند Prototype آن را بررسی کند.

2. Prototype Chain چیست؟

Prototype Chain زنجیره‌ای از روابط Prototype است که JavaScript هنگام Property Lookup از آن استفاده می‌کند.

3. اگر Property روی Object وجود نداشته باشد چه اتفاقی می‌افتد؟

JavaScript Prototype را بررسی می‌کند. اگر Property در آنجا نیز وجود نداشته باشد، Lookup در Prototype بعدی ادامه پیدا می‌کند.

4. Own Property و Inherited Property چه تفاوتی دارند؟

Own Property مستقیماً روی Object قرار دارد؛ Inherited Property از طریق Prototype Chain پیدا می‌شود.

5. چرا Shared Methods را روی Prototype قرار می‌دهیم؟

زیرا چند Instance می‌توانند از یک Method مشترک استفاده کنند، بدون اینکه برای هر Instance نسخه مستقلی از آن Method ایجاد شود.

6. چگونه Prototype یک Object را مشاهده کنیم؟

با:

Object.getPrototypeOf(object);

7. آیا Method روی Prototype به Instance کپی می‌شود؟

خیر. Instance از طریق Prototype Chain به Method دسترسی پیدا می‌کند.

Mid-Level

8. تفاوت User.prototype و Object.getPrototypeOf(user) چیست؟

User.prototype یک Property روی Function User است. Object.getPrototypeOf(user) Prototype مربوط به Object user را برمی‌گرداند. در Constructor Pattern معمولی این دو می‌توانند به یک Object اشاره کنند.

9. Property Lookup چگونه انجام می‌شود؟

JavaScript ابتدا Object را بررسی می‌کند. اگر Property پیدا نشود، Prototype مستقیم را بررسی می‌کند و در صورت نیاز در Prototype Chain ادامه می‌دهد تا Property پیدا شود یا به null برسد.

10. چرا user1.login === user2.login می‌تواند true باشد؟

اگر login روی Prototype قرار گرفته باشد، هر دو Instance هنگام Lookup به همان Function موجود در Prototype می‌رسند.

11. Shadowing چیست؟

وقتی Object و Prototype هر دو Propertyای با یک نام داشته باشند، Property موجود روی خود Object در Lookup اولویت دارد و Property Prototype را Shadow می‌کند.

12. آیا Prototype فقط برای Methods استفاده می‌شود؟

خیر. Prototype می‌تواند Data Properties نیز داشته باشد.

13. چرا Arrayها می‌توانند map() را اجرا کنند؟

زیرا Arrayها به Array.prototype متصل هستند و Behaviorهای مشترک Array از طریق آن در دسترس قرار می‌گیرند.

14. چرا hasOwnProperty('login') ممکن است false باشد ولی user.login() کار کند؟

زیرا login Own Property نیست و از Prototype Chain پیدا می‌شود.

Senior

15. چرا Prototype را نباید صرفاً یک Memory Optimization دانست؟

زیرا Prototype بخش بنیادی Object Model در JavaScript است. Property Lookup، Shared Behavior و دسترسی به بسیاری از Built-in Methods بر اساس همین مدل انجام می‌شود.

16. چرا تفاوت prototype و [[Prototype]] از نظر مهندسی مهم است؟

زیرا این دو در دو سطح متفاوت قرار دارند. prototype یک Property روی Function است، در حالی که [[Prototype]] رابطه داخلی یک Object با Prototype آن را مدل می‌کند. اشتباه گرفتن این دو باعث برداشت نادرست از رابطه Constructor و Instance می‌شود.

17. چرا Property Lookup را می‌توان یک Delegation Mechanism دانست؟

زیرا Object در صورت پیدا نکردن Property، Lookup را به Prototype خود واگذار می‌کند و این فرآیند می‌تواند در Prototype Chain ادامه پیدا کند.

18. چرا Shared Methods روی Prototype با this مربوط به Instance کار می‌کنند؟

زیرا محل قرار گرفتن Function و Object مربوط به Method Invocation دو مفهوم متفاوت‌اند. در:

user.login();

Method ممکن است از Prototype پیدا شود، اما Invocation از طریق user انجام می‌شود؛ بنابراین this در این Method به user مربوط می‌شود.

19. چرا Shadowing در Prototype Chain اهمیت دارد؟

زیرا Object می‌تواند یک مقدار یا Behavior مخصوص خود ایجاد کند، بدون اینکه Property موجود در Prototype را تغییر دهد.

20. اگر Property در هیچ Prototypeی پیدا نشود چه اتفاقی می‌افتد؟

Lookup تا رسیدن به null ادامه پیدا می‌کند. پس از پایان Chain، Property پیدا نشده است و در یک Property Access معمولی نتیجه می‌تواند undefined باشد.

21. چرا تغییر Prototype پس از ایجاد Object معمولاً انتخاب مناسبی نیست؟

زیرا Prototype Relationship بهتر است بخشی روشن و پایدار از طراحی Object باشد. تغییر مکرر آن می‌تواند پیچیدگی و مشکلات نگهداری ایجاد کند.

22. آیا JavaScript واقعاً Prototype-based است؟

بله. Object Model اصلی JavaScript بر Prototypeها و Prototype Chain استوار است. Syntaxهای سطح بالاتری که در فصل‌های بعد بررسی می‌شوند، بر همین مکانیزم تکیه دارند.

Golden Answers

Junior

Prototype یک Object است که می‌تواند منبع Properties و Methods برای Object دیگری باشد. اگر Property روی خود Object پیدا نشود، JavaScript می‌تواند Prototype آن را بررسی کند.

Mid-Level

هر Object می‌تواند از طریق [[Prototype]] به یک Prototype مرتبط باشد. هنگام Property Lookup، JavaScript ابتدا خود Object و سپس Prototypeهای آن را بررسی می‌کند. در Constructor Pattern، Shared Methods معمولاً روی Constructor.prototype قرار می‌گیرند تا Instanceها بتوانند از یک Behavior مشترک استفاده کنند.

Senior

Prototype بخشی بنیادی از Object Model در JavaScript است، نه صرفاً یک تکنیک برای کاهش Memory. Objectها از طریق [[Prototype]] می‌توانند به Objectهای دیگری متصل شوند و Property Lookup در صورت پیدا نشدن Property روی Object فعلی، در Prototype Chain ادامه پیدا می‌کند. Constructor Functionها نیز یک prototype Property دارند که در Constructor Pattern برای Prototype Instanceها استفاده می‌شود. این مدل پایه Shared Behavior، Inherited Properties و بسیاری از رفتارهای Built-in Objectهای JavaScript است.

Conclusion

در آغاز فصل یک مسئله داشتیم:

چند Instance رفتار مشترکی دارند؛ آیا باید این Behavior را برای هر Instance جداگانه ایجاد کنیم؟

Prototype پاسخ این مسئله را فراهم می‌کند.

Behavior مشترک می‌تواند در Prototype قرار بگیرد:

Instance
↓
Prototype
↓
Shared Behavior

اما داستان در همین‌جا تمام نمی‌شود.

Prototype خودش یک Object است و می‌تواند Prototype دیگری داشته باشد:

Instance
↓
Prototype
↓
Prototype
↓
Object.prototype
↓
null

در نتیجه JavaScript می‌تواند برای پیدا کردن یک Property، از Object فعلی شروع کند و در Prototype Chain به جست‌وجو ادامه دهد.

این مدل ذهنی، پایه درک صحیح Objectها در JavaScript است.

در فصل بعد، با ES Classes آشنا می‌شویم و خواهیم دید که Syntax مربوط به class چگونه همین مدل Prototype-based را به شکلی ساده‌تر و ساختاریافته‌تر در اختیار برنامه‌نویس قرار می‌دهد.