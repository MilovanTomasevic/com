---
layout: page
title: Python Tutorial for Beginners - 9. File Objects - Reading and Writing to Files
description: >
  Python Tutorial for Beginners - 9. File Objects - Reading and Writing to Files...
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

##  Test text

{% highlight text linenos %}
1) This is a test file
2) With multiple lines of data...
3) Third line
4) Fourth line
5) Fifth line
6) Sixth line
7) Seventh line
8) Eighth line
9) Ninth line
10) Tenth line
{% endhighlight %}
text.txt
{:.figure}

##  Run Code 1 in Python

{% highlight text linenos %}
#File Objects

##The Basics:
f = open("test.txt", "r")
f = open("test.txt", "w")
f = open("test.txt", "a")
f = open("test.txt", "r+")
print(f.name)
print(f.mode)
f.close()
{% endhighlight %}
text.txt
{:.figure}


##  Run Code 2 in Python

{% highlight python linenos %}
##Reading Files:
with open("test.txt", "r") as f:
	pass
{% endhighlight %}
start.py
{:.figure}

##  Run Code 3 in Python

{% highlight python linenos %}
##Reading Files: uncomment part by part 
with open("test.txt", "r") as f:
	#pass

	#Small Files:
	f_contents = f.read()
	print(f_contents)

	##Big Files:
	#f_contents = f.readlines()
	#print(f_contents)

    ###With the extra lines:
	#f_contents = f.readline()
	#print(f_contents)
	#f_contents = f.readline()
	#print(f_contents)

	###Without the extra lines:
	#f_contents = f.readline()
	#print(f_contents, end = '')
	#f_contents = f.readline()
	#print(f_contents, end = '')

	###Iterating through the file:
	#for line in f:
		#print(line, end = '')

	###Going Back....:
	#f_contents = f.read()
	#print(f_contents, end = '')

	###Printing by characters:
	#f_contents = f.read(100)
	#print(f_contents, end = '')
	#f_contents = f.read(100)
	#print(f_contents, end = '')
	#f_contents = f.read(100)
	#print(f_contents, end = '')

	###Iterating through small chunks:
	#size_to_read = 100
	#f_contents = f.read(size_to_read)
	#while len(f_contents) > 0:
		#print(f_contents)
		#f_contents = f.read(size_to_read)

	###Iterating through small chunks, with 10 characters:
	#size_to_read = 10
	#f_contents = f.read(size_to_read)
	#print(f_contents, end = '')
	#f.seek(0)
	#f_contents = f.read(size_to_read)
	#print(f_contents, end = '')
	#print(f.tell())
	#while len(f_contents) > 0:
		#print(f_contents, end = '*')
		#f_contents = f.read(size_to_read)
#print(f.mode)
#print(f.closed)
#print(f.read())
{% endhighlight %}
start.py
{:.figure}

##  Run Code 4 in Python

{% highlight python linenos %}
##Writing Files:
###The Error:
with open("test.txt", "r") as f:
	f.write("Test")

###Writing Starts:
with open("test2.txt", "w") as f:
	pass
	f.write("Test")
	f.seek(0)
	f.write("Test")
	f.seek("R")
{% endhighlight %}
start.py
{:.figure}


##  Run Code 5 in Python

{% highlight python linenos %}
##Copying Files:
with open("test.txt", "r") as rf:
	with open("test_copy.txt", "w") as wf:
		for line in rf:
			wf.write(line)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 6 in Python

{% highlight python linenos %}
#Copying the/your image:
###The Error
with open("yourPic.jpg", "r") as rf:
	with open("yourPic_copy.jpg", "w") as wf:
		for line in rf:
			wf.write(line)
{% endhighlight %}
start.py
{:.figure}

##  Run Code 7 in Python

{% highlight python linenos %}
###Copying the image starts, without chunks:
with open("yourPic.jpg", "rb") as rf:
	with open("yourPic_copy.jpg", "wb") as wf:
		for line in rf:
			wf.write(line)
{% endhighlight %}
start.py
{:.figure}


##  Run Code 8 in Python

{% highlight python linenos %}
###Copying the image with chunks:
with open("yourPic.jpg", "rb") as rf:
	with open("yourPic_copy.jpg", "wb") as wf:
		chunk_size = 4096
        rf_chunk = rf.read(chunk_size)
        while len(rf_chunk) > 0:
            wf.write(rf_chunk)
            rf_chunk = rf.read(chunk_size)
{% endhighlight %}
start.py
{:.figure}
