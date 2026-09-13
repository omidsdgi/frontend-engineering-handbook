# JavaScript Book --- New Roadmap

> **Revised Architecture --- Narrative Learning + Layered Runtime**

این سند، نقشه راه نهایی پیشنهادی برای مجموعه **JavaScript Book** است.

معماری جدید کتاب بر پایه چهار اصل شکل گرفته است:

-   حرکت از **Fundamentals** به مفاهیم وابسته و سپس به Application
    Architecture
-   قرار گرفتن **Arrays & Collections** پیش از Objects و OOP
-   آموزش **Runtime به‌صورت لایه‌ای** و در محل طبیعی وابستگی هر مفهوم، نه
    در یک بخش مستقل و جدا از جریان کتاب
-   جلوگیری از تکرار مفاهیم Modern JavaScript که پیش‌تر در فصل‌های اصلی
    آموزش داده شده‌اند

هدف این نقشه راه حفظ عمق فنی Blueprint قبلی، اما اصلاح وابستگی‌های آموزشی
و جریان کلی یادگیری است.

------------------------------------------------------------------------

# Architecture Overview

``` text
Fundamentals
   ↓
Functions
   ↓
Arrays & Collections
   ↓
Objects
   ↓
OOP
   ↓
Numbers / Dates / Intl
   ↓
Browser / DOM
   ↓
Async JavaScript
   ↓
Modules / Tooling / Architecture
```

در این معماری، **Runtime یک Part مستقل نیست**؛ بلکه به‌صورت لایه‌ای در
نقاطی آموزش داده می‌شود که خواننده برای درک آن آماده است:

``` text
Function Layer
   ↓
Scope Layer
   ↓
Execution Layer
   ↓
Call Stack Layer
   ↓
Lexical Lookup / Closure Layer
   ↓
this / Binding Layer
   ↓
Engine / Runtime Environment Layer
   ↓
Browser Host Layer
   ↓
Async Runtime Layer
   ↓
Event Loop / Queue Layer
```

بنابراین Runtime ابتدا به‌اندازه‌ای معرفی می‌شود که برای فهم Function و
Scope لازم است، سپس Execution و Call Stack ساخته می‌شوند، بعد رابطه آن‌ها
با Browser و در نهایت Async Runtime و Event Loop توضیح داده می‌شود.

------------------------------------------------------------------------

# Part 01 --- JavaScript Fundamentals

## Chapter 01 --- What is JavaScript?

### Chapter Goal

خواننده بتواند JavaScript را به‌عنوان یک Programming Language بشناسد،
جایگاه ECMAScript را تشخیص دهد و تفاوت Language و Host Environment را در
سطح مفهومی درک کند.

### Core Question

JavaScript چیست و چه نقشی در توسعه نرم‌افزار مدرن دارد؟

### Concept Flow

``` text
Programming
↓
JavaScript
↓
Programming Language
↓
ECMAScript
↓
Language Features
↓
Host Environment
↓
Browser / Server
↓
Modern JavaScript
```

------------------------------------------------------------------------

## Chapter 02 --- Values and Variables

### Chapter Goal

خواننده بتواند مفهوم Value و Variable را از یکدیگر تفکیک کند و
Declaration، Initialization و Assignment را به‌درستی تحلیل کند.

### Core Question

Variable چیست و JavaScript چگونه Value را در برنامه مدیریت می‌کند؟

### Concept Flow

``` text
Information
↓
Data
↓
Value
↓
Variable
↓
Identifier
↓
Declaration
↓
Initialization
↓
Assignment
↓
Naming
```

------------------------------------------------------------------------

## Chapter 03 --- Data Types

### Chapter Goal

خواننده بتواند Types مختلف Valueها را تشخیص دهد و تفاوت Primitive و
Object را توضیح دهد.

### Core Question

JavaScript چگونه انواع مختلف Value را مدیریت می‌کند؟

### Concept Flow

``` text
Value
↓
Type
↓
Dynamic Typing
↓
Primitive Types
↓
Object
↓
Primitive vs Object
↓
typeof
↓
Type Checking
```

------------------------------------------------------------------------

## Chapter 04 --- let, const and var

### Chapter Goal

خواننده بتواند تفاوت Declarationهای مختلف را توضیح دهد و انتخاب مناسب
بین `let`، `const` و `var` را تحلیل کند.

### Core Question

چرا JavaScript سه روش مختلف برای Variable Declaration دارد؟

### Concept Flow

``` text
Variable Declaration
↓
var
↓
Problems of var
↓
let
↓
const
↓
Block Scope
↓
Declaration vs Assignment
↓
Best Practices
```

> Hoisting و TDZ در این فصل فقط در حد اشاره معرفی می‌شوند و Runtime آن‌ها
> بعداً به‌صورت دقیق آموزش داده می‌شود.

------------------------------------------------------------------------

## Chapter 05 --- Operators and Expressions

### Chapter Goal

خواننده بتواند Expression، Operand و Operator را تشخیص دهد و رفتار
Operatorهای اصلی را تحلیل کند.

### Core Question

JavaScript چگونه با استفاده از Operators عملیات را روی Values انجام
می‌دهد؟

### Concept Flow

``` text
Expression
↓
Operand
↓
Operator
↓
Arithmetic
↓
Assignment
↓
Comparison
↓
Logical
↓
Unary
↓
Ternary
↓
Precedence
```

------------------------------------------------------------------------

## Chapter 06 --- Strings and Template Literals

### Chapter Goal

خواننده بتواند داده‌های متنی را ایجاد، ترکیب و قالب‌بندی کند و جایگاه
Template Literals را در JavaScript مدرن درک کند.

### Core Question

JavaScript چگونه داده‌های متنی را ذخیره، ترکیب و تولید می‌کند؟

### Concept Flow

``` text
Text Data
↓
String
↓
String Literal
↓
Escape Characters
↓
Concatenation
↓
Template Literals
↓
Interpolation
↓
Multiline Strings
```

------------------------------------------------------------------------

## Chapter 07 --- Taking Decisions

### Chapter Goal

خواننده بتواند جریان اجرای برنامه را بر اساس Conditions کنترل کند و
Truthy/Falsy و Equality را در تصمیم‌گیری به‌کار گیرد.

### Core Question

JavaScript چگونه اجرای برنامه را بر اساس شرایط مختلف کنترل می‌کند؟

### Concept Flow

``` text
Boolean
↓
Condition
↓
if
↓
else
↓
else if
↓
Truthy / Falsy
↓
Boolean Conversion
↓
Equality
↓
switch
↓
Conditional Patterns
```

------------------------------------------------------------------------

## Chapter 08 --- Loops

### Chapter Goal

خواننده بتواند اجرای تکراری را با Loopهای مختلف طراحی و کنترل کند.

### Core Question

JavaScript چگونه اجرای تکراری را مدیریت می‌کند؟

### Concept Flow

``` text
Repetition
↓
Iteration
↓
for
↓
Counter
↓
while
↓
do...while
↓
break
↓
continue
↓
Nested Loops
↓
Iteration Patterns
```

------------------------------------------------------------------------

## Chapter 09 --- Type Conversion and Coercion

### Chapter Goal

خواننده بتواند Type Conversion و Type Coercion را از یکدیگر تشخیص دهد و
رفتار تبدیل Valueها را پیش‌بینی کند.

### Core Question

JavaScript چه زمانی و چگونه Values را بین Types تبدیل می‌کند؟

### Concept Flow

