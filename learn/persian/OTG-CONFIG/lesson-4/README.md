# بررسی فایل های پشتیبان قدیمی و فایل های Unreferenced

در این بخش از دوره آموزشی OWASP-WSTG به دومین بخش از استاندارد WSTG با شناسه WSTG-CONFIG-004 می پردازیم که مربوط به ارزیابی مدیریت پسوند فایل‌ها برای اطلاعات حساس می باشد.
### خلاصه

در حالی که بسیاری از فایل‌های درون یک سرور وب به طور مستقیم توسط خود سرور اداره می‌شوند، پیدا کردن فایل‌های بدون استفاده یا فراموش‌شده که می‌توانند برای به دست آوردن اطلاعات مهم در مورد زیرساخت یا Credential مورد استفاده قرار گیرند، یکی از مواردی است که باید به آن توجه شود.

اکثر سناریوهای رایج در رابطه با موضوع مذکور عبارتند از:

وجود نسخه‌های قدیمی تغییر نام یافته از فایل‌های اصلاح‌شده (ویرایش فایل در وب سرور)
فایل‌هایی که بارگذاری می‌شوند و می‌توانند به عنوان سورس دانلود شوند.

فایل‌های پشتیبانی که به صورت خودکار یا دستی به شکل آرشیوهای فشرده (با پسوندهای zip یا rar و…) در وب سرور قرار دارند.

فایل‌های پشتیبان همچنین می‌توانند به طور خودکار توسط سیستم فایل سرور که برنامه بر روی آن میزبانی می‌شود، ایجاد شوند، یک ویژگی که معمولا به عنوان “Snapshote” نامیده می‌شود.

همه این فایل‌ها ممکن است منجر به دسترسی تست نفوذگر به بخش‌های داخلی، درهای پشتی، واسط‌های اجرایی مدیریتی، یا حتی Credential‌های مختلف برای اتصال به سیستم مدیریتی برنامه یا سرور پایگاه‌داده را شوند.

یک منبع مهم آسیب‌پذیری در فایل‌هایی قرار دارد که هیچ ارتباطی با برنامه ندارند، اما به عنوان نتیجه‌ای از ویرایش فایل‌های برنامه، یا پس از ایجاد نسخه‌های پشتیبان آنلاین، یا با ترک فایل‌های قدیمی یا فایل‌های بدون ارجاع (Unreferenced) ایجاد می‌شوند.

انجام ویرایش در محل همان محل وب سرور، ممکن است منجر به تولید فایل‌هایی به صورت نا خواسته گردد. این فایل‌ها اغلب یک پشتیبان از فایل اصلی هستند که در هنگام ویرایش در وب سرور ایجاد می‌شوند. این موضوع ممکن است یک تهدید امنیتی جدی برای برنامه باشد. این امر به این دلیل رخ می‌دهد که کپی‌های پشتیبان ممکن است با پسوند یا نامی متفاوت از فایل‌های اصلی تولید شوند.

ساخت یک کپی به صورت دستی نیز می‌تواند همان اثر را ایجاد نماید (‏به کپی کردن یک فایل در همان دایرکتوری فکر کنید). فایل‌های آرشیوی با پسوند gz ،tar و zip که توسط ما تولید می‌شوند (‏فراموش می‌کنیم تا آن‌ها را حذف نموده و یا به بخش دیگری منتقل نماییم) ‏به وضوح فرمت متفاوتی دارند که در صورتی که اقدام به حذف آن‌ها نکنیم، در دسترس عموم قرار خواهند داشت.

موضوع دیگر مربوط به نسخه‌های خودکار ایجاد شده توسط بسیاری از ویرایشگران می‌باشد. به عنوان مثال،emacs یک کپی پشتیبان به نام فایل به همراه ~ را در هنگام ویرایش فایل تولید می‌کند. (‏ساخت یک کپی به صورت دستی ممکن است همان اثر را ایجاد کند)

