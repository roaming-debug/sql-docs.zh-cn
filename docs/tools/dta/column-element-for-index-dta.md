---
title: 索引的列元素 (DTA)
description: 在 dta 实用工具中，索引的 Column 元素指定为用户指定的配置创建索引的列。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Column element
ms.assetid: ba9fac20-26bd-4333-940e-842c15241b46
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.openlocfilehash: ea803914ac69a51663b6d489600a7c3719dd5ee0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342068"
---
# <a name="column-element-for-index-dta"></a>索引的列元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定在其上为用户指定的配置创建索引的列。  
  
## <a name="syntax"></a>语法  
  
```  
  
<Create>  
  <Index>  
    <Name>...</Name>  
    <Column [Type | SortOrder]>  
     ...code removed here...  
     </Column>  
```  
  
## <a name="element-attributes"></a>元素属性  
  
 **类型**：可选。 指定索引列类型。 使用 string 数据类型将此属性指定为以下允许  值之一：  
  
-   **KeyColumn**  
  
     指定按索引键进行引用的列。 使用下面的语法设置此属性：  
  
    ```  
    <Column Type="KeyColumn">  
    ```  
  
     有关键列的详细信息，请参阅 [群集索引和非群集索引介绍](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)。  
  
-   **IncludedColumn**  
  
     指定某列是包含列（而不是键列）。 使用下面的语法设置此属性：  
  
    ```  
    <Column Type="IncludedColumn">  
    ```  
  
     有关包含列的详细信息，请参阅 [创建带有包含列的索引](../../relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
 SortOrder  ：可选。 指定列的排序顺序。 请使用 **string** 数据类型按如下格式指定 **Ascending** 或 **Descending** 排序顺序：  
  
```  
<Column SortOrder="Ascending">  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|最多可以为 **Index** 元素指定 1024 列。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[索引元素 (DTA)](../../tools/dta/index-element-dta.md)|  
|**子元素**|[列的名称元素 (DTA)](../../tools/dta/name-element-for-column-dta.md)|  
  
## <a name="example"></a>示例  
 有关此元素的用法示例，请参阅[用户指定配置 (DTA) 的 XML 输入文件示例](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
