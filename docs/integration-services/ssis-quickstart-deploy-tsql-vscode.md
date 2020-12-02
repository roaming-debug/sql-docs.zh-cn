---
description: 使用 TRANSACT-SQL 从 Visual Studio Code 部署 SSIS 项目
title: 使用 TRANSACT-SQL (VS Code) 部署 SSIS 项目 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e5ea310eb9054beb5fdab77e589ad9fbc2901cc7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130013"
---
# <a name="deploy-an-ssis-project-from-visual-studio-code-with-transact-sql"></a>使用 TRANSACT-SQL 从 Visual Studio Code 部署 SSIS 项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何使用 Visual Studio Code 连接到 SSIS 目录数据库，然后使用 TRANSACT-SQL 语句将 SSIS 项目部署到 SSIS 目录。

Visual Studio Code 是支持扩展的代码编辑器，适用于 Windows、macOS 和 Linux，其支持的扩展包括用于连接到 Microsoft SQL Server、Azure SQL 数据库或 Azure Synapse Analytics 的 `mssql` 扩展。 有关 VS Code 的详细信息，请参阅 [Visual Studio Code](https://code.visualstudio.com/)。

## <a name="prerequisites"></a>先决条件

在开始之前，请确保已安装最新版本的 Visual Studio Code 并已加载 `mssql` 扩展。 若要下载这些工具，请参阅以下页面：
-   [下载 Visual Studio Code](https://code.visualstudio.com/Download)
-   [mssql 扩展](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息将 SSIS 项目部署到以下平台：

-   Windows 上的 SQL Server。

无法使用此快速入门中的信息将 SSIS 包部署到 Azure SQL 数据库。 `catalog.deploy_project` 存储过程需使用指向本地文件系统中 `.ispac` 文件的路径。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息将 SSIS 包部署到 Linux 上的 SQL Server。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="set-language-mode-to-sql-in-vs-code"></a>在 VS Code 中将语言模式设置为 SQL

若要启用 `mssql` 命令和 T-SQL IntelliSense，请在 Visual Studio Code 中将语言模式设置为 SQL。

1. 打开 Visual Studio Code，然后打开一个新窗口。 

2. 在状态栏的右下角单击“纯文本”。
 
3. 在打开的“选择语言模式”下拉菜单中选择或输入“SQL”，然后按 ENTER 将语言模式设置为 SQL。 

## <a name="supported-authentication-method"></a>支持的身份验证方法

请参阅[用于部署的身份验证方法](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment)。

## <a name="connect-to-the-ssis-catalog-database"></a>连接到 SSIS 目录数据库

使用 Visual Studio Code 来建立到 SSIS 目录的连接。

1. 在 VS Code 中，按 CTRL+SHIFT+P（或 F1）打开命令面板。

2. 键入 sqlcon，然后按 ENTER。

3. 按 **ENTER** 选择“创建连接配置文件”。 本步骤是为 SQL Server 实例创建连接配置文件。

4. 按照提示为新连接配置文件指定连接属性。 指定每个值后，按 **ENTER** 继续。 

   | 设置       | 建议的值 | 更多信息 |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器名称** | 完全限定的服务器名称 |  |
   | **数据库名称** | **SSISDB** | 要连接到的数据库的名称。 |
   | **身份验证** | SQL 登录名 | |
   | **用户名** | 服务器管理员帐户 | 此帐户是在创建服务器时指定的帐户。 |
   | **密码(SQL 登录名)** | 服务器管理员帐户的密码 | 此密码是在创建服务器时指定的密码。 |
   | **是否保存密码？** | 是或否 | 如果不希望每次都输入密码，请选择“是”。 |
   | **输入此配置文件的名称** | 一个配置文件名称，如 mySSISServer | 保存一个配置文件名称可以加快后续登录时的连接速度。 | 

5. 按 ESC 键关闭信息消息，该消息通知你，配置文件已创建并且已连接。

6. 在状态栏中验证连接。

## <a name="run-the-t-sql-code"></a>运行 T-SQL 代码
运行下面的 TRANSACT-SQL 代码以部署 SSIS 项目。

1. 在“编辑器”窗口中，在空查询窗口中输入以下查询。

2. 为系统更新 `catalog.deploy_project` 存储过程中的参数值。

3. 按 CTRL+SHIFT+E 以运行代码并部署项目。

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary = (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>后续步骤
- 考虑部署包的其他方式。
    - [使用 SSMS 部署 SSIS 包](./ssis-quickstart-deploy-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [从命令提示符部署 SSIS 包](./ssis-quickstart-deploy-cmdline.md)
    - [使用 PowerShell 部署 SSIS 包](ssis-quickstart-deploy-powershell.md)
    - [使用 C# 部署 SSIS 包](./ssis-quickstart-deploy-dotnet.md) 
- 运行已部署的包。 若要运行包，可以从多个工具和语言中进行选择。 有关详细信息，请参阅以下文章：
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md) 
