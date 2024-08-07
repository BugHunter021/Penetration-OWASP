# رمزنگاری در وب

در این بخش از دوره آموزشی SEC542 به مباحث مختلف در حوزه رمزنگاری وب و آسیب پذیری های موجود در این بخش می پردازیم.

### توضیح Encrypting HTTP in Transit

پروتکل HTTP موفقیت بزرگی به حساب می آید (البته برای نفوذگران). چرا که هر کسی با استفاده از ابزارهایی مانند وایرشارک یا TCPDump به راحتی می تواند محتویات فرستاده شده در شبکه را بررسی و تحلیل نمایید. به منظور رسیدگی به این مشکل، ما بر Transport-Layer Security یا TLS تکیه می کنیم. نسخه پیشین TLS سرویس Secure Sockets Layer یا SSL می باشد.

به طور غیر رسمی، اغلب مردم از SSL برای اشاره هم به SSL و هم TLS استفاده می کنند. SSL نسخه دو و قبل از آن به مراتب از امنیت کمتری نسبت به SSL نسخه سه برخوردار هستند. توجه داشته باشید که SSL/TLS تحت لایه Application در مدل OSI عمل می کنند و می تواند برای هر پروتکل لایه Application مورد استفاده قرار گیرد. ویژگی مذکور به ما این امکان را می دهد تا از آن به منظور ایمن سازی پروتکل هایی مانند SMTP یا IMAP استفاده کنیم. پروتکل های LDAPS، SMTPS، IMAPS و HTTPS نمونه های موفقی از این دست بوده اند و محصولات VPN مانند OpenVpn بوسیله SSL قدرت یافته اند و همچنین اجازه نصب بسیار ساده تر از گزینه های IPSEC را می دهند.

توجه داشته باشید کهTLS/SSL متکی به زیر ساخت کلید عمومی یا PKI به منظور رمزنگاری و Trust می باشد.

### توضیح Certificate Trusts

صاحب وب سرور یک کلید ایجاد می کند و آن را به یک اعتبار دهنده گواهینامه یا Certificate Authority عرضه می کند.
مرکز صدور گواهیCA، مراکزی امین و دارای اعتبار می باشند که وظیفه مطابقت کلیدهای عمومی یک شخص برای تایید هویت و شناسایی آن شخص را دارند و مشخص می کنند که این کلید خاص متعلق به شخص خاصی است.

در این مراکز گواهینامه ها صادر، باطل و یا تمدید می شوند. زمانی که یک مرکزCA، شخص یا سازمانی را تایید می کند، آنگاه کلید عمومی با کلید خصوصی مرتبط با آن از طریق مرکز امنی مطابقت داده می شود.
درگواهینامه های دیجیتالی، امضای دیجیتالی صادرکننده گواهی و همچنین نام صادرکننده گواهی از مراکز CA وارد می شود، که دارابودن این ویژگی، ضمانت مورد نیازرا در رابطه با اینکه این کلید عمومی متعلق به این شخص هست را تامین می کند. بنابراین معتبربودن افراد نیز توسط CA تایید می شود.

در واقع این مراکز با تایید هویت شخص یا سازمان اقدام به صدورگواهینامه می کنند. بنابراین با درخواست گواهینامه از طرف شخص یا سازمان، دو کلید عمومی و خصوصی از طرف مرکز CA به آنها تحویل داده می شود، که توسط مشخصات شخص یا سازمان و کلید عمومی، مراکز CA گواهینامه ای صادر می کنند که مشمول امضای صادرکننده گواهی، مشخصات شخص یا سازمان و تاریخ اعتبار آن گواهی می باشد. در این میان، کلید خصوصی توسط شخص یا سازمان در مکان امنی نگهداری می شود.
 
 ### آیتم CA رمزنگاری در وب

هنگامی که مرورگر وب یک صفحه HTTPS را از سرور وب درخواست می کند، کلید عمومی سرور را دریافت می کند. به دلیل این که Sign شدن توسط CA انجام شده و CA قابل اطمینان می باشد، مرورگر نیز به آن سرور اعتماد کرده و به این نتیجه می رسد که سرور همان چیزی است که خودش را معرفی می کند(به معنی تایید سرور است).
پروتکل HTTPS برای ایجاد یک اتصال رمز شده بین دو سیستم کامپیوتری مورد استفاده قرار می گیرد. این حفاظت از اطلاعات موجب می شود تا داده های منتقل شده از دید ابزارهای Sniffing در امان بماند.

