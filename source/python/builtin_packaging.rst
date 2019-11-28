builtin - Packaging (managing python imports/files/projects)
============================================================
Managing imports can be quite a challenge in python at first. There are builtin python
packages, packages you install (through pip), and your own files that can all be imported into your project.
This section will hopefully shine some light on how imports work in python, and give you an idea on how
to setup your own package/library. Note that folks use library and package in python interchangeably. We
will use "packages" since that is how python folder system calls them.

Level Setting Imports
---------------------

Importing syntax and meaning
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1.) Import everything under a package (this can be slow and not preferable since it is not explicit)

.. code-block:: python

    import unittest

    # now use it via
    mycase = unittest.TestCase

    # we can also use our own custom shortname for an import via:
    import unittest as ut

    # now use it via
    mycase = ut.TestCase

2.) Import specifics (preferred option because it is explicit)

.. code-block:: python

    from unittest import TestCase

    # now use it via
    mycase = TestCase

Importing Global Site-Packages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Site-packages are accessible to the python interpreter no matter where your current working
directory may be (see in more detail :ref:`builtin_packages_ref1`).

.. code-block:: python

    import unittest # under Python/Lib as a module folder
    import csv # under Python/Lib as a module file
    import pandas # under Python/Lib/site-packages as a module folder (if pandas is pip installed)


Importing your own files in a working directory.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    Folder structure:

        folder1
        |-file1.py
        |-file2.py
        |-folder2
            |-file3.py

1.) files in the same folder as your original script

.. code-block:: python

    # this code is in file1.py

    # we dont need to type ".py" at the end of the file names
    import file2

    # to use a function "foo" that is in file2.py
    file2.foo()

2.) import files from a sub-directory

.. code-block:: python

    # this code is in file1.py

    # sub-folders are handled by "." instead of "/" in python imports
    import folder2.file3

    # to use a function "foo" that is in file3
    folder2.file3.foo()

3.) import files from a directory above

.. note:: You can only import files on the same directory level as your starting script file!
   Never higher! You will get an ``ValueError: attempted relative import beyond top-level package``


.. code-block:: python

    # this code is in file3.py

    # the following works if we launch our original script: python file1.py
    #  in file1.py the same code exists as in bullet 2) that then calls file3.py
    # note: this works because file1.py (top level directory file was launched)
    #  if you tried to run file3.py by itself: python file3.py it will fail because of the note above
    from file2 import foo

    foo()


How to make your package accessible to your python instances
------------------------------------------------------------
Let's say we wrote a piece of python code that we want to reuse. We have a few very annoying options to
directly call this python file from any directory:

- we can brute force copy the file from one directory to the next so we that we can locally import it (terrible
  option, never do this)
- we can absolute/relative path import it into our other project (not good solution since folder paths
  change all the time)
- we can add our python script location to our system environment PATH. This can be done by either editing
  our windows account environment PATHs or within python using ``sys.path``. (now we are getting warmer but
  this solution still depends on file paths that again might change)

.. note:: The best solution is to add your python file/package to your ``Python/Lib/site-packages`` folder.
          All site-package scripts and packages are available to any python instance you may spin up and
          importing is just as easy as any other site-package import (ex: ``import mycustompackage``)


.. _builtin_packages_ref1:

How Do Site-packages (builtin or pip installed packages) Import
---------------------------------------------------------------
Python comes with a few very handy builtin packages all stored in the you ``Python/Lib`` like os, csv,
html, etc., and ``Python/Lib/site-packages`` for pip installed packages like pandas, pyqt and so on.
These packages are directly accessible to any python interpreter as long as it is in ``Python/Lib`` or
in ``Python/Lib/site-packages``. Let's take a look at how an existing one works:

1.) A python package stored in ``Python/Lib`` or ``Python/Lib/site-packages`` is accessible to your python
interpreter no matter where the interpreter was launched from!

2.) Take for instance the ``unittest`` builtin package. It is located under ``Python/Lib``
Pay special attention to the fact that ``unittest`` is actually a folder.

3.) We can import this package from any python interpreter by typing

.. code-block:: python

    import unittest

4.) The code above import all of unittest. So that great and all but how does it work? ``unittest``
as we noted earlier is a folder. Well when we python treats folders like modules so long as that
folder contains a ``__init__.py``. Open up ``Python/Lib/unittest`` and convince yourself that, that is
in fact true.

5.) Now we are faced with another question. What in the world is a ``__init__.py`` file?
A ``__init__.py`` is referred to as a constructor, it converts a folder into a module that can be imported
by python, and when imported all functions/classes within ``__init__.py`` are imported (in this case ``unittest.TestCase`` for
instance)

.. code-block:: python

    # sample code from: Python/Lib/unittest/__init__.py

    __all__ = ['TestResult', 'TestCase',]

    from .result import TestResult
    from .case import TestCase

.. code-block:: python

    # our script can use unittest and it's imported results by...
    import unittest

    # this works because python imported unittest from Python/Lib
    #  then unittest/__init__.py imported under the hood TestResult and TestCase
    mytestcase = unittest.TestCase

.. code-block:: python

    # note that we could just as well have jumped straight to TestCase if
    #  that is all we were using (this is always more preferred to import only what you need)
    from unittest import TestCase

    # now use it by...
    mytestcase = TestCase

6.) Before closing out this site-package example, let's take a look at ``__all__``.

    6.1) By default python a general import call: ``import unittest`` will import all
    functions/classes/modules that are listed in the ``__init__.py`` file.

    6.2) ``__all__`` will prevent the user from importing anything that is not explicitly stated in
    the ``__all__ = [...]`` list. Note, this is only true if the user uses the explicit import
    form ``from unittest import *``.

.. note:: ``__all__`` does not exempt the user from directly importing a hidden function. For example lets
            suppose there is a function under unittest called ``hiddenfunc`` we could bypass the ``__all__``
            restriction by directly importing the function name ``from unittest import hiddenfunc`` or
            simply just using the general import ``import unittest`` that import everything in the ``__init__.py``


How to structure your own package
---------------------------------
more on __init__ and __all__