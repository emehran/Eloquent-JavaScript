# DOM یا مدل شیء سند

{{quote {author: "Friedrich Nietzsche", title: "Beyond Good and Evil", chapter: true}

Too bad! Same old story! Once you've finished building your house you
notice you've accidentally learned something that you really should
have known—before you started.

quote}}

{{figure {url: "img/chapter_picture_14.jpg", alt: "Picture of a tree with letters and scripts hanging from its branches", chapter: "framed"}}}

{{index drawing, parsing}}

وقتی یک صفحه وب را در مرورگرتان باز می کنید، مرورگر متن HTML صفحه را گرفته و
آن را تفسیر می کند،￼ بسیار شبیه به آنچه تجزیه‌گر ما برای تجزیه‌ی برنامه‌ها در [فصل ?](language#parsing) انجام می داد. مرورگر یک مدل از ساختار سند می سازد و از آن برای نمایش سند روی
صفحه‌ی نمایش استفاده می کند.

{{index "live data structure"}}

این نمایش از سند، یکی از ابزارهایی است که یک برنامه‌ی جاوااسکریپت در جعبه‌ی شنی (sandbox) خود به
آن دسترسی دارد. یک ساختار داده که می‌تواند خوانده شود یا تغییر یابد. ساختار
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

این صفحه دارای ساختار پیش رو می‌باشد:

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

## Changing the document

{{index "side effect", "removeChild method", "appendChild method", "insertBefore method", [DOM, construction], [DOM, modification]}}

Almost everything about the DOM data structure can be changed. The
shape of the document tree can be modified by changing parent-child
relationships. Nodes have a `remove` method to remove them from their
current parent node. To add a child node to an element node, we can
use `appendChild`, which puts it at the end of the list of children,
or `insertBefore`, which inserts the node given as the first argument
before the node given as the second argument.

```{lang: "text/html"}
<p>One</p>
<p>Two</p>
<p>Three</p>

<script>
  let paragraphs = document.body.getElementsByTagName("p");
  document.body.insertBefore(paragraphs[2], paragraphs[0]);
</script>
```

A node can exist in the document in only one place. Thus, inserting
paragraph _Three_ in front of paragraph _One_ will first remove it
from the end of the document and then insert it at the front,
resulting in _Three_/_One_/_Two_. All operations that insert a node
somewhere will, as a ((side effect)), cause it to be removed from its
current position (if it has one).

{{index "insertBefore method", "replaceChild method"}}

The `replaceChild` method is used to replace a child node with another
one. It takes as arguments two nodes: a new node and the node to be
replaced. The replaced node must be a child of the element the method
is called on. Note that both `replaceChild` and `insertBefore` expect
the _new_ node as their first argument.

## Creating nodes

{{index "alt attribute", "img (HTML tag)"}}

Say we want to write a script that replaces all ((image))s (`<img>`
tags) in the document with the text held in their `alt` attributes,
which specifies an alternative textual representation of the image.

{{index "createTextNode method"}}

This involves not only removing the images but adding a new text node
to replace them. Text nodes are created with the
`document.createTextNode` method.

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

Given a string, `createTextNode` gives us a text node that we can
insert into the document to make it show up on the screen.

{{index "live data structure", "getElementsByTagName method", "childNodes property"}}

The loop that goes over the images starts at the end of the list. This
is necessary because the node list returned by a method like
`getElementsByTagName` (or a property like `childNodes`) is _live_.
That is, it is updated as the document changes. If we started from the
front, removing the first image would cause the list to lose its first
element so that the second time the loop repeats, where `i` is 1, it
would stop because the length of the collection is now also 1.

{{index "slice method"}}

If you want a _solid_ collection of nodes, as opposed to a live one,
you can convert the collection to a real array by calling
`Array.from`.

```
let arrayish = {0: "one", 1: "two", length: 2};
let array = Array.from(arrayish);
console.log(array.map(s => s.toUpperCase()));
// → ["ONE", "TWO"]
```

{{index "createElement method"}}

To create ((element)) nodes, you can use the `document.createElement`
method. This method takes a tag name and returns a new empty node of
the given type.

{{index "Popper, Karl", [DOM, construction], "elt function"}}

{{id elt}}

The following example defines a utility `elt`, which creates an
element node and treats the rest of its arguments as children to that
node. This function is then used to add an attribution to a quote.

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

This is what the resulting document looks like:

{{figure {url: "img/blockquote.png", alt: "A blockquote with attribution",width: "8cm"}}}

if}}

## Attributes

