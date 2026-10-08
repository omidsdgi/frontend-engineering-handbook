# PROJECT_SPEC_FINAL — React Book

# Frontend Engineering Handbook
## AI Authoring Specification — React, Ecosystem and Frameworks

**Version:** 1.0

---

# هدف این سند

این سند مرجع اصلی تولید محتوای کتاب **Frontend Engineering Handbook — React** است.

این سند برای تولید محتوای آموزشی توسط مدل‌های زبانی تهیه شده است تا تمام فصل‌ها از نظر ساختار، کیفیت، عمق فنی، ترتیب مفاهیم و سبک آموزشی یکپارچه باشند.

این Specification بر پایه اصول عمومی کتاب JavaScript بنا شده است، اما قوانین اختصاصی React، React Ecosystem و Frameworkهای مدرن را نیز تعریف می‌کند.

هرگاه میان دستورهای کاربر و این سند تعارضی وجود داشت، مگر آنکه کاربر صریحاً خلاف آن را درخواست کرده باشد، این سند ملاک اصلی تولید محتوای React خواهد بود.

---

# رابطه این سند با کتاب JavaScript

این کتاب ادامه منطقی کتاب JavaScript است.

بنابراین اصول زیر از کتاب JavaScript حفظ می‌شوند:

- Concept First
- Why Before How
- Mental Model
- Engineering Thinking
- No Memorization
- Practical Knowledge
- Zero Noise
- Complete But Concise
- Dependency Rule
- Scope Rule
- Learning Flow
- Narrative Flow
- کیفیت علمی
- کیفیت آموزشی
- کیفیت مهندسی
- Technical Interview

اما React نیازهای آموزشی جدیدی ایجاد می‌کند.

بنابراین این سند علاوه بر اصول مشترک، قوانین اختصاصی زیر را تعریف می‌کند:

- React Mental Model
- Component Thinking
- Declarative UI
- Rendering Model
- State and Data Flow
- Component Composition
- Hooks
- Server/Client Boundaries
- Routing
- Data Fetching
- Forms
- Async UI
- Error Handling
- React Ecosystem
- Framework Thinking
- Next.js
- Production Architecture

---

# هدف کتاب

هدف کتاب صرفاً آموزش React API نیست.

هدف، ساختن مدل ذهنی یک **Frontend Engineer مدرن** است که بتواند React را به‌عنوان یک ابزار برای ساخت رابط‌های کاربری و Applicationهای واقعی تحلیل و استفاده کند.

خواننده پس از مطالعه باید بتواند:

- توضیح دهد React چه مسئله‌ای را حل می‌کند.
- مدل Declarative UI را درک کند.
- Component را به‌عنوان واحد طراحی UI تحلیل کند.
- Rendering و Re-rendering را توضیح دهد.
- State و Data Flow را طراحی کند.
- میان State، Props و Derived Data تفاوت بگذارد.
- Componentها را به‌صورت قابل نگهداری Composition کند.
- Hooks را بر اساس نیاز و نه حفظ API استفاده کند.
- رفتار یک React Application را تحلیل کند.
- Routing، Data Fetching و Form Handling را در جای درست معماری قرار دهد.
- تفاوت React Core، Library، Ecosystem و Framework را تشخیص دهد.
- نقش ابزارها و کتابخانه‌هایی مانند React Router را توضیح دهد.
- جایگاه Frameworkهایی مانند Next.js را در معماری یک Application درک کند.
- یک React Application واقعی را از نظر Architecture تحلیل کند.
- انتخاب ابزار را بر اساس نیاز فنی انجام دهد.
- در مصاحبه‌های فنی React و Frontend پاسخ دقیق و مستدل ارائه کند.

---

# مخاطب کتاب

مخاطب اصلی:

- Junior Frontend Developer
- Mid-Level Frontend Developer

مخاطب ثانویه:

- دانشجوی علوم کامپیوتر
- توسعه‌دهندگان JavaScript که قصد ورود به React دارند.
- برنامه‌نویسان سایر زبان‌ها که JavaScript را آموخته‌اند و می‌خواهند وارد Frontend مدرن شوند.

این کتاب برای فردی که هیچ آشنایی با JavaScript ندارد نوشته نمی‌شود.

پیش‌نیاز اصلی کتاب:

- JavaScript Fundamentals
- Functions
- Objects
- Arrays
- Scope
- Closures
- Async JavaScript
- Modules
- مفاهیم پایه Browser و Web

در صورت نیاز، مفاهیم JavaScript فقط به‌اندازه لازم مرور می‌شوند و آموزش کامل آن‌ها به کتاب JavaScript واگذار می‌شود.

---

# جایگاه React در کتاب

React نباید به‌عنوان یک مجموعه API مستقل آموزش داده شود.

مدل آموزشی باید از این مسیر پیروی کند:

```text
Web UI Problem
↓
UI Complexity
↓
Declarative UI
↓
React Mental Model
↓
Component
↓
Props
↓
State
↓
Rendering
↓
Events
↓
Component Composition
↓
Hooks
↓
Application State and Data Flow
↓
Application Concerns
↓
Routing
↓
Data Fetching
↓
Forms
↓
Async UI
↓
Error Handling
↓
React Ecosystem
↓
Framework Thinking
↓
Next.js
↓
Production React Applications
```

این نمودار Concept Flow کلی کتاب است.

نقشه راه فصل‌ها باید این جریان را به Concept Flowهای دقیق‌تر تبدیل کند.

---

# React یک Library است

در متن کتاب باید از نظر فنی میان این مفاهیم تفاوت گذاشته شود:

- React
- Library
- Router
- State Management Library
- Data Fetching Library
- Build Tool
- Framework

React نباید بدون توضیح فنی به‌عنوان Framework معرفی شود.

React عمدتاً مسئول ساخت UI و مدل Component-based آن است.

نیازهای دیگری مانند Routing، Data Fetching، Rendering Strategy، Deployment و Application Architecture ممکن است توسط ابزارها یا Frameworkهای دیگر تأمین شوند.

---

# Frameworkها بخش فرعی کتاب نیستند

Frameworkهای مدرن باید بخشی جدی از مسیر یادگیری باشند.

در میان Frameworkها، **Next.js جایگاه ویژه و محوری دارد**.

بنابراین Next.js نباید صرفاً به‌عنوان یک فصل کوتاه در پایان کتاب معرفی شود.

با این حال، Next.js نباید پیش از ساخته شدن Mental Model صحیح React آموزش داده شود.

قاعده:

```text
React Mental Model
↓
React Application Development
↓
Understanding Application Boundaries
↓
Framework Problem
↓
Next.js
↓
Production Application Architecture
```

کتاب باید ابتدا به خواننده توضیح دهد چه مسئله‌ای باعث نیاز به Framework می‌شود و سپس نشان دهد Next.js چگونه بخشی از این مسائل را حل می‌کند.

---

# React Ecosystem

اکوسیستم React نباید به شکل فهرستی از Libraryها آموزش داده شود.

هر ابزار باید از یک **نیاز واقعی** متولد شود.

الگوی صحیح:

```text
Application Need
↓
Problem
↓
Limitation of Current Approach
↓
New Concept
↓
Tool / Library
↓
Trade-off
```

برای مثال:

```text
Navigation Problem
↓
Need for URL-based UI
↓
Routing
↓
Router
↓
React Router
```

یا:

```text
Application Complexity
↓
State Coordination Problem
↓
State Management
↓
State Management Libraries
```

نام یک Library به‌تنهایی دلیل آموزش آن نیست.

---

# Next.js به‌عنوان Framework

Next.js باید به‌عنوان یک Framework برای ساخت Applicationهای React تحلیل شود، نه صرفاً مجموعه‌ای از APIها.

موضوعات Next.js باید در پاسخ به نیازهای Application مطرح شوند.

برای مثال:

```text
React Application
↓
Need for Routing
↓
Application Routing
↓
Framework-level Routing
```

یا:

```text
React UI
↓
Need for Server Rendering / Server-side Capabilities
↓
Rendering Strategy
↓
Framework
↓
Next.js
```

یا:

```text
Application
↓
Need for Data Access and Server Boundaries
↓
Server-side Capabilities
↓
Next.js Architecture
```

جزئیات دقیق Next.js باید مطابق نقشه راه و نسخه مورد استفاده کتاب تعیین شوند و نباید از یک Version خاص بدون دلیل به کل کتاب تعمیم داده شوند.

---

# فلسفه اصلی آموزش

کتاب باید پاسخ دهد:

> چرا React و ابزارهای اطراف آن وجود دارند؟

نه فقط:

> Syntax این API چیست؟

هر مفهوم باید ابتدا مسئله‌ای را ایجاد یا آشکار کند و سپس راه‌حل معرفی شود.

الگوی اصلی:

```text
Need
↓
Problem
↓
Definition
↓
Explanation
↓
Consequence
↓
New Need
↓
New Concept
```

این الگو جایگزین آموزش فهرست‌وار APIها است.

---

# اصل اول — Concept First

هر مفهوم قبل از Syntax آموزش داده شود.

اشتباه:

```text
useState(...)
useEffect(...)
```

و سپس توضیح اینکه State و Side Effect چه هستند.

صحیح:

```text
UI State Problem
↓
State Concept
↓
Need for Persistent Component Data
↓
useState
```

---

# اصل دوم — Why Before How

ابتدا:

> چرا این مفهوم لازم است؟

سپس:

> این مفهوم چیست؟

و در نهایت:

> چگونه از آن استفاده می‌کنیم؟

---

# اصل سوم — Mental Model

هر فصل باید حداقل یک Mental Model مهم در ذهن خواننده ایجاد یا اصلاح کند.

نمونه Mental Modelهای مهم React:

- Declarative UI
- Component as a UI Unit
- Props as Input
- State as Component-owned changing data
- Rendering as UI calculation
- Re-rendering as another render calculation
- One-way data flow
- Composition
- Hook as a mechanism for using React capabilities from components
- Server/Client Boundary
- Framework as an Application-level solution

