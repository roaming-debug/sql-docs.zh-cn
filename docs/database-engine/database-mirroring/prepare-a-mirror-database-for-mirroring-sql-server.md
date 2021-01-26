---
title: 为镜像准备镜数据库
description: 了解如何通过使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中准备用于数据库镜像的 SQL Server 数据库。
ms.custom: seo-lt-2019
ms.date: 11/10/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], preparing for mirroring
- logins [SQL Server], database mirroring
- mirror database [SQL Server]
ms.assetid: 8676f9d8-c451-419b-b934-786997d46c2b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1996d9610067e7db52c538b20df0680fbc741548
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766380"
---
# <a name="prepare-a-mirror-database-for-mirroring-sql-server"></a>为镜像准备镜像数据库 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在数据库镜像会话开始之前，数据库所有者或系统管理员必须确保已创建镜像数据库并可进行镜像。 创建新镜像数据库的最低要求是：执行主体数据库的完整备份和一个后续日志备份，并使用 WITH NORECOVERY 将这两个备份还原到镜像服务器实例上。  
  
 本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中准备镜像数据库。  
  
-   **开始之前：**  
  
     [要求](#Requirements)  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   [准备现有镜像数据库以重新启动镜像](#PrepareToRestartMirroring)  
  
-   [准备新的镜像数据库](#CombinedProcedure)  
  
-   **跟进：** [准备镜像数据库之后](#FollowUp)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="requirements"></a><a name="Requirements"></a> 要求  
  
-   主体服务器和镜像服务器实例必须运行在相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上。 尽管镜像服务器可以具有更高版本的 SQL Server，但仅在仔细计划的升级过程中建议此配置。 在此类配置中，会面临自动故障转移的风险，数据移动在其中会被自动挂起，因为数据不能移到更低版本的 SQL Server。 有关详细信息，请参阅 [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)。  
  
-   主体服务器和镜像服务器实例必须运行在相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上。 有关 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中数据库镜像支持的信息，请参阅 [SQL Server 2017 的各版本和支持的功能](~/sql-server/editions-and-components-of-sql-server-2017.md)。  
  
-   数据库必须使用完整恢复模式。  
  
     有关详细信息，请参阅[查看或更改数据库的恢复模式 (SQL Server)](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) 或 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 和 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
-   镜像数据库的名称必须与主体数据库的名称相同。  
  
-   为使镜像正常运行，镜像数据库必须处于 RESTORING 状态。 准备镜像数据库时，对于每个还原操作都必须使用 RESTORE WITH NORECOVERY。 至少，需要还原主体数据库的完整备份（WITH NORECOVERY），之后进行所有后续的日志备份。  
  
-   计划创建镜像数据库的系统的磁盘驱动器空间必须足以容纳镜像数据库。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不能镜像 **master**、 **msdb**、 **temp** 或 **model** 系统数据库。  
  
-   不能镜像属于 [AlwaysOn 可用性组](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)的数据库。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   使用最近的主体数据库的完整数据库备份或最近的差异数据库备份。  
  
-   如果计划在主体数据库中非常频繁地运行日志备份作业，则可能需要禁用备份作业，直到镜像启动为止。  
  
-   如有可能，镜像数据库的路径（包括驱动器号）应该与主体数据库的路径相同。  
  
     如果文件路径必须互不相同（例如，如果主体数据库位于“F:”驱动器上，但镜像系统没有“F:”驱动器），则必须在 RESTORE 语句中加入 MOVE 选项。  
  
    > [!IMPORTANT]  
    >  在不影响会话的情况下，在镜像会话过程中添加文件要求该文件路径同时存在于两个服务器上。 因此，如果在创建镜像数据库时移动了数据库文件，则随后在镜像数据库上的添加文件操作可能会失败，并可能会导致镜像挂起。 有关处理失败的创建文件操作的详细信息，请参阅[数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)。  
  
-   如果主体数据库具有任何全文目录，建议参阅[数据库镜像和全文目录 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)。  
  
-   对于生产数据库，始终备份到单独的设备。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 备份数据库时，TRUSTWORTHY 设置为 OFF。 因此，在新的镜像数据库中，TRUSTWORTHY 始终为 OFF。 如果数据库在故障转移之后需要得到信任，则必须执行其他设置步骤。 有关详细信息，请参阅 [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)中准备镜像数据库。  
  
 有关启用镜像数据库主秘钥自动加密的详细信息，请参阅 [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 数据库所有者或系统管理员。  
  
##  <a name="to-prepare-an-existing-mirror-database-to-restart-mirroring"></a><a name="PrepareToRestartMirroring"></a> 准备现有镜像数据库以重新启动镜像  
 如果已删除镜像，并且该镜像数据库仍处于 RECOVERING 状态，则可以重新启动镜像。  
  
1.  至少进行主体数据库的一个日志备份。 有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)数据库还原到一个新位置并且可以选择重命名该数据库。  
  
2.  在镜像数据库中，使用 RESTORE WITH NORECOVERY 还原删除镜像后在主体数据库中执行的所有日志备份。 有关详细信息，请参阅 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)中准备镜像数据库。  
  
##  <a name="to-prepare-a-new-mirror-database"></a><a name="CombinedProcedure"></a> 准备新的镜像数据库  
 **准备镜像数据库**  
  
> [!NOTE]  
>  有关此过程的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  连接到主体服务器实例。  
  
2.  创建主体数据库的完整数据库备份或差异数据库备份。  
  
    -   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
    -   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)。  
  
3.  一般您需要至少进行主体数据库的一个日志备份。 但是，如果数据库刚刚创建而尚未进行日志备份，或者如果恢复模式刚刚从 SIMPLE 更改为 FULL，则不必进行日志备份。  
  
    -   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
4.  除非备份是在从两个系统均可访问的网络驱动器上，否则将数据库备份和日志备份复制到将承载镜像服务器实例的系统。  
  
5.  连接到镜像服务器实例。  
  
6.  使用 RESTORE WITH NORECOVERY，通过还原完整数据库备份和最近的差异数据库备份（后者为可选项）到镜像服务器实例，来创建镜像数据库。  
  
    > [!NOTE]  
    >  如果要逐个文件组地还原数据库，则要确保还原整个数据库。  
  
    -   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
    -   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) 和 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)中准备镜像数据库。  
  
