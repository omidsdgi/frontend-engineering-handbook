# Chapter 25 — Object Utilities and Practical Patterns

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* هدف و کاربرد **Object Utilities** را در JavaScript توضیح دهید.
* تفاوت `Object.keys()`، `Object.values()` و `Object.entries()` را تشخیص دهید.
* برای Inspection یک Object، Utility مناسب را انتخاب کنید.
* داده‌های موجود در Object را برای پردازش به شکل مناسب تبدیل کنید.
* از `Object.entries()` برای Mapping و ساخت مجدد Object استفاده کنید.
* Objectهای واقعی مانند API Data، Configuration و Application State را تحلیل کنید.
* Object را به‌عنوان یک مدل از داده‌های مرتبط طراحی کنید.
* Objectهای خوانا، قابل پیش‌بینی و قابل نگهداری ایجاد کنید.
* از پیچیده کردن بی‌دلیل Object Structure جلوگیری کنید.
* تفاوت میان **نگاه کردن به Object** و **تغییر دادن Object** را درک کنید.
* الگوهای رایج Transformation را در کدهای واقعی تشخیص دهید.

---

# Core Question

> **چگونه با Objectهای واقعی و داده‌های Application به‌صورت حرفه‌ای کار کنیم؟**

در فصل‌های قبل، Object را به‌عنوان ساختاری برای نگهداری Propertyها و Methodها شناختیم.

سپس با **Enhanced Object Literals**، **Destructuring**، **Optional Chaining**، **Nullish Coalescing** و **Rest / Spread** یاد گرفتیم که چگونه Objectها را خواناتر ایجاد، استخراج و گسترش دهیم.

اما در یک Application واقعی فقط ایجاد و دسترسی به Object کافی نیست.

داده‌ای از API دریافت می‌کنیم.

باید آن را بررسی کنیم.

بخشی از آن را استخراج کنیم.

ساختار داده را برای یک نیاز خاص تغییر دهیم.

یا از روی آن Object جدیدی بسازیم.

برای انجام این کارها، JavaScript چند Utility مهم در اختیار ما قرار می‌دهد:

```javascript
Object.keys()
Object.values()
Object.entries()
```

این فصل از **Inspection** شروع می‌کند، سپس به **Transformation** می‌رسد و در نهایت نشان می‌دهد چگونه این ابزارها می‌توانند به طراحی Objectهای قابل نگهداری در Application کمک کنند.

---

# Object Inspection

وقتی یک Object را دریافت می‌کنیم، همیشه نمی‌خواهیم مستقیماً به یک Property خاص دسترسی پیدا کنیم.

گاهی می‌خواهیم بدانیم:

* چه Propertyهایی دارد؟
* چه Valueهایی دارد؟
* رابطه Key و Valueهای آن چیست؟

در این حالت، به جای بررسی دستی Object می‌توانیم آن را به یک Collection قابل پردازش تبدیل کنیم.

سه Utility اصلی برای این کار عبارت‌اند از:

```javascript
Object.keys()
Object.values()
Object.entries()
```

تفاوت اصلی آن‌ها در نوع داده‌ای است که از Object استخراج می‌کنند.

---

## `Object.keys()`

### چرا؟

گاهی فقط نام Propertyها برای ما اهمیت دارد.

برای مثال، یک Configuration Object داریم:

```javascript
const config = {
  theme: 'dark',
  language: 'en',
  notifications: true
};
```

اگر بخواهیم نام تمام Propertyها را بررسی کنیم، می‌توانیم از `Object.keys()` استفاده کنیم.

### Definition

`Object.keys()` یک Array شامل **Own Enumerable Property Keys** یک Object برمی‌گرداند.

مثال:

```javascript
const config = {
  theme: 'dark',
  language: 'en',
  notifications: true
};

const keys = Object.keys(config);

console.log(keys);
```

خروجی:

```text
['theme', 'language', 'notifications']
```

در نتیجه:

```text
Object
  ↓
Object.keys()
  ↓
Array of Keys
```

---

## چرا نتیجه Array است؟

Object برای نگهداری ارتباط:

```text
key → value
```

طراحی شده است.

اما زمانی که می‌خواهیم روی مجموعه‌ای از Keyها پردازش انجام دهیم، داشتن یک Array بسیار مناسب‌تر است.

مثلاً:

```javascript
const keys = Object.keys(config);

for (const key of keys) {
  console.log(key);
}
```

اکنون می‌توانیم Keyها را مانند یک Collection پردازش کنیم.

---

## نکته فنی

`Object.keys()` فقط **Own Enumerable Properties** را در نتیجه قرار می‌دهد.

بنابراین Propertyهای Inherited در خروجی آن قرار نمی‌گیرند.

این نکته برای تحلیل دقیق رفتار Object مهم است و در مباحث Prototypeها اهمیت بیشتری پیدا خواهد کرد.

---

