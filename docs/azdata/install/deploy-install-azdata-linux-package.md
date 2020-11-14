---
title: 使用 apt 安装 Azure Data CLI (azdata)
description: 了解如何使用 apt 安装 Azure Data CLI (azdata) 工具。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4508db56c0246f181f9244fe0a3b853a3e91eb24
ms.sourcegitcommit: 4b7ecc080795c5f90322d60df5c0550884f48140
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94334446"
---
# <a name="install-azure-data-cli-azdata-with-apt"></a>使用 apt 安装 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

[!INCLUDE[azdata](../../includes/applies-to-version/azdata.md)]

带有 `apt` 的 Linux 分发版有 `azdata-cli` 包。 该 CLI 包已在使用 `apt` 的 Linux 版本中测试：

- Ubuntu 16.04、Ubuntu 18.04

[!INCLUDE [azdata-package-installation-remove-pip-install](../../includes/azdata-package-installation-remove-pip-install.md)]

## <a name="install-with-apt"></a>使用 apt 进行安装

>[!IMPORTANT]
> `azdata-cli` 的 RPM 包依赖于 python3 包。 在你的系统上，这可能是早于所要求的 Python 3.6.x  的 Python 版本。 如果这对你来说是问题，请找到一个替代 python3 包，或按照使用 [`pip`](../install/deploy-install-azdata-pip.md) 的手动安装说明执行操作。

1. 安装必要的依赖项以安装 `azdata-cli`。

   ```bash
   sudo apt-get update
   sudo apt-get install gnupg ca-certificates curl wget software-properties-common apt-transport-https lsb-release -y
   ```

2. 导入 Microsoft 存储库密钥。

   ```bash
   curl -sL https://packages.microsoft.com/keys/microsoft.asc |
   gpg --dearmor |
   sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
   ```

3. 创建本地存储库信息。

   对于 Ubuntu 16.04 客户端运行：

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/prod.list)"
    ```

   对于 Ubuntu 18.04 客户端运行：

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/prod.list)"
    ```

   对于 Ubuntu 20.04 客户端，运行：

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/prod.list)"
    ```

4. 安装 `azdata-cli`。

   ```bash
   sudo apt-get update
   sudo apt-get install -y azdata-cli
   ```

## <a name="verify-install"></a>验证安装

```bash
azdata
azdata --version
```

## <a name="update"></a>更新

使用 `apt-get update` 和 `apt-get install` 命令更新 `azdata-cli`。

```bash
sudo apt-get update && sudo apt-get install --only-upgrade -y azdata-cli
```

## <a name="uninstall"></a>卸载

1. 从系统中删除包。

   ```bash
   sudo apt-get remove -y azdata-cli
   ```

2. 如果不打算重新安装 `azdata-cli`，请删除存储库信息。

   ```bash
   sudo rm /etc/apt/sources.list.d/azdata-cli.list
   ```

3. 删除存储库密钥。

   ```bash
   sudo rm /etc/apt/trusted.gpg.d/dpgswdist.v1.asc.gpg
   ```

4. 删除不再需要的依赖项。

   ```bash
   sudo apt autoremove
   ```

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ver15.md)]？](../../big-data-cluster/big-data-cluster-overview.md)。

将 azdata 用于[已启用 Azure Arc 的数据服务](/azure/azure-arc/data/)
