---
description: 系统数据库
title: 系统数据库 | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- system databases [SQL Server]
- displaying system database data
- modifying system data
- viewing system database data
ms.assetid: 30468a7c-4225-4d35-aa4a-ffa7da4f1282
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3476e6d03afae3083e80a7fc607bff77922921b3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478328"
---
# <a name="system-databases"></a>系统数据库

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含以下系统数据库。  
  
|系统数据库|说明|  
|---------------------|-----------------|  
|[master 数据库](../../relational-databases/databases/master-database.md)|记录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有系统级信息。|  
|[msdb 数据库](../../relational-databases/databases/msdb-database.md)|用于 SQL Server 代理计划警报和作业。|  
|[model 数据库](../../relational-databases/databases/model-database.md)|用作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上创建的所有数据库的模板。 对 **model** 数据库进行的修改（如数据库大小、排序规则、恢复模式和其他数据库选项）将应用于以后创建的所有数据库。|  
|[Resource 数据库](../../relational-databases/databases/resource-database.md)|一个只读数据库，包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]包括的系统对象。 系统对象在物理上保留在 **Resource** 数据库中，但在逻辑上显示在每个数据库的 **sys** 架构中。|  
|[tempdb 数据库](../../relational-databases/databases/tempdb-database.md)|一个工作空间，用于保存临时对象或中间结果集。|  

> [!IMPORTANT]
> 对于 Azure SQL 数据库单一数据库和弹性池，仅 master 数据库和 tempdb 数据库适用。 有关详细信息，请参阅[什么是 Azure SQL 数据库服务器](/azure/sql-database/sql-database-servers#what-is-an-azure-sql-database-server)。 有关 Azure SQL 数据库上下文中关于 tempdb 的讨论，请参阅 [Azure SQL 数据库中的 tempdb 数据库](tempdb-database.md#tempdb-database-in-sql-database)。 对于 Azure SQL 托管实例，所有系统数据库都适用。 若要详细了解 Azure SQL 数据库托管实例，请参阅[什么是托管实例](/azure/sql-database/sql-database-managed-instance)
  
## <a name="modifying-system-data"></a>修改系统数据  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持用户直接更新系统对象（如系统表、系统存储过程和目录视图）中的信息。 实际上， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了一整套管理工具，用户可以使用这些工具充分管理他们的系统以及数据库中的所有用户和对象。 其中包括：  
  
-   管理实用工具，如 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
-   SQL-SMO API。 此工具使程序员获得在其应用程序中管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的全部功能。  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本和存储过程。 它们可以使用系统存储过程和 [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL 语句。  
  
 这些工具保护应用程序不受系统对象更改的影响。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有时需要更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 新版本中的系统表，以支持添加到该版本中的新功能。 但应用程序在发出直接引用系统表的 SELECT 语句时，通常依赖于旧的系统表格式。 站点可能在重写从系统表中进行选择的应用程序之后，才能升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新版本。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 考虑了系统存储过程、DDL 和 SQL-SMO 发布的接口，力求维持这些接口的向后兼容性。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持对系统表定义触发器，因为触发器可能会更改系统的操作。  
  
> [!NOTE]  
>  系统数据库不能位于 UNC 共享目录中。  
  
## <a name="viewing-system-database-data"></a>查看系统数据库数据  
 不要编码直接查询系统表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，除非这是获得应用程序所需信息的唯一方法。 相反，应用程序应该通过使用以下方法获得目录和系统信息：  
  
-   系统目录视图  
  
-   SQL-SMO  
  
-   Windows Management Instrumentation (WMI) 接口  
  
-   应用程序中使用的数据 API（如 ADO、OLE DB 或 ODBC）的目录函数、方法、特性或属性。  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 系统存储过程和内置函数。  
  
## <a name="related-tasks"></a>Related Tasks  
 [备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)  
  
 [在对象资源管理器中隐藏系统对象](../../ssms/object/hide-system-objects-in-object-explorer.md)  
  
## <a name="related-content"></a>相关内容  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
 [数据库](../../relational-databases/databases/databases.md)  
  
