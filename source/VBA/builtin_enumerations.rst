Enumerations
------------
Enumerations are constants which represent a set of possible values.  
Microsoft has a number of built-in enumerations so that we developers can remember the names instead of numbers.
The following are all examples of built-in enumerations:

- xlCalculationManual
- xlCalculationAutomatic
- xlCalculationSemiautomatic
- vbYesNo
- vbOKOnly

You can use your immediate window to see the value behind an enumeration.  ``?xlCalculationManual`` will print out ``-4135``.

Enum Statement
++++++++++++++
You can define your own enumeration set with the ``Enum`` statement.  By default, the enumerated values will start at 0
and increment upwards, but you can override this with your own values.
As an example, say you want to take some action depending on what button a user presses on a MsgBox.  
The MsgBox, when used as a function, will return an integer, but we can define an enumeration set to use instead.

.. code-block::vbscript

    Public Enum vbMsgBoxResult
        vbOK = 1
        vbCancel = 2
        vbAbort = 3
        vbRetry = 4
        vbIgnore = 5
        vbYes = 6
        vbNo = 7
    End Enum
    Sub WithoutEnum()
        Select Case Msgbox("Click Yes or No", vbYesNo)
            Case 6
                Msgbox "You clicked Yes!"
            Case 7
                Msgbox "You clicked No!"
        End Select
    End Sub
    Sub WithEnum()
        Select Case Msgbox("Click Yes or No", vbYesNo)
            Case vbYes
                Msgbox "You clicked Yes!"
            Case vbNo
                Msgbox "You clicked No!"
        End Select
    End Sub

Enumerations as Arguments
+++++++++++++++++++++++++
You may have noticed by now that visual basic will help you auto-complete your code by displaying available options.
For example, if you type ``Application.Calculation=``, you should see the list of calculation options you can use.
We can do the same by creating an enumeration set and then defining it as an input to a sub or function.

.. code-block:: vbscript
    Public Enum xxArgument
        xxOption1 = 1
        xxOption2
        xxOption3
    End Enum
    Function MyFunction(ByVal InputOption As xxArgument) As String
        MyFunction = "You used option " & InputOption
    End Function

Put the above code into a new module and then type ``?MyFunction(`` into your immediate window.  It should display the three
options we created as potential inputs to help auto-complete!
