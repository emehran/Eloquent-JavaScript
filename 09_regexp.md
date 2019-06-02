# عبارات باقاعده

{{quote {author: "Jamie Zawinski", chapter: true}

Some people, when confronted with a problem, think 'I know, I'll use
regular expressions.' Now they have two problems.

quote}}

{{index "Zawinski, Jamie"}}

{{if interactive

{{quote {author: "Master Yuan-Ma", title: "The Book of Programming", chapter: true}

Yuan-Ma said, 'When you cut against the grain of the wood, much
strength is needed. When you program against the grain of the problem,
much code is needed.'

quote}}

if}}

{{figure {url: "img/chapter_picture_9.jpg", alt: "A railroad diagram", chapter: "square-framed"}}}

{{index evolution, adoption, integration}}

ابزارها و تکنیک‌های برنامه نویسی در طول زمان به شکلی نامنظم و تکاملی حفظ می‌شوند و گسترش می یابند. این‌طور نیست که همیشه آن‌هایی که درخشان یا  خوب هستند برنده شوند؛ بلکه تکنیک‌ها و ابزارهایی باقی‌ می‌مانند که در یک حوزه‌ی مناسب به اندازه‌ی کافی خوب عمل می کنند یا این ویژگی را دارند که با تکنولوژی موفق دیگری به خوبی یکپارچه و تلفیق می شوند.

{{index "domain-specific language"}}

در این فصل، در باره‌ی یکی از این ابزارهای موفق، _((عبارات باقاعده))_، صحبت خواهم کرد. عبارات باقاعده روشی برای توصیف _((الگوها))_ در داده‌های متنی (رشته‌ای) می‌باشند. این عبارات، زبانی کوچک و مجزا را تشکیل می دهند که بخشی از زبان جاوااسکریپت و خیلی زبان‌ها و سیستم های دیگر محسوب می شوند.

{{index [interface, design]}}

عبارات باقاعده، به طور همزمان هم خیلی بی‌قواره و هم فوق‌العاده کاربردی هستند. قواعد دستوری آن‌ها رمزگونه و رابط برنامه‌نویسی آن ها در جاوااسکریپت کمی نچسب است. اما ابزار بسیار قدرتمندی برای پردازش و وارسی رشته‌ها محسوب می شوند. درک صحیح عبارات باقاعده، شما را به برنامه‌نویس موثر‌تری تبدیل می کند.

## ایجاد عبارات باقاعده

{{index ["regular expression", creation], "RegExp class", "literal expression", "slash character"}}

یک عبارت باقاعده یک نوع شیء است. می توان آن را هم با سازنده‌ی `RegExp` و هم به طور مستقیم با قرار دادن یک الگو بین دو کاراکتر اسلش (`/`) ایجاد نمود.

```
let re1 = new RegExp("abc");
let re2 = /abc/;
```
هر دوی عبارت‌های باقاعده‌ی بالا نمایانگر یک ((الگو)) می باشند: کاراکتر _a_ که بعد از آن _b_ و بعد _c_  می آید.

{{index ["backslash character", "in regular expressions"], "RegExp class"}}

زمانی که از سازنده‌ی `RegExp` استفاده می شود، الگو به صورت رشته‌ی معمولی نوشته می شود؛ بنابراین قوانین معمول برای کاراکتر بک‌اسلش برقرار است.

{{index ["regular expression", escaping], [escaping, "in regexps"], "slash character"}}

در روش دوم که در آن الگو بین دو کاراکتر اسلش ظاهر می شود، تفسیر بک اسلش کمی متفاوت است. اول اینکه، به دلیل اینکه کاراکتر اسلش نشان دهنده پایان الگو است، بایستی یک بک اسلش را قبل از اسلشی که می خواهیم به عنوان بخشی از الگو تفسیر شود قرار دهیم. افزون بر آن، بک اسلش‌هایی که بخشی از کدکاراکترهای خاص (مانند <bdo>`\n`</bdo>) محسوب نمی شوند، بر خلاف حالت رشته‌ای، حفظ شده و باعث تغییر در معنای الگو خواهند شد. بعضی کاراکترها مثل علامت سوال یا مثبت، معانی خاصی در عبارات باقاعده دارند و اگر قرار است نمایانگر کاراکتر خودشان باشند، باید قبلشان یک بک اسلش قرار داده شود.


```
let eighteenPlus = /eighteen\+/;
```

## آزمایش تطبیق الگو

{{index matching, "test method", ["regular expression", methods]}}

اشیاء عبارات باقاعده دارای تعدادی متد می باشند. ساده‌ترین آن ها متد `test` است. اگر به این متد یک رشته ارسال کنید، با برگرداندن یک مقدار بولی، به شما خواهد گفت که آیا در رشته‌ی داده شده نمونه‌ای مطابق الگوی عبارت باقاعده، وجود دارد یا خیر.

```
console.log(/abc/.test("abcde"));
// → true
console.log(/abc/.test("abxde"));
// → false
```

{{index pattern}}

اگر در ((عبارات باقاعده)) هیچ کاراکتر خاصی استفاده نشود، آن عبارت معادل همان دنباله‌ی کاراکترها می باشد. اگر _abc_ در هر جای رشته‌ای که مورد آزمایش قرار داده ایم قرار گرفته باشد ( نه فقط در شروع رشته)، متد `test` مقدار `true` را تولید می کند.


## مجموعه‌های کاراکتر

{{index "regular expression", "indexOf method"}}

فهمیدن اینکه آیا یک رشته حاوی _abc_ هست یا خیر را می توان به خوبی با متد  `indexOf`  نیز انجام داد. عبارات باقاعده به ما امکان تولید ((الگوهای)) پیچیده‌تری را می دهند.

فرض کنید قصد داریم همه‌ ((اعداد)) را شناسایی کنیم. در یک عبارت باقاعده، قرار دادن یک ((مجموعه‌)) کاراکتر درون براکت باعث می شود که آن بخش از عبارت با هر کاراکتری که بین براکت‌ها آمده است تطبیق یابد.

هر دوی عبارت‌های زیر همه‌ی رشته‌هایی که دارای رقم هستند را شامل می شود:

```
console.log(/[0123456789]/.test("in 1992"));
// → true
console.log(/[0-9]/.test("in 1992"));
// → true
```

{{index "hyphen character"}}

برای مشخص کرد یک بازه از کاراکترها می توان درون براکت‌ها از یک کاراکتر (‍`-`) بین دو کاراکتر استفاده کرد که ترتیب کاراکترها توسط کد یونیکد آن‌ها مشخص می شود. کاراکترهای ۰ تا ۹ کنار هم و در بازه‌ی یونیکد (کدهای 48 تا 57) قرار دارند بنابراین <bdo>`[0-9]`</bdo> همه‌ی آن ها را پوشش داده و هر رقمی را شامل می شود.

