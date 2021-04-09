---
layout: page
title: Python Tutorial for Beginners - 12. Using Try/Except Blocks for Error Handling
description: >
  Python Tutorial for Beginners - 1. Using Try/Except Blocks for Error Handling...
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


##  Test file

~~~sh
# file: 'test_file.txt'
Test File Contents!
~~~
test_file.txt
{:.figure}

##  Currupt file

~~~sh
# file: 'currupt_file.txt'
Currupt File!
~~~
currupt_file.txt
{:.figure}


##  Run Code 1 in Python

~~~py
# file: 'app1.py'
try:
     pass
except Exception:
    pass
else:
   pass
finally:
   pass
~~~


##  Run Code 2 in Python

~~~py
# file: 'app2.py'
f = open('testfile.txt')

try:
     pass
except Exception:
    pass
~~~

##  Run Code 3 in Python

~~~py
# file: 'app3.py'
try:
    f = open('testfile.txt')
except Exception:
    print('Sorry. This file doenst exist')
~~~

##  Run Code 4 in Python

~~~py
# file: 'app4.py'
try:
    f = open('test_file.txt')
    var = bad_var
except FileNotFoundError:
    print('Sorry. This file doenst exist')
~~~


##  Run Code 5 in Python

~~~py
# file: 'app5.py'
try:
    f = open('test_file.txt')
    var = bad_var
except FileNotFoundError:
    print('Sorry. This file doenst exist')
except Exception:
    print('Sorry. Samting went wrong')
~~~

##  Run Code 6 in Python

~~~py
# file: 'app6.py'
try:
    f = open('testfile.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
~~~

##  Run Code 7 in Python

~~~py
# file: 'app7.py'
try:
    f = open('test_file.txt')
except FileNotFoundError as e:
    print(e)
except Exception as e:
    print(e)
else:
   print(f.read())
   f.close()
~~~


##  Run Code 8 in Python

~~~py
# file: 'app8.py'
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
~~~

##  Run Code 9 in Python

~~~py
# file: 'app9.py'
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
~~~

##  Run Code 10 in Python

~~~py
# file: 'app10.py'
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
~~~


##  Run Code 11 in Python

~~~py
# file: 'app11.py'
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
~~~