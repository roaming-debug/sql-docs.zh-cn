---
description: CreateParameter 方法 (ADO)
title: " (ADO) 的 CreateParameter 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::raw_CreateParameter
- Command15::CreateParameter
helpviewer_keywords:
- CreateParameter method [RDS]
ms.assetid: 9666fdcc-0544-4ed7-a97b-c415f2a56d7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 61793ca158cee6d4143128eae7fdecf3722ae61e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171382"
---
# <a name="createparameter-method-ado"></a>CreateParameter 方法 (ADO)
创建具有指定属性的新 [参数](./parameter-object.md) 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set parameter = command.CreateParameter (Name, Type, Direction, Size, Value)  
```  
  
## <a name="return-value"></a>返回值  
 返回一个 **参数** 对象。  
  
#### <a name="parameters"></a>参数  
 *名称*  
 可选。 一个包含 **参数** 对象名称的 **字符串** 值。  
  
 *Type*  
 可选。 指定 **参数** 对象的数据类型的 [DataTypeEnum](./datatypeenum.md)值。  
  
 *方向*  
 可选。 指定 **参数** 对象类型的 [ParameterDirectionEnum](./parameterdirectionenum.md)值。  
  
 *大小*  
 可选。 一个 **长整型** 值，指定参数值的最大长度（以字符或字节为单位）。  
  
 值  
 可选。 一个 **变量** ，指定 **参数** 对象的值。  
  
## <a name="remarks"></a>备注  
 使用 **CreateParameter** 方法创建一个具有指定名称、类型、方向、大小和值的新 **参数** 对象。 您在参数中传递的任何值都将写入相应的 **参数** 属性。  
  
 此方法不会将 **参数** 对象自动追加到 [Command](./command-object-ado.md)对象的 **Parameters** 集合。 这使你可以设置其他属性，这些属性的值会在你将 **参数** 对象追加到集合时进行验证。  
  
 如果在 *类型* 参数中指定可变长度的数据类型，则必须在将其追加到 **Parameters** 集合之前传递 *Size* 参数或设置 **参数** 对象的 [size](./size-property-ado-parameter.md)属性;否则，将发生错误。  
  
 如果在 *类型* 参数中指定数值数据类型 (**adNumeric** 或 **adDecimal**) ，则还必须设置 [NumericScale](./numericscale-property-ado.md)和 [精度](./precision-property-ado.md)属性。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Append 和 CreateParameter 方法示例 (VB) ](./append-and-createparameter-methods-example-vb.md)   
 [附加和 CreateParameter 方法示例 (VC + +) ](./append-and-createparameter-methods-example-vc.md)   
 [ADO (追加方法) ](./append-method-ado.md)   
 [参数对象](./parameter-object.md)   
 [参数集合 (ADO)](./parameters-collection-ado.md)