البته هر سیستمی که مربوط به این ارتباطات می باشد، قادر به رمزگشایی داده ها نیز هست. این سیستم می تواند یک سرور پروکسی باشد که وظیفه فوروارد نمودن و دریافت ترافیک را برای اتصال دارد بوده و یا سیستم های نهایی باشد. در هر دو طرف، مهاجم می تواند به راحتی به داده ها با استفاده از ابزارهای Debugging و تکنیک DLL Injection دسترسی پیدا نماید.

مشکل دیگری که وجود دارد این است که حملات برنامه های وب که از جلسات رمز شده HTTPS استفاده می کنند از دید دستگاه های استاندارد شبکه و سیستم های تشخیص نفوذ پنهان می مانند به دلیل اینکه آن ها رمزنگاری شده اند. این موضوع به مهاجمین اجازه می دهد تا اکسپلویت های خود را با کمترین احتمال تشخیص اجرا نمایند. البته بسیاری از شرکت های ارائه دهنده سیستم های تشخیص نفوذ، در محصولات خود قابلیت رمزگشایی ترافیک HTTPS را قرار می دهند که این موضوع نیاز مند داشتن کلید خصوصی مربوط به سرورهای محافظت شده می باشد.
همچنین برخی از وب سایت های از معامری دیگر استفاده می کنند که با نام HTTPS Accelerator شناخته می شود و اجازه می دهد که HTTPS بر روی دستگاه شبکه پیش از رسیدن داده ها به وب سرور اصطلاحا Terminate شود.
این عملکرد نه تنها باعث افزایش عملکرد می شود بلکه همچنین اجازه می دهد تا IDS ترافیک بین HTTPS Accelerator و وب سرور را مشاهده نماید.

### بخش Testing for Weak SSL/TLS Ciphers

یکی از بخش های موجود در متدولوژی OWASP، تست رمزنگاری یا Cryptography می باشد. هدف این بخش، اطمینان از این است که اطلاعات به صورت محرمانه و ایمن انتقال پیدا می کند. بخش اول از این تست که با عنوان OTG-CRYPST-001 می باشد، به دنبال تضمین امنیت با تمرکز بر تست کارایی رمزنگاری حمل و نقل است.

ابزارهای مختلفی برای کمک به تست پیکربندی HTTPS وجود دارند که سه مورد از محبوبترین آن ها OpenSSL، اسکریپت ssl-enume-cipher ابزار Nmap و آزمایشگاه Qualys SSL می باشد. با استفاده از این ابزارها، می توان نوع پروتکل استفاده شده، نوع و طول کلید استفاده شده، معتبر بودن گواهینامه و اطلاعات دیگری در خصوص پروتکل HTTPS را به شما ارائه می دهد. همچنین باید کنترل کنیم که منابع مورد نظر ما که باید با HTTPS به آن ها دسترسی پیدا کرد، آیا بوسیله HTTP هم قابل دسترسی هستند یا خیر.

### ابزار Openssl

ابزار Openssl ما را قادر می سازد تا گواهینامه ها را تولید، sign، مدیریت نموده و تایید اعتبار گواهی ها را انجام دهیم. همچنین با استفاده از Openssl می توان ارتباطات SSL را به صورت مستقیم ایجاد نمود. در برخی مقالات افراد به Openssl لقب چاقوی سوئیسی SSL را می دهند همانند ابزار Netcat که به عنوان چاقوی سوئیسی TCP/IP می باشد.

این ابزار در اکثر سرورها موجود می باشد. برای تست SSLv2 از دستور زیر استفاده می شود:
```bash
openssl s_client -connect www.google.com:443 -tls1
```
با استفاده از دستور بالا بررسی می شود که آیا سرور نسخه ضعیف از TLS نسخه پایین را پشتیبانی می کند یا خیر؟

علاوه بر این دستور زیر نیز نشان می دهد که آیا سرور Null Cipher را فعال نموده است یا خیر که در واقع انتقال در این صورت به شکل Plain Text می شود.

### استفاده از Nmap برای تست SSL

برای تست SSL با استفاده از ابزار Nmap می توان از دستور زیر استفاده نمود:
```bash
nmap -sV --script ssl-enum-ciphers -p 443 google.com
```

در این اسکریپت به هر کدام از Cipher ها رتبه بندی از A تا F داده می شود که نشان دهنده میزان قدرت یا ضعف در کلیدهای رمزنگاری آن ها می باشد.

