---
description: AffectEnum
title: AffectEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AffectEnum
helpviewer_keywords:
- AffectEnum enumeration [ADO]
ms.assetid: 1ab921a0-6c57-43b4-9291-701b2599f3e8
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a4f722e1ac4557ff5e2fb89d63c3d40f490c5b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164858"
---
# <a name="affectenum"></a>AffectEnum
指定哪些记录受操作影响。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adAffectAll**|3|如果没有应用于 **记录集** 的 [筛选器](./filter-property.md)，则会影响所有记录。<br /><br /> 如果 " **筛选器** " 属性设置为字符串条件 (如 "Author = ' Smith '" ) ，则操作会影响当前章节中的可见记录。<br /><br /> 如果 " **筛选器** " 属性设置为 [FilterGroupEnum](./filtergroupenum.md) 或书签数组的成员，则该操作将影响 **记录集** 的所有行。 **注意： adAffectAll** 在 Visual Basic 对象浏览器中处于隐藏状态。|  
|**adAffectAllChapters**|4|影响 **记录集** 的所有同级章节中的所有记录，包括那些通过当前应用的任何 **筛选器** 看不到的记录。|  
|**adAffectCurrent**|1|仅影响当前记录。|  
|**adAffectGroup**|2|仅影响满足当前 [Filter](./filter-property.md) 属性设置的记录。 若要使用此选项，必须将 **Filter** 属性设置为 **FilterGroupEnum** 值或 **书签** 的数组。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums。所有|  
|AdoEnums. ALLCHAPTERS|  
|AdoEnums 影响。当前|  
|AdoEnums。影响组|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [CancelBatch 方法 (ADO)](./cancelbatch-method-ado.md)  
        [Delete 方法（ADO 记录集）](./delete-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [重新同步方法](./resync-method.md)  
        [UpdateBatch 方法](./updatebatch-method.md)  
    :::column-end:::
:::row-end:::