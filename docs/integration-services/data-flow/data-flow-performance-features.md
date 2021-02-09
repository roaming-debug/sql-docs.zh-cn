---
description: 数据流性能特点
title: 数据流性能特点 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- Integration Services packages, performance
- performance [Integration Services]
- data flow [Integration Services], troubleshooting
- SQL Server Integration Services packages, performance
- loading data
- control flow [Integration Services], troubleshooting
- SSIS packages, performance
- packages [Integration Services], performance
- queries [Integration Services], troubleshooting
- sorting data [Integration Services]
- aggregations [Integration Services]
ms.assetid: c4bbefa6-172b-4547-99a1-a0b38e3e2b05
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 600ab1bce27f4fa53c2e25ae7562a870f2ccf6a6
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536348"
---
# <a name="data-flow-performance-features"></a>数据流性能特点

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本主题针对如何设计 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包提供建议，以避免出现常见性能问题。 本主题还提供有关可以用于对包的性能进行故障排除的功能和工具的信息。  
  
## <a name="configuring-the-data-flow"></a>配置数据流  
 若要配置数据流任务以获取更好的性能，可以配置任务的属性，调整缓冲区大小，以及将包配置为可以并行执行。  
  
### <a name="configure-the-properties-of-the-data-flow-task"></a>配置数据流任务的属性  
  
> [!NOTE]  
>  必须为包中的每个数据流任务单独设置本节所讨论的属性。  
  
 您可以配置数据流任务的下列属性，这些属性都会对性能产生影响：  
  
-   为缓冲区数据（BufferTempStoragePath 属性）和包含二进制大型对象 (BLOB) 数据的列（BLOBTempStoragePath 属性）指定临时存储位置。 默认情况下，这些属性包含 TEMP 和 TMP 环境变量的值。 您可能希望指定不同或更快的硬盘驱动器上的其他文件夹来存放临时文件，或将它们分布在多个驱动器上。 可以指定多个目录，并用分号来分隔这些目录名。  
  
-   通过设置 DefaultBufferSize 属性定义任务使用缓冲区的默认大小，并通过设置 DefaultBufferMaxRows 属性来定义每个缓冲区中最大的行数。 设置 AutoAdjustBufferSize 属性可指明是否根据 DefaultBufferMaxRows 属性的值自动计算缓冲区的默认大小。 默认缓冲区大小为 10 MB，最大缓冲区大小为 2^31-1 字节。 默认最大行数为 10,000。  
  
-   通过设置 EngineThreads 属性来设置任务在执行过程中可使用的线程数。 此属性为数据流引擎提供有关使用线程数的建议。 默认值为 10，最小值为 3。 但是，不论此属性值为多少，引擎都不会使用超过其所需的线程数。 如果需要避免并发问题，引擎所使用的线程数也可能会超过此属性指定的线程数。  
  
-   指示数据流任务是否以优化模式运行（RunInOptimizedMode 属性）。 优化模式会从数据流删除未使用的列、输出和组件，从而提高性能。  
  
    > [!NOTE]  
    >  可以在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中的项目级设置同名属性 RunInOptimizedMode，以指示调试过程中数据流任务以优化模式运行。 此项目属性在设计时将覆盖数据流任务的 RunInOptimizedMode 属性。  
  
### <a name="adjust-the-sizing-of-buffers"></a>调整缓冲区大小  
 数据流引擎通过计算一行数据的估计大小来开始调整其缓冲区大小的任务。 然后引擎将估计的单行大小与 DefaultBufferMaxRows 值相乘以获得缓冲区大小的初步工作值。  
  
-   如果你将 AutoAdjustBufferSize 设置为 true，那么引擎数据流引擎会使用计算的值作为缓冲区大小，并忽略 DefaultBufferSize 的值。  
  
-   如果你将 AutoAdjustBufferSize 设置为 false，那么引擎数据流引擎会使用以下规则来确定缓冲区大小。  
  
    -   如果该结果大于 DefaultBufferSize 值，引擎将减少行数。  
  
    -   如果该结果小于内部计算的最小缓冲区大小，引擎将增加行数。  
  
    -   如果结果在最小缓冲区大小和 DefaultBufferSize 值之间，引擎将调整缓冲区大小，以尽可能接近估计行大小乘以 DefaultBufferMaxRows 值得出的结果。  
  
 开始测试数据流任务的性能时，请使用 DefaultBufferSize 和 DefaultBufferMaxRows 的默认值。 对数据流任务启用日志记录，并选择 BufferSizeTuning 事件以查看每个缓冲区中包含多少行。  
  
 在开始调整缓冲区大小之前，您可以采取的重要改进措施是通过删除不需要的列并配置相应的数据类型来减少每一个数据行的大小。  
  
 若要确定缓冲区的最佳数目及其大小，请在试验 DefaultBufferSize 值和 DefaultBufferMaxRows 值的同时监视性能以及由 BufferSizeTuning 事件报告的信息。  
  
 不要将缓冲区大小增加到开始对磁盘进行分页的点。 与未经过优化的缓冲区大小相比，对磁盘进行分页对性能的阻碍作用更大。 若要确定是否在进行分页，可监视 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台 (MMC) 的性能管理单元中的“Buffers spooled”性能计数器。  
  