{{index "href attribute", [DOM, attributes]}}

Some element ((attribute))s, such as `href` for links, can be accessed
through a property of the same name on the element's ((DOM))
object. This is the case for most commonly used standard attributes.

{{index "data attribute", "getAttribute method", "setAttribute method", attribute}}

But HTML allows you to set any attribute you want on nodes. This can
be useful because it allows you to store extra information in a
document. If you make up your own attribute names, though, such
attributes will not be present as properties on the element's node.
Instead, you have to use the `getAttribute` and `setAttribute` methods
to work with them.

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

It is recommended to prefix the names of such made-up attributes with
`data-` to ensure they do not conflict with any other attributes.

{{index "getAttribute method", "setAttribute method", "className property", "class attribute"}}

There is a commonly used attribute, `class`, which is a ((keyword)) in
the JavaScript language. For historical reasons—some old JavaScript
implementations could not handle property names that matched
keywords—the property used to access this attribute is called
`className`. You can also access it under its real name, `"class"`, by
using the `getAttribute` and `setAttribute` methods.

## Layout

{{index layout, "block element", "inline element", "p (HTML tag)", "h1 (HTML tag)", "a (HTML tag)", "strong (HTML tag)"}}

You may have noticed that different types of elements are laid out
differently. Some, such as paragraphs (`<p>`) or headings (`<h1>`),
take up the whole width of the document and are rendered on separate
lines. These are called _block_ elements. Others, such as links
(`<a>`) or the `<strong>` element, are rendered on the same line with
their surrounding text. Such elements are called _inline_ elements.

{{index drawing}}

For any given document, browsers are able to compute a layout, which
gives each element a size and position based on its type and content.
This layout is then used to actually draw the document.

{{index "border (CSS)", "offsetWidth property", "offsetHeight property", "clientWidth property", "clientHeight property", dimensions}}

The size and position of an element can be accessed from JavaScript.
The `offsetWidth` and `offsetHeight` properties give you the space the
element takes up in _((pixel))s_. A pixel is the basic unit of
measurement in the browser. It traditionally corresponds to the
smallest dot that the screen can draw, but on modern displays, which
can draw _very_ small dots, that may no longer be the case, and a
browser pixel may span multiple display dots.

Similarly, `clientWidth` and `clientHeight` give you the size of the
space _inside_ the element, ignoring border width.

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

Giving a paragraph a border causes a rectangle to be drawn around it.

{{figure {url: "img/boxed-in.png", alt: "A paragraph with a border",width: "8cm"}}}

if}}

{{index "getBoundingClientRect method", position, "pageXOffset property", "pageYOffset property"}}

{{id boundingRect}}

The most effective way to find the precise position of an element on
the screen is the `getBoundingClientRect` method. It returns an object
with `top`, `bottom`, `left`, and `right` properties, indicating the
pixel positions of the sides of the element relative to the top left
of the screen. If you want them relative to the whole document, you
must add the current scroll position, which you can find in the
`pageXOffset` and `pageYOffset` bindings.

{{index "offsetHeight property", "getBoundingClientRect method", drawing, laziness, performance, efficiency}}

Laying out a document can be quite a lot of work. In the interest of
speed, browser engines do not immediately re-layout a document every
time you change it but wait as long as they can. When a JavaScript
program that changed the document finishes running, the browser will
have to compute a new layout to draw the changed document to
the screen. When a program _asks_ for the position or size of
something by reading properties such as `offsetHeight` or calling
`getBoundingClientRect`, providing correct information also requires
computing a ((layout)).

{{index "side effect", optimization, benchmark}}

A program that repeatedly alternates between reading DOM layout
information and changing the DOM forces a lot of layout computations
to happen and will consequently run very slowly. The following code is
an example of this. It contains two different programs that build up a
line of _X_ characters 2,000 pixels wide and measures the time each
one takes.

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

## Styling

{{index "block element", "inline element", style, "strong (HTML tag)", "a (HTML tag)", underline}}

We have seen that different HTML elements are drawn differently. Some
are displayed as blocks, others inline. Some add styling—`<strong>`
makes its content ((bold)), and `<a>` makes it blue and underlines it.

{{index "img (HTML tag)", "default behavior", "style attribute"}}

The way an `<img>` tag shows an image or an `<a>` tag causes a link to
be followed when it is clicked is strongly tied to the element type.
But we can change the styling associated with an element, such
as the text color or underline. Here is an example that uses
the `style` property:

```{lang: "text/html"}
<p><a href=".">Normal link</a></p>
<p><a href="." style="color: green">Green link</a></p>
```

