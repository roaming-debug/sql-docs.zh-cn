---
title: backupmediaset (Transact-sql) |Microsoft Docs
description: Backupmediaset 的引用，每个备份介质集在该表中各占一行。
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupmediaset
- backupmediaset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], backupmediaset system table
- backupmediaset system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: 10b4d744e7ef4e0d11a9788580ea7f8c5a67bd1f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091567"
---
# <a name="backupmediaset-transact-sql"></a>backupmediaset (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

每个备份介质集在表中占一行。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|唯一介质集标识号。 标识，主键。|  
|**media_uuid**|**uniqueidentifier**|介质集的 UUID。 所有 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 介质集都具有 UUID。<br /><br /> 但对于的早期版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，如果介质集只包含一个介质簇，则 **media_uuid** 列可能为 NULL (**media_family_count** 为 1) 。|  
|**media_family_count**|**tinyint**|媒体集中的媒体簇数。 可以为 NULL。|  
|name|**nvarchar(128)**|介质集的名称。 可以为 NULL。<br /><br /> 有关详细信息，请参阅 MEDIANAME and MEDIADESCRIPTION in [BACKUP &#40;transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md)。|  
|description|**nvarchar(255)**|介质集的文本化说明。 可以为 NULL。<br /><br /> 有关详细信息，请参阅 MEDIANAME and MEDIADESCRIPTION in [BACKUP &#40;transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md)。|  
|**software_name**|**nvarchar(128)**|写入介质标签的备份软件名称。 可以为 NULL。|  
|**software_vendor_id**|**int**|写入备份介质标签的软件供应商标识号。 可以为 NULL。<br /><br /> 的值 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为十六进制0x1200。|  
|**MTF_major_version**|**tinyint**|用于生成该介质集的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 磁带格式的主要版本号。 可以为 NULL。|  
|**mirror_count**|**tinyint**|介质集中的镜像数。|  
|**is_password_protected**|**bit**|指定介质集是否受到密码保护：<br /><br /> 0 = 未受到保护<br /><br /> 1 = 受到保护|  
|**is_compressed**|**bit**|备份是否已压缩：<br /><br /> 0 = 未压缩<br /><br /> 1 = 压缩<br /><br /> 在 **msdb** 升级期间，此值设置为 NULL。 表示未压缩的备份。|  
|**is_encrypted**|**小段**|备份是否已加密：<br /><br /> 0 = 未加密<br /><br /> 1 = 已加密|  
  
## <a name="remarks"></a>备注  
 通过 VERIFYONLY 从 *BACKUP_DEVICE* 还原将用介质集标头中的相应值填充 **backupmediaset** 表的列。  
  
 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
