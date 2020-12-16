---
title: 创建数据库快照 (Transact-SQL) | Microsoft Docs
description: 了解如何使用 Transact-SQL 创建 SQL Server 数据库快照。 了解创建快照的先决条件和最佳做法。
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], creating
ms.assetid: 187fbba3-c555-4030-9bdf-0f01994c5230
author: stevestein
ms.author: sstein
ms.openlocfilehash: e427b52c416fe3ca325fd476959827c67ecacaaa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483669"
---
# <a name="create-a-database-snapshot-transact-sql"></a>创建数据库快照 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库快照的唯一方式是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 不支持创建数据库快照。  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 可以使用任何恢复模式的源数据库必须满足以下先决条件：  
  
-   服务器实例必须运行支持数据库快照的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中数据库快照支持的详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
-   源数据库必须处于联机状态，除非该数据库是数据库镜像会话中的镜像数据库。  
  
-   若要在镜像数据库中创建数据库快照，数据库必须处于同步 [镜像状态](../../database-engine/database-mirroring/mirroring-states-sql-server.md)。  
  
-   不能将源数据库配置为可缩放共享数据库。  

::: moniker range="=sql-server-2016||=sql-server-2017"

- 源数据库不得包含 MEMORY_OPTIMIZED_DATA 文件组。

:::moniker-end

> [!IMPORTANT]
> 有关其他重要事项的信息，请参阅 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 本节讨论以下最佳做法：  
  
-   [最佳做法：命名数据库快照](#Naming)  
  
-   [最佳做法：限制数据库快照的数量](#Limiting_Number)  
  
-   [最佳做法：将客户端连接到数据库快照](#Client_Connections)  
  
####  <a name="best-practice-naming-database-snapshots"></a><a name="Naming"></a> 最佳做法：命名数据库快照  
 创建数据库快照之前，考虑如何命名它们是非常重要的。 每个数据库快照都需要一个唯一的数据库名称。 为了便于管理，数据库快照的名称可以包含标识数据库的信息，例如：  
  
-   源数据库的名称。  
  
-   表明新名称用于快照的指示信息。  
  
-   快照的创建日期和时间、序列号或一些其他的信息（例如一天中的某个时间）以区分给定的数据库上的连续快照。  
  
 例如，假设有一系列的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库快照。 在上午 6 时和下午 6 时（基于 24 小时制）之间， 以 6 个小时为间隔创建三个每日快照。 每个每日快照保持 24 小时才被删除并被同一名称的新快照替换。 请注意，每个快照名称指明了小时，而非天：  
  
```  
AdventureWorks_snapshot_0600  
AdventureWorks_snapshot_1200  
AdventureWorks_snapshot_1800  
```  
  
 另外，如果这些每日快照创建的时间每天都变化，则推荐使用不太精确的命名约定，例如：  
  
```  
AdventureWorks_snapshot_morning  
AdventureWorks_snapshot_noon  
AdventureWorks_snapshot_evening  
```  
  
#### <a name="best-practice-limiting-the-number-of-database-snapshots"></a><a name="Limiting_Number"></a> 最佳做法：限制数据库快照的数量  
 随着时间的变化创建一系列快照可捕获源数据库的连续快照。 每个数据库快照会一直保存在系统中，直到被显式删除。 因为每个快照会随着原始页的更新而不断增长，所以您可能想在创建新快照后通过删除旧的快照来节省空间。  
  

**注意！** 若要还原到某个数据库快照，则需要从该数据库中删除所有其他快照。  
  
####  <a name="best-practice-client-connections-to-a-database-snapshot"></a><a name="Client_Connections"></a> 最佳做法：将客户端连接到数据库快照  
 若要使用数据库快照，客户端需要知道它的位置。 正在创建或删除另一个数据库快照时，用户可以从一个数据库快照读取。 但是，如果用新快照替代现有快照，您需要将客户端重新定向到新快照。 用户可以通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]手动连接到数据库快照。 但是，若要支持生产环境，您应该创建一个编程解决方案，该方案透明地将报表编写客户端定向到数据库的最新数据库快照。  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 可创建数据库的任何用户都可以创建数据库快照；但是，若要创建镜像数据库的快照，你必须是 **sysadmin** 固定服务器角色的成员。  
  
##  <a name="how-to-create-a-database-snapshot-using-transact-sql"></a><a name="TsqlProcedure"></a> 如何创建数据库快照（使用 Transact-SQL）  
 **创建数据库快照**  
  
>  有关此过程的示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  根据源数据库的当前大小，确保有足够的磁盘空间存放数据库快照。 数据库快照的最大大小为创建快照时源数据库的大小。 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)。  
  
2.  使用 AS SNAPSHOT OF 子句对文件执行 CREATE DATABASE 语句。 创建快照需要指定源数据库的每个数据库文件的逻辑名称。 语法如下所示：  

     CREATE DATABASE *database_snapshot_name*  
  
     ON  
  
     (  
  
     NAME =*logical_file_name*,  
  
     FILENAME ='*os_file_name*'  
  
     ) [ ,...*n* ]  
  
     AS SNAPSHOT OF *source_database_name*  
  
     [;]  
  
     其中，source_database_name 是源数据库，logical_file_name 是引用该文件时在 SQL Server 中使用的逻辑名称，os_file_name 是创建该文件时操作系统使用的路径和文件名，database_snapshot_name 是将数据库恢复到的快照的名称。 有关该语法的完整描述，请参阅 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)。  
  
    > [!NOTE]  
    >  创建数据库快照时，CREATE DATABASE 语句中不允许有日志文件、脱机文件、还原文件和不起作用的文件。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
  
> [!NOTE]  
>  示例中使用的扩展名 `.ss` 是随意选择的。  
  
 本部分包含以下示例：  
  
-   A. [对 AdventureWorks 数据库创建快照](#Creating_on_AW)  
  
-   B. [对 Sales 数据库创建快照](#Creating_on_Sales)
  
####  <a name="a-creating-a-snapshot-on-the-adventureworks-database"></a><a name="Creating_on_AW"></a> A. 对 AdventureWorks 数据库创建快照  
 此示例对 `AdventureWorks` 数据库创建数据库快照。 快照名称 `AdventureWorks_dbss_1800`及其稀疏文件的名称 `AdventureWorks_data_1800.ss`指明了创建时间 6 P.M.（1800 小时）。  
  
```  
CREATE DATABASE AdventureWorks_dbss1800 ON  
( NAME = AdventureWorks, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks_data_1800.ss' )  
AS SNAPSHOT OF AdventureWorks;  
GO  
```  
  
####  <a name="b-creating-a-snapshot-on-the-sales-database"></a><a name="Creating_on_Sales"></a> B. 对 Sales 数据库创建快照  
 此示例对 `sales_snapshot1200`数据库创建数据库快照 `Sales` 。 此数据库是在 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)中的“创建具有文件组的数据库”示例中创建的。  
  
```  
--Creating sales_snapshot1200 as snapshot of the  
--Sales database:  
CREATE DATABASE sales_snapshot1200 ON  
( NAME = SPri1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri1dat_1200.ss'),  
( NAME = SPri2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri2dt_1200.ss'),  
( NAME = SGrp1Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\mssql\data\SG1Fi1dt_1200.ss'),  
( NAME = SGrp1Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG1Fi2dt_1200.ss'),  
( NAME = SGrp2Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi1dt_1200.ss'),  
( NAME = SGrp2Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi2dt_1200.ss')  
AS SNAPSHOT OF Sales;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [查看数据库快照 (SQL Server)](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [将数据库恢复到数据库快照](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md)  
  
-   [删除数据库快照 (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)  
  
