---
description: EditMode 属性
title: EditMode 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::EditMode
helpviewer_keywords:
- EditMode property
ms.assetid: a1b04bb2-8c8b-47f9-8477-bfd0368b6f68
author: rothja
ms.author: jroth
ms.openlocfilehash: 5854dce12ddb2303077111f3129274861a340ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171241"
---
# <a name="editmode-property"></a>EditMode 属性
指示当前记录的编辑状态。  
  
## <a name="return-value"></a>返回值  
 返回 [EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md) 值。  
  
## <a name="remarks"></a>备注  
 ADO 维护与当前记录相关联的编辑缓冲区。 此属性指示是否已对此缓冲区进行了更改，或者是否已创建新记录。 使用 **EditMode** 属性可确定当前记录的编辑状态。 如果编辑进程已中断并确定是否需要使用 [Update](../../../ado/reference/ado-api/update-method.md) 或 [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) 方法，则可以测试挂起的更改。  
  
 在 *即时更新模式* 中，在调用 **update** 方法的成功调用后， **EditMode** 属性重置为 **adEditNone** 。 当调用 [delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md)时，如果不能成功删除数据源中的记录或记录 (例如，因为存在引用完整性冲突) ，则 (**EditMode** adEditInProgress) [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)仍处于编辑模式  =   。 因此，必须先调用 **CancelUpdate** [，然后](../../../ado/reference/ado-api/move-method-ado.md)才能将当前记录移 (例如， [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)或 [Close](../../../ado/reference/ado-api/close-method-ado.md) ) 。  
  
 在 *批处理更新模式下* (提供程序将在其中缓存多个更改并将其写入基础数据源，仅当你) 调用 [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) 方法时， **EditMode** 属性的值会在执行第一个操作时更改，并且不会通过调用 **update** 方法重置。 即使执行了不同的操作，后续操作也不会更改 **EditMode** 属性的值。 例如，如果第一个操作是添加一条新记录，第二个操作对现有记录进行了更改，则 **EditMode** 的属性仍将为 **adEditAdd**。 直到调用 **UpdateBatch** 后， **EditMode** 属性才会重置为 **adEditNone** 。 若要确定执行了哪些操作，请将 [Filter](../../../ado/reference/ado-api/filter-property.md) 属性设置为 [adFilterPending](../../../ado/reference/ado-api/filtergroupenum.md) ，以便仅显示具有挂起更改的记录，并检查每个记录的 [Status](../../../ado/reference/ado-api/status-property-ado-recordset.md) 属性以确定对数据所做的更改。  
  
> [!NOTE]
>  仅当存在当前记录时， **EditMode** 才可以返回有效的值。 如果 [BOF 或 EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md)为 true，则 **EditMode** 将返回错误，如果当前记录已被删除，则返回错误。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [CursorType、LockType 和 EditMode 属性示例 (VB) ](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vb.md)   
 [CursorType、LockType 和 EditMode 属性示例 (VC + +) ](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vc.md)   
 [AddNew 方法 (ADO) ](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [ADO 记录集 (Delete 方法) ](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [CancelUpdate 方法 (ADO) ](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [Update 方法](../../../ado/reference/ado-api/update-method.md)
