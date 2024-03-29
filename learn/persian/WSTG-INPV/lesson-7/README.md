# تست مربوط به تزریق XML

در این بخش از دوره آموزشی OWASP-WSTG به هفتمین بخش از استاندارد WSTG با شناسه WSTG-INPV-07 می پردازیم که مربوط به بررسی XML Injection می باشد.

# خلاصه

تست مربوط به تزریق XML زمانی انجام می شود که امکان ارسال داده به سمت سرور بوسیله XML انجام می شود و تست نفوذگر تلاش می‌کند تا یک سند XML را به برنامه تزریق کند. اگر تجزیه‌گر XML نتواند اعتبارسنجی داده را به درستی انجام دهد، در این صورت نتیجه مثبت حاصل شده و تزریق انجام خواهد شد.

این بخش نمونه‌های عملی تزریق XML را توصیف می‌کند. ابتدا، یک ارتباط مبتنی بر XML تعریف شده و اصول کاری آن توضیح داده خواهد شد. سپس، به روش کشف پرداخته می‌شود که در آن سعی می‌کنیم متاکارترهای XML را وارد کنیم. هنگامی که اولین مرحله انجام شد، تست نفوذگر اطلاعاتی در مورد ساختار XML خواهد داشت، بنابراین می‌توان داده‌ها و برچسب‌های XML را تزریق کرد (‏Tag Injection)‏.
اهداف تست

* شناسایی نقاط مستعد تزریق XML
* ارزیابی انواع اکسپلویت‌هایی که می‌توان به آن‌ها دست یافت و شدت آن‌ها

# چگونه تست را انجام دهیم

بیایید فرض کنیم که یک برنامه وب از یک ارتباط مبتنی برXML به منظور ثبت کاربر استفاده می کند. این کار با ایجاد و افزودن یک user>node در فایل xmlDb انجام می‌شود.

فرض کنید محتوای فایل xmlDB شبیه به تصویر زیر باشد:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<users>
<user>
<username>gandalf</username>
<password>!c3</password>
<userid>0</userid>
<mail>gandalf@middleearth.com</mail>
</user>
<user>
<username>Stefan0</username>
<password>w1s3c</password>
<userid>500</userid>
<mail>Stefano@whysec.hmm</mail>
</user>
</users>
```
هنگامی که یک کاربر خودش را با پر کردن یک فرم HTML ثبت می‌کند، برنامه داده‌های کاربر را در یک درخواست استاندارد دریافت می‌کند، که به خاطر سادگی، فرض می‌کنیم که درخواست مذکور به صورت GET ارسال می‌شود.
برای مثال، مقادیر زیر را در نظر بگیرید:
```text
Username: tony
Password: Un6R34kb!e
E-mail: s4tan@hell.com
```
درخواست ایجاد شده به شکل زیر خواهد بود:
```txt
http://www.example.com/addUser.php?username=tony&password=Un6R34kb!e&email=s4tan@hell.com
```

سپس برنامه Node زیر را می‌سازد:
```xml
<user>
<username>tony</username>
<password>Un6R34kb!e</password>
<userid>500</userid>
<mail>s4tan@hell.com</mail>
</user>
```

که به xmlDB اضافه خواهد شد:
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<users>
<user>
<username>gandalf</username>
<password>!c3</password>
<userid>0</userid>
<mail>gandalf@middleearth.com</mail>
</user>
<user>
<username>Stefan0</username>
<password>w1s3c</password>
<userid>500</userid>
<mail>Stefano@whysec.hmm</mail>
</user>
<user>
<username>tony</username>
<password>Un6R34kb!e</password>
<userid>500</userid>
<mail>s4tan@hell.com</mail>
</user>
</users>
```

### مرحله Discovery

اولین مرحله به منظور آزمایش یک برنامه جهت شناسایی آسیب‌پذیری تزریقXML، تلاش برای وارد کردن متاکارترهای XML است.

متاکارترهایXML عبارتند از:

#### عبارت Single Quote:

هنگامی که Sanitization به درستی انجام نمی‌شود، اگر مقدار تزریق شده بخشی از یک مقدار ویژگی در یک تگ باشد، این کاراکتر می‌تواند در طول تجزیه XML یک استثنا ایجاد کند.

به عنوان مثال، فرض کنید که ویژگی زیر وجود دارد:
```xml
<node attrib='$inputValue' />
```
بنابراین، اگر از تک کوتیشن مانند زیر استفاده کنیم:
```xml
inputValue = foo'
```
این عبارت به عنوان مقدار attrib درج شده و در انتها مقدار زیر را خواهیم داشت:
```xml
<node attrib='foo' ' />
```
بدین شکل، سند XML نتیجه درستی نداشته و فرم اصلی خود را نخواهد داشت.

#### عبارت Double Quote:

این کاراکتر همان معنی را دارد که یک تک کوتیشن دارد و اگر مقدار صفت در دابل کوتیشن محصور باشد می‌تواند مورد استفاده قرار گیرد. به عنوان مثال، فرض کنید که ویژگی زیر وجود دارد:
```xml
<node attrib="$inputValue"/>
```