# `Object.values()`

گاهی Keyها برای ما مهم نیستند و فقط Valueها را می‌خواهیم.

برای مثال:

```javascript
const prices = {
  laptop: 1200,
  mouse: 50,
  keyboard: 100
};
```

اگر بخواهیم تمام قیمت‌ها را داشته باشیم:

```javascript
const values = Object.values(prices);

console.log(values);
```

خروجی:

```text
[1200, 50, 100]
```

### Definition

`Object.values()` یک Array شامل **Own Enumerable Property Values** یک Object برمی‌گرداند.

مدل ذهنی:

```text
Object
  ↓
Object.values()
  ↓
Array of Values
```

---

## یک کاربرد واقعی

فرض کنید می‌خواهیم مجموع قیمت‌های موجود در یک Object را محاسبه کنیم.

```javascript
const prices = {
  laptop: 1200,
  mouse: 50,
  keyboard: 100
};

const total = Object.values(prices)
  .reduce((sum, price) => sum + price, 0);

console.log(total);
```

در اینجا Object ابتدا به Collectionای از Valueها تبدیل شده است:

```text
{
  laptop: 1200,
  mouse: 50,
  keyboard: 100
}

        ↓ Object.values()

[1200, 50, 100]
```

سپس Collection پردازش شده است.

هدف این مثال آموزش `reduce()` نیست؛ بلکه نشان دادن این ایده است که Utility می‌تواند داده Object را به شکلی تبدیل کند که برای پردازش مناسب‌تر باشد.

---

# `Object.entries()`

تا اینجا دو حالت را دیدیم:

```javascript
Object.keys()
```

فقط Keyها را می‌دهد.

```javascript
Object.values()
```

فقط Valueها را می‌دهد.

اما گاهی ارتباط بین Key و Value را باید حفظ کنیم.

اینجا `Object.entries()` مناسب است.

### Definition

`Object.entries()` یک Array از Entryها برمی‌گرداند که هر Entry یک Array دو عضوی به شکل:

```text
[key, value]
```

است.

مثال:

```javascript
const user = {
  name: 'Omid',
  role: 'developer',
  active: true
};

const entries = Object.entries(user);

console.log(entries);
```

خروجی:

```text
[
  ['name', 'Omid'],
  ['role', 'developer'],
  ['active', true]
]
```

مدل ذهنی:

```text
Object
  ↓
Object.entries()
  ↓
[
  [key, value],
  [key, value],
  [key, value]
]
```

---

# تفاوت سه Utility

اکنون می‌توانیم تفاوت آن‌ها را در یک جدول خلاصه کنیم:

| Utility               | نتیجه                   |
| --------------------- | ----------------------- |
| `Object.keys(obj)`    | Array از Keyها          |
| `Object.values(obj)`  | Array از Valueها        |
| `Object.entries(obj)` | Array از `[key, value]` |

مثلاً برای:

```javascript
const user = {
  name: 'Omid',
  role: 'developer'
};
```

نتیجه‌ها:

```javascript
Object.keys(user);
// ['name', 'role']

Object.values(user);
// ['Omid', 'developer']

Object.entries(user);
// [['name', 'Omid'], ['role', 'developer']]
```

### مدل ذهنی اصلی

```text
keys
  → فقط Key

values
  → فقط Value

entries
  → Key + Value
```

انتخاب Utility باید بر اساس داده‌ای باشد که برای عملیات بعدی نیاز داریم.

---

# Object Transformation

Inspection فقط مرحله اول است.

در Applicationهای واقعی معمولاً بعد از بررسی Object، می‌خواهیم داده را تغییر شکل دهیم.

برای مثال:

```text
API Object
    ↓
Inspection
    ↓
Extract relevant data
    ↓
Transform
    ↓
Application Object
```

این فرایند را می‌توان **Object Transformation** نامید.

---

# Extracting Data

فرض کنید API چنین داده‌ای برگرداند:

```javascript
const user = {
  id: 101,
  name: 'Omid',
  email: 'omid@example.com',
  role: 'developer',
  active: true
};
```

Application فقط به `name` و `role` نیاز دارد.

می‌توانیم یک Object جدید ایجاد کنیم:

```javascript
const profile = {
  name: user.name,
  role: user.role
};
```

این ساده‌ترین شکل Transformation است.

اما وقتی داده Dynamic باشد، استفاده از `Object.entries()` می‌تواند مفیدتر باشد.

---

# Mapping Entries

فرض کنید می‌خواهیم تمام Priceها را ۱۰ درصد افزایش دهیم.

```javascript
const prices = {
  laptop: 1000,
  mouse: 50,
  keyboard: 100
};
```

ابتدا Object را به Entryها تبدیل می‌کنیم:

```javascript
Object.entries(prices);
```

نتیجه:

```javascript
[
  ['laptop', 1000],
  ['mouse', 50],
  ['keyboard', 100]
]
```

