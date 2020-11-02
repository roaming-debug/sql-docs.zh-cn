---
title: DTA 建议的性能改进
description: 了解数据库引擎优化顾问如何通过分析 SQL Server 中的数据库工作负载，建议行存储和列存储索引的组合。
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, performance improvements
ms.assetid: 2e51ea06-81cb-4454-b111-da02808468e6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 8c64f567d8da74a2eb15b5647a2b259ba52193b4
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678948"
---
# <a name="performance-improvements-using-database-engine-tuning-advisor-dta-recommendations"></a>使用数据库引擎优化顾问 (DTA) 改进性能的建议
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


---
**列存储** 索引能够大幅提升数据仓库和分析工作负荷的性能，尤其是对于需要扫描大型表的查询。 对于访问相对较少量的数据和搜索特定值或值范围的查询而言， **行存储** （B+ 树）索引可发挥最大的效果。 由于行存储索引可按排序顺序传送行，因此还可以减少查询执行计划中数据排序产生的开销。 选择生成行存储索引与列存储索引的哪种组合取决于应用程序的工作负荷。

数据库引擎优化顾问 (DTA) 首次在 SQL Server 2016 中引入，可以通过分析给定的数据库工作负荷来建议适当的 **行存储索引和列存储索引组合** 。 

为了演示 DTA 建议对工作负荷性能带来的好处，我们试验几个真实的客户工作负荷。 对于每个客户工作负荷，我们让 DTA 分析了单个查询以及整个查询工作负荷。 我们考虑了三种备选方案：
  
  1. **仅限列存储** ：仅为所有表生成列存储索引且不使用 DTA。 
  2. **DTA（仅限行存储）** ：结合仅建议行存储索引的选项运行 DTA。
  3. **DTA（行存储 + 列存储）** ：结合同时建议行存储索引和列存储索引的选项运行 DTA。  
   
然后，对于每种方案，我们实现了建议的索引。 我们报告了多次运行查询或工作负荷后的平均 CPU 时间（以毫秒为单位）。 下图描绘了两个不同客户数据库中工作负荷的 CPU 时间（以毫秒为单位）。 请注意，Y 轴（CPU 时间）使用对数刻度。   


![显示 DTA 列存储行存储性能的条形图的屏幕截图。](../../relational-databases/performance/media/dta-columnstore-rowstore-performance.gif)



**混合物理设计的需要** ：对应于客户 1 查询 1 的第一组条形。 DTA（行存储 + 列存储）建议四个列存储索引和六个行存储索引的组合，与仅限列存储索引和 DTA（仅限行存储）的方案相比，可将 CPU 时间降低 2.5 到 4 倍。 此方案展示了 *即使对于单个查询* ，由行存储索引和列存储索引构成的混合物理设计也能带来的优势。 

**行存储索引建议的有效性** ：第二和第三组条形（对应于客户 1 查询 2 和客户 2 查询 1）表示查询包含可从适当行存储索引受益的选择性筛选器谓词。 对于这两个查询，DTA（仅限行存储）和 DTA（行存储 + 列存储）只建议行存储索引。 这些示例还表明，即使结合用于建议列存储索引的选项调用 DTA，基于开销的方法也能确保仅当工作负荷能够从列存储索引受益时，才建议这种索引。

**列存储索引建议的有效性** ：对应于客户 2 查询 2 第四组条形表示查询将扫描可从列存储索引受益的大型表。 与存在列存储索引时相比，DTA（仅限行存储）生成的建议的 CPU 时间更大。 DTA（行存储 + 列存储）建议适当的列存储索引，因此与仅限列存储选项的查询执行性能相当。

**针对包含多个查询的工作负荷中的建议有效性** ：对应于客户 2 整个工作负荷的最后一组条形示范了 DTA 在工作负荷中分析多个查询，以建议一组适当的、可改善整个工作负荷的执行开销的行存储索引和列存储索引的能力。 DTA（行存储 + 列存储）建议四个列存储索引和几十个行存储索引，与仅生成列存储索引的选项相比，对工作负荷的改善超过了一个量级；与 DTA（仅限行存储）相比，带来了 4 到 5 倍的改善。

概括而言，上面的示例演示了 DTA 适当利用 SQL Server 数据库引擎中支持的行存储索引和列存储索引，建议可大幅降低工作负载的 CPU 时间的索引组合的能力。 

<a name="see-also"></a>另请参阅
---
[Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)

[数据引擎优化顾问 (DAT) 中的列存储索引建议](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)

[列存储索引指南](~/relational-databases/indexes/columnstore-indexes-overview.md)

[针对数据仓库的列存储索引](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)

[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)

[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)