``` text
Value
↓
Type Conversion
↓
Explicit Conversion
↓
Implicit Coercion
↓
To String
↓
To Number
↓
To Boolean
↓
Equality
↓
Common Coercion Rules
```

------------------------------------------------------------------------

## Chapter 10 --- Developer Tools and Debugging

### Chapter Goal

خواننده بتواند اجرای JavaScript را مشاهده، متوقف، بررسی و Debug کند.

### Core Question

چگونه می‌توان اجرای واقعی JavaScript را مشاهده، بررسی و Debug کرد؟

### Concept Flow

``` text
Browser
↓
Developer Tools
↓
Console
↓
Errors
↓
Sources
↓
Debugger
↓
Breakpoint
↓
Scope Inspection
↓
Call Stack Inspection
↓
Watch
↓
Network
↓
Debugging Workflow
```

------------------------------------------------------------------------

## Chapter 11 --- Coding Challenge

### Chapter Goal

خواننده بتواند مفاهیم Fundamentals را برای حل یک مسئله واقعی ترکیب کند.

### Core Question

چگونه مفاهیم Fundamentals را برای حل یک مسئله واقعی ترکیب کنیم؟

### Concept Flow

``` text
Problem
↓
Requirements
↓
Decomposition
↓
Plan
↓
Implementation
↓
Testing
↓
Debugging
↓
Refactoring
↓
Review
```

------------------------------------------------------------------------

# Part 02 --- Functions and the Runtime Foundations

این Part محل آغاز آموزش لایه‌ای Runtime است. ابتدا Function به‌عنوان واحد
قابل اجرای Logic معرفی می‌شود؛ سپس Scope، Execution Context، Call Stack و
Lookup ساخته می‌شوند. در این مرحله Runtime فقط به‌اندازه‌ای معرفی می‌شود که
رفتار Functionها قابل توضیح باشد.

## Chapter 12 --- Function Fundamentals

### Chapter Goal

خواننده بتواند Function را به‌عنوان واحد قابل استفاده مجدد Logic تعریف
کند و Invocation و Return را تحلیل کند.

### Core Question

Function چیست و چگونه JavaScript کد را قابل استفاده مجدد می‌کند؟

### Concept Flow

``` text
Function
↓
Declaration
↓
Parameters
↓
Arguments
↓
Invocation
↓
Execution
↓
Return
↓
Function Output
↓
Reusable Logic
```

------------------------------------------------------------------------

## Chapter 13 --- Function Expressions and Arrow Functions

### Chapter Goal

خواننده بتواند Function Declaration، Function Expression و Arrow
Function را مقایسه کند و تفاوت رفتاری آن‌ها را تشخیص دهد.

### Core Question

Function Declaration، Function Expression و Arrow Function چه تفاوتی
دارند؟

### Concept Flow

``` text
Function as Value
↓
Function Expression
↓
Anonymous Function
↓
Arrow Function
↓
Implicit Return
↓
Function Syntax Choice
```

------------------------------------------------------------------------

## Chapter 14 --- Parameters, Arguments and Default Parameters

### Chapter Goal

خواننده بتواند Interface ورودی یک Function را طراحی کند و رفتار
Parameters، Arguments و Default Parameters را تحلیل کند.

### Core Question

چگونه ورودی‌های Function را به‌صورت قابل اعتماد طراحی کنیم؟

### Concept Flow

``` text
Parameters
↓
Arguments
↓
Multiple Parameters
↓
Default Parameters
↓
undefined
↓
Parameter Behavior
↓
API Design
```

------------------------------------------------------------------------

## Chapter 15 --- First-Class and Higher-Order Functions

### Chapter Goal

خواننده بتواند Function را به‌عنوان Value درک کند و Higher-Order Function
را تحلیل کند.

### Core Question

چرا Function در JavaScript مانند یک Value قابل استفاده است؟

### Concept Flow

``` text
Function as Value
↓
First-Class Function
↓
Passing Functions
↓
Returning Functions
↓
Callback
↓
Higher-Order Function
↓
Abstraction
↓
Functional Thinking
```

------------------------------------------------------------------------

## Chapter 16 --- Callback Functions

### Chapter Goal

خواننده بتواند Callback را به‌عنوان سازوکاری برای واگذاری اجرای Logic
تحلیل کند و تفاوت Callbackهای Synchronous و Asynchronous را تشخیص دهد.

### Core Question

Callback چگونه اجرای یک Function را به Function یا سیستم دیگری واگذار
می‌کند؟

### Concept Flow

``` text
Function as Value
↓
Callback
↓
Synchronous Callback
↓
Asynchronous Callback
↓
Event / Timer
↓
Nested Callbacks
↓
Callback Hell
↓
Need for Promises
```

> Async Runtime در این فصل فقط در حد ایجاد مسئله معرفی می‌شود؛ سازوکار آن
> در Part مربوط به Async JavaScript ساخته خواهد شد.

------------------------------------------------------------------------

## Chapter 17 --- Scope

### Chapter Goal

خواننده بتواند Scope را به‌عنوان مرز دسترسی به Identifierها تحلیل کند و
Global، Function و Block Scope را تشخیص دهد.

### Core Question

JavaScript چگونه تعیین می‌کند یک Identifier در کجا قابل دسترسی باشد؟

### Concept Flow

``` text
Identifier
↓
Scope
↓
Global Scope
↓
Function Scope
↓
Block Scope
↓
Lexical Scope
↓
Variable Accessibility
↓
Scope Rules
```

------------------------------------------------------------------------

## Chapter 18 --- Execution Context and Call Stack

### Chapter Goal

خواننده بتواند هنگام Invocation یک Function، ایجاد Execution Context و
قرار گرفتن آن در Call Stack را به‌صورت مرحله‌ای تحلیل کند.

### Core Question

JavaScript هنگام اجرای Function چه محیطی ایجاد می‌کند و اجرای Functionها
را چگونه مدیریت می‌کند؟

### Concept Flow

``` text
Program Execution
↓
Execution Context
↓
Global Execution Context
↓
Function Invocation
↓
Function Execution Context
↓
Call Stack
↓
LIFO
↓
Nested Calls
↓
Return
↓
Stack Trace
↓
Stack Overflow
```

این فصل اولین لایه جدی Runtime را می‌سازد: **Execution Layer**.

------------------------------------------------------------------------

## Chapter 19 --- Scope Chain and Variable Lookup

### Chapter Goal

خواننده بتواند مسیر جستجوی Identifier را از Scope جاری تا Scopeهای
بیرونی تحلیل کند.

### Core Question

JavaScript چگونه یک Identifier را در Scopeهای مختلف پیدا می‌کند؟

### Concept Flow

``` text
Identifier
↓
Current Scope
↓
Outer Scope
↓
Scope Chain
↓
Variable Lookup
↓
Nested Functions
↓
Lexical Environment
```

این فصل **Lexical Lookup Layer** را روی مدل Execution قبلی اضافه می‌کند.

------------------------------------------------------------------------

## Chapter 20 --- Hoisting and Temporal Dead Zone

### Chapter Goal

خواننده بتواند رفتار Declarationها را در زمان ایجاد Environment و
Initialization تحلیل کند و Hoisting را با مدل ساده «جابجایی کد» اشتباه
نگیرد.

### Core Question

Hoisting واقعاً چیست و چرا رفتار `var`، `let` و `const` متفاوت است؟

### Concept Flow

``` text
Execution Context
↓
Environment Creation
↓
Bindings
↓
Hoisting Model
↓
var
↓
Function Declaration
↓
let / const
↓
Temporal Dead Zone
↓
Initialization
```

