---
layout: page
title: Python-Design-Patterns decorator
description: >
  Python Design Patterns Tutorial for beginners - Learn Python Design Patterns in simple and easy steps starting from basic to advanced concepts with examples ...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## decorator Model

![](/courses/python-fesign-patterns/structural/viz/decorator.py.png)

## Python-Design-Patterns decorator

~~~py
# file: 'decorator.py'
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
*What is this pattern about?
The Decorator pattern is used to dynamically add a new feature to an
object without changing its implementation. It differs from
inheritance because the new feature is added only to that particular
object, not to the entire subclass.

*What does this example do?
This example shows a way to add formatting options (boldface and
italic) to a text by appending the corresponding tags (<b> and
<i>). Also, we can see that decorators can be applied one after the other,
since the original text is passed to the bold wrapper, which in turn
is passed to the italic wrapper.

*Where is the pattern used practically?
The Grok framework uses decorators to add functionalities to methods,
like permissions or subscription to an event:
http://grok.zope.org/doc/current/reference/decorators.html

*References:
https://sourcemaking.com/design_patterns/decorator

*TL;DR80
Adds behaviour to object without affecting its class.
"""

from __future__ import print_function

class TextTag(object):
    """Represents a base text tag"""

    def __init__(self, text):
        self._text = text

    def render(self):
        return self._text

class BoldWrapper(TextTag):
    """Wraps a tag in <b>"""

    def __init__(self, wrapped):
        self._wrapped = wrapped

    def render(self):
        return "<b>{}</b>".format(self._wrapped.render())

class ItalicWrapper(TextTag):
    """Wraps a tag in <i>"""

    def __init__(self, wrapped):
        self._wrapped = wrapped

    def render(self):
        return "<i>{}</i>".format(self._wrapped.render())

if __name__ == '__main__':
    simple_hello = TextTag("hello, world!")
    special_hello = ItalicWrapper(BoldWrapper(simple_hello))
    print("before:", simple_hello.render())
    print("after:", special_hello.render())

### OUTPUT ###
# before: hello, world!
# after: <i><b>hello, world!</b></i>
~~~
decorator.py
{:.figure}

## decorator Test

~~~py
# file: 'test_decorator.py'
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import unittest
from structural.decorator import TextTag, BoldWrapper, ItalicWrapper

class TestTextWrapping(unittest.TestCase):
    def setUp(self):
        self.raw_string = TextTag('raw but not cruel')

    def test_italic(self):
        self.assertEqual(ItalicWrapper(self.raw_string).render(), '<i>raw but not cruel</i>')

    def test_bold(self):
        self.assertEqual(BoldWrapper(self.raw_string).render(), '<b>raw but not cruel</b>')

    def test_mixed_bold_and_italic(self):
        self.assertEqual(BoldWrapper(ItalicWrapper(self.raw_string)).render(), '<b><i>raw but not cruel</i></b>')
~~~
test_decorator.py
{:.figure}
