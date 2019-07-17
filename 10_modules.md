{{meta {load_files: ["code/packages_chapter_10.js", "code/chapter/07_robot.js"]}}}

# ماژول‌ها

{{quote {author: "Tef", title: "Programming is Terrible", chapter: true}

Write code that is easy to delete, not easy to extend.

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_10.jpg", alt: "Picture of a building built from modular pieces", chapter: framed}}}

{{index organization, [code, "structure of"]}}

یک برنامه‌ی ایده‌آل دارای ساختاری شفاف و روشن است. به راحتی می توان کارکرد آن
را توضیح داد و هر بخش￼ آن نقشی را ایفا می کند که به خوبی تعریف شده است.

{{index "organic growth"}}

معمولا یک برنامه‌ی واقعی به شکلی ارگانیک رشد می کند. قابلیت‌های جدید، همانطور که لازم
می شوند به برنامه افزوده شوند. ساختاردهی – و حفظ ساختار – کاری مجزایی است. کاری است که
فقط در آینده با مزایای آن روبرو می‌شوید زمانی که کسی دوباره روی برنامه قرار است کار کند. پس
ممکن است وسوسه‌انگیز باشد که از آن غفلت کنید و بگذارید بخش‌های برنامه عمیقا دچار
آشفتگی شوند.

{{index readability, reuse, isolation}}

در عمل این غفلت دو اشکال ایجاد می کند. اول اینکه درک یک سیستم بدون‌ ساختار مشکل است.
اگر همه‌ی بخش‌های برنامه در تماس با دیگر بخش‌ها باشند، سخت می توان بخشی از برنامه را
به صورت جداگانه بررسی نمود. شما مجبورید که درکی کلی و جامع از برنامه داشته باشید.
دوم اینکه، اگر بخواهید هر کدام از قابلیت‌های برنامه‌ای اینچنینی را در جایی دیگر
استفاده کنید، از اول نوشتن آن قابلیت ممکن است از جداسازی آن از برنامه، آسان تر
باشد.

اصطلاح “big ball of mud” (توپ‌ بزرگ گلی) اغلب برای برنامه‌های بزرگی که ساختاری ندارند استفاده می
شود. همه چیز به هم چسبیده است و زمانی که قصد دارید یک قسمت را جدا کنید، کل آن
قسمت یا برنامه متلاشی می شود و دستانتان را کثیف کند.


## ماژول‌ها

{{index dependency, [interface, module]}}

استفاده از _ماژول‌ها_ تلاشی برای اجتناب از این گونه مشکلات است. یک ماژول بخشی از
برنامه است که مشخص می کند به کدام بخش‌های دیگر از برنامه وابسته است (وابستگی‌های
آن) و چه قابلیتی برای استفاده‌ی دیگر ماژول ها فراهم می کند (_رابط_ آن).

{{index "big ball of mud"}}

رابط‌های ماژول شباهت‌ زیادی با رابط‌های شیء دارند، همانطور که با آن ها در[فصل ?](object#interface)
آشنا شدیم. رابط‌ها بخشی از ماژول را در دسترس جهان بیرون می گذارند و بقیه‌ی قسمت
ها را به صورت خصوصی حفظ می کنند. با محدودسازی راه‌های تعامل ماژول‌ها با یکدیگر ، سیستم بیشتر
شبیه لگو می شود، جایی که قطعات توسط متصل‌کننده‌هایی که به خوبی تعریف شده اند با هم
تعامل دارند و کمتر به توپ گلی شباهت دارد که همه چیز در آن با هم مخلوط شده است.

{{index dependency}}

ارتباطات بین ماژول ها را _وابستگی‌ها_ می نامند. زمانی که یک ماژول به بخشی از یک
ماژول دیگر نیاز دارد، گفته می شود که به آن ماژول وابستگی دارد. زمانی که این
وابستگی در خود ماژول به صورت مشخص اعلام شود، می توان از آن برای شناسایی دیگر
ماژول‌هایی که لازم است برای اجرای یک ماژول خاص حضور داشته باشند استفاده کرد و به
صورت خودکار آن وابستگی‌ها را بارگذاری کرد.

برا جداسازی ماژول‌ها به این روش ، لازم است هر کدام محدوده‌ی خصوصی خودش را داشته
باشد.

فقط قرار دادن کدهای جاوااسکریپت در فایل های جداگانه این امکان را فراهم نمی کند.
فایل‌ها همچنان فضای نام سراسری یکسانی را به صورت مشترک استفاده می کنند. ممکن است به صورت
تصادفی یا آگاهانه بین متغیرهای یکدیگر تداخل ایجاد کنند و ساختار وابستگی همچنان
غیر شفاف خواهد ماند. می توان کار بهتری کرد که در ادامه خواهیم دید.

{{index design}}

طراحی یک ساختار ماژول مناسب برای یک برنامه ممکن است سخت باشد. در مرحله‌ای که هنوز
در حال بررسی مشکل هستید، و چیزهای متفاوتی را آزمایش می کنید، ممکن است علاقه‌ای
نداشته باشید که زیاد به ساختاردهی فکر کنید، چرا که می تواند باعث حواسپرتی زیادی
بشود. وقتی به نتیجه‌ای استوار و قابل اتکا رسیدید ، آن زمان مناسب برای اقدام جهت
سازماندهی برنامه خواهد بود.

## بسته‌ها

{{index bug, dependency, structure, reuse}}

یکی از مزایای ساختن یک برنامه بر اساس قسمت‌های جداگانه، و داشتن قابلیت اجرای آن
قسمت‌ها به صورت مستقل، این است که ممکن است بتوانید آن قسمت‌ها را در برنامه‌های
مختلف به کار ببرید.

{{index "parseINI function"}}

اما چگونه آن را راه‌اندازی می کنید؟ فرض کنیم من می خواهم که تابع `parseINI` را که
در [فصل ?](regexp#ini) نوشتیم در برنامه‌ی دیگری استفاده کنم. اگر روشن باشد که این تابع چه
وابستگی‌هایی دارد (که در اینجا ندارد)، می توانم به سادگی کدهای مورد نیاز را به
پروژه‌ی جدیدم کپی کنم و از آن استفاده کنم. اما در این صورت اگر مشکلی در آن کد
پیدا کنم، احتمالا آن مشکل را فقط در برنامه‌ای که در حال کار روی آن هستم رفع
خواهم کرد و فراموش می کنم که در برنامه‌ی دیگر نیز آن را اصلاح کنم.

{{index duplication, "copy-paste programming"}}

به محض اینکه به کپی کردن کدها اقدام کنید متوجه خواهید شد که با جابجا کردن کپی‌ها
بین برنامه ها و به روز رسانی آن ها وقت و انرژی خودتان را تلف می کنید.

اینجا‌ است که بسته‌ها _((package))s_ کاربرد خواهند داشت. یک بسته یک قطعه کد است که
می‌تواند توزیع شود (کپی و نصب شود). یک بسته می تواند دارای یک یا چند ماژول باشد
و همچنین اطلاعاتی در باره‌ی دیگر بسته‌هایی که به آنها وابسته است دارد. یک بسته همچنین
همراه با مستنداتی می آید که کارکرد آن را شرح می دهد، بنابراین کسانی که بسته را
ننوشته اند قادر خواهند بود که از آن استفاده کنند.

زمانی که مشکلی در یک بسته شناسایی شود یا ویژگی جدیدی به آن افزوده شود بسته به
روز رسانی می گردد. اکنون برنامه‌هایی که به آن وابستگی داشته اند (که خود ممکن
است بسته باشند ) می توانند به نسخه جدیدتر به‌روز شوند.

{{id modules_npm}}

{{index installation, upgrading, "package manager", download, reuse}}

کارکردن به این روش نیاز به زیرساخت دارد. جایی را نیاز داریم که بسته‌ها را ذخیره و
جستجو کنیم و راهی￼ سرراست برای نصب و به روز رسانی آن ها لازم است. در دنیای
جاوااسکریپت این زیرساخت توسط NPM در ([_https://npmjs.org_](https://npmjs.org)) فراهم شده است.

NPM دارای دو بخش است: یک سرویس آنلاین که افراد می توانند به بارگیری و
بارگذاری بسته‌ها اقدام کنند و یک برنامه (که به همراه Node.js می آید) که به شما
کمک می کند که آن ها را مدیریت کنید.

{{index "ini package"}}

در زمان نوشتن این کتاب، بیش از نیم میلیون بسته در NPM وجود دارد. باید بگم که بخش
زیادی از این بسته‌ها بدرد نخور هستند اما تقریبا هر بسته‌ی مفیدی که عموما در دسترس
باشد را می توان آنجا پیدا کرد. به عنوان مثال، یک تجزیه‌گر فایل ini، شبیه به چیزی
که خودمان در  [فصل ?](regexp) ساختیم، در بسته‌ای به نام `ini` وجود دارد.


{{index "command line"}}

[فصل ?](node) شما را با نحوه‌ی نصب بسته‌ها به صورت محلی و با استفاده از برنامه‌ی خط فرمان
`npm` آشنا خواهد کرد.

در دسترس داشتن بسته‌های باکیفیت قابل دانلود بسیار ارزشمند است. این بدین معناست
که می توان از اختراع دوباره یک برنامه در بیشتر مواقع جلوگیری کنیم، برنامه ای که
صدها نفر قبل تر نوشته اند و می توان آن را با چند حرکت به صورت استوار و تست شده
پیاده سازی کرد.

{{index maintenance}}

کپی کردن یک نرم افزار آسان است، بنابراین اگر کسی آن را نوشته باشد، انتشار آن
بین دیگر افراد روند کارآمدی محسوب می شود. اما به عنوان فرد اول نوشتن آن کار می
برد و پاسخ دادن به افرادی که مشکلاتی را در کد پیدا می‌کنند یا درخواست ویژگی جدیدی
را دارند، کار بیشتری می طلبد.

به صورت پیش فرض، شما مالک کپی‌رایت کدی هستید که می نویسید و دیگر افراد فقط با
اجازه‌ی شما می توانند از آن استفاده کنند. اما به دلیل اینکه بعضی افراد باحال
(nice) هستند و اینکه انتشار یک نرم افزار خوب باعث کمی شهرت بین برنامه نویسان می
شود، خیلی از بسته‌ها تحت مجوزی منتشر می شوند که به صراحت اجازه استفاده دیگران از
برنامه را می دهد.

بیشتر کدهایی که در NPM وجود دارند دارای چنین مجوزی هستند. بعضی از مجوزها از شما
می خواهند که کدی که با استفاده از بسته نوشته‌اید را با همان مجوز منتشر کنید. بعضی
دیگر کمتر درخواستی دارند و فقط لازم است که مجوزی که همراه بسته وجود دارد را در
هنگام توزیع کنار آن نگه دارید. بیشتر جامعه‌ی برنامه نویسی جاوااسکریپت از این نوع
مجوز استفاده می کنند. زمانی که از بسته‌های دیگر افراد استفاده می کنید، مطمئن شوید
که از مجوز آن آگاهی دارید.

## فراهم ساختن ماژول‌ها

قبل از 2015 در جاوااسکریپت سیستم ماژول داخلی وجود نداشت. با این وجود برنامه نویسان برای
بیشتر از یک دهه، سیستم های بزرگی را برنامه نویسی می کردند درحالیکه _نیاز_ به
ماژول‌ها وجود داشت.

{{index [function, scope], [interface, module], [object, as module]}}

بنابراین آن ها سیستم ماژول خودشان را با استفاده از خود زبان طراحی کردند. می
توان از توابع جاوااسکریپت برای ایجاد حوزه‌های محلی و از اشیاء به
عنوان رابط‌های ماژول استفاده کرد.

{{index "Date class", "weekDay module"}}

مثال زیر یک ماژول برای انتخاب بین نام روزها و عددشان است ( که از متد `getDay` مربوط به
`Date` استفاده می کند). رابط آن از <bdo>`weekDay.name`</bdo> و <bdo>`weekDay.number`</bdo> تشکیل شده است
و متغیر محلی `names` را در حوزه‌ی یک تابع که بلادرنگ فراخوانی می شود پنهان می کند.

```
const weekDay = function() {
  const names = ["Sunday", "Monday", "Tuesday", "Wednesday",
                 "Thursday", "Friday", "Saturday"];
  return {
    name(number) { return names[number]; },
    number(name) { return names.indexOf(name); }
  };
}();

console.log(weekDay.name(weekDay.number("Sunday")));
// → Sunday
```

{{index dependency, [interface, module]}}

این سبک از ماژول‌ها، تا حدی ایزوله کردن را فراهم می کند، اما وابستگی را
پشتیبانی نمی کند. به جای آن، رابطش را در حوزه‌ی سراسری قرار می دهد، و انتظار
دارد که در صورت وجود، وابستگی‌هایش تعریف شده باشند تا بتواند کاری مشابه سیستم
وابستگی‌ها انجام دهد. برای مدتی طولانی این روش در برنامه نویسی وب استفاده می شد
اما الان تقریبا از رده خارج شده است.

اگر قصد دارید ارتباطات مربوط به وابستگی را به عنوان بخشی از کد داشته باشید،
بایستی مدیریت بارگیری وابستگی‌ها را به عهده بگیرید. برای این کار لازم است بتوانیم رشته‌ها را به عنوان کد اجرا کنیم. این کار در جاوااسکریپت قابل اجرا است.

{{id eval}}

## ارزیابی داده‌ها به عنوان کد‌های اجرایی

{{index evaluation, interpretation}}

راه‌های متعددی برای گرفتن داده‌ها (یک رشته از کد) و اجرای آن به عنوان بخشی از
برنامه کنونی وجود دارد.

{{index isolation, eval}}

روشن ترین راه، استفاده از عملگر `eval` است که رشته‌ای را در قلمروی (scope)
_فعلی_ اجرا می کند. این ایده معمولا ایده‌ی بدی محسوب می شود چرا که باعث از بین
رفتن بعضی از خاصیت‌هایی می شود که در قلمرو‌ها به صورت نرمال وجود دارند، مثل حدس
زدن آسان متغیری که یک نام مشخص به آن ارجاع می دهد.

```
const x = 1;
function evalAndReturnX(code) {
  eval(code);
  return x;
}

console.log(evalAndReturnX("var x = 2"));
// → 2
console.log(x);
// → 1
```

{{index "Function constructor"}}

یک راه کم‌خطرتر تفسیر داده به عنوان کد، استفاده از سازنده `Function` است. این
تابع دو آرگومان دریافت می کند: یک رشته که حاوی یک لیست جدا شده با ویرگول از نام
آرگومان‌ها است و یک رشته که حاوی بدنه تابع است. این تابع کد را درون یک مقدار
تابع می‌پوشاند بنابراین قلمروی مختص به خودش خواهد داشت و تداخلی با دیگر قلمروها
نخواهد داشت.

```
let plusOne = Function("n", "return n + 1;");
console.log(plusOne(4));
// → 5
```

این دقیقا همان‌ چیزی است که برای یک سیستم ماژول نیاز داریم. می توانیم کدهای ماژول را
درون یک تابع قرار دهیم و از قلمروی مربوط به تابع به عنوان قلمروی ماژول استفاده
نماییم.

## CommonJS

{{id commonjs}}

{{index "CommonJS modules"}}

رایج ترین شیوه‌ای که برای اضافه کردن ماژول‌ها به جاوااسکریپت استفاده می شود،
ماژول‌های _CommonJS_ می‌باشد. <bdo>Node.js</bdo> از آن استفاده می کند و سیستمی است که
بیشتر بسته‌های NPM از آن استفاده می کنند.

{{index "require function", [interface, module]}}

مفهوم اصلی در ماژول‌های CommonJS تابعی به نام `‌require` است. زمانی که از این تابع
به همراه نام ماژول یک وابستگی استفاده می کنید، این تابع اطمینان حاصل می کند
که ماژول مورد نظر بارگیری شده است و رابط آن را برمی گرداند.

{{index "exports object"}}

به دلیل اینکه بارگیرنده‌ (loader)، ماژول مورد نظر را درون یک تابع قرار می دهد، ماژول‌ها به
صورت خودکار قلمروی‌ محلی خودشان را می گیرند. تنها کاری که بایستی بکنند این است که
تابع `require` را فراخوانی کنند تا به وابستگی های آن ها دسترسی داشته باشند و
رابطشان را در شیئی که به `exports` تخصیص یافته قرار دهند.

{{index "formatDate module", "Date class", "ordinal package", "date-names package"}}

این ماژول به عنوان مثال، یک تابع ویرایش تاریخ فراهم می سازد (date-formatting).
اینجا از دو بسته از NMP استفاده می شود – بسته‌ی `ordinal` برای تبدیل اعداد به
رشته‌های شبیه `"1st"` و `"2nd"` , و بسته‌ی <bdo>`date-names`</bdo> برای گرفتن نام‌های انگلیسی روزهای
هفته و ماه‌ها. این ماژول یک تابع به نام `fomatDate` را صادر (export) می کند که یک شیء `Date` و
یک رشته به عنوان قالب دریافت‌ می کند.

رشته‌ی قالب می تواند حاوی کدهایی باشد که فرمت
تاریخ را مشخص می کند مثل `YYYY` برای نمایش سال به صورت کامل و `‌Do` برای نام‌های
ترتیبی روزهای ماه. می توانید به آن رشته‌ای به شکل <bdo>`"MMMM Do YYYY"`</bdo> برای دریافت
چیزی شبیه <bdo>"November 22nd 2017"</bdo> ارسال کنید.

```
const ordinal = require("ordinal");
const {days, months} = require("date-names");

exports.formatDate = function(date, format) {
  return format.replace(/YYYY|M(MMM)?|Do?|dddd/g, tag => {
    if (tag == "YYYY") return date.getFullYear();
    if (tag == "M") return date.getMonth();
    if (tag == "MMMM") return months[date.getMonth()];
    if (tag == "D") return date.getDate();
    if (tag == "Do") return ordinal(date.getDate());
    if (tag == "dddd") return days[date.getDay()];
  });
};
```

{{index "destructuring binding"}}

رابط `ordinal` یک تابع ساده است در حالیکه <bdo>`date-names`</bdo> یک شیء را که حاوی چند چیز
متفاوت است صادر می کند- دو مقداری که در نام های آرایه‌ها استفاده کردیم. استفاده
از روش تخریب (Destructuring) برای ایجاد متغیر برای رابط‌های وارد شده بسیار مناسب
است.

ماژول تابع رابط خودش را به `exports` اضافه می کند در نتیجه ماژول‌های وابسته، به آن
دسترسی خواهند داشت. می توانیم از این ماژول به این صورت استفاده کنیم:

```
const {formatDate} = require("./format-date");

console.log(formatDate(new Date(2017, 9, 13),
                       "dddd the Do"));
// → Friday the 13th
```

{{index "require function", "CommonJS modules", "readFile function"}}

{{id require}}

می توان `require` را در کوتاه ترین شکل خودش  تعریف کرد:

```{test: wrap, sandbox: require}
require.cache = Object.create(null);

function require(name) {
  if (!(name in require.cache)) {
    let code = readFile(name);
    let module = {exports: {}};
    require.cache[name] = module;
    let wrapper = Function("require, exports, module", code);
    wrapper(require, module.exports, module);
  }
  return require.cache[name].exports;
}
```

{{index [file, access]}}

در این کد، `readFile` یک تابع ساختگی است که یک فایل را خوانده و محتوای آن را به
صورت رشته برمی‌گرداند. جاوااسکریپت استاندارد، این قابلیت را فراهم نمی کند – اما
محیط‌های متفاوت جاوااسکریپت مثل مرورگر و <bdo>Node.js</bdo>، راه‌های خودشان را برای دسترسی به
فایل‌ها فراهم می سازند. مثال بالا فقط وانمود می کند که `readFile` وجود دارد.

{{index cache, "Function constructor"}}

برای جلوگیری از بارگیری چندباره‌ی یک ماژول ، `require` ماژول‌هایی که تاکنون
بارگیری شده اند را جایی (در حافظه‌ی نهان) نگه داری می کند. در زمان فراخوانی،
ابتدا بارگیری ماژول خواسته شده را در گذشته بررسی می کند و در صورت نبود،
آن را بارگیری می کند. این روند شامل خواندن کد ماژول، قرار دادن آن درون یک
تابع و فراخوانی آن می شود.

{{index "ordinal package", "exports object", "module object", [interface, module]}}

رابط بسته‌ی `ordinal` که قبل تر دیدیم یک شیء نیست بلکه یک تابع است. یک ایراد وارده
به ماژول‌های CommonJS این است که اگرچه سیستم ماژول، یک رابط به صورت شیئی خالی برای شما ایجاد
می کند (که در `exports` قرار می گیرد)، می توانید آن را با هر مقداری که بخواهید
به وسیله‌ی بازنویسی خاصیت <bdo>`module.exports`</bdo> جایگزین کنید. این کار را
خیلی از ماژول ها انجام می دهند تا بتوانند یک مقدار ساده را به جای یک شیء رابط
صادر کنند.


با تعریف `require`، `exports`، و `module` به عنوان پارامترهای تابع پوشش دهنده‌ی تولید شده (و ارسال مقدارهای مناسب در هنگام فراخوانی)، بارگیرنده‌ اطمینان حاصل می
کند که این متغیرها در قلمروی مربوط به ماژول در دسترس خواهند بود.

{{index resolution, "relative path"}}

روشی که در آن، رشته‌ی داده شده به `require` به یک نام فایل واقعی یا یک آدرس وب
تفسیر می شود، در سیستم های مختلف متفاوت است. زمانی که این رشته با
<bdo>`"./"`</bdo> یا <bdo>`"../"`</bdo> شروع می شود عموما نسبت به نام فایل ماژول
فعلی در نظر گرفته می شود. بنابراین <bdo>`"./format-date"`</bdo> فایلی به نام
<bdo>`format-date.js`</bdo> که در همان پوشه قرار دارد در نظر گرفته می شود.


زمانی که نام نسبی نیست، <bdo>Node.js</bdo> به جستجوی بسته ای با همان نام اقدام می کند. در
کد مثال این فصل، ما این گونه نام‌ها را به عنوان بسته‌های NPM تفسیر می کنیم. در [فصل ?](node) به جزئیات نصب و استفاده از ماژول‌های NPM خواهیم پرداخت.

{{id modules_ini}}

{{index "ini package"}}
اکنون به جای نوشتن تجزیه‌گر فایل INI خودمان، می توانیم از یکی از بسته‌های موجود در
NPM استفاده کنیم.

```
const {parse} = require("ini");

console.log(parse("x = 10\ny = 20"));
// → {x: "10", y: "20"}
```

## ماژول‌های ECMASCRIPT

ماژول‌های CommonJS به خوبی کار می کنند و ترکیب آن ها با NPM به جامعه‌ی
برنامه‌نویسان جاوااسکریپت اجازه داده است که کدها را در مقیاس بزرگ به اشتراک
بگذارند.

{{index "exports object", linter}}

اما کمی لازم است تا دستی به سر و روی آن‌ها کشید. ظاهر نوشتاری آن اندکی مشکل دارد
– به عنوان مثال چیزهایی که به `exports` اضافه می کند در قلمروی محلی در دسترس
نیستند. و به دلیل اینکه `require` یک فراخوانی به یک تابع معمولی است که هر نوع
آرگومانی را قبول می کند، نه فقط مقادیر رشته‌ای، تشخیص وابستگی‌های یک ماژول بدون
اجرای کدهای آن می توان سخت شود.

{{index "import keyword", dependency, "ES modules"}}

{{id es}}

به همین علت است که جاوااسکریپت استاندارد از 2015 سیستم ماژول متفاوت خودش را
معرفی کرد که معمولا _((ES modules))_ نامیده می شود. _ES_ مخفف _ECMAScript_
است. مفاهیم اصلی وابستگی‌ها و رابط‌ها به همان صورت باقی می ماند اما جزئیات آن‌ها متفاوت
است. برای یک مورد ، نشان‌گذاری اکنون درون زبان یکپارچه شده است. به جای فراخوانی
یک تابع برای دسترسی به یک وابستگی، از کلید‌واژه‌ی مخصوصی به نام `import` استفاده می
کنید.

```
import ordinal from "ordinal";
import {days, months} from "date-names";

export function formatDate(date, format) { /* ... */ }
```

{{index "export keyword", "formatDate module"}}

به طور مشابه، کلیدواژه‌ی `export` برای صدور استفاده می شود. می توان از آن در
ابتدای یک تابع، کلاس، یا تعریف متغیر استفاده کرد <bdo>(`let`,
`const`, یا `var`)</bdo>

{{index [binding, exported]}}

یک رابط ماژول ES، یک مقدار واحد نیست بلکه مجموعه‌ای از متغیرهای نامگذاری شده است.
ماژولی که در بالا آمده است `formatDate` را به یک تابع تخصیص می دهد. زمانی که از
یک ماژول دیگر اقدام به _وارد کردن_ می کنید شما متغیر را وارد می کنید نه مقدار آن
را که معنای آن این است که یک صدور ماژول ممکن است مقدار یک متغیر را در هر زمانی
تغییر دهد و ماژول‌هایی که آن را وارد کرده اند، مقدار جدیدش را خواهند دید.

{{index "default export"}}

زمانی که متغیری به نام `default` وجود داشته باشد، آن به عنوان مقدار صادر شده‌ی اصلی
ماژول در نظر گرفته می شود. اگر ماژولی مانند `ordinal` را که در مثال آمد، بدون
استفاده از براکت‌ها دور نام متغیر وارد کنید، متغیر￼ `default` آن ماژول را
دریافت می کنید. این گونه ماژول‌ها می توانند در کنار صدور `default`، متغیرهای دیگری را هم با نام‌های مختلف صادر
کنند.

برای ایجاد یک صدور پیش فرض (default) می توانید از <bdo>`export default`</bdo> قبل از یک
عبارت، تعریف تابع یا کلاس استفاده کنید.

```
export default ["Winter", "Spring", "Summer", "Autumn"];
```
می توان نام متغیرهایی که وارد شده اند را با استفاده از `as` تغییر داد.

```
import {days as dayNames} from "date-names";

console.log(dayNames.length);
// → 7
```
یک تفاوت مهم دیگر این است که عمل وارد کردن ماژول ES قبل از این که اجرای اسکریپت
ماژول شروع شود اتفاق می‌افتد. یعنی تعریف `import` درون تابع یا بلوک‌ها ظاهر نمی
شود و نام وابستگی‌ها باید به صورت رشته محصور در نقل قول باشد نه عبارت‌های
دلخواه.

در زمان نوشتن این کتاب، جامعه‌ی برنامه‌نویسان جاوااسکریپت در مسیر استفاده از این سبک
ماژول‌ها هستند. اما این روند به آهستگی اتفاق می افتد. چند سالی طول کشید بعد از
مشخص شدن فرمت جدید، که مرورگرها و Node.js شروع به پشتیبانی از آن کردند. و اگرچه
آن‌ها تقریبا از آن پشتیبانی کامل می کنند اما این پشتیبانی هنوز با ایراداتی
روبرو است و بحث‌هایی پیرامون شیوه‌ی توزیع این گونه ماژول ها در NPM هنوز در جریان
است.

خیلی از برنامه‌ها را با ماژول‌های ES می نویسند و به صورت خودکار به دیگر فرمت‌ها در
هنگام انتشار تبدیل می کنند. ما در دوره‌ی گذار به سر می بریم که در آن دو سیستم
مدیریت ماژول همزمان استفاده می شوند و خوب است که قادر باشیم کدها را در هر دو
سیستم بنویسیم و بخوانیم.


## ساخت و بسته‌بندی

{{index compilation, "type checking"}}

در واقع خیلی از پروژه‌های جاوااسکریپت حتی ، از لحاظ فنی، در زبان جاوااسکریپت
نوشته نمی شوند. افزونه‌هایی وجود دارد که به طور گسترده استفاده می شوند مانند
همان گویشی از جاوااسکریپت که به انواع داده حساس بود و در [فصل ?](error#typing) ذکر شد. برنامه
نویسان اغلب شروع به استفاده از افزونه‌هایی می کنند که خیلی پیش‌تر برای اضافه شدن به پلتفرم‌ها برنامه ریزی شده اند.

برای اجرایی کردن این کار، کدهایشان را کامپایل می کنند، کد را از گویش
جاوااسکریپت مورد نظرشان به جاوااسکریپت ساده ترجمه می کنند – یا حتی به یک نسخه‌ی
قدیمی از جاوااسکریپت ترجمه می کنند که مرورگرهای قدیمی بتوانند آن را اجرا کنند.

{{index latency, performance, [file, access], [network, speed]}}

قرار دادن یک برنامه‌ی ماژولار (پیمانهای) که از 200 فایل متفاوت تشکیل
شده است در یک صفحه‌ی وب مشکلات خودش را خواهد داشت. اگر دریافت و استفاده از یک
فایل در شبکه 50 هزارم ثانیه زمان بگیرد، کل برنامه ده ثانیه زمان خواهد گرفت یا
شاید نیمی از آن زمان اگر بتوانیم چندین فایل را همزمان بارگیری کنید. این زمان
زیادی را تلف خواهد کرد. به دلیل اینکه بارگیری یک فایل بزرگ واحد، از تعداد زیادی
فایل کوچک، عموما سریع تر اتفاق می افتد. برنامه نویسان وب به سراغ ابزارهایی رفته
اند که برنامه‌هایشان را (که با زحمت به ماژول ها
تقسیم کرده اند) گرفته و تبدیل به یک فایل بزرگ کند قبل از اینکه بخواهند برنامه
را در وب منتشر کنند. این ابزار￼ را بسته‌ساز می نامند _((bundler))s_.

{{index "file size"}}

و می توانیم پارا فراتر بگذاریم. جدا از تعداد فایل‌ها، حجم این فایل‌ها هم در سرعت
انتقالشان در شبکه تعیین کننده است. بنابراین، جامعه‌ی جاوااسکریپت کاران، ابزارهای
فشرده ساز را اختراع کردند (minifier). این ابزار یک برنامه‌ی جاوااسکریپت را گرفته
و با حذف توضیحات، فضاهای خالی، تغییر نام متغیرها و جایگزینی بعضی کدها با
معادل‌های کوچک تر، حجم آن را کاهش می دهند.

{{index pipeline, tool}}

پس اصلا دور از انتظار نیست که که کدی که در یک بسته‌ی NPM یافته اید یا در یک صفحه‌ی
وب اجرا می شود پیش از آن مراحل مختلفی از تبدیل را طی کرده باشد – تبدیل از
جاوااسکریپت مدرن به جاوااسکریپت قدیمی‌تر، از ماژول‌های ES به CommonJS، بسته‌بندی
شده و فشرده شده باشد. در این کتاب به جزئیات مربوط به این ابزار نمی پردازیم، چون هم
کسل کننده خواهد بود هم به سرعت تغییر می کنند. فقط حواستان باشد که کدی که شما
معمولا اجرا می کنید اغلب همانی نیست که نوشته شده است.

## طراحی ماژول

{{index [module, design], [interface, module], [code, "structure of"]}}

ساختاردهی به یک برنامه یکی از جنبه‌های حساس و ظریف از برنامه نویسی محسوب می شود.
هر قطعه‌ی معناداری از قابلیت‌ها را می توان به اشکال مختلفی مدلسازی کرد.

طراحی یک برنامه‌ی خوب، سلیقه‌ای  است – مصالحه‌هایی پیش می‌آید که سلیقه در آن دخیل
است. بهترین راه برای یادگیری ارزش یک طراحی دارای ساختار خوب، این است که برنامه‌های
زیادی را مورد مطالعه قرار دهید یا با آن ها کار کنید تا متوجه شوید که چه چیزی
مفید و چه چیز نادرست است. تصور نکنید که درهم‌ریختگی و آشفتگی به هر حال پیش خواهد
آمد. تقریبا هر چیزی را با صرف کمی تفکر بیشتر می توان با سازماندهی بهتر کرد.

{{index [interface, module]}}

یکی از جنبه‌های طراحی ماژول که باید رعایت شود، آسانی استفاده است. اگر چیزی را می سازید که قرار است
توسط کاربران متعددی استفاده شود (یا حتی توسط خودتان، طی مدت سه ماه، زمانی که
دیگر جزئیات کاری که انجام داده اید را به خاطر نمی آورید) اگر رابط برنامه‌ی شما
ساده و قابل تشخیص باشد کمک زیادی می کند.


{{index "ini package", JSON}}

ممکن است معنای آن این باشد که از قرارداد‌های موجود پیروی کنید. یک مثال خوب می تواند
بسته‌ی `ini` باشد. این ماژول کار بسته‌ی استاندارد `JSON` را با فراهم نمودن `parse` و
`stringify` (برای نوشتن یک فایل INI) تقلید می کند و شبیه `JSON` عمل تبدیل بین رشته‌ها
و اشیاء ساده را انجام می دهد. بنابراین رابط در اینجا کوچک و آشنا است و بعد از
اینکه برای اولین بار از آن استفاده کنید احتمالا روش استفاده از آن را به خاطر
خواهید داشت.

{{index "side effect", "hard disk", composability}}

حتی اگر هیچ تابع استاندارد یا بسته‌ای که به طور گسترده استفاده می شود برای تقلید
وجود نداشت، می توانید با￼ استفاده از ساختارهای داده‌ی ساده و تمرکز بر انجام یک
کار، ماژول های خودتان را قابل پیشبینی کنید. به عنوان مثال، خیلی از بسته‌های تجزیه‌ی
فایل INI در NPM تابعی دارند که مستقیما یک فایل ini را از دیسک سخت خوانده و تجزیه
می کند. این کار باعث می شود که نتوان از این ماژول‌ها در مرورگر استفاده کرد، به
دلیل اینکه در مرورگر به طور مستقیم به سیستم فایل دسترسی نداریم و پیچیدگی ای
اضافه می کند که بهتر بود با ترکیب ماژول با یک تابع خواندن فایل دیگر، حل می شد.

{{index "pure function"}}

این موضوع به یکی دیگر از جنبه‌های مفیدی که در طراحی ماژول وجود دارد اشاره می کند – آسانی ترکیب با
کدهای دیگر. ماژول‌هایی که تمرکز روی کار خاصی دارند و مقدارها را محاسبه می کنند در
طیف گسترده‌تری از برنامه ها کاربرد دارند نسبت به ماژول‌های بزرگتری که کارهای
پیچیده‌ای انجام می دهند و دارای اثرات جانبی هستند. خواننده‌ی فایل INIای که اصرار
به خواندن فایل از دیسک سخت را دارد در مواردی که محتوای فایل از دیگر منابع می
آید بدون کاربرد خواهد بود.

{{index "object-oriented programming"}}

همچنین، اشیاء دارای وضعیت (stateful) گاهی اوقات مفید هستند یا حتی لازم
اند اما اگر چیزی را بتوان با یک تابع انجام داد، از تابع استفاده کنید. تعدادی از
بسته‌های خواننده‌ی فایل INI در NPM، سبکی را برای رابط خود استفاده کرده اند که ابتدا
از شما می خواهند که یک شیء ایجاد کنید، بعد فایل مورد نظر را درون شیء بارگیری
کنید و درنهایت از متدهای خاصی برای گرفتن نتایج استفاده کنید. این کار در سنت شیء
گرایی رایج است و باید گفت بسیار بد است. به جای ساختن یک فراخوانی تابع و ادامه
حرکت، باید تشریفات حرکت شیء تان بین وضعیت‌های مختلف را انجام دهید. و به دلیل
اینکه داده‌ها اکنون در یک نوع خاصی از شیء قرار گرفته اند، تمام کدی که با آن در
ارتباط است باید آن نوع را بداند که باعث ایجاد وابستگی‌های درونی بی‌فایده می شود.

اغلب نمی‌توان توان از تعریف ساختارهای داده‌ی جدید صرف نظر کرد – فقط تعداد کمی از موارد
اساسی و پایه‌ای توسط زبان استاندارد فراهم شده است و خیلی از انواع داده می بایست
پیچیده تر از یک آرایه یا یک نگاشت (map) باشد. اما زمانی که یک آرایه کافی است،
از همان استفاده کنید.

یک نمونه از ساختار داده‌های کمی پیچیده تر، گراف است که در [فصل ?](robot) ذکر شد. یک
راه یکسان و مشخص برای نمایش یک گراف در جاوااسکریپت وجود ندارد. در آن فصل، ما
از یک شیء که خاصیت‌هایش آرایه‌هایی از رشته‌ها را نگه داری می کردند استفاده کردیم –
گره‌هایی که از یک گره قابل دستیابی بودند.


بسته‌های متعددی در باره‌ی مسیریابی در NPM وجود دارند اما هیچ کدامشان از این فرمت
گراف استفاده نمی کنند. معمولا این بسته ها به لبه‌های گراف امکان داشتن وزن را می
دهند، که همان فاصله یا هزینه‌ای است که به آن‌ها اختصاص داده شده است. در نمایش ما این‌کار ممکن نیست.

{{index "Dijkstra, Edsger", pathfinding, "Dijkstra's algorithm", "dijkstrajs package"}}

به عنوان مثال، بسته‌ای به نام `dijkstrajs` وجود دارد. یک راه حل شناخته شده برای
مسیریابی، که نسبتا شبیه به تابع `findRoute` ما است، _الگوریتم دیکسترا_ است که ادسخر
دیکسترا آن را نوشت. پسوند `js` معمولا به انتهای￼ بسته‌ها اضافه می شود تا نشان دهد
که آن ها به زبان جاوااسکریپت نوشته شده اند. بسته‌ی `dijkstrajs` از یک فرمت گراف
شبیه به فرمت ما استفاده می کند اما به جای آرایه‌ها از اشیاء استفاده می کند که
مقدارهای خاصیت‌هایش از جنس اعداد هستند – وزن وزن‌ لبه‌ها.

بنابراین اگر بخواهیم از آن بسته استفاده کنیم، بایستی اطمینان حاصل کنیم که گراف
ما در فرمتی که بسته پشتیبانی می کند ذخیره شده باشد. همه‌ی لبه‌ها وزن یکسانی می گیرند زیرا مدل ساده‌شده‌ی
ما برای هر مسیر هزینه‌ی یکسانی در نظر می گیرد (یک دور).

```
const {find_path} = require("dijkstrajs");

let graph = {};
for (let node of Object.keys(roadGraph)) {
  let edges = graph[node] = {};
  for (let dest of roadGraph[node]) {
    edges[dest] = 1;
  }
}

console.log(find_path(graph, "Post Office", "Cabin"));
// → ["Post Office", "Alice's House", "Cabin"]
```
این خود مانعی برای ترکیب پذیری محسوب می شود – زمانی که بسته‌های متنوع از
ساختارهای متفاوتی برای توصیف چیزهای یکسان استفاده می کنند، ترکیب آن ها سخت
خواهد شد. بنابراین، اگر قصد دارید ترکیب پذیری را هم در طراحی لحاظ کنید، توجه
کنید که چه ساختارهای داده‌ای دیگر برنامه نویسان استفاده می کنند و هر وقت ممکن شد
از روش آن ها استفاده کنید.

## Summary

Modules provide structure to bigger programs by separating the code
into pieces with clear interfaces and dependencies. The interface is
the part of the module that's visible from other modules, and the
dependencies are the other modules that it makes use of.

Because JavaScript historically did not provide a module system, the
CommonJS system was built on top of it. Then at some point it _did_ get
a built-in system, which now coexists uneasily with the CommonJS
system.

A package is a chunk of code that can be distributed on its own. NPM
is a repository of JavaScript packages. You can download all kinds of
useful (and useless) packages from it.

## Exercises

### A modular robot

{{index "modular robot (exercise)", module, robot, NPM}}

{{id modular_robot}}

These are the bindings that the project from [Chapter ?](robot)
creates:

```{lang: "text/plain"}
roads
buildGraph
roadGraph
VillageState
runRobot
randomPick
randomRobot
mailRoute
routeRobot
findRoute
goalOrientedRobot
```

If you were to write that project as a modular program, what modules
would you create? Which module would depend on which other module, and
what would their interfaces look like?

Which pieces are likely to be available prewritten on NPM? Would you
prefer to use an NPM package or write them yourself?

{{hint

{{index "modular robot (exercise)"}}

Here's what I would have done (but again, there is no single _right_
way to design a given module):

{{index "dijkstrajs package"}}

The code used to build the road graph lives in the `graph` module.
Because I'd rather use `dijkstrajs` from NPM than our own pathfinding
code, we'll make this build the kind of graph data that `dijkstajs`
expects. This module exports a single function, `buildGraph`. I'd have
`buildGraph` accept an array of two-element arrays, rather than
strings containing hyphens, to make the module less dependent on the
input format.

The `roads` module contains the raw road data (the `roads` array) and
the `roadGraph` binding. This module depends on `./graph` and exports
the road graph.

{{index "random-item package"}}

The `VillageState` class lives in the `state` module. It depends on
the `./roads` module because it needs to be able to verify that a
given road exists. It also needs `randomPick`. Since that is a
three-line function, we could just put it into the `state` module as
an internal helper function. But `randomRobot` needs it too. So we'd
have to either duplicate it or put it into its own module. Since this
function happens to exist on NPM in the `random-item` package, a good
solution is to just make both modules depend on that. We can add the
`runRobot` function to this module as well, since it's small and
closely related to state management. The module exports both the
`VillageState` class and the `runRobot` function.

Finally, the robots, along with the values they depend on such as
`mailRoute`, could go into an `example-robots` module, which depends
on `./roads` and exports the robot functions. To make it possible for
`goalOrientedRobot` to do route-finding, this module also depends
on `dijkstrajs`.

By offloading some work to ((NPM)) modules, the code became a little
smaller. Each individual module does something rather simple and can
be read on its own. Dividing code into modules also often suggests
further improvements to the program's design. In this case, it seems a
little odd that the `VillageState` and the robots depend on a specific
road graph. It might be a better idea to make the graph an argument to
the state's constructor and make the robots read it from the state
object—this reduces dependencies (which is always good) and makes it
possible to run simulations on different maps (which is even better).

Is it a good idea to use NPM modules for things that we could have
written ourselves? In principle, yes—for nontrivial things like the
pathfinding function you are likely to make mistakes and waste time
writing them yourself. For tiny functions like `random-item`, writing
them yourself is easy enough. But adding them wherever you need them
does tend to clutter your modules.

However, you should also not underestimate the work involved in
_finding_ an appropriate NPM package. And even if you find one, it
might not work well or may be missing some feature you need. On top
of that, depending on NPM packages means you have to make sure they
are installed, you have to distribute them with your program, and you
might have to periodically upgrade them.

So again, this is a trade-off, and you can decide either way depending
on how much the packages help you.

hint}}

### Roads module

{{index "roads module (exercise)"}}

Write a ((CommonJS module)), based on the example from [Chapter
?](robot), that contains the array of roads and exports the graph
data structure representing them as `roadGraph`. It should depend on a
module `./graph`, which exports a function `buildGraph` that is used
to build the graph. This function expects an array of two-element
arrays (the start and end points of the roads).

{{if interactive

```{test: no}
// Add dependencies and exports

const roads = [
  "Alice's House-Bob's House",   "Alice's House-Cabin",
  "Alice's House-Post Office",   "Bob's House-Town Hall",
  "Daria's House-Ernie's House", "Daria's House-Town Hall",
  "Ernie's House-Grete's House", "Grete's House-Farm",
  "Grete's House-Shop",          "Marketplace-Farm",
  "Marketplace-Post Office",     "Marketplace-Shop",
  "Marketplace-Town Hall",       "Shop-Town Hall"
];
```

if}}

{{hint

{{index "roads module (exercise)", "destructuring binding", "exports object"}}

Since this is a ((CommonJS module)), you have to use `require` to
import the graph module. That was described as exporting a
`buildGraph` function, which you can pick out of its interface object
with a destructuring `const` declaration.

To export `roadGraph`, you add a property to the `exports` object.
Because `buildGraph` takes a data structure that doesn't precisely
match `roads`, the splitting of the road strings must happen in your
module.

hint}}

### Circular dependencies

{{index dependency, "circular dependency", "require function"}}

A circular dependency is a situation where module A depends on B, and
B also, directly or indirectly, depends on A. Many module systems
simply forbid this because whichever order you choose for loading such
modules, you cannot make sure that each module's dependencies have
been loaded before it runs.

((CommonJS modules)) allow a limited form of cyclic dependencies. As
long as the modules do not replace their default `exports` object and
don't access each other's interface until after they finish loading,
cyclic dependencies are okay.

The `require` function given [earlier in this
chapter](modules#require) supports this type of dependency cycle. Can
you see how it handles cycles? What would go wrong when a module in a
cycle _does_ replace its default `exports` object?

{{hint

{{index overriding, "circular dependency", "exports object"}}

The trick is that `require` adds modules to its cache _before_ it
starts loading the module. That way, if any `require` call made while
it is running tries to load it, it is already known, and the current
interface will be returned, rather than starting to load the module
once more (which would eventually overflow the stack).

If a module overwrites its `module.exports` value, any other module
that has received its interface value before it finished loading will
have gotten hold of the default interface object (which is likely
empty), rather than the intended interface value.

hint}}
