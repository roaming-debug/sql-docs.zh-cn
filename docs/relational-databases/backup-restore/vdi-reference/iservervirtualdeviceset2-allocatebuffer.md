---
title: IServerVirtualDeviceSet2::AllocateBuffer
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::AllocateBuffer 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d311b2253e9083c78207d1443782096b4c82a491
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128945"
---
# <a name="iservervirtualdeviceset2allocatebuffer-vdi"></a>IServerVirtualDeviceSet2::AllocateBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

AllocateBuffer 函数获取虚拟设备集的共享内存缓冲区  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::AllocateBuffer (
   LPVOID*      ppBuffer,
   UINT32      dwSize,
   UINT32      dwAlignment
);
```

## <a name="parameters"></a>parameters

*ppBuffer* 这会返回指向缓冲区开头的指针。

*dwSize* 这是缓冲区的大小（以字节为单位）。 不包括客户端请求的任何前缀区域。 任何此类区域对服务器都是隐藏的，并且在返回缓冲地址之前会有可用空间。

*dwAlignment* 指定缓冲区的对齐边界。 例如，值为 4096 将确保缓冲区在 4096 字节的边界上对齐。 这意味着返回的地址会将低序位 12 位设为零。 此参数必须是 2 的幂。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 已返回缓冲区。 |
| VD_E_MEMORY | 出现内存不足的情况。 |
| VD_E_INVALID | 参数无效。 |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。