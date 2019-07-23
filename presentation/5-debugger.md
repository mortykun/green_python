# Debugging Python Scripts

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Analyzing Crashed Programs
- Debugging Existing Script
- Using Breakpoints



# Crashed Script



## The Script

```python
def first():
    second()

def second():
    third()

first()
```



## The Crash

```
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



## Demo



# Debug Existing Script



## The Script

Simpler code, hidden bug

```python
num = 3

def compute():
    for i in range(4):
        divider = num - i
        print 1.0 / divider

compute()
```



## Running it

```
$ python crash.py
0.333333333333
0.5
1.0
Traceback (most recent call last):
  File "crash.py", line 8, in <module>
    compute()
  File "crash.py", line 6, in compute
    print 1.0 / divider
ZeroDivisionError: float division by zero
```



## Using `pdb`

To run with the `pdb` module:
```
$ python -m pdb crash.py

> crash.py(1)<module>()
-> num = 3
(Pdb)
```



## `pdb` Commands

Command     | Action
---         | ---
c           | Continue until next breakpoint / crash
pp _arg_    | Pretty print _arg_
l           | Display current region of code
q           | Quit debug mode



## Demo 2



# Using Breakpoints

- Stop the code at requested position
- Check / Change the value of variables
- Try things out (arbitrary Python code)



## Add a Breakpoint

Add the following line where you want the code to stop:

```python
import pdb; pdb.set_trace()
```



## Our Modified Script

```python
num = 3

def compute():
    for i in range(4):
        import pdb; pdb.set_trace()
        divider = num - i
        print 1.0 / divider

compute()
```



## Demo 3



## More `pdb` Commands

Command     | Action
---         | ---
c           | Continue until next breakpoint / crash
pp _arg_    | Pretty print _arg_
l           | Display current region of code
n           | Advance to next command in current stack frame
s           | Advance to next command (step into)
r           | Advance until returning to previous stack frame (step out)
w           | Print current stack (Traceback)
u           | Go up the stack
d           | Go down the stack
b _position_| Add Breakpoint. Using pdb.set_trace() is more common.
q           | Quit debug mode



# Summary

- Python Traceback stack traces are informative
- `pdb` enables powerful Step-by-step debugging
- Run python code in the context you are debugging



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
