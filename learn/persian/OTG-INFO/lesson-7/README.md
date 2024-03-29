# شناسایی مسیرهای اجرایی و ساختار اپلیکیشن

در این بخش از دوره آموزشی OWASP-OTG به اولین بخش از استاندارد OTG با شناسه OTG-INFO-007 می پردازیم که مربوط به شناسایی مسیرهای اجرایی و ساختار اپلیکیشن می باشد.

# خلاصه

قبل از انجام تست OTG-INFO-007 ، شناخت و درک ساختار اپلیکیشن یک امر مهم و حیاتی محسوب می‌شود. بدون درک عمیق از ساختار و طرح اپلیکیشن، یک تست ضعیف و ناموفق خواهیم داشت.
# اهداف تست و ارزیابی

هدف از تست OTG-INFO-007 و ارزیابی، بررسی اپلیکیشن و درک نحوه کارکرد و جریان کاری اساسی آن است.

### چگونه تست کنیم؟

در تست جعبه سیاه، ارزیابی همه کد اپلیکیشن کار سخت و دشوار است. نه تنها ارزیاب امنیتی هیچ گونه دید و اطلاعاتی از کد و مسیرهای اپلیکیشن ندارد، بلکه، حتی در صورت مطلع بودن از کدها و مسیرها، تست همه کدها و مسیرها یک فرایند بسیار زمان بر و طاقت فرسایی خواهد بود. چند رویکرد برای تست و ارزیابی کد داریم:

* مسیر: تمامی مسیرهای اپلیکیشن که شامل آنالیز مقادیر مرزی و ترکیبی برای هر مسیر تصمیم‌گیری است را تست کنید. از آنجایی که دقت این رویکرد بسیار بالا است، تعداد مسیرهای قابل تست با هر هر شاخه تصمیم‌گیری افزایش می‌یابد.
* جریان کاری: مقداردهی متغیرها را از طریق تعامالات خارجی مانند کاربران تست می‌کند. این رویکرد بر روی جریان کاری، تبدیل و کاربرد داده در اپلیکیشن تمرکز دارد.
* مسابقه: چندین نمونه همزمان و متقارن اپلیکیشن که یک داده را دستکاری می‌کنند، تست می‌کند.

### آشنایی با otg-info-006

لازم است برای انتخاب نوع متد و میزان استفاده از آن متد با صاحب اپلیکیشن مذاکراتی داشته باشید. برای این منظور می‌توانید از رویکردهای ساده‌ای استفاده کنید. به عنوان مثال، می‌توانید از صاحب اپلیکیشن سوال کنید که کدام توابع یا بخش کد برای آن‌ها مهم هستند.
### تست جعبه سیاه

برای نمایش دامنه پوشش کد به صاحب اپلیکیشن، ارزیاب امنیتی می‌تواند کار خود را با یک برگه یادداشت شروع کرده و تمامی لینک‌های استخراجی توسط اسپایدر را یادداشت کند. سپس، ارزیاب امنیتی می‌تواند نقاط تصمیم‌گیری را با دقت بررسی کرده و مسیرهای کد حساس و مهم را شناسایی کند. موارد یافت شده باید با ذکر URL، سند اثبات و اسکرین شات مسیرهای یافت‌ شده، مستند بشوند.
### تست جعبه خاکستری / سفید

کار ارزیابی امنیتی در تست OTG-INFO-007 به مراتب آسان‌تر خواهد بود چرا که اطلاعاتی از طرف صاحب اپلیکیشن از قبل در اختیار ارزیاب امنیتی گذاشته شده است.

### مثال

#### اسپایدرینگ خودکار

اسپایدر خودکار ابزاری است که برای یافتن منابع روی وب سایت مورد استفاده قرار می‌گیرد. این ابزار کار خود را با در دست داشتن لیستی از URLهای اولیه به نام دانه شروع می‌کند. ابزارهای اسپایدرینگ زیادی وجود دارند، با این حال، در زیر مثالی از ZAP آورده شده است:

ابزار ZAP ویژگی‌های اسپایدرینگ زیر را دارد و ارزیاب امنیتی می‌تواند بر حسب نیازش، آن‌ها را انتخاب کند:

* بخش Spider Site: لیست دانه شامل تمامی آدرس‌های یافت شده برای سایت انتخابی است.
* بخش Spider Subtree: لیست دانه شامل تمامی آدرس‌های یافت شده است و در زیر زیرشاخه نود انتخابی نمایش داده می‌شود.
* بخش Spider URL: لیست دانه فقط شامل URI مربوط به نود انتخاب شده است.
* بخش Spider all in Scope: لیست دانه شامل تمامی URIهایی که کاربر به عنوان in Scope انتخاب کرده است، می‌باشد.

# ابزارها

* ابزار Zed Attack Proxy (ZAP)
* ابزار یادداشت برداری
* ابزار رسم دیاگرام
