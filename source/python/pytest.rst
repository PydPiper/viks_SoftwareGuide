builtin - pytest
================


To mock fileread without an actual file
---------------------------------------

.. code-block:: python

    f = io.StringIO("text\n")

    f.readline()
    >>> "text"