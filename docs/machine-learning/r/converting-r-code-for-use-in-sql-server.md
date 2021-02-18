---
title: 转换 R 代码使之能在 SQL 中运行
description: 将 R 代码迁移到 SQL Server 存储过程中，以实现解决方案部署和对 SQL Server 上关系数据的访问。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0b0d3e451c3b95bfa2401f80176dbdb659f1cb86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336282"
---
# <a name="convert-r-code-for-execution-in-sql-server-in-database-instances"></a>转换 R 代码以在 SQL Server（数据库内）实例中执行
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文提供了有关如何修改 R 代码以使其可在 SQL Server 中运行的高级指南。 

将 R 代码从 R Studio 或其他环境移到 SQL Server 时，大多数情况下代码不需要进一步修改即可运行，比如说代码很简单时（例如要求某些输入并返回值的函数）。 此外，移植使用 RevoScaleR 或 MicrosoftML 包的解决方案也更容易，这些包支持在不同的执行上下文中运行（只需稍加修改）   。

但是，如果以下任一条件成立，则你可能需要对代码进行大量更改：

+ 使用访问网络或无法在 SQL Server 上安装的 R 库。
+ 代码对 SQL Server 之外的数据源进行单独调用，如 Excel 工作表、共享位置上的文件和其他数据库。 
+ 要运行 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的 \@script 参数中的代码，并同时参数化存储过程  。
+ 最初的解决方案包含多个在生产环境单独执行时效率可能更高的步骤，如数据准备、特征工程与模型定型、评分或报告。
+ 想要通过更改库、使用并行执行或将某些处理任务卸载到 SQL Server 来优化性能。 

## <a name="step-1-plan-requirements-and-resources"></a>步骤 1。 规划要求和资源

### <a name="packages"></a>包

+ 确定所需的包，并确保它们可用于 SQL Server 上。
 
+ 预先在机器学习服务使用的默认包库中安装包。 用户库不受支持。

### <a name="data-sources"></a>数据源

+ 如果要在 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中嵌入 R 代码，则标识主数据源和辅助数据源。 

  + 主数据源是大型数据集，例如模型定型数据或用于预测的输入数据  。 计划将最大的数据集映射到 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的输入参数。

  + 辅助数据源通常是较小的数据集，例如系数列表或其他分组变量  。 
  
  目前，sp_execute_external_script 仅支持单个数据集作为存储过程的输入。 但是，可以添加多个标量或二进制输入。

  前面带有 EXECUTE 的存储过程调用不能用作 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的输入。 你可以使用查询、视图或任何其他有效的 SELECT 语句。

+ 确定所需的输出。 如果使用 sp_execute_external_script 运行 R 代码，存储过程只能输出一个数据帧作为结果。 但是，也可以输出多个标量输出，包括二进制格式的绘图和模型，以及从 R 代码或 SQL 参数得到的其他标量值。

### <a name="data-types"></a>数据类型

+ 针对可能的数据类型问题创建一份查检表。

  SQL Server 机器学习服务支持所有 R 数据类型。 但是，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的数据类型比 R 更广泛。所以，在将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据发送到 R 时，会执行某些隐式数据类型转换，反之亦然。 可能需要显式强制转换或转换某些数据。 

  支持 NULL 值。 但是，R 使用 `na` 数据构造来表示缺失值（类似于 null 值）。

+ 考虑消除对 R 不能使用的数据的依赖：例如，SQL Server 的 rowid 和 GUID 数据类型不能由 R 使用，会生成错误。

  有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

## <a name="step-2-convert-or-repackage-code"></a>步骤 2. 转换或重新打包代码

更改代码的程度取决于你是要从远程客户端提交 R 代码以在 SQL Server 计算上下文中运行，还是要将代码部署为存储过程的一部分以获得更好的性能和数据安全性。 将代码包装在存储过程中会有一些额外的要求。 

+ 为避免数据移动，应尽可能将主输入数据定义为 SQL 查询。

+ 在存储过程中运行 R 时，可以传递多个标量输入  。 对于要在输出中使用的任何参数，请添加 OUTPUT 关键字  。 

  例如，下面的标量输入 `@model_name` 包含模型名称，在结果中该模型名称也是该列自身中的输出：

  ```sql
  EXEC sp_execute_external_script @model_name="DefaultModel" OUTPUT, @language=N'R', @script=N'R code here'
  ```

+ 作为 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 存储过程的参数传入的任何变量必须映射到 R 代码中的变量。 默认情况下，变量按名称映射。

  输入数据集中的所有列必须也映射到 R 脚本中的变量。  例如，假设 R 脚本包含如下所示的公式：

  ```R
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  ```
  
  如果输入数据集不包含具有匹配名称 ArrDelay、CRSDepTime、DayOfWeek、CRSDepHour 和 DayOfWeek 的列，则会引发错误。

