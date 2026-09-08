# Chapter 32 — Arrays Fundamentals

## Chapter Goal

در پایان این فصل، خواننده باید بتواند توضیح دهد که چرا Array به‌عنوان یک Collection در JavaScript وجود دارد، چگونه عناصر آن با Index سازمان‌دهی و دسترسی می‌شوند، `length` چه مفهومی دارد، Mutation چگونه روی Array اثر می‌گذارد، چرا Array یک Reference Value است و چگونه می‌توان عناصر آن را با Iteration پردازش کرد.

هدف این فصل حفظ کردن Syntaxهای Array نیست.

هدف، ساختن یک مدل ذهنی صحیح از Array است؛ مدلی که در فصل‌های بعدی بتوان بر اساس آن رفتار Methodهای Array و الگوهای پیشرفته‌تر را به‌درستی درک کرد.

---

## Core Question

> چگونه چند Value را در یک Collection مرتب و قابل دسترسی نگهداری کنیم؟

---

## Introduction

تا اینجا بیشتر با Valueهایی کار کرده‌ایم که هر کدام یک مقدار مستقل را نمایش می‌دادند.

برای مثال، یک Product ممکن است یک `name`، یک `price` و یک `category` داشته باشد.

اما در یک Application واقعی، معمولاً با یک Value منفرد سروکار نداریم.

یک فروشگاه مجموعه‌ای از Productها دارد.

یک Cart مجموعه‌ای از Itemها دارد.

یک Recipe مجموعه‌ای از Ingredientها دارد.

یک API ممکن است مجموعه‌ای از Recipeها را برگرداند.

در چنین شرایطی، نگهداری هر Value در یک Variable جداگانه خیلی زود به یک مشکل تبدیل می‌شود.

```js
const recipe1 = "Pasta";
const recipe2 = "Pizza";
const recipe3 = "Salad";
const recipe4 = "Soup";
```

این روش نه مقیاس‌پذیر است و نه ارتباط میان این Valueها را به‌خوبی نشان می‌دهد.

مسئله واقعی این است:

> چگونه چند Value مرتبط را به‌عنوان یک Collection واحد نگهداری کنیم؟

برای پاسخ به این سؤال ابتدا باید مفهوم Collection را درک کنیم.

---

## From Individual Values to Collections

**Collection** ساختاری است که برای نگهداری مجموعه‌ای از Valueها استفاده می‌شود.

به جای اینکه هر Value را به‌صورت مستقل مدیریت کنیم، Collection آن‌ها را در یک ساختار واحد قرار می‌دهد.

این تغییر، فقط یک تغییر Syntaxی نیست.

وقتی چند Value در یک Collection قرار می‌گیرند، می‌توانیم درباره آن‌ها به‌عنوان یک مجموعه واحد فکر کنیم.

برای مثال:

```js
const recipes = ["Pasta", "Pizza", "Salad", "Soup"];
```

اکنون چهار Value در یک Collection قرار دارند.

به جای چهار Variable، یک Variable داریم که یک مجموعه از Recipeها را نگهداری می‌کند.

این همان مسئله‌ای است که Array برای آن طراحی شده است.

---

## Array چیست؟

**Array** یکی از مهم‌ترین Data Structureهای داخلی JavaScript برای نگهداری مجموعه‌ای مرتب از Valueها است.

برای ایجاد یک Array معمولاً از **Array Literal** استفاده می‌کنیم:

```js
const recipes = ["Pasta", "Pizza", "Salad"];
```

در اینجا:

```text
recipes
   ↓
Array
 ┌─────────┬─────────┬─────────┐
 │ "Pasta" │ "Pizza" │ "Salad" │
 └─────────┴─────────┴─────────┘
```

Array چند ویژگی مهم دارد.

اول اینکه عناصر آن دارای ترتیب هستند.

دوم اینکه هر عنصر از طریق یک موقعیت مشخص قابل دسترسی است.

سوم اینکه تعداد عناصر آن می‌تواند تغییر کند.

چهارم اینکه یک Array می‌تواند Valueهای مختلف را نگهداری کند.

برای مثال:

```js
const product = [
  "Laptop",
  1200,
  true,
  { category: "electronics" }
];
```

از نظر زبان JavaScript، Array الزام نمی‌کند که تمام عناصر یک Type داشته باشند.

اما در طراحی نرم‌افزار، معمولاً بهتر است یک Array مجموعه‌ای از Valueهای مرتبط و هم‌معنا را نگهداری کند.

برای مثال:

```js
const prices = [120, 250, 90, 300];
```

مدل داده واضح است.

در مقابل:

```js
const data = ["Laptop", 1200, true, null];
```

از نظر زبان معتبر است، اما از نظر طراحی داده لزوماً Collection معناداری نیست.

بنابراین باید میان **امکان زبان** و **طراحی مناسب داده** تفاوت قائل شویم.

---

## چرا ترتیب در Array اهمیت دارد؟

Array یک Collection مرتب است.

این بدان معناست که موقعیت هر عنصر بخشی از ساختار Array است.

در مثال زیر:

```js
const recipes = ["Pasta", "Pizza", "Salad"];
```

`Pasta` قبل از `Pizza` قرار دارد و `Pizza` قبل از `Salad`.

بنابراین Array فقط نمی‌گوید:

> این سه Value وجود دارند.

بلکه می‌گوید:

> این سه Value در این ترتیب قرار گرفته‌اند.

این ویژگی زمانی اهمیت زیادی پیدا می‌کند که ترتیب داده‌ها بخشی از منطق Application باشد.

برای مثال، ترتیب Itemهای یک Cart، ترتیب نتایج یک Search یا ترتیب مراحل یک Workflow می‌تواند معنا داشته باشد.

---

## Index و Element

حالا که چند Value را در یک Array قرار داده‌ایم، سؤال بعدی این است:

> چگونه یک Value مشخص را پیدا کنیم؟

برای پاسخ، Array به هر عنصر یک **Index** اختصاص می‌دهد.

Index موقعیت یک Element در Array را مشخص می‌کند.

نکته مهم این است که Index در JavaScript از **صفر** شروع می‌شود.

```js
const recipes = ["Pasta", "Pizza", "Salad"];
```

ساختار آن چنین است:

```text
Index       0         1         2
            ↓         ↓         ↓
         "Pasta"   "Pizza"   "Salad"
```

بنابراین:

```js
recipes[0]; // "Pasta"
recipes[1]; // "Pizza"
recipes[2]; // "Salad"
```

این یکی از مهم‌ترین قواعد Array در JavaScript است:

> اولین Element دارای Index برابر `0` است، نه `1`.

---

## چرا Index از صفر شروع می‌شود؟

برای استفاده روزمره، کافی است این قانون را به خاطر داشته باشیم.

اما از نظر مدل ذهنی، Index را بهتر است به‌عنوان **offset از ابتدای Collection** در نظر بگیریم.

اولین Element هیچ فاصله‌ای از ابتدای Array ندارد:

```text
offset = 0
```

دومین Element یک موقعیت جلوتر است:

```text
offset = 1
```

و به همین ترتیب ادامه پیدا می‌کند.

این مدل ذهنی بعدها در Iteration و محاسبه موقعیت عناصر بسیار مفید خواهد بود.

---

## دسترسی به Element

برای دسترسی به یک Element از **Bracket Notation** استفاده می‌کنیم:

```js
recipes[1];
```

عبارت داخل `[]` همان Index موردنظر است.

می‌توانیم Index را نیز از یک Variable دریافت کنیم:

```js
const index = 2;

recipes[index];
```

این ویژگی مهم است، زیرا در برنامه واقعی معمولاً Index به‌صورت ثابت نوشته نمی‌شود.

ممکن است Index نتیجه یک محاسبه یا بخشی از Logic برنامه باشد.

---

## آخرین Element

یکی از نیازهای رایج هنگام کار با Array، دسترسی به آخرین Element است.

از آنجا که Index از صفر شروع می‌شود، Index آخرین Element برابر است با:

```js
recipes.length - 1
```

بنابراین:

```js
recipes[recipes.length - 1];
```

آخرین Element را برمی‌گرداند.

این رابطه بسیار مهم است:

```text
first index → 0
last index  → length - 1
```

دلیل آن نیز مستقیم است.

اگر Array سه Element داشته باشد:

```text
length = 3
```

Indexها عبارت‌اند از:

```text
0, 1, 2
```

پس آخرین Index برابر است با:

```text
3 - 1 = 2
```

---

## اگر Index وجود نداشته باشد چه می‌شود؟

اگر به Indexای دسترسی پیدا کنیم که Elementی در آن موقعیت وجود ندارد، JavaScript مقدار `undefined` را برمی‌گرداند.

```js
const recipes = ["Pasta", "Pizza"];

console.log(recipes[5]); // undefined
```

این رفتار مهم است.

JavaScript در این حالت به‌طور خودکار Error ایجاد نمی‌کند.

بنابراین وجود نداشتن یک Element و وجود داشتن یک Element با مقدار `undefined` باید در تحلیل داده با دقت در نظر گرفته شود.

---

## Element چیست؟

هر Valueای که در Array قرار دارد یک **Element** آن Array است.

برای مثال:

```js
const products = ["Laptop", "Phone", "Tablet"];
```

سه Element داریم:

```text
"Laptop" → Element
"Phone"  → Element
"Tablet" → Element
```

Index و Element دو مفهوم متفاوت هستند.

Index موقعیت را مشخص می‌کند.

Element خود Valueای است که در آن موقعیت قرار گرفته است.

می‌توان این رابطه را چنین دید:

```text
Index
  ↓
Position
  ↓
Element
  ↓
Value
```

این تفکیک مفهومی برای درک Array بسیار مهم است.

---

## length؛ تعداد عناصر یا چیزی بیشتر؟

بعد از اینکه مفهوم Index و Element را فهمیدیم، سؤال طبیعی بعدی این است:

> از کجا بدانیم Array چند Element دارد؟

برای این کار از Propertyای به نام `length` استفاده می‌کنیم.

