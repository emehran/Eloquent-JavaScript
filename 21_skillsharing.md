{{meta {code_links: "[\"code/skillsharing.zip\"]"}}}

# پروژه: وب‌سایت اشتراک مهارت‌ها

{{quote {author: "مارگارت فولر", chapter: true}

اگر دانشی دارید، بگذارید دیگران از چراغ دانش‌تان بهره‌مند شوند.

quote}}

{{index "skill-sharing project", meetup, "project chapter"}}

{{figure {url: "img/chapter_picture_21.jpg", alt: "Picture of two unicycles", chapter: "framed"}}}

یک جلسه‌ی اشتراک مهارت، رخدادی است که در آن‌جا افرادی با یک علاقه‌ی مشترک دور هم جمع می‌شوند و دانش خود را به صورت خودمانی و کوتاه ارائه می‌کنند. در یک جلسه‌ی اشتراک مهارت باغبانی، ممکن است فردی به توضیح نحوه‌ی کاشت کرفس بپردازد. یا در یک گروه اشتراک مهارت برنامه‌نویسی، شما می‌توانید شرکت کنید و درباره‌ی Node.js مطلبی ارائه کنید.

{{index learning, "users' group"}}

این‌گونه جلسات- که اغلب وقتی درباره‌ی کامپیوتر است، گروه‌‌های کاربران نامیده می‌شود (users' groups)- روش مناسبی برای وسعت بخشیدن به گستره‌ی دانش‌تان، یادگیری درباره‌ی پیش‌رفت‌ها و توسعه‌های جدید، یا ملاقات افراد جدید با علايق مشترک می‌باشند. خیلی از شهر‌های بزرگ جلسات جاوااسکریپت دارند. معمولا شرکت در این‌گونه‌ جلسات رایگان است، و جلساتی که من تجربه کرده‌ام، همگی گرم و دوستانه بوده اند.

در این فصل پروژه‌ی پایانی، هدف ما این ‌است که وب‌سایتی برای مدیریت ارائه‌هایی که در یک جلسه‌ی اشتراک مهارت اجرا می‌شود، ایجاد کنیم. تصور کنید که گروه کوچکی از کاربران به صورت منظم در دفتر کار یکی از اعضا ملاقات می‌کنند تا درباره‌ی یک‌چرخه‌ها صحبت کنند. مسئول برگزاری پیشین به شهر دیگری نقل مکان کرده است و کسی برای قبول این کار قدم پیش‌ نگذاشته است. ما به سیستمی نیاز داریم که به شرکت کنندگان این امکان را بدهد تا بتوانند ارائه‌ها را پیشنهاد داده و در مورد آن‌ها بحث کنند بدون اینکه نیازی به یک مدیر برگزارکننده‌ی مرکزی باشد.

[درست مانند [فصل پیش](node)، بخشی از کدی که در این فصل می‌آید برای محیط Node.js نوشته شده است، و اجرای مستقیم آن در صفحه‌ی HTMLای کد را در آن مشاهده می‌کنید، بعید است که کار کند.]{if interactive} کد کامل پروژه را می‌توانید از [_https://eloquentjavascript.net/code/skillsharing.zip_](https://eloquentjavascript.net/code/skillsharing.zip) دانلود کنید.

## طراحی

{{index "skill-sharing project", persistence}}

در این پروژه، بخشی مربوط به سرویس‌دهنده است که برای Node.js نوشته شده است، و بخشی مربوط به کلاینت که برای مرورگر نوشته شده است. سرویس‌دهنده داده‌های سیستم را ذخیره می ‌کند و آن‌ها را برای کلاینت فراهم می‌سازد. همچنین میزبانی و سرو فایل‌هایی که بخش سمت کلایت را پیاده سازی می‌کنند نیز با سرویس دهنده است.

{{index [HTTP, client]}}

سرویس‌دهنده لیست ارائه‌هایی که برای جلسه‌ی بعدی پیشنهاد شده اند را نگه‌داری می‌کند و کلاینت آن‌ها را نمایش می‌دهد. هر ارائه دارای یک نام ارائه کننده، یک عنوان، یک خلاصه، و آرایه‌ای از نظرات مرتبط با آن می‌باشد. کلاینت به کاربران امکان پیشنهاد ارائه‌های جدید ( اضافه کردن آن‌ها به لیست)، حذف آن‌ها و ارسال نظر به ارائه‌های فعلی را فراهم می‌سازد. هر بار که کاربر تغییری این‌گونه ایجاد می‌کند، کلاینت یک درخواست HTTP به سرویس‌دهنده‌ برای آن ارسال می کند.

{{figure {url: "img/skillsharing.png", alt: "Screenshot of the skill-sharing website",width: "10cm"}}}

{{index "live view", "user experience", "pushing data", connection}}

اپلیکیشن به صورتی تنظیم خواهد شد که یک نمای زنده از ارائه‌های پیشنهاد شده‌ی کنونی نمایش دهد. هنگامی که کسی، جایی، یک ارائه‌ی جدید ثبت می‌کند یا نظری ارسال می‌کند، همه‌ی افرادی که صفحه‌ی سایت را باز نگه‌داشته اند بایستی تغییرات را بلافاصله ببینند. این ویژگی کمی چالش ایجاد خواهد کرد- زیرا راهی وجود ندارد که سرویس‌دهنده تماسی را به یک کلاینت برقرار سازد، و همچنین راه مناسبی برای دانستن اینکه کدام کلاینت‌ها در حال حاضر در حال مشاهده‌ی یک وب سایت هستند وجود ندارد.

{{index "Node.js"}}

یک راه حل رایج برای این مشکل وجود دارد که _((long polling))_ نامیده می‌شود که یکی از انگیزه‌های موجود برای طراحی Node بوده است.

## Long polling

{{index firewall, notification, "long polling", network, [browser, security]}}

برای اینکه بتوان بلافاصله کلاینت را از تغییری باخبر کرد، لازم است تا ارتباطی با کلاینت برقرار کنیم. با توجه به اینکه مرورگرهای وب از دیرباز درخواست اتصال را قبول نمی‌کنند و اغلب پشت روترهایی هستند که این‌گونه درخواست‌های اتصال را بلاک می‌کنند، ارسال اتصال توسط سرویس‌دهنده کارایی ندارد.

می توانیم کلاینت را طوری هماهنگ کنیم که اتصالی برقرار کرده و آن را باز نگه‌دارد تا سرویس‌دهنده بتواند با کمک آن اطلاعاتی که نیاز است ارسال شود را ارسال کند.

{{index socket}}

اما درخواست‌های HTTP فقط از جریان‌های ساده‌ی داده پشتیبانی می کنند: کلاینت یک درخواست ارسال می‌نماید، سرویس‌دهنده یک پاسخ برای آن درخواست برمی‌گرداند، و فقط همین. فناوری‌ای وجود دارد که WebSockets نام دارد، و توسط مرورگرهای مدرن پشتیبانی می‌شود. به کمک این فناوری، می‌توان اتصالاتی برای تبادل داده‌ها به صورت دلخواه باز نمود. اما استفاده‌ی درست از آن کمی پیچیده است.

در این فصل، ما از تکنیکی ساده تر-((long polling))- استفاده می‌کنیم جایی که کلاینت‌ها به صورت مداوم از سرویس‌دهنده‌ به وسیله‌ی درخواست‌های HTTP معمولی، تقاضای داده‌های جدید می‌کند، و سرویس‌دهنده در صورت نبود چیز جدیدی برای گزارش، ارسال پاسخ را متوقف می کند.

{{index "live view"}}

اگر کلاینت همیشه‌ درخواست بازی از نوع polling داشته باشد، می‌تواند انتظار داشته باشد که در صورت در دسترس قرار گرفتن داده‌ی جدید، آن را به سرعت از سرویس‌دهنده دریافت خواهد کرد. به عنوان مثال، اگر Fatma سایت اشتراک مهارت ما را مرورگرش باز داشته باشد، آن مرورگر درخواستی برای به‌روز‌رسانی‌ها به سرویس‌دهنده‌ خواهد داشت و منتظر پاسخ برای آن می‌ماند. زمانی که Iman ارائه ای را در مورد یک‌چرخه‌‌سواری در شیب‌های تند ثبت می‌کند، سرویس‌دهنده می‌داند که Fatma منتظر خبر جدیدی است و پاسخی حاوی ارائه جدید ثبت شده به درخواست او ارسال می‌کند. مرورگر Fatma داده‌ها را دریافت کرده و صفحه‌ی نمایش را با اطلاعات ارائه‌ی جدید به‌روز می‌کند.

{{index robustness, timeout}}

برای جلوگیری از لغو شدن اتصال‌ها به دلیل نبود فعالیت، تکنیک‌های long polling معمولا یک بیشینه‌ی زمان برای هر درخواست در نظر می‌گیرند، پس از گذشت آن زمان، سرویس‌دهنده پاسخی را به هر حال ارسال می‌کند، حتی درصورتی که چیزی برای گزارش نداشته باشد، که بعد از آن کلاینت دوباره درخواستی ارسال می‌کند. دوباره ارسال مدوام درخواست باعث می‌شود که تکنیک پایدار شود، زیرا به کلاینت امکان پوشش مشکلات موقت سرویس‌دهنده و قطع ارتباط می‌دهد.

{{index "Node.js"}}

یک سرویس‌دهنده‌ی پرترافیک که از تکنیک long polling استفاده می‌کند ممکن است هزاران درخواست منتظر پاسخ داشته باشد، که به معنای همین تعداد اتصال TCP باز می‌باشد. Node، که امکان مدیریت اتصال‌های زیاد بدون نیاز به ایجاد thread‌های مجزا و کنترل آن‌ها را به آسانی فراهم می‌سازد، گزینه‌ی مناسبی برای این‌گونه سیستم‌ها می‌باشد.

## رابط HTTP

{{index "skill-sharing project", [interface, HTTP]}}

پیش از اینکه به سراغ طراحی سرویس‌دهنده یا کلاینت برویم، اجازه‌ دهید به نقطه‌ای که هر دوی‌ آن‌ها با آن ارتباط برقرار می‌کنند فکر کنیم: رابط HTTP که در بستر آن تعامل صورت خواهد گرفت.

{{index [path, URL], [method, HTTP]}}

ما از JSON به عنوان فرمت بدنه‌ی درخواست‌ها و پاسخ‌هایمان استفاده خواهیم کرد. درست مانند سرویس‌دهنده‌ی فایل مربوط به [Chapter ?](node#file_server)، سعی خواهیم کرد که از متد‌های HTTP و سرنام‌ها به درستی بهره ببریم. رابط ما پیرامون مسیر <bdo>`/talks`</bdo> خواهد بود. مسیر‌هایی که با <bdo>`/talks`</bdo> شروع نمی‌شوند برای سرو فایل‌های ایستا-کدهای جاوااسکریپت و HTML مربوط به سیستم کلاینت - استفاده می‌شوند.

{{index "GET method"}}

یک درخواست `GET` به مسیر <bdo>‍‍‍`/talks`</bdo>، یک سند JSON به صورت زیر برمی‌گرداند:

```{lang: "application/json"}
[{"title": "Unituning",
  "presenter": "Jamal",
  "summary": "Modifying your cycle for extra style",
  "comments": []}]}
```

{{index "PUT method", URL}}

ایجاد یک ارائه‌ی جدید به وسیله‌ی یک درخواست `PUT` به آدرسی مانند <bdo>`/talks/Unituning`</bdo> صورت می‌گیرد، جاییکه بخش بعد از اولین اسلش نمایانگر عنوان ارائه می‌باشد. بدنه‌ی درخواست `PUT` باید حاوی یک شیء JSON باشد که که دارای خاصیت‌های `presenter` و `summary` باشد.

{{index "encodeURIComponent function", [escaping, "in URLs"], [whitespace, "in URLs"]}}

با توجه به اینکه عنوان ارائه‌ها ممکن است دارای فاصله یا دیگر کاراکترهایی باشد که ممکن است در URL درست نمایش نیابند، عنوان‌ها باید به وسیله‌ی `encodeURIComponent` در هنگام ساخت URL کدگذاری شوند.

```
console.log("/talks/" + encodeURIComponent("How to Idle"));
// → /talks/How%20to%20Idle
```

یک درخواست برای ایجاد ارائه‌ای درباره‌ی حرکت بدون رکاب‌زدن ممکن است چیزی شبیه درخواست زیر باشد:

```{lang: http}
PUT /talks/How%20to%20Idle HTTP/1.1
Content-Type: application/json
Content-Length: 92

{"presenter": "Maureen",
 "summary": "Standing still on a unicycle"}
```

همچنین این‌گونه URLها درخواست‌های `GET` برای دریافت نمایش JSON یک ارائه و درخواست `DELETE` برای حذف یک ارائه را پشتیبانی می‌کند.

{{index "POST method"}}

افزودن یک نظر (comment) به یک ارائه به وسیله‌ی یک درخواست `POST` به یک URL شبیه به <bdo>`/talks/Unituning/comments`</bdo> صورت می‌گیرد که بدنه‌ی درخواست به صورت JSON و دارای خاصیت‌های `message` و `author` می‌باشد.

```{lang: http}
POST /talks/Unituning/comments HTTP/1.1
Content-Type: application/json
Content-Length: 72

{"author": "Iman",
 "message": "Will you talk about raising a cycle?"}
```

{{index "query string", timeout, "ETag header", "If-None-Match header"}}

برای پشتیبانی از تکنیک ((long polling))، درخواست‌های `GET` به <bdo>`/talks`</bdo> باید سرنام‌های بیشتری داشته باشند تا به سرویس‌دهنده خبر دهند که در صورت نبود خبر جدید،‌ ارسال پاسخ را باید به تاخیر بیاندازد. ما از یک جفت سرنام که معمولا برای مدیریت کش(حافظه‌ی نهان) استفاده می‌شوند، `ETag` و `If-None-Match` استفاده می‌کنیم.

{{index "304 (HTTP status code)"}}
سرویس‌دهنده‌ها ممکن است یک سرنام `Etag` (برچسب موجودیت) در یک پاسخ قرار دهند. مقدار آن رشته‌ای است که نسخه‌ی فعلی منبع را مشخص می‌کند. کلاینت‌ها، وقتی در آینده آن منبع را درخواست می‌کنند، ممکن است یک درخواست شرطی بسازند که این کار را با قرار دادن سرنام `If-None-Match` و مقداری مشابه مقدار `Etag` در درخواست انجام می‌دهند. اگر منبع مورد نظر تغییر نکرده است، سرویس‌دهنده پاسخی با کد وضعیت 304 ارسال می‌کند که معنای آن "تغییر نکرده است" می‌باشد. این پاسخ به کلاینت می‌گوید که نسخه‌ی کش شده مشابه نسخه‌ی فعلی است. زمانی که مقدار برچسب متفاوت بود، سرویس‌دهنده به صورت عادی پاسخ می‌دهد.

{{index "Prefer header"}}

ما به چیزی مثل این نیاز داریم، کلاینت بتواند به سرویس‌دهنده بگوید که کدام نسخه از لیست ارائه‌ها را دارد، و سرویس‌دهنده فقط زمانی پاسخ دهد که آن لیست به‌روز شده است. اما به‌جای اینکه بلافاصله پاسخی با کد 304 برگرداند، سرویس‌دهنده باید پاسخ را نگه‌دارد و فقط زمانی پاسخ دهد که چیزی تغییر کرده یا زمان مشخصی طی شده است. برای ایجاد تمایز بین درخواست‌های long polling و درخواست‌های معمولی، سرنام دیگری، `Prefer: wait=90`، اضافه ‌می‌کنیم که به سرویس‌دهنده می‌گوید کلاینت تا 90 ثانیه می‌تواند برای پاسخ صبر کند.

سرویس‌دهنده یک شماره‌ نسخه که با هر بار تغییر ارائه‌ها به‌روز می‌شود را نگه‌داری کرده و آن را به عنوان مقدار `ETag` استفاده می‌کند. کلاینت‌ها می‌توانند درخواست‌هایی مانند زیر را ارسال کنند تا با بروز یک تغییر از آن باخبر شوند:

```{lang: null}
GET /talks HTTP/1.1
If-None-Match: "4"
Prefer: wait=90

(time passes)

HTTP/1.1 200 OK
Content-Type: application/json
ETag: "5"
Content-Length: 295

[....]
```

{{index security}}

پروتکلی که اینجا شرح داده شد هیچ کنترل درخواستی را انجام نمی‌دهد. هر کسی می‌تواند نظر دهد، ارائه‌ها را تغییر دهد، و حتی حذف‌شان کند. (با توجه به اینکه اینترنت پر است از افراد خراب‌کار، اگر سیستمی این‌چنینی را جایی در اینترنت بدون لایه‌ای محافظ قرار دهید، نباید انتظار پایان خوشی داشته باشید.
)

## سرویس‌دهنده

{{index "skill-sharing project"}}

خب بیایید ساخت بخش مربوط به سرویس‌دهنده را شروع کنیم. کد این بخش در محیط Node اجرا می‌شود.

### Routing (مسیرگزینی)

{{index "createServer function", [path, URL], [method, HTTP]}}
سرویس‌دهنده‌ی ما از `createServer` برای شروع یک سرویس‌دهنده‌ی HTTP استفاده می‌کند. در تابعی که قرار است درخواست‌های جدید را مدیریت کند، باید بین انواع درخواست‌هایی که ما پشتیبانی می‌کنیم تمایز قائل شویم (با توجه به متد درخواست و مسیر درخواستی). این کار را می‌تواند به وسیله‌ی یک زنجیره‌ی بلند از دستورات `if` انجام داد، اما راه نیکوتری وجود دارد.

{{index dispatch}}

یک _((router))_ یک مؤلفه است که به ما کمک می‌کند تا یک درخواست را به تابعی که می‌تواند آن را رسیدگی کند گسیل دهیم. می‌توانید برای مسیرگزین(router) مشخص کنید که مثلا درخواست‌های `PUT` که دارای مسیری باشند که با عبارت باقاعده‌ی <bdo>`/^\/talks\/([^\/]+)$/`</bdo> تطابق داشته باشد (<bdo>`/talks/`</bdo> و پس از آن یک عنوان) می‌توانند با یک تابع داده شده رسیدگی شوند. افزون بر آن، مسیرگزین می‌تواند برای استخراج بخش‌های معنادار مسیر (در این مورد عنوان ارائه) استفاده شود بخشی که در عبارت باقاعده درون پرانتز قرار دارد و نتیجه را به تابع رسیدگی‌کننده ارسال کند.

در NPM چندین بسته‌ی خوب برای مدیریت مسیرها (routing) وجود دارد، اما ما اینجا نسخه‌ی خودمان را می‌نویسیم تا قواعد آن را روشن کنیم.

{{index "require function", "Router class", module}}

فایلی به نام `router.js` وجود دارد که در ادامه از ماژول سرویس‌دهنده‌ی ما `require` خواهد شد:

```{includeCode: ">code/skillsharing/router.js"}
const {parse} = require("url");

module.exports = class Router {
  constructor() {
    this.routes = [];
  }
  add(method, url, handler) {
    this.routes.push({method, url, handler});
  }
  resolve(context, request) {
    let path = parse(request.url).pathname;

    for (let {method, url, handler} of this.routes) {
      let match = url.exec(path);
      if (!match || request.method != method) continue;
      let urlParts = match.slice(1).map(decodeURIComponent);
      return handler(context, ...urlParts, request);
    }
    return null;
  }
};
```

{{index "Router class"}}

ماژول ما کلاس `Router` را صادر (export) می‌کند. یک شیء router امکان ثبت گرداننده‌های جدید را به وسیله‌ی متد `add` فراهم می‌سازد و می‌تواند درخواست‌ها را به وسیله‌ی متد `resolve` نتیجه‌یابی کند.

{{index "some method"}}

متد دوم در صورت پیدا کردن یک گرداننده، یک پاسخ برمی‌گرداند و درغیر این صورت، مقدار `null` را تولید می‌کند. این متد مسیر‌ها را یک به یک (به ترتیبی که تعریف شده اند) امتحان می‌کند تا زمانی که مسیر منطبق پیدا شود.

{{index "capture group", "decodeURIComponent function", [escaping, "in URLs"]}}

توابع گرداننده با مقدار `context` (که در اینجا نمونه‌ی سرویس‌دهنده است)، رشته‌های تطبیقی برای هر گروهی که در عبارت باقاعده‌شان تعریف شده، و شیء درخواست(request)، فراخوانی می‌شوند. رشته باید کدگشایی URL شده باشد زیرا ممکن است URL دارای کدهای شبیه به `%20` باشد.

### سرو کردن فایل‌ها

زمانی که درخواستی با هیچ‌یک از انواع درخواست تعریف شده در مسیر‌گزین ما (router) تطبیق نمی‌یابد، سرویس‌دهنده باید آن درخواست را درخواستی برای یک فایل در پوشه‌ی `public` تفسیر کند. می‌توان از سرویس‌دهنده‌ی فایلی که در [فصل ?](node#file_server) ایجاد شد برای سرو این‌گونه فایل‌ها استفاده کرد، اما ما نه نیاز به پشتیبانی از `PUT` و `DELETE` داریم نه قصد آن را داریم، همچنین دوست داریم ویژگی‌های پیشرفته‌ای مثل پشتیبانی از حافظه‌ی نهان (کش) را داشته باشیم. پس اجازه بدهید از یک بسته‌ی آزمایش‌شده و کارآمد سرویس‌دهنده‌ی فایل موجود در NPM استفاده کنیم.

{{index "createServer function", "ecstatic package"}}

انتخاب من `ecstatic` بود. خب این فقط تنها سرویس‌دهنده‌ی موجود در NPM نبود، اما برای کار ما مناسب است و به خوبی کار می‌کند. بسته‌ی `ecstatic` تابعی را فراهم می‌سازد که می‌توان آن را با یک شیء تنظیمات فراخواند تا یک تابع گرداننده‌ی درخواست به وجود آورد. ما از گزینه‌ی `root` استفاده می‌کنیم تا به سرویس‌دهنده اعلام کنیم که کجا باید به دنبال فایل‌ها بگردد. تابع گرداننده پارامترهای `response` و `request` را دریافت می کند و می‌توان آن را مستقیما به `createServer` فرستاد تا سرویس‌دهنده‌ای ایجاد کنیم که فقط فایل‌ها را سرو می‌کند. با توجه به اینکه قصد داریم ابتدا درخواست‌هایی را بررسی کنیم که باید به صورت خاص رسیدگی شوند، بنابراین آن را توسط تابع دیگری پوشش می‌دهیم.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
const {createServer} = require("http");
const Router = require("./router");
const ecstatic = require("ecstatic");

const router = new Router();
const defaultHeaders = {"Content-Type": "text/plain"};

class SkillShareServer {
  constructor(talks) {
    this.talks = talks;
    this.version = 0;
    this.waiting = [];

    let fileServer = ecstatic({root: "./public"});
    this.server = createServer((request, response) => {
      let resolved = router.resolve(this, request);
      if (resolved) {
        resolved.catch(error => {
          if (error.status != null) return error;
          return {body: String(error), status: 500};
        }).then(({body,
                  status = 200,
                  headers = defaultHeaders}) => {
          response.writeHead(status, headers);
          response.end(body);
        });
      } else {
        fileServer(request, response);
      }
    });
  }
  start(port) {
    this.server.listen(port);
  }
  stop() {
    this.server.close();
  }
}
```

کد بالا برای پاسخ‌ها از سبکی مشابه سرویس‌دهنده‌ی فایل [فصل پیش](node) استفاده می‌کند- گرداننده‌ها promise برمی‌گردانند که به اشیائی منتج می‌شوند که پاسخ را مشخص می‌کنند. سرویس دهنده درون یک شیء قرار می‌گیرد که همچنین وضعیت آن را نیز نگه‌داری می‌کند.

### ارائه‌ها به صورت منابع

ارائه‌های پیشنهادی در خاصیت `talks` سرویس‌دهنده ذخیره می‌شوند، شیئی که نام خاصیت‌های آن عنوان‌های ارائه‌ها می‌باشد. این ارائه‌ها به عنوان منابع HTTP در آدرس <bdo>`/talks/[title]`</bdo> در معرض دسترسی قرار می‌گیرند، بنابراین ما نیاز به گرداننده‌هایی داریم که به مسیرگزین‌مان اضافه شوند و متدهای متنوعی که کلاینت‌ها می‌توانند برای کار با آن‌ها استفاده کنند را پیاده سازی کنند.

{{index "GET method", "404 (HTTP status code)"}}

گرداننده‌ی درخواست‌های `GET` برای دریافت یک ارائه باید به دنبال ارائه بگردد و پاسخی حاوی اطلاعات ارائه به صورت JSON یا یک خطای 404 را برگرداند.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
const talkPath = /^\/talks\/([^\/]+)$/;

router.add("GET", talkPath, async (server, title) => {
  if (title in server.talks) {
    return {body: JSON.stringify(server.talks[title]),
            headers: {"Content-Type": "application/json"}};
  } else {
    return {status: 404, body: `No talk '${title}' found`};
  }
});
```

{{index "DELETE method"}}

حذف یک ارائه با پاک کردن آن از شیء `talks` صورت می‌گیرد.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
router.add("DELETE", talkPath, async (server, title) => {
  if (title in server.talks) {
    delete server.talks[title];
    server.updated();
  }
  return {status: 204};
});
```

{{index "long polling", "updated method"}}

متد `updated`، که در [ادامه](skillsharing#updated) آن را تعریف خواهیم کرد، درخواست‌های long polling را از وجود تغییر باخبر می‌کند.

{{index "readStream function", "body (HTTP)", stream}}

برای بازیابی محتوای یک بدنه‌ی درخواست، تابعی تعریف می‌کنیم که `readStream` نام دارد، که همه‌ی محتوای یک استریم قابل خواندن را می‌خواند و یک promise برمی‌گرداند که به یک رشته منتج می‌شود.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
function readStream(stream) {
  return new Promise((resolve, reject) => {
    let data = "";
    stream.on("error", reject);
    stream.on("data", chunk => data += chunk.toString());
    stream.on("end", () => resolve(data));
  });
}
```

{{index validation, input, "PUT method"}}

گرداننده‌ای که نیاز است بدنه‌های درخواست‌ها را بخواند گرداننده‌ی `PUT` می‌باشد، که این گرداننده برای ایجاد ارائه‌های جدید استفاده می‌شود. بررسی اینکه داده‌های داده شده دارای خاصیت‌های `presenter` و `summary` باشد به عهده‌ای این گرداننده است. داده‌هایی که از بیرون از سیستم می‌آیند ممکن است بی‌معنا باشد، و ما قصد نداریم مدل داده‌ درونی‌مان را خراب کنیم یا در صورت دریافت درخواستی بد، سرویس‌دهنده از کار بیفتد.

{{index "updated method"}}

اگر داده‌ها معتبر باشند، گرداننده شیئی که نمایانگر ارائه جدید است را در شیء `talks` ذخیره می‌کند، احتمالا ارائه ای با همین نام را بازنویسی می‌کند و دوباره تابع `updated` را فراخوانی می‌کند.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
router.add("PUT", talkPath,
           async (server, title, request) => {
  let requestBody = await readStream(request);
  let talk;
  try { talk = JSON.parse(requestBody); }
  catch (_) { return {status: 400, body: "Invalid JSON"}; }

  if (!talk ||
      typeof talk.presenter != "string" ||
      typeof talk.summary != "string") {
    return {status: 400, body: "Bad talk data"};
  }
  server.talks[title] = {title,
                         presenter: talk.presenter,
                         summary: talk.summary,
                         comments: []};
  server.updated();
  return {status: 204};
});
```

{{index validation, "readStream function"}}

افزودن یک نظر به یک ارائه به همین صورت است. از `readStream` برای گرفتن محتوای درخواست استفاده می کنیم، داده‌ی به‌ دست آمده را اعتبارسنجی می‌کنیم و در صورت معتبر بودن آن را به صورت یک نظر ذخیره می‌کنیم.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
router.add("POST", /^\/talks\/([^\/]+)\/comments$/,
           async (server, title, request) => {
  let requestBody = await readStream(request);
  let comment;
  try { comment = JSON.parse(requestBody); }
  catch (_) { return {status: 400, body: "Invalid JSON"}; }

  if (!comment ||
      typeof comment.author != "string" ||
      typeof comment.message != "string") {
    return {status: 400, body: "Bad comment data"};
  } else if (title in server.talks) {
    server.talks[title].comments.push(comment);
    server.updated();
    return {status: 204};
  } else {
    return {status: 404, body: `No talk '${title}' found`};
  }
});
```

{{index "404 (HTTP status code)"}}

تلاش برای اضافه کردن یک نظر به ارائه ای که وجود ندارد منجر به بازگشتن خطای 404 می‌گردد.

### پشتیبانی از Long Polling

جالب ترین بخش سرویس‌دهنده، بخشی است که تکنیک long polling را انجام می‌دهد. زمانی که یک درخواست `GET` برای <bdo>‍‍‍`/talks`</bdo> دریافت می‌شود، ممکن است یک درخواست معمولی یا یک درخواست به سبک long polling باشد.

{{index "talkResponse method", "ETag header"}}

با توجه به اینکه در موارد متعددی لازم است که آرایه‌ای از ارائه‌ها را به کلاینت ارسال کنیم، ابتدا یک متد کمکی تعریف می کنیم که این‌ آرایه را برای ما بسازد و سرنام `ETag` را در پاسخ قرار دهد.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
SkillShareServer.prototype.talkResponse = function() {
  let talks = [];
  for (let title of Object.keys(this.talks)) {
    talks.push(this.talks[title]);
  }
  return {
    body: JSON.stringify(talks),
    headers: {"Content-Type": "application/json",
              "ETag": `"${this.version}"`}
  };
};
```

{{index "query string", "url package", parsing}}
گرداننده خود نیاز دارد تا سرنام‌های درخواست را بررسی کند تا مطمئن شود سرنام‌های `If-None-Match` و `Prefer` موجود باشند. Node سرنام‌ها را که نامشان به صورت غیرحساس به بزرگی/کوچکی حروف مشخص می‌شود را با حروف کوچک ذخیره می‌کند.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
router.add("GET", /^\/talks$/, async (server, request) => {
  let tag = /"(.*)"/.exec(request.headers["if-none-match"]);
  let wait = /\bwait=(\d+)/.exec(request.headers["prefer"]);
  if (!tag || tag[1] != server.version) {
    return server.talkResponse();
  } else if (!wait) {
    return {status: 304};
  } else {
    return server.waitForChanges(Number(wait[1]));
  }
});
```

{{index "long polling", "waitForChanges method", "If-None-Match header", "Prefer header"}}

اگر برچسبی داده نشده بود یا برچسب داده شده با نسخه‌ی کنونی سرویس‌دهنده منطبق نبود، گرداننده لیست‌ ارائه ها را برمی‌گرداند. اگر درخواست شرطی باشد و ارائه ها تغییری نکرده باشند، ما سرنام `Prefer` را بررسی می‌کنیم تا ببینیم که آیا لازم است پاسخ‌دادن را به تاخییر بیاندازیم یا باید سریع پاسخ دهیم.

{{index "304 (HTTP status code)", "setTimeout function", timeout, "callback function"}}

توابع callback برای درخواست‌هایی که به تاخیر انداخته شده اند در آرایه‌ی `waiting` سرویس‌دهنده ذخیره می‌شوند در نتیجه در هنگام تغییر چیزی می‌توان آن‌ها را باخبر کرد. متد `waitForChanges` همچنین بلافاصله یک زمان‌سنج برای پاسخ با یک کد وضعیت 304 تنظیم می‌کند که در صورتی‌که درخواست به مدت طولانی منتظر بماند عمل خواهد کرد.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
SkillShareServer.prototype.waitForChanges = function(time) {
  return new Promise(resolve => {
    this.waiting.push(resolve);
    setTimeout(() => {
      if (!this.waiting.includes(resolve)) return;
      this.waiting = this.waiting.filter(r => r != resolve);
      resolve({status: 304});
    }, time * 1000);
  });
};
```

{{index "updated method"}}

{{id updated}}

ثبت یک تغییر به وسیله‌ی `updated` باعث افزایش شماره نسخه، خاصیت ‍`version`، می‌شود و همه‌ی درخواست‌های در انتظار را بیدار می‌کند.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
SkillShareServer.prototype.updated = function() {
  this.version++;
  let response = this.talkResponse();
  this.waiting.forEach(resolve => resolve(response));
  this.waiting = [];
};
```

{{index [HTTP, server]}}

کد سرویس‌دهنده اینجا به پایان می‌رسد. اگر ما یک نمونه از `SkillShareServer` ایجاد کرده و روی درگاه 8000 اجرا کنیم، سرویس‌دهنده‌ی ایجاد شده، فایل ها را از زیرپوشه‌ی `public` به همراه یک رابط مدیریت ارائه تحت مسیر <bdo>`/talks`</bdo> سرو می‌کند.

```{includeCode: ">code/skillsharing/skillsharing_server.js"}
new SkillShareServer(Object.create(null)).start(8000);
```

## کلاینت

{{index "skill-sharing project"}}

بخش مربوط به کلاینت وب‌سایت اشتراک مهارت از سه فایل تشکیل می‌شود: یک صفحه‌ی HTML ساده، یک برگه‌ی سبک CSS، و یک فایل جاوااسکریپت.

### HTML

{{index "index.html"}}
یکی از قراردادهای بسیار پراستفاده در سرویس‌دهنده‌های وب این است که در صورت دریافت درخواستی مستقیم به مسیری که به یک پوشه ختم می‌شود، سرویس‌دهنده تلاش می‌کند تا فایلی به نام `index.html` را سرو کند. ماژول سرویس‌دهنده‌ی فایلی که ما استفاده می‌کنیم، ‍`ecstatic`، از این قرارداد پشتیبانی می‌کند. زمانی که یک درخواست به مسیر `/` ارسال می‌شود، سرویس‌دهنده به دنبال فایل <bdo>`./public/index.html`</bdo> می‌گردد (<bdo>`./public`</bdo> ریشه‌ای است که ما تعیین کرده ایم) و آن فایل را در صورت وجود بازمی‌گرداند.

بنابراین، اگر قصد داریم صفحه‌ای را در هنگام باز شدن سرویس‌دهنده‌مان نمایش دهیم، باید آن صفحه را در <bdo>`public/index.html`</bdo> قرار دهیم. فایل index ما به شکل زیر می‌باشد:

```{lang: "text/html", includeCode: ">code/skillsharing/public/index.html"}
<!doctype html>
<meta charset="utf-8">
<title>Skill Sharing</title>
<link rel="stylesheet" href="skillsharing.css">

<h1>Skill Sharing</h1>

<script src="skillsharing_client.js"></script>
```

{{index CSS}}
این فایل عنوان صفحه را تعریف کرده و یک فایل CSS اضافه می‌کند. فایل CSS تعدادی سبک تعریف می کند و علاوه بر چند کار دیگر، فاصله‌ی بین ارائه‌ها را تنظیم می‌کند.

در ادامه، یک سرعنوان به بالای صفحه اضافه می‌کند و اسکریپتی که کد کلاینت اپلیکیشن را دارد را نیز بارگیری می‌کند.

### کنش‌ها(actions)

وضعیت اپلیکیشن حاوی لیست ارائه‌ها و نام کاربر می‌باشد، و ما آن را در یک شیء `{talks, user}` ذخیره می‌کنیم. رابط کاربری مستقیما اجازه‌ی دستکاری وضعیت و ارسال درخواست HTTP را نخواهد داشت. در عوض، رابط کنش‌ها (actions) را گسیل می‌دهد که عمل مورد نظر کاربر را توضیح می‌دهند.

{{index "handleAction function"}}

تابع `handleAction` یک action گرفته و به آن عمل می‌کند. با توجه به اینکه به‌روز‌رسانی‌های وضعیت خیلی ساده می‌باشند، تغییر وضعیت در همان تابع صورت می‌گیرد.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function handleAction(state, action) {
  if (action.type == "setUser") {
    localStorage.setItem("userName", action.user);
    return Object.assign({}, state, {user: action.user});
  } else if (action.type == "setTalks") {
    return Object.assign({}, state, {talks: action.talks});
  } else if (action.type == "newTalk") {
    fetchOK(talkURL(action.title), {
      method: "PUT",
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify({
        presenter: state.user,
        summary: action.summary
      })
    }).catch(reportError);
  } else if (action.type == "deleteTalk") {
    fetchOK(talkURL(action.talk), {method: "DELETE"})
      .catch(reportError);
  } else if (action.type == "newComment") {
    fetchOK(talkURL(action.talk) + "/comments", {
      method: "POST",
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify({
        author: state.user,
        message: action.message
      })
    }).catch(reportError);
  }
  return state;
}
```

{{index "localStorage object"}}
نام کاربر را در `localeStorage` ذخیره می‌کنیم در نتیجه می‌توان آن را با بارگیری صفحه بازیابی کرد.

{{index "fetch function", "status property"}}

این کنش‌ها نیاز دارند تا با سرویس‌دهنده برای ساخت درخواست‌های شبکه با استفاده از `fetch` و به رابط HTTPای که پیش‌تر توصیف شد تعامل کنند. ما از یک تابع پوشش‌دهنده به نام `fetchOK` استفاده می‌کنیم، که باعث می‌شود اطمینان حاصل شود که promise برگشتی در صورت تولید خطا توسط سرویس‌دهنده لغو شود.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function fetchOK(url, options) {
  return fetch(url, options).then(response => {
    if (response.status < 400) return response;
    else throw new Error(response.statusText);
  });
}
```

{{index "talkURL function", "encodeURIComponent function"}}

تابع کمکی زیر برای ساخت یک URL برای یک ارائه با یک عنوان داده‌شده استفاده می‌شود.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function talkURL(title) {
  return "talks/" + encodeURIComponent(title);
}
```

{{index "error handling", "user experience", "reportError function"}}

زمانی که یک درخواست با مشکل روبرو می‌شود، دوست‌ نداریم صفحه‌ی کاربر بدون هیچ توضیحی ثابت بماند. پس تابعی تعریف می‌کنیم به نام `reportError` که حداقل به کاربر متنی را نشان می‌دهد که چیزی با مشکل روبرو شده است.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function reportError(error) {
  alert(String(error));
}
```

### ساخت و نمایش مؤلفه‌ها

{{index "renderUserField function"}}

از روشی مشابه آنچه در [فصل ?](paint) دیدیم، که تقسیم اپلیکیشن به مؤلفه‌ها بود استفاده می‌کنیم. اما با توجه به اینکه بعضی از مؤلفه‌ها هرگز نیاز به‌ به‌روز‌رسانی ندارند یا در صورت به‌روز شدن، از نو به صورت کامل بازایجاد می‌شوند، ما آن‌ها را نه بصورت کلاس بلکه به شکل توابعی تعریف می‌کنیم که مستقیما یک گره‌ی DOM برمی‌گردانند. به عنوان مثال، در اینجا مؤلفه‌ای داریم که فیلدی را نشان می دهد که کاربر می‌تواند نامش را در آن وارد کند:

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function renderUserField(name, dispatch) {
  return elt("label", {}, "Your name: ", elt("input", {
    type: "text",
    value: name,
    onchange(event) {
      dispatch({type: "setUser", user: event.target.value});
    }
  }));
}
```

{{index "elt function"}}

تابع `elt` برای ساخت عناصر DOM استفاده می‌شود، همانی است که در [فصل ?](paint) استفاده می‌کردیم.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no, hidden: true}
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

{{index "renderTalk function"}}

تابع مشابهی برای ساخت و نمایش ارائه‌ها در صفحه ‌استفاده می شود، که لیستی از نظر‌ها و فرمی برای افزودن یک نظر جدید را نیز شامل می‌شود.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function renderTalk(talk, dispatch) {
  return elt(
    "section", {className: "talk"},
    elt("h2", null, talk.title, " ", elt("button", {
      type: "button",
      onclick() {
        dispatch({type: "deleteTalk", talk: talk.title});
      }
    }, "Delete")),
    elt("div", null, "by ",
        elt("strong", null, talk.presenter)),
    elt("p", null, talk.summary),
    ...talk.comments.map(renderComment),
    elt("form", {
      onsubmit(event) {
        event.preventDefault();
        let form = event.target;
        dispatch({type: "newComment",
                  talk: talk.title,
                  message: form.elements.comment.value});
        form.reset();
      }
    }, elt("input", {type: "text", name: "comment"}), " ",
       elt("button", {type: "submit"}, "Add comment")));
}
```

{{index "submit event"}}

گرداننده‌ رخداد `"submit"` متد `form.reset` را فراخوانی می‌کند تا محتوای فرم را پس از ایجاد یک کنش `"newComment"` پاک کند.

در صورت ایجاد بخش نسبتا پیچیده‌ای از DOM، این سبک از برنامه‌نویسی در ابتدا کمی شلوغ‌ به نظر می‌رسد. افزونه‌ی پراستفاده‌ای برای جاوااسکریپت (غیراستاندارد) وجود دارد که JSX نام دارد و به شما امکان نوشتن مستقیم HTML درون اسکریپت‌های جاوااسکریپت را می‌دهد که می‌تواند این‌گونه کدها را زیبا تر کند (البته بسته به اینکه شما کد زیبا را چگونه ارزیابی کنید). پیش از اینکه بتوانید این کد را اجرا کنید، باید برنامه‌ای روی اسکریپت‌تان اجرا کنید تا کدهای شبه‌HTML را به فراخوانی‌های تابع جاوااسکریپت تبدیل کند بسیار شبیه به آن چه اینجا استفاده کردیم.

ساخت و نمایش نظرها ساده تر است.

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function renderComment(comment) {
  return elt("p", {className: "comment"},
             elt("strong", null, comment.author),
             ": ", comment.message);
}
```

{{index "form (HTML tag)", "renderTalkForm function"}}

سرانجام، فرمی که کاربر برای ایجاد یک ارائه‌ی جدید استفاده می‌کند به صورت زیر ایجاد می‌شود:

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function renderTalkForm(dispatch) {
  let title = elt("input", {type: "text"});
  let summary = elt("input", {type: "text"});
  return elt("form", {
    onsubmit(event) {
      event.preventDefault();
      dispatch({type: "newTalk",
                title: title.value,
                summary: summary.value});
      event.target.reset();
    }
  }, elt("h3", null, "Submit a Talk"),
     elt("label", null, "Title: ", title),
     elt("label", null, "Summary: ", summary),
     elt("button", {type: "submit"}, "Submit"));
}
```

### Polling

{{index "pollTalks function", "long polling", "If-None-Match header", "Prefer header", "fetch function"}}

برای راه‌اندازی اپلیکیشن به لیست ارائه‌های موجود نیاز داریم. به دلیل اینکه بارگیری ابتدایی بسیار به فرایند long polling مرتبط است - `ETag` به دست آمده از بارگیری باید در هنگام درخواست polling استفاده شود - تابعی خواهیم نوشت که به ارسال درخواست polling به سرویس‌دهنده برای <bdo>`/talks`</bdo> ادامه خواهد داد و هنگامی که مجموعه‌ی جدیدی از ارائه‌ها در دسترس باشد، یک تابع callback فراخوانی می‌کند .

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
async function pollTalks(update) {
  let tag = undefined;
  for (;;) {
    let response;
    try {
      response = await fetchOK("/talks", {
        headers: tag && {"If-None-Match": tag,
                         "Prefer": "wait=90"}
      });
    } catch (e) {
      console.log("Request failed: " + e);
      await new Promise(resolve => setTimeout(resolve, 500));
      continue;
    }
    if (response.status == 304) continue;
    tag = response.headers.get("ETag");
    update(await response.json());
  }
}
```

{{index "async function"}}

این تابع از نوع `async` می‌باشد در نتیجه استفاده از حلقه و انتظار برای این درخواست در آن ساده‌ تر خواهد بود. این تابع حلقه‌ای بی‌نهایت را اجرا می‌کند که در هر تکرار، لیستی از ارائه‌ها را بازیابی می‌کند، یا به صورت عادی، یا اگر این اولین درخواست نباشد، با سرنام‌های اضافه شده که باعث شده این درخواست long polling در نظر گرفته شود.

{{index "error handling", "Promise class", "setTimeout function"}}

زمانی که یک درخواست با مشکل روبرو می‌شود، تابع اندکی صبر می‌کند و سپس دوباره تلاش می‌کند. با این کار، اگر اتصال شبکه برای لحظه‌ای قطع شود و دوباره برگردد، نرم‌افزار می‌تواند خودش را بازیابی کند و به به‌روز‌رسانی ادامه دهد. promise منتج شده به وسیله‌ی `setTimeout` روشی‌ است که تابع `async` را وادار می‌سازد اندکی منتظر بماند.

{{index "304 (HTTP status code)", "ETag header"}}

هنگامی‌که سرویس‌دهنده پاسخی با کد 304 برمی‌گرداند، معنای آن این است که یک درخواست long polling منقضی شده است، بنابراین تابع باید بدون درنگ به سراغ راه‌اندازی درخواست بعدی برود. اگر پاسخ یک پاسخ عادی 200 باشد، بدنه‌ی آن به عنوان JSON خوانده شده و به callback ارسال می‌شود، و مقدار سرنام `ETag` آن برای تکرار بعدی ذخیره می‌شود.

### اپلیکیشن

{{index "SkillShareApp class"}}

مؤلفه‌ی پیش‌رو، همه‌ی اجزاء رابط کاربری را گردآوری می‌کند:

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
class SkillShareApp {
  constructor(state, dispatch) {
    this.dispatch = dispatch;
    this.talkDOM = elt("div", {className: "talks"});
    this.dom = elt("div", null,
                   renderUserField(state.user, dispatch),
                   this.talkDOM,
                   renderTalkForm(dispatch));
    this.syncState(state);
  }

  syncState(state) {
    if (state.talks != this.talks) {
      this.talkDOM.textContent = "";
      for (let talk of state.talks) {
        this.talkDOM.appendChild(
          renderTalk(talk, this.dispatch));
      }
      this.talks = state.talks;
    }
  }
}
```

{{index synchronization, "live view"}}

زمانی که ارائه‌ها تغییر می‌کنند، این مؤلفه همه‌ی آن‌ها را بازترسیم می‌نماید. این کار ساده اما اضافی است. در قسمت تمرین‌ها به سراغ این مشکل خواهیم رفت.

می‌توانیم اپلیکیشن را به صورت زیر راه‌اندازی کنیم:

```{includeCode: ">code/skillsharing/public/skillsharing_client.js", test: no}
function runApp() {
  let user = localStorage.getItem("userName") || "Anon";
  let state, app;
  function dispatch(action) {
    state = handleAction(state, action);
    app.syncState(state);
  }

  pollTalks(talks => {
    if (!app) {
      state = {user, talks};
      app = new SkillShareApp(state, dispatch);
      document.body.appendChild(app.dom);
    } else {
      dispatch({type: "setTalks", talks});
    }
  }).catch(reportError);
}

runApp();
```

اگر سرویس‌دهنده را اجرا کنید و دو صفحه‌ی مرورگر را برای [_http://localhost:8000_](http://localhost:8000/) در کنار هم باز کنید، می‌توانید مشاهده نمایید که کارهایی که در یک پنجره انجام می‌دهید در دیگر پنجره قابل مشاهده است.

## تمرین‌ها

{{index "Node.js", NPM}}

تمرین‌های این قسمت درباره‌ی ایجاد تغییر روی سیستمی است که در این فصل ایجاد شده است. برای کار روی آن‌ها، اطمینان حاصل کنید کد‌های مورد نیاز را ([_https://eloquentjavascript.net/code/skillsharing.zip_](https://eloquentjavascript.net/code/skillsharing.zip)) بارگیری کنید، Node را نصب کنید [_https://nodejs.org_](https://nodejs.org)، و وابستگی‌های پروژه را نیز به وسیله‌ی `npm install` نصب کنید.

### مانایی داده‌ها در دیسک

{{index "data loss", persistence, [memory, persistence]}}
سرویس‌دهنده‌ی سایت اشتراک مهارت داده‌هایش را کاملا در حافظه نگه‌داری می‌کند. معنای آن این است که در صورت متوقف شدن یا شروع مجدد به هر دلیلی، تمامی ارائه‌ها و نظرات از بین خواهند رفت.

{{index "hard drive"}}

سرویس‌دهنده را توسعه دهید تا بتواند داده‌های مربوط به ارائه‌ها را روی دیسک ذخیره کرده و به صورت خودکار در صورت شروع مجدد بازیابی کند. به بهینگی فکر نکنید و ساده‌ترین راهی که کار می‌کند را انتخاب کنید.

{{hint

{{index "file system", "writeFile function", "updated method", persistence}}

ساده‌ترین راه‌حلی که من می‌توانم پیشنهاد دهم این است که کل شیء `talks` را به صورت JSON کدگذاری کنید و درون یک فایل به وسیله‌ی `writeFile` ذخیره کنید. هم‌اکنون متدی به نام `updated` موجود می‌باشد که با هر بار تغییر داده‌های سرویس‌دهنده فراخوانی می‌شود. می‌توان آن را توسعه داد تا داده‌های جدید را در دیسک ذخیره کند.

{{index "readFile function"}}

یک نام برای فایل انتخاب کنید مثلا ‍<bdo>`./talks.json`</bdo>. در زمانی که سرویس‌دهنده شروع به کار می‌کند، می‌تواند آن فایل را به وسیله‌ی `readFile` بخواند و اگر این خواندن موفقیت آمیز بود، محتوای فایل خوانده شده را می‌تواند به عنوان داده‌های ابتدایی استفاده کند.

{{index prototype, "JSON.parse function"}}

مراقب باشید. شیء `talks` در ابتدا به عنوان یک شیء بدون prototype آغاز شد، در نتیجه می‌توان از عملگر `in` با خیال راحت استفاده کرد. خروجی <bdo>`JSON.parse`</bdo> اشیاء معمولی با پروتوتایپ ‍`Object.prototype` می باشد. اگر از JSON به عنوان فرمت فایل‌تان استفاده کنید، لازم می‌شود تا خاصیت‌های شیئی که توسط `JSON.parse` تولید می‌شود را به یک شیء بدون prototype کپی کند.

hint}}

### بازنشانی فیلد ورود نظرات

{{index "comment field reset (exercise)", template, [state, "of application"]}}

کلیت بازترسیم ارائه‌ها به خوبی کار می‌کند زیرا معمولا تفاوت بین یک گره‌ی DOM و جایگزین مشابه‌ش تشخیص داده نمی‌شود. اما استثناهایی هم وجود دارد. اگر شروع به تایپ چیزی به عنوان یک نظر در یکی از پنجره‌های مرورگر در فیلد مربوط به آن کنید، و سپس در دیگر پنجره، یک نظر به یک ارائه اضافه کنید، فیلد پنجره‌ی اول بازترسیم خواهد شد که در نتیجه هم محتوایش و هم focus روی آن از بین می‌رود.

در یک بحث داغ، جاییکه چندین کاربر در حال افزودن نظراتشان در یک زمان می‌باشند، این ایراد ممکن است آزاردهنده باشد. می‌توانید راه حلی برای آن پیدا کنید؟

{{hint

{{index "comment field reset (exercise)", template, "syncState method"}}
احتمالا بهترین روش انجام این‌کار این است که اشیاء مولفه برای ارائه‌ها بسازیم، به همراه یک متد `syncState`، در نتیجه می‌توان آن‌ها را به‌روز کرد تا یک نسخه‌ی تغییریافته‌ی یک ارائه را نشان دهند. در زمان انجام کارهای عادی، تنها راهی که یک ارائه می‌تواند تغییر کند اضافه کردن نظرات بیشتر است، پس متد `syncState` می‌تواند نسبتا ساده باشد.

بخش مشکل ماجرا این است که، زمانی که یک لیست تغییر یافته از ارائه‌ها می‌آید، ما باید لیست مؤلفه‌های موجود DOM را با ارائه‌های موجود در لیست جدید یکپارچه کنیم- با حذف مؤلفه‌هایی که ارائه‌هایش حذف شده است و به‌روز‌رسانی مؤلفه‌هایی که ارائه‌ش تغییر یافته است.

{{index synchronization, "live view"}}

برای انجام این کار، شاید مفید باشد که ساختار داده‌ای داشته باشیم که مؤلفه‌های ارائه را تحت عنوان‌های ارائه ذخیره کند در نتیجه می‌توان به سادگی وجود یک مؤلفه برای ارائه‌ی داده شده را بررسی کرد. بعدا می‌توانید آرایه‌ی جدید ارائه‌ها را پیمایش کرده و برای هر یک از آن‌ها، یا یک مؤلفه‌ی موجود را هماهنگ کنید یا مؤلفه‌ی جدید را بسازید. برای حذف مؤلفه‌ها برای ارائه‌هایی که حذف شده اند، لازم است همچنین مؤلفه‌ها را پیمایش کنید و تا ببینید ارائه‌های مربوط هنوز موجود هستند یا خیر.

hint}}
