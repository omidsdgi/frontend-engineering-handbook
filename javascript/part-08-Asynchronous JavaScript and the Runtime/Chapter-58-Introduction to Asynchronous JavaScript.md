حتماً. در بازنویسی، اسکلت فصل و Concept Flow را تغییر نمی‌دهم؛ هدف این است که همان محتوا از نظر استدلالی به کیفیت Chapter 27 نزدیک‌تر شود: هر Concept از یک مسئله طبیعی متولد شود، تعریف بلافاصله بعد از نیاز بیاید، و در پایان هر بخش سؤال بعدی خواننده را به Concept بعدی هدایت کند.

همچنین عمداً وارد جزئیات Event Loop، Queue، Promise و async/await نمی‌شوم، چون Roadmap آن‌ها را برای فصل‌های بعدی قرار داده است.

Chapter 58 — Introduction to Asynchronous JavaScript

اهداف فصل

پس از مطالعه این فصل، می‌توانید:

تفاوت Synchronous و Asynchronous Execution را توضیح دهید.
مفهوم Blocking را در جریان اجرای JavaScript تحلیل کنید.
توضیح دهید چرا عملیات زمان‌بر می‌توانند اجرای برنامه را متوقف کنند.
مفهوم Non-Blocking را درک کنید.
رابطه Single JavaScript Thread و Host Environment را توضیح دهید.
Asynchronous Programming را به‌عنوان پاسخی به مسئله Blocking تحلیل کنید.
تفاوت Asynchronous Execution و Parallel Execution را تشخیص دهید.
مرز این فصل با Event Loop، Promise و async/await را مشخص کنید.
Core Question

چگونه JavaScript بدون متوقف کردن اجرای برنامه با عملیات زمان‌بر کار می‌کند؟

برای پاسخ به این سؤال، مسیر فصل چنین است:

Synchronous Execution
↓
Long-Running Operation
↓
Blocking
↓
Non-Blocking
↓
Single JavaScript Thread
↓
Host Environment
↓
Asynchronous Programming
مقدمه

تا اینجا JavaScript را عمدتاً در قالب اجرای معمول کد بررسی کرده‌ایم.

Function فراخوانی می‌شود، Execution Context ایجاد می‌شود، دستورها اجرا می‌شوند و در نهایت نتیجه در اختیار برنامه قرار می‌گیرد.

این مدل تا زمانی کاملاً مناسب است که عملیات موردنظر سریع و قابل پیش‌بینی باشد.

اما Applicationهای واقعی فقط با چنین عملیات‌هایی سروکار ندارند.

فرض کنید کاربر روی یک Recipe کلیک می‌کند و Application باید اطلاعات آن را از Server دریافت کند.

در اینجا JavaScript نمی‌تواند تضمین کند که پاسخ Server دقیقاً چه زمانی آماده خواهد شد.

ممکن است پاسخ در چند میلی‌ثانیه برسد یا زمان بیشتری طول بکشد.

اکنون یک مسئله مهم شکل می‌گیرد:

اگر JavaScript عملیات را به‌ترتیب اجرا می‌کند، آیا باید برای تمام مدت دریافت پاسخ متوقف بماند؟

اگر پاسخ مثبت باشد، یک عملیات خارجی و زمان‌بر می‌تواند ادامه اجرای Application را متوقف کند.

برای فهمیدن اینکه JavaScript چگونه از این وضعیت جلوگیری می‌کند، ابتدا باید ببینیم اجرای معمول JavaScript چگونه انجام می‌شود.

Synchronous Execution

در مدل معمول اجرای JavaScript، عملیات به‌صورت ترتیبی پیش می‌روند.

به این مدل Synchronous Execution گفته می‌شود.

یعنی اجرای یک جریان از عملیات به این شکل قابل تصور است:

Operation A
↓
Operation B
↓
Operation C
↓
Operation D

برای مثال:

const price = 100;
const tax = 10;

const total = price + tax;

console.log(total);

در این مثال، ابتدا price و tax مقداردهی می‌شوند، سپس total محاسبه می‌شود و در نهایت console.log() اجرا می‌شود.

