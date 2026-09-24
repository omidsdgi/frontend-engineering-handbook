
Chapter 56 — DOM Traversing
Core Question
چگونه از یک DOM Element به Elementهای مرتبط برسیم؟
Element→  Parent →  Children →  Siblings →  closest →  matches →  Traversal Patterns________________________________________
مقدمه
در فصل‌های قبل یاد گرفتیم چگونه  Elementهای DOM را پیدا کنیم و چگونه آن‌ها را تغییر دهیم. اما در Applicationهای واقعی همیشه از Document شروع نمی‌کنیم. گاهی یک Element را از قبل در اختیار داریم و مسئله این است که از همان Element به بخش مرتبط دیگری از UI برسیم. فرض کنید در یک Recipe Application روی یک Button کلیک شده است:								<article class="recipe-card">
  <div class="recipe-content">
    <h2 class="recipe-title">Pasta</h2>
    <button class="recipe-action">View recipe</button>
اگر Button را داشته باشیم، ممکن است به Card مربوط به آن نیاز داشته باشیم.			       </div> 
در اینجا دوباره جستجو کردن کل Document لزوماً مسئله را به‌درستی بیان نمی‌کند.		    </article>
ما از قبل یک نقطه شروع داریم.  Related Element →   Current Element 
 DOM برای چنین حرکتی، روابط ساختاری مشخصی در اختیار ما قرار می‌دهد. این همان  DOM Traversing است.________________________________________
از Element موجود به Element مرتبط
DOM  یک Tree است. هر Element در این Tree جایگاه مشخصی دارد و با  Elementهای دیگر رابطه دارد. 
یک Element می‌تواند: 
Parent  داشته باشد.	 Child داشته باشد.	Sibling  داشته باشد.    در مسیر Ancestorهای خود قرار داشته باشد.
بنابراین اگر Element فعلی را داشته باشیم، لازم نیست همیشه دوباره از Document شروع کنیم. می‌توانیم از موقعیت فعلی آن در Tree استفاده کنیم. این تفاوت، نقطه شروع فهم Traversing است:
Find Element →  Document→  Selection 	     در مقابل  Traversing: 
Related Element →  Move through DOM relationship→ Current Element
 Selection درباره پیدا کردن است.  Traversing درباره حرکت از چیزی که پیدا کرده‌ایم است.________________________________________
Parent؛ حرکت به سمت بالا
اولین رابطه‌ای که معمولاً به آن نیاز داریم Parent است. در مثال بالا اگر Button را در اختیار داشته باشیم:
const button = document.querySelector('.recipe-action')			می‌توانیم Parent Element آن را با 
button .parentElement به دست آوریم. یعنی: Parent Element  این Element را بده. این رابطه در بسیاری از UIها طبیعی است. یک Button ممکن است بخشی از یک Container باشد و Logic برنامه به همان Container نیاز داشته باشد.________________________________________
چرا  parentElement و نه همیشه parentNode؟
 DOM فقط از Elementها تشکیل نشده است. Element یکی از انواع  Node است و DOM می‌تواند  Nodeهای 
دیگری مانند Text Node نیز داشته باشد. به همین دلیل دو API متفاوت داریم: element .parentElement و 
element .parentNode
 parentElement مشخصاً Parent ای را هدف قرار می‌دهد که یک Element باشد. parentNode  رابطه را در سطح عمومی‌تر Node بیان می‌کند. برای مثال، وقتی هدف ما حرکت در UI بین  Elementهای HTML است، parentElement مدل ذهنی مستقیم‌تری دارد. نکته مهم این است که اگر Parent Element وجود نداشته باشد، parentElement  می‌تواند null  باشد. پس Traversing همیشه باید مرزهای Tree را نیز در نظر بگیرد.________________________________________
Children؛ حرکت به سمت پایین
حال مسئله را برعکس کنیم. اگر Parent را داشته باشیم و بخواهیم  Elementهای داخل آن را بررسی کنیم، به Children  نیاز داریم. فرض کنید در مثال قبلی Container را داشته باشیم:
const content = document.querySelector('.recipe-content') 					recipe-content
می‌توانیم  Child Elementهای آن را با  content .children دریافت کنیم.				      ├── h2
در اینجا  children همان Child Elementها را هدف قرار می‌دهد.				      └── button
________________________________________
 children و childNodes
