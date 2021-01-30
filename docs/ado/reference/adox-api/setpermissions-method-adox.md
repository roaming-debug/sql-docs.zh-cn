---
description: SetPermissions 方法 (ADOX)
title: SetPermissions 方法 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- User25::SetPermissions
- User25::raw_SetPermissions
- _Group25::SetPermissions
- _Group25::raw_SetPermissions
helpviewer_keywords:
- SetPermissions method [ADOX]
ms.assetid: b7f925d7-b05c-4376-bb49-f8d2c17b8b24
author: rothja
ms.author: jroth
ms.openlocfilehash: 3c5db51dc0af4f8e86fbaa68b20dbd82a327c038
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164033"
---
# <a name="setpermissions-method-adox"></a>SetPermissions 方法 (ADOX)
指定 [组](./group-object-adox.md) 或 [用户](./user-object-adox.md) 对对象的权限。  
  
## <a name="syntax"></a>语法  
  
```  
  
GroupOrUser.SetPermissions Name, ObjectType, Action, Rights [, Inherit] [, ObjectTypeId]  
```  
  
#### <a name="parameters"></a>参数  
 *名称*  
 一个 **字符串** 值，该值指定要为其设置权限的对象的名称。  
  
 *ObjectType*  
 一个 **长整型** 值，可以是 [ObjectTypeEnum](./objecttypeenum.md) 常量之一，它指定要获取其权限的对象的类型。  
  
 *Action*  
 一个 **长整型** 值，可以是 [ActionEnum](./actionenum.md) 常量之一，用于指定设置权限时要执行的操作的类型。  
  
 权限  
 一个 **长整型** 值，它可以是一个或多个 [RightsEnum](./rightsenum.md) 常量的位掩码，用于指示要设置的权限。  
  
 *从此*  
 可选。 一个 **长整型** 值，它可以是 [InheritTypeEnum](./inherittypeenum.md) 常量之一，用于指定对象将如何继承这些权限。 默认值为 **adInheritNone**。  
  
 *ObjectTypeId*  
 可选。 一个 **变量** 值，指定 OLE DB 规范未定义的提供程序对象类型的 GUID。 如果 *ObjectType* 设置为 **adPermObjProviderSpecific**，则此参数是必需的;否则，不使用此方法。  
  
## <a name="remarks"></a>备注  
 如果提供程序不支持设置组或用户的访问权限，则会发生错误。  
  
> [!NOTE]
>  调用 **SetPermissions** 时，将操作设置为 **AdAccessRevoke** 会替代 *权限* 参数的任何设置。 如果希望 *权限* 参数中指定的权限生效，请不要将 *操作* 设置为 **adAccessRevoke** 。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [组对象 (ADOX)](./group-object-adox.md)  
    :::column-end:::
    :::column:::
        [用户对象 (ADOX)](./user-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [GetPermissions 和 SetPermissions 方法示例 (VB) ](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [GetPermissions 方法 (ADOX) ](./getpermissions-method-adox.md)   
 [Name 属性 (ADOX)](./name-property-adox.md)