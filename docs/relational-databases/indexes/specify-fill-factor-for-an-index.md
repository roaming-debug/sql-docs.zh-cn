---
description: 为索引指定填充因子
title: 为索引指定填充因子 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- fill factor [SQL Server]
- page splits [SQL Server]
ms.assetid: 237a577e-b42b-4adb-90cf-aa7fb174f3ab
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: d80ece2cf3b8974f203bf03601dd9c41bfc9bf79
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478138"
---
# <a name="specify-fill-factor-for-an-index"></a>为索引指定填充因子
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  本主题说明什么是填充因子以及如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中指定索引的填充因子值。  
  
 提供填充因子选项是为了优化索引数据存储和性能。 当创建或重新生成索引时，填充因子的值可确定每个叶级页上要填充数据的空间百分比，以便在每一页上保留一些剩余空间作为以后扩展索引的可用空间。 例如，指定填充因子的值为 80 表示每个叶级页上将有 20% 的空间保留为空，以便随着向基础表中添加数据而为扩展索引提供空间。 在索引行之间保留可用空间，而不是在索引的末尾保留。  
  
 填充因子的值是 1 到 100 之间的百分比，服务器范围的默认值为 0，这表示将完全填充叶级页。  
  
> [!NOTE]  
>  填充因子的值 0 和 100 在所有方面都是相同的。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [性能注意事项](#Performance)  
  
     [安全性](#Security)  
  
-   **为索引指定填充因子，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="performance-considerations"></a><a name="Performance"></a> 性能注意事项  
  
#### <a name="page-splits"></a>页拆分  
 正确选择填充因子值可提供足够的空间，以便随着向基础表中添加数据而扩展索引，从而降低页拆分的可能性。如果向已满的索引页添加新行， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将把大约一半的行移到新页中，以便为该新行腾出空间。 这种重组称为页拆分。 页拆分可为新记录腾出空间，但是执行页拆分可能需要花费一定的时间，此操作会消耗大量资源。 此外，它还可能造成碎片，从而导致 I/O 操作增加。 如果经常发生页拆分，可通过使用新的或现有的填充因子值来重新生成索引，从而重新分发数据。 有关详细信息，请参阅 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。  
  
 虽然采用较小的非零填充因子值可减少随着索引的增长而拆分页的需求，但是索引将需要更多的存储空间，并且会降低读取性能。 即使对于面向许多插入和更新操作的应用程序，数据库的读取次数一般也会超过数据库写入次数的 5 到 10 倍。 因此，指定一个不同于默认值的填充因子会降低数据库的读取性能，而降低量与填充因子设置的值成反比。 例如，当填充因子的值为 50 时，数据库的读取性能会降低两倍。 读取性能降低是因为索引包含较多的页，因此增加了检索数据所需的磁盘 I/O 操作。  
  
#### <a name="adding-data-to-the-end-of-the-table"></a>将数据添加到表的末尾  
 如果新数据在表中均匀分布，则不是 0 或 100 的非零填充因子对性能有利。 但是，如果所有数据都添加到表的末尾，则不会填充索引页中的可用空间。 例如，如果索引键列是 IDENTITY 列，则新行的键将总是增加，并且索引行在逻辑意义上将添加到索引的末尾。 如果将用加长行的大小的数据来更新现有行，则请使用小于 100 的填充因子。 每页上的额外字节将有助于把行中的额外长度造成的页拆分降低到最小限度。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-specify-a-fill-factor-by-using-table-designer"></a>使用表设计器指定填充因子  
  
1.  在对象资源管理器中，单击加号以便展开包含你要指定索引填充因子的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  右键单击你要指定索引的填充因子的表，然后选择“设计”。  
  
4.  在“表设计器”菜单上，单击“索引/键”。  
  
5.  选择您要指定填充因子的索引。  
  
6.  展开 **“填充规范”**，选择 **“填充因子”** 行并在行中输入所需的填充因子。  
  
7.  单击“关闭”  。  
  
8.  在“文件”菜单上，选择“保存”以保存 _table_name_。  
  
#### <a name="to-specify-a-fill-factor-in-an-index-by-using-object-explorer"></a>使用对象资源管理器为索引指定填充因子  
  
1.  在对象资源管理器中，单击加号以便展开包含你要指定索引填充因子的表的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以展开要指定索引的填充因子的表。  
  
4.  单击加号以便展开 **“索引”** 文件夹。  
  
5.  右键单击要指定填充因子的索引，然后选择“属性”。  
  
6.  在 **“选择页”** 下，选择 **“选项”** 。  
  
7.  在 **“填充因子”** 行中，输入所需的填充因子。  
  
8.  单击“确定”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-specify-a-fill-factor-in-an-existing-index"></a>在现有索引中指定填充因子  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例重新生成现有索引，并在重新生成操作过程中应用指定的填充因子。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Rebuilds the IX_Employee_OrganizationLevel_OrganizationNode index   
    -- with a fill factor of 80 on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD WITH (FILLFACTOR = 80);   
    GO  
    ```  
  
#### <a name="another-way-to-specify-a-fill-factor-in-an-index"></a>为索引指定填充因子的其他方法  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    /* Drops and re-creates the IX_Employee_OrganizationLevel_OrganizationNode index on the HumanResources.Employee table with a fill factor of 80.  
    */  
  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)   
    WITH (DROP_EXISTING = ON, FILLFACTOR = 80);   
    GO  
    ```  
  
 有关详细信息，请参阅 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)。  
  
  
