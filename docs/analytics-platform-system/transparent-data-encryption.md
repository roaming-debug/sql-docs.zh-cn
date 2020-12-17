---
title: 透明数据加密
description: 并行数据仓库 (TDE) 的透明数据加密 (PDW) 对数据和事务日志文件以及特殊 PDW 日志文件执行实时 i/o 加密和解密。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: dc6b582895a684386ed2d14b0c31612dcd0a47d1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641563"
---
# <a name="transparent-data-encryption"></a>透明数据加密
可以采取多种预防措施来帮助保护数据库，例如，设计安全系统、加密机密资产，以及围绕数据库服务器构建防火墙。 但是，对于物理媒体 (例如驱动器或备份磁带) 被盗的情况，恶意方只需还原或附加数据库并浏览数据。 一种解决方案是加密数据库中的敏感数据，并通过证书保护用于加密数据的密钥。 这可以防止任何没有密钥的人使用这些数据，但这种保护必须事先计划。  
  
*透明数据加密* (TDE) 执行对数据和事务日志文件以及特殊 PDW 日志文件的实时 i/o 加密和解密。 加密使用数据库加密密钥 (DEK)，它存储在数据库引导记录中，可在恢复时使用。 DEK 是使用存储在 SQL Server PDW 的 master 数据库中的证书保护的对称密钥。 TDE 保护“处于休眠状态”的数据，即数据和日志文件。 它提供了遵从许多法律、法规和各个行业建立的准则的能力。 利用此功能，软件开发人员可以使用 AES 和3DES 加密算法来加密数据，而无需更改现有应用程序。  
  
> [!IMPORTANT]  
> TDE 不会为客户端与 PDW 之间传输的数据提供加密。 有关如何在客户端和 SQL Server PDW 之间加密数据的详细信息，请参阅 [预配证书](provision-certificate.md)。  
>   
> TDE 在移动或使用数据时不会对其进行加密。 SQL Server PDW 中 PDW 组件之间的内部流量未加密。 临时存储在内存缓冲区中的数据未加密。 若要缓解这种风险，请控制物理访问和与 SQL Server PDW 的连接。  
  
对数据库实施保护措施后，可以通过使用正确的证书还原此数据库。  
  
> [!NOTE]  
> 为 TDE 创建证书时，应立即备份该证书以及关联的私钥。 如果证书变为不可用，或者如果必须在另一台服务器上还原或附加数据库，则必须同时具有证书和私钥的备份，否则将无法打开该数据库。 即使不再对数据库启用 TDE，也应该保留加密证书。 即使数据库未加密，事务日志的某些部分仍可能保持受到保护，但在执行数据库的完整备份前，对于某些操作可能需要证书。 超过过期日期的证书仍可以用于通过 TDE 加密和解密数据。  
  
数据库文件加密在页面级执行。 已加密数据库中的页在写入磁盘之前会进行加密，在读入内存时会进行解密。 TDE 不会增加已加密数据库的大小。  
  
下图显示了 TDE 加密的密钥层次结构：  
  
![显示层次结构](media/tde-architecture.png "TDE_Architecture")  
  
## <a name="using-transparent-data-encryption"></a><a name="using-tde"></a>使用透明数据加密  
若要使用 TDE，请按以下步骤操作。 在准备 SQL Server PDW 来支持 TDE 时，前三个步骤只执行一次。  
  
1.  在 master 数据库中创建一个主密钥。  
  
2.  使用 **sp_pdw_database_encryption** 启用 SQL Server PDW 上的 TDE。 此操作会修改临时数据库，以确保对未来的临时数据的保护，如果存在任何具有临时表的活动会话，则会失败。 **sp_pdw_database_encryption** 在 pdw 系统日志中打开用户数据掩码。  (有关 PDW 系统日志中的用户数据屏蔽的详细信息，请参阅 [sp_pdw_log_user_data_masking](../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)。 )   
  
3.  使用 [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) 创建可以对将存储证书备份的共享进行身份验证和写入的凭据。 如果目标存储服务器已有凭据，则可以使用现有凭据。  
  
4.  在 master 数据库中，创建受主密钥保护的证书。  
  
5.  将证书备份到存储共享。  
  
6.  在用户数据库中，创建数据库加密密钥并通过存储在 master 数据库中的证书对其进行保护。  
  
7.  使用 `ALTER DATABASE` TDE 对数据库进行加密。  
  
下面的示例演示如何 `AdventureWorksPDW2012` 使用 `MyServerCert` 在 SQL Server PDW 中创建的名为的证书对数据库进行加密。  
  
