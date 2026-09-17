# Chapter 22 — Object Destructuring and Optional Access

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* توضیح دهید چرا Object Destructuring برای استخراج داده‌های موردنیاز از Object مفید است.
* Syntax پایه Object Destructuring را بنویسید و تحلیل کنید.
* Propertyها را هنگام Destructuring به Variableهای جدید متصل کنید.
* Propertyها را هنگام Destructuring Rename کنید.
* برای Propertyهای دارای `undefined` مقدار پیش‌فرض تعیین کنید.
* Nested Destructuring را برای Objectهای تو‌در‌تو به‌کار ببرید.
* توضیح دهید چرا Nested Destructuring برای داده‌های Optional همیشه مناسب نیست.
* با Optional Chaining به Propertyهای Nested، Methodها و Array Elementها به‌صورت ایمن دسترسی پیدا کنید.
* تفاوت `?.` با Property Access معمولی را توضیح دهید.
* مفهوم `null` و `undefined` را در Safe Data Access تشخیص دهید.
* از Nullish Coalescing برای تعیین Fallback مناسب استفاده کنید.
* تفاوت `??` و `||` را در سناریوهای واقعی تشخیص دهید.
* Destructuring، Optional Chaining و Nullish Coalescing را در یک Data Access Pattern ترکیب کنید.
* خطاهای رایج هنگام کار با داده‌های ناقص یا Optional را تشخیص دهید.

## Core Question

چگونه داده‌های Object را به‌صورت خوانا و ایمن استخراج کنیم؟

در فصل‌های قبل دیدیم که Object برای نگهداری داده‌های مرتبط استفاده می‌شود.

برای مثال:

```js
const user = {
  name: 'Omid',
  email: 'omid@example.com',
  role: 'admin'
};
```

اگر چند Property را بخواهیم، می‌توانیم آن‌ها را مستقیماً Access کنیم:

```js
const name = user.name;
const email = user.email;
```

اما JavaScript Syntax دیگری نیز برای بیان همین Intent دارد:

```js
const { name, email } = user;
```

این Syntax **Object Destructuring** نام دارد.

سپس با مسئله دیگری روبه‌رو می‌شویم.

در Applicationهای واقعی همه داده‌ها همیشه وجود ندارند.

برای مثال:

```js
const user = {
  name: 'Omid'
};
```

اگر بخواهیم مستقیماً بنویسیم:

```js
user.profile.city;
```

و `profile` وجود نداشته باشد، اجرای Code با Runtime Error متوقف می‌شود.

برای چنین شرایطی JavaScript **Optional Chaining** را فراهم می‌کند:

```js
user.profile?.city;
```

در نهایت ممکن است بخواهیم اگر مقدار وجود نداشت، یک Value جایگزین داشته باشیم:

```js
const city = user.profile?.city ?? 'Unknown';
```

بنابراین مسیر این فصل چنین است:

```text
Object
↓
Destructuring
↓
Renaming
↓
Default Values
↓
Nested Destructuring
↓
Optional Chaining
↓
Nullish Coalescing
↓
Safe Data Access
```

---

## مقدمه

Objectها معمولاً داده‌های بیشتری از آنچه یک بخش از Application نیاز دارد، در خود نگه می‌دارند.

برای مثال:

```js
const account = {
  owner: 'Omid',
  balance: 5000,
  currency: 'USD'
};
```

اگر فقط `owner` و `balance` را بخواهیم:

```js
const owner = account.owner;
const balance = account.balance;
```

این Code کاملاً صحیح است.

اما Intent واقعی ما این است:

> از این Object، Propertyهای موردنیاز را استخراج کن.

Object Destructuring این Intent را مستقیم‌تر بیان می‌کند:

```js
const { owner, balance } = account;
```

اکنون مسئله دیگری داریم.

فرض کنید داده از یک API دریافت شده است و بخشی از ساختار ممکن است وجود نداشته باشد:

```js
const user = {
  name: 'Omid'
};
```

این Code خطرناک است:

```js
user.profile.city;
```

زیرا `profile` وجود ندارد.

پس باید بتوانیم به شکلی ایمن به داده دسترسی پیدا کنیم:

```js
user.profile?.city;
```

این فصل از دو مسئله اصلی حرکت می‌کند:

```text
استخراج خوانای داده
        ↓
دسترسی ایمن به داده
```

این دو مسئله به هم مرتبط هستند، اما یکسان نیستند.

---

# Destructuring

## چرا Destructuring؟

به این Code توجه کنید:

```js
const account = {
  owner: 'Omid',
  balance: 5000,
  currency: 'USD'
};

const owner = account.owner;
const balance = account.balance;
const currency = account.currency;
```

این Code کاملاً معتبر است.

اما اگر هدف ما فقط استخراج چند Property باشد، JavaScript می‌تواند این Intent را مستقیم‌تر بیان کند:

```js
const { owner, balance, currency } = account;
```

در اینجا Object را تغییر نمی‌دهیم.

فقط Propertyهای موردنیاز را از آن استخراج می‌کنیم.

بنابراین:

> **Object Destructuring یک Syntax برای استخراج Valueهای Propertyهای Object و قرار دادن آن‌ها در Variableهای جدید است.**

مدل ذهنی:

```text
account
   │
   ├── owner
   ├── balance
   └── currency
        │
        ↓
Destructuring
        │
        ├── owner
        ├── balance
        └── currency
```

Destructuring یک عملیات Mutation روی Object نیست.

Object اصلی همچنان همان Object است.

---

## Syntax پایه

Syntax کلی:

```js
const { propertyName } = object;
```

برای چند Property:

```js
const { propertyA, propertyB } = object;
```

مثال:

```js
const product = {
  name: 'Laptop',
  price: 1200,
  stock: 15
};

const { name, price } = product;

console.log(name);
console.log(price);
```

خروجی:

```text
Laptop
1200
```

در اینجا:

```js
const { name, price } = product;
```

یعنی:

```text
Property name
    ↓
Variable name

Property price
    ↓
Variable price
```

---

## Property Matching

Object Destructuring بر اساس **نام Property** انجام می‌شود، نه ترتیب Propertyها.

برای مثال:

```js
const product = {
  price: 1200,
  name: 'Laptop'
};

const { name, price } = product;
```

اگرچه `price` در Object قبل از `name` قرار گرفته است، نتیجه همچنان صحیح است:

```text
name  → 'Laptop'
price → 1200
```

این نکته با Array Destructuring که در بخش‌های بعدی کتاب بررسی خواهد شد متفاوت است.

در Object Destructuring، JavaScript به دنبال Propertyهایی با همان نام می‌گردد.

مدل ذهنی:

```text
{name, price}
    ↓
جست‌وجوی Propertyهای name و price
    ↓
استخراج Valueها
```

بنابراین ترتیب نوشتن Propertyها در Pattern اهمیتی ندارد.

---

## Destructuring و Object اصلی

Destructuring باعث تغییر Object نمی‌شود:

```js
const user = {
  name: 'Omid',
  role: 'admin'
};

const { name } = user;

console.log(user);
```

Object همچنان:

```js
{
  name: 'Omid',
  role: 'admin'
}
```

است.

