---
layout: page
title: Python Tutorial for Beginners - 1. Strings - Working with Textual Data
description: >
  Python Tutorial for Beginners - 1. Strings - Working with Textual Data...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---


##  Run Code 1 in Python

{% highlight python linenos %}
print('Hello World')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 2 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
message = 'Milovan's World'
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
message = 'Milovan\'s World'
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 5 in Python

{% highlight python linenos %}
message = "Milovan\'s World"
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
message = """Hello World
test test test"""
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
message = 'Hello World'
print(len(message))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 8 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message[0])
print(message[10])
print(message[11]) #error
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message[0:5])
print(message[:5])
print(message[6:])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message.lower())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 11 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message.upper())
print(message.count('Hello'))
print(message.count('l'))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 12 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message.find('World'))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 13 in Python

{% highlight python linenos %}
message = 'Hello World'
print(message.find('Universe'))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 14 in Python

{% highlight python linenos %}
message = 'Hello World'
message.replace('World', 'Universe')
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 15 in Python

{% highlight python linenos %}
message = 'Hello World'
new_message = message.replace('World', 'Universe')
print(new_message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 16 in Python

{% highlight python linenos %}
message = 'Hello World'
message = message.replace('World', 'Universe')
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 17 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'
message = greeting + name
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 18 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'
message = greeting + ', '+ name
print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 19 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

message = greeting + ', '+ name + '. Welcome!'

print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 20 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

message = '{}, {}. Welcome!'.format(greeting, name)

print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 21 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

message = f'{greeting}, {name}. Welcome!'

print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 22 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

message = f'{greeting}, {name.upper()}. Welcome!'

print(message)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 23 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

print(dir(name))
{% endhighlight %}
start.py
{:.figure}

##  Run Code 24 in Python

{% highlight python linenos %}
greeting = 'Hello'
name = 'Milovan'

print(help(str))
{% endhighlight %}
start.py
{:.figure}