```js
const recipes = ["Pasta", "Pizza", "Salad"];

recipes.length; // 3
```

در یک Array معمولی، `length` تعداد عناصر را نشان می‌دهد.

اما از نظر فنی، `length` دقیقاً معادل «آخرین Index + 1» است و با ساختار Indexهای Array ارتباط دارد.

برای Array بالا:

```text
length = 3
last index = 2
```

پس:

```text
last index = length - 1
```

این رابطه را باید به‌عنوان بخشی از مدل ذهنی Array در نظر گرفت.

---

## تغییر length

`length` یک Property قابل تنظیم است.

می‌توان مقدار آن را تغییر داد.

برای مثال:

```js
const recipes = ["Pasta", "Pizza", "Salad"];

recipes.length = 2;

console.log(recipes);
```

اکنون Array فقط دو Element دارد:

```js
["Pasta", "Pizza"]
```

یعنی کاهش `length` می‌تواند باعث حذف Elementهای انتهایی Array شود.

همچنین می‌توان `length` را افزایش داد:

```js
recipes.length = 5;
```

در این حالت Array دارای فضای منطقی بیشتری خواهد شد و موقعیت‌های جدید می‌توانند بدون داشتن Element واقعی وجود داشته باشند.

این رفتار نشان می‌دهد که `length` صرفاً یک شمارنده ساده نیست.

---

## Array و Mutation

تا اینجا Array را ساخته و عناصر آن را خوانده‌ایم.

اما در Application واقعی، داده‌ها ثابت نیستند.

Recipe جدید ممکن است اضافه شود.

Product ممکن است حذف شود.

Cart ممکن است تغییر کند.

پس سؤال بعدی این است:

> چگونه State یک Array را تغییر دهیم؟

وقتی ساختار موجود را مستقیماً تغییر می‌دهیم، با مفهوم **Mutation** روبه‌رو هستیم.

برای مثال:

```js
const cart = ["Laptop", "Mouse"];

cart[1] = "Keyboard";
```

اکنون همان Array تغییر کرده است:

```js
["Laptop", "Keyboard"]
```

در اینجا Array جدیدی ایجاد نکرده‌ایم.

همان Array قبلی را تغییر داده‌ایم.

این همان Mutation است.

---

## Mutation فقط تغییر Element نیست

Mutation به تغییر دادن خود Array اشاره دارد.

برای مثال، تغییر یک Element:

```js
cart[0] = "Monitor";
```

یک Mutation است.

تغییر `length` نیز می‌تواند Mutation باشد:

```js
cart.length = 1;
```

در هر دو حالت، ساختار موجود Array تغییر کرده است.

این موضوع در طراحی Application اهمیت دارد، زیرا Mutation می‌تواند روی بخش‌های دیگری از برنامه که به همان Array دسترسی دارند نیز اثر بگذارد.

برای درک این موضوع باید به یک مفهوم مهم‌تر برسیم:

**Reference.**

---

## Array یک Reference Value است

تا اینجا ممکن است Array را مانند یک Value معمولی تصور کنیم.

اما Array در JavaScript یک **Object** است و Objectها به‌صورت Reference Value رفتار می‌کنند.

این موضوع باعث می‌شود Assignment یک Array با Assignment یک Primitive Value تفاوت داشته باشد.

برای مثال:

```js
const cartA = ["Laptop", "Mouse"];
const cartB = cartA;
```

ممکن است در نگاه اول تصور کنیم که اکنون دو Array داریم.

اما چنین نیست.

هر دو Variable به همان Object اشاره می‌کنند.

```text
cartA ─────┐
           ↓
      ["Laptop", "Mouse"]
           ↑
cartB ─────┘
```

بنابراین اگر از طریق `cartB` Array را تغییر دهیم:

```js
cartB[0] = "Monitor";
```

مقدار `cartA` نیز تغییر کرده است:

```js
console.log(cartA);
// ["Monitor", "Mouse"]
```

چرا؟

زیرا `cartA` و `cartB` دو Reference به یک Array هستند.

---

## Assignment یک Array کپی ایجاد نمی‌کند

این نکته یکی از مهم‌ترین مفاهیم Array است.

وقتی می‌نویسیم:

```js
const second = first;
```

برای یک Array، یک Array مستقل جدید ایجاد نمی‌شود.

Reference موجود کپی می‌شود.

مدل ذهنی صحیح:

```text
Variable A ──┐
             ↓
          Array
             ↑
Variable B ──┘
```

نه:

```text
Variable A → Array A

Variable B → Array B
```

این تفاوت دلیل بسیاری از رفتارهایی است که در ابتدا هنگام کار با Arrayها غیرمنتظره به نظر می‌رسند.

---

## مقایسه Arrayها

همین مفهوم هنگام مقایسه Arrayها نیز اهمیت دارد.

```js
const a = [1, 2, 3];
const b = [1, 2, 3];

console.log(a === b);
```

نتیجه:

```js
false
```

چرا؟

زیرا `a` و `b` دو Object متفاوت هستند.

محتوای آن‌ها یکسان است، اما Reference یکسان نیست.