### <a name="configure-the-package-for-parallel-execution"></a>将包配置为支持并行执行  
 并行执行能改善具有多个物理或逻辑处理器的计算机的性能。 为了支持在包中并行执行不同的任务， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用两个属性： **MaxConcurrentExecutables** 和 **EngineThreads**。  
  
#### <a name="the-maxconcurrentexcecutables-property"></a>MaxConcurrentExcecutables 属性  
 **MaxConcurrentExecutables** 属性是包本身的一个属性。 此属性定义可同时运行的任务的数量。 默认值为 -1，表示物理或逻辑处理器的个数加 2。  
  
 若要了解此属性的工作原理，可参考一个包含三个数据流任务的示例包。 如果将 **MaxConcurrentExecutables** 设置为 3，则可以同时运行所有三个数据流任务。 但是，假定每个数据流任务都具有 10 个源到目标执行树。 将 **MaxConcurrentExecutables** 设置为 3 不能确保每个数据流任务内的执行树都能并行运行。  
  
#### <a name="the-enginethreads-property"></a>EngineThreads 属性  
 **EngineThreads** 属性是每个数据流任务的属性。 此属性定义数据流引擎可以创建和并行运行的线程数。 **EngineThreads** 属性同样适用于数据流引擎为源创建的源线程和该引擎为转换和目标创建的工作线程。 因此，将 **EngineThreads** 设置为 10 表示该引擎可以创建多达 10 个源线程和多达 10 个工作线程。  
  
 若要理解此属性的工作原理，可参考包含三个数据流任务的示例包。 每个数据流任务都包含 10 个源到目标执行树。 如果将每个数据流任务的 EngineThreads 设置为 10，则可以同时运行所有 30 个执行树。  
  
> [!NOTE]  
>  线程不在本主题的讨论范围之内。 但是，通用规则是并行运行的线程数不要多于可用的处理器个数。 运行的线程数多于可用的处理器个数可能会降低性能，因为此时需要在线程间频繁进行上下文切换。  
  
## <a name="configuring-individual-data-flow-components"></a>配置单个数据流组件  
 若要配置单个数据流组件以优化性能，可以按照某些通用指导原则进行操作。 同时还存在针对各种类型的数据流组件的特定指导原则：源数据流组件、转换数据流组件和目标数据流组件等。  
  
### <a name="general-guidelines"></a>一般性指导  
 无论采用何种数据流组件，为了改善性能您应该遵循下面两个通用指导原则：优化查询和避免不必要的字符串。  
  
#### <a name="optimize-queries"></a>优化查询  
 大量数据流组件都将在从源中提取数据时，或在查询操作中创建引用表时使用查询。 默认查询使用 SELECT * FROM \<tableName> 语法。 这种类型的查询返回源表中的所有列。 在设计时使所有列可用，这意味着可以选择任意列作为查找列、传递列或源列。 但是，在选择了要使用的列后，您应该修改查询使其只包括那些所选择的列。 删除多余的列可以使包中的数据流更高效，因为列越少则创建的行越小。 因为行越小，可以置入一个缓冲区的行就越多，对数据集中所有行进行处理的工作量也就越少。  
  
 您可以键入查询或使用查询生成器来构造查询。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中运行包时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“进度”选项卡将列出警告信息。 其中包括当源向数据流提供了某个数据列但下游数据流组件却没有使用它时出现的警告信息。 您可以使用 **RunInOptimizedMode** 属性来自动删除这些列。  
  
#### <a name="avoid-unnecessary-sorting"></a>避免不必要的排序  
 排序本身是非常缓慢的操作，因此避免不必要的排序可以提高包数据流的性能。  
  
 某些情况下，源数据在下游组件使用其之前已经进行了排序。 当 SELECT 查询使用 ORDER BY 子句或者数据按排序顺序插入源中时，即出现这种预排序。 对于这种预排序的源数据，您可以提供一个提示说明数据已排序，从而避免使用排序转换来满足特定下游转换的排序要求。 （例如，合并和合并联接转换要求使用已排序的输入。）若要提供一个提示说明数据已排序，必须执行下面的任务：  
  
