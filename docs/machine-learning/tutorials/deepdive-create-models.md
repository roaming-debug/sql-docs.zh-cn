---
title: 使用 RevoScaleR 创建 R 模型
description: 了解如何创建线性回归模型来分析在上一教程中所丰富的数据。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 34b9aa0d591d4f2a1e2f0a017a7f86612b14d7a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470588"
---
# <a name="create-r-models-sql-server-and-revoscaler-tutorial"></a>创建 R 模型（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 7 个教程。

你已经丰富了定型数据。 在本教程中，将使用回归建模来分析数据。 线性模型是预测分析领域中的一种重要工具。 RevoScaleR 包包含可以对工作负荷进行细分以及并行运行的回归算法  。

> [!div class="checklist"]
> * 创建线性回归模型
> * 创建逻辑回归模型

## <a name="create-a-linear-regression-model"></a>创建线性回归模型

在此步骤中，需要创建一个简单的线性模型，此模型通过将 gender 和 creditLine 列中的值用作独立变量来估计客户的信用卡余额   。
  
为此，请使用 [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod) 函数，它支持远程计算上下文。
  
1. 创建一个 R 变量来存储已完成的模型，然后调用 rxLinMod，从而传递相应的公式  。
  
    ```R
    linModObj <- rxLinMod(balance ~ gender + creditLine,  data = sqlFraudDS)
    ```
  
2. 若要查看结果的摘要，请对模型对象调用标准 R summary 函数  。
  
     ```R
     summary(linModObj)
     ```

你可能会很奇怪一个普通的 R 函数（如 summary  ）会在此起作用，因为在上一步中，你将计算上下文设置为服务器。 但是，即使 **rxLinMod** 函数使用远程计算上下文创建模型，它也会返回包含本地工作站模型的对象并将其存储在共享目录中。

因此，你可以假定该模型是使用“本地”上下文创建的，然后对其运行标准 R 命令。

**结果**

```R
Linear Regression Results for: balance ~ gender + creditLineData: sqlFraudDS (RxSqlServerData Data Source)
Dependent variable(s): balance
Total independent variables: 4 (Including number dropped: 1)
Number of valid observations: 10000
Number of missing observations: 0
Coefficients: (1 not defined because of singularities)

Estimate Std. Error t value Pr(>|t|) (Intercept)
3253.575 71.194 45.700 2.22e-16
gender=Male -88.813 78.360 -1.133 0.257
gender=Female Dropped Dropped Dropped Dropped
creditLine 95.379 3.862 24.694 2.22e-16
Signif. codes: 0  0.001  0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 3812 on 9997 degrees of freedom
Multiple R-squared: 0.05765
Adjusted R-squared: 0.05746
F-statistic: 305.8 on 2 and 9997 DF, p-value: < 2.2e-16
Condition number: 1.0184
```

## <a name="create-a-logistic-regression-model"></a>创建逻辑回归模型

接着，创建一个逻辑回归模型，它指示特定客户是否存在欺诈风险。 你将使用 RevoScaleR  [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit) 函数，它支持在远程计算上下文中调整逻辑回归模型。

将计算上下文保持为原样。 你还将继续使用相同的数据源。

1. 调用 rxLogit  函数并传递需用于定义模型的公式。

    ```R
    logitObj <- rxLogit(fraudRisk ~ state + gender + cardholder + balance + numTrans + numIntlTrans + creditLine, data = sqlFraudDS, dropFirst = TRUE)
    ```
  
    因为它是大型模型，其中包含 60 个独立变量（包括三个删除的虚拟变量），所以可能必须等待一段时间，让计算上下文返回对象。
    
    模型如此大的原因在于，在 R 中（以及在 **RevoScaleR** 包中），分类系数变量的每个级别都自动被视为单独的虚拟变量。
  
2. 若要查看返回模型的摘要，请调用 summary  函数。
  
    ```R
    summary(logitObj)
    ```
  
**部分结果**

```R
Logistic Regression Results for: fraudRisk ~ state + gender + cardholder + balance + numTrans + numIntlTrans + creditLine
Data: sqlFraudDS (RxSqlServerData Data Source)
Dependent variable(s): fraudRisk
Total independent variables: 60 (Including number dropped: 3)
Number of valid observations: 10000 -2

LogLikelihood: 2032.8699 (Residual deviance on 9943 degrees of freedom)

Coefficients:
Estimate Std. Error z value Pr(>|z|)     (Intercept)
-8.627e+00  1.319e+00  -6.538 6.22e-11
state=AK                Dropped    Dropped Dropped  Dropped
state=AL             -1.043e+00  1.383e+00  -0.754   0.4511

(other states omitted)

gender=Male             Dropped    Dropped Dropped  Dropped
gender=Female         7.226e-01  1.217e-01   5.936 2.92e-09
cardholder=Principal    Dropped    Dropped Dropped  Dropped
cardholder=Secondary  5.635e-01  3.403e-01   1.656   0.0977
balance               3.962e-04  1.564e-05  25.335 2.22e-16
numTrans              4.950e-02  2.202e-03  22.477 2.22e-16
numIntlTrans          3.414e-02  5.318e-03   6.420 1.36e-10
creditLine            1.042e-01  4.705e-03  22.153 2.22e-16

Signif. codes:  0 '\*\*\*' 0.001 '\*\*' 0.01 '\*' 0.05 '.' 0.1 ' ' 1
Condition number of final variance-covariance matrix: 3997.308
Number of iterations: 15
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [对新数据进行评分](../../machine-learning/tutorials/deepdive-score-new-data.md)