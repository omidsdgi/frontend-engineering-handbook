Chapter 70 — CommonJS and Module Systems
Chapter Goal

در پایان این فصل، خواننده باید بتواند:

CommonJS را به‌عنوان یک Module System در JavaScript توضیح دهد.
نقش require و module.exports را تحلیل کند.
تفاوت اصلی CommonJS و ES Modules را درک کند.
تفاوت آن‌ها را از نظر نحوه Loading و Execution توضیح دهد.
بداند چرا هر دو سیستم در Ecosystem JavaScript وجود دارند.
در یک پروژه جدید، انتخاب مناسب‌تری برای Module System داشته باشد.
Core Question

ES Modules و CommonJS چه تفاوتی دارند و چرا هر دو در Ecosystem JavaScript وجود دارند؟

Introduction

در فصل قبل دیدیم که Module چگونه مشکل Global Scope را کاهش می‌دهد و چگونه می‌توان یک Application را به بخش‌های مستقل تقسیم کرد.

در JavaScript مدرن، این کار معمولاً با ES Modules انجام می‌شود:

export
↓
import
↓
Module Scope

اما اگر با پروژه‌های قدیمی‌تر یا بسیاری از پروژه‌های Node.js مواجه شویم، ممکن است به Syntax متفاوتی برسیم:

const recipe = require('./recipe');

یا:

module.exports = recipe;

در اینجا با یک سؤال مواجه می‌شویم:

اگر JavaScript دارای ES Modules است، چرا هنوز require و module.exports را می‌بینیم؟

پاسخ این است که JavaScript فقط یک Module System نداشته است.

CommonJS قبل از رواج ES Modules به‌وجود آمد و سال‌ها یکی از روش‌های اصلی مدیریت Moduleها در Node.js بود.

بنابراین برای درک صحیح پروژه‌های JavaScript، کافی نیست فقط Syntax مدرن import و export را بشناسیم. باید بدانیم CommonJS چه مسئله‌ای را حل کرد، چگونه کار می‌کند و چه تفاوتی با ES Modules دارد.

جریان این فصل چنین است:

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
Module Problem

وقتی Application کوچک است، ممکن است تمام کدها در یک فایل قرار بگیرند.

اما با بزرگ‌تر شدن Application، کدها معمولاً به بخش‌های مختلف تقسیم می‌شوند:

recipe.js
search.js
api.js
ui.js
utils.js

هر فایل می‌تواند مسئولیت مشخصی داشته باشد.

اما تقسیم کد به چند فایل به‌تنهایی کافی نیست.

باید مشخص کنیم:

یک فایل چه چیزی را در اختیار دیگر فایل‌ها قرار می‌دهد؟
فایل دیگر چگونه به آن دسترسی پیدا می‌کند؟
Dependencyها چگونه مدیریت می‌شوند؟
کد هر فایل چگونه از Scope سایر فایل‌ها جدا می‌شود؟

این همان مسئله‌ای است که Module System باید حل کند.

در فصل قبل، ES Modules را به‌عنوان راهکار استاندارد JavaScript بررسی کردیم.

اما ES Modules تنها راهکاری نیست که در Ecosystem JavaScript وجود داشته است.

برای درک دلیل وجود CommonJS باید به محیطی توجه کنیم که قبل از رواج ES Modules نیاز به یک Module System داشت.

CommonJS

CommonJS یک Module System است که برای سازمان‌دهی کد JavaScript طراحی شد و در Ecosystem مربوط به Node.js به‌طور گسترده مورد استفاده قرار گرفت.

در CommonJS هر فایل معمولاً یک Module در نظر گرفته می‌شود.

برای مثال فرض کنید فایل زیر یک Recipe را تعریف می‌کند:

const recipe = {
title: 'Pasta',
publisher: 'Jonas',
};

module.exports = recipe;

فایل دیگری می‌تواند این Module را دریافت کند:

const recipe = require('./recipe');

console.log(recipe.title);

در اینجا دو مفهوم اصلی CommonJS را می‌بینیم:

module.exports
↓
Export

require()
↓
Import

بنابراین همان مسئله‌ای که در ES Modules با export و import حل می‌کردیم، در CommonJS با module.exports و require() حل می‌شود.

اما این دو سیستم از نظر نحوه اجرای Moduleها کاملاً یکسان نیستند.

require

