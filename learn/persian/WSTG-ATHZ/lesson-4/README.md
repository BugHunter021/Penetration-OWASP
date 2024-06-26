# بررسی Insecure Direct Object References

در این بخش از دوره آموزشی OWASP-WSTG به پنجمین بخش از استاندارد WSTG با شناسه WSTG-ATHZ-04 می پردازیم که مربوط به بررسی Insecure Direct Object References می باشد.

### خلاصه

آسیب‌پذیری Insecure Direct Object References یا IDOR‏ زمانی رخ می‌دهد که یک برنامه دسترسی مستقیم به اشیا را براساس ورودی فراهم‌شده توسط کاربر انجام می‌دهد. در نتیجه نفوذگر با این آسیب‌پذیری می‌تواند Authorization را Bypass نموده و به طور مستقیم به منابع در سیستم (برای مثال رکوردها یا فایل‌های پایگاه‌داده) دسترسی داشته باشند.

روش  IDOR به مهاجمان این امکان را می‌دهد که با تغییر مقدار پارامتری که به طور مستقیم به یک شی اشاره می‌کند، Authorization را دور زده و به منابع دسترسی داشته باشند. چنین منابعی می‌توانند ورودی‌های پایگاه‌داده متعلق به سایر کاربران، فایل‌های موجود در سیستم و غیره باشند. این امر ناشی از این واقعیت است که برنامه ورودی تامین‌شده توسط کاربر را می‌گیرد و از آن برای بازیابی یک شی بدون انجام بررسی‌های اعتبارسنجی کافی استفاده می‌کند.

### اهداف تست

* نقاطی را که ممکن است در آن‌ها ارجاء به اشیا رخ دهند، شناسایی کنید.
* اقدامات کنترل دسترسی را ارزیابی کنید و بررسی کنید که آیا آن‌ها در برابر IDOR آسیب‌پذیر هستند یا خیر.

### چگونه تست را انجام دهیم

برای آزمایش این آسیب‌پذیری، تست نفوذگر ابتدا باید تمام مکان‌ها را در برنامه‌ای که در آن ورودی کاربر برای ارجاع مستقیم اشیا استفاده می‌شود، شناسایی کند. برای مثال، مکان‌هایی که در آن‌ها ورودی کاربر برای دسترسی به یک ردیف پایگاه‌داده، یک فایل، صفحات برنامه و غیره استفاده می‌شود. سپس باید مقدار پارامتر مورد استفاده برای ارجاء اشیا را تغییر دهد و ارزیابی کند که آیا بازیابی اشیا متعلق به کاربران دیگر امکان پذیر است یا خیر.

بهترین راه برای آزمایش منابع هدف مستقیم داشتن حداقل دو کاربر (‏اغلب بیشتر)‏ برای پوشش دادن اشیا و توابع مختلف متعلق به خود است. برای مثال دو کاربر که هر کدام به اشیا مختلف دسترسی دارند (‏مانند اطلاعات خرید، پیام‌های خصوصی و غیره)‏ و ‏ کاربران با امتیازات مختلف (‏برای مثال کاربران مدیر)‏ برای دیدن اینکه آیا ارجاعات مستقیمی به عملکرد برنامه وجود دارد یا خیر.

با داشتن چندین کاربر، دیگر نیازی به حدس زدن مقادیر توسط تست نفوذگر نبوده و در زمان تست صرفه‌جویی می‌شود.

در زیر چند سناریوی معمول برای این آسیب‌پذیری و روش‌های تست هر کدام آورده شده‌است:

### روش The Value of a Parameter Is Used Directly to Retrieve a Database Record

درخواست نمونه:
```url
foo.bar/somepage?invoice=12345
```

در این مورد، مقدار پارامتر invoice به عنوان یک شاخص در جدول فاکتورهای پایگاه‌داده استفاده می‌شود. این برنامه مقدار این پارامتر را می‌گیرد و از آن در یک پرس و جو به پایگاه‌داده استفاده می‌کند. سپس برنامه اطلاعات فاکتور را به کاربر بر می‌گرداند.

