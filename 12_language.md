{{meta {load_files: ["code/chapter/12_language.js"], zip: "node/html"}}}

# پروژه: نوشتن یک زبان برنامه‌نویسی

{{quote {author: "Hal Abelson and Gerald Sussman", title: "Structure and Interpretation of Computer Programs", chapter: true}

The evaluator, which determines the meaning of expressions in a
programming language, is just another program.

quote}}

{{index "Abelson, Hal", "Sussman, Gerald", SICP, "project chapter"}}

{{figure {url: "img/chapter_picture_12.jpg", alt: "Picture of an egg with smaller eggs inside", chapter: "framed"}}}

نوشتن زبان برنامه‌نویسی خودتان به طرز شگفت‌آوری آسان و آموزنده است (تا زمانی
که سطح انتظاراتتان زیاد￼ بالا نباشد).


هدف اصلی من در این فصل این است که به شما نشان دهم نوشتن زبان برنامه‌نویسی خودتان
چیزی ماوارایی و جادویی نیست. خیلی وقت‌ها احساس می کردم بعضی از اختراعات انسان ها به شدت
هوشمندانه و پیچیده است و من هرگز نمی توانم آن ها را درک کنم. اما با کمی مطالعه
و آزمایش، اغلب متوجه می شدم که اتفاقا معمولی و شدنی هستند.

{{index "Egg language", [abstraction, "in Egg"]}}

در این فصل یک زبان برنامه‌نویسی خواهیم ساخت که نام آن Egg می‌باشد. این زبان
بسیار ساده و کوچک خواهد بود – اما به اندازه‌ی کافی قدرتمند خواهد بود تا بتواند
هر محاسبه‌ای که تصور می‌کنید را انجام دهد. در این زبان می توان با استفاده از
توابع، تجرید‌های ساده را به وجود آورد.

{{id parsing}}

## تجزیه

{{index parsing, validation, [syntax, "of Egg"]}}

در چشم ترین قسمت یک زبان برنامه‌نویسی، گرامر (_syntax_) یا شیوه‌ی نشان‌گذاری آن
است. یک تجزیه‌گر (_parser_) برنامه‌ای است که متنی را خوانده و ساختار داده‌ای تولید می کند که
نمایانگر ساختار برنامه‌ای است که در آن متن قرار دارد. اگر آن متن یک برنامه‌ی
معتبر را شکل نداده باشد، تجزیه‌گر باید خطایی تولید کند.

{{index "special form", [function, application]}}

زبان برنامه‌نویسی ما گرامری ساده و یکپارچه خواهد داشت. همه چیز در زبان Egg از جنس
عبارت خواهند بود. یک عبارت می تواند نام یک متغیر، یک عدد، یک رشته، یا یک
کاربرد (application) باشد. کاربردها برای فراخوانی توابع استفاده می شوند؛ همچنین
برای ساختارهایی مثل `if` و `while` نیز استفاده می شوند.

{{index "double-quote character", parsing, [escaping, "in strings"], [whitespace, syntax]}}

برای این که تجزیه‌گر را ساده نگه داریم، رشته‌ها در زبان Egg چیزهایی مثل گریز با
بک‌اسلش را پشتیبانی نمی کنند. یک رشته به طور ساده دنباله‌ای از کاراکترها است که
شامل نقل‌قول جفتی نمی‌باشد و توسط نقل قول جفتی محصور می‌شود. یک عدد برابر با دنباله‌ای از
ارقام است. در نام متغیرها می می‌توان از هر کاراکتری به جز فضای خالی و کاراکترهایی که
معنای خاصی در گرامر زبان دارند استفاده نمود.

{{index "comma character", [parentheses, arguments]}}

کاربردها به همان سبکی که در جاوااسکریپت هستند نوشته می شوند: بعد از یک عبارت
پرانتز قرار می‌گیرد که درون آن هر تعداد آرگومان مجاز می‌باشد که به وسیله‌ی
ویرگول از هم جدا می‌شوند.

```{lang: null}
do(define(x, 10),
   if(>(x, 5),
      print("large"),
      print("small")))
```

{{index block, [syntax, "of Egg"]}}

یکریختی مورد نظر در زبان Egg به این معنا است که چیزهایی که در جاوااسکریپت به
عنوان عملگر محسوب می شدند (مثل `>`)، در این زبان متغیرهایی معمولی می‌باشند که
مثل دیگر قابلیت‌ها به کار گرفته می شوند. و به دلیل اینکه در گرامر این زبان
مفهومی به نام بلاک وجود ندارد، به ساختاری به نام `do` نیاز داریم بتوانیم انجام
متوالی چند عمل را نمایش دهیم.

{{index "type property", parsing, ["data structure", tree]}}

ساختار داده‌ای که تجزیه‌گر از آن برای توصیف یک برنامه استفاده خواهد کرد از اشیاء
((عبارت)) تشکیل شده است که هر کدام از آن ها یک خاصیت `type` دارد که نمایانگر نوع آن
عبارت و خاصیت‌های دیگری برای توصیف محتوای آن دارد.

