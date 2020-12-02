---
description: 修改索引
title: 修改索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 15a7386c08d2b9147858cab3fde212cf285902cb
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88499357"
---
# <a name="modify-an-index"></a>修改索引
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  本主题将说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改索引。  
  
> [!IMPORTANT]  
>  不能通过此方法修改作为 PRIMARY KEY 或 UNIQUE 约束的结果而创建的索引， 而必须修改约束。  
  
 **本主题内容**  
  
-   **若要修改索引，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>修改索引  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”**，展开该表所属的数据库，再展开 **“表”**。  
  
3.  展开该索引所属的表，再展开 **“索引”**。  
  
4.  右键单击要修改的索引，然后单击“属性”。  
  
5.  在 **“索引属性”** 对话框中进行所需的更改。 例如，您可以从索引键中添加或删除列，或更改索引选项的设置。  
  
#### <a name="to-modify-index-columns"></a>修改索引列  
  
1.  若要添加、删除或更改索引列的位置，请从 **“索引属性”** 对话框中选择 **“常规”** 页。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-modify-an-index"></a>修改索引  
  
下面的示例使用 `DROP_EXISTING` 选项，删除 AdventureWorks 数据库中 `Production.WorkOrder` 表的 `ProductID` 列上的现有索引并重新创建。 还设置了 `FILLFACTOR` 和 `PAD_INDEX` 选项。  
  
[!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
下面的示例使用 ALTER INDEX 为索引 `AK_SalesOrderHeader_SalesOrderNumber`设置了几个选项。  
  
[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>修改索引列  
  
1.  若要添加、删除或更改索引列的位置，您必须删除并重新创建该索引。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [设置索引选项](../../relational-databases/indexes/set-index-options.md)   
 [重命名索引](../../relational-databases/indexes/rename-indexes.md)  
  
  
