# DOM یا مدل شیء سند

{{quote {author: "فردریش نیچه", title: "فراسوی نیک و بد", chapter: true}

چه بد! داستانی تکراری! وقتی کار ساخت خانه را تمام می‌کنی، متوجه می‌شوی که
چیزی یاد گرفتی که می‌بایست پیش از شروع کار می‌دانستی.

quote}}

{{figure {url: "img/chapter_picture_14.jpg", alt: "Picture of a tree with letters and scripts hanging from its branches", chapter: "framed"}}}

{{index drawing, parsing}}

وقتی یک صفحه وب را در مرورگرتان باز می کنید، مرورگر متن HTML صفحه را گرفته و آن
را تفسیر می کند، بسیار شبیه به آنچه تجزیه‌گر ما برای تجزیه‌ی برنامه‌ها در [فصل
?](language#parsing) انجام می داد. مرورگر یک مدل از ساختار سند می سازد و از آن
برای نمایش سند روی صفحه‌ی نمایش استفاده می کند.

{{index "live data structure"}}

این نمایش از سند، یکی از ابزارهایی است که یک برنامه‌ی جاوااسکریپت در جعبه‌ی شنی (sandbox) خود به
آن دسترسی دارد. یک ساختار داده که می‌تواند خوانده شود یا تغییر یابد؛ ساختار
داده‌ای _زنده_: زمانی که تغییری در آن رخ می‌دهد، صفحه‌ای که در مانیتور نمایش داده می
شود نیز به‌روز می شود تا تغییرات را منعکس کند.

## ساختار سند

{{index [HTML, structure]}}

می توانید یک سند HTML را به عنوان مجموعه‌ای از مستطیل‌های تودرتو در نظر بگیرید.
برچسب‌هایی مثل <bdo>`<body>`</bdo> و <bdo>`</body>`</bdo>، دیگر برچسب‌ها را در
بر می گیرند، که خود نیز حاوی برچسب‌های دیگر یا متن می‌باشند. به عنوان مثال، سندی
از [فصل قبل](browser) را مشاهده ‌می کنید:

```{lang: "text/html", sandbox: "homepage"}
<!doctype html>
<html>
  <head>
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
ساختار این صفحه به شکل زیر است:

{{figure {url: "img/html-boxes.svg", alt: "HTML document as nested boxes", width: "7cm"}}}

{{indexsee "Document Object Model", DOM}}

ساختار داده‌ای که مرورگر برای نمایش این سند استفاده می کند از این شکل پیروی می
کند. برای هر مستطیل، یک شیء وجود دارد، که می توانیم با آن ارتباط برقرار کرده
تا چیزهایی مثل برچسب HTML آن یا برچسب‌ها و متونی که دربر دارد را بدست بیاوریم.
این طرز نمایش را _مدل شیء سند_ یا به اختصار DOM می گویند.

{{index "documentElement property", "head property", "body property", "html (HTML tag)", "body (HTML tag)", "head (HTML tag)"}}

متغیر سراسری `document` امکان دسترسی به این اشیاء را فراهم می سازد. خاصیت
`documentElement` آن به شیئی ارجاع می دهد که نمایانگر برچسب <bdo>`<html>`</bdo>
است. چون هر سند HTML دارای یک سرصفحه و یک بدنه می باشد ، در نتیجه دارای
خاصیت‌های `head` و `body` نیز می باشد که به آن عناصر اشاره می می‌کنند.

## درخت‌ها

{{index [nesting, "of objects"]}}

کمی به درخت‌های گرامر که در [فصل ?](language#parsing) معرفی شدند فکر کنید. ساختار آن‌ها بسیار شبیه
به ساختار یک سند مرورگر است. هر _((گره))_ ممکن است به دیگر گره‌ها ارجاع دهد، _فرزندان_،
که خود می توانند فرزندان خود را داشته باشند. این شکل یک ساختار تودرتوی معمول است
که عناصر در آن می توانند حاوی زیرعنصرهایی مشابه باشند.

{{index "documentElement property", [DOM, tree]}}

یک ساختار داده را درخت می نامیم زمانی که دارای شاخه‌هایی بدون دور است ( یک گره
ممکن نیست به شکل￼ مستقیم یا غیر مستقیم حاوی خودش باشد) و دارای یک _((ریشه))_ مشخص
باشد. در رابطه با DOM، ریشه‌ی درخت <bdo>`document.documentElement`</bdo> است.

{{index sorting, ["data structure", "tree"], "syntax tree"}}

درخت‌ها در علم کامپیوتر کاربرد زیادی دارند. علاوه بر نمایش ساختارهای درختی مانند
اسناد HTML یا برنامه‌ها، اغلب از آن‌ها برای نگهداری مجموعه‌های مرتب شده‌ی داده
استفاده می شود چراکه معمولا ورود یا جستجوی عناصر در یک درخت با کارایی بیشتری
نسبت به یک‌ آرایه‌ی تخت انجام می شود.

{{index "leaf node", "Egg language"}}

یک درخت معمول دارای انواع مختلفی از گره‌ها می‌باشد. درخت گرامر [زبان Egg](language) دارای
شناسه‌ها، مقادیر، و گره‌های کاربرد (Application) بود. گره‌های کاربرد می توانستند دارای فرزند
باشند، درحالیکه شناسه‌ها و مقادیر از جنس _برگ_ بودند؛ منظور گره‌هایی است که فرزندی ندارند.

{{index "body property", [HTML, structure]}}

همین روال برای DOM هم برقرار است. گره‌ها به عنوان عناصر، که برچسب‌های HTML را
نمایش می دهند، ساختار سند را تعیین می کنند. این گره‌ها می توانند گره‌های فرزند
داشته باشند. یک نمونه از این نوع گره‌ها <bdo>`document.body`</bdo> است. بعضی از این فرزندان می
توانند گره‌هایی از نوع برگ باشند، مثل متن‌ها یا گره‌های توضیحات.

{{index "text node", element, "ELEMENT_NODE code", "COMMENT_NODE code", "TEXT_NODE code", "nodeType property"}}

هر شیء گره‌ی DOM دارای خاصیتی به نام `nodeType` است که حاوی کدی (عددی) است که نوع
آن گره را مشخص می کند. عناصر دارای کد 1 می‌باشند که همچنین به عنوان یک خاصیت ثابت
 <bdo>`Node.ELEMENT_NODE`</bdo> نیز تعریف شده است. گره‌های متنی ، که نمایانگر یک قسمت از متن
در سند می باشند دارای کد 3 می‌باشند (<bdo>`Node.TEXT_NODE`</bdo>). کد 8 نیز به توضیحات اختصاص دارد (<bdo>`Node.COMMENT_NODE`</bdo>).

یک روش دیگر برای به تصویر کشیدن درخت مربوط به سندمان، شکل زیر است.

{{figure {url: "img/html-tree.svg", alt: "HTML document as a tree",width: "8cm"}}}

برگ‌ها، گره‌های متنی هستند و پیکان‌ها نمایانگر روابط والد-فرزندی بین گره‌ها می باشند.

{{id standard}}

## استاندارد

{{index "programming language", [interface, design], [DOM, interface]}}

استفاده از کدهای عددی رمزگونه برای نمایش نوع گره‌ها چیزی نیست که در جاوااسکریپت
معمول باشد. در ادامه فصل می بینیم که دیگر بخش‌های رابط DOM نیز حسی نامانوس ایجاد
می کنند. دلیل آن این است که DOM فقط برای جاوااسکریپت طراحی نشده است. بلکه تلاش
شده که رابط آن نسبت به زبان‌ها بی طرف باشد که بتوان از آن در دیگر سیستم‌ها به
خوبی استفاده شود – نه فقط در HTML بلکه همچنین برای XML که فرمتی عمومی برای داده‌ها است
و گرامری شبیه به HTML دارد.

{{index consistency, integration}}

خوب این زیاد مطلوب نیست. استانداردها اغلب مفید هستند. اما در این مورد خاص، مزیت آن
(سازگاری فرا زبانی) آن قدرها قانع کننده نیست. داشتن رابطی که به خوبی با زبانی
که استفاده می کنید یکپارچه شود، زمان زیادی برای شما صرفه جویی می کند تا اینکه یک
رابط عمومی برای همه‌ی زبان‌ها داشته باشیم.

{{index "array-like object", "NodeList type"}}

یک نمونه از این یکپارچگی ضعیف، خاصیت `childNodes` است که گره‌های عنصر در DOM،
حاوی آن می‌باشند. این خاصیت یک شیء آرایه‌طور را نگهداری می کند که خاصیتی به نام
`length` دارد و همچنین خاصیت‌هایی دارد که توسط اعداد برچسب‌گذاری شده اند تا بتوان به
گره‌های فرزند دسترسی داشت. اما این یک نمونه از نوع `NodeList` است نه یک آرایه‌ی
واقعی بنابراین متدهای آرایه مثل `slice` و `map` را ندارد.

{{index [interface, design], [DOM, construction], "side effect"}}

همچنین مشکلاتی وجود دارد که ناشی از طراحی ضعیف است. به عنوان مثال، راهی برای
ایجاد یک گره جدید به همراه گره‌های فرزند و خصوصیت‌ها در یک گام وجود ندارد. بلکه
باید ابتدا گره را ایجاد کنید، بعد فرزندان و خصوصیت‌ها را یکی پس از دیگری به
وسیله اثرات جانبی بسازید. کدهایی که با DOM تعامل زیادی برقرار می‌کنند، معمولا
طولانی، تکراری و بدریخت هستند.

{{index library}}

اما این ایرادات، مشکلاتی مهلک محسوب نمی شوند. چون جاوااسکریپت این امکان را به
ما می دهد که تجریدهای خودمان را بنویسیم، می توان راه‌های بهتری را برای انجام
عملیات برنامه‌تان طراحی کنید. خیلی از کتاب‌خانه‌هایی که برای برنامه‌نویسی مرتبط با
مرورگر ایجاد شده اند، این ابزار را فراهم می کنند.


## حرکت در درخت

{{index pointer}}

گره‌های DOM حاوی پیوندهای زیادی به دیگر گره‌های نزدیک می‌باشند. نمودار زیر این موضوع را به تصویر
می کشد:

{{figure {url: "img/html-links.svg", alt: "Links between DOM nodes",width: "6cm"}}}

{{index "child node", "parentNode property", "childNodes property"}}

اگرچه این نمودار فقط یک پیوند برای هر نوع نشان می دهد، اما هر گره دارای یک خاصیت به
نام `parentNode` است که به گره‌هایی که بخشی از آن است اشاره می کند. به همین صورت، هر
گره‌ی عنصر (گره نوع ‍‍1) دارای یک خاصیت `childNodes` است که به یک شیء آرایه‌طور
اشاره می کند که حاوی فرزندان آن گره می باشد.

{{index "firstChild property", "lastChild property", "previousSibling property", "nextSibling property"}}

در تئوری، می توانید با استفاده از پیوندهای والد و فرزند، به هر جای درخت حرکت کنید.
اما جاوااسکریپت پیوندهای مناسب دیگری را در اختیار شما قرار می دهد. خاصیت‌های
`firstChild` و `lastChild` به اولین و آخرین عنصرهای فرزند اشاره می کنند یا مقدار
`null` را در صورتی که فرزندی نداشته باشد خواهند داشت. به طور مشابه،
`previousSibling` و `nextSibling` به گره‌های همجوار اشاره می کنند که گره‌هایی هستند
که تحت والد مشترکی قرار دارند و درست قبل یا بعد از گره‌ی مورد نظر قرار گرفته اند.
برای اولین فرزند، `previousSibling` برابر با￼ `null` خواهد بود و برای فرزند آخر،
`nextSibling` برابر `null` خواهد بود.

{{index "children property", "text node", element}}

همچنین خاصیتی به نام `children` وجود دارد که شبیه به `childeNodes` است با این
تفاوت که فقط فرزندان نوع عنصر (نوع 1) را شامل می شود نه دیگر انواع گره فرزند.
این خاصیت می تواند در زمانی که به گره های متنی نیازی ندارید استفاده شود.

{{index "talksAbout function", recursion, [nesting, "of objects"]}}

وقتی با یک ساختار داده‌ی تودرتو مثل این ساختار کار می کنید، توابع بازگشتی اغلب
مفید هستند. تابع مثال زیر یک سند را برای یافتن گره‌های متنی جستجو می کند که دارای
یک رشته‌ی خاص باشند و در صورت پیدا کردن آن مقدار `true` را برمی گرداند.

{{id talksAbout}}

```{sandbox: "homepage"}
function talksAbout(node, string) {
  if (node.nodeType == Node.ELEMENT_NODE) {
    for (let i = 0; i < node.childNodes.length; i++) {
      if (talksAbout(node.childNodes[i], string)) {
        return true;
      }
    }
    return false;
  } else if (node.nodeType == Node.TEXT_NODE) {
    return node.nodeValue.indexOf(string) > -1;
  }
}

console.log(talksAbout(document.body, "book"));
// → true
```

{{index "childNodes property", "array-like object", "Array.from function"}}

چون `childNodes` یک آرایه‌ی واقعی نیست نمی توان برای پیمایش آن از
<bdo>`for`/`of`</bdo> استفاده کرد بلکه باید یا از یک حلقه‌ی معولی `for` بهره برد یا از <bdo>`Array.from`</bdo> استفاده نمود.

{{index "nodeValue property"}}

خاصیت `nodeValue` یک گره‌ی متنی، حاوی رشته‌ی متنی است که گره نشان می دهد.

## پیدا کردن عناصر

{{index [DOM, querying], "body property", "hard-coding", [whitespace, "in HTML"]}}

حرکت در طول پیوندها به گره‌های والد، فرزندان و گره‌های همجوار اغلب مفید است. اما
اگر بخواهیم یک گره‌ی خاص را در یک سند پیدا کنیم، شروع از
<bdo>`document.body`</bdo> و پیمایش یک مسیر ثابت از خاصیت‌ها برای یافتن گره،
ایده‌ی خوبی نیست. برای این کار نیاز است تا فرض‌هایی درباره‌ی یک ساختار دقیق از
سند داشته باشیم – ساختاری که احتمالا قرار است آن را تغییر دهید. یکی دیگر از
فاکتورهایی که کار را پیچیده می کند این است که گره‌های متنی برای فضاهای خالی بین
گره‌ها هم ایجاد می شوند. مثلا در برچسب <bdo>`<body>`</bdo> سند، فقط سه فرزند
ندارد (<bdo>`<h1>`</bdo> و دو <bdo>`<p>`</bdo> ) بلکه در واقع دارای هفت فرزند
می‌باشد. آن سه عنصر به همراه فضاهای خالی قبل، بعد و بین‌شان.

{{index "search problem", "href attribute", "getElementsByTagName method"}}

بنابراین اگر بخواهیم خصوصیت `href` پیوند موجود در سند را به دست بیاوریم، نمی
خواهیم دستوری شبیه به ” فرزند دوم ششمین فرزند body سند را به دست بیاور” داشته
باشیم. بهتر می بود اگر می توانستیم دستوری شبیه به “اولین لینکی که در سند آمده است
را بگیر” داشته باشیم. و می توانیم این کار را بکنیم.

```{sandbox: "homepage"}
let link = document.body.getElementsByTagName("a")[0];
console.log(link.href);
```

{{index "child node"}}

تمامی گره‌های عنصر، دارای متدی به نام `getElementsByTagName` هستند که تمامی عناصری
که برچسب داده شده را دارند و زیرمجموعه‌ی آن گره محسوب می شوند (فرزند مستقیم و
غیر مستقیم) را جمع آوری می کند و￼ به صورت یک شیء آرایه‌طور برمی گرداند.

{{index "id attribute", "getElementById method"}}

برای یافتن یک گره‌ی خاص _واحد_، می توانید به آن یک خصوصیت `id` اختصاص دهید و از
<bdo>`document.getElementById`</bdo> استفاده کنید.

```{lang: "text/html"}
<p>My ostrich Gertrude:</p>
<p><img id="gertrude" src="img/ostrich.png"></p>

<script>
  let ostrich = document.getElementById("gertrude");
  console.log(ostrich.src);
</script>
```

{{index "getElementsByClassName method", "class attribute"}}

سومین متد که کاری مشابه انجام می دهد `getElementsByClassName` است که شبیه به
`getElementsByTagName` عمل می کند و در محتوای یک گره‌ی عنصر به جستجو می پردازد و
تمامی عناصری که رشته‌ی داده شده را در خصوصیت `class` شان دارند برمی گرداند.

## ایجاد تغییر در سند

{{index "side effect", "removeChild method", "appendChild method", "insertBefore method", [DOM, construction], [DOM, modification]}}

تقریبا تمامی قسمت‌های ساختار داده‌ی DOM را می‌توان تغییر داد. می توان شکل درخت سند را
با ایجاد تغییر در روابط والد-فرزندی دستکاری کرد. گره‌ها دارای متدی به نام `remove`
می‌باشند که می توان از آن برای حذف گره از والد کنونی‌اش استفاده نمود. برای افزودن یک
گره‌ی فرزند به یک گره‌ی عنصر می‌توانیم از `appendChild` استفاده کنیم که باعث می شود
آن گره به انتهای لیست فرزندان اضافه شود. یا از `insertBefore` استفاده کنیم که
گره‌ای که به عنوان آرگومان اول آمده را قبل از گره‌ای که به عنوان آرگومان دوم
آمده است وارد می کند.

```{lang: "text/html"}
<p>One</p>
<p>Two</p>
<p>Three</p>

<script>
  let paragraphs = document.body.getElementsByTagName("p");
  document.body.insertBefore(paragraphs[2], paragraphs[0]);
</script>
```

یک گره فقط در یک موقعیت از سند می تواند موجودیت داشته باشد. بنابراین، قراردادن
پاراگراف _3_ جلوی پاراگراف یک _1_ آن را از انتهای سند حذف می کند و بعد جلو
پاراگراف _1_ قرار می دهد که نتیجه به این صورت می شود: <bdo>_3/2/1_</bdo>. تمامی
عملیاتی که گره‌ای را در جایی قرار می دهد به عنوان یک اثر جانبی موجب می شوند که
ابتدا آن گره از جایگاه فعلی‌اش حذف شود (اگر جایی بوده باشد).

{{index "insertBefore method", "replaceChild method"}}

متد `replaceChild` برای جایگزین کردن یک گره‌ی فرزند با گره‌ی فرزندی دیگر استفاده
می شود. این متد دو گره به عنوان آرگومان دریافت می کند: گره‌ی جدید و گره‌ای که
باید جایگزین شود. گره‌ی جایگزین شده باید فرزند عنصری باشد که متد روی آن
فراخوانی شده است. توجه داشته باشید که هر دوی `replaceChild` و `insertBefore` به
عنوان آرگومان اول گره‌ای _جدید_ دریافت می کنند.

## ایجاد گره‌ها

{{index "alt attribute", "img (HTML tag)"}}

فرض کنید می خواهیم اسکریپتی بنویسیم که تمامی عکس‌های موجود در سند (برچسب‌های
<bdo>`<img>`</bdo>) را با نوشته‌ای که در خصوصیت `alt` آن قرار دارد جایگزین کند، نوشته‌ای که
برای مشخص کردن یک نمایش متنی برای تصویر استفاده می شود.

{{index "createTextNode method"}}

این کار هم شامل حذف عکس‌ها می شود و هم ایجاد یک گره‌ی متنی جهت جایگزینی عکس.
گره‌های متنی توسط <bdo>`document.createTextNode`</bdo> ایجاد می شوند.

```{lang: "text/html"}
<p>The <img src="img/cat.png" alt="Cat"> in the
  <img src="img/hat.png" alt="Hat">.</p>

<p><button onclick="replaceImages()">Replace</button></p>

<script>
  function replaceImages() {
    let images = document.body.getElementsByTagName("img");
    for (let i = images.length - 1; i >= 0; i--) {
      let image = images[i];
      if (image.alt) {
        let text = document.createTextNode(image.alt);
        image.parentNode.replaceChild(text, image);
      }
    }
  }
</script>
```

{{index "text node"}}

با دادن یک رشته، تابع `createTextNode` به ما یک گره‌ی متنی تحویل می دهد که می
توانیم آن را در سند قرار داده تا در صفحه نمایش نشان داده شود.

{{index "live data structure", "getElementsByTagName method", "childNodes property"}}

حلقه‌ای که عکس‌ها را پیمایش می کند از پایان لیست کارش را شروع می کند. این کار لازم است چرا
که لیست گره‌ها که توسط متدی مثل `getElementsByTagName` برگردانده می شود ( یا خاصیتی
مثل `childNodes`)، لیستی زنده است. به این معنا که با تغییر سند، لیست هم به‌روز می
شود. اگر از ابتدا شروع می کردیم، حذف اولین عکس ممکن بود باعث شود لیست اولین
عنصرش را از دست بدهد که در این صورت با تکرار حلقه در بار دوم ، زمانی که `i` برابر
1 می شود، از کار می ایستاد زیرا طول مجموعه اکنون برابر 1 است.

{{index "slice method"}}

اگر یک مجموعه‌ی ثابت از گره‌ها را لازم دارید، به‌جای یک مجموعه‌ی زنده از آن ها،
می توانید مجموعه را با فراخوانی <bdo>`Array.from`</bdo> به یک آرایه‌ی واقعی تبدیل کنید.

```
let arrayish = {0: "one", 1: "two", length: 2};
let array = Array.from(arrayish);
console.log(array.map(s => s.toUpperCase()));
// → ["ONE", "TWO"]
```

{{index "createElement method"}}

برای ایجاد گره‌های عنصر، می توانید از متد <bdo>`document.createElement`</bdo> استفاده کنید.
این متد یک نام برچسب گرفته و یک گره‌ی تهی از نوع داده شده را بر می گرداند.

{{index "Popper, Karl", [DOM, construction], "elt function"}}

{{id elt}}

در مثال پیش رو یک تابع کاربردی به نام `elt` ایجاد می شود که یک گره‌ی عنصر را ایجاد
کرده و دیگر آرگومان‌هایش را به عنوان فرزندان آن گره در نظر می گیرد. در ادامه از همین تابع برای افزودن یک خصوصیت به یک برچسب نقل قول استفاده می‌شود.

```{lang: "text/html"}
<blockquote id="quote">
  No book can ever be finished. While working on it we learn
  just enough to find it immature the moment we turn away
  from it.
</blockquote>

<script>
  function elt(type, ...children) {
    let node = document.createElement(type);
    for (let child of children) {
      if (typeof child != "string") node.appendChild(child);
      else node.appendChild(document.createTextNode(child));
    }
    return node;
  }

  document.getElementById("quote").appendChild(
    elt("footer", "—",
        elt("strong", "Karl Popper"),
        ", preface to the second editon of ",
        elt("em", "The Open Society and Its Enemies"),
        ", 1950"));
</script>
```

{{if book

نتیجه به شکل زیر خواهد بود:

{{figure {url: "img/blockquote.png", alt: "A blockquote with attribution",width: "8cm"}}}

if}}

## خصوصیت‌ها

{{index "href attribute", [DOM, attributes]}}

بعضی از خصوصیت‌های عناصر مثل `href` برای پیوندها را می توان به عنوان خاصیتی با همین
نام روی شیء DOM عنصر، مورد دستیابی قرار داد. این برای بیشتر خصوصیت‌های استاندارد
رایج، برقرار است.

{{index "data attribute", "getAttribute method", "setAttribute method", attribute}}

اما HTML این امکان را فراهم کرده است که هر خصوصیتی که بخواهید را به گره‌ها اضافه
کنید. این امکان می‌تواند مفید باشد زیرا به شما اجازه می دهد اطلاعات بیشتری را در یک
سند ذخیره کنید. با این وجود اگر نام خصوصیت‌های خودتان را بسازید، این خصوصیت‌ها به
عنوان خاصیت شیء گره در دسترس نخواهند بود. برای کار با آن ها باید از متدهای
`getAttribute` و `setAttribute` استفاده کنید.

```{lang: "text/html"}
<p data-classified="secret">The launch code is 00000000.</p>
<p data-classified="unclassified">I have two feet.</p>

<script>
  let paras = document.body.getElementsByTagName("p");
  for (let para of Array.from(paras)) {
    if (para.getAttribute("data-classified") == "secret") {
      para.remove();
    }
  }
</script>
```

توصیه شده که نام این گونه‌ خصوصیت‌های ساختگی را با پیشوند <bdo>`data-`</bdo> آغاز کنید تا
مطمئن شوید که تداخلی با دیگر خصوصیت‌ها پیش نخواهد آمد.

{{index "getAttribute method", "setAttribute method", "className property", "class attribute"}}

خصوصیت `class` که یکی از خصوصیت‌های رایج است، یک کلیدواژه در جاوااسکریپت محسوب می
شود. به دلایل تاریخی – بعضی از پیاده‌سازی‌های قدیمی جاوااسکریپت نمی توانستند
نام‌های خاصیتی که با کلیدواژه‌ها مطابقت داشتند را مدیریت کنند – خاصیتی که برای
دسترسی به این خصوصیت در نظر گرفته شده است `className` است. همچنین می توانید تحت
نام واقعی خودش `"class"` نیز به وسیله‌ی متدهای `getAttribute` و `setAttribute` به آن
دسترسی داشته باشید.

## طرح‌بندی (layout)

{{index layout, "block element", "inline element", "p (HTML tag)", "h1 (HTML tag)", "a (HTML tag)", "strong (HTML tag)"}}

ممکن است متوجه شده باشید که انواع مختلف عنصرها به صورت متفاوتی طرح بندی می شوند.
بعضی مانند پاراگراف ها (<bdo>`<p>`</bdo>) یا سرعنوان‌ها (`<h1>`)، تمامی عرض سند را اشغال کرده و
هر کدام در خط جدیدی به نمایش در می‌آیند. این عناصر، عناصر بلاک نامیده می شوند. دیگر
عناصر، مثل پیوندها (`<a>`) یا عنصر <bdo>`<strong>`</bdo> در همان خط کنار متن پیرامونشان نمایش
داده می شوند. این عنصرها را عناصر درون خطی (inline) می نامند.

{{index drawing}}

برای هر سند داده شده، مرورگرها می‌توانند با درنظر گرفتن موقعیت و اندازه‌ی هر
عنصر، یک طرح را محاسبه کنند. این طرح بعد برای به تصویر کشیدن سند استفاده می
شود.

{{index "border (CSS)", "offsetWidth property", "offsetHeight property", "clientWidth property", "clientHeight property", dimensions}}

اندازه و موقعیت یک عنصر را می توان از طریق جاوااسکریپت به دست آورد. خاصیت‌های
`offsetWidth` و `offsetHeight` فضایی که یک عنصر اشغال می کند را در واحد _پیکسل_ نشان می
دهند. یک پیکسل واحد بنیادی اندازه‌گیری در مرورگر است. به طور سنتی این واحد به
کوچکترین نقطه‌ای که صفحه‌نمایش می تواند به تصویر بکشد مرتبط است اما در مانیتورهای
مدرن، که قادرند نقطه‌های _خیلی_ کوچک را به تصویر بکشند، دیگر موضوعیت￼ ندارد و یک
پیکسل مرورگر ممکن است چند نقطه‌ را در صفحه نمایش پوشش دهد.

به طور مشابه، `clientWidth` و `clientHeight` به شما اندازه‌ی فضای _درون_ یک عنصر
را نشان می دهد و عرض خط مرزی (border) در نظر گرفته نمی شود.

```{lang: "text/html"}
<p style="border: 3px solid red">
  I'm boxed in
</p>

<script>
  let para = document.body.getElementsByTagName("p")[0];
  console.log("clientHeight:", para.clientHeight);
  console.log("offsetHeight:", para.offsetHeight);
</script>
```

{{if book

اگر به یک پاراگراف یک خط مرزی اضافه کنیم باعث می شود که یک مستطیل دور آن کشیده شود.

{{figure {url: "img/boxed-in.png", alt: "A paragraph with a border",width: "8cm"}}}

if}}

{{index "getBoundingClientRect method", position, "pageXOffset property", "pageYOffset property"}}

{{id boundingRect}}

موثرترین روش برای بدست آوردن موقعیت دقیق یک عنصر روی صفحه‌ی نمایش، استفاده از متد
`getBoundingClientRect` است. این متد یک شیء بر می گرداند که شامل خاصیت‌های `top`،
`bottom`، `left` و `right` می باشد که مشخص می کنند موقعیت هر ضلع یک عنصر به نسبت بالا
و چپ صفحه‌نمایش چند پیکسل است. اگر این اعداد را نسبت به کل سند لازم دارید، بایستی
موقعیت اسکرول فعلی را به آن اضافه کنید. این موقعیت را می می‌توان در متغیرهای
`pageXOffset` و `pageYOffset` بدست آورد.

{{index "offsetHeight property", "getBoundingClientRect method", drawing, laziness, performance, efficiency}}

طرح بندی یک سند می تواند کار زیادی ببرد. برای اعمال سرعت بیشتر، موتور مروگر با
هر بار تغییر در سند آن را دوباره طرح بندی نمی کند بلکه تا زمانی که بتواند آن
را به تاخیر می اندازد .زمانی که برنامه‌ی جاوااسکریپت که سند را تغییر داده است به
پایان اجرایش برسد، مرورگر بایستی یک طرح جدید محاسبه کند تا بتواند سند
تغییریافته را به تصویر بکشد. زمانی که یک برنامه موقعیت یا اندازه‌ی چیزی را با
خواندن خاصیت‌هایی مثل `offsetHeight` یا فراخوانی `getBoundingClientRect` درخواست می
کند، فراهم ساختن اطلاعات صحیح نیز نیاز به محاسبه‌ی طرح دارد.

{{index "side effect", optimization, benchmark}}

برنامه‌ای که زیاد و به تکرار به خواندن اطلاعات طرح DOM و تغییر DOM می پردازد باعث
می شود که محاسبات طرح‌بندی زیاد اتفاق بیفتد که در نتیجه این برنامه به کندی اجرا
خواهد شد. کد پیش رو مثالی از این گونه برنامه است. این مثال از دو برنامه تشکیل
یافته است که خطی از کاراکترهای _X_ با پهنای <bdo>2,000</bdo> پیکسل می سازند و زمانی که هرکدام
طول می کشد را محاسبه می کنند.

```{lang: "text/html", test: nonumbers}
<p><span id="one"></span></p>
<p><span id="two"></span></p>

<script>
  function time(name, action) {
    let start = Date.now(); // Current time in milliseconds
    action();
    console.log(name, "took", Date.now() - start, "ms");
  }

  time("naive", () => {
    let target = document.getElementById("one");
    while (target.offsetWidth < 2000) {
      target.appendChild(document.createTextNode("X"));
    }
  });
  // → naive took 32 ms

  time("clever", function() {
    let target = document.getElementById("two");
    target.appendChild(document.createTextNode("XXXXX"));
    let total = Math.ceil(2000 / (target.offsetWidth / 5));
    target.firstChild.nodeValue = "X".repeat(total);
  });
  // → clever took 1 ms
</script>
```

## سبک دهی

{{index "block element", "inline element", style, "strong (HTML tag)", "a (HTML tag)", underline}}

تاکنون دیده‌ایم که عناصر مختلف HTML به شکل متفاوتی به تصویر کشیده می‌شوند. بعضی
به عنوان بلاک و بعضی درون‌خطی (inline) نشان داده می شوند. بعضی سبک بصری اضافه می کنند –
`<strong>` محتوایش را توپر می کند و `<a>` محتوایش را آبی رنگ و زیر آن خط می
اندازد.

{{index "img (HTML tag)", "default behavior", "style attribute"}}

شیوه‌ای که یک برچسب `<img>` تصویر را نمایش می دهد یا یک برچسب `<a>` متنی را به
یک لینک تبدیل می‌کند، به طور کامل به نوع آن عنصر وابسته است. اما سبک بصری‌ای که به
صورت پیش‌فرض به یک عنصر اضافه می شود، مثل رنگ متن یا داشتن زیرخط، را می توانیم
تغییر دهیم. در اینجا مثالی که از خصوصیت `style` استفاده می کند را می بینیم.

```{lang: "text/html"}
<p><a href=".">Normal link</a></p>
<p><a href="." style="color: green">Green link</a></p>
```

{{if book

لینک دوم به جای این که با رنگ پیش‌فرض نمایش داده شود سبز خواهد شد.

{{figure {url: "img/colored-links.png", alt: "A normal and a green link",width: "2.2cm"}}}

if}}

{{index "border (CSS)", "color (CSS)", CSS, "colon character"}}

یک خصوصیت style می تواند حاوی یک یا چند _اعلان_ باشد که شامل یک خاصیت (مثل
`color`) که به همراه دونقطه و مقدارش می‌آید. زمانی که بیش از یک اعلان وجود دارد،
باید هر اعلان با یک نقطه‌ویرگول جدا شود مثل <bdo>`"color: red; border: none"`</bdo>.

{{index "display (CSS)", layout}}

خیلی از جنبه‌های مربوط به سند وجود دارند که می توانند توسط سبک‌دهی تاثیر بپذیرند.
به عنوان مثال خاصیت `display` برای کنترل نحوه‌ی نمایش یک عنصر به صورت درون‌خطی یا
بلاک استفاده می شود.

```{lang: "text/html"}
This text is displayed <strong>inline</strong>,
<strong style="display: block">as a block</strong>, and
<strong style="display: none">not at all</strong>.
```

{{index "hidden element"}}

برچسب `block` در خط خودش به پایان می رسد به دلیل اینکه عناصر بلاکی به صورت درون
خطی کنار متن پیرامونشان نشان داده نمی شوند. برچسب آخر اصلا نمایش
داده نمی شود – <bdo>`display: none`</bdo> مانع از نمایش یک عنصر در صفحه‌ی نمایش می شود. این روشی
برای مخفی کردن عناصر است. معمولا ترجیح داده می شود بجای حذف کامل یک عنصر از سند، از این روش استفاده شود
به دلیل این که بازگرداندن آن در آینده در این روش آسان تر است.

{{if book

{{figure {url: "img/display.png", alt: "Different display styles",width: "4cm"}}}

if}}

{{index "color (CSS)", "style attribute"}}

کدهای جاوااسکریپت می توانند مستقیما سبک‌دهی یک عنصر را با استفاده از خاصیت `style`
دستکاری کنند. این￼ خاصیت شیءای را نگهداری می کند که خاصیت‌هایی برای همه‌ی ویژگی‌های
سبک‌دهی دارد. مقدار این خاصیت‌ها از نوع رشته است که می توانیم برای تغییر یک جنبه‌ی
خاص از سبک بصری عنصر مورد نظر آن را بنویسیم.

```{lang: "text/html"}
<p id="para" style="color: purple">
  Nice text
</p>

<script>
  let para = document.getElementById("para");
  console.log(para.style.color);
  para.style.color = "magenta";
</script>
```

{{index "camel case", capitalization, "hyphen character", "font-family (CSS)"}}

نام بعضی از خاصیت‌های سبک‌دهی حاوی کاراکتر <bdo>خط پیوند (-)</bdo> است مانند <bdo>`font-family`</bdo>. به دلیل
این که کار با این نوع نام ها در جاوااسکریپت کمی دشوار است (برای دسترسی باید
چیزی مثل <bdo>`style["font-family"]`</bdo> داشته باشید)، نام خاصیت‌های این گونه نام‌ها در شیء
`style` آن خط پیوند را حذف کرده و حرف بعد از آن را با حروف بزرگ می نویسند <bdo>(`style.fontFamily`)</bdo>.

## سبک‌های آبشاری

{{index "rule (CSS)", "style (HTML tag)"}}

{{indexsee "Cascading Style Sheets", CSS}}
{{indexsee "style sheet", CSS}}

سیستم سبک‌دهی بصری برای HTML را CSS می نامند که مخفف برگه‌های سبک آبشاری یا سلسله‌مراتبی
(cascading style sheets) است. یک برگه‌ی سبک به مجموعه‌ای از دستورات گفته می شود که
برای سبک‌دهی ظاهری به عناصر یک سند استفاده می شوند. می توان آن را درون یک جفت
برچسب <bdo>`<style>`</bdo> قرار داد.

```{lang: "text/html"}
<style>
  strong {
    font-style: italic;
    color: gray;
  }
</style>
<p>Now <strong>strong text</strong> is italic and gray.</p>
```

{{index "rule (CSS)", "font-weight (CSS)", overlay}}

معنای _آبشاری_ این است که قوانینی با سلسله‌مراتب با هم ترکیب می شوند تا سبک نهایی را برای یک
عنصر تولید کنند. در مثال بالا، سبک پیش‌فرض برای برچسب‌های <bdo>`<strong>`</bdo>، که در آن
<bdo>`font-weight: bold`</bdo> تعریف شده بود، توسط دستوری دیگر که در برچسب `<style>` آمده است
تغییر یافته و <bdo>`font-style`</bdo> و `color` به آن اضافه شده است.

{{index "style (HTML tag)", "style attribute"}}

وقتی چندین دستور یک مقدار را برای یک خاصیت واحد تعریف می کنند، آخرین دستوری که
خوانده شود دارای حق تقدم بالاتری خواهد بود و اعمال می شود. بنابراین اگر دستوری
که در برچسب <bdo>`<style>`</bdo> آمده است <bdo>`font-weight: normal`</bdo> را داشته باشد، دستور پیش‌فرض
<bdo>`font-weight`</bdo> در نظر گرفته نمی شود و متن به شکل نرمال نمایش داده می شود نه به صورت
توپر. دستوراتی که در خصوصیت `style` به صورت مستقیم به یک گره اعمال می شوند دارای
بالاترین حق تقدم هستند و همیشه در سلسله‌مراتب برنده می شوند.

{{index uniqueness, "class attribute", "id attribute"}}

می توان چیزهایی بجز نام برچسب‌ها را در دستورات CSS مورد هدف قرار داد. دستوری به
شکل <bdo>`.abc`</bdo>، به همه‌ی عناصری که خصوصیت classشان دارای مقدار `"abc"` است اعمال می شود.
دستوری به شکل <bdo>`#xyz`</bdo> به عنصری اعمال می شود که خصوصیت `id` آن برابر `"xyz"` باشد (که
باید در سند منحصر به فرد باشد).

