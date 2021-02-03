---
description: MSSQLSERVER_1418
title: MSSQLSERVER_1418 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1418 (Database Engine error)
ms.assetid: 6e9c7241-0201-44e0-9f8b-b3c4e293f0f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cb8cff51e4b33f2d5aeb99c030073544e195b11d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197010"
---
# <a name="mssqlserver_1418"></a>MSSQLSERVER_1418
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|1418|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBM_PARTNERNOTFOUND|  
|消息正文|服务器网络地址 "%.*ls" 无法访问或不存在。 请检查网络地址名称，并检查本地和远程端点的端口是否正常运行。|  
  
## <a name="explanation"></a>说明  
该服务器网络端点未做出响应，这是因为无法到达指定的服务器网络地址或该地址不存在。  
  
> [!NOTE]  
> 默认情况下，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 操作系统会阻止所有端口。  
  
## <a name="user-action"></a>用户操作  
验证网络地址名称并重新发出命令。  
  
伙伴双方可能都需要执行更正操作。 例如，如果在主体服务器实例上尝试运行 SET PARTNER 时引发此消息，则此消息可能表示您只需要在镜像服务器实例上执行更正操作。 但是，伙伴双方可能都需要执行更正操作。  
  
### <a name="additional-corrective-actions"></a>其他更正操作  
  
-   确保镜像数据库已为镜像做好准备。  
  
-   确保镜像服务器实例的名称和端口都正确。  
  
-   确保目标镜像服务器实例不在防火墙之后。  
  
-   确保主体服务器实例不在防火墙之后。  
  
-   使用 **sys.database_mirroring_endpoints** 目录视图中的 **state** 或 **state_desc** 列验证伙伴上的端点是否都已启动。 如果未启动任一端点，则执行 ALTER ENDPOINT 语句将其启动。  
  
-   确保主体服务器实例正在侦听分配给其数据库镜像端点的端口，并且镜像服务器实例正在侦听它自己的端口。 有关详细信息，请参阅本主题后面的“验证端口可用性”部分。 如果某个伙伴没有侦听为其分配的端口，则请修改数据库镜像端点以侦听其他端口。  
  
    > [!IMPORTANT]  
    > 错误配置的安全性可引发常规设置错误消息。 通常，服务器实例只删除错误的连接请求而不做出响应。 对于调用方，安全配置错误可能由于其他许多原因引起，如镜像数据库处于错误状态或不存在、权限不正确等等。  
  
### <a name="using-the-error-log-file-for-diagnosis"></a>使用错误日志文件进行诊断  
在某些情况下，错误日志文件只能用于进行调查。 在这些情况下，请确定错误日志是否包含数据库镜像端点 TCP 端口的错误消息 26023。 此错误严重级别为 16，可能指示数据库镜像端点尚未启动。 即使 **sys.database_mirroring_endpoints** 显示端点状态为已启动，也会发生此错误。  
  
解决遇到的所有问题之后，请在主体服务器上重新运行 ALTER DATABASE *database_name* SET PARTNER 语句。  
  
### <a name="verifying-port-availability"></a>验证端口可用性  
为数据库镜像会话配置网络时，请确保每个服务器实例的数据库镜像端点只由数据库镜像过程使用。 如果另一个过程正在侦听分配给数据库镜像端点的端口，则其他服务器实例的数据库镜像过程将无法连接到该端点。  
  
若要显示基于 Windows 的服务器正在侦听的所有端口，请使用 **netstat** 命令提示实用工具。 **netstat** 语法取决于 Windows 操作系统的版本。 有关详细信息，请参阅操作系统文档。  
  
#### <a name="windows-server-2003-service-pack-1-sp1"></a>Windows Server 2003 Service Pack 1 (SP1)  
若要列出侦听端口以及打开这些端口的进程，请在 Windows 命令提示符下输入以下命令：  
  
**netstat -abn**  
  
#### <a name="windows-server-2003-pre-sp1"></a>Windows Server 2003（SP1 以前的版本）  
若要标识侦听端口以及打开这些端口的进程，请按照以下步骤进行：  
  
1.  获取进程 ID  
  
    若要了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的进程 ID，请连接到该实例并使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT SERVERPROPERTY('ProcessID')   
    ```  
  
    有关详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“SERVERPROPERTY (Transact-SQL)”。  
  
2.  然后，将进程 ID 与以下 **netstat** 命令的输出进行匹配：  
  
    **netstat -ano**  
  
## <a name="see-also"></a>另请参阅  
[ALTER ENDPOINT (Transact-SQL)](~/t-sql/statements/alter-endpoint-transact-sql.md)  
[数据库镜像终结点 (SQL Server)](~/database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
[为镜像准备镜像数据库 (SQL Server)](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
[SERVERPROPERTY (Transact-SQL)](~/t-sql/functions/serverproperty-transact-sql.md)  
[指定服务器网络地址（数据库镜像）](~/database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
[sys.database_mirroring_endpoints (Transact-SQL)](~/relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
[数据库镜像配置故障排除 (SQL Server)](~/database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
