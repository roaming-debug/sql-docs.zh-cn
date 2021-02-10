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
ms.openlocfilehash: 66c34576605f028401ec0cad513599ee264f542f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050137"
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