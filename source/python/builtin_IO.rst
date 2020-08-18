builtin - File Input/Output (IO)
================================

Syntax
------
Although there are many ways to work with files, by far the safest is with "Context Manager", see :doc:`builtin_context_manager`.
A Context Manager has an Enter and an Exit protocol that handles events. In terms of file IO,
``with open`` opens a file stream on Enter, and closes file stream on Exit, therefore files are
always closed down once the code is done streaming it.

.. code-block:: python

    # read a file
    with open("filename.txt", "r") as f:
        while True:
            line = f.readline()
            if not line:
                break

    # write a file
    text = ["hello", "world"]
    with open("filename.txt", "w") as f:
        for line in text:
            f.write(line + "\n")

Open file object modes:

- ``r``: read only
- ``w``: write only
- ``x``: execution only
- ``a``: append to existing file
- ``b``: binary mode (this is combined with ``r`` or ``w``
- ``+``: open for updating (reading/writing)


Working with directories
------------------------

.. code-block:: python
    import os
    from pathlib import Path

    # returns a list of files/folders in current working dir
    os.listdir()
    >>> ['file1.txt', 'file2.txt', 'folder']
    # or feed it a subdir path
    os.listdir('./folder')
    >>> ['subfile1.txt', 'subfile2.txt']

    os.path.isfile('file1.txt')
    >>> True
