---
description: '访问分层记录集中的行 (示例) '
title: 访问分层记录集中的行 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 25f1d2a1-6d5e-4457-aa07-5db5c75dee18
author: rothja
ms.author: jroth
ms.openlocfilehash: 369d9ef5548a0ff30b08dc27c9df38b0f3c1fa1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028632"
---
# <a name="accessing-rows-in-a-hierarchical-recordset-example"></a>访问分层记录集中的行 (示例) 
下面的示例演示访问分层 [记录集中](../../reference/ado-api/recordset-object-ado.md)的行所需的步骤：

1.  **作者** 和 **titleauthor** 表中的 **记录集** 对象均由作者 ID 相关联。

2.  外部循环显示每个作者的名字和姓氏、省/市/自治区和标识。

3.  每行的追加的 **记录集** 从 [字段](../../reference/ado-api/fields-collection-ado.md) 集合中检索并分配给 *rstTitleAuthor*。

4.  内部循环显示追加的 **记录集中** 每一行的四个字段。

 出于说明目的， [StayInSync](../../reference/ado-api/stayinsync-property.md) 属性设置为 **false** ，以便您可以看到在外部循环的每次迭代中显式更改章节。 为了使代码示例更高效，你可以在步骤3中移动步骤2中第一行之前的分配，以便仅执行一次分配。 然后将 [StayInSync](../../reference/ado-api/stayinsync-property.md)属性设置为 **true**，以便当 *rst* 移动到新行时， *rstTitleAuthor* 将隐式地自动更改为相应的章节。

## <a name="example"></a>示例

```
Sub datashape()
   Dim cnn As New ADODB.Connection
   Dim rst As New ADODB.Recordset
   Dim rstTitleAuthor As New ADODB.Recordset

   cnn.Provider = "MSDataShape"
   cnn.Open    "Data Provider=MSDASQL;" & _
               "Data Source=SRV;Integrated Security=SSPI;Database=Pubs"
' STEP 1
   rst.StayInSync = FALSE
   rst.Open    "SHAPE  {select * from authors} "  & _
               "APPEND ({select * from titleauthor} " & _
               "RELATE au_id TO au_id) AS chapTitleAuthor", _
               cnn
' STEP 2
   While Not rst.EOF
      Debug.Print    rst("au_fname"), rst("au_lname"), _
                     rst("state"), rst("au_id")
' STEP 3
      Set rstTitleAuthor = rst("chapTitleAuthor").Value
' STEP 4
      While Not rstTitleAuthor.EOF
         Debug.Print rstTitleAuthor(0), rstTitleAuthor(1), _
                     rstTitleAuthor(2), rstTitleAuthor(3)
         rstTitleAuthor.MoveNext
      Wend
      rst.MoveNext
   Wend
End Sub
```

## <a name="see-also"></a>另请参阅
 [数据定形概述](./data-shaping-overview.md)[字段对象](../../reference/ado-api/field-object.md)[字段集合 (ado) ](../../reference/ado-api/fields-collection-ado.md) [正式形状语法](./formal-shape-grammar.md) [Microsoft 数据定形 OLE DB 服务 (ADO 服务提供程序](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)) [记录集对象 (Ado) ](../../reference/ado-api/recordset-object-ado.md) [数据定形的所需提供程序](./required-providers-for-data-shaping.md)[形状](./shape-append-clause.md) [](./shape-commands-in-general.md) [计算子句](./shape-compute-clause.md) [Visual Basic for Applications 函数](./visual-basic-for-applications-functions.md)