---
title: 数据库 | Microsoft Docs
description: 了解数据库架构、表、文件组、登录名和角色。 了解如何使用 SQL Server Management Studio 工具来处理数据库。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- data warehouse [SQL Server]
- OLTP databases [SQL Server]
- databases [SQL Server], about databases
ms.assetid: 316eea58-81b8-4bf3-a1fc-801946740e94
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d1550e59cfa9579cf2c7bf38cdbc8c688b447b69
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478368"
---
# <a name="databases"></a>数据库
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据库由表的集合组成，这些表用于存储一组特定的结构化数据。 表中包含行（也称为记录或元组）和列（也称为属性）的集合。 表中的每一列都用于存储某种类型的信息，例如，日期、名称、金额和数字。  
  
## <a name="basic-information-about-databases"></a>有关数据库的基本信息  
 一台计算机可以安装一个或多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可以包含一个或多个数据库。  在数据库中，有一个或多个对象所有权组（称为架构）。 在每个架构中，都存在数据库对象，如表、视图和存储过程。 某些对象（如证书和非对称密钥）包含在数据库中，但不包含在架构中。 有关创建表的详细信息，请参阅 [Tables](../../relational-databases/tables/tables.md)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库存储在文件的文件系统中。 可将文件分为若干文件组。 有关文件和文件组的详细信息，请参阅 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)。  
  
 如果某人获得对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的访问权限，则将其标识为一个登录名。 当某些人获取对数据库的访问权限时，他们将被标识为数据库用户。 数据库用户可以基于登录名。 如果启用包含的数据库，则可以创建不基于登录名的数据库用户。 有关用户的详细信息，请参阅 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)。  
  
 可以授予对数据库具有访问权限的用户访问数据库中对象的权限。 尽管可以将权限授予各个用户，但建议创建数据库角色，将数据库用户添加到角色中，然后对角色授予访问权限。 对角色（而不是用户）授予权限更容易保持权限一致，随着用户数目的增长和持续更改也更易于了解。 有关角色权限的详细信息，请参阅 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md) 和[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)。  
  
## <a name="working-with-databases"></a>使用数据库  
 大多数使用数据库的人员都使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 工具。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 工具有一个图形用户界面，用于创建数据库和数据库中的对象。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 还具有一个查询编辑器，用于通过编写 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句与数据库进行交互。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装磁盘进行安装，也可以从 MSDN 中下载。 有关 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 工具的详细信息，请参阅 [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md)。
  
## <a name="in-this-section"></a>本节内容  

:::row:::
    :::column:::
        [系统数据库](../../relational-databases/databases/system-databases.md)  
        [包含的数据库](../../relational-databases/databases/contained-databases.md)  
        [Microsoft Azure 中的 SQL Server 数据文件](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
        [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  
        [数据库状态](../../relational-databases/databases/database-states.md)  
        [文件状态](../../relational-databases/databases/file-states.md)  
        [估计数据库的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
        [将数据库复制到其他服务器](../../relational-databases/databases/copy-databases-to-other-servers.md)  
        [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
        [向数据库中添加数据文件或日志文件](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
        [更改数据库的配置设置](../../relational-databases/databases/change-the-configuration-settings-for-a-database.md)  
        [创建数据库](../../relational-databases/databases/create-a-database.md)  
        [删除数据库](../../relational-databases/databases/delete-a-database.md)  
    :::column-end:::
    :::column:::
        [删除数据库中的数据文件或日志文件](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
        [显示数据库的数据和日志空间信息](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
        [增加数据库的大小](../../relational-databases/databases/increase-the-size-of-a-database.md)  
        [重命名数据库](../../relational-databases/databases/rename-a-database.md)  
        [将数据库设置为单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)  
        [收缩数据库](../../relational-databases/databases/shrink-a-database.md)  
        [收缩文件](../../relational-databases/databases/shrink-a-file.md)  
        [查看或更改数据库的属性](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md)  
        [查看 SQL Server 实例的数据库列表](../../relational-databases/databases/view-a-list-of-databases-on-an-instance-of-sql-server.md)  
        [查看或更改数据库的兼容级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)  
        [使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
        [创建用户定义的数据类型别名](../../relational-databases/databases/create-a-user-defined-data-type-alias.md)  
        [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)  
    :::column-end:::
:::row-end:::

## <a name="related-content"></a>相关内容  
 [索引](../../relational-databases/indexes/indexes.md)  
  
 [视图](../../relational-databases/views/views.md)  
  
 [存储过程（数据库引擎）](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)  