```{lang: "text/css"}
.subtle {
  color: gray;
  font-size: 80%;
}
#header {
  background: blue;
  color: white;
}
/* p elements with id main and with classes a and b */
p#main.a.b {
  margin-bottom: 20px;
}
```

{{index "rule (CSS)"}}

قاعده‌ی حق تقدمی که موجب می‌شد دستوری که آخر تعریف شده بود اعمال شود، زمانی موثر
است که دستورات دارای specificity (درجه‌ی صراحت) یکسانی باشند. درجه‌ی صراحت یک
دستور، معیاری از میزان دقتی است که آن دستور، عناصر هدفش را توصیف می کند که توسط
عدد و نوع (برچسب، class و ID) عناصر مورد هدف تعیین می شود. به عنوان مثال، دستوری
که <bdo>`p.a`</bdo> را هدف قرار می دهد دارای صراحت بیشتری از دستوراتی است که `p` یا فقط <bdo>`.a`</bdo> را
هدف قرار می دهند و بنابراین حق تقدم بیشتری خواهد داشت.

{{index "direct child node"}}

دستوری به شکل <bdo>`p > a {…}`</bdo> سبک‌های تعریف شده را به همه‌ی برچسب‌های `<a>` که فرزند مستقیم
برچسب‌های `<p>` محسوب می شوند اعمال می کند. به طور مشابه، <bdo>`p a {…}`</bdo> به همه‌ی
برچسب‌های `<a>` که درون برچسب‌های `<p>` باشند اعمال می شود فارغ از اینکه فرزند مستقیم
یا غیر مستقیم باشند.