اینجا یکی از تفاوت‌های مهم DOM ظاهر می‌شود. HTML  فقط Tagها نیست. Whitespaceها و  Line Breakهای موجود در Markup نیز می‌توانند به‌صورت Text Node در DOM وجود داشته باشند. به همین دلیل element .childNodes تمام  Child Nodeها را در سطح Node در نظر می‌گیرد. اما element .children فقط Child Elementها را برمی‌گرداند . پس مدل ذهنی این دو چنین است: Child Elementها→ children		         تمام Child Nodeها→ childNodes وقتی مسئله ما Navigation بین  Elementهای HTML است،  children معمولاً دقیقاً همان رابطه‌ای را بیان می‌کند که به آن نیاز داریم. ________________________________________
وقتی فقط یک Child را می‌خواهیم
گاهی به تمام Children نیاز نداریم. فقط می‌خواهیم بدانیم اولین یا آخرین Child Element کدام است. برای این کار element .firstElementChild   و:           element .lastElementChild راداریم. مثلا: <div class="recipe-content">       
  <h2 class="recipe-title">Pasta</h2>
  <p>Italian recipe</p>
  <button>View recipe</button>
</div>
در این ساختار: content .firstElementChild به  h2 اشاره می‌کند. و content .lastElementChild به  button اشاره می‌کند. اما اینجا باید یک تفاوت مهندسی را در نظر بگیریم.  firstElementChild درباره موقعیت صحبت می‌کند: 
اولین Child Element نه درباره معنای آن: عنوان Recipe
اگر فردا یک Icon یا Element جدید قبل از h2  قرار گیرد، اولین Child تغییر خواهد کرد. بنابراین Traversing مبتنی بر Position باید زمانی استفاده شود که Position واقعاً بخشی از منطق UI باشد. ________________________________________
Siblings؛ حرکت در همان سطح
فرض کنید ساختار زیر را داریم:						<div class="recipe-content">
  <h2 class="recipe-title">Pasta</h2>
  <p class="recipe-description">Italian recipe</p>
  <button class="recipe-action">View recipe</button>
</div>
سه Element داخل Container وجود دارند:    h2     p	   button        همه آن‌ها یک Parent مشترک دارند. بنابراین Sibling  یکدیگر هستند. اگر  h2 را داشته باشیم و بخواهیم Element بعدی را پیدا کنیم: title .nextElementSibling 
به p  می‌رسیم و اگر Button را داشته باشیم: button .previousElementSibling به p  می‌رسیم.  پس:
previousElementSibling ← Current → nextElementSibling به ما اجازه می‌دهد در همان سطح DOM حرکت کنیم. ________________________________________
چرا ElementSibling؟
در سطح  Node نیز API هایی مانند : 	 	nextSibling		previousSibling	      وجود دارند. 
اما این APIها فقط Element را هدف نمی‌گیرند. اگر هدف ما حرکت بین  Elementهای HTML باشد، APIهای : nextElementSibling		previousElementSibling	رابطه موردنظر را دقیق‌تر بیان می‌کنند. بنابراین همان اصل قبلی دوباره ظاهر می‌شود: API  را بر اساس رابطه‌ای انتخاب کنید که واقعاً برای Logic شما مهم است. ________________________________________
مسئله مهم‌تر: وقتی تعداد Parentها را نمی‌دانیم
تا اینجا حرکت به Parent ساده بود: element .parentElement 			اما فرض کنید UI پیچیده‌تر شود:
<article class="recipe-card">
  <div class="recipe-content">
    <div class="recipe-actions">
      <button class="recipe-action">
Button  را داریم، اما Card را می‌خواهیم. می‌توانیم سه بار به Parent برویم:			 	View recipe
button .parentElement .parentElement .parentElement;   اما این کد یک مشکل مهم دارد.	   </button>
 Logic ما در واقع نمی‌خواهد: سه Parent به بالا حرکت کند.					          </div>
