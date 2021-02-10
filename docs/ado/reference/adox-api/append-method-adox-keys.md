---
description: Append 方法（ADOX 项）
title: 追加方法 (ADOX 密钥) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Keys::Append
- Keys::raw_Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 215a5391-f422-42ec-99ea-4e6fbb5d3d64
author: rothja
ms.author: jroth
ms.openlocfilehash: 573bfb24f68896f647f4c0d9ff5867ee1d38e5fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050508"
---
# <a name="append-method-adox-keys"></a>Append 方法（ADOX 项）
向[Keys](./keys-collection-adox.md)集合添加新的[Key](./key-object-adox.md)对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Keys.Append Key [,KeyType] [,Column] [,RelatedTable] [,RelatedColumn]  
```  
  
#### <a name="parameters"></a>参数  
 *Key*  
 要追加的 **密钥** 对象或要创建和追加的密钥的名称。  
  
 *KeyType*  
 可选。 指定键类型的 **Long** 值。 *密钥* 参数对应于 **密钥** 对象的 [Type](./type-property-key-adox.md)属性。  
  
 *列*  
 可选。 一个 **字符串** 值，该值指定要编制索引的列的名称。 *Columns* 参数对应于 [列](./column-object-adox.md)对象的 [Name](./name-property-adox.md)属性的值。  
  
 *RelatedTable*  
 可选。 一个 **字符串** 值，该值指定相关表的名称。 *RelatedTable* 参数对应于 [Table](./table-object-adox.md)对象的 **Name** 属性的值。  
  
 *RelatedColumn*  
 可选。 一个 **字符串** 值，该值指定外键的相关列的名称。 *RelatedColumn* 参数对应于 [列](./column-object-adox.md)对象的 **Name** 属性的值。  
  
## <a name="remarks"></a>备注  
 *Columns* 参数可以采用列的名称或列名的数组。  
  
## <a name="applies-to"></a>应用于  
 [项集合 (ADOX)](./keys-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [将方法追加 (ADOX 组) ](./append-method-adox-groups.md)   
 [Append 索引 (Append 方法) ](./append-method-adox-indexes.md)   
 [附加方法 (ADOX 过程) ](./append-method-adox-procedures.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [ADOX 用户 (追加方法) ](./append-method-adox-users.md)   
 [Append 方法（ADOX 视图）](./append-method-adox-views.md)