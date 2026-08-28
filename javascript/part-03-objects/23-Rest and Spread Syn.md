# Chapter 23 — Rest and Spread Syntax

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Spread Syntax** را به‌صورت دقیق توضیح دهید.
* توضیح دهید Spread چگونه یک Collection را در یک Context گسترش می‌دهد.
* از Spread برای کار با **Arrays** استفاده کنید.
* از Spread برای ایجاد و ترکیب **Objects** استفاده کنید.
* مفهوم **Rest Syntax** را از Spread تشخیص دهید.
* توضیح دهید Rest چگونه Values یا Properties باقی‌مانده را جمع می‌کند.
* از Rest Parameters برای دریافت تعداد نامحدود Arguments استفاده کنید.
* از Object Rest برای استخراج Properties باقی‌مانده استفاده کنید.
* تفاوت **Shallow Copy** و Deep Copy را در حد این فصل تشخیص دهید.
* از Rest و Spread برای Copying، Merging و Updating داده‌ها استفاده کنید.
* ارتباط Rest و Spread را با الگوهای **Immutable** درک کنید.
* تفاوت Rest و Spread را در Contextهای مختلف به‌درستی تحلیل کنید.

---

# مقدمه

در فصل‌های قبل با Objectها، Properties، References و Destructuring آشنا شدیم.

در Destructuring، هدف اصلی این بود که بتوانیم داده‌های موجود در یک Collection را **استخراج** کنیم.

مثلاً:

```javascript
const user = {
  name: 'Omid',
  city: 'Baku',
  role: 'Developer'
};

const { name, city } = user;
```

در اینجا داده‌ها از Object خارج و در Variables قرار گرفتند.

اما در Applicationهای واقعی فقط استخراج داده لازم نیست.

گاهی باید:

* چند Array را با هم ترکیب کنیم.
* یک Array جدید بر اساس Array موجود بسازیم.
* چند Object را ترکیب کنیم.
* یک Object را با چند Property جدید به‌روزرسانی کنیم.
* تمام Arguments یک Function را دریافت کنیم.
* بخشی از یک Object را جدا کنیم و بقیه Properties را نگه داریم.

این مسائل یک سؤال مشترک ایجاد می‌کنند:

> چگونه داده‌ها را یا **گسترش** دهیم یا **جمع** کنیم؟

JavaScript برای این دو نیاز Syntax مشترکی در اختیار ما قرار می‌دهد:

```javascript
...
```

اما این سه نقطه در دو نقش متفاوت استفاده می‌شوند:

```text
Spread → Expand
Rest   → Collect
```

در Spread، داده‌های یک Collection را باز می‌کنیم تا عناصر آن در یک Context قرار بگیرند.

در Rest، چند Value یا Property را که باقی مانده‌اند جمع می‌کنیم و در یک Collection قرار می‌دهیم.

بنابراین برای درک این Syntax نباید فقط شکل `...` را حفظ کنیم.

باید بدانیم:

> آیا داده در حال **گسترش یافتن** است یا در حال **جمع شدن**؟

این تفاوت، مدل ذهنی اصلی این فصل است.

---

# Core Question

> **چگونه JavaScript داده‌ها را جمع یا گسترش می‌دهد؟**

جریان مفهومی فصل:

```text
Collection
↓
Spread
↓
Expansion
↓
Rest
↓
Collection of Remaining Values
↓
Arrays
↓
Objects
↓
Function Parameters
↓
Immutable Patterns
```

در این فصل، Rest و Spread را در سه Context اصلی بررسی می‌کنیم:

```text
Array
Object
Function Parameters
```

---

# Collection؛ نقطه شروع Rest و Spread

قبل از بررسی Syntax، باید یک مدل ساده از Collection داشته باشیم.

Collection یعنی مجموعه‌ای از Values که در یک ساختار نگهداری می‌شوند.

در این فصل مهم‌ترین Collectionهایی که با آن‌ها کار می‌کنیم عبارت‌اند از:

```javascript
const products = ['Laptop', 'Mouse', 'Keyboard'];
```

و:

```javascript
const user = {
  name: 'Omid',
  role: 'Developer'
};
```

در Array، Collection از Elements تشکیل شده است.

در Object، Collection از Properties تشکیل شده است.

Rest و Spread به ما اجازه می‌دهند با این داده‌ها به شکل انعطاف‌پذیرتری کار کنیم.

---

# Spread Syntax

## چرا Spread؟

فرض کنید دو Array داریم:

```javascript
const frontend = ['HTML', 'CSS'];
const backend = ['Node.js', 'SQL'];
```

می‌خواهیم یک Array جدید بسازیم که همه عناصر هر دو Array را داشته باشد.

بدون Spread می‌توانیم عناصر را یکی‌یکی اضافه کنیم:

```javascript
const skills = [
  frontend[0],
  frontend[1],
  backend[0],
  backend[1]
];
```

این روش هم طولانی است و هم با تغییر تعداد عناصر مناسب نیست.

Spread این عملیات را ساده می‌کند:

```javascript
const skills = [...frontend, ...backend];
```

اکنون:

```javascript
console.log(skills);
```

نتیجه:

```text
['HTML', 'CSS', 'Node.js', 'SQL']
```

اینجا `...frontend` به این معناست:

> عناصر موجود در `frontend` را در این Array گسترش بده.

---

## تعریف Spread

### تعریف ساده

**Spread Syntax** داده‌های یک Iterable یا Object را در یک Context دیگر گسترش می‌دهد.

برای Array:

```javascript
const numbers = [1, 2, 3];

const copy = [...numbers];
```

مفهوم ذهنی:

```text
numbers
[1, 2, 3]

      ↓ Spread

[...numbers]

      ↓

[1, 2, 3]
```

Spread یک Array را داخل Array دیگر قرار نمی‌دهد؛ عناصر آن را در Array جدید گسترش می‌دهد.

