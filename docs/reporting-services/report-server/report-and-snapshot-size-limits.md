---
title: 报表和快照的大小限制 | Microsoft Docs
description: 了解在报表服务器中发布报表、在运行时呈现报表以及将报表保存到文件系统时报表的大小限制。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- large reports
- maximum report size
- size [SQL Server], reports
- report size [Reporting Services]
- reports [Reporting Services], size
- denial of service attacks [Reporting Services]
ms.assetid: 1e3be259-d453-4802-b2f5-6b81ef607edf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5b5e8dbaee71c53f8f9491f8a1abcd15559e4800
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596846"
---
# <a name="report-and-snapshot-size-limits"></a>报表和快照的大小限制
  管理 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署的管理员可以通过本主题提供的信息了解在将报表发布到报表服务器、运行时呈现报表以及将报表保存到文件系统时报表的大小限制。 本主题还提供了有关如何度量报表服务器数据库大小的实践指南，并介绍了快照大小对服务器性能的影响。  
  
## <a name="maximum-size-for-published-reports"></a>已发布报表的最大大小  
 在报表服务器上，报表和模型的大小基于发布到报表服务器的报表定义 (.rdl) 和报表模型 (.smdl) 文件的大小。 报表服务器不限制发布的报表的大小。 不过，[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 规定了发布到服务器的项的大小上限。 默认情况下，大小限制为 4 MB。 如果将超出此限制的文件上载或发布到报表服务器，就会收到 HTTP 异常。 在这种情况下，可以通过增大 Machine.config 文件中 **maxRequestLength** 元素的值来修改该默认值。  
  
 尽管报表模型可能很大，但报表定义很少超过 4 MB。 比较常见的报表大小以千字节 (KB) 为单位。 但是，如果包括嵌入的图像，则这些图像的编码可能会造成报表定义过大（超过 4 MB 的默认值）。  
  
 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 对发布的文件实施最大限制，以减小拒绝服务攻击对服务器的威胁。 增大上限的值会破坏此限制所提供的某些保护。 请在确信增大该值所获得的好处大于任何其他安全隐患时才这样做。  
  
 请记住，为 **maxRequestLength** 元素设置的值必须大于要强制实施的实际大小限制。 当在 SOAP 信封中封装所有参数，以及对某些参数应用 Base64 编码之后，HTTP 请求大小会不可避免地增加，因此你需要设置较大的值以考虑这种情况。 Base64 编码会将原始数据增大约 33%。 因此，为 **maxRequestLength** 元素指定的值需要比实际可用项目大小高出约 33%。 例如，如果为 **maxRequestLength** 指定 64 MB 的值，则实际上可以预计发布到报表服务器的报表文件的最大大小约为 48 MB。  
  
## <a name="report-size-in-memory"></a>内存中的报表大小  
 运行报表时，报表大小等于报表中返回的数据量加上输出流的大小。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 对呈现报表的大小无最大限制。 系统内存决定大小的上限（默认情况下，在呈现报表时，报表服务器将使用所有可用的已配置内存），但您可以指定配置设置以设置内存阈值和内存管理策略。 有关详细信息，请参阅 [为报表服务器应用程序配置可用内存](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)。  
  
 任何报表的大小都会因返回的数据量以及对报表使用的呈现格式而存在很大的差异。 参数化报表可能较大也可能较小，具体取决于参数值对查询结果的影响方式。 您选择的报表输出格式将通过以下方式影响报表大小：  
  
-   HTML 每次处理一页报表。 由于报表以较小的单元来进行处理，因此处理特定块区所需的内存也更少。  
  
-   在向用户显示报表之前，PDF、Excel、TIFF、XML 和 CSV 会在内存中处理整个报表。  
  
 若要度量所呈现报表的大小，可以查看报表执行日志。 有关详细信息，请参阅 [报表服务器 ExecutionLog 和 ExecutionLog3 视图](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md)。  
  
 若要计算所呈现报表在磁盘中的大小，可以导出该报表，然后将其保存到文件系统（保存的文件包括数据和报表格式信息）。  
  
 仅在呈现到 Excel 格式时，才对报表大小有严格限制。 工作表不能超过 65536 行或 256 列。 其他呈现格式没有这些限制，因此大小仅受服务器上资源量的限制。  
  
> [!NOTE]  
>  报表处理和呈现都在内存中进行。 如果您具有大型报表或大量用户，则务必进行某种容量规划，确保您执行的报表服务器部署令用户满意。 有关工具和指南的详细信息，请参阅 MSDN 上的以下发布内容： [Planning for Scalability and Performance with Reporting Services](/previous-versions/sql/sql-server-2005/administrator/cc966418(v=technet.10)) （规划 Reporting Services 的扩展性和性能）和 [Using Visual Studio 2005 to Perform Load Testing on a SQL Server 2005 Reporting Services Report Server](/previous-versions/sql/sql-server-2005/administrator/aa964139(v=sql.90))（使用 Visual Studio 2005 在 SQL Server 2005 Reporting Services 报表服务器上执行负载测试）。  
  
## <a name="measuring-snapshot-storage"></a>度量快照存储  
 任何给定快照的大小都与报表中的数据量成正比。 快照通常比报表服务器上存储的其他项大得多。 快照大小通常在几 MB 到几十 MB 之间。 如果报表非常大，则可能会看到更大的快照。 根据使用快照的频率以及配置报表历史记录的方式，报表服务器数据库所需的磁盘空间量可能会在短期内迅速增加。  
  
 默认情况下， **reportserver** 和 **reportservertempdb** 数据库设置为自动增长。 尽管数据库大小会自动增大，但绝不会自动减小。 如果 **reportserver** 数据库由于您删除了快照而具有多余容量，则必须手动减少容量，以恢复磁盘空间。 同样，如果 **reportservertempdb** 增大以容纳超高的交互式报表量，则减少容量之前，磁盘空间分配将始终保持此设置。  
  
 若要度量报表服务器数据库的大小，可以运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令。 定期计算数据库总大小有助于合理估计在一段时间内如何为报表服务器数据库分配空间。 下面的语句用于测量当前使用的空间量（这些语句假设你使用的是默认数据库名称）：  
  
```  
USE ReportServer  
EXEC sp_spaceused  
```  
  
## <a name="snapshot-size-and-report-server-performance"></a>快照大小和报表服务器性能  
 快照大小会影响处理和呈现报表时的服务器性能。 呈现操作对服务器性能的影响最大，因此如果您具有大型快照，则在用户请求报表时可能会出现延迟。 当快照大小超过 100 MB 时，根据用户的数量，可能会遇到延迟情况。  
  
 若要最大程度地降低因大型快照造成的性能延迟，可以执行以下操作：  
  
-   在不同的计算机上部署报表服务器和 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。  
  
-   添加更多系统内存。  
  
-   查阅 MSDN 网站上的“Reporting Services 的伸缩性和性能表现规划”文档，以了解如何为企业配置报表服务器的最佳实践。  
  
 报表服务器数据库中存储的快照数量本身并不是一个性能因素。 可以存储大量快照，而不对服务器性能产生任何影响。 可以无限期地保留快照。 但应注意，报表历史记录是可配置的。 如果报表服务器管理员降低了报表历史记录限制，您可能会丢失要保留的历史报表。 如果删除报表，则所有的报表历史记录也将随之删除。  
  
## <a name="see-also"></a>另请参阅  
 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   
 [报表服务器数据库（SSRS 本机模式）](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [处理大型报表](../../reporting-services/report-server/process-large-reports.md)  
  
