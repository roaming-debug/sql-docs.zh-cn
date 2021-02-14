---
title: SQL Server 主实例配置属性
titleSuffix: SQL Server big data clusters
description: SQL Server 主实例配置属性的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2f251357c818577b0ecd761c4a5ca2f030eeca58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043967"
---
# <a name="sql-server-master-instance-configuration-properties"></a>SQL Server 主实例配置属性

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

## <a name="properties"></a>属性

可以在部署时为主实例配置以下 SQL Server 选项。

|属性|选项|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>示例

以下示例启用 SQL 代理、遥测，为 Enterprise Edition 设置 PID 并启用了跟踪标志 1204.：

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

有关说明，请参阅[配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主实例](configure-sql-server-master-instance.md)。

## <a name="next-steps"></a>后续步骤

[配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主实例](configure-sql-server-master-instance.md)