Logic  ما می‌خواهد:  Recipe Card مرتبط با این Button را پیدا کند. این دو جمله یکسان نیستند.	       </div>
اولی درباره Position  صحبت می‌کند. دومی درباره Meaning . اینجاست که  closest() وارد می‌شود. 	 </article>
________________________________________
closest()؛ حرکت بر اساس Meaning
به‌جای وابستگی به تعداد Wrapperها، می‌توانیم رابطه موردنظر را با Selector بیان کنیم:
const card = button .closest('.recipe-card')
 closest() از Element فعلی شروع می‌کند و در مسیر Ancestorها حرکت می‌کند تا نزدیک‌ترین Element مطابق Selector  را پیدا کند. مدل ذهنی:		    button → .recipe-actions → .recipe-content → .recipe-card وقتی به .recipe-card برسد، همان Element را برمی‌گرداند. بنابراین:	     button .closest('.recipe-card') یعنی: از این Element شروع کن و در مسیر Ancestorها، نزدیک‌ترین Element مطابق .recipe-card  را پیدا کن. ________________________________________
closest() چرا از parentElement مهم‌تر می‌شود؟
مزیت اصلی  closest() این نیست که Syntax کوتاه‌تری دارد. مزیت اصلی این است که رابطه موردنظر را بیان می‌کند. این:         button .parentElement .parentElement .parentElement 	  می‌گوید: سه سطح به بالا برو. اما این: button .closest('.recipe-card') 	می‌گوید:	    Card مرتبط را پیدا کن.
اگر فردا یک Wrapper جدید به DOM اضافه شود، تعداد  Parentها تغییر می‌کند. اما تا زمانی که Card همچنان با .recipe-card  قابل شناسایی باشد،  Logic ما به تعداد  Wrapperها وابسته نیست.
این همان تفاوت مهم بین:	 Positional Traversing		و:	Semantic Traversing	است.________________________________________
closest() از خود Element نیز شروع می‌شود
یک نکته دقیق وجود دارد.  closest() جستجو را از Parent شروع نمی‌کند. ابتدا خود Element را بررسی می‌کند. 
برای مثال: card .closest('.recipe-card') 	  	اگر  card خودش با .recipe-card  مطابقت داشته باشد، همان 
card  برگردانده می‌شود. سپس اگر مطابقت نداشته باشد، Traversing  به سمت Parent ادامه پیدا می‌کند. اگر هیچ Ancestor  مطابق Selector پیدا نشود، نتیجه: 	null	خواهد بود. بنابراین closest()  را می‌توان چنین مدل کرد:
Current Element → matches selector? → (Yes → return) → No → Parent → Continue________________________________________
matches()؛ وقتی فقط می‌خواهیم Element را بررسی کنیم
اکنون یک سؤال متفاوت داریم. فرض کنید Element را داریم و فقط می‌خواهیم بدانیم: آیا این Element همان نوع Elementی است که به دنبال آن هستیم؟ برای این کار  matches() را داریم.       element .matches('.recipe-action') اگر Element با Selector مطابقت داشته باشد:     true   و اگر مطابقت نداشته باشد:	 false	برگردانده می‌شود. 
تفاوت آن با  closest() مهم است. 
element .matches('.recipe-card')    یعنی: آیا خود این Element یک Recipe Card است؟
 اما: element .closest('.recipe-card')     یعنی: نزدیک‌ترین Recipe Card در مسیر این Element و Ancestorهای آن کدام است؟				پس:  → Check matches()			→ Find closest()
matches()  حرکت نمی‌کند. فقط یک شرط درباره Element فعلی ایجاد می‌کند. ________________________________________
از APIها به یک مدل Traversal
اکنون می‌توانیم کل مسیر فصل را بدون حفظ کردن Methodهای جداگانه ببینیم.
اگر به سمت بالا نیاز داریم: element .parentElement
اگر به سمت پایین نیاز داریم: element .children
اگر اولین یا آخرین Child مهم است: element .firstElementChild		element .lastElementChild
اگر در همان سطح حرکت می‌کنیم: element .nextElementSibling	element .previousElementSibling
اگر یک Ancestor را بر اساس Meaning می‌خواهیم: element .closest('.selector')
و اگر فقط می‌خواهیم خود Element را بررسی کنیم: element .matches('.selector');
بنابراین مسئله اصلی حفظ کردن این Methodها نیست. مسئله این است که ابتدا رابطه موردنیاز را تشخیص دهیم.
Parent? → parentElement 	Children? → children			          Matching Ancestor? → closest()
Sibling? →( nextElementSibling – previousElementSibling ) 	            Current Element Match? → matches()
این همان مدل ذهنی‌ای است که DOM Traversing باید ایجاد کند.________________________________________
Traversal Patterns در UI واقعی
اکنون می‌توانیم از این روابط برای ساخت Patternهای واقعی استفاده کنیم. فرض کنید Event مربوط به یک Button را دریافت کرده‌ایم و Element موردنظر ما Card مربوط به آن است. ساختار:				     recipe-list
   └── recipe-card
          └── button
