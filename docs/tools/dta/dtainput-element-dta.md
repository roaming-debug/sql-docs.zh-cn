---
title: DTAInput 元素 (DTA)
description: 在 dta 实用工具中，DTAInput 元素包含用于数据库引擎优化顾问的 XML 输入的定义。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DTAInput element
ms.assetid: 40c19abf-ded5-43de-be96-5b43b1b81b03
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 275bd84346c334e0c92c73a7750660338c957fdf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338726"
---
# <a name="dtainput-element-dta"></a>DTAInput 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

包含用于数据库引擎优化顾问的 XML 输入的定义。  
  
## <a name="syntax"></a>语法  
  
```  
  
<DTAXML>  
    <DTAInput>  
    ...code removed here...  
    </DTAInput>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|---------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|可选。对于每个 **DTAXML** 元素可以使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[DTAXML 元素 (DTA)](../../tools/dta/dtaxml-element-dta.md)|  
|**子元素**|[服务器元素 (DTA)](../../tools/dta/server-element-dta.md)<br /><br /> [工作负荷元素 (DTA)](../../tools/dta/workload-element-dta.md)<br /><br /> [TuningOptions 元素 (DTA)](../../tools/dta/tuningoptions-element-dta.md)<br /><br /> [配置元素 (DTA)](../../tools/dta/configuration-element-dta.md)|  
  
## <a name="remarks"></a>备注  
 此元素是数据库引擎优化顾问输入架构层次结构的根元素。 数据库引擎优化顾问的输入可以是各种参数，这些参数指定了要优化其数据库的服务器、工作负荷、优化选项或用户指定配置。  
  
## <a name="example"></a>示例  
 有关 **DTAInput** 元素的使用示例，请参阅 [简单 XML 输入文件示例 (DTA)](../../tools/dta/simple-xml-input-file-sample-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