Variable جدید:

```js
name
```

Value مربوط به Property `name` را دریافت کرده است.

پس:

```text
Destructuring
→ Extraction

نه:

Destructuring
→ Mutation
```

این تفاوت مهم است؛ زیرا Destructuring را نباید با حذف یا تغییر Propertyهای Object اشتباه گرفت.

---

# Renaming

گاهی نام Property با نامی که می‌خواهیم برای Variable استفاده کنیم یکسان نیست.

برای مثال:

```js
const user = {
  name: 'Omid',
  email: 'omid@example.com'
};
```

فرض کنید در یک بخش از Application می‌خواهیم Variableای با نام `userName` داشته باشیم.

نمی‌توانیم بنویسیم:

```js
const { userName } = user;
```

زیرا Object Propertyای با نام `userName` ندارد.

در عوض از Syntax زیر استفاده می‌کنیم:

```js
const { name: userName } = user;
```

اینجا:

```text
name
 ↓
Property Object

userName
 ↓
نام Variable جدید
```

پس:

> در Destructuring، سمت چپ `:` نام Property و سمت راست آن نام Variable جدید است.

مثال:

```js
const user = {
  name: 'Omid',
  email: 'omid@example.com'
};

const { name: userName, email: userEmail } = user;

console.log(userName);
console.log(userEmail);
```

خروجی:

```text
Omid
omid@example.com
```

---

## چرا Renaming مهم است؟

Renaming فقط یک Syntax کوتاه نیست.

در Application واقعی ممکن است چند Object Property با نام یکسان داشته باشیم.

برای مثال:

```js
const user = {
  name: 'Omid'
};

const company = {
  name: 'Tech Company'
};
```

اگر بخواهیم هر دو را در یک Scope داشته باشیم، استفاده از نام `name` برای هر دو Variable ممکن نیست.

می‌توانیم آن‌ها را Rename کنیم:

```js
const { name: userName } = user;
const { name: companyName } = company;
```

اکنون Intent Code واضح‌تر است:

```text
userName
companyName
```

به‌جای:

```text
name
name
```

بنابراین Renaming دو کاربرد مهم دارد:

* جلوگیری از Name Conflict
* افزایش وضوح معنایی Variable

---

## نکته مهم درباره Syntax

این Code:

```js
const { name: userName } = user;
```

به این معنی نیست که Propertyای به نام `userName` در Object وجود دارد.

Property همچنان:

```js
name
```

است.

فقط Variableای که Value آن را دریافت می‌کند:

```js
userName
```

نام دارد.

بنابراین:

```text
Object Property
name
   ↓
Variable
userName
```

این تفاوت در خواندن Code بسیار مهم است.

---

# Default Values

اکنون فرض کنید Property موردنظر ممکن است در Object وجود نداشته باشد.

برای مثال:

```js
const user = {
  name: 'Omid'
};
```

اگر بنویسیم:

```js
const { role } = user;
```

مقدار:

```js
role
```

برابر خواهد بود با:

```js
undefined
```

گاهی این رفتار مناسب است.

اما گاهی Application به یک Value پیش‌فرض نیاز دارد.

برای این کار می‌توانیم Default Value تعیین کنیم:

```js
const { role = 'user' } = user;
```

اکنون:

```js
console.log(role);
```

خروجی:

```text
user
```

مدل ذهنی:

```text
role موجود است؟
       │
   ┌───┴───┐
  Yes      No
   │        │
   ↓        ↓
 Value   'user'
```

---

## Default Value فقط برای `undefined`

یک نکته بسیار مهم وجود دارد.

Default Value در Destructuring زمانی استفاده می‌شود که Value برابر `undefined` باشد.

برای مثال:

```js
const user = {
  role: undefined
};

const { role = 'user' } = user;

console.log(role);
```

خروجی:

```text
user
```

اما اگر Property مقدار دیگری داشته باشد، Default Value استفاده نمی‌شود.

برای مثال:

```js
const user = {
  role: null
};

const { role = 'user' } = user;

console.log(role);
```

خروجی:

```text
null
```

بنابراین:

```text
undefined
→ Default Value

null
→ Default Value اعمال نمی‌شود
```

این تفاوت در طراحی Data Handling بسیار مهم است.

---

## Default Value و Valueهای Falsy

Default Value فقط برای `undefined` فعال می‌شود.

بنابراین Valueهای زیر همچنان حفظ می‌شوند:

```js
const settings = {
  enabled: false,
  retryCount: 0,
  label: ''
};

const {
  enabled = true,
  retryCount = 3,
  label = 'Default'
} = settings;
```

نتیجه:

```text
enabled    → false
retryCount → 0
label      → ''
```

این رفتار با استفاده از `||` برای تعیین Default Value متفاوت است.

تفاوت `||` و `??` را بعدتر بررسی خواهیم کرد.

---

# Nested Destructuring

تا اینجا Propertyهای مستقیم Object را استخراج کردیم.

اما Objectها می‌توانند Nested باشند.

برای مثال:

```js
const user = {
  name: 'Omid',
  address: {
    city: 'Sari',
    country: 'Iran'
  }
};
```

اگر بخواهیم `city` را استخراج کنیم، می‌توانیم ابتدا `address` را استخراج کنیم:

```js
const { address } = user;

const { city } = address;
```

اما JavaScript اجازه می‌دهد این دو مرحله را ترکیب کنیم:

```js
const {
  address: { city }
} = user;
```

اکنون:

```js
console.log(city);
```

خروجی:

```text
Sari
```

مدل ذهنی:

```text
user
 │
 └── address
       │
       └── city
             │
             ↓
           city
```

این Syntax را **Nested Destructuring** می‌نامیم.

---

## Nested Destructuring با Renaming

همان‌طور که Propertyهای سطح اول را Rename می‌کنیم، Propertyهای Nested را نیز می‌توان Rename کرد.

برای مثال:

```js
const user = {
  address: {
    city: 'Sari'
  }
};

const {
  address: { city: userCity }
} = user;
```

اکنون:

```js
console.log(userCity);
```

خروجی:

```text
Sari
```

در اینجا:

```text
address
   ↓
Nested Object

city
   ↓
userCity
```

---

## Nested Destructuring با Default Value

می‌توان Default Value را نیز در Nested Structure استفاده کرد.

برای مثال:

```js
const user = {
  address: {
    city: undefined
  }
};

const {
  address: { city = 'Unknown' }
} = user;
```

اکنون:

```js
console.log(city);
```

خروجی:

```text
Unknown
```

اما باید یک نکته مهم را در نظر بگیریم.

اگر خود `address` وجود نداشته باشد:

```js
const user = {
  name: 'Omid'
};
```

این Code:

```js
const {
  address: { city }
} = user;
```

می‌تواند Runtime Error ایجاد کند.

زیرا JavaScript نمی‌تواند از یک مقدار `undefined`، Property `city` را استخراج کند.

این نکته ما را به مسئله بعدی می‌رساند:

> وقتی ساختار داده قطعی نیست، Destructuring همیشه بهترین ابزار برای دسترسی به آن نیست.

---

# Destructuring در برابر Optional Access

این دو مفهوم را نباید یکی در نظر گرفت.

