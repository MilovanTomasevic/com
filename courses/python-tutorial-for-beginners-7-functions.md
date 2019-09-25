---
layout: page
title: Python Tutorial for Beginners - 7. Functions
description: >
  Python Tutorial for Beginners - 7. Functions...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Run Code 1 in Python

{% highlight python linenos %}
def hello_func():
    pass

print(hello_func())
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
def hello_func():
    print('Hello Function')

print(hello_func())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
def hello_func():
    print('Hello Function')

hello_func()
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
def hello_func():
    print('Hello Function')

hello_func()
hello_func()
hello_func()
hello_func()
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
def hello_func():
    return 'Hello Function'

print(hello_func())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
def hello_func():
    return 'Hello Function'

print(hello_func().upper())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
def hello_func(greeting):
    return '{} Function. '.format(greeting)

print(hello_func('Hi'))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
def hello_func(greeting, name = 'You'):
    return '{}, {}'.format(greeting, name)

print(hello_func('Hi', name = 'MT'))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
def student_info(*args, **kwargs):
    print(args)
    print(kwargs)

student_info('Math', 'Art', name= 'John', age=22)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
def student_info(*args, **kwargs):
    print(args)
    print(kwargs)

courses = ['Math', 'Art']
info = {'name': 'John', 'age': 22}

student_info(*courses, **info)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 11 in Python

{% highlight python linenos %}
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
{% endhighlight %}
start.py
{:.figure}