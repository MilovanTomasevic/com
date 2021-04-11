---
layout: post
title: How to use glob() to find files recursively?
image: https://i.pinimg.com/originals/99/65/d3/9965d3d198d5cb0ba5d6311d8a9a7ea3.png
description: >
  If you don't want to use pathlib, use can use `glob.glob('**/*.c')`
hide_description: true
hide_image: true
comments: true
more_posts: posts.md
featured: true
categories: [stackoverflow]
tags: [python, path, filesystems, glob, fnmatch]
---

How to use glob() to find files recursively?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/2186525/how-to-use-glob-to-find-files-recursively) by [Ben Gartner
](https://stackoverflow.com/users/169175/ben-gartner)

This is what I have:
~~~py
glob(os.path.join('src','*.c'))
~~~
but I want to search the subfolders of src. Something like this would work:
~~~py
glob(os.path.join('src','*.c'))
glob(os.path.join('src','*','*.c'))
glob(os.path.join('src','*','*','*.c'))
glob(os.path.join('src','*','*','*','*.c'))
~~~
But this is obviously limited and clunky.


## [Answer](https://stackoverflow.com/a/2186565/13155046) by [Johan Dahlin](https://stackoverflow.com/users/14337/johan-dahlin)

**pathlib.Path.rglob**

Use [`pathlib.Path.rglob`][1] from the the [`pathlib`][2] module, which was introduced in Python 3.5.
~~~py
from pathlib import Path

for path in Path('src').rglob('*.c'):
    print(path.name)
~~~
If you don't want to use pathlib, use can use [`glob.glob('**/*.c')`][3], but don't forget to pass in the `recursive` keyword parameter and it will use inordinate amount of time on large directories.

For cases where matching files beginning with a dot (`.`); like files in the current directory or hidden files on Unix based system, use the [`os.walk`][4] solution below.

**os.walk**

For older Python versions, use [`os.walk`][4] to recursively walk a directory and [`fnmatch.filter`][5] to match against a simple expression:
~~~py
import fnmatch
import os

matches = []
for root, dirnames, filenames in os.walk('src'):
    for filename in fnmatch.filter(filenames, '*.c'):
        matches.append(os.path.join(root, filename))
~~~
  [1]: https://docs.python.org/3/library/pathlib.html#pathlib.Path.rglob
  [2]: https://docs.python.org/3/library/pathlib.html
  [3]: https://docs.python.org/3/library/glob.html#glob.glob
  [4]: https://docs.python.org/2/library/os.html#os.walk
  [5]: https://docs.python.org/2/library/fnmatch.html#fnmatch.filter


## [Answer](https://stackoverflow.com/a/63224043/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

```py
import os, glob

for each in glob.glob('path/**/*.c', recursive=True):
    print(f'Name with path: {each} \nName without path: {os.path.basename(each)}')
```

```sh
- `glob.glob('*.c')                    `:matches all files ending in `.c` in current directory
- `glob.glob('*/*.c')                  `:same as 1
- `glob.glob('**/*.c')                 `:matches all files ending in `.c` in the immediate subdirectories only, but not in the current directory
- `glob.glob('*.c',recursive=True)     `:same as 1
- `glob.glob('*/*.c',recursive=True)   `:same as 3
- `glob.glob('**/*.c',recursive=True)  `:matches all files ending in `.c` in the current directory and in all subdirectories
```