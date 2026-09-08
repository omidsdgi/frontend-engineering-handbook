# Chapter 31 — Encapsulation and Static Members

## اهداف فصل

پس از پایان این فصل، انتظار می‌رود بتوانید:

* مفهوم **Encapsulation** را در طراحی Class توضیح دهید.
* تفاوت میان **Public State** و **Private State** را در JavaScript تشخیص دهید.
* توضیح دهید چرا همه Stateهای یک Object نباید مستقیماً از بیرون قابل تغییر باشند.
* Private Fields را با Syntax `#` ایجاد و استفاده کنید.
* نقش Encapsulation را در کنترل State و حفظ Invariants توضیح دهید.
* تفاوت میان Access مستقیم به Property و استفاده از **Getter / Setter** را درک کنید.
* توضیح دهید چه زمانی Getter و Setter مفید هستند و چرا نباید صرفاً برای هر Property از آن‌ها استفاده کرد.
* تفاوت میان **Instance Members** و **Static Members** را تشخیص دهید.
* Static Method و Static Property را تعریف و استفاده کنید.
* توضیح دهید چرا بعضی Behaviorها به Instance تعلق ندارند و باید در سطح Class قرار بگیرند.
* مفهوم **Class-Level Behavior** را در طراحی Classها به‌کار ببرید.

---

# Core Question

> **چگونه Class طراحی کنیم تا State و Behavior کنترل‌شده و قابل استفاده مجدد باشند؟**

جریان این فصل:

```text
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

---

# مقدمه

در فصل‌های گذشته دیدیم که Class فقط یک Syntax برای ایجاد Object نیست.

Class به ما اجازه می‌دهد State و Behavior مربوط به یک مفهوم را در یک ساختار مشخص قرار دهیم.

برای مثال، فرض کنید در یک Application فروشگاهی با `BankAccount` کار می‌کنیم.

هر Account می‌تواند Stateای مانند:

```text
owner
balance
```

داشته باشد و Behaviorهایی مانند:

```text
deposit()
withdraw()
```

ارائه کند.

می‌توانیم Class را به این شکل تعریف کنیم:

```javascript
class BankAccount {
  constructor(owner, balance) {
    this.owner = owner;
    this.balance = balance;
  }

  deposit(amount) {
    this.balance += amount;
  }

  withdraw(amount) {
    this.balance -= amount;
  }
}
```

اکنون ایجاد یک Account ساده است:

```javascript
const account = new BankAccount('Omid', 5000);
```

اما یک مسئله مهم در این طراحی وجود دارد.

مقدار `balance` مستقیماً از بیرون قابل دسترسی است:

```javascript
account.balance = -100000;
```

JavaScript مانع این تغییر نمی‌شود.

در حالی که از نظر Domain، چنین مقداری ممکن است کاملاً نامعتبر باشد.

Account باید بتواند وضعیت خودش را کنترل کند.

بنابراین مسئله جدیدی مطرح می‌شود:

> آیا هر Stateای که یک Object دارد، باید مستقیماً از بیرون قابل دسترسی و تغییر باشد؟

پاسخ منفی است.

اینجاست که مفهوم **Encapsulation** اهمیت پیدا می‌کند.

---

# Public State

تا اینجا بیشتر Stateهایی که در Class ایجاد کرده‌ایم، Public بوده‌اند:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}
```

وقتی Instance ایجاد می‌کنیم:

```javascript
const user = new User(
  'Omid',
  'omid@example.com'
);
```

می‌توانیم مستقیماً به State دسترسی داشته باشیم:

```javascript
console.log(user.name);
console.log(user.email);
```

و حتی آن را تغییر دهیم:

```javascript
user.name = 'Ali';
```

این رفتار برای بعضی Stateها کاملاً مناسب است.

اما همیشه این‌طور نیست.

فرض کنید یک `BankAccount` داریم:

```javascript
class BankAccount {
  constructor(owner, balance) {
    this.owner = owner;
    this.balance = balance;
  }
}
```

اکنون هر Code دیگری می‌تواند بنویسد:

```javascript
account.balance = -5000;
```

در نتیجه Object می‌تواند وارد وضعیتی شود که از نظر منطق Application معتبر نیست.

این مسئله فقط درباره Bank Account نیست.

در بسیاری از Objectهای واقعی، بعضی Stateها باید دارای محدودیت باشند.

برای مثال:

```text
Product price
Order status
Account balance
User permissions
Cart total
```

همه این‌ها ممکن است قواعدی داشته باشند که نباید توسط هر بخش از برنامه به‌صورت مستقیم نقض شوند.

بنابراین هرچه Application بزرگ‌تر می‌شود، کنترل نحوه تغییر State اهمیت بیشتری پیدا می‌کند.

---

# Private State

اگر یک State نباید مستقیماً از بیرون تغییر کند، به مکانیسمی نیاز داریم که آن State را از دسترسی مستقیم خارج کند.

JavaScript برای Classها **Private Fields** را فراهم می‌کند.

Private Field با علامت `#` تعریف می‌شود:

```javascript
class BankAccount {
  #balance;

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
  }
}
```

در اینجا:

```text
owner
```

یک Public Field است.

اما:

```text
#balance
```

یک Private Field است.

تفاوت آن‌ها در نحوه دسترسی است.

این کد معتبر است:

```javascript
console.log(account.owner);
```

اما این کد معتبر نیست:

```javascript
console.log(account.#balance);
```

Private Field فقط از داخل Class قابل دسترسی است.

بنابراین:

```javascript
class BankAccount {
  #balance;

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
  }

  deposit(amount) {
    this.#balance += amount;
  }
}
```

اکنون `balance` دیگر مستقیماً در اختیار Code بیرونی نیست.

---

# چرا Private State مهم است؟

Private بودن State فقط به معنای «مخفی بودن» نیست.

هدف اصلی این است که **کنترل تغییر State را در اختیار خود Object قرار دهیم**.

در طراحی قبلی:

```javascript
account.balance = -5000;
```

هر Codeای می‌توانست State را تغییر دهد.

اما در طراحی جدید:

```javascript
class BankAccount {
  #balance;

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
  }

  deposit(amount) {
    if (amount <= 0) return;

    this.#balance += amount;
  }
}
```

تغییر `balance` از مسیر مشخصی انجام می‌شود.

این Method می‌تواند قبل از تغییر State، قواعد موردنظر را بررسی کند.

در نتیجه، Object فقط State را نگهداری نمی‌کند؛ بلکه **مسئول حفظ وضعیت معتبر خودش نیز هست**.

---

# Encapsulation

اکنون می‌توانیم مسئله را در سطح بزرگ‌تری ببینیم.

