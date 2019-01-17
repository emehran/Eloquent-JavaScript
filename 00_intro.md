{{meta {load_files: ["code/intro.js"]}}}

# Introduction

{{quote {author: "Ellen Ullman", title: "Close to the Machine: Technophilia and its Discontents", chapter: true}

We think we are creating the system for our own purposes. We believe
we are making it in our own image... But the computer is not really
like us. It is a projection of a very slim part of ourselves: that
portion devoted to logic, order, rule, and clarity.

quote}}

{{figure {url: "img/chapter_picture_00.jpg", alt: "Picture of a screwdriver and a circuit board", chapter: "framed"}}}

این کتاب درباره‌ی فرمان‌ دادن به کامپیوتر‌هاست. امروزه کامپیوترها تقریبا مثل پیچ‌گوشتی‌ها رایج و پراستفاده هستند اما از آن ها پیچیده‌ترند و بکارگیری آن‌ها برای انجام کاری که می خواهید نیز همیشه کار آسانی نیست.

اگر کاری که از کامپیوترتان می خواهید تا انجام دهد، کاری سرراست و قابل فهم باشد مثل نمایش ایمیل‌تان یا کار به عنوان ماشین حساب، می توانید نرم‌افزار مناسب آن را باز کرده و استفاده کنید. اما برای کارهای خاص یا وظایفی که انتهای مشخصی ندارند، احتمالا برنامه‌ای وجود ندارد.

اینجاست که ((برنامه‌نویسی)) به میدان می‌آید. _برنامه‌نویسی_ همان ساختن یک _برنامه_ — مجموعه‌ای از دستورات دقیق که به یک کامپیوتر می گوید که چه کار بکند — می باشد. از آنجا که کامپیوترها موجوداتی احمق و درعین حال دقیق هستند، برنامه‌نویسی اساسا کاری خسته‌کننده و پرمشقت است.

{{index [programming, "joy of"], speed}}

خوشبختانه اگر بتوانید با این سختی کنار بیایید و شاید حتی از فکر کردن به گونه‌ای که ماشین‌های احمق درک کنند، لذت ببرید، برنامه نویسی می تواند رضایت بخش باشد. برنامه نویسی باعث می شود بتوانید کارهایی که به صورت دستی _بی نهایت_ زمان می برد را در چند ثانیه انجام دهید. این راهی است که با آن می توانید با کامپیوترتان کارهایی بکنید که پیش‌تر نمی توانست انجام دهد. همچنین برنامه‌نویسی تمرینی شگفت‌انگیز برای پرورش تفکر انتزاعی محسوب می شود.

بیشتر کار برنامه‌نویسی به وسیله‌ی زبان‌های برنامه‌نویسی صورت می گیرد. یک _زبان برنامه‌نویسی_ زبانی است برای فرمان‌دادن به کامپیوترها ساخته می شود. جالب توجه است که موثرترین روشی که برای ارتباط با یک کامپیوتر پیدا کرده ایم به اندازه‌ی زیادی وام‌دار روشی‌ است که در ارتباط با یکدیگر استفاده می کنیم. مانند زبان‌های بشری، زبان‌های کامپیوتری به واژه‌ها و عبارات امکان ترکیب به شیوه‌های جدید را می دهد و امکان بیان مفاهیم جدید را فراهم می سازد.

{{index [JavaScript, "availability of"], "casual computing"}}

روزگاری رابط‌های کاربری مبتنی بر زبان، مانند BASIC و DOS که در دهه‌های هشتاد و نود میلادی استفاده می شدند، روش اصلی تعامل با کامپیوترها بودند. اکنون رابط‌های بصری به طور گسترده‌ای جایگزین آن‌ها شده اند که یادگیری آسان‌تری دارند و در عین حال آزادی کمتری فراهم می سازند. اگر بدانید کجا را جستجو کنید، زبان‌های کامپیوتری هنوز موجود هستند. یکی از اینگونه‌ زبان‌ها، جاوااسکریپت است، که در هر مرورگر وب مدرن وجود دارد و در نتیجه تقریبا در همه‌ی دیوایس‌ها در دسترس است.