{{if book

The second link will be green instead of the default link color.

{{figure {url: "img/colored-links.png", alt: "A normal and a green link",width: "2.2cm"}}}

if}}

{{index "border (CSS)", "color (CSS)", CSS, "colon character"}}

A style attribute may contain one or more _((declaration))s_, which
are a property (such as `color`) followed by a colon and a value (such
as `green`). When there is more than one declaration, they must be
separated by ((semicolon))s, as in `"color: red; border: none"`.

{{index "display (CSS)", layout}}

A lot of aspects of the document can be influenced by
styling. For example, the `display` property controls whether an
element is displayed as a block or an inline element.

```{lang: "text/html"}
This text is displayed <strong>inline</strong>,
<strong style="display: block">as a block</strong>, and
<strong style="display: none">not at all</strong>.
```

{{index "hidden element"}}

The `block` tag will end up on its own line since ((block element))s
are not displayed inline with the text around them. The last tag is
not displayed at all—`display: none` prevents an element from showing
up on the screen. This is a way to hide elements. It is often
preferable to removing them from the document entirely because it
makes it easy to reveal them again later.

{{if book

{{figure {url: "img/display.png", alt: "Different display styles",width: "4cm"}}}

if}}

{{index "color (CSS)", "style attribute"}}

JavaScript code can directly manipulate the style of an element
through the element's `style` property. This property holds an object
that has properties for all possible style properties. The values of
these properties are strings, which we can write to in order to change
a particular aspect of the element's style.

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

Some style property names contain hyphens, such as `font-family`.
Because such property names are awkward to work with in JavaScript
(you'd have to say `style["font-family"]`), the property names in the
`style` object for such properties have their hyphens removed and the
letters after them capitalized (`style.fontFamily`).

## Cascading styles

{{index "rule (CSS)", "style (HTML tag)"}}

{{indexsee "Cascading Style Sheets", CSS}}
{{indexsee "style sheet", CSS}}

The styling system for HTML is called ((CSS)), for _Cascading Style
Sheets_. A _style sheet_ is a set of rules for how to style
elements in a document. It can be given inside a `<style>` tag.

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

The _((cascading))_ in the name refers to the fact that multiple such
rules are combined to produce the final style for an element. In the
example, the default styling for `<strong>` tags, which gives them
`font-weight: bold`, is overlaid by the rule in the `<style>` tag,
which adds `font-style` and `color`.

{{index "style (HTML tag)", "style attribute"}}

When multiple rules define a value for the same property, the most
recently read rule gets a higher ((precedence)) and wins. So if the
rule in the `<style>` tag included `font-weight: normal`,
contradicting the default `font-weight` rule, the text would be
normal, _not_ bold. Styles in a `style` attribute applied directly to
the node have the highest precedence and always win.

{{index uniqueness, "class attribute", "id attribute"}}

It is possible to target things other than ((tag)) names in CSS rules.
A rule for `.abc` applies to all elements with `"abc"` in their `class`
attribute. A rule for `#xyz` applies to the element with an `id`
attribute of `"xyz"` (which should be unique within the document).

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

The ((precedence)) rule favoring the most recently defined rule
applies only when the rules have the same _((specificity))_. A rule's
specificity is a measure of how precisely it describes matching
elements, determined by the number and kind (tag, class, or ID) of
element aspects it requires. For example, a rule that targets `p.a` is
more specific than rules that target `p` or just `.a` and would thus
take precedence over them.

{{index "direct child node"}}

The notation `p > a {…}` applies the given styles to all `<a>` tags
that are direct children of `<p>` tags. Similarly, `p a {…}` applies
to all `<a>` tags inside `<p>` tags, whether they are direct or
indirect children.

## Query selectors

{{index complexity, CSS}}

We won't be using style sheets all that much in this book.
Understanding them is helpful when programming in the browser, but
they are complicated enough to warrant a separate book.

{{index "domain-specific language", [DOM, querying]}}

The main reason I introduced _((selector))_ syntax—the notation used
in style sheets to determine which elements a set of styles apply
to—is that we can use this same mini-language as an effective way to
find DOM elements.

{{index "querySelectorAll method", "NodeList type"}}

The `querySelectorAll` method, which is defined both on the `document`
object and on element nodes, takes a selector string and returns a
`NodeList` containing all the elements that it matches.

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

Unlike methods such as `getElementsByTagName`, the object returned by
`querySelectorAll` is _not_ live. It won't change when you change the
document. It is still not a real array, though, so you still need to
call `Array.from` if you want to treat it like one.

{{index "querySelector method"}}

The `querySelector` method (without the `All` part) works in a similar
way. This one is useful if you want a specific, single element. It
will return only the first matching element or null when no element
matches.

{{id animation}}

## Positioning and animating

{{index "position (CSS)", "relative positioning", "top (CSS)", "left (CSS)", "absolute positioning"}}

The `position` style property influences layout in a powerful way. By
default it has a value of `static`, meaning the element sits in its
normal place in the document. When it is set to `relative`, the
element still takes up space in the document, but now the `top` and
`left` style properties can be used to move it relative to that normal
place. When `position` is set to `absolute`, the element is removed
from the normal document flow—that is, it no longer takes up space and
may overlap with other elements. Also, its `top` and `left` properties
can be used to absolutely position it relative to the top-left corner
of the nearest enclosing element whose `position` property isn't
`static`, or relative to the document if no such enclosing element
exists.

{{index [animation, "spinning cat"]}}

We can use this to create an animation. The following document
displays a picture of a cat that moves around in an ((ellipse)):

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

The gray arrow shows the path along which the image moves.

{{figure {url: "img/cat-animation.png", alt: "A moving cat head",width: "8cm"}}}

if}}