در CommonJS برای دریافت یک Module از require() استفاده می‌شود.

برای مثال:

const api = require('./api');

مفهوم این کد ساده است:

Module موردنظر را پیدا کن، آن را دریافت کن و مقدار Export شده آن را در اختیار این فایل قرار بده.

برای مثال:

// api.js

const getRecipes = () => {
// ...
};

module.exports = getRecipes;

و در فایل دیگر:

const getRecipes = require('./api');

اکنون getRecipes همان مقداری است که از Module مربوط به api.js Export شده است.

بنابراین:

api.js
↓
module.exports
↓
require('./api')
↓
getRecipes

require() در CommonJS فقط یک Syntax برای Import نیست؛ بخشی از مدل Module System است که Node.js بر اساس آن Module را دریافت و اجرا می‌کند.

module.exports

اگر require() وظیفه دریافت Module را بر عهده دارد، باید راهی برای مشخص کردن چیزی که یک Module در اختیار دیگران قرار می‌دهد وجود داشته باشد.

در CommonJS این کار با module.exports انجام می‌شود.

برای مثال:

const calculateTotal = (price, quantity) => {
return price * quantity;
};

module.exports = calculateTotal;

Module مشخص کرده است که مقدار اصلی قابل دریافت از آن، تابع calculateTotal است.

سپس:

const calculateTotal = require('./cart');

مقدار دریافت‌شده همان تابع است.

می‌توان این رابطه را چنین دید:

Module
↓
module.exports
↓
Exported Value
↓
require()
↓
Imported Value

بنابراین module.exports تعیین می‌کند که یک CommonJS Module چه چیزی را در اختیار مصرف‌کننده قرار دهد.

Exporting Multiple Values

یک Module همیشه فقط یک مقدار ساده در اختیار Application قرار نمی‌دهد.

ممکن است یک Module چند Function یا چند Value داشته باشد.

در این حالت می‌توان یک Object را Export کرد:

const getRecipes = () => {
// ...
};

const formatRecipe = recipe => {
// ...
};

module.exports = {
getRecipes,
formatRecipe,
};

سپس:

const { getRecipes, formatRecipe } = require('./recipeUtils');

در اینجا Module یک Object را Export کرده است و مصرف‌کننده Properties موردنظر را از آن دریافت می‌کند.

بنابراین CommonJS می‌تواند یک Value واحد یا مجموعه‌ای از Valueها را در اختیار سایر Moduleها قرار دهد.

چرا CommonJS به وجود آمد؟

تا اینجا ممکن است سؤال مهم‌تری شکل گرفته باشد:

اگر CommonJS چنین کاری را انجام می‌دهد، چرا اصلاً ES Modules ایجاد شد؟

برای پاسخ باید به ترتیب زمانی توجه کنیم.

CommonJS در دوره‌ای مورد استفاده قرار گرفت که JavaScript هنوز یک Module System استاندارد در خود زبان نداشت.

Applicationهای بزرگ به روشی برای:

تقسیم کد
مدیریت Dependency
جلوگیری از وابستگی به Global Scope
استفاده مجدد از کد

نیاز داشتند.

CommonJS یکی از راهکارهایی بود که این نیاز را برطرف کرد.

بنابراین وجود CommonJS یک اتفاق تصادفی نیست.

این سیستم پاسخی به یک نیاز واقعی در Ecosystem JavaScript بود.

اما با رشد JavaScript و استاندارد شدن Moduleها در خود زبان، راهکار دیگری وارد شد:

ES Modules

ES Modules

ES Modules یا ESM، Module System استاندارد JavaScript است.

در این سیستم از:

export

برای Export کردن و از:

import

برای Import کردن استفاده می‌شود.

برای مثال:

// recipe.js

export const recipe = {
title: 'Pasta',
publisher: 'Jonas',
};

و:

// app.js

import { recipe } from './recipe.js';

این Syntax را در فصل قبل به‌صورت کامل بررسی کردیم.

بنابراین در این فصل هدف تکرار آموزش ES Modules نیست.

هدف این است که آن را در کنار CommonJS قرار دهیم تا تفاوت دو سیستم روشن شود.

import و export

در ES Modules رابطه بین Moduleها به‌صورت مستقیم با import و export بیان می‌شود.

برای مثال:

export const getRecipes = () => {
// ...
};

و:

import { getRecipes } from './api.js';

