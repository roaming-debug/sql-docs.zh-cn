---
description: GetObjectOwner 和 SetObjectOwner 方法示例 (VB)
title: GetObjectOwner 和 SetObjectOwner 方法示例 (VB) |Microsoft Docs
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
- SetObjectOwner method [ADOX], Visual Basic example
- GetObjectOwner method [ADOX], Visual Basic example
ms.assetid: e44ec3d4-42ae-447d-aaed-bdea53cb0cca
author: rothja
ms.author: jroth
ms.openlocfilehash: 8c5b5125f65eec46818ba4d43c8b305f6d82d964
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054212"
---
# <a name="getobjectowner-and-setobjectowner-methods-example-vb"></a>GetObjectOwner 和 SetObjectOwner 方法示例 (VB)
此示例演示 [GetObjectOwner](./getobjectowner-method-adox.md) 和 [SetObjectOwner](./setobjectowner-method.md) 方法。 此代码假定存在组记帐 (请参阅 [组和用户追加，ChangePassword 方法示例 (VB) ](./groups-and-users-append-changepassword-methods-example-vb.md) ，以了解如何将此组添加到系统) 。 "类别" 表的所有者设置为 "记帐"。  
  
```  
' BeginOwnersVB  
Sub OwnersX()  
  
    Dim tblLoop As New ADOX.Table  
    Dim cat As New ADOX.Catalog  
    Dim strOwner As String  
  
    ' Open the Catalog.  
    cat.ActiveConnection = "Provider=Microsoft.Jet.OLEDB.4.0;" & _  
        "Data Source=c:\Program Files\" & _  
        "Microsoft Office\Office\Samples\Northwind.mdb;" & _  
        "jet oledb:system database=" & _  
        "c:\Program Files\Microsoft Office\Office\system.mdw"  
  
    ' Print the original owner of Categories  
    strOwner = cat.GetObjectOwner("Categories", adPermObjTable)  
    Debug.Print "Owner of Categories: " & strOwner  
  
    ' Set the owner of Categories to Accounting  
    cat.SetObjectOwner "Categories", adPermObjTable, "Accounting"  
  
    ' List the owners of all tables and columns in the catalog.  
    For Each tblLoop In cat.Tables  
        Debug.Print "Table: " & tblLoop.Name  
        Debug.Print "   Owner: " & _  
            cat.GetObjectOwner(tblLoop.Name, adPermObjTable)  
    Next tblLoop  
  
    ' Restore the original owner of Categories  
    cat.SetObjectOwner "Categories", adPermObjTable, strOwner  
  
End Sub  
' EndOwnersVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [GetObjectOwner 方法 (ADOX) ](./getobjectowner-method-adox.md)   
 [SetObjectOwner 方法](./setobjectowner-method.md)