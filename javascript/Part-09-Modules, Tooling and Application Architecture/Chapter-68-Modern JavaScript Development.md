Chapter 68 — Modern JavaScript Development
اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

قابلیت‌های Modern JavaScript را به‌عنوان ابزارهای حل مسئله تحلیل کنید.
توضیح دهید چرا Syntaxهای جدید زمانی ارزشمند هستند که یک مسئله واقعی را ساده‌تر کنند.
let و const را در طراحی کد مدرن به‌درستی به‌کار ببرید.
از Arrow Function در موقعیت مناسب استفاده کنید.
Template Literal را برای ساخت Stringهای پویا به‌کار بگیرید.
داده‌ها را با Destructuring خوانا استخراج کنید.
برای ورودی‌های اختیاری Function از Default Parameters استفاده کنید.
با Rest و Spread داده‌ها را جمع‌آوری یا گسترش دهید.
Objectهای Application را با Enhanced Object Literals خواناتر بسازید.
با Optional Chaining به داده‌های تو‌در‌تو به‌صورت ایمن دسترسی پیدا کنید.
تفاوت Optional Chaining و Nullish Coalescing را در طراحی کد تشخیص دهید.
چند قابلیت Modern JavaScript را در یک Pattern واقعی ترکیب کنید.
تشخیص دهید چه زمانی استفاده از یک Syntax جدید واقعاً خوانایی و Maintainability را افزایش می‌دهد.
Core Question

JavaScript مدرن چگونه خوانایی، انعطاف‌پذیری و Maintainability کد را افزایش می‌دهد؟

جریان این فصل:

Modern JavaScript
↓
let / const
↓
Arrow Functions
↓
Template Literals
↓
Destructuring
↓
Default Parameters
↓
Rest / Spread
↓
Enhanced Object Literals
↓
Optional Chaining
↓
Nullish Coalescing
↓
Practical Patterns

این فصل یک Recap کاربردی است.

بنابراین قرار نیست let، Arrow Function، Destructuring یا سایر قابلیت‌ها را برای اولین بار آموزش دهیم. این مفاهیم در فصل‌های قبلی به‌صورت مستقل بررسی شده‌اند. هدف این فصل این است که آن‌ها را از حالت Syntaxهای جداگانه خارج کنیم و در قالب ابزارهای حل مسئله در کنار یکدیگر قرار دهیم. Roadmap نیز همین نقش را برای این فصل تعیین کرده است.

مقدمه

در طول فصل‌های قبلی، با مجموعه‌ای از قابلیت‌های JavaScript آشنا شدیم.

const و let را برای مدیریت Variableها دیدیم.

Arrow Function را به‌عنوان شکل دیگری از تعریف Function شناختیم.

Template Literal را برای ساخت متن‌های پویا بررسی کردیم.

Destructuring امکان استخراج مستقیم داده از Array و Object را فراهم کرد.

Default Parameters طراحی ورودی Functionها را انعطاف‌پذیرتر کردند.

Rest و Spread امکان جمع‌آوری و گسترش داده‌ها را فراهم کردند.

Enhanced Object Literals ساخت Objectهای مدرن را خواناتر کردند.

Optional Chaining دسترسی ایمن به داده‌های تو‌در‌تو را ممکن کرد.

و Nullish Coalescing برای تعیین مقدار جایگزین در صورت وجود null یا undefined به کار رفت.

اگر هرکدام از این قابلیت‌ها را جداگانه ببینیم، ممکن است فقط مجموعه‌ای از Syntaxهای جدید به نظر برسند.

اما در یک Application واقعی، معمولاً این Syntaxها جدا از یکدیگر استفاده نمی‌شوند.

برای مثال، یک Function ممکن است:

با const تعریف شود،
یک Arrow Function باشد،
یک Object را به‌عنوان ورودی دریافت کند،
با Destructuring داده‌های آن Object را استخراج کند،
برای یکی از ورودی‌ها Default Value داشته باشد،
با Template Literal یک متن تولید کند،
با Optional Chaining به داده‌ای تو‌در‌تو دسترسی پیدا کند،
و با Nullish Coalescing مقدار پیش‌فرض مناسبی تعیین کند.

در چنین شرایطی، ارزش اصلی Modern JavaScript در یک Syntax خاص نیست.

ارزش آن در ترکیب چند قابلیت برای کاهش پیچیدگی کد است.

پس سؤال این فصل این نیست که:

هر Syntax چگونه نوشته می‌شود؟

بلکه سؤال مهم‌تر این است:

چگونه قابلیت‌های Modern JavaScript را برای ساخت کدی خواناتر و قابل نگهداری‌تر ترکیب کنیم؟

از Syntax به ابزار حل مسئله

وقتی یک قابلیت جدید وارد زبان می‌شود، ممکن است ابتدا فقط یک شکل کوتاه‌تر برای نوشتن کد به نظر برسد.

اما یک Syntax زمانی اهمیت مهندسی پیدا می‌کند که بتواند یکی از مشکلات کد را کاهش دهد.

برای مثال:

const userName = user.name;
const userRole = user.role;

این کد کاملاً معتبر است.

اما اگر هدف ما استخراج چند Property از یک Object باشد، Destructuring می‌تواند همین Intent را مستقیم‌تر بیان کند:

const { name, role } = user;

در اینجا Destructuring صرفاً تعداد کاراکترهای کد را کاهش نداده است.

کد دوم مستقیماً می‌گوید:

از Object مربوط به user، Propertyهای name و role را استخراج کن.

این همان تفاوت میان Syntax کوتاه‌تر و کد خواناتر است.

Modern JavaScript زمانی ارزشمند است که Syntax بتواند Intent برنامه را واضح‌تر بیان کند.

let و const: مدیریت دقیق‌تر Bindingها

در فصل مربوط به Variable Declaration دیدیم که let و const برای تعریف Bindingها استفاده می‌شوند.

در کد مدرن، انتخاب میان آن‌ها بخشی از بیان Intent برنامه است.

فرض کنید مقدار یک Variable قرار نیست دوباره Assignment شود:

const user = getUser();

استفاده از const به خواننده می‌گوید که Binding مربوط به user قرار نیست دوباره به Value دیگری اشاره کند.

اگر Binding باید در ادامه تغییر کند:

let currentPage = 1;

currentPage = 2;

استفاده از let این تغییر را به‌صورت صریح نشان می‌دهد.

پس انتخاب میان const و let فقط یک انتخاب Syntaxی نیست.

این انتخاب اطلاعاتی درباره رفتار مورد انتظار برنامه در اختیار خواننده قرار می‌دهد.

در نتیجه، یک Pattern رایج در کد مدرن این است:

const
↓
وقتی Reassignment نداریم

let
↓
وقتی Reassignment لازم است

این رویکرد باعث می‌شود Mutationهای احتمالی در کد واضح‌تر باشند.

در مقابل، استفاده از let برای تمام Variableها اطلاعات کمتری درباره Intent برنامه منتقل می‌کند.

بنابراین Modern JavaScript از اینجا یک اصل ساده به ما می‌دهد:

تا زمانی که Reassignment لازم نیست، Binding را با const تعریف کنید.

این موضوع درباره Mutation داخلی Object یا Array نیست.

برای مثال:

const user = {
name: 'Omid'
};

user.name = 'Ali';

در اینجا Binding مربوط به user دوباره Assignment نشده است؛ بلکه State داخل Object تغییر کرده است.

بنابراین const به معنای Immutable بودن خود Object نیست.

این تفاوت برای درک رفتار کدهای مدرن ضروری است.

Arrow Functions: بیان مستقیم‌تر Logic

Function یکی از اصلی‌ترین ابزارهای JavaScript برای سازمان‌دهی Logic است.

در فصل‌های قبلی دیدیم که Function را می‌توان به شکل‌های مختلف تعریف کرد.

Arrow Function یکی از Syntaxهای مدرن برای بیان Function است:

const formatPrice = price => `${price} USD`;

این Syntax زمانی ارزش خود را نشان می‌دهد که Function کوچک و مشخص باشد.

برای مثال، اگر یک Function فقط یک Value را تبدیل کند:

const double = number => number * 2;

Intent بسیار مستقیم است:

input
↓
transformation
↓
output

این ویژگی به‌خصوص در Callbackها اهمیت پیدا می‌کند.

برای مثال:

const prices = [100, 200, 300];

const formattedPrices = prices.map(
price => `${price} USD`
);

در اینجا Arrow Function بخشی از یک Pattern بزرگ‌تر است.

Array شامل Data است.

map() مسئول Iteration و Transformation است.

Arrow Function Logic مربوط به Transformation را بیان می‌کند.

بنابراین Arrow Function به‌تنهایی هدف نیست.

هدف، جدا کردن Logic از Mechanism است.

map()
↓
Mechanism

price => ...
↓
Transformation Logic

این تفکیک باعث می‌شود کد خواناتر شود.

البته Arrow Function همیشه انتخاب مناسب نیست.

در فصل this دیدیم که Arrow Function this مخصوص خود را ایجاد نمی‌کند.

بنابراین انتخاب Arrow Function باید بر اساس رفتار مورد نیاز Function انجام شود، نه صرفاً به دلیل کوتاه‌تر بودن Syntax.

Template Literals: نزدیک کردن Data به متن

در Applicationهای واقعی، متن‌های ثابت معمولاً کافی نیستند.

اغلب لازم است Data را داخل یک Message، Label، URL یا Output قرار دهیم.

برای مثال:

const name = 'Omid';
const role = 'admin';

const message = `User ${name} has the role ${role}.`;

Template Literal این رابطه میان متن و Data را مستقیم نشان می‌دهد.

در روش قدیمی‌تر ممکن بود از Concatenation استفاده کنیم:

const message =
'User ' + name + ' has the role ' + role + '.';

هر دو روش می‌توانند نتیجه یکسانی تولید کنند.

اما Template Literal ساختار نهایی متن را به شکل نزدیک‌تری به خروجی مورد نظر نشان می‌دهد.

این موضوع در تولید URL نیز مفید است:

const recipeId = 42;

const url = `/recipes/${recipeId}`;

در اینجا Template Literal فقط یک Syntax جدید نیست.

آنچه مهم است این است که رابطه میان Static Text و Dynamic Data در یک Expression مشخص شده است.

