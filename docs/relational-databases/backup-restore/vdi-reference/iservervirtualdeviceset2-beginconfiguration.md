---
title: IServerVirtualDeviceSet2::BeginConfiguration
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::BeginConfiguration 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d1a2bd52e923f64bac8b1865cee5e18aa2f6345e
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125360"
---
# <a name="iservervirtualdeviceset2beginconfiguration-vdi"></a>IServerVirtualDeviceSet2::BeginConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

服务器调用 BeginConfiguration 函数以开始配置虚拟设备集  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::BeginConfiguration (
   DWORD   dwFeatures,
   DWORD   dwAlignment,
   DWORD   dwBlockSize,
   DWORD   dwMaxTransferSize,
   DWORD   dwTimeout
);
```

## <a name="parameters"></a>parameters

*dwFeatures* 已修改的功能掩码。 VDF_WriteMedia 和/或 VDF_ReadMedia。

*dwAlignment* 最终对齐方式。 如果为 0，则默认为 dwBlockSize。 必须是 2 的幂，>= dwBlockSize 且 <= 64 KB。

*dwBlockSize* 最小传输单位（以字节为单位）。 必须是 2 的幂，>=512 且 <= 64 KB。

*dwMaxTransferSize* 将尝试的最大传输。 必须是 64 KB 的倍数。

*dwTimeout* 等待主客户端完成它将提供的缓冲区声明的毫秒数。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 虚拟设备集处于可配置状态。 |
| VD_E_ABORT | 已调用 SignalAbort。 |
| VD_E_PROTOCOL | 虚拟设备集不处于已连接状态。 |

## <a name="remarks"></a>备注

调用此函数后，虚拟设备集将转变为可配置状态，在此状态下，将确定缓冲区布局。
（按照参数）设置基本配置后，这些值在虚拟设备集使用期间保持不变。 虚拟设备集的对齐属性用于控制数据缓冲区的对齐方式。 该值会设置在逐个缓冲区上可能会被替代的最小对齐值。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。