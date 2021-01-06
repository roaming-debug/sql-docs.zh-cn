---
title: 配置数据库镜像
description: 介绍使用 Windows 身份验证配置主体与镜像之间数据库镜像关系的步骤。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7584e4611d957f48efcb73d12b5965f0a4727b45
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642013"
---
# <a name="configure-database-mirroring"></a>配置数据库镜像
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改为使用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。  
  
 准备好镜像数据库后（请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)），便可以建立数据库镜像会话。 主体服务器实例、镜像服务器实例和见证服务器实例都必须是单独的服务器实例，并位于单独的主机系统中。  
  
> [!IMPORTANT]
>  我们建议您在非高峰时段配置数据库镜像，因为配置镜像会影响性能。  
> 
> [!NOTE]
>  给定的服务器实例可以参与到多个具有相同或不同伙伴的并发数据库镜像会话中。 某个服务器实例可能在某些会话中是伙伴，而在其他会话中则是见证服务器。 镜像服务器实例必须与主体服务器实例运行相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 并非 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].的每个版本都提供数据库镜像。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。 此外，极力建议这些服务器实例在可以处理相同工作负荷的类似系统上运行。  
  
### <a name="to-establish-a-database-mirroring-session"></a>建立数据库镜像会话  
  
1.  创建镜像数据库。 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
2.  在每个服务器实例上设置安全性。  
  
     数据库镜像会话中的每个服务器实例都需要一个数据库镜像端点。 如果端点不存在，则必须先创建。  
  
    > [!NOTE]  
    >  服务器实例对数据库镜像使用的验证形式是其数据库镜像端点的一种属性。 两种类型的传输安全性可用于数据库镜像：Windows 身份验证或基于证书的身份验证。 有关详细信息，请参阅 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)。  
  
     在每台主体服务器和镜像服务器上，请确保存在用于数据库镜像的端点。 无论支持的镜像会话数是多少，服务器实例都只能有一个数据库镜像端点。 如果只将该服务器实例用于数据库镜像会话中的伙伴，你可以为终结点分配伙伴角色 (ROLE **=** PARTNER)。 如果还要将该服务器用于其他数据库镜像会话中的见证服务器，则请将端点的角色分配为 ALL。  
  
     若要执行 SET PARTNER 语句，必须将两个合作伙伴的端点的 STATE 都设置为 STARTED。  
  
     若要了解服务器实例是否具有数据库镜像端点并了解其角色和状态，请对该实例使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  请勿重新配置正在使用的数据库镜像端点。 如果数据库镜像端点已存在并处于使用状态，则我们建议您将此端点用于服务器实例中的每一个会话。 删除正在使用的端点可导致端点重新启动，中断现有会话的连接，从而导致其他服务器实例出现错误。 这在具有自动故障转移功能的高安全性模式下尤为重要，在此模式下，在伙伴上重新配置端点可能会导致故障转移。 此外，如果已经为会话设置了见证服务器，则删除数据库镜像端点会造成该会话的主体服务器失去仲裁；如果发生这种情况，数据库会进入脱机状态，其用户也会断开连接。 有关详细信息，请参阅[仲裁：见证服务器如何影响数据库可用性（数据库镜像）](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md)。  
  
     如果任一伙伴缺少终结点，请参阅[为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)。  
  
3.  如果服务器实例使用不同的域用户帐户运行，则每个实例还需要在其他实例的 **master** 数据库中具有登录名。 如果登录名不存在，则必须先创建。 有关详细信息，请参阅 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)。  
  
4.  若要将主体服务器设置为镜像数据库中的伙伴，请连接到镜像服务器，然后执行下面的语句：  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     其中，\<database\_name\> 是要镜像的数据库的名称（此名称在两个伙伴上相同），\<server\_network\_address\> 是主体服务器的服务器网络地址 。  
  
     服务器网络地址的语法如下：  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     其中，\<system-address> 是明确标识目标计算机系统的字符串，\<port> 是伙伴服务器实例的镜像终结点使用的端口号 。 有关详细信息，请参阅 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。  
  
     例如，在镜像服务器实例中，下面的 ALTER DATABASE 语句将伙伴设置为原始主体服务器实例。 数据库名称为“AdventureWorks”，系统地址为 DBSERVER1（伙伴系统的名称），伙伴数据库镜像终结点使用的端口为 7022：  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     与主体服务器连接时，此语句将准备镜像服务器来形成会话。  
  
