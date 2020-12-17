---
title: Azure Data Studio 笔记本（Python、R）
description: 了解如何使用 SQL Server 机器学习服务在 Azure Data Studio 中的笔记本中运行 Python 和 R 脚本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: bd7dacd4807e4e779f43d396d9d1e9b19d4134dc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471198"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>使用 SQL Server 机器学习服务在 Azure Data Studio 笔记本中运行 Python 和 R 脚本
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

了解如何使用 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)在 [Azure Data Studio](../../azure-data-studio/what-is.md) 笔记本中运行 Python 和 R 脚本。 Azure Data Studio 是一种跨平台数据库工具。

## <a name="prerequisites"></a>先决条件

- 在工作站计算机上[下载并安装 Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)。 Azure Data Studio 是跨平台的，可在 Windows、macOS 和 Linux 上运行。

- 安装并启用了 SQL Server 机器学习服务的服务器。 可以在 Windows、Linux 或大数据群集上使用机器学习服务：

  - [在 Windows 上安装 SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)。
  - [在 Linux 上安装 SQL Server 机器学习服务](../../linux/sql-server-linux-setup-machine-learning.md)。
  - [通过机器学习服务在 SQL Server 大数据群集上运行 Python 和 R 脚本](../../big-data-cluster/machine-learning-services.md)。

## <a name="create-a-sql-notebook"></a>创建 SQL 笔记本

> [!IMPORTANT]
> 机器学习服务作为 SQL Server 的一部分运行。 因此，你需要使用 SQL 内核而不是 Python 内核。

可以在 Azure Data Studio 中配合使用 SQL 笔记本和机器学习服务。 若要创建新笔记本，请执行以下步骤：

1. 单击“文件”和“新建 Notebook”创建新的笔记本   。 默认情况下，笔记本将使用 SQL 内核  。

1. 单击“附加到”和“更改连接”   。 

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 更改连接](media/ads-attach-to-connection.png)
    
1. 连接到现有的或新的 SQL Server。 可以：

    1. 在“最近的连接”或“保存的连接”下选择现有连接   。

    1. 在“连接详细信息”下创建新连接  。 填写 SQL Server 和数据库的连接详细信息。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 连接详细信息](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>运行 Python 或 R 脚本

SQL Notebook 由代码单元格和文本单元格组成。 代码单元格用于通过存储过程 [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 运行 Python 或 R 脚本。 文本单元格可用于在笔记本中记录代码。

### <a name="run-a-python-script"></a>运行 Python 脚本

遵循以下步骤运行 Python 脚本：

1. 单击“+ 代码”添加代码单元格  。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 添加代码块](media/ads-add-code.png)  

1. 在代码单元格中，输入以下脚本：

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. 单击“运行单元格”（圆形的黑色箭头），或按 F5 运行单个单元格   。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 运行 Python 代码](media/ads-run-python.png)  

1. 结果将显示在代码单元格下方。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook Python 代码输出](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>运行 R 脚本

遵循以下步骤运行 R 脚本：

1. 单击“+ 代码”添加代码单元格  。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 添加代码块](media/ads-add-code.png)  

1. 在代码单元格中，输入以下脚本：

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. 单击“运行单元格”（圆形的黑色箭头），或按 F5 运行单个单元格   。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook 运行 R 代码](media/ads-run-r.png)  

1. 结果将显示在代码单元格下方。

    > [!div class="mx-imgBorder"]
    > ![Azure Data Studio SQL Notebook R 代码输出](media/ads-run-r-output.png)  

## <a name="next-steps"></a>后续步骤

- [如何使用 Azure Data Studio 中的笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [创建和运行 SQL Server 笔记本](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [快速入门：通过 SQL Server 机器学习服务运行简单的 Python 脚本](../tutorials/quickstart-python-create-script.md)
- [快速入门：通过 SQL Server 机器学习服务运行简单的 R 脚本](../tutorials/quickstart-r-create-script.md)