ممکن است Event Target دقیقاً Button نباشد و یک Element داخلی آن باشد. اما Logic ما به Card نیاز دارد.
در این حالت: const card = event .target .closest('.recipe-card') 	یک Traversal طبیعی انجام می‌دهد:
Event Target → closest() → Recipe Card
این Pattern به‌خصوص در UIهایی که تعداد زیادی Element مشابه دارند، مفید است؛ زیرا  Logic بر اساس رابطه بین Target  و Container نوشته می‌شود، نه بر اساس تعداد  Wrapperهای DOM . ________________________________________
 Position یا  Meaning؟
در این مرحله یک اصل مهم را می‌توانیم استخراج کنیم. همه  Traversalها بد نیستند. مسئله این است که بدانیم 
چه نوع رابطه‌ای را بیان می‌کنند. 
اگر واقعاً می‌خواهیم: اولین Child را بگیریم. 	این کد کاملاً مناسب است:          container .firstElementChild
اگر واقعاً می‌خواهیم:  Sibling بعدی را بگیریم.  	این کد کاملاً مناسب است:          element .nextElementSibling  
اما اگر مسئله این است: Card  مرتبط با این Button را پیدا کن. وابستگی به تعداد  Parentها مناسب نیست. در اینجا:
element .closest('.recipe-card') رابطه را بهتر بیان می‌کند.
بنابراین قاعده کلی این نیست که: همیشه از closest()  استفاده کن. قاعده درست این است:
Traversal  را بر اساس رابطه واقعی مسئله انتخاب کن.________________________________________
DOM Traversing  و Selection جایگزین یکدیگر نیستند
ممکن است در بعضی موقعیت‌ها هم querySelector()  و هم Traversing بتوانند به نتیجه برسند. اما مدل مسئله متفاوت است. اگر بگوییم: در Document اولین  .recipe-card را پیدا کن. مسئله Selection است:
document.querySelector('.recipe-card')
اما اگر بگوییم: Card  مربوط به این Button را پیدا کن. مسئله Traversing است:      button .closest('.recipe-card')
در کدهای مهندسی، این تفاوت مهم است؛ زیرا API انتخاب‌شده باید Intent کد را نیز منتقل کند.________________________________________
Best Practices
1.	رابطه را قبل از API مشخص کنید
ابتدا بپرسید: به Parent نیاز دارم؟	  Child ؟	 Sibling؟	 Ancestor؟ 	یا فقط بررسی تطابق؟ سپس API مناسب را انتخاب کنید.________________________________________
2.	برای Ancestor معنایی از  closest() استفاده کنید.
اگر هدف یک Container مشخص است، وابستگی به تعداد parentElementها ایجاد نکنید.________________________________________
3.	 Element و Node را با هم اشتباه نگیرید.
برای UI Traversing معمولاً API های Element-oriented انتخاب واضح‌تری هستند.________________________________________
4.	 Position را فقط زمانی استفاده کنید که Position بخشی از Logic باشد.
firstElementChild  یعنی اولین Child ، نه «عنوان» ،  «Button» یا «Card» .________________________________________
5.	نتیجه Traversing را همیشه قطعی فرض نکنید.
بعضی Traversalها می‌توانند به  null برسند؛ مخصوصاً  closest() زمانی که Match وجود نداشته باشد. ________________________________________
6.	Traversal را بیش از حد زنجیره‌ای نکنید.
زنجیره‌هایی مانند: element .parentElement .parentElement .parentElement
وابستگی زیادی به ساختار فعلی DOM ایجاد می‌کنند. ________________________________________
common mistake
اشتباه اول: یکی دانستن Selection و Traversing
 querySelector() و Traversing هر دو می‌توانند Element پیدا کنند، اما از نقطه شروع و با مدل متفاوتی کار می‌کنند.________________________________________