Destructuring: استخراج Data بر اساس Intent

در Applicationهای واقعی، Functionها اغلب Object دریافت می‌کنند.

فرض کنید اطلاعات یک Recipe به Function داده شده است:

const recipe = {
title: 'Pasta',
publisher: 'Jonas',
servings: 4
};

اگر فقط title و publisher مورد نیاز باشد، می‌توانیم بنویسیم:

const { title, publisher } = recipe;

اکنون Function یا Logic ما مستقیماً با داده‌های مورد نیاز کار می‌کند.

این موضوع در Parameters نیز بسیار کاربردی است:

const displayRecipe = ({ title, publisher }) => {
return `${title} by ${publisher}`;
};

در اینجا چند Concept قبلی در یک Pattern ترکیب شده‌اند:

Object
↓
Destructuring
↓
Function Parameter
↓
Arrow Function
↓
Template Literal

این دقیقاً همان نقطه‌ای است که Modern JavaScript از مجموعه‌ای از Syntaxها به یک روش بیان Logic تبدیل می‌شود.

Function مشخص نمی‌کند که ابتدا Object را دریافت کند و سپس Propertyها را یکی‌یکی استخراج کند.

از همان ابتدا می‌گوید:

من به title و publisher نیاز دارم.

این موضوع به خوانایی Interface Function کمک می‌کند.

Default Parameters: تعریف رفتار پیش‌فرض

گاهی یک Function می‌تواند بدون دریافت یک Argument خاص نیز رفتار معناداری داشته باشد.

فرض کنید Function زیر تعداد صفحه را دریافت می‌کند:

const getPageLabel = (page = 1) => {
return `Page ${page}`;
};

اکنون:

getPageLabel();

می‌تواند از مقدار پیش‌فرض 1 استفاده کند.

در اینجا Default Parameter یک مسئله مشخص را حل می‌کند:

اگر Caller مقدار این Parameter را ارسال نکرد، Function چه Valueای داشته باشد؟

این تصمیم را می‌توان در Interface خود Function قرار داد.

به همین دلیل Default Parameter فقط Syntax کوتاه‌تر نیست.

آن بخشی از Contract Function است که رفتار پیش‌فرض را تعریف می‌کند.

ترکیب Destructuring و Default Parameters

ارزش واقعی این قابلیت‌ها زمانی بیشتر مشخص می‌شود که در کنار یکدیگر استفاده شوند.

فرض کنید یک Function اطلاعات Search را دریافت می‌کند:

const searchRecipes = ({
query,
page = 1,
limit = 10
}) => {
// ...
};

در یک Definition کوتاه، سه تصمیم مشخص شده است:

query از Object استخراج می‌شود.
page مقدار پیش‌فرض 1 دارد.
limit مقدار پیش‌فرض 10 دارد.

این Syntax زمانی مفید است که Object واقعاً نماینده یک مجموعه ورودی مرتبط باشد.

اگر Function فقط یک Value ساده دریافت می‌کند، اضافه کردن Object و Destructuring صرفاً برای استفاده از Syntax مدرن، کد را بهتر نمی‌کند.

بنابراین یک اصل مهندسی مهم شکل می‌گیرد:

Modern Syntax باید پیچیدگی واقعی را کاهش دهد؛ نه اینکه پیچیدگی جدیدی برای استفاده از Syntax ایجاد کند.

Rest و Spread: دو نیاز متفاوت

Rest و Spread از یک Syntax مشابه ... استفاده می‌کنند، اما مسئله یکسانی را حل نمی‌کنند.

در Spread، یک Collection یا Object را گسترش می‌دهیم.

در Rest، چند Value را جمع‌آوری می‌کنیم.

این تفاوت را باید بر اساس موقعیت ... درک کرد.

برای مثال، در Function:

const logValues = (...values) => {
console.log(values);
};

...values نقش Rest دارد.

Argumentهای متعدد را جمع‌آوری می‌کند و آن‌ها را در یک Array قرار می‌دهد.

در مقابل:

const first = [1, 2];
const second = [3, 4];

const numbers = [...first, ...second];

اینجا Spread باعث گسترش Elementهای Array می‌شود.

پس:

Rest
↓
Collect

Spread
↓
Expand

این مدل ذهنی از حفظ کردن Syntax مهم‌تر است.

Spread و Objectهای Application

Spread در Objectها نیز برای ساخت Object جدید کاربرد زیادی دارد.

فرض کنید:

const user = {
name: 'Omid',
role: 'user'
};

اگر بخواهیم Object جدیدی بر اساس آن بسازیم و یک Property را تغییر دهیم:

const admin = {
...user,
role: 'admin'
};

در اینجا Object قبلی را مستقیماً Mutation نکرده‌ایم.

یک Object جدید ساخته‌ایم که Propertyهای Object قبلی را دریافت کرده و سپس role را با Value جدید جایگزین کرده است.

این Pattern در کدهای Application، مخصوصاً هنگام کار با State و Data Transformation، بسیار رایج است.

مدل ذهنی آن:

Existing Data
↓
Spread
↓
New Object
↓
Override / Add Properties

بنابراین Spread می‌تواند به بیان واضح‌تر یک Transformation کمک کند.

