{{meta {load_files: ["code/chapter/19_paint.js"], zip: "html include=[\"css/paint.css\"]"}}}

# پروژه: یک ویرایشگر پیکسلی

{{quote {author: "خوان میرو", chapter: true}

به رنگ‌های بیشمار پیش رویم نگاه می کنم. به بوم نقاشی خالی‌ام نیز. آن‌گاه، سعی‌ می کنم رنگ‌ها را مانند واژه‌هایی که شعر‌ها را می‌سازند به کار ببرم، مانند نوت‌هایی که به موسیقی شکل می‌دهند.

quote}}

{{index "Miro, Joan", "drawing program example", "project chapter"}}

{{figure {url: "img/chapter_picture_19.jpg", alt: "Picture of a tiled mosaic", chapter: "framed"}}}

هرآنچه برای ساخت یک اپلیکیشن وب ساده مورد نیاز است، در فصل‌های پیشین آمده است. در این فصل، فقط قرار است از آن‌ها استفاده کنیم.

{{index [file, image]}}

اپلیکیشن ما قرار است یک برنامه‌ی ترسیم پیکسلی باشد، جایی که می توانید یک تصویر
را پیکسل به پیکسل تغییر دهید. این کار با دستکاری حالت بزرگ‌نمایی شده‌ی تصویر،
که جدولی از خانه‌های رنگ شده است صورت می گیرد. می توانید از آن برای باز کردن
فایل‌های تصویری و خط خطی کردن روی آن‌ها با موس یا هر وسیله‌ی اشاره‌گر دیگر استفاده کنید و سپس آن را ذخیره نمایید. برنامه کاربردی ما شبیه به تصویر زیر خواهد بود:


{{figure {url: "img/pixel_editor.png", alt: "The pixel editor interface, with colored pixels at the top and a number of controls below that", width: "8cm"}}}

نقاشی روی یک کامپیوتر خیلی جالب است. نیازی نیست نگران ابزار، توانایی یا استعداد خاصی باشید. کافی است فقط شروع به کشیدن کنید.

## مؤلفه‌ها یا اجزاء

{{index drawing, "select (HTML tag)", "canvas (HTML tag)", component}}

رابط برنامه‌ی کاربردی ما، شامل یک `<canvas>` بزرگ در بالای صفحه می باشد که چندین فیلد
فرم نیز در زیر آن قرار گرفته است. کاربر با انتخاب یک ابزار از لیست فیلد `<select>` و
کلیک کردن، لمس کردن یا کشیدن موس روی بوم به ترسیم می پردازد. همچنین ابزارهایی برای رسم تک‌پیکسل‌ها، چهارضلعی‌ها، رنگ‌کردن یک ناحیه، و انتخاب یک رنگ از روی یک تصویر وجود دارد.

{{index [DOM, components]}}

ما رابط ویرایشگر را به شکل تعدادی مؤلفه ساختاردهی می کنیم، اشیائی که مسئول
بخش‌هایی از DOM می باشند و ممکن است حاوی دیگر مؤلفه ها در درون خود باشند.

{{index [state, "of application"]}}

وضعیت (state) برنامه‌ی کاربردی شامل تصویر فعلی، ابزار انتخاب شده و رنگ انتخاب شده می
باشد. ما کارها را طوری تنظیم می کنیم که در نتیجه وضعیت با یک مقدار مشخص شود و
دیگر مؤلفه‌ها همیشه با توجه به وضعیت فعلی شکل بگیرند.

برای پی بردن به اهمیت این موضوع، اجازه بدهید راه دیگر را هم بررسی کنیم – پخش قسمت‌های وضعیت (state)
در سراسر رابط. تا نقطه‌ی مشخصی، این روش، برنامه‌نویسی ساده تری
دارد. می توانیم تنها یک فیلد رنگ در نظر بگیریم و هر زمان که نیاز به دانستن رنگ فعلی داشتیم، مقدار آن را بخوانیم.

اما در ادامه تصمیم می گیریم یک گزینش‌گر رنگ اضافه کنیم – ابزاری که به شما اجازه می دهد
تا با کلیک روی تصویر، رنگ پیکسل داده‌شده را انتخاب کنید. برای اینکه کاری کنیم
تا فیلد رنگ ، رنگ درست را نمایش دهد، ابزار ما بایستی بداند که فیلد رنگ مورد نظر موجود
است و با هر بار انتخاب رنگ جدید آن را به‌روز رسانی کند. اگر جای دیگری را نیز در نظر گرفتید که رنگ انتخاب شده را نمایان کند( ممکن است اشاره‌گر موس برای این کار
در نظر گرفته شده باشد)، باید کد مربوط به تغییر رنگ را نیز به‌روز‌رسانی کنید
تا آن جای جدید را نیز هماهنگ نگه دارید.

{{index modularity}}

در عمل، این کار شما را با مشکل روبرو می کند طوری که هر بخش از رابط لازم است تا
درباره‌ی همه بخش‌های دیگر آگاه باشد، که خیلی ماژولار نیست. برای برنامه‌های کاربردی
کوچک مثل مورد این فصل، ممکن است مشکل خاصی نباشد. برای پروژه های بزرگتر می تواند این
کار به کابوس بزرگی ختم شود.

خوب برای پیشگیری از بروز این کابوس، ما سعی می کنیم که در مورد جریان داده (data flow) خیلی
سختگیرانه عمل کنیم. یک وضعیت وجود دارد و رابط قرار است بر اساس آن وضعیت ترسیم
شود. یک مؤلفه‌ی رابط ممکن است به کارهای کاربر با به‌روز رسانی وضعیت پاسخ دهد، که
در آن نقطه‌، مؤلفه‌ها شانس دارند تا خودشان را با وضعیت جدید هماهنگ سازند.

{{index library, framework}}

در عمل، هر مؤلفه به گونه‌ای تنظیم می شود که زمانی که به آن وضعیت جدیدی داده
شود، این وضعیت را به مؤلفه‌های فرزندش نیز اعلام کند تا آن‌‌هایی که نیاز دارند به‌روز
شوند. درست کردن این قسمت کمی با زحمت همراه است. اجرای مناسب و راحت این بخش، مزیت
و نقطه‌ی برتری خیلی از کتابخانه‌های مربوط به برنامه نویسی مرورگر محسوب می شود. اما
برای برنامه‌ی کوچکی مثل برنامه‌ی ما ، می توانیم بدون داشتن این زیرساخت نیز جلو
برویم.

{{index [state, transitions]}}

به‌روز رسانی‌های وضعیت به شکل اشیاء نمایش داده می شوند که ما آن ها را actions می
نامیم. مؤلفه‌ها ممکن است این گونه اکشن ها را ایجاد کنند و گسیل  (_((dispatch))_) دهند –
آن ها را به تابع مرکزی مدیریت وضعیت ارسال نمایند. آن تابع وضعیت بعدی را محاسبه می
کند که پس از آن ، مؤلفه‌های رابط، خودشان را به وضعیت جدید به‌روز رسانی می‌کنند.

{{index [DOM, components]}}

ما در حال پرداختن به بخش پرکار اجرای یک رابط کاربری و اعمال ساختار به آن هستیم.
اگرچه بخش‌های مرتبط با DOM هنوز پر از اثرات جانبی هستند، با این وجود توسط یک زیرساخت
مفهومی ساده نگهداری می شوند – چرخه‌ی به‌روز رسانی وضعیت. وضعیت مشخص می کند که DOM
چگونه ظاهر یابد، و تنها راهی که رخدادهای DOM دارند برای تغییر وضعیت این است که
اکشن‌ها را به وضعیت مورد نظر گسیل دهند.

{{index "data flow"}}

راه‌های زیادی برای انجام این کار وجود دارد که هر کدام مزایا و معایب خودشان را دارند، اما ایده‌ی اصلی همه‌ی آن ها مشترک است: تغییرات وضعیت بایستی از طریق یک کانال یگانه‌ی مشخص انجام شود نه در همه‌ی قسمت‌های موجود.

{{index "dom property", [interface, object]}}

مؤلفه‌های ما کلاس هایی خواهند بود که با یک رابط مطابقت دارند. سازنده‌ی آن ها ،
وضعیتی را دریافت می کند که ممکن است وضعیت کلی برنامه باشد یا بخشی از آن اگر
نیازی به دسترسی به همه چیز نیست و از آن برای ساختن خاصیت `dom` استفاده می کند،
DOMای که نمایانگر مؤلفه می باشد. اغلب سازنده‌ها مقدارهای دیگری را نیز دریافت می
کنند که در طول زمان ثابت هستند مانند تابعی که می توانند از آن برای گسیل یک اکشن
استفاده کنند.

{{index "syncState method"}}

هر مؤلفه دارای متدی به نام `syncState` می باشد که برای هماهنگ‌سازی آن با یک مقدار
وضعیت جدید استفاده می شود. این متد یک آرگومان دریافت می کند، وضعیت، که از نوع
مشابه اولین آرگومان سازنده‌اش می باشد.

## وضعیت یا State

{{index "Picture class", "picture property", "tool property", "color property", "Matrix class"}}

وضعیت برنامه شیئی خواهد بود که دارای خاصیت‌های `picture،` `tool،` و `color` می باشد.
picture خودش نیز یک شی است که طول، ارتفاع و محتوای پیکسل تصویر را ذخیره می
نماید. پیکسل‌ها درون یک آرایه ذخیره می شوند به همان شکلی که کلاس ماتریس از[فصل
?](object)
عمل می کرد – ردیف به ردیف و از بالا به پایین.

```{includeCode: true}
class Picture {
  constructor(width, height, pixels) {
    this.width = width;
    this.height = height;
    this.pixels = pixels;
  }
  static empty(width, height, color) {
    let pixels = new Array(width * height).fill(color);
    return new Picture(width, height, pixels);
  }
  pixel(x, y) {
    return this.pixels[x + y * this.width];
  }
  draw(pixels) {
    let copy = this.pixels.slice();
    for (let {x, y, color} of pixels) {
      copy[x + y * this.width] = color;
    }
    return new Picture(this.width, this.height, copy);
  }
}
```

{{index "side effect", "persistent data structure"}}

قصد ما این است که بتوانیم با تصویر مانند یک مقدار غیر قابل تغییر رفتار کنیم به
دلایلی که بعدا در ادامه فصل خواهیم گفت. اما گاهی نیز لازم است تا بخشی از پیکسل‌ها
را یکجا تغییر دهیم. برای این که بتوانیم این کار را انجام دهیم کلاس ما متدی به
نام `draw` دارد که آرایه‌ای را می‌گیرد که شامل پیکسل‌های تغییر یافته می‌باشد –
اشیائی با خاصیت‌های `x` ، `y` و `color` – و تصویر جدیدی با آن پیکسل‌های بازنویسی شده
ایجاد کند. این متد از `slice` بدون استفاده از آرگومان بهره می برد تا تمام آرایه‌ی
پیکسل‌ها را کپی کند – شروع slice به صورت پیشفرض برابر 0 و مقدار پیش‌فرض انتهای آن طول آرایه می
باشد.

{{index "Array constructor", "fill method", ["length property", "for array"], [array, creation]}}

متد `empty` از دو ویژگی آرایه‌ها بهره می برد که تاکنون مشاهده نکرده ایم. سازنده
`Array` را می توان با یک عدد برای ساخت یک آرایه تهی با طولی مشخص فراخوانی کرد. و
متد `fill` را می توان برای پر کردن این آرایه با یک مقدار مشخص استفاده کرد. این
متدها برای ساختن آرایه‌ای که در آن همه‌ی پیکسل‌ها رنگ مشابهی دارند استفاده می شود.

{{index "hexadecimal number", "color component", "color field", "fillColor property"}}

رنگ‌ها به عنوان رشته‌هایی که حاوی کدهای رنگ معمول در CSS می باشند – یک علامت هش
(`#`) که بعد از آن شش رقم هگزادسیمال (مبنای 16) قرار می گیرد، دو رقم برای جزء
قرمز، دو رقم برای سبز و دو رقم برای بخش آبی – ذخیره می شوند. این شیوه‌ی نوشتن
رنگ ها مقداری رمزگونه و نامناسب به نظر می رسد اما این فرمتی است که فیلدهای ورودی
رنگ در HTML استفاده می کنند و می توان از آن برای خاصیت `fillColor` مربوط به بستر (context)
ترسیم در canvas استفاده کرد. بنابراین در جایی که قصد استفاده از رنگ‌ها در این
برنامه را داریم، مناسب هستند.

{{index black}}

رنگ سیاه، زمانی اتفاق می افتد که همه‌ی اجزای رنگ برابر صفر باشند و به صورت <bdo>`"#000000"`</bdo> نوشته می شود ، و رنگ صورتی
روشن شبیه به <bdo>`"#ff00ff"`</bdo> نوشته می شود، به صورتی که قسمت رنگ قرمز و آبی در آن در بیشینه‌ی مقدار خود، 255، هستند که به صورت `ff` در هگزادسیمال نوشته می شود ( که از _a_ تا _f_ را به عنوان ارقامی معادل 10 تا 15 در نظر می گیرند).

{{index [state, transitions]}}

ما به رابط کاربری این امکان را می دهیم که actionها را به عنوان اشیائی گسیل (dispatch) دهد که خاصیت‌هایشان، خاصیت‌های وضعیت قبلی را بازنویسی می کنند. فیلد رنگ، زمانی که کاربر آن را تغییر می دهد، می تواند شیئی مثل `{color: field.value}` را گسیل دهد که از آن، تابع به‌روز رسانی زیر می تواند مقدار جدید را محاسبه کند.

{{index "updateState function"}}

```{includeCode: true}
function updateState(state, action) {
  return Object.assign({}, state, action);
}
```

{{index "period character", spread, "Object.assign function"}}

این الگوی نسبتا پیچیده، که در آن از `Object.assign` برای افزودن خاصیت‌های `state` به یک شیء خالی در ابتدا استفاده می کند و بعد بعضی از آن ها را با خاصیت‌هایی از `action` بازنویسی می کند، در کدهای جاوااسکریپتی که از اشیاء غیرقابل تغییر استفاده می کنند معمول است. روش راحت و مناسب تر این کار که در آن از عملگر سه نقطه استفاده می شود تا همه‌ی خاصیت‌های شیء دیگر را در یک عبارت شیء قرار دهد، در آخرین مراحل استاندارد شدن در جاوااسکریپت به سر می برد. با این ویژگی، می توانستید به جای شکل قبلی، بنویسید `{...state, ...action}`. در زمان نوشتن این کتاب، این ویژگی هنوز در همه‌ی مرورگرها پشتیبانی نمی شود.

## ساختن DOM

{{index "createElement method", "elt function", [DOM, construction]}}

یکی از کارهای اصلی که مؤلفه‌های رابط کاربری انجام می دهند ساخت ساختار DOM است. ما قصد نداریم که دوباره به صورت مستقیم از متدهای صریح DOM برای این کار استفاده کنیم، بنابراین نسخه‌ای از تابع `elt` که کمی توسعه یافته است را به کار خواهیم برد:

```{includeCode: true}
function elt(type, props, ...children) {
  let dom = document.createElement(type);
  if (props) Object.assign(dom, props);
  for (let child of children) {
    if (typeof child != "string") dom.appendChild(child);
    else dom.appendChild(document.createTextNode(child));
  }
  return dom;
}
```

{{index "setAttribute method", "attribute", "onclick property", "click event", "event handling"}}

تفاوت اصلی بین این نسخه و نسخه‌ای که از آن در [فصل ?](game#domdisplay) استفاده کرده ام آن است که این نسخه خاصیت‌ها را به گره‌های DOM اختصاص می دهد نه به attributes. این بدان معنی است که نمی توانیم از آن برای تنظیم خصیصه‌های (attribute) دلخواه استفاده کنیم، اما می توانیم از آن برای تنظیم خاصیت‌هایی که مقدارشان رشته‌ای نیست استفاده کنیم مانند `onclick`، که می‌تواند تابعی را به عنوان گرداننده‌ی رخداد کلیک ثبت کند.

{{index "button (HTML tag)"}}

این باعث می شود که این سبک از ثبت گرداننده‌های رخداد مجاز شود:

```{lang: "text/html"}
<body>
  <script>
    document.body.appendChild(elt("button", {
      onclick: () => console.log("click")
    }, "The button"));
  </script>
</body>
```

## canvas

اولین مؤلفه ای که تعریف خواهیم کرد بخشی از رابط خواهد بود که قرار است تصویر را
به صورت جدولی از مربع‌های رنگ شده نشان دهد. این مؤلفه مسئول دو کار می باشد: نمایش
یک تصویر و ایجاد تعامل بین رخدادهای مربوط به اشاره‌گر و دیگر قسمت‌های برنامه‌ی
کاربردی.

{{index "PictureCanvas class", "callback function", "scale constant", "canvas (HTML tag)", "mousedown event", "touchstart event", [state, "of application"]}}

همینطور، می توانیم آن را به صورت یک مؤلفه که فقط اطلاعاتی در مورد تصویر فعلی
دارد تعریف کنیم، نه درباره‌ی وضعیت کلی برنامه. به دلیل اینکه این مؤلفه نمی داند
که برنامه به صورت کلی چگونه کار می کند، قادر نیست تا اکشن‌ها را مستقیما گسیل دهد.
در عوض، در زمان پاسخ به رخدادهای اشاره‌گر، تابع callbackای را فراخوانی می کند که
توسط کدی که آن را به وجود آورده فراهم شده است، که بخش‌های مربوط به برنامه را
مدیریت می کند.

```{includeCode: true}
const scale = 10;

class PictureCanvas {
  constructor(picture, pointerDown) {
    this.dom = elt("canvas", {
      onmousedown: event => this.mouse(event, pointerDown),
      ontouchstart: event => this.touch(event, pointerDown)
    });
    this.syncState(picture);
  }
  syncState(picture) {
    if (this.picture == picture) return;
    this.picture = picture;
    drawPicture(this.picture, this.dom, scale);
  }
}
```

{{index "syncState method", efficiency}}

هر پیکسل را با مربعهای 10 در 10 رسم می کنیم، همانطور که در ثابت `scale` مشخص شده است. برای اجتناب از کار اضافی، مولفه، تصویر فعلی اش را حفظ و رصد می کند و فقط زمانی باز ترسیم را انجام می دهد که تصویر جدیدی به `syncState` داده شود.

{{index "drawPicture function"}}

تابع اصلی که مسئول ترسیم است ، اندازه‌ی بوم را براساس اندازه‌ی تصویر و ثابت مقیاس
تنظیم می کند و آن را با مربع‌ها پر می کند، یک مربع به ازای هر پیکسل.

```{includeCode: true}
function drawPicture(picture, canvas, scale) {
  canvas.width = picture.width * scale;
  canvas.height = picture.height * scale;
  let cx = canvas.getContext("2d");

  for (let y = 0; y < picture.height; y++) {
    for (let x = 0; x < picture.width; x++) {
      cx.fillStyle = picture.pixel(x, y);
      cx.fillRect(x * scale, y * scale, scale, scale);
    }
  }
}
```

{{index "mousedown event", "mousemove event", "button property", "buttons property", "pointerPosition function"}}

زمانی که دکمه‌ی چپ موس فشرده شود در حالیکه اشاره‌گر روی تصویر است، مؤلفه تابع `pointerDown` را فراخوانی می کند و موقعیت مکانی پیکسلی که روی آن کلیک شده را به آن ارسال می نماید- در مختصات تصویر. این کار برای پیاده‌سازی تعاملات موس با تصویر استفاده می شود. تابع callback ممکن است callback دیگری را بازگرداند تا از حرکت اشاره‌گر به پیکسل دیگر آگاه شود در حالیکه دکمه‌ی موس پایین نگه داشته شده است.

```{includeCode: true}
PictureCanvas.prototype.mouse = function(downEvent, onDown) {
  if (downEvent.button != 0) return;
  let pos = pointerPosition(downEvent, this.dom);
  let onMove = onDown(pos);
  if (!onMove) return;
  let move = moveEvent => {
    if (moveEvent.buttons == 0) {
      this.dom.removeEventListener("mousemove", move);
    } else {
      let newPos = pointerPosition(moveEvent, this.dom);
      if (newPos.x == pos.x && newPos.y == pos.y) return;
      pos = newPos;
      onMove(newPos);
    }
  };
  this.dom.addEventListener("mousemove", move);
};

function pointerPosition(pos, domNode) {
  let rect = domNode.getBoundingClientRect();
  return {x: Math.floor((pos.clientX - rect.left) / scale),
          y: Math.floor((pos.clientY - rect.top) / scale)};
}
```

{{index "getBoundingClientRect method", "clientX property", "clientY property"}}

چون ما اندازه‌ی پیکسل‌ها را می دانیم و می توانیم از تابع `getBoundingClientRect` برای پیدا کردن موقعیت بوم روی صفحه استفاده کنیم، می توان از مختصات رخداد موس (`clientX` و `clientY`) به مختصات تصویر دست پیدا کرد. این اعداد همیشه رند می شوند پس به یک پیکسل مشخص اشاره می کنند.

{{index "touchstart event", "touchmove event", "preventDefault method"}}

با رخدادهای مربوط به لمس، می بایست کاری مشابه انجام دهیم، اما با استفاده از رخدادهای متفاوت و اطمینان از اینکه `preventDefault` را روی `touchstart` فراخوانی کرده باشیم تا از جابجایی تصویر (panning) جلوگیری شود.

```{includeCode: true}
PictureCanvas.prototype.touch = function(startEvent,
                                         onDown) {
  let pos = pointerPosition(startEvent.touches[0], this.dom);
  let onMove = onDown(pos);
  startEvent.preventDefault();
  if (!onMove) return;
  let move = moveEvent => {
    let newPos = pointerPosition(moveEvent.touches[0],
                                 this.dom);
    if (newPos.x == pos.x && newPos.y == pos.y) return;
    pos = newPos;
    onMove(newPos);
  };
  let end = () => {
    this.dom.removeEventListener("touchmove", move);
    this.dom.removeEventListener("touchend", end);
  };
  this.dom.addEventListener("touchmove", move);
  this.dom.addEventListener("touchend", end);
};
```

{{index "touches property", "clientX property", "clientY property"}}

برای رخدادهای لمسی، `clientX` و `clientY` مستقیما روی شیء رخداد در دسترس نیستند، اما می توانیم از مختصات شیء اولین لمس در خاصیت `touches` استفاده کنیم.

## برنامه‌ی کاربردی

برای اینکه بتوان برنامه را جز به جز ساخت، مؤلفه‌ی اصلی را به عنوان یک پوسته حول یک بوم تصویر پیاده سازی می کنیم و مجموعه‌ای پویا از ابزارها و کنترل‌ها را به سازنده‌اش ارسال می کنیم.

کنترل‌ها، همان عناصر موجود در رابط کاربری هستند که در زیر تصویر قرار می گیرند. آن‌ها
به شکل آرایه‌ای از سازنده‌های مؤلفه در دسترس قرار می گیرند.


{{index "br (HTML tag)", "flood fill", "select (HTML tag)", "PixelEditor class", dispatch}}

ابزارها کارهایی مثل ترسیم پیکسل‌ها یا رنگ‌کردن یک قسمت را انجام می‌دهند. برنامه مجموعه‌ای از ابزارها را به صورت یک فیلد `<select>` نمایش می دهد. ابزاری که  انتخاب شده مشخص می کند که تعامل اشاره‌گر با تصویر چه خواهد بود. مجموعه‌ی ابزار‌های موجود به صورت یک شیء فراهم می‌شوند که نام‌های نشان داده شده در فیلد select را به توابعی که آن ابزار را پیاده سازی می کنند، نگاشت می کند. این توابع یک موقعیت تصویر ، یک وضعیت فعلی برنامه و نیز یک تابع `dispatch` را به عنوان آرگومان می گیرند. خروجی آن‌ها ممکن است یک گرداننده‌ی حرکت باشد که با تغییر مکان اشاره‌گر به یک پیکسل دیگر و در نتیجه تغییر وضعیت و موقعیت جدید فراخوانی می شود.


```{includeCode: true}
class PixelEditor {
  constructor(state, config) {
    let {tools, controls, dispatch} = config;
    this.state = state;

    this.canvas = new PictureCanvas(state.picture, pos => {
      let tool = tools[this.state.tool];
      let onMove = tool(pos, this.state, dispatch);
      if (onMove) return pos => onMove(pos, this.state);
    });
    this.controls = controls.map(
      Control => new Control(state, config));
    this.dom = elt("div", {}, this.canvas.dom, elt("br"),
                   ...this.controls.reduce(
                     (a, c) => a.concat(" ", c.dom), []));
  }
  syncState(state) {
    this.state = state;
    this.canvas.syncState(state.picture);
    for (let ctrl of this.controls) ctrl.syncState(state);
  }
}
```
گرداننده‌ی اشاره‌گری که به `PictureCanvas` داده می شود ، ابزاری که در حال حاضر انتخاب شده است را با آرگومان‌های مناسب فراخوانی می کند و اگر پاسخ آن یک گرداننده‌ی حرکت باشد، آن را به گونه‌ای تغییر می دهد که وضعیت را نیز دریافت کند.

{{index "reduce method", "map method", [whitespace, "in HTML"], "syncState method"}}

تمامی کنترل‌ها در `this.controls` ساختار یافته و ذخیره می شوند در نتیجه می توان آنها را با تغییر وضعیت برنامه به‌روز رسانی کرد. فراخوانی `reduce` باعث به وجود آمدن فاصله‌هایی بین عناصر کنترلی DOM می شود. این کار باعث می‌شود طوری نمایش داده نشوند که به نظر برسد همگی باهم فشرده شده اند.

{{index "select (HTML tag)", "change event", "ToolSelect class", "syncState method"}}

اولین کنترل، منوی انتخاب ابزار است. این کنترل یک عنصر `<select>` با یک گزینه برای هر ابزار می سازد و یک
گرداننده‌ی رخداد برای `"change"` تنظیم می کند که وضعیت برنامه را با انتخاب هر ابزار تغییر می دهد.


```{includeCode: true}
class ToolSelect {
  constructor(state, {tools, dispatch}) {
    this.select = elt("select", {
      onchange: () => dispatch({tool: this.select.value})
    }, ...Object.keys(tools).map(name => elt("option", {
      selected: name == state.tool
    }, name)));
    this.dom = elt("label", null, "🖌 Tool: ", this.select);
  }
  syncState(state) { this.select.value = state.tool; }
}
```

{{index "label (HTML tag)"}}

با قرار دادن متن عنوان فیلد و خود فیلد درون یک عنصر `<label>` به مرورگر اعلام می‌کنیم که عنوان مورد نظر مربوط به این فیلد می باشد در نتیجه می توانید به عنوان مثال روی عنوان کلیک کنید تا فیلد فعال شود.

{{index "color field", "input (HTML tag)"}}

همچنین لازم است تا بتوانیم رنگ را تغییر دهیم – پس اجازه بدهید تا یک کنترل برای آن در نظر بگیریم. یک عنصر `<input>` که خصیصه‌ی `type` آن `color` است به ما فیلد فرمی را می دهد که برای انتخاب رنگ طراحی شده است. مقدار این فیلد همیشه یک کد رنگ CSS به فرمت <bdo>`"#RRGGBB"`</bdo> می باشد (قرمز ، سبز و آبی. برای هر رنگ دو رقم). مرورگر یک رابط انتخابگر رنگ را به کاربر نشان می دهد تا بتواند با آن کار کند.

{{if book

بسته به مرورگر، انتخابگر رنگ ممکن است شبیه به تصویر زیر باشد:

{{figure {url: "img/color-field.png", alt: "A color field", width: "6cm"}}}

if}}

{{index "ColorSelect class", "syncState method"}}

این کنترل فیلد مذکور را ایجاد می کند و طوری آن را می نویسد که با خاصیت `color` وضعیت برنامه همگام بماند.

```{includeCode: true}
class ColorSelect {
  constructor(state, {dispatch}) {
    this.input = elt("input", {
      type: "color",
      value: state.color,
      onchange: () => dispatch({color: this.input.value})
    });
    this.dom = elt("label", null, "🎨 Color: ", this.input);
  }
  syncState(state) { this.input.value = state.color; }
}
```

## ابزار ترسیم

پیش از آنکه بتوانیم چیزی رسم کنیم، لازم است تا ابزارهایی که قرار است کارکرد موس یا رخدادهای لمسی روی بوم را کنترل کند پیاده سازی کنیم.

{{index "draw function"}}

پایه‌ای ترین ابزار، ابزار ترسیم است  که هر پیکسلی که روی آن کلیک می شود یا لمس می شود را به رنگ انتخاب شده تغییر می دهد. این ابزار اکشنی را برای تغییر تصویر به تصویری که در آن پیکسل مورد اشاره به رنگ انتخاب شده تغییر یافته است، گسیل می‌نماید.

```{includeCode: true}
function draw(pos, state, dispatch) {
  function drawPixel({x, y}, state) {
    let drawn = {x, y, color: state.color};
    dispatch({picture: state.picture.draw([drawn])});
  }
  drawPixel(pos, state);
  return drawPixel;
}
```

تابع بلافاصله تابع `drawPixel` را فراخوانی می کند و بعد آن را بر می گرداند در نتیجه دوباره برای پیکسل‌های لمس‌شده‌ی دیگر در هنگامی که کاربر موس یا انگشتش را حرکت می دهد، فراخوانی می شود.

{{index "rectangle function"}}

برای ترسیم اشکال بزرگتر بهتر است امکان کشیدن چهار‌ضلعی‌ فراهم باشد. ابزار `rectangle` یک چهارضلعی بین نقطه‌ی شروع ترسیم و نقطه‌ای که موس متوقف می شود ترسیم می کند.

```{includeCode: true}
function rectangle(start, state, dispatch) {
  function drawRectangle(pos) {
    let xStart = Math.min(start.x, pos.x);
    let yStart = Math.min(start.y, pos.y);
    let xEnd = Math.max(start.x, pos.x);
    let yEnd = Math.max(start.y, pos.y);
    let drawn = [];
    for (let y = yStart; y <= yEnd; y++) {
      for (let x = xStart; x <= xEnd; x++) {
        drawn.push({x, y, color: state.color});
      }
    }
    dispatch({picture: state.picture.draw(drawn)});
  }
  drawRectangle(start);
  return drawRectangle;
}
```

{{index "persistent data structure", [state, persistence]}}

یک نکته‌ی جزئی در پیاده سازی این ابزار این است که در هنگام کشیدن (dragging)، چهارضلعی از روی وضعیت  اصلی روی تصویر بازترسیم می‌شود. در این حالت، می توانید چهارضلعی را در هنگام رسم بزرگتر یا کوچکتر کنید بدون اینکه چهارضلعی هایی که این میان رسم می شوند روی تصویر نهایی باقی بمانند. این یکی از دلایلی است که اشیاء تصویری غیرقابل تغییر مفید واقع می شوند – بعدا دلیل دیگری نیز خواهیم دید.

پیاده سازی رنگ آمیزی ناحیه‌های هم‌رنگ یا متصل (flood fill) مقداری پیچیده‌تر است. این ابزار پیکسل مورد اشاره و همه‌ی پیکسل‌های همجوارش که رنگ مشابهی دارند را رنگ می کند. همجوار به این معناست که به صورت افقی یا عمودی همجوار باشد نه به صورت قطری. این تصویر مجموعه‌ی پیکسل‌های رنگ شده در زمان استفاده از این ابزار مشخص را نشان می دهد:

{{figure {url: "img/flood-grid.svg", alt: "A pixel grid showing the area filled by a flood fill operation", width: "6cm"}}}

{{index "fill function"}}

به شکل جالبی، روشی که ما برای انجام این کار استفاده می کنیم شبیه به کدی است که برای مسیریابی در [فصل ?](robot) نوشتیم. در حالی که آن کد به جستجو در طول یک گراف برای یافتن یک مسیر می پرداخت، این کد به جستجو در یک گرید برای پیدا کردن پیکسل‌های متصل می پردازد. مشکل نگهداری و رصد یک مجموعه شاخه از مسیرهای ممکن، شبیه به آن مساله است.

```{includeCode: true}
const around = [{dx: -1, dy: 0}, {dx: 1, dy: 0},
                {dx: 0, dy: -1}, {dx: 0, dy: 1}];

function fill({x, y}, state, dispatch) {
  let targetColor = state.picture.pixel(x, y);
  let drawn = [{x, y, color: state.color}];
  for (let done = 0; done < drawn.length; done++) {
    for (let {dx, dy} of around) {
      let x = drawn[done].x + dx, y = drawn[done].y + dy;
      if (x >= 0 && x < state.picture.width &&
          y >= 0 && y < state.picture.height &&
          state.picture.pixel(x, y) == targetColor &&
          !drawn.some(p => p.x == x && p.y == y)) {
        drawn.push({x, y, color: state.color});
      }
    }
  }
  dispatch({picture: state.picture.draw(drawn)});
}
```
آرایه‌ی پیکسل‌های رسم شده ، به عنوان لیست کار این تابع استفاده می‌شود. برای هر پیکسل که بررسی می شود، بایستی ببینیم که آیا پیکسل‌های همجوارش رنگ مشابهی دارند یا خیر و آیا قبل از این رنگ‌آمیزی شده اند. با ورود پیکسل‌های جدید، شمارنده‌ی حلقه از طول آرایه‌ی `drawn` عقب می‌افتد. تمامی پیکسل‌های جلوتر از آن همچنان لازم است تا بررسی شوند. زمانی که شمارنده به طول آرایه می رسد ، هیچ پیکسل بررسی نشده‌ای نمی ماند و کار تابع تمام می شود.

{{index "pick function"}}

آخرین ابزار، گزینشگر رنگ می باشد که به شما امکان انتخاب یک رنگ با استفاده از اشاره گر و استفاده از آن به عنوان رنگ فعلی ترسیم را می دهد.

```{includeCode: true}
function pick(pos, state, dispatch) {
  dispatch({color: state.picture.pixel(pos.x, pos.y)});
}
```

{{if interactive

اکنون می توانیم اپلیکیشن‌مان را آزمایش نماییم!

```{lang: "text/html"}
<div></div>
<script>
  let state = {
    tool: "draw",
    color: "#000000",
    picture: Picture.empty(60, 30, "#f0f0f0")
  };
  let app = new PixelEditor(state, {
    tools: {draw, fill, rectangle, pick},
    controls: [ToolSelect, ColorSelect],
    dispatch(action) {
      state = updateState(state, action);
      app.syncState(state);
    }
  });
  document.querySelector("div").appendChild(app.dom);
</script>
```

if}}

## ذخیره سازی و بارگیری

{{index "SaveButton class", "drawPicture function", [file, image]}}

پس از اتمام ترسیم شاهکار هنری‌مان، می خواهیم آن را ذخیره کنیم. بایستی دکمه‌ای اضافه کنیم که بتوان به وسیله‌ی آن تصویر فعلی را به شکل یک فایل عکسی بارگیری کرد. این کنترل دکمه‌ی مذکور را ایجاد می کند.

```{includeCode: true}
class SaveButton {
  constructor(state) {
    this.picture = state.picture;
    this.dom = elt("button", {
      onclick: () => this.save()
    }, "💾 Save");
  }
  save() {
    let canvas = elt("canvas");
    drawPicture(this.picture, canvas, 1);
    let link = elt("a", {
      href: canvas.toDataURL(),
      download: "pixelart.png"
    });
    document.body.appendChild(link);
    link.click();
    link.remove();
  }
  syncState(state) { this.picture = state.picture; }
}
```

{{index "canvas (HTML tag)"}}


این مؤلفه تصویر فعلی را رصد کرده در نتیجه می تواند آن را ذخیره کند. برای  ساخت یک فایل تصویری ، از عنصر `<canvas>` که تصویر را روی آن رسم می کنیم (در مقیاس یک پیکسل بر پیکسل) استفاده می کنیم.

{{index "toDataURL method", "data URL"}}

متد `toDataURL` روی یک canvas یک URL ایجاد می کند که با <bdo>`data:`</bdo> شروع می شود. برخلاف URLهای <bdo>`http:`</bdo> و <bdo>`https:`</bdo> آدرس  (URL)های از نوع data حاوی تمام منبع در خود URL می باشند. این URL ها معمولا خیلی بلند هستند اما به ما این امکان را می دهند تا پیوندهای واقعی به تصاویر دلخواه بسازیم، در دل خود مرورگر.

{{index "a (HTML tag)", "download attribute"}}

برای اینکه کاری کنیم تا مرورگر تصویر ایجاد شده را دانلود کند، یک عنصر لینک می سازیم که به این URL اشاره می کند و دارای یک خصیصه‌ی `download` می‌باشد. این گونه لینک‌ها، در زمان کلیک شدن، باعث می شوند که مرورگر یک پنجره‌ی محاوره‌ای ذخیره فایل را نمایش دهند. ما آن لینک را به سند اضافه می کنیم و عمل کلیک کردن را روی آن شبیه‌سازی می کنیم و سپس آن را حذف می کنیم.

کارهای زیادی با تکنولوژی مرورگر ها می توان انجام داد اما گاهی اوقات برای انجام کاری لازم است که از روش‌های نامانوس استفاده کنیم.

{{index "LoadButton class", control, [file, image]}}

و بدتر هم می‌شود. لازم است بتوانیم یک فایل تصویری را نیز به درون برنامه بارگیری کنیم. برای این کار، دوباره یک مؤلفه دکمه می سازیم.

```{includeCode: true}
class LoadButton {
  constructor(_, {dispatch}) {
    this.dom = elt("button", {
      onclick: () => startLoad(dispatch)
    }, "📁 Load");
  }
  syncState() {}
}

function startLoad(dispatch) {
  let input = elt("input", {
    type: "file",
    onchange: () => finishLoad(input.files[0], dispatch)
  });
  document.body.appendChild(input);
  input.click();
  input.remove();
}
```

{{index [file, access], "input (HTML tag)"}}

برای دسترسی به یک فایل روی کامپیوتر کاربر، کاربر بایستی یک فایل را با استفاده از فیلد ورودی فایل انتخاب کند. اما من نمی خواهم که دکمه‌ی بارگیری شبیه به فیلد ورودی فایل باشد؛ بنابراین بعد از فشردن دکمه، فیلد فایل را ایجاد می کنیم و سپس وانمود می کنیم که خودش کلیک را انجام داده است.

{{index "FileReader class", "img (HTML tag)", "readAsDataURL method", "Picture class"}}

زمانی که کاربر یک فایل را انتخاب می کند، می توانیم از `FileReader` برای دسترسی به محتوای آن استفاده کنیم و دوباره به عنوان یک data URL. این URL را می توان برای ایجاد یک عنصر `<img>` استفاده کرد اما به دلیل اینکه دسترسی مستقیمی به پیکسل‌های این تصویر نداریم، نمی توانیم یک شیء `Picture` از آن بسازیم.


```{includeCode: true}
function finishLoad(file, dispatch) {
  if (file == null) return;
  let reader = new FileReader();
  reader.addEventListener("load", () => {
    let image = elt("img", {
      onload: () => dispatch({
        picture: pictureFromImage(image)
      }),
      src: reader.result
    });
  });
  reader.readAsDataURL(file);
}
```

{{index "canvas (HTML tag)", "getImageData method", "pictureFromImage function"}}

برای دسترسی به پیکسل‌ها بایستی ابتدا یک تصویر روی عنصر `<canvas>` رسم کنیم. context این canvas متدی به نام `getImageData` دارد که به جاوااسکریپت این امکان را می دهد تا پیکسل های آن را بخواند. بنابراین با قرار گرفتن تصویر روی بوم، می توانیم به آن دسترسی داشته باشید و شی `Picture` را بسازیم.


```{includeCode: true}
function pictureFromImage(image) {
  let width = Math.min(100, image.width);
  let height = Math.min(100, image.height);
  let canvas = elt("canvas", {width, height});
  let cx = canvas.getContext("2d");
  cx.drawImage(image, 0, 0);
  let pixels = [];
  let {data} = cx.getImageData(0, 0, width, height);

  function hex(n) {
    return n.toString(16).padStart(2, "0");
  }
  for (let i = 0; i < data.length; i += 4) {
    let [r, g, b] = data.slice(i, i + 3);
    pixels.push("#" + hex(r) + hex(g) + hex(b));
  }
  return new Picture(width, height, pixels);
}
```

ما اندازه‌ی تصاویر را به 100 در 100 پیکسل محدود خواهیم کرد به این دلیل که هر تصویر بزرگتری روی صفحه‌نمایش ما خیلی بزرگ به نظر می رسد و ممکن است باعث کندی رابط کاربری شود.


{{index "getImageData method", color, transparency}}

خاصیت `data` مربوط به شیءای که با `getImageData` برمی گردد، آرایه‌ای از مؤلفه‌های رنگ می باشد. برای هر پیکسل متعلق به چهارضلعی، که توسط آرگومان‌ها مشخص شده است، دارای چهار مقدار می‌باد که نمایانگر اجزای قرمز، سبز، آبی و آلفای رنگ پیکسل می‌باشند که اعدادی بین 0 و 255 هستند. بخش alpha مربوط به شفافیت می باشد – زمانی که برابر صفر است، پیکسل کاملا شفاف می باشد و وقتی 255 است کامل مات می شود. برای مقصود ما، می توانیم از آن صرف نظر کنیم.


{{index "hexadecimal number", "toString method"}}

دو عدد هگزادسیمال برای هر مؤلفه که در مقدار رنگ ما آمده است، به شکلی دقیق متناظر با طیف 0 تا 255 است – دو رقم مبنای 16 می توانند  <bdo>16^2^ = 256</bdo> عدد متفاوت را نمایش دهند. به متد `toString` مربوط به اعداد می توان یک مبنا به عنوان آرگومان فرستاد، بنابراین  <bdo>`n.toString(16)`</bdo> رشته‌ای تولید می کند که در مبنای 16 نشان داده می شود. بایستی مطمئن شویم که هر عدد دو رقم اشغال می کند در نتیجه تابع کمکی `hex` تابع `padStart` را برای افزودن صفرهای پیشین در صورت نیاز فراخوانی خواهد شد.

اکنون می توانیم بارگیری و ذخیره کنیم. یک امکان دیگر می ماند تا کار تمام شود.


## بازگشت در تاریخچه (undo)

نیمی از روند عمل ویرایش شامل انجام اشتباهات کوچک و برطرف کردن آن‌ها است. بنابراین یک ویژگی و کارکرد مهم در یک برنامه‌ی ترسیم این است که بتوان در تاریخچه به عقب بازگشت.

{{index "persistent data structure", [state, "of application"]}}

برای اینکه بتوانیم تغییرات را خنثی کنیم، لازم است تا نسخه‌ی قبلی از تصویر را جایی ذخیره کنیم. به دلیل اینکه این مقدار غیرقابل تغییر می باشد این کار آسان است. اما این کار مستلزم آن است که فیلد دیگری در وضعیت برنامه تعبیه شود.

{{index "done property"}}

ما آرایه‌ای به نام `done` اضافه خواهیم کرد تا نسخه‌های قبلی تصویر را نگهداری کنیم. نگهداری این خاصیت نیازمند تابع به‌روز رسانی وضعیت پیچیده‌تری می باشد که تصاویر را به آرایه اضافه کند.

{{index "doneAt property", "historyUpdateState function", "Date.now function"}}

اما ما قصد نداریم هر تغییری را ذخیره کنیم. فقط تغییراتی که در بازه‌ی زمانی خاصی رخ داده باشند. برای انجام این کار، نیاز به خاصیتی دومی هست، `doneAt،` که زمان آخرین ذخیره‌ی یک تصویر در تاریخچه را نگهداری می کند.

```{includeCode: true}
function historyUpdateState(state, action) {
  if (action.undo == true) {
    if (state.done.length == 0) return state;
    return Object.assign({}, state, {
      picture: state.done[0],
      done: state.done.slice(1),
      doneAt: 0
    });
  } else if (action.picture &&
             state.doneAt < Date.now() - 1000) {
    return Object.assign({}, state, action, {
      done: [state.picture, ...state.done],
      doneAt: Date.now()
    });
  } else {
    return Object.assign({}, state, action);
  }
}
```

{{index "undo history"}}

زمانی که اکشن مااز جنس خنثی کردن تغییر باشد، تابع، آخرین تصویر موجود در تاریخچه را برداشته و آن را به عنوان تصویر فعلی قرار می دهد. همچنین مقدار `doneAt` را برابر صفر قرار می‌دهد تا تغییر بعدی در تاریخچه ثبت شود و  امکان عقب‌گرد برای آن در صورت نیاز فراهم باشد.


در غیر این صورت، اگر اکشن حاوی تصویر جدیدی باشد و آخرین باری که ما تصویری را ذخیره کرده ایم بیش از یک ثانیه عقب تر باشد ( 1000 میلی ثانیه) خاصیت‌های `done` و `doneAt` به‌روز می شوند تا تصویر قبلی را ذخیره می کنند.

{{index "UndoButton class", control}}

مؤلفه‌ی دکمه‌ی خنثی (undo) کاری زیادی انجام نمی دهد. اکشن‌های خنثی سازی را با بروز کلیک گسیل می دهد و  زمانی که چیزی برای بازگشت یا خنثی سازی موجود نیست، خودش را غیرفعال می کند.


```{includeCode: true}
class UndoButton {
  constructor(state, {dispatch}) {
    this.dom = elt("button", {
      onclick: () => dispatch({undo: true}),
      disabled: state.done.length == 0
    }, "⮪ Undo");
  }
  syncState(state) {
    this.dom.disabled = state.done.length == 0;
  }
}
```

## اکنون زمان ترسیم است

{{index "PixelEditor class", "startState constant", "baseTools constant", "baseControls constant", "startPixelEditor function"}}

برای بالا آوردن برنامه، نیاز است تا یک وضعیت، مجموعه‌ای از ابزار، مجموعه‌ای از کنترل‌ها و یک تابع گسیل (dispatch) را ایجاد کنیم. می توانیم آن ها را به سازنده‌ی `PixelEditor` ارسال نماییم تا مؤلفه‌ی اصلی را ایجاد نماید. به دلیل اینکه نیاز است تا چندین ویرایشگر در بخش تمرین‌ها بسازیم، در اینجا چند متغیر تعریف می کنیم.


```{includeCode: true}
const startState = {
  tool: "draw",
  color: "#000000",
  picture: Picture.empty(60, 30, "#f0f0f0"),
  done: [],
  doneAt: 0
};

const baseTools = {draw, fill, rectangle, pick};

const baseControls = [
  ToolSelect, ColorSelect, SaveButton, LoadButton, UndoButton
];

function startPixelEditor({state = startState,
                           tools = baseTools,
                           controls = baseControls}) {
  let app = new PixelEditor(state, {
    tools,
    controls,
    dispatch(action) {
      state = historyUpdateState(state, action);
      app.syncState(state);
    }
  });
  return app.dom;
}
```

{{index "destructuring binding", "= operator", [property, access]}}

زمانی که یک شیء یا یک آرایه را destruct می کنیم، می توانیم از = بعد از نام متغیر برای تعیین یک مقدار
پیشفرض برای متغیر استفاده کنیم که در زمانی که خاصیت موجود نیست یا برابر `undefined` می باشد استفاده می شود. تابع `startPixelEditor` از این خاصیت استفاده می کند تا شیءای را با تعدادی خاصیت اختیاری به عنوان آرگومان قبول می کند. اگر خاصیت `tools` را فراهم نسازید به عنوان مثال `tools` به `baseTools` متناظر
خواهد شد.

و این روشی است که ما به وسیله‌ی آن، یک ویرایشگر واقعی را روی صفحه‌ی نمایش اجرا می کنیم

```{lang: "text/html", startCode: true}
<div></div>
<script>
  document.querySelector("div")
    .appendChild(startPixelEditor({}));
</script>
```

{{if interactive

می توانید کمی به ترسیم بپردازید. من منتظر می‌مانم.

if}}

## چرا این کار این قدر پرزحمت است

تکنولوژی مرورگرها شگفت‌انگیز است.این تکنولوژی مجموعه‌ای قدرتمند از بلاک‌ها برای ساخت رابط، روش‌های سبک‌دهی و تغییر سبک، و ابزارهایی برای اشکال‌زدایی و بررسی برنامه‌هایتان را فراهم می‌سازد. نرم‌افزاری که برای مرورگر می نویسید تقریبا روی هر کامپیوتر یا گوشی یا تبلتی اجرا می شود.


در عین حال، تکنولوژی مرورگر مزخرف است. شما باید مجموعه‌ای بزرگ از ترفندهای احمقانه و اطلاعات مبهم را یاد بگیرید تا به آن مسلط شوید و همچنین مدل برنامه‌نویسی پیش‌فرضی که فراهم می سازد بسیار مشکل‌زا است که بیشتر برنامه نویسان ترجیح می دهند تا آن را با لایه‌های متعددی از تجرید بپوشانند تا این که مستقیما با آن کار کنند.

{{index standard, evolution}}

اگرچه این شرایط قطعا در حال بهبود می باشد، اما بیشتر این تغییرات در قالب معرفی عناصر جدید برای پوشش کمبودها انجام می شود که خود به پیچیدگی آن افزوده است. یک ویژگی که توسط میلیون‌ها وبسایت استفاده می شود را نمی توان جایگزین کرد. و حتی اگر این کار شدنی باشد، اینکه با چه جایگزین شود خود تصمیمی سخت است.

{{index "social factors", "economic factors", history}}

تکنولوژی هیچ وقت در خلاء قابل بررسی نیست – ما توسط ابزارهایمان و جامعه،اقتصاد و فاکتورهای تاریخی که آن ها را ایجاد کرده است محدود می شویم. ممکن است این واقعیت آزاردهنده باشد، اما به طول کلی پربارتر خواهد بود اگر تلاش کنیم تا فهم خوبی از نحوه‌ی کارکرد واقعیت‌های فنی کنونی داشته باشد- و اینکه چرا به شکل فعلی کار می کند- تا اینکه به جنگ آن برویم یا مقاومت پذیرش آن بخواهیم چیزی دیگری را درخواست کنیم.


تجریدهای جدید می توانند مفید باشند. مدل مؤلفه و روش جریان داده ای که من در این فصل استفاده کردم شکل خاصی از آن‌ها بود. همانطور که ذکر شد، کتابخانه‌هایی وجود دارند که باعث می شوند برنامه‌نویسی رابط کاربری بسیار دلپذیر تر بشود. در زمان نوشتن این کتاب ، [React](https://reactjs.org/) و [Angular](https://angular.io/) انتخاب‌های پرآوازه‌ای هستند اما صنعت پرتحرکی از این چهارچوب‌ها در حال فعالیت می باشد. اگر به برنامه نویسی اپلیکیشن‌های وب علاقمند هستید، پیشنهاد می کنم چند تایی از آن ها را بررسی کنید تا از نحوه‌ی کارکرد آن ها باخبر شوید و ببینید چه مزایایی با خود می آورند.

## تمرین‌ها

برنامه‌ی ما هنوز جای بهتر شدن دارد. اجازه بدهید چند ویژگی جدید به عنوان تمرین به آن اضافه کنیم.


### میان‌برهای صفحه‌کلید

{{index "keyboard bindings (exercise)"}}

به برنامه میانبر های صفحه کلید اضافه کنید. اولین حرف هر ابزار برای انتخاب آن ابزار و [control]{keyname}-Z یا [command]{keyname}-Z برای انجام خنثی سازی.


{{index "PixelEditor class", "tabindex attribute", "elt function", "keydown event"}}

این کار را با ایجاد تغییر در مؤلفه‌ی `PixelEditor` انجام دهید. به خاصیت `tabIndex` متعلق به عنصر پوششی `<div>` ، مقدار 0 را اعمال کنید. در نتیجه می تواند توسط صفحه‌کلید فعال شود. توجه داشته باشید که خاصیت مذکور که به خصیصه‌ی `tabindex` متناظر است `tabIndex` خوانده می شود که حرف اول آن بزرگ است و تابع `elt` ما نام‌های خاصیت را قبول می کند. گرداننده‌های رخدادهای صفحه‌کلید را مستقیما روی آن عنصر ثبت کنید. یعنی باید کلیک و لمس touch یا tab روی برنامه انجام شود قبل از اینکه بتوانید با آن توسط صفحه کلید تعامل کنید.

{{index "ctrlKey property", "metaKey property", "control key", "command key"}}

به خاطر داشته باشید که رخدادهای صفحه‌کلید دارای خاصیت‌های `ctrlKey` و `metaKey‍` (برای کلید [command]{keyname} در مک) می باشند که می توانید از آن ها برای اینکه ببینید آیا این کلیدها پایین نگه‌داشته شده اند استفاده کنید.

{{if interactive

```{test: no, lang: "text/html"}
<div></div>
<script>
  // The original PixelEditor class. Extend the constructor.
  class PixelEditor {
    constructor(state, config) {
      let {tools, controls, dispatch} = config;
      this.state = state;

      this.canvas = new PictureCanvas(state.picture, pos => {
        let tool = tools[this.state.tool];
        let onMove = tool(pos, this.state, dispatch);
        if (onMove) {
          return pos => onMove(pos, this.state, dispatch);
        }
      });
      this.controls = controls.map(
        Control => new Control(state, config));
      this.dom = elt("div", {}, this.canvas.dom, elt("br"),
                     ...this.controls.reduce(
                       (a, c) => a.concat(" ", c.dom), []));
    }
    syncState(state) {
      this.state = state;
      this.canvas.syncState(state.picture);
      for (let ctrl of this.controls) ctrl.syncState(state);
    }
  }

  document.querySelector("div")
    .appendChild(startPixelEditor({}));
</script>
```

if}}

{{hint

{{index "keyboard bindings (exercise)", "key property", "shift key"}}


خاصیت `key` متعلق به رخداد‌های کلید‌های حروف همان حروف به شکل کوچک خواهند بود، اگر کلید [shift]{keyname} فشرده شده نباشد. در اینجا ما به رخداد‌های کلیدهایی که [shift]{keyname} دارند توجه‌ای نمی‌کنیم.

{{index "keydown event"}}

یک گرداننده‌ی `"keydown"` می تواند شیء رخدادش را برای تطبیق با میان‌برها بررسی کند. می‌توانید لیست حروف‌ اول را از شیء `tools` به دست بیاورید تا نیاز نباشد آن‌ها را از ابتدا بنویسید.

{{index "preventDefault method"}}

زمانی که یک رخداد کلید با یک میانبر مطابقت دارد، `preventDefault` را روی آن فراخوانی کنید و اکشن مناسب با آن را گسیل دهید.

hint}}

### ترسیم بهینه

{{index "efficient drawing (exercise)", "canvas (HTML tag)", efficiency}}

در طول ترسیم بیشترین کاری که برنامه‌ی ما انجام می دهد توسط `drawPicture` صورت می گیرد. ایجاد یک وضعیت جدید و به‌روز رسانی ما بقی قسمت‌های DOM کار زیاد پرهزینه‌ای محسوب نمی شود، اما بازنقاشی تمامی پیکسل‌ها روی canvas نسبتا کار می برد.

{{index "syncState method", "PictureCanvas class"}}

راهی بیابید که متد `syncState` مربوط به PictureCanvas را بتوان با باز ترسیم تنها پیکسل‌هایی که واقعا تغییر نموده‌اند سریع تر کرد.


{{index "drawPicture function", compatibility}}

به یاد داشته باشید که `drawPicture` توسط دکمه‌ی ذخیره‌سازی نیز استفاده شده است بنابراین اگر آن را تغییر دهید مطمئن شوید که تغییرات موجب از کار افتادن آن نمی شود. می توان نسخه‌ی جدیدی از آن را با نامی دیگر ساخت.

{{index "width property", "height property"}}

همچنین توجه داشته باشید که تغییر اندازه‌ی یک عنصر `<canvas>`، توسط تغییر خاصیت‌های `width` و `height` آن، باعث می شود که از صفحه پاک شود و به طور کامل به صورت شفاف در بیاید.


{{if interactive

```{test: no, lang: "text/html"}
<div></div>
<script>
  // Change this method
  PictureCanvas.prototype.syncState = function(picture) {
    if (this.picture == picture) return;
    this.picture = picture;
    drawPicture(this.picture, this.dom, scale);
  };

  // You may want to use or change this as well
  function drawPicture(picture, canvas, scale) {
    canvas.width = picture.width * scale;
    canvas.height = picture.height * scale;
    let cx = canvas.getContext("2d");

    for (let y = 0; y < picture.height; y++) {
      for (let x = 0; x < picture.width; x++) {
        cx.fillStyle = picture.pixel(x, y);
        cx.fillRect(x * scale, y * scale, scale, scale);
      }
    }
  }

  document.querySelector("div")
    .appendChild(startPixelEditor({}));
</script>
```

if}}

{{hint

{{index "efficient drawing (exercise)"}}

این تمرین مثال خوبی است که نشان می دهد چگونه ساختارهای داده‌ی غیرقابل تغییر می توانند باعث سریع تر شدن کد شوند. به دلیل اینکه ما به هر دوی تصاویر جدید و قدیم دسترسی داریم، می توانیم آن‌ها را مقایسه کرده و فقط پیکسل‌هایی را بازترسیم کنیم که رنگشان تغییر یافته است و از 99 درصد از کار ترسیم اضافی بپرهیزیم.

{{index "drawPicture function"}}

شما همچنین می توانید تابع جدیدی `updatePicture` بنویسید یا به `drawPicture` آرگومان جدیدی اضافه کنید که می‌تواند undefined یا برابر تصویر قبل باشد. برای هر پیکسل، تابع بررسی می‌کند آیا تصویر داده‌شده‌ی قبلی در این موقعیت رنگ مشابهی دارد که در این صورت از آن رد می شود.

{{index "width property", "height property", "canvas (HTML tag)"}}

با توجه به اینکه canvas با تغییر اندازه، پاک می شود، باید از دستکاری `width` و ‍`height` آن در زمانی که تصویر قدیمی و جدید اندازه‌ی مشابهی دارند، خودداری کنید. در صورت نبود اندازه‌ی برابر، که در هنگام بارگیری تصویر جدید رخ می دهد، می توانید متغیرهایی که تصویر قبلی را نگه‌داری می کردند را پس از تغییر اندازه‌ی canvas برابر null قرار دهید زیرا نباید هیچ پیکسلی پس از تغییر اندازه‌ی canvas از قلم بیفتد.

hint}}

### دوایر

{{index "circles (exercise)", dragging}}

ابزاری به نام `circle` تعریف کنید که با کشیدن آن روی بوم بتوان دایره‌ی توپر رسم نمود. مرکز دایره در قسمتی قرار بگیرد که شروع رسم یا لمس قرار می گیرد و شعاع آن با حرکت موس معلوم می شود.


{{if interactive

```{test: no, lang: "text/html"}
<div></div>
<script>
  function circle(pos, state, dispatch) {
    // Your code here
  }

  let dom = startPixelEditor({
    tools: Object.assign({}, baseTools, {circle})
  });
  document.querySelector("div").appendChild(dom);
</script>
```

if}}

{{hint

{{index "circles (exercise)", "rectangle function"}}

می توانید از ابزار `rectangle‍` الهام گیری کنید. شبیه آن ابزار، در هنگام حرکت اشاره‌گر موس، می خواهید به ترسیم روی تصویر آغازین ادامه دهید نه تصویر فعلی.

برای دستیابی به پیکسل‌هایی که بایستی رنگ شوند، می توانید از قضیه‌ی فیثاغورس استفاده کنید. ابتدا فاصله‌ی بین موقعیت اشاره‌گر فعلی و موقعیت آغازین را با محاسبه‌ی ریشه‌ی دوم (`Math.sqrt`) مجموع مربع‌های (`Math.pow(x,2)`) تفاوت مختصات x و مربع تفاوت مختصات y، به دست بیاورید. سپس سراغ مربع پیکسل‌های اطراف نقطه‌ی آغازین بروید با این شرط که ضلع‌شان حداقل دوبرابر شعاع باشد، و آن‌هایی که در محدوده‌ی شعاع قرار می‌گیرند را رنگ کنید، با استفاده از قضیه‌ی فیثاغورس، فاصله‌ی آن‌ها را از مرکز به دست می‌آید.

حواستان باشد که پیکسل‌هایی که بیرون از محدوده‌ی تصویر هستند را رنگ نکنید.

hint}}

### خطوط صحیح

{{index "proper lines (exercise)", "line drawing"}}

این تمرین از دو تمرین قبلی پیشرفته تر است و لازم است تا راه حلی برای مسئله‌ای نه چندان ساده طراحی کنید. مطمئن شوید که  زمان و صبر کافی برای حل آن قبل از کار کردن روی تمرین، در اختیار دارید و اگر در ابتدای کار موفق به حل آن نشدید نا امید نشوید.

{{index "draw function", "mousemove event", "touchmove event"}}

در بیشتر مرورگرها، زمانی که ابزار `draw` را انتخاب می کنید و سریع روی تصویر به کشیدن می پردازید، خط بسته تولید نمی شود. در عوض آنچه رخ میدهد کشیده شدن نقاطی مقطع است که دلیل آن این است که رخدادهای `"mousemove"` یا `"touchmove"` به سرعت و پی در پی ایجاد نمی شوند که همه‌ی پیکسل‌ها را پوشش دهند.

ابزار `draw` را بهبود دهید تا بتواند یک خط کامل را رسم نماید. این به این معنا است که شما باید تابع گرداننده‌ی حرکت را تغییر دهید تا موقعیت قبلی را به خاطر داشته باشد و آن را به موقعیت فعلی متصل کند.

برای انجام این کار، به دلیل اینکه پیکسل‌ها می توانند با فاصله‌ی دلخواهی واقع شوند، لازم است تا یک تابع عمومی برای ترسیم خط بنویسید.

یک خط بین دو نقطه توسط زنجیره‌ای از پیکسل‌های متصل رسم می شود که تا جای ممکن مستقیم هستند و از نقطه‌ی شروع به نقطه‌ی پایان قرار می گیرند. پیکسل‌هایی که به صورت قطری مجاور هم هستند به عنوان پیکسل‌های متصل محسوب می شوند. بنابراین یک خط مورب می تواند شبیبه به تصویر سمت چپ باشد نه تصویر سمت راست.

{{figure {url: "img/line-grid.svg", alt: "Two pixelated lines, one light, skipping across pixels diagonally, and one heavy, with all pixels connected horizontally or vertically", width: "6cm"}}}

درنتیجه، اگر کدی داشته باشیم که یک خط بین دو نقطه‌ی دلخواه رسم کند، ممکن است همچنین ادامه دهیم و از آن برای تعریف یک ابزار `line` نیز بهره ببریم که برای ترسیم خط مستقیم بکار می رود.


{{if interactive

```{test: no, lang: "text/html"}
<div></div>
<script>
  // The old draw tool. Rewrite this.
  function draw(pos, state, dispatch) {
    function drawPixel({x, y}, state) {
      let drawn = {x, y, color: state.color};
      dispatch({picture: state.picture.draw([drawn])});
    }
    drawPixel(pos, state);
    return drawPixel;
  }

  function line(pos, state, dispatch) {
    // Your code here
  }

  let dom = startPixelEditor({
    tools: {draw, line, fill, rectangle, pick}
  });
  document.querySelector("div").appendChild(dom);
</script>
```

if}}

{{hint

{{index "proper lines (exercise)", "line drawing"}}


موضوع مهم درباره‌ی مشکل ترسیم یک خط پیکسلی این است که در واقع ما با چهار مسئله مشابه اما کمی متفاوت روبرو هستیم. رسم یک خط افقی از چپ به راست آسان است - مختصات x را پیمایش می کنید و در هر گام یک پیکسل را رنگ می کنید. اگر خط شیب کمی داشته باشد (کمتر از ۴۵ درجه یا ¼π رادیان )، می توانید مختصات y را در طول زاویه درون‌یابی کنید. هنوز به یک پیکسل برای هر موقعیت x نیاز دارید چراکه مختصات y آن‌ها توسط شیب تعیین می شود.

اما به محض اینکه شییب شما بیشتر از 45 درجه شود، باید روش برخورد با مختصات را عوض کنید. اکنون به یک پیکسل برای هر موقعیت y نیاز دارید چراکه خط بیشتر به بالا حرکت می‌کند تا به چپ. و در نتیجه، وقتی از شیب 135 درجه عبور می کنید، باید به پیمایش مختصات x برگردید اما از راست به چپ.

در واقع نیازی نیست که چهار حلقه بنویسید. با توجه به اینکه رسم یک خط از نقطه‌ی A به B همانند رسم خط از B به A می‌باشد، می‌توانید موقعیت‌های شروع و پایان را برای خطوطی که از راست به چپ می‌روند جابجا کنید و مانند چپ به راست آن‌ها را در نظر بگیرید.

بنابراین به دو حلقه‌ی متفاوت نیاز دارید. اولین چیزی که تابع رسم خط شما باید انجام دهد این است که بررسی کند که تفاوت مختصات x بزرگ‌تر از تفاوت مختصات y باشد. اگر این طور بود، این خط، خطی افقی شکل است و اگر نبود، عموی شکل می باشد.

{{index "Math.abs function", "absolute value"}}

اطمینان حاصل کنید که قدر مطلق تفاوت مقادیر x و y را مقایسه می کنید که می توان این کار را با <bdo>`Math.abs`</bdo> انجام داد.

{{index "swapping bindings"}}

به محض اینکه بدانید حول کدام محور پیمایش خواهید داشت، می توانید بررسی کنید که نقطه‌ی شروع دارای مختصات بالاتری در طول آن محور نسبت به نقطه‌ی پایان باشد و در صورت نیاز آن‌ها را جابجا کنید. یک روش مختصر برای جابجایی مقادیر دو متغیر در جاوااسکریپت استفاده از انتساب و تخریب به شکل زیر است:


```{test: no}
[start, end] = [end, start];
```

{{index rounding}}

اکنون می توانید شیب خط را محاسبه نمایید، که خود مقداری که مختصات روی دیگر محور تغیر می کند را برای هر گامی که روی محور اصلی برمی‌دارید، تعیین می نماید. با استفاده از آن، می توانید از یک حلقه برای محور اصلی استفاده کنید و در حین آن موقعیت متناظر روی محور دیگر را نیز رصد کنید، و می توانید پیکسل‌ها را در هر گام حلقه رسم کنید. مطمئن شوید که مختصات محور غیراصلی را رند نمایید چراکه احتمالا اعشاری باشند و متد `draw`به مختصات اعشاری پاسخ خوبی نمی دهد.


hint}}
