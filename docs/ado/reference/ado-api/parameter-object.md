---
description: Parameter 对象
title: 参数对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parameter
helpviewer_keywords:
- Parameter object [ADO]
ms.assetid: e010e794-7f0f-4026-8b5b-37328e437d63
author: rothja
ms.author: jroth
ms.openlocfilehash: d3922289548cbc3cb2e694399ac9f09e13d73985
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170616"
---
# <a name="parameter-object"></a>Parameter 对象
表示基于参数化查询或存储过程与 [命令](./command-object-ado.md) 对象关联的参数或参数。  
  
## <a name="remarks"></a>备注  
 许多提供程序都支持参数化命令。 这是一次定义所需操作的命令，但变量 (或参数) 用于更改命令的某些详细信息。 例如，SQL SELECT 语句可以使用参数来定义 WHERE 子句的匹配条件，并使用另一个参数定义 SORT BY 子句的列名称。  
  
 **参数** 对象表示与参数化查询关联的参数，或 in/out 参数和存储过程的返回值。 **参数** 对象的某些集合、方法或属性可能不可用，具体取决于提供程序的功能。  
  
 使用 **参数** 对象的集合、方法和属性，可以执行以下操作：  
  
-   设置或返回具有 [name](./name-property-ado.md) 属性的参数的名称。  
  
-   设置或返回 [值](./value-property-ado.md) 属性为的参数的值。 **值** 是 **参数** 对象的默认属性。  
  
-   设置或返回 [属性](./attributes-property-ado.md)、 [方向](./direction-property.md)、 [精度](./precision-property-ado.md)、 [NumericScale](./numericscale-property-ado.md)、 [大小](./size-property-ado-parameter.md)和 [类型](./type-property-ado.md) 属性的参数特征。  
  
-   使用 [AppendChunk](./appendchunk-method-ado.md) 方法将长二进制或字符数据传递给参数。  
  
-   使用 [Properties](./properties-collection-ado.md) 集合访问特定于提供程序的特性。  
  
 如果知道与要调用的存储过程或参数化查询相关联的参数的名称和属性，则可以使用 [CreateParameter](./createparameter-method-ado.md) 方法创建具有相应属性设置的 **参数** 对象，并使用 [Append](./append-method-ado.md) 方法将它们添加到 [parameters](./parameters-collection-ado.md) 集合。 这使您可以设置和返回参数值，而无需对 **Parameters** 集合调用 [Refresh](./refresh-method-ado.md)方法即可从提供程序中检索参数信息，这可能会占用大量资源。  
  
 **参数** 对象对于脚本编写是不安全的。  
  
 本部分包含以下主题。  
  
-   [参数对象属性、方法和事件](./parameter-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO) 的命令对象 (](./command-object-ado.md)   
 [CreateParameter 方法 (ADO) ](./createparameter-method-ado.md)   
 [ADO) 的参数集合 (](./parameters-collection-ado.md)   
 [属性集合 (ADO)](./properties-collection-ado.md)