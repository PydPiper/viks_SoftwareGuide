bulitin - strings
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