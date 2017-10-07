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

I prefer to avoid writing comments, and write code that can be understood them.
Where they are absolutely necessary, comments should add value to the code, and be written in a way that doesn't interfere with the code.
I start and end all my comments with a blank commented line.
This forces me to think twice about adding useless one-liner comments.
It also nudges you towards explaining code blocks as opposed to lines, focusing on the _why_ & _what_ instead of the _how_.

Yes:

```python
#
# This is a comment.
# After we do X, we need to do Y.
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
