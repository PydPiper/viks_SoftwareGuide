Database Management Systems
---------------------------
Access is Microsoft's database product.  It's important to note that, at it's core, Access is a Database Management System (DBMS).  
The application is controlled via Visual Basic, but the standard DBMS language is SQL.  
The GUI of Access allows users to operate it without knowledge of SQL, but it is highly recommended that you learn SQL if you plan
on controlling Access with VBA.  There are a few variations of SQL to be aware of.  The SQL used in Access has a few differences 
in syntax from the more robust T-SQL.  Below are a few examples:

Delimiters

- Text: Access accepts both ``"`` and ``'`` while T-SQL only accepts ``'``
- Date: Access uses ``#`` while T-SQL uses ``'``
- Wildcard: Access uses ``*`` while T-SQL uses ``%``

Functions

- Conditions: Access uses ``IIF()`` while T-SQL uses ``CASE, IF-THEN-ELSE``
- Conversions: Access uses VBA functions like ``CDate()``, ``CLng()``, ``CInt()`` while T-SQL uses ``CAST()``
- Nulls:  Access uses ``Nz()`` while T-SQL uses ``ISNULL()``

Access Objects
--------------
There are four main Access Objects.

- Tables
- Queries
- Forms
- Reports

Tables and Queries are similar in that they both represent data.  Forms and Reports are similar in that they both represent a UI.
As such, Tables and Queries share similar properties and methods as do Forms and Reports.  For this tutorial, we will only be
focusing on data manipulation (Queries).  

Queries are just stored pieces of SQL.  When a query is opened, it presents the user with the results of the SQL execution.
The query builder UI in Access allows users to drag and drop tables/queries, create joins, select fields, apply criteria, 
and sort their data.  All of these interactions with the UI are translated into SQL behind the scenes.  You can create a query
in the UI, and then go to ``SQL View`` to see the resulting SQL.  The important thing to remember is just that anything you can do
with a query, you can do by executing SQL.  Therefore, any automation of data in Acces using VBA involves executing SQL.

For the rest of the tutorial, we'll be using the following ``Customers`` dataset:

+------------+------------+-----------+-----------+-----------+
| ID         | Name       | State     | Date      | Sale      |
+============+============+===========+===========+===========+
| 1          | John Doe   | WA        | 1/1/2019  | 500.50    |
+------------+------------+-----------+-----------+-----------+
| 2          | Jane Smith | OR        | 2/1/2019  | 250.25    |
+------------+------------+-----------+-----------+-----------+
| 3          | Mike White | CA        | 3/1/2019  | 1000.00   |
+------------+------------+-----------+-----------+-----------+
| 4          | Mike Smith | OR        | 4/1/2019  | 300.00    |
+------------+------------+-----------+-----------+-----------+

Recordsets
++++++++++
The main object that you'll use in Access VBA are recordsets.  These are digital representations of data that you can use to
iterate through, add/modify records, or grab values to use in variables for the rest of your code.  

A recordset is created using the ``OpenRecordset`` method.  You can open a recordset as ReadOnly using ``dbOpenSnapshot``.
If you plan to make edits to the data, use ``dbOpenDynaset``.

Some important properties and methods for recordsets are listed below:

- ``BOF``: Beginning of file (Boolean Property)
- ``EOF``: End of file (Boolean Property)
- ``Edit``: Edit record (Method)
- ``AddNew``: Add new record (Method)
- ``Update``: Commit changes to record (Method)
- ``Delete``: Delete current record (method)
- ``MoveNext``: Move pointer to next record (Method)
- ``MoveFirst``: Move pointer to first record (Method)
- ``Close``: Close recordset connection (Method)

This example creates a recordset of Customers who live in either WA or OR and prints their names to the immediate window.
Note: A recordset that is both at the beginning of a file and the end of a file simultaneously is empty.

.. code-block:: vbscript

    Sub LoopThroughRecordset()
        Dim rst As Recordset
        Dim sql As String
        
        sql = "SELECT Name FROM Customers WHERE State In('WA','OR');"
        Set rst = DBEngine(0)(0).OpenRecordset(sql, dbOpenSnapshot)
        
        With rst
            If Not .BOF And Not .EOF Then
                Do
                    Debug.Print .Fields("Name").Value
                    .MoveNext
                Loop Until .EOF
            End If
        End With
    ExitLine:
        rst.Close
        Set rst = Nothing
    End Sub

This example uses a recordset to add a new customer to the table and then prints their new ID (ID must be an AutoNumber to work!).

