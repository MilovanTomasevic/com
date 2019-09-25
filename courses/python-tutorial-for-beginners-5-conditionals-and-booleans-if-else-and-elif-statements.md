---
layout: page
title: Python Tutorial for Beginners - 5. Conditionals and Booleans - If, Else, and Elif Statements
description: >
  Python Tutorial for Beginners - 5. Conditionals and Booleans - If, Else, and Elif Statements...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Run Code 1 in Python

{% highlight python linenos %}
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
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
language = 'Python'

if language == 'Python':
    print('Language is Python')
else:
    print('No match')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
language = 'Java'

if language == 'Python':
    print('Language is Python')
elif language == 'Java':
    print('Language is java')
elif language == 'JavaScritp':
    print('Language is JavaSrtipt')
else:
    print('No match')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
user = 'Admin'
logged_in = True

if user == 'Admin' and logged_in:
    print('Admin page')
else:
    print('Bad Creds')
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
user = 'Admin'
logged_in = False

if user == 'Admin' or logged_in:
    print('Admin page')
else:
    print('Bad Creds')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
user = 'Admin'
logged_in = False

if not logged_in:
    print('Plese Log In')
else:
    print('Welcome')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b)
print(a is b)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(a is b)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(a == b)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
print(id(a) == id(b))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 11 in Python

{% highlight python linenos %}
a = [1, 2, 3]
b = a

print(id(a))
print(id(b))
print(id(a) == id(b))
{% endhighlight %}
start.py
{:.figure}


##  Run Code 12 in Python

{% highlight python linenos %}
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
{% endhighlight %}
start.py
{:.figure}

##  Run Code 13 in Python

{% highlight python linenos %}
condition = None

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 14 in Python

{% highlight python linenos %}
condition = 0

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}


##  Run Code 15 in Python

{% highlight python linenos %}
condition = 10

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 16 in Python

{% highlight python linenos %}
condition = []

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 17 in Python

{% highlight python linenos %}
condition = {}

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}


##  Run Code 18 in Python

{% highlight python linenos %}
condition = 'Test'

if condition:
    print('Evaluated to True')
else:
    print('Evaluated to False')
{% endhighlight %}
start.py
{:.figure}
