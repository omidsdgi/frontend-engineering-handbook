Chapter 53 — Creating and Modifying DOM Elements
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

با createElement() یک DOM Element جدید ایجاد کنید.
تفاوت میان ایجاد یک Node و قرار دادن آن در DOM را توضیح دهید.
Node جدید را قبل از قرار دادن در DOM آماده کنید.
با append() و prepend() Nodeها را به یک Element اضافه کنید.
با before() و after() جایگاه Node را نسبت به یک Element مشخص کنید.
Elementهای موجود را با remove() از DOM حذف کنید.
UI را به‌صورت Dynamic بر اساس Data ایجاد کنید.
نقش DocumentFragment را در گروه‌بندی چند DOM Update درک کنید.
Core Question

چگونه UI را به‌صورت Dynamic با JavaScript تولید و تغییر دهیم؟

جریان این فصل:

DOM
↓
createElement
↓
Content
↓
append / prepend
↓
before / after
↓
remove
↓
Dynamic Rendering
↓
DocumentFragment
مقدمه

در فصل قبل یاد گرفتیم چگونه Elementهای موجود در DOM را پیدا و تغییر دهیم.

اما این کار یک پیش‌فرض مهم دارد:

Element موردنیاز ما از قبل در DOM وجود داشته باشد.

در یک Application واقعی، همیشه چنین نیست.

فرض کنید کاربر در یک Recipe Application جستجو می‌کند. نتیجه جستجو از API دریافت می‌شود، اما Result Cardهای مربوط به Recipeها هنوز در صفحه وجود ندارند.

پس مسئله دیگر فقط تغییر یک Element موجود نیست.

ما باید:

Data
↓
New DOM Elements
↓
Rendered UI

ایجاد کنیم.

بنابراین بعد از یادگیری Selection و Manipulation، سؤال طبیعی بعدی این است:

اگر Element موردنیاز هنوز وجود نداشته باشد، چگونه آن را ایجاد کنیم؟

پاسخ با createElement() آغاز می‌شود.

ایجاد یک Element جدید

فرض کنید باید یک Result جدید برای Recipe ایجاد کنیم.

JavaScript برای ایجاد Element جدید متدی به نام createElement() در اختیار ما قرار می‌دهد:

const recipe = document.createElement('article');

در اینجا یک article جدید ایجاد شده است.

اما هنوز چیزی در صفحه تغییر نکرده است.

دلیل آن ساده است:

createElement() فقط یک Node جدید ایجاد می‌کند؛ آن را هنوز به DOM متصل نمی‌کند.

بنابراین باید میان دو مفهوم تفاوت بگذاریم:

createElement()
↓
Create Node

و:

append()
↓
Insert Node

این جداسازی اهمیت زیادی دارد، زیرا معمولاً می‌خواهیم Node را ابتدا آماده کنیم و بعد آن را وارد DOM کنیم.

آماده‌سازی Node جدید

Element جدیدی که ایجاد کرده‌ایم هنوز Content یا ویژگی موردنیاز UI را ندارد.

بنابراین قبل از Insert کردن آن، می‌توانیم آن را آماده کنیم.

مثلاً:

const recipe = document.createElement('article');

recipe.classList.add('recipe-card');
recipe.textContent = 'Pasta';

اکنون Node آماده ورود به DOM است.

فرآیند ایجاد یک Element را می‌توان چنین دید:

createElement()
↓
New Node
↓
Configure Node
↓
Ready for DOM

در این مرحله همان APIهایی که در فصل قبل برای تغییر Content و Classها یاد گرفتیم دوباره استفاده می‌شوند.

تفاوت این است که این بار روی Elementی کار می‌کنیم که خودمان تازه ایجاد کرده‌ایم.

قرار دادن Node در DOM

اکنون Node ساخته شده و آماده است.

اما هنوز بخشی از DOM Tree نیست.

برای قرار دادن آن در ساختار DOM باید مشخص کنیم:

این Node باید در کدام قسمت Tree قرار بگیرد؟

