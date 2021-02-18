---
title: 列的名称元素 (DTA)
description: 在 dta 实用工具中，列的 Name 元素在用户指定的配置中为索引列指定一个名称。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Name element
ms.assetid: f93b61de-01fe-4237-ada4-f1e481550564
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a4fe19ac2de02115abcdd10e51abdfb089609581
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353519"
---
# <a name="name-element-for-column-dta"></a>列的名称元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在用户指定的配置中指定索引列的名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Index>  
    <Column>  
        <Name>...</Name>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，长度没有限制。|  
|**默认值**|无。|  
|**出现次数**|对于每个 **Column** 元素必须使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[索引的列元素 (DTA)](../../tools/dta/column-element-for-index-dta.md)|  
|**子元素**|无。|  
  
## <a name="example"></a>示例  
 有关此元素的用法示例，请参阅[使用用户指定配置 (DTA) 的 XML 输入文件示例](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
