---
description: 删除表（数据库引擎）
title: 删除表（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table deletions [SQL Server]
- deleting tables
- removing tables
- dropping tables
ms.assetid: ca6aa3e9-9885-44c3-bafc-aec441fd97ec
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 362345ebbccf171f8659b345551a1c2eece5cb11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179791"
---
# <a name="delete-tables-database-engine"></a>删除表（数据库引擎）
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  您可以通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中从您的数据库中删除表。  
  
> [!CAUTION]  
>  删除表之前一定要慎重考虑。 如果现有查询、视图、用户定义函数、存储过程或程序引用该表，删除操作将使这些对象无效。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **使用以下工具删除表：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不能删除被 FOREIGN KEY 约束引用的表。 必须先删除引用 FOREIGN KEY 约束或引用表。 如果要在同一个 DROP TABLE 语句中删除引用表以及包含主键的表，则必须先列出引用表。  
  
-   删除表时，表的规则或默认值将被解除绑定，与该表关联的任何约束或触发器将被自动删除。 如果要重新创建表，则必须重新绑定相应的规则和默认值，重新创建某些触发器，并添加所有必需的约束。  
  
-   如果删除的表包含带有 FILESTREAM 属性的 **varbinary (max)** 列，则不会删除在文件系统中存储的任何数据。  
  
-   不应在同一个批处理中对同一个表执行 DROP TABLE 和 CREATE TABLE。 否则，可能出现意外错误。  
  
-   任何引用已删除表的视图或存储过程都必须显式删除或修改，以便删除对该表的引用。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要拥有该表所属架构的 ALTER 权限、该表的 CONTROL 权限或 **db_ddladmin** 固定数据库角色中的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-a-table-from-the-database"></a>从数据库中删除表  
  
1.  在对象资源管理器中选择要删除的表。  
  
2.  右键单击该表，再从快捷菜单中选择“删除”。  
  
3.  此时，将显示一个消息框，提示您确认删除。 单击 **“是”** 。  

    > [!NOTE]  
    >  删除一个表将自动移除与该表之间的所有关系。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-delete-a-table-in-query-editor"></a>在查询编辑器中删除表  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    DROP TABLE dbo.PurchaseOrderDetail;  
  
    ```  
  
 有关详细信息，请参阅 [DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)。  
  
  