اشتباه دوم: یکی دانستن children  و childNodes
 children فقط Elementها را هدف قرار می‌دهد؛  childNodes تمام Nodeها را شامل می‌شود.________________________________________
اشتباه سوم: استفاده از parentElement  برای یک رابطه معنایی
اگر هدف  Card، Form  یا Container مشخصی است، تعداد  Parentها معیار خوبی برای بیان رابطه نیست.________________________________________
اشتباه چهارم: تصور اینکه matches()  جستجو می‌کند
 matches() فقط خود Element را بررسی می‌کند . ________________________________________
اشتباه پنجم: تصور اینکه closest()  فقط Parent را بررسی می‌کند
 closest() ابتدا خود Element را بررسی می‌کند و سپس به  Ancestorها حرکت می‌کند. ________________________________________
اشتباه ششم: وابستگی بی‌دلیل به  Position
اگر Logic می‌گوید  «Recipe Title»، استفاده از  firstElementChild فقط به این دلیل که Title فعلاً اولین Child است، وابستگی غیرضروری ایجاد می‌کند. ________________________________________
Summary
DOM Traversing  زمانی مطرح می‌شود که یک Element را در اختیار داریم و می‌خواهیم از جایگاه آن در DOM  Tree  برای رسیدن به  Elementهای مرتبط استفاده کنیم. از parentElement  برای حرکت به Parent Element استفاده می‌کنیم. از  children برای دسترسی به Child Elementها استفاده می‌کنیم. firstElementChild  و lastElementChild  اولین و آخرین Child Element را در اختیار قرار می‌دهند. nextElementSibling  و previousElementSibling  برای حرکت بین Sibling Elementها استفاده می‌شوند.
وقتی Ancestor موردنظر را بر اساس یک Selector می‌شناسیم،  closest() انتخاب مناسبی است؛ زیرا به‌جای تعداد لایه‌های  DOM، رابطه معنایی موردنظر را بیان می‌کند. matches()  نیز زمانی استفاده می‌شود که فقط می‌خواهیم بدانیم Element  فعلی با یک Selector مطابقت دارد یا خیر.
بنابراین DOM Traversing بیش از آنکه مجموعه‌ای از Methodها باشد، یک روش Navigation در DOM Tree است.
________________________________________
Key Takeaways
•	 DOM یک Tree از Nodeهاست.
•	هر Element جایگاه و روابط مشخصی در این Tree دارد.
•	Traversing  از یک Element موجود شروع می‌شود.
•	parentElement  برای حرکت به Parent Element است.
•	 children فقط Child Elementها را شامل می‌شود.
•	childNodes  در سطح Node کار می‌کند.
•	 nextElementSibling و  previousElementSibling بین Sibling Elementها حرکت می‌کنند.
•	closest()  نزدیک‌ترین Element مطابق Selector را در مسیر خود Element و Ancestorهای آن پیدا می‌کند.
•	matches()  فقط Element فعلی را بررسی می‌کند.
•	 Traversing مبتنی بر Position به ساختار دقیق DOM وابسته‌تر است.
•	Traversing مبتنی بر Meaning معمولاً Intent کد را بهتر بیان می‌کند.
•	انتخاب API باید بر اساس رابطه واقعی موردنیاز انجام شود، نه صرفاً کوتاه‌ترین Syntax .
________________________________________
Technical Interview
Junior-Level
1. DOM Traversing چیست؟
 حرکت از یک Node یا Element موجود در DOM به Elementهای مرتبط مانند Parent، Child، Sibling یا Ancestor است.