اکنون می‌توانیم هر Entry را Transform کنیم.

```javascript
const updatedPrices = Object.entries(prices).map(
  ([product, price]) => [product, price * 1.1]
);
```

نتیجه:

```javascript
[
  ['laptop', 1100],
  ['mouse', 55],
  ['keyboard', 110]
]
```

اما هنوز یک Array داریم.

اگر هدف نهایی ما Object باشد، باید آن را دوباره به Object تبدیل کنیم.

---

# Rebuilding Objects

برای ساخت Object از Entryها می‌توانیم از:

```javascript
Object.fromEntries()
```

استفاده کنیم.

```javascript
const updatedPrices = Object.fromEntries(
  Object.entries(prices).map(
    ([product, price]) => [product, price * 1.1]
  )
);
```

نتیجه:

```javascript
{
  laptop: 1100,
  mouse: 55,
  keyboard: 110
}
```

مدل ذهنی این Transformation بسیار مهم است:

```text
Object
  ↓
Object.entries()
  ↓
Array of [key, value]
  ↓
map()
  ↓
Transformed entries
  ↓
Object.fromEntries()
  ↓
New Object
```

این الگو زمانی بسیار مفید است که بخواهیم **تمام یا بخشی از Propertyهای Object را به‌صورت عمومی Transform کنیم.**

---

# چرا `Object.entries()` در Transformation مهم است؟

فرض کنید فقط یک Property مشخص را تغییر دهیم:

```javascript
const updatedUser = {
  ...user,
  role: 'admin'
};
```

این الگو در فصل قبل بررسی شد و برای تغییر چند Property مشخص بسیار مناسب است.

اما اگر بخواهیم روی مجموعه‌ای از Propertyها بر اساس یک Rule عمومی کار کنیم، `Object.entries()` ابزار مناسب‌تری است.

مثلاً:

```javascript
const settings = {
  fontSize: 14,
  spacing: 8,
  radius: 4
};
```

اگر بخواهیم همه Valueها را تغییر دهیم:

```javascript
const updatedSettings = Object.fromEntries(
  Object.entries(settings).map(([key, value]) => [
    key,
    value * 2
  ])
);
```

نتیجه:

```javascript
{
  fontSize: 28,
  spacing: 16,
  radius: 8
}
```

اینجا Transformation روی **ساختار کلی Object** انجام شده است.

---

# Filtering Object Properties

یکی دیگر از الگوهای رایج، نگه‌داشتن فقط برخی Propertyهاست.

فرض کنید API اطلاعات بیشتری از نیاز Application ارسال کرده است:

```javascript
const user = {
  id: 101,
  name: 'Omid',
  email: 'omid@example.com',
  password: 'secret',
  role: 'developer'
};
```

اگر بخواهیم Objectی بدون `password` بسازیم، یکی از روش‌های مناسب استفاده از Object Rest است:

```javascript
const { password, ...safeUser } = user;
```

اکنون:

```javascript
safeUser
```

شامل داده‌های باقی‌مانده است.

این الگو در فصل 23 بررسی شد.

اما اگر Rule مربوط به انتخاب Propertyها Dynamic باشد، می‌توانیم از Entries استفاده کنیم:

```javascript
const publicUser = Object.fromEntries(
  Object.entries(user).filter(
    ([key]) => key !== 'password'
  )
);
```

نتیجه:

```javascript
{
  id: 101,
  name: 'Omid',
  email: 'omid@example.com',
  role: 'developer'
}
```

نکته مهم این است که اینجا از چند مفهوم قبلی در یک جریان جدید استفاده می‌کنیم:

```text
Object
↓
entries
↓
filter
↓
fromEntries
↓
New Object
```

هدف این فصل آموزش `filter()` نیست؛ هدف شناخت یک **Transformation Pattern برای Object** است.

---

# Real Application Data

Object Utilityها زمانی ارزش واقعی خود را نشان می‌دهند که با داده‌های Application کار کنیم.

سه مورد مهم در این فصل:

```text
API Data
Configuration
Application State
```

---

# API Objects

داده‌های دریافتی از API معمولاً دقیقاً با ساختار مورد نیاز UI یا Application یکسان نیستند.

برای مثال:

```javascript
const recipe = {
  id: 42,
  title: 'Pasta',
  publisher: 'Food Studio',
  cookingTime: 35,
  servings: 4,
  image: 'pasta.jpg'
};
```

ممکن است یک Component فقط به این داده‌ها نیاز داشته باشد:

```javascript
{
  id,
  title,
  image
}
```

می‌توانیم Object مورد نیاز را بسازیم:

```javascript
const cardData = {
  id: recipe.id,
  title: recipe.title,
  image: recipe.image
};
```

این کار یک مرز مفید میان **External Data** و **Application Data** ایجاد می‌کند.

Application مجبور نیست در تمام بخش‌های خود به ساختار کامل API وابسته باشد.

