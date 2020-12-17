---
title: 使用 R 创建 SSIS 和 SSRS 工作流
description: 结合了 SQL Server 机器学习服务和 R 服务、Reporting Services (SSRS) 和 SQL Server Integration Services (SSIS) 的集成方案。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/28/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 12088a5d5a8413f76176da09e2f8aa94e72e63f0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470908"
---
# <a name="create-ssis-and-ssrs-workflows-with-r-on-sql-server"></a>在 SQL Server 上使用 R 创建 SSIS 和 SSRS 工作流
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍如何结合 SQL Server 两项重要功能和 SQL Server 机器学习服务的语言和数据科学功能来使用嵌入式 R 和 Python 脚本：SQL Server Integration Services (SSIS) 和 SQL Server Reporting Services SSRS。 SQL Server 中的 R 和 Python 库提供了统计和预测功能。 SSIS 和 SSRS 分别提供协调的 ETL 转换和可视化效果。 本文介绍如何在此工作流模式中将所有这些功能组合在一起：

> [!div class="checklist"]
> * 创建包含 R 或 Python 可执行文件的存储过程
> * 从 SSIS 或 SSRS 中执行存储过程

本文中的示例主要介绍 R 和 SSIS，但相应概念和步骤同样适用于 Python。 第二部分提供了有关 SSRS 可视化效果的指南和链接。

<a name="bkmk_ssis"></a> 

## <a name="use-ssis-for-automation"></a>使用 SSIS 实现自动化

数据科学工作流具有高迭代性，并涉及许多数据转换操作，包括缩放、聚合、概率的计算，以及属性的重命名和合并。 数据科学家习惯于使用 R、Python 或其他语言执行其中许多任务，但对企业数据执行此类工作流时需要与 ETL 工具和过程无缝集成。

由于 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 支持在 R 中通过 Transact-SQL 和存储过程运行复杂的操作，因此，你可以将数据科学任务与现有的 ETL 流程集成。 可以使用非常高效的工具（包括 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[tsql](../../includes/tsql-md.md)]）来优化数据准备，而无需要执行一连串内存密集型任务。 

下面是一些关于如何使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 自动化数据处理和管道建模的建议：

+ 从本地或云数据源提取数据来构建培训数据 
+ 在数据集成工作流过程中构建和运行 R 或 Python 模型
+ （按计划地）定期对模型进行再培训
+ 将 R 或 Python 脚本的结果加载到其他目标，如 Excel、Power BI、Oracle 和 Teradata
+ 通过执行 SSIS 任务在 SQL 数据库中创建数据功能
+ 使用条件分支切换 R 和 Python 作业的计算上下文

## <a name="ssis-example"></a>SSIS 示例

以下示例来自 Jimmy Wong 在一个现已停用的 MSDN 博客上撰写的一篇文章：`https://blogs.msdn.microsoft.com/ssis/2016/01/11/operationalize-your-machine-learning-project-using-sql-server-2016-ssis-and-r-services/`

本示例演示如何使用 SSIS 实现任务自动化。 使用 SQL Server Management Studio 创建含有嵌入式 R 的存储过程，然后通过 SSIS 包中的[执行 T-SQL 任务](../../integration-services/control-flow/execute-t-sql-statement-task.md)执行这些存储过程。

要逐步完成此示例，你应熟悉 Management Studio、SSIS、SSIS 设计器、包设计和 T-SQL。 SSIS 包使用三个[执行 T-SQL 任务](../../integration-services/control-flow/execute-t-sql-statement-task.md)，这些任务将培训数据插入到表中，为数据建模，并对数据进行评分以获取预测输出。

### <a name="load-training-data"></a>加载培训数据

在 SQL Server Management Studio 中运行以下脚本，创建用于存储数据的表。 在本练习中，你应创建并使用测试数据库。 

```T-SQL
Use test-db
GO

Create table ssis_iris (
    id int not null identity primary key
    , "Sepal.Length" float not null, "Sepal.Width" float not null
    , "Petal.Length" float not null, "Petal.Width" float not null
    , "Species" varchar(100) null
);
GO
```

创建将培训数据加载到数据帧的存储过程。 本示例使用内置 Iris 数据集。 

```T-SQL
Create procedure load_iris
as
begin
    execute sp_execute_external_script
        @language = N'R'
        , @script = N'iris_df <- iris;'
        , @input_data_1 = N''
        , @output_data_1_name = N'iris_df'
    with result sets (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100)));
end;
```

在 SSIS 设计器中，创建一个用于执行先前定义的存储过程的[执行 SQL 任务](../../integration-services/control-flow/execute-sql-task.md)。 SQLStatement 的脚本删除现有数据，指定要插入的数据，然后调用存储过程来提供数据  。

```T-SQL
truncate table ssis_iris;
insert into ssis_iris("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species")
exec dbo.load_iris;
```

![插入数据](../media/create-workflows-using-r-in-sql-server/ssis-exec-sql-insert-data.png "插入数据")

### <a name="generate-a-model"></a>生成模型