Destructuring می‌گوید:

> چه داده‌ای را از Object استخراج کنم؟

Optional Chaining می‌گوید:

> اگر بخشی از مسیر وجود نداشت، چگونه بدون Error به آن دسترسی پیدا کنم؟

برای مثال:

```js
const user = {
  name: 'Omid'
};

const { name } = user;
const city = user.profile?.city;
```

در اینجا:

```text
{name}
```

مسئله Extraction را حل می‌کند.

و:

```text
profile?.city
```

مسئله Safe Access را حل می‌کند.

بنابراین این دو Syntax مکمل یکدیگر هستند، نه جایگزین کامل یکدیگر.

---

# Optional Chaining

## چرا Optional Chaining؟

فرض کنید User Object از یک API دریافت شده است:

```js
const user = {
  name: 'Omid'
};
```

ممکن است `profile` در پاسخ API وجود داشته باشد:

```js
const user = {
  name: 'Omid',
  profile: {
    city: 'Sari'
  }
};
```

و ممکن است وجود نداشته باشد:

```js
const user = {
  name: 'Omid'
};
```

اگر بنویسیم:

```js
user.profile.city;
```

در حالت دوم:

```text
user
 ↓
profile
 ↓
undefined
 ↓
.city
```

JavaScript تلاش می‌کند Property `city` را از `undefined` بخواند و Runtime Error ایجاد می‌شود.

Optional Chaining این مسئله را حل می‌کند:

```js
user.profile?.city;
```

اگر `profile` وجود نداشته باشد، JavaScript به‌جای ادامه دادن Access، نتیجه `undefined` تولید می‌کند.

---

## Syntax پایه

Syntax:

```js
object?.property
```

مثال:

```js
const user = {
  name: 'Omid'
};

const city = user.profile?.city;

console.log(city);
```

خروجی:

```text
undefined
```

هیچ Runtime Errorای رخ نمی‌دهد.

مدل ذهنی:

```text
user.profile
      │
      ↓
وجود دارد؟
   │
 ┌─┴──┐
Yes   No
 │     │
 ↓     ↓
city  undefined
```

---

## Optional Chaining فقط `null` و `undefined` را بررسی می‌کند

Optional Chaining زمانی مسیر را متوقف می‌کند که مقدار سمت چپ `?.` برابر:

```text
null
```

یا:

```text
undefined
```

باشد.

برای مثال:

```js
const user = null;

console.log(user?.name);
```

خروجی:

```text
undefined
```

و:

```js
const user = undefined;

console.log(user?.name);
```

نیز:

```text
undefined
```

اما اگر Value یک Object معتبر باشد، Access ادامه پیدا می‌کند:

```js
const user = {
  name: 'Omid'
};

console.log(user?.name);
```

خروجی:

```text
Omid
```

---

# Nested Access

Optional Chaining زمانی بیشترین ارزش را دارد که داده Nested باشد.

برای مثال:

```js
const response = {
  user: {
    profile: {
      city: 'Sari'
    }
  }
};
```

می‌توانیم بنویسیم:

```js
const city = response.user?.profile?.city;
```

اگر `profile` وجود نداشته باشد:

```js
response.user?.profile?.city;
```

نتیجه:

```text
undefined
```

اگر حتی `user` ممکن است وجود نداشته باشد، می‌توانیم زنجیره را از همان نقطه Optional کنیم:

```js
const city = response.user?.profile?.city;
```

اگر:

```js
response.user === undefined
```

باشد، نتیجه `undefined` خواهد بود.

---

## یک نکته مهم درباره محل `?.`

Optional Chaining باید در نقطه‌ای استفاده شود که احتمال `null` یا `undefined` بودن Value وجود دارد.

برای مثال:

```js
const user = {
  profile: undefined
};
```

این Code ایمن نیست:

```js
user.profile.city;
```

اما:

```js
user.profile?.city;
```

ایمن است.

بنابراین `?.` باید قبل از Accessای قرار بگیرد که ممکن است روی `null` یا `undefined` انجام شود.

---

# Optional Chaining و Method Calls

Optional Chaining فقط برای Property Access نیست.

اگر یک Method ممکن است وجود نداشته باشد، می‌توانیم از:

```js
?.()
```

استفاده کنیم.

برای مثال:

```js
const user = {
  name: 'Omid'
};

user.print?.();
```

اگر `print` وجود نداشته باشد، Call انجام نمی‌شود و نتیجه `undefined` خواهد بود.

اما اگر Method وجود داشته باشد:

```js
const user = {
  name: 'Omid',

  print() {
    console.log(this.name);
  }
};

user.print?.();
```

Method اجرا می‌شود.

این Syntax برای APIهایی مفید است که یک Method ممکن است Optional باشد.

---

## تفاوت `user.print?.()` با `user?.print()`

این دو Syntax از یک نقطه شروع نمی‌شوند.

```js
user.print?.();
```

یعنی:

> اگر `print` وجود داشت، آن را Call کن.

در مقابل:

```js
user?.print();
```

یعنی:

> اگر `user` وجود داشت، Property `print` را Access کن و سپس Method را Call کن.

اگر هر دو بخش ممکن است Optional باشند:

```js
user?.print?.();
```

می‌توان هر دو را کنترل کرد.

این Syntax باید فقط زمانی استفاده شود که Optional بودن هر بخش واقعاً بخشی از مدل داده یا API باشد.

---

# Optional Chaining و Array Access

Optional Chaining برای Array Access نیز قابل استفاده است.

فرض کنید:

```js
const response = {
  users: [
    { name: 'Omid' }
  ]
};
```

می‌توانیم بنویسیم:

```js
const firstUser = response.users?.[0];
```

اگر `users` وجود نداشته باشد:

```js
response.users?.[0];
```

نتیجه:

```text
undefined
```

برای دسترسی به Property داخل Element نیز می‌توانیم زنجیره را ادامه دهیم:

```js
const firstUserName = response.users?.[0]?.name;
```

مدل ذهنی:

```text
response
   ↓
users?
   ↓
[0]?
   ↓
name
```

این Pattern در کار با داده‌های API بسیار رایج است.

---

# Optional Chaining و Methodهای موجود

Optional Chaining به این معنی نیست که Method را بدون توجه به قرارداد API فراخوانی کنیم.

برای مثال:

```js
user.print?.();
```

اگر `print` وجود نداشته باشد، هیچ اتفاقی نمی‌افتد.

این رفتار زمانی مفید است که نبودن Method یک حالت معتبر باشد.

اما اگر `print` طبق طراحی Application باید حتماً وجود داشته باشد، استفاده از Optional Chaining ممکن است یک خطای طراحی را پنهان کند.

بنابراین:

> Optional Chaining ابزار حذف همه Errorها نیست؛ ابزار بیان Optional بودن یک مسیر Access است.

---

# Nullish Coalescing

Optional Chaining معمولاً به یک سؤال دیگر منجر می‌شود:

اگر Access نتیجه `undefined` داد، چه مقدار دیگری باید استفاده کنیم؟

برای مثال:

```js
const user = {
  name: 'Omid'
};

const city = user.profile?.city;
```

در اینجا:

```js
city === undefined
```

