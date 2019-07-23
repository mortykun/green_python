# Interacting with Processes

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

[---]

# Agenda

- Simple interactions using PIPEs
- `pexpect` introduction and API
- `pexpect` examples

[---]

### Basic Interaction - using Pipes

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

### Pipes are good (for:)

- Simple scenarios
- Mainly for getting the output of non-interactive programs

[---]

### Pipes are bad (for:)

- Buffer issues - flush writes
- Apps writing too much to pipe can cause OS to deadlock
- Long or unlimited data can fill data read buffer
- Some applications bypass stdout, print to terminal (TTY) directly
- Apps built using C stdio library use big buffers for pipes

[---]

### Bypassing stdout

- Apps can read\write directly to the TTY for security reasons
- No way to redirect the stdin with the password

[---]

### C `stdio.h` PIPE handling
- 1 line buffer when reading \ writing
- Bigger buffer when redirected to a PIPE
- No way to tell application to flush

[---]

## `pexpect`

- 3'rd party module
- supports POSIX systems (on Windows try using Cygwin)
- Implements a "pseudo-terminal" for running and interacting with applications

[---]

### `pexpect` API

Command             | Description
---                 | ---
`spawn()`           | runs the child process
`expect()`          | child process continues until patterns detected
`send()`            | sends the input to child process
`sendline()`        | send() with a linefeed
`interact()`        | lets user interact with child process

[---]

### Example

```python
# shell_config.py
username = raw_input("Please enter username: ")
shell = raw_input("Enter preferred shell: ")
print "configuring user {} to use {}".format(username, shell)
```

```python
# handle_shell_config.py
import pexpect

s = pexpect.spawn('python shell_config.py', echo=False)
s.expect('Please enter username: ')
s.sendline('amit')
s.expect('Enter preferred shell: ')
s.sendline('/usr/local/bin/zsh')
s.expect(pexpect.EOF)
print s.before
```

[---]

### EOF
if pattern was not found and child process ended EOF exception is raised

[---]

### TIMEOUT
if pattern was not found during defined timout, TIMEOUT exception is raised

[---]

### `child.expect()`

Syntax:

    child.expect(pattern, timeout=30)

Examples:

    child.expect('banana')
    child.expect(['banana', 'apple', 'cherry'])
    child.expect('wal+') # will match wal and not wall

[---]

### patterns

- String
- EOF or TIMEOUT
- Compiled re
- list of previous types

[---]

### regular expression patterns
- One character lookahead, therefore
- Not greedy (match as little as possible)

[---]

### child.send()
- Writes a string to the child application
- returns the number of bytes writen

```python
send(string)
sendline(string)	# adds newline
```

[---]

### before, after
- `before` - all text up to the expected string pattern
- `after` - the text matched by the expected pattern

[---]

### Example 1 - ftp

```python
# ftp.py
import pexpect

s = pexpect.spawn('ftp ftp.openbsd.org')
s.expect('Name.*:')
s.sendline('anonymous')
s.expect('Password:')
s.sendline('steve@apple.com')
s.expect('ftp>')
s.sendline('cd /pub/OpenBSD')
s.expect('ftp>')
s.sendline('get README')
s.expect('ftp>')
s.sendline('bye')
```

[---]

### Example 2 - operations

Script:
```python
# operations.py
import random
operations = ['maintenance', 'upgrade', 'shutdown']
op = random.choice(operations)
reply = raw_input("Perform {}?\n".format(op))
if (reply in ("yes", "Yes")):
    print op + " performed"
else:
    print op + " operation canceled"
```

[---]
### Example 2 - operations (Continued)

Handler script:
```python
# perform_operation.py
import pexpect

child = pexpect.spawn("python operations.py", echo=False)
operations = ['maintenance', 'upgrade', 'shutdown']
options = ["Perform {}\?".format(op) for op in operations]
res = child.expect(options, timeout=2)
if res in [0, 1]:           # allow only maintenance or upgrade
    child.sendline("Yes")
else:
    child.sendline("No")
child.expect(pexpect.EOF)
print child.before
```

[---]

# Summary

- Simple interaction can be done with PIPEs
- `pexpect` was created for all other cases
- Easily handles simple iteractions
- Supports regular expressions, multiple options, and more
- For more information see the [full documentation](https://pexpect.readthedocs.org/en/latest/)

[---]

# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")

[---]

# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)
