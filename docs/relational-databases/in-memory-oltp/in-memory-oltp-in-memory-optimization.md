---
title: 内存中 OLTP（内存中优化）| Microsoft Docs
description: 将这些示例和资源用于内存中 OLTP，后者可显著提高 SQL Server 中的性能。
ms.custom: ''
ms.date: 11/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 281fe018c5c2d5e0717fb2c35dfcc8c3c9e8c3fc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100220"
---
# <a name="in-memory-oltp-and-memory-optimization"></a>内存中 OLTP 和内存优化

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 可显著改善事务处理、数据引入和数据加载的性能以及暂时数据方案。  若要跳转到快速测试自己的内存优化表和本机编译的存储过程所需的基本代码和知识，请参阅
 -  [快速入门 1：可提高 Transact SQL 性能的内存中 OLTP 技术](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)。  
 
我们向 YouTube 上传了说明 SQL Server 上的内存中 OLTP 并演示性能优势的 [17 分钟视频](#anchorname-17minute-video)。

深入了解内存中 OLTP 的详细概述以及查看显示技术中性能优势的方案：

- [概述和使用方案](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 请注意， [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 是用于提高事务处理性能的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 技术。 有关提高报告和分析查询性能的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 技术，请参阅 [列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)。
  
 已对 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]，以及 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的内存中 OLTP 进行了多项改进。 增加了 Transact-SQL 外围应用，以使其更易于迁移数据库应用程序。 添加了对内存优化表和本机编译的存储过程执行 ALTER 操作的支持，以使其更易于维护应用程序。
  
> [!NOTE]  
>  **进行试用**  
>   
>  内存中 OLTP 在高级层和业务关键层 Azure SQL 数据库和弹性池中可用。 若要在 Azure SQL 数据库中开始使用内存中 OLTP 以及列存储，请参阅 [Optimize Performance using In-Memory Technologies in SQL Database](/azure/azure-sql/in-memory-oltp-overview)（在 SQL 数据库中使用内存中技术优化性能）。  
  

## <a name="in-this-section"></a>在本节中  
 本节包括下列主题：  
  
|主题|说明|  
|-----------|-----------------|  
|[快速入门 1：可提高 Transact SQL 性能的内存中 OLTP 技术](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|深入探讨内存中 OLTP|
|[概述和使用方案](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|有关什么是内存中 OLTP 以及什么是显示性能优势的方案的概述。|
|[使用内存优化表的要求](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|讨论使用内存优化的表的硬件和软件要求及指导原则。|  
|[内存中 OLTP 代码示例](./sample-database-for-in-memory-oltp.md)|包含说明如何创建和使用内存优化的表的代码示例。|  
|[Memory-Optimized Tables](./sample-database-for-in-memory-oltp.md)|介绍内存优化的表。|  
|[内存优化表变量](./faster-temp-table-and-table-variable-by-using-memory-optimization.md)|一个代码示例，其中展示如何使用内存优化的表变量代替传统的表变量以减少 tempdb 的使用次数。|  
|[内存优化表上的索引](./indexes-for-memory-optimized-tables.md)|介绍内存优化索引。|  
|[本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)|介绍本机编译的存储过程。|  
|[管理内存中 OLTP 的内存](/previous-versions/sql/sql-server-2016/dn465872(v=sql.130))|了解和管理系统中的内存用量。|  
|[创建和管理用于内存优化对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|论述数据和差异文件，其中存储有关内存优化的表中事务的信息。|  
|[内存优化表的备份、还原和恢复](/previous-versions/sql/sql-server-2016/dn624160(v=sql.130))|讨论内存优化表的备份、还原和恢复。|  
|[对内存中 OLTP 的 Transact-SQL 支持](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|讨论 [!INCLUDE[tsql](../../includes/tsql-md.md)] 对 [!INCLUDE[hek_2](../../includes/hek-2-md.md)]的支持。|  
|[对内存中 OLTP 数据库的高可用性支持](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|讨论 [!INCLUDE[hek_2](../../includes/hek-2-md.md)]中的可用性组和故障转移群集。|  
|[SQL Server 对内存中 OLTP 的支持](./transact-sql-support-for-in-memory-oltp.md)|列出支持内存优化表的新增和更新的语法和功能。|  
|[迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)|论述如何将基于磁盘的表迁移到内存优化的表。|  
| &nbsp; | &nbsp; |

## <a name="links-to-other-websites"></a>其他网站的链接

此部分提供其他网站的链接，这些网站包含有关 SQL Server 上的内存中 OLTP 的信息。

- [说明内存中 OLTP 并演示性能优势的视频](#anchorname-17minute-video)

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [SQL Server 内存中 OLTP 内部组件技术白皮书](./sql-server-in-memory-oltp-internals-for-sql-server-2016.md)  

-   [SQL Server In-Memory OLTP and Columnstore Feature Comparison](https://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   SQL Server 2016 内存中 OLTP 的新增功能[第 1 部分](/archive/blogs/sqlserverstorageengine/in-memory-oltp-whats-new-in-sql2016-ctp3)和[第 2 部分](/archive/blogs/sqlserverstorageengine/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3)
  
-   [内存中 OLTP - 常见的工作负荷模式和迁移注意事项](/previous-versions/dn673538(v=msdn.10))  
  
-   [内存中 OLTP 博客](https://cloudblogs.microsoft.com/sqlserver/2013/06/26/sql-server-2014-in-memory-technologies-blog-series-introduction/)  

## <a name="17-minute-video-indexed"></a><a name="anchorname-17minute-video"></a>已编制索引的 17分钟视频

- _视频标题：_ &nbsp; **SQL Server 2016 中的内存中 OLTP**
- _发布日期：_ &nbsp; 2019 年 3 月 10 日，发布于 `YouTube.com`。
- _持续时间：_ &nbsp; 17:32 &nbsp; &nbsp;（有关视频链接，请参阅以下 [索引](#anchorname-index-17minute-video)。）
- _：_ &nbsp; SQL Server 高级项目经理 Jos de Bruijn

### <a name="demo-can-be-downloaded"></a>可以下载演示

在时间标记 08:09 处，视频会运行两次演示。 可通过以下链接下载视频中使用的可运行性能演示的源代码：

- [内存中 OLTP 性能演示 v1.0，源代码](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

视频中所示的常规步骤如下：

1. 演示首先使用一个常规表来运行。
2. 接下来，我们会看到如何通过在 SQL Server Management Studio (SSMS) 中单击几次来创建和填充表的内存优化版本。
3. 随后演示使用内存优化表重新运行。 可测量到速度大幅提高。

### <a name="index-to-each-section-in-the-video"></a><a name="anchorname-index-17minute-video"></a>视频中每节的索引

| 时间标记链接 | 节标题 |
| :------------- | :------------ |
| A.&nbsp;[00:00](https://www.youtube.com/watch?v=l5l5eophmK4&t=0) | 开头。 |
| <br/>B.&nbsp;[00:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=56) | <br/>客户为何应关注内存中 OLTP。 |
| &nbsp; &nbsp; [01:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=63) | 新式硬件需要数据库系统的新式体系结构。 |
| &nbsp; &nbsp; [02:10](https://www.youtube.com/watch?v=l5l5eophmK4&t=130) | 所生成数据的分解；操作需要是即时的（低延迟）。 |
| &nbsp; &nbsp; [03:19](https://www.youtube.com/watch?v=l5l5eophmK4&t=199) | 降低 TCO — 使用拥有的资源完成更多工作。 |
| <br/>C.&nbsp;[03:33](https://www.youtube.com/watch?v=l5l5eophmK4&t=213) | <br/>什么是内存中 OLTP。<br/>使用内存优化技术优化了性能。 |
| &nbsp; &nbsp; [05:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=303) | 事务处理速度提高多达 30 倍。 |
| &nbsp; &nbsp; [05:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=322) | 完全持久 — 数据可在发生服务器故障时保留下来。 |
| &nbsp; &nbsp; [06:15](https://www.youtube.com/watch?v=l5l5eophmK4&t=375) | 完整集成在 SQL Server 中。 因此无需学习新语言或工具。 |
| &nbsp; &nbsp; [07:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=442) | 在 SQL Server 2014 中首次发布，但是在 2016 中进行了重大改进。 |
| &nbsp; &nbsp; [07:58](https://www.youtube.com/watch?v=l5l5eophmK4&t=558) | 也在 Azure SQL 数据库中可用（云中）。 |
| <br/>D.&nbsp;[08:09](https://www.youtube.com/watch?v=l5l5eophmK4&t=489) | <br/>性能演示。<br/> 使用一个常规表运行演示。 |
| &nbsp; &nbsp; [09:11](https://www.youtube.com/watch?v=l5l5eophmK4&t=551) | SSMS 上下文菜单：“报表”&gt;“事务性能分析”  |
| &nbsp; &nbsp; [10:38](https://www.youtube.com/watch?v=l5l5eophmK4&t=638) | SSMS 上下文菜单：**内存优化顾问**<br/> &nbsp; &nbsp; 通过常规表实际创建内存优化表，以及迁移数据。 |
| &nbsp; &nbsp; [11:28](https://www.youtube.com/watch?v=l5l5eophmK4&t=688) | 重新运行演示，速度提高 45 倍。 |
| <br/>E.&nbsp;[12:17](https://www.youtube.com/watch?v=l5l5eophmK4&t=737) | <br/>在 SQL Server 2016 中更易于使用内存中 OLTP（与 2014 相比）。 |
| &nbsp; &nbsp; [12:43](https://www.youtube.com/watch?v=l5l5eophmK4&t=763) | 简化了分析以帮助进行应用迁移。 |
| &nbsp; &nbsp; [13:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=783) | 通过改进的 Transact-SQL 语言支持（例如，具有外键和触发器）降低了应用迁移的复杂性。 |
| &nbsp; &nbsp; [13:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=836) | 提高了可管理性。<br/> &nbsp; &nbsp; 例如，更改架构和索引，以及自动更新统计信息。 |
| <br/>F.&nbsp;[14:46](https://www.youtube.com/watch?v=l5l5eophmK4&t=886) | <br/>改进了可伸缩性。 |
| &nbsp; &nbsp; [15:12](https://www.youtube.com/watch?v=l5l5eophmK4&t=912) | 大型内存优化表（每个数据库多达 2TB）。 |
| &nbsp; &nbsp; [15:34](https://www.youtube.com/watch?v=l5l5eophmK4&t=934) | 更好地缩放。 |
| &nbsp; &nbsp; [16:41](https://www.youtube.com/watch?v=l5l5eophmK4&t=1001) | 使用已拥有的资源完成更多工作！ |
| <br/>G.&nbsp;[16:53](https://www.youtube.com/watch?v=l5l5eophmK4&t=1013) | <br/>最终注释。 （17:32 结束。） |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>另请参阅  
 [数据库功能](../databases/databases.md)  
