---
description: RESTORE 语句 - LABELONLY (Transact-SQL)
title: RESTORE LABELONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LABELONLY
- RESTORE_LABELONLY_TSQL
- LABELONLY_TSQL
- RESTORE LABELONLY
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE LABELONLY statement
- backup media [SQL Server], content information
ms.assetid: 7cf0641e-0d55-4ffb-9500-ecd6ede85ae5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017
ms.openlocfilehash: 4bb66a3711bcb2a5a309c4b712470c4fb3b16bb9
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88478703"
---
# <a name="restore-statements---labelonly-transact-sql"></a>RESTORE 语句 - LABELONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]
  返回一个结果集，它包含给定备份设备识别的备份介质的相关信息。  
  
> [!NOTE]  
>  有关参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
RESTORE LABELONLY   
FROM <backup_device>   
[ WITH   
 {  
--Media Set Options  
   MEDIANAME = { media_name | @media_name_variable }   
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
 有关 RESTORE LABELONLY 参数的说明，请参阅 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。  
  
## <a name="result-sets"></a>结果集  
 RESTORE LABELONLY 的结果集包含提供下列信息的一个行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**MediaName**|**nvarchar(128)**|介质的名称。|  
|**MediaSetId**|**uniqueidentifier**|介质集的唯一标识号。|  
|**FamilyCount**|**int**|媒体集中的媒体簇数。|  
|**FamilySequenceNumber**|**int**|此介质簇的序号。|  
|**MediaFamilyId**|**uniqueidentifier**|媒体簇的唯一标识号。|  
|**MediaSequenceNumber**|**int**|此介质在介质簇中的序号。|  
|**MediaLabelPresent**|**tinyint**|介质说明中是否包含：<br /><br /> 1 = [!INCLUDE[msCoName](../../includes/msconame-md.md)] 磁带格式媒体标签<br /><br /> 0 = 媒体描述|  
|**MediaDescription**|**nvarchar(255)**|介质说明（自由格式的文本）或磁带格式介质标签。|  
|**SoftwareName**|**nvarchar(128)**|写入标签的备份软件名称。|  
|**SoftwareVendorId**|**int**|写入备份的软件供应商的唯一供应商标识号。|  
|**MediaDate**|**datetime**|标签的写入日期和时间。|  
|**Mirror_Count**|**int**|介质集中的镜像服务器数 (1-4)。<br /><br /> 注意：在媒体集中为不同镜像服务器写入的标签是相同的。|  
|**IsCompressed**|**bit**|备份是否已压缩：<br /><br /> 0 = 未压缩<br /><br /> 1 = 已压缩|  
  
> [!NOTE]  
>  如果为介质集定义了密码，则只有在命令的 MEDIAPASSWORD 选项中指定了正确的介质密码时，RESTORE LABELONLY 才会返回信息。  
  
## <a name="general-remarks"></a>一般备注  
 执行 RESTORE LABELONLY 是一种查找备份介质所含内容的快速方法。 由于 RESTORE LABELONLY 只读取介质标头，因此，即使使用高容量磁带设备，该语句的执行速度也会非常快。  
  
## <a name="security"></a>安全性  
 备份操作可以有选择地指定介质集的密码。 如果为介质集定义了密码，则必须在 RESTORE 语句中指定正确的密码。 此密码可防止在未经授权的情况下使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行还原操作以及向介质追加备份集。 但是，密码不会阻止使用 BACKUP 语句的 FORMAT 选项覆盖介质。  
  
> [!IMPORTANT]  
>  此密码提供的安全性较低。 它旨在防止经过授权的用户或未经授权的用户使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具执行不正确的还原操作。 但是不能防止通过其他方式或通过替换密码来读取备份数据。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 保护备份的最佳做法是将备份磁带存储在安全位置，或者备份到由适当的访问控制列表 (ACL) 保护的磁盘文件。 ACL 应设置在创建备份的根目录下。  
  
### <a name="permissions"></a>权限  
 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中，获取有关备份集或备份设备的信息要求具有 CREATE DATABASE 权限。 有关详细信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
