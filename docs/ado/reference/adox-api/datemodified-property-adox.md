---
description: DateModified 属性 (ADOX)
title: DateModified 属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::get_DateModified
- _Table::DateModified
- _Table::GetDateModified
helpviewer_keywords:
- DateModified property [ADOX]
ms.assetid: fed09266-1547-4bda-9088-c254d81cc738
author: rothja
ms.author: jroth
ms.openlocfilehash: ef5fe0610059d8dff38732d356cbd8b1562ed7d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054282"
---
# <a name="datemodified-property-adox"></a>DateModified 属性 (ADOX)
指示上次修改对象的日期。  
  
## <a name="return-values"></a>返回值  
 返回指定修改日期的 **变量** 值。 如果提供程序不支持 **DateModified** ，则该值为 null。  
  
## <a name="remarks"></a>备注  
 新追加的对象的 **DateModified** 属性为 null。 追加新的 [视图](./view-object-adox.md)或 [过程](./procedure-object-adox.md)之后，必须调用 [视图](./views-collection-adox.md)或 [过程](./procedures-collection-adox.md)集合的 [Refresh](../ado-api/refresh-method-ado.md)方法，以获取 **DateModified** 属性的值。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [过程对象 (ADOX)](./procedure-object-adox.md)  
    :::column-end:::
    :::column:::
        [表对象 (ADOX)](./table-object-adox.md)  
    :::column-end:::
    :::column:::
        [视图对象 (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [DateCreated 和 DateModified 属性示例 (VB) ](./datecreated-and-datemodified-properties-example-vb.md)   
 [DateCreated 属性 (ADOX)](./datecreated-property-adox.md)