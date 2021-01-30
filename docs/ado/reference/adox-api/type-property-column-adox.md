---
description: Type 属性（列）(ADOX)
title: Type 属性 (列)  (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::Type
- _Column::GetType
- _Column::get_Type
- _Column::put_Type
- _Column::PutType
helpviewer_keywords:
- Type property [ADOX]
ms.assetid: 5c6718b6-f728-478a-8afb-5d17b0a91d1f
author: rothja
ms.author: jroth
ms.openlocfilehash: 77d7bcf4ddc6beb29dc04513952050c8ba042c55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163995"
---
# <a name="type-property-column-adox"></a>Type 属性（列）(ADOX)
指示列的数据类型。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回可以是 [DataTypeEnum](../ado-api/datatypeenum.md)常量之一的 **Long** 值。 默认值为 **adVarWChar**。  
  
## <a name="remarks"></a>备注  
 此属性是可读/写的，直到将 [列](./column-object-adox.md) 对象追加到集合或其他对象（该对象是只读的）。  
  
## <a name="applies-to"></a>应用于  
 [列对象 (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [ParentCatalog 属性示例 (VB) ](./parentcatalog-property-example-vb.md)   
 [ (ADOX) 类型属性 (键) ](./type-property-key-adox.md)   
 [Type 属性（表）(ADOX)](./type-property-table-adox.md)