به منظور کسب اطلاعات بیشتر در مورد این اسکریپت می توانید به لینک زیر مراجعه نمایید:

https://nmap.org/nsedoc/scripts/ssl-enum-ciphers.html

### بررسی کیفیت Qualys SSL Labs

یک منبع برجسته عمومی برای ارزیابی تنظیمات SSL از طریق Qualys در دسترسی می باشد. وب سایت ssllab.com به صورت رایگان امکان تست پیکربندی SSL را برای شما فراهم می نماید. با ورود به سایت شما می توانید بر روی لینک Test your server کلیک نموده و آدرس سایت خود را در آن وارد نمایید تا ارزیابی آن آغاز گردد.
این وب سایت پس از انجام تست، جزئیات بسیار جالبی را از نحوه پیکربندی SSL به شما نمایش می دهد و رتبه بندی مورد نظر خود را نیز به شما اعلام می نماید. تصویر زیر نمونه ای از ارزیابی وب سایت sans.org با استفاده از ssllab.com می باشد:

### سایت Qualys SSL Labs

با توجه به داده هایی که با ابزارهای مذکور به دست آمده است، هم اکنون باید این داده ها مورد تجزیه و تحلیل قرار گیرد و در نهایت می بایست مشکلات موجود در SSL به صاحب سایت ارائه شود. به عنوان مثال پشتیبانی از SSL نسخه دو و سه یا قبل از آن با توجه به وجود آسیب پذیری های شناخته شده، باید گزارش شوند. همچنین نسخه هایی از SSL که رمزنگاری زیر 128 بیتی را پشتیبانی می کنند و الگوریتم های Hashing ضعیف مانند MD5 یا SHA1 نیز می توانند مشکل ساز باشد که این موارد نیز باید گزارش شوند. علاوه بر این بدون توجه به تنظیمات، هر گونه مسائل مربوط به گواهینامه مانند منقضی شدن گواهی نامه نیز جزء مواردی است که باید گزارش شود.

### مشکل Heartbleed

مشکل (Heartbleed) که در لغت به معنای خونریزی قلبی است، یک اشکال نرم‌افزاری در کتابخانه‌ رمزنگاری متن باز Open SSL است که به مهاجم اجازه‌ خواندن حافظه‌ رایانه‌ای که در حال اجرای این نرم‌افزار است را 
می‌دهد. این اشکال امنیتی مرتبط با مدیریت حافظه در ماژول Heratbeats این برنامه می باشد.

این مشکل امنیتی می‌تواند در هر تپش اکستنشن Heartbeat مربوط به Openssl، تا ۶۴ کیلو بایت اطلاعات از حافظهٔ مربوط به همین نرم‌افزار در RAM رایانه را در اختیار مهاجم قرار دهد. این باگ درون ماژول Heartbeats از پکیج openssl قرار دارد که به همین دلیل این باگ را خونریزی قلبی (Heartbleed) نام گذاری کرده‌اند که کد cve mitro باگ نیز CVE-2014-0160 می باشد.

سازوکار این حفره امنیتی به این صورت است که شخص مهاجم یک در خواست هارت بیت (تپش قلب) دستکاری شده را به سروری که در حال اجرای openssl است ارسال می کند و منتظر پاسخ سرور می‌ماند. از آنجایی که مشکل کنترل طول متغیر در برنامه‌نویسی openssl ویرایش‌های آسیب پذیر وجود دارد، برنامه نمی‌تواند صحت درخواست مهاجم را ارزیابی نماید و این مسئله موجب می‌شود که برنامه به درخواست مهاجم پاسخ دهد و ۶۴ کیلوبایت از حافظه برنامه را به شکل تصادفی بخواند و برای مهاجم ارسال نماید.

در حین حمله، هر تپش قلب 64KB از پکت‌هایی که با استفاده از پروتوکل Transport Layer Security V1 با سرور رد و بدل می‌شوند و در حافظه سیستم ذخیره می‌شوند را آشکار می‌کند و به مهاجم ارسال می‌کند. حال اگر این عمل تکرار شود اطلاعاتی از جمله نام کاربری و پسورد سرور یا پسورد مدیران یا اطلاعات کاربران یا هر اطلاعات امنیتی دیگری مانند کوکی‌ها نیز به دست خواهند آمد.

