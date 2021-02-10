---
description: Update Resync 属性 - 动态 (ADO)
title: " (ADO) 更新重新同步 Property-Dynamic |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Update Resync property [ADO]
ms.assetid: 8a3bb608-66d7-4128-a3ef-84cb0556de0d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e52c07a4f776c740079ae1460a2890d7aac02ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056292"
---
# <a name="update-resync-property-dynamic-ado"></a>Update Resync 属性 - 动态 (ADO)
指定 [UpdateBatch](./updatebatch-method.md) 方法是否后跟隐式重新 [同步](./resync-method.md) 方法操作，如果是，则为该操作的范围。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个或多个 [ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md) 值。  
  
## <a name="remarks"></a>备注  
 ADCPROP_UPDATERESYNC_ENUM 的值可以组合在一起，但 adResyncAll 的值已表示剩余值的组合。  
  
 常量 **adResyncConflicts** 将重新同步值作为基础值存储，但不会覆盖挂起的更改。  
  
 当 [CursorLocation](./cursorlocation-property-ado.md)属性设置为 **AdUseClient** 时，**更新重新同步** 是追加到 [Recordset](./recordset-object-ado.md)对象 [Properties](./properties-collection-ado.md) collection 的动态属性。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)