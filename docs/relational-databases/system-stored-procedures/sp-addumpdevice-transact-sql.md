---
description: sp_addumpdevice (Transact-SQL)
title: sp_addumpdevice (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addumpdevice_TSQL
- sp_addumpdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], defining
- sp_addumpdevice
ms.assetid: c2d2ae49-0808-46d8-8444-db69a69d0ec3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 68bad4307f35aebe34c7c2617881c5dc56c0a4ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198397"
---
# <a name="sp_addumpdevice-transact-sql"></a>sp_addumpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  

将备份设备添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addumpdevice [ @devtype = ] 'device_type'   
    , [ @logicalname = ] 'logical_name'   
    , [ @physicalname = ] 'physical_name'  
      [ , { [ @cntrltype = ] controller_type |  
          [ @devstatus = ] 'device_status' }  
      ]  
```  
  
## <a name="arguments"></a>参数  
`[ @devtype = ] 'device_type'` 备份设备的类型。 *device_type* 是 **varchar (20)**，无默认值，并且可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**磁盘**|硬盘文件作为备份设备。|  
|**磁盘**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 支持的任何磁带设备。<br /><br /> 注意：在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将不再支持磁带备份设备。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。|  
  
`[ @logicalname = ] 'logical_name'` Backup 和 RESTORE 语句中使用的备份设备的逻辑名称。 *logical_name* **sysname**，无默认值，且不能为 NULL。  
  
`[ @physicalname = ] 'physical_name'` 备份设备的物理名称。 物理名称必须遵从操作系统文件名规则或网络设备的通用命名约定，并且必须包含完整路径。 *physical_name* 为 **nvarchar (260)**，没有默认值，且不能为 NULL。  
  
 在远程网络位置上创建备份设备时，请确保启动[!INCLUDE[ssDE](../../includes/ssde-md.md)]时所用的名称对远程计算机有相应的写权限。  
  
 如果添加磁带设备，则此参数必须为 Windows 分配给本地磁带设备的物理名称;例如，计算机上的第一个磁带设备的 **\\ \\ .\TAPE0** 。 磁带设备必须连接到服务器计算机上，不能远程使用。 如果名称包含非字母数字的字符，请用引号将其引起来。  
  
> [!NOTE]  
>  此过程会在目录中输入指定的物理名称。 此过程不会尝试访问或创建设备。  
  
`[ @cntrltype = ] 'controller_type'` 弃用. 如果指定该选项，则忽略此参数。 支持它完全是为了向后兼容。 **Sp_addumpdevice** 的新用法应省略此参数。  
  
`[ @devstatus = ] 'device_status'` 弃用. 如果指定该选项，则忽略此参数。 支持它完全是为了向后兼容。 **Sp_addumpdevice** 的新用法应省略此参数。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **sp_addumpdevice** 将备份设备添加到 **sys.backup_devices** 目录视图。 然后便可以在 BACKUP 和 RESTORE 语句中逻辑引用该设备。 **sp_addumpdevice** 不执行对物理设备的任何访问。 只有在执行 BACKUP 或 RESTORE 语句后才会访问指定的设备。 创建一个逻辑备份设备可简化 BACKUP 和 RESTORE 语句，在这种情况下指定设备名称将代替使用 "TAPE =" 或 "DISK =" 子句指定设备路径。  
  
 所有权和权限问题可能干扰磁盘或文件备份设备的使用。 请确保已将相应的文件权限授予用于启动[!INCLUDE[ssDE](../../includes/ssde-md.md)]的 Windows 帐户。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)]支持将磁带备份到 Windows 支持的磁带设备上。 有关 Windows 支持的磁带设备的详细信息，请参阅 Windows 的硬件兼容性列表。 若要查看计算机上可用的磁带设备，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
 对于特定的磁带机，请仅使用驱动器厂商建议的推荐磁带。 如果您使用的是数字音频磁带 (DAT) 驱动器，请使用计算机级的 DAT 磁带（数字数据存储 (DDS)）。  
  
 无法在事务中执行 **sp_addumpdevice** 。  
  
 若要删除设备，请使用 [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) 或[SQL Server Management Studio](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)。  
  
## <a name="permissions"></a>权限  
 要求具有 **diskadmin** 固定服务器角色中的成员身份。  
  
 要求拥有写入磁盘的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-a-disk-dump-device"></a>A. 添加磁盘转储设备  
 下面的示例添加了一个名为 `mydiskdump` 的磁盘备份设备，其物理名称为 `c:\dump\dump1.bak`。  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak';  
```  
  
### <a name="b-adding-a-network-disk-backup-device"></a>B. 添加网络磁盘备份设备  
 下面的示例显示了添加名为 `networkdevice` 的远程磁盘备份设备的过程。 用于启动[!INCLUDE[ssDE](../../includes/ssde-md.md)]的名称必须对该远程文件 (`\\<servername>\<sharename>\<path>\<filename>.bak`) 拥有权限。  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'networkdevice',  
    '\\<servername>\<sharename>\<path>\<filename>.bak';  
```  
  
### <a name="c-adding-a-tape-backup-device"></a>C. 添加磁带备份设备  
 下面的示例添加物理名称为 `tapedump1` 的 `\\.\tape0` 设备。  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'tapedump1', '\\.\tape0';  
```  
  
### <a name="d-backing-up-to-a-logical-backup-device"></a>D. 备份到逻辑备份设备  
 以下示例为某备份磁盘文件创建了名为 `AdvWorksData` 的逻辑备份设备。 该示例随后会将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库备份到此逻辑备份设备。  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\AdvWorksData.bak';  
GO  
BACKUP DATABASE AdventureWorks2012   
 TO AdvWorksData  
   WITH FORMAT;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sp_dropdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [sys.backup_devices (Transact-SQL)](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