Encapsulation فقط به معنای Private کردن Propertyها نیست.

**Encapsulation یعنی State و Behavior مربوط به یک Object را در یک Abstraction قرار دهیم و دسترسی و تغییر State را تا حد لازم کنترل کنیم.**

در یک طراحی Encapsulated، Code بیرونی نباید مجبور باشد مستقیماً با جزئیات داخلی Object کار کند.

برای مثال، در یک Bank Account بهتر است Code بیرونی بگوید:

```javascript
account.deposit(1000);
```

نه اینکه مستقیماً مقدار داخلی را تغییر دهد:

```javascript
account.balance += 1000;
```

در حالت اول، Account خودش مسئول تغییر State است.

در حالت دوم، منطق تغییر State به بیرون از Account منتقل شده است.

این تفاوت کوچک، از نظر طراحی بسیار مهم است.

---

# Encapsulation و Invariants

یکی از دلایل مهم Encapsulation، حفظ **Invariant**های Object است.

Invariant یعنی شرطی که باید در وضعیت معتبر Object برقرار باشد.

برای یک Bank Account ممکن است یکی از این قواعد وجود داشته باشد:

```text
balance must not be negative
```

اگر `balance` Public باشد، هر بخش Application می‌تواند این قاعده را نقض کند:

```javascript
account.balance = -1000;
```

اما اگر State خصوصی باشد:

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }

  withdraw(amount) {
    if (amount > this.#balance) {
      return;
    }

    this.#balance -= amount;
  }
}
```

Object خودش می‌تواند تصمیم بگیرد که آیا تغییر مجاز است یا خیر.

بنابراین Encapsulation کمک می‌کند **قواعد داخلی Object در خود Object متمرکز باقی بمانند**.

---

# Private Fields با `#`

Syntax مربوط به Private Field ساده است:

```javascript
class User {
  #password;

  constructor(password) {
    this.#password = password;
  }
}
```

هر Field که با `#` شروع شود، Private است.

این Field فقط در بدنه همان Class قابل دسترسی است.

برای مثال:

```javascript
class User {
  #password;

  constructor(password) {
    this.#password = password;
  }

  changePassword(newPassword) {
    this.#password = newPassword;
  }
}
```

Method `changePassword()` می‌تواند به:

```javascript
this.#password
```

دسترسی داشته باشد.

اما Code بیرونی نمی‌تواند مستقیماً آن را بخواند یا تغییر دهد.

---

# Private Field با Convention متفاوت است

گاهی در Codeهای قدیمی‌تر ممکن است چیزی شبیه این ببینیم:

```javascript
this._balance = balance;
```

علامت `_` یک Convention است.

یعنی برنامه‌نویس به دیگران می‌گوید:

> بهتر است مستقیماً به این Property دسترسی نداشته باشید.

اما JavaScript مانع دسترسی نمی‌شود.

مثلاً:

```javascript
account._balance = -5000;
```

از نظر زبان کاملاً ممکن است.

اما:

```javascript
this.#balance
```

Private واقعی است.

Code بیرونی نمی‌تواند آن را مستقیماً Access کند.

بنابراین باید میان این دو تفاوت قائل شویم:

```text
_balance
↓
Convention

#balance
↓
Private Class Field
```

---

# Encapsulation به معنی مخفی کردن همه چیز نیست

یک سوءبرداشت رایج این است که:

> برای Encapsulation باید همه Propertyها را Private کنیم.

این هدف Encapsulation نیست.

برخی Stateها کاملاً طبیعی است که Public باشند.

برای مثال:

```javascript
class Product {
  constructor(title, price) {
    this.title = title;
    this.price = price;
  }
}
```

اگر Application نیاز دارد `title` را بخواند، Public بودن آن می‌تواند کاملاً مناسب باشد.

مسئله این است که باید آگاهانه تصمیم بگیریم:

> کدام State باید مستقیماً قابل دسترسی باشد و کدام State باید تحت کنترل Object باقی بماند؟

Encapsulation درباره **کنترل مناسب دسترسی** است، نه مخفی کردن بی‌دلیل اطلاعات.

---

# Getters

اکنون یک مسئله جدید داریم.

فرض کنید `balance` را Private کرده‌ایم:

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }
}
```

Code بیرونی دیگر نمی‌تواند بنویسد:

```javascript
account.#balance;
```

اما ممکن است نیاز داشته باشیم مقدار Balance را بخوانیم.

اگر State کاملاً Private باشد، چگونه آن را در اختیار Consumer قرار دهیم؟

اینجاست که **Getter** وارد می‌شود.

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }

  get balance() {
    return this.#balance;
  }
}
```

اکنون می‌توان نوشت:

```javascript
console.log(account.balance);
```

بدون اینکه `#balance` مستقیماً در دسترس باشد.

نکته مهم این است که:

```javascript
account.balance
```

از نظر ظاهری مانند دسترسی به یک Property است، اما در واقع Getter اجرا می‌شود.

---

# چرا Getter مفید است؟

Getter به ما اجازه می‌دهد **نحوه دسترسی به State را از نحوه نگهداری آن جدا کنیم**.

در مثال:

```javascript
get balance() {
  return this.#balance;
}
```

Consumer فقط می‌داند که می‌تواند Balance را بخواند.

اما نمی‌داند این Value چگونه در داخل Object نگهداری می‌شود.

این جداسازی در آینده اهمیت بیشتری پیدا می‌کند.

ممکن است امروز:

```javascript
return this.#balance;
```

کافی باشد.

اما بعداً ممکن است Value از چند State داخلی محاسبه شود.

برای مثال:

```javascript
get fullName() {
  return `${this.#firstName} ${this.#lastName}`;
}
```

در این حالت `fullName` یک State ذخیره‌شده نیست.

یک Value محاسبه‌شده است.

اما Consumer همچنان می‌تواند آن را مانند Property بخواند:

```javascript
user.fullName;
```

بنابراین Getter می‌تواند یک **Computed Property** نیز ارائه دهد.

---

# Setter

Getter برای خواندن State بود.

اما گاهی می‌خواهیم اجازه تغییر State را نیز بدهیم، در حالی که این تغییر باید تحت کنترل باشد.

برای این کار می‌توان از **Setter** استفاده کرد.

```javascript
class Product {
  #price;

  constructor(price) {
    this.price = price;
  }

  get price() {
    return this.#price;
  }

  set price(value) {
    if (value < 0) {
      throw new Error('Price cannot be negative');
    }

    this.#price = value;
  }
}
```

اکنون:

```javascript
const product = new Product(100);