---

# اصل چهارم — Engineering Thinking

کتاب نباید خواننده را به حفظ APIها تشویق کند.

خواننده باید بتواند از روی مسئله تصمیم بگیرد:

- آیا این داده State است؟
- آیا این داده Derived Data است؟
- آیا باید به Parent منتقل شود؟
- آیا Composition مناسب‌تر است؟
- آیا Context لازم است؟
- آیا State Management Library لازم است؟
- آیا Data Fetching باید در Client انجام شود یا Server؟
- آیا Routing باید توسط React Router انجام شود یا Framework؟
- آیا پروژه به Framework نیاز دارد؟
- آیا Next.js برای این نیاز مناسب است؟

---

# اصل پنجم — No Memorization

خواننده نباید Hookها، Props یا APIها را به‌صورت فهرست حفظ کند.

هر API باید در ارتباط با مسئله‌ای مشخص معرفی شود.

هدف:

```text
Problem → Reasoning → Concept → API
```

نه:

```text
API → Syntax → Memorization
```

---

# اصل ششم — Practical Knowledge

تمام مفاهیم باید کاربرد واقعی داشته باشند.

مثال‌های ترجیحی:

- User
- Product
- Recipe
- Cart
- Order
- Search
- Dashboard
- Form
- API
- Authentication
- Navigation
- Server Data
- Application State

مثال‌هایی که صرفاً برای نمایش Syntax ساخته شده‌اند تا حد امکان حذف شوند.

---

# اصل هفتم — Zero Noise

هیچ مطلبی صرفاً برای افزایش حجم کتاب وارد نشود.

تاریخچه، جزئیات کم‌ارزش، APIهای کم‌کاربرد و مقایسه‌های بدون ارزش مهندسی نباید فضای کتاب را اشغال کنند.

---

# اصل هشتم — Complete But Concise

کتاب باید از نظر مفهومی کامل باشد، اما از تکرار جلوگیری کند.

اگر مفهومی قبلاً در JavaScript Book آموزش داده شده است، در React Book فقط در صورت نیاز مرور شود.

---

# اصل نهم — Library / Ecosystem Awareness

هر بار که یک ابزار یا Library معرفی می‌شود، جایگاه آن در Ecosystem باید روشن باشد.

برای هر ابزار در صورت اهمیت باید مشخص شود:

- چه مسئله‌ای را حل می‌کند؟
- در چه لایه‌ای قرار دارد؟
- چه چیزی را حل نمی‌کند؟
- جایگزین چه روشی است؟
- چه Trade-offهایی دارد؟
- آیا جزء React Core است یا خارج از آن؟
- آیا جزء Framework است یا Library مستقل؟

---

# اصل دهم — Framework Awareness

کتاب نباید React را جدا از Application Architecture آموزش دهد.

خواننده باید به‌تدریج از:

```text
Component
```

به:

```text
Application
```

و سپس به:

```text
Framework
```

حرکت کند.

Framework باید زمانی وارد شود که محدودیت‌های یک Application بزرگ یا Production Application نیاز به آن را آشکار کرده باشند.

---

# ترتیب آموزش مفاهیم

هر Concept باید زمانی معرفی شود که تمام پیش‌نیازهای آن آماده باشند.

هیچ مفهوم مهمی:

- زودتر از زمان مناسب
- دیرتر از زمان مناسب
- بدون نیاز
- بدون پیش‌نیاز

آموزش داده نشود.

---

# Dependency Rule

قبل از تولید هر Concept باید بررسی شود:

1. این Concept به چه مفاهیمی وابسته است؟
2. آیا آن مفاهیم قبلاً آموزش داده شده‌اند؟
3. آیا خواننده Mental Model لازم را دارد؟
4. آیا Concept جدید از یک نیاز واقعی Concept قبلی متولد می‌شود؟

اگر پاسخ منفی است، Concept نباید وارد جریان فعلی شود.

---

# Concept Causality Rule

بین Conceptهای متوالی باید رابطه علت و معلولی وجود داشته باشد.

مثال:

```text
Component
↓
Need to Configure Component
↓
Props
↓
Need for Changing Component-owned Data
↓
State
↓
Need to Respond to User Interaction
↓
Events
```

وجود چند Concept مرتبط به‌تنهایی کافی نیست.

Conceptها باید از یکدیگر تغذیه کنند.

---

# Scope Rule

هر فصل فقط باید سؤال اصلی خودش را پاسخ دهد.

اگر موضوعی متعلق به فصل آینده است:

- نباید آموزش داده شود.
- فقط در حد یک اشاره کوتاه مجاز است.
- نباید با مثال آموزشی مستقل توضیح داده شود.

مثال:

اگر فصل درباره Props است، نباید در همان فصل State Management Library به‌صورت کامل آموزش داده شود.

---

# React Core و Ecosystem را مخلوط نکنید

کتاب باید مرزهای زیر را روشن نگه دارد:

```text
React Core
↓
React Application Patterns
↓
React Ecosystem
↓
Frameworks
↓
Production Architecture
```

