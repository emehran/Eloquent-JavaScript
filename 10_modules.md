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

## Evaluating data as code

{{index evaluation, interpretation}}

There are several ways to take data (a string of code) and run it as
part of the current program.

{{index isolation, eval}}

The most obvious way is the special operator `eval`, which will
execute a string in the _current_ ((scope)). This is usually a bad idea
because it breaks some of the properties that scopes normally have,
such as it being easily predictable which binding a given name refers
to.

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

A less scary way of interpreting data as code is to use the `Function`
constructor. It takes two arguments: a string containing a
comma-separated list of argument names and a string containing the
function body. It wraps the code in a function value so that it gets
its own scope and won't do odd things with other scopes.

```
let plusOne = Function("n", "return n + 1;");
console.log(plusOne(4));
// → 5
```

This is precisely what we need for a module system. We can wrap the
module's code in a function and use that function's scope as module
((scope)).

## CommonJS

{{id commonjs}}

{{index "CommonJS modules"}}

The most widely used approach to bolted-on JavaScript modules is
called _CommonJS modules_. ((Node.js)) uses it and is the system used
by most packages on ((NPM)).

{{index "require function", [interface, module]}}

The main concept in CommonJS modules is a function called `require`.
When you call this with the module name of a dependency, it makes sure
the module is loaded and returns its interface.

{{index "exports object"}}

Because the loader wraps the module code in a function, modules
automatically get their own local scope. All they have to
do is call `require` to access their dependencies and put their
interface in the object bound to `exports`.

{{index "formatDate module", "Date class", "ordinal package", "date-names package"}}

This example module provides a date-formatting function. It uses two
((package))s from NPM—`ordinal` to convert numbers to strings like
`"1st"` and `"2nd"`, and `date-names` to get the English names for
weekdays and months. It exports a single function, `formatDate`, which
takes a `Date` object and a ((template)) string.

The template string may contain codes that direct the format, such as
`YYYY` for the full year and `Do` for the ordinal day of the month.
You could give it a string like `"MMMM Do YYYY"` to get output like
"November 22nd 2017".

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

The interface of `ordinal` is a single function, whereas `date-names`
exports an object containing multiple things—`days` and `months` are
arrays of names. Destructuring is very convenient when creating
bindings for imported interfaces.

The module adds its interface function to `exports` so that modules
that depend on it get access to it. We could use the module like this:

```
const {formatDate} = require("./format-date");

console.log(formatDate(new Date(2017, 9, 13),
                       "dddd the Do"));
// → Friday the 13th
```

{{index "require function", "CommonJS modules", "readFile function"}}

{{id require}}

We can define `require`, in its most minimal form, like this:

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

In this code, `readFile` is a made-up function that reads a file and
returns its contents as a string. Standard JavaScript provides no such
functionality—but different JavaScript environments, such as the
browser and Node.js, provide their own ways of accessing files.
The example just pretends that `readFile` exists.

{{index cache, "Function constructor"}}

To avoid loading the same module multiple times, `require` keeps a
store (cache) of already loaded modules. When called, it first checks
if the requested module has been loaded and, if not, loads it. This
involves reading the module's code, wrapping it in a function, and
calling it.

{{index "ordinal package", "exports object", "module object", [interface, module]}}

The interface of the `ordinal` package we saw before is not an
object but a function. A quirk of the CommonJS modules is that,
though the module system will create an empty interface object for you
(bound to `exports`), you can replace that with any value by
overwriting `module.exports`. This is done by many modules to export a
single value instead of an interface object.

By defining `require`, `exports`, and `module` as ((parameter))s for
the generated wrapper function (and passing the appropriate values
when calling it), the loader makes sure that these bindings are
available in the module's ((scope)).

{{index resolution, "relative path"}}

The way the string given to `require` is translated to an actual
filename or web address differs in different systems. When it starts with
`"./"` or `"../"`, it is generally interpreted as relative to the
current module's filename. So `"./format-date"` would be the file
named `format-date.js` in the same directory.

When the name isn't relative, Node.js will look for an installed
package by that name. In the example code in this chapter, we'll
interpret such names as referring to NPM packages. We'll go into more
detail on how to install and use NPM modules in [Chapter ?](node).

{{id modules_ini}}

{{index "ini package"}}