------------------------------------------------------------------------

## Chapter 21 --- The this Keyword

### Chapter Goal

خواننده بتواند `this` را بر اساس Call Site و نوع Invocation تحلیل کند.

### Core Question

JavaScript چگونه مقدار `this` را بر اساس نحوه فراخوانی Function تعیین
می‌کند؟

### Concept Flow

``` text
this
↓
Invocation
↓
Global Context
↓
Method Call
↓
Regular Function
↓
Arrow Function
↓
Constructor Call
```

------------------------------------------------------------------------

## Chapter 22 --- Explicit Function Binding

### Chapter Goal

خواننده بتواند مقدار `this` را به‌صورت صریح کنترل کند و کاربرد `call`،
`apply` و `bind` را درک کند.

### Core Question

چگونه مقدار `this` را به‌صورت صریح کنترل کنیم؟

### Concept Flow

``` text
this
↓
Implicit Binding
↓
Explicit Binding
↓
call
↓
apply
↓
bind
↓
Function Borrowing
↓
Partial Application
```

------------------------------------------------------------------------

## Chapter 23 --- Closures

### Chapter Goal

خواننده بتواند رابطه Lexical Scope، Scope Chain و Function را توضیح دهد
و Closure را به‌عنوان نتیجه این رابطه تحلیل کند.

### Core Question

چگونه یک Function می‌تواند Scope بیرونی خود را بعد از پایان اجرای آن حفظ
کند؟

### Concept Flow

``` text
Nested Function
↓
Lexical Scope
↓
Scope Chain
↓
Closure
↓
Preserved Environment
↓
Private State
↓
Function Factory
↓
Real Applications
```

این فصل **Closure Layer** را روی Scope و Lookup قبلی بنا می‌کند.

------------------------------------------------------------------------

## Chapter 24 --- Strict Mode

### Chapter Goal

خواننده بتواند تفاوت Sloppy Mode و Strict Mode را توضیح دهد و آثار آن بر
رفتار زبان را تحلیل کند.

### Core Question

Strict Mode چه رفتارهایی را در JavaScript تغییر می‌دهد و چرا اهمیت دارد؟

### Concept Flow

``` text
Sloppy Mode
↓
Strict Mode
↓
Restrictions
↓
this Behavior
↓
Errors
↓
Safer JavaScript
↓
Modules
```

------------------------------------------------------------------------

# Part 03 --- Arrays and Collections

## Chapter 25 --- Arrays Fundamentals

### Chapter Goal

خواننده بتواند Array را به‌عنوان Collection مرتب‌شده از Values مدل کند و
عملیات پایه روی آن را تحلیل کند.

### Core Question

Array چیست و چگونه مجموعه‌ای از Values را مدیریت می‌کند؟

### Concept Flow

``` text
Collection
↓
Array
↓
Index
↓
Length
↓
Element Access
↓
Mutation
↓
Reference Behavior
↓
Array Construction
```

------------------------------------------------------------------------

## Chapter 26 --- Array Methods

### Chapter Goal

خواننده بتواند Methodهای اصلی Array را بر اساس نوع مسئله انتخاب کند.

### Core Question

چگونه با Array Methods بدون نوشتن Logic تکراری کار کنیم؟

### Concept Flow

``` text
Array
↓
Method
↓
Mutation Methods
↓
Adding / Removing
↓
Searching
↓
Transforming
↓
Non-Mutating Methods
↓
Method Selection
```

------------------------------------------------------------------------

## Chapter 27 --- Array Iteration

### Chapter Goal

خواننده بتواند Iteration روی Array را با Callbackها و Methodهای
Iteration تحلیل کند.

### Core Question

چگونه روی عناصر Array به‌صورت قابل پیش‌بینی Iteration انجام دهیم؟

### Concept Flow

``` text
Array
↓
Iteration
↓
Callback
↓
forEach
↓
Index
↓
Element
↓
Iteration Side Effects
↓
Iteration Choice
```

------------------------------------------------------------------------

## Chapter 28 --- map, filter and reduce

### Chapter Goal

خواننده بتواند Transform، Select و Aggregate را با `map`، `filter` و
`reduce` مدل کند.

### Core Question

چگونه یک Array را به Array یا Result جدید تبدیل، فیلتر یا تجمیع کنیم؟

### Concept Flow

``` text
Array
↓
Transformation
↓
map
↓
Selection
↓
filter
↓
Aggregation
↓
reduce
↓
Accumulator
↓
Data Transformation
```

------------------------------------------------------------------------

## Chapter 29 --- find, some, every and Sorting

### Chapter Goal

خواننده بتواند Search، Condition Checking و Ordering را با Methodهای
مناسب Array انجام دهد.

### Core Question

چگونه در Array جستجو کنیم، Conditions را بررسی کنیم و داده‌ها را مرتب
کنیم؟

### Concept Flow

``` text
Array
↓
Search
↓
find
↓
Condition Checking
↓
some
↓
every
↓
Ordering
↓
sort
↓
Comparator
↓
Mutation Awareness
```

------------------------------------------------------------------------

## Chapter 30 --- Destructuring and Advanced Array Patterns

### Chapter Goal

خواننده بتواند Values آرایه را به‌شکل خوانا استخراج کند و Patternهای
ترکیبی Array را در Code واقعی به‌کار گیرد.

### Core Question

چگونه داده‌های Array را به‌صورت خوانا و انعطاف‌پذیر استخراج و ترکیب کنیم؟

### Concept Flow

``` text
Array
↓
Destructuring
↓
Position
↓
Skipping Values
↓
Default Values
↓
Rest Pattern
↓
Nested Patterns
↓
Practical Data Extraction
```

------------------------------------------------------------------------

## Chapter 31 --- Sets and Maps

### Chapter Goal

خواننده بتواند Array، Set و Map را بر اساس مدل داده و نیاز Application
مقایسه کند.

### Core Question

چه زمانی Array مناسب نیست و باید از Set یا Map استفاده کنیم؟

### Concept Flow

``` text
Collection
↓
Array Limitations
↓
Set
↓
Uniqueness
↓
Map
↓
Key / Value
↓
Iteration
↓
Collection Choice
↓
Real Applications
```

------------------------------------------------------------------------

# Part 04 --- Objects

## Chapter 32 --- Objects Fundamentals

### Chapter Goal

خواننده بتواند Object را به‌عنوان ساختاری برای سازمان‌دهی State و Behavior
مرتبط مدل کند.

### Core Question

Object چگونه داده و رفتار مرتبط را در یک ساختار واحد سازمان‌دهی می‌کند؟

### Concept Flow

``` text
Object
↓
Property
↓
Key / Value
↓
Object Literal
↓
Property Access
↓
Mutation
↓
Method
↓
Reference
```

------------------------------------------------------------------------

## Chapter 33 --- Object Methods and this

### Chapter Goal

خواننده بتواند Methodهای Object را تحلیل کند و رابطه Method Invocation و
`this` را درک کند.

### Core Question

Object چگونه رفتار را از طریق Methods و `this` مدل می‌کند؟

### Concept Flow

``` text
Object
↓
Function Property
↓
Method
↓
Method Invocation
↓
this
↓
Implicit Binding
↓
Arrow Function Difference
↓
Method Patterns
```

------------------------------------------------------------------------

## Chapter 34 --- Enhanced Object Literals

### Chapter Goal

خواننده بتواند Object Literalهای مدرن را خوانا و پویا طراحی کند.

### Core Question

JavaScript چگونه Object Literals را برای نوشتن Objectهای مدرن‌تر و
خواناتر توسعه داده است؟

