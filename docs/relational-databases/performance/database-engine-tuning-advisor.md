---
title: 数据库引擎优化顾问 | Microsoft Docs
description: 了解如何使用数据库引擎优化顾问进行故障排除、调整大量查询、分析设计更改以及管理 SQL Server 中的存储空间。
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.general.f1
ms.assetid: 50dd0a0b-a407-4aeb-bc8b-b02a793aa30a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fd814ec47548406eef0b8436751a034c97f4717
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505294"
---
# <a name="database-engine-tuning-advisor"></a>Database Engine Tuning Advisor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] 数据库引擎优化顾问 (DTA) 分析数据库并对优化查询性能提出建议。 借助数据库引擎优化顾问，您不必精通数据库结构或深谙 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，即可选择和创建索引、索引视图和分区的最佳集合。 使用 DTA，您可以执行以下任务。  
  
-   特定问题查询故障排除  
  
-   优化跨一个或多个数据库的大型查询集  
  
-   执行潜在物理设计更改的探索性“假设”分析  
  
-   管理存储空间  
  
## <a name="database-engine-tuning-advisor-benefits"></a>数据库引擎优化顾问优点  
 在未完全了解数据库结构和针对数据库运行的查询的情况下，优化查询性能较为困难。 数据库引擎优化顾问 (DTA) 可以简化此任务，也即，分析当前查询计划缓存，或分析所创建的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询的工作负载并提出适当的物理设计建议。 对于更高级的数据库管理员，DTA 公开一个强大机制，用于执行不同物理设计的探索性“假设”分析。 DTA 可以提供下列信息。  
  
-   通过使用查询优化器分析工作负荷中的查询，推荐数据库的最佳行存储和[列存储](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)索引组合。  
  
-   为工作负荷中引用的数据库推荐对齐分区或非对齐分区。  
  
-   推荐工作负荷中引用的数据库的索引视图。  
  
-   分析所建议的更改将会产生的影响，包括索引的使用，查询在表之间的分布，以及查询在工作负荷中的性能。  
  
-   推荐为执行一个小型的问题查询集而对数据库进行优化的方法。  
  
-   允许通过指定磁盘空间约束等高级选项对推荐进行自定义。  
  
-   提供对所给工作负荷的建议执行效果的汇总报告。  

-   考虑备选方案，即：您以假定配置的形式提供可能的设计结构方案，供数据库引擎优化顾问进行评估。

-   优化各种源（包括 SQL Server Query Store、计划缓存、SQL Server Profiler 跟踪文件，或者表或 .SQL 文件）中的工作负荷。

  
数据库引擎优化顾问旨在处理以下查询工作负载类型：  
  
-   仅联机事务处理 (OLTP) 查询  
  
-   仅联机分析处理 (OLAP) 查询  
  
-   OLTP 和 OLAP 混合查询  
  
-   大量查询工作负荷（查询多于数据修改）  
  
-   大量更新工作负荷（数据修改多于查询）  
  
## <a name="dta-components-and-concepts"></a>DTA 组件和概念  
 **数据库引擎优化顾问图形用户界面**  
 一个易于使用的界面，可用来指定工作负荷和选择各种优化选项。  
  
 **dta** 实用工具  
 数据库引擎优化顾问的命令提示符版本。 通过 **dta** 实用工具，您可以在应用程序和脚本中使用数据库引擎优化顾问功能。  
  
 **工作负载**  
 Transact-SQL 脚本文件、跟踪文件或跟踪表，包含要优化的数据库的代表性工作负荷。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]开始，您可以指定计划缓存作为工作负荷。  从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，可以[将 Query Store 指定为工作负荷](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md)。 
  
 **XML 输入文件**  
 数据库引擎优化顾问可用于优化工作负载的 XML 格式文件。 XML 输入文件支持在 GUI 或 **dta** 实用工具中不可用的高级优化选项。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 数据库引擎优化顾问具有下列限制和局限性。  
  
-   它不能添加或删除唯一索引或强制 `PRIMARY KEY` 或 `UNIQUE` 约束的索引。  
  
-   它无法分析设置为单用户模式的数据库。  
  
-   如果您为优化建议指定的最大磁盘空间超过了实际可用空间，数据库引擎优化顾问将使用指定的值。 但是，当您执行建议脚本来实施它时，如果未先添加更多磁盘空间，则脚本会失败。 可以使用 **dta** 实用工具的 **-B** 选项指定最大磁盘空间，也可以通过在“高级优化选项”对话框中输入值来指定最大磁盘空间。  
  