---

# چرا Data Modeling مهم است؟

Object فقط یک Container برای Propertyها نیست.

در Application واقعی، Object معمولاً یک **مدل داده** را نمایش می‌دهد.

برای مثال:

```javascript
const order = {
  id: 501,
  customer: 'Omid',
  total: 250,
  status: 'paid'
};
```

این Object می‌تواند مفهوم یک Order را در Application مدل کند.

اگر Propertyها به‌صورت منظم و معنادار انتخاب شوند، کدهایی که با آن کار می‌کنند نیز قابل فهم‌تر خواهند بود.

اما اگر Object به‌مرور زمان Propertyهای نامرتبط زیادی دریافت کند، مدل داده مبهم می‌شود.

---

# Configuration Objects

Objectها برای نگهداری Configuration نیز بسیار مناسب هستند.

مثلاً:

```javascript
const config = {
  theme: 'dark',
  language: 'en',
  pageSize: 20
};
```

مزیت این مدل این است که Configurationهای مرتبط در یک ساختار قرار گرفته‌اند.

کد مصرف‌کننده می‌تواند به شکل مشخصی با آن کار کند:

```javascript
console.log(config.theme);
console.log(config.pageSize);
```

اگر Configuration بزرگ شود، نام‌گذاری و سازمان‌دهی Propertyها اهمیت بیشتری پیدا می‌کند.

---

# Application State

در Applicationهای واقعی، Object می‌تواند State مرتبط را نیز مدل کند.

برای مثال:

```javascript
const state = {
  user: null,
  cartCount: 3,
  isLoading: false
};
```

این ساختار نشان می‌دهد Application در یک لحظه چه اطلاعاتی را نگهداری می‌کند.

اما State خوب فقط به معنی «قرار دادن همه چیز داخل یک Object» نیست.

باید مشخص باشد:

* هر Property چه مفهومی دارد؟
* آیا Property واقعاً به این State تعلق دارد؟
* آیا نام آن دقیق است؟
* آیا داده تکراری وجود دارد؟

این پرسش‌ها بخشی از Data Modeling هستند.

---

# Transformation Boundaries

یکی از الگوهای مهم در Application این است که داده را در مرز مناسب Transform کنیم.

فرض کنید API داده زیر را برگرداند:

```javascript
const apiUser = {
  user_id: 101,
  first_name: 'Omid',
  account_status: 'active'
};
```

Application می‌تواند آن را به مدل داخلی خود تبدیل کند:

```javascript
const user = {
  id: apiUser.user_id,
  name: apiUser.first_name,
  active: apiUser.account_status === 'active'
};
```

اکنون بخش‌های دیگر Application می‌توانند با مدل ساده‌تر کار کنند:

```javascript
console.log(user.name);
console.log(user.active);
```

مدل ذهنی:

```text
External Data
      ↓
Transformation
      ↓
Application Model
      ↓
UI / Business Logic
```

این الگو باعث می‌شود وابستگی Application به شکل خام داده خارجی کاهش پیدا کند.

---

# Maintainable Objects

اکنون به آخرین بخش Concept Flow می‌رسیم:

**Maintainable Objects**

Object قابل نگهداری Objectی نیست که صرفاً کوچک باشد.

Object قابل نگهداری باید:

* مفهوم مشخصی داشته باشد.
* Propertyهای مرتبط را کنار هم نگه دارد.
* نام‌های واضح داشته باشد.
* از داده‌های تکراری تا حد ممکن جلوگیری کند.
* ساختار آن قابل پیش‌بینی باشد.
* مسئولیت بیش از حد نداشته باشد.

---

# Naming

نام Property باید معنای آن را مشخص کند.

مثلاً:

```javascript
const user = {
  n: 'Omid',
  a: true
};
```

کد کوتاه است، اما قابل فهم نیست.

بهتر:

```javascript
const user = {
  name: 'Omid',
  active: true
};
```

نام‌گذاری خوب بخشی از طراحی Object است، نه صرفاً مسئله ظاهری.

---

# Organization

اگر چند Property یک مفهوم مشترک دارند، گاهی بهتر است در یک Object داخلی قرار بگیرند.

مثلاً:

```javascript
const user = {
  name: 'Omid',
  email: 'omid@example.com',
  address: {
    city: 'Baku',
    country: 'Azerbaijan'
  }
};
```

در اینجا:

```text
user
 ├── name
 ├── email
 └── address
      ├── city
      └── country
```

ساختار Object رابطه میان داده‌ها را بهتر نشان می‌دهد.

اما Nested Object نباید صرفاً برای ایجاد ساختار بیشتر استفاده شود.

اگر Nesting بیش از حد شود، دسترسی و Transformation داده دشوارتر خواهد شد.

---

# Avoiding Complexity

این Object را در نظر بگیرید:

```javascript
const app = {
  user: {
    profile: {
      account: {
        settings: {
          preferences: {
            theme: 'dark'
          }
        }
      }
    }
  }
};
```

ممکن است ساختار واقعی Application پیچیده باشد، اما باید مراقب باشیم Object Structure را بدون دلیل عمیق نکنیم.

ساختار پیچیده هزینه دارد:

```text
More Nesting
↓
Harder Access
↓
Harder Transformation
↓
Harder Maintenance
```

هدف طراحی Object ایجاد ساختاری است که **معنای داده را روشن کند**، نه اینکه صرفاً همه داده‌ها را داخل لایه‌های بیشتر قرار دهد.

---

# Object Utility یا Property Access؟

این دو را نباید با هم اشتباه گرفت.

اگر دقیقاً می‌دانیم چه Propertyای را می‌خواهیم:

```javascript
user.name
```

معمولاً ساده‌ترین انتخاب است.

اگر می‌خواهیم تمام Keyها را بررسی کنیم:

```javascript
Object.keys(user);
```

اگر تمام Valueها را می‌خواهیم:

```javascript
Object.values(user);
```

اگر به Key و Value با هم نیاز داریم:

```javascript
Object.entries(user);
```

پس Utilityها جایگزین Property Access نیستند.

آن‌ها برای **کار کردن با Object به‌عنوان یک مجموعه از Propertyها** مناسب هستند.

---

# Best Practices

## 1. Utility را بر اساس هدف انتخاب کنید

```javascript
Object.keys(user);
```

وقتی Keyها مهم هستند.

```javascript
Object.values(user);
```

وقتی Valueها مهم هستند.

```javascript
Object.entries(user);
```

وقتی Key و Value باید با هم پردازش شوند.

---

## 2. Object را فقط برای پردازش به Array تبدیل کنید

تبدیل Object به Array باید هدف مشخصی داشته باشد.

مثلاً:

```javascript
Object.entries(settings)
```

زمانی منطقی است که بخواهیم روی Propertyها Iteration یا Transformation انجام دهیم.

---

## 3. داده خارجی را در مرز مناسب Transform کنید

اگر API Structure با Application Model متفاوت است، بهتر است Transformation در نقطه مشخصی انجام شود.

```text
API
↓
Transformation
↓
Application Model
```

نه اینکه تفاوت ساختار API در تمام Application پخش شود.

---

## 4. Objectها را با یک مسئولیت مشخص طراحی کنید

این Object:

```javascript
const user = {
  name: 'Omid',
  email: 'omid@example.com',
  theme: 'dark',
  cartCount: 3,
  lastApiRequest: 1200
};
```

ممکن است داده‌های متعلق به چند مفهوم متفاوت را در یک ساختار قرار داده باشد.

قبل از اضافه کردن Property جدید باید پرسید:

> آیا این داده واقعاً بخشی از همین مدل است؟

---

## 5. Transformation را قابل خواندن نگه دارید

این کد:

```javascript
const result = Object.fromEntries(
  Object.entries(data)
    .filter(([key]) => key !== 'internal')
    .map(([key, value]) => [key, transform(value)])
);
```

ممکن است برای یک Transformation پیچیده مناسب باشد.

اما اگر Logic زیاد شود، بهتر است آن را به Functionهای کوچک‌تر تقسیم کنیم.

هدف استفاده از Utilityها **کاهش پیچیدگی** است، نه مخفی کردن آن.

---

# Common Mistakes

## اشتباه اول: تصور اینکه `Object.keys()` تمام Propertyها را برمی‌گرداند

`Object.keys()` فقط **Own Enumerable Properties** را برمی‌گرداند.

Propertyهای Inherited در نتیجه آن قرار نمی‌گیرند.

---

## اشتباه دوم: انتظار Object از `Object.entries()`

خروجی:

```javascript
Object.entries(user);
```

یک Array است، نه Object.

مثلاً:

```javascript
[
  ['name', 'Omid'],
  ['role', 'developer']
]
```

---

## اشتباه سوم: استفاده از `Object.values()` وقتی Key مهم است

اگر بعداً باید بدانیم هر Value متعلق به کدام Property بوده است:

```javascript
Object.values()
```

اطلاعات Key را از بین می‌برد.

در این حالت:

```javascript
Object.entries()
```

انتخاب مناسب‌تری است.

---

## اشتباه چهارم: استفاده از Utility برای دسترسی به یک Property

اگر فقط این را می‌خواهیم:

```javascript
user.email
```

نیازی به:

```javascript
Object.entries(user)
```

نداریم.

Utility زمانی ارزشمند است که بخواهیم با مجموعه Propertyها کار کنیم.

---

## اشتباه پنجم: تصور اینکه Transformation همیشه Object اصلی را تغییر می‌دهد

مثلاً:

```javascript
const result = Object.fromEntries(
  Object.entries(data).map(...)
);
```

