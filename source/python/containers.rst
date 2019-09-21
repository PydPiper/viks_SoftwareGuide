builtin - Tuples, Lists, Sets, Dictionaries
===========================================
There are 4 common collectors available to the users in python;
Tuples, Lists, Sets, and Dictionaries. Each of them have unique features
that work best for different objectives. Before digging into each of them,
let's set a common ground for definitions:

- Mutable: An object that can be changed after it is created

    - list, set, dict

.. code-block:: python

    a = [1,2,3]
    id(a)
    >>> 63674848
    a.append(4)
    id(a)
    >>> 63674848 # note that the id stays the same, however the content of the list changed

- Immutable: An object that cannot be changed after it is created

    - bool, int, float, str, tuple, frozenset

.. code-block:: python

    # same id every time you call id on a bool, int, float, str
    id(123)
    >>> 1794170960
    a = (1,2,3)
    id(a)
    >>> 69202848 # now note once you change the tuple size the id rolls
    a += (4,)
    id(a)
    >>> 65455520 # new id

Tuples
------

.. code-block:: python
    :linenos:

    # syntax
    a = (1,2,3)
    # add to tuple
    a += (4,)
    a
    >>> (1,2,3,4)
    # index an item
    a[0]
    >>> 1 # note first item of any container start at 0

    # there are 2 methods available on a tuple
    # count: count occurrence of a item within a tuple
    a.count(4)
    >>> 1
    # index: find the index for a given value
    a.index(4)
    >>> 3

Lists
-----


Sets
----


Dictionaries
------------
