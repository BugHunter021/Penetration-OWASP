# بررسی Vulnerable Remember Password

در این بخش از دوره آموزشی OWASP-WSTG به چهارمین بخش از استاندارد WSTG با شناسه WSTG-ATHN-05 می پردازیم که مربوط به بررسی Vulnerable Remember Password می باشد.

### خلاصه

آیتم Credentialها، گسترده‌ترین تکنولوژی احراز هویت مورد استفاده هستند. با توجه به چنین کاربرد گسترده‌ای از جفت‌های نام کاربری – رمز عبور، کاربران دیگر قادر به مدیریت صحیح اعتبارنامه‌های خود در میان بسیاری از برنامه‌های کاربردی مورد استفاده نیستند.

به منظور کمک به کاربران در credential های خود، فن‌آوری‌های متعددی ظاهر شدند:

برنامه‌های کاربردی یک عملکرد remember me را فراهم می‌کنند که به کاربر اجازه می‌دهد تا برای مدت طولانی، بدون درخواست دوباره از وی برای اعتبار سنجی خود، معتبر باقی بماند.

مورد بعدی استفاده Password Managers از جمله Password Manager های مرورگر است که به کاربر اجازه می‌دهند تا اعتبارات خود را به شیوه‌ای امن ذخیره کند و بعدا بدون دخالت کاربر آن‌ها را در فرم‌های کاربری تزریق کند.

### اهداف تست

اعتبار سنجی اینکه جلسه ایجاد شده به طور ایمن مدیریت می‌شود و اعتبار کاربر را در معرض خطر قرار نمی‌دهد.

### چگونه تست را انجام دهیم

از آنجا که این روش‌ها تجربه کاربری بهتری را فراهم می‌کنند و به کاربر اجازه می‌دهند تا تمام credential های خود را فراموش کند، سطح حمله را افزایش می‌دهند.

برخی برنامه‌ها اطلاعات کاربری را به صورت رمزگذاری شده در مکانیزم های ذخیره سازی مرورگر ذخیره می کنند، که با دنبال کردن [Web Storage Testing Scenario](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage) و گذر از سناریوهای [Session Analysis](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema#session-analysis)، می‌توانید اعتبار را تأیید کنید.

این Credential ها نباید به هیچ وجه در برنامه سمت مشتری ذخیره شوند و باید توسط نشانه‌های تولید شده در سمت سرور جایگزین شود.

برخی از برنامه‌ها به طور خودکار Credential های کاربر را تزریق می کنند که می‌تواند توسط حملات [ClickJacking](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking) و [CSRF](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery) مورد سوء استفاده قرار گیرد.

توکن‌ها باید از نظر طول عمر توکن تحلیل شوند، زیرا برخی توکن‌ها هرگز منقضی نمی‌شوند و اگر آن توکن‌ها به سرقت بروند کاربران را در معرض خطر قرار می‌دهند. در این بخش می بایست اطمینان حاصل کنید که سناریوی تست Session Timeout را انجام داده اید.

### روش Remediation

اقدامات خوب [مدیریت نشست](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html) را دنبال کنید.

اطمینان حاصل کنید که هیچ Credential ای به صورت Clear Text ذخیره نشده و یا به راحتی در فرم‌های Encode یا Encrypt شده در مکانیزم‌های ذخیره‌سازی مرورگر قابل بازیابی نیستند؛ آن‌ها باید در سمت سرور ذخیره شوند
و از [شیوه‌های ذخیره‌سازی رمز عبور](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html خوبی پیروی کنند.

