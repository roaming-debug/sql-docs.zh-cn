---
description: UpdateRule 属性 (ADOX)
title: UpdateRule 属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Key::GetUpdateRule
- _Key::get_UpdateRule
- _Key::PutUpdateRule
- _Key::put_UpdateRule
- _Key::UpdateRule
helpviewer_keywords:
- UpdateRule property [ADOX]
ms.assetid: f4e21060-40cb-4790-8611-4086a092dda2
author: rothja
ms.author: jroth
ms.openlocfilehash: 395fe27432e33eee9329c202f4cc95fe93949bdc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053438"
---
# <a name="updaterule-property-adox"></a>UpdateRule 属性 (ADOX)
指示在 [更新主键时](./key-object-adox.md) 执行的操作。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置并返回一个 **Long** 值，该值可以是 [RuleEnum](./ruleenum.md) 常数之一。 默认值为 **adRINone**。  
  
## <a name="remarks"></a>备注  
 对于已追加到集合的 [键](./key-object-adox.md) 对象，此属性是只读的。  
  
## <a name="applies-to"></a>应用于  
 [项对象 (ADOX)](./key-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [项 Append 方法、项 Type、RelatedColumn、RelatedTable 和 UpdateRule 属性示例 (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)