---
title: 使用 Jupyter 笔记本和 Azure Data Studio 管理大数据群集 (BDC)
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Jupyter 笔记本和 Azure Data Studio 管理大数据群集 (BDC)。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 34592d9a6cb3db8bde5a2a25098314fd9ace1208
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039477"
---
# <a name="manage-big-data-clusters-bdc-the-cluster-with-notebooks"></a>使用笔记本管理大数据群集 (BDC)

此页是适用于 SQL Server 大数据群集的笔记本的索引。 这些可执行的笔记本 (.ipynb) 旨在用于 SQL Server 2019，以帮助管理大数据群集。

每个笔记本均可检查其自身的依赖项。 “运行所有单元格”要么成功完成，要么抛出异常，并显示指向另一个笔记本的超链接提示，以解决缺少依赖项的问题。 单击提示超链接转到后续的笔记本，按“运行所有单元格”，成功后返回到原始笔记本，再按“运行所有单元格”。

一旦安装了所有依赖项，但“运行所有单元格”失败，每个笔记本就会分析结果，并尽可能生成指向另一个笔记本的超链接提示，以进一步帮助解决问题。


## <a name="installing-and-uninstalling-utilities-on-big-data-cluster-bdc"></a>在大数据群集 (BDC) 上安装和卸载实用工具

此部分包含一组笔记本，用于安装和卸载管理 SQL Server 大数据群集所需的命令行工具和包。

|名称 |说明 |
|---|---|---|---|
|SOP010 - 升级大数据群集|使用此笔记本来通过 azdata 升级大数据群集。 |
|SOP012 - 安装适用于 Mac 的 unixodbc|如果在使用 brew 安装适用于 SQL Server 的 odbc 时出错，则使用此笔记本。|
|SOP036 - 安装 kubectl 命令行接口|使用此笔记本来安装 kubectl 命令行接口，而不用考虑你的 OS。|
|SOP037 - 卸载 kubectl 命令行接口|使用此笔记本来卸载 kubectl 命令行接口，而不用考虑你的 OS。|
|SOP038 - 安装 Azure 命令行接口|使用此笔记本来安装 Azure CLI 命令行接口，而不用考虑你的 OS。|
|SOP039 - 卸载 Azure 命令行接口|使用此笔记本来卸载 Azure CLI 命令行接口，而不用考虑你的 OS。|
|SOP040 - 在 ADS Python 沙盒中升级 pip|使用此笔记本在 ADS Python 沙盒中升级 pip。|
|SOP054 - 安装 azdata 命令行接口|使用此笔记本来安装 azdata 命令行接口，而不用考虑你的 OS。|
|SOP055 - 卸载 azdata 命令行接口|使用此笔记本来卸载 azdata 命令行接口，而不用考虑你的 OS。|
|SOP059 - 安装 Kubernetes Python 模块|使用此笔记本来通过 Python 安装 Kubernetes 模块。|
|SOP060 - 卸载 kubernetes 模块|使用此笔记本来通过 Python 卸载 Kubernetes 模块。|
|SOP061 - 删除大数据群集|使用此笔记本来删除 BDC 群集。|
|SOP062 - 安装 python-sql 和 pyodbc 模块|使用此笔记本来安装 ipython-sql 和 pyodbc 模块。|
|SOP063 - 安装 azdata CLI（使用包管理器）|使用此笔记本来安装 azdata CLI（使用包管理器）。|
|SOP064 - 卸载 azdata CLI（使用包管理器）|使用此笔记本来卸载 azdata CLI（使用包管理器）。|
|SOP069 - 安装 ODBC for SQL Server|使用此笔记本来安装 ODBC 驱动程序，因为 azdata 中的某些子命令需要 SQL Server ODBC 驱动程序。|


## <a name="managing-certificates-on-big-data-clusters-bdc"></a>管理大数据群集 (BDC) 上的证书

一组笔记本，用于运行笔记本来管理大数据群集上的证书。

