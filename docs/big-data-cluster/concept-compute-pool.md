---
title: 什么是计算池？
titleSuffix: SQL Server big data clusters
description: 本文介绍 SQL Server 2019 大数据群集中的计算池。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/15/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 26efddc55b0e43f1290be1cde2b593bed4f10640
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551578"
---
# <a name="what-are-compute-pools-in-a-sql-server-big-data-cluster"></a>什么是 SQL Server 大数据群集中的计算池？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍 SQL Server 大数据群集中 *SQL Server 计算池* 的作用。 计算池为 SQL Server 大数据群集提供横向扩展计算资源。 它们用于从 SQL Server 主实例中卸载计算工作或中间结果集。 以下部分介绍计算池的体系结构、功能和使用情况。

你还可以观看下面提供的 5 分钟视频，简要了解计算池：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Big-Data-Cluster-Compute-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="compute-pool-architecture"></a>计算池体系结构

计算池由 Kubernetes 中运行的一个或多个计算 pod 组成。 由 [SQL Server 主实例](concept-master-instance.md)来协调这些 pod 的自动创建和管理过程。 每个 pod 包含一组基本服务和一个 SQL Server 数据库引擎的实例。

![计算池体系结构](media/concept-compute-pool/compute-pool-architecture.png)

## <a name="scale-out-groups"></a>横向扩展组

计算池可充当对不同外部数据源（例如 SQL Server、Oracle、MongoDB、Teradata 和 HDFS）进行的分布式查询的 PolyBase 横向扩展组。 通过使用 Kubernetes 中的计算 Pod，SQL Server 大数据群集可自动为 PolyBase 横向扩展组创建和配置计算 Pod。

## <a name="compute-pool-scenarios"></a>计算池场景

使用计算池的场景包括：

- 当提交到主实例的查询使用[存储池](concept-storage-pool.md)中的一个或多个表时。

- 当提交到主实例的查询使用[数据池](concept-data-pool.md)中具有轮循机制分布的一个或多个表时。

- 当提交到主实例的查询使用具有 SQL Server、Oracle、MongoDB 和 Teradata 的外部数据源的已分区表时。 在此场景中，必须启用查询提示选项 (FORCE SCALEOUTEXECUTION)。

- 当提交到主实例的查询使用 [HDFS 层](hdfs-tiering.md)中的一个或多个表时。

不使用计算池的场景包括：

- 当提交到主实例的查询使用外部 Hadoop HDFS 群集中的一个或多个表时。

- 当提交到主实例的查询使用 Azure Blob 存储中的一个或多个表时。

- 当提交到主实例的查询使用具有 SQL Server、Oracle、MongoDB 和 Teradata 的外部数据源的未分区表时。

- 当已启用查询提示选项 (DISABLE SCALEOUTEXECUTION) 时。

- 当提交到主实例的查询应用于位于主实例上的数据库时。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/microsoft/sqlworkshops-bdc)
