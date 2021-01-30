---
description: 视图集合 (ADOX)
title: Views 集合 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Views
- Views
helpviewer_keywords:
- Views collection [ADOX]
ms.assetid: a55d380c-2b7b-4b57-af74-8ba0b3de0db9
author: rothja
ms.author: jroth
ms.openlocfilehash: 51a32bb1952e5c8100ed1d13cb7ba72b99e8994d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169097"
---
# <a name="views-collection-adox"></a>视图集合 (ADOX)
包含目录的所有 [视图](./view-object-adox.md) 对象。  
  
## <a name="remarks"></a>备注  
 **Views** 集合的 [APPEND](./append-method-adox-views.md)方法对于 ADOX 是唯一的。 可以执行以下操作：  
  
-   使用 **Append** 方法将新视图添加到集合。  
  
 其余属性和方法对于 ADO 集合是标准的。 可以执行以下操作：  
  
-   使用 [Item](../ado-api/item-property-ado.md) 属性访问集合中的视图。  
  
-   返回集合中包含 [Count](../ado-api/count-property-ado.md) 属性的视图的数目。  
  
-   使用 [Delete](./delete-method-adox-collections.md) 方法从集合中删除视图。  
  
-   更新集合中的对象，以反映包含 [Refresh](../ado-api/refresh-method-ado.md) 方法的当前数据库架构。  
  
 本部分包含以下主题。  
  
-   [视图集合属性、方法和事件](./views-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [视图和字段集合示例 (VB) ](./views-and-fields-collections-example-vb.md)   
 [ (VB) 视图追加方法示例 ](./views-append-method-example-vb.md)   
 [Views 集合、CommandText 属性示例 (VB) ](./views-collection-commandtext-property-example-vb.md)   
 [Views Delete 方法示例 (VB) ](./views-delete-method-example-vb.md)   
 [ (VB) Views Refresh 方法示例 ](./views-refresh-method-example-vb.md)   
 [目录对象 (ADOX) ](./catalog-object-adox.md)   
 [视图对象 (ADOX)](./view-object-adox.md)