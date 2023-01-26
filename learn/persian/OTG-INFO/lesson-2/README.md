# جمع‌آوری اطلاعات از وب سرور

در این بخش از دوره آموزشی **OWASP-OTG** به اولین بخش از استاندارد OTG با شناسه OTG-INFO-002 می پردازیم که مربوط به جمع آوری اطلاعات از وب سرور می باشد.

# خلاصه

جمع‌آوری اطلاعات از وب سرور یک امر مهم برای ارزیاب امنیتی محسوب می‌شود. نوع و نسخه وب سرور دامنه کاری ارزیابان امنیتی را محدود می‌کند و به آن‌ها این اجازه را می‌دهد تا حین تست، آسیب‌پذیری‌ها و اکسپلویت‌های مخصوص آن وب سرور را پیدا کنند.

در بازار امروز، برندها و نسخه‌های مختلف وب سرور موجود هستند. یافتن نوع وب سرور مورد ارزیابی در فرایند تست و ارزیابی کمک بسزایی می‌کند و حتی می‌تواند مسیر ادامه کار را تغییر دهد. این اطلاعات را می‌توان از طریق فرستادن دستورات خاصی به وب سرور و بررسی خروجی‌های آن‌ها یافت.

توجه داشته باشید که نسخه‌های مختلف یک برند وب سرور می‌تواند جواب‌های مختلفی به سوال‌ها و دستورات یکسان بدهند. با دانستن نحوه‌ی پاسخگویی وب سرورها به دستورات خاص و ذخیره آن‌ها در یک پایگاه داده مخصوص، ارزیاب امنیتی می‌تواند این دستورات را به وب سرور ارسال و خروجی آن‌ها را بررسی کرده و با اطلاعات پایگاه داده مقایسه کند تا بتواند نوع و نسخه وب سرور را مشخص کند.

لازم به ذکر است که امکان دارد که نسخه‌های مختلف به دستورات یکسان، عکس‌العمل یکسانی نشان دهند؛ پس برای اینکه بتوانید نسخه وب سرور را دقیقاً مشخص کنید، حتماً از چندین دستور مجزا استفاده کنید. احتمال اینکه نسخه‌های مختلف به دستورات HTTP عکس‌العمل یکسانی نشان بدهند، کم است؛ بنابراین، ارزیاب امنیتی با افزایش تعداد دستورات متنوع ارسال شده به وب سرور، دقت تشخیص خود را افزایش دهند.
# اهداف تست و ارزیابی

هدف از این تست، یافتن نوع و نسخه وب سرور و آسیب‌پذیری‌ها و در نتیجه اکسپلویت‌های مرتبط با آن در حین ارزیابی است.

### چگونه تست کنیم؟

#### تست جعبه سیاه

آسان‌ترین و ابتدایی‌ترین روش شناسایی سرور، بررسی بخش Server در سرایند پاسخ HTTP است. در این آزمایش از ابزار netcat استفاده شده است.

سوال و پاسخ HTTP زیر را در نظر بگیرید:

```bash
$ nc 202.41.76.251 80
HEAD / HTTP/1.0
```

```js
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2003 02:53:29 GMT
Server: Apache/1.3.3 (Unix) (Red Hat/Linux)
Last-Modified: Wed, 07 Oct 1998 11:18:14 GMT
ETag: “1813-49b-361b4df6”
Accept-Ranges: bytes
Content-Length: 1179
Connection: close
Content-Type: text/html
```

اطلاعات بخش Server بیانگر این هستند که نوع و نسخه سرور مورد نظر آپاچی 1.3.3 است. در زیر چهار سرایند پاسخ HTTP برای سرورهای مختلف آورده شده است.
سرور آپاچی نسخه 1.3.23

HTTP/1.1 200 OK
Date: Sun, 15 Jun 2003 17:10: 49 GMT
Server: Apache/1.3.23
Last-Modified: Thu, 27 Feb 2003 03:48: 19 GMT
ETag: 32417-c4-3e5d8a83
Accept-Ranges: bytes
Content-Length: 196
Connection: close
Content-Type: text/HTML
سرور IIS مایکروسافت نسخه 5.0

HTTP/1.1 200 OK
Server: Microsoft-IIS/5.0
Expires: Yours, 17 Jun 2003 01:41: 33 GMT
Date: Mon, 16 Jun 2003 01:41: 33 GMT
Content-Type: text/HTML
Accept-Ranges: bytes
Last-Modified: Wed, 28 May 2003 15:32: 21 GMT
ETag: b0aac0542e25c31: 89d
Content-Length: 7369
سرور Netscape Enterprise نسخه 4.1

HTTP/1.1 200 OK
Server: Netscape-Enterprise/4.1
Date: Mon, 16 Jun 2003 06:19: 04 GMT
Content-type: text/HTML
Last-modified: Wed, 31 Jul 2002 15:37: 56 GMT
Content-length: 57
Accept-ranges: bytes
Connection: close
سرور SunONE نسخه 6.1

