---
title: 数据科学解决方案模板
description: 本文介绍了演示最佳做法和提供构建基块以帮助实现机器学习解决方案的行业特定的模板。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/29/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: e7d7b36c2c19d48fec393e38c741244f6713dcd3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470628"
---
# <a name="data-science-scenarios-and-solution-templates"></a>数据科学方案和解决方案模板
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍了大量 SQL Server 机器学习解决方案模板。 这些模板演示最佳做法并提供构建基块，帮助快速实现机器学习解决方案。 每个模板都旨在解决特定行业的特定数据科学问题。
每个模板中的任务从数据准备和特征工程扩展到模型定型和计分。 

::: moniker range="=sql-server-2016"
使用这些模板了解 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 的工作原理。 然后，可以根据自己的方案自定义模板并构建自定义解决方案。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
使用这些模板了解 SQL Server 机器学习服务的工作方式。 然后，可以根据自己的方案自定义模板并构建自定义解决方案。
::: moniker-end

每个解决方案都包括示例数据、R 代码或 Python 代码，以及 SQL 存储过程（如果适用）。 代码可以在首选的 R 或 Python 开发环境中运行，计算在 SQL Server 中完成。 在某些情况下，可以使用 T-SQL 和任何 SQL 客户端工具（如 SQL Server Management Studio）直接运行代码。

> [!TIP]
> 
> 大多数模板都有多个版本，支持用于机器学习的本地和云平台。 例如，你可以仅使用 SQL Server 生成解决方案，也可以在 Microsoft R Server 或 Azure 机器学习中生成解决方案。

+ 有关下载和安装说明，请参阅 [如何使用模板](#bkmk_HowTo)。

## <a name="fraud-detection"></a>欺诈检测

[在线欺诈检测模板 (SQL Server R Services)](https://github.com/Microsoft/r-server-fraud-detection)

**内容：** 检测欺诈性交易的能力对在线企业非常重要。 为了降低退款损失，企业需要快速识别使用被盗付款方式或凭据进行的交易。 发现欺诈性交易后，企业通常会采取措施尽快地阻止某些帐户，以防止进一步损失。 在此方案中，将学习如何使用在线购买交易中的数据来识别可能的欺诈行为。

**方式：** 将欺诈检测作为二分类问题解决。 此模板中使用的方法可以轻松地应用于其他域中的欺诈检测。


## <a name="campaign-optimization"></a>市场活动优化

[预测与潜在客户联系的方式和时间](https://microsoft.github.io/r-server-campaign-optimization/)

**内容：** 此解决方案使用保险行业数据根据人口统计、历史响应数据和特定于产品的详细信息来预测潜在客户。  还会生成建议，以指示接触用户以影响购买行为的最佳渠道和时间。

**方式：** 该解决方案创建并比较了多个模型。 还演示了使用存储过程的自动化数据集成和数据准备。 在数据库、Azure 和 HDInsight Spark 中为 SQL Server 提供了并行示例。 

## <a name="health-care-predict-length-of-stay-in-hospital"></a>卫生保健：预测住院时长 

[预测住院时长](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1)

**内容：** 准确预测哪些患者可能需要长期住院治疗，这是护理和计划的重要组成部分。 管理员需要能够确定哪些设施需要更多资源，而医务人员需要确保他们能够满足患者的需求。

**方式：** 此解决方案使用 Data Science Virtual Machine，并包含启用机器学习的 SQL Server 实例。 还包括一组 Power-BI 报表，可用于与已部署的模型进行交互。

## <a name="customer-churn"></a>客户流失

[客户流失预测模板 (SQL Server R Services)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/Churn/README.md)

**内容：** 分析和预测客户流失对于任何必须管理和防止客户流失到竞争对手的行业都非常重要，如：银行业、电信业和零售业等等。 流失分析的目的是确认哪些客户可能会流失，然后采取适当措施维系这些客户及其业务。

**方式：** 此模板将流失问题描述为“二元分类”问题  。 它使用来自于两个源，客户人口统计和客户交易记录的示例数据，将客户分为“可能”和“不太可能”流失两类。
  
## <a name="predictive-maintenance"></a>预见性维护

[预测维护模板 (SQL Server 2016)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/PredictiveMaintenance/README.md)

**内容：** 预测维护旨在通过捕获以前的故障并将该信息用于预测设备可能会出现故障的时间和位置，以此来提高维护任务的效率。 对于依赖于分布式数据或传感器的应用程序，预测设备过时功能尤其有价值。 该方法也可用于物联网设备中的误差监测或预测。

**方式：** 此解决方案的重点在于回答“服务中的计算机会在何时出现故障？”这一问题 输入数据表示飞机引擎的模拟传感器测量值。 通过监视引擎的当前操作条件（例如当前工作周期、设置、传感器测量值）所获得的数据用于创建三种类型的预测模型：

-   **回归模型**，预测引擎在出现故障之前可持续的时间。 示例模型预测指标“剩余使用寿命”(RUL)，也称为“故障时间”(TTF)。
  
-   **分类模型**，预测引擎是否可能出现故障。
  
    “二类分类模型”预测在特定时间段内引擎是否会出现故障  。

    “多类分类模型”预测特定引擎是否会出现故障，以及如果出现故障，提供可能的故障时间窗口  。 例如，对于某个给定的日期，可以预测是否有任何设备可能会在那天出现故障，或者在其后某个时间段内出现故障。

## <a name="energy-demand-forecasting"></a>能量需求预测

[使用 SQL Server R Services 的能量需求预测模板](https://gallery.cortanaintelligence.com/Tutorial/Energy-Demand-Forecast-Template-with-SQL-Server-R-Services-1)

**内容：** ：需求预测是包括能源、零售和服务业在内的各个领域的一个重要问题。 准确的需求预测有助于公司进行更好的生产计划、资源配置并做出其他重要的业务决策。 在能源领域，需求预测对于降低储能成本、平衡供需至关重要。

**方式：** 此模板使用 SQL Server R Services 预测对电力的需求。 用于预测的模型是一个基于“rxDForest”的随机林回归模型，这是一个包含在 Microsoft R Server 中的高性能机器学习算法  。 解决方案包括一个需求模拟器、用于定型模型的所有 R 和 T-SQL 代码以及可用于生成和报告预测的存储过程。 


## <a name="how-to-use-the-templates"></a><a name="bkmk_HowTo"></a>如何使用模板

要下载每个模板中包含的文件，可以使用 GitHub 命令，或者可以打开链接，然后单击“下载 Zip 文件”  将所有文件都保存到计算机。  下载后，解决方案通常会包含这些文件夹：
  
-   **Data**：包含用于每个应用程序的示例数据。
  
-   **R**：包含解决方案所需的所有 R 开发代码。 解决方案需要 Microsoft R Server 提供的库，但可以在任何 R IDE 中打开和编辑。 通过将计算上下文设置为 SQL Server 实例，R 代码已经过优化，以便在“数据库内”执行计算。
  
-   **SQLR**：包含多个.sql 文件，可以在如 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 之类的 SQL 环境中运行这些文件，以创建执行相关任务（如数据处理、特征工程和模型部署）的存储过程。
  
    文件夹还包含 PowerShell 脚本，可以运行此脚本来调用所有脚本以及创建端到端环境。 请务必编辑脚本以适应环境。

## <a name="next-steps"></a>后续步骤

+ [Python 教程](./python-tutorials.md)
+ [R 教程](./r-tutorials.md)