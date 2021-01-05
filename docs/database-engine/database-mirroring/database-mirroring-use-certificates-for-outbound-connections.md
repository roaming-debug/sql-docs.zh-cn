---
title: 数据库镜像 - 使用证书进行出站连接 | Microsoft Docs
description: 了解如何配置服务器实例，以在设置出站连接后使用证书对数据库镜像的入站连接进行身份验证。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- outbound connections [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 464c9096-10d6-4c5e-8bb1-19acba27ad9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 163b7af8795b663645133307317e39646deee6e5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637393"
---
# <a name="database-mirroring---use-certificates-for-outbound-connections"></a>数据库镜像 - 使用证书进行出站连接
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明配置服务器实例以使用证书对数据库镜像的出站连接进行身份验证的步骤。 必须配置出站连接，才可以设置入站连接。  
  
> [!NOTE]  
>  服务器实例上的所有镜像连接都使用单个数据库镜像端点，必须在创建端点时指定服务器实例的身份验证方法。  
  
 配置出站连接的进程分为以下基本步骤：  
  
1.  在 **master** 数据库中，创建数据库主密钥。  
  
2.  在 **master** 数据库中，为服务器实例创建加密证书。  
  
3.  使用服务器实例的证书为该服务器实例创建端点。  
  
4.  将证书备份到文件，并将其安全地复制到其他系统。  
  
 必须对每一个伙伴和见证服务器（如果存在）完成以上步骤。  
  
 下面的过程详细说明了这些步骤。 对于每个步骤，该过程都提供了一个在名为 HOST_A 的系统上配置服务器实例的示例。 伴随的示例部分说明了在名为 HOST_B 的系统上配置另一服务器实例的步骤（步骤相同）。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-configure-server-instances-for-outbound-mirroring-connections-on-host_a"></a>配置用于出站镜像连接的服务器实例（在 HOST_A 上）  
  
1.  在 **master** 数据库上，创建数据库主密钥（如果不存在）。 若要查看数据库的现有密钥，请使用 [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) 目录视图。  
  
     若要创建数据库主密钥，请使用下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令：  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
     使用唯一的强密码，并将其记录到一个安全的位置。  
  
     有关详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md) 和[创建数据库主密钥](../../relational-databases/security/encryption/create-a-database-master-key.md)。  
  
2.  在 **master** 数据库中，对服务器实例创建一个用于其数据库镜像出站连接的加密证书。  
  
     例如，为 HOST_A 系统创建一个证书。  
  
    > [!IMPORTANT]  
    >  如果您想要使用超过一年的证书，则通过在 CREATE CERTIFICATE 语句中使用 EXPIRY_DATE 选项，按 UTC 时间指定到期日期。 此外，我们建议您使用 SQL Server Management Studio 来创建基于策略的管理规则，以便在证书到期时提醒您。 使用策略管理的“创建新条件”对话框，在“证书”方面的“\@ExpirationDate”字段中创建此规则  。 有关详细信息，请参阅 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) 和 [保护 SQL Server](../../relational-databases/security/securing-sql-server.md)。  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate for database mirroring',   
       EXPIRY_DATE = '11/30/2013';  
    GO  
    ```  
  
     有关详细信息，请参阅 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)。  
  
     若要查看 **master** 数据库中的证书，可以使用下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    USE master;  
    SELECT * FROM sys.certificates;  
    ```  
  
     有关详细信息，请参阅 [sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)。  
  
3.  确保每个服务器实例上都存在数据库镜像端点。  
  
     如果服务器实例上已存在数据库镜像端点，则您应将该端点重新用于在服务器实例上建立的任何其他会话。 若要确定服务器实例上是否存在数据库镜像端点并查看其配置，请使用下面的语句：  
  
    ```  
    SELECT name, role_desc, state_desc, connection_auth_desc, encryption_algorithm_desc   
       FROM sys.database_mirroring_endpoints;  
    ```  
  
     如果端点不存在，请创建一个端点，该端点使用此证书进行出站连接，并使用此证书的凭据通过其他系统的验证。 这是一个服务器范围内的端点，供服务器实例参与的所有镜像会话使用。  
  
     例如，为 HOST_A 上的示例服务器实例创建镜像端点。  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
     有关详细信息，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)的信息。  
  
4.  备份证书并将其复制到其他系统。 若要在其他系统上配置入站连接，此步骤是必需的。  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
     有关详细信息，请参阅 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)。  
  
     使用您选择的任何安全方法复制此证书。 必须格外小心地保证所有证书的安全。  
  
 前面步骤中的示例代码将在 HOST_A 上配置出站连接。  
  
 您现在需要对 HOST_B 执行相同的出站步骤， 下面的示例部分说明了这些步骤。  
  
## <a name="example"></a>示例  
 下面的示例说明了如何配置 HOST_B 以进行出站连接。  
  
```  
USE master;  
--Create the database Master Key, if needed.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
GO  
-- Make a certifcate on HOST_B server instance.  
CREATE CERTIFICATE HOST_B_cert   
   WITH SUBJECT = 'HOST_B certificate for database mirroring',   
   EXPIRY_DATE = '11/30/2013';  
GO  
--Create a mirroring endpoint for the server instance on HOST_B.  
CREATE ENDPOINT Endpoint_Mirroring  
   STATE = STARTED  
   AS TCP (  
      LISTENER_PORT=7024  
      , LISTENER_IP = ALL  
   )   
   FOR DATABASE_MIRRORING (   
      AUTHENTICATION = CERTIFICATE HOST_B_cert  
      , ENCRYPTION = REQUIRED ALGORITHM AES  
      , ROLE = ALL  
   );  
GO  
--Backup HOST_B certificate.  
BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
GO   
--Using any secure copy method, copy C:\HOST_B_cert.cer to HOST_A.  
```  
  
 使用您选择的任何安全方法将证书复制到其他系统。 必须格外小心地保证所有证书的安全。  
  
> [!IMPORTANT]  
>  在建立出站连接之后，必须在每个服务器实例上为其他服务器实例配置入站连接。 有关详细信息，请参阅 [允许数据库镜像终结点将证书用于入站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)。  
  
 有关创建镜像数据库（包括 Transact-SQL 示例）的详细信息，请参阅[为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)。  
  
 有关建立高性能模式会话的 Transact-SQL 示例，请参阅[示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  
 建议您对数据库镜像连接进行加密，除非您能够保证网络的安全。  
  
 将证书复制到其他系统时，请使用安全的复制方法。  
  
## <a name="see-also"></a>另请参阅  
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