{{index identifier}}

عبارت‌های نوع `"value"` نمایانگر رشته‌ها و اعداد خام می‌باشند. خاصیت `value` آن ها حاوی
مقدار رشته یا عددی است که نماینده آن هستند. عبارت‌های نوع `"word"` برای شناسه‌ها
(نام‌ها) استفاده می شوند. این گونه اشیاء دارای خاصیتی به نام `name` می باشند که نام
شناسه‌ را به عنوان یک رشته نگه‌داری می کند. در آخر، عبارت `"apply"` نمایانگر یک
کاربرد است. این عبارت‌ها دارای یک خاصیت به نام `operator` می باشند که به عبارتی
اشاره می کند مورد استعمال قرار گرفته است و خاصیتی به نام `args` دارند که آرایه‌ای از
عبارت‌های آرگومان را نگه‌داری می کند.

بخش <bdo>`>(x, 5)`</bdo> از برنامه‌ی قبلی به شکل زیر نمایش داده می شود:

```{lang: "application/json"}
{
  type: "apply",
  operator: {type: "word", name: ">"},
  args: [
    {type: "word", name: "x"},
    {type: "value", value: 5}
  ]
}
```

{{indexsee "abstract syntax tree", "syntax tree", ["data structure", tree]}}

این گونه از ساختار داده را _درخت گرامر_ می گویند. اگر اشیاء را به عنوان نقطه در
نظر بگیرید و پیوندهای بین‌شان را به عنوان خطوط بین نقطه‌ها، نمای آن شبیه به درخت
خواهد بود. این واقعیت که عبارت‌ها خود از عبارت‌های دیگری تشکیل می شوند ، که آن
ها هم ممکن است شامل عبارت‌های دیگری باشند ، شبیه به شاخه‌های درخت است که خود
دارای شاخه‌های دیگر هستند.

{{figure {url: "img/syntax_tree.svg", alt: "The structure of a syntax tree",width: "5cm"}}}

{{index parsing}}

