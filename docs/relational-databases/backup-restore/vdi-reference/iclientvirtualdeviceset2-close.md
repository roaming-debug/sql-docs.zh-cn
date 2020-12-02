---
title: IClientVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::Close 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: ccac23e8049884ba7bd403c91f6bde8a1f29cf84
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128979"
---
# <a name="iclientvirtualdeviceset2close-vdi"></a>IClientVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Close 函数用于关闭 IClientVirtualDeviceSet2::Create 创建的虚拟设备集  。 其结果是释放与虚拟设备集关联的所有资源。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 将在虚拟设备集成功关闭后返回。 |
| VD_E_PROTOCOL | 未采取任何操作，因为虚拟设备集未打开。 |
| VD_E_OPEN | 设备仍处于打开状态。 |

## <a name="remarks"></a>备注

对 Close 的调用是由客户端做出的声明，表示应释放虚拟设备集所用的所有资源。 调用 Close 前，客户端须确保已终止涉及数据缓冲区和虚拟设备的所有活动。 Close 会使由 OpenDevice 返回的所有虚拟设备接口失效。

在返回 Close 调用后，允许客户端在虚拟设备集接口上发出一个 Create 调用。 此类调用将为后续的 BACKUP 或 RESTORE 操作创建新的虚拟设备集。

如果在一台或多台虚拟设备处于打开状态时调用 Close，则会返回 VD_E_OPEN。 在这种情况下，如果可能，将在内部触发 SignalAbort 以确保正确关闭。 将释放 VDI 资源。 调用 IClientVirtualDeviceSet2::Close 前，客户端应在每台设备上等待 VD_E_CLOSE 指示。 如果客户端知道虚拟设备集已处于异常终止状态，则它应不会收到 GetCommand 发出的 VD_E_CLOSE 指示，并且当共享缓冲区上的活动终止后，它可以调用 IClientVirtualDeviceSet2::Close。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。