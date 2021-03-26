---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 70bf06deb43e861209e12b6481f3fd414cb9361f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833729"
---
## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装 [机器学习服务](../../sql-server-machine-learning-services.md)，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

运行以下命令以在 SUSE Linux Enterprise Server (SLES) 上安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 R 自定义运行时。

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>安装 R

1. 如果安装了[机器学习服务](../../sql-server-machine-learning-services.md)，则已在 `/opt/microsoft/ropen/3.5.2/lib64/R` 中安装 R。 如果要继续使用该路径作为 R_HOME，则可以跳过此步骤。

    如果要使用 R 的其他运行时，首先需要删除 `microsoft-r-open-mro`，然后再继续安装新版本。

    ```bash
    sudo zypper remove microsoft-r-open-mro-3.4.4
    ```

1. 安装用于 SUSE Linux Enterprise Server (SLES) 的 [R（3.3 或更高版本）](https://www.r-project.org/)。 默认情况下，R 安装在 /usr/lib64/R 中。 此路径是 R_HOME。 如果在其他位置安装 R，请记下该路径作为你的 R_HOME。

    请按照以下步骤安装 R：

    ```bash
    sudo zypper ar -f http://download.opensuse.org/repositories/devel:/languages:/R:/patched/openSUSE_12.3/ R-patched
    sudo zypper --gpg-auto-import-keys ref
    sudo zypper install R-core-libs R-core R-core-doc R-patched
    ```

    如果不需要 R-tcltk-3.6.1，则可以忽略有关此包的警告。

## <a name="install-gcc-c"></a>安装 gcc-c++

在 SUSE Linux Enterprise Server (SLES) 上安装 gcc-c++。 它用于稍后安装的 Rcpp。

```bash
sudo zypper install gcc-c++
```
