---
title: IClientVirtualDeviceSet2::SignalAbort
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::SignalAbort 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: b03452441b99cd477f0901e3868109867c66e2b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347194"
---
# <a name="iclientvirtualdeviceset2signalabort-vdi"></a>IClientVirtualDeviceSet2::SignalAbort (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

**SignalAbort** 此函数用于发出应终止异常的信号。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::SignalAbort ();
```

## <a name="return-value"></a>返回值

返回 *HRESULT* ，指示方法调用是成功还是失败。 值 NOERROR 指示方法调用已成功。 非零值指示已发生错误。

## <a name="remarks"></a>备注

任何时候，客户端都可以选择中止 BACKUP 或 RESTORE 操作。 此例程发出信号：应停止所有操作。 整个虚拟设备集的状态进入中止状态。 任何设备上都不会返回进一步的命令。 所有未完成的命令将自动完成，并返回 ERROR_OPERATION_ABORTED 作为完成代码。 在安全终止了提供给客户端的任何尚未使用的缓冲区以后，客户端应调用 IClientVirtualDeviceSet2::Close。 有关详细信息，请参阅异常终止。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。