product.price = 150;
```

باعث اجرای Setter می‌شود.

اما:

```javascript
product.price = -50;
```

می‌تواند رد شود.

در اینجا Setter یک نقطه کنترل برای تغییر State ایجاد کرده است.

---

# Getter و Setter یک Property واقعی ایجاد نمی‌کنند

این نکته فنی مهم است.

وقتی می‌نویسیم:

```javascript
get price() {
  return this.#price;
}
```

در واقع یک Property معمولی به نام `price` روی هر Instance ایجاد نکرده‌ایم.

`price` یک **Accessor Property** است که رفتار خواندن آن توسط Getter تعریف شده است.

به همین دلیل:

```javascript
product.price
```

Getter را اجرا می‌کند.

و:

```javascript
product.price = 150;
```

Setter را اجرا می‌کند.

این Syntax باعث می‌شود Interface عمومی Object ساده باقی بماند، در حالی که Implementation داخلی می‌تواند کنترل‌شده باشد.

---

# آیا برای هر Property باید Getter و Setter بسازیم؟

خیر.

این یکی از سوءبرداشت‌های رایج درباره Encapsulation است.

گاهی چنین کدی می‌بینیم:

```javascript
get name() {
  return this.#name;
}

set name(value) {
  this.#name = value;
}
```

اگر هیچ منطق یا کنترلی در Setter وجود نداشته باشد، سؤال مهمی مطرح می‌شود:

> آیا این لایه اضافی واقعاً ارزش دارد؟

اگر هدف فقط این باشد که یک Private Field را بدون هیچ کنترل یا تبدیل، بخوانیم و بنویسیم، استفاده از Getter و Setter ممکن است ارزش طراحی محدودی داشته باشد.

Getter و Setter زمانی مفیدتر می‌شوند که بخواهیم:

* دسترسی را کنترل کنیم.
* Value را اعتبارسنجی کنیم.
* Value را محاسبه کنیم.
* Representation داخلی را از Interface عمومی جدا کنیم.
* هنگام تغییر State منطق مشخصی اجرا کنیم.

بنابراین Getter و Setter ابزار هستند، نه الزام.

---

# Encapsulation و Public API

یکی از نتایج مهم Encapsulation، ایجاد یک **Public API** مشخص برای Object است.

برای مثال:

```javascript
class BankAccount {
  #balance;

  constructor(balance) {
    this.#balance = balance;
  }

  deposit(amount) {
    // ...
  }

  withdraw(amount) {
    // ...
  }

  get balance() {
    return this.#balance;
  }
}
```

از بیرون، Consumer فقط با این Interface کار می‌کند:

```text
deposit()
withdraw()
balance
```

اما اینکه `balance` دقیقاً چگونه ذخیره یا تغییر می‌کند، جزئیات داخلی Class است.

این جداسازی باعث می‌شود بتوانیم Implementation داخلی را تغییر دهیم، بدون اینکه لزوماً Code مصرف‌کننده تغییر کند.

در نتیجه، Encapsulation فقط یک ویژگی Syntax نیست.

یک ابزار مهم برای **کاهش وابستگی میان بخش‌های مختلف برنامه** است.

---

# Instance Members در برابر Static Members

تا اینجا همه Methodهایی که ایجاد کردیم، به Instance مربوط بودند.

برای مثال:

```javascript
class BankAccount {
  deposit(amount) {
    // ...
  }
}
```

وقتی می‌نویسیم:

```javascript
const account = new BankAccount(5000);

account.deposit(1000);
```

Method `deposit()` درباره یک Account مشخص عمل می‌کند.

به همین دلیل این Behavior به Instance تعلق دارد.

اما همیشه Behavior مربوط به یک Instance خاص نیست.

گاهی یک Operation به خود Class مربوط می‌شود.

مثلاً فرض کنید می‌خواهیم بدانیم چه تعداد Bank Account ایجاد شده است.

این اطلاعات متعلق به یک Account خاص نیست.

به کل `BankAccount` مربوط است.

اینجاست که **Static Members** اهمیت پیدا می‌کنند.

---

# Static Method

برای تعریف Static Method از Keyword `static` استفاده می‌کنیم:

```javascript
class BankAccount {
  static createDefault() {
    return new BankAccount(0);
  }

  constructor(balance) {
    this.balance = balance;
  }
}
```

اکنون Method:

```javascript
createDefault()
```

متعلق به خود Class است، نه Instance.

بنابراین آن را این‌گونه فراخوانی می‌کنیم:

```javascript
const account = BankAccount.createDefault();
```

نه:

```javascript
account.createDefault();
```

این تفاوت، مفهوم اصلی Static Method را مشخص می‌کند.

---

# چرا Static Method به Instance تعلق ندارد؟

برای پاسخ به این سؤال باید به مسئولیت Method نگاه کنیم.

فرض کنید:

```javascript
BankAccount.createDefault();
```

یک Account جدید ایجاد می‌کند.

برای اجرای این Operation هنوز Accountای وجود ندارد.

بنابراین منطقی نیست این Behavior را به یک Instance موجود وابسته کنیم.

Static Method زمانی مناسب است که Operation به **Class یا مفهوم کلی آن** مربوط باشد، نه به State یک Instance مشخص.

برای مثال:

```text
BankAccount.createDefault()
```

به مفهوم Bank Account مربوط است.

اما:

```text
account.deposit(1000)
```

به یک Account مشخص مربوط است.

این همان تفاوت اصلی است:

```text
Instance Method
↓
Works with an Instance

Static Method
↓
Works at Class Level
```

---

# Static Method و Utility Behavior

Static Methodها گاهی برای Operationهایی استفاده می‌شوند که به یک Instance خاص نیاز ندارند.

مثلاً:

```javascript
class User {
  static isValidEmail(email) {
    return email.includes('@');
  }
}
```

اکنون:

```javascript
User.isValidEmail('omid@example.com');
```

در اینجا Method به User خاصی نیاز ندارد.

فقط یک Value دریافت می‌کند و آن را بررسی می‌کند.

به همین دلیل قرار دادن آن در سطح Class می‌تواند منطقی باشد.

اما باید مراقب باشیم Static Method را صرفاً به دلیل اینکه «Utility» است، بی‌دلیل به Class اضافه نکنیم.

مهم این است که Operation از نظر مفهومی به Class مربوط باشد.

---

# Static Properties

Static بودن فقط برای Methodها نیست.

می‌توان Property نیز در سطح Class تعریف کرد:

```javascript
class BankAccount {
  static accountCount = 0;

