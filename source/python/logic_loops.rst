builtin - logic loops
=====================
- There are infinite ways to write code but it is important to highlight
  the wise words from The Zen of Python: "Flat is better than nested" in
  this section more than any other because it is far too easy to nest
  logic loops that are confusing to read after numerous indents. As a
  suggestion try to limit nested loops to 3-4; not only will it help
  downstream users pickup your code but the code will also be easier to
  test. Logic that needs more than 3-4 nested loops should be broken up
  into separate functions.

True False
----------
In python the following all evaluate to True:

- 0 == True (note that float 0.0 is not True, only the int 0)
- 'any non empty str' == True
- non empty set, tuple, list, dict


General "and" "or" "not" "any" "all"
------------------------------------

.. code-block:: python
    :linenos:

    2 == 5 or 10 == 10
    True

    2 == 5 and 10 == 10
    False

    2 != 5 and 10 == 10
    True
    # in a alternate form
    not 2 == 5 and 10 == 10
    True

    # "any" is powerful with list comprehensions:
    any(i == 4 for i in [3,4,5])
    True
    any(i == 10 for i in [3,4,5])
    False

    # "all" works similar to "any" but all instances of the iterable must eval to True
    all(i%2 == 0 for i in [4,6,8])
    True # because all num/2 result with a remainder of 0 (the values are all even)
    all(i%2 == 0 for i in [4,5,8])
    False # this evals to True, False, True which is overall False

if elif else
------------

.. code-block:: python
    :linenos:

    x = 5
    if x == 5:
        print('x == 5')
    elif x == 6:
        print('x != 5 but x does equal 6!')
    elif x == 7:
        print('x !=5, x != 6, but x == 7')
    else:
        print('x was not equal to 5, 6, or 7')

    # there is a 1 liner short for a simple if/else
    y = x + 10 if x == 5 else 0
    >>> y = 15 # these are really useful for initializing variable

for loop
--------

.. code-block:: python
    :linenos:

    # iterate through strings by char
    for char in "this":
        print(char)

    >>> 't'
    >>> 'h'
    >>> 'i'
    >>> 's'

    for value in range(start=1, stop=30, step=10):
        print(value)
    >>> '1'
    >>> '11'
    >>> '21'

    # iterate through sets, tuples, lists
    for value in [10,20,30]:
        print(value)

    >>> '10'
    >>> '20'
    >>> '30'

    # it is often useful to iterate through the values and also keep index
    for index, value in enumerate([10,20,30], start=100):
        print(index, value)
    >>> '100 10'
    >>> '101 20'
    >>> '102 30'

    # iterate through dicts (iterate on keys, values, or items)
    for key, value in {'key1':1, 'key2':2}.items():
        print(key, value)

    >>> 'key1 1'
    >>> 'key2 2'

List Comprehensions (alt for loops)
-----------------------------------



while loop
----------



try/except/pass
---------------

