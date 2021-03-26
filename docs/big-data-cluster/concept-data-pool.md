---
title: 什么是数据池？
titleSuffix: SQL Server big data clusters
description: 了解 SQL Server 数据池在 SQL Server 大数据群集中的作用，以及 SQL 数据池的体系结构和功能。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd42bed16500e58f6fb1873f28ccd5abce5d30ab
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551566"
---
# <a name="what-are-data-pools-in-a-sql-server-big-data-cluster"></a>什么是 SQL Server 大数据群集中的数据池？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍 SQL Server 大数据群集中 SQL Server 数据池的作用。 以下部分介绍了数据池的体系结构、功能和应用场景。

这个 5 分钟的视频介绍了数据池，并说明了如何从数据池查询数据：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Querying-Data-from-Big-Data-Cluster-Data-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="data-pool-architecture"></a>数据池体系结构

数据池包含一个或多个 SQL Server 数据池实例，这些实例为群集提供永久的 SQL Server 存储。 它允许在外部数据源中对缓存数据执行性能查询并分担作业。 使用 T-SQL 查询或 Spark 作业将数据引入到数据池中。 为了提高大型数据集的性能，将引入的数据分布到分片中，并存储在池中的所有 SQL Server 实例中。 支持的分发方法为轮循机制并已进行复制。 为了优化读取访问，在每个数据池实例中的每个表上创建了聚集列存储索引。 数据池用作 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的横向扩展数据市场。

![横向扩展数据市场](media/concept-data-pool/data-virtualization-improvements.png)

从 SQL Server 主实例可以管理对数据池中 SQL server 实例的访问。 创建了数据池的外部数据源以及用于存储数据缓存的 PolyBase 外部表。 在后台，控制器使用与外部表匹配的表在数据池中创建一个数据库。 SQL Server 主实例中的工作流是透明的；控制器会将特定外部表请求重定向到数据池中的 SQL Server 实例，该实例可以通过计算池、执行查询并返回结果集。 数据池中的数据只能引入或查询，不能修改。 因此，任何数据刷新都需要删除表，然后再重新创建表和重新填充数据。

## <a name="data-pool-scenarios"></a>数据池应用场景

 报告是一个常见的数据池应用场景。 例如，可以将加入多个 PolyBase 数据源且用于每周报告的复杂查询放入数据池。 缓存数据提供了本地快速计算，无需返回原始数据集。 同样，需要定期刷新的面板数据可能会缓存到数据池中，以便优化报告。 机器学习重复探索还可以从数据池中的数据集缓存中获益。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/microsoft/sqlworkshops-bdc)
