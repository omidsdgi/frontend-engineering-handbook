# Chapter 30 — Inheritance and Polymorphism

Core Question

چگونه Classها رفتار مشترک را به ارث می‌برند و رفتار متفاوت ارائه می‌کنند؟

Inheritance → extends →Parent Class → Child Class → super → Method Overriding 

→ Polymorphism → Composition

مقدمه

در فصل 29 دیدیم که class ،Syntax خواناتری برای ساخت Objectهای مرتبط فراهم می‌کند و Instanceها می‌توانند State و Behavior داشته باشند. اما با یک مسئله جدید روبه‌رو می‌شویم.

فرض کنید در یک Application چند نوع User داریم. همه Userها ویژگی‌ها و رفتارهای مشترکی دارند، اما بعضی از آن‌ها قابلیت‌های مخصوص خود را نیز دارند. اگر برای هر نوع User یک Class کاملاً مستقل بسازیم، احتمالاً بخشی از Code را بارها تکرار خواهیم کرد. از طرف دیگر، اگر همه چیز را در یک Class بزرگ قرار دهیم، مسئولیت‌های متفاوت با یکدیگر مخلوط می‌شوند. پس سؤال این است:

اگر دو نوع Object بخش قابل توجهی از State و Behavior خود را مشترک داشته باشند، چگونه می‌توان این رابطه را در طراحی Code مدل کرد؟ اینجاست که Inheritance وارد می‌شود. 

Inheritance

Inheritance یعنی یک Class جدید بتواند ویژگی‌ها و رفتارهای مرتبط با یک Class موجود را بر پایه یک رابطه مشخص دریافت کند و در صورت نیاز آن‌ها را گسترش یا تغییر دهد. در این رابطه معمولاً یک Class را Parent Class و Class دیگر را Child Class می‌نامیم.

هدف اصلی این رابطه، فقط جلوگیری از تکرار چند خط Code نیست. Inheritance زمانی معنا پیدا می‌کند که بین دو نوع Object یک رابطه مفهومی واقعی وجود داشته باشد و Child واقعاً نوع تخصص‌یافته‌تری از Parent باشد. 

extends

در JavaScript، Class Inheritance با extends ایجاد می‌شود. وقتی یک Class با extends از Class دیگری مشتق می‌شود، یک رابطه میان آن‌ها ایجاد می‌شود که به JavaScript اجازه می‌دهد Behavior مربوط به Parent را از طریق زنجیره Prototype در اختیار Instanceهای Child قرار دهد.

اما باید میان State و Behavior تفاوت قائل شویم. در Inheritance، Child می‌تواند از Behavior موجود در Parent استفاده کند، در حالی که State مربوط به Instance از طریق Constructorها روی Instance ایجاد و مقداردهی می‌شود. در واقع، Behaviorهایی مانند Methodهای Instance ، می‌توانند از طریق Prototype Chain به Child Instanceها برسند. اما State مربوط به Instance معمولاً به‌عنوان Property روی خود Instance ایجاد و مقداردهی می‌شود. این تفاوت برای درک صحیح Inheritance بسیار مهم است

بنابراین JavaScript هنگام جست‌وجوی Property یا Method می‌تواند از Object به Prototype و سپس در زنجیره Prototype به Parent برسد. پس Inheritance در Class Syntax یک مفهوم جدا از Prototype نیست.

« class و extends ،Syntax خواناتری برای ساخت روابطی هستند که در نهایت با Prototypeها کار می‌کنند.»

Parent Class و Child Class

Instance دو دسته Behavior در اختیار دارد. Behaviorی که از Parent می‌آید و Behaviorی که در Child تعریف شده است . در نتیجه Child لزوماً جایگزین Parent نیست. Child می‌تواند:

Behavior موجود را استفاده کند. Behavior جدید اضافه کند. Behavior موجودرا Override کند.



super

وقتی یک Child Class از Parent Class ارث‌بری می‌کند، ممکن است Child علاوه بر State خودش، به Stateهایی نیاز داشته باشد که در Parent تعریف شده‌اند. اگر Child Constructor مخصوص خودش را داشته باشد، دیگر Initialization مربوط به Parent به‌صورت خودکار در همان Constructor انجام نمی‌شود. بنابراین Child باید راهی داشته باشد تا Initialization مربوط به Parent را نیز درخواست کند. برای این کار از super() استفاده می‌کنیم.

class BankAccount {  class PremiumAccount extends BankAccount {

constructor(owner, balance) {   constructor(owner, balance, cashbackRate) {

this.owner = owner;   super(owner, balance);

this.balance = balance } }   this .cashbackRate = cashbackRate }}

در اینجا PremiumAccount مسئول Initialization مربوط به State مخصوص خودش است، اما State تعریف‌شده در BankAccount را خودش دوباره مقداردهی نمی‌کند. super(owner, balance) ،Constructor مربوط به Parent را اجرا می‌کند و اجازه می‌دهد Parent مسئولیت Initialization مربوط به خودش را حفظ کند. بنابراین جریان ساخت Instance را می‌توان این‌گونه دید: new PremiumAccount(...) → PremiumAccount Constructor → super(...)

→ BankAccount Constructor → Parent State Initialization → PremiumAccount State Initialization

چرا super() باید قبل از this باشد؟

در یک Derived Class، اگر Constructor مخصوص Child را تعریف کنیم، Child باید Initialization مربوط به Parent را نیز انجام دهد. بنابراین نمی‌تواند قبل از انجام این Initialization از this استفاده کند.

در یک Derived Constructor، Instance باید ابتدا توسط Parent Constructor مقداردهی اولیه شود. فراخوانی super() همین مرحله را انجام می‌دهد. پس از آن، Child می‌تواند با this به همان Instance دسترسی پیدا کند و State مخصوص خودش را مقداردهی کند. بنابراین ترتیب Initialization چنین است:

Child Constructor → super(...) → Parent Initialization → this → Child Initialization

به همین دلیل در یک Derived Constructor، super() فقط برای اجرای Constructor والد نیست؛ مرحله‌ای است که Initialization مربوط به Parent را انجام می‌دهد و پس از آن استفاده از this در Child مجاز می‌شود.



استفاده از super برای Parent Method

super فقط برای Constructor استفاده نمی‌شود. Child می‌تواند Method مربوط به Parent را نیز با super فراخوانی کند. مثلاً: class PremiumAccount extends Account {   class Account {

deposit(amount) {   deposit(amount) {

this.balance += amount } }   super .deposit(amount);

 console.log('Premium reward applied') } }

اکنون: account .deposit(1000) ابتدا Behavior مربوط به Parent اجرا می‌شود: super .deposit(amount) 

و سپس Child Behavior خودش را اضافه می‌کند: console.log('Premium reward applied')

بنابراین super در اینجا به معنی: استفاده از Implementation مربوط به Parent است ، در حالی که همچنان روی همان Child Instance کار می‌کنیم.

نکته مهم این است که super .deposit() باعث نمی‌شود this به Parent تبدیل شود. بلکه this همچنان مربوط به Instance فعلی است.

