---
description: msdb 数据库
title: msdb 数据库 | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, msdb database
- alerts [SQL Server], msdb database
- jobs [SQL Server], msdb database
- msdb database [SQL Server]
ms.assetid: 5032cb2d-65a0-40dd-b569-4dcecdd58ceb
author: stevestein
ms.author: sstein
ms.openlocfilehash: 80ada8fcce7fe1480dca5e22d4a5b8be3e73b617
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421979"
---
# <a name="msdb-database"></a>msdb 数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **代理使用** msdb [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库来计划警报和作业， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 和数据库邮件等其他功能也使用该数据库。  
  
 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 **msdb** 中的表中自动保留一份完整的联机备份和还原历史记录。 这些信息包括执行备份一方的名称、备份时间和用来存储备份的设备或文件。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 使用这些信息来提出计划，还原数据库和应用任何事务日志备份。 将会记录有关所有数据库的备份事件，即使它们是由自定义应用程序或第三方工具创建的。 例如，如果使用调用 SQL Server 管理对象 (SMO) 对象的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 应用程序执行备份操作，则事件将记录在 msdb 系统表、[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 应用程序日志和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中。 为了帮助您保护存储在 **msdb** 中的信息，我们建议您考虑将 **msdb** 事务日志放在容错存储区中。  
  
 默认情况下， **msdb** 使用简单恢复模式。 如果使用 [备份和还原历史记录](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md) 表，我们建您对 **msdb** 使用完整恢复模式。 有关详细信息，请参阅[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)。 请注意，当安装或升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，只要使用 Setup.exe 重新生成系统数据库， **msdb** 的恢复模式便会自动设置为简单。  
  
> [!IMPORTANT]  
>  在进行任何更新 **msdb** 的操作后，例如备份或还原任何数据库后，我们建议您备份 **msdb**。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。  
  
## <a name="physical-properties-of-msdb"></a>msdb 的物理属性  
 下表列出了 **msdb** 数据和日志文件的初始配置值。 对于不同版本的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]，这些文件的大小可能略有不同。  
  
|文件|逻辑名称|物理名称|文件增长|  
|----------|------------------|-------------------|-----------------|  
|主数据|MSDBData|MSDBData.mdf|以 10% 的速度自动增长到磁盘充满为止。|  
|日志|MSDBLog|MSDBLog.ldf|以 10% 的速度自动增长到最大 2 TB。|  
  
 若要移动 **msdb** 数据库或日志文件，请参阅 [移动系统数据库](../../relational-databases/databases/move-system-databases.md)。  
  
### <a name="database-options"></a>数据库选项  
 下表列出了 **msdb** 数据库中每个数据库选项的默认值以及该选项是否可以修改。 若要查看这些选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。  
  
|数据库选项|默认值|是否可修改|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|ON|否|  
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
|DB_CHAINING|ON|是|  
|ENCRYPTION|OFF|否|  
|MIXED_PAGE_ALLOCATION|ON|否|  
|NUMERIC_ROUNDABORT|OFF|是|  
|PAGE_VERIFY|CHECKSUM|是|  
|PARAMETERIZATION|SIMPLE|是|  
|QUOTED_IDENTIFIER|OFF|是|  
|READ_COMMITTED_SNAPSHOT|OFF|否|  
|RECOVERY|SIMPLE|是|  
|RECURSIVE_TRIGGERS|OFF|是|  
|Service Broker 选项|ENABLE_BROKER|是|  
|TRUSTWORTHY|ON|是|  
  
 有关这些数据库选项的说明，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
## <a name="restrictions"></a>限制  
 不能在 **msdb** 数据库中执行下列操作：  
  
-   更改排序规则。 默认排序规则为服务器排序规则。  
-   删除数据库。  
-   从数据库中删除 **guest** 用户。  
-   启用变更数据捕获。  
-   参与数据库镜像。  
-   删除主文件组、主数据文件或日志文件。  
-   重命名数据库或主文件组。  
-   将数据库设置为 OFFLINE。  
-   将主文件组设置为 READ_ONLY。  
  
## <a name="related-content"></a>相关内容  
- [系统数据库](../../relational-databases/databases/system-databases.md)  
- [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
- [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
- [移动数据库文件](../../relational-databases/databases/move-database-files.md)  
- [数据库邮件](../../relational-databases/database-mail/database-mail.md)  
- [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