این مرزبندی به معنای جدایی مطلق نیست.

گاهی یک مفهوم در یک لایه نیاز به لایه بعدی ایجاد می‌کند.

اما آموزش هر لایه باید با Mental Model همان لایه انجام شود.

---

# React Router

React Router باید به‌عنوان یک راه‌حل Routing در Ecosystem React معرفی شود، نه به‌عنوان بخشی از React Core.

مسیر آموزشی باید از:

```text
Multiple UI States / Views
↓
URL
↓
Navigation
↓
Routing Problem
↓
Router
↓
React Router
```

ساخته شود.

جزئیات React Router فقط در نقشه راه مخصوص خود تعیین می‌شوند.

---

# State Management

State Management نباید با React State یکسان فرض شود.

کتاب باید تفاوت میان:

- Local State
- Lifted State
- Derived Data
- Shared State
- Context
- External State Management

را بر اساس نیاز معماری توضیح دهد.

هیچ State Management Library نباید صرفاً به‌عنوان ابزار محبوب معرفی شود.

ابتدا مسئله باید ساخته شود.

---

# Data Fetching

Data Fetching نباید صرفاً به معنای اجرای یک `fetch()` آموزش داده شود.

کتاب باید تفاوت میان:

- Server Data
- UI State
- Loading State
- Error State
- Cache
- Synchronization
- Mutation

را در سطح مناسب توضیح دهد.

ابزارهای Data Fetching باید پس از ایجاد مسئله معرفی شوند.

---

# Rendering

Rendering یکی از محورهای اصلی کتاب است.

خواننده باید بتواند توضیح دهد:

- UI چگونه از State و Props حاصل می‌شود.
- Render چه معنایی دارد.
- Re-rendering چرا اتفاق می‌افتد.
- تغییر State چگونه بر UI اثر می‌گذارد.
- Browser DOM و React Rendering چه رابطه‌ای دارند.

Rendering نباید به یک دستورالعمل API تبدیل شود.

---

# Component Design

Component نباید صرفاً یک Function دارای JSX معرفی شود.

کتاب باید Component را به‌عنوان یک واحد طراحی UI و مسئولیت توضیح دهد.

در صورت نیاز باید موضوعاتی مانند:

- Responsibility
- Composition
- Reusability
- Coupling
- Cohesion
- Component Boundary

بررسی شوند.

---

# Props

Props باید به‌عنوان مکانیزم انتقال Input از Parent به Child تحلیل شوند.

باید تفاوت میان:

- Input
- State
- Derived Data
- Callback Prop

برای خواننده روشن شود.

---

# State

State باید به‌عنوان پاسخی به نیاز داده‌ای که در طول عمر Component تغییر می‌کند و تغییر آن باید در UI منعکس شود آموزش داده شود.

هر داده‌ای State نیست.

کتاب باید از Stateهای غیرضروری جلوگیری کند.

---

# Derived Data

یکی از اصول مهم کتاب:

> اگر مقداری را بتوان از Props یا State موجود محاسبه کرد، الزاماً نباید به State مستقل تبدیل شود.

این اصل باید با مثال‌های واقعی و تحلیل معماری توضیح داده شود.

---

# Data Flow

مدل پیش‌فرض React باید به‌صورت دقیق توضیح داده شود.

```text
Parent
↓
Props
↓
Child
```

و در صورت نیاز:

```text
Child Event
↓
Callback
↓
Parent State Update
↓
New Props
↓
New UI
```

این مدل باید پیش از ورود به راه‌حل‌های پیچیده‌تر تثبیت شود.

---

# Hooks

Hooks نباید به‌صورت فهرست API آموزش داده شوند.

هر Hook باید از نیازی که قبلاً ایجاد شده است متولد شود.

برای مثال:

```text
Need for Component State
↓
useState
```

یا:

```text
Need to Synchronize with External System
↓
Effect
↓
useEffect
```

کتاب باید میان:

- Render Logic
- Event Logic
- Effect Logic

تمایز ایجاد کند.

---

# Effect

Effect نباید به‌عنوان مکانیسم عمومی برای اجرای هر Logic معرفی شود.

ابتدا باید مشخص شود:

- Render چه کاری انجام می‌دهد؟
- Event Handler چه کاری انجام می‌دهد؟
- External System چیست؟
- چه زمانی Synchronization لازم است؟

سپس Effect معرفی شود.

---

# Performance

Performance نباید از ابتدا محور آموزش React باشد.

ابتدا باید Correctness و Mental Model ساخته شود.

سپس در صورت وجود مسئله واقعی:

- Re-render Cost
- Memoization
- Component Boundaries
- Expensive Computation
- Code Splitting
- Lazy Loading

بررسی شوند.

بهینه‌سازی بدون وجود مسئله واقعی نباید توصیه شود.

---

# Forms

Forms باید از نیاز واقعی تعامل کاربر ساخته شوند.

مسیر کلی:

```text
User Input
↓
Form State
↓
Validation
↓
Submission
↓
Async Operation
↓
Loading / Error / Success
```

