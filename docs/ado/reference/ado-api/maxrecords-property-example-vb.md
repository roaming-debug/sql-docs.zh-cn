---
description: MaxRecords 属性示例 (VB)
title: MaxRecords 属性示例 (VB) |Microsoft Docs
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
- MaxRecords property [ADO], Visual Basic example
ms.assetid: 630a3be4-7a87-41cf-997e-8bb50d89db1e
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e484eb17d263cf94cb4e270c8f31f5b04144f5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167131"
---
# <a name="maxrecords-property-example-vb"></a>MaxRecords 属性示例 (VB)
此示例使用 [MaxRecords](./maxrecords-property-ado.md) 属性打开一个 [记录集](./recordset-object-ado.md) ，其中包含 ***标题*** 表中的10个最昂贵的标题。  
  
```  
'BeginMaxRecordsVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim rstTitles As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLTitles As String  
  
    ' Open a connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open recordset containing the 10 most expensive  
    ' titles in the Titles table  
    Set rstTitles = New ADODB.Recordset  
    rstTitles.MaxRecords = 10  
  
    strSQLTitles = "SELECT Title, Price FROM Titles ORDER BY Price DESC"  
    rstTitles.Open strSQLTitles, strCnxn, adOpenStatic, adLockReadOnly, adCmdText  
  
    ' Display the contents of the recordset  
    Debug.Print "Top Ten Titles by Price:"  
  
    Do Until rstTitles.EOF  
        Debug.Print "  " & rstTitles!Title & " - " & rstTitles!Price  
        rstTitles.MoveNext  
    Loop  
  
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
'EndMaxRecordsVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [MaxRecords 属性 (ADO) ](./maxrecords-property-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)