## گزینشگرهای پرس و جو

{{index complexity, CSS}}

ما در این کتاب زیاد از برگه‌های سبک‌دهی استفاده نخواهیم کرد. درک آن ها برای
برنامه‌نویسی در مرورگر مفید است اما دامنه‌ی بحث در باره‌ی سبک‌دهی به اندازه‌ای
گسترده می‌باشد که نیاز به کتاب مجزایی داشته باشند.

{{index "domain-specific language", [DOM, querying]}}

علت اینکه قواعد گزینشگر (selector) – منظور شیوه‌ی نشان‌کذاری
استفاده شده در برگه‌های سبک‌دهی برای تعیین عناصر هدف برای اعمال سبک‌ها می‌باشد – را معرفی کردم این
است که می توانیم از این زبان نصف و نیمه به عنوای روشی موثر برای پیدا کردن عناصر
DOM استفاده کنیم.

{{index "querySelectorAll method", "NodeList type"}}

متد `querySelectorAll` که هم در شیء `document` موجود است و هم روی گره‌های عنصر،
یک رشته‌ی گزینشگر دریافت می کند و یک `NodeList‍` که حاوی تمامی عناصر تطبیق
خورده است را برمی گرداند.

```{lang: "text/html"}
<p>And if you go chasing
  <span class="animal">rabbits</span></p>
<p>And you know you're going to fall</p>
<p>Tell 'em a <span class="character">hookah smoking
  <span class="animal">caterpillar</span></span></p>
<p>Has given you the call</p>

<script>
  function count(selector) {
    return document.querySelectorAll(selector).length;
  }
  console.log(count("p"));           // All <p> elements
  // → 4
  console.log(count(".animal"));     // Class animal
  // → 2
  console.log(count("p .animal"));   // Animal inside of <p>
  // → 2
  console.log(count("p > .animal")); // Direct child of <p>
  // → 1
</script>
```

