---
title: R 教程：在 SQL 中开发模型
description: 了解如何基于 SQL Server 2016 或 SQL Server 2017 中的 R 功能支持，生成用于预测建模的端到端解决方案。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/11/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: afd34354290134d0973c1105a902b96c4bbdd706
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469998"
---
# <a name="tutorial-sql-development-for-r-data-scientists"></a>教程：面向 R 数据科学家的 SQL 开发
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

本教程主要面向数据科学家，介绍如何基于 SQL Server 2016 或 SQL Server 2017 中的 R 功能支持，生成用于预测建模的端到端解决方案。 本教程使用 SQL Server 上的 [NYCTaxi_sample](demo-data-nyctaxi-in-sql.md) 数据库。 

你将结合使用 R 代码、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据和自定义 SQL 函数生成一个分类模型，该模型指示司机在特定出租车行程中可能获得小费的概率。 你还会将 R 模型部署到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，并使用服务器数据基于该模型生成分数。

此示例可以扩展到各类现实问题，例如预测客户对销售活动的反应，或预测各个活动的消费或出席率。 因为该模型可以从存储过程进行调用，所以可以轻松地将它嵌入到应用程序中。

本演练旨在向 R 开发人员介绍 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]，因此会尽可能使用 R。 但这并不意味着 R 一定是每个任务的最佳工具。 在许多情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会提供更好的性能，特别是对于诸如数据聚合和特征工程这类任务。  这类任务可以特别受益于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中的新功能，如内存优化的列存储索引。 我们会尽量在此过程中指出可能的优化。

## <a name="prerequisites"></a>先决条件

+ [与 R 集成的 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md#verify-installation)或 [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

+ [数据库权限](../security/user-permission.md)和 SQL Server 数据库用户登录名

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ [纽约市出租车演示数据库](demo-data-nyctaxi-in-sql.md)

+ R IDE（如 RStudio）或 R 附带的内置 RGUI 工具

建议在客户端工作站上执行此演练。 你必须能够在同一网络上连接到启用了 SQL Server 和 R 语言的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机。 有关工作站配置的说明，请参阅[设置用于 R 开发的数据科学客户端](../r/set-up-a-data-science-client.md)。

或者，可以在同时具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 R 开发环境的计算机上运行此演练，但我们不建议在生产环境中使用此配置。 如果需要将客户端和服务器放在同一台计算机上，请确保安装另一组 Microsoft R 库，以便从“远程”客户端发送 R 脚本。 请勿使用安装在 SQL Server 实例的程序文件中的 R 库。 具体而言，如果使用一台计算机，则需要在这两个位置都使用 RevoScaleR 库，以支持客户端和服务器操作。

+ C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

> [!NOTE]
> 如果使用的是 [Machine Learning Server](/machine-learning-server/) 或 [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/)，而不是 R 客户端，则 RevoScaleR 的路径为 C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR

<a name="add-packages"></a>

## <a name="additional-r-packages"></a>其他 R 包

本演练需要一些未默认作为 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]的一部分安装的 R 库。 必须将这些包安装到用于开发解决方案的客户端上以及用于部署解决方案的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机上。

### <a name="on-a-client-workstation"></a>在客户端工作站上

在 R 环境中，复制以下行并在控制台窗口（Rgui 或 IDE）中执行代码。 某些包还会安装所需的包。 总共安装大约 32 个包。 必须连接 Internet 才能完成此步骤。
    
  ```R
  # Install required R libraries, if they are not already installed.
  if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
  if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
  if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
  if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
  ```

### <a name="on-the-server"></a>在服务器上

在 SQL Server 上安装包时有多个选项可选。 例如，SQL Server 提供 [R 包管理](../package-management/install-additional-r-packages-on-sql-server.md)功能，让数据库管理员可以创建包存储库，并为用户分配相应权限来安装自己的包。 但是，如果你是计算机上的管理员，则可以使用 R 安装新包，只要安装到正确的库中即可。

> [!NOTE]
> 在服务器上，即使出现提示，也 **不要** 安装到用户库。 如果安装到用户库，SQL Server 实例将找不到或无法运行包。 有关详细信息，请参阅 [在 SQL Server 上安装新的 R 包](../package-management/install-additional-r-packages-on-sql-server.md)。

1. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机上，**以管理员身份** 打开 RGui.exe。  如果已使用默认设置安装 SQL Server R Services，则可以在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64 中找到 Rgui.exe。

2. 在 R 提示符处，运行下面的 R 命令：
  
  ```R
  install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  ```
  此示例使用 R grep 函数搜索可用路径的矢量，并查找包含“Program Files”的路径。 有关详细信息，请参阅 [https://www.rdocumentation.org/packages/base/functions/grep](https://www.rdocumentation.org/packages/base/functions/grep)。

  如果认为包已安装，请通过运行 `installed.packages()` 来查看已安装包的列表。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览并汇总数据](walkthrough-view-and-summarize-data-using-r.md)