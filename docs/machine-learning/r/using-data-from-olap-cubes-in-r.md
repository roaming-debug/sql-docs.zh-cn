---
title: 在 R 中使用来自 OLAP 多维数据集的数据
description: 本文介绍了 olapR API，并概述了适用于不熟悉多维数据集数据库的 R 用户的 OLAP 和 MDX。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/15/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: e44d7393b3f5dd447cc64a742181e535d05081e1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470748"
---
# <a name="using-data-from-olap-cubes-in-r"></a>在 R 中使用来自 OLAP 多维数据集的数据
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

olapR 包是由 Microsoft 提供的 R 包，可与 Machine Learning Server 和 SQL Server 结合使用，让你可以运行 MDX 查询，以获取来自 OLAP 多维数据集的数据  。 通过此包，无需创建链接服务器或清除平展行集；可以直接从 R 中获取 OLAP 数据。

本文介绍了 API，并概述了适用于不熟悉多维数据集数据库的 R 用户的 OLAP 和 MDX。

> [!IMPORTANT]
> Analysis Services 实例可支持常规多维数据集或表格模型，但一个实例不能同时支持这两种类型的模型。 因此，在尝试针对多维数据集生成 MDX 查询之前，请验证 Analysis Services 实例是否包含多维模型。

## <a name="what-is-an-olap-cube"></a>什么是 OLAP 多维数据集？