{{index "live data structure"}}

برخلاف متدهایی مثل `getElementsByTagName`، شیءای که توسط `querySelectorAll`
برگردانده می شود زنده یا پویا نیست. با تغییر سند، این شیء به روز نمی شود و هنوز یک
آرایه‌ی واقعی نیست و اگر لازم دارید تا از ویژگی‌های آرایه‌ها بهره ببرید باید
<bdo>`Array.from`</bdo> را روی آن‌ها فراخوانی کنید.

{{index "querySelector method"}}

متد `querySelector` (بدون All) به شکل مشابهی عمل می کند. این متد برای زمانی که قصد
دارید یک عنصر مشخص را هدف قرار دهید مناسب است. این متد فقط اولین عنصری که مطابق
گزینشگر بود را برمی گرداند و در صورت پیدا نکردن هیچ عنصری مقدار null را تولید می
کند.

{{id animation}}

## موقعیت دهی و متحرک‌سازی

{{index "position (CSS)", "relative positioning", "top (CSS)", "left (CSS)", "absolute positioning"}}

خاصیت `position` در سبک‌دهی، تاثیر مهمی در در طرح‌بندی صفحه‌ می‌گذارد. به صورت
پیش فرض مقدار آن برابر `static` است که یعنی عنصر مورد نظر در موقعیت نرمال خودش در
سند قرار می گیرد. زمانی که این مقدار به `relative` تغییر می یابد، عنصر همچنان
فضایی در سند اشغال می کند اما اکنون می توان از خاصیت‌های `top` و `left` برای تغییر
مکان آن نسبت به جایگاه نرمالش استفاده کرد. زمانی که `position` برابر `absolute`
قرار گیرد، عنصر مورد نظر از جریان چیدمان صفحه خارج می شود – به این معنا که دیگر
فضایی را اشغال نمی کند و ممکن است روی دیگر عناصر بیفتد. همچنین در این حالت
خاصیت‌های `top` و `left` را می توان برای موقعیت دهی مطلق عنصر نسبت به گوشه‌ی بالا و چپ
نزدیک ترین عنصر والدش (که دارای مقدار `position` غیر از static است) استفاده کرد یا
در صورت نبود آن، نسبت به کل سند موقعیت دهی می شود.