---

## تفاوت Spread با قرار دادن مستقیم Array

این دو را مقایسه کنید:

```javascript
const products = ['Laptop', 'Mouse'];

const result = [products];
```

و:

```javascript
const result = [...products];
```

در حالت اول:

```javascript
[
  ['Laptop', 'Mouse']
]
```

یک Array تو‌در‌تو ایجاد می‌شود.

اما در حالت دوم:

```javascript
[
  'Laptop',
  'Mouse'
]
```

عناصر Array در Collection جدید گسترش پیدا می‌کنند.

این یکی از مهم‌ترین کاربردهای Spread است.

---

# Spread با Array

یکی از رایج‌ترین کاربردهای Spread، ساختن Array جدید از یک Array موجود است.

```javascript
const products = ['Laptop', 'Mouse', 'Keyboard'];

const copiedProducts = [...products];
```

اکنون `copiedProducts` یک Array جدید است.

```javascript
console.log(copiedProducts === products);
```

نتیجه:

```text
false
```

چون Spread عناصر Array را در یک Array جدید قرار داده است.

---

## Copying با Spread

این الگو برای ایجاد یک **Shallow Copy** بسیار رایج است:

```javascript
const products = ['Laptop', 'Mouse'];

const copy = [...products];
```

اکنون:

```text
products ──→ ['Laptop', 'Mouse']

copy     ──→ ['Laptop', 'Mouse']
```

دو Array مستقل هستند.

بنابراین تغییر ساختاری یکی، دیگری را تغییر نمی‌دهد:

```javascript
copy.push('Keyboard');

console.log(products);
console.log(copy);
```

خروجی:

```text
['Laptop', 'Mouse']

['Laptop', 'Mouse', 'Keyboard']
```

---

# Spread و Reference

این رفتار با مفهوم Reference که در فصل Objects آموختیم ارتباط دارد.

در این کد:

```javascript
const copy = products;
```

هر دو Variable به همان Array اشاره می‌کنند.

```text
products ──┐
           ├──→ Array
copy     ──┘
```

اما در:

```javascript
const copy = [...products];
```

Array جدیدی ایجاد می‌شود:

```text
products ──→ Array A
copy     ──→ Array B
```

بنابراین Spread در Array می‌تواند برای ایجاد یک **Shallow Copy** استفاده شود.

---

# Spread برای ترکیب Arrays

Spread فقط برای Copying نیست.

می‌توانیم چند Array را نیز ترکیب کنیم:

```javascript
const primary = ['HTML', 'CSS'];
const secondary = ['JavaScript', 'TypeScript'];

const skills = [...primary, ...secondary];
```

نتیجه:

```javascript
[
  'HTML',
  'CSS',
  'JavaScript',
  'TypeScript'
]
```

همچنین می‌توانیم Values جدید را در هر نقطه قرار دهیم:

```javascript
const skills = [
  ...primary,
  'React',
  ...secondary
];
```

نتیجه:

```javascript
[
  'HTML',
  'CSS',
  'React',
  'JavaScript',
  'TypeScript'
]
```

این ویژگی باعث می‌شود Spread ابزار مناسبی برای ساخت Collectionهای جدید باشد.

---

# Expansion

اکنون می‌توانیم دقیق‌تر بگوییم Spread چه کاری انجام می‌دهد.

فرض کنید:

```javascript
const numbers = [10, 20, 30];
```

وقتی می‌نویسیم:

```javascript
[...numbers]
```

سه نقطه به‌صورت مفهومی عناصر Collection را به Context اطراف آن وارد می‌کند:

```text
[...numbers]

     ↓

[10, 20, 30]
```

پس:

> Spread یک Collection را به مجموعه‌ای از عناصر قابل استفاده در Context مقصد تبدیل می‌کند.

این Context می‌تواند یک Array جدید باشد یا Context دیگری که از Spread پشتیبانی می‌کند.

---

# Spread در Function Calls

Spread فقط در Arrayها استفاده نمی‌شود.

می‌توانیم یک Array را هنگام فراخوانی Function نیز گسترش دهیم.

فرض کنید:

```javascript
function calculateTotal(a, b, c) {
  return a + b + c;
}

const prices = [100, 200, 300];
```

می‌توانیم بنویسیم:

```javascript
calculateTotal(...prices);
```

از نظر مفهومی:

```javascript
calculateTotal(100, 200, 300);
```

در اینجا Spread عناصر Array را به Arguments تبدیل می‌کند.

نکته مهم این است که Spread در Function Call همچنان همان ایده اصلی را دارد:

```text
Collection
↓
Expansion
↓
Individual values
```

---

# Spread با Object

Spread در Object نیز کاربرد بسیار مهمی دارد.

فرض کنید:

```javascript
const user = {
  name: 'Omid',
  role: 'Developer'
};
```

می‌توانیم یک Object جدید بسازیم:

```javascript
const copy = {
  ...user
};
```

نتیجه:

```javascript
{
  name: 'Omid',
  role: 'Developer'
}
```

در اینجا Properties Object در Object جدید گسترش داده شده‌اند.

---

## Object Spread برای Merging

می‌توانیم چند Object را نیز ترکیب کنیم:

```javascript
const user = {
  name: 'Omid'
};

const preferences = {
  theme: 'dark'
};

const profile = {
  ...user,
  ...preferences
};
```

نتیجه:

```javascript
{
  name: 'Omid',
  theme: 'dark'
}
```

این الگو برای ترکیب داده‌های Application بسیار رایج است.

---

# Property Overriding

اگر دو Object Property همنام داشته باشند، ترتیب Spread اهمیت دارد.

```javascript
const defaults = {
  theme: 'light',
  language: 'en'
};

const userSettings = {
  theme: 'dark'
};

const settings = {
  ...defaults,
  ...userSettings
};
```

نتیجه:

```javascript
{
  theme: 'dark',
  language: 'en'
}
```

چرا؟

زیرا Propertyهای بعدی می‌توانند Propertyهای قبلی با همان Key را Override کنند.

بنابراین:

```javascript
{
  ...defaults,
  ...userSettings
}
```

با:

```javascript
{
  ...userSettings,
  ...defaults
}
```

یکسان نیست.

در حالت دوم:

```javascript
theme
```

از `defaults` خواهد آمد.

این موضوع در Merging Objectها اهمیت زیادی دارد.

---

# Updating Objects با Spread

یکی از کاربردهای بسیار مهم Object Spread، ایجاد Object جدید با تغییر یک Property است.

فرض کنید:

```javascript
const user = {
  name: 'Omid',
  role: 'Developer',
  city: 'Baku'
};
```

می‌خواهیم فقط `role` را تغییر دهیم.

می‌توانیم بنویسیم:

```javascript
const updatedUser = {
  ...user,
  role: 'Senior Developer'
};
```

Object اصلی تغییر نمی‌کند.

```javascript
console.log(user.role);
```

نتیجه:

```text
Developer
```

و:

```javascript
console.log(updatedUser.role);
```

نتیجه:

```text
Senior Developer
```

مدل ذهنی:

```text
Original Object
      ↓
   Spread
      ↓
New Object
      ↓
Override Property
```

این الگو یکی از پایه‌های مهم Immutable Updates است.

---

# Rest Syntax

تا اینجا Spread را بررسی کردیم.

Spread یعنی:

> داده را باز کن و گسترش بده.

اما گاهی برعکس آن را نیاز داریم.

فرض کنید:

```javascript
const numbers = [10, 20, 30, 40];
```

می‌خواهیم مقدار اول را جدا کنیم و همه عناصر باقی‌مانده را در یک Array دیگر قرار دهیم.

در این حالت می‌توانیم از Rest استفاده کنیم:

```javascript
const [first, ...rest] = numbers;
```

اکنون:

```javascript
first
```

برابر است با:

```text
10
```

و:

```javascript
rest
```

برابر است با:

```javascript
[20, 30, 40]
```

اینجا `...rest` به معنای:

> همه عناصر باقی‌مانده را جمع کن.

---

# Rest چیست؟

### تعریف ساده

**Rest Syntax** مقادیر یا Properties باقی‌مانده را جمع می‌کند و آن‌ها را در یک Collection قرار می‌دهد.

پس تفاوت اصلی:

```text
Spread → Expand
Rest   → Collect Remaining
```

است.

---

## Rest و Destructuring

Rest در Array Destructuring بسیار طبیعی است.

```javascript
const [first, second, ...others] = [
  'HTML',
  'CSS',
  'JavaScript',
  'React'
];
```

اکنون:

```javascript
first
```

برابر است با:

```text
HTML
```

و:

```javascript
second
```

برابر است با:

```text
CSS
```

و:

```javascript
others
```

برابر است با:

```javascript
['JavaScript', 'React']
```

Rest همه عناصر باقی‌مانده را جمع کرده است.

---

# Rest Parameters

Rest فقط در Destructuring استفاده نمی‌شود.

یکی از کاربردهای مهم آن در Function Parameters است.

فرض کنید Functionای داریم که تعداد Arguments آن مشخص نیست.

```javascript
function calculateTotal(a, b, c) {
  return a + b + c;
}
```

این Function دقیقاً برای سه Argument طراحی شده است.

اما شاید بخواهیم هر تعداد Price دریافت کنیم:

```javascript
calculateTotal(100, 200);
calculateTotal(100, 200, 300);
calculateTotal(100, 200, 300, 400);
```

در این شرایط Rest Parameters مناسب است:

```javascript
function calculateTotal(...prices) {
  return prices;
}
```

اکنون:

```javascript
calculateTotal(100, 200, 300);
```

داخل Function:

```javascript
prices
```

یک Array خواهد بود:

```javascript
[100, 200, 300]
```

---

# Remaining Arguments

Rest Parameters تمام Arguments باقی‌مانده را جمع می‌کند.

مثلاً:

```javascript
function logUser(name, ...roles) {
  console.log(name);
  console.log(roles);
}

logUser(
  'Omid',
  'Developer',
  'Admin',
  'Reviewer'
);
```

خروجی:

```text
Omid
['Developer', 'Admin', 'Reviewer']
```

در اینجا:

```javascript
name
```

اولین Argument را دریافت می‌کند.

و:

```javascript
...roles
```

تمام Arguments باقی‌مانده را در یک Array قرار می‌دهد.

مدل ذهنی:

```text
Arguments
↓
name
↓
remaining arguments
↓
roles[]
```

---

# Rest Parameters و API Design

Rest Parameters برای طراحی Functionهایی که تعداد ورودی آن‌ها متغیر است مفید هستند.

مثلاً:

```javascript
function createReport(title, ...sections) {
  return {
    title,
    sections
  };
}
```

اکنون:

```javascript
const report = createReport(
  'Monthly Report',
  'Sales',
  'Users',
  'Revenue'
);
```

نتیجه:

```javascript
{
  title: 'Monthly Report',
  sections: ['Sales', 'Users', 'Revenue']
}
```

Function یک API انعطاف‌پذیر دارد.

---

# Rest در Object Destructuring

Rest در Object نیز می‌تواند Properties باقی‌مانده را جمع کند.

فرض کنید:

```javascript
const user = {
  name: 'Omid',
  role: 'Developer',
  city: 'Baku',
  language: 'English'
};
```

می‌خواهیم `name` را جدا کنیم و بقیه Properties را نگه داریم:

```javascript
const { name, ...details } = user;
```

اکنون:

```javascript
name
```

برابر است با:

```text
Omid
```

و:

```javascript
details
```

برابر است با:

```javascript
{
  role: 'Developer',
  city: 'Baku',
  language: 'English'
}
```