2. تفاوت children و childNodes چیست؟
children فقط Child Elementها را برمی‌گرداند، در حالی که childNodes تمام Child Nodeها را شامل می‌شود.
3. چگونه Parent یک Element را پیدا می‌کنیم؟
element.parentElement;
4. چگونه به Sibling بعدی می‌رسیم؟
element.nextElementSibling;
و برای Sibling قبلی:
element.previousElementSibling;
________________________________________
Mid-Level
5. تفاوت closest() و querySelector() چیست؟
querySelector() از Root مشخصی به سمت Descendantها جستجو می‌کند، اما closest() از Element فعلی شروع می‌کند و به سمت Ancestorها حرکت می‌کند.
6. چرا این کد شکننده است؟
button.parentElement.parentElement.parentElement;
زیرا به تعداد دقیق لایه‌های DOM وابسته است. تغییر Wrapperها می‌تواند نتیجه Traversing را تغییر دهد.
7. matches() چه کاری انجام می‌دهد؟
بررسی می‌کند که Element فعلی با CSS Selector مشخص‌شده مطابقت دارد یا خیر و true یا false برمی‌گرداند.
8. چرا nextElementSibling با nextSibling متفاوت است؟
nextElementSibling فقط به Sibling از نوع Element حرکت می‌کند، در حالی که nextSibling در سطح Node عمل می‌کند.
________________________________________
Senior-Level
9. چه زمانی closest() انتخاب بهتری از زنجیره parentElement است؟
وقتی هدف یک Ancestor معنایی است و تعداد لایه‌های بین Element فعلی و آن Ancestor ممکن است تغییر کند. closest() رابطه را با Selector بیان می‌کند.
10. آیا closest() وابستگی به DOM را حذف می‌کند؟
خیر. همچنان به Selector و ساختار معنایی DOM وابسته است؛ اما وابستگی به تعداد دقیق Wrapperها را کاهش می‌دهد.
11. چه زمانی Traversing مبتنی بر Position مناسب است؟
زمانی که Position واقعاً بخشی از منطق UI باشد؛ مثلاً وقتی «اولین Child» یا «Sibling بعدی» دقیقاً همان چیزی است که برنامه نیاز دارد.
12. چرا matches() در Traversal Patternها مفید است؟
زیرا می‌تواند مشخص کند Element فعلی با Selector موردنظر مطابقت دارد یا خیر و بر اساس آن Logic Traversal تصمیم بگیرد که چه کاری انجام شود.
________________________________________
Golden Answers
DOM Traversing در یک جمله چیست؟
DOM Traversing یعنی حرکت از یک Element موجود به Elementهای مرتبط بر اساس روابط ساختاری DOM مانند Parent، Child، Sibling و Ancestor.
closest() چیست؟
closest() از Element فعلی شروع می‌کند و در مسیر Ancestorها نزدیک‌ترین Element مطابق Selector را پیدا می‌کند.
matches() چیست؟
matches() بررسی می‌کند که Element فعلی با یک CSS Selector مطابقت دارد یا خیر.
تفاوت Selection و Traversing چیست؟
Selection برای پیدا کردن Element از یک Root مانند Document استفاده می‌شود؛ Traversing از Element موجود شروع می‌کند و در روابط DOM حرکت می‌کند.
چرا closest() معمولاً از چند parentElement بهتر است؟
زیرا به‌جای تعداد لایه‌های DOM، رابطه معنایی موردنظر را با Selector بیان می‌کند و در نتیجه نسبت به تغییر تعداد Wrapperها مقاوم‌تر است.
________________________________________
Conclusion
وقتی یک Element را در DOM در اختیار داریم، آن Element یک نقطه جداافتاده نیست.
بخشی از یک Tree است.
از آن نقطه می‌توانیم:
          Parent
             ↑
             │
Previous ← Element → Next
             │
             ↓
          Children
حرکت کنیم.
اگر رابطه موردنظر یک Ancestor مشخص باشد، می‌توانیم آن رابطه را با Selector بیان کنیم:
element.closest('.recipe-card');
و اگر فقط بخواهیم Element فعلی را بررسی کنیم:
element.matches('.recipe-card');
اما مهم‌ترین نکته فصل حفظ کردن این APIها نیست.
مدل ذهنی درست این است:
ابتدا رابطه موردنیاز را تشخیص بده؛ سپس APIای را انتخاب کن که همان رابطه را دقیق‌ترین شکل بیان می‌کند.
اگر مسئله Position است، Traversing مبتنی بر Position مناسب است.
اگر مسئله Meaning است، رابطه را با Selector بیان کنید.
به این ترتیب DOM Traversing از مجموعه‌ای از Methodهای پراکنده به یک ابزار مهندسی برای Navigation در DOM تبدیل می‌شود.
