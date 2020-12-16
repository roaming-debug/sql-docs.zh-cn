---
description: 创建唯一索引
title: 创建唯一索引 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- unique indexes
- designing indexes [SQL Server], unique
- clustered indexes, unique
- indexes [SQL Server], unique
- nonclustered indexes [SQL Server], unique
- unique indexes, design guidelines
ms.assetid: 56b5982e-cb94-46c0-8fbb-772fc275354a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46b70b81745047bc85f2d7bbc677321dde618c81
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480058"
---
# <a name="create-unique-indexes"></a>创建唯一索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建表的唯一索引。 唯一索引能够保证索引键中不包含重复的值，从而使表中的每一行从某种方式上具有唯一性。 创建 UNIQUE 约束和创建与约束无关的唯一索引并没有明显的区别。 进行数据验证的方式相同，而且对于唯一索引是由约束创建的还是手动创建的，查询优化器并不加以区分。 但是，创建列的 UNIQUE 约束会使索引目标更清晰。 有关 UNIQUE 约束的详细信息，请参阅 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md)。  
  
 在创建唯一索引时，可以设置一个忽略重复键的选项。 如果此选项已设置为“是”，当你试图通过添加影响多行的数据来创建重复键（使用 INSERT 语句）时，则不会添加包含重复项的行。 如果此选项设置为 **“否”**，则整个插入操作将失败，并且将回滚所有数据。  
  
> [!NOTE]  
>  如果单个列在多行中包含 NULL，则无法对该列创建唯一索引。 同样，如果列的组合在多行中包含 NULL，则无法对多个列创建唯一索引。 在进行索引时，它们都被视为重复值。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [唯一索引的优点](#Benefits)  
  
     [典型实现](#Implementations)  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建表的唯一索引，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="benefits-of-a-unique-index"></a><a name="Benefits"></a> 唯一索引的优点  
  
-   多列唯一索引能够保证索引键中值的每个组合都是唯一的。 例如，如果为 **LastName**、 **FirstName** 和 **MiddleName** 列的组合创建了唯一索引，则表中的任意两行都不会有这些列值的相同组合。  
  
-   只要每个列中的数据是唯一的，就可以为同一个表创建一个唯一聚集索引和多个唯一非聚集索引。  
  
-   唯一索引能够确保定义的列的数据完整性。  
  
-   唯一索引提供帮助查询优化器生成更高效的执行计划的其他信息。  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> 典型实现  
 唯一索引可通过以下方式实现：  
  
-   **PRIMARY KEY 或 UNIQUE 约束**  
  
     在创建 PRIMARY KEY 约束时，如果不存在该表的聚集索引且未指定唯一非聚集索引，则将自动对一列或多列创建唯一聚集索引。 主键列不允许空值。  
  
     在创建 UNIQUE 约束时，默认情况下将创建唯一非聚集索引，以便强制 UNIQUE 约束。 如果不存在该表的聚集索引，则可以指定唯一聚集索引。  
  
     有关详细信息，请参阅 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md) 和 [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md)。  
  
-   **独立于约束的索引**  
  
     可以为一个表定义多个唯一非聚集索引。  
  
     有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
-   **索引视图**  
  
     若要创建索引视图，请对一个或多个视图列定义唯一聚集索引。 视图将执行，并且结果集存储在该索引的页级别中，其存储方式与表数据存储在聚集索引中的方式相同。 有关详细信息，请参阅 [创建索引视图](../../relational-databases/views/create-indexed-views.md)。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果数据中存在重复的键值，则不能创建唯一索引、UNIQUE 约束或 PRIMARY KEY 约束。  
  
-   唯一非聚集索引可以包括包含性非键列。 有关详细信息，请参阅 [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-unique-index-by-using-the-table-designer"></a>使用表设计器创建唯一索引  
  
1.  在“对象资源管理器”中，展开包含您要创建唯一索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  右键单击你要创建唯一索引的表，然后选择“设计”。  
  
4.  在“表设计器”菜单上，选择“索引/键”。  
  
5.  在“索引/键”对话框中，单击“添加”。  
  
6.  从“选定的主/唯一键或索引”文本框中选择新索引。  
  
7.  在主网格中，在“(常规)”下，选择“类型”，然后从列表中选择“索引”。  
  
8.  选择“列”，然后单击省略号 (…)。  
  
9. 在 **“索引列”** 对话框中的 **“列名”** 下，选择要编制索引的列。 最多可选择 16 列。 为获得最佳的性能，请只为每个索引选择一列或两列。 对于所选的每一列，指定索引是以升序还是以降序来排列此列的值。  
  
10. 选择索引的所有列后，单击 **“确定”**。  
  
11. 在主网格中，在“(常规)”下，选择“是唯一的”，然后从列表中选择“是”。  
  
12. 可选：在主网格中，在 **“表设计器”** 下，选择 **“忽略重复键”** ，然后从列表中选择 **“是”** 。 如果要忽略尝试添加导致唯一索引中有重复键的数据，请这样做。  
  
13. 单击“关闭”。  
  
14. 在“文件”菜单上，单击“保存 _table\_name_”。  
  
#### <a name="create-a-unique-index-by-using-object-explorer"></a>使用对象资源管理器创建唯一索引  
  
1.  在“对象资源管理器”中，展开包含您要创建唯一索引的表的数据库。  
  
2.  展开 **“表”** 文件夹。  
  
3.  展开要为其创建唯一索引的表。  
  
4.  右键单击“索引”文件夹，指向“新建索引”，然后选择“非群集索引…”  。  
  
5.  在 **“新建索引”** 对话框的 **“常规”** 页中，在 **“索引名称”** 框中输入新索引的名称。  
  
6.  选中 **“唯一”** 复选框。  
  
7.  在“索引键列”下，单击“添加…” 。  
  
8.  在“从 table\_name 选择列”对话框中，选中要添加到唯一索引的一个或多个表列的复选框。  
  
9. 单击“确定”。  
  
10. 在 **“新建索引”** 对话框中，单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-unique-index-on-a-table"></a>创建表的唯一索引  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named AK_UnitMeasure_Name and delete it if found  
    IF EXISTS (SELECT name from sys.indexes  
               WHERE name = N'AK_UnitMeasure_Name')   
       DROP INDEX AK_UnitMeasure_Name ON Production.UnitMeasure;   
    GO  
    -- Create a unique index called AK_UnitMeasure_Name  
    -- on the Production.UnitMeasure table using the Name column.  
    CREATE UNIQUE INDEX AK_UnitMeasure_Name   
       ON Production.UnitMeasure (Name);   
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
  
