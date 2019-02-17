{{meta {load_files: ["code/chapter/06_object.js"], zip: "node/html"}}}

# زندگی اسرارآمیز اشیاء

{{quote {author: "Barbara Liskov", title: "Programming with Abstract Data Types", chapter: true}

An abstract data type is realized by writing a special kind of program
[…] which defines the type in terms of the operations which can be
performed on it.

quote}}

{{index "Liskov, Barbara", "abstract data type"}}

{{figure {url: "img/chapter_picture_6.jpg", alt: "Picture of a rabbit with its proto-rabbit", chapter: framed}}}

اشیاء جاوااسکریپت، در [فصل ?](data) معرفی شدند. در فرهنگ برنامه نویسی، مفهومی وجود دارد که به آن _((برنامه نویسی شیء گرا))_ گفته می شود؛ مجموعه‌ای از تکنیک ها که از اشیاء (و مفاهیم مرتبط) به عنوان اصل مرکزی سازماندهی برنامه استفاده می کند.

با وجود اینکه هیچ کس روی تعریف دقیق آن توافق ندارد، برنامه نویسی شیء گرا، طراحی خیلی از زبان‌های برنامه نویسی را شکل داده است، مثل خود جاوااسکریپت. این فصل توضیح می دهد چگونه این ایده‌ها را در جاوااسکریپت پیاده سازی کنیم.

## کپسوله‌سازی یا Encapsulation

{{index encapsulation, isolation, modularity}}

ایده‌ی اصلی در برنامه نویسی شیء گرا، تقسیم برنامه به قسمت‌های کوچکتر است با این شرط که هر قسمت مسئول مدیریت وضعیت خودش باشد.

در این روش، بعضی از اطلاعات مربوط به نحوه‌ی کارکرد یک قسمت از برنامه را می توان به صورت _محلی_ برای همان قسمت نگه داری کرد. کسی که روی قسمت‌های دیگر برنامه کار می کند نیازی نیست تا این اطلاعات را به خاطر داشته باشد یا حتی اصلا از وجودشان آگاه باشد.  در صورت تغییر این جزئیات محلی، فقط لازم است کد‌هایی را به‌روز کنیم که مستقیما پیرامون آن قرار دارند.

{{id interface}}
{{index [interface, object]}}

بخش‌های مختلف این گونه برنامه‌ها،  برای ارتباط با یکدیگر از  _رابط‌ها_ (اینترفیس) استفاده می کنند؛ مجموعه‌ای محدود از توابع یا متغیرها که قابلیت‌های مفیدی در سطح بالاتری از انتزاع ایجاد می کنند  و جزئیات دقیق پیاده سازی‌شان را مخفی می کنند.

{{index "public properties", "private properties", "access control", [method, interface]}}

قسمت‌های این‌گونه برنامه را  به وسیله‌ی اشیاء مدل‌سازی می کنند. رابط آن ها شامل مجموعه‌ای مشخص از متدها و خاصیت‌ها می باشد. خاصیت‌هایی را که بخشی از رابط محسوب می شوند، _عمومی_ (public) و آن هایی را که کدهای بیرونی نباید به آن‌ها دسترسی داشته باشند، خاصیت‌ها _خصوصی_ (private) می نامند.

{{index "underscore character"}}

بسیاری از زبان‌های برنامه نویسی راهی برای تمایز خاصیت‌های خصوصی و عمومی فراهم می سازند و از دسترسی کدهای بیرونی به خاصیت‌های خصوصی جلوگیری می کنند.  جاوااسکریپت بار دیگر انتخاب روش مینیمال، این گونه عمل نمی کند – حداقل تا الان. البته کارهایی برای افزودن این ویژگی به زبان در حال شکل گیری است.

با وجود اینکه خود زبان به صورت درونی از این تمایز پشتیبانی نمی کند، برنامه نویسان جاوااسکریپت این مفهوم را به شکل موفقیت‌آمیزی به کار می برند. معمولا برای مشخص کردن رابط‌های در دسترس از توضیحات یا مستندات برنامه استفاده می کنند. همچنین یک روش رایج برای مشخص کردن خاصیت‌های خصوصی، استفاده از کاراکتر زیرخط (`_`) در شروع این خاصیت‌ها است.

جدا کردن رابط از پیاده‌‌سازی، ایده‌ی بسیار خوبی است. این کار را معمولا _((کپسوله‌سازی))_ یا مخفی‌سازی می نامند.

{{id obj_methods}}

## متد‌ها

{{index "rabbit example", method, [property, access]}}
متدها خاصیت‌هایی هستند که مقدار‌های تابع را نگه‌داری می کنند؛ همین. این یک متد ساده است:

```
let rabbit = {};
rabbit.speak = function(line) {
  console.log(`The rabbit says '${line}'`);
};

rabbit.speak("I'm alive.");
// → The rabbit says 'I'm alive.'
```

{{index "this binding", "method call"}}

معمولا متد‌ها کاری را روی شیءای که بر روی آن فراخوانی شده‌اند انجام می دهند. . زمانی که یک تابع به عنوان یک متد فراخوانی می شود – به عنوان یک خاصیت جستجو می شود و بلافاصله فراخوانی می شود مانند <bdo>`object.method()`</bdo> – متغیری که `this` نامیده می‌شود به طور خودکار به شیءای که روی آن فراخوانی شده است اشاره می کند.

```{includeCode: "top_lines:6", test: join}
function speak(line) {
  console.log(`The ${this.type} rabbit says '${line}'`);
}
let whiteRabbit = {type: "white", speak};
let hungryRabbit = {type: "hungry", speak};

whiteRabbit.speak("Oh my ears and whiskers, " +
                  "how late it's getting!");
// → The white rabbit says 'Oh my ears and whiskers, how
//   late it's getting!'
hungryRabbit.speak("I could use a carrot right now.");
// → The hungry rabbit says 'I could use a carrot right now.'
```

{{id call_method}}

{{index "call method"}}

می توانید `this` را یک پارامتر اضافه در نظر بگیرید که به شکلی دیگر ارسال شده است. اگر بخواهید آن آشکارا ارسال نمایید، می توانید از متد `call` توابع استفاده کنید. این متد مقدار `this` را به عنوان آرگومان اول می گیرد و دیگر آرگومان ها را به عنوان پارامترهای معمولی تفسیر می کند.

```
speak.call(hungryRabbit, "Burp!");
// → The hungry rabbit says 'Burp!'
```

به دلیل اینکه هر تابع متغیر `this` مربوط به خود را دارد، که مقدارش بستگی به نحوه‌ی فراخوانی آن تابع دارد، نمی توان به `this` محصور در  حوزه‌ی تعریف یک تابع معمولی (که با کلیدواژه‌ی `function` تعریف شده است) دسترسی داشت.

{{index "this binding", "arrow function"}}

توابع پیکانی (arrow functions) متفاوت عمل می کنند - این توابع `this` را به جایی مقید نمی کنند اما می توانند متغیر `this` قلمروی پیرامونشان را ببینند. بنابراین، به وسیله‌ی کدی مانند مثال زیر، می توان به `this` از درون یک تابع محلی اشاره کرد:

```
function normalize() {
  console.log(this.coords.map(n => n / this.length));
}
normalize.call({coords: [0, 2, 3], length: 5});
// → [0, 0.4, 0.6]
```

{{index "map method"}}

اگر آرگومان ارسالی به تابع `map` را به وسیله کلیدواژه‌ی `function` نوشته‌ بودم، کد بالا کار نمی کرد.

{{id prototypes}}

## Prototype ها

{{index "toString method"}}

با دقت به کد زیر نگاه کنید.

```
let empty = {};
console.log(empty.toString);
// → function toString(){…}
console.log(empty.toString());
// → [object Object]
```

{{index magic}}

خاصیتی را از یک شیء تهی بیرون کشیده‌ام. جادوگری!

{{index [property, inheritance], [object, property]}}

البته واقعا این‌طور نیست. فقط بخشی از اطلاعات مربوط به شیوه‌ی کارکرد اشیاء را در جاوااسکریپت، هنوز توضیح نداده بودم. بیشتر اشیاء علاوه بر خاصیت‌های خودشان، خاصیتی به نام _((prototype))_ دارند. یک prototype (نمونه‌ی اولیه) خود یک شیء دیگر است که به عنوان یک منبع جایگزین برای خاصیت‌ها استفاده می شود. زمانی که یک شیء، درخواستی برای یک خاصیت دریافت می کند که، آن خاصیت را ندارد، جستجو در prototype اش صورت می گیرد، سپس prototype متعلق prototype شیء مورد نظر، جستجو می شود و این روند ادامه دارد.

{{index "Object prototype"}}

بنابراین نمونه‌ی اولیه‌ی (prototype) آن شیء تهی کدام است؟ بله آن prototype جد بزرگ شیء می باشد که تقریبا پشت همه‌ی اشیاء قرار دارد، <bdo>`Object.prototype`</bdo>.

```
console.log(Object.getPrototypeOf({}) ==
            Object.prototype);
// → true
console.log(Object.getPrototypeOf(Object.prototype));
// → null
```

{{index "getPrototypeOf function"}}

همانطور که حدس می زنید، <bdo>`object.getPrototypeOf`</bdo>، برای گرفتن prototype یک شیء استفاده می شود.

{{index "toString method"}}

روابط بین prototype ها در اشیاء جاوااسکریپت، یک ساختار درختی را شکل می دهند که در ریشه‌ی این ساختار، <bdo>`Object.prototype`</bdo> قرار می گیرد. درون این شیء چند متد وجود دارد که در تمامی اشیاء حضور دارند، مانند `toString`، که عمل تبدیل یک شیء به رشته‌ را انجام می دهد.

{{index inheritance, "Function prototype", "Array prototype", "Object prototype"}}

خیلی از اشیاء به طور مستقیم دارای <bdo>`Object.prototype`</bdo> به عنوان ((prototype)) خودشان نیستند، اما در عوض شیء دیگری دارند که مجموعه‌ی متفاوتی از خاصیت‌های پیش‌فرض را فراهم می سازد. توابع از <bdo>`Function.prototype`</bdo> و آرایه ها از <bdo>`Array.prototype`</bdo> مشتق شده اند.

```
console.log(Object.getPrototypeOf(Math.max) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf([]) ==
            Array.prototype);
// → true
```

{{index "Object prototype"}}

این گونه prototype ها خود نیز دارای یک prototype می باشند که اغلب همان <bdo>`Object.prototype`</bdo> است. پس به طور غیر مستقیم متدهایی شبیه `toString` را فراهم می کنند.

{{index "rabbit example", "Object.create function"}}

می توانید از متد <bdo>`Object.create`</bdo> برای ساختن یک شیء با یک ((prototype)) خاص استفاده کنید.

```
let protoRabbit = {
  speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
};
let killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!");
// → The killer rabbit says 'SKREEEE!'
```

{{index "shared property"}}

خاصیتی شبیه به <bdo>`speak(line)`</bdo> در یک تعریف شیء، شکل کوتاه تعریف یک متد است. این کار خاصیتی به نام `speak` را تعریف کرده و یک تابع را به عنوان مقدار به آن می دهد.

"protoRabbit" به عنوان یک ظرف برای خاصیت‌های مشترک همه‌ی خرگوش‌ها استفاده می شود. یک شی مجزای rabbit (خرگوش) مثل killerRabbit، دارای خاصیت‌های اختصاصی است که فقط متعلق به خودش – و در این مثال متعلق به نوع خودش – است و نیز خاصیت‌های مشترکی دارد که آن‌ها را از نمونه‌ی اولیه‌اش می گیرد.

{{id classes}}

## کلاس‌ها

{{index "object-oriented programming"}}

سیستم ((prototype)) جاوااسکریپت را می توان به عنوان برداشتی نسبتا غیر رسمی از مفهوم _((کلاس‌ها))_ در برنامه‌نویسی شیء گرا  تفسیر کرد. یک کلاس، سرشت نوعی از شیء را تعریف می کند – متدها و خاصیت‌هایی که دارد. به اشیائی که از کلاس ها ایجاد می شوند، _((نمونه‌های))_ یک کلاس می گویند.

{{index [property, inheritance]}}

prototype ها برای تعریف خاصیت‌هایی مفید می باشند که مقدار مشابهی را در طول همه‌ی نمونه‌های یک کلاس به اشتراک می گذارند، مانند متدها. خاصیت‌هایی که برای هر نمونه متفاوت هستند، مانند خاصیت `type` در مثال خرگوش بایستی به طور مستقیم در خود اشیاء ذخیره بشوند.

{{id constructors}}

بنابراین برای اینکه بتوان نمونه‌ای از یک کلاس داده شده را ساخت، باید شیءای را ایجاد کنید که از یک prototype درست گرفته شده است، _همچنین_ باید مطمئن شوید که این شیء، خاصیت‌هایی که نمونه‌های کلاس قرار است از پیش داشته باشند را دارد. این کاری است که یک تابع _((سازنده))_ انجام می دهد.

```
function makeRabbit(type) {
  let rabbit = Object.create(protoRabbit);
  rabbit.type = type;
  return rabbit;
}
```

{{index "new operator", "this binding", "return keyword", [object, creation]}}

جاوااسکریپت راهی را فراهم ساخته که بتوان این گونه توابع را آسان تر تعریف کرد. اگر در ابتدای فراخوانی یک تابع کلیدواژه‌ی `new` را قرار دهید ، آن تابع به عنوان تابع سازنده عمل خواهد کرد. این بدان معناست که یک شیء با prototype صحیح به طور خودکار ساخته شده ، به `this`  درون تابع مقید می شود و در انتهای تابع برگردانده می شود.

{{index "prototype property"}}

شیء prototype استفاده شده برای ساختن شیء مورد نظر را می توان با دسترسی به خاصیت `prototype` از تابع سازنده پیدا کرد.

{{index "rabbit example"}}

```
function Rabbit(type) {
  this.type = type;
}
Rabbit.prototype.speak = function(line) {
  console.log(`The ${this.type} rabbit says '${line}'`);
};

let weirdRabbit = new Rabbit("weird");
```

{{index constructor}}