**第一种：启用 SQL Server PDW 上的 TDE。** 此操作仅需要一次。  
  
```sql  
USE master;  
GO  
  
-- Create a database master key in the master database  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';  
GO  
  
-- Enable encryption for PDW  
EXEC sp_pdw_database_encryption 1;  
GO  
  
-- Add a credential that can write to the share  
-- A credential created for a backup can be used if you wish  
EXEC sp_pdw_add_network_credentials 'SECURE_SERVER', '<domain>\<Windows_user>', '<password>';  
```  
  
**其次：在 master 数据库中创建和备份证书。** 此操作仅需要一次。 可以为每个数据库创建一个单独的证书 (建议) ，也可以使用一个证书保护多个数据库。  
  
```sql  
-- Create certificate in master  
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';  
GO  
  
-- Back up the certificate with private key  
BACKUP CERTIFICATE MyServerCert   
    TO FILE = '\\SECURE_SERVER\cert\MyServerCert.cer'  
    WITH PRIVATE KEY   
    (   
        FILE = '\\SECURE_SERVER\cert\MyServerCert.key',  
        ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>'   
    )   
GO  
```  
  
**Last：创建 DEK，并使用 ALTER DATABASE 来加密用户数据库。** 此操作针对每个受 TDE 保护的数据库重复。  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_128  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
  
ALTER DATABASE AdventureWorksPDW2012 SET ENCRYPTION ON;  
GO  
```  
  
加密和解密操作由 SQL Server 安排在后台线程中执行。 您可以使用本文后面显示的列表中的目录视图和动态管理视图查看这些操作的状态。  
  
> [!CAUTION]  
> 启用了 TDE 的数据库的备份文件也使用数据库加密密钥进行加密。 因此，当您还原这些备份时，用于保护数据库加密密钥的证书必须可用。 也就是说，除了备份数据库之外，您还要确保自己保留了服务器证书的备份以防数据丢失。 如果证书不再可用，将会导致数据丢失。  
  
## <a name="commands-and-functions"></a>命令和函数  
TDE 证书必须使用数据库主密钥加密才能被下列语句接受。  
  
下表提供了 TDE 命令和函数的链接和说明。  
  
|命令或函数|目的|  
|-----------------------|-----------|  
|[CREATE DATABASE ENCRYPTION KEY](../t-sql/statements/create-database-encryption-key-transact-sql.md)|创建一个用于加密数据库的密钥。|  
|[ALTER DATABASE ENCRYPTION KEY](../t-sql/statements/alter-database-encryption-key-transact-sql.md)|更改用于加密数据库的密钥。|  
|[DROP DATABASE ENCRYPTION KEY](../t-sql/statements/drop-database-encryption-key-transact-sql.md)|删除用于加密数据库的密钥。|  
|[ALTER DATABASE](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw)|说明用于启用 TDE 的 **ALTER DATABASE** 选项。|  
  
## <a name="catalog-views-and-dynamic-management-views"></a>目录视图和动态管理视图  
下表显示了 TDE 目录视图和动态管理视图。  
  
|目录视图或动态管理视图|目的|  
|-------------------------------------------|-----------|  
|[sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|显示数据库信息的目录视图。|  
|[sys.certificates](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|显示数据库中的证书的目录视图。|  
|[sys.dm_pdw_nodes_database_encryption_keys](../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql.md)|动态管理视图，提供有关数据库中使用的加密密钥和数据库加密状态的每个节点的信息。|  
  
## <a name="permissions"></a>权限  
如上表中所述，TDE 的每项功能和每个命令都有各自的权限要求。  
  
查看 TDE 所涉及的元数据需要 `CONTROL SERVER` 权限。  
  
## <a name="considerations"></a>注意事项  
当进行数据库加密操作的重新加密扫描时，将禁用对数据库的维护操作。  
  
您可以使用 **sys.dm_pdw_nodes_database_encryption_keys** 动态管理视图查找数据库加密状态。 有关详细信息，请参阅本文前面的 *目录视图和动态管理视图* 部分。  
  
### <a name="restrictions"></a>限制  
在 `CREATE DATABASE ENCRYPTION KEY` 、、 `ALTER DATABASE ENCRYPTION KEY` 或语句期间，不允许执行以下操作 `DROP DATABASE ENCRYPTION KEY` `ALTER DATABASE...SET ENCRYPTION` 。  
  
-   删除数据库。  
  
-   使用 `ALTER DATABASE` 命令。  
  
-   正在启动数据库备份。  
  
-   启动数据库还原。  
  
以下操作或条件将阻止 `CREATE DATABASE ENCRYPTION KEY` 、 `ALTER DATABASE ENCRYPTION KEY` 、 `DROP DATABASE ENCRYPTION KEY` 或 `ALTER DATABASE...SET ENCRYPTION` 语句。  
  
-   正在 `ALTER DATABASE` 执行命令。  
  
-   正在进行任何数据备份。  
  
当创建数据库文件时，如果启用了 TDE，则即时文件初始化功能不可用。  
  
### <a name="areas-not-protected-by-tde"></a>不受 TDE 保护的区域  
TDE 不保护外部表。  
  
TDE 不保护诊断会话。 当使用诊断会话时，用户应注意不要使用敏感参数进行查询。 显示敏感信息的诊断会话应在不再需要时立即将其删除。  
  
将 TDE 保护的数据放置在 SQL Server PDW 内存中时对其进行解密。 当设备上出现某些问题时，将创建内存转储。 转储文件表示出现问题时内存的内容，并且可以包含未加密形式的敏感数据。 内存转储的内容应在与他人共享之前查看。  
  
Master 数据库不受 TDE 保护。 尽管 master 数据库不包含用户数据，但它确实包含登录名等信息。  
  
### <a name="transparent-data-encryption-and-transaction-logs"></a>透明数据加密与事务日志  
启用数据库以使用 TDE 会对虚拟事务日志的剩余部分进行清零，以强制执行下一个虚拟事务日志。 这可以保证在数据库设置为加密后事务日志中不会留有明文。 可以通过查看视图中的列来查找每个 PDW 节点上的日志文件加密状态 `encryption_state` `sys.dm_pdw_nodes_database_encryption_keys` ，如以下示例中所示：  
  
```sql  
WITH dek_encryption_state AS   
(  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id, encryption_state  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
        INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
           ON dek.database_id = node_db_map.database_id AND dek.pdw_node_id = node_db_map.pdw_node_id  
        LEFT JOIN sys.pdw_database_mappings AS db_map  
            ON node_db_map .physical_name = db_map.physical_name  
        INNER JOIN sys.dm_pdw_nodes AS nodes  
            ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
)  
SELECT TOP 1 encryption_state  
       FROM dek_encryption_state  
       WHERE dek_encryption_state.database_id = DB_ID('AdventureWorksPDW2012 ')  
       ORDER BY (CASE encryption_state WHEN 3 THEN -1 ELSE encryption_state END) DESC;  
