---
description: Join Filters
title: 联接筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], join
- publications [SQL Server replication], join filters
- merge replication join filters [SQL Server replication]
- join filters
ms.assetid: dd78fd8f-56e3-4582-9abd-6bc25c91e075
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a667c6055a43886239102bd9985d06fa714a24d2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470239"
---
# <a name="join-filters"></a>Join Filters
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  联接筛选器允许根据发布中相关表的筛选方式对表进行筛选。 通常用参数化筛选器筛选父表，然后采用与定义各表之间联接完全相同的方式来定义联接筛选器。 联接筛选器扩展了参数化筛选器，以便仅复制与联接筛选器子句匹配的相关表中的数据。  
  
 联接筛选器通常遵循为其应用到的表定义的主键/外键关系，但并不严格受限于主键/外键关系。 联接筛选器可以基于任何逻辑比较两表中相关数据。  
  
 请考虑下列 [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] 示例数据库中的表，这些表通过主键与外键的关系相互关联：  
  
-   **HumanResources.Employee**  
  
-   **Sales.SalesOrderHeader**  
  
-   **Sales.SalesOrderDetail**  
  
 这些表可用在支持移动销售队伍的应用程序中，但必须经过筛选，以使 **HumanResources.Employee** 表中的各销售人员只接收与其客户订单相关的数据。  
  
 第一个步骤是在父表上定义参数化筛选器。本例中的父表是 **HumanResources.Employee** 表。 这个表包括 **LoginID** 列，该列包含 *domain\login* 格式的各个雇员登录名。 若要筛选该表以使每个雇员只接收与他们相关的数据，请指定一个参数化筛选器子句：  
  
```  
LoginID = SUSER_SNAME()  
```  
  
 此筛选器确保每个雇员的订阅仅包含 **HumanResources.Employee** 表中与该雇员相关的数据（这在本例中是一个单行）。 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
 下一步是将此筛选器扩展到各相关表，所使用的语法类似于指定两表间联接所使用的语法。 第一个联接筛选器子句是：  
  
```  
Employee.EmployeeID = SalesOrderHeader.SalesPersonID  
```  
  
 这个子句确保订阅仅包含与每个销售人员相关的订单数据。 第二个联接筛选器子句是：  
  
```  
SalesOrderHeader.SalesOrderID = SalesOrderDetail.SalesOrderID  
```  
  
 这个子句确保订阅仅包含与每个销售人员订单数据相关的详细信息数据。 此例说明的是在各点处联接的单个表。也可以在各点联接多个表。  
  
 通过新建发布向导和 **“发布属性”** 对话框可一次一个地添加联接筛选器，也可用编程的方式添加它们。 也可用新建发布向导自动生成联接筛选器：为一个表指定一个行筛选器，并将联接筛选器应用于所有相关表。 有关详细信息，请参阅[定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)，[自动生成合并项目间的一组联接筛选器 (SQL Server Management Studio)](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)和[定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
## <a name="optimizing-join-filter-performance"></a>优化联接筛选器性能  
 可以按照以下指南来优化联接筛选器性能：  
  
-   限制联接筛选器层次结构中的表数。  
  
     联接筛选器可关联的表数目不限，但有大量表的筛选器会严重影响合并处理过程的性能。 如果要生成五个或更多表的联接筛选器，请考虑其他解决方案：不筛选小表、不会发生更改的表或主要是查找表的表。 仅在必须在订阅中分区的表之间使用联接筛选器。  
  
-   在适当处将“联接唯一键”  选项设置为 **True** 。  
  
     如果父级中的联接列是唯一的，则合并进程的性能有较大的优化空间。 如果联接条件是依据某个唯一列，请为联接筛选器设置“联接唯一键”  选项。 有关设置此选项的详细信息，请参阅前一部分中列出的操作指南主题。  
  
-   确保为联接筛选器中引用的列建立索引。  
  
     如果为筛选器中引用的列建立了索引，复制就可以更高效地处理筛选器。  
  
-   请不要模仿联接筛选器创建行筛选器。  
  
     使用 WHERE 子句中的子查询来模仿联接筛选器创建行筛选器是有可能的，例如：  
  
    ```  
    WHERE Customer.SalesPersonID IN (SELECT EmployeeID FROM Employee WHERE LoginID = SUSER_SNAME())   
    ```  
  
     强烈建议在联接筛选器中而不要在子查询中表示所有这类逻辑。 如果您的应用程序要求行筛选器使用子查询，请确保子查询仅引用不发生更改的查找数据。  
  
## <a name="see-also"></a>另请参阅  
 [为合并复制筛选已发布数据](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)   
 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