اما باید توجه داشت که Spread یک Deep Clone ایجاد نمی‌کند.

اگر Object شامل Objectهای تو‌در‌تو باشد، کپی سطح بالایی انجام می‌شود و Referenceهای داخلی می‌توانند همچنان مشترک باشند.

این نکته زمانی اهمیت پیدا می‌کند که با State پیچیده یا Nested Data کار می‌کنیم.

Enhanced Object Literals: ساخت Object بر اساس Data موجود

فرض کنید Variableهایی داریم:

const name = 'Omid';
const role = 'admin';

و می‌خواهیم Object بسازیم.

در JavaScript مدرن می‌توان نوشت:

const user = {
name,
role
};

JavaScript از نام Variableها به‌عنوان Property Name استفاده می‌کند.

این Property Shorthand باعث می‌شود وقتی Data و نام Property یکسان هستند، Intent مستقیم‌تر بیان شود.

همین ایده برای Methodها نیز وجود دارد:

const user = {
name,

login() {
// ...
}
};

در اینجا Object هم Data و هم Behavior را در یک ساختار مشخص قرار می‌دهد.

این قابلیت زمانی اهمیت بیشتری پیدا می‌کند که Objectها از چند منبع Data ساخته شوند.

برای مثال:

const createUser = (name, email) => ({
name,
email,

login() {
// ...
}
});

در اینجا چند قابلیت قبلی در یک ساختار طبیعی ترکیب شده‌اند.

Function مسئول ساخت Data است.

Object Literal مسئول مدل‌سازی Object است.

Property Shorthand از تکرار نام‌ها جلوگیری می‌کند.

Method Shorthand Behavior را مستقیماً در Object قرار می‌دهد.

این همان چیزی است که Enhanced Object Literals را از یک Syntax تزئینی جدا می‌کند.

Optional Chaining: وقتی ساختار Data قطعی نیست

تا اینجا فرض کردیم Property مورد نیاز ما وجود دارد.

اما در Application واقعی همیشه چنین تضمینی وجود ندارد.

فرض کنید اطلاعات User از یک API دریافت می‌شود و ممکن است profile وجود نداشته باشد:

const user = {
name: 'Omid'
};

اگر بنویسیم:

user.profile.avatar

در صورت نبودن profile، دسترسی به avatar باعث خطا می‌شود.

Optional Chaining این مسئله را به شکل کنترل‌شده‌تری مدیریت می‌کند:

user.profile?.avatar

معنای آن این است:

اگر profile وجود داشت، avatar را بررسی کن؛ در غیر این صورت مقدار undefined تولید کن.

این قابلیت مخصوصاً برای Dataهایی مفید است که از API یا منابع خارجی می‌آیند و ساختار آن‌ها همیشه کامل نیست.

برای مثال:

const avatarUrl = user.profile?.avatar;

اکنون Logic ما به جای Crash کردن در صورت نبودن profile، می‌تواند با undefined ادامه پیدا کند.

اما یک نکته مهم وجود دارد.

Optional Chaining به این معنا نیست که Data همیشه معتبر است.

فقط دسترسی را در نقطه مشخصی ایمن‌تر می‌کند.

Validation همچنان ممکن است لازم باشد.

بنابراین:

Optional Chaining
↓
Safe Access

نه:

Optional Chaining
↓
Data Validation

این دو مسئله یکسان نیستند.

Nullish Coalescing: وقتی مقدار جایگزین لازم است

Optional Chaining ممکن است نتیجه undefined ایجاد کند.

در اینجا سؤال بعدی طبیعی است:

اگر مقدار وجود نداشت، چه Valueای استفاده کنیم؟

برای این مسئله Nullish Coalescing وارد می‌شود:

const avatarUrl =
user.profile?.avatar ?? '/default-avatar.png';

اگر سمت چپ null یا undefined باشد، مقدار سمت راست استفاده می‌شود.

این رفتار با || یکسان نیست.

فرض کنید:

const count = 0;

اگر بنویسیم:

const result = count || 10;

چون 0 یک Falsy Value است، مقدار 10 انتخاب می‌شود.

اما:

const result = count ?? 10;

مقدار 0 حفظ می‌شود، زیرا 0 نه null است و نه undefined.

این تفاوت در Applicationهایی که 0، false یا String خالی می‌توانند Valueهای معتبر باشند، اهمیت زیادی دارد.

پس مدل ذهنی مناسب این است:

||
↓
Fallback بر اساس Truthiness

??
↓
Fallback فقط برای null / undefined

در نتیجه انتخاب میان این دو Operator باید بر اساس معنی Data انجام شود.

Optional Chaining و Nullish Coalescing در کنار یکدیگر

این دو قابلیت اغلب به‌صورت طبیعی کنار یکدیگر قرار می‌گیرند.

فرض کنید API اطلاعات Recipe را برگردانده است:

const recipe = {
title: 'Pasta',
publisher: {
name: 'Jonas'
}
};

اگر بخواهیم نام Publisher را نمایش دهیم:

const publisher =
recipe.publisher?.name ?? 'Unknown';

دو مسئله در یک Expression حل شده است:

publisher وجود ندارد؟
↓
Optional Chaining
↓
undefined

undefined است؟
↓
Nullish Coalescing
↓
'Unknown'

