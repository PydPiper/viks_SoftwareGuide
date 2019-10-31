builtin - Functions
===================
.. note:: Note that all functions in python end with a return, even when a ``return`` is not explicitly typed out in your code
          (of course this is only true if an exception is not raise within the function). When a ``return`` is not present within
          a function, the function will simply ``return None``.

.. note:: Note that local variable data will only be stored in memory while the code is within the function loop. Meaning,
          a variable within a function can only be called within the function. See :ref:`ref1`

Syntax
------

.. code-block:: python
    :linenos:

    # bare minimum
    def foo(arg1, arg2):
        return arg1 + int(arg2)

    # python version 3.5+ added type-hints that improves your editors inline error handling,
    #  testing, linters, etc.
    # to be explicit: everything on line 7 is type-hints. Lines 8-14 is your docstring
    #  (for when you call help(foo), or autodoc)
    def foo(arg1: int, arg2: str="0") -> int:
        """
        Simple add function

        :param (int) arg1: positional argument one
        :param (str) arg2: positional argument two
        :return (int): addition of arg1 + arg2
        """

        return arg1 + int(arg2)

    # lamba function (no-name function, or inline function definition)
    lambda arg1, arg2: arg1 + int(arg2)

All about variables
-------------------
- mandatory vs optional arguments

    - see line 7 from syntax example
    - arg1 is mandatory because it has no predefined value
    - arg2 is optional because it has a predefined value of "0", therefore foo can be called by
      ``foo(1)`` or ``foo(1,"0")`` both returning the same value

- positional vs. keyword arguments

    - data must be entered in a positional order, unless it's fed with keyword arguments (see example below)

