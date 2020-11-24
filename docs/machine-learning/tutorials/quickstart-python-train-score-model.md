---
title: 快速入门：在 Python 中定型模型
titleSuffix: SQL machine learning
description: 在本快速入门中，你将使用 Python 创建并训练预测模型。 将此模型保存到数据库的表中，然后通过 SQL 机器学习使用此模型来通过新数据预测值。
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 6913f80cb96942d7063e4c61caf2c2109ee79a48
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94869975"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-python-with-sql-machine-learning"></a>快速入门：通过 SQL 机器学习在 Python 中创建预测模型并对其进行评分
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

在本快速入门中，你将使用 Python 创建并训练预测模型。 将此模型保存到 SQL Server 实例中的表，然后在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)、[Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)或 [SQL Server 大数据群集](../../big-data-cluster/machine-learning-services.md)上使用此模型基于新数据来预测值。

你将创建并执行 SQL 中运行的两个存储过程。 第一个存储过程使用经典 Iris 花卉数据集，并生成 Naïve Bayes 模型，用于根据花卉特征预测 Iris 种类。 第二个存储过程用于评分，它调用第一个过程中生成的模型，从而根据新数据输出一组预测。 通过将 Python 代码用于 SQL 存储过程，操作会包含在 SQL 中，可重复使用，并且可以由其他存储过程和客户端应用程序进行调用。

完成本快速入门后，你将了解以下内容：

> [!div class="checklist"]
> - 如何在存储过程中嵌入 Python 代码
> - 如何通过存储过程上的输入将输入传递给你的代码
> - 如何将存储过程用于操作模型

## <a name="prerequisites"></a>先决条件

若要运行本快速入门，需要具备以下先决条件。

- 以下平台之一上的 SQL 数据库：
  - [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)。 如需安装，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。
  - SQL Server 大数据群集。 了解如何[在 SQL Server 大数据群集上启用机器学习服务](../../big-data-cluster/machine-learning-services.md)。
  - Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

- 用于运行包含 Python 脚本的 SQL 查询的工具。 本快速入门使用 [Azure Data Studio](../../azure-data-studio/what-is.md)。

- 本练习所用的示例数据是 Iris 示例数据。 按照 [Iris 演示数据](demo-data-iris-in-sql.md)中的说明创建示例数据库 irissql。

## <a name="create-a-stored-procedure-that-generates-models"></a>创建用于生成模型的存储过程

在本步骤中，将创建一个用于生成模型的存储过程，以预测结果。

1. 打开 Azure Data Studio，连接到 SQL 实例，然后打开一个新的查询窗口。

1. 连接到 irissql 数据库。

    ```sql
    USE irissql
    GO
    ```

1. 复制以下代码，以创建新的存储过程。

   在运行时，此过程将调用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 以启动 Python 会话。 

   Python 代码所需的输入将作为输入参数在此存储过程中进行传递。 输出将是基于适用于机器学习算法的 Python scikit-learn 库的定型模型。

   此代码使用 [pickle](https://docs.python.org/2/library/pickle.html) 来序列化模型。 将使用 iris_data 表的 0 到 4 列中的数据定型该模型。 

   该过程的第二部分中所示的参数阐明了数据输入和模型输出。 你希望存储过程中运行的 Python 代码尽可能具有明确定义的输入和输出，这些输入和输出到在运行时传入的存储过程输入和输出。

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model VARBINARY(max) OUTPUT)
    AS
    BEGIN
        EXECUTE sp_execute_external_script @language = N'Python'
            , @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]], iris_data[["SpeciesId"]].values.ravel()))
    '
            , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
            , @input_data_1_name = N'iris_data'
            , @params = N'@trained_model varbinary(max) OUTPUT'
            , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

1. 验证存储过程是否存在。 

   如果上一步骤中的 T-SQL 脚本运行时未出现错误，则会创建名为 generate_iris_model 的新存储过程，并将其添加到 irissql 数据库 。 可在 Azure Data Studio“对象资源管理器”中的“可编程性”下方找到存储过程 。

## <a name="execute-the-procedure-to-create-and-train-models"></a>执行过程以创建和定型模型

在本步骤中，需要执行过程以运行嵌入代码，并创建已定型和已序列化的模型作为输出。 

存储在数据库中以供重复使用的模型将序列化为字节流，并存储在数据库表中的 VARBINARY(MAX) 列。 创建、定型、序列化模型并将其保存到数据库之后，在对工作负载评分时可通过其他过程或 [PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md) 函数调用该模型。

