---
title: 数据的性能优化
description: 本文将讨论在 SQL Server 中运行的 R 或 Python 脚本的性能优化。 还将介绍可以用来更新 R 代码的方法，这些方法既可以提高性能，又可以避免已知的问题。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 995b30e9c69b14148a67f18c36e0f42d01c419eb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470818"
---
# <a name="performance-tuning-and-data-optimization-for-r"></a>R 的性能优化和数据优化
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文将讨论在 SQL Server 中运行的 R 或 Python 脚本的性能优化。 你可以使用这些方法来更新 R 代码，这些方法既可以提高性能，又可以避免已知的问题。

## <a name="choosing-a-compute-context"></a>选择计算上下文

在 SQL Server 中，运行 R 或 Python 脚本时可以使用本地或 SQL 计算上下文。

使用本地计算上下文时，将在计算机而非服务器上执行分析。 因此，如果要从 SQL Server 获取数据以在代码中使用，则必须通过网络获取数据。 这种网络传输造成的性能下降取决于传输的数据大小、网络速度，以及同时发生的其他网络传输数量。

使用“SQL Server 计算上下文”时，将在服务器上执行该代码。 如果从 SQL Server 获取数据，则数据应该是运行分析的服务器的本地数据，因此不会引入任何网络开销。 如果需要从其他源导入数据，请考虑预先安排 ETL。

处理大型数据集时，始终应该使用 SQL 计算上下文。

## <a name="factors"></a>因素

R 语言具有因子的概念，它们是分类数据的特殊变量。 数据科学家经常在公式中使用因子变量，因为将分类变量作为因子处理可以确保机器学习函数正确处理数据。

通过设计，因子变量可以从字符串转换为整数，然后再转换回来进行存储或处理。 除非参数 stringsAsFactors 设置为“False”，否则 R `data.frame` 函数会将所有字符串作为因子变量处理。 这意味着字符串将自动转换为整数进行处理，然后映射回原始字符串。

如果因子的源数据存储为整数，性能可能会受到影响，因为 R 在运行时将因子整数转换为字符串，然后执行其内部的字符串到整数的转换。

若要避免这种运行时转换，可以考虑将值作为整数存储在 SQL Server 表中，并使用 colInfo 参数指定用作因子的列的级别。 RevoScaleR 中的大多数数据源对象都使用参数 colInfo。 使用此参数可以命名数据源使用的变量，指定其类型，并在列值上定义变量级别或转换。

例如，以下 R 函数调用从表中获取整数 1、2 和 3，但将值映射到级别为“apple”、“orange”和“banana”的因子。

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

当源列包含字符串时，使用 colInfo 参数提前指定级别始终更有效。 例如，下面的 R 代码在读取字符串时将其视为因子。

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

如果模型生成中不存在语义差异，则最后一种方法可能会提高性能。

## <a name="data-transformations"></a>数据转换

数据科研人员通常在分析过程中使用以 R 语言编写的转换函数。 转换函数应用于从表中检索的每个行。 在 SQL Server 中，此类转换应用于批处理中检索的所有行，这需要 R 解释器和分析引擎之间的通信。 执行转换时，数据将从 SQL 移到分析引擎，再移到 R 解释程序进程，然后转移回去。

出于此原因，根据涉及的数据量，使用转换作为 R 代码的一部分可能会给算法性能造成严重的负面影响。

更有效的做法是，在执行分析之前，将所有必要的列放在表或视图中，并避免在计算期间执行转换。 如果无法将更多的列添加到现有表中，可以考虑创建另一个表或视图来包含转换后的列，然后使用相应的查询来检索数据。

## <a name="batch-row-reads"></a>批处理行读取

如果在代码中使用 SQL Server 数据源 (`RxSqlServerData`)，建议尝试使用参数 rowsPerRead 指定批处理大小。 此参数定义查询的行数，然后发送到外部脚本进行处理。 在运行时，算法只看到每个批中指定的行数。