مدل ذهنی ساده آن:

Calculate
↓
Calculate
↓
Log

این رفتار مزیت مهمی دارد: ترتیب اجرای عملیات قابل پیش‌بینی است.

اما همین ویژگی وقتی با یک عملیات زمان‌بر مواجه می‌شویم، می‌تواند به یک مشکل تبدیل شود.

فرض کنید Operation B برای تکمیل شدن به زمان قابل‌توجهی نیاز داشته باشد.

در مدل Synchronous:

Operation A
↓
Operation B
↓
Wait
↓
Operation C

تا زمانی که جریان اجرای Operation B به مرحله لازم نرسد، اجرای بعدی جلو نمی‌رود.

پس مشکل اصلی از اینجا آغاز می‌شود:

اگر یک Operation طولانی شود، چه اتفاقی برای عملیات بعدی می‌افتد؟

Long-Running Operation

همه عملیات‌ها از نظر زمان اجرا یکسان نیستند.

برخی عملیات‌ها تقریباً بلافاصله نتیجه می‌دهند.

اما برخی عملیات‌ها به زمان بیشتری نیاز دارند یا زمان تکمیل آن‌ها مستقیماً در کنترل JavaScript نیست.

برای مثال، Application ممکن است بخواهد:

داده‌ای را از یک Server دریافت کند.
برای یک Timer منتظر بماند.
نتیجه یک عملیات وابسته به محیط اجرا را دریافت کند.
به یک تعامل خارجی واکنش نشان دهد.

در چنین شرایطی، پایان عملیات دقیقاً در همان لحظه‌ای که آن را شروع می‌کنیم مشخص نیست.

این همان چیزی است که در این فصل با مفهوم Long-Running Operation به آن اشاره می‌کنیم.

اما خودِ زمان‌بر بودن هنوز مشکل اصلی نیست.

مشکل زمانی ایجاد می‌شود که JavaScript مجبور شود اجرای ادامه برنامه را به پایان این عملیات وابسته کند.

برای مثال:

Start Operation
↓
Long-Running Operation
↓
?
↓
Next Operation

اگر JavaScript در علامت ? متوقف بماند، عملیات زمان‌بر باعث Blocking شده است.

Blocking

Blocking یعنی یک عملیات مانع ادامه اجرای جریان موردنظر شود.

در ساده‌ترین مدل:

Operation A
↓
Long-Running Operation
↓
BLOCK
↓
Operation B

تا زمانی که عملیات Blocking به مرحله لازم نرسد، Operation B نمی‌تواند در همان جریان اجرا شود.

پس می‌توانیم رابطه را این‌گونه ببینیم:

Long-Running Operation
↓
Waiting
↓
Blocking
↓
Execution Cannot Continue

اینجا یک نکته مهم وجود دارد.

Long-Running بودن و Blocking بودن دقیقاً یک مفهوم نیستند.

یک عملیات ممکن است زمان‌بر باشد، اما مسئله مهم این است که آیا جریان اجرای JavaScript را متوقف می‌کند یا خیر.

بنابراین سؤال مهم‌تر این نیست که:

«این عملیات چقدر طول می‌کشد؟»

بلکه این است:

«آیا JavaScript برای تمام مدت اجرای آن مجبور است منتظر بماند؟»

اگر پاسخ مثبت باشد، Application می‌تواند با مشکل مواجه شود.

چرا Blocking مشکل‌ساز است؟

این مسئله در Browser اهمیت بیشتری پیدا می‌کند.

یک Browser Application فقط محاسبات داخلی انجام نمی‌دهد.

Application باید هم‌زمان بتواند به فعالیت‌های مختلف پاسخ دهد:

User Interaction
↓
JavaScript
↓
UI Update
↓
Other Application Work

حالا فرض کنید یک عملیات زمان‌بر اجرای JavaScript را متوقف کند:

User Interaction
↓
Long-Running Operation
↓
BLOCKED
↓
Other Work

در چنین شرایطی، عملیات‌های دیگر نمی‌توانند به‌موقع در جریان اجرای JavaScript قرار بگیرند.

پس یک نیاز طبیعی شکل می‌گیرد:

آیا می‌توان عملیات زمان‌بر را شروع کرد، بدون اینکه اجرای JavaScript برای تمام مدت آن متوقف شود؟

این سؤال ما را به مفهوم Non-Blocking می‌رساند.

Non-Blocking

Non-Blocking به مدلی اشاره می‌کند که در آن شروع یک عملیات زمان‌بر، جریان اجرای JavaScript را مجبور نمی‌کند تا پایان آن عملیات متوقف بماند.

در این مدل:

Start Long-Running Operation
↓
Continue Execution
↓
Other Work

و عملیات زمان‌بر می‌تواند بعداً به مرحله تکمیل برسد.

بنابراین تفاوت دو مدل را می‌توان این‌گونه دید:

Blocking:

Start
↓
Wait
↓
Complete
↓
Continue

در مقابل:

Non-Blocking:

Start
↓
Continue
↓
Other Work
↓
Operation Completes Later

این تفاوت، نقطه شروع Asynchronous Programming است.

اما اکنون یک سؤال مهم‌تر داریم:

اگر JavaScript برای تمام مدت عملیات منتظر نمی‌ماند، چه چیزی عملیات زمان‌بر را در این فاصله مدیریت می‌کند؟

برای پاسخ، ابتدا باید جایگاه JavaScript را در Runtime بهتر بشناسیم.

Single JavaScript Thread

در مدل اجرای JavaScript، اجرای مستقیم کد را می‌توان به‌صورت یک جریان اجرای واحد در نظر گرفت.

به همین دلیل معمولاً گفته می‌شود:

JavaScript یک زبان Single-Threaded است.

اما این عبارت نیاز به دقت دارد.

Single-Threaded بودن JavaScript به این معنا نیست که کل Browser یا کل Runtime فقط یک Thread دارد.

منظور این است که اجرای مستقیم JavaScript در یک جریان اجرای مشخص، چند قطعه JavaScript را به‌صورت هم‌زمان اجرا نمی‌کند.

مدل ساده:

JavaScript Execution
↓
Code A
↓
Code B
↓
Code C

نه:

Code A ──┐
├── اجرای هم‌زمان JavaScript
Code B ──┘

این نکته سؤال مهمی ایجاد می‌کند.

اگر JavaScript در یک جریان اجرا می‌شود و هم‌زمان چند قطعه JavaScript را اجرا نمی‌کند، پس چگونه یک عملیات زمان‌بر می‌تواند بدون Blocking شدن جریان JavaScript ادامه پیدا کند؟

پاسخ این سؤال در خود JavaScript Language به‌تنهایی قرار ندارد.

برای دیدن تصویر کامل‌تر باید به Host Environment نگاه کنیم.

Host Environment

JavaScript همیشه در یک محیط اجرا می‌شود.

خود JavaScript Language قابلیت‌های زبانی مانند:

Variables
Functions
Objects
Operators
Control Flow

را تعریف می‌کند.

اما Application به قابلیت‌های دیگری نیز نیاز دارد.

در Browser، برای مثال، JavaScript باید بتواند با:

Web Page
User Interaction
Timerها
Network
سایر قابلیت‌های Browser

تعامل کند.

این قابلیت‌ها بخشی از Host Environment هستند.

بنابراین باید دو سطح را از هم جدا کنیم:

JavaScript Language
↓
JavaScript Execution

و:

Host Environment
↓
Browser Capabilities

رابطه کلی:

JavaScript
↕
Host Environment

این تفکیک برای Asynchronous JavaScript بسیار مهم است.

زیرا تمام عملیات زمان‌بر مستقیماً توسط جریان اجرای JavaScript انجام نمی‌شوند.

برخی عملیات در اختیار قابلیت‌های Host Environment قرار می‌گیرند.

چرا Host Environment به Asynchronous Programming مربوط می‌شود؟

اکنون می‌توانیم مسئله را یک مرحله دقیق‌تر ببینیم.

JavaScript یک جریان اجرای مشخص دارد:

JavaScript
↓
Execution

اما Application به عملیاتی نیاز دارد که ممکن است زمان‌بر باشند:

JavaScript
↓
Request for External Operation
↓
Host Environment

در این مدل، JavaScript لازم نیست تمام مدت اجرای آن عملیات خارجی را در همان جریان اجرای مستقیم خود سپری کند.

به‌صورت مفهومی:

JavaScript
↓
Start Operation
↓
Host Environment
↓
JavaScript continues

اکنون مسئله‌ای که در ابتدای فصل داشتیم، به شکل جدیدی دیده می‌شود.

ما می‌خواستیم:

Long-Running Operation
↓
No Blocking

و اکنون می‌دانیم که Host Environment بخشی از این مدل را فراهم می‌کند.

اما هنوز یک سؤال باقی مانده است:

وقتی عملیات در Host Environment به پایان رسید، نتیجه چگونه دوباره وارد جریان اجرای JavaScript می‌شود؟

این دقیقاً جایی است که Runtime به یک سازوکار هماهنگ‌کننده نیاز دارد.

جزئیات این سازوکار در فصل بعد بررسی خواهد شد.

Asynchronous Programming

اکنون می‌توانیم مفهوم اصلی فصل را دقیق‌تر تعریف کنیم.

Asynchronous Programming روشی برای طراحی و اجرای برنامه است که در آن نتیجه یک عملیات زمان‌بر لازم نیست قبل از ادامه جریان اجرای برنامه آماده شده باشد.

به بیان ساده:

برنامه می‌تواند یک عملیات زمان‌بر را شروع کند و بدون انتظار مستقیم برای پایان آن، به ادامه اجرای خود بپردازد.

مدل ذهنی:

Start Operation
↓
Do Not Block
↓
Continue Execution
↓
Operation Completes Later
↓
Handle Result

این مدل مستقیماً از مسئله‌ای که در ابتدای فصل دیدیم به وجود آمده است:

Long-Running Operation
↓
Blocking Problem
↓
Need for Non-Blocking
↓
Asynchronous Programming

بنابراین Asynchronous Programming یک مفهوم مستقل و تصادفی نیست.

پاسخی است به یک نیاز واقعی در اجرای Application.

یک مثال واقعی

فرض کنید یک Recipe Application باید اطلاعات یک Recipe را از Server دریافت کند.

در یک مدل Blocking می‌توانستیم چنین تصوری داشته باشیم:

User clicks Recipe
↓
Request starts
↓
Wait for Server
↓
Response arrives
↓
Continue Application

مشکل این مدل این است که جریان Application در زمان انتظار به نتیجه Request وابسته شده است.

مدل Non-Blocking چنین مسئله‌ای را تغییر می‌دهد:

User clicks Recipe
↓
Request starts
↓
Application continues
↓
Server responds later
↓
Application handles result

نکته اصلی این مثال Request نیست.

نکته اصلی این است که:

شروع یک عملیات زمان‌بر نباید الزاماً به معنای توقف جریان اجرای Application باشد.

همین نیاز، اساس Asynchronous Programming است.

Asynchronous به معنی Parallel نیست

در اینجا یک سوءبرداشت مهم باید برطرف شود.

ممکن است از توضیحات قبلی این نتیجه گرفته شود که:

اگر JavaScript منتظر عملیات نمی‌ماند، پس چند عملیات JavaScript هم‌زمان اجرا می‌شوند.

این نتیجه درست نیست.

Asynchronous و Parallel دو مفهوم متفاوت هستند.

Asynchronous درباره نحوه مدیریت عملیات و انتظار است.

Parallelism درباره اجرای واقعی چند عملیات در یک زمان با استفاده از منابع اجرایی مختلف است.

پس:

Asynchronous
→ جریان اجرای برنامه برای یک عملیات زمان‌بر متوقف نمی‌شود

در حالی که:

Parallel
→ چند عملیات واقعاً به‌صورت هم‌زمان اجرا می‌شوند

بنابراین می‌توان رفتار Asynchronous داشت، بدون اینکه چند قطعه JavaScript در همان لحظه روی یک جریان JavaScript اجرا شوند.

این تفاوت، یکی از بخش‌های مهم مدل ذهنی Async JavaScript است.