{{index [animation, "spinning cat"]}}

می توانیم از این خاصیت برای ایجاد یک پویانمایی استفاده کنیم. صفحه‌ی پیش رو تصویری
از یک گربه را نشان می دهد که به دور دایره‌ای حرکت می کند.

```{lang: "text/html", startCode: true}
<p style="text-align: center">
  <img src="img/cat.png" style="position: relative">
</p>
<script>
  let cat = document.querySelector("img");
  let angle = Math.PI / 2;
  function animate(time, lastTime) {
    if (lastTime != null) {
      angle += (time - lastTime) * 0.001;
    }
    cat.style.top = (Math.sin(angle) * 20) + "px";
    cat.style.left = (Math.cos(angle) * 200) + "px";
    requestAnimationFrame(newTime => animate(newTime, time));
  }
  requestAnimationFrame(animate);
</script>
```

{{if book

پیکان خاکستری رنگ، مسیر حرکت تصویر گربه را نشان می دهد.

{{figure {url: "img/cat-animation.png", alt: "A moving cat head",width: "8cm"}}}

if}}

{{index "top (CSS)", "left (CSS)", centering, "relative positioning"}}

تصویر ما در مرکز صفحه قرار گرفته است و مقدار `position` آن برابر `relative` است. ما پیوسته مقدار `top` و `left`
تصویر را برای ایجاد حرکت به‌روز رسانی می کنیم.