{{index [whitespace, matching], "alphanumeric character", "period character"}}

برای بعضی از گروه‌های کاراکتری روش کوتاه‌تری هم از پیش تعریف شده است. اعداد یکی از آن ها هستند: مثلا <bdo>`\d`</bdo> معنایی مشابه <bdo>`[0-9]`</bdo> دارد.

{{index "newline character", [whitespace, matching]}}

{{table {cols: [1, 5]}}}

| `\d`    | هر کاراکتر عددی
| `\w`    | یک کاراکتر از نوع عدد یا حرف الفبا (“کاراکتر کلمه”)
| `\s`    | همه‌ی کاراکترهای فضای‌خالی ( فاصله، تب، خط جدید، و مشابه آن ها)
| `\D`    | کاراکتری که از نوع عدد _نباشد_
| `\W`    | کاراکتری که عدد و حرف الفبا نباشد
| `\S`    | کاراکتری که فضای خالی محسوب نشود
| `.`     | همه‌ی کاراکترها به جز کاراکتر خط جدید

بنابراین می توانید فرمت ((تاریخ)) و ((زمانی)) شبیه <bdo>01-30-2003
15:20</bdo> را با عبارت زیر شناسایی کنید:

```
let dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/;
console.log(dateTime.test("01-30-2003 15:20"));
// → true
console.log(dateTime.test("30-jan-2003 15:20"));
// → false
```

{{index ["backslash character", "in regular expressions"]}}

