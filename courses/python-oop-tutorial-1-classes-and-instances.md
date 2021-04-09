---
layout: page
title: Python OOP Tutorial - 1. Classes and Instances
description: >
  Python OOP Tutorial - 1. Classes and Instances...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

##  Run Code 1 in Python

~~~py
# file: 'start1.py'
class Employee:

    def __init__(self, first, last, pay):
        self.first = first
        self.last = last
        self.email = first + '.' + last + '@email.com'
        self.pay = pay

    def fullname(self):
        return '{} {}'.format(self.first, self.last)

emp_1 = Employee('Corey', 'Schafer', 50000)
emp_2 = Employee('Test', 'Employee', 60000)
~~~
start1.py
{:.figure}