اینجا Object Rest تمام Properties باقی‌مانده را جمع کرده است.

---

# Object Rest و Data Extraction

این الگو در کار با داده‌های واقعی بسیار مفید است.

فرض کنید API اطلاعات زیادی درباره یک User برمی‌گرداند:

```javascript
const user = {
  id: 101,
  name: 'Omid',
  email: 'omid@example.com',
  role: 'Developer',
  createdAt: '2026-08-27'
};
```

فرض کنید برای یک بخش از Application فقط `id` و `name` را می‌خواهیم و بقیه داده‌ها را در یک Object جداگانه نگه می‌داریم:

```javascript
const {
  id,
  name,
  ...metadata
} = user;
```

اکنون:

```javascript
metadata
```

شامل Properties باقی‌مانده است:

```javascript
{
  email: 'omid@example.com',
  role: 'Developer',
  createdAt: '2026-08-27'
}
```

این الگو با Destructuring فصل قبل ارتباط مستقیم دارد:

```text
Destructuring
↓
Extract specific data
↓
Rest
↓
Collect remaining data
```

---

# Spread و Rest در کنار هم

اکنون تفاوت دو مفهوم را در یک مثال ببینیم.

```javascript
const user = {
  name: 'Omid',
  role: 'Developer',
  city: 'Baku'
};

const { name, ...details } = user;

const updatedUser = {
  ...details,
  name: 'Sara'
};
```

در خط اول:

```javascript
...details
```

نقش **Rest** دارد.

زیرا Properties باقی‌مانده را جمع می‌کند.

در خط دوم:

```javascript
...details
```

نقش **Spread** دارد.

زیرا Properties را در Object جدید گسترش می‌دهد.

بنابراین شکل Syntax به‌تنهایی کافی نیست.

این Context است که نقش آن را مشخص می‌کند.

---

# یک قاعده ذهنی بسیار مهم

هر بار `...` را دیدید، این سؤال را بپرسید:

> داده در حال ورود به یک Collection است یا از یک Collection خارج می‌شود؟

اگر داده در حال **گسترش یافتن** باشد:

```text
Spread
```

اگر داده‌های **باقی‌مانده در حال جمع شدن** باشند:

```text
Rest
```

مثال:

```javascript
const copy = [...products];
```

Spread است.

اما:

```javascript
const [first, ...others] = products;
```

Rest است.

---

# Copying و Shallow Copy

یکی از مهم‌ترین کاربردهای Spread، ایجاد Copy است.

برای Array:

```javascript
const productsCopy = [...products];
```

برای Object:

```javascript
const userCopy = { ...user };
```

اما باید توجه کنیم که این Copy یک **Shallow Copy** است.

یعنی فقط سطح اول Collection کپی می‌شود.

---

# Shallow Copy چیست؟

فرض کنید:

```javascript
const user = {
  name: 'Omid',
  address: {
    city: 'Baku'
  }
};

const copy = {
  ...user
};
```

در سطح اول:

```text
user
  ↓
Object A

copy
  ↓
Object B
```

اما Property زیر:

```javascript
address
```

خودش یک Object است.

Spread در اینجا یک Object جدید برای `address` ایجاد نکرده است.

در نتیجه:

```text
user.address ──┐
               ├──→ same nested Object
copy.address ──┘
```

بنابراین:

```javascript
copy.address.city = 'Tehran';

console.log(user.address.city);
```

نتیجه:

```text
Tehran
```

است.

زیرا Object داخلی هنوز Shared است.

---

# مدل ذهنی Shallow Copy

```text
Original Object
│
├── name ────────────→ copied value
│
└── address ─────────→ same nested reference
                         ↑
New Object              │
│                       │
├── name ──────────────→ copied value
│
└── address ────────────┘
```

بنابراین Spread به‌تنهایی یک Deep Copy ایجاد نمی‌کند.

در این فصل فقط باید این محدودیت را بشناسیم.

روش‌های Deep Copy و جزئیات مرتبط با آن در مباحث پیشرفته‌تر بررسی خواهند شد.

---

# Merging

Spread ابزار مناسبی برای Merging است.

مثلاً:

```javascript
const baseConfig = {
  language: 'en',
  theme: 'light'
};

const userConfig = {
  theme: 'dark'
};

const config = {
  ...baseConfig,
  ...userConfig
};
```

نتیجه:

```javascript
{
  language: 'en',
  theme: 'dark'
}
```

این الگو در Configuration Objects بسیار کاربردی است.

---

# Updating

Spread همچنین برای ایجاد نسخه جدیدی از یک Object با تغییر محدود استفاده می‌شود.

```javascript
const product = {
  id: 101,
  name: 'Laptop',
  price: 1200
};

const updatedProduct = {
  ...product,
  price: 1100
};
```

Object اصلی دست‌نخورده باقی می‌ماند.

این الگو پایه یکی از روش‌های مهم **Immutable Update** است.

---

# Immutable Patterns

در Applicationهای مدرن، گاهی نمی‌خواهیم Collection موجود را مستقیماً Mutation کنیم.

فرض کنید:

```javascript
const user = {
  name: 'Omid',
  role: 'Developer'
};
```

به‌جای:

```javascript
user.role = 'Admin';
```

می‌توانیم Object جدید بسازیم:

```javascript
const updatedUser = {
  ...user,
  role: 'Admin'
};
```

در اینجا:

```text
Original
   ↓
Spread
   ↓
New Object
   ↓
Updated Property
```

ساختار قبلی حفظ می‌شود و Object جدید ایجاد می‌شود.

همین ایده برای Array نیز کاربرد دارد:

```javascript
const products = ['Laptop', 'Mouse'];

const updatedProducts = [
  ...products,
  'Keyboard'
];
```

Array جدید ساخته می‌شود.

---

# چرا Immutable Patterns مهم هستند؟

