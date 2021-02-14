---
title: sys.dm_db_tuning_recommendations (Transact-sql) |Microsoft Docs
description: 了解如何在 SQL Server 和 Azure SQL 数据库中查找潜在的性能问题和建议的修补程序
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_tuning_recommendations
- dm_db_tuning_recommendations
- sys.dm_db_tuning_recommendations_TSQL
- dm_db_tuning_recommendations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database tuning recommendations feature [SQL Server], sys.dm_db_tuning_recommendations dynamic management view
- sys.dm_db_tuning_recommendations dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 401636f25e34c3c76faed1cc73fc54b9e2b911bf
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489341"
---
# <a name="sysdm_db_tuning_recommendations-transact-sql"></a>sys.dm \_ db \_ 优化 \_ 建议 (transact-sql) 
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  返回有关优化建议的详细信息。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。

| **列名** | **数据类型** | **说明** |
| --- | --- | --- |
| name  | **nvarchar(4000)** | 建议的唯一名称。 |
| type  | **nvarchar(4000)** | 生成建议的自动优化选项的名称，例如 `FORCE_LAST_GOOD_PLAN` |
| **reason** | **nvarchar(4000)** | 提供此建议的原因。 |
| **有效 \_ 时间** | **datetime2** | 第一次生成此建议时。 |
| **上次 \_ 刷新时间** | **datetime2** | 上次生成此建议的时间。 |
| State  | **nvarchar(4000)** | 描述建议状态的 JSON 文档。 可用字段如下：<br />-   `currentValue` -建议的当前状态。<br />-   `reason` -常量，用于描述建议处于当前状态的原因。|
| **是 \_ 可执行 \_ 操作** | **bit** | 1 = 可通过脚本对数据库执行建议 [!INCLUDE[tsql_md](../../includes/tsql-md.md)] 。<br />0 = 无法对数据库执行建议 (例如：仅信息或已还原的建议)  |
| **是 \_ revertable \_ 操作** | **bit** | 1 = 数据库引擎可以自动监视和还原建议。<br />0 = 不能自动监视和还原建议。 大多数 &quot; 可执行 &quot; 的操作将为 &quot; revertable &quot; 。 |
| **执行 \_ 操作 \_ 开始 \_ 时间** | **datetime2** | 应用建议的日期。 |
| **执行 \_ 操作 \_ 持续时间** | **time** | 执行操作的持续时间。 |
| **执行 \_ 操作 \_ 启动 \_ 者** | **nvarchar(4000)** | `User` = 建议用户手动强制计划。 <br /> `System` = 系统自动应用建议。 |
| **执行 \_ 操作 \_ 启动 \_ 时间** | **datetime2** | 应用建议的日期。 |
| **还原 \_ 操作 \_ 开始 \_ 时间** | **datetime2** | 还原建议的日期。 |
| **还原 \_ 操作 \_ 持续时间** | **time** | 还原操作的持续时间。 |
| **还原 \_ 操作 \_ 启动 \_ 者** | **nvarchar(4000)** | `User` = 用户手动 unforced 推荐的计划。 <br /> `System` = 系统自动还原建议。 |
| **还原 \_ 操作 \_ 启动 \_ 时间** | **datetime2** | 还原建议的日期。 |
| **分值** | **int** | 针对0-100 缩放的此建议的预计值/影响 (更好)  |
| **详细** | **nvarchar(max)** | 包含有关建议的更多详细信息的 JSON 文档。 可用字段如下：<br /><br />`planForceDetails`<br />-    `queryId` - \_ 回归查询的查询 id。<br />-    `regressedPlanId` -回归计划的 plan_id。<br />-   `regressedPlanExecutionCount` -检测到回归之前，执行具有回归计划的查询的次数。<br />-    `regressedPlanAbortedCount` -执行回归计划期间检测到的错误数。<br />-    `regressedPlanCpuTimeAverage` -检测到回归之前，回归查询使用的平均 CPU 时间 (（微秒）) 。<br />-    `regressedPlanCpuTimeStddev` -检测到回归之前，回归查询占用的 CPU 时间的标准偏差。<br />-    `recommendedPlanId` -应强制执行的计划 plan_id。<br />-   `recommendedPlanExecutionCount`-在检测到回归之前应强制执行的查询的执行次数。<br />-    `recommendedPlanAbortedCount` -执行应强制执行的计划期间检测到的错误数。<br />-    `recommendedPlanCpuTimeAverage` - (用计划执行的查询所使用的平均 CPU 时间) ，该查询使用的是在检测到回归之前应强制 (计算的计划) 。<br />-    `recommendedPlanCpuTimeStddev` 检测到回归之前，回归查询占用的 CPU 时间的标准偏差。<br /><br />`implementationDetails`<br />-  `method` -应该用于更正回归的方法。 值始终为 `TSql` 。<br />-    `script` - [!INCLUDE[tsql_md](../../includes/tsql-md.md)] 为了强制建议的计划而应执行的脚本。 |
  
