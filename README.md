# PySnooper - Never use print for debugging again #

**PySnooper** is a poor man's debugger. If you've used Bash, it's like `set -x` for Python, except it's fancier.

Your story: You're trying to figure out why your Python code isn't doing what you think it should be doing. You'd love to use a full-fledged debugger with breakpoints and watches, but you can't be bothered to set one up right now.

You want to know which lines are running and which aren't, and what the values of the local variables are.

Most people would use `print` lines, in strategic locations, some of them showing the values of variables.

**PySnooper** lets you do the same, except instead of carefully crafting the right `print` lines, you just add one decorator line to the function you're interested in. You'll get a play-by-play log of your function, including which lines ran and   when, and exactly when local variables were changed.

What makes **PySnooper** stand out from all other code intelligence tools? You can use it in your shitty, sprawling enterprise codebase without having to do any setup. Just slap the decorator on, as shown below, and redirect the output to a dedicated log file by specifying its path as the first argument.

# Example #

We're writing a function that converts a number to binary, by returning a list of bits. Let's snoop on it by adding the `@pysnooper.snoop()` decorator:

```python
import pysnooper

@pysnooper.snoop()
def number_to_bits(number):
    if number:
        bits = []
        while number:
            number, remainder = divmod(number, 2)
            bits.insert(0, remainder)
        return bits
    else:
        return [0]

number_to_bits(6)
```
The output to stderr is:

![img](https://camo.githubusercontent.com/3ec11fee6df7ded55956086a6d31e5e573451b30f8c22bbca56c51ff913116dc/68747470733a2f2f692e696d6775722e636f6d2f5472463356566a2e6a7067)

Or if you don't want to trace an entire function, you can wrap the relevant part in a `with` block:

```python
import pysnooper
import random

def foo():
    lst = []
    for i in range(10):
        lst.append(random.randrange(1, 1000))

    with pysnooper.snoop():
        lower = min(lst)
        upper = max(lst)
        mid = (lower + upper) / 2
        print(lower, mid, upper)

foo()
```

which outputs something like:

```
New var:....... i = 9
New var:....... lst = [681, 267, 74, 832, 284, 678, ...]
09:37:35.881721 line        10         lower = min(lst)
New var:....... lower = 74
09:37:35.882137 line        11         upper = max(lst)
New var:....... upper = 832
09:37:35.882304 line        12         mid = (lower + upper) / 2
74 453.0 832
New var:....... mid = 453.0
09:37:35.882486 line        13         print(lower, mid, upper)
Elapsed time: 00:00:00.000344
```

# Features #

If stderr is not easily accessible for you, you can redirect the output to a file:

```python
@pysnooper.snoop('/my/log/file.log')
```

You can also pass a stream or a callable instead, and they'll be used.

See values of some expressions that aren't local variables:

```python
@pysnooper.snoop(watch=('foo.bar', 'self.x["whatever"]'))
```

Show snoop lines for functions that your function calls:

```python
@pysnooper.snoop(depth=2)
```

**See [Advanced Usage](https://github.com/cool-RR/PySnooper/blob/master/ADVANCED_USAGE.md) for more options.** <------


# Installation with Pip #

The best way to install **PySnooper** is with Pip:

```console
$ pip install pysnooper
```

# Other installation options #

Conda with conda-forge channel:

```console
$ conda install -c conda-forge pysnooper
```

Arch Linux:

```console
$ yay -S python-pysnooper
```

Fedora Linux:

```console
$ dnf install python3-pysnooper
```

# License #

Copyright (c) 2019 Ram Rachum and collaborators, released under the MIT license.


# Media Coverage #

[Hacker News thread](https://news.ycombinator.com/item?id=19717786)
and [/r/Python Reddit thread](https://www.reddit.com/r/Python/comments/bg0ida/pysnooper_never_use_print_for_debugging_again/) (22 April 2019)

详细教程：https://blog.csdn.net/chinesehuazhou2/article/details/109759400

