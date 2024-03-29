# نگاشت و رسم نقشه معماری اپلیکیشن

در این بخش از دوره آموزشی OWASP-OTG به اولین بخش از استاندارد OTG با شناسه OTG-INFO-010 می پردازیم که مربوط به نگاشت و رسم نقشه معماری اپلیکیشن می باشد.

### خلاصه

زیرساخت پیچیده اجزای داخلی و نامتقارن وب سرور می‌تواند شامل چندین نوع وب اپلیکیشن باشد و برای همین منظور، مدیریت و بازبینی پیکربندی یک گام مهم و اساسی در اجرا و تست هر اپلیکیشن تلقی می‌شود. در واقع، تنها یک آسیب پذیری می‌تواند امنیت کل زیرساخت را زیر سوال ببرد و حتی مشکلات ریز و غیر ضروری می‌توانند باعث ایجاد ریسک‌های بزرگ برای سایر اپلیکیشن‌ها در سرور شوند.

برای حل این مشکلات، بازبینی عمق پیکربندی و مشکلات امنیتی از اهمیت بسزایی برخوردار هستند. قبل از انجام هر گونه بازبینی، نگاشت و رسم نقشه شبکه و معماری اپلیکیشن امری ضروری است. المان‌های متعددی که زیرساخت را شکل می‌دهند باید مشخص شوند تا بتوان نحوه تعاملات آن‌ها با وب اپلیکیشن و تاثیر آن‌ها روی امنیت را درک کرد.

### چگونه تست کنیم؟

### نگاشت و رسم نقشه معماری اپلیکیشن

معماری اپلیکیشن باید از طریق تست‌های گوناگون و شناسایی اجزای تشکیل دهنده آن رسم شود. در نصب‌های کوچک و اولیه، مانند یک اپلیکیشن ساده مبتنی بر CGI، به یک سرور برای پیاده‌سازی وب سرور که اپلیکیشن‌های به زبان C، Perl یا Shell CGI و حتی مکانیم احراز هویت را اجرا می‌کند ، نیاز است.

در نصب‌های پیچیده‌تر، مانند سیستم بانکی آنلاین، چندین سرور دخیل هستند. این سرورها می‌توانند شامل سرورهای پراکسی معکوس، وب سرور، سرور اپلیکیشن، سرور دیتابیس یا سرور LDAP باشند. هر کدام از این سرورها برای مقاصد مختلفی مورد استفاده قرار می‌گیرند و حتی ممکن است در شبکه‌های مختلف که بین آن‌ها دیوار آتش مجزا نصب شده، قرار گرفته باشند. با این کار، DMZهای ایزوله مختلفی ساخته می‌شوند و به همین علت، تهدیدات یک DMZ نیز ایزوله می‌شوند و سایر DMZها و حتی کل معماری را تهدید نمی‌کنند.

در صورتیکه توسعه دهندگان اپلیکیشن، در قالب مستندات یا مذاکرات حضوری، اطلاعات معماری شبکه را به تیم تست نفوذ بدهند، کار ترسیم شبکه و تست نفوذ آسان خواهد بود. اما اگر قرار باشد که تست نفوذ ار نوع جعبه سیاه و کور باشد، این کار به مراتب سخت‌تر خواهد بود.

در حالت دوم، ارزیاب امنیتی با یک تصور ساده که با تنها با یک سرور رو به رو است، کار خود را شروع می‌کند. سپس، اطلاعات بیشتری را کسب کرده، المان‌های متعددی را استخراج می‌کنند و نقشه معماری را گسترش می‌دهند.

ارزیاب امنیتی از خود سوال‌های متعددی می‌پرسد.

اولین سوال این خواهد که بود که آیا سیستم دیوار آتشی برای محافظت از وب سرور وجود دارد؟ به این سوال از طریق اسکن شبکه و بررسی پورت‌های فیلتر شده در لبه شبکه جواب می‌دهد. پاسخ این سوال را می‌توان با تست بسته‌های شبکه و شناسایی نوع دیوار آتش استفاده شده، بهینه‌تر کرد.

آیا دیوار آتش حالتمند است یا روی روتر یک فیلتر دسترسی وجود دارد؟ چگونه پیکربندی و تنظیم شده است؟ آیا می‌توان آن را دور زد؟

تشخیص یک پراکسی معکوس در جلوی وب سرور نیازمند بررسی بنرهای سرور است. حتی می‌توان پاسخ‌های وب سرور برای درخواست‌ها را با پاسخ‌های مورد انتظار مقایسه کرده و پراکسی معکوس را شناسایی کنیم.