از آنجا که مقدار فاکتور به طور مستقیم وارد پرس و جو می‌شود، با تغییر مقدار پارامتر، بازیابی هر شی فاکتور بدون توجه به کاربر که فاکتور به او تعلق دارد امکان پذیر است.

برای تست این مورد، تست نفوذگر باید شناسه یک فاکتور متعلق به یک کاربر آزمایشی متفاوت را به دست آورد (‏اطمینان حاصل کند که قرار نیست این اطلاعات را در هر منطق کسب‌وکار برنامه مشاهده کند)‏ و سپس بررسی کند که آیا دسترسی به اشیا بدون مجوز امکان پذیر است یا خیر.

### روش The Value of a Parameter Is Used Directly to Perform an Operation in the System

درخواست نمونه:
```url
foo.bar/changepassword?user=someuser
```
در این مورد، مقدار پارامتر user به برنامه می‌گوید که کدام کاربر قصد تغییر کلمه عبور خود را دارد. در بسیاری از موارد این مرحله بخشی از یک Wizard یا یک عملیات چند مرحله‌ای خواهد بود. در مرحله اول، برنامه درخواستی را دریافت خواهد کرد که بیان می‌کند رمز عبور برای کدام کاربر باید تغییر کند و در مرحله بعد، کاربر یک رمز عبور جدید (‏بدون درخواست برای رمز عبور فعلی)‏ را ارائه خواهد کرد.

پارامتر user برای ارجاع مستقیم شی کاربر که عملیات تغییر رمز برای آن انجام خواهد شد، استفاده می‌شود. برای آزمایش این مورد، تست نفوذگر باید تلاش کند تا نام کاربری آزمایشی متفاوتی را نسبت به نام کاربری که در حال حاضر وارد سیستم شده‌است، ارایه دهد و بررسی کند که آیا امکان تغییر رمز عبور کاربر دیگری وجود دارد یا خیر.

### روش The Value of a Parameter Is Used Directly to Retrieve a File System Resource

درخواست نمونه:
```url
foo.bar/showImage?img=img00011
```

در این مورد، مقدار پارامتر فایل برای بیان اینکه کاربر قصد بازیابی چه فایلی را دارد استفاده می‌شود. مهاجم با ارائه نام یا شناسه یک فایل متفاوت (برای مثال file=image00012.jpg) قادر به بازیابی اشیا متعلق به کاربران دیگر خواهد بود.
برای آزمایش این مورد، تست نفوذگر باید مرجعی را به دست آورد که در حالت عادی امکان دسترسی به آن با کاربر جاری وجود ندارد و یا متعلق به کاربر دیگر است.

نکته: این آسیب‌پذیری اغلب همراه با یک آسیب‌پذیری پیمایش مسیر / دایرکتوری (Directory/Path Traversal) مورد بهره‌برداری قرار می‌گیرد.

### روش The Value of a Parameter Is Used Directly to Access Application Functionality

درخواست نمونه:
```url
foo.bar/accessPage?menuitem=12
```

در این مورد، مقدار پارامتر menuitem برای نشان دادن این موضوع به برنامه استفاده می‌شود که کدام آیتم منو (‏و بنابراین کدام کارکرد برنامه)‏ برای کاربر جاری در حال درخواست است. فرض کنید که کاربر قرار است محدود شود و بنابراین لینک‌هایی دارد که تنها برای دسترسی به آیتم‌های منو ۱، ۲ و ۳ در دسترس هستند.

برای تست این مورد، تست نفوذگر مکانی را شناسایی می‌کند که در آن عملکرد برنامه با ارجاع به آیتم منو تعیین می‌شود، مقادیر آیتم‌های منو را که کاربر تست می‌تواند به آن‌ها دسترسی داشته باشد، شناسایی می‌کند و سپس سایر آیتم‌های منو را امتحان می‌کند.

در مثال‌های بالا تغییر یک پارامتر به تنهایی کافی است. با این حال، گاهی اوقات ممکن است مرجع شی بین بیش از یک پارامتر تقسیم شود و آزمایش باید براساس آن تنظیم گردد.

# مراجع

[Top 10 2013-A4-Insecure Direct Object References](https://owasp.org/www-project-top-ten/2017/Release_Notes)

