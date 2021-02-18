---
title: FeatureSet 元素 (DTA)
description: 在 dta 实用工具中，FeatureSet 元素包含分析期间数据库引擎优化顾问使用的物理设计结构。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- FeatureSet element
ms.assetid: f2070c53-4a5c-4c11-ac38-96ee200c84f0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 64dc5efd785497298f838d5655702185517d1794
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335993"
---
# <a name="featureset-element-dta"></a>FeatureSet 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

包含数据库引擎优化顾问在分析中使用的物理设计结构（索引或索引视图）。  
  
## <a name="syntax"></a>语法  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <FeatureSet>...</FeatureSet>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，无最大长度。|  
|**允许的值**|**IDX_IV**<br /> 索引和索引视图。<br /><br /> **IDX**<br /> 仅限索引。<br /><br /> **IV**<br /> 仅限索引视图。<br /><br /> **NCL_IDX**<br /> 仅限非聚集索引。<br /><br /> 将这些值中的一个值用于此元素。|  
|**默认值**|**IDX**|  
|**出现次数**|除非使用了 **TuningOptions** 元素，否则对每个 **DropOnlyMode** 元素仅需要一次。 如果使用 **DropOnlyMode** ，则无法使用 **FeatureSet**。 这两种元素是互相排斥的。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[TuningOptions 元素 (DTA)](../../tools/dta/tuningoptions-element-dta.md)|  
|**子元素**|无。|  
  
## <a name="example"></a>示例  
 有关此元素的使用示例，请参阅[简单 XML 输入文件示例 (DTA)](../../tools/dta/simple-xml-input-file-sample-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
