---
title: RevoScaleR 深入探讨教程
description: 在本教程系列中，了解如何使用 SQL Server 机器学习 R 集成调用 RevoScaleR 函数。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9d7300acb587dd25c294918fe5ae3a6d6a90ddbc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470568"
---
# <a name="tutorial-use-revoscaler-r-functions-with-sql-server-data"></a>教程：对 SQL Server 数据使用 RevoScaleR R 函数
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

此多部分教程系列介绍了一系列 RevoScaleR 函数，可用于与数据科学相关的任务  。 在此过程中，你将了解如何创建远程计算上下文、在本地和远程计算上下文之间移动数据以及在远程 SQL Server 上执行 R 代码。 还将了解如何在本地和远程服务器上分析和绘制数据，以及如何创建和部署模型。

[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) 是 Microsoft R 包，为数据科学和机器学习工作负荷提供分布式的并行处理。 对于 SQL Server 中的 R 开发，RevoScaleR 是核心内置包之一，具有用于创建数据源对象、设置计算上下文、管理包的函数，最重要的是端对端地处理数据（从导入到可视化和分析）  。 SQL Server 中的机器学习算法依赖于 RevoScaleR 数据源  。 考虑到 RevoScaleR 的重要性，了解调用其函数的时机和方法是一项基本技能  。 

## <a name="prerequisites"></a>先决条件

+ 具有 R 功能的 [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)或 [SQL Server R Services（数据库内）](../install/sql-r-services-windows-install.md)
  
+ [数据库权限](../security/user-permission.md)和 SQL Server 数据库用户登录名

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ IDE（如 RStudio）或 R 附带的内置 RGUI 工具

若要在本地和远程计算上下文之间来回切换，则需要两个系统。 本地上下文通常是一个开发工作站，具有足够强大的数据科学工作负载。 在这种情况下，远程上下文是启用 R 功能 SQL Server。 

切换计算上下文的前提是在本地和远程系统上都具有相同版本的 RevoScaleR  。 在本地工作站上，可以通过安装 Microsoft R Client 获取 RevoScaleR 包和相关的提供程序  。

如果需要将客户端和服务器放在同一台计算机上，请确保安装另一组 Microsoft R 库，以便从“远程”客户端发送 R 脚本。 请勿使用安装在 SQL Server 实例的程序文件中的 R 库。 具体而言，如果使用一台计算机，则需要在这两个位置都使用 RevoScaleR 库，以支持客户端和服务器操作  。

+ C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

有关客户端配置的说明，请参阅[设置用于 R 开发的数据科学客户端](../r/set-up-a-data-science-client.md)。


## <a name="r-development-tools"></a>R 开发工具

R 开发人员通常使用 IDE 来编写和调试 R 代码。 一些建议如下：

- 适用于 Visual Studio 的 R 工具 (RTVS) 是一款免费插件，为 Microsoft R 提供 Intellisense、调试和支持。可以将其与 R Server 和 SQL Server 机器学习服务一起使用  。 若要下载，请参阅 [R Tools for Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019)。

- **RStudio** 是广受欢迎的 R 开发环境之一。 有关详细信息，请参阅 [https://www.rstudio.com/products/RStudio/](https://www.rstudio.com/products/RStudio/)。

- 在 SQL Server 或 R Client 中安装 R 时，也默认安装基础 R 工具（R.exe、RTerm.exe、RScripts.exe）。 如果不想安装 IDE，可以使用内置 R 工具来执行本教程中的代码。

在本地计算机和远程计算机上都需要回调 RevoScaleR  。 若使用 RStudio 的通用安装或缺少 Microsoft R 库的其他环境，则无法完成本教程。 有关详细信息，请参阅 [设置数据科学客户端](../r/set-up-a-data-science-client.md)。

## <a name="summary-of-tasks"></a>任务摘要

+ 数据最初是从 CSV 文件或 XDF 文件中获取的。 使用 RevoScaleR 包中的函数将数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  。
+ 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算上下文执行模型定型和评分操作。 
+ 使用 RevoScaleR 函数创建新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表以保存评分结果  。
+ 在服务器上和本地计算上下文中创建图表。
+ 基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据定型模型，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中运行 R。
+ 提取数据的子集，并在本地工作站上将其保存为 XDF 文件，以便在分析中重复使用。
+ 开启到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的 ODBC 连接，获取用于评分的新数据。 评分是在本地工作站上完成的。
+ 创建自定义 R 函数并在服务器计算上下文中运行，以执行模拟。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程 1：创建数据库和权限](deepdive-work-with-sql-server-data-using-r.md)