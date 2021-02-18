---
title: DatabaseToConnect 元素 (DTA)
description: 在 dta 实用工具中，DatabaseToConnect 元素指定数据库引擎优化顾问在优化工作负载时连接到的第一个数据库。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DatabaseToConnect element
ms.assetid: 65153a66-3aee-4429-99b7-0816ac23c285
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: daa54d591f1c4babfaeb1d22bc8da585909142b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342063"
---
# <a name="databasetoconnect-element-dta"></a>DatabaseToConnect 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

指定数据库引擎优化顾问在优化工作负荷时连接到的第一个数据库。  
  
## <a name="syntax"></a>语法  
  
```  
  
    <TuningOptions>  
...code removed here...  
      <DatabaseToConnect>...</DatabaseToConnect>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|**string**，长度没有限制。|  
|**默认值**|无。|  
|**出现次数**|可选。 对于每个 **TuningOptions** 元素可以使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[TuningOptions 元素 (DTA)](../../tools/dta/tuningoptions-element-dta.md)|  
|**子元素**|无|  
  
## <a name="remarks"></a>备注  
 使用 **DatabaseToConnect** ，可以指定希望数据库引擎优化顾问在启动优化会话时连接到的第一个数据库的名称。 此元素只能指定一个数据库。 如果指定了多个数据库名称，数据库引擎优化顾问将返回错误。  
  
## <a name="example"></a>示例  
 有关使用示例，请参阅[使用内联工作负荷的 XML 输入文件示例 (DTA)](../../tools/dta/xml-input-file-sample-with-inline-workload-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