-   将上游数据流组件输出上的 **IsSorted** 属性设置为 **True**。  
  
-   然后指定数据排序所依据的排序键列。  
  
 有关详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。  
  
 如果必须在数据流中对数据排序，则可以将数据流设计为使用尽可能少的排序操作来提高性能。 例如，数据流使用多播转换复制数据集。 可以在多播转换运行前对数据集进行一次排序，而不是在转换后再对多个输出进行排序。  
  
 有关详细信息，请参阅 [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md)、 [Merge Transformation](../../integration-services/data-flow/transformations/merge-transformation.md)、 [Merge Join Transformation](../../integration-services/data-flow/transformations/merge-join-transformation.md)和 [Multicast Transformation](../../integration-services/data-flow/transformations/multicast-transformation.md)。  
  
### <a name="sources"></a>源  
  
#### <a name="ole-db-source"></a>OLE DB 源  
 使用 OLE DB 源从视图中检索数据时，选择“SQL 命令”作为数据访问模式并输入 SELECT 语句。 访问数据时，使用 SELECT 语句要比选择“表或视图”作为数据访问模式的执行效果更佳。  
  
### <a name="transformations"></a>转换  
 使用本节中的建议可以改善聚合、模糊查找、模糊分组、查找、合并联接和渐变维度转换的性能。  
  
#### <a name="aggregate-transformation"></a>聚合转换  
 聚合转换包括 **Keys**、 **KeysScale**、 **CountDistinctKeys** 和 **CountDistinctScale** 属性。 通过使用这些属性，使转换能够为转换缓存的数据预先分配转换所需的内存量，从而提高了性能。 如果知道要从 **“分组依据”** 操作产生的准确或近似组数，则可分别设置 **Keys** 和 **KeysScale** 属性。 如果知道要从 **“非重复计数”** 操作产生的非重复值的准确或近似数量，则可分别设置 **CountDistinctKeys** 和 **CountDistinctScale** 属性。  
  
 如果需要在数据流中创建多个聚合，应考虑使用一个聚合转换而不是创建多个转换来创建多个聚合。 如果聚合是其他聚合的子集，这种方法能够提高性能，因为转换可以优化内部存储，并且只需扫描传入的数据一次。 例如，如果聚合使用 GROUP BY 子句和 AVG 聚合，将它们组合成一个转换可以提高性能。 但是，在一个聚合转换内执行多个聚合会序列化聚合操作，因此，当必须独立计算多个聚合时，这种方法可能不会改善性能。  
  
#### <a name="fuzzy-lookup-and-fuzzy-grouping-transformations"></a>模糊查找和模糊分组转换  
 有关如何优化模糊查找和模糊分组转换的性能的信息，请参阅白皮书： [Fuzzy Lookup and Fuzzy Grouping in SQL Server Integration Services 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90))（SQL Server Integration Services 2005 中的模糊查找和模糊分组转换）。  
  
#### <a name="lookup-transformation"></a>查找转换  
 通过输入仅查找所需列的 SELECT 语句，最小化内存中引用数据的大小。 这种方法优于选择整个表或视图，因为后者将返回大量不必要的数据。  
  
#### <a name="merge-join-transformation"></a>Merge Join Transformation  
 您不再必须配置 **MaxBuffersPerInput** 属性的值，因为 Microsoft 已进行了更改，减少了合并联接转换将占用过多内存的风险。 在合并联接的多个输入以不相等速率生成数据时，有时候可能会发生此问题。  
  
