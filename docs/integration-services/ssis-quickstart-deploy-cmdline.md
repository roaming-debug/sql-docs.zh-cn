---
description: 使用 ISDeploymentWizard.exe 从命令提示符部署 SSIS 项目
title: 从命令提示符部署 SSIS 项目 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb78c1ab660ff3ac0cccfdc36779b659ba654474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354258"
---
# <a name="deploy-an-ssis-project-from-the-command-prompt-with-isdeploymentwizardexe"></a>使用 ISDeploymentWizard.exe 从命令提示符部署 SSIS 项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何通过运行 Integration Services 部署向导 (`ISDeploymentWizard.exe`) 从命令提示符部署 SSIS 项目。

有关 Integration Services 部署向导的详细信息，请参阅 [Integration Services 部署向导](packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard)。

## <a name="prerequisites"></a>先决条件

本文中介绍的用于部署到 Azure SQL 数据库的验证需要 SQL Server Data Tools (SSDT) 版本 17.4 或更高版本。 要获取最新版 SSDT，请参阅[下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)。

Azure SQL 数据库服务器在端口 1433 上进行侦听。 如果尝试从企业防火墙内连接到 Azure SQL 数据库服务器，必须在企业防火墙中打开该端口，才能成功连接。

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息将 SSIS 项目部署到以下平台：

-   Windows 上的 SQL Server。

-   Azure SQL 数据库。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息将 SSIS 包部署到 Linux 上的 SQL Server。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="for-azure-sql-database-get-the-connection-info"></a>对于 Azure SQL 数据库，请获取连接信息

要将项目部署到 Azure SQL 数据库，则获取所需的信息以连接到 SSIS 目录数据库 (SSISDB)。 在接下来的步骤中需要完全限定的服务器名称和登录信息。

1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 从左侧的菜单选择“SQL 数据库”，然后选择“SQL 数据库”页中的 SSISDB 数据库。 
3. 在数据库的“概述”页上，查看完全限定的服务器名称。 若想查看“单击以复制”选项，将鼠标悬停在服务器名称上。 
4. 如果忘记了 Azure SQL 数据库服务器登录信息，导航到 SQL 数据库服务器页以查看服务器管理员名称。 如有必要，可重置密码。

## <a name="supported-authentication-method"></a>支持的身份验证方法

请参阅[用于部署的身份验证方法](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment)。

## <a name="start-the-integration-services-deployment-wizard"></a>启动 Integration Services 部署向导
1. 打开“命令提示符”窗口。

2. 运行 `ISDeploymentWizard.exe`。 Integration Services 部署向导随即打开。

    如果包含 `ISDeploymentWizard.exe` 的文件夹不在 `path` 环境变量中，可能需要使用 `cd` 命令将目录更改为该文件夹的目录。 对于 SQL Server 2017，此文件夹通常位于 `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn`。

## <a name="deploy-a-project-with-the-wizard"></a>使用向导部署项目
1. 在向导的“简介”页上查看简介。 单击“下一步”打开“选择源”页。

2. 在“选择源”页上，选择要部署的现有 SSIS 项目。
    -   若要部署通过在开发环境中生成项目而创建的项目部署文件，请选择“项目部署文件”并输入 .ispac 文件的路径。
    -   要部署已部署到 SSIS 目录数据库的项目，请选择“Integration Services 目录”，然后在目录中输入该项目的服务器名称和路径。
    单击“下一步”  以查看“选择目标”  页。
  
3.  在“选择目标”页上，选择项目的目标。
    -   输入完全限定服务器名称。 如果目标服务器是 Azure SQL 数据库服务器，则名称采用以下格式：`<server_name>.database.windows.net`。
    -   提供身份验证信息，然后选择“连接”。 请参阅本文中的[用于部署的身份验证方法](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment)。
    -   然后选择“浏览”，在 SSISDB 中选择目标文件夹。
    -   再选择“下一步”打开“评审”页。 （仅当选择“连接”后，才会启用“下一步”按钮。）

4.  在“查看”页上，查看所选的设置。
    -   可以通过单击 **“上一步”** 或单击左窗格中的任意步骤来更改所做的选择。
    -   单击“部署”  以启动部署过程。

5.  如果正在部署到 Azure SQL 数据库服务器，则会打开“验证”页并检查项目中的包是否存在可能阻止包在 Azure SSIS Integration Runtime 中按预期运行的已知问题。 有关详细信息，请参阅[验证部署到 Azure 的 SSIS 包](lift-shift/ssis-azure-validate-packages.md)。

6.  部署过程完成之后，“结果”页随即打开。 该页显示每个操作是成功了还是失败了。
    -   如果操作失败，则单击“结果”列中的“失败”可显示错误说明。
    -   （可选）单击“保存报告...”以将结果保存到某一 XML 文件。
    -   单击“关闭”退出向导。

## <a name="next-steps"></a>后续步骤
- 考虑部署包的其他方式。
    - [使用 SSMS 部署 SSIS 包](./ssis-quickstart-deploy-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [使用 PowerShell 部署 SSIS 包](ssis-quickstart-deploy-powershell.md)
    - [使用 C# 部署 SSIS 包](./ssis-quickstart-deploy-dotnet.md) 
- 运行已部署的包。 若要运行包，可以从多个工具和语言中进行选择。 有关详细信息，请参阅以下文章：
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md) 
