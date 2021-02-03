---
description: MSSQLSERVER_17207
title: MSSQLSERVER_17207
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: fb3d8d885345c30e432f25f005969664ab514fa6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196711"
---
# <a name="mssqlserver_17207"></a>MSSQLSERVER_17207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |
| :-------- | :---- |
|产品名称|SQL Server|  
|事件 ID|17207|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBLKIO_OS2DISKERROR|  
|消息正文|%ls:创建或打开文件 '%ls' 时出现操作系统错误 %ls。 请诊断并更正该操作系统错误，然后重试操作。|  


## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 由于指定的 OS 错误而无法打开指定的文件。  

当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法打开数据库和/或事务日志文件时，Windows 应用程序事件或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中可能会出现错误 17207。 此错误如以下示例所示。

``` 
Error: 17207, Severity: 16, State: 1.
FileMgr::StartSecondaryDataFiles: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'F:\MSSQL\DATA\MyDB_FG1_1.ndf'. Diagnose and correct the operating system error, and retry the operation.
```

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动过程中，或在尝试启动数据库的任何数据库操作（如 ALTER DATABASE）期间，都可能看到这些错误。 在某些情况下，你可能会看到 17207 和 17204 错误，而在其他情况下，你可能只会看到其中一种错误。

如果用户数据库遇到这些错误，该数据库将处于 RECOVERY_PENDING 状态，并且应用程序无法访问数据库。 如果系统数据库遇到这些错误，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将不会启动，并且你无法连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的此实例。 这也可能导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集资源脱机。

如果问题与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件流文件组相关，你将注意到仅列出了完整目录路径，而不是文件名。 以下是一个示例。 
```
Error: 17207, Severity: 16, State: 1.
STREAMFCB::Startup: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\bpa_files_test_fs_1\bpa_files_test_fs_1'. Diagnose and correct the operating system error, and retry the operation.
```

## <a name="cause"></a>原因
在使用任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库之前，必须启动数据库。 数据库启动过程涉及初始化各种数据结构（这些结构表示数据库和数据库文件），打开属于数据库的所有文件，最后在数据库上运行恢复。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) Windows API 函数打开属于数据库的文件。
 
消息 17207（和 17204）指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试在启动过程中打开数据库文件时遇到错误。
 
这些错误消息包含下列信息：
1. 尝试打开文件的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 函数的名称。 通常在这些错误消息中看到如下函数名称：
   - FCB::Open                              - 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试打开文件时遇到错误
   - FileMgr::StartPrimaryDataFiles         - 主数据文件或属于主文件组的文件
   - FileMgr::StartSecondaryDataFiles       - 属于辅助文件组的文件
   - FileMgr::StartLogFiles                 - 事务日志文件
   - STREAMFCB::Startup                     - SQL FileStream 容器
   - FCB::RemoveAlternateStreams
  
      
1. 状态信息，用于区分可生成此错误消息的函数中的多个位置。
1. 文件的完整物理路径。
1. 对应于文件的文件 ID。
1. 操作系统错误代码和错误说明。 在某些情况下，只会看到错误代码。
 
在这些错误消息中输出的操作系统错误信息是导致错误 17204 的根本原因。 这些错误消息的常见原因是权限问题或文件路径不正确。


