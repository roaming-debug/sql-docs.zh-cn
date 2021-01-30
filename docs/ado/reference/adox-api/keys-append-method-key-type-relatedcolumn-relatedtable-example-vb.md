---
description: 项 Append 方法、项 Type、RelatedColumn、RelatedTable 和 UpdateRule 属性示例 (VB)
title: 在两个表之间创建新的外键关系 (VB) |Microsoft Docs
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
- Key Type property [ADOX], Visual Basic example
- RelatedTable property [ADOX], Visual Basic example
- Keys Append method [ADOX], Visual Basic example
- UpdateRule property [ADOX], Visual Basic example
- RelatedColumn property [ADOX], Visual Basic example
ms.assetid: 13b5b1c3-6af6-439e-bb65-976578ba6bc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fb89ca61eacd7f10b0fe5546c569e9ebe16f73c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169323"
---
# <a name="keys-append-method-key-type-relatedcolumn-relatedtable-and-updaterule-properties-example-vb"></a>项 Append 方法、项 Type、RelatedColumn、RelatedTable 和 UpdateRule 属性示例 (VB)
下面的代码演示如何在名为 **Customers** 和 **Orders** 的两个现有表之间创建新的外键关系。  
  
```  
' BeginCreateKeyVB  
Sub Main()  
    On Error GoTo CreateKeyError  
  
    Dim kyForeign As New ADOX.Key  
    Dim cat As New ADOX.Catalog  
  
    ' Connect to the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Define the foreign key.  
    kyForeign.Name = "CustOrder"  
    kyForeign.Type = adKeyForeign  
    kyForeign.RelatedTable = "Customers"  
    kyForeign.Columns.Append "CustomerId"  
    kyForeign.Columns("CustomerId").RelatedColumn = "CustomerId"  
    kyForeign.UpdateRule = adRICascade  
  
    ' Append the foreign key to the keys collection.  
    cat.Tables("Orders").Keys.Append kyForeign  
  
    'Delete the key t demonstrate the Delete method.  
    cat.Tables("Orders").Keys.Delete kyForeign.Name  
  
    'Clean up.  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set kyForeign = Nothing  
    Exit Sub  
  
CreateKeyError:  
    Set cat = Nothing  
    Set kyForeign = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateKeyVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [列对象 (ADOX) ](./column-object-adox.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [关键对象 (ADOX) ](./key-object-adox.md)   
 [项集合 (ADOX) ](./keys-collection-adox.md)   
 [名称属性 (ADOX) ](./name-property-adox.md)   
 [RelatedColumn 属性 (ADOX) ](./relatedcolumn-property-adox.md)   
 [RelatedTable 属性 (ADOX) ](./relatedtable-property-adox.md)   
 [Table 对象 (ADOX) ](./table-object-adox.md)   
 [表集合 (ADOX) ](./tables-collection-adox.md)   
 [ (ADOX) 类型属性 (键) ](./type-property-key-adox.md)   
 [UpdateRule 属性 (ADOX)](./updaterule-property-adox.md)