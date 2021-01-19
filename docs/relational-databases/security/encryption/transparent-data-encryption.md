---
title: 透明数据加密 (TDE) | Microsoft Docs
description: 了解加密 SQL Server、Azure SQL 数据库和 Azure Synapse Analytics 数据的透明数据加密 (TDE)，也称为静态数据加密。
ms.custom: ''
ms.date: 05/09/2019
ms.prod: sql
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06288dc0bc6e16ab8928322005254f5fac1ddc61
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171479"
---
# <a name="transparent-data-encryption-tde"></a>透明数据加密 (TDE)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

透明数据加密 (TDE) 技术可以加密 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]和 [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)] 数据文件。 这种加密方式称为静态数据加密。

为了帮助保护数据库的安全，可以采取以下预防措施：

* 设计安全的系统。
* 对机密资产加密。
* 在数据库服务器外围构建防火墙。

但恶意方如果窃取了驱动器或备份磁带等物理介质，就可以还原或附加数据库并浏览其数据。

一种解决方案是加密数据库中的敏感数据，并使用证书保护用于加密数据的密钥。 此解决方案可以防止没有密钥的人使用这些数据。 但必须提前规划好此类保护。

TDE 对数据和日志文件进行实时 I/O 加密和解密。 加密使用的是数据库加密密钥 (DEK)。 数据库启动记录存储该密钥，供还原时使用。 DEK 是一种对称密钥。 它由服务器的 master 数据库存储的证书或 EKM 模块所保护的非对称密钥提供保护。

TDE 保护静态数据，也就是数据和日志文件。 它让你可以遵循许多法律、法规和各个行业建立的准则。 借助此功能，软件开发人员可以使用 AES 和 3DES 加密算法来加密数据，且无需更改现有的应用程序。

> [!IMPORTANT]
> TDE 不提供跨信道加密。 若要详细了解如何跨信道加密数据，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。
>
>**相关主题：**
>
> - [借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)
> - [Azure Synapse Analytics 中的透明数据加密 (TDE) 入门](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)
> - [将受 TDE 保护的数据库移到其他 SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)
> - [使用 EKM 在 SQL Server 上启用 TDE](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)
> - [使用具有 SQL 加密功能的 SQL Server 连接器](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [有关 TDE 的常见问题解答的 SQL Server 安全博客](/archive/blogs/sqlsecurity/feature-spotlight-transparent-data-encryption-tde)

## <a name="about-tde"></a>关于 TDE

数据库文件加密在页面级执行。 已加密数据库中的页在写入磁盘之前会进行加密，在读入内存时会进行解密。 TDE 不会增加已加密数据库的大小。

### <a name="information-applicable-to-sssds"></a>适用于 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]的信息

将 TDE 与 [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] V12 一起使用时，[!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 将自动为你创建存储在 master 数据库中的服务器级别证书。 若要在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]上移动 TDE 数据库，无需为移动操作解密数据库。 有关将 TDE 与 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]一起使用的详细信息，请参阅 [Azure SQL 数据库的透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)。

### <a name="information-applicable-to-ssnoversion"></a>适用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的信息

保证数据库的安全后，可以使用正确的证书来还原数据库。 有关证书的详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。

启用 TDE 后，立即备份证书及其关联的私钥。 如果证书不可用，或者如果在另一台服务器上还原或附加数据库，则需要备份证书和私钥。 否则，将无法打开数据库。

即使你在数据库上禁用了 TDE，也要保留加密证书。 虽然数据库未加密，但事务日志的某些部分仍可能受到保护。 在执行完整的数据库备份之前，某些操作可能还需要使用证书。

你仍可使用超过过期日期的证书来通过 TDE 加密和解密数据。

### <a name="encryption-hierarchy"></a>加密层次结构

下图显示了 TDE 加密体系结构。 在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]上使用 TDE 时，用户仅能配置数据库级项目（数据库加密密钥和 ALTER DATABASE 部分）。

