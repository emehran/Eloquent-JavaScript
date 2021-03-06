{{meta {docid: values}}}

# مقدارها, انواع داده, و عملگرها

{{quote {author: "استاد Yuan-Ma", title: "کتاب برنامه‌نویسی", chapter: true}
پایین تر از سطح ماشین، برنامه است که حرکت می‌کند و به‌ آسانی‌ منبسط و منقبض می‌شود. الکترون‌ها با هماهنگی تمام، پراکنده و دوباره جمع می‌شوند. اشکال روی مانیتور چیزی جز امواج روی آب نیستند. جوهر اصلی در زیر به صورت نامرئی می‌ماند.

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_1.jpg", alt: "Picture of a sea of bits", chapter: framed}}}

{{index "binary data", data, bit, memory}}

درون دنیای کامپیوتر، فقط داده‌ (data) وجود دارد. شما می‌توانید داده‌ها را بخوانید، تغییر دهید، داده‌های جدیدی ایجاد کنید – اما چیزی به جز داده وجود خارجی ندارد. همه‌ی این داده‌ها به شکل دنباله‌ای بلند از بیت ها ذخیره می‌شوند که اساسا به یکدیگر شباهت دارند.

{{index CD, signal}}

_بیت‌ها_ معمولا با صفر و یک توصیف می‌شوند اما در واقع هر نوعی از چیزهای دو مقداری را می‌توان بیت در نظر گرفت. درون کامپیوتر، بیت ها به شکل بار الکتریکی بالا و پایین، سیگنال ضعیف و قوی یا نقاط تاریک و روشن روی سطح یک CD در می آیند. هر مقدار گسسته‌ای از اطلاعات را می‌توان به دنباله‌ای از صفرها و یک‌ها و درنتیجه به صورت بیت‌ها نشان داد.

{{index "binary number", radix, "decimal number"}}

به عنوان مثال، می‌توانیم عدد 13 را به شکل بیت‌ها نشان دهیم. مکانیزم نمایش، شبیه نمایش اعداد دهدهی است؛ اما با این تفاوت که به جای 10 رقم مختلف، شما فقط دو رقم در اختیار دارید که وزن هر کدام با ضریب 2 از راست به چپ افزایش می‌یابد. در اینجا بیت‌هایی که عدد 13 را نشان می‌دهند به همراه وزن هر رقم در زیر آن آورده شده است.

```{lang: null}
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

بنابراین عدد دودویی مورد نظر 00001101 می‌باشد که معادل رقم‌های غیر صفر آن 8, 4, و 1 می‌باشد که برابر است با 13.

## مقدارها

{{index [memory, organization], "volatile data storage", "hard drive"}}

دریایی از بیت‌ها را تصور کنید. اقیانوسی از آن‌ها را. یک کامپیوتر مدرن معمولی دارای بیش از ۳۰ میلیارد بیت در حافظه‌ی فرارش (حافظه‌ی اصلی) است. حافظه‌های غیر فرار(مثل دیسک سخت یا هارد دیسک و شبیه آن) چندین برابر بیشتر و بزرگ تر هستند.

برای اینکه بتوان با این تعداد از بیت‌ها کار کرد و در بین آن‌ها گم نشد، باید آن‌ها را به تکه‌هایی که هر کدام بخشی از اطلاعات را نمایش می‌دهند، تقسیم کنیم. در یک محیط جاوااسکریپتی، این تکه‌ها را به نام _((مقدار‌ها))_ می خوانند. اگرچه همه‌ی مقدارها از بیت‌ها تشکیل شده اند، اما نقش‌های مختلفی را ایفا می کنند. هر مقدار دارای یک نوع داده است که نقش آن را مشخص می‌کند. بعضی مقدارها عدد هستند، بعضی حروف متن هستند، بعضی مقدارها تابع (function) می‌باشند و مانند آن.

{{index "garbage collection"}}

برای ایجاد یک مقدار، فقط کافیست نام آن را فراخوانی کنید. بسیار سرراست. نیازی به مواد اولیه یا پرداخت هزینه‌ای نیست؛ فقط فراخوانی کنید و تمام. شما آن را ایجاد کردید. البته آن‌ها از عدم به وجود نمی آیند. مقدار‌ها باید جایی ذخیره شوند و اگر بخواهید در یک زمان مقدار بسیار زیادی از آن‌ها را استفاده کنید، ممکن است که با کمبود حافظه روبرو شوید. خوشبختانه، این مشکل فقط زمانی رخ می دهد که در یک آن واحد (همزمان) نیاز به همه‌ی آن‌ها داشته باشید. به محض اینکه از مقداری استفاده نکنید، آن مقدار از چرخه خارج خواهد شد و بیت‌های متناظرش برای ایجاد مقدارهای جدید مورد استفاده قرار خواهد گرفت.

این فصل به معرفی عناصر اساسی برنامه‌های جاوااسکریپت می پردازد که شامل نوع مقدارهای ساده و عملگرهایی که روی این مقادیر می‌توان اعمال کرد می‌شود.

## اعداد

{{index [syntax, number], number, [number, notation]}}

مقدارهای مربوط به نوع عدد (number) طبیعتا مقادیر عددی هستند. در یک برنامه جاوااسکریپت، این مقادیر به شکل زیر نوشته می‌شوند:

```
13
```

{{index "binary number"}}

استفاده از این مقدار در برنامه باعث می‌شود تا الگوی بیتی متناظر عدد 13، در داخل حافظه‌ی کامپیوتر به وجود آید.

{{index [number, representation], bit}}

جاوااسکریپت از تعداد بیت ثابتی (64 بیت) برای ذخیره یک عدد استفاده می‌کند. با این 64 بیت می‌توان ترکیب‌های بسیار زیادی ساخت و البته که تعداد اعداد متفاوتی که می‌توان نمایش داد دارای محدودیت است. برای _N_ ((رقم)) دهدهی، می‌توان 10^N^ ( ده به توان N) عدد را نمایش داد. به طور مشابه، برای 64 رقم دودویی، می‌توان 2^64^ عدد متفاوت را نشان داد که چیزی حدود 18 کوینتیلیون ( عدد 18 و 18 تا صفر جلوی آن) می‌شود که رقم بسیار بزرگی است.

حافظه‌ی کامپیوتر سابقا بسیار کوچک‌تر بود و برای نمایش اعدا مجبور بودیم از 8 یا 16 بیت استفاده کنیم. در این شرایط به راحتی امکان رخ دادن سرریز (_((overflow))_) فراهم بود – عددی تولید شود که با تعداد بیت موجود امکان نمایش آن نبود. امروزه، حتی کامپیوتر‌هایی که در جیب شما هم جا می‌شوند دارای حافظه‌های بزرگ هستند بنابراین به راحتی می‌توانید از قطعه‌های 64 بیتی استفاده کنید و نیازی نیست نگران رخ دادن سرریز باشیم مگر در شرایطی که با اعدادی نجومی و بسیار بزرگ سر و کار داریم.

{{index sign, "floating-point number", "sign bit"}}

البته تمامی اعداد صحیح کمتر از 18 کوینتیلیون را نمی‌توان در جاوااسکریپت نمایش داد. آن تعداد بیت‌هایی که گفته شد، باید اعداد منفی را هم جا بدهند بنابراین یک بیت برای نگه‌داری علامت عدد استفاده می‌شود. دردسر بزرگ‌تر این است که اعداد غیرصحیح (اعشاری) را نیز باید نشان داد. برای این کار بعضی از بیت‌ها را برای ذخیره‌ی محل قرارگیری نقطه‌ی اعشاری یا علامت ممیز در نظر می گیرند. در واقع بزرگترین عددی صحیحی را که می‌توان ذخیره کرد،  بیشتر در بازه‌ی 9 کوادریلیون (عدد 9 و 15 صفر) قرار می‌گیرد که البته هنوز رقم بسیار بزرگی است.

{{index [number, notation], "fractional number"}}

اعداد کسری یا اعشاری را با استفاده از نقطه می نویسند.

```
9.81
```

{{index exponent, "scientific notation", [number, notation]}}

برای اعداد خیلی بزرگ یا خیلی کوچک، همچنین می‌توانید از روش نمادگذاری علمی استفاده کنید. به این صورت که کاراکتر _e_ (ابتدای واژه‌ی _exponent_ به معنای توان) را به همراه توان عدد در انتهای عدد مورد نظر می نویسیم.

```
2.998e8
```

عدد بالا معادل <bdo>2.998 × 10^8^ = 299,800,000</bdo> می‌باشد.

{{index pi, [number, "precision of"], "floating-point number"}}

محاسبات روی اعداد صحیح (integer) کمتر از بیشینه‌ی ذکر شده (9 کوادریلیون)، همیشه دقیق محاسبه می‌شوند. متاسفانه، محاسبات روی اعداد اعشاری معمولا این چنین دقیق نیستند. درست مثل عدد پی (Pi) که نمی‌توان آن را با دنباله‌ای متناهی از ارقام نشان داد، وقتی فقط 64 بیت برای ذخیره‌ی اعداد در اختیار داریم، بسیاری از اعداد، بخشی از دقت را در محاسبات از دست می‌دهند. این قابل قبول به نظر نمی‌رسد، اما در عمل، مشکل جدی فقط در شرایط خاص رخ می دهد. نکته‌ی مهمی که باید در نظر داشت این است که حواسمان باید به این نبود دقت باشد و با ارقام اعشاری به صورت تقریبی کار کنیم نه به عنوان مقادیر صد در صد دقیق.

### حساب

{{index [syntax, operator], operator, "binary operator", arithmetic, addition, multiplication}}

کار اصلی ما با اعداد انجام محاسبات است. عملیات جبری یا حسابی مثل جمع یا ضرب، دو مقدار عددی را می‌گیرند و عددی جدیدی را از آن ‌ها تولید می کنند. در جاوااسکریپت این گونه محاسبات به شکل زیر خواهند بود:

```
100 + 4 * 11
```

{{index [operator, application], asterisk, "plus character", "* operator", "+ operator"}}

نماد‌های `+` و `*` را _عملگر_ می نامند. اولین نماد، نماد جمع و نماد دوم برای ضرب استفاده می‌شود. اگر عملگری بین دو مقدار قرار بگیرد، روی آن‌ها عمل کرده و مقدار جدیدی را تولید می‌کند.

{{index grouping, parentheses, precedence}}

آیا معنای عبارت بالا این است که عدد 4 را با 100 جمع کن و حاصل را در 11 ضرب نما؟ یا عمل ضرب قبل از جمع انجام می‌شود؟ همانطور که ممکن است حدس زده باشید، عمل ضرب زودتر انجام می‌شود. اما درست مثل ریاضیات، می‌توانید عمل جمع را داخل پرانتز قرار دهید تا ترتیب را عوض کنید.

```
(100 + 4) * 11
```

{{index "hyphen character", "slash character", division, subtraction, minus, "- operator", "/ operator"}}

برای عمل تفریق، عملگر `-` و برای تقسیم، عملگر `/` وجود دارد.

زمانی که بدون استفاده از پرانتز از عملگرها استفاده می‌شود، ترتیبی که برای محاسبه و تقدم آن‌ها استفاده می‌شود براساس _((تقدم))_ عملگرها است. در مثال قبل، مشاهده کردید که عمل ضرب قبل از عمل جمع انجام می‌شود. عملگر `/` هم تقدم مشابهی با `*` دارد. `+` و `-` هم تقدم یکسانی دارند. زمانی که چندین عملگر با تقدم مشابه در یک عبارت قرار می گیرند، مثل <bdo>`1 - 2 + 1`</bdo>، نحوه بکارگیری از چپ به راست خواهد بود: <bdo>`(1 - 2) + 1`</bdo>٫

نگران این قوانین تقدم نباشید. اگر نسبت به اولویت تقدم‌ها شک داشتید، از پرانتز استفاده کنید.

{{index "modulo operator", division, "remainder operator", "% operator"}}

هنوز یک عملگر حسابی دیگر  مانده است، عملگری که احتمالا کارکرد‌ آن ا حدس نزده‌اید. نماد `%` برای عمل محاسبه‌ی باقی مانده استفاده می‌شود.`X % Y` به معنای محاسبه‌ی باقی مانده‌ی تقسیم `X` بر `Y` است. مثلا حاصل عبارت <bdo>`314 % 100`</bdo> می‌شود `14` و <bdo>`144 % 12`،</bdo> عدد 0 را تولید می‌کند. تقدم عملگر باقی‌مانده نیز مانند جمع و تقسیم است. شما اغلب برای این عملگر، اصطلاح پیمانه را مشاهده می‌کنید، اگرچه از نظر فنی همان باقی مانده دقیق تر است.

### اعداد خاص

{{index [number, "special values"]}}

در جاوااسکریپت سه مقدار خاص وجود دارند که به عنوان عدد محسوب می‌شوند اما خاصیت و رفتار اعداد معمولی را ندارند.

{{index infinity}}

دو تای اول `Infinity` و <bdo>`-Infinity`</bdo> هستند که نماد مثبت و منفی بی‌نهایت می‌باشند. <bdo>`Infinity - 1`</bdo> ، همچنان `Infinity` خواهد بود. زیاد به محاسباتی که بر اساس مقدار Infinity هستند اعتماد نکنید. از نقطه‌نظر ریاضیات، زیاد استوار نیستند و ممکن است باعث تولید نتیجه‌ای بشوند که همان عدد خاص سوم ما می‌باشد: `NaN`.

{{index NaN, "not a number", "division by zero"}}

`NaN` مخفف Not a Number به معنای “غیر عدد” است. اگرچه مقداری برای نوع داده‌ی عدد محسوب می‌شود. اگر به عنوان مثال، سعی کنید که `0 / 0` (صفر را بر صفر تقسیم کنید) را محاسبه کنید با NaN روبرو خواهید شد یا `Infinity - Infinity` یا هر عمل حسابی دیگر روی اعداد که نتیجه‌ی آن مشخص و با معنا نباشد.

## رشته‌ها

{{indexsee "grave accent", backtick}}

{{index [syntax, string], text, character, [string, notation], "single-quote character", "double-quote character", "quotation mark", backtick}}

نوع داده‌ی بنیادی بعدی _((رشته))_ است. رشته‌ها برای نمایش متن استفاده می‌شوند و به این صورت نوشته می‌شوند که محتوای آن‌ها داخل علامت نقل قول تک یا جفت قرار می‌گیرد.

```
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

می‌توانید از کاراکتر نقل قول تک، نقل قول جفت یا (`` - backtick) برای مشخص کردن رشته‌ها استفاده کنید البته تا زمانی که برای شروع و پایان رشته از یک علامت یکسان استفاده می‌کنید.

{{index "line break", "newline character"}}

جاوااسکریپت تقریبا هر مقداری که در داخل علامت‌های نقل قول محصور بشود را رشته در نظر می‌گیرد یا سعی می‌کند به رشته تبدیل کند. البته چند کاراکتر ویژه، شرایط متفاوتی دارند. اگر بخواهیم خود علامت نقل قول را بین علامت‌های نقل قول در یک رشته قرار دهیم، مشکل خواهیم داشت. کاراکترهای خط جدید (کاراکترهایی که پس از فشردن کلید [enter]{keyname} به وجود می‌آیند) را نیز بدون “گریز دادن” فقط می‌توان زمانی در رشته قرار داد که رشته توسط کاراکتر backticks(`) محصور شده باشد.

{{index [escaping, "in strings"], ["backslash character", "in strings"]}}

برای اینکه اینگونه کاراکترها را در یک رشته قرار دهیم، از این روش استفاده می‌شود: هرگاه درون یک رشته‌‌ی متنی که بین نقل قول محصور شده است، کاراکتر (`\`) بک اسلش پیدا شود، به این معنا است که کاراکتر بعد از آن معنای خاصی دارد و باید طور دیگری تفسیر شود. به این کار گریزدادن (_escaping_) گفته می‌شود. کاراکتر نقل قولی که قبل از آن، کاراکتر `\` قرار گرفته، دیگر به معنای کاراکتر پایان رشته تفسیر نمی‌شود بلکه خود بخشی از رشته خواهد بود. اگر بعد از `\` کاراکتر `n` قرار گیرد، به عنوان یک خط جدید تفسیر می‌شود. به طور مشابه، کاراکتر `t` بعد از `\` ، به معنای ((tab character)) خواهد بود. به رشته‌ی زیر توجه کنید.:

```
"This is the first line\nAnd this is the second"
```

خروجی واقعی متن بالا به شکل زیر خواهد بود:

```{lang: null}
This is the first line
And this is the second
```

مواقعی هم پیش خواهد آمد که بخواهید خود بک اسلش را درون یک رشته نمایش دهید نه به صورت یک کد خاص. در این صورت از دو بک اسلش پشت سر هم استفاده می‌شود. این کار باعث می‌شود که یکی از آن‌ها در رشته‌ی نهایی نشان داده شود. مثلا برای اینکه رشته‌ای متناظر با رشته‌ی زیر را نمایش دهید:
"_A newline
character is written like `"`\n`"`._"
در جاوااسکریپت باید به شکل زیر نوشته شود:

```
"A newline character is written like \"\\n\"."
```

{{id unicode}}

{{index [string, representation], Unicode, character}}

رشته‌ها نیز باید به شکل دنباله‌ای از بیت ها تبدیل شوند تا درون کامپیوتر ذخیره شوند. روشی که جاوااسکریپت برای این کار استفاده می‌کند بر اساس استاندارد _((Unicode))_ است. این استاندارد عددی را به همه‌ی کاراکترهایی که ممکن است نیاز داشته باشید (شامل کاراکترهای از زبان های یونانی، عربی، ژاپنی، ارمنی و غیره)،  اختصاص می دهد. اگر برای هر کاراکتر عددی داشته باشیم، رشته ها را می‌توان به وسیله دنباله‌ای از این اعداد توصیف کرد.

{{index "UTF-16", emoji}}

و این کاری است که جاوااسکریپت انجام می دهد. اما اینجا یک مشکل وجود دارد: سیستم نمایش جاوااسکریپت از 16 بیت برای هر عنصر رشته استفاده می‌کند که باعث می‌شود بتوان تا 2^16^ کاراکتر متفاوت را پوشش داد. اما یونیکد کاراکترهای بیشتری را تعریف می‌کند که در حال حاضر تقریبا دوبرابر این تعداد است. بنابراین بعضی کاراکترها مثل خیلی از کاراکترهای شکلک(ایموجی)، در یک رشته دو “فضای کاراکتر” را اشغال می کنند. به این نکته در [فصل
?](higher_order#code_units) باز خواهیم گشت.

{{index "+ operator", concatenation}}

نمی‌توان عملیات حسابی تقسیم، ضرب یا تفریق را روی رشته‌ها انجام داد، اما عملگر `+` را می‌توان استفاده کرد. این عملگر روی رشته‌ها جمع حسابی را انجام نمی دهد، بلکه عمل چسباندن دو رشته را صورت می‌دهد. کد زیر رشته‌ی `"concatenate"` را تولید می‌کند.

```
"con" + "cat" + "e" + "nate"
```

می‌توان روی مقادیر رشته ای عملیاتی را به وسیله توابع مرتبط (_متدها_) انجام داد که در [فصل ?](data#methods) به آن‌ها می پردازیم.

{{index interpolation, backtick}}

رشته‌هایی که به وسیله‌ی نقل قول تک یا جفتی نوشته می‌شوند خیلی شبیه به هم عمل می کنند – تنها تفاوت بر می گردد به علامت نقل قولی که باید در صورت استفاده با توجه به آن گریز داده شود. رشته‌هایی که با علامت backtick محصور می‌شوند،  معمولا _((template
literals))_ نامیده می‌شوند که قابلیت بیشتری دارند. جدا از اینکه می‌توان در آن‌ها خطوط رشته را شکست یا خطوط متعدد داشت، می‌توانند مقادیر دیگر را نیز در خود جاسازی کنند.

```
`half of 100 is ${100 / 2}`
```

زمانی که چیزی را داخل<bdo>`${}`</bdo> می نویسید و آن را با backtick محصور می‌کنید، نتیجه عبارت مورد نظر محاسبه شده، به رشته تبدیل می‌شود و در جای مورد نظر قرار می‌گیرد. مثال بالا رشته‌ی "_half of 100 is 50_" را تولید می‌کند.

## عملگرهای یکانی

{{index operator, "typeof operator", type}}

همه‌ی عملگرها را با نمادها نشان نمی‌دهند. بعضی از آن‌ها با حروف معمولی لاتین نوشته‌ می‌شوند. به عنوان مثال می‌توان عملگر `typeof` را ذکر نمود که نوع داده‌ی ورودی خود را به شکل یک رشته برمی گرداند.

```
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

{{index "console.log", output, "JavaScript console"}}

{{id "console.log"}}

در مثال‌های کتاب برای نشان دادن نتیجه‌ی ارزیابی چیزی،‌ از دستور `console.log` استفاده می کنیم. برای اطلاعات بیشتر در باره آن به [فصل بعد](program_structure) مراجعه کنید.

{{index negation, "- operator", "binary operator", "unary operator"}}

دیگر عملگر‌هایی که تا به حال دیده‌ایم بر روی دو مقدار عمل می کردند اما `typeof` فقط به یک مقدار نیاز دارد. عملگرهایی که به دو مقدار نیاز دارند، عملگرهای دودویی (_binary_) نامیده می‌شوند،‌ در حالیکه آن‌هایی که روی یک مقدار عمل می کنند را عملگرهای _یکانی_ می نامند. عملگر تفریق (-) را می‌توان هم به عنوان یکانی و هم دودویی استفاده کرد.

```
console.log(- (10 - 2))
// → -8
```

### مقدارهای بولی

{{index Boolean, operator, true, false, bit}}

گاهی اوقات لازم است تا به وسیله‌ی یک مقدار فقط دو حالت را شناسایی و تمییز دهیم، مثل “yes” و “no” یا “on” و “off”. برای این منظور جاوااسکریپت نوع داده‌ی بولی (_Boolean_) را در نظر گرفته است که فقط دارای دو مقدار است: true و false (که به همین شکل نوشته می‌شوند).

### مقایسه‌ها

{{index comparison}}

یکی از راه‌های تولید مقدار‌های بولی را در مثال زیر می بینیم:

```
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

{{index [comparison, "of numbers"], "> operator", "< operator", "greater than", "less than"}}

علامت‌های <bdo>`>`</bdo> و <bdo>`<`</bdo> به ترتیب همان نمادهای سنتی برای “بزرگتر است از” و “کوچکتر است از” هستند. آن‌ها عملگرهایی دودویی می‌باشند.  استفاده از آن‌ها منجر به تولید یک مقدار بولی می‌شود که نشان می دهد که true است یا خیر.

رشته‌ها را هم می‌توان به روشی مشابه مقایسه کرد.

```
console.log("Aardvark" < "Zoroaster")
// → true
```

{{index [comparison, "of strings"]}}

رشته‌ها تقریبا براساس حروف الفبا مرتب می‌شوند اما نه کاملا به شکلی که در واژه‌نامه می بینید: حروف بزرگ در عمل مقایسه از حروف کوچک “کوچک‌تر” محسوب می گردند،‌ بنابراین عبارت <bdo>`"Z" < "a"`</bdo> مقدار true را برمی‌گرداند، وکاراکترهای غیرالفبایی (!,-, و مانند آن‌ها) نیز ترتیب دارند و مقایسه می‌شوند. در زمان مقایسه‌ی رشته ها، جاوااسکریپت کاراکترهای هر رشته را یک به یک از چپ به راست براساس کدهای یونیکدشان، باهم مقایسه می‌کند.

{{index equality, ">= operator", "<= operator", "== operator", "!= operator"}}

دیگر عملگرهای مقایسه‌ای به این صورت هستند: عملگر=< (بزرگتر مساوی)،=> (کوچکتر مساوی)، == (برابری) و=! (نابرابری).

```
console.log("Itchy" != "Scratchy")
// → true
console.log("Apple" == "Orange")
// → false
```

{{index [comparison, "of NaN"], NaN}}

تنها یک مقدار در جاوااسکریپت وجود دارد که با خودش برابر نیست، و آن `NaN` است که به معنای “مقدار غیر عددی” می‌باشد.

```
console.log(NaN == NaN)
// → false
```

`NaN` به این منظور ایجاد شده که نشان دهد نتیجه‌ی محاسبه بی معنا بوده است، و به همین دلیل، با نتیجه‌ی هیچ محاسبه‌ی بی‌معنای دیگری برابر نخواهد بود.

### عملگرهای منطقی

{{index reasoning, "logical operators"}}

عملیاتی نیز وجود دارند که می‌توانند روی مقدارهای بولی اجرا شوند. جاوااسکریپت از سه عملگر منطقی پشتیبانی می‌کند: _and_ ، _or_ و _not_. این عملگرها را می‌توان برای انجام عملیات منطقی روی عبارات بولی استفاده کرد.

{{index "&& operator", "logical and"}}

عملگر `&&` نماد _and_ منطقی است. این عملگر دودویی و نتیجه‌ی آن زمانی صحیح (true) است که هر دو عملوند یا مقداری که به آن داده می‌شود، صحیح (true) باشند.

```
console.log(true && false)
// → false
console.log(true && true)
// → true
```

{{index "|| operator", "logical or"}}

عملگر `||` نماد _or_ منطقی است. نتیجه‌ی این عملگر زمانی صحیح (true) است که یکی از دو مقدار داده شده، صحیح باشد.

```
console.log(false || true)
// → true
console.log(false || false)
// → false
```

{{index negation, "! operator"}}

_Not_ یا نقیض با علامت تعجب (`!`) نمایش داده می‌شود که عملگری یکانی است و مقداری را که به آن داده می‌شود را نقیض می‌کند ( برعکس می‌کند) – <bdo>`!true`</bdo> مقدار `false` را بر می گرداند و <bdo>`!false`</bdo> مقدار `true`.

{{index precedence}}

زمانی که این عملگرها را با عملگرهای حسابی و دیگر عملگرها ترکیب می کنیم، همیشه واضح نیست که کی لازم است از پرانتزها استفاده کرد. در عمل معمولا می‌توان با دانستن اینکه در بین عملگرهایی که بررسی کردیم، `||` دارای کمترین حق تقدم، بعد `&&` و پس از آن عملگرهای مقایسه (`>`, `==`, …) و بعد بقیه عملگرها قرار می گیرند، پیش رفت. با توجه به ترتیب تقدم‌ها به این شکل در عبارتی مثل عبارت پایین به حداقل پرانتز می‌توان اکتفا کرد.:

```
1 + 1 == 2 && 10 * 10 > 50
```

{{index "conditional execution", "ternary operator", "?: operator", "conditional operator", "colon character", "question mark"}}

آخرین عملگر منطقی که قصد دارم در باره آن صحبت کنم، نه یکانی است و دودویی، بلکه سه‌تایی (_ternary_) می‌باشد و روی سه مقدار عمل می‌کند. برای نوشتن آن از علامت سوال و دونقطه به شکل زیر استفاده می‌شود:

```
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

این عملگر را عملگر شرطی می نامند (گاهی هم همان عملگر سه‌تایی نامیده می‌شود به این دلیل که تنها عملگری است که این ویژگی را در جاوااسکریپت دارد). مقداری که در سمت چپ علامت سوال قرار می‌گیرد، مشخص می‌کند که کدام یک از دو مقدار بعدی به عنوان نتیجه برگردانده می‌شود. زمانی که صحیح (true) ارزیابی شود، مقدار وسطی انتخاب می‌شود، و هنگامی که false است، آخرین مقدار که در سمت راست قرار دارد، برگردانده می‌شود.

## مقدارهای پوچ

{{index undefined, null}}

دو مقدار خاص وجود دارد که به شکل `null` و `undefined` نوشته می‌شوند که برای مشخص کردن مقدار‌هایی که درجاوااسکریپت معنایی ندارند استفاده می‌شوند. آن‌ها خودشان مقدار هستند اما اطلاعات خاصی را حمل نمی کنند.

عملیات زیادی در جاوااسکریپت وجود دارند که مقدار معناداری را تولید نمی کنند ( در ادامه مواردی را خواهید دید) و با توجه‌ به اینکه بالاخره باید مقداری را برگردانند، `undefined` را تولید می کنند.

تفاوت معنای دو مقدار `undefined` و `null` به خاطر نحوه‌ی طراحی خود جاوااسکریپت است و در بیشتر اوقات باهم تفاوت معنایی خاصی ندارند. اگر شرایطی وجود دارد که باید نگران تفاوت این دو باشید، پیشنهاد می کنم که هر دو را معادل هم در نظر بگیرید و هرکدام را خواستید به جای دیگری استفاده کنید ( در آینده بیشتر به آن می پردازیم).

## تبدیل خودکار نوع داده

{{index NaN, "type coercion"}}

در مقدمه کتاب، توضیح دادم که جاوااسکریپت برای اینکه هر برنامه‌ای که شما به آن می دهید را تفسیر کند، تلاش بسیاری می‌کند، حتی برنامه‌هایی که به نظر صحیح نمی آیند. در عبارات زیر این مساله به خوبی نشان داده شده است:

```
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

{{index "+ operator", arithmetic, "* operator", "- operator"}}

زمانی که یک عملگر، به نوع مقدار غلطی اعمال می‌شود، جاوااسکریپت با استفاده از مجموعه‌ای از قواعد که اغلب قابل پیش بینی و انتظار نیستند، بی سر و صدا مقدار مورد نظر را به نوعی که می خواهد تبدیل می‌کند. به این کار، تبدیل خودکار نوع یا _((type coercion))_ می گویند. بنابراین `null` در عبارت اول به `0` تبدیل می‌شود، و `"5"` در عبارت دوم به `5` ( از رشته به عدد) تبدیل می‌شود. همچنین در عبارت سوم، `+` ابتدا سعی می‌کند که رشته‌ها را بهم الحاق کند قبل از اینکه عمل جمع حسابی را انجام بدهد بنابراین عدد `1` را به `"1"` ( عدد به رشته) تبدیل می‌کند.

{{index "type coercion", [number, "conversion to"]}}

اگر مقداری را نتوان به شکل واضحی به یک عدد تبدیل کرد‌ (مثلا `"five"` یا `undefined`)، مقدار `NaN` تولید خواهد شد. همچنین عملیات حسابی روی مقدار `NaN` باعث تولید دوباره‌ی `NaN` می‌شود، بنابراین اگر در حین برنامه‌نویسی متوجه شدید که این مقدار را در جایی که انتظارش را نداشتید،‌ مشاهده کردید، به دنبال تبدیل تصادفی نوع داده بگردید.

{{index null, undefined, [comparison, "of undefined values"], "== operator"}}

زمانی که مقدارهایی از یک نوع داده را با `==` مقایسه می کنیم، خروجی به راحتی قابل پیش بینی است: اگر دو مقدار مشابه‌ هم باشند، نتیجه صحیح (true) خواهد بود به استثنای مقدار `NaN`. اما هنگامی‌که نوع داده‌ها متفاوت اند، جاوااسکریپت از یک سری دستورات پییچیده و گیج‌کننده برای انجام مقایسه استفاده می‌کند. در اکثر موارد، راه‌حل انتخابی تبدیل یکی از مقادیر به نوع مقدار دیگر است. هرچند زمانی که `null` یا `undefined` در یک سمت از عملگر قرار می‌گیرد، فقط زمانی مقدار true تولید می‌شود که هر دو طرف یا `null` یا `undefined` باشند.

```
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

این رفتار جاوااسکریپت درباره‌ی `null` و `undefined` اغلب کاربردی است.
می‌توانید به سادگی با مقایسه‌ی یک مقدار با `null` با استفاده از عملگر `==` یا <bdo>`!=`</bdo>، به واقعی و معنادار بودن یک مقدار پی ببرید.

{{index "type coercion", [Boolean, "conversion to"], "=== operator", "!== operator", comparison}}

اما برای آزمایش اینکه مقداری دقیقا برابر با `false` است یا نه، چه باید کرد؟ طبق قوانین مربوط به تبدیل رشته‌ها و اعداد به مقادیر بولی، `0`, `NaN` و رشته‌ی خالی (“”)، به صورت false تفسیر می‌شوند، درحالی‌که دیگر مقادیر به عنوان true در نظر گرفته می‌شوند. به همین علت عبارتی مثل <bdo>`0 == false`</bdo> و <bdo>`"" == false`</bdo> به عنوان true ارزیابی می‌شوند. برای این گونه موارد، زمانی که این تبدیل خودکار نوع داده را نمی خواهید، می‌توان از دو عملگر دیگر استفاده کرد: `===` و <bdo>`!==`</bdo>. عملگر اول بررسی می‌کند که مقدار دقیقا متناظر با مقدار دیگر باشد، و دومی بررسی می‌کند که دقیقا متناظر نباشند. بنابراین <bdo>`"" === false`</bdo> ، مقدار ناصحیح(false) را برمی گرداند، همانطور که مورد انتظار بود.

من پیشنهاد می‌کنم که از عملگر مقایسه‌ای سه‌کاراکتری به صورت پیشگیرانه استفاده شود تا از تبدیل داده‌های ناخواسته که باعث دردسر می‌شوند جلوگیری شود. اما زمانی که از شباهت نوع داده دو طرف مطمئن هستید، استفاده از عملگر‌های کوتاه‌تر نادرست نخواهد بود.

### اتصال کوتاه در عملگرهای منطقی

{{index "type coercion", [Boolean, "conversion to"], operator}}

عملگرهای منطقی `&&` و `||` از روش به خصوصی برای ارزیابی مقدارهای انواع داده‌ی مختلف استفاده می کنند. ابتدا مقدار سمت چپ عملگر را به نوع بولی تبدیل می کنند تا تصمیم بگیرند که چه کنند و بسته به نوع عملگر و نتیجه تبدیل نوع داده، یا مقدار اولیه سمت چپ را برمی‌گردانند یا مقدار سمت راست را.

{{index "|| operator"}}

عملگر `||` ، به عنوان مثال، زمانی مقدار سمت چپ عبارت را برمیگرداند که بتوان آن را به true تبدیل کرد و در غیر این صورت مقدار سمت راست را برمیگرداند. این تبدیل همانطور که انتظارش را داشتید برای مقادیر بولی عمل می‌کند و برای مقادیر دیگر انواع داده نیز به شکل مشابهی عمل می‌کند.

```
console.log(null || "user")
// → user
console.log("Agnes" || "user")
// → Agnes
```

{{index "default value"}}

با این ویژگی می‌توان از عملگر `||` به عنوان روشی برای در نظر گرفتن مقدار پیش‌فرض در عبارت‌ها استفاده کرد. اگر مقداری را داشته باشید که ممکن است تهی باشد، می‌توانید بعد از آن `||` به همراه مقداری جایگزین قرار دهید. اگر مقدار اولیه را بتوان به false تبدیل کرد، نتیجه‌ای که دریافت خواهید کرد برابر با مقدار جایگزین خواهد بود. طبق قوانین تبدیل رشته‌ها و اعداد به مقادیر بولی، `0`, `NaN` و رشته‌ی تهی (`""`) به عنوان `false` در نظر گرفته می‌شود، درحالیکه دیگر مقادیر به عنوان `true` شمرده می‌شود. بنابراین <bdo>`0 || -1`</bdo> مقدار <bdo>`-1`</bdo> و عبارت <bdo>`"" || "!?"`</bdo> مقدار <bdo>`"!?"`</bdo> را تولید می‌کند.

{{index "&& operator"}}

عملگر `&&` به شکل مشابهی عمل می‌کند، اما در جهت عکس. زمانی‌که مقداری که در سمت چپ عبارت قرار دارد، عبارتی است که به false تبدیل می‌شود، همان مقدار سمت چپ برگردانده خواهد شد، در غیر اینصورت، مقدار سمت راست ارزیابی و برگردانده می‌شود.

یکی دیگر از ویژگی‌های مهم این دو عملگر این است که عبارت سمت راست آن‌ها، فقط در صورت نیاز ارزیابی می‌شود. در مورد مثل <bdo>`true || X`</bdo>، مهم نیست که `X` چیست، حتی اگر عبارتی است که در صورت اجرا، کار وحشتناکی انجام می دهد – نتیجه عبارت ، true خواهد بود و `X` اصلا ارزیابی یا اجرا نمی‌شود. همین قضیه برای <bdo>`false && X`</bdo> نیز صادق است که نتیجه‌ی آن false است و `X` ارزیابی نمی‌شود. این‌ کار ارزیابی مدار کوتاه نامیده می‌شود.

{{index "ternary operator", "?: operator", "conditional operator"}}

عملگر منطقی (سه‌تایی) نیز به شکل مشابهی کار میکند. عبارت اول، همیشه ارزیابی می‌شود، اما دومین یا سومین مقدار – مقداری که انتخاب نمی‌شود – ارزیابی هم نخواهد شد.

## خلاصه

در این فصل به چهار نوع از مقدارهای جاوااسکریپت نگاهی انداختیم:‌اعداد، رشته‌ها، مقادیر بولی، و مقادیر تعریف نشده.

این مقدار‌ها را می‌توان با تایپ نامشان (`true`, `null`) یا مقدارشان (`13`, `"abc"`) به وجود آورد. شما می‌توانید با استفاده از عملگرها، این مقدارها را ترکیب کنید و تغییر دهید. ما عملگرهای دودویی را برای عملیات حسابی (`+`, `-`, `*`, `/`,
و `%`)، عملگر الحاق رشته (`+`)، عملگرهای مقایسه (<bdo>`==`, `!=`, `===`,
`!==`, `<`, `>`, `<=`, `>=`</bdo>)، منطقی (`&&`, `||`)، همچنین چندین عملگر یکانی (`-` برای منفی کردن یک عدد، `!` برای نقیض منطقی، و `typeof` برای فهمیدن نوع داده‌ی یک مقدار) و عملگر سه‌تایی (<bdo>`?:`</bdo>) برای انتخاب یکی از دو مقدار بر اساس مقدار سوم را در این فصل مشاهده نمودیم.

این مقدار اطلاعات برای استفاده از جاوااسکریپت به عنوان یک ماشین‌ حساب جیبی و نه بیشتر کافی است. در [فصل بعد](program_structure) شروع به ترکیب این عبارت‌ها خواهیم کرد تا بتوانیم برنامه‌هایی ابتدایی بسازیم.
