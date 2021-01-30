---
description: DeleteRule 属性 (ADOX)
title: DeleteRule 属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Key::put_DeleteRule
- _Key::DeleteRule
- _Key::GetDeleteRule
- _Key::PutDeleteRule
- _Key::get_DeleteRule
helpviewer_keywords:
- DeleteRule property [ADOX]
ms.assetid: 87bd4c0a-cae3-4007-a939-4193acaa00ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 4b591759ee1964ed1e5962558d8590d69957d3be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169430"
---
# <a name="deleterule-property-adox"></a>DeleteRule 属性 (ADOX)
指示在删除主键时执行的操作。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置并返回一个 **Long** 值，该值可以是 [RuleEnum](./ruleenum.md) 常数之一。 默认值为 **adRINone**。  
  
## <a name="remarks"></a>备注  
 对于已追加到集合的 [键](./key-object-adox.md) 对象，此属性是只读的。  
  
## <a name="applies-to"></a>应用于  
 [项对象 (ADOX)](./key-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [DeleteRule 属性示例 (VB)](./deleterule-property-example-vb.md)