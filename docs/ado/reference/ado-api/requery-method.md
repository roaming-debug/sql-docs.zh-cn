---
description: Requery 方法
title: Requery 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Requery
- Recordset15::raw_Requery
helpviewer_keywords:
- Requery method [ADO]
ms.assetid: d81ab76f-1aa8-4ccf-92ec-b65254dc3ea1
author: rothja
ms.author: jroth
ms.openlocfilehash: 060c1c7f792a0cf74e9ca8192167c41fcb7b9399
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051618"
---
# <a name="requery-method"></a>Requery 方法
通过重新执行对象所基于的查询来更新 [记录集](./recordset-object-ado.md) 对象中的数据。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Requery Options  
```  
  
#### <a name="parameters"></a>参数  
 *选项*  
 可选。 一个位掩码，其中包含影响此操作的 [ExecuteOptionEnum](./executeoptionenum.md) 和 [CommandTypeEnum](./commandtypeenum.md) 值。  
  
> [!NOTE]
>  如果 *选项* 设置为 **adAsyncExecute**，此操作将以异步方式执行，并且在结束时将发出 [RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) 事件。 **AdExecuteNoRecords** 或 **adExecuteStream** 的 **ExecuteOpenEnum** 值不应与 **Requery** 一起使用。  
  
## <a name="remarks"></a>备注  
 使用 **Requery** 方法，通过重新发出原始命令并再次检索数据来刷新数据源的 **记录集** 对象的全部内容。 调用此方法等效于连续调用 [Close](./close-method-ado.md) 和 [Open](./open-method-ado-recordset.md) 方法。 如果正在编辑当前记录或添加新记录，则会发生错误。  
  
 **Recordset** 对象处于打开状态时，定义游标性质的属性 ([CursorType](./cursortype-property-ado.md)、 [LockType](./locktype-property-ado.md)、 [MaxRecords](./maxrecords-property-ado.md)等) 为只读。 因此， **Requery** 方法只能刷新当前游标。 若要更改任何游标属性并查看结果，必须使用 [Close](./close-method-ado.md) 方法，使属性再次变为读/写。 然后，可以更改属性设置并调用 [Open](./open-method-ado-recordset.md) 方法以重新打开光标。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [执行、再次查询和清除方法示例 (VB) ](./execute-requery-and-clear-methods-example-vb.md)   
 [ (VBScript) 执行、再次查询和清除方法示例 ](./execute-requery-and-clear-methods-example-vbscript.md)   
 [执行、再次查询和清除方法示例 (VC + +) ](./execute-requery-and-clear-methods-example-vc.md)   
 [CommandText 属性 (ADO)](./commandtext-property-ado.md)