هدف این فصل آموزش یک Framework یا State Management Library نیست.

اما از نظر مهندسی باید بدانیم که ساختن نسخه جدید از داده به‌جای Mutation مستقیم، می‌تواند مدیریت State را قابل پیش‌بینی‌تر کند.

برای مثال:

```javascript
const updatedUser = {
  ...user,
  role: 'Admin'
};
```

به‌صورت واضح نشان می‌دهد که:

* `user` داده قبلی است.
* `updatedUser` نسخه جدید است.
* Property `role` مقدار جدید دارد.

این الگو در بسیاری از Applicationهای Frontend مدرن استفاده می‌شود.

---

# Rest و Spread در یک نگاه

اکنون می‌توانیم دو مفهوم را کنار هم قرار دهیم:

| مفهوم           | نقش                            |
| --------------- | ------------------------------ |
| Spread          | گسترش داده‌های یک Collection   |
| Rest            | جمع کردن داده‌های باقی‌مانده   |
| Array Spread    | گسترش Elements                 |
| Object Spread   | گسترش Properties               |
| Rest Parameters | جمع کردن Arguments             |
| Array Rest      | جمع کردن Elements باقی‌مانده   |
| Object Rest     | جمع کردن Properties باقی‌مانده |
| Spread Copy     | ایجاد Shallow Copy             |
| Spread Merge    | ترکیب Collections              |
| Spread Update   | ایجاد نسخه جدید با تغییرات     |

مدل ذهنی اصلی:

```text
             ...
            /   \
       Spread    Rest
          ↓        ↓
       Expand    Collect
          ↓        ↓
      Collection  Remaining
```

---

# Common Mistakes

## اشتباه اول: Rest و Spread را بر اساس `...` تشخیص دادن

صرفاً دیدن:

```javascript
...
```

کافی نیست.

باید Context را بررسی کنیم.

```javascript
const copy = [...products];
```

Spread است.

اما:

```javascript
const [first, ...others] = products;
```

Rest است.

---

## اشتباه دوم: تصور اینکه Spread Object را Deep Copy می‌کند

❌

```javascript
const copy = { ...user };
```

به معنای Deep Copy نیست.

Spread یک Shallow Copy ایجاد می‌کند.

اگر Objectهای تو‌در‌تو داشته باشیم، References داخلی می‌توانند مشترک باشند.

---

## اشتباه سوم: انتظار Mutation

این کد:

```javascript
const copy = [...products];
```

Array اصلی را تغییر نمی‌دهد.

Spread یک Collection جدید می‌سازد.

---

## اشتباه چهارم: استفاده از Rest در هر جای Function Parameters

Rest Parameter باید در انتهای Parameter List قرار گیرد.

صحیح:

```javascript
function log(name, ...roles) {
  // ...
}
```

اما این الگو صحیح نیست:

```javascript
function log(...roles, name) {
  // ...
}
```

زیرا Rest Parameter باید آخرین Parameter باشد.

---

## اشتباه پنجم: اشتباه گرفتن Rest با Arguments Object

در Functionهای معمولی می‌توان به `arguments` دسترسی داشت، اما Rest Parameters یک Array واقعی ایجاد می‌کنند.

```javascript
function log(...values) {
  console.log(Array.isArray(values));
}
```

نتیجه:

```text
true
```

بنابراین Rest Parameters برای طراحی Functionهای مدرن معمولاً واضح‌تر و مناسب‌تر است.

---

## اشتباه ششم: تصور اینکه Rest همیشه یک Object یا Array جدید را از همه داده‌ها می‌سازد

Rest فقط داده‌های **باقی‌مانده در آن Pattern** را جمع می‌کند.

مثلاً:

```javascript
const [first, second, ...others] = [1, 2, 3, 4];
```

مقدار `others` فقط شامل:

```javascript
[3, 4]
```

است.

---

## اشتباه هفتم: نادیده گرفتن ترتیب در Object Spread

این دو یکسان نیستند:

```javascript
{
  ...defaults,
  ...userSettings
}
```

و:

```javascript
{
  ...userSettings,
  ...defaults
}
```

Propertyهای بعدی می‌توانند Propertyهای قبلی با همان Key را Override کنند.

---

## اشتباه هشتم: تصور اینکه Spread و Destructuring یک مفهوم هستند

Destructuring برای **استخراج ساختاری داده** استفاده می‌شود.

Spread برای **گسترش داده** و Rest برای **جمع کردن داده‌های باقی‌مانده** استفاده می‌شود.

البته Rest می‌تواند در Destructuring استفاده شود:

```javascript
const { name, ...details } = user;
```

اما این به معنی یکسان بودن دو مفهوم نیست.

---

# Best Practices

## از Spread برای ایجاد Collection جدید استفاده کنید

وقتی قصد دارید Array جدیدی بر اساس Array موجود بسازید:

```javascript
const updated = [...items, newItem];
```

این الگو واضح و قابل خواندن است.

---

## در Object Update ترتیب را آگاهانه انتخاب کنید

اگر Property جدید باید مقدار قبلی را Override کند:

```javascript
const updated = {
  ...user,
  role: 'Admin'
};
```

Property جدید را بعد از Spread قرار دهید.

---

## Rest را برای Functionهای دارای تعداد متغیر Arguments استفاده کنید

به‌جای طراحی چندین Parameter ثابت برای تعداد نامشخص ورودی:

```javascript
function sum(...numbers) {
  // ...
}
```

مدل Function واضح‌تر می‌شود.

---

## Shallow Copy را با Deep Copy اشتباه نگیرید

اگر داده‌های Nested دارید، Spread فقط سطح اول را Copy می‌کند.

بنابراین قبل از استفاده از Spread برای Copy، ساختار داده را بررسی کنید.

---

## از Rest برای استخراج و نگه‌داشتن Remaining Data استفاده کنید

برای مثال:

```javascript
const { id, ...payload } = user;
```

این الگو زمانی مناسب است که یک بخش از داده باید جدا شود و بقیه داده‌ها همچنان به‌صورت یک Object مورد استفاده قرار گیرند.

---

# Summary

در این فصل دیدیم که Syntax زیر:

```javascript
...
```

دو نقش کاملاً متفاوت دارد.

در **Spread**، داده‌های یک Collection گسترش پیدا می‌کنند.

```javascript
const copy = [...products];
```

در **Rest**، داده‌های باقی‌مانده جمع می‌شوند.

```javascript
const [first, ...others] = products;
```

Spread را ابتدا با Array بررسی کردیم.

از آن برای:

* Copying
* Combining
* Expansion

استفاده کردیم.

سپس Object Spread را بررسی کردیم.

با آن توانستیم:

* Object ایجاد کنیم.
* Objectها را Merge کنیم.
* Propertyها را Override کنیم.
* نسخه جدیدی از Object بسازیم.
* Object را با حفظ Object اصلی Update کنیم.

سپس Rest را بررسی کردیم.

در Array Destructuring، Rest عناصر باقی‌مانده را جمع می‌کند.

در Object Destructuring، Rest Properties باقی‌مانده را جمع می‌کند.

در Function Parameters، Rest Arguments را در یک Array قرار می‌دهد.

در نهایت دیدیم که Spread می‌تواند برای Immutable Patterns استفاده شود؛ اما Copy ایجادشده توسط Spread **Shallow** است و برای داده‌های Nested الزاماً یک Copy مستقل از تمام سطوح ایجاد نمی‌کند.

مدل ذهنی نهایی این فصل بسیار ساده است:

```text
Spread → Expand
Rest   → Collect Remaining
```

اما تشخیص درست آن‌ها همیشه به **Context** بستگی دارد.

---

# Key Takeaways

* `...` در JavaScript می‌تواند نقش Spread یا Rest داشته باشد.
* Spread برای گسترش داده‌های یک Collection استفاده می‌شود.
* Rest برای جمع کردن داده‌های باقی‌مانده استفاده می‌شود.
* Spread و Rest دو مفهوم متفاوت هستند.
* Spread در Arrayها می‌تواند Elements را گسترش دهد.
* Array Spread برای Copy کردن و ترکیب Arrayها بسیار رایج است.
* Spread در Function Call می‌تواند یک Collection را به Arguments گسترش دهد.
* Object Spread Properties را در Object جدید گسترش می‌دهد.
* ترتیب Object Spread هنگام Merge اهمیت دارد.
* Propertyهای بعدی می‌توانند Propertyهای قبلی با همان Key را Override کنند.
* Object Spread برای Immutable Update بسیار رایج است.
* Array و Object Copy با Spread معمولاً Shallow هستند.
* Nested Objects و Arrays می‌توانند همچنان Reference مشترک داشته باشند.
* Array Rest عناصر باقی‌مانده را در یک Array جمع می‌کند.
* Object Rest Properties باقی‌مانده را در یک Object جمع می‌کند.
* Rest Parameters Arguments را در یک Array جمع می‌کنند.
* Rest Parameter باید آخرین Parameter باشد.
* Rest Parameters یک Array واقعی هستند.
* Rest Parameters با `arguments` یکسان نیستند.
* Rest و Destructuring مفاهیم یکسانی نیستند.
* Spread برای Expansion و Rest برای Collection of Remaining Values است.
* Context مشخص می‌کند `...` چه نقشی دارد.
* Immutable Update یعنی به‌جای Mutation مستقیم، نسخه جدیدی از داده ایجاد کنیم.
* Spread یکی از ابزارهای مهم برای پیاده‌سازی این الگو است.
* Spread به‌تنهایی Deep Copy ایجاد نمی‌کند.

---

# Technical Interview

## Junior

### 1. Rest و Spread چیستند؟

**Golden Answer:**

Rest و Spread هر دو از Syntax `...` استفاده می‌کنند، اما نقش متفاوتی دارند. Spread یک Collection را گسترش می‌دهد، در حالی که Rest مقادیر یا Properties باقی‌مانده را جمع می‌کند.

---

### 2. تفاوت اصلی Rest و Spread چیست؟

**Golden Answer:**

Spread داده را از یک Collection خارج و در Context مقصد گسترش می‌دهد؛ Rest داده‌های باقی‌مانده را داخل یک Collection جمع می‌کند.

```text
Spread → Expand
Rest   → Collect
```

---

### 3. Spread در Array چه کاربردی دارد؟

**Golden Answer:**

برای ایجاد Array جدید، Copy کردن Array، ترکیب چند Array و گسترش Elements استفاده می‌شود.

```javascript
const copy = [...products];

const all = [...frontend, ...backend];
```

---

### 4. آیا Spread یک Array جدید ایجاد می‌کند؟

**Golden Answer:**

وقتی در یک Array literal مانند `[...items]` استفاده شود، یک Array جدید ایجاد می‌کند؛ اما این Copy در سطح Shallow است و References مربوط به داده‌های Nested می‌توانند مشترک باقی بمانند.

---

### 5. Rest Parameter چیست؟

**Golden Answer:**

Rest Parameter به Function اجازه می‌دهد تعداد متغیری از Arguments را در یک Array جمع کند.

```javascript
function sum(...numbers) {
  // numbers is an Array
}
```

---

### 6. آیا Rest Parameter یک Array واقعی است؟

**Golden Answer:**

بله. برخلاف `arguments`، Rest Parameter یک Array واقعی است و تمام قابلیت‌های Array را دارد.

---

### 7. Object Spread چه کاری انجام می‌دهد؟

**Golden Answer:**

Properties یک Object را در Object دیگری گسترش می‌دهد و می‌تواند برای Copy، Merge و ایجاد نسخه جدید از Object استفاده شود.

