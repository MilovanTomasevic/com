---
layout: page
title: Python Tutorial for Beginners - 4. Dictionaries - Working with Key-Value Pairs
description: >
  Python Tutorial for Beginners - 4. Dictionaries - Working with Key-Value Pairs...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Run Code 1 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

for key, value in student.items():
    print(key, value) 
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student)
print(student['courses'])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student['phone']) #KeyError: 'phone'
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

print(student.get('name'))
print(student.get('phone'))

print(student.get('phone', 'Not found'))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

student['phone'] = '555-5555'
student['name'] = 'MT'

print(student)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

student.update({'name': 'MT', 'age': 31, 'phone':'0401234567'})

print(student)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

age = student.pop('age')

print(student)
print(age)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}


print(len(student))
print(student.values())
print(student.items())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
student = {'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}

for key, value in student.items():
    print(key, value) 
{% endhighlight %}
start.py
{:.figure}