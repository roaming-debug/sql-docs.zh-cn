---
description: NextRecordset 方法 (ADO)
title: " (ADO) 的 NextRecordset 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- NextRecordset
- Recordset15::NextRecordset
- Recordset15::raw_NextRecordset
helpviewer_keywords:
- NextRecordset method [ADO]
ms.assetid: ab1fa449-a695-4987-b1ee-bc68f89418dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 3388a5f37fa8d82aa951f1cc82eb9f43556d9d37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041577"
---
# <a name="nextrecordset-method-ado"></a>NextRecordset 方法 (ADO)
清除当前 [记录集](./recordset-object-ado.md) 对象，并通过前进一系列命令来返回下一个 **记录集** 。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set recordset2 = recordset1.NextRecordset(RecordsAffected )  
```  
  
## <a name="return-value"></a>返回值  
 返回 **记录集** 对象。 在语法模型中， *recordset1* 和 *recordset2* 可以是同一 **Recordset** 对象，也可以使用单独的对象。 使用单独的 **记录集** 对象时，在调用 **NextRecordset** 后重置原始 **记录集** 上的 **ActiveConnection** 属性 (*recordset1*) 将生成错误。  
  
#### <a name="parameters"></a>参数  
 *RecordsAffected*  
 可选。 一个 **长整型** 变量，提供程序返回当前操作影响的记录数。  
  
> [!NOTE]
>  此参数仅返回受操作影响的记录数;它不会从用于生成 **记录集** 的 select 语句返回记录计数。  
  
## <a name="remarks"></a>备注  
 使用 **NextRecordset** 方法可以返回复合命令语句或返回多个结果的存储过程的下一个命令的结果。 如果基于复合命令语句打开 **Recordset** 对象 (例如，"SELECT \* FROM table1;\*从 table2 中选择 ) "使用 [Execute](./execute-method-ado-command.md)方法对某个 [命令](./command-object-ado.md)使用 Execute 方法" 或在 **记录集中** 使用 [Open](./open-method-ado-recordset.md)方法，ADO 只执行第一个命令，并将结果返回 *记录集*。 若要访问语句中后面的命令的结果，请调用 **NextRecordset** 方法。  
  
 只要有其他结果，并且包含复合语句的 **记录集** 未在进程边界之间断开连接或被封送， **NextRecordset** 方法将继续返回 **Recordset** 对象。 如果返回行的命令成功执行但未返回任何记录，则返回的 **Recordset** 对象将打开但为空。 通过验证 [BOF](./bof-eof-properties-ado.md) 和 [EOF](./bof-eof-properties-ado.md) 属性均 **为 True** 来测试这种情况。 如果非行返回的命令成功执行，则返回的 **记录集** 对象将关闭，您可以通过测试 **记录集** 的 "[状态](./state-property-ado.md)" 属性来验证该对象。 如果没有更多结果，则 *记录集* 将设置为 *Nothing*。  
  
 **NextRecordset** 方法不可用于已断开连接的 **记录集** 对象，其中 [ActiveConnection](./activeconnection-property-ado.md)已在 Microsoft Visual Basic 中设置为 **Nothing** () 或 (其他) 语言中为 NULL。  
  
 如果在即时更新模式下编辑正在进行，则调用 **NextRecordset** 方法将生成错误;首先调用 [Update](./update-method.md) 或 [CancelUpdate](./cancelupdate-method-ado.md) 方法。  
  
 若要通过填充 [parameters](./parameters-collection-ado.md) 集合或传递具有原始 **Open** 或 **Execute** 调用的数组，在复合语句中传递多个命令的参数，这些参数在该集合或数组中的顺序必须与命令序列中各自的命令顺序相同。 在读取输出参数值之前，必须完成所有结果的读取。  
  
 OLE DB 提供程序确定复合语句中每个命令的执行时间。 例如， [用于 SQL Server 的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md)在接收复合语句后执行批处理中的所有命令。 在调用 **NextRecordset** 时，只返回生成的 **记录集**。  
  
 但是，在调用 NextRecordset 之后，其他提供程序可能会在语句中执行下一个命令。 对于这些提供程序，如果在单步执行整个命令语句之前显式关闭 **Recordset** 对象，则 ADO 从不执行剩余的命令。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [NextRecordset 方法示例 (VB) ](./nextrecordset-method-example-vb.md)   
 [NextRecordset 方法示例 (VC++)](./nextrecordset-method-example-vc.md)