---
description: State 属性 (ADO MD)
title: State 属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- State
- Cellset::State
helpviewer_keywords:
- State property [ADO MD]
ms.assetid: 06d480ca-9eb6-4570-a45d-a73539bddd32
author: rothja
ms.author: jroth
ms.openlocfilehash: e2a9cfdd30460b790527c6c8a8eb5bcd7d05b7bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050848"
---
# <a name="state-property-ado-md"></a>State 属性 (ADO MD)
指示单元集的当前状态。  
  
## <a name="return-values"></a>返回值  
 返回一个 **长** 整数，该整数指示 [单元集](./cellset-object-ado-md.md) 对象的当前条件，并且为只读。 以下值有效： **adStateClosed** (0) 和 **adStateOpen** (1) 。  
  
## <a name="remarks"></a>备注  
 若要使用 [ObjectStateEnum](../ado-api/objectstateenum.md) 常量名称，必须在项目中引用 ADO 类型库。 有关详细信息，请参阅将 [ADO 与 ADO MD 配合使用](../../guide/multidimensional/using-ado-with-ado-md.md) 。  
  
## <a name="applies-to"></a>应用于  
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [Close 方法 (ADO MD) ](./close-method-ado-md.md)   
 [Open 方法 (ADO MD)](./open-method-ado-md.md)