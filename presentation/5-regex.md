# Regular Expressions

<br/>
[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)



# Agenda

- Use case
- Origins
- Syntax Tools
- Searching Methods
- Modifying Methods
- `String`'s simple methods



# What for?

Let's look for `'cat'` in a given string:
```python
>>> s = 'the white cat is sleeping'
>>> 'cat' in s
True
```



## More complex

Now, let's look for either `'cat'` or `'kitten'`:
```python
>>> s = 'the white kitten is sleeping'
>>> 'cat' in s or 'kitten' in s
True
```

<span class="fragment">
What if we wanted `'cat'`, `'kitten'`, `'puppy'` or `'pup'`?
</span>

<br/>

<span class="fragment">
Too long, hard to read, hard to maintain, not efficient
</span>



## Meet `re`

```python
>>> s = 'the white kitten is sleeping'
>>> import re
>>> re.search(r'cat|kitten', s).span()
(10, 16)
>>> re.search(r'dog|puppy', s)
None
```



# Origins

![ Kleen ](img/Kleene.jpg "Mathematisches Forschungsinstitut Oberwolfach,http://owpdb.mfo.de/detail?photo_id=2122")
![ Ken ](img/ken.jpg "http://www.catb.org/~esr/jargon/html/U/Unix.html")

- __Stephen Cole__, 1950s - _regular sets_
- __Ken Thompson__  - _QED_ and _ed_,
  influencing _AWK_, _Emacs_ and _vi_
- __Henry Spencer__'s regex library influenced _Perl_
- Other languages influenced, amongst them Python
- Added to industry standards such as ISO SGML



# Syntax Tools



## Raw Strings and Regex

- '\' has special meaning in strings ('\n' for newline, etc.)
- To avoid special meanings in regex, use the raw string by prefixing with
  `r`:

```python
>>> print('first\nsecond')      # regular string
first
second

>>> print(r'first\nsecond')     # raw string
first\nsecond
```



## Simple string

Let's search for the string `kit`:

```python
>>> re.search(r'kit', 'nice kitten').span()
(5, 8)

>>> re.search(r'kit', 'nice banana')
None
```



## `'.'`

`'c'`, followed by any single char, followed by `'t'`:

<span class="fragment">
`c.t`
</span>



## `'.'`

`'c'`, followed by any single char, followed by `'t'`:

input   | pattern   | result
--      | --        | --
'cat'   | c.t       | <span class="fragment">✔</span>
'cot'   | c.t       | <span class="fragment">✔</span>
'coat'  | c.t       | <span class="fragment">✘</span>
'bat'   | c.t       | <span class="fragment">✘</span>



## `'.'`
### Any Single Char

`'c'`, followed by any single char, followed by `'t'`:

```python
>>> re.search(r'c.t', 'I found a cat').span()
(10, 13)

>>> re.search(r'c.t', 'I found a coat')
None
```



## `'^'`

`book` at the start of the string:

<span class="fragment">
`^book`
</span>



## `'^'`

`book` at the start of the string:

input               | pattern   | result
--                  | --        | --
'book of job'       | ^book     | <span class="fragment">✔</span>
'bookof job'        | ^book     | <span class="fragment">✔</span>
' book of job'      | ^book     | <span class="fragment">✘</span>
'the book of job'   | ^book     | <span class="fragment">✘</span>



## `'^'`
### Start of String

`book` at the start of the string:

```python
>>> re.search(r'^book', 'book of job').span()
(0, 4)

>>> re.search(r'^book', ' book of job')
None
```



## `'$'`

`duck` at the end of the string:

<span class="fragment">
`duck$`
</span>



## `'$'`

`duck` at the end of the string:

input       | pattern   | result
--          | --        | --
'duck'      | duck$     | <span class="fragment">✔</span>
'a duck'    | duck$     | <span class="fragment">✔</span>
'ducks'     | duck$     | <span class="fragment">✘</span>



## `'$'`
### End of String

`duck` at the end of the string:

```python
>>> re.search(r'duck$', 'a duck').span()
(2, 6)
>>> re.search(r'duck$', 'many ducks')
None
```



## `'*'`

`'d'` followed by zero or more `'u'` chars, then `'ck'`

<span class="fragment">
`du*ck`
</span>



## `'*'`

`'d'` followed by zero or more `'u'` chars, then `'ck'`

