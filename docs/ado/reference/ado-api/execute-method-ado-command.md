---
description: Execute 方法（ADO 命令）
title: ADO 命令 (的 Execute 方法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Execute
- Command15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: f84a5ff3-0528-4ad7-9bea-9a15103378dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 1bc27de85c9c8e1e47ec3fb89ec8fadce07713c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171154"
---
# <a name="execute-method-ado-command"></a>Execute 方法（ADO 命令）
执行[命令对象](../../../ado/reference/ado-api/command-object-ado.md)的[CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md)或[CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md)属性中指定的查询、SQL 语句或存储过程。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set recordset = command.Execute( RecordsAffected, Parameters, Options )  
```  
  
## <a name="return-value"></a>返回值  
 返回 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md) 对象引用、流或不返回 **任何内容**。  
  
#### <a name="parameters"></a>参数  
 *RecordsAffected*  
 可选。 一个 **长整型** 变量，提供程序返回操作影响的记录数。 *RecordsAffected* 参数仅适用于操作查询或存储过程。 *RecordsAffected* 不返回结果返回的查询或存储过程返回的记录数。 若要获取此信息，请使用 [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) 属性。 在与 **adAsyncExecute** 一起使用时， **Execute** 方法不会返回正确的信息，只是因为异步执行命令时，在该方法返回时，受影响的记录数可能尚未已知。  
  
 *参数*  
 可选。 与在 **CommandText** 或 **CommandStream** 中指定的输入字符串或流结合 **使用的参数值数组。** 当传入此参数时， (输出参数不会返回正确的值。 )   
  
 *选项*  
 可选。 一个 **长整型** 值，该值指示提供程序应如何计算 [命令](../../../ado/reference/ado-api/command-object-ado.md)对象的 [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md)或 [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md)属性。 可以是使用 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) 和/或 [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) 值进行的位掩码值。 例如，如果想让 ADO 将 **CommandText** 属性的值作为文本进行计算，则可以结合使用 **adCmdText** 和 **adExecuteNoRecords** ，并指示该命令应丢弃，而不会返回命令文本执行时可能生成的任何记录。  
  
> [!NOTE]
>  使用 **ExecuteOptionEnum** 值 **adExecuteNoRecords** ，通过最大限度地减少内部处理来提高性能。 如果指定了 **adExecuteStream** ，则将忽略选项 **adAsyncFetch** 和 **adAsynchFetchNonBlocking** 。 不要将 **adCmdFile** 或 **adCmdTableDirect** 的 **CommandTypeEnum** 值用于 **Execute**。 这些值只能用作 **记录集** 的 [打开](../../../ado/reference/ado-api/open-method-ado-recordset.md)和重新 [查询](../../../ado/reference/ado-api/requery-method.md)方法的选项。  
  
## <a name="remarks"></a>备注  
 对 **命令** 对象使用 **Execute** 方法会执行在对象的 **CommandText** 属性或 **CommandStream** 属性中指定的查询。  
  
 结果在默认情况下将在 **记录集中** 返回 () 或作为二进制信息流。 若要获取二进制流，请在 "*选项*" 中指定 **adExecuteStream** ，然后通过设置命令来提供流 **。属性 ( "输出流" )**。 可以指定 ADO **流** 对象来接收结果，也可以指定另一个流对象，如 IIS 响应对象。 如果在使用 **adExecuteStream** 调用 **Execute** 之前未指定流，则会发生错误。 从 **Execute** 返回的流的位置是特定于提供程序的。  
  
 如果此命令不打算返回结果 (例如，只要指定了选项 **adExecuteNoRecords** ，) 提供程序将不返回 **任何内容**;否则 Execute 将返回已关闭的 **记录集**。 如果没有所需的 **记录集** ，某些应用程序语言允许忽略此返回值。  
  
 当 **CommandType** 为 **adCmdStoredProc**、 **adCmdTable** 或 **adCmdTableDirect** 时，如果用户为 **CommandStream** 指定了值，则 **执行** 将引发错误。  
  
 如果查询具有参数，则使用 **命令** 对象的参数的当前值，除非使用通过 **执行** 调用传递的参数值重写这些值。 可以通过在调用 **Execute** 方法时省略某些参数的新值，来重写参数的子集。 指定参数的顺序与方法传递参数的顺序相同。 例如，如果有四个 (或多个) 参数，而你只希望为第一个和第四个参数传递新值，则将 `Array(var1,,,var4)` 作为 *parameters* 参数传递。  
  
> [!NOTE]
>  当传入 *参数* 参数时，输出参数不会返回正确的值。  
  
 此操作结束时，将发出 [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) 事件。  
  
> [!NOTE]
>  发出包含 Url 的命令时，使用 http 方案的命令将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../../ado/guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [执行、再次查询和清除方法示例 (VB) ](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [ (VBScript) 执行、再次查询和清除方法示例 ](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [执行、再次查询和清除方法示例 (VC + +) ](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [CommandStream 属性 (ADO) ](../../../ado/reference/ado-api/commandstream-property-ado.md)   
 [ (ADO) 的 CommandText 属性 ](../../../ado/reference/ado-api/commandtext-property-ado.md)   
 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)   
 [ADO 连接 (执行方法) ](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [ExecuteComplete 事件 (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)
