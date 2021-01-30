---
description: 过程集合 (ADOX)
title: 过程集合 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedures
- Catalog::Procedures
helpviewer_keywords:
- Procedures collection [ADOX]
ms.assetid: dc7a38e1-93b9-4034-9af2-ff419e8fb2a3
author: rothja
ms.author: jroth
ms.openlocfilehash: c77c3eccd6cc2e61898b1619ec6255f65227384b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169245"
---
# <a name="procedures-collection-adox"></a>过程集合 (ADOX)
包含目录的所有 [过程](./procedure-object-adox.md) 对象。  
  
## <a name="remarks"></a>备注  
 **过程** 集合的 [APPEND](./append-method-adox-procedures.md)方法对于 ADOX 是唯一的。 可以执行以下操作：  
  
-   使用 **Append** 方法向集合中添加一个新过程。  
  
 其余属性和方法对于 ADO 集合是标准的。 可以执行以下操作：  
  
-   使用 [Item](../ado-api/item-property-ado.md) 属性访问集合中的过程。  
  
-   返回集合中包含 [Count](../ado-api/count-property-ado.md) 属性的过程数。  
  
-   使用 [Delete](./delete-method-adox-collections.md) 方法从集合中删除过程。  
  
-   更新集合中的对象，以反映包含 [Refresh](../ado-api/refresh-method-ado.md) 方法的当前数据库架构。  
  
 本部分包含以下主题。  
  
-   [索引集合属性、方法和事件](./indexes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [Command 和 CommandText 属性示例 (VB) ](./command-and-commandtext-properties-example-vb.md)   
 [参数集合、Command 属性示例 (VB) ](./parameters-collection-command-property-example-vb.md)   
 [步骤 Append 方法示例 (VB) ](./procedures-append-method-example-vb.md)   
 [过程 Delete 方法示例 (VB) ](./procedures-delete-method-example-vb.md)   
 [过程 Refresh 方法示例 (VB) ](./procedures-refresh-method-example-vb.md)   
 [过程集合属性、方法和事件](./procedures-collection-properties-methods-and-events.md)   
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [过程对象 (ADOX)](./procedure-object-adox.md)