---
title: PolyBase 横向扩展组 | Microsoft Docs
description: 使用 PolyBase 组功能创建 SQL Server 实例群集。 这提高了来自外部源的大型数据集的查询性能。
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- sql13.swb.polybasescaleoutcluster.page.f1
helpviewer_keywords:
- PolyBase
- PolyBase, scale-out groups
- scale-out PolyBase
ms.assetid: c7810135-4d63-4161-93ab-0e75e9d10ab5
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016 || =sqlallproducts-allversions'
ms.openlocfilehash: 3ab1841c009094831ae114d701d526a1d3036dba
ms.sourcegitcommit: 773c1203e3c4617606cecb2626f6b2f2c855a53d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96535271"
---
# <a name="polybase-scale-out-groups"></a>PolyBase 横向扩展组

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

在处理 Hadoop 或 Azure Blob 存储中的大型数据集时，具有 PolyBase 的独立 SQL Server 实例可能成为性能瓶颈。 PolyBase 组功能允许你创建 SQL Server 实例的群集来处理来自外部数据源的大型数据集（如 Hadoop 或 Azure Blob 存储），从而通过一种扩展的方式提高查询性能。 现在可以缩放 SQL Server 计算以满足工作负载的性能需求。 PolyBase 横向扩展组，即 SQL Server 实例组，使你能够处理并行处理体系结构中的大型外部数据集。 向组添加更多 SQL Server 实例可线性提高数据加载和查询性能。 
  
请参阅 [PolyBase 入门](./polybase-guide.md) 和 [PolyBase 指南](../../relational-databases/polybase/polybase-guide.md)。
  
![显示 PolyBase 横向扩展组的示意图。](../../relational-databases/polybase/media/polybase-scale-out-groups.png "PolyBase 横向扩展组")  
  
## <a name="head-node"></a>头节点  

头节点包含 PolyBase 查询提交到的 SQL Server 实例。 每个 PolyBase 组只能有一个头节点。 头节点是 SQL Server 实例上 SQL Server 数据库引擎、PolyBase 引擎和 PolyBase 数据移动服务的逻辑组。 使用 SQL Server 2017 和 SQL Server 2016 时，头节点必须是企业版。 从 SQL Server 2019 开始，PolyBase 头节点可以是企业版或标准版。
  
## <a name="compute-node"></a>计算节点

计算节点包含协助扩展查询处理外部数据的 SQL Server 实例。 计算节点是 SQL Server 和 SQL Server 实例上的 PolyBase 数据移动服务的逻辑组。 PolyBase 组可以有多个计算节点。 头节点和计算节点必须都运行相同版本的 SQL Server。 SQL Server 2016 初始版本允许计算节点为企业版或标准版。 从 SQL Server 2016 SP1 开始，SQL Server 的所有版本都可以作为计算节点。

## <a name="scale-out-reads"></a>横向扩展读取

当查询外部 SQL Server、Oracle 或 Teradata 实例，分区表将受益于横向扩展读取。 PolyBase 横向扩展组中的每个节点可启动最多 8 个读取器来读取外部数据。 并且每个读取器被分配到一个分区，以在外部表中读取。 

例如，假设你有包含 12 个每月分区和一个 3 节点 PolyBase 横向扩展组的外部 SQL Server 表，每个节点将使用 4 个 PolyBase 读取器来处理这 12 个分区中的每一个。 如下图所示。 

> [!NOTE]
>  这不同于通过 Hadoop 的横向扩展读取。 

![PolyBase 横向扩展读取](../../relational-databases/polybase/media/polybase-scale-out-groups2.png "PolyBase 横向扩展组")
  
## <a name="distributed-query-processing"></a>分布式查询处理  

PolyBase 查询会提交到头节点上的 SQL Server。 查询中引用外部表的那部分会移交给 PolyBase 引擎。
  
PolyBase 引擎是 PolyBase 查询背后的关键组件。 它对外部数据查询进行分析、生成查询计划并将工作分发到计算节点上的数据移动服务以便执行。 完成工作后，它将接收来自计算节点的结果，并将其提交给 SQL Server 进行处理并返回到客户端。
  
PolyBase 数据移动服务接收来自 PolyBase 引擎的指令，并在 HDFS 和 SQL Server 之间以及头节点和计算节点上的 SQL Server 实例之间传输数据。
  
## <a name="next-steps"></a>后续步骤

若要配置 PolyBase 横向扩展组，请参阅以下指南：

[改进 Windows 上的 PolyBase 横向扩展组](configure-scale-out-groups-windows.md)

## <a name="see-also"></a>另请参阅

 [sys-dm-exec-compute-nodes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)   
 [sys-dm-exec-compute-node-status](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)   
 [sys.dm_exec_compute_node_errors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
