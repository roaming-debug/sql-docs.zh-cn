---
title: 脱机部署
titleSuffix: SQL Server big data clusters
description: 了解如何执行 SQL Server 2019 大数据群集的脱机部署，以及如何将容器映像加载到专用存储库。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9cbe53718818116ae9a0c435a1cf985c132f759b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047273"
---
# <a name="perform-an-offline-deployment-of-a-sql-server-big-data-cluster"></a>执行 SQL Server 大数据群集的脱机部署

本文介绍如何执行 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的脱机部署。 大数据群集必须有权访问从中拉取容器映像的 Docker 存储库。 脱机安装是指将所需的映像置于专用 Docker 存储库中。 然后，该专用存储库会用作新部署的映像源。

## <a name="prerequisites"></a>必备条件

- 任何受支持的 Linux 分发或用于 Mac/Windows 的 Docker 上的 Docker 引擎 1.8+。 有关详细信息，请参阅 [Install Docker](https://docs.docker.com/engine/installation/)（安装 Docker）。

## <a name="load-images-into-a-private-repository"></a>将映像加载到专用存储库

以下步骤介绍了如何从 Microsoft 存储库中拉取大数据群集容器映像，然后将其推送到专用存储库中。

> [!TIP]
> 以下步骤说明这一过程。 但是，为简化任务，可以使用[自动化脚本](#automated)，而不是手动运行这些命令。

1. 重复以下命令即可拉取大数据群集容器映像。 将 `<SOURCE_IMAGE_NAME>` 替换为每个[映像名称](#images)。 将 `<SOURCE_DOCKER_TAG>` 替换为大数据群集版本的标记，例如 2019-GDR1-ubuntu-16.04  。  

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG>
   ```

1. 登录到目标专用 Docker 注册表。

   ```PowerShell
   docker login <TARGET_DOCKER_REGISTRY> -u <TARGET_DOCKER_USERNAME> -p <TARGET_DOCKER_PASSWORD>
   ```

1. 使用以下命令为每个映像标记本地映像：

   ```PowerShell
   docker tag mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG> <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```

1. 将本地映像推送到专用 Docker 存储库：

   ```PowerShell
   docker push <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```
 
> [!WARNING]
> 将大数据群集映像推送到专用存储库后，请不要对这些映像进行修改。 使用修改后的映像执行部署将导致不受支持的大数据群集设置。


### <a name="big-data-cluster-container-images"></a><a id="images"></a> 大数据群集容器映像

脱机安装需要以下大数据群集容器映像：
- **mssql-service-proxy**
- **mssql-control-watchdog**
- **mssql-controller**
- **mssql-dns**
- **mssql-hadoop**
- **mssql-mleap-serving-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-monitor-collectd**
- **mssql-monitor-elasticsearch**
- **mssql-monitor-fluentbit**
- **mssql-monitor-grafana**
- **mssql-monitor-influxdb**
- **mssql-monitor-grafana**
- **mssql-monitor-telegraf**
- **mssql-security-knox**
- **mssql-security-support**
- **mssql-server-controller**
- **mssql-server-data**
- **mssql-ha-operator**
- **mssql-ha-supervisor**
- **mssql-service-proxy**
- **mssql-ssis-app-runtime**


## <a name="automated-script"></a><a id="automated"></a> 自动化脚本

可以使用自动化 Python 脚本，该脚本将自动拉取所有必需的容器映像，并将其推送到专用存储库中。

> [!NOTE]
> 使用该脚本的先决条件是 Python。 有关如何安装 Python 的详细信息，请参阅 [Python 文档](https://wiki.python.org/moin/BeginnersGuide/Download)。

1. 使用 curl 从 Bash 或 PowerShell 下载脚本：

   ```PowerShell
   curl -o push-bdc-images-to-custom-private-repo.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/offline/push-bdc-images-to-custom-private-repo.py"
   ```

1. 然后，使用下列其中一个命令运行该脚本：

   **Windows：**

   ```PowerShell
   python push-bdc-images-to-custom-private-repo.py
   ```

   **Linux：**

   ```bash
   sudo python push-bdc-images-to-custom-private-repo.py
   ```

1. 按照提示输入 Microsoft 存储库和专用存储库信息。 完成脚本后，所有必需的映像都应位于专用存储库中。

1. 请按照[此处](deployment-custom-configuration.md#docker)的说明了解如何自定义 `control.json` 部署配置文件，以使用容器注册表和存储库。 请注意，必须在部署之前设置 `DOCKER_USERNAME` 和 `DOCKER_PASSWORD` 环境变量，以启用对专用存储库的访问权限。

## <a name="install-tools-offline"></a>脱机安装工具

大数据群集部署需要多种工具，包括 Python、[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 和 Kubectl 。 通过下列步骤在脱机服务器上安装这些工具。

### <a name="install-python-offline"></a><a id="python"></a> 脱机安装 python

1. 在具有 Internet 访问权限的计算机上，下载以下包含 Python 的压缩文件之一：

   | 操作系统 | 下载 |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. 将压缩文件复制到目标计算机，并将其解压缩到所选文件夹中。

1. （仅适用于 Windows）从该文件夹运行 `installLocalPythonPackages.bat`，并将完整路径作为参数传递到同一文件夹。

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

### <a name="install-azdata-offline"></a><a id="azdata"></a> 脱机安装 azdata

1. 在具有 Internet 连接和 [Python](https://wiki.python.org/moin/BeginnersGuide/Download) 的计算机上运行以下命令，将所有 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 包下载到当前文件夹。

   ```PowerShell
   pip download -r https://aka.ms/azdata
   ```

1. 将下载的包和 `requirements.txt` 文件复制到目标计算机。

1. 在目标计算机上运行以下命令，指定将之前的文件复制到其中的文件夹。

   ```PowerShell
   pip install --no-index --find-links <path-to-packages> -r <path-to-requirements.txt>
   ```

### <a name="install-kubectl-offline"></a><a id="kubectl"></a> 脱机安装 Kubectl

若要将 Kubectl 安装到脱机计算机，请使用以下步骤。

1. 使用 curl 将 Kubectl 下载到所选文件夹 。 有关详细信息，请参阅[使用 curl 安装 Kubectl 二进制](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl)。

1. 将文件夹复制到目标计算机。

## <a name="deploy-from-private-repository"></a>从专用存储库进行部署

若要从专用存储库进行部署，请使用[部署指南](deployment-guidance.md)中所述的步骤，但使用指定专用 Docker 存储库信息的自定义部署配置文件。 以下 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令演示如何更改名为 `control.json` 的自定义部署配置文件中的 Docker 设置：

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.repository=<your-docker-repository>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.registry=<your-docker-registry>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.imageTag=<your-docker-image-tag>"
```

部署会提示你提供 Docker 用户名和密码，也可在 `DOCKER_USERNAME` 和 `DOCKER_PASSWORD` 环境变量中指定用户名和密码。

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅 [如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。