در مقابل:

```js
const a = [1, 2, 3];
const b = a;

console.log(a === b);
```

نتیجه:

```js
true
```

زیرا هر دو Variable به همان Array اشاره می‌کنند.

بنابراین هنگام مقایسه Arrayها باید میان دو مفهوم تفاوت بگذاریم:

```text
Same Reference
```

و:

```text
Same Contents
```

عملگر `===` در اینجا Reference یکسان را بررسی می‌کند، نه برابری محتوای عناصر را.

---

## چرا Reference برای Array مهم است؟

زیرا در Applicationهای واقعی ممکن است یک Array بین چند بخش مختلف برنامه به اشتراک گذاشته شود.

فرض کنید:

```js
const cart = ["Laptop", "Mouse"];
```

یک بخش از برنامه Reference این Array را دریافت می‌کند.

اگر آن بخش Array را Mutation کند، هر بخش دیگری که همان Reference را در اختیار دارد تغییر را مشاهده خواهد کرد.

بنابراین هنگام کار با Array باید همیشه این سؤال را در ذهن داشته باشیم:

> آیا می‌خواهم همین Array را تغییر دهم یا می‌خواهم یک Array مستقل داشته باشم؟

این سؤال پایه‌ای، درک بسیاری از الگوهای بعدی JavaScript را ساده‌تر می‌کند.

---

## Array و Object بودن

Array یک Object است، اما یک Object معمولی برای نگهداری Collection نیست.

Array ویژگی‌هایی دارد که آن را برای داده‌های ترتیبی مناسب می‌کنند.

برای مثال:

```js
const products = ["Laptop", "Phone", "Tablet"];
```

در اینجا ترتیب و موقعیت عناصر اهمیت دارد.

در مقابل، Object معمولاً برای مدل کردن Propertyهای نام‌گذاری‌شده مناسب است:

```js
const product = {
  name: "Laptop",
  price: 1200,
  category: "Electronics"
};
```

در Object، ما معمولاً به دنبال یک Property مشخص هستیم.

در Array، معمولاً با یک موقعیت مشخص و یک ترتیب مواجهیم.

پس انتخاب میان Object و Array باید بر اساس **مدل داده** انجام شود.

---

## Arrayهای تو در تو

یک Array می‌تواند Elementهایی داشته باشد که خودشان Array باشند.

برای مثال:

```js
const categories = [
  ["Laptop", "Phone"],
  ["Shirt", "Shoes"]
];
```

در اینجا Elementهای اصلی خودشان Array هستند.

می‌توان با چند Index به داده داخلی دسترسی پیدا کرد:

```js
categories[0][1];
```

نتیجه:

```js
"Phone"
```

نکته مهم این است که هر `[]` یک مرحله دسترسی به یک Array را نشان می‌دهد.

```text
categories
    ↓
[0]
    ↓
["Laptop", "Phone"]
    ↓
[1]
    ↓
"Phone"
```

Arrayهای تو در تو برای داده‌های چندسطحی مفید هستند، اما هرچه ساختار عمیق‌تر شود، خوانایی و مدیریت داده نیز دشوارتر خواهد شد.

بنابراین استفاده از Nesting باید بر اساس نیاز واقعی Data Model باشد.

---

## Iteration؛ وقتی یک Element کافی نیست

تا اینجا می‌توانیم یک Element مشخص را با Index بخوانیم.

اما در Application واقعی معمولاً مسئله بزرگ‌تر است.

مثلاً ممکن است بخواهیم تمام Productهای یک Array را نمایش دهیم.

```js
const products = ["Laptop", "Phone", "Tablet"];
```

خواندن تک‌تک عناصر به‌صورت دستی:

```js
console.log(products[0]);
console.log(products[1]);
console.log(products[2]);
```

راه‌حل مناسبی نیست.

اگر Array شامل صد یا هزار Element باشد، چنین روشی غیرعملی است.

اینجا مفهوم **Iteration** وارد می‌شود.

Iteration یعنی حرکت کنترل‌شده روی عناصر یک Collection به‌گونه‌ای که بتوانیم هر Element را پردازش کنیم.

در ساده‌ترین شکل، از `for` استفاده می‌کنیم:

```js
for (let i = 0; i < products.length; i++) {
  console.log(products[i]);
}
```

در اینجا سه مفهوم قبلی با یکدیگر ترکیب می‌شوند:

```text
Array
  ↓
length
  ↓
Index
  ↓
Element
  ↓
Iteration
```

Loop با استفاده از Index از اولین Element شروع می‌کند و تا آخرین Element حرکت می‌کند.

---

## چرا length در Iteration اهمیت دارد؟

در مثال قبل:

```js
i < products.length
```

شرط Loop را تعیین می‌کند.

اگر Array سه Element داشته باشد:

```text
length = 3
```

Indexهای معتبر:

```text
0
1
2
```

هستند.

بنابراین شرط:

```js
i < 3
```

اجازه می‌دهد `i` مقادیر زیر را بگیرد:

```text
0
1
2
```

اما وقتی:

```text
i = 3
```

شرط برقرار نیست.

