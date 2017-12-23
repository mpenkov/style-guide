# style-guide

My preferred style for writing Python code

I'm too lazy to write this from scratch, so here are some starting points, and go from there:

1. [PEP8](https://www.python.org/dev/peps/pep-0008/)
2. [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)

Next, I'll list my personal additions and extensions to the above.

## Closing Parentheses, Brackets, and Braces

I prefer these to be under the first character of the line that started the multi-line construct.
This way, it's easier to find the end of the multi-line construct: you just scan down, as opposed to down and right.

Yes:

```python
data = {
    'foo': 'bar',
    # ... lots of other elements here
    'baz': 'boz'
}
my_function(
    foo,
    # ... lots of other elements here
    bar
)
```

No:

```python
data = {
    'foo': 'bar',
    # ... lots of other elements here
    'baz': 'boz'}
my_function(
    foo,
    # ... lots of other elements here
    bar)
```

## Line Length

I find that 80 characters per line is a bit stingy, so aim for 90, with a hard limit of 100.
For more about this, see [Beyond PEP8](https://www.youtube.com/watch?v=wf-BqAjZb8M).

## Comments

I prefer to avoid writing comments, and write code that can be understood without them.
Where they are absolutely necessary, comments should add value to the code, and be written in a way that doesn't interfere with the code.
I start and end all my comments with a blank commented line.
This forces me to think twice about adding useless one-liner comments.
It also nudges you towards explaining code blocks as opposed to lines, focusing on the _why_ & _what_ instead of the _how_.

Yes:

```python
#
# This is a comment.
# After we do X, we need to do Y, because otherwise Z will happen, which is bad.
#
foo = 'bar'
foo.upper()
```

No:

```python
# This is a comment
foo = 'bar'
# And now we have to do Y.
foo.upper()
```

If your code _really_ need comments, ask yourself: can you rewrite it such that it doesn't?

> Write code that is easy to understand; the more you do this, the fewer comments you will need.

(from [The Practice of Programming](http://michael.penkov.id.au/blog/2017/12/24/practice.html))

## Imports

I'm OK with using either absolute or relative imports.

I only import packages and modules.
This helps me remember where everything comes from.
It also makes things easier to mock, when you actually have to mock them.

Yes:

```python
import io
buf = io.BytesIO()
```

No:

```python
from io import BytesIO
buf = BytesIO()
```

I have a few "pet" imports that I like:

Yes:

```python
import os.path as P
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
# Any others?
```

No:

```python
import os.path
import numpy
import pandas
import matplotlib.pyplot
```

## String Quotes

I favor single quotes for literals, and double quotes for docstrings and multi-line strings.

Yes:

```python
def foo():
    """This function returns 'bar', as you would expect it to.""" 
    return 'bar'
```

No:

```python
def foo():
    '''This function returns 'bar', as you would expect it to.''' 
    return "bar"
```

When dealing with long strings, I'm OK with either backslashing or using implicit concatenation.

OK:

```python
ok = "This should be \
one line."
also_ok = ("This should be "
           "one line.")
```

## Docstring Format

I prefer the Sphinx way of writing docstrings over Google's.
The former looks good when rendered, and is still readable in its raw form.
The latter is readable when rendered, and looks good in raw form.

Yes:

```python
def get_thing(name):
    """Return a Thing given its name.

    :param str name: The name of the Thing.
    :returns: The Thing that corresponds to the specified name.
    :rtype: Thing
    :raises: ValueError if the specified name doesn't correspond to a Thing.
    """
```

No:

```python
def get_thing(name):
    """Return a Thing given its name.

    Args:
        name: The name of the Thing.

    Returns: the Thing that corresponds to the specified name.

    Raises: ValueError if the specified name doesn't correspond to a Thing.
    """
```

## Public and Internal Interfaces

> Any backwards compatibility guarantees apply only to public interfaces.
> Accordingly, it is important that users be able to clearly distinguish between public and internal interfaces.

I prefix internal things with an underscore.
This lets me know I can liberally rewrite that code without worrying about breaking compatibility:

```python
def _make_it_awesome(x):
    return x


def public_function(x):
    """This does something really awesome."""
    return _make_it_awesome(x)
```

## List Comprehensions

Favor declarative code over procedural code (focus on the _what_, not the _how_).
List comprehensions can help here.

Yes:

```python
print(sum(i**2 for i in range(10)))
```

No:

```python
result = 0
for i in range(10):
    result += i**2
print(result)
```

# Pet Peeves

The below aren't exactly "style" issues, but when I'm reading, writing or reviewing code, they still come up often enough for me to consider them relevant.

## Positional vs Keyword Arguments

Favor keyword arguments unless it's extremely obvious what the positional arguments are.

Yes:

```python
twitter_search('@obama', retweets=False, numtweets=20, popular=True)
```

No:

```python
twitter_search('@obama', False, 20, True)
```

Example stolen from [this presentation](https://youtu.be/OSGv2VnC0go?t=32m00s).

## Avoid Functions Masquerading as Functions

If your class only has two functions and one of them is a constructor, you may be dealing with a function masquerading as a class.

Yes:

```python
def get(url):
    # some relevant work here
    return requests.get(url)

body = get('http://mpenkov.github.io')
```

No:

```python
class Get(object):
    def __init__(self, url):
        self.url = url

    def do(self):
        # some relevant work here
        return requests.get(url)

body = Get('http://mpenkov.github.io').do()
```

## Don't Use Namespaces to Create Taxonomies

> Flat is better than nested.

Namespaces are for preventing name clashes.
They are not for creating taxonomies.

Yes:

```python
import muffin
muf = muffin.find_muffin("Misha's favorite muffin")
```

No:

```python
import muffin.muffinfinder
muf = muffin.muffinfinder.find_muffin("Misha's favorite muffin")
```

Example inspired by [this talk](https://youtu.be/o9pEzgHorH0).

## Naming Things

- Use active verbs for function and method names; use nouns for variable names.
- Use descriptive names for globals, short names for locals.
- Names should be informative, concise, memorable and pronouncible.
- Avoid unnecessarily long names.  Clarity is often achieved through brevity.

Yes:

```python
def is_odd(number):
    return number % 2 == 1
```

No:

```python
def odd(x):
    return x % 2 == 1
```

## Be Clear

> The goal is to write clear code, not clever code.
> The proper criterion is ease of understanding.

(from [The Practice of Programming](http://michael.penkov.id.au/blog/2017/12/24/practice.html))

## Be Natural

Write statements in a natural order: make your code look like English.
Avoid unnecessary negations.

Yes:

```python
if not (0 < month <= 12):
    raise ValueError
```

Yes:

```python
if month <= 0 or month > 12:
    raise ValueError
```

No:

```python
if not (month >= 0 and month <= 12):
    raise ValueError
```

## Be Helpful

Error messages should state the form of valid input.

Yes:

```python
if not (0 < month <= 12):
    raise ValueError('month must be an integer between 1 and 12, inclusive')
```

No:

```python
if not (0 < month <= 12):
    raise ValueError('bad month')
```

## Keep it Simple

When dealing with conditionals, handle the easy cases first.
Avoid nesting conditionals where possible.
This removes significant burden for the reader.

# Relevant Links

Here are some great talks to listen to:

- [Stop Writing Classes](https://youtu.be/o9pEzgHorH0)
- [Transforming Code into Beautiful, Idiomatic Python](https://youtu.be/OSGv2VnC0go)
- [How to Write Reusable Code](https://youtu.be/r9cnHO15YgU)
- [Beyond PEP8](https://youtu.be/wf-BqAjZb8M)
- [The Other Hard Problem](https://youtu.be/bg1wdbKBRKg)
- [Jane Austen on PEP8](https://youtu.be/55gXwFviOuQ)
- [The Practice of Programming](http://michael.penkov.id.au/blog/2017/12/24/practice.html)

Last but not least, [here](https://youtu.be/knMg6G9_XCg) is the talk that inspired me to write this "guide".
