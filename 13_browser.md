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

Machines connected to the Internet get an _((IP address))_, which is a
number that can be used to send messages to that machine, and looks
something like `149.210.142.219` or `2001:4860:4860::8888`. But lists
of more or less random numbers are hard to remember and awkward to
type, so you can instead register a _((domain)) name_ for a specific
address or set of addresses. I registered _eloquentjavascript.net_ to
point at the IP address of a machine I control and can thus use that
domain name to serve web pages.

{{index browser}}

If you type this URL into your browser's ((address bar)), the browser
will try to retrieve and display the ((document)) at that URL. First,
your browser has to find out what address _eloquentjavascript.net_
refers to. Then, using the ((HTTP)) protocol, it will make a
connection to the server at that address and ask for the resource
_/13_browser.html_. If all goes well, the server sends back a
document, which your browser then displays on your screen.

## HTML

{{index HTML}}

{{indexsee "Hypertext Markup Language", HTML}}

HTML, which stands for _Hypertext Markup Language_, is the document
format used for web pages. An HTML document contains ((text)), as well
as _((tag))s_ that give structure to the text, describing things such
as links, paragraphs, and headings.

A short HTML document might look like this:

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

This is what such a document would look like in the browser:

{{figure {url: "img/home-page.png", alt: "My home page",width: "6.3cm"}}}

if}}

{{index [HTML, notation]}}

The tags, wrapped in ((angle brackets)) (`<` and `>`, the symbols for
_less than_ and _greater than_), provide information about the
((structure)) of the document. The other ((text)) is just plain text.

{{index doctype, version}}

The document starts with `<!doctype html>`, which tells the browser to
interpret the page as _modern_ HTML, as opposed to various dialects
that were in use in the past.

{{index "head (HTML tag)", "body (HTML tag)", "title (HTML tag)", "h1 (HTML tag)", "p (HTML tag)"}}

HTML documents have a head and a body. The head contains information
_about_ the document, and the body contains the document itself. In
this case, the head declares that the title of this document is "My
home page" and that it uses the UTF-8 encoding, which is a way to
encode Unicode text as binary data. The document's body contains a
heading (`<h1>`, meaning "heading 1"—`<h2>` to `<h6>` produce
subheadings) and two ((paragraph))s (`<p>`).

{{index "href attribute", "a (HTML tag)"}}

Tags come in several forms. An ((element)), such as the body, a
paragraph, or a link, is started by an _((opening tag))_ like `<p>`
and ended by a _((closing tag))_ like `</p>`. Some opening tags, such
as the one for the ((link)) (`<a>`), contain extra information in the
form of `name="value"` pairs. These are called _((attribute))s_. In
this case, the destination of the link is indicated with
`href="http://eloquentjavascript.net"`, where `href` stands for
"hypertext reference".

{{index "src attribute", "self-closing tag", "img (HTML tag)"}}

Some kinds of ((tag))s do not enclose anything and thus do not need to
be closed. The metadata tag `<meta charset="utf-8">` is an example of
this.

{{index [escaping, "in HTML"]}}

To be able to include ((angle brackets)) in the text of a document,
even though they have a special meaning in HTML, yet another form of
special notation has to be introduced. A plain opening angle bracket
is written as `&lt;` ("less than"), and a closing bracket is written
as `&gt;` ("greater than"). In HTML, an ampersand (`&`) character
followed by a name or character code and a semicolon (`;`) is called an _((entity))_
and will be replaced by the character it encodes.

{{index ["backslash character", "in strings"], "ampersand character", "double-quote character"}}

This is analogous to the way backslashes are used in JavaScript
strings. Since this mechanism gives ampersand characters a special
meaning, too, they need to be escaped as `&amp;`. Inside attribute
values, which are wrapped in double quotes, `&quot;` can be used to
insert an actual quote character.

{{index "error tolerance", parsing}}

HTML is parsed in a remarkably error-tolerant way. When tags that
should be there are missing, the browser reconstructs them. The way in
which this is done has been standardized, and you can rely on all
modern browsers to do it in the same way.

The following document will be treated just like the one shown previously:

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

The `<html>`, `<head>`, and `<body>` tags are gone completely. The
browser knows that `<meta>` and `<title>` belong in the head and that
`<h1>` means the body has started. Furthermore, I am no longer
explicitly closing the paragraphs since opening a new paragraph or
ending the document will close them implicitly. The quotes around the
attribute values are also gone.

This book will usually omit the `<html>`, `<head>`, and `<body>` tags
from examples to keep them short and free of clutter. But I _will_
close tags and include quotes around attributes.

{{index browser}}

I will also usually omit the ((doctype)) and `charset` declaration.
This is not to be taken as an encouragement to drop these from HTML
documents. Browsers will often do ridiculous things when you forget
them. You should consider the doctype and the `charset` metadata
to be implicitly present in examples, even when they are not actually shown
in the text.

{{id script_tag}}

## HTML and JavaScript

{{index [JavaScript, "in HTML"], "script (HTML tag)"}}

In the context of this book, the most important HTML tag is
`<script>`. This tag allows us to include a piece of JavaScript in a
document.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script>alert("hello!");</script>
```

{{index "alert function", timeline}}

Such a script will run as soon as its `<script>` tag is encountered
while the browser reads the HTML. This page will pop up a dialog when
opened—the `alert` function resembles `prompt`, in that it pops up a
little window, but only shows a message without asking for input.

{{index "src attribute"}}

Including large programs directly in HTML documents is often
impractical. The `<script>` tag can be given an `src` attribute  to fetch a script file (a text file containing a JavaScript
program) from a URL.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script src="code/hello.js"></script>
```

The _code/hello.js_ file included here contains the same
program—`alert("hello!")`. When an HTML page references other URLs as
part of itself—for example, an image file or a script—web browsers
will retrieve them immediately and include them in the page.

{{index "script (HTML tag)", "closing tag"}}

A script tag must always be closed with `</script>`, even if it refers
to a script file and doesn't contain any code. If you forget this, the
rest of the page will be interpreted as part of the script.

{{index "relative path", dependency}}

You can load ((ES modules)) (see [Chapter ?](modules#es)) in the
browser by giving your script tag a `type="module"` attribute. Such
modules can depend on other modules by using ((URL))s relative to
themselves as module names in `import` declarations.

{{index "button (HTML tag)", "onclick attribute"}}

Some attributes can also contain a JavaScript program. The `<button>`
tag shown next (which shows up as a button) has an `onclick`
attribute. The attribute's value will be run whenever the button is
clicked.

```{lang: "text/html"}
<button onclick="alert('Boom!');">DO NOT PRESS</button>
```

{{index "single-quote character", [escaping, "in HTML"]}}

Note that I had to use single quotes for the string in the `onclick`
attribute because double quotes are already used to quote the whole
attribute. I could also have used `&quot;`.

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