ممکن است بخواهیم به‌جای `undefined` مقدار:

```text
Unknown
```

نمایش داده شود.

برای این کار از **Nullish Coalescing Operator** یعنی `??` استفاده می‌کنیم:

```js
const city = user.profile?.city ?? 'Unknown';
```

مدل ذهنی:

```text
user.profile?.city
        │
        ↓
null یا undefined؟
      │
   ┌──┴──┐
  Yes    No
   │      │
   ↓      ↓
Unknown  Value
```

---

## چرا `??`؟

Operator `??` زمانی سمت راست خود را انتخاب می‌کند که سمت چپ:

```text
null
```

یا:

```text
undefined
```

باشد.

برای مثال:

```js
const value = null;

const result = value ?? 10;

console.log(result);
```

خروجی:

```text
10
```

اما:

```js
const value = 0;

const result = value ?? 10;

console.log(result);
```

خروجی:

```text
0
```

زیرا `0` نه `null` است و نه `undefined`.

---

# تفاوت `??` و `||`

این تفاوت در Applicationهای واقعی بسیار مهم است.

`||` بر اساس Truthy / Falsy رفتار می‌کند.

`??` فقط `null` و `undefined` را به‌عنوان نبود Value در نظر می‌گیرد.

برای مثال:

```js
const count = 0;

const result1 = count || 10;
const result2 = count ?? 10;
```

نتیجه:

```text
result1 → 10
result2 → 0
```

چرا؟

زیرا:

```text
0
↓
Falsy
```

بنابراین `||` مقدار سمت راست را انتخاب می‌کند.

اما:

```text
0
↓
not null
not undefined
```

پس `??` همان `0` را حفظ می‌کند.

---

## `false` نیز یک Value معتبر است

مثال:

```js
const settings = {
  enabled: false
};

const enabled = settings.enabled ?? true;
```

نتیجه:

```text
false
```

اما:

```js
const enabled = settings.enabled || true;
```

نتیجه:

```text
true
```

اگر `false` معنای واقعی و معتبر در Application داشته باشد، استفاده از `||` اشتباه است.

---

## String خالی نیز ممکن است معتبر باشد

برای مثال:

```js
const user = {
  nickname: ''
};

const nickname = user.nickname ?? 'Guest';
```

نتیجه:

```text
''
```

اما:

```js
const nickname = user.nickname || 'Guest';
```

نتیجه:

```text
Guest
```

بنابراین انتخاب بین `||` و `??` باید بر اساس معنی داده انجام شود.

---

# ترکیب Optional Chaining و Nullish Coalescing

یکی از Patternهای بسیار رایج:

```js
const city = user.profile?.city ?? 'Unknown';
```

اینجا سه مرحله داریم:

```text
user.profile?.city
        ↓
Safe Access
        ↓
null / undefined
        ↓
??
        ↓
Fallback
```

مثال کامل:

```js
const user = {
  name: 'Omid'
};

const city = user.profile?.city ?? 'Unknown';

console.log(city);
```

خروجی:

```text
Unknown
```

اگر داده وجود داشته باشد:

```js
const user = {
  name: 'Omid',
  profile: {
    city: 'Sari'
  }
};

const city = user.profile?.city ?? 'Unknown';
```

خروجی:

```text
Sari
```

اگر مقدار `city` برابر `null` باشد نیز Fallback استفاده می‌شود:

```js
const user = {
  profile: {
    city: null
  }
};

const city = user.profile?.city ?? 'Unknown';
```

نتیجه:

```text
Unknown
```

اما اگر مقدار `city` برابر `''` یا `0` باشد، همان Value حفظ می‌شود.

---

# API Data Patterns

داده‌هایی که از API دریافت می‌شوند، یکی از مهم‌ترین موقعیت‌های استفاده از این Syntaxها هستند.

فرض کنید Response چنین باشد:

```js
const response = {
  user: {
    name: 'Omid',
    profile: {
      city: 'Sari'
    }
  }
};
```

اگر فقط `user` و `name` را نیاز داشته باشیم، می‌توانیم از Destructuring استفاده کنیم:

```js
const { user } = response;
const { name } = user;
```

یا:

```js
const { user: currentUser } = response;
const { name } = currentUser;
```

اما اگر `profile` Optional باشد، بهتر است برای Access آن از Optional Chaining استفاده کنیم:

```js
const city = user.profile?.city;
```

و اگر UI نیاز به مقدار جایگزین داشته باشد:

```js
const city = user.profile?.city ?? 'Unknown';
```

این سه ابزار نقش‌های متفاوت دارند:

```text
Destructuring
→ استخراج داده موردنیاز

Optional Chaining
→ جلوگیری از Error هنگام نبودن مسیر Optional

Nullish Coalescing
→ تعیین Fallback برای null / undefined
```

---

## Missing Data

نبودن داده همیشه به معنی Error نیست.

در یک Application ممکن است:

* User پروفایل خود را کامل نکرده باشد.
* API یک Field را برای بعضی کاربران ارسال نکند.
* یک Configuration اختیاری باشد.
* یک Array خالی باشد.
* یک Property مقدار `null` داشته باشد.

در چنین شرایطی باید ابتدا مشخص کنیم:

> آیا نبودن داده یک حالت معتبر است یا یک خطای Application؟

اگر نبودن داده معتبر باشد، Safe Access می‌تواند انتخاب مناسبی باشد:

```js
const city = user.profile?.city ?? 'Unknown';
```

اگر داده طبق قرارداد Application باید حتماً وجود داشته باشد، پنهان کردن مشکل با Optional Chaining ممکن است تصمیم مناسبی نباشد.

---

# یک الگوی کامل

فرض کنید یک Product از API دریافت شده است:

```js
const product = {
  name: 'Laptop',
  pricing: {
    amount: 1200,
    currency: 'USD'
  }
};
```

می‌توانیم Data اصلی را با Destructuring استخراج کنیم:

```js
const { name, pricing } = product;
```

و سپس:

```js
const price = pricing?.amount ?? 0;
```

در اینجا:

```text
name
↓
Destructuring

pricing?.amount
↓
Optional Access

?? 0
↓
Fallback
```

اگر `pricing` وجود نداشته باشد:

```js
const product = {
  name: 'Laptop'
};

const { name, pricing } = product;

const price = pricing?.amount ?? 0;
```

نتیجه:

```text
name  → Laptop
price → 0
```

این Code هم خوانا است و هم Intent آن مشخص است.

---

# Safe Data Access

اکنون می‌توانیم کل Concept Flow فصل را در یک مثال ببینیم.

فرض کنید:

```js
const response = {
  user: {
    name: 'Omid',
    profile: {
      city: 'Sari'
    }
  }
};
```

ابتدا داده موردنیاز را استخراج می‌کنیم:

```js
const { user } = response;
```

سپس:

```js
const { name } = user;
```

و برای داده Optional:

```js
const city = user.profile?.city ?? 'Unknown';
```

در نهایت:

```js
console.log(name);
console.log(city);
```

خروجی:

```text
Omid
Sari
```

مدل ذهنی:

```text
Object
  ↓
Destructuring
  ↓
Data Extraction
  ↓
Optional Chaining
  ↓
Safe Access
  ↓
Nullish Coalescing
  ↓
Fallback
```

