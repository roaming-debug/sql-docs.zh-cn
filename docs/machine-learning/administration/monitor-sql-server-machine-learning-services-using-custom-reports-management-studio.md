---
title: 使用自定义报表监视脚本
description: 使用 SQL Server Management Studio (SSMS) 中的自定义报表来监视外部脚本（Python 和 R）的执行情况、使用的资源、诊断问题，以及 SQL Server 机器学习服务中的优化性能。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337075"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>使用 SQL Server Management Studio 中的自定义报表监视 Python 和 R 脚本执行
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

使用 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) 中的自定义报表来监视外部脚本（Python 和 R）的执行情况和资源使用情况，诊断问题，以及优化 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中的性能。

在这些报表中，可以查看以下详细信息：

- 活动 Python 或 R 会话
- 实例的配置设置
- 机器学习作业的执行统计信息
- R Services 的扩展事件
- 当前实例上安装的 Python 或 R 包

本文介绍如何安装和使用为 SQL Server 机器学习服务提供的自定义报表。

若要详细了解 SQL Server Management Studio 中的报表，请参阅 [Management Studio 中的自定义报表](../../ssms/object/custom-reports-in-management-studio.md)。

## <a name="how-to-install-the-reports"></a>如何安装报表

报表使用 SQL Server Reporting Services 进行设计，但可直接通过 SQL Server Management Studio 使用。 无需在 SQL Server 实例上安装 Reporting Services。

若要使用这些报表，请执行以下步骤：

1. 从 GitHub 下载适用于 SQL Server 机器学习服务的 [SSMS 自定义报告](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports)。

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > Azure SQL 托管实例不支持自定义报表“ML 服务 - 配置实例”。
   ::: moniker-end

2. 将报表复制到 Management Studio

    1. 找到 SQL Server Management Studio 使用的自定义报表文件夹。 默认情况下，自定义报表存储在此文件夹中（其中“user_name”是你的 Windows 用户名  ）：

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       还可指定其他文件夹或创建子文件夹。

    2. 将下载的 *.RDL 文件复制到自定义报表文件夹中。

3. 运行 Management Studio 中的报表

    1. 在 Management Studio 中，右键单击要在其中运行报表的示例的“数据库”  节点。

    2. 单击“报表”  ，然后单击“自定义报表”  。

    3. 在“打开文件”  对话框中，找到自定义报表文件夹。

    4. 选择某个下载的 RDL 文件，然后单击“打开”  。

## <a name="reports"></a>报表

[GitHub 中的 SSMS 自定义报告存储库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports)包含以下报表：

| 报表 | 说明 |
|-|-|
| 活动会话 | 当前连接到 SQL Server 实例和运行 Python 或 R 脚本的用户。 |
| 配置 | 机器学习服务的安装设置和 Python 或 R 运行时的属性。 |
| 配置实例 | 配置机器学习服务。 |
| 执行统计信息 | 机器学习作业的执行统计信息。 例如，可以获取外部脚本执行的总数和并行执行数。 |
| 扩展事件 | 可用于更深入地了解外部脚本执行的扩展事件。 |
| 包 | 列出 SQL Server 实例上安装的 R 或 Python 包及其属性，如版本和名称。 |
| 资源使用情况 | 查看 SQL Server 的 CPU、内存、IO 使用情况和外部脚本执行情况。 还可查看外部资源池的内存设置。 |

## <a name="next-steps"></a>后续步骤

- [使用动态管理视图 (DMV) 监视 SQL Server 机器学习服务](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [在 SQL Server 机器学习服务中使用扩展事件监视 Python 和 R 脚本](extended-events.md)