+ 在某些情况下，必须提前为结果定义输出架构。

  例如，若要将数据插入到表中，则必须使用 WITH RESULT SET 子句来指定架构  。

  如果 R 脚本使用 `@parallel=1` 参数，则还需要输出架构。 原因是 SQL Server 可能会创建多个进程来并行运行查询，最后收集结果。 因此，必须先准备输出架构，然后才能创建并行进程。
  
  在其他情况下，你可以通过使用“WITH RESULT SETS UNDEFINED”选项来省略结果架构  。 此语句从 R 脚本返回数据集，不需命名列或指定 SQL 数据类型。

+ 考虑使用 T-SQL（而不使用 R）生成计时或跟踪数据。

  例如，你可以通过添加传递给结果的 T-SQL 调用来传递用于审核和存储的系统时间或其他信息，而不是在 R 脚本中生成类似的数据。 

### <a name="improve-performance-and-security"></a>提高性能和安全性

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ 避免将预测或中间结果写入文件。 改为将预测写入表，以避免数据移动。
::: moniker-end

+ 提前运行所有查询，并检查 SQL Server 查询计划以确定可以并行执行的任务。

  如果输入查询可并行化，请将 `@parallel=1` 作为参数的一部分设置为 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。 

  只要 SQL Server 可以处理分区表或者在多个进程之间分布查询并最终聚合结果，就通常可以使用此标志进行并行处理。 如果用于训练模型的算法需要读取所有数据，或者你需要创建聚合，则通常无法使用此标志进行并行处理。

+ 检查 R 代码，确定是否可以独立执行某些步骤，或者可以使用单独的存储过程调用来更有效地执行某些步骤。 例如，通过分别执行特征工程或特征提取，并将值保存到表中，可以获得更好的性能。

+ 尝试使用 T-SQL 而不是使用 R 代码进行基于集的计算的方法。

  ::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
  例如，该 R 解决方案显示用户定义的 T-SQL 函数和 R 如何执行相同的特征工程任务：[数据科学端到端演练](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)。
  ::: moniker-end

+ 如有可能，请将传统的 R 函数替换为支持分布式执行的 ScaleR 函数  。 有关详细信息，请参阅 [Comparison of Base R and Scale R Functions](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r)（比较 Base R 函数和 Scale R 函数）。

+ 咨询数据库开发人员，确定通过 SQL Server 功能提高性能的方式，例如[内存优化表](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md) 或 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)（如果你拥有 Enterprise Edition 的话）。

## <a name="step-3-prepare-for-deployment"></a>步骤 3. 准备部署

+ 通知管理员，以便在部署代码之前可以安装和测试包。 

  在开发环境中，也许可以将包安装为代码的一部分，但在生产环境中，这是一种不好的做法。 

  无论是使用存储过程，还是在 SQL Server 计算上下文中运行 R 代码，都不支持用户库。

### <a name="package-your-r-code-in-a-stored-procedure"></a>在存储过程中打包 R 代码

+ 如果代码相对简单，可以将它嵌入用户定义的 T-SQL 函数中，而无需修改，如此示例中所述：

  + [使用 T-SQL 和 R 的特征工程](../tutorials/r-taxi-classification-create-features.md)

+ 如果代码相对复杂，则使用 R 包 sqlrutils 来转换代码  。 该包旨在帮助有经验的 R 用户编写优秀的存储过程代码。 

  第一步是将 R 代码重写为具有明确定义的输入和输出的单个函数。

  然后，使用 sqlrutils 包生成格式正确的输入和输出  。 sqlrutils 包会生成完整的存储过程代码，还可以在数据库中注册该存储过程  。 

  有关详细信息和示例，请参阅 [sqlrutils (SQL)](ref-r-sqlrutils.md)。

### <a name="integrate-with-other-workflows"></a>与其他工作流集成

+ 利用 T-SQL 工具和 ETL 进程。 作为数据工作流的一部分，请预先执行特征工程、特征提取和数据清理。

  使用专用的 R 开发环境（如 [!INCLUDE[rsql_rtvs_md](../../includes/rsql-rtvs-md.md)] 或 RStudio）时，你可能会将数据提取到计算机，以迭代方式分析数据，然后写出或显示结果。 
  
  但是，如果将独立的 R 代码迁移到了 SQL Server，则此过程的大部分步骤可以简化或委派给其他 SQL Server 工具。 

+ 使用安全的异步可视化策略。

  SQL Server 用户通常无法访问服务器上的文件，且 SQL 客户端工具通常不支持 R 图形设备。 如果解决方案的一部分是生成绘图或其他图形，则考虑将绘图导出为二进制数据并将其保存到表或写入。

+ 在存储过程中包装预测和评分函数，供应用程序直接访问。

## <a name="next-steps"></a>后续步骤

若要查看如何在 SQL Server 中部署 R 解决方案的示例，请参阅以下示例：

+ [教程：通过 SQL 机器学习在 R 中开发预测模型](../tutorials/r-predictive-model-introduction.md)

+ [R 教程：使用二元分类来预测纽约市出租车费用](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [端到端数据科学解决方案](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)包含了对使用 R 和 T-SQL 的功能工程的比较
::: moniker-end
