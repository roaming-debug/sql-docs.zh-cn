---
title: IServerVirtualDeviceSet2::IsSharedBuffer
titlesuffix: SQL Server VDI reference
description: 本文提供了 IServerVirtualDeviceSet2::IsSharedBuffer 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 47d615bd8996d07444542549623d6849184815c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347096"
---
# <a name="iservervirtualdeviceset2issharedbuffer-vdi"></a>IServerVirtualDeviceSet2::IsSharedBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

IsSharedBuffer 函数确定给定的缓冲区地址是否指的是 AllocateBuffer 方法提供的某个共享缓冲区  。

## <a name="syntax"></a>语法

```c
HRESULT IServerVirtualDeviceSet2::IsSharedBuffer (
   LPVOID   lpBuffer
);
```

## <a name="parameters"></a>parameters

*lpBuffer* 这是缓冲区的地址。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| TRUE | 该缓冲区是共享缓冲区。 |
| FALSE | 该缓冲区不是虚拟设备集的一部分。 |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。