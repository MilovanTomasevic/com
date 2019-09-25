---
layout: page
title: Python Tutorial for Beginners - 10. CSV Module - How to Read, Parse, and Write CSV Files
description: >
  Python Tutorial for Beginners - 10. CSV Module - How to Read, Parse, and Write CSV Files...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---


##  Test text

{% highlight csv linenos %}
first_name,last_name,email
John,Doe,john-doe@bogusemail.com
Mary,Smith-Robinson,maryjacobs@bogusemail.com
Dave,Smith,davesmith@bogusemail.com
Jane,Stuart,janestuart@bogusemail.com
Tom,Wright,tomwright@bogusemail.com
Steve,Robinson,steverobinson@bogusemail.com
Nicole,Jacobs,nicolejacobs@bogusemail.com
Jane,Wright,janewright@bogusemail.com
Jane,Doe,janedoe@bogusemail.com
Kurt,Wright,kurtwright@bogusemail.com
Kurt,Robinson,kurtrobinson@bogusemail.com
Jane,Jenkins,janejenkins@bogusemail.com
Neil,Robinson,neilrobinson@bogusemail.com
Tom,Patterson,tompatterson@bogusemail.com
Sam,Jenkins,samjenkins@bogusemail.com
Steve,Stuart,stevestuart@bogusemail.com
Maggie,Patterson,maggiepatterson@bogusemail.com
Maggie,Stuart,maggiestuart@bogusemail.com
Jane,Doe,janedoe@bogusemail.com
Steve,Patterson,stevepatterson@bogusemail.com
Dave,Smith,davesmith@bogusemail.com
Sam,Wilks,samwilks@bogusemail.com
Kurt,Jefferson,kurtjefferson@bogusemail.com
Sam,Stuart,samstuart@bogusemail.com
Jane,Stuart,janestuart@bogusemail.com
Dave,Davis,davedavis@bogusemail.com
Sam,Patterson,sampatterson@bogusemail.com
Tom,Jefferson,tomjefferson@bogusemail.com
Jane,Stuart,janestuart@bogusemail.com
Maggie,Jefferson,maggiejefferson@bogusemail.com
Mary,Wilks,marywilks@bogusemail.com
Neil,Patterson,neilpatterson@bogusemail.com
Corey,Davis,coreydavis@bogusemail.com
Steve,Jacobs,stevejacobs@bogusemail.com
Jane,Jenkins,janejenkins@bogusemail.com
John,Jacobs,johnjacobs@bogusemail.com
Neil,Smith,neilsmith@bogusemail.com
Corey,Wilks,coreywilks@bogusemail.com
Corey,Smith,coreysmith@bogusemail.com
Mary,Patterson,marypatterson@bogusemail.com
Jane,Stuart,janestuart@bogusemail.com
Travis,Arnold,travisarnold@bogusemail.com
John,Robinson,johnrobinson@bogusemail.com
Travis,Arnold,travisarnold@bogusemail.com
{% endhighlight %}
names.csv
{:.figure}

##  Run Code 1 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    for line in csv_reader:
        print(line)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    for line in csv_reader:
        print(line[2])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    next(csv_reader)
    
    for line in csv_reader:
        print(line[2])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    with open('new_names.csv', 'w') as new_file:
        csv_writer = csv.writer(new_file, delimiter='-')

        for line in csv_reader:
            csv_writer.writerow(line)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    with open('new_names.csv', 'w') as new_file:
        csv_writer = csv.writer(new_file, delimiter='\t')

        for line in csv_reader:
            csv_writer.writerow(line)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
import csv

with open('new_names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    for line in csv_reader:
        print(line)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
import csv

with open('new_names.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter='\t')
    for line in csv_reader:
        print(line)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)

    for line in csv_reader:
        print(line)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 9 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)

    for line in csv_reader:
        print(line['email'])
{% endhighlight %}
start.py
{:.figure}

##  Run Code 10 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)

    with open('new_names.csv', 'w') as new_file:
        fieldnames = ['first_name', 'last_name', 'email']

        csv_writer = csv.DictWriter(new_file, fieldnames=fieldnames, delimiter='\t')

        csv_writer.writeheader()

        for line in csv_reader:
            csv_writer.writerow(line)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 11 in Python

{% highlight python linenos %}
import csv

with open('names.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)

    with open('new_names.csv', 'w') as new_file:
        fieldnames = ['first_name', 'last_name']

        csv_writer = csv.DictWriter(new_file, fieldnames=fieldnames, delimiter='\t')

        csv_writer.writeheader()

        for line in csv_reader:
            del line['email']
            csv_writer.writerow(line)
{% endhighlight %}
start.py
{:.figure}