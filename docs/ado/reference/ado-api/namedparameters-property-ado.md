---
description: NamedParameters 属性 (ADO)
title: " (ADO) 的 NamedParameters 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::NamedParameters
helpviewer_keywords:
- NamedParameters property [ADO]
ms.assetid: 42409387-026c-435f-a9b1-bf4167095875
author: rothja
ms.author: jroth
ms.openlocfilehash: 9ce2c73ec00431f649d24a9effda2a7e9ecb6cfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170773"
---
# <a name="namedparameters-property-ado"></a>NamedParameters 属性 (ADO)
指示是否应将参数名称传递给提供程序。  
  
## <a name="remarks"></a>备注  
 当此属性为 true 时，ADO 将为 [命令对象](./command-object-ado.md)传递 **参数** 集合中每个参数的 **Name** 属性的值。 提供程序使用参数名来匹配 **CommandText** 或 **CommandStream** 属性中的参数。 如果此属性为 false (默认) ，则将忽略参数名，并且提供程序使用参数的顺序将值与 **CommandText** 或 **CommandStream** 属性中的参数相匹配。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO) 的 CommandText 属性 ](./commandtext-property-ado.md)   
 [CommandStream 属性 (ADO) ](./commandstream-property-ado.md)   
 [参数集合 (ADO)](./parameters-collection-ado.md)