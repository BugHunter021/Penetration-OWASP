# شناسایی زیرساخت و اینترفیس های مدیریتی

در این بخش از دوره آموزشی OWASP-WSTG به دومین بخش از استاندارد WSTG با شناسه WSTG-CONFIG-005 می پردازیم که مربوط به شناسایی زیرساخت و اینترفیس های مدیریتی می باشد.
### خلاصه

مدیر اینترفیس‌ها ممکن است در برنامه یا در سرور برنامه حضور داشته باشند تا به کاربران خاص اجازه دهند تا فعالیت‌های ممتاز در سایت را انجام دهند. اینترفیس‌های مدیریت ممکن است در برنامه یا در سرور وجود داشته باشند تا به کاربران خاص اجازه دهند تا فعالیت‌های مدیریتی را در سایت انجام دهند. این تست صورت می‌گیرد تا مشخص شود که آیا امکان دسترسی به عملکردهای مدیریتی برای کاربران معمولی و غیر مجاز وجود دارد یا خیر.

یک برنامه ممکن است به یک اینترفیس مدیریتی نیاز داشته باشد تا یک کاربر ممتاز (سطح دسترسی بالا) را قادر به دسترسی به عملکردی نماید که ممکن است تغییراتی را در نحوه عملکرد سایت ایجاد کند. چنین تغییراتی ممکن است شامل موارد زیر باشد:

* تنظیمات حساب‌های کاربری
* طراحی سایت و لایه بندی آن
* تغییر در داده‌ها
* تغییر در پیکربندی‌های سایت

در بسیاری از موارد، چنین اینترفیس‌هایی، دارای محافظت لازم در برابر دسترسی‌های مجاز نمی‌باشند. هدف از این تست، شناسایی این اینترفیس‌های مدیریتی و دسترسی به قابلیت‌های در نظر گرفته شده برای کاربران ممتاز است.
# آزمایش اهداف

اینترفیس‌ها و عملکرد مدیریتی مخفی را شناسایی کنید.
### چگونه امتحان کنیم.
#### آزمایش جعبه سیاه

بخش زیر بردارهایی را توصیف می‌کند که ممکن است برای آزمایش حضور اینترفیس‌های مدیریتی مورد استفاده قرار گیرند. این تکنیک‌ها همچنین ممکن است برای بررسی مواردی مانند بالا بردن سطح دسترسی، تست مربوط به Insecure Direct Object Reference یا عبور از مکانیزم‌های Authorization نیز مورد استفاده قرار گیرند.

* شناسایی (Enumeration) دایرکتوری‌ها و فایل‌ها. یک اینترفیس مدیریتی ممکن است وجود داشته باشد اما به طور آشکار در دسترس تست نفوذگر نباشد. تلاش برای حدس زدن مسیر اینترفیس مدیریتی ممکن است به سادگی درخواست برای صفحاتی مانند /admin یا /administrator و غیر باشد و البته سناریویی دیگر برای شناسایی این صفحات استفاده از تکنیک‌های جست و جو در گوگل (Google Hacking) می‌باشد.

* ابزارهای زیادی برای اجرای Brute Force محتویات سرور وجود دارد، نام برخی از این ابزارهای برای کسب اطلاعات بیشتر در قسمت ابزارهای در همین بخش قرار داده شده است. یک تستر همچنین باید نام فایل مربوط به صفحه مدیریت را شناسایی کند. Force Browsing برای صفحه شناسایی‌شده ممکن است دسترسی به اینترفیس را فراهم کند.

* توضیحات و لینک‌ها در سورس کد بسیاری از سایت‌ها از کد مشترکی استفاده می‌کنند که برای تمام کاربران سایت بارگذاری شده ‌است. با بررسی تمام منابع ارسالی به مشتری، ممکن است لینک‌ها به عملکرد مدیریتی کشف شوند پس آن‌ها باید بررسی شوند.

 بازبینی سرور و مستندات برنامه. اگر سرور یا برنامه کاربردی به صورت پیش‌فرض پیکربندی شده باشد، ممکن است دسترسی به اینترفیس مدیریتی با استفاده از اطلاعات پیکربندی یا به وسیله اسناد و مدارک راهنما امکان پذیر باشد. علاوه بر این در صورتی که اینترفیس مدیریتی شناسایی شد، می‌بایست لیستی از کلمات عبور پیش‌فرض نیز بررسی شوند.

