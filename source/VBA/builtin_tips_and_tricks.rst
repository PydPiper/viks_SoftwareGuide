Tips and Tricks
===============
This section is a collection of tips and tricks to help make your code more stable, efficient, or supportable.

Named Ranges
------------
If you reference a range from code, it is highly advised that you use a named range instead of the range address.

- You can create a named range by selecting the range and then editing the area to the left of the formula bar

- Or by going to ``Formulas > Name Manager``.

Using named ranges will make your code much more stable as they will move around when changes
are made to your worksheet.  For example, say your code was grabbing a name from cell A1.  Now imagine you or another user
adds a new column or row above or to the left of A1.  Without named ranges, your code would still be looking for something in A1,
but that cell has now moved to A2 or B1.  With named ranges, your code would work without issue.

.. code-block:: vbscript

    ' code reference by address (not desirable)
    Sub UsingRangeAddress()
        Debug.Print Range("A1").Value
    End Sub

    ' code reference by namespace (preferred)
    Sub UsingNamedRanges()
        Debug.Print Range("UserName").Value
    End Sub
    
Calculations, Events, and Screen Updating
-----------------------------------------
If your code is manipulating your spreadsheet and you find it taking a long time to run, try turning off 
calculations, events, and screen updating.  Don't forget to turn them back on at the end!  

.. code-block:: vbscript

    Sub MyProcedure()
        With Application
            .Calculation = xlCalculationManual
            .EnableEvents = False
            .ScreenUpdating = False
        End With
        
        'Code to run
        
        With Application
            .Calculation = xlCalculationAutomatic
            .EnableEvents = True
            .ScreenUpdating = True
        End With
    End Sub
    
R1C1 Reference Style
--------------------
R1C1 is great for writing chunks of formula along a row or column.  You can switch between viewing your formulas in
R1C1 by going to ``File > Options > Formulas > R1C1 Reference Style``.  You do not need to have this option on to 
write formulas in R1C1.

For an example, say we want to write formulas in cells A2:Z2 to make them equal to the cell above them.

.. code-block:: vbscript
    
    Sub FormulasWithoutR1C1()
        For i = 1 To 26
            Cells(2, i).Formula = "=" &  Split(Cells(1, i).Address, "$")(1) & 1
        Next
    End Sub
    Sub FormulasWithR1C1()
        Range("A2:Z2").FormulaR1C1 = "=R[-1]C"
    End Sub
    