.. code-block:: vbscript

    Sub AddCustomerRecordset()
        Dim rst As Recordset
        Dim sql As String
        Dim iID AS Long
        
        sql = "SELECT * FROM Customers;"
        Set rst = DBEngine(0)(0).OpenRecordset(sql, dbOpenDynaset)
        
        With rst
            .AddNew
            iID = .Fields("ID").Value
            .Fields("Name").Value = "Daniel Park"
            .Fields("State").Value = "CA"
            .Fields("Date").Value = CDate("5/1/2019")
            .Fields("Sale").Value = 777.77
            .Update
        End With
        Debug.Print iID
    ExitLine:
        rst.Close
        Set rst = Nothing
    End Sub

This example uses a recordset to update the state of anyone in CA to HI.

.. code-block:: vbscript

    Sub UpdateStateRecordset()
        Dim rst As Recordset
        Dim sql As String
        
        sql = "SELECT * FROM Customers;"
        Set rst = DBEngine(0)(0).OpenRecordset(sql, dbOpenDynaset)
        
        With rst
            If Not .BOF And Not .EOF Then
                Do
                    If .Fields("State") = "CA" Then
                        .Edit
                        .Fields("State") = "HI"
                        .Update
                    End If
                    .MoveNext
                Loop Until .EOF
            End If
        End With
    ExitLine:
        rst.Close
        Set rst = Nothing
    End Sub

DoCmd
+++++
If you're familiar with SQL, you might've noticed that the previous two examples are actually pretty inefficient  for what they're doing.
An similar thing could be accomplished using a single SQL statement representing an ``Action Query``.

The ``DoCmd`` class has a number of useful methods that can be used to automate behavior in Access.  
One of these methods is the ``DoCmd.RunSQL`` method.  Below is the last example to update States recreated using ``DoCmd.RunSQL``.

.. code-block:: vbscript

    Sub UpdateStateRunSQL()
        Dim sql As String
        
        sql = "UPDATE Customers SET State = 'HI' WHERE State = 'CA';"
        DoCmd.RunSQL sql
    End Sub
    
If you run this, you may notice a pop-up asking for confirmation on the change you're about to make.  To suppress this,
we can use the ``DoCmd.SetWarnings`` method.

.. code-block:: vbscript

    Sub UpdateStateRunSQL()
        Dim sql As String
        
        sql = "UPDATE Customers SET State = 'HI' WHERE State = 'CA';"
        DoCmd.SetWarnings False
        DoCmd.RunSQL sql
        DoCmd.SetWarnings True
    End Sub

There are many more methods of ``DoCmd``.  Check them out using the Object Browser!

DFunctions
++++++++++
Access has a few functions to look up and calculate statistics on data.  

- ``DLookup()``: Similar to ``VLookup()`` in Excel, but allows for multiple criteria
- ``DMin()``: Similar to ``Min()`` in Excel,  but allows for multiple criteria
- ``DMax()``: Similar to ``Max()`` in Excel,  but allows for multiple criteria
- ``DCount()``: Similar to ``CountIfs()`` in Excel
- ``DSum()``: Similar to ``SumIfs()`` in Excel

All of these functions have the same three arguments:

1.  FieldName
2.  TableName or QueryName
3.  Criteria

The example below uses ``DLookup()`` to get the first ID of a customer named Mike who lives in OR.  
It's important to also use the ``Nz()`` function to handle nulls if no record matches our criteria.
We'll just run this in the immediate window.

.. code-block:: vbscript

    ?Nz(DLookup("ID", "Customers", "Name = 'Mike*' AND State = 'OR'"),0)

This example uses ``DSum()`` to calculate the total sales in CA.  ``DCount()`` and ``DSum`` do not need an ``Nz()`` wrapper.

.. code-block:: vbscript

    ?DSum("Sale", "Customers", "State = 'CA'")

We can now recreate the second recordset example of adding a customer using ``DoCmd.RunSQL`` to append the record 
and ``DMax()`` to get the newly added ID.

.. code-block:: vbscript

    Sub AddCustomerRunSQL()
        Dim sql As String
        Dim iID AS Long
        
        sql = "INSERT INTO Customers ( Name, State, Date, Sale ) " & _
                "SELECT 'Daniel Park' AS Name, 'CA' As State, #5/1/2019# As Date, 777.77 As Sale;"
                
        DoCmd.SetWarnings False
        DoCmd.RunSQL sql
        DoCmd.SetWarnings True
        
        iID = DMax("ID", "Customers")
        Debug.Print iID
    End Sub

Access from Excel
-----------------
Below is a custom class module for Excel that allows you to interface and manipulate data stored in an Access Database or SQL Server
using Access-like syntax.  To use it, copy the code into a new class module and name it ``clsDB``.  
You will also need to add a reference to ``Microsoft Office X.X Access Database Engine Object Library``.