ظاهر عبارت بالا خیلی بی‌قواره است، درست است؟ نیمی از آن بک‌اسلش است که الگو را بیش از حد شلوغ کرده و تشخیص معنای آن را سخت نموده‌ است. در [ادامه](regexp#date_regexp_counted) با نسخه‌ای از آن که کمی بهبود یافته است آشنا خواهیم شد.

{{index [escaping, "in regexps"], "regular expression", set}}

این کدهای بک‌اسلش را همچنین می توان درون براکت استفاده کرد. به عنوان مثال، <bdo>`[\d.]`</bdo> به معنای یک رقم یا یک کاراکتر نقطه است. اما خود نقطه وقتی داخل براکت قرار می گیرد معنای خاصش را از دست می دهد. این قضیه برای دیگر کاراکتر های خاص مثل `+` هم برقرار است.

{{index "square brackets", inversion, "caret character"}}

برای _معکوس_ کردن یک مجموعه‌ی کاراکتر – به این معنا که شما قصد دارید هر کاراکتری _بجز_ آنهایی که در مجموعه مشخص شده اند را بیان کنید – می توانید از یک کاراکتر (`^`) بعد از براکت شروع بازه استفاده کنید.

```
let notBinary = /[^01]/;
console.log(notBinary.test("1100100010100110"));
// → false
console.log(notBinary.test("1100100010200110"));
// → true
```

## تکرار بخش‌هایی از یک الگو

{{index ["regular expression", repetition]}}

می دانیم که چگونه یک عدد یا رقم را شناسایی کنیم.  چه باید کرد اگر بخواهیم که یک عدد کامل – دنباله‌ای از یک یا بیشتر رقم - را هدف قرار بدهیم؟

{{index "plus character", repetition, "+ operator"}}

زمانی که از یک علامت مثبت (`+`) را بعد از چیزی در یک عبارت باقاعده قرار می‌دهید، این علامت نشان می دهد که آن عنصر ممکن است یک بار یا بیشتر تکرار شود. بنابراین ، <bdo> `/\d+/`</bdo> به معنای مطابقت عبارت با تعداد یک یا بیشتر از کاراکترهای عددی خواهد بود.

```
console.log(/'\d+'/.test("'123'"));
// → true
console.log(/'\d+'/.test("''"));
// → false
console.log(/'\d*'/.test("'123'"));
// → true
console.log(/'\d*'/.test("''"));
// → true
```

{{index "* operator", asterisk}}

کاراکتر ستاره (`*`) معنای مشابهی دارد با این تفاوت که به الگو اجازه می دهد تا صفر بار تکرار (نبودن کاراکتر) را هم شامل شود. اگر بعد از چیزی کاراکتر ستاره قرار گیرد باعث می شود که الگو همیشه چیزی برای مطابقت پیدا کند - در صورتی که نتواند متنی برای مطابقت پیدا کند، با نبود آن عنصر مطابقت خواهد داد.

{{index "British English", "American English", "question mark"}}

استفاده از علامت سوال (?) در یک الگو به معنای _((اختیاری))_ بودن است، یعنی ممکن است که آن عنصر نباشد یا یک بار حاضر باشد. در مثال پیش رو، کاراکتر _u_ اختیاری است و می تواند باشد و در صورت نبودن هم الگو صدق خواهد کرد.

```
let neighbor = /neighbou?r/;
console.log(neighbor.test("neighbour"));
// → true
console.log(neighbor.test("neighbor"));
// → true
```

{{index repetition, [braces, "in regular expression"]}}

برای مشخص کردن این موضوع که یک الگو باید به تعداد دقیقی رخ دهد، می توانید از
کروشه استفاده کنید؛ به عنوان مثال، قرار دادن `{4}` بعد از یک عنصر، باعث می‌شود که الگو انتظار داشته باشد آن عنصر دقیقا 4 مرتبه رخ داده باشد. همچنین می توان یک بازه را نیز مشخص نمود:‌ <bdo>`{2,4}`</bdo> به این معنا است که این عنصر باید حداقل دو مرتبه و حداکثر چهار مرتبه رخ دهد.

{{id date_regexp_counted}}

اینجا نسخه‌ی دیگر از الگوی تشخیص تاریخ و زمان را داریم که امکان تشخیص روز، ماه و ساعت به هر دو فرمت تک رقمی و دو رقمی را دارد. همچنین درک این الگو کمی راحت‌تر از الگوی پیشین است.

```
let dateTime = /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/;
console.log(dateTime.test("1-30-2003 8:45"));
// → true
```

همچنین می توانید بازه‌هایی که انتهایی باز دارند را نیز مشخص کنید. این کار با حذف رقم پس از ویرگول انجام می شود. بنابراین، <bdo>`{5,}`</bdo> به معنای پنج یا بیشتر می باشد.

## دسته‌بندی زیرعبارات

{{index ["regular expression", grouping], grouping, [parentheses, "in regular expressions"]}}

برای استفاده از یک عملگر مانند `*` یا `+` روی بیش از یک عنصر در آنِ واحد، باید از پرانتز استفاده کنید.
از دید عملگرهایی که بعد از عبارت‌های داخل پرانتز قرار می‌گیرند، هر عبارت محصور بین پرانتز به عنوان یک عنصر در نظر گرفته می شود.

```
let cartoonCrying = /boo+(hoo+)+/i;
console.log(cartoonCrying.test("Boohoooohoohooo"));
// → true
```

{{index crying}}

کاراکترهای `+` اول و دوم فقط به _o_ دوم از _boo_ و _hoo_ اعمال می شوند. کاراکتر `+` سوم به کل گروه <bdo>`(hoo+)`</bdo> اعمال می شود و یک یا بیش از یک بار تکرار آن الگو را شامل می‌شود.

{{index "case sensitivity", capitalization, ["regular expression", flags]}}

کاراکتر `i` که در انتهای عبارت مثال آمده است باعث می شود که عبارت باقاعده به بزرگی و کوچکی حروف حساس نباشد، یعنی کاراکتر _B_ بزرگ هم در رشته‌ی ورودی تطبیق خواهد خورد، با وجود اینکه الگو خودش به حروف کوچک نوشته شده است.


## تطبیق‌ها و گروه‌ها

{{index ["regular expression", grouping], "exec method", [array, "RegExp match"]}}

متد `test` ساده ترین راهی است که برای تطبیق یک عبارت باقاعده استفاده می شود. این متد فقط تطبیق و عدم تطبیق عبارت را مشخص می کند و دیگر هیچ. عبارات باقاعده همچنین متدی به نام `exec` (به معنای اجرا) دارند که در صورت نبود تطبیق، مقدار `null` را بر‌می گرداند و در صورت وجود تطبیق، شیئی شامل اطلاعاتی راجع به آن تولید می کند.

```
let match = /\d+/.exec("one two 100");
console.log(match);
// → ["100"]
console.log(match.index);
// → 8
```

{{index "index property", [string, indexing]}}

شیءای که از یک متد `exec` برگردانده می شود خاصیتی به نام `index` دارد که نقطه شروع تطبیق پیدا شده را در رشته به ما می نشان می دهد. علاوه بر آن، این شیء شبیه به ( و در واقع یک ) آرایه‌ای از رشته‌ها است، که عنصر اولش رشته‌ای است که با الگو مطابقت داشته است – در مثال قبل ، دنباله‌ای از ((اعداد)) که به دنبال آن بودیم.

{{index [string, methods], "match method"}}

مقدارهای رشتهای متدی به نام `match` دارند که به شکل مشابهی عمل می کند.

```
console.log("one two 100".match(/\d+/));
// → ["100"]
```

{{index grouping, "capture group", "exec method"}}

زمانی که یک عبارت باقاعده شامل زیرعبارتهایی باشد که با پرانتز گروه‌بندی شده اند، متن‌هایی که با آن گروه‌ها مطابقت دارند نیز درون یک آرایه نمایش داده خواهد شد. تطبیق کامل همیشه در همان عنصر اول است. عنصر بعدی آرایه متعلق به بخشی است که توسط اولین گروه تطبیق یافته است (گروهی که پرانتز شروعش در عبارت اول آمده است)، سپس گروه دوم و الی آخر.

```
let quotedText = /'([^']*)'/;
console.log(quotedText.exec("she said 'hello'"));
// → ["'hello'", "hello"]
```

{{index "capture group"}}

زمانی که برای یک گروه تطبیقی در رشته پیدا نمی شود (به عنوان مثال، زمانی که بعد از گروه علامت سوال قرار گرفته باشد) موقعیت آن در آرایه‌ی خروجی به صورت `undefined` خواهد بود. به طور مشابه، اگر یک گروه چندین تطبیق داشته باشد، فقط آخرین آن‌ها در آرایه قرار خواهد گرفت.

```
console.log(/bad(ly)?/.exec("bad"));
// → ["bad", undefined]
console.log(/(\d)+/.exec("123"));
// → ["123", "3"]
```

{{index "exec method", ["regular expression", methods], extraction}}

از قابلیت گروه‌ها می توان برای استخراج قسمت‌های یک رشته استفاده کرد. به عنوان مثال، زمانی که فقط بودن یک تاریخ در یک رشته برای ما مهم نیست و قصد داریم تا آن را از دل آن استخراج کرده و شیئی حاوی آن بسازیم، می
توانیم با استفاده از پرانتز در الگوی ارقام، به طور مستقیم آن را در نتیجه‌ی `exec` مجزا کنیم.

اما ابتدا، یک فاصله‌ی کوتاه بگیریم و کمی در رابطه‌با راه از پیش تعریف شده برای نمایش مقادیر زمان و تاریخ در جاوااسکریپت صحبت کنیم.

## The Date class

{{index constructor, "Date class"}}

JavaScript has a standard class for representing ((date))s—or, rather,
points in ((time)). It is called `Date`. If you simply create a date
object using `new`, you get the current date and time.

```{test: no}
console.log(new Date());
// → Mon Nov 13 2017 16:19:11 GMT+0100 (CET)
```

{{index "Date class"}}

You can also create an object for a specific time.

```
console.log(new Date(2009, 11, 9));
// → Wed Dec 09 2009 00:00:00 GMT+0100 (CET)
console.log(new Date(2009, 11, 9, 12, 59, 59, 999));
// → Wed Dec 09 2009 12:59:59 GMT+0100 (CET)
```

{{index "zero-based counting", [interface, design]}}

JavaScript uses a convention where month numbers start at zero (so
December is 11), yet day numbers start at one. This is confusing and
silly. Be careful.

The last four arguments (hours, minutes, seconds, and milliseconds)
are optional and taken to be zero when not given.

{{index "getTime method"}}

Timestamps are stored as the number of milliseconds since the start of
1970, in the UTC ((time zone)). This follows a convention set by
"((Unix time))", which was invented around that time. You can use
negative numbers for times before 1970. The `getTime` method on a date
object returns this number. It is big, as you can imagine.

```
console.log(new Date(2013, 11, 19).getTime());
// → 1387407600000
console.log(new Date(1387407600000));
// → Thu Dec 19 2013 00:00:00 GMT+0100 (CET)
```

{{index "Date.now function", "Date class"}}

If you give the `Date` constructor a single argument, that argument is
treated as such a millisecond count. You can get the current
millisecond count by creating a new `Date` object and calling
`getTime` on it or by calling the `Date.now` function.

{{index "getFullYear method", "getMonth method", "getDate method", "getHours method", "getMinutes method", "getSeconds method", "getYear method"}}

Date objects provide methods such as `getFullYear`, `getMonth`,
`getDate`, `getHours`, `getMinutes`, and `getSeconds` to extract their
components. Besides `getFullYear` there's also `getYear`, which gives
you the year minus 1900 (`98` or `119`) and is mostly useless.

{{index "capture group", "getDate method", [parentheses, "in regular expressions"]}}

Putting parentheses around the parts of the expression that we are
interested in, we can now create a date object from a string.

```
function getDate(string) {
  let [_, month, day, year] =
    /(\d{1,2})-(\d{1,2})-(\d{4})/.exec(string);
  return new Date(year, month - 1, day);
}
console.log(getDate("1-30-2003"));
// → Thu Jan 30 2003 00:00:00 GMT+0100 (CET)
```

{{index destructuring, "underscore character"}}

The `_` (underscore) binding is ignored and used only to skip the
full match element in the array returned by `exec`.

## Word and string boundaries

{{index matching, ["regular expression", boundary]}}

Unfortunately, `getDate` will also happily extract the nonsensical
date 00-1-3000 from the string `"100-1-30000"`. A match may happen
anywhere in the string, so in this case, it'll just start at the
second character and end at the second-to-last character.

{{index boundary, "caret character", "dollar sign"}}

If we want to enforce that the match must span the whole string, we
can add the markers `^` and `$`. The caret matches the start of the
input string, whereas the dollar sign matches the end. So, `/^\d+$/`
matches a string consisting entirely of one or more digits, `/^!/`
matches any string that starts with an exclamation mark, and `/x^/`
does not match any string (there cannot be an _x_ before the start of
the string).

{{index "word boundary", "word character"}}

If, on the other hand, we just want to make sure the date starts and
ends on a word boundary, we can use the marker `\b`. A word boundary
can be the start or end of the string or any point in the string that
has a word character (as in `\w`) on one side and a nonword character
on the other.

```
console.log(/cat/.test("concatenate"));
// → true
console.log(/\bcat\b/.test("concatenate"));
// → false
```

{{index matching}}

Note that a boundary marker doesn't match an actual character. It just
enforces that the regular expression matches only when a certain
condition holds at the place where it appears in the pattern.

## Choice patterns

{{index branching, ["regular expression", alternatives], "farm example"}}

Say we want to know whether a piece of text contains not only a number
but a number followed by one of the words _pig_, _cow_, or _chicken_,
or any of their plural forms.

We could write three regular expressions and test them in turn, but
there is a nicer way. The ((pipe character)) (`|`) denotes a
((choice)) between the pattern to its left and the pattern to its
right. So I can say this:

```
let animalCount = /\b\d+ (pig|cow|chicken)s?\b/;
console.log(animalCount.test("15 pigs"));
// → true
console.log(animalCount.test("15 pigchickens"));
// → false
```

{{index [parentheses, "in regular expressions"]}}

Parentheses can be used to limit the part of the pattern that the pipe
operator applies to, and you can put multiple such operators next to
each other to express a choice between more than two alternatives.

## The mechanics of matching

{{index ["regular expression", matching], [matching, algorithm], "search problem"}}

Conceptually, when you use `exec` or `test`, the regular expression
engine looks for a match in your string by trying to match the
expression first from the start of the string, then from the second
character, and so on, until it finds a match or reaches the end of the
string. It'll either return the first match that can be found or fail
to find any match at all.

{{index ["regular expression", matching], [matching, algorithm]}}

To do the actual matching, the engine treats a regular expression
something like a ((flow diagram)). This is the diagram for the
livestock expression in the previous example:

{{figure {url: "img/re_pigchickens.svg", alt: "Visualization of /\\b\\d+ (pig|cow|chicken)s?\\b/"}}}

{{index traversal}}

Our expression matches if we can find a path from the left side of the
diagram to the right side. We keep a current position in the string,
and every time we move through a box, we verify that the part of the
string after our current position matches that box.

So if we try to match `"the 3 pigs"` from position 4, our progress
through the flow chart would look like this:

 - At position 4, there is a word ((boundary)), so we can move past
   the first box.

 - Still at position 4, we find a digit, so we can also move past the
   second box.

 - At position 5, one path loops back to before the second (digit)
   box, while the other moves forward through the box that holds a
   single space character. There is a space here, not a digit, so we
   must take the second path.

 - We are now at position 6 (the start of _pigs_) and at the three-way
   branch in the diagram. We don't see _cow_ or _chicken_ here, but we
   do see _pig_, so we take that branch.

 - At position 9, after the three-way branch, one path skips the _s_
   box and goes straight to the final word boundary, while the other
   path matches an _s_. There is an _s_ character here, not a word
   boundary, so we go through the _s_ box.

 - We're at position 10 (the end of the string) and can match only a
   word ((boundary)). The end of a string counts as a word boundary,
   so we go through the last box and have successfully matched this
   string.

{{id backtracking}}

## Backtracking

{{index ["regular expression", backtracking], "binary number", "decimal number", "hexadecimal number", "flow diagram", [matching, algorithm], backtracking}}

The regular expression `/\b([01]+b|[\da-f]+h|\d+)\b/` matches either a
binary number followed by a _b_, a hexadecimal number (that is, base
16, with the letters _a_ to _f_ standing for the digits 10 to 15)
followed by an _h_, or a regular decimal number with no suffix
character. This is the corresponding diagram:

{{figure {url: "img/re_number.svg", alt: "Visualization of /\\b([01]+b|\\d+|[\\da-f]+h)\\b/"}}}

{{index branching}}

When matching this expression, it will often happen that the top
(binary) branch is entered even though the input does not actually
contain a binary number. When matching the string `"103"`, for
example, it becomes clear only at the 3 that we are in the wrong
branch. The string _does_ match the expression, just not the branch we
are currently in.

{{index backtracking, "search problem"}}

So the matcher _backtracks_. When entering a branch, it remembers its
current position (in this case, at the start of the string, just past
the first boundary box in the diagram) so that it can go back and try
another branch if the current one does not work out. For the string
`"103"`, after encountering the 3 character, it will start trying the
branch for hexadecimal numbers, which fails again because there is no
_h_ after the number. So it tries the decimal number branch. This one
fits, and a match is reported after all.

{{index [matching, algorithm]}}

The matcher stops as soon as it finds a full match. This means that if
multiple branches could potentially match a string, only the first one
(ordered by where the branches appear in the regular expression) is
used.

Backtracking also happens for ((repetition)) operators like + and `*`.
If you match `/^.*x/` against `"abcxe"`, the `.*` part will first try
to consume the whole string. The engine will then realize that it
needs an _x_ to match the pattern. Since there is no _x_ past the end
of the string, the star operator tries to match one character less.
But the matcher doesn't find an _x_ after `abcx` either, so it
backtracks again, matching the star operator to just `abc`. _Now_ it
finds an _x_ where it needs it and reports a successful match from
positions 0 to 4.

{{index performance, complexity}}

It is possible to write regular expressions that will do a _lot_ of
backtracking. This problem occurs when a pattern can match a piece of
input in many different ways. For example, if we get confused while
writing a binary-number regular expression, we might accidentally
write something like `/([01]+)+b/`.

{{figure {url: "img/re_slow.svg", alt: "Visualization of /([01]+)+b/",width: "6cm"}}}

{{index "inner loop", [nesting, "in regexps"]}}

If that tries to match some long series of zeros and ones with no
trailing _b_ character, the matcher first goes through the inner
loop until it runs out of digits. Then it notices there is no _b_, so
it backtracks one position, goes through the outer loop once, and
gives up again, trying to backtrack out of the inner loop once more.
It will continue to try every possible route through these two loops.
This means the amount of work _doubles_ with each additional
character. For even just a few dozen characters, the resulting match
will take practically forever.

## The replace method

{{index "replace method", "regular expression"}}

String values have a `replace` method that can be used to replace
part of the string with another string.

```
console.log("papa".replace("p", "m"));
// → mapa
```

{{index ["regular expression", flags], ["regular expression", global]}}

The first argument can also be a regular expression, in which case the
first match of the regular expression is replaced. When a `g` option
(for _global_) is added to the regular expression, _all_ matches in
the string will be replaced, not just the first.

```
console.log("Borobudur".replace(/[ou]/, "a"));
// → Barobudur
console.log("Borobudur".replace(/[ou]/g, "a"));
// → Barabadar
```

{{index [interface, design], argument}}

It would have been sensible if the choice between replacing one match
or all matches was made through an additional argument to `replace` or
by providing a different method, `replaceAll`. But for some
unfortunate reason, the choice relies on a property of the regular
expression instead.

{{index grouping, "capture group", "dollar sign", "replace method", ["regular expression", grouping]}}

The real power of using regular expressions with `replace` comes from
the fact that we can refer to matched groups in the replacement
string. For example, say we have a big string containing the names of
people, one name per line, in the format `Lastname, Firstname`. If we
want to swap these names and remove the comma to get a `Firstname
Lastname` format, we can use the following code:

```
console.log(
  "Liskov, Barbara\nMcCarthy, John\nWadler, Philip"
    .replace(/(\w+), (\w+)/g, "$2 $1"));
// → Barbara Liskov
//   John McCarthy
//   Philip Wadler
```

The `$1` and `$2` in the replacement string refer to the parenthesized
groups in the pattern. `$1` is replaced by the text that matched
against the first group, `$2` by the second, and so on, up to `$9`.
The whole match can be referred to with `$&`.

{{index [function, "higher-order"], grouping, "capture group"}}

It is possible to pass a function—rather than a string—as the second
argument to `replace`. For each replacement, the function will be
called with the matched groups (as well as the whole match) as
arguments, and its return value will be inserted into the new string.

Here's a small example:

```
let s = "the cia and fbi";
console.log(s.replace(/\b(fbi|cia)\b/g,
            str => str.toUpperCase()));
// → the CIA and FBI
```

Here's a more interesting one:

```
let stock = "1 lemon, 2 cabbages, and 101 eggs";
function minusOne(match, amount, unit) {
  amount = Number(amount) - 1;
  if (amount == 1) { // only one left, remove the 's'
    unit = unit.slice(0, unit.length - 1);
  } else if (amount == 0) {
    amount = "no";
  }
  return amount + " " + unit;
}
console.log(stock.replace(/(\d+) (\w+)/g, minusOne));
// → no lemon, 1 cabbage, and 100 eggs
```

This takes a string, finds all occurrences of a number followed by an
alphanumeric word, and returns a string wherein every such occurrence
is decremented by one.

The `(\d+)` group ends up as the `amount` argument to the function,
and the `(\w+)` group gets bound to `unit`. The function converts
`amount` to a number—which always works since it matched `\d+`—and
makes some adjustments in case there is only one or zero left.

## Greed

{{index greed, "regular expression"}}

It is possible to use `replace` to write a function that removes all
((comment))s from a piece of JavaScript ((code)). Here is a first
attempt:

```{test: wrap}
function stripComments(code) {
  return code.replace(/\/\/.*|\/\*[^]*\*\//g, "");
}
console.log(stripComments("1 + /* 2 */3"));
// → 1 + 3
console.log(stripComments("x = 10;// ten!"));
// → x = 10;
console.log(stripComments("1 /* a */+/* b */ 1"));
// → 1  1
```

{{index "period character", "slash character", "newline character", "empty set", "block comment", "line comment"}}

The part before the _or_ operator matches two slash characters
followed by any number of non-newline characters. The part for
multiline comments is more involved. We use `[^]` (any character that
is not in the empty set of characters) as a way to match any
character. We cannot just use a period here because block comments can
continue on a new line, and the period character does not match
newline characters.

But the output for the last line appears to have gone wrong. Why?

{{index backtracking, greed, "regular expression"}}

The `[^]*` part of the expression, as I described in the section on
backtracking, will first match as much as it can. If that causes the
next part of the pattern to fail, the matcher moves back one character
and tries again from there. In the example, the matcher first tries to
match the whole rest of the string and then moves back from there. It
will find an occurrence of `*/` after going back four characters and
match that. This is not what we wanted—the intention was to match a
single comment, not to go all the way to the end of the code and find
the end of the last block comment.

Because of this behavior, we say the repetition operators (`+`, `*`,
`?`, and `{}`) are _((greed))y_, meaning they match as much as they
can and backtrack from there. If you put a ((question mark)) after
them (`+?`, `*?`, `??`, `{}?`), they become nongreedy and start by
matching as little as possible, matching more only when the remaining
pattern does not fit the smaller match.

And that is exactly what we want in this case. By having the star
match the smallest stretch of characters that brings us to a `*/`, we
consume one block comment and nothing more.

```{test: wrap}
function stripComments(code) {
  return code.replace(/\/\/.*|\/\*[^]*?\*\//g, "");
}
console.log(stripComments("1 /* a */+/* b */ 1"));
// → 1 + 1
```

A lot of ((bug))s in ((regular expression)) programs can be traced to
unintentionally using a greedy operator where a nongreedy one would
work better. When using a ((repetition)) operator, consider the
nongreedy variant first.

## Dynamically creating RegExp objects

{{index ["regular expression", creation], "underscore character", "RegExp class"}}

There are cases where you might not know the exact ((pattern)) you
need to match against when you are writing your code. Say you want to
look for the user's name in a piece of text and enclose it in
underscore characters to make it stand out. Since you will know the
name only once the program is actually running, you can't use the
slash-based notation.

But you can build up a string and use the `RegExp` ((constructor)) on
that. Here's an example:

```
let name = "harry";
let text = "Harry is a suspicious character.";
let regexp = new RegExp("\\b(" + name + ")\\b", "gi");
console.log(text.replace(regexp, "_$1_"));
// → _Harry_ is a suspicious character.
```

{{index ["regular expression", flags], ["backslash character", "in regular expressions"]}}

When creating the `\b` ((boundary)) markers, we have to use two
backslashes because we are writing them in a normal string, not a
slash-enclosed regular expression. The second argument to the `RegExp`
constructor contains the options for the regular expression—in this
case, `"gi"` for global and case insensitive.

But what if the name is `"dea+hl[]rd"` because our user is a ((nerd))y
teenager? That would result in a nonsensical regular expression that
won't actually match the user's name.

{{index ["backslash character", "in regular expressions"], [escaping, "in regexps"], ["regular expression", escaping]}}

To work around this, we can add backslashes before any character that
has a special meaning.

```
let name = "dea+hl[]rd";
let text = "This dea+hl[]rd guy is super annoying.";
let escaped = name.replace(/[\\[.+*?(){|^$]/g, "\\$&");
let regexp = new RegExp("\\b" + escaped + "\\b", "gi");
console.log(text.replace(regexp, "_$&_"));
// → This _dea+hl[]rd_ guy is super annoying.
```

## The search method

{{index ["regular expression", methods], "indexOf method", "search method"}}

The `indexOf` method on strings cannot be called with a regular
expression. But there is another method, `search`, that does expect a
regular expression. Like `indexOf`, it returns the first index on
which the expression was found, or -1 when it wasn't found.

```
console.log("  word".search(/\S/));
// → 2
console.log("    ".search(/\S/));
// → -1
```

Unfortunately, there is no way to indicate that the match should start
at a given offset (like we can with the second argument to `indexOf`),
which would often be useful.

## The lastIndex property

{{index "exec method", "regular expression"}}

The `exec` method similarly does not provide a convenient way to start
searching from a given position in the string. But it does provide an
*in*convenient way.

{{index ["regular expression", matching], matching, "source property", "lastIndex property"}}

Regular expression objects have properties. One such property is
`source`, which contains the string that expression was created from.
Another property is `lastIndex`, which controls, in some limited
circumstances, where the next match will start.

{{index [interface, design], "exec method", ["regular expression", global]}}

Those circumstances are that the regular expression must have the
global (`g`) or sticky (`y`) option enabled, and the match must happen
through the `exec` method. Again, a less confusing solution would have
been to just allow an extra argument to be passed to `exec`, but
confusion is an essential feature of JavaScript's regular expression
interface.

```
let pattern = /y/g;
pattern.lastIndex = 3;
let match = pattern.exec("xyzzy");
console.log(match.index);
// → 4
console.log(pattern.lastIndex);
// → 5
```

{{index "side effect", "lastIndex property"}}

If the match was successful, the call to `exec` automatically updates
the `lastIndex` property to point after the match. If no match was
found, `lastIndex` is set back to zero, which is also the value it has
in a newly constructed regular expression object.

The difference between the global and the sticky options is that, when
sticky is enabled, the match will succeed only if it starts directly
at `lastIndex`, whereas with global, it will search ahead for a
position where a match can start.

```
let global = /abc/g;
console.log(global.exec("xyz abc"));
// → ["abc"]
let sticky = /abc/y;
console.log(sticky.exec("xyz abc"));
// → null
```

{{index bug}}

When using a shared regular expression value for multiple `exec`
calls, these automatic updates to the `lastIndex` property can cause
problems. Your regular expression might be accidentally starting at an
index that was left over from a previous call.

```
let digit = /\d/g;
console.log(digit.exec("here it is: 1"));
// → ["1"]
console.log(digit.exec("and now: 1"));
// → null
```

{{index ["regular expression", global], "match method"}}

Another interesting effect of the global option is that it changes the
way the `match` method on strings works. When called with a global
expression, instead of returning an array similar to that returned by
`exec`, `match` will find _all_ matches of the pattern in the string
and return an array containing the matched strings.

```
console.log("Banana".match(/an/g));
// → ["an", "an"]
```

So be cautious with global regular expressions. The cases where they
are necessary—calls to `replace` and places where you want to
explicitly use `lastIndex`—are typically the only places where you
want to use them.

### Looping over matches

{{index "lastIndex property", "exec method", loop}}

A common thing to do is to scan through all occurrences of a pattern
in a string, in a way that gives us access to the match object in the
loop body. We can do this by using `lastIndex` and `exec`.

```
let input = "A string with 3 numbers in it... 42 and 88.";
let number = /\b\d+\b/g;
let match;
while (match = number.exec(input)) {
  console.log("Found", match[0], "at", match.index);
}
// → Found 3 at 14
//   Found 42 at 33
//   Found 88 at 40
```

{{index "while loop", ["= operator", "as expression"], [binding, "as state"]}}

This makes use of the fact that the value of an ((assignment))
expression (`=`) is the assigned value. So by using `match =
number.exec(input)` as the condition in the `while` statement, we
perform the match at the start of each iteration, save its result in a
binding, and stop looping when no more matches are found.

{{id ini}}
## Parsing an INI file

{{index comment, "file format", "enemies example", "INI file"}}

To conclude the chapter, we'll look at a problem that calls for
((regular expression))s. Imagine we are writing a program to
automatically collect information about our enemies from the
((Internet)). (We will not actually write that program here, just the
part that reads the ((configuration)) file. Sorry.) The configuration
file looks like this:

```{lang: "text/plain"}
searchengine=https://duckduckgo.com/?q=$1
spitefulness=9.7

; comments are preceded by a semicolon...
; each section concerns an individual enemy
[larry]
fullname=Larry Doe
type=kindergarten bully
website=http://www.geocities.com/CapeCanaveral/11451

[davaeorn]
fullname=Davaeorn
type=evil wizard
outputdir=/home/marijn/enemies/davaeorn
```

{{index grammar}}

The exact rules for this format (which is a widely used format,
usually called an _INI_ file) are as follows:

- Blank lines and lines starting with semicolons are ignored.

- Lines wrapped in `[` and `]` start a new ((section)).

- Lines containing an alphanumeric identifier followed by an `=`
  character add a setting to the current section.

- Anything else is invalid.

Our task is to convert a string like this into an object whose
properties hold strings for settings written before the first
section header and subobjects for sections, with those subobjects
holding the section's settings.

{{index "carriage return", "line break", "newline character"}}

Since the format has to be processed ((line)) by line, splitting up
the file into separate lines is a good start. We saw
the `split` method in [Chapter ?](data#split).
Some operating systems, however, use not just a newline character to
separate lines but a carriage return character followed by a newline
(`"\r\n"`). Given that the `split` method also allows a regular
expression as its argument, we can use a regular expression like
`/\r?\n/` to split in a way that allows both `"\n"` and `"\r\n"`
between lines.

```{startCode: true}
function parseINI(string) {
  // Start with an object to hold the top-level fields
  let result = {};
  let section = result;
  string.split(/\r?\n/).forEach(line => {
    let match;
    if (match = line.match(/^(\w+)=(.*)$/)) {
      section[match[1]] = match[2];
    } else if (match = line.match(/^\[(.*)\]$/)) {
      section = result[match[1]] = {};
    } else if (!/^\s*(;.*)?$/.test(line)) {
      throw new Error("Line '" + line + "' is not valid.");
    }
  });
  return result;
}

console.log(parseINI(`
name=Vasilis
[address]
city=Tessaloniki`));
// → {name: "Vasilis", address: {city: "Tessaloniki"}}
```

{{index "parseINI function", parsing}}

The code goes over the file's lines and builds up an object.
Properties at the top are stored directly into that object, whereas
properties found in sections are stored in a separate section object.
The `section` binding points at the object for the current section.

There are two kinds of significant lines—section headers or property
lines. When a line is a regular property, it is stored in the current
section. When it is a section header, a new section object is created,
and `section` is set to point at it.

{{index "caret character", "dollar sign", boundary}}

Note the recurring use of `^` and `$` to make sure the expression
matches the whole line, not just part of it. Leaving these out results
in code that mostly works but behaves strangely for some input, which
can be a difficult bug to track down.

{{index "if keyword", assignment, ["= operator", "as expression"]}}

The pattern `if (match = string.match(...))` is similar to the trick
of using an assignment as the condition for `while`. You often aren't
sure that your call to `match` will succeed, so you can access the
resulting object only inside an `if` statement that tests for this. To
not break the pleasant chain of `else if` forms, we assign the result
of the match to a binding and immediately use that assignment as the
test for the `if` statement.

{{index [parentheses, "in regular expressions"]}}

If a line is not a section header or a property, the function checks
whether it is a comment or an empty line using the expression
`/^\s*(;.*)?$/`. Do you see how it works? The part between the
parentheses will match comments, and the `?` makes sure it also
matches lines containing only whitespace. When a line doesn't match
any of the expected forms, the function throws an exception.

## International characters

{{index internationalization, Unicode, ["regular expression", internationalization]}}

Because of JavaScript's initial simplistic implementation and the fact
that this simplistic approach was later set in stone as ((standard))
behavior, JavaScript's regular expressions are rather dumb about
characters that do not appear in the English language. For example, as
far as JavaScript's regular expressions are concerned, a "((word
character))" is only one of the 26 characters in the Latin alphabet
(uppercase or lowercase), decimal digits, and, for some reason, the
underscore character. Things like _é_ or _β_, which most definitely
are word characters, will not match `\w` (and _will_ match uppercase
`\W`, the nonword category).

