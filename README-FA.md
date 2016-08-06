<p dir="rtl">
راهنمای اصول و قواعد کدنویسی به زبان سوئیفت.
</p>

<p dir="rtl">
هدف از این راهنما، تشویق به استفاده از الگوهای نوشتاری است تا اهداف زیر (به ترتیب اهمیت تقریبی) برآورده شود:
</p>

<ol dir="rtl">
<li> افزایش دقت و کاهش احتمال خطاهای برنامه نویسی </li>
<li>افزایش شفافیت در نیت</li>
<li>کاهش میزان کدنویسی (پرگویی در کد)</li>
<li>مباحثه کمتر در مورد زیبایی‌شناسی کد</li>
 </ol>

<p dir="rtl">
اگر پیشنهادی دارید، لطفاً <a href="CONTRIBUTING.md">راهنمای کمک</a> به ما را مطالعه کنید.:zap:
</p>

----

<h4 dir="rtl">فاصله‌گذاری</h4>
<ul dir="rtl">
<li>به جای Space از Tab استفاده کنید.</li>
<li>فایل‌ها را با یک خط خالی در انتها ببندید.</li>
<li>آزادانه از فاصله‌ی عمودی (خط‌های خالی‌) استفاده کنید تا کد خود را به تکه‌های منطقی تقسیم کنید.</li>
<li>فاصله‌های حاشیه‌ای بی مورد اضافه نکنید.</li>
<ul><li>حتی خطوط جدید را با عقب‌بردگی شروع نکنید.</li></ul>

</ul>

<h4 dir="rtl">در صورت امکان استفاده از `let` را به استفاده از `var` ترجیح دهید</h4>
<p dir="rtl">
هر جا ممکن بود (و اگر شک داشتید) به جای <span dir="ltr">`var foo = ...`</span> از <span dir="ltr">`let foo = ...`</span> استفاده کنید. فقط وقتی از `var` استفاده کنید که مطلقاً مجبور باشید. (یعنی وقتی که می‌دانید مقدار متغیر ممکن است تغییر کند. برای مثال وقتی که متغیر شما از نوع `weak` باشد).
</p>
<p dir="rtl"><em>استدلال:</em> هدف و مفهوم هر دو عبارت واضح است، اما استفاده‌ی پیش فرض از `let` موجب اطمینان و شفافیت بیشتر کد می‌شود.</p>
<p dir="rtl">استفاده از `let` نه تنها تضمین می‌کند که مقدار تغییر نمی‌کند، این مفهوم را به روشنی به برنامه‌نویس منتقل می‌کند. در نتیجه کدها پس از آن می‌توانند با اطمینان از این عدم تغییر نوشته شوند.
</p>
<p dir="rtl">
بدین ترتیب توجیه کدها ساده‌تر می‌شود. اگر شما از `var` استفاده کنید و همچنان فرض کنید که مقدار تغییر نمی‌کند، مجبور خواهید بود خودتان آن‌را بیازمایید.
</p>
<p dir="rtl">
به همین ترتیب، هر جایی یک `var` مشاهده کردید، می‌توانید فرض کنید که مقدار آن تغییر می‌کند و از خودتان بپرسید چرا؟
</p>

<h4 dir="rtl">زودتر از تابع بازگردید و یا حلقه را متوقف کنید</h4>
<p dir="rtl">اگر شرایط خاصی برای ادامه عملکرد کد خود دارید، سعی کنید در صورت امکان سریعتر از آن خارج شوید. در نتیجه، به جای:</p>
```swift
if n.isNumber {
    // Use n here
} else {
    return
}
```

<p dir="rtl">از این کد استفاده کنید:</p>

```swift
guard n.isNumber else {
    return
}
// Use n here
```
<p dir="rtl">شما می‌توانید از `if` هم استفاده کنید، اما استفاده از `guard` ارجح است، زیرا `guard` بدون `return`، `break` و یا `continue` خطای زمان کامپایل ایجاد می‌کند، در نتیجه خروج بموقع تضمین می‌شود.</p>

