---
description: RESTORE 语句 - VERIFYONLY (Transact-SQL)
title: RESTORE VERIFYONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 00926a8ee1244112d1fb5a01273e90d22a5753a7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463967"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>RESTORE 语句 - VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  验证备份但不还原备份，检查备份集是否完整以及整个备份是否可读。 但是，RESTORE VERIFYONLY 不尝试验证备份卷中的数据结构。 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，RESTORE VERIFYONLY 得到了增强以对数据进行附加检查，从而提高检测到错误的可能性。 其目标是尽可能接近实际的还原操作。 有关详细信息，请参阅“备注”部分。  

 如果备份有效，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 会返回一条成功消息。  
  
> [!NOTE]  
>  有关参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
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
 有关 RESTORE VERIFYONLY 参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
## <a name="general-remarks"></a>一般备注  
 介质集或备份集必须包含最低限度的正确信息，才能被解释为 Microsoft Tape Format。 如果没有这些信息，RESTORE VERIFYONLY 将停止，并且指示备份格式无效。  
  
 RESTORE VERIFYONLY 执行下列检查：  
  
-   备份集是否完整以及所有卷是否可读。  
  
-   数据库页中的一些标头字段，例如页 ID（就如同要写入数据一样）。  
  
-   校验和（如果介质中提供的话）。  
  
-   目标设备中是否有足够的空间。  
  
> [!NOTE]  
>  RESTORE VERIFYONLY 不对数据库快照进行检查。 要在恢复操作之前验证数据库快照，可以运行 DBCC CHECKDB。  
  
> [!NOTE]  
>  借助快照备份，RESTORE VERIFYONLY 可确认快照是否在备份文件中指定的位置处存在。 快照备份是 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中的新功能。 有关快照备份的详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
## <a name="security"></a>安全性  
 在备份时，可以根据需要为介质集、备份集或这两者指定密码。 如果已经在介质集或备份集上定义了密码，则必须在 RESTORE 语句中指定正确的密码。 这些密码可防止未经授权而使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行还原操作以及向介质追加备份集。 但是，密码不会阻止使用 BACKUP 语句的 FORMAT 选项覆盖介质。  
  
> [!IMPORTANT]  
>  此密码提供的安全性较低。 它旨在防止经过授权的用户或未经授权的用户使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行不正确的还原操作。 但是不能防止通过其他方式或通过替换密码来读取备份数据。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 保护备份的最佳做法是将备份磁带存储在安全位置，或者备份到由适当的访问控制列表 (ACL) 保护的磁盘文件。 ACL 应设置在创建备份的根目录下。  
  
### <a name="permissions"></a>权限  
 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，获取有关备份集或备份设备的信息需要具有 CREATE DATABASE 权限。 有关详细信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
 
## <a name="examples"></a>示例  
 以下示例从磁盘验证备份。
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
