---
title: IClientVirtualDevice::CompleteCommand
titlesuffix: SQL Server VDI reference
description: 本文提供了 IClientVirtualDevice::CompleteCommand 命令的参考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 367d398160daa8b9a9da5bfc3a3285cf61b6d085
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128994"
---
# <a name="iclientvirtualdevicecompletecommand-vdi"></a>IClientVirtualDevice::CompleteCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

CompleteCommand 函数用于通知 SQL Server 命令已完成  。 应返回相应的命令完成信息。

## <a name="syntax"></a>语法

```c
HRESULT IClientVirtualDevice::CompleteCommand (
   VDC_Command*         const pCmd,
   UINT32               dwCompletionCode,
   UINT32               dwBytesTransferred,
   UINT64               dwlPosition
);
```

## <a name="parameters"></a>parameters

*pCmd* 这是之前从 IClientVirtualDevice::GetCommand 返回的命令的地址。

*dwCompletionCode* 这是指示完成状态的 WIN32 状态代码。 必须为所有命令返回此参数。 返回的代码应适合于正在执行的命令。 在任何情况下，ERROR_SUCCESS 均用于表示已成功执行的命令。 有关可能的代码的完整列表，请参阅文件 Winerror.h。 有关每个命令的典型状态代码列表，请参阅“命令”。

*dwBytesTransferred* 这是已成功传输的字节数。 仅为数据传输命令 Read 和 Write 返回。

*dwlPosition* 这是仅针对 GetPosition 命令的响应。

## <a name="return-value"></a>返回值

|返回值 | 说明 |
|---|---|
| NOERROR | 已正确标注完成情况。 |
| VD_E_INVALID | pCmd 不是一条活动命令。 |
| VD_E_ABORT | 已发出中止的信号。 |
| VD_E_PROTOCOL | 设备未打开。 |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [SQL Server 虚拟设备接口引用概述](reference-virtual-device-interface.md)。
