# Processes

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

[---]

# Agenda

- External processes
- Threads, synchronisation and locking
- Multiprocessing and Queues

[---]

# External processes

- Origins
- `subprocess`
- Common scenarios
- Interacting with external process
- Piping processes

[---]

## Origins

- `fork()`
- `exec()`
- `spawn()`

[---]

## `subprocess`

[---]

## Common scenarios

[---]

### `subprocess.call()`

- Running an external program and waiting until it finishes
- synchronous call
- Program's returncode is returned

```python
>>> res = subprocess.call(args=['/bin/echo', 'green banana'])
green banana

>>> print res
0
```

[---]

### args=(string or arg sequence)

- Arguments passed to the call
- Can be a string or string sequence

[---]

### shell=(boolean)

- Run the command inside a shell
- Can be useful (wildcards, etc.)
- Can be dangerous if derived from user input

[---]

```python
>>> res = subprocess.call(args='exit 1', shell=True)

>>> print res
1
```

[---]

### `subprocess.check_call()`

- Run external program and monitor its returncode:
    - If 0 - return
    - Otherwise raise `CalledProcessError`

```python
>>> res = subprocess.check_call(args='exit 1', shell=True)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/Cellar/python/2.7.6_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/subprocess.py", line 540, in check_call
    raise CalledProcessError(retcode, cmd)
subprocess.CalledProcessError: Command 'exit 1' returned non-zero exit status 1
```
[---]

### `subprocess.check_output()`

- Run external program, monitor its returncode:
    - If 0 - return command's output
    - Otherwise raise `CalledProcessError`

```python
>>> res = subprocess.check_output(args=['/bin/echo',
                                        'green banana'])

>>> print res
green banana
```

[---]

## Finer control

- Previous functions were implementations for common uses
- For finer control we use the Popen class directly

[---]

### Running a process asynchronously

```python
>>> p = subprocess.Popen(args='/bin/sleep 5; echo after',
                         shell=True)
>>> print 'during'
during
after
```

[---]

### Using a PIPE for output

`communicate()` waits until the process terminates, then returns its data

```python
import subprocess
p = subprocess.Popen('echo before; /bin/sleep 10; echo after',
                     shell=True,
                     stdout=subprocess.PIPE,)
out, error = p.communicate()
for line in out.splitlines():
    print 'output: ', line
```
Output:
```python
output:  before
output:  after
```

[---]

### Using more PIPES

Full interaction with a process' input, output and error streams

```python
p = subprocess.Popen('cat -; echo "send to stderr" 1>&2',
                     shell=True,
                     stdin=subprocess.PIPE,
                     stdout=subprocess.PIPE,
                     stderr=subprocess.PIPE)
out, err = p.communicate('this is the input')
print 'out =', out
print 'err =', err
```
Output:
```json
out = this is the input
err = send to stderr
```

[---]

## Piping processes

Bash:
```bash
echo "the cat is eating" | sed s/eat/drink/
```

Python:
```python
echo = subprocess.Popen(['/bin/echo', 'the cat is eating'],
                        stdout=subprocess.PIPE)
sed = subprocess.Popen(['/usr/bin/sed', 's/eat/drink/'],
                       stdin=echo.stdout,
                       stdout=subprocess.PIPE)
echo.stdout.close() # Allow echo to receive a SIGPIPE if sed exits.
print sed.communicate()[0]
```

Output:

```xml
the cat is drinking
```

[---]

## Additional abilities

- Send a signal to a child process using `Popen.send_signal()`
- Kill a child process using `Popen.kill()`
- Wait for child process to finish using `Popen.wait()`

[---]

## Legacy code

- The `subprocess` module replaces previous methods that you can find in legacy code:
    - `os.system`
    - `os.spawn*`
    - `os.popen*`
    - `popen2.*`
    - `commands.*`

[---]

# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")

[---]

# Threads

- Motivation
- Synchronisation and locking

[---]

## Motivation

- Perform multiple activities in parallel
- Perform lengthy I/O while keeping UI reactive
- Utilize multicore machines (\*)

[---]

## Example of using threads

```python
import threading

def worker(fruit):
    print 'fruit is {}'.format(fruit)

for fruit in ['orange', 'banana', 'melon']:
    t = threading.Thread(target=worker, args=(fruit,))
    t.start()
```
Output:
```xml
fruit is orange
fruit is banana
fruit is melon
```

[---]

## Threads synchronisation

- Let's code the following story:
    - Account balance 100 NIS
    - Wife deposits 20 NIS
    - Husband withdraws 50 NIS
    - They do it simultaneously

[---]

## Threads synchronisation (1)

What's wrong with this implementation?

```python
import logging
import threading

class Account(object):
    def __init__(self, balance):
        self._balance = balance

    def set_balance(self, num):
        self._balance = num

    def get_balance(self):
        return self._balance
```

[---]

## Threads synchronisation (2)