### Concept Flow

``` text
Object Literal
↓
Property Shorthand
↓
Method Shorthand
↓
Computed Properties
↓
Property Expressions
↓
Dynamic Object Construction
```

------------------------------------------------------------------------

## Chapter 35 --- Object Destructuring and Optional Access

### Chapter Goal

خواننده بتواند داده‌های Object را خوانا و ایمن استخراج کند.

### Core Question

چگونه داده‌های Object را به‌صورت خوانا و ایمن استخراج کنیم؟

### Concept Flow

``` text
Object
↓
Destructuring
↓
Renaming
↓
Default Values
↓
Nested Destructuring
↓
Optional Chaining
↓
Nullish Coalescing
↓
Safe Data Access
```

------------------------------------------------------------------------

## Chapter 36 --- Rest and Spread Syntax

### Chapter Goal

خواننده بتواند داده‌ها را با Spread گسترش دهد و با Rest جمع‌آوری کند.

### Core Question

چگونه JavaScript داده‌ها را جمع یا گسترش می‌دهد؟

### Concept Flow

``` text
Collection
↓
Spread
↓
Expansion
↓
Rest
↓
Remaining Values
↓
Arrays
↓
Objects
↓
Function Parameters
↓
Immutable Patterns
```

------------------------------------------------------------------------

## Chapter 37 --- Object Utilities and Data Modeling

### Chapter Goal

خواننده بتواند Objectهای واقعی Application را با Utilityهای مناسب بررسی
و Transform کند.

### Core Question

چگونه با Objectهای واقعی و داده‌های Application به‌صورت حرفه‌ای کار کنیم؟

### Concept Flow

``` text
Objects
↓
Inspection
↓
Object.keys
↓
Object.values
↓
Object.entries
↓
Transformation
↓
Data Modeling
↓
Maintainable Objects
```

------------------------------------------------------------------------

# Part 05 --- Object-Oriented Programming

## Chapter 38 --- OOP Fundamentals

### Chapter Goal

خواننده بتواند Object-Oriented Thinking را به‌عنوان روشی برای مدیریت
State، Behavior و Responsibility تحلیل کند.

### Core Question

Object-Oriented Programming چگونه پیچیدگی نرم‌افزار را با Objects و
Responsibilities مدیریت می‌کند؟

### Concept Flow

``` text
Programming Paradigms
↓
Object-Oriented Thinking
↓
Object
↓
State
↓
Behavior
↓
Responsibility
↓
Encapsulation
↓
Abstraction
↓
Inheritance
↓
Polymorphism
```

------------------------------------------------------------------------

## Chapter 39 --- Constructor Functions

### Chapter Goal

خواننده بتواند Objectهای مشابه را با Constructor Function ایجاد کند و
نقش `new` و `this` را تحلیل کند.

### Core Question

چگونه قبل از ES6 Class Syntax، Objectهای مشابه را با Constructor
Functions ایجاد می‌کردیم؟

### Concept Flow

``` text
Object Creation
↓
Constructor Function
↓
Constructor Invocation
↓
new
↓
this
↓
Instance
↓
Multiple Instances
↓
Shared Behavior Problem
↓
Need for Prototypes
```

------------------------------------------------------------------------

## Chapter 40 --- Prototypes and Prototype Chain

### Chapter Goal

خواننده بتواند Prototype Relationship و Property Lookup در Prototype
Chain را توضیح دهد.

### Core Question

JavaScript چگونه از طریق Prototypeها رفتار و Properties را بین Objects
به اشتراک می‌گذارد؟

### Concept Flow

``` text
Object
↓
Prototype
↓
Prototype Property
↓
Shared Methods
↓
Prototype Relationship
↓
Property Lookup
↓
Prototype Chain
↓
Inherited Behavior
↓
Built-in Prototypes
```

------------------------------------------------------------------------

## Chapter 41 --- ES Classes

### Chapter Goal

خواننده بتواند Class Syntax را به‌عنوان Syntaxی برای مدل‌سازی OOP در
JavaScript تحلیل کند و رابطه آن را با Prototypeها درک کند.

### Core Question

Class Syntax چگونه Object-Oriented Programming را در JavaScript ساده‌تر
می‌کند؟

### Concept Flow

``` text
Class
↓
Constructor
↓
Instance
↓
Methods
↓
Fields
↓
Private Fields
↓
Getters / Setters
↓
Static Members
↓
Prototype Relationship
```

------------------------------------------------------------------------

## Chapter 42 --- Inheritance and Polymorphism

### Chapter Goal

خواننده بتواند Inheritance، Method Overriding و Polymorphism را تحلیل
کند و محدودیت‌های Inheritance را بشناسد.

### Core Question

چگونه Classها رفتار مشترک را به ارث می‌برند و رفتار متفاوت ارائه می‌کنند؟

### Concept Flow

``` text
Inheritance
↓
extends
↓
Parent Class
↓
Child Class
↓
super
↓
Method Overriding
↓
Method Lookup
↓
Polymorphism
↓
Composition
```

------------------------------------------------------------------------

## Chapter 43 --- Encapsulation and Static Members

### Chapter Goal

خواننده بتواند State عمومی و خصوصی را از هم تفکیک کند و رفتار
Instance-Level و Class-Level را طراحی کند.

### Core Question

چگونه Class طراحی کنیم تا State و Behavior کنترل‌شده و قابل استفاده مجدد
باشند؟

### Concept Flow

``` text
Class
↓
Public State
↓
Private State
↓
Encapsulation
↓
Getters / Setters
↓
Static Methods
↓
Static Properties
↓
Class-Level Behavior
```

------------------------------------------------------------------------

## Chapter 44 --- Memory Management and References

### Chapter Goal

خواننده بتواند Reachability، Garbage Collection و Memory Leak را در سطح
مهندسی توضیح دهد.

### Core Question

JavaScript چگونه Memory موردنیاز برنامه را مدیریت و Valueهای
غیرقابل‌دسترسی را پاک‌سازی می‌کند؟

### Concept Flow

``` text
Value
↓
Memory Allocation
↓
Reference
↓
Reachability
↓
Garbage Collection
↓
Memory Release
↓
Retained References
↓
Memory Leak
```

این فصل **Memory Layer** را پس از Object و OOP قرار می‌دهد؛ جایی که
خواننده Reference و Object Lifetime را می‌شناسد.

------------------------------------------------------------------------

# Part 06 --- Numbers, Dates and Internationalization

## Chapter 45 --- Working with Numbers

### Chapter Goal

خواننده بتواند Numberها، تبدیل‌های عددی و خطاهای رایج محاسبات عددی در
JavaScript را تحلیل کند.

### Core Question

JavaScript چگونه Numberها را نمایش، تبدیل و پردازش می‌کند؟

### Concept Flow

``` text
Number
↓
Numeric Literals
↓
Number Conversion
↓
NaN
↓
Infinity
↓
Floating-Point Behavior
↓
Precision
↓
Practical Number Handling
```

------------------------------------------------------------------------

## Chapter 46 --- Math Object

### Chapter Goal

خواننده بتواند از قابلیت‌های `Math` برای محاسبات کاربردی استفاده کند.

### Core Question

چگونه محاسبات عددی رایج را با Math انجام دهیم؟

### Concept Flow

``` text
Math
↓
Constants
↓
Rounding
↓
Power
↓
Square Root
↓
Min / Max
↓
Random
↓
Practical Calculations
```

------------------------------------------------------------------------

## Chapter 47 --- BigInt

