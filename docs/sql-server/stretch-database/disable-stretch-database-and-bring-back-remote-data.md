---
description: 禁用 Stretch Database 和移回远程数据
title: 禁用 Stretch Database 和移回远程数据
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: c1bbb24e-47e3-46aa-b786-fcadf9fb65ce
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 315cfed40b1ccdadf349d9b371d6baac5928d3af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081020"
---
# <a name="disable-stretch-database-and-bring-back-remote-data"></a>禁用 Stretch Database 和移回远程数据
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  若要禁用表的 Stretch Database，请在 SQL Server Management Studio 中为表选择“拉伸”  。 然后选择以下选项之一：  
  
-   **禁用 | 从 Azure 返回数据**。 将表的远程数据从 Azure 复制回到 SQL Server，然后为该表禁用 Stretch Database。 此操作会产生数据传输费用，并且不可取消。  
  
-   **禁用 | 将数据保留在 Azure 中**。 禁用表的 Stretch Database。  放弃 Azure 中的表的远程数据。  
  
 还可以使用 Transact-SQL 禁用表或数据库的 Stretch Database。  
  
 为表禁用 Stretch Database 之后，数据迁移会停止，查询结果不再包括来自远程表的结果。  
  
 如果只想暂停数据迁移，请参阅 [暂停和恢复数据迁移 (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)。  
  
> [!NOTE]
> 针对表或数据库禁用 Stretch Database 不会删除远程对象。 如果希望删除远程表或远程数据库，则需要使用 Azure 管理门户进行删除。 远程对象会继续产生 Azure 成本，直到删除它们。 有关详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
## <a name="disable-stretch-database-for-a-table"></a>禁用表的 Stretch Database  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>使用 SQL Server Management Studio 禁用表的 Stretch Database  
  
1.  在 SQL Server Management Studio 的对象资源管理器中，选择希望对其禁用 Stretch Database 的表。  
  
2.  右键单击并选择“拉伸”，然后选择下列选项之一。  
  
    -   **禁用 | 从 Azure 返回数据**。 将表的远程数据从 Azure 复制回到 SQL Server，然后为该表禁用 Stretch Database。 此命令不能取消。  
  
        > [!NOTE]
        > 将表的远程数据从 Azure 复制回 SQL Server 会产生数据传输费用。 有关详细信息，请参阅 [数据传输定价详细信息](https://azure.microsoft.com/pricing/details/data-transfers/)。  
  
         将所有远程数据从 Azure 复制回到 SQL Server 之后，将为表禁用延伸。  
  
    -   **禁用 | 将数据保留在 Azure 中**。 禁用表的 Stretch Database。  放弃 Azure 中的表的远程数据。  
  
    > [!NOTE]
    > 禁用表的 Stretch Database 不会删除远程数据或远程表。 如要删除远程表，必须使用 Azure 管理门户进行删除。 远程表会继续产生 Azure 成本，直到被删除。 有关详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>使用 Transact-SQL 禁用表的 Stretch Database  
  
-   要禁用表的“延伸”和将表中的远程数据从 Azure 复制回 SQL Server，请运行以下命令。当所有远程数据已从 Azure 复制回 SQL Server 后，禁用表的“延伸”。

    此命令不能取消。  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ; 
    GO 
    ```  
  
    > [!NOTE]
    > 将表的远程数据从 Azure 复制回 SQL Server 会产生数据传输费用。 有关详细信息，请参阅 [数据传输定价详细信息](https://azure.microsoft.com/pricing/details/data-transfers/)。    
  
-   若要为某个表禁用延伸并放弃远程数据，请运行以下命令。  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ; 
    GO
    ```  
  
> [!NOTE]
> 禁用表的 Stretch Database 不会删除远程数据或远程表。 如要删除远程表，必须使用 Azure 管理门户进行删除。 远程表会继续产生 Azure 成本，直到被删除。 有关详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
## <a name="disable-stretch-database-for-a-database"></a>为数据库禁用 Stretch Database  
 禁用数据库的 Stretch Database 之前，必须在数据库中已启用“拉伸”的各个表上禁用 Stretch Database。  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>使用 SQL Server Management Studio 为数据库禁用 Stretch Database  
  
1.  在 SQL Server Management Studio 的对象资源管理器中，选择要对其禁用 Stretch Database 的数据库。  
  
2.  右键单击并选择“任务”，选择“延伸”，然后选择“禁用”。  
  
> [!NOTE]
> 禁用数据库的 Stretch Database 不会删除远程数据库。 若要删除远程数据库，必须使用 Azure 管理门户。 远程数据库会继续产生 Azure 成本，直到被删除。 有关详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>使用 Transact-SQL 禁用数据库的 Stretch Database  
 运行以下命令。  
  
```sql  
ALTER DATABASE <Stretch-enabled database name>  
    SET REMOTE_DATA_ARCHIVE = OFF ;  
GO 
```  
  
> [!NOTE]
> 禁用数据库的 Stretch Database 不会删除远程数据库。 若要删除远程数据库，必须使用 Azure 管理门户。 远程数据库会继续产生 Azure 成本，直到被删除。 有关详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [暂停和恢复数据迁移 (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
  
