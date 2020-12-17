---
title: 使用 R 进行预测性建模
description: 本文介绍通过与 SQL Server 集成可以实现的数据科学过程的改进。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/15/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 283eaa89b214e9324e6b63bffe34735f6067bd39
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470878"
---
# <a name="data-exploration-and-predictive-modeling-with-r-in-sql-server"></a>在 SQL Server 中通过 R 进行数据浏览和预测性建模
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍通过与 SQL Server 集成可以实现的数据科学过程的改进。

## <a name="the-data-science-process"></a>数据科学过程

数据科学家经常使用 R 来浏览数据并构建预测模型。 此过程通常需要反复经历各种尝试和错误才能获得良好的预测模型。 有经验的数据科学家会使用 RODBC 包连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库并将数据提取到本地工作站，然后对数据进行浏览，最后再使用标准 R 包构建预测模型。

但是，这种方法有很多缺点，这影响了 R 在企业中的广泛采用。 

+ 数据移动可能很慢、效率低下或不安全
+ R 本身存在性能和缩放限制

当你需要移动和分析大量数据，或者使用的数据集不适合计算机上可供使用的内存时，这些缺点就会变得更明显。

你可以使用 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 随附的新的可缩放包和 R 函数来克服这些困难。 

## <a name="whats-different-about-revoscaler"></a>RevoScaleR 有什么不同？

**RevoScaleR** 包包含部分最常用 R 函数的实现，这些实现在经过重新设计后可提供并行度和缩放功能。 有关详细信息，请参阅[使用 RevoScaleR 进行分布式计算](/machine-learning-server/r/how-to-revoscaler-distributed-computing)。

RevoScaleR 包还允许更改 *执行上下文*。 这意味着，不管是完整解决方案还是单个函数，都应使用托管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的资源进行计算，而不应使用本地工作站。 这样做有很多好处：避免不必要的数据移动，并可利用服务器计算机上更强大的计算资源。

## <a name="r-environment-and-packages"></a>R 环境和包

[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 中支持的 R 环境包含运行时、开源语言，以及多个包所支持和扩展的图形引擎。 该语言允许使用通过包实现的多种扩展。  

### <a name="using-other-r-packages"></a>使用其他 R 包

除 Microsoft 机器学习附带的专有 R 库之外，你几乎可以在解决方案中使用任何 R 包，包括：

+ 公共存储库提供的通用 R 包。 你可以从公共存储库（例如 CRAN，托管 6000 多个可供数据科学家使用的包）获取最常用的开源 R 包。
  
  对于 Windows 平台，R 包以 zip 文件形式提供，可以在获得 GPL 许可的情况下下载和安装。  
  
  有关如何安装适用于 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]的第三方包的信息，请参阅 [Install Additional R Packages on SQL Server](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md)  
  
+ [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]提供的其他包和库。
  
     **RevoScaleR** 包包括高性能大数据分析、支持常用数据科学任务的改进版函数、适用于 Naive Bayes 的优化学习模型、线性回归、时序模型、神经网络以及高级数学库。  
  
     **RevoPemaR** 包允许你在 R 中开发自己的并行外部存储器算法。  
  
     有关这些包及其使用方法的详细信息，请参阅[什么是 RevoScaleR ](/machine-learning-server/r/concept-what-is-revoscaler)和 [RevoPemaR 入门](/machine-learning-server/r/how-to-developer-pemar)。 

+ MicrosoftML 包含 Microsoft 数据科学团队提供的高度优化的机器学习算法和数据转换的集合  。 Azure 机器学习还使用了许多算法。 有关详细信息，请参阅 [SQL Server 中的 MicrosoftML](ref-r-microsoftml.md)。

### <a name="r-development-tools"></a>R 开发工具

开发 R 解决方案时，请确保下载 Microsoft R Client。 此免费下载包括支持远程计算上下文和可缩放算法所需的库：

+ **[!INCLUDE[rsql_rro-noversion](../../includes/rsql-rro-noversion-md.md)]：** R 运行时的一个分发版以及一组包（例如 Intel 数学内核库），用于提升标准 R 操作的性能。  
  
+ **RevoScaleR：** 一个可用来将计算推送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 R 包。 [!INCLUDE[rsql_rre-noversion](../../includes/rsql-rre-noversion-md.md)] 列中的一个值匹配。 它还包括一组常用 R 函数，这些函数在重新设计后具有更好的性能和可伸缩性。 你可以通过 **rx** 前缀来标识这些性能已改善的函数。 它还包括了针对各种源的增强数据提供程序；这些函数具有前缀 **Rx**。

你可以使用支持 R 的任何基于 Windows 的代码编辑器，例如 [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)] 或 RStudio。 [!INCLUDE[rsql_rro-noversion](../../includes/rsql-rro-noversion-md.md)] 的下载包还包括 R 的常用命令行工具，例如 RGui.exe。

## <a name="use-new-data-sources-and-compute-contexts"></a>使用新数据源和计算上下文

通过 RevoScaleR 包连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，请查看在 R 代码中使用的这三个函数：

+ **RxSqlServerData** 是 RevoScaleR 包中提供的函数，支持通过改进型数据连接来连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]随附的新的可伸缩包和 R 函数来克服这些困难。
  
     可以在 R 代码中使用此函数来定义 *数据源*。 数据源对象指定数据所在的服务器和表，并管理在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中读写数据的任务。
  
-   **RxInSqlServer** 函数可用来指定计算上下文。   换言之，你可以指定执行 R 代码的位置：本地工作站或托管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机。  有关详细信息，请参阅 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)。
  
     当你设置计算上下文时，该上下文仅影响支持远程执行上下文的计算，即 RevoScaleR 包及相关函数提供的 R 操作。 通常，基于标准 CRAN 包的 R 解决方案不能在远程计算上下文中运行，但如果是由 T-SQL 启动的，则它们可以在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 计算机上运行。 不过，你可以使用 `rxExec` 函数调用各个 R 函数并在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 中远程运行它们。

有关如何创建和使用数据源和执行上下文的示例，请参阅以下教程：

+ [对数据科学的深入探讨](../../machine-learning/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)  
+  [使用 Microsoft R 进行数据分析](/machine-learning-server/r/how-to-introduction)

## <a name="deploy-r-code-to-production"></a>将 R 代码部署到生产环境

数据科学的一个重要部分是将你的分析提供给他人，或者使用预测模型来改善业务成果或流程。 在 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]中，当 R 脚本或模型就绪以后，可以轻松地转移到生产。

有关如何将代码转移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的第三方包的信息，请参阅 [Operationalizing Your R Code](../../machine-learning/r/operationalizing-your-r-code.md)随附的新的可伸缩包和 R 函数来克服这些困难。

通常情况下，在开始部署时会对脚本进行清除，去掉生产过程中不需要的脚本。 将计算移近数据时，可能会找到比 R 中的所有操作更有效地移动、汇总或呈现数据的方法。建议数据科学家就改善性能的方法咨询数据库开发人员，尤其是在解决方案进行可能在 SQL 中更有效的数据清理或功能设计时。 可能需要更改 ETL 流程来确保模型的构建或评分工作流不会失败，并可能需要以正确格式提供输入数据。

## <a name="see-also"></a>另请参阅

[Base R 函数和 RevoScaleR 函数之对比](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r)

[SQL Server 中的 RevoScaleR 库](ref-r-revoscaler.md)