-   为了安全起见，数据库引擎优化顾问不能优化驻留在远程服务器上的跟踪表中的工作负荷。 若要消除此限制，可以使用跟踪文件替代跟踪表，或将跟踪表复制到远程服务器。  
  
-   当强制实施约束时，例如为优化建议指定最大磁盘空间时强制的约束（通过使用 **-B** 选项或“高级优化选项”对话框），数据库引擎优化顾问可能会被迫删除某些现有的索引。 在此情况下，生成的数据库引擎优化顾问建议可能生成负的预期提高值。  
  
-   指定限制优化时间的约束时（通过使用 **dta** 实用工具的 **-A** 选项或通过选择“优化选项”选项卡上的“限制优化时间”），数据库引擎优化顾问可能超过该时间限制，以便针对到当时为止已处理的工作负荷，生成精确预期的提高值和分析报告。  
  
-   数据库引擎优化顾问可能在下列情况下不提供建议：  
  
    1.  正在优化的表所包含的数据页数少于 10。  
  
    2.  建议的索引对当前物理数据库设计的查询性能预计带来的提高值不够。  
  
    3.  运行数据库引擎优化顾问的用户不是 **db_owner** 数据库角色或 **sysadmin** 固定服务器角色的成员。 工作负荷中的查询在运行数据库引擎优化顾问的用户的安全上下文中进行分析。 该用户必须是 **db_owner** 数据库角色的成员。  
  
-   数据库引擎优化顾问将优化会话数据和其他信息存储在 **msdb** 数据库中。 如果对 **msdb** 数据库进行了更改，可能会有丢失优化会话数据的风险。 若要消除此风险，请对 **msdb** 数据库实施适当的备份策略。  
  
## <a name="performance-considerations"></a>性能注意事项  
 在分析过程中，数据库引擎优化顾问可能占用相当多的处理器及内存资源。 若要避免降低生产服务器速度，请采用下列策略之一：  
  
-   在服务器空闲时优化数据库。 数据库引擎优化顾问可能影响维护任务性能。  
  
-   使用测试服务器/生产服务器功能。 有关详细信息，请参阅  [减轻生产服务器优化负荷](../../relational-databases/performance/reduce-the-production-server-tuning-load.md)。  
  
-   指定数据库引擎优化顾问仅分析物理数据库设计结构。 数据库引擎优化顾问提供许多选项，但是请仅指定所需选项。  
  
## <a name="dependency-on-xp_msver-extended-stored-procedure"></a>与 xp_msver 扩展存储过程的依赖关系  
 数据库引擎优化顾问需要依赖 **xp_msver** 扩展存储过程才能提供全部功能。 该扩展存储过程默认是打开的。 数据库引擎优化顾问使用该扩展存储过程，提取要优化的数据库所在计算机中的处理器数以及可用内存数。 如果 **xp_msver** 不可用，则数据库引擎优化顾问将假定正在运行数据库引擎优化顾问的计算机的硬件特征。 如果无法获得运行数据库引擎优化顾问的计算机的硬件特征，则假设该计算机有一个处理器和 1024 MB 内存。  
  
 该依赖关系会影响分区建议，因为推荐的分区数取决于这两个值（处理器数和可用内存）。 如果您使用测试服务器来优化您的生产服务器，该依赖关系还会影响优化结果。 在该方案中，数据库引擎优化顾问使用 **xp_msver** 来提取生产服务器的硬件特征。 在测试服务器上的工作负荷优化之后，数据库引擎优化顾问将使用这些硬件属性来生成建议。 有关详细信息，请参阅 [xp_msver (Transact-SQL)](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md)。  
  
## <a name="database-engine-tuning-advisor-tasks"></a>数据库引擎优化顾问任务  
 下表列出了常见的数据库引擎优化顾问任务以及描述如何执行这些任务的主题。  
  
|数据库引擎优化顾问任务|主题|  
|-----------------------------------------|-----------|  
|初始化并启动数据库引擎优化顾问。<br /><br /> 通过指定计划缓存、创建脚本或生成跟踪文件或跟踪表来创建工作负荷。<br /><br /> 通过使用数据库引擎优化顾问图形用户界面工具来优化数据库。<br /><br /> 创建 XML 输入文件以优化工作负荷。<br /><br /> 查看数据库引擎优化顾问用户界面选项的描述。|[启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|查看数据库优化操作的结果。<br /><br /> 选择并实施优化建议。<br /><br /> 针对工作负荷执行“假设”探索性分析。<br /><br /> 检查现有优化会话，基于现有会话克隆会话 <br />或编辑现有优化建议，以进一步计算和实施。<br /><br /> 查看数据库引擎优化顾问用户界面选项的描述。|[查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)|  
  
  