Overriding 

در یک سلسله‌مراتب ارث‌بری، ممکن است یک Behavior در Parent تعریف شده باشد و همه Childها نیز به آن Behavior نیاز داشته باشند. اما نیاز داشتن به یک Behavior مشترک، لزوماً به معنای یکسان بودن نحوه اجرای آن نیست. هر Child ممکن است برای انجام همان مسئولیت، منطق متفاوتی نیاز داشته باشد.

برای مثال، Parent می‌تواند یک Method عمومی را تعریف کند که بیانگر یک مسئولیت مشترک میان همه Childهاست. اما وقتی همین Method در یک Child اجرا می‌شود، ممکن است آن Child به جزئیات متفاوتی برای انجام آن مسئولیت نیاز داشته باشد. در این حالت، استفاده از همان پیاده‌سازی Parent دیگر مناسب نیست. Child باید بتواند رفتار مخصوص خودش را ارائه کند، بدون آنکه رابطه ارث‌بری و Behavior مشترک میان آن‌ها از بین برود.

JavaScript برای این وضعیت امکان Overriding را فراهم می‌کند. در Overriding، Child متدی را با همان نام Method موجود در Parent تعریف می‌کند و به این ترتیب، نسخه مخصوص خودش از آن Behavior را در اختیار می‌گیرد.

class Account {  class PremiumAccount extends Account {

deposit(amount) {   deposit(amount) {

this.balance += amount } }   console.log('Premium reward applied') } }

در این ساختار، extends رابطه ارث‌بری را ایجاد می‌کند و تعریف دوباره Method در Child، رفتار آن Method را برای Child تغییر می‌دهد. JavaScript برای این کار Keyword جداگانه‌ای مانند override ندارد. تشخیص این وضعیت بر اساس ساختار Prototype Chain و وجود Property همنام انجام می‌شود.

در این مثال، PremiumAccount متد ()deposit را Override کرده است. بنابراین هنگامی که ()deposit از یک Instance از PremiumAccount فراخوانی شود، نسخه تعریف‌شده در PremiumAccount اجرا خواهد شد و نسخه موجود در Account به‌صورت خودکار اجرا نمی‌شود.

گاهی Child نمی‌خواهد رفتار Parent را کاملاً کنار بگذارد. ممکن است منطق Parent همچنان برای Child مناسب باشد و Child فقط بخواهد Behavior دیگری به آن اضافه کند. در چنین شرایطی می‌توان نسخه Parent را با super فراخوانی کرد: class PremiumAccount extends Account {

deposit(amount) {

super .deposit(amount);

console.log('Premium reward applied') } }

در اینجا deposit() در PremiumAccount، Method موجود در Account را Override کرده است؛ اما قبل از اجرای Behavior مخصوص خود، نسخه Parent را نیز با super .deposit(amount) فراخوانی می‌کند. بنابراین Child می‌تواند Behavior Parent را حفظ کرده و آن را با منطق مخصوص خودش گسترش دهد.

در نتیجه، Overriding به Child اجازه می‌دهد یک مسئولیت مشترک را حفظ کند، اما نحوه اجرای آن مسئولیت را متناسب با نیاز خودش تغییر دهد. Parent در این ساختار بیشتر بیان‌کننده Behavior مشترک است، در حالی که Child می‌تواند جزئیات اجرای آن Behavior را تعیین کند. 

Polymorphism

تا اینجا دیدیم که در یک سلسله ‌مراتب ارث‌بری، Parent می‌تواند یک Behavior مشترک را تعریف کند و Child نیز در صورت نیاز، همان Behavior را با پیاده‌سازی متفاوتی Override کند. اما در اینجا یک سؤال مهم‌تر مطرح می‌شود: اگر چند Object مختلف یک Method با نام یکسان داشته باشند، چگونه می‌توانیم با همه آن‌ها به یک شکل کار کنیم، در حالی که هرکدام رفتار متناسب با نوع خود را اجرا کنند؟ پاسخ به این سؤال ما را به مفهوم Polymorphism می‌رساند.

واژه Polymorphism به معنای «چندشکلی» است؛ اما در برنامه‌نویسی، مفهوم آن چیزی فراتر از یک نام یا ویژگی ظاهری است. Polymorphism یعنی بتوانیم با Objectهای متفاوت، از طریق یک Behavior مشترک کار کنیم، در حالی که هر Object می‌تواند پیاده‌سازی مخصوص خودش را برای آن Behavior ارائه دهد.

برای درک این مفهوم، فرض کنید چند نوع مختلف از Notification داریم. همه آن‌ها مسئولیت ارسال یک پیام را بر عهده دارند، اما نحوه انجام این مسئولیت برای هر نوع Notification متفاوت است.

class Notification {  class EmailNotification extends Notification {

send(message) {  send(message) {

console.log(`Sending notification: ${message}`) } }  console.log(`Sending Email: ${message}`) } }

class SMSNotification extends Notification {  class PushNotification extends Notification {

send(message) {  send(message) {

console.log(`Sending SMS: ${message}`); } } console.log(`SendingPushNotification:${message}`)}}

در این ساختار، Notification یک Behavior عمومی به نام send() را تعریف می‌کند. هر Child نیز همین Method را Override کرده و نحوه اجرای مخصوص خودش را مشخص می‌کند. اکنون می‌توانیم Functionای بنویسیم که Notification را ارسال کند: function sendNotification(notification, message) {

notification .send(message)}

نکته مهم این Function این است که هیچ اطلاعی از نوع دقیق notification ندارد. این Function نمی‌داند Object دریافت‌شده از نوع EmailNotification، SMSNotification یا PushNotification است. تنها چیزی که برای آن اهمیت دارد این است که Object موردنظر Behaviorای به نام send() ارائه می‌کند.

اکنون می‌توانیم Notificationهای مختلف را به همین Function بدهیم:

sendNotification(new EmailNotification(), 'Hello')

sendNotification(new SMSNotification(), 'Hello')

sendNotification(new PushNotification(), 'Hello')

در هر سه حالت، Function دقیقاً یک Code دارد: notification .send(message)

اما نتیجه متفاوت است. برای EmailNotification، پیاده‌سازی مربوط به Email اجرا می‌شود؛ برای SMSNotification، پیاده‌سازی مربوط به SMS؛ و برای PushNotification، پیاده‌سازی مربوط به Push.

اینجاست که رفتار Polymorphic شکل می‌گیرد:

یک نقطه استفاده، یک Behavior مشترک و چند Implementation متفاوت.

تعریف ساده Polymorphism 

Polymorphism یعنی بتوانیم Objectهای متفاوت را از طریق یک Behavior مشترک مورد استفاده قرار دهیم، در حالی که هر Object پیاده‌سازی مخصوص خودش را برای آن Behavior ارائه می‌دهد.

در مثال ما: notification .send(message) Behavior مشترک است. اما Implementation می‌تواند متفاوت باشد:

EmailNotification → send email   SMSNotification → send SMS 