TTP/1.1 200 OK
Server: Sun-ONE-Web-Server/6.1
Date: Tue, 16 Jan 2007 14:53:45 GMT
Content-length: 1186
Content-type: text/html
Date: Tue, 16 Jan 2007 14:50:31 GMT
Last-Modified: Wed, 10 Jan 2007 09:58:26 GMT
Accept-Ranges: bytes
Connection: close

با این حال، روش تست و ارزیابی ذکر شده قدرت بالایی در تشخیص سرور ندارد و دقت آن محدود است چرا که روش‌هایی وجود دارند که اطلاعات بخش Server سرایند پاسخ HTTP را می‌توانند مخفی کرده یا حتی اطلاعات دیگری نشان دهند. به عنوان مثال، یکی از خروجی‌های احتمالی می‌تواند به شکل زیر باشد:

403 HTTP/1.1 Forbidden
Date: Mon, 16 Jun 2003 02:41: 27 GMT
Server: Unknown-Webserver/1.0
Connection: close
Content-Type: text/HTML; charset=iso-8859-1

در این حالت، اطلاعات بخش Server مخفی شده‌اند. بنابراین، ارزیاب امنیتی نمی‌تواند با تکیه بر این اطلاعات، نوع و نسخه سرور را تشخیص دهد.
رفتار پروتکلی

تکنیک‌های بهبود و ارتقاء یافته زیادی به ویژگی‌های وب سرورهای موجود در بازار دقت می‌کنند. در زیر روش‌هایی ذکر شده‌اند که به ارزیاب امنیتی اجازه می‌دهند تا نوع وب سرور مورد استفاده را تشخیص بدهد.
ترتیب فیلد سرایند HTTP

روش اول شامل مشاهده ترتیب سرایندهای متعدد در پاسخ HTTP است. هر سروری یک ترتیب داخلی سرایند دارد. به عنوان مثال، پاسخ‌های زیر را در نظر بگیرید:
سرور آپاچی نسخه 1.3.23

$ nc apache.example.com 80
HEAD / HTTP/1.0

HTTP/1.1 200 OK
Date: Sun, 15 Jun 2003 17:10: 49 GMT
Server: Apache/1.3.23
Last-Modified: Thu, 27 Feb 2003 03:48: 19 GMT
ETag: 32417-c4-3e5d8a83
Accept-Ranges: bytes
Content-Length: 196
Connection: close
Content-Type: text/HTML
سرور IIS مایکروسافت نسخه 5.0

$ nc iis.example.com 80
HEAD / HTTP/1.0

HTTP/1.1 200 OK
Server: Microsoft-IIS/5.0
Content-Location: http://iis.example.com/Default.htm
Date: Fri, 01 Jan 1999 20:13: 52 GMT
Content-Type: text/HTML
Accept-Ranges: bytes
Last-Modified: Fri, 01 Jan 1999 20:13: 52 GMT
ETag: W/e0d362a4c335be1: ae1
Content-Length: 133
سرور Netscape Enterprise نسخه 4.1

$ nc netscape.example.com 80
HEAD / HTTP/1.0

HTTP/1.1 200 OK
Server: Netscape-Enterprise/4.1
Date: Mon, 16 Jun 2003 06:01: 40 GMT
Content-type: text/HTML
Last-modified: Wed, 31 Jul 2002 15:37: 56 GMT
Content-length: 57
Accept-ranges: bytes
Connection: close
سرور SunONE نسخه 6.1

$ nc sunone.example.com 80
HEAD / HTTP/1.0

HTTP/1.1 200 OK
Server: Sun-ONE-Web-Server/6.1
Date: Tue, 16 Jan 2007 15:23:37 GMT
Content-length: 0
Content-type: text/html
Date: Tue, 16 Jan 2007 15:20:26 GMT
Last-Modified: Wed, 10 Jan 2007 09:58:26 GMT
Connection: close

اگر به دقت این پاسخ‌های بررسی کرده باشید، متوجه خواهید شد که ترتیب بخش Date و Server بین سرورهای آپاچی، Netscape Enterprise و IIS متفاوت است.
تست درخواست‌های ناقص و ناهنجار

یکی دیگر از روش‌های تست و ارزیابی، ارسال درخواست‌های ناقص و ناهنجار یا درخواست‌های صفحات نامعتبر و بی‌وجود به سرور است. پاسخ‌های HTTP زیر را در نظر بگیرید:
سرور آپاچی نسخه 1.3.23

$ nc apache.example.com 80
GET / HTTP/3.0