{{index "requestAnimationFrame function", drawing, animation}}

{{id animationFrame}}

این اسکریپت از `requestAnimationFrame` برای زمانبندی اجرای تابع `animate` هنگامی
که مرورگر آماده است تا تصویر صفحه را دوباره بکشد، استفاده می کنیم. تابع `animate` خود
دوباره `requestAnimationFrame` را برای زمانبندی به‌روزرسانی بعدی فراخوانی می کند.
زمانی که پنجره‌ی مرورگر (یا تب مرورگر) فعال است، این باعث می شود که به‌روز‌رسانی‌ها
با نرخ 60 بار در ثانیه انجام شود که پویانمایی روانی را تولید می کند.

{{index timeline, blocking}}

اگر فقط DOM را در حلقه به‌روزرسانی می کردیم، صفحه ممکن بود قفل شود چیزی در
تصویر نمایش داده نشود. مرورگرها صفحه‌ی نمایش خود را در هنگام اجرای یک برنامه‌ی
جاوااسکریپت به‌روزرسانی نمی کنند و اجازه‌ی هیچ تعاملی با صفحه را هم فراهم نمی
کنند. به همین دلیل است که به `requestAnimationFrame` نیاز داریم – این تابع به
مرورگر می گوید که کار ما در این لحظه تمام است و می تواند به کارش ادامه دهد، مثل
به‌روزرسانی صفحه نمایش و پاسخ به درخواست‌های کاربر.

{{index "smooth animation"}}