Asynchronous Programming چه چیزی را حل می‌کند؟

اکنون می‌توانیم مسئله را از ابتدا تا اینجا دنبال کنیم.

در ابتدا:

Synchronous Execution

را داشتیم.

سپس با یک عملیات زمان‌بر مواجه شدیم:

Long-Running Operation

اگر اجرای برنامه منتظر آن بماند:

Blocking

اتفاق می‌افتد.

اما Application به مدلی نیاز دارد که در آن عملیات زمان‌بر بتواند بدون متوقف کردن جریان اجرای JavaScript ادامه پیدا کند:

Non-Blocking

از طرف دیگر، JavaScript را نمی‌توان به‌سادگی به‌عنوان محیطی در نظر گرفت که تمام عملیات Application را خودش و به‌صورت هم‌زمان اجرا می‌کند.

اینجا Host Environment وارد مدل می‌شود.

در نتیجه:

Synchronous Execution
↓
Long-Running Operation
↓
Blocking Problem
↓
Need for Non-Blocking Execution
↓
Single JavaScript Thread
↓
Host Environment
↓
Asynchronous Programming

اکنون مدل اصلی فصل کامل شده است.

اما یک بخش مهم هنوز توضیح داده نشده است.

نتیجه عملیات Async چگونه برمی‌گردد؟

فرض کنید:

JavaScript
↓
Start Operation
↓
Host Environment

و JavaScript اجرای خود را ادامه می‌دهد.

بعداً:

Operation Completes

حالا چه اتفاقی می‌افتد؟

نتیجه عملیات باید somehow دوباره در اختیار JavaScript قرار گیرد.

اما JavaScript نمی‌تواند در همان لحظه هر کدی را که Host Environment آماده کرده است، به‌صورت تصادفی اجرا کند.

پس Runtime به یک سازوکار برای هماهنگ‌کردن این عملیات نیاز دارد.

مدل مفهومی:

JavaScript
↓
Host Environment
↓
Operation Completes
↓
Runtime Coordination
↓
JavaScript Execution

این Runtime Coordination همان مسئله‌ای است که در فصل بعد با مفهوم Event Loop آن را به‌صورت دقیق بررسی خواهیم کرد.

در نتیجه، در این فصل هنوز لازم نیست بدانیم:

Task Queue چگونه کار می‌کند.
Microtask Queue چیست.
Event Loop دقیقاً چه زمانی Queue را بررسی می‌کند.
ترتیب اجرای Callbackها چگونه تعیین می‌شود.

این‌ها پاسخ سؤال بعدی هستند، نه سؤال این فصل.

Asynchronous JavaScript یک مدل ترکیبی است

اکنون می‌توانیم یک سوءبرداشت دیگر را نیز برطرف کنیم.

Asynchronous JavaScript به این معنی نیست که JavaScript دیگر Synchronous نیست.

بخش زیادی از کد JavaScript همچنان Synchronous اجرا می‌شود.

برای مثال:

Function Call
↓
Statement
↓
Calculation
↓
Return

همچنان در جریان معمول JavaScript انجام می‌شود.

Asynchronous Programming زمانی وارد مدل می‌شود که Application با عملیاتی مواجه شود که تکمیل آن ممکن است زمان ببرد و نمی‌خواهیم جریان اجرای JavaScript برای تمام مدت آن متوقف شود.

بنابراین مدل صحیح‌تر این است:

Synchronous JavaScript
+
Asynchronous Operations
+
Host Environment
↓
Asynchronous Application

به همین دلیل، Asynchronous Programming را باید به‌عنوان مدیریت یک جریان ترکیبی از عملیات Synchronous و Asynchronous درک کرد.

چرا این مدل برای Applicationهای واقعی ضروری است؟

Applicationهای واقعی دائماً با عملیات‌هایی مواجه‌اند که زمان تکمیل آن‌ها تحت کنترل مستقیم JavaScript نیست.

برای مثال:

Network
Timer
User Interaction
External Resources

اگر هر عملیات زمان‌بر باعث شود JavaScript تا پایان آن متوقف بماند، Application در معرض Blocking قرار می‌گیرد.

