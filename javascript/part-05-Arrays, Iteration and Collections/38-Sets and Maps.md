# Chapter 38 — Sets and Maps
## Core Question
چه زمانی Array برای Collection مناسب نیست و باید از Set یا Map استفاده کنیم؟
جریان این فصل: Collection→ Array Limitations → Set → Unique Values → Map → Key / Value
→ Iteration→ Use Cases → Choosing the Collection
________________________________________
## مقدمه
در فصل‌های قبل دیدیم که Array یکی از مهم‌ترین ابزارهای JavaScript برای نگهداری مجموعه‌ای از داده‌هاست. البتهArray زمانی انتخاب مناسبی است که داده‌ها دارای Order باشند و بتوانیم با استفاده از Index به عناصر دسترسی پیدا کنیم. اما همه Collectionها چنین نیازی ندارند. فرض کنید می‌خواهیم لیستی از Categoryهای یک فروشگاه داشته باشیم: const categories = [ 'computer', 'mobile', 'computer', 'audio']
در اینجا وجود مقدار computer بیش از یک بار برای ما معنای خاصی ندارد. ما در واقع می‌خواهیم بدانیم:
چه Categoryهایی وجود دارند؟ نه اینکه: این Category چند بار در Array تکرار شده است؟
در چنین شرایطی Array می‌تواند کار را انجام دهد، اما ساختار داده‌ای که انتخاب کرده‌ایم دقیقاً با مسئله هماهنگ نیستJavaScript برای چنین نیازهایی Collectionهای دیگری نیزدر اختیار ما قرارمی‌دهد. دومورد مهم آن‌ها عبارت‌انداز:
• Set برای نگهداری Unique Values
• Map برای نگهداری ارتباط Key / Value
بنابراین مسئله اصلی این فصل یادگیری چند Method جدید نیست؛ بلکه یادگیری این است که هر Collection برای چه نوع مسئله‌ای طراحی شده است.
________________________________________
Set؛ زمانی که Unique Values مهم هستند
Set یک Collection از مقادیر Unique است؛ یعنی یک مقدار نمی‌تواند بیش از یک بار در آن وجود داشته باشد .برای ساختن یک Set از Constructor مربوط به آن استفاده می‌کنیم:
```js
const categories = new Set([ 'computer', 'mobile', 'computer', 'audio']) console.log(categories)
```
نتیجه شامل سه مقدار خواهد بود: computer mobile audio مقدار computer فقط یک بار نگهداری می‌شود. بنابراین اگر مسئله ما جلوگیری از تکرار داده‌ها باشد، Set مدل داده‌ای مناسب‌تری نسبت به Array است.
________________________________________
## افزودن و حذف مقدار در Set
برای اضافه کردن مقدار از add() استفاده می‌کنیم:
```js
const categories = new Set() categories .add('computer') categories .add('mobile')
```
اگر همان مقدار را دوباره اضافه کنیم، Collection تغییر نمی‌کند: categories .add('computer')
هنوز فقط دو مقدار داریم. برای حذف یک مقدار از delete() استفاده می‌کنیم: categories .delete('mobile') و برای بررسی وجود یک مقدار از has() استفاده می‌کنیم: // true console.log(categories .has('computer')) نکته مهم این است که Set برای پرسش‌هایی مانند: آیا این مقدار در Collection وجود دارد؟ مدل مناسبی ارائه می‌کند. مدل ذهنی: Set → Unique Values → add / delete / has
________________________________________
## تعداد عناصر Set
برای دانستن تعداد عناصر یک Set از size استفاده می‌کنیم: console.log(categories .size) size یک Property است، نه Method؛ بنابراین از () استفاده نمی‌کنیم. این تفاوت کوچک اما مهم است:
categories .size نه: categories .size()
________________________________________
Set و Iteration
Set برخلاف Array ،Index ندارد. بنابراین این نوع دسترسی برای آن وجود ندارد: categories[0] اگر بخواهیم عناصر آن را بررسی کنیم، می‌توانیم از for...of استفاده کنیم: for (const category of categories) {
console.log(category) }
همچنین می‌توانیم یک Set را با forEach() پیمایش کنیم:
categories .forEach(category => { console.log(category)})
در Set ترتیب ورود مقادیر حفظ می‌شود، اما هدف اصلی آن دسترسی با Index نیست؛ هدف اصلی، نگهداری مقادیر Unique است. این تفاوت در انتخاب Collection اهمیت زیادی دارد.
________________________________________
## تبدیل Array به Set
یکی از کاربردهای بسیار رایج Set حذف مقادیر تکراری از یک Array است. const numbers = [1, 2, 2, 3, 3, 4]
می‌توانیم آن را به Set تبدیل کنیم: // Set(4) { 1, 2, 3, 4 } const uniqueNumbers = new Set(numbers) اگر دوباره به یک Array نیاز داشته باشیم، از Spread استفاده می‌کنیم؛
```js
const uniqueNumbers = [...new Set(numbers)] اکنون نتیجه یک Array جدید است: [1, 2, 3, 4]
```
اینجا ارتباط مستقیمی با فصل قبل داریم: Array → Set → Unique Values → Spread → New Array بنابراین Set فقط یک Collection مستقل نیست؛ بلکه می‌تواند در کنار Array برای حل مسائل واقعی استفاده شود.
________________________________________
الگوی کاربردی Set؛ حذف داده‌های تکراری
یکی از کاربردهای رایج Set زمانی است که داده‌ها از یک منبع خارجی دریافت شده‌اند و ممکن است مقدار تکراری داشته باشند. فرض کنید شناسه چند محصول را داریم: const productIds = [101, 102, 101, 103, 102] اگر بخواهیم فقط شناسه‌های Unique را نگه داریم:
```js
const uniqueProductIds = [...new Set(productIds)] console.log(uniqueProductIds) // [101, 102, 103]
```
در این الگو، Set مسئول حذف تکرارهاست و Spread نتیجه را دوباره به Array تبدیل می‌کند. این الگو زمانی مفید است که خروجی نهایی همچنان باید Array باشد، اما قبل از آن باید داده‌های تکراری حذف شوند.
مدل ذهنی: Array with duplicates → Set → Unique Values → Array
________________________________________
Map؛ زمانی که داده‌ها رابطه Key / Value دارند
در Set هر Element یک Value بود. اما گاهی هر داده به یک Key مشخص وابسته است. برای مثال می‌خواهیم اطلاعات قیمت محصولات را بر اساس نام محصول نگهداری کنیم: Laptop → 1200 Phone → 800 Tablet → 600 در چنین شرایطی Map انتخاب مناسبی است.
Map یک Collection از زوج‌های: Key → Value است. برای ایجاد آن: const prices = new Map() سپس با set() یک Key و Value اضافه می‌کنیم: prices .set('Laptop', 1200) prices .set('Phone', 800) اکنون می‌توانیم مقدار مربوط به یک Key را با get() دریافت کنیم: // 1200 console.log(prices .get('Laptop')) برای بررسی وجود یک Key از has() استفاده می‌کنیم: // true console.log(prices .has('Phone')) و برای حذف یک Entry از delete() استفاده می‌کنیم: prices .delete('Phone')
مدل ذهنی: Map → Key / Value → set / get / has / delete
________________________________________
Map برخلاف Object
در نگاه اول ممکن است Map شبیه Object به نظر برسد.
در Map می‌توانیم بنویسیم: و در Object:
const prices = { const prices = new Map([
Laptop: 1200, ['Laptop', 1200],
Phone: 800 } ['Phone', 800] ])
هر دو می‌توانند رابطه‌ای بین یک Key و یک Value ایجاد کنند، اما هدف و رفتار آن‌ها یک سان نیست. Object در درجه اول برای مدل‌کردن یک Entity و Properties آن مناسب است: const user = { name: 'Ali' , age: 30}
در مقابل، Map زمانی مناسب‌تر است که خودِ رابطه: Key → Value بخش اصلی مسئله باشد. برای مثال:
const scores = new Map() scores .set('Ali', 18) scores .set('Sara', 20)
در اینجا نام شخص نقش Key را دارد و امتیاز نقش Value .
یک تفاوت مهم دیگر این است که Keyهای Map می‌توانند فقط String یا Symbol نباشند؛ Map می‌تواند Object، Array و سایر Valueها را نیز به‌عنوان Key نگهداری کند. const user = { name: 'Ali' }
const scores = new Map() scores .set(user, 20) console.log(scores .get(user)) // 20
بنابراین Map برای زمانی طراحی شده است که یک Collection واقعی از ارتباط‌های Key / Value داشته باشیم.
________________________________________
ایجاد Map با Initial Values
می‌توانیم هنگام ساخت Map، Entryهای اولیه را نیز مشخص کنیم:
```js
const prices = new Map([ ['Laptop', 1200] , ['Phone', 800] , ['Tablet', 600] ])
```
هر Entry یک Array دو عضوی است: [Key, Value] این ساختار در زمان Iteration نیز اهمیت پیدا می‌کند.
________________________________________
Iteration روی Map
می‌توانیم مستقیماً روی Map با for...of پیمایش کنیم:
```js
for (const [product, price] of prices) { console.log(product, price) }
```
اینجا از Destructuring فصل قبل نیز استفاده کرده‌ایم. هر Entry از Map به شکل: [Key, Value] در اختیار ما قرار می‌گیرد و با Array Destructuring آن را به دو متغیر تقسیم می‌کنیم: [product, price]
Map همچنین متدهای keys() و values() و entries() را در اختیار ما قرار می‌دهد. برای مثال:
for (const product of prices .keys()) { console.log(product)}
for (const price of prices .values()) { console.log(price) }
entries() زوج‌های for (const [product, price] of prices .entries()) { console.log(product, price)}
Key / Value را برمی‌گرداند. درواقع Iteration در Map به ما اجازه می‌دهد مستقیماً باساختار اصلی Collection کارکنیم________________________________________
## اندازه Map
مانند Set، تعداد Entryهای Map نیز با size مشخص می‌شود: console.log(prices .size)
برای Map نیز: size یک Property است. 
________________________________________
## الگوی کاربردی Map؛ دسترسی سریع به داده بر اساس Key
فرض کنید اطلاعات کاربران را داریم و در بخش‌های مختلف برنامه لازم است کاربر را بر اساس id پیدا کنیم. می‌توانیم اطلاعات را به صورت Map سازمان‌دهی کنیم: const users = new Map([ [101, 'Ali'], [102, 'Sara'], [103, 'Reza']]) اکنون برای دریافت نام کاربر کافی است Key را در اختیارget() قرار دهیم: // Sara console.log(users .get(102)) در اینجا id نقش Key و اطلاعات کاربر نقش Value را دارد. این الگو زمانی مناسب است که سؤال اصلی برنامه چیزی شبیه این باشد:اطلاعات مربوط به اینKey چیست؟ درچنین مسئله‌ای Map مستقیماًبامدل داده موردنیاز ماهماهنگ است
________________________________________
انتخاب بین Array، Set و Map
اکنون سه Collection مهم داریم: Array → Ordered Collection + Index Set → Unique Values
بنابراین انتخاب Collection باید از مسئله شروع شود. Map → Key / Value اگر داده‌ها دارای ترتیب هستند و Index اهمیت دارد : Array انتخاب طبیعی است.
```js
const products = ['Laptop', 'Phone', 'Tablet']
```
اگر فقط می‌خواهیم مجموعه‌ای از مقادیر Unique داشته باشیم: Set انتخاب مناسب‌تری است.
```js
const categories = new Set([ 'computer', 'mobile', 'audio' ])
```
اگر هر مقدار به یک Key مشخص وابسته است: Map انتخاب مناسب‌تری است.
```js
const prices = new Map([ ['Laptop', 1200], ['Phone', 800] ])
```
بنابراین به جای حفظ کردن APIها، ابتدا باید سؤال درست را مطرح کنیم: داده من چه رابطه‌ای دارد؟
اگر پاسخ: Position / Order باشد، معمولاً Array مناسب است.
اگر پاسخ: Unique Values باشد، Set گزینه مناسبی است.
اگر پاسخ: Key → Value باشد، Map انتخاب مناسبی است.
________________________________________
## Best Practices
• قبل از انتخاب Collection، مسئله داده را مشخص کنید.
• برای داده‌های Unique از Set استفاده کنید.
• برای رابطه مستقیم Key / Value از Map استفاده کنید.
• برای داده‌هایی که Order و Index اهمیت دارند، Array را انتخاب کنید.
• برای دریافت مقدار از Map از get() استفاده کنید.
• برای بررسی وجود مقدار در Set از has() استفاده کنید.
• برای تعداد عناصر Set و Map از size استفاده کنید.
• فقط به دلیل شباهت Map و Object آن‌ها را جایگزین یکدیگر ندانید.

