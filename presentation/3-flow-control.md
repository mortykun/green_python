# Flow Control

> “Life is a series of natural and spontaneous changes. Don’t resist them –
that only creates sorrow. Let reality be reality. Let things flow naturally
forward in whatever way they like.” - Lao-Tzu

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Conditional Statements
- Looping Mechanisms



# Conditional Statements

- `if`
- Nested `if`



## When is it needed?
### Examples

- Good morning / afternoon / evening / night
- Password verification
- Personalized wedding invitations



## Python Conditionals
### Ingredients

- Condition
- Action



## `if`
### Example

```python
passwd = raw_input('Please enter your password:')
if passwd == 'banana':
    print('Password is correct!')
```



## `if`
### Syntax

    if condition1:
        action1
    elif condition2:
        action2
    elif condition3:
        action3
    .
    .
    .
    else:
        actionX



## `if`
### There is no `switch` statement in Python

Instead use if-elif-else to implement all different cases



## Nested `if`s
### An `if` inside an `if`

    if color == 'green':
        if size_in_cm == 10:
            print('It could be an apple')
        elif size_in_cm == 50:
            print('It surely is a watermelon')



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Looking for the Truth

- Truth Values
- Comparison Operators
- Comparing Different Types
- Boolean Operations
- Chained Comparisons
- Conditional Expressions
- Sequence and Collection Tests



## Truth Value
### Examples

```python
>>> a = True
>>> if a == True:           # True == True
...    print("a is True")
a is True
```
And:

```python
>>> if 3 < 5:
...    print("3 is smaller")
3 is smaller
```



## Truth Value
### Values have Truth Values

```python
>>> a = True
>>> if a == True:           # True == True
...    print("a is True")
a is True
```

Can also be written:

```python
>>> a = True
>>> if a:                   # True
...    print("a is True")
a is True
```



## True Value
### So what value evaluate as True?
<span class="fragment">
Almost _all_ of them
</span>



## Truth Value
### Example

```python
>>> a = 500
>>> if a:                               # 500 evaluates as True
...    print("500 is True as well")
500 is True as well
```



## Truth Value
### What is False?

- `None`
- `False`
- `0` in a Numerical value (`0`, `0.0`, etc.)
- Empty Sequence - `""`, `[]`, `()`
- Empty Dictionary - `{}`
- Empty Set - `set()`



## True or False?
### Quiz

- `"Magic"`
- `True`
- `231`
- `231 - 231`
- `['a', 'small', 'step']`
- `{}`



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



## Comparison Operators
- `>`
- `<`
- `==`
- `>=`
- `<=`
- `!=` (Also `<>` in old code)
- `is [not]`
- `[not] in`



## `is`
### Returns True if the objects are the same
- 3 and 3 are the same
- '3' and '3' are the same
- 3 and '3' are **not** the same



## `is`
### Quiz - True or False?

Expression      | True or False?
--              | --
`7 is 7`        | <span class="fragment">True</span>
`7 == 7`        | <span class="fragment">True</span>
`7 == 7.0`      | <span class="fragment">True</span>
`7 is 7.0`      | <span class="fragment">False</span>



## x `in` s
### Return True if x is a member of s

    items = ['apple', 'banana', 'cat']
    'cat' in items			# True
    'dog' in items			# False



## `in`
### Quiz - True or False?

```
items = ['apple', 'banana', 'cat']
'cat' in items                      # True or False?
```

```
dict = {'one': 1, 'two': 2}
'two' in dict                       # True or False?
2 in dict                           # True or False?
```

```
name = 'Inigo Montoya'
'toy' in name                       # True or False?
'inigo' in name                     # True or False?
```



## Comparing Different Types
Useful for numerical types

```python
>>> 5 > 3.3
True
```



## Comparing Different Types
Dangerous\* when comparing Number and String

```python
>>> '1' < 2

# in Python 2
False     # Don't compare different types!

# in Python 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() < int()
```
<br/>
<br/>
( \* No longer possible in Python3 )



# Boolean Operations

> Connecting multiple conditions



## Bananas

<img src="img/bananas.jpg" alt="http://fr.fotopedia.com/items/zeb-tE_PEixKPoo" style="height: 500px;"/>



## Detecting a Tasty Banana
### Condition Parts

- `color == 'yellow'`
- `color == 'brown'`
- `taste == 'sweet'`
- `weight < 100`
- `length >= 3`
- `length <= 30`
- `has_seeds == False`



## Detecting a Tasty Banana
### Connecting with `and`, `or` and `not`

