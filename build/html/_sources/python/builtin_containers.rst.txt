builtin - Tuples, Lists, Sets, Dictionaries
===========================================
There are 4 common collectors available to the users in python;
Tuples, Lists, Sets, and Dictionaries. Each of them have unique features
that work best for different objectives.

Tuples
------
Tuples are immutable (once a value is added, its ID cannot be changed), see :doc:`builtin_mutable`.
These are great for ensuring that values stay consistent throughout your code. Note that there is
a subtlety here though; a tuple ``test=(1,[2,3])`` cannot change its IDs, however there
is a list within the tuple that can change its internal values (therefore the tuple is not,
truely immutable when it contains mutable objects).

- Syntax

.. code-block:: python

    a = (1,2,3)
    # index an item
    a[0]
    >>> 1 # note first item of any container in python starts at 0
    a[-1] # negative indexing is from the end
    >>> 3

- Add to a Tuple

.. code-block:: python

    a += (4,)
    a
    >>> (1,2,3,4)

- Get a subset of a tuple

.. code-block:: python

    a[2:]
    >>> (3,4)

- Method of Tuple: "count" occurrence of an item
- Method of Tuple: "index" finds the index for a given value

.. code-block:: python

    # there are 2 methods available on a tuple
    # count: count occurrence of a item within a tuple
    a.count(4)
    >>> 1
    # index: find the index for a given value
    a.index(4)
    >>> 3

Lists
-----
Lists are extremely useful because you can change them on the fly (mutable object, see more on
:doc:`builtin_mutable`). However, there is a fine line between when a List should be used over
Dictionaries. You can just about do everything with a Dictionary that you can do with a List, and
when Lists start to look double, triple+ nested - a Dictionary should be looked at for code
readability/use.

See :ref:`logic_loops_list_comprehensions` for list comprehensions, a 1-liner code that replaces a simple
for loop with an optional if statement.

- Syntax

.. code-block:: python

    a = [1,2,3]
    # index on item
    a[0] # note first item of any container in python starts at 0
    >>> 1
    a[-1] # negative indexing is from the end
    >>> 3

- add/remove to a list

.. code-block:: python

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

- list slicing (sublist from list, reverse order, skip content)

.. code-block:: python

    a = [1,2,3,4,5]


    # get a subset of a list
    a[2:]   # this reads, [from starting index item included, to end index item NOT included]
    # by not specifying the end index, we get from index 2 all the way to the end of the list
    >>> [3,4,5]

    # negative indexing
    a[:-2]  # give me everything from start to 2 indexes before the end
    >>> [1,2,3]

    # reverse order
    a[::-1]
    >>> [5,4,3,2,1]

    # skip every 2 for example
    a[::2]
    >>> [1,3,5]

- define multiple variables on 1 line

.. code-block:: python

    # define multiple variables on same line
    mylist = [1,2,3]
    a, b, c = mylist
    a
    >>> 1
    b
    >>> 2

    # also good for initializing variables
    a, b, c = [""]*3 # will all be empty strings

.. _list_copy:

List - Copy
^^^^^^^^^^^
See also :doc:`builtin_copy` for more information.

- true copy -> same ID, changing the index of one, changes the other

.. code-block:: python

    a = [1,2,3]
    b = a
    id(a) == id(b)
    >>> True
    b.append(100)
    b
    >>> [1,2,3,100]
    a
    >>> [1,2,3,100]

- shallow copy -> new list ID, however the values are the same object ID

.. code-block:: python

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

- deep copy -> new list ID, and new content IDs

.. code-block:: python

    import copy as cp
    nested = [1,2]
    a = [nested,3,4]
    b = cp.deepcopy(a) # note that this is a slow process, for optimization look for deepcopy first
    nested.append(100)
    a
    >>> [[1,2,100],3,4]
    b
    >>> [[1,2],3,4] # nested is no longer linked in a deepcopy to list "b"

List Trick - Split a list into equal bits
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    a = [1,2,3,4,5,6,7,8,9]
    list(zip(*[iter(a)]*3))
    # this translates to: make a list of ( create a single tuple from ( 3 iterators of "a" ) )
    >>> [(1, 2, 3), (4, 5, 6), (7, 8, 9)]

    # iter(a)*3 -> 3 iterators are created with the same ID
    # for explanation lets call these iter1.1, iter1.2, iter1.3
    # zip(* (iter1.1, iter1.2, iter1.3)) unpacks the iterator with "next"
    # (next(iter1.1 pos0), next(iter1.2 pos1), (next(iter1.3 pos3)), (next(iter1.1 pos4), ...so on
    # since the iters are all identical objects, they share the "next" counter
    # zip takes the 3 subdivided iters one value at a time and creates a tuple out of 3x next calls
    # this step repeats until a StopIteration is hit
    # the last step is to convert a zip object to a list via: list(zip...)