{{index [whitespace, matching]}}

By a strange historical accident, `\s` (whitespace) does not have this
problem and matches all characters that the Unicode standard considers
whitespace, including things like the ((nonbreaking space)) and the
((Mongolian vowel separator)).

Another problem is that, by default, regular expressions work on code
units, as discussed in [Chapter ?](higher_order#code_units), not
actual characters. This means characters that are composed of two
code units behave strangely.

```
console.log(/🍎{3}/.test("🍎🍎🍎"));
// → false
console.log(/<.>/.test("<🌹>"));
// → false
console.log(/<.>/u.test("<🌹>"));
// → true
```

The problem is that the 🍎 in the first line is treated as two code
units, and the `{3}` part is applied only to the second one.
Similarly, the dot matches a single code unit, not the two that make
up the rose ((emoji)).

You must add a `u` option (for ((Unicode))) to your regular
expression to make it treat such characters properly. The wrong
behavior remains the default, unfortunately, because changing that
might cause problems for existing code that depends on it.

{{index "character category", [Unicode, property]}}

Though this was only just standardized and is, at the time of writing,
not widely supported yet, it is possible to use `\p` in a regular
expression (that must have the Unicode option enabled) to match all
characters to which the Unicode standard assigns a given property.

```{test: never}
console.log(/\p{Script=Greek}/u.test("α"));
// → true
console.log(/\p{Script=Arabic}/u.test("α"));
// → false
console.log(/\p{Alphabetic}/u.test("α"));
// → true
console.log(/\p{Alphabetic}/u.test("!"));
// → false
```

Unicode defines a number of useful properties, though finding the one
that you need may not always be trivial. You can use the
`\p{Property=Value}` notation to match any character that has the
given value for that property. If the property name is left off, as in
`\p{Name}`, the name is assumed to be either a binary property such as
`Alphabetic` or a category such as `Number`.

{{id summary_regexp}}

## Summary

Regular expressions are objects that represent patterns in strings.
They use their own language to express these patterns.

{{table {cols: [1, 5]}}}

| `/abc/`     | A sequence of characters
| `/[abc]/`   | Any character from a set of characters
| `/[^abc]/`  | Any character _not_ in a set of characters
| `/[0-9]/`   | Any character in a range of characters
| `/x+/`      | One or more occurrences of the pattern `x`
| `/x+?/`     | One or more occurrences, nongreedy
| `/x*/`      | Zero or more occurrences
| `/x?/`      | Zero or one occurrence
| `/x{2,4}/`  | Two to four occurrences
| `/(abc)/`   | A group
| `/a|b|c/`   | Any one of several patterns
| `/\d/`      | Any digit character
| `/\w/`      | An alphanumeric character ("word character")
| `/\s/`      | Any whitespace character
| `/./`       | Any character except newlines
| `/\b/`      | A word boundary
| `/^/`       | Start of input
| `/$/`       | End of input

A regular expression has a method `test` to test whether a given
string matches it. It also has a method `exec` that, when a match is
found, returns an array containing all matched groups. Such an array
has an `index` property that indicates where the match started.

Strings have a `match` method to match them against a regular
expression and a `search` method to search for one, returning only the
starting position of the match. Their `replace` method can replace
matches of a pattern with a replacement string or function.

Regular expressions can have options, which are written after the
closing slash. The `i` option makes the match case insensitive. The
`g` option makes the expression _global_, which, among other things,
causes the `replace` method to replace all instances instead of just
the first. The `y` option makes it sticky, which means that it will
not search ahead and skip part of the string when looking for a match.
The `u` option turns on Unicode mode, which fixes a number of problems
around the handling of characters that take up two code units.

Regular expressions are a sharp ((tool)) with an awkward handle. They
simplify some tasks tremendously but can quickly become unmanageable
when applied to complex problems. Part of knowing how to use them is
resisting the urge to try to shoehorn things that they cannot cleanly
express into them.

## Exercises

{{index debugging, bug}}

It is almost unavoidable that, in the course of working on these
exercises, you will get confused and frustrated by some regular
expression's inexplicable ((behavior)). Sometimes it helps to enter
your expression into an online tool like
[_https://debuggex.com_](https://www.debuggex.com/) to see whether its
visualization corresponds to what you intended and to ((experiment))
with the way it responds to various input strings.

### Regexp golf

{{index "program size", "code golf", "regexp golf (exercise)"}}

_Code golf_ is a term used for the game of trying to express a
particular program in as few characters as possible. Similarly,
_regexp golf_ is the practice of writing as tiny a regular expression
as possible to match a given pattern, and _only_ that pattern.

{{index boundary, matching}}

For each of the following items, write a ((regular expression)) to
test whether any of the given substrings occur in a string. The
regular expression should match only strings containing one of the
substrings described. Do not worry about word boundaries unless
explicitly mentioned. When your expression works, see whether you can
make it any smaller.

 1. _car_ and _cat_
 2. _pop_ and _prop_
 3. _ferret_, _ferry_, and _ferrari_
 4. Any word ending in _ious_
 5. A whitespace character followed by a period, comma, colon, or semicolon
 6. A word longer than six letters
 7. A word without the letter _e_ (or _E_)

Refer to the table in the [chapter summary](regexp#summary_regexp) for
help. Test each solution with a few test strings.

{{if interactive
```
// Fill in the regular expressions

verify(/.../,
       ["my car", "bad cats"],
       ["camper", "high art"]);

verify(/.../,
       ["pop culture", "mad props"],
       ["plop", "prrrop"]);

verify(/.../,
       ["ferret", "ferry", "ferrari"],
       ["ferrum", "transfer A"]);

verify(/.../,
       ["how delicious", "spacious room"],
       ["ruinous", "consciousness"]);

verify(/.../,
       ["bad punctuation ."],
       ["escape the period"]);

verify(/.../,
       ["hottentottententen"],
       ["no", "hotten totten tenten"]);

verify(/.../,
       ["red platypus", "wobbling nest"],
       ["earth bed", "learning ape", "BEET"]);


function verify(regexp, yes, no) {
  // Ignore unfinished exercises
  if (regexp.source == "...") return;
  for (let str of yes) if (!regexp.test(str)) {
    console.log(`Failure to match '${str}'`);
  }
  for (let str of no) if (regexp.test(str)) {
    console.log(`Unexpected match for '${str}'`);
  }
}
```

if}}

### Quoting style

{{index "quoting style (exercise)", "single-quote character", "double-quote character"}}

Imagine you have written a story and used single ((quotation mark))s
throughout to mark pieces of dialogue. Now you want to replace all the
dialogue quotes with double quotes, while keeping the single quotes
used in contractions like _aren't_.

{{index "replace method"}}

Think of a pattern that distinguishes these two
kinds of quote usage and craft a call to the `replace` method that
does the proper replacement.

{{if interactive
```{test: no}
let text = "'I'm the cook,' he said, 'it's my job.'";
// Change this call.
console.log(text.replace(/A/g, "B"));
// → "I'm the cook," he said, "it's my job."
```
if}}

{{hint

{{index "quoting style (exercise)", boundary}}

The most obvious solution is to replace only quotes with a nonword
character on at least one side—something like `/\W'|'\W/`. But you
also have to take the start and end of the line into account.

{{index grouping, "replace method", [parentheses, "in regular expressions"]}}

In addition, you must ensure that the replacement also includes the
characters that were matched by the `\W` pattern so that those are not
dropped. This can be done by wrapping them in parentheses and
including their groups in the replacement string (`$1`, `$2`). Groups
that are not matched will be replaced by nothing.

hint}}

### Numbers again

{{index sign, "fractional number", [syntax, number], minus, "plus character", exponent, "scientific notation", "period character"}}

Write an expression that matches only JavaScript-style ((number))s. It
must support an optional minus _or_ plus sign in front of the number,
the decimal dot, and exponent notation—`5e-3` or `1E10`—again with an
optional sign in front of the exponent. Also note that it is not
necessary for there to be digits in front of or after the dot, but the
number cannot be a dot alone. That is, `.5` and `5.` are valid
JavaScript numbers, but a lone dot _isn't_.

{{if interactive
```{test: no}
// Fill in this regular expression.
let number = /^...$/;

// Tests:
for (let str of ["1", "-1", "+15", "1.55", ".5", "5.",
                 "1.3e2", "1E-4", "1e+12"]) {
  if (!number.test(str)) {
    console.log(`Failed to match '${str}'`);
  }
}
for (let str of ["1a", "+-1", "1.2.3", "1+1", "1e4.5",
                 ".5.", "1f5", "."]) {
  if (number.test(str)) {
    console.log(`Incorrectly accepted '${str}'`);
  }
}
```

if}}

{{hint

{{index ["regular expression", escaping], ["backslash character", "in regular expressions"]}}

First, do not forget the backslash in front of the period.

Matching the optional ((sign)) in front of the ((number)), as well as
in front of the ((exponent)), can be done with `[+\-]?` or `(\+|-|)`
(plus, minus, or nothing).

{{index "pipe character"}}

The more complicated part of the exercise is the problem of matching
both `"5."` and `".5"` without also matching `"."`. For this, a good
solution is to use the `|` operator to separate the two cases—either
one or more digits optionally followed by a dot and zero or more
digits _or_ a dot followed by one or more digits.

{{index exponent, "case sensitivity", ["regular expression", flags]}}

Finally, to make the _e_ case insensitive, either add an `i` option to
the regular expression or use `[eE]`.

hint}}
