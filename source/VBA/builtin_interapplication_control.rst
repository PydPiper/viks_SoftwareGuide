builtin- Interapplication Control
=================================
One of the best things about VBA is that it's the common language used by all Microsoft Office Applications.  
If you master VBA in Excel, you could easily learn VBA for Access, Word, Outlook, Powerpoint, Visio, or Projects.
They each have their own libraries and differ only in terms of the Objects being manipulated, but the core language and
coding structure is exactly the same.  This also makes controlling one Office Application from another incredibly trivial.

Early Binding vs. Late Binding
------------------------------

The process of assigning an object to an object variable is called "binding".  In Early Binding (Static Binding), this occurs during
compile time.  In Late Binding (Dynamic Binding) , this doesn't happen until runtime. 

The benefit of Early Binding in VBA is that it'll ensure that you're using the proper Objects, Properties, and Methods 
as you code (ie. it will provide auto-complete options, auto-check syntax, and allows you to use built-in enumerations).  
The downside of Early Binding in VBAis that you will need to add appropriate library references in order for the code to 
compile at all.  
To add a reference,go to Tools > References and then check off the reference you want to add.  
In the example below, you will need to add the ``Microsoft Outlook X.X Object Library`` to your workbook before the code can work.

.. code-block:: vbscript

    Sub EarlyBindingExample()
        Dim olApp As Outlook.Application
        Dim olMsg As Outlook.MailItem
        
        Set olApp = CreateObject("Outlook.Application")
        Set olMsg = olApp.CreateItem(olMailItem)
            olMsg.Display
    End Sub

By contrast, the benefit of Late Binding is that you don't have to worry about having Library References in order for your code to work.
This becomes extremely helpful when you're building reusable code that may end up in many workbooks.  Late Binding allows you
to just copy and paste the code in that new workbook and have it work right away!  The downside of Late Binding is that you
don't get the help of the auto-complete, auto-checker, and must use values instead of the enumerations.

.. code-block:: vbscript

    Sub LateBindingExample()
        Dim olApp As Object
        Dim olMsg As Object
        
        Set olApp = CreateObject("Outlook.Application")
        Set olMsg = olApp.CreateItem(0)
            olMsg.Display
    End sub

.. note:: 

A happy medium might be to code using Early Binding and then replace your specific object variable references 
with just ``Object`` and then replace your enumerations with it's value before removing the library reference.  
You can check the value of an enumeration by typing in "?" followed by the enumeration in your immediate window (``?olMailItem`` = 0).

Setting An Application Instance
-------------------------------
The first thing you need to do to control another application is grab an instance of it.  
There are two ways to do this:

- Set olApp = CreateObject("Outlook.Application")
- Set olApp = GetObject(, "Outlook.Application")

As the syntax suggests, the first method will create a new instance while the second will try to grab an existing instance.
The second method is useful if you're trying to access something that's already open (a workbook for example), 
but comes with the risk of throwing an error if there is no existing instance to grab.  Usually, the first method is preferred
because it allows you to control an instance without impacting what the user is doing in their existing instance.

Note that the first method will create an instance that is hidden in the background.  To make it visible, set the Application's
Visible property = True.  Also, most application instances will remain open even after the code finishes executing (the exception is Outlook).
If your code is doing something to a Word document or Excel spreadsheet and you don't set it's visibility to True and don't quit
the application in the end, your user may end up with a number of open application instances running in the background.

Once you have the Application instance, you can access all of the Objects within that Application by drilling down into it's members!

Example #1: Sending an Outlook Email
------------------------------------
This code can be used from any Office Application to create an email in Outlook.

.. code-block:: vbscript

    Sub CreateEmail(ByVal sSubject As String, _
                    ByVal sHTMLBody As String, _
                    ByVal sTo As String, _
                    Optional ByVal sCC As String = "", _
                    Optional ByVal sBC As String = "", _
                    Optionl ByVal bSend As Boolean = False)
        Dim olApp As Object
        Dim olMsg As Object
        
        Set olApp = CreateObject("Outlook.Application")
        Set olMsg = olApp.CreateItem(0)
        
        With olMsg
            .Subject = sSubject
            .htmlBody = sHTMLBody
            .To = sTo
            .CC = sCC
            .BC = sBC
            If bSend Then
                .Send
            Else
                .Display
            End If
        End With
    
    ExitLine:
        Set olMsg = Nothing
        set olApp = Nothing
    End Sub
    
Example #2: Exporting an Access Table or Query to Excel
-------------------------------------------------------
This code can be used from an Access Database to export the contents of a table to query into an Excel spreadsheet.
Note: This is not the only way to export data from Access to Excel!

.. code-block:: vbscript
    
    Sub ExportData(ByVal sTableOrQuery As String)
        Dim xlApp As Object
        Dim xlWb As Object
        Dim rst As Recordset
        Dim fld As Field
        Dim iFld As Integer
        
        Set rst = DBEngine(0)(0).OpenRecordset(sTableOrQuery, dbOpenSnapshot)
        Set xlApp = CreateObject("Excel.Application")
            xlApp.Visible = True
        Set xlWb = xlApp.Workbooks.Add
            With xlWb.Sheets(1)
                For Each fld In rst.Fields
                    iFld = iFld + 1
                    .Cells(1, iFld).Value = fld.Name
                Next
                .Cells(2,1).CopyFromRecordset rst
            End with
    
    ExitLine:
        rst.Close
        Set rst = Nothing
        Set xlWb = Nothing
        Set xlApp = Nothing
    End Sub
