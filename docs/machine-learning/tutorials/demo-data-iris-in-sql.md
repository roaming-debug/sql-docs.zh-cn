---
description: 用于 SQL 机器学习 Python 和 R 教程的 Iris 演示数据
title: 用于教程的 Iris 演示数据集
titleSuffix: SQL machine learning
Description: 创建一个包含 Iris 数据集的数据库和多个预测模型。 此数据集在关于 SQL 机器学习的 R 和 Python 教程中使用。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 76e746701aa331cf1720ea56befd5956fd65cfd4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470458"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>用于 SQL 机器学习 Python 和 R 教程的 Iris 演示数据
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

在此练习中，创建一个数据库，用于存储来自 [Iris 花卉数据集](https://en.wikipedia.org/wiki/Iris_flower_data_set)的数据以及基于相同数据的模型。 Iris 数据包含在 R 和 Python 发行版中，并用于 SQL 机器学习的机器学习教程。

若要完成此练习，应具有 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 或其他可以运行 T-SQL 查询的工具。

使用此数据集的教程和快速入门包括以下内容：

+ [快速入门：在 Python 中创建预测模型并对其进行评分](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>创建数据库

1. 启动 SQL Server Management Studio，并打开一个新的“查询”窗口  。  

2. 为此项目创建一个新数据库，并更改“查询”窗口的上下文以使用该新数据库  。

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. 添加一些空表：一个用于存储数据，另一个用于存储已定型的模型。 “iris_models”表用于存储在其他练习中生成的序列化模型。

    以下代码创建用于定型数据的表。

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. 运行以下代码以创建用于存储定型模型的表。 若要将 Python（或 R）模型保存在 SQL Server 中，必须将它们序列化并存储在 varbinary(max) 类型的列中。

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    除模型内容之外，通常还会为其他有用的元数据添加列，例如模型的名称、定型日期、源算法和参数、源数据等等。 但现在，为简便起见，我们将只使用模型名称。

## <a name="populate-the-table"></a>填充表

可以从 R 或 Python 获取内置 Iris 数据。 可以使用 Python 或 R 将数据加载到数据帧中，然后将其插入数据库中的表中。 将定型数据从外部会话移动到一个表中包含以下几个步骤：

+ 设计一个用于获取所需数据的存储过程。
+ 执行该存储过程以实际获取数据。
+ 构造 INSERT 语句，以指定在何处保存检索到的数据。

1. 在具有 Python 集成的系统上，创建以下使用 Python 代码加载数据的存储过程。

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    运行此代码时，应收到消息“命令已成功完成。” 这意味着存储过程已根据你的规范创建。

2. 或者，在具有 R 集成的系统上，创建一个使用 R 的过程。

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. 若要实际填充表，请运行存储过程并指定应在其中写入数据的表。 运行时，存储过程执行 Python 或 R 代码，该代码加载内置 Iris 数据集，然后将数据插入“iris_data”表。

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    如果不熟悉 T-SQL，请注意，INSERT 语句只添加新的数据；它不会检查现有的数据，也不会删除和重新生成表。 若要避免在表中获取相同数据的多个副本，可以先运行以下语句：`TRUNCATE TABLE iris_data`。 T-SQL [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) 语句可删除现有数据，但保持表结构完整。

## <a name="query-the-data"></a>查询数据

作为验证步骤，运行查询以确认已上传数据。

1. 在“对象资源管理器”中的“数据库”下，右键单击“irissql”数据库，然后启动一个新查询。

2. 运行一些简单的查询：

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>后续步骤

在下面的快速入门中，你将创建一个机器学习模型并将其保存到表中，然后使用该模型生成预测结果。

+ [快速入门：在 Python 中创建预测模型并对其进行评分](quickstart-python-train-score-model.md)