به کپی کردن file به نام file.old توجه کنید. در برخی موارد توسعه دهندگان وب ممکن است از این روش برای عدم نمایش فایل‌ها و یا ذخیره آن‌ها برای استفاده در آینده بهره ببرند. این موضوع هم به نوبه خود تهدیدی برای دسترسی افراد به این فایل‌ها محسوب می‌شود. همچنین سیستم فایل اصلی در برنامه کاربردی می‌تواند یک Snapshot از برنامه شما را در زمان‌های مختلف بدون اطلاع شما ایجاد کند، که ممکن است از طریق وب نیز در دسترس باشد و با توجه به ذخیره سازی آن، یک تهدید برای برنامه شما ایجاد نماید.

در نتیجه، این فعالیت‌ها، فایل‌هایی را تولید می‌کنند که مورد نیاز برنامه نبوده و ممکن است متفاوت از فایل اصلی، توسط سرور وب اداره شوند. به عنوان مثال، اگر ما یک کپی از login.asp را با نام login.asp.old ذخیره نماییم، این فایل معمولا به جای اجرا در وب سرور، به عنوان متن ساده ذخیره می‌شود. به عبارت دیگر، دسترسی به login.asp موجب اجرای کد سمت سرور شده ولی دسترسی به login.asp.old باعث نمایش محتوای آن (‏که همان کد سمت سرور است)‏به طور واضح به کاربر شده و در مرورگر، به وی نمایش داده خواهد شد. این موضوع ممکن است اطلاعات حساس برنامه وب را آشکار نموده و امنیت برنامه وب را به خطر بیاندازد.

به طور کلی، افشای کد سمت سرور موضوع جالبی نیست. شما نه تنها منطق کسب‌وکار را آشکار می‌کنید، بلکه ممکن است به صورت ناخواسته اطلاعات مربوط به برنامه را نیز آشکار نمایید که این امر به مهاجم در راستای رسیدن به اهدافش کمک می‌کند (‏نام مسیر، ساختار داده، و غیره)‏.

نکته: توجه داشته باشید که نام کاربری و کلمه عبور در تعداد زیادی از فایل‌های موجود بر روی سرور به صورت متن واضح وجود دارد(‏که یک عمل بسیار خطرناک و بی‌دقت است)‏ و در صورتی که این اطلاعات در فایل‌هایی مانند login.asp.old وجود داشته باشد، مهاجم پس از دسترسی به این فایل قادر به مشاهده آن‌ها خواهد بود.

موضوع دیگر فایل‌های مرجع نشده یا unreferenced file‌ها می‌باشند که به دلیل انتخاب نوعی از طراحی یا پیکربندی، اجازه می‌دهند انواع مختلفی از فایل‌های مربوط به برنامه مانند فایل‌های داده، فایل‌های پیکربندی و فایل‌های لاگ در دایرکتوری‌های سیستم فایل ذخیره شوند که توسط سرور وب قابل‌دسترسی هستند. این فایل‌ها معمولا هیچ دلیلی برای حضور در فضای سیستم فایل که می‌تواند از طریق وب قابل‌دسترسی باشد، ندارند، زیرا آن‌ها باید تنها در سطح برنامه، توسط خود برنامه (‏و نه توسط مرور اتفاقی کاربر)‏ در دسترس باشند.

### آیتم Threats

پرونده‌های قدیمی، فایل‌های پشتیبان و فایل‌های بدون ارجاع، تهدیدات مختلفی را برای امنیت یک برنامه وب ایجاد می‌کنند که برخی از آن عبارتند از:

* فایل‌های ارجاع نشده ممکن است اطلاعات حساسی را آشکار کنند که می‌توانند حمله متمرکز علیه برنامه را تسهیل کنند؛ برای مثال، شامل فایل‌های حاوی Credential‌های پایگاه‌داده، فایل‌های پیکربندی حاوی ارجاعات به محتوای پنهان دیگر، مسیرهای مستقیم فایل‌ها و غیره باشند.
* صفحات ارجاع نشده ممکن است شامل قابلیت‌های قدرتمندی باشند که می‌توانند برای حمله به برنامه مورد استفاده قرار گیرند. مثال دیگر می‌تواند یک صفحه مدیریتی باشد که به محتوای منتشر شده متصل نبوده اما می‌تواند توسط هر کاربری که می‌داند آن را از کجا پیدا کند، قابل‌دسترسی باشد.
* پرونده‌های قدیمی و پشتیبان ممکن است شامل آسیب‌پذیری‌هایی باشند که در نسخه‌های اخیر رفع شده‌اند؛ به عنوان مثال viewdoc.old.jsp ممکن است شامل یک آسیب‌پذیری پیمایش دایرکتوری باشد که درviewdoc.jsp رفع شده‌است اما هنوز هم می‌تواند توسط هر کسی که نسخه قدیمی را پیدا می‌کند مورد استفاده قرار گیرد.
* فایل‌های پشتیبان ممکن است کد منبع را برای صفحاتی که جهت اجرا بر روی سرور طراحی شده‌ اند آشکار کنند؛ به عنوان مثال درخواست از صفحه viewdoc.bak ممکن است کد مبدا را برای صفحه viewdoc.jsp برگرداند. این موضوع می‌تواند برای یافتن نقاط ضعفی که یافتن آن‌ها ممکن است با درخواست‌های کورکورانه به صفحه مذکور، مدت‌ها به طول بیانجامد، مناسب باشد. در حالی که این تهدید به طور واضح برای زبان‌های برنامه‌نویسی مانند Perl، PHP، ASP، shellscript،JSP و غیره اعمال می‌شود، ولی تنها به آن‌ها محدود نمی‌شود.
* آرشیوهای پشتیبان ممکن است حاوی کپی‌هایی از تمام فایل‌ها داخل یا حتی خارج از محل Webroot باشند. این موضوع به نفوذگر کمک می‌کند تا به سرعت کل برنامه، از جمله صفحات بدون ارجاع، کد منبع، فایل‌ها و غیره را شناسایی نماید. به عنوان مثال اگر فایل myservlets.jar.old که ‏یک کپی پشتیبان از کلاس‌های پیاده‌سازی servlet شما می‌باشد را در محیط برنامه رها کنید، شما اطلاعات حساس زیادی را در معرض قرار داده اید که مستعد تجزیه و تحلیل و مهندسی معکوس است.
* در برخی موارد کپی کردن یا ویرایش یک فایل، پسوند آن را تغییر نمی‌دهد، بلکه نام پرونده را عوض می‌کند. این اتفاق در محیط‌های ویندوز بسیار رخ می‌دهد، هنگامی که عملیات کپی کردن فایل صورت می‌گیرد، نام فایل‌ها با عبارت “Copy of” تولید می‌شود. از آنجا که پسوند فایل بدون تغییر باقی می‌ماند، این موردی نیست که در آن یک فایل اجرایی به عنوان متن ساده توسط سرور وب برگشت داده شود، بنابراین یک مورد از افشای کد منبع نیست. با این حال، این فایل‌ها نیز خطرناک هستند، زیرا این احتمال وجود دارد که آن‌ها شامل منطق منسوخ و نادرست باشند که در صورت فراخوانی، می‌توانند خطاهای برنامه را برگردانند، که اگر نمایش پیام خطا فعال باشد، ممکن است اطلاعات ارزشمندی را برای یک مهاجم به ارمغان بیاورند.
* فایل‌های لاگ نیز ممکن است حاوی اطلاعات حساس در مورد فعالیت‌های کاربران برنامه باشند، برای مثال داده‌های حساس ارسال شده در پارامترهای URL، شناسه نشست وURL بازدید شده، نمونه‌هایی از موارد حساس در لاگ فایل‌ها می‌باشند ‏که ممکن است محتوای Unreferenced اضافی را آشکار کند. همچنین Snapshot‌های مربوط به سیستم فایل ممکن است حاوی کپی‌هایی از یک بخش کد بوده و حاوی نقاط ضعفی هستند که در نسخه‌های اخیر برطرف شده‌اند. به عنوان مثال snapshot/monthly.1/view.php. ممکن است حاوی یک آسیب پذیری Directory Traversal باشد که در فایل view.php رفع شده است ولی هنوز امکان اکسپلویت آن در فایل پیشین برای افرادی که به آن دسترسی دارند، وجود داشته باشد.

