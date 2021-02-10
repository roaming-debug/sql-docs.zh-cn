---
description: CompareEnum
title: CompareEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CompareEnum
helpviewer_keywords:
- CompareEnum enumeration [ADO]
ms.assetid: bc8f710d-0621-4673-8d8e-0361e44abed0
author: rothja
ms.author: jroth
ms.openlocfilehash: 69773a836dbbf039ed5521f862f2e49e62b13456
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034787"
---
# <a name="compareenum"></a>CompareEnum
指定由书签表示的两个记录的相对位置。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adCompareEqual**|1|指示书签相等。|  
|**adCompareGreaterThan**|2|指示第一个书签位于第二个书签之后。|  
|**adCompareLessThan**|0|指示第一个书签位于第二个书签之前。|  
|**adCompareNotComparable**|4|指示无法比较书签。|  
|**adCompareNotEqual**|3|指示书签不相等且不排序。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums|  
|AdoEnums. GREATERTHAN|  
|AdoEnums. LESSTHAN|  
|AdoEnums. NOTCOMPARABLE|  
|AdoEnums. NOTEQUAL|  
  
## <a name="applies-to"></a>应用于  
 [CompareBookmarks 方法 (ADO)](./comparebookmarks-method-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [CompareBookmarks 方法 (ADO)](./comparebookmarks-method-ado.md)