---
description: 过程 Refresh 方法示例 (VB)
title: " (VB) 的过程 Refresh 方法示例 |Microsoft Docs"
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
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 289cee12f65511cf64a68837043eacb37d2f2763
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054842"
---
# <a name="procedures-refresh-method-example-vb"></a>过程 Refresh 方法示例 (VB)
下面的代码演示如何刷新[目录](./catalog-object-adox.md)的[过程](./procedures-collection-adox.md)集合。 这是必需的，然后才能访问 **目录** 中的 [过程](./procedure-object-adox.md)对象。  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [过程集合 (ADOX) ](./procedures-collection-adox.md)   
 [Refresh 方法 (ADO)](../ado-api/refresh-method-ado.md)