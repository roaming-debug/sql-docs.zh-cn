---
description: Name 属性 (ADOX)
title: 名称属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::PutName
- _Table::GetName
- _Key::Name
- _Key::get_Name
- _Column::GetName
- _Index::Name
- _Index::put_Name
- _Column::PutName
- _Key::put_Name
- _Table::put_Name
- _User25::PutName
- _Index::get_Name
- _Column::get_Name
- _Group25::Name
- _Group25::get_Name
- _Column::Name
- _User25::get_Name
- _Table::Name
- _Group25::GetName
- _Index::PutName
- _Column::put_Name
- _Key::GetName
- _Table::get_Name
- _User25::Name
- _User25::put_Name
- _Index::GetName
- _User25::GetName
helpviewer_keywords:
- Name property [ADOX]
ms.assetid: 81b92baf-b6b9-4f4e-9f33-4503795518cd
author: rothja
ms.author: jroth
ms.openlocfilehash: 83ef51697cd3ad1ad1a01355baead5d90788813f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171996"
---
# <a name="name-property-adox"></a>Name 属性 (ADOX)
指示对象的名称。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值。  
  
## <a name="remarks"></a>备注  
 名称在集合中不必是唯一的。  
  
 **Name** 属性对 [列](./column-object-adox.md)、[组](./group-object-adox.md)、[键](./key-object-adox.md)、[索引](./index-object-adox.md)、[表](./table-object-adox.md)和 [用户](./user-object-adox.md)对象是可读/写的。 " **名称** " 属性在 " [目录](./catalog-object-adox.md)"、" [过程](./procedure-object-adox.md)" 和 " [视图](./view-object-adox.md) " 对象上是只读的。  
  
 对于读/写对象 (**列**、 **组**、 **键**、 **索引**、 **表** 和 **用户** 对象) ，默认值为空字符串 ( "" ) 。  
  
> [!NOTE]
>  对于键，此属性在已追加到集合的 **键** 对象上是只读的。 对于表，对于已追加到集合的 **表** 对象，此属性是只读的。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [列对象 (ADOX)](./column-object-adox.md)  
        [组对象 (ADOX)](./group-object-adox.md)  
        [索引对象 (ADOX)](./index-object-adox.md)  
    :::column-end:::
    :::column:::
        [项对象 (ADOX)](./key-object-adox.md)  
        [过程对象 (ADOX)](./procedure-object-adox.md)  
        [属性对象 (ADO)](../ado-api/property-object-ado.md)  
    :::column-end:::
    :::column:::
        [表对象 (ADOX)](./table-object-adox.md)  
        [用户对象 (ADOX)](./user-object-adox.md)  
        [视图对象 (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [列和表追加方法、Name 属性示例 (VB) ](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog 属性示例 (VB)](./parentcatalog-property-example-vb.md)