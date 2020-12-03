---
title: 数据库引擎服务启动选项 | Microsoft Docs
description: 熟悉 SQL Server 数据库引擎启动选项。 查看有关如何使用它们的提示，并了解每个选项的用途。
ms.custom: ''
ms.date: 08/01/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], startup option
- overriding default startup options
- minimal configuration mode [SQL Server], startup option
- default startup options
- temporarily override default startup options [SQL Server]
- startup options [SQL Server]
- starting SQL Server, options
- single-user mode [SQL Server], startup parameter
- overriding default startup parameters
- minimal configuration mode [SQL Server], startup parameter
- default startup parameters
- temporarily override default startup parameters [SQL Server]
- startup parameters [SQL Server]
- starting SQL Server, parameters
ms.assetid: d373298b-f6cf-458a-849d-7083ecb54ef5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 614d630ae8e3ac85baaecb78b4232af7e6e687a0
ms.sourcegitcommit: 644223c40af7168f9d618526e9f4cd24e115d1db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96328047"
---
# <a name="database-engine-service-startup-options"></a>数据库引擎服务启动选项

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

启动选项指定在启动期间所需的某些文件位置，并指定一些服务器范围的条件。 大多数用户不需要指定启动选项，除非您在排除 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 故障或者具有不常见问题，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户支持指示使用启动选项。  
  
> [!WARNING]  
>  错误地使用启动选项可能会影响服务器性能并且可能阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动。  
>
>  使用“mssql”用户启动 Linux 上的 SQL Server 以防止将来的启动问题。 示例： `sudo -u mssql /opt/mssql/bin/sqlservr [STARTUP OPTIONS]` 
  
## <a name="about-startup-options"></a>关于启动选项  
 安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，安装程序会将一组默认的启动选项写入 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 注册表。 可以使用这些启动选项指定备用的 master 数据库文件、master 数据库日志文件或错误日志文件。 如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 找不到所需文件， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不启动。  
  
 可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器设置启动选项。 有关信息，请参阅[配置服务器启动选项 (SQL Server Configuration Manager)](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)。  
  
## <a name="list-of-startup-options"></a>启动选项列表  
### <a name="default-startup-options"></a>默认启动选项  

|选项|说明|  
|-----------------------------|-----------------|  
|**-d**  *master_file_path*|是 master 数据库文件的完全限定路径（通常为：C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\master.mdf）。 如果没有提供此选项，则使用现有的注册表参数。|  
|**-e**  *error_log_path*|是错误日志文件的完全限定路径（通常为 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\LOG\ERRORLOG）。 如果没有提供此选项，则使用现有的注册表参数。|  
|**-l**  *master_log_path*|是 master 数据库日志文件的完全限定路径（通常为：C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\mastlog.ldf）。 如果没有指定此选项，则使用现有的注册表参数。|  
  
### <a name="other-startup-options"></a>其他启动选项   

