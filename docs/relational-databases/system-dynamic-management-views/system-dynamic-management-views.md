---
description: " (Transact-sql) 的动态管理视图"
title: 动态管理视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], about dynamic management objects
- server state information [SQL Server]
- dynamic management functions [SQL Server]
- metadata [SQL Server], dynamic management objects
- dynamic management views [SQL Server]
- DMVs [SQL Server]
- functions [SQL Server], dynamic management
- server scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server]
ms.assetid: cf893ecb-0bf6-4cbf-ac00-8a1099e405b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 856777121b7d613c9ac28843c98520d07a93ceac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196271"
---
# <a name="system-dynamic-management-views"></a>系统动态管理视图
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  动态管理视图和函数返回可用于监视服务器实例的运行状况、诊断故障以及优化性能的服务器状态信息。  
  
> [!IMPORTANT]  
>  动态管理视图和函数返回特定于实现的内部状态数据。 在未来的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中，它们的架构和返回的数据可能会发生更改。 因此，未来版本中的动态管理视图和函数可能与此版本中的动态管理视图和函数不兼容。 例如，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，Microsoft 可能会通过在列列表的末尾添加列来扩充任何动态管理视图的定义。 我们建议不要在生产代码中使用语法 `SELECT * FROM dynamic_management_view_name`，这是因为返回的列数可能会更改和中断应用程序。  
  
 动态管理视图和函数分为两种类型：  
  
-   服务器范围内的动态管理视图和函数。 此类型需要具有该服务器的 VIEW SERVER STATE 权限。  
  
-   数据库范围内的动态管理视图和函数。 此类型需要具有该数据库的 VIEW DATABASE STATE 权限。  
  
## <a name="querying-dynamic-management-views"></a>查询动态管理视图  
 通过使用两部分、三部分或四部分所组成的名称，可在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中引用动态管理视图。 另一方面，也可使用两部分或三部分所组成的名称在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中引用动态管理函数。 不能使用只由一部分组成的名称在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中引用动态管理视图和函数。  
  
 所有动态管理视图和函数都存在于 sys 架构中，并遵循 dm_* 命名约定。 当使用动态管理视图或函数时，必须使用 sys 架构作为视图或函数名称的前缀。 例如，若要查询 dm_os_wait_stats 动态管理视图，请运行以下查询：  
  
 ```sql
SELECT wait_type, wait_time_ms  
FROM sys.dm_os_wait_stats;  
```  
  
### <a name="required-permissions"></a>所需权限  
 查询动态管理视图或函数需要对于对象具有 SELECT 权限以及 VIEW SERVER STATE 或 VIEW DATABASE STATE 权限。 这样您可以有选择地限制用户或登录名对动态管理视图和函数的访问。 为此，首先在 master 中创建用户，然后拒绝该用户对不希望被访问的动态管理视图或函数的 SELECT 权限。 此后，无论该用户的数据库上下文如何，用户都将无法选择这些动态管理视图或函数。  
  
> [!NOTE]  
>  由于 DENY 的优先级高，所以如果用户被授予 VIEW SERVER STATE 权限但被拒绝 VIEW DATABASE STATE 权限，则该用户只能查看服务器级别信息，但不能查看数据库级别信息。  
  
## <a name="in-this-section"></a>本节内容  
 动态管理视图和函数划分为以下类别。  

:::row:::
    :::column:::
        [Always On 可用性组动态管理视图和函数 (Transact-sql) ](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)

        [与变更数据捕获相关的动态管理视图 (Transact-SQL)](change-data-capture-sys-dm-cdc-errors.md)

        [与更改跟踪相关的动态管理视图](change-tracking-sys-dm-tran-commit-table.md)

        [与公共语言运行时相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)

        [与数据库镜像相关的动态管理视图 &#40;Transact-sql&#41;](database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)

        [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)

        [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)

        [扩展事件动态管理视图](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)

        [Filestream 和 FileTable 动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)

        [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)

        [异地复制动态管理视图和函数 &#40;Azure SQL 数据库&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)

        [与索引相关的动态管理视图和函数 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)

        [与 SQL&#41;相关的动态管理视图和函数 &#40;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)

        [与对象相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)

        [与查询通知相关的动态管理视图 &#40;Transact-sql&#41;](query-notifications-sys-dm-qn-subscriptions.md)

        [与复制相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)

        [与资源调控器相关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

        [与安全性相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)

        [与服务器相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)

        [与 Service Broker 有关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)

        [与空间数据相关的动态管理视图和函数 &#40;Transact-sql&#41;](./spatial-data-sys-dm-db-objects-disabled-on-compatibility-level-change.md)

        [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)

        [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)

        [&#40;Transact-sql 的 Stretch Database 动态管理视图&#41;]()

        [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;授予服务器权限 ](../../t-sql/statements/grant-server-permissions-transact-sql.md)   
 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)  
  
