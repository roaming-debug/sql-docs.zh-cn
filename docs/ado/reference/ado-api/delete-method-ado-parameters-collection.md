---
description: Delete 方法（ADO 参数集合）
title: 删除方法 (ADO 参数集合) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Delete
- _DynaCollection::raw_Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 160c575e-df63-4ade-a2d3-5fd8f72e70cc
author: rothja
ms.author: jroth
ms.openlocfilehash: 63b71709bd3c6f07c1dad3d7f3c471ecd267b202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171301"
---
# <a name="delete-method-ado-parameters-collection"></a>Delete 方法（ADO 参数集合）
从 [参数](../../../ado/reference/ado-api/parameters-collection-ado.md) 集合中删除对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Parameters.Delete Index  
```  
  
#### <a name="parameters"></a>参数  
 *Index*  
 一个 **字符串** 值，其中包含要删除的对象的名称，或该对象在集合中 (索引) 的序号位置。  
  
## <a name="remarks"></a>备注  
 通过对集合使用 **Delete** 方法，您可以删除集合中的某个对象。 此方法仅可用于 [Command](../../../ado/reference/ado-api/command-object-ado.md)对象的 **Parameters** 集合。 在调用 **Delete** 方法时，必须使用 [参数](../../../ado/reference/ado-api/parameter-object.md)对象的 [Name](../../../ado/reference/ado-api/name-property-ado.md)属性或其集合索引-对象变量不是有效的参数。  
  
## <a name="applies-to"></a>应用于  
 [参数集合 (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 字段集合的 Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [ADO 记录集 (Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [DeleteRecord 方法 (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
