---
layout: page
title: Python Tutorial for Beginners - 6. Loops and Iterations - For-While Loops
description: >
  Python Tutorial for Beginners - 6. Loops and Iterations - For-While Loops...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Run Code 1 in Python

{% highlight python linenos %}
nums = [1, 2, 3, 4, 5]

for num in nums
    print(num)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
nums = [1, 2, 3, 4, 5]

for num in nums
    if num == 3
      print('Found!')
      break
    print(num)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 3 in Python

{% highlight python linenos %}
nums = [1, 2, 3, 4, 5]

for num in nums
    if num == 3
      print('Found!')
      continue
    print(num)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
nums = [1, 2, 3, 4, 5]

for num in nums
    for letter in 'abc'
        print(num, letter)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 5 in Python

{% highlight python linenos %}
for i in range(1, 11)
    print(i)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
x = 0

while True:
    # if x == 5:
    #     break
    print(x)
    x += 1
{% endhighlight %}
start.py
{:.figure}