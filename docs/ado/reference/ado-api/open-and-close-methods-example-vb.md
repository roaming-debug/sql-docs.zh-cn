---
description: Open 和 Close 方法示例 (VB)
title: " (VB) 打开和关闭方法示例 |Microsoft Docs"
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
- Close method [ADO], Visual Basic example
- Open method [ADO], Visual Basic example
ms.assetid: 1311d561-0e86-40f5-8cbc-ad8f13e626d1
author: rothja
ms.author: jroth
ms.openlocfilehash: b2c1798c1574aa671677ffbf944db595a48c16cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041447"
---
# <a name="open-and-close-methods-example-vb"></a>Open 和 Close 方法示例 (VB)
此示例使用已打开的 [记录集](./recordset-object-ado.md)和 [连接](./connection-object-ado.md)对象上的 **打开** 和 [关闭](./close-method-ado.md)方法。  
  
```  
'BeginOpenVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub OpenX()  
    On Error GoTo ErrorHandler  
  
    Dim Cnxn As ADODB.Connection  
    Dim rstEmployees As ADODB.Recordset  
    Dim strCnxn As String  
    Dim strSQLEmployees As String  
    Dim varDate As Variant  
  
    ' Open connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open strCnxn  
  
    ' Open employee table  
    Set rstEmployees = New ADODB.Recordset  
    strSQLEmployees = "employee"  
    rstEmployees.Open strSQLEmployees, Cnxn, adOpenKeyset, adLockOptimistic, adCmdTable  
  
    ' Assign the first employee record's hire date  
    ' to a variable, then change the hire date  
    varDate = rstEmployees!hire_date  
    Debug.Print "Original data"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
        rstEmployees!lname & " - " & rstEmployees!hire_date  
    rstEmployees!hire_date = #1/1/1900#  
    rstEmployees.Update  
    Debug.Print "Changed data"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
        rstEmployees!lname & " - " & rstEmployees!hire_date  
  
    ' Requery Recordset and reset the hire date  
    rstEmployees.Requery  
    rstEmployees!hire_date = varDate  
    rstEmployees.Update  
    Debug.Print "Data after reset"  
    Debug.Print "  Name - Hire Date"  
    Debug.Print "  " & rstEmployees!fname & " " & _  
       rstEmployees!lname & " - " & rstEmployees!hire_date  
  
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
'EndOpenVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO (关闭方法) ](./close-method-ado.md)   
 [ADO) 的连接对象 (](./connection-object-ado.md)   
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [ADO 记录集 (打开方法) ](./open-method-ado-recordset.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)