{{meta {load_files: ["code/chapter/16_game.js", "code/levels.js"], zip: "html include=[\"css/game.css\"]"}}}

# Project: یک بازی پرشی

{{quote {author: "Iain Banks", title: "The Player of Games", chapter: true}

All reality is a game.

quote}}

{{index "Banks, Ian", "project chapter", simulation}}

{{figure {url: "img/chapter_picture_16.jpg", alt: "Picture of a game character jumping over lava", chapter: "framed"}}}

بیشتر شیفتگی اولیه من نسبت به کامپیوترها، مثل خیلی از بچه‌های دیگر، از بازی‌های
کامپیوتری شروع شد. من￼ جذب دنیاهای شبیه‌سازی شده‌ی کوچکی شدم که می توانستم آن‌ها
را اداره کنم و در آن‌ها داستان‌هایی گشوده می‌شد – گمان می کنم بیشتر به خاطر گسترش
تخیلاتم به درون بازی‌ها بود تا امکانات و قابلیت‌های خود بازی ها.

من برای هیچ کس حرفه‌ی برنامه نویسی بازی‌های کامپیوتر را آرزو نمی کنم. بسیار شبیه
به صنعت موسیقی، اختلاف زیاد بین تعداد زیاد افرادی که دوست دارند در آن کار کنند و
تقاضای واقعی برای آن‌ها، باعث ایجاد محیط نسبتا نامناسبی می شود. اما نوشتن بازی‌ها
برای تفریح کاری دلچسب است.

{{index "jump-and-run game", dimensions}}

در این فصل به سراغ پیاده‌سازی یک بازی پرشی (سکوبازی) کوچک می رویم. سکوبازی‌ها (یا بازی‌های
حرکت و پرش)، بازی‌هایی هستند که بازیکن باید یک شخصیت را در جهان بازی حرکت دهد که
معمولا این جهان دو بعدی است و از کنار نمایش داده می شود و شخصیت از روی (و درون)
چیزها می تواند بپرد.

## بازی

{{index minimalism, "Palef, Thomas", "Dark Blue (game)"}}

بازی ما به طور کلی بر پایه‌ی بازی [Dark
Blue](http://www.lessmilk.com/games/10)[
(_www.lessmilk.com/games/10_)]{if book}
که توسط توماس پالف ساخته شده خواهد بود. من این بازی را انتخاب کردم به دلیل اینکه
هم سرگرم‌کننده و هم ساده است، و نیازی نیست برای نوشتن آن کدنویسی خیلی زیادی انجام
شود. بازی به شکل زیر خواهد بود.

{{figure {url: "img/darkblue.png", alt: "The game Dark Blue"}}}

{{index coin, lava}}

مستطیل تیره رنگ نمایانگر شخصیت بازی است که وظیفه‌اش جمع آوری مستطیل های زرد
(سکه‌ها) بدون برخورد با چیزهای قرمز رنگ (گدازه‌ها) است. یک مرحله بازی زمانی کامل
می شود که تمامی سکه‌ها جمع آوری شده باشند.

{{index keyboard, jumping}}

بازیکن می تواند به وسیله‌ی کلیدهای چپ و راست صفحه‌کلید جابجا شود و با فشردن کلید بالا،
بپرد. پریدن توانایی خاص این کاراکتر است. می تواند چندین برابر قد خودش بپرد و در
هوا جهتش را عوض کند. بازی به طور کامل واقع‌گرایانه نیست اما به بازیکن این حس را
القا می کند که کاملا شخصیت بازی تحت کنترلش حرکت می کند.

{{index "fractional number", discretization, "artificial life", "electronic life"}}

این بازی از یک پیش زمینه‌ی ثابت تشکیل شده است که مثل یک grid طرح بندی شده است
همراه با عناصر متحرکی که روی پیش‌زمینه قرار گرفته اند. هر فیلد از grid یا خالی، یا
رنگ‌شده یا یک گدازه است. عناصر متحرک شامل بازیکنان، سکه‌ها و بعضی از گدازه‌ها می
شوند. موقعیت این عناصر محدود به grid نیست. – ممکن است مختصاتشان اعشاری باشد که
باعث حرکت نرم‌تر آن‌ها خواهد شد.

## تکنولوژی مورد استفاده

{{index "event handling", keyboard, [DOM, graphics]}}

ما از DOM مرورگر برای نمایش بازی استفاده می کنیم و ورودی کاربر را مدیریت
رخدادهای کلیدها خواهیم خواند.

{{index rectangle, "background (CSS)", "position (CSS)", graphics}}

کدهای مربوط به صفحه‌ی نمایش و صفحه‌کلید فقط بخشی کوچکی از کاری که برای ساخت این
بازی لازم است را شامل می شوند. به دلیل این که همه چیز شبیه به مستطیل‌های رنگی
است، مشکل طراحی نداریم: عناصر DOM را ایجاد کرده و با استفاده از سبک‌دهی به آن‌ها
رنگ پیش‌زمینه ،اندازه و موقعیت می دهیم.

{{index "table (HTML tag)"}}

می توانیم پیش‌زمینه را به شکل یک جدول نمایش دهیم چرا که این gridای ثابت از
چهارگوش‌ها است. عناصری که آزادانه حرکت می کنند را می توان با موقعیت‌دهی مطلق روی طرح
قرار داد.

{{index performance, [DOM, graphics]}}

