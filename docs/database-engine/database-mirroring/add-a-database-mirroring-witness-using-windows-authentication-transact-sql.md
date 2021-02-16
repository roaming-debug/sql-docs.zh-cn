---
title: 配置见证服务器
description: '介绍如何使用 Transact-SQL 通过 Windows 身份验证配置数据库镜像见证服务器。 '
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- Windows authentication [SQL Server]
- database mirroring [SQL Server], witness
ms.assetid: bf5e87df-91a4-49f9-ae88-2a6dcf644510
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5702769994efd65abeeb330205a2a83362fee6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353252"
---
# <a name="add-a-database-mirroring-witness-using-windows-authentication-transact-sql"></a>使用 Windows 身份验证添加数据库镜像见证服务器 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  为了给数据库设置见证服务器，数据库所有者为见证服务器的角色分配数据库引擎实例。 见证服务器实例可以与主体服务器实例或镜像服务器实例运行于同一台计算机上，但这样会明显降低自动故障转移的可靠性。  
  
 极力建议见证服务器应位于另外一台计算机上。 给定的服务器可以参与到多个具有相同或不同伙伴的并发数据库镜像会话中。 给定的服务器在某些会话中可能是伙伴，而在其他会话中则是见证服务器。  
  
 见证服务器专用于具有自动故障转移功能的高安全性模式。 在设置见证服务器之前，极力建议确保 SAFETY 属性当前设置为 FULL。  
  
> [!IMPORTANT]  
>  我们建议您在非高峰时段配置数据库镜像，因为配置会影响性能。  
  
## <a name="establish-a-witness"></a>建立见证服务器  
  
1.  在见证服务器实例上，请确保存在用于数据库镜像的端点。 无论支持的镜像会话数是多少，服务器实例都只能有一个数据库镜像端点。 如果只将该服务器实例用于数据库镜像会话中的见证服务器，你可以为终结点分配见证服务器角色 (ROLE **=** WITNESS)。 如果要将该服务器实例用于其他数据库镜像会话中的伙伴，请将端点的角色分配为 ALL。  
  
     若要执行 SET WITNESS 语句，数据库镜像会话必须已启动（在伙伴之间），并且见证服务器端点的 STATE 必须设置为 STARTED。  
  
     若要了解见证服务器实例是否具有其数据库镜像端点并了解其角色和状态，请针对该实例使用以下 Transact-SQL 语句：  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  如果数据库镜像端点已存在并处于使用状态，则我们建议您将此端点用于服务器实例中的每一个会话。 删除正在使用的端点会中断现有会话的连接。 如果已经为会话设置了见证服务器，则删除数据库镜像端点会造成该会话的主体服务器失去仲裁；如果发生这种情况，数据库会进入脱机状态，其用户也会断开连接。 有关详细信息，请参阅[仲裁：见证服务器如何影响数据库可用性（数据库镜像）](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md)。  
  
     如果见证服务器缺少终结点，请参阅[为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)。  
  
2.  如果使用不同的域用户帐户运行伙伴实例，请在每个实例的 master 数据库中为不同帐户创建登录名。 有关详细信息，请参阅 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)。  
  
3.  连接到主体服务器并执行下面的语句：  
  
     ALTER DATABASE <database_name> SET WITNESS **=** <server_network_address>  
  
     其中，<database_name> 是要镜像的数据库的名称（此名称在两个伙伴上相同），<server_network_address> 是见证服务器实例的服务器网络地址。  
  
     服务器网络地址的语法如下：  
  
     TCP <b>://</b> _\<system-address>_ <b>:</b> _\<port>_  
  
     其中，\<*system-address>* 是明确标识目标计算机系统的字符串，\<*port>* 是合作服务器实例的镜像终结点使用的端口号。 有关详细信息，请参阅 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。  
  
     例如，在主体服务器实例上，下面的 ALTER DATABASE 语句设置见证服务器。 数据库名称为“AdventureWorks”，系统地址为 DBSERVER3（见证服务器系统的名称），见证服务器的数据库镜像终结点使用的端口为 `7022`：  
  
    ```  
    ALTER DATABASE AdventureWorks   
      SET WITNESS = 'TCP://DBSERVER3:7022'  
    ```  
  
## <a name="example"></a>示例  
 下面的示例将建立一个数据镜像见证服务器。 在见证服务器实例（ `WITNESSHOST4`上的默认实例）上：  
  
1.  为仅使用端口 `7022`的 WITNESS 角色创建此服务器实例的一个端点。  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    ```  
  
2.  如果伙伴实例与见证服务器实例的域用户帐户不同，则为伙伴实例的域用户帐户创建一个登录名；例如，假定见证服务器作为 `SOMEDOMAIN\witnessuser`运行，而伙伴作为 `MYDOMAIN\dbousername`运行。 为伙伴创建登录名，如下所示：  
  
    ```  
    --Create a login for the partner server instances,  
    --which are both running as MYDOMAIN\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [MYDOMAIN\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [MYDOMAIN\dbousername];  
    GO  
    ```  
  
3.  在每个伙伴服务器实例上，为见证服务器实例创建登录名：  
  
    ```  
    --Create a login for the witness server instance,  
    --which is running as SOMEDOMAIN\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [SOMEDOMAIN\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account   
    --of partners  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [SOMEDOMAIN\witnessuser];  
    GO  
    ```  
  
4.  在主体服务器上，设置见证服务器（位于 `WITNESSHOST4`上）：  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4:7022'  
    GO  
    ```  
  
> [!NOTE]  
>  服务器网络地址通过端口号来指示目标服务器实例，端口号映射到该实例的镜像端点。  
  
 有关显示安全设置、准备镜像数据库、设置伙伴以及添加见证服务器的完整示例的信息，请参阅[设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)   
 [从数据库镜像会话删除见证服务器 (SQL Server)](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)   
 [数据库镜像见证服务器](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