```python
def deposit(num):
    balance = account.get_balance()
    logging.info('deposit: balance before is ' + balance)
    account.set_balance(balance + num)
    logging.info('deposit: balance after is '
                 + account.get_balance())

def withdraw(num):
    balance = account.get_balance()
    logging.info('withdraw: balance before is ' + balance)
    account.set_balance(balance - num)
    logging.info('withdraw: balance after is '
                 + account.get_balance())
```

[---]

## Threads synchronisation (3)

```python
logging.basicConfig(level=logging.DEBUG)

account = Account(100)
t1 = threading.Thread(target=deposit, args=(20,))
t2 = threading.Thread(target=withdraw, args=(50,))

t1.start()
t2.start()
```

Output:
```xml
INFO:root:deposit: balance before is 100
INFO:root:withdraw: balance before is 100
INFO:root:deposit: balance after is 120
INFO:root:withdraw: balance after is 50
```

[---]

### The problem

Some resources require synchronisation

[---]

### The soulution

Locking those resources

[---]

## Locking

`threading.Lock` can be acquired and released

[---]

```python
import logging
import threading

class Account(object):
    def __init__(self, balance):
        self._balance = balance
        self.lock = threading.Lock()

    def set_balance(self, num):
        self._balance = num

    def get_balance(self):
        return self._balance
```

[---]

```python
def deposit(num):
    account.lock.acquire()
    balance = account.get_balance()
    logging.info('deposit: balance before is ' + balance)
    account.set_balance(balance + num)
    logging.info('deposit: balance after is '
                 + account.get_balance())
    account.lock.release()
```
```python
def withdraw(num):
    account.lock.acquire()
    balance = account.get_balance()
    logging.info('withdraw: balance before is ' + balanace
    account.set_balance(balance - num)
    logging.info('withdraw: balance after is '
                 + account.get_balance())
    account.lock.release()
```

[---]

```python
logging.basicConfig(level=logging.DEBUG)

account = Account(100)
t1 = threading.Thread(target=deposit, args=(20,))
t2 = threading.Thread(target=withdraw, args=(50,))

t1.start()
t2.start()
```
Output:
```xml
INFO:root:deposit: balance before is 100
INFO:root:deposit: balance after is 120
INFO:root:withdraw: balance before is 120
INFO:root:withdraw: balance after is 70
```

[---]

## Global Interpreter Lock (GIL)

- CPython always uses this hidden lock
- The threads don't actually run simultaneously
- Multiple threads are great for lengthy I/O operations
- For CPU-heavy calculations use `multiprocessing` instead

[---]

# `multiprocessing`

[---]

- `multiprocessing` runs worker tasks on multiple processes
- Similar interface as `threading`

[---]

### Reminder

As the source will be loaded into a process, always protect the "main":

```python
if __name__ == "__main__":
    ...
    ...
```

[---]

## Problematic code

```python
import threading

def worker(num):
    print "worker number {}".format(num)

if __name__ == "__main__":
    for i in range(5):
        t = threading.Thread(target=worker, args=(i,))
        t.start()
```

[---]

## Output

```xml
worker number 0worker number 1
 worker number 4

worker number 2worker number 3
```

[---]

## Using `multiprocessing`

```python
import multiprocessing

def worker(num):
    print "worker number {}".format(num)

if __name__ == "__main__":
    for i in range(5):
        p = multiprocessing.Process(target=worker, args=(i,))
        p.start()
```

```xml
worker number 0
worker number 1
worker number 2
worker number 3
worker number 4
```

[---]

# `Queue`

[---]

- `queue.Queue` - for multiple threads
- `multiprocessing.Queue` - for multiple processes

[---]

```python
import multiprocessing
import time
import random

if __name__ == "__main__":
    fruits = multiprocessing.Queue()
    for _ in range(2):
        mp = multiprocessing.Process(target=worker, args=(fruits,))
        mp.start()

    for fruit in ['apple', 'strawberry', 'lemon', 'banana']:
        fruits.put(fruit)

    for _ in range(2): fruits.put('DONE')
```

[---]

```python
def worker(queue):
    process = multiprocessing.current_process().name
    while True:
        task = queue.get()
        if task == 'DONE':
            print('{}: DONE'.format(process))
            break
        print('{}: {} - start'.format(process, task))
        sleep_time = round(random.uniform(0, 1), 3)
        time.sleep(sleep_time)
        print('{}: {} - end ({} seconds)'.format(
                process, task, sleep_time))
```

```
Process-1: apple - start
Process-2: strawberry - start
Process-2: strawberry - end (0.012 seconds)
Process-2: lemon - start
Process-1: apple - end (0.021 seconds)
Process-1: banana - start
Process-2: lemon - end (0.051 seconds)
Process-2: DONE
Process-1: banana - end (0.15 seconds)
Process-1: DONE
```

[---]

# Summary

- `subprocess` allows running external tasks
- `subprocess` replaces older modules, exposes various API
- Threads share the same memory, needs to be synchronised
- `Queue.Queue` and `subprocess.Queue` can communicate tasks

[---]

# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")

[---]

# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
