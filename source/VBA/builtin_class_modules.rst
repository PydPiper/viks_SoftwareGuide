Class Modules
-------------
Class Modules allow you to create your own objects in VBA.  Similar to built-in objects like the Workbook, Worksheet, or Range object, 
Class Module objects can have their own set of properties and methods.

There are four different items in a class module:

- Member Variables
- Properties
- Methods
- Events

To start, let's create a new Class Module in a workbook named ``clsClient``.  Let's also create a new regular Module so we can test things.

Member Variables
++++++++++++++++
Member Variables are dimensioned using ``Private`` or ``Public`` within your Class Module.  If they are dimensioned using ``Public``,
they can be accessed and manipulated from outside the Class Module.

.. code-block:: vbscript

    'Class Module: clsClient
    Public FullName AS String


.. code-block:: vbscript

    'Regular Module
    Sub TestMyClass()
        'Create Object from Class Module
        Dim oClient As New clsClient

        'Assign value to Public Member Variable
        oClient.FullName = "John Doe"

        'Retrieve value from Public Member Variable
        Debug.Print oClient.FullName
    End Sub
    
It is usually best practice, however, to use ``Private`` Member Variables and then use Class Properties to set and get information.

Properties
++++++++++
The three commands for using properties in a Class Module are:

- Get: Returns an object or value
- Let: Sets a value
- Set: Sets an object

Let's turn our Member Variable "FullName" into a Class Property.

.. code-block:: vbscript

    'Class Module: clsClient
    Private msFullName As String
    
    Public Property Get FullName() As String
        FullName = msFullName
    End Property
    
    Public Property Let FullName(ByVal sValue As String)
        msFullName = sValue
    End Property

Your existing code in the regular class module will still work just the same, but if you step through it, you'll see how
the property "FullName" is set when the value is assigned and retrieved when printed.

You can create ReadOnly properties by just using Get without Let.

.. code-block:: vbscript

    'Class Module: clsClient
    Private msFullName As String
    
    Public Property Get FullName() As String
        FullName = msFullName
    End Property
    
    Public Property Let FullName(ByVal sValue As String)
        msFullName = sValue
    End Property
    
    Public Property Get FirstName() As String
        FirstName = Left(FullName, Instr(FullName, " ") - 1)
    End Property
    
    Public Property Get LastName() As String
        LastName = Right(FullName, Len(FullName) - Instr(FullName, " "))
    End Property

.. code-block:: vbscript

    'Regular Module
    Sub TestMyClass()
        'Create Object from Class Module
        Dim oClient As New clsClient

        'Assign value to Public Member Variable
        oClient.FullName = "John Doe"

        'Retrieve value from Properties
        Debug.Print oClient.FullName
        Debug.Print oClient.FirstName
        Debug.Print oClient.LastName
    End Sub

Methods
+++++++
Class Methods are ``Subs`` or ``Functions`` in a Class Module.

.. code-block:: vbscript

    'Class Module: clsClient
    Private msFullName As String
    
    Public Property Get FullName() As String
        FullName = msFullName
    End Property
    
    Public Property Let FullName(ByVal sValue As String)
        msFullName = sValue
    End Property
    
    Public Property Get FirstName() As String
        FirstName = Left(FullName, Instr(FullName, " ") - 1)
    End Property
    
    Public Property Get LastName() As String
        LastName = Right(FullName, Len(FullName) - Instr(FullName, " "))
    End Property
    
    Public Sub ExportToTextFile()
        Dim sFile As String
        
        sFile = Application.DefaultFilePath & "\client.txt"
        Open sFile For Output As #1
            Write #1, "First: " & FirstName
            Write #1, "Last: " & LastName
        Close #1
        MsgBox "Exported to " & sFile & "!"
    End Sub
    
.. code-block:: vbscript

    'Regular Module
    Sub TestMyClass()
        'Create Object from Class Module
        Dim oClient As New clsClient

        'Assign value to Public Member Variable
        oClient.FullName = "John Doe"

        'Export Client to Text File
        oClient.ExportToTextFile
    End Sub
    
Events
++++++
A Class Module has two events:

- Initialize: Triggered when new object of class is created
- Terminated: Triggered when class object is deleted

In other programming languages, these may be referred to as the ``Constructor`` and the ``Destructor``.
However, you cannot pass parameters to ``Initialize`` like you would a ``Constructor``.

Add these to the bottom of your class module code:

.. code-block:: vbscript

    Private Sub Class_Initialize()
        MsgBox "Class Initialized"
    End Sub
    
    Private Sub Class_Terminate()
        MsgBox "Class Terminated"
    End Sub
    
    
