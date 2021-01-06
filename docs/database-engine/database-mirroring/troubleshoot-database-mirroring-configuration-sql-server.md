---
title: 配置数据库镜像时的常见问题
description: 提供有关信息以帮助解决设置数据库镜像会话时遇到的问题。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2b50f06d3830a8f723d9d6ecc570125ecafb136a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641533"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>数据库镜像配置故障排除 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题提供有关信息以帮助您解决设置数据库镜像会话时遇到的问题。  
  
> [!NOTE]  
>  请确保满足所有 [数据库镜像的先决条件](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md)。  
  
|问题|总结|  
|-----------|-------------|  
|错误消息 1418|此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 消息指示无法到达服务器网络地址或该地址不存在，同时建议您确认网络地址名称并重新发出命令。 |  
|[帐户](#Accounts)|介绍了正确配置运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所用的帐户的相关要求。|  
|[端点](#Endpoints)|介绍了正确配置每个服务器实例的数据库镜像端点的要求。|  
|[SystemAddress](#SystemAddress)|概述了在数据库镜像配置中指定服务器实例的系统名称的备选方法。|  
|[网络访问](#NetworkAccess)|记录了允许每个服务器实例通过 TCP 访问其他一个或多个服务器实例的端口的要求。|  
|[镜像数据库准备](#MirrorDbPrep)|概述了准备镜像数据库以开始镜像的要求。|  
|[失败的创建文件操作](#FailedCreateFileOp)|说明如何响应失败的创建文件操作。|  
|[使用 Transact-SQL 开始镜像](#StartDbm)|说明 ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** 语句所需的顺序。|  
|[跨数据库事务](#CrossDbTxns)|自动故障转移可能导致自动不正确地解决有疑问的事务。 因此，数据库镜像不支持跨数据库事务。|  
  
##  <a name="accounts"></a><a name="Accounts"></a> 帐户  
 必须正确配置运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所用的帐户。  
  
1.  帐户是否具有正确的权限？  
  
    1.  如果这些帐户在相同的域帐户中运行，则会减少配置错误的几率。  
  
    2.  如果这些帐户在不同的域中运行或不属于域帐户，则必须在其他计算机的 **master** 中创建一个登录帐户，并且必须授予该登录帐户端点的 CONNECT 权限。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。 这包括网络服务帐户。  
  
2.  如果使用本地系统帐户将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为服务运行，则必须使用证书进行身份验证。 有关详细信息，请参阅[使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)。  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 必须正确配置端点。  
  
1.  确保每个服务器实例（主体服务器、镜像服务器和见证服务器，如果有的话）都有数据库镜像端点。 有关详细信息，请参阅 [sys.database_mirroring_endpoints (Transact SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)，并根据身份验证的形式，参阅[为 Windows 身份验证创建数据库镜像终结点 (Transact SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)或 [使用数据库镜像终结点证书 (Transact SQ)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)。  
  
2.  检查端口号是否正确。  
  
     若要标识当前与服务器实例的数据库镜像终结点关联的端口，请使用 [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) 和 [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) 目录视图。  
  
3.  对于难以解释的数据库镜像设置问题，建议您检查每个服务器实例以确定它是否正在侦听相应的端口。   
  
4.  确保已启动端点 (STATE = STARTED)。 对于各个服务器实例，使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     有关 **state_desc** 列的详细信息，请参阅 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)。  
  
     若要启动端点，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     有关详细信息，请参阅 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
5.  检查 ROLE 是否正确。 对每个服务器实例使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     有关详细信息，请参阅 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)。  
  
6.  从其他服务器实例的服务帐户登录需要 CONNECT 权限。 确保其他服务器的登录帐户具有 CONNECT 权限。 若要确定哪个登录帐户拥有对端点的 CONNECT 权限，请对每个服务器实例使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-address"></a><a name="SystemAddress"></a> 系统地址  
 对于数据库镜像配置中服务器实例的系统名称，可以使用明确标识系统的任何名称。 服务器地址可以是系统名称（如果各系统都在同一个域中）、完全限定域名或 IP 地址（最好是静态 IP 地址）。 保证使用完全限定域名的有效性。 有关详细信息，请参阅 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 必须允许每个服务器实例都能通过 TCP 访问其他一个或多个服务器实例的端口。 当服务器实例位于相互不信任的不同域（不可信的域）中时，这尤为重要。 这会限制服务器实例之间大部分的通信。  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 无论是首次开始镜像还是在删除镜像后再次开始镜像，都要验证镜像数据库是否可以进行镜像。  
  
 在镜像服务器上创建镜像数据库时，请确保指定相同数据库名称 WITH NORECOVERY 来还原主体数据库备份。 此外，还必须再次使用 WITH NORECOVERY 应用进行该备份之后创建的所有日志备份。  
  
 另外，我们建议，如有可能，镜像数据库的文件路径（包括驱动器号）应该与主体数据库的路径相同。 如果文件路径必须互不相同（例如，如果主体数据库位于“F:”驱动器上，但镜像系统没有“F:”驱动器），则必须在 RESTORE 语句中加入 MOVE 选项。  
  
> [!IMPORTANT]  
>  如果在创建镜像数据库时移动了数据库文件，则可能导致以后不挂起镜像就无法向数据库添加文件。  
  
 如果数据库镜像已经停止，则必须将对主体数据库执行的所有后续日志备份应用到镜像数据库中，然后才可以重新启动镜像。  
  
 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 在不影响镜像会话的情况下添加文件要求该文件路径同时存在于两个服务器上。 因此，如果在创建镜像数据库时移动了数据库文件，则随后在镜像数据库上的添加文件操作可能会失败，并可能会导致镜像挂起。  
  
 修复此问题：  
  
1.  数据库所有者必须删除此镜像会话，并还原包含所添加文件的文件组的完整备份。  
  
2.  然后，所有者必须备份主体服务器上包含添加文件操作的日志，并使用 WITH NORECOVERY 和 WITH MOVE 选项在镜像数据库上手动还原此日志备份。 执行此操作可在镜像服务器上创建指定的文件路径，并将相应的新文件还原到该位置。  
  
3.  若要准备数据库以进行新的镜像会话，数据库所有者还必须使用 WITH NO RECOVERY 选项还原主体服务器上所有其他未完成的日志备份。  
  
 有关详细信息，请参阅[删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)、[为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)、[使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)、[使用数据库镜像端点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)，或[使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)。  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> 使用 Transact-SQL 开始镜像  
 发出 ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** 语句的顺序非常关键。  
  
1.  第一个语句必须在镜像服务器上运行。 发出此语句时，镜像服务器不会尝试联系任何其他服务器实例。 相反，镜像服务器指示其数据库先进行等待，直到主体服务器与镜像服务器建立联系。  
  
2.  第二个 ALTER DATABASE 语句必须在主体服务器上运行。 此语句使主体服务器尝试连接到镜像服务器。 创建此连接之后，镜像服务器随后将尝试通过其他连接与主体服务器建立连接。  
  
 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
> [!NOTE]  
>  有关使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 启动镜像的信息，请参阅[使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)。  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> 跨数据库事务  
 在具有自动故障转移功能的高安全性模式下镜像数据库时，自动故障转移可能会导致自动解析并且可能错误解析有疑问的事务。 如果提交跨数据库事务时在任一数据库中进行自动故障转移，则数据库之间可能发生逻辑上的不一致。  
  
 自动故障转移可能影响的跨数据库事务类型包括：  
  
-   正在同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中更新多个数据库的事务。  
  
-   使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 的事务。  
  
 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。  
  
## <a name="see-also"></a>另请参阅  
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