یک Object جدید ایجاد می‌کند.

این با Mutation مستقیم Object اصلی متفاوت است.

---

## اشتباه ششم: پیچیده کردن بی‌دلیل Object

Nested Object بیشتر همیشه به معنای طراحی بهتر نیست.

اگر ساختار داده بیش از حد عمیق شود، فهم و استفاده از آن دشوارتر می‌شود.

---

## اشتباه هفتم: وابسته کردن کل Application به ساختار خام API

اگر ساختار API مستقیماً در همه بخش‌های Application استفاده شود، تغییر API می‌تواند بخش‌های زیادی از Application را تحت تأثیر قرار دهد.

Transformation می‌تواند یک مرز میان External Data و Application Model ایجاد کند.

---

## اشتباه هشتم: مخلوط کردن داده‌های نامرتبط

Objectی که Propertyهای متعلق به چند مفهوم مختلف را نگهداری می‌کند، به‌مرور زمان سخت‌تر قابل فهم و نگهداری می‌شود.

---

# Summary

در این فصل دیدیم که Object در Application واقعی فقط یک ساختار برای ذخیره Propertyها نیست.

گاهی باید Object را **Inspect** کنیم.

گاهی باید داده‌های آن را **Transform** کنیم.

و گاهی باید از آن برای ایجاد یک **Data Model** واضح و قابل نگهداری استفاده کنیم.

برای Inspection سه Utility اصلی داریم:

```javascript
Object.keys()
Object.values()
Object.entries()
```

`Object.keys()` Keyها را برمی‌گرداند.

`Object.values()` Valueها را برمی‌گرداند.

`Object.entries()` ارتباط میان Key و Value را در قالب Entryهای `[key, value]` حفظ می‌کند.

این تفاوت، پایه انتخاب Utility مناسب است.

سپس دیدیم که `Object.entries()` می‌تواند Object را به Collectionای تبدیل کند که روی آن Transformation انجام دهیم.

در صورت نیاز می‌توانیم Entryهای جدید را با:

```javascript
Object.fromEntries()
```

دوباره به Object تبدیل کنیم.

در Applicationهای واقعی این الگو برای کار با:

* API Data
* Configuration
* Application State

مفید است.

اما ابزارهای Object زمانی ارزش واقعی دارند که در خدمت **Data Modeling** و **Maintainability** باشند.

هدف این نیست که Object را تا حد ممکن پیچیده کنیم.

هدف این است که داده‌ها را در ساختاری قرار دهیم که:

* مفهوم مشخصی داشته باشد.
* نام‌های واضح داشته باشد.
* مسئولیت مشخصی داشته باشد.
* برای Transformation و استفاده مجدد مناسب باشد.

---

# Key Takeaways

* Object برای مدل کردن داده‌های مرتبط بسیار مناسب است.
* Object Inspection یعنی بررسی ساختار و Propertyهای Object.
* `Object.keys()` یک Array از Own Enumerable Keys می‌سازد.
* `Object.values()` یک Array از Own Enumerable Values می‌سازد.
* `Object.entries()` یک Array از `[key, value]` می‌سازد.
* `Object.keys()` زمانی مناسب است که Keyها مهم باشند.
* `Object.values()` زمانی مناسب است که فقط Valueها مورد نیاز باشند.
* `Object.entries()` زمانی مناسب است که Key و Value باید با هم پردازش شوند.
* نتیجه این سه Utility یک Array است.
* Utilityهای Object جایگزین Property Access معمولی نیستند.
* برای یک Property مشخص معمولاً `object.property` ساده‌تر است.
* `Object.entries()` می‌تواند پایه یک Transformation Pattern باشد.
* `Object.fromEntries()` می‌تواند Entryهای Transform‌شده را دوباره به Object تبدیل کند.
* Object Transformation می‌تواند داده خارجی را به Application Model تبدیل کند.
* API Data همیشه نباید مستقیماً در تمام Application استفاده شود.
* Configuration Object می‌تواند تنظیمات مرتبط را در یک ساختار مشخص نگه دارد.
* Application State نیز می‌تواند با Object مدل شود.
* Data Modeling یعنی انتخاب ساختار مناسب برای نمایش یک مفهوم در Application.
* Naming مناسب خوانایی Object را افزایش می‌دهد.
* Organization مناسب رابطه میان داده‌ها را روشن‌تر می‌کند.
* Nesting بیش از حد می‌تواند Complexity ایجاد کند.
* Object Utility باید برای کاهش پیچیدگی استفاده شود، نه ایجاد آن.
* Maintainability نتیجه طراحی قابل فهم، قابل پیش‌بینی و متناسب با مسئولیت Object است.

---

# Technical Interview

## سطح Junior

### ⭐ `Object.keys()` چه کاری انجام می‌دهد؟

`Object.keys()` یک Array شامل Own Enumerable Property Keys یک Object برمی‌گرداند.

