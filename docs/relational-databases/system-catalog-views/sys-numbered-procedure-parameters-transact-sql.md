---
description: sys.numbered_procedure_parameters (Transact-SQL)
title: sys.numbered_procedure_parameters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- numbered_procedure_parameters_TSQL
- sys.numbered_procedure_parameters_TSQL
- numbered_procedure_parameters
- sys.numbered_procedure_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.numbered_procedure_parameters catalog view
ms.assetid: a441d46d-1f30-41c2-8d94-e9442f59786e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 861331152b5e3c25b09d14e326414a4ba25ba78b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191276"
---
# <a name="sysnumbered_procedure_parameters-transact-sql"></a>sys.numbered_procedure_parameters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  带编号过程的每个参数都在表中对应一行。 当您创建带编号的存储过程时，基过程的编号为 1。 所有后续过程的编号依次为 2、3 等。 **sys.numbered_procedure_parameters** 包含为2和更高编号的所有后续过程的参数定义。 该视图不显示基存储过程（编号 = 1）的参数。 基存储过程类似于无编号的存储过程。 因此，它的参数在 [transact-sql)  (](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)中表示。  
  
> [!IMPORTANT]  
>  不推荐使用带编号的过程。 建议您不要使用带编号过程。 当编译使用此目录视图的查询时，将会激发 DEPRECATION_ANNOUNCEMENT 事件。  
  
> [!NOTE]  
>  带编号的过程不支持 XML 和 CLR 参数。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此参数所属对象的 ID。|  
|**procedure_number**|**smallint**|对象中这种过程的数目（2 或更多）。|  
|name|**sysname**|参数的名称。 在 **procedure_number** 中是唯一的。|  
|**parameter_id**|**int**|参数的 ID。 在 **procedure_number** 中是唯一的。|  
|**system_type_id**|**tinyint**|参数的系统类型的 ID。|  
|**user_type_id**|**int**|由用户定义的由参数定义的类型 ID。|  
|**max_length**|**smallint**|参数的最大长度（字节）。<br /><br /> -1 = 列数据类型为 varchar(max)、nvarchar(max) 或 varbinary(max)。|  
|**精度**|**tinyint**|如果参数是基于数值的，则表示参数的精度；否则为 0。|  
|**scale**|**tinyint**|如果参数是基于数值的，则表示参数的小数位数；否则为 0。|  
|is_output|**bit**|1 = 输出或返回参数；否则为 0|  
|**is_cursor_ref**|**bit**|1 = 参数是游标引用参数。|  
  
> [!NOTE]  
>  带编号的过程不支持 XML 和 CLR 参数。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
