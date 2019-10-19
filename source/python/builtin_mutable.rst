builtin - Mutable/Immutable
===========================
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