به همین دلیل استفاده از:

```js
i < array.length
```

با منطق Zero-Based Indexing سازگار است.

---

## Iteration بدون وابستگی به تعداد عناصر

یکی از مزایای مهم Iteration این است که Code ما دیگر به تعداد فعلی عناصر وابسته نیست.

این Code:

```js
for (let i = 0; i < products.length; i++) {
  console.log(products[i]);
}
```

برای این Array:

```js
["Laptop", "Phone"]
```

کار می‌کند.

و برای این Array نیز:

```js
["Laptop", "Phone", "Tablet", "Monitor", "Keyboard"]
```

همان Logic بدون تغییر کار می‌کند.

این همان چیزی است که Iteration را برای کار با Collectionها ضروری می‌کند.

در این فصل فقط مفهوم Iteration را معرفی کردیم.

الگوهای مختلف Iteration و انتخاب میان آن‌ها در فصل‌های بعدی به‌صورت مستقل بررسی خواهند شد.

---

## یک مدل ذهنی کامل از Array

اکنون می‌توانیم مفاهیمی را که در این فصل مرحله‌به‌مرحله ساختیم، کنار یکدیگر قرار دهیم.

فرض کنید:

```js
const recipes = ["Pasta", "Pizza", "Salad"];
```

در این مثال:

```text
Collection
    ↓
Array
    ↓
Elements
    ↓
Index
    ↓
length
    ↓
Mutation
    ↓
Reference
    ↓
Iteration
```

هر مفهوم پاسخ‌دهنده یک سؤال است.

**Collection**

چند Value مرتبط را کجا نگهداری کنیم؟

**Array**

برای داده‌های مرتب و دارای موقعیت چه ساختاری داشته باشیم؟

**Index**

چگونه یک Element مشخص را پیدا کنیم؟

**Element**

چه Valueای در آن موقعیت قرار دارد؟

**length**

Array چه تعداد موقعیت را در اختیار دارد؟

**Mutation**

چگونه Array موجود را تغییر دهیم؟

**Reference**

اگر Array را به Variable دیگری Assign کنیم، چه چیزی منتقل می‌شود؟

**Iteration**

چگونه روی تمام عناصر Collection حرکت کنیم؟

این زنجیره همان مدل ذهنی‌ای است که باید از این فصل باقی بماند.

---

## Best Practices

### Array را بر اساس مدل داده انتخاب کنید

اگر داده‌ها یک مجموعه مرتب و قابل پیمایش هستند، Array انتخاب طبیعی است.

اگر داده‌ها مجموعه‌ای از Propertyهای نام‌گذاری‌شده یک Entity هستند، Object معمولاً مدل مناسب‌تری است.

---

### هنگام کار با Array، Reference را فراموش نکنید

این دو Code یک معنا ندارند:

```js
const copy = original;
```

و:

```js
const copy = [...original];
```

در اولی Reference همان Array منتقل می‌شود.

در دومی یک Array جدید ایجاد می‌شود.

جزئیات Spread و Copy کردن Collectionها در فصل مربوط به الگوهای پیشرفته Array بررسی خواهد شد.

---

### از Index به‌صورت آگاهانه استفاده کنید

Index زمانی مناسب است که موقعیت Element بخشی از مسئله باشد.

اما نباید هر مسئله‌ای را صرفاً با Index حل کرد.

در بسیاری از عملیات روی Collection، روش‌های سطح بالاتر خواناتر و مناسب‌تر هستند که در فصل‌های بعدی بررسی می‌شوند.

---

### Mutation را آگاهانه انجام دهید

Mutation همیشه بد نیست.

گاهی تغییر مستقیم یک Array دقیقاً همان چیزی است که Application نیاز دارد.

اما باید بدانیم آیا بخش دیگری از برنامه همان Reference را در اختیار دارد یا خیر.

اگر چند بخش به یک Array مشترک دسترسی داشته باشند، Mutation می‌تواند State سایر بخش‌ها را نیز تغییر دهد.

---

### از `length` برای منطق وابسته به اندازه Collection استفاده کنید

به‌جای فرض کردن تعداد عناصر:

```js
i < 3
```

از ساختار داده استفاده کنید:

```js
i < recipes.length
```

این Code با تغییر تعداد عناصر همچنان معتبر باقی می‌ماند.

---

## Common Mistakes

### اشتباه اول: تصور اینکه Index از 1 شروع می‌شود

```js
recipes[1];
```

دومین Element را برمی‌گرداند، نه اولین Element را.

اولین Element:

```js
recipes[0];
```

است.

---

### اشتباه دوم: تصور اینکه Assignment یک Array را کپی می‌کند

```js
const second = first;
```

Array جدیدی ایجاد نمی‌کند.

هر دو Variable به همان Array اشاره می‌کنند.

---

### اشتباه سوم: مقایسه Arrayها بر اساس محتوا با `===`

```js
[1, 2] === [1, 2];
```

نتیجه `false` است، زیرا دو Array متفاوت ایجاد شده‌اند.

`===` در مورد Objectها و Arrayها بر اساس Reference عمل می‌کند.

