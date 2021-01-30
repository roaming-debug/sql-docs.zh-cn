---
description: 项对象 (ADOX)
title: " (ADOX) 的密钥对象 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Key
helpviewer_keywords:
- Key object [ADOX]
ms.assetid: 55f116fe-4d56-4892-bffe-0cdd6fc727c9
author: rothja
ms.author: jroth
ms.openlocfilehash: 51c694c39004330dd780a2deea632eee186ad9c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169342"
---
# <a name="key-object-adox"></a>项对象 (ADOX)
表示数据库表中的主键或唯一键字段。  
  
## <a name="remarks"></a>备注  
 下面的代码创建一个新 **密钥**：  
  
```  
Dim obj As New Key  
```  
  
 使用 **Key** 对象的属性和集合，可以执行以下操作：  
  
-   标识具有 [Name](./name-property-adox.md) 属性的键。  
  
-   确定该键是 primary、foreign 还是 unique，并具有 [Type](./type-property-key-adox.md) 属性。  
  
-   使用 [columns](./columns-collection-adox.md) 集合访问键的数据库列。  
  
-   指定带有 [RelatedTable](./relatedtable-property-adox.md) 属性的相关表的名称。  
  
-   确定删除或更新包含 [DeleteRule](./deleterule-property-adox.md) 和 [UpdateRule](./updaterule-property-adox.md) 属性的主键时执行的操作。  
  
 本部分包含以下主题。  
  
-   [项对象属性、方法和事件](./key-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [项集合 (ADOX)](./keys-collection-adox.md)