7.  使用 RESTORE WITH NORECOVERY，将所有未完成的日志备份应用到镜像数据库。  
  
    -   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 必须先创建镜像数据库，才能启动数据库镜像会话。 应该在启动镜像会话之前执行此操作。  
  
 此示例使用了 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库，默认情况下，该数据库使用简单恢复模式。  
  
1.  若要对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库使用数据库镜像，请改用完整恢复模式：  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  将数据库的恢复模式从 SIMPLE 更改为 FULL 之后，创建一个完整备份，以用于创建镜像数据库。 由于恢复模式已更改，因此指定了 WITH FORMAT 选项来创建新的介质集。 这对区分完整恢复模式下的备份与以前在简单恢复模式下创建的备份非常有用。 为了实现此示例的目的，在数据库所在的同一驱动器上创建备份文件 (`C:\AdventureWorks.bak`)。  
  
    > [!NOTE]  
    >  对于生产数据库，应始终备份到单独的设备。  
  
     在主体服务器实例 ( `PARTNERHOST1`) 上，创建主体数据库的完整备份，如下所示：  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  将完整备份复制到镜像服务器。  
  
4.  使用 RESTORE WITH NORECOVERY，将完整备份还原到镜像服务器实例。 还原命令取决于主体数据库与镜像数据库的路径是否相同。  
  
    -   **如果路径相同：**  
  
         在 `PARTNERHOST5`的镜像服务器实例上，还原完整备份，如下所示：  
  
        ```  
        RESTORE DATABASE AdventureWorks   
            FROM DISK = 'C:\AdventureWorks.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **如果路径不同：**  
  
         如果镜像数据库的路径与主体数据库的路径不同（例如，它们所在的驱动器号不同），则创建镜像数据库要求还原操作包含 MOVE 子句。  
  
        > [!IMPORTANT]  
        >  如果主体数据库与镜像数据库的路径名称不同，则无法添加文件。 原因是在接收添加文件操作所需的日志时，镜像服务器实例尝试将新文件放置在主体数据库所在的位置。  
  
         例如，以下命令将位于 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\ 中的主体数据库备份还原到镜像数据库所在的另一个位置 D:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Dat`a\`。  
  
        ```  
        RESTORE DATABASE AdventureWorks  
           FROM DISK='C:\AdventureWorks.bak'  
           WITH NORECOVERY,   
              MOVE 'AdventureWorks_Data' TO   
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Data.mdf',   
              MOVE 'AdventureWorks_Log' TO  
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Log.ldf';  
        GO  
        ```  
  
5.  创建完整备份之后，必须在主体数据库中创建日志备份。 例如，下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将日志备份到先前的完整备份所使用的文件中：  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  在开始镜像之前，必须应用必要的日志备份（以及所有后续日志备份）。  
  
     例如，以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句还原 `C:\AdventureWorks.bak`中的第一个日志：  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  如果在开始镜像之前进行任何其他日志备份，则还必须使用 WITH NORECOVERY 按顺序将所有这些日志备份还原到镜像服务器上。  
  
     例如，以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句还原 `C:\AdventureWorks.bak`中的其他两个日志：  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
 有关设置数据库镜像、显示安全设置、准备镜像数据库、设置合作伙伴以及添加见证服务器的完整示例的信息，请参阅 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)中准备镜像数据库。  
  
##  <a name="follow-up-after-preparing-a-mirror-database"></a><a name="FollowUp"></a> 跟进：准备镜像数据库之后  
  
1.  如果在最近的 RESTORE LOG 操作之后已执行了任何其他日志备份，则还必须使用 RESTORE WITH NORECOVERY 手动应用其他每个日志备份。  
  
2.  开始镜像会话。 有关详细信息，请参阅 [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) 在 [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)中准备镜像数据库。  
  
3.  如果您在主体数据库上禁用了备份作业，则重新启用该作业。  
  
4.  如果数据库在故障转移后需要得到信任，则必须在镜像开始后执行额外的设置步骤。 有关详细信息，请参阅 [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)中准备镜像数据库。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [数据库镜像和全文目录 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)   
 [数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)  
  
  

