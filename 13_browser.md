# جاوااسکریپت و مرورگر

{{quote {author: "Tim Berners-Lee", title: "The World Wide Web: A very short personal history", chapter: true}

The dream behind the Web is of a common information space in which we
communicate by sharing information. Its universality is essential: the
fact that a hypertext link can point to anything, be it personal,
local or global, be it draft or highly polished.

quote}}

{{index "Berners-Lee, Tim", "World Wide Web", HTTP, [JavaScript, "history of"], "World Wide Web"}}

{{figure {url: "img/chapter_picture_13.jpg", alt: "Picture of a telephone switchboard", chapter: "framed"}}}

فصل‌های آینده‌ی این کتاب به مرورگرهای وب خواهد پرداخت. بدون مرورگرهای وب،
جاوااسکریپتی هم￼ وجود نخواهد داشت. یا حتی اگر هم می بود، هیچ کسی توجهی به آن
نمی‌کرد.

{{index decentralization, compatibility}}

از همان ابتدا فناوری وب غیر متمرکز بوده است، نه فقط از لحاظ فنی بلکه روش رشد آن هم
این گونه بوده است. ارائه‌دهندگان مختلف مرورگر، قابلیت‌های اختصاصی و موردی جدیدی را
که گهگاه به روش‌های نسنجیده صورت گرفته است، اضافه کرده اند، که بعضا مورد استفاده دیگران قرار گرفته – و در نهایت به عنوان یک استاندارد در
آمده است.

این اتفاق هم خوش یمن و هم مشکل ساز بوده است. از یک سو، این که یک فقط گروه مرکزی
کنترل یک سیستم را نداشته باشند باعث رشد و ارتقای آن می شود اما بهبود سیستم توسط
گروه‌های مختلف که همکاری خوبی باهم ندارند (یا گاهی اوقات به روشنی خصومت هم
دارند) هم بدی‌های خودش را دارد. از سویی دیگر، نمی‌توان انتظار یک سیستم مستحکم را
با روش بی حساب و کتابی که وب در آن توسعه یافت داشت. بعضی از قسمت‌های آن کاملا
گیج‌کننده است و به‌خوبی درک نمی‌شوند.

## شبکه ها و اینترنت

شبکه‌های کامپیوتری از دهه‌ی پنجاه میلادی (1950s) به وجود آمده اند. اگر دو یا چند
کامپیوتر را با کابل به هم متصل کنید و اجازه بدهید که داده‌ها بین این کابل‌ها ارسال
و دریافت شوند، می توانید کارهای شگفت‌انگیزی انجام دهید.

و اگر اتصال دو کامپیوتر در یک ساختمان به ما امکان انجام کارهای شگفتانگیز را می دهد،
اتصال کامپیوترها در وسعت سیاره باید خیلی بهتر بشود. فناوری‌ای که برای شروع
پیاده‌سازی این هدف توسعه داده شد در دهه‌ی هشتاد میلادی (1980) و شبکه‌ی نتیجه‌ی آن
_((اینترنت))_ نامیده‌ شد. این فناوری به به آنچه وعده داده بود رسیده است.

یک کامپیوتر می تواند از این شبکه برای انتقال بیت‌ها به یک کامپیوتر دیگر استفاده کند.
برای اینکه این انتقال بیت‌ها باعث تعاملی موثر شود، باید کامپیوتر‌های دو سمت از معنای بیت‌هایی که منتقل
می‌شوند آگاه باشند. معنای یک دنباله‌ از بیت‌ها به طور کلی به نوع و مکانیزم رمزگذاری چیزی بستگی دارد که
این دنباله قرار است بیان کند.

{{index [network, protocol]}}

یک _((پروتکول)) شبکه_ یک سبک تعامل در یک شبکه را توصیف می کند. پروتکول‌هایی برای ارسال
ایمیل، دریافت ایمیل، به اشتراک گذاری فایل‌ها یا حتی برای کنترل کامپیوترهایی که
ممکن است توسط نرم‌افزارهای مخرب آلوده شده باشند وجود دارد.

{{indexsee "Hypertext Transfer Protocol", HTTP}}

به عنوان مثال، _پروتکول انتقال ابرمتن_ (((HTTP)))، برای بازیابی منابع مشخص (شامل اطلاعاتی مثل صفحات وب یا تصاویر) استفاده می‌شود. طبق این پروتکل، طرف ارسال‌کننده‌ی درخواست باید درخواستش را با یک خط مانند مثال زیر شروع کند، که شامل نام آن منبع مورد نظر و نسخه‌ای از پروتکل که قرار است استفاده شود می‌باشد:

```{lang: "text/plain"}
GET /index.html HTTP/1.1
```
قوانین بسیار زیادی وجود دارد که درخواست‌دهنده می تواند برای مشخص نمودن جزئیات بیشتر در متن درخواست قرار دهد و نیز طرف دوم، که منبع خواسته شده را بر‌می‌گرداند، می تواند در محتوایش جای‌ دهد.
در [فصل ?](http) بیشتر به HTTP خواهیم پرداخت.

{{index layering, stream, ordering}}

بیشتر پروتکل‌ها بر پایه‌ی دیگر پروتکل‌ها ساخته می شوند. HTTP با شبکه به عنوان
وسیله‌ای جریان‌دار برخورد می‌کند که در آن می توانید بیت ها را قرار داده و با
ترتیبی صحیح به مقصدی صحیح برسانید. همانطور که در [فصل ?](async) دیدیم، کسب
اطمینان از این صحت عملکرد، یکی از مسائل نسبتا مشکل است.

{{index TCP}}

{{indexsee "Transmission Control Protocol", TCP}}

_پروتکل کنترل مخابره_ (TCP) ، پروتکلی است که این مشکل را برطرف می کند. تمامی
وسایلی که از طریق اینترنت به هم متصل هستند به زبان TCP صحبت می کنند و بیشتر تعاملات
روی اینترنت بر پایه‌ی آن ساخته شده است.

{{index "listening (TCP)"}}

یک ارتباط TCP به این شکل عمل می کند: یک کامپیوتر باید منتظر بماند یا به گوش باشد
(_listening_)، تا دیگر کامپیوترها شروع به صحبت با آن کنند. برای آنکه بتوان به
انواع مختلف ارتباطات در آن واحد روی یک دستگاه واحد گوش کرد هر شنونده یک عدد
دارد (که _پورت_ نامیده می شود) که به آن اختصاص داده شده است. بیشتر پروتکل‌ها مشخص می
کنند که کدام پورت به صورت پیشفرض باید استفاده شود. به عنوان مثال، زمانی که قصد
داریم تا یک ایمیل را با استفاده از پروتکل SMTP ارسال کنیم ماشینی که از طریق آن
ایمیل را ارسال می کنیم انتظار می رود که به پورت 25 گوش کند.

یک کامپیوتر دیگر می تواند با استفاده از شماره‌ی پورت صحیح، به ماشین هدفش ارتباط گرفته و یک اتصال برقرار نماید. اگر ماشین هدف در دسترس باشد و به آن پورت گوش دهد، این اتصال با
موفقیت ایجاد می شود. کامپیوتر شنونده را سرویس دهنده (_server_) می نامند و
کامپیوتری که به آن متصل می شود را یک سرویس گیرنده (_client_) می نامند.


{{index [abtraction, "of the network"]}}

این گونه ارتباطات مانند یک لوله‌ی دوطرفه عمل می کنند که بیت‌ها می توانند در آن
جریان داشته باشند – ماشین‌هایی که در هر دو سمت قرار دارند می توانند داده‌ها را
درون آن قرار دهند. به محض اینکه بیت‌ها با موفقیت منتقل شدند، می توان آن ها را
توسط ماشینی که در دیگر سمت قرار داد دوباره خواند. این مدل مدلی مناسب است. می
توانید فرض کنید که TCP یک تجرید از شبکه فراهم می کند.

{{id web}}

## وب

وب جهان‌گستر یا _World Wide Web_ (نباید با اینترنت معادل گرفته شود) مجموعه‌ای از
پروتکل‌ها و فرمت‌ها است که به ما این امکان را می دهد تا صفحات وب را توسط یک
مرورگر مشاهده کنیم. بخش "وب" آن اشاره به این حقیقت دارد که این گونه صفحات می
توانند به آسانی با یکدیگر پیوند برقرار سازند و این ارتباطات شبکه‌ای بزرگ را می
سازد که کاربران می توانند درون آن حرکت کنند.

برای اینکه بخشی از وب باشید، تمام چیزی که لازم دارید این است که یک ماشین را به
اینترنت متصل نمایید و آن ماشین را تنظیم کنید که روی پورت 80 به پروتکل HTTP
گوش کند، بنابراین دیگر کامپیوترها می توانند از این ماشین برای گرفتن اسناد
درخواست دهند.

