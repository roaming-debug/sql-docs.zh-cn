---
description: 'sys.pdw_diag_sessions (Transact-sql) '
title: sys.pdw_diag_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 4d23688a-cddb-4eed-8231-ecde2a0b0e65
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 3418e974be1c5b52b3fe4bcdcca8fa6dd4bfcc57
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404545"
---
# <a name="syspdw_diag_sessions-transact-sql"></a>sys.pdw_diag_sessions (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  保存有关已在系统上创建的各种诊断会话的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|**name**|**nvarchar(255)**|诊断会话的名称。<br /><br /> 此视图的键。||  
|**xml_data**|**nvarchar(4000)**|描述会话的 XML 有效负载。||  
|**is_active**|**bit**|指示标志是否处于活动状态的标记。||  
|**host_address**|**nvarchar(255)**|承载会话定义 (控制节点) 的计算机的地址。||  
|principal_id|**int**|在数据库级别创建会话的用户的 ID。||  
|database_id|**int**|作为诊断会话范围的数据库的 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
