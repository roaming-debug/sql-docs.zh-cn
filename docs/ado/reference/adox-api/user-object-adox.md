---
description: 用户对象 (ADOX)
title: 用户对象 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- User
helpviewer_keywords:
- User object [ADOX]
ms.assetid: f68e32ce-ef7c-407d-bdb5-d280947ae0e2
author: rothja
ms.author: jroth
ms.openlocfilehash: c5ca59cb5900643202c2d541072c48b275b5b5df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169141"
---
# <a name="user-object-adox"></a>用户对象 (ADOX)
表示在受保护的数据库中具有访问权限的用户帐户。  
  
## <a name="remarks"></a>备注  
 [目录](./catalog-object-adox.md)的[用户](./users-collection-adox.md)集合表示所有目录的用户。 [组](./group-object-adox.md)的 **用户** 集合仅表示特定组的用户。  
  
 使用 **用户** 对象的属性、集合和方法，您可以：  
  
-   标识具有 [Name](./name-property-adox.md) 属性的用户。  
  
-   使用 [ChangePassword](./changepassword-method-adox.md) 方法更改用户的密码。  
  
-   使用 [GetPermissions](./getpermissions-method-adox.md) 和 [SetPermissions](./setpermissions-method-adox.md) 方法确定用户是否具有读取、写入或删除权限。  
  
-   使用 [groups](./groups-collection-adox.md) 集合访问用户所属的组。  
  
-   使用 [properties](../ado-api/properties-collection-ado.md) 集合访问特定于提供程序的属性。  
  
-   确定用户的 [ParentCatalog](./parentcatalog-property-adox.md) 。  
  
 本部分包含以下主题。  
  
-   [用户对象属性、方法和事件](./user-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [GetPermissions 和 SetPermissions 方法示例 (VB) ](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [组集合 (ADOX) ](./groups-collection-adox.md)   
 [用户集合 (ADOX)](./users-collection-adox.md)