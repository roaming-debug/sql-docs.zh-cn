---
description: 'AbsolutePosition 和 CursorLocation 属性示例 (VB) '
title: AbsolutePosition 和 CursorLocation 属性示例 (VB) |Microsoft Docs
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
- AbsolutePosition property [ADO], Visual Basic example
- CursorLocation property [ADO], Visual Basic example
ms.assetid: c4755799-c60a-4b5e-a01f-b85dd0e0a7f9
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e710b1df8c7d7215190de9f4fefc0df16649bba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035968"
---
# <a name="absoluteposition-and-cursorlocation-properties-example-vb"></a>AbsolutePosition 和 CursorLocation 属性示例 (VB) 
此示例演示 [AbsolutePosition](./absoluteposition-property-ado.md) 属性如何跟踪枚举 [记录集](./recordset-object-ado.md)的所有记录的循环的进度。 它通过将光标设置为客户端光标来使用 [CursorLocation](./cursorlocation-property-ado.md) 属性来启用 **AbsolutePosition** 属性。  
  
```  
'BeginAbsolutePositionVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQL As String  
        'record variables  
    Dim strMessage As String  
  
    'Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open Employee recordset with  
    ' Client-side cursor to enable AbsolutePosition property  
    Set rstEmployees = New ADODB.Recordset  
    strSQL = "employee"  
    rstEmployees.Open strSQL, strCnxn, adUseClient, adLockReadOnly, adCmdTable  
  
    ' Enumerate Recordset  
    Do While Not rstEmployees.EOF  
        ' Display current record information  
        strMessage = "Employee: " & rstEmployees!lname & vbCr & _  
            "(record " & rstEmployees.AbsolutePosition & _  
            " of " & rstEmployees.RecordCount & ")"  
        If MsgBox(strMessage, vbOKCancel) = vbCancel Then Exit Do  
        rstEmployees.MoveNext  
    Loop  
  
    ' clean up  
    rstEmployees.Close  
    Cnxn.Close  
    Set rstEmployees = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
   ' clean up  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndAbsolutePositionVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [AbsolutePosition 属性 (ADO) ](./absoluteposition-property-ado.md)   
 [CursorLocation 属性 (ADO) ](./cursorlocation-property-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)