<h4 dir="rtl">از گشایش اجباری متغیر‌های اختیاری اجتناب کنید</h4>

<p dir="rtl">
اگر یک متغیر به نام `foo` و از نوع <span dir="ltr">`FooType?`</span> یا <span dir="ltr">`FooType!`</span> دارید، در صورت امکان آنرا به اجبار بازگشایی نکنید تا به مقدار آن دسترسی پیدا کنید <span dir="ltr">(`foo!`)</span>.
</p>
<p dir="rtl">این روش ارجح است:</p>

```swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}
```
<p dir="rtl">همچنین ممکن است در بعضی از این حالات بخواهید از قابلیت تداوم زنجیره‌ی اختیاری در سوئیفت استفاده کنید.</p>

```swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```

<p dir="rtl"><em>استدلال:</em> استفاده‌ی غیر ضمنی از `if let` برای متغیر‌های انتخابی منتج به کدی مطمئن‌تر می‌شود. گشایش اجباری موجبات خطاهای زمان اجرا را فراهم می‌کند.</p>

<h4 dir="rtl">از متغیر‌های اختیاری ظاهراً اجباری استفاده نکنید</h4>

<p dir="rtl">اگر `foo` ممکن است nil باشد، در صورت امکان از <span dir="ltr">`let foo: FooType?`</span> به جای <span dir="ltr">`let foo: FooType!`</span> استفاده کنید. (توجه داشته باشید که عموماً می‌توان به جای `!` از `?` استفاده کرد.)</p>

<p dir="rtl"><em>استدلال:</em> متغیرهای اختیاری غیر ضمنی، منتج به کدهای مطمئن‌تر می‌شوند. متغیرهای اختیاری ظاهراً اجباری امکان خطاهای زمان اجرا را فراهم می‌کنند.</p>

<h4 dir="rtl">توابع بازگرداننده‌ی ضمنی را به متغیر‌های فقط خواندنی و یا زیرمجموعه‌خوانی ترجیح دهید</h4>

<p dir="rtl">هرگاه امکان‌پذیر بود، عبارت `get` را از متغیر‌های فقط خواندنی ویا زیر‌محموعه‌خانی‌های فقط خواندنی حذف کنید.</p>
<p dir="rtl">در نتیجه بنویسید:</p>

```swift
var myGreatProperty: Int {
	return 4
}

subscript(index: Int) -> T {
    return objects[index]
}
```

<p dir="rtl">و ننویسید:</p>

```swift
var myGreatProperty: Int {
	get {
		return 4
	}
}

subscript(index: Int) -> T {
    get {
        return objects[index]
    }
}
```

<p dir="rtl"><em>استدلال:</em>هدف و مفهوم مدل اول مشخص است و در نتیجه کد کمتری خواهیم نوشت.</p>

<h4 dir="rtl">همیشه سطح دسترسی را برای تعاریف سطح بالا مشخص کنید</h4>

<p dir="rtl">توابع سطح بالا، تعریف نوع و یا متغیرها باید همیشه تعریف مشخص سطح دسترسی داشته باشند:</p>

```swift
public var whoopsGlobalState: Int
internal struct TheFez {}
private func doTheThings(things: [Thing]) {}
```

<p dir="rtl">ولیکن تعاریف درونی آنها، هرگاه مناسب بود، می توانند سطح دسترسی ضمنی داشته باشند:</p>

```swift
internal struct TheFez {
	var owner: Person = Joshaber()
}
```

<p dir="rtl"><em>استدلال:</em> به ندرت مناسب است که تعاریف سطح بالا `internal` باشند، بیان صریح آن تضمین می‌کند که به این موضوع قبل از تصمیم‌گیری فکر کنیم. در داخل آن تعاریف، استفاده مجدد همان سطح دسترسی تکرار مکررات است و مقادیر پیش‌فرض معمولاً مناسب هستند.</p>

<h4 dir="rtl">هنگام اتلاق نوع ، همواره دو نقطه را به مشخصه بچسبانید</h4>