  constructor(owner) {
    this.owner = owner;
    BankAccount.accountCount++;
  }
}
```

اکنون:

```javascript
const account1 = new BankAccount('Omid');
const account2 = new BankAccount('Sara');
```

مقدار:

```javascript
BankAccount.accountCount
```

برابر با تعداد Accountهای ایجادشده خواهد بود.

نکته مهم این است که:

```javascript
account1.accountCount
```

به همان شکل یک Instance Property در دسترس نیست.

زیرا `accountCount` متعلق به Class است.

---

# Instance State در برابر Class State

اکنون می‌توانیم تفاوت را به شکل دقیق‌تری ببینیم.

فرض کنید:

```javascript
class User {
  static userCount = 0;

  constructor(name) {
    this.name = name;
    User.userCount++;
  }
}
```

در این Class دو نوع State داریم.

State مربوط به Instance:

```javascript
this.name
```

و State مربوط به Class:

```javascript
User.userCount
```

اگر دو User ایجاد کنیم:

```javascript
const user1 = new User('Omid');
const user2 = new User('Sara');
```

هر Instance `name` مستقل خود را دارد:

```text
user1.name → Omid
user2.name → Sara
```

اما `userCount` متعلق به کل Class است:

```text
User.userCount → 2
```

بنابراین:

```text
Instance State
↓
Belongs to each Object

Static State
↓
Belongs to the Class
```

---

# یک مثال کامل

اکنون می‌توانیم Encapsulation و Static Members را در یک مثال واقعی‌تر کنار یکدیگر ببینیم.

فرض کنید می‌خواهیم یک `BankAccount` طراحی کنیم.

Account باید:

* Owner داشته باشد.
* Balance را نگهداری کند.
* Deposit انجام دهد.
* Withdraw انجام دهد.
* Balance را از طریق یک Getter ارائه کند.
* تعداد Accountهای ایجادشده را در سطح Class نگهداری کند.

```javascript
class BankAccount {
  static accountCount = 0;

  #balance;

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;

    BankAccount.accountCount++;
  }

  deposit(amount) {
    if (amount <= 0) {
      throw new Error('Amount must be positive');
    }

    this.#balance += amount;
  }

  withdraw(amount) {
    if (amount <= 0 || amount > this.#balance) {
      throw new Error('Invalid withdrawal');
    }

    this.#balance -= amount;
  }

  get balance() {
    return this.#balance;
  }
}
```

اکنون:

```javascript
const account1 = new BankAccount('Omid', 5000);
const account2 = new BankAccount('Sara', 3000);
```

هر Account Balance مستقل خود را دارد:

```javascript
console.log(account1.balance);
console.log(account2.balance);
```

اما تعداد Accountها متعلق به Class است:

```javascript
console.log(BankAccount.accountCount);
```

خروجی:

```text
2
```

و Code بیرونی نمی‌تواند مستقیماً `#balance` را تغییر دهد.

بنابراین:

```text
BankAccount
│
├── Static State
│   └── accountCount
│
├── Instance State
│   ├── owner
│   └── #balance
│
└── Instance Behavior
    ├── deposit()
    ├── withdraw()
    └── balance
```

این ساختار نشان می‌دهد که یک Class می‌تواند هم‌زمان State و Behavior را در دو سطح مدیریت کند:

```text
Instance Level
Class Level
```

---

# Static Method و Static Property در کنار هم

گاهی Class هم State و هم Behavior در سطح Class دارد.

برای مثال:

```javascript
class BankAccount {
  static accountCount = 0;

  static getAccountCount() {
    return BankAccount.accountCount;
  }

  constructor(owner) {
    this.owner = owner;
    BankAccount.accountCount++;
  }
}
```

اکنون:

```javascript
new BankAccount('Omid');
new BankAccount('Sara');

console.log(
  BankAccount.getAccountCount()
);
```

در اینجا:

```text
accountCount
```

State سطح Class است.

و:

```text
getAccountCount()
```

Behavior سطح Class است.

بنابراین Static Members می‌توانند یک **Class-Level API** ایجاد کنند.

---

# Static Members و Instance Members را اشتباه نگیریم

یکی از خطاهای رایج این است که تصور کنیم Static Method از طریق Instance نیز قابل دسترسی است.

مثلاً:

```javascript
class User {
  static createGuest() {
    return new User('Guest');
  }
}

const user = new User('Omid');
```

این درست است:

```javascript
User.createGuest();
```

اما این درست نیست:

```javascript
user.createGuest();
```

زیرا `createGuest()` یک Static Method است.

به همین ترتیب اگر:

```javascript
class User {
  login() {
    console.log('Logged in');
  }
}
```

داشته باشیم:

```javascript
const user = new User('Omid');
```

Method:

```javascript
user.login();
```

متعلق به Instance است.

نه:

```javascript
User.login();
```

بنابراین باید همیشه بپرسیم:

> این Behavior درباره یک Object مشخص است یا درباره خود Class؟

پاسخ همین سؤال معمولاً مشخص می‌کند که Method باید Instance باشد یا Static.

---

# Class-Level Behavior

اکنون می‌توانیم Static Members را در سطح مفهومی بالاتری ببینیم.

در OOP همه Behaviorها به Instance تعلق ندارند.

گاهی یک Class علاوه بر تعریف Instanceها، مسئولیت‌هایی در سطح خود نیز دارد.

برای مثال:

```text
User
├── Instance Behavior
│   └── login()
│
└── Class Behavior
    └── createGuest()
```

یا:

```text
BankAccount
├── Instance Behavior
│   ├── deposit()
│   └── withdraw()
│
└── Class Behavior
    └── createDefault()
```

در اینجا Static Members برای نمایش **Class-Level Behavior** استفاده می‌شوند.

این همان دلیل اصلی وجود `static` است.

نه صرفاً یک Syntax دیگر برای تعریف Method.

---

# یک مدل ذهنی برای طراحی Class

در این مرحله می‌توانیم Class را از سه زاویه ببینیم.

### 1. Public State

اطلاعاتی که می‌توانند بخشی از Public API باشند:

```javascript
this.owner
```

### 2. Private State

اطلاعات داخلی که باید تحت کنترل Object باقی بمانند:

```javascript
this.#balance
```

### 3. Class-Level State و Behavior

اطلاعات و Operationهایی که به کل Class مربوط هستند:

```javascript
BankAccount.accountCount
BankAccount.createDefault()
```

بنابراین یک Class می‌تواند چنین ساختاری داشته باشد:

```text
Class
│
├── Public Instance State
│
├── Private Instance State
│
├── Instance Behavior
│
└── Class-Level State / Behavior
```

این مدل ذهنی برای طراحی Classهای واقعی بسیار مهم است.

---

# Encapsulation و Static Members چه ارتباطی دارند؟

