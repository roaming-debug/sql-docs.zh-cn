---
title: 安装预先定型的模型
description: 向 SQL Server 机器学习服务（R 或 Python）或 SQL Server R Services 添加预先定型的模型，这些模型用于情绪分析和图像特征化。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/30/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 8402ec9094b7e765764d5e650ac27a0b87b0e5d2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471118"
---
# <a name="install-pre-trained-machine-learning-models-on-sql-server"></a>在 SQL Server 上安装预先定型的机器学习模型
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍如何使用 PowerShell 将用于情绪分析和图像特征化的免费预训练机器学习模型添加到具有 R 或 Python 集成的 SQL Server 实例 。 预先定型的模型由 Microsoft 生成并且可供使用，作为安装后任务添加到实例中。 有关这些模型的详细信息，请参阅本文的[资源](#bkmk_resources)部分。

安装完成后，预先定型的模型将被视为一种实现细节，它支持 MicrosoftML (R) 和 MicrosoftML (Python) 库中的特定功能。 不应（并且不能）查看、自定义或重新定型模型，也不能在自定义代码或成对的其他函数中将它们视为独立的资源。 

若要使用预先定型的模型，请调用下表中列出的函数。

| R 函数 (MicrosoftML) | Python 函数 (microsoftml) | 使用情况 |
|--------------------------|-------------------------------|-------|
| [getSentiment](/machine-learning-server/r-reference/microsoftml/getsentiment) | [get_sentiment](/machine-learning-server/python-reference/microsoftml/get-sentiment) | 对文本输入生成正负情绪分数。 |
| [featurizeImage](/machine-learning-server/r-reference/microsoftml/featurizeimage) | [featurize_image](/machine-learning-server/python-reference/microsoftml/featurize-image) | 从图像文件输入中提取文本信息。 |

## <a name="prerequisites"></a>先决条件

机器学习算法是计算密集型的。 对于低等到中等的工作负荷，包括使用所有示例数据来完成教程演练，我们建议使用 16 GB RAM。

必须具有计算机和 SQL Server 的管理员权限，才能添加预先定型的模型。

必须启用外部脚本，并且必须运行 SQL Server LaunchPad 服务。 安装说明提供了启用和验证这些功能的步骤。 

::: moniker range=">=sql-server-2017"
[MicrosoftML R 包](/machine-learning-server/r-reference/microsoftml/microsoftml-package)或 [microsoftml Python 包](/machine-learning-server/python-reference/microsoftml/microsoftml-package)包含预先定型的模型。

[SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)包含机器学习库的两种语言版本，因此无需执行任何其他操作即可满足此必备条件。 由于存在这些库，所以能使用本文中所述的 PowerShell 脚本将预先定型的模型添加到这些库。
::: moniker-end

::: moniker range="=sql-server-2016"
[MicrosoftML R 包](/machine-learning-server/r-reference/microsoftml/microsoftml-package)包含预先定型的模型。

[SQL Server R Services](sql-r-services-windows-install.md)（仅 R）不包含现成的 [MicrosoftML 包](/machine-learning-server/r-reference/microsoftml/microsoftml-package)。 若要添加 MicrosoftML，必须执行[组件升级](../install/upgrade-r-and-python.md)。 组件升级的一个优点是可以同时添加预先定型的模型，所以无需运行 PowerShell 脚本。 不过如果已经升级，但第一次没有添加预先定型的模型，则可以按照本文所述的内容运行 PowerShell 脚本。 SQL Server 的两个版本都适用。 在执行此操作之前，请确认 MicrosoftML 库存在于 `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`。
::: moniker-end

<a name="file-location"></a>

## <a name="check-whether-pre-trained-models-are-installed"></a>检查是否安装了预先定型的模型

R 和 Python 模型的安装路径如下所示：

+ R 模型：`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\MicrosoftML\mxLibs\x64`

+ Python 模型：`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\microsoftml\mxLibs`

下面列出的是模型文件名：

+ AlexNet\_Updated.model
+ ImageNet1K\_mean.xml
+ pretrained.model
+ ResNet\_101\_Updated.model
+ ResNet\_18\_Updated.model
+ ResNet\_50\_Updated.model

如果已安装这些模型，请跳到[验证步骤](#verify)以确认可用性。

## <a name="download-the-installation-script"></a>下载安装脚本

单击 [https://aka.ms/mlm4sql](https://aka.ms/mlm4sql) 下载文件 Install-MLModels.ps1  。

## <a name="execute-with-elevated-privileges"></a>使用提升的权限执行

1. 启动 PowerShell。 在任务栏上，右键单击 PowerShell 程序图标，然后选择“以管理员身份运行”  。
2. 输入安装脚本文件的完全限定路径，并且包含实例名称。 这里假定为“下载”文件夹和默认实例，该命令可能如下所示：

   ```powershell
   PS C:\WINDOWS\system32> C:\Users\<user-name>\Downloads\Install-MLModels.ps1 MSSQLSERVER
   ```

**输出**

在连接 Internet 的 SQL Server 机器学习服务默认实例（带有 R 和 Python）上，应会看到类似于以下内容的消息。

   ```powershell
   MSSQL14.MSSQLSERVER
        Verifying R models [9.2.0.24]
        Downloading R models [C:\Users\<user-name>\AppData\Local\Temp]
        Installing R models [C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\]
        Verifying Python models [9.2.0.24]
        Installing Python models [C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\]
   PS C:\WINDOWS\system32>
   ```

<a name="verify"> </a>

## <a name="verify-installation"></a>验证安装

首先，检查 [mxlibs 文件夹](#file-location)中的新文件。 接下来，运行演示代码以确认模型已安装且正常工作。 

### <a name="r-verification-steps"></a>R 验证步骤

1. 启动 C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64 处的 RGUI.EXE  。

2. 在命令提示符处粘贴以下 R 脚本。

    ```R
    # Create the data
    CustomerReviews <- data.frame(Review = c(
    "I really did not like the taste of it",
    "It was surprisingly quite good!",
    "I will never ever ever go to that place again!!"),
    stringsAsFactors = FALSE)

    # Get the sentiment scores
    sentimentScores <- rxFeaturize(data = CustomerReviews, 
                                    mlTransforms = getSentiment(vars = list(SentimentScore = "Review")))

    # Let's translate the score to something more meaningful
    sentimentScores$PredictedRating <- ifelse(sentimentScores$SentimentScore > 0.6, 
                                            "AWESOMENESS", "BLAH")

    # Let's look at the results
    sentimentScores
    ```

3. 按 Enter 查看情绪分数。 输出应如下所示：

    ```R
    > sentimentScores
                                            Review SentimentScore
    1           I really did not like the taste of it      0.4617899
    2                 It was surprisingly quite good!      0.9601924
    3 I will never ever ever go to that place again!!      0.3103435
    PredictedRating
    1            BLAH
    2     AWESOMENESS
    3            BLAH
    ```

### <a name="python-verification-steps"></a>Python 验证步骤

1. 启动 C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES 处的 Python.exe  。

2. 在命令提示符处粘贴以下 Python 脚本

    ```python
    import numpy
    import pandas
    from microsoftml import rx_logistic_regression, rx_featurize, rx_predict, get_sentiment

    # Create the data
    customer_reviews = pandas.DataFrame(data=dict(review=[
                "I really did not like the taste of it",
                "It was surprisingly quite good!",
                "I will never ever ever go to that place again!!"]))
                
    # Get the sentiment scores
    sentiment_scores = rx_featurize(
        data=customer_reviews,
        ml_transforms=[get_sentiment(cols=dict(scores="review"))])
        
    # Let's translate the score to something more meaningful
    sentiment_scores["eval"] = sentiment_scores.scores.apply(
                lambda score: "AWESOMENESS" if score > 0.6 else "BLAH")
    print(sentiment_scores)
    ```

3. 按 Enter 打印分数。 输出应如下所示：

    ```python
    >>> print(sentiment_scores)
                                                review    scores         eval
    0            I really did not like the taste of it  0.461790         BLAH
    1                  It was surprisingly quite good!  0.960192  AWESOMENESS
    2  I will never ever ever go to that place again!!  0.310344         BLAH
    >>>
    ```

> [!NOTE]
> 如果演示脚本失败，请首先检查文件位置。 在具有多个 SQL Server 实例的系统上，或者对于与独立版本并行运行的实例，安装脚本可能会错误地读取环境，并将文件放在错误的位置。 一般情况下，手动将文件复制到正确的 mxlib 文件夹可以解决此问题。

## <a name="examples-using-pre-trained-models"></a>使用预先定型的模型的示例

下面的链接包含调用预先定型的模型的示例代码。

+ [代码示例：使用文本特征化器进行情绪分析](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Samples/101/BinaryClassification/SimpleSentimentAnalysis)

<a name="bkmk_resources"></a> 

## <a name="research-and-resources"></a>研究和资源

目前可用的模型是用于情绪分析和图像分类的深度神经网络 (DNN) 模型。 所有预先定型的模型都通过 Microsoft 的 [Computation Network Toolkit](https://cntk.ai/Features/Index.html)（计算网络工具包）或 **CNTK** 进行定型。

每个网络的配置都基于以下引用实现：

+ ResNet-18
+ ResNet-50
+ ResNet-101
+ AlexNet

有关这些深度学习模型中使用的算法的详细信息，以及如何使用 CNTK 将其实现和定型的详细信息，请参阅以下文章：

+ [Microsoft Researchers' Algorithm Sets ImageNet Challenge Milestone](https://www.microsoft.com/research/blog/microsoft-researchers-algorithm-sets-imagenet-challenge-milestone/)（Microsoft 研究人员的算法成为 ImageNet 挑战中的里程碑）

+ [Microsoft Computational Network Toolkit offers most efficient distributed deep learning computational performance](https://www.microsoft.com/research/blog/microsoft-computational-network-toolkit-offers-most-efficient-distributed-deep-learning-computational-performance/)（Microsoft 计算网络工具包提供最有效的分布式深度学习计算性能）

## <a name="see-also"></a>另请参阅

+ [SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)
+ [升级 SQL Server 实例中的 R 和 Python 组件](../install/upgrade-r-and-python.md)
+ [适用于 R 的 MicrosoftML 包](/machine-learning-server/r-reference/microsoftml/microsoftml-package)
+ [适用于 Python 的 microsoftml 包](/machine-learning-server/python-reference/microsoftml/microsoftml-package)