در Frameworkها، تفاوت میان Client-side و Server-side form handling نیز در زمان مناسب بررسی می‌شود.

---

# Async UI

Applicationهای واقعی فقط Success State ندارند.

کتاب باید در سطح مناسب وضعیت‌های زیر را تحلیل کند:

- Initial
- Loading
- Success
- Empty
- Error
- Submitting
- Updating

این وضعیت‌ها باید به‌عنوان بخشی از UI Architecture دیده شوند.

---

# Server / Client Boundary

با ورود به Frameworkها، مخصوصاً Next.js، مفهوم مرز Server و Client باید به‌عنوان یک Concept معماری مهم آموزش داده شود.

این بخش نباید صرفاً به حفظ Syntax مربوط شود.

خواننده باید بتواند توضیح دهد:

- چرا یک Logic باید روی Server اجرا شود؟
- چرا یک UI Interaction به Client نیاز دارد؟
- داده چگونه از Server به UI می‌رسد؟
- مرز Server و Client چه تأثیری بر Architecture دارد؟

---

# Next.js

Next.js یکی از محورهای اصلی بخش Framework کتاب است.

آموزش Next.js باید بر اساس نیازهای Application انجام شود.

موضوعات احتمالی می‌توانند شامل موارد زیر باشند، اما ترتیب و دامنه آن‌ها باید توسط Roadmap تعیین شود:

- Application Routing
- File-based Routing
- Layouts
- Server and Client Components
- Server-side Capabilities
- Data Access
- Rendering Strategies
- Caching
- Mutations
- Forms
- Error Handling
- Loading UI
- Metadata
- Asset Handling
- Authentication Boundaries
- Deployment
- Production Architecture

این فهرست به‌تنهایی Concept Flow نیست.

هر موضوع فقط زمانی وارد فصل می‌شود که نیاز آموزشی آن ایجاد شده باشد.

---

# Next.js و React نباید یکی فرض شوند

کتاب باید همیشه مرز این دو را روشن کند.

React:

```text
UI Library
```

Next.js:

```text
React Framework
```

هر قابلیتی که توسط Next.js ارائه می‌شود نباید به React نسبت داده شود.

و هر قابلیت React نیز نباید به‌عنوان قابلیت اختصاصی Next.js معرفی شود.

---

# Version Awareness

React و Frameworkهای آن دائماً تغییر می‌کنند.

بنابراین:

- اصول پایدار باید از APIهای نسخه‌ای جدا شوند.
- Version-specific behavior باید با Version مشخص شود.
- ویژگی‌های Experimental یا تغییرپذیر نباید به‌عنوان اصول عمومی معرفی شوند.
- اگر یک API یا رفتار به Version خاصی وابسته است، این وابستگی باید صریح باشد.
- Roadmap باید مفاهیم پایدار را از جزئیات نسخه‌ای جدا کند.

---

# Tooling

Build Toolها و ابزارهای Development باید فقط در صورتی آموزش داده شوند که برای فهم Application Architecture یا Workflow ضروری باشند.

نباید کتاب React به فهرست ابزارهای محبوب تبدیل شود.

---

# انتخاب ابزار

کتاب نباید ابزارها را بر اساس محبوبیت رتبه‌بندی کند.

انتخاب باید بر اساس:

- Problem
- Requirements
- Trade-offs
- Project Size
- Team Needs
- Runtime Constraints
- Deployment Model
- Maintainability

انجام شود.

---

# مثال‌ها

مثال‌ها باید:

- کوتاه
- واقعی
- قابل اجرا
- قابل تحلیل
- مرتبط با Application

باشند.

نمونه‌های ترجیحی:

```text
Recipe
Search
Cart
Product
Dashboard
User
Order
Authentication
API
Form
Navigation
```

مثال نباید فقط برای نمایش Syntax ساخته شود.

---

# کد

کد باید حداقل مقدار لازم برای توضیح Concept را داشته باشد.

کدهای طولانی فقط زمانی مجازند که خود Architecture موضوع فصل باشد.

هر مثال باید:

1. هدف مشخص داشته باشد.
2. با Concept Flow مرتبط باشد.
3. قابلیت تحلیل داشته باشد.
4. از پیچیدگی غیرضروری دور باشد.

---

# Narrative Flow

Concept Flow معماری یادگیری است.

اما متن نهایی نباید Conceptها را به مجموعه‌ای از Blockهای جداگانه تبدیل کند.

Conceptها باید در یک روایت طبیعی حرکت کنند.

مرز Conceptها آموزشی است، نه الزاماً Heading مستقل.

یک Concept می‌تواند زمینه Concept بعدی را بسازد.

---

# Common Mistakes

Common Mistakes در سطح Chapter سازمان‌دهی شوند.

نباید برای هر Concept یک فهرست تکراری از اشتباهات ایجاد شود.

تنها زمانی یک هشدار در محل Concept قرار گیرد که حذف آن باعث شکل‌گیری سوءبرداشت جدی شود.

---

# Key Takeaways

