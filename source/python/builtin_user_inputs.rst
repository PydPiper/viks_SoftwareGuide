builtin - User Inputs
=====================

Runtime Inputs
--------------

.. code-block:: python

    # python3: runtime input (stops your code and waits for a user input)
    store = input("Text asking for input: ")

- python hack for "switch" statement

.. code-block:: python

    # python doesnt have a "switch" keyword function builtin, but you are given the power to forge one!
    #   lets say you have multiple options on input
    print("Options:\n"
          "1) Main Menu\n"
          "2) Names\n"
          "3) Addresses\n")
    # functions that handle each menu:
    def main_menu():
        pass
    def names():
        pass
    def addresses():
        pass
    # switch statement to pipe the inputs
    piper = {"1": main_menu,
             "2": names,
             "3": addresses}
    # the dict stores the function objects, and piper[input]() then calls the function upon valid input
    piper.get([input("Enter option 1, 2, or 3: ")], "Invalid Input")()


Commandline Inputs
------------------

.. code-block:: python

    # ask for user inputs when calling the script
    #   note that sys.argv[0] is the name of the script (in this example "script.py")

    # access argument via sys
    import sys

    # we are looking for 3 inputs, else give guidance for proper input format
    if len(sys.argv) == (1+3):
        name_first = sys.argv[1]
        name_last = sys.argv[2]
        age = sys.argv[3]
    else:
        print("Script Usage:\n"
              "script.py arg1 arg2 arg3\n"
              "where,\n'
              "arg1 == First name\n"
              "arg2 == Last name\n"
              "arg3 == age\n")