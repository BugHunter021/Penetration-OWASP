<div dir="rtl">


# بررسی Session Fixation

در این بخش از دوره آموزشی OWASP-WSTG به ششمین بخش از استاندارد WSTG با شناسه WSTG-SESS-03 می پردازیم که مربوط به بررسی Session Fixation می باشد.

# خلاصه

تثبیت نشست یا Session Fixation با فرآیند نا امن حفظ مقدار یکسانی از کوکی‌های نشست قبل و بعد از احراز هویت ایجاد می‌شود. این معمولا زمانی اتفاق می‌افتد که از کوکی‌های نشست برای ذخیره اطلاعات وضعیت حتی قبل از ورود به سیستم استفاده می‌شود، به عنوان مثال، اضافه کردن آیتم‌ها به یک کارت خرید قبل از احراز هویت برای پرداخت.

در اکسپلویت عمومی آسیب‌های تثبیت نشست، یک مهاجم می‌تواند مجموعه‌ای از کوکی‌های نشست را از وب سایت هدف بدون احراز هویت اولیه به دست آورد. سپس مهاجم می‌تواند این کوکی‌ها را با استفاده از تکنیک‌های مختلف به مرورگر قربانی وارد کند. اگر قربانی سپس در وب سایت هدف احراز هویت کند و کوکی‌ها به محض ورود refresh نشوند، قربانی توسط کوکی‌های نشست انتخاب‌شده توسط مهاجم شناسایی خواهد شد. سپس مهاجم قادر به جعل هویت قربانی با این کوکی‌های شناخته‌شده است.

این مساله را می‌توان با refresh نمودن کوکی‌های نشست پس از فرآیند احراز هویت برطرف نمود. همچنین، با اطمینان از یکپارچگی (integrity) کوکی‌های نشست می‌توان از این حمله جلوگیری کرد. هنگامی که گرفتن حمله کنندگان شبکه را در نظر دارید، به عنوان مثال، مهاجمانی که شبکه مورد استفاده توسط قربانی را کنترل می‌کنند، از HSTS کامل استفاده کرده و یا پیشوند __Host- / __Secure- را به نام کوکی اضافه می‌کنند.

پذیرش کامل HSTS زمانی رخ می‌دهد که میزبان،HSTS را برای خود و تمام حوزه‌های فرعی آن فعال کند. این موضوع در مقاله‌ای به نام Testing for Integrity Flaws in Web Sessions توسط Stefano Calzavara، Alvise Rabitti، Alessio Ragazzo و Michele Bugliesi توضیح داده شده‌است.

### اهداف تست

* تجزیه و تحلیل مکانیسم احراز هویت و جریان آن

* اجرای Force کردن کوکی‌ها و ارزیابی تاثیر آن‌ها.

### چگونه تست را انجام دهیم

در این بخش به توضیح استراتژی آزمون می‌پردازیم که در بخش بعدی نشان داده خواهد شد.

گام اول ارسال درخواست به سایت برای آزمایش است (به عنوان مثال www.example.com) اگر تست نفوذگر موارد زیر را درخواست کند:

```js
GET / HTTP/1.1
Host: www.example.com
```

آنگاه پاسخ زیر را دریافت خواهد کرد:
```js
HTTP/1.1 200 OK
Date: Wed, 14 Aug 2008 08:45:11 GMT
Server: IBM_HTTP_Server
Set-Cookie: JSESSIONID=0000d8eyYq3L0z2fgq10m4v-rt4: -1; Path=/; secure Cache-Control: no-cache="set-cookie, set-cookie2"
Expires: Thu, 01 Dec 1994 16:00:00 GMT
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=Cp1254
Content-Language: en-US
```

برنامه یک شناسه جلسه جدید، با مقدار زیر برای کاربر تنظیم می‌کند:

SESSIONID=0000d8eyYq3L0z2fgq10m4v-rt4:-1

سپس، اگر تست نفوذگر با موفقیت به برنامه با متد POST زیر احراز هویت کند.

www.example.com/authentication.php:

```js
POST /authentication.php HTTP/1.1
Host: www.example.com
[...]
Referer: http://www.example.com
Cookie: JSESSIONID=0000d8eyYq3L0z2fgq10m4v-rt4:-1 Content-Type: application/x-www-form-urlencoded Content-length: 57
Name=Meucci&wpPassword-secret! &wpLoginattempt=Log+in
```

تست نفوذگر پاسخ زیر را از سرور مشاهده می‌کند:

```js
HTTP/1.1 200 OK
Date: Thu, 14 Aug 2008 14:52:58 GMT Server: Apache/2.2.2 (Fedora)
X-Powered-By: PHP/5.1.6
Content-language: en
Cache-Control: private, must-revalidate, max-age=0
X-Content-Encoding: gzip
Content-length: 4090
Connection: close
Content-Type: text/html; charset=UTF-8
...
HTML data
```

از آنجا که هیچ کوکی جدیدی برای احراز هویت موفق صادر نشده است، تست نفوذگر می‌داند که انجام Session Hijacking ممکن است مگر اینکه یکپارچگی (integrity) کوکی نشست تضمین شده‌باشد.

تست نفوذگر می‌تواند یک شناسه نشست معتبر را به یک کاربر ارسال کند (‏احتمالا با استفاده از یک ترفند مهندسی اجتماعی)‏، سپس منتظر بماند تا آن‌ها احراز هویت کنند و متعاقبا تایید کند که امتیازات (privileges) به این کوکی اختصاص داده شده‌است.

### بررسی Test with Forced Cookies

این استراتژی تست مهاجمان شبکه را هدف قرار می‌دهد، از این رو فقط باید برای سایت‌هایی اعمال شود که HSTS کامل ندارند (سایت‌هایی با پذیرش کامل HSTS امن هستند، زیرا همه کوکی‌های آن‌ها یکپارچگی دارند). ما فرض می‌کنیم که دو حساب تستی در وب سایت تحت آزمایش داریم، یکی به عنوان قربانی و دیگری به عنوان مهاجم عمل می‌کند. ما سناریویی را شبیه‌سازی می‌کنیم که در آن مهاجم در مرورگر قربانی تمام کوکی‌های که پس از ورود به سیستم تازه صادر نشده اند و یکپارچگی ندارند را Force می‌کند. پس از ورود قربانی، مهاجم کوکی‌های Force شده را به وب سایت برای دسترسی به حساب قربانی ارائه می‌دهد: اگر آن‌ها برای عمل کردن از طرف قربانی کافی باشند، Session Fixation امکان پذیر است.

#### در اینجا به مراحل اجرای این آزمون اشاره می‌کنیم:

* صفحه ورود به سیستم وب سایت را شناسایی نمایید.
* کوکی‌ها را قبل از ورود به سیستم در جایی ذخیره کنید، به استثنای کوکی‌های که حاوی پیشوند __Host- و __Secure- در نام خود هستند.
* به عنوان قربانی به وب سایت وارد شده و هر صفحه‌ای با یک عملکرد که نیاز به احراز هویت دارد را شناسایی نمایید.
* مجموعه کوکی‌هایی که در مرحله دو آن‌ها را ذخیره نموده بودید، تنظیم کنید.
* عملکرد شناسایی شده در مرحله ۳ را فراخوانی کنید.
* مشاهده کنید که آیا عملیات در مرحله ۵ با موفقیت انجام شده‌است. اگر چنین باشد، حمله موفقیت‌آمیز بوده است.
* پاک کردن مجموعه کوکی‌ها و ورود به عنوان مهاجم و رسیدن به صفحه شناسایی شده در مرحله ۳.
* کوکی‌های ذخیره‌شده در مرحله ۲ را یکی یکی در بخش کوکی بنویسید.
* مجدد عملکرد شناسایی‌شده در مرحله ۳ را فراخوانی کنید.
* بخش کوکی‌ها را پاک کنید و دوباره به عنوان قربانی وارد سیستم شوید.
* مشاهده کنید که آیا عملیات در مرحله ۹ با موفقیت در حساب قربانی انجام شده‌است یا خیر. اگر چنین بود، حمله موفقیت‌آمیز بوده است؛ در غیر این صورت سایت، در برابر Session Fixation ایمن است.

ما استفاده از دو ماشین یا مرورگر مختلف را برای قربانی و مهاجم توصیه می‌کنیم. در صورتی که برنامه تحت وب، برای تایید دسترسی فعال از یک کوکی خاص فرآیند انگشت نگاری (Fingerprinting) را انجام می‌دهد، این روش، امکان بروز تعداد تشخیص‌های مثبت کاذب را کاهش می‌دهد. یک نوع کوتاه‌تر اما با دقت کم‌تر از استراتژی تست، تنها به یک حساب تست نیاز دارد. همان مراحل را دنبال می‌کند، اما در مرحله ۶ متوقف می‌شود.

### اصلاح Remediation

پس از اینکه کاربر با موفقیت احراز هویت کرد، یک نوسازی نشانه نشست را اجرا کنید.

برنامه باید همیشه قبل از احراز هویت یک کاربر، ابتدا شناسه جلسه موجود را باطل نموده و اگر احراز هویت موفقیت آمیز باشد، شناسه جلسه دیگری را ارائه کند
