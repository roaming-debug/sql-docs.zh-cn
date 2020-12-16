---
title: 计划并测试数据库引擎升级计划 | Microsoft Docs
description: 本文介绍在开始 SQL Server 升级之前进行的规划，包括计划清单以及开发和测试升级计划。
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 19c5b725-7400-4881-af8f-fd232ca28234
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 5e98874d2e5d1673a4f666de81b82e92b20bfa9f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463618"
---
# <a name="plan-and-test-the-database-engine-upgrade-plan"></a>计划并测试数据库引擎升级计划

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
 若要成功执行 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 升级，无论使用何种方法，都需要进行相应规划。  
  
## <a name="release-notes-and-known-upgrade-issues"></a>发行说明和已知升级问题  
 升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 之前，请先查看：

- [SQL Server 2017 发行说明](../../sql-server/sql-server-2017-release-notes.md) 
- [SQL Server 2016 发行说明](../../sql-server/sql-server-2016-release-notes.md) 
- [SQL Server 数据库引擎的后向兼容性](../discontinued-database-engine-functionality-in-sql-server.md)文章。  
  
## <a name="pre-upgrade-planning-checklist"></a>升级前规划清单  
 升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]之前，请先查看以下清单和相关文章。 这些文章适用于所有升级（无论使用何种升级方法），有助于确定最合适的升级方法：滚动升级、新安装升级或就地升级。 例如，如果从 SQL Server 2005 或从 SQL Server 的 32 位版本升级操作系统，可能无法执行就地升级或滚动升级。 有关决策树的信息，请参阅 [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)。  
  
-   硬件和软件要求：查看安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的硬件和软件要求。 可在以下位置找到这些要求：[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。 任何升级计划周期都应考虑升级硬件（较新硬件速度更快，并且由于减少了处理器或合并了数据库和服务器，可以减少授权）和升级操作系统。 这些硬件和软件更改的类型将影响你选择的升级方法类型。  
  
-   当前环境：研究你当前的环境，了解所用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件和连接到你的环境的客户端。  
  
    -   客户端提供程序：尽管升级不需要更新每个客户端的提供程序，但是你可以选择这样做。 如果计划升级 [!INCLUDE[sql14](../../includes/sssql14-md.md)] 或较旧版本，以下 [!INCLUDE[sql15](../../includes/sssql15-md.md)] 功能将要求为每个客户端提供更新的提供程序，或者更新的提供程序将提供其他功能：  
  
       -   [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
  
       -   [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
       -   [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
       -   TLS 安全更新程序  

   >[!NOTE]
   >前面的列表也适用于 [!INCLUDE[sscurrent](../../includes/sscurrent-md.md)]。
  
-   第三方组件：确定第三方组件（如集成备份）的兼容性。  
  
-   目标环境：验证目标环境是否符合硬件和软件要求，以及是否能够支持原始系统的要求。 例如，升级可能涉及将多个 SQL Server 实例合并为单个新 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例，或将你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境虚拟化为一个私有或公有云。  
  
-   版本：确定升级的 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 适当版本，并确定升级的有效升级路径。 有关详细信息，请参阅 [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。 在从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的某一版本升级到另一版本之前，请验证要升级到的版本是否支持当前使用的功能。  
  
    > [!NOTE]  
    >  在从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise 版的之前版本升级到 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 时，请在“Enterprise Edition：基于内核授予许可”和 Enterprise Edition 之间进行选择。 这些 Enterprise 版本仅在许可模式方面存在不同。 有关详细信息，请参阅 [Compute Capacity Limits by Edition of SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。  
  
-   后向兼容性：查看 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 数据库引擎后向兼容性文章，了解 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 和要从其开始升级的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本之间的行为变化。 请参阅 [SQL Server Database Engine Backward Compatibility](../discontinued-database-engine-functionality-in-sql-server.md)。  
  
-   数据迁移助手：运行数据迁移助手，以帮助诊断可能会阻止升级过程或由于重大更改而需要修改现有脚本或应用程序的问题。
    可在[此处](https://aka.ms/get-dma)下载数据迁移助手。  
  
-   系统配置检查器：运行 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 系统配置检查器 (SCC) 以确定 SQL Server 安装程序是否在你计划升级以前检测到任何阻塞问题。 有关详细信息，请参阅 [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)。  
  
-   升级内存优化表：将包含内存优化表的 SQL Server 2014 数据库实例升级到 SQL Server 2016 时，升级进程将需要更多时间以将内存优化表转换为新的磁盘上格式（进行这些步骤时，数据库将处于脱机状态）。   时间量取决于内存优化表的大小和 I/O 子系统的速度。 针对就地和新安装升级，升级需要三个数据操作（滚动升级不需要进行步骤 1，但需要进行步骤 2 和步骤 3）：  
  
    1.  使用旧的磁盘上格式运行数据库恢复（包含将内存优化表中的所有数据从磁盘上加载到内存中）  
  
    2.  以新的磁盘上格式将数据序列化到磁盘  
  
    3.  使用新格式运行数据库恢复（包含将内存优化表中的所有数据从磁盘上加载到内存中）  
  
     此外，在此过程中磁盘空间不足将导致恢复失败。 确保磁盘上有足够的空间来存储现有数据库以及与数据库中 MEMORY_OPTIMIZED_DATA 文件组中容器的当前大小相等的其他存储，以执行就地升级或将 SQL Server 2014 数据库附加到 SQL Server 2016 实例。 使用以下查询来确定 MEMORY_OPTIMIZED_DATA 文件组当前所需的磁盘空间，以及升级成功所需的可用磁盘空间量：  
  
    ```  
    select cast(sum(size) as float)*8/1024/1024 'size in GB'   
    from sys.database_files  
    where data_space_id in (select data_space_id from sys.filegroups where type=N'FX')  
    ```  
  
## <a name="develop-and-test-the-upgrade-plan"></a>开发和测试升级计划  
 最好的方法是像对待任何 IT 项目一样对待升级。 组织一个升级团队，该团队具有数据库管理、网络、提取、转换和加载 (ETL) 以及升级所需的其他技能。 团队需要进行以下工作：  
  
-   选择升级方法：请参阅[选择数据库引擎升级方法](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)。  
  
-   制定回滚计划：如果需要回滚，执行此计划可以还原原始环境。  
  
-   确定验收条件：将用户切换到已升级环境之前，验证升级是否成功成。  
  
-   测试升级计划：要使用实际工作负荷测试性能，请使用 Microsoft SQL Server Distributed Replay 实用工具。 此实用工具可以使用多台计算机模拟任务关键型工作负荷，从而重播跟踪数据。 通过在 SQL Server 升级前和升级后在测试服务器上执行重播，可以测量性能差异，并查找你的应用程序可能与升级版本不兼容的问题。 有关详细信息，请参阅 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md) 和[管理工具命令行选项（Distributed Replay 实用工具）](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)。  
  
## <a name="next-steps"></a>后续步骤  
[升级数据库引擎](../../database-engine/install-windows/upgrade-database-engine.md) 
  
## <a name="additional-resources"></a>其他资源 
[数据库迁移指南](https://aka.ms/datamigration)