---

### اشتباه چهارم: فرض اینکه `length` همیشه فقط یک شمارنده ساده است

`length` با ساختار Indexهای Array ارتباط دارد.

به همین دلیل:

```js
array[array.length - 1]
```

به آخرین Element اشاره می‌کند.

---

### اشتباه پنجم: نادیده گرفتن Mutation

وقتی یک Array را از طریق Reference مشترک تغییر می‌دهیم، سایر Variableهایی که همان Array را نگه می‌دارند نیز تغییر را مشاهده می‌کنند.

بنابراین باید همیشه مشخص باشد که تغییر State عمداً انجام شده یا ناخواسته.

---

### اشتباه ششم: نوشتن Loop وابسته به تعداد ثابت عناصر

```js
for (let i = 0; i < 3; i++) {
  console.log(products[i]);
}
```

اگر تعداد عناصر تغییر کند، Logic دیگر درست نیست.

استفاده از:

```js
i < products.length
```

Code را با اندازه واقعی Collection هماهنگ می‌کند.

---

## Summary

Array برای نگهداری مجموعه‌ای مرتب از Valueها استفاده می‌شود.

هر Array از Elementهایی تشکیل شده است که با Index شناسایی می‌شوند.

Index در JavaScript از صفر شروع می‌شود.

برای دسترسی به یک Element از Bracket Notation استفاده می‌کنیم:

```js
recipes[0];
```

Property `length` اندازه Array را مشخص می‌کند و آخرین Index معمولاً برابر است با:

```js
array.length - 1;
```

Array یک Object و در نتیجه یک Reference Value است.

بنابراین Assignment یک Array، Array جدیدی ایجاد نمی‌کند؛ بلکه Reference موجود را منتقل می‌کند.

Mutation باعث تغییر همان Array می‌شود و اگر Reference مشترک وجود داشته باشد، سایر بخش‌های برنامه نیز می‌توانند تغییر را مشاهده کنند.

در نهایت، وقتی تعداد عناصر بیشتر از یک مورد باشد، Iteration امکان پردازش عناصر Collection را به‌صورت سیستماتیک فراهم می‌کند.

---

## Key Takeaways

* **Array** یک Collection مرتب از Valueها است.
* هر Value داخل Array یک **Element** است.
* هر Element یک **Index** دارد.
* Indexها از `0` شروع می‌شوند.
* اولین Index برابر `0` و آخرین Index معمولاً برابر `length - 1` است.
* `length` با اندازه و ساختار Indexهای Array ارتباط دارد.
* Array یک Object و یک **Reference Value** است.
* Assignment یک Array، Reference را منتقل می‌کند، نه یک کپی مستقل را.
* تغییر Array از طریق یک Reference می‌تواند روی Referenceهای دیگر نیز اثر بگذارد.
* **Iteration** برای پردازش سیستماتیک عناصر Collection استفاده می‌شود.
* در طراحی حرفه‌ای، باید میان Array، Object و سایر Collectionها بر اساس مدل داده انتخاب کرد.

---

# Technical Interview

## Junior Level

### 1. Array در JavaScript چیست؟

Array یک Object است که برای نگهداری مجموعه‌ای مرتب از Valueها استفاده می‌شود و عناصر آن از طریق Index قابل دسترسی هستند.

---

### 2. Index در Array چیست؟

Index موقعیت یک Element در Array را مشخص می‌کند.

در JavaScript Index از `0` شروع می‌شود.

---

### 3. اولین Element یک Array چگونه قابل دسترسی است؟

با Index صفر:

```js
array[0];
```

---

### 4. چگونه آخرین Element یک Array را پیدا می‌کنیم؟

معمولاً با:

```js
array[array.length - 1];
```

زیرا آخرین Index برابر با `length - 1` است.

---

### 5. `length` در Array چه چیزی را نشان می‌دهد؟

`length` اندازه Array را نشان می‌دهد و در یک Array معمولی با تعداد Elementهای آن مرتبط است.

همچنین آخرین Index موجود معمولاً برابر با `length - 1` است.

---

### 6. آیا Array می‌تواند Valueهایی با Typeهای مختلف داشته باشد؟

بله.

JavaScript از نظر زبان چنین محدودیتی ندارد.

```js
const data = [10, "JavaScript", true];
```

اما در طراحی Application بهتر است عناصر یک Collection از نظر مفهومی مرتبط باشند.

---

### 7. اگر به Indexای خارج از محدوده Array دسترسی پیدا کنیم چه می‌شود؟

JavaScript معمولاً `undefined` برمی‌گرداند:

```js
const items = ["A", "B"];

items[5]; // undefined
```

---

## Mid-Level

### 8. چرا Index در JavaScript از صفر شروع می‌شود؟

Index را می‌توان به‌عنوان Offset از ابتدای Collection در نظر گرفت.

اولین Element دارای Offset صفر است، بنابراین اولین Index برابر `0` است و آخرین Index برابر `length - 1` خواهد بود.

---

### 9. آیا Assignment یک Array باعث ایجاد یک کپی مستقل می‌شود؟

