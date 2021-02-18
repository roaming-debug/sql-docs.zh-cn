---
title: sqlservr 应用程序
description: sqlservr 应用程序可以在命令提示符下启动、停止、暂停和继续 SQL Server 的实例。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], sqlservr
- command prompt [SQL Server], pausing/resuming instance of SQL Server
- starting instance of SQL Server
- command prompt [SQL Server], continuing instance of SQL Server
- sqlservr utility
- pausing instance of SQL Server
- stopping instance of SQL Server
- resuming SQL Server
- command prompt [SQL Server], stopping instance of SQL Server
- command prompt [SQL Server], starting instance of SQL Server
- continuing instance of SQL Server
ms.assetid: 60e8ef0a-0851-41cf-a6d8-cca1e04cbcdb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 793943c492766037adbce8c077b9a391c041c88c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338586"
---
# <a name="sqlservr-application"></a>sqlservr 应用程序

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

sqlservr 应用程序可以在命令提示符下启动、停止、暂停和继续 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例。

## <a name="syntax"></a>语法

```cmd
sqlservr [-s instance_name] [-c] [-d master_path] [-f] 
     [-e error_log_path] [-l master_log_path] [-m]
     [-n] [-T trace#] [-v] [-x]
```

## <a name="arguments"></a>参数

-s instance_name 指定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 要连接到的实例。 如果未指定命名实例， **sqlservr** 将启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的默认实例。

> [!IMPORTANT]
>启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例时，必须在该实例的相应目录中使用 **sqlservr** 应用程序。 对于默认实例，从 \MSSQL\Binn 目录运行 **sqlservr** 。 对于命名实例，从 \MSSQL$ **instance_name** \Binn 目录运行 *sqlservr*。

 -c 指示独立于 Windows 服务控制管理器启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。 从命令提示符下启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 时可使用此选项，以缩短 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的启动时间。

> [!NOTE]
>使用此选项时，将无法通过使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务管理器或 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] net stop **命令停止** 。如果注销计算机，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将停止。）

-d master_path 指示 master 数据库文件的完全限定的路径。 **-d** 与 *master_path* 之间没有空格。 如果没有提供此选项，则使用现有的注册表参数。

-f 以最小配置启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例。 在配置值的设置（例如，过度分配内存）妨碍服务器启动时，这非常有用。

-e error_log_path 指示错误日志文件的完全限定的路径。 如果未指定，则默认位置为 `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL\Log\Errorlog`（对于默认实例）和 `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL$*instance_name*\Log\Errorlog`（对于命名实例）。 在 **-e** 和 *error_log_path* 之间没有空格。

-l master_log_path 指示 master 数据库事务日志文件的完全限定的路径。 **-l** 与 *master_log_path* 之间没有空格。

-m 指示以单用户模式启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例。 如果以单用户模式启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，则只有一个用户可以连接。 确保将已完成事务定期从磁盘缓存写入数据库设备的 CHECKPOINT 机制将不启动。 通常情况下，在遇到需要修复系统数据库这样的问题时才使用该选项。启用 **sp_configure allow updates** 选项。 默认情况下， **allow updates** 被禁用。

-n 用于启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的命名实例。 如果不设置 **-s** 参数，则尝试启动默认实例。 必须先在命令提示符处切换到实例的相应 BINN 目录，然后才能启动 **sqlservr.exe**。 例如，如果 Instance1 为其二进制文件使用了 \mssql$Instance1，则用户必须位于 \mssql$Instance1\binn 目录中才能启动 **sqlservr.exe -s instance1**。 如果用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -n **选项启动** 实例，则最好也使用 **-e** 选项，否者将不会记录 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 事件。

-T trace# 指示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例启动时，指定的跟踪标志 (trace#) 应同时生效。 跟踪标记用于以非标准行为启动服务器。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。

>[!IMPORTANT]
>指定跟踪标志时，请使用 **-T** 来传递跟踪标志号。 **接受小写的 t (** -t [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)])；但是 **-t** 通常用于设置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持工程师所需的其他内部跟踪标志。

-v 显示服务器的版本号。

-x 不保留 CPU 时间和高速缓存命中率统计信息。 可获得最大性能。

## <a name="remarks"></a>备注
多数情况下，sqlservr.exe 程序只用于故障排除或主要维护。 在命令提示符下使用 sqlservr.exe 启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不作为服务启动，因此无法使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] net **命令停止** 。 用户可以连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，但 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工具将显示服务的状态，以便 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器正确指示服务已停止。 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 可以与服务器连接，但它也可以指示服务已停止。

## <a name="compatibility-support"></a>兼容性支持
以下参数在 [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] 中已过时，不再受支持。

|参数 | 详细信息|
|:-----|:-----|
|**-h** | 启用 AWE 时，在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 早期版本的 32 位实例中为热添加内存元数据保留虚拟内存地址空间。 通过 [!INCLUDE[sssql14](../includes/sssql14-md.md)] 支持。 有关详细信息，请参阅 [SQL Server 2016 中不再使用的 SQL Server 功能](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)。|
|**-g** | *memory_to_reserve*<br/><br>适用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 32 位实例的早期版本。 通过 [!INCLUDE[sssql14](../includes/sssql14-md.md)] 支持。 指定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 为位于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程中但在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存池之外的内存分配保留的内存整数量 (MB)。 有关详细信息，请参阅 [SQL Server 2014“服务器内存配置选项”文档](/previous-versions/sql/2014/database-engine/configure-windows/server-memory-server-configuration-options?view=sql-server-2014&preserve-view=true)。|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>另请参阅
 [数据库引擎服务启动选项](../database-engine/configure-windows/database-engine-service-startup-options.md)