Key Takeaways نیز در سطح Chapter ارائه شوند.

این بخش باید مهم‌ترین Mental Modelها و اصول قابل انتقال فصل را خلاصه کند.

نباید صرفاً فهرستی از APIها باشد.

---

# ساختار هر فصل

هر فصل باید در حالت استاندارد شامل موارد زیر باشد:

1. Chapter Goals
2. Core Question
3. Introduction
4. Main Body based on Concept Flow
5. Best Practices
6. Common Mistakes
7. Summary
8. Key Takeaways
9. Technical Interview
10. Golden Answers
11. Conclusion

این ساختار استاندارد است.

Headingهای بدنه اصلی باید بر اساس جریان طبیعی Conceptها شکل بگیرند.

---

# Chapter Goal

Goal باید نتیجه یادگیری را بیان کند.

نباید صرفاً فهرست موضوعات باشد.

نامناسب:

> آموزش useState و useEffect

مناسب:

> خواننده بتواند تفاوت State و Synchronization با External Systems را تحلیل کند و در جای مناسب از سازوکارهای React استفاده کند.

---

# Core Question

هر فصل باید یک سؤال مرکزی داشته باشد.

Core Question باید:

- واقعی
- قابل پاسخ
- محدود
- مرتبط با Concept Flow

باشد.

---

# Introduction

مقدمه باید با یک Problem یا Question شروع شود.

سپس پاسخ اولیه و مسیر حل مسئله را ایجاد کند.

مقدمه نباید تاریخچه طولانی React باشد.

---

# Best Practices

Best Practices باید نتیجه طبیعی Concepts فصل باشند.

نباید به توصیه‌های کلی و بدون دلیل تبدیل شوند.

---

# Technical Interview

تمام فصل‌های اصلی باید بخش Technical Interview داشته باشند.

سطح سؤال‌ها:

- Junior
- Mid-Level
- Senior

---

# تفاوت سطوح مصاحبه

تفاوت سطح سؤال‌ها نباید صرفاً در سخت‌تر شدن Syntax باشد.

Junior بیشتر بر:

- تعریف
- تشخیص
- رفتار پایه
- استفاده صحیح

تمرکز دارد.

Mid-Level بیشتر بر:

- تحلیل
- انتخاب راه‌حل
- Data Flow
- Trade-off
- طراحی Component

تمرکز دارد.

Senior بیشتر بر:

- Architecture
- Scalability
- Performance
- Boundaries
- Trade-offs
- Failure Modes
- Framework Decisions

تمرکز دارد.

---

# Golden Answers

برای سؤال‌های مهم، پاسخ Golden باید:

- مستقیم
- دقیق
- کوتاه
- فنی
- قابل استفاده در مصاحبه

باشد.

پاسخ نباید به شکل گفت‌وگوی ساختگی با Interviewer نوشته شود.

---

# Coding Challenge

در صورت نیاز، Coding Challenge فقط در پایان فصل قرار گیرد.

Challenge باید مستقیماً با Conceptهای فصل مرتبط باشد.

---

# ارجاع به آینده

ارجاع به فصل‌های آینده مجاز است.

اما آموزش آن‌ها در فصل فعلی ممنوع است.

مثال:

> این مسئله در بخش Routing و سپس در Frameworkها به‌صورت کامل بررسی خواهد شد.

نباید وارد آموزش آن موضوع شد.

---

# جلوگیری از تکرار JavaScript Book

اگر Conceptی قبلاً در JavaScript Book به‌صورت کامل آموزش داده شده است:

- نباید دوباره از صفر آموزش داده شود.
- فقط باید در صورت نیاز مرور شود.
- تمرکز باید بر نقش آن Concept در React باشد.

مثلاً Function، Closure یا Promise نباید دوباره به‌صورت مستقل بازنویسی شوند مگر آنکه برای فهم Concept React ضروری باشند.

---

# مرز React و JavaScript

کتاب باید به‌صورت مداوم نشان دهد که کدام رفتار مربوط به JavaScript است و کدام مربوط به React.

مثال:

```text
JavaScript:
Function
Closure
Object
Promise
Module
Event

React:
Component
Props
State
Rendering
Hooks
Composition
```

این مرزبندی برای ساخت Mental Model صحیح ضروری است.

---

# مرز React و Browser

همین اصل درباره Browser نیز برقرار است.

برای مثال:

- DOM
- Browser Event
- Network
- Storage

نباید بدون توضیح به React نسبت داده شوند.

کتاب باید تفاوت میان Browser Capability و React Abstraction را روشن کند.

---

# منابع علمی

اولویت منابع:

1. React Documentation
2. React Reference / Official Documentation
3. Next.js Official Documentation
4. ECMAScript Specification
5. MDN
6. WHATWG
7. TC39
8. مستندات رسمی ابزارهای مورد استفاده
9. کتاب‌های معتبر
10. مقالات فنی معتبر

برای APIها و رفتارهای نسخه‌ای، منابع رسمی و Version-specific اولویت دارند.

---

# دیدگاه Jonas Schmedtmann

