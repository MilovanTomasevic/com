---
layout: page
title: Python Matplotlib - 8. Time Series
description: >
  Python Matplotlib - 8. Time Series...
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
from datetime import datetime, timedelta
from matplotlib import pyplot as plt
from matplotlib import dates as mpl_dates

plt.style.use('seaborn')

dates = [
    datetime(2019, 5, 24),
    datetime(2019, 5, 25),
    datetime(2019, 5, 26),
    datetime(2019, 5, 27),
    datetime(2019, 5, 28),
    datetime(2019, 5, 29),
    datetime(2019, 5, 30)
]

y = [0, 1, 3, 4, 6, 5, 7]

# data = pd.read_csv('data.csv')
# price_date = data['Date']
# price_close = data['Close']

# plt.title('Bitcoin Prices')
# plt.xlabel('Date')
# plt.ylabel('Closing Price')

plt.tight_layout()

plt.show()
{% endhighlight %}
starting_code.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
import pandas as pd
from datetime import datetime, timedelta
from matplotlib import pyplot as plt
from matplotlib import dates as mpl_dates

plt.style.use('seaborn')

data = pd.read_csv('data.csv')

data['Date'] = pd.to_datetime(data['Date'])
data.sort_values('Date', inplace=True)

price_date = data['Date']
price_close = data['Close']

plt.plot_date(price_date, price_close, linestyle='solid')

plt.gcf().autofmt_xdate()

plt.title('Bitcoin Prices')
plt.xlabel('Date')
plt.ylabel('Closing Price')

plt.tight_layout()

plt.show()
{% endhighlight %}
finished_code.py
{:.figure}


##  Output

![](/courses/python-matplotlib/python-matplotlib-8-time-series-pic1.png)
{:.figure}


##  CSV data

{% highlight csv linenos %}
Date,Open,High,Low,Close,Adj Close,Volume
2019-05-18,7266.080078,8281.660156,7257.259766,8193.139648,8193.139648,723011166
2019-05-19,8193.139648,8193.139648,7591.850098,7998.290039,7998.290039,637617163
2019-05-20,7998.290039,8102.319824,7807.770020,7947.930176,7947.930176,357803946
2019-05-21,7947.930176,8033.759766,7533.660156,7626.890137,7626.890137,424501866
2019-05-22,7626.890137,7971.259766,7478.740234,7876.500000,7876.500000,386766321
2019-05-23,7876.500000,8165.450195,7801.569824,7996.399902,7996.399902,413162746
2019-05-24,7996.399902,8140.819824,7948.680176,8059.129883,8059.129883,179206342
2019-05-25,8059.129883,8779.000000,7894.529785,8726.230469,8726.230469,483663699
2019-05-26,8726.230469,8931.530273,8668.459961,8785.169922,8785.169922,507164714
2019-05-27,8785.169922,8818.709961,8562.200195,8718.849609,8718.849609,360752199
2019-05-28,8718.849609,8760.480469,8444.099609,8664.559570,8664.559570,380343928
2019-05-29,8664.559570,9065.889648,8027.209961,8276.250000,8276.250000,815525590
2019-05-30,8276.250000,8570.780273,8116.000000,8560.080078,8560.080078,500141087
2019-05-31,8550.629883,8576.339844,8459.650391,8504.980469,8504.980469,69915456
{% endhighlight %}
data.csv
{:.figure}