builtin - Strings
=================

Syntax
------

.. code-block:: python
    :linenos:

    # strings can be represented in 2 different ways:
    "string with double quotes"
    'string with single quates'

    # benefit for double quotes: ability to use the apostrophe (')
    "it's a nice day"


Common String Tools
-------------------
.. note:: examples below assume the variable "x" is a string variable (ex: x="thiss")

- To uppercase: ``"this".upper()`` >> "THIS"
- To lowercase: ``"ThiS".lower()"`` "this"
- To title: ``"this is a title".title()`` >>> "This Is A Title"
- To add to string: ``x.append("that")`` or ``x += "that"``
- To count char occurrence: ``x.count("s")`` >>> 2 because x="thiss" and there are 2x "s"
- To find the index of a char: ``x.index("h")`` >>> 1

    - note string index start from 0
    - index will return the first occurrence from the left
    - similarly you can use ``x.find("s")`` >>> 3 or ``x.rfind("s")`` >>> 4

- To slice a string: variable[start:end:reverse]

    - slicing start will include the starting char, but NOT the ending char ``"this"[1:3]`` >>> "hi"
    - start/end field can be left blank to grab all of the start/end ``"this"[1:]`` >>> "his"
    - reverse order ``"this"[::-1]`` >>> "siht"

- To replace a char(s): ``"this this this".replace("thi", "set", 2)`` >>> "sets sets this"
- To check if str is a int: ``"345".isdigit()"`` >>> True
- To check if str is numeric: ``"345.1".isnumeric()"`` >>> True
- To find the length of a str: ``len("this")`` >>> 4 (subtract 1 if len of a string is used for indexing)
- To sort a str: ``sorted("this")`` >>> ['h', 'i', 's', 't']
- To join strings: ``"/".join(['this','that'])`` >> "this/that"
- To split strings: ``"this and that".split(" ")`` >>> ['this', 'and', 'that']
- To add a unix formatted new line (line feed): ``"this\n"``
- To add windows carriage return + line feed: ``"this\r\n"``
- To add a tab: ``"this\t"``


String Arguments and Formatting
-------------------------------

.. code-block:: python
    :linenos:

    # f-strings (python3+)
    f"x is equal to {x}"
    # benefit is that f-strings allows you to perform arithmetic/logic on the spot
    f"x is equal to {x + 5}"
    f"x is equal to {x if x < 5 else x + 5}"

    # format (python2-3)
    "x is equal to {}".format(x)
    "x is equal to {x1}".format(x1=x)

    # % "modulo operator"
    "x is equal to %(x1)d" % {"x1": x}

Formatting the argument injections

- {:5.2} 5 in this case is the str-length,and 2 is number of significant digits
  note significant digits overrule:
  {:3.5} will have a str-len of 6 chars for a positive number (5 digits and a ".")
  {:3.5} will have a str-len of 7 chars for a negative number (5 digits a "-" and ".")

.. code-block:: python

    f"{1:4}"
    >>> '   1'
    f"{1.11111:4}"
    >>> '1.11111' # not what you would expect str-len is not 4
    f"{1.11111:4.2}"
    >>> ' 1.1'
    f"{1.11111:2.4}"
    >>> '1.111' # note that sigfig wins vs str-len

- < > = ^: left, right, padding of characters, center rules

.. code-block:: python

    f"{1:<4}"
    >>> '1   '
    f"{1:>4}"
    >>> '   1'
    f"{1:0=4}"
    # note padding only works on int or float
    >>> '0001'
    f"{1:^4}"
    >>> ' 1  '

- "+" "-" "space": use sign for both pos/neg values (ie: "+5" and "-5"), sign for neg only ("5" "-5"),
  use sign for neg only but leave space for positive (" 5" "-5")

.. code-block:: python

    f"{1:+}|{-1:+}|{1:-}|{-1:-}|{1: }|{-1: }"
    >>> '+1|-1|1|-1| 1|-1|'

- d: int

.. code-block:: python

    f"{123:d}"
    >>> '123' # note that this does not convert a float to a int or str to int

- f: float (by default 6 decimals)

.. code-block:: python

    f"{1:f}"
    >>> '1.000000' # note flag f does convert a int to a float but NOT str->float

- e and E: exponent with small "e" or large "E" (default 6 decimals)

.. code-block:: python

    f"{1:e}"
    >>> '1.000000e+00' # similar to float conversion

- g: The precise rules are as follows: suppose that the result formatted with presentation type
  'e' and precision p-1 would have exponent exp. Then if -4 <= exp < p, the number is formatted
  with presentation type 'f' and precision p-1-exp. Otherwise, the number is formatted with
  presentation type 'e' and precision p-1. In both cases insignificant trailing zeros are removed
  from the significand, and the decimal point is also removed if there are no remaining digits
  following it.
  Positive and negative infinity, positive and negative zero, and nans, are formatted as inf,
  -inf, 0, -0 and nan respectively, regardless of the precision.
  A precision of 0 is treated as equivalent to a precision of 1. The default precision is 6.

- %: percentage. Multiplies the value by 100 and uses (f) format followed by a percent sign

.. code-block:: python

    f"{1:%}"
    >>> '100.000000%' # similar to float conversion

- ,: to separate every 1000 by a comma

.. code-block:: python

    f"{1000:,}"
    >>> '1,000'

- positional arg call:

.. code-block:: python

    "pos0={0}, pos2={2}, pos0={0}".format(*[10,20,30])
    >>> 'pos0=10, pos2=30, pos0=10'


Trick - Hide print statements (closure)
---------------------------------------
See :closures:`functions` under functional programming for more information about closures.

.. code-block:: python
    :linenos:

    import os, sys

    class HiddenPrints:
        def __enter__(self):
            # log the original stdout setting
            self._original_stdout = sys.stdout
            # buffer stdout into an empty path
            sys.stdout = open(os.devnull, "w")

        def __exit__(self, exc_type, exc_val, exc_tb):
            # close the buffer
            sys.stdout.close()
            # reset stdout setting to original
            sys.stdout = self._original_stdout