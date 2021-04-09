---
layout: page
title: Python Tutorial for Beginners - 5. Conditionals and Booleans - If, Else, and Elif Statements
description: >
  Python Tutorial for Beginners - 5. Conditionals and Booleans - If, Else, and Elif Statements...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
# Comparisons:
# Equal:            ==
# Not Equal:        !=
# Greater Than:     >
# Less Than:        <
# Greater or Equal: >=
# Less or Equal:    <=
# Object Identity:  is

language = 'Python'

if language == 'Python':
    print('Conditional was True')
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
language = 'Python'

if language == 'Python':
    print('Language is Python')
else:
    print('No match')
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
language = 'Java'

if language == 'Python':
    print('Language is Python')
elif language == 'Java':
    print('Language is java')
elif language == 'JavaScritp':
    print('Language is JavaSrtipt')
else:
    print('No match')
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
user = 'Admin'
logged_in = True

if user == 'Admin' and logged_in:
    print('Admin page')
else:
    print('Bad Creds')
~~~


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
user = 'Admin'
logged_in = False

if user == 'Admin' or logged_in:
    print('Admin page')
else:
    print('Bad Creds')
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
user = 'Admin'
logged_in = False

if not logged_in:
    print('Plese Log In')
else:
    print('Welcome')
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b)
print(a is b)
~~~


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(a is b)
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(a == b)
~~~

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(id(a) == id(b))
~~~


##  Run Code 11 in Python

~~~py
# file: 'app11.py'
a = [1, 2, 3]
b = a

print(id(a))
print(id(b))
print(id(a) == id(b))
~~~


##  Run Code 12 in Python

~~~py
# file: 'app12.py'
# False Values:
    # False
    # None
    # Zero of any numeric type
    # Any empty sequence. For example, '', (), [].
    # Any empty mapping. For example, {}.

condition = False

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~

##  Run Code 13 in Python

~~~py
# file: 'app13.py'
condition = None

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~

##  Run Code 14 in Python

~~~py
# file: 'app14.py'
condition = 0

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~


##  Run Code 15 in Python

~~~py
# file: 'app15.py'
condition = 10

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~

##  Run Code 16 in Python

~~~py
# file: 'app16.py'
condition = []

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~

##  Run Code 17 in Python

~~~py
# file: 'app17.py'
condition = {}

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~


##  Run Code 18 in Python

~~~py
# file: 'app18.py'
condition = 'Test'

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
~~~