{{index "top (CSS)", "left (CSS)", centering, "relative positioning"}}

Our picture is centered on the page and given a `position` of
`relative`. We'll repeatedly update that picture's `top` and `left`
styles to move it.

{{index "requestAnimationFrame function", drawing, animation}}

{{id animationFrame}}

The script uses `requestAnimationFrame` to schedule the `animate`
function to run whenever the browser is ready to repaint the screen.
The `animate` function itself again calls `requestAnimationFrame` to
schedule the next update. When the browser window (or tab) is active,
this will cause updates to happen at a rate of about 60 per second,
which tends to produce a good-looking animation.

{{index timeline, blocking}}

If we just updated the DOM in a loop, the page would freeze, and
nothing would show up on the screen. Browsers do not update their
display while a JavaScript program is running, nor do they allow any
interaction with the page. This is why we need
`requestAnimationFrame`—it lets the browser know that we are done for
now, and it can go ahead and do the things that browsers do, such as
updating the screen and responding to user actions.

{{index "smooth animation"}}

The animation function is passed the current ((time)) as an
argument. To ensure that the motion of the cat per millisecond is
stable, it bases the speed at which the angle changes on the
difference between the current time and the last time the function
ran. If it just moved the angle by a fixed amount per step, the motion
would stutter if, for example, another heavy task running on the same
computer were to prevent the function from running for a fraction of a
second.

{{index "Math.cos function", "Math.sin function", cosine, sine, trigonometry}}

{{id sin_cos}}

Moving in ((circle))s is done using the trigonometry functions
`Math.cos` and `Math.sin`. For those who aren't familiar with
these, I'll briefly introduce them since we will occasionally use them
in this book.

{{index coordinates, pi}}

`Math.cos` and `Math.sin` are useful for finding points that lie on a
circle around point (0,0) with a radius of one. Both functions
interpret their argument as the position on this circle, with zero
denoting the point on the far right of the circle, going clockwise
until 2π (about 6.28) has taken us around the whole circle. `Math.cos`
tells you the x-coordinate of the point that corresponds to the given
position, and `Math.sin` yields the y-coordinate. Positions (or
angles) greater than 2π or less than 0 are valid—the rotation repeats
so that _a_+2π refers to the same ((angle)) as _a_.

{{index "PI constant"}}

This unit for measuring angles is called ((radian))s—a full circle is
2π radians, similar to how it is 360 degrees when measuring in
degrees. The constant π is available as `Math.PI` in JavaScript.

{{figure {url: "img/cos_sin.svg", alt: "Using cosine and sine to compute coordinates",width: "6cm"}}}

{{index "counter variable", "Math.sin function", "top (CSS)", "Math.cos function", "left (CSS)", ellipse}}

The cat animation code keeps a counter, `angle`, for the current angle
of the animation and increments it every time the `animate` function
is called. It can then use this angle to compute the current position
of the image element. The `top` style is computed with `Math.sin` and
multiplied by 20, which is the vertical radius of our ellipse. The
`left` style is based on `Math.cos` and multiplied by 200 so that the
ellipse is much wider than it is high.

{{index "unit (CSS)"}}