* اطلاعات عمومی در دسترس. بسیاری از برنامه‌های کاربردی مانند وردپرس یا جوملا که در واقع سیستم‌های مدیریت محتوا (CMS) هستند، دارای اینترفیس‌های مدیریتی پیش‌فرض می‌باشند.

* پورت‌های موجود در سرور. اینترفیس‌های مدیریتی ممکن است بر روی یک پورت متفاوت در سرور میزبان قرار گرفته باشند. به عنوان مثال، اینترفیس مدیریت Apache tomcat اغلب در پورت ۸۰۸۰ دیده می‌شود.

* دستکاری در پارامتر. برای فعال کردن عملکرد مدیریتی ممکن است به یک پارامتر GET یا POST یا یک متغیر در کوکی نیاز باشد. مواردی که به این موضوع مربوط می‌شوند شامل فیلدهای مخفی مانند
```js
<input type="hidden" name="admin" value="no">
```
یا در یک کوکی مانند زیر می‌باشند.

 ```html
 Cookie: session_cookie; useradmin=0
```
هنگامی که یک اینترفیس مدیریتی کشف شد، ترکیبی از تکنیک‌های بالا ممکن است برای تلاش برای دور زدن احراز هویت، مورد استفاده قرار گیرد. اگر این تست با شکست روبرو شود، تست نفوذگر ممکن است تلاش برای یک حمله Brute Force را در دستور کار خود قرار دهد. در صورتی که تست نفوذگر قصد انجام حمله Brute Force را داشته باشد، باید از پتانسیل قفل حساب کاربری آگاهی لازم را داشته باشد.

### آزمایش جعبه خاکستری

بررسی دقیق‌تر سرور و اجزای برنامه می بایست برای اطمینان از Hardening صورت گرفته، انجام شود (صفحات مدیریتی برای همه افراد از طریق استفاده از فیلترینگ IP یا کنترل های دیگر، قابل‌دسترس نباشند)‏ و در صورت امکان، تایید این که همه اجزا از اعتبار یا پیکربندی پیش‌فرض استفاده نمی‌کنند. همچنین سورس کد نیز باید مورد بررسی قرار گیرد تا اطمینان حاصل شود که مدل Authentication و Authorization ایجاد شده به صورت واضح تفکیک وظایف بین کاربران عادی و مدیران سایت را تضمین می‌کند. توابع اینترفیس کاربر به اشتراک گذاشته شده بین کاربران عادی و کاربران مدیر نیز می‌بایست برای اطمینان از جداسازی واضح بین ترسیم این اجزا و نشت اطلاعات از این عملکرد مشترک بررسی شود.

هر چارچوب وب ممکن است صفحات یا مسیر پیش‌فرض مدیریت خود را داشته باشد. به عنوان مثال:

WebSphere:
```text
/admin
/admin-authz.xml
/admin.conf
/admin.passwd
/admin/*
/admin/logon.jsp
/admin/secure/logon.jsp

```
PHP:
```text
/phpinfo /phpmyadmin/
/phpMyAdmin/
/mysqladmin/
/MySQLadmin
/MySQLAdmin
/login.php
/logon.php
/xmlrpc.php
/dbadmin

```
FrontPage:
```text
/admin.dll 
/admin.exe
/administrators.pwd
/author.dll
/author.exe
/author.log
/authors.pwd
/cgi-bin
```
WebLogic:
```text
/AdminCaptureRootCA 
/AdminClients
/AdminConnections
/AdminEvents
/Admin JDBC
/AdminLicense
/AdminMain
/AdminProps
/AdminRealm
/AdminThreads
```
WordPress:
```text
wp-admin/
wp-admin/about.php
wp-admin/admin-ajax.php
wp-admin/admin-db.php
wp-admin/admin-footer.php
wp-admin/admin-functions.php
wp-admin/admin-header.php
```
## ابزارها

ابزار OWASP ZAP – Forced Browse که از پروژه قبلی OWASP با نام DirBuster در حال حاضر استفاده می‌شود.

ابزار THC-HYDRA ابزاری که به منظور انجام Brute Force از آن استفاده می‌شود.

توجه داشته باشید که یک Brute Forcer زمانی که از یک دیکشنری خوب مانند دیکشنری Netsparker استفاده شود، نتایج بهتری را فراهم می‌نماید.

www.netsparker.com/blog/web-security/svn-digger-better-lists-for-forced-browsing/

## منابع:


https://cirt.net/passwords

https://github.com/fuzzdb-project/fuzzdb/blob/master/discovery/predictable-filepaths/login-file-locations/Logins.txt

https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/business-logic/CommonDebugParamNames.txt