{{indexsee "web browser", browser}}

این کتاب سعی خواهد کرد تا شما را با این زبان به اندازه‌ای آشنا کند تا بتوانید کارهای مفید و مفرحی با آن انجام دهید.

## درباره‌ی برنامه‌نویسی

{{index [programming, "difficulty of"]}}

علاوه بر آموزش جاوااسکریپت، قواعد پایه‌ای برنامه‌نویسی را نیز معرفی خواهم کرد. به نظر می رسد برنامه‌نویسی سخت است. قواعد اساسی آن ساده و روشن هستند اما برنامه‌هایی که بر اساس این قوانین ساخته می شوند پتانسیل این را دارند تا به حدی پیچیده بشوند که قواعد و پیچیدگی‌های خودشان را داشته باشند. در برنامه‌نویسی شما در حال ساخت مارپیچ خودتان هستید و ممکن است به شکلی در آن گم بشوید.‍

{{index learning}}
در زمان خواندن این کتاب مواقعی پیش خواهد آمد که مطالب کتاب حس بدی در شما ایجاد کنند. اگر در برنامه‌نویسی تازه‌وارد هستید، مطالب جدید زیادی برای یادگیری وجود خواهد داشت. بیشتر این مطالب بعد به صورتی ترکیب خواهند شد تا لازم باشد ارتباط بیشتری با آن‌ها برقرار سازید.

تلاش متناسب برای یادگیری بستگی به خود شما دارد. در زمان مطالعه کتاب و تقلا برای یادگیری مطالب، نباید هیچ نتیجه‌ی منفی‌ای نسبت به توانایی‌های خودتان بگیرید. توانایی شما خوب است فقط لازم است تا ادامه دهید. استراحتی بکنید، بعضی از مطالب را دوباره مطالعه کنید و مطمئن شوید که برنامه‌های نمونه و تمرین‌ها را خوب خوانده و فهمیده اید. یادگیری کاری دشوار است اما هرچیزی که یادبگیرید، مال شماست و ادامه یادگیری را آسان‌تر می کند.

{{quote {author: "Ursula K. Le Guin", title: "The Left Hand of Darkness"}

{{index "Le Guin, Ursula K."}}
وقتی اقدام اثری ندارد، اطلاعات جمع‌آوری کن; زمانی وقتی اطلاعات سودی نمی رساند، استراحت کن.
quote}}

{{index [program, "nature of"], data}}

یک برنامه‌ در واقع خیلی چیز ها است. متنی است که توسط یک برنامه‌نویس تایپ شده است، نیرویی پیش‌ران است که باعث می شود کامپیوتر کاری را بایست انجام دهد، داده‌هایی است که در حافظه‌ی کامپیوتر قرار دارند، در عین حال کارهایی که در همان حافظه‌ رخ می دهند را کنترل می کند. تمثیل‌هایی که تلاش دارند برنامه‌ها را با اشیائی که برای ما آشنا هستند مقایسه کنند به زیاد موفق نیستند. یکی از این قیاس‌های تقریبا مناسب، قیاس برنامه با ماشین است. تعداد زیادی قطعات جدا از که با هم درگیر هستند و برای اینکه کل ماشین کار کند، باید راه‌هایی را در نظر بگیریم که این قطعات با هم اتصال دارند و برای کاکرد کل ماشین تعامل می کنند.

یک ((کامپیوتر)) یک ماشین فیزیکی است که مانند یک میزبان برای این قسمت‌های غیرفیزیکی عمل می کند. خود کامپیوترها فقط قادرند کارهایی بی‌نهایت پیش‌ پا افتاده و سرراست را انجام دهند. دلیل اینکه این قدر کاربرد دارند این است که همین کارها را فوق العاده ((سریع)) انجام می دهند. یک برنامه می تواند به صورت هوشمندانه تعداد بسیار زیادی از این
کارهای ساده را باهم ترکیب کند تا وظایف خیلی پیچیده‌ای را به انجام برساند.