این را با تجزیه‌گری که برای فایل تنظیمات در [فصل ?](regexp#ini) نوشتیم مقایسه کنید، که ساختاری
ساده داشت: ورودی را به خطوطی تقسیم می کرد و همه‌ی آن خطوط را یکی یکی رسیدگی می
کرد. خطوط فقط می‌توانستند شکل‌های محدودی داشته باشند.

{{index recursion, [nesting, "of expressions"]}}

در اینجا باید راه حلی دیگری پیدا کنیم. عبارت‌ها توسط خطوط جدا نمی شوند و ساختاری
بازگشتی دارند. عبارت‌های کاربردی (application) حاوی عبارت‌های دیگر می‌باشند.

{{index elegance}}

خوشبختانه، این مشکل را می توان به خوبی با نوشتن یک تابع بازگشتی تجزیه‌گر حل نمود
به صورتی که نمایانگر ماهیت بازگشتی زبان باشد.

{{index "parseExpression function", "syntax tree"}}

تابعی به نام `parseExpression` را تعریف می کنیم که رشته‌ای را به عنوان ورودی دریافت
می کند و یک شیء را باز می گرداند که حاوی ساختار داده‌ی مورد نظر برای عبارتی است که در
ابتدای رشته آمده است، به همراه بخشی از رشته که بعد از تجزیه آن عبارت باقی می
ماند. در زمان تجزیه‌ زیر عبارت‌ها (مثلا آرگومان‌های ورودی یک کاربرد)، این تابع
را می توان دوباره فراخوانی کرد تا عبارت آرگومان را به همراه متن باقی مانده
تولید کند. ممکن است که این متن، حاوی آرگومان‌های بیشتر یا پرانتز‌ آخر لیست ورودی‌ها باشد.

اولین بخش تجزیه‌گر به این صورت خواهد بود:

This is the first part of the parser:

```{includeCode: true}
function parseExpression(program) {
  program = skipSpace(program);
  let match, expr;
  if (match = /^"([^"]*)"/.exec(program)) {
    expr = {type: "value", value: match[1]};
  } else if (match = /^\d+\b/.exec(program)) {
    expr = {type: "value", value: Number(match[0])};
  } else if (match = /^[^\s(),#"]+/.exec(program)) {
    expr = {type: "word", name: match[0]};
  } else {
    throw new SyntaxError("Unexpected syntax: " + program);
  }

  return parseApply(expr, program.slice(match[0].length));
}

function skipSpace(string) {
  let first = string.search(/\S/);
  if (first == -1) return "";
  return string.slice(first);
}
```

{{index "skipSpace function", [whitespace, syntax]}}

به دلیل این که زبان Egg مانند جاوااسکریپت امکان استفاده از فضاهای خالی بین عناصر
را مجاز می شمرد، می بایست مکررا فضاهای خالی را از ابتدای یک رشته‌ی برنامه حذف
کنیم. این کار توسط تابع `skipSpace` صورت می گیرد.

{{index "literal expression", "SyntaxError type"}}

بعد از چشم‌پوشی از فضاهای خالی ابتدایی، تابع `parseExpression` از سه عبارت باقاعده
برای شناسایی سه عنصر اساسی که زبان Egg از آن‌ها پشتیبانی می کند شامل: رشته‌ها،
اعداد و کلمه‌ها، استفاده می کند. تجزیه‌گر با توجه به اینکه کدام یک از آن عناصر
تطبیق بخورد ساختار‌داده‌ی متفاوتی را تولید می کند. اگر ورودی با هیچ کدام از آن سه
شکل تطبیق نخورد، آن عبارت معتبر نخواهد بود و تجزیه‌گر یک خطا تولید می کند. ما از
`SyntaxError` به جای `Error` به عنوان تابع سازنده استثنا استفاده می کنیم که یک نوع
خطای استاندارد دیگر است، به این دلیل که این نوع کمی اختصاصی تر است – همچنین این
نوع خطا زمانی تولید می شود که تلاشی برای اجرای یک برنامه نامعتبر جاوااسکریپت
صورت می گرفته باشد.

{{index "parseApply function"}}

سپس آن قسمت که تطبیق خورده است را از رشته‌ی برنامه حذف می کنیم و رشته را به
همراه شیء متعلق به عبارت، به `parseApply` ارسال می کنیم که بررسی کند آیا عبارت از نوع
کاربرد (application) است. اگر بود، تابع قسمت داخل پرانتز را – لیست آرگومان‌ها –
تجزیه می کند.

```{includeCode: true}
function parseApply(expr, program) {
  program = skipSpace(program);
  if (program[0] != "(") {
    return {expr: expr, rest: program};
  }

  program = skipSpace(program.slice(1));
  expr = {type: "apply", operator: expr, args: []};
  while (program[0] != ")") {
    let arg = parseExpression(program);
    expr.args.push(arg.expr);
    program = skipSpace(arg.rest);
    if (program[0] == ",") {
      program = skipSpace(program.slice(1));
    } else if (program[0] != ")") {
      throw new SyntaxError("Expected ',' or ')'");
    }
  }
  return parseApply(expr, program.slice(1));
}
```

{{index parsing}}

اگر کاراکتر بعدی در برنامه یک پرانتز آغاز نباشد، پس ورودی یک کاربرد نیست و تابع
`parseApply` عبارتی که دریافت کرده بود را بر‌می‌گرداند.

{{index recursion}}

در غیر این صورت، از پرانتز آغاز عبور کرده و شیء درخت گرامر را برای این عبارت
کاربرد می سازد. سپس به صورت بازگشتی تابع `parseExpression` را فراخوانی می کند تا
هر یک از آرگومان‌ها را تا زمانیکه به یک پرانتز پایان برسد تجزیه کند. عمل بازگشتی
به صورت غیر مستقیم است، و با فراخوانی یکدیگر `parseApply` و `parseExpression` صورت
می پذیرد.

به دلیل اینکه می توان یک عبارت کاربرد را اجرا کرد (مثل عبارت <bdo>`multiplier(2)(1)`</bdo>)،
تابع `parseApply` باید بعد از آن که یک کاربرد را تجزیه کرد خودش را دوباره
فراخوانی کند تا اگر جفت پرانتز دیگری در ادامه آمده است متوجه آن بشود.


{{index "syntax tree", "Egg language", "parse function"}}

این تمام چیزی است که برای قسمت تجزیه‌ی Egg نیاز داریم. آن را در تابعی سرراست به
نام `parse` قرار می دهیم که بررسی می کند آیا بعد از تجزیه‌ی عبارت (یک برنامه‌ی Egg
یک عبارت واحد است) به انتهای رشته‌ی ورودی رسیده باشد و به ما ساختار دادهی برنامه
را تحویل دهد.

```{includeCode: strip_log, test: join}
function parse(program) {
  let {expr, rest} = parseExpression(program);
  if (skipSpace(rest).length > 0) {
    throw new SyntaxError("Unexpected text after program");
  }
  return expr;
}

console.log(parse("+(a, 10)"));
// → {type: "apply",
//    operator: {type: "word", name: "+"},
//    args: [{type: "word", name: "a"},
//           {type: "value", value: 10}]}
```

{{index "error message"}}

کار می کند! این تابع اطلاعات خیلی مفیدی در زمان بروز شکست به ما نمی دهد و خط و ستونی که
در آن عبارت شروع می شود را ذخیره نمی کند، که اگر بود، در زمان گزارش خطاها در
آینده کاربرد داشت، اما به هر حال برای هدف فعلی ما به اندازه کافی خوب است.

## ارزیاب

{{index "evaluate function", evaluation, interpretation, "syntax tree", "Egg language"}}

با داشتن درخت گرامر یک برنامه چه می تواند کرد؟ البته که اجرای آن! و
این چیزی است که ارزیاب انجام می دهد. ارزیاب، یک درخت گرامر و یک قلمروی شیء که
مقادیر را به نام‌ها اختصاص می دریافت می کند و عبارتی را که درخت نمایش می دهد،
 ارزیابی می کند و مقداری که این عبارت تولید می کند را برمی‌گرداند.

```{includeCode: true}
const specialForms = Object.create(null);

function evaluate(expr, scope) {
  if (expr.type == "value") {
    return expr.value;
  } else if (expr.type == "word") {
    if (expr.name in scope) {
      return scope[expr.name];
    } else {
      throw new ReferenceError(
        `Undefined binding: ${expr.name}`);
    }
  } else if (expr.type == "apply") {
    let {operator, args} = expr;
    if (operator.type == "word" &&
        operator.name in specialForms) {
      return specialForms[operator.name](expr.args, scope);
    } else {
      let op = evaluate(operator, scope);
      if (typeof op == "function") {
        return op(...args.map(arg => evaluate(arg, scope)));
      } else {
        throw new TypeError("Applying a non-function.");
      }
    }
  }
}
```

{{index "literal expression", scope}}

ارزیاب برای هر نوعی از عبارت‌ها کد به خصوصی دارد. عبارتی که شامل یک مقدار ساده
باشد، معادل خود مقدار خواهد بود. (به عنوان مثال، عبارت `100` فقط به عنوان عدد `100`
ارزیابی می شود) برای یک متغیر، باید بررسی کنیم که در قلمروی مورد نظر تعریف
شده باشد و در این صورت مقدار آن متغیر را بدست بیاوریم.

{{index [function, application]}}

کاربردها کمی پیچیده‌تر هستند. اگر در شکل خاصی باشند، مانند `if`، چیزی را ارزیابی
نمی کنیم و عبارت‌های آرگومان را به همراه قلمرو به تابعی که این شکل را رسیدگی می کند
ارسال می کنیم. اگر یک فراخوانی معمولی باشد، عملگر را ارزیابی کرده، اطمینان حاصل
می کنیم که یک تابع باشد، و آن را با آرگومان‌های ارزیابی شده فراخوانی می کنیم.

برای نمایش مقدارهای تابع Egg از مقدارهای تابع جاوااسکریپت استفاده خواهیم کرد. در
[ادامه](language#egg_fun) به این بخش باز خواهیم گشت، زمانی که شکل خاصی که `fun` خوانده می شود تعریف
شده باشد.

{{index readability, "evaluate function", recursion, parsing}}

ساختار بازگشتی `evaluate` به ساختاری مشابه تجزیه‌گر نزدیک است و هر دو بازتاب ساختار
خود زبان هستند. همچنین می توانستیم ارزیاب و تجزیه‌گر را یکپارچه کنیم و در حین
تجزیه، ارزیابی را نیز انجام دهیم، اما جدا کردن آن ها به این سبک باعث شفافیت
بیشتر برنامه می شود.

{{index "Egg language", interpretation}}

این تمام چیزی است که برای تفسیر Egg مورد نیاز است. به همین سادگی. اما هنوز بدون
تعریف چندین شکل خاص و افزودن چند مقدار مفید به محیط، نمی توان کار زیادی با
این زبان انجام داد.

## شکل‌های خاص

{{index "special form", "specialForms object"}}

شیء `specialForm` برای تعریف گرامر ویژه در Egg استفاده می شود. این شیء کلمه‌ها را
به توابعی که این شکل‌ها را ارزیابی می کنند انتساب می دهد. فعلا این شیء تهی است.
بیایید  `if` را به آن اضافه کنیم.

```{includeCode: true}
specialForms.if = (args, scope) => {
  if (args.length != 3) {
    throw new SyntaxError("Wrong number of args to if");
  } else if (evaluate(args[0], scope) !== false) {
    return evaluate(args[1], scope);
  } else {
    return evaluate(args[2], scope);
  }
};
```

{{index "conditional execution", "ternary operator", "?: operator", "conditional operator"}}

ساختار `if` در Egg دقیقا به سه آرگومان نیاز دارد. اولین آرگومان را ارزیابی می
کند، و اگر نتیجه‌ی آن برابر با مقدار `false` نبود، به سراغ ارزیابی دومی می رود. در
غیر این صورت، سومین آرگومان ارزیابی می شود. این شکل `if` بیشتر شبیه به عملگر
سه‌تای <bdo>`?:`</bdo> در جاوااسکریپت است تا دستور `if` در آن. این یک عبارت است، نه یک
دستور و مقداری را تولید می کند که همان نتیجه‌ی آرگومان دوم و سوم می‌باشد.

{{index Boolean}}

Egg همچنین در چگونگی رسیدگی به مقدار شرط در عبارت `if` با جاواسکریپت تفاوت دارد.
این عبارت چیزهایی مثل صفر یا رشته‌ی خالی را `false` در نظر نمی گیرد، فقط مقدار
دقیق `false` را در نظر می گیرد.

{{index "short-circuit evaluation"}}

علت نمایش `if` به عنوان یک شکل خاص به جای یک تابع معمولی، این است که تمامی
آرگومان‌ها در توابع قبل از این که تابع فراخوانی بشود ارزیابی می شوند در حالیکه
`if` بایستی فقط بعد از یکی از آرگومان‌ها دوم یا سوم بسته به مقدار آرگومان اول
ارزیابی شود.

شکل خاص `while` به همین صورت است.

```{includeCode: true}
specialForms.while = (args, scope) => {
  if (args.length != 2) {
    throw new SyntaxError("Wrong number of args to while");
  }
  while (evaluate(args[0], scope) !== false) {
    evaluate(args[1], scope);
  }

  // Since undefined does not exist in Egg, we return false,
  // for lack of a meaningful result.
  return false;
};
```

یکی دیگر از بلاک‌ها ساختاری پایه `do` است که تمامی آرگومان‌هایش را از بالا به
پایین اجرا می کند. مقدار آن برابر با مقداری است که توسط آرگومان آخر تولید می
شود.

```{includeCode: true}
specialForms.do = (args, scope) => {
  let value = false;
  for (let arg of args) {
    value = evaluate(arg, scope);
  }
  return value;
};
```

{{index ["= operator", "in Egg"], [binding, "in Egg"]}}

برای این که قادر باشیم تا متغیرهایی ایجاد کنیم و مقادیر جدیدی را به آن ها
اختصاص دهیم، همچنین نیاز به تعریف شکلی به نام `define` داریم. این شکل به عنوان
آرگومان اول یک واژه را دریافت می کند و به عنوان آرگومان دوم، عبارتی را که منجر
به تولید مقداری می شود که قرار است به آن واژه منتسب شود. به دلیل این که `define`
مثل هر چیز دیگر، یک عبارت است باید مقداری را برگرداند. ما طوری آن را می سازیم
که مقداری که به آن انتساب یافته را برگرداند (درست شبیه عملگر `=` در
جاوااسکریپت).

```{includeCode: true}
specialForms.define = (args, scope) => {
  if (args.length != 2 || args[0].type != "word") {
    throw new SyntaxError("Incorrect use of define");
  }
  let value = evaluate(args[1], scope);
  scope[args[0].name] = value;
  return value;
};
```

## محیط

{{index "Egg language", "evaluate function", [binding, "in Egg"]}}

قلمرویی که توسط `evaluate` قبول می شود یک شیء است که خاصیت‌هایی دارد که نام
آن‌ها متناظر با نام متغیرها می‌باشد و مقادیر آن برابر مقدار آن متغیرها خواهد
بود. بیایید شیئی را تعریف کنیم که نماینده قلمروی سراسری باشد.

برای این که بتوان از ساختار `if` که پیش‌تر تعریف کرده ایم استفاده کنیم باید به
مقادیر بولی دسترسی داشته باشیم. به دلیل این که فقط دو مقدار بولی وجوددارد، نیاز
به گرامر خاصی برای آن ها نداریم. کافی است تا دو مقدار `true` و `false` را به دو نام
منتسب کنیم و از آن ها استفاده کنیم.

```{includeCode: true}
const topScope = Object.create(null);

topScope.true = true;
topScope.false = false;
```
اکنون می توانیم یک عبارت ساده را که یک مقدار بولی را معکوس میکند ارزیابی کنیم.

```
let prog = parse(`if(true, false, true)`);
console.log(evaluate(prog, topScope));
// → false
```

{{index arithmetic, "Function constructor"}}

برای فراهم ساختن عملگرهای اصلی حسابی و مقایسه، همچنین چند مقدار تابع به قلمرو
اضافه خواهیم کرد. به خاطر علاقه به کوتاه نگه داشتن کد، به جای تعریف جداگانه‌ی هر کدام، از سازنده‌ی `Function` برای ترکیب چند تابع عملگر در یک حلقه استفاده می کنیم.

```{includeCode: true}
for (let op of ["+", "-", "*", "/", "==", "<", ">"]) {
  topScope[op] = Function("a, b", `return a ${op} b;`);
}
```
داشتن راهی برای چاپ مقادیر نیز بسیار کاربردی خواهد بود، بنابراین <bdo>`console.log`</bdo> را
در یک تابع قرار می دهیم و نام ان را `print` می گذاریم:

```{includeCode: true}
topScope.print = value => {
  console.log(value);
  return value;
};
```

{{index parsing, "run function"}}

با این کار به اندازه کافی ابزارهای مقدماتی برای نوشتن برنامه‌های ساده در اختیار
خواهیم داشت. توابع پیش رو راهی مناسب براب تجزیه‌ی یک برنامه و اجرای آن در یک
قلمروی جدید را فراهم می سازند.

```{includeCode: true}
function run(program) {
  return evaluate(parse(program), Object.create(topScope));
}
```

{{index "Object.create function", prototype}}

ما از زنجیره‌ی پروتوتایپ شیء برای نمایش قلمروهای تودرتو استفاده خواهیم کرد، تا
برنامه بتواند متغیرهای خودش را به قلمروی محلی بدون ایجاد تغییر در قلمروی بالادست
اضافه کند.

```
run(`
do(define(total, 0),
   define(count, 1),
   while(<(count, 11),
         do(define(total, +(total, count)),
            define(count, +(count, 1)))),
   print(total))
`);
// → 55
```

{{index "summing example", "Egg language"}}

این برنامه‌ای است که تاکنون چندین بار دیده‌ایم، که مجموع اعداد ‍‍1 تا 10 را
محاسبه می کند و به زبان Egg نوشته شده است. قطعا ظاهر این برنامه از برنامه‌ی
معادل جاوااسکریپتش زشت‌تر است – اما برای زبان برنامه‌نویسی‌ای که با کمتر از 150
خط کدنویسی پیاده‌سازی شده است بد نیست.

{{id egg_fun}}

## توابع

{{index function, "Egg language"}}

یک زبان برنامه‌نویسی بدون داشتن توابع، زبانی فقیر محسوب می شود.

خوشبختانه، به آسانی می توان یک ساختار `fun` به زبان افزود، که آرگومان آخرش
را به عنوان بدنه‌ی تابع در نظر بگیرد و از آرگومان های قبلی به عنوان نام
پارامترهای تابع استفاده کند.

```{includeCode: true}
specialForms.fun = (args, scope) => {
  if (!args.length) {
    throw new SyntaxError("Functions need a body");
  }
  let body = args[args.length - 1];
  let params = args.slice(0, args.length - 1).map(expr => {
    if (expr.type != "word") {
      throw new SyntaxError("Parameter names must be words");
    }
    return expr.name;
  });

  return function() {
    if (arguments.length != params.length) {
      throw new TypeError("Wrong number of arguments");
    }
    let localScope = Object.create(scope);
    for (let i = 0; i < arguments.length; i++) {
      localScope[params[i]] = arguments[i];
    }
    return evaluate(body, localScope);
  };
};
```

{{index "local scope"}}

توابع در Egg، قلمروی محلی خودشان را دریافت می کنند. تابعی که با `fun` تولید می شود
قلمروی محلی خودش را ایجاد کرده و آرگومان‌هایش را به آن مقید می کند. سپس بدنه‌ی
تابع را در این قلمرو ارزیابی کرده و نتیجه را باز می گرداند.

```{startCode: true}
run(`
do(define(plusOne, fun(a, +(a, 1))),
   print(plusOne(10)))
`);
// → 11

run(`
do(define(pow, fun(base, exp,
     if(==(exp, 0),
        1,
        *(base, pow(base, -(exp, 1)))))),
   print(pow(2, 10)))
`);
// → 1024
```

## کامپایل کردن

{{index interpretation, compilation}}

چیزی که تاکنون ساخته‌ایم یک مفسر است. در طول ارزیابی، این مفسر به طور مستقیم روی
چیزی که توسط تجزیه‌گر تولید شده عمل می نماید.

{{index efficiency, performance, [binding, definition], [memory, speed]}}

کامپایل کردن روندی است که در آن گامی دیگر بین تفسیر و اجرای برنامه اضافه می
شود، که کد برنامه را به چیزی که بتوان با کارایی بیشتری ارزیابی کرد تبدیل می کند
و این کار با انجام حداکثر کار ممکن قبل از مرحله‌ی ارزیابی میسر می شود. به عنوان
مثال، در زبان‌هایی که خوب طراحی شده اند، در زمان استفاده از یک متغیر، به روشنی می
توان فهمید که کدام متغیر مورد اشاره است، بدون اینکه نیاز باشد برنامه واقعا اجرا
شود. این کار باعث می شود که از جستجوی متغیر بر اساس نام در هر بار استفاده از آن
جلوگیری شود، و به جای آن مستقیما آن را از قسمت‌های مشخصی از حافظه به‌دست بیاوریم.

به طور سنتی، کامپایل شامل تبدیل برنامه به کد ماشین می شود، فرمت خامی که یک
پردازنده‌ی کامپیوتر می تواند اجرا کند. اما هر روندی که برنامه را به نمایش متفاوتی
تبدیل کند را می توان به عنوان کامپایل در نظر گرفت.

{{index simplicity, "Function constructor", transpilation}}

می توان یک استراتژی ارزیابی جایگزین برای Egg، نوشت که در آن ابتدا برنامه را به
یک برنامه‌ی جاوااسکریپت تبدیل کند، از `Function` برای فراخوانی جاوااسکریپت برای
کامپایل آن استفاده کند و سپس نتیجه را اجرا کند. اگر این کار به درستی انجام شود باعث می
شود که Egg خیلی سریع تر اجرا شود در حالیکه سادگی پیادهسازی را هنوز با خود دارد.

اگر به این موضوع علاقه دارید و قصد دارید مقداری زمان صرف آن کنید، پیشنهاد می
کنم این کامپایلری که ذکر شد را به عنوان یک تمرین پیاده سازی کنید.

## تقلب

{{index "Egg language"}}

زمانی که `if` و `while` را تعریف کردیم، احتمالا متوجه شدید که این دو پوششی کم و بیش
ساده برای `if` و `while` خود جاواسکریپت بودند. به طور مشابه، مقدارها در Egg همان
مقدارهای معمولی جاوااسکریپت هستند.

اگر پیاده‌سازی Egg را که بر اساس جاوااسکریپت ساخته شده است با میزان کار و پیچیدگی
ای که لازم است تا بتوان یک زبان برنامه‌نویسی را مستقیما بر اساس قابلیت‌های خامی
که ماشین می تواند انجام دهد ساخت، مقایسه کنید، تفاوت خیلی قابل توجه است. صرف نظر
از آن، امیدوارم این مثال درکی از روش کارکرد زبان‌های برنامه‌نویسی را به
شما منتقل کرده باشد.

و زمانی‌که لازم است کاری انجام شود، تقلب کردن موثرتر از این است که همه چیز را
خودتان انجام دهید. البته زبانی که برای آموزش در این فصل ایجاد شد کاری را انجام
نمی دهد که از معادلش در جاوااسکریپت بهتر عمل کند￼ اما موقعیت‌هایی وجود دارد که
نوشتن زبان‌های کوچک برای انجام کارهای واقعی کاربرد دارد.

این گونه زبان‌ها نیازی نیست که شبیه به یک زبان برنامه‌نویسی متداول باشند. مثلا اگر
جاوااسکریپت از عبارت‌های باقاعده به صورت درونی پشتیبانی نمی کرد، می توانستید مفسر
و ارزیاب خودتان را برای عبارات باقاعده بنویسید.

{{index "artificial intelligence"}}

یا تصور کنید که در حال ساخت یک دایناسور روباتیک غول پیکر هستید و لازم است تا
رفتار آن را برنامه‌نویسی کنید. جاوااسکریپت ممکن است موثر ترین روش این کار
نباشد، ممکن است در عوض به دنبال زبانی شبیه به زیر باشید.

```{lang: null}
behavior walk
  perform when
    destination ahead
  actions
    move left-foot
    move right-foot

behavior attack
  perform when
    Godzilla in-view
  actions
    fire laser-eyes
    launch arm-rockets
```

{{index expressivity}}

به این گونه زبانها، معمولا _((زبانی با دامنه‌ی خاص))_ نامیده می شوند، زبانی که برای
بیان دامنه‌ی کوچکی از اطلاعات تدارک دیده می شود. این گونه زبان‌ها می توانند نسبت به
یک زبان متداول عام رساتر باشند زیرا طراحی آن‌ها دقیقا برای توصیف چیزهایی
بوده است که در دامنه‌شان وجود دارد نه چیز دیگر.

## تمرین‌ها

### آرایه‌ها

{{index "Egg language", "arrays in egg (exercise)", [array, "in Egg"]}}

پشتیبانی از آرایه‌ها را به Egg اضافه کنید و این کار را با افزودن سه تابع پیش رو
به قلمروی بالایی انجام دهید: تابع <bdo>`array(...values)`</bdo> برای ساختن
آرایه‌ای که حاوی مقدارهای آرگومان است، <bdo>`length(array)`</bdo> برای گرفتن
طول یک آرایه و <bdo>`element(array, n)`</bdo> برای به دست آوردن n^th^ عنصر
آرایه.

{{if interactive

```{test: no}
// Modify these definitions...

topScope.array = "...";

topScope.length = "...";

topScope.element = "...";

run(`
do(define(sum, fun(array,
     do(define(i, 0),
        define(sum, 0),
        while(<(i, length(array)),
          do(define(sum, +(sum, element(array, i))),
             define(i, +(i, 1)))),
        sum))),
   print(sum(array(1, 2, 3))))
`);
// → 6
```

if}}

{{hint

{{index "arrays in egg (exercise)"}}

ساده‌ترین روش انجام آن این است که از آرایه‌های خود جاوااسکریپت برای نمایش آرایه‌های Egg بهره ببرید.

{{index "slice method"}}

مقادیری که به قلمروی بالایی اضافه می ‌شوند باید تابع باشند. با استفاده از یک
آرگومان rest (که با سه نقطه نوشته می شود)، تعریف `array` بسیار ساده خواهد شد.

hint}}

### بستار (Closure)

{{index closure, [function, scope], "closure in egg (exercise)"}}

روشی که برای تعریف `fun` استفاده کردیم به توابع در Egg این امکان را می دهد که به
قلمروی پیرامونشان بتوانند ارجاع دهند، به این صورت که به بدنه‌ی تابع این امکان را می دهد تا از
مقدارهای محلی که در زمان تعریف تابع قابل مشاهده بوده اند استفاده کند درست شبیه
کاری که توابع در جاوااسکریپت انجام می دهند.

برنامه‌ی پیش رو این مفهوم را نشان می‌دهد: تابع `f` یک یک تابع دیگر برمی‌گرداند؛
تابعی که آرگومان‌هایش را با آرگومان‌های `f` جمع می نماید، به این معنا که برای
انجام این کار باید بتواند به قلمروی محلی درون `f` دسترسی داشته باشد تا از مقدار
متغیر `a` استفاده کند.

```
run(`
do(define(f, fun(a, fun(b, +(a, b)))),
   print(f(4)(5)))
`);
// → 9
```

به قسمت تعریف `fun` برگردید و توضیح دهید چه مکانیزمی باعث این رفتار شده است.

{{hint

{{index closure, "closure in egg (exercise)"}}

می‌دانیم که ما از مکانیزم جاوااسکریپت برای ساخت یک قابلیت مشابه در Egg بهره می‌بریم. قلمروی محلی
به صورتی که ارزیابی می‌شوند به شکل‌های خاص داده می‌شوند در نتیجه شکل‌های خاص می توانند زیر‌شکل‌های خودشان را در همان قلمرو ارزیابی کنند. تابعی که توسط `fun` برگردانده می شود به آرگومان `scope`ای که به تابع محصورش داده می‌شود دسترسی دارد و از آن برای ایجاد قلمروی محلی‌اش در هنگام فراخوانی استفاده می کند.

{{index compilation}}

معنای آن این است که خمیرمایه‌ی قلمروی محلی برابر با قلمرویی خواهد بود که در آن تابع ایجاد شده است، که موجب می شود بتوان به متغیرهای آن از درون تابع دسترسی داشت. این تمام چیزی است که در پیاده‌سازی بستار وجود دارد (اگرچه برای کامپایل آن به صورتی که بهینه عمل کند لازم است تا کارهای بیشتری انجام شود).

hint}}

### Comments

{{index "hash character", "Egg language", "comments in egg (exercise)"}}

It would be nice if we could write ((comment))s in Egg. For example,
whenever we find a hash sign (`#`), we could treat the rest of the
line as a comment and ignore it, similar to `//` in JavaScript.

{{index "skipSpace function"}}

We do not have to make any big changes to the parser to support this.
We can simply change `skipSpace` to skip comments as if they are
((whitespace)) so that all the points where `skipSpace` is called will
now also skip comments. Make this change.

{{if interactive

```{test: no}
// This is the old skipSpace. Modify it...
function skipSpace(string) {
  let first = string.search(/\S/);
  if (first == -1) return "";
  return string.slice(first);
}

console.log(parse("# hello\nx"));
// → {type: "word", name: "x"}

console.log(parse("a # one\n   # two\n()"));
// → {type: "apply",
//    operator: {type: "word", name: "a"},
//    args: []}
```
if}}

{{hint

{{index "comments in egg (exercise)", [whitespace, syntax]}}

Make sure your solution handles multiple comments in a row, with
potentially whitespace between or after them.

A ((regular expression)) is probably the easiest way to solve this.
Write something that matches "whitespace or a comment, zero or more
times". Use the `exec` or `match` method and look at the length of the
first element in the returned array (the whole match) to find out how
many characters to slice off.

hint}}

### Fixing scope

{{index [binding, definition], assignment, "fixing scope (exercise)"}}

Currently, the only way to assign a binding a value is `define`.
This construct acts as a way both to define new bindings and to give
existing ones a new value.

{{index "local binding"}}

This ((ambiguity)) causes a problem. When you try to give a nonlocal
binding a new value, you will end up defining a local one with the
same name instead. Some languages work like this by design, but I've
always found it an awkward way to handle ((scope)).

{{index "ReferenceError type"}}

Add a special form `set`, similar to `define`, which gives a binding a
new value, updating the binding in an outer scope if it doesn't
already exist in the inner scope. If the binding is not defined at
all, throw a `ReferenceError` (another standard error type).

{{index "hasOwnProperty method", prototype, "getPrototypeOf function"}}

The technique of representing scopes as simple objects, which has made
things convenient so far, will get in your way a little at this point.
You might want to use the `Object.getPrototypeOf` function, which
returns the prototype of an object. Also remember that scopes do not
derive from `Object.prototype`, so if you want to call
`hasOwnProperty` on them, you have to use this clumsy expression:

```{test: no}
Object.prototype.hasOwnProperty.call(scope, name);
```

{{if interactive

```{test: no}
specialForms.set = (args, scope) => {
  // Your code here.
};

run(`
do(define(x, 4),
   define(setx, fun(val, set(x, val))),
   setx(50),
   print(x))
`);
// → 50
run(`set(quux, true)`);
// → Some kind of ReferenceError
```
if}}

{{hint

{{index [binding, "compilation of"], assignment, "getPrototypeOf function", "hasOwnProperty method", "fixing scope (exercise)"}}

You will have to loop through one ((scope)) at a time, using
`Object.getPrototypeOf` to go to the next outer scope. For each scope,
use `hasOwnProperty` to find out whether the binding, indicated by the
`name` property of the first argument to `set`, exists in that scope.
If it does, set it to the result of evaluating the second argument to
`set` and then return that value.

{{index "global scope", "run-time error"}}

If the outermost scope is reached (`Object.getPrototypeOf` returns
null) and we haven't found the binding yet, it doesn't exist, and an
error should be thrown.

hint}}