این آسیب پذیری هم‌چنین به مهاجم اجازه‌ بازیابی کلید‌های شخصی SSL را می‌دهد. این حفره به هکر ها اجازه سرقت اطلاعاتی که توسط SSL/TLS رمزنگاری شده اند را می دهد که این امر موجب میشود تا اطلاعات مهمی نظیر کلمات عبور وب سایت ها، ایمیل ها، پیام های فوری (IM) و حتی اطلاعات انتقال یافته بر روی برخی شبکه های خصوصی مجازی ( VPN ) قابل کشف باشد. این حفره تا زمانی که نسخه آسیب پذیر OpenSSL بر روی سرور نصب باشد می تواند منشاء سرقت اطلاعات در حال تبادل با سرور باشد. بااستفاده از این حفره امنیتی میتوان بین سرور و کلاینت یک نشست ایجاد کرد و محتویات حافظه RAM را استخراج کرد که امکان کشف اطلاعاتی مانند کلید های رمزنگاری شده را میسر می نماید.

بطور مثال میتوان با ردیابی اطلاعات فردی که از طریق سرویس https به سایتی که آسیب پذیر است متصل شده ؛ تمامی مشخصات و اطلاعات مبادله شده مانند نام کاربری و رمزعبور وی را کشف کرد. نکته قابل توجه این است که این حفره امنیتی چون از سمت پروتکل TLS سرویس را مورد حمله قرار میدهد؛ می تواند امنیت سرویس هایی نظیر HTTP،FTP،SMTP را نیز به خطر بیندازد.
این مشکل امنیتی در تاریخ ۱۴ مارس ۲۰۱۲ به شکل گسترده‌ای در کارگزار‌های وب در سراسر دنیا انتشار یافته و اولین بار در آوریل ۲۰۱۴ توسط یکی از مهندسین تیم امنیتی گوگل شناسایی شد.

حتی گفته می شود که این باگ به مدت 2 سال بر روی سرور های NSA وجود داشته و هیچکس از آن اطلاع نداشته است. لازم به ذکر است که Openssl نسخه کمتر از 1.0.1 یا بزرگتر از 1.0.1g آسیب پذیر نیستند.

جهت کسب اطلاعات بیشتر در خصوص این آسیب پذیری می توانید به سایت زیر نیز مراجعه نمایید:

http://heartbleed.com/

### چالش The CloudFlare Challenge

بسیاری از افراد معتقدند که کلید خصوصی سرور را نمی توان از طریق آسیب پذیری Heartbleed به سرقت برد. در همین راساتا CloudFlare یک چالشی را ایجاد نمود که در آن یک سرور آسیب پذیر وجود داشت و شما می بایست کلید خصوصی سرور را استخراج نمایید. در توضیح این چالش آمده است که :
“ما یک سرور Nginx را با آسیب پذیری Openssl راه اندازی نمودیم و چالش این است که شما باید کلید خصوصی را به سرقت ببرید.”
جالب است که در اولین روز از آغاز چالش، چهار نفر موفق شدند تا کلید خصوصی را به سرقت ببرند.
شما می توانید نحوه دسترسی به این سرور و استخراج کلید را در لینک زیر مشاهده نمایید:

https://gist.github.com/epixoip/10570627
### اکسپلویت  Exploit

به منظور اکسپلویت آسیب پذیری Heartbleed شما می توانید از یک کد پایتونی که در آدرس زیر قابل دسترسی می باشد استفاده نمایید:

https://github.com/sensepost/heartbleed-poc/blob/master/heartbleed-poc.py

### بررسی CHS Heartbleed Compromise

سیستم Community Health System (CHS) که سیستم های بهداشت عمومی هستند، از طریق یک دستگاه Juniper VPN که دارای آسیب پذیری Heartbleed بود مورد نفوذ قرار گرفت. در این مورد نفوذگران توانسته بودند که اطلاعات کاربری را از حافظه یک دستگاه CHS Juniper که در آن زمان از آسیب پذیری Heartbleed رنج می برد، به دست آورند و از طریق VPN وارد سیستم شوند. لازم به ذکر است که در این حمله حدود 4 و نیم میلیون پرونده سلامت به سرقت رفت.
خبر مربوط به این حمله در سایت Trustsec نیز منتشر شده است که برای مطالعه آن می توانید به لینک زیر مراجعه نمایید:

https://www.trustedsec.com/2014/08/chs-hacked-heartbleed-exclusive-trustedsec/

# منابع این بخش:

https://news.asis.io
