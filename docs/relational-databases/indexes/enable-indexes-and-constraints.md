---
description: 启用索引和约束
title: 启用索引和约束 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], enabling
- nonclustered indexes [SQL Server], enabling a disabled index
- index enabling [SQL Server]
- disabled indexes [SQL Server], how to enable
- constraints [SQL Server], enabling
- clustered indexes, enabling disabled indexes
ms.assetid: c55c8865-322e-4ab0-ba04-ea1f56735353
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9df5eb968e329f67fd9a98f7ddb1d4eff027a35
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460360"
---
# <a name="enable-indexes-and-constraints"></a>启用索引和约束
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题介绍了如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中启用已禁用的索引。 索引被禁用后一直保持禁用状态，直到它重新生成或删除。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要启用禁用的索引，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   在索引重新生成之后，任何因禁用索引而被禁用的约束必须手动将其启用。 PRIMARY KEY 和 UNIQUE 约束可通过重新生成相关联的索引来启用。 您必须重新生成（启用）索引才能启用引用 PRIMARY KEY 或 UNIQUE 约束的 FOREIGN KEY 约束。 FOREIGN KEY 约束可使用 ALTER TABLE CHECK CONSTRAINT 语句来启用。  
  
-   当 ONLINE 选项设置为 ON 时，不能重新生成禁用的聚集索引。  
  
-   当禁用或启用聚集索引，禁用非聚集索引时，对聚集索引操作会对禁用的非聚集索引有如下影响。  
  
    |聚集索引操作|禁用的非聚集索引…|  
    |----------------------------|-----------------------------------|  
    |ALTER INDEX REBUILD。|保持禁用状态。|  
    |ALTER INDEX ALL REBUILD。|重新生成或启用。|  
    |DROP INDEX。|保持禁用状态。|  
    |CREATE INDEX WITH DROP_EXISTING。|保持禁用状态。|  
  
     创建一个新的聚集索引，其行为与 ALTER INDEX ALL REBUILD 相同。  
  
-   与聚集索引相关联的非聚集索引允许的操作，取决于这两种索引类型的状态是禁用还是启用。 下表概括了非聚集索引允许的操作。  
  
    |非聚集索引操作|聚集和非聚集索引均禁用时。|聚集索引启用而非聚集索引处于禁用或启用中的任一状态时。|  
    |-------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
    |ALTER INDEX REBUILD。|操作失败。|操作成功。|  
    |DROP INDEX。|操作成功。|操作成功。|  
    |CREATE INDEX WITH DROP_EXISTING。|操作失败。|操作成功。|  

-   重新生成禁用的压缩非聚集索引时，data_compression 默认为“none”，表示该索引要解压缩。 这是因为禁用非聚集索引后，压缩设置元数据会丢失。 要解决此问题，必须在重新生成语句中指定显式数据压缩。

###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 如果使用 DBCC DBREINDEX，用户必须拥有该表；或者是 sysadmin 固定服务器角色的成员；或者是 db_ddladmin 和 db_owner 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-enable-a-disabled-index"></a>启用禁用的索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要启用索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要启用索引的表。  
  
4.  单击加号以便展开 **“索引”** 文件夹。  
  
5.  右键单击要启用的索引，然后选择“重新生成”。  
  
6.  在 **“重新生成索引”** 对话框中，确认正确的索引位于 **“要重新生成的索引”** 网格中，然后单击 **“确定”** 。  
  
#### <a name="to-enable-all-indexes-on-a-table"></a>为表启用所有索引  
  
1.  在对象资源管理器中，单击加号以便展开包含您要启用索引的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要启用索引的表。  
  
4.  右键单击“索引”文件夹，然后选择“全部重新生成”。  
  
5.  在 **“重新生成索引”** 对话框中，确认正确的索引位于 **“要重新生成的索引”** 网格中，然后单击 **“确定”**。 若要从 **“要重新生成的索引”** 网格中删除索引，请选择该索引，再按 Delete 键。  
  
 在 **“重新生成索引”** 对话框中将提供以下信息：  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-enable-a-disabled-index-using-alter-index"></a>使用 ALTER INDEX 启用已禁用的索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD;   
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-create-index"></a>使用 CREATE INDEX 启用已禁用的索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- re-creates the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    -- using the OrganizationLevel and OrganizationNode columns  
    -- and then deletes the existing IX_Employee_OrganizationLevel_OrganizationNode index  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)  
    WITH (DROP_EXISTING = ON);  
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-dbcc-dbreindex"></a>使用 DBCC DBREINDEX 启用已禁用的索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", IX_Employee_OrganizationLevel_OrganizationNode);  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-alter-index"></a>使用 ALTER INDEX 启用表上的所有索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    ALTER INDEX ALL ON HumanResources.Employee  
    REBUILD;  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-dbcc-dbreindex"></a>使用 DBCC DBREINDEX 启用表上的所有索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", " ");  
    GO  
    ```  
  
 有关详细信息，请参阅 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)、[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md) 和 [DBCC DBREINDEX (Transact-SQL)](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)。  
  
  
