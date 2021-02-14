---
description: 还原已启用延伸的数据库 (Stretch Database)
title: 还原已启用延伸的数据库
ms.date: 07/06/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: cebc1f6d-d5ea-460d-ae60-d047d29c2723
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 18c4d602edea0f414863e3fc54bfa58d1e6ca2cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079788"
---
# <a name="restore-stretch-enabled-databases-stretch-database"></a>还原已启用延伸的数据库 (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  必要时，从多种类型的故障、错误和灾难中恢复并还原已备份的数据库。
  
  有关备份的详细信息，请参阅 [备份已启用延伸数据库](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)。

> [!TIP]
> 备份仅仅是完整的高可用性和业务连续性解决方案的一部分。 有关高可用性的详细信息，请参阅 [高可用性解决方案](../../database-engine/sql-server-business-continuity-dr.md)。

## <a name="restore-your-sql-server-data"></a>还原 SQL Server 数据
若要从硬件故障或损坏中恢复，请从备份中还原已启用延伸的 SQL Server 数据库。 你也可以继续使用当前使用的 SQL Server 还原方法。 有关详细信息，请参阅 [还原与恢复概述](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)。

还原 SQL Server 数据库之后，必须运行 **sys.sp_rda_reauthorize_db** 存储过程，重新建立已启用延伸的 SQL Server 数据库和远程 Azure 数据库之间的连接。 有关详细信息，请参阅 [还原 SQL Server 数据库和远程 Azure 数据库之间的连接](#reconnect)。

## <a name="restore-your-remote-azure-data"></a>还原远程 Azure 数据

### <a name="recover-a-live-azure-database"></a>恢复实时 Azure 数据库
在 Azure 快照上的 SQL Server Stretch Database 服务中，所有的实时数据（至少每隔 8 小时）都使用 Azure 存储快照。 这些快照将保留 7 天。 这样，便可以将数据还原到生成最新快照前 7 天内至少 21 个时间点中的一个。

要使用 Azure 门户将实时 Azure 数据库还原到较早的时间点，请执行以下操作。

1. 登录到 [Azure 门户][]。
2. 在屏幕左侧选择“**浏览**”，并选择“**SQL 数据库**”。
3. 导航到数据库，然后选择它。
4. 在数据库边栏选项卡的顶部，单击“还原”。
5. 指定新的 **数据库名称**，选择一个 **还原点**，并单击“**创建**”。
6. 数据库还原过程将随即开始，并且可以使用“**通知**”监视还原进度。

### <a name="recover-a-deleted-azure-database"></a>恢复已删除的 Azure 数据库
Azure 上的 SQL Server Stretch Database 服务在删除数据库之前会创建数据库快照，并将其保留 7 天。 之后，它不再保留实时数据库的快照。 这使你能够将已删除的数据库还原到被删除的时间点。

若要通过使用 Azure 门户将已删除的数据库还原到被删除的时间点，请执行以下操作。

1. 登录到 [Azure 门户][]。
2. 在屏幕左侧选择“**浏览**”，并选择“**SQL Server**”。
3. 导航到服务器，然后选择它。
4. 在服务器的边栏选项卡上向下滚动到“操作”，并单击“**删除的数据库**”磁贴。
5. 选择要使用的数据库。
5. 指定新的 **数据库名称**，并单击“**创建**”。
6. 数据库还原过程将随即开始，并且可以使用“**通知**”监视还原进度。

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a><a name="reconnect"></a>还原 SQL Server 数据库和远程 Azure 数据库之间的连接

1.  如果要连接到使用不同的名称或不同区域中的已还原的 Azure 数据库，请运行 [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) 存储过程，断开与之前 Azure 数据库的连接。  
  
2.  运行 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 存储过程，将已启用延伸的本地数据库重新连接到 Azure 数据库。  
  
    -   以 sysname 或 varchar(128) 值的形式提供现有数据库范围凭据。 （不要使用 varchar(max)。）可以在视图 **sys.database_scoped_credentials** 中查找凭据名称。  
  
    -   指定是否复制远程数据的副本并连接到副本（推荐）。  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO  
    ```  
    
  ## <a name="see-also"></a>另请参阅  
 [备份启用了延伸的数据库](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
 [对 Stretch Database 进行管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 
 [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)  
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
 
 [Azure 门户]: https://portal.azure.com/
 