<p dir="rtl">هنگامی که نوع یک مشخصه را به آن اتلاق می کنید، همواره دونقطه را به اسم آن بپجسانید، سپس یک فاصله بگذارید و سپس نوع آن را بنویسید.</p>

```swift
class SmallBatchSustainableFairtrade: Coffee { ... }

let timeToCoffee: NSTimeInterval = 2

func makeCoffee(type: CoffeeType) -> Coffee { ... }
```

<p dir="rtl"><em>استدلال:</em> موئلفه‌ی نوع، مطلبی در باره‌ی آن مشخصه می‌گوید، درنتیجه باید کنار آن قرار گیرد.</p>

<p dir="rtl">همچنین، وقتی یک نوع را در یک دیکشنری مشخص می‌کنید، همواره دونقطه را بلافاصله پس از کلید بنویسید، در ادامه یک فاصله بگذارید و سپس نوع مقدار را بنویسید.</p>

```swift
let capitals: [Country: City] = [ Sweden: Stockholm ]
```

<h4 dir="rtl">فقط در صورت نیاز صراحتاً از `self` استفاده کنید.</h4>

<p dir="rtl">هر گار به متغیرها و یا توابع `self` نیاز دارید، اشاره به `self` را به قرینه‌ی ضمنی حذف کنید.</p>

```swift
private class History {
	var events: [Event]

	func rewrite() {
		events = []
	}
}
```

<p dir="rtl">فقط به وقتی که بالاجبار مجبور هستید، به طور مثال در یک closure و یا در صورت تداخل نام‌ها عبارت مورد نظر را استفاده کنید:</p>

```swift
extension History {
	init(events: [Event]) {
		self.events = events
	}

	var whenVictorious: () -> () {
		return {
			self.rewrite()
		}
	}
}
```

<p dir="rtl"><em>استدلال:</em> بدین‌تریب بعد معنایی تسخیر `self` بارزتر می‌شود و از زیاده‌نویسی در جاهای دیگر خودداری می‌شود.</p>

<h4 dir="rtl">استفاده از struct را به استفاده از class ترجیح دهید</h4>

<p dir="rtl">به جز در مواردی که نیازمندی دارید که تنها با کلاس قابل پیاده سازی است (مانند هویت شی و یا deinitializers) شی مورد نظر را با استفاده از struct ها پیاده سازی کنید.</p>
<p dir="rtl">توجه داشته باشید که وراثت به نوبه خود و به تنهایی، دلیل خوبی برای استفاده از class نیست، زیرا چند وجهی بودن را می توان با استفاده از پروتکل ها و استفاده‌ی مجدد از کدها را از طریق ترکیب می‌توان پیاده‌سازی کرد.</p>

<p dir="rtl">برای مثال، وراثت در این class:</p>

```swift
class Vehicle {
    let numberOfWheels: Int

    init(numberOfWheels: Int) {
        self.numberOfWheels = numberOfWheels
    }

    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(numberOfWheels)
    }
}

class Bicycle: Vehicle {
    init() {
        super.init(numberOfWheels: 2)
    }
}

class Car: Vehicle {
    init() {
        super.init(numberOfWheels: 4)
    }
}
```

<p dir="rtl">می‌تواند با این تعاریف خلاصه‌سازی شود:</p>

```swift
protocol Vehicle {
    var numberOfWheels: Int { get }
}

func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
    return pressurePerWheel * Float(vehicle.numberOfWheels)
}

struct Bicycle: Vehicle {
    let numberOfWheels = 2
}

struct Car: Vehicle {
    let numberOfWheels = 4
}
```

<p dir="rtl"><em>استدلال:</em> متغیرهای مقداری با استفاده از `let` رفتار قابل پیش‌بینی‌تری دارند.</p>

<h4 dir="rtl">کلاس‌ها را به طور پیش‌فرض `final` تعریف کنید</h4>

