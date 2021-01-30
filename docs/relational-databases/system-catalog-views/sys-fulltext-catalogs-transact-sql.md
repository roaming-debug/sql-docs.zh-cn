---
description: sys.fulltext_catalogs (Transact-SQL)
title: sys.fulltext_catalogs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_catalogs_TSQL
- sys.fulltext_catalogs
- fulltext_catalogs
- fulltext_catalogs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_catalogs catalog view
ms.assetid: cf1489ff-4819-41fa-a62a-4ed797a16207
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 38420a12b6b6cab661644ef8d176c6e08c5e0c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191514"
---
# <a name="sysfulltext_catalogs-transact-sql"></a>sys.fulltext_catalogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个全文目录对应一行。  
  
> [!NOTE]  
>  未来版本的中将删除以下列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ： **data_space_id**、 **file_id** 和 **path**。 请不要在新的开发工作中使用这些列，并尽快修改当前使用上述任意列的应用程序。  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|fulltext_catalog_id|**int**|全文目录的 ID。 该 ID 在数据库中的所有全文目录中是唯一的。|  
|name|**sysname**|目录的名称。 在该数据库中是唯一的。|  
|path|**nvarchar(260)**|目录所在的文件系统中的目录的名称。|  
|is_default|**bit**|默认的全文目录。<br /><br /> True = 默认。<br /><br /> False = 非默认。|  
|is_accent_sensitivity_on|**bit**|目录的区分重音设置。<br /><br /> True = 区分重音。<br /><br /> True = 不区分重音。|  
|data_space_id|**int**|创建此目录时所在的文件组。|  
|file_id|**int**|与目录关联的全文文件的文件 ID。|  
|principal_id|**int**|全文目录所属的数据库主体的 ID。|  
|is_importing|**bit**|指示是否正在导入全文目录：<br /><br /> 1 = 正在导入目录。<br /><br /> 2 = 没有导入目录。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [ALTER FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)  
  
  
