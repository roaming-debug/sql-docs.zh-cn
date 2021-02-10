---
description: CursorTypeEnum
title: CursorTypeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorTypeEnum
helpviewer_keywords:
- CursorTypeEnum enumeration [ADO]
ms.assetid: ffc6e245-4471-42ae-84dd-e85bddfce983
author: rothja
ms.author: jroth
ms.openlocfilehash: 9ce49416e2cb298e4829022b7da0840a5e2c516a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025863"
---
# <a name="cursortypeenum"></a>CursorTypeEnum
指定 [记录集](./recordset-object-ado.md) 对象中使用的游标类型。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adOpenDynamic**|2|使用动态游标。 其他 **用户的添加** 、更改和删除是可见的，并且允许所有类型的移动（书签除外，如果提供程序不支持）。|  
|**adOpenForwardOnly**|0|默认。 使用只进游标。 与静态游标相同，不同之处在于只能向前滚动记录。 当只需通过 **记录集** 进行一次传递时，这可以提高性能。|  
|**adOpenKeyset**|1|使用键集游标。 与动态游标类似，但你看不到其他用户添加的记录，但其他用户删除的记录无法从 **记录集中** 访问。 其他用户的数据更改仍然可见。|  
|**adOpenStatic**|3|使用静态游标，该游标是一组记录的静态副本，可用于查找数据或生成报表。 其他用户的添加、更改或删除不可见。|  
|**adOpenUnspecified**|-1|不指定游标的类型。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. CursorType|  
|AdoEnums. CursorType. FORWARDONLY|  
|AdoEnums. CursorType|  
|AdoEnums. CursorType|  
|AdoEnums. CursorType。未指定|  
  
## <a name="applies-to"></a>应用于  
 [CursorType 属性 (ADO)](./cursortype-property-ado.md)