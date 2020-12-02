---
description: 将现有索引移动到其他文件组中
title: 将现有索引移动到其他文件组 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- moving tables
- switching filegroups for index
- moving indexes
- indexes [SQL Server], moving
- filegroups [SQL Server], switching
ms.assetid: 167ebe77-487d-4ca8-9452-4b2c7d5cb96e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: abbd77be3e20cc69bc12cd69f07fa82ec99d95db
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470452"
---
# <a name="move-an-existing-index-to-a-different-filegroup"></a>将现有索引移动到其他文件组中
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中将现有索引从其当前文件组移动到其他文件组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要将现有索引移动到其他文件组中，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果表具有聚集索引，则将此聚集索引移动到新文件组的同时也会将表移动到该文件组。  
  
-   不能通过使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]来移动使用 UNIQUE 或 PRIMARY KEY 约束创建的索引。 若要移动这些索引，请在 [中使用](../../t-sql/statements/create-index-transact-sql.md) CREATE INDEX [!INCLUDE[tsql](../../includes/tsql-md.md)]语句以及 (DROP_EXISTING=ON) 选项。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup-using-table-designer"></a>使用表设计器将现有索引移到其他文件组  
  
1.  在“对象资源管理器”中，单击加号以展开包含该特定表的数据库，该表包含您要移动的索引。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  右键单击您要移动的索引的表，然后选择“设计”。  
  
4.  在“表设计器”菜单上，单击“索引/键”。  
  
5.  选择要移动的索引。  
  
6.  在主网格中，展开 **“数据空间规范”**。  
  
7.  选择 **“文件组或分区方案名称”** 并从列表中选择要将索引移动到的文件组或分区方案。  
  
8.  单击“关闭”  。  
  
9. 在“文件”菜单上，选择“保存”以保存 _table_name_。  

#### <a name="to-move-an-existing-index-to-a-different-filegroup-in-object-explorer"></a>在“对象资源管理器”中将现有索引移到其他文件组  
  
1.  在“对象资源管理器”中，单击加号以展开包含该特定表的数据库，该表包含您要移动的索引。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以展开包含您要移动的索引的表。  
  
4.  单击加号以便展开 **“索引”** 文件夹。  
  
5.  右键单击要移动的索引，然后选择“属性”。  
  
6.  在 **“选择页”** 下，选择 **“存储”**。  
  
7.  选择移动此索引的目标文件组。  
  
     如果表或索引已分区，则选择要在其中移动索引的分区架构。 有关已分区索引的详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
     如果要移动聚集索引，则可以使用联机处理。 联机处理使并发用户可以在索引操作期间访问基础数据和非聚集索引。 有关详细信息，请参阅 [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md)。  
  
     在使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]的多处理器计算机上，可以通过指定最大的并行度值来配置用于执行索引语句的处理器数。 并非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本均提供并行索引操作功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅“SQL Server 2016 各个版本支持的功能”。 有关并行索引操作的详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
8.  单击“确定”。   
  
 “索引属性 - index_name”对话框的“存储”页面中提供了以下信息 ：  
  
 **文件组**  
 在指定的文件组中存储索引。 该列表仅显示标准 (row) 文件组。 默认情况下，将在该列表中选择相应数据库的 PRIMARY 文件组。  
  
 **Filestream 文件组**  
 指定 FILESTREAM 数据的文件组。 该列表仅显示 FILESTREAM 文件组。 默认情况下，将在该列表中选择 PRIMARY FILESTREAM 文件组。  
  
 **分区方案**  
 在分区方案中存储索引。 单击 **“分区方案”** 可启用下面的网格。 默认情况下，将在该列表中选择用于存储表数据的分区方案。 在列表中选择其他分区方案将更新该网格中的信息。  
  
 如果数据库中没有分区方案，则分区方案选项不可用。  
  
 **Filestream 分区方案**  
 指定 FILESTREAM 数据的分区方案。 分区方案必须与 **“分区方案”** 选项中指定的方案对称。  
  
 如果表未分区，则该字段为空白。  
  
 **分区方案参数**  
 显示构成分区方案的列的名称。  
  
 **表列**  
 选择要映射到分区方案的表或视图。  
  
 **列数据类型**  
 显示有关列的数据类型信息。  
  
> [!NOTE]  
>  如果表列是计算列，则 **“列数据类型”** 显示为“计算列”。  
  
 **允许在移动索引时在线处理 DML 语句**  
 在索引操作过程中，允许用户访问基础表或聚集索引数据以及任何相关联的非聚集索引。  
  
> [!NOTE]  
>  此选项对 XML 索引不可用，或者如果索引为禁用的聚集索引，此选项也不可用。  
  
 **设置最大并行度**  
 限制执行并行计划时所使用的处理器数。 默认值为 0，表示使用实际可用的 CPU 数。 若将此值设置为 1，则取消生成并行计划；若将此值设置为大于 1 的数，则会限制单个查询执行使用的最多处理器数。 该选项仅在此对话框处于 **“重新生成”** 或 **“重新创建”** 状态时才可用。  
  
> [!NOTE]  
>  如果指定的值比可用 CPU 数大，则将使用实际的可用 CPU 数。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup"></a>将现有索引移动到其他文件组中  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Creates the TransactionsFG1 filegroup on the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP TransactionsFG1;  
    GO  
    /* Adds the TransactionsFG1dat3 file to the TransactionsFG1 filegroup. Please note that you will have to change the filename parameter in this statement to execute it without errors.  
    */  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = TransactionsFG1dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\DATA\TransactionsFG1dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP TransactionsFG1;  
    GO  
    /*Creates the IX_Employee_OrganizationLevel_OrganizationNode index  
      on the TransactionsPS1 filegroup and drops the original IX_Employee_OrganizationLevel_OrganizationNode index.  
    */  
    CREATE NONCLUSTERED INDEX IX_Employee_OrganizationLevel_OrganizationNode  
        ON HumanResources.Employee (OrganizationLevel, OrganizationNode)  
        WITH (DROP_EXISTING = ON)  
        ON TransactionsFG1;  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
  
