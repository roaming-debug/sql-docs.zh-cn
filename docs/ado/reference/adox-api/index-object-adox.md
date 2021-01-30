---
description: 索引对象 (ADOX)
title: 索引对象 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index
helpviewer_keywords:
- Index object [ADOX]
ms.assetid: 6b9578c0-bc94-46b9-b801-c18e14b04b31
author: rothja
ms.author: jroth
ms.openlocfilehash: a11389324337d4e5a9deea347dda8ecd5cea3016
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172041"
---
# <a name="index-object-adox"></a>索引对象 (ADOX)
表示数据库表中的索引。  
  
## <a name="remarks"></a>备注  
 下面的代码创建一个新 **索引**：  
  
```  
Dim obj As New Index  
```  
  
 使用 **索引** 对象的属性和集合，可以：  
  
-   标识具有 [Name](./name-property-adox.md) 属性的索引。  
  
-   使用 [columns](./columns-collection-adox.md) 集合访问索引的数据库列。  
  
-   指定索引键是否必须具有[唯一属性。](./unique-property-adox.md)  
  
-   指定索引是否为具有 [PrimaryKey](./primarykey-property-adox.md) 属性的表的主键。  
  
-   指定在其索引字段中具有 null 值的记录是否具有具有 [IndexNulls](./indexnulls-property-adox.md) 属性的索引条目。  
  
-   指定是否用 [聚集](./clustered-property-adox.md) 属性对索引进行聚集。  
  
-   使用 [properties](../ado-api/properties-collection-ado.md) 集合访问提供程序特定的索引属性。  
  
> [!NOTE]
>  如果表中已追加到 [Tables](./tables-collection-adox.md)集合的 [表](./table-object-adox.md)对象中不 **存在列，** 则将 [该列](./column-object-adox.md)追加到 **索引** 的 **Columns** 集合中时，将出现错误。  
  
> [!NOTE]
>  数据访问接口可能不支持 **Index** 对象的所有属性。 如果为该访问接口不支持的属性设置值，则会出现错误。 对于新 **索引** 对象，当对象追加到集合时，将发生错误。 对于现有对象，在设置属性时会发生此错误。  
  
> [!NOTE]
>  创建 **索引** 对象时，是否存在可选属性的适当默认值并不保证提供程序支持该属性。 有关提供程序支持的属性的详细信息，请参阅提供程序文档。  
  
 本部分包含以下主题。  
  
-   [索引对象属性、方法和事件](./index-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB 的索引 Append 方法示例) ](./indexes-append-method-example-vb.md)   
 [IndexNulls 属性示例 (VB) ](./indexnulls-property-example-vb.md)   
 [PrimaryKey 和 Unique 属性示例 (VB) ](./primarykey-and-unique-properties-example-vb.md)   
 [SortOrder 属性示例 (VB) ](./sortorder-property-example-vb.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [索引集合 (ADOX) ](./indexes-collection-adox.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)