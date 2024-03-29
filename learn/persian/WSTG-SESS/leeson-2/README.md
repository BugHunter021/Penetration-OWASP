<div dir="rtl">
    
# بررسی Cookies Attributes
    

در این بخش از دوره آموزشی OWASP-WSTG به ششمین بخش از استاندارد **WSTG** با شناسه WSTG-SESS-02 می پردازیم که مربوط به بررسی Cookies Attributes می باشد.
    
## خلاصه

کوکی‌ها وب (‏که در اینجا کوکی‌ها نامیده می‌شوند) ‏اغلب یک بردار حمله کلیدی برای کاربران مخرب هستند (‏معمولا سایر کاربران را هدف قرار می‌دهند) ‏و برنامه باید همواره تلاش لازم را در جهت حفاظت از کوکی‌ها انجام دهد.
HTTP یک پروتکل بدون وضعیت یا Stateless است، به این معنی که هیچ reference ای برای درخواست‌های ارسال‌شده توسط همان کاربر ندارد. به منظور رفع این مشکل، Session ها ایجاد و به درخواست‌های HTTP اضافه شدند. مرورگرها حاوی تعداد زیادی از مکانیزم‌های ذخیره‌سازی هستند که در این بخش از راهنما، هر کدام از آن‌ها مورد بحث قرار می‌گیرند.

پر استفاده ترین مکانیسم ذخیره‌سازی در مرورگر ها ذخیره‌سازی کوکی است. کوکی‌ها را می‌توان توسط سرور، با گنجاندن یک هدر Set-Cookie در پاسخ HTTP یا با استفاده از JavaScript تنظیم کرد. کوکی‌ها می‌توانند به دلایل مختلفی مورد استفاده قرار گیرند، مانند:

* session management
* personalization
* tracking

به منظور ایمن سازی داده‌های کوکی، ابزارهایی را برای کمک به قفل کردن این کوکی‌ها و محدود کردن سطح حمله آن‌ها توسعه داده‌اند. با گذشت زمان کوکی‌ها به یک مکانیزم ذخیره‌سازی مطلوب برای برنامه‌های کاربردی وب تبدیل شده‌اند، زیرا آن‌ها انعطاف‌پذیری زیادی در استفاده و حفاظت از خود نشان می‌دهند.

ابزارهای حفاظت از کوکی‌ها عبارتند از:

* Cookie Attributes
* Cookie Prefixes
## اهداف تست

اطمینان حاصل کنید که پیکربندی امنیتی مناسب برای کوکی‌ها تنظیم شده‌باشد.
    
### چگونه تست را انجام دهیم

در زیر توضیح هر Attribute و Prefix مورد بحث قرار خواهد گرفت. تست نفوذگر باید تایید کند که آن‌ها به درستی توسط برنامه مورد استفاده قرار می‌گیرند. کوکی را می‌توان با استفاده از یک Intercepting Proxy یا با مراجعه به کوکی مرورگر بررسی کرد.
    
# بررسی Cookie Attributes

   ### آیتم Secure Attribute

مشخصه Secure به مرورگر می‌گوید که تنها در صورتی کوکی را ارسال کند که درخواست بر روی یک کانال امن مانند HTTPS ارسال شده‌باشد. این کار به محافظت از کوکی در برابر ارسال درخواست‌های رمزنگاری نشده کمک خواهد کرد. اگر برنامه را بتوان هم از طریق HTTP و هم از طریق HTTPS در دسترس قرار داد، یک مهاجم می‌تواند کاربر را هدایت کند تا کوکی خود را به عنوان بخشی از درخواست‌های محافظت نشده ارسال کند.

   ### آیتم HttpOnly Attribute

از مشخصه HttpOnly برای جلوگیری از حملاتی مانند افشای نشست (Session Leakage) استفاده می‌شود، زیرا اجازه دسترسی به کوکی از طریق یک اسکریپت سمت کلاینت مانند JavaScript را نمی‌دهد.

این امر کل سطح حمله حملات XSS را محدود نمی‌کند، زیرا مهاجم هنوز می‌تواند درخواست را به جای کاربر ارسال کند، اما دسترسی گسترده به بردارهای حمله XSS را محدود می‌کند.

### آیتم Domain Attribute

مشخصه Domain برای مقایسه دامنه کوکی در برابر دامنه سروری که درخواست HTTP برای آن انجام شده، استفاده می‌شود. اگر دامنه منطبق باشد یا اگر یک زیر دامنه باشد، آنگاه مشخصه Path در ادامه بررسی خواهد شد.