### Chapter Goal

خواننده بتواند محدودیت Number و کاربرد BigInt را تشخیص دهد.

### Core Question

چه زمانی Number برای نمایش Integerهای بزرگ کافی نیست؟

### Concept Flow

``` text
Integer
↓
Number Precision
↓
Large Integers
↓
BigInt
↓
BigInt Literals
↓
Operations
↓
Number / BigInt Boundary
↓
Use Cases
```

------------------------------------------------------------------------

## Chapter 48 --- Working with Dates

### Chapter Goal

خواننده بتواند Date را ایجاد، خوانده و پردازش کند و مسائل Time Zone را
در سطح کاربردی تحلیل کند.

### Core Question

JavaScript چگونه زمان و تاریخ را نمایش و مدیریت می‌کند؟

### Concept Flow

``` text
Time
↓
Date
↓
Timestamp
↓
Creation
↓
Reading Components
↓
Formatting
↓
Arithmetic
↓
Time Zone
↓
Date Pitfalls
```

------------------------------------------------------------------------

## Chapter 49 --- Intl and Internationalization

### Chapter Goal

خواننده بتواند Number، Date و متن را بر اساس Locale مناسب نمایش دهد.

### Core Question

چگونه داده‌های Application را برای Localeهای مختلف به‌شکل صحیح نمایش دهیم؟

### Concept Flow

``` text
Locale
↓
Internationalization
↓
Intl
↓
Number Formatting
↓
Date Formatting
↓
Relative Formatting
↓
Collation
↓
Localized UI Data
```

------------------------------------------------------------------------

# Part 07 --- Browser and DOM

## Chapter 50 --- Browser Environment

### Chapter Goal

خواننده بتواند تفاوت JavaScript Language، Engine و Browser Host
Environment را در سطح دقیق تشخیص دهد.

### Core Question

Browser چه قابلیت‌هایی را در اختیار JavaScript قرار می‌دهد که بخشی از خود
زبان نیستند؟

### Concept Flow

``` text
JavaScript Language
↓
ECMAScript
↓
JavaScript Engine
↓
Runtime Environment
↓
Browser
↓
Host APIs
↓
Web APIs
↓
Browser Runtime
```

این فصل **Engine / Runtime Environment Layer** را پس از آنکه خواننده
Execution و Memory را آموخته است، کامل‌تر می‌کند.

------------------------------------------------------------------------

## Chapter 51 --- DOM Fundamentals

### Chapter Goal

خواننده بتواند DOM را به‌عنوان مدل درختی Document و واسط JavaScript با
صفحه Web تحلیل کند.

### Core Question

JavaScript چگونه ساختار یک Web Page را به‌صورت قابل دستکاری مدل می‌کند؟

### Concept Flow

``` text
Document
↓
DOM
↓
Node
↓
Element
↓
Tree
↓
Document Structure
↓
JavaScript / DOM Boundary
```

------------------------------------------------------------------------

## Chapter 52 --- Selecting and Manipulating Elements

### Chapter Goal

خواننده بتواند Elementها را انتخاب و Properties، Attributes و Content
آن‌ها را تغییر دهد.

### Core Question

چگونه Elementهای DOM را پیدا و تغییر دهیم؟

### Concept Flow

``` text
DOM
↓
Selection
↓
Querying
↓
Element Reference
↓
Content
↓
Attributes
↓
Properties
↓
Classes
↓
Styles
```

------------------------------------------------------------------------

## Chapter 53 --- Creating and Modifying DOM Elements

### Chapter Goal

خواننده بتواند DOM Nodeهای جدید ایجاد و به Document اضافه کند.

### Core Question

چگونه UI را با JavaScript به‌صورت پویا ایجاد و تغییر دهیم؟

### Concept Flow

``` text
DOM
↓
createElement
↓
Node Configuration
↓
Child Nodes
↓
append
↓
prepend
↓
Insert / Replace
↓
Remove
↓
Dynamic UI
```

------------------------------------------------------------------------

## Chapter 54 --- Events and Event Handling

### Chapter Goal

خواننده بتواند Event-driven Interaction را با Event Listenerها تحلیل و
پیاده‌سازی کند.

### Core Question

Browser چگونه User Interaction را به JavaScript منتقل می‌کند؟

### Concept Flow

``` text
User Action
↓
Event
↓
Event Target
↓
Event Listener
↓
Event Handler
↓
Event Object
↓
Default Action
↓
preventDefault
```

------------------------------------------------------------------------

## Chapter 55 --- Event Propagation and Delegation

### Chapter Goal

خواننده بتواند Capture، Target و Bubble را تحلیل کند و Event Delegation
را در UIهای واقعی به‌کار گیرد.

### Core Question

Event چگونه در DOM حرکت می‌کند و چگونه می‌توان از این رفتار برای
Delegation استفاده کرد؟

### Concept Flow

``` text
DOM Tree
↓
Event Propagation
↓
Capturing
↓
Target
↓
Bubbling
↓
stopPropagation
↓
Event Delegation
↓
Dynamic Elements
```

------------------------------------------------------------------------

## Chapter 56 --- DOM Traversing

### Chapter Goal

خواننده بتواند روابط بین Nodeها را برای Navigation در DOM تحلیل کند.

### Core Question

چگونه از یک DOM Element به Elementهای مرتبط برسیم؟

### Concept Flow

``` text
Element
↓
Parent
↓
Children
↓
Siblings
↓
closest
↓
matches
↓
Traversal Patterns
```

------------------------------------------------------------------------

## Chapter 57 --- Forms and User Input

### Chapter Goal

خواننده بتواند Form Submission و User Input را مدیریت و اعتبارسنجی اولیه
را طراحی کند.

### Core Question

چگونه Input کاربر را از Form دریافت، بررسی و پردازش کنیم؟

### Concept Flow

``` text
Form
↓
Input
↓
User Data
↓
Submit Event
↓
FormData
↓
Validation
↓
Error State
↓
Submission Flow
```

------------------------------------------------------------------------

# Part 08 --- Asynchronous JavaScript and the Runtime

این Part **Async Runtime Layer** را کامل می‌کند. در اینجا مفاهیمی که
پیش‌تر به‌صورت تدریجی ساخته شده‌اند---Execution Context، Call Stack،
Browser Runtime و Promise---به یک مدل واحد برای Asynchronous Execution
متصل می‌شوند.

## Chapter 58 --- Introduction to Asynchronous JavaScript

### Chapter Goal

خواننده بتواند تفاوت Synchronous و Asynchronous Execution را توضیح دهد و
مفهوم Non-Blocking را درک کند.

### Core Question

چگونه JavaScript بدون متوقف کردن اجرای برنامه با عملیات زمان‌بر کار
می‌کند؟

### Concept Flow

``` text
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
```

------------------------------------------------------------------------

## Chapter 59 --- Event Loop

### Chapter Goal

خواننده بتواند رابطه Call Stack، Host APIs، Tasks، Microtasks و Event
Loop را تحلیل کند.

### Core Question

JavaScript چگونه Async Tasks را با Call Stack و Queues هماهنگ می‌کند؟

### Concept Flow

``` text
Call Stack
↓
Host APIs
↓
Task Queue
↓
Microtask Queue
↓
Event Loop
↓
Scheduling
↓
Execution Order
```

این فصل **Event Loop Layer** را روی تمام لایه‌های Runtime قبلی بنا می‌کند.

------------------------------------------------------------------------

## Chapter 60 --- AJAX and HTTP Communication

### Chapter Goal