```javascript
const updatedUser = {
  ...user,
  role: 'Admin'
};
```

---

### 8. Object Rest چه کاری انجام می‌دهد؟

**Golden Answer:**

Properties باقی‌مانده را در یک Object جدید جمع می‌کند.

```javascript
const { id, ...details } = user;
```

در اینجا `details` شامل همه Properties به‌جز `id` است.

---

## Mid-Level

### 9. چرا این دو کد رفتار متفاوتی دارند؟

```javascript
const result = [products];
```

و:

```javascript
const result = [...products];
```

**Golden Answer:**

در حالت اول، خود Array به‌عنوان یک Element داخل Array جدید قرار می‌گیرد و ساختار Nested ایجاد می‌شود. در حالت دوم، Spread Elements موجود در Array را گسترش می‌دهد.

---

### 10. آیا Spread یک Deep Copy ایجاد می‌کند؟

**Golden Answer:**

خیر. Spread یک Shallow Copy ایجاد می‌کند. Properties یا Elements سطح اول در Collection جدید قرار می‌گیرند، اما Nested Objects و Arrays می‌توانند همان Reference قبلی را حفظ کنند.

---

### 11. چرا ترتیب Spread در این کد مهم است؟

```javascript
const settings = {
  ...defaults,
  ...userSettings
};
```

**Golden Answer:**

چون اگر یک Key در هر دو Object وجود داشته باشد، مقدار Property بعدی مقدار قبلی را Override می‌کند. بنابراین در این مثال `userSettings` بر `defaults` اولویت دارد.

---

### 12. چگونه با Spread یک Object را بدون Mutation مستقیم Update می‌کنیم؟

**Golden Answer:**

Object قبلی را Spread می‌کنیم و Property موردنظر را بعد از آن قرار می‌دهیم:

```javascript
const updatedUser = {
  ...user,
  role: 'Admin'
};
```

در نتیجه Object جدید ساخته می‌شود و Object اصلی تغییر نمی‌کند.

---

### 13. Rest در Destructuring چه کاربردی دارد؟

**Golden Answer:**

Rest اجازه می‌دهد بخشی از داده را استخراج کنیم و تمام عناصر یا Properties باقی‌مانده را در یک Collection جدید نگه داریم.

```javascript
const { name, ...details } = user;
```

---

### 14. تفاوت Rest Parameter و `arguments` چیست؟

**Golden Answer:**

هر دو می‌توانند Arguments را در اختیار Function قرار دهند، اما Rest Parameter یک Array واقعی است و Syntax مدرن و صریح‌تری دارد. `arguments` یک Array نیست و فقط در Functionهای معمولی رفتار مخصوص خود را دارد.

---

### 15. آیا Rest Parameter می‌تواند قبل از Parameter دیگری قرار بگیرد؟

**Golden Answer:**

خیر. Rest Parameter باید آخرین Parameter باشد.

صحیح:

```javascript
function log(name, ...roles) {}
```

اما:

```javascript
function log(...roles, name) {}
```

مجاز نیست.

---

### 16. آیا Spread و Rest از نظر عملکرد زبان یک عملیات یکسان هستند؟

**Golden Answer:**

خیر. هر دو از `...` استفاده می‌کنند، اما Grammar و Context آن‌ها متفاوت است. Spread برای Expansion و Rest برای Binding یا Collection of Remaining Values استفاده می‌شود.

---

### 17. این کد چه کاری انجام می‌دهد؟

```javascript
const [first, ...others] = products;
```

**Golden Answer:**

اولین Element در `first` قرار می‌گیرد و تمام Elements باقی‌مانده در Array جدیدی به نام `others` جمع می‌شوند.

---

### 18. چگونه می‌توان چند Object را Merge کرد؟

**Golden Answer:**

با Object Spread:

```javascript
const result = {
  ...first,
  ...second
};
```

اگر Key مشترک وجود داشته باشد، مقدار Objectای که دیرتر Spread شده است بر مقدار قبلی غلبه می‌کند.

---

## Senior

### 19. مدل ذهنی صحیح برای تشخیص Rest و Spread چیست؟

**Golden Answer:**

به‌جای حفظ Syntax باید Direction داده را بررسی کرد.

اگر داده از یک Collection گرفته شده و در Context مقصد باز می‌شود، Spread است:

```text
Collection → Expand
```

اگر چند داده در یک Binding یا Collection جمع می‌شوند، Rest است:

```text
Remaining Values → Collect
```

بنابراین `...` به‌تنهایی مشخص‌کننده نقش نیست؛ Context تعیین‌کننده است.

---

### 20. چرا می‌گوییم Spread یک Shallow Copy ایجاد می‌کند؟

**Golden Answer:**

زیرا Spread در سطح Collection جدید، Elements یا Properties را کپی می‌کند، اما اگر یکی از آن‌ها یک Object یا Array باشد، Reference آن Nested Value می‌تواند در Object یا Array جدید نیز استفاده شود.

```javascript
const original = {
  address: {
    city: 'Baku'
  }
};

const copy = {
  ...original
};
```

در اینجا `copy.address` و `original.address` به همان Nested Object اشاره می‌کنند.

---

### 21. چرا Object Spread برای Immutable Update مهم است؟

**Golden Answer:**

چون به‌جای Mutation مستقیم Object موجود، می‌توان یک Object جدید ایجاد کرد:

```javascript
const updated = {
  ...state,
  status: 'success'
};
```

در نتیجه Identity Object جدید متفاوت است و داده قبلی بدون تغییر باقی می‌ماند. این الگو برای مدیریت قابل پیش‌بینی State در Applicationهای Frontend اهمیت دارد.

---

### 22. آیا Spread تضمین می‌کند Object اصلی هرگز تغییر نکند؟

**Golden Answer:**