.. code-block:: vbscript
    
    'Class Module: clsDB
    'Author: Kevin Kim
    'Required Reference: Microsoft Office X.X Access Database Engine Object Library
    
    Private Enum dbCnnType
        dbCnnTypeAccess
        dbCnnTypeSQLServer
    End Enum
    Private mCnnType As Integer
    Private mTempDB As DAO.Database
    Private sConnection As String
    Private sDB As String
    Public Property Get Connection() As String
        Connection = sConnection
    End Property
    Public Property Get CnnType() As Integer
        CnnType = mCnnType
    End Property
    Public Property Let CnnType(aValue As Integer)
        mCnnType = aValue
    End Property
    Public Property Let Connection(aValue As String)
        If Len(Dir(aValue, vbNormal)) > 0 Then
            sDB = aValue
            CnnType = dbCnnTypeAccess
            sConnection = vbNullString
        Else
            CnnType = dbCnnTypeSQLServer
            sConnection = aValue
        End If
    End Property
    Public Function SQLServerConnection(ServerName As String, Database As String) As String
        SQLServerConnection = "ODBC;Driver={SQL Server};" & _
                                    "Server=" & ServerName & ";" & _
                                    "Database=" & Database & ";"
    End Function
    Private Function TempDB() As DAO.Database
        Dim oWS As DAO.Workspace
        Dim sTempDB As String

        If mTempDB Is Nothing Then
            Set oWS = DBEngine.Workspaces(0)

            If CnnType = dbCnnTypeAccess Then
                sTempDB = sDB
            ElseIf CnnType = dbCnnTypeSQLServer Then
                sTempDB = Environ("Temp") & "\temp.accdb"

                If Len(Dir(sTempDB)) > 0 Then
                    Kill sTempDB
                End If

                oWS.CreateDatabase sTempDB, dbLangGeneral
            End If

            Set mTempDB = oWS.OpenDatabase(sTempDB)
        End If

    ExitLine:
        Set TempDB = mTempDB
        Exit Function
    End Function
    Public Function OpenRecordSet(sql As String, _
                                    Optional RecordsetType As Integer = dbOpenSnapshot) As DAO.Recordset
        Dim qdef As DAO.QueryDef

        Set qdef = TempDB.CreateQueryDef(vbNullString)

        If Connection <> vbNullString Then
            qdef.Connect = Connection
        End If

        With qdef
            .sql = sql
            .ReturnsRecords = True
            Set OpenRecordSet = .OpenRecordSet(RecordsetType)
        End With

    ExitLine:
        Set qdef = Nothing
        Exit Function
    End Function
    Public Sub RunSQL(sql As String)
        Dim qdef As DAO.QueryDef

        Set qdef = TempDB.CreateQueryDef(vbNullString)

        If Connection <> vbNullString Then
            qdef.Connect = Connection
        End If

        With qdef
            .sql = sql
            .ReturnsRecords = False
            .Execute (dbSeeChanges)
        End With

    ExitLine:
        Set qdef = Nothing
        Exit Sub
    End Sub
    Public Function QueryDef(Item As Variant) As DAO.QueryDef
        Set QueryDef = TempDB.QueryDefs(Item)
    End Function
    Public Function TableDef(Item As Variant) As DAO.TableDef
        Set TableDef = TempDB.TableDefs(Item)
    End Function
    Public Function DLookup(Expr As String, _
                            Domain As String, _
                            Optional Criteria As String = vbNullString) As Variant
        Dim rst As DAO.Recordset
        Dim sql As String

        sql = "SELECT TOP 1 " & Expr & " As MyVal " & _
                "FROM " & Domain
            If Criteria <> vbNullString Then
                sql = sql & " " & _
                        "WHERE " & Criteria
            End If
            sql = sql & ";"

        Set rst = OpenRecordSet(sql)
            If Not rst.BOF And Not rst.EOF Then
                DLookup = rst(0)
            Else
                DLookup = Null
            End If

    ExitLine:
        rst.Close
        Set rst = Nothing
    End Function
    Public Function DSum(Expr As String, _
                            Domain As String, _
                            Optional Criteria As String = vbNullString) As Variant
        Dim rst As DAO.Recordset
        Dim sql As String

        sql = "SELECT SUM(" & Expr & ") AS MyVal " & _
                "FROM " & Domain
            If Criteria <> vbNullString Then
                sql = sql & " " & _
                        "WHERE " & Criteria
            End If
            sql = sql & ";"

        Set rst = OpenRecordSet(sql)
            If Not rst.BOF And Not rst.EOF Then
                DSum = rst(0)
            Else
                DSum = 0
            End If

    ExitLine:
        rst.Close
        Set rst = Nothing
    End Function
    Public Function DCount(Expr As String, _
                            Domain As String, _
                            Optional Criteria As String = vbNullString) As Variant
        Dim rst As DAO.Recordset
        Dim sql As String

        sql = "SELECT Count(" & Expr & ") AS MyVal " & _
                "FROM " & Domain
            If Criteria <> vbNullString Then
                sql = sql & " " & _
                        "WHERE " & Criteria
            End If
            sql = sql & ";"

        Set rst = OpenRecordSet(sql)
            If Not rst.BOF And Not rst.EOF Then
                DCount = rst(0)
            Else
                DCount = 0
            End If

    ExitLine:
        rst.Close
        Set rst = Nothing
    End Function
    Public Function DMax(Expr As String, _
                            Domain As String, _
                            Optional Criteria As String = vbNullString) As Variant
        Dim rst As DAO.Recordset
        Dim sql As String

        sql = "SELECT Max(" & Expr & ") AS MyVal " & _
                "FROM " & Domain
            If Criteria <> vbNullString Then
                sql = sql & " " & _
                        "WHERE " & Criteria
            End If
            sql = sql & ";"

        Set rst = OpenRecordSet(sql)
            If Not rst.BOF And Not rst.EOF Then
                DMax = rst(0)
            Else
                DMax = Null
            End If

    ExitLine:
        rst.Close
        Set rst = Nothing
    End Function
    Public Function DMin(Expr As String, _
                            Domain As String, _
                            Optional Criteria As String = vbNullString) As Variant
        Dim rst As DAO.Recordset
        Dim sql As String

        sql = "SELECT Min(" & Expr & ") AS MyVal " & _
                "FROM " & Domain
            If Criteria <> vbNullString Then
                sql = sql & " " & _
                        "WHERE " & Criteria
            End If
            sql = sql & ";"

        Set rst = OpenRecordSet(sql)
            If Not rst.BOF And Not rst.EOF Then
                DMin = rst(0)
            Else
                DMin = Null
            End If

    ExitLine:
        rst.Close
        Set rst = Nothing
    End Function
    Public Function ObjectExists(sObjectType As String, sObjectName As String) As Boolean
         Dim tbl As DAO.TableDef
         Dim qry As DAO.QueryDef
         Dim i As Integer

         If sObjectType = "Table" Then
              For Each tbl In TempDB.TableDefs
                   If tbl.Name = sObjectName Then
                        ObjectExists = True
                        Exit Function
                   End If
              Next tbl
         ElseIf sObjectType = "Query" Then
              For Each qry In TempDB.QueryDefs
                   If qry.Name = sObjectName Then
                        ObjectExists = True
                        Exit Function
                   End If
              Next qry
         ElseIf sObjectType = "Form" Or sObjectType = "Report" Or sObjectType = "Module" Then
              For i = 0 To TempDB.Containers(sObjectType & "s").Documents.Count - 1
                   If DB.Containers(sObjectType & "s").Documents(i).Name = sObjectName Then
                        ObjectExists = True
                        Exit Function
                   End If
              Next i
         ElseIf sObjectType = "Macro" Then
              For i = 0 To TempDB.Containers("Scripts").Documents.Count - 1
                   If DB.Containers("Scripts").Documents(i).Name = sObjectName Then
                        ObjectExists = True
                        Exit Function
                   End If
              Next i
         Else
              MsgBox "Invalid Object Type passed, must be Table, Query, Form, Report, Macro, or Module"
         End If

    End Function
    Public Function Nz(aValue As Variant, aValueIfNull As Variant) As Variant
        If IsNull(aValue) Then
            Nz = aValueIfNull
        Else
            Nz = aValue
        End If
    End Function

