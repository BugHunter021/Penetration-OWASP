
<div dir="rtl">

# Clean Code در PHP

## فهرست 

  1. [مقدمه](#introduction)
  2. [متغیرها](#variables)
     * [از نام های متغیر معنی دار و قابل تلفظ استفاده کنید](#use-meaningful-and-pronounceable-variable-names)
     * [از همان واژگان برای همان نوع متغیر استفاده کنید](#use-the-same-vocabulary-for-the-same-type-of-variable)
     * [از نام های قابل جستجو استفاده کنید (بخش 1)](#use-searchable-names-part-1)
     * [از نام های قابل جستجو استفاده کنید (بخش 2)](#use-searchable-names-part-2)
     * [از متغیرهای توضیحی استفاده کنید](#use-explanatory-variables)
     * [خودداری از ساختارهای تودرتو (بخش 1)](#avoid-nesting-too-deeply-and-return-early-part-1)
     * [خودداری از ساختارهای تودرتو (بخش 2)](#avoid-nesting-too-deeply-and-return-early-part-2)
     * [از نگاشت ذهنی خودداری کنید](#avoid-mental-mapping)
     * [نیاز به تکرار نام شی در متغیرها نیست](#dont-add-unneeded-context)
     * [از آرگومان های پیش فرض به جای اتصال کوتاه یا شرطی استفاده کنید](#use-default-arguments-instead-of-short-circuiting-or-conditionals)
  3. [ساختارهای مقایسه ای](#comparison)
     * [از مقایسه با نوع یکسان استفاده کنید](#use-identical-comparison)
     * [اپراتورهای اتصال NULL](#null-coalescing-operator)
  4. [توابع](#functions)
     * [آرگومان های توابع (حالت ایده آل 2 آرگومان یا کمتر)](#function-arguments-2-or-fewer-ideally)
     * [نام تابع باید بگوید که چه کاری انجام می دهد](#function-names-should-say-what-they-do)
     * [توابع فقط باید شامل یک سطح انتزاع باشند](#functions-should-only-be-one-level-of-abstraction)
     * [از flagها به عنوان پارامترهای توابع استفاده نکنید](#dont-use-flags-as-function-parameters)
     * [از عوارض جانبی یا Side Effects خودداری کنید](#avoid-side-effects)
     * [از نوشتن توابع عمومی جلوگیری کنید](#dont-write-to-global-functions)
     * [از Singleton pattern استفاده نکنید](#dont-use-a-singleton-pattern)
     * [دستورات شرطی را کپسوله سازی کنید](#encapsulate-conditionals)
     * [از استفاده ی شروط منفی جلوگیری کنید](#avoid-negative-conditionals)
     * [از شرطی شدن اجتناب کنید](#avoid-conditionals)
     * [از بررسی نوع خودداری کنید (بخش 1)](#avoid-type-checking-part-1)
     * [از بررسی نوع خودداری کنید (بخش 2)](#avoid-type-checking-part-2)
     * [حذف کد مرده](#remove-dead-code)
  5. [اشیا و ساختارهای داده](#objects-and-data-structures)
     * [استفاده از کپسوله سازی اشیا](#use-object-encapsulation)
     * [اعضای اشیا را با public, protected, private تعریف کنید](#make-objects-have-privateprotected-members)
  6. [کلاس ها](#classes)
     * [ترکیب را بر ارث بری ترجیح دهید](#prefer-composition-over-inheritance)
     * [از استفاده ی fluent interfaces خودکاری کنید](#avoid-fluent-interfaces)
     * [Prefer final classes](#prefer-final-classes)
  7. [SOLID](#solid)
     * [Single Responsibility Principle (SRP)](#single-responsibility-principle-srp)
     * [Open/Closed Principle (OCP)](#openclosed-principle-ocp)
     * [Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)
     * [Interface Segregation Principle (ISP)](#interface-segregation-principle-isp)
     * [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)
  8. [اصل DRY](#dont-repeat-yourself-dry)
  9. [ترجمه ها](#translations)

## مقدمه

اصول مهندسی نرم افزار ، از کتاب Robert Code Martin [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ، اقتباس گرفته شده است تا در این نوشته اصول آن برای زبان برنامه نویسی PHP را بررسی کنیم. این یک راهنمای ساده نیست. این یک راهنما برای تولید نرم افزارهای قابل خواندن با خوانایی بالا، قابل استفاده مجدد در زبان برنامه نویسی پی اچ پی است.

لازم نیست که هر اصلی که گفته شد دقیقاً رعایت شود، و حتی تعداد کمتر مورد توافق همه توسعه دهدگان قرار خواهند گرفت. اینها رهنمودهایی هستند و چیز دیگری فراتر از این نیستند ، اما مواردی هستند که در طول چندین سال تجربه جمعی توسط نویسندگان Clean Code نوشته شده است.

Inspired from [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript).

اگرچه بسیاری از توسعه دهندگان هنوز از PHP 5 استفاده می کنند ، اما بیشتر نمونه های این مقاله فقط با PHP 7.1+ کار می کنند.

## متغیرها

### از نام های متغیر معنی دار و قابل تلفظ استفاده کنید:

**بد:**

</div>

```php
declare(strict_types=1);

$ymdstr = $moment->format('y-m-d');
```
<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

$currentDate = $moment->format('y-m-d');
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از همان واژگان برای همان نوع متغیر استفاده کنید:

**بد:**

</div>

```php
declare(strict_types=1);

getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

getUser();
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از نام های قابل جستجو استفاده کنید (بخش 1)

ما بیشتر از آنچه را که می نویسیم کدها را می خوانیم. مهم است که کدی که می نویسیم قابل خواندن و قابل جستجو باشد. با انتخاب نام برای متغیرهایی که در نهایت برای درک برنامه ما معنی دار تر هستند کدهای خود را ساده تر می کنیم. نام های متغیرهای خود را در برنامه ی مورد نظرتان قابل جستجو انتخاب کنید.

**بد:**

</div>

```php
declare(strict_types=1);

// What the heck is 448 for?
$result = $serializer->serialize($data, 448);
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

$json = $serializer->serialize($data, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
```

<div dir="rtl">

### از نام های قابل جستجو استفاده کنید (بخش 2)

**بد:**

</div>

```php
declare(strict_types=1);

class User
{
    // What the heck is 7 for?
    public $access = 7;
}

// What the heck is 4 for?
if ($user->access & 4) {
    // ...
}

// What's going on here?
$user->access ^= 2;
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class User
{
    public const ACCESS_READ = 1;

    public const ACCESS_CREATE = 2;

    public const ACCESS_UPDATE = 4;

    public const ACCESS_DELETE = 8;

    // User as default can read, create and update something
    public $access = self::ACCESS_READ | self::ACCESS_CREATE | self::ACCESS_UPDATE;
}

if ($user->access & User::ACCESS_UPDATE) {
    // do edit ...
}

// Deny access rights to create something
$user->access ^= User::ACCESS_CREATE;
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از متغیرهای توضیحی استفاده کنید

**بد:**

</div>

```php
declare(strict_types=1);

$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches[1], $matches[2]);
```

<div dir="rtl">

**بدک نیست:**

این حالت از حالت بالایی بهتر است، اما هنوز به شدت به regex وابسته هستیم.

</div>

```php
declare(strict_types=1);

$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

[, $city, $zipCode] = $matches;
saveCityZipCode($city, $zipCode);
```

<div dir="rtl">

**خوب:**

با نامگذاری الگوهای فرعی وابستگی به regex را کاهش می دهیم.

</div>

```php
declare(strict_types=1);

$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(?<city>.+?)\s*(?<zipCode>\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches['city'], $matches['zipCode']);
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### خودداری از ساختارهای تودرتو (بخش 1)

با if-else های زیاد و طولانی و تو در تو خواندن کد شما دشوار می شود. برای این کار از متغیرهای صریح استفاده کنید.

**بد:**

</div>

```php
declare(strict_types=1);

function isShopOpen($day): bool
{
    if ($day) {
        if (is_string($day)) {
            $day = strtolower($day);
            if ($day === 'friday') {
                return true;
            } elseif ($day === 'saturday') {
                return true;
            } elseif ($day === 'sunday') {
                return true;
            }
            return false;
        }
        return false;
    }
    return false;
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function isShopOpen(string $day): bool
{
    if (empty($day)) {
        return false;
    }

    $openingDays = ['friday', 'saturday', 'sunday'];

    return in_array(strtolower($day), $openingDays, true);
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### خودداری از ساختارهای تودرتو (بخش 2)

**بد:**

</div>

```php
declare(strict_types=1);

function fibonacci(int $n)
{
    if ($n < 50) {
        if ($n !== 0) {
            if ($n !== 1) {
                return fibonacci($n - 1) + fibonacci($n - 2);
            }
            return 1;
        }
        return 0;
    }
    return 'Not supported';
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function fibonacci(int $n): int
{
    if ($n === 0 || $n === 1) {
        return $n;
    }

    if ($n >= 50) {
        throw new Exception('Not supported');
    }

    return fibonacci($n - 1) + fibonacci($n - 2);
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از نگاشت ذهنی خودداری کنید

سعی کنید کاری کنید که کسی که کد شما را می خواند نیاز به ترجمه نداشته باشد و به صورت صریع متوجه شود.


**بد:**

</div>

```php
$l = ['Austin', 'New York', 'San Francisco'];

for ($i = 0; $i < count($l); $i++) {
    $li = $l[$i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Wait, what is `$li` for again?
    dispatch($li);
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

$locations = ['Austin', 'New York', 'San Francisco'];

foreach ($locations as $location) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch($location);
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### نیاز به تکرار نام شی در متغیرها نیست

برای نام گذاری اعضای یک کلاس نیاز به استفاده از نام کلاس در نام آن ها نیست.

**بد:**

</div>

```php
declare(strict_types=1);

class Car
{
    public $carMake;

    public $carModel;

    public $carColor;

    //...
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class Car
{
    public $make;

    public $model;

    public $color;

    //...
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از آرگومان های پیش فرض به جای اتصال کوتاه یا شرطی استفاده کنید

**خیلی خوب نیست:**

استفاده از `$breweryName` مناسب نیست زیرا می تواند `NULL` باشد.

</div>

```php
function createMicrobrewery($breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

<div dir="rtl">

**بدک نیست:**

این حالت بهتر از حالت قبلی است زیرا قابل درک تر است اما ارزش متغیر را نیز می توان بهتر کنترل کرد.

</div>

```php
function createMicrobrewery($name = null): void
{
    $breweryName = $name ?: 'Hipster Brew Co.';
    // ...
}
```

<div dir="rtl">

**خوب:**

می توانید از [type hinting](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration) استفاده کنید و مطمئن باشید که مقدار `$breweryName` برابر `NULL` نخواهد بود.

</div>

```php
function createMicrobrewery(string $breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## ساختارهای مقایسه ای

### از مقایسه با نوع یکسان استفاده کنید - [identical comparison](http://php.net/manual/en/language.operators.comparison.php)

**خیلی خوب نیست:**

در مقایسه ی ساده رشته ها و اعداد رشته به صورت یکسان در نظر گرفته می شود.

</div>

```php
declare(strict_types=1);

$a = '42';
$b = 42;

if ($a != $b) {
    // The expression will always pass
}
```

<div dir="rtl">

مقایسه متغیر a و b برابر `FALSE` است اما در واقع `TRUE` است! رشته 42 با عدد 42 متفاوت است.


**خوب:**

در این حالت مقایسه هم با نوع متغیر و هم با مقدار آن انجام می شود.

</div>

```php
declare(strict_types=1);

$a = '42';
$b = 42;

if ($a !== $b) {
    // The expression is verified
}
```

<div dir="rtl">

مقایسه ی a با b در کد بالا برابر `TRUE` خواهد شد.

**[⬆ برگشت به بالا](#table-of-contents)**

### اپراتورهای اتصال NULL

null coalescing عملگر جدیدی است که در [introduced in PHP 7](https://www.php.net/manual/en/migration70.new-features.php) معرفی شده است. null coalescing را با علامت `??` می نویسیم که اگر درست باشد عملوند اول خود را برمی گرداند در غیر اینصورت عملوند دوم خود را بر می گرداند؛ به طور کلی به جای `isset` استفاده می شود.

**بد:**

</div>

```php
declare(strict_types=1);

if (isset($_GET['name'])) {
    $name = $_GET['name'];
} elseif (isset($_POST['name'])) {
    $name = $_POST['name'];
} else {
    $name = 'nobody';
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

$name = $_GET['name'] ?? $_POST['name'] ?? 'nobody';
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## توابع

### آرگومان های توابع (حالت ایده آل 2 آرگومان یا کمتر)

محدود کردن مقدار پارامترهای توابع بسیار مهم است زیرا تست عملکرد کدهای شما آسان تر می شود. داشتن بیش از سه مورد منجر به سختی در عملیات تست و ردگیری خطا می شود.

تعداد صفر ایده آل است؛ بین یک تا دو خوب است؛ بیش از سه مناسب نیست.

اگر بیش از سه آرگومان دارید احتمالا چند کار را در یک کار انجام می دهید که نباید اینگونه باشد و احتمالا استدلال شما از یک شی نادرست است.

**بد:**

</div>

```php
declare(strict_types=1);

class Questionnaire
{
    public function __construct(
        string $firstname,
        string $lastname,
        string $patronymic,
        string $region,
        string $district,
        string $city,
        string $phone,
        string $email
    ) {
        // ...
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class Name
{
    private $firstname;

    private $lastname;

    private $patronymic;

    public function __construct(string $firstname, string $lastname, string $patronymic)
    {
        $this->firstname = $firstname;
        $this->lastname = $lastname;
        $this->patronymic = $patronymic;
    }

    // getters ...
}

class City
{
    private $region;

    private $district;

    private $city;

    public function __construct(string $region, string $district, string $city)
    {
        $this->region = $region;
        $this->district = $district;
        $this->city = $city;
    }

    // getters ...
}

class Contact
{
    private $phone;

    private $email;

    public function __construct(string $phone, string $email)
    {
        $this->phone = $phone;
        $this->email = $email;
    }

    // getters ...
}

class Questionnaire
{
    public function __construct(Name $name, City $city, Contact $contact)
    {
        // ...
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### نام تابع باید بگوید که چه کاری انجام می دهد

**بد:**

</div>

```php
class Email
{
    //...

    public function handle(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// What is this? A handle for the message? Are we writing to a file now?
$message->handle();
```

<div dir="rtl">

**خوب:**

</div>

```php
class Email
{
    //...

    public function send(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// Clear and obvious
$message->send();
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### توابع فقط باید شامل یک سطح انتزاع باشند

وقتی بیش از یک سطح انتزاع داشته باشید ، عملکرد شما معمولاً پیچیده تر می شود؛ تقسیم توابع منجر به استفاده مجدد و تست آسان تر می شود.

**بد:**

</div>

```php
declare(strict_types=1);

function parseBetterPHPAlternative(string $code): void
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            // ...
        }
    }

    $ast = [];
    foreach ($tokens as $token) {
        // lex...
    }

    foreach ($ast as $node) {
        // parse...
    }
}
```

<div dir="rtl">

**خیلی بد نیست:**

</div>

```php
function tokenize(string $code): array
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            $tokens[] = /* ... */;
        }
    }

    return $tokens;
}

function lexer(array $tokens): array
{
    $ast = [];
    foreach ($tokens as $token) {
        $ast[] = /* ... */;
    }

    return $ast;
}

function parseBetterPHPAlternative(string $code): void
{
    $tokens = tokenize($code);
    $ast = lexer($tokens);
    foreach ($ast as $node) {
        // parse...
    }
}
```

<div dir="rtl">

**خوب:**

بهترین راه حل ، حذف وابستگی های تابع `parseBetterPHPAlternative()` است.

</div>

```php
class Tokenizer
{
    public function tokenize(string $code): array
    {
        $regexes = [
            // ...
        ];

        $statements = explode(' ', $code);
        $tokens = [];
        foreach ($regexes as $regex) {
            foreach ($statements as $statement) {
                $tokens[] = /* ... */;
            }
        }

        return $tokens;
    }
}

class Lexer
{
    public function lexify(array $tokens): array
    {
        $ast = [];
        foreach ($tokens as $token) {
            $ast[] = /* ... */;
        }

        return $ast;
    }
}

class BetterPHPAlternative
{
    private $tokenizer;
    private $lexer;

    public function __construct(Tokenizer $tokenizer, Lexer $lexer)
    {
        $this->tokenizer = $tokenizer;
        $this->lexer = $lexer;
    }

    public function parse(string $code): void
    {
        $tokens = $this->tokenizer->tokenize($code);
        $ast = $this->lexer->lexify($tokens);
        foreach ($ast as $node) {
            // parse...
        }
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از flagها به عنوان پارامترهای توابع استفاده نکنید

flagها به کاربر شما می گویند که این تابع بیش از یک کار انجام می دهد. توابع باید یک کار انجام دهند. اگر توابع شما براساس کدهای boolean دنبال می شوند، توابع خود را تجزیه کنید.

**بد:**

</div>

```php
declare(strict_types=1);

function createFile(string $name, bool $temp = false): void
{
    if ($temp) {
        touch('./temp/' . $name);
    } else {
        touch($name);
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function createFile(string $name): void
{
    touch($name);
}

function createTempFile(string $name): void
{
    touch('./temp/' . $name);
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از عوارض جانبی یا Side Effects خودداری کنید

به جای اینکه مقدار یک تابع در یک متغیر global نوشته شود به صورت غیر void آنرا تعریف کنید و مقدار را برگردانید.

**بد:**

</div>

```php
declare(strict_types=1);

// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
$name = 'Ryan McDermott';

function splitIntoFirstAndLastName(): void
{
    global $name;

    $name = explode(' ', $name);
}

splitIntoFirstAndLastName();

var_dump($name);
// ['Ryan', 'McDermott'];
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function splitIntoFirstAndLastName(string $name): array
{
    return explode(' ', $name);
}

$name = 'Ryan McDermott';
$newName = splitIntoFirstAndLastName($name);

var_dump($name);
// 'Ryan McDermott';

var_dump($newName);
// ['Ryan', 'McDermott'];
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از نوشتن توابع عمومی جلوگیری کنید.

مفهوم Polluting globals یک عمل بد در بسیاری از زبان های برنامه نویسی می باشد چون شما می توانید با یک کتابخانه یا API دیگری این کار را انجام دهید.

برای مثال: اگر بخواهید یک آرایه ی configuration بسازید چه می کنید؟ شما می توانید یک تابع عمومی `config()` بنویسید یا با یک کتابخانه ی دیگر این کار را انجام دهید.

**بد:**

</div>

```php
declare(strict_types=1);

function config(): array
{
    return [
        'foo' => 'bar',
    ];
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class Configuration
{
    private $configuration = [];

    public function __construct(array $configuration)
    {
        $this->configuration = $configuration;
    }

    public function get(string $key): ?string
    {
        // null coalescing operator
        return $this->configuration[$key] ?? null;
    }
}
```

<div dir="rtl">

configuration را بارگیری کنید و نمونه از از کلاس `Configuration` ایجاد کنید

</div>

```php
declare(strict_types=1);

$configuration = new Configuration([
    'foo' => 'bar',
]);
```

<div dir="rtl">

حالا باید یک نمونه از `Configuration` استفاده کنید


**[⬆ برگشت به بالا](#table-of-contents)**

### از Singleton pattern استفاده نکنید

</div>

Singleton is an [anti-pattern](https://en.wikipedia.org/wiki/Singleton_pattern). Paraphrased from Brian Button:
 1. They are generally used as a **global instance**, why is that so bad? Because **you hide the dependencies** of your application in your code, instead of exposing them through the interfaces. Making something global to avoid passing it around is a [code smell](https://en.wikipedia.org/wiki/Code_smell).
 2. They violate the [single responsibility principle](#single-responsibility-principle-srp): by virtue of the fact that **they control their own creation and lifecycle**.
 3. They inherently cause code to be tightly [coupled](https://en.wikipedia.org/wiki/Coupling_%28computer_programming%29). This makes faking them out under **test rather difficult** in many cases.
 4. They carry state around for the lifetime of the application. Another hit to testing since **you can end up with a situation where tests need to be ordered** which is a big no for unit tests. Why? Because each unit test should be independent from the other.

There is also very good thoughts by [Misko Hevery](http://misko.hevery.com/about/) about the [root of problem](http://misko.hevery.com/2008/08/25/root-cause-of-singletons/).

<div dir="rtl">

**بد:**

</div>

```php
declare(strict_types=1);

class DBConnection
{
    private static $instance;

    private function __construct(string $dsn)
    {
        // ...
    }

    public static function getInstance(): self
    {
        if (self::$instance === null) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    // ...
}

$singleton = DBConnection::getInstance();
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class DBConnection
{
    public function __construct(string $dsn)
    {
        // ...
    }

    // ...
}
```

<div dir="rtl">

نمونه ای از کلاس DBConnection ایجاد کرده و آن را با [DSN](http://php.net/manual/en/pdo.construct.php#refsect1-pdo.construct-parameters) پیکربندی کنید.

</div>

```php
declare(strict_types=1);

$connection = new DBConnection($dsn);
```

<div dir="rtl">

و اکنون باید از نمونه `DBConnection` در برنامه خود استفاده کنید.

**[⬆ برگشت به بالا](#table-of-contents)**

### دستورات شرطی را کپسوله سازی کنید

**بد:**

</div>

```php
declare(strict_types=1);

if ($article->state === 'published') {
    // ...
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

if ($article->isPublished()) {
    // ...
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از استفاده ی شروط منفی جلوگیری کنید

**بد:**

</div>

```php
declare(strict_types=1);

function isDOMNodeNotPresent(DOMNode $node): bool
{
    // ...
}

if (! isDOMNodeNotPresent($node)) {
    // ...
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function isDOMNodePresent(DOMNode $node): bool
{
    // ...
}

if (isDOMNodePresent($node)) {
    // ...
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از شرطی شدن اجتناب کنید

به نظر می رسد این یک کار غیرممکن است. با شنیدن این موضوع احتمالا همه ی برنامه نویسان عزیز می گویند "چگونه قرار است بدون if ، کاری انجام دهیم؟" پاسخ این است که شما می توانید برای رسیدن به همان کار در بسیاری از موارد از چند case استفاده کنید. سوال دوم معمولاً این است ، "خوب این عالی است اما چرا من می خواهم این کار را انجام دهم؟" پاسخ این است که مفهوم قبلی که در ارتباط با clean code خواندیم این است که یک تابع فقط باید یک کار واحد انجام دهد. وقتی کلاس ها و توابعی را دارید که دستور if دارند ، به خواننده ی کد خود می گویید عملکرد این بخش شما بیش از یک کار را انجام می دهد. به یاد داشته باشید ، فقط یک کار را باید انجام دهید.

**بد:**

</div>

```php
declare(strict_types=1);

class Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        switch ($this->type) {
            case '777':
                return $this->getMaxAltitude() - $this->getPassengerCount();
            case 'Air Force One':
                return $this->getMaxAltitude();
            case 'Cessna':
                return $this->getMaxAltitude() - $this->getFuelExpenditure();
        }
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

interface Airplane
{
    // ...

    public function getCruisingAltitude(): int;
}

class Boeing777 implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getPassengerCount();
    }
}

class AirForceOne implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude();
    }
}

class Cessna implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getFuelExpenditure();
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از بررسی نوع خودداری کنید (بخش 1)

در زبان برنامه نویسی PHP بررسی نوع نداریم اگر برای این موضوع وسواس دارید باید با یک تابع استفاده کنید که بهتر است این کار را انجام ندهید.

**بد:**

</div>

```php
declare(strict_types=1);

function travelToTexas($vehicle): void
{
    if ($vehicle instanceof Bicycle) {
        $vehicle->pedalTo(new Location('texas'));
    } elseif ($vehicle instanceof Car) {
        $vehicle->driveTo(new Location('texas'));
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function travelToTexas(Vehicle $vehicle): void
{
    $vehicle->travelTo(new Location('texas'));
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از بررسی نوع خودداری کنید (بخش 2)

اگر با مقادیر ابتدایی اولیه مانند رشته ها ، اعداد صحیح و آرایه ها کار می کنید و از PHP 7+ استفاده می کنید و نمی توانید از polymorphism استفاده کنید اما هنوز هم به بررسی نوع نیاز دارید ، باید اعلام نوع یا حالت سخت را در نظر بگیرید. نوع ایستا در بالای نحو استاندارد PHP را برای شما فراهم می کند. مشکلی که در بررسی دستی نوع وجود دارد این است که انجام آن به بخش اضافی نیاز دارد که type-safety ساختگی شما خوانایی از دست رفته را جبران نمی کند.

کدهای PHP خود را به صورت Clean Code نگه دارید ، تست های خوبی بنویسید و بررسی های خوبی در مورد کد داشته باشید. در غیر این صورت ، همه این کارها با اعلام نوع دقیق PHP یا حالت دقیق انجام دهید.

**بد:**

</div>

```php
declare(strict_types=1);

function combine($val1, $val2): int
{
    if (! is_numeric($val1) || ! is_numeric($val2)) {
        throw new Exception('Must be of type Number');
    }

    return $val1 + $val2;
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function combine(int $val1, int $val2): int
{
    return $val1 + $val2;
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### حذف کد مرده

کد مرده به همان اندازه ای که کد تکراری بد است نامناسب است. هیچ دلیلی وجود ندارد که آن را در کدهای خود نگه دارید. اگر کدی فراخوانی نمی شود، خلاص شوید!

**بد:**

</div>

```php
declare(strict_types=1);

function oldRequestModule(string $url): void
{
    // ...
}

function newRequestModule(string $url): void
{
    // ...
}

$request = newRequestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function requestModule(string $url): void
{
    // ...
}

$request = requestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**


## اشیا و ساختارهای داده

### استفاده از کپسوله سازی اشیا

در PHP می توانید کلمات کلیدی `public`, `protected`, `privaten` داریم که با استفاده از آن ، می توانید دسترسی به ویژگی ها را روی یک اشیا کنترل کنید.

</div>

* When you want to do more beyond getting an object property, you don't have
to look up and change every accessor in your codebase.
* Makes adding validation simple when doing a `set`.
* Encapsulates the internal representation.
* Easy to add logging and error handling when getting and setting.
* Inheriting this class, you can override default functionality.
* You can lazy load your object's properties, let's say getting it from a
server.

Additionally, this is part of [Open/Closed](#openclosed-principle-ocp) principle.

<div dir="rtl">

**بد:**

</div>

```php
declare(strict_types=1);

class BankAccount
{
    public $balance = 1000;
}

$bankAccount = new BankAccount();

// Buy shoes...
$bankAccount->balance -= 100;
```

<div dir="rtl">

**خوب:**

</div>

```php
class BankAccount
{
    private $balance;

    public function __construct(int $balance = 1000)
    {
      $this->balance = $balance;
    }

    public function withdraw(int $amount): void
    {
        if ($amount > $this->balance) {
            throw new \Exception('Amount greater than available balance.');
        }

        $this->balance -= $amount;
    }

    public function deposit(int $amount): void
    {
        $this->balance += $amount;
    }

    public function getBalance(): int
    {
        return $this->balance;
    }
}

$bankAccount = new BankAccount();

// Buy shoes...
$bankAccount->withdraw($shoesPrice);

// Get balance
$balance = $bankAccount->getBalance();
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### اعضای اشیا را با protected, private تعریف کنید

برای اطلاعات بیشتر [این نوشته](http://fabien.potencier.org/pragmatism-over-theory-protected-vs-private.html) را از [Fabien Potencier](https://github.com/fabpot) بخوانید.


**بد:**

</div>

```php
declare(strict_types=1);

class Employee
{
    public $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }
}

$employee = new Employee('John Doe');
// Employee name: John Doe
echo 'Employee name: ' . $employee->name;
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class Employee
{
    private $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}

$employee = new Employee('John Doe');
// Employee name: John Doe
echo 'Employee name: ' . $employee->getName();
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## کلاس ها

### ترکیب را بر ارث بری ترجیح دهید

ابتدا بهتر است کمی در ارتباط با [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns)ها بخوانید.

**بد**

</div>

```php
declare(strict_types=1);

class Employee
{
    private $name;

    private $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    // ...
}

// Bad because Employees "have" tax data.
// EmployeeTaxData is not a type of Employee

class EmployeeTaxData extends Employee
{
    private $ssn;

    private $salary;

    public function __construct(string $name, string $email, string $ssn, string $salary)
    {
        parent::__construct($name, $email);

        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}
```

<div dir="rtl">

**خوب*

</div>

```php
declare(strict_types=1);

class EmployeeTaxData
{
    private $ssn;

    private $salary;

    public function __construct(string $ssn, string $salary)
    {
        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}

class Employee
{
    private $name;

    private $email;

    private $taxData;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function setTaxData(EmployeeTaxData $taxData): void
    {
        $this->taxData = $taxData;
    }

    // ...
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### از استفاده ی fluent interfaces خودکاری کنید

در ارتباط با [Fluent interface](https://en.wikipedia.org/wiki/Fluent_interface) ها بخوانید؛ برای بهتر متوجه شدن این موضوع این نوشته را از Marco Pivetta بخوانید.

</div>

While there can be some contexts, frequently builder objects, where this
pattern reduces the verbosity of the code (for example the [PHPUnit Mock Builder](https://phpunit.de/manual/current/en/test-doubles.html)
or the [Doctrine Query Builder](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)),
more often it comes at some costs:

1. Breaks [Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_%28object-oriented_programming%29).
2. Breaks [Decorators](https://en.wikipedia.org/wiki/Decorator_pattern).
3. Is harder to [mock](https://en.wikipedia.org/wiki/Mock_object) in a test suite.
4. Makes diffs of commits harder to read.

<div dir="rtl">

**بد:**

</div>

```php
declare(strict_types=1);

class Car
{
    private $make = 'Honda';

    private $model = 'Accord';

    private $color = 'white';

    public function setMake(string $make): self
    {
        $this->make = $make;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function setModel(string $model): self
    {
        $this->model = $model;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function setColor(string $color): self
    {
        $this->color = $color;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = (new Car())
    ->setColor('pink')
    ->setMake('Ford')
    ->setModel('F-150')
    ->dump();
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class Car
{
    private $make = 'Honda';

    private $model = 'Accord';

    private $color = 'white';

    public function setMake(string $make): void
    {
        $this->make = $make;
    }

    public function setModel(string $model): void
    {
        $this->model = $model;
    }

    public function setColor(string $color): void
    {
        $this->color = $color;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = new Car();
$car->setColor('pink');
$car->setMake('Ford');
$car->setModel('F-150');
$car->dump();
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### کلاس های final را ترجیح دهید

</div>

The `final` should be used whenever possible:

1. It prevents uncontrolled inheritance chain.
2. It encourages [composition](#prefer-composition-over-inheritance).
3. It encourages the [Single Responsibility Pattern](#single-responsibility-principle-srp).
4. It encourages developers to use your public methods instead of extending the class to get access on protected ones.
5. It allows you to change your code without any break of applications that use your class.

The only condition is that your class should implement an interface and no other public methods are defined.

For more informations you can read [the blog post](https://ocramius.github.io/blog/when-to-declare-classes-final/) on this topic written by [Marco Pivetta (Ocramius)](https://ocramius.github.io/).

<div dir="rtl">

**بد:**

</div>

```php
declare(strict_types=1);

final class Car
{
    private $color;

    public function __construct($color)
    {
        $this->color = $color;
    }

    /**
     * @return string The color of the vehicle
     */
    public function getColor()
    {
        return $this->color;
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

interface Vehicle
{
    /**
     * @return string The color of the vehicle
     */
    public function getColor();
}

final class Car implements Vehicle
{
    private $color;

    public function __construct($color)
    {
        $this->color = $color;
    }

    public function getColor()
    {
        return $this->color;
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## SOLID

**SOLID** مخففی است که توسط مایکل پرز برای پنج اصل اول به نام رابرت مارتین معرفی شده است، که به معنی پنج اصل اساسی برنامه نویسی و طراحی شی گرا است.

</div>

 * [S: Single Responsibility Principle (SRP)](#single-responsibility-principle-srp)
 * [O: Open/Closed Principle (OCP)](#openclosed-principle-ocp)
 * [L: Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)
 * [I: Interface Segregation Principle (ISP)](#interface-segregation-principle-isp)
 * [D: Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)

<div dir="rtl">

### Single Responsibility Principle یا SRP

همانطور که در Clean Code بیان شد ، "هرگز نباید بیش از یک کار برای یک کلاس وجود داشته باشد". بسته بندی کلاس هایی که قابلیت های زیادی دارند ، وسوسه انگیز است ، مانند زمانی که فقط اجازه دارید یک چمدان را در پرواز با خود حمل کنید. مسئله این است که کلاس شما از نظر مفهومی منسجم نخواهد بود و دلایل زیادی برای تغییر در آن ایجاد می کند. به حداقل رساندن تعداد دفعات لازم برای تغییر کلاس مهم است. این مهم است زیرا اگر کارایی بیش از حد در یک کلاس وجود دارد و شما بخشی از آن را اصلاح می کنید ، درک اینکه چگونه این امر بر سایر ماژول های وابسته در کد شما تأثیر می گذارد دشوار است.

**بد:**

</div>

```php
declare(strict_types=1);

class UserSettings
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function changeSettings(array $settings): void
    {
        if ($this->verifyCredentials()) {
            // ...
        }
    }

    private function verifyCredentials(): bool
    {
        // ...
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

class UserAuth
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function verifyCredentials(): bool
    {
        // ...
    }
}

class UserSettings
{
    private $user;

    private $auth;

    public function __construct(User $user)
    {
        $this->user = $user;
        $this->auth = new UserAuth($user);
    }

    public function changeSettings(array $settings): void
    {
        if ($this->auth->verifyCredentials()) {
            // ...
        }
    }
}
```
<div dir="rtl">


**[⬆ برگشت به بالا](#table-of-contents)**

### Open/Closed Principle یا OCP

همانطور که برتراند مایر اظهار داشت ، "نهادهای نرم افزاری (کلاسها ، ماژولها ، توابع و ...) باید برای پسوند باز باشند، اما برای اصلاح بسته هستند."

این به چه معناست؟ این اصل در اصل بیان می کند که شما باید به افرادی که کد شما را می خوانند اجازه دهید ویژگی های جدید را بدون تغییر کد موجود اضافه کنند.

**بد:**

</div>

```php
declare(strict_types=1);

abstract class Adapter
{
    protected $name;

    public function getName(): string
    {
        return $this->name;
    }
}

class AjaxAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'ajaxAdapter';
    }
}

class NodeAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'nodeAdapter';
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        $adapterName = $this->adapter->getName();

        if ($adapterName === 'ajaxAdapter') {
            return $this->makeAjaxCall($url);
        } elseif ($adapterName === 'httpNodeAdapter') {
            return $this->makeHttpCall($url);
        }
    }

    private function makeAjaxCall(string $url): Promise
    {
        // request and return promise
    }

    private function makeHttpCall(string $url): Promise
    {
        // request and return promise
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

interface Adapter
{
    public function request(string $url): Promise;
}

class AjaxAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class NodeAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        return $this->adapter->request($url);
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### Liskov Substitution Principle یا LSP

**بد:**

</div>

```php
declare(strict_types=1);

class Rectangle
{
    protected $width = 0;

    protected $height = 0;

    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Rectangle
{
    public function setWidth(int $width): void
    {
        $this->width = $this->height = $width;
    }

    public function setHeight(int $height): void
    {
        $this->width = $this->height = $height;
    }
}

function printArea(Rectangle $rectangle): void
{
    $rectangle->setWidth(4);
    $rectangle->setHeight(5);

    // BAD: Will return 25 for Square. Should be 20.
    echo sprintf('%s has area %d.', get_class($rectangle), $rectangle->getArea()) . PHP_EOL;
}

$rectangles = [new Rectangle(), new Square()];

foreach ($rectangles as $rectangle) {
    printArea($rectangle);
}
```

<div dir="rtl">

**خوب:**

</div>

```php
interface Shape
{
    public function getArea(): int;
}

class Rectangle implements Shape
{
    private $width = 0;
    private $height = 0;

    public function __construct(int $width, int $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square implements Shape
{
    private $length = 0;

    public function __construct(int $length)
    {
        $this->length = $length;
    }

    public function getArea(): int
    {
        return $this->length ** 2;
    }
}

function printArea(Shape $shape): void
{
    echo sprintf('%s has area %d.', get_class($shape), $shape->getArea()).PHP_EOL;
}

$shapes = [new Rectangle(4, 5), new Square(5)];

foreach ($shapes as $shape) {
    printArea($shape);
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### Interface Segregation Principle یا ISP

**بد:**

</div>

```php
declare(strict_types=1);

interface Employee
{
    public function work(): void;

    public function eat(): void;
}

class HumanEmployee implements Employee
{
    public function work(): void
    {
        // ....working
    }

    public function eat(): void
    {
        // ...... eating in lunch break
    }
}

class RobotEmployee implements Employee
{
    public function work(): void
    {
        //.... working much more
    }

    public function eat(): void
    {
        //.... robot can't eat, but it must implement this method
    }
}
```

<div dir="rtl">

**خوب:**

هر کارگری یک کارمند نیست ، بلکه هر کارمندی یک کارگر است.

</div>

```php
declare(strict_types=1);

interface Workable
{
    public function work(): void;
}

interface Feedable
{
    public function eat(): void;
}

interface Employee extends Feedable, Workable
{
}

class HumanEmployee implements Employee
{
    public function work(): void
    {
        // ....working
    }

    public function eat(): void
    {
        //.... eating in lunch break
    }
}

// robot can only work
class RobotEmployee implements Workable
{
    public function work(): void
    {
        // ....working
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

### Dependency Inversion Principle یا DIP

این اصل دو چیز اساسی را بیان می کند:
1. ماژول های سطح بالا نباید به ماژول های سطح پایین وابسته باشند. هر دو باید به Abstractionها بستگی داشته باشند.
2. Abstractionها نباید به جزئیات بستگی داشته باشد. جزئیات باید به Abstractionها بستگی داشته باشد.

**بد:**

</div>

```php
declare(strict_types=1);

class Employee
{
    public function work(): void
    {
        // ....working
    }
}

class Robot extends Employee
{
    public function work(): void
    {
        //.... working much more
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

interface Employee
{
    public function work(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // ....working
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... working much more
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## اصل DRY

کلمه ی DRY مخفف Don’t repeat yourself است. سعی کنید اصل DRY را رعایت کنید.

تمام تلاش خود را انجام دهید تا از نوشتن کدهای تکراری جلوگیری کنید. کد تکراری بد است زیرا به این معنی است که در صورت نیاز به تغییر منطق در ساختار برنامه، بیش از یک مکان برای تغییر دادن چیزی وجود دارد که باید آن را انجام دهید.

تصور کنید اگر یک رستوران اداره می کنید و موجودی خود را پیگیری می کنید: تمام گوجه فرنگی ، پیاز ، سیر ، ادویه جات و ... را در لیست نوشته باشید. اگر چندین لیست دارید که این مورد را در آن نگه می دارید ، پس هنگام تهیه یک غذا با گوجه فرنگی همه ی لیست ها باید به روز شوند ولی اگر فقط یک لیست موجودی دارید ، فقط یک جا برای به روزرسانی وجود دارد که باید آنرا تغییر دهید!!

اگر غالباً کد تکراری دارید علت آن این است که دو یا چند چیز کمی متفاوت دارید که اشتراکات زیادی با هم دارند، اما تفاوت آن ها شما را مجبور می کند که دو یا چند عملکرد جداگانه برای آن ها داشته باشید که بیشتر کارهای مشابه را انجام می دهند. حذف کد تکراری به معنای ایجاد انتزاعی است که می تواند مجموعه ای از موارد مختلف را فقط با یک تابع / ماژول / کلاس مدیریت کند.

درست گرفتن انتزاع بسیار مهم است، به همین دلیل باید از اصول SOLID مندرج در بخش Class ها پیروی کنید. انتزاعات بد ممکن است از کد تکراری بدتر باشد، بنابراین مراقب باشید! با گفتن این، اگر می توانید انتزاع خوبی انجام دهید، آن را انجام دهید! کدتان را مجددا تکرار نکنید، در غیر این صورت هر زمان که بخواهید یک چیز را تغییر دهید باید بخش های مختلفی را ویرایش کنید.

**بد:**

</div>

```php
declare(strict_types=1);

function showDeveloperList(array $developers): void
{
    foreach ($developers as $developer) {
        $expectedSalary = $developer->calculateExpectedSalary();
        $experience = $developer->getExperience();
        $githubLink = $developer->getGithubLink();
        $data = [$expectedSalary, $experience, $githubLink];

        render($data);
    }
}

function showManagerList(array $managers): void
{
    foreach ($managers as $manager) {
        $expectedSalary = $manager->calculateExpectedSalary();
        $experience = $manager->getExperience();
        $githubLink = $manager->getGithubLink();
        $data = [$expectedSalary, $experience, $githubLink];

        render($data);
    }
}
```

<div dir="rtl">

**خوب:**

</div>

```php
declare(strict_types=1);

function showList(array $employees): void
{
    foreach ($employees as $employee) {
        $expectedSalary = $employee->calculateExpectedSalary();
        $experience = $employee->getExperience();
        $githubLink = $employee->getGithubLink();
        $data = [$expectedSalary, $experience, $githubLink];

        render($data);
    }
}
```

<div dir="rtl">

**عالی:**

بهتر است از نسخه فشرده کد استفاده کنید.

</div>

```php
declare(strict_types=1);

function showList(array $employees): void
{
    foreach ($employees as $employee) {
        render([$employee->calculateExpectedSalary(), $employee->getExperience(), $employee->getGithubLink()]);
    }
}
```

<div dir="rtl">

**[⬆ برگشت به بالا](#table-of-contents)**

## ترجمه ها

ترجمه های این داکیومنت به زبان های دیگر:

* :cn: **چینی:**
   * [php-cpm/clean-code-php](https://github.com/php-cpm/clean-code-php)
* :ru: **روسی:**
   * [peter-gribanov/clean-code-php](https://github.com/peter-gribanov/clean-code-php)
* :es: **اسپانیایی:**
   * [fikoborquez/clean-code-php](https://github.com/fikoborquez/clean-code-php)
* :brazil: **پرتغالی:**
   * [fabioars/clean-code-php](https://github.com/fabioars/clean-code-php)
   * [jeanjar/clean-code-php](https://github.com/jeanjar/clean-code-php/tree/pt-br)
* :thailand: **تایلندی:**
   * [panuwizzle/clean-code-php](https://github.com/panuwizzle/clean-code-php)
* :fr: **فرانسوی:**
   * [errorname/clean-code-php](https://github.com/errorname/clean-code-php)
* :vietnam: **ویتنامی**
   * [viethuongdev/clean-code-php](https://github.com/viethuongdev/clean-code-php)
* :kr: **کره ای:**
   * [yujineeee/clean-code-php](https://github.com/yujineeee/clean-code-php)
* :tr: **ترکی:**
   * [anilozmen/clean-code-php](https://github.com/anilozmen/clean-code-php)
* :fa: **فارسی:**
   * [amirshnll/clean-code-php](https://github.com/amirshnll/clean-code-php)

**[⬆ برگشت به بالا](#table-of-contents)**

</div>
