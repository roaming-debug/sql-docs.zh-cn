---
description: Append 方法（ADOX 索引）
title: 追加方法 (ADOX 索引) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Indexes::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6695769f-275b-4b70-81bd-1a5f7d74926c
author: rothja
ms.author: jroth
ms.openlocfilehash: feb259809798e3260e4e201c4658bfbc1455f168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050538"
---
# <a name="append-method-adox-indexes"></a>Append 方法（ADOX 索引）
将新 [索引](./index-object-adox.md) 对象添加到 [索引](./indexes-collection-adox.md) 集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
Indexes.Append Index [,Columns]  
```  
  
#### <a name="parameters"></a>参数  
 *Index*  
 要追加的 **索引** 对象或要创建并追加的索引的名称。  
  
 *“列”*  
 可选。 一个 **变量** 值，指定要编制索引的列)  ( (s) 的名称。 *Columns* 参数对应于 [列](./column-object-adox.md)对象的 [Name](./name-property-adox.md)属性的值 (s) 。  
  
## <a name="remarks"></a>备注  
 *Columns* 参数可以采用列的名称或列名的数组。  
  
 如果提供程序不支持创建索引，则会出现错误。  
  
## <a name="applies-to"></a>应用于  
 [索引集合 (ADOX)](./indexes-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB 的索引 Append 方法示例) ](./indexes-append-method-example-vb.md)   
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [将方法追加 (ADOX 组) ](./append-method-adox-groups.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [附加方法 (ADOX 过程) ](./append-method-adox-procedures.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [ADOX 用户 (追加方法) ](./append-method-adox-users.md)   
 [Append 方法（ADOX 视图）](./append-method-adox-views.md)