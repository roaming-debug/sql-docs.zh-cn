---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/18/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: f6ec3bbcc720fe472cc7146d59c30f0f3fc65ab0
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833726"
---
+ RExtension 需要 GLIBCXX_3.4.20。 请确保 Red Hat Enterprise Linux (RHEL) 上安装的 libstdc++.so.6 版本提供它。

## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装 [机器学习服务](../../sql-server-machine-learning-services.md)，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

运行以下命令以在 Red Hat Enterprise Linux (RHEL) 上安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 R 自定义运行时。

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-r"></a>安装 R

1. 如果安装了[机器学习服务](../../sql-server-machine-learning-services.md)，则已在 `/opt/microsoft/ropen/3.5.2/lib64/R` 中安装 R。 如果要继续使用该路径作为 R_HOME，则可以跳过此步骤。

    如果要使用 R 的其他运行时，首先需要删除 `microsoft-r-open-mro`，然后再继续安装新版本。

    ```bash
    sudo yum erase microsoft-r-open-mro-3.5.2
    ```

1. 安装用于 Red Hat Enterprise Linux (RHEL) 的 [R（3.3 或更高版本）](https://www.r-project.org/)。 默认情况下，R 安装在 /usr/lib64/R 中。 此路径是 R_HOME。 如果在其他位置安装 R，请记下该路径作为你的 R_HOME。

    ```bash
    sudo yum install -y R
    ```
