---
description: sys.sysperfinfo (Transact-SQL)
title: sys.sysperfinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysperfinfo_TSQL
- sys.sysperfinfo_TSQL
- sys.sysperfinfo
- sysperfinfo
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysperfinfo compatibility view
- sysperfinfo system table
ms.assetid: e22a81cd-27de-4690-9443-6aad6393bd3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 477121d623733b5bafcf4385d8069715a333175e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095393"
---
# <a name="syssysperfinfo-transact-sql"></a>sys.sysperfinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 可通过 Windows 系统监视器显示的内部性能计数器的表示形式。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar(128)**|性能对象名称，例如 **SQLServer： LockManager** 或 **sqlserver： BufferManager**。|  
|**counter_name**|**nchar(128)**|对象中性能计数器的名称，如请求的 **页面请求** 或 **锁**。|  
|**instance_name**|**nchar(128)**|计数器的命名实例。 例如，会为每种类型的锁（如 **表**、 **页**、 **键** 等等）维护计数器。 实例名在相似的计数器之间是有区别的。|  
|**cntr_value**|**bigint**|实际计数器值。 通常，该值是一个级别或对实例事件发生进行计数的单调递增计数器。|  
|**cntr_type**|**int**|Windows 性能体系结构定义的计数器类型。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
