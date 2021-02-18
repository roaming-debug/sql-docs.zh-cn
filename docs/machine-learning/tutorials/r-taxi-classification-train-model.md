---
title: R 教程：训练并保存模型
titleSuffix: SQL machine learning
description: 在此系列教程的第四部分（共五部分），你将在 SQL Server 上使用 Transact-SQL，采用 R 和 SQL 机器学习来训练并保存模型。
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 3b5606b9f14006632bfa2782d1d547b4565e36ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350932"
---
# <a name="r-tutorial-train-and-save-model"></a>R 教程：训练并保存模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

在此系列教程的第四部分（共五部分），你将学习如何使用 R 训练机器学习模型。使用在上一部分中创建的数据特征来训练该模型，然后将训练后的模型保存到一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。 在本例中，R 包已随 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 安装，因此可通过 SQL 完成所有操作。

在本文中，你将：

> [!div class="checklist"]
> + 使用 SQL 存储过程创建并训练模型
> + 将训练后的模型保存到 SQL 表中

在[第一部分](r-taxi-classification-introduction.md)中，你安装了必备条件并还原了示例数据库。

在[第二部分](r-taxi-classification-explore-data.md)中，你查看了示例数据，并生成一些绘图。

在[第三部分](r-taxi-classification-create-features.md)中，你学习了如何使用 Transact-SQL 函数根据原始数据创建特征。 然后从存储过程调用了该函数，创建了包含该功能值的表。

在[第五部分](r-taxi-classification-deploy-model.md)中，你将了解如何操作在第四部分中训练和保存的模型。

## <a name="create-the-stored-procedure"></a>创建存储过程

从 T-SQL 调用 R 时，可以使用系统存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。 但是，对于经常重复执行的过程（如重新训练模型），在其他存储过程中封装对 `sp_execute_external_script` 的调用则会更加方便。

1. 在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中，打开一个新的“查询”窗口。

2. 运行以下语句，创建存储过程 RTrainLogitModel。 此存储过程定义输入数据，并使用 glm 来创建逻辑回归模型。

   ```sql
   CREATE PROCEDURE [dbo].[RTrainLogitModel] (@trained_model varbinary(max) OUTPUT)
   
   AS
   BEGIN
     DECLARE @inquery nvarchar(max) = N'
       select tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance,
       pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
       from nyctaxi_sample
       tablesample (70 percent) repeatable (98052)
   '
   
     EXEC sp_execute_external_script @language = N'R',
                                     @script = N'
   ## Create model
   logitObj <- glm(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet, family = binomial)
   summary(logitObj)
   
   ## Serialize model 
   trained_model <- as.raw(serialize(logitObj, NULL));
   ',
     @input_data_1 = @inquery,
     @params = N'@trained_model varbinary(max) OUTPUT',
     @trained_model = @trained_model OUTPUT; 
   END
   GO
   ```

   + 为了确保留下一些数据以测试模型，70% 的数据是出于定型目的从出租车数据表中随机选择的。

   + SELECT 查询使用自定义标量函数 *fnCalculateDistance* 计算上车与下车位置之间的直接距离。 查询的结果存储在默认 R 输入变量 `InputDataset` 中。
  
   + R 脚本调用 R 函数 glm 来创建逻辑回归模型。
  
     二进制变量 _tipped_ 用作标签或结果列，模型使用以下这些特征列进行调整：_passenger_count_、_trip_distance_、_trip_time_in_secs_ 和 _direct_distance_。
  
   + 已定型模型（保存在 R 变量 `logitObj` 中）会进行序列化，并作为输出参数返回。

## <a name="train-and-deploy-the-r-model-using-the-stored-procedure"></a>使用存储过程定型并部署 R 模型

因为存储过程已包含输入数据的定义，所以无需提供输入查询。

1. 要定型和部署 R 模型，请调用存储过程，并将其插入到数据库表 nyc_taxi_models 中，以便可以将其用于未来的预测：

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC RTrainLogitModel @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('RTrainLogit_model', @model);
   ```

2. 查看 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的“消息”窗口，以查看通过管道传递到 R 的 stdout 流的消息，如下所示： 

   “来自外部脚本的 STDOUT 消息:读取的行数：1193025，已处理的总行数:1193025，总区块时间:0.093 秒”

3. 语句完成后，打开表 nyc_taxi_models。 数据处理和模型调整可能需要一些时间。

   可以看到已添加了一个新行，该行在列 model 中包含序列化模型，在列 name 中包含模型名称 RTrainLogit_model。

   ```text
   model                        name
   ---------------------------- ------------------
   0x580A00000002000302020....  RTrainLogit_model
   ```

在此教程的下一部分中，你将使用已训练模型来生成预测。

## <a name="next-steps"></a>后续步骤

本文内容：

> [!div class="checklist"]
> + 使用 SQL 存储过程创建并训练了模型
> + 将训练后的模型保存到了 SQL 表中

> [!div class="nextstepaction"]
> [R 教程：在 SQL 存储过程中运行预测](r-taxi-classification-deploy-model.md)