توجه داشته باشید که تنها میزبان‌هایی که به دامنه مشخص‌شده تعلق دارند می‌توانند یک کوکی را برای آن دامنه تنظیم کنند. علاوه بر این، مشخصه Domain نمی‌تواند یک دامنه سطح بالا باشد (‏.org یا .com). اگر مشخصه دامنه تنظیم نشده باشد، آنگاه نام میزبان سرور که کوکی را تولید می‌کند به عنوان مقدار پیش‌فرض دامنه استفاده می‌شود.

به عنوان مثال، اگر یک کوکی توسط یک برنامه در app.mydomain.com بدون مجموعه مشخصه Domain تنظیم شده‌باشد، آنگاه کوکی برای تمام درخواست‌های متعاقب برای app.mydomain.comو Subdomain های آن (مانند hacker.app.mydomain.com) ارسال خواهد شد، اما برای otherapp.mydomain.com این چنین نخواهد بود. اگر یک توسعه دهنده می‌خواست این محدودیت را از بین ببرد، می‌توانست مشخصه Domain را روی mydomain.com قرار دهد.

در این مورد، کوکی به تمام درخواست‌ها برای app.mydomain.com و Subdomain ‌های myDomain.com، مانند hacker.app.mydomain.com و حتی bank.mydomain.com فرستاده می‌شود. اگر یک سرور آسیب‌پذیر در یک Subdomain وجود داشته باشد (‏برای مثال، otherapp.mydomain.com)‏ و مشخصه Domain به درستی تنظیم نشده‌باشد (برای مثال، mydomain.com) در این صورت سرور آسیب‌پذیر می‌تواند برای برداشت کوکی‌ها (‏مانند session token ها)‏ در سراسر دامنه mydomain.com مورد سوء استفاده قرار گیرد.
    
### آیتم Path Attribute
    
مشخصه Path نقش مهمی در تنظیم دامنه کوکی‌های مرتبط با دامنه بازی می‌کند. علاوه بر دامنه، مسیر URL ای که کوکی برای آن معتبر است می‌تواند مشخص شود. اگر دامنه و مسیر منطبق بودند، سپس کوکی در درخواست ارسال خواهد شد. درست مانند مشخصه Domain، اگر مشخصه Path بیش از حد آزادانه تنظیم شده‌باشد، آن گاه می‌تواند برنامه را در برابر حملات توسط برنامه‌های دیگر بر روی همان سرور آسیب‌پذیر کند.

برای مثال، اگر مشخصه Path بر روی Web Server Root یا / تنظیم شده‌باشد، آنگاه کوکی‌های برنامه برای هر برنامه در یک دامنه یک‌سان ارسال خواهند شد (‏اگر برنامه‌های متعدد تحت همان سرور قرار گیرند)‏. چند مثال برای برنامه‌های چندگانه تحت یک سرور عبارتند از:

* path=/bank
* path=/private
* path=/docs
* path=/docs/admin
    
### آیتم Expires Attribute

مشخصه Expires برای موارد زیر استفاده می‌شود:

* تنظیم کوکی‌های پایدار (Persistent Cookies)
* محدود نمودن طول عمر کوکی اگر یک جلسه بیش از حد طول بکشد
* حذف کوکی به صورت اجباری با تنظیم تاریخ

بر خلاف کوکی‌های نشست (Session Cookies)، کوکی‌های مداوم (Persistent Cookies) توسط مرورگر استفاده خواهند شد تا زمانی که کوکی منقضی شود. هنگامی که تاریخ انقضا از زمان تعیین‌شده تجاوز کرد، مرورگر کوکی را حذف خواهد کرد.

### آیتم SameSite Attribute

مشخصه SameSite برای بیان این موضوع استفاده می‌شود که یک کوکی نباید همراه با درخواست‌های cross-site فرستاده شود. این ویژگی به سرور این امکان را می‌دهد تا ریسک نشت اطلاعات مربوط به cross-orgin را کاهش دهد.

در برخی موارد، از آن به عنوان یک استراتژی کاهش ریسک (‏یا دفاع در مکانیزم عمیق)‏ برای جلوگیری از حملات جعلی درخواست متقابل (Cross-Site Request Forgery) استفاده می‌شود. این ویژگی را می توان در سه حالت مختلف پیکربندی کرد:

* Strict
* Lax
* None

### مقدار Strict

مقدار Strict محدود کننده‌ترین استفاده از سایت SameSite است، که به مرورگر اجازه می‌دهد تا کوکی را تنها به زمینه first-party بدون پیمایش top-level ارسال کند. به عبارت دیگر، داده‌های مرتبط با کوکی تنها بر روی درخواست‌های منطبق با سایت فعلی نشان‌داده‌شده در نوار URL مرورگر ارسال خواهد شد. این کوکی به درخواست‌های ایجاد شده توسط وب سایت‌های third-party فرستاده نخواهد شد. این مقدار به ویژه برای اقدامات انجام شده در همان دامنه توصیه می‌شود.