این همان مسیر اصلی فصل است.

---

# Best Practices

## 1. Destructuring را برای بیان واضح Intent استفاده کنید

اگر چند Property مشخص موردنیاز است:

```js
const { name, email } = user;
```

اغلب Intent را بهتر از چند Access جداگانه بیان می‌کند:

```js
const name = user.name;
const email = user.email;
```

اما اگر فقط یک Property لازم است، Access مستقیم نیز کاملاً مناسب است:

```js
const name = user.name;
```

هدف، کوتاه کردن Code به هر قیمت نیست.

هدف، واضح‌تر کردن Intent است.

---

## 2. هنگام Name Conflict از Renaming استفاده کنید

به‌جای ایجاد نام‌های مبهم:

```js
const { name } = user;
```

در صورت نیاز:

```js
const { name: userName } = user;
```

این کار مخصوصاً هنگام کار با چند Object خوانایی Code را افزایش می‌دهد.

---

## 3. Default Value را برای `undefined` در نظر بگیرید

این:

```js
const { role = 'user' } = user;
```

فقط زمانی Default را اعمال می‌کند که `role` برابر `undefined` باشد.

اگر `null` باید نیز به‌عنوان نبود داده در نظر گرفته شود، Default Value در Destructuring به‌تنهایی کافی نیست.

در چنین شرایطی می‌توان از `??` استفاده کرد:

```js
const role = user.role ?? 'user';
```

---

## 4. Nested Destructuring را برای ساختارهای قابل‌اعتماد استفاده کنید

اگر ساختار داده مشخص و معتبر است:

```js
const {
  address: { city }
} = user;
```

می‌تواند خوانا باشد.

اما اگر `address` ممکن است وجود نداشته باشد، این Syntax می‌تواند Runtime Error ایجاد کند.

در چنین شرایطی Safe Access مناسب‌تر است:

```js
const city = user.address?.city;
```

---

## 5. Optional Chaining را فقط برای داده واقعاً Optional استفاده کنید

این Code:

```js
user.profile?.city;
```

زمانی مناسب است که `profile` واقعاً ممکن است وجود نداشته باشد.

اگر `profile` طبق قرارداد Application باید همیشه وجود داشته باشد، استفاده افراطی از `?.` می‌تواند Error واقعی را پنهان کند.

---

## 6. Fallback را بر اساس معنی داده انتخاب کنید

اگر `0`، `false` یا `''` مقدار معتبر است، `??` معمولاً Intent دقیق‌تری دارد:

```js
const count = response.count ?? 10;
```

به‌جای:

```js
const count = response.count || 10;
```

انتخاب Operator باید بر اساس Semantic داده باشد، نه صرفاً کوتاه‌تر بودن Syntax.

---

## 7. Syntax کوتاه‌تر را همیشه بهتر فرض نکنید

این Code:

```js
const city = response.user?.profile?.address?.city ?? 'Unknown';
```

ممکن است معتبر باشد، اما اگر ساختار بسیار پیچیده شود، خوانایی کاهش پیدا می‌کند.

در چنین شرایطی می‌توان Access را به چند مرحله منطقی تقسیم کرد.

هدف اصلی:

```text
Readability
+
Correctness
+
Clear Intent
```

است.

---

# اشتباهات رایج

## اشتباه ۱: تصور اینکه Destructuring Object را تغییر می‌دهد

این:

```js
const { name } = user;
```

Object را Mutation نمی‌کند.

فقط Value مربوط به `name` را استخراج می‌کند.

---

## اشتباه ۲: اشتباه گرفتن Property Name و Variable Name هنگام Renaming

در:

```js
const { name: userName } = user;
```

`name` نام Property است.

`userName` نام Variable جدید است.

Propertyای به نام `userName` از Object درخواست نمی‌شود.

---

## اشتباه ۳: تصور اینکه Default Value برای `null` نیز فعال می‌شود

این:

```js
const { role = 'user' } = user;
```

برای:

```js
role === undefined
```

Default را اعمال می‌کند.

اما اگر:

```js
role === null
```

باشد، مقدار `null` حفظ می‌شود.

---

## اشتباه ۴: استفاده از Nested Destructuring روی ساختار نامطمئن

این Code:

```js
const {
  profile: { city }
} = user;
```

اگر `profile` وجود نداشته باشد، می‌تواند Runtime Error ایجاد کند.

برای داده Optional:

```js
const city = user.profile?.city;
```

مناسب‌تر است.

---

## اشتباه ۵: تصور اینکه Optional Chaining همه Errorها را متوقف می‌کند

Optional Chaining فقط همان Access یا Call مشخص را کنترل می‌کند.

برای مثال:

```js
const user = null;

console.log(user?.name);
```

ایمن است.

اما `?.` جایگزین Error Handling عمومی نیست.

---

## اشتباه ۶: اشتباه گرفتن `??` با `||`

این دو Operator رفتار یکسانی ندارند.

```js
const count = 0;

count || 10;
```

نتیجه:

```text
10
```

اما:

```js
count ?? 10;
```

نتیجه:

```text
0
```

اگر `0` مقدار معتبر باشد، `??` معمولاً انتخاب صحیح‌تری است.

---

## اشتباه ۷: استفاده افراطی از Optional Chaining

این Code:

```js
response?.user?.profile?.address?.city
```

ممکن است کاملاً صحیح باشد.

اما اگر همه این Objectها طبق قرارداد Application باید وجود داشته باشند، استفاده از `?.` می‌تواند یک مشکل واقعی را به `undefined` تبدیل کند.

Optional بودن باید بخشی از مدل داده باشد، نه فقط راهی برای جلوگیری از Error.

---

## اشتباه ۸: یکی دانستن Extraction و Safe Access

این دو:

```js
const { name } = user;
```

و:

```js
user.profile?.city;
```

یک مسئله را حل نمی‌کنند.

اولی:

```text
Extraction
```

است.

دومی:

```text
Safe Access
```

است.

درک این تفاوت باعث می‌شود Syntax مناسب را بر اساس مسئله انتخاب کنیم.

---

# Summary

در این فصل دیدیم که Objectها فقط محل نگهداری داده نیستند؛ نحوه دسترسی به این داده‌ها نیز روی خوانایی و ایمنی Code تأثیر دارد.

ابتدا **Object Destructuring** را یاد گرفتیم:

```js
const { name, email } = user;
```

Destructuring اجازه می‌دهد Propertyهای موردنیاز را مستقیماً استخراج کنیم.

Destructuring Object را تغییر نمی‌دهد و بر اساس Property Name انجام می‌شود، نه ترتیب Propertyها.

سپس یاد گرفتیم که می‌توانیم هنگام Destructuring نام Variable را تغییر دهیم:

```js
const { name: userName } = user;
```

در ادامه Default Value را بررسی کردیم:

```js
const { role = 'user' } = user;
```

دیدیم که Default Value فقط برای `undefined` فعال می‌شود و برای `null` اعمال نمی‌شود.

سپس Nested Destructuring را دیدیم:

```js
const {
  address: { city }
} = user;
```

