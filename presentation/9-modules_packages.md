# Modules and Packages

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda
- What is a Module
- Loading a Module
- Running a module as a script
- What is a Package
- Loading a Module from a Package
- Selected Modules Overview



## What is a Module

- Every python file is a module



## Loading a Module

example.py:

```python
def mult(first, second):
    return first * second
def welcome(name):
    return 'Have a wonderful day, {}!'.format(name)
```

Using the interpreter:

```python
>>> import example
>>> print example.mult(3, 5)
15
```



## Importing into our namespace

We can import a function or class directly into our namespace

```python
>>> from example import mult, welcome
>>> mult(20, 6)
120
>>> welcome('Griselda')
'Have a wonderful day, Griselda!'
```



## Importing everything from a module

We can import everything(\*) from a module directly into our namespace

```python
>>> from example import *
>>> mult(4, 21)
42
>>> welcome('Kilikina')
'Have a wonderful day, Kilikina!'
```

(\*) except items whose name start with \_ , e.g. \_hidden()



## `from foo import *`

- ↑ Easy to type the import line (imports everything)
- ↑ Easier to use (no need to type the namespace)
- ⬇ Imported names and local ones might clash



## Running a module as a script

We add these lines to modules:
```python
if __name__ == '__main__':
    # main code of the script
```

What does it do?



## What's in a `__name__`?

- A variable that is defined in each module
- Usually holds the module's name (e.g., `'example'`)
- When the module is being run as a script, `__name__` holds the string `'__main__'`
- Unless we use this condition, our main code will run whenever the module is imported



## What is a Package

- A directory containing modules



## The Package structure

The package directory structure:

```
furniture/
        __init__.py
		tables/
				__init__.py
				livingroomtable.py
				coffeetable.py
		chairs/
				__init__.py
				armchair.py
				electricchair.py
```



## Loading a module from a Package

```python
import furniture.tables.livingroomtable
print furniture.tables.livingroomtable.get_height()
```
avoid using the package namespace:

```python
from furniture.tables import livingroomtable
print livingroomtable.get_height()
```
if we only need a single method:

```python
from furniture.tables.livingroomtable import get_height
print get_height()
```



## `import *` with Packages

Imports everything listed under the variable `__all__` defined in the file
\_\_init\_\_.py at the root of the directory

<br/>
furniture/tables/\_\_init\_\_.py:

```python
__all__ = ["livingroomtable", "coffee"]
```
```python
>>> from furniture.tables import *
```



## Module Search Path - `sys.path`

Initialized with:

- Directory containing the python script
- `PYTHONPATH` Environemnt variable
- Installation dependant default



# Selected Modules Overview



## PySerial

- Easy Python code access to devices connected to the Serial port
- Supports a multitude of configurations including different byte sizes, stop
  bits, parity etc.
- Allows accessing the device like a file
- 3'rd Party module



## PySerial - Common Usage

Accessing a device using 9600 baud, 8-N-1 (8 data bits, No parity bit and 1
stop bit)

```python
>>> import serial
>>> ser = serial.Serial(0)  # open first serial port
>>> print ser.name          # check which port was really used
>>> ser.write("hello")      # write a string
>>> ser.close()             # close port
```



## PySerial - Common Usage (2)

Accessing a device on port 1 using 19200 baud, 8-N-1 with a timeout of 1 second

```python
>>> ser = serial.Serial('/dev/ttyS1', 19200, timeout=1)
>>> x = ser.read()          # read one byte
>>> s = ser.read(10)        # read up to ten bytes (timeout)
>>> line = ser.readline()   # read a '\n' terminated line
>>> ser.close()

```



## `subprocess`

- Used for spawning new processes and shell commands
- Allows passing and receiving data from the child processes' pipes
- A part of the standard library



## Common Usage

Run a command with argument, wait for it to end and receive the return code:

```python
>>> res = subprocess.call(['which', 'sh'])
/bin/sh
>>> res
0
```

Run the command, wait for it to end and return its output as a byte string:

```python
>>> res = subprocess.check_output(['which', 'sh'])
>>> res
'/bin/sh\n'
```



## More `subprocess` usage
Run the command using the shell, _not_ waiting for it, but rather returning
pipes to all three streams:

```python
p = Popen("cmd", shell=True, bufsize=bufsize,
          stdin=PIPE, stdout=PIPE, stderr=PIPE, close_fds=True)
(child_stdin,
 child_stdout,
 child_stderr) = (p.stdin, p.stdout, p.stderr)
```



## `argparse`

- Builds a parser for command line argument
- API for mandatory and optional arguments
- Builds usage and help messages
- Part of the Python standard library
- Before Python 2.7 use `optparse`



## Parameters from command line

```python
#!/usr/bin/env python
# commandline_args.py - a script for printing the command line arguments

import sys
print(sys.argv)
```

```
$ python commandline_args.py 1 2 last
['commandline_args.py', '1', '2', 'last']
```



## Real example using `sys.argv`

```python
#!/usr/bin/env python
# argparse_example_commandline_glob.py - print number of file names from given path
from glob import glob
import sys

path = sys.argv[1]              # mandatory parm
number_of_files = None          # optional param with default value
if len(sys.argv) > 2:
    number_of_files = int(sys.argv[2])

files = glob(path)[:number_of_files]
print(files)
```

