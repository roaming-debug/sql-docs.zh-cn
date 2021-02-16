---
description: 使用 .NET 应用中的 C# 代码 部署 SSIS 项目
title: 使用 .NET 代码 (C#) 部署 SSIS 项目 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3a5b6cb5c5c8d6afdf6bc1d59259e495ca49f10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354251"
---
# <a name="deploy-an-ssis-project-with-c-code-in-a-net-app"></a>使用 .NET 应用中的 C# 代码 部署 SSIS 项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何写入用于连接数据库服务器和部署 SSIS 项目的 C# 代码。

若要创建 C# 应用，可以使用 Visual Studio、Visual Studio Code 或所选的另一工具。

## <a name="prerequisites"></a>先决条件

开始之前，请确保已安装 Visual Studio 或 Visual Studio Code。 从 [Visual Studio 下载](https://www.visualstudio.com/downloads/)中下载免费的社区版 Visual Studio 或免费的 Visual Studio Code。

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
5. 单击“显示数据库连接字符串”。
6. 查看完整的 ADO.NET 连接字符串。 或者，此代码可使用 `SqlConnectionStringBuilder` 和提供的独立参数值重新创建此连接字符串。

## <a name="supported-authentication-method"></a>支持的身份验证方法

请参阅[用于部署的身份验证方法](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment)。

## <a name="create-a-new-visual-studio-project"></a>创建一个新的 Visual Studio 项目

1. 在 Visual Studio 中，选择“文件”、“新建”、“项目”。 
2. 在“新建项目”对话框中，展开“Visual C#”。
3. 选择“控制台应用”，并在项目名称处输入“deploy_ssis_project”。
4. 单击“确定”以在 Visual Studio 中创建并打开新的项目。

## <a name="add-references"></a>添加引用
1. 在解决方案资源管理器中，右键单击“引用”文件夹并选择“添加引用”。 打开“引用管理器”对话框。
2. 在“引用管理器”对话框中，展开“程序集”并选择“扩展”。
3. 选择以下要添加的两个引用：
    -   Microsoft.SqlServer.Management.Sdk.Sfc
    -   Microsoft.SqlServer.Smo
4. 单击“浏览”按钮以将引用添加到 Microsoft.SqlServer.Management.IntegrationServices 中。 （仅在全局程序集缓存 (GAC) 中安装该程序集。）打开“选择要引用的文件”对话框。
5. 在“选择要引用的文件”对话框中，导航到包含此程序集的 GAC 文件夹。 通常，此文件夹位置为 `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`。
6. 选择文件夹中的程序集（即 .dll 文件），然后单击“添加”。
7. 单击“确定”以关闭“引用管理器”对话框并添加三个引用。 若要确保是三个引用，检查解决方案资源管理器中的“引用”列表。

## <a name="add-the-c-code"></a>添加 C# 代码 
1. 打开 Program.cs。

2. 将 **Program.cs** 的内容替换为以下代码。 为服务器、数据库、用户和密码添加适当的值。

> [!NOTE]
> 下面的示例使用 Windows 身份验证。 要使用 SQL Server 身份验证，请将 `Integrated Security=SSPI;` 参数替换为 `User ID=<user name>;Password=<password>;`。 如果连接到 Azure SQL 数据库服务器，则无法使用 Windows 身份验证。

```csharp
using Microsoft.SqlServer.Management.IntegrationServices;
using System;
using System.Data.SqlClient;
using System.IO;

namespace deploy_ssis_project
{
    class Program
    {
        static void Main(string[] args)
        {
            // Variables
            string targetServerName = "localhost";
            string targetFolderName = "Project1Folder";
            string projectName = "Integration Services Project1";
            string projectFilePath = @"C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac";

            // Create a connection to the server
            string sqlConnectionString = "Data Source=" + targetServerName +
                ";Initial Catalog=master;Integrated Security=SSPI;";
            SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);

            // Create the Integration Services object
            IntegrationServices integrationServices = new IntegrationServices(sqlConnection);

            // Get the Integration Services catalog
            Catalog catalog = integrationServices.Catalogs["SSISDB"];

            // Create the target folder
            CatalogFolder folder = new CatalogFolder(catalog,
                targetFolderName, "Folder description");
            folder.Create();

            Console.WriteLine("Deploying " + projectName + " project.");

            byte[] projectFile = File.ReadAllBytes(projectFilePath);
            folder.DeployProject(projectName, projectFile);

            Console.WriteLine("Done.");
        }
    }
}
```

## <a name="run-the-code"></a>运行代码

1. 若要运行应用程序，请按 **F5**。
2. 在 SSMS 中，验证项目是否已部署。

## <a name="next-steps"></a>后续步骤
- 考虑部署包的其他方式。
    - [使用 SSMS 部署 SSIS 包](./ssis-quickstart-deploy-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [从命令提示符部署 SSIS 包](./ssis-quickstart-deploy-cmdline.md)
    - [使用 PowerShell 部署 SSIS 包](ssis-quickstart-deploy-powershell.md)
- 运行已部署的包。 若要运行包，可以从多个工具和语言中进行选择。 有关详细信息，请参阅以下文章：
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md) 
