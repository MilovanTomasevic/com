---
layout: page
title: Python Tutorial for Beginners - 1. Strings - Working with Textual Data
description: >
  Python Tutorial for Beginners - 1. Strings - Working with Textual Data...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
print('Hello World')
~~~

##  Run Code 2 in Python

~~~py
# file: 'app2.py'
message = 'Hello World'
print(message)
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
message = 'Milovan's World'
print(message)
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
message = 'Milovan\'s World'
print(message)
~~~

##  Run Code 5 in Python

~~~py
# file: 'app5.py'
message = "Milovan\'s World"
print(message)
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
message = """Hello World
test test test"""
print(message)
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
message = 'Hello World'
print(len(message))
~~~

##  Run Code 8 in Python

~~~py
# file: 'app8.py'
message = 'Hello World'
print(message[0])
print(message[10])
print(message[11]) #error
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
message = 'Hello World'
print(message[0:5])
print(message[:5])
print(message[6:])
~~~

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
message = 'Hello World'
print(message.lower())
~~~

##  Run Code 11 in Python

~~~py
# file: 'app11.py'
message = 'Hello World'
print(message.upper())
print(message.count('Hello'))
print(message.count('l'))
~~~

##  Run Code 12 in Python

~~~py
# file: 'app12.py'
message = 'Hello World'
print(message.find('World'))
~~~

##  Run Code 13 in Python

~~~py
# file: 'app13.py'
message = 'Hello World'
print(message.find('Universe'))
~~~

##  Run Code 14 in Python

~~~py
# file: 'app14.py'
message = 'Hello World'
message.replace('World', 'Universe')
print(message)
~~~

##  Run Code 15 in Python

~~~py
# file: 'app15.py'
message = 'Hello World'
new_message = message.replace('World', 'Universe')
print(new_message)
~~~

##  Run Code 16 in Python

~~~py
# file: 'app16.py'
message = 'Hello World'
message = message.replace('World', 'Universe')
print(message)
~~~

##  Run Code 17 in Python

~~~py
# file: 'app17.py'
greeting = 'Hello'
name = 'Milovan'
message = greeting + name
print(message)
~~~

##  Run Code 18 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'
message = greeting + ', '+ name
print(message)
~~~

##  Run Code 19 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

message = greeting + ', '+ name + '. Welcome!'

print(message)
~~~

##  Run Code 20 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

message = '{}, {}. Welcome!'.format(greeting, name)

print(message)
~~~

##  Run Code 21 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

message = f'{greeting}, {name}. Welcome!'

print(message)
~~~

##  Run Code 22 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

message = f'{greeting}, {name.upper()}. Welcome!'

print(message)
~~~

##  Run Code 23 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

print(dir(name))
~~~

##  Run Code 24 in Python

~~~py
# file: 'app.py'
greeting = 'Hello'
name = 'Milovan'

print(help(str))
~~~