اگر بخواهیم Node را به انتهای Childهای یک Element اضافه کنیم، از append() استفاده می‌کنیم.

const list = document.querySelector('.recipes');

list.append(recipe);

اکنون ساختار DOM به شکل مفهومی چنین است:

recipes
├── existing recipe
├── existing recipe
└── recipe

بنابراین append() مسئله قرار دادن Node در انتهای Childهای Parent را حل می‌کند.

قرار دادن Node در ابتدای یک Element

گاهی ترتیب Nodeها اهمیت دارد.

فرض کنید جدیدترین Recipe باید قبل از Recipeهای قبلی نمایش داده شود.

در این حالت اضافه کردن Node به انتهای فهرست مناسب نیست.

می‌توانیم از prepend() استفاده کنیم:

list.prepend(recipe);

اکنون Node جدید در ابتدای Childهای list قرار می‌گیرد.

recipes
├── new recipe
├── existing recipe
└── existing recipe

پس تفاوت این دو متد را می‌توان به‌سادگی چنین بیان کرد:

append()
→ انتهای Childها

prepend()
→ ابتدای Childها

تا اینجا مسئله ما قرار دادن Node به‌عنوان Child یک Element بود.

اما همیشه نمی‌خواهیم Node جدید را صرفاً اول یا آخر Childها قرار دهیم.

گاهی جایگاه دقیق آن نسبت به یک Element مشخص است.

قرار دادن Node قبل یا بعد از یک Element

فرض کنید فهرست Recipeها چنین ساختاری دارد:

Recipe List
├── Recipe A
├── Recipe B
└── Recipe C

اکنون می‌خواهیم Recipe جدید دقیقاً قبل از Recipe B قرار بگیرد.

در این شرایط مسئله ما دیگر «اول یا آخر Childهای Parent» نیست.

ما یک Reference مشخص داریم و می‌خواهیم Node جدید نسبت به همان Reference قرار بگیرد.

برای این کار از before() استفاده می‌کنیم:

recipeB.before(newRecipe);

نتیجه:

Recipe List
├── Recipe A
├── New Recipe
├── Recipe B
└── Recipe C

اگر بخواهیم Node بعد از Recipe B قرار بگیرد، از after() استفاده می‌کنیم:

recipeB.after(newRecipe);

نتیجه:

Recipe List
├── Recipe A
├── Recipe B
├── New Recipe
└── Recipe C

بنابراین اکنون چهار روش اصلی برای Insert کردن Node داریم:

append()
→ انتهای Childهای Element

prepend()
→ ابتدای Childهای Element

before()
→ قبل از یک Element

after()
→ بعد از یک Element

انتخاب میان این متدها باید بر اساس جایگاه موردنظر Node در DOM Tree انجام شود.

حذف یک Element

Dynamic UI فقط به ایجاد و اضافه کردن Node محدود نیست.

ممکن است یک Element پس از تغییر وضعیت Application دیگر موردنیاز نباشد.

برای مثال، Application در هنگام دریافت اطلاعات یک Loading Element نمایش می‌دهد:

Loading...

پس از دریافت Data، دیگر نیازی به آن Element نداریم.

در این شرایط می‌توان آن را با remove() حذف کرد:

loading.remove();

Element از DOM جدا می‌شود.

بنابراین:

Existing Node
↓
remove()
↓
Node removed from DOM

remove() زمانی مفید است که Reference خود Element را در اختیار داریم.

removeChild()

روش دیگری برای حذف Node وجود دارد:

parent.removeChild(child);

در این روش Parent مسئول حذف Child است.

در مقابل:

child.remove();

خود Node از DOM جدا می‌شود.

هر دو روش برای حذف Node قابل استفاده‌اند، اما وقتی Reference خود Element را داریم، remove() بیان مستقیم‌تری از عملی است که می‌خواهیم انجام دهیم.

Dynamic Rendering

اکنون می‌توانیم مسئله اصلی فصل را دوباره بررسی کنیم.

فرض کنید API فهرستی از Recipeها را برگردانده است:

const recipes = [
{ title: 'Pasta' },
{ title: 'Pizza' },
{ title: 'Risotto' }
];

این Data به‌تنهایی UI نیست.

Application باید آن را به DOM Structure تبدیل کند.

برای هر Recipe می‌توانیم یک Element ایجاد کنیم:

recipes.forEach(recipe => {
const item = document.createElement('li');

item.textContent = recipe.title;

recipeList.append(item);
});

در اینجا یک الگوی مهم شکل می‌گیرد:

Data
↓
Create Element
↓
Configure Element
↓
Insert into DOM
↓
Rendered UI

این همان چیزی است که Dynamic Rendering را شکل می‌دهد.

UI دیگر صرفاً چیزی نیست که هنگام بارگذاری HTML از قبل وجود داشته باشد.

بخشی از UI می‌تواند در زمان اجرای Application و بر اساس Data ایجاد شود.

UI از Data ساخته می‌شود

در یک Application واقعی، Data ممکن است تغییر کند.

نتیجه Search ممکن است جدید باشد، تعداد محصولات ممکن است تغییر کند یا کاربر ممکن است یک Item را حذف کند.

در چنین شرایطی DOM باید بتواند خود را با وضعیت جدید هماهنگ کند.

بنابراین:

Application State
↓
UI Representation
↓
DOM

رابطه‌ای طبیعی میان Data و DOM ایجاد می‌شود.

اگر Data جدیدی دریافت شود، ممکن است به:

Create
↓
Insert

نیاز داشته باشیم.

اگر یک بخش دیگر معتبر نباشد:

Remove

و اگر Node موجود باید با محتوای جدید جایگزین شود، می‌توانیم از روش‌های مناسب DOM برای جایگزینی آن استفاده کنیم.

هدف اصلی این فصل، درک همین چرخه است:

JavaScript می‌تواند ساختار DOM را در زمان اجرا بر اساس وضعیت Application تغییر دهد.

چرا چند DOM Update می‌تواند مسئله‌ساز شود؟

فرض کنید باید تعداد زیادی Element جدید ایجاد کنیم.

ممکن است برای هر Item یک Node بسازیم و بلافاصله آن را وارد DOM کنیم:

Create Item 1 → Insert
Create Item 2 → Insert
Create Item 3 → Insert
Create Item 4 → Insert
...

در تعداد کم، این روش کاملاً قابل قبول است.

اما وقتی تعداد Updateها زیاد می‌شود، سؤال دیگری مطرح می‌شود:

آیا لازم است هر Node را همان لحظه وارد DOM کنیم؟

برای پاسخ به این سؤال، باید بتوانیم چند Node را ابتدا خارج از Document اصلی آماده کنیم و سپس آن‌ها را یکجا وارد کنیم.

اینجا DocumentFragment وارد جریان می‌شود.

DocumentFragment

DocumentFragment یک Container موقت برای Nodeهاست.

می‌توانیم Nodeهای موردنظر را ابتدا در آن قرار دهیم:

const fragment = document.createDocumentFragment();

fragment.append(item1);
fragment.append(item2);
fragment.append(item3);

اکنون:

Document
│
└── Fragment
├── item1
├── item2
└── item3

سپس Fragment را در محل موردنظر Insert می‌کنیم:

recipeList.append(fragment);

در نتیجه Nodeهای موجود در Fragment وارد recipeList می‌شوند.

نکته مهم این است که DocumentFragment بخشی از UI نهایی نیست.

نقش آن بیشتر شبیه یک Container موقت برای آماده‌سازی گروهی Nodeهاست.

مدل ذهنی:

Create Fragment
↓
Create Nodes
↓
Configure Nodes
↓
Append Nodes to Fragment
↓
Append Fragment to DOM
چرا DocumentFragment؟

نیاز اصلی اینجا گروه‌بندی DOM Updateها است.

به‌جای اینکه فرآیند ساخت و Insert کردن تعداد زیادی Node را به‌صورت پراکنده انجام دهیم، می‌توانیم ابتدا مجموعه Nodeها را آماده کنیم و سپس آن مجموعه را در محل موردنظر قرار دهیم.