در بازی‌ها و دیگر برنامه‌هایی که در آن‌ها باید تصاویر به حرکت درآیند و به ورودی کاربر بدون
تاخیر پاسخ دهند، کارایی خیلی مهم است. اگرچه DOM اساسا برای انجام کارهای گرافیکی
سطح بالا طراحی نشده است، اما در عمل از چیزی که انتظارش را داشتید بهتر کار می
کند. در [فصل
?](dom#animation) چند متحرک‌سازی مشاهده نمودید. در یک کامپیوتر مدرن، یک بازی ساده مثل
این بازی به خوبی اجرا می شود، حتی اگر به بهینه‌سازی آن زیاد فکر نکنیم.

{{index canvas, [DOM, graphics]}}

در [فصل بعدی](canvas) به سراغ تکنولوژی دیگری در مرورگر خواهیم رفت، برچسب <bdo>`<canvas>`</bdo> که روش
ریشه‌دارتری برای کشیدن تصاویر فراهم می سازد؛ کار با اشکال و پیکسل‌ها به جای استفاده از عناصر DOM.

## مراحل بازی

{{index dimensions}}

ما به روشی خوانا و قابل ویرایش برای انسان نیاز داریم تا به وسیله‌های آن مراحل
بازی را مشخص کنیم. چون می توان همه چیز را روی grid شروع کرد، می‌توانیم از رشته‌های
بلند که در آن هر کاراکتر نماینده‌ی یک عنصر است استفاده کنیم –چه بخشی از grid
پیش‌زمینه یا یک عنصر در حال حرکت.

طرح مورد نظر برای یک مرحله‌ی کوچک ممکن است شبیه زیر باشد:

```{includeCode: true}
let simpleLevelPlan = `
......................
..#................#..
..#..............=.#..
..#.........o.o....#..
..#.@......#####...#..
..#####............#..
......#++++++++++++#..
......##############..
......................`;
```

{{index level}}

نقطه‌ها نماینده‌ی فضاهای خالی، کاراکترهای هش (`#`) معرف دیوارها و علامتهای مثبت
نماینده‌ی گدازه‌ها می باشند. نقطه شروع بازیکن با علامت `@` مشخص شده است. هر کاراکتر
O نماینده‌ی یک سکه است و علامت مساوی (`=`) در بالا یک بلاک از گدازه است که به صورت
افقی جلوعقب می رود.

{{index bouncing}}

دو نوع دیگر از گدازه‌های متحرک را پشتیبانی خواهیم کرد: یک کاراکتر پایپ (`|`)
گلوله‌های متحرک عمودی ایجاد می کند و `v` نشان‌دهنده گدازه‌هایی است که چکیده می شوند
– گدازه‌های متحرک عمودی که بالا پایین نمی روند بلکه فقط به￼ سمت پایین حرکت می کنند و
وقتی به زمین رسیدن به نقطه‌ی اولشان بر می گردند.

کل بازی شامل چندین مرحله می شود که بازیکن باید به اتمام برساند. وقتی همه‌ی سکه‌ها
جمع‌آوری شد یک مرحله به اتمام می رسد. اگر بازیکن با گدازه برخورد کند ، مرحله‌ی
کنونی به ابتدا برخواهد گشت و بازیکن می تواند دوباره تلاش کند.

{{id level}}

## خواندن یک مرحله

{{index "Level class"}}

کلاس پیشرو یک شیء مرحله را ذخیره می کند. آرگومان آن باید رشته‌ای باشد که مرحله را
تعریف می کند.

```{includeCode: true}
class Level {
  constructor(plan) {
    let rows = plan.trim().split("\n").map(l => [...l]);
    this.height = rows.length;
    this.width = rows[0].length;
    this.startActors = [];

    this.rows = rows.map((row, y) => {
      return row.map((ch, x) => {
        let type = levelChars[ch];
        if (typeof type == "string") return type;
        this.startActors.push(
          type.create(new Vec(x, y), ch));
        return "empty";
      });
    });
  }
}
```

{{index "trim method", "split method", [whitespace, trimming]}}

متد `trim` در اینجا برای حذف فضاهای خالی شروع و پایان رشته‌ی طرح استفاده می شود.
این به طرح مثال ما این امکان را می دهد که در یک خط جدید شروع شود تا همه‌ی خطوط
مستقیما زیر یکدیگر قرار بگیرند. رشته‌ی باقیمانده براساس کاراکترهای خط جدید تقسیم
می شود و هر خط درون یک آرایه پخشی می شود و آرایه‌ای از کاراکترها تولید می شود.

{{index [array, "as matrix"]}}

بنابراین `rows` آرایه‌ای از آرایه‌های کاراکترها را نگهداری می کند، همان ردیف‌های
طرح. می توانیم طول و عرض هر مرحله را از این ها بدست بیاوریم. اما هنوز لازم است
که عناصر متحرک را از grid پیش‌زمینه جدا کنیم. عناصر متحرک را بازیگران می نامیم.
آن را در آرایه‌ای از اشیاء ذخیره می کنیم. پیش‌زمینه آرایه‌ای از آرایه‌های رشته‌ها
خواهد بود که نوع فیلدهایی مثل `"empty"`، `"wall"`، یا `"lava"` را نگهداری می کند.

{{index "map method"}}

برای ایجاد این آرایه‌ها به سراغ تک تک ردیف ها و بعد محتوای آن‌ها می رویم. به
خاطر داشته باشید که متد `map` اندیس آرایه را به عنوان آرگومان دوم به تابع
ارسال می کند که به ما مختصات x و y کاراکتر داده‌شده را می دهد. موقعیت ها در این
بازی به صورت جفت‌هایی از مختصات با مبدا بالا و چپ <bdo>0,0</bdo> ذخیره می شوند و هر مربع
پیش زمینه دارای 1 واحد طول و عرض می باشد.

{{index "static method"}}

برای تفسیر کاراکترهای موجود در طرح، تابع سازنده‌ی `Level` از شیء `levelChars` استفاده می
کند که عناصر پیش‌زمینه را به رشته‌ها و بازیگران را به کلاس‌ها نگاشت می کند. زمانی
که `type` یک کلاس بازیگر است، متد استاتیک `create` آن برای ایجاد یک شیء استفاده می
شود که به `startActors` افزوده می شود و تابع map مقدار `"empty"` را برای این مربع
پیش‌زمینه برمی گرداند.


{{index "Vec class"}}

موقعیت بازیگر به عنوان یک شیء `Vec` ذخیره می شود که یک بردار دوبعدی است،
شیءای با خاصیت‌های `x` و `y`، همانطور که در قسمت تمرین‌ها [فصل ?](object#exercise_vector) مشاهده شد.


{{index [state, in objects]}}

با اجرای بازی، بازیگران در مکان‌های متفاوتی قرار می گیرند یا حتی به طور کامل
ناپدید می شوند (همانطور که سکه‌ها در صورت جمع‌آوری ناپدید می شوند). ما از یک کلاس
`State` برای رصد وضعیت بازی در حال اجرا استفاده می کنیم.

```{includeCode: true}
class State {
  constructor(level, actors, status) {
    this.level = level;
    this.actors = actors;
    this.status = status;
  }

  static start(level) {
    return new State(level, level.startActors, "playing");
  }

  get player() {
    return this.actors.find(a => a.type == "player");
  }
}
```

زمانی که بازی به اتمام می رسد، خاصیت `status` به `"lost"` یا `"won"` تغییر می
کند .

این دوباره یک ساختار داده‌ی مانا محسوب می شود – به روز رسانی وضعیت بازی باعث
ایجاد وضعیت جدیدی می شود و وضعیت قبلی را دست‌نخورده باقی می گذارد.

## بازیگران

{{index actor, "Vec class", [interface, object]}}

اشیاء بازیگر نمایانگر موقعیت و وضعیت یک عنصر متحرک در بازی ما می‌باشند. تمامی اشیاء
بازیگر از رابط یکسانی پیروی می کنند. خاصیت `pos` آن‌ها، مختصات گوشه‌ی بالا-چپ عنصر
را نگهداری کرده و خاصیت `size` آن‌ها اندازه‌شان را نگه‌داری می کند.

آن‌ها نیز دارای یک متد `update` می باشند که برای محاسبه‌ی وضعیت و موقعیت جدیدشان
بعد از یک گام زمانی داده شده است. این متد کاری که یک بازیگر انجام می دهد را
شبیه‌سازی می کند- حرکت در پاسخ به کلیدهای جهت دار برای بازیکن، حرکت جلو و عقب
برای گدازه‌ها – و یک شیء بازیگر جدید و به‌روز بر می گرداند.


یک خاصیت `type` حاوی رشته‌ای است که نوع بازیگر را مشخص می کند – `“player”،` `“coin”` یا
`“lava”`. در هنگام کشیدن طرح بازی این خاصیت مفید خواهد بود. – شکل مستطیلی که برای
یک بازیگر کشیده می شود بر اساس نوعش می باشد.

کلاس‌های بازیگر دارای یک متد استاتیک به نام `create` هستند که به وسیله‌ی سازنده‌ی
`Level` برای ایجاد یک بازیگر از یک کاراکتر موجود در طرح مرحله استفاده می شود. به
آن مختصات کاراکتر و خود کاراکتر داده می شود، که ضروری است زیرا کلاس `Lava`
کاراکترهای متعددی را رسیدگی می کند.

{{id vector}}

برای مقادیر دوبعدی از کلاس `Vec` استفاده می کنیم مثل موقعیت و اندازه‌ی بازیگران.

```{includeCode: true}
class Vec {
  constructor(x, y) {
    this.x = x; this.y = y;
  }
  plus(other) {
    return new Vec(this.x + other.x, this.y + other.y);
  }
  times(factor) {
    return new Vec(this.x * factor, this.y * factor);
  }
}
```

{{index "times method", multiplication}}

متد `times` با توجه به عدد دریافتی اندازه‌ی یک بردار (vector) را تغییر می دهد. زمانی که
لازم است تا یک بردار سرعت￼ را در یک وقفه‌ی زمان ضرب کنیم تا فاصله‌ی پیموده شده را در
طول آن زمان به دست بیاوریم، مفید خواهد بود.

انواع مختلف بازیگران دارای کلاس‌های خودشان می باشند، به دلیل اینکه رفتارهایشان
خیلی متفاوت است. اجازه بدهید این کلاس ها را تعریف کنیم. بعدا به متدهای `update`
شان خواهیم پرداخت.

{{index simulation, "Player class"}}

کلاس `Player` دارای خاصیتی به نام `speed` است که سرعت فعلی اش را ذخیره می کند تا
جاذبه و تکانه (momentum) را شبیه‌سازی کند.

```{includeCode: true}
class Player {
  constructor(pos, speed) {
    this.pos = pos;
    this.speed = speed;
  }

  get type() { return "player"; }

  static create(pos) {
    return new Player(pos.plus(new Vec(0, -0.5)),
                      new Vec(0, 0));
  }
}

Player.prototype.size = new Vec(0.8, 1.5);
```
چون یک بازیکن یک و نیم برابر یک مربع ارتفاع دارد، موقعیت اولیه‌ی آن برابر با نصف
مربع بالای موقعیتی که کاراکتر `@` ظاهر می شود تنظیم می شود. با این کار، قسمت پایین
آن با قسمت پایین مربعی که در آن ظاهر می شود تراز خواهد شد.


خاصیت `size` برای همه‌ی نمونه‌های گرفته شده از `Player` یکسان است پس می توان آن را به
جای ذخیره در نمونه‌ها در prototype ذخیره کرد. می توانستیم از یک getter
مثل `type` استفاده کنیم اما در این صورت یک شیء `Vec` جدید هر بار که خاصیت خوانده می
شد ایجاد و برگردانده می شد که کاری بیهوده است. (رشته‌ها با توجه به غیرقابل
تغییر بودن، نیازی ندارند با هر بار ارزیابی از نو ایجاد شوند).

{{index "Lava class", bouncing}}

زمانی که یک بازیگر `Lava` را می سازیم، لازم است که شیء با توجه با کاراکتری که بر
پایه‌ی آن است مقداردهی متفاوتی شود. گدازه‌ی پویا با سرعت فعلی‌اش حرکت می کند تا
زمانی که به یک مانع بخورد. در این نقطه، اگر دارای خاصیت `reset` باشد، به موقعیت
اولیه‌اش برمی‌گردد (dripping). اگر نداشت، سرعتش معکوس شده و درجهت مخالف به حرکت
ادامه می دهد (bouncing).

متد `create` به کاراکترهایی که سازنده‌ی `Level` ارسال می کند نگاه کرده و بازیگران
گدازه‌ی مناسب ایجاد می کند.

```{includeCode: true}
class Lava {
  constructor(pos, speed, reset) {
    this.pos = pos;
    this.speed = speed;
    this.reset = reset;
  }

  get type() { return "lava"; }

  static create(pos, ch) {
    if (ch == "=") {
      return new Lava(pos, new Vec(2, 0));
    } else if (ch == "|") {
      return new Lava(pos, new Vec(0, 2));
    } else if (ch == "v") {
      return new Lava(pos, new Vec(0, 3), pos);
    }
  }
}

Lava.prototype.size = new Vec(1, 1);
```

{{index "Coin class", animation}}

بازیگران `Coin` نسبتا ساده هستند. بیشترشان فقط در جای خود ثابت هستند. اما برای
اینکه کمی به بازی پویایی اضافه کنیم، آن‌ها را در جا حرکت می دهیم. برای انجام این
کار، یک شیء سکه موقعیت پایه‌ای را به همراه یک خاصیت `wobble` که حرکت درجا را رصد می
کند ذخیره می کند. این دو با هم موقعیت واقعی سکه را مشخص می کنند (که در خاصیت
`pos` حفظ می شوند).

```{includeCode: true}
class Coin {
  constructor(pos, basePos, wobble) {
    this.pos = pos;
    this.basePos = basePos;
    this.wobble = wobble;
  }

  get type() { return "coin"; }

  static create(pos) {
    let basePos = pos.plus(new Vec(0.2, 0.1));
    return new Coin(basePos, basePos,
                    Math.random() * Math.PI * 2);
  }
}

Coin.prototype.size = new Vec(0.6, 0.6);
```

{{index "Math.random function", "random number", "Math.sin function", sine, wave}}

در [فصل ?](dom#sin_cos)، دیدیم که متد <bdo>`Math.sin`</bdo> مختصات عرضی نقطه‌ای
روی دایره را به ما می دهد. مقدار این مختصات با حرکت در محیط دایره به صورت موجی
در یک بازه بالا و پایین می رود که موجب می شود تابع سینوس گزینه‌ی خوبی برای
مدلسازی حرکت موجی برای ما باشد.

{{index pi}}

برای جلوگیری از حالتی که همه‌ی سکه‌ها همزمان بالا و پایین بروند، فاز شروع هر سکه
به صورت تصادفی تعیین می شود. فاز موج <bdo>`Math.sin`</bdo> همان عرض موجی است که تولید می کند
و برابر با 2π می باشد. مقدار بازگشتی از <bdo>`Math.random`</bdo> را در آن عدد ضرب کرده تا
موقعیت شروع تصادفی ای به سکه روی موج بدهیم.

{{index map, [object, "as map"]}}

اکنون می توانیم شیء `levelChars` را تعریف کنیم که کاراکترهای طرح را روی انواع grid
پیش‌زمینه یا کلاس‌های بازیگر نگاشت کند.

```{includeCode: true}
const levelChars = {
  ".": "empty", "#": "wall", "+": "lava",
  "@": Player, "o": Coin,
  "=": Lava, "|": Lava, "v": Lava
};
```

این به ما تمامی بخش‌های لازم برای ایجاد نمونه‌ی `Level` را می دهد.

```{includeCode: strip_log}
let simpleLevel = new Level(simpleLevelPlan);
console.log(`${simpleLevel.width} by ${simpleLevel.height}`);
// → 22 by 9
```

کار باقی مانده این است که این مرحله‌ها را روی صفحه‌ی نمایش نشان دهیم و زمان و حرکت
را درون آن مدلسازی کنیم.

## کپسوله‌سازی به عنوان یک بار

{{index "programming style", "program size", complexity}}

بیشتر کدهای این فصل بدون در نظر گرفته کپسوله سازی نوشته شده اند و این کار دو
دلیل دارد. اول اینکه کپسولهسازی کار بیشتری از ما می گیرد. باعث بزرگتر شدن برنامه
می شود و نیاز به طرح مفایهم و رابط های بیشتری دارد. به دلیل این که نمی توان در
اینجا کد زیادی به نمایش گذاشت و برای خواننده کسل کننده خواهد شد، من تلاش کردم که
که برنامه را کوچک نگه دارم.

{{index [interface, design]}}

دوما، عناصر متنوع درون این بازی با هم ارتباط تنگاتنگی دارند و اگر رفتار یکی از
آن ها تغییر کند، بعید است که دیگر عناصر بتوانند به همان صورت قبلی بمانند.
رابط‌های بین این عناصر ممکن است به اینجا ختم شود که فرض‌های زیادی درباره‌ی نحوه‌ی
عملکرد بازی در نظر بگیرند. این باعث می شود که اثرگذاری این رابط‌ها بسیار کاهش
یابد- هربار که بخشی از سیستم را تغییر می دهید، همچنان بایستی نگران نحوه‌ی اثر آن
روی دیگر قسمت‌ها باشید چراکه رابط‌های آن ها شرایط جدید را پوشش نداده اند.


بعضی نقاط قابل برش در سیستم _((cutting point))s_ خودشان مناسب قرارگرفته به عنوان
قسمت‌های مجزا￼ توسط رابط‌های دقیق هستند، اما دیگر قسمت‌ها این طور نیستند. تلاش در
جهت کپسوله‌سازی چیزی که کرانه‌ی مناسبی محسوب نمی شود، روش مطمئنی برای تلف کردن
انرژی زیادی است. زمانی که مرتکب این اشتباه می شوید معمولا متوجه می شوید که رابط
شما به شکل نامناسبی بزرگ و دارای جزئیات می شود و اغلب با تکامل برنامه، لازم است
تغییر کند.


{{index graphics, encapsulation, graphics}}

یک چیز هست که قصد داریم تا کپسوله‌سازی کنیم و آن طراحی زیرسیستم است. دلیل این کار
این است که ما بازی را به روش متفاوتی در فصل آینده قرار است نمایش دهیم. با قرار
دادن عمل طراحی پشت یک رابط، می توانیم همین برنامه‌ی بازی را آنجا بارگیری کرده و
ماژول نمایش جدیدی را به خدمت بگیریم.


{{id domdisplay}}

## رسم

{{index "DOMDisplay class", [DOM, graphics]}}

عمل کپسوله کردن کد ((رسم اشکال)) با تعریف یک شیء _((display))_ انجام می شود که وضعیت و مرحله‌ی
داده شده را نمایش می دهد. نوع displayای که در این فصل تعریف می کنیم
`DOMDisplay` خوانده می شود به دلیل این که از عناصر DOM برای نمایش مرحله استفاده می
شود.

{{index "style attribute", CSS}}

ما از یک برگه‌ی سبک‌دهی (css) برای تنظیم رنگ‌های واقعی و دیگر خاصیت‌های ثابت عناصر سازنده‌ی
بازی استفاده می کنیم. همچنین می توان مستقیما خاصیت `style` عناصر را بعد از
ایجادشان مقداردهی کرد اما این کار برنامه‌ها را بی‌نظم و شلوغ می‌کند.

{{index "class attribute"}}

تابع کمکی زیر روشی مختصر برای ایجاد یک عنصر و اختصاص چند خصیصه و گره‌ی فرزند
فراهم می کند.

```{includeCode: true}
function elt(name, attrs, ...children) {
  let dom = document.createElement(name);
  for (let attr of Object.keys(attrs)) {
    dom.setAttribute(attr, attrs[attr]);
  }
  for (let child of children) {
    dom.appendChild(child);
  }
  return dom;
}
```
یک display به این صورت ایجاد می‌شود که به آن عنصر والدی اختصاص داده می‌شود که
باید خودش و یک شیء مرحله را به آن اضافه کند.

```{includeCode: true}
class DOMDisplay {
  constructor(parent, level) {
    this.dom = elt("div", {class: "game"}, drawGrid(level));
    this.actorLayer = null;
    parent.appendChild(this.dom);
  }

  clear() { this.dom.remove(); }
}
```

{{index level}}

grid پیش‌زمینه‌ی مرحله، که همیشه ثابت است، فقط یک بار رسم می شود. بازیگران اما
با هر بار تغییر صفحه نمایش توسط یک وضعیت جدید از نو رسم می شوند. خاصیت
`actorLayer` برای رصد عنصری که بازیگران را نگهداری می کند استفاده می شود تا آن ها
بتوانند به آسانی تغییر و حذف شوند.

{{index scaling, "DOMDisplay class"}}

مختصات و اندازه‌ها ما در واحدهای grid اندازه‌گیری می شوند، برای اندازه یا فاصله 1
به معنای یک بلاک grid است. زمانی که اندازه‌ها پیکسلی را تنظیم می کنیم، می بایست
مقیاس این مختصات را افزایش دهیم – اگر برای هر مربع یک پیکسل در نظر بگیریم همه‌ی
عناصر بازی به شدت کوچک می شوند. ثابت `scale` تعداد پیکسل معادل یک واحد در صفحه‌ی
نمایش را تعیین می کند.

```{includeCode: true}
const scale = 20;

function drawGrid(level) {
  return elt("table", {
    class: "background",
    style: `width: ${level.width * scale}px`
  }, ...level.rows.map(row =>
    elt("tr", {style: `height: ${scale}px`},
        ...row.map(type => elt("td", {class: type})))
  ));
}
```

{{index "table (HTML tag)", "tr (HTML tag)", "td (HTML tag)", "spread operator"}}

همانطور که قبلا ذکر شد، پیش‌زمینه به عنوان یک عنصر <bdo>`<table>`</bdo> رسم می شود. این عنصر
به ساختار خاصیت￼ `rows` مربوط به مرحله به خوبی هماهنگی دارد – هر ردیف از grid به
یک ردیف جدول (<bdo>`<tr>`</bdo>) تبدیل می شود. رشته‌های موجود در گرید به عنوان نام‌های کلاس
برای سلول‌های جدول (<bdo>`<td>`</bdo>) استفاده می شوند. عملگر توزیع (سه‌نقطه) برای ارسال
آرایه‌ی گره‌های فرزند به `elt` به عنوان آرگومان‌های مجزا استفاده می شود.

{{id game_css}}

کد CSS زیر موجب می شود که جدول شبیه پیش‌زمینه‌ای که دوست داریم بشود:



```{lang: "text/css"}
.background    { background: rgb(52, 166, 251);
                 table-layout: fixed;
                 border-spacing: 0;              }
.background td { padding: 0;                     }
.lava          { background: rgb(255, 100, 100); }
.wall          { background: white;              }
```

{{index "padding (CSS)"}}

بعضی از این خاصیت‌ها (<bdo>`table-layout`</bdo>،<bdo>`border-spacing`</bdo> و
`padding`) برای تغییر رفتارهای پیشفرض ناخواسته است. ما نمی خواهیم که قالب جدول
وابسته به محتوای خانه‌هایش باشد و دوست نداریم بین خانه‌های جدول فاصله باشد یا
درونشان padding داشته باشند.

{{index "background (CSS)", "rgb (CSS)", CSS}}

دستور `background` رنگ پیش‌زمینه را تنظیم می کند. در CSS می توان رنگ را هم با
نامشان (white) و هم با فرمت‌های مثل <bdo>`rgb(R, G, B)`</bdo> که سه رنگ اصلی قرمز، سبز و آبی
که تشکیل دهنده رنگ هستند با اعدادی بین 0 تا 255 مشخص می شوند. براین اساس، در <bdo>`rgb(52, 166,
251)`</bdo> قرمز برابر 52، سبز 166 و آبی 251 می باشد. چون قسمت آبی بیشترین عدد
را دارد نتیجه رنگی متمایل به آبی خواهد بود. می توانید آن را در دستور <bdo>`.lava`</bdo>
مشاهده کنید، که در آنجا اولین عدد (قرمز) بزرگترین عدد است.

{{index [DOM, graphics]}}

ما هر بازیگر را با ایجاد یک عنصر DOM برای آن رسم کردیم و موقعیت و اندازه‌ی آن
عنصر را بر اساس خاصیت‌های بازیگر مورد نظر تنظیم کردیم. مقادیر باید در `scale` ضرب
شوند تا از واحدهای بازی به پیکسل تبدیل شوند.

```{includeCode: true}
function drawActors(actors) {
  return elt("div", {}, ...actors.map(actor => {
    let rect = elt("div", {class: `actor ${actor.type}`});
    rect.style.width = `${actor.size.x * scale}px`;
    rect.style.height = `${actor.size.y * scale}px`;
    rect.style.left = `${actor.pos.x * scale}px`;
    rect.style.top = `${actor.pos.y * scale}px`;
    return rect;
  }));
}
```

{{index "position (CSS)", "class attribute"}}

برای اینکه به یک عنصر بیش از یک کلاس اختصاص بدهیم، نام کلاس‌ها را با فضای خالی از
هم جدا می کنیم. در کد CSSای که در ادامه نمایش داده می شود، کلاس `actor` به همه‌ی
عناصر بازیگر موقعیتی مطلقشان را تخصیص می دهد. نام نوع بازیگران به عنوان کلاسی
اضافی استفاده می شود تا به هر کدام یک رنگ اختصاص یابد. نیازی نیست که کلاس `lava`
را دوباره تعریف کنیم چون از همان کلاس `lava` که برای مربع‌های grid تعریف کرده بودیم
در قبل استفاده خواهیم کرد.

```{lang: "text/css"}
.actor  { position: absolute;            }
.coin   { background: rgb(241, 229, 89); }
.player { background: rgb(64, 64, 64);   }
```

{{index graphics, optimization, efficiency, [state, "of application"], [DOM, graphics]}}

متد `syncState` برای نمایش دادن یک وضعیت داده شده استفاده می شود. ابتدا تصاویر
گرافیکی قدیمی بازیگران را حذف می کند، در صورت وجود، و سپس بازیگران را در موقعیت
جدیدشان از نو ترسیم می کند. ممکن￼ وسوسه‌انگیز باشد که از عناصر DOM برای بازیگران
دوباره استفاده کنیم، اما برای این کار، لازم است تا کلی حساب و کتاب اضافی برای
انتساب بازیگران به عناصر DOM انجام دهیم و باز مطمئن شویم با ناپدید شدن هر بازیگر
آن عناصر مرتبط را نیز حذف کنیم. به دلیل اینکه تعداد انگشت‌شماری بازیگر در این
بازی وجود دارد، از نو ترسیم کردن همه‌ی آنها کار هزینه‌بردازی محسوب نمی شود.

```{includeCode: true}
DOMDisplay.prototype.syncState = function(state) {
  if (this.actorLayer) this.actorLayer.remove();
  this.actorLayer = drawActors(state.actors);
  this.dom.appendChild(this.actorLayer);
  this.dom.className = `game ${state.status}`;
  this.scrollPlayerIntoView(state);
};
```

{{index level, "class attribute"}}

با افزودن وضعیت فعلی مرحله به عنوان یک نام کلاس به wrapper، می توانیم شخصیت بازی
را در زمان برنده شدن یا باختن بازی سبک‌دهی متفاوتی بکنیم و این کار با افزودن یک
دستور CSS که زمانی اعمال می شود که بازیکن عنصر والدش دارای کلاس داده شده باشد.

```{lang: "text/css"}
.lost .player {
  background: rgb(160, 64, 64);
}
.won .player {
  box-shadow: -4px -7px 8px white, 4px -7px 8px white;
}
```

{{index player, "box shadow (CSS)"}}

بعد از برخورد با گدازه، رنگ بازیکن به قرمز تیره تغییر می کند که سوختن را نمایش
دهد. زمانی که آخرین سکه هم جمع شد دو سایه‌ی سفید تار- یکی به بالا-چپ و دیگری به
بالا-راست اضافه می کنیم- تا جلوه‌ی هاله‌ی روشن را ایجاد کنیم.


{{id viewport}}

{{index "position (CSS)", "max-width (CSS)", "overflow (CSS)", "max-height (CSS)", viewport, scrolling, [DOM, graphics]}}

نمی توانیم فرض بگیریم که مرحله‌ی بازی همیشه در میدان دید (viewport) باشد – منظور عنصری است که
در آن بازی را ترسیم می کنیم. به همین دلیل است که فراخوانی `scrollPlayerIntoView`
لازم است – این تابع باعث می شود تا در صورتی که مرحله‌ی بازی از اندازه‌ی میدان دید
فراتر رفت، آن عنصر میدان دید اسکرول شود و شخصیت بازی نزدیک وسط تصویر آن قرار
گیرد. دستورات ‌CSS پیش رو به عنصر wrapper بازی بیشینه‌ی اندازه را اختصاص داده و
اطمینان حاصل می کند که هر چیزی که بیرون از محدوده‌ی این عنصر قرار بگیرد قابل
مشاهده نخواهد بود. همچنین به عنصر بیرونی یک موقعیت نسبی تخصیص دادیم که باعث می
شود بازیگران درون آن نسبت به گوشه‌ی چپ-بالای مرحله موقعیت دهی شوند.


```{lang: "text/css"}
.game {
  overflow: hidden;
  max-width: 600px;
  max-height: 450px;
  position: relative;
}
```

{{index scrolling}}

در متد `scrollPlayerIntoView` ما موقعیت بازیکن را پیدا می کنیم و موقعیت اسکرول
عنصر پوشاننده‌ی آن را به‌روز می کنیم. موقعیت اسکرول را با دستکاری خاصیت‌های
`scollLeft` و `scrollTop` وقتی که بازیکن خیلی به کناره‌ها نزیک می شود تغییر می دهیم.

```{includeCode: true}
DOMDisplay.prototype.scrollPlayerIntoView = function(state) {
  let width = this.dom.clientWidth;
  let height = this.dom.clientHeight;
  let margin = width / 3;

  // The viewport
  let left = this.dom.scrollLeft, right = left + width;
  let top = this.dom.scrollTop, bottom = top + height;

  let player = state.player;
  let center = player.pos.plus(player.size.times(0.5))
                         .times(scale);

  if (center.x < left + margin) {
    this.dom.scrollLeft = center.x - margin;
  } else if (center.x > right - margin) {
    this.dom.scrollLeft = center.x + margin - width;
  }
  if (center.y < top + margin) {
    this.dom.scrollTop = center.y - margin;
  } else if (center.y > bottom - margin) {
    this.dom.scrollTop = center.y + margin - height;
  }
};
```

{{index center, coordinates, readability}}

روشی که در آن مرکز بازیکن را پیدا کردیم نشان می دهد چگونه متدهای موجود در نوع
`Vec` امکان محاسبات روی اشیاء را به شکلی نسبتا خوانا فراهم می کنند. برای پیدا کردن
مرکز بازیگر، موقعیت آن را (گوشه‌ی بالا-چپ) به نیمی از اندازه‌اش اضافه می کنیم. در
مختصات مرحله آن مرکز محسوب می شود اما ما نیاز به مختصات در واحد پیکس داریم که
بتوانیم بردار نتیجه را در مقیاس نمایش مان ضرب کنیم.

{{index validation}}

در ادامه مجموعه‌ای از بررسی‌ها را داریم که اطمینان حاصل شود که موقعیت بازیکن بیرون
از بازه‌ی مجاز قرار نگیرد. توجه داشته باشید که گاهی اوقات مختصات اسکرول تولیدی
نادرست می شود، عددی منفی یا بیشتر از محدوده‌ی قابل اسکرول. این مشکلی پیش نخواهد
آورد – DOM آن ها را به مقدارهای قابل قبول محدود می کند. اگر مقدار `scrollLeft` را
برابر <bdo>-10</bdo> تنظیم کنید به صورت خودکار 0 خواهد شد.

کمی کار راحت تر می شد اگر همیشه بازیکن در مرکز میدان دید scroll می شد. اما این
باعث نسبتا حالت لرزش ایجاد می کرد. در هنگام پرش، تصویر مداوم به سمت بالا و پایین
حرکت می کند. بهتر است که یک ناحیه بیطرف در مرکز صفحه‌ی نمایش داشته باشیم که بتوان
در آن بدون ایجاد اسکرول به حرکت پرداخت.

{{index [game, screenshot]}}

اکنون می توانیم مرحله را به نشان دهیم.

```{lang: "text/html"}
<link rel="stylesheet" href="css/game.css">

<script>
  let simpleLevel = new Level(simpleLevelPlan);
  let display = new DOMDisplay(document.body, simpleLevel);
  display.syncState(State.start(simpleLevel));
</script>
```

{{if book

{{figure {url: "img/game_simpleLevel.png", alt: "Our level rendered",width: "7cm"}}}

if}}

{{index "link (HTML tag)", CSS}}

برچسب <bdo>`<link>`</bdo> زمانی که با <bdo>`rel="stylesheet"`</bdo> استفاده می
شود ، باعث بارگیری یک فایل CSS درون صفحه می شود. فایل <bdo>`game.css`</bdo> سبک‌های مورد نیاز
بازی را در بر دارد.

## Motion and collision

{{index physics, [animation, "platform game"]}}

Now we're at the point where we can start adding motion—the most
interesting aspect of the game. The basic approach, taken by most
games like this, is to split ((time)) into small steps and, for each
step, move the actors by a distance corresponding to their speed
multiplied by the size of the time step. We'll measure time in
seconds, so speeds are expressed in units per second.

{{index obstacle, "collision detection"}}

Moving things is easy. The difficult part is dealing with the
interactions between the elements. When the player hits a wall or
floor, they should not simply move through it. The game must notice
when a given motion causes an object to hit another object and respond
accordingly. For walls, the motion must be stopped. When hitting a
coin, it must be collected. When touching lava, the game should be lost.

Solving this for the general case is a big task. You can find
libraries, usually called _((physics engine))s_, that simulate
interaction between physical objects in two or three ((dimensions)).
We'll take a more modest approach in this chapter, handling only
collisions between rectangular objects and handling them in a rather
simplistic way.

{{index bouncing, "collision detection", [animation, "platform game"]}}

Before moving the ((player)) or a block of ((lava)), we test whether
the motion would take it inside of a wall. If it does, we simply
cancel the motion altogether. The response to such a collision depends
on the type of actor—the player will stop, whereas a lava block will
bounce back.

{{index discretization}}

This approach requires our ((time)) steps to be rather small since it
will cause motion to stop before the objects actually touch. If the
time steps (and thus the motion steps) are too big, the player would
end up hovering a noticeable distance above the ground. Another
approach, arguably better but more complicated, would be to find the
exact collision spot and move there. We will take the simple approach
and hide its problems by ensuring the animation proceeds in small
steps.

{{index obstacle, "touches method", "collision detection"}}

{{id touches}}

This method tells us whether a ((rectangle)) (specified by a position
and a size) touches a grid element of the given type.

```{includeCode: true}
Level.prototype.touches = function(pos, size, type) {
  var xStart = Math.floor(pos.x);
  var xEnd = Math.ceil(pos.x + size.x);
  var yStart = Math.floor(pos.y);
  var yEnd = Math.ceil(pos.y + size.y);

  for (var y = yStart; y < yEnd; y++) {
    for (var x = xStart; x < xEnd; x++) {
      let isOutside = x < 0 || x >= this.width ||
                      y < 0 || y >= this.height;
      let here = isOutside ? "wall" : this.rows[y][x];
      if (here == type) return true;
    }
  }
  return false;
};
```

{{index "Math.floor function", "Math.ceil function"}}

The method computes the set of grid squares that the body ((overlap))s
with by using `Math.floor` and `Math.ceil` on its ((coordinates)).
Remember that ((grid)) squares are 1 by 1 units in size. By
((rounding)) the sides of a box up and down, we get the range of
((background)) squares that the box touches.

{{figure {url: "img/game-grid.svg", alt: "Finding collisions on a grid",width: "3cm"}}}

We loop over the block of ((grid)) squares found by ((rounding)) the
((coordinates)) and return `true` when a matching square is found.
Squares outside of the level are always treated as `"wall"` to ensure
that the player can't leave the world and that we won't accidentally
try to read outside of the bounds of our `rows` array.

The state `update` method uses `touches` to figure out whether the player
is touching lava.

```{includeCode: true}
State.prototype.update = function(time, keys) {
  let actors = this.actors
    .map(actor => actor.update(time, this, keys));
  let newState = new State(this.level, actors, this.status);

  if (newState.status != "playing") return newState;

  let player = newState.player;
  if (this.level.touches(player.pos, player.size, "lava")) {
    return new State(this.level, actors, "lost");
  }

  for (let actor of actors) {
    if (actor != player && overlap(actor, player)) {
      newState = actor.collide(newState);
    }
  }
  return newState;
};
```

The method is passed a time step and a data structure that tells it which keys
are being held down. The first thing it does is call the `update`
method on all actors, producing an array of updated actors. The actors
also get the time step, the keys, and the state, so that they can base
their update on those. Only the player will actually read keys, since
that's the only actor that's controlled by the keyboard.

If the game is already over, no further processing has to be done (the
game can't be won after being lost, or vice versa). Otherwise, the
method tests whether the player is touching background lava. If so,
the game is lost, and we're done. Finally, if the game really is still
going on, it sees whether any other actors overlap the player.

Overlap between actors is detected with the `overlap` function. It
takes two actor objects and returns true when they touch—which is the
case when they overlap both along the x-axis and along the y-axis.

```{includeCode: true}
function overlap(actor1, actor2) {
  return actor1.pos.x + actor1.size.x > actor2.pos.x &&
         actor1.pos.x < actor2.pos.x + actor2.size.x &&
         actor1.pos.y + actor1.size.y > actor2.pos.y &&
         actor1.pos.y < actor2.pos.y + actor2.size.y;
}
```

If any actor does overlap, its `collide` method gets a chance to
update the state. Touching a lava actor sets the game status to
`"lost"`. Coins vanish when you touch them and set the status to
`"won"` when they are the last coin of the level.

```{includeCode: true}
Lava.prototype.collide = function(state) {
  return new State(state.level, state.actors, "lost");
};

Coin.prototype.collide = function(state) {
  let filtered = state.actors.filter(a => a != this);
  let status = state.status;
  if (!filtered.some(a => a.type == "coin")) status = "won";
  return new State(state.level, filtered, status);
};
```

{{id actors}}

## Actor updates

{{index actor, "Lava class", lava}}

Actor objects' `update` methods take as arguments the time step, the
state object, and a `keys` object. The one for the `Lava` actor type
ignores the `keys` object.

```{includeCode: true}
Lava.prototype.update = function(time, state) {
  let newPos = this.pos.plus(this.speed.times(time));
  if (!state.level.touches(newPos, this.size, "wall")) {
    return new Lava(newPos, this.speed, this.reset);
  } else if (this.reset) {
    return new Lava(this.reset, this.speed, this.reset);
  } else {
    return new Lava(this.pos, this.speed.times(-1));
  }
};
```

{{index bouncing, multiplication, "Vec class", "collision detection"}}

This `update` method computes a new position by adding the product of the ((time)) step
and the current speed to its old position. If no obstacle blocks that
new position, it moves there. If there is an obstacle, the behavior
depends on the type of the ((lava)) block—dripping lava has a `reset`
position, to which it jumps back when it hits something. Bouncing lava
inverts its speed by multiplying it by -1 so that it starts moving in
the opposite direction.

{{index "Coin class", coin, wave}}

Coins use their `update` method to wobble. They ignore collisions with
the grid since they are simply wobbling around inside of their own
square.

```{includeCode: true}
const wobbleSpeed = 8, wobbleDist = 0.07;

Coin.prototype.update = function(time) {
  let wobble = this.wobble + time * wobbleSpeed;
  let wobblePos = Math.sin(wobble) * wobbleDist;
  return new Coin(this.basePos.plus(new Vec(0, wobblePos)),
                  this.basePos, wobble);
};
```

{{index "Math.sin function", sine, phase}}

The `wobble` property is incremented to track time and then used as an
argument to `Math.sin` to find the new position on the ((wave)). The
coin's current position is then computed from its base position and an
offset based on this wave.

{{index "collision detection", "Player class"}}

That leaves the ((player)) itself. Player motion is handled separately
per ((axis)) because hitting the floor should not prevent horizontal
motion, and hitting a wall should not stop falling or jumping motion.

```{includeCode: true}
const playerXSpeed = 7;
const gravity = 30;
const jumpSpeed = 17;

Player.prototype.update = function(time, state, keys) {
  let xSpeed = 0;
  if (keys.ArrowLeft) xSpeed -= playerXSpeed;
  if (keys.ArrowRight) xSpeed += playerXSpeed;
  let pos = this.pos;
  let movedX = pos.plus(new Vec(xSpeed * time, 0));
  if (!state.level.touches(movedX, this.size, "wall")) {
    pos = movedX;
  }

  let ySpeed = this.speed.y + time * gravity;
  let movedY = pos.plus(new Vec(0, ySpeed * time));
  if (!state.level.touches(movedY, this.size, "wall")) {
    pos = movedY;
  } else if (keys.ArrowUp && ySpeed > 0) {
    ySpeed = -jumpSpeed;
  } else {
    ySpeed = 0;
  }
  return new Player(pos, new Vec(xSpeed, ySpeed));
};
```

{{index [animation, "platform game"], keyboard}}

The horizontal motion is computed based on the state of the left and
right arrow keys. When there's no wall blocking the new position
created by this motion, it is used. Otherwise, the old position is
kept.

{{index acceleration, physics}}

Vertical motion works in a similar way but has to simulate ((jumping))
and ((gravity)). The player's vertical speed (`ySpeed`) is first
accelerated to account for ((gravity)).

{{index "collision detection", keyboard, jumping}}

We check for walls again. If we don't hit any, the new position is
used. If there _is_ a wall, there are two possible outcomes. When the
up arrow is pressed _and_ we are moving down (meaning the thing we hit
is below us), the speed is set to a relatively large, negative value.
This causes the player to jump. If that is not the case, the player
simply bumped into something, and the speed is set to zero.

The gravity strength, ((jumping)) speed, and pretty much all other
((constant))s in this game have been set by ((trial and error)). I
tested values until I found a combination I liked.

## Tracking keys

{{index keyboard}}

For a ((game)) like this, we do not want keys to take effect once per
keypress. Rather, we want their effect (moving the player figure) to
stay active as long as they are held.

{{index "preventDefault method"}}

We need to set up a key handler that stores the current state of the
left, right, and up arrow keys. We will also want to call
`preventDefault` for those keys so that they don't end up
((scrolling)) the page.

{{index "trackKeys function", "key code", "event handling", "addEventListener method"}}

The following function, when given an array of key names, will return
an object that tracks the current position of those keys. It registers
event handlers for `"keydown"` and `"keyup"` events and, when the key
code in the event is present in the set of codes that it is tracking,
updates the object.

```{includeCode: true}
function trackKeys(keys) {
  let down = Object.create(null);
  function track(event) {
    if (keys.includes(event.key)) {
      down[event.key] = event.type == "keydown";
      event.preventDefault();
    }
  }
  window.addEventListener("keydown", track);
  window.addEventListener("keyup", track);
  return down;
}

const arrowKeys =
  trackKeys(["ArrowLeft", "ArrowRight", "ArrowUp"]);
```

{{index "keydown event", "keyup event"}}

The same handler function is used for both event types. It looks at
the event object's `type` property to determine whether the key state
should be updated to true (`"keydown"`) or false (`"keyup"`).

{{id runAnimation}}

## Running the game

{{index "requestAnimationFrame function", [animation, "platform game"]}}

The `requestAnimationFrame` function, which we saw in [Chapter
?](dom#animationFrame), provides a good way to animate a game. But its
interface is quite primitive—using it requires us to track the time at
which our function was called the last time around and call
`requestAnimationFrame` again after every frame.

{{index "runAnimation function", "callback function", [function, "as value"], [function, "higher-order"], [animation, "platform game"]}}

Let's define a helper function that wraps those boring parts in a
convenient interface and allows us to simply call `runAnimation`,
giving it a function that expects a time difference as an argument and
draws a single frame. When the frame function returns the value
`false`, the animation stops.

```{includeCode: true}
function runAnimation(frameFunc) {
  let lastTime = null;
  function frame(time) {
    if (lastTime != null) {
      let timeStep = Math.min(time - lastTime, 100) / 1000;
      if (frameFunc(timeStep) === false) return;
    }
    lastTime = time;
    requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);
}
```

{{index time, discretization}}

I have set a maximum frame step of 100 milliseconds (one-tenth of a
second). When the browser tab or window with our page is hidden,
`requestAnimationFrame` calls will be suspended until the tab or
window is shown again. In this case, the difference between `lastTime`
and `time` will be the entire time in which the page was hidden.
Advancing the game by that much in a single step would look silly and
might cause weird side effects, such as the player falling through the
floor.

The function also converts the time steps to seconds, which are an
easier quantity to think about than milliseconds.

{{index "callback function", "runLevel function", [animation, "platform game"]}}

The `runLevel` function takes a `Level` object and a ((display))
constructor and returns a promise. It displays the level (in
`document.body`) and lets the user play through it. When the level is
finished (lost or won), `runLevel` waits one more second (to let the
user see what happens) and then clears the display, stops the
animation, and resolves the promise to the game's end status.

```{includeCode: true}
function runLevel(level, Display) {
  let display = new Display(document.body, level);
  let state = State.start(level);
  let ending = 1;
  return new Promise(resolve => {
    runAnimation(time => {
      state = state.update(time, arrowKeys);
      display.syncState(state);
      if (state.status == "playing") {
        return true;
      } else if (ending > 0) {
        ending -= time;
        return true;
      } else {
        display.clear();
        resolve(state.status);
        return false;
      }
    });
  });
}
```

{{index "runGame function"}}

A game is a sequence of ((level))s. Whenever the ((player)) dies, the
current level is restarted. When a level is completed, we move on to
the next level. This can be expressed by the following function, which
takes an array of level plans (strings) and a ((display)) constructor:

```{includeCode: true}
async function runGame(plans, Display) {
  for (let level = 0; level < plans.length;) {
    let status = await runLevel(new Level(plans[level]),
                                Display);
    if (status == "won") level++;
  }
  console.log("You've won!");
}
```

{{index "asynchronous programming", "event handling"}}

Because we made `runLevel` return a promise, `runGame` can be written
using an `async` function, as shown in [Chapter ?](async). It returns
another promise, which resolves when the player finishes the game.

{{index game, "GAME_LEVELS data set"}}

There is a set of ((level)) plans available in the `GAME_LEVELS`
binding in [this chapter's
sandbox](https://eloquentjavascript.net/code#16)[
([_https://eloquentjavascript.net/code#16_](https://eloquentjavascript.net/code#16))]{if
book}. This page feeds them to `runGame`, starting an actual game.

```{sandbox: null, focus: yes, lang: "text/html", startCode: true}
<link rel="stylesheet" href="css/game.css">

<body>
  <script>
    runGame(GAME_LEVELS, DOMDisplay);
  </script>
</body>
```

{{if interactive

See if you can beat those. I had quite a lot of fun building them.

if}}

## Exercises

### Game over

{{index "lives (exercise)", game}}

It's traditional for ((platform game))s to have the player start with
a limited number of _lives_ and subtract one life each time they die.
When the player is out of lives, the game restarts from the beginning.

{{index "runGame function"}}

Adjust `runGame` to implement lives. Have the player start with three.
Output the current number of lives (using `console.log`) every time a
level starts.

{{if interactive

```{lang: "text/html", test: no, focus: yes}
<link rel="stylesheet" href="css/game.css">

<body>
<script>
  // The old runGame function. Modify it...
  async function runGame(plans, Display) {
    for (let level = 0; level < plans.length;) {
      let status = await runLevel(new Level(plans[level]),
                                  Display);
      if (status == "won") level++;
    }
    console.log("You've won!");
  }
  runGame(GAME_LEVELS, DOMDisplay);
</script>
</body>
```

if}}

### Pausing the game

{{index "pausing (exercise)", "escape key", keyboard}}

Make it possible to pause (suspend) and unpause the game by pressing
the Esc key.

{{index "runLevel function", "event handling"}}

This can be done by changing the `runLevel` function to use another
keyboard event handler and interrupting or resuming the animation
whenever the Esc key is hit.

{{index "runAnimation function"}}

The `runAnimation` interface may not look like it is suitable for this
at first glance, but it is if you rearrange the way `runLevel` calls
it.

{{index [binding, global], "trackKeys function"}}

When you have that working, there is something else you could try. The
way we have been registering keyboard event handlers is somewhat
problematic. The `arrows` object is currently a global binding, and
its event handlers are kept around even when no game is running. You
could say they _((leak))_ out of our system. Extend `trackKeys` to
provide a way to unregister its handlers and then change `runLevel`
to register its handlers when it starts and unregister them again when
it is finished.

{{if interactive

```{lang: "text/html", focus: yes, test: no}
<link rel="stylesheet" href="css/game.css">

<body>
<script>
  // The old runLevel function. Modify this...
  function runLevel(level, Display) {
    let display = new Display(document.body, level);
    let state = State.start(level);
    let ending = 1;
    return new Promise(resolve => {
      runAnimation(time => {
        state = state.update(time, arrowKeys);
        display.syncState(state);
        if (state.status == "playing") {
          return true;
        } else if (ending > 0) {
          ending -= time;
          return true;
        } else {
          display.clear();
          resolve(state.status);
          return false;
        }
      });
    });
  }
  runGame(GAME_LEVELS, DOMDisplay);
</script>
</body>
```

if}}

{{hint

{{index "pausing (exercise)", [animation, "platform game"]}}

An animation can be interrupted by returning `false` from the
function given to `runAnimation`. It can be continued by calling
`runAnimation` again.

{{index closure}}

So we need to communicate the fact that we are pausing the game to the
function given to `runAnimation`. For that, you can use a binding that
both the event handler and that function have access to.

{{index "event handling", "removeEventListener method", [function, "as value"]}}

When finding a way to unregister the handlers registered by
`trackKeys`, remember that the _exact_ same function value that was
passed to `addEventListener` must be passed to `removeEventListener`
to successfully remove a handler. Thus, the `handler` function value
created in `trackKeys` must be available to the code that unregisters
the handlers.

You can add a property to the object returned by `trackKeys`,
containing either that function value or a method that handles the
unregistering directly.

hint}}

### A monster

{{index "monster (exercise)"}}

It is traditional for platform games to have enemies that you can jump
on top of to defeat. This exercise asks you to add such an actor type
to the game.

We'll call it a monster. Monsters move only horizontally. You can make
them move in the direction of the player, bounce back and forth
like horizontal lava, or have any movement pattern you want. The class
doesn't have to handle falling, but it should make sure the monster
doesn't walk through walls.

When a monster touches the player, the effect depends on whether the
player is jumping on top of them or not. You can approximate this by
checking whether the player's bottom is near the monster's top. If
this is the case, the monster disappears. If not, the game is lost.

{{if interactive

```{test: no, lang: "text/html", focus: yes}
<link rel="stylesheet" href="css/game.css">
<style>.monster { background: purple }</style>

<body>
  <script>
    // Complete the constructor, update, and collide methods
    class Monster {
      constructor(pos, /* ... */) {}

      get type() { return "monster"; }

      static create(pos) {
        return new Monster(pos.plus(new Vec(0, -1)));
      }

      update(time, state) {}

      collide(state) {}
    }

    Monster.prototype.size = new Vec(1.2, 2);

    levelChars["M"] = Monster;

    runLevel(new Level(`
..................................
.################################.
.#..............................#.
.#..............................#.
.#..............................#.
.#...........................o..#.
.#..@...........................#.
.##########..............########.
..........#..o..o..o..o..#........
..........#...........M..#........
..........################........
..................................
`), DOMDisplay);
  </script>
</body>
```

if}}

{{hint

{{index "monster (exercise)", "persistent data structure"}}

If you want to implement a type of motion that is stateful, such as
bouncing, make sure you store the necessary state in the actor
object—include it as constructor argument and add it as a property.

Remember that `update` returns a _new_ object, rather than changing
the old one.

{{index "collision detection"}}

When handling collision, find the player in `state.actors` and
compare its position to the monster's position. To get the _bottom_ of
the player, you have to add its vertical size to its vertical
position. The creation of an updated state will resemble either
`Coin`'s `collide` method (removing the actor) or `Lava`'s (changing
the status to `"lost"`), depending on the player position.

hint}}
