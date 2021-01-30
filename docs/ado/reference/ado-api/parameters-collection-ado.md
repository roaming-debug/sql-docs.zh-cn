---
description: 参数集合 (ADO)
title: ADO)  (参数集合 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::get_Parameters
- Command15::Parameters
- Command15::GetParameters
helpviewer_keywords:
- Parameters collection [ADO]
ms.assetid: 497cae10-3913-422a-9753-dcbb0a639b1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 4cd94745a6ec2d4f34245b53f509752c13688d45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170594"
---
# <a name="parameters-collection-ado"></a>参数集合 (ADO)
包含[Command](./command-object-ado.md)对象的所有[参数](./parameter-object.md)对象。  
  
## <a name="remarks"></a>备注  
 **命令** 对象具有由 **参数** 对象组成的 **参数** 集合。  
  
 对 **command** 对象的 **Parameters** 集合使用 [Refresh](./refresh-method-ado.md)方法将检索在 **命令** 对象中指定的存储过程或参数化查询的提供程序参数信息。 某些提供程序不支持存储过程调用或参数化查询;使用此类提供程序时对 **参数** 集合调用 **Refresh** 方法将返回错误。  
  
 如果尚未定义自己的 **参数** 对象，并且在调用 **Refresh** 方法之前访问 **参数** 集合，则 ADO 将自动调用方法并为您填充集合。  
  
 如果知道与要调用的存储过程或参数化查询相关联的参数的属性，则可以最大程度地减少对提供程序的调用以提高性能。 使用 [CreateParameter](./createparameter-method-ado.md) 方法创建具有相应属性设置的 **参数** 对象，并使用 [Append](./append-method-ado.md) 方法将它们添加到 **Parameters** 集合。 这使你可以设置和返回参数值，而无需调用提供程序来获取参数信息。 如果要写入未提供参数信息的提供程序，则必须使用此方法手动填充 **参数** 集合，以便能够使用参数。 如有必要，请使用 [Delete](./delete-method-ado-parameters-collection.md)方法从 **Parameters** 集合中删除 **参数** 对象。  
  
 **记录** 集的 **参数** 集合中的对象超出范围 (因此关闭 **记录集** 时将变为不可用) 。  
  
 使用 **命令** 调用存储过程时，将检索存储过程的返回值/输出参数，如下所示：  
  
1.  调用没有参数的存储过程时，应在对 **命令** 对象调用 **Execute** 方法之前调用 **parameters** 集合上的 **Refresh** 方法。  
  
2.  当使用参数调用存储过程并将参数显式追加到带 **Append** 的 **parameters** 集合时，应将返回值/输出参数追加到 **parameters** 集合。 必须首先将返回值追加到 **Parameters** 集合。 使用 **Append** 将其他参数以定义顺序添加到 **参数** 集合中。 例如，存储过程 SPWithParam 有两个参数。 第一个参数 *InParam* 是定义为 adVarChar (20) 的输入参数，第二个参数 *OutParam* 是定义为 adVarChar (20) 的输出参数。 可以通过以下代码检索返回值/输出参数。  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Append ccmd.CreateParameter(, adInteger, adParamReturnValue, , NULL)   ' return value  
    ccmd.parameters.Append ccmd.CreateParameter("InParam", adVarChar, adParamInput, 20, "hello world")   ' input parameter  
    ccmd.parameters.Append ccmd.CreateParameter("OutParam", adVarChar, adParamOutput, 20, NULL)   ' output parameter  
  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of this stored procedure  
    ' Access ccmd.parameters("OutParam") as the output parameter of this stored procedure.  
  
    ```  
  
3.  当使用参数调用存储过程并通过对 **parameters** 集合调用 **Item** 方法来配置参数时，可以从 **parameters** 集合中检索存储过程的返回值/输出参数。 例如，存储过程 SPWithParam 有两个参数。 第一个参数 *InParam* 是定义为 adVarChar (20) 的输入参数，第二个参数 *OutParam* 是定义为 adVarChar (20) 的输出参数。 可以通过以下代码检索返回值/输出参数。  
  
    ```vb
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Item("InParam").value = "hello world" ' input parameter  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of stored procedure  
    ' Access ccmd.parameters(2) or ccmd.parameters("OutParam") as the output parameter.  
    ```  
  
 本部分包含以下主题。  
  
-   [参数集合属性、方法和事件](./parameters-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO (追加方法) ](./append-method-ado.md)   
 [CreateParameter 方法 (ADO) ](./createparameter-method-ado.md)   
 [参数对象](./parameter-object.md)