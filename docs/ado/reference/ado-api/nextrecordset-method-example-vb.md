---
description: NextRecordset 方法示例 (VB)
title: NextRecordset 方法示例 (VB) |Microsoft Docs
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
- NextRecordset method [ADO], Visual Basic example
ms.assetid: b14806da-80d9-4da4-bb87-f558b36a6ac0
author: rothja
ms.author: jroth
ms.openlocfilehash: d2508879bff3bb5335001dbe76b26814382f6a6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041567"
---
# <a name="nextrecordset-method-example-vb"></a>NextRecordset 方法示例 (VB)
此示例使用 [NextRecordset](./nextrecordset-method-ado.md) 方法查看记录集中的数据，该记录集使用由三个单独的 **SELECT** 语句组成的复合命令语句。  
  
```  
'BeginNextRecordsetVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    ' connection and recordset variables  
    Dim rstCompound As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim SQLCompound As String  
  
    Dim intCount As Integer  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open compound recordset  
    Set rstCompound = New ADODB.Recordset  
    SQLCompound = "SELECT * FROM Authors; " & _  
        "SELECT * FROM stores; " & _  
        "SELECT * FROM jobs"  
    rstCompound.Open SQLCompound, Cnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
    ' Display results from each SELECT statement  
    intCount = 1  
    Do Until rstCompound Is Nothing  
        Debug.Print "Contents of recordset #" & intCount  
  
        Do Until rstCompound.EOF  
            Debug.Print , rstCompound.Fields(0), rstCompound.Fields(1)  
            rstCompound.MoveNext  
        Loop  
  
        Set rstCompound = rstCompound.NextRecordset  
        intCount = intCount + 1  
    Loop  
  
    ' clean up  
    Cnxn.Close  
    Set rstCompound = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstCompound Is Nothing Then  
        If rstCompound.State = adStateOpen Then rstCompound.Close  
    End If  
    Set rstCompound = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndNextRecordsetVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [NextRecordset 方法 (ADO) ](./nextrecordset-method-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)