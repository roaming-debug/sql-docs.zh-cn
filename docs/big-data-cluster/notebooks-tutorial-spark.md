---
title: 使用 Spark 运行示例笔记本
titleSuffix: SQL Server big data clusters
description: 本教程介绍如何在 SQL Server 2019 大数据群集上加载并运行示例 Spark 笔记本。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 03/30/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6996e2b9b7705511fc13d479e39e9436a5e054f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044097"
---
# <a name="run-a-sample-notebook-using-spark"></a>使用 Spark 运行示例笔记本

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 上的 Azure Data Studio 中加载和运行笔记本。 数据科学家和数据工程师可针对群集运行 Python、R 或 Scala 代码。

> [!TIP]
> 如果需要，可以下载并运行本教程中的命令脚本。 有关说明，请参阅 GitHub 上的 [Spark 示例](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark)。

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- [大数据工具](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
- [将示例数据加载到大数据群集中](tutorial-load-sample-data.md)

## <a name="download-the-sample-notebook-file"></a>下载示例笔记本文件

按照以下说明将示例笔记本文件 spark-sql.ipynb 加载到 Azure Data Studio 中。

1. 打开 bash 命令提示符 (Linux) 或 Windows PowerShell。

1. 导航到要将示例笔记本文件下载到其中的目录。

1. 运行以下 curl 命令，从 GitHub 下载笔记本文件：

   ```bash
   curl https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/data-loading/transform-csv-files.ipynb -o transform-csv-files.ipynb
   ```

## <a name="open-the-notebook"></a>打开笔记本

以下步骤演示如何在 Azure Data Studio 中打开笔记本文件：

1. 在 Azure Data Studio 中，连接到大数据群集的主实例。 有关详细信息，请参阅[连接到大数据群集](connect-to-big-data-cluster.md)。

1. 双击“服务器”窗口中的 HDFS/Spark 网关连接。 然后选择“打开笔记本”。

   ![打开笔记本](media/notebook-tutorial-spark/azure-data-studio-open-notebook.png)

1. 等待要填充的 Kernel 和目标上下文（“附加到”） 。 将 Kernel 设置为 PySpark3，将“附加到”设置为大数据群集终结点的 IP 地址  。

   ![设置 Kernel 和“附加到”](media/notebook-tutorial-spark/set-kernel-and-attach-to.png)

## <a name="run-the-notebook-cells"></a>运行笔记本单元格

可以通过按单元格左侧的“播放”按钮来运行每个笔记本单元格。 单元格完成运行后，结果会显示在笔记本中。

![运行笔记本单元格](media/notebook-tutorial-spark/run-notebook-cell.png)

连续运行示例笔记本中的每个单元格。 有关结合使用笔记本和 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [如何使用笔记本](../azure-data-studio/notebooks/notebooks-guidance.md)
- [如何管理 Azure Data Studio 中的笔记本](notebooks-manage-bdc.md)

## <a name="next-steps"></a>后续步骤

了解有关笔记本的详细信息：
> [!div class="nextstepaction"]
> [如何使用笔记本](../azure-data-studio/notebooks/notebooks-guidance.md)