Encapsulation و Static Members دو مفهوم یکسان نیستند.

Encapsulation درباره **کنترل دسترسی و مسئولیت State و Behavior** است.

Static Members درباره **سطح تعلق یک State یا Behavior** هستند.

مثلاً:

```javascript
class BankAccount {
  #balance;

  static accountCount = 0;
}
```

اینجا:

```text
#balance
↓
Private Instance State

accountCount
↓
Static Class State
```

این دو مفهوم می‌توانند در یک Class کنار هم استفاده شوند، اما هرکدام یک مسئله متفاوت را حل می‌کنند.

Encapsulation می‌پرسد:

> چه چیزی باید از بیرون قابل دسترسی یا تغییر باشد؟

Static می‌پرسد:

> این State یا Behavior متعلق به یک Instance است یا به خود Class؟

این دو سؤال از نظر طراحی متفاوت هستند.

---

# Best Practices

## State را بر اساس مسئولیت واقعی آن طراحی کنید

هر Stateای را صرفاً به دلیل امکان دسترسی Public نکنید.

اگر تغییر مستقیم یک State می‌تواند قواعد Object را نقض کند، Private کردن آن و ارائه یک API کنترل‌شده را در نظر بگیرید.

---

## Private Fields را برای State داخلی استفاده کنید

برای Stateای که نباید از بیرون مستقیماً Access شود:

```javascript
#balance
```

راهکار استاندارد JavaScript است.

به `_balance` به‌عنوان Private واقعی تکیه نکنید.

---

## Getter و Setter را فقط در صورت نیاز استفاده کنید

Getter و Setter زمانی ارزش بیشتری دارند که:

* دسترسی نیاز به کنترل داشته باشد.
* Value محاسبه شود.
* Validation لازم باشد.
* Representation داخلی بخواهد از Public API جدا باشد.

برای هر Property صرفاً یک Getter و Setter ایجاد نکنید.

---

## Static را بر اساس تعلق Behavior انتخاب کنید

اگر Method به State یک Instance مشخص نیاز دارد، معمولاً Instance Method مناسب است.

اگر Operation به خود Class مربوط است و به Instance مشخصی نیاز ندارد، Static Method می‌تواند انتخاب مناسبی باشد.

---

## Public API را کوچک و روشن نگه دارید

Consumer نباید برای انجام یک Operation ساده مجبور شود با جزئیات داخلی Object کار کند.

ترجیحاً:

```javascript
account.deposit(1000);
```

به جای:

```javascript
account.balance += 1000;
```

---

# Common Mistakes

## اشتباه اول: Private کردن همه Stateها

Encapsulation به معنای Private کردن همه چیز نیست.

هدف، کنترل مناسب دسترسی است.

---

## اشتباه دوم: تصور اینکه `_property` یک Private Field واقعی است

```javascript
this._balance
```

Private واقعی نیست.

این فقط یک Convention است.

Private واقعی:

```javascript
this.#balance
```

است.

---

## اشتباه سوم: تلاش برای دسترسی مستقیم به Private Field

این کد معتبر نیست:

```javascript
account.#balance;
```

Private Field فقط از داخل Class مربوطه قابل دسترسی است.

---

## اشتباه چهارم: استفاده از Setter بدون منطق

اگر Setter فقط این کار را انجام دهد:

```javascript
set price(value) {
  this.#price = value;
}
```

بدون اینکه هیچ کنترل، Validation یا Abstraction اضافی ارائه کند، ممکن است وجود آن ضرورتی نداشته باشد.

---

## اشتباه پنجم: تصور اینکه Static Method از Instance قابل دسترسی است

اگر:

```javascript
class User {
  static createGuest() {}
}
```

داریم، باید بنویسیم:

```javascript
User.createGuest();
```

نه:

```javascript
user.createGuest();
```

---

## اشتباه ششم: یکی دانستن Static State و Instance State

این دو سطح متفاوت دارند:

```text
this.name
↓
Instance State

User.userCount
↓
Class State
```

---

## اشتباه هفتم: استفاده از Static صرفاً برای Utility بودن

اینکه یک Method به‌ظاهر Utility است، به‌تنهایی دلیل کافی برای Static کردن آن نیست.

باید بررسی کنیم آیا Behavior واقعاً به خود Class مربوط است یا نه.

---

# Summary

در این فصل از یک سؤال طراحی شروع کردیم:

> چگونه Class طراحی کنیم تا State و Behavior کنترل‌شده و قابل استفاده مجدد باشند؟

در ابتدا دیدیم که Public State ساده و قابل استفاده است، اما همیشه مناسب نیست.

اگر هر بخش Application بتواند State داخلی Object را مستقیماً تغییر دهد، ممکن است Object وارد وضعیت نامعتبر شود.

برای کنترل این وضعیت، JavaScript **Private Fields** را ارائه می‌کند:

```javascript
#balance
```

Private Field فقط از داخل Class قابل دسترسی است.

این قابلیت بخشی از **Encapsulation** را ممکن می‌کند.

Encapsulation یعنی State و Behavior را در یک Abstraction قرار دهیم و دسترسی به جزئیات داخلی را به شکل کنترل‌شده طراحی کنیم.

در نتیجه، به جای اینکه Code بیرونی مستقیماً State را تغییر دهد:

```javascript
account.balance += 1000;
```

می‌توانیم مسئولیت تغییر State را به خود Object بسپاریم:

```javascript
account.deposit(1000);
```

برای ارائه کنترل‌شده State، می‌توان از **Getter** استفاده کرد:

```javascript
get balance() {
  return this.#balance;
}
```

و اگر تغییر State نیز باید از یک نقطه کنترل‌شده عبور کند، می‌توان از **Setter** استفاده کرد:

```javascript
set price(value) {
  // validation
}
```

اما Getter و Setter ابزار هستند و نباید بدون نیاز واقعی برای همه Propertyها ایجاد شوند.

سپس دیدیم که همه State و Behaviorها به Instance تعلق ندارند.

بعضی Stateها به کل Class مربوط هستند:

```javascript
static accountCount = 0;
```

و بعضی Behaviorها نیز در سطح Class قرار می‌گیرند:

```javascript
static createDefault() {
  return new BankAccount(0);
}
```

این‌ها **Static Members** هستند.

بنابراین تفاوت اصلی را می‌توان این‌گونه خلاصه کرد:

```text
Instance Member
↓
Belongs to an Instance

Static Member
↓
Belongs to the Class
```

و از اینجا یک مدل ذهنی کامل‌تر برای Class به دست می‌آوریم:

```text
Class
│
├── Public Instance State
│
├── Private Instance State
│
├── Instance Behavior
│
└── Class-Level State / Behavior
```

