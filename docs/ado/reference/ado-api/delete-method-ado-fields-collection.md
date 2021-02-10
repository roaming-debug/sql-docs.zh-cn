---
description: Delete 方法（ADO 字段集合）
title: " (ADO 字段集合) 的删除方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Fields20::Delete
- Fields20::raw_Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 25bedc25-c51c-4cab-96ce-930b959965d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cfc2784e0a6019e4a3aaee68cb18c54b0e52fc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034407"
---
# <a name="delete-method-ado-fields-collection"></a>Delete 方法（ADO 字段集合）
从 " [字段](../../../ado/reference/ado-api/fields-collection-ado.md) " 集合中删除一个对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Fields.Delete Field  
```  
  
#### <a name="parameters"></a>参数  
 字段  
 指定要删除的 [字段](../../../ado/reference/ado-api/field-object.md)对象的 **变量**。 此参数可以是 **字段** 对象的名称，也可以是 **字段** 对象本身的序号位置。  
  
## <a name="remarks"></a>备注  
 对打开的 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)调用 **Delete** 方法会导致运行时错误。  
  
## <a name="applies-to"></a>应用于  
 [字段集合 (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 参数集合的 Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [ADO 记录集 (Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [DeleteRecord 方法 (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