![透明数据库加密体系结构](../../../relational-databases/security/encryption/media/tde-architecture.png)

## <a name="enable-tde"></a>启用 TDE

若要使用 TDE，请按以下步骤操作。

**适用于**： [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。

1. 创建主密钥。

1. 创建或获取由主密钥保护的证书。

1. 创建数据库加密密钥并使用此证书保护该密钥。

1. 将数据库设置为使用加密。

下面的示例演示如何使用安装在服务器上的 `MyServerCert` 证书来加密和解密 `AdventureWorks2012` 数据库。

```sql
USE master;
GO
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';
go
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';
go
USE AdventureWorks2012;
GO
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_128
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;
GO
ALTER DATABASE AdventureWorks2012
SET ENCRYPTION ON;
GO
```

加密和解密操作由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]安排在后台线程中执行。 若要查看这些操作的状态，可以使用本文后面部分显示的表中的目录视图和动态管理视图。

> [!CAUTION]
> 启用了 TDE 的数据库备份库文件也使用数据库加密密钥进行加密。 因此，在还原这些备份时，用于保护数据库加密密钥的证书必须是可用的。 因此，除了备份数据库之外，一定要注意维护好服务器证书的备份。 如果证书不再可用，就会造成数据丢失。
>
> 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。

## <a name="commands-and-functions"></a>命令和函数

对于接受 TDE 证书的以下语句，请使用数据库主密钥对其加密。 如果只用密码加密，这些语句将拒绝将它们视为加密程序。

> [!IMPORTANT]
> 如果在 TDE 使用证书后将证书设置为密码保护，那么数据库在重启后将无法访问。

下表提供了 TDE 命令和函数的链接和说明：