پس مسیر طبیعی مسئله چنین است:

External / Long-Running Operation
↓
Waiting is possible
↓
Blocking is undesirable
↓
Need Non-Blocking
↓
Asynchronous Programming

این دقیقاً همان دلیلی است که Asynchronous Programming را به یکی از بخش‌های اساسی JavaScript Runtime تبدیل می‌کند.

مدل ذهنی نهایی

در پایان فصل باید بتوانیم کل مفهوم را بدون وابستگی به Syntax خاصی توضیح دهیم.

JavaScript normally executes code synchronously
↓
Some operations take time
↓
Waiting can block execution
↓
Blocking is undesirable in interactive applications
↓
We need non-blocking behavior
↓
JavaScript execution remains a single execution flow
↓
Host Environment can handle external operations
↓
This enables asynchronous programming

و اکنون یک سؤال طبیعی و اجتناب‌ناپذیر داریم:

اگر Host Environment یک عملیات را بعداً تکمیل کند، نتیجه آن دقیقاً چگونه دوباره وارد جریان اجرای JavaScript می‌شود؟

این سؤال نقطه شروع فصل بعد است:

Chapter 59 — Event Loop

Best Practices
Asynchronous را با Parallel اشتباه نگیرید

Asynchronous بودن به معنی اجرای هم‌زمان چند قطعه JavaScript نیست.

Long-Running را با Blocking یکی ندانید

زمان‌بر بودن یک عملیات با متوقف کردن جریان JavaScript یکسان نیست.

JavaScript و Host Environment را جدا ببینید

Browser APIs را نباید بدون تفکیک مفهومی، بخشی از خود زبان JavaScript در نظر گرفت.

ابتدا مدل ذهنی را بسازید

قبل از یادگیری Promise و async/await باید مسئله‌ای را که این ابزارها برای آن به وجود آمده‌اند درک کنید.

Synchronous و Asynchronous را مقابل یکدیگر قرار ندهید

Application واقعی می‌تواند هم‌زمان از هر دو مدل استفاده کند.

Common Mistakes
اشتباه اول: «JavaScript فقط یک Thread دارد»

این جمله بدون Context دقیق نیست.

Single-Threaded بودن به جریان اجرای JavaScript اشاره دارد، نه اینکه کل Browser فقط یک Thread داشته باشد.

اشتباه دوم: «Asynchronous یعنی Parallel»

این دو مفهوم متفاوت هستند.

Asynchronous درباره عدم انتظار مستقیم است؛ Parallelism درباره اجرای واقعی هم‌زمان عملیات است.

اشتباه سوم: «Non-Blocking یعنی عملیات فوراً تمام می‌شود»

خیر.

عملیات همچنان ممکن است زمان ببرد؛ فقط جریان JavaScript برای تمام مدت آن متوقف نمی‌شود.

اشتباه چهارم: «Asynchronous یعنی JavaScript دیگر Synchronous نیست»

خیر.

بخش زیادی از کد JavaScript همچنان Synchronous اجرا می‌شود.

اشتباه پنجم: «Promise همان Asynchronous JavaScript است»

Promise یک abstraction برای مدیریت نتیجه آینده یک عملیات است؛ خود مفهوم Asynchronous Programming گسترده‌تر از Promise است.

جزئیات Promise در فصل مربوط به آن بررسی خواهد شد.

Summary

JavaScript در حالت معمول، کد را به‌صورت Synchronous اجرا می‌کند.

وقتی یک عملیات زمان‌بر وارد این جریان می‌شود، اگر JavaScript برای تکمیل آن منتظر بماند، ممکن است اجرای برنامه Blocking شود.

در Applicationهای تعاملی، چنین Blockingای می‌تواند مانع ادامه مناسب سایر کارها شود.

در نتیجه به مدلی نیاز داریم که در آن عملیات زمان‌بر بدون توقف جریان اصلی JavaScript ادامه پیدا کند.

این مدل Non-Blocking است.

برای درک آن باید میان JavaScript Execution و Host Environment تفاوت قائل شویم.

JavaScript یک جریان اجرای مستقیم مشخص دارد، در حالی که Host Environment قابلیت‌هایی برای تعامل با محیط اجرا فراهم می‌کند.

