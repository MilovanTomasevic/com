---
layout: page
title: Python-Design-Patterns observer
description: >
  Python Design Patterns Tutorial for beginners - Learn Python Design Patterns in simple and easy steps starting from basic to advanced concepts with examples ...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

## observer Model

![](/courses/python-fesign-patterns/behavioral/viz/observer.py.png)

## Python-Design-Patterns observer

```py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
http://code.activestate.com/recipes/131499-observer-pattern/

*TL;DR80
Maintains a list of dependents and notifies them of any state changes.
"""

from __future__ import print_function

class Subject(object):
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def detach(self, observer):
        try:
            self._observers.remove(observer)
        except ValueError:
            pass

    def notify(self, modifier=None):
        for observer in self._observers:
            if modifier != observer:
                observer.update(self)

# Example usage
class Data(Subject):
    def __init__(self, name=''):
        Subject.__init__(self)
        self.name = name
        self._data = 0

    @property
    def data(self):
        return self._data

    @data.setter
    def data(self, value):
        self._data = value
        self.notify()

class HexViewer:
    def update(self, subject):
        print(u'HexViewer: Subject %s has data 0x%x' % (subject.name, subject.data))

class DecimalViewer:
    def update(self, subject):
        print(u'DecimalViewer: Subject %s has data %d' % (subject.name, subject.data))

# Example usage...
def main():
    data1 = Data('Data 1')
    data2 = Data('Data 2')
    view1 = DecimalViewer()
    view2 = HexViewer()
    data1.attach(view1)
    data1.attach(view2)
    data2.attach(view2)
    data2.attach(view1)

    print(u"Setting Data 1 = 10")
    data1.data = 10
    print(u"Setting Data 2 = 15")
    data2.data = 15
    print(u"Setting Data 1 = 3")
    data1.data = 3
    print(u"Setting Data 2 = 5")
    data2.data = 5
    print(u"Detach HexViewer from data1 and data2.")
    data1.detach(view2)
    data2.detach(view2)
    print(u"Setting Data 1 = 10")
    data1.data = 10
    print(u"Setting Data 2 = 15")
    data2.data = 15

if __name__ == '__main__':
    main()

### OUTPUT ###
# Setting Data 1 = 10
# DecimalViewer: Subject Data 1 has data 10
# HexViewer: Subject Data 1 has data 0xa
# Setting Data 2 = 15
# HexViewer: Subject Data 2 has data 0xf
# DecimalViewer: Subject Data 2 has data 15
# Setting Data 1 = 3
# DecimalViewer: Subject Data 1 has data 3
# HexViewer: Subject Data 1 has data 0x3
# Setting Data 2 = 5
# HexViewer: Subject Data 2 has data 0x5
# DecimalViewer: Subject Data 2 has data 5
# Detach HexViewer from data1 and data2.
# Setting Data 1 = 10
# DecimalViewer: Subject Data 1 has data 10
# Setting Data 2 = 15
# DecimalViewer: Subject Data 2 has data 15
```
observer.py
{:.figure}

## observer Test

```py
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import unittest
from behavioral.observer import Subject, Data, DecimalViewer, HexViewer

try:
    from unittest.mock import patch
except ImportError:
    from mock import patch

class TestSubject(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls.s = Subject()
        cls.dec_obs = DecimalViewer()
        cls.hex_obs = HexViewer()

    def test_a_observer_list_shall_be_empty_initially(cls):
        cls.assertEqual(len(cls.s._observers), 0)

    def test_b_observers_shall_be_attachable(cls):
        cls.s.attach(cls.dec_obs)
        cls.assertEqual(isinstance(cls.s._observers[0], DecimalViewer), True)
        cls.assertEqual(len(cls.s._observers), 1)
        cls.s.attach(cls.hex_obs)
        cls.assertEqual(isinstance(cls.s._observers[1], HexViewer), True)
        cls.assertEqual(len(cls.s._observers), 2)

    def test_c_observers_shall_be_detachable(cls):
        cls.s.detach(cls.dec_obs)
        # hex viewer shall be remaining if dec viewer is detached first
        cls.assertEqual(isinstance(cls.s._observers[0], HexViewer), True)
        cls.assertEqual(len(cls.s._observers), 1)
        cls.s.detach(cls.hex_obs)
        cls.assertEqual(len(cls.s._observers), 0)

class TestData(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls.dec_obs = DecimalViewer()
        cls.hex_obs = HexViewer()
        cls.sub = Data('Data')
        # inherited behavior already tested with TestSubject
        cls.sub.attach(cls.dec_obs)
        cls.sub.attach(cls.hex_obs)

    def test_data_change_shall_notify_all_observers_once(cls):
        with patch.object(cls.dec_obs, 'update') as mock_dec_obs_update, patch.object(
            cls.hex_obs, 'update'
        ) as mock_hex_obs_update:
            cls.sub.data = 10
            cls.assertEqual(mock_dec_obs_update.call_count, 1)
            cls.assertEqual(mock_hex_obs_update.call_count, 1)

    def test_data_value_shall_be_changeable(cls):
        cls.sub.data = 20
        cls.assertEqual(cls.sub._data, 20)

    def test_data_name_shall_be_changeable(cls):
        cls.sub.name = 'New Data Name'
        cls.assertEqual(cls.sub.name, 'New Data Name')
```
test_observer.py
{:.figure}