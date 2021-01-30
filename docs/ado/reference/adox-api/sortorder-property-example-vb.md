---
description: SortOrder 属性示例 (VB)
title: 排序 (VB) 的属性示例 |Microsoft Docs
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
- SortOrder property [ADOX]
ms.assetid: d9502254-d89b-4bcb-94f1-6418f89e7f30
author: rothja
ms.author: jroth
ms.openlocfilehash: 09ecc9649da8d080ac0fc1dbde4f26cc850907e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164028"
---
# <a name="sortorder-property-example-vb"></a>SortOrder 属性示例 (VB)
此示例演示已追加到[索引](./index-object-adox.md)的[Columns](./columns-collection-adox.md)集合的[列](./column-object-adox.md)的[SortOrder](./sortorder-property-adox.md)属性。 该代码将升序索引追加到 **Employees** 表中的 Country 列，然后显示记录。 然后，该代码将向 **Employees** 表中的 "国家/地区" 列附加一个降序索引，并再次显示记录。 显示升序和降序索引之间的差异。  
  
```  
' BeginSortOrderVB  
Sub Main()  
    On Error GoTo SortOrderXError  
  
    Dim cnn As New ADODB.Connection  
    Dim catNorthwind As New ADOX.Catalog  
    Dim idxAscending As New ADOX.Index  
    Dim idxDescending As New ADOX.Index  
    Dim rstEmployees As New ADODB.Recordset  
  
    ' Connect to the catalog.  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
    Set catNorthwind.ActiveConnection = cnn  
  
    ' Append Country column to new index.  
    idxAscending.Columns.Append "Country"  
    idxAscending.Columns("Country").SortOrder = adSortAscending  
    idxAscending.Name = "Ascending"  
    idxAscending.IndexNulls = adIndexNullsAllow  
  
    'Append new index to Employees table.  
    catNorthwind.Tables("Employees").Indexes.Append idxAscending  
  
    rstEmployees.Index = idxAscending.Name  
    rstEmployees.Open "Employees", cnn, adOpenKeyset, _  
        adLockOptimistic, adCmdTableDirect  
  
    With rstEmployees  
        .MoveFirst  
        Debug.Print "Index = " & .Index  
        Debug.Print "  Country - Name"  
  
        ' Enumerate the Recordset. The value of the  
        ' IndexNulls property will determine if the newly  
        ' added record appears in the output.  
        Do While Not .EOF  
            Debug.Print "    " & !Country & " - " & _  
                !FirstName & " " & !LastName  
            .MoveNext  
        Loop  
  
        .Close  
    End With  
  
    ' Append Country column to new index.  
    idxDescending.Columns.Append "Country"  
    idxDescending.Columns("Country").SortOrder = adSortDescending  
    idxDescending.Name = "Descending"  
    idxDescending.IndexNulls = adIndexNullsAllow  
  
    'Append descending index to Employees table.  
    catNorthwind.Tables("Employees").Indexes.Append idxDescending  
  
    rstEmployees.Index = idxDescending.Name  
    rstEmployees.Open "Employees", cnn, adOpenKeyset, _  
        adLockOptimistic, adCmdTableDirect  
  
    With rstEmployees  
        .MoveFirst  
        Debug.Print "Index = " & .Index  
        Debug.Print "  Country - Name"  
  
        ' Enumerate the Recordset. The value of the  
        ' IndexNulls property will determine if the newly  
        ' added record appears in the output.  
        Do While Not .EOF  
            Debug.Print "    " & !Country & " - " & _  
                !FirstName & " " & !LastName  
            .MoveNext  
        Loop  
  
        .Close  
    End With  
  
    ' Delete new indexes because this is a demonstration.  
    catNorthwind.Tables("Employees").Indexes.Delete idxAscending.Name  
    catNorthwind.Tables("Employees").Indexes.Delete idxDescending.Name  
  
    'Clean up  
    cnn.Close  
    Set catNorthwind = Nothing  
    Set idxAscending = Nothing  
    Set idxDescending = Nothing  
    Set rstEmployees = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
SortOrderXError:  
  
    Set catNorthwind = Nothing  
    Set idxAscending = Nothing  
    Set idxDescending = Nothing  
  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndSortOrderVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [列对象 (ADOX) ](./column-object-adox.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [索引对象 (ADOX) ](./index-object-adox.md)   
 [SortOrder 属性 (ADOX)](./sortorder-property-adox.md)