---
description: 当前记录和记录集的大小
title: 记录集的当前记录和大小 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e63ff331-8655-4be7-82c6-e6cd6cc9d16d
author: rothja
ms.author: jroth
ms.openlocfilehash: 791ca36a7f5a61a0ad0208753386fba826d6d06e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033337"
---
# <a name="current-record-and-size-of-recordset"></a>当前记录和记录集的大小
本节介绍如何在 JScript 代码示例的示例 **记录集中** 查找光标的当前位置 [，以返回记录集](./jscript-code-example-to-return-a-recordset.md)。  
  
## <a name="current-record"></a>当前记录  
 数据集中的当前记录对应于 **Recordset** 对象的游标位置所指向的。 当从数据源返回 **记录集** 对象作为调用记录集的结果时 **。打开**、 **Command.Exe太漂亮** 或 **Connection.Exe** (包括 **NamedCommand** 和 **StoredProcedure**) ，光标设置为指向第一条记录。 在示例数据集中，初始当前记录是 "叔叔 Bob 的随机是 Pears" 项。  
  
## <a name="size-of-recordset"></a>记录集的大小  
 若要确定 **recordset** 对象的大小，请获取 **RecordCount** 属性的值。 此值是一个长整数，指示记录 **集中** 的记录数。 如果从 Microsoft SQL Server 的 OLEDB 提供程序返回数据集，则此值将提供返回的行数。 读取已关闭 **记录集** 的 **RecordCount** 属性会导致错误。  
  
 如果无法确定记录数，则属性的值为-1。  
  
 **RecordCount** 属性的值还取决于提供程序的功能和使用的游标类型。 对于只进游标，该值为-1。 对于静态或键集游标，此值是 **记录集** 对象中返回的记录的实际数量。 对于动态游标，该值为-1 或记录的实际数量，具体取决于数据源。  
  
 支持 **Recordcount** 的游标必须更难工作，因此需要更多的计算能力，而游标不支持 **Recordcount**。 如果不需要知道记录数，则使用不同的游标类型可能有助于提高应用程序的性能，尤其是在必须处理大型数据集时。  
  
 在某些情况下，如果不首先从数据源中提取所有记录，则提供程序或游标无法确定 **RecordCount** 值。 若要确保准确计数，请调用 **记录集**。**MoveLast** 方法，然后调用 **RecordCount**。  
  
 使用 [JScript 代码示例](./jscript-code-example-to-return-a-recordset.md)获取的示例 **记录集** 对象使用只进游标，因此，对此对象调用 **RecordCount** 始终会导致-1。 如果更改调用 **记录集** 的代码行。**Open** 方法，如以下示例中所示， **RecordCount** 属性将返回提取的实际记录数。  
  
```  
oRs.Open sSQL, sCnStr, adOpenStatic, adLockOptimistic, adCmdText   
```  
  
 这是因为 [用于 SQL Server 的 Microsoft OLE DB 提供程序的](../appendixes/microsoft-ole-db-provider-for-sql-server.md) 静态游标支持 **RecordCount**。 在此示例中，有五条记录，因此 **RecordCount** 应生成值5。  
  
 本部分包含以下主题。  
  
 [记录集的边界](./boundaries-of-a-recordset.md)