---
title: 备份到 URL 的最佳做法和故障排除
description: 了解 SQL Server 备份和还原到 Azure Blob 存储的最佳做法和故障排除提示。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: de676bea-cec7-479d-891a-39ac8b85664f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6620e688dcd8094bbc5b27bfbb540a2e7d3b1585
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348836"
---
# <a name="sql-server-back-up-to-url-best-practices-and-troubleshooting"></a>从 SQL Server 备份到 URL 的最佳做法和故障排除

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  本主题介绍 SQL Server 备份和还原到 Azure Blob 服务的最佳做法和故障排除提示。  
  
 有关将 Azure Blob 存储服务用于 SQL Server 备份或还原操作的详细信息，请参阅：  
  
-   [使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
-   [教程：将 SQL Server 备份和还原到 Azure Blob 存储服务](../../relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="managing-backups"></a><a name="managing-backups-mb1"></a> 管理备份  
 下表列出了管理备份的一般建议：  
  
-   建议为每个备份使用唯一文件名以防止意外覆盖 blob。  
  
-   创建容器时，建议将访问级别设置为 **“私有”** ，这样只有可以提供所需的身份验证信息的用户或帐户可以在容器中读取或写入 blob。  
  
-   对于在 Azure 虚拟机中运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，请使用与虚拟机相同区域中的存储帐户，以免产生区域之间的数据传输成本。 使用同一区域还可以确保备份和还原操作具有最佳性能。  
  
-   失败的备份活动可能导致无效的备份文件。 我们建议定期标识失败的备份和删除 blob 文件。 有关详细信息，请参阅 [Deleting Backup Blob Files with Active Leases](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md)  
  
-   在备份期间使用 `WITH COMPRESSION` 选项可以最大程度降低存储成本和存储事务成本。 它也会减少完成备份过程所需的时间。  

- 按照[从 SQL Server 备份到 URL](./sql-server-backup-to-url.md) 中的建议，设置 `MAXTRANSFERSIZE` 和 `BLOCKSIZE` 参数。

- SQL Server 对于使用的存储冗余类型不可知。 每种存储冗余（LRS、ZRS、GRS、RA-GRS、RA-GZRS 等）都支持备份到页 blob 和块 blob。
  
## <a name="handling-large-files"></a>处理大型文件  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份操作使用多个线程来优化与 Azure Blob 存储服务的数据传输。  但是性能取决于各种因素，如 ISV 带宽和数据库的大小。 如果您计划从内部 SQL Server 数据库备份大型数据库或文件组，建议您首先执行某些吞吐量测试。 Azure [SLA for Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_0/) 限制了 blob 的最大处理时间，你需要考虑这个限制。  
  
-   按照[管理备份](#managing-backups-mb1)部分的建议使用 `WITH COMPRESSION` 选项，在备份大型文件时这一点非常重要。  
  
## <a name="troubleshooting-backup-to-or-restore-from-url"></a>备份到 URL 或从 URL 还原故障排除  

以下内容提供了在备份到 Azure Blob 存储服务或从中还原时出现问题的一些快速解决方法。  
  
要避免由于不支持的选项或限制导致的错误，请参阅 [使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) 一文，查看限制列表以及 BACKUP 和 RESTORE 命令的支持信息。  

初始化失败 

并行备份到同一 blob 导致一个备份失败，发生 **“初始化失败”** 错误。 

如果使用的是页 Blob（例如 `BACKUP... TO URL... WITH CREDENTIAL`），请使用以下错误日志帮助解决备份错误：  
  
设置跟踪标志 3051 以启用记录到具有以下格式的特定错误日志：  
  
`BackupToUrl-\<instname>-\<dbname>-action-\<PID>.log` 其中，`\<action>` 为以下值之一：  
  
-   **DB**  
-   **FILELISTONLY**  
-   **LABELONLY**  
-   **HEADERONLY**  
-   **VERIFYONLY**  
  
还可以查看 Windows 事件日志（位于应用程序日志之下，名为“`SQLBackupToUrl`”），查找相关信息。  

**由于出现 I/O 设备错误，因此无法执行该请求。**

在备份大型数据库时，请考虑 COMPRESSION、MAXTRANSFERSIZE、BLOCKSIZE 和多个 URL 参数。  请参阅[将 VLDB 备份到 Azure Blob 存储](/archive/blogs/sqlcat/backing-up-a-vldb-to-azure-blob-storage)

错误： 

```console
Msg 3202, Level 16, State 1, Line 1
Write on "https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak" failed: 
1117(The request could not be performed because of an I/O device error.)
Msg 3013, Level 16, State 1, Line 1
BACKUP DATABASE is terminating abnormally.
```

示例解析： 

```sql  
BACKUP DATABASE TestDb
TO URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak',
URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_1.bak',
URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_2.bak'
WITH COMPRESSION, MAXTRANSFERSIZE = 4194304, BLOCKSIZE = 65536;  
```  

设备上的消息文件标记未对齐。

从压缩备份中还原时，您可能看到以下错误：  
  
```
SqlException 3284 occurred. Severity: 16 State: 5  
Message Filemark on device 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak' is not aligned.
Reissue the Restore statement with the same block size used to create the backupset: '65536' looks like a possible value.  
```
  
要解决此错误，请重新发布指定了 BLOCKSIZE = 65536 的 RESTORE 语句   。  
  
**失败的备份活动可能导致 blob 产生活动租约。**

由于 blob 具有活动租约，备份期间出错：`Failed backup activity can result in blobs with active leases.`  

如果重新尝试执行备份语句，备份操作可能失败，出现类似于以下的错误：  

```
Backup to URL received an exception from the remote endpoint. Exception Message: 
The remote server returned an error: (412) There is currently a lease on the blob and no lease ID was specified in the request. 
```

如果尝试对具有活动租约的备份 blob 文件执行还原语句，则还原操作失败，出现类似于以下的错误：  
  
`Exception Message: The remote server returned an error: (409) Conflict..`  
  
发生这种错误时，需要删除 blob 文件。 有关此情形和如何解决此问题的详细信息，请参阅 [Deleting Backup Blob Files with Active Leases](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md)  

操作系统错误 50：不支持该请求
 
备份数据库时，可能会由于以下原因而出现错误 `Operating system error 50(The request is not supported)`： 

   - 指定的存储帐户不是常规用途 V1/V2。
   - SAS 令牌超过 128 个字符。
   - 创建凭据时，SAS 令牌的令牌开头有一个 `?` 符号。 如果是，请删除它。
   - 当前连接无法使用存储资源管理器或 SQL Server Management Studio (SSMS) 从当前计算机连接到存储帐户。 
   - 分配给 SAS 令牌的策略已过期。 使用 Azure 存储资源管理器创建新策略，并使用该策略创建新的 SAS 令牌，或者更改凭据并再次尝试备份。 

**身份验证错误**
  
`WITH CREDENTIAL` 是一个新选项，在备份到 Azure Blob 存储服务或从中还原时需要该选项。

与凭据有关的失败可能包括：`The credential specified in the **BACKUP** or **RESTORE** command does not exist. `

要避免此问题，如果备份语句中没有指定凭据，可以使用 T-SQL 语句来创建凭据。 以下是您可以使用的一个示例：  

  
```sql  
IF NOT EXISTS  
(SELECT * FROM sys.credentials   
WHERE credential_identity = 'mycredential')  
CREATE CREDENTIAL <credential name> WITH IDENTITY = 'mystorageaccount'  
, SECRET = '<storage access key>' ;  
```  
  
凭据存在但是用于运行备份命令的登录帐户没有访问凭据的权限。 使用角色为 db_backupoperator 且拥有“更改任意凭据”权限的登录帐户 。  
  
验证存储帐户名称和密钥值。 在凭据中存储的信息必须与你在备份和还原操作中使用的 Azure 存储帐户的属性值匹配。  
  

400（错误的请求）错误

使用 SQL Server 2012，执行类似于如下的备份时可能会遇到错误：

```
Backup to URL received an exception from the remote endpoint. Exception Message: 
The remote server returned an error: (400) Bad Request..
```

这是由 Azure 存储帐户支持的 TLS 版本引起的。 更改支持的 TLS 版本或使用 [KB4017023](https://support.microsoft.com/en-us/topic/kb4017023-sql-server-2012-2014-or-2016-backup-to-microsoft-azure-blob-storage-service-url-isn-t-compatible-for-tls-1-2-e9ef6124-fc05-8128-86bc-f4f4f5ff2b78) 中列出的解决方法。


## <a name="proxy-errors"></a>代理错误  
 如果您使用代理服务器访问 Internet，可能会发现以下问题：  
  
 代理服务器连接限制  
  
 代理服务器可能具有限制每分钟连接次数的设置。 “备份到 URL”进程是一个多线程进程，因此可能超过此限制。 如果出现此情况，代理服务器将终止连接。 若要解决此问题，请更改代理设置，使 SQL Server 不使用该代理。 下面是一些您可能在错误日志中看到的类型或错误消息的示例：  
  
```console
Write on "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak" failed: Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
```console
A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Error could not be gathered from Remote Endpoint.  
  
Msg 3013, Level 16, State 1, Line 2  
  
BACKUP DATABASE is terminating abnormally.  
```

```console
BackupIoRequest::ReportIoError: write failure on backup device https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak'. Operating system error Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
如果使用跟踪标志 3051 打开详细日志记录，您还可能在日志中看到以下消息：  
  
`HTTP status code 502, HTTP Status Message Proxy Error (The number of HTTP requests per minute exceeded the configured limit. Contact your ISA Server administrator.)` 
  
 **未选择默认代理设置：**  
  
有时候，若不选用默认设置，会导致代理身份验证错误，如下所示：
 
 `A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Backup to URL received an exception from the remote endpoint. Exception Message: The remote server returned an error: (407)* **Proxy Authentication Required.`  
  
若要解决此问题，请使用以下步骤创建一个配置文件，以允许“备份到 URL”进程使用默认代理设置：  
  
1.  使用以下 xml 内容创建一个名为 `BackuptoURL.exe.config` 的配置文件：  
  
    ```xml  
    <?xml version ="1.0"?>  
    <configuration>   
                    <system.net>   
                                    <defaultProxy enabled="true" useDefaultCredentials="true">   
                                                    <proxy usesystemdefault="true" />   
                                    </defaultProxy>   
                    </system.net>  
    </configuration>  
    ```  
  
2.  将该配置文件置于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 Binn 文件夹中。 例如，如果我的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装在计算机的 C 驱动器上，可将该配置文件置在 `C:\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\Binn` 中。  
  
## <a name="see-also"></a>另请参阅  
 [从 Microsoft Azure 中存储的备份还原](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)
