---
title: 使用 miniCRAN 创建存储库
titleSuffix: SQL machine learning
description: 了解如何使用 miniCRAN 包来创建包和依赖项的本地存储库，从而脱机安装 R 包。
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f93a1180f25f764330db44306457e775961bc0da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350691"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>使用 miniCRAN 创建本地 R 包存储库
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文介绍如何使用 [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) 来创建包和依赖项的本地存储库，从而脱机安装 R 包。 miniCRAN 识别包和依赖项，并将其下载到一个文件夹中，你将该文件夹复制到其他计算机以脱机安装 R 包。

可以指定一个或多个包，miniCRAN 会以递归方式读取这些包的依赖关系树。 然后，它会从 CRAN 或类似存储库中仅下载列出的包及其依赖项。

完成后，miniCRAN 会创建一个内部一致的存储库，其中包含所选包和所有必需的依赖项。 可以将此本地存储库移动到服务器，并继续在无 Internet 连接的情况下安装包。

经验丰富的 R 用户经常在已下载包的 DESCRIPTION 文件中查找依赖项包的列表。 但是，Imports 中列出的包可能具有二级依赖项。 出于此原因，我们建议将 miniCRAN 用于组装所需包的完整集合。

## <a name="why-create-a-local-repository"></a>为什么创建本地存储库

创建本地包存储库的目的是提供单个位置，组织中的服务器管理员或其他用户可以使用该位置在服务器上安装新的 R 包（尤其是对于无法访问 Internet 的用户）。 创建存储库之后，可以通过添加新包或升级现有包的版本来对其进行修改。

包存储库对于以下方案非常有用：

- 安全性：许多 R 用户都习惯从 CRAN 或其某个镜像站点中随意下载并安装新的 R 包。 但是，出于安全原因，运行 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 的生产服务器通常都未连接 Internet。

- 离线安装更轻松：若要将包安装到脱机服务器，则还需要下载所有包依赖项。 使用 miniCRAN 可以更轻松地获取正确格式的所有依赖项，并避免出现依赖项错误。

- 版本管理已改进：在多用户环境中，有充分的理由可以避免在服务器上无限制安装多个包版本。 使用本地存储库为用户提供一致的包集。

## <a name="install-minicran"></a>安装 miniCRAN

miniCRAN 包本身依赖于 18 个其他 CRAN 包，这些包中的 RCurl 包与 curl-devel 包具有系统依赖关系。 同样，XML 包依赖于 libxml2-devel。 若要解决依赖关系，建议首先在具有完全 Internet 访问权限的计算机上构建本地存储库。

在具有基本 R、R 工具和 Internet 连接的计算机上运行以下命令。 假定这不是你的 SQL Server 计算机。 以下命令安装 miniCRAN 包和 igraph 包。 此示例检查是否已安装包，但你可以绕过 `if` 语句直接安装包。

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>设置 CRAN 镜像和 MRAN 快照

指定用于获取包的镜像站点。 例如，可以使用 MRAN 站点或区域中包含所需包的任何其他站点。 如果下载失败，请尝试使用另一个镜像站点。

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>创建本地文件夹

创建用于存储所收集的包的本地文件夹。 如果经常重复此操作，可能需要使用描述性名称，如“miniCRANZooPackages”或“miniCRANMyRPackageV2”。

将该文件夹指定为本地存储库。 R 语法对路径名称使用正斜杠，这与 Windows 惯例相反。

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>将包添加到本地存储库

安装并加载 miniCRAN 后，创建指定其他要下载的包的列表。

请勿向此初始列表添加依赖项。 miniCRAN 使用的 igraph 包会自动生成依赖项列表。 有关如何使用生成的依赖项关系图的详细信息，请参阅 [Using miniCRAN to identify package  dependencies](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html)（使用 miniCRAN 识别包依赖项）。

1. 将目标包“zoo”和“forecast”添加到变量。

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. （可选）绘制依赖项关系图。 这非必需操作，但可以提供信息。

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. 创建本地存储库。 如果需要，请务必将 R 版本更改为 SQL Server 实例上安装的版本。 如果执行了组件升级，则你的版本可能会比原始版本要新。 有关详细信息，请参阅[获取 R 包信息](../package-management/r-package-information.md)。

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   根据这些信息，miniCRAN 包会创建你之后将包复制到 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 所需的文件夹结构。

此时，你应该有一个文件夹，其中包含你所需的包和任何必需的其他包。 该文件夹应包含压缩包集合。 请勿解压包或重命名任何文件。

或者，运行以下代码以列出本地 miniCRAN 存储库中包含的包。

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>将包添加到实例库

使用所需的包创建本地存储库后，将包存储库移动到 SQL Server 计算机。 以下过程介绍如何使用 R 工具安装包。

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> 安装包的建议方法是使用 sqlmlutils。 请参阅[使用 sqlmlutils 安装新的 R 包](install-additional-r-packages-on-sql-server.md)。
::: moniker-end

1. 将包含 miniCRAN 存储库的文件夹完整复制到计划在其中安装包的服务器。 文件夹通常具有以下结构： 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   在此过程中，我们假设根驱动器上有一个文件夹。

2. 打开与实例关联的 R 工具（例如，可以使用 Rgui.exe）。 右键单击并选择“以管理员身份运行”，以允许该工具更新系统。

   ::: moniker range="=sql-server-2016"
   - 例如，RGUI 的默认文件位置是 `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`。
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   - 例如，RGUI 的文件位置是 `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`。
   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   - 例如，RGUI 的文件位置是 `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`。
   ::: moniker-end

3. 获取实例库的路径，并将其添加到库路径列表。

   ::: moniker range="=sql-server-2016"
   例如，

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   例如，

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   例如，

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. 在服务器上，将复制 miniCRAN 存储库的新位置指定为 `server_repo`。

    在此示例中，我们假定已将存储库复制到服务器上的一个临时文件夹中。

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. 由于你在服务器上的新 R 工作区中工作，因此还必须提供要安装的包列表。

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. 安装包，提供 miniCRAN 存储库本地副本的路径。

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. 从实例库中，可以使用如下所示的命令查看已安装的包：

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>另请参阅

+ [获取 R 包信息](../package-management/r-package-information.md)
+ [R 教程](../tutorials/r-tutorials.md)