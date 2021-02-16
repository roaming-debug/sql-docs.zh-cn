---
title: IServerVirtualDeviceSet2::SignalAbort
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::SignalAbort 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7dd21dfb56d093a6d35d977577971545c3fb1263
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347052"
---
# <a name="iservervirtualdeviceset2signalabort-vdi"></a>IServerVirtualDeviceSet2::SignalAbort (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

SignalAbort 函数发出应终止异常的信号  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::SignalAbort ();
```

## <a name="return-value"></a>返回值

返回 *HRESULT* ，指示方法调用是成功还是失败。 值 NOERROR 指示方法调用已成功。 非零值指示已发生错误。

## <a name="remarks"></a>备注

任何时候，服务器都可以选择中止 BACKUP 或 RESTORE 操作。

此例程发出信号：应停止所有操作。 整个接口进入中止状态。 任何设备上都不会接受进一步的命令。 完成代理将为每个未完成的请求返回 ERROR_OPERATION_ABORTED，并返回到其调用方。 将忽略在客户端上记录的任何完成操作。

服务器确保不再需要使用从虚拟设备接口返回的缓冲区或设备。 服务器随后会执行异常终止清除操作，其中应包括调用 IServerVirtualDeviceSet2::Close 函数。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。