---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 83205b4a11be46888f8c7da8f29c84494d012740
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460850"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

基于存储模型生成预测值或评分。 有关详细信息，请参阅[使用 PREDICT T-SQL 函数本机计分](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)。

## <a name="syntax"></a>语法

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>参数

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
`MODEL` 参数用于指定用于评分或预测的模型。 将模型指定为变量或文字或标量表达式。

`PREDICT` 支持使用 [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) 和 [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) 包训练的模型。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
`MODEL` 参数用于指定用于评分或预测的模型。 将模型指定为变量或文字或标量表达式。

在 Azure SQL 托管实例中，`PREDICT` 支持 [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) 格式的模型，或使用 [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) 和 [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) 包训练的模型。
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
`MODEL` 参数用于指定用于评分或预测的模型。 将模型指定为变量或文字或标量表达式或标量子查询。

在 Azure Synapse Analytics 中，`PREDICT` 支持 [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) 格式的模型。
::: moniker-end

**数据**

DATA 参数用于指定用于评分或预测的数据。 在查询中以表源的形式指定数据。 表源可以是表、表别名、CTE 别名、视图或表值函数.

**RUNTIME = ONNX**

> [!IMPORTANT]
> `RUNTIME = ONNX` 参数仅在 [Azure SQL 托管实例](/azure/azure-sql/managed-instance/machine-learning-services-overview)、[Azure SQL Edge](/azure/sql-database-edge/onnx-overview) 和 [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is) 中可用。

指示用于执行模型的机器学习引擎。 `RUNTIME` 参数值始终为 `ONNX`。 对于 Azure SQL Edge 和 Azure Synapse Analytics，此参数是必需的。 在 Azure SQL 托管实例上，此参数是可选的，仅在使用 ONNX 模型时使用。

WITH ( <result_set_definition> )

WITH 子句用于指定 `PREDICT` 函数返回的输出的架构。

除了 `PREDICT` 函数本身返回的列之外，所有在数据输入中包含的列都可以在查询中使用。

### <a name="return-values"></a>返回值

没有可用的预定义架构；不验证模型的内容，也不验证返回的列值。

- `PREDICT` 函数作为输入通过列传递。
- `PREDICT` 函数还生成新列，但列数以及其数据类型取决于用于预测的模型类型。

任何与数据、模型或列格式相关的错误消息都由与模型关联的基础预测函数返回。

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>备注

Windows 和 Linux 上的所有版本的 SQL Server 2017 或更高版本都支持 `PREDICT` 功能。 无需启用[机器学习服务](../../machine-learning/sql-server-machine-learning-services.md)即可使用 `PREDICT`。
::: moniker-end

### <a name="supported-algorithms"></a>支持的算法

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
使用的模型必须是使用 [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) 或 [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) 包中支持的算法之一创建的。 若要查看当前支持的模型列表，请参阅[使用 PREDICT T-SQL 函数本机评分](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)。
::: moniker-end
::: moniker range="=azure-sqldw-latest"
支持可转换为 [ONNX](https://onnx.ai/) 模型格式的算法。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
支持可转换为 [ONNX](https://onnx.ai/) 模型格式的算法，以及使用 [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) 或 [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) 包中支持的算法之一创建的模型。 若要查看 RevoScaleR 和 revoscalepy 中当前支持的算法的列表，请参阅[使用 PREDICT T-SQL 函数本机评分](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)。
::: moniker-end

### <a name="permissions"></a>权限

`PREDICT` 不需要任何权限；但是，用户需要数据库 `EXECUTE` 权限，以及查询用作输入的任何数据的权限。 如果模型已存储在表中，则用户还须能够从表中读取模型。

## <a name="examples"></a>示例

以下示例展示了调用 `PREDICT` 的语法。

### <a name="using-predict-in-a-from-clause"></a>在 FROM 子句中使用 PREDICT

此示例引用 `SELECT` 语句的 `FROM` 子句中的 `PREDICT` 函数：

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

`DATA` 参数中为表源指定的别名 d 用于引用属于 `dbo.mytable` 的列。 为 `PREDICT` 函数指定的别名 p 用于引用 `PREDICT` 函数返回的列。

- 模型存储为“模型”表中的 `varbinary(max)` 列。 ID 和说明等其他信息保存在表中以标识模式 。
- `DATA` 参数中为表源指定的别名 d 用于引用属于 `dbo.mytable` 的列。 输入数据列的名称应与模型的输入名称匹配。
- 为 `PREDICT` 函数指定的别名 p 用于引用 `PREDICT` 函数返回的预测列。 列名应与模型的输出名称相同。
- 所有输入数据列和预测列都可显示在 SELECT 语句中。

::: moniker range=">=azure-sqldw-latest"

可以重写前面的示例查询，通过将 `MODEL` 指定为标量子查询来创建视图：

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>将 PREDICT 与 INSERT 语句相结合

一个常见的预测用例是生成输入数据的评分，然后将预测值插入到表中。 下面的示例假定，应用程序调用操作会使用存储过程将包含预测值的行插入到表中：

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- `PREDICT` 的结果存储在名为 PredictionResults 的表中。 
- 模型存储为“模型”表中的 `varbinary(max)` 列。 ID 和说明等其他信息可保存在表中以标识模式。
- 为 `DATA` 参数中的表源指定的别名 d 用于引用 `dbo.mytable` 中的列。输入数据列的名称应与模型的输入名称匹配。
- 为 `PREDICT` 函数指定的别名 p 用于引用 `PREDICT` 函数返回的预测列。 列名应与模型的输出名称相同。
- 所有输入列和预测列都可显示在 SELECT 语句中。

## <a name="next-steps"></a>后续步骤

- [使用 PREDICT T-SQL 函数本机计分](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [了解有关 ONNX 模型的详细信息](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end
