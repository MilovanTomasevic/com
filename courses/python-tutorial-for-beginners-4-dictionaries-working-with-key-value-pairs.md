---
layout: page
title: Python Tutorial for Beginners - 4. Dictionaries - Working with Key-Value Pairs
description: >
  Python Tutorial for Beginners - 4. Dictionaries - Working with Key-Value Pairs...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

##  Run Code 1 in Python

~~~py
# file: 'app1.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

for key, value in student.items():
    print(key, value) 
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student)
print(student['courses'])
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student['phone']) #KeyError: 'phone'
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student.get('name'))
print(student.get('phone'))

print(student.get('phone', 'Not found'))
~~~


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

student['phone'] = '555-5555'
student['name'] = 'MT'

print(student)
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

student.update({'name': 'MT', 'age': 31, 'phone':'0401234567'})

print(student)
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

age = student.pop('age')

print(student)
print(age)
~~~


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}


print(len(student))
print(student.values())
print(student.items())
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

for key, value in student.items():
    print(key, value) 
~~~