Sets
----
Sets are the best for storing unique values, finding same value intersects, finding different value
intersects or combining unique values. It is far too easy to always use Lists for everything, but
always remember that Sets are available to handle unique values very fast and efficiently that would
otherwise require more work on a List.

- Syntax: create, add, remove

.. code-block:: python

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

- find the overlaps between 2 sets

.. code-block:: python

    a = {1,2,4}
    b = {2,3,4}
    a.intersection(b)
    >>> {2,4}

- find the difference between 2 sets

.. code-block:: python

    #
    a = {1,2,4}
    b = {2,3,4}
    a.difference(b)
    >>> {1,3}

- get the combined - non duplicate of 2 sets

.. code-block:: python

    a = {1,2,4}
    b = {2,3,4}
    a.union(b)
    >>> {1,2,3,4}


Dictionaries
------------
Dictionaries are great for name-space like structured data (key/value pairs).

Easy to read and use,
however it can be tricky to use while writing large pieces of code, since available keys are not auto-completed
therefore the programmer has to remember what keys are available for use. For large-code or more-program-friendly
use case - classes should be looked at for containing data in its name-space attributes that is auto-completed.

See :ref:`logic_loops_list_comprehensions` for dictionary comprehensions.

.. code-block:: python

    # syntax
    a = {"key1": "value1", "key2": "value2"}
    a["key1"] # access value via keys
    >>> "value1"

    # report out a default value if a key does not exist with "get" instead of raising a KeyError
    a.get("key3", "not on record")
    >>> "not on record"

    # add to dict
    a = {"key1": "value1"}
    a["key2"] = "value2"
    # or with "update"
    a.update({"key3": "value3"})
    # the same syntax can be used to update an existing key/value pair

    # iterate through keys and values
    for k, v in a.items():
        print(k, v)
    >>> "key1 value1"
    >>> "key2 value2"


Trick - Handling nested dicts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    # pulling out a sub-dict from sub-dict values
    database = {1:{'name': 'bob', 'color': 'blue'},
                2:{'name': 'jay', 'color': 'green'},
                3:{'name': 'kai', 'color': 'blue'},}
    # lets pull out a sub-dict database for all color=blue people
    subdb = {ID: subdict for ID, subdict in database.items() if subdict['color'] == 'blue'}

Trick - Merging 2 dicts (shallow copy)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Note that a shallow copy will create a new dict ID but the key and value objects will still
be the same object ID as the originals. (this is only an issue if the original dicts are defined
via mutable variables). The example below will not have any issues since strings and integers are
immutable.

.. code-block:: python

    x = {'a': 1, 'b': 2}
    y = {'b': 3, 'c': 4}

    z = {**x, **y}
    >>> {'c': 4, 'a': 1, 'b': 3}

Trick - Adding a custom attribute to dict
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ever wish that there was a cleaner attribute to a builtin object? I ran into this when
trying to write a user-friendly API but did not want to force the users to necessarily
know what a dict.keys() were, instead i wanted to write it out as something much more pythonic

.. code-block:: python

    # lets say we have a database class that holds IDs of items in a dictionary
    class MyDB():
        def __init__(self):
            # we initialize an empty dictionary
            self.items = {}

        # define a method for adding items
        def additem(self,ID,arg):
            # we then update the dictionary storage
            self.items.update({ID: arg})

    # now to use it, we could say
    db = MyDB()
    db.additem(1,'apples')
    db.additem(2,'oranges')

    # we can display the item IDs by typing
    db.items.keys()
    # but the results are not very nice to read, nor is db.items.keys() intuitive for users
    >>> dict_keys([1,2])

- wouldnt it be nice if we could just type db.items.ids? Let's see how it's done

.. code-block:: python

    # same class as before, but self.items = a custom class now that extends dictionaries
    class MyDB():
        def __init__(self):
            # we initialize an empty dictionary
            self.items = ExtDict()

        # define a method for adding items
        def additem(self,ID,arg):
            # we then update the dictionary storage
            self.items.update({ID: arg})


    # a custom class to extend dictionaries
    #  inherit from dict all the builtin attributes of a dictionary
    class ExtDict(dict):
        @property
        def ids(self):
            return list(self.keys())


    # now let's use it
    db = MyDB()
    db.additem(1,'apples')
    db.additem(2,'oranges')

    # lets get the existing IDs a more user-friendly way
    db.items.ids    # easy to read/use
    >>> [1,2]       # easy to read/use