控制一次处理的数据量的能力可以帮助你解决或避免问题。 例如，如果输入数据集宽度非常宽（包含多个列），或者数据集有几个大型列（如自由文本），则可以减小批处理大小，以避免将数据分页出内存。

默认情况下，此参数的值设置为 50,000，确保即使在内存较低的计算机上，也有良好的性能。 如果服务器的可用内存足够高，将此值增大到 500,000 甚至 1,000,000 可以实现更高的性能，尤其是处理大型表时。

在大型数据集上和可以在多个进程上运行的任务中，增加批处理大小的好处显而易见。 但是，增加此值并不总是产生最佳结果。 建议对数据和算法进行试验，以确定最佳值。

## <a name="parallel-processing"></a>并行处理

若要提高“rx”分析函数的性能，可以利用 SQL Server 的能力，使用服务器计算机上可用的内核并行执行任务。

可通过两种方式使用 SQL Server 中的 R 实现并行化：

+ 使用 \@parallel。 使用 `sp_execute_external_script` 存储过程运行 R 脚本时，将 `@parallel` 参数设置为 `1`。 如果你的 R 脚本不使用具有其他处理机制的 RevoScaleR 函数，则这是最佳方法。 如果脚本使用 RevoScaleR 函数（通常以“rx”作为前缀），则会自动执行并行处理，并且不需要显式地将 `@parallel` 设置为 `1`。

  如果 R 脚本可以并行化，且 SQL 查询可以并行化，则数据库引擎将创建多个并行进程。 可创建的最大进程数等于实例的“最大并行度”(MAXDOP) 设置。 然后，所有进程运行相同的脚本，但只接收一部分数据。
  
  因此，此方法对于必须查看所有数据的脚本（例如，在定型模型时）而言不起作用。 但是，在并行执行批量预测等任务时，这种方式却很有作用。 有关结合 `sp_execute_external_script` 使用并行度的详细信息，请参阅[在 Transact-SQL 中使用 R 代码](../tutorials/quickstart-r-create-script.md)中的“高级技巧：并行处理”。

+ **使用 numTasks = 1。** 在 SQL Server 计算上下文中使用“rx”函数时，请将 numTasks 参数的值设置为要创建的进程数。 创建的进程数不能超过“MAXDOP”；但是，实际创建的进程数由数据库引擎确定，并且可能小于你请求的进程数。

  如果 R 脚本可并行化，并且 SQL 查询可并行化，则在运行 rx 函数时，SQL Server 将创建多个并行进程。 创建的实际进程数取决于多种因素。 这些因素包括资源监管、当前资源用量、其他会话，以及在 R 脚本中使用的查询的查询执行计划。

## <a name="query-parallelization"></a>查询并行性

在 Microsoft R 中，可以通过将数据定义为 RxSqlServerData 数据源对象来使用 SQL Server 数据源。

基于整个表或视图创建数据源：

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

基于 SQL 查询创建数据源：

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> 如果在数据源中指定了表而不是查询，R Services 使用内部试探法确定要从该表中提取的所需列；但是，这种方法不太可能会导致并行执行。

为了确保可以并行地分析数据，用于检索数据的查询应该以数据库引擎可以创建并行查询计划的方式构建。 如果代码或算法使用了大量数据，请确保针对 `RxSqlServerData` 的查询经过了并行执行优化。 无法生成并行执行计划的查询可能会导致计算单个进程。

如果需要处理大型数据集，请在运行 R 代码之前使用 Management Studio 或其他 SQL 查询分析器来分析执行计划。 然后，执行任何建议的步骤来提高查询的性能。 例如，表中的缺失某个索引可能会影响执行查询所要花费的时间。 有关详细信息，请参阅[监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)。

另一个可能会影响性能的常见错误是查询检索的列数超过了所需的列数。 例如，如果公式仅基于三列，但源表有 30 列，则无需移动数据。