ترکیب این مدل با Non-Blocking Execution، پایه Asynchronous Programming را تشکیل می‌دهد.

Asynchronous بودن نیز به معنای Parallel بودن نیست.

اکنون مسئله اصلی را می‌شناسیم؛ اما هنوز سازوکار هماهنگی نتیجه عملیات Async با JavaScript را نمی‌دانیم.

این دقیقاً مسئله‌ای است که Event Loop در فصل بعد توضیح خواهد داد.

Key Takeaways
Synchronous Execution عملیات را در یک جریان ترتیبی اجرا می‌کند.
Long-Running Operation ممکن است زمان قابل‌توجهی برای تکمیل نیاز داشته باشد.
Blocking زمانی رخ می‌دهد که چنین عملیاتی ادامه جریان اجرای موردنظر را متوقف کند.
Non-Blocking اجازه می‌دهد جریان JavaScript برای تمام مدت عملیات متوقف نشود.
JavaScript را باید در کنار Host Environment تحلیل کرد.
Single-Threaded بودن JavaScript با Single-Threaded بودن کل Runtime یکسان نیست.
Asynchronous Programming پاسخ به نیاز مدیریت Non-Blocking عملیات زمان‌بر است.
Asynchronous و Parallel دو مفهوم متفاوت هستند.
Asynchronous Programming به معنی حذف Synchronous Execution نیست.
Event Loop سازوکار هماهنگی اجرای Async را توضیح می‌دهد و موضوع فصل بعد است.
Technical Interview
Junior
Synchronous Execution چیست؟

مدلی از اجرای برنامه است که در آن عملیات در یک جریان مشخص و ترتیبی اجرا می‌شوند و اجرای عملیات بعدی در همان جریان به پیشرفت عملیات قبلی وابسته است.

Blocking چیست؟

Blocking زمانی رخ می‌دهد که یک عملیات مانع ادامه اجرای جریان موردنظر شود تا زمانی که آن عملیات به مرحله لازم برسد.

Non-Blocking چیست؟

مدلی است که در آن شروع یک عملیات زمان‌بر، جریان اجرای JavaScript را مجبور نمی‌کند تا پایان آن عملیات متوقف بماند.

Asynchronous Programming چیست؟

مدلی برای مدیریت عملیات‌هایی است که ممکن است زمان‌بر باشند، به‌گونه‌ای که جریان اجرای برنامه مجبور نباشد برای تمام مدت اجرای آن عملیات متوقف بماند.

Mid-Level
چرا Asynchronous Programming در Browser اهمیت دارد؟

زیرا Browser Application با عملیات‌هایی مانند Network Communication، Timerها و تعامل کاربر سروکار دارد که ممکن است زمان‌بر باشند. اگر اجرای JavaScript برای تمام مدت این عملیات‌ها متوقف شود، Application نمی‌تواند به‌موقع سایر کارها را پردازش کند.

آیا Asynchronous به معنی Parallel است؟

خیر.

Asynchronous به نحوه مدیریت عملیات و عدم انتظار مستقیم برای تکمیل آن مربوط است، در حالی که Parallelism به اجرای واقعی چند عملیات در یک بازه زمانی اشاره دارد.

چرا JavaScript و Host Environment باید از هم تفکیک شوند؟

زیرا JavaScript Language قابلیت‌های زبان را تعریف می‌کند، در حالی که Host Environment قابلیت‌های محیط اجرا را فراهم می‌کند. بسیاری از عملیات‌های موردنیاز Applicationهای Browser از طریق همین محیط در اختیار JavaScript قرار می‌گیرند.

Senior
آیا Single-Threaded بودن JavaScript با Asynchronous بودن آن تناقض دارد؟

خیر.

Single-Threaded بودن به جریان اجرای JavaScript اشاره دارد؛ یعنی چند قطعه JavaScript در همان جریان به‌صورت هم‌زمان اجرا نمی‌شوند. Asynchronous Programming به JavaScript اجازه می‌دهد برای عملیات زمان‌بر جریان اجرای خود را متوقف نکند. تعامل با Host Environment و سازوکار Runtime این دو را با یکدیگر هماهنگ می‌کند.

