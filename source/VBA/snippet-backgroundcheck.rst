Many times, when automating Microsoft Office, you'll want to keep the application window hidden from the user.
If your code breaks when this happens, the application will be left open in the background.  
Here is some code to 1) check if any application instances are open in the background and then 2) kill those instances.

.. code-block:: vba

    Public Enum msAppType
        msAppAccess
        msAppExcel
        msAppWord
    End Enum
    Public Function CountBackgroundProcess(ByVal msAppType As Integer) As Integer
        Dim oWin32, oList, oProcess As Object
        Dim vAppArr As Variant
        Dim iCount As Integer

        vAppArr = Array("MSACCESS", "EXCEL", "WINWORD")

        'Query Background Processes
        Set oWin32 = GetObject("winmgmts:{impersonationLevel=impersonate}!\\.\root\cimv2")
        Set oList = oWin32.ExecQuery("SELECT * FROM WIN32_Process " & _
                                        "WHERE Name='" & vAppArr(msAppType) & ".EXE' " & _
                                            "AND CommandLine Like '%-Embedding'")

        'Count
        iCount = oList.Count

    ExitLine:
        CountBackgroundProcess = iCount
        Set oProcess = Nothing
        Set oList = Nothing
        Set oWin32 = Nothing
    End Function
    Public Function KillBackgroundProcess(ByVal msAppType As Integer) As Boolean
        Dim oWin32, oList, oProcess As Object
        Dim vAppArr As Variant
        Dim bErr As Boolean

        vAppArr = Array("MSACCESS", "EXCEL", "WINWORD")

        'Query Background Processes
        Set oWin32 = GetObject("winmgmts:{impersonationLevel=impersonate}!\\.\root\cimv2")
        Set oList = oWin32.ExecQuery("SELECT * FROM WIN32_Process " & _
                                        "WHERE Name='" & vAppArr(msAppType) & ".EXE' " & _
                                            "AND CommandLine Like '%-Embedding'")

        'Terminate
        If oList.Count > 0 Then
            For Each oProcess In oList
                If oProcess.Terminate <> 0 Then
                    bErr = True
                End If
            Next
        End If

    ExitLine:
        KillBackgroundProcess = Not bErr
        Set oProcess = Nothing
        Set oList = Nothing
        Set oWin32 = Nothing
    End Function
