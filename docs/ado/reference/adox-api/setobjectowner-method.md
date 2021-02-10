---
description: SetObjectOwner 方法
title: SetObjectOwner 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Catalog::SetObjectOwner
- _Catalog::raw_SetObjectOwner
helpviewer_keywords:
- SetObjectOwner method [ADOX]
ms.assetid: e5170a37-9d6e-43db-bfb6-9b6631fa3048
author: rothja
ms.author: jroth
ms.openlocfilehash: 2d75966ce8d5c1f9fddea9c09ff254eca9106e62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053802"
---
# <a name="setobjectowner-method"></a>SetObjectOwner 方法
指定 [目录](./catalog-object-adox.md)中对象的所有者。  
  
## <a name="syntax"></a>语法  
  
```  
  
Catalog.SetObjectOwner ObjectName, ObjectType, OwnerName [,ObjectTypeId]  
```  
  
#### <a name="parameters"></a>参数  
 *ObjectName*  
 一个 **字符串** 值，该值指定要为其指定所有者的对象的名称。  
  
 *ObjectType*  
 一个 **长整型** 值，可以是指定所有者类型的 [ObjectTypeEnum](./objecttypeenum.md) 常量之一。  
  
 *OwnerName*  
 一个 **字符串** 值，该值指定要拥有对象的 [用户](./user-object-adox.md)或 [组](./group-object-adox.md)的 [名称](./name-property-adox.md)。  
  
 *ObjectTypeId*  
 可选。 一个 **变量** 值，指定 OLE DB 规范未定义的提供程序对象类型的 GUID。 如果 *ObjectType* 设置为 **adPermObjProviderSpecific**，则此参数是必需的;否则，不使用此方法。  
  
## <a name="remarks"></a>备注  
 如果提供程序不支持指定对象所有者，将出现错误。  
  
## <a name="applies-to"></a>应用于  
 [目录对象 (ADOX)](./catalog-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [GetObjectOwner 和 SetObjectOwner 方法示例 (VB) ](./getobjectowner-and-setobjectowner-methods-example-vb.md)   
 [GetObjectOwner 方法 (ADOX)](./getobjectowner-method-adox.md)