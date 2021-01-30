---
description: 属性对象 (ADOX)
title: 属性对象 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Property object [ADOX]
ms.assetid: 6a56def6-dbe6-4ccc-a491-8d076889f019
author: rothja
ms.author: jroth
ms.openlocfilehash: ff7c450ea1812ee5e0a9441640e2884b0394ac43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164065"
---
# <a name="property-object-adox"></a>属性对象 (ADOX)
表示 ADOX 对象的特性。  
  
## <a name="remarks"></a>备注  
 ADOX 对象具有两种类型的属性：内置和动态。  
  
 内置属性是使用 MyObject 语法立即可用于任何新对象的属性。 它们不会在对象的 [Properties 集合](../ado-api/properties-collection-ado.md)中显示为属性对象，因此虽然您可以更改其值，但不能修改它们的特征。  
  
 动态属性由基础数据提供程序定义，并出现在相应 ADOX 对象的 Properties 集合中。  只能通过集合引用动态属性，使用 MyObject (0) 或 MyObject ( "Name" ) 语法。  
  
 不能删除任何一种属性。  
  
 动态属性对象具有自己的四个内置属性：  
  
 [Name](../ado-api/name-property-ado.md)属性是用于标识属性的字符串。  
  
 [Type](../ado-api/type-property-ado.md)属性是指定属性数据类型的整数。  
  
 [值](../ado-api/value-property-ado.md)属性是包含属性设置的变量。 值是属性对象的默认属性。  
  
 " [属性](../ado-api/attributes-property-ado.md) " 属性是一个 long 值，指示特定于提供程序的属性的特征。  
  
 本部分包含以下主题。  
  
-   [ADOX 属性对象属性、方法和事件](./adox-property-object-properties-methods-and-events.md)