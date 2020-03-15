{{meta {load_files: ["code/chapter/16_game.js", "code/levels.js", "code/chapter/17_canvas.js"], zip: "html include=[\"img/player.png\", \"img/sprites.png\"]"}}}

# ترسیم روی Canvas

{{quote {author: "M.C. Escher", title: "cited by Bruno Ernst in The Magic Mirror of M.C. Escher", chapter: true}

نقاشی فریب است.

quote}}

{{index "Escher, M.C."}}

{{figure {url: "img/chapter_picture_17.jpg", alt: "Picture of a robot arm drawing on paper", chapter: "framed"}}}

{{index CSS, "transform (CSS)", [DOM, graphics]}}

مرورگرها روش‌های متعددی برای نمایش عناصر گرافیکی را در اختیار ما می گذارند. ساده‌ترین راه استفاده از سبک‌های CSS برای رنگ‌دهی و موقعیت‌دهی عناصر معمول DOM می‌باشد.
این روش می تواند شما را از مسیر نسبتا دور کند، همانطور که بازی ساخته شده در
[فصل قبل](game) نشان داد. با افزودن تصاویر پس‌زمینه نیمه شفاف به گره‌ها، می توانیم گره‌ها
را دقیقا تبدیل به چیزی کنیم که لازم داریم. حتی می شود که عناصر را با استفاده از
دستور `transform` در CSS بچرخانیم یا تغییر شکل دهیم.

اما به هر حال ما از DOM برای کاری استفاده می کنیم که برای آن طراحی نشده است.
بعضی کارها مثل ترسیم یک خط بین دو نقطه‌ی دلخواه، کاری به شدت ناهمگون با ماهیت
عناصر HTML معمولی است.

{{index SVG, "img (HTML tag)"}}

دو گزینه‌ی دیگر پیش روی ما قرار داد. روش اول استفاده از DOM اما با بکارگیری تصاویر برداری مقیاس‌پذیر (SVG) نسبت به HTML است. می توانید SVG را به عنوان گویشی برای نشانه‌گذاری سند اما با تمرکز بر اشکال به جای متون در نظر گرفت. می توانید یک سند SVG را مستقیما درونی یک سند HTML قرار دهید یا آن را در یک برچسب <bdo>`<img>`</bdo> قرار دهید.

{{index clearing, [DOM graphics], [interface, canvas]}}

گزینه‌ی دوم استفاده از _((canvas))_ است. یک canvas یک عنصر ‌DOM است که یک تصویر
را کپسوله سازی می کند. این عنصر یک رابط برنامه نویسی برای ترسیم اشکال در فضای
اشغال شده توسط آن را فراهم می سازد. تفاوت اصلی بین یک canvas و یک تصویر SVG
این است که در SVG تعریف اصلی اشکال حفظ می شود در نتیجه می توان آن ها را در هر
زمان حرکت یا تغییر اندازه داد. یک canvas، در سوی دیگر، اشکال را به‌محض اینکه ترسیم شدند، به پیکسل‌ها
(نقطه‌های رنگی روی یک محل تصویر)  تبدیل می کند و چیزی که این
پیکسل‌ها نمایندگی می کنند را جایی نگه‌داری نمی‌کند. تنها راهی که برای حرکت دادن یک
شکل درون یک canvas وجود دارد پاک کردن آن (پاک کردن قسمتی از canvas که شکل
آنجا وجود دارد) و ترسیم دوباره‌ی شکل در جایگاه جدید است.

## SVG


