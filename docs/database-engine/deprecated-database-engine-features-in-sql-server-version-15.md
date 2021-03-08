---
description: '[!INCLUDE[sssql19-md](../includes/sssql19-md.md)] 中弃用的数据库引擎功能'
title: SQL Server 2019 中弃用的数据库引擎功能 | Microsoft Docs
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4e322ff478e9ccdc031882e7c3b5b18fd8506e42
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186440"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>SQL Server 2019 (15.x) 中不推荐使用的数据库引擎功能

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

除了先前版本中已弃用的功能之外，[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] 不弃用任何功能：

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>弃用准则

如果功能标记为已弃用，表示：

- 该功能仅处于维护模式。 不进行新的更改，包括使用新功能解决互操作性的相关更改。
- 我们努力不从将来的版本中删除已弃用的功能，使升级更简单。 但是，在极少数情况下，如果该功能限制了将来的创新，我们可能选择从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中将其永久停用（删除）。
- 请勿在新的开发工作中使用已弃用的功能。 对于现有应用，计划尽快修改当前使用这些功能的应用程序。     

可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Deprecated Features Object 性能计数器或 `deprecation_announcement` 和 `deprecation_final_support` 扩展事件监视已弃用功能的使用情况。 有关详细信息，请参阅 [使用 SQL Server 对象](../relational-databases/performance-monitor/use-sql-server-objects.md)。  

## <a name="query-deprecated-features"></a>查询已弃用的功能

这些计数器的值也可通过执行以下语句而得：  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>请参阅

- [SQL Server 2019 中数据库引擎功能的重大更改](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [SQL Server 中弃用的数据库引擎功能](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [SQL Server 数据库引擎的后向兼容性](./discontinued-database-engine-functionality-in-sql-server.md)
