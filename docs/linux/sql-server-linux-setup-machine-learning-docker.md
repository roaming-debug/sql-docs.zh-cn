---
title: 在 Docker 上安装
titleSuffix: SQL Server Machine Learning Services
description: 了解如何在 Docker 上安装 SQL Server 机器学习服务（Python 和 R）。
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 05/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: e53acfa9e0fdfb7f7ee9b1ab6b7de5cd43ba8a9d
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870035"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-docker"></a>在 Docker 上安装 SQL Server 机器学习服务（Python 和 R）

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

本文介绍如何在 Docker 上安装 SQL Server 机器学习服务。 可使用机器学习服务在数据库中执行 Python 和 R 脚本。 我们不为预建容器提供机器学习服务。 你可以使用 [GitHub 上的可用示例模板](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices)在 SQL Server 容器中创建一个。

## <a name="prerequisites"></a>先决条件

- Git 命令行接口。

- 任何受支持的 Linux 发行版本上的 Docker 引擎 1.8 及更高版本，或用于 Mac/Windows 的 Docker。 有关详细信息，请参阅[了解 Docker](https://docs.docker.com/get-docker/)。

- 另请参阅 [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

## <a name="clone-the-mssql-docker-repository"></a>克隆 mssql-docker 存储库

以下命令将 `mssql-docker` git 存储库克隆到本地目录。

1. 打开 Linux 或 Mac 上的 Bash 终端。

2. 创建一个目录来保存 mssql-docker 存储库的本地副本。

3. 运行 git clone 命令，以克隆 mssql-docker 存储库：

    ```bash
    git clone https://github.com/microsoft/mssql-docker mssql-docker
    ```

## <a name="build-a-sql-server-linux-container-image"></a>构建 SQL Server Linux 容器映像

完成以下步骤以生成 docker 映像：

1. 将目录更改为 mssql-mlservices 目录：
    
    ```bash
    /mssql-docker/linux/preview/examples/mssql-mlservices
    ```

2. 在同一目录中，运行以下命令：

    ```bash
    docker build -t mssql-server-mlservices .
    ```

3. 运行以下命令：

    ```bash
    docker run -d -e MSSQL_PID=Developer -e ACCEPT_EULA=Y -e ACCEPT_EULA_ML=Y -e MSSQL_SA_PASSWORD=<password> -v <directory on the host OS>:/var/opt/mssql -p 1433:1433 mssql-server-mlservices
    ```
  
    > [!NOTE]
    > 可以为 MSSQL_PID 使用以下任一值：Developer（免费）、Express（免费）、Enteprise（付费）、Standard（付费）。 如果使用付费版，请确保已购买许可证。 将（密码）替换为实际密码。 使用 -v 的卷装载是可选的。 将（主机操作系统上的目录）替换为要在其中装载数据库数据和日志文件的实际目录。
    

4. 通过运行以下命令进行确认：

    ```bash
    docker ps -a
    ```

   > [!NOTE]
   > 必须安装几个 GB 大小的包，才能生成 Docker 映像。 此脚本可能需要一段时间才能完成运行，具体视网络带宽而定。

## <a name="run-the-sql-server-linux-container-image"></a>运行 SQL Server Linux 容器映像

1. 先设置环境变量，再运行容器。 将 PATH_TO_MSSQL 环境变量设置为主机目录：

   ```bash
   export MSSQL_PID='Developer'
   export ACCEPT_EULA='Y'
   export ACCEPT_EULA_ML='Y'
   export PATH_TO_MSSQL='/home/mssql/'
   ```
  
   > [!NOTE]
   > 在容器中运行 SQL Server 生产版本的过程略有不同。 有关详细信息，请参阅 [在 Docker 上配置 SQL Server 容器映像](./sql-server-linux-docker-container-deployment.md)。 如果使用相同的容器名称和端口，本教程的其余部分仍适用于生产容器。

2. 若要查看 Docker 容器，请运行 `docker ps` 命令：

   ```bash
   sudo docker ps -a
   ```

3. 如果“状态”列显示“正常运行”状态，表明 SQL Server 正在容器中运行，且正在侦听“端口”列中指定的端口。 如果 SQL Server 容器的“状态”列显示“已退出”，则参阅[配置指南的疑难解答部分](./sql-server-linux-docker-container-troubleshooting.md) 。

 
    输出：

    ```
    CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
    941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour     0.0.0.0:1401->1433/tcp   sql1
    ```

## <a name="enable-machine-learning-services"></a>启用机器学习服务

若要启用机器学习服务，请连接到 SQL Server 实例，并运行以下 T-SQL 语句：

```sql
EXEC sp_configure  'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE
```

## <a name="next-steps"></a>后续步骤

Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 一起使用：

+ [Python 教程：在 SQL Server 机器学习服务中使用线性回归来预测雪橇租赁](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python 教程：配合使用 K-Means 群集和 SQL Server 机器学习服务对客户进行分类](../machine-learning/tutorials/python-clustering-model.md)

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [教程：适用于 R 开发人员的数据库内分析](../machine-learning/tutorials/r-taxi-classification-introduction.md)