Now, instead of writing our own INI file parser, we can use one from
((NPM)).

```
const {parse} = require("ini");

console.log(parse("x = 10\ny = 20"));
// → {x: "10", y: "20"}
```

## ECMAScript modules

((CommonJS modules)) work quite well and, in combination with NPM,
have allowed the JavaScript community to start sharing code on a large
scale.

{{index "exports object", linter}}

But they remain a bit of a duct-tape ((hack)). The ((notation)) is
slightly awkward—the things you add to `exports` are not available in
the local ((scope)), for example. And because `require` is a normal
function call taking any kind of argument, not just a string literal,
it can be hard to determine the dependencies of a module without
running its code.

{{index "import keyword", dependency, "ES modules"}}

{{id es}}

This is why the JavaScript standard from 2015 introduces its own,
different module system. It is usually called _((ES modules))_, where
_ES_ stands for ((ECMAScript)). The main concepts of dependencies and
interfaces remain the same, but the details differ. For one thing, the
notation is now integrated into the language. Instead of calling a
function to access a dependency, you use a special `import` keyword.

```
import ordinal from "ordinal";
import {days, months} from "date-names";

export function formatDate(date, format) { /* ... */ }
```

{{index "export keyword", "formatDate module"}}

Similarly, the `export` keyword is used to export things. It may
appear in front of a function, class, or binding definition (`let`,
`const`, or `var`).

{{index [binding, exported]}}

An ES module's interface is not a single value but a set of named
bindings. The preceding module binds `formatDate` to a function. When
you import from another module, you import the _binding_, not the
value, which means an exporting module may change the value of the
binding at any time, and the modules that import it will see its new
value.

{{index "default export"}}

When there is a binding named `default`, it is treated as the module's
main exported value. If you import a module like `ordinal` in the
example, without braces around the binding name, you get its `default`
binding. Such modules can still export other bindings under different
names alongside their `default` export.

To create a default export, you write `export default` before an
expression, a function declaration, or a class declaration.

```
export default ["Winter", "Spring", "Summer", "Autumn"];
```

It is possible to rename imported bindings using the word `as`.

```
import {days as dayNames} from "date-names";

console.log(dayNames.length);
// → 7
```

Another important difference is that ES module imports happen before
a module's script starts running. That means `import` declarations
may not appear inside functions or blocks, and the names of
dependencies must be quoted strings, not arbitrary expressions.

At the time of writing, the JavaScript community is in the process of
adopting this module style. But it has been a slow process. It took
a few years, after the format was specified, for browsers and Node.js
to start supporting it. And though they mostly support it now, this
support still has issues, and the discussion on how such modules
should be distributed through ((NPM)) is still ongoing.

Many projects are written using ES modules and then automatically
converted to some other format when published. We are in a
transitional period in which two different module systems are used
side by side, and it is useful to be able to read and write code in
either of them.

## Building and bundling

{{index compilation, "type checking"}}

