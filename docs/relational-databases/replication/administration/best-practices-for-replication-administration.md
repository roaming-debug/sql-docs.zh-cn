---
title: 复制管理最佳做法 | Microsoft Docs
description: 配置复制后，请按照以下最佳做法来管理 SQL Server 中的复制拓扑。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- administering replication, best practices
- replication [SQL Server], administering
ms.assetid: 850e8a87-b34c-4934-afb5-a1104f118ba8
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 17f96bc2ee6c1e8c4d55491424b0099c467ed8de
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461878"
---
# <a name="best-practices-for-replication-administration"></a>复制管理最佳实践
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在配置复制后，了解如何管理复制拓扑十分重要。 本主题介绍了许多方面的基本最佳实践指导原则，还提供了有关每一方面的详细信息的链接。 除遵循本主题中介绍的最佳实践指导原则之外，还请仔细阅读常见问题解答主题，以了解常见的问题：[复制管理常见问题解答](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)。  
  
 将最佳实践指导原则分成两个部分很有用：  
  
-   下列信息介绍应为所有复制拓扑实现的最佳实践：  
  
    -   开发并测试备份和还原策略。  
  
    -   编写复制拓扑脚本。  
  
    -   创建阈值和警报。  
  
    -   监视复制拓扑。  
  
    -   如果有必要，确定性能基准并优化复制。  
  
-   下列信息介绍应该考虑，但可能不是您的拓扑所必需的最佳实践：  
  
    -   定期验证数据。  
  
    -   通过配置文件调整代理参数。  
  
    -   调整发布和分发保持期。  
  
    -   如果应用程序要求发生更改，了解如何更改项目和发布属性。  
  
    -   如果应用程序要求发生更改，了解如何进行架构更改。  
  
## <a name="develop-and-test-a-backup-and-restore-strategy"></a>开发并测试备份和还原策略  
 所有数据库都应定期备份，还应对还原这些备份的能力定期进行测试；复制的数据库没有差异。 下列数据库应定期备份：  
  
-   发布数据库  
  
-   分发数据库  
  
-   订阅数据库  
  
-   发布服务器、分发服务器和所有订阅服务器上的 **msdb** 数据库和 **master** 数据库  
  
 对于复制的数据库，需要特别注意与备份和还原数据有关的信息。 有关详细信息，请参阅 [备份和还原复制的数据库](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)。  
  
## <a name="script-the-replication-topology"></a>编写复制拓扑脚本  
 制订灾难恢复计划时，应要求对拓扑中的所有复制组件编写脚本，另外，脚本还可以用来自动处理重复性的任务。 脚本包含实现脚本化复制组件（如发布或订阅）所需的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 系统存储过程。 创建完组件后，可以在向导（如新建发布向导）或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中创建脚本。 您可以使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 **sqlcmd** 查看、修改和运行脚本。 脚本可以与备份文件存储在一起，以便在必须重新配置复制拓扑时使用。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
 如果进行了任何属性更改，则应重新编写组件脚本。 如果对事务复制使用自定义存储过程，则应与脚本一起存储每个过程的副本。如果过程发生更改，应更新相应的副本（通常会由于架构更改或应用程序要求的更改而更新过程）。 有关自定义过程的详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
## <a name="establish-performance-baselines-and-tune-replication-if-necessary"></a>如果有必要，确定性能基准并优化复制  
 配置复制前，建议先熟悉一下影响复制性能的因素：  
  
-   服务器和网络硬件  
  
-   数据库设计  
  
-   分发服务器配置  
  
-   发布设计和选项  
  
-   筛选器设计和用法  
  
-   订阅选项  
  
-   快照选项  
  
-   代理参数  
  
-   维护  
  
 配置复制后，建议制定一个性能基准，这使您可以确定复制在应用程序和拓扑的典型工作量下的行为。 使用复制监视器和系统监视器确定复制性能的以下五个维度的典型数目：  
  
-   滞后时间：在复制拓扑中的节点之间传播数据更改所用的时间。  
  
-   吞吐量：一段时间内系统可以持续的复制活动量（以一段时间内传递的命令来度量）。  
  
-   并发：可以在系统上同时运行的复制进程数。  
  
-   同步持续时间：完成给定同步所用的时间。  
  