## <a name="remarks"></a>备注  
 `sys.dm_db_tuning_recommendations`当数据库引擎识别潜在的查询性能回归时，将更新返回的信息，并且不会保留。 建议仅在重启后才会保留 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果数据库管理员要在服务器回收后保留这些建议，则应该定期制作优化建议的备份副本。 

 `currentValue` 列中的字段 `state` 可能包含以下值：
 
 | 状态 | 描述 |
 |--------|-------------|
 | `Active` | 建议处于活动状态且尚未应用。 用户可以采用建议脚本并手动执行它。 |
 | `Verifying` | 已应用建议 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ，并且内部验证过程会将强制计划的性能与回归计划进行比较。 |
 | `Success` | 已成功应用建议。 |
 | `Reverted` | 建议会还原，因为没有显著的性能提升。 |
 | `Expired` | 建议已过期，无法再应用。 |

列中的 JSON 文档 `state` 包含描述为何处于当前状态的原因。 "原因" 字段中的值可能是： 

| 原因 | 说明 |
|--------|-------------|
| `SchemaChanged` | 建议过期，因为引用的表的架构已更改。 如果在新架构上检测到新的查询计划回归，则将创建新的建议。 |
| `StatisticsChanged`| 由于所引用表的统计信息发生变化，建议过期。 如果基于新统计信息检测到新的查询计划回归，则将创建新的建议。 |
| `ForcingFailed` | 不能对查询强制执行建议的计划。 `last_force_failure_reason`在[sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)视图中找到，查找失败的原因。 |
| `AutomaticTuningOptionDisabled` | `FORCE_LAST_GOOD_PLAN` 在验证过程中，用户禁用了选项。 `FORCE_LAST_GOOD_PLAN`使用[ALTER DATABASE SET AUTOMATIC_TUNING &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)语句启用选项，或使用 "在列中编写脚本" 手动强制执行计划 `[details]` 。 |
| `UnsupportedStatementType` | 无法对查询强制执行计划。 不受支持的查询的示例包括游标和 `INSERT BULK` 语句。 |
| `LastGoodPlanForced` | 已成功应用建议。 |
| `AutomaticTuningOptionNotEnabled`| [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 已识别潜在的性能回归，但 `FORCE_LAST_GOOD_PLAN` 未启用选项-请参阅 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 手动应用建议或启用 `FORCE_LAST_GOOD_PLAN` 选项。 |
| `VerificationAborted`| 由于重新启动或查询存储清理，验证过程将中止。 |
| `VerificationForcedQueryRecompile`| 由于没有显著的性能改进，因此重新编译查询。 |
| `PlanForcedByUser`| 用户使用 [sp_query_store_force_plan &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) 过程手动强制计划。 如果用户显式决定强制实施某个计划，则数据库引擎将不应用建议。 |
| `PlanUnforcedByUser` | 用户使用 [sp_query_store_unforce_plan &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md) 过程手动 unforced 计划。 由于用户显式还原了推荐的计划，数据库引擎将继续使用当前计划并生成新的建议（如果将来出现某种计划回归）。 |
| `UserForcedDifferentPlan` | 用户使用 [sp_query_store_force_plan &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) 过程手动强制执行不同的计划。 如果用户显式决定强制实施某个计划，则数据库引擎将不应用建议。 |
| `TempTableChanged` | 在计划中使用的临时表已更改。 |

 "详细信息" 列中的统计信息不显示运行时计划统计信息 (例如，当前 CPU 时间) 。 建议的详细信息是在回归检测时进行的，并描述为何 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 发现性能回归。 使用 `regressedPlanId` 和 `recommendedPlanId` 查询 [查询存储目录视图](../../relational-databases/performance/how-query-store-collects-data.md) 以查找确切的运行时计划统计信息。

## <a name="examples-of-using-tuning-recommendations-information"></a>使用优化建议信息的示例  

### <a name="example-1"></a>示例 1
下面将获取 [!INCLUDE[tsql](../../includes/tsql-md.md)] 为任何给定查询强制执行良好计划的生成的脚本：  
 
```sql
SELECT name, reason, score,
    JSON_VALUE(details, '$.implementationDetails.script') AS script,
    details.* 
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON(details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressed_plan_id int '$.regressedPlanId',
            last_good_plan_id int '$.recommendedPlanId') AS details
WHERE JSON_VALUE(state, '$.currentValue') = 'Active';
```
### <a name="example-2"></a>示例 2
下面的内容获取生成的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，该脚本为任何给定查询强制执行良好计划，并获取有关预计收益的其他信息：

```sql
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  *(regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

### <a name="example-3"></a>示例 3
下面的内容获取生成的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，该脚本为任何给定的查询强制执行良好的计划，并获取其他信息，其中包括查询存储中存储的查询文本和查询计划：

```sql
WITH cte_db_tuning_recommendations
AS (SELECT reason,
        score,
        query_id,
        regressedPlanId,
        recommendedPlanId,
        current_state = JSON_VALUE(state, '$.currentValue'),
        current_state_reason = JSON_VALUE(state, '$.reason'),
        script = JSON_VALUE(details, '$.implementationDetails.script'),
        estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
        error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
    FROM sys.dm_db_tuning_recommendations
    CROSS APPLY OPENJSON(Details, '$.planForceDetails')
    WITH ([query_id] int '$.queryId',
        regressedPlanId int '$.regressedPlanId',
        recommendedPlanId int '$.recommendedPlanId',
        regressedPlanErrorCount int,    
        recommendedPlanErrorCount int,
        regressedPlanExecutionCount int,
        regressedPlanCpuTimeAverage float,
        recommendedPlanExecutionCount int,
        recommendedPlanCpuTimeAverage float
        )
    )
SELECT qsq.query_id,
    qsqt.query_sql_text,
    dtr.*,
    CAST(rp.query_plan AS XML) AS RegressedPlan,
    CAST(sp.query_plan AS XML) AS SuggestedPlan
FROM cte_db_tuning_recommendations AS dtr
INNER JOIN sys.query_store_plan AS rp ON rp.query_id = dtr.query_id
    AND rp.plan_id = dtr.regressedPlanId
INNER JOIN sys.query_store_plan AS sp ON sp.query_id = dtr.query_id
    AND sp.plan_id = dtr.recommendedPlanId
INNER JOIN sys.query_store_query AS qsq ON qsq.query_id = rp.query_id
INNER JOIN sys.query_store_query_text AS qsqt ON qsqt.query_text_id = qsq.query_text_id;
```

有关可用于查询建议视图中的值的 JSON 函数的详细信息，请参阅中的 [Json 支持](../json/json-data-sql-server.md) [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 。
  
## <a name="permissions"></a>权限  

需要 `VIEW SERVER STATE` 中的权限 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 。   
需要 `VIEW DATABASE STATE` 中数据库的权限 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。   

## <a name="see-also"></a>另请参阅  
 [自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [sys.database_automatic_tuning_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)   
 [sys.database_query_store_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [JSON 支持](../json/json-data-sql-server.md)