```python
(color == 'yellow' or color == 'brown') and \
taste == 'sweet' and \
weight < 100 and \
length >= 3 and length <= 30 and \
not has_seeds
```



## Short Circuit
### Testing only what is needed

What about a yellow, sour object?

```python
(color == 'yellow' or color == 'brown') and \
taste == 'sweet' and \
weight < 100 and \
length >= 3 and length <= 30 and \
not has_seeds
```



## Short Circuit
### Summary

- _a_ `and` _b_ - evaluate _b_ only if _a_ is True
- _a_ `or` _b_ - evaluate _b_ only if _a_ if False



# Chained Comparisons



## Combine Two Conditions

```python
length >= 3 and length <= 30

# Can be shortened to:

3 <= length <= 30
```



## Chaining is More Efficient
Middle part is calculated only once

```python
>>> if 4999999940000000 < sum(range(100000000)) < 4999999950000001:
        print "True"
True
```




## Chained Comparisons
### Summary

- __+__ More natural to write and read
- __+__ Shorter code
- __+__ More efficient
- __-__ Odd comparisons are valid:
 - `4 < x > 500`



## Chained Comparisons
### Quiz - What do these mean?

```python
20 < x < 32
```

```python
500 > x < 20
```



# Conditional Expressions

> Concise syntax for `if` statements



## What we learnt so far

    if fruit == 'banana':
        taste = sweet
    else:
        taste = sour



## a.k.a. Ternary Operator

`X if C else Y`

```python
taste = 'sweet' if fruit == 'banana' else 'sour'
```



# Sequence and Collection Tests



## `all()`
### `True` if all items are `True`

```python
>>> love_programming = [True, True, True, True]
>>> love_singing = [True, True, False, True]

>>> all(love_programming)
True

>>> all(love_singing)
False
```



## `any()`
### `True` if at least one item is `True`

```python
>>> love_programming = [True, True, True, True]
>>> love_singing = [True, True, False, True]

>>> any(love_programming)
True

>>> any(love_singing)
True
```



## `any()`, `all()`
### Note - Usage with Dictionaries

With dictionaries those methods check the keys, making it less useful, and even
surprising at times

    >>> d = {0: '0', None: 'None'}
    >>> d.keys()
    dict_keys([0, None])

    >>> any(d)
    False



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Loops

- `while`
- `for`



## `While` Loop
### Run as long as a condition holds



## Example

Let's sum all the numbers starting at 1 and ending with the first number that
divides in 311:

```python
total = 0
if 1 % 311 != 0:
    total += 1
if 2 % 311 != 0:
    total += 2
if 3 % 311 != 0:
    total += 3
.
.
.
```



## `While` Loop
### Syntax

```python
while condition:
    action1
else:
    action2
```



## Rewriting our example using `while`

```python
total = 0
i = 1
while i % 311 != 0:
    total += i
    i += 1
```



## `for` Loops
### Run action for every item in a collection



## Example

Given a list of fruits, print each one to the screen with the number of letters
in it's name

```python
>>> fruits = ['banana', 'apple', 'lychee']
>>> i = 0
... while i < len(fruits):
...     print(fruits[i], len(fruit[i]))
...     i += 1
banana 6
apple 5
lychee 6
```



## Rewriting our example using `for`

```python
# >>> fruits = ['banana', 'apple', 'lychee']
# >>> i = 0
# ... while i < len(fruits):
# ...     print(fruits[i], len(fruit[i]))
# ...     i += 1

>>> for single_fruit in fruits:
...     print(single_fruit, len(single_fruit))
banana 6
apple 5
lychee 6
```



# Looping tools

- `range`
- `enumerate`
- `zip`



## Using `range()`
### Run a task a certain number of times



## Example
Print the sentence "All you need is love" five times

```python
>>> i = 0
... while i < 5:
...     print("All you need is love")
...     i += 1
All you need is love
All you need is love
All you need is love
All you need is love
All you need is love
```



## Using `range()`

```python
>>> range(5)        # Nubers from 0 to 4
[0, 1, 2, 3, 4]

>>> range(2, 9)     # Numbers from 2 to 8
[2, 3, 4, 5, 6, 7, 8]

>>> range(2, 9, 3)  # Numbers from 2 to 8 in steps of 3
[2, 5, 8]
```



## Rewriting our example

```python
# >>> i = 0
# ... while i < 5:
# ...     print("All you need is love")
# ...     i += 1

>>> for i in range(5):
...     print("All you need is love")
All you need is love
All you need is love
All you need is love
All you need is love
All you need is love
```



