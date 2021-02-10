---
description: 操作非参数化命令
title: 非参数化命令的操作 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- non-parameterized commands [ADO]
- data shaping [ADO], non-parameterized commands
ms.assetid: 9700e50a-9f17-4ba3-8afb-f750741dc6ca
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee255f895e934c4405d888d72f15b8cee9159d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037197"
---
# <a name="operation-of-non-parameterized-commands"></a>操作非参数化命令
对于非参数化命令，会执行所有提供程序命令，并在命令执行过程中创建 **记录集** 。 如果以同步方式执行命令，将完全填充所有 **记录集** 。 如果选择了异步填充模式，则 **记录集** 的填充状态将取决于填充模式和 **记录集** 的大小。  
  
 例如， *parent-command* 可能会从 customers 表中为公司返回一个 **记录集** ， *子命令* 可能会返回 orders 表中所有客户的订单 **记录集** 。  
  
```  
SHAPE {SELECT * FROM Customers}   
   APPEND ({SELECT * FROM Orders} AS chapOrders   
   RELATE customerID TO customerID)  
```  
  
 对于非参数化的父子关系，每个父记录集对象和子 **记录集** 对象都必须具有共同的列以将它们关联起来。 列首先在关联子句、 *父列* 和 *子列* 中进行命名。 这些列在各自的 **记录集** 对象中可能具有不同的名称，但必须引用相同的信息才能指定有意义的关系。 例如，" **客户** " 和 " **订单" 记录集** 对象可以有 customerID 字段。 由于子 **记录集** 的成员身份由提供程序命令确定，因此子 **记录集** 可以包含孤立的行。 这些孤立行不可访问，无需进行额外的整形。  
  
 数据定形将章节列附加到父 **记录集**。 章节列中的值是对子 **记录集中** 的行的引用，后者满足了关系子句。 也就是说，在给定父行的 *父列* 中，同一值位于章节子项的所有行的 *子列* 中。 当在同一个关系子句中使用多个 TO 子句时，它们使用 AND 运算符隐式组合在一起。 如果 "关联" 子句中的父列不构成父 **记录集** 的键，则一个子行可以有多个父行。  
  
 访问 "章节" 列中的引用时，ADO 会自动检索引用所表示的 **记录集** 。 请注意，虽然已检索整个子 **记录集** ，但在非参数化命令中，章节只显示行的子集。  
  
 如果追加列没有 *章节别名*，将自动为其生成名称。 该列的 [字段](../../reference/ado-api/field-object.md) 对象将追加到 **Recordset** 对象的 [Fields](../../reference/ado-api/fields-collection-ado.md) 集合，其数据类型将为 **adChapter**。  
  
 有关导航分层 **记录集** 的信息，请参阅 [访问分层记录集中的行](./accessing-rows-in-a-hierarchical-recordset.md)。  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](./data-shaping-example.md)   
 [正式形状语法](./formal-shape-grammar.md)   
 [常用 Shape 命令](./shape-commands-in-general.md)