Encapsulation به ما کمک می‌کند **کنترل کنیم چه چیزی از بیرون قابل دسترسی و تغییر باشد**.

Static Members به ما کمک می‌کنند **مشخص کنیم یک State یا Behavior به Instance تعلق دارد یا به خود Class**.

این دو مفهوم در کنار یکدیگر Class را از یک ساختار ساده برای ایجاد Object، به ابزاری دقیق‌تر برای **مدیریت State، کنترل Behavior و طراحی API** تبدیل می‌کنند.

---

# Key Takeaways

* Encapsulation یعنی کنترل دسترسی به State و جزئیات داخلی Object.
* Public State مستقیماً از بیرون قابل دسترسی و تغییر است.
* Private Fields با Syntax `#` تعریف می‌شوند.
* `_property` فقط یک Convention است و Private واقعی ایجاد نمی‌کند.
* Private State به Object اجازه می‌دهد قواعد داخلی و Invariantهای خود را بهتر کنترل کند.
* Getter برای کنترل یا محاسبه هنگام خواندن یک Value استفاده می‌شود.
* Setter برای کنترل یا Validation هنگام تغییر یک Value استفاده می‌شود.
* Getter و Setter نباید بدون نیاز واقعی برای همه Propertyها ایجاد شوند.
* Instance Members به یک Instance مشخص تعلق دارند.
* Static Members به خود Class تعلق دارند.
* Static Method با نام Class فراخوانی می‌شود، نه از طریق Instance.
* Static Property State مشترک در سطح Class را نمایش می‌دهد.
* انتخاب میان Instance و Static باید بر اساس **محل واقعی مسئولیت** انجام شود.
* Encapsulation و Static دو مفهوم متفاوت هستند و دو مسئله متفاوت را حل می‌کنند.
* یک Class می‌تواند هم‌زمان Public State، Private State، Instance Behavior و Class-Level Behavior داشته باشد.
* طراحی خوب Class بیشتر از حفظ Syntax به تشخیص درست **State، Behavior، Responsibility و سطح دسترسی** وابسته است.
## Technical Interview

### 1. Encapsulation در JavaScript چیست؟

**Encapsulation** یا کپسوله‌سازی، روشی برای طراحی یک Object یا Class است که در آن **State داخلی و جزئیات پیاده‌سازی کنترل می‌شوند** و تنها بخش‌هایی که سایر قسمت‌های برنامه واقعاً به آن‌ها نیاز دارند در اختیار بیرون قرار می‌گیرند.

هدف Encapsulation صرفاً مخفی کردن داده‌ها نیست. هدف اصلی این است که مشخص کنیم **چه کسی، از چه طریقی و تحت چه قوانینی می‌تواند State یک Object را مشاهده یا تغییر دهد.**

به همین دلیل، Encapsulation ارتباط مستقیمی با حفظ صحت و یکپارچگی State دارد.

---

### 2. چرا دسترسی مستقیم به Public State می‌تواند مشکل‌ساز باشد؟

وقتی یک State به‌صورت Public در دسترس باشد، هر بخش دیگری از برنامه می‌تواند آن را بدون رعایت قوانین Object تغییر دهد.

برای مثال:

```js
account.balance = -1000;
```

اگر `balance` یک Property عمومی باشد، هیچ مکانیزمی مانع قرار گرفتن Account در یک وضعیت نامعتبر نمی‌شود.

Encapsulation با محدود کردن نحوه دسترسی و تغییر State، این مشکل را کنترل می‌کند.

---

### 3. تفاوت `_property` و `#property` چیست؟

استفاده از `_` مانند:

```js
this._balance
```

تنها یک **Naming Convention** است.

یعنی برنامه‌نویس با استفاده از `_` اعلام می‌کند که این Property یک جزئیات داخلی است و نباید مستقیماً مورد استفاده قرار گیرد. اما JavaScript این موضوع را enforce نمی‌کند.

در مقابل:

```js
this.#balance
```

یک **Private Field واقعی** است که توسط خود زبان JavaScript محافظت می‌شود.

بنابراین:

> `_property` فقط یک قرارداد بین برنامه‌نویسان است، اما `#property` توسط زبان JavaScript enforce می‌شود.

---

### 4. Private Field در JavaScript چیست؟

**Private Field** فیلدی است که با علامت `#` در Class تعریف می‌شود:

```js
class Account {
  #balance = 0;
}
```

این Field تنها در Scope مربوط به همان Class قابل دسترسی است.

کد خارج از Class نمی‌تواند مستقیماً به آن دسترسی داشته باشد:

```js
account.#balance;
```

این کد معتبر نیست.

نکته مهم این است که Private Field بخشی از Public API Object محسوب نمی‌شود.

---

### 5. آیا یک Subclass می‌تواند مستقیماً به Private Field کلاس والد دسترسی داشته باشد؟

خیر.

فرض کنید Class والد چنین Private Fieldای داشته باشد:

```js
class Account {
  #balance = 0;
}
```

Subclass نمی‌تواند مستقیماً بنویسد:

```js
this.#balance;
```

زیرا `#balance` متعلق به همان Classی است که آن را تعریف کرده است.

این نکته تفاوت مهمی میان **Inheritance** و **Private State** ایجاد می‌کند.

Inheritance به Child اجازه می‌دهد از Behavior و Memberهای قابل دسترس Parent استفاده کند، اما Private Field همچنان خصوصی باقی می‌ماند.

---

### 6. Encapsulation دقیقاً از چه چیزی محافظت می‌کند؟

Encapsulation در درجه اول از **Integrity و قوانین State داخلی Object** محافظت می‌کند.

فرض کنید یک Account باید همیشه این قانون را رعایت کند:

```text
balance >= 0
```

اگر هر کدی بتواند مقدار `balance` را مستقیماً تغییر دهد، حفظ این قانون دشوار خواهد بود.

به جای آن می‌توان تغییر State را از طریق Behaviorهایی مانند این موارد کنترل کرد:

```js
deposit(amount)
withdraw(amount)
```

در این حالت خود Object مسئول کنترل تغییرات State است.

بنابراین Encapsulation باعث می‌شود Object فقط داده نگهداری نکند، بلکه **قوانین مربوط به State خودش را نیز کنترل کند.**

---

### 7. آیا برای پیاده‌سازی Encapsulation باید تمام Propertyها را Private کنیم؟

خیر.

هدف Encapsulation این نیست که همه چیز را مخفی کنیم.

برخی Stateها و Behaviorها عمداً بخشی از Public API هستند و باید در اختیار سایر بخش‌های برنامه قرار بگیرند.

