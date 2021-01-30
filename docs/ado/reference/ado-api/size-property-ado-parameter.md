---
description: Size 属性（ADO 参数）
title: ADO 参数) 的大小属性 (|Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Size
helpviewer_keywords:
- Size property [ADO Parameter]
ms.assetid: e6bad449-ebdb-4dd3-886a-9e6f1e7ee5d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 5f47c76cd0c79d1ffade6ff163558be5e5a254be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166543"
---
# <a name="size-property-ado-parameter"></a>Size 属性（ADO 参数）
指示 [参数](./parameter-object.md) 对象的最大大小，以字节或字符为单位。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **长整型** 值，该值指示 **参数** 对象中的值的最大大小（以字节为单位）。  
  
## <a name="remarks"></a>备注  
 使用 " **Size** " 属性可确定从 **参数** 对象的 [Value](./value-property-ado.md)属性写入或读取的值的最大大小。  
  
 如果为 **参数** 对象指定长度可变的数据类型 (例如，任何 **字符串** 类型（如 **adVarChar**) ），则必须先设置对象的 **Size** 属性，然后再将其追加到 [Parameters](./parameters-collection-ado.md) 集合中;否则，将发生错误。  
  
 如果已将 **参数** 对象追加到 [Command](./command-object-ado.md)对象的 **Parameters** 集合，并将其类型更改为可变长度数据类型，则必须先设置 **参数** 对象的 **Size** 属性，然后再执行 **命令** 对象;否则，将发生错误。  
  
 如果使用 [Refresh](./refresh-method-ado.md) 方法从提供程序中获取参数信息，并返回一个或多个可变长度的数据类型 **参数** 对象，则 ADO 可能会根据其最大可能大小为参数分配内存，这可能会导致在执行过程中出错。 若要防止出现错误，应在执行命令之前显式设置这些参数的 **大小** 属性。  
  
 **Size** 属性是可读/写的。  
  
## <a name="applies-to"></a>应用于  
 [参数对象](./parameter-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VB) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VC + +) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (JScript) ](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Size 属性（ADO 流）](./size-property-ado-stream.md)