Here's an example of how to use the class module to pull the Customers data into a spreadsheet (without headers).  
We'll assume that the Customers table lives in an Access Database located here: ``C:\MyDatabase.accdb``

.. code-block:: vbscript

    Sub PullCustomersExcel()
        Dim cDB As New clsDB
        Dim rst As DAO.Recordset
        Dim sql As String
        
        sql = "SELECT * FROM Customers;"
        
        With cDB
            .Connection = "C:\MyDatabase.accdb"
            Set rst = .OpenRecordset(sql, dbOpenSnapshot)
        End With
        
        ThisWorkbook.Sheets(1).Range("A1").CopyFromRecordset rst
    
    ExitLine:
        rst.Close
        Set rst = Nothing
    End Sub
 

Here's an example of how use the class module to append a record to the Customers table and then print the newly created ID.
 
.. code-block:: vbscript

    Sub AddCustomerRunSQLExcel()
        Dim cDB As New clsDB
        Dim sql As String
        Dim iID AS Long
        
        sql = "INSERT INTO Customers ( Name, State, Date, Sale ) " & _
                "SELECT 'Daniel Park' AS Name, 'CA' As State, #5/1/2019# As Date, 777.77 As Sale;"

        With cDB
            .Connection = "C:\MyDatabase.accdb"
            .RunSQL sql
            iID = .DMax("ID", "Customers")
        End With
        
        Debug.Print iID
    End Sub
