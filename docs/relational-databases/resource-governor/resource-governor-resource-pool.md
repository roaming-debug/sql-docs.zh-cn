---
title: 资源调控器资源池
description: SQL Server 资源调控器指定针对传入应用程序请求可在资源池内使用的 CPU、物理 IO 和内存的使用量的限制。
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool
- resource pool [SQL Server], overview
- resource pool [SQL Server]
ms.assetid: 306b6278-e54f-42e6-b746-95a9315e0cbe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4028f19dc9d7ac319b083b5e6077021da59e7a9b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172309"
---
# <a name="resource-governor-resource-pool"></a>资源调控器资源池
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源调控器中，资源池表示 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的部分物理资源。 通过资源调控器，您可以指定针对传入应用程序请求可在资源池内使用的 CPU、物理 IO 和内存的使用量的限制。 每个资源池均可包含一个或多个工作负荷组。 在某个会话启动时，资源调控器分类器会将此会话分配给一个特定的工作负荷组，并且此会话必须使用分配给该工作负荷组的资源运行。  
  
## <a name="resource-pool-concepts"></a>资源池概念  
 资源池或池表示服务器的物理资源。 您可以将池看作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例内部的一个虚拟 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 池有两个部分。 一部分不与其他池重叠，这使得资源预留最少。 另一部分与其他池共享，支持最大可能的资源消耗。 通过为每个资源（CPU、内存和物理 IO）指定以下一个或多个设置定义池资源：  
  
-   **MIN_CPU_PERCENT 和 MAX_CPU_PERCENT**  
  
     这些设置是在存在 CPU 争用时针对资源池中所有请求的最小和最大有保障的平均 CPU 带宽。 您可以使用这些设置为多个工作负荷建立基于各工作负荷需要的可预测 CPU 资源使用量。 例如，假定公司中的销售和营销部门共享同一个数据库。 销售部门具有大量占用 CPU 的含高优先级查询的工作负荷。 营销部门也具有大量占用 CPU 的工作负荷，但具有较低优先级的查询。 通过为每个部门创建单独的资源池，您可以为“销售”资源池分配 70% 的 *最小* CPU 百分比，为“营销”资源池分配 30% 的 *最大* CPU 百分比。 这确保销售工作负荷收到其要求的 CPU 资源，并且营销工作负荷与销售工作负荷对 CPU 的需求保持相互独立。 请注意，该最大 CPU 百分比是一个机会最大值。 如果有可用 CPU 容量，该工作负荷将占用这些可用 CPU 容量直至 100%。 该最大值仅适用于存在对 CPU 资源的争用的情况。 在此示例中，如果销售工作负荷被禁用，则营销工作负荷可以占用 100% 的 CPU（如果需要）。  
  
-   **CAP_CPU_PERCENT**  
  
     CAP_CPU_PERCENT 设置对资源池中所有请求的 CPU 带宽有硬性上限。 与资源池关联的工作负荷可以使用高于 MAX_CPU_PERCENT 值的 CPU 容量（如果可用），但不能超过 CAP_CPU_PERCENT 的值。 使用上面的示例，假定正在针对营销部门的资源使用量对其收费。 他们希望可预测的计费并且不希望支付超过 30% 的 CPU。 这可以通过为“营销”资源池将 CAP_CPU_PERCENT 设置为 30 来实现。  
  
-   **MIN_MEMORY_PERCENT 和 MAX_MEMORY_PERCENT**  
  
     这些设置是为此资源池保留的、不能与其他资源池共享的最小和最大内存量。 对于没有内存优化表的数据库，此处引用的内存是授予查询执行的内存，而非缓冲池内存（例如数据和索引页）。 为某一资源池设置最小内存值意味着您在确保指定的内存百分比将可用于可在此资源池中运行的任何请求。 与 MIN_CPU_PERCENT 相比，这是个重要的不同之处，因为在此情况下，内存可能保留在给定的资源池中，即使该资源池在属于该池的工作负荷组中没有任何请求。 因此，在使用此设置时务必要非常小心，因为此内存将不可用于任何其他池，即使没有任何活动请求。 为某一资源池设置最大内存值意味着在该资源池中正在运行请求时，它们将永远不会获得超过此总体内存百分比的内存。

     为了使用 Resource Governor 来管理内存优化表的内存，应将数据库绑定到单独的资源池。 有关详细信息，请参阅[将具有内存优化表的数据库绑定至资源池](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)。
  
-   **AFFINITY**  
  
     通过此设置，您可以将某一资源池与一个或多个计划程序或 NUMA 节点相关联，以便实现更好的对 CPU 资源的隔离。 使用上面的销售和营销方案，假定销售部门需要更好隔离的环境，并且希望在所有时候都占用 100% 的 CPU 内核。 通过使用 AFFINITY 选项，可在不同的 CPU 上计划销售和营销工作负荷。 假定针对“营销”池的 CAP_CPU_PERCENT 仍存在，并且营销工作负荷继续占用一个内核的最高 30%，而销售工作负荷占用其他内核的 100%。 就销售和营销工作负荷而言，它们运行在两个独立的计算机上。  
  
-   **MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME**  
  
     这些设置是针对某一资源池的每个磁盘卷每秒的最小和最大物理 IO 操作数 (IOPS)。 您可以使用这些设置控制针对某一给定资源池的用户线程而发出的物理 IO 数。 例如，销售部门在较大批次中生成若干月末报表。 这些批次中的查询可生成可导致磁盘卷饱和并且影响数据库中其他更高优先级工作负荷性能的 IO。 若要隔离此工作负荷，对于销售部门资源池，将 MIN_IOPS_PER_VOLUME 设置为 20，并将 MAX_IOPS_PER_VOLUME 设置为 100，以此控制可发出用于工作负荷的 IO 级别。  
  