{{index [programming, "joy of"]}}
یک برنامه، ساختمانی از جنس فکر است. ساخت آن هزینه ندارد. سبک است و به آسانی توسط تایپ‌کردن پیشرفت می کند.

اما در صورت بی دقتی، اندازه و پیچیدگی یک برنامه به حدی رشد می کند که از کنترل خارج می شود و حتی برای فردی که آن را ایجاد کرده گیج کننده می شود. مشکل اصلی در برنامه‌نویسی، تحت کنترل نگه‌داشتن برنامه است. درست کار کردن یک برنامه زیباست. هنر برنامه‌نویسی، مهارت کنترل‌کردن پیچیدگی برنامه است. برنامه‌ی بزرگ با ساده‌سازی پیچیدگی‌اش مهار می شود.

{{index "programming style", "best practices"}}

بعضی برنامه‌نویسان بر این باورند که بهترین روش مدیریت این پیچیدگی، استفاده از مجموعه‌ای محدود از تکنیک‌های روشن در برنامه‌ها است. این افراد قوانین سخت‌گیرانه‌ای ("بهترین روش‌ها - best practices") را وضع و تجویز کرده‌اند تا برنامه‌ها باید از آن‌ها تبعیت کرده و در محدوده‌ی امن و کوچک آن‌ها باقی بمانند.

{{index experiment}}

این کار نه تنها کسل کننده است، بلکه موثر هم نیست. مشکلات جدید اغلب نیازمند راه‌حل‌های جدید می‌باشند. رشته‌ی برنامه‌نویسی جوان است و هنوز به سرعت در حال توسعه می باشند و به اندازه‌ی کافی متنوع است که فضا برای روش‌های مختلف وجود داشته باشد. اشتباهات مهلکی در طراحی برنامه ممکن است صورت گیرد و بهتر است مرتکب آن ها بشوید و ادامه دهید تا آن ها را درک کنید. نوشتن یک برنامه‌ی خوب با تمرین و توسعه بدست می آید نه با یادگیری لیستی از قوانین.

## چرا زبان برنامه‌نویسی اهمیت دارد

{{index "programming language", "machine code", "binary data"}}

در ابتدا، زمانی که محاسبه‌ی کامپیوتری متولد شد، زبان برنامه‌نویسی وجود نداشت. برنامه ها چیزی شبیه به زیر بودند:

```{lang: null}
00110001 00000000 00000000
00110001 00000001 00000001
00110011 00000001 00000010
01010001 00001011 00000010
00100010 00000010 00001000
01000011 00000001 00000000
01000001 00000001 00000001
00010000 00000010 00000000
01100010 00000000 00000000
```

{{index [programming, "history of"], "punch card", complexity}}

برنامه‌ی بالا اعداد 1 تا 10 را باهم جمع کرده و نتیجه را چاپ می نماید: <bdo>`1 + 2 + ... + 10 = 55`</bdo>. این برنامه می تواند روی یک ماشین ساده فرضی اجرا شود. برای برنامه‌نویسی کامپیوتر‌های اولیه، لازم بود تا ردیف بزرگی از سویچ‌ها را در موقعیت مناسب قرار داد یا در نوارهای مقوایی مخصوص را سوراخ ایجاد کرد و در کامپیوتر قرار داد. احتمالا می توانید تصور کنید که این کار چقدر خسته‌کننده و در معرض اشتباه بود. حتی نوشتن برنامه‌های ساده نیازمند هوش و نظم زیادی بود. نوشتن برنامه‌های پیچیده تقریبا قابل تصور نبود.

{{index bit, "wizard (mighty)"}}

البته، وارد کردن این الگوهای بیتی رمزگونه (صفر و یک‌ها) باعث می شد که برنامه نویس احساس کند که جادوگیری چیره‌دست است و حس رضایت شغلی در آن‌ها ایجاد می کرد.

{{index memory, instruction}}

