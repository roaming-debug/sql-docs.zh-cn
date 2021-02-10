---
description: Item 属性 (ADO)
title: ADO)  (项属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parameters::GetItem
- Indexes::GetItem
- Parameters::Item
- Tables::Item
- Procedures::Item
- Users::GetItem
- Tables::GetItem
- Procedures::GetItem
- Users::get_Item
- Users::Item
- Views::GetItem
- Groups::Item
- Groups::get_Item
- Columns::Item
- Indexes::Item
- Fields15::GetItem
- Columns::GetItem
- Fields::Item
- Indexes::get_Item
- Columns::get_Item
- Fields15::Item
- Views::get_Item
- Groups::GetItem
- Errors::get_Item
- Fields15::get_Item
- Tables::get_Item
- Views::Item
- Errors::GetItem
- Parameters::get_Item
- Errors::Item
- Procedures::get_Item
helpviewer_keywords:
- Item property [ADO]
ms.assetid: e11484bb-c5c7-42d8-9bb8-21572125d727
author: rothja
ms.author: jroth
ms.openlocfilehash: 3e87e7ae8592afc67b049fb25c4e66392f45eb89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041897"
---
# <a name="item-property-ado"></a>Item 属性 (ADO)
按名称或序号指示集合中的特定成员。  
  
## <a name="syntax"></a>语法  
  
```  
Set object = collection.Item ( Index )  
```  
  
## <a name="return-value"></a>返回值  
 返回对象引用。  
  
## <a name="parameters"></a>参数  
 *Index*  
 **变量** 表达式，计算结果为集合中对象的名称或序号。  
  
## <a name="remarks"></a>备注  
 使用 **Item** 属性返回集合中的特定对象。 如果 **项** 在与 *Index* 参数对应的集合中找不到对象，则会发生错误。 此外，某些集合不支持命名对象;对于这些集合，必须使用序号引用。  
  
 **Item** 属性是所有集合的默认属性;因此，以下语法形式可互换：  
  
```  
collection.Item (Index)  
collection (Index)  
```  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [轴集合 (ADO MD)](../ado-md-api/axes-collection-ado-md.md)  
        [列集合 (ADOX)](../adox-api/columns-collection-adox.md)  
        [CubeDefs 集合 (ADO MD)](../ado-md-api/cubedefs-collection-ado-md.md)  
        [维度集合 (ADO MD)](../ado-md-api/dimensions-collection-ado-md.md)  
        [错误集合 (ADO)](./errors-collection-ado.md)  
        [字段集合 (ADO)](./fields-collection-ado.md)  
        [组集合 (ADOX)](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [层次结构集合 (ADO MD)](../ado-md-api/hierarchies-collection-ado-md.md)  
        [索引集合 (ADOX)](../adox-api/indexes-collection-adox.md)  
        [项集合 (ADOX)](../adox-api/keys-collection-adox.md)  
        [级别集合 (ADO MD)](../ado-md-api/levels-collection-ado-md.md)  
        [成员集合 (ADO MD)](../ado-md-api/members-collection-ado-md.md)  
        [参数集合 (ADO)](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [位置集合 (ADO MD)](../ado-md-api/positions-collection-ado-md.md)  
        [过程集合 (ADOX)](../adox-api/procedures-collection-adox.md)  
        [属性集合 (ADO)](./properties-collection-ado.md)  
        [表集合 (ADOX)](../adox-api/tables-collection-adox.md)  
        [用户集合 (ADOX)](../adox-api/users-collection-adox.md)  
        [视图集合 (ADOX)](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [Item 属性示例 (VB) ](./item-property-example-vb.md)   
 [Item 属性示例 (VC++)](./item-property-example-vc.md)