```
title: Fun with Python
layout: post
tags: ['python', 'pandas', 'trading', 'post']
```

##Pulling yahoo finance data with python

Python is fun.  It's probably the language that is the most fun to play around with data in.  I've been messing around with Yahoo finance data recently and I've always wanted to make it big on the stock market so let's get started making millions!

You'll need Python, the [requests ](http://docs.python-requests.org/en/latest/) library and the [pandas](http://pandas.pydata.org/) library.

### Grab Tesla Motor's price history
Yahoo has a nice rest api for grabbing share market price history. For instance to download the price history for [Tesla Motors](http://au.finance.yahoo.com/q?s=TSLA) we just need to download the link: [http://ichart.finance.yahoo.com/table.csv?s=TSLA](http://ichart.finance.yahoo.com/table.csv?s=TSLA)

``` python
import requests
import StringIO

def downloadFile(url):
    req = requests.get(url)
    if req.status_code != 200:
        raise Exception(req.status_code, 'Error downloading file ' + url)
    else:
        return req.text

csv = downloadFile('http://ichart.finance.yahoo.com/table.csv?s=TSLA')
```

This should print the contents of the TSLA csv price history:
```
>>> print(csv)
Date,Open,High,Low,Close,Volume,Adj Close
2013-09-27,187.52,191.28,186.43,190.90,5916400,190.90
2013-09-26,186.70,189.68,185.61,188.64,6614400,188.64
2013-09-25,183.56,186.30,180.50,185.24,8239000,185.24
2013-09-24,179.14,184.96,177.65,182.33,6273400,182.33
2013-09-23,184.48,185.48,177.11,181.11,8173400,181.11
2013-09-20,178.90,185.83,178.56,183.39,13376700,183.39
2013-09-19,170.80,180.47,169.08,177.92,15578300,177.92
2013-09-18,167.07,167.45,164.20,166.22,5330600,166.22
2013-09-17,165.08,168.42,163.36,166.23,5496900,166.23
2013-09-16,168.00,170.85,165.85,166.58,7574900,166.58
... and so on

```

So that gives us our data.  Now we can use the excellent pandas library to load it and do some calculations.

But first we need to reverse the order of the lines so that when we're calculating our moving averages we don't miss the most recent days.
``` python
lines = csv.split('\n')
lines.reverse()
lines[0] = lines[-1] #move the header row back to the top
orderedcsv = "\n".join(lines[0:-1])

```

Now we can read the file properly:
``` python
from pandas import *

tsla = read_csv(StringIO.StringIO(orderedcsv))

```

tsla is now a pandas data frame.  It has columns matching the header line from our csv.
``` python
>>> tsla
<class 'pandas.core.frame.DataFrame'>
Int64Index: 819 entries, 0 to 818
Data columns (total 7 columns):
Date         819  non-null values
Open         819  non-null values
High         819  non-null values
Low          819  non-null values
Close        819  non-null values
Volume       819  non-null values
Adj Close    819  non-null values
dtypes: float64(5), int64(1), object(1)

```

Let's check out the first 10 rows of tsla
``` python
>>> DataFrame(tsla, index=range(0,10))
         Date   Open   High    Low  Close    Volume  Adj Close
0  2010-06-29  19.00  25.00  17.54  23.89  18766300      23.89
1  2010-06-30  25.79  30.42  23.30  23.83  17187100      23.83
2  2010-07-01  25.00  25.92  20.27  21.96   8218800      21.96
3  2010-07-02  23.00  23.10  18.71  19.20   5139800      19.20
4  2010-07-06  20.00  20.00  15.83  16.11   6866900      16.11
5  2010-07-07  16.40  16.63  14.98  15.80   6921700      15.80
6  2010-07-08  16.14  17.52  15.57  17.46   7711400      17.46
7  2010-07-09  17.58  17.90  16.55  17.40   4050600      17.40
8  2010-07-12  17.95  18.07  17.00  17.05   2202500      17.05
9  2010-07-13  17.39  18.64  16.90  18.14   2680100      18.14
```

Now we can use the built in pandas rolling_mean() function to calculate some moving averages:
``` python
tsla['CloseMA20'] = rolling_mean(tsla.Close, 20)
tsla['CloseMA52'] = rolling_mean(tsla.Close, 52)

#Note that I cut out a couple columns here for brevity
>>> DataFrame(tsla, index=range(0,10), columns=['Date', 'Open', 'Close', 'Volume', 'CloseMA20', 'CloseMA52'])
         Date   Open  Close    Volume  CloseMA20  CloseMA52
0  2010-06-29  19.00  23.89  18766300        NaN        NaN
1  2010-06-30  25.79  23.83  17187100        NaN        NaN
2  2010-07-01  25.00  21.96   8218800        NaN        NaN
3  2010-07-02  23.00  19.20   5139800        NaN        NaN
4  2010-07-06  20.00  16.11   6866900        NaN        NaN
5  2010-07-07  16.40  15.80   6921700        NaN        NaN
6  2010-07-08  16.14  17.46   7711400        NaN        NaN
7  2010-07-09  17.58  17.40   4050600        NaN        NaN
8  2010-07-12  17.95  17.05   2202500        NaN        NaN
9  2010-07-13  17.39  18.14   2680100        NaN        NaN
```

Wait a minute, why are our new columns not numbers? 

This is because we calculated rolling averages for these values but we didnt yet have enough previous days to calculate the average so they become NaN.  If we look a bit further in to the dataframe we'll see that we start getting values as we expect:
``` python
>>> DataFrame(tsla, index=range(18,28), columns=['Date', 'Open', 'Close', 'Volume', 'CloseMA20', 'CloseMA52'])
          Date   Open  Close   Volume  CloseMA20  CloseMA52
18  2010-07-26  21.50  20.95   922200        NaN        NaN
19  2010-07-27  20.91  20.55   619700    19.8715        NaN
20  2010-07-28  20.55  20.72   467200    19.7130        NaN
21  2010-07-29  20.77  20.35   616000    19.5390        NaN
22  2010-07-30  20.20  19.94   426900    19.4380        NaN
23  2010-08-02  20.50  20.92   718100    19.5240        NaN
24  2010-08-03  21.00  21.95  1230500    19.8160        NaN
25  2010-08-04  21.95  21.26   913000    20.0890        NaN
26  2010-08-05  21.54  20.45   796200    20.2385        NaN
27  2010-08-06  20.10  19.59   741900    20.3480        NaN

>>> DataFrame(tsla, index=range(48,58), columns=['Date', 'Open', 'Close', 'Volume', 'CloseMA20', 'CloseMA52'])
          Date   Open  Close   Volume  CloseMA20  CloseMA52
48  2010-09-07  20.61  20.54   243400    19.4285        NaN
49  2010-09-08  20.66  20.90   288400    19.5220        NaN
50  2010-09-09  21.00  20.71   376200    19.6625        NaN
51  2010-09-10  20.75  20.17   386600    19.7910  19.856923
52  2010-09-13  20.89  20.72   360800    19.9110  19.795962
53  2010-09-14  20.54  21.12   654700    20.0280  19.743846
54  2010-09-15  20.98  21.98   684600    20.1695  19.744231
55  2010-09-16  22.15  20.94  2684500    20.2780  19.777692
56  2010-09-17  21.02  20.23  1198500    20.3500  19.856923
57  2010-09-20  20.67  21.06   947500    20.4480  19.958077
```

We now have a column of 20 day moving averages and a column of 52 day moving averages of the Close price.

Let's make ourselves a simple moving average cross trading system.  We'll add a column called Crossover that tells us if today is a buy signal (when the 20 day average crossed above the 52 day average) or whether today is a sell signal (when the 20 day average crossed below the 52 day average).
The values for the Crossover column will be:
-  1 = Buy signal
-  0 = No change
- -1 = Sell signal

``` python
import math

def crossover(seriesA, seriesB):
  if len(seriesA) != len(seriesB):
    raise Exception(0, 'seriesA and seriesB are not the same length')

  comparisons = (seriesA - seriesB) / abs(seriesA - seriesB)
  cross = [0]
  for i in range(1,len(comparisons)):
    current = comparisons[i]
    prev = comparisons[i-1]
    if (math.isnan(current) or math.isnan(prev)) or current == prev:
      # No change so no crossover.  (or NaN so no crossover)
      cross.append(0)
    elif current != prev:
      cross.append(current)

  return cross
```

Now we have our function to calculate the crossover row, it's just a matter of adding the row to the tsla data frame:
``` python
tsla['Crossover'] = crossover(tsla.CloseMA20, tsla.CloseMA52)
```

We can now see which days would have been moving average crossover days and whether we needed to buy or sell:
``` python
>>> DataFrame(tsla[tsla['Crossover'] != 0], columns=['Date', 'Close', 'CloseMA20', 'CloseMA52', 'Crossover'])
           Date  Close  CloseMA20  CloseMA52  Crossover
52   2010-09-13  20.72    19.9110  19.795962          1
135  2011-01-10  28.45    28.7520  28.941346         -1
192  2011-04-01  26.66    23.7345  23.691154          1
278  2011-08-04  24.75    27.9745  28.105385         -1
323  2011-10-07  26.99    25.1540  24.968077          1
378  2011-12-27  28.57    30.4135  30.479423         -1
410  2012-02-13  31.49    29.3660  29.333654          1
460  2012-04-25  32.91    34.1180  34.261346         -1
508  2012-07-03  30.66    31.0105  30.980000          1
530  2012-08-03  27.27    30.4110  30.540385         -1
570  2012-10-01  29.16    29.2970  29.228654          1
581  2012-10-16  28.06    28.9760  29.161154         -1
599  2012-11-13  31.61    29.1675  29.115385          1
685  2013-03-20  35.95    36.3400  36.453462         -1
689  2013-03-26  37.86    36.7370  36.683462          1
>>>
```
So if we had been trading this particular strategy on TSLA for the past two years we would have had 15 trading events.  Of course it's easy to pick a stock that has been trending heavily upward show that a simple moving average cross strategy would have made money, but hopefully this post has shown how fun and easy it is to do surprisingly powerful things with python and pandas.