input       | pattern   | result
--          | --        | --
'duck'      | du*ck     | <span class="fragment">✔</span>
'dck'       | du*ck     | <span class="fragment">✔</span>
'duuuuuck'  | du*ck     | <span class="fragment">✔</span>
'douck'     | du*ck     | <span class="fragment">✘</span>



## `'*'`
### 0 or more repetitions of previous RE

`'d'` followed by zero or more `'u'` chars, then `'ck'`

```python
>>> re.search(r'du*ck', 'duck').span()
(0, 4)
>>> re.search(r'du*ck', 'dck').span()
(0, 3)
>>> re.search(r'du*ck', 'duuuuck').span()
(0, 7)
>>> re.search(r'du*ck', 'douck')
None
```

`'d'`, any number of __any__ char, then `'ck'`:

```python
>>> re.search(r'd.*ck', 'duck').span()
(0, 4)
>>> re.search(r'd.*ck', 'donald duck').span()
(0, 11)
```



## `'+'`

`'d'`, one or more `'o'` chars, then `'g':`

<span class="fragment">
`do+g`
</span>



## `'+'`

`'d'`, one or more `'o'` chars, then `'g':`

input       | pattern   | result
--          | --        | --
'dog'       | do+g      | <span class="fragment">✔</span>
'dooog'     | do+g      | <span class="fragment">✔</span>
'dig'       | do+g      | <span class="fragment">✘</span>
'dg'        | do+g      | <span class="fragment">✘</span>



## `'+'`
### 1 or more repetitions of previous RE

`'d'`, one or more `'o'` chars, then `'g':`

```python
>>> re.search(r'do+g', 'dog').span()
(0, 3)
>>> re.search(r'do+g', 'doooooog').span()
(0, 8)
>>> re.search(r'do+g', 'dg')
None
```



## `'?'`

Supporting British spelling - `'color'` or `'colour'` (`u` is optional)

<span class="fragment">
`colou?r`
</span>



## `'?'`

Supporting British spelling - `'color'` or `'colour'` (`u` is optional)

input       | pattern   | result
--          | --        | --
'color'     | colou?r   | <span class="fragment">✔</span>
'colour'    | colou?r   | <span class="fragment">✔</span>
'colouor'   | colou?r   | <span class="fragment">✘</span>



## `'?'`
### 0 or 1 repetitions of previous RE

Supporting British spelling - `'color'` or `'colour'` (`u` is optional)

```python
>>> re.search(r'colou?r', 'color').span()
(0, 5)
>>> re.search(r'colou?r', 'colour').span()
(0, 6)
>>> re.search(r'colou?r', 'colouor')
None
```



## {m}

The word `'good'` with 5 `o`'s:

<span class="fragment">
`go{5}d`
</span>



## {m}

The word `'good'` with 5 `o`'s:

input       | pattern   | result
--          | --        | --
'goooood'   | go{5}d    | <span class="fragment">✔</span>
'gooooood'  | go{5}d    | <span class="fragment">✘</span>
'good'      | go{5}d    | <span class="fragment">✘</span>



## {m}
### m copies of previous RE should be matched

The word `'good'` with 5 `o`'s:

```python
>>> re.search(r'go{5}d', 'goooood').span()
(0, 7)
>>> re.search(r'go{5}d', 'gooooood')
None
>>> re.search(r'go{5}d', 'good')
None
```



## {m,n}

The word good with 3-5 o's:

<span class="fragment">
`go{3,5}d`
</span>



## {m,n}

The word good with 3-5 o's:

input       | pattern   | result
--          | --        | --
'goood'     | go{3,5}d  | <span class="fragment">✔</span>
'goooood'   | go{3,5}d  | <span class="fragment">✔</span>
'good'      | go{3,5}d  | <span class="fragment">✘</span>
'gooooood'  | go{3,5}d  | <span class="fragment">✘</span>



## {m,n}
### m to n repetitions of previous RE

The word good with 3-5 o's:

```python
>>> re.search(r'go{3,5}d', 'goooood').span()
(0, 7)
>>> re.search(r'go{3,5}d', 'goood').span()
(0, 5)
>>> re.search(r'go{3,5}d', 'good')
None
>>> re.search(r'go{3,5}d', 'gooooood')
None
```



## '\\'

The string 'star\*' with the actual char `*`:

<span class="fragment">
`star\*`
</span>



## '\\'

The string 'star\*' with the actual char `*`:

