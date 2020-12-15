---
description: 'sys.pdw_health_component_groups (Transact-sql) '
title: sys.pdw_health_component_groups (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
ms.assetid: 5ba27432-7a29-4420-b73d-def621c0b3ac
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 552a4b28187687c21312c5918788eaf85e799597
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464628"
---
# <a name="syspdw_health_component_groups-transact-sql"></a>sys.pdw_health_component_groups (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  存储有关组件和设备的逻辑分组的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|group_id|**int**|组件和设备的唯一标识符。<br /><br /> 此视图的键。|NOT NULL|  
|group_name|**nvarchar(255)**|组件和设备的逻辑组名称。|NOT NULL|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