خیر.

```js
const a = [1, 2, 3];
const b = a;
```

`a` و `b` به همان Array اشاره می‌کنند.

بنابراین:

```js
b[0] = 100;
```

مقدار `a[0]` را نیز تغییر می‌دهد.

---

### 10. چرا این مقایسه `false` است؟

```js
[1, 2, 3] === [1, 2, 3];
```

زیرا دو Array مستقل ایجاد شده‌اند.

هر Array یک Object متفاوت با Reference متفاوت است.

`===` در اینجا برابری Reference را بررسی می‌کند، نه برابری محتوای Arrayها را.

---

### 11. تفاوت Element و Index چیست؟

Index موقعیت Element را مشخص می‌کند.

Element خود Valueای است که در آن موقعیت قرار دارد.

برای مثال:

```js
const products = ["Laptop", "Phone"];
```

در:

```js
products[1];
```

عدد `1` Index و مقدار `"Phone"` Element است.

---

### 12. Mutation در Array چیست؟

Mutation یعنی تغییر دادن خود Array موجود.

برای مثال:

```js
products[0] = "Monitor";
```

در این حالت Array جدیدی ساخته نشده است؛ همان Array تغییر کرده است.

---

### 13. چرا Mutation می‌تواند در Application مشکل ایجاد کند؟

زیرا چند بخش از Application ممکن است Reference یک Array مشترک را داشته باشند.

اگر یک بخش آن Array را Mutation کند، سایر بخش‌هایی که همان Reference را دارند نیز State جدید را مشاهده می‌کنند.

بنابراین Mutation باید آگاهانه انجام شود.

---

### 14. آیا Array یک Primitive Value است؟

خیر.

Array یک Object است.

در نتیجه هنگام Assignment و مقایسه، رفتار Referenceای دارد.

---

### 15. چرا `array[array.length - 1]` آخرین Element را برمی‌گرداند؟

زیرا Indexها از صفر شروع می‌شوند.

اگر Array دارای `length` برابر `n` باشد، Indexهای آن از `0` تا `n - 1` هستند.

بنابراین آخرین Index:

```js
length - 1
```

است.

---

## Senior Level

### 16. آیا می‌توان گفت `length` همیشه تعداد واقعی Elementهای Array است؟

در کاربردهای معمول، `length` با تعداد عناصر Array هم‌راستا است، اما از نظر فنی `length` نشان‌دهنده اندازه ساختاری Array و بزرگ‌ترین Index موجود به‌علاوه یک است.

به همین دلیل امکان ایجاد Arrayهایی با **Empty Slots** وجود دارد و `length` می‌تواند از تعداد Elementهای واقعاً موجود متفاوت باشد.

این تفاوت در Arrayهای Sparse اهمیت پیدا می‌کند.

---

### 17. Arrayهای Sparse چه هستند؟

Sparse Arrayهایی هستند که در محدوده `0` تا `length - 1` برخی موقعیت‌ها Element واقعی ندارند.

برای مثال:

```js
const items = [];

items[2] = "Product";

console.log(items.length); // 3
```

در اینجا `length` برابر `3` است، اما فقط یک Element واقعی در Index `2` قرار دارد.

بنابراین نباید `length` را در تمام شرایط به‌عنوان شمارش ساده Elementهای موجود در نظر گرفت.

---

### 18. تفاوت Array و Object از دید مدل داده چیست؟

Array برای Collectionهای مرتب و Index-based مناسب است.

Object معمولاً برای نگهداری مجموعه‌ای از Propertyهای نام‌گذاری‌شده یک Entity مناسب‌تر است.

برای مثال:

```js
const product = {
  name: "Laptop",
  price: 1200
};
```

در اینجا `name` و `price` ویژگی‌های یک Entity هستند.

اما:

```js
const products = ["Laptop", "Phone", "Tablet"];
```

یک Collection مرتب از Products را نمایش می‌دهد.

بنابراین انتخاب میان آن‌ها باید بر اساس Data Model انجام شود، نه صرفاً Syntax.

---

### 19. چرا `const` مانع Mutation یک Array نمی‌شود؟

`const` مانع Reassignment Variable می‌شود، نه تغییر Objectای که Variable به آن اشاره می‌کند.

بنابراین:

```js
const products = ["Laptop", "Phone"];

products[0] = "Tablet";
```

مجاز است.

اما:

```js
products = ["Tablet"];
```

مجاز نیست.

مدل ذهنی صحیح:

```text
const
↓
Reference قابل تغییر نیست
↓
Object مورد اشاره الزاماً Immutable نیست
```

---

### 20. وقتی یک Array را به Function ارسال می‌کنیم، چه چیزی منتقل می‌شود؟

برای Array، Reference Value به Function منتقل می‌شود.

بنابراین اگر Function خود Array را Mutation کند، Caller می‌تواند تغییر را مشاهده کند.

```js
function updateProducts(products) {
  products[0] = "Monitor";
}

const products = ["Laptop", "Phone"];

updateProducts(products);

console.log(products);
// ["Monitor", "Phone"]
```

