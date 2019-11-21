builtin - Runtime Errors
========================
By default, VBA will throw a dialog box with an error number and description when it encounters a runtime error.  
The user is asked to either end the code execution or debug the error themselves by going to the line where it broke.
This behavior can be changed using the command ``On Error``.

- ``On Error GoTo [Line]`` will cause the code to jump to a specific line.
- ``On Error Resume Next`` will force the code to continue running even if an error is encountered.
- ``On Error GoTo 0`` will reset the error handling behavior back to default.

The example below shows the general structure of a procedure with an error handler.  
If you run the procedure and enter a string (such as "hello world") when prompted for a number, our own error prompt will appear
and then the code will exit.

.. code-block:: vbscript

    Sub OnErrorGoToLine()
        Dim dVar As Double
        
        On Error GoTo ErrLine
        dVar = InputBox("Enter a number")
    
        MsgBox "Good job!"
    ExitLine:
        On Error GoTo 0
        Exit Sub
    ErrLine:
        MsgBox "That is not a number!"
        Err.Clear
        Resume ExitLine
    End Sub

Validation Errors
-----------------
You can use a similar structure and the ``GoTo`` command for handling errors caused by validation checks within your code.  
These are things that wouldn't trigger a runtime error, but maybe you wouldn't want to allow the rest of the code to execute 
if certain conditions aren't met.  Note that you have to use ``GoTo`` instead of ``Resume`` in your handler (``Resume`` only applies to runtime errors).

.. code-block:: vbscript

    Sub ValidationErrors()
        Dim vVar As Variant
        
        vVar = Inputbox("Enter an even positive whole number no larger than 10")
        
        If Len(vVar) = 0 Then GoTo ErrNothingEntered
        If Not IsNumeric(vVar) Then GoTo ErrNotNumeric
        If Not Int(vVar) = vVar Then GoTo ErrNotInteger
        If Not vVar/2 = Int(vVar/2) Then GoTo ErrNotEven
        If vVar <= 0 Then GoTo ErrNotPositive
        If vVar > 10 Then GoTo ErrTooLarge
        
        MsgBox "Good job!"
    ExitLine:
        Exit Sub
    ErrNothingEntered:
        MsgBox "You didn't enter anything..."
        GoTo ExitLine
    ErrNotNumeric:
        MsgBox "That isn't a number..."
        GoTo ExitLine
    ErrNotInteger:
        MsgBox "That isn't a whole number..."
        GoTo ExitLine
    ErrNotEven
        MsgBox "That is not an even number..."
        GoTo ExitLine
    ErrNotPositive
        MsgBox "That is not a positive number..."
        GoTo ExitLine
    ErrTooLarge:
        MsgBox "That is larger than 10..."
        GoTo ExitLine
    End Sub

Clean Ups
---------
One of the main reasons we want to have an error handler is to clean up our environment before allowing the code to exit.
For example, say we have some code that is manipulating a spreadsheet so we decide to turn off calculations and events to gain speed.
If our code breaks without an error handler and the user ends execution, those settings will remain off.  
We can use the ``ExitLine`` of our code to house our clean up items so this doesn't happen.

.. code-block:: vbscript

    Sub ErrorWithCleanUp()
        
        With Application
            .Calculation = xlCalculationManual
            .EnableEvents = False
            .ScreenUpdating = False
        End With
        
        On Error GoTo ErrLine
        'Some code that does stuff
        
    ExitLine:
        On Error GoTo 0
        With Application
            .Calculation = xlCalculationAutomatic
            .EnableEvents = True
            .ScreenUpdating = True
        End With
        Exit Sub
    ErrLine:
        MsgBox Err.Number & ": " & Err.Description
        Err.Clear
        Resume ExitLine
    End Sub

It's also good practice to close any hidden objects and release object variables from memory.

.. code-block:: vbscript

    Sub ErrorReleaseObjects()
        Dim xlApp As Object
        Dim xlWb As Object
        
        On Error GoTo ErrLine
        Set xlApp = CreateObject("Excel.Application")
        Set xlWb = xlApp.Workbooks.Open("C:\SomeRandomSpreadsheet.xlsx")
        
        'Some code that does stuff
        
    ExitLine:
        On Error GoTo 0
        If Not xlWb Is Nothing Then
            xlWb.Saved = True
            xlWb.Close
            Set xlWb = Nothing
        End If
        If Not xlApp Is Nothing Then
            xlApp.Quit
            Set xlApp = Nothing
        End If
        Exit Sub
    ErrLine:
        MsgBox Err.Number & ": " & Err.Description
        Err.Clear
        Resume ExitLine
    End Sub
    

