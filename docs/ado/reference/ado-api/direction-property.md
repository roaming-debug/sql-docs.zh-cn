---
description: Direction 属性
title: Direction 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Direction
helpviewer_keywords:
- Direction property
ms.assetid: d5732578-3434-4dcd-a9f7-db1abd1b3b94
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f202ed23634834d569cfa6b499074806572729d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167518"
---
# <a name="direction-property"></a>Direction 属性
指示 [参数](../../../ado/reference/ado-api/parameter-object.md) 是表示输入参数、输出参数、输入参数还是输出参数，如果参数是来自存储过程的返回值，则为。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 [ParameterDirectionEnum](../../../ado/reference/ado-api/parameterdirectionenum.md) 值。  
  
## <a name="remarks"></a>备注  
 使用 " **方向** " 属性可以指定如何在过程中传递参数。 **Direction** 属性是可读/写的;这使你可以使用不返回此信息的提供程序，或者在不希望 ADO 对提供程序进行额外调用以检索参数信息时设置此信息。  
  
 并非所有提供程序都可以确定其存储过程中的参数的方向。 在这些情况下，必须在执行查询之前设置 " **方向** " 属性。  
  
## <a name="applies-to"></a>应用于  
 [参数对象](../../../ado/reference/ado-api/parameter-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VB) ](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VC + +) ](../../../ado/reference/ado-api/activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (JScript) ](../../../ado/reference/ado-api/activeconnection-commandtext-timeout-type-size-example-jscript.md)
