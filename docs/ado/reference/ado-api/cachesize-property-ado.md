---
description: CacheSize 属性 (ADO)
title: " (ADO) 的 CacheSize 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CacheSize
helpviewer_keywords:
- CacheSize property [ADO]
ms.assetid: 49dc9a49-af7b-433b-be36-7a14ca984fb7
author: rothja
ms.author: jroth
ms.openlocfilehash: 9aef38a384c352648d5487ee9dae9a79d605e8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167822"
---
# <a name="cachesize-property-ado"></a>CacheSize 属性 (ADO)
指示 [记录集中](./recordset-object-ado.md) 缓存到内存中的记录集的记录数。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个必须大于0的 **长整型** 值。 默认值为 1。  
  
## <a name="remarks"></a>备注  
 使用 **CacheSize** 属性可控制从提供程序在本地内存中一次检索的记录数。 例如，如果 **CacheSize** 为10，则在第一次打开 **Recordset** 对象之后，提供程序会在本地内存中检索前10条记录。 当您在 **Recordset** 对象中移动时，提供程序将从本地内存缓冲区返回数据。 一旦移过缓存中的最后一条记录，提供程序就会将数据源中的后10个记录检索到缓存中。  
  
> [!NOTE]
>  **CacheSize** 基于) 的 **记录集** 对象的 **Properties** 集合中 (的 **最大打开行** 特定于提供程序的属性。 不能将 **CacheSize** 设置为大于 **最大打开行数** 的值。 若要修改提供程序可以打开的行数，请设置 " **最大打开行** 数"。  
  
 **CacheSize** 的值可以在 **Recordset** 对象的生命周期内进行调整，但更改此值只会影响在后续从数据源中检索后缓存中的记录数。 仅更改属性值不会更改缓存的当前内容。  
  
 如果要检索的记录数少于 **CacheSize** 指定的数目，则提供程序将返回其余记录并且不会发生错误。  
  
 不允许使用零的 **CacheSize** 设置，并且将返回错误。  
  
 从缓存检索的记录不反映其他用户对源数据所做的并发更改。 若要强制执行所有缓存数据的更新，请使用 [Resync](./resync-method.md) 方法。  
  
 如果将 **CacheSize** 设置为大于1的值，则在检索记录后发生删除操作时，导航方法 ([Move](./move-method-ado.md)、 [MoveFirst、MoveLast、MoveNext 和 MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) 可能会导致导航到已删除的记录。 初始提取后，在您尝试从已删除的行访问数据值之前，后续删除将不会反映在您的数据缓存中。 但是，将 **CacheSize** 设置为 one 可消除此问题，因为无法提取删除的行。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [CacheSize 属性示例 (VB) ](./cachesize-property-example-vb.md)   
 [CacheSize 属性示例 (VC + +) ](./cachesize-property-example-vc.md)   
 [CacheSize 属性示例 (JScript)](./cachesize-property-example-jscript.md)