اکنون می‌توانیم آن را با Template Literal ترکیب کنیم:

const label =
`${recipe.title} by ${recipe.publisher?.name ?? 'Unknown'}`;

در اینجا چند Concept پشت سر هم قرار گرفته‌اند:

Object
↓
Optional Chaining
↓
Nullish Coalescing
↓
Template Literal

این همان نوع ترکیبی است که این فصل قصد دارد آن را به یک مدل ذهنی تبدیل کند.

Modern JavaScript به‌عنوان یک سیستم ترکیبی

تا اینجا هر Concept یک مسئله مشخص را حل کرد:

const
↓
Control Reassignment

Arrow Function
↓
Concise Function Expression

Template Literal
↓
Dynamic Text

Destructuring
↓
Data Extraction

Default Parameters
↓
Default Input Behavior

Rest
↓
Collect Values

Spread
↓
Expand Values

Enhanced Object Literal
↓
Readable Object Construction

Optional Chaining
↓
Safe Access

Nullish Coalescing
↓
Nullish Fallback

اما در Application واقعی این مسائل معمولاً مستقل نیستند.

یک Function می‌تواند همزمان چند مورد از آن‌ها را نیاز داشته باشد.

برای مثال، فرض کنید یک Function مسئول ساخت Label مربوط به Recipe است:

const createRecipeLabel = ({
title,
publisher,
servings = 1
}) => {
const author = publisher?.name ?? 'Unknown';

return `${title} by ${author} — ${servings} servings`;
};

این مثال را نباید به‌عنوان یک Syntax جدید یاد گرفت.

باید آن را به‌عنوان ترکیب چند تصمیم قبلی تحلیل کرد:

Object Parameter
↓
Destructuring
↓
Default Parameter
↓
Optional Chaining
↓
Nullish Coalescing
↓
Template Literal
↓
String Output

هر بخش یک مسئولیت مشخص دارد.

Destructuring مشخص می‌کند Function به چه داده‌ای نیاز دارد.

Default Parameter رفتار پیش‌فرض را مشخص می‌کند.

Optional Chaining عدم قطعیت ساختار Object را مدیریت می‌کند.

Nullish Coalescing مقدار جایگزین را مشخص می‌کند.

Template Literal خروجی نهایی را می‌سازد.

این همان نقطه‌ای است که Modern JavaScript از «مجموعه‌ای از قابلیت‌ها» به زبان بیان Intent تبدیل می‌شود.

یک Pattern واقعی‌تر

فرض کنید Application یک User را از API دریافت می‌کند و می‌خواهیم یک Object مناسب برای UI بسازیم.

ورودی:

const apiUser = {
id: 42,
name: 'Omid',
profile: {
city: 'Baku'
}
};

می‌توانیم Function را این‌گونه طراحی کنیم:

const createUserViewModel = ({
id,
name,
profile
}) => ({
id,
name,
city: profile?.city ?? 'Unknown'
});

در این مثال:

Destructuring داده‌های مورد نیاز را استخراج می‌کند.
Enhanced Object Literal باعث می‌شود id و name مستقیماً وارد Object جدید شوند.
Optional Chaining دسترسی به profile.city را ایمن می‌کند.
Nullish Coalescing مقدار جایگزین تعیین می‌کند.

خروجی:

{
id: 42,
name: 'Omid',
city: 'Baku'
}

نکته مهم این است که هیچ‌کدام از این Syntaxها به‌تنهایی هدف اصلی نیستند.

هدف، تبدیل Data خام به Data مناسب برای مصرف بخش دیگری از Application است.

چه زمانی Modern Syntax واقعاً مفید است؟

اکنون باید از یک سوءبرداشت مهم جلوگیری کنیم.

Modern JavaScript به این معنا نیست که:

هرچه Syntax جدیدتر باشد، کد بهتر است.

ممکن است یک Syntax جدید در یک Context مشخص، کد را پیچیده‌تر کند.

برای مثال اگر Destructuring باعث شود خواننده مجبور شود ساختار پیچیده‌ای از Object را دنبال کند، استفاده از آن لزوماً خوانایی را افزایش نمی‌دهد.

همین موضوع درباره Arrow Function، Spread و سایر قابلیت‌ها نیز وجود دارد.

هدف اصلی باید این باشد:

Problem
↓
Appropriate Language Feature
↓
Clearer Intent
↓
Maintainable Code

نه:

Modern Syntax
↓
Use Everywhere

این تفاوت میان استفاده از قابلیت زبان و مهندسی کد با قابلیت زبان است.

Practical Patterns

اکنون می‌توانیم چند Pattern اصلی را که از ترکیب Conceptهای این فصل به وجود می‌آیند، به‌صورت یک مدل ذهنی جمع کنیم.

Pattern اول: دریافت Object و استخراج Data

وقتی Function با یک مجموعه داده مرتبط کار می‌کند:

const formatUser = ({ name, role }) => {
return `${name} — ${role}`;
};

رابطه:

Object Input
↓
Destructuring
↓
Function Logic
↓
Template Literal
Pattern دوم: Default Behavior

وقتی یک ورودی اختیاری است:

const getPage = (page = 1) => {
return page;
};