خواننده بتواند Client، Server، HTTP Request و Response را به‌صورت یک
جریان ارتباطی تحلیل کند.

### Core Question

Browser چگونه با Server ارتباط برقرار می‌کند؟

### Concept Flow

``` text
Client
↓
Request
↓
HTTP
↓
Server
↓
Response
↓
Status
↓
JSON
↓
AJAX
↓
XMLHttpRequest
↓
Modern Fetch
```

------------------------------------------------------------------------

## Chapter 61 --- Fetch API

### Chapter Goal

خواننده بتواند با Fetch API یک HTTP Request را ارسال و Response را
پردازش کند.

### Core Question

چگونه با Fetch API به‌صورت مدرن با HTTP Resources کار کنیم؟

### Concept Flow

``` text
HTTP
↓
fetch
↓
Promise
↓
Request
↓
Response
↓
Body
↓
JSON
↓
HTTP Errors
↓
API Integration
```

------------------------------------------------------------------------

## Chapter 62 --- Promises

### Chapter Goal

خواننده بتواند Promise را به‌عنوان مدل مدیریت نتیجه آینده یک عملیات Async
تحلیل کند.

### Core Question

Promise چگونه نتیجه یک عملیات Async را به‌صورت قابل ترکیب مدیریت می‌کند؟

### Concept Flow

``` text
Callback Problem
↓
Promise
↓
Pending
↓
Fulfilled
↓
Rejected
↓
then
↓
catch
↓
finally
↓
Chaining
```

------------------------------------------------------------------------

## Chapter 63 --- Promise Combinators

### Chapter Goal

خواننده بتواند چند Promise را بر اساس رابطه زمانی و موفقیت موردنیاز
ترکیب کند.

### Core Question

چگونه چند Promise را به‌صورت هم‌زمان یا وابسته مدیریت کنیم؟

### Concept Flow

``` text
Multiple Promises
↓
Promise.all
↓
Parallel Work
↓
Promise.allSettled
↓
Promise.race
↓
Promise.any
↓
Concurrency Patterns
```

------------------------------------------------------------------------

## Chapter 64 --- Async/Await

### Chapter Goal

خواننده بتواند Promise-based Code را با `async` و `await` خواناتر و قابل
کنترل‌تر بنویسد.

### Core Question

چگونه Promise-based Code را با Syntax خواناتر مدیریت کنیم؟

### Concept Flow

``` text
Promise
↓
async
↓
await
↓
Sequential Async Flow
↓
try / catch
↓
finally
↓
Parallel Async
↓
Error Handling
```

------------------------------------------------------------------------

## Chapter 65 --- Async JavaScript Behind the Scenes

### Chapter Goal

خواننده بتواند رابطه `async`، `await`، Promise، Microtask و Event Loop
را در Runtime تحلیل کند.

### Core Question

`async/await` و Promise در Runtime واقعاً چگونه اجرا می‌شوند؟

### Concept Flow

``` text
async Function
↓
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Call Stack
↓
Event Loop
↓
Resume Execution
```

این فصل **لایه‌های Runtime را به یک مدل واحد متصل می‌کند**؛ بنابراین
Runtime دیگر یک موضوع جدا و منفصل از سایر مباحث نیست.

------------------------------------------------------------------------

## Chapter 66 --- Error Handling

### Chapter Goal

خواننده بتواند خطاهای Synchronous و Asynchronous را به‌صورت قابل اعتماد
مدیریت کند.

### Core Question

چگونه خطاهای Synchronous و Asynchronous را به‌صورت قابل اعتماد مدیریت
کنیم؟

### Concept Flow

``` text
Error
↓
throw
↓
Error Object
↓
try
↓
catch
↓
finally
↓
Promise Rejection
↓
Async Error
↓
Recovery
```

------------------------------------------------------------------------

## Chapter 67 --- Advanced Async Patterns

### Chapter Goal

خواننده بتواند Async Operations را در Applicationهای واقعی قابل کنترل و
قابل اعتماد طراحی کند.

### Core Question

چگونه Async Operations را در Applicationهای واقعی قابل کنترل و قابل
اعتماد کنیم؟

### Concept Flow

``` text
Async Operations
↓
Sequential
↓
Parallel
↓
Loading State
↓
Race Condition
↓
Cancellation
↓
AbortController
↓
Resource Cleanup
↓
Real Applications
```

------------------------------------------------------------------------

# Part 09 --- Modules, Tooling and Application Architecture

## Chapter 68 --- Modern JavaScript Development

### Chapter Goal

خواننده بتواند قابلیت‌های Modern JavaScript را به‌عنوان ابزارهای حل مسئله
و نه مجموعه‌ای از Syntaxهای جداگانه به‌کار گیرد.

### Core Question

JavaScript مدرن چگونه خوانایی، انعطاف‌پذیری و Maintainability کد را
افزایش می‌دهد؟

### Concept Flow

``` text
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
```

این فصل **Recap کاربردی** است و نباید آموزش اولیه مفاهیمی را که قبلاً در
فصل‌های مستقل ارائه شده‌اند تکرار کند.

------------------------------------------------------------------------

## Chapter 69 --- JavaScript Modules

### Chapter Goal

خواننده بتواند یک Application را به Moduleهای مستقل و دارای Boundary
مشخص تقسیم کند.

### Core Question

چگونه یک JavaScript Application بزرگ را به Moduleهای مستقل تقسیم کنیم؟

### Concept Flow

``` text
Large Application
↓
Global Scope Problem
↓
Module
↓
Encapsulation
↓
ES Modules
↓
export
↓
import
↓
Module Scope
↓
Dynamic import
```

------------------------------------------------------------------------

## Chapter 70 --- CommonJS and Module Systems

### Chapter Goal

خواننده بتواند ES Modules و CommonJS را از نظر Syntax، Loading و کاربرد
در Ecosystem مقایسه کند.

### Core Question

ES Modules و CommonJS چه تفاوتی دارند و چرا هر دو در اکوسیستم JavaScript
وجود دارند؟

### Concept Flow

``` text
Module Problem
↓
CommonJS
↓
require
↓
module.exports
↓
ES Modules
↓
import / export
↓
Execution Differences
↓
Modern Recommendation
```

------------------------------------------------------------------------

## Chapter 71 --- NPM and Package Management

### Chapter Goal

خواننده بتواند Dependencyهای یک JavaScript Project را با npm و
package.json مدیریت کند.

### Core Question

چگونه Dependencyهای یک JavaScript Project را مدیریت کنیم؟

### Concept Flow

``` text
JavaScript Ecosystem
↓
Package
↓
Registry
↓
npm
↓
package.json
↓
Dependencies
↓
Versioning
↓
Scripts
↓
Package Management
```

------------------------------------------------------------------------

## Chapter 72 --- JavaScript Build Process

### Chapter Goal

خواننده بتواند دلیل و مراحل اصلی Build Process را در یک پروژه مدرن توضیح
دهد.

### Core Question

چرا پروژه‌های JavaScript مدرن به Build Process نیاز دارند؟

### Concept Flow

``` text
Source Code
↓
Dependencies
↓
Modules
↓
Transformation
↓
Bundling
↓
Optimization
↓
Production Build
```

------------------------------------------------------------------------

## Chapter 73 --- Parcel

### Chapter Goal

خواننده بتواند نقش یک Build Tool را در Development و Production با
Parcel مشاهده و تحلیل کند.

### Core Question

Parcel چگونه فرآیند Development و Production Build را برای یک پروژه
JavaScript مدیریت می‌کند؟

### Concept Flow