```
$ python argparse_example_commandline_glob.py "/var/*" 5
['/var/agentx', '/var/at', '/var/audit', '/var/backups', '/var/db']
```



## `sys.argv`

- Command line arguments as a list
- First argument is script file name
- No "mandatory" arguments
- No type validation
- No usage or help text
- Requires extra "boilerplate" coding



## Basic `argparse` usage

```python
#!/usr/bin/env python
# argparse_example_basic_argparse_1.py - print files from given path
from argparse import ArgumentParser
from glob import glob

parser = ArgumentParser()
parser.add_argument("path", help="Path of files including wildcards")
parser.add_argument("-n", "--number", type=int, default=None,
                    help="Optional number of files to print")
args = parser.parse_args()

files = glob(args.path)[:args.number]
print(files)
```
```
$ python argparse_example_basic_argparse_1.py "/var/*" --number 5
['/var/agentx', '/var/at', '/var/audit', '/var/backups', '/var/db']
```



## Usage

```markdown
$ python argparse_example_basic_argparse_1.py --help
usage: argparse_example_basic_argparse_1.py [-h] [-n NUMBER] path

positional arguments:
  path                  Path to find files, including wildcards

optional arguments:
  -h, --help            show this help message and exit
  -n NUMBER, --number NUMBER
                        Optional number of files to print
```



## Validation

Number of arguments:
```markdown
$ python argparse_example_basic_argparse_1.py
usage: argparse_example_basic_argparse_1.py [-h] [-n NUMBER] path
argparse_example_basic_argparse_1.py: error: too few arguments
```

Type of second argument:
```markdown
$ python argparse_example_basic_argparse_1.py "/var/*" --number "apple"
usage: argparse_example_basic_argparse_1.py [-h] [-n NUMBER] path
argparse_example_basic_argparse_1.py:
	error: argument -n/--number: invalid int value: 'apple'
```



## More advanced uses
[`argparse` documentation on the Python.org website](https://docs.python.org/2/library/argparse.html#module-argparse)



# Summary

- Every Python file is a module
- A Package is a directory of Python files with an \_\_init\_\_.py file in it
- Modules are imported with their (optional) namespace
- Python has many built in and 3'rd Party libraries for solving common problems



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Appendix - Installing 3'rd Party Modules



## The Promise

Over 62K packages solving many needs and usecases (more than once...)



## Problems

- Complicated installers
- Limited OS permissions, cannot install
- Projects needs different external packages, also
- Projects need different versions of the same packages



## A Solution

Download 3'rd party packages and keep them inside local package



## Another Solution

`pip` + `virtualenv`



## `pip`

- Tool for installing Python packages from the [Python Package Index](http://pypi.python.org/)
- Replaces a more primitive installer called `easy_install`



### Installing `pip`

```sh
sudo easy_install pip
```



### Installing Packages Globaly
####(Can be a bad idea!)

```sh
$ pip install serial

# Or, if normal user doesn't have write permissions:
$ sudo pip install serial
```



## `virtualenv`

- Allows setting up a "sandbox" with:
	- Specific Python interpreter
	- Specific packages installed



### Installing `virtualenv`

If you have `pip` installed:

```sh
$ pip install virtualenv

# Or, if normal user doesn't have write permissions:
$ sudo pip install virtualenv
```



### Creating a new virtual environment

```sh
$ cd python_project
$ virtualenv venv
New python executable in venv/bin/python2.7
Also creating executable in venv/bin/python
Installing setuptools, pip...done.
```



### Manually calling `python` and `pip`

Sandboxed `python` and `pip` can be called manually:

```sh
$ cd python_project
$ ls venv
bin     include lib

$ venv/bin/python -c "print 'hello'"
hello
```



### Using the virtual environment

Setting the environment so that `python` and `pip` call the sandboxed versions:

```sh
$ which python
/usr/local/bin/python
$ cd python_project
$ . venv/bin/activate
$ which python
/private/tmp/python_project/venv/bin/python

# do a lot of work in virtual environment, then
$ deactivate
$ which python
/usr/local/bin/python
```



### Package Installation inside virtual environment

```sh
$ python -c 'import serial; print serial'
<module 'serial' from '/usr/local/lib/python2.7/site-packages/serial/__init__.pyc'>
$ . venv/bin/activate
$ pip install pyserial
Downloading/unpacking pyserial
  Downloading pyserial-2.7.tar.gz (122kB): 122kB downloaded

...

Successfully installed pyserial
Cleaning up...
$ python -c 'import serial; print serial'
<module 'serial' from '/private/tmp/python_project/venv/lib/python2.7/site-packages/serial/__init__.pyc'>
$ deactivate
$ python -c 'import serial; print serial'
<module 'serial' from '/usr/local/lib/python2.7/site-packages/serial/__init__.pyc'>
```



### Requirements file
Creating it:

```sh
$ pip freeze > requirements.txt
$ cat requirements.txt
lxml==3.4.4
pyserial==2.7
wsgiref==0.1.2
```

Using is:
```sh
pip install -r requirements.txt
```



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