input       | pattern    | result
--          | --         | --
'star*'     | star\\*    | <span class="fragment">✔</span>
'star\\*'   | star\\*    | <span class="fragment">✘</span>
'star'      | star\\*    | <span class="fragment">✘</span>
'starr'     | star\\*    | <span class="fragment">✘</span>
'stark'     | star\\*    | <span class="fragment">✘</span>



## '\\'
### Escapes special char following ('*', '?', etc.)

The string 'star\*' with the actual char `*`:

```python
>>> re.search(r'star\*', 'star*').span()
(0, 5)
>>> re.search(r'star\*', 'star\*')
None
>>> re.search(r'star\*', 'star')
None
>>> re.search(r'star\*', 'starr')
None
>>> re.search(r'star\*', 'stark')
None
```



## [abc]

The word 'mark' with 'm' or 'M':

<span class="fragment">
`[Mm]ark`
</span>



## [abc]

The word 'mark' with 'm' or 'M':

input       | pattern    | result
--          | --         | --
'mark'      | [Mm]ark    | <span class="fragment">✔</span>
'Mark'      | [Mm]ark    | <span class="fragment">✔</span>
'Dark'      | [Mm]ark    | <span class="fragment">✘</span>
'ark'       | [Mm]ark    | <span class="fragment">✘</span>



## [abc]
### 'a', 'b' or 'c'

The word 'mark' with 'm' or 'M':

```python
>>> re.search(r'[Mm]ark', 'mark').span()
(0, 4)
>>> re.search(r'[Mm]ark', 'Mark').span()
(0, 4)
>>> re.search(r'[Mm]ark', 'Dark')
None
>>> re.search(r'[Mm]ark', 'ark')
None
```



## [a-z]

Single capital letter or digit, followed by 'ark':

<span class="fragment">
`[A-Z0-9]ark`
</span>



## [a-z]

Single capital letter or digit, followed by 'ark':

input       | pattern       | result
--          | --            | --
'Mark'      | [A-Z0-9]ark   | <span class="fragment">✔</span>
'Dark'      | [A-Z0-9]ark   | <span class="fragment">✔</span>
'9ark'      | [A-Z0-9]ark   | <span class="fragment">✔</span>
'mark'      | [A-Z0-9]ark   | <span class="fragment">✘</span>
'ark'       | [A-Z0-9]ark   | <span class="fragment">✘</span>



## [a-z]
### range, a char between `a` and `z`

Single capital letter or digit, followed by 'ark':

```python
>>> re.search(r'[A-Z0-9]ark', 'Mark').span()
(0, 4)
>>> re.search(r'[A-Z0-9]ark', 'Dark').span()
(0, 4)
>>> re.search(r'[A-Z0-9]ark', '9ark').span()
(0, 4)
>>> re.search(r'[A-Z0-9]ark', 'ark')
None
```



## [^abc]

Character different from `M` or `m` followed by 'ark':

<span class="fragment">
`[^Mm]ark`
</span>



## [^abc]

Character different from `M` or `m` followed by 'ark':

input       | pattern    | result
--          | --         | --
'Dark'      | [^Mm]ark   | <span class="fragment">✔</span>
'Bark'      | [^Mm]ark   | <span class="fragment">✔</span>
'9ark'      | [^Mm]ark   | <span class="fragment">✔</span>
'Mark'      | [^Mm]ark   | <span class="fragment">✘</span>
'mark'      | [^Mm]ark   | <span class="fragment">✘</span>
'ark'       | [^Mm]ark   | <span class="fragment">✘</span>



## [^abc]
### Characters complementing the set (all but 'a', 'b' or 'c')

Character different from `M` or `m` followed by 'ark':

```python
>>> re.search(r'[^Mm]ark', 'Dark').span()
(0, 4)
>>> re.search(r'[^Mm]ark', '9ark').span()
(0, 4)
>>> re.search(r'[^Mm]ark', 'Mark')
None
>>> re.search(r'[^Mm]ark', 'mark')
None
>>> re.search(r'[^Mm]ark', 'ark')
None
```



## '|'

Either 'cat' or 'dog':

<span class="fragment">
`cat|dog`
</span>



## '|'

Either 'cat' or 'dog':

input       | pattern       | result
--          | --            | --
'cat'       | cat&#124;dog  | <span class="fragment">✔</span>
'dog'       | cat&#124;dog  | <span class="fragment">✔</span>
'banana'    | cat&#124;dog  | <span class="fragment">✘</span>



## '|'
### A|B - RE A or B

Either 'cat' or 'dog':