<p dir="rtl">کلا‌س‌ها باید  به شکل `final` شروع شوند و فقط وقتی  باید اجازه Subclassing داده شود که یک نیاز واقعی و صحیح برای وراثت تشخیص داده شده باشد. حتی در این‌صورت نیز تا آنجایی که ممکن است، تعاریف داخلی آن کلاس باید `final` باشند و به نوبه خود از این قاعده پیروی کنند.</p>
<p dir="rtl"><em>استدلال:</em> ترکیب عموماً به وراثت ارجحیت دارد و انتخاب وراثت بدین معنی خواهد بود که در مورد آن به اندازه کافی فکر شده است.</p>

<h4 dir="rtl">در صورت امکان از نوشتن پارامترِ نوع خودداری کنید.</h4>

<p dir="rtl">متدهای کلاس‌هایی که نوع در  آنها بصورت پارامتر تعریف شده، می توانند از ذکر آن صرفنظر کنند (در شرایطی که نوع مطرح شده با آنجا در کلاس تعریف شده یکی باشد.) برای مثال:</p>

```swift
struct Composite<T> {
	…
	func compose(other: Composite<T>) -> Composite<T> {
		return Composite<T>(self, other)
	}
}
```

 <p dir="rtl">می‌توانست به این شکل باشد:</p>

```swift
struct Composite<T> {
	…
	func compose(other: Composite) -> Composite {
		return Composite(self, other)
	}
}
```

<p dir="rtl"><em>استدلال:</em> صرف‌نظر کردن از پارامترهای نوعِ اضافی می‌تواند منطق وجود آن‌ها را روشن‌تر کرده و همچنین شفافیت بیشتری به هنگامی که مقدار بازگشتی نوع متفاوتی دارد ایجاد می کند.</p>

<h4 dir="rtl">اطراف تعاریف عملگرها فاصله گذاری کنید</h4>

<p dir="rtl">هنگام تعریف عملگرها در اطراف آن فضای خالی در نظر بگیرید. به جای:</p>

```swift
func <|(lhs: Int, rhs: Int) -> Int
func <|<<A>(lhs: A, rhs: A) -> A
```

<p dir="rtl">بنویسید:</p>

```swift
func <| (lhs: Int, rhs: Int) -> Int
func <|< <A>(lhs: A, rhs: A) -> A
```
<p dir="rtl"><em>استدلال:</em> عملگرهای شامل حروف نقطه‌گذاری می‌شوند که در صورت قرار گرفتن در کنار نقطه‌گذاری در اطراف آن، موجب سختی در خواندن کد می‌شوند. اضافه کردن فاصله در اطراف عملگر موجب خوانایی بیشتر کد می‌شود.</p>

<h5 dir="rtl">پانویسِ مترجم:</h5>
<p dir="rtl">ترجمه‌ی این متن به دلایل زیر انجام شد:</p>
<ul dir="rtl">
	<li>وجود یک راهنمای خوب به زبان فارسی برای کد نویسان فارسی زبان</li>
	<li>نشان دادن این موضوع که ترجمه‌ی متون فنی و بویژه در حوزه‌ی برنامه نویسی، کار بسیار بیهوده‌ای است، چونکه بیشتر کلمات برگردان غلط و گمراه‌کننده ای هستند که غالباً نمی‌توانند مفهوم را به خوبی منتقل کنند. لذا پیشنهاد آن است که تا آنجا که می ‌توانید در جهت فراگیری زبان انگلیسی تخصصی برای حرفه‌ی خود تلاش کنید!</li>
</ul>

<h4 dir="rtl">ترجمه‌ها</h4>

* [中文版](https://github.com/Artwalk/swift-style-guide/blob/master/README_CN.md)
* [日本語版](https://github.com/jarinosuke/swift-style-guide/blob/master/README_JP.md)
* [한국어판](https://github.com/minsOne/swift-style-guide/blob/master/README_KR.md)
* [Versión en Español](https://github.com/antoniosejas/swift-style-guide/blob/spanish/README-ES.md)
* [Versão em Português do Brasil](https://github.com/fernandocastor/swift-style-guide/blob/master/README-PTBR.md)