اگر Jonas درباره یک Concept دیدگاه آموزشی مهمی داشته باشد، می‌توان بخشی با عنوان زیر اضافه کرد:

## دیدگاه Jonas

این بخش باید:

- کوتاه
- فنی
- بدون نقل‌قول طولانی
- هماهنگ با Concept Flow

باشد.

وجود این بخش الزامی نیست.

---

# زبان نگارش

زبان اصلی کتاب:

**فارسی**

سبک:

- رسمی
- روان
- آموزشی
- دقیق
- فنی
- بدون اغراق
- بدون شعار
- بدون لحن تبلیغاتی

اصطلاحات فنی رایج می‌توانند به انگلیسی حفظ شوند.

نام APIها، Libraryها، Frameworkها و Code Identifierها باید به شکل اصلی خود نوشته شوند.

---

# ویژگی پاراگراف

هر پاراگراف باید یک ایده اصلی داشته باشد.

جملات تا حد امکان کوتاه و دقیق باشند.

پاراگراف‌های بسیار طولانی که چند مفهوم مستقل را ترکیب می‌کنند باید شکسته شوند.

---

# ممنوع است

موارد زیر ممنوع هستند:

- تکرار غیرضروری
- حاشیه
- داستان‌پردازی
- شوخی
- اغراق
- جملات انگیزشی
- تاریخچه طولانی
- معرفی افراد بدون ارزش فنی
- فهرست‌سازی بدون ارتباط مفهومی
- آموزش API بدون Problem
- آموزش Library صرفاً به دلیل محبوبیت
- ورود زودهنگام به Framework
- مخلوط کردن React و Next.js
- نسبت دادن قابلیت Browser به React
- نسبت دادن قابلیت Next.js به React
- بهینه‌سازی بدون وجود Problem
- استفاده از State برای داده‌های Derived بدون دلیل
- ورود به مباحث آینده خارج از Scope فصل

---

# Architecture Thinking

در نیمه دوم کتاب، تمرکز باید به‌تدریج از Component به Application Architecture منتقل شود.

حرکت کلی:

```text
Component
↓
Component Tree
↓
Data Flow
↓
State Ownership
↓
Composition
↓
Application State
↓
Routing
↓
Data Layer
↓
Server / Client Boundary
↓
Framework
↓
Production Architecture
```

این حرکت باید تدریجی باشد.

---

# Production Thinking

کتاب باید خواننده را از Demo Application به Production Application هدایت کند.

موضوعاتی مانند:

- Loading
- Error
- Empty State
- Authentication
- Data Fetching
- Caching
- Performance
- Accessibility
- SEO
- Deployment
- Security Boundaries
- Maintainability

باید زمانی وارد شوند که جایگاه آن‌ها در Architecture آماده شده باشد.

این موارد نباید به‌صورت فهرست پراکنده آموزش داده شوند.

---

# Accessibility

Accessibility نباید یک موضوع تزئینی تلقی شود.

در جایی که Component یا UI Interaction آموزش داده می‌شود، اصول ضروری Accessibility باید رعایت شوند.

اما آموزش کامل Accessibility باید در Scope و Roadmap مناسب خود انجام شود.

---

# Performance

Performance باید با Mental Model شروع شود.

قبل از معرفی ابزارهای Optimization باید مشخص شود:

> چه چیزی کند است و چرا؟

اصل:

```text
Measure
↓
Identify Bottleneck
↓
Understand Cause
↓
Optimize
↓
Measure Again
```

---

# Architecture Trade-offs

هر راه‌حل معماری باید همراه با Trade-off مناسب خود تحلیل شود.

نباید هیچ ابزار یا Patternی به‌عنوان:

- همیشه بهترین
- تنها راه درست
- راه‌حل همه پروژه‌ها

معرفی شود.

---

# کتاب نباید Catalog باشد

هدف کتاب معرفی تمام Libraryهای React نیست.

هدف، آموزش توانایی تحلیل Application و انتخاب ابزار مناسب است.

بنابراین پوشش کمترِ مفاهیم با عمق مناسب، بر فهرست طولانی ابزارها ترجیح دارد.

---

# Version-Specific Content

هرجا رفتار یا API وابسته به Version است:

- Version باید مشخص شود.
- منبع رسمی همان Version بررسی شود.
- Concept پایدار از Syntax یا API موقت جدا شود.
- از تعمیم رفتار یک Version به تمام React یا Next.js خودداری شود.

---

# کیفیت علمی

هیچ تعریف مهمی نباید مبهم باشد.

هیچ رفتار فنی بدون دلیل توضیح داده نشود.

در موضوعات پیچیده، ابتدا Mental Model و سپس جزئیات فنی ارائه شود.

---

# کیفیت آموزشی

پس از مطالعه هر فصل، خواننده باید بتواند:

- مفهوم را با زبان خود توضیح دهد.
- دلیل وجود آن را بیان کند.
- رفتار آن را تحلیل کند.
- کاربرد آن را در پروژه تشخیص دهد.
- محدودیت‌ها و Trade-offهای آن را بیان کند.

---