تفاوت Long-Running Operation و Blocking چیست؟

Long-Running Operation عملیاتی است که تکمیل آن زمان می‌برد. Blocking زمانی رخ می‌دهد که اجرای این عملیات مانع ادامه جریان اجرای موردنظر شود. بنابراین هر عملیات زمان‌بر الزاماً به معنای Blocking نیست.

چرا Event Loop باید بعد از Introduction to Asynchronous JavaScript آموزش داده شود؟

زیرا ابتدا باید مسئله ساخته شود: عملیات زمان‌بر می‌تواند Blocking ایجاد کند و Application به Non-Blocking Execution نیاز دارد. پس از شکل‌گیری این نیاز، Event Loop به‌عنوان سازوکار هماهنگ‌کننده میان اجرای JavaScript و عملیات Async معرفی می‌شود.

Golden Answers
Junior — Golden Answer

Asynchronous JavaScript چیست؟

Asynchronous JavaScript مدلی برای مدیریت عملیات‌هایی است که ممکن است زمان‌بر باشند، به‌گونه‌ای که JavaScript مجبور نباشد جریان اجرای خود را تا پایان آن عملیات متوقف کند. نتیجه عملیات می‌تواند بعداً در اختیار برنامه قرار گیرد.

Mid-Level — Golden Answer

چرا JavaScript به Asynchronous Programming نیاز دارد؟

Applicationهای واقعی با عملیات‌هایی مانند Network Communication و سایر عملیات وابسته به Host Environment سروکار دارند که ممکن است زمان‌بر باشند. اگر JavaScript برای تمام مدت اجرای این عملیات‌ها منتظر بماند، جریان برنامه Blocking می‌شود. Asynchronous Programming امکان مدیریت این عملیات‌ها بدون توقف مستقیم جریان اصلی را فراهم می‌کند.

Senior — Golden Answer

چگونه Single-Threaded بودن JavaScript با Asynchronous Programming سازگار است؟

Single-Threaded بودن به جریان اجرای مستقیم JavaScript اشاره دارد، نه اینکه کل Runtime فقط یک Thread داشته باشد. JavaScript برای عملیات زمان‌بر مجبور نیست جریان اجرای خود را تا پایان آن عملیات متوقف کند؛ Host Environment می‌تواند بخشی از این عملیات را مدیریت کند و Runtime بعداً نتیجه را با جریان JavaScript هماهنگ کند. سازوکار دقیق این هماهنگی توسط Event Loop توضیح داده می‌شود.

Conclusion

مسئله Asynchronous JavaScript از یک نیاز واقعی شروع می‌شود.

JavaScript در حالت معمول عملیات را به‌صورت Synchronous اجرا می‌کند.

اما Applicationهای واقعی با عملیات‌هایی مواجه‌اند که تکمیل آن‌ها ممکن است زمان ببرد.

اگر JavaScript برای تمام مدت چنین عملیاتی منتظر بماند، جریان اجرای Application می‌تواند Blocking شود.

بنابراین به مدلی نیاز داریم که عملیات زمان‌بر را بدون توقف جریان اصلی مدیریت کند.

از اینجا مسیر مفهومی ما شکل می‌گیرد:

Synchronous Execution
↓
Long-Running Operation
↓
Blocking
↓
Need for Non-Blocking
↓
Single JavaScript Thread
↓
Host Environment
↓
Asynchronous Programming

اکنون مسئله را می‌شناسیم.

JavaScript باید بتواند با عملیات‌هایی کار کند که پایان آن‌ها فوراً مشخص نیست، بدون اینکه برای تمام مدت اجرای آن‌ها متوقف شود.

اما هنوز یک سؤال اساسی باقی مانده است:

وقتی عملیات Async در Host Environment تمام می‌شود، چه چیزی تعیین می‌کند نتیجه آن چه زمانی دوباره وارد جریان اجرای JavaScript شود؟

این سؤال، ما را به Event Loop می‌رساند.

و این دقیقاً نقطه شروع Chapter 59 — Event Loop است.