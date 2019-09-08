Functions Quick User Guide
==========================

Syntax
------

.. code-block:: python
    :linenos:

    # bare minimum
    def foo(arg1, arg2):
        return arg1 + int(arg2)

    # python version 3.5+ added type-hints that improves your editors inline error handling, testing, linters, etc.
    # to be explicit: everything on line 7 is type-hints. Lines 8-14 is your docstring (for when you call help(foo), or autodoc)
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
    - arg2 is optional because it has a predefined value of "0", therefore when foo can be called
      ``foo(1)`` or ``foo(1,"0")`` both returning the same value

- positional vs. keyword arguments

    - data must be entered in a positional order, unless it's fed with keyword arguments (see example below)

.. code-block:: python
    :linenos:

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

Call function by its string name
--------------------------------
- Call a function by string when it is imported with ``getattr()``

.. code-block:: python
    :linenos:

    import file1

    # in this case: getattr(module, a function is a property of a module)(arguments for function)
    getattr(file1 , "foo")(1,"2")
    >>> 3

- Call a function by string in the same file with ``globals()`` ``locals()``

.. code-block:: python
    :linenos:

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
    >>> from local: -1
    >>> from global: 3

Function dundur
---------------

.. code-block:: python
    :linenos:

    # string name of a function
    foo.__name__
    >>> 'foo'

    # list of arguments of a function
    foo.__code__.co_varnames
    >>> ('arg1', 'arg2')

Trick - Clean Function Piping
-----------------------------
Ever need to rip through a bunch of "if" statements to call the function you want? Try combing a piping dictionary with
function calls.

.. code-block:: python
    :linenos:

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
            # instead of coding up a bunch of if condition == something, you can make use of a dict's keyword arguments to pipe for you
            return piper[condition](val1, val2)
        except KeyError:
            raise UserWarning(f"Incorrect input value for condition={condition}")