-   资源占用：复制处理结束时所用的硬件和网络资源。  
  
 滞后时间和吞吐量与事务复制关系最密切，因为建立在事务复制基础上的系统通常需要低滞后时间和大吞吐量。 并发和同步持续时间与合并复制关系最密切，因为建立在合并复制基础上的系统通常具有大量的订阅服务器，并且发布服务器可能具有大量与这些订阅服务器的并发同步。  
  
 确定基准数目后，请在复制监视器中设置阈值。 有关详细信息，请参阅[在复制监视器中设置阈值和警告](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)和[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。 如果您遇到性能问题，建议您通读上面列出的增强性能主题中的建议，并在影响所遇到问题的区域中应用更改。  
  
## <a name="create-thresholds-and-alerts"></a>创建阈值和警报  
 复制监视器允许您设置大量与状态和性能相关的阈值。 建议为拓扑设置相应的阈值；如果达到阈值，将显示警告，而且（可选）可以向电子邮件帐户、寻呼机或其他设备发送警报。 有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。  
  
 除了可与监视阈值关联的警报外，复制还提供大量响应复制代理操作的预定义警报。 管理员可以使用这些警报随时了解复制拓扑的状态。 建议通读描述警报的主题，并使用适合您的管理需要的警报（必要时还可以创建其他警报）。 有关详细信息，请参阅[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。  
  
## <a name="monitor-the-replication-topology"></a>监视复制拓扑  
 建立复制拓扑并配置阈值和警报后，建议定期监视复制。 监视复制拓扑是部署复制的一个重要方面。 因为复制活动是分布式的，所以跟踪复制过程中涉及的所有计算机的活动和状态非常有必要。 以下工具可用于监视复制：  
  
-   复制监视器是监视复制的最重要的工具，它允许您监视复制拓扑的总体运行状况。 有关详细信息，请参阅 [Monitoring Replication](../../../relational-databases/replication/monitor/monitoring-replication.md)。  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] 和复制管理对象 (RMO) 提供了监视复制的接口。 有关详细信息，请参阅 [Monitoring Replication](../../../relational-databases/replication/monitor/monitoring-replication.md)。  
  
-   系统监视器也可用于监视复制性能。 有关详细信息，请参阅 [Monitoring Replication with System Monitor](../../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md)。  
  
## <a name="validate-data-periodically"></a>定期验证数据  
 复制不要求验证，但建议对事务复制和合并复制定期运行验证。 验证允许您验证订阅服务器上的数据与发布服务器上的数据是否匹配。 验证成功表示当时发布服务器上的所有更改已复制到订阅服务器上（如果订阅服务器支持更新，则订阅服务器上的所有更改也复制到发布服务器上）并且两个数据库是同步的。  
  
 建议根据发布数据库的备份计划执行验证。 例如，如果发布数据库有一个每周执行一次的完整备份，则可以在每周完成备份后运行一次验证。 有关详细信息，请参阅[验证已复制的数据](../../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
## <a name="use-agent-profiles-to-change-agent-parameters-if-necessary"></a>如果有必要，使用代理配置文件更改代理参数  
 代理配置文件为设置复制代理参数提供了一个便利的方法。 也可以在代理命令行上指定参数，但通常更适合使用预定义的代理配置文件或创建新的配置文件（如果需要更改参数值）。 例如，如果使用合并复制并且订阅服务器从宽带连接转为拨号连接，这时可考虑使用合并代理的“慢速链接”  配置文件，此配置文件使用一组更适合慢速通信链接的参数。 有关详细信息，请参阅 [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
## <a name="adjust-publication-and-distribution-retention-periods-if-necessary"></a>如果有必要，调整发布和分发保持期  
 事务复制和合并复制分别使用保持期确定事务在分发数据库中的存储时间以及订阅必须同步的频率。 建议开始时使用默认设置，但要监视拓扑以确定是否需要调整默认设置。 例如，在合并复制中，发布保持期（默认为 14 天）决定元数据在系统表中的存储时间。 如果订阅总是在五天内同步，请考虑将该设置调整为较小的数字，这样可以减少元数据，还可能提供更好的性能。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
## <a name="understand-how-to-modify-publications-if-application-requirements-change"></a>如果应用程序的要求发生更改，了解如何修改发布  
 在创建发布后，可能需要添加或删除项目或者更改发布和项目属性。 创建发布后，多数更改是允许的，但在某些情况下还需要生成发布的新快照，并且/或者重新初始化对发布的订阅。 有关详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)和[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
## <a name="understand-how-to-make-schema-changes-if-application-requirements-change"></a>如果应用程序的要求发生更改，了解如何进行架构更改  
 在很多情况下，将应用程序投入生产后都需要进行架构更改。 在复制拓扑中，这些更改通常必须传播到所有的订阅服务器。 复制支持对已发布对象进行多种架构更改。 对 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发布服务器中相应的发布对象进行下列任何一种架构更改时，默认会将该更改传播到所有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器：  
  
-   ALTER TABLE  
  
-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
 有关详细信息，请参阅[对发布数据库进行架构更改](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
## <a name="see-also"></a>另请参阅  
 [复制管理常见问题解答](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
  
