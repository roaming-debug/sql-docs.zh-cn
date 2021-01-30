---
description: CommandTypeEnum
title: CommandTypeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommandTypeEnum
helpviewer_keywords:
- CommandTypeEnum enumeration [ADO]
ms.assetid: 4b1feb9c-a855-40fe-a906-efe688687e9f
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cea565eed8ae7fa9800284c19b03cdbc704b5e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164659"
---
# <a name="commandtypeenum"></a>CommandTypeEnum
指定如何解释命令参数。  
  
 验证用户提供的 *command.commandstring* 值非常重要，这是为了避免应用程序用户为 ADO 提供可能的危险命令。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adCmdUnspecified**|-1|不指定命令类型参数。|  
|**adCmdText**|1|将 [CommandText](./commandtext-property-ado.md) 计算为命令或存储过程调用的文本定义。|  
|**adCmdTable**|2|将 **CommandText** 计算为表名称，其列全部由内部生成的 SQL 查询返回。|  
|**adCmdStoredProc**|4|将 **CommandText** 计算为存储过程名称。|  
|**adCmdUnknown**|8|默认。 指示 **CommandText** 属性中的命令类型未知。<br /><br /> 当命令类型未知时，ADO 将多次尝试解释 **CommandText**。<br /><br /> -   **CommandText** 被解释为命令或存储过程调用的文本定义。 这与 **adCmdText** 的行为相同。<br />-   **CommandText** 是存储过程的名称。 这与 **adCmdStoredProc** 的行为相同。<br />-   **CommandText** 被解释为表的名称。 所有列都由内部生成的 SQL 查询返回。 这与 **adCmdTable** 的行为相同。|  
|**adCmdFile**|256|将 **CommandText** 计算为持久存储的 [记录集](./recordset-object-ado.md)的文件名。 与 Recordset 一起使用 **。** 仅 [打开](./open-method-ado-recordset.md)或重新 [查询](./requery-method.md)。|  
|**adCmdTableDirect**|512|将 **CommandText** 计算为其列全部返回的表名。 与 Recordset 一起使用。仅 **打开** 或重新 **查询** 。 若要使用 [Seek](./seek-method.md)方法，必须使用 **AdCmdTableDirect** 打开 **记录集**。<br /><br /> 此值不能与 [ExecuteOptionEnum](./executeoptionenum.md) 值 **adAsyncExecute** 组合。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. CommandType。未指定|  
|AdoEnums. CommandType|  
|AdoEnums. CommandType|  
|AdoEnums. CommandType. STOREDPROC|  
|AdoEnums. CommandType. 未知|  
|AdoEnums. CommandType. 文件|  
|AdoEnums. CommandType. TABLEDIRECT|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [CommandType 属性 (ADO)](./commandtype-property-ado.md)  
        [Execute 方法（ADO 命令）](./execute-method-ado-command.md)  
    :::column-end:::
    :::column:::
        [Execute 方法（ADO 连接）](./execute-method-ado-connection.md)  
        [Open 方法（ADO 记录集）](./open-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [Requery 方法](./requery-method.md)  
    :::column-end:::
:::row-end:::