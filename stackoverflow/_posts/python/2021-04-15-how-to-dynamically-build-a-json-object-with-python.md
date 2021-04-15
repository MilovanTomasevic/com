---
layout: post
title: How to dynamically build a JSON object with Python?
image: https://i.ytimg.com/vi/hJ2HfejqppE/maxresdefault.jpg
description: >
  I am new to Python and I am playing with JSON data. I would like to dynamically build a JSON object by adding some key-value to an existing JSON object.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-15
categories: [stackoverflow]
tags: [python, json, convert]
---

How to dynamically build a JSON object with Python?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/23110383/how-to-dynamically-build-a-json-object-with-python) by [Backo](https://stackoverflow.com/users/824806/backo)

I am new to Python and I am playing with JSON data. I would like to dynamically build a JSON object by adding some key-value to an existing JSON object.

I tried the following but I get `TypeError: 'str' object does not support item assignment`:
```py
import json

json_data = json.dumps({})
json_data["key"] = "value"

print 'JSON: ', json_data
```

## [Answer](https://stackoverflow.com/a/23110401/13155046) by [Martijn Pieters♦](https://stackoverflow.com/users/100297/martijn-pieters)

You build the object **before** encoding it to a JSON string:
```py
import json

data = {}
data['key'] = 'value'
json_data = json.dumps(data)
```
JSON is a *serialization* format, textual data *representing* a structure. It is not, itself, that structure.


## [Answer](https://stackoverflow.com/a/66054971/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

- `json.loads` take a string as input and returns a dictionary as output.
- `json.dumps` take a dictionary as input and returns a string as output.

If you need to convert JSON data into a python object, it can do so with `Python3`, in one line without additional installations, using [`SimpleNamespace`][1] and `object_hook`:

### from string

```py
import json
from types import SimpleNamespace

string = '{"foo":3, "bar":{"x":1, "y":2}}'

# Parse JSON into an object with attributes corresponding to dict keys.
x = json.loads(string, object_hook=lambda d: SimpleNamespace(**d))

print(x.foo)
print(x.bar.x)
print(x.bar.y)
```
 
output:

```sh
3
1
2
```

### from file:

JSON object: `data.json`

```json
{
    "foo": 3,
    "bar": {
        "x": 1,
        "y": 2
    }
}
```

```py
import json
from types import SimpleNamespace

with open("data.json") as fh:
    string = fh.read()

# Parse JSON into an object with attributes corresponding to dict keys.
x = json.loads(string, object_hook=lambda d: SimpleNamespace(**d))

print(x.foo)
print(x.bar.x)
print(x.bar.y)
```

output:

```sh
3
1
2
```

### from requests

```py
import json
from types import SimpleNamespace
import requests

r = requests.get('https://api.github.com/users/MilovanTomasevic')

# Parse JSON into an object with attributes corresponding to dict keys.
x = json.loads(r.text, object_hook=lambda d: SimpleNamespace(**d))

print(x.name)
print(x.company)
print(x.blog)
```

output:

```sh
Milovan Tomašević
NLB
milovantomasevic.com
```

For more beautiful and faster access to JSON response from API, take a look at [this response][2].


  [1]: https://lwn.net/Articles/818777/
  [2]: https://stackoverflow.com/questions/38547924/how-to-use-jsonview-in-chrome/65958809#65958809