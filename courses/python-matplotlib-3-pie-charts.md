---
layout: page
title: Python Matplotlib - 3. Pie Charts
description: >
  Python Matplotlib - 3. Pie Charts...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'starting_code.py'
from matplotlib import pyplot as plt

plt.style.use("fivethirtyeight")


plt.title("My Awesome Pie Chart")
plt.tight_layout()
plt.show()

# Colors:
# Blue = #008fd5
# Red = #fc4f30
# Yellow = #e5ae37
# Green = #6d904f
~~~
starting_code.py
{:.figure}


##  Output

![](/courses/python-matplotlib/python-matplotlib-3-pie-charts-pic1.png)
{:.figure}


##  Run Code 2 in Python

~~~py
# file: 'finished_code.py'
from matplotlib import pyplot as plt

plt.style.use("fivethirtyeight")

slices = [59219, 55466, 47544, 36443, 35917]
labels = ['JavaScript', 'HTML/CSS', 'SQL', 'Python', 'Java']
explode = [0, 0, 0, 0.1, 0]

plt.pie(slices, labels=labels, explode=explode, shadow=True,
        startangle=90, autopct='%1.1f%%',
        wedgeprops={'edgecolor': 'black'})

plt.title("My Awesome Pie Chart")
plt.tight_layout()
plt.show()
~~~
finished_code.py
{:.figure}


##  Output

![](/courses/python-matplotlib/python-matplotlib-3-pie-charts-pic2.png)
{:.figure}