## <a name="user-action"></a>用户操作  
1. 要解决错误 17207，需要了解关联的操作系统错误代码并诊断该错误。 解决了操作系统错误后，可以尝试重启数据库（例如，使用 ALTER DATABASE SET ONLINE）或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，使受影响的数据库处于联机状态。 在某些情况下，可能无法解决操作系统错误，因此必须采取特定的纠正措施。 本节将介绍这些操作。
1. 如果 17207 错误消息只包含错误代码，而不包含错误说明，则可以尝试在操作系统 shell 中运行以下命令来解析错误代码：net helpmsg <error code>。 如果收到的错误代码是一个 8 位数状态代码，可以参考[如何将 HRESULT 转换为 Win32 错误代码？](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133)之类的信息源解码这些状态代码，了解它们在操作系统中都代表什么错误。
1. 如果收到 ```Access is Denied``` 操作系统错误 = 5，请考虑使用以下方法：
   -  通过在 Windows 资源管理器中查看文件的属性，检查文件的权限设置。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 Windows 组对各种文件资源预配访问控制。 确保相应的组（名称类似于 SQLServerMSSQLUser$ComputerName$MSSQLSERVER 或 SQLServerMSSQLUser$ComputerName$InstanceName）具有对错误消息中提及的数据库文件的必要权限。 有关更多详细信息，请参阅[配置数据库引擎访问的文件系统权限](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true)。 确保 Windows 组实际包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动帐户或服务 SID。
   -  查看当前正在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的用户帐户。 可以使用 Windows 任务管理器来获取此信息。 查找可执行文件“sqlservr.exe”的“用户名”值。 另外，如果你最近更改了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户，请注意，支持执行此操作的方法是使用 SQL Server 配置管理器实用工具。 有关详细信息，请参阅 [SQL Server 配置管理器](../sql-server-configuration-manager.md)。 
   -  根据操作类型（在服务器启动期间打开数据库、附加数据库、还原数据库等），用于模拟和访问数据库文件的帐户可能会有所不同。 请查看[保护数据和日志文件](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105))主题，了解哪些操作为哪些帐户设置了哪些权限。 使用 Windows SysInternals [进程监视器](/sysinternals/downloads/procmon)之类的工具，了解是否在 SQL Server 实例服务启动帐户（或服务 SID）或模拟帐户的安全上下文中执行了文件访问。

      如果 SQL Server 正在模拟可执行 ALTER DATABASE 或 CREATE DATABASE 操作的登录用户凭据，你将在进程监视器工具（示例）中注意到以下信息。

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
  
1. 如果遇到 `The system cannot find the file specified` OS 错误 = 3：
   - 查看错误消息中的完整路径。
   - 确保磁盘驱动器和文件夹路径可见并可从 Windows 资源管理器访问。
   - 查看 Windows 事件日志，以确定此磁盘驱动器是否存在任何问题。
   - 如果路径不正确并且该数据库已经存在于系统中，则可以使用[移动数据库文件](../databases/move-database-files.md)文章中所述的方法更改数据库文件路径。 你可能必须使用此过程，尤其是对于遇到 17204 或 17207 的系统数据库文件，并且你正在使用指定磁盘驱动器不可用的灾难恢复方案。 本主题还介绍了如何识别各种系统数据库[master、model、tempdb、msdb 和 mssqlsystemresource]的当前位置。
   - 如果由于缺少数据库文件而出现此错误，则必须从有效备份还原数据库：
     - 如果与错误关联的数据库文件属于辅助文件组，则可以选择将该文件组标记为脱机，使数据库联机，然后单独执行该文件组的还原。 有关详细信息，请参阅主题 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) 的脱机部分。
     - 如果产生错误的文件是一个事务日志文件，请查看主题 [CREATE DATABASE (Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md) 中“FOR ATTACH”和“FOR ATTACH_REBUILD_LOG”部分下的信息，以了解如何重新创建丢失的事务日志文件。
   - 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试访问这些位置上的数据库文件之前，请确保任何磁盘或网络位置[如 iSCSI 驱动器]可用。 如果需要，请在群集管理员或服务控制管理器中创建所需的依赖项。

1. 如果遇到 `The process cannot access the file because it is being used by another process` 操作系统错误 = 32：
   - 使用 Windows Sysinternals 中的[进程资源管理器](/sysinternals/downloads/process-explorer)或[句柄](/sysinternals/downloads/handle)之类的工具来确定其他进程或服务是否已获取此数据库文件的排他锁。
   - 阻止该进程访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库文件。 常见示例包括防病毒程序（请参阅以下[知识库文章](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server)中的文件排除指南）。
   - 在群集环境中，确保前一个所属节点中的 sqlservr.exe 进程实际上已经将句柄释放到数据库文件。 通常不会发生这种情况，但群集或 I/O 路径的错误配置可能会导致此类问题。
