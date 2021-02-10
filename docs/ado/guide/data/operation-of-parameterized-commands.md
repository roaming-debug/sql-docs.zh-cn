---
description: 参数化命令的操作
title: 参数化命令的操作 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
ms.assetid: 4fae0d54-83b6-4ead-99cc-bcf532daa121
author: rothja
ms.author: jroth
ms.openlocfilehash: 55eaf3798ee8d14a776da14010b4b6617d0e2723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032649"
---
# <a name="operation-of-parameterized-commands"></a>参数化命令的操作
如果使用的是大型子 **记录集**（特别是与父 **记录集** 的大小相比），但只需访问几个子章节，则可以发现使用参数化命令更有效。  
  
 *非参数化命令* 将检索整个父记录集和子 **记录集**，并向父记录集追加章节列，然后为每个父行分配对相关子章节的引用。  
  
 *参数化命令* 将检索整个父 **记录集**，但在访问章节列时，仅检索章节 **记录集**。 检索策略的这种差异可能会显著提高性能。  
  
 例如，你可以指定以下内容：  
  
```  
SHAPE {SELECT * FROM customer}   
   APPEND ({SELECT * FROM orders WHERE cust_id = ?}   
   RELATE cust_id TO PARAMETER 0)  
```  
  
 父表和子表的列名称相同， *cust_id*。 *子命令* 有一个 "？" 占位符，其中关系子句引用的是 "..." (参数 0 ") 。  
  
> [!NOTE]
>  PARAMETER 子句仅适用于 shape 命令语法。 它不与 ADO [参数](../../reference/ado-api/parameter-object.md) 对象或 [参数](../../reference/ado-api/parameters-collection-ado.md) 集合关联。  
  
 执行参数化形状命令时，将发生以下情况：  
  
1.  执行 *父-命令* 并返回 Customers 表中的父 **记录集** 。  
  
2.  章节列将追加到父 **记录集**。  
  
3.  访问父行的章节列时，将使用 customer.cust_id 的值作为参数的值来执行 *子命令* 。  
  
4.  在步骤3中创建的数据提供程序行集中的所有行都用于填充子 **记录集**。 在此示例中，这是 Orders 表中的所有行，其中 cust_id 等于 customer.cust_id 的值。 默认情况下，将在客户端上缓存子 **记录集**，直到释放对父 **记录集** 的所有引用。 若要更改此行为，请将 **记录集**[动态属性](../../reference/ado-api/ado-dynamic-property-index.md)**缓存子行** 设置为 **False**。  
  
5.  对检索到的子行的引用 (也就是说，子 **记录集**) 的章节位于父 **记录集** 的当前行的章节列中。  
  
6.  访问另一行的章节列时，将重复执行步骤3-5。  
  
 默认情况下，" **缓存子行** " 动态属性设置为 **True** 。 缓存行为根据查询的参数值而有所不同。 在带有单个参数的查询中，给定参数值的子 **记录集** 将缓存在具有该值的子级的请求之间。 以下代码对此做了演示：  
  
```  
SCmd = "SHAPE {select * from customer} " & _  
         "APPEND({select * from orders where cust_id = ?} " & _  
         "RELATE cust_id TO PARAMETER 0) AS chpCustOrder"  
Rst1.Open sCmd, Cnn1  
Set RstChild = Rst1("chpCustOrder").Value  
Rst1.MoveNext      ' Next cust_id passed to Param 0, & new rs fetched   
                   ' into RstChild.  
Rst1.MovePrevious  ' RstChild now holds cached rs, saving round trip.  
```  
  
 在包含两个或更多参数的查询中，仅当所有参数值都与缓存的值匹配时，才使用缓存的子。  
  
## <a name="parameterized-commands-and-complex-parent-child-relations"></a>参数化命令和复杂的父子关系  
 除了使用参数化命令提高同等联接类型层次结构的性能外，参数化命令还可用于支持更复杂的父子关系。 例如，假设有一个包含两个表的小一个 "联盟" 数据库：一个团队 (team_id、team_name) 以及游戏 (日期 home_team visiting_team) 的另一个。  
  
 使用非参数化层次结构时，无法将 "团队" 和 "游戏" 表关联起来，因为每个团队的子 **记录集** 都包含其完整计划。 您可以创建包含本计划或公路计划的章节，但不能同时包含这两者。 这是因为，关系子句会限制 (pc1 = cc1) 和 (pc2 = pc2) 形式的父子关系。 因此，如果你的命令包含 "将 team_id 关联到 home_team，team_id 到 visiting_team"，则只会获得一个团队在玩自己的游戏。 所需的是 " (team_id = home_team) 或 (team_id = visiting_team) "，但形状提供程序不支持或子句。  
  
 若要获得所需的结果，可以使用参数化命令。 例如：  
  
```  
SHAPE {SELECT * FROM teams}   
APPEND ({SELECT * FROM games WHERE home_team = ? OR visiting_team = ?}   
        RELATE team_id TO PARAMETER 0,   
               team_id TO PARAMETER 1)   
```  
  
 此示例利用了 SQL WHERE 子句的更大灵活性来获得所需的结果。  
  
> [!NOTE]
>  使用 WHERE 子句时，参数不能使用 text、ntext 和 image 的 SQL 数据类型，也不会生成包含以下说明的错误： `Invalid operator for data type` 。  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](./data-shaping-example.md)   
 [正式形状语法](./formal-shape-grammar.md)   
 [常用 Shape 命令](./shape-commands-in-general.md)