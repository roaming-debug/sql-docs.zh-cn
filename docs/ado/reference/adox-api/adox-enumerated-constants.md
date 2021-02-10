---
description: ADOX 枚举常量
title: ADOX 枚举常量 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- enumerated constants [ADOX]
ms.assetid: 9d91f511-d46f-44ef-97ef-77bf93836186
author: rothja
ms.author: jroth
ms.openlocfilehash: 227e66568fcc140c5a14a23af7a6471a710c23c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050628"
---
# <a name="adox-enumerated-constants"></a>ADOX 枚举常量
为了协助调试，ADOX 枚举常量会列出每个常量的值。 不过，此值是纯粹的建议，可能会因 ADOX 的一个版本而发生变化。 你的代码只应依赖于枚举常量的名称，而不是实际值。  
  
 定义下面的枚举常数。  
  
|枚举|描述|  
|-----------------|-----------------|  
|[ActionEnum](./actionenum.md)|指定在调用 **SetPermissions** 时要执行的操作的类型。|  
|[AllowNullsEnum](./allownullsenum.md)|指定是否为具有 null 值的记录编制索引。|  
|[ColumnAttributesEnum](./columnattributesenum.md)|指定 **列** 的特征。|  
|[DataTypeEnum](../ado-api/datatypeenum.md)|指定 **字段**、 **参数** 或 **属性** 的数据类型。|  
|[InheritTypeEnum](./inherittypeenum.md)|指定对象如何继承具有 **SetPermissions** 的权限集。|  
|[KeyTypeEnum](./keytypeenum.md)|指定 **键** 的类型： primary、foreign 或 unique。|  
|[ObjectTypeEnum](./objecttypeenum.md)|指定要为其设置权限或所有权的数据库对象的类型。|  
|[RightsEnum](./rightsenum.md)|指定组或用户对对象的权限。|  
|[RuleEnum](./ruleenum.md)|指定删除 **密钥** 时要遵循的规则。|  
|[SortOrderEnum](./sortorderenum.md)|指定索引列的排序顺序。|  
  
## <a name="see-also"></a>另请参阅  
 [ADOX API 参考](./adox-object-model.md)   
 [数据定义语言和安全性的 ADO 扩展 (ADOX)](../../guide/extensions/ado-extensions-for-data-definition-language-and-security-adox.md)