بنابراین، Code مصرف‌کننده لازم نیست نوع دقیق Object را بشناسد. این نکته بخش مهمی از مفهوم Polymorphism است. مسئله فقط این نیست که چند Object یک Method همنام دارند؛ مسئله این است که کدی که از آن‌ها استفاده می‌کند، می‌تواند بدون وابستگی به نوع دقیق آن‌ها، همان Behavior را درخواست کند.

تعریف فنی Polymorphism

در این الگو، بخشی از برنامه بر اساس یک Behavioral Contract مشترک با Object کار می‌کند . Objectهای مختلف Operation مورد انتظار را ارائه می‌کنند، اما هرکدام می‌توانند Implementation متفاوتی داشته باشند.

JavaScript برای این کار به تعریف رسمی interface مانند برخی زبان‌های دیگر نیاز ندارد. در این مثال، Function فقط انتظار دارد Object دریافت‌شده بتواند Method زیر را اجرا کند: notification .send(message);

بنابراین Contract مورد انتظار، در ساده‌ترین شکل، این است که Object یک send() قابل فراخوانی داشته باشد. این موضوع با ماهیت Dynamic و Object-Based زبان JavaScript سازگار است. Function لزوماً از قبل نمی‌خواهد بداند Object دقیقاً متعلق به چه Classای است؛ بلکه در زمان اجرا با Object دریافت‌شده کار می‌کند و Behavior موردنیاز خود را از آن درخواست می‌کند. 

چرا Polymorphism یک مدل ذهنی مهم ایجاد می‌کند؟

اهمیت Polymorphism زمانی آشکار می‌شود که به جای تمرکز بر نوع دقیق Object، بر مسئولیتی که Object باید انجام دهد تمرکز کنیم. بدون Polymorphism ممکن است Function مرکزی را بر اساس نوع Object طراحی کنیم:

function deliver(notification) {

if (notification .type === 'email') {

// send email }

if (notification .type === 'sms') {

// send SMS } }

در این طراحی، deliver() باید انواع مختلف Notification را بشناسد. بنابراین هر بار که نوع جدیدی از Notification به برنامه اضافه شود، ممکن است لازم باشد همین Function را نیز تغییر دهیم. برای مثال، اگر بعداً PushNotification اضافه شود، منطق مرکزی باید از نوع جدید نیز اطلاع داشته باشد: email sms push

در نتیجه، با افزایش تعداد انواع، مسئولیت Function مرکزی نیز بیشتر می‌شود. اما Polymorphism نگاه متفاوتی به این مسئله دارد. به جای اینکه Function مرکزی تصمیم بگیرد چگونه هر Notification ارسال شود، این مسئولیت را به خود Object واگذار می‌کنیم: function deliver(notification) {

notification .send() }

اکنون deliver() فقط یک مسئولیت دارد: از Notification بخواهد که خودش مسئولیت ارسال را انجام دهد.

در این حالت: deliver(email)  deliver(sms)  deliver(push)

همگی می‌توانند از یک مسیر مشترک استفاده کنند؛ به شرط آنکه Object موردنظر Behavior مورد انتظار را ارائه کند.

این تغییر، در ظاهر ساده است، اما از نظر طراحی اهمیت زیادی دارد.

• در روش اول، Function مرکزی باید نوع Object و جزئیات اجرای آن را بشناسد.

• در روش دوم، Function فقط Behavior مورد نیاز را می‌شناسد و اجرای آن Behavior را به خود Object واگذار می‌کند.

به بیان دیگر، Polymorphism کمک می‌کند میان استفاده از یک Behavior و نحوه پیاده‌سازی آن Behavior تفاوت قائل شویم. کدی که از Object استفاده می‌کند، می‌داند چه کاری باید از Object بخواهد؛ اما لازم نیست بداند آن کار چگونه انجام می‌شود. هر Object مسئول ارائه Implementation مناسب خودش است.

Polymorphism و Method Lookup 

برای درک اینکه JavaScript چگونه این رفتار را ممکن می‌کند، باید دوباره به Method Lookup و Prototype Chain برگردیم. وقتی می‌نویسیم: notification .send(message) JavaScript باید مشخص کند send دقیقاً از کجا می‌آید و کدام Implementation باید اجرا شود.

در ابتدا JavaScript در خود Object به دنبال Propertyای با نام send می‌گردد. اگر چنین Propertyای در Object وجود نداشته باشد، جست‌وجو در Prototype آن ادامه پیدا می‌کند. سپس در صورت نیاز، Prototypeهای بعدی نیز بررسی می‌شوند تا Method مورد نظر پیدا شود یا زنجیره Prototype به پایان برسد.

برای مثال، اگر EmailNotification نسخه مخصوص خودش از send() را داشته باشد، Method Lookup همان نسخه را پیدا می‌کند. در نتیجه، پیاده‌سازی EmailNotification اجرا می‌شود. اما اگر EmailNotification خودش send() را تعریف نکرده باشد، جست‌وجو ادامه پیدا می‌کند و ممکن است به Prototype مربوط به Notification برسد. در این حالت، نسخه‌ای که در Notification تعریف شده است مورد استفاده قرار می‌گیرد.

بنابراین، Polymorphism در JavaScript یک سازوکار کاملاً جدا از Prototype Chain نیست. یکی از پایه‌های عملی آن، همان Method Lookup است. Objectهای مختلف می‌توانند یک Behavior مشترک را ارائه دهند و JavaScript هنگام فراخوانی Method، بر اساس Object واقعی و مسیر Prototype Chain، Implementation مناسب را پیدا می‌کند.

از این منظر، رابطه میان سه مفهوم اصلی این بخش روشن‌تر می‌شود:

• Inheritance امکان به‌اشتراک‌گذاری Behavior را فراهم می‌کند.

• Overriding به Child اجازه می‌دهد Implementation متفاوتی برای آن Behavior ارائه کند.

• Polymorphism باعث می‌شود کدی که با این Objectها کار می‌کند بتواند از همان Behavior مشترک استفاده کند، بدون آنکه به Implementation خاص هر Child وابسته باشد.

به همین دلیل، Polymorphism صرفاً به معنای «داشتن چند نسخه از یک Method » نیست. اهمیت اصلی آن در نحوه طراحی و فکر کردن درباره کد است. وقتی برنامه را بر اساس Behaviorهای مشترک طراحی می‌کنیم، کدی که از Objectها استفاده می‌کند می‌تواند با Objectهای بیشتری کار کند، بدون اینکه برای هر نوع جدید مجبور باشیم منطق استفاده از آن Object را از ابتدا تغییر دهیم.

برای مثال، اگر Function ما فقط به send() نیاز داشته باشد، اضافه شدن یک Notification جدید لزوماً نیازی به تغییر در Function ندارد: class WhatsAppNotification extends Notification {

send(message) {

console.log(`Sending WhatsApp: ${message}`) } }

اکنون همان Function قبلی می‌تواند با این Object نیز کار کند: deliver(new WhatsAppNotification())

Function deliver() لازم نیست بداند Notification جدید از چه نوعی است. آنچه اهمیت دارد این است که Object موردنظر Behavior مورد انتظار را ارائه می‌کند. این همان مدل ذهنی مهمی است که Polymorphism به طراحی برنامه اضافه می‌کند: Consumer → Common Behavior → Different Implementations

Consumer به جای شناختن همه انواع Objectها، با Behavior موردنیاز کار می‌کند و هر Object مسئول ارائه Implementation مناسب خودش است. در نتیجه، هنگام مشاهده یک Method Call مانند:

notification .send(message) بهتر است فقط به خود Method Call نگاه نکنیم. این عبارت در واقع یک سؤال را به JavaScript واگذار می‌کند: برای این Object، send کجا تعریف شده است و کدام Implementation باید اجرا شود؟

پاسخ این سؤال از طریق Method Lookup و Prototype Chain به دست می‌آید. همین فرآیند است که به Objectهای مختلف اجازه می‌دهد یک Behavior مشترک داشته باشند، اما Implementation متفاوتی ارائه دهند.

در نهایت، Polymorphism یک مدل ذهنی مهم برای کار با Objectها در JavaScript ایجاد می‌کند:

ما معمولاً با Object ، بر اساس Behavior آن کار می‌کنیم، نه صرفاً بر اساس نوع آن.

این طرز فکر باعث می‌شود کد کمتر به جزئیات انواع وابسته باشد و در برابر اضافه شدن Object های جدید، انعطاف‌پذیرتر باقی بماند. 

Inheritance فقط برای Code Reuse نیست

یکی از رایج‌ترین برداشت‌های ناقص درباره Inheritance این است که هدف آن فقط جلوگیری از تکرار Code و استفاده مجدد از آن است. این برداشت کاملاً نادرست نیست، اما برای درک نقش واقعی Inheritance کافی نیست.

وقتی می‌نویسیم: class PremiumAccount extends Account { }

در واقع فقط نگفته‌ایم که PremiumAccount می‌تواند بخشی از Code موجود در Account را دوباره استفاده کند. مهم‌تر از آن، یک رابطه مفهومی میان این دو نوع Object ایجاد کرده‌ایم: PremiumAccount is an Account

یعنی PremiumAccount باید واقعاً یک نوع تخصص‌یافته از Account باشد.

این نکته در طراحی بسیار مهم است؛ زیرا Inheritance فقط یک ابزار فنی برای Reuse کردن Code نیست. با استفاده از extends درباره رابطه میان دو مفهوم در مدل برنامه تصمیم می‌گیریم.

برای مثال، فرض کنید Order و Product هر دو Propertyای به نام id دارند. این شباهت به‌هیچ‌وجه به این معنا نیست که باید بنویسیم: class Product extends Order { }

شباهت در Property، و حتی شباهت در بعضی Methodها، به ‌تنهایی رابطه Inheritance ایجاد نمی‌کند. رابطه باید از نظر Domain و Responsibility نیز منطقی باشد. سؤال اصلی این نیست که آیا دو Class مقداری Code مشترک دارند؛ بلکه این است که آیا یکی واقعاً نوعی از دیگری است یا نه.

به همین دلیل، Inheritance را بهتر است ابتدا به‌عنوان یک رابطه طراحی در نظر بگیریم و سپس به مزایای فنی آن، مانند Code Reuse، توجه کنیم.

یک مثال کامل

برای اینکه مفاهیم اصلی این فصل را در کنار یکدیگر ببینیم، یک مثال نسبتاً واقعی‌تر در نظر بگیریم. فرض کنید Application ما یک سیستم Notification دارد. در این سیستم، یک Notification عمومی داریم که مسئولیت‌های مشترک میان انواع مختلف Notification را تعریف می‌کند: class Notification {

constructor(recipient) {

this .recipient = recipient }

send() {

console.log(`Sending notification to ${this .recipient}`) } }

اکنون می‌توانیم نوع تخصص‌یافته‌تری برای Email ایجاد کنیم: class EmailNotification extends Notification {

constructor(recipient, subject) {

super(recipient);

this .subject = subject }

send() {

console.log(`Sending email to ${this .recipient}: ${this .subject}`) } }

و برای SMS نیز Class دیگری تعریف کنیم: class SMSNotification extends Notification {

constructor(recipient, message) {

super(recipient);

this .message = message }

send() {

console.log(`Sending SMS to ${this .recipient}: ${this .message}`) } }

در اینجا EmailNotification و SMSNotification هر دو از Notification ارث‌بری می‌کنند؛ بنابراین بخشی از ساختار و Behavior مشترک خود را از Parent دریافت می‌کنند. در عین حال، هرکدام State مخصوص خود را نیز دارند.

EmailNotification علاوه بر recipient، یک subject دارد: recipient subject

در حالی که SMSNotification علاوه بر recipient، یک message دارد: recipient message

اما مهم‌تر از State مشترک یا متفاوت، این است که هر دو یک Behavior مشترک نیز ارائه می‌کنند: send()

با این تفاوت که Implementation این Behavior برای هر Child متفاوت است. اکنون می‌توانیم از هر دو نوع Object ایجاد کنیم:

const email = new EmailNotification(  const sms = new SMSNotification(

'omid@example.com',   '+989121234567',

'Order confirmed')   'Your order is ready')

در این مرحله، یک سؤال مهم مطرح می‌شود: آیا برای کار با این دو Object باید بدانیم یکی Email است و دیگری SMS؟

برای مثال، اگر Application فقط بخواهد یک Notification را ارسال کند، چرا باید Function مربوط به ارسال از نوع دقیق Notification اطلاع داشته باشد؟ به همین دلیل می‌توانیم Function عمومی‌تری بنویسیم:

function deliver(notification) { notification .send()}

اکنون: deliver(email) deliver(sms)  هر دو از همان Function استفاده می‌کنند.

درحالت اول،Implementation مربوط به Email اجرامی‌شود: Sending email to omid@example.com: Order confirmed

و درحالت دوم، Implementation مربوط به SMS اجرا می‌شود: Sending SMS to +989121234567: Your order is ready

در این مثال، تمام زنجیره مفاهیمی که در این فصل بررسی کردیم در کنار یکدیگر قرار می‌گیرند:

Notification → Inheritance → EmailNotification / SMSNotification → super()

→Method Overriding → Polymorphism

این مفاهیم مستقل از یکدیگر نیستند. هر مفهوم، مسئله‌ای را حل می‌کند که از مفهوم قبلی به وجود آمده است

• Inheritance ر ابطه Parent و Child را ایجاد می‌کند.

• super() به Child اجازه می‌دهد از Initialization یا Behavior مربوط به Parent استفاده کند.

• Overriding امکان ارائه Implementation متفاوت برای یک Behavior مشترک را فراهم می‌کند.

• و در نهایت Polymorphism به Code مصرف‌کننده اجازه می‌دهد با آن Behavior مشترک کار کند، بدون اینکه به Implementation خاص هر Child وابسته باشد. 

Inheritance و Composition

تا اینجا Inheritance راه‌حل مناسبی به نظر می‌رسد. اما یک سؤال مهم باقی می‌ماند:

آیا هر زمانی که چند Object ، Behavior یا مسئولیت مشترکی دارند، باید میان آن‌ها رابطه Inheritance ایجاد کنیم؟

پاسخ منفی است. فرض کنید Application ما یک Order دارد. این Order ممکن است با چند مسئولیت مختلف مانند موارد زیر سروکار داشته باشد : Payment Shipping Discount

ممکن است وسوسه شویم برای هر وضعیت یک Class فرزند ایجاد کنیم: PaidOrder یا ShippedOrder → Order

اما خیلی زود یک مسئله جدی ایجاد می‌شود. فرض کنید یک Order می‌تواند هم‌زمان:

Payment داشته باشد، Shipping داشته باشد، و از Discount نیز استفاده کند.

آیا باید برای هر ترکیب ممکن، یک Child Class جدید ایجاد کنیم؟

در چنین شرایطی تعداد Class ها می‌تواند به‌ سرعت افزایش پیدا کند. علاوه بر آن، مدل برنامه نیز پیچیده‌تر می‌شود و رابطه میان Classها دیگر به‌ سادگی قابل درک نخواهد بود.

مسئله اصلی این است که Payment، Shipping و Discount لزوماً نوعی از Order نیستند. آن‌ها می‌توانند قابلیت‌ها یا اجزایی باشند که Order از آن‌ها استفاده می‌کند. اینجاست که مفهوم Composition اهمیت پیدا می‌کند.

Composition چیست؟

در Composition به‌جای اینکه بگوییم: Object جدید، نوعی از Object قبلی است. می‌گوییم:

Object جدید از چند Component یا Behavior تشکیل شده است، یا از آن‌ها استفاده می‌کند.

به صورت مفهومی، Inheritance رابطه‌ای مانند این ایجاد می‌کند: PremiumAccount → Account

در اینجا PremiumAccount یک نوع از Account است. اما در Composition ساختار می‌تواند چنین باشد: Order 

├── Payment

├── Shipping

└── Discount

در اینجا Order نوعی از Payment یا Shipping نیست. بلکه از این اجزا استفاده می‌کند یا آن‌ها را در ساختار خود به کار می‌گیرد. بنابراین، در ساده‌ترین بیان می‌توان تفاوت این دو را این ‌گونه دید :

Inheritance → is-a  Composition → has-a / uses-a

در Inheritance، رابطه is-a اهمیت دارد. در Composition، رابطه has-a یا uses-a اهمیت بیشتری پیدا می‌کند.

این تفاوت، یکی از مهم‌ترین تصمیم‌های طراحی در کار با Objectهاست. 

چه زمانی Inheritance مناسب است؟

Inheritance زمانی انتخاب مناسبی است که رابطه میان Parent و Child از نظر مفهومی روشن باشد. Child باید واقعاً نوعی تخصص‌یافته از Parent باشد و بتواند مسئولیت و Behavioral Contract مربوط به Parent را حفظ کند.

همچنین باید Behavior مشترک واقعاً بخشی از مفهوم Child باشد و Hierarchy ایجادشده همچنان ساده و قابل فهم باقی بماند. برای مثال: Notification → EmailNotification رابطه قابل درکی است.

EmailNotification یک نوع Notification است و می‌تواند مسئولیت مشترک send() را ارائه کند؛ حتی اگر نحوه اجرای این مسئولیت در Child متفاوت باشد. در چنین شرایطی، Inheritance نه ‌تنها Code Reuse ایجاد می‌کند، بلکه ساختار مفهومی Domain را نیز به شکل مناسبی نمایش می‌دهد.

چه زمانی Composition مناسب‌تر است؟

Composition معمولاً زمانی انتخاب مناسب‌تری است که Object از چند قابلیت نسبتاً مستقل تشکیل شده باشد. اگر این قابلیت‌ها بتوانند در Objectهای مختلف نیز مورد استفاده قرار گیرند، یا رابطه is-a میان آن‌ها طبیعی نباشد، Composition می‌تواند مدل ساده‌تر و انعطاف‌پذیرتری ایجاد کند.

همچنین زمانی که Hierarchy به‌ سرعت در حال پیچیده شدن است، یا می‌خواهیم اجزای سیستم را مستقل‌تر تغییر دهیم، Composition معمولاً گزینه قابل بررسی بهتری است. برای مثال: Order

├── Payment

├── Shipping

└── Discount

در چنین مدلی هر مسئولیت می‌تواند Component مستقلی داشته باشد. این ساختار اجازه می‌دهد قابلیت‌ها از یکدیگر جدا باقی بمانند و تغییر در یک بخش لزوماً به ایجاد یا تغییر تعداد زیادی Child Class منجر نشود.

بنابراین، مسئله فقط این نیست که کدام روش Code بیشتری Reuse می‌کند. مسئله اصلی این است که کدام روش رابطه واقعی میان Objectها را بهتر مدل می‌کند.



اشتباهات رایج

اشتباه اول: تصور اینکه Inheritance فقط State را به ارث می‌دهد

Inheritance را نباید صرفاً به معنای دریافت Propertyهای Parent در نظر گرفت. Behavior نیز می‌تواند از طریق Prototype Chain در اختیار Child Instance قرار گیرد.

از طرف دیگر، State مربوط به Instance معمولاً در زمان اجرای Constructor روی خود Instance ایجاد می‌شود.

پس باید میان: Instance State و: Inherited Behavior تفاوت قائل شویم.

اشتباه دوم: تصور اینکه extends ،Code را Copy می‌کند.

وقتی می‌نویسیم: class PremiumAccount extends Account { } Methodهای Parent در Child Class کپی نمی‌شوند. بلکه رابطه Prototype ایجاد می‌شود و Property Lookup می‌تواند از Child به Parent ادامه پیدا کند. 

اشتباه سوم: تصور اینکه super() یک Object جدید می‌سازد.

super() یک Parent Instance جدید ایجاد نمی‌کند. در Child Constructor super(owner, balance) :

Constructor مربوط به Parent را برای Initialization همان Instance فراخوانی می‌کند.



اشتباه چهارم: تصور اینکه super .method() مقدار this را به Parent تغییر می‌دهد.

class PremiumAccount extends Account {

در اینجا Method مربوط بهParent روی همان Child Instance اجرا می‌شود. deposit(amount) {

this همچنان به Object فعلی مربوط است. super .deposit(amount) } }



اشتباه پنجم: استفاده از this قبل از super() در Derived Constructor

class PremiumAccount extends Account {  class PremiumAccount extends Account {

constructor(owner) {   constructor(owner) {

this.owner = owner;   super(owner);

super(owner) } }   this .rewardLevel = 'gold' }}

در Derived Constructor باید ابتدا Parent Initialization انجام شود. این کد صحیح نیست. 

اشتباه ششم: تصور اینکه Polymorphism فقط با extends ممکن است.

extends یکی از روش‌های رایج ایجاد رابطه‌ای است که به Polymorphism منجر می‌شود، اما مفهوم Polymorphism محدود به Inheritance نیست. در JavaScript، یک Function می‌تواند با هر Objectی کار کند که Behavior مورد نیاز آن را ارائه کند. مثلاً: function deliver(notification) { notification .send()}

این Function در اصل به وجود: send() نیاز دارد، نه لزوماً به یک Class Hierarchy خاص.

اشتباه هفتم: استفاده از Inheritance فقط برای جلوگیری از چند خط Code تکراری

اگر تنها دلیل استفاده از Inheritance این باشد که چند Property یا Method مشترک داریم، باید ابتدا رابطه Domain را بررسی کنیم. Inheritance یک تصمیم طراحی است، نه فقط یک ابزار برای حذف Duplication. 

اشتباه هشتم: ساخت Hierarchyهای عمیق

Hierarchies بسیار عمیق می‌توانند فهم Code را دشوار کنند. هرچه وابستگی میان Classها بیشتر شود، تغییر یک Parent می‌تواند روی تعداد بیشتری از Childها اثر بگذارد. در چنین شرایطی باید بررسی کنیم آیا Composition مدل ساده‌تری ارائه می‌دهد یا خیر. 

Best Practices

1. ابتدا رابطه Domain را بررسی کنید.

قبل از نوشتن extends بپرسید: آیا Child واقعاً نوعی از Parent است؟ اگر پاسخ روشن نیست، Inheritance احتمالاً انتخاب مناسبی نیست.

2. Parent را با مسئولیت مشخص طراحی کنید

Parent نبایدصرفاً محلی برای جمع‌کردن Code مشترک باشد. Behaviorهای Parent بایدبخشی ازمفهوم عمومی‌آن‌باشند



3. از super() برای حفظ Initialization Parent استفاده کنید.

اگر Parent مسئول ایجاد بخشی از State است، Child نباید بدون دلیل همان Initialization را تکرار کند.

4. از Overriding برای تغییر مسئولیت مشخص استفاده کنید.

اگر Child فقط به بخش کوچکی از Behavior Parent نیاز دارد، ابتدا بررسی کنید آیا واقعاً باید Method را Override کند یا خیر. Overriding باید رفتار را واضح‌تر کند، نه پیچیده‌تر.

5. Polymorphism را برای کاهش وابستگی استفاده کنید.

به جای اینکه Consumer تمام انواع Object را بشناسد، بهتر است Consumer تا حد امکان بر Behavior مورد نیاز تمرکز کند



6. وقتی رابطه has-a است، Composition را بررسی کنید.

اگر Object از چند قابلیت مستقل تشکیل شده است، Composition معمولاً مدل طبیعی‌تری ارائه می‌دهد.



Summary

در فصل قبل با Classها آشنا شدیم و دیدیم که Class می‌تواند الگوی ایجاد Instanceهایی با State و Behavior مشخص باشد. اما در یک Application واقعی، همیشه با Objectهایی کاملاً مستقل روبه‌رو نیستیم. گاهی چند نوع Object بخشی از State و Behavior خود را با یکدیگر به اشتراک می‌گذارند یا یکی از آن‌ها نوع تخصص‌یافته‌تری از دیگری است.

در چنین شرایطی می‌توانیم از Inheritance استفاده کنیم.

با: class PremiumAccount extends Account {} یک رابطه میان Parent و Child ایجاد می‌کنیم.

extends این رابطه را برقرار می‌کند و Child می‌تواند از Behavior موجود در Parent استفاده کند.

اگر Child دارای Constructor باشد، می‌تواند با super()، Constructor مربوط به Parent را برای Initialization همان Instance فراخوانی کند: super(name) همچنین در Methodها می‌توان با: super .method()

به Implementation مربوط به Parent دسترسی داشت.

Child نیز می‌تواند Method موجود در Parent را با تعریف یک Method همنام تغییر دهد. این رفتار را Method Overriding می‌نامیم.

در Polymorphism، Code مصرف‌کننده می‌تواند با یک Behavior مشترک کار کند، در حالی که Objectهای مختلف Implementation متفاوتی از آن Behavior ارائه می‌کنند. برای مثال: function deliver(notification) {

notification .send()}

همین Function می‌تواند با: EmailNotification SMSNotification و انواع دیگری از Notification که همان Behavior را ارائه می‌کنند، کار کند. در اینجا Caller به نوع دقیق Object وابسته نیست؛ بلکه به Behavior موردنیاز وابسته است.

اما در کنار این مفاهیم، یک نکته طراحی مهم نیز باید همیشه در نظر گرفته شود: Inheritance همیشه بهترین راه برای Reuse نیست. اگر رابطه واقعی میان Objectها: is-a باشد، Inheritance می‌تواند انتخاب مناسبی باشد.

اما اگر رابطه بیشتر: has-a یا: uses-a باشد، Composition معمولاً مدل طبیعی‌تری ارائه می‌کند.

بنابراین، هدف مهندسی این نیست که بیشترین استفاده را از extends داشته باشیم. هدف این است که رابطه واقعی میان Objectها را درست مدل کنیم. 

Key Takeaways

• Inheritance رابطه‌ای میان یک Parent Class و یک Child Class ایجاد می‌کند.

• extends برای ایجاد Class Inheritance استفاده می‌شود.

• Child می‌تواند Behavior موجود در Parent را استفاده کند.

• Inherited Methodها از طریق Prototype Chain قابل دسترسی هستند.

• State مربوط به Instance معمولاً روی خود Instance ایجاد و مقداردهی می‌شود.

• نباید State مربوط به Instance را با Inherited Behavior یکی دانست.

• super() ،Constructor مربوط به Parent را برای Initialization همان Instance فراخوانی می‌کند.

• در Derived Constructor باید قبل از استفاده از this، super() فراخوانی شود.

• super .method() می‌تواند Implementation مربوط به Parent را از Child فراخوانی کند.

• super .method() باعث تغییر this به Parent نمی‌شود.

• Method Overriding یعنی Child Implementation مربوط به یک Method Parent را با Implementation خودش جایگزین کند.

• Overriding باعث می‌شود Behavior یکسان در Objectهای مختلف، Implementationهای متفاوت داشته باشد.

• Polymorphism اجازه می‌دهد Consumer با یک Behavior مشترک با Objectهای مختلف کار کند.

• Polymorphism در JavaScript می‌تواند بر اساس وجود Behavior مورد نیاز در Object شکل بگیرد.

• Inheritance فقط ابزاری برای جلوگیری از Code Duplication نیست؛ یک تصمیم طراحی برای مدل‌کردن رابطه میان Objectها است.

• شباهت چند Property یا Method به‌تنهایی دلیل مناسبی برای Inheritance نیست.

• Composition در شرایطی که Object از چند قابلیت مستقل تشکیل شده است، می‌تواندانتخاب مناسب‌تری باشد.

• is-a معمولاً به Inheritance و has-a معمولاً به Composition اشاره می‌کند. 

Technical Interview

سطح پایه (Junior)

⭐ Inheritance چیست؟

Inheritance مکانیزمی است که به یک Child Class اجازه می‌دهد Behavior مرتبط با یک Parent Class را استفاده یا گسترش دهد. در JavaScript معمولاً با extends ایجاد می‌شود.

⭐ extends چه کاری انجام می‌دهد؟

extends یک رابطه Inheritance میان دو Class ایجاد می‌کند و باعث می‌شود Child بتواند به Behaviorهای Parent از طریق Prototype Chain دسترسی داشته باشد. 

⭐ Parent Class و Child Class چیستند؟

Parent Class ، Class عمومی‌تری است که Behavior یا State مشترک را تعریف می‌کند.

Child Class ،Class تخصصی‌تری است که از Parent ارث‌بری می‌کند و می‌تواند Behaviorهای Parent را استفاده کند، Behavior جدید اضافه کند یا Behavior موجود را Override کند. 

⭐ super() چه کاری انجام می‌دهد؟

super() در Child Constructor، Constructor مربوط به Parent را فراخوانی می‌کند تا Initialization ، State مربوط به Parent انجام شود. همچنین super .method() برای فراخوانی Method مربوط به Parent استفاده می‌شود. 

⭐ چرا در Child Constructor قبل از this باید از super() استفاده کنیم؟

در Derived Constructor، Initialization مربوط به Parent باید ابتدا انجام شود. بنابراین استفاده از this قبل از super() باعث ReferenceError می‌شود. 

⭐ Method Overriding چیست؟

Method Overriding زمانی اتفاق می‌افتد که Child Methodی با همان نام Method موجود در Parent تعریف کند و Implementation مخصوص خودش را ارائه دهد. 

⭐ Polymorphism چیست؟

Polymorphism یعنی بتوانیم Objectهای مختلف را از طریق یک Behavior مشترک استفاده کنیم، در حالی که هر Object می‌تواند Implementation متفاوتی از آن Behavior داشته باشد.

⭐ تفاوت Inheritance و Composition چیست؟

در Inheritance یک رابطه‌ی Parent/Child یا معمولاً is-a ایجاد می‌شود. Admin is a User

در Composition یک Object از Object دیگری استفاده می‌کند و رابطه معمولاًhas-a یا uses-a است. Order uses Logger

بنابراین: Inheritance برای مدل‌کردن رابطه‌ی واقعی میان انواع Objects مناسب است؛ Composition برای ترکیب قابلیت‌های مستقل معمولاً انعطاف‌پذیرتر است. 

سطح Mid-Level

⭐ آیا extends ، Methodهای Parent را Copy می‌کند؟

خیر. Methodها Copy نمی‌شوند. Inheritance میان Prototypeها ایجاد می‌شود و Property Lookup می‌تواند از Child Prototype به Parent Prototype ادامه پیدا کند.

⭐ تفاوت State و Behavior در Inheritance چیست؟

State مربوط به Instance معمولاً به‌عنوان Property روی خود Instance ایجاد می‌شود، در حالی که Instance Methodها می‌توانند از طریق Prototype Chain از Parent در دسترس Child قرار بگیرند. 

⭐ آیا super .method() روی Parent Instance اجرا می‌شود؟

خیر. super .method() ، Implementation مربوط به Parent را پیدا می‌کند، اما Method روی همان Child Instance فعلی اجرا می‌شود و this همچنان به آن Instance مربوط است.

⭐ چگونه Inheritance می‌تواند به Polymorphism منجر شود؟

اگر Parent یک Behavior مشترک تعریف کند و Childها آن Behavior را Override کنند، Consumer می‌تواند با همان Method مشترک با Objectهای مختلف کار کند، در حالی که Implementation واقعی بر اساس Object انتخاب می‌شود.

⭐ آیا Polymorphism در JavaScript به extends وابسته است؟

خیر. extends یکی از روش‌های ایجاد چنین ساختاری است، اما JavaScript می‌تواند بر اساس وجود Behavior مورد نیازبا Objectها کارکند. مثلاً Function ای که فقط send() را فراخوانی می‌کند، لزوماً به Class Hierarchy خاصی وابسته نیست

⭐ چرا Inheritance را نباید فقط برای Code Reuse استفاده کرد؟

زیرا Inheritance یک رابطه مفهومی میان انواع Objectها ایجاد می‌کند. اگر فقط چند Property یا Method مشترک داشته باشیم اما رابطه واقعی is-a وجود نداشته باشد، Inheritance می‌تواند Coupling و پیچیدگی غیرضروری ایجاد کند.

⭐ چه زمانی Composition بهتر از Inheritance است؟

وقتی Object از چند قابلیت مستقل تشکیل شده باشد یا رابطه میان Objectها بیشتر has-a باشد، Composition معمولاً مدل انعطاف‌پذیرتری ایجاد می‌کند. 

⭐ تفاوت super() و super .method() چیست؟

super() ،Constructor مربوط به Parent را فراخوانی می‌کند super .method() ،Implementation یک Method مربوط به Parent را از Child فراخوانی می‌کند.

⭐ چرا ممکن است به جای تکرار Behavior در چند Class از Inheritance استفاده کنیم؟

اگر چند Class واقعاً Behavior مشترکی داشته باشند و رابطه‌ی منطقی Parent/Child میان آن‌ها وجود داشته باشد، Inheritance اجازه می‌دهد Behaviorمشترک در یک Parent تعریف شود و Childها آن را استفاده کنند. در نتیجه Behavior مشترک در چند Class تکرار نمی‌شود و تغییر آن نیز می‌تواند در یک نقطه انجام شود. اما هدف نباید صرفاً کاهش خطوط کد باشد؛ رابطه‌ی Inheritance باید از نظر مدل دامنه نیز منطقی باشد. 

⭐ تفاوت Override کردن یک Method با فراخوانی Parent Method با super چیست؟

در Override، Child یک Method با همان نام تعریف می‌کند و هنگام فراخوانی، Behavior مربوط به Child اجرا می‌شود تا Behavior متفاوتی ارائه دهد. اما با super()، Child می‌تواند صراحتاً Method مربوط به Parent را فراخوانی کند. بنابراین Override می‌تواند Behavior Parent را جایگزین کند، در حالی که super امکان استفاده از Behavior Parent را در Child فراهم می‌کند.

⭐ چگونه یک Child Class می‌تواند Behavior Parent را گسترش دهد؟

Child ، ابتدا Behavior Parent را با super .method() اجرا می‌کند و سپس می‌تواند Behavior جدید خود را اضافه کند. در این حالت Behavior Parent حذف نشده است؛ بلکه Child آن را گسترش داده است. 

⭐ چرا Code Reuse به‌تنهایی دلیل مناسبی برای استفاده از Inheritance نیست؟

Inheritance یک رابطه‌ی ساختاری و وابستگی میان Classها ایجاد می‌کند. استفاده از Inheritance فقط برای Reuse می‌تواند Hierarchy نامناسب، Coupling بیشتر و وابستگی‌های سخت برای تغییر ایجاد کند. 

⭐ چگونه تشخیص می‌دهید یک رابطه برای Inheritance مناسب است؟

ابتدا بررسی می‌کنم آیا Child واقعاً یک نوع تخصصی از Parent است یا خیر: Child is a Parent

سپس بررسی می‌کنم Behavior مشترک واقعاً بخشی از مدل Parent باشد، رابطه در آینده نیز منطقی بماند و Child بتواند Contract رفتاری Parent را حفظ کند. 

⭐ Polymorphism چگونه می‌تواند تعداد شرط‌های موجود در Code را کاهش دهد؟

به جای اینکه یک Function نوع Object را بررسی کند و برای هر نوع شرط جداگانه داشته باشد، می‌توان Behavior مشترکی تعریف کرد و Implementation هر نوع Object را به خودش سپرد.

در این حالت Caller فقط به method رفتاری مشترک وابسته است و لازم نیست نوع دقیق Object را بررسی کند.

⭐ چرا Composition ممکن است از Inheritance انعطاف‌پذیرتر باشد؟

Composition به جای ایجاد یک Hierarchy ثابت، قابلیت‌ها را از طریق ترکیب Objectها در کنار یکدیگر قرار می‌دهد. یک Object می‌تواند از چند Component مستقل استفاده کند، بدون اینکه مجبور باشد در یک زنجیره‌ی Parent/Child قرار بگیرد. این مدل معمولاً Coupling کمتری ایجاد می‌کند و تغییر یا جایگزینی یک Component را ساده‌تر می‌سازد.

سطح Senior

⭐ مهم‌ترین ریسک استفاده گسترده از Inheritance چیست؟

مهم‌ترین ریسک، ایجاد Coupling شدید میان Parent و Childها و شکل‌گیری Hierarchyهای پیچیده است. تغییر در Parent می‌تواند Behavior چندین Child را تحت تأثیر قرار دهد و در نتیجه نگهداری سیستم دشوارتر شود.

⭐ چرا Inheritance را یک تصمیم طراحی می‌دانیم و نه صرفاً یک تکنیک برای Reuse؟

زیرا extends یک رابطه مفهومی و ساختاری میان دو نوع Object ایجاد می‌کند. بنابراین انتخاب آن باید بر اساس رابطه Domain، مسئولیت‌ها و Contract رفتاری انجام شود، نه صرفاً بر اساس وجود Code مشترک. 

⭐ چگونه تشخیص می‌دهید Inheritance انتخاب مناسبی نیست؟

اگر رابطه is-a طبیعی نباشد، اگر Parent فقط برای جمع‌کردن Code مشترک ساخته شده باشد، اگر Hierarchy در حال عمیق‌شدن باشد یا اگر قابلیت‌ها مستقل و قابل ترکیب باشند، باید Composition را جدی‌تر بررسی کرد. 

⭐ چرا Method Overriding برای Polymorphism مهم است؟

زیرا اجازه می‌دهدچند Object یک Behavior مشترک داشته باشند اماآن را با Implementationهای متفاوت اجرا کنند. Consumer همچنان ازیک Operation مشترک استفاده می‌کندوجزئیات Implementation به Object واگذارمی‌شود

⭐ چگونه Prototype Chain به Polymorphism مربوط می‌شود؟

در Class Inheritance، extends رابطه Prototypeها را ایجاد می‌کند. هنگام Method Lookup ، JavaScript ابتدا

Child Prototype را بررسی می‌کند و سپس در صورت نیاز به Parent Prototype می‌رود؛ بنابراین اگر Child ،Method را Override کرده باشد، Implementation Child پیدا و اجرا می‌شود.

⭐ آیا extends در JavaScript به معنی Copy شدن Methodهای Parent داخل Childاست؟

خیر. extends ،Source Code یا Methodهای Parent را داخل Child کپی نمی‌کند؛ بلکه یک رابطه‌ی Inheritance ایجاد می‌کند. در Class Syntax، این رابطه بر پایه‌ی Prototype mechanism زبان JavaScript شکل می‌گیرد و Methodهای Inherited از طریق زنجیره‌ی Prototype قابل دسترسی هستند.

بنابراین Inheritance را باید یک رابطه دانست، نه یک عملیات Copy.

⭐ Polymorphism چه ارزش مهندسی‌ای برای طراحی نرم‌افزار دارد؟

Polymorphism اجازه می‌دهد Caller به یک Behavior مشترک وابسته باشد، در حالی که Implementation واقعی توسط Object مشخص می‌شود. نتیجه، کاهش وابستگی به Typeهای مشخص، کاهش شرط‌های مربوط به نوع Object و امکان اضافه‌کردن Implementationهای جدید با تغییر کمتر در Caller است. بنابراین ارزش اصلی Polymorphism در Separation of Concerns، کاهش Coupling و افزایش قابلیت توسعه است.

Conclusion

در فصل 29، Class به ما اجازه داد ساختار و Behavior یک نوع Object را در یک Abstraction واحد تعریف کنیم. اما در Applicationهای واقعی، همه Objectها مستقل نیستند. گاهی یک Class، نوع تخصصی‌تری از Class دیگر است. برای مثال: Admin is a User در چنین شرایطی، Inheritance می‌تواند رابطه میان این دو را مدل کند.

با استفاده از: extends Class فرزند به Parent متصل می‌شود. Child می‌تواند Behaviorهای Parent را استفاده کند، Behavior جدید اضافه کند و در صورت نیاز Behavior موجود را Override کند. وقتی Child دارای Constructor باشد، super() برای اجرای Constructor مربوط به Parent استفاده می‌شود: super(name)

و در Methodها می‌توان با: super .method() Behavior مربوط به Parent را فراخوانی کرد.

سپس با Method Overriding می‌توان یک Behavior مشترک را برای انواع مختلف Object به شکل متفاوت پیاده‌سازی کرد. اینجاست که به Polymorphism می‌رسیم:

Common Behavior → Different Implementations → Polymorphism

در این مدل، Caller به یک Behavior مشترک وابسته است، نه به جزئیات هرImplementation.

اما Inheritance را نباید صرفاً به‌عنوان ابزاری برای Reuse کردن Code در نظر گرفت. Inheritance یک رابطه طراحی ایجاد می‌کند. اگر رابطه واقعی: is-a  باشد، Inheritance می‌تواند انتخاب مناسبی باشد.

اگر رابطه: has-a یا: uses-a  باشد، Composition معمولاً مدل طبیعی‌تری ارائه می‌کند.

بنابراین، تصمیم نهایی درباره Inheritance یا Composition نباید بر اساس میزان Code Reuse گرفته شود. پرسش اصلی این است که رابطه واقعی میان Objectها چیست و کدام مدل آن رابطه را به شکل روشن‌تر و ساده‌تری بیان می‌کند؟

مدل ذهنی نهایی این فصل را می‌توان چنین دید:

Shared Behavior → Inheritance → extends → Parent / Child → super() → Method Overriding

→ Polymorphism → Design Decision → Inheritance or Composition

این مدل کمک می‌کند Inheritance و Polymorphism را صرفاً چند Keyword یا Syntax در JavaScript نبینیم، بلکه آن‌ها را به‌عنوان ابزارهایی برای مدل‌سازی رابطه‌ها، سازمان‌دهی Behavior و طراحی ساختار نرم‌افزار درک کنیم.

 