بنابراین، اگر از دابل کوتیشن مانند زیر استفاده کنیم:
```xml
$inputValue = foo'
```

نتیجه به صورت زیر خواهد بود:
```xml
<node attrib="foo""/>
```
در این صورت سند XML نتیجه نامعتبری را در برخواهد داشت.

#### علامت‌های Angular Parentheses:  < >

در صورت اضافه نمودن این علامت‌ها به ورودی کاربر مانند موارد زیر:
```xml
Username = foo<
```

برنامه یک Node جدید خواهد ساخت:
```xml
<user>
<username>foo<</username>
<password>Un6R34kb!e</password>
<userid>500</userid>
<mail>s4tan@hell.com</mail>
</user>
```

اما به دلیل وجود یک علامت > ، سند XML نتیجه نامعتبری خواهد داشت.

Comment Tag:

این کاراکترها به عنوان آغاز / پایان یک کامنت یا توضیحات تفسیر می‌شوند. بنابراین با تزریق یکی از آن‌ها در پارامتر نام کاربر به شکل زیر:
```xml
Username = foo<!--
```
برنامه یک Node مانند موارد زیر خواهد ساخت:
```xml
<user>
<username>foo<!--</username>
<password>Un6R34kb!e</password>
<userid>500</userid>
<mail>s4tan@hell.com</mail>
</user>
```

این توالی نیز منجر به نتیجه معتبر در XML نخواهد شد.

Ampersand: &

Ampersand در زبان XML برای نشان دادن Entity ها استفاده می‌شود و نمونه‌ای از آن به شکل &symbol; است. یک Entity به یک کاراکتر در مجموعه کاراکترهای Unicode نگاشت یا Map می‌شود.

به عنوان مثال:
```xml
<tagnode>&lt;</tagnode>
```

به خوبی شکل‌گرفته و معتبر است و نشاندهنده < می‌باشد.

اگر & با & کدگذاری نشده باشد، می‌توان از آن برای بررسی XML Injection استفاده نمود.

در واقع، اگر یک ورودی مانند شکل زیر ارائه شود:
```xml
Username = &foo
```

که Node جدیدی ایجاد خواهد شد:

```xml
<user>
<username>&foo</username>
<password>Un6R34kb!e</password>
<userid>500</userid>
<mail>s4tan@hell.com</mail>
</user>
```

اما در عین حال، سند XML نتیجه نامعتبری خواهد داشت. $foo با سمیکالن خاتمه نیافته است.

قسمتCDATA :

کاراکترهای محصور در یک بخش CDATA توسط یک تجزیه کننده XML اصطلاحا Parse نمی‌شوند. اگر نیاز باشد که یک رشته را در داخل یک Node متنی نشان دهیم، یک بخش CDATA می‌تواند مورد استفاده قرار گیرد:
```xml
<node>
<![CDATA[ <foo>]]>
</node>
```

به طوری که به صورت markup در نظر گرفته نخواهد شد و به عنوان داده‌های کاراکتری در نظر گرفته می‌شود.

اگر یک‌ Node به روش زیر ایجاد شود:

```xml
<username><![CDATA[ <$userName]]></username>
```

تست نفوذگر می‌تواند مقدار مربوط به پایان رشته CDATA یعنی ]]> را تزریق کند.

```xml
userName = ]]>
```
این کار در خروجی به صورت زیر خواهد بود:
```xml
<username><![CDATA[]]>]]></username>
```

که یک قطعه XML معتبر نیست.

آزمون دیگر مربوط به تگ CDATA است. فرض کنید که سند XML برای ایجاد یک صفحه HTML پردازش شده‌است. در این مورد، بخش CDATA ممکن است به سادگی حذف شود، بدون اینکه محتویات آن‌ها را بررسی گردد. سپس می‌توان تگ‌های HTML را که در صفحه تولید شده قرار می‌گیرد، تزریق کرد و روال‌های Sanitization موجود را کاملا دور زد.

بیایید یک مثال واقعی را در نظر بگیریم. فرض کنید یک Node داریم که حاوی برخی متن است که به کاربر نمایش داده خواهد شد.
```html
<html>
     $HTMLCode
</html>
```
سپس، یک مهاجم می‌تواند ورودی زیر را فراهم کند:
```js
$HTML Code = <![CDATA[<]]>script<![CDATA[>]]>alert('xss')<![CDATA[<]]>/script<![CDATA[>]]>
```

و Node زیر را ایجاد کند:
```html
<html>
    <![CDATA[<]]>script<![CDATA[>]]>alert('xss')<![CDATA[<]]>/script<![CDATA[>]]>
</html>

```
در طول پردازش، جداکننده‌های بخش CDATA حذف می‌شوند و کد HTML زیر را تولید می‌کنند:
```js
<script>
    alert('XSS')
</script>
```
نتیجه این است که برنامه در برابر XSS آسیب‌پذیر می‌باشد.

