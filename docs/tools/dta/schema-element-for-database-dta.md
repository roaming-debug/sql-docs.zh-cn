---
title: 数据库的架构元素 (DTA)
description: 在 dta 实用工具中，数据库的架构元素会指定要优化的数据库的架构。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Schema element
ms.assetid: d932e59c-953f-4ab4-934d-b6baf344835c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: cdce54e625d2d3861f61d1100fe03dd409b43e81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345860"
---
# <a name="schema-element-for-database-dta"></a>数据库的架构元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定要优化的数据库的架构。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Database>  
...code removed here...  
    <Schema>...</Schema>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|对于在 **Server** 元素下指定的 **Database** 元素，需要使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[服务器的数据库元素 (DTA)](../../tools/dta/database-element-for-server-dta.md)|  
|**子元素**|[架构的名称元素 (DTA)](../../tools/dta/name-element-for-schema-dta.md)<br /><br /> [架构的表元素 (DTA)](../../tools/dta/table-element-for-schema-dta.md)|  
  
## <a name="example"></a>示例  
 有关此元素的使用示例，请参阅[服务器元素 (DTA)](../../tools/dta/server-element-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
