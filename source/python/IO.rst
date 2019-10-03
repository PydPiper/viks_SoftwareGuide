builtin - File Input/Output (IO)
================================

Syntax
------
Although there are many ways to work with files, by far the safest is with "Context Manager".
A Context Manager has an Enter and an Exit protocol that handles event. In terms of file IO,
``with open`` opens file stream on Enter, and closes file stream on Exit, therefore files are
always closed done once the code is done streaming it.

.. code-block:: python

    with open("filename.txt", "r") as f:
        while True:
            line = f.readline()
            if not line:
                break
