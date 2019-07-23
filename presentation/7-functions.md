# Functions

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Basic Usage
- Function Definition
- Inside Functions
- Lambda Functions



# Basic Usage



# Basics

Definition:

```python
def power2(num):
    result = num**2
    return result
```

Usage:
```python
>>> power2(2)
4

>>> power2(11)
121
```



## The Simplest Function

```python
def say_hello():
    print 'hello'
```

```python
>>> say_hello()
hello
```



## An Even Simpler Function

```python
def nothing():
    pass
```

```python
>>> nothing()
```



# Function Definition

- Arguments
- Return Value
- Function Name



## Arguments

- Passed By Value
- Keyword Arguments
- Default Argument Values
- Arbitrary Argument Lists
- Arbitrary Argument Dictionary



### Passed By Value
```python
def change(var):
    var = 'Changed'
```

```python
>>> aa = 'Original'
>>> change(aa)
>>> print aa
Original
```



### Argument Confusion

```python
def weather(temperature, humidity, rainfall,
            wind, pressure, visibility, clouds):
    print 'what a fine day!'
```

```python
>>> weather(30, 0.55, 0, (17, 300), 1010, 10.0, 'Few 1066m')
what a fine day!
```

It's easy to get mixed up with so many Arguments



### Keyword Arguments

```python
def weather(temperature, humidity, rainfall,
            wind, pressure, visibility, clouds):
    print 'what a fine day!'
```

<br/>
The solution is to use the arguments' keywords:

```python
>>> weather(temperature=30, humidity=0.55, rainfall=0, wind=(17, 300),
            pressure=1010, visibility=10.0, clouds='Few 1066m')
what a fine day!
```



### Keyword Arguments

Argument order becomes flexible:

```python
>>> weather(clouds='Few 1066m', temperature=30,
            rainfall=0, humidity=0.55, wind=(17, 300),
            pressure=1010, visibility=10.0)
what a fine day!
```



### Keyword Arguments

Mixing positional and keyword arguments:

```python
>>> weather(30, 0.55, 0, wind=(17, 300), pressure=1010,
            visibility=10.0, clouds='Few 1066m')
what a fine day!
```

Remember - Keyword arguments always come last!



### Default Argument Values

```python
def foo(name, number, color='blue'):
    print 'color is ' + color
```



### Default Argument Values

Good position:
```python
def foo(name, number, color='blue'):
    print 'color is ' + color
```

Bad position:
```python
def foo(name, color='blue', number):
    print 'color is ' + color
```



## Flexible Functions

- Variable number of arguments
- Variable number of keyword arguments



### Arbitrary Argument Lists (1)

```python
def flexible(name, number, *args):
    print 'number of unplanned arguments is', len(args)
    for arg in args:
        print 'extra arg is', arg

>>> flexible('Dan', 17, 'loves apples', 'loves skydiving')
number of unplanned arguments is 2
extra arg is loves apples
extra arg is loves skydiving
```

- Extra arguments are wrapped in a tuple
- The `*` sign is for tuple packing (no pointers in Python, C people!)



### Arbitrary Argument Lists (2)
#### Using Keyword Arguments

```python
def flexible(name, number, **kwargs):
    print 'number of unplanned arguments is', len(kwargs)
    for key in kwargs:
        print 'extra arg key:', key, ', value:', kwargs[key]

>>> flexible('Dan', 17, fruit='loves apples', sport='loves skydiving')
number of unplanned arguments is 2
extra arg key: fruit , value: loves apples
extra arg key: sport , value: loves skydiving
```

The ** prefix is for dictionary packing



## Mixing

```python
def flexible(name, number=17, *args, **kwargs):
    print 'name is', name
    print 'number is', number
    print 'number of unplanned arguments is', len(args)
    print 'number of unplanned keyword arguments is', len(kwargs)
```
```python
>>> flexible('Dan', 17, 'loves apples', sport='loves skydiving')
name is Dan
number is 17
number of unplanned arguments is 1
number of unplanned keyword arguments is 1

>>> flexible('Gob', 92, 17, 65, 'loves apples')
name is Gob
number is 92
number of unplanned arguments is 3
number of unplanned keyword arguments is 0
```



## Return Value
### Using Different Types

```python
def funny(arg):
    if arg == 'number':
        return 42
    elif arg == 'word':
        return 'banana'
    else:
        return True
```
```python
>>> funny('word')
'banana'

>>> funny('number')
42

>>> funny(1.2)
True
```



## Return Value
### Ignoring it

```python
def funny():
    return 42
```
```python
print 'before'
funny()
print 'after'
```

Output:

```python
before
after
```



## Function Name

The rules are similar to variable names, we'll talk about it more in the next
topic - Classes.



# Inside Functions
- Variable Scope
- Nested Functions
- Documentation



## Variable Scope
### Living inside the function

```python
def foo():
    bar = 3
    print bar
```
```
>>> foo()
3

>>> print bar
NameError: name 'bar' is not defined
```



## Variable Scope
### Defined outside the function
#### Reading

```python
name = 'Takahiro Hinata'

def foo():
    print name
```
```
>>> foo()
Takahiro Hinata
```



## Variable Scope
### Defined outside the function
#### Writing

```python
name = 'Takahiro Hinata'

def foo():
    name = 'Anabel Isidoro'
    print name
```
```
>>> foo()
Anabel Isidoro

>>> print name
Takahiro Hinata
```



## Variable Scope
### Global Variable

```python
name = 'Takahiro Hinata'

def foo():
    global name
    name = 'Anabel Isidoro'
    print name
```
```
>>> print name
Takahiro Hinata

>>> foo()
Anabel Isidoro

>>> print name
Anabel Isidoro

```



## Nested Functions
### Warm up Example

```python
def foo():
    def bar():
        return 42
    return bar() * 5
```
```python
>>> print foo()
210
```



## Nested Functions
### Closures

```python
def create_addX(x):
    def internal(y):
        return y + x
    return internal
```
```python
>>> add7 = create_addX(7)
>>> print add7(23)
30

>>> add500 = create_addX(500)
>>> print add500(10)
510
```



## Documentation
### 3 Step Solution

- Meaningful Names
- Method Documentation - Docstring
- Code Comments



## Documentation
### The Docstring - Simple Example

```python
def foo(x):
    """Short description.

    Longer description first line.
    Longer description second line.

    """
    return x + 42
```



## Documentation
### The Docstring - Real Method

```python
def create_addX(x):
    """Create a function for adding a given number to parameters.

    This function creates a new function that will always add a
    given number to the number passed to it.

    :param x: the number that the result function will always add
    :return: a function that always adds a given number

    """
    def internal(y):
        return y + x
    return internal
```



# Lambda Functions

```python
def apply(func, num):
    return func(num)

def add_42(x):
    return x + 42
```

```python
>>> apply(add_42, 500)
542

>>> apply(lambda num: num + 42, 500)
542
```



# Summary

- Functions enable us to simplify and reuse our code
- Powerful function syntax with:
    - Default values
    - Optional arguments
- Lambda functions are simple one-liners




# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
