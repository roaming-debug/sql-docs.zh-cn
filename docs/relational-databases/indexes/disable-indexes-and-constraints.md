---
description: 禁用索引和约束
title: 禁用索引和约束 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.disableindexes.f1
helpviewer_keywords:
- disabled indexes [SQL Server], index operations
- nonclustered indexes [SQL Server], disabling
- disabled indexes [SQL Server], guidelines
- clustered indexes, disabling
- constraints [SQL Server], disabling
- disabled indexes [SQL Server], viewing
- FOREIGN KEY constraints, disabling
- statistical information [SQL Server], indexes
- index disabling [SQL Server]
- indexed views [SQL Server], disabled indexes
ms.assetid: 2198f1af-fa44-47e9-92df-f4fde322ba18
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97718ff757e09cd33116c47f31155f6ae0edb08b
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048890"
---
# <a name="disable-indexes-and-constraints"></a>禁用索引和约束
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中禁用索引或约束。 禁用索引可以防止用户访问索引，而对于聚集索引，则可以防止用户访问基础表数据。 索引定义保留在元数据中，非聚集索引的索引统计信息仍保留。 对视图禁用非聚集索引或聚集索引会以物理方式删除索引数据。 禁用表的聚集索引可以防止对数据的访问，数据仍保留在表中，但在删除或重新生成索引之前，无法对这些数据执行数据操作语言 (DML) 操作。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **禁用索引，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   索引处于禁用状态时，不对其进行维护。  
  
-   查询优化器创建查询执行计划时不考虑禁用的索引。 另外，引用包含表提示的已禁用索引的查询将失败。  
  
-   无法创建与现有禁用索引同名的索引。  
  
-   可以删除已禁用索引。  
  
-   禁用唯一索引时，还将禁用 PRIMARY KEY 约束或 UNIQUE 约束及引用其他表中的索引列的所有 FOREIGN KEY 约束。 禁用聚集索引时，还将禁用基础表的所有传入和传出的 FOREIGN KEY 约束。 索引处于禁用状态时，会在警告消息中列出约束名称。 重新生成索引后，必须使用 ALTER TABLE CHECK CONSTRAINT 语句手动启用所有约束。  
  
-   相关联的聚集索引处于禁用状态时，会自动禁用非聚集索引。 表或视图的聚集索引处于启用状态或删除了表的聚集索引时，才能启用这些非聚集索引。 除非使用 ALTER INDEX ALL REBUILD 语句启用了聚集索引，否则必须显式启用非聚集索引。  
  
-   ALTER INDEX ALL REBUILD 语句将重新生成并启用表的所有已禁用索引（视图的已禁用索引除外）。 视图的索引必须另外使用 ALTER INDEX ALL REBUILD 语句来启用。  
  
-   禁用表的聚集索引时，还将禁用针对引用该表的视图的所有聚集索引和非聚集索引。 必须像重新生成被引用表的索引那样重新生成这些索引。  
  
-   不能访问已禁用的聚集索引的数据行，但可以删除或重新生成该聚集索引。  
  
-   表中没有已禁用的聚集索引时，可以联机重新生成已禁用的非聚集索引。 但是，如果使用 ALTER INDEX REBUILD 语句或 CREATE INDEX WITH DROP_EXISTING 语句，则务必脱机重新生成已禁用的聚集索引。 有关联机索引操作的详细信息，请参阅 [联机执行索引操作](../../relational-databases/indexes/perform-index-operations-online.md)。  
  
-   无法对包含已禁用的聚集索引的表成功执行 CREATE STATISTICS 语句。  
  
-   当索引处于禁用状态且存在以下条件时，AUTO_CREATE_STATISTICS 数据库选项将对列创建新的统计信息：  
  
    -   AUTO_CREATE_STATISTICS 设置为 ON  
  
    -   当前还没有列的相关统计信息。  
  
    -   在查询优化过程中需要统计信息。  
  
-   如果聚集索引处于禁用状态， [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 无法返回有关基础表的信息；该语句将报告聚集索引已禁用。 [DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) 不能用于对已禁用的索引进行碎片整理；该语句将失败并显示错误消息。 可以使用 [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md) 重新生成已禁用索引。  
  
-   创建一个新的聚集索引将启用以前禁用的非聚集索引。 有关详细信息，请参阅 [Enable Indexes and Constraints](../../relational-databases/indexes/enable-indexes-and-constraints.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 若要执行 ALTER INDEX，至少需要对表或视图具有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-disable-an-index"></a>禁用索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要禁用索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要禁用索引的表。  
  
4.  单击加号以便展开 **“索引”** 文件夹。  
  
5.  右键单击要禁用的索引，然后选择“禁用”。  
  
6.  在 **“禁用索引”** 对话框中，确认正确的索引位于 **“要禁用的索引”** 网格中，然后单击 **“确定”**。  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>禁用表的所有索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要禁用索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要禁用索引的表。  
  
4.  右键单击“索引”文件夹，然后选择“全部禁用”。  
  
5.  在 **“禁用索引”** 对话框中，确认正确的索引位于 **“要禁用的索引”** 网格中，然后单击 **“确定”**。 若要从 **“要禁用的索引”** 网格中删除索引，请选择该索引，再按 Delete 键。  
  
 在 **“禁用索引”** 对话框中将提供以下信息：  
  
 **Index Name**  
 显示索引的名称。 执行期间，此列还会显示表示状态的图标。  
  
 **表名**  
 显示创建索引的表或视图的名称。  
  
 **索引类型**  
 显示索引的类型：“聚集”、“非聚集”、“空间”或 ”XML”。  
  
 **Status**  
 显示禁用操作的状态。 执行之后可能的值包括：  
  
-   空白  
  
     执行之前， **“状态”** 为空白。  
  
-   **正在学习**  
  
     禁用索引操作已启动，但尚未完成。  
  
-   **Success**  
  
     禁用操作已成功完成。  
  
-   **错误**  
  
     禁用索引操作过程中遇到错误，操作未成功完成。  
  
-   **已停止**  
  
     用户已停止禁用索引操作，该操作未成功完成。  
  
 **消息**  
 禁用操作期间提供错误消息文本。 执行过程中，错误显示为超链接。 超链接的文本描述错误的正文。 **“消息”** 列宽度一般不够，无法阅读完整的消息文本。 获取完整文本的方法有两种：  
  
-   将鼠标指针移到消息单元上以显示包含错误文本的工具提示。  
  
-   单击超链接以显示一个对话框，显示完整的错误。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-disable-an-index"></a>禁用索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- disables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    DISABLE;  
    ```  
  
#### <a name="to-disable-all-indexes-on-a-table"></a>禁用表的所有索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Disables all indexes on the HumanResources.Employee table.  
    ALTER INDEX ALL ON HumanResources.Employee  
    DISABLE;  
    ```  
  
 有关详细信息，请参阅 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)。  
  
  
