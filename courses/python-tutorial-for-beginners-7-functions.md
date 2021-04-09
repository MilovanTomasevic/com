---
layout: page
title: Python Tutorial for Beginners - 7. Functions
description: >
  Python Tutorial for Beginners - 7. Functions...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
def hello_func():
    pass

print(hello_func())
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
def hello_func():
    print('Hello Function')

print(hello_func())
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
def hello_func():
    print('Hello Function')

hello_func()
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
def hello_func():
    print('Hello Function')

hello_func()
hello_func()
hello_func()
hello_func()
~~~


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
def hello_func():
    return 'Hello Function'

print(hello_func())
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
def hello_func():
    return 'Hello Function'

print(hello_func().upper())
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
def hello_func(greeting):
    return '{} Function. '.format(greeting)

print(hello_func('Hi'))
~~~


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
def hello_func(greeting, name = 'You'):
    return '{}, {}'.format(greeting, name)

print(hello_func('Hi', name = 'MT'))
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
def student_info(*args, **kwargs):
    print(args)
    print(kwargs)

student_info('Math', 'Art', name= 'John', age=22)
~~~

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
def student_info(*args, **kwargs):
    print(args)
    print(kwargs)

courses = ['Math', 'Art']
info = {'name': 'John', 'age': 22}

student_info(*courses, **info)
~~~


##  Run Code 11 in Python

~~~py
# file: 'app11.py'
# Number of days per month. First value placeholder for indexing purposes.
month_days = [0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]


def is_leap(year):
    """Return True for leap years, False for non-leap years."""

    return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)


def days_in_month(year, month):
    """Return number of days in that month in that year."""

    # year 2017
    # month 2
    if not 1 <= month <= 12:
        return 'Invalid Month'

    if month == 2 and is_leap(year):
        return 29

    return month_days[month]

print(is_leap(2017))
print(is_leap(2020))
print(days_in_month(2017, 2))
~~~