خود عملیات Spread Object اصلی را Mutation نمی‌کند، اما این به معنای مستقل بودن کامل داده‌های Nested نیست. اگر Object جدید به Nested Reference مشترک دسترسی داشته باشد و آن Object داخلی Mutation شود، داده مشترک می‌تواند در Object اصلی نیز تغییر کند.

---

### 23. در این مثال چه اتفاقی می‌افتد؟

```javascript
const user = {
  name: 'Omid',
  address: {
    city: 'Baku'
  }
};

const updated = {
  ...user,
  address: {
    ...user.address,
    city: 'Tehran'
  }
};
```

**Golden Answer:**

در سطح اول یک Object جدید ساخته می‌شود و سپس برای `address` نیز یک Object جدید ایجاد می‌شود. بنابراین هم Object بیرونی و هم Nested Object مربوط به `address` مستقل هستند و Mutation روی `updated.address` باعث تغییر `user.address` نمی‌شود.

---

### 24. چرا Rest Parameters از نظر طراحی API مفید هستند؟

**Golden Answer:**

زیرا Function را برای دریافت تعداد متغیری از Arguments آماده می‌کنند، بدون اینکه لازم باشد تعداد دقیق Arguments از قبل مشخص باشد.

```javascript
function createReport(title, ...sections) {
  // ...
}
```

این Syntax Contract مربوط به Function را واضح‌تر می‌کند: یک Argument مشخص برای `title` و هر تعداد Section برای `sections`.

---

### 25. آیا Rest و Spread فقط Syntaxهای کوتاه‌تر هستند؟

**Golden Answer:**

خیر. آن‌ها فقط Sugar برای کوتاه کردن کد نیستند. Rest و Spread ابزارهایی برای مدل‌سازی جریان داده هستند: Spread برای Expansion و ساخت Collectionهای جدید و Rest برای Collection کردن Remaining Values. این قابلیت‌ها مستقیماً روی طراحی API، Data Transformation و Immutable Update اثر می‌گذارند.

---

### 26. چگونه می‌توان Rest و Spread را با Destructuring در یک الگوی واقعی ترکیب کرد؟

**Golden Answer:**

می‌توان ابتدا داده‌های موردنیاز را با Destructuring استخراج و باقی داده‌ها را با Rest جمع کرد، سپس با Spread Object جدید ساخت.

```javascript
const user = {
  id: 101,
  name: 'Omid',
  role: 'Developer',
  city: 'Baku'
};

const { id, ...details } = user;

const updated = {
  ...details,
  role: 'Admin'
};
```

در اینجا Destructuring ساختار داده را باز می‌کند، Rest داده‌های باقی‌مانده را جمع می‌کند و Spread برای ساخت Object جدید استفاده می‌شود.

---

### 27. اگر یک Interviewer بپرسد «آیا `...` یعنی تبدیل Collection به Collection دیگر؟» چه پاسخی می‌دهید؟

**Golden Answer:**

این تعریف بیش از حد کلی است. `...` یک Syntax واحد با یک رفتار ثابت نیست. در Spread، عناصر یا Properties در Context مقصد گسترش می‌یابند؛ در Rest، Values یا Properties باقی‌مانده در یک Binding یا Collection جمع می‌شوند. بنابراین باید Context نحوی را بررسی کرد.

---

# Golden Answers — خلاصه مصاحبه‌ای

### Junior

> **Rest** برای جمع کردن مقادیر باقی‌مانده و **Spread** برای گسترش مقادیر یک Collection استفاده می‌شود. هر دو از `...` استفاده می‌کنند، اما Context تعیین می‌کند کدام نقش را دارند.

### Mid-Level

> Spread در Array و Object برای Copying، Merging و ساخت نسخه جدید بسیار کاربردی است. Rest در Destructuring و Function Parameters برای جمع کردن Remaining Values استفاده می‌شود. Spread Copy یک Shallow Copy است، بنابراین Nested References همچنان می‌توانند Shared باشند.

### Senior

> Rest و Spread را نباید صرفاً به‌عنوان Syntax کوتاه‌تر دید. آن‌ها ابزارهای زبانی برای کنترل جریان داده هستند. Spread Collection را در Context مقصد Expand می‌کند، در حالی که Rest Remaining Values را در یک Binding جمع می‌کند. این تفاوت در طراحی Function API، Object Transformation، Merging و Immutable Update اهمیت مهندسی دارد.

---

# Conclusion

Rest و Spread دو کاربرد متفاوت از یک Syntax هستند:

```javascript
...
```

Spread زمانی استفاده می‌شود که بخواهیم داده‌های یک Collection را **گسترش دهیم**:

```javascript
const updated = {
  ...user,
  role: 'Admin'
};
```

Rest زمانی استفاده می‌شود که بخواهیم داده‌های **باقی‌مانده را جمع کنیم**:

```javascript
const { name, ...details } = user;
```

این تفاوت را می‌توان در یک مدل ذهنی خلاصه کرد:

```text
Spread
Collection
    ↓
Expand
    ↓
Context

Rest
Values / Properties
    ↓
Remaining
    ↓
Collect
```

در Applicationهای واقعی، این دو Syntax فقط برای کوتاه‌تر نوشتن کد نیستند.

Spread به ما کمک می‌کند Collectionهای جدید بسازیم، داده‌ها را Merge کنیم و بدون Mutation مستقیم نسخه جدیدی از داده ایجاد کنیم.

Rest به ما اجازه می‌دهد ورودی‌های متغیر Function را مدیریت کنیم یا داده‌های باقی‌مانده را از یک Collection جدا کنیم.

در نهایت، مهم‌ترین نکته این فصل حفظ کردن `...` نیست.

هنگام مشاهده آن باید بپرسیم:

> **آیا داده در حال Expand شدن است یا Remaining Data در حال Collect شدن؟**

اگر پاسخ این سؤال را بدانیم، تشخیص Rest و Spread دیگر وابسته به حفظ کردن Syntax نخواهد بود.
