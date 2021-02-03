---
description: RESTORE 语句 - REWINDONLY (Transact-SQL)
title: RESTORE REWINDONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE_REWINDONLY_TSQL
- RESTORE REWINDONLY
dev_langs:
- TSQL
helpviewer_keywords:
- closing backup devices
- backup devices [SQL Server], rewinding
- media [SQL Server]
- open back devices
- rewinding backup devices
- RESTORE REWINDONLY statement
ms.assetid: 7f825b40-2264-4608-9809-590d0f09d882
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc5e119a9f3bad173ca72dc7b2905a82481fa1ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186611"
---
# <a name="restore-statements---rewindonly-transact-sql"></a>RESTORE 语句 - REWINDONLY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  倒带并关闭指定的磁带设备，这些设备在以 NOREWIND 选项执行 BACKUP 或 RESTORE 语句后保持打开状态。 仅磁带设备支持此命令。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
RESTORE REWINDONLY   
FROM <backup_device> [ ,...n ]  
[ WITH {UNLOAD | NOUNLOAD}]  
}   
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | TAPE = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **\<backup_device> ::=** 
  
 指定要用于还原操作的逻辑或物理备份设备。  
  
 { *logical_backup_device_name* |  **@** _logical\_backup\_device\_name\_var_ }  
 由 sp_addumpdevice 创建的、用于还原数据库的备份设备的逻辑名称，该名称必须符合有关标识符的规则。 如果作为变量 ( **@** _logical\_backup\_device\_name\_var_) 提供，则可以将该备份设备名称指定为字符串常量 ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) 或字符字符串数据类型（ntext 或 text 数据类型除外）的变量。  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 允许从指定的磁盘或磁带设备还原备份。 应以设备的实际名称（例如，完整路径和文件名）指定磁盘和磁带的设备类型：DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL\BACKUP\Mybackup.bak' 或 TAPE = '\\\\.\TAPE0'。 如果指定为变量 (@physical\_backup\_device\_name\_var)，则可以将该设备名称指定为字符串常量 (@physical\_backup\_device\_name\_var = 'physical_backup_device_name') 或字符字符串数据类型（ntext 或 text 数据类型除外）的变量 。  
  
 如果使用的是具有 UNC 名称（必须包含计算机名称）的网络服务器，请指定磁盘的设备类型。 有关如何使用 UNC 名称的详细信息，请参阅[备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
 运行 Microsoft SQL Server 所用的帐户必须对远程计算机或网络服务器拥有 READ 访问权，这样才能执行 RESTORE 操作。  
  
 *n*  
 一个占位符，用于指示可以指定多个备份设备和逻辑备份设备。 备份设备或逻辑备份设备最多可以有 64 个。  
  
 还原顺序是否要求备份设备的数量与创建备份所属的介质集时所用的数量相同，这取决于还原是脱机还原还是联机还原。 脱机还原允许还原所用的设备少于创建备份时所用的设备。 联机还原要求使用备份的所有备份设备。 使用较少的设备进行还原将会失败。  
  
 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
> [!NOTE]  
>  从镜像介质集中还原备份时，对于每个介质簇，只能指定一个镜像服务器。 但是，在出现错误时，拥有其他镜像服务器可以快速解决某些还原问题。 您可以使用其他镜像服务器中的相应卷替换损坏的介质卷。 请注意，对于脱机还原来说，虽然您可以使用比介质簇少的设备进行还原，但每个簇仅处理一次。  
  
 **WITH 选项**  
  
 UNLOAD  
 指定 RESTORE 完成后自动倒带并卸载磁带。 启动新用户会话时，其默认设置为 UNLOAD。 如果不指定 NOUNLOAD，设置将始终为 UNLOAD。 此选项只用于磁带设备。 如果 RESTORE 使用非磁带设备，则将忽略此选项。  
  
 NOUNLOAD  
 指定在执行 RESTORE 后不从磁带机中自动卸载磁带。 始终设置为 NOUNLOAD，直到指定 UNLOAD 为止。  
  
## <a name="general-remarks"></a>一般备注  
 RESTORE REWINDONLY 是 RESTORE LABELONLY FROM TAPE = \<name> WITH REWIND 的替代命令。 可以从 [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) 动态管理视图中查看打开的磁带驱动器列表。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 任何用户都可以使用 RESTORE REWINDONLY。  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  

