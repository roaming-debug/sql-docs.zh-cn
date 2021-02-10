---
description: Mode 属性 (ADO)
title: ADO)  (模式属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Mode
- _Stream::Mode
- _Record::Mode
helpviewer_keywords:
- Mode property [ADO]
ms.assetid: 808661eb-0d7c-4e6d-8e40-9dc3bef3d77a
author: rothja
ms.author: jroth
ms.openlocfilehash: ae638bd51ea34a81efa5fb3192592fa8f3a005c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041777"
---
# <a name="mode-property-ado"></a>Mode 属性 (ADO)
指示用于修改 [连接](./connection-object-ado.md)、 [记录](./record-object-ado.md)或 [流](./stream-object-ado.md) 对象中的数据的可用权限。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 [ConnectModeEnum](./connectmodeenum.md) 值。 **连接** 的默认值为 **adModeUnknown**。 **记录** 对象的默认值为 **adModeRead**。 与基础源关联的 **流** 的默认值 (使用 URL 作为源打开，或者作为 **记录**) 的默认 **流** **。** 与 (在内存中实例化的基础源) 不关联的 **流** 的默认值为 **adModeUnknown**。  
  
## <a name="remarks"></a>备注  
 使用 **Mode** 属性可以设置或返回当前连接上的提供程序所使用的访问权限。 仅当关闭 **连接** 对象时，才可以设置 **Mode** 属性。  
  
 对于 **流** 对象，如果未指定访问模式，则从用于打开 **流** 对象的源继承。 例如，如果从 **记录** 对象打开流，则默认情况下，将以与 **记录** 相同的模式打开 **流**。  
  
 当对象处于关闭状态时，此属性是可读/写的，当对象处于打开状态时，此属性为只读。  
  
> [!NOTE]
>  **远程数据服务使用情况** 当在客户端 **连接** 对象上使用时， **模式** 属性只能设置为 **adModeUnknown**。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [连接对象 (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录对象 (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [流对象 (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [IsolationLevel 和 Mode Properties (VB) ](./isolationlevel-and-mode-properties-example-vb.md)   
 [IsolationLevel 和 Mode Properties (VC + +) ](./isolationlevel-and-mode-properties-example-vc.md)