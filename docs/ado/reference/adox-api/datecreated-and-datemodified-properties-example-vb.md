---
description: DateCreated 和 DateModified 属性示例 (VB)
title: DateCreated 和 DateModified 属性示例 (VB) |Microsoft Docs
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
- DateCreated property [ADOX], Visual Basic example
- DateModified property [ADOX], Visual Basic example
ms.assetid: d608ea35-6e68-402f-8184-a5041e408678
author: rothja
ms.author: jroth
ms.openlocfilehash: 56df1a9ee6f79f2bba06ca4d4c0e8a2e62942b62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052038"
---
# <a name="datecreated-and-datemodified-properties-example-vb"></a>DateCreated 和 DateModified 属性示例 (VB)
此示例演示 [DateCreated](./datecreated-property-adox.md) 和 [DateModified](./datemodified-property-adox.md) 属性，方法是将新 [列](./column-object-adox.md) 添加到现有 [表](./table-object-adox.md) 并创建新 **表**。 若要运行此示例，需要 DateOutput 过程。  
  
```  
' BeginDateCreatedVB  
Sub Main()  
    On Error GoTo DateCreatedXError  
  
    Dim cat As New ADOX.Catalog  
    Dim tblEmployees As ADOX.Table  
    Dim tblNewTable As ADOX.Table  
  
    ' Connect to the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    With cat  
        Set tblEmployees = .Tables("Employees")  
  
        ' Print current information about the Employees table.  
        DateOutput "Current properties", tblEmployees  
  
        ' Create and append column to the Employees table.  
        tblEmployees.Columns.Append "NewColumn", adInteger  
        .Tables.Refresh  
  
        ' Print new information about the Employees table.  
        DateOutput "After creating a new column", tblEmployees  
  
        ' Delete new column because this is a demonstration.  
        tblEmployees.Columns.Delete "NewColumn"  
  
        ' Create and append new Table object to the Northwind database.  
        Set tblNewTable = New ADOX.Table  
        tblNewTable.Name = "NewTable"  
        tblNewTable.Columns.Append "NewColumn", adInteger  
        .Tables.Append tblNewTable  
        .Tables.Refresh  
  
        ' Print information about the new Table object.  
        DateOutput "After creating a new table", .Tables("NewTable")  
  
        ' Delete new Table object because this is a demonstration.  
        .Tables.Delete tblNewTable.Name  
    End With  
  
    'Clean up.  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
DateCreatedXError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
  
Sub DateOutput(strTemp As String, tblTemp As ADOX.Table)  
    ' Print DateCreated and DateModified information about  
    ' specified Table object.  
    Debug.Print strTemp  
    Debug.Print "    Table: " & tblTemp.Name  
    Debug.Print "        DateCreated = " & tblTemp.DateCreated  
    Debug.Print "        DateModified = " & tblTemp.DateModified  
    Debug.Print  
End Sub  
' EndDateCreatedVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [DateCreated 属性 (ADOX) ](./datecreated-property-adox.md)   
 [DateModified 属性 (ADOX) ](./datemodified-property-adox.md)   
 [过程对象 (ADOX) ](./procedure-object-adox.md)   
 [过程集合 (ADOX) ](./procedures-collection-adox.md)   
 [视图对象 (ADOX) ](./view-object-adox.md)   
 [视图集合 (ADOX)](./views-collection-adox.md)