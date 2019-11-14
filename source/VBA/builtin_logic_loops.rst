builtin - Logic Loops
=====================

True False
----------
In VBA, the following all evaluate to True:

- -1=True
- 0=False

If ElseIf Else
--------------

.. code-block:: vbscript

    Sub IfElseIfElse()
        x = Inputbox("Enter a number between 1 and 10")
        If x < 5 Then
            MsgBox "x was less than 5"
        ElseIf x < 7 Then
            MsgBox "x was greater than or equal to 5 but less than 7"
        Else
            MsgBox "x was greater than or equal to 7"
        End If
    End Sub


Select Case
-----------

.. code-block:: vbscript
  
    Sub SelectCase()
        x = Inputbox("Enter either 'Cat', 'Dog', 'Bush', or 'Tree'")
        Select Case x
            Case "Cat", "Dog"
                MsgBox "That is an animal"
            Case "Bush", "Tree"
                MsgBox "That is a plant"
            Case Else
                MsgBox "I do not know what that is"
        End Select
    End Sub


For Loops
---------
  
.. code-block:: vbscript

    Sub ForLoops()
        'Basic Incremental Loop
        For i = 0 To 10
            Debug.Print i
        Next
        
        'Custom Step Size Loop
        For i = 0 To 10 Step 2
            Debug.Print i
        Next
        
        'Reverse Loop
        For i = 10 To 0 Step -1
            Debug.Print i
        Next
        
        'For Each Loop
        For Each xlSht In ThisWorkbook.Sheets
            Debug.Print xlSht.Name
        Next
        
        'Exiting For Loop Early
        For i = 0 To 10
            Debug.Print i
            If i = 5 Then
                Exit For
            End If
        Next
    End Sub
    
    
Do Loops
--------
There are two forms of the Do While/Do Until Loops.  The difference is that one will evaluate the criteria 
before entering the loop while the other won't evaluate the criteria until at the end of the loop.
In the example script below, try changing the value of x at the start of each loop to 5 and see what happens.

.. code-block:: vbscript

    Sub DoLoops()
        'Do Until Loop #1
        x = 1
        Do Until x = 5
            x = x + 1
            Debug.Print x
        Loop
        
        'Do Until Loop #2
        x = 1
        Do
            x = x + 1
            Debug.Print x
        Loop Until x = 5
        
        'Do While Loop #1
        x = 1
        Do While x < 5
            x = x + 1
            Debug.Print x
        Loop
        
        'Do While Loop #2
        x = 1
        Do
            x = x + 1
            Debug.Print x
        Loop While x < 5
    End Sub
  
  
Looping Through Files in a Folder
---------------------------------
This is a method of looping through files in a folder using the ``Dir()`` function.

.. code-block:: vbscript

    Sub FileLoop()
        Dim MyFile As String
        
        'Looping through all files
        MyFile = Dir("C:\", vbNormal)
        Do While Len(MyFile) > 0
            Debug.Print MyFile
            MyFile = Dir()
        Loop
        
        'Looping through .csv files
        MyFile = Dir("C:\*.csv", vbNormal)
        Do While Len(MyFile) > 0
            Debug.Print MyFile
            MyFile = Dir()
        Loop        
    End Sub