## Common Mistakes
اشتباه اول: استفاده از Array برای حذف دستی مقادیر تکراری
اگر هدف اصلی Collection، Unique بودن داده‌هاست، استفاده از Set مدل مناسب‌تری ارائه می‌دهد.
________________________________________
اشتباه دوم: تصور اینکه Set دارای Index است
Set برای دسترسی با Index طراحی نشده است.
________________________________________
اشتباه سوم: استفاده اشتباه از Map
این کد: prices['Laptop'] روش دسترسی به Map نیست. برای Map باید از: prices .get('Laptop') استفاده کنیم.
________________________________________
اشتباه چهارم: اشتباه گرفتن size با Method
صحیح: prices .size غلط: prices .size()
________________________________________
اشتباه پنجم: تصور اینکه Map و Object کاملاً یکسان هستند
هر دو می‌توانند رابطه Key / Value ایجاد کنند، اما مدل داده و API آن‌ها متفاوت است.
________________________________________
## Summary
Array، Set و Map همگی برای نگهداری Collection استفاده می‌شوند، اما مسئله‌ای که حل می‌کنند یکسان نیست. Array برای Collectionهایی مناسب است که Order و Index اهمیت دارد. Set برای نگهداری Unique Values طراحی شده است و هر مقدار را فقط یک بار نگهداری می‌کند. Map برای نگهداری ارتباط Key / Value طراحی شده است و API مشخصی مانند set()، get()، has() و delete() ارائه می‌دهد.
مهم‌ترین نکته این فصل، حفظ کردن Methodها نیست؛ بلکه توانایی انتخاب Collection مناسب بر اساس مدل داده است.
________________________________________
## Key Takeaways
• Array → Ordered Collection و Index
• Set → Unique Values
• Map → Key / Value
• Set با add()، delete() و has() مدیریت می‌شود.
• Map با set()، get()، delete() و has() مدیریت می‌شود.
• هر دو Set و Map دارای Property به نام size هستند.
• Set ، Index ندارد.
• Map با get() به Value دسترسی می‌دهد.
• Map الزاماً جایگزین Object نیست؛ انتخاب بین آن‌ها به مدل داده بستگی دارد.
• انتخاب Collection باید از نیاز داده شروع شود، نه از Syntax .

