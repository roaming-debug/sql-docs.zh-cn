---
title: IServerVirtualDevice::SendCommand
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDevice::SendCommand 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f03e87863d21abfc4abf91811616cbacd14caff2
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125381"
---
# <a name="iservervirtualdevicesendcommand-vdi"></a>IServerVirtualDevice::SendCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

SendCommand 函数通过使用从 IServerVirtualDeviceSet2::OpenDevice 返回的虚拟设备对象向客户端发送命令  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDevice::SendCommand (
   VDS_Command*   pCmd
);
```

## <a name="parameters"></a>parameters

*pCmd* 这是一个指向命令请求块的指针。 有关详细信息，请参阅“命令”。 CompletionFunction 字段必须设置为指向具有以下签名的函数地址：

```c
void callbackFunction ( VDS_Command *pCmd);
```

当客户端指示某个命令已完成时，完成代理会进行此回调。 SQL Server 设置 pCmd 的 completionContext 字段。 其目的是提供回调函数的上下文。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 此命令已成功排队等待应用到客户端。 |
| VD_E_QUEUE_FULL | 设备队列已满。 |
| VD_E_IO_ERROR | 设备处于 IO 错误状态。 |
| VD_E_PROTOCOL | 设备不是活动的。 |

## <a name="remarks"></a>备注

如果尝试发送命令时出现错误，则会调用回调函数，并且命令缓冲区中的 completionCode 设置如下：

| completionCode | 错误 |
|---|---|
| VD_E_QUEUE_FULL | ERROR_NO_SYSTEM_RESOURCES |
| VD_E_IO_ERROR   | ERROR_IO_DEVICE |
| VD_E_PROTOCOL   | ERROR_INVALID_HANDLE |
| VD_E_ABORT      | ERROR_OPERATION_ABORTED |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。