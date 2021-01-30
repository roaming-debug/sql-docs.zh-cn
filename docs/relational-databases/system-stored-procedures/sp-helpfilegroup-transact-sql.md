---
description: sp_helpfilegroup (Transact-SQL)
title: sp_helpfilegroup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfilegroup_TSQL
- sp_helpfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfilegroup
ms.assetid: 619716b5-95dc-4538-82ae-4b90b9da8ebc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39899feae9ad7b4cd6f2ab45caf11ddd3e89cd98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204408"
---
# <a name="sp_helpfilegroup-transact-sql"></a>sp_helpfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回与当前数据库相关联的文件组的名称及属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpfilegroup [ [ @filegroupname = ] 'name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @filegroupname = ] 'name'` 当前数据库中任意文件组的逻辑名称。 *名称* 为 **sysname**，默认值为 NULL。 如果未指定 *name* ，则列出当前数据库中的所有文件组，并仅显示 "结果集" 部分中显示的第一个结果集。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**groupname**|**sysname**|文件组的名称。|  
|**groupid**|**smallint**|数字文件组标识符。|  
|**filecount**|**int**|文件组中的文件数目。|  
  
 如果指定 *name* ，则返回文件组中的每个文件对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**file_in_group**|**sysname**|文件组中文件的逻辑名称。|  
|**fileid**|**smallint**|数字文件标识符。|  
|**filename**|**nchar (260)**|文件的物理名称，包括目录路径。|  
|**大小**|**nvarchar (15)**|文件大小 (KB)。|  
|**maxsize**|**nvarchar (15)**|文件的最大大小。<br /><br /> 这是文件可增长到的最大大小。 此字段中的 UNLIMITED 值表示文件可以一直增长到磁盘变满为止。|  
|**年**|**nvarchar (15)**|文件的增量。 表示每次需要新的空间时给文件增加的空间大小。<br /><br /> 0 = 文件的大小是固定的，不会增长。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-all-filegroups-in-a-database"></a>A. 返回数据库中的所有文件组  
 下面的示例返回有关 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中文件组的信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup;  
GO  
```  
  
### <a name="b-returning-all-files-in-a-filegroup"></a>B. 返回文件组中的所有文件  
 下面的示例返回有关 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库的 `PRIMARY` 文件组中所有文件的信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfilegroup 'PRIMARY';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
