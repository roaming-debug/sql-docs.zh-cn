---
description: 连接的 Close 方法、表 Type 属性示例 (VB)
title: 连接关闭方法，表类型属性示例 (VB) |Microsoft Docs
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
- Close method [ADOX], Visual Basic example
- Type property [ADOX], Visual Basic example
ms.assetid: f88e7a3b-19ed-46e2-b2ce-3b611d9b8166
author: rothja
ms.author: jroth
ms.openlocfilehash: d5049e6d1e60a2a54633c1fa0fe7adacb0685690
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172215"
---
# <a name="connection-close-method-table-type-property-example-vb"></a>连接的 Close 方法、表 Type 属性示例 (VB)
将 [ActiveConnection](./activeconnection-property-adox.md) 属性设置为 **Nothing** 应关闭到目录的连接。 关联的集合将为空。 从目录中的架构对象创建的任何对象都将是孤立对象。 那些已缓存的对象上的所有属性仍可用，但读取需要调用提供程序的属性的尝试将失败。  
  
```  
' BeginCloseConnectionVB  
Sub Main()  
    On Error GoTo CloseConnectionByNothingError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim tbl As ADOX.Table  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Set tbl = cat.Tables(0)  
    Debug.Print tbl.Type    ' Cache tbl.Type info  
    Set cat.ActiveConnection = Nothing  
    Debug.Print tbl.Type    ' tbl is orphaned  
    ' Previous line will succeed if this info was cached.  
    Debug.Print tbl.Columns(0).DefinedSize  
    ' Previous line will fail if this info has not been cached.  
  
    'Clean up.  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
CloseConnectionByNothingError:  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndCloseConnectionVB  
```  
  
 关闭用于打开目录的 [连接](../ado-api/connection-object-ado.md) 对象应具有与将 **ActiveConnection** 属性设置为 **Nothing** 相同的效果。  
  
```  
Attribute VB_Name = "Connection"  
```  
  
## <a name="see-also"></a>另请参阅  
 [ActiveConnection 属性 (ADOX) ](./activeconnection-property-adox.md)   
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [列对象 (ADOX) ](./column-object-adox.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [Table 对象 (ADOX) ](./table-object-adox.md)   
 [表集合 (ADOX) ](./tables-collection-adox.md)   
 [Type 属性（表）(ADOX)](./type-property-table-adox.md)