``` text
Project
↓
Entry
↓
Parcel
↓
Development Server
↓
Asset Processing
↓
Bundling
↓
Optimization
↓
Production Build
```

------------------------------------------------------------------------

## Chapter 74 --- Babel

### Chapter Goal

خواننده بتواند نقش Babel در Transform کردن Syntax و هدف‌گذاری محیط‌های
مختلف را توضیح دهد.

### Core Question

Babel چگونه Syntax مدرن JavaScript را برای محیط‌های هدف مختلف Transform
می‌کند؟

### Concept Flow

``` text
Modern Syntax
↓
Babel
↓
Parser
↓
Transformation
↓
Presets
↓
Plugins
↓
Polyfills
↓
Browser Compatibility
```

------------------------------------------------------------------------

## Chapter 75 --- Modern JavaScript Development Workflow

### Chapter Goal

خواننده بتواند یک Workflow حرفه‌ای برای توسعه، بررسی، Build و انتشار
JavaScript Project طراحی کند.

### Core Question

چگونه JavaScript را در یک Workflow حرفه‌ای توسعه، بررسی و آماده انتشار
کنیم؟

### Concept Flow

``` text
Project
↓
Source Code
↓
Modules
↓
Dependencies
↓
Linting
↓
Formatting
↓
Testing / Debugging
↓
Build
↓
Production
```

------------------------------------------------------------------------

## Chapter 76 --- Application Architecture Fundamentals

### Chapter Goal

خواننده بتواند Script ساده را به Applicationی با Responsibilities مشخص و
قابل نگهداری تبدیل کند.

### Core Question

چگونه JavaScript Code را از Script ساده به Application قابل نگهداری
تبدیل کنیم؟

### Concept Flow

``` text
Script
↓
Growing Complexity
↓
Architecture
↓
Responsibilities
↓
Separation of Concerns
↓
Modularity
↓
Maintainability
↓
Scalability
```

------------------------------------------------------------------------

## Chapter 77 --- MVC Architecture

### Chapter Goal

خواننده بتواند مسئولیت‌های Model، View و Controller و جریان داده میان
آن‌ها را تحلیل کند.

### Core Question

چگونه Model، View و Controller مسئولیت‌های Application را از یکدیگر جدا
می‌کنند؟

### Concept Flow

``` text
Application
↓
Responsibilities
↓
Model
↓
View
↓
Controller
↓
Data Flow
↓
Separation
↓
Maintainability
```

------------------------------------------------------------------------

## Chapter 78 --- Application State Management

### Chapter Goal

خواننده بتواند State را در Application شناسایی، مالکیت آن را مشخص و
جریان Update آن را طراحی کند.

### Core Question

چگونه State را در یک JavaScript Application به‌صورت قابل پیش‌بینی مدیریت
کنیم؟

### Concept Flow

``` text
Application
↓
State
↓
Local State
↓
Shared State
↓
Global State
↓
Single Source of Truth
↓
Update
↓
Rendering
↓
Synchronization
```

------------------------------------------------------------------------

## Chapter 79 --- Model Design

### Chapter Goal

خواننده بتواند Data، API، Transformation و Business Logic را در Model
سازمان‌دهی کند.

### Core Question

چگونه Data و Business Logic را در Model سازمان‌دهی کنیم؟

### Concept Flow

``` text
Model
↓
Data
↓
API
↓
Async Loading
↓
Transformation
↓
Business Rules
↓
State
↓
Persistence
```

------------------------------------------------------------------------

## Chapter 80 --- View Architecture

### Chapter Goal

خواننده بتواند View را طوری طراحی کند که Rendering و UI Logic قابل
نگهداری باشند.

### Core Question

چگونه View را طوری طراحی کنیم که Rendering و UI Logic قابل نگهداری
باشند؟

### Concept Flow

``` text
View
↓
DOM
↓
Rendering
↓
UI State
↓
Events
↓
Reusable View
↓
Separation
↓
Maintainability
```

------------------------------------------------------------------------

## Chapter 81 --- Controller and Application Flow

### Chapter Goal

خواننده بتواند Controller را به‌عنوان هماهنگ‌کننده جریان User، Model و
View تحلیل کند.

### Core Question

چگونه Controller جریان بین User، Model و View را هماهنگ می‌کند؟

### Concept Flow

``` text
User Action
↓
Event
↓
Controller
↓
Model
↓
Async Operation
↓
State Update
↓
View
↓
UI Update
```

------------------------------------------------------------------------

## Chapter 82 --- Event-Driven Architecture and Pub/Sub

### Chapter Goal

خواننده بتواند Event-Driven Communication و Pub/Sub را به‌عنوان یک الگوی
معماری مستقل از Framework تحلیل کند.

### Core Question

چگونه بخش‌های مختلف Application بدون وابستگی مستقیم با یکدیگر ارتباط
برقرار کنند؟

### Concept Flow

``` text
Components
↓
Coupling Problem
↓
Events
↓
Publisher
↓
Subscriber
↓
Event Bus
↓
Decoupling
↓
Application Architecture
```

> این فصل یک Pattern معماری عمومی JavaScript است و نباید به‌عنوان الزام
> برای React یا سایر Frameworkها آموزش داده شود.

------------------------------------------------------------------------

## Chapter 83 --- Forkify Architecture

### Chapter Goal

خواننده بتواند مفاهیم کتاب را در Architecture یک Application واقعی ترکیب
کند.

### Core Question

چگونه تمام مفاهیم JavaScript کتاب را در یک Application واقعی ترکیب کنیم؟

### Concept Flow

``` text
Application Requirements
↓
Architecture
↓
Modules
↓
Model
↓
View
↓
Controller
↓
State
↓
Async API
↓
Events
↓
Rendering
↓
Complete Application
```

------------------------------------------------------------------------

## Chapter 84 --- JavaScript Professional Patterns and Final Review

### Chapter Goal

خواننده بتواند تمام مفاهیم کتاب را در قالب اصول مهندسی، تصمیم‌گیری فنی و
یک مدل ذهنی واحد جمع‌بندی کند.

### Core Question

چگونه تمام مفاهیم JavaScript را از Language Fundamentals تا Runtime،
Browser، Async و Application Architecture در یک مدل ذهنی واحد ترکیب
کنیم؟

### Concept Flow

``` text
Language Fundamentals
↓
Functions
↓
Runtime Foundations
↓
Arrays & Collections
↓
Objects
↓
OOP
↓
Numbers / Dates / Intl
↓
Browser / DOM
↓
Async Runtime
↓
Modules
↓
Tooling
↓
Application Architecture
↓
Professional JavaScript
```

این فصل باید شامل دو بخش روایی باشد:

``` text
Professional Patterns
↓
Readable Code
↓
Separation of Concerns
↓
Reusable Functions
↓
Modular Design
↓
Predictable State
↓
Error Handling
↓
Async Reliability
↓
Maintainability
```

و سپس:

``` text
Final Review
↓
Language
↓
Data
↓
Functions
↓
Runtime
↓
Collections
↓
Objects
↓
OOP
↓
Browser
↓
Async
↓
Modules
↓
Architecture
↓
Engineering Judgment
```

------------------------------------------------------------------------

# Layered Runtime Architecture

Runtime در این Blueprint عمداً از مسیر اصلی یادگیری جدا نشده است. هر لایه
زمانی معرفی می‌شود که Conceptهای لازم برای درک آن ساخته شده باشند.

## Layer 1 --- Function Execution

در Function Fundamentals، خواننده ابتدا با Invocation و Execution آشنا
می‌شود.

