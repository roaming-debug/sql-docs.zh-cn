---
title: 使用 sp_rxPredict 进行实时评分
description: 了解如何在 SQL Server 中使用 sp_rxPredict 系统存储过程执行实时评分，从而在预测工作负载时实现高性能的预测或评分。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/31/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 19f9f1c6cfc293bfba0d44e34e0a30bf386bdb3d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470958"
---
# <a name="real-time-scoring-with-sp_rxpredict-in-sql-server"></a>在 SQL Server 中使用 sp_rxPredict 进行实时评分
[!INCLUDE[sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

了解如何在 SQL Server 中使用 [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) 系统存储过程执行实时评分，从而在预测工作负载时实现高性能的预测或评分。

`sp_rxPredict` 的实时评分与语言无关，执行时不依赖于[机器学习服务](../sql-server-machine-learning-services.md)或 [R 服务](../r/sql-server-r-services.md)中的 R 或 Python 运行时。 假设在 SQL Server 中使用 Microsoft 函数创建和定型了一个模型，然后将它序列化为二进制格式，则可以使用实时评分在没有安装 R 或 Python 加载项的 SQL Server 实例上生成新数据输入的预测结果。

## <a name="how-real-time-scoring-works"></a>实时评分的工作原理

支持对基于 R 中的 [RevoScaleR](../r/ref-r-revoscaler.md) 或 [MicrosoftML](../r/ref-r-microsoftml.md)，或 Python 中的 [revoscalepy](../python/ref-py-revoscalepy.md) 或 [microsoftml](../python/ref-py-microsoftml.md) 中的函数的特定模型类型进行实时评分。 它根据提供给存储为特殊二进制格式的机器学习模型的用户输入来使用本机 C++ 库生成评分。

因为无需调用 [Microsoft 机器学习服务](../sql-server-machine-learning-services.md)或 [R 服务](../r/sql-server-r-services.md)中的外部语言运行时即可将已训练的模型用于评分，所以减少了多个进程的开销。 

实时评分是一个多步骤流程：

1. 必须基于每个数据库启用执行评分的存储过程。
2. 以二进制格式加载已预定型的模型。
3. 提供要评分的新输入数据（表格或各行）作为模型的输入。
4. 若要生成分数，请调用 [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) 存储过程。

## <a name="prerequisites"></a>先决条件

+ [启用 SQL Server CLR 集成](../../relational-databases/clr-integration/clr-integration-enabling.md)。

+ [启用实时评分](#bkmk_enableRtScoring)。

+ 必须使用下面列出的某个受支持的 rx 算法预定型模型  。 对于 R，具有 `sp_rxPredict` 的实时评分适用于 [RevoScaleR 和 MicrosoftML 支持的算法](#bkmk_rt_supported_algos)。 对于 Python，请参阅 [revoscalepy 和 microsoftml 支持的算法](#bkmk_py_supported_algos)。

+ 使用适用于 R 的 [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) 和适用于 Python 的 [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) 对模型进行序列化。 已对这些序列化函数进行了优化，以支持快速评分。

+ 将模型保存到要从中调用该模型的数据库引擎实例。 此实例不需要具有 R 或 Python 运行时扩展。

> [!Note]
> 实时评分目前已经过优化，可快速预测较小的数据集，从几行到数十万行的数据不等。 对于大数据集，使用 [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) 进行预测的速度可能会更快。

<a name ="bkmk_enableRtScoring"></a> 

## <a name="enable-real-time-scoring"></a>启用实时评分

必须对要用于评分的每个数据库启用此功能。 服务器管理员应运行包含在 RevoScaleR 包中的命令行实用工具 RegisterRExt.exe。

> [!NOTE]
> 若要进行实时评分，需要在实例中启用 SQL CLR 功能；此外，需要将数据库标记为可信。 运行脚本时，系统将为你执行这些操作。 但是，在此之前，请考虑其他的安全隐患！

1. 打开提升的命令提示符，并导航到 RegisterRExt.exe 所在的文件夹。 在默认安装中可以使用以下路径：

    `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\`

2. 运行以下命令，替换要从中启用扩展存储过程的实例和目标数据库的名称：

    `RegisterRExt.exe /installRts [/instance:name] /database:databasename`

    例如，若要将扩展存储过程添加到默认实例上的 CLRPredict 数据库中，请键入：

    `RegisterRExt.exe /installRts /database:CLRPRedict`

    如果数据库在默认实例上，实例名称则是可选的。 如果使用的是命名实例，则必须指定实例名称。

3. RegisterRExt.exe 创建以下对象：

    + 受信任的程序集
    + 存储过程 `sp_rxPredict`
    + 新数据库角色 `rxpredict_users`。 数据库管理员可以使用此角色向使用实时评分功能的用户授予权限。

4. 将需要运行 `sp_rxPredict` 的任何用户添加到新角色中。

> [!NOTE]
>
> SQL Server 2017 及更高版本中提供了其他安全措施来防止出现 CLR 集成问题。 这些措施还对此存储过程的使用施加了其他限制。

## <a name="disable-real-time-scoring"></a>禁用实时评分

若要禁用实时评分功能，请打开提升的命令提示符，并运行以下命令：`RegisterRExt.exe /uninstallrts /database:<database_name> [/instance:name]`

<a name="bkmk_py_supported_algos"></a>

## <a name="supported-algorithms"></a>支持的算法

### <a name="python-algorithms-using-real-time-scoring"></a>使用实时评分的 Python 算法

+ revoscalepy 模型

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) \*
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) \*
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) \*
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) \*
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) \*
  
  标记有 \* 的模型还支持使用 PREDICT 函数的本机评分。

+ microsoftml 模型

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_net](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

+ microsoftml 提供的转换

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/featurize-text)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)

<a name="bkmk_rt_supported_algos"></a>

### <a name="r-algorithms-using-real-time-scoring"></a>使用实时评分的 R 算法

+ RevoScaleR 模型

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod) \*
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit) \*
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees) \*
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree) \*
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest) \*
  
  标记有 \* 的模型还支持使用 PREDICT 函数的本机评分。

+ MicrosoftML 模型

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

+ MicrosoftML 提供的转换

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

### <a name="unsupported-model-types"></a>不支持的模型类型

实时评分不使用解释器；因此，在评分步骤中，不支持任何可能需要解释器的功能。  这些情况可能包括：

+ 不支持使用 `rxGlm` 或 `rxNaiveBayes` 算法的模型。

+ 使用转换函数或包含转换的公式（例如 `A ~ log(B`）的模型不支持实时评分。 若要使用此类型的模型，建议在将数据传递到实时评分前对输入数据执行转换。

## <a name="example"></a>示例

本部分介绍准备和保存实时预测模型所需的步骤，并提供有关如何使用 R 从 T-SQL 调用函数的示例。

### <a name="step-1-prepare-and-save-the-model"></a>步骤 1。 准备并保存模型

sp\_rxPredict 所需的二进制格式与使用 PREDICT 函数所需的格式相同。 因此，将对 [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) 的一个调用包含在你的 R 代码中，并确保指定 `realtimeScoringOnly = TRUE`，如本示例所示：

```R
model <- rxSerializeModel(model.name, realtimeScoringOnly = TRUE)
```

### <a name="step-2-call-sp_rxpredict"></a>步骤 2. 调用 sp_rxPredict

像调用任何其他存储过程一样调用 `sp_rxPredict`。 在当前版本中，此存储过程仅采用两个参数： _\@model_，用于二进制格式的模型；以及 _\@inputData_，用于评分中要使用的数据，定义为有效的 SQL 查询。

由于二进制格式与 PREDICT 函数使用的格式相同，因此可以使用先前示例中的模型和数据表。

```sql
DECLARE @irismodel varbinary(max)
SELECT @irismodel = [native_model_object] from [ml_models]
WHERE model_name = 'iris.dtree' 
AND model_version = 'v1''

EXEC sp_rxPredict
@model = @irismodel,
@inputData = N'SELECT * FROM iris_rx_data'
```

> [!NOTE]
> 如果用于评分的输入数据不包含符合模型要求的列，对 `sp_rxPredict` 的调用则会失败。 目前仅支持以下 .NET 数据类型：double、float、short、ushort、long、ulong 和 string。
>
> 因此，你可能需要在输入数据中筛选掉不受支持的类型，然后再将它用于实时评分。
>
> 有关相应的 SQL 类型的信息，请参阅 [SQL-CLR 类型映射](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping)或[映射 CLR 参数数据](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)。

## <a name="next-steps"></a>后续步骤

+ [结合利用 SQL 机器学习和 PREDICT T-SQL 函数的本机评分功能](native-scoring-predict-transact-sql.md)
+ [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md)
+ [SQL 机器学习](../index.yml)