```python
>>> re.search(r'cat|dog', 'cat').span()
(0, 3)
>>> re.search(r'cat|dog', 'dog').span()
(0, 3)
>>> re.search(r'cat|dog', 'banana')
None
```



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



## Character Groups

For our convenience, certain character groups have been prepared.

Here we detect digits and non-digits:

```python
# digits followed by non-digits
>>> re.search(r'[0-9]+ [^0-9]+', '357 Cat~s#').span()
(0, 10)

# \d - digit, \D - non-Digit
>>> re.search(r'\d+ \D+', '357 Cat~s#').span()
(0, 10)
```



## Character Groups (2)

Detecting whitespace:

```python
>>> print('come  \n  here')
come
  here
>>> print(re.search(r'come[ \n\t]+here',
                    'come \n\t   here').span())
(0, 14)

# \s - whitespace
>>> print(re.search(r'come\s+here',
                    'come   \n   here').span())
(0, 15)
```



## Character Groups - List

Symbol  | Meaning
--      | --
\d      | Decimal digits
\D      | NOT a decimal digit
\s      | Whitespace - [ \t\n\r\f\v] and more if unicode
\S      | NOT whitespace
\w      | Word character [a-zA-Z0-9\_] and more if unicode
\W      | NOT word character



## Search Flags
### Flags provide search options

Ignore case:

```python
>>> re.search(r'cats', 'cats').span()
(0, 4)
>>> re.search(r'cats', 'CATS')
None
>>> re.search(r'cats', 'cats', re.IGNORECASE).span()
(0, 4)
>>> re.search(r'cats', 'CATS', re.IGNORECASE).span()
(0, 4)
```



## Search Flags (2)

Regular expressions can get complicated. We can space and comment them:

```python
>>> re.search(r'\d{3,5} \D+', '2673 bananas').span()
(0, 12)

# can also be written:
>>> exp = r'''\d{3,5}   # number
...           \         # space char
...           \D+       # word'''
>>> re.search(exp, '2673 bananas', re.VERBOSE).span()
(0, 12)
```



## Search Flags - List

Flag            | Meaning
--              | --
DOTALL, S       | '.' to also match newlines
IGNORECASE, I   | Case-insensitive matches
LOCALE, L       | Locale-aware match
MULTILINE, M    | Multi-line matching, affects ^ and $
VERBOSE, X      | Allow whitespace and comments
ASCII, A        | Make escapes (\w, \b, \s, \d)



## Grouping

Detecting strings like these:

- `a a`
- `apple apple`
- `eat a sweet sweet fruit`

A _unit_ that appears twice separated by a single space

<span class="fragment">
`(.+) \1`
</span>



## Grouping (2)

Part    | Meaning
--      | --
`(.+)`  | Defining group 1
`\1`    | Expecting group 1

```python
>>> m = re.search(r'(.+) \1', 'apple apple')
>>> m.span()
(0, 11)
>>> m.group(1)
'apple'

>>> m = re.search(r'(.+) \1', 'eat a sweet sweet apple')
>>> m.span()
(6, 17)
>>> m.group(1)
'sweet'
```



## Grouping (3)

Using multiple groups:

```python
>>> m = re.search(r'(.+) and (.+): \2, \1',
                  'plum and orange: orange, plum')
>>> m.span()
(0, 29)
>>> m.group(1)
'plum'
>>> m.group(2)
'orange'
```



# Searching Methods

- `search()`
- `match()`
- `findall()`
- `finditer()`



## `search()`

What we've been using so far

```python
>>> m = re.search(r'ba', 'big banana')
>>> m.span()
(4, 6)
```



## `match()`

Pattern must match from the beginning of the string

```python
>>> re.match(r'ba', 'banana').span()
(0, 2)
>>> re.match(r'ba', 'big banana')
None
```



## `findall()`

Prepare a list of all the non-overlapping matches

```python
>>> re.findall(r'fun\w*', 'having fun with the funny cat')
['fun', 'funny']
```



## `finditer()`

Iterating over all the non-overlapping matches

```python
>>> for m in re.finditer(r'fun\w*',
                         'having fun with the funny cat'):
...     print(m.span())
...
(7, 10)
(20, 25)
```



# Modifying Methods

- `split()`
- `sub()` and `subn()`



## `split()`

Splitting the string using a regular expression:

```python
>>> re.split(r'[?_-]{2,}',
             'small__large???cat-dog----banana')
['small', 'large', 'cat-dog', 'banana']
```



