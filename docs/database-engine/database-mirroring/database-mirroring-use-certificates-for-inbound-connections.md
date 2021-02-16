---
title: 使用证书进行入站连接
description: 介绍配置服务器实例以使用证书对数据库镜像的入站连接进行身份验证的步骤。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- inbound connections
- database mirroring [SQL Server], security
ms.assetid: 5d48bb98-61f0-4b99-8f1a-b53f831d63d0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c1ff8414ef765459bea59af5d158a0d4bdcd6cc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349223"
---
# <a name="database-mirroring---use-certificates-for-inbound-connections"></a>数据库镜像 - 使用证书进行入站连接
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明配置服务器实例以使用证书对数据库镜像的入站连接进行身份验证的步骤。 在可以建立入站连接之前，必须在每个服务器实例上配置出站连接。 有关详细信息，请参阅 [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)。  
  
 配置入站连接的过程通常有以下几个步骤：  
  
1.  为其他系统创建登录名。  
  
2.  创建一个使用该登录名的用户。  
  
3.  获取其他服务器实例的镜像端点的证书。  
  
4.  将该证书与在步骤 2 中创建的用户相关联。  
  
5.  授予对该镜像端点的登录名的 CONNECT 权限。  
  
 如果存在见证服务器，还必须为见证服务器设置进站连接。 这需要在两个伙伴上为见证服务器设置登录名、用户和证书，反之亦然。  
  
 下面的过程详细说明了这些步骤。 对于每个步骤，该过程都提供了一个在名为 HOST_A 的系统上配置服务器实例的示例。 伴随的示例部分说明了在名为 HOST_B 的系统上配置另一服务器实例的步骤（步骤相同）。  
  
### <a name="to-configure-server-instances-for-inbound-mirroring-connections-on-host_a"></a>为入站镜像连接配置服务器实例（在 HOST_A 上）  
  
1.  为其他系统创建登录名。  
  
     下面的示例在 HOST_A 上的服务器实例的 **master** 数据库中为系统 HOST_B 创建登录名；在此示例中，登录名为 `HOST_B_login`。 请用自己的密码替换示例密码。  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login   
       WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
     有关详细信息，请参阅 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)。  
  
     若要查看此服务器实例上的登录名，可以使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT * FROM sys.server_principals  
    ```  
  
     有关详细信息，请参阅 [ys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)。  
  
2.  创建一个使用该登录名的用户。  
  
     下面的示例为上述步骤中创建的登录名创建了一个用户 `HOST_B_user`。  
  
    ```  
    USE master;  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
     有关详细信息，请参阅 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)。  
  
     若要查看此服务器实例上的用户，可以使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT * FROM sys.sysusers;  
    ```  
  
     有关详细信息，请参阅 [sys.sysusers (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)。  
  
3.  获取其他服务器实例的镜像端点的证书。  
  
     如果配置出站连接时还没有获取远程服务器实例的镜像端点的证书副本，请执行此操作。 若要执行此操作，请按照 [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md) 在该服务器实例上对证书进行备份。 将证书复制到其他系统时，请使用安全的复制方法。 必须格外小心地保证所有证书的安全。  
  
     有关详细信息，请参阅 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)。  
  
4.  将该证书与在步骤 2 中创建的用户相关联。  
  
     下面的示例将 HOST_B 的证书与它在 HOST_A 上的用户关联。  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
     有关详细信息，请参阅 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)。  
  
     若要查看此服务器实例上的证书，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT * FROM sys.certificates  
    ```  
  
     有关详细信息，请参阅 [sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)。  
  
5.  授予对远程镜像端点的登录名的 CONNECT 权限。  
  
     例如，若要将对 HOST_A 的权限授予 HOST_B 上的远程服务器实例，以连接到其本地登录名，即连接到 `HOST_B_login`，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    USE master;  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
     有关详细信息，请参阅 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)。  
  
 这将完成对 HOST_B 登录到 HOST_A 所需的证书身份验证的设置。  
  
 您现在需要在 HOST_B 上为 HOST_A 执行相同的入站步骤， 下面的示例部分中示例的入站部分说明了这些步骤。  
  
## <a name="example"></a>示例  
 下面的示例说明了配置入站连接的 HOST_B。  
  
> [!NOTE]  
>  此示例使用一个包含 HOST_A 证书的证书文件，该证书由[允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md) 中的代码片段创建。  
  
```  
USE master;  
--On HOST_B, create a login for HOST_A.  
CREATE LOGIN HOST_A_login WITH PASSWORD = 'AStrongPassword!@#';  
GO  
--Create a user, HOST_A_user, for that login.  
CREATE USER HOST_A_user FOR LOGIN HOST_A_login  
GO  
--Obtain HOST_A certificate. (See the note   
--   preceding this example.)  
--Asscociate this certificate with the user, HOST_A_user.  
CREATE CERTIFICATE HOST_A_cert  
   AUTHORIZATION HOST_A_user  
   FROM FILE = 'C:\HOST_A_cert.cer';  
GO  
--Grant CONNECT permission for the server instance on HOST_A.  
GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO HOST_A_login  
GO  
```  
  
 如果打算在具有自动故障转移功能的高安全性模式下运行，则必须重复相同的设置步骤为出站连接和入站连接配置见证服务器。  
  
 有关创建镜像数据库（包括 Transact-SQL 示例）的详细信息，请参阅[为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)。  
  
 有关建立高性能模式会话的 Transact-SQL 示例，请参阅 [示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  
 将证书复制到其他系统时，请使用安全的复制方法。 必须格外小心地保证所有证书的安全。  
  
## <a name="see-also"></a>另请参阅  
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
