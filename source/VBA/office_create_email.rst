Create Outlook Email
--------------------
This macro can be used in any office application to generate an email in Outlook.  
Subject and HTMLBody are required arugments while the rest are optional.  
The ``fSend`` argument controls whether the email is automatically sent or displayed at the end (default is display).
The ``vAttachments`` argument needs to be an array of filepaths. 
The ``vEmbeddedImages`` argument also needs to be an array of filepaths, 
but these also need to be referenced within the HTMLBody (will expand upon this later).

.. code-block:: vbscript

    Public Sub Create_Email(ByVal sSubject As String, _
                            ByVal sHTML As String, _
                            Optional ByVal sTo As String = "", _
                            Optional ByVal sCC As String = "", _
                            Optional ByVal sBC As String = "", _
                            Optional ByVal sOnBehalfOf As String = "", _
                            Optional ByVal fSend As Boolean = False, _
                            Optional ByVal vAttachments As Variant, _
                            Optional ByVal vEmbeddedImages As Variant)
        Dim olApp As Object             'Outlook Application
        Dim olMail As Object            'Outlook MailItem
        Dim olRecipient As Object       'Outlook Recipient
        Dim olAttach As Object          'Outlook Attachment
        Dim oPropAcc As Object          'Attachment Property Accessor
        Dim iAttch As Integer           'Attachment Counter
        Dim sTempDir As String          'Temp Directory
        Dim oFSO As Object              'File System Object

        Const PR_ATTACH_MIME_TAG = "http://schemas.microsoft.com/mapi/proptag/0x370E001E"
        Const PR_ATTACH_CONTENT_ID = "http://schemas.microsoft.com/mapi/proptag/0x3712001E"
        Const PR_ATTACHMENT_HIDDEN = "http://schemas.microsoft.com/mapi/proptag/0x7FFE000B"

        'Create Outlook Objects
        Set olApp = CreateObject("Outlook.Application")
        Set olMail = olApp.CreateItem(0)

        'Create Email
        With olMail
            .To = sTo
            .CC = sCC
            .BCC = sBC
            .Subject = sSubject
            .HTMLBody = sHTML
            If Len(sOnBehalfOf) > 0 Then
                .SentOnBehalfOfName = sOnBehalfOf
            End If

            'Embed Images
            If Not IsMissing(vEmbeddedImages) Then
                For iAttch = 0 To UBound(vEmbeddedImages)
                    Set olAttach = .Attachments.Add(vEmbeddedImages(iAttch))
                    Set oPropAcc = olAttach.PropertyAccessor
                        oPropAcc.SetProperty PR_ATTACH_MIME_TAG, "image/jpg"
                        oPropAcc.SetProperty PR_ATTACH_CONTENT_ID, "item" & iAttch + 1
                        oPropAcc.SetProperty PR_ATTACHMENT_HIDDEN, True
                Next
            End If

            'Remove Temp Folder
            Set oFSO = CreateObject("Scripting.FileSystemObject")
            sTempDir = Dir(Environ("Temp") & "\TempHTML*", vbDirectory)
                If Len(sTempDir) > 0 Then
                    Do
                        oFSO.DeleteFolder Environ("Temp") & "\" & sTempDir
                        sTempDir = Dir
                    Loop Until Len(sTempDir) = 0
                End If
            Set oFSO = Nothing

            'Add Attachments
            If Not IsMissing(vAttachments) Then
                For iAttch = 0 To UBound(vAttachments)
                    .Attachments.Add vAttachments(iAttch)
                    Kill vAttachments(iAttch)
                Next
            End If

            'Resolve Recipients
            For Each olRecipient In .Recipients
                olRecipient.Resolve
            Next

            'Send or Display
            If fSend Then
                .Send
            Else
                .Display
            End If
        End With

    ExitLine:
        On Error GoTo 0
        Set olApp = Nothing
        Set olMail = Nothing
    End Sub