In fact, many JavaScript projects aren't even, technically, written in
JavaScript. There are extensions, such as the type checking
((dialect)) mentioned in [Chapter ?](error#typing), that are widely
used. People also often start using planned extensions to the language
long before they have been added to the platforms that actually run
JavaScript.

To make this possible, they _compile_ their code, translating it from
their chosen JavaScript dialect to plain old JavaScript—or even to a
past version of JavaScript—so that old ((browsers)) can run it.

{{index latency, performance, [file, access], [network, speed]}}

Including a modular program that consists of 200 different
files in a ((web page)) produces its own problems. If fetching a
single file over the network takes 50 milliseconds, loading
the whole program takes 10 seconds, or maybe half that if you can
load several files simultaneously. That's a lot of wasted time.
Because fetching a single big file tends to be faster than fetching a
lot of tiny ones, web programmers have started using tools that roll
their programs (which they painstakingly split into modules) back into
a single big file before they publish it to the Web. Such tools are
called _((bundler))s_.

{{index "file size"}}

And we can go further. Apart from the number of files, the _size_ of
the files also determines how fast they can be transferred over the
network. Thus, the JavaScript community has invented _((minifier))s_.
These are tools that take a JavaScript program and make it smaller by
automatically removing comments and whitespace, renaming bindings, and
replacing pieces of code with equivalent code that take up less space.

{{index pipeline, tool}}

So it is not uncommon for the code that you find in an NPM package or
that runs on a web page to have gone through _multiple_ stages of
transformation—converted from modern JavaScript to historic
JavaScript, from ES module format to CommonJS, bundled, and minified.
We won't go into the details of these tools in this book since they
tend to be boring and change rapidly. Just be aware that the
JavaScript code you run is often not the code as it was written.

## Module design

{{index [module, design], [interface, module], [code, "structure of"]}}

Structuring programs is one of the subtler aspects of programming. Any
nontrivial piece of functionality can be modeled in various ways.

Good program design is subjective—there are trade-offs involved and
matters of taste. The best way to learn the value of well-structured
design is to read or work on a lot of programs and notice what works
and what doesn't. Don't assume that a painful mess is "just the way it
is". You can improve the structure of almost everything by putting
more thought into it.

{{index [interface, module]}}

One aspect of module design is ease of use. If you are designing
something that is intended to be used by multiple people—or even by
yourself, in three months when you no longer remember the specifics of
what you did—it is helpful if your interface is simple and
predictable.

{{index "ini package", JSON}}

That may mean following existing conventions. A good example is the
`ini` package. This module imitates the standard `JSON` object by
providing `parse` and `stringify` (to write an INI file) functions,
and, like `JSON`, converts between strings and plain objects. So the
interface is small and familiar, and after you've worked with it once,
you're likely to remember how to use it.

{{index "side effect", "hard disk", composability}}

Even if there's no standard function or widely used package to
imitate, you can keep your modules predictable by using simple ((data
structure))s and doing a single, focused thing. Many of the INI-file
parsing modules on NPM provide a function that directly reads such a
file from the hard disk and parses it, for example. This makes it
impossible to use such modules in the browser, where we don't have
direct file system access, and adds complexity that would have been
better addressed by _composing_ the module with some file-reading
function.

{{index "pure function"}}

This points to another helpful aspect of module design—the ease with
which something can be composed with other code. Focused modules that
compute values are applicable in a wider range of programs than bigger
modules that perform complicated actions with side effects. An INI
file reader that insists on reading the file from disk is useless in a
scenario where the file's content comes from some other source.

{{index "object-oriented programming"}}

Relatedly, stateful objects are sometimes useful or even necessary,
but if something can be done with a function, use a function. Several
of the INI file readers on NPM provide an interface style that
requires you to first create an object, then load the file into your
object, and finally use specialized methods to get at the results.
This type of thing is common in the object-oriented tradition, and
it's terrible. Instead of making a single function call and moving on,
you have to perform the ritual of moving your object through various
states. And because the data is now wrapped in a specialized object
type, all code that interacts with it has to know about that type,
creating unnecessary interdependencies.

Often defining new data structures can't be avoided—only a few
basic ones are provided by the language standard, and many types of
data have to be more complex than an array or a map. But when an
array suffices, use an array.

An example of a slightly more complex data structure is the graph from
[Chapter ?](robot). There is no single obvious way to represent a
((graph)) in JavaScript. In that chapter, we used an object whose
properties hold arrays of strings—the other nodes reachable from that
node.

There are several different pathfinding packages on ((NPM)), but none
of them uses this graph format. They usually allow the graph's edges to
have a weight, which is the cost or distance associated with it. That isn't
possible in our representation.

{{index "Dijkstra, Edsger", pathfinding, "Dijkstra's algorithm", "dijkstrajs package"}}

For example, there's the `dijkstrajs` package. A well-known approach
to pathfinding, quite similar to our `findRoute` function, is called
_Dijkstra's algorithm_, after Edsger Dijkstra, who first wrote it
down. The `js` suffix is often added to package names to indicate the
fact that they are written in JavaScript. This `dijkstrajs` package
uses a graph format similar to ours, but instead of arrays, it uses
objects whose property values are numbers—the weights of the edges.

So if we wanted to use that package, we'd have to make sure that our
graph was stored in the format it expects. All edges get the same
weight since our simplified model treats each road as having the
same cost (one turn).

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

This can be a barrier to composition—when various packages are using
different data structures to describe similar things, combining them
is difficult. Therefore, if you want to design for composability,
find out what ((data structure))s other people are using and, when
possible, follow their example.

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
