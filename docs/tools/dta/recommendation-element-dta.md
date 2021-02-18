---
title: 建议元素 (DTA)
description: 在 dta 实用工具中，建议元素包含有关属于用户指定配置的假设索引的信息。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Recommendation element
ms.assetid: 679ea535-865a-4633-a4d3-5b3090515158
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 96684c5962d6417f87999411131c924045933dd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345872"
---
# <a name="recommendation-element-dta"></a>建议元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

包含有关属于用户指定配置的假设索引的信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Configuration>  
    ...code removed here...  
    <Table>  
      <Name>...</Name>  
      <Recommendation>  
      ...code removed here...  
      </Recommendation>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|可选。 对于每个 **Table** 元素可以使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[架构的表元素 (DTA)](../../tools/dta/table-element-for-schema-dta.md)|  
|**子元素**|[创建元素 (DTA)](../../tools/dta/create-element-dta.md)<br /><br /> **Drop** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。|  
  
## <a name="remarks"></a>备注  
 在数据库引擎优化顾问 XML 架构中，此元素的名称为 **RecommendationTypecomplexType** 。 该元素用于指定假设配置的索引。 不要将此 **Recommendation** 元素与可用于指定分区的其他类型 (**RecommendationPType**) 或视图 (**RecommendationViewType**) 混淆。 有关这些 **Recommendation** 元素其他类型的信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。  
  
## <a name="example"></a>示例  
 有关此元素的用法示例，请参阅[用户指定配置 (DTA) 的 XML 输入文件示例](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