با این حال، می‌تواند محدودیت‌هایی با برخی از سیستم‌های مدیریت نشست داشته باشد که به طور منفی بر تجربه ناوبری کاربر تاثیر می‌گذارد. از آنجایی که مرورگر در هیچ درخواستی که از دامنه یا ایمیل شخص ثالث ایجاد می‌شود، کوکی را ارسال نمی‌کند، کاربر باید دوباره وارد سیستم شود حتی اگر قبلاً یک جلسه تأیید اعتبار شده داشته باشد.

### مقدار Lax

کم‌تر از Strict محدود کننده است. اگر URL برابر با دامنه کوکی (‏first-party)‏ باشد، کوکی ارسال خواهد شد، حتی اگر لینک از دامنه third-party بیاید. این مقدار توسط اکثر مرورگرها رفتار پیش‌فرض در نظر گرفته می‌شود زیرا تجربه کاربر بهتری نسبت به مقدار Strict فراهم می‌کند. برای دارایی‌هایی مانند تصاویر، جایی که ممکن است برای دسترسی به آنها به کوکی‌ها نیاز نباشد، فعال نمی‌شود.
### مقدار None

مقدار None مشخص می‌کند که مرورگر کوکی را در درخواست‌هایcross-site (رفتار عادی قبل از اجرای SameSite) تنها در صورتی ارسال می‌کند که از ویژگی Secure نیز استفاده شود، به عنوان مثال: SameSite=None; Secure

این یک مقدار توصیه شده است، به جای اینکه هیچ مقدار SameSite را مشخص نکنید، زیرا استفاده از مشخصه Secure را Force می‌کند.

### آیتم Cookie Prefixes

کوکی‌های طراحی شده قابلیت تضمین یکپارچگی و محرمانه بودن اطلاعات ذخیره شده در آنها را ندارند. این محدودیت‌ها باعث می‌شود که سرور در مورد نحوه تنظیم ویژگی‌های یک کوکی در هنگام ایجاد اطمینان لازم را نداشته باشد. به منظور ارائه چنین ویژگی هایی به سرورها به روشی سازگار با عقب (Backwards-Compatible)، صنعت مفهوم Cookie Name Prefixes را برای تسهیل انتقال چنین جزئیاتی که به عنوان بخشی از نام کوکی تعبیه شده است، معرفی کرده است.

### مقدار Host Prefix

پیشوند __Host-انتظار دارد که کوکی‌ها شرایط زیر را برآورده کنند:

* کوکی باید با مشخصه Secure تنظیم شود.
* کوکی باید از یک URI که توسط user agent ایمن در نظر گرفته می‌شود، تنظیم شود.
* فقط برای میزبانی ارسال شود که کوکی را تنظیم می‌کند و نباید هیچ ویژگی Domain را در بر بگیرد.
* کوکی باید با مشخص Path با مقدار / تنظیم شود تا برای هر درخواست به میزبان ارسال شود.

به همین دلیل کوکی Set-Cookie: __Host-SID=12345; Secure; Path=/ پذیرفته می شود در حالی که هر یک از موارد زیر همیشه رد می شود:

```js
Set-Cookie: __Host-SID=12345 Set-Cookie: __Host-SID=12345; Secure
Set-Cookie: __Host-SID=12345; Domain=site.example
Set-Cookie: __Host-SID=12345; Domain=site.example; Path=/
Set-Cookie: __Host-SID=12345; Secure; Domain=site.example; Path=/
```

### مقدار  Secure Prefix

پیشوند __Secure- محدودیت کمتری دارد و می‌توان آن را با افزودن رشته حساس به حروف بزرگ و کوچک __Secure- به نام کوکی معرفی کرد.
    
انتظار می‌رود هر کوکی که منطبق با پیشوند __Secure- باشد شرایط زیر را برآورده سازد:

* کوکی باید با مشخصه Secure تنظیم شود.
* کوکی باید از یک URI که توسط user agent ایمن در نظر گرفته می‌شود، تنظیم شود.
    
### نمونه Strong Practices

براساس نیازهای کاربردی و نحوه عملکرد کوکی، ویژگی‌ها و پیشوندها باید اعمال شوند. هرچه کوکی بیشتر قفل شود، بهتر است.
    
با کنار هم قرار دادن همه این موارد، می‌توانیم امن‌ترین پیکربندی مشخصه کوکی را به صورت زیر تعریف کنیم:

Set-Cookie: __Host-SID=session token; path=/; Secure; HttpOnly; SameSite=Strict.
