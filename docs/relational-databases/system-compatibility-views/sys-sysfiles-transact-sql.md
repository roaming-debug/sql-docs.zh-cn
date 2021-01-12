---
description: sys.sysfiles (Transact-SQL)
title: " (Transact-sql) sys.sys文件 |Microsoft Docs"
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysfiles
- sys.sysfiles_TSQL
- sys.sysfiles
- sysfiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfiles system table
- sys.sysfiles compatibility view
ms.assetid: 3b47f38d-1cff-404d-89d3-9342c451c802
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 862ffbdd0967068ef1b8345176edb4551e542196
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099058"
---
# <a name="syssysfiles-transact-sql"></a>sys.sysfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  数据库中的每个文件对应一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**fileid**|**smallint**|每个数据库的唯一文件标识号。|  
|**groupid**|**smallint**|文件组标识号。|  
|**大小**|**int**|文件大小（8 KB 页）。|  
|**maxsize**|**int**|最大文件大小（以 8 KB 为单位的页）。<br /><br /> 0 = 无增长。<br /><br /> -1 = 文件将一直增长到磁盘充满为止。<br /><br /> 268435456 = 日志文件将增长到最大大小 2 TB。<br /><br /> 注意：如果使用无限制的日志文件大小升级的数据库，日志文件的最大大小将报告为-1。|  
|**年**|**int**|数据库的增长大小。 根据 **状态** 的值，可以是页数或文件大小的百分比。<br /><br /> 0 = 无增长。|  
|**status**|**int**|以兆字节 (MB) 或 kb (KB) **增长** 值的状态位。<br /><br /> 0x2 = 磁盘文件。<br /><br /> 0x40 = 日志文件。<br /><br /> 0x100000 = 增长。 该值是百分比，不是页数。|  
|**性能**|**int**|保留。|  
|name|**sysname**|文件的逻辑名称。|  
|**filename**|**nvarchar(260)**|物理设备的名称。 这包括文件的完整路径。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
