---
title: 备份和还原 Linux 上的 SQL Server 数据库
description: 了解如何备份和还原 Linux 上的 SQL Server 数据库。 此外，了解如何使用 SQL Server Management Studio (SSMS) 进行备份和还原。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 11/14/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: d30090fb-889f-466e-b793-5f284fccc4e6
ms.openlocfilehash: 92210c7d111f44a965383a8c2099068b2030227e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338291"
---
# <a name="backup-and-restore-sql-server-databases-on-linux"></a>备份和还原 Linux 上的 SQL Server 数据库

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

可以使用多个不同的选项备份 Linux 上的 SQL Server 2017 中的数据库。 在 Linux 服务器上，可以使用 sqlcmd 连接到 SQL Server 并进行备份。 如果是 Windows，则可以在连接到 Linux 上的 SQL Server 后通过用户界面进行备份。 各平台间的备份功能都是相同的。 例如，可以将数据库备份到本地、远程驱动器或 [Microsoft Azure Blob 存储服务](../relational-databases/backup-restore/sql-server-backup-to-url.md)中。

> [!IMPORTANT]
> Linux 上的 SQL Server 仅支持使用块 blob 备份到 Azure Blob 存储。 使用存储密钥进行备份和还原将导致使用页 blob，而这不受支持。 请改为使用共享访问签名。 有关块 blob 和页 blob 的信息，请参阅[备份到块 blob 和页 blob](../relational-databases/backup-restore/sql-server-backup-to-url.md#blockbloborpageblob)。

## <a name="backup-a-database"></a>备份数据库

下例使用 sqlcmd 连接到本地 SQL Server 实例，并对名为 `demodb` 的用户数据库进行完整备份。

```bash
sqlcmd -S localhost -U SA -Q "BACKUP DATABASE [demodb] TO DISK = N'/var/opt/mssql/data/demodb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

运行该命令后，SQL Server 将提示输入密码。 输入密码后，shell 将返回备份进度结果。 例如：

```
Password:
10 percent processed.
21 percent processed.
32 percent processed.
40 percent processed.
51 percent processed.
61 percent processed.
72 percent processed.
80 percent processed.
91 percent processed.
Processed 296 pages for database 'demodb', file 'demodb' on file 1.
100 percent processed.
Processed 2 pages for database 'demodb', file 'demodb_log' on file 1.
BACKUP DATABASE successfully processed 298 pages in 0.064 seconds (36.376 MB/sec).
```

### <a name="backup-the-transaction-log"></a>备份事务日志

如果数据库处于完整恢复模式，还可以进行事务日志备份以获得更精细的还原选项。 下例使用 sqlcmd 连接到本地 SQL Server 实例，并对事务日志进行备份。

```bash
sqlcmd -S localhost -U SA -Q "BACKUP LOG [demodb] TO DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak' WITH NOFORMAT, NOINIT, NAME = N'demodb_LogBackup', NOSKIP, NOREWIND, NOUNLOAD, STATS = 5"
```

## <a name="restore-a-database"></a>还原数据库

下例使用 sqlcmd 连接到 SQL Server 本地实例，并对 demodb 数据库进行还原。 请注意，`NORECOVERY` 选项用于其他日志文件备份的还原。 如果不打算还原其他日志文件，请删除 `NORECOVERY` 选项。

```bash
sqlcmd -S localhost -U SA -Q "RESTORE DATABASE [demodb] FROM DISK = N'/var/opt/mssql/data/demodb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, NORECOVERY, STATS = 5"
```

> [!TIP]
> 如果意外使用 NORECOVERY，但没有其他日志文件备份，请运行不含其他参数的 `RESTORE DATABASE demodb` 命令。 这将完成还原，并使数据库正常运行。

### <a name="restore-the-transaction-log"></a>还原事务日志

以下命令将还原以前的事务日志备份。

```bash
sqlcmd -S localhost -U SA -Q "RESTORE LOG demodb FROM DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak'"
```

## <a name="backup-and-restore-with-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 备份和还原

可以从 Windows 计算机使用 SSMS 连接到 Linux 数据库并通过用户界面进行备份。

>[!NOTE] 
> 使用最新版本的 SSMS 连接到 SQL Server。 若要下载和安装最新版本，请参阅[下载 SSMS](../ssms/download-sql-server-management-studio-ssms.md)。 有关如何使用 SSMS 的详细信息，请参阅[使用 SSMS 管理 Linux 上的 SQL Server](sql-server-linux-manage-ssms.md)。

下列步骤将引导你使用 SSMS 完成备份。 

1. 启动 SSMS 并连接到 Linux 上的 SQL Server 2017 中的服务器。

1. 在“对象资源管理器”中，右键单击数据库，单击“任务”，然后单击“备份...”。

1. 在“备份数据库”对话框中，验证参数和选项，然后单击“确定”。
 
SQL Server 将完成数据库备份。

### <a name="restore-with-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 还原 

以下步骤将引导你使用 SSMS 完成数据库还原。

1. 在 SSMS 中，右键单击“数据库”，然后单击“还原数据库...”。 

1. 单击“源”下的“设备:”，然后单击省略号 (...)。

1. 查找数据库备份文件，然后单击“确定”。 

1. 在“还原计划”下，验证备份文件和设置。 单击“确定”。 

1. SQL Server 将还原数据库。 

## <a name="see-also"></a>另请参阅

* [创建完整数据库备份 (SQL Server)](../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
* [备份事务日志 (SQL Server)](../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)
* [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)
* [SQL Server 备份到 URL](../relational-databases/backup-restore/sql-server-backup-to-url.md)
