---
title: KeepExisting 元素 (DTA)
description: 在 dta 实用工具中，KeepExisting 元素指定在生成建议时数据库引擎优化顾问保留的物理设计结构。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- KeepExisting element
ms.assetid: e67aae61-d06d-4a03-85ba-6516c3502dce
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 829500f7335d1cb805b309ef642e6f70393b6912
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345906"
---
# <a name="keepexisting-element-dta"></a>KeepExisting 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定数据库引擎优化顾问在生成建议时必须保留的物理设计结构（索引、索引视图或分区）。  
  
## <a name="syntax"></a>语法  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <KeepExisting>...</KeepExisting>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，服务器强制执行的长度限制。|  
|**允许的值**|**NONE**<br /> 无现有结构。<br /><br /> **ALL**<br /> 所有现有结构。<br /><br /> **ALIGNED**<br /> 所有分区对齐结构。<br /><br /> **CL_IDX**<br /> 表中的所有聚集索引。<br /><br /> **IDX**<br /> 表中的所有聚集索引和非聚集索引。<br /><br /> 只能将这些值中的一个用于此元素。|  
|**默认值**|无。|  
|**出现次数**|可选。 对于每个 **TuningOptions** 元素只能使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[TuningOptions 元素 (DTA)](../../tools/dta/tuningoptions-element-dta.md)|  
|**子元素**|无。|  
  
## <a name="example"></a>示例  
 有关此元素的使用示例，请参阅[简单 XML 输入文件示例 (DTA)](../../tools/dta/simple-xml-input-file-sample-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
