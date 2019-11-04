builtin - Context Manager
=========================
Context Manager is a function or class that has an "Enter" buildup code, and an
"Exit" teardown code that the user is able simply call in a "with" block. The most
common example of this is the builtin file IO class "open". Open has an "Enter" that
opens file streaming and an "Exit" that safely closes the file for streaming.

Object Oriented Version
-----------------------

- Setup

.. code-block:: python

    # context manager setup
    class MyContextManager():

        # predefine input on "with" code line
        def __init__(self, preload):
            print("context initialized on runtime")
            self.preload = preload

        # enter method is called when the code enters the with block
        def __enter__(self):
            print("inside with block")
            # keep the context manager class simple
            #   by returning another class that does the work from the __enter__ block
            # When a "with MyContextManger(10) mycont:" is called,
            #   we are really saying that mycont = worker_class(10)
            return worker_class(self.preload)

        # exit method is called when the code exits the with block
        def __exit__(self, exc_type,exc_val, exc_tb):
            # exc_type ==> exception type (ie. TypeError, ValueError, KeyError)
            # exc_val  ==> raise exception argument (ie. TypeError("this is the exc_val"))
            # exc_tb   ==> traceback message (where the in the code did the exception occur)

            # exit method receives arg containing details of exception raised in the "with" block
            #   if context can handlethe exception, __exit__() should return a true
            #   (ie. error does not need to be propagated)
            #   returning false causes the exception to be re-raised after __exit__()

            # this feature is really nice when trying to graciously handle exceptions
            if exc_type is ValueError:
                print("handled ValueError, raised exception value = ",exc_val)
                return True
            return False

- Extend the context manger functionality

.. code-block:: python

    # define worker class
    class worker_class():
        def __init__(self, preload):
            self.preload = preload
        def extended(self, val):
            print(val, self.preload)

- How to use it

.. code-block:: python

    # now to use it
    with MyContextManager(10) as mycont:
        # code without any errors
        mycont.extended(100)

    >>> "context initialized on runtime"
    >>> "inside with block"
    >>> 100, 10

    # how it works with handled error
    with MyContextManager(10) as mycont:
        # code without any errors
        mycont.extended(100)
        raise ValueError("HI!")

    >>> "context initialized on runtime"
    >>> "inside with block"
    >>> 100, 10
    >>> "handled ValueError, raised exception value =  HI!"

Functional Programming Version
------------------------------
We can accomplish the same task with a function definition as shown above and a library called
contextlib

- Define the worker class (same as above, this does not have to be a class object, this example
  is simply reusing the same code as above for clarity)

.. code-block:: python

    # define worker class (same as above)
    class worker_class():
        def __init__(self, preload):
            self.preload = preload
        def extended(self, val):
            print(val, self.preload)


- Setup the context manager function

.. code-block:: python

    from contextlib import contextmanager

    @contextmanager
    def MyContextManager(preload):
        # __init__ code goes here
        print("context initialized on runtime")
        try:
            print("inside with block")
            yield worker_class(preload)
        except ValueError as e:
            print("handled ValueError, raised exception value = ",e)
        finally:
            # __exit__ code goes here
            print("__exit__ cleanup code goes here")


- Use it

.. code-block:: python

    with MyContextManager(10) as mycont:
        # code without any errors
        mycont.extended(100)

    >>> "context initialized on runtime"
    >>> "inside with block"
    >>> 100, 10
    >>> "__exit__ cleanup code goes here"