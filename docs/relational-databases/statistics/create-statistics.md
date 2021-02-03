---
title: 创建统计信息 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中为表或索引视图的列创建查询优化统计信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.stat.properties.f1
- sql13.swb.statistics.filter.f1
- sql13.swb.stat.columns.f1
- sql13.swb.statistics.propertis.f1
helpviewer_keywords:
- creating statistics
- statistics [SQL Server], creating
ms.assetid: 95a455fb-664d-4c95-851e-c6b62d7ebe04
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba784c0c2fc9733f482a5af09870b48951c6079d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250287"
---
# <a name="create-statistics"></a>创建统计信息
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  您可以通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中为表或索引视图的一个或多个列创建查询优化统计信息。 对于大多数查询，查询优化器已为高质量查询计划生成必要的统计信息；但在少数一些情况下，您需要创建附加的统计信息。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要创建统计信息，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   在使用 CREATE STATISTICS 语句创建统计信息前，请确认在数据库级别设置了 AUTO_CREATE_STATISTICS 选项。 这将确保查询优化器继续定期为查询谓词列创建单列统计信息。  
  
-   每个统计信息对象至多可列出 32 列。  
  
-   不能删除、重命名或更改在筛选的统计信息谓词中定义的表列的定义。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求用户是表或索引视图所有者，或者是以下角色之一的成员： **sysadmin** 固定服务器角色、 **db_owner** 固定数据库角色或 **db_ddladmin** 固定数据库角色。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-statistics"></a>创建统计信息  
  
1.  在 **“对象资源管理器”** 中，单击加号以便展开要创建新的统计信息的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开您要创建新统计信息的表。  
  
4.  右键单击“统计信息”文件夹，然后选择“新建统计信息…” 。  
  
     以下属性将显示在“新建表 _table\_name_ 的统计信息”对话框的“常规”页面上。  
  
     **表名**  
     显示统计信息中所涉及表的名称。  
  
     **统计信息名称**  
     显示存储统计信息的数据库对象的名称。  
  
     **统计信息列**  
     此网格显示该组统计信息中所涉及的列。 网格中的所有值都是只读的。  
  
     **名称**  
     显示统计信息中所涉及列的名称。 它可以是单个的列或单个表中的列组合。  
  
     **数据类型**  
     指示统计信息中所涉及列的数据类型。  
  
     **大小**  
     显示每列的数据类型大小。  
  
     **标识**  
     如果选中此项，则指示标识列。  
  
     **允许 Null 值**  
     指示列是否接受空值。  
  
     **添加**  
     将表中的其他列添加到统计信息网格。  
  
     **删除**  
     从统计信息网格中删除所选列。  
  
     **上移**  
     将所选列移动到统计信息网格中更靠前的位置。 网格中的位置会显著地影响统计信息的有效性。  
  
     **“下移”**  
     将所选列移到统计信息网格中更后面的位置。  
  
     **上次更新了这些列的统计信息:**  
     指示统计信息的存在时间。 如果统计信息是最新的，则会更有价值。 在对数据进行较大的更改或者添加异常数据之后，请更新统计信息。 对于数据分布一致的表，其统计信息更新频率应较低。  
  
     **更新这些列的统计信息**  
     选中此项后将在对话框关闭时更新统计信息。  
  
     以下属性将显示在“新建 _table\_name_ 的统计信息”对话框的“筛选器”页面上。  
  
     **筛选表达式**  
     定义要将哪些数据行包含在筛选的统计信息中。 例如： `Production.ProductSubcategoryID IN ( 1,2,3 )`  
  
5.  在“新建表 _table\_name_ 的统计信息”对话框中，在“常规”页面上，单击“添加”。  
  
     **“选择列”** 对话框中显示以下属性： 此信息为只读信息。  
  
     **名称**  
     显示统计信息中所涉及列的名称。 它可以是单个的列或单个表中的列组合。  
  
     **数据类型**  
     指示统计信息中所涉及列的数据类型。  
  
     **大小**  
     显示每列的数据类型大小。  
  
     **标识**  
     如果选中，则指示标识列。  
  
     **允许 Null**  
     指示列是否接受空值。  
  
6.  在 **“选择列”** 对话框中，选中要为其创建统计信息的每个列旁边的复选框，然后单击 **“确定”** 。  
  
7.  在“新建表 _table\_name_ 的统计信息”对话框中，单击“确定”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-statistics"></a>创建统计信息  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- Create new statistic object called ContactMail1  
    -- on the BusinessEntityID and EmailPromotion columns in the Person.Person table.   
  
    CREATE STATISTICS ContactMail1  
        ON Person.Person (BusinessEntityID, EmailPromotion);   
    GO  
    ```  
  
4.  上面创建的统计信息可能会改进以下查询的结果。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT LastName, FirstName  
    FROM Person.Person  
    WHERE EmailPromotion = 2  
    ORDER BY LastName, FirstName;   
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
  
