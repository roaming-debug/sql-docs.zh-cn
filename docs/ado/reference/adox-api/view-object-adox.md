---
description: 视图对象 (ADOX)
title: View Object (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- View
helpviewer_keywords:
- View object [ADOX]
ms.assetid: 653421ce-7b94-43d0-9bc6-4900f8f2af45
author: rothja
ms.author: jroth
ms.openlocfilehash: 700b82c39352842cc20b017c90df686c95215586
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169123"
---
# <a name="view-object-adox"></a>视图对象 (ADOX)
表示一组筛选的记录或虚拟表。 与 ADO [命令](../ado-api/command-object-ado.md) 对象结合使用时， **视图** 对象可用于添加、删除或修改视图。  
  
## <a name="remarks"></a>备注  
 视图是从其他数据库表或视图创建的虚拟表。 **视图** 对象允许您创建一个视图，而无需知道或使用该提供程序的 "创建视图" 语法。  
  
 使用 **视图** 对象的属性，可以：  
  
-   标识具有 [Name](./name-property-adox.md) 属性的视图。  
  
-   指定可用于在 [命令](./command-property-adox.md)属性中添加、删除或修改视图的 ADO **命令** 对象。  
  
-   返回具有 [DateCreated](./datecreated-property-adox.md) 和 [DateModified](./datemodified-property-adox.md) 属性的日期信息。  
  
 本部分包含以下主题。  
  
-   [视图对象属性、方法和事件](./view-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [视图和字段集合示例 (VB) ](./views-and-fields-collections-example-vb.md)   
 [ (VB) 视图追加方法示例 ](./views-append-method-example-vb.md)   
 [Views 集合、CommandText 属性示例 (VB) ](./views-collection-commandtext-property-example-vb.md)   
 [Views Delete 方法示例 (VB) ](./views-delete-method-example-vb.md)   
 [视图集合 (ADOX)](./views-collection-adox.md)