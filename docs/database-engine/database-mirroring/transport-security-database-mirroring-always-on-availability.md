---
title: 传输安全性：可用性组和数据库镜像
description: 了解如何在参与 Always On 可用性组的数据库之间或托管在 SQL Server 上的数据库镜像会话中保护消息。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- cryptography [SQL Server], database mirroring
- certificates [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- transport security
- database mirroring [SQL Server], security
ms.assetid: 49239d02-964e-47c0-9b7f-2b539151ee1b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8ff60136994d809b78cf09ebd98e4cc36ceb1a02
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641621"
---
# <a name="transport-security---database-mirroring---always-on-availability"></a>传输安全性 - 数据库镜像 - AlwaysOn 可用性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  传输安全性涉及身份验证，或者还涉及数据库间交换的消息的加密。 对于数据库镜像和 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]，身份验证和加密在数据库镜像端点配置。 有关数据库镜像端点的简介，请参阅 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)。  
  
 **本主题内容：**  
  
-   [身份验证](#Authentication)  
  
-   [数据加密](#DataEncryption)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="authentication"></a><a name="Authentication"></a> 身份验证  
 身份验证是验证用户是否具有其所声明的身份的过程。 数据库镜像端点之间的连接需要进行身份验证。 伙伴或见证服务器的连接请求（如果有）必须进行身份验证。  
  
 用于数据库镜像或 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 的服务器实例使用的身份验证类型是数据库镜像端点的一种属性。 数据库镜像端点可以使用两种类型的传输安全性功能：Windows 身份验证（安全支持提供程序接口 (SSPI)）和基于证书的身份验证。  
  
### <a name="windows-authentication"></a>Windows 身份验证  
 在 Windows 身份验证下，每个服务器实例使用运行进程的 Windows 用户帐户的 Windows 凭据登录到另一端。 Windows 身份验证可能要求对登录帐户进行一些手动配置，如下所示：  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例基于相同的域帐户作为服务运行，则无需额外配置。  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例基于不同的域帐户（在相同的域或受信任的域中）作为服务运行，则必须在其他每个服务器实例上的 **master** 中创建各帐户的登录名，并且必须授予该登录帐户对端点的 CONNECT 权限。  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例作为网络服务帐户运行，则必须在其他每个服务器上的 _master_ **\\** _中创建各主机帐户 (_ DomainName **ComputerName$** ) 的登录名，并且必须授予该登录帐户对端点的 CONNECT 权限。 其原因在于，基于网络服务帐户运行的服务器实例使用主机的域帐户进行身份验证。  
  
> [!NOTE]  
>  有关使用 Windows 身份验证设置数据库镜像会话的示例，请参阅[示例：使用 Windows 身份验证设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)。  
  
### <a name="certificates"></a>证书  
 在某些情况下，例如服务器实例不在受信任域中或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为本地服务运行时，Windows 身份验证不可用。 在这种情况下，对连接请求进行身份验证需要使用证书，而不是用户凭据。 每个服务器实例的镜像端点必须使用其自己的本地创建的证书进行配置。  
  
 创建证书时，将建立加密方法。 有关详细信息，请参阅 [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)。 请务必小心管理您使用的证书。  
  
 建立连接时，服务器实例使用其证书的私钥建立自己的标识。 接收连接请求的服务器实例使用发送方证书的公钥对发送方的标识进行身份验证。 例如，请考虑两个服务器实例 Server_A 和 Server_B。 Server_A 先使用其私钥加密连接标头，然后再将连接请求发送到 Server_B。 Server_B 使用 Server_A 证书的公钥解密连接标头。 如果解密的标头是正确的，则 Server_B 就会知道该标头是 Server_A 加密的，从而连接通过了身份验证。 如果解密的标头是错误的，则 Server_B 就会知道该连接请求不可靠，从而拒绝连接。  
  
##  <a name="data-encryption"></a><a name="DataEncryption"></a> 数据加密  
 默认情况下，数据库镜像端点要求加密通过镜像连接发送的数据。 在这种情况下，端点只能连接到也使用加密的端点。 除非您可以保证您的网络是安全的，否则我们建议您对数据库镜像连接加密。 但是，您可以禁用加密或支持加密（不是必需的）。 如果禁用加密，则数据将永远不会加密，并且端点无法连接到要求加密的端点。 如果支持加密，则只有另一端点也支持或要求加密时，数据才会加密。  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建的镜像端点可以使用必需加密或禁用加密的方式来创建。 若要将加密设置为 SUPPORTED，请使用 ALTER ENDPOINT [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 有关详细信息，请参阅 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
 或者，您可以通过在 CREATE ENDPOINT 语句或 ALTER ENDPOINT 语句中为 ALGORITHM 选项指定下列值之一，控制端点所使用的加密算法：  
  
|ALGORITHM 值|说明|  
|---------------------|-----------------|  
|RC4|指定端点必须使用 RC4 算法。 这是默认值。<br /><br /> <strong>\*\* 警告 \*\*</strong> 不推荐使用 RC4 算法。 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 我们建议使用 AES。|  
|AES|指定端点必须使用 AES 算法。|  
|AES RC4|指定两个端点将与优先使用 AES 算法的此端点协商加密算法。|  
|RC4 AES|指定两个端点将与优先使用 RC4 算法的此端点协商加密算法。|  
  
 如果连接端点同时指定了这两种算法，但顺序不同，则选择接受连接的端点。  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，可以通过任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
>   
>  尽管 RC4 远远快于 AES，但它是一个相对较弱的算法，而 AES 是一个相对较强的算法。 因此，建议使用 AES 算法。  
  
 有关用于指定加密的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法的信息，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **为数据库镜像端点配置传输安全性**  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
## <a name="see-also"></a>另请参阅  
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [DROP ENDPOINT (Transact SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [SQL Server 数据库引擎和 Azure SQL 数据库的安全中心](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [sys.dm_db_mirroring_connections (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)   
 [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