|名称 |说明 |
|---|---|---|---|
|CER001 - 生成根 CA 证书|生成根 CA 证书。 考虑在每个环境中对所有非生产群集使用一个根 CA 证书，因为这种技术减少了需要上传到连接到这些群集的客户端的根 CA 证书的数量。 |
|CER002 - 下载现有根 CA 证书|使用此笔记本从群集下载已生成的根 CA 证书。|
|CER003 - 上传现有根 CA 证书|CER003 - 上传现有根 CA 证书。|
|CER004 - 下载和上传现有根 CA 证书|下载和上传现有根 CA 证书。 |
|CER010 - 在本地安装已生成的根 CA|此笔记本会在本地（从大数据群集）复制使用“CER001 - 生成根 CA 证书”或“CER003 - 上传现有根 CA 证书”安装的已生成的根 CA 证书，然后将根 CA 证书安装到此计算机的本地证书存储中。|
|CER020 - 创建 Management Proxy 证书|此笔记本为 Management Proxy 终结点创建证书。|
|CER021 - 创建 Knox 证书|此笔记本为 Knox Gateway 终结点创建证书。|
|CER022 - 创建 App Proxy 证书|此笔记本为 App Deploy Proxy 终结点创建证书。|
|CER023 - 创建 Master 证书|此笔记本为 Master 终结点创建证书。|
|CER030 - 使用已生成的 CA 对 Management Proxy 证书进行签名|此笔记本使用通过“CER001 - 生成根 CA 证书”或“CER003 - 上传现有根 CA 证书”生成的根 CA 证书，对使用“CER020 - 创建 Management Proxy 证书”创建的证书进行签名|
|CER031 - 使用已生成的 CA 对 Knox 证书进行签名|此笔记本使用通过“CER001 - 生成根 CA 证书”或“CER003 - 上传现有根 CA 证书”生成的根 CA 证书，对使用“CER021 - 创建 Knox 证书”创建的证书进行签名|
|CER032 - 使用已生成的 CA 对 App-Proxy 证书进行签名|此笔记本使用通过“CER001 - 生成根 CA 证书”或“CER003 - 上传现有根 CA 证书”生成的根 CA 证书，对使用“CER022 - 创建 App Proxy 证书”创建的证书进行签名。|
|CER033 - 使用已生成的 CA 对 Master 证书进行签名|此笔记本使用通过“CER001 - 生成根 CA 证书”或“CER003 - 上传现有根 CA 证书”生成的根 CA 证书，对使用“CER023 - 创建 Master 证书”创建的证书进行签名。|
|CER040 - 安装已签名的 Management Proxy 证书|此笔记本将使用“CER030 - 使用已生成的 CA 对 Management Proxy 证书进行签名”签名的证书安装到大数据群集中。|
|CER041 - 安装已签名的 Knox 证书|此笔记本将使用“CER031 - 使用已生成的 CA 对 Knox 证书进行签名”签名的证书安装到大数据群集中。|
|CER042 - 安装已签名的 App-Proxy 证书|此笔记本将使用“CER032 - 使用已生成的 CA 对 App-Proxy 证书进行签名”签名的证书安装到大数据群集中。|
|CER043 - 安装已签名的 Controller 证书|此笔记本将使用“CER034 - 使用群集根 CA 对 Controller 证书进行签名”签名的证书安装到大数据群集中。请注意，在此笔记本的最后，控制器 Pod 和所有使用 PolyBase 的 Pod（主池和计算池 Pod）将重启来加载新证书。|
|CER050 - 等待 BDC 恢复正常状态|在控制器 Pod 和使用 PolyBase 的 Pod 已重启来加载新证书后，此笔记本将等待大数据群集恢复正常状态。|
|CER100 - 为群集配置自签名证书|此笔记本会在大数据群集中生成新的根 CA，并为每个终结点新建证书（这些终结点是：Management、Gateway、App-Proxy 和 Controller）。 使用已生成的新根 CA 对每个新证书进行签名（Controller 证书除外，此类证书是使用现有群集根 CA 进行签名），然后将每个证书安装到大数据群集中。 将已生成的新根 CA 下载到此计算机的“受信任的根证书颁发机构”证书存储中。 所有已生成的自签名证书都将存储在 test_cert_store_root 位置处的控制器 Pod 中。|
|CER101 - 使用现有根 CA 为群集配置自签名证书|此笔记本会使用大数据群集中已生成的现有根 CA（使用 CER003 进行上传），并为每个终结点（Management、Gateway、App-Proxy 和 Controller）新建证书，然后使用已生成的新根 CA 对每个新证书进行签名（Controller 证书除外，此类证书是使用现有群集根 CA 进行签名），将每个证书安装到大数据群集中。 所有已生成的自签名证书都将存储在 test_cert_store_root 位置处的控制器 Pod 中。 在完成此笔记本后，从此计算机（以及任何安装了新根 CA 的计算机）对大数据群集进行的所有 https:// 访问都将显示为安全。 “笔记本运行程序”一章将确保创建用于运行 App-Deploy 的 CronJob (OPR003)，并将安装群集根 CA，以允许安全获取 JWT 令牌和 swagger.json。|

## <a name="backup-and-restore-from-big-data-cluster-bdc"></a>从大数据群集 (BDC) 备份和还原

此部分包含一组笔记本，用于对 SQL Server 大数据群集执行备份和还原操作。

| 名称 | 说明 |
|--|--|
| SOP008 - 使用 distcp 将 HDFS 文件备份到 Azure Data Lake Store Gen2 | 此标准操作过程 (SOP) 将数据从源 SQL Server 2019 BDC 群集的 HDFS 文件系统备份到你指定的 Azure Data Lake Store Gen2 帐户。 请确保已将 Azure Data Lake Store Gen2 帐户配置为启用了“分层命名空间”。 |

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
