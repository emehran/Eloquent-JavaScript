{{meta {load_files: ["code/packages_chapter_10.js", "code/chapter/07_robot.js"]}}}

# ماژول‌ها

{{quote {author: "Tef", title: "Programming is Terrible", chapter: true}

کدی بنویسید که راحت بتوان در صورت نیاز حذفش کرد، لازم نیست قابل توسعه باشد.

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_10.jpg", alt: "Picture of a building built from modular pieces", chapter: framed}}}

{{index organization, [code, "structure of"]}}

یک برنامه‌ی ایده‌آل دارای ساختاری شفاف و روشن است. به راحتی می‌توان کارکرد آن
را توضیح داد و هر بخش از آن نقشی را ایفا می‌کند که به خوبی تعریف شده است.

{{index "organic growth"}}

معمولا یک برنامه‌ی واقعی به شکلی ارگانیک رشد می‌کند. قابلیت‌های جدید، همانطور که
لازم می‌شوند به برنامه افزوده می‌شوند. ساختاردهی – و حفظ ساختار – کار مجزایی است.
کاری است که فقط در آینده با مزایای آن روبرو می‌شوید، زمانی که کسی دوباره روی
برنامه قرار است کار کند. پس ممکن است وسوسه شوید که از آن غفلت کنید و
بگذارید بخش‌های برنامه عمیقا دچار آشفتگی شوند.

{{index readability, reuse, isolation}}

در عمل این غفلت دو اشکال ایجاد می‌کند. اول اینکه درک یک سیستم بدون‌ ساختار مشکل است.
اگر همه‌ی بخش‌های برنامه در تماس با دیگر بخش‌ها باشند، سخت می‌توان بخشی از برنامه را
به صورت جداگانه بررسی نمود. شما باید درکی کلی و جامع از برنامه داشته باشید.
دوم اینکه، اگر بخواهید هر کدام از قابلیت‌های برنامه‌ای اینچنینی را در جایی دیگر
استفاده کنید، از اول نوشتن آن قابلیت ممکن است از جداسازی آن از برنامه، آسان تر
باشد.

اصطلاح “big ball of mud” (توپ‌ بزرگ گلی) اغلب برای برنامه‌های بزرگی که ساختاری ندارند استفاده می
شود. همه چیز به هم چسبیده است و زمانی که قصد دارید یک قسمت را جدا کنید، کل آن
قسمت یا برنامه متلاشی می‌شود و دستانتان را کثیف می‌کند.

## ماژول‌ها

{{index dependency, [interface, module]}}

استفاده از _ماژول‌ها_ تلاشی برای اجتناب از این گونه مشکلات است. یک ماژول بخشی از
برنامه است که مشخص می‌کند به کدام بخش‌های دیگر از برنامه وابسته است (وابستگی‌های
آن) و چه قابلیتی برای استفاده‌ی دیگر ماژول ها فراهم می‌کند (_رابط_ آن).

{{index "big ball of mud"}}

رابط‌های ماژول شباهت‌ زیادی با رابط‌های شیء دارند، همانطور که با آن ها در[فصل ?](object#interface)
آشنا شدیم. رابط‌ها بخشی از ماژول را در دسترس جهان بیرون می گذارند و بقیه‌ی قسمت
ها را به صورت خصوصی حفظ می کنند. با محدودسازی راه‌های تعامل ماژول‌ها با یکدیگر ، سیستم بیشتر
شبیه لگو می‌شود، جایی که قطعات توسط متصل‌کننده‌هایی که به خوبی تعریف شده اند با هم
تعامل دارند و کمتر به توپ گلی شباهت خواهند داشت که همه چیز در آن با هم مخلوط شده است.

{{index dependency}}

ارتباطات بین ماژول ها را _وابستگی‌ها_ می نامند. زمانی که یک ماژول به بخشی از یک
ماژول دیگر نیاز دارد، گفته می‌شود که به آن ماژول وابستگی دارد. زمانی که این
وابستگی در خود ماژول به صورت مشخص اعلام شود، می‌توان از آن برای شناسایی دیگر
ماژول‌هایی که لازم است برای اجرای یک ماژول خاص حضور داشته باشند استفاده کرد و به
صورت خودکار آن وابستگی‌ها را بارگیری کرد.

برا جداسازی ماژول‌ها به این روش ، لازم است هر کدام از آن‌ها، قلمروی (scope) خصوصی خودش را داشته
باشد.

فقط قرار دادن کدهای جاوااسکریپت در فایل های جداگانه این امکان را فراهم نمی‌کند.
فایل‌ها همچنان فضای نام سراسری یکسانی را به صورت مشترک استفاده می کنند. ممکن است به صورت
تصادفی یا آگاهانه بین متغیرهای یکدیگر تداخل ایجاد کنند و ساختار وابستگی همچنان
غیر شفاف خواهد ماند. می‌توان کار بهتری انجام داد که در ادامه خواهیم دید.

{{index design}}

طراحی یک ساختار ماژول مناسب برای یک برنامه ممکن است سخت باشد. در مرحله‌ای که هنوز
در حال بررسی مشکل هستید، و چیزهای متفاوتی را آزمایش می‌کنید، ممکن است علاقه‌ای
نداشته باشید که زیاد به ساختاردهی فکر کنید، چرا که می‌تواند باعث حواسپرتی زیادی
بشود. زمانی که به نتیجه‌ای استوار و قابل اتکا رسیدید ، وقت مناسب برای اقدام جهت
سازماندهی برنامه فرا رسیده است.

## بسته‌ها

{{index bug, dependency, structure, reuse}}

یکی از مزایای ساختن یک برنامه بر اساس قسمت‌های جداگانه، و داشتن قابلیت اجرای آن
قسمت‌ها به صورت مستقل، این است که ممکن است بتوانید آن قسمت‌ها را در برنامه‌های
مختلف به کار ببرید.

{{index "parseINI function"}}

اما چگونه آن را راه‌اندازی می‌کنید؟ فرض کنیم من می خواهم که تابع `parseINI` را که
در [فصل ?](regexp#ini) نوشتیم در برنامه‌ی دیگری استفاده کنم. اگر روشن باشد که این تابع چه
وابستگی‌هایی دارد (که در اینجا ندارد)، می‌توانم به سادگی کدهای مورد نیاز را به
پروژه‌ی جدیدم کپی کنم و از آن استفاده کنم. اما در این صورت اگر مشکلی در آن کد
پیدا کنم، احتمالا آن مشکل را فقط در برنامه‌ای که در حال کار روی آن هستم رفع
خواهم کرد و فراموش می کنم که در برنامه‌ی دیگر نیز آن را اصلاح کنم.

{{index duplication, "copy-paste programming"}}

به محض اینکه به کپی کردن کدها اقدام کنید متوجه خواهید شد که با جابجا کردن کپی‌ها
بین برنامه ها و به روز رسانی آن ها وقت و انرژی خودتان را تلف می‌کنید.

اینجا‌ است که بسته‌ها(_((package))s_) کاربرد خواهند داشت. یک بسته یک قطعه کد است که
می‌تواند توزیع شود (کپی و نصب شود). یک بسته می‌تواند دارای یک یا چند ماژول باشد
و همچنین اطلاعاتی درباره‌ی دیگر بسته‌هایی که به آنها وابسته است دارد. یک بسته همچنین
همراه با مستنداتی می آید که کارکرد آن را شرح می دهد، بنابراین کسانی که بسته را
ننوشته اند قادر خواهند بود که از آن استفاده کنند.

زمانی که مشکلی در یک بسته شناسایی شود یا ویژگی جدیدی به آن افزوده شود بسته به
روز رسانی می گردد. اکنون برنامه‌هایی که به آن وابستگی داشته اند (که خود ممکن
است بسته باشند ) می‌توانند به نسخه جدیدتر به‌روز شوند.

{{id modules_npm}}

{{index installation, upgrading, "package manager", download, reuse}}

کارکردن به این روش نیاز به زیرساخت دارد. جایی را نیاز داریم که بسته‌ها را ذخیره و
جستجو کنیم و راهی سرراست برای نصب و به روز رسانی آن ها مورد نیاز است. در دنیای
جاوااسکریپت این زیرساخت توسط NPM در ([_https://npmjs.org_](https://npmjs.org)) فراهم شده است.

NPM دارای دو بخش است: یک سرویس آنلاین که افراد می‌توانند به بارگیری و
بارگذاری بسته‌ها اقدام کنند و یک برنامه (که به همراه Node.js می آید) که به شما
کمک می‌کند که آن ها را مدیریت کنید.

{{index "ini package"}}

در زمان نوشتن این کتاب، بیش از نیم میلیون بسته در NPM وجود دارد. باید اشاره کنم که بخش
زیادی از این بسته‌ها بدرد نخور هستند اما تقریبا هر بسته‌ی مفیدی که عموما در دسترس
باشد را می‌توان آنجا پیدا کرد. به عنوان مثال، یک تجزیه‌گر فایل ini، شبیه به چیزی
که خودمان در [فصل ?](regexp) ساختیم، در بسته‌ای به نام `ini` وجود دارد.

{{index "command line"}}

[فصل ?](node) شما را با نحوه‌ی نصب بسته‌ها به صورت محلی و با استفاده از برنامه‌ی خط فرمان
`npm` آشنا خواهد کرد.

در دسترس داشتن بسته‌های باکیفیت و قابل دانلود بسیار ارزشمند است. این بدین معنا است
که می‌توان از اختراع دوباره یک برنامه در بیشتر مواقع جلوگیری کنیم، برنامه ای که
صدها نفر پیش‌ تر نوشته اند و می‌توان آن را با چند حرکت به صورت استوار و تست شده
پیاده سازی کرد.

{{index maintenance}}

کپی کردن یک نرم افزار آسان است، بنابراین اگر کسی آن را نوشته باشد، انتشار آن
بین دیگر افراد روند کارآمدی محسوب می‌شود. اما به عنوان فرد اول نوشتن آن کار می
برد و پاسخ دادن به افرادی که مشکلاتی را در کد پیدا می‌کنند یا درخواست ویژگی جدیدی
را دارند، کار بیشتری می طلبد.

به صورت پیش فرض، شما مالک کپی‌رایت کدی هستید که می نویسید و دیگر افراد فقط با
اجازه‌ی شما می‌توانند از آن استفاده کنند. اما به دلیل اینکه بعضی افراد باحال
(nice) هستند و اینکه انتشار یک نرم افزار خوب باعث کمی شهرت بین برنامه‌نویسان می
شود، خیلی از بسته‌ها تحت مجوزی منتشر می‌شوند که به صراحت اجازه استفاده دیگران از
برنامه را می دهد.

بیشتر کدهایی که در NPM وجود دارند دارای چنین مجوزی هستند. بعضی از مجوزها از شما
می خواهند که کدی که با استفاده از بسته نوشته‌اید را با همان مجوز منتشر کنید. بعضی
دیگر کمتر درخواستی دارند و فقط لازم است که مجوزی که همراه بسته وجود دارد را در
هنگام توزیع کنار آن نگه دارید. بیشتر جامعه‌ی برنامه‌نویسی جاوااسکریپت از این نوع
مجوز استفاده می کنند. زمانی که از بسته‌های دیگر افراد استفاده می‌کنید، مطمئن شوید
که از مجوز آن آگاهی دارید.

## فراهم ساختن ماژول‌ها

قبل از 2015 در جاوااسکریپت سیستم ماژول داخلی وجود نداشت. با این وجود برنامه‌نویسان برای
بیشتر از یک دهه، سیستم های بزرگی را برنامه‌نویسی می کردند درحالیکه _نیاز_ به
ماژول‌ها وجود داشت.

{{index [function, scope], [interface, module], [object, as module]}}

بنابراین آن ها سیستم ماژول خودشان را با استفاده از خود زبان طراحی کردند. می
توان از توابع جاوااسکریپت برای ایجاد قلمروهای محلی و از اشیاء به
عنوان رابط‌های ماژول استفاده کرد.

{{index "Date class", "weekDay module"}}

مثال زیر یک ماژول برای انتخاب بین نام روزها و عددشان است ( که از متد `getDay` مربوط به
`Date` استفاده می‌کند). رابط آن از <bdo>`weekDay.name`</bdo> و <bdo>`weekDay.number`</bdo> تشکیل شده است
و متغیر محلی `names` را در قلمروی یک تابع که بلادرنگ فراخوانی می‌شود پنهان می‌کند.

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

این سبک از ماژول‌ها، تا حدی ایزوله کردن را فراهم می‌کند، اما وابستگی را
پشتیبانی نمی‌کند. به جای آن، رابطش را در قلمروی سراسری قرار می دهد، و انتظار
دارد که در صورت وجود، وابستگی‌هایش تعریف شده باشند تا بتواند کاری مشابه سیستم
وابستگی‌ها انجام دهد. برای مدتی طولانی این روش در برنامه‌نویسی وب استفاده می شد
اما الان تقریبا از رده خارج شده است.

اگر قصد دارید ارتباطات مربوط به وابستگی را به عنوان بخشی از کد داشته باشید،
باید مدیریت بارگیری وابستگی‌ها را به عهده بگیرید. برای این کار لازم است بتوانیم رشته‌ها را به عنوان کد اجرا کنیم. این کار در جاوااسکریپت قابل اجرا است.

{{id eval}}

## ارزیابی داده‌ها به عنوان کد‌های اجرایی

{{index evaluation, interpretation}}

راه‌های متعددی برای گرفتن داده‌ها (یک رشته از کد) و اجرای آن به عنوان بخشی از
برنامه کنونی وجود دارد.

{{index isolation, eval}}

روشن ترین راه، استفاده از عملگر `eval` است که رشته‌ای را در قلمروی (scope)
_فعلی_ اجرا می‌کند. این ایده معمولا ایده‌ی بدی محسوب می‌شود چرا که باعث از بین
رفتن بعضی از خاصیت‌هایی می‌شود که در قلمرو‌ها به صورت نرمال وجود دارند، مثل حدس
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
تابع دو آرگومان دریافت می‌کند: یک رشته که حاوی یک لیست جدا شده با ویرگول از نام
آرگومان‌ها است و یک رشته که حاوی بدنه تابع است. این تابع کد را درون یک مقدار
تابع می‌پوشاند بنابراین قلمروی مختص به خودش را خواهد داشت و تداخلی با دیگر قلمروها
نخواهد داشت.

```
let plusOne = Function("n", "return n + 1;");
console.log(plusOne(4));
// → 5
```

این دقیقا همان‌ چیزی است که برای یک سیستم ماژول نیاز داریم. می‌توانیم کدهای ماژول را
درون یک تابع قرار دهیم و از قلمروی مربوط به تابع به عنوان قلمروی ماژول استفاده
نماییم.

## CommonJS

{{id commonjs}}

{{index "CommonJS modules"}}

رایج ترین شیوه‌ای که برای اضافه کردن ماژول‌ها به جاوااسکریپت استفاده می‌شود،
ماژول‌های _CommonJS_ می‌باشد. <bdo>Node.js</bdo> از آن استفاده می‌کند و سیستمی است که
بیشتر بسته‌های NPM از آن استفاده می کنند.

{{index "require function", [interface, module]}}

مفهوم اصلی در ماژول‌های CommonJS تابعی به نام `‌require` است. زمانی که از این تابع
به همراه نام ماژول یک وابستگی استفاده می‌کنید، این تابع اطمینان حاصل می‌کند
که ماژول مورد نظر بارگیری شده است و رابط آن را برمی گرداند.

{{index "exports object"}}

به دلیل اینکه بارگیرنده‌ (loader)، ماژول مورد نظر را درون یک تابع قرار می دهد، ماژول‌ها به
صورت خودکار قلمروی‌ محلی خودشان را می گیرند. تنها کاری که باید بکنند این است که
تابع `require` را فراخوانی کنند تا به وابستگی های آن ها دسترسی داشته باشند و
رابطشان را در شیئی که به `exports` تخصیص یافته قرار دهند.

{{index "formatDate module", "Date class", "ordinal package", "date-names package"}}

این ماژول به عنوان مثال، یک تابع ویرایش تاریخ را فراهم می سازد (date-formatting).
اینجا از دو بسته از NPM استفاده می‌شود – بسته‌ی `ordinal` برای تبدیل اعداد به
رشته‌هایی شبیه `"1st"` و `"2nd"` , و بسته‌ی <bdo>`date-names`</bdo> برای گرفتن نام‌های انگلیسی روزهای
هفته و ماه‌ها. این ماژول یک تابع به نام `fomatDate` را صادر (export) می‌کند که یک شیء `Date` و
یک رشته به عنوان قالب دریافت‌ می‌کند.

رشته‌ی قالب می‌تواند حاوی کدهایی باشد که فرمت
تاریخ را مشخص می‌کند مثل `YYYY` برای نمایش سال به صورت کامل و `‌Do` برای نام‌های
ترتیبی روزهای ماه. می‌توانید به آن رشته‌ای به شکل <bdo>`"MMMM Do YYYY"`</bdo> برای دریافت
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
متفاوت است صادر می‌کند- دو مقداری که در نام های آرایه‌ها استفاده کردیم. استفاده
از روش تخریب (Destructuring) برای ایجاد متغیر برای رابط‌های وارد شده بسیار مناسب
است.

ماژول تابع رابط خودش را به `exports` اضافه می‌کند در نتیجه ماژول‌های وابسته، به آن
دسترسی خواهند داشت. می‌توانیم از این ماژول به این صورت استفاده کنیم:

```
const {formatDate} = require("./format-date");

console.log(formatDate(new Date(2017, 9, 13),
                       "dddd the Do"));
// → Friday the 13th
```

{{index "require function", "CommonJS modules", "readFile function"}}

{{id require}}

می‌توان `require` را در کوتاه ترین شکل خودش تعریف کرد:

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
صورت رشته برمی‌گرداند. جاوااسکریپت استاندارد، این قابلیت را فراهم نمی‌کند – اما
محیط‌های متفاوت جاوااسکریپت مثل مرورگر و <bdo>Node.js</bdo>، راه‌های خودشان را برای دسترسی به
فایل‌ها فراهم می سازند. مثال بالا فقط وانمود می‌کند که `readFile` وجود دارد.

{{index cache, "Function constructor"}}

برای جلوگیری از بارگیری چندباره‌ی یک ماژول ، `require` ماژول‌هایی که تاکنون
بارگیری شده اند را جایی (در حافظه‌ی نهان) نگه داری می‌کند. در زمان فراخوانی،
ابتدا بارگیری ماژول خواسته شده را در گذشته بررسی می‌کند و در صورت نبود،
آن را بارگیری می‌کند. این روند شامل خواندن کد ماژول، قرار دادن آن درون یک
تابع و فراخوانی آن می‌شود.

{{index "ordinal package", "exports object", "module object", [interface, module]}}

رابط بسته‌ی `ordinal` که قبل تر دیدیم یک شیء نیست بلکه یک تابع است. یک ایراد وارده
به ماژول‌های CommonJS این است که اگرچه سیستم ماژول، یک رابط به صورت شیئی خالی برای شما ایجاد
می‌کند (که در `exports` قرار می‌گیرد)، می‌توانید آن را با هر مقداری که بخواهید
به وسیله‌ی بازنویسی خاصیت <bdo>`module.exports`</bdo> جایگزین کنید. این کار را
خیلی از ماژول ها انجام می‌دهند تا بتوانند یک مقدار ساده را به جای یک شیء رابط
صادر کنند.

با تعریف `require`، `exports` و `module` به عنوان پارامترهای تابع پوشش دهنده‌ی تولید شده (و ارسال مقدارهای مناسب در هنگام فراخوانی)، بارگیرنده‌ اطمینان حاصل می
کند که این متغیرها در قلمروی مربوط به ماژول در دسترس خواهند بود.

{{index resolution, "relative path"}}

روشی که در آن، رشته‌ی داده شده به `require` به یک نام فایل واقعی یا یک آدرس وب
تفسیر می‌شود، در سیستم های مختلف متفاوت است. زمانی که این رشته با
<bdo>`"./"`</bdo> یا <bdo>`"../"`</bdo> شروع می‌شود عموما نسبت به نام فایل ماژول
فعلی در نظر گرفته می‌شود. بنابراین <bdo>`"./format-date"`</bdo> فایلی به نام
<bdo>`format-date.js`</bdo> که در همان پوشه قرار دارد در نظر گرفته می‌شود.

زمانی که نام نسبی نیست، <bdo>Node.js</bdo> به جستجوی بسته ای با همان نام اقدام می‌کند. در
کد مثال این فصل، ما این گونه نام‌ها را به عنوان بسته‌های NPM تفسیر می کنیم. در [فصل ?](node) به جزئیات نصب و استفاده از ماژول‌های NPM خواهیم پرداخت.

{{id modules_ini}}

{{index "ini package"}}
اکنون به جای نوشتن تجزیه‌گر فایل INI خودمان، می‌توانیم از یکی از بسته‌های موجود در
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

اما کمی لازم است تا دستی به سر و روی آن‌ها بکشیم. ظاهر نوشتاری آن اندکی مشکل دارد
– به عنوان مثال چیزهایی که به `exports` اضافه می‌کند در قلمروی محلی در دسترس
نیستند. و به دلیل اینکه `require` یک فراخوانی به یک تابع معمولی است که هر نوع
آرگومانی را قبول می‌کند، نه فقط مقادیر رشته‌ای، تشخیص وابستگی‌های یک ماژول بدون
اجرای کدهای آن می‌توان سخت شود.

{{index "import keyword", dependency, "ES modules"}}

{{id es}}

به همین علت است که جاوااسکریپت استاندارد از 2015 سیستم ماژول متفاوت خودش را
معرفی کرد که معمولا _((ES modules))_ نامیده می‌شود. _ES_ مخفف _ECMAScript_
است. مفاهیم اصلی وابستگی‌ها و رابط‌ها به همان صورت باقی می ماند اما جزئیات آن‌ها متفاوت
است.  درنتیجه، نشان‌گذاری اکنون درون زبان یکپارچه شده است. به جای فراخوانی
یک تابع برای دسترسی به یک وابستگی، از کلید‌واژه‌ی مخصوصی به نام `import` استفاده می
کنید.

```
import ordinal from "ordinal";
import {days, months} from "date-names";

export function formatDate(date, format) { /* ... */ }
```

{{index "export keyword", "formatDate module"}}

به طور مشابه، کلیدواژه‌ی `export` برای صدور استفاده می‌شود. می‌توان از آن در
ابتدای یک تابع، کلاس، یا تعریف متغیر استفاده کرد (`let`,
`const`, یا `var`)

{{index [binding, exported]}}

یک رابط ماژول ES، یک مقدار واحد نیست بلکه مجموعه‌ای از متغیرهای نامگذاری شده است.
ماژولی که در بالا آمده است `formatDate` را به یک تابع تخصیص می دهد. زمانی که از
یک ماژول دیگر اقدام به _وارد کردن_ می‌کنید شما متغیر را وارد می‌کنید نه مقدار آن
را که معنای آن این است که یک صدور ماژول ممکن است مقدار یک متغیر را در هر زمانی
تغییر دهد و ماژول‌هایی که آن را وارد کرده اند، مقدار جدیدش را خواهند دید.

{{index "default export"}}

زمانی که متغیری به نام `default` وجود داشته باشد، آن به عنوان مقدار صادر شده‌ی اصلی
ماژول در نظر گرفته می‌شود. اگر ماژولی مانند `ordinal` را که در مثال آمد، بدون
استفاده از براکت‌ها دور نام متغیر وارد کنید، متغیر `default` آن ماژول را
دریافت می‌کنید. این گونه ماژول‌ها می‌توانند در کنار صدور `default`، متغیرهای دیگری را هم با نام‌های مختلف صادر
کنند.

برای ایجاد یک صدور پیش فرض (default) می‌توانید از <bdo>`export default`</bdo> قبل از یک
عبارت، تعریف تابع یا کلاس استفاده کنید.

```
export default ["Winter", "Spring", "Summer", "Autumn"];
```

می‌توان نام متغیرهایی که وارد شده اند را با استفاده از `as` تغییر داد.

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
مدیریت ماژول همزمان استفاده می‌شوند و خوب است که قادر باشیم کدها را در هر دو
سیستم بنویسیم و بخوانیم.

## ساخت و بسته‌بندی

{{index compilation, "type checking"}}

در واقع خیلی از پروژه‌های جاوااسکریپت حتی ، از لحاظ فنی، در زبان جاوااسکریپت
نوشته نمی‌شوند. افزونه‌هایی وجود دارد که به طور گسترده استفاده می‌شوند مانند
همان گویشی از جاوااسکریپت که به انواع داده حساس بود و در [فصل ?](error#typing) ذکر شد. برنامه
نویسان اغلب شروع به استفاده از افزونه‌هایی می کنند که خیلی پیش‌تر برای اضافه شدن به پلتفرم‌ها برنامه ریزی شده اند.

برای اجرایی کردن این کار، کدهایشان را کامپایل می کنند، کد را از گویش
جاوااسکریپت مورد نظرشان به جاوااسکریپت ساده ترجمه می کنند – یا حتی به یک نسخه‌ی
قدیمی از جاوااسکریپت ترجمه می کنند که مرورگرهای قدیمی بتوانند آن را اجرا کنند.

{{index latency, performance, [file, access], [network, speed]}}

قرار دادن یک برنامه‌ی ماژولار (پیمانه‌ای) که از 200 فایل متفاوت تشکیل
شده است در یک صفحه‌ی وب مشکلات خودش را خواهد داشت. اگر دریافت و استفاده از یک
فایل در شبکه 50 هزارم ثانیه زمان بگیرد، کل برنامه ده ثانیه زمان خواهد گرفت یا
شاید نیمی از آن زمان اگر بتوانیم چندین فایل را همزمان بارگیری کنید. این زمان
زیادی را تلف خواهد کرد. به دلیل اینکه بارگیری یک فایل بزرگ واحد، از تعداد زیادی
فایل کوچک، عموما سریع تر اتفاق می افتد. برنامه‌نویسان وب به سراغ ابزارهایی رفته
اند که برنامه‌هایشان را (که با زحمت به ماژول ها
تقسیم کرده اند) گرفته و تبدیل به یک فایل بزرگ کند؛ قبل از اینکه بخواهند برنامه
را در وب منتشر کنند. این ابزار را بسته‌ساز می نامند _((bundler))s_.

{{index "file size"}}

و می‌توانیم پا را فراتر بگذاریم. جدا از تعداد فایل‌ها، حجم این فایل‌ها هم در سرعت
انتقالشان در شبکه تعیین کننده است. بنابراین، جامعه‌ی برنامه‌نویسان جاوااسکریپت، ابزارهای
فشرده ساز را اختراع کردند (minifier). این ابزارها یک برنامه‌ی جاوااسکریپت را گرفته
و با حذف توضیحات، فضاهای خالی، تغییر نام متغیرها و جایگزینی بعضی کدها با
معادل‌های کوچک تر، حجم آن را کاهش می‌دهند.

{{index pipeline, tool}}

پس اصلا دور از انتظار نیست که که کدی که در یک بسته‌ی NPM یافته اید یا در یک صفحه‌ی
وب اجرا می‌شود پیش از آن مراحل مختلفی از تبدیل را طی کرده باشد – تبدیل از
جاوااسکریپت مدرن به جاوااسکریپت قدیمی‌تر، از ماژول‌های ES به CommonJS، بسته‌بندی
 و فشرده شده باشد. در این کتاب به جزئیات مربوط به این ابزار نمی پردازیم، چون هم
کسل کننده خواهد بود هم به سرعت تغییر می کنند. فقط حواستان باشد که کدی که شما
معمولا اجرا می‌کنید اغلب همانی نیست که نوشته شده است.

## طراحی ماژول

{{index [module, design], [interface, module], [code, "structure of"]}}

ساختاردهی به یک برنامه یکی از جنبه‌های حساس و ظریف از برنامه‌نویسی محسوب می‌شود.
هر قطعه‌ی معناداری از قابلیت‌ها را می‌توان به اشکال مختلفی مدلسازی کرد.

طراحی یک برنامه‌ی خوب، سلیقه‌ای است – مصالحه‌هایی پیش می‌آید که سلیقه در آن دخیل
است. بهترین راه برای یادگیری ارزش یک طراحی دارای ساختار خوب، این است که برنامه‌های
زیادی را مورد مطالعه قرار دهید یا با آن ها کار کنید تا متوجه شوید که چه چیزی
مفید و چه چیز نادرست است. تصور نکنید که با درهم‌ریختگی و آشفتگی به هر حال پیش خواهد
آمد. تقریبا هر چیزی را با صرف کمی تفکر بیشتر می‌توان با سازماندهی بهتر کرد.

{{index [interface, module]}}

یکی از جنبه‌های طراحی ماژول که باید رعایت شود، آسانی استفاده می‌باشد. اگر چیزی را می سازید که قرار است
توسط کاربران متعددی استفاده شود (یا حتی توسط خودتان، طی مدت سه ماه، زمانی که
دیگر جزئیات کاری که انجام داده اید را به خاطر نمی آورید) اگر رابط برنامه‌ی شما
ساده و قابل تشخیص باشد کمک زیادی می‌کند.

{{index "ini package", JSON}}

ممکن است معنای آن این باشد که از قرارداد‌های موجود پیروی کنید. یک مثال خوب می‌تواند
بسته‌ی `ini` باشد. این ماژول کار بسته‌ی استاندارد `JSON` را با فراهم نمودن `parse` و
`stringify` (برای نوشتن یک فایل INI) تقلید می‌کند و شبیه `JSON` عمل تبدیل بین رشته‌ها
و اشیاء ساده را انجام می دهد. بنابراین رابط در اینجا کوچک و آشنا است و بعد از
اینکه برای اولین بار از آن استفاده کنید احتمالا روش استفاده از آن را به خاطر
خواهید داشت.

{{index "side effect", "hard disk", composability}}

حتی اگر هیچ تابع استاندارد یا بسته‌ای که به طور گسترده استفاده می‌شود برای تقلید
وجود نداشت، می‌توانید با استفاده از ساختارهای داده‌ی ساده و تمرکز بر انجام یک
کار، ماژول های خودتان را قابل پیشبینی کنید. به عنوان مثال، خیلی از بسته‌های تجزیه‌ی
فایل INI در NPM تابعی دارند که مستقیما یک فایل ini را از دیسک سخت خوانده و تجزیه
می‌کند. این کار باعث می‌شود که نتوان از این ماژول‌ها در مرورگر استفاده کرد، به
دلیل اینکه در مرورگر به طور مستقیم به سیستم فایل دسترسی نداریم و پیچیدگی ای
اضافه می‌کند که بهتر بود با ترکیب ماژول با یک تابع خواندن فایل دیگر، حل می شد.

{{index "pure function"}}

این موضوع به یکی دیگر از جنبه‌های مفیدی که در طراحی ماژول وجود دارد اشاره می‌کند – آسانی ترکیب با
کدهای دیگر. ماژول‌هایی که تمرکز روی کار خاصی دارند و مقدارها را محاسبه می کنند در
طیف گسترده‌تری از برنامه ها کاربرد دارند نسبت به ماژول‌های بزرگتری که کارهای
پیچیده‌ای انجام می‌دهند و دارای اثرات جانبی هستند. خواننده‌ی فایل INIای که اصرار
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
ارتباط است باید آن نوع را بداند که باعث ایجاد وابستگی‌های درونی بی‌فایده می‌شود.

اغلب نمی‌توان از تعریف ساختارهای داده‌ی جدید صرف نظر کرد – فقط تعداد کمی از موارد
اساسی و پایه‌ای توسط زبان استاندارد فراهم شده است و خیلی از انواع داده می بایست
پیچیده تر از یک آرایه یا یک نگاشت (map) باشد. اما زمانی که یک آرایه کافی است،
از همان استفاده کنید.

یک نمونه از ساختار داده‌های کمی پیچیده تر، گراف است که در [فصل ?](robot) ذکر شد. یک
راه یکسان و مشخص برای نمایش یک گراف در جاوااسکریپت وجود ندارد. در آن فصل، ما
از یک شیء که خاصیت‌هایش آرایه‌هایی از رشته‌ها را نگه داری می کردند استفاده کردیم –
گره‌هایی که از یک گره قابل دستیابی بودند.

بسته‌های متعددی درباره‌ی مسیریابی در NPM وجود دارند اما هیچ کدامشان از این فرمت
گراف استفاده نمی کنند. معمولا این بسته ها به لبه‌های گراف امکان داشتن وزن را می
دهند، که همان فاصله یا هزینه‌ای است که به آن‌ها اختصاص داده شده است. در نمایش ما این‌کار ممکن نیست.

{{index "Dijkstra, Edsger", pathfinding, "Dijkstra's algorithm", "dijkstrajs package"}}

به عنوان مثال، بسته‌ای به نام `dijkstrajs` وجود دارد. یک راه حل شناخته شده برای
مسیریابی، که نسبتا شبیه به تابع `findRoute` ما است، _الگوریتم دیکسترا_ است که ادسخر
دیکسترا آن را نوشت. پسوند `js` معمولا به انتهای بسته‌ها اضافه می‌شود تا نشان دهد
که آن ها به زبان جاوااسکریپت نوشته شده اند. بسته‌ی `dijkstrajs` از یک فرمت گراف
شبیه به فرمت ما استفاده می‌کند اما به جای آرایه‌ها از اشیاء استفاده می‌کند که
مقدارهای خاصیت‌هایش از جنس اعداد هستند – وزن وزن‌ لبه‌ها.

بنابراین اگر بخواهیم از آن بسته استفاده کنیم، باید اطمینان حاصل کنیم که گراف
ما در فرمتی که بسته پشتیبانی می‌کند ذخیره شده باشد. همه‌ی لبه‌ها وزن یکسانی می گیرند زیرا مدل ساده‌شده‌ی
ما برای هر مسیر هزینه‌ی یکسانی در نظر می‌گیرد (یک دور).

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

این خود مانعی برای ترکیب پذیری محسوب می‌شود – زمانی که بسته‌های متنوع از
ساختارهای متفاوتی برای توصیف چیزهای یکسان استفاده می کنند، ترکیب آن ها سخت
خواهد شد. بنابراین، اگر قصد دارید ترکیب پذیری را هم در طراحی لحاظ کنید، توجه
کنید که چه ساختارهای داده‌ای دیگر برنامه‌نویسان استفاده می کنند و هر وقت ممکن شد
از روش آن ها استفاده کنید.

## خلاصه

ماژول‌ها برای برنامه‌های بزرگ‌تر، ساختار ایجاد می کنند و این کار را با جداسازی
کدها به بخش‌هایی با رابط‌ها و وابستگی های شفاف انجام می‌دهند. رابط بخشی از ماژول
است که برای دیگر ماژول‌ها، قابل رویت است و وابستگی ها، ماژول‌های دیگری هستند که
ماژول از آن‌ها استفاده می‌کند.

به دلیل اینکه جاوااسکریپت از ابتدا سیستم ماژول نداشت، سیستم CommonJS با استفاده
از خود زبان ایجاد شد. اما بعد از مدتی، جاوااسکریپت سیستم داخلی خودش را ایجاد
کرد؛ سیستمی که در کنار سیستم CommonJS وجود دارد.

یک بسته، قطعه کدی است که می‌توان آن را مستقلا توزیع کرد. NPM مخزن بسته‌های
جاوااسکریپت است. می‌توانید انواع بسته‌های کاربردی (و بدون کاربرد) را از آن دانلود
کنید.

## تمرین‌ها

### یک ربات ماژولار

{{index "modular robot (exercise)", module, robot, NPM}}

{{id modular_robot}}

موارد زیر، متغیرهایی هستند که پروژه‌ی [فصل ?](robot) ایجاد می‌کند:

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

اگر بخواهید آن پروژه را به صورت برنامه‌ای ماژولار بنویسید، چه ماژول‌هایی ایجاد
می‌کنید. کدام ماژول به یک ماژول دیگر وابستگی خواهد داشد؟ و رابط آن ها چگونه خواهد بود؟

کدام قسمت‌ها احتمالا قبلا نوشته شده اند و از NPM در دسترس هستند؟ ترجیح می‌دهید تا از بسته‌های NPM استفاده کنید یا خودتان آن‌ها را بنویسید؟

{{hint

{{index "modular robot (exercise)"}}

من اگر بودم این کار را می کردم (البته فقط یک راه درست برای طراحی یک ماژول وجود
ندارد):

{{index "dijkstrajs package"}}

کدی که برای ساخت گراف مسیر استفاده می‌شود در درون ماژول `graph` وجود دارد. به
دلیل اینکه من ترجیح می دهم از بسته‌ی `dijkstrajs` از NPM استفاده کنم تا اینکه
خودم بنویسم، گراف را طوری خواهیم ساخت که داده‌هایی مناسب بسته‌ی `dijkstajs`
تولید کند. این ماژول یک تابع صدور می‌کند، تابع `buildGraph`. من تابع
`buildGraph` را طوری می سازم که آرایه‌ای از آرایه‌های دو عنصری را به جای
رشته‌هایی که خط‌ تیره دارند بپذیرد تا ماژول کمتر به فرمت ورودی وابسته باشد.

ماژول `roads` حاوی داده‌های خام مربوط به راه‌ها می‌باشد ( آرایه‌ی `roads`) به
همراه متغیر `roadGraph`. این ماژول به <bdo>`./graph`</bdo> وابستگی دارد و گراف
راه را صادر می‌کند.

{{index "random-item package"}}

کلاس `VillageState` در ماژول `state` قرار دارد. این کلاس به ماژول
<bdo>`./roads`</bdo> وابستگی‌ دارد به این خاطر که نیاز دارد تا وجود راه
داده‌شده را اعتبارسنجی کند. همچنین نیاز به `randomPick` دارد. چون این تابع سه خط
کد بیشتر ندارد، می‌توانیم آن را درون ماژول `state` به عنوان یک تابع کمکی قرار
دهیم. اما `randomRobot` هم به آن نیاز دارد. پس باید یا کد را تکرار کنیم یا یک
ماژول برایش در نظر بگیریم. این تابع در NPM به نام `random-item` وجود دارد پس
بهتر است که هر دو ماژول را به آن وابسته کنیم. می‌توانیم تابع `runRobot` را به
این ماژول نیز اضافه کنیم چراکه هم کوچک است و هم به مدیریت وضعیت مرتبط می‌باشد.
ماژول هر دوی کلاس `VillageState` و تابع `runRobot` را صادر می نماید.

در انتها، روبات‌ها به همراه مقادیری که به آن‌ها وابسته‌ اند مانند `mailRoute` را
می‌توان درون یک ماژول `example-robots` قرار داد که خود این ماژول وابسته به
<bdo>`./roads`</bdo> می‌باشد و تابع robot را صادر می‌کند. برای اینکه امکان
مسیریابی را برای `goalOrientedRobot` فراهم کنیم، این ماژول همچنین به
`dijkstrajs` وابسته خواهد بود.

با سپردن بخشی از کارها به ماژول‌های NPM، کد ما اندکی کوچک‌تر می‌شود. هر ماژول مجزا کاری
نسبتا ساده انجام می دهد و مستقل عمل می‌کند. تقسیم کد به ماژول‌ها اغلب موجب بهبود‌های
بیشتری در طراحی برنامه می‌شود. در این مثال، کمی غیر طبیعی به نظر می‌رسد که `VillageState` و ربات‌ها به
یک گراف مسیر خاص وابسته هستند. احتمالا بهتر است که گراف را به عنوان یک آرگومان برای سازنده‌ی وضعیت
استفاده کنیم و ربات‌ها آن را از شیء وضعیت بخوانند - که این کار وابستگی‌ها را کاهش می دهد (که همیشه خوب است) و امکان اجرای شبیه‌سازی روی نقشه‌های متفاوت را هم فراهم می‌کند (که عالی است).

آیا ایده‌ی خوبی است که از ماژول‌های NPM به جای چیزهایی که خودمان می‌توانستیم بنویسیم استفاده کنیم؟
قاعدتا بله. برای موارد مهم مثل همین مسیریابی که در صورت کدنویسی توسط خودتان، احتمال اشتباه و اتلاف
وقت، زیاد است. برای تابع‌های کوچک مثل `random-item`، که نوشتن آن خیلی ساده است، استفاده از آن‌ها
در جاهای مختلف می‌تواند ماژول شما را شلوغ و بی نظم کند.

به هر حال، نباید کاری که صرف پیدا کردن یک بسته‌ی مناسب در NPM می‌شود را هم دست
کم بگیرید. و حتی اگر بسته‌ی مورد نظر را پیدا کردید، ممکن است به خوبی کار نکند یا
ویژگی‌های مورد نظر شما را نداشته باشد. علاوه‌ بر آن، وابسته بودن به بسته‌های NPM به
این معنا است که باید اطمینان حاصل کنید که آن‌ها نصب شده اند، و باید به همراه
برنامه‌تان توزیع شوند، و هر از چند گاهی باید به روز رسانی شوند.

بنابراین یک بار دیگر، این تصمیم نوعی مصالحه و بده بستان است و شما باید میزان کمکی که این بسته‌ها
به برنامه شما می کنند را در نظر بگیرید.

hint}}

### ماژول راه‌ها

{{index "roads module (exercise)"}}

ماژول CommonJS ای بنویسید که بر اساس مثال [فصل
?](robot) آرایه‌ای از راه‌ها را داشته باشد و ساختار داده‌ی گراف را به عنوان `roadGraph` صادر کند. باید
به ماژولی به نام <bdo>`./graph`</bdo> وابسته باشد که تابعی به نام `buildGraph` را صادر می‌کند
که برای ساخت گراف استفاده می‌شود. این تابع نیاز به آرایه‌ای از آرایه‌های دو عنصری دارد ( نقاط شروع و پایان راه‌ها).

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

چون این یک ماژول CommonJS است، باید از `require` برای ورود ماژول گراف استفاده
کنید. این ماژول به صورت تابع `buildGraph` مشخص شده است که می‌توانید آن را از
رابط شیئش به وسیله اعلان `const` و روش تخریب (destruction) مورد ارجاع قرار
دهید.

برای صدور `roadGraph`، یک خاصیت به شیء `exports` اضافه می‌کنید. به دلیل اینکه `buildGraph` ساختار داده‌ای دریافت می‌کند که دقیقا با ‍‍‍`roads` مطابقت ندارد، عمل جداسازی رشته‌ی راه‌ها باید در درون ماژول شما انجام شود.

hint}}

### وابستگی‌های دایره‌ای

{{index dependency, "circular dependency", "require function"}}

یک وابستگی دایره‌ای در شرایطی رخ می دهد که ماژول A به ماژول B، ماژول B همچنین،
مستقیم یا غیر مستقیم، به A وابسته باشد. خیلی از سیستم‌های ماژول، این کار را ممنوع
کرده اند به دلیل اینکه هر ترتیبی که شما انتخاب کنید برای بارگیری این گونه
ماژول‌ها، نمی‌توان قبل از اجرا مطمئن شد وابستگی‌های هر ماژول بارگیری شده اند یا
خیر.

ماژول های CommonJS شکل محدودی از وابستگی‌های دایره‌ای را اجازه می دهد. تا زمانی
که ماژول‌ها، اشیاء export پیشفرض‌شان را جایگزین نکنند و به رابط‌های یکدیگر
اجازه‌ی دسترسی قبل از پایان بارگیری ندهند، وابستگی‌های دایره‌ای مجاز هستند.

تابع `require` که [پیش‌تر در این فصل](modules#require) آمد، از این نوع از
چرخه‌ی وابستگی پشتیبانی می‌کند. آیا می‌توانید توضیح دهید چگونه‌ این کار را انجام می
دهد؟ چه مشکلی پیش می آید اگر یک ماژول در یک چرخه‌ی شیء `export` پیشفرض خود را
جایگزین کند؟

{{hint

{{index overriding, "circular dependency", "exports object"}}

نکته این است که `require` ماژول‌ها را پیش از آنکه شروع به بارگیری ماژول کند به حافظه‌ی نهانش اضافه می‌کند. در این صورت، اگر در حین اجرای یک `require` فراخوانی دیگری به ماژول صورت گیرد، تابع از پیش نسبت به آن باخبر است و رابط فعلی برگردانده می‌شود و از بارگیری مجدد و اضافی ماژول جلوگیری می‌کند ( که این بارگیری مجدد باعث رخ دادن سرریز خواهد شد).

اگر یک ماژول مقدار <bdo>`module.exports`</bdo> اش را بازنویسی کند، هر ماژول
دیگری که مقدار رابط آن ماژول را قبل از پایان بارگیری‌اش دریافت کرده است، شیء رابط پیش‌فرض
را نگه خواهد داشت (که احتمالا تهی است) نه مقدار رابطی که انتظارش را دارد.

hint}}