HTTP/1.1 400 Bad Request
Date: Sun, 15 Jun 2003 17:12: 37 GMT
Server: Apache/1.3.23
Connection: close
Transfer: chunked
Content-Type: text/HTML; charset=iso-8859-1
سرور IIS مایکروسافت نسخه 5.0

$ nc iis.example.com 80
GET / HTTP/3.0

HTTP/1.1 200 OK
Server: Microsoft-IIS/5.0
Content-Location: http://iis.example.com/Default.htm
Date: Fri, 01 Jan 1999 20:14: 02 GMT
Content-Type: text/HTML
Accept-Ranges: bytes
Last-Modified: Fri, 01 Jan 1999 20:14: 02 GMT
ETag: W/e0d362a4c335be1: ae1
Content-Length: 133
سرور Netscape Enterprise نسخه 4.1

$ nc netscape.example.com 80
GET / HTTP/3.0

HTTP/1.1 505 HTTP Version Not Supported
Server: Netscape-Enterprise/4.1
Date: Mon, 16 Jun 2003 06:04: 04 GMT
Content-length: 140
Content-type: text/HTML
Connection: close
سرور SunONE نسخه 6.1

$ nc sunone.example.com 80
GET / HTTP/3.0

HTTP/1.1 400 Bad request
Server: Sun-ONE-Web-Server/6.1
Date: Tue, 16 Jan 2007 15:25:00 GMT
Content-length: 0
Content-type: text/html
Connection: close

در مثال‌های فوق، شاهد پاسخ‌های گوناگون از سرورها هستیم. مشاهدات و ارزیابی‌های یکسانی نیز با ارسال درخواست‌های نامعتبر قابل انجام است. به عنوان مثال، پاسخ‌های زیر را در نظر بگیرید:
سرور آپاچی نسخه 1.3.23

$ nc apache.example.com 80
GET / JUNK/1.0

HTTP/1.1 200 OK
Date: Sun, 15 Jun 2003 17:17: 47 GMT
Server: Apache/1.3.23
Last-Modified: Thu, 27 Feb 2003 03:48: 19 GMT
ETag: 32417-c4-3e5d8a83
Accept-Ranges: bytes
Content-Length: 196
Connection: close
Content-Type: text/HTML
سرور IIS مایکروسافت نسخه 5.0

$ nc iis.example.com 80
GET / JUNK/1.0

HTTP/1.1 400 Bad Request
Server: Microsoft-IIS/5.0
Date: Fri, 01 Jan 1999 20:14: 34 GMT
Content-Type: text/HTML
Content-Length: 87
سرور Netscape Enterprise نسخه 4.1

$ nc netscape.example.com 80
GET / JUNK/1.0
سرور SunONE نسخه 6.1

$ nc sunone.example.com 80
GET / JUNK/1.0
ابزارها

• httprint – http://net-square.com/httprint.html
• httprecon – http://www.computec.ch/projekte/httprecon/
• Netcraft – http://www.netcraft.com
ارزیابی خودکار

در کنار اتکا به روش‌های دستی جمع آوری اطلاعات و بررسی سرایندهای وب سرور، ارزیاب امنیتی می‌تواند از ابزارهای خودکار برای دستیابی به نتایج یکسان و مشابه قبلی استفاده کند.

تست‌های متعددی وجود دارند که می‌توان با انجام آن‌ها به مشخصات دقیق وب سرور پی برد. خوشبختانه، ابزارهایی وجود دارند که این تست‌ها را به طور خودکار انجام می‌دهند. ابزار httprint یکی از آن ابزارها است. این ابزار از یک دیکشنری امضاء که اجازه شناسایی و تشخیص نوع و نسخه وب سرور را می‌دهد، استفاده می‌کند.

مثالی از محیط این ابزار در زیر آورده شده است.
OTG-INFO-002
تست آنلاین

اگر ارزیاب امنیتی بخواهد به طور نامحسوس عمل کرده و ارتباط مستقیمی با وب سایت هدف نداشته باشد، می‌تواند از ابزارهای آنلاین مانند Netcraft استفاده کند. با استفاده این ابزار می‌توانیم اطلاعاتی از قبیل سیستم عامل، نوع وب سرور، مدت زمان بالا بودن سرور، صاحب و مالک و تاریخچه تغییرات وب سرور و سیستم عامل جمع آوری کنیم. مثالی از این در زیر آورده شده است:
OTG-INFO-002

انتظار می‌رود پروژه OWASP Unmaskme یکی دیگر از ابزارهای آنلاین جمع آوری اطلاعات از وب سایت‌ها و تجزیه و تحلیل کلی متادیتاهای استخراج شده باشد. هدف اصلی این پروژه ایجاد فراهم نمودن امکاناتی برای مسئولان و صاحبان وب سایت است تا بتوانند از طریق آن‌ها متادیتاهای سایت را از دید امنیتی بررسی و ارزیابی کنند. 
