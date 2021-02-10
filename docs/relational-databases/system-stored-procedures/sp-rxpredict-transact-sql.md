---
title: sp_rxPredict |Microsoft Docs
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 3bca21ddf2674fd8eccbe9ca2df635623e56201e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063635"
---
# <a name="sp_rxpredict"></a>sp_rxPredict  
[!INCLUDE [SQL Server 2016 Windows only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]

为给定输入生成预测值，该输入包含以二进制格式存储在 SQL Server 数据库中的机器学习模型。

以近乎实时的速度对 R 和 Python 机器学习模型进行评分。 `sp_rxPredict`作为 `rxPredict` [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)和[MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)中 R 函数的包装提供的存储过程，以及[revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package)和[MicrosoftML](/machine-learning-server/python-reference/microsoftml/microsoftml-package)中的[rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) Python 函数。 它是用 c + + 编写的，专门针对计分操作进行了优化。

尽管必须使用 R 或 Python 来创建模型，但在目标数据库引擎实例上以二进制格式对其进行序列化并存储后，即使未安装 R 或 Python 集成，也可以从该数据库引擎实例中使用该模型。 有关详细信息，请参阅 [sp_rxPredict 的实时评分](../../machine-learning/predictions/real-time-scoring.md)。

## <a name="syntax"></a>语法

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>参数

**model**

支持格式的预先训练模型。 

**input**

有效的 SQL 查询

### <a name="return-values"></a>返回值

返回分数列以及输入数据源中的任何传递列。
如果算法支持生成此类值，则可以返回其他分数列，如置信区间。

## <a name="remarks"></a>备注

若要启用存储过程，必须在实例上启用 SQLCLR。

> [!NOTE]
> 启用此选项有安全隐患。 如果无法在服务器上启用 SQLCLR，请使用其他实现，如 [TRANSACT-SQL 预测](../../t-sql/queries/predict-transact-sql.md?view=sql-server-2017&preserve-view=true) 函数。

用户需要 `EXECUTE` 对数据库的权限。

### <a name="supported-algorithms"></a>支持的算法

若要创建和训练模型，请使用支持的 R 或 Python 算法之一， [SQL Server 机器学习服务 (r 或 python) ](../../machine-learning/sql-server-machine-learning-services.md)， [SQL Server 2016 R 服务](../../machine-learning/r/sql-server-r-services.md)， [SQL Server Machine Learning Server (独立)  (R 或 Python) ](../../machine-learning/r/r-server-standalone.md)，或 [SQL Server 2016 R Server (独立) ](../../machine-learning/r/r-server-standalone.md?view=sql-server-2016&preserve-view=true)。

#### <a name="r-revoscaler-models"></a>R： RevoScaleR 模型

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree)
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest)

#### <a name="r-microsoftml-models"></a>R： MicrosoftML 模型

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

#### <a name="r-transformations-supplied-by-microsoftml"></a>R： MicrosoftML 提供的转换

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

#### <a name="python-revoscalepy-models"></a>Python： revoscalepy 模型

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit)
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees)
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree)
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest)


#### <a name="python-microsoftml-models"></a>Python： microsoftml 模型

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

#### <a name="python-transformations-supplied-by-microsoftml"></a>Python： microsoftml 提供的转换

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)
  
### <a name="unsupported-model-types"></a>不支持的模型类型

以下模型类型不受支持：

+ `rxGlm` `rxNaiveBayes` 在 RevoScaleR 中使用或算法的模型
+ R 中的 PMML 模型
+ 使用其他第三方库创建的模型 

## <a name="examples"></a>示例

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

除了是有效的 SQL 查询外， *\@ inputData* 中的输入数据还必须包含与存储模型中的列兼容的列。

`sp_rxPredict` 仅支持以下 .NET 列类型： double、float、short、ushort、long、ulong 和 string。 你可能需要在输入数据中筛选掉不受支持的类型，然后将其用于实时计分。 

  有关相应的 SQL 类型的信息，请参阅 [SQL-CLR 类型映射](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping)或[映射 CLR 参数数据](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)。