این الگو مخصوصاً در سناریوهایی مفید است که تعداد زیادی Node باید به‌صورت برنامه‌ریزی‌شده ساخته شوند.

البته DocumentFragment یک راه‌حل جادویی برای تمام مسائل Performance نیست.

نکته مهم‌تر این است که ابتدا باید مسئله DOM Update را درست مدل کنیم و سپس در صورت نیاز از Fragment برای گروه‌بندی عملیات استفاده کنیم.

از یک Node تا یک UI کامل

اکنون تمام مسیر Concept Flow را داریم.

ابتدا فقط یک مسئله ساده داشتیم:

چگونه Elementی را ایجاد کنیم که در DOM وجود ندارد؟

پاسخ:

createElement

اما بعد از ایجاد Element، مسئله بعدی ظاهر شد:

چگونه آن را در ساختار DOM قرار دهیم؟

پاسخ:

append
prepend
before
after

سپس مسئله حذف مطرح شد:

remove

و در نهایت، وقتی تعداد Nodeها افزایش پیدا کرد، نیاز به آماده‌سازی گروهی Nodeها مطرح شد:

DocumentFragment

بنابراین جریان کامل به این شکل است:

DOM
↓
createElement
↓
Content
↓
append / prepend
↓
before / after
↓
remove
↓
Dynamic Rendering
↓
DocumentFragment

این Flow در واقع مسیر طبیعی ایجاد و تغییر یک UI Dynamic است.

Best Practices
1. Creation را از Insertion جدا کنید

ابتدا Node را ایجاد و آماده کنید و سپس آن را وارد DOM کنید.

const card = document.createElement('article');

card.textContent = 'Recipe';

recipeList.append(card);
2. متد Insert را بر اساس جایگاه Node انتخاب کنید

به‌جای حفظ کردن Syntax، جایگاه موردنظر را در DOM مشخص کنید:

append()
→ انتهای Childها

prepend()
→ ابتدای Childها

before()
→ قبل از Reference

after()
→ بعد از Reference
3. Node موجود را بی‌دلیل دوباره ایجاد نکنید

اگر یک Node موجود باید به موقعیت دیگری منتقل شود، می‌توان همان Node را در محل جدید Insert کرد.

ایجاد Node جدید همیشه لازم نیست.

4. Dynamic Rendering را بر اساس Data طراحی کنید

در UIهای Dynamic بهتر است رابطه میان Data و DOM روشن باشد:

Data
↓
DOM Representation
↓
Rendered UI
5. DocumentFragment را برای گروه‌بندی Updateها به‌کار ببرید

وقتی تعداد زیادی Node باید ایجاد و سپس وارد یک بخش از DOM شوند، DocumentFragment می‌تواند Container موقت مناسبی برای آماده‌سازی آن‌ها باشد.

Common Mistakes
اشتباه اول: تصور اینکه createElement() عنصر را در صفحه قرار می‌دهد
const item = document.createElement('li');

این کد فقط Node را ایجاد می‌کند.

برای قرار گرفتن در DOM باید آن را Insert کنیم.

اشتباه دوم: اشتباه گرفتن append() با prepend()

append() Node را در انتهای Childها قرار می‌دهد، در حالی که prepend() آن را در ابتدای Childها قرار می‌دهد.

اشتباه سوم: استفاده از Parent برای مسئله‌ای که جایگاه نسبی دارد

اگر هدف این است که Node دقیقاً قبل یا بعد از یک Element قرار بگیرد، before() یا after() بیان مستقیم‌تری از مسئله هستند.

اشتباه چهارم: تصور اینکه Insert کردن Node موجود آن را Clone می‌کند

اگر یک Node موجود را به Parent دیگری اضافه کنیم، Node جابه‌جا می‌شود.

کپی جدیدی از آن ایجاد نمی‌شود.

اشتباه پنجم: تصور اینکه remove() محتوای Element را پاک می‌کند