سازنده‌ها (در حقیقت همه توابع) به طور خودکار خاصیتی به نام `prototype` را می گیرند، که به صورت پیش فرض، یک شیء خالی که از <bdo>`Object.prototype`</bdo> گرفته شده است را نگه داری می کند. اگر بخواهید  می توانید این خاصیت را تغییر دهید و با شیءای دیگر عوض کنید. یا می توانید به شیء خالی فعلی خاصیت‌هایی را اضافه کنید همانطور که در مثال این کار انجام شد.

{{index capitalization}}

براساس عرف، نام سازنده‌ها را با حروف بزرگ شروع می کنند تا بتوان به سادگی بین آن ها و توابع معمولی تمییز داد.

{{index "prototype property", "getPrototypeOf function"}}

درک تفاوت بین چگونگی ارتباط یک prototype با یک سازنده ( توسط خاصیت‌ `prototype`اش) و اینکه اشیاء چگونه می تواند prototype داشته باشند ( که می تواند به وسیله <bdo>`Object.getPrototypeOf`</bdo>  بدست آید) اهمیت دارد. prototype واقعی یک سازنده، در واقع <bdo>`Function.prototype`</bdo> است، به این خاطر که سازنده‌ها از نوع تابع به شمار می آیند. _خاصیت_ `prototype` یک سازنده، نمونه‌ی اولیه‌ای را نگه‌داری می کند که ساخت نمونه‌های اشیاء از روی آن صورت می گیرد.

```
console.log(Object.getPrototypeOf(Rabbit) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf(weirdRabbit) ==
            Rabbit.prototype);
// → true
```

## استفاده از نماد class

بنابراین ((کلاس‌های)) جاوااسکریپت، همان توابع ((سازنده)) به همراه یک خاصیت ((prototype)) می باشند. به همین صورت نیز کار می کنند و تا پیش از سال 2015، این روش تنها راهی بود که باید نوشته می شدند. این روز‌ها، روش مناسب‌تری برای پیاده‌سازی کلاس‌ها در اختیار داریم.

```{includeCode: true}
class Rabbit {
  constructor(type) {
    this.type = type;
  }
  speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
}

let killerRabbit = new Rabbit("killer");
let blackRabbit = new Rabbit("black");
```

{{index "rabbit example", [braces, class]}}

کلیدواژه‌ی `class` موجب شروع یک اعلان کلاس می شود که به ما این امکان را می دهد تا سازنده‌ و مجموعه‌ی متدها را یکجا تعریف کنیم. هر تعداد متدی که نیاز باشد را می توان درون کروشه‌های تعریف کلاس قرار داد. متدی که با نام `constructor` نوشته می شود، به صورت خاصی تفسیر می شود. این متد تابع سازنده‌ی واقعی را فراهم می سازد که به نام `Rabbit` قید خواهد خورد. ما بقی متدها درون prototype سازنده بسته‌بندی می شوند. بنابراین، تعریف کلاس به شکل بالا، معادل تعریف سازنده در قسمت قبل است. فقط زیباتر به نظر می رسد.

{{index ["class declaration", properties]}}

در تعریف کلاس فقط می توان _متدها_ – خاصیت‌هایی که توابع را نگه‌داری می کنند – را برای اضافه شدن به ((prototype)) تعریف کرد. این محدودیت می تواند در مواقعی که قصد دارید مقداری از نوع غیر تابع را ذخیره کنید مشکل ایجاد کند. می توانید برای این گونه خاصیت‌ها همچنان به صورت مستقیم prototype را بعد از تعریف کلاس تغییر دهید.

`class` درست شبیه `function` می تواند به صورت عبارت و دستور استفاده شود. اگر به عنوان عبارت استفاده شود، متغیری تعریف نکرده و سازنده را به عنوان یک مقدار تولید می کند. می توانید نام کلاس را در این روش از تعریف حذف کنید.

```
let object = new class { getWord() { return "hello"; } };
console.log(object.getWord());
// → hello
```

## بازنویسی و تغییر خاصیت‌های مشتق شده

{{index "shared property", overriding, [property, inheritance]}}

هنگامی که خاصیتی را به یک شیء اضافه می کنید، فارغ از اینکه در prototype آن وجود داشته باشد یا خیر، خاصیت مورد نظر به _خود_ شیء اضافه خواهد شد. در صورتی وجود خاصیتی با همین نام در prototype، خاصیت موجود در prototype بی اثر خواهد بود و پشت خاصیت خود شیء پنهان می شود.

```
Rabbit.prototype.teeth = "small";
console.log(killerRabbit.teeth);
// → small
killerRabbit.teeth = "long, sharp, and bloody";
console.log(killerRabbit.teeth);
// → long, sharp, and bloody
console.log(blackRabbit.teeth);
// → small
console.log(Rabbit.prototype.teeth);
// → small
```

{{index [prototype, diagram]}}

نمودار زیر شرایطی را به تصویر می کشد که بعد از اجرای کد بالا رخ می دهد. prototypeهای `Rabbit` و `Object` پشت `killerRabbit` قرار می گیرند، مانند نوعی پس‌زمینه، که در صورت نبودن خاصیت‌ها در خود شیء، به آن‌ها رجوع می شود.

{{figure {url: "img/rabbits.svg", alt: "Rabbit object prototype schema",width: "8cm"}}}

{{index "shared property"}}

تغییر و بازنویسی خاصیت‌هایی که در پروتوتایپ وجود دارند می تواند کاربرد داشته باشد. همانطور که در مثال rabbit teeth (خاصیت teeth در شیء خرگوش) نشان داده شد، می توان از آن برای مشخص کردن خاصیت های استثناء در نمونه‌ اشیاء یک کلاس عمومی‌تر استفاده کرد و اجازه داد اشیاء معمول مقدار استاندارد را از prototype خود دریافت کنند.

{{index "toString method", "Array prototype", "Function prototype"}}

بازنویسی به این شکل (overridding)، همچنین برای تعریف نسخه‌ی متفاوتی از متد `toString` برای prototypeهای استاندارد تابع(function) و آرایه (array) استفاده می شود. متدی که با `toString` پیش‌فرض شیء پایه متفاوت است.

```
console.log(Array.prototype.toString ==
            Object.prototype.toString);
// → false
console.log([1, 2].toString());
// → 1,2
```

{{index "toString method", "join method", "call method"}}

فراخوانی `toString` بر روی یک آرایه نتیجه‌ای شبیه فراخوانی <bdo>`.join(",")`</bdo> روی آن را خواهد داشت – که باعث می شود بین مقادیر آرایه ویرگول قرار گیرد. فراخوانی مستقیم <bdo>`Object.prototype.toString`</bdo> با یک آرایه، رشته‌ی متفاوتی را تولید می کند. این تابع چیزی در مورد آرایه ها نمی داند، پس خیلی ساده واژه‌ی _object_ و نام نوع داده را بین یک جفت براکت چاپ می کند.

```
console.log(Object.prototype.toString.call([1, 2]));
// → [object Array]
```

## ساختار داده‌ی Map

{{index "map method"}}

