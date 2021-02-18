---
title: 索引的文件组元素 (DTA)
description: 在 dta 实用工具中，索引的 Filegroup 元素指定将为用户指定的配置创建索引的文件组。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Filegroup element [DTA]
ms.assetid: 7078d2fb-fa77-44fc-beb3-c095088fcb85
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 8c7c2e6dbecbe46232f2e194821ab1fc0e4937f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335957"
---
# <a name="filegroup-element-for-index-dta"></a>索引的文件组元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定要针对用户指定的配置，对其创建索引的文件组。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Index>  
  <Name>...</Name>  
  <Column>  
    <Name>...</Name>  
  <Filegroup>...</Filegroup>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，长度没有限制。|  
|**默认值**|无。|  
|**出现次数**|可选。 对于每个 **Index** 元素可以使用一次。 如果为 **PartitionScheme** 元素指定了 **PartitionColumn** 和 **Index** 元素，则不能使用此元素。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[索引元素 (DTA)](../../tools/dta/index-element-dta.md)|  
|**子元素**|无。|  
  
## <a name="example"></a>示例  
 有关此元素的用法示例，请参阅[使用用户指定配置 (DTA) 的 XML 输入文件示例](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
