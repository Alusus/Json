# Json
[[عربي]](readme.ar.md)

A JSON parser for Alusus Language. For now, this library supports only reading and extracting information from JSON,
not creating JSONs.

## Adding to the Project

You can add it to the project using APM:

```
import "Apm";
Apm.importFile("Alusus/Json");
```

## Example

```
import "Srl/Console.alusus"
import "Apm";
Apm.importFile("Alusus/Json");

use Srl
func start {
    // define a variable with type Json to hold the information we want.
    def obj: Json = "{\n\t\"id\": 1,\n\t\"classes\": [\"c1\", \"c2\", \"c3\"],\n\t\"passed\": true\n}"

    // get the value stored under the key `id` as integer variable.
    def id: int = obj.getInt("id");
    Console.print("id is %d\n", id);

    // check if the value stored under the key `classes` is an array.
    def classesObj: Json = obj.getObject("classes");
    if (classesObj.isArray()) {
        Console.print("Array\n");
    }
    else {
        Console.print("Not array\n");
    }

    // get the value stored under the key `passed` as boolean variable
    def passed: Bool = obj.getBool("passed");
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
    def jsonString: String;
    def keys: Array[String];
    def values: Array[String];
}
```

`jsonString`: string that represents the information stored as Json.

`keys`: keys contained in `jsonString`.

`values`: values stored under `keys`.

### Initialization

We can initialize this class using many methods:

```
handler this~init()
```
default initialization without parameters.

```
handler this~init(str: String)
```
initialization from a string.

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
checks if `jsonString` is an object or not.

### isArray

```
handler this.isArray(): bool;
```
checks if `jsonString` is an array or not.

### getLength

```
handler this.getLength(): Int;
```
get the number of values in the `jsonString`.

### get

```
handler this.get(key: ptr[array[Char]]): String;
```
get the value stored under `key`.

parameters:

`key`: the key as an array of char.

```
handler this.get(index: Int): String;
```
get the value with index number `index`.

parameters:

`index`: the index of the value we want in the array of values.

note: if the value is a string, then it is returned with the quotation marks.

### getString

```
handler this.getString(key: ptr[array[Char]]): String;
handler this.getString(index: Int): String;
```
Calls the right version of method `get` and returns the result without the quotation marks.

### getObject

```
handler this.getObject(key: ptr[array[Char]]): Json;
handler this.getObject(index: Int): Json;
```
Calls the right version of the method `get` and returns the result as an object of class `Json`.

### getInt

```
handler this.getInt(key: ptr[array[Char]]): Int;
handler this.getInt(index: Int): Int;
```
Calls the right version of the method `get` and returns the result converted to the type `int`.

### getFloat

```
handler this.getFloat(key: ptr[array[Char]]): Float;
handler this.getFloat(index: Int): Float
```
Calls the right version of the method `get` and returns the result converted to the type `float`.

### getBool

```
handler this.getBool(key: ptr[array[Char]]): Bool
handler this.getBool(index: Int): Bool;
```
Calls the right version of the method `get` and returns the result converted to the type `bool`.