1. 运行以下脚本以执行过程。 用于运行存储过程的特定语句是第四行的 `EXECUTE`。

   此特定脚本会删除相同名称（“Naive Bayes”）的现有模型，从而为通过重新运行同一过程而创建的新模型腾出空间。 如果不删除模型，则会出现“对象已存在”错误。 该模型存储在名为 iris_models 的表中，并在 irissql 数据库创建时预配 。

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes'
    EXECUTE generate_iris_model @model OUTPUT;
    DELETE iris_models WHERE model_name = @new_model_name;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    GO
    ```

1. 验证是否已插入该模型。

    ```sql
    SELECT * FROM dbo.iris_models
    ```

    **结果**

    | model_name  | 模型 |
    |---|-----------------|
    | Naive Bayes | 0x800363736B6C6561726E2E6E616976655F62617965730A… | 

## <a name="create-and-execute-a-stored-procedure-for-generating-predictions"></a>创建并执行存储过程以生成预测

现已创建、定型并保存了模型，接下来继续进行下一步：创建存储过程以生成预测。 为此，将调用 `sp_execute_external_script` 来运行 Python 脚本，该脚本加载序列化模型并提供要进行评分的新数据输入。

1. 运行以下代码以创建执行评分的存储过程。 在运行时，此过程将加载二进制模型，使用 `[1,2,3,4]` 列作为输入，并指定 `[0,5,6]` 列作为输出。

   ```sql
   CREATE PROCEDURE predict_species (@model VARCHAR(100))
   AS
   BEGIN
       DECLARE @nb_model VARBINARY(max) = (
               SELECT model
               FROM iris_models
               WHERE model_name = @model
               );
   
       EXECUTE sp_execute_external_script @language = N'Python'
           , @script = N'
   import pickle
   irismodel = pickle.loads(nb_model)
   species_pred = irismodel.predict(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]])
   iris_data["PredictedSpecies"] = species_pred
   OutputDataSet = iris_data[["id","SpeciesId","PredictedSpecies"]] 
   print(OutputDataSet)
   '
           , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
           , @input_data_1_name = N'iris_data'
           , @params = N'@nb_model varbinary(max)'
           , @nb_model = @nb_model
       WITH RESULT SETS((
                   "id" INT
                 , "SpeciesId" INT
                 , "SpeciesId.Predicted" INT
                   ));
   END;
   GO
   ```

2. 执行存储过程并提供模型名称“Naive Bayes”，以便该过程知道使用哪个模型。

   ```sql
   EXECUTE predict_species 'Naive Bayes';
   GO
   ```

   运行存储过程时，它将返回 Python data.frame。 此行 T-SQL 指定返回结果的架构：`WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`。 可以将结果插入新表，也可以将其返回应用程序。

   ![运行存储过程的结果集](media/train-score-using-python-NB-model-results.png)

   这些结果是关于种类的 150 种预测，使用花卉特征作为输入。 对大多数观察结果而言，预测种类与实际种类匹配。

   通过使用 Python iris 数据集进行定型和评分，使本示例简单易懂。 更为典型的方法是运行 SQL 查询以获取新数据，并将其作为 `InputDataSet` 传递到 Python。

## <a name="conclusion"></a>结束语

在本练习中，你了解了如何创建专用于不同任务的存储过程，其中每个存储过程都使用了系统存储过程 `sp_execute_external_script` 来启动 Python 进程。 Python 进程的输入作为参数传递到 `sp_execute_external`。 Python 脚本本身和数据库中的数据变量都作为输入进行传递。

通常，只应计划将 Azure Data Studio 与经过优化的 Python 代码结合使用，或与返回基于行的输出的简单 Python 代码结合使用。 Azure Data Studio 作为工具支持 T-SQL 之类的查询语言并返回平展行集。 如果代码生成散点图或直方图等可视化输出，则需要可在存储过程之外呈现图像的独立工具或最终用户应用程序。

对于一些习惯了编写完全包含型脚本（可处理各种操作）的 Python 开发人员而言，可能没必要将任务组织到独立的过程中。 但定型和评分的用例不同。 可通过分离任务，将每个任务置于不同的计划，并限定每个操作的权限范围。

最后一个优点是可以使用参数修改进程。 在本练习中，创建了模型（本示例中名为“Naive Bayes”）的 Python 代码作为输入传递到另一个存储过程，该过程在评分过程中调用该模型。 本练习仅使用一个模型，但可以想象到，在评分任务中将模型参数化会使该脚本更为有用。

## <a name="next-steps"></a>后续步骤

若要详细了解通过 SQL 机器学习使用 Python 的教程，请参阅：

- [Python 教程](python-tutorials.md)