این Syntax برای ساختارهای مشخص و قابل‌اعتماد مفید است، اما اگر Objectهای Nested Optional باشند، ممکن است Runtime Error ایجاد کند.

در ادامه به **Optional Chaining** رسیدیم:

```js
user.profile?.city;
```

Optional Chaining امکان دسترسی ایمن به داده‌هایی را فراهم می‌کند که ممکن است `null` یا `undefined` باشند.

این Syntax برای Method Call نیز قابل استفاده است:

```js
user.print?.();
```

و برای Array Access:

```js
response.users?.[0];
```

در نهایت **Nullish Coalescing** را بررسی کردیم:

```js
const city = user.profile?.city ?? 'Unknown';
```

دیدیم که `??` فقط زمانی Fallback را انتخاب می‌کند که مقدار سمت چپ `null` یا `undefined` باشد.

در مقابل، `||` تمام Falsy Valueها را بررسی می‌کند.

بنابراین:

```text
Destructuring
→ استخراج خوانای داده

Renaming
→ کنترل نام Variable

Default Values
→ Fallback برای undefined

Nested Destructuring
→ استخراج داده از Objectهای Nested

Optional Chaining
→ دسترسی ایمن به داده‌های Optional

Nullish Coalescing
→ Fallback برای null / undefined

Safe Data Access
→ ترکیب این ابزارها بر اساس نیاز واقعی Application
```

---

# Key Takeaways

1. Object Destructuring برای استخراج Propertyهای Object استفاده می‌شود.

2. Destructuring Object را تغییر نمی‌دهد.

3. Object Destructuring بر اساس Property Name انجام می‌شود، نه ترتیب Propertyها.

4. می‌توان Property را هنگام Destructuring Rename کرد:

```js
const { name: userName } = user;
```

5. در Renaming، سمت چپ `:` نام Property و سمت راست نام Variable جدید است.

6. Default Value در Destructuring فقط برای `undefined` فعال می‌شود:

```js
const { role = 'user' } = user;
```

7. `null` باعث فعال شدن Default Value در Destructuring نمی‌شود.

8. Nested Destructuring برای ساختارهای مشخص و قابل‌اعتماد مفید است.

9. Nested Destructuring روی ساختار Optional ممکن است Runtime Error ایجاد کند.

10. Optional Chaining با `?.` برای دسترسی به داده‌های احتمالی استفاده می‌شود:

```js
user.profile?.city;
```

11. Optional Chaining در Method Call نیز قابل استفاده است:

```js
user.print?.();
```

12. Optional Chaining در Array Access نیز قابل استفاده است:

```js
user.orders?.[0];
```

13. Optional Chaining زمانی مسیر را متوقف می‌کند که مقدار مورد بررسی `null` یا `undefined` باشد.

14. `??` فقط زمانی Fallback را انتخاب می‌کند که مقدار `null` یا `undefined` باشد.

15. `||` تمام Falsy Valueها را بررسی می‌کند.

16. `0`، `false` و `''` ممکن است Valueهای معتبر باشند؛ بنابراین در چنین شرایطی `??` می‌تواند انتخاب مناسب‌تری باشد.

17. Destructuring و Optional Chaining دو مسئله متفاوت را حل می‌کنند:

```text
Destructuring
→ استخراج داده

Optional Chaining
→ دسترسی ایمن
```

18. Optional Chaining نباید برای پنهان کردن خطاهای واقعی Application به‌صورت افراطی استفاده شود.

19. Syntax کوتاه‌تر همیشه به معنی Code بهتر نیست.

20. انتخاب Syntax باید بر اساس خوانایی، Intent و Optional بودن واقعی داده انجام شود.

---

# Technical Interview

## Junior

### Question 1 — What is Object Destructuring?

**Answer:**

Object Destructuring یک Syntax در JavaScript است که اجازه می‌دهد Propertyهای یک Object را بر اساس نام آن‌ها استخراج کنیم و در Variableهای جدید قرار دهیم.

مثال:

```js
const user = {
  name: 'Omid',
  age: 30
};

const { name, age } = user;
```

اکنون `name` و `age` Variableهای جدیدی هستند که Value متناظر با Propertyهای Object را دریافت کرده‌اند.

---

### Question 2 — Does Destructuring modify the original Object?

**Answer:**

خیر.

Destructuring یک عملیات Extraction است و Object اصلی را Mutation نمی‌کند.

```js
const user = {
  name: 'Omid'
};

const { name } = user;
```

Object همچنان همان Propertyها را دارد.

---

### Question 3 — How does Object Destructuring match properties?

**Answer:**

Object Destructuring بر اساس Property Name انجام می‌شود، نه ترتیب Propertyها.

```js
const product = {
  price: 1200,
  name: 'Laptop'
};

const { name, price } = product;
```

در اینجا `name` و `price` بر اساس نام Property پیدا می‌شوند.

---

### Question 4 — What is Renaming in Destructuring?

**Answer:**

Renaming یعنی هنگام Destructuring، برای Variable جدید نام متفاوتی تعیین کنیم.

```js
const { name: userName } = user;
```

در این مثال `name` نام Property و `userName` نام Variable جدید است.

---

### Question 5 — What is a Default Value in Destructuring?

**Answer:**

Default Value مقداری است که زمانی استفاده می‌شود که Property موردنظر `undefined` باشد.

```js
const { role = 'user' } = user;
```

اگر `role` وجود نداشته باشد، Variable `role` مقدار `'user'` دریافت می‌کند.

---

### Question 6 — What problem does Optional Chaining solve?

**Answer:**

Optional Chaining برای دسترسی ایمن به Property، Method یا Array Elementای استفاده می‌شود که ممکن است وجود نداشته باشد.

```js
const city = user.profile?.city;
```

اگر `profile` برابر `null` یا `undefined` باشد، نتیجه `undefined` خواهد بود و Runtime Error مربوط به این Access ایجاد نمی‌شود.

---

## Mid-Level

### Question 7 — Why is `const { name } = user` different from `const name = user.name`?

**Answer:**

از نظر نتیجه برای این Property مشخص یکسان هستند، اما Syntax آن‌ها Intent متفاوتی را بیان می‌کند.

```js
const name = user.name;
```

یک Property Access مستقیم است.

اما:

```js
const { name } = user;
```

به‌صورت مستقیم بیان می‌کند که می‌خواهیم Propertyهای مشخصی را از Object استخراج کنیم.

Destructuring مخصوصاً زمانی خواناتر می‌شود که چند Property موردنیاز باشد.

---

### Question 8 — Does Object Destructuring depend on Property order?

**Answer:**

خیر.

Object Destructuring بر اساس نام Property انجام می‌شود.

```js
const user = {
  email: 'omid@example.com',
  name: 'Omid'
};

const { name, email } = user;
```

ترتیب Propertyها در Object یا Pattern تعیین‌کننده نیست.

---

### Question 9 — What is the difference between these two?

```js
const { name } = user;
```

و:

```js
const { name: userName } = user;
```

**Answer:**

در اولی Variable جدید `name` نام دارد.

در دومی Property `name` استخراج می‌شود، اما Variable جدید `userName` نام دارد.

```text
name
↓
userName
```