``` text
Function
↓
Invocation
↓
Execution
↓
Return
```

هدف این مرحله ساختن مدل اولیه «کد چگونه اجرا می‌شود» است.

## Layer 2 --- Scope and Lexical Environment

پس از Function، مفهوم Scope معرفی می‌شود.

``` text
Identifier
↓
Scope
↓
Lexical Scope
↓
Accessibility
```

## Layer 3 --- Execution Context and Call Stack

بعد از اینکه Function و Scope شناخته شدند، Execution Context و Call
Stack معرفی می‌شوند.

``` text
Function Invocation
↓
Execution Context
↓
Call Stack
↓
Return
```

این مرحله توضیح می‌دهد Function در زمان اجرا چه محیطی دارد و چرا Nested
Calls به ترتیب مشخص اجرا می‌شوند.

## Layer 4 --- Variable Lookup

بعد از Execution، Lookup ساخته می‌شود.

``` text
Current Scope
↓
Outer Scope
↓
Scope Chain
↓
Variable Lookup
```

## Layer 5 --- Closures

Closure نتیجه طبیعی ترکیب Function، Lexical Scope و Scope Chain است.

``` text
Nested Function
↓
Lexical Scope
↓
Scope Chain
↓
Closure
↓
Preserved Environment
```

## Layer 6 --- Binding and this

بعد از ساخته‌شدن مدل Function Execution، رفتار `this` آموزش داده می‌شود.

``` text
Invocation
↓
Binding
↓
this
↓
Implicit / Explicit / Constructor Binding
```

## Layer 7 --- Engine and Browser Runtime

پس از اینکه خواننده Execution را در سطح Language فهمید، Browser Runtime
معرفی می‌شود.

``` text
JavaScript
↓
Engine
↓
Runtime Environment
↓
Browser Host
↓
Web APIs
```

در این مرحله مرز میان **Language** و **Host Environment** روشن می‌شود.

## Layer 8 --- Asynchronous Runtime

بعد از Browser APIs و HTTP، نیاز به Non-Blocking Execution مطرح می‌شود.

``` text
Call Stack
↓
Host APIs
↓
Task / Microtask Queues
↓
Event Loop
↓
Continuation
```

## Layer 9 --- Promise Runtime

Promise و Async/Await روی همین Runtime بنا می‌شوند.

``` text
Promise
↓
await
↓
Suspension
↓
Continuation
↓
Microtask
↓
Event Loop
↓
Resume Execution
```

## Runtime Learning Principle

``` text
Do not teach Runtime as a disconnected chapter.
Build Runtime from the execution model outward.
```

یعنی:

``` text
Function
→ Scope
→ Execution Context
→ Call Stack
→ Lookup
→ Closure
→ this
→ Engine / Host
→ Async Runtime
→ Event Loop
→ Promise / async-await Runtime
```

این ترتیب از معرفی زودهنگام مفاهیم پیچیده جلوگیری می‌کند و هر Runtime
Concept را بر پایه دانشی که قبلاً ساخته شده است قرار می‌دهد.

------------------------------------------------------------------------

# Global Concept Flow

``` text
JavaScript
   ↓
Values
   ↓
Variables
   ↓
Types
   ↓
Expressions
   ↓
Control Flow
   ↓
Functions
   ↓
Scope
   ↓
Execution Context
   ↓
Call Stack
   ↓
Variable Lookup
   ↓
Closures
   ↓
this / Binding
   ↓
Arrays
   ↓
Collections
   ↓
Objects
   ↓
Object Utilities
   ↓
OOP
   ↓
Prototypes
   ↓
Classes
   ↓
Inheritance / Polymorphism
   ↓
Encapsulation
   ↓
Memory
   ↓
Numbers
   ↓
Dates / Intl
   ↓
Browser Runtime
   ↓
DOM
   ↓
Events
   ↓
HTTP
   ↓
Async JavaScript
   ↓
Promises
   ↓
async / await
   ↓
Event Loop
   ↓
Modules
   ↓
NPM
   ↓
Build Tools
   ↓
Application Architecture
   ↓
MVC
   ↓
State
   ↓
Forkify
   ↓
Professional JavaScript
```

------------------------------------------------------------------------

# Architectural Changes from the Previous Blueprint

## 1. Arrays moved before Objects

Arrays and Collections are now introduced before the Object and OOP
layers so that the reader can understand collection-oriented data
manipulation before entering deeper Object modeling.

## 2. Runtime is no longer a standalone Part

The previous standalone:

``` text
JavaScript Behind the Scenes
```

is removed as an isolated Part.

Its concepts are redistributed according to dependency:

``` text
Scope
→ Execution Context / Call Stack
→ Lookup
→ Hoisting / TDZ
→ this
→ Closure
→ Browser Runtime
→ Async Runtime
→ Event Loop
```

## 3. Runtime is taught in layers

The reader first learns the minimum execution model required for
Functions, then expands that model through Scope, Execution Context,
Call Stack, Lookup, Closures, Engine/Host Environment and finally Async
Runtime.

## 4. Modern JavaScript is no longer a duplicate syntax chapter

Capabilities such as:

``` text
Arrow Functions
Template Literals
Destructuring
Default Parameters
Rest / Spread
Enhanced Object Literals
Optional Chaining
Nullish Coalescing
```

are taught where their concepts naturally belong.

The later Modern JavaScript chapter is therefore a **practical
synthesis**, not a second first-time syntax course.

## 5. OOP remains dependent on Objects

The OOP sequence remains:

``` text
Objects
↓
OOP Fundamentals
↓
Constructor Functions
↓
Prototypes
↓
Classes
↓
Inheritance / Polymorphism
↓
Encapsulation / Static Members
```

This preserves the historical and technical relationship between
JavaScript's Object Model, Prototype Model and Class Syntax.

## 6. Browser comes after the language and object model

Browser APIs and DOM are introduced only after the reader understands:

``` text
Values
Functions
Scope
Execution
Objects
Collections
OOP
```

This keeps the boundary between JavaScript Language and Browser APIs
clear.

## 7. Async comes after Browser and HTTP foundations

Async JavaScript is introduced after the reader understands Browser
Runtime and HTTP communication.

The progression becomes:

``` text
Browser Runtime
↓
Events
↓
HTTP
↓
Non-Blocking Work
↓
Event Loop
↓
Fetch
↓
Promises
↓
async / await
↓
Async Runtime
```

## 8. Architecture remains at the end

Application Architecture is intentionally kept after:

``` text
Language
↓
Runtime
↓
Browser
↓
Async
↓
Modules
↓
Tooling
```

so that Architecture is based on concepts already learned rather than
introducing abstractions prematurely.

------------------------------------------------------------------------

# Final Learning Architecture

``` text
PART 01
JavaScript Fundamentals
        ↓
PART 02
Functions + Runtime Foundations
        ↓
PART 03
Arrays & Collections
        ↓
PART 04
Objects
        ↓
PART 05
Object-Oriented Programming
        ↓
PART 06
Numbers / Dates / Intl
        ↓
PART 07
Browser / DOM
        ↓
PART 08
Async JavaScript + Async Runtime
        ↓
PART 09
Modules / Tooling / Application Architecture
```

این معماری، مسیر کتاب را از **Language → Execution → Data Structures →
Object Model → Browser → Async Runtime → Application Architecture** حرکت
می‌دهد و Runtime را به‌جای یک موضوع منفصل، به‌صورت یک مدل ذهنی تدریجی در کل
کتاب می‌سازد.
