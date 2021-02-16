---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 7e9ec54be052303c0b350a0037aa3f2ee9caa0d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348756"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|5120|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DSK_FCB_FAILURE|  
|消息正文|表错误:无法打开物理文件 "%.*ls"。 操作系统错误 %d: "%ls"。|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法打开数据库文件。  消息中提供的操作系统错误指向更具体的根本失败原因。 通常，你会看到此错误与其他错误一起出现，如 [17204](mssqlserver-17204-database-engine-error.md) 或 [17207](mssqlserver-17207-database-engine-error.md)。
  
## <a name="user-action"></a>用户操作  
  
  诊断并更正该操作系统错误，然后重试操作。 有多种状态可帮助 Microsoft 缩小产品中出现错误的范围。 
  
### <a name="access-is-denied"></a>拒绝访问 
如果收到 `Access is Denied` 操作系统错误 = 5，请考虑使用以下方法：
   -  通过在 Windows 资源管理器中查看文件的属性，检查文件的权限设置。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 Windows 组对各种文件资源预配访问控制。 确保相应的组[名称类似于 SQLServerMSSQLUser$ComputerName$MSSQLSERVER 或 SQLServerMSSQLUser$ComputerName$InstanceName]具有对错误消息中提及的数据库文件的必要权限。 有关更多详细信息，请参阅[配置数据库引擎访问的文件系统权限](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true)。 确保 Windows 组实际包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动帐户或服务 SID。
   -  查看当前正在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的用户帐户。 可以使用 Windows 任务管理器来获取此信息。 查找可执行文件“sqlservr.exe”的“用户名”值。 另外，如果你最近更改了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户，请注意，支持执行此操作的方法是使用 [SQL Server 配置管理器](../sql-server-configuration-manager.md)实用工具。 
   -  根据操作类型（在服务器启动期间打开数据库、附加数据库、还原数据库等），用于模拟和访问数据库文件的帐户可能会有所不同。 请查看[保护数据和日志文件](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105))主题，了解哪些操作为哪些帐户设置了哪些权限。 使用 Windows SysInternals [进程监视器](/sysinternals/downloads/procmon)这样的工具，了解是否在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例服务启动帐户[或服务 SID] 或模拟帐户的安全上下文中执行了文件访问。

      如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在模拟可执行 ALTER DATABASE 或 CREATE DATABASE 操作的登录用户凭据，你将在进程监视器工具（示例）中注意到以下信息。
      
        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>附加位于网络连接存储上的文件  
如果无法重新附加位于网络连接存储上的数据库，可能会在应用程序日志中记录类似如下的消息。

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

之所以出现此问题，是因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在分离数据库时会重置文件权限。 当你尝试重新附加数据库时，将出现失败，因为共享权限受限。

若要解决此问题，请执行以下步骤：
1. 使用 -T 启动选项启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 使用此启动选项可在 [SQL Server 配置管理器](../sql-server-configuration-manager.md)中打开跟踪标志 1802（有关 1802 的详细信息，请参阅 [跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)）。 若要详细了解如何更改启动参数，请参阅[数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)。

2. 使用以下命令拆离数据库。
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. 使用以下命令重新附加数据库。
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