#### <a name="slowly-changing-dimension-transformation"></a>渐变维度转换  
 渐变维度向导和渐变维度转换是能满足大多数用户需要的通用工具。 但是，该向导生成的数据流未针对性能进行优化。  
  
 通常，渐变维度转换中最慢的组件是一次对单行执行 UPDATE 的 OLE DB 命令转换。 因此，改善渐变维度转换性能最有效的方法是替换 OLE DB 命令转换。 可以用目标组件来替换这些转换，目标组件将要更新的所有行保存到一个临时表中。 然后，可以添加执行 SQL 任务，该任务同时对所有行执行基于单集的 Transact-SQL UPDATE。  
  
 高级用户可以为渐变维度处理设计自定义数据流，此数据流将针对大型维度进行优化。 有关此方法的讨论和示例，请参阅白皮书 [Project REAL: Business Intelligence ETL Design Practices](https://www.microsoft.com/download/details.aspx?id=14582)（Project REAL：Business Intelligence ETL 设计实践）中的章节 "Unique dimension scenario"（唯一维度方案）。  
  
### <a name="destinations"></a>Destinations  
 若要改善目标的性能，请考虑使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标并测试目标的性能。  
  
#### <a name="sql-server-destination"></a>SQL Server 目标  
 当包将数据加载到同一计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标。 此目标针对高速大容量加载进行优化。  
  
#### <a name="testing-the-performance-of-destinations"></a>测试目标的性能  
 您可能会发现将数据保存到目标时所花的时间比预期的要长。 为了确定速度缓慢是否是由于目标处理数据的能力不足造成的，可以暂时将目标替换为行计数转换。 如果吞吐量显著提高，很可能是加载数据的目标导致速度减缓。  
  
### <a name="review-the-information-on-the-progress-tab"></a>查看“进度”选项卡上的信息  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供有关在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中运行包时控制流和数据流的信息。 **“进度”** 选项卡按执行顺序列出任务和容器，而且还包括每个任务和容器及包自身的开始时间和结束时间、警告以及错误消息。 它还按执行顺序列出数据流组件并包括进度信息（显示为完成百分比）和处理的行数。  
  
 若要允许或禁止在 **“进度”** 选项卡上显示消息，请在 **SSIS** 菜单上切换 **“调试进度报告”** 选项。 禁用进度报告有助于在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中运行复杂包时改进性能。  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [为合并转换和合并联接转换排序数据](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-content"></a>相关内容  
 **文章和博客文章**  
  
-   technet.microsoft.com 上的技术文章 [SQL Server 2005 Integration Services：性能策略](/previous-versions/sql/sql-server-2005/administrator/cc966530(v=technet.10))  
  
-   technet.microsoft.com 上的技术文章 [Integration Services：性能优化技术](/previous-versions/sql/sql-server-2005/administrator/cc966529(v=technet.10))  
  
-   SQLCAT 针对 BI 和 Analytics 的指南中的技术文章[通过将同步转换拆分为多个任务来增加管道的吞吐量](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20BI%20and%20Analytics.pdf)
  
-   msdn.microsoft.com 上的技术文章 [数据加载性能指南](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))。  
  
-   msdn.microsoft.com 上的技术文章 [我们使用 SSIS 加载 1TB 数据仅用了 30 分钟，因此您也行](/previous-versions/sql/sql-server-2008/dd537533(v=sql.100))。  
  
-   sqlcat.com 上的技术文章 [前 10 个 SQL Server Integration Services 最佳实践](https://techcommunity.microsoft.com/t5/datacat/top-10-sql-server-integration-services-best-practices/ba-p/305163)。  
  
-   sqlcat.com 上的技术文章和示例 [针对 SSIS 的“平衡的数据分发服务器”](https://www.sqlshack.com/ssis-balanced-data-distributor-overview/)。  
  
-   blogs.msdn.com 上的博客文章 [解决 SSIS 包性能问题](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553)。  
  
 **视频**  
  
-   视频系列 [设计和优化企业中 SSIS 包的性能（SQL 视频系列）](https://go.microsoft.com/fwlink/?LinkId=400878)  
  
-   technet.microsoft.com 上的视频 [优化企业中的 SSIS 包数据流（SQL Server 视频）](/previous-versions/ff686901(v=msdn.10))  
  
-   technet.microsoft.com 上的视频 [理解 SSIS 数据流缓冲区（SQL Server 视频）](/previous-versions/ff686905(v=msdn.10))  
  
-   channel9.msdn.com 上的视频 [Microsoft SQL Server Integration Services 性能设计模式](https://go.microsoft.com/fwlink/?LinkID=233698&clcid=0x409)。  
  
-   sqlcat.com 上的演示文稿 [Microsoft IT 如何利用 SQL Server 2008 SSIS 数据流引擎的增强功能](https://channel9.msdn.com/Shows/TechNet+Radio/TechNet-Radio-How-Microsoft-IT-Leverages-SQL-Server-2008-SSIS-Dataflow-Engine-Enhancements)。  
  
-   technet.microsoft.com 上的视频 [平衡的数据分发服务器](/previous-versions/dn912438(v=msdn.10))。  
  
## <a name="see-also"></a>另请参阅  
 [包开发的故障排除工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)   
 [包执行的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
