---
description: Status 属性（ADO 字段）
title: " (ADO 字段) 的状态属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field::Status
- Field::get_Status
- Field::GetStatus
helpviewer_keywords:
- Status property [ADO Field]
ms.assetid: 8cd1f7f4-0a3a-4f07-b8ba-6582e70140ad
author: rothja
ms.author: jroth
ms.openlocfilehash: d8ec88ad85b4f0432ce4740e8f54647640f2e7bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040607"
---
# <a name="status-property-ado-field"></a>Status 属性（ADO 字段）
指示 [Field](./field-object.md) 对象的状态。  
  
## <a name="return-value"></a>返回值  
 返回 [FieldStatusEnum](./fieldstatusenum.md) 值。 默认值为 **adFieldOK**。  
  
## <a name="remarks"></a>备注  
  
## <a name="record-field-status"></a>记录字段状态  
 在调用对象的 [Update](./update-method.md)方法之前，将缓存对 [Record](./record-object-ado.md)对象的 **Field** 对象的值所做的更改。 此时，如果对字段的值所做的更改导致了错误，OLE DB 将 **DB_E_ERRORSOCCURRED** (2147749409) 引发错误。 导致错误的 **字段** 集合中的任何 **字段** 对象的 Status 属性将包含 [FieldStatusEnum](./fieldstatusenum.md)中的一个值，用于描述问题的原因。  
  
 为了增强性能，将缓存对 **Record** 对象的 [字段](./fields-collection-ado.md)集合的添加和删除操作，直到调用 **Update** 方法，然后在批处理乐观更新中进行更改。 如果不调用 **Update** 方法，服务器将不会更新。 如果任何更新失败，则返回 DB_E_ERRORSOCCURRED)  (OLE DB 提供程序错误， **状态** 属性指示操作和错误状态代码的组合值。 例如， **ADFIELDPENDINGINSERT 或 adFieldPermissionDenied**。 每个 **字段** 的 "**状态**" 属性可用于确定不添加、修改或删除 **字段** 的原因。  
  
 添加、修改或删除 **字段** 时遇到的许多类型的问题都通过 **Status** 属性进行报告。 例如，如果用户删除某个 **字段**，则会将其标记为从 **Fields** 集合中删除。 如果后续的 **更新** 返回错误，因为用户试图删除其没有权限的 **字段** ，则该 **字段** 的 **状态** 将为 **adFieldPermissionDenied 或 adFieldPendingDelete**。 调用 [CancelUpdate](./cancelupdate-method-ado.md) 方法将恢复原始值并将 **状态** 设置为 **adFieldOK**。  
  
 同样， **Update** 方法可能会返回错误，因为添加了新 **字段** 并给出了不合适的值。 在这种情况下，新 **字段** 将位于 **Fields** 集合中，并且状态为 " **adFieldPendingInsert** "，可能是根据提供程序 **)  (的** 。 可以为新 **字段** 提供相应的值，并再次调用 **Update** 。  
  
## <a name="recordset-field-status"></a>记录集字段状态  
 在调用对象的 [Update](./update-method.md)方法之前，对 [记录集](./recordset-object-ado.md)的字段集合中 **字段** 对象的值所做的更改将被缓存。 此时，如果对字段的值所做的更改导致了错误，OLE DB 将 **DB_E_ERRORSOCCURRED** (2147749409) 引发错误。 导致错误的 **字段** 集合中的任何 **字段** 对象的 Status 属性将包含 [FieldStatusEnum](./fieldstatusenum.md)中的一个值，用于描述问题的原因。  
  
## <a name="applies-to"></a>应用于  
 [字段对象](./field-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [状态属性示例 ( (VB) 字段) ](./status-property-example-field-vb.md)   
 [Status 属性示例 (VC++)](./status-property-example-vc.md)