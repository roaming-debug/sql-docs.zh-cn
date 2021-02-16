---
title: 重命名计算机托管实例
description: 重命名托管 SQL Server 实例的计算机时，更新 sys.servers 中存储的系统元数据。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- remote login errors [SQL Server]
- standalone computer names [SQL Server]
- names [SQL Server], standalone instances of SQL Server
- renaming standalone instances of SQL Server
- sysservers system table
- removing remote logins
- deleting remote logins
- dropping remote logins
ms.assetid: bbaf1445-b8a2-4ebf-babe-17d8cf20b037
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b84f700ab19b940d8aa742be94dc2c3ab3726507
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353042"
---
# <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server"></a>重命名承载 SQL Server 独立实例的计算机

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

如果更改了运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的计算机的名称，则会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动期间识别新名称。 不必再次运行安装程序以重置计算机名称。 只需使用以下步骤对存储在 sys.servers 中并由系统函数 @@SERVERNAME 报告的系统元数据进行更新。 更新系统元数据，以反映出使用 @@SERVERNAME 或从 sys.servers 中查询服务器名称的远程连接或应用程序的计算机名称的变化。  
  
不能通过以下步骤重命名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 这些步骤只能用于重命名实例名中与计算机名称对应的部分。 例如，可以将承载名为 Instance1 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机称（名为 MB1）更改为其他名称，例如 MB2。 但是，名称中的实例部分 Instance1 将保持不变。 在此示例中， \\\\*ComputerName*\\*InstanceName* 将从 \\\MB1\Instance1 更改为 \\\MB2\Instance1。  
  
 **开始之前**  
  
 开始重命名过程之前，请查看下列信息：  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集的一部分，则计算机的重命名过程不同于承载独立实例的计算机的重命名过程。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持对复制所涉及的计算机进行重命名。 如果主计算机永久丢失，则可以重命名日志传送中的辅助计算机。 有关详细信息，请参阅[日志传送和复制 (SQL Server)](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)。  
  
-   如果重命名配置为使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的计算机，则 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]在计算机名称更改后可能不可用。 有关详细信息，请参阅 [重命名报表服务器计算机](../../reporting-services/report-server/rename-a-report-server-computer.md)。  
  
-   重命名配置为使用数据库镜像的计算机时，必须先关闭数据库镜像，然后才能执行重命名操作。 然后，使用新的计算机名称重新建立数据库镜像。 数据库镜像的元数据将不会自动更新来反映新计算机名称。 使用以下步骤更新系统元数据。  
  
-   如果用户通过以硬编码方式引用计算机名称的 Windows 组连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则用户可能无法再连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 重命名之后，如果 Windows 组指定的是旧计算机名称，则可能会发生这种情况。 若要确保这样的 Windows 组在重命名操作之后可以连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请更新 Windows 组以指定新计算机名称。  
  
 重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，可使用新计算机名称连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 为确保 @@SERVERNAME 返回本地服务器实例更新后的名称，应根据自己的方案手动运行以下过程。 采用的过程取决于所更新的计算机承载的是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的默认实例还是命名实例。  
  
## <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-ssnoversion"></a>重命名托管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例的计算机  
  
-   对于承载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]默认实例的重命名计算机，请运行以下过程：  
  
    ```sql
    EXEC sp_dropserver '<old_name>';  
    GO  
    EXEC sp_addserver '<new_name>', local;  
    GO  
    ```  
  
     重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
-   对于承载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]命名实例的重命名计算机，请运行以下过程：  
  
    ```sql
    EXEC sp_dropserver '<old_name\instancename>';  
    GO  
    EXEC sp_addserver '<new_name\instancename>', local;  
    GO  
    ```  
  
     重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
## <a name="after-the-renaming-operation"></a>重命名操作之后  
 重命名计算机后，所有使用旧计算机名称的连接都必须通过使用新名称进行连接。  
  
## <a name="verify-renaming-operation"></a>验证重命名操作  
  
-   通过 @@SERVERNAME 或 sys.servers 选择信息。 @@SERVERNAME 函数将返回新名称，sys.servers 表将显示该新名称。 以下示例演示了如何使用 @@SERVERNAME。  
  
    ```  
    SELECT @@SERVERNAME AS 'Server Name';  
    ```  
  
## <a name="additional-considerations"></a>其他注意事项  
 **远程登录名** - 如果计算机具有任何远程登录名，运行 **sp_dropserver** 可能会产生类似如下的错误：  
  
 `Server: Msg 15190, Level 16, State 1, Procedure sp_dropserver, Line 44 There are still remote logins for the server 'SERVER1'.`  
  
 若要解决此错误，必须删除此服务器的远程登录。  
  
### <a name="drop-remote-logins"></a>删除远程登录  
  
-   对于默认实例，请运行以下过程：  
  
    ```sql
    EXEC sp_dropremotelogin old_name;  
    GO  
    ```  
  
-   对于命名实例，请运行以下过程：  
  
    ```sql
    EXEC sp_dropremotelogin old_name\instancename;  
    GO  
    ```  
  
 **链接服务器配置** - 链接服务器配置将受到计算机重命名操作的影响。 使用 **sp_addlinkedserver** 或 **sp_setnetname** 更新计算机名称的引用。 有关详细信息，请参阅 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 或 [sp_setnetname (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)。  
  
 **客户端别名** - 采用命名管道的客户端别名将受到计算机重命名操作的影响。 例如，如果创建了指向 SRVR1 的别名“PROD_SRVR”，而且该别名采用 Named Pipes 协议，则相应的管道名称将类似于 `\\SRVR1\pipe\sql\query`。 计算机重命名后，named pipe 的路径将不再有效。 有关 Named Pipes 的详细信息，请参阅 [使用 Named Pipes 创建有效的连接字符串](/previous-versions/sql/sql-server-2008/ms189307(v=sql.100))。  
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  