تابع پویانمایی، زمان فعلی را به عنوان یک آرگومان دریافت می کند. برای کسب اطمینان
از اینکه میزان حرکت￼ گربه در هزارم ثانیه مداوم و باثبات است، این تابع سرعت را
در قسمت‌هایی که زاویه تغییر می کند بر اساس تفاوت بین زمان فعلی و آخرین باری که
تابع اجرا شد در نظر می گیرد. اگر با هر قدم مقدار ثابتی حرکت در زاویه انجام می‌شد،
ممکن بود حرکت تصویر لنگ بزند، درصورتی‌ که به عنوان مثال یک برنامه‌ی سنگین دیگر روی همان
کامپیوتر اجرا می شد که تابع را برای کسری از ثانیه از حرکت می انداخت.

{{index "Math.cos function", "Math.sin function", cosine, sine, trigonometry}}

{{id sin_cos}}

برای حرکت دایره‌ای از توابع مثلثات مثل <bdo>`Math.cos`</bdo> و <bdo>`Math.sin`</bdo> استفاده می شود. برای
افرادی که با این مفاهیم آشنا نیستند، به طور خلاصه آن ها را معرفی خواهم کرد چرا
که کم و بیش از آن ها در کتاب استفاده خواهیم کرد.

{{index coordinates, pi}}

متدهای <bdo>`Math.cos`</bdo> و <bdo>`Math.sin`</bdo> برای پیدا کردن نقاطی که روی محیط دایره‌ای با مرکز
<bdo>(0,0)</bdo> و شعاع یک استفاده می شوند. هر دوی این متدها ورودی‌هایشان را به عنوان
موقعیت‌هایی روی این دایره تفسیر می کنند، که صفر به معنای نقطه‌ای است که در
راست‌ترین قسمت دایره قرار گرفته است و حرکت در جهت گردش عقربه‌های ساعت تا <bdo>2π</bdo> می
باشد (حدود <bdo>6.28</bdo>) که یک دور کامل دایره انجام می شود. <bdo>`Math.cos`</bdo> مختصات x نقطه‌ای که
مربوط به موقعیت داده شده است را برمی‌گرداند در حالیکه <bdo>`Math.sin`</bdo> مختصات y آن را می
دهد. موقعیت‌هایی (یا زاویه‌ها) که از <bdo>2π</bdo> بزرگتر باشند یا از 0 کوچکتر باشند معتبر
محسوب می شوند – چرخش تکرار می شود بنابراین <bdo>_a_+2π</bdo> معادل همان زاویه‌ی _a_ خواهد بود.

{{index "PI constant"}}

این واحد اندازه‌گیری برای زاویه‌ها را رادیان می نامند- یک دایره‌ی کامل برابر <bdo>2π</bdo>
رادیان است، مشابه 360 در واحد درجه. ثابت π در جاوااسکریپت توسط <bdo>`Math.PI`</bdo> در دسترس
است.

{{figure {url: "img/cos_sin.svg", alt: "Using cosine and sine to compute coordinates",width: "6cm"}}}

{{index "counter variable", "Math.sin function", "top (CSS)", "Math.cos function", "left (CSS)", ellipse}}

کد پویانمایی گربه یک شمارنده هم نگهداری می کند، `angle`، تا زاویه‌ی فعلی حرکت را
داشته باشد و با هر بار فراخوانی تابع `animate` آن را افزایش می دهد. بعدا می تواند
از این زاویه برای محاسبه موقعیت فعلی عنصر تصویر استفاده کند. مقدار خاصیت سبکی
`top` هم به وسیله‌ی <bdo>`Math.sin`</bdo> و ضرب آن در 20 محاسبه می شود که نمایانگر شعاع عمودی
در بیضی ما است. مقدار `left` نیز بر اساس <bdo>`Math.cos`</bdo> و ضرب آن در 200 است بنابراین
بیضی ما دارای عرض بسیار بیشتری نسبت به طولش است.

{{index "unit (CSS)"}}

توجه داشته باشید که مقادیر مربوط به سبک‌ها معمولا به واحد نیاز دارند. در این
مثال، ما باید `"px"` را به عدد اضافه کنیم تا به مرورگر اعلام کنیم که شمارش در
واحد پیکسل می باشد (نه سانتیمتر، “em”، یا دیگر واحدها). به راحتی ممکن است این
نکته فراموش شود. استفاده از اعداد بدون واحدها باعث می شود که سبک‌دهی شما اعمال
نگردد- مگر اینکه آن عدد 0 باشد که همیشه معنای یکسانی دارد.

## خلاصه

برنامه‌های جاوااسکریپت می توانند در صفحه‌ای که مرورگر به نمایش می گذارد، با
استفاده از یک ساختار داده به نام DOM، دخالت و دستکاری کنند. این ساختار داده
نمایانگر مدل مرورگر از صفحه است و یک برنامه‌ی جاوااسکریپت می تواند آن را
تغییر دهد و در سندی که به نمایش درمی آید تغییر ایجاد کند.

DOM به شکل یک درخت سازماندهی شده است که در آن عناصر به صورت سلسله‌مراتبی براساس
ساختار سند مرتب می شوند. اشیائی که نماینده‌ی عناصر هستند دارای خاصیت‌هایی مانند
`parentNode` و `childNodes` هستند که می توان از آن ها برای حرکت در این درخت استفاده
کرد.

نحوه‌ی نمایش یک سند را می توان با سبک‌دهی تغییر داد و این کار به دو روش چسباندن سبک‌ها به
عناصر به صورت مستقیم و یا با تعریف دستوراتی که عناصر خاصی را هدف قرار می دهند صورت می‌پذیرد.
خاصیت‌های سبک‌دهی زیاد و متنوعی وجود دارد مثل `color` یا `display`. کدهای جاوااسکریپت
می توانند سبک یک عنصر را مستقیما از طریق خصوصیت `style` دستکاری کنند.

## تمرین‌ها

{{id exercise_table}}

### ساخت یک جدول

{{index "table (HTML tag)"}}

یک جدول HTML به وسیله‌ی ساختار برچسب‌های زیر ساخته می شود:

```{lang: "text/html"}
<table>
  <tr>
    <th>name</th>
    <th>height</th>
    <th>place</th>
  </tr>
  <tr>
    <td>Kilimanjaro</td>
    <td>5895</td>
    <td>Tanzania</td>
  </tr>
</table>
```

{{index "tr (HTML tag)", "th (HTML tag)", "td (HTML tag)"}}

برای هر ردیف، برچسب <bdo>`<table>`</bdo> یک برچسب <bdo>`<tr>`</bdo> خواهد داشت.
درون این برچسب‌های <bdo>`<tr>`</bdo> می توانیم سلول‌های جدول را قرار دهیم: سلول‌های معمولی
(<bdo>`<td>`</bdo>) یا سلول‌های عنوان (<bdo>`<th>`</bdo>).

با در دست داشتن مجموعه اطلاعاتی در باره‌ی کوه‌ها، آرایه‌ای از اشیاء شامل `name`،
`height` و `place` ، یک ساختار DOM برای جدولی که این اشیاء را می شمارد ایجاد کنید.
این جدول باید یک ستون برای هر کلید و یک ردیف برای هر شیء داشته باشد، مازاد بر
آن یک ردیف عنوان به وسیله‌ی عنصرهای <bdo>`<th>`</bdo> در قسمت بالا که نام ستون‌ها را لیست کند.

این برنامه را به صورتی بنویسید که در آن ستون‌ها به طور خودکار از اشیاء گرفته می می‌شوند و
این کار با گرفتن نام‌های خاصیت‌های اولین شیء در مجموعه‌ی داده‌ها رخ می دهد.

به این صورت بنویسید که ستون‌ها به صورت خودکار با گرفتن نام‌ خاصیت‌های اولین شیء
در مجموعه‌ی داده‌ها ایجاد شوند.

جدول نهایی را به عنصری که دارای خصوصیت `id` برابر با `"mountains"` می‌باشد اضافه کنید تا
در صفحه نمایش داده￼ شود.

