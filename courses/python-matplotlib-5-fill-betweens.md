---
layout: page
title: Python Matplotlib - 5. Fill Betweens
description: >
  Python Matplotlib - 5. Fill Betweens...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Run Code 1 in Python

{% highlight python linenos %}
import pandas as pd
from matplotlib import pyplot as plt

data = pd.read_csv('data.csv')
ages = data['Age']
dev_salaries = data['All_Devs']
py_salaries = data['Python']
js_salaries = data['JavaScript']

plt.plot(ages, dev_salaries, color='#444444',
         linestyle='--', label='All Devs')

plt.plot(ages, py_salaries, label='Python')

# overall_median = 57287

plt.legend()

plt.title('Median Salary (USD) by Age')
plt.xlabel('Ages')
plt.ylabel('Median Salary (USD)')

plt.tight_layout()

plt.show()
{% endhighlight %}
starting_code.py
{:.figure}


##  Output

![](/courses/python-matplotlib/python-matplotlib-5-fill-betweens-pic1.png)
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
import pandas as pd
from matplotlib import pyplot as plt

data = pd.read_csv('data.csv')
ages = data['Age']
dev_salaries = data['All_Devs']
py_salaries = data['Python']
js_salaries = data['JavaScript']

plt.plot(ages, dev_salaries, color='#444444',
         linestyle='--', label='All Devs')

plt.plot(ages, py_salaries, label='Python')

overall_median = 57287

plt.fill_between(ages, py_salaries, dev_salaries,
                 where=(py_salaries > dev_salaries),
                 interpolate=True, alpha=0.25, label='Above Avg')

plt.fill_between(ages, py_salaries, dev_salaries,
                 where=(py_salaries <= dev_salaries),
                 interpolate=True, color='red', alpha=0.25, label='Below Avg')

plt.legend()

plt.title('Median Salary (USD) by Age')
plt.xlabel('Ages')
plt.ylabel('Median Salary (USD)')

plt.tight_layout()

plt.show()
{% endhighlight %}
finished_code.py
{:.figure}


##  Output

![](/courses/python-matplotlib/python-matplotlib-5-fill-betweens-pic2.png)
{:.figure}


##  CSV data

{% highlight csv linenos %}
Age,All_Devs,Python,JavaScript
18,17784,20046,16446
19,16500,17100,16791
20,18012,20000,18942
21,20628,24744,21780
22,25206,30500,25704
23,30252,37732,29000
24,34368,41247,34372
25,38496,45372,37810
26,42000,48876,43515
27,46752,53850,46823
28,49320,57287,49293
29,53200,45000,53437
30,56000,50000,56373
31,62316,55000,62375
32,64928,70000,66674
33,67317,71496,68745
34,68748,75370,68746
35,73752,83640,74583
36,77232,84666,79000
37,78000,84392,78508
38,78508,78254,79996
39,79536,85000,80403
40,82488,87038,83820
41,88935,91991,88833
42,90000,100000,91660
43,90056,94796,87892
44,95000,97962,96243
45,90000,93302,90000
46,91633,99240,99313
47,91660,102736,91660
48,98150,112285,102264
49,98964,100771,100000
50,100000,104708,100000
51,98988,108423,91660
52,100000,101407,99240
53,108923,112542,108000
54,105000,122870,105000
55,103117,120000,104000
{% endhighlight %}
data.csv
{:.figure}