هر خط ار برنامه‌ی قبلی حاوی یک دستور است. می توان‌ آن را در زبان فارسی به صورت زیر نوشت:

 1. عدد 0 را در موقعیت 0 حافظه ذخیره کن.
 2. عدد 1 را در موقعیت 1 در حافظه ذخیره کن.
 3. مقدار موجود در موقعیت 1 حافظه را در موقعیت 2 حافظه ذخیره کن.
 4. عدد 11 را از مقداری که در موقعیت 2 در حافظه قرار دارد، تفریق کن.
 5. اگر مقدار موجود در موقعیت 2 در حافظه برابر با 0 است، به سراغ دستور شماره 9 برو.
 6. مقدار موجود در موقعیت 1 حافظه را به مقدار موجود در موقعیت 0 حافظه اضافه نما.
 7. عدد 1 را به مقدار موجود در موقعیت 1 حافظه اضافه نما.
 8. دستور شماره 3 را اجرا کن.
 9. مقدار موجود در موقعیت 0 حافظه را در خروجی قرار بده.

{{index readability, naming, binding}}

اگرچه نوشته بالا از سوپ بیت‌ها خواناتر است، ولی همچنان نامفهوم و مبهم است. استفاده از نام‌ها به جای اعداد برای دستورات و موقعیت‌های حافظه کمک‌کننده است.

```{lang: "text/plain"}
 Set “total” to 0.
 Set “count” to 1.
[loop]
 Set “compare” to “count”.
 Subtract 11 from “compare”.
 If “compare” is zero, continue at [end].
 Add “count” to “total”.
 Add 1 to “count”.
 Continue at [loop].
[end]
 Output “total”.
```

{{index loop, jump, "summing example"}}

می توانید بگویید برنامه چگونه کار می کند؟ در دو خط اول به دو مکان در حافظه مقدارهای اولیه اختصاص داده می شود: `total` برای ساختن نتیجه محاسبه استفاده می شود و `count` عددی را که در حال حاضر در دست داریم را ردگیری می کند. خطوطی که از `compare` استفاده می کنند احتمالا مبهم ترین خطوط به نظر می آیند. برنامه می خواهد بررسی کند آیا `count` برابر با 11 است تا در مورد توقف اجرای برنامه تصمیم بگیرد. با توجه به اینکه ماشین فرضی ما نسبتا ابتدایی می باشد، فقط می تواند صفر بودن یک عدد را آزمایش کند و بر اساس آن تصمیم بگیرد.  بنابراین از موقعیتی در حافظه که برچسب `compare` دارد برای محاسبه مقدار `count - 11` استفاده می کند و بر اساس آن مقدار تصمیم می گیرد. دو خط بعدی مقدار `count` را به نتیجه محاسبه جمع کرده و مقدار `count` را بعد از هربار که برنامه متوجه شد که مقدار `count` هنوز ‍‍11 نیست، یک واحد افزایش می دهد.

همین برنامه در جاوااسکریپت به صورت زیر خواهد بود:


```
let total = 0, count = 1;
while (count <= 10) {
  total += count;
  count += 1;
}
console.log(total);
// → 55
```

{{index "while loop", loop, [braces, block]}}

این نسخه کمی بهتر شده است. مهم تر از همه، نیازی نیست نحوه‌ی انتقال برنامه بین دستورات را مشخص کنیم.  ساختار `while` این وظیفه را به عهده می گیرد. این ساختار، بلوک کد (بین کروشه‌ها) زیرینش را تا زمانی که شرطش برقرار باشد اجرا می کند. این شرط همان <bdo>`count <= 10`</bdo> می باشد به معنای "_count_ مساوی یا کوچکتر از 10 باشد" است. دیگر نیازی نیست که مقداری موقت ایجاد کنیم و آن را با صفر مقایسه کنیم، که کار جالبی نبود. بخشی از قدرت زبان‌های برنامه‌نویسی این است که این گونه جزئیات اضافی را پوشش می دهند.

{{index "console.log"}}

در پایان برنامه، بعد از اینکه ساختار `while` به اتمام رسید، دستور `console.log` برای قراردادن نتیجه در خروجی استفاده می شود.

{{index "sum function", "range function", abstraction, function}}

در نهایت، اگر شانس استفاده از دستورات  `range` و `sum` را داشتیم، که به ترتیب برای ایجاد مجموعه‌ای از اعداد در یک بازه‌ و محاسبه‌ی جمع یک مجموعه اعداد استفاده می شوند، برنامه به شکل زیر نوشته می شد:

```{startCode: true}
console.log(sum(range(1, 10)));
// → 55
```

{{index readability}}

درسی که از این داستان می شود گرفت این است که یک برنامه‌ی یکسان را می توان به دو صورت طولانی و کوتاه ، ناخوانا و خوانا نوشت. اولین نسخه‌ی این برنامه بسیار گنگ بود در حالیکه که آخرین نسخه‌ی آن تقریبا به زبان انگلیسی نوشته شده است: <bdo>`log` the `sum` of the `range` of numbers from 1 to 10.</bdo> (مجموع اعداد بازه‌ی 1  تا 10 را بنویس.). در [فصل‌های بعد](data) خواهیم دید که چگونه دستوراتی مثل `sum` و `range` را خودمان تعریف کنیم.

{{index ["programming language", "power of"], composability}}

یک زبان برنامه‌نویسی خوب با فراهم نمودن امکان اجرای دستوراتی در سطح بالا، به برنامه‌نویس کمک می کند. باعث می شود که جزئیات به کم‌اهمیت کنار گذاشته شوند، بلوک‌های سازنده‌ی مناسب فراهم می کند (مانند `while` و `console.log`)،به شما اجازه می دهد تا بلوک‌های سازنده‌ی خودتان را تعریف کنید (مانند `sum` و `range`) و نوشتن این بلاک‌ها را آسان می نماید.

## جاوااسکریپت چیست؟

{{index history, Netscape, browser, "web application", JavaScript, [JavaScript, "history of"], "World Wide Web"}}

{{indexsee WWW, "World Wide Web"}}

{{indexsee Web, "World Wide Web"}}

جاوااسکریپت در سال 1995 به عنوان روشی برای افزودن برنامه‌ها به صفحات وب در مرورگر Netscape Navigator معرفی شد. این زبان بعد از آن توسط همه‌ی مرورگرهای وب گرافیکی به خدمت گرفته شده است. جاوااسکریپت باعث شده تا ساخت برنامه‌های وب مدرن ممکن شود، برنامه‌هایی که می توانید بدون نیاز به بارگیری مجدد صفحه وب برای هر کار، با آن‌ها تعامل برقرار کنید. جاوااسکریپت همچنین به صورت سنتی در وبسایت‌های بیشتری برای انجام اشکال متنوعی از تعامل و هوشمندی بکار گرفته می شود.

{{index Java, naming}}

لازم است گفته شود که جاوااسکریپت تقریبا هیچ ربطی به زبان برنامه‌نویسی جاوا ندارد. از نام جاوا برای ملاحظات بازاریابی استفاده شد نه ارتباط آن ها. در زمان معرفی جاوااسکریپت، زبان جاوا به شدت تبلیغ می شد و محبوبیت زیادی هم کسب می کرد. با قرض گرفتن این نام ظاهرا قصد سوار شدن بر این موفقیت مورد نظر بوده است. اکنون همین نام جا افتاده است.

{{index ECMAScript, compatibility}}

بعد از بکارگیری جاوااسکریپت خارج از Netscape، یک سند استاندارد نوشته شد تا نحوه‌ای که زبان جاوااسکریپت بایستی کار کند توصیف شود و در نتیجه نرم‌افزارهای متنوعی که قصد پشتیبانی از این زبان را دارند، همه به زبان یکسانی اشاره کنند. به این استاندارد، استاندارد ECMAScript گفته می شود. این نام گذاری بعد از اینکه سازمان بین‌المللی Ecma که کار استانداردسازی انجام داد، صورت گرفت. در عمل، اصطلاح ECMAScript و JavaScript را می توان به جای هم به کار برد - هر دو نام به یک زبان اشاره می کند.

{{index [JavaScript, "weaknesses of"], debugging}}