## Technical Interview
### Junior
⭐ Array چیست؟
Collection ترتیبی از Values است که Elements آن با Index قابل دسترسی هستند.
________________________________________
⭐ Set چیست؟
Collectionای از Unique Values است که هر مقدار را فقط یک بار نگه می‌دارد.
________________________________________
⭐ Map چیست؟
Collectionای از Key / Value ، Pairs است که برای نگهداری و دسترسی به داده بر اساس Key طراحی شده است. ________________________________________
⭐ تفاوت اصلی Array و Set چیست؟
Array یک Collection ترتیبی با Index است، در حالی که Set برای نگهداری مقادیر Unique طراحی شده و مقدار تکراری را نگه نمی‌دارد.________________________________________
⭐ چگونه بررسی می‌کنیم یک مقدار در Set وجود دارد؟
با استفاده از متد has(): set .has(value)
________________________________________
⭐ چگونه تعداد عناصر Set را به دست می‌آوریم؟
با Property به نام size: set .size
________________________________________
### Mid-Level
⭐ تفاوت get() و set() در Map چیست؟
set() یک Key و Value را اضافه یا به‌روزرسانی می‌کند، در حالی که get() مقدار مربوط به یک Key را برمی‌گرداند.________________________________________
⭐ چه زمانی Set را به Array ترجیح می‌دهید؟
زمانی که Unique بودن داده‌ها نیاز اصلی باشد و دسترسی بر اساس Index اهمیت نداشته باشد.
________________________________________
⭐ چه تفاوتی بین Map و Object وجود دارد؟
هر دو می‌توانند رابطه Key / Value ایجاد کنند، اما Map به‌طور اختصاصی برای Collectionهای Key / Value طراحی شده، API مشخصی مانند get() و set() دارد و Keyهای آن می‌توانند انواع مختلفی از Valueها باشند.
________________________________________
### Senior
⭐ چرا انتخاب Collection باید بر اساس مدل داده انجام شود؟
چون هر Collection یک مدل دسترسی و مجموعه‌ای از تضمین‌های متفاوت ارائه می‌کند. Array برای Order و Index، Set برای Unique Values و Map برای Key / Value طراحی شده است. انتخاب صحیح باعث می‌شود ساختار داده با مسئله هماهنگ باشد و منطق برنامه ساده‌تر شود.
________________________________________
⭐ چگونه مقادیر تکراری یک Array را حذف می‌کنید؟
const uniqueValues = [...new Set(values)]
Set مقادیر تکراری را حذف می‌کند و Spread نتیجه را دوباره به یک Array تبدیل می‌کند.
________________________________________
⭐ آیا Map جایگزین Object است؟
خیر. Object برای مدل‌کردن Entity و Properties آن بسیار مناسب است، در حالی که Map برای Collectionهایی طراحی شده که رابطه Key / Value هسته اصلی داده است.
________________________________________
## Conclusion
در فصل‌های گذشته یاد گرفتیم که Array چگونه داده‌ها را نگهداری و پردازش می‌کند و با Destructuring و Spread می‌توانیم ساختار آن را به شکل‌های مختلف مدیریت کنیم. اما یک مهندس JavaScript نباید هر Collection را با Array حل کند.
گاهی مسئله ما ترتیب و Index است؛ در این حالت Array انتخاب مناسبی است. گاهی مسئله ما فقط نگهداری Unique Values است؛ در این حالت Set مدل دقیق‌تری ارائه می‌دهد و گاهی مسئله اصلی ایجاد رابطه بین یک Key و Value است؛ در این حالت Map ابزار مناسب‌تری است.
بنابراین مدل ذهنی این فصل را می‌توان در یک جمله خلاصه کرد:
ابتدا مدل داده را تشخیص بده، سپس Collection مناسب را انتخاب کن.
