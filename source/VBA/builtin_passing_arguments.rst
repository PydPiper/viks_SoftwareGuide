builtin - Subroutines and Functions
===================================
The main difference between a subroutine and function is that a function returns a value while a subroutine does not.
When calling one subroutine from another, you would use the ``Call`` command (Note: the code can run without using ``Call``, 
but it's much easier to understand what's happening when its there).  To use a function you created, just assign it
to a variable like any other function.

.. code-block:: vbscript

    Sub MyFirstSub()
        Call MySecondSub
    End Sub
    Sub MySecondSub()
        x = MyFunction()
        Debug.Print x
    End Sub
    Function MyFunction() As String
        MyFunction = "Hello World!"
    End Function
    
Input Arguments
---------------
Both subroutines and functions can take in any number of arguments.  These can either be passed by value (``ByVal``)
or by reference (``ByRef``).  ``ByVal`` will send the value of the variable while ``ByRef`` will send the variable itself.
In both cases, you should also define the type of variable being passed.

.. code-block:: vbscript

    Sub Main()
        Dim x As Integer
        x = 3
        Debug.Print TripleByVal(x) ' this return 3*3=9
        Debug.Print x              ' this returns the original x=3
        Debug.Print TripleByRef(x) ' this return 3*3=9, but...
        Debug.Print x              ' it ByRef also returns any changes to variable, hence x=9
    End Sub
    Function TripleByVal(ByVal x As Integer) AS Integer
        x = x * 3
        TripleByVal = x
    End Function
    Function TripleByRef(ByRef x As Integer) AS Integer
        x = x * 3
        TripleByRef = x
    End Function

If you run the above ``Main()`` subroutine, you should see the following numbers printed to your immediate window: 9, 3, 9, 9.
You can see how passing ``ByRef`` passed the variable itself so it retains any changes made to it.
By default, VBA will pass arguments ``ByRef`` so be sure to use ``ByVal`` if you don't want that behavior.

Optional Arguments
------------------
You can define arguments as optional if you don't always need it.  It is usually a good idea to define a default value
to use if the argument is ommited.  If you don't, VBA will apply it's own default value (usually an empty string or 0).

.. code-block:: vbscript

    Sub Main()
        MsgBox MyGreeting("John")
        MsgBox MyGreeting()
    End Sub
    Function MyGreeting(Optional ByVal UserName As String = "Human") As String
        MyGreeting = "Hello " & UserName & "!"
    End Function
