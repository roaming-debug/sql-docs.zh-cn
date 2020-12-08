---
title: 查看统计信息属性 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 显示 SQL Server 中表或索引视图的当前查询优化统计信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.statistics.details.f1
helpviewer_keywords:
- viewing statistics properties
- statistics [SQL Server], viewing properties
ms.assetid: 0eaa2101-006e-4015-9979-3468b50e0aaa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ea35cef5c24a73ca8f0e08b055429097bf7d5179
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506511"
---
# <a name="view-statistics-properties"></a>查看统计信息属性
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  您可以通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示 [!INCLUDE[tsql](../../includes/tsql-md.md)]中表或索引视图的当前查询优化统计信息。 统计信息对象包含一个带有统计信息的相关元数据的标题、一个带有统计信息对象第一个键列中的值的分布的直方图，以及一个用于度量各列之间的相关性的密度向量。 有关直方图和密度向量的详细信息，请参阅 [DBCC SHOW_STATISTICS (Transact-SQL) ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要查看统计信息属性，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 若要查看统计信息对象，用户必须是表所有者，或者是 **sysadmin** 固定服务器角色、 **db_owner** 固定数据库角色或 **db_ddladmin** 固定数据库角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-statistics-properties"></a>查看统计信息属性  
  
1.  在 **“对象资源管理器”** 中，单击加号以便展开要创建新的统计信息的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以展开要查看统计信息的属性的表。  
  
4.  单击加号以便展开 **“统计信息”** 文件夹。  
  
5.  双击要查看其属性的统计信息对象，然后选择“属性”。  
  
6.  在“统计信息属性 - statistics_name”对话框的“选择页”窗格中，选择“详细信息” 。  
  
     以下属性将显示在“统计信息属性 - statistics_name”对话框的“详细信息”页上 。  
  
     **表名**  
     显示统计信息中所涉及表的名称。  
  
     **统计信息名称**  
     显示存储统计信息的数据库对象的名称。  
  
     **INDEX 统计信息 statistics_name**  
     此文本框显示从统计信息对象返回的属性。 此属性可分为三个部分：统计信息头、密度向量和直方图。  
  
     下面的信息介绍结果集中为统计信息头返回的列。  
  
     **名称**  
     统计信息对象的名称。  
  
     **已更新**  
     上一次更新统计信息的日期和时间。  
  
     **行**  
     上次更新统计信息时表或索引视图中的总行数。 如果筛选统计信息或者统计信息与筛选索引对应，该行数可能小于表中的行数。  
  
     **Rows Sampled**  
     用于统计信息计算的抽样总行数。 如果 Rows Sampled < Rows，显示的直方图和密度结果则是根据抽样行估计的。  
  
     **步骤**  
     直方图中的梯级数。 每个梯级都跨越一个列值范围，后跟上限列值。 直方图梯级是根据统计信息中的第一个键列定义的。 最大梯级数为 200。  
  
     **Density**  
     计算公式为 1/统计信息对象第一个键列中的所有值（不包括直方图边界值）的非重复值。 查询优化器不使用此 Density 值，显示此值的目的是为了与 SQL Server 2008 之前的版本实现向后兼容。  
  
     **Average Key Length**  
     统计信息对象中所有键列的每个值的平均字节数。  
  
     **String Index**  
     Yes 指示统计信息对象包含字符串摘要统计信息，以改进对使用 LIKE 运算符的查询谓词的基数估计；例如 `WHERE ProductName LIKE '%Bike'`。 字符串摘要统计信息与直方图分开存储，如果统计信息对象为 **char**、 **varchar**、 **nchar**、 **nvarchar**、 **varchar(max)** 、 **nvarchar(max)** 、 **text** 或 **ntext** 类型，则基于其第一个键列创建字符串摘要统计信息。  
  
     **筛选表达式**  
     包含在统计信息对象中的表行子集的谓词。 NULL = 未筛选的统计信息。  
  
     **Unfiltered Rows**  
     应用筛选表达式前表中的总行数。 如果筛选表达式为 NULL，则 Unfiltered Rows 等于 Rows。  
  
     下面的信息介绍结果集中为密度向量返回的列。  
  
     **All Density**  
     密度为 1/非重复值。 结果显示统计信息对象中各列的每个前缀的密度，每个密度显示一行。 非重复值是每个行前缀和列前缀的列值的非重复列表。 例如，如果统计信息对象包含键列 (A, B, C)，结果将报告以下每个列前缀中非重复值列表的密度：(A)、(A,B) 和 (A, B, C)。 使用前缀 (A, B, C)，以下每个列表都是一个非重复值列表：(3, 5, 6)、(4, 4, 6)、(4, 5, 6) 和 (4, 5, 7)。 使用前缀 (A, B)，相同列值具有以下非重复值列表：(3, 5)、(4, 4) 和 (4, 5)。  
  
     **Average Length**  
     存储列前缀的列值列表的平均长度（以字节为单位）。 例如，如果列表 (3, 5, 6) 中的每个值都需要 4 个字节，则长度为 12 个字节。  
  
     **“列”**  
     为其显示 All density 和 Average length 的前缀中的列的名称。  
  
     下面的信息介绍结果集中为直方图返回的列。  
  
     **RANGE_HI_KEY**  
     直方图梯级的上限列值。 列值也称为键值。  
  
     **RANGE_ROWS**  
     其列值位于直方图梯级内（不包括上限）的行的估算数目。  
  
     **EQ_ROWS**  
     其列值等于直方图梯级的上限的行的估算数目。  
  
     **DISTINCT_RANGE_ROWS**  
     非重复列值位于直方图梯级内（不包括上限）的行的估算数目。  
  
     **AVG_RANGE_ROWS**  
     重复列值位于直方图梯级内（不包括上限）的平均行数（如果 DISTINCT_RANGE_ROWS > 0，则为 RANGE_ROWS / DISTINCT_RANGE_ROWS）。  
  
7.  单击“确定”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-statistics-properties"></a>查看统计信息属性  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- The following example displays all statistics information for the AK_Address_rowguid index of the Person.Address table.   
    DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);   
    GO  
    ```  
  
 有关详细信息，请参阅 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)。  
  
#### <a name="to-find-all-of-the-statistics-on-a-table-or-view"></a>查找表或视图上的所有统计信息  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    /*Gets the following information: name and ID of the statistics, whether the statistics were created automatically or by the user, whether the statistics were created with the NORECOMPUTE option, and whether the statistics have a filter and, if so, what that filter is.  
    */  
    SELECT name AS statistics_name  
        ,stats_id  
        ,auto_created  
        ,user_created  
        ,no_recompute  
        ,has_filter  
        ,filter_definition  
    -- using the sys.stats catalog view  
    FROM sys.stats  
    -- for the Sales.SpecialOffer table  
    WHERE object_id = OBJECT_ID('Sales.SpecialOffer');  
    GO  
    ```  
  
 有关详细信息，请参阅 [sys.stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)。  
  
  
