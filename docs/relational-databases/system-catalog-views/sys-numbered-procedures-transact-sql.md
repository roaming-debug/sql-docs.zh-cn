---
description: sys.numbered_procedures (Transact-SQL)
title: sys.numbered_procedures (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.numbered_procedures_TSQL
- numbered_procedures
- sys.numbered_procedures
- numbered_procedures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.numbered_procedures catalog view
ms.assetid: 5b6d6498-bac6-4266-94b9-d16ef5089cf0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34d67487512792261397d80e9353edecf4775b82
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095491"
---
# <a name="sysnumbered_procedures-transact-sql"></a>sys.numbered_procedures (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  每个作为带编号的过程而创建的 SQL Server 存储过程都占一行。 对于基（编号 = 1）存储过程，它不显示行。 基本存储过程的条目可在 **sys.databases** 和 **sys** 等视图中找到。  
  
> [!IMPORTANT]  
>  不推荐使用带编号的过程。 建议您不要使用带编号过程。 当编译使用此目录视图的查询时，将会激发 DEPRECATION_ANNOUNCEMENT 事件。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|存储过程的对象的 ID。|  
|**procedure_number**|**smallint**|对象中这种过程的数目（2 或更多）。|  
|**definition**|**nvarchar(max)**|用于定义此过程的 SQL Server 文本。<br /><br /> NULL = 已加密。|  
  
> [!NOTE]  
>  带编号的过程不支持 XML 和 CLR 参数。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
