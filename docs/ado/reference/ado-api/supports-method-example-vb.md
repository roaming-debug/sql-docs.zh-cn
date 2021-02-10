---
description: Supports 方法示例 (VB)
title: " (VB) 支持方法示例 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Supports method [ADO], Visual Basic example
ms.assetid: 135aab26-ff5c-4fd9-910f-65cdead0b47e
author: rothja
ms.author: jroth
ms.openlocfilehash: faaa4aaba30a37c8c13ff2d3b189f425595d6ee7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056486"
---
# <a name="supports-method-example-vb"></a>Supports 方法示例 (VB)
此示例使用 [支持](./supports-method.md) 方法显示使用不同游标类型打开的记录集支持的选项。 此过程需要 DisplaySupport 过程才能运行。  
  
```  
'BeginSupportsVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    ' recordset and connection variables  
    Dim rstTitles As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLTitles As String  
     ' array variables  
    Dim arrCursorType(4) As Integer  
    Dim intIndex As Integer  
  
    ' open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Fill array with CursorType constants  
    arrCursorType(0) = adOpenForwardOnly  
    arrCursorType(1) = adOpenKeyset  
    arrCursorType(2) = adOpenDynamic  
    arrCursorType(3) = adOpenStatic  
  
    ' open recordset using each CursorType and optimistic locking  
    For intIndex = 0 To 3  
        Set rstTitles = New ADODB.Recordset  
        rstTitles.CursorType = arrCursorType(intIndex)  
        rstTitles.LockType = adLockOptimistic  
  
        strSQLTitles = "Titles"  
        rstTitles.Open strSQLTitles, Cnxn, , , adCmdTable  
  
        Select Case arrCursorType(intIndex)  
           Case adOpenForwardOnly  
              Debug.Print "ForwardOnly cursor supports:"  
           Case adOpenKeyset  
              Debug.Print "Keyset cursor supports:"  
           Case adOpenDynamic  
              Debug.Print "Dynamic cursor supports:"  
           Case adOpenStatic  
              Debug.Print "Static cursor supports:"  
        End Select  
  
        ' call the DisplaySupport procedure from below  
        ' to display the supported options  
        DisplaySupport rstTitles  
  
    Next intIndex  
  
    ' clean up  
    rstTitles.Close  
    Cnxn.Close  
    Set rstTitles = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstTitles Is Nothing Then  
        If rstTitles.State = adStateOpen Then rstTitles.Close  
    End If  
    Set rstTitles = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSupportsVB  
  
'BeginSupports2VB  
Public Sub DisplaySupport(rstTemp As ADODB.Recordset)  
  
   Dim arrConstants(11) As Long  
   Dim blnSupports As Boolean  
   Dim intIndex As Integer  
  
   ' Fill array with cursor option constants  
   arrConstants(0) = adAddNew  
   arrConstants(1) = adApproxPosition  
   arrConstants(2) = adBookmark  
   arrConstants(3) = adDelete  
   arrConstants(4) = adFind  
   arrConstants(5) = adHoldRecords  
   arrConstants(6) = adMovePrevious  
   arrConstants(7) = adNotify  
   arrConstants(8) = adResync  
   arrConstants(9) = adUpdate  
   arrConstants(10) = adUpdateBatch  
  
   For intIndex = 0 To 10  
      blnSupports = _  
         rstTemp.Supports(arrConstants(intIndex))  
      If blnSupports Then  
         Select Case arrConstants(intIndex)  
            Case adAddNew  
               Debug.Print "   AddNew"  
            Case adApproxPosition  
               Debug.Print "   AbsolutePosition and AbsolutePage"  
            Case adBookmark  
               Debug.Print "   blnkmark"  
            Case adDelete  
               Debug.Print "   Delete"  
            Case adFind  
               Debug.Print "   Find"  
            Case adHoldRecords  
               Debug.Print "   Holding Records"  
            Case adMovePrevious  
               Debug.Print "   MovePrevious and Move"  
            Case adNotify  
               Debug.Print "   Notifications"  
            Case adResync  
               Debug.Print "   Resyncing data"  
            Case adUpdate  
               Debug.Print "   Update"  
            Case adUpdateBatch  
               Debug.Print "   batch updating"  
         End Select  
      End If  
   Next intIndex  
  
End Sub  
'EndSupports2VB  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO)  (Recordset 对象 ](./recordset-object-ado.md)   
 [Supports 方法](./supports-method.md)