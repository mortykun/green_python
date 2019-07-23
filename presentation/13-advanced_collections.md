# Advanced Collections

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Using map, filter and reduce
- List Comprehension
- Copying Collections - shallow and deep



## Using `map`

Runs a function on each item in a sequence

```python
>>> l = [1, 2, 3, 4]

>>> def times6(x):
...     return x*6
...
>>> map(times6, l)
[6, 12, 18, 24]

>>> map(lambda x: x**2, l)
[1, 4, 9, 16]
```



## Using `filter`

Runs a function over every item in a sequence and keep the item only if the
function evaluates to `True`

```python
>>> l = [-2, -1, 0, 1, 2]

>>> def negative(x):
...     return x < 0
...
>>> filter(negative, l)
[-2, -1]

>>> filter(lambda x: x % 2 == 0, l)
[-2, 0, 2]
```



## Using `reduce`

Returns a single value constructed by calling the binary function function on
the first two items of the sequence, then on the result and the next item, and
so on

```python
>>> l = [1, 2, 3, 4, 5]

>>> def mult(x, y):
...     return x * y
...
>>> reduce(mult, l)
120

>>> reduce(lambda x, y: x+y, l)
15
```



## List Comprehension

An elegant and concise way to create lists using map and filter concepts

```python
>>> l = [1, 2, 3, 4, 5]
>>> [x**2 for x in l]
[1, 4, 9, 16, 25]

>>> [x**2 for x in l if x % 2 == 0]
[4, 16]
```
```python
>>> l2 = [-2, -1, 0, 1, 2]
>>> [x for x in l2 if x < 0]
[-2, -1]
```



## Copying Collections

The problem:

```python
>>> list1 = [1, 2, 3, 4]

>>> list2 = list1

>>> list2[0] = 500

>>> print list2
[500, 2, 3, 4]

>>> print list1
[500, 2, 3, 4]
```



## This is called shallow copy

- Only the reference to the list is copied
- Changes to the "new" list affect the "old" list



## Suggested Solution

Duplicate the list items

```python
>>> list1 = [1, 2, 3, 4]

>>> list2 = list1[:]

>>> list2[0] = 500

>>> print list2
[500, 2, 3, 4]

>>> print list1
[1, 2, 3, 4]
```



## The Problem (2)

But the problem is not solved yet:

```python
>>> >>> list1 = [[1, 1, 1], 2, 3, 4]

>>> list2 = list1[:]

>>> list2[0][1] = 500

>>> print list2
[[1, 500, 1], 2, 3, 4]

>>> print list1
[[1, 500, 1], 2, 3, 4]
```



## The Actual Problem

- Our style of copying the list is called shallow copy
- It copies references to list items
- We want to go deep and duplicate the actual list items



## `copy.deepcopy()`

```python
>>> import copy

>>> list1 = [[1, 1, 1], 2, 3, 4]

>>> list2 = copy.deepcopy(list1)

>>> list2[0][1] = 500

>>> print list2
[[1, 500, 1], 2, 3, 4]

>>> print list1
[[1, 1, 1], 2, 3, 4]
```



## `copy.deepcopy()`

How does it work?



# Summary

- `map`, `filter` and `reduce` are powerful functional programming tools
- List Comprehensions are elegant replacements for `map` and `filter`
- To really copy data structures use `copy.deepcopy()`



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