```javascript
const user = {
  name: 'Omid',
  role: 'developer'
};

Object.keys(user);
// ['name', 'role']
```

---

### ⭐ تفاوت `Object.keys()` و `Object.values()` چیست؟

`Object.keys()` Keyها را برمی‌گرداند، در حالی که `Object.values()` Valueها را برمی‌گرداند.

```javascript
Object.keys(user);
// ['name', 'role']

Object.values(user);
// ['Omid', 'developer']
```

---

### ⭐ `Object.entries()` چه چیزی برمی‌گرداند؟

یک Array از Entryها که هر Entry شامل `[key, value]` است.

```javascript
Object.entries(user);
```

نتیجه:

```javascript
[
  ['name', 'Omid'],
  ['role', 'developer']
]
```

---

### ⭐ چرا نتیجه `Object.keys()` یک Array است؟

زیرا Utility داده Object را به Collectionای از Keyها تبدیل می‌کند تا بتوان آن‌ها را با ابزارهای Iteration و Transformation پردازش کرد.

---

### ⭐ چه زمانی `Object.entries()` را به `Object.values()` ترجیح می‌دهید؟

وقتی علاوه بر Value، به Key آن نیز نیاز داریم.

---

## سطح Mid-Level

### ⭐ چگونه تمام Valueهای یک Object را پردازش می‌کنید؟

می‌توان Object را با `Object.values()` به Array تبدیل و سپس آن را پردازش کرد.

```javascript
const prices = {
  laptop: 1000,
  mouse: 50,
  keyboard: 100
};

const pricesWithTax = Object.values(prices).map(
  price => price * 1.1
);
```

نکته مهم این است که در این روش ارتباط Value با Key حفظ نمی‌شود.

---

### ⭐ چگونه تمام Propertyهای یک Object را Transform و دوباره Object ایجاد می‌کنید؟

می‌توان از الگوی:

```text
Object
↓
Object.entries()
↓
Transformation
↓
Object.fromEntries()
↓
Object
```

استفاده کرد.

مثال:

```javascript
const prices = {
  laptop: 1000,
  mouse: 50
};

const updatedPrices = Object.fromEntries(
  Object.entries(prices).map(([key, value]) => [
    key,
    value * 1.1
  ])
);
```

---

### ⭐ چرا `Object.entries()` برای Transformation قدرتمند است؟

زیرا Object را به مجموعه‌ای از `[key, value]` تبدیل می‌کند و در نتیجه می‌توان Key و Value را هم‌زمان بررسی یا تغییر داد.

---

### ⭐ چگونه فقط بعضی Propertyهای یک Object را نگه می‌دارید؟

برای Ruleهای ساده می‌توان از Object Rest استفاده کرد:

```javascript
const { password, ...safeUser } = user;
```

برای Ruleهای Dynamic می‌توان از `Object.entries()`، یک Transformation مانند `filter()` و سپس `Object.fromEntries()` استفاده کرد.

---

### ⭐ چرا API Data را مستقیماً در همه Application استفاده نمی‌کنیم؟

زیرا ساختار API ممکن است با نیاز داخلی Application متفاوت باشد و در آینده تغییر کند.

Transformation می‌تواند یک مرز ایجاد کند:

```text
API Data
↓
Transformation
↓
Application Model
```

در نتیجه وابستگی بخش‌های مختلف Application به ساختار خارجی کاهش می‌یابد.

---

### ⭐ آیا `Object.keys()` روی Inherited Properties نیز کار می‌کند؟

خیر.

`Object.keys()` فقط Own Enumerable Properties را برمی‌گرداند.

---

## سطح Senior

### ⭐ چگونه Object Utilities را در طراحی Application به کار می‌برید؟

Object Utilities را صرفاً ابزار Syntax نمی‌بینم.

آن‌ها بخشی از یک جریان Data Transformation هستند:

```text
External Data
↓
Inspection
↓
Selection
↓
Transformation
↓
Application Model
```

مثلاً برای API Data می‌توانم با `Object.entries()` Propertyها را بررسی و Transform کنم و سپس با `Object.fromEntries()` مدل داخلی مورد نیاز را ایجاد کنم.

هدف اصلی کاهش Coupling و ایجاد Data Model قابل پیش‌بینی است.

---

### ⭐ تفاوت Property Access و Object Inspection چیست؟

Property Access برای دسترسی به یک Property مشخص است:

```javascript
user.name;
```

اما Object Inspection زمانی است که می‌خواهیم Object را به‌عنوان مجموعه‌ای از Propertyها بررسی کنیم:

```javascript
Object.keys(user);
Object.values(user);
Object.entries(user);
```

بنابراین این دو جایگزین مستقیم یکدیگر نیستند.

---

### ⭐ چه زمانی استفاده از `Object.entries()` می‌تواند نشانه طراحی نامناسب باشد؟

