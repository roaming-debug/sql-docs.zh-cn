---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 83b39be5be128ba4fbda17765a7b67deead2c80c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072716"
---
## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装 [机器学习服务](../../sql-server-machine-learning-services.md)，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

运行以下命令以在 Ubuntu Linux 上安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 R 自定义运行时。

1. 如果可以，请在安装之前运行此命令以刷新系统中的包。

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. 使用此命令安装 mssql-server-extensibility。

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-r"></a>安装 R

1. 如果安装了[机器学习服务](../../sql-server-machine-learning-services.md)，则已在 `/opt/microsoft/ropen/3.5.2/lib64/R` 中安装 R。 如果要继续使用该路径作为 R_HOME，则可以跳过此步骤。

    如果要使用 R 的其他运行时，首先需要删除 `microsoft-r-open-mro`，然后再继续安装新版本。

    ```bash
    sudo apt remove microsoft-r-open-mro-3.5.2
    ```

1. 安装适用于 Ubuntu 的 [R（3.3 或更高版本）](https://www.r-project.org/)。 默认情况下，R 安装在 /usr/lib/R 中。 此路径是 R_HOME。 如果在其他位置安装 R，请记下该路径作为你的 R_HOME。

    下面是适用于 Ubuntu 的示例说明。 为你的 R 版本更改以下存储库 URL。

    ```bash
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6
    
    # Add R CRAN repository. This repository works for R 4.0.x.
    #
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
    sudo apt-get update
    
    # Install R runtime.
    #
    sudo apt-get -y install r-base-core
    ```
