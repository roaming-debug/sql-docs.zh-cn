---
title: IServerVirtualDeviceSet2::RequestBuffers
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::RequestBuffers 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: ddbafcb7c9389fbe26252e341a9d73056a9c1ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347060"
---
# <a name="iservervirtualdeviceset2requestbuffers-vdi"></a>IServerVirtualDeviceSet2::RequestBuffers (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

RequestBuffers 函数通知 VDI 服务器将需要一定数量的、具有给定大小和对齐要求的缓冲区  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::RequestBuffers (
   DWORD   dwSize,
   DWORD   dwAlignment,
   DWORD   dwCount
);
```

## <a name="parameters"></a>parameters

*dwSize* 标识每个缓冲区的大小。 此大小只应包含数据所需的区域。 VDI 负责任何对齐和前缀要求。

*dwAlignment* 这些缓冲区要求的对齐方式。 相比于使用“BeginConfiguration”指定的基本对齐值，可以使用更严格的值。 如果该值为 0，则将默认为基本对齐值。

*dwCount* 具有给定大小和对齐方式的“AllocateBuffer”将请求的缓冲区数。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 函数成功。 |
| VD_E_ABORT | 已请求中止。 |
| VD_E_PROTOCOL | 该集未处于可能声明缓冲区分配的状态中（请参阅状态转换矩阵）。 |
| VD_E_MEMORY | 无法获取请求的内存。 |

## <a name="remarks"></a>备注

在使用 AllocateBuffer 分配缓冲区之前，应使用此方法。 使用 RequestBuffers 请求具有给定大小和对齐方式的缓冲区集，然后使用 AllocateBuffer 分配单个缓冲区。

在配置阶段，会将 RequestBuffers 调用“组合”在一起，以便在进行 EndConfiguration 调用时可以使用单个缓冲区区域（此时会对它进行分配）。 完成配置后，执行任何 RequestBuffers 调用都会立即分配更多缓冲区空间。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。