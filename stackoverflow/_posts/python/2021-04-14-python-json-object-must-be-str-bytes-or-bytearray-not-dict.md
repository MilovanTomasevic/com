---
layout: post
title: python JSON object must be str, bytes or bytearray, not 'dict
image: https://codebrainer.azureedge.net/images/what-is-json-in-javascript.png
description: >
  `json.loads` take a string as input and returns a dictionary as output, `json.dumps` take a dictionary as input and returns a string as output.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-14
categories: [stackoverflow]
tags: [python, json, dictionary]
---

python JSON object must be str, bytes or bytearray, not 'dict

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/136097/difference-between-staticmethod-and-classmethod) by [barak manos](https://stackoverflow.com/users/1382251/barak-manos)

In Python 3+, to load json previously saved like this:

```py
json.dumps(dictionary)
```

the output is something like
```py
{"('Hello',)": 6, "('Hi',)": 5}
```
when I use
```py
json.loads({"('Hello',)": 6, "('Hi',)": 5})
```
it doesn't works, this happens:

> TypeError: the JSON object must be str, bytes or bytearray, not 'dict'

## [Answer](https://stackoverflow.com/a/42354033/13155046) by [barak manos](https://stackoverflow.com/users/1382251/barak-manos)

`json.loads` take a string as input and returns a dictionary as output.

`json.dumps` take a dictionary as input and returns a string as output.

---
With `json.loads({"('Hello',)": 6, "('Hi',)": 5})`,

You are calling `json.loads` with a dictionary as input.

You can fix it as follows (though I'm not quite sure what's the point of that):
```py
d1 = {"('Hello',)": 6, "('Hi',)": 5}
s1 = json.dumps(d1)
d2 = json.loads(s1)
```

## [Answer](https://stackoverflow.com/a/66070438/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)


### [`json.dumps()`][1] is used to decode JSON data

```py
import json

# initialize different data
str_data = 'normal string'
int_data = 1
float_data = 1.50
list_data = [str_data, int_data, float_data]
nested_list = [int_data, float_data, list_data]
dictionary = {
    'int': int_data,
    'str': str_data,
    'float': float_data,
    'list': list_data,
    'nested list': nested_list
}

# convert them to JSON data and then print it
print('String :', json.dumps(str_data))
print('Integer :', json.dumps(int_data))
print('Float :', json.dumps(float_data))
print('List :', json.dumps(list_data))
print('Nested List :', json.dumps(nested_list, indent=4))
print('Dictionary :', json.dumps(dictionary, indent=4))  # the json data will be indented
```

output:

```sh
String : "normal string"
Integer : 1
Float : 1.5
List : ["normal string", 1, 1.5]
Nested List : [
    1,
    1.5,
    [
        "normal string",
        1,
        1.5
    ]
]
Dictionary : {
    "int": 1,
    "str": "normal string",
    "float": 1.5,
    "list": [
        "normal string",
        1,
        1.5
    ],
    "nested list": [
        1,
        1.5,
        [
            "normal string",
            1,
            1.5
        ]
    ]
}
```

-  Python Object to JSON Data Conversion


|                 Python                 |  JSON  |
|:--------------------------------------:|:------:|
|                  dict                  | object |
|               list, tuple              |  array |
|                   str                  | string |
| int, float, int- & float-derived Enums | number |
|                  True                  |  true  |
|                  False                 |  false |
|                  None                  |  null  |


### [`json.loads()`][1] is used to convert JSON data into Python data. 

```py
import json

# initialize different JSON data
arrayJson = '[1, 1.5, ["normal string", 1, 1.5]]'
objectJson = '{"a":1, "b":1.5 , "c":["normal string", 1, 1.5]}'

# convert them to Python Data
list_data = json.loads(arrayJson)
dictionary = json.loads(objectJson)

print('arrayJson to list_data :\n', list_data)
print('\nAccessing the list data :')
print('list_data[2:] =', list_data[2:])
print('list_data[:1] =', list_data[:1])

print('\nobjectJson to dictionary :\n', dictionary)
print('\nAccessing the dictionary :')
print('dictionary[\'a\'] =', dictionary['a'])
print('dictionary[\'c\'] =', dictionary['c'])
```

output:

```sh
arrayJson to list_data :
 [1, 1.5, ['normal string', 1, 1.5]]

Accessing the list data :
list_data[2:] = [['normal string', 1, 1.5]]
list_data[:1] = [1]

objectJson to dictionary :
 {'a': 1, 'b': 1.5, 'c': ['normal string', 1, 1.5]}

Accessing the dictionary :
dictionary['a'] = 1
dictionary['c'] = ['normal string', 1, 1.5]
```

- JSON Data to Python Object Conversion


|      JSON     | Python |
|:-------------:|:------:|
|     object    |  dict  |
|     array     |  list  |
|     string    |   str  |
|  number (int) |   int  |
| number (real) |  float |
|      true     |  True  |
|     false     |  False |



  [1]: https://docs.python.org/3/library/json.html#module-json
