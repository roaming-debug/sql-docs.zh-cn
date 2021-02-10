---
description: ADO Run-Time 错误
title: ADO 错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
author: rothja
ms.author: jroth
ms.openlocfilehash: bed9958455af424c4f18cb0f6051eb30e2efaa3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033387"
---
# <a name="ado-run-time-errors"></a>ADO Run-Time 错误
ADO 错误作为运行时错误报告给你的程序。 您可以使用编程语言的错误捕获机制来捕获和处理它们。 例如，在 Visual Basic 中，使用 **On Error** 语句。 在 Visual C++ 中，它取决于用于访问 ADO 库的方法。 使用 #import 时，请使用 **try-catch** 块。 否则，c + + 程序员需要通过调用 **GetErrorInfo** 显式检索 error 对象。 以下 Visual Basic sub 过程演示捕获 ADO 错误：

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 此 **Form_Load** 事件过程有意通过尝试打开相同的 **连接** 对象来创建一个错误。 第二次调用 **Open** 方法时，将激活错误处理程序。 在这种情况下，错误的类型为 **adErrObjectOpen**，因此，在恢复程序执行之前，错误处理程序会显示以下消息：

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 此错误消息包含 Visual Basic **Err** 对象提供的每条信息，但 **LastDLLError** 值除外，此处不适用。 错误号告诉你发生了哪个错误。 如果你不想自行处理错误，则说明非常有用。 您可以直接将它传递给用户。 尽管通常需要使用为应用程序自定义的消息，但不能预见每个错误;说明提供了一些有关错误的提示。 在示例代码中，此错误是由 **连接** 对象报告的。 你将在此处看到对象的类型或编程 ID-不是变量名。

> [!NOTE]
>  Visual Basic **Err** 对象只包含有关最新错误的信息。 对于最新的 ADO 操作引发的每个错误，**连接** 对象的 ADO **Errors** 集合都包含一个 **error** 对象。 使用 **errors** 集合而不是 **Err** 对象处理多个错误。 有关 **错误** 集合的详细信息，请参阅 [提供程序错误](./provider-errors.md)。 但是，如果没有有效的 **连接** 对象，则 **Err** 对象是有关 ADO 错误的唯一信息来源。

 哪些类型的操作可能会导致 ADO 错误？ 常见 ADO 错误可以涉及打开对象（如 **连接** 或 **记录集**、尝试更新数据或调用提供程序不支持的方法或属性）。

 还可以将 OLE DB 错误作为 **错误** 集合中的运行时错误传递给应用程序。

 以下主题提供了有关 ADO 错误的详细信息。

-   [ADO 错误参考](./ado-error-reference.md)