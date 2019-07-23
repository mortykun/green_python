# Introduction to Modular Software

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Functions
- Classes
- Exceptions



# Functions



## Simple Function

```python
def say_hello():
    print 'hello'
>>> say_hello()
hello
```



## Function with Parameters

```python
def print_sum(a, b):
    print 'sum is', a + b

>>> print_sum(5, 2)
7
>>> print_sum(12, 37)
49
```



## Function returning value

```python
def power2(num):
    result = num**2
    return result
```
```python
>>> power2(2)
4

>>> power2(11)
121
```



# Classes



## Using Classes

Creating and using a new date instance

```python
>>> from datetime import date
>>> d = date(2013, 05, 22)
>>> print d
2013-05-22
```



## Using Built-in Classes

list
```python
>>> pi_digits = [3, 1, 4, 1, 6]

>>> print pi_digits
[3, 1, 4, 1, 6]
>>> pi_digits.remove(4)
>>> print pi_digits
[3, 1, 1, 6]
```

string
```python
>>> s = 'pi_digits contains digits from pi'

>>> print s
pi_digits contains digits from pi
>>> print s.upper()
PI_DIGITS CONTAINS DIGITS FROM PI
```



## Defining New Classes

```python
class Banana(object):
    pass

>>> b = Banana()
```



## Class Methods

```python
class Banana(object):
    def taste(self):
        return 'sweet'

>>> b = Banana()
>>> print b.taste()
sweet
```



## Class Methods (2)

```python
class Banana(object):
    def taste_sweet(self):
        return 'sweet'
    def taste_sour(self):
        return 'sour'
    def get_taste(self, color):
        if color == 'yellow':
            return self.taste_sweet()
        return self.taste_sour()

>>> b = Banana()
>>> print b.get_taste('green')
sour
```



# Exceptions



## Unhandled Exception

```python
>>> print f
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'f' is not defined
```



## There is a problem here

```python
def first():
    second()

def second():
    third()

first()
```



## How the crash looks like

```python
$ python simple_crash.py
Traceback (most recent call last):
  File "simple_crash.py", line 7, in <module>
    first()
  File "simple_crash.py", line 2, in first
    second()
  File "simple_crash.py", line 5, in second
    third()
NameError: global name 'third' is not defined
```



## Handling the Exception

```python
def first():
    second()

def second():
    try:
        third()
    except NameError:
        print "Sorry, can't find the function third"

first()
```
```python
$ python simple_crash_handled.py
Sorry, can't find the function third
```



# Summary

- Functions handle specific tasks
- Classes simulate physical objects and have internal values and functionality
- Exceptions simplify our code, enable us to handle problems



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