5.  若要将镜像服务器设置为主体数据库中的伙伴，请连接到主体服务器，然后执行下面的语句：  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     有关详细信息，请参阅步骤 4。  
  
     例如，在主体服务器实例中，下面的 ALTER DATABASE 语句将伙伴设置为原始镜像服务器实例。 数据库名称为“AdventureWorks”，系统地址为 DBSERVER2（伙伴系统的名称），伙伴数据库镜像终结点使用的端口为 7025：  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     在主体服务器上输入此语句将启动数据库镜像会话。  
  
6.  默认情况下，会话设置为完整事务安全（SAFETY 设置为 FULL），此设置会在同步、不带自动故障转移功能的高安全性模式下启动会话。 您可以将会话重新配置为在具有自动故障转移功能的高安全性模式下运行，或者在异步、高性能模式下运行，如下所示：  
  
    -   **具有自动故障转移的高安全性模式**  
  
         如果希望高安全性模式会话支持自动故障转移，则请添加见证服务器实例。 有关详细信息，请参阅[使用 Windows 身份验证添加数据库镜像见证服务器 (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)。  
  
    -   **高性能模式**  
  
         另外，如果您不想进行自动故障转移，并且您对性能的追求超过了可用性，则请关闭事务安全。 有关详细信息，请参阅[更改数据库镜像会话中的事务安全 (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)。  
  
        > [!NOTE]  
        >  在高性能模式下，WITNESS 应设置为 OFF。 有关详细信息，请参阅[仲裁：见证服务器如何影响数据库可用性（数据库镜像）](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md)。  
  
## <a name="example"></a>示例  
  
> [!NOTE]  
>  下面的示例针对现有的镜像数据库在伙伴间建立了数据库镜像会话。 有关创建镜像数据库的详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)。  
  
 该示例显示了创建没有见证服务器的数据库镜像会话的基本步骤。 这两个伙伴是两个计算机系统（PARTNERHOST1 和 PARTNERHOST5）中的默认服务器实例。 这两个伙伴实例运行相同的 Windows 域用户帐户 (MYDOMAIN\dbousername)。  
  
1.  在主体服务器实例（PARTNERHOST1 中的默认实例）中，创建支持所有使用端口 7022 的角色的端点：  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  有关如何设置登录名的示例，请参阅 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).的每个版本都提供数据库镜像。  
  
2.  在镜像服务器实例（PARTNERHOST5 中的默认实例）中，创建支持所有使用端口 7022 的角色的端点：  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  在主体服务器实例（位于 PARTNERHOST1）中，备份数据库：  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  在镜像服务器实例（位于 `PARTNERHOST5`）中还原数据库：  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  创建数据库的完整备份之后，必须在主体数据库中创建日志备份。 例如，下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将日志备份到先前数据库备份所使用的同一个文件中：  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  在开始镜像之前，必须应用必要的日志备份（以及所有后续日志备份）。  
  
     例如，以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句还原 C:\AdventureWorks.bak 中的第一个日志：  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  在镜像服务器实例中，将 PARTNERHOST1 中的服务器实例设置为伙伴（使它成为初始主体服务器）：  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  默认情况下，数据库镜像会话在同步模式下运行，这是由于将会话设置为完整事务安全（SAFETY 设置为 FULL）。 若要使会话在异步、高性能模式下运行，请将 SAFETY 设置为 OFF。 有关详细信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
8.  在主体服务器实例上，将 `PARTNERHOST5` 上的服务器实例设置为伙伴（使其成为初始镜像服务器）：  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. 或者，如果想使用具有自动故障转移功能的高安全性模式，则请设置见证服务器实例。 有关详细信息，请参阅[使用 Windows 身份验证添加数据库镜像见证服务器 (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)。  
  
> [!NOTE]  
>  有关显示安全设置、准备镜像数据库、设置伙伴以及添加见证服务器的完整示例的信息，请参阅[设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [数据库镜像和日志传送 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

