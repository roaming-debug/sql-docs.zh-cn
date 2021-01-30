---
description: AllowNullsEnum
title: AllowNullsEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AllowNullsEnum
helpviewer_keywords:
- AllowNullsEnum enumeration [ADOX]
ms.assetid: 6acf3689-1a7f-4379-9d7f-df452ccbac27
author: rothja
ms.author: jroth
ms.openlocfilehash: 872d3dfd6658a7190e3f18fcc23a1b7fb34da1f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164313"
---
# <a name="allownullsenum"></a>AllowNullsEnum
指定是否为具有 null 值的记录编制索引。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adIndexNullsAllow**|0|索引允许键列为空的项。 如果在键列中输入 null 值，则会将该项插入到索引中。|  
|**adIndexNullsDisallow**|1|默认。 索引不允许键列为空的项。 如果在键列中输入 null 值，将出现错误。|  
|**adIndexNullsIgnore**|2|此索引不插入包含空键的项。 如果在键列中输入 null 值，则该条目将被忽略且不发生错误。|  
|**adIndexNullsIgnoreAny**|4|此索引不会插入某些键列的值为 null 的项。 对于包含多列键的索引，如果在某个列中输入 null 值，则该条目将被忽略且不发生错误。|  
  
## <a name="applies-to"></a>应用于  
 [IndexNulls 属性 (ADOX)](./indexnulls-property-adox.md)