|选项 |说明|   
|---------------------------|-----------------|  
|**-c**|缩短从命令提示符启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时的启动时间。 通常， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 通过调用服务控制管理器作为服务启动。 由于在通过命令提示符启动时 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 不作为服务启动，因此请使用 **-c** 跳过此步骤。|  
|**-f**|以最小配置启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 在配置值的设置（例如，过度分配内存）妨碍服务器启动时，这非常有用。 在最低配置模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 置于单用户模式。 有关详细信息，请参阅下面的 **-m** 说明。|  
|**-kDecimalNumber**| 此启动参数限制每秒检查点 I/O 请求的数量，其中 DecimalNumber 表示以 MB/S 为单位的检查点速度。  更改此值可能会影响备份速度或执行恢复过程，因此请谨慎操作。 也就是说，如果你为参数指定的值很小，那么你的恢复时间可能较长，且完成备份所耗时间可能会稍微更长一点，原因是备份启动的检查点进程也会被延迟。 你可摒弃此参数，改用以下方法来帮助消除系统上遇到的 I/O 瓶颈：<br /> - 提供适当的硬件来维持由 SQL Server 发布的 I/O 请求 <br /> - 进行足够的应用程序优化 | 
|**-m**|在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，只能连接一个用户，并且不启动 CHECKPOINT 进程。 CHECKPOINT 保证将已完成的事务定期从磁盘缓存写入数据库设备。 （通常，在遇到需要修复的系统数据库问题时使用此选项。）启用 sp_configure allow updates 选项。 默认情况下，allow updates 处于禁用状态。 在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可使计算机本地 Administrators 组的任何成员作为 sysadmin 固定服务器角色的成员连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅 [在系统管理员被锁定时连接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)。有关单用户模式的详细信息，请参阅 [在单用户模式下启动 SQL Server](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)。|  
|**-mClient 应用程序名称**|将连接限制为指定的客户端应用程序。 例如， `-mSQLCMD`  将连接限制为单个连接并且该连接必须将自身标识为 SQLCMD 客户端程序。 当您正在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并且未知的客户端应用程序正在占用这个唯一的可用连接时，使用此选项。 使用 `"Microsoft SQL Server Management Studio - Query"` 与 SSMS 查询编辑器连接。 SSMS 查询编辑器选项不能使用 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 配置管理器进行配置，因为它包括该工具拒绝使用的短划线字符。<br /><br /> 客户端应用程序名称区分大小写。 如果应用程序名称包含空格或特殊字符，则需要使用双引号引起来。<br /><br />**从命令行启动时的示例：**<br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlservr -s MSSQLSERVER -m"Microsoft SQL Server Management Studio - Query"` <br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlservr -s MSSQLSERVER -mSQLCMD` <br /><br /> **安全说明：** 不要将此选项作为安全功能使用。 客户端应用程序提供客户端应用程序名称，并且提供假名称来作为连接字符串的一部分。|  
|**-n**|不要使用 Windows 应用程序日志来记录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。 如果你使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -n **启动** 实例，我们建议你同时使用 **-e** 启动选项。 否则，将不会记录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。|  
|**-s**|用于启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的命名实例。 如果 **-s** 参数未设置，则系统会尝试启动默认实例。 必须先在命令提示符处切换到实例的相应 BINN 目录，然后才能启动 **sqlservr.exe**。 例如，如果 Instance1 为其二进制文件使用了 `\mssql$Instance1`，则用户必须位于 `\mssql$Instance1\binn` 目录中才能启动 sqlservr.exe -s instance1。|  
|**-T**  *trace#*|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动时，指定的跟踪标志 (*trace#* ) 应同时生效。 跟踪标记用于以非标准行为启动服务器。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。<br /><br /> **重要提示：** 使用 -T 选项指定跟踪标记时，请使用大写“T”来传递跟踪标记号。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]也可接受小写“t”，只是它用于设置仅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持工程师才需要的其他内部跟踪标志。 （不读取“控制面板”启动窗口中指定的参数。）|  
|**-x**|禁用下列监视功能：<br /> - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能监视器计数器<br /> - 保留 CPU 时间和高速缓存命中率统计信息<br /> - 收集 DBCC SQLPERF 命令的信息<br /> - 收集某些动态管理视图的信息<br /> - 许多扩展事件事件点<br /><br /> **警告：** 使用 -x 启动选项时，可供用户用于诊断 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的性能和功能问题的信息将显著减少。|  
|**-E**|增加为文件组中的每个文件分配的区数。 对于那些运行索引或数据扫描的用户数量受到限制的数据仓库应用程序，此选项可能十分有用。 不应在其他应用程序中使用此选项，因为它可能降低性能。 32 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]不支持此选项。|  
  
## <a name="using-startup-options-for-troubleshooting"></a>使用启动选项进行故障排除  
 某些启动选项（如单用户模式和最低配置模式）主要在故障排除过程中使用。 使用“-m”或“-f”选项启动服务器进行故障排除的最简单方法是使用命令行，同时还能手动启动 sqlservr.exe 。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net start **启动** 时，启动选项使用的是左斜线 (/)，而不是连字符 (-)。  
  
## <a name="using-startup-options-during-normal-operations"></a>在正常操作中使用启动选项  
 最好在每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时使用某些启动选项。 完成这些选项（如用跟踪标志启动）的最简单方法是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器来配置启动参数。 这些工具将启动选项保存为注册表项，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将始终用这些启动选项来启动。  
  
## <a name="compatibility-support"></a>兼容性支持  

有关已从以前版本中删除的选项，请参阅 [sqlservr 应用程序](../../tools/sqlservr-application.md#compatibility-support)。

## <a name="related-tasks"></a>Related Tasks  
[配置 scan for startup procs 服务器配置选项](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)  
[启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)
[配置服务器启动选项（SQL Server 配置管理器）](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
  
## <a name="see-also"></a>另请参阅  
 [检查点 (Transact-SQL)](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sqlservr Application](../../tools/sqlservr-application.md)  
  
  
