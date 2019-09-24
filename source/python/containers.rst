builtin - Tuples, Lists, Sets, Dictionaries
===========================================
There are 4 common collectors available to the users in python;
Tuples, Lists, Sets, and Dictionaries. Each of them have unique features
that work best for different objectives. Before digging into each of them,
let's set a common ground for definitions:

- Mutable: An object that can be changed after it is created

    - list, set, dict

.. code-block:: python
    :linenos:

    a = [1,2,3]
    id(a)
    >>> 63674848
    a.append(4)
    id(a)
    >>> 63674848 # note that the ID stays the same, however the content of the list changed

- Immutable: An object that cannot be changed after it is created

    - bool, int, float, str, tuple, frozenset

.. code-block:: python
    :linenos:

    # same ID every time you call id on a bool, int, float, str
    id(123)
    >>> 1794170960
    a = (1,2,3)
    id(a)
    >>> 69202848 # now note once you change the tuple size the ID rolls
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
    # get a subset of a tuple
    a[2:]
    >>> (3,4)

    # there are 2 methods available on a tuple
    # count: count occurrence of a item within a tuple
    a.count(4)
    >>> 1
    # index: find the index for a given value
    a.index(4)
    >>> 3

Lists
-----

.. code-block:: python
    :linenos:

    # syntax
    a = [1,2,3]
    # add to list
    a += [4,]
    # or
    a.append(5)
    a
    >>> [1,2,3,4,5]
    # remove an item from the list
    a.remove(3)
    a
    >>> [1,2,4,5]
    # remove item by index value
    a.pop(3)
    >>> 5
    a
    >>> [1,2,4]
    # get a subset of a list
    a[2:]
    >>> [4,]

List - Copy
^^^^^^^^^^^

.. code-block:: python
    :linenos:

    # true copy -> same ID, changing the index of one, changes the other
    a = [1,2,3]
    b = a
    id(a) == id(b)
    >>> True
    b.append(100)
    b
    >>> [1,2,3,100]
    a
    >>> [1,2,3,100]

    # shallow copy -> new list ID, however the values are the same object ID
    nested = [1,2]
    a = [nested,3,4]
    b = a[:] # this
    id(b) == id(a)
    >>> False
    # however note that altering a MUTABLE value changes the value on both "a" and "b"
    nested.append(100) # note that append is alters the list, but does not change its id
    b
    >>> [[1,2,100],3,4]
    a # now note, that "a" also changed - this is called a shallow copy
    >>> [[1,2,100],3,4]

    # deep copy -> new list ID, and new content IDs
    import copy as cp
    nested = [1,2]
    a = [nested,3,4]
    b = cp.deepcopy(a) # note that this is a slow process, for optimization look for deepcopy first
    nested.append(100)
    a
    >>> [[1,2,100],3,4]
    b
    >>> [[1,2],3,4] # nested is no longer linked in a deepcopy to list "b"

Sets
----

.. code-block:: python
    :linenos:

    # sets are great to use over lists when the user does not want to keep duplicates
    a = {1,2,10}

    # to add
    a.add(2) # this is duplicate and will not be added
    a
    >>> {1,2,10}
    a.add(4) # this is not a duplicate, therefore it is added
    a
    >>> {1,2,10,4}

    # to remove
    a.remove(10)
    a
    >>>{1,2,4}

    # find the overlaps between 2 sets
    a = {1,2,4}
    b = {2,3,4}
    a.intersection(b)
    >>> {2,4}

    # find the difference between 2 sets
    a = {1,2,4}
    b = {2,3,4}
    a.difference(b)
    >>> {1,3}

    # get the combined - non duplicate of 2 sets
    a = {1,2,4}
    b = {2,3,4}
    a.union(b)
    >>> {1,2,3,4}


Dictionaries
------------

.. code-block:: python
    :linenos:

    # syntax
    a = {"key1": "value1", "key2": "value2"}
    a["key1"] # access value via keys
    >>> "value1"

    # iterate through keys and values
    for k, v in a.items():
        print(k, v)
    >>> "key1 value1"
    >>> "key2 value2"