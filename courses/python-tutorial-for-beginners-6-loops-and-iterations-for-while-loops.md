---
layout: page
title: Python Tutorial for Beginners - 6. Loops and Iterations - For-While Loops
description: >
  Python Tutorial for Beginners - 6. Loops and Iterations - For-While Loops...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
nums = [1, 2, 3, 4, 5]

for num in nums
    print(num)
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
nums = [1, 2, 3, 4, 5]

for num in nums
    if num == 3
      print('Found!')
      break
    print(num)
~~~


##  Run Code 3 in Python

~~~py
# file: 'app3.py'
nums = [1, 2, 3, 4, 5]

for num in nums
    if num == 3
      print('Found!')
      continue
    print(num)
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
nums = [1, 2, 3, 4, 5]

for num in nums
    for letter in 'abc'
        print(num, letter)
~~~

##  Run Code 5 in Python

~~~py
# file: 'app5.py'
for i in range(1, 11)
    print(i)
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
x = 0

while True:
    # if x == 5:
    #     break
    print(x)
    x += 1
~~~