remove() خود Element را از DOM خارج می‌کند.

این عملیات با تغییر textContent یکسان نیست.

اشتباه ششم: استفاده از DocumentFragment بدون نیاز

DocumentFragment یک ابزار برای مدیریت گروهی Nodeهاست، نه چیزی که باید در تمام DOM Manipulationها استفاده شود.

ابتدا مسئله را مشخص کنید؛ سپس در صورت نیاز از آن استفاده کنید.

Summary

تا اینجا از مسئله ایجاد یک Element جدید شروع کردیم.

createElement() یک Node جدید ایجاد می‌کند، اما آن را به DOM متصل نمی‌کند.

پس از ایجاد Node می‌توانیم Content و سایر ویژگی‌های آن را تنظیم کنیم و سپس آن را با:

append()
prepend()

به Childهای یک Element اضافه کنیم.

اگر جایگاه Node باید نسبت به یک Element مشخص باشد، از:

before()
after()

استفاده می‌کنیم.

وقتی Node دیگر موردنیاز نیست، می‌توانیم آن را با:

remove()

از DOM خارج کنیم.

ترکیب این عملیات امکان ایجاد Dynamic Rendering را فراهم می‌کند؛ یعنی UI می‌تواند در زمان اجرای Application بر اساس Data ساخته و تغییر داده شود.

وقتی تعداد Nodeهای موردنیاز زیاد می‌شود، DocumentFragment می‌تواند به‌عنوان یک Container موقت برای آماده‌سازی گروهی Nodeها مورد استفاده قرار گیرد.

Key Takeaways
createElement() یک DOM Element جدید ایجاد می‌کند.
Creation به‌تنهایی باعث ورود Node به DOM نمی‌شود.
Node می‌تواند قبل از Insert شدن Configure شود.
append() Node را در انتهای Childها قرار می‌دهد.
prepend() Node را در ابتدای Childها قرار می‌دهد.
before() Node را قبل از یک Element قرار می‌دهد.
after() Node را بعد از یک Element قرار می‌دهد.
remove() Element را از DOM خارج می‌کند.
Insert کردن یک Node موجود باعث Clone شدن آن نمی‌شود؛ Node جابه‌جا می‌شود.
Dynamic Rendering می‌تواند Data را به DOM Structure تبدیل کند.
DocumentFragment یک Container موقت برای گروه‌بندی Nodeهاست.
انتخاب API باید بر اساس جایگاه و نقش Node در DOM Tree انجام شود.
Technical Interview
Junior
1. createElement() چه کاری انجام می‌دهد؟

یک DOM Element جدید ایجاد می‌کند، اما آن را خودکار وارد Document نمی‌کند.

2. تفاوت append() و prepend() چیست؟

append() Node را به انتهای Childها و prepend() آن را به ابتدای Childهای Element اضافه می‌کند.

3. چگونه یک Element را از DOM حذف می‌کنیم؟

با remove():

element.remove();
4. تفاوت before() و after() چیست؟

before() Node را قبل از یک Element و after() آن را بعد از همان Element قرار می‌دهد.

Mid-Level
5. تفاوت Creation و Insertion در DOM چیست؟

Creation ایجاد یک Node جدید است؛ Insertion قرار دادن آن Node در ساختار DOM است. createElement() برای Creation و متدهایی مانند append() برای Insertion استفاده می‌شوند.

6. اگر یک Node موجود را با append() به Parent دیگری اضافه کنیم چه اتفاقی می‌افتد؟

Node موجود جابه‌جا می‌شود و در Parent جدید قرار می‌گیرد؛ Clone جدیدی ایجاد نمی‌شود.

7. DocumentFragment چه مسئله‌ای را حل می‌کند؟

به ما اجازه می‌دهد چند Node را ابتدا در یک Container موقت آماده کنیم و سپس مجموعه آن‌ها را در محل موردنظر DOM قرار دهیم.

Senior
8. Dynamic Rendering چگونه با DOM Creation ارتباط دارد؟

