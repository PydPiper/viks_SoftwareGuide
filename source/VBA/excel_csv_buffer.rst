excel - Buffer Data From .csv Without Opening
=============================================
A common output format for many different programs is the Comma Separated Values (csv). CSVs are
great and easy to work with, but in order to being working with the data in excel we first have to copy
it in. This can be a very annoying manual task that is also prone to mis-selection error. The following
piece of code fixes this problem by buffering the CSV file into excel without ever launching a window for
the csv!


- Step 1: Enable Microsoft Scripting Runtime. In the VBA Editor (Alt+F11) -> Tools -> References -> checkbox: Microsoft Scripting Runtime.
This will allow the object ``FileSystemObject`` to read data from the csv without pulling up a window. Note that this is a setting
for the excel file itself that has to be done 1 time.

- Step 2: Built the CSV to Excel buffer code. This can be in a macro. VBA Editor -> Insert -> Macro

.. code-block:: vbscript

    ' set excel attributes to manual for better performance
    Application.ScreenUpdating = False
    Application.Calculation = xlCalculationManual


    ' In case an csv filename was incorrect or doesnt exists in path
    On Error GoTo Error_FileNotFound

    ' Declare the workbook we are working on
    Dim WB as Workbook
    Set SB = ThisWorkBook

    ' As for CSV filename. Note that this does not need to be an input box. See more cleaver solution
    '  with Worksheet_Change and a drop-down menu
    Dim FileName as String
    FileName = InputBox("Enter the full csv file name. (ex: data.csv)")

    ' Get current workbook file path (assuming csv data file is in the same folder)
    Dim DirPath as String
    DirPath = WB.Path

    ' Full file path for the csv file
    Dim FilePath as String
    FilePath = dir_path & "/" & FileName

    ' Row/Column to start copying csv data to (this example "A1" or cell(1,1))
    Dim StartRow as Integer
    Dim StartCol as Integer
    StartRow = 1
    StartCol = 1

    ' Clear previous contents from cells (assuming we are buffering csv data to tab "Sheet1")
    Dim WS as WorkSheet
    Set WS = WorkSheets("Sheet1")
    WS.Cells.ClearContents


    ' Setup FileSystemObject that will buffer the csv file
    Dim FSO as FileSystemObject
    Set FSO = New FileSystemObject

    with FSO
        With .OpenTextFile(FilePath, ForReading)
            if Not .AtEndOfStream Then .SkipLine

            Do Until .AtEndOfStream
                ' split file by its delimiter, in this case "," for csv
                LineItems = split(.ReadLine, ",")
                ' get the max columns for the current line of csv textline to iterate over
                Dim MaxCol as Integer
                MaxCol = Ubound(LineItems) - LBound(LineItems) + 1
                ' iterate over each item and buffer the csv data into each excel column cell of the current row
                Dim col as Integer
                col = 0
                Do Until StartCol = MaxCol
                    ' on csv index starts at 0, we want ours to start at the specified StartCol
                    WS.Cells(StartRow + row, StartCol + col).Value = LineItems(col)
                    col = col + 1
                Loop
            row = row + 1
            Loop
        End With
    End With

    ' reset excel attributes
    Application.ScreenUpdating = True
    Application.Calculation = xlCalculationAutomatic


    Exit Sub

    Error_FileNotFound:
        MsgBox("Error - Incorrect file name and/or path. Double check .csv filename and path is correct")
        Exit Sub

    End Sub

