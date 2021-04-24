---
layout: post
title: How to remove the left part of a string?
image: https://miro.medium.com/max/3724/1*saSNHPFIa1YtVGA1yCERvQ.png
description: >
  I have some simple python code that searches files for a string e.g. `path=c:\path`, where the `c:\path` part may vary.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-25
categories: [stackoverflow]
tags: [python, string]
---

How to remove the left part of a string?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/599953/how-to-remove-the-left-part-of-a-string) by [grigoryvp](https://stackoverflow.com/users/69882/grigoryvp)

I have some simple python code that searches files for a string e.g. `path=c:\path`, where the `c:\path` part may vary. The current code is:

```py
def find_path(i_file):
    lines = open(i_file).readlines()
    for line in lines:
        if line.startswith("Path="):
            return # what to do here in order to get line content after "Path=" ?
```

What is a simple way to get the text after `Path=`?


## [Answer](https://stackoverflow.com/a/64400605/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

`removeprefix()` and `removesuffix()`  string methods added in **Python 3.9** due to issues associated with `lstrip` and `rstrip` interpretation of parameters passed to them. Read [PEP 616][1] for more details.

```py
# in python 3.9
>>> s = 'python_390a6'

# apply removeprefix()
>>> s.removeprefix('python_')
'390a6'

# apply removesuffix()
>>> s = 'python.exe'
>>> s.removesuffix('.exe')
'python'

# in python 3.8 or before
>>> s = 'python_390a6'
>>> s.lstrip('python_')
'390a6'

>>> s = 'python.exe'
>>> s.rstrip('.exe')
'python'
```

`removesuffix` example with a list:
```py
plurals = ['cars', 'phones', 'stars', 'books']
suffix = 's'

for plural in plurals:
    print(plural.removesuffix(suffix))
```

output:
```sh
car
phone
star
book
```

`removeprefix` example with a list:

```py
places = ['New York', 'New Zealand', 'New Delhi', 'New Now']

shortened = [place.removeprefix('New ') for place in places]
print(shortened)
```

output:
```sh
['York', 'Zealand', 'Delhi', 'Now']
```


  [1]: https://www.python.org/dev/peps/pep-0616/