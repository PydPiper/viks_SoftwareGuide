10min - Python Starter Kit
==========================
This starter kit was meant for readers that have never used python
and have very little knowledge of programming concepts.

Installation - Windows Guide Only
---------------------------------
1) Download the latest version of python from `python.org <https://www.python.org/>`_

2) During installation all the default selection will work, but pay attention to where python is being installed.
   The newer versions on windows will be place it under: ``c/users/yourusername/AppData/Local/Programs``

3) Once python is installed browse to folder it was installed in, something like
   ``c/users/yourusername/AppData/Local/Programs/Python38-32`` and inside you will see a ``python.exe``

4) Add the python folder to your windows ``PATH`` so that you can pull up python from any terminal
   (cmd, powershell, bash).

    4.1) Hit the windows start menu (bottom left windows icon)

    4.2) Type ``environment`` and you are looking for ``edit environment variables for your account``

    4.3) Highlight ``Path`` and hit ``edit``

    4.4) A new window will open, hit ``new`` and paste in your path where python was installed ``c/users/yourusername/AppData/Local/Programs/Python38-32``

    4.5) Hit ``OK`` on both windows and you are good to go!

5) Running python: pull up a terminal (start menu > cmd > enter) and type ``python``

**Things you should know about**:

- Python is manually downloaded from python.org. There is no update button to get a newer version,
  you will have to go back to python.org and download a new version manually again.

- When installing Python, you are also installing a python package installer (pip) that unlocks python's
  superpowers. Packages can be imported with a single line of a code and before you know it you are
  scraping the web, working on excel/text files or performing machine learning with only 10 lines of code.

- Everything in python is about versions. Python has a version, under it your pip has a version, under that
  your packages will have versions.


Run your first Script
---------------------
You have python installed and it works. Now you can type away at a python session in a terminal but
once you close the terminal, your code will also be gone. So instead we can write a script can you
can call any number of times:

1) create a script file (lets say on your desktop): Right Mouse Button > New > Text Document
2) rename it ``myscript.py``
3) open it with your text editor (notepad++ is a decent pick)
4) type out your python code and save, example:

.. code-block:: python

    print("Hello World!")

5) run your script by typing ``python myscript.py`` in your terminal(your terminal has to be in the same folder).
   Start Menu > type "cmd" > enter > then in the terminal ``cd Desktop`` then try ``python myscript.py``

Python Highlevel Concepts
-------------------------

.. note:: ">>>" is not part of any code, it is simply shown here to distinguish between code written
          and its output results. (ie. do not copy/write lines containing ">>>")

Basics
++++++

- code comment: ``# this is a comment``
- define a variable (no declaration needed!): ``a = 5``
- understand your basic types:

    - int:``1`` or ``2342134``
    - float: ``1.0`` or ``2342134.12341234``
    - string: ``"this"`` or ``'that'`` both valid but double quotes is better for ``"it's a nice day"``
    - list (arrays if you prefer): 1D ``[1,2,3]`` 2D ``[[1,2,3],[10,20,30],[100,200,300]]``
    - there are a lot more but these are the basics

What can I do with integers/floats (math)
+++++++++++++++++++++++++++++++++++++++++

.. code-block:: python

    a = 5
    b = 10
    c = a + b
    print(c)
    >>> 15
    # add, subtract, multiply, divide, power
    5 + 10 - 10 * 5 /5 ** 2
    >>> 13.0

What can I do with strings
++++++++++++++++++++++++++

- split up text

.. code-block:: python

    a = 'this is a string'
    b = a.split(" ") # split text base on " " single spaces
    b
    >>> ['this', 'is', 'a', 'string']

- replace characters

.. code-block:: python

    a = 'this is a string'
    b = a.replace('s','S')
    b
    >>> 'thiS iS a String'


- add two strings

.. code-block:: python

    a = 'this'
    b = 'that'
    c = a + b
    c
    >>> "thisthat'
    # or use join, note items have to be in square brackets
    d = ' '.join([a,b]) # join "a" and "b" with a " " space
    >>> 'this that'

- sub-strings (slicing)

.. code-block:: python

    a = 'this is a string'
    a[0] # index to a character (python indexing start at 0)
    >>> 't'
    b = a[0:4] # give me the characters from index 0 to start-of index 4, t=0,h=1,i=2=s=3,4=' '
    b
    >>> 'this'

What can I do with lists
++++++++++++++++++++++++

- indexing

.. code-block:: python

    a = [10,20,30]
    a[0] # python indexing starts at 0
    >>> 10
    a[0:2] # from index 0=10, to right before index 2=30 so that's 20
    >>> [10,20]

- add to a list

.. code-block:: python

    a = [] # empty list
    a.append(10) # append one at a time
    a += [20,30] # add another list to it
    a
    >>> [10,20,30]

- 2D array (really just a nested list)

.. code-block:: python

    x = [10,20,30] # 3 x-coordinates
    y = [40,50,60] # 3 y-coordinates
    myarray = list(zip(x,y))
    myarray
    >>> [(10, 40), (20, 50), (30, 60)]
    myarray[1] # what is the x,y -coordinate of point 2 (note again python index starts from 0)
    >>> (20,50)
    myarray[1][0] # what is the x-coordinate of point 2
    >>> 20
    myarray[1][1] # what is the y-coordinate of point 2
    >>> 50

How to write logic loops (if, for, while)
+++++++++++++++++++++++++++++++++++++++++
equal: ``==``, not equal: ``!=``, and: ``and``, or: ``or``

- if statements

.. code-block:: python

    if 1 == 1 and 1 == 2:
        print('1 is equal to 1 and also equal to 2')
    elif 1 != 1:
        print('1 is not equal to 1')
    else:
        print('none of the conditions were true')

- for loop

.. code-block:: python

    mylist = [10,20,30]
    for item in mylist:
        print(item)
    >>> 10
    >>> 20
    >>> 30

- while loop

.. code-block:: python

    i = 0
    while i < 3:
        print(i)
        i += 1
    >>> 0
    >>> 1
    >>> 2

How to write functions
++++++++++++++++++++++

.. code-block:: python

    # define function with 2 inputs
    def myfunc(input1, input2):
        result = intput1 + input2 + 10
        return result

    # call a function with inputs 1,2
    func(1,2)
    >>> 13

How do I read/write files
+++++++++++++++++++++++++

- read a file

.. code-block:: python

    # container for lines of text out of our file
    lines = []

    # use the python builtin function "open" to start streaming a file for read "r"
    with open('test.txt', 'r') as f:
        while True:
            # read each line in a file
            line = f.readline()
            # add each line to our container
            lines.append(line)
            # at the end of the file, line=""
            # in which case we stop reading the file and break out of the loop
            if not line:
                break

- write a file

.. code-block:: python

    # writing is very similar, except we "w" for write
    with open('test2.txt', 'w') as f:
        f.write('Hello World')