در نتیجه:

ES Module
↓
export
↓
import
↓
Consumer Module

در CommonJS همین مفهوم با Syntax دیگری بیان می‌شود:

CommonJS Module
↓
module.exports
↓
require()
↓
Consumer Module

پس تفاوت فقط در نام Syntaxها نیست.

مدل اجرای این دو سیستم نیز تفاوت‌هایی دارد.

Execution Differences

یکی از مهم‌ترین تفاوت‌های CommonJS و ES Modules در نحوه مدیریت Moduleها و Loading آن‌هاست.

در CommonJS، require() بخشی از مدل اجرایی CommonJS است.

برای مثال:

const api = require('./api');

در زمان اجرای کد، Dependency موردنظر دریافت می‌شود.

این مدل به CommonJS اجازه می‌دهد که require() را مانند یک Function Call در کد مشاهده کنیم.

اما ES Modules بر اساس مدل متفاوتی طراحی شده‌اند.

برای مثال:

import { api } from './api.js';

import یک Statement معمولی مانند فراخوانی یک Function نیست.

Module Dependencies توسط سیستم Module قبل از اجرای کامل Module مدیریت می‌شوند.

به همین دلیل ES Modules اطلاعات ساختاری بیشتری درباره Dependencyهای یک Module در اختیار Runtime قرار می‌دهند.

این تفاوت یکی از دلایل مهم تفاوت رفتار CommonJS و ESM است.

Static و Dynamic بودن Module Structure

در ES Modules، Dependencyها ساختار مشخصی دارند.

برای مثال:

import { getRecipes } from './api.js';

سیستم Module می‌تواند قبل از اجرای Body اصلی Module، رابطه آن با Module دیگر را تشخیص دهد.

به این ویژگی معمولاً Static Module Structure گفته می‌شود.

در CommonJS، require() یک Call Expression است:

const api = require('./api');

و می‌تواند در جریان اجرای برنامه مورد استفاده قرار گیرد.

این تفاوت باعث می‌شود مدل CommonJS انعطاف اجرایی بیشتری داشته باشد، در حالی که ES Modules ساختار Module را برای Runtime و Tooling قابل تحلیل‌تر می‌کند.

این تفاوت برای ابزارهای مدرن JavaScript اهمیت زیادی دارد.

CommonJS در برابر ES Modules

اکنون می‌توان دو سیستم را در یک تصویر قرار داد:

ویژگی	CommonJS	ES Modules
Export	module.exports	export
Import	require()	import
Module Scope	دارد	دارد
Module System استاندارد زبان	خیر	بله
مدل Dependency	بیشتر Runtime-oriented	Static-oriented
استفاده تاریخی در Node.js	بسیار گسترده	مدرن و استاندارد
مناسب برای کد جدید	وابسته به محیط	معمولاً انتخاب ترجیحی

نکته مهم این است که CommonJS یک سیستم «غلط» نیست.

CommonJS در زمان و محیط خودش یک راهکار مهم برای حل مشکل Moduleها بود.

اما نیازهای امروز Ecosystem JavaScript باعث شده ES Modules نقش محوری‌تری پیدا کند.

آیا CommonJS هنوز مهم است؟

بله.

شناخت CommonJS هنوز اهمیت دارد، زیرا در دنیای واقعی ممکن است با پروژه‌ها، Packageها یا کدهایی مواجه شویم که از آن استفاده می‌کنند.

برای مثال ممکن است در یک پروژه با چنین کدی روبه‌رو شویم:

const express = require('express');

اگر فقط ES Modules را بشناسیم، ممکن است این Syntax برایمان ناآشنا باشد.

اما اگر مدل CommonJS را بدانیم، رابطه کاملاً روشن است:

require()
↓
دریافت Dependency

بنابراین یادگیری CommonJS بیشتر از آنکه برای نوشتن پروژه‌های جدید ضروری باشد، برای خواندن، تحلیل و نگهداری کدهای موجود اهمیت دارد.

Modern Recommendation

اکنون سؤال نهایی این است:

در یک پروژه جدید از CommonJS استفاده کنیم یا ES Modules؟

پاسخ به محیط پروژه و ابزارهای آن بستگی دارد، اما در پروژه‌های مدرن JavaScript، ES Modules معمولاً انتخاب ترجیحی است.

دلیل این انتخاب فقط Syntax مدرن‌تر نیست.

