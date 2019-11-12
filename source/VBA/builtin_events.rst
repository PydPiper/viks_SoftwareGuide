Events
------
Events allow you to have code triggered by specific actions.  In Excel, these could be:

- Sheet Level Events

  - Clicking or double-clicking on a cell
  - Changing a cell's value
  - Calculating the sheet
  
- Workbook Level Events

  - Adding or changing a sheet
  - Opening, saving, or closing a workbook
  
- UserForm Level Events

  - Initializing a userform
  - Clicking a button on a userform

Turning Events On/Off
+++++++++++++++++++++
Events can have a negative impact on user experience if they are triggered too often and/or have long runtimes.  
You could also find yourself writing an event that triggers itself or others unintentionally.  
For example, if you had the following ``Worksheet_Change`` event in your workbook, changing the value of any cell
would cause an infinite loop caused by the change event changing the value of the cell below and re-triggering itself.

.. code-block:: vbscript

    Private Sub Worksheet_Change(ByVal Target As Range)
        Target.Offset(1,0).Value = Target.Value
    End Sub

If you ever need to run some code without having an event fire you would use ``Application.EnableEvents = False``.  
Don't forget to turn events back on after your code runs using ``Application.EnableEvents = True``.  
Also, avoid the common pitfall of turning events off and not have an error handler to turn them back on when things go wrong!

Event Optimization
++++++++++++++++++
It's sometimes worthwhile to have kickout logic at the beginning of your event to handle cases where you don't want your code to run.
For example, say we only want to run some code if the user enters something into a single cell.  
We would want to kick out if the user changes more than one cell or if the post-change value was null.

.. code-block:: vbscript

    Private Sub Worksheet_Change(ByVal Target As Range)
        If Target.Count > 1 Then Exit Sub
        If Target.Value = "" Then Exit Sub
        'Code to run
    End Sub
