---
title: 什么是独立的 Machine Learning Server 或 R Server？
description: 了解 SQL Server 安装程序中独立 R Server 和 Machine Learning Server 至今的区别。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/13/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 67cdc278387de1faef999aa67543ec73c021bc41
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489737"
---
# <a name="what-are-standalone-machine-learning-server-or-r-server-in-sql-server"></a>什么是 SQL Server 中独立的 Machine Learning Server 或 R Server？
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

SQL Server 为独立于 SQL Server 运行的独立 R Server 或 Machine Learning Server 提供安装支持。 根据 SQL Server 版本，独立服务器具有开放源代码 R 基础（可能是 Python），它们与 Microsoft 提供的具有大规模统计和预测分析功能的高性能库重叠。 库还能启用通过 R 或 Python 编写脚本的机器学习任务。 

在 SQL Server 2016 中，此功能称为 R Server（独立版），且仅用于 R  。 在 SQL Server 2017 中，它称为 Machine Learning Server（独立版），且包括 R 和 Python  。  

> [!Note]
> 独立服务器由 SQL Server 安装程序进行安装，其在功能上等同于 [Microsoft Machine Learning Server](/machine-learning-server/what-is-machine-learning-server) 的非 SQL 品牌版本，支持相同的用户方案，包括远程执行、操作化和 Web 服务，以及 R 库和 Python 库的完整集合。

## <a name="components"></a>组件

SQL Server 2016 仅支持 R。 SQL Server 2017 支持 R 和 Python。 下表介绍了每个版本中的功能。