配置 CPU 或内存时，所有池的 MIN 值之和不能超过服务器资源的 100%。 此外，配置 CPU 或内存时，MAX 值和 CAP 值可以设置为 MIN 和 100% 之间（包括 100%）的任何值。  
  
如果某个池定义了非零 MIN，将重新调整其他池的有效 MAX 值。 从 100% 中减去为某个池配置的 MAX 值的最小值以及其他池的 MIN 值之和。  
  
下表说明了几个上述概念。 该表显示了内部池、默认池和两个用户定义池的设置。 
  
|池名称|MIN 百分比设置|MAX 百分比设置|计算的有效 MAX 百分比|计算的共享百分比|注释|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|内部|0|100|100|0|有效 MAX 百分比和共享百分比不适用于内部池。|  
|default|0|100|30|30|有效 MAX 值的计算如下：min(100,100-(20+50)) = 30。 计算的共享百分比为有效 MAX - MIN = 30。|  
|池 1|20|100|50|30|有效 MAX 值的计算方式为：min(100,100-50) = 50。 计算的共享百分比为有效 MAX - MIN = 30。|  
|池 2|50|70|70|20|有效 MAX 值的计算方式为：min(70,100-20) = 70。 共享百分比的计算方式为：有效 MAX - MIN = 20。|  

下面的公式用于计算上表中有效 MAX 百分比和共享百分比：  
  
-   Min(X,Y) 表示 X 和 Y 中的较小值。  
  
-   Sum(X) 表示所有池的 X 值之和。  
  
-   总计共享百分比 = 100 - sum(MIN %)。  
  
-   有效 MAX 百分比 = min(X,Y)。  
  
-   共享百分比 = 有效 MAX 百分比 - MIN 百分比。  

以上表为示例，我们可以进一步说明创建其他池时进行的调整。 此池为池 3，MIN 百分比设置为 5。  
  
|池名称|MIN 百分比设置|MAX 百分比设置|计算的有效 MAX 百分比|计算的共享百分比|注释|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|内部|0|100|100|0|有效 MAX 百分比和共享百分比不适用于内部池。|  
|default|0|100|25|25|有效 MAX 值的计算如下：min(100,100-(20+50+5)) = 25。 计算的共享百分比为有效 MAX - MIN = 25。|  
|池 1|20|100|45|25|有效 MAX 值的计算方式为：min(100,100-55) = 45。 计算的共享百分比为有效 MAX - MIN = 25。|  
|池 2|50|70|70|20|有效 MAX 值的计算方式为：min(70,100-25) = 70。 共享百分比的计算方式为：有效 MAX - MIN = 20。|  
|池 3|5|100|30|25|有效 MAX 值的计算如下：min(100,100-70) = 30。 计算的共享百分比为有效 MAX - MIN = 25。|  
  
 池的共享部分用于指示在有可用资源时，可用资源的流向。 但是，在消耗资源时，资源流向指定的池，不能共享。 这在给定池中没有请求并且为该池指定的资源可以释放到其他池时，能够提高资源使用率。  
  
 池配置的某些极特殊情况是：  
  
-   所有池定义的最小值的总计表示 100% 的服务器资源。 在这种情况下，有效最大值等于最小值。 这等于将服务器资源划分为无重叠部分，而不考虑任何指定池内的资源消耗情况。  
  
-   所有池的最小值为零。 所有池竞争使用可用资源，其最终大小基于各个池消耗的资源。 其他因素（如策略）也对调整最终池大小起作用。  
  
资源调控器预定义两个资源池：内部池和默认池。 你可以添加其他池。  
  
**内部池**  
  
内部池表示由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自身消耗的资源。 这个池始终只包含内部组，在任何情况下都不允许更改池。 不限制内部池的资源消耗。 池中的所有工作负荷均视为服务器函数的关键内容，资源调控器允许内部池在与其他池的限制发生冲突时争用资源。  
  
> [!NOTE]  
>  不从总体资源使用情况中减去内部池和内部组资源使用情况。 使用可用的总体资源计算百分比。  
  
**默认池**  
  
默认池是第一个预定义的用户池。 在进行任何配置之前，默认池只包含默认组。 不能创建或删除默认池，但可以更改。 默认池除了包含默认组，还可以包含用户定义的组。 对于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本，例程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 操作有默认资源池，外部进程（如执行 R 脚本）有默认外部资源池。  
  
> [!NOTE]  
>  可以更改默认组，但不能从默认池中移出。  
  
**外部池**  
  
用户可以通过定义外部池来定义外部进程的资源。 对于 R 服务，这尤其可调控 `rterm.exe`、`BxlServer.exe` 及它们生成的其他进程。  
  
**用户定义的资源池**  
  
用户定义的资源池是在您的环境中为特定的工作负荷创建的资源池。 资源调控器提供用于创建、更改和删除资源池的 DDL 语句。  
  
## <a name="resource-pool-tasks"></a>资源池任务  
  
|任务说明|项目|  
|----------------------|-----------|  
|说明如何创建一个资源池。|[创建资源池](../../relational-databases/resource-governor/create-a-resource-pool.md)|  
|说明如何更改资源池设置。|[更改资源池设置](../../relational-databases/resource-governor/change-resource-pool-settings.md)|  
|说明如何删除资源池。|[删除资源池](../../relational-databases/resource-governor/delete-a-resource-pool.md)|  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [资源调控器分类器函数](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [使用模板配置资源调控器](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [查看资源调控器属性](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