سؤال اصلی این نیست که:

> «چه چیزهایی را می‌توانم Private کنم؟»

بلکه سؤال بهتر این است:

> «کدام بخش‌های این Object باید در اختیار سایر قسمت‌های برنامه قرار بگیرند؟»

بنابراین Encapsulation بیشتر درباره **Controlled Exposure** است تا Maximum Hiding.

---

### 8. Getter چیست؟

**Getter** نوعی Accessor است که اجازه می‌دهد یک Behavior با Syntax شبیه Property در اختیار مصرف‌کننده قرار گیرد.

برای مثال:

```js
class Account {
  #balance = 1000;

  get balance() {
    return this.#balance;
  }
}
```

اکنون مصرف‌کننده می‌تواند بنویسد:

```js
account.balance;
```

به جای:

```js
account.getBalance();
```

Getter زمانی بسیار مفید است که بخواهیم یک مقدار داخلی یا حتی یک مقدار محاسبه‌شده را بدون افشای مستقیم Implementation داخلی در اختیار بیرون قرار دهیم.

---

### 9. Setter چیست؟

**Setter** اجازه می‌دهد عملیات Assignment به یک Property باعث اجرای Logic مشخصی شود.

برای مثال:

```js
class User {
  #age;

  set age(value) {
    if (value < 0) {
      throw new Error("Age cannot be negative");
    }

    this.#age = value;
  }
}
```

اکنون:

```js
user.age = 25;
```

تنها یک Assignment ساده نیست؛ بلکه Setter اجرا می‌شود و می‌تواند قبل از تغییر State، مقدار را اعتبارسنجی کند.

بنابراین Setter می‌تواند بخشی از منطق کنترل State باشد.

---

### 10. آیا Getter و Setter همان Methodهای معمولی هستند؟

از نظر پیاده‌سازی، Getter و Setter با Functionها مرتبط هستند، اما از نظر نحوه استفاده، **Accessor Property** محسوب می‌شوند.

یک Method معمولی با Parentheses فراخوانی می‌شود:

```js
account.getBalance();
```

اما Getter مانند یک Property خوانده می‌شود:

```js
account.balance;
```

همچنین Setter هنگام Assignment اجرا می‌شود:

```js
account.balance = 500;
```

این تفاوت Syntax در واقع نشان‌دهنده تفاوت Interface آن‌ها برای مصرف‌کننده است.

---

### 11. چرا نباید برای تمام Propertyها Getter و Setter بسازیم؟

زیرا در این صورت ممکن است بدون نیاز، Implementation داخلی Class را در Public API قرار دهیم.

اگر برای هر Private Field یک Getter و Setter بسازیم، ممکن است Class عملاً به یک Wrapper ساده برای State داخلی خودش تبدیل شود.

برای مثال ممکن است منطقی باشد که مقدار `balance` قابل خواندن باشد:

```js
account.balance;
```

اما تغییر آن فقط از طریق Behaviorهای مشخصی انجام شود:

```js
account.deposit(100);
account.withdraw(50);
```

در این طراحی، Class همچنان کنترل تغییرات State را در اختیار دارد.

---

### 12. Static Method چیست؟

**Static Method** متدی است که به خود Class تعلق دارد، نه به Instanceهایی که از آن ساخته می‌شوند.

برای مثال:

```js
class MathUtils {
  static square(value) {
    return value * value;
  }
}
```

این Method از طریق Class فراخوانی می‌شود:

```js
MathUtils.square(5);
```

اما از طریق Instance قابل دسترسی نیست:

```js
const math = new MathUtils();

math.square(5); // Error
```

بنابراین Static Method نشان‌دهنده **Class-Level Behavior** است.

---

### 13. چرا یک Method را Static می‌کنیم؟

سؤال اصلی این است:

> آیا این Behavior به State یک Instance مشخص وابسته است؟

اگر Method به State یک Instance مانند:

```js
this.balance
this.name
this.owner
```

وابسته باشد، معمولاً باید Instance Method باشد.

اما اگر Behavior به یک Instance خاص وابسته نباشد و در سطح خود Class معنا داشته باشد، می‌تواند Static باشد.

برای مثال:

```js
class User {
  static createGuest() {
    return new User("Guest");
  }
}
```

در اینجا Method روی یک User موجود عملیاتی انجام نمی‌دهد؛ بلکه یک User جدید ایجاد می‌کند.

بنابراین متعلق بودن آن به Class منطقی‌تر است.

---

### 14. Static Property چیست؟

**Static Property**، Propertyای است که به خود Class تعلق دارد، نه به Instanceهای آن.

برای مثال:

```js
class Account {
  static accountCount = 0;
}
```

این Property از طریق Class قابل دسترسی است:

```js
Account.accountCount;
```

نه از طریق یک Instance:

```js
account.accountCount;
```

Static Property برای نگهداری Stateای مناسب است که مفهوم آن به **کل Class** مربوط می‌شود، نه به یک Object خاص.

---

### 15. تفاوت Instance State و Static State چیست؟

**Instance State** متعلق به یک Object مشخص است.

```js
class User {
  constructor(name) {
    this.name = name;
  }
}
```

هر Instance مقدار `name` مخصوص خودش را دارد.

در مقابل، **Static State** متعلق به خود Class است:

```js
class User {
  static count = 0;
}
```

اینجا `count` متعلق به `User` Class است.

می‌توان این تفاوت را به شکل زیر خلاصه کرد:

```text
Instance State → متعلق به یک Object
Static State   → متعلق به خود Class
```

این یکی از مهم‌ترین مدل‌های ذهنی برای درک Static Members است.

---

### 16. آیا Instance Method می‌تواند مستقیماً به Static Property از طریق `this` دسترسی داشته باشد؟

خیر.

در یک Instance Method:

```js
this
```

به Instance فعلی اشاره می‌کند.

بنابراین:

```js
this.count;
```

به دنبال Propertyای روی Instance می‌گردد، نه Static Property روی Class.

Static Property به خود Class تعلق دارد، بنابراین باید در Static Context یا از طریق مرجع مناسب به Class دسترسی پیدا کرد.

---

### 17. آیا Static Method می‌تواند به Instance State دسترسی داشته باشد؟

نه، دست‌کم نه از طریق `this`.

در یک Static Method:

```js
this
```

به خود Class اشاره می‌کند، نه به یک Instance.

بنابراین Static Method نمی‌تواند از طریق `this` به State مربوط به یک User یا Account خاص دسترسی داشته باشد.

اگر Static Method نیاز به یک Instance داشته باشد، آن Instance باید به‌صورت صریح در اختیار آن قرار گیرد.

---