## `sub()` and `subn()`

Both substitute part of the string using a regex. `subn()` also returns how many
such substitutions it did:

```python
>>> s = 'the small kid ate the smaller cake'
>>> re.sub(r'small\w*', 'big', s)
'the big kid ate the big cake'

>>> re.subn(r'small\w*', 'big', s)
('the big kid ate the big cake', 2)
```



## `re.sub()`
### Replacing using a function

```python
def change(match):
    word = match.group()
    if word == 'banana':
        return 'fruit'
    return word
```
```python
>>> s = 'eat a tasty banana each day'

>>> re.sub(r'\w+', change, s)
'eat a tasty fruit each day'

>>> re.sub(r'\w+',
           lambda match: 'fruit' if match.group() == 'banana'
                         else match.group(),
           s)
'eat a tasty fruit each day'
```



## `re.sub()`
### Replacing using a function

```python
def increase(match):
    num = int(match.group())
    return str(num + 1)
```
```python
>>> s = "eat at least 2 bananas each day"

>>> re.sub(r'\d+', increase, s)
'eat at least 3 bananas each day'

>>> re.sub(r'\d+', lambda match: str(int(match.group()) + 1), s)
'eat at least 3 bananas each day'
```



# Compiled Patterns

- Python compiles a Pattern object for every search
- When using a pattern more than once - better create the Pattern
  ourselves

```python
>>> re.findall(r'[abc]{3}', 'aa bbb ccccc d')
['bbb', 'ccc']

>>> p = re.compile(r'[abc]{3}')
>>> p.findall('aa bbb ccccc d')
['bbb', 'ccc']
```



## `String`'s simple methods

- For simple cases always prefer `string`'s methods as they are simpler, lighter
and also faster

- Let's review a couple of options for using string's methods



## `re.search()` and `string.find()`

```python
>>> re.search(r'small', 'a small cat with a small apetite').span()
(2, 7)

>>> 'a small cat with a small apetite'.find('small')
2
```



## `string.replace()` and `re.sub()`

```python
# using re.sub()
>>> re.sub(r'small', 'big', 'a small cat with a small apetite')
'a big cat with a big apetite'

# using string.replace()
>>> 'a small cat with a small apetite'.replace('small', 'big')
'a big cat with a big apetite'
```



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Summary

- Regular Expressions detect patterns in strings
- Provide various ways to define the pattern
- Can be used for altering the string
- For simple needs prefer String's methods



# Summary (2)

> Some people, when confronted with a problem, think "I know, I'll use regular
> expressions."  Now they have two problems. -- Jamie Zawinski



# Questions

![Questions](img/q_everything.jpg "Duncan Hull, https://secure.flickr.com/photos/dullhunk/202872717/")



# Appendix



## Regex Syntax table

<table style="font-size: large; line-height: 1em;">
<thead>
<tr> <th>symbol</th> <th>meaning</th> </tr>
</thead>
<tbody>
<tr> <td><code>'.'</code></td> <td>Any Single Char</td> </tr>
<tr> <td><code>'^'</code></td> <td>Start of String</td> </tr>
<tr> <td><code>'$'</code></td> <td>End of String</td> </tr>
<tr> <td><code>'*'</code></td> <td>0 or more repetitions of previous RE, greedy</td> </tr>
<tr> <td><code>'+'</code></td> <td>1 or more repetitions of previous RE, greedy</td> </tr>
<tr> <td><code>'?'</code></td> <td>0 or 1 repetitions of previous RE, greedy</td> </tr>
<tr> <td>*?, +?, ??</td> <td>Non Greedy versions</td> </tr>
<tr> <td>{m}</td> <td>m copies of previous RE should be matched</td> </tr>
<tr> <td>{m,n}</td> <td>m to n repetitions of previous RE, greedy</td> </tr>
<tr> <td>{m,n}?</td> <td>m to n repetitions, non-greedy</td> </tr>
<tr> <td>'\'</td> <td>Escapes special char following ('*', '?', etc.)</td> </tr>
<tr> <td>[]</td> <td>Set of chars</td> </tr>
<tr> <td>    - [abc]</td> <td>- 'a', 'b' or 'c'</td> </tr>
<tr> <td>    - [a-z]</td> <td>- range, lowercase</td> </tr>
<tr> <td>    - [^abc]</td> <td>- Characters complementing the set (all but 'a', 'b' or 'c')</td> </tr>
<tr> <td>'A|B'</td> <td>B - RE A or B</td> </tr>
<tr> <td>(...)</td> <td>Select group inside the RE</td> </tr>
</tbody>
</table>
<!--
symbol         | meaning
--            | --
`'.'`         | Any Single Char
`'^'`         | Start of String
`'$'`         | End of String
`'*'`         | 0 or more repetitions of previous RE, greedy
`'+'`         | 1 or more repetitions of previous RE, greedy
`'?'`         | 0 or 1 repetitions of previous RE, greedy
*?, +?, ??    | Non Greedy versions
{m}           | m copies of previous RE should be matched
{m,n}         | m to n repetitions of previous RE, greedy
{m,n}?        | m to n repetitions, non-greedy
'\'           | Escapes special char following ('*', '?', etc.)
[]            | Set of chars
    - [abc]   | - 'a', 'b' or 'c'
    - [a-z]   | - range, lowercase
    - [^abc]  | - Characters complementing the set (all but 'a', 'b' or 'c')
