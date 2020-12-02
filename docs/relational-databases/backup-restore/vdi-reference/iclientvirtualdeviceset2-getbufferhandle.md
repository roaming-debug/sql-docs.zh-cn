---
title: IClientVirtualDeviceSet2::GetBufferHandle
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::GetBufferHandle 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1c3b771e3b5ec69cbfb33a21496ffd0455d0b2a7
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128975"
---
# <a name="iclientvirtualdeviceset2getbufferhandle-vdi"></a>IClientVirtualDeviceSet2::GetBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

某些应用程序可能需要多个进程，以对由 IClientVirtualDevice2::GetCommand 返回的缓冲区进行操作  。 在这种情况下，接收命令的进程可使用 GetBufferHandle 获取用于标识缓冲区的、与进程无关的句柄  。 随后可将此句柄传达给打开同一虚拟设备集的其他任何进程。 然后，此进程将使用 IClientVirtualDeviceSet2::MapBufferHandle 获取缓冲区的地址。 因为每个进程可能映射不同地址中的缓冲区，因此该地址可能与其伙伴进程中的地址不同。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::GetBufferHandle (
   BYTE*         pBuffer,
   DWORD*      pBufferHandle
);
```

## <a name="parameters"></a>parameters

*pBuffer* 这是通过 Read 或 Write 命令获取的缓冲区的地址。

*pBufferHandle* 返回缓冲区的唯一标识符。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 函数成功。 |
| VD_E_PROTOCOL | 当前未打开虚拟设备集。 |
| VD_E_INVALID | pBuffer 不是有效地址。 |

## <a name="remarks"></a>备注

调用 GetBufferHandle 函数的进程负责在数据传输完成后调用 IClientVirtualDevice2::CompleteCommand。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。