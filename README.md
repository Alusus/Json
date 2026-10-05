# Json

[[عربي]](README.ar.md)

A JSON library for Alusus Language. It supports reading and extracting information from JSON, serializing
objects to JSON, and parsing JSON directly into typed objects.

## Adding to the Project

You can add it to the project using APM:

```
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
```

## Example

```
import "Srl/Console.alusus"
import "Apm";
Apm.importPackage("Alusus/Json@0.2");

use Srl
func start {
    // define a variable with type Json to hold the information we want.
    def obj: Json = "{\n\t\"id\": 1,\n\t\"classes\": [\"c1\", \"c2\", \"c3\"],\n\t\"passed\": true\n}"

    // get the value stored under the key `id` as integer variable.
    def id: int = obj("id");
    Console.print("id is %d\n", id);

    // check if the value stored under the key `classes` is an array.
    def classesObj: Json = obj("classes");
    if (classesObj.isArray()) {
        Console.print("Array\n");
    }
    else {
        Console.print("Not array\n");
    }

    // get the value stored under the key `passed` as boolean variable
    def passed: Bool = obj("passed");
    // check if the student passed the exam or not.
    if (passed) {
        Console.print("passed\n");
    }
    else {
        Console.print("failed\n");
    }

}
start();
```

## Json class

### Member Variables

```
class Json {
    def keys: Array[String];
    def values: Array[Json];
    def rawValue: String;
}
```

`keys`: keys of the JSON if it's an object.

`values`: values of the JSON if it's an array or an object.

`rawValue`: raw value (unparsed) if the JSON is neither an object nor an array.

### Initialization

We can initialize this class using many methods:

```
handler this~init()
```
default initialization without parameters.

```
handler this~init(str: ptr[array[Char]])
```
initialization from an array of char.

```
handler this~init(json: ref[Json])
```
initialization from another object of the class, copy initialization.

### isObject

```
handler this.isObject(): bool;
```
Checks if the JSON is an object.

### isArray

```
handler this.isArray(): bool;
```
Checks if the JSON is an array or not.

### getLength

```
handler this.getLength(): Int;
```
Get the number of values in the JSON. This will return a positive number if the JSON is an object
or an array.

### getKey

```
handler this.getKey(index: Int): String;
```
Returns the key at at the specified index if the JSON is an object. Returns an empty string if
the JSON is not an object or the index is out of range.

### stringfy

```
func stringfy [T: type] (obj: ref[T]): String;
```

Serializes an object to a JSON string.

### parse

```
func parse [T: type] (obj: ref[T], str: CharsPtr);
func parse [T: type] (obj: ref[T], json: ref[Json]);
```

Deserializes JSON into an existing object.

The first takes the JSON as a string, the second takes a json object.

#### Missing Keys

A field whose key is missing from the JSON is set to its default value.

#### Memory

`parse` allocates memory for some field types, and you must free it yourself to avoid memory leaks:

* `CharsPtr` is allocated with `Memory.alloc`. Free it with `Memory.free(obj.field)`.
* `ref[T]` is allocated with `newObj[T]`. Free it with `freeObj[obj.field]`.

Every `CharsPtr` inside a container is allocated separately, so each element must be freed:

```
def i: Int;
for i = 0, i < obj.names.getLength(), ++i Memory.free(obj.names(i));   // names: Array[CharsPtr]
```

Parsing into the same object again allocates new memory for its `CharsPtr` and `ref[T]` fields
without freeing the old memory. Free those fields first.

### Supported Field Types

`stringfy` and `parse` support the following field types. `X` can be any of these types, so types can
be nested to any depth, for example `Map[String, Array[Nullable[Int]]]`.

* `String` and `CharsPtr`, written as a JSON string. A null `CharsPtr` is written as `null`.
* `Bool`, written as `true` or `false`.
* `Int` (`Int[32]`), `Int[64]`, `Float` (`Float[32]`), and `Float[64]`, written as a number.
* `Nullable[X]`, written as the value, or `null` when it's unset.
* `Array[X]`, written as a JSON array.
* `Map[String, X]`, written as a JSON object.
* Any class, including classes inside modules and template classes, written as a nested JSON object.
* `ref[X]` as a field's type, written as the value, or `null` when it's empty.
* `SrdRef[X]` and `UnqRef[X]`, written as the value, or `null` when they're empty.
* `WkRef[X]`, written as the value, or `null` when it's empty (`stringfy` only).

These types are not supported and give a build error (`SPPH1015`) on the field:

* Maps whose key is not `String`, such as `Map[Int, X]`.
* Pointers other than `CharsPtr`, and fixed-size arrays (`array[T, n]`).
* `ref[X]` inside a container or `Nullable`, such as `Array[ref[X]]`.
* `UnqRef[X]` inside a container or `Nullable`, since a unique reference can't be copied.
* `WkRef[X]` in `parse` since `WkRef` never owns the object it points to.

## Reading values from a `Json` object directly

Use an explicit cast. Because a `Json` value can be
converted both to `String` and to `Nullable[String]`, an implicit assignment such as
`def name: Nullable[String] = json("name");` fails with `SPPA1005`. Write:

```
def json: Json("{\"name\": \"Sarmed\"}");
def name: Nullable[String] = json("name")~cast[Nullable[String]];
```

## JsonStringBuilderMixin

The `JsonStringBuilderMixin` is a mixin that can be used with `StringBuilder` to provide JSON string
formatting capabilities. It automatically escapes special characters in strings when building JSON output.

### Usage

```
import "Srl/StringBuilder";
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
use Srl;

def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
```

### Format Specifiers

The mixin provides special format specifiers for the `format` method:

- `%js` - Format a `String` value with proper JSON escaping (escapes quotes, backslashes, etc.)
- `%jpc` - Format a `CharsPtr` value with proper JSON escaping
- `%jns` - Format a `Nullable[String]` value, writing `null` if it has no value, otherwise applying the same JSON escaping as `%js`

### Example

```
import "Srl/Console";
import "Srl/StringBuilder";
import "Srl/Nullable";
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
use Srl;

func testJsonStringBuilderMixin {
    def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
    def stringVal: String("test\"quotes'");
    def charsPtrVal: CharsPtr("test\"quotes'");
    def nullableVal: Nullable[String]("test\"quotes'");
    def nullVal: Nullable[String];
    stringBuilder.format(
        "{ \"name\": %js, \"value\": %jpc, \"nullable\": %jns, \"null\": %jns }",
        stringVal, charsPtrVal, nullableVal, nullVal
    );
    Console.print("%s\n", stringBuilder~cast[String].buf);
}
testJsonStringBuilderMixin();
```

Output:
```
{ "name": "test\"quotes'", "value": "test\"quotes'", "nullable": "test\"quotes'", "null": null }
```

---

## License

This project is licensed under the MIT license. See the `LICENSE` file for details.

