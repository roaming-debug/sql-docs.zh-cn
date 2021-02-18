---
title: 架构的名称元素 (DTA)
description: 在 dta 实用工具中，架构的名称元素包含架构名称。 本文将介绍这一元素。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Name element
ms.assetid: 014e4854-fed2-454b-8557-5f7c5bb6b17a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 3df910d06d065f84dfd214aacf9a92b81e58ac96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338706"
---
# <a name="name-element-for-schema-dta"></a>架构的名称元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

包含架构的名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Database>  
...code removed here  
    <Schema>  
        <Name>...</Name>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，介于 1 到 255 个字符之间|  
|**默认值**|无。|  
|**出现次数**|要求每个 **架构** 元素出现一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[数据库的架构元素 (DTA)](../../tools/dta/schema-element-for-database-dta.md)|  
|**子元素**|无。|  
  
## <a name="example"></a>示例  
 有关此元素的使用示例，请参阅[服务器元素 (DTA)](../../tools/dta/server-element-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
