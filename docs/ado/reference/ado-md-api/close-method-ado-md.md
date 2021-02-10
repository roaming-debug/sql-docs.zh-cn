---
description: Close 方法 (ADO MD)
title: Close 方法 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Close
- Cellset::Close
helpviewer_keywords:
- Close method [ADO MD]
ms.assetid: a3aa594d-f9d4-4654-8625-ec20153ff5d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 0c334b7868162c5ec378ca0f4d981503622a639c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056732"
---
# <a name="close-method-ado-md"></a>Close 方法 (ADO MD)
关闭打开的单元集。  
  
## <a name="syntax"></a>语法  
  
```  
  
Cellset.Close  
```  
  
## <a name="remarks"></a>备注  
 使用 **close** 方法关闭 [单元集](./cellset-object-ado-md.md) 对象将释放关联的数据，包括任何相关 [单元](./cell-object-ado-md.md)、 [轴](./axis-object-ado-md.md)、 [位置](./position-object-ado-md.md)或 [成员](./member-object-ado-md.md) 对象中的数据。 关闭 **单元集** 不会将其从内存中删除;您可以更改其属性设置，稍后再次打开它。 若要从内存中完全消除对象，请将对象变量设置为 **Nothing**。  
  
 稍后可以调用 [Open](./open-method-ado-md.md) 方法，使用相同或其他源字符串重新打开 **单元集** 。 当 **单元格集** 对象关闭时，检索任何属性或调用引用基础数据或元数据的任何方法都将生成错误。  
  
## <a name="applies-to"></a>应用于  
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [Axis 对象 (ADO MD) ](./axis-object-ado-md.md)   
 [Cell Object (ADO MD) ](./cell-object-ado-md.md)   
 [成员对象 (ADO MD) ](./member-object-ado-md.md)   
 [Open 方法 (ADO MD) ](./open-method-ado-md.md)   
 [ (ADO MD 位置对象) ](./position-object-ado-md.md)   
 [State 属性 (ADO MD)](./state-property-ado-md.md)