با واژه‌ی _map_ در [فصل پیش](higher_order#map) آشنا شدیم. از آن برای تغییر یک ساختار داده به وسیله‌ی اعمال یک تابع به عناصر آن استفاده شد. درست است که شاید کمی گیج‌کننده باشد اما در برنامه‌نویسی، همین واژه برای موضوع مرتبط و نسبتا متفاوتی نیز استفاده می شود.

{{index "map (data structure)", "ages example", ["data structure", map]}}

یک _map_ یک ساختار داده است که مقدارهایی را (کلید‌ها) به مقدارهای دیگر مرتبط می سازد. به عنوان مثال، ممکن است بخواهید اسم‌ها را به سن‌ها نگاشت (map) کنید. می توان از یک شیء برای این کار استفاده کرد.

```
let ages = {
  Boris: 39,
  Liang: 22,
  Júlia: 62
};

console.log(`Júlia is ${ages["Júlia"]}`);
// → Júlia is 62
console.log("Is Jack's age known?", "Jack" in ages);
// → Is Jack's age known? false
console.log("Is toString's age known?", "toString" in ages);
// → Is toString's age known? true
```

{{index "Object.prototype", "toString method"}}

در این مثال نام خاصیت های شیء، برابر با نام اشخاص است و مقدار‌ این خاصیت‌ها برابر سن شان. اما بی شک، در بین اسامی، کسی به نام toString نداشته‌ایم. بله به دلیل اینکه اشیاء ساده، از <bdo>`Object.prototype`</bdo> مشتق شده اند، به نظر می رسد که این خاصیت آنجا وجود دارد.

{{index "Object.create function", prototype}}

بدین لحاظ، استفاده از اشیاء ساده به جای map خطراتی دارد. راه های متفاوتی برای فرار از این مشکل وجود دارد. اول اینکه می توان شیءای را _بدون_ prototype ایجاد کرد. اگر به متد <bdo>`Object.create`</bdo> مقدار `null` را بفرستید، شیء تولیدی دیگر از روی <bdo>`Object.prototype`</bdo> ساخته نمی شود و می توان با خیال راحت به عنوان map از آن استفاده کرد.

```
console.log("toString" in Object.create(null));
// → false
```

{{index [property, naming]}}

نام خاصیت یک شیء باید از نوع رشته‌ باشد. اگر به یک نگاشت نیاز داشته باشید که کلید‌های آن را نتوان به سادگی به رشته تبدیل کرد (مثل استفاده اشیاء به عنوان کلید)، نمی توانید برای پیاده‌سازی آن نگاشت از یک شیء استفاده کنید.

{{index "Map class"}}

خوشبختانه، جاوااسکریپت کلاسی به نام `Map` را فراهم ساخته است که دقیقا برای همین هدف نوشته شده است. این کلاس برای ذخیره‌ی نگاشت ها با هر نوع کلیدی استفاده می شود.

```
let ages = new Map();
ages.set("Boris", 39);
ages.set("Liang", 22);
ages.set("Júlia", 62);

console.log(`Júlia is ${ages.get("Júlia")}`);
// → Júlia is 62
console.log("Is Jack's age known?", ages.has("Jack"));
// → Is Jack's age known? false
console.log(ages.has("toString"));
// → false
```

{{index [interface, object], "set method", "get method", "has method", encapsulation}}

متدهای `get` و `set` و `has` بخش‌هایی از رابط شیء `Map` هستند. نوشتن ساختار داده‌ای که بتوان به وسیله‌ی آن مجموعه‌ی بزرگی از مقدارها را به سرعت به روز رسانی و جستجو کرد کار ساده ای نیست، اما نیازی نیست ما نگران آن باشیم. کسانی قبلا این کار را برای ما انجام داده اند و می توانیم به سراغ این رابط ساده برویم و از حاصل کار آن ها استفاده کنیم.

{{index "hasOwnProperty method", "in operator"}}

اگر شیء ساده‌ای دارید و بنا به دلایلی لازم است از آن به عنوان یک ساختار map استفاده کنید، لازم است بدانید که `Object.keys` فقط کلید‌های _خود_ یک شیء را برمی گرداند نه آن‌هایی که در prototype آن قرار دارند. به عنوان یک جایگزین برای عملگر `in،` می توانید از متد  `hasOwnProperty` استفاده کنید که پروتوتایپ شیء را در نظر نمی گیرد.

```
console.log({x: 1}.hasOwnProperty("x"));
// → true
console.log({x: 1}.hasOwnProperty("toString"));
// → false
```

## چندریختی

{{index "toString method", "String function", polymorphism, overriding, "object-oriented programming"}}

زمانی که تابع `String` ( که یک مقدار را به رشته تبدیل می کند) را روی یک شیء فراخوانی می کنید، متد `toString` آن شیء فراخوانی می شود و سعی می کند تا رشته‌ای معنادار از شیء مورد نظر تولید کند. پیش‌تر اشاره کردم که بعضی از prototypeهای استاندارد، نسخه‌ی `toString` اختصاصی خودشان را تعریف می کنند تا با این کار بتوانند اطلاعات مفیدتری نسبت به <bdo>`"[object Object]"`</bdo>  تولید کنند. شما نیز می توانید این کار را انجام دهید.

```{includeCode: "top_lines: 3"}
Rabbit.prototype.toString = function() {
  return `a ${this.type} rabbit`;
};

console.log(String(blackRabbit));
// → a black rabbit
```

{{index "object-oriented programming", [interface, object]}}

این نمونه‌ای ساده از یک ایده‌ی قدرتمند است. زمانی که کدی نوشته می شود تا با اشیائی کار کند که دارای یک رابط خاص هستند – در این مثال، یک متد `toString` – می توان با این کد، هر شیء دیگری را که از این رابط پشتیبانی می کند، شامل نمود و به درستی استفاده کرد.

این تکنیک را چندریختی می گویند.  یک کد _چندریخت_  می تواند با مقدارهایی از شکل‌های مختلف کار کنند مادامیکه این شکل‌ها رابطی که کد انتظارش را دارد پشتیبانی کند.

{{index "for/of loop", "iterator interface"}}

در [فصل ?](data#for_of_loop) اشاره کردم که با استفاده از حلقه‌ی <bdo>`for`/`of`</bdo> می توان ساختارهای داده‌ی مختلف را پیمایش کرد. این یک مورد دیگر از چندریختی محسوب می شود – این گونه حلقه‌ها از ساختار داده انتظار دارند که رابط‌ خاصی را در دسترس حلقه قرار دهند، رابطی که آرایه‌ها و رشته ها فراهم می سازند. و شما نیز می توانید این رابط را به اشیاء خودتان اضافه کنید! اما قبل از اینکه بتوانیم این کار را بکنیم، لازم است بدانیم که سمبل (symbol) چیست.

## سمبل‌ها - Symbols

می توان در رابط‌های متعدد، از یک نام یکسان برای کارهای مختلف استفاده کرد. مثلا، من می توانم رابطی تعریف کنم که در آن متد `toString` مثلا یک شیء را به یک تکه ریسمان تبدیل کند. اما یک شیء نمی تواند هم از رابطی که تعریف کردیم و هم پیاده‌سازی استاندارد `toString` مطابقت کند.

این کار نه ایده‌ی خوبی است و نه مشکل رایجی محسوب می شود. بیشتر برنامه‌نویسان جاوااسکریپت به آن فکر هم نمی کنند. اما طراحان زبان، همان افرادی که _شغلشان_ فکر کردن به همین موضوعات است، در هر صورت راه حلی برای ما فراهم ساخته اند.

{{index "Symbol function", [property, naming]}}

پیش‌تر که ادعا کردم نام خاصیت‌ها از جنس رشته هستند، عبارت کاملا دقیقی استفاده نکرده بودم.  بله معمولا از جنس رشته‌اند اما می توانند از نوع _((symbol))_ نیز باشند. سمبل‌ها مقادیری هستند که با تابع `Symbol` ایجاد می شوند. برخلاف رشته‌ها، سمبل‌هایی که تازه ایجاد می شوند یکتا هستند – نمی توان یک سمبل یکسان را دوبار ایجاد کرد.

```
let sym = Symbol("name");
console.log(sym == Symbol("name"));
// → false
Rabbit.prototype[sym] = 55;
console.log(blackRabbit[sym]);
// → 55
```

رشته‌ای که به تابع `Symbol` ارسال می کنید در هنگام تبدیل آن به رشته استفاده می شود و می تواند شناسایی سمبل را مثلا هنگام نشان دادن در کنسول ساده تر کند. فارغ از آن معنای دیگری ندارد - می توان چندین سمبل را با یک نام تعریف کرد.

منحصر به فرد بودن و امکان استفاده به عنوان نام یک خاصیت، باعث می شود که استفاده از سمبل‌ها، گزینه‌ی مناسبی برای تعریف رابط‌هایی باشد که می توانند بی دردسر در کنار دیگر خاصیت‌ها بدون توجه به نام آن ها تعریف شوند.

```{includeCode: "top_lines: 1"}
const toStringSymbol = Symbol("toString");
Array.prototype[toStringSymbol] = function() {
  return `${this.length} cm of blue yarn`;
};

console.log([1, 2].toString());
// → 1,2
console.log([1, 2][toStringSymbol]());
// → 2 cm of blue yarn
```

{{index [property, naming]}}

می توان در تعریف شیء یا کلاس، خاصیت‌هایی که نامشان از جنس symbol است را با قراردادن براکت دور نامشان استفاده نمود. این کار باعث می شود که نام خاصیت ارزیابی شود، بسیار شبیه به استفاده از براکت برای دسترسی به خاصیت‌ها، که باعث می شود بتوانیم به متغیری که یک سمبل را نگه‌داری می کند اشاره کنیم.

```
let stringObject = {
  [toStringSymbol]() { return "a jute rope"; }
};
console.log(stringObject[toStringSymbol]());
// → a jute rope
```

## رابط تکرارکننده

{{index "iterable interface", "Symbol.iterator symbol", "for/of loop"}}

انتظار می رود شیءای که به حلقه‌ی <bdo>`for`/`of`</bdo> داده می شود _قابل تکرار_ باشد. یعنی دارای یک متد است که به وسیله‌ی <bdo>`Symbol.iterator`</bdo> نام گذاری شده است ( مقداری از نوع سمبل که توسط خود زبان تعریف شده است و به عنوان یک خاصیت از تابع `Symbol` ذخیره می شود).

{{index "iterator interface", "next method"}}

وقتی این متد فراخوانی می شود، خروجی آن شیءای خواهد بود که رابط دومی را فراهم می سازد، رابط _تکرارکننده_. این چیزی است که عمل تکرار را انجام می دهد. این تکرارکننده دارای متدی به نام `next` است که نتیجه‌ی بعدی را برمی گرداند. این نتیجه بایستی یک شیء باشد که خاصیتی به نام  `value` دارد که مقدار بعدی را در صورت وجود نگه‌داری می کند و خاصیتی به نام `done` دارد که در صورت نبود نتیجه‌ای دیگر، برابر با true خواهد و و در غیر این صورت false را خواهد داشت.

توجه داشته باشید که نام خاصیت های `next،` `value` و `done` از نوع رشته‌ی ساده است نه از جنس سمبل. فقط <bdo>`Symbol.iterator`</bdo> است که در واقع از جنس سمبل است و احتمالا به اشیاء متفاوت _زیادی_ اضافه خواهد شد.

می توانیم مستقیما از این رابط استفاده کنیم.

```
let okIterator = "OK"[Symbol.iterator]();
console.log(okIterator.next());
// → {value: "O", done: false}
console.log(okIterator.next());
// → {value: "K", done: false}
console.log(okIterator.next());
// → {value: undefined, done: true}
```

{{index "matrix example", "Matrix class", [array, "as matrix"]}}

{{id matrix}}

بیایید یک ساختار قابل تکرار را پیاده سازیم کنیم. در مثال زیر کلاسی به نام _matrix_ خواهیم ساخت که مانند یک آرایه‌ی دوبعدی عمل می کند

```{includeCode: true}
class Matrix {
  constructor(width, height, element = (x, y) => undefined) {
    this.width = width;
    this.height = height;
    this.content = [];

    for (let y = 0; y < height; y++) {
      for (let x = 0; x < width; x++) {
        this.content[y * width + x] = element(x, y);
      }
    }
  }

  get(x, y) {
    return this.content[y * this.width + x];
  }
  set(x, y, value) {
    this.content[y * this.width + x] = value;
  }
}
```

کلاس بالا محتوای خود را در یک آرایه به تعداد عناصر <bdo>_width_ × _height_</bdo> ذخیره می کند. عناصر به صورت ردیف به ردیف ذخیره می شوند، بنابراین به عنوان مثلا عنصر سوم در ردیف پنجم در موقعیت <bdo>4 × _width_ + 2</bdo> ذخیره می شود ( با در نظر داشتن اندیس گذاری از صفر).

تابع سازنده، یک طول، یک عرض و تابعی اختیاری برای محتوا می گیرد که این تابع برای مقداردهی اولیه استفاده می شود.  متدهای `get` و `set` برای به روز رسانی عناصر و دریافت آن ها در matrix‌ تعریف شده اند.

در زمان پیمایش یک ماتریس، معمولا دانستن موقعیت عناصر به اندازه‌ی خود عناصر مهم هستند، بنابراین تکرارکننده‌ی ما اشیائی با خاصیت‌های `x` و `y` و `value`  تولید می کند.

{{index "MatrixIterator class"}}

```{includeCode: true}
class MatrixIterator {
  constructor(matrix) {
    this.x = 0;
    this.y = 0;
    this.matrix = matrix;
  }

  next() {
    if (this.y == this.matrix.height) return {done: true};

    let value = {x: this.x,
                 y: this.y,
                 value: this.matrix.get(this.x, this.y)};
    this.x++;
    if (this.x == this.matrix.width) {
      this.x = 0;
      this.y++;
    }
    return {value, done: false};
  }
}
```

آمار پیشرفت تکرار در طول یک ماتریس توسط خاصیت‌های `x` و `y` ضبط و ثبت می شود. متد `next` با بررسی اینکه آیا به انتهای ماتریس رسیده ایم یا خیر شروع می کند. اگر نرسیده بود، _ابتدا_ شیءای را ایجاد می کند که مقدار فعلی را نگه داری کند و _سپس_ موقعیت آن را به روز رسانی می کند و در صورت نیاز به سراغ ردیف بعدی رفت.

اجازه بدهید که کلاس `Matrix` را قابل تکرار کنیم. در این کتاب، گاهی از بعد از تعریف کلاس‌ها، prototype را دستکاری خواهم کرد تا متدهایی را به آن‌ها اضافه کنم، در نتیجه بخش‌های کدهای مجزا کوچک خواهند ماند و به دیگر قسمت‌ها وابسته نخواهند شد. در یک برنامه‌ی معمولی، جایی که نیازی نیست تا کدها را به قسمت‌های کوچکتر تقسیم کرد، می توانید این متدها را مستقیما درون بدنه کلاس تعریف کنید.

```{includeCode: true}
Matrix.prototype[Symbol.iterator] = function() {
  return new MatrixIterator(this);
};
```

{{index "for/of loop"}}

اکنون می توانیم یک ماتریس را به وسیله‌ی <bdo>`for`/`of`</bdo> پیمایش کنیم.

```
let matrix = new Matrix(2, 2, (x, y) => `value ${x},${y}`);
for (let {x, y, value} of matrix) {
  console.log(x, y, value);
}
// → 0 0 value 0,0
// → 1 0 value 1,0
// → 0 1 value 0,1
// → 1 1 value 1,1
```

## گیرنده‌ها(getters) ، گذارنده‌ها(setters) و متدهای ایستا (static)

{{index [interface, object], [property, definition], "Map class"}}

رابط‌ها بیشتر از متد‌ها تشکیل شده اند، اما می می توانند خاصیت‌هایی که مقادیر تابعی را نگه داری نمی کنند را نیز داشته باشند. به عنوان مثال، اشیاء `Map` دارای خاصیتی به نام `size` می باشند که تعداد کلید‌هایی که در آن‌ها ذخیره شده است را نگه داری می کند.

در این‌گونه اشیاء لزومی ندارد خاصیتی مثل `size` را مستقیما در خود نمونه شیء محاسبه و ذخیره نمود. حتی خاصیت‌هایی که به صورت مستقیم در دسترس هستند، ممکن است متدی را مخفیانه فراخوانی کنند. این گونه‌ی خاصیت‌ها را _((getter))_ یا گیرنده‌ی مقدار می گویند که به وسیله‌ی نوشتن `get` در ابتدای نام یک متد، در یک عبارت تعریف شیء یا کلاس، تعریف می شوند.

```{test: no}
let varyingSize = {
  get size() {
    return Math.floor(Math.random() * 100);
  }
};

console.log(varyingSize.size);
// → 73
console.log(varyingSize.size);
// → 49
```

{{index "temperature example"}}

زمانی که کسی مقدار خاصیت‌ `size` را درخواست می کند، متدی که به آن پیوند خورده است فراخوانی می شود. می توانید مار مشابهی را برای مقدار دهی به یک خاصیت هم انجام دهید که به آن _((setter))_ یا گذارنده می گویند.

```{test: no, startCode: true}
class Temperature {
  constructor(celsius) {
    this.celsius = celsius;
  }
  get fahrenheit() {
    return this.celsius * 1.8 + 32;
  }
  set fahrenheit(value) {
    this.celsius = (value - 32) / 1.8;
  }

  static fromFahrenheit(value) {
    return new Temperature((value - 32) / 1.8);
  }
}

let temp = new Temperature(22);
console.log(temp.fahrenheit);
// → 71.6
temp.fahrenheit = 86;
console.log(temp.celsius);
// → 30
```

کلاس `temperature` در مثال بالا این امکان را فراهم می کند تا میزان دما را به صورت ((سلسیوس)) یا ((فارنهایت)) بنویسید، اما در درون کلاس، این مقدار فقط به سلسیوس ذخیره می شود و به طور خودکار توسط متدهای گیرنده و گذارنده (setter, getter) به ‍`fahrenheit` تبدیل می شود.

{{index "static method"}}

گاهی اوقات می خواهید تا بعضی خاصیت‌ها را به جای prototype، به طور مستقیم در تابع سازنده داشته باشید. این گونه متدها به نمونه‌ی کلاس دسترسی نخواهند داشت اما می توان از آن‌ها به عنوان روش‌های دیگر ایجاد نمونه‌ها استفاده کرد.

درون تعریف یک کلاس، متدهای که کلیدواژه‌ی `static` در ابتدای آن ها نوشته می شود، روی تابع سازنده ذخیره می شوند. بنابراین در کلاس `Temperature` می توانید برای تولید دما به وسیله‌ی درجه‌ی فارنهایت از<bdo>`Temperature.fromFahrenheit(100)`</bdo> استفاده کنید.

## ارث‌بری

{{index inheritance, "matrix example", "object-oriented programming", "SymmetricMatrix class"}}

بعضی از ماتریس‌ها را به عنوان ماتریس‌های _متقارن_ می شناسند. اگر یک ماتریس متقارن را حول قطر بالا-چپ-به-پایین-راست بازتاب کنید، تفاوتی در شکل آن ایجاد نمی شود. به بیان دیگر، مقدار موجود در _x_,_y_ همیشه مشابه مقدار _‌y_,_x_ است.

تصور کنید که به یک ساختار داده مانند `Matrix` نیاز داریم با این شرط که متقارن بودن و ماندن ماتریس را ضمانت کند. می توانیم چنین ساختار داده‌ای را از صفر بنوسیم، اما این کار باعث می شود که کدهایی را تکرار کنیم که قبلا شبیه‌شان را نوشته ایم.

{{index overriding, prototype}}

سیستم prototype در جاوااسکریپت این امکان را فراهم کرده است که یک کلاس جدید را بر اساس یک کلاس دیگر اما با بازتعریف بعضی از خاصیت‌های آن ایجاد کنیم. prototype کلاس جدید از prototype کلاس قبلی مشتق می شود اما تعریف جدیدی را برای به عنوان مثال، متد `set` آن در نظر می گیرد.

در اصطلاح برنامه نویسی شیء گرا به این کار ارث بری می گویند. کلاس جدید خاصیت‌ها و رفتار را از کلاسی دیگر به _((ارث می برد))_.

```{includeCode: "top_lines: 17"}
class SymmetricMatrix extends Matrix {
  constructor(size, element = (x, y) => undefined) {
    super(size, size, (x, y) => {
      if (x < y) return element(y, x);
      else return element(x, y);
    });
  }

  set(x, y, value) {
    super.set(x, y, value);
    if (x != y) {
      super.set(y, x, value);
    }
  }
}

let matrix = new SymmetricMatrix(5, (x, y) => `${x},${y}`);
console.log(matrix.get(2, 3));
// → 3,2
```

استفاده از واژه‌ی `extends` به این معنا است که این کلاس نباید بر اساس prototype پیش‌فرض `Object` ساخته شود بلکه بر اساس کلاس دیگری خواهد بود. این کلاس را _((superclass))_ (کلاس والد) می نامند. کلاسی که از آن گرفته می شود را _((subclass))_ (زیرکلاس) می گویند.

برای مقداردهی اولیه یک نمونه از  `SymmetricMatrix،` سازنده، تابع سازنده‌ی کلاس والدش (superclass) را به وسیله‌ کلیدواژه‌ی  `super` فراخوانی می کند. این کار لازم است به این دلیل که اگر این شیء جدید قرار است شبیه `Matrix` (به‌طورکلی) رفتار کند، به خاصیت‌هایی که ماتریس‌ها دارند نیاز پیدا خواهد کرد. برای اطمینان از متقارن بودن ماتریس، تابع سازنده، متد `content` را در بر می گیرد تا مختصات را برای مقادیر پایین قطر اصلی جابجا کند.

متد `set` دوباره از `super` استفاده می کند، اما این بار هدف، فراخوانی سازنده‌اش نیست. بلکه برای فراخوانی یک متد خاص از متدهای کلاس والد (superclass) می باشد. متد `set` را بازنویسی می کنیم اما می خواهیم از رفتار اصلی آن استفاده کنیم. به دلیل اینکه `this.set` به متد `set` _جدید_ اشاره می کند، نمی توان از آن استفاده کرد. درون متد‌های کلاس، کلیدواژه‌ی `super` راهی فراهم می سازد تا متد‌هایی که در کلاس والد تعریف شده اند را بتوان فراخوانی کرد.

با کمک ارث‌بری می توانیم با کار نسبتا کمتری، نوع‌ داده‌های متفاوتی از انواع داده‌ی موجود بسازیم. درکنار کپسوله‌سازی و چندریختی، ارث‌بری یکی از بخش‌های اساسی برنامه‌نویسی شیء گرا می باشد. البته دو ویژگی اول را عموما به عنوان ایده‌هایی فوق‌العاده می شناسند اما در باره‌ی ارث‌بری اختلاف‌ نظرهایی وجود دارد.

{{index complexity, reuse, "class hierarchy"}}

در حالیکه کپسوله‌سازی و چندریختی را می توان برای _جداسازی_ کدها و کاهش نابسامانی کل برنامه استفاده کرد، ((ارث‌بری)) اساسا کلاس‌ها را به هم وابسته می کند و به شکلی باعث ایجاد نابسامانی _بیشتر_ می شود. زمانی که از کلاسی ارث‌ می برید، نسبت به حالتی که فقط قصد استفاده از آن را دارید، باید اطلاعات بیشتری از نحوه‌ی کارکرد آن کلاس داشته باشید. ارث‌ بری می تواند ابزار مفیدی باشد و من گاهی از آن در برنامه‌هایم استفاده می کنم، اما نباید اولین گزینه‌ای باشد که به سراغش می روید. و احتمالا خوب نیست به دنبال فرصت‌هایی باشید که در آن‌ها سلسله مراتبی از کلاس‌ها را ایجاد کنید (مثل شجره‌نامه‌ای از کلاس‌ها).

## عملگر instanceof

{{index type, "instanceof operator", constructor, object}}

گاهی لازم است بدانیم که یک شیء از کلاس خاصی مشتق شده است یا خیر. برای این منظور، جاوااسکریپت یک عملگر دودویی به نام `instanceof` در نظر گرفته است.

```
console.log(
  new SymmetricMatrix(2) instanceof SymmetricMatrix);
// → true
console.log(new SymmetricMatrix(2) instanceof Matrix);
// → true
console.log(new Matrix(2, 2) instanceof SymmetricMatrix);
// → false
console.log([1] instanceof Array);
// → true
```

{{index inheritance}}

این عملگر انواع وارث را مورد کنکاش قرار می دهد، مثلا `SymmetricMatrix` نمونه‌ای از `Matrix` است. این عملگر را همچنین می توان برای سازنده‌های استاندارد مثل `Array` نیز استفاده کرد. تقریبا همه‌ی اشیاء نمونه‌ای از `Object` هستند.

## خلاصه

بنابراین اشیاء کاری بیش از نگه‌داری خاصیت‌های خود انجام می دهند. اشیاء prototype دارند که خود نیز اشیاء دیگری می باشند. تا زمانی که prototype یک شیء خاصیتی را داشته باشد، آن شیء نیز دارای آن خاصیت خواهد بود با وجود اینکه در ظاهر فاقد آن است. prototype اشیاء معمولی، <bdo>`Object.prototype`</bdo> می باشد.

می توان از توابع سازنده که معمولا نامشان با حروف بزرگ شروع می شود، با استفاده از کلیدواژه‌ی `new`، برای ایجاد اشیاء جدید استفاده کرد. prototype شیء ایجاد شده، شیءای است که در خاصیت‌ `prototype` سازنده پیدا می شود. می توان از این ویژگی برای قرار دادن همه‌ی خاصیت‌های مشترک یک نوع خاص در prototype آن بهره برد. روش دیگری برای تعریف یک سازنده  و prototype آن وجود دارد که از کلیدواژه‌ی `class` استفاده می کند.

می توانید با تعریف گذارنده‌ها (setters) و گیرنده‌ها (getters)، به طور مخفیانه متدهایی را ایجاد کنید که با هر بار دسترسی به یک خاصیت شیء، فراخوانی می شوند. متدهای ایستا (static) متدهایی هستند که در سازنده‌ی کلاس ذخیره می شوند نه در prototype آن.

عملگر `instanceof` را اگر به یک شیء و یک سازنده اعمال کنید، به شما خواهد گفت که آن شیء نمونه‌ای از آن سازنده می باشد یا خیر.

یکی از کارهای مفیدی که می توان با اشیاء انجام داد این است که یک رابط برای آن‌ها مشخص نمود که دیگران فقط بتوانند از طریق آن رابط با شیء ارتباط برقرار کنند. با این کار، دیگر جزئیات مربوط به ساختار شیء شما کپسوله شده و پشت رابط مخفی می مانند.

اشیائی با انواع مختلف می توانند رابط یکسانی را پیاده‌سازی و استفاده کنند (توسط رابط یکسانی به کار گرفته شوند ). کدی که برای استفاده از یک رابط نوشته شده است به صورت خودکار می داند که چگونه با هر تعداد شیء متفاوت که آن رابط را دارند کار کند. این کار _چندریختی_ نامیده می شود.

زمانی که چندین کلاس را پیاده سازی می کنیم که تنها در بعضی جزئیات با هم تفاوت دارند، می توانیم کلاس‌های جدید را به عنوان _زیرکلاس‌های_ کلاس های موجود بنویسیم که بعضی از رفتارهای آن ها را به _ارث_ ببرند.


## تمرین‌ها

{{id exercise_vector}}

### نوع بردار

{{index dimensions, "Vec class", coordinates, "vector (exercise)"}}

کلاسی به نام `Vec` تعریف کنید که نمایانگر یک فضای دوبعدی برداری باشد. این کلاس دو عدد `x` و `y` را به عنوان پارامتر (عددی) دریافت می کند، که با همین نام به عنوان خاصیت ذخیره می شود.

{{index addition, subtraction}}

به پروتوتایپ `Vec` دو متد `plus` و `minus` را اضافه کنید که بردار دیگری را به عنوان یک پارامتر گرفته و بردار جدیدی را برمیگرداند که  تفاوت یا جمع مقدارهای _x_ و _y_ دو بردار (`this` و پارامترها) را برمی گرداند

خاصیت دریافت‌کننده‌ای (getter) به نام `length` به پروتوتایپ اضافه کنید که طول بردار را محاسبه می کند – فاصله‌ی بین نقطه‌ی (_x_,_y_)  از مبدا (0,0).

{{if interactive

```{test: no}
// Your code here.

console.log(new Vec(1, 2).plus(new Vec(2, 3)));
// → Vec{x: 3, y: 5}
console.log(new Vec(1, 2).minus(new Vec(2, 3)));
// → Vec{x: -1, y: -1}
console.log(new Vec(3, 4).length);
// → 5
```
if}}

{{hint

{{index "vector (exercise)"}}

Look back to the `Rabbit` class example if you're unsure how `class`
declarations look.

{{index Pythagoras, "defineProperty function", "square root", "Math.sqrt function"}}

Adding a getter property to the constructor can be done by putting the
word `get` before the method name. To compute the distance from (0, 0)
to (x, y), you can use the Pythagorean theorem, which says that the
square of the distance we are looking for is equal to the square of
the x-coordinate plus the square of the y-coordinate. Thus, [√(x^2^ +
y^2^)]{if html}[[$\sqrt{x^2 + y^2}$]{latex}]{if tex} is the number you
want, and `Math.sqrt` is the way you compute a square root in
JavaScript.

hint}}

### گروه‌ها

{{index "groups (exercise)", "Set class", "Group class", "set (data structure)"}}

{{id groups}}

محیط استاندارد جاوااسکریپت ساختار داده‌ی دیگری به نام `Set` را فراهم می کند. شبیه به نمونه‌ای از`Map`، یک مجموعه (set) مجموعه‌ای از مقدارها را نگه داری می کند. برخلاف `Map`، این ساختار داده مقادیر را با هم مرتبط نمی کند – فقط مشخص می کند که کدام مقادیر در مجموعه وجود دارند.  یک مقدار فقط می تواند یکبار به مجموعه اضافه شود – اگر دوباره یک مقدار خاص را اضافه کنیم، اثری نخواهد داشت.

{{index "add method", "delete method", "has method"}}

کلاسی به نام `Group` (زیرا `Set` قبلا رزرو شده است) تعریف کنید. شبیه `Set،` این کلاس متدهای  `add،` `delete` و `has` را دارد. سازنده این کلاس یک گروه (group)  خالی ایجاد می کند، متد `add،` یک مقدار را به گروه اضافه می کند (البته اگر قبلا عضو گروه نبود)، متد `delete` آرگومان ورودی‌اش را از گروه حذف می کند (البته اگر وجود داشت)، و متد `has،` مقداری بولی را تولید می کند که نماینگر این است که آرگومانش در عضو گروه بوده است یا خیر.

{{index "=== operator", "indexOf method"}}

از عملگر `===` استفاده کنید یا چیزی مشابه مثل `indexOf،` تا بتوانید محاسبه کنید که آیا دو مقدار مشابه هستند یا خیر.

{{index "static method"}}

به کلاس متد استاتیکی به نام `from` اضافه کید که شیءای قابل شمارش را به عنوان آرگومان دریافت می کند و گروهی ایجاد می کند که دارای تمامی مقادیری است که با پیمایش شیء بدست می آید.

{{if interactive

```{test: no}
class Group {
  // Your code here.
}

let group = Group.from([10, 20]);
console.log(group.has(10));
// → true
console.log(group.has(30));
// → false
group.add(10);
group.delete(10);
console.log(group.has(10));
// → false
```

if}}

{{hint

{{index "groups (exercise)", "Group class", "indexOf method", "includes method"}}

The easiest way to do this is to store an array of group members
in an instance property. The `includes` or `indexOf` methods can be
used to check whether a given value is in the array.

{{index "push method"}}

Your class's ((constructor)) can set the member collection to an empty
array. When `add` is called, it must check whether the given value is
in the array or add it, for example with `push`, otherwise.

{{index "filter method"}}

Deleting an element from an array, in `delete`, is less
straightforward, but you can use `filter` to create a new array
without the value. Don't forget to overwrite the property holding the
members with the newly filtered version of the array.

{{index "for/of loop", "iterable interface"}}

The `from` method can use a `for`/`of` loop to get the values out of
the iterable object and call `add` to put them into a newly created
group.

hint}}

### گروه‌های قابل شمارش

{{index "groups (exercise)", [interface, object], "iterator interface", "Group class"}}

{{id group_iterator}}

کلاس `Group` که در مثال قبل ایجاد کردید را قابل شمارش کنید. اگر در مورد آن شکل از رابط سوال دارید، مراجعه کنید به بخشی که پیش تر در این فصل در مورد رابط شمارش گر بحث شد.

اگر از یک آرایه برای نمایش اعضای گروه استفاده کرده اید،شمارش‌گری که با فراخوانی <bdo>`Symbol.iterator`</bdo> روی آرایه ایجاد شده است را به عنوان پاسخ استفاده نکنید، این کار درست است اما هدف این تمرین چیز دیگری است.

اگر شمارش‌گر شما رفتاری غیر معمول در هنگام پیمایش و بروزرسانی گروه از خود نشان می دهد، مشکلی نیست.

{{if interactive

```{test: no}
// Your code here (and the code from the previous exercise)

for (let value of Group.from(["a", "b", "c"])) {
  console.log(value);
}
// → a
// → b
// → c
```

if}}

{{hint

{{index "groups (exercise)", "Group class", "next method"}}

It is probably worthwhile to define a new class `GroupIterator`.
Iterator instances should have a property that tracks the current
position in the group. Every time `next` is called, it checks whether
it is done and, if not, moves past the current value and returns it.

The `Group` class itself gets a method named by `Symbol.iterator`
that, when called, returns a new instance of the iterator class for
that group.

hint}}

### قرض گرفتن یک متد

پیش تر در این فصل گفته شد که متد `hasOwnProperty` یک شیء را می توان به عنوان جایگزینی کاراتر نسبت به عملگر in در شرایط که قصد دارید تا خاصیت‌های پروتوتایپ را درنظر نگیرید کاربرد دارد. اما چه خواهد شد اگر map شما نیز لازم باشد تا واژه‌ی `"hasOwnProperty"`  را داشته باشد؟ در این صورت دیگر نمی توانی این متد را فراخوانی کند، به این علت که خاصیت شیء مقدار این متد را مخفی می کند.

آیا می توانی راهی را پیدا کنید که بتوان `hasOwnProperty` را روی یک شی فراخونی کرد، که این شیء خاصیتی با همین نام را هم داشته باشد؟

{{if interactive

```{test: no}
let map = {one: true, two: true, hasOwnProperty: true};

// Fix this call
console.log(map.hasOwnProperty("one"));
// → true
```

if}}

{{hint

Remember that methods that exist on plain objects come from
`Object.prototype`.

Also remember that you can call a function with a specific `this`
binding by using its `call` method.

hint}}