## Using `range()`
### Quiz

How can we use `range()` to create this list?

```python
[10, 8, 6, 4, 2]
```



## `enumerate()`

Iterating over a collection of items, using both the item and it's index:

```python
>>> fruits = ['banana', 'apple', 'lychee']
>>> for i in range(len(fruits)):
...     print('no.', i, 'is', fruits[i])
no. 0 is banana
no. 1 is apple
no. 2 is lychee
```



## Rewrite using `enumerate()`

```python
# >>> fruits = ['banana', 'apple', 'lychee']
# >>> for i in range(len(fruits)):
# ...     print('no.', i, 'is', fruits[i])

>>> for i, single_fruit in enumerate(fruits):
...     print('no.', i, 'is', single_fruit)
no. 0 is banana
no. 1 is apple
no. 2 is lychee
```



## Quiz
What is the difference between these two forms?

```python
>>> for i, single_fruit in enumerate(fruits):
...     pass
```

and:

```python
>>> for (i, single_fruit) in enumerate(fruits):
...     pass
```



## `zip()`

Merge two lists with items in matching positions:

```python
>>> stuff = ['Electric Light', 'Microwave Oven',
...          'Canned Food']
>>> people = ['Thomas Edison', 'Percy Spencer',
...           'Nicolas Appert']
>>> for i in range(len(stuff)):
...     print('{0} had invented the {1}'.format(people[i],
...                                             stuff[i]))
Thomas Edison had invented the Electric Light
Percy Spencer had invented the Microwave Oven
Nicolas Appert had invented the Canned Food
```



## `zip()`
### Usage

```python
>>> letters = ['a', 'b', 'c', 'd']
>>> capitals = ['A', 'B', 'C', 'D']
>>> numbers = [1, 2, 3, 4]
>>> list(zip(letters, capitals, numbers))
[('a', 'A', 1), ('b', 'B', 2), ('c', 'C', 3), ('d', 'D', 4)]
```



## `zip()`
### Example Solved:

```python
# >>> for i in range(len(stuff)):
# ...     print('{0} had invented the {1}'.format(people[i],
# ...                                             stuff[i]))

>>> stuff = ['Electric Light', 'Microwave Oven',
             'Canned Food']
>>> people = ['Thomas Edison', 'Percy Spencer',
              'Nicolas Appert']
>>> for person, concept in zip(people, stuff):
...     print('{0} had invented the {1}'.format(person,
                                                concept))
Thomas Edison had invented the Electric Light
Percy Spencer had invented the Microwave Oven
Nicolas Appert had invented the Canned Food
```



# Loop Control Statements

> Loop super-powers!

- `break`
- `continue`
- `pass`



## Password Validation

Remember the code for password validation?

```python
passwd = raw_input('Please enter your password:')
if passwd == 'banana':
    print('Password is correct!')
```

Let's write it in a loop until the user enters the right password.



## Password validation code in a loop

```python
passwd = raw_input('Please enter your password:')
while passwd != 'banana':
    print('Password is incorrect!')
    passwd = raw_input('Please enter your password:')
```

Can we write it without code duplication?



## Breaking the loop

```python
while True:
    passwd = raw_input('Please enter your password:')
    if passwd == 'banana':
        break
    print('Password is incorrect!')
```



## Or `else`?

```python
known_names = ['Dan', 'Gil', 'Adam']
username = raw_input('What is your name?')
for name in known_names:
    if name == username:
        print('I know you!')
        break
else:
    print('I do not know you!')
```



## Let's `continue`

Implementing 7-boom

```python
i = 1
while True:
    if i % 7 == 0:
        print('Boom!')
    else:
        print(i)
    i += 1
```



## Using `continue`

```python
i = 0
while True:
    i += 1
    if i % 7 == 0:
        print('Boom!')
        continue
    print(i)
```



## `pass`
`pass` does nothing :)

```python
if num < 15:
    pass	# implement!

while name != 'Banana':
    pass	# implement!

while True:
    pass    # Busy wait for Ctrl+C
```



# Summary (1)

- `if` checks for truth value
- Most values evaluate as `True`
- Chained comparisons come handy: 20 < x < 100
- Parenthesis are not required in conditional expressions:
    - `x > 3 and y == 50`
    - (`x > 3) and (y == 50`)



# Summary (2)
- `while` and `for` are used for looping
- `range(num)` is useful for performing `num` iterations
- `break` and `continue` are loop super-powers!



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

All Rights Reserved to Amit Kotlovski