Property Object تغییر نمی‌کند.

---

### Question 10 — When does a Destructuring Default Value apply?

**Answer:**

Default Value فقط زمانی اعمال می‌شود که Property مقدار `undefined` داشته باشد.

```js
const user = {
  role: undefined
};

const { role = 'user' } = user;
```

نتیجه:

```text
user
```

اما برای:

```js
const user = {
  role: null
};
```

مقدار `null` حفظ می‌شود.

---

### Question 11 — Why can Nested Destructuring cause a Runtime Error?

**Answer:**

زیرا برای استخراج Property Nested، Object والد باید وجود داشته باشد.

این Code را در نظر بگیرید:

```js
const user = {
  name: 'Omid'
};

const {
  profile: { city }
} = user;
```

در اینجا `profile` وجود ندارد و مقدار آن `undefined` است.

JavaScript نمی‌تواند `city` را از `undefined` استخراج کند.

برای ساختار Optional می‌توان از:

```js
const city = user.profile?.city;
```

استفاده کرد.

---

### Question 12 — What is the difference between `?.` and normal property access?

**Answer:**

Access معمولی:

```js
user.profile.city;
```

فرض می‌کند مسیر موردنظر معتبر است.

اگر `profile` برابر `null` یا `undefined` باشد، Runtime Error ایجاد می‌شود.

اما:

```js
user.profile?.city;
```

در صورت `null` یا `undefined` بودن `profile`، Access را متوقف کرده و `undefined` برمی‌گرداند.

---

### Question 13 — Can Optional Chaining be used with Method Calls?

**Answer:**

بله.

اگر Method ممکن است وجود نداشته باشد:

```js
user.print?.();
```

اگر `print` وجود داشته باشد، Call می‌شود.

اگر وجود نداشته باشد، Call انجام نمی‌شود و نتیجه `undefined` است.

---

### Question 14 — What is the difference between `??` and `||`?

**Answer:**

`??` فقط `null` و `undefined` را به‌عنوان نبود Value در نظر می‌گیرد.

`||` تمام Falsy Valueها را بررسی می‌کند.

برای مثال:

```js
const count = 0;

count || 10; // 10
count ?? 10; // 0
```

اگر `0` یک Value معتبر باشد، `??` انتخاب دقیق‌تری است.

---

### Question 15 — How do Destructuring and Optional Chaining complement each other?

**Answer:**

این دو Syntax دو مسئله متفاوت را حل می‌کنند.

Destructuring برای Extraction است:

```js
const { name, profile } = user;
```

Optional Chaining برای Safe Access است:

```js
const city = profile?.city;
```

می‌توان آن‌ها را در یک جریان ترکیب کرد:

```js
const { name, profile } = user;

const city = profile?.city ?? 'Unknown';
```

مدل ذهنی:

```text
Destructuring
    ↓
Extraction

Optional Chaining
    ↓
Safe Access

Nullish Coalescing
    ↓
Fallback
```

---

## Senior

### Question 16 — Why is Optional Chaining not simply a better form of Property Access?

**Answer:**

زیرا Optional Chaining یک Semantic مشخص دارد:

> نبودن Value در این مسیر یک حالت قابل‌قبول است.

برای مثال:

```js
const city = user.profile?.city;
```

یعنی `profile` ممکن است وجود نداشته باشد.

اما اگر `profile` طبق قرارداد Application باید همیشه وجود داشته باشد، استفاده از `?.` ممکن است یک Data Integrity Problem را پنهان کند و آن را به `undefined` تبدیل کند.

بنابراین Optional Chaining باید برای داده واقعاً Optional استفاده شود، نه برای حذف همه Runtime Errorها.

---

### Question 17 — Why is `??` often more accurate than `||` for Application Data?

**Answer:**

زیرا `??` مفهوم **missing value** را به `null` و `undefined` محدود می‌کند.

در Application Data، Valueهایی مانند:

```text
0
false
''
```

ممکن است کاملاً معتبر باشند.

برای مثال:

```js
const settings = {
  enabled: false
};

const enabled = settings.enabled ?? true;
```

نتیجه:

```text
false
```

اما با:

```js
settings.enabled || true;
```

نتیجه `true` خواهد بود.

بنابراین `??` زمانی دقیق‌تر است که Intent ما این باشد:

> فقط در صورت نبود واقعی Value، از Fallback استفاده کن.

---

### Question 18 — What is the engineering difference between Destructuring and Safe Access?

**Answer:**

Destructuring یک ابزار **Data Extraction** است.

Optional Chaining یک ابزار **Safe Access** است.

مثلاً:

```js
const { name } = user;
```

می‌گوید:

> Property `name` را از این Object استخراج کن.

در مقابل:

```js
user.profile?.city;
```

می‌گوید:

> اگر `profile` وجود داشت، `city` را Access کن؛ در غیر این صورت `undefined` برگردان.

این تفاوت نشان می‌دهد که این Syntaxها جایگزین کامل یکدیگر نیستند.

---

### Question 19 — Why can excessive Optional Chaining be a design problem?

**Answer:**

زیرا Optional Chaining می‌تواند یک وضعیت غیرمنتظره را به `undefined` تبدیل کند.

فرض کنید طبق قرارداد Application:

```text
user
  ↓
profile
  ↓
city
```

باید همیشه وجود داشته باشند.

اگر بنویسیم:

```js
const city = user?.profile?.city;
```

ممکن است یک Data Problem واقعی بدون علامت باقی بماند.

در طراحی حرفه‌ای باید بین این دو وضعیت تفاوت قائل شویم:

```text
Data is optional
        vs
Data should exist but is missing
```

در حالت اول `?.` مفید است.

در حالت دوم باید اجازه دهیم مشکل قابل مشاهده و قابل بررسی باقی بماند.

---

### Question 20 — What is the difference between Destructuring Default Values and Nullish Coalescing?

**Answer:**

Default Value در Destructuring فقط هنگام `undefined` فعال می‌شود:

```js
const { role = 'user' } = user;
```

اما `??` برای هر دو مقدار `null` و `undefined` فعال می‌شود:

```js
const role = user.role ?? 'user';
```

بنابراین:

```text
Destructuring Default
→ undefined

??
→ undefined + null
```

اگر `null` نیز باید به‌عنوان نبود Value در نظر گرفته شود، `??` ابزار مناسب‌تری است.

---

### Question 21 — Why is this combination useful in real applications?

```js
const city = user.profile?.city ?? 'Unknown';
```

**Answer:**

زیرا سه Intent مشخص را در یک Expression بیان می‌کند:

```text
profile ممکن است وجود نداشته باشد
        ↓
Optional Chaining

city ممکن است null/undefined باشد
        ↓
Nullish Coalescing

در این صورت مقدار Unknown استفاده کن
        ↓
Fallback
```

این الگو برای داده‌های Optional مانند API Responseها و Configurationها بسیار مفید است؛ البته فقط زمانی که نبودن داده واقعاً یک حالت معتبر باشد.

---

# Golden Answers

## Junior

### اگر در مصاحبه پرسیده شد: «Destructuring چیست؟»

پاسخ مناسب:

