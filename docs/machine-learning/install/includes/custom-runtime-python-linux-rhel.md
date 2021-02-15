---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: c6d15087150e914e62b5d19951715198ced47512
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072858"
---
## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装 [机器学习服务](../../sql-server-machine-learning-services.md)，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

运行以下命令以在 Red Hat Enterprise Linux (RHEL) 上安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 Python 自定义运行时。

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-python-37-and-pandas"></a>安装 Python 3.7 和 pandas

用于自定义 Python 运行时的 Python 语言扩展目前仅支持 [Python 3.7](https://www.python.org/)。 如果要使用不同版本的 Python，请按照 [Python 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python)中的说明来修改和重新生成扩展。

1. 运行以下命令以安装 Python 3.7。

    ```bash
    # Install python3.7 and the corresponding library:
    yum install gcc openssl-devel bzip2-devel libffi-devel zlib-devel
    
    cd /usr/src
    wget https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tgz
    tar xzf Python-3.7.9.tgz
    
    cd Python-3.7.9
    ./configure --enable-optimizations --prefix=/usr
    make altinstall
    ```

1. 运行以下命令以安装 pandas 包

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
