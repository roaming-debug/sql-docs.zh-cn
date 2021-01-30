---
description: sp_helpmergedeleteconflictrows (Transact-SQL)
title: sp_helpmergedeleteconflictrows (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergedeleteconflictrows
- sp_helpmergedeleteconflictrows_TSQL
helpviewer_keywords:
- sp_helpmergedeleteconflictrows
ms.assetid: 222be651-5690-4341-9dfb-f9ec1d80c970
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07ca90c1eb7792ea5d197fe37a3542f526f870b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179247"
---
# <a name="sp_helpmergedeleteconflictrows-transact-sql"></a>sp_helpmergedeleteconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关丢失删除的冲突的数据行的信息。 如果使用了分散的冲突日志，则将在发布服务器上对发布数据库执行此存储过程，或在订阅服务器上对订阅数据库执行此存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergedeleteconflictrows [ [ @publication = ] 'publication']  
    [ , [ @source_object = ] 'source_object']  
    [ , [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publsher_db'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** 。 如果指定了发布，将返回由该发布限定的所有冲突。  
  
`[ @source_object = ] 'source_object'` 源对象的名称。 *source_object* 为 **nvarchar (386)**，默认值为 NULL。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。*发布服务器* 的 **sysname**，默认值为 NULL。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。*publisher_db* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**source_object**|**nvarchar (386)**|删除冲突的源对象。|  
|**rowguid**|**uniqueidentifier**|删除冲突的行标识符。|  
|**conflict_type**|**int**|指示冲突类型的代码：<br /><br /> **1** = UpdateConflict：在行级别上检测到冲突。<br /><br /> **2** = ColumnUpdateConflict：在列级别上检测到冲突。<br /><br /> **3** = UpdateDeleteWinsConflict：删除入选冲突。<br /><br /> **4** = UpdateWinsDeleteConflict：丢失冲突的已删除 rowguid 记录在此表中。<br /><br /> **5** = UploadInsertFailed：无法在发布服务器上应用订阅服务器中的 Insert。<br /><br /> **6** = DownloadInsertFailed：无法在订阅服务器上应用发布服务器中的 Insert。<br /><br /> **7** = UploadDeleteFailed：无法将订阅服务器上的删除操作上载到发布服务器。<br /><br /> **8** = DownloadDeleteFailed：无法将发布服务器上的删除操作下载到订阅服务器。<br /><br /> **9** = Uploadinsertfailed：订阅服务器上的更新无法在发布服务器上应用。<br /><br /> **10** = DownloadUpdateFailed：无法将发布服务器上的更新应用于订阅服务器。|  
|**reason_code**|**Int**|与上下文相关的错误代码。|  
|**reason_text**|**varchar (720)**|与上下文相关的错误说明。|  
|**origin_datasource**|**varchar(255)**|冲突的起源。|  
|**pubid**|**uniqueidentifier**|发布标识符。|  
|**MSrepl_create_time**|**datetime**|添加冲突信息的时间。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergedeleteconflictrows** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员和 **db_owner** 固定数据库角色的成员才能执行 **sp_helpmergedeleteconflictrows**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