# اهداف تست

در این بخش باید فایل‌های مرجع نشده که ممکن است حاوی اطلاعات حساس باشند را پیدا و آنالیز نمایید.

### چگونگی انجام تست
#### آزمایش جعبه سیاه

جهت آزمایش فایل‌های مرجع نشده از هر دو تکنیک خودکار و دستی استفاده می‌شود و معمولا شامل ترکیبی از موارد زیر است:

##### نتیجه‌گیری از طرح نام گذاری مورد استفاده برای محتوای منتشر شده

صفحات و قابلیت‌های برنامه را جمع آوری (Enumerate) نمایید. این کار می‌تواند به صورت دستی و با استفاده از یک مرورگر، یا با استفاده از یک ابزار Spidering انجام شود. بیشتر برنامه‌ها از یک طرح نامگذاری قابل‌تشخیص استفاده می‌کنند و منابع را در صفحات و دایرکتوری‌ها با استفاده از کلماتی که عملکرد آن‌ها را توصیف می‌کنند، سازماندهی می‌کنند. از طرح نامگذاری مورد استفاده برای محتوای منتشر شده، اغلب استنباط نام و محل صفحات Unreferenced امکان پذیر است. به عنوان مثال، اگر صفحه viewuser.asp را شناسایی نمودید، آنگاه به دنبال صفحاتی مانندedituser.asp، adduser.asp و یا deletuser.asp باشید.

همچنین اگر یک دایرکتوری با نام / app / user پیدا شد، سپس به دنبال / app / admin و / app / manager نیز باشید.

### دیگر موضوعات در محتوای منتشر شده

بسیاری از برنامه‌های کاربردی وب سرنخ‌هایی را در محتوای منتشر شده باقی می‌گذارند که می‌تواند منجر به کشف صفحات و قابلیت‌های پنهان شود. این سرنخ‌ها اغلب در سورس کد HTML و یا فایل‌هایJavaScript قابل مشاهده هستند. کد منبع برای تمام مطالب منتشر شده باید به صورت دستی بررسی شود تا سرنخ‌هایی در مورد صفحات و قابلیت‌های دیگر شناسایی گردد.

### به عنوان مثال:

نظرات برنامه نویسان و بخش‌های توضیحی موجود در سورس کد ممکن است به محتوای پنهان اشاره داشته باشند:
```html
<!-- <A HREF="uploadfile.jsp">Upload a document to the server</A> -->
<!-- Link removed while bugs in uploadfile.jsp are fixed -->

```

فایل‌ها یا کدهای جاوا اسکریپت ممکن است حاوی لینک‌ مربوط به صفحه‌ای باشند که تنها در واسط گرافیکی کاربر تحت شرایط خاصی ارائه شده‌اند:
```js
var adminUser=false;
if (adminUser) menu.add (new menuItem ("Maintain users", "/admin/useradmin.jsp"));
```
صفحات HTML ممکن است حاوی فرم‌هایی باشند که با غیرفعال بودن عنصر SUBMIT پنهان هستند:
```html
<form action="forgot Password.jsp" method="post">
<input type="hidden" name="userID" value="123">
<!-- <input type="submit" value="Forgot Password"> --> </form>
```

منبع دیگر سرنخ‌ها در مورد دایرکتوری‌های Unreferenced، فایلrobots.txt است که برای ارائه دستورالعمل به ربات‌های وب مورد استفاده قرار می‌گیرد:
```html
User-agent:
Disallow: /Admin
Disallow: /uploads
Disallow: /backup Disallow: /~jbloggs Disallow: /include
```

### حدس زدن کور

در ساده‌ترین شکل ممکن، این کار شامل اجرای فهرستی از نام فایل‌های رایج از طریق یک موتور درخواست و تلاش برای حدس زدن فایل‌ها و دایرکتوری‌های موجود بر روی سرور است. اسکریپت زیر که باnetcat ترکیب شده است یک Guessing Attack ساده را علیه وب سرور انجام می‌دهد:

```bash
#!/bin/bash
server=example.org
port=80
while read url
do
echo -ne "$url\t"
echo -e "GET /$url HTTP/1.0\nHost: $server\n" | netcat $server $port | head -1 
done | tee outputfile
```

بسته به سرور، شما ممکن است متد GET را برای دریافت سریع تر نتایج با متد HEAD جایگزین نمایید. با بررسی کدهای وضعیت (Status Code) موجود در پاسخ، می‌توان نتیجه لازم را دریافت نمود. کد پاسخ ۲۰۰ (OK)‏ ‏معمولا نشان می‌دهد که یک منبع معتبر پیدا شده ‌است. (‏در صورتی که سرور صفحه سفارشی “not found” را با استفاده از کد ۲۰۰ تحویل ندهد) اما همچنین به دنبال ۳۰۱ (Moved)‏، ۳۰۲ (Found)‏، ۴۰۱ ‏ (Unauthorized)‏، ۴۰۳ (Forbidden)‏و ۵۰۰ (Internal Error)‏ باشید، چراکه ممکن است منابع یا دایرکتورهایی را نشان دهد که ارزش بررسی بیشتر را دارند.

در ادامه می‌بایست Guessing Attack بر روی دایرکتوری root و همچنین بر روی تمام دایرکتورهایی که از طریق تکنیک‌های دیگر Enumeration، شناسایی شده‌اند، اجرا شود. حملات پیشرفته‌تر و موثرتر Guessing را می‌توان به طرق زیر انجام داد:

* شناسایی پسوند‌های فایل مورد استفاده در نواحی شناخته‌شده برنامه (‏به عنوان مثال jsp، aspx، html)‏ و استفاده از یک Wordlist اولیه برای بررسی آن‌ها (‏یا استفاده از یک لیست طولانی‌تر از پسوند‌های رایج به شرط اینکه منابع سرور دچار مشکل نشود)‏
* برای هر فایل شناسایی ‌شده از طریق تکنیک‌های Enumeration، یک Wordlist سفارشی ایجاد نموده و فهرستی از پسوند‌های فایل معمول (‏از جمله ~، bak، txt، src، dev، old، inc، orig، copy، tmp، swp و غیره) ‏تهیه نمایید. سپس از هر پسوند، قبل، بعد و به جای پسوند فایل واقعی استفاده کنید. (index.asp.bak – index.bak – bak.bak)
اطلاعات به‌دست‌آمده از طریق آسیب‌پذیری‌های سرور و پیکربندی نادرست
بدیهی‌ترین روشی که در آن سرور با پیکربندی نادرست، ممکن است صفحات بدون ارجاع را افشا کند، از طریق فعال بودن Directory Listing است. در این بخش می‌بایست تمام دایرکتوری‌های برشمرده‌شده(Enumerated) را برای شناسایی هر کدام از مواردی که Directory Listingرا فراهم می‌کنند، درخواست کنید. زیرا ممکن است قابلیت Directory Listing تنها برای برخی از دایرکتوری‌ها فعال باشد.

اطلاعات به دست آمده از آسیب پذیری های موجود در سرور و پیکربندی نادرست

آسیب‌پذیری‌های متعددی در سرورهای وب یافت شده‌است که به مهاجم این امکان را می‌دهد تا محتوای ارجاع نشده را شمارش کند، برای مثال:

* آسیب پذیری Directory Listing در وب سرور آپاچی (?M=D)
* اسکریپت‌های IIS مختلف، که منجر به افشای سورس می‌شود.
* آسیب پذیری Directory Listing مربوط به IIS WebDAV

استفاده از اطلاعات عمومی موجود

صفحات و عملکردها در برنامه‌های کاربردی وب که از درون برنامه کاربردی ارجاع داده نمی‌شوند، ممکن است از دیگر منابع دامنه عمومی ارجاع داده شوند.

منابع مختلفی که برای این موضوع وجود دارد شامل:

* صفحات که قبلا ارجاع داده می‌شدند، هنوز هم ممکن است در آرشیوهای موتورهای جستجوی اینترنتی وجود داشته و در نتایج جست و جو ظاهر شوند. برای مثال، ممکن است لینک مربوط به صفحه1998result.asp دیگر در وب سایت شرکت وجود نداشته باشد، اما ممکن است در سرور و در پایگاه‌های داده موتور جستجو باقی بماند. این اسکریپت قدیمی ممکن است شامل آسیب‌پذیری‌هایی باشد که می‌تواند برای به خطر انداختن کل سایت مورد استفاده قرار گیرد. به منظور شناسایی این صفحات از طریق موتور جست و جوی گوگل می‌توان از دستورsite: استفاده نمود، مانند: site:www.example.com. در این صورت فایل‌هایی که توسط گوگل ایندکس شده اند نمایش داده می‌شود. البته فایل‌های پشتیبان به احتمال زیاد توسط فایل‌های دیگر ارجاع داده نمی‌شوند و بنابراین ممکن است توسط گوگل ایندکس نشده باشند، اما اگر آن‌ها در دایرکتوری‌های قابل مرور (Directory Listing) قرار گیرند، موتور جستجو ممکن است اطلاعات لازم در مورد آن‌ها را بدست آورد.

* علاوه بر این، گوگل و یاهو نسخه‌های ذخیره‌سازی شده از صفحات یافت‌شده توسط روبات‌های خود را حفظ می‌کنند. حتی اگر 1998result.asp از سرور حذف شده ‌باشد، یک نسخه از خروجی آن ممکن است هنوز هم توسط این موتورهای جستجو ذخیره شده باشد. نسخه ذخیره‌شده ممکن است حاوی منابع، یا سرنخ‌هایی در مورد محتوای مخفی باشد که هنوز بر روی سرور باقی مانده‌است.

* محتوایی که از درون یک برنامه کاربردی مورد اشاره قرار نمی‌گیرد، ممکن است توسط وب سایت‌های شخص ثالث به هم متصل شود. به عنوان مثال، برنامه‌ای که پرداخت‌های آنلاین را به نمایندگی از تاجران شخص ثالث پردازش می‌کند ممکن است شامل انواع قابلیت‌های قراردادی باشد که تنها با لینک‌های موجود در وب سایت‌های مشتریان خود یافت می‌شود.

### مدل Filename Filter Bypass

از آنجا که انکار فیلترهای لیست براساس عبارات منظم هستند، گاهی اوقات می‌توان از ویژگی‌های مبهم مربوط به نام فایل در OS استفاده کرد که توسعه دهنده انتظار آن را ندارد.
تست نفوذگر گاهی اوقات می‌تواند از روش‌های متفاوتی استفاده کند که نام فایل‌ها موجود در برنامه، سرور وب و OS و الگوهای مربوط به نام فایل آن را تجزیه و تحلیل نماید.
به عنوان مثال در الگوی نام 8.3 در ویندوز c:\program files به C:\PROGRA~1 تبدیل می‌شود. (Tilde Directory)

در این حالت موارد زیر رخ باید انجام شود:

* حذف کاراکترهای ناسازگار
* تبدیل Space به Underscore
* دریافت شش کاراکتر اول نام اصلی
* اضافه نمودن عبارت ~ و عدد به انتهای شش کاراکتر

در این الگو، شش کاراکتر برای نام فایل و سه کاراکتر هم برای پسوند فایل در نظر گرفته می‌شود.
آزمایش جعبه خاکستری

اجرای آزمایش جعبه خاکستری در برابر فایل‌های قدیمی و پشتیبان، نیاز به بررسی فایل‌های موجود در دایرکتوری‌های متعلق به وب دارد. از لحاظ تئوری، بررسی باید با دست انجام شود تا کامل باشد. با این حال، از آنجا که در بسیاری از موارد در نامگذاری فایل‌ها تمایل به استفاده از نام‌های مشابه مشابه دارد، این جستجو می‌تواند بوسیله یک اسکریپت نیز انجام شود. به عنوان مثال، قرار دادن .old به پایان فایل‌ها از نمونه‌های پر استفاده در این موضوع می‌باشد. یک استراتژی خوب، یک بررسی زمانبندی شده برای شناسایی فایل‌هایی با پسوندهای ذکر شده می‌باشد و همچنین انجام بررسی‌های دستی، که نیاز به زمان طولانی تری برای انجام آن خواهد بود.

