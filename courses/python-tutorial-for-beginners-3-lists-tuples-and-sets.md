---
layout: page
title: Python Tutorial for Beginners - 3. Lists, Tuples, and Sets
description: >
  Python Tutorial for Beginners - 3. Lists, Tuples, and Sets...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses)
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(len(courses))
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[0])
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[3])
~~~


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[-1])
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[4]) #list index out of range
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[0:2])
~~~


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[:2])
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[2:])
~~~

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.append('Art')

print(courses) 
~~~


##  Run Code 11 in Python

~~~py
# file: 'app11.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.insert(0, 'Art')

print(courses)
~~~


##  Run Code 12 in Python

~~~py
# file: 'app12.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.insert(0, courses_2)

print(courses)
~~~

##  Run Code 13 in Python

~~~py
# file: 'app13.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.insert(0, courses_2)

print(courses[0])
~~~

##  Run Code 14 in Python

~~~py
# file: 'app14.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.extend(courses_2)

print(courses)
~~~


##  Run Code 15 in Python

~~~py
# file: 'app15.py'
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.append(courses_2)

print(courses)
~~~

##  Run Code 16 in Python

~~~py
# file: 'app16.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.remove('Math')

print(courses)
~~~

##  Run Code 17 in Python

~~~py
# file: 'app17.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.pop()

print(courses)
~~~


##  Run Code 18 in Python

~~~py
# file: 'app18.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

popped = courses.pop()

print(popped) 
print(courses)
~~~

##  Run Code 19 in Python

~~~py
# file: 'app19.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.reverse()
print(courses)
~~~

##  Run Code 20 in Python

~~~py
# file: 'app20.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.sort()
print(courses)
~~~

##  Run Code 21 in Python

~~~py
# file: 'app21.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

courses.sort()
nums.sort()

print(courses) 
print(nums)
~~~


##  Run Code 22 in Python

~~~py
# file: 'app22.py'

~~~

##  Run Code 23 in Python

~~~py
# file: 'app23.py'
:)
~~~

##  Run Code 24 in Python

~~~py
# file: 'app24.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

courses.sort(reverse=True)
nums.sort(reverse=True)

print(courses) 
print(nums)
~~~


##  Run Code 25 in Python

~~~py
# file: 'app25.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted(courses)

print(courses)
~~~

##  Run Code 26 in Python

~~~py
# file: 'app26.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted_courses = sorted(courses)

print(sorted_courses)
~~~

##  Run Code 27 in Python

~~~py
# file: 'app27.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted_courses = sorted(courses)

print(sorted_courses) 
print(min(nums))
print(max(nums))
print(sum(nums))
~~~


##  Run Code 28 in Python

~~~py
# file: 'app28.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

print(courses.index('Math'))

print('Math' in courses)
~~~

##  Run Code 29 in Python

~~~py
# file: 'app29.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

for item in courses:
    print(item)
~~~

##  Run Code 30 in Python

~~~py
# file: 'app30.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

for index, courses in enumerate (courses, start =1):
    print(index, courses)
~~~

##  Run Code 31 in Python

~~~py
# file: 'app31.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ', '.join(courses)
print(courses_str)
~~~


##  Run Code 32 in Python

~~~py
# file: 'app32.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ' - '.join(courses)
print(courses_str)
~~~

##  Run Code 33 in Python

~~~py
# file: 'app33.py'
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ' - '.join(courses)

new_list = courses_str.split(' - ')

print(courses_str)
print(new_list)
~~~

##  Run Code 34 in Python

~~~py
# file: 'app34.py'
# Mutable
list_1 = ['History', 'Math', 'Physics', 'CompSci']
list_2 = list_1

print(list_1)
print(list_2)

list_1[0] = 'Art'

print(list_1)
print(list_2)
~~~


##  Run Code 35 in Python

~~~py
# file: 'app35.py'
# Immutable
tuple_1 = ('History', 'Math', 'Physics', 'CompSci')
tuple_2 = tuple_1

print(tuple_1)
print(tuple_2)

# tuple_1[0] = 'Art'

# print(tuple_1)
# print(tuple_2)
~~~

##  Run Code 36 in Python

~~~py
# file: 'app36.py'
# Immutable
tuple_1 = ('History', 'Math', 'Physics', 'CompSci')
tuple_2 = tuple_1

print(tuple_1)
print(tuple_2)

tuple_1[0] = 'Art'

print(tuple_1)
print(tuple_2)
~~~

##  Run Code 37 in Python

~~~py
# file: 'app37.py'
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}

print(cs_courses)
~~~


##  Run Code 38 in Python

~~~py
# file: 'app38.py'
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci', 'Math'}

print(cs_courses)
~~~

##  Run Code 39 in Python

~~~py
# file: 'app39.py'
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.intersection(art_courses))
~~~

##  Run Code 40 in Python

~~~py
# file: 'app40.py'
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.difference(art_courses))
~~~

##  Run Code 41 in Python

~~~py
# file: 'app41.py'
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.union(art_courses))
~~~


##  Run Code 42 in Python

~~~py
# file: 'app42.py'
# Empty Lists
empty_list = []
empty_list = list()

# Empty Tuples
empty_tuple = ()
empty_tuple = tuple()

# Empty Sets
empty_set = {} # This isn't right! It's a dict
empty_set = set()
~~~
