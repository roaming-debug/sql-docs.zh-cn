---
description: Enable Stretch Database for a database
title: Enable Stretch Database for a database
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling database
- enabling database for Stretch Database
ms.assetid: 37854256-8c99-4566-a552-432e3ea7c6da
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b93f34e5a5b716c649f0902dabefe651253c9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079978"
---
# <a name="enable-stretch-database-for-a-database"></a>Enable Stretch Database for a database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  若要为现有数据库配置 Stretch Database，请在 SQL Server Management Studio 中为数据库选择“任务 | Stretch | 启用”，以打开“为数据库启用 Stretch”向导。 你也可以使用 Transact-SQL 来为数据库启用 Stretch Database。  
  
 如果你为单个表选择“任务 | Stretch | 启用”，且尚未为数据库启用 Stretch Database，向导将会为数据库配置 Stretch Database，并在此过程中让你配置表。 请执行本主题中的步骤，而非[为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)中的步骤。  
  
 在数据库或表上启用 Stretch Database 需要 db_owner 权限。 对数据库启用 Stretch Database 还需要有 CONTROL DATABASE 权限。  

> [!NOTE]
> 之后如果要禁用 Stretch Database，请记住，禁用表或数据库的 Stretch Database 不会删除远程对象。 如果希望删除远程表或远程数据库，则需要使用 Azure 管理门户进行删除。 远程对象会继续产生 Azure 成本，直到手动删除它们。 
 
## <a name="before-you-get-started"></a>准备工作  
  
-   在为数据库配置 Stretch 之前，建议你运行 Stretch Database Advisor 以识别适合配置 Stretch 的数据库和表。 延伸数据库顾问还可识别阻碍性问题。 有关详细信息，请参阅 [通过运行 Stretch Database 顾问标识适用于 Stretch Database 的数据库和表](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)。  
  
-   查看 [Stretch Database 的限制](../../sql-server/stretch-database/limitations-for-stretch-database.md)。  
  
-   Stretch Database 会将数据迁移到 Azure。 因此，你必须拥有 Azure 帐户，以及计费的订阅。 若要获取 Azure 帐户， [请单击此处](https://azure.microsoft.com/pricing/free-trial/)。  
  
-   具有创建新 Azure 服务器或选择现有 Azure 服务器所需的连接和登录名信息。  
  
##  <a name="prerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a> 先决条件：在服务器上启用 Stretch Database  
 必须先在本地服务器上启用 Stretch Database，然后才能在数据库或表上启用它。 此操作需要 sysadmin 或 serveradmin 权限。  
  
-   如果你拥有必要的管理权限，“为数据库启用 Stretch”向导将会为服务器配置 Stretch。  
  
-   如果你没有必要的权限，在你运行向导之前，管理员必须通过运行 **sp_configure** 来手动启用该选项，否则便需由管理员运行该向导。  
  
 若要在服务器上手动启用 Stretch Database，请运行 **sp_configure** 并打开 **remote data archive** 选项。 以下示例通过将“**远程数据存档**”选项的值设置为 1 来启用该选项。  
  
```sql
EXEC sp_configure 'remote data archive' , '1';  
GO

RECONFIGURE;  
GO  
```  
  
 有关详细信息，请参阅[配置远程数据存档服务器配置选项](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md)和 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。  
  
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a> 使用向导在数据库上启用 Stretch Database  
 有关为数据库启用延伸向导的信息，包括必须输入的信息以及必须做出的选择，请参阅 [Get started by running the Enable Database for Stretch Wizard](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)（通过运行“启用数据库延伸向导”入门）。  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a> 使用 Transact-SQL 在数据库上启用 Stretch Database  
 必须先在数据库上启用 Stretch Database，然后才能在单个表上启用它。  
  
 在数据库或表上启用 Stretch Database 需要 db_owner 权限。 对数据库启用 Stretch Database 还需要有 CONTROL DATABASE 权限。  
  
1.  在开始之前，请选择 Stretch Database 要将数据迁移到的现有 Azure 服务器，或创建一个新的 Azure 服务器。  
  
2.  在该 Azure 服务器上，创建一个允许 SQL Server 与远程服务器通信的防火墙规则，该规则使用 SQL Server 的 IP 地址范围。  

    可以轻松找到所需值，并且通过尝试从 SQL Server Management Studio (SSMS) 中的对象资源管理器连接 Azure 服务器来创建防火墙规则。 SSMS 可帮助你通过打开以下对话框（已包括所需的 IP 地址值）来创建规则。
    
    ![Stretch 的防火墙规则](../../sql-server/stretch-database/media/firewall-rule-for-stretch.png)
  
3.  若要为 SQL Server 数据库配置 Stretch Database，该数据库必须拥有数据库主密钥。 数据库主密钥能保护 Stretch Database 用来连接到远程数据库的凭据。 下面是创建一个新数据库主密钥的示例。  
  
    ```sql  
    USE <database>; 
    GO  
  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD='<password>'; 
    GO
    ```  
    有关数据库主密钥的详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md) 和[创建数据库主密钥](../../relational-databases/security/encryption/create-a-database-master-key.md)。
    