在 SQL Server Management Studio 中运行以下脚本，创建存储模型的表。 

```T-SQL
Use test-db
GO

Create table ssis_iris_models (
    model_name varchar(30) not null default('default model') primary key,
    model varbinary(max) not null
);
GO
```

创建一个通过 [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod) 生成线性模型的存储过程。 RevoScaleR 和 revoscalepy 库在 SQL Server 的 R 和 Python 会话中自动可用，因此无需导入该库。

```T-SQL
Create procedure generate_iris_rx_model
as
begin
    execute sp_execute_external_script
        @language = N'R'
        , @script = N'
          irisLinMod <- rxLinMod(Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width + Species, data = ssis_iris);
          trained_model <- data.frame(payload = as.raw(serialize(irisLinMod, connection=NULL)));'
        , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from ssis_iris'
        , @input_data_1_name = N'ssis_iris'
        , @output_data_1_name = N'trained_model'
    with result sets ((model varbinary(max)));
end;
GO
```

在 SSIS 设计器中，创建一个[执行 SQL 任务](../../integration-services/control-flow/execute-sql-task.md)来执行 generate_iris_rx_model  存储过程。 模型被序列化并保存到 ssis_iris_models 表中。 SQLStatement  脚本如下所示：

```T-SQL
insert into ssis_iris_models (model)
exec generate_iris_rx_model;
update ssis_iris_models set model_name = 'rxLinMod' where model_name = 'default model';
```

![生成线性模型](../media/create-workflows-using-r-in-sql-server/ssis-exec-rxlinmod.png "生成线性模型")

在此任务完成后，作为检查点，你可以查询 ssis_iris_models ，查看其是否包含一个二进制模型。

### <a name="predict-score-outcomes-using-the-trained-model"></a>使用“培训”模型预测（评分）结果

现在已有用于加载培训数据和生成模型的代码，剩下的唯一步骤是使用此模型生成预测。 

为此，请将 R 脚本放入 SQL 查询中，以触发 ssis_iris_model 上的 [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) 内置 R 函数。 名为 predict_species_length 的存储过程将完成此任务  。

```T-SQL
Create procedure predict_species_length (@model varchar(100))
as
begin
    declare @rx_model varbinary(max) = (select model from ssis_iris_models where model_name = @model);
    -- Predict based on the specified model:
    exec sp_execute_external_script
        @language = N'R'
        , @script = N'
irismodel <-unserialize(rx_model);
irispred <- rxPredict(irismodel, ssis_iris[,2:6]);
OutputDataSet <- cbind(ssis_iris[1], irispred$Sepal.Length_Pred, ssis_iris[2]);
colnames(OutputDataSet) <- c("id", "Sepal.Length.Actual", "Sepal.Length.Expected");
#OutputDataSet <- subset(OutputDataSet, Species.Length.Actual != Species.Expected);
'
    , @input_data_1 = N'
    select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from ssis_iris
    '
    , @input_data_1_name = N'ssis_iris'
    , @params = N'@rx_model varbinary(max)'
    , @rx_model = @rx_model
    with result sets ( ("id" int, "Species.Length.Actual" float, "Species.Length.Expected" float)
        );
end;
```

在 SSIS 设计器中，创建一个[执行 SQL 任务](../../integration-services/control-flow/execute-sql-task.md)，用于执行 predict_species_length 存储过程来生成预测的花瓣长度  。

```T-SQL
exec predict_species_length 'rxLinMod';
```

![生成预测](../media/create-workflows-using-r-in-sql-server/ssis-exec-predictions.png "生成预测")

### <a name="run-the-solution"></a>运行解决方案

在 SSIS 设计器中，按 F5 来执行此包。 应会看到与以下屏幕截图类似的结果。

![按 F5 以在调试模式下运行](../media/create-workflows-using-r-in-sql-server/ssis-exec-F5-run.png "按 F5 以在调试模式下运行")

<a name="bkmk_ssrs"></a> 

## <a name="use-ssrs-for-visualizations"></a>使用 SSRS 实现可视化

虽然 R 可以创建图表和有趣的可视化效果，但其与外部数据源集成度不佳，这意味着每个图表或图形必须单独生成。 共享可能也比较困难。

借助 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]，你可以在 R 中通过 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程运行复杂的操作，这些操作可通过各种企业报表工具（包括 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 Power BI）轻松处理。

## <a name="next-steps"></a>后续步骤

本文中的 SSIS 和 SSRS 示例演示了执行包含嵌入式 R 或 Python 脚本的存储过程的两种情况。 关键点在于，你可以将 R 或 Python 脚本提供给任何可以对存储过程发送执行请求的应用程序或工具。 SSIS 的另一个关键点在于，你可以创建用于自动执行和计划各种操作的包，例如数据采集、清理、操作，并且这些操作中包括了 R 或 Python 数据科学功能。 如需了解更多信息和做法，请参阅[在 SQL Server 机器学习服务中使用存储过程操作 R 代码](operationalizing-your-r-code.md)。