این کتاب به جزئیات کار با SVG نمی پردازد، اما به طور مختصر با نحوه‌ی عملکرد آن
آشنا می شویم. [در پایان این فصل](canvas#graphics_tradeoffs)، به ملاحظاتی خواهیم پرداخت که در هنگام انتخاب مکانیزم ترسیم برای اپلیکیشن نیاز است در نظر گرفته شود.

این یک سند HTML است که حاوی یک تصویر SVG ساده می باشد.

```{lang: "text/html", sandbox: "svg"}
<p>Normal HTML here.</p>
<svg xmlns="http://www.w3.org/2000/svg">
  <circle r="50" cx="50" cy="50" fill="red"/>
  <rect x="120" y="5" width="90" height="90"
        stroke="blue" fill="none"/>
</svg>
```

{{index "circle (SVG tag)", "rect (SVG tag)", "XML namespace", XML, "xmlns attribute"}}

خصیصه‌ی `xmlns` باعث می شود که یک عنصر (به همراه عناصر فرزندش) به "فضای نام XML"
متفاوتی تغییر کند. این فضای نام، که توسط یک URL شناسایی می شود، گویشی که در
سند با آن صحبت می کنیم را مشخص می کند. برچسب‌های <bdo>`<circle>`</bdo> و <bdo>`<rect>`</bdo> که در HTML وجود ندارند، در SVG معنای خاصی دارند – این برچسب‌ها با استفاده از سبک و موقعیتی
که در خصیصه‌هایشان مشخص می شود اشکالی را ترسیم می کنند.

{{if book

سند ما به این شکل نمایش داده می شود:

{{figure {url: "img/svg-demo.png", alt: "An embedded SVG image",width: "4.5cm"}}}

if}}

{{index [DOM, graphics]}}

این برچسب‌ها عناصر DOM را ایجاد می کنند، درست مثل برچسب های HTML که اسکریپت‌ها می
توانند با آن‌ها کار کنند. به عنوان مثال، این کد عنصر <bdo>`<circle>`</bdo> را تغییر می دهد تا
رنگش خاکستری شود:


```{sandbox: "svg"}
let circle = document.querySelector("circle");
circle.setAttribute("fill", "cyan");
```

## عنصر Canvas

{{index [canvas, size], "canvas (HTML tag)"}}

عناصر گرافیکی canvas را می‌توان درون یک عنصر <bdo>`<canvas>`</bdo> ترسیم کرد. می توانید به این
عنصر خصیصه‌های `width` و `height` را اضافه کنید تا اندازه‌ی آن به پیکسل تعیین شود.

یک canvas جدید، تهی است به این معنا که یک فضای خالی را در سند نشان می دهد و
کاملا شفاف است.


{{index "2d (canvas context)", "webgl (canvas context)", OpenGL, [canvas, context], dimensions, [interface, canvas]}}

برچسب <bdo>`<canvas>`</bdo> برای این منظور تعریف شده است که سبک‌های مختلف ترسیم را
پشتیبانی کند. برای اینکه به یک محیط ترسیم واقعی دسترسی داشته باشیم ، ابتدا نیاز
داریم تا یک بستر (_((context))_) تعریف کنیم، شیئی که متدهایش رابط ترسیم را فراهم می
سازند. در حال حاضر دو سبک رایج ترسیم پشتیبانی می شود: <bdo>`"2d"`</bdo> برای گرافیک‌های دوبعدی
و “webgl” برای گرافیک‌های سه بعدی با رابط OpenGL.

{{index rendering, graphics, efficiency}}

این کتاب WebGL را پوشش نمی دهد – فقط به دوبعدی خواهیم پرداخت. اما اگر به گرافیک
سه بعدی علاقه دارید پیشنهاد می کنم که WebGL را بررسی کنید. در WebGL رابط مستقیمی
به سخت‌افزار گرافیکی وجود دارد که به شما امکان می دهد که حتی صحنه‌های پیچیده را با
استفاده از جاوااسکریپت به خوبی رندر یا تولید کنید.

{{index "getContext method", [canvas, context]}}

برای ایجاد یک بستر (context) از متد `getContext` مربوط به <bdo>`<canvas>`</bdo> در DOM استفاده می کنید.

```{lang: "text/html"}
<p>Before canvas.</p>
<canvas width="120" height="60"></canvas>
<p>After canvas.</p>
<script>
  let canvas = document.querySelector("canvas");
  let context = canvas.getContext("2d");
  context.fillStyle = "red";
  context.fillRect(10, 10, 100, 50);
</script>
```

بعد از ایجاد شیء context، در مثال، یک چهارضلعی صد پیکسل در پنجاه پیکسل رسم می‌شود که مختصات
گوشه‌ی بالا-چپ آن برابر <bdo>(10,10)</bdo> است.

{{if book

{{figure {url: "img/canvas_fill.png", alt: "A canvas with a rectangle",width: "2.5cm"}}}

if}}

{{index SVG, coordinates}}

درست مثل HTML (و SVG)، سیستم مختصاتی که canvas استفاده می کند (0,0) را در گوشه‌ی
بالا-چپ قرار می دهد و محور عمودی مثبت، پایین تر از آن در نظر گرفته می شود.
بنابراین (10,10) می شود 10 پیکسل به سمت پایین و راست گوشه‌ی بالا-چپ.


{{id fill_stroke}}

## خطوط و سطوح

{{index filling, stroking, drawing, SVG}}

در رابط canvas، شکل را می توان پر (fill) کرد، یعنی به مساحتش رنگ یا الگو اختصاص
داد، یا می توان دور آن خط کشید (stroke). همین اصطلاحات در SVG هم استفاده می
شوند.

{{index "fillRect method", "strokeRect method"}}

متد `fillRect` یک چهارضلعی را با رنگ پر می کند. این متد ابتدا مختصات طولی و عرضی
گوشه‌ی بالا-چپ چهارضلعی را می‌گیرد، بعد طول و ارتفاع آن را دریافت می کند. یک متد
مشابه دیگر به نام `strokeRect` برای کشیدن خط دور چهارضلعی استفاده می شود.

{{index [state, "of canvas"]}}

هیچکدام از دو متد پارامتر دیگری دریافت نمی کنند. رنگ مورد نظر و ضخامت خط و
مواردی از این دست توسط آرگومان مشخص نمی شوند (که منطقا می بایست انجام می شد)
اما در عوض توسط خاصیت‌های شیء بستر (context) تعیین می شوند.

{{index filling, "fillStyle property"}}

خاصیت `fillStyle` سبک پرشدن اشکال را کنترل می کند. می توان آن را با یک رشته که
نمایانگر یک رنگ خاص است با استفاده از روش مشخص کردن رنگ‌ها در CSS تنظیم کرد.

{{index stroking, "line width", "strokeStyle property", "lineWidth property", canvas}}

خاصیت `strokeStyle` به طور مشابهی کار می کند اما رنگ مشخص شده، برای خط دور شکل استفاده می شود. عرض این خط توسط خاصیت `lineWidth` مشخص می شود که می تواند شامل هر عدد مثبتی باشد.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.strokeStyle = "blue";
  cx.strokeRect(5, 5, 50, 50);
  cx.lineWidth = 5;
  cx.strokeRect(135, 5, 50, 50);
</script>
```

{{if book

کد بالا دو چهارضلعی آبی را ترسیم می کند، یکی با خطی ضخیم تر از دیگری.

{{figure {url: "img/canvas_stroke.png", alt: "Two stroked squares",width: "5cm"}}}

if}}

{{index "default value", [canvas, size]}}

زمانی که `with` و `height` مشخص نمی شوند، مثل مثال بالا، عنصر canvas طول پیش‌فرض 300
پیکسل و ارتفاع 150 پیکسل را خواهد گرفت.

## مسیرها

{{index [path, canvas], [interface, design], [canvas, path]}}

یک مسیر، امتدادی از خطوط است. رابط دوبعد canvas از روش ویژه ای برای توصیف مسیرها
استفاده می کند. این کار به طور کامل توسط اثرات جانبی صورت می گیرد. مسیرها
مقادیری نیستند که بتوان آن ها را ذخیره کرد یا ارسال نمود. در عوض، اگر می خواهید
با مسیرها کار کنید، باید دنباله‌ای از فراخوانی‌ها را برای توصیف شکل آن داشته
باشید.


```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  for (let y = 10; y < 100; y += 10) {
    cx.moveTo(10, y);
    cx.lineTo(90, y);
  }
  cx.stroke();
</script>
```

{{index canvas, "stroke method", "lineTo method", "moveTo method", shape}}

در مثال بالا مسیری را توسط چند خط افقی ایجاد کرده و با استفاده از متد `stroke`
دور آن خط می‌کشد. هر قسمتی که با `lineTo` ایجاد شده است از موقعیت فعلی
مسیر شروع می شود. موقعیت مورد نظر معمولا در انتهای قسمت قبلی قرار دارد مگر اینکه
`moveTo` فراخوانی شده باشد. در آن صورت، بخش بعدی از موقعیتی که به `moveTo` داده شده
است شروع می شود.


{{if book

مسیری که در برنامه‌ی مثال قبل ترسیم شده بود شبیه زیر است:

{{figure {url: "img/canvas_path.png", alt: "Stroking a number of lines",width: "2.1cm"}}}

if}}

{{index [path, canvas], filling, [path, closing], "fill method"}}

زمانی که یک مسیر (با متد `fill`) پر می شود، هر شکل به صورت مجزا پر می شود. یک
مسیر می تواند حاوی اشکال متعددی باشد – هر حرکت `moveTo` یک شکل جدید شروع می کند.
اما لازم است که مسیر بسته باشد ) به این معنا که نقطه‌ی شروع و پایانش یکسان باشد(
تا بتوان آن را پر کرد. اگر مسیر هنوز بسته نشده است خطی از از نقطه‌ی پایان به
نقطه‌ی آغاز وصل می شود و شکلی که توسط یک مسیر بسته ایجاد می شود پر می شود.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  cx.moveTo(50, 10);
  cx.lineTo(10, 70);
  cx.lineTo(90, 70);
  cx.fill();
</script>
```
در مثال بالا یک مثلث توپر کشیده می شود. توجه داشته باشید که فقط دو ضلع از مثلث
صراحتا ترسیم شده اند. ضلع سوم، از گوشه‌ی پایین-راست تا بالا، به صورت ضمنی است
و اگر به مسیر، خطر مرزی (stroke) اختصاص داده می شد، آن‌جا دیده نمی‌شد.

{{if book

{{figure {url: "img/canvas_triangle.png", alt: "Filling a path",width: "2.2cm"}}}

if}}

{{index "stroke method", "closePath method", [path, closing], canvas}}

شما می توانید متد `closePath` را نیز استفاده کنید تا صراحتا یک مسیر را ببندید و
ضلعی واقعی را به نقطه‌ی شروع رسم کنید. این ضلع در هنگام اختصاص خط مرزی به مسیر
رسم می شود.


## خطوط منحنی

{{index [path, canvas], canvas, drawing}}

یک مسیر می تواند شامل خطوط منحنی باشد. رسم این خطوط متاسفانه کمی بیشتر کار می
برد.

{{index "quadraticCurveTo method"}}

متد `quadraticCurveTo` یک منحنی را از نقطه‌ی داده شده ترسیم می نماید. برای تعیین
میزان انحنای خط، این متد یک نقطه‌ی کنترل و یک نقطه‌ی مقصد را دریافت می کند. این
نقطه‌ی کنترل را می توان به عنوان یک خط جذب‌کننده در نظر گرفت که به خط انحنا می
بخشد. خط از میان نقطه‌ی کنترل نخواهد گذشت اما اگر خط مستقیمی بین نقاط ابتدایی و انتهایی رسم شود به سمت نقطه‌ی کنترل انحنا خواهد داشد. مثال زیر
این مفهوم را به تصویر می کشد.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  cx.moveTo(10, 90);
  // control=(60,10) goal=(90,90)
  cx.quadraticCurveTo(60, 10, 90, 90);
  cx.lineTo(60, 10);
  cx.closePath();
  cx.stroke();
</script>
```

{{if book

در این مثال یک مسیر به شکل زیر رسم می شود:

{{figure {url: "img/canvas_quadraticcurve.png", alt: "A quadratic curve",width: "2.3cm"}}}

if}}

{{index "stroke method"}}

یک منحنی درجه دوم از چپ به راست با مرکز کنترل (60,10) رسم می کنیم و سپس دو خط
ضلعی که به سمت آن نقطه‌ی کنترل رسم می شوند و به شروع خط بر‌می‌گردند. شکل نتیجه، کمی
شبیه به نماد Star Trek (مجموعه‌ی پیشتازان فضا) می شود. می توانید اثر این نقطه‌ی کنترل را مشاهده
کنید: خطوط از گوشه‌های پایینی جدا می شوند و به سمت نقطه‌ی کنترل جهت می گیرند و به
سمت نقطه‌ی هدفشان انحنا می یابند.

{{index canvas, "bezierCurveTo method"}}

متد `bezierCurveTo` منحنی مشابهی را رسم می کند. به جای یک نقطه‌ی کنترل، این متد دارای دو نقطه می باشد – برای هر نقطه‌ی پایانی، یک نقطه‌ی کنترل. در اینجا با طرح مشابهی که عملکرد این نوع منحنی را نشان می دهد آشنا می شویم:

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  cx.moveTo(10, 90);
  // control1=(10,10) control2=(90,10) goal=(50,90)
  cx.bezierCurveTo(10, 10, 90, 10, 50, 90);
  cx.lineTo(90, 10);
  cx.lineTo(10, 10);
  cx.closePath();
  cx.stroke();
</script>
```

دو نقطه‌ی کنترل در اینجا جهت دو قسمت انتهایی منحنی را مشخص می کنند. هر چه بیشتر
از نقاط کنترل دور می شویم، درجه‌ی انحنا در آن جهت بیشتر می شود.

{{if book

{{figure {url: "img/canvas_beziercurve.png", alt: "A bezier curve",width: "2.2cm"}}}

if}}

{{index "trial and error"}}

کار کردن با این گونه منحنی ها می تواند سخت باشد – همیشه نمی توان به روشنی نقاط
کنترل شیئی که قصد رسم آن را دارید پیدا نمود. گاهی اوقات می توان آن ها را محاسبه
کرد و گاهی هم باید فقط با آزمایش و خطا آن ها را یافت.

{{index "arc method", arc}}

متد `arc` روشی است برای ترسیم خطی که روی محیط دایره‌ای شکل انحنا می یابد. این
متد یک جفت مختصات برای مرکز قوس، یک شعاع و زوایای شروع و پایان را دریافت می کند.

{{index pi, "Math.PI constant"}}

دو پارامتر آخر این امکان را فراهم می سازند که فقط بخشی از دایره را بتوانیم رسم
کنیم. زوایا در واحد رادیان اندازه‌گیری می شوند نه واحد درجه. این یعنی یک دایره‌ی
کامل دارای زاویه‌ی <bdo>2π</bdo> یا <bdo>`2 * Math.PI`</bdo> می باشد که تقریبا
برابر <bdo>6.28</bdo> است. زاویه از نقطه‌ی سمت راست مرکز دایره شروع به افزایش می
یابد و در جهت خلاف عقربه‌های ساعت حرکت می کند. می توانید از عدد 0 شروع کرده و با
عددی بزرگتر از <bdo>2π</bdo> (مثلا 7) رسم یک دایره‌ی کامل را تکمیل کنید.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  // center=(50,50) radius=40 angle=0 to 7
  cx.arc(50, 50, 40, 0, 7);
  // center=(150,50) radius=40 angle=0 to ½π
  cx.arc(150, 50, 40, 0, 0.5 * Math.PI);
  cx.stroke();
</script>
```

{{index "moveTo method", "arc method", [path, " canvas"]}}

تصویر تولید شده شامل خطی است که از سمت راست یک دایره‌ی کامل (اولین فراخوانی به
`arc`) به سمت راست تصویر یک چهارم دایره (فراخوانی دوم) کشیده شده است. شبیه دیگر
متدهای رسم مسیر، خطی که توسط `arc` ترسیم می شود به قسمت قبلی مسیر متصل می شود. برای
جلوگیری از این کار می توانید از `moveTo` استفاده کنید یا مسیر جدیدی را ترسیم کنید.

{{if book

{{figure {url: "img/canvas_circle.png", alt: "Drawing a circle",width: "4.9cm"}}}

if}}

{{id pie_chart}}

## رسم یک نمودار کیکی (pie chart)

{{index "pie chart example"}}

تصور کنید که به تازگی شغلی در شرکت EconomiCorp Ince پیدا کرده اید و اولین کاری
که به شما سپرده می شود این باشد که یک نمودار کیکی برای نتایج رضایت‌سنجی مشتریان
رسم کنید.

متغیر `result` حاوی آرایه‌ای از اشیاء است که نتایج نظرسنجی را نشان می دهد.


```{sandbox: "pie", includeCode: true}
const results = [
  {name: "Satisfied", count: 1043, color: "lightblue"},
  {name: "Neutral", count: 563, color: "lightgreen"},
  {name: "Unsatisfied", count: 510, color: "pink"},
  {name: "No comment", count: 175, color: "silver"}
];
```

{{index "pie chart example"}}

برای رسم یک نمودار کیکی باید تعدادی برش کیک که هر کدام از یک قوس و دو خط از مرکز
آن قوس تشکیل شده اند رسم کنیم. می توانیم زاویه‌ای که توسط هر قوس اشغال می شود را
با تقسیم کل دایره (2π) بر مجموع تعداد پاسخ‌ها و ضرب آن عدد ( زاویه مربوط به هر
پاسخ) در تعداد افرادی که یک گزینه‌ی مشخص را انتخاب کرده اند بدست بیاوریم.

```{lang: "text/html", sandbox: "pie"}
<canvas width="200" height="200"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  let total = results
    .reduce((sum, {count}) => sum + count, 0);
  // Start at the top
  let currentAngle = -0.5 * Math.PI;
  for (let result of results) {
    let sliceAngle = (result.count / total) * 2 * Math.PI;
    cx.beginPath();
    // center=100,100, radius=100
    // from current angle, clockwise by slice's angle
    cx.arc(100, 100, 100,
           currentAngle, currentAngle + sliceAngle);
    currentAngle += sliceAngle;
    cx.lineTo(100, 100);
    cx.fillStyle = result.color;
    cx.fill();
  }
</script>
```

{{if book
این کد نمودار زیر را رسم می کند:

{{figure {url: "img/canvas_pie_chart.png", alt: "A pie chart",width: "5cm"}}}

if}}

اما نموداری که اطلاعاتی در مورد هر برش نمایش نمی دهد زیاد کاربردی نیست. لازم است راهی برای رسم متن
روی canvas پیدا کنیم.

## متن

{{index stroking, filling, "fillStyle property", "fillText method", "strokeText method"}}

در یک بستر (context) ترسیم دو بعدی، متدی به نام `fillText` و `strokeText` در دسترس است. متد
دوم برای رسم خط مرزی برای حروف می تواند کاربرد داشته باشد اما معمولا متدی که
استفاده می شود `fillText` است. این متد فضای حروف را با سبکی که توسط `fillStyle` کنونی
مشخص می شود، پر می کند.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.font = "28px Georgia";
  cx.fillStyle = "fuchsia";
  cx.fillText("I can draw text, too!", 10, 50);
</script>
```

می توانید اندازه، سبک و قلم متن را با خاصیت font مشخص نمایید. در این مثال فقط
اندازه‌ی قلم و نام خانواده‌ی آن مشخص می شود. همچنین برای انتخاب یک سبک می توانید به
ابتدای این رشته مقدار `italic` یا `bold` را اضافه نمایید.

{{index "fillText method", "strokeText method", "textAlign property", "textBaseline property"}}

دو آرگومان آخر `fillText` و `strokeText`، موقعیتی که در آن نوشته ترسیم می شود را مشخص
می کنند. به صورت پیش‌فرض این دو آرگومان موقعیت شروع خط زمینه متن را مشخص می کنند
که خطی است که حروف روی آن می ایستند البته بدون در نظر گرفتن قسمت‌های بیرون‌زده در
حروفی مثل j یا p. می توانید موقعیت افقی را با تنظیم خاصیت `textAlign` به `"end"` یا
`"center"` و موقعیت عمودی را با تنظیم `textBaseline` به `"top"` ، ‍`"middle"` یا `"bottom"`
تغییر دهید.

{{index "pie chart example"}}

در قسمت [تمرین‌ها](canvas#exercise_pie_chart) به مشکل افزودن متن به نمودار کیکی باز خواهیم گشت.

## تصاویر

{{index "vector graphics", "bitmap graphics"}}

در گرافیک کامپیوتری بین تصاویر برداری (vector) و تصاویر نقشه‌بیتی (bitmap) تفاوت قائل
می شوند. تصاویر برداری همان‌هایی هستند که در این فصل به رسم آن‌ها می پرداختیم – یک
تصویر را با توصیف اشکالی به شکلی منطقی مشخص می کردیم. تصاویر گرافیکی بیتی، از
سوی دیگر، اشکال واقعی را مشخص نمی کنند بلکه با اطلاعات پیکسل‌ها کار می کنند (
ناحیه‌هایی از نقاط رنگ شده).

{{index "load event", "event handling", "img (HTML tag)", "drawImage method"}}

متد `drawImage` این امکان را به ما می دهد تا داده‌های پیکسلی را روی canvas ترسیم کنیم.
این داده‌های پیکسلی می توانند ریشه در یک عنصر <bdo>`<img>`</bdo> داشته باشند یا متعلق به
canvas دیگری باشند. مثال پیش رو یک عنصر آزاد <bdo>`<img>`</bdo> را ایجاد کرده و یک فایل عکس
را درون آن بارگیری می کند. اما نمی تواند عکس مورد مورد نظر را شروع به ترسیم کند
چرا که مرورگر ممکن است هنوز آن را بارگیری نکرده باشد. برای حل این مشکل، یک
گرداننده برای رخداد `"load"` ثبت می کنیم تا بعد از بارگیری عکس آن را رسم کند.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  let img = document.createElement("img");
  img.src = "img/hat.png";
  img.addEventListener("load", () => {
    for (let x = 10; x < 200; x += 30) {
      cx.drawImage(img, x, 10);
    }
  });
</script>
```

{{index "drawImage method", scaling}}

به صورت پیشفرض، `drawImage` تصویر را در اندازه‌ی اصلی‌اش رسم می کند. همچنین می
توانید به آن دو آرگومان اضافی ارسال کنید تا طول و عرض متفاوتی داشته باشد.

زمانی که به تابع `drawImage` _نه_ (9) آرگومان ارسال شود، می توان از آن برای ترسیم بخش
خاصی از یک عکس استفاده کرد. آرگومان های دوم تا پنجم ناحیه‌ای چهارضلعی شکلی از عکس
منبع که باید کپی بشود را مشخص می کنند (x،y،width و height) و آرگومان‌های ششم تا
نهم ناحیه‌ای (روی canvas) که چهارضلعی مشخص شده قرار است قرار بگیرد را مشخص می
کنند.


{{index "player", "pixel art"}}

می توان از این متد برای قرار دادن عناصر تصویری متعدد درون یک فایل تصویر
(sprite) و ترسیم بخشی مورد نیاز استفاده کرد. به عنوان مثال، تصویر زیر را در
اختیار داریم که که شخصیت یک بازی را در حالت های مختلف نشان می دهد.

{{figure {url: "img/player_big.png", alt: "Various poses of a game character",width: "6cm"}}}

{{index [animation, "platform game"]}}

با ترسیم متوالی حالت شخصیت، می توانیم یک پویانمایی از راه رفتن را به نمایش
بگذاریم.

{{index "fillRect method", "clearRect method", clearing}}

برای متحرک‌سازی یک تصویر روی یک canvas متد `clearRect` مفید است. این متد مشابه
`fillRect` عمل می کند با این تفاوت که به جای رنگ‌کردن یک ناحیه با حذف پیکسل‌های رسم
شده‌ی قبلی باعث می شود که آن ناحیه شفاف شود.

{{index "setInterval function", "img (HTML tag)"}}

می دانیم که در sprite، هر زیرتصویر، دارای 24 پیکسل طول و 30 پیکسل ارتفاع می
باشد. کد زیر تصاویر را بارگیری کرده و یک وقفه‌ی زمانی برای رسم فریم بعدی تنظیم
می کند:

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  let img = document.createElement("img");
  img.src = "img/player.png";
  let spriteW = 24, spriteH = 30;
  img.addEventListener("load", () => {
    let cycle = 0;
    setInterval(() => {
      cx.clearRect(0, 0, spriteW, spriteH);
      cx.drawImage(img,
                   // source rectangle
                   cycle * spriteW, 0, spriteW, spriteH,
                   // destination rectangle
                   0,               0, spriteW, spriteH);
      cycle = (cycle + 1) % 8;
    }, 120);
  });
</script>
```

{{index "remainder operator", "% operator", [animation, "platform game"]}}

متغیر `cycle` موقعیت ما را در پویانمایی رصد می کند. در هر فریم، این متغیر افزایش
می یابد بعد به بازهی 0 تا 7 دوباره به وسیله‌ی عملگر باقیمانده بر می گردد . این
متغیر بعد برای محاسبه مختصات طولی آن sprite برای حالت فعلی شخصیت در تصویر
استفاده می شود.

## تغییر شکل

{{index transformation, mirroring}}

{{indexsee flipping, mirroring}}

چه می شود اگر بخواهیم که شخصیت ما به جای حرکت به راست به سمت چپ حرکت کند؟ البته
مجموعه‌ی دیگری از تصاویر را رسم کنیم. اما می توان همچنین canvas را طوری تنظیم کرد
که تصاویر را به سمت دیگر رسم کند.

{{index "scale method", scaling}}

فراخوانی متد `scale` موجب می شود که هرچیزی که بعد از آن رسم شود تغییر اندازه دهد.
این متد دو پارامتر را دریافت می کند، یک پارامتر برای اندازه‌ی افقی و دیگری برای
تغییر عمودی.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.scale(3, .5);
  cx.beginPath();
  cx.arc(50, 50, 40, 0, 7);
  cx.lineWidth = 3;
  cx.stroke();
</script>
```

{{if book

با فراخوانی `scale` ، دایره‌ی ما سه برابر عریض تر و ارتفاعش نصف شد.

{{figure {url: "img/canvas_scale.png", alt: "A scaled circle",width: "6.6cm"}}}

if}}

{{index mirroring}}

تغییر اندازه در همه‌ی قسمت‌های تصویر رسم شده اعمال می شود شامل ضخامت خط که با
توجه به اعداد مشخص شده کشیده یا فشرده می شود. اگر این تغییر با عددی منفی انجام
شود باعث می شود که تصویر وارونه شود. این وارونگی نسبت به نقطه‌ی <bdo>(0,0)</bdo>
رخ می دهد که به این معنا است که جهت سیستم مختصات نیز وارونه می شود. با اعمال
تغییر اندازه‌ی <bdo>-1</bdo>، شکلی در موقعیت طولی 100 رسم شده در جایی قرار می
گیرد که سابقا <bdo>-100</bdo> بوده است.

{{index "drawImage method"}}

بنابراین برای اینکه یک تصویر را وارونه کنیم، نمی توان فقط
<bdo>`cx.scale(-1,1)`</bdo> را قبل از فراخوانی `drawImage` اضافه کرد چرا که این کار باعث می شود که
تصویر بیرون از ناحیه canvas قرار گیرد، جایی که دیگر قابل مشاهده نخواهد بود. برای
رفع این مشکل می توانید مختصات داده شده به `drawImage` را تغییر دهید و تصویر را
در موقعیت طولی <bdo>-50</bdo> به جای 0 رسم کنید. یک راه حل دیگر هم، که در آن نیازی نیست
تغییر در کد ترسیم برای تغییر اندازه اعمال شود، این است که محوری که تغییر اندازه
در آن رخ می دهد را تغییر دهیم.

{{index "rotate method", "translate method", transformation}}

متدهای دیگری در کنار `scale` وجود دارند که روی سیستم مختصات در canvas اثر می
گذارند. می توانید متعاقبا تصاویر رسم شده را به وسیله‌ی متد `rotate` بچرخانید یا به
وسیله متد `translate` حرکت دهید. نکته‌ی جالب – و گیج کننده – این است که این
تغییرشکل‌دادن‌ها انباشته می شوند به این معنا که هر کدام متناسب و با توجه به تغییر
شکل قبلی صورت می‌گیرد.

{{index "rotate method", "translate method"}}

بنابراین اگر دوبار و هر بار به اندازه‌ی 10 پیکسل به صورت افقی تصویر را جابجا
کنیم (با translate)، همه چیز 20 پیکسل در سمت راست رسم می شوند. اگر ابتدا مرکز
سیستم مختصات را به نقطه‌ی <bdo>(50,50)</bdo> منتقل کنیم سپس 20 درجه (حدود <bdo>0.1π</bdo> رادیان)
بچرخانیم، آن چرخش حول نقطه‌ی <bdo>(50,50)</bdo> رخ خواهد داد.

{{figure {url: "img/transform.svg", alt: "Stacking transformations",width: "9cm"}}}

{{index coordinates}}

اما اگر _ابتداد_ 20 درجه چرخش ایجاد کنیم سپس به انتقال به مقدار <bdo>(50, 50)</bdo>
بپردازیم، انتقال در سیستم مختصات چرخانده شده اعمال می شود و درنتیجه جهت متفاوت
می شود. ترتیبی که تغییرشکل‌ها در آن اعمال می شوند مهم هستند.

{{index axis, mirroring}}

برای وارونه کردن یک تصویر حول خط عمودی در یک نقطه‌ی طولی داده شده (x)، می توان به
صورت زیر عمل کرد:

```{includeCode: true}
function flipHorizontally(context, around) {
  context.translate(around, 0);
  context.scale(-1, 1);
  context.translate(-around, 0);
}
```

{{index "flipHorizontally method"}}

ما محور y را به جایی که قصد داریم انعکاس آنجا رخ دهد منتقل می کنیم، تصویر را
وارونه می کنیم، و در نهایت محور y را به جای مناسب خودش در فضای وارونه‌شده برمی
گردانیم. تصویر زیر مشخص می کند چرا این روش درست کار می کند:

{{figure {url: "img/mirror.svg", alt: "Mirroring around a vertical line",width: "8cm"}}}

{{index "translate method", "scale method", transformation, canvas}}

این تصویر سیستم های مختصات را قبل و بعد از انجام وارونگی نسبت به
خط مرکزی نشان می دهد. مثلث‌ها عددگذاری شده اند تا هر گام را نشان دهند. اگر یک
مثلث را در موقعیت طولی مثبتی رسم می کردیم، به صورت پیش فرض در جایی قرار می گرفت که
مثلث شماره 1 قرار دارد. فراخوانی ابتدایی `flipHorizontally` موجب انتقال به سمت
راست می شود، که ما را به مثلث شماره 2 می رساند. بعد با تغییر اندازه و وارونه‌کردن
مثلث به موقعیت 3 می رسد. این جایی نیست که با وارونه شدن نسبت به خط داده شده می
بایست قرار می گرفت. فراخوانی دوم به تابع `translate` مشکل را حل می کند – این متد
جابجایی اولیه را لغو کرده و موجب می شود مثلث 4 درست جایی که باید ظاهر شود.

اکنون می توانیم یک کاراکتر وارونه را در موقعیت <bdo>(100,0)</bdo> به وسیله‌ی وارونه‌کردن محیط
نسبت به مرکز عمودی کاراکتر رسم کنیم.

```{lang: "text/html"}
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  let img = document.createElement("img");
  img.src = "img/player.png";
  let spriteW = 24, spriteH = 30;
  img.addEventListener("load", () => {
    flipHorizontally(cx, 100 + spriteW / 2);
    cx.drawImage(img, 0, 0, spriteW, spriteH,
                 100, 0, spriteW, spriteH);
  });
</script>
```

## ذخیره و حذف تغییر شکل‌ها

{{index "side effect", canvas, transformation}}

دگرگونی‌ها یا تغییر شکل‌های ایجاد شده باقی می مانند. هرچیزی که بعد از شخصیت وارونه‌شده رسم می کنیم
نیز وارونه می شود. ممکن است این خواسته‌ی ما نباشد.

می توان دگرگونی فعلی را ذخیره کرد، به چندین ترسیم و دگرگونی دیگر پرداخت و سپس
دگرگونی ذخیره شده را بازگرداند. این کار معمولا برای تابعی که به صورت موقت مختصات
سیستم را تغییر می دهد مناسب است. ابتدا، هر تغییر شکلی که کد فراخواننده تابع استفاده
می کرد را ذخیره می کنیم. بعد تابع کارش را انجام می دهد (در وضعیت دگرگونی
موجود)، احتمالا دگرگونی‌های بیشتری اعمال می کند. و در نهایت، به دگرگونی‌ای که با آن
شروع کردیم باز می گردیم.

{{index "save method", "restore method", [state, "of canvas"]}}

متدهای `save` و `restore` روی بستر canvas دوبعدی مدیریت این دگرگونی را به عهده می
گیرند. از نظر مفهومی این متدها یک پشته از حالت های دگرگونی را نگه می دارند. زمانی که `save` را
فراخوانی می کنید، حالت فعلی درون پشته `push` می شود و زمانی که `restore` را فراخوانی
می کنید، وضعیت بالای پشته برداشته شده و به عنوان بستر دگرگونی فعلی استفاده می
شود. می توانید همچنین `resetTransform` را فراخوانی کنید تا کل دگرگونی را بازنشانی
کنید.

{{index "branching recursion", "fractal example", recursion}}

تابع `branch` در مثال پیش رو به شما نشان می دهد که چه کاری می توانید با یک تابع که
دگرگونی را تغییر داده و بعد یک تابع دیگر (در اینجا خودش) را فراخوانی می کند
بکنید، که به ترسیم با دگرگونی داده‌شده ادامه می دهد.

این تابع یک شکل درخت‌گونه با یک خط رسم می کند و مرکز دستگاه مختصات را به پایان
خط منتقل می کند و خودش را دو مرتبه فراخوانی می کند- اول به سمت چپ می چرخد و بعد
به راست. با هر بار فراخوانی طول شاخه‌ی کشیده شده کوتاه می شود و فراخوانی بازگشتی
زمانی که طول به زیر 8 برسد متوقف می شود.

```{lang: "text/html"}
<canvas width="600" height="300"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  function branch(length, angle, scale) {
    cx.fillRect(0, 0, 1, length);
    if (length < 8) return;
    cx.save();
    cx.translate(0, length);
    cx.rotate(-angle);
    branch(length * scale, angle, scale);
    cx.rotate(2 * angle);
    branch(length * scale, angle, scale);
    cx.restore();
  }
  cx.translate(300, 0);
  branch(60, 0.5, 0.8);
</script>
```

{{if book

شکل نتیجه به این صورت خواهد بود.

{{figure {url: "img/canvas_tree.png", alt: "A recursive picture",width: "5cm"}}}

if}}

{{index "save method", "restore method", canvas, "rotate method"}}

اگر فراخوانی‌های `save` و `restore` نمی بودند، فراخوانی بازگشتی دوم به `branch` موجب می
شد که موقعیت و چرخش معادل خروجی اولی فراخوانی بشود. نتیجه به شاخه‌ی فعلی متصل
نمی شد اما به جای اتصال به درونی ترین شاخه، راست ترین شاخه که با اولین فراخوانی
رسم شده بود متصل می شد. شکل نتیجه ممکن بود جالب شود ولی قطعا یک درخت نمی شود.


{{id canvasdisplay}}

## بازگشت به بازی

{{index "drawImage method"}}

اکنون به اندازه‌ی کافی در مورد رسم روی canvas می دانیم تا بتوانیم روی سیستم نمایش
مبتنی بر canvas برای بازی [فصل قبل](game) کار کنیم. سیستم نمایش جدید فقط شامل مستطیل های
رنگی نخواهد بود. بلکه با استفاده از `drawImage` تصاویری را رسم می کنیم که عناصر
بازی را به تصویر بکشند.

{{index "CanvasDisplay class", "DOMDisplay class", [interface, object]}}

یک شیء نمایش دیگری به نام `CanvasDisplay` تعریف می کنیم، که رابطه‌ای مثل
`DOMDisplay` را از [فصل
?](game#domdisplay) مثل متدهای `syncState` و `clear` را پشتیبانی می کند.

{{index [state, "in objects"]}}

شیء ما اطلاعات بیشتری را نسبت به `DOMDisplay` دریافت می کند . به جای استفاده از
موقعیت scroll مربوط به عنصر DOM، میدان دید (viewport) خودش را مدیریت می کند که قسمتی از
مرحله که دیده می شود را مشخص می کند. و در آخر، یک خاصیت `flipPlayer` خواهد داشت تا
حتی زمانی‌که بازیکن ایستاده است، جهت صورتش بر اساس آخرین حرکت تنظیم شود.

```{sandbox: "game", includeCode: true}
class CanvasDisplay {
  constructor(parent, level) {
    this.canvas = document.createElement("canvas");
    this.canvas.width = Math.min(600, level.width * scale);
    this.canvas.height = Math.min(450, level.height * scale);
    parent.appendChild(this.canvas);
    this.cx = this.canvas.getContext("2d");

    this.flipPlayer = false;

    this.viewport = {
      left: 0,
      top: 0,
      width: this.canvas.width / scale,
      height: this.canvas.height / scale
    };
  }

  clear() {
    this.canvas.remove();
  }
}
```
متد `syncState` ابتدا یک میدان‌دید جدید را محاسبه می کند و سپس صحنه‌ی بازی را در
موقعیت مناسب رسم می کند.

```{sandbox: "game", includeCode: true}
CanvasDisplay.prototype.syncState = function(state) {
  this.updateViewport(state);
  this.clearDisplay(state.status);
  this.drawBackground(state.level);
  this.drawActors(state.actors);
};
```

{{index scrolling, clearing}}

برخلاف `DOMDisplay` ، در این سبک نیازی نیست که پس‌زمینه با هر بار به روز رسانی از
نو ترسیم شود. به دلیل اینکه اشکال روی بوم(canvas) همان پیکسل‌ها هستند، بعد از
این که آن ها را ترسیم کردیم، راه خوبی برای حرکت دادن (یا حذفشان) وجود ندارد.
تنها راه به روز رسانی canvas نمایش، پاک کردن و از نو رسم کردن صحنه است. ممکن است
scroll کرده باشیم، که موجب می شود پس‌زمینه در موقعیت متفاوتی قرار بگیرد.

{{index "CanvasDisplay class"}}

متد `updateViewport` شبیه به متد `scrollPlayerIntoView` مربوط به شیء `DOMDisplay` می
باشد. این متد بررسی می کند که بازیکن به لبه‌ی صفحه نزدیک شده باشد که در آن صورت میدان‌دید (viewport) را
 حرکت می دهد.


```{sandbox: "game", includeCode: true}
CanvasDisplay.prototype.updateViewport = function(state) {
  let view = this.viewport, margin = view.width / 3;
  let player = state.player;
  let center = player.pos.plus(player.size.times(0.5));

  if (center.x < view.left + margin) {
    view.left = Math.max(center.x - margin, 0);
  } else if (center.x > view.left + view.width - margin) {
    view.left = Math.min(center.x + margin - view.width,
                         state.level.width - view.width);
  }
  if (center.y < view.top + margin) {
    view.top = Math.max(center.y - margin, 0);
  } else if (center.y > view.top + view.height - margin) {
    view.top = Math.min(center.y + margin - view.height,
                        state.level.height - view.height);
  }
};
```

{{index boundary, "Math.max function", "Math.min function", clipping}}

فراخوانی متدهای <bdo>`Math.max`</bdo> و <bdo>`Math.min`</bdo> موجب می شود
اطمینان کنیم که فضای خالی خارج از طرح مرحله به وجود نیاید. <bdo>`Math.max(x, 0)`</bdo> باعث می
شود که عدد تولیدی کمتر از صفر نباشد. <bdo>`Math.min`</bdo> به طور مشابه گارانتی می کند که یک
مقدار کمتر از مرز مشخصی بماند.

در زمان پاک کردن صفحه، از رنگ متفاوتی بسته به اینکه بازی را برنده شده
باشیم ( رنگی روشن تر) یا باخته باشیم (تاریک‌تر) استفاده می کنیم.

```{sandbox: "game", includeCode: true}
CanvasDisplay.prototype.clearDisplay = function(status) {
  if (status == "won") {
    this.cx.fillStyle = "rgb(68, 191, 255)";
  } else if (status == "lost") {
    this.cx.fillStyle = "rgb(44, 136, 214)";
  } else {
    this.cx.fillStyle = "rgb(52, 166, 251)";
  }
  this.cx.fillRect(0, 0,
                   this.canvas.width, this.canvas.height);
};
```

{{index "Math.floor function", "Math.ceil function", rounding}}

برای رسم یک پس‌زمینه با استفاده از همان ترفندی که در متد `touches` در [فصل
قبل](game#touches) استفاده کردیم به سراغ قطعات مربعی که در میدان‌دید فعلی قرار می
گیرند می رویم.

```{sandbox: "game", includeCode: true}
let otherSprites = document.createElement("img");
otherSprites.src = "img/sprites.png";

CanvasDisplay.prototype.drawBackground = function(level) {
  let {left, top, width, height} = this.viewport;
  let xStart = Math.floor(left);
  let xEnd = Math.ceil(left + width);
  let yStart = Math.floor(top);
  let yEnd = Math.ceil(top + height);

  for (let y = yStart; y < yEnd; y++) {
    for (let x = xStart; x < xEnd; x++) {
      let tile = level.rows[y][x];
      if (tile == "empty") continue;
      let screenX = (x - left) * scale;
      let screenY = (y - top) * scale;
      let tileX = tile == "lava" ? scale : 0;
      this.cx.drawImage(otherSprites,
                        tileX,         0, scale, scale,
                        screenX, screenY, scale, scale);
    }
  }
};
```

{{index "drawImage method", sprite, tile}}

قطعاتی غیر تهی توسط `drawImage` رسم شده اند. تصویر `otherSprites` حاوی عکس‌های عناصر
بازی به جز شخصیت اصلی می باشد. شامل از چپ به راست کاشی دیوار، کاشی گدازه، و
sprite یک سکه.

{{figure {url: "img/sprites_big.png", alt: "Sprites for our game",width: "1.4cm"}}}

{{index scaling}}

ابعداد کاشی‌های پس‌زمینه 20 در 20 می باشد به دلیل اینکه در `DOMDisplay` از همین
ابعاد استفاده کرده ایم. بنابراین میزان جابجایی (offset) برای کاشی‌های گدازه 20 است
(مقدار متغیر `scale`) و این مقدار برای کاشی‌های دیوار 0 خواهد بود.

{{index drawing, "load event", "drawImage method"}}

نیازی نیست که برای بارگیری sprite تصویر زمانی منتظر بمانیم. فراخوانی `drawImage`
با تصویری که هنوز بارگیری نشده نتیجه‌ای نخواهد داشت. بنابراین وقتی در حال
بارگیری تصاویر هستیم، ممکن است برای رسم چند فریم ابتدایی در بازی با مشکل روبرو
شویم؛ اما این مشکل جدی نیست زیرا تصویر آن به آن به روز می شود و
به محض اینکه بارگیری تمام شود صحنه‌ی بازی تکمیل می شود.

{{index "player", [animation, "platform game"], drawing}}

تصویر آدمکی که پیش‌تر نمایش داده شد را برای نمایش بازیکن استفاده خواهیم کرد. کدی که
وظیفه‌ی رسم آن را دارد باید sprite و جهت صورت مناسبی را با توجه به حرکت فعلی بازیکن
انتخاب کند. هشت sprite اول نمایانگر راه‌رفتن شخصیت هستند. زمانی که بازیگر روی
زمین راه می رود، با توجه به زمان، بین این تصاویر انتخاب می کنیم. قصد داریم هر 60
هزارم ثانیه فریم را تغییر دهیم در نتیجه زمان در ابتدا بر 60 تقسیم می گردد. در
زمانی که بازیکن در حالت ایستاده است، نهمین sprite را رسم می کنیم. در زمان انجام
پرش، که وقتی سرعت عمودی صفر نباشد تشخیص داده می شود، از دهمین، راست‌ترین تصویر
sprite استفاده می کنیم.

{{index "flipHorizontally function", "CanvasDisplay class"}}

به دلیل این که spriteها اندکی عریض تر از شیء بازیکن هستند (24 به جای 16) ، -که
برای افزودن کمی فضا برای پاها و دستان شخصیت می باشد — متد باید مختصات طولی و طول (width)
را با مقدار داده شده (`playerXOverlap`) تنظیم کند.

```{sandbox: "game", includeCode: true}
let playerSprites = document.createElement("img");
playerSprites.src = "img/player.png";
const playerXOverlap = 4;

CanvasDisplay.prototype.drawPlayer = function(player, x, y,
                                              width, height){
  width += playerXOverlap * 2;
  x -= playerXOverlap;
  if (player.speed.x != 0) {
    this.flipPlayer = player.speed.x < 0;
  }

  let tile = 8;
  if (player.speed.y != 0) {
    tile = 9;
  } else if (player.speed.x != 0) {
    tile = Math.floor(Date.now() / 60) % 8;
  }

  this.cx.save();
  if (this.flipPlayer) {
    flipHorizontally(this.cx, x + width / 2);
  }
  let tileX = tile * width;
  this.cx.drawImage(playerSprites, tileX, 0, width, height,
                                   x,     y, width, height);
  this.cx.restore();
};
```
متد `drawPlayer` توسط `drawActors` فراخوانی می شود که مسئول ترسیم تمامی بازیگران در
بازی می باشد.

```{sandbox: "game", includeCode: true}
CanvasDisplay.prototype.drawActors = function(actors) {
  for (let actor of actors) {
    let width = actor.size.x * scale;
    let height = actor.size.y * scale;
    let x = (actor.pos.x - this.viewport.left) * scale;
    let y = (actor.pos.y - this.viewport.top) * scale;
    if (actor.type == "player") {
      this.drawPlayer(actor, x, y, width, height);
    } else {
      let tileX = (actor.type == "coin" ? 2 : 1) * scale;
      this.cx.drawImage(otherSprites,
                        tileX, 0, width, height,
                        x,     y, width, height);
    }
  }
};
```
در هنگام رسم چیزی به جز بازیکن اصلی، به نوع آن نگاه می کنیم تا میزان جابجایی لازم
برای پیدا کردن sprite مورد نظر را پیدا کنیم. کاشی گدازه با 20 و سکه با در 40 (
دو برابر `scale`) پیدا می شوند.


{{index viewport}}

لازم است تا موقعیت میدان‌دید را در هنگام محاسبه‌ی موقعیت بازیگر کم کنیم به این
دلیل که موقعیت <bdo>(0,0)</bdo> روی canvas ما به گوشه‌ی بالاچپ میدان دید ارتباط دارد، نه
گوشه‌ی بالاچپ مرحله. همچنین می‌توانستیم از `translate` برای این کار استفاده کنیم. هر
دو روش صحیح است.


{{if interactive

این کار، سیستم نمایش جدید را به `runGame` متصل می کند:


```{lang: "text/html", sandbox: game, focus: yes, startCode: true}
<body>
  <script>
    runGame(GAME_LEVELS, CanvasDisplay);
  </script>
</body>
```

if}}

{{if book

{{index [game, screenshot], [game, "with canvas"]}}

این سیستم نمایش جدید را به سرانجام می‌رساند. بازی شبیه به شکل زیر خواهد شد.

{{figure {url: "img/canvas_game.png", alt: "The game as shown on canvas",width: "8cm"}}}

if}}

{{id graphics_tradeoffs}}

## انتخاب یک رابط گرافیکی

زمانی که لازم است عناصر گرافیکی در مرورگر ایجاد شوند، می توانید بین HTML، SVG و
استفاده از canvas انتخاب کنید. روش واحدی که به بهترین شکل در همه‌ی شرایط مناسب باشد
وجود ندارد. هر گزینه‌ای نقاط قوت و ضعفی دارد.

{{index "text wrapping"}}

استفاده از HTML ساده، مزیت سادگی را به همراه دارد. همچنین این گزینه با متن‌ها به
خوبی یکپارچه می شود. هر دوی SVG و Canvas به شما امکان رسم متن را می دهند اما
برای موقعیت دهی متن یا شکست آن به خطوط جدید در صورت جا نشدن در یک خط کمکی نمی
کنند. در یک تصویر مبتنی بر HTML خیلی آسان تر می توان بلوک‌های متنی را قرار داد.

{{index zooming, SVG}}


از SVG می توان برای تولید گرافیک‌هایی با وضوح بالا که در هر سطحی از بزرگ‌نمایی خوب
به نظر می رسند استفاده کرد. برخلاف HTML، در واقع SVG برای ترسیم طراحی شده است
بنابراین گزینه‌ی مناسبتری برای این کار است.

{{index [DOM, graphics], SVG, "event handling", ["data structure", tree]}}

هر دوی SVG و HTML ساختار داده‌ای را فراهم می سازند (DOM) که نمایانگر تصویر شما
خواهد بود. این باعث می‌شود که بتوان عناصر را پس از ترسیم تغییر داد. اگر نیاز
دارید که به طور مداوم بخش کوچکی از یک تصویر بزرگ را در پاسخ به فعالیت کاربر یا
به دلیل متحرک‌سازی تغییر دهید، استفاده از canvas بدون اینکه کمک شایانی بکند
هزینه‌ی زیادی خواهد داشت. DOM نیز به ما این امکان را می دهد که گرداننده‌های رخداد
موس را روی هر عنصر در تصویر (حتی اشکالی که با SVG رسم شده اند) ثبت کنیم. این کار
با canvas شدنی نیست.

{{index performance, optimization}}

اما روش مبتنی بر پیکسل canvas در مواقعی که تعداد زیادی عناصر کوچک رسم می کنیم
مزیت محسوب می شود. این واقعیت که canvas یک ساختار داده تشکیل نمی دهد بلکه فقط به
طور مداوم در همان سطح پیکسل به ترسیم می پردازد هزینه‌ی کمتری برای هر شکل در
canvas ایجاد می شود.

{{index "ray tracer"}}

همچنین جلوه‌هایی وجود دارند که فقط زمانی قابل اعمال هستند که از روشی مبتنی بر
پیکسل استفاده شده باشد؛ مانند رندر یک صحنه به صورت یک پیکسل در آن
واحد (مثلا با استفاده از روش رهگیری نور (ray tracer)) یا پس‌پردازش یک تصویر با جاوااسکریپت (
مثل تار کردن یا distort).

در بعضی موارد، ممکن است بخواهید چندتا از این تکنیک‌ها را باهم ترکیب کنید. مثلا
ممکن است یک گراف را با SVG یا canvas ترسیم کنید اما اطلاعات متنی را با استفاده
از یک عنصر HTML که روی تصویر موقعیت دهی می‌شود نشان دهید.

{{index display}}

برای برنامه‌هایی که تعداد کاربران زیادی ندارند، زیاد مهم نیست از کدام رابط استفاده می
کنید. صفحه‌ی نمایشی که ما برای بازی‌مان در این فصل ساختیم می توانست با هر کدام از
این سه تکنولوژی گرافیکی پیاده سازی شود چرا که نه نیاز به ترسیم متن است نه
تعاملات با موس یا کار با تعداد بیش از اندازه از عناصر.


## خلاصه

در این فصل به بحث درباره‌ی تکنیک‌های ترسیم گرافیک در مرورگر پرداختیم و تمرکز ما
روی عنصر <bdo>`<canvas>`</bdo> ‌بود.

یک گره‌ی canvas نمایانگر ناحیه‌ای است در سند که برنامه‌ی ما
در آن قسمت به ترسیم خواهد پرداخت. این ترسیم توسط یک شیء بستر (context) ترسیم انجام می شود
که توسط متد `getContext` ایجاد می گردد.

رابط ترسیم دوبعدی (2D) این امکان را به ما می دهد تا اشکال متنوعی را رنگ‌ کرده یا
خط مرزی بدهیم. خاصیت `fillStyle` این بستر (context) نحوه‌ی رنگ‌آمیزی اشکال را مشخص
می کند. خاصیت‌های `strokeStyle` و `lineWidth` نحوه‌ی ترسیم خطوط را کنترل می کنند.

چهارضلعی ها و بخش‌های متنی را می توان با یک فراخوانی متد ترسیم کرد. دو متد
`fillRect` و `strokeRect` برای ترسیم چهارضلعی و متدهای `fillText` و `strokeText` برای
رسم متن استفاده می شوند. برای ترسیم اشکال دلخواه، ابتدا باید یک مسیر ایجاد کنید.

{{index stroking, filling}}

فراخوانی متد `beginPath` باعث ایجاد یک مسیر جدید می شود. چند متد دیگر برای افزودن
خطوط و منحنی‌ها به همین مسیر فراخوانی می شوند. به عنوان مثال، `lineTo` یک خط مستقیم
اضافه می کند. زمانی که یک مسیر به پایان رسید، می توان با متد `fill` آن را پر (رنگ)
کرد یا با استفاده از متد `stroke` دور آن خط مرزی رسم کرد.

حرکت دادن پیکسل‌ها از یک تصویر یا یک canvas دیگر به canvas ما توسط متد `drawImage`
انجام می پذیرد. به صورت پیش‌فرض، این متد کل تصویر مبدا را رسم می کند، اما با مشخص
کردن پارامترهای بیشتر می توانی یک ناحیه‌ی خاص از تصویر را کپی کرد. ما از این روش
برای بازی خودمان و کپی کردن حالت‌های کاراکتر بازی از یک تصویر که شامل همه‌ی حالت
ها بود استفاده کردیم.


دگرگون‌سازی (transformation) این امکان را به شما می دهد که یک شکل را به صورت‌های
متعدد ترسیم کنید. یک بستر ترسیم دوبعدی، دارای شکلی است که می‌توان آن را با استفاده از
`translate`، `scale` و `rotate` تغییر داد. این تغییرات روی تمامی ترسیم‌های بعدی تاثیر
می گذارد. یک حالت دگرگون‌سازی را می توان با استفاده از متد `save` ذخیره کرد و با
متد `restore` بازگردانی کرد.

زمانی که یک تصویر متحرک را روی یک canvas نمایش می دهیم، متد `clearRect` را می توان
برای پاک‌سازی یک قسمت از canvas قبل از ترسیم دوباره استفاده کرد.

## تمرین‌ها

### شکل‌ها

{{index "shapes (exercise)"}}

برنامه‌ای بنویسید که اشکال زیر را روی یک canvas رسم نماید:

{{index rotation}}

1. یک ذوزنقه (یک چهارضلعی که یک طرف آن پهن‌تر است)

2. یک لوزی قرمز (یک چهارگوش که 45 درجه یا <bdo>¼π</bdo> رادیان چرخانده شده است)

3. یک خط زیگزاگی

4. یک مارپیچ که از 100 قسمت خط مستقیم تشکیل شده است

5. یک ستاره‌ی زرد


{{figure {url: "img/exercise_shapes.png", alt: "The shapes to draw",width: "8cm"}}}

زمانی که دو شکل آخر را رسم می کنید ممکن است لازم باشد به توضیحات مربوط به
<bdo>`Math.cos`</bdo> و <bdo>`Math.sin`</bdo> در [فصل ?](dom#sin_cos) رجوع کنید که توضیح می دهد چگونه مختصات روی یک
دایره را به وسیله‌ی این توابع به‌دست بیاورید.

{{index readability, "hard-coding"}}

پیشنهاد من این است که برای هر شکل یک تابع بنویسید. موقعیت را به آن به همراه دیگر
خاصیت‌های اختیاری مثل اندازه یا تعداد نقاط به عنوان پارامتر ارسال کنید. روش دیگر
که نوشتن اعداد به طور مستقیم در بدنه کد است باعث می شود که تغییر دادن و خوانایی
کد سخت شود.


{{if interactive

```{lang: "text/html", test: no}
<canvas width="600" height="200"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");

  // Your code here.
</script>
```

if}}

{{hint

{{index [path, canvas], "shapes (exercise)"}}

آسان ترین روش ترسیم ذوزنقه (1) استفاده از یک مسیر (path) است. مختصات مرکزی مناسبی را
انتخاب کنید و هر یک از چهار گوشه‌ را اطراف آن اضافه نمایید.

{{index "flipHorizontally function", rotation}}

برای ترسیم لوزی (2)، می توان از راه سرراست استفاده از مسیر یا روش جالب اسفاده از یک `rotate` (دگرگونی) استفاده نمود. برای استفاده از چرخش، باید از یک ترفند مانند کاری که در تابع `flipHorizontally` انجام دادیم،‌استفاده کنید. به دلیل اینکه می خواهیم حول مرکز چهارضلعی چرخش صورت گیرد نه پیرامون نقطه‌ی <bdo>(0,0)</bdo>، ابتدا باید به آن نقطه `translate` کنید، سپس چرخش، و دوباره بازگشت به وسیله‌ی translate.

اطمینان حاصل کنید که دگرگونی انجام شده را پس از ترسیم هر شکل بازنشانی (reset) کنید.

{{index "remainder operator", "% operator"}}

برای شماره‌ی (3)، زیگزاگ، استفاده مکرر از فراخوانی‌های `lineTo` برای هر قسمت خط ، مناسب نیست؛ بلکه باید از یک حلقه استفاده کنید. در هر گام تکرار، می توانید یک یا دو قسمت خط (راست و سپس چپ) را ترسیم کنید، که در این صورت باید از (<bdo>`% 2`</bdo>) برای تشخیص زوج بودن شاخص حلقه استفاده کنید تا راست و چپ را مشخص نمایید.

همچنین برای رسم مارپیچ (4) نیز به حلقه نیاز دارید. اگر مجموعه‌ای از نقاط که هر
نقطه پیرامون دایره‌ای به مرکزیت مارپیچ حرکت می‌کنند، رسم کنید، به دایره خواهید
رسید. اگر در طول حلقه، شعاع دایره‌ای که در حال حاضر روی نقطه‌ی فعلی را قرار می
دهید تغییر دهید و بیش از یک مرتبه حرکت کنید، نتیجه‌ی کار یک مارپیچ خواهد شد.

{{index "quadraticCurveTo method"}}

ستاره (5) به وسیله‌ی خطوط `quadraticCurveTo` ترسیم می‌شود. همچنین می توانید آن را به وسیله‌ی خطوط مستقیم رسم کنید. یک دایره را به هشت قسمت برای ستاره‌ای با هشت نقطه تقسیم کنید یا به هر تعدادی که مایل هستید. بین این نقاط خط رسم کنید، انحنا را به سمت مرکز ستاره مشخص کنید. با استفاده از `quadraticCurveTo`، می توانید از مرکز به عنوان نقطه‌ی کنترل استفاده کنید.

hint}}

{{id exercise_pie_chart}}

### نمودار کیکی

{{index label, text, "pie chart example"}}

[پیش‌تر](canvas#pie_chart) در این فصل مثالی از یک برنامه را مشاهده کردیم که یک
نمودار کیکی رسم می کرد. این برنامه را تغییر داده تا نام هر دسته کنار برش
مربوطه در نمودار پدیدار شود. سعی کنید تا روشی پیدا کنید که متن‌ها را به گونه‌ای
مرتب و خودکار موقعیت دهی کند که برای مجموعه‌ی داده‌های دیگر نیز کار کند. می توانید
فرض کنید که دسته‌ها دارای فروانی زیاد و کافی هستند که فضا برای نوشتن برچسب‌هایشان
فراهم باشد.

ممکن است دوباره به توابع <bdo>`Math.sin`</bdo> و <bdo>`Math.cos`</bdo> که در [فصل ?](dom#sin_cos) توضیح داده
شده نیاز داشته باشید.


{{if interactive

```{lang: "text/html", test: no}
<canvas width="600" height="300"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  let total = results
    .reduce((sum, {count}) => sum + count, 0);
  let currentAngle = -0.5 * Math.PI;
  let centerX = 300, centerY = 150;

  // Add code to draw the slice labels in this loop.
  for (let result of results) {
    let sliceAngle = (result.count / total) * 2 * Math.PI;
    cx.beginPath();
    cx.arc(centerX, centerY, 100,
           currentAngle, currentAngle + sliceAngle);
    currentAngle += sliceAngle;
    cx.lineTo(centerX, centerY);
    cx.fillStyle = result.color;
    cx.fill();
  }
</script>
```

if}}

{{hint

{{index "fillText method", "textAlign property", "textBaseline property", "pie chart example"}}

لازم است تا `fillText` را فراخوانی نموده و خاصیت‌های ‍`textAlign` و
`textBaseline` مرتبط با context آن را طوری تنظیم کنید که متن جایی که می خواهید
ظاهر شود.

یک روش روشن برای موقعیت‌دادن برچسب‌ها این است که متن را روی خطی قرار دهید که از مرکز نمودار به سمت میانه‌ی برش می‌رود.

قطعا نمی‌خواهید که متن را مستقیما کنار برش قرار دهید بلکه با چندین پیکسل فاصله کنار نمودار باید نمایش داده شود.

زاویه‌ی این خط برابر است با <bdo>`currentAngle + 0.5 * sliceAngle`</bdo>. کد پیش رو، جایی روی این خط با فاصله‌ی 120 پیکسل از مرکز می یابد:


```{test: no}
let middleAngle = currentAngle + 0.5 * sliceAngle;
let textX = Math.cos(middleAngle) * 120 + centerX;
let textY = Math.sin(middleAngle) * 120 + centerY;
```

برای `textBaseLine`، مقدار `"middle"` احتمالا با این روش مناسب باشد. مقدار `textAlign` بستگی دارد که در حال حاضر در کدام سمت دایره قرار داریم. سمت چپ، باید مقدار آن `"right"` باشد و سمت راست نیز مقدار `"left"` مناسب است که باعث می شود متن از کیک فاصله بگیرد.

{{index "Math.cos function"}}

اگر در به دست آوردن سمت دایره با توجه با زاویه‌ی در دسترس دچار مشکل شدید، به توضیحات مربوط به ‍<bdo>`Math.cos`</bdo> در [فصل
?](dom#sin_cos) رجوع کنید. کسینوس یک زاویه، متختصات x مرتبط با آن را مشخص می کند که سمتی از دایره‌ که در آن قرار داریم را روشن می کند.

hint}}

### جست و خیز توپ

{{index [animation, "bouncing ball"], "requestAnimationFrame function", bouncing}}

با استفاده از تکنیک `requestAnimationFrame` که در [فصل
?](dom#animationFrame) و [فصل ?](game#runAnimation) مشاهده کردیم مستطیلی رسم کنید که یک توپ متحرک درون آن باشد. توپ با سرعتی ثابت حرکت می کند و با برخورد به دیوارهای مستطیل برگشته و جهت حرکتش عوض می شود.

{{if interactive

```{lang: "text/html", test: no}
<canvas width="400" height="400"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");

  let lastTime = null;
  function frame(time) {
    if (lastTime != null) {
      updateAnimation(Math.min(100, time - lastTime) / 1000);
    }
    lastTime = time;
    requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);

  function updateAnimation(step) {
    // Your code here.
  }
</script>
```

if}}

{{hint

{{index "strokeRect method", animation, "arc method"}}

رسم یک مستطیل با استفاده از `strokeRect` کاری آسان است.  یک متغیر برای نگه‌داری اندازه‌ی چهارضلعی یا دو متغیر اگر طول و عرض چهارضلعی شما متفاوت است، تعریف کنید. برای ایجاد یک توپ، از یک مسیر و یک فراخوانی <bdo>`arc(x, y,
radius, 0, 7)`</bdo> استفاده کنید که کمانی رسم می کند که از صفر تا بیش از یک دایره‌ی کامل ادامه خواهد داشت. سپس مسیر را پر کنید.

{{index "collision detection", "Vec class"}}

برای مدل‌سازی موقعیت و سرعت توپ، می توانید از کلاس `Vec` متعلق به [فصل ?](game#vector)[ (که در این صفحه موجود است‌)]{if interactive} استفاده کنید. به این کلاس یک سرعت اولیه که ترجیحا کاملا عمودی یا افقی نباشد، و برای هر فریم آن سرعت را در زمان سپری شده ضرب کنید. زمانی که توپ خیلی به دیوار عمودی نزدیک شد، مولفه‌ی x سرعت آن را معکوس کنید. همین کار را برای مولفه‌ی y آن در هنگام برخورد به دیوار افقی انجام دهید.

{{index "clearRect method", clearing}}

پس از پیداکردن موقعیت و سرعت جدید توپ، از `clearRect` برای پاک کردن صحنه و بازترسیم آن به وسیله‌ی موقعیت جدید استفاده کنید.

hint}}

### پیش‌محاسبه وارونه‌سازی

{{index optimization, "bitmap graphics", mirror}}

یک اشکال که در دگرگون‌سازی (transformation) وجود دارد این است که استفاده از آن باعث می شود رسم
تصاویر بیتی کند شود. موقعیت و اندازه هر پیکسل باید تغییر داده شود و اگرچه محتمل
است که مرورگرها در این مساله بهتر و باهوش تر در آینده عمل کنند، در حال حاضر این
امر باعث می شود که زمان ترسیم یک نقشه‌ی بیتی به شکل محسوسی زیاد شود.

در یک بازی مثل بازی ما، که فقط یک sprite تغییر شکل داده رسم می کنیم، مشکلی به
وجود نمی آورد. اما تصور کنید که لازم باشد صدها کاراکتر یا هزاران ذره‌ی چرخان برای
یک انفجار رسم کنیم.

به دنبال راه حلی بگردید که به ما این امکان را بدهد که یک کارکتر برعکس را بتوانیم
بدون بارگیری فایل‌های تصویری اضافی و بدون فراخوانی `drawImage` برای هر فریم رسم
کنیم.

{{hint

{{index mirror, scaling, "drawImage method"}}

نکته‌ی کلیدی به راه حل این است که ما می توانیم از یک عنصر canvas به عنوان منبع یک تصویر در هنگام استفاده از `drawImage` استفاده کنیم. می توان یک <bdo>`<canvas>`</bdo> اضافه بدون اضافه کردن آن به سند، ایجاد کرد و sprite های وارونه‌شده مان را یک بار در آن رسم نمود. در هنگام رسم یک فریم، کافی است تنها sprite‌های وارونه‌شده را به canvas اصلی کپی کنیم.

{{index "load event"}}

با توجه نمود که تصاویر بلافاصله بارگیری نمی‌شوند. ما عمل وارونه‌سازی را یک بار انجام می دهیم و اگر این کار قبل از بارگیری تصاویر صورت گیرد، چیزی رسم نخواهد شد. یک گرداننده‌ی `"load"` روی تصویر در اینجا می‌تواند برای ترسیم تصاویر وارونه روی canvas اضافه استفاده شود. این canvas را می توان به عنوان منبع ترسیم بلافاصله استفاده نمود (تا زمانی که ما کاراکتر را روی آن رسم کنیم خالی خواهد بود).


hint}}