+ 避免使用 `SELECT *`！
+ 花点时间查看数据集中的列，并仅识别分析所需的列
+ 从查询中删除任何包含与 R 代码不兼容的数据类型的列，如 GUID 和 rowguid
+ 检查不支持的日期和时间格式
+ 与其加载表，不如创建一个选择特定值或强制转换列的视图，以避免转换错误

## <a name="optimizing-the-machine-learning-algorithm"></a>优化机器学习算法

本部分提供 Microsoft R 中特定于 RevoScaleR 和其他选项的其他提示和资源。

> [!TIP]
> 关于 R 优化的一般讨论不在本文的讨论范围之内。 如果需要提高代码编写速度，建议参阅热门文章 [The R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf)。 这篇文章涵盖了 R 语言中的编程结构、生动语言和细节中的常见缺陷，并提供了许多 R 编程技术的具体示例。

### <a name="optimizations-for-revoscaler"></a>RevoScaleR 的优化

许多 RevoScaleR 算法都支持用于控制已定型模型生成方式的参数。 尽管模型的精确性和正确性非常重要，但算法的性能也同样重要。 若要在精确性和定型时间之间获得正确的平衡，可以通过修改参数来提高计算速度。在许多情况下，可以在不降低精确性和正确性的前提下提高性能。

+ [rxDTree](/r-server/r-reference/revoscaler/rxdtree)

  `rxDTree` 支持 `maxDepth` 参数，该参数控制决策树的深度。 由于增大 `maxDepth` 可能会降低性能，因此，必须分析增大深度带来的好处与降低性能的好处，这点非常重要。

  还可通过调整 `maxNumBins``maxDepth`、`maxComplete` 和 `maxSurrogate` 的参数来控制时间复杂性与预测准确性之间的平衡。 将深度增大到 10 或 15 以上可能会使计算开销变得极高。

+ [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)

  如果公式中的第一个因变量是因子变量，请尝试使用 `cube` 参数。
  
  如果 `cube` 设置为 `TRUE`，将使用分块求逆执行回归，与标准的回归计算相比，这可能会提高速度，减少内存用量。 如果公式中有大量的变量，性能提升可能非常明显。

+ [rxLogit](/r-server/r-reference/revoscaler/rxlogit)

  如果第一个因变量是因子变量，则使用 `cube` 参数。
  
  如果 `cube` 设置为 `TRUE`，该算法将使用分块求逆，这可能会提高速度，减少内存用量。 如果公式中有大量的变量，性能提升可能非常明显。

有关优化 RevoScaleR 的详细信息，请参阅以下文章：

+ 支持文章：[rxDForest 和 rxDTree 的性能优化选项](https://support.microsoft.com/kb/3104235)

+ 控制模型在提升树模型中的拟合方法：[使用随机梯度提升来估算模型](/r-server/r/how-to-revoscaler-boosting)

+ RevoScaleR 如何移动和处理数据的概述：[在 ScaleR 中编写自定义分块算法](/r-server/r/how-to-developer-write-chunking-algorithms)

+ RevoScaleR 的编程模型：[管理 RevoScaleR 中的线程](/r-server/r/how-to-developer-manage-threads)

+ [rxDForest](/r-server/r-reference/revoscaler/rxdforest) 的函数参考

+ [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees) 的函数参考

### <a name="use-microsoftml"></a>使用 MicrosoftML

另外，你也可查看新的“MicrosoftML”包，该包提供了可缩放的机器学习算法，这种算法可以使用 RevoScaleR 提供的计算上下文和转换。

+ [MicrosoftML 入门](/r-server/r/concept-what-is-the-microsoftml-package)

+ [如何选择 MicrosoftML 算法](/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

## <a name="next-steps"></a>后续步骤

+ 有关可用于提高 R 代码性能的 R 函数，请参阅[使用 R 代码分析函数提高性能](using-r-code-profiling-functions.md)。

+ 有关在 SQL Server 中进行性能优化的详细信息，请参阅 [SQL Server 数据库引擎和 Azure SQL 数据库的性能中心](/sql/relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database)。
