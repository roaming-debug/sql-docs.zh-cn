---
title: IClientVirtualDevice::GetCommand
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDevice::GetCommand 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 226c11ebde66450431719c8d6c2e027479eb3ca4
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125415"
---
# <a name="iclientvirtualdevicegetcommand-vdi"></a>IClientVirtualDevice::GetCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

GetCommand 函数用于获取排队等待应用到设备的下一条命令  。 请求函数后，此函数将等待下一个命令。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDevice::GetCommand (
   DWORD               dwTimeOut,
   VDC_Command**      const ppCmd
);
```

## <a name="parameters"></a>parameters

*ppCmd* 成功返回命令后，该参数将返回要执行的命令的地址。 返回的内存是只读的。 命令完成后，会将此指针传递到 CompleteCommand 例程。 有关每个命令的详细信息，请参阅“命令”。

*dwTimeOut* 此为等待时间（毫秒）。 使用 INFINTE 可无限期等待。 使用 0 可轮询命令。 如果当前无可用的命令，则返回 VD_E_TIMEOUT。 如果发生超时，客户端将决定后续操作。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 已获取命令。 |
| VD_E_CLOSE | 服务器已关闭该设备。 |
| VD_E_TIMEOUT | 无可用命令且超时时间已过。 |
| VD_E_ABORT | 客户端或服务器已使用 SignalAbort 执行强制关闭。 |

## <a name="remarks"></a>备注

如果返回 VD_E_CLOSE，表示 SQL Server 已关闭设备。 这是正常关闭操作的一部分。 在所有设备均已关闭后，客户端将调用 IClientVirtualDeviceSet2::Close 以关闭虚拟设备集。

如果必须阻塞此例程以等待命令，则该线程将停留在可发出警报的状态。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。