lib - pytest
============
The concept of writing code to test code can be a bit bizarre at first, but the idea is to test
the behavior of a function or integration of functions with a set of inputs that have a known
desired output value (ie. add 1 + 3; the inputs are 1 and 3, and we know that the correct answer
should be 4).

Why write tests:

- Code reliability. Does the code work the way it is was intended?
- Code maintainability. During refactoring or feature addition/removal does the code still do
  what it was supposed to do?
- Code longevity. During version updates, outputs of dependent packages can change. A properly tested
  code will ensure that all outputs of dependent packages still work for your code.
- Tested code will yield cleaner code as well. It will be come very apparent in practice
  that a single function that does everything will be much hard to test (and read) than that same code refactored
  into many smaller functions with explicit output.

Assertions: `Python.org <https://docs.python.org/3/library/2to3.html?highlight=assert#2to3fixer-asserts>`_


Setup your first test
---------------------
Although it is not necessary to create tests in another file, it is highly encouraged to keep
the code base nice and clean.

- The code we want to test: let's say the the following is in a file ``mycode.py``

.. code-block:: python

    # filename: mycode.py

    def adder(x,y):
        return x + y

- The test code: let's say the following is in a file ``test_mycode.py`` It is a good idea to
  keep write a "test" file for each "code" file. Code files should also be short (not 1000s of lines).

.. code-block:: python

    # filename: test_mycode.py

    # import the builtin test library
    import unittest
    # import your code module (mycode.py, assuming both mycode.py and test_myode.py is in the same folder)
    import mycode


    # setup the test class
    class test_mycode(unittest.TestCase):

        # setUp is optional: but if you are re-using inputs this saves you from retying them
        def setUp(self):
            self.input1 = 10
            self.input2 = 20

        # pytest looks for all methods that start with "test_"
        def test_one(self):
            # there are many different asserts, see full list above
            #  here we are testing if our function "adder" adds 10+20 correctly and equals 30
            self.assertEqual(mycode.adder(10,20),30)

        # we can write as many tests as we like,
        #   here is the same test input/out but with our setUp variables
        def test_two(self):
            self.assertEqual(mycode.adder(self.input1,self.input2),30)

- To run pytest, we type the following in the terminal

.. code-block:: shell

    python -m pip install pytest pytest-cov
    python -m pytest --cov-report=html --cov='.'

- The output will look something like:

.. code-block:: shell

    ============================= test session starts =============================
    platform win32 -- Python 3.8.0, pytest-5.2.2, py-1.8.0, pluggy-0.13.0
    rootdir: C:\Users\yourusername\Desktop
    collected 2 items

    test_mycode.py ..                                                        [100%]

    ============================== 2 passed in 0.03s ==============================

How to check if a function raises an error
------------------------------------------
Reusing the same example from ``mycode.py``

.. code-block:: python

    # filename: test_mycode.py

    import unittest
    import mycode


    class test_mycode(unittest.TestCase):

        def test_error(self):
            # to test a error raise, we have to enclose the code being testing a "with" block
            #  here we are testing if our code raises a TypeError when adding 10 + "20" as it should
            with self.assertRaises(TypeError):
                mycode.adder(10,"20")

How to report out code test coverage
------------------------------------
Code test coverage writes out a detailed report on what percent of your code the test actually executed.


.. code-block:: shell

    python -m pytest --cov-report=html --cov='.'

You can also write out a single ``xml`` coverage file. This is useful for CI (continuous integration)
since you only have to point your upload/file to 1 file.

.. code-block:: shell

    python -m pytest --cov-report=xml --cov='.'

To mock file-read without an actual file
----------------------------------------

.. code-block:: python

    f = io.StringIO("text\n")

    f.readline()
    >>> "text"