زمانی که صرفاً برای دسترسی به یک یا چند Property مشخص، کل Object را به Entryهای Array تبدیل کنیم.

مثلاً برای:

```javascript
user.name
```

استفاده از:

```javascript
Object.entries(user)
```

پیچیدگی غیرضروری ایجاد می‌کند.

`Object.entries()` زمانی ارزشمند است که واقعاً بخواهیم روی مجموعه Propertyهای Object عملیات عمومی مانند Iteration، Filtering یا Transformation انجام دهیم.

---

### ⭐ چگونه بین Object Structure و Maintainability تعادل برقرار می‌کنید؟

Object باید یک مفهوم مشخص را مدل کند.

Propertyها باید مرتبط و دارای نام واضح باشند.

اگر Nesting بیش از حد باعث سخت شدن دسترسی و Transformation شود، باید ساختار را بازبینی کرد.

معیار اصلی این نیست که Object چقدر کوچک یا بزرگ است؛ معیار این است که آیا Structure آن مفهوم داده را به‌صورت واضح و قابل پیش‌بینی نمایش می‌دهد یا خیر.

---

### ⭐ آیا Object Transformation همیشه باید Immutable باشد؟

خیر.

این فصل نمی‌گوید Mutation همیشه نادرست است.

اما ایجاد Object جدید در Transformationهای داده‌ای می‌تواند Side Effect را کاهش دهد و مرز میان داده اصلی و داده Transform‌شده را واضح‌تر کند.

انتخاب میان Mutation و ساخت Object جدید باید بر اساس نیاز Application و قرارداد داده انجام شود.

---

### ⭐ یک الگوی حرفه‌ای برای تبدیل API Data به Application Model چیست؟

یک الگوی ساده این است:

```javascript
function normalizeUser(apiUser) {
  return {
    id: apiUser.user_id,
    name: apiUser.first_name,
    active: apiUser.account_status === 'active'
  };
}
```

سپس:

```javascript
const user = normalizeUser(apiUser);
```

مدل داخلی Application دیگر به جزئیات نام‌گذاری API وابسته نیست.

این کار مرز مشخصی میان **External Representation** و **Internal Representation** ایجاد می‌کند.

---

# Golden Answers

### Junior

> **`Object.keys()`، `Object.values()` و `Object.entries()` چه تفاوتی دارند؟**

`Object.keys()` Keyها، `Object.values()` Valueها و `Object.entries()` زوج‌های `[key, value]` را در قالب Array برمی‌گردانند.

---

### Mid-Level

> **چگونه یک Object را به‌صورت عمومی Transform می‌کنید؟**

Object را با `Object.entries()` به Array از Entryها تبدیل می‌کنم، Transformation مورد نیاز را انجام می‌دهم و در صورت نیاز با `Object.fromEntries()` دوباره Object می‌سازم.

```javascript
const result = Object.fromEntries(
  Object.entries(data).map(([key, value]) => [
    key,
    transform(value)
  ])
);
```

---

### Senior

> **نقش Object Utilities در Application Architecture چیست؟**

Object Utilities فقط ابزار دسترسی به داده نیستند؛ می‌توانند بخشی از Data Transformation Pipeline باشند.

برای مثال:

```text
External Data
↓
Inspection
↓
Selection / Transformation
↓
Application Model
```

با این رویکرد، ساختار خارجی داده از مدل داخلی Application جدا می‌شود و تغییرات API کمتر به بخش‌های مختلف سیستم سرایت می‌کند.

---

# Conclusion

در ابتدای فصل، سؤال این بود:

> **چگونه با Objectهای واقعی و داده‌های Application به‌صورت حرفه‌ای کار کنیم؟**

پاسخ فقط یادگیری سه Method نیست.

```javascript
Object.keys()
Object.values()
Object.entries()
```

این Utilityها زمانی اهمیت پیدا می‌کنند که بدانیم **چه نوع داده‌ای برای مرحله بعدی نیاز داریم.**

اگر Keyها مهم‌اند:

```javascript
Object.keys()
```

اگر Valueها مهم‌اند:

```javascript
Object.values()
```

اگر Key و Value باید با هم پردازش شوند:

```javascript
Object.entries()
```

و اگر Entryهای Transform‌شده باید دوباره به Object تبدیل شوند:

```javascript
Object.fromEntries()
```

از اینجا به بعد، مسئله اصلی دیگر Syntax نیست.

مسئله، **مدل کردن داده و طراحی Transformationهای قابل فهم** است.

یک Object خوب باید مفهوم مشخصی را نمایش دهد.

نام‌های واضح داشته باشد.

داده‌های مرتبط را سازمان‌دهی کند.

و بدون پیچیدگی غیرضروری قابل استفاده و نگهداری باشد.

این نگاه، Object را از یک Container ساده به ابزاری برای **Data Modeling و طراحی Application** تبدیل می‌کند.