رابطه:

Optional Input
↓
Default Parameter
↓
Predictable Behavior
Pattern سوم: Safe Data Access

وقتی ساختار Data قطعی نیست:

const city =
user.profile?.address?.city ?? 'Unknown';

رابطه:

Nested Data
↓
Optional Chaining
↓
Possible undefined
↓
Nullish Coalescing
↓
Fallback
Pattern چهارم: ساخت Object جدید

وقتی Object جدیدی بر اساس Data موجود ایجاد می‌کنیم:

const updatedUser = {
...user,
role: 'admin'
};

رابطه:

Existing Object
↓
Spread
↓
New Object
↓
Override Property
Pattern پنجم: ترکیب چند قابلیت

در Application واقعی ممکن است چند Pattern در کنار هم قرار بگیرند:

const createCard = ({
title,
author,
image
}) => ({
title,
author: author?.name ?? 'Unknown',
image: image ?? '/placeholder.jpg'
});

این Function هم‌زمان:

Object را دریافت می‌کند.
Destructuring انجام می‌دهد.
Object جدید ایجاد می‌کند.
Optional Chaining استفاده می‌کند.
Nullish Coalescing برای Fallback دارد.

بنابراین خواننده می‌تواند Logic را از روی ساختار Syntax دنبال کند.

این همان هدف اصلی Modern JavaScript است:

Syntax باید ساختار فکر برنامه‌نویس را واضح‌تر کند.

Best Practices
1. const را انتخاب پیش‌فرض قرار دهید

اگر Reassignment لازم نیست، از const استفاده کنید.

2. Arrow Function را به‌دلیل رفتار مناسب آن انتخاب کنید، نه صرفاً کوتاهی Syntax

به‌خصوص زمانی که تفاوت رفتار this اهمیت دارد، انتخاب Function Syntax باید آگاهانه باشد.

3. Destructuring را برای بیان Data مورد نیاز استفاده کنید

اگر Function فقط به چند Property از یک Object نیاز دارد، Destructuring می‌تواند Interface آن را واضح‌تر کند.

4. Default Parameters را نزدیک به Contract Function قرار دهید

اگر مقدار پیش‌فرض بخشی از رفتار طبیعی Function است، آن را در Parameter Definition مشخص کنید.

5. Rest و Spread را از یکدیگر تفکیک کنید

مدل ذهنی ساده را حفظ کنید:

Rest → Collect
Spread → Expand
6. Optional Chaining را با Validation اشتباه نگیرید

Optional Chaining فقط دسترسی را در برابر null یا undefined ایمن‌تر می‌کند.

اعتبارسنجی Data مسئله دیگری است.

7. ?? را زمانی انتخاب کنید که فقط null و undefined باید Fallback ایجاد کنند

اگر 0، false یا '' Valueهای معتبر هستند، || ممکن است رفتار مورد انتظار را نداشته باشد.

8. Modern Syntax را فقط برای مدرن بودن استفاده نکنید

معیار اصلی باید:

Readability
+
Intent
+
Maintainability

باشد.

اشتباهات رایج
استفاده از let برای همه Variableها

این کار تفاوت میان Bindingهای قابل Reassignment و غیرقابل Reassignment را پنهان می‌کند.

تصور اینکه const Object را Immutable می‌کند

const از Reassignment خود Binding جلوگیری می‌کند، نه از Mutation داخلی Object.

استفاده از Arrow Function در تمام موقعیت‌ها

Arrow Function رفتار خاصی نسبت به this دارد.

بنابراین جایگزین مکانیکی تمام Functionهای معمولی نیست.

استفاده از || به‌جای ?? بدون توجه به نوع Data

این کار می‌تواند Valueهای معتبر مانند 0 یا false را با Fallback جایگزین کند.

استفاده بیش از حد از Destructuring

Destructuring باید خوانایی را افزایش دهد.

اگر ساختار بسیار پیچیده‌ای ایجاد کند، ممکن است خوانایی کاهش یابد.

تصور اینکه Spread یک Deep Clone است

Spread در Object و Array به‌طور کلی یک کپی سطحی ایجاد می‌کند.

Referenceهای Nested می‌توانند همچنان مشترک باشند.

استفاده از Optional Chaining برای پنهان کردن خطای Data

اگر یک Property باید حتماً وجود داشته باشد، Optional Chaining نباید صرفاً برای جلوگیری از مشاهده خطا استفاده شود.

گاهی نبودن Data باید صراحتاً مدیریت شود.

استفاده از Syntax جدید بدون مسئله واقعی

کدی که فقط برای استفاده از قابلیت جدید پیچیده‌تر شده باشد، الزاماً Modern یا Maintainable نیست.

Summary

Modern JavaScript مجموعه‌ای از Syntaxهای مستقل نیست.

این قابلیت‌ها ابزارهایی هستند که به برنامه‌نویس اجازه می‌دهند Intent خود را دقیق‌تر در کد بیان کند.

let و const به ما اجازه می‌دهند رفتار Bindingها را واضح‌تر مشخص کنیم.

Arrow Function شکل مناسبی برای بسیاری از Functionهای کوتاه و Callbackها فراهم می‌کند، البته با رفتار خاص خود در رابطه با this.

