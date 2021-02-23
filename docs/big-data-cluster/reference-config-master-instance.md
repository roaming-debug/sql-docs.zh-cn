---
title: SQL Server 主实例配置属性
titleSuffix: SQL Server big data clusters
description: SQL Server 主实例配置属性的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343515"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>SQL Server 主实例配置属性 - CU9 之前的版本

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> 以下信息仅适用于未启用配置且需要 mssql-conf 来配置 SQL Server 主实例的 CU9 之前版本群集。 CU9 和更高版本的群集利用配置管理功能，不再需要 mssql-conf 文件。 可在[此处](reference-config-bdc-overview.md)找到 SQL Server 主实例和其他 BDC 组件的可用配置。

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
