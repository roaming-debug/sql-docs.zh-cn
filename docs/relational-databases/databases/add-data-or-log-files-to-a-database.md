---
title: 向数据库中添加数据文件或日志文件 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 将数据或日志文件添加到 SQL Server 2019 中的数据库中。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- adding data files
- adding files
- adding log files
- file additions [SQL Server], steps
- files [SQL Server], adding
- data additions [SQL Server]
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: stevestein
ms.author: sstein
ms.openlocfilehash: e0e411e82a6492fda48cc8d3541dbd272075f994
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813134"
---
# <a name="add-data-or-log-files-to-a-database"></a>向数据库中添加数据文件或日志文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中向数据库添加数据文件或日志文件。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **向数据库中添加数据文件或日志文件，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   当 BACKUP 语句正在运行时，不能添加或删除文件。  
  
-   可以为每个数据库指定最多 32,767 个文件和 32,767 个文件组。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>向数据库添加数据文件或日志文件  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开“数据库”，右键单击要从中添加文件的数据库，然后单击“属性”。  
  
3.  在 **“数据库属性”** 对话框中，选择 **“文件”** 页。  
  
4.  若要添加数据或事务日志文件，请单击 **“添加”** 。  
  
5.  在 **“数据库文件”** 网格中，输入文件的逻辑名称。 该文件名在数据库中必须唯一。  
  
6.  选择文件类型：数据或日志。  
  
7.  对于数据文件，从列表中选择文件应属于的文件组，或选择 \<new filegroup> 以创建新的文件组。 事务日志不能放在文件组中。  
  
8.  指定文件的初始大小。 根据数据库中您希望的最大数据量，使数据文件尽可能大。  
  
9. 若要指定文件的增长方式，请在“自动增长”列中单击 (…) 。 从下列选项中进行选择：  
  
    1.  若要允许当前选中的文件根据数据空间量的需求增加而增长，请选中 **“启用自动增长”** 复选框，然后从下列选项中进行选择：  
  
    2.  若要指定文件按固定增量增长，请选择 **“按 MB”** 并指定一个值。  
  
    3.  若要指定文件按当前文件大小的百分比增长，请选择 **“按百分比”** 并指定一个值。  
  
10. 若要指定最大文件大小限制，请从下列选项中进行选择：  
  
    1.  若要指定文件能够增长到的最大大小，请选择“限制文件增长(MB)”并指定一个值。  
  
    2.  若要允许文件根据需要增长，请选择 **“不限制文件增长”** 。  
  
    3.  若要防止文件增长，请清除 **“启用自动增长”** 复选框。 文件大小不会增长到超过“初始大小(MB)”列中指定的值。  
  
    > [!NOTE]  
    >  最大数据库大小由可用磁盘空间量决定，许可限制由正在使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本决定。  
  
11. 指定文件位置的路径。 指定的路径必须存在才能添加文件。  
  
    > [!NOTE]  
    >  默认情况下，数据和事务日志放在相同的驱动器和路径中以适应单磁盘系统，但这对于生产环境可能并非最佳方式。 有关详细信息，请参阅 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)。  
  
12. 单击“确定”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>向数据库添加数据文件或日志文件  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此实例向数据库添加由两个文件组成的文件组。 此示例在 `Test1FG1` 数据库中创建文件组 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] ，然后将两个 5MB 的文件添加到该文件组。  
  
 [!code-sql[DatabaseDDL#AlterDatabase2](../../relational-databases/databases/codesnippet/tsql/add-data-or-log-files-to_1.sql)]  
  
 有关更多示例，请参阅 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [删除数据库中的数据文件或日志文件](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [增加数据库的大小](../../relational-databases/databases/increase-the-size-of-a-database.md)  
  
  
