---
description: Count 属性 (ADO)
title: ADO)  (计数属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Count
helpviewer_keywords:
- Count property [ADO]
ms.assetid: da9ccd1f-d402-41a2-940c-45556fc5340d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a99489ca105a3661fb17c7ea8a62ee831e899ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167665"
---
# <a name="count-property-ado"></a>Count 属性 (ADO)
指示集合中的对象数。  
  
## <a name="return-value"></a>返回值  
 返回一个 **长整型** 值。  
  
## <a name="remarks"></a>备注  
 使用 **Count** 属性来确定给定集合中有多少个对象。  
  
 由于集合成员的编号以零开始，因此，您始终应始终从零个成员开始编写循环，并以 **Count** 属性的值减1结束。 如果使用的是 Microsoft Visual Basic 并且想要循环遍历集合的成员而不检查 **Count** 属性，请使用 **For Each .。。下一个** 命令。  
  
 如果 **Count** 属性为零，则集合中不存在任何对象。  
  
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
 [Count 属性示例 (VB) ](./count-property-example-vb.md)   
 ["计数" 属性示例 (VC + +) ](./count-property-example-vc.md)   
 [Refresh 方法 (ADO)](./refresh-method-ado.md)