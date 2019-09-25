---
layout: page
title: Python Tutorial for Beginners - 12. Using Try/Except Blocks for Error Handling
description: >
  Python Tutorial for Beginners - 1. Using Try/Except Blocks for Error Handling...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---


##  Test file

{% highlight text linenos %}
Test File Contents!
{% endhighlight %}
test_file.txt
{:.figure}

##  Currupt file

{% highlight text linenos %}
Currupt File!
{% endhighlight %}
currupt_file.txt
{:.figure}


##  Run Code 1 in Python

{% highlight python linenos %}
try:
     pass
except Exception:
    pass
else:
   pass
finally:
   pass
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
f = open('testfile.txt')

try:
     pass
except Exception:
    pass
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
try:
    f = open('testfile.txt')
except Exception:
    print('Sorry. This file doenst exist')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
try:
    f = open('test_file.txt')
    var = bad_var
except FileNotFoundError:
    print('Sorry. This file doenst exist')
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
try:
    f = open('test_file.txt')
    var = bad_var
except FileNotFoundError:
    print('Sorry. This file doenst exist')
except Exception:
    print('Sorry. Samting went wrong')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
try:
    f = open('testfile.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
try:
    f = open('test_file.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
else:
   print(f.read())
   f.close()
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
try:
    f = open('test_file.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
else:
   print(f.read())
   f.close()
finally:
   print('Excecuting Finally...')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
try:
    f = open('testfile.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
else:
   print(f.read())
   f.close()
finally:
   print('Excecuting Finally...')
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
try:
    f = open('curruptfile.txt')
except IOError as e:
    print('First!')
except Exception as e:
    print('Second')
else:
    print(f.read())
    f.close()
finally:
    print("Executing Finally...")

print('End of program')
{% endhighlight %}
start.py
{:.figure}


##  Run Code 11 in Python

{% highlight python linenos %}
try:
    f = open('curruptfile.txt')
    # if f.name == 'currupt_file.txt':
    #     raise Exception
except IOError as e:
    print('First!')
except Exception as e:
    print('Second')
else:
    print(f.read())
    f.close()
finally:
    print("Executing Finally...")

print('End of program')
{% endhighlight %}
start.py
{:.figure}