---
title: IClientVirtualDeviceSet2::OpenDevice
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::OpenDevice 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6abf203001af56c11bb8d1cee3f233877fb8d51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347260"
---
# <a name="iclientvirtualdeviceset2opendevice-vdi"></a>IClientVirtualDeviceSet2::OpenDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

OpenDevice 函数将打开虚拟设备集中的某个设备  。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::OpenDevice (
   LPCWSTR                  lpName,
   IClientVirtualDevice**   ppVirtualDevice

);
```

## <a name="parameters"></a>parameters

*lpName* 用于标识虚拟设备。

*ppVirtualDevice* 如果此函数成功，会返回指向虚拟设备的指针。 此接口用于 GetCommand 和 CompleteCommand。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 函数成功。 |
| VD_E_ABORT | 已请求中止。 |
| VD_E_OPEN |所有设备均处于打开状态。 |
| VD_E_PROTOCOL | 设备集未处于正在初始化状态或此特定设备已打开。 |
| VD_E_INVALID | 设备名称无效。 它不是构成设备集的已知名称之一。 |

## <a name="remarks"></a>备注

可能返回 VD_E_OPEN 且无任何问题。 在返回此代码前，客户端可能通过循环方式调用 OpenDevice。
如果配置了多台设备（例如，n 台设备），虚拟设备集将返回 n 个唯一的设备接口。 第一台设备的名称与虚拟设备集的名称相同。 其他设备的名称由 BACKUP/RESTORE 语句的 VIRTUAL_DEVICE 子句指定。

此 GetConfiguration 函数可用于在可以打开设备前一直等待。

如果此函数不成功，将通过 ppVirtualDevice 返回一个 null 值。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。