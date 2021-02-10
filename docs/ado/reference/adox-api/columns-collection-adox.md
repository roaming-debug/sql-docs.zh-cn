---
description: 列集合 (ADOX)
title: 列集合 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index::Columns
- Table::Columns
- Key::Columns
- Columns
helpviewer_keywords:
- Columns collection [ADOX]
ms.assetid: 23b9fea8-4f76-4a51-95ce-1a6ce4560b34
author: rothja
ms.author: jroth
ms.openlocfilehash: 81a7e0c521b96b737c44e3c88f2b14be42176a31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050267"
---
# <a name="columns-collection-adox"></a>列集合 (ADOX)
包含表、索引或键的所有 [列](./column-object-adox.md) 对象。  
  
## <a name="remarks"></a>备注  
 **列** 集合的 [APPEND](./append-method-adox-columns.md)方法对于 ADOX 是唯一的。 方法：  
  
-   使用 **Append** 方法向集合中添加一个新列。  
  
 其余属性和方法对于 ADO 集合是标准的。 方法：  
  
-   使用 [Item](../ado-api/item-property-ado.md) 属性访问集合中的列。  
  
-   返回集合中包含的 [Count](../ado-api/count-property-ado.md) 属性的列数。  
  
-   使用 [Delete](./delete-method-adox-collections.md) 方法从集合中删除列。  
  
-   更新集合中的对象，以反映包含 [Refresh](../ado-api/refresh-method-ado.md) 方法的当前数据库的架构。  
  
> [!NOTE]
>  如果 **列** 不存在于已追加到 [Tables](./tables-collection-adox.md)集合的 [表](./table-object-adox.md)中，则当向 [索引](./index-object-adox.md)的 **Columns** 集合追加 **列** 时，将发生错误。  
  
 本部分包含以下主题。  
  
-   [列集合属性、方法和事件](./columns-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [列和表追加方法、Name 属性示例 (VB) ](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [连接关闭方法，表类型属性示例 (VB) ](./connection-close-method-table-type-property-example-vb.md)   
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog 属性示例 (VB) ](./parentcatalog-property-example-vb.md)   
 [SortOrder 属性示例 (VB) ](./sortorder-property-example-vb.md)   
 [列对象 (ADOX)](./column-object-adox.md)