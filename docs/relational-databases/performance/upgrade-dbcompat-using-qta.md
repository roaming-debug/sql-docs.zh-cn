---
title: 使用查询优化助手升级数据库
description: 了解查询优化助手如何指导完成推荐的工作流程，以在升级到较新的 SQL Server 版本期间保持性能稳定。
ms.custom: seo-dt-2019
ms.date: 02/13/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.querytuning.f1
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811e7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 1032f8e1ac32a08c9a6878c54fac0ee5e5c213e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015488"
---
# <a name="upgrading-databases-by-using-the-query-tuning-assistant"></a>使用查询优化助手升级数据库

[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

从较低版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或更高版本，且将[数据库兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)升级到最新可用级别时，工作负载可能会面临性能回归风险。 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 升级到任何较新版本时，出现此情况的可能性更小。

自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，每个新版本的所有查询优化器更改均限制为最新的数据库兼容性级别，因此系统不会在升级后立即更改执行计划，而是在用户将 `COMPATIBILITY_LEVEL` 数据库选项更改为最新可用版本后更改。 有关 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中引入的查询优化器更改的详细信息，请参阅[基数估计器](../../relational-databases/performance/cardinality-estimation-sql-server.md)。 要详细了解兼容性级别及其对升级的影响，请参阅[兼容性级别和数据库引擎升级](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)。

如果升级遵循以下建议工作流，那么将数据库兼容性级别提供的此限制功能与查询存储相结合，可在升级过程中拥有对查询性能很高的控制级别。 有关用于升级兼容性级别的建议工作流的详细信息，请参阅[更改数据库兼容性模式和使用查询存储](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)。 

![使用查询存储的推荐数据库升级工作流](../../relational-databases/performance/media/query-store-usage-5.png "使用查询存储的推荐数据库升级工作流") 

[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 引入了[自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)，进一步提升了对升级的控制，并且能够自动执行上述建议工作流中的最后一步。

自 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 起，新的查询优化助手 (QTA) 功能将在升级到更新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本期间指导用户完成建议工作流以保持性能稳定，如[查询存储使用方案](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade)的“在升级到新版 SQL Server 期间保持性能稳定”部分中所述。 不过，QTA 不会回滚到以前已知的优质计划，如建议的工作流的最后一步所示。 相反，QTA 会跟踪在[查询存储“回归查询”](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed)视图中找到的任何回归，并遍历适用的优化器模型变体的可能排列，以便生成新的更优质计划。

> [!IMPORTANT]
> QTA 不会生成用户工作负载。 如果在应用程序未使用的环境中运行 QTA，请确保可以通过其他方式在目标 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 上执行代表性测试工作负载。

## <a name="the-query-tuning-assistant-workflow"></a>查询优化助手工作流

QTA 的起点假设将数据库从以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（通过 [CREATE DATABASE ...FOR ATTACH](../..//relational-databases/databases/attach-a-database.md) 或 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)）移动到更新版本的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]，并且升级前的数据库兼容性级别不会立即更改。 QTA 将引导用户完成以下步骤：

1. 根据用户设置的工作负载持续时间（以天为单位）的建议设置来配置查询存储。 考虑与典型业务周期匹配的工作负载持续时间。
2. 请求启动所需的工作负载，以便查询存储可以收集工作负载数据的基线（若尚未提供）。
3. 升级到用户所选的目标数据库兼容性级别。
4. 请求收集第 2 次传递的工作负载数据，用于进行比较和回归检测。
5. 循环访问根据[查询存储回归的查询](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed)视图找到的任何回归，通过收集有关适用优化器模型变体的可能排列的运行时统计信息进行试验，并测量结果。 
6. 报告测量到的改进，并且可选择允许使用[计划指南](../../relational-databases/performance/plan-guides.md)保留那些更改。

有关附加数据库的详细信息，请参阅[数据库分离和附加](../../relational-databases/databases/database-detach-and-attach-sql-server.md#AttachDb)。

请参阅下文，了解 QTA 如何使用上文所述的查询存储，只更改升级兼容性级别建议工作流的最后几个步骤。 QTA 专为选定的回归查询提供了优化选择，用于使用优化的执行计划新建改进状态；而不是提供选项，供用户选择是使用当前低效的执行计划，还是使用最后一个已知的优质执行计划。

![使用 QTA 的推荐数据库升级工作流](../../relational-databases/performance/media/qta-usage.png "使用 QTA 的推荐数据库升级工作流")

### <a name="qta-tuning-internal-search-space"></a>QTA 优化内部搜索空间

QTA 仅面向可以从查询存储中执行的 `SELECT` 查询。 若编译参数是已知的，那么参数化查询符合条件。 此时，依赖于运行时构造（如临时表或表变量）的查询不符合条件。

QTA 面向 [基数估计器 (CE)](../../relational-databases/performance/cardinality-estimation-sql-server.md) 版本更改带来的查询回归已知可能的模式。 例如，将数据库从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和数据库兼容性级别 110 升级到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 和数据库兼容性级别 140 时，一些查询可能发生回归，因为它们是专为适用于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (CE 70) 中存在的 CE 版本而设计的。 这并不意味着从 CE 140 还原为 CE 70 是唯一选择。 只要较新版本中的某项特定更改引入了回归，就有可能提示查询只使用上一个 CE 版本中对特定查询更有效的相关部分，同时依然利用 CE 较新版本的所有其他改进。 还能使工作负载中未回归的其他查询获益于较新版 CE 的改进。

由 QTA 搜索到的 CE 模式如下：

- **独立性与相关性**：如果独立性假设为特定查询提供的评估更好，为了说明相关性，查询提示 `USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES')` 会使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成执行计划，方法是在对筛选器的 `AND` 谓词进行估算时使用最小选择性。 有关详细信息，请参阅 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)和 [CE 的版本](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce)。
- **简单包含与基础包含**：如果不同的联接包含为特定查询提供了更好的估计，则查询提示 `USE HINT ('ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS')` 会使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过使用“简单包含”假设（而不是默认的“基础包含”假设）来生成执行计划。 有关详细信息，请参阅 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)和 [CE 的版本](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce)。
- **多语句表值函数 (MSTVF) 固定基数猜测** - 100 行与1 行：如果使用 TVF 默认固定估值 100 行与使用 TVF 固定估值 1 行（对应于 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 及早期版本的查询优化器 CE 模型下的默认值）相比，并不能带来更有效的计划，则使用查询提示 `QUERYTRACEON 9488` 生成执行计划。 有关 MSTVF 的详细信息，请参阅[创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF)。

> [!NOTE]
> 万不得已时，如果狭窄范围的提示不能为符合条件的查询模式带来足够好的结果，也可考虑使用查询提示 `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')` 生成执行计划，从而充分利用 CE 70。

> [!IMPORTANT]
> 任何提示都会强制执行可能在将来的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新中解决的特定行为。 我们建议仅在没有其他选择且打算每次新升级都重新访问提示的代码时，才应用提示。 强制行为可能导致工作负载无法受益于更新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中引入的增强功能。

## <a name="starting-query-tuning-assistant-for-database-upgrades"></a>为数据库升级启动查询优化助手

QTA 是一种基于会话的功能，它将会话状态存储在首次创建会话的用户数据库的 `msqta` 架构中。 可在一段时间内在单个数据库上创建多个优化会话，但是任何给定的数据库只能存在一个活动会话。

### <a name="creating-a-database-upgrade-session"></a>创建数据库升级会话

1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开对象资源管理器并连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。

2. 对于计划升级数据库兼容性级别的数据库，右键选择数据库名称，依次选择“任务”、“数据库升级”和“新建数据库升级会话”。

3. 在 QTA 向导窗口中，配置会话需要两个步骤：

    1. 在“设置”窗口中，将查询存储配置为捕获相当于要分析和优化的一个完整业务周期的工作负载数据。
        - 以天为单位输入预期的工作负载持续时间（最短为 1 天）。 这将用于提出推荐的查询存储设置，以暂时允许收集完整基线。 要确保能够分析在更改数据库兼容性级别后找到的任何回归查询，捕获良好的基线至关重要。 
        - 完成 QTA 工作流之后，设置用户数据库应处于的预期目标数据库兼容性级别。
        完成后，选择“下一步”。

       ![新的数据库升级会话设置窗口](../../relational-databases/performance/media/qta-new-session-setup.png "新的数据库升级设置窗口")  
  
    2. 在“设置”窗口中，有两列显示目标数据库中查询存储的“当前”状态以及“推荐”设置  。 
        - 默认选择“推荐”设置，但选择“当前”列的单选按钮会接受当前设置，还可以微调当前的查询存储配置。
        - 建议的“过时查询阈值”设置的数值是预期工作负载持续时间（以天为单位）的两倍。 这是因为查询存储需要保存有关基线工作负载和数据库升级后的工作负载的信息。
        完成后，选择“下一步”。

       ![新的数据库升级设置窗口](../../relational-databases/performance/media/qta-new-session-settings.png "新的数据库升级设置窗口")

       > [!IMPORTANT]
       > 建议的“最大大小”是适合于短时间工作负载的任意值。
       > 但是，请记住，对于非常密集的工作负载（即可能生成许多不同的计划时），该值可能不足以保存关于基线和数据库升级后的工作负载的信息。
       > 如果预料到会出现这种情况，请输入一个合适的较大值。

4. “优化”窗口结束会话配置，并引导完成打开并继续处理会话的后续步骤。 完成后，选择“完成”。

    ![新的数据库升级优化窗口](../../relational-databases/performance/media/qta-new-session-tuning.png "新的数据库升级优化窗口")

> [!NOTE]
> 一种可能的替代方案是将数据库备份从生产服务器（其中，数据库已完成建议的数据库兼容性升级工作流）还原到测试服务器。

### <a name="executing-the-database-upgrade-workflow"></a>执行数据库升级工作流

1. 对于计划升级数据库兼容性级别的数据库，右键选择数据库名称，依次选择“任务”、“数据库升级”和“监视会话”。

2. “会话管理”页列出范围内数据库的当前和过去的会话。 选择所需会话，然后选择“详细信息”。

    > [!NOTE]
    > 如果当前会话不存在，请选择“刷新”按钮。

    列表包含以下信息：
    - **会话 ID**
    - **会话名称**：系统生成的名称由数据库名称以及创建会话的日期和时间组成。
    - **状态**：会话状态（活动或关闭）。
    - **说明**：系统生成的说明，包括用户所选的目标数据库兼容性级别以及业务周期工作负载天数。
    - **开始时间**：创建会话的日期和时间。

    ![QTA 会话管理页面](../../relational-databases/performance/media/qta-session-management.png "QTA 会话管理页面")

    > [!NOTE]
    > **删除会话**：删除为所选会话存储的任何数据。
    > 然而，删除已关闭的会话不会删除之前部署的任何计划指南。
    > 如果删除已部署计划指南的会话，则无法使用 QTA 进行回滚。
    > 改为使用 [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) 系统表搜索计划指南，然后使用 [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md) 手动删除。
  
3. 新会话的入口点是“数据收集”步骤。

    > [!NOTE]
    > 通过“会话”按钮可回到“会话管理”页面，而活动会话保持不变 。

    此步骤有以下三个子步骤：

    1. **基线数据收集**：要求用户运行代表性工作负载周期，以便查询可以收集基线。 完成此工作负载后，选中“工作负载运行已完成”并选择“下一步”。

        > [!NOTE]
        > 运行工作负载时，可关闭 QTA 窗口。 稍后回到仍处于活动状态的会话会从停止的那一步继续。 

        ![QTA 步骤 2 子步骤 1](../../relational-databases/performance/media/qta-step2-substep1.png "QTA 步骤 2 子步骤 1")

    2. **升级数据库**：会提示需要将数据库兼容性级别升级到期望目标的权限。 若要继续进行下一个子步骤，请选择“是”。

        ![QTA 步骤 2 子步骤 2 - 升级数据库兼容性级别](../../relational-databases/performance/media/qta-step2-substep2-prompt.png "QTA 步骤 2 子步骤 2 - 升级数据库兼容性级别")

        下一页确认已成功升级数据库兼容性级别。

        ![QTA 步骤 2 子步骤 2](../../relational-databases/performance/media/qta-step2-substep2.png "QTA 步骤 2 子步骤 2")

    3. “观测到的数据收集”要求用户重新运行代表性工作负载周期，以便查询存储可收集用于搜索优化机会的比较基线。 执行工作负载时，使用“刷新”按钮持续更新回归的查询列表（若找到任何回归的查询）。 更改“要显示的查询数”值，以限制显示的查询数量。 列表顺序受“指标”（持续时间或 CpuTime）和“聚合”（默认使用平均值） 。 还需选择要显示的查询数量。 完成此工作负载后，选中“工作负载运行已完成”并选择“下一步”。

        ![QTA 步骤 2 子步骤 3](../../relational-databases/performance/media/qta-step2-substep3.png "QTA 步骤 2 子步骤 3")

        列表包含以下信息：
        - **查询 ID** 
        - 查询文本：可通过选择“...”按钮展开的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。
        - **运行次数**：显示为整个工作负载收集执行该查询的次数。
        - **基线指标**：升级数据库兼容性之前，为收集基线数据选定的单位为毫秒的指标（持续时间或 CpuTime）。
        - **观测到的指标**：升级数据库兼容性之后，为收集数据选定的单位为毫秒的指标（持续时间或 CpuTime）。
        - **百分比变化**：数据库兼容性升级状态前后所选指标的百分比变化。 负数表示查询的测量的回归数量。
        - **可优化**：True 或 False，具体取决于查询是否有资格进行试验 。

4. **查看分析** 可选择要试验的查询并查找优化机会。 “要显示的查询数量”值成为要试验的符合条件的查询的范围。 选中所需查询后，选择“下一步”开始试验。

    > [!NOTE]
    > 不能选择“可优化”为“False”查询进行试验。

    > [!IMPORTANT]
    > 系统显示一个提示，告知在 QTA 进行到试验阶段后，不可能回到“查看分析”页面。   
    > 如果在前进到试验阶段之前未选择所有符合条件的查询，需在稍后创建新会话并重复工作流。 这需要将数据库兼容性级别重置回之前的值。

    ![QTA 步骤 3](../../relational-databases/performance/media/qta-step3.png "QTA 步骤 3")

5. **查看结果**：可选择要将建议优化部署为计划指南的查询。 

    列表包含以下信息：
    - **查询 ID**
    - 查询文本：可通过选择“...”按钮展开的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。
    - **状态**：显示查询的当前试验状态。
    - **基线指标**：为步骤 2 子步骤 3 中执行的查询选定的单位为毫秒的指标（持续时间或 CpuTime），表示升级数据库兼容性后的回归查询。
    - **观测到的指标**：为对试验后的查询进行很好的建议优化选定的单位为毫秒的指标（持续时间或 CpuTime），已进行足够好的建议优化。
    - **百分比变化**：试验状态前后所选指标的百分比变化，表示执行建议的优化后测得的查询的改进量。
    - **查询选项**：链接到改进查询执行指标的建议提示。
    - **可部署**：True 或 False，具体取决于是否能将建议的查询优化部署为计划指南 。

    ![QTA 步骤 4](../../relational-databases/performance/media/qta-step4.png "QTA 步骤 4")

6. **验证**：显示之前为此会话选定的查询的部署状态。 此页的列表不同于上一页，“可以部署”列更改为了“可以回滚” 。 此列可以为 True 或 False，具体取决于是否可以回滚部署的查询优化以及是否可以删除它的计划指南 。

    ![QTA 步骤 5](../../relational-databases/performance/media/qta-step5.png "QTA 步骤 5")

    如果之后需要对建议优化进行回滚，请选择相关查询，并选择“回滚”。 删除此查询计划指南并更新列表，以删除已回滚的查询。 请注意，在下图中，已删除查询 8。

    ![QTA 步骤 5 - 回退](../../relational-databases/performance/media/qta-step5-rollback.png "QTA 步骤 5 - 回退")

    > [!NOTE]
    > 删除已关闭的会话不会删除之前部署的任何计划指南。
    > 如果删除已部署计划指南的会话，则无法使用 QTA 进行回滚。
    > 改为使用 [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) 系统表搜索计划指南，然后使用 [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md) 手动删除。
  
## <a name="permissions"></a>权限

需要的成员资格为 db_owner 角色。
  
## <a name="see-also"></a>另请参阅

- [兼容性级别和数据库引擎升级](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)
- [性能监视和优化工具](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [更改数据库兼容性模式和使用 Query Store](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)
- [基数估计器](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)   
- [使用 SQL Server 查询优化助手](/learn/modules/use-sql-server-query-tuning-assistant/)