4.  当你为数据库配置 Stretch Database 时，必须为 Stretch Database 提供凭据，以供本地 SQL Server 与远程 Azure 服务器之间进行通信使用。 有两个选项。  
  
    -   你可以提供管理员凭据。  
  
        -   如果通过运行向导启用 Stretch Database，则可以在那时创建凭据。  
  
        -   如果计划通过运行 **ALTER DATABASE** 启用 Stretch Database，则必须在运行 **ALTER DATABASE** 以启用 Stretch Database 之前手动创建凭据。 
        
        下面是创建一个新凭据的示例。
  
        ```sql  
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>  
            WITH IDENTITY = '<identity>' , SECRET = '<secret>' ;
        GO   
        ```  

         有关凭据的详细信息，请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。 创建凭据需要 ALTER ANY CREDENTIAL 权限。  

    -   你可以使用联合服务帐户，让 SQL Server 与远程 Azure 服务器在下列条件全数成立时进行通信。  
  
        -   用来运行 SQL Server 实例的服务帐户为域帐户。  
  
        -   域帐户属于其 Active Directory 与 Azure Active Directory 联合的域。  
  
        -   远程 Azure 服务器已配置为支持 Azure Active Directory 身份验证。  
  
        -   必须将运行 SQL Server 实例的服务帐户配置为远程 Azure 服务器上的 dbmanager 或 sysadmin 帐户。  
  
5.  若要为数据库配置 Stretch Database，请运行 ALTER DATABASE 命令。  
  
    1.  对于 SERVER 参数，请提供现有 Azure 服务器的名称，包括名称的 `.database.windows.net` 部分 — 例如 `MyStretchDatabaseServer.database.windows.net`。  
  
    2.  提供具有 CREDEMTIAL 参数的现有管理员凭据，或指定 FEDERATED_SERVICE_ACCOUNT = ON。 以下示例提供了一个现有的凭据。  
  
    ```sql  
    ALTER DATABASE <database name>  
        SET REMOTE_DATA_ARCHIVE = ON  
            (  
                SERVER = '<server_name>' ,  
                CREDENTIAL = <db_scoped_credential_name>  
            ) ;  
    GO
    ```  
  
## <a name="next-steps"></a>后续步骤  
-   [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) 以启用其他表。  
  
-   [数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) 以查看数据迁移的状态。  
  
-   [暂停和恢复数据迁移 (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [延伸数据库的管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [备份启用了延伸的数据库](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [还原已启用延伸的数据库](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>另请参阅  
 [通过运行 Stretch Database 顾问标识适用于 Stretch Database 的数据库以及表](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
