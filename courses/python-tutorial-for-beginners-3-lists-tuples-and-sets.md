---
layout: page
title: Python Tutorial for Beginners - 3. Lists, Tuples, and Sets
description: >
  Python Tutorial for Beginners - 3. Lists, Tuples, and Sets...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---


##  Run Code 1 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(len(courses))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[0])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[3])
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[-1])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[4]) #list index out of range
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[0:2])
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[:2])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
print(courses[2:])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.append('Art')

print(courses) 
{% endhighlight %}
start.py
{:.figure}


##  Run Code 11 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.insert(0, 'Art')

print(courses)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 12 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.insert(0, courses_2)

print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 13 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.insert(0, courses_2)

print(courses[0])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 14 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.extend(courses_2)

print(courses)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 15 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']
courses_2 = ['Art', 'Education']
courses.append(courses_2)

print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 16 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.remove('Math')

print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 17 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.pop()

print(courses)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 18 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

popped = courses.pop()

print(popped) 
print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 19 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.reverse()
print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 20 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses.sort()
print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 21 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

courses.sort()
nums.sort()

print(courses) 
print(nums)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 22 in Python

{% highlight python linenos %}

{% endhighlight %}
start.py
{:.figure}

##  Run Code 23 in Python

{% highlight python linenos %}

{% endhighlight %}
start.py
{:.figure}

##  Run Code 24 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

courses.sort(reverse=True)
nums.sort(reverse=True)

print(courses) 
print(nums)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 25 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted(courses)

print(courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 26 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted_courses = sorted(courses)

print(sorted_courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 27 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

nums = [1, 5, 2, 4, 3]

sorted_courses = sorted(courses)

print(sorted_courses) 
print(min(nums))
print(max(nums))
print(sum(nums))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 28 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

print(courses.index('Math'))

print('Math' in courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 29 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

for item in courses:
    print(item)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 30 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

for index, courses in enumerate (courses, start =1):
    print(index, courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 31 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ', '.join(courses)
print(courses_str)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 32 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ' - '.join(courses)
print(courses_str)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 33 in Python

{% highlight python linenos %}
courses = ['History', 'Math', 'Physics', 'CompSci']

courses_str = ' - '.join(courses)

new_list = courses_str.split(' - ')

print(courses_str)
print(new_list)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 34 in Python

{% highlight python linenos %}
# Mutable
list_1 = ['History', 'Math', 'Physics', 'CompSci']
list_2 = list_1

print(list_1)
print(list_2)

list_1[0] = 'Art'

print(list_1)
print(list_2)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 35 in Python

{% highlight python linenos %}
# Immutable
tuple_1 = ('History', 'Math', 'Physics', 'CompSci')
tuple_2 = tuple_1

print(tuple_1)
print(tuple_2)

# tuple_1[0] = 'Art'

# print(tuple_1)
# print(tuple_2)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 36 in Python

{% highlight python linenos %}
# Immutable
tuple_1 = ('History', 'Math', 'Physics', 'CompSci')
tuple_2 = tuple_1

print(tuple_1)
print(tuple_2)

tuple_1[0] = 'Art'

print(tuple_1)
print(tuple_2)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 37 in Python

{% highlight python linenos %}
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}

print(cs_courses)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 38 in Python

{% highlight python linenos %}
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci', 'Math'}

print(cs_courses)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 39 in Python

{% highlight python linenos %}
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.intersection(art_courses))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 40 in Python

{% highlight python linenos %}
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.difference(art_courses))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 41 in Python

{% highlight python linenos %}
# Sets
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'CompSci'}

print(cs_courses.union(art_courses))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 42 in Python

{% highlight python linenos %}
# Empty Lists
empty_list = []
empty_list = list()

# Empty Tuples
empty_tuple = ()
empty_tuple = tuple()

# Empty Sets
empty_set = {} # This isn't right! It's a dict
empty_set = set()
{% endhighlight %}
start.py
{:.figure}