در Dynamic Rendering، Application Data را به یک Representation قابل نمایش تبدیل می‌کند. JavaScript بر اساس آن Data، Nodeهای DOM را ایجاد و Configure می‌کند و سپس آن‌ها را در ساختار DOM قرار می‌دهد.

9. چرا Creation و Insertion به‌عنوان دو مرحله جدا طراحی شده‌اند؟

زیرا Application ممکن است بخواهد Node را ابتدا ایجاد و کامل کند و تنها پس از آماده شدن، آن را به Document متصل کند. این جداسازی کنترل بیشتری بر ساخت و تغییر DOM فراهم می‌کند.

10. نقش DocumentFragment در DOM Manipulation چیست؟

DocumentFragment یک Container موقت برای گروهی از Nodeهاست. می‌توان Nodeها را ابتدا در آن آماده کرد و سپس Fragment را در DOM قرار داد. بنابراین برای سناریوهایی که تعداد زیادی Node باید به‌صورت گروهی آماده و Insert شوند، ابزار مناسبی است.

Golden Answers
Junior — Golden Answer

چگونه یک Element جدید را در DOM ایجاد می‌کنیم؟

ابتدا با document.createElement() آن را ایجاد می‌کنیم، سپس Content و ویژگی‌های موردنیاز را تنظیم کرده و با متدهایی مانند append() یا prepend() آن را وارد DOM می‌کنیم.

createElement
↓
Configure
↓
Insert
Mid-Level — Golden Answer

تفاوت append()، prepend()، before() و after() چیست؟

این متدها همگی برای قرار دادن Node در DOM استفاده می‌شوند، اما جایگاه متفاوتی را مشخص می‌کنند. append() و prepend() Node را به‌ترتیب در انتها و ابتدای Childهای یک Element قرار می‌دهند. before() و after() نیز Node را نسبت به یک Element مشخص، قبل یا بعد از آن قرار می‌دهند.

Senior — Golden Answer

Dynamic Rendering در DOM چگونه انجام می‌شود؟

Dynamic Rendering فرآیندی است که در آن Application بر اساس Data یا وضعیت جاری، DOM Structure موردنیاز را ایجاد یا تغییر می‌دهد. JavaScript می‌تواند Nodeها را ایجاد و Configure کند، آن‌ها را در DOM قرار دهد، Nodeهای قبلی را حذف یا جایگزین کند و در صورت نیاز چند Node را با استفاده از DocumentFragment به‌صورت گروهی آماده کند.

در نتیجه می‌توان این فرآیند را چنین خلاصه کرد:

Data
↓
DOM Representation
↓
Create / Modify
↓
Insert / Remove
↓
Rendered UI
Conclusion

در ابتدای فصل با یک نیاز ساده روبه‌رو شدیم:

اگر Element موردنیاز UI در DOM وجود نداشته باشد، چگونه آن را ایجاد کنیم؟

createElement() پاسخ این نیاز است.

اما ایجاد Node پایان کار نیست.

Node باید آماده شود و سپس در جای مناسب DOM قرار گیرد:

createElement
↓
Configure
↓
append / prepend
↓
before / after

از طرف دیگر، UI Dynamic به حذف Nodeها نیز نیاز دارد:

remove

ترکیب این عملیات به JavaScript اجازه می‌دهد ساختار DOM را بر اساس Data و وضعیت Application در زمان اجرا تغییر دهد.

در نتیجه:

DOM
↓
createElement
↓
Content
↓
append / prepend
↓
before / after
↓
remove
↓
Dynamic Rendering
↓
DocumentFragment

این مسیر، پایه‌ای برای فهم Dynamic UI در Browser است.

اما ایجاد و تغییر DOM به‌تنهایی کافی نیست. یک UI واقعی باید بتواند به عمل کاربر نیز واکنش نشان دهد.

این مسئله ما را به مفهوم بعدی می‌رساند:

Browser چگونه متوجه می‌شود کاربر چه کاری انجام داده است؟

پاسخ این سؤال، موضوع فصل بعد یعنی Events and Event Handling است.