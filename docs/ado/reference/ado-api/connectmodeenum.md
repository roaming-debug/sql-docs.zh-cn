---
description: ConnectModeEnum
title: ConnectModeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectModeEnum
helpviewer_keywords:
- ConnectModeEnum enumeration [ADO]
ms.assetid: 3792c294-5161-4538-a908-22a5fc50b85f
author: rothja
ms.author: jroth
ms.openlocfilehash: ec16f4d1db295e2160c587246cb9e77a921cfa72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167706"
---
# <a name="connectmodeenum"></a>ConnectModeEnum
指定可用于修改 [连接](./connection-object-ado.md)中的数据、打开 [记录](./record-object-ado.md)或为 **Record** 和 [Stream](./stream-object-ado.md)对象的 [Mode](./mode-property-ado.md)属性指定值的可用权限。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adModeRead**|1|指示只读权限。|  
|**adModeReadWrite**|3|指示读取/写入权限。|  
|**adModeRecursive**|0x400000|与其他 *\* ShareDeny \** 值一起使用 (**adModeShareDenyNone**、 **adModeShareDenyWrite** 或 **adModeShareDenyRead**) ，以将共享限制传播到当前 **记录** 的所有子记录。 如果 **记录** 没有任何子级，则不会产生任何影响。 如果将运行时错误仅用于 **adModeShareDenyNone** ，则会生成此错误。 但是，在与其他值结合使用时，可以将它与 **adModeShareDenyNone** 一起使用。 例如，可以使用 "**adModeRead** " 或 " **adModeShareDenyNone** " 或 " **adModeRecursive**"。|  
|**adModeShareDenyNone**|16|允许其他人打开具有任何权限的连接。 不能对其他人拒绝读取或写入访问权限。|  
|**adModeShareDenyRead**|4|阻止其他人打开具有读取权限的连接。|  
|**adModeShareDenyWrite**|8|阻止其他人打开具有写入权限的连接。|  
|**adModeShareExclusive**|12|阻止其他人打开连接。|  
|**adModeUnknown**|0|默认。 指示权限尚未设置或无法确定。|  
|**adModeWrite**|2|指示只写权限。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. ConnectMode. READ|  
|AdoEnums. ConnectMode|  
| (没有等效的 ConnectMode) |  
|AdoEnums.ConnectMode.SHAREDENYNONE|  
|AdoEnums.ConnectMode.SHAREDENYREAD|  
|AdoEnums.ConnectMode.SHAREDENYWRITE|  
|AdoEnums.ConnectMode.SHAREEXCLUSIVE|  
|AdoEnums. ConnectMode. 未知|  
|AdoEnums. ConnectMode|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [Mode 属性 (ADO)](./mode-property-ado.md)  
        [Open 方法（ADO 记录）](./open-method-ado-record.md)  
    :::column-end:::
    :::column:::
        [Open 方法（ADO 流）](./open-method-ado-stream.md)  
        [流对象 (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::