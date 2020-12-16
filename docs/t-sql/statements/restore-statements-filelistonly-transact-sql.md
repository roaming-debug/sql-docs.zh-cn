---
description: RESTORE 语句 - FILELISTONLY (Transact-SQL)
title: RESTORE FILELISTONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RESTORE FILELISTONLY
- RESTORE_FILELISTONLY_TSQL
- FILELISTONLY
- FILELISTONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backups [SQL Server], file lists
- RESTORE FILELISTONLY statement
- listing backed up files
ms.assetid: 0b4b4d11-eb9d-4f3e-9629-6c79cec7a81a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 4ad3fac6d16fd8c033f4dc260b048a0d0baa421c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463995"
---
# <a name="restore-statements---filelistonly-transact-sql"></a>RESTORE 语句 - FILELISTONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]


  返回由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的备份集内包含的数据库和日志文件列表组成的结果集。  

> [!NOTE]  
>  有关参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
RESTORE FILELISTONLY   
FROM <backup_device>   
[ WITH   
 {  
--Backup Set Options  
   FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
```  

> [!NOTE] 
> URL 是用于为 Microsoft Azure Blob 存储指定位置和文件名的格式，从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 开始受支持。 虽然 Microsoft Azure 存储是一种服务，不过实现与磁盘和磁带类似，从而可为所有三种设备提供一致且无缝的还原体验。

## <a name="arguments"></a>参数  
 有关 RESTORE FILELISTONLY 参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
## <a name="result-sets"></a>结果集  
 客户端可以使用 RESTORE FILELISTONLY 获得备份集中所含文件的列表。 此信息以结果集的形式返回，在结果集中每个文件占一行。  
  
|列名称|数据类型|说明|  
|-|-|-|  
|LogicalName|**nvarchar(128)**|文件的逻辑名称。|  
|PhysicalName|**nvarchar(260)**|文件的物理名称或操作系统名称。|  
|类型|**char(1)**|文件的类型，其中包括：<br /><br /> L = Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件<br /><br /> D = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件<br /><br /> F = 全文目录<br /><br /> S = FileStream、FileTable 或 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 容器|  
|FileGroupName|**nvarchar(128)** NULL|包含文件的文件组的名称。|  
|大小|**numeric(20,0)**|当前大小（以字节为单位）。|  
|MaxSize|**numeric(20,0)**|允许的最大大小（以字节为单位）。|  
|FileID|**bigint**|文件标识符，在数据库中唯一。|  
|CreateLSN|**numeric(25,0)**|创建文件时的日志序列号。|  
|DropLSN|**numeric(25,0)** NULL|删除文件时的日志序列号。 如果文件尚未删除，该值为 NULL。|  
|UniqueID|**uniqueidentifier**|文件的全局唯一标识符。|  
|ReadOnlyLSN|**numeric(25,0) NULL**|包含该文件的文件组从读写属性更改为只读属性（最新更改）时的日志序列号。|  
|ReadWriteLSN|**numeric(25,0)** NULL|包含该文件的文件组从只读属性更改为读写属性（最新更改）时的日志序列号。|  
|BackupSizeInBytes|**bigint**|此文件的备份的大小（字节）。|  
|SourceBlockSize|**int**|包含文件的物理设备（并非备份设备）的块大小（以字节为单位）。|  
|FileGroupID|**int**|文件组的 ID。|  
|LogGroupGUID|**uniqueidentifier** NULL|NULL。|  
|DifferentialBaseLSN|**numeric(25,0)** NULL|对于差异备份，日志序列号大于或等于 DifferentialBaseLSN 的更改都包含在差异中。<br /><br /> 对于其他备份类型，该值为 NULL。|  
|DifferentialBaseGUID|**uniqueidentifier** NULL|对于差异备份，该值是差异基准的唯一标识符。<br /><br /> 对于其他备份类型，该值为 NULL。|  
|IsReadOnly|**bit**|1 = 文件为只读文件。|  
|IsPresent|**bit**|1 = 文件出现在备份中。|  
|TDEThumbprint|**varbinary(32)** NULL|显示数据库加密密钥的指纹。 加密程序的指纹是带有加密密钥的证书的 SHA-1 哈希。 有关数据库加密的信息，请参阅[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。|  
|SnapshotURL|**nvarchar(360)** NULL|FILE_SNAPSHOT 备份中包含的数据库文件的 Azure 快照的 URL。 如果没有 FILE_SNAPSHOT 备份，则返回 NULL。|  
  
## <a name="security"></a>安全性  
 在备份时，可以根据需要为介质集、备份集或这两者指定密码。 如果已经在介质集或备份集上定义了密码，则必须在 RESTORE 语句中指定正确的密码。 这些密码可防止未经授权而使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行还原操作以及向介质追加备份集。 但是，密码不会阻止使用 BACKUP 语句的 FORMAT 选项覆盖介质。  
  
> [!IMPORTANT]  
>  此密码提供的安全性较低。 它旨在防止经过授权的用户或未经授权的用户使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行不正确的还原操作。 但是不能防止通过其他方式或通过替换密码来读取备份数据。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]保护备份的最佳做法是将备份磁带存储在安全的位置，或者备份到由适当的访问控制列表 (ACL) 保护的磁盘文件。 ACL 应设置在创建备份的根目录下。  
  
### <a name="permissions"></a>权限  
 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，获取有关备份集或备份设备的信息需要具有 CREATE DATABASE 权限。 有关详细信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 下面的示例从名为 `AdventureWorksBackups` 的备份设备中返回信息。 该示例使用 `FILE` 选项指定设备中的第二个备份集。  
  
```sql  
RESTORE FILELISTONLY FROM AdventureWorksBackups   
   WITH FILE=2;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
