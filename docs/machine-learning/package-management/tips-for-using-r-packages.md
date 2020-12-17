---
title: 使用 R 包的提示
titleSuffix: SQL machine learning
description: 了解 R 或 SQL Server 新用户在 SQL Server 中使用 R 包的有用提示。
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e7480b4d31685be402c98892557ddfb5f7db0ab5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470978"
---
# <a name="tips-for-using-r-packages"></a>使用 R 包的提示

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文提供了关于在 SQL Server 中使用 R 包的有用提示。 这些提示适用于不熟悉 R 的 DBA 以及拥有丰富经验但对 SQL Server 实例中的包访问不熟悉的 R 开发人员。

## <a name="if-youre-new-to-r"></a>不熟悉 R

了解一些有关 R 包管理的基础知识对第一次安装 R 包的管理员很有帮助。

### <a name="package-dependencies"></a>包依赖项

R 包常常依赖于多个其他包，其中一些包可能在实例使用的默认 R 库中不可用。 有时，包需要一个不同于已安装版本的依赖包版本。 包依赖项记录在包中嵌入的 DESCRIPTION 文件中，但有时不完整。 可以使用名为 [iGraph](https://igraph.org/r/) 的包来完整表述依赖项关系图。

如果需要安装多个包，或者想要确保组织中的每个人都获得正确的包类型和版本，我们建议使用 [miniCRAN](https://mran.microsoft.com/package/miniCRAN) 包分析完整的依赖链。 minicRAN 会创建可在多个用户或计算机之间共享的本地存储库。 有关详细信息，请参阅[使用 miniCRAN 创建本地包存储库](create-a-local-package-repository-using-minicran.md)。

### <a name="package-sources-versions-and-formats"></a>包源、版本和格式

可以从多个来源获取 R 包，例如 [CRAN](https://cran.r-project.org/) 和 [Bioconductor](https://www.bioconductor.org/)。 R 语言的官方站点 (<https://www.r-project.org/>) 列出了许多这样的资源。 Microsoft 提供 [MRAN](https://mran.microsoft.com/) 来分发开放源代码 R 包 ([MRO](https://mran.microsoft.com/open)) 和其他包。 许多包被发布到了 GitHub 中，开发人员可以从其中获取源代码。

::: moniker range=">=sql-server-2016"
R 包在多个计算平台上运行。 确保安装的版本为 Windows 二进制文件。
::: moniker-end

::: moniker range=">=sql-server-linux-ver15"
R 包在多个计算平台上运行。 请确保安装的版本为 Linux 二进制文件。
::: moniker-end

### <a name="know-which-library-youre-installing-to-and-which-packages-are-already-installed"></a>了解要安装到的库以及已安装的包

如果之前修改了计算机上的 R 环境，安装前应先确保 R 环境变量 `.libPath` 仅使用一个路径。

该路径应指向实例的 R_SERVICES 文件夹。 有关详细信息（包括如何确定哪些包已安装），请参阅[获取 R 包信息](../package-management/r-package-information.md)。

## <a name="if-youre-new-to-sql-server"></a>不熟悉 SQL Server

如果身为 R 开发人员的你要编写在 SQL Server 上执行的代码，服务器安全策略会限制你对 R 环境的控制能力。 以下提示介绍了一些典型的情况，并提供了有关在此环境下进行开发的建议。

### <a name="r-user-libraries-not-supported-on-sql-server"></a>R 用户库：在 SQL Server 上不受支持

需要安装新的 R 包的 R 开发人员习惯于随意安装包，当默认库不可用或开发人员不是计算机管理员时使用专用的用户库。 例如，在典型的 R 开发环境中，用户会将包的位置添加到 R 环境变量 `libPath` 中，或者引用完整的包路径，如下所示：

```R
library("c:/Users/<username>/R/win-library/packagename")
```

在 SQL Server 中运行 R 解决方案时，此方法无效，因为 R 包必须安装到与实例相关联的特定默认库中。 如果包在默认库中不可用，则在尝试调用该包时，可能会收到此错误：

library(xxx) 出错：没有名为“package-name”的包 

有关如何在 SQL Server 中安装 R 包的信息，请参阅[在 SQL Server 机器学习服务或 SQL Server R Services 上安装新的 R 包](install-additional-r-packages-on-sql-server.md)。

### <a name="how-to-avoid-package-not-found-errors"></a>如何避免“找不到包”错误

遵循以下准则有助于避免“找不到包”错误。

+ 消除对用户库的依赖。

    将必需的 R 软件包安装到自定义用户库是一种不好的开发实践。 如果不具有库位置访问权限的其他用户运行解决方案，可能导致出现错误。

    同样，如果包安装在默认库中，则 R 运行时将从默认库加载包，即使 R 代码中指定了不同的版本。

+ 确保代码能够在共享环境中运行。

+ 避免将包安装作为解决方案的一部分。 如果你无包安装权限，则代码将失败。 即使具有包安装权限，也应该与要执行的其他代码分开。

+ 检查代码以确保未调用已卸载的包。

+ 更新代码以删除对 R 包或 R 库路径的直接引用。

+ 了解与实例相关联的包库。 有关详细信息，请参阅[获取 R 包信息](../package-management/r-package-information.md)。

## <a name="see-also"></a>另请参阅

::: moniker range="<=sql-server-2017"
+ [使用 R 工具安装包](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [使用 sqlmlutils 安装新的 R 包](install-additional-r-packages-on-sql-server.md)
::: moniker-end