Template Literal رابطه میان متن ثابت و Data پویا را واضح می‌کند.

Destructuring داده‌های مورد نیاز را مستقیم استخراج می‌کند.

Default Parameters رفتار پیش‌فرض Function را در Interface آن تعریف می‌کنند.

Rest برای جمع‌آوری و Spread برای گسترش داده‌ها استفاده می‌شوند.

Enhanced Object Literals ساخت Objectهای مبتنی بر Data موجود را خواناتر می‌کنند.

Optional Chaining دسترسی به ساختارهای احتمالی و تو‌در‌تو را ایمن‌تر می‌کند.

Nullish Coalescing امکان تعریف Fallback برای null و undefined را فراهم می‌کند.

اما مهم‌ترین نکته این فصل ترکیب این قابلیت‌ها است.

کد مدرن زمانی ارزشمند است که:

Language Feature
↓
Solves a Real Problem
↓
Expresses Intent
↓
Improves Readability
↓
Improves Maintainability

بنابراین Modern JavaScript بیشتر از آنکه درباره Syntaxهای جدید باشد، درباره استفاده مهندسی از قابلیت‌های زبان است.

Key Takeaways
Modern JavaScript را نباید مجموعه‌ای از Syntaxهای جداگانه در نظر گرفت.
const انتخاب مناسب زمانی است که Binding نیاز به Reassignment ندارد.
let زمانی استفاده می‌شود که Reassignment مورد نیاز است.
Arrow Function فقط Syntax کوتاه‌تر نیست و رفتار خاصی در رابطه با this دارد.
Template Literal برای ترکیب متن و Data پویا مناسب است.
Destructuring Data مورد نیاز را مستقیم از Array یا Object استخراج می‌کند.
Default Parameters رفتار پیش‌فرض ورودی Function را مشخص می‌کنند.
Rest داده‌ها را جمع می‌کند.
Spread داده‌ها را گسترش می‌دهد.
Enhanced Object Literals ساخت Object را با Data موجود ساده‌تر می‌کنند.
Optional Chaining برای Safe Access به Dataهای احتمالی استفاده می‌شود.
Nullish Coalescing برای Fallback در برابر null و undefined است.
?? و || بر اساس یک منطق یکسان عمل نمی‌کنند.
Spread به‌طور معمول Deep Clone ایجاد نمی‌کند.
Modern Syntax نباید فقط به دلیل جدید بودن استفاده شود.
معیار اصلی انتخاب Syntax باید وضوح Intent و Maintainability باشد.
Technical Interview
Junior
1. چرا const در کدهای مدرن JavaScript معمولاً انتخاب مناسبی برای Variableها است؟

زیرا وقتی Binding قرار نیست دوباره Assignment شود، const این Intent را به‌صورت صریح بیان می‌کند و از Reassignment ناخواسته جلوگیری می‌کند.

2. تفاوت Rest و Spread چیست؟

Rest چند Value را جمع‌آوری می‌کند؛ Spread یک Collection یا Object را گسترش می‌دهد.

Rest → Collect
Spread → Expand
3. Template Literal چه مشکلی را حل می‌کند؟

ترکیب متن ثابت با Data پویا را خواناتر می‌کند و امکان Interpolation را فراهم می‌کند.

4. Optional Chaining چه کاری انجام می‌دهد؟

امکان دسترسی به Propertyهای تو‌در‌تو را فراهم می‌کند، بدون اینکه در صورت null یا undefined بودن بخش مشخص‌شده، همان دسترسی باعث خطا شود.

Mid-Level
5. تفاوت || و ?? چیست؟

|| بر اساس Truthiness تصمیم می‌گیرد، در حالی که ?? فقط زمانی از سمت راست استفاده می‌کند که مقدار سمت چپ null یا undefined باشد.

بنابراین:

0 || 10

نتیجه 10 است، اما:

0 ?? 10

نتیجه 0 است.

6. چرا Modern JavaScript فقط مجموعه‌ای از Syntaxهای کوتاه‌تر نیست؟

زیرا هدف اصلی این قابلیت‌ها کاهش پیچیدگی و بیان دقیق‌تر Intent است. ارزش یک Syntax زمانی مشخص می‌شود که یک مسئله واقعی را ساده‌تر و کد را قابل فهم‌تر کند.

7. آیا const باعث Immutable شدن Object می‌شود؟

خیر.

const از Reassignment خود Binding جلوگیری می‌کند، اما Properties داخلی Object همچنان می‌توانند تغییر کنند.

8. چرا Destructuring می‌تواند API یک Function را خواناتر کند؟

زیرا Function می‌تواند مستقیماً مشخص کند به کدام بخش‌های یک Object نیاز دارد:

const formatUser = ({ name, role }) => {
// ...
};

در نتیجه Interface Function بخشی از ساختار Data مورد نیاز را نشان می‌دهد.

Senior
9. چگونه می‌توان Modern JavaScript را بدون تبدیل آن به Syntax Overuse به‌کار گرفت؟

هر قابلیت باید بر اساس مسئله‌ای که حل می‌کند انتخاب شود.

برای مثال:

Need
↓
Choose Language Feature
↓
Express Intent
↓
Evaluate Readability
↓
Evaluate Maintainability

