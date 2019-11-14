10min - VBA Starter Kit
=======================
This starter kit is meant for readers that have never used VBA and have very little knowledge of programming concepts.

Initial Set-Up
--------------
All Microsoft Office Applications can be controlled using Visual Basic for Applications (VBA).  
The basic framework is the same whether you're coding in Excel, Word, Access, Outlook, Powerpoint, Visio, or Projects.  
The main difference is that each application has it's own set of unique objects to work with.
The majority of this guide will cover VBA in Excel, but feel free to explore the capabilities of the other office products.

1)  We'll first want to add the ``Developer Tab`` to our ribbon.  Open Excel and go to ``File > Options > Customize Ribbon``, 
    check off ``Developer`` and hit ``OK``.  You should now see a ``Developer Tab`` that you can use to open up the 
    ``Visual Basic`` window.  Note:  You can also use hotkeys ``Alt+F11`` to open up the editor.
2)  Next, we will want to configure our ``Visual Basic`` window to show some helpful tools and panels.
    This is mostly to preference so feel free to add and configure to whatever you're most comfortable with.
    Go to ``View`` and add the ``Immediate Window`` and ``Properties Window``.  
    You should already have the ``Project Explorer`` displayed with a tree structure of your workbook, but if you don't, 
    you can add it by going to ``View`` and then ``Project Explorer``.  
    Finally, we're going to add the ``Edit Toolbar`` since it gives us some quick ways to format our code.
    Go to ``View > Toolbars > Edit`` and then drag it to the top where the rest of the toolbar lives.
    
Running your first script
-------------------------
VBA code can be stored in Application Objects (eg. Sheet1, Sheet2, Sheet3, ThisWorkbook), 
Userforms, Modules, or Class Modules.  The most general place to add code is within a basic Module.

1)  Go to ``Insert > Module`` and a new Module called 'Module1' should appear in your ``Project Explorer`` and open for editing.
    Let's change the name of this Module by using the ``Properties Window`` to change ``(Name)`` from 'Module1' to 'MyFirstScript'.
2)  Now click in the main window and type in:

.. code-block:: vbscript

  Sub HelloWorld()
    Debug.Print "Hello World!"
  End Sub

3)  Run your script by clicking on the green Play button in the toolbar or by hitting ``F5``.  
    Note that you will need to have your mouse cursor somewhere in the script you want to run.  
    Otherwise, a pop-up will appear asking you to choose which script to run.
    You should see ``Hello World!`` get printed to your ``Immediate Window``.
4)  You can also step through your code line-by-line using ``F8``.  Give it a try!


Introduction to Data Variables
------------------------------
Data Variable Types
+++++++++++++++++++

.. note:: These are just some of the most commonly used variables.  For the full list of Data Varaible Types see Microsoft's `Data Type Summary <https://docs.microsoft.com/en-us/office/vba/language/reference/user-interface-help/data-type-summary>`_

-   ``String``: Denoted by double-quotes.  "This is a string"
-   ``Integer``: Whole number between -32,768 and 32,767
-   ``Long``: Whole number between -2,147,483,648 and 2,147,483,647
-   ``Double``: Double-precision floating point
-   ``Boolean``: ``TRUE`` or ``FALSE``
-   ``Date``: January 1, 100 to December 31, 9999
-   ``Variant``: Special variable that can hold any data type
    
Variable Declaration & Scope
++++++++++++++++++++++++++++
There are three ways to declare a variable (each defining a different level of scope).

-   ``Dim``: Procedure level (local) variable.  Must be declared within the procedure using it.
-   ``Private``: Module level variable.  Visible to any procedure within the module.  Must be declared at the top of the module.
-   ``Public``: Global level variable.  Visible to any module within the project.  Must be declared at the top of the module.

.. code-block:: vbscript

    Public gMyPublicVar As Variant
    Public mMyPrivateVar As Variant
    Sub MyProcedure()
        Dim MyLocalVar As Variant
    End Sub

Expanding on your first script (Pt. 1)
--------------------------------------
Let's now build upon our first script to apply what we've learned about variables and touch upon the basics of commenting, debugging, and user inputs.

1)  Let's add a comment and a string variable to hold our message.  Comments are initiated by a single quote.
    Unfortunately, the concept of Comment Blocks do not exist in VBA.

.. code-block:: vbscript

  Sub HelloWorld()
    'This was my very first VBA script!
    Dim msg As String
    
    msg = "Hello World!"
    Debug.Print msg
  End Sub