ES Modules بخشی از استاندارد خود JavaScript است و با Ecosystem مدرن JavaScript، Browserها و ابزارهای جدید Build و Development هماهنگی مناسبی دارد.

در نتیجه:

New Project
↓
ES Modules

اما این به معنی حذف کامل CommonJS نیست.

اگر پروژه‌ای از CommonJS استفاده می‌کند، باید مدل آن را بشناسیم و بتوانیم رابطه زیر را تحلیل کنیم:

require()
↕
module.exports

پس یک JavaScript Developer حرفه‌ای باید هر دو مدل را بشناسد، اما لزوماً در پروژه جدید از هر دو استفاده نکند.

یک مدل ذهنی ساده

برای جلوگیری از حفظ کردن Syntaxها، می‌توان دو سیستم را این‌گونه در ذهن نگه داشت:

CommonJS
module.exports
↓
require()
ES Modules
export
↓
import

هر دو یک مسئله اصلی را حل می‌کنند:

چگونه کد یک Module را در اختیار Module دیگری قرار دهیم؟

اما روش طراحی و اجرای آن‌ها متفاوت است.

بنابراین تفاوت اصلی را نباید فقط در Syntax جست‌وجو کرد.

تفاوت مهم‌تر در Module Model و نحوه مدیریت Dependencyهاست.

Best Practices
1. در پروژه جدید، Module System را آگاهانه انتخاب کنید

به‌جای ترکیب تصادفی CommonJS و ES Modules، سیستم مورد استفاده پروژه را بشناسید و مطابق ساختار پروژه عمل کنید.

2. CommonJS و ES Modules را با هم اشتباه نگیرید

این دو Syntax از دو Module System متفاوت می‌آیند:

require()

و:

import

معنای کلی مشابهی دارند، اما مدل یکسانی ندارند.

3. برای خواندن پروژه‌های قدیمی CommonJS را بشناسید

شناخت CommonJS فقط برای نوشتن کد جدید نیست.

بخشی از مهارت حرفه‌ای، توانایی خواندن و نگهداری کد موجود است.

4. Module System را با Package Management یکی ندانید

Module System مشخص می‌کند Moduleها چگونه کد را Export و Import می‌کنند.

Package Management موضوع دیگری است و به مدیریت Dependencyهای پروژه مربوط می‌شود.

این دو مفهوم به یکدیگر مرتبط‌اند، اما یکسان نیستند.

Common Mistakes
اشتباه اول: تصور اینکه require همان import است

این دو در سطح مفهومی هر دو برای دریافت Dependency استفاده می‌شوند، اما متعلق به دو Module System متفاوت هستند.

اشتباه دوم: تصور اینکه CommonJS دیگر وجود ندارد

ممکن است ES Modules انتخاب مدرن‌تری باشد، اما CommonJS همچنان در بسیاری از کدهای موجود دیده می‌شود.

اشتباه سوم: تصور اینکه module.exports فقط یک Syntax دیگر برای export است

این دو متعلق به دو مدل متفاوت هستند.

module.exports = value;

در CommonJS معنا دارد، در حالی که:

export default value;

مربوط به ES Modules است.

اشتباه چهارم: ترکیب کردن Syntaxها بدون توجه به محیط

برای مثال نباید صرفاً به دلیل شباهت مفهومی، بدون توجه به Configuration پروژه چنین Syntaxهایی را به‌صورت تصادفی ترکیب کنیم:

const api = require('./api');

و:

import user from './user.js';

اینکه چنین ترکیبی در یک محیط خاص ممکن است یا خیر، به Runtime و Configuration پروژه وابسته است.

Summary

در این فصل دیدیم که JavaScript بیش از یک Module System داشته است.

CommonJS برای مدیریت Moduleها در Ecosystem JavaScript، به‌ویژه در Node.js، نقش مهمی داشته است.

در CommonJS:

module.exports
↓
Export

و:

require()
↓
Import

در ES Modules:

export
↓
Export

و:

import
↓
Import

تفاوت این دو فقط در Syntax نیست.

ES Modules دارای ساختار Staticتری برای Dependencyهاست، در حالی که CommonJS بر مدل اجرایی متفاوتی مبتنی است.

