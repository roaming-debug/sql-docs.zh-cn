---
title: 什么是 SQL Server 机器学习服务（Python 和 R）？
titleSuffix: ''
description: 机器学习服务是 SQL Server 中一项支持使用关系数据运行 Python 和 R 脚本的功能。 可以使用开源包和框架，以及 Microsoft Python 和 R 包进行预测分析和机器学习。 脚本在数据库中执行，而不将数据移动到 SQL Server 外部或是在网络上移动。 本文介绍 SQL Server 机器学习服务的基础知识以及如何开始使用该服务。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 23206301c399449042380976e985ec3981933caa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340312"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>什么是 Python 和 R 的 SQL Server 机器学习服务？
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

机器学习服务是 SQL Server 中一项支持使用关系数据运行 Python 和 R 脚本的功能。 可以使用开源包和框架，以及 [Microsoft Python 包和 R 包](#packages)进行预测分析和机器学习。 脚本在数据库中执行，而不将数据移动到 SQL Server 外部或是在网络上移动。 本文介绍 SQL Server 机器学习服务的基础知识以及如何开始使用该服务。

有关其他 SQL 平台上的机器学习，请参阅 [SQL 机器学习文档](index.yml)。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> 要在 SQL Server 中执行 Java，请参阅 [Java 语言扩展文档](../language-extensions/java-overview.md)。
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>在 SQL Server 中执行 Python 和 R 脚本

使用 SQL Server 机器学习服务，你可以在数据库中执行 Python 和 R 脚本。 可以使用它来准备和清理数据、执行特征工程以及在数据库中定型、评估和部署机器学习模型。 此功能在数据所在的位置运行脚本，无需通过网络将数据传输到其他服务器。

你可以通过存储过程 [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 在 SQL Server 实例上执行 Python 和 R 脚本。

Python 和 R 的基本分发包含在机器学习服务中。 除了 Microsoft 包之外，还可以安装和使用开源包和框架，例如 PyTorch、TensorFlow 和 scikit-learn。

机器学习服务使用扩展性框架在 SQL Server 中运行 Python 和 R 脚本。 详细了解工作原理：

+ [扩展性框架](concepts/extensibility-framework.md)
+ [Python 扩展](concepts/extension-python.md)
+ [R 扩展](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>机器学习服务入门

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [在 Windows 上](install/sql-machine-learning-services-windows-install.md)或[在 Linux 上](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)安装 SQL Server 机器学习服务。 还可以使用[大数据群集上的机器学习服务](../big-data-cluster/machine-learning-services.md)和 [Azure SQL 托管实例上的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

1. 配置开发工具。 可以参阅[在 Azure Data Studio 笔记本中运行 Python 和 R 脚本](install/sql-machine-learning-azure-data-studio.md)。 还可以在 [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) 中运行 T-SQL。

1. 编写你的第一个 Python 或 R 脚本。

   + [适用于 SQL 机器学习的 Python 教程](tutorials/python-tutorials.md)
   + [适用于 SQL 机器学习的 R 教程](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ 编写你的第一个 Python 或 R 脚本。

   + [适用于 SQL 机器学习的 Python 教程](tutorials/python-tutorials.md)
   + [适用于 SQL 机器学习的 R 教程](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [在 Windows 上安装 SQL Server 机器学习服务](install/sql-machine-learning-services-windows-install.md)。

1. 配置开发工具。 可以参阅[在 Azure Data Studio 笔记本中运行 Python 和 R 脚本](install/sql-machine-learning-azure-data-studio.md)。 还可以在 [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) 中使用 T-SQL。

1. 编写你的第一个 Python 或 R 脚本。

   + [适用于 SQL 机器学习的 Python 教程](tutorials/python-tutorials.md)
   + [适用于 SQL 机器学习的 R 教程](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Python 和 R 版本

下面列出了机器学习服务中包含的 Python 和 R 版本。

| SQL Server 版本 | Python 版本 | R 版本 |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

有关 SQL Server 2016 中的 R 版本，请参阅[“什么是 R 服务？”中的“R 版本”部分](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version)

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Python 和 R 包

除了 Microsoft 的企业包外，还可以使用开源包和框架。 大多数常见的开源 Python 和 R 包都已预装在机器学习服务中。 还包括 Microsoft 提供的以下 Python 和 R 包：

| 语言 | 程序包 | 说明 |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | 可缩放的 Python 的主包。 数据转换和操作、统计摘要、可视化和多种形式的建模。 此外，此包中的函数会自动在可用内核之间分配工作负载以进行并行处理。 |
| Python | [microsoftml](python/ref-py-microsoftml.md) | 添加机器学习算法，以便为文本分析、图像分析和情绪分析创建自定义模型。 | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | 用于可缩放 R 的主包。数据转换和操作、统计摘要、可视化和多种形式的建模。 此外，此包中的函数会自动在可用内核之间分配工作负载以进行并行处理。 |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | 添加机器学习算法，以便为文本分析、图像分析和情绪分析创建自定义模型。 |
| R | [olapR](r/ref-r-olapr.md) | R 函数，用于针对 SQL Server Analysis Services OLAP 多维数据集进行 MDX 查询。 |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | 一种在 T-SQL 存储过程中使用 R 脚本、将该存储过程注册到数据库并在 [R 开发环境](r/set-up-a-data-science-client.md)中运行存储过程的机制。 |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) 是 Microsoft 中 R 的增强型分发。 它是用于统计分析和数据科学的完整开源平台。 它基于 R，并 100% 与 R 兼容，包括很多其他功能，可以带来更高的性能和可再现性。 |

有关与机器学习服务一并安装的包以及如何安装其他包的详细信息，请参阅：

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [获取 Python 包信息](package-management/python-package-information.md)
+ [使用 sqlmlutils 安装 Python 包](package-management/install-additional-python-packages-on-sql-server.md)
+ [获取 R 包信息](package-management/r-package-information.md)
+ [使用 sqlmlutils 安装 新的 R 包](package-management/install-additional-r-packages-on-sql-server.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
+ [获取 Python 包信息](package-management/python-package-information.md)
+ [使用 Python 工具在 SQL Server 上安装包](package-management/install-python-packages-standard-tools.md)
+ [获取 R 包信息](package-management/r-package-information.md)
+ [使用 T-SQL (CREATE EXTERNAL LIBRARY) 将 R 包安装在 SQL Server 上](package-management/install-r-packages-with-tsql.md)。
::: moniker-end

## <a name="next-steps"></a>后续步骤

+ [在 Windows 上](install/sql-machine-learning-services-windows-install.md)或[在 Linux 上](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)安装 SQL Server 机器学习服务
+ [适用于 SQL 机器学习的 Python 教程](tutorials/python-tutorials.md)
+ [适用于 SQL 机器学习的 R 教程](tutorials/r-tutorials.md)