2)  Let's try out some debugging techniques.  
    
    2.1) Click on the grey bar to the left of ``Debug.Print msg`` to add a breakpoint.
    Alternatively, click on that line and hit ``F9``.  Now run your script and
    it will stop right before executing that line of code (it will be highlighted yellow and nothing would have printed).

    2.2) In your ``Immediate Window``, type in ``?msg`` and hit enter.  This will return the value stored in your variable ``msg``.
    You could also hover your mouse over ``msg`` in your script and a tooltip will appear showing it's value.

    2.3) Now in your ``Immediate Window``, type ``msg = "Bonjour World!"``.  This reassigns the value stored in your variable.
    If you allow the script to finish executing by hitting Play or ``F5``, it will print the new value we just assigned.

3)  Let's now grab some info from our user to make our greeting a little more personalized.  
    To do this, we'll also need to concatinate our strings together using ``&``.
    Finally, we're also going to have the message pop up instead of print out.

.. code-block:: vbscript

  Sub HelloWorld()
    'This was my very first VBA script!
    Dim msg As String
    Dim user As String
    
    user = Inputbox("What's your name?")
    msg = "Hello " & user & "!"
    MsgBox(msg)
  End Sub
  
Introduction to Objects, Properties, and Methods
------------------------------------------------
Objects
+++++++
::

    "Objects are the fundamental building block of Visual Basic;
    nearly everything you do in Visual Basic involves modifying objects.
    Every element of Microsoft Word - documents, tables, paragraphs, bookmarks, 
    fields, and so on - can be represented by an object in Visual Basic."
                                                            -Microsoft Dev Center
                                                            
-   An object can be a member of another object.  For example, the Sheet Object is a member of 
    the Workbook Object which is then a member of the Application Object.  To access an Object's 
    member, use a period (``Application.ThisWorkbook.ActiveSheet``).  
        -   In many cases, you don't need to explicitely define the full heirarchy down to the object you want work with.
            ``Application.ActiveWorkbook.ActiveSheet.Cells(1,1).Value = "Hello World!"`` is the fully defined heirarchy,
            but ``Cells(1,1).Value = "Hello World!"`` would work just the same.

Properties
++++++++++
::

    "A property is an attribute of an object or an aspect of its behavior.
    For example, properties of a document include its name, its content, and
    its save status, as well as whether change tracking is turned on.  To change
    the characteristics of an object, you change the values of its properties."
                                                            -Microsoft Dev Center

Methods
+++++++
::

    "A method is an action that an object can perform.  For example, just as a 
    document can be printed, the Document object has a PrintOut method. Methods
    often have arguments that qualify how the action is performed."
                                                            -Microsoft Dev Center

Object Variables
++++++++++++++++
Object variables allow you to store a reference to any object.  The main difference in using an object variable as opposed to a
data variable is that you need to use the word ``Set`` to assign something to it.

.. code-block:: vbscript

    Sub MyProcedure()
        Dim xlSht As Excel.Worksheet
        Dim sheetName As String
        
        Set xlSht = ActiveSheet
        sheetName = xlSht.Name
    End Sub

.. note:: This example uses early binding to declare the variable ``xlSht`` specifically as an ``Excel.Worksheet`` object. You could also use late binding to declare the variable as just an Object like ``Dim xlSht As Object``.  Early binding requires you to have the appropriate library references loaded beforehand.

.. note:: If you need help with any Object in VBA, your best resource is the ``Object Browser``.  Go to ``View > Object Browser`` or hit ``F2`` to open it up.  The ``Object Browser`` allows you to look up anything about an Object including it's Properties and Methods.

Expanding on your first script (Pt. 2)
--------------------------------------
Let's build upon our first script one last time to practice using Objects, Properties, and Methods.  First, we're going to read the user's name from the ``Value Property`` of the ``Range Object`` for Cell A1 and then we'll execute the object's ``ClearContents Method`` to clear out their name before displaying the message.  

.. code-block:: vbscript

  Sub HelloWorld()
    'This was my very first VBA script!
    Dim xlRng As Object
    Dim msg As String
    Dim user As String
    
    Set xlRng = ActiveSheet.Range("A1")
    user = xlRng.Value
    xlRng.ClearContents
    msg = "Hello " & user & "!"
    MsgBox(msg)
  End Sub