اگر Syntax جدید کد را پیچیده‌تر کند، صرفاً جدید بودن آن دلیل مناسبی برای استفاده نیست.

10. چرا ترکیب Optional Chaining و Nullish Coalescing یک Pattern مهم در Applicationهای واقعی است؟

زیرا Applicationها اغلب با Dataهایی کار می‌کنند که ساختار یا مقدار برخی بخش‌های آن قطعی نیست.

برای مثال:

const city =
user.profile?.address?.city ?? 'Unknown';

Optional Chaining مسئله Safe Access را حل می‌کند و Nullish Coalescing مسئله Fallback را.

11. آیا Spread برای ساخت یک نسخه مستقل از هر Object کافی است؟

خیر.

Spread معمولاً یک Shallow Copy ایجاد می‌کند. بنابراین اگر Object دارای Referenceهای تو‌در‌تو باشد، آن Referenceها می‌توانند میان Object اصلی و Object جدید مشترک باقی بمانند.

12. معیار مهندسی برای انتخاب بین Syntaxهای مختلف Modern JavaScript چیست؟

معیار اصلی باید این باشد که Syntax انتخاب‌شده تا چه اندازه:

Intent را واضح می‌کند.
پیچیدگی را کاهش می‌دهد.
خوانایی را افزایش می‌دهد.
رفتار مورد انتظار را دقیق‌تر بیان می‌کند.
Maintainability را بهبود می‌دهد.

Modern بودن Syntax به‌تنهایی معیار کافی نیست.

Golden Answers

Modern JavaScript چیست؟

Modern JavaScript مجموعه قابلیت‌هایی است که در نسخه‌های جدیدتر زبان برای بیان خواناتر و مؤثرتر Logic، Data و Behavior برنامه ارائه شده‌اند. ارزش آن‌ها در حل مسئله و بهبود Maintainability است، نه صرفاً کوتاه‌تر کردن Syntax.

تفاوت Rest و Spread چیست؟

Rest برای جمع‌آوری چند Value در یک ساختار استفاده می‌شود، در حالی که Spread برای گسترش Valueهای موجود در یک ساختار به‌کار می‌رود.

تفاوت || و ?? چیست؟

|| بر اساس Truthiness عمل می‌کند، اما ?? فقط برای null و undefined مقدار جایگزین را انتخاب می‌کند.

Optional Chaining چه مشکلی را حل می‌کند؟

Safe Access به Propertyهای احتمالی و تو‌در‌تو را فراهم می‌کند و در صورت null یا undefined بودن بخش مشخص‌شده، به‌جای ادامه دادن دسترسی نامعتبر، undefined تولید می‌کند.

چرا const به معنای Immutable بودن Object نیست؟

زیرا const فقط Reassignment Binding را محدود می‌کند. Object همچنان می‌تواند Mutation شود.

چه زمانی Modern Syntax ارزش مهندسی دارد؟

وقتی یک مسئله واقعی را ساده‌تر کند، Intent را واضح‌تر بیان کند و خوانایی و Maintainability کد را افزایش دهد.

Conclusion

در ابتدای این فصل، Modern JavaScript ممکن بود مجموعه‌ای از قابلیت‌های آشنا به نظر برسد:

let / const
Arrow Functions
Template Literals
Destructuring
Default Parameters
Rest / Spread
Enhanced Object Literals
Optional Chaining
Nullish Coalescing

اما اکنون می‌توانیم رابطه میان آن‌ها را ببینیم.

هر قابلیت برای پاسخ به یک نیاز مشخص وارد جریان می‌شود.

گاهی می‌خواهیم Reassignment را کنترل کنیم.

گاهی می‌خواهیم Function کوتاه و مشخصی بنویسیم.

گاهی باید Data را از یک Object استخراج کنیم.

گاهی ورودی اختیاری است.

گاهی باید داده‌ها را جمع یا گسترش دهیم.

گاهی Object جدیدی بر اساس Data موجود می‌سازیم.

گاهی ساختار Data قطعی نیست.

و گاهی برای چنین Dataای به یک مقدار جایگزین نیاز داریم.

بنابراین Modern JavaScript یک فهرست از Syntaxهای جدید نیست.

این قابلیت‌ها زمانی ارزش واقعی پیدا می‌کنند که بتوانیم آن‌ها را در پاسخ به نیازهای واقعی Application در کنار یکدیگر قرار دهیم.

اما حتی اگر تمام Logic یک Application را با Syntaxهای مدرن در یک Function بنویسیم، هنوز یک مسئله مهم باقی می‌ماند.

تا اینجا فرض کرده‌ایم تمام Logic و Data در یک فضای مشترک قرار دارند.

اما وقتی Application بزرگ‌تر می‌شود، قرار دادن همه چیز در یک Scope مشترک باعث برخورد نام‌ها، وابستگی‌های نامشخص و دشوار شدن مدیریت کد می‌شود.

در نتیجه سؤال طبیعی بعدی این است:

چگونه یک JavaScript Application بزرگ را به بخش‌های مستقل تقسیم کنیم تا هر بخش Boundary مشخصی داشته باشد؟

این سؤال ما را به مفهوم JavaScript Modules می‌رساند؛ موضوع فصل بعد.