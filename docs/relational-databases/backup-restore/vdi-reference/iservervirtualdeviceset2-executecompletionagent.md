---
title: IServerVirtualDeviceSet2::ExecuteCompletionAgent
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::ExecuteCompletionAgent 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce53793d624c0a56fda48805c5c2fc8fc178e42c
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128884"
---
# <a name="iservervirtualdeviceset2executecompletionagent-vdi"></a>IServerVirtualDeviceSet2::ExecuteCompletionAgent (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

ExecuteCompletionAgent 函数用于实现完成代理的主循环  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::ExecuteCompletionAgent ();
```

## <a name="return-value"></a>返回值

返回 *HRESULT* ，指示方法调用是成功还是失败。 值 NOERROR 指示方法调用已成功。 非零值指示已发生错误。

## <a name="remarks"></a>备注

完成代理提供了一种机制，SQL Server 可以通过该机制将自身与虚拟设备命令完成操作同步。 它必须处于活动状态才能发出任何命令，因此它应保持活动状态，直到关闭所有设备。

由于 SQL Server 可能必须执行特殊的线程初始化，因此该接口不会启动新的控制线程。 SQL Server 会设置线程，然后将控制传递到此例程。 该线程在 Windows NT 进程间通信 (IPC) 机制上必须是可阻止的，并且必须能够调用带有通过 IServerVirtualDevice::SendCommand 发送的命令的任何回调函数。

在调用 IServerVirtualDeviceSet2::Close 或 SignalAbort 之前，不会返回此函数。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。