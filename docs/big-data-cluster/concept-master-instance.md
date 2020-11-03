---
title: 什么是主实例？
titleSuffix: SQL Server big data clusters
description: 本文介绍 SQL Server 2019 大数据群集中的 SQL Server 主实例。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 57de001599923d46139883f2f8a691f9d682abf3
ms.sourcegitcommit: ab9ddcc16fdfc245cf9a49d1e90bb1ffe3958c38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92914306"
---
# <a name="what-is-the-master-instance-in-a-sql-server-big-data-cluster"></a>什么是 SQL Server 大数据群集中的主实例？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍 SQL Server 大数据群集中 SQL Server 主实例的作用。 主实例是在 SQL Server 大数据群集中运行的 SQL Server 实例，用于管理连接性、横向扩展查询、元数据和用户数据库以及机器学习服务。

SQL Server 主实例提供以下功能：

## <a name="connectivity"></a>连接

SQL Server 主实例为群集提供外部可访问的 TDS 终结点。 可将应用程序或 SQL Server 工具（如 Azure Data Studio 或 SQL Server Management Studio）连接到此终结点，就像使用任何其他 SQL Server 实例一样。

## <a name="scale-out-query-management"></a>横向扩展查询管理

SQL Server 主实例包含横向扩展查询引擎，可用于在[计算池](concept-compute-pool.md)中的节点上跨 SQL Server 实例分发查询。 横向扩展查询引擎还通过 Transact-SQL 提供对群集中所有 Hive 表的访问权限，而无需任何其他配置。

## <a name="metadata-and-user-databases"></a>元数据和用户数据库

除标准 SQL Server 系统数据库外，SQL 主实例还包含以下内容：

- 保存 HDFS 表元数据的元数据数据库。
- 数据平面分片映射。
- 提供群集数据平面访问权限的外部表的详细信息。
- 用户数据库中定义的 PolyBase 外部数据源和外部表。

还可选择将自己的用户数据库添加到 SQL Server 主实例。

## <a name="machine-learning-services"></a>机器学习服务

SQL Server 机器学习服务是数据库引擎的附加功能，用于在 SQL Server 中执行 Java、R 和 Python 代码。 此功能基于 SQL Server 扩展性框架，该框架将外部进程与核心引擎进程隔离，但以存储过程、包含 R 或 Python 语句的 T-SQL 脚本，或包含 T-SQL 的 Java、R 或 Python 代码的形式与关系数据完全集成。

作为 SQL Server 大数据群集的一部分，SQL Server 主实例默认提供机器学习服务。 这意味着一旦在 SQL Server 主实例上启用外部脚本执行，就可以使用 sp_execute_external_script 执行 Java、R 和 Python 脚本。

### <a name="advantages-of-machine-learning-services-in-a-big-data-cluster"></a>大数据群集中机器学习服务的优点

借助 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]，可将大数据轻松地联接到通常存储在企业数据库中的维度数据。 当大数据不仅仅由组织的各个部分掌握，而且还包含在报表、仪表板和应用程序中时，其价值会大幅增加。 与此同时，数据科学家可以继续使用 Spark/HDFS 生态系统工具，并轻松、实时地访问 SQL Server 主实例和外部数据源（可 _通过_ SQL Server 主实例访问）中的数据。

使用 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]，可以通过企业数据湖实现更多操作。 SQL Server 开发人员和分析人员可以：

* 生成使用企业数据湖中的数据的应用程序。
* 使用 Transact-SQL 查询推断所有数据。
* 使用现有 SQL Server 工具和应用程序生态系统来访问和分析企业数据。
* 通过数据虚拟化和数据市场减少数据移动需求。
* 继续将 Spark 用于大数据方案。
* 使用 Spark 或 SQL Server 生成智能企业应用程序，以通过数据湖训练模型。
* 在生产数据库中操作模型以获得最佳性能。
* 将数据直接流式传输到企业数据市场以进行实时分析。
* 使用交互式分析和 BI 工具直观地探索数据。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
