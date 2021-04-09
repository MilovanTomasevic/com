---
layout: page
title: Python Tutorial for Beginners - 8. Import Modules and Exploring The Standard Library
description: >
  Python Tutorial for Beginners - 8. Import Modules and Exploring The Standard Library...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

##  Run Code 1 in Python

~~~py
# file: 'app1.py'
print('Imported my_module...')

test = 'Test String'


def find_index(to_search, target):
    '''Find the index of a value in a sequence'''
    for i, value in enumerate(to_search):
        if value == target:
            return i

    return -1
~~~
my_module.py
{:.figure}


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
import my_module

courses = ['History', 'Math', 'Physics', 'CompSci']

index = my_module.find_index(courses, 'Math')
print(index)
~~~
start.py
{:.figure}

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
import my_module as mm

courses = ['History', 'Math', 'Physics', 'CompSci']

index = mm.find_index(courses, 'Math')
print(index)
~~~
start.py
{:.figure}

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
from my_module import find_index

courses = ['History', 'Math', 'Physics', 'CompSci']

index = find_index(courses, 'Math')
print(index)
~~~
start.py
{:.figure}


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
from my_module import find_index, test

courses = ['History', 'Math', 'Physics', 'CompSci']

index = find_index(courses, 'Math')
print(index)
print(test)
~~~
start.py
{:.figure}

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
from my_module import find_index as fi, test

courses = ['History', 'Math', 'Physics', 'CompSci']

index = fi(courses, 'Math')
print(index)
print(test)
~~~
start.py
{:.figure}

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
from my_module import *

courses = ['History', 'Math', 'Physics', 'CompSci']

index = find_index(courses, 'Math')
print(index)
print(test)
~~~
start.py
{:.figure}


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
from my_module import find_index, test
import sys

courses = ['History', 'Math', 'Physics', 'CompSci']

index = find_index(courses, 'Math')
# print(index)
# print(test)

print(sys.path)
~~~
start.py
{:.figure}

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
# move module file to desktop

import sys
sys.path.append('/Users/mt/Desktop/My-Modules') # move file to Desktop/My-Modules

from my_module import find_index, test


courses = ['History', 'Math', 'Physics', 'CompSci']

index = find_index(courses, 'Math')
# print(index)
# print(test)

print(sys.path)
~~~
start.py
{:.figure}



##  Linux/Mac

~~~sh
# file: 'terminal'
# goto terminal
nano ~/.bash_profile

# insert
export PYTHONPATH="/Users/mt/Desktop/My-Modules"

ctrl+x Y
# restart terminal
~~~
start.py
{:.figure}

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
import my_module
import sys
sys.path
~~~
start.py
{:.figure}


##  Windows

~~~sh
# file: 'terminal'

# start
# righ click on computer and click on propertis
# Advanced system seting
# Advandes
# Environment Variables
# Create - NEW 
#   Variable name: PYTHONPATH
#   Variable value: C:\Users\MT\Desktop\My-Modules
# OK OK

# Open Command promt

# python
import my_module
import sys
sys.path
~~~
start.py
{:.figure}

##  Run Code 11 in Python

~~~py
# file: 'app11.py'
import random

courses = ['History', 'Math', 'Physics', 'CompSci']

random_course = random.choice(courses)
~~~
start.py
{:.figure}


##  Run Code 12 in Python

~~~py
# file: 'app12.py'
import math

courses = ['History', 'Math', 'Physics', 'CompSci']

rads = math.radians(90)
print(math.sin(rads))
~~~
start.py
{:.figure}

##  Run Code 13 in Python

~~~py
# file: 'app13.py'
import datetime
import calendar

courses = ['History', 'Math', 'Physics', 'CompSci']

today = datetime.date.today()
print(today)
~~~
start.py
{:.figure}

##  Run Code 14 in Python

~~~py
# file: 'app14.py'
import os

courses = ['History', 'Math', 'Physics', 'CompSci']

print(os.getcwd())
print(os.__file__)
~~~
start.py
{:.figure}


##  Run Code 15 in Python

~~~py
# file: 'app15.py'
import antigravity
~~~
start.py
{:.figure}