'|'           | A|B - RE A or B
(...)         | Select group inside the RE
-->
<!--## Advanced-->
<!--- *?, +?, ??    - Non Greedy versions-->
<!--- {m,n}?        - m to n repetitions, non-greedy-->
<!--- (?aiLmsux)    RE flags, should be used first in expression string-->
<!--- (?:...)       Substring matched will not be accessible-->
<!--- (?P<name>...) Give a name to the substring matched, can be used in the RE-->
<!--- (?P=name)     Use the substring matched earlier with this name-->
<!--- (?#...)       Comment, ignored-->
<!--- (?=...)       Lookahead, matching only if `...` matched, but ... not part of match-->
<!--- (?!...)       Negative Lookahead-->
<!--- (?<=...)      Positive lookbehind assertion-->
<!--- (?<!...)      Negative lookbehind assertion-->
<!--- (?(id/name)yes-pattern|no-pattern)    Matches yes-pattern if id/name group exists, optional no-pattern otherwise-->



## Escape Sequences

<table style="font-size: large; line-height: 1em;">
<thead>
<tr> <th>Sequence</th> <th>Matches</th> </tr>
</thead>
<tbody>
<tr> <td>\number</td> <td>Contents of the group with this number</td> </tr>
<tr> <td>\A</td> <td>Start of the string</td> </tr>
<tr> <td>\b</td> <td>Empty string at the begining/end of a word</td> </tr>
<tr> <td>\B</td> <td>Empty string NOT at the begining/end of a word</td> </tr>
<tr> <td>\d</td> <td>Decimal digits (watch out for unicode!)</td> </tr>
<tr> <td>\D</td> <td>NOT a decimal digit</td> </tr>
<tr> <td>\s</td> <td>Whitespace - [ \t\n\r\f\v] and more if unicode</td> </tr>
<tr> <td>\S</td> <td>NOT whitespace</td> </tr>
<tr> <td>\w</td> <td>Word character [a-zA-Z0-9_] and more if unicode</td> </tr>
<tr> <td>\W</td> <td>NOT word character</td> </tr>
<tr> <td>\Z</td> <td>End of string</td> </tr>
</tbody>
</table>
<!--Sequence | Matches-->
<!----       | ---->
<!--\number  | Contents of the group with this number-->
<!--\A       | Start of the string-->
<!--\b       | Empty string at the begining/end of a word-->
<!--\B       | Empty string NOT at the begining/end of a word-->
<!--\d       | Decimal digits (watch out for unicode!)-->
<!--\D       | NOT a decimal digit-->
<!--\s       | Whitespace - [ \t\n\r\f\v] and more if unicode-->
<!--\S       | NOT whitespace-->
<!--\w       | Word character [a-zA-Z0-9_] and more if unicode-->
<!--\W       | NOT word character-->
<!--\Z       | End of string-->



## Flags

Flag            | Meaning
--              | --
DOTALL, S       | '.' to also match newlines
IGNORECASE, I   | Case-insensitive matches
LOCALE, L       | Locale-aware match
MULTILINE, M    | Multi-line matching, affects ^ and $
VERBOSE, X      | Allow whitespace and comments
ASCII, A        | Make escapes (\w, \b, \s, \d)



# Thanks!

[ Amit Kotlovski ](mailto:amit@amitkot.com) / [ @amitkot ](http://twitter.com/amitkot)

All Rights Reserved to Amit Kotlovski
