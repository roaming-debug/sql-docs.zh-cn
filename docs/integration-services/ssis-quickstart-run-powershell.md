---
description: 使用 PowerShell 运行 SSIS 包
title: 使用 PowerShell 运行 SSIS 包 | Microsoft Docs
ms.date: 09/17/2020
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0f82c5746f456fe03b3fdca21b0297fbcc63d0d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348215"
---
# <a name="run-an-ssis-package-with-powershell"></a>使用 PowerShell 运行 SSIS 包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何使用 PowerShell 脚本连接到数据库服务器并运行 SSIS 包。

## <a name="prerequisites"></a>先决条件

Azure SQL 数据库服务器在端口 1433 上进行侦听。 如果尝试从企业防火墙内连接到 Azure SQL 数据库服务器，必须在企业防火墙中打开该端口，才能成功连接。

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息在以下平台上运行 SSIS 包：

-   Windows 上的 SQL Server。

-   Azure 数据工厂 (ADF) 中的 SSIS 集成运行时 (IR)，其中 SSIS 目录 (SSISDB) 由 Azure SQL 托管实例 (MI) 承载。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息在 Linux 上运行 SSIS 包。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="for-azure-sql-database-get-the-connection-info"></a>对于 Azure SQL 数据库，请获取连接信息

要在 Azure SQL 数据库上运行包，请获取连接到 SSIS 目录数据库 (SSISDB) 所需的连接信息。 在接下来的步骤中需要完全限定的服务器名称和登录信息。

1. 登录 [Azure 门户](https://portal.azure.com/)。
2. 从左侧的菜单选择“SQL 数据库”，然后选择“SQL 数据库”页中的 SSISDB 数据库。 
3. 在数据库的“概述”页上，查看完全限定的服务器名称。 若想查看“单击以复制”选项，将鼠标悬停在服务器名称上。 
4. 如果忘记了 Azure SQL 数据库服务器登录信息，导航到 SQL 数据库服务器页以查看服务器管理员名称。 如有必要，可重置密码。
5. 单击“显示数据库连接字符串”。
6. 查看完整的 ADO.NET 连接字符串。

## <a name="ssis-powershell-provider"></a>SSIS PowerShell 提供程序
可以使用 SSIS PowerShell 提供程序连接到 SSIS 目录并在其中执行包。

下面是如何使用 SSIS PowerShell 提供程序在包目录中执行 SSIS 包的基本示例。

```powershell
(Get-ChildItem SQLSERVER:\SSIS\localhost\Default\Catalogs\SSISDB\Folders\Project1Folder\Projects\'Integration Services Project1'\Packages\ |
WHERE { $_.Name -eq 'Package.dtsx' }).Execute("false", $null)
```

## <a name="powershell-script"></a>PowerShell 脚本
为以下脚本顶部的变量提供相应的值，然后运行脚本以运行 SSIS 包。

> [!NOTE]
> 下面的示例使用 Windows 身份验证。 要使用 SQL Server 身份验证，请将 `Integrated Security=SSPI;` 参数替换为 `User ID=<user name>;Password=<password>;`。 如果连接到 Azure SQL 数据库服务器，则无法使用 Windows 身份验证。 

```powershell
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectName = "Integration Services Project1"
$PackageName = "Package.dtsx"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, "+
    "Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = `
    "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection

# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]

# Get the folder
$folder = $catalog.Folders[$TargetFolderName]

# Get the project
$project = $folder.Projects[$ProjectName]

# Get the package
$package = $project.Packages[$PackageName]

Write-Host "Running " $PackageName "..."

$result = $package.Execute("false", $null)

Write-Host "Done."
```

## <a name="next-steps"></a>后续步骤
- 考虑运行包的其他方式。
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md) 
