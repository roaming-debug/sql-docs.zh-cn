---
description: model 数据库
title: model 数据库 | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- template databases [SQL Server]
- model database [SQL Server], about model databases
- model database [SQL Server]
ms.assetid: 4e4f739b-fd27-4dce-8be6-3d808040d8d7
author: stevestein
ms.author: sstein
ms.openlocfilehash: 01d5e44a93776560b5e89c0dc03a36b780f7ab8d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637692"
---
# <a name="model-database"></a>model 数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   model  数据库用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上创建所有数据库的模板。 因为每次启动 **时都会创建** tempdb [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，所以 **model** 数据库必须始终存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统中。 **model** 数据库的全部内容（包括数据库选项）都会被复制到新的数据库。 启动期间，也可使用 **model** 数据库的某些设置创建新的 **tempdb** ，因此 **model** 数据库必须始终存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统中。  
  
 新创建的用户数据库与 model 数据库使用相同的 [恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md) 。 默认值是用户可配置的。 若要了解模式的当前恢复模式，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
> [!IMPORTANT]  
>  如果用特定于用户的模板信息修改 **model** 数据库，我们建议您备份 **model**。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。  
  
## <a name="model-usage"></a>model 的用法  
 当发出 CREATE DATABASE 语句时，将通过复制 **model** 数据库中的内容来创建数据库的第一部分， 然后用空页填充新数据库的剩余部分。  
  
 如果修改 **model** 数据库，之后创建的所有数据库都将继承这些修改。 例如，可以设置权限或数据库选项或者添加对象，例如，表、函数或存储过程。 **model** 数据库的文件属性是一个例外且会被忽略（数据文件的初始大小除外）。 模型数据库数据和日志文件的默认初始大小为 8 MB。  
  
## <a name="physical-properties-of-model"></a>model 的物理属性  
 下表列出了 **model** 数据和日志文件的初始配置值。  
  
|文件|逻辑名称|物理名称|文件增长|  
|----------|------------------|-------------------|-----------------|  
|主数据|modeldev|model.mdf|以 64 MB 的速度自动增长到磁盘充满为止。|  
|日志|modellog|modellog.ldf|以 64 MB 的速度自动增长到最大 2 TB。|  

对于 SQL Server 2014，请参阅[模型数据库](/previous-versions/sql/2014/relational-databases/databases/model-database?view=sql-server-2014&preserve-view=true)，了解默认文件增加值。  

 若要移动 **model** 数据库或日志文件，请参阅 [移动系统数据库](../../relational-databases/databases/move-system-databases.md)。  
  
### <a name="database-options"></a>数据库选项  
 下表列出了 **model** 数据库中每个数据库选项的默认值以及该选项是否可以修改。 若要查看这些选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。  
  
|数据库选项|默认值|是否可修改|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|是|  
|ANSI_NULL_DEFAULT|OFF|是|  
|ANSI_NULLS|OFF|是|  
|ANSI_PADDING|OFF|是|  
|ANSI_WARNINGS|OFF|是|  
|ARITHABORT|OFF|是|  
|AUTO_CLOSE|OFF|是|  
|AUTO_CREATE_STATISTICS|ON|是|  
|AUTO_SHRINK|OFF|是|  
|AUTO_UPDATE_STATISTICS|ON|是|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|是|  
|CHANGE_TRACKING|OFF|否|  
|CONCAT_NULL_YIELDS_NULL|OFF|是|  
|CURSOR_CLOSE_ON_COMMIT|OFF|是|  
|CURSOR_DEFAULT|GLOBAL|是|  
|数据库可用性选项|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|否<br /><br /> 是<br /><br /> 是|  
|DATE_CORRELATION_OPTIMIZATION|OFF|是|  
|DB_CHAINING|OFF|否|  
|ENCRYPTION|OFF|否|  
|MIXED_PAGE_ALLOCATION|ON|否|  
|NUMERIC_ROUNDABORT|OFF|是|  
|PAGE_VERIFY|CHECKSUM|是|  
|PARAMETERIZATION|SIMPLE|是|  
|QUOTED_IDENTIFIER|OFF|是|  
|READ_COMMITTED_SNAPSHOT|OFF|是|  
|RECOVERY|取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本*|是|  
|RECURSIVE_TRIGGERS|OFF|是|  
|Service Broker 选项|DISABLE_BROKER|否|  
|TRUSTWORTHY|OFF|否|  
  
 * 若要验证数据库的当前恢复模式，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) 或 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)。  
  
 有关这些数据库选项的说明，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
## <a name="restrictions"></a>限制  
 不能在 **model** 数据库中执行下列操作：  
  
-   添加文件或文件组。  
  
-   更改排序规则。 默认排序规则为服务器排序规则。  
  
-   更改数据库所有者。 **模型** 的所有者是 **sa**。  
  
-   删除数据库。  
  
-   从数据库中删除 **guest** 用户。  
  
-   启用变更数据捕获。  
  
-   参与数据库镜像。  
  
-   删除主文件组、主数据文件或日志文件。  
  
-   重命名数据库或主文件组。  
  
-   将数据库设置为 OFFLINE。  
  
-   将主文件组设置为 READ_ONLY。  
  
-   使用 WITH ENCRYPTION 选项创建过程、视图或触发器。 加密密钥与在其中创建对象的数据库绑定在一起。 在 **model** 数据库中创建的加密对象只能用于 **model** 中。  
  
## <a name="related-content"></a>相关内容  
 [系统数据库](../../relational-databases/databases/system-databases.md)  
  
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
 [移动数据库文件](../../relational-databases/databases/move-database-files.md)  
  
  
