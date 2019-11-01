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
    
Run your first script
---------------------
VBA code can be stored in Application Objects (eg. Sheet1, Sheet2, Sheet3, ThisWorkbook), 
Userforms, Modules, or Class Modules.  The most general place to add code is within a basic Module.

1)  Go to ``Insert > Module`` and a new Module called 'Module1' should appear in your ``Project Explorer`` and open for editing.
    Let's change the name of this Module by using the ``Properties Window`` to change ``(Name)`` from 'Module1' to 'MyFirstScript'.
2)  Now click in the main window and type in:

.. code-block:: vba

  Sub HelloWorld()
    Debug.Print "Hello World!"
  End Sub

3)  Run your script by clicking on the green Play button in the toolbar or by hitting ``F5``.  
    Note that you will need to have your mouse cursor somewhere in the script you want to run.  
    Otherwise, a pop-up will appear asking you to choose which script to run.
    You should see ``Hello World!`` get printed to your ``Immediate Window``.
4)  You can also step through your code line-by-line using ``F8``.  Give it a try!

Expanding on your first script
------------------------------
Let's now build upon our first script to learn the basics of commenting, variable declaration, debugging, and user inputs.

1)  Let's add a comment and a string variable to hold our message.  Comments are initiated by a single quote.
    Unfortunately, the concept of Comment Blocks do not exist in VBA.

.. code-block:: vba

  Sub HelloWorld()
    'This was my very first VBA script!
    Dim msg As String
    
    msg = "Hello World!"
    Debug.Print msg
  End Sub

2)  Let's try out some debugging techniques.  
    
    2.1)  Click on the grey bar to the left of ``Debug.Print msg`` to add a breakpoint.
          Alternatively, click on that line and hit ``F9``.  Now run your script and 
          it will stop right before executing that line of code (it will be highlighted yellow and nothing would have printed).
    2.2)  In your ``Immediate Window``, type in ``?msg`` and hit enter.  This will return the value stored in your variable ``msg``.
          You could also hover your mouse over ``msg`` in your script and a tooltip will appear showing it's value.
    2.3)  Now in your ``Immediate Window``, type ``msg = "Bonjour World!"``.  This reassigns the value stored in your variable.
          If you allow the script to finish executing by hitting Play or ``F5``, it will print the new value we just assigned.

3)  Let's now grab some info from our user to make our greeting a little more personalized.  
    To do this, we'll also need to concatinate our strings together using ``&``.
    Finally, we're also going to have the message pop up instead of print out.

.. code-block:: vba

  Sub HelloWorld()
    'This was my very first VBA script!
    Dim msg As String
    Dim user As String
    
    user = Inputbox("What's your name?")
    msg = "Hello " & user & "!"
    MsgBox(msg)
  End Sub

