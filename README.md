# tinyjson
A lightweight, minimal json library for lily

The implementation for this library was inspired by the yojson library for Ocaml and follows some of the same conventions.

A json value is defined as an enum:

```
enum Json {
    _Null,
    _Bool(Boolean),
    _Int(Integer),
    _Float(Double),
    _String(String),
    _Assoc(Hash[String, Json]),
    _List(List[Json])
}
```
There are some convenience functions defined to make dealing with the tree structure more sane:

```
# find the json value associated with the a key in a
# json value
define member(Json, String): Json {...}

# convert a json list value into a lily list
define toList(Json): List[Json] {...}

# convert a json value into a lily value
define toInt/String/Float/Bool(Json): Integer/String/Double/Boolean {...}

# convert a json value list into a list of lily values
define filterInt/String/Float/Bool(List[Json]): List[Integer/String/Double/Boolean] {...}

# filter a list of json values to a list of values that the given member maps to
define filterMember(j: List[Json], mem: String): List[Json]

# print out a json value
define printJson(Json) {...}

# decode a string into a json value
define decode(String): Json {...}

# encode a json value into a string
define encode(Json): String {...}
```

When a json function encounters an error, it throws an exception called 'JsonError'
which has the member function 'printError':
```
try: {
    # code
except json.JsonError as je:
    je.printError()
}
```

Example:

```
import json

var j = json.decode("{\"foo\": \"bar\", \"nums\": [1,2,3]}")
var s = json.encode(json.Json._Assoc(["foo" => json.Json._String("bar")]))

try: {
    var str = json.member(j, "foo")
    json.printJson(val)
    print("")

    var numbers = json.member(j, "nums")
    json.filterInt(json.toList(val)).each(|num| print(num))

except json.JsonError as je:
    je.printError()
}

```

Todo:
* Add support for escaped characters
* Add support for hex numbers
* Add support for exponents (e.g., 23e3)
