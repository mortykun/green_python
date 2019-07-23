# Data Storage

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Opening files
- Reading from files
- Writing to files
- Random Access
- Persistent Data Structures



# Keeping Data
## Options and Motivation



## In code



## In memory



# Opening Files



## Basic Example

Opening a file for read using the 'r' flag

```python
>>> f = open('existing_file', 'r')
>>> f.read()
'nice'
>>> f.close()
```



## `read` is the default

```python
>>> f = open('existing_file', 'r')
>>> f.read()
'nice'
>>> f.close()
```

Can also be written as:

```python
>>> f = open('existing_file')
>>> f.read()
'nice'
>>> f.close()
```



## `write`

Using the 'w' flag

```python
>>> f = open('filename', 'w')       # opens the file only for write
>>> f.write('some data')
>>> f.close()

>>> s = open('filename', 'w')       # erase and open it for write again
>>> s.write('some more data')
>>> s.close()

>>> g = open('filename')
>>> g.read()
'some more data'
>>> g.close()
```



## `append`ing

Using the 'a' flag

```python
>>> f = open('filename', 'w')   # opening file for write
>>> f.write('first')
>>> f.close()

>>> g = open('filename', 'a')   # opening for append
>>> g.write(' second')          # appends to the end of the file
>>> g.close()

>>> h = open('filename')
>>> h.read()
'first second'
>>> h.close()
```



## read and write

Using the 'r+' flag

```python
>>> f = open('filename', 'r+')
>>> f.read()
'first second'
>>> f.write(' last')
>>> f.close()

>>> f = open('filename', 'r')
>>> f.read()
'first second last'
```



## Flag Summary

flag    | Meaning
--      | --
`r`     | __Read__. File must exist.
`w`     | __Write__. If exists file will be truncated.
`a`     | __Append__. If does not exist a new file is created.
`r+`    | __Update (read & write)__. File must exist.
`w+`    | __Update (read & write)__. If file exists it is truncated.
`a+`    | __Update__. All writes are done at end of file. If file does not exist, it is created.



## Opening a binary file

Add `b` to open flag

```python
>>> f = open('filename', 'rb')
```



## Make sure file is closed

```python
try:
    f = open('filename')
    # use f
finally:
    f.close()
```



## Using `with` to open files

a *context manager* for using a file and closing it automatically

```python
>>> with open('filename') as f:
...     print f.readlines()
['first line\n', 'second line\n', 'last line\n']

>>> f
<closed file 'filename', mode 'r' at 0x1042c8420>
```



# Using The File

- Read
- Write
- Standard streams
- Random Access



## Reading from a file

We will be using this file:

```markdown
first line
second line
last line
```



## Read a single line

Until a newline char (`\n`)

```python
>>> f = open('filename')
>>> f.readline()
'first line\n'
>>> f.readline()
'second line\n'
>>> f.readline()
'last line'
>>> f.readline()
''
>>> f.close()
```



## Reading number of bytes

Will read until reaching that number or EOF is reached

```python
>>> f = open('filename')
>>> f.read(3)
'fir'
>>> f.close()
```



## Reading until EOF is reached

If no number of bytes is specified

```python
>>> f = open('filename')
>>> f.read()
'first line\nsecond line\nlast line'
```



## Read and split to lines

- Read the file
- Split it into a list of lines

```python
>>> f = open('filename', 'r')
>>> f.read().splitlines()
['first line', 'second line', 'last line']
>>> f.close()
```



## Iterating a file line-by-line

Naïve solution:

```python
# f = open('filename')
for line in f.readlines():
    print line,
# f.close()
```

What is wrong with this method?

<br/>
<span class="fragment">⬇ Reads whole file before iteration starts</span>

<span class="fragment">⬇ Whole file is kept in memory</span>



## Efficiently iterate over lines in a file

Reads a line and handles it:

```python
# f = open('file')
for line in f:
    print(line)
# f.close()
```



## Write a string

```python
>>> f = open('filename', 'w')
>>> f.write('some text')
>>> f.close()
```



## Write a list of lines

```python
>>> f = open('filename', 'w')
>>> l = ['first line\n', 'second line\n']
>>> f.writelines(l)
>>> f.close()

>>> f = open('filename')
>>> f.read()
'first line\nsecond line\n'
>>> f.close()
```



## Flush recent writes

Writes are buffered for efficiency, to flush them do:

```python
>>> f = open('filename', 'w')
>>> f.write('some text')
>>> f.flush()
```



## Standard Streams

- 3 files that are always open
- Used for interacting with the user

<br/>
Example:

```python
>>> import sys
>>> sys.stdout.write('nice')    # write to standard output → screen
nice
```



## Standard Streams

file    | usage
--      | --
stdin   | input from user using keyboard
stdout  | normal output to screen
stderr  | error output to screen



## Getting user input
### Using stdin

```python
# code
sys.stdout.write('Please enter some data: ')
data = sys.stdin.read()
print 'input was:', data

# screen output
Please enter some data:
I typed this manually
^D
input was: I typed this manually
```



## Getting user input
### Better solution

```python
# code
data2 = raw_input('Please enter some data: ')
print 'input was:', data2

# output
Please enter some data: This is the data requested
input was: This is the data requested
```



## Random Access

- `tell` - Get current cursor location
- `seek` - Move cursor to specific location

```python
>>> f = open('filename', 'r')
>>> f.tell()
0
>>> f.read()
'first line\nsecond line\n'
>>> f.tell()
23
>>> f.seek(1)
1
>>> f.tell()
1
>>> f.read(4)
'irst'
>>> f.close()
```



## `seek` syntax

```python
seek(offset, [whence])
```

<br/>

whence  | seek relative to:
--      | --
0       | start of file (default)
1       | current cursor position
2       | end of file

<br/>
> <u>Python 3:</u>
> current-relative and end-relative require binary files.



# Data structures and persistency

- `pickle`
- `shelve`



## `pickle`
### Store a variable in a file for later use



## `pickle`

Usage:

```python
>>> import pickle
>>> f = open('filename', 'wb')
>>> s1 = {12, 'deeee', (23, 12, 'ttt')}
>>> pickle.dump(s1, f)
>>> f.close()

>>> f = open('filename', 'rb')
>>> s2 = pickle.load(f)

>>> s2
set([(23, 12, 'ttt'), 12, 'deeee'])
```



## `Pickle` - notes

- **Security** - Data is *not* encrypted
- **Protocol** - binary by default, optional alternative implementations (including ASCII)
- **Performance** - Use cPickle for ~1000 time faster



## `shelve`
### A persistent dictionary



## `shelve`

Usage:

```python
>>> import shelve
>>> sh = shelve.open('shelve_file')
>>> sh['fruit'] = 'banana'
>>> sh.close()

>>> shsh = shelve.open('shelve_file')
>>> print shsh['fruit']
banana
```



# Summary

- A file can be opened for reading / writing / appending / read+write
- It can be sequentially read from or written to
- Specific locations in the file can be accessed
- There are handy persistent structures for storing data



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

All Rights Reserved to Amit Kotlovski
