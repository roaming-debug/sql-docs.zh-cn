---
description: sp_helpfile (Transact-SQL)
title: sp_helpfile (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpfile
- sp_helpfile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpfile
ms.assetid: 1546e0ae-5a99-4e01-9eb9-d147fa65884c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 87bf389e2cd28fdf031308c116c32105d2f3c215
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204426"
---
# <a name="sp_helpfile-transact-sql"></a>sp_helpfile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回与当前数据库关联的文件的物理名称及属性。 使用此存储过程确定附加到服务器或从服务器分离的文件名。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpfile [ [ @filename= ] 'name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @filename = ] 'name'` 当前数据库中任意文件的逻辑名称。 *名称* 为 **sysname**，默认值为 NULL。 如果未指定 *name* ，则返回当前数据库中所有文件的属性。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|逻辑文件名。|  
|**fileid**|**smallint**|文件的数字标识符。 如果指定了 *名称* ，则不会返回 *。*|  
|**filename**|**nchar (260)**|物理文件名。|  
|**文件**|**sysname**|文件所属的文件组。<br /><br /> NULL = 文件为日志文件。 它决不是文件组的一部分。|  
|**大小**|**nvarchar (15)**|文件大小 (KB)。|  
|**maxsize**|**nvarchar (15)**|文件大小可达到的最大值。 此字段中的 UNLIMITED 值表示文件可以一直增长到磁盘变满为止。|  
|**年**|**nvarchar (15)**|文件的增量。 表示每次需要新空间时为文件增加的空间大小。<br /><br /> 0 = 文件的大小是固定的，不会增长。|  
|**使用情况**|**varchar (9)**|对于数据文件，该值为 **"仅数据"** ; 对于日志文件，该值为 **"仅记录"**。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例返回有关 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 中的文件的信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_helpfile;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
