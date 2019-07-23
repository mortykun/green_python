# Strings

<br/>
[Amit Kotlovski](mailto:amit@amitkot.com) / [@amitkot](http://twitter.com/amitkot)



# Agenda

- Building Strings
- Printing
- String Methods
- String Formatting
- Slicing, Splitting and Joining



# Building Strings

- Quotes
- Special Characters
- Concatenation



## Quotes

`'Single Quotes'` and `"Double Quotes"`

```python
>>> a = 'this is a string'
>>> b = "this is also a string"
>>> a
'this is a string'
>>> b
'this is also a string'
```



## Quotes

`'''Three Single Quotes'''` and `"""Three Double Quotes"""`

```python
>>> c = '''This is a
Multi-Line
String'''
>>> d = """This is also a
multi
line String
"""
>>> c
'This is a\nMulti-Line\nString'
>>> d
'This is also a\nmulti\nline String\n'
```



## Special Characters

There are characters with special meaning, preceded by a backslash `\`:

```
# \n is for newline
>>> print('first\nsecond')
first
second
```
```
# \t for horizontal tabs
>>> print('first\tsecond\tthird')
first	second	third
```
```
# \v for vertical tabs
>>> print('first\vsecond\vthird')
first
     second
           third
```



## Special Characters
### Escaping special characters

```
# escaping the \ char
>>> print('first\\nsecond')
first\nsecond
```
```
# escaping quotes
>>> print('\'quoted\'')
'quoted'
```



## Special Characters
**Escape Sequence** | **Meaning**
--- | ---
\newline | Ignored
\\ | Backslash (\)
\' | Single quote (')
\" | Double quote (")
\n | ASCII Linefeed (LF)
\t | ASCII Horizontal Tab (TAB)
\v | ASCII Vertical Tab (VT)
\ooo | Character with octal value ooo
\xhh | Character with hex value hh
<br/>
Full list on 
http://docs.python.org/reference/lexical_analysis.html



## Raw Strings

Prefix the string with an `r` to disable Special Characters

```python
>>> print('first\nsecond')          # normal string
first
second

>>> print(r'first\nsecond')         # raw string
first\nsecond```



## Concatenation
### Consecutive String Literals

Strings Literals written one after the other get concatenated:
```python
>>> 'bananas' 'apples' 'tomatoes'
'bananasapplestomatoes'
```

Does not work with variables:
```python
>>> a = 'apples'
>>> a 'bananas' 'tomatoes'
  File "<stdin>", line 1
    a 'bananas' 'tomatoes'
              ^
SyntaxError: invalid syntax
```



## Concatenation
### Joining Strings Using `+`
```
>>> a = 'apples'
>>> a + 'bananas' + 'tomatoes'
'applesbananastomatoes'
```



## Concatenation
### Joining Different Types

```python
# mixing apples and numbers
>>> print(3 + ' apples')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'

# Use str() to convert to string
>>> print(str(3) + ' apples')
3 apples
```



# Printing
## Python 3 `print`
## Python 2 `print`



## Python 3 `print` expression

```python
>>> if True:
...     print('first', 'second')
...     print('another line')
first second
another line

>>> if True:
...     print('first', 'second', sep=', ', end='. ')
...     print('another line', end='.')
first, second. another line.

```



## Python 2 `print` statement

```python
>>> if True:
...     print 'first', 'second'
...     print 'another line'
first second
another line

# The , in the end eliminates the new line
>>> if True:
...     print 'first, ', 'second.', 
...     print 'another line.'
first, second. another line.

```



## Using Python 3's `print()` function in Python 2

```python
# using Python 2.7.4
>>> print('first', 'second', sep=', ', end='. ')
  File "<stdin>", line 1
    print('first', 'second', sep=', ', end='. ')
                                ^
SyntaxError: invalid syntax

>>> from __future__ import print_function
>>> if True:
...     print('first', 'second', sep=', ', end='. ')
...     print('another line', end='.')
first, second. another line.
```



# String Methods

- String Testing
- String Modification



## String Testing

There are many string functions for testing various conditions of the string
```python
>>> b = 'banana'
>>> b.endswith('na')
True
>>> b.endswith('ga')
False
```
```
>>> 'ANA' in b
False
>>> 'nana' in b
True
```
```
>>> b.count('na')
2
```
```
>>> c = 'the cat has eaten all your cake'
>>> c.find('has')
8
```



## String Testing
### Selected Functions

function                | description
--                      | --
count(_sub_)            | how many times _sub_ appears in string
endswith(_suffix_)      | True if ends with _suffix_
find(_sub_)             | get lowest index where _sub_ is
_sub_ in _str_          | check if _sub_ is a substring of _str_
isalpha()               | True if at least one char, all letters
islower()               | True if all cased chars are lowercase
isnumeric()             | True if at least one char, all numeric
isspace()               | True if at least one char, only whitespace
istitle()               | True if at least one char, all words have first letter capitalized
isupper()               | True if at least one char, all chars are uppercase
startswith(_prefix_)    | return True if string starts with _prefix_

<br/>
Find them all at [ The Python Documentation ](http://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)



## String Modification

```python
>>> b = 'drink a lot of water every day          '
# switch to uppercase
>>> b.upper()
'DRINK A LOT OF WATER EVERY DAY          '
```
```python
# replace sub with new one
>>> b.replace('water', 'lemonade')
'drink a lot of lemonade every day          '
```
```python
# striping whitespace
>>> b.lstrip()
'drink a lot of water every day          '
>>> b.strip()
'drink a lot of water every day'
```
```python
# split and return a 3-tuple
>>> b.partition('of')
('drink a lot ', 'of', ' water every day          ')
```



## String Modification
### Selected Functions

function                        | description
--                              | --
center(_width_, [_fillchar_])   | wrap with _fillchar_ until str is at least _width_ long
ljust(_width_, [_fillchar_])   | left justify using _fillchar_ in a string at least _width_ long
lower()                         | changes all letters to lower case
rjust(_width_, [_fillchar_])   | right justify using _fillchar_ in a string at least _width_ long
splitlines([_keepends_])        | split to a list of lines with/without newlines
upper()                         | changes all letters to upper case

<br/>
Find them all at [ The Python Documentation ](http://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)



# String Formatting

- `format()`
- `%`



## `format()`

How many strings do we create here?

```
>>> 'this ' + 'is ' + 'a very ' + 'lovely '\
    + 'way to ' + 'write'
'this is a very lovely way to write'
```

<span class="fragment">(11)</span>



## `format()`

Manually using _variable_ values with strings

```python
>>> name = input('Please enter your name: ')
Please enter your name: Dan
>>> hello = 'Hello ' + name + ', nice to meet you!'
>>> print(hello)
Hello Dan, nice to meet you!
```



## `format()`
### Use `format` for _templates_

```python
# with position
>>> 'Hello {0}, {1} to meet you!'.format('Dan', 'great')
'Hello Dan, great to meet you!'
```



## `format()`
### Positions

```python
# without position
>>> 'Hello {}, {} to meet you!'.format('Dan', 'nice')
'Hello Dan, nice to meet you!'
```
```python
# reverse position
>>> 'Hello {1}, {0} to meet you!'.format('Dan', 'nice')
'Hello nice, Dan to meet you!'
```
```python
# repeating an argument
>>> 'Hello {0}, {0} to meet you!'.format('Dan', 'nice')
'Hello Dan, Dan to meet you!'
```
```python
# argument by name
>>> 'A {adj} {obj}'.format(adj='yellow', obj='banana')
'A yellow banana'
```



## `format()`
### Alignment and Width

```python
# left align, width - 10
>>> '{:<10}'.format('cat')
'cat       '
```
```python
# right align, width - 10
>>> '{:>10}'.format('cat')
'       cat'
```
```python
# center align, width - 10
>>> '{:^10}'.format('cat')
'   cat    '
```
```python
# center align, width - 10, pad with _
>>> '{:_^10}'.format('cat')
'___cat____'
```



## `format()`
### Digits

```python
# give number at least 5 chars
>>> '{:5}'.format(3)
'    3'
```
```python
# more than 5 are needed
>>> '{:5}'.format(300000)
'300000'
```
```python
# give at least 5 chars, fill with 0 to the left
>>> '{:05}'.format(3)
'00003'
```



## `format()`
### Precision

```python
# convert number to float with precision 3
>>> '{:0.3f}'.format(37)
'37.000'
```
```python
>>> '{:0.3f}'.format(37.0004)	# round down
'37.000'
```
```python
>>> '{:0.3f}'.format(37.0005)	# round up
'37.001'
```



## `format()`
### Advanced: Using different bases

Base        | Digits
--          | --
Binary      | 0, 1
Octal       | 0, 1, 2, 3, 4, 5, 6, 7
Decimal     | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
Hexadecimal | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, a, b, c, d, e, f



## `format()`
### Advanced: Using different bases

Base                | Represenation
--                  | --
1 in Binary         | <span class="fragment">1</span>
2 in Binary         | <span class="fragment">10</span>
---                 | 
7 in Octal          | <span class="fragment">7</span>
8 in Octal          | <span class="fragment">10</span>
---                 | 
9 in Hexadecimal    | <span class="fragment">9</span>
10 in Hexadecimal   | <span class="fragment">a</span>
15 in Hexadecimal   | <span class="fragment">f</span>
16 in Hexadecimal   | <span class="fragment">10</span>

---
How would you write 27 in other bases?



## `format()`
### Advanced: Using different bases

```python
# converting bases
>>> "int: {0:d}  bin: {0:b}  oct: {0:o}  \
...  hex: {0:x}".format(27)
'int: 27  bin: 11011  oct: 33  hex: 1b'

# display prefixes
>>> "int: {0:#d}  bin: {0:#b}  oct: {0:#o}  \
...  hex: {0:#x}".format(27)
'int: 27  bin: 0b11011  oct: 0o33  hex: 0x1b'
```



## `format()`
### Syntax

```
"{" [field_name\number] [":" [[fill]align][sign][#][0][width]
                             [,][.precision][type]] "}"

fill        ::=  <a character other than '{' or '}'>
align       ::=  "<" | ">" | "=" | "^"
sign        ::=  "+" | "-" | " "
width       ::=  integer
precision   ::=  integer
type        ::=  "b" | "c" | "d" | "e" | "E" | "f" 
                 | "F" | "g" | "G" | "n" | "o" 
                 | "s" | "x" | "X" | "%"
```
Read more on [ Python Documentation ](http://docs.python.org/3/library/string.html#formatstrings)



## Old c-style formating

The base syntax is `format % values`

```python
# basic
>>> 'I ate %d bananas' % 20
'I ate 20 bananas'

# using strings, floats with 6 digits and precision of 2
>>> '%s %06.2f cups of %s' % ('Drink', 3, 'water')
'Drink 003.00 cups of water'

# using keywords
>>> '%(action)s %(num)d cups' % {'action': 'Pour', 'num': 12}
'Pour 12 cups'
```



# Slice, Split and Join

- Slice
- Split
- Join



## Slice
### Using sequence tactics on a string (1)

We'll use:
```python
>>> s = 'abcdefghijklmnopqrstuvwxyz'
```

```python
# Specific char
>>> s[2]
'c'
>>> s[-1]
'z'
```



## Slice
### Using sequence tactics on a string (2)

```python
# ranges
>>> s[6:12]
'ghijkl'
>>> s[6:]
'ghijklmnopqrstuvwxyz'
>>> s[:12]
'abcdefghijkl'
>>> s[1:-1:2]
'bdfhjlnprtvx'
```



## Slice
### Using sequence tactics on a string (3)

```python
# Multiplication
>>> 'cat' * 3
'catcatcat'
```



## `split()`

Example for a Comma Separated Values (CSV) data file:

    july,tel-aviv,hot,40
    january,jerusalem,cold,9
    April,hertzlia,warm,28
    ...



## `split()`

Separating the data using the `','` string:

    >>> s = 'july,tel-aviv,hot,40'
    >>> s.split(',')
    ['july', 'tel-aviv', 'hot', '40']



## `split()`

Separating using a longer string:

    >>> 'tomato is the thing to eat'.split('to')
    ['', 'ma', ' is the thing ', ' eat']



## `join()`

Concatenating a collection of strings

```python
>>> ', '.join(['Jon', 'Robb', 'Sansa', \
               'Arya', 'Bran', 'Rickon'])
'Jon, Robb, Sansa, Arya, Bran, Rickon'
```



# Summary

- Printing - Using `print()`
- Building Strings - Using `'`, `"`, `"""` and `+`
- String Methods
- String Formatting - Using `format()`
- Slicing, Splitting and Joining



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

All Rights Reserved to Amit Kotlovski