به همین دلیل ES Modules در JavaScript مدرن انتخاب معمول‌تری برای پروژه‌های جدید است، در حالی که CommonJS همچنان برای درک و نگهداری بسیاری از پروژه‌ها و Packageهای موجود اهمیت دارد.

Key Takeaways
CommonJS یک Module System قدیمی‌تر و مهم در Ecosystem JavaScript است.
require() برای دریافت Module استفاده می‌شود.
module.exports مقدار قابل Export از یک CommonJS Module را مشخص می‌کند.
ES Modules Module System استاندارد JavaScript است.
import و export Syntax اصلی ES Modules هستند.
CommonJS و ES Modules فقط دو Syntax متفاوت نیستند؛ مدل اجرایی متفاوتی دارند.
ES Modules ساختار Staticتری برای Dependencyها ارائه می‌کند.
CommonJS هنوز برای خواندن و نگهداری کدهای موجود مهم است.
در پروژه‌های مدرن، ES Modules معمولاً انتخاب ترجیحی است.
Module System با Package Management یک مفهوم نیست.
Technical Interview
Junior Level
سؤال: CommonJS چیست؟

Golden Answer:

CommonJS یک Module System است که برای تقسیم کد JavaScript به Moduleهای مستقل استفاده می‌شود. در آن برای Export از module.exports و برای Import از require() استفاده می‌شود.

سؤال: تفاوت require و import چیست؟

Golden Answer:

هر دو برای دریافت Dependency استفاده می‌شوند، اما متعلق به دو Module System متفاوت هستند. require() مربوط به CommonJS و import مربوط به ES Modules است.

Mid-Level
سؤال: تفاوت اصلی CommonJS و ES Modules چیست؟

Golden Answer:

CommonJS از require() و module.exports استفاده می‌کند و Dependencyها را در مدل اجرایی CommonJS مدیریت می‌کند. ES Modules از import و export استفاده می‌کند و ساختار Staticتری برای Dependencyها دارد. ES Modules همچنین Module System استاندارد JavaScript است.

سؤال: چرا CommonJS هنوز در پروژه‌های JavaScript دیده می‌شود؟

Golden Answer:

زیرا CommonJS قبل از رواج ES Modules در Ecosystem JavaScript، به‌خصوص Node.js، بسیار گسترده استفاده می‌شد. بنابراین بسیاری از پروژه‌ها و Packageهای موجود همچنان بر اساس آن ساخته شده‌اند.

Senior-Oriented
سؤال: چرا Static بودن ES Modules برای Ecosystem JavaScript اهمیت دارد؟

Golden Answer:

زیرا ساختار Static Dependencyها به Runtime و ابزارهای توسعه اجازه می‌دهد رابطه بین Moduleها را پیش از اجرای کامل کد تحلیل کنند. این ویژگی برای Tooling، تحلیل Dependencyها و بهینه‌سازی‌های مدرن اهمیت دارد.

سؤال: آیا CommonJS یک روش منسوخ و بی‌استفاده است؟

Golden Answer:

خیر. CommonJS یک Module System قدیمی‌تر است، اما هنوز در بسیاری از پروژه‌ها و Packageهای موجود استفاده می‌شود. برای پروژه‌های جدید، ES Modules معمولاً انتخاب مدرن‌تری است، اما شناخت CommonJS برای خواندن و نگهداری کدهای موجود ضروری است.

Conclusion

در این فصل دیدیم که مسئله Moduleها با یک Syntax خاص شروع نمی‌شود.

مسئله اصلی این است که یک Application بزرگ چگونه کد خود را بین بخش‌های مستقل تقسیم می‌کند و Dependencyهای بین آن‌ها را مدیریت می‌کند.

CommonJS یکی از پاسخ‌های مهم به این مسئله بود:

module.exports
↓
require()

بعدها ES Modules به‌عنوان Module System استاندارد JavaScript وارد شد:

export
↓
import

بنابراین اگر در یک پروژه با require() مواجه شویم، نباید آن را صرفاً یک Syntax قدیمی بدانیم. باید بدانیم پشت آن یک Module System متفاوت قرار دارد.

اکنون که Moduleها را شناختیم، یک سؤال طبیعی شکل می‌گیرد:

اگر Moduleها کد Application را سازمان‌دهی می‌کنند، Dependencyهای خارجی مانند Libraryها و Packageها را چگونه مدیریت می‌کنیم؟

پاسخ این سؤال ما را به NPM و Package Management می‌رساند.