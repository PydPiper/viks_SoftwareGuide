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


To mock file-read without an actual file
---------------------------------------

.. code-block:: python

    f = io.StringIO("text\n")

    f.readline()
    >>> "text"