# کیفیت مهندسی

خواننده باید بتواند از Concepts برای تصمیم‌گیری در Application واقعی استفاده کند.

هدف فقط تولید کد صحیح نیست.

هدف تولید تصمیم مهندسی صحیح است.

---

# کیفیت مصاحبه

خواننده باید بتواند:

- تعریف دقیق ارائه کند.
- رفتار را توضیح دهد.
- تفاوت مفاهیم مشابه را بیان کند.
- راه‌حل مناسب انتخاب کند.
- Trade-off را تحلیل کند.
- در سطح معماری تصمیم خود را توجیه کند.

---

# Self-Check پیش از تولید هر فصل

مدل زبانی باید پیش از تولید هر فصل این موارد را بررسی کند:

### Concept

- آیا هر Concept دقیقاً مشخص است؟
- آیا Concept اضافه وارد نشده است؟
- آیا Conceptی از قلم نیفتاده است؟

### Dependency

- آیا تمام پیش‌نیازها آماده هستند؟
- آیا Concept جدید از نیاز Concept قبلی ایجاد شده است؟

### Causality

- آیا بین Conceptها رابطه علت و معلولی وجود دارد؟

### Scope

- آیا فصل فقط سؤال اصلی خود را پاسخ می‌دهد؟
- آیا Conceptهای آینده وارد آموزش نشده‌اند؟

### React Boundary

- آیا React با JavaScript یا Browser اشتباه نشده است؟

### Ecosystem Boundary

- آیا React Core با Libraryهای Ecosystem مخلوط نشده است؟

### Framework Boundary

- آیا Next.js یا Framework دیگر زودتر از زمان مناسب وارد نشده است؟
- آیا قابلیت Framework به React نسبت داده نشده است؟

### Mental Model

- آیا خواننده یک مدل ذهنی جدید به دست می‌آورد؟

### Engineering

- آیا Concept در تصمیم‌گیری واقعی کاربرد دارد؟
- آیا Trade-offهای مهم بیان شده‌اند؟

### Examples

- آیا مثال واقعی، کوتاه و قابل اجرا است؟
- آیا مثال فقط برای نمایش Syntax ساخته نشده است؟

### Narrative

- آیا Concept Flow به روایت طبیعی تبدیل شده است؟
- آیا Headingها صرفاً کپی Conceptها نیستند؟

### Chapter

- آیا Best Practices وجود دارد؟
- آیا Common Mistakes در سطح Chapter است؟
- آیا Summary وجود دارد؟
- آیا Key Takeaways وجود دارد؟
- آیا Technical Interview وجود دارد؟
- آیا Golden Answers وجود دارد؟
- آیا Conclusion وجود دارد؟

### Version

- آیا مطالب Version-specific به‌درستی مشخص شده‌اند؟

---

# قانون نهایی

پیش از ارائه هر فصل، مدل زبانی باید از خود بپرسد:

> آیا این فصل خواننده را یک قدم از شناخت React به سمت توانایی مهندسی یک React Application واقعی حرکت می‌دهد؟

و همچنین:

- آیا Problem قبل از Solution ساخته شده است؟
- آیا Why قبل از How آمده است؟
- آیا Conceptها بر اساس Dependency مرتب شده‌اند؟
- آیا هر Concept از نیاز Concept قبلی متولد شده است؟
- آیا بین Conceptها رابطه علت و معلولی وجود دارد؟
- آیا React Core، Ecosystem و Framework از یکدیگر تفکیک شده‌اند؟
- آیا جایگاه Next.js در زمان مناسب و به‌عنوان Framework توضیح داده شده است؟
- آیا هیچ Library صرفاً به دلیل محبوبیت وارد کتاب شده است؟
- آیا JavaScript دوباره و غیرضروری آموزش داده نشده است؟
- آیا Browser Capability با React Capability اشتباه نشده است؟
- آیا متن برای پروژه واقعی ارزش دارد؟
- آیا متن برای Technical Interview ارزش دارد؟
- آیا مطلب غیرضروری وارد شده است؟
- آیا Concept Flow به روایت طبیعی تبدیل شده است؟
- آیا سطح فنی متن با سایر فصل‌های کتاب هماهنگ است؟

اگر پاسخ هر یک از پرسش‌های فوق منفی باشد، فصل باید پیش از ارائه بازنویسی شود.

---

# اصل نهایی مجموعه

این کتاب قرار نیست خواننده را به یک **React API User** تبدیل کند.

هدف، تبدیل او به یک **Frontend Engineer** است که بتواند:

```text
Understand the Problem
↓
Build the Mental Model
↓
Choose the Right Concept
↓
Design the Application
↓
Choose the Right Tool
↓
Understand the Trade-offs
↓
Build for Production
```

را انجام دهد.

React ابزار اصلی این مسیر است.

React Ecosystem ابزارهای تکمیل‌کننده آن است.

Frameworkهایی مانند **Next.js** لایه Application و Production را تکمیل می‌کنند.

اما در تمام مسیر، هدف نهایی همچنان **Engineering Thinking** است.
