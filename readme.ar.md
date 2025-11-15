# Json
[[English]](readme.md)

مكتبة لقراءة ملفات جيسون (Json) باستخدام لغة الأسس. تقتصر هذه المكتبة حاليا على قراءة الجيسون و ليس إنشاءه.

## الإضافة إلى المشروع

يمكن إضافة المكتبة للمشروع باستعمال مدير الحزم:

<div dir=rtl>

```
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Json"، "جـيسون.أسس")؛
```

</div>

```
import "Apm";
Apm.importFile("Alusus/Json");
```

## مثال

<div dir=rtl>

```
اشمل "مـتم/طـرفية.أسس"؛
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Json"، "جـيسون.أسس")؛

استخدم مـتم؛


دالة ابدا {
    // نقوم بتعريف متغير من النمط جـيسون يحمل البيانات التي نريدها
    عرف كائن: جـيسون = "{\n\t\"id\": 1,\n\t\"classes\": [\"c1\", \"c2\", \"c3\"],\n\t\"passed\": true\n}"

    // نقوم بجلب القيمة المخزنة في المفتاح id كمتغير صحيح
    عرف معرف: صحيح = كائن("id")؛
    طـرفية.اطبع("id is %d\n", معرف)؛

    // نقوم بالتحقق من أن القيمة المخزنة في المفتاح classes هي مصفوفة
    عرف كائن_الصفوف: جـيسون = كائن("classes")؛
    إذا كائن_الصفوف.أهي_مصفوفة() {
        طـرفية.اطبع("Array\n")؛
    }
    وإلا {
        طـرفية.اطبع("Not array\n")؛
    }

    // نقوم بجلب القيمة المخزنة في المفتاح passed كمتغير ثنائي
    عرف نجح: ثنائي = كائن("passed")؛
    // نقوم بالتحقق فيما إذا نجح الطالب أم لا
    إذا نجح {
        طـرفية.اطبع("passed\n")؛
    }
    وإلا {
        طـرفية.اطبع("failed\n")؛
    }
}
ابدا()؛
```

</div>

```
import "Srl/Console.alusus"
import "Apm";
Apm.importFile("Alusus/Json");

use Srl
func start {
    // نقوم بتعريف متغير من النمط جـيسون يحمل البيانات التي نريدها
    def obj: Json = "{\n\t\"id\": 1,\n\t\"classes\": [\"c1\", \"c2\", \"c3\"],\n\t\"passed\": true\n}"

    // كمتغير صحيح id نقوم بجلب القيمة المخزنة في المفتاح
    def id: int = obj("id");
    Console.print("id is %d\n", id);

    // هي مصفوفة classes نقوم بالتحقق من أن القيمة المخزنة في المفتاح
    def classesObj: Json = obj("classes");
    if (classesObj.isArray()) {
        Console.print("Array\n");
    }
    else {
        Console.print("Not array\n");
    }

    // كمتغير ثنائي passed نقوم بجلب القيمة المخزنة في المفتاح
    def passed: Bool = obj("passed");
    // نقوم بالتحقق فيما إذا نجح الطالب أم لا
    if (passed) {
        Console.print("passed\n");
    }
    else {
        Console.print("failed\n");
    }

}
start();
```

## الصنف جـيسون (Json)

### المتغيرات

<div dir=rtl>

```
صنف جـيسون {
    عرف المفاتيح: مصفوفة[نص]؛
    عرف القيم: مصفوفة[جـيسون]؛
    عرف القيمة_الخام: نص؛
}
```

</div>

```
class Json {
    def keys: Array[String];
    def values: Array[Json];
    def rawValue: String;
}
```

`المفاتيح` (`keys`): المفاتيح التي يحويها الجيسون إن كان كائنًا.

`القيم` (`values`) القيم التي يحويها الجيسون إن كان مصفوفة أو كائنًا.

`القيمة_الخام` (`rawValue`): سلسلة نصية للقيمة الحالية إن لم تكن مصفوفة ولا كائنًا.

### التهيئة

يمكن تهيئة هذا الصنف باستعمال العديد من الطرق:

<div dir=rtl>

```
عملية هذا~هيئ()
```

</div>

```
handler this~init()
```
تهيئة افتراضية بدون أي قيم.

<div dir=rtl>

```
عملية هذا~هيئ(نص: مؤشر[مصفوفة[مـحرف]])
```

</div>

```
handler this~init(str: ptr[array[Char]])
```
تهيئة من مصفوفة من المحارف.