### 18. تفاوت Static Method و Instance Method چیست؟

تفاوت اصلی به **محل تعلق Behavior** مربوط می‌شود.

Instance Method به Objectهایی تعلق دارد که از Class ساخته شده‌اند:

```js
account.deposit();
```

Static Method به خود Class تعلق دارد:

```js
Account.createDefault();
```

بنابراین:

```text
Instance Method → Behavior مربوط به یک Instance
Static Method   → Behavior مربوط به Class
```

این تفاوت را باید مفهومی درک کرد، نه صرفاً به‌عنوان تفاوتی در Syntax.

---

### 19. رابطه Encapsulation و Static Members چیست؟

این دو مفهوم یک مسئله واحد را حل نمی‌کنند.

**Encapsulation** به این سؤال پاسخ می‌دهد:

> چگونه State و Implementation داخلی Object را کنترل کنیم؟

در مقابل، **Static Members** به این سؤال پاسخ می‌دهند:

> آیا این State یا Behavior به یک Instance خاص تعلق دارد یا به کل Class؟

این دو مفهوم می‌توانند در کنار یکدیگر استفاده شوند.

برای مثال یک `Account` می‌تواند برای هر Instance، `balance` خصوصی داشته باشد و هم‌زمان یک Static Property برای نگهداری تعداد Accountهای ایجادشده داشته باشد.

---

### 20. آیا JavaScript از Private Static Field پشتیبانی می‌کند؟

بله.

Static Field نیز می‌تواند Private باشد:

```js
class User {
  static #count = 0;
}
```

در این حالت Field هم **Static** است و هم **Private**.

یعنی دو سؤال مستقل را هم‌زمان پاسخ می‌دهیم:

```text
این State به چه چیزی تعلق دارد؟
→ Class

چه کسی می‌تواند به آن دسترسی داشته باشد؟
→ فقط خود Class
```

این ترکیب نشان می‌دهد که `static` و `private` دو مفهوم مستقل هستند و می‌توانند با یکدیگر ترکیب شوند.

---

### 21. Public API یک Class چیست؟

**Public API** مجموعه‌ای از Propertyها و Methodهایی است که Class عمداً برای استفاده سایر قسمت‌های برنامه در اختیار قرار می‌دهد.

برای مثال:

```js
account.deposit(100);
account.withdraw(50);
account.balance;
```

این‌ها می‌توانند بخشی از Public API یک Account باشند.

در مقابل، جزئیات Implementation مانند Private Fields نباید الزاماً بخشی از API باشند.

بنابراین یک Class خوب فقط State و Behavior ندارد؛ بلکه مشخص می‌کند:

> «چه چیزی را نشان بدهم، چه چیزی را پنهان کنم و چه قوانینی را برای استفاده از آن تعیین کنم؟»

این همان نقطه‌ای است که Encapsulation از یک ویژگی Syntaxی به یک **اصل طراحی** تبدیل می‌شود.

---

## Conclusion

در این فصل، Encapsulation و Static Members را نه به‌عنوان چند ویژگی مستقل Syntax، بلکه به‌عنوان ابزارهایی برای **طراحی بهتر Class** بررسی کردیم.

در ابتدا دیدیم که اگر State یک Object بدون کنترل در اختیار سایر بخش‌های برنامه قرار گیرد، هر کدی می‌تواند آن را تغییر دهد و Object را وارد وضعیتی نامعتبر کند. از همین نیاز، مفهوم **Encapsulation** شکل می‌گیرد: Object باید تا حد امکان مسئول کنترل State و قوانین مربوط به تغییر آن باشد.

سپس با **Private Fields** آشنا شدیم و دیدیم که JavaScript با استفاده از `#` امکان ایجاد State واقعاً Private را فراهم می‌کند. در مقابل، `_property` تنها یک قرارداد نام‌گذاری است و از نظر زبان محدودیتی ایجاد نمی‌کند.

بعد از آن به **Getters و Setters** رسیدیم. این‌ها راهی برای طراحی یک Public API کنترل‌شده هستند؛ به‌خصوص زمانی که لازم است خواندن یا تغییر یک مقدار با Logic مشخصی همراه باشد. اما نکته مهم این بود که Encapsulation به معنای قرار دادن Getter و Setter برای همه Propertyها نیست. هدف، **Expose کردن رفتار موردنیاز و پنهان کردن Implementation غیرضروری** است.

در بخش دوم فصل، تمرکز از Instance به Class تغییر کرد. دیدیم که بعضی Behaviorها به یک Object مشخص تعلق ندارند و بعضی Stateها نیز مربوط به یک Instance خاص نیستند. اینجا مفهوم **Static Members** معنا پیدا می‌کند.

یک Instance Method در زمینه یک Object مشخص اجرا می‌شود:

```js
account.deposit(100);
```

در حالی که یک Static Method در سطح Class قرار دارد:

```js
Account.createDefault();
```

به همین ترتیب، Instance State متعلق به یک Object است، در حالی که Static State متعلق به خود Class است.

در نهایت، می‌توانیم تمام این مفاهیم را در یک مدل ذهنی ساده کنار هم قرار دهیم:

```text
Class
 │
 ├── Instance Members
 │     ├── Instance State
 │     └── Instance Behavior
 │
 └── Static Members
       ├── Class State
       └── Class Behavior
```

و در هر دو سطح می‌توان میزان دسترسی را نیز کنترل کرد:

```text
                Public        Private
Instance        Public        #private
Static          static        static #
```

اما مهم‌تر از Syntax، تصمیم طراحی پشت این مفاهیم است.

هنگام طراحی یک Class باید بتوانیم به سه سؤال پاسخ دهیم:

1. **این State یا Behavior متعلق به یک Instance است یا به خود Class؟**
2. **آیا این بخش باید در اختیار کد بیرونی قرار بگیرد یا یک Implementation Detail است؟**
3. **اگر State قابل تغییر است، چه قوانینی باید هنگام تغییر آن رعایت شوند؟**

اگر این سؤال‌ها را درست پاسخ دهیم، استفاده از `private`، `get`، `set` و `static` دیگر انتخاب‌هایی تصادفی نخواهند بود؛ بلکه نتیجه طبیعی یک تصمیم طراحی خواهند بود.

در این نقطه، مسیر OOP که از Object و Class آغاز کردیم، یک گام مهم جلوتر می‌رود: **Class دیگر فقط قالبی برای ساخت Object نیست؛ بلکه مرزی برای State، Behavior و مسئولیت ایجاد می‌کند.**

این نگاه، پایه‌ای مهم برای طراحی Objectهای قابل اعتماد، قابل نگهداری و قابل توسعه در JavaScript است.