OLAP 是联机分析处理的缩写。 一直以来，OLAP 解决方案广泛用于捕获和存储重要的业务数据。 通过多种工具、仪表板和可视化效果将 OLAP 数据用于业务分析。 有关详细信息，请参阅[联机分析处理](https://en.wikipedia.org/wiki/Online_analytical_processing)。

Microsoft 提供了 [Analysis Services](/analysis-services/analysis-services-overview)，这使你可以以多维数据集或表格模型的形式设计、部署和查询 OLAP 数据   。 多维数据集是一种多维数据库。 维度与数据的方面或 R 中的因素类似：使用维度来标识要汇总或分析的某些特定数据子集  。 例如，时间是一个重要维度，以至于许多 OLAP 解决方案都包含默认定义的多个日历，以便在切片和汇总数据时使用。 

出于性能方面的考虑，OLAP 数据库通常会预先计算汇总（或聚合），然后将其进行存储以实现更快的检索  。 汇总基于度量值，后者表示可应用于数值数据的公式  。 可使用维度来定义数据子集，然后计算该数据的度量值。 例如，可使用度量值计算某个产品系列多个季度减去税费后的总销售额、报告特定供应商的平均运输成本以及年初至今支付的累计工资等。

MDX（即多维表达式的缩写）是用于查询多维数据集的语言。 MDX 查询通常包含一个数据定义，该数据定义包含一个或多个维度，以及至少一个度量值，但是，MDX 查询也可能更加复杂，并且包括滚动窗口、累计平均值、求和、排名或百分位数。 

以下是开始生成 MDX 查询时可能会有所帮助的一些其他术语：

+ “切片”通过使用来自单个维度的值获取多维数据集的子集  。

+ 通过指定多个维度上值的范围，“切块”  创建子多维数据集。

+ “向下钻取”  从摘要导航到详细信息。

+ “向上钻取”  从详细信息移动到较高级别的聚合。

+ “汇总”  总结某个维度上的数据。

+ “透视”  旋转多维数据集或数据选择。

## <a name="how-to-use-olapr-to-create-mdx-queries"></a>如何使用 olapR 创建 MDX 查询

下面的文章提供了针对多维数据集创建或执行查询的语法的详细示例：

+ [如何使用 R 创建 MDX 查询](../../machine-learning/r/how-to-create-mdx-queries-using-olapr.md)

## <a name="olapr-api"></a>olapR API

**olapR** 包支持两种创建 MDX 查询的方法：

- 使用 MDX 生成器。  使用包中的 R 函数生成简单的 MDX 查询，方法是选择一个多维数据集，然后设置轴和切片器。 如果无法访问传统 OLAP 工具，或对 MDX 语言没有深入的了解，则可以使用此简单方法生成有效的 MDX 查询。

    并非所有 MDX 查询都可以使用此方法创建，因为 MDX 可能非常复杂。 但是，此 API 支持大多数最常见和有用的操作，包括在 N 维度中切片、切块、向下钻取、汇总和透视。

+ 复制粘贴格式正确的 MDX。  手动创建，然后在任何 MDX 查询中粘贴。 如果有要重新使用的现有 MDX 查询，或如果要生成的查询对于 olapR 而言处理太过复杂，则此选项是最佳选择  。

    在使用任何客户端实用工具（如 SSMS 或 Excel）生成 MDX 后，保存查询字符串。 将此 MDX 字符串作为参数提供给 olapR 中的 SSAS 查询处理程序   。 提供程序会将查询发送到指定的 Analysis Services 服务器，并将结果传递回 R。 

有关如何生成 MDX 查询或运行现有 MDX 查询的示例，请参阅[如何使用 R 创建 MDX 查询](../../machine-learning/r/how-to-create-mdx-queries-using-olapr.md)。

## <a name="known-issues"></a>已知问题

本部分列出了有关 olapR 包的一些已知问题和常见问题  。

### <a name="tabular-model-support"></a>表格模型支持

如果连接到包含表格模型的 Analysis Services 实例，则 `explore` 函数将报告成功，并返回值 TRUE。 但是，表格模型对象不同于多维对象，且多维数据库的结构也不同于表格模型的结构。

尽管 DAX（数据分析表达式）是表格模型通常使用的语言，但如果你已熟悉 MDX，则可以针对表格模型设计有效的 MDX 查询。 不能使用 olapR 构造函数针对表格模型生成有效 MDX 查询。

但是，使用 MDX 查询从表格模型中检索数据是一种效率低下的方式。 如果需要从表格模型中获取数据以在 R 中使用，请考虑改用以下方法：

+ 在模型上启用 DirectQuery，并将服务器作为链接服务器添加到 SQL Server 中。 
+ 如果表格模型是在关系数据市场上构建的，则直接从源获取数据。

### <a name="how-to-determine-whether-an-instance-contains-tabular-or-multidimensional-models"></a>如何确定实例是否包含表格模型或多维模型

一个 Analysis Services 实例只能包含一种类型的模型，但可以包含多个模型。 原因在于，表格模型和多维模型之间在控制数据存储和处理方式方面存在根本差异。 例如，表格模型存储在内存中，并利用列存储索引执行非常快速的计算。 在多维模型中，数据存储在磁盘上，并且已预先定义聚合，且通过使用 MDX 查询进行检索。

如果使用 SQL Server Management Studio 等客户端连接到 Analysis Services，则可以通过查看数据库图标一眼看出支持的模型类型。

还可以查看和查询服务器属性，以确定实例支持的模型类型。 服务器模式属性支持两个值：多维或表格    。

有关这两种模型类型的常规信息，请参阅以下文章：

+ [比较多维模型和表格模型](/analysis-services/comparing-tabular-and-multidimensional-solutions-ssas)

有关查询服务器属性的信息，请参阅以下文章：

+ [OLE DB for OLAP 架构行集](/previous-versions/sql/sql-server-2012/ms126079(v=sql.110))

### <a name="writeback-is-not-supported"></a>不支持写回

不能将自定义 R 计算的结果写回到多维数据集。

通常情况下，即使为写回启用了多维数据集，也仅支持有限的操作，并且可能还需要其他配置。 建议使用 MDX 进行此类操作。

+ [启用写操作的维度](/analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions)
+ [可写入的分区](/analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-write-enabled-partitions)
+ [设置对单元数据的自定义访问权限](/analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services)

### <a name="long-running-mdx-queries-block-cube-processing"></a>长时间运行的 MDX 查询会阻止处理多维数据集

尽管 olapR 包仅执行读取操作，但是长时间运行的 MDX 查询可以创建锁，从而阻止处理多维数据集  。 始终预先测试 MDX 查询，以便了解应返回的数据量。

如果尝试连接到已锁定的多维数据集，则可能会收到错误，指出无法访问 SQL Server 数据仓库。 建议的解决方案包括启用远程连接、检查服务器或实例名称等；但是，请考虑预先打开连接的可能性。

SSAS 管理员可以通过标识和终止打开的会话来防止锁定问题。 在服务器级别，还可以将超时属性应用到 MDX 查询，以强制终止所有长时间运行的查询。

## <a name="resources"></a>资源

如果不熟悉 OLAP 或 MDX 查询，请参阅以下 Wikipedia 文章： 

+ [OLAP 多维数据集](https://en.wikipedia.org/wiki/OLAP_cube)
+ [MDX 查询](https://en.wikipedia.org/wiki/MultiDimensional_eXpressions)