این رفتار نتیجه Reference Semantics مربوط به Objectهاست.

---

### 21. آیا `const` برای Array به معنی Immutable بودن Array است؟

خیر.

این یکی از سوءبرداشت‌های رایج است.

`const` فقط اجازه Reassignment Variable را نمی‌دهد.

بنابراین:

```js
const cart = ["Laptop"];

cart.push("Mouse");
```

مجاز است.

اما:

```js
cart = ["Phone"];
```

خطا ایجاد می‌کند.

پس باید میان **Immutable Binding** و **Immutable Object** تفاوت قائل شد.

---

### 22. مهم‌ترین مدل ذهنی برای کار با Array چیست؟

Array را نباید فقط به‌عنوان «لیستی از Valueها» دید.

مدل کامل‌تر این است:

```text
Array
 ↓
Ordered Collection
 ↓
Elements
 ↓
Index-based Access
 ↓
length
 ↓
Mutation
 ↓
Reference Semantics
 ↓
Iteration
```

این مدل ذهنی کمک می‌کند رفتار Array را در موقعیت‌های مختلف تحلیل کنیم، نه اینکه صرفاً Syntaxهای آن را حفظ کنیم.

---

# Conclusion

در این فصل از یک مسئله ساده شروع کردیم:

> اگر Application چند Value مرتبط داشته باشد، چگونه آن‌ها را در یک ساختار واحد نگهداری کنیم؟

پاسخ این سؤال، مفهوم **Collection** و سپس **Array** را وارد بحث کرد.

Array به ما اجازه می‌دهد چند Value مرتبط را در یک ساختار مرتب نگهداری کنیم. اما برای استفاده حرفه‌ای از Array، دانستن Syntax ایجاد آن کافی نیست.

باید بدانیم هر Element کجا قرار دارد.

اینجا **Index** وارد می‌شود.

Index موقعیت Element را مشخص می‌کند و از صفر شروع می‌شود. بنابراین اولین Element در Index `0` و آخرین Element معمولاً در Index `length - 1` قرار دارد.

سپس به `length` رسیدیم.

`length` به ما درباره اندازه ساختاری Array اطلاعات می‌دهد و ارتباط مستقیمی با Indexهای آن دارد. این Property یکی از پایه‌های اصلی کار با Array و به‌خصوص Iteration است.

بعد از آن، مسئله تغییر داده‌ها مطرح شد.

Array یک ساختار ثابت نیست. می‌توان State آن را تغییر داد. اما همین تغییر ما را به مفهوم **Mutation** می‌رساند.

Mutation زمانی مهم‌تر می‌شود که متوجه شویم Array یک Object و در نتیجه یک **Reference Value** است.

وقتی می‌نویسیم:

```js
const second = first;
```

یک Array جدید ایجاد نمی‌کنیم.

دو Variable به یک Array اشاره می‌کنند.

در نتیجه یک Mutation از طریق یک Reference می‌تواند از طریق Reference دیگری نیز مشاهده شود.

این نقطه، یکی از مهم‌ترین تفاوت‌های کار با Array در JavaScript با تصور ساده «کپی شدن مقدار» است.

در پایان به **Iteration** رسیدیم.

وقتی Collection کوچک باشد، دسترسی مستقیم به Indexها ممکن است کافی باشد. اما با افزایش تعداد عناصر، نیاز داریم روی Collection حرکت کنیم و عناصر آن را به‌صورت سیستماتیک پردازش کنیم.

به این ترتیب Concept Flow فصل کامل می‌شود:

```text
Collection
↓
Array
↓
Index
↓
Element
↓
length
↓
Mutation
↓
Reference
↓
Iteration
```

این زنجیره فقط فهرستی از اصطلاحات نیست.

هر مرحله پاسخی به مسئله‌ای است که از مرحله قبل ایجاد می‌شود:

```text
چند Value داریم؟
        ↓
Collection

چه Collectionای برای داده مرتب مناسب است؟
        ↓
Array

چگونه یک Value مشخص را پیدا کنیم؟
        ↓
Index

چه چیزی در آن موقعیت قرار دارد؟
        ↓
Element

چند موقعیت در اختیار داریم؟
        ↓
length

چگونه Collection را تغییر دهیم؟
        ↓
Mutation

اگر Array را به بخش دیگری از برنامه بدهیم چه اتفاقی می‌افتد؟
        ↓
Reference

چگونه روی تمام عناصر حرکت کنیم؟
        ↓
Iteration
```

اگر این رابطه مفهومی را درک کرده باشیم، Array دیگر مجموعه‌ای از Syntaxهای پراکنده نیست.

Array تبدیل می‌شود به یک **Ordered Collection با Index-based Access، قابل تغییر و Reference-based** که می‌توان عناصر آن را به‌صورت سیستماتیک Iteration کرد.

در فصل بعد، همین مدل ذهنی را یک گام جلوتر می‌بریم و بررسی می‌کنیم که JavaScript چه ابزارهایی برای **افزودن، حذف، جست‌وجو، برش، تغییر و ترکیب عناصر Array** در اختیار ما قرار می‌دهد.
