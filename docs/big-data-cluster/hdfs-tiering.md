---
title: 配置 HDFS 分层
titleSuffix: SQL Server big data clusters
description: 本文介绍如何配置 HDFS 分层，以将外部 Azure Data Lake Storage 文件系统装载到 SQL Server 2019 大数据群集上的 HDFS 中。
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c95bb10dbd063c56029a01b5a828f80eb5ba4d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046669"
---
# <a name="configure-hdfs-tiering-on-big-data-clusters-2019"></a>在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上配置 HDFS 分层

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

借助 HDFS 分层，可以在 HDFS 中装载与 HDFS 兼容的外部文件系统。 本文介绍如何为 SQL Server 大数据群集配置 HDFS 分层。 目前，我们支持连接到 Azure Data Lake Storage Gen2 和 Amazon S3。 

## <a name="hdfs-tiering-overview"></a>HDFS 分层概述

借助分层，应用程序可以无缝访问各种外部存储中的数据，就像数据驻留在本地 HDFS 一样。 装载是一种元数据操作，该操作会将描述外部文件系统上的命名空间的元数据复制到本地 HDFS。 此元数据包括有关外部目录和文件及其权限和 ACL 的信息。 当通过查询或其他方式访问数据本身时，仅按需复制相应的数据。 现在可以从 SQL Server 大数据群集访问外部文件系统数据。 你可以对此数据运行 Spark 作业和 SQL 查询，就像对群集上的 HDFS 中存储的任何本地数据运行它们一样。

此 7 分钟视频概述了 HDFS 分层：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Unify-your-data-lakes-with-HDFS-tiering/player?WT.mc_id=dataexposed-c9-niner]


### <a name="caching"></a>Caching
现在默认保留 HDFS 存储总量的 1% 来缓存装载的数据。 缓存是跨装载的全局设置。

> [!NOTE]
> HDFS 分层是由 Microsoft 开发的一项功能，其早期版本已作为 Apache Hadoop 3.1 分发版的一部分发布。 有关详细信息，请参阅 [https://issues.apache.org/jira/browse/HDFS-9806](https://issues.apache.org/jira/browse/HDFS-9806) 获取详细信息。

以下部分展示了如何使用 Azure Data Lake Storage Gen2 数据源配置 HDFS 分层的示例。

## <a name="refresh"></a>刷新

HDFS 分层支持刷新。 刷新现有装载可获取远程数据的最新快照。

## <a name="prerequisites"></a>先决条件

- [部署大数据群集](deployment-guidance.md)
- [大数据工具](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="mounting-instructions"></a>装载说明

我们支持连接到 Azure Data Lake Storage Gen2 和 Amazon S3。 有关如何针对这些存储类型进行装载的说明，请参阅以下文章：

- [如何在大数据群集中装载 ADLS Gen2 以实现 HDFS 分层](hdfs-tiering-mount-adlsgen2.md)
- [如何在大数据群集中装载 S3 以实现 HDFS 分层](hdfs-tiering-mount-s3.md)

## <a name="known-issues-and-limitations"></a><a id="issues"></a> 已知问题和限制

以下列表提供了在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 中使用 HDFS 分层时的已知问题和当前限制：

- 如果装载长时间处于 `CREATING` 状态，则很可能已失败。 在这种情况下，请取消命令并在必要时删除装载。 在重试之前，请验证参数和凭据是否正确。

- 无法在现有目录上创建装载。

- 无法在现有装载中创建装载。

- 如果装入点不存在任何上级，则将使用默认为 r-xr-xr-x (555) 的权限进行创建。

- 装载创建可能需要一些时间，具体取决于要装载的文件的数量和大小。 在此过程中，用户看不到装载下的文件。 创建装载时，所有文件都将添加到临时路径（默认为 `/_temporary/_mounts/<mount-location>`）中。

- 装载创建命令是异步的。 运行该命令后，可以检查装载状态以了解装载的状态。

- 创建装载时，用于 **--mount-path** 的参数本质上是装载的唯一标识符。 必须在后续命令中使用相同的字符串，包括末尾的“/”（如果存在）。

- 装载是只读的。 无法在装载下创建任何目录或文件。

- 我们不建议装载可以更改的目录和文件。 创建装载后，对远程位置的任何更改或更新都不会反映在 HDFS 的装载中。 如果远程位置发生了更改，可以选择删除并重新创建装载，以反映已更新的状态。

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