|命令或函数|目的|
|-------------------------|-------------|
|[CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|创建用于加密数据库的密钥| 
|[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|更改用于加密数据库的密钥|
|[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|删除用于加密数据库的密钥|
|[ALTER DATABASE SET 选项 (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|解释用于启用 TDE 的 ALTER DATABASE 选项|

## <a name="catalog-views-and-dynamic-management-views"></a>目录视图和动态管理视图

 下表显示了 TDE 目录视图和动态管理视图。

|目录视图或动态管理视图|目的|
|---------------------------------------------|-------------|
|[sys.databases (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|显示数据库信息的目录视图|
|[sys.certificates (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|用于显示数据库中的证书的目录视图|
|[sys.dm_database_encryption_keys (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|用于提供有关数据库加密密钥的信息以及加密状态的动态管理视图。|

## <a name="permissions"></a>权限

如上表中所述，TDE 的每项功能和每个命令都有各自的权限要求。

查看 TDE 所涉及的元数据要求拥有对证书的 VIEW DEFINITION 权限。

## <a name="considerations"></a>注意事项

当进行数据库加密操作的重新加密扫描时，将禁用对数据库的维护操作。 可以使用数据库的单用户模式设置来执行维护操作。 有关详细信息，请参阅 [将数据库设置为单用户模式](../../../relational-databases/databases/set-a-database-to-single-user-mode.md)。

使用 sys.dm_database_encryption_keys 动态管理视图来查找数据库加密状态。 有关详细信息，请参阅本文前面的“目录视图和动态管理视图”部分。

在 TDE 过程中，数据库中的所有文件和文件组都进行加密。 如果将数据库中的任何文件组标记为 READ ONLY，数据库加密操作将会失败。

如果在数据库镜像或日志发送中使用数据库，则两个数据库都将进行加密。 日志事务在它们之间发送时，都是加密的。

> [!IMPORTANT]
> 当数据库设置为加密时，将加密全文检索。 在 SQL Server 2008 之前的 SQL Server 版本中创建的这些索引由 SQL Server 2008 或更高版本导入数据库，并由 TDE 加密。

> [!TIP]
> 若要监视数据库的 TDE 状态更改，请使用 SQL Server Audit 或 SQL 数据库审核。 就 SQL Server 而言，在审核操作组 DATABASE_CHANGE_GROUP 下跟踪 TDE，可在 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)中找到该组。

## <a name="restrictions"></a>限制

在初始数据库加密、密钥更改或数据库解密期间，不允许执行下列操作：

- 从数据库中的文件组中删除文件

- 删除数据库

- 使数据库脱机

- 分离数据库

- 将数据库或文件组转换为 READ ONLY 状态

在执行 CREATE DATABASE ENCRYPTION KEY、ALTER DATABASE ENCRYPTION KEY、DROP DATABASE ENCRYPTION KEY 和 ALTER DATABASE...SET ENCRYPTION 语句期间，不允许执行下列操作：

- 从数据库中的文件组中删除文件

- 删除数据库

- 使数据库脱机

- 分离数据库

- 将数据库或文件组转换为 READ ONLY 状态

- 使用 ALTER DATABASE 命令

- 启动数据库或数据库文件备份

- 启动数据库或数据库文件还原

- 创建快照

下列操作或条件阻止执行 CREATE DATABASE ENCRYPTION KEY、ALTER DATABASE ENCRYPTION KEY、DROP DATABASE ENCRYPTION KEY 和 ALTER DATABASE...SET ENCRYPTION 语句：

- 数据库为只读或包含只读文件组。

- 正在运行 ALTER DATABASE 命令。

- 正在进行数据备份。

- 数据库处于脱机或还原状态。

- 正在创建快照。

- 正在运行数据库维护任务。

创建了数据库文件后，如果启用了 TDE，则无法执行即时文件初始化。

要使用非对称密钥加密数据库加密密钥，非对称密钥必须位于可扩展密钥管理提供程序上。

## <a name="tde-scan"></a>TDE 扫描

要在数据库上启用 TDE，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 必须执行加密扫描。 扫描将数据文件中的每个页面读入缓冲池，然后将加密页面写入磁盘。

为了让你对加密扫描有更多的控制权，[!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 引入了 TDE 扫描，其中包含暂停和恢复语法。 你可以在系统工作量大时或在业务关键时段暂停扫描，然后稍后再恢复扫描。

使用以下语法暂停 TDE 加密扫描：

```sql
ALTER DATABASE <db_name> SET ENCRYPTION SUSPEND;
```

同样，使用以下语法恢复 TDE 加密扫描：

```sql
ALTER DATABASE <db_name> SET ENCRYPTION RESUME;
```

encryption_scan_state 列已被添加到 sys.dm_database_encryption_keys 动态管理视图。 它显示加密扫描的当前状态。 还有一个名为 encryption_scan_modify_date 的新列，此列包含上次加密扫描状态更改的日期和时间。

如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例在其加密扫描被暂停时重启，则会在启动时的错误日志中记录一条消息。 该消息指示已暂停现有扫描。

## <a name="tde-and-transaction-logs"></a>TDE 和事务日志

让数据库使用 TDE，会删除当前虚拟事务日志的剩余部分。 删除操作会强制创建下一个事务日志。 这种行为保证了数据库被设置为加密后，日志中不会留有明文。

要查看日志文件加密的状态，请参阅 `sys.dm_database_encryption_keys` 视图中的 `encryption_state` 列，如以下示例所示：

```sql
USE AdventureWorks2012;
GO
/* The value 3 represents an encrypted state
   on the database and transaction logs. */
SELECT *
FROM sys.dm_database_encryption_keys
WHERE encryption_state = 3;
GO
```

有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 日志文件体系结构的详细信息，请参阅[事务日志 (SQL Server)](../../../relational-databases/logs/the-transaction-log-sql-server.md)。

在数据库加密密钥更改前，之前的数据库加密密钥将对写入到事务日志的所有数据进行加密。

如果更改数据库加密密钥两次，必须先执行日志备份，然后才能再次更改数据库加密密钥。

## <a name="tde-and-the-tempdb-system-database"></a>TDE 和 tempdb 系统数据库

如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例中的任何其他数据库是使用 TDE 加密的，则会加密 tempdb 系统数据库。 这种加密可能会对同一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的未加密数据库产生性能影响。 有关 tempdb 系统数据库的详细信息，请参阅 [tempdb 数据库](../../../relational-databases/databases/tempdb-database.md)。

## <a name="tde-and-replication"></a>TDE 和复制

复制不会以加密形式从启用了 TDE 的数据库中自动复制数据。 如果想保护分发和订阅服务器数据库，请单独启用 TDE。

快照复制可以将数据存储在未加密的中间文件（如 BCP 文件）中。 事务性复制和合并复制的初始数据分发也可以这样做。 在此类复制期间，可以启用加密来保护信道。

有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。

## <a name="tde-and-always-on"></a>TDE 和 Always On    
可[将加密数据库添加到 Always On 可用性组](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)。  

要对属于可用性组的数据库进行加密，在主要副本上创建[数据库加密密钥](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)之前，请在所有次要副本上创建主密钥和证书或非对称密钥 (EKM)。  

如果使用证书保护数据库加密密钥 (DEK)，请备份在主要副本上创建的[证书](../../../t-sql/statements/backup-certificate-transact-sql.md)，然后在所有次要副本上[根据文件创建证书](../../../t-sql/statements/create-certificate-transact-sql.md)，再在主要副本上创建数据库加密密钥。 

## <a name="tde-and-filestream-data"></a>TDE 和 FILESTREAM 数据

即使启用了 TDE，也不会加密 FILESTREAM 数据。

<a name="scan-suspend-resume"></a>

## <a name="remove-tde"></a>删除 TDE

使用 `ALTER DATABASE` 语句从数据库中删除加密。

```sql
ALTER DATABASE <db_name> SET ENCRYPTION OFF;
```

若要查看数据库的状态，请使用 [sys.dm_database_encryption_keys](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 动态管理视图。

请先等到解密完成，再使用 [DROP DATABASE ENCRYPTION KEY](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md) 删除数据库加密密钥。

> [!IMPORTANT]
> 将用于 TDE 的主密钥和证书备份到安全位置。 需要主密钥和证书，才能还原使用 TDE 加密数据库时进行的备份。 删除数据库加密密钥后，进行日志备份，然后对解密的数据库进行新的完整备份。 

## <a name="tde-and-buffer-pool-extension"></a>TDE 和缓冲池扩展

在使用 TDE 加密数据库时，与缓冲池扩展 (BPE) 相关的文件不会被加密。 对于这些文件，使用文件系统级别的加密工具，如 BitLocker 或 EFS。

## <a name="tde-and-in-memory-oltp"></a>TED 和内存中 OLTP

可在拥有内存中 OLTP 对象的数据库上启用 TDE。 在 [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] 和 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]中，如果启用 TDE，将对内存中 OLTP 日志记录和数据加密。 在 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 中，如果启用 TDE，将对内存中 OLTP 日志记录加密，但不对 MEMORY_OPTIMIZED_DATA 文件组中的文件加密。

## <a name="related-tasks"></a>相关任务

[将受 TDE 保护的数据库移到其他 SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
[使用 EKM 在 SQL Server 上启用 TDE](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
[使用 Azure Key Vault (SQL Server) 的可扩展密钥管理](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  

## <a name="related-content"></a>相关内容

[借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
[Azure Synapse Analytics 中的透明数据加密 (TDE) 入门](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql)  
[SQL Server 加密](../../../relational-databases/security/encryption/sql-server-encryption.md)  
[SQL Server 和数据库加密密钥（数据库引擎）](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

## <a name="see-also"></a>另请参阅

[SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[FILESTREAM (SQL Server)](../../../relational-databases/blob/filestream-sql-server.md)