به عنوان مثال، بعضی از پراکسی‌های معکوس به عنوان سیستم‌های حفاظت و جلوگیری از نفوذ عمل کرده و حملات احتمالی روی وب سرور را مسدود می‌کنند. اگر گمان می‌رود که وب سرور باید برای درخواست صفحه نامعتبر باید پیام 404 را بازگرداند، اما در عمل، برای حملات تحت وب یک کد و پیام دیگری را برمی‌گرداند، می‌توان حدس زد که سایت توسط یک پراکسی معکوس یا دیوار آتش که درخواست‌ها را فیلتر کرده و پیام‌های خطای جدا از حالت پیش فرض را برمی‌گرداند، محافظت می‌شود.

به عنوان مثال دیگر، اگر وب سرور مجموعه‌ای از متدهای موجود HTTP اعم از TRACE را برمی‌گرداند، اما متدهای مورد انتظار پیام خطا بازمی‌گردانند، احتمال وجود چیزی ما بین سرور و کلاینت وجود دارد که آن‌ها را مسدود می‌کند.

گاهی اوقات، سیستم محافظ خود را لو می‌دهد:

![alt text](https://github.com/BugHunter021/penetration-test/blob/main/learn/persian/OTG-INFO/lesson-10/images/owasp-030.jpg)


پراکسی‌های معکوس می‌توانند تحت عنوان پراکسی‌های حافظه نهان معرفی شوند تا عملکرد و کارایی اپلیکیشن‌های سمت سرور را ارتقا دهند. می‌توان با بررسی سرایند سرور، این پراکسی‌ها را شناسایی کرد. حتی می‌توان با استفاده از درخواست‌های زمان‌بندی شده و مقایسه زمان ذخیره درخواست‌ها روی سرور، به وجود این نوع پراکسی پی برد.

المان دیگری که می‌توان آن‌ را تشخیص داد، سیستم توزیع بار ترافیکی شبکه است. غالباً، این سیستم‌ها بار ترافیکی روی یک پورت مشخص را با استفاده از الگوریتم‌های مختلف، روی سیستم‌های متعدد توزیع می‌کنند. بنابراین، برای شناسایی چنین سیستمی باید چندین درخواست را بررسی و نتایج آن‌ها را مقایسه کرد تا بتوان تشخیص داد که درخواست‌ها به سمت چند سرور می‌روند. به عنوان مثال، با توجه به سرایند Date، اگر ساعت‌های سرورها هماهنگ نشده باشند، می‌توان سیستم توزیع بار را تشخیص داد.

گاهی اوقات، سیستم توزیع بار ترافیکی مانند Nortel’s Alteon WebSystems، اطلاعات جدیدی در سرایندها تزریق می‌کنند و وجود خود را در سرایند HTTP مشخص می‌کنند.

شناسایی وب سرورهای اپلیکیشن معمولاً کار آسانی است. درخواست‌ها برای منابع مختلف توسط سرور اپلیکیشن اداره می‌شود و سرایند پاسخ به طور چشم‌گیری تفاوت خواهد کرد. راه دیگر برای شناسایی آن‌ها، بررسی کوکی تنظیم شده معرف وب سرور اپلیکیشن، مانند JSESSIONID، توسط وب سرور، مانند J2EE، است.

تشخیص سرورهای احراز هویت مانند دایرکتوری‌های LDAP، دیتابیس‌های رابطه‌ای یا سرورهای RADIUS، کار آسانی نیست چرا که این سرورها توسط خود اپلیکیشن مخفی می‌شوند.

دیتابیس‌های سمت سرور را می‌توان با یک بررسی ساده اپلیکیشن شناسایی کرد. اگر داده‌های پویایی وجود دارند که هر لحظه تولید می‌شوند، احتمال خیلی زیاد از جایی مانند یک دیتابیس نشات می‌گیرند. گاهی اوقات، نحوه درخواست اطلاعات می‌تواند به وجود یک دیتابیس سمت سرور اشاره کند.

به عنوان مثال، اپلیکیشن‌ فروشگاهی آنلاین که از شاخص‌های عددی زیادی هنگام جست و جوی موارد مختلف، استفاده می‌کنند، یک دیتابیس سمت سرور برای این منظور دارند. با این حال، هنگام تست کور اپلیکیشن، اطلاعات دیتابیس زمانی در دسترس خواهد بود که بنا به دلایلی مانند مدیریت ضعیف خطاها یا باگ SQLi، دیتابیس آسیب‌پذیر باشد.
