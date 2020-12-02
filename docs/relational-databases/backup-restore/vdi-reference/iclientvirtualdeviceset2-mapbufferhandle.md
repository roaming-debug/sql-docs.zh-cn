---
title: IClientVirtualDeviceSet2::MapBufferHandle
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::MapBufferHandle 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 70f6d117fd7a0349f2da2e03ff2603eaf55898fe
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128962"
---
# <a name="iclientvirtualdeviceset2mapbufferhandle-vdi"></a>IClientVirtualDeviceSet2::MapBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

MapBufferHandle 函数用于从通过其他某个进程获取的缓冲区句柄中获取有效的缓冲区地址  。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::MapBufferHandle (
   DWORD      dwBuffer,
   BYTE**      ppBuffer
);
```

## <a name="parameters"></a>parameters

*dwBuffer* 这是由 IClientVirtualDeviceSet2::GetBufferHandle 返回的句柄。

*ppBuffer* 这是当前进程中有效缓冲区的地址。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 函数成功。 |
| VD_E_PROTOCOL | 当前未打开虚拟设备集。 |
| VD_E_INVALID | ppBuffer 是无效的句柄。 |

## <a name="remarks"></a>备注

为了与句柄正确通信，必须格外谨慎。 对于单个虚拟设备集而言，句柄是本地的。 共享句柄的合作伙伴进程必须确保仅在最初获取缓冲区的虚拟设备集范围内使用缓冲区句柄。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。