{{index "right-aligning", "text-align (CSS)"}}

بعد از این که این قسمت را تکمیل کردید سلول‌هایی که حاوی
مقادیر عددی هستند را راست چین کنید و این کار را با تنظیم <bdo>`style.textAlign`</bdo> برابر با
`"right"` انجام دهید.

{{if interactive

```{test: no, lang: "text/html"}
<h1>Mountains</h1>

<div id="mountains"></div>

<script>
  const MOUNTAINS = [
    {name: "Kilimanjaro", height: 5895, place: "Tanzania"},
    {name: "Everest", height: 8848, place: "Nepal"},
    {name: "Mount Fuji", height: 3776, place: "Japan"},
    {name: "Vaalserberg", height: 323, place: "Netherlands"},
    {name: "Denali", height: 6168, place: "United States"},
    {name: "Popocatepetl", height: 5465, place: "Mexico"},
    {name: "Mont Blanc", height: 4808, place: "Italy/France"}
  ];

  // Your code here
</script>
```

if}}

{{hint

{{index "createElement method", "table example", "appendChild method"}}

می توانید از <bdo>`document.createElement`</bdo> برای ایجاد گره‌های جدید استفاده
کنید، همچنین از <bdo>`document.createTextNode`</bdo> برای ایجاد گره‌های متنی و
از <bdo>`appendChild`</bdo> نیز برای قرار دادن گره‌ها در دیگر گره‌ها.

{{index "Object.keys function"}}

لازم خواهد شد تا نام‌ کلید‌های شیء را یک بار پیمایش کرده تا ردیف بالایی را پر کنید و بعد دوباره برای هر شیء

در ادامه لازم دارید تا نام کلید‌ها را پیمایش کنید؛ یک بار برای پر کردن ردیف
بالایی و بعد دوباره برای هر همه‌ی اشیاء موجود در آرایه تا بتوانید ردیف‌های داده
را بسازید. برای بدست آوردن یک آرایه از نام کلید‌ها از شیء اول،
<bdo>`Object.keys`</bdo> به دردتان خواهد خورد.

{{index "getElementById method", "querySelector method"}}

برای اضافه کردن جدول به گره‌ی والد فعلی، می توانید از
<bdo>`document.getElementById`</bdo> یا <bdo>`document.querySelector`</bdo> برای
پیدا کردن گره‌ای با خاصیت `id` مورد نظر استفاده کنید.

hint}}

### گرفتن عناصر به وسیله‌ی نام برچسب‌ها

{{index "getElementsByTagName method", recursion}}

متد <bdo>`document.getElementsByTagName`</bdo> تمامی عناصر فرزند را برای نام
برچسب داده شده برمی گرداند. نسخه‌ی خودتان از این متد را به عنوان یک تابع بنویسید
که یک گره و یک رشته (نام برچسب) را به عنوان ورودی‌ها بگیرد و آرایه‌ای که حاوی
تمامی گره‌های فرزند متعلق به آن برچسب را برگرداند.

{{index "nodeName property", capitalization, "toLowerCase method", "toUpperCase method"}}

برای پیدا کردن نام برچسب یک عنصر، از خاصیت `nodeName` استفاده کنید. اما توجه داشته
باشید که این خاصیت نام برچسب را با حروف بزرگ برمی گرداند. از متدهای رشته،
`toLowerCase` یا `toUpperCase` برای درست کردن آن استفاده کنید.

{{if interactive

```{lang: "text/html", test: no}
<h1>Heading with a <span>span</span> element.</h1>
<p>A paragraph with <span>one</span>, <span>two</span>
  spans.</p>

<script>
  function byTagName(node, tagName) {
    // Your code here.
  }

  console.log(byTagName(document.body, "h1").length);
  // → 1
  console.log(byTagName(document.body, "span").length);
  // → 3
  let para = document.querySelector("p");
  console.log(byTagName(para, "span").length);
  // → 2
</script>
```
if}}

{{hint

{{index "getElementsByTagName method", recursion}}

ساده‌ترین روش پیاده‌سازی این راه‌حل استفاده از یک تابع بازگشتی است، شبیه به
[`talksAbout` تابع](dom#talksAbout) که پیش‌تر در این فصل تعریف گردید.

{{index concatenation, "concat method", closure}}

می‌توانید عناصر آرایه‌ی تولیدی را به وسیله‌ی فراخوانی تابع `byTagname` به صورت
بازگشتی به هم بچسبانید تا خروجی را تولید کنید. یا می توانید تابعی درونی تعریف
کنید که خودش را به صورت بازگشتی فراخوانی کند که این تابع به یک متغیر آرایه که در
تابع بیرونی‌اش تعریف شده دسترسی دارد و می تواند عناصری که پیدا‌ می‌کند را به آن
اضافه نماید. فراموش نکنید که باید تابع درونی را یک بار از تابع بیرونی فراخوانی
کنید تا روند کار شروع شود.

{{index "nodeType property", "ELEMENT_NODE code"}}

تابع بازگشتی باید نوع گره را بررسی کیند. در اینجا فقط مایلیم تا گره‌ نوع 1
(`Node.ELEMENT_NODE`) را در نظر بگیریم. برای این نوع گره‌ها، ما باید فرزندانشان
را پیمایش کنیم و برای هر فرزند، مشاهده کنیم که آیا با پرس‌وجوی‌ ما مطابقت دارد
یا خیر و همچنین یک فراخوانی بازگشتی روی آن نیز داشته باشیم تا فرزندان آن را نیز
پوشش داده باشیم.

hint}}

### کلاه گربه

{{index "cat's hat (exercise)", [animation, "spinning cat"]}}

پویانمایی [ایجاد شده در قبل](dom#animation) را توسعه دهید تا گربه و کلاهش
(<bdo>`<img src="img/hat.png">`</bdo>) هر کدام در جهت مخالف هم بچرخند.

یا کاری کنید که کلاه دور گربه بچرخد. یا پویانمایی را به صورتی که جالب باشد تغییر دهید.

{{index "absolute positioning", "top (CSS)", "left (CSS)", "position (CSS)"}}

برای ساده سازی روند موقعیت دهی چند شیء، احتمالا ایده‌ی خوبی است که به سراغ موقعیت
دهی مطلق برویم. به این معنا که `top` و `left` بر اساس گوشه‌ی چپ و بالای سند محاسبه
بشوند. برای جلوگیری از مختصات منفی، که باعث می شود که تصاویر به بیرون از فضای
قابل مشاهده صفحه منتقل شوند، می توانید یک عدد مشخص و ثابت را به مقادیر موقعیت
ها اضافه کنید.

{{if interactive

```{lang: "text/html", test: no}
<style>body { min-height: 200px }</style>
<img src="img/cat.png" id="cat" style="position: absolute">
<img src="img/hat.png" id="hat" style="position: absolute">

<script>
  let cat = document.querySelector("#cat");
  let hat = document.querySelector("#hat");

  let angle = 0;
  let lastTime = null;
  function animate(time) {
    if (lastTime != null) angle += (time - lastTime) * 0.001;
    lastTime = time;
    cat.style.top = (Math.sin(angle) * 40 + 40) + "px";
    cat.style.left = (Math.cos(angle) * 200 + 230) + "px";

    // Your extensions here.

    requestAnimationFrame(animate);
  }
  requestAnimationFrame(animate);
</script>
```

if}}

{{hint

<bdo>`Math.cos`</bdo> و <bdo>`Math.sin`</bdo> زاویه‌ها را در واحد رادیان
اندازه‌گیری می کنند، جایی‌که یک دایره‌ی کامل برابر با <bdo>2π</bdo> می‌باشد.
برای یک زاویه‌ی داده شده، می توانید برای به‌دست آوردن زاویه‌ی مخالف، نصف
<bdo>2π</bdo> که می‌شود <bdo>`Math.PI`</bdo>. این کار برای قرار دادن کلاه در طرف دیگر دایره کاربرد دارد.


hint}}