<div dir=rtl>

```
عملية هذا~هيئ(جيسون: سند[جـيسون])
```

</div>

```
handler this~init(json: ref[Json])
```

تهيئة من كائن من هذا الصنف، أي تهيئة ناسخة.


### أهو_كائن (isObject)

<div dir=rtl>

```
عرف أهو_كائن(): ثنائي؛
```

</div>

```
handler this.isObject(): bool;
```

وظيفة تعيد فيما إذا كان نص الجيسون هو كائن أم لا.

### أهي_مصفوفة (isArray)

<div dir=rtl>

```
عرف أهي_مصفوفة(): ثنائي؛
```

</div>

```
handler this.isArray(): bool;
```
وظيفة تعيد فيما إذا كان نص الجيسون مصفوفة أم لا.

### هات_الطول (getLength)

<div dir=rtl>

```
عرف هات_الطول(): صحيح؛
```

</div>

```
handler this.getLength(): Int;
```
وظيفة تعيد عدد القيم في نص الجيسون.

### هات_مفتاح (getKey)

<div dir=rtl>

```
عرف هات_مفتاح(تسلسل: صـحيح): نص؛
```

</div>

```
handler this.getKey(index: Int): String;
```

وظيفة تعيد المفتاح المخزن عند التسلسل المعطى إن كان الجيسون كائنًا. ترجع نصا فارغا إن لم يكن الجيسون
كائنا أو كان التسلسل خارج المدى.

## مـكون_منشئ_نص_جيسون (JsonStringBuilderMixin)

`مـكون_منشئ_نص_جيسون` هو مكون يمكن استخدامه مع `مـنشئ_نص` لتوفير إمكانيات تنسيق نصوص جيسون.
يقوم تلقائياً بمعالجة المحارف الخاصة في النصوص عند بناء مخرجات جيسون.

### الاستخدام

<div dir=rtl>

```
اشمل "مـتم/مـنشئ_نص"؛
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Json"، "جـيسون.أسس")؛
استخدم مـتم؛

عرّف منشئ_نص: مـنشئ_نص[مـكون_منشئ_نص_جيسون](512، 512)؛
```

</div>

```
import "Srl/StringBuilder";
import "Apm";
Apm.importFile("Alusus/Json");
use Srl;

def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
```

### محددات التنسيق

يوفر المكون محددات تنسيق خاصة لوظيفة `املأ` (`format`):

- `%جن` أو `%js` - تنسيق قيمة `نـص` (`String`) مع معالجة محارف جيسون الخاصة (معالجة علامات الاقتباس، الخطوط المائلة، إلخ.)
- `%جمح` أو `%jpc` - تنسيق قيمة `مـؤشر_محارف` (`CharsPtr`) مع معالجة محارف جيسون الخاصة

### مثال

<div dir=rtl>

```
اشمل "مـتم/طـرفية"؛
اشمل "مـتم/نـص"؛
اشمل "مـتم/تـطبيق"؛
اشمل "مـتم/مـنشئ_نص"؛
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Json"، "جـيسون.أسس")؛
استخدم مـتم؛

دالة اختبر_مكون_منشئ_نص_جيسون {
    عرّف منشئ_نص: مـنشئ_نص[مـكون_منشئ_نص_جيسون](512، 512)؛
    عرّف قيمة_نص: نـص("اختبار\"اقتباس'")؛
    عرّف قيمة_مؤشر_محارف: مـؤشر_محارف("اختبار\"اقتباس'")؛
    منشئ_نص.املأ("{ \"الاسم\": %جن, \"القيمة\": %جمح }"، قيمة_نص، قيمة_مؤشر_محارف)؛
    طـرفية.اطبع("%s\ج"، منشئ_نص~مثل[نـص].صوان)؛
}
اختبر_مكون_منشئ_نص_جيسون()؛
```

</div>

```
import "Srl/Console";
import "Srl/StringBuilder";
import "Apm";
Apm.importFile("Alusus/Json");
use Srl;

func testJsonStringBuilderMixin {
    def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
    def stringVal: String("test\"quotes'");
    def charsPtrVal: CharsPtr("test\"quotes'");
    stringBuilder.format("{ \"name\": %js, \"value\": %jpc }", stringVal, charsPtrVal);
    Console.print("%s\n", stringBuilder~cast[String].buf);
}
testJsonStringBuilderMixin();
```

المخرجات:
```
{ "name": "test\"quotes'", "value": "test\"quotes'" }
```