Note that styles usually need _units_. In this case, we have to append
`"px"` to the number to tell the browser that we are counting in ((pixel))s
(as opposed to centimeters, "ems", or other units). This is easy to
forget. Using numbers without units will result in your style being
ignored—unless the number is 0, which always means the same thing,
regardless of its unit.

## Summary

JavaScript programs may inspect and interfere with the document that
the browser is displaying through a data structure called the DOM.
This data structure represents the browser's model of the document,
and a JavaScript program can modify it to change the visible document.

The DOM is organized like a tree, in which elements are arranged
hierarchically according to the structure of the document. The objects
representing elements have properties such as `parentNode` and
`childNodes`, which can be used to navigate through this tree.

The way a document is displayed can be influenced by _styling_, both
by attaching styles to nodes directly and by defining rules that match
certain nodes. There are many different style properties, such as
`color` or `display`. JavaScript code can manipulate an element's
style directly through its `style` property.

## Exercises

{{id exercise_table}}

### Build a table

{{index "table (HTML tag)"}}

An HTML table is built with the following tag structure:

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

For each _((row))_, the `<table>` tag contains a `<tr>` tag. Inside of
these `<tr>` tags, we can put cell elements: either heading cells
(`<th>`) or regular cells (`<td>`).

Given a data set of mountains, an array of objects with `name`,
`height`, and `place` properties, generate the DOM structure for a
table that enumerates the objects. It should have one column per key
and one row per object, plus a header row with `<th>` elements at the
top, listing the column names.

Write this so that the columns are automatically derived from the
objects, by taking the property names of the first object in the data.

Add the resulting table to the element with an `id` attribute of
`"mountains"` so that it becomes visible in the document.

{{index "right-aligning", "text-align (CSS)"}}

Once you have this working, right-align cells that contain number
values by setting their `style.textAlign` property to `"right"`.

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

You can use `document.createElement` to create new element nodes,
`document.createTextNode` to create text nodes, and the `appendChild`
method to put nodes into other nodes.

{{index "Object.keys function"}}

You'll want to loop over the key names once to fill in the top row and
then again for each object in the array to construct the data rows. To
get an array of key names from the first object, `Object.keys` will be
useful.

{{index "getElementById method", "querySelector method"}}

To add the table to the correct parent node, you can use
`document.getElementById` or `document.querySelector` to find the node
with the proper `id` attribute.

hint}}

### Elements by tag name

{{index "getElementsByTagName method", recursion}}

The `document.getElementsByTagName` method returns all child elements
with a given tag name. Implement your own version of this as a
function that takes a node and a string (the tag name) as arguments
and returns an array containing all descendant element nodes with the
given tag name.

{{index "nodeName property", capitalization, "toLowerCase method", "toUpperCase method"}}

To find the tag name of an element, use its `nodeName` property. But
note that this will return the tag name in all uppercase. Use the
`toLowerCase` or `toUpperCase` string methods to compensate for this.

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

The solution is most easily expressed with a recursive function,
similar to the [`talksAbout` function](dom#talksAbout) defined earlier
in this chapter.

{{index concatenation, "concat method", closure}}

You could call `byTagname` itself recursively, concatenating the
resulting arrays to produce the output. Or you could create an inner
function that calls itself recursively and that has access to an array
binding defined in the outer function, to which it can add the
matching elements it finds. Don't forget to call the ((inner
function)) once from the outer function to start the process.

{{index "nodeType property", "ELEMENT_NODE code"}}

The recursive function must check the node type. Here we are
interested only in node type 1 (`Node.ELEMENT_NODE`). For such
nodes, we must loop over their children and, for each child, see
whether the child matches the query while also doing a recursive call
on it to inspect its own children.

hint}}

### The cat's hat

{{index "cat's hat (exercise)", [animation, "spinning cat"]}}

Extend the cat animation defined [earlier](dom#animation) so that
both the cat and his hat (`<img src="img/hat.png">`) orbit at opposite
sides of the ellipse.

Or make the hat circle around the cat. Or alter the animation in some
other interesting way.

{{index "absolute positioning", "top (CSS)", "left (CSS)", "position (CSS)"}}

To make positioning multiple objects easier, it is probably a good
idea to switch to absolute positioning. This means that `top` and
`left` are counted relative to the top left of the document. To avoid
using negative coordinates, which would cause the image to move
outside of the visible page, you can add a fixed number of pixels to
the position values.

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

`Math.cos` and `Math.sin` measure angles in radians, where a full
circle is 2π. For a given angle, you can get the opposite angle by
adding half of this, which is `Math.PI`. This can be useful for
putting the hat on the opposite side of the orbit.

hint}}
