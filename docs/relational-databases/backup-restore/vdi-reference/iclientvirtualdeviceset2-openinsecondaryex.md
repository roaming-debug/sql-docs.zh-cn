---
title: IClientVirtualDeviceSet2::OpenInSecondaryEx
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDeviceSet2::OpenInSecondaryEx 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 18e53141365aa4581cef05c14d35cf53c8f24387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347213"
---
# <a name="iclientvirtualdeviceset2openinsecondaryex-vdi"></a>IClientVirtualDeviceSet2::OpenInSecondaryEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

OpenInSecondaryEx 函数打开次要客户端中的虚拟设备集  。 主客户端必须已使用 CreateEx 和 GetConfiguration 设置了虚拟设备集。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDeviceSet2::OpenInSecondaryEx (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpSetName
);
```

## <a name="parameters"></a>parameters

*lpInstanceName* 此字符串标识将向其发送 SQL 命令的 SQL Server 实例。

*lpSetName* 用于标识设备集。 此名称区分大小写，且必须与主客户端调用 IClientVirtualDeviceSet2::Create 时所用的名称相匹配。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 函数成功。 |
| VD_E_PROTOCOL | 虚拟设备集已打开，或者虚拟设备集尚未准备好接受来自次要客户端的打开请求。 |
| VD_E_ABORT | 正在中止操作。 |

## <a name="remarks"></a>备注

使用多个进程模型时，主要客户端负责检测次要客户端的正常或异常终止。

实例名称必须标识要向发出 T-SQL 的实例。 NULL 标识默认实例。 不接受 "machineName\" 前缀。

OpenInSecondaryEx 取代原始 SQL Server 7.0 版接口中定义的原始 IClientVirtualDeviceSet::OpenInSecondary。 新开发应使用 OpenInSecondaryEx。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。