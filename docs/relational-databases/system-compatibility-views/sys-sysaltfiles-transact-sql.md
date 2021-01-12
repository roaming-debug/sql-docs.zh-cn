---
description: sys.sysaltfiles (Transact-SQL)
title: sys.sysaltfiles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysaltfiles_TSQL
- sys.sysaltfiles
- sysaltfiles_TSQL
- sysaltfiles
dev_langs:
- TSQL
helpviewer_keywords:
- sysaltfiles system table
- sys.sysaltfiles compatibility view
ms.assetid: 698dec23-5336-4108-87a5-f8e407f8da09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6139fb221ab1d960d8cfb455e592ac16fbcee09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097792"
---
# <a name="syssysaltfiles-transact-sql"></a>sys.sysaltfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在特殊情况下，包含与数据库中的文件相对应的行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**fileid**|**smallint**|文件标识号。 它对每个数据库都是唯一的。|  
|**groupid**|**smallint**|文件组标识号。|  
|**大小**|**int**|文件大小（以 8 KB 页为单位）。|  
|**maxsize**|**int**|最大文件大小（以 8 KB 为单位的页）。<br /><br /> 0 = 无增长。<br /><br /> -1 = 文件将一直增长到磁盘充满为止。<br /><br /> 268435456 = 日志文件将增长到最大大小 2 TB。<br /><br /> 注意：如果使用无限制的日志文件大小升级的数据库，日志文件的最大大小将报告为-1。|  
|**年**|**int**|数据库的增长大小。<br /><br /> 0 = 无增长。 根据状态的值，可以是页数或文件大小的百分比。 如果 **status** 为0x100000，则 **增长** 是文件大小的百分比;否则，为页数。|  
|**status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**性能**|**int**|保留。|  
|**dbid**|**smallint**|该文件所属数据库的数据库标识号。|  
|name|**sysname**|文件的逻辑名称。|  
|**filename**|**nvarchar(260)**|物理设备的名称。 这包括文件的完整路径。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