{{index URL}}

{{indexsee "Uniform Resource Locator", URL}}

هر ((سند)) روی وب به وسیله‌ی یک نشانی وب (URL) نام گذاری می شود که چیزی
شبیه این خواهد شد:

```{lang: null}
  http://eloquentjavascript.net/13_browser.html
 |      |                      |               |
 protocol       server               path
```

{{index HTTPS}}

قسمت اول نشان می دهد که این URL از پروتکل HTTP استفاده می کند (برخلاف مثلا
HTTP رمزگذاری شده که به شکل <bdo>https://</bdo> نوشته می شود.). سپس بخشی می آید که
سرویس‌دهنده‌ای که سند را از آن درخواست می کنیم در آن مشخص شده است. در آخر یک مسیر
رشته‌ای که سند مورد نظر (یا منبع) را مشخص می کند.

دستگاه‌هایی که به اینترنت متصل می شوند یک _((آدرس IP))_ دریافت می کنند که عددی است که
می توان از آن برای ارسال پیام به آن دستگاه استفاده نمود و ظاهری شبیه به
<bdo>`149.210.142.219`</bdo> یا <bdo>`2001:4860:4860::8888`</bdo> دارد. اما یک لیست عددی کم و بیش تصادفی
را به سختی می توان به خاطر سپرد و تایپ آن هم دشوار است؛ پس در عوض می توان یک
نام دامنه برای یک آدرس خاص یا مجموعه‌ای از آدرس‌ها ثبت کرد. من دامنه‌ی
<bdo>_eloquentjavascript.net_</bdo> را برای اشاره به آدرس IP یک ماشین که توسط خودم مدیریت می
شود ثبت کرده ام بنابراین  می‌توانم از آن دامنه برای انتشار صفحات وب
استفاده کنم.

{{index browser}}

اگر URLای که دیدیم را در نوار آدرس مرورگرتان تایپ کنید، مرورگر تلاش می کند که
آن سندی که در URL مشخص شده است را بازیابی و نمایش دهد. ابتدا، مرورگر شما باید
آدرسی که <bdo>_eloquentjavascript.net_</bdo> به آن اشاره می کند را به‌دست بیاورد. سپس با
استفاده از پروتوکول ((HTTP)) اتصالی با سرویس دهنده در آن آدرس برقرار کرده و آن
منبع <bdo>_/13_browser.html_</bdo> را درخواست می کند. اگر همه چیز به خوبی پیش رفت، سرویس
دهنده یک سند را برمی گرداند که مرورگر شما آن را روی صفحه‌ی مانیتور شما نمایش می
دهد.

## HTML

{{index HTML}}

{{indexsee "Hypertext Markup Language", HTML}}

HTML که مخفف _زبان نشانه‌گذاری ابرمتن_ است، یک فرمت سند برای صفحات وب می باشد. یک
سند HTML حاوی متن و _((برچسب‌هایی))_ (tags) است که به آن متن ساختار می بخشند و چیزهایی مثل
مثل پاراگراف‌ها، پیوند‌ها و عنوان‌ها را توصیف می کنند می، باشد.

HTML, which stands for _Hypertext Markup Language_, is the document
format used for web pages. An HTML document contains ((text)), as well
as _((tag))s_ that give structure to the text, describing things such
as links, paragraphs, and headings.

یک سند کوچک HTML ممکن است شبیه به زیر باشد:

```{lang: "text/html"}
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My home page</title>
  </head>
  <body>
    <h1>My home page</h1>
    <p>Hello, I am Marijn and this is my home page.</p>
    <p>I also wrote a book! Read it
      <a href="http://eloquentjavascript.net">here</a>.</p>
  </body>
</html>
```

{{if book

سندی با کد بالا در مرورگر به این شکل خواهد بود:

{{figure {url: "img/home-page.png", alt: "My home page",width: "6.3cm"}}}

if}}

{{index [HTML, notation]}}

برچسب ها که توسط علامت‌های بزرگتر و کوچکتر محصور شده اند (<bdo>`<`</bdo> و
<bdo>`>`</bdo>) اطلاعاتی درباره‌ی ساختار سند فراهم می سازند. دیگر متن‌ها فقط متن
ساده محسوب می شوند.

{{index doctype, version}}

سند HTML با <bdo>`<!doctype html>`</bdo> آغاز می گردد که به مرورگر می گوید این صفحه را به
عنوان یک صفحه‌ی _مدرن_ HTML تفسیر کند؛ برخلاف دیگر حالتهایی که پیش‌تر استفاده می
شد.


{{index "head (HTML tag)", "body (HTML tag)", "title (HTML tag)", "h1 (HTML tag)", "p (HTML tag)"}}

سندهای HTML دارای بخشی به نام head و بخشی به نام body است. قسمت head حاوی
اطلاعاتی درباره خود سند است و body حاوی خود سند. در این مثال، قسمت head اعلان می
کند که عنوان این سند <bdo>“My home page”</bdo> است و این سند از رمزگذاری
<bdo>UTF-8</bdo> استفاده می کند که روشی برای رمزگذاری متن‌های یونیکد به عنوان
داده‌های دودویی است. قسمت body سند دارای یک عنوان ( <bdo>`<h1>`</bdo> به معنای
"عنوان 1" – <bdo>`<h2>`</bdo> تا <bdo>`<h6>`</bdo> زیرعنوان کوچکتری را تولید می
کنند) و دو پاراگراف می‌باشد (<bdo>`<p>`</bdo>).

{{index "href attribute", "a (HTML tag)"}}

برچسب‌ها به شکل‌های متعددی می آیند. یک عنصر، مثل body ، یک پاراگراف، یا یک پیوند،
به وسیله‌ی یک _برچسب آغازین_ مثل <bdo>`<p>`</bdo> شروع شده و با یک _برچسب پایانی_
مثل <bdo>`</p>`</bdo> اتمام می یابد. بعضی از برچسب‌های آغازین مثل برچسب مربوط به
پیوند <bdo>`<a>`</bdo> حاوی اطلاعات بیشتری به شکل جفت‌های نام و مقدار
<bdo>`name="value"`</bdo> می باشند که _خصوصیت_ (attribute) نامیده می شوند. در این مثال، مقصد
پیوند توسط <bdo>`href="http://eloquentjavascript.net"`</bdo> مشخص شده است که
`href` مخفف <bdo>hypertext reference</bdo> (مرجع منبع) می باشد.

{{index "src attribute", "self-closing tag", "img (HTML tag)"}}

بعضی از انواع برچسب‌ها چیزی را محصور نمی کنند بنابراین به برچسب پایانی نیازی
ندارند. برچسب فراداده <bdo>`<meta charset="utf-8">`</bdo> مثالی از اینگونه برچسب ها است.

{{index [escaping, "in HTML"]}}

برای این که بتوان از علامت‌های بزرگتر و کوچکتر در متن یک سند استفاده کرد، با وجود
اینکه این علامت ها در￼ HTML معنای به خصوصی دارند، یک شکل از نشانه‌گذاری ویژه وجود
دارد که باید معرفی شود. یک علامت کوچکتر را به وسیله‌ی <bdo>`&lt;`</bdo>  و علامت
بزرگتر را با <bdo>`&gt;`</bdo>  می نویسند. در HTML کاراکتر `&` اگر با
یک کلمه و یک نقطه‌ویرگول (`;`) دنبال شود یک _موجودیت_ (entity) نامیده می شود و توسط کاراکتری
معادلش که به رمز درآمده است جایگزین می گردد.

{{index ["backslash character", "in strings"], "ampersand character", "double-quote character"}}

این کار مشابه کاری است که بک‌اسلش در رشته‌های جاوااسکریپت انجام می داد. به دلیل
اینکه این مکانیزم، به کاراکترهای `&` معنای خاصی می دهد، برای نوشتن خود
امپرسند می توان از <bdo>`&amp;`</bdo> استفاده کرد. در قسمت مقدار خصوصیت‌ها که توسط نقل‌قول
جفتی محصور شده اند، <bdo>`&quot;`</bdo> را می توان برای وارد کردن کاراکتر واقعی نقل قول
استفاده کرد.

{{index "error tolerance", parsing}}

HTML به شکلی تفسیر می شود که تحمل خطای بالایی دارد. زمانی که برچسب‌هایی که
باید جایی موجود بودند نوشته نشده باشند، مرورگر آن ها را بازسازی می کند. روش
این بازسازی به صورت استاندارد درآمده است. می توانید به مرورگرهای مدرن اعتماد
کنید که همه به طور یکسان این کار را انجام دهند.

سندی که در ادامه می آید شبیه به سندی تفسیر می شود که پیش تر آمد.

```{lang: "text/html"}
<!doctype html>

<meta charset=utf-8>
<title>My home page</title>

<h1>My home page</h1>
<p>Hello, I am Marijn and this is my home page.
<p>I also wrote a book! Read it
  <a href=http://eloquentjavascript.net>here</a>.
```

{{index "title (HTML tag)", "head (HTML tag)", "body (HTML tag)", "html (HTML tag)"}}

برچسب های <bdo>`<html>`</bdo>، <bdo>`<head>`</bdo> و <bdo>`<body>`</bdo> کلا
نوشته نشده اند. مرورگر می می‌داند که برچسب‌های <bdo>`<meta>`</bdo> و
<bdo>`<title>`</bdo> متعلق به قسمت head می باشند و <bdo>`<h1>`</bdo> به معنای
شروع قسمت body است. علاوه بر آن، در مثال بالا پاراگراف‌ها را با برچسب‌های پایانی
نبسته‌ام به این دلیل که یک پاراگراف جدید یا پایان سند این کار را خود به خود به
صورت ضمنی انجام می دهد. علامت‌های نقل‌قولی که پیرامون مقادیر خصوصیت‌ها بود نیز
نیامده است.


در این کتاب معمولا برچسب‌های <bdo>`<html>`</bdo>، <bdo>`<head>`</bdo> و
<bdo>`<body>`</bdo> از مثال‌ها حذف می شود تا فضای کمتری اشغال شود اما برچسب‌های
پایانی را خواهم آورد و همچنین نقل‌قول‌ها را پیرامون خصوصیتها قرار خواهم داد.

{{index browser}}

همچنین قسمت اعلان سند (doctype) و `charset` را معمولا قلم می گیرم. این کار را
برای تشویق شما برای حذف این قسمت‌ها انجام نمی دهم. مرورگرها معمولا زمانی که
فراموش می کنید تا این قسمت‌ها را بنویسید، واکنش عجیب غریبی از خود نشان می دهند.
شما می توانید فرض کنید که قسمت doctype و `charset` به صورت پیشفرض و ضمنی در مثال‌ها
وجود دارند حتی زمانی که در متن مثال دیده نمی شوند.


{{id script_tag}}

## اچ‌تی‌ام‌ال و جاوااسکریپت

{{index [JavaScript, "in HTML"], "script (HTML tag)"}}

در این کتاب، مهمترین برچسب HTML برچسب <bdo>`<script>`</bdo> است. این برچسب به ما امکان
افزودن جاوااسکریپت به یک سند را فراهم می می‌سازد.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script>alert("hello!");</script>
```

{{index "alert function", timeline}}

این اسکریپت به محض اینکه مرورگر به برچسب <bdo>`<script>`</bdo> در سند HTML برسد
اجرا خواهد شد. وقتی این صفحه باز شود یک پنجره‌ی گفتگو را نشان خواهد داد – تابع
`alert` شبیه به `prompt` عمل می کند با
این تفاوت که فقط یک پیام نمایش می دهد و امکان دریافت ورودی از کاربر را فراهم نمی کند.

{{index "src attribute"}}

قراردادن برنامه‌های بزرگ به طور مستقیم در سندهای HTML اغلب کاربردی نیست. برچسب
<bdo>`<script>`</bdo> می تواند خصوصیتی به نام `src` داشته باشد که به وسیله‌ی آن یک فایل اسکریپت
(یک فایل متنی که حاوی یک برنامه جاوااسکریپت است) را از یک URL بارگیری کند.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script src="code/hello.js"></script>
```

فایل  <bdo>_code/hello.js_</bdo> که در اینجا قرار گرفته است حاوی برنامه‌ی مشابهی
است – <bdo>`alert("hello!")`</bdo>. زمانی که یک صفحه‌ی HTML به دیگر URLها به عنوان بخشی از خود
ارجاع می دهد، به عنوان مثال یک تصویر یا یک اسکریپت – مرورگرهای وب آن منابع را
گرفته و در صفحه قرار می دهند.

{{index "script (HTML tag)", "closing tag"}}

یک برچسب script همیشه باید توسط یک <bdo>`</script>`</bdo> بسته شود، حتی زمانی که به فایلی
که در آن کدی وجود ندارد ارجاع می دهد. اگر فراموش کنید آن را ببندید، بخش‌های
دیگر صفحه که زیر آن قرار گرفته به عنوان بخشی از اسکریپت در نظر گرفته می شود.

{{index "relative path", dependency}}

می توانید ماژول‌های ES را در مرورگرها بارگیری کنید ( [فصل ?](modules#es)) و این کار با استفاده
از خصوصیت <bdo>`type="module"`</bdo> صورت می گیرد. این ماژول ها می توانند با استفاده از URLها
نسبت به خودشان به عنوان نام ماژول در اعلان `import` وابسته به دیگر ماژول‌ها باشند.

{{index "button (HTML tag)", "onclick attribute"}}

بعضی خصوصیت‌ها همچنین می توانند حاوی یک برنامه‌ی جاوااسکریپت باشند. برچسب <bdo>`<button>`</bdo>
که در ادامه می آید (که یک دکمه نمایش می دهد) خصوصیتی به نام `onclick` دارد.
مقدار این خصوصیت وقتی که این دکمه کلیک می شود اجرا می شود.

```{lang: "text/html"}
<button onclick="alert('Boom!');">DO NOT PRESS</button>
```

{{index "single-quote character", [escaping, "in HTML"]}}

توجه داشته باشید که من از علامت‌های نقل قول تکی برای رشته‌ی مربوط به خصوصیت
`onclick` استفاده کرده ام￼ به دلیل اینکه نقل قول جفتی برای خود خصوصیت استفاده شده
است. می توانستم همچنین از <bdo>`&quot;`</bdo> استفاده کنم.

## In the sandbox

{{index "malicious script", "World Wide Web", browser, website, security}}

Running programs downloaded from the ((Internet)) is potentially
dangerous. You do not know much about the people behind most sites you
visit, and they do not necessarily mean well. Running programs by
people who do not mean well is how you get your computer infected by
((virus))es, your data stolen, and your accounts hacked.

Yet the attraction of the Web is that you can browse it without
necessarily ((trust))ing all the pages you visit. This is why browsers
severely limit the things a JavaScript program may do: it can't look
at the files on your computer or modify anything not related to the
web page it was embedded in.

{{index isolation}}

Isolating a programming environment in this way is called
_((sandbox))ing_, the idea being that the program is harmlessly
playing in a sandbox. But you should imagine this particular kind of
sandbox as having a cage of thick steel bars over it so that the
programs playing in it can't actually get out.

The hard part of sandboxing is allowing the programs enough room to be
useful yet at the same time restricting them from doing anything
dangerous. Lots of useful functionality, such as communicating with
other servers or reading the content of the copy-paste ((clipboard)),
can also be used to do problematic, ((privacy))-invading things.

{{index leak, exploit, security}}

Every now and then, someone comes up with a new way to circumvent the
limitations of a ((browser)) and do something harmful, ranging from
leaking minor private information to taking over the whole machine
that the browser runs on. The browser developers respond by fixing the
hole, and all is well again—until the next problem is discovered, and
hopefully publicized, rather than secretly exploited by some
government agency or ((mafia)).

## Compatibility and the browser wars

{{index Microsoft, "World Wide Web"}}

In the early stages of the Web, a browser called ((Mosaic)) dominated
the market. After a few years, the balance shifted to
((Netscape)), which was then, in turn, largely supplanted by
Microsoft's ((Internet Explorer)). At any point where a single
((browser)) was dominant, that browser's vendor would feel entitled to
unilaterally invent new features for the Web. Since most users used
the most popular browser, ((website))s would simply start using those
features—never mind the other browsers.

This was the dark age of ((compatibility)), often called the
_((browser wars))_. Web developers were left with not one unified Web
but two or three incompatible platforms. To make things worse, the
browsers in use around 2003 were all full of ((bug))s, and of course
the bugs were different for each ((browser)). Life was hard for people
writing web pages.

{{index Apple, "Internet Explorer", Mozilla}}

Mozilla ((Firefox)), a not-for-profit offshoot of ((Netscape)),
challenged Internet Explorer's position in the late 2000s. Because
((Microsoft)) was not particularly interested in staying competitive
at the time, Firefox took a lot of market share away from it. Around
the same time, ((Google)) introduced its ((Chrome)) browser, and
Apple's ((Safari)) browser gained popularity, leading to a situation
where there were four major players, rather than one.

{{index compatibility}}

The new players had a more serious attitude toward ((standards)) and
better ((engineering)) practices, giving us less incompatibility and
fewer ((bug))s. Microsoft, seeing its market share crumble, came
around and adopted these attitudes in its Edge browser, which replaces
Internet Explorer. If you are starting to learn web development today,
consider yourself lucky. The latest versions of the major browsers
behave quite uniformly and have relatively few bugs.
