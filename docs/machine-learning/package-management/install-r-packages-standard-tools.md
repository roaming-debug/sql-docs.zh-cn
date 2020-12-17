---
title: 使用 R 工具安装包
description: 了解如何使用标准 R 工具将新的 R 包安装到 SQL Server 机器学习服务或 SQL Server R Services 的实例。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 5943de8bcc6588572bc3acebed5b3ba4104b7a96
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471058"
---
# <a name="install-packages-with-r-tools"></a>使用 R 工具安装包

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

本文介绍了如何使用标准 R 工具将新的 R 包安装到 SQL Server 机器学习服务或 SQL Server R Services 的实例。 可以将包安装在具有 Internet 连接的 SQL Server 上，也可以安装在与 Internet 隔离的 SQL Server 上。

除了标准 R 工具之外，还可以使用以下工具安装 R 包：

+ [RevoScaleR](install-r-packages-with-revoscaler.md)
::: moniker range="=sql-server-2017"
+ [T-SQL](install-r-packages-with-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end

## <a name="general-considerations"></a>一般注意事项

+ 在 SQL Server 中运行的 R 代码只能使用在默认实例库中安装的包。 SQL Server 无法从外部库加载包，即使该库位于同一台计算机上也是如此。
这包括与其他 Microsoft 产品一起安装的 R 库。

+ R 包库位于 SQL Server 实例的“程序文件”文件夹中，默认情况下，在此文件夹中安装需要管理员权限。 有关详细信息，请参阅[包库位置](../package-management/r-package-information.md#default-r-library-location)。

  ::: moniker range="=sql-server-2017"
  非管理员可以使用 RevoScaleR 9.0.1 或更高版本或使用 CREATE EXTERNAL LIBRARY 安装包。 dbo_owner 用户或具有 CREATE EXTERNAL LIBRARY 权限的用户可以将 R 包安装到当前数据库  。 有关详细信息，请参阅：
  + [使用 RevoScaleR 安装 R 包](install-r-packages-with-revoscaler.md)
  + [使用 T-SQL (CREATE EXTERNAL LIBRARY) 将 R 包安装在 SQL Server 上](install-r-packages-with-tsql.md)
  ::: moniker-end

  ::: moniker range="=sql-server-2016"
  非管理员可以使用 RevoScaleR 9.0.1 及更高版本安装包。 dbo_owner 用户可以将 R 包安装到当前数据库  。 有关详细信息，请参阅[使用 RevoScaleR 安装 R 包](install-r-packages-with-revoscaler.md)。
  ::: moniker-end

+ 在强化的 SQL Server 环境中，可能希望避免使用以下包：
  + 需要网络访问权限的包
  + 需要提升的文件系统访问权限的包
  + 用于 Web 开发或不受益于在 SQL Server 内部运行的其他任务的包

## <a name="online-installation-with-internet-access"></a>联机安装（能够访问 Internet）

如果 SQL Server 有权访问 Internet，则可以使用标准包安装工具安装 R 包。

1. 确定实例库的位置（请参阅[获取 R 包信息](../package-management/r-package-information.md)），并导航到安装 R 工具的文件夹。

   ::: moniker range="=sql-server-2016"
   例如，SQL Server 默认实例的默认路径为：

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   例如，SQL Server 默认实例的默认路径为：

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. 以管理员身份从此文件夹运行 R 或 Rgui   。

1. 运行 R 命令 `install.packages`，并指定包名称。 如果包有任何依赖项，则安装程序会自动下载并安装这些依赖项。

如果你有多个 SQL Server 并行实例，请分别对要使用包的每个实例运行安装。 无法跨实例共享包。

## <a name="offline-installation-no-internet-access"></a><a name = "bkmk_offlineInstall"></a> 脱机安装（无法访问 Internet）

通常，托管生产数据库的服务器无法连接 Internet。 若要在该环境中安装 R 包，你需要提前下载并准备包和依赖项（作为压缩的文件），然后将这些文件复制到服务器上的文件夹。 文件就绪后，可以脱机安装包。

标识所有依赖项变得非常复杂。 对于 R，建议使用 [miniCRAN **创建本地存储库**](https://andrie.github.io/miniCRAN/)。
miniCRAN 具有要安装的包列表，可分析依赖项，并收集所有必要的压缩文件  。 然后，它将创建单个存储库，可以将其复制到隔离的 SQL Server 实例。 [igraph **包还有助于分析包依赖项**](https://igraph.org/r/)。

有关详细信息，请参阅[使用 miniCRAN 创建本地 R 包存储库](create-a-local-package-repository-using-minicran.md)。

zip 文件位于 SQL Server 实例上后，即可使用服务器上的标准 R 工具安装它。

1. 确定实例库的位置（请参阅[获取 R 包信息](../package-management/r-package-information.md)），并导航到安装 R 工具的文件夹。 

   ::: moniker range="=sql-server-2016"
   例如，SQL Server 默认实例的默认路径为：

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   例如，SQL Server 默认实例的默认路径为：

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. 以管理员身份从此文件夹运行 R 或 Rgui   。

1. 运行 R 命令 `install.packages`，并指定包或存储库名称以及压缩文件的位置。 例如：

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   此命令从其本地压缩文件中提取 R 包 `mynewpackage`，并安装包。 如果包具有任何依赖项，则安装程序会检查库中的现有包。 如果已创建包含依赖项的存储库，则安装程序还将安装所需的包。

   > [!NOTE]
   > 如果实例库中不存在任何所需的包，并且在压缩文件中找不到这些包，则目标包安装失败。

作为 miniCRAN 的替代方法，可以手动执行以下步骤  ：

1. 标识所有包依赖项。
1. 检查是否已在服务器上安装了任何所需的包。 如果已安装包，请验证版本是否正确。
1. 将包和所有依赖项下载到能够访问 Internet 的单独计算机。
1. 将包和依赖项放置在单个包存档中。
1. 如果尚未压缩存档，则将其压缩。
1. 将文件移动到服务器可以访问的文件夹。
1. 运行受支持的安装命令或 DDL 语句，将包安装到实例库中。

## <a name="see-also"></a>另请参阅

+ [获取 R 包信息](r-package-information.md)
+ [使用 R 包的提示](tips-for-using-r-packages.md)
+ [SQL Server R 语言教程](../tutorials/r-tutorials.md)