### مدل Remediation

برای تضمین یک استراتژی حفاظتی موثر، تست ما باید با یک سیاست امنیتی ترکیب شود که به وضوح اقدامات خطرناک را ممنوع می‌کند. به موارد زیر دقت نمایید:

* ویرایش پرونده‌ها در محل، بر روی سرور وب یا سیستم‌های فایل اپلیکیشن سرور. این یک عادت بد است، زیرا به احتمال زیاد این امر، منجر به ایجاد فایل‌های پشتیبان یا موقتی توسط ویراستاران می‌گردد. شگفت‌انگیز است که بدانید این کار حتی در سازمان‌های بزرگ نیز تا حد زیادی انجام می‌شود. اگر شما کاملا نیاز به ویرایش فایل‌ها در یک محیط عملیاتی دارید، مطمئن شوید که چیزی را در برنامه رها نکنید (فایل اضافه ای ایجاد نکنید) و در نظر بگیرید که این کار را با ریسک خودتان انجام می‌دهید.
* به دقت هر فعالیت دیگری که بر روی سیستم‌ فایل وب سرور انجام می‌شود، را بررسی کنید. به عنوان مثال، اگر گاهی اوقات لازم است از چند دایرکتوری Snapshot تهیه نمایید (‏که نباید در سیستم‌های محیط عملیاتی آن‌ها را انجام دهید)‏، ممکن است وسوسه شوید که اول آن‌ها را در یک فایل زیپ قرار دهید تا در ادامه بتوانید آن را به صورت مستقیم دانلود نمایید. مراقب باشید که این فایل‌های آرشیو را رها نکنید.
* ایجاد سیاست‌های مناسب مدیریت پیکربندی که به جلوگیری از فایل‌های منسوخ و مرجع نشده (unreferenceed) کمک کند.
* فایل‌های داده، فایل‌های لاگ، فایل‌های پیکربندی و غیره باید در دایرکتورهایی که توسط سرور وب قابل‌دسترسی نیستند ذخیره شوند تا افشای آن‌ها به صورت مستقیم از برنامه وب امکان پذیر نباشد.
* Snapshot‌های تهیه شده از فایل سیستم نباید از طریق وب در دسترس باشند. وب سرور خود را پیکربندی کنید تا دسترسی به این دایرکتوری‌ها را رد کنید، برای مثال در وب سرور Apache پیشنهاد می‌شود تا از تنظیمات زیر برای این موضوع، استفاده نمایید:
```html
<Location ~ ".snapshot">
Order deny, allow Deny from all
</Location>
```

# ابزارها

ابزارهای ارزیابی آسیب‌پذیری معمولا شامل بررسی‌های لازم را برای شناسایی دایرکتوری‌های وب با نام‌های استاندارد (‏مانند “admin”، “test”، “backup” و غیره)‏ بوده و گزارش هر دایرکتوری وب که امکان Indexing را فراهم می‌کند، در اختیار ما قرار می‌دهد. اگر نمی‌توانید هیچ Directory Listing در برنامه به دست آورید، باید تلاش کنید تا نسخه پشتیبان احتمالی را بررسی کنید. ابزارهایی مانند Nessus و Nikto2 در این بخش مورد استفاده قرار می‌گیرد. همچنین ابزارهای زیر می‌توان به منظور انجام Spidering استفاده نمود:

* wget
* Wget for Windows
* Sam Spade
* Spike proxy includes a web site crawler function
* Xenu
* curl

برخی از این ابزارها در توزیع‌های استاندارد لینوکس گنجانده شده‌اند. همچنین ابزارهای توسعه وب معمولا شامل امکاناتی برای شناسایی لینک‌های شکسته (Broken Link) و فایل‌های مرجع نشده (Unreferenced Files) هستند.