| 组件 | 说明 |
|-----------|-------------|
| R 包 | [RevoScaleR](ref-r-revoscaler.md) 是可缩放 R 的主库，具有用于数据操作、转换、可视化和分析的函数  。  <br/>[MicrosoftML](ref-r-microsoftml.md) 添加了机器学习算法，以便为文本分析、图像分析和情绪分析创建自定义模型  。 <br/>[sqlRUtils](ref-r-sqlrutils.md) 提供了帮助程序函数，用于将 R 脚本置于 T-SQL 存储过程中、向数据库注册存储过程，以及从 R 开发环境运行存储过程  。<br/>[**olapR**](ref-r-olapr.md) 用于在 R 中指定 MDX 查询。|
| Microsoft R Open (MRO) | [MRO](https://mran.microsoft.com/open) 是 R 的 Microsoft 开放源代码分发版。其中包括包和解释器  。 始终使用安装程序中捆绑的 MRO 版本。 |
| R 工具 | R 控制台窗口和命令提示符是 R 分发版中的标准工具。 可在 \Program files\Microsoft SQL Server\140\R_SERVER\bin\x64 查找它们。 |
| R 示例和脚本 |  开放源代码 R 和 RevoScaleR 包中包括内置数据集，因此可以使用预安装的数据来创建和运行脚本。 可在 \Program files\Microsoft SQL Server\140\R_SERVER\library\datasets and \library\RevoScaleR 查找它们。 |
| Python 包 | [revoscalepy](../python/ref-py-revoscalepy.md) 是可缩放 Python 的主库，具有用于数据操作、转换、可视化和分析的函数  。 <br/>[microsoftml](../python/ref-py-microsoftml.md) 添加了机器学习算法，以便为文本分析、图像分析和情绪分析创建自定义模型  。  |
| Python 工具 | 内置的 Python 命令行工具适用于临时测试和任务。 可在 \Program files\Microsoft SQL Server\140\PYTHON_SERVER\python.exe 查找此工具。 |
| Anaconda | Anaconda 是 Python 和基本包的开放源代码分发。 |
| Python 示例和脚本 | 与 R 一样，Python 包含内置的数据集和脚本。 可在 \Program files\Microsoft SQL Server\140\PYTHON_SERVER\lib\site-packages\revoscalepy\data\sample-data 查找 revoscalepy 数据。 |
| R 和 Python 中的预定型模型 | 预定型模型是为特定用例创建的，由 Microsoft 数据科学工程团队维护。 可以按原样使用预定型模型对文本中的正面-负面情绪进行评分，或使用你提供的新数据输入来检测图像中的特征。 预定型模型在独立服务器上受支持且可用，但无法通过 SQL Server 安装程序对其进行安装。 有关详细信息，请参阅[在 SQL Server 上安装预定型机器学习模型](../install/sql-pretrained-models-install.md)。 |

## <a name="using-a-standalone-server"></a>使用独立服务器

R 和 Python 开发者通常会选择独立服务器，以打破开放源代码 R 和 Python 的内存和处理限制。 在独立服务器上执行的 R 和 Python 库可以在多个核心上加载和处理大量数据，并将结果聚合到单个合并输出中。 高性能函数专为规模和实用性而设计：在 Microsoft 设计和支持的商业服务器软件中，提供预测分析、统计建模、数据可视化和领先的机器学习算法。

作为与 SQL Server 分离的独立服务器，其 R 和 Python 环境是使用独立服务器（而非 SQL Server）中提供的基础操作系统和标准工具进行配置、保护和访问的。 没有对 SQL Server 关系数据的内置支持。 如果要使用 SQL Server 数据，可以创建数据源对象和连接（就像从任何客户端创建数据源对象和连接一样）。

如果需要进行本地计算和远程计算，则作为 SQL Server 的附属物的独立服务器还可用作功能强大的开发环境。 独立服务器上的 R 和 Python 包与随数据库引擎安装提供的 R 和 Python 包相同，允许实现代码可移植性和[计算上下文切换](/machine-learning-server/r/concept-what-is-compute-context)。

## <a name="how-to-get-started"></a>如何入门

从安装程序开始，将二进制文件附加到你最喜爱的开发工具中，然后编写你的第一个脚本。

### <a name="step-1-install-the-software"></a>步骤 1：安装软件

安装以下任一版本：

+ [SQL Server 2017 Machine Learning Server（独立版）](../install/sql-machine-learning-standalone-windows-install.md)
+ [SQL Server 2016 R Server（独立版）- 仅限 R](../install/sql-machine-learning-standalone-windows-install.md?view=sql-server-2016&preserve-view=true)

### <a name="step-2-configure-a-development-tool"></a>步骤 2：配置开发工具

在独立服务器上，通常使用安装在同一台计算机上的开发工具在本地执行工作。

+ [设置 R 工具](set-up-a-data-science-client.md)
+ [设置 Python 工具](../python/setup-python-client-tools-sql.md)

### <a name="step-3-write-your-first-script"></a>步骤 3：编写你的第一个脚本

使用 RevoScaleR、revoscalepy 和机器学习算法中的函数来编写 R 或 Python 脚本。
  
  + [通过 25 个函数探索 R 和 RevoScaleR](/machine-learning-server/r/tutorial-r-to-revoscaler)：从基本的 R 命令开始，然后使用 RevoScaleR 可分发的分析函数，这些函数可提高 R 解决方案的性能和可伸缩性。 包括许多最流行的 R 建模包的可并行化版本，例如 K-均值聚类、决策树和决策林以及用于数据操作的工具。

  + [快速入门：microsoftml Python 包的二元分类示例](/machine-learning-server/python/quickstart-binary-classification-with-microsoftml)：使用 microsoftml 中的函数和熟知的乳腺癌数据集创建二元分类模型。

选择最适合任务的语言。 R 非常适合用于使用 SQL 难以实现的统计计算。 对数据进行基于集的操作时，可利用强大的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来实现最佳的性能。 对列进行快速计算时，可使用内存数据库引擎。

### <a name="step-4-operationalize-your-solution"></a>步骤 4：操作你的解决方案

独立服务器可以使用非 SQL 品牌 [Microsoft Machine Learning Server](/machine-learning-server/what-is-machine-learning-server) 的[操作化](/machine-learning-server/what-is-operationalization)功能。 可以为操作化配置独立服务器，这样配置具有以下好处：将代码作为 Web 服务部署和托管、运行诊断、测试 Web 服务容量。

### <a name="step-5-maintain-your-server"></a>步骤 5：维护服务器

SQL Server 定期发布累积更新。 应用累积更新可以增强现有安装的安全性和功能。 

有关新功能或更改的功能的说明，请参阅 [CAB 下载](../install/sql-ml-cab-downloads.md)文章和 Web 页上的 [SQL Server 2016 累积更新](https://support.microsoft.com/help/3177312/sql-server-2016-build-versions)和 [SQL Server 2017 累积更新](https://support.microsoft.com/help/4047329)。 

有关如何将更新应用到现有实例的详细信息，请参阅安装说明中的[应用更新](../install/sql-machine-learning-standalone-windows-install.md#apply-cu)。

## <a name="see-also"></a>另请参阅

 [安装 R Server（独立版）或 Machine Learning Server（独立版）](../install/sql-machine-learning-standalone-windows-install.md)