> Object Destructuring یک Syntax در JavaScript است که اجازه می‌دهد Propertyهای یک Object را بر اساس نام آن‌ها استخراج کرده و در Variableهای جدید قرار دهیم. این عملیات Object اصلی را تغییر نمی‌دهد.

مثال:

```js
const { name, email } = user;
```

---

### اگر پرسیده شد: «Optional Chaining چه مشکلی را حل می‌کند؟»

پاسخ مناسب:

> Optional Chaining برای دسترسی ایمن به Property، Method یا Array Elementای استفاده می‌شود که ممکن است وجود نداشته باشد. اگر مقدار مورد بررسی `null` یا `undefined` باشد، Access متوقف شده و `undefined` تولید می‌شود.

مثال:

```js
const city = user.profile?.city;
```

---

### اگر پرسیده شد: «`??` چیست؟»

پاسخ مناسب:

> `??` یک Operator برای تعیین Fallback است که فقط زمانی مقدار سمت راست را انتخاب می‌کند که مقدار سمت چپ `null` یا `undefined` باشد.

مثال:

```js
const count = value ?? 10;
```

---

## Mid-Level

### اگر پرسیده شد: «چرا `const { name: userName } = user` با `const { userName } = user` متفاوت است؟»

پاسخ مناسب:

> در `const { name: userName } = user`، Propertyای به نام `name` از Object استخراج می‌شود و در Variableای به نام `userName` قرار می‌گیرد. اما `const { userName } = user` مستقیماً به دنبال Propertyای با نام `userName` می‌گردد.

---

### اگر پرسیده شد: «Default Value در Destructuring چه زمانی اعمال می‌شود؟»

پاسخ مناسب:

> Default Value فقط زمانی اعمال می‌شود که Value برابر `undefined` باشد. اگر Value برابر `null`، `0`، `false` یا `''` باشد، همان Value حفظ می‌شود.

مثال:

```js
const { count = 10 } = { count: 0 };

console.log(count);
```

خروجی:

```text
0
```

---

### اگر پرسیده شد: «تفاوت `??` و `||` چیست؟»

پاسخ مناسب:

> `??` فقط `null` و `undefined` را به‌عنوان نبود Value در نظر می‌گیرد، درحالی‌که `||` تمام Falsy Valueها را بررسی می‌کند. بنابراین اگر `0`، `false` یا `''` مقدار معتبر باشد، `??` معمولاً انتخاب مناسب‌تری است.

مثال:

```js
const count = 0;

count || 10; // 10
count ?? 10; // 0
```

---

## Senior

### اگر پرسیده شد: «چه زمانی Optional Chaining می‌تواند بد باشد؟»

پاسخ مناسب:

> Optional Chaining زمانی مناسب است که نبودن داده بخشی از حالت معتبر Application باشد. اگر یک Property طبق قرارداد سیستم باید حتماً وجود داشته باشد، استفاده افراطی از `?.` می‌تواند یک خطای واقعی را پنهان کرده و آن را به `undefined` تبدیل کند. بنابراین `?.` باید بر اساس Semantic داده استفاده شود، نه صرفاً برای جلوگیری از Runtime Error.

---

### اگر پرسیده شد: «Destructuring و Optional Chaining چه رابطه‌ای دارند؟»

پاسخ مناسب:

> این دو ابزار دو مسئله متفاوت را حل می‌کنند. Destructuring برای Extraction و دسترسی خواناتر به Propertyهای موردنیاز است، درحالی‌که Optional Chaining برای Safe Access به مسیرهایی است که ممکن است `null` یا `undefined` باشند. در Applicationهای واقعی می‌توان آن‌ها را با `??` ترکیب کرد تا Extraction، Safe Access و Fallback به‌صورت واضح مدل شوند.

---

### اگر پرسیده شد: «چرا `??` از نظر Semantic با `||` متفاوت است؟»

پاسخ مناسب:

> `||` بر مبنای Truthiness تصمیم می‌گیرد، درحالی‌که `??` بر مبنای Nullish بودن تصمیم می‌گیرد. `??` فقط `null` و `undefined` را نبود Value تلقی می‌کند. این تفاوت برای Application Data مهم است، زیرا `0`، `false` و String خالی ممکن است Valueهای معتبر باشند.

---

### اگر پرسیده شد: «چگونه Safe Data Access را در یک API Response طراحی می‌کنید؟»

پاسخ مناسب:

> ابتدا باید مشخص کنم کدام داده‌ها Required و کدام Optional هستند. برای Extraction داده‌های مشخص می‌توانم از Destructuring استفاده کنم. برای مسیرهای Optional از Optional Chaining و برای تعیین Fallback در صورت `null` یا `undefined` از `??` استفاده می‌کنم. در عین حال از Optional Chaining افراطی اجتناب می‌کنم تا خطاهای واقعی Data Contract پنهان نشوند.

مثال:

```js
const { user } = response;

const city = user.profile?.city ?? 'Unknown';
```

این Code سه Intent مشخص دارد:

```text
Extract user
↓
Access optional profile/city
↓
Use fallback when nullish
```

---

# Conclusion

در این فصل یک مسئله مهم در کار با Objectها را بررسی کردیم:

> چگونه فقط داده موردنیاز را به‌صورت خوانا استخراج کنیم و در عین حال هنگام کار با داده‌های Optional از Access ناامن جلوگیری کنیم؟

Object Destructuring پاسخ بخش اول است:

```js
const { name, email } = user;
```

Renaming به ما اجازه می‌دهد نام Variable را با نیاز Application هماهنگ کنیم:

```js
const { name: userName } = user;
```

Default Values نیز برای زمانی مناسب هستند که Property مقدار `undefined` داشته باشد:

```js
const { role = 'user' } = user;
```

برای ساختارهای Nested و قابل‌اعتماد می‌توان از Nested Destructuring استفاده کرد:

```js
const {
  address: { city }
} = user;
```

اما وقتی ساختار داده Optional است، Optional Chaining ابزار مناسب‌تری است:

```js
const city = user.profile?.city;
```

و اگر برای `null` یا `undefined` به یک مقدار جایگزین نیاز داشته باشیم:

```js
const city = user.profile?.city ?? 'Unknown';
```

در نتیجه، مدل ذهنی این فصل باید چنین باشد:

```text
Object
   ↓
چه داده‌ای لازم است؟
   ↓
Destructuring
   ↓
نام Variable مناسب است؟
   ↓
Renaming
   ↓
ممکن است Property undefined باشد؟
   ↓
Default Value
   ↓
ساختار Nested و قابل‌اعتماد است؟
   ↓
Nested Destructuring
   │
   └── اگر Optional است
          ↓
     Optional Chaining
          ↓
     null / undefined؟
          ↓
     Nullish Coalescing
          ↓
        Fallback
```

هدف اصلی این Syntaxها کوتاه‌تر کردن Code نیست.

هدف، بیان دقیق‌تر Intent و ساختن یک مسیر خوانا و قابل اعتماد برای دسترسی به Data است.

در فصل بعد، وارد Syntax دیگری برای کار با داده‌های Collection و Object خواهیم شد و بررسی می‌کنیم JavaScript چگونه می‌تواند داده‌ها را **جمع** یا **گسترش** دهد.
