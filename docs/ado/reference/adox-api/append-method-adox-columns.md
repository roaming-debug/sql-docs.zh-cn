---
description: Append 方法（ADOX 列）
title: ) 的 ADOX 列 (追加方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Columns::raw_Append
- Columns::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 7a46d23c-efef-4ec7-815d-cd3ac86787dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 7cf3b6e70bb7df0aff23d8d4d6c92b4fe6656e21
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050548"
---
# <a name="append-method-adox-columns"></a>Append 方法（ADOX 列）
向[Columns](./columns-collection-adox.md)集合添加一个新的[列](./column-object-adox.md)对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Columns.Append Column [,Type] [,DefinedSize]  
```  
  
#### <a name="parameters"></a>参数  
 *列*  
 要追加的 **列** 对象或者要创建并追加的列的名称。  
  
 类型  
 可选。 指定列的数据类型的 **Long** 值。 *类型* 参数对应于 **列** 对象的 [type](./type-property-column-adox.md)属性。  
  
 *DefinedSize*  
 可选。 指定列的大小的 **Long** 值。 *DefinedSize* 参数对应于 **列** 对象的 [DefinedSize](./definedsize-property-adox.md)属性。  
  
> [!NOTE]
>  如果 **列** 不存在于已追加到 [Tables](./tables-collection-adox.md)集合的 [表](./table-object-adox.md)中，则当向 [索引](./index-object-adox.md)的 **Columns** 集合追加 **列** 时，将发生错误。  
  
## <a name="applies-to"></a>应用于  
 [列集合 (ADOX)](./columns-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [列和表追加方法、Name 属性示例 (VB) ](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog 属性示例 (VB) ](./parentcatalog-property-example-vb.md)   
 [将方法追加 (ADOX 组) ](./append-method-adox-groups.md)   
 [Append 索引 (Append 方法) ](./append-method-adox-indexes.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [附加方法 (ADOX 过程) ](./append-method-adox-procedures.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [ADOX 用户 (追加方法) ](./append-method-adox-users.md)   
 [Append 方法（ADOX 视图）](./append-method-adox-views.md)