.. code-block:: python

    # from the syntax example above with foo():

    # call a function with positionals
    foo(1,"2")
    >>> 3

    # call a function by unpacking positionals
    mylist = [1, "2"]
    foo(*mylist)
    >>> 3

    # call a function by keywords
    foo(arg2="2", arg1=1)
    >>> 3

    # call a function by unpacking keywords
    mydict = {"arg2":"2, "arg1": 1}
    foo(**mydict)
    >>> 3

.. _ref1:

Common pitfall - Global vs Local vs NonLocal
--------------------------------------------

- General Global variable

.. code-block:: python

    # global variable
    x = 5

    def func():
        print(x)

    # by default, a function will only be able to access argument variable or variable defined
    #  within the function
    func()
    >>> UnboundLocalError: local variable 'x' referenced before assignment

    def func(y):
        z = 15
        print(y,z)

    # now both variable y and z are accessible, so there are no errors
    func(y=10)
    >>> 10 15

- Accessing a Global Variable

.. code-block:: python

    # to access a global variable defined outside of the function
    def func():
        global x    # make it accessible within the function
        print(x)
        x = 500

    # using "global" we can allow our function to reach outside the local variables and
    #  grab the variable "x"
    x = 5
    func()
    >>> 5
    # notice no error this time, but be careful, the function also altered the value of "x"
    # note that "x" in the function is no longer a "local" variable, x is now globally redefined!
    x
    >>> 500

- Accessing a Nonlocal Variable (nested functions or loops within a function)

.. code-block:: python

    # alternatively we can access variables from nested functions via "nonlocal"
    def func():
        y = 5
        def func2():
            nonlocal y
            print(y)

    func()
    >>> 5

Call function by its string name
--------------------------------
- Call a function by string when it is imported with ``getattr()``

.. code-block:: python

    import file1

    # in this case: getattr(module, a function is a property of a module)(arguments for function)
    getattr(file1 , "foo")(1,"2")
    >>> 3

- Call a function by string in the same file with ``globals()`` ``locals()``

.. code-block:: python

    # suppose we have a simple add function:
    def func(a,b):
        return a + b

    # locals and globals will return the same here
    locals()["func"](1,2)
    >>> 3
    globals()["func"](1,2)
    >>> 3

    # the difference between locals and globals comes in when a property is nested
    def nest():
        def func(a,b):
            return a - b
        # locals will look in the current layer (ie. within nest())
        print("from local: ", locals()["func"](1,2))
        # globals will look at the module layer
        print("from global: ", globals()["func"](1,2))

    nest()
    >>> 'from local: -1'
    >>> 'from global: 3'

Function dundur
---------------

.. code-block:: python

    # string name of a function
    foo.__name__
    >>> 'foo'

    # list of arguments of a function
    foo.__code__.co_varnames
    >>> ('arg1', 'arg2')


functional programming: map, filter, and reduce
-----------------------------------------------

.. code-block:: python

    # map works-on multiple iterables at the same time
    #   take the following 2 lists and a simple add function for instance
    a = [1,2,3]
    b = [4,5,6]
    def add(x,y):
        return x + y
    list(map(add, a, b))
    >>> [5, 7, 9] # 1+4, 2+5, 3+6

    # use filter to narrow down a iterable with a custom true/false function
    a = [1,2,3]
    def test_odd(x):
        return x % 2 # returns 0 for even (same as true), 1 for odd (same as false)
    list(filter(test_odd, a))
    >>> [1, 3]

    # use reduce to narrow down a iterable to a single value
    a = [1,2,3]
    def multiply(x,y):
        return x*y
    reduce(multiply, a)
    >>> 6

functional programming - factory/closures/currying
--------------------------------------------------
Factory - A function that keeps its own internal state (see example below)
Closure - A "Factory" assigned to a variable
Currying - Similar to a "Closure" but input arguments changes the functionality of the Closure

- Simple Function (NOT A FACTORY example to stage set)

.. code-block:: python

    # the following function is not a "factory" because its state is not internal
    #   meaning, that each instance of a function will carry the same state, see demo below:
    counter = 0 # some global initial state
    def incrementer():
        global counter # allow access to global variable "counter" within the function
        counter += 1 # increment the "state", but note that counter is linked to a global state
        return counter

    incro1 = incrementer() # if incrementer was a "factory", incro1 would be called a "Closure"
    incro2 = incrementer()

    # it doesnt matter that we have 2 instances of the function,
    #   their "state" is linked to a global variable
    incro1 # incro1 was assigned as a function, therefore calling the function doesnt require another "()"
    >>> 1

    # we would expect that incro2 would also return 1 but their "state" is linked
    incro2
    >>> 2

- Factory Function

.. code-block:: python

    # this is what makes factories unique from regular functions,
    #   their internal state unique to each instance
    def incrementer():
        counter = 0 # internal state set
        def return_func():
            nonlocal counter # allow access to one level higher variable; ie. "counter"
            counter += 1 # change the internal state
            return counter # this is whats ultimately returned when called by a "Closure"
        # this is the tricky part...
        # when incrementer is initially defined, it runs through the code inside the function
        # sets initial "counter" state to 0
        # setups up a function "return_func()" but does nothing with it
        # then! the "Closure" variable is actually == the "return_func"
        # this is why counter = 0 is never reset after initialized, because
        # calling the "Closure" variable is actually calling "return_func"
        return return_func

    # lets see this in practice...
    incro1 = incrementer() # incro1 is a "Closure" that sets counter = 0 and returns incro1=return_func
    incro2 = incrementer() # lets make a second copy to demonstrate that "state" is unique

    # note that in this case we have to put "()" since incro=return_func,
    #   and to call return_func we need: return_func()
    incro1()
    >>> 1
    incro2()
    >>> 1
    incro2()
    >>> 2
    incro2()
    >>> 3
    incro1()
    >>> 2 # indeed state is unique to each instance!

- Currying Function (special Closure)

.. code-block:: python

    # now is the best time to show a builtin - library shortcut from "functools" called "partial"
    #   "partial" creates a "Closure" for you
    from functools import partial

    def multiply(x, n=1):
        return x * n

    # times3 is a unique "Closure" created from a "Factory" of multiply
    times3 = partial(multiply, n=3)
    # another unique "Closure" built from the same "Factory" but with different function
    times5 = partial(multiply, n=5)

    times3(2)
    >>> 6
    times5(2)
    >>> 10

Trick - Clean Function Piping
-----------------------------
Ever need to rip through a bunch of "if" statements to call the function you want? Try combing a piping dictionary with
function calls.

.. code-block:: python

    def func_one(a,b):
        return a+b

    def func_two(a,b):
        return a-b

    def func_three(a,b):
        return a*b


    def math(val1: float=0.0, val2: float=0.0, condition: str="one") -> float:
        """
        Takes a value and multiplies it by a string amount

        :param (float) val: input value
        :param (str) multi: multiplier in string
        :return (float): multiplied input value
        """

        piper = {"one": func_one,
                 "two": func_two,
                 "three": func_three,}

        try:
            # instead of coding up a bunch of if condition == something, you can make use of a
            #  dict's keyword arguments to pipe for you
            return piper[condition](val1, val2)
        except KeyError:
            raise UserWarning(f"Incorrect input value for condition={condition}")


Trick - Define function via text
--------------------------------

.. code-block:: python

    # use exec() to execute text and add it to the global variables
    exec("def f(x): return x*2", globals())
    f(5)
    >>> 10