```  
  
所有在数据库加密密钥更改前写入事务日志的数据都将使用之前的数据库加密密钥加密。  
  
### <a name="pdw-activity-logs"></a>PDW 活动日志  
SQL Server PDW 维护一组旨在进行故障排除的日志。  (注意，这并不是事务日志、SQL Server 错误日志或 Windows 事件日志。 ) 这些 PDW 活动日志可以包含明文形式的完整语句，其中某些语句可以包含用户数据。 典型的示例包括 **INSERT** 和 **UPDATE** 语句。 可以使用 **sp_pdw_log_user_data_masking** 显式打开或关闭用户数据屏蔽。 启用加密 SQL Server PDW 会自动打开 PDW 活动日志中用户数据的屏蔽，以便对其进行保护。 **sp_pdw_log_user_data_masking** 也可用于在不使用 TDE 时屏蔽语句，但不建议这样做，因为这样可以大大降低 Microsoft 支持部门团队分析问题的能力。  
  
### <a name="transparent-data-encryption-and-the-tempdb-system-database"></a>透明数据加密与 tempdb 系统数据库  
使用 [sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)启用加密时，将加密 tempdb 系统数据库。 这在任何数据库可以使用 TDE 之前都是必需的。 这可能会对同一实例 SQL Server PDW 上的未加密数据库产生性能影响。  
  
## <a name="key-management"></a>密钥管理  
 (DEK) 的数据库加密密钥受存储在 master 数据库中的证书保护。 这些证书受 master 数据库 (DMK) 的数据库主密钥保护。 DMK 需要受服务主密钥 (SMK) 的保护，以便用于 TDE。  
  
系统可以访问密钥，而无需人工干预 (例如提供密码) 。 如果证书不可用，系统将输出一个错误，该错误说明在有正确的证书可用之前无法解密 DEK。  
  
将数据库从一台设备移到另一台设备时，必须先在目标服务器上还原用于保护其 "DEK" 的证书。 然后，可以照常还原数据库。 有关详细信息，请参阅标准 SQL Server 文档： [将受 TDE 保护的数据库移到另一个 SQL Server](../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)。  
  
只要存在使用数据库备份的数据库备份，就应保留用于加密 Dek 的证书。 证书备份必须包含证书私钥，因为在没有私钥的情况下，不能将证书用于数据库还原。 这些证书私钥备份存储在单独的文件中，由必须为证书还原提供的密码进行保护。  
  
## <a name="restoring-the-master-database"></a>还原 master 数据库  
在灾难恢复过程中，可以使用 **DWConfig** 还原 master 数据库。  
  
-   如果控制节点未更改（即，如果在从其创建备份 master 数据库的相同和未更改的设备上还原了 master 数据库），则无需执行其他操作即可读取 DMK 和所有证书。  
  
-   如果在其他设备上还原 master 数据库，或者该控制节点自 master 数据库备份后发生了更改，则需要执行其他步骤才能重新生成 DMK。  
  
    1.  打开 DMK：  
  
        ```sql  
        OPEN MASTER KEY DECRYPTION BY PASSWORD = '<password>';  
        ```  
  
    2.  通过 SMK 添加加密：  
  
        ```sql  
        ALTER MASTER KEY   
            ADD ENCRYPTION BY SERVICE MASTER KEY;  
        ```  
  
    3.  重新启动设备。  
  
## <a name="upgrade-and-replacing-virtual-machines"></a>升级和替换虚拟机  
如果在执行升级或替换 VM 的设备上存在 DMK，则必须以参数形式提供 DMK 密码。  
  
升级操作的示例。 `**********`将替换为你的 DMK 密码。  
  
`setup.exe /Action=ProvisionUpgrade ... DMKPassword='**********'`  
  
用于替换虚拟机的操作的示例。  
  
`setup.exe /Action=ReplaceVM ... DMKPassword='**********'`  
  
在升级过程中，如果用户数据库已加密并且未提供 DMK 密码，升级操作将失败。 替换过程中，如果存在 DMK 时未提供正确的密码，则该操作将跳过 DMK 恢复步骤。 所有其他步骤都将在 "替换 VM" 操作结束时完成，但该操作将在结束时报告失败以指示需要执行其他步骤。 在 "安装日志" (位于 **\ProgramData\Microsoft\Microsoft SQL Server Parallel Data Warehouse\100\Logs\Setup \\<时间戳> \detail-setup**) 中，末尾附近会显示以下警告。  
  
`**_ WARNING \_\*\* DMK is detected in master database, but could not be recovered automatically! The DMK password was either not provided or is incorrect!`
  
在 PDW 中手动执行这些语句，然后重新启动设备，以便恢复 DMK：  
  
```sql
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<DMK password>';  
ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY;  
```
  
使用 **还原 Master 数据库** 段落中的步骤恢复数据库，然后重新启动设备。  
  
如果在操作后 DMK 已存在，但在执行该操作后未恢复，则在查询数据库时将引发以下错误消息。  
  
```sql
Msg 110806;  
A distributed query failed: Database '<db_name>' cannot be opened due to inaccessible files or insufficient memory or disk space. See the SQL Server errorlog for details.
```  
  
## <a name="performance-impact"></a>性能影响  
TDE 对性能的影响取决于你拥有的数据类型、存储方式以及 SQL Server PDW 上的工作负荷类型。 当受 TDE 保护时，读取并解密数据或加密数据，然后写入数据的 i/o 是 CPU 密集型活动，在同时发生其他 CPU 密集型活动时，会产生更大的影响。 由于 TDE 加密 `tempdb` ，TDE 可能会影响未加密的数据库的性能。 为了获得准确的性能，您应该使用您的数据和查询活动来测试整个系统。  
  
## <a name="related-content"></a>相关内容  
以下链接包含有关 SQL Server 如何管理加密的一般信息。 这些文章可帮助你了解 SQL Server 加密，但这些文章没有特定于 SQL Server PDW 的信息，它们讨论了 SQL Server PDW 中不存在的功能。  
  
-   [SQL Server 加密](../relational-databases/security/encryption/sql-server-encryption.md)  
  
-   [加密层次结构](../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [SQL Server 和数据库加密密钥](../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

  
## <a name="see-also"></a>另请参阅  
[ALTER DATABASE](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw)  
[CREATE MASTER KEY](../t-sql/statements/create-master-key-transact-sql.md)  
[CREATE DATABASE ENCRYPTION KEY](../t-sql/statements/create-database-encryption-key-transact-sql.md)  
[BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)  
[sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)  
[sp_pdw_database_encryption_regenerate_system_keys](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
[sp_pdw_log_user_data_masking](../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
[sys.certificates](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)  
[sys.dm_pdw_nodes_database_encryption_keys](../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql.md)  
