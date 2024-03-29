# بررسی Subdomain Takeover

در این بخش از دوره آموزشی OWASP-WSTG به دومین بخش از استاندارد **WSTG** با شناسه WSTG-CONFIG-010 می پردازیم که مربوط به بررسی Subdomain Takeover می باشد.

### خلاصه

بهره‌برداری موفق از این نوع آسیب‌پذیری به نفوذگر این امکان را می‌دهد که کنترل زیردامنه قربانی را در دست بگیرد. این حمله به موارد زیر بستگی دارد:

* رکورد مربوط به زیردامنه ثبت شده در سرور DNS قربانی، پیکربندی شده باشد تا به یک منبع غیر موجود یا غیرفعال، سرور خارجی و یک Endpoint وصل شود. استفاده از محصولات XaaS ‏ (Anything as a Service) و خدمات ابری عمومی، اهداف بالقوه زیادی را برای این حمله ارائه می‌دهند.

* ارائه‌دهنده سرویس میزبانی / سرویس خارجی / نقطه پایانی به درستی به بررسی مالکیت زیردامنه رسیدگی نمی‌کند.

اگر تصاحب زیردامنه موفق باشد، طیف گسترده‌ای از حملات مانند ارائه محتوای مخرب، فیشینگ، سرقت کوکی‌های کاربر، Credentialها و غیره، ممکن خواهد شد.

این آسیب‌پذیری می‌تواند برای طیف گسترده‌ای از رکوردهای DNS شامل A، CNAME، MX، NS،TXT و غیره مورد استفاده قرار گیرد.

از نظر شدت حمله، یک تصاحب زیردامنه NS (‏اگر چه احتمال کمتری دارد)‏ بالاترین تاثیر را دارد زیرا یک حمله در این بخش، موفق می‌تواند منجر به کنترل کامل بر منطقه DNS و دامنه قربانی شود.

### دامنه GitHub

قربانی (victim.com) از Github برای توسعه استفاده می‌کند و یک رکورد DNS (coderepo.victim.com) را برای دسترسی به آن پیکربندی می‌کند.

قربانی تصمیم می‌گیرد که مخزن کد خود را از Github به یک پلت فرم تجاری منتقل کند و coderepo.victim.com را از سرور DNS خود حذف نمی‌کند.

یک نفوذگر متوجه می‌شود که coderepo.victim.com در Github میزبانی می‌شود و از GitHub Pages برای بدست آوردن coderepo.victim.com با استفاده از حساب Github خود استفاده می‌کند.

### وضعیت Expired Domain

قربانی (victim.com)صاحب دامنه دیگری است (victimotherdomain.com) ‏ ‏و از یک رکورد CNAME ‏(www) ‏برای ارجاع به دامنه دیگر استفاده می‌کند.

‏www.victim.com => victimotherdomain.com

در برخی موارد،victimotherdomain.com منقضی می‌شود و برای ثبت‌نام توسط هر کسی در دسترس است.

از آنجا که رکورد CNAME از DNS Zone مربوط به victim.com حذف نشده است، هر کسی کهvictimotherdomain.com را ثبت نماید تا زمانی که رکورد DNS موجود می باشد، دسترسی کامل را بر روی www.victim.com خواهد داشت.
# اهداف تست

* تمام دامنه های ممکن (‏قبلی و فعلی) ‏را شناسایی (Enumerate) نمایید.
* دامنه های فراموش‌شده یا پیکربندی نشده را شناسایی کنید.

### چگونه تست را انجام دهیم
#### تست جعبه سیاه

اولین قدم شناسایی سرورهای DNS قربانی و رکوردهای منبع (Resource Records) آن است.

روش‌های متعددی برای انجام این کار وجود دارد، برای مثال شناسایی رکوردهای DNS با استفاده از یک لیست از دیکشنری زیردامنه‌های عمومی و پرکاربرد، Brute Force دامنه DNS یا استفاده از موتورهای جستجوی وب و دیگر منابع داده OSINT، مواردی هستند که به منظور شناسایی رکوردهای DNS می‌توان از آن‌ها استفاده نمود.

روش دیگر استفاده از دستور dig می باشد. با استفاده از دستور dig، تست نفوذگر به دنبال پیغام‌های پاسخ سرور DNS زیر است که پس از آن بررسی بیشتر را آغاز کند:

* NXDOMAIN
* SERVFAIL
* REFUSED
* no servers could be reached.
* Testing DNS A, CNAME Record Subdomain Takeover

در این بخش ابتدا یک DNS Enumeration اولیه در مورد دامنه قربانی با استفاده از dnsrecon انجام می‌گردد:

در ادامه باید بررسی شود که کدام رکوردهای DNS اصطلاحا مرده هستند و به خدمات غیر فعال یا استفاده نشده اشاره دارد. استفاده از دستور dig برای رکورد CNAME :

پاسخ‌های DNS که NXDOMAIN در آن‌ها مشاهده می‌شود، به تحقیقات بیشتری را نیاز دارند.

برای تست یک رکورد A، تست نفوذگر یک جستجوی پایگاه‌داده whois را اجرا می‌کند و Github را به عنوان ارائه‌دهنده سرویس شناسایی می‌کند:

سپس تست نفوذگر از subdomain.victim.com بازدید می‌کند. ، یا درخواست HTTP GET را برای آن ارسال می‌نماید که پاسخ “404 – File not found” برگشت داده می‌شود و این نشانه وجود آسیب‌پذیری می‌باشد.

تست نفوذگر با استفاده از GitHub Pages، دامنه را تقاضا می‌کند:
Testing NS Record Subdomain Takeover

در این بخش می‌بایست تمام nameserver های دامنه مورد نظر را شناسایی نمایید:

در این مثال ساختگی، تست نفوذگر بررسی می‌کند که آیا دامنه expireddomain.com فعال است یا خیر. در صورتی که دامنه در دسترس باشد، زیردامنه آسیب پذیر خواهد بود.

پاسخ DNS زیر، تحقیقات بیشتری را طلب می‌کند:

* SERVFAIL
* REFUSED

### تست جعبه خاکستری

در این حالت تست نفوذگر اطلاعات مربوط به DNS Zone را در اختیار دارد و این بدان معنی است که به DNS Enumeration نیازی نیست. روش تست نیز در این حالت مشابه حالت پیشین است.

### روش اصلاح Remediation

برای کاهش خطر تصاحب زیر دامنه، رکورد(های) آسیب پذیر DNS باید از DNS Zone حذف شوند. همچنین نظارت مستمر و بررسی‌های دوره‌ای به عنوان بهترین روش توصیه می‌شود.

# ابزارها

* dig – man page
* recon-ng – Web Reconnaissance framework
* theHarvester – OSINT intelligence gathering tool
* Sublist3r – OSINT subdomain enumeration tool
* dnsrecon – DNS Enumeration Script
* OWASP Amass DNS enumeration
