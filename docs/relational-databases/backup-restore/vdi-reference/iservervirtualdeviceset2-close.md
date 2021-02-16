---
title: IServerVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::Close 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bc520d4cb4ded130c6d44a2fef70ddd4b5ff5d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347145"
---
# <a name="iservervirtualdeviceset2close-vdi"></a>IServerVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Close 函数关闭 IServerVirtualDeviceSet2::Open 打开的虚拟设备集  。 其结果是释放与虚拟设备关联的所有资源。 此函数返回后，IServerVirtualDeviceSet2 句柄并不起作用，它应返回到 COM。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| VD_E_PROTOCOL | 设备仍处于打开状态。 |

## <a name="remarks"></a>备注

不应在关闭设备前关闭虚拟设备集。 如果出现这种情况，则返回 VD_E_PROTOCOL。 此操作会导致 Close 立即释放其对共享内存的映射。 如果服务器继续要求从虚拟设备接口返回资源的所有权，则该服务器会受到访问冲突的影响。 接口执行 SignalAbort 处理。

如果完成代理正在运行，则在返回到其调用方之前，完成所有未完成的命令。 所有未完成的命令均通过 ERROR_OPERATION_ABORTED 完成。 也就是说，为每个未完成的命令调用回调函数。

在所有情况下，包括返回错误的情况，Close 均会释放虚拟设备接口的所有资源。 从 VDI 返回的任何缓冲区和其他接口指针均无效。

请务必确保在卸载 COM 库之前完成代理已终止。 如果在完成代理返回到其调用方之前卸载该库，则进程可能会导致指令冲突。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。