افرادی هستند که چیزهای ناخوش‌آیندی نسبت به جاوااسکریپت می گویند. خیلی از این چیزها درست هستند. زمانی که لازم داشتم تا چیزی را به زبان جاوااسکریپت برای اولین بار بنویسم، خیلی سریع به سراغ سرزنش آن رفتم. تقریبا هر چیزی که تایپ می کردم قبول می‌کرد اما به صورتی تفسیر می نمود که کاملا با چیزی انتظارش را می کشیدم متفاوت بود. درسته که این رفتار به این واقعیت که من اطلاعی از نحوه‌ی عملکرد زبان نداشتم ربط زیادی داشت، اما یک مسئله در جاوااسکریپت واقعیت دارد: جاوااسکریپت به طرز خنده‌داری در کارهایی که مجاز می شمرد روشن‌فکر است. ایده‌ی پشت این نوع طراحی این بوده است که برنامه‌نویسی در جاوااسکریپت را برای تازه‌کارها آسان‌تر کنند. در واقعیت، اتفاقی که افتاده این است که پیدا کردن مشکلات برنامه با این کار سخت‌ تر می شود زیرا سیستم مشکلات را به شما نشان نمی دهد.

{{index [JavaScript, "flexibility of"], flexibility}}

البته این انعطاف، مزیت‌هایی نیز به همراه دارد. راه را برای بروز تکنیک‌های زیادی باز می کند که در زبان‌های سخت‌گیر تر ممکن نیست و همانطور که خواهید دید ( مثلا در [فصل ?](modules))، می توان‌ از آن برای پوشش بعضی از اشکالات خود جاوااسکریپت بهره برد.  بعد از یادگیری درست این زبان و سپری کردن مدتی با آن، من آموختم که واقعا جاوااسکریپت رو دوست داشته باشم.

{{index future, [JavaScript, "versions of"], ECMAScript, "ECMAScript 6"}}

نسخه‌های متفاوتی از جاوااسکریپت وجود دارد. نسخه‌ی سوم ECMAScript در زمانی که جاوااسکریپت گوی سبقت را ربود، حدودا بین سال‌های 2000 تا 2010، به صورت گسترده‌ای پشتیبانی می شد. در طول این مدت، کار روی نسخه‌ی جاه‌طلبانه 4 ، در جریان بود. نسخه‌ای که برنامه‌ریزی شده بود تا بهبود‌ها و امکاناتی اساسی به زبان اضافه کند. تغییر اساسی زبانی زنده و پراستفاده، از نقطه‌نظر سیاست کاری مشکل بود و کار روی نسخه‌ی 4 در سال 2008 متوقف شد و منجر به کار روی نسخه‌ی بسیار محافظه‌کارانه‌تر 5 شد، که فقط تغییرات و بهبود‌هایی که محل اختلاف نبودند را در بر داشت، که در سال 2009 منتشر شد. سپس در سال 2015 نسخه‌ی 6 بیرون آمد، یک به‌روز رسانی اساسی که بعضی از آن ایده‌هایی که در نسخه‌ی 4 برنامه‌ریزی شده بودند را در بر داشت. از آن موقع به بعد، هر سال تغییراتی جدید و کوچک را شاهد هستیم.

این واقعیت که جاوااسکریپت در حال تکامل است به این معناست که مرورگرها نیز باید همواره به روز شوند، پس اگر از یک مرورگر قدیمی‌تر استفاده کنید، ممکن است همه‌ی ویژگی‌ها را پشتیبانی نکند. طراحان زبان جاوااسکریپت حواسشان هست که تغییراتی ایجاد نکنند که باعث خراب شدن برنامه‌های موجود بشود، بنابراین مرورگرهای جدید، برنامه‌های قدیمی را نیز به درستی اجرا می نمایند. در این کتاب، من از نسخه‌ی 2017 جاوااسکریپت استفاده می کنم.

{{index [JavaScript, "uses of"]}}

Web browsers are not the only platforms on which JavaScript is used.
Some databases, such as MongoDB and CouchDB, use JavaScript as their
scripting and query language. Several platforms for desktop and server
programming, most notably the ((Node.js)) project (the subject of
[Chapter ?](node)), provide an environment for programming JavaScript
outside of the browser.

## Code, and what to do with it

{{index "reading code", "writing code"}}

_Code_ is the text that makes up programs. Most chapters in this book
contain quite a lot of code. I believe reading code and writing ((code))
are indispensable parts of ((learning)) to program. Try to not just
glance over the examples—read them attentively and understand them.
This may be slow and confusing at first, but I promise that you'll
quickly get the hang of it. The same goes for the ((exercises)). Don't
assume you understand them until you've actually written a working
solution.

{{index interpretation}}

I recommend you try your solutions to exercises in an actual
JavaScript interpreter. That way, you'll get immediate feedback on
whether what you are doing is working, and, I hope, you'll be tempted
to ((experiment)) and go beyond the exercises.

{{if interactive

When reading this book in your browser, you can edit (and run) all
example programs by clicking them.

if}}

{{if book

{{index download, sandbox, "running code"}}

The easiest way to run the example code in the book, and to experiment
with it, is to look it up in the online version of the book at
[_https://eloquentjavascript.net_](https://eloquentjavascript.net/). There,
you can click any code example to edit and run it and to see the
output it produces. To work on the exercises, go to
[_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code),
which provides starting code for each coding exercise and allows you
to look at the solutions.

if}}

{{index "developer tools", "JavaScript console"}}

If you want to run the programs defined in this book outside of the
book's website, some care will be required. Many examples stand on their
own and should work in any JavaScript environment. But code in later
chapters is often written for a specific environment (the browser or
Node.js) and can run only there. In addition, many chapters define
bigger programs, and the pieces of code that appear in them depend on
each other or on external files. The
[sandbox](https://eloquentjavascript.net/code) on the website provides
links to Zip files containing all the scripts and data files
necessary to run the code for a given chapter.

## Overview of this book

This book contains roughly three parts. The first 12 chapters discuss
the JavaScript language. The next seven chapters are about web
((browsers)) and the way JavaScript is used to program them. Finally,
two chapters are devoted to ((Node.js)), another environment to
program JavaScript in.

Throughout the book, there are five _project chapters_, which describe
larger example programs to give you a taste of actual programming. In
order of appearance, we will work through building a [delivery
robot](robot), a [programming language](language), a [platform
game](game), a [pixel paint program](paint), and a [dynamic
website](skillsharing).

The language part of the book starts with four chapters that introduce
the basic structure of the JavaScript language. They introduce
[control structures](program_structure) (such as the `while` word you
saw in this introduction), [functions](functions) (writing your own
building blocks), and [data structures](data). After these, you will
be able to write basic programs. Next, Chapters [?](higher_order) and
[?](object) introduce techniques to use functions and objects to write
more _abstract_ code and keep complexity under control.

After a [first project chapter](robot), the language part of the book
continues with chapters on [error handling and bug fixing](error),
[regular expressions](regexp) (an important tool for working with
text), [modularity](modules) (another defense against complexity), and
[asynchronous programming](async) (dealing with events that take
time). The [second project chapter](language) concludes the first part
of the book.

The second part, Chapters [?](browser) to [?](paint), describes the
tools that browser JavaScript has access to. You'll learn to display
things on the screen (Chapters [?](dom) and [?](canvas)), respond to
user input ([Chapter ?](event)), and communicate over the network
([Chapter ?](http)). There are again two project chapters in this
part.

After that, [Chapter ?](node) describes Node.js, and [Chapter
?](skillsharing) builds a small website using that tool.

{{if commercial

Finally, [Chapter ?](fast) describes some of the considerations that
come up when optimizing JavaScript programs for speed.

if}}

## Typographic conventions

{{index "factorial function"}}

In this book, text written in a `monospaced` font will represent
elements of programs—sometimes they are self-sufficient fragments, and
sometimes they just refer to part of a nearby program. Programs (of
which you have already seen a few) are written as follows:

```
function factorial(n) {
  if (n == 0) {
    return 1;
  } else {
    return factorial(n - 1) * n;
  }
}
```

{{index "console.log"}}

Sometimes, to show the output that a program produces, the
expected output is written after it, with two slashes and an arrow in
front.

```
console.log(factorial(8));
// → 40320
```

Good luck!
