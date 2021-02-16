---
title: 使用 Windows 身份验证配置数据库镜像 (T-SQL)
description: 本文包含在 SQL Server 中使用 Windows 身份验证和 Transact-SQL 创建具有见证服务器的数据库镜像会话的示例。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: 35800769-aede-4aac-b077-0e0e487e302f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 50b27862389c0977abcfe5072534043458bd5b3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342312"
---
# <a name="example-configure-database-mirroring-using-windows-authentication-transact-sql"></a>示例：使用 Windows 身份验证配置数据库镜像 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  此示例说明使用 Windows 身份验证来创建带有见证服务器的数据库镜像会话所需的所有阶段。 本主题中的示例使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 注意，可以不使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，而使用配置数据库镜像安全向导来设置数据库镜像。 有关详细信息，请参阅本主题后面的 [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)。  
  
## <a name="prerequisite"></a>先决条件  
 该示例使用了 **AdventureWorks** 示例数据库，在默认情况下该数据库使用简单恢复模式。 若要对此数据库进行数据库镜像，必须将它更改为使用完整恢复模式。 若要用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 实现此目的，请使用 ALTER DATABASE 语句，如下所示：  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks   
SET RECOVERY FULL;  
GO  
```  
  
 有关更改 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中恢复模式的信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)。  
  
### <a name="permissions"></a>权限  
 需要对数据库的 ALTER 权限和 CREATE ENDPOINT 权限，或者需要 **sysadmin** 固定服务器角色的成员资格。  
  
## <a name="example"></a>示例  
 在此示例中，两个伙伴服务器和见证服务器分别是三台计算机系统上的默认服务器实例。 这三个服务器实例运行同一个 Windows 域，但本示例的见证服务器实例的用户帐户（用作启动服务帐户）有所不同。  
  
 下表总结了此示例中使用的值。  
  
|初始镜像角色|宿主系统|域用户帐户|  
|----------------------------|-----------------|-------------------------|  
|主体|PARTNERHOST1|\<Mydomain>\\<dbousername\>|  
|镜像|PARTNERHOST5|\<Mydomain>\\<dbousername\>|  
|见证|WITNESSHOST4|\<Somedomain>\\<witnessuser\>|  
  
1.  在主体服务器实例（PARTNERHOST1 中的默认实例）上创建端点。  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=PARTNER)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    -- Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
2.  在镜像服务器实例（PARTNERHOST5 中的默认实例）上创建端点。  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
3.  在见证服务器实例（WITNESSHOST4 中的默认实例）上创建端点。  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    --Create a login for the partner server instances,  
    --which are both running as Mydomain\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [Mydomain\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
4.  创建镜像数据库。 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
5.  在 PARTNERHOST5 中的镜像服务器实例上，将 PARTNERHOST1 中的服务器实例设置为伙伴（使它成为初始的主体服务器实例）。  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1.COM:7022'  
    GO  
    ```  
  
6.  在 PARTNERHOST1 中的主体服务器实例上，将 PARTNERHOST5 中的服务器实例设置为伙伴（使它成为初始的镜像服务器实例）。  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5.COM:7022'  
    GO  
    ```  
  
7.  在主体服务器中，设置见证服务器（位于 WITNESSHOST4 中）。  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4.COM:7022'  
    GO  
    ```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
-   [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [允许数据库镜像终结点将证书用于入站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
