---
description: 使用 Transact-SQL 从 SSMS 部署 SSIS 项目
title: 使用 Transact-SQL (SSMS) 部署 SSIS 项目 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 609090873281197875eba8cfeb54292df9f79ba9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354231"
---
# <a name="deploy-an-ssis-project-from-ssms-with-transact-sql"></a>使用 Transact-SQL 从 SSMS 部署 SSIS 项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



快速入门将演示如何使用 SQL Server Management Studio (SSMS) 连接到 SSIS 目录数据库，然后使用 Transact-SQL 语句向 SSIS 目录部署 SSIS 项目。 

SQL Server Management Studio 是一种集成环境，用于管理从 SQL Server 到 SQL 数据库的任何 SQL 基础结构。 有关 SSMS 的详细信息，请参阅 [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md)。

## <a name="prerequisites"></a>先决条件

开始之前，请确保具有最新版本的 SQL Server Management Studio。 要下载 SSMS，请参阅[下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息将 SSIS 项目部署到以下平台：

-   Windows 上的 SQL Server。

无法使用此快速入门中的信息将 SSIS 包部署到 Azure SQL 数据库。 `catalog.deploy_project` 存储过程需使用指向本地文件系统中 `.ispac` 文件的路径。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息将 SSIS 包部署到 Linux 上的 SQL Server。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="supported-authentication-method"></a>支持的身份验证方法

请参阅[用于部署的身份验证方法](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment)。

## <a name="connect-to-the-ssis-catalog-database"></a>连接到 SSIS 目录数据库

使用 SQL Server Management Studio 与 SSIS 目录建立连接。 

1. 打开 SQL Server Management Studio。

2. 在“连接到服务器”对话框中，输入以下信息：

   | 设置       | 建议的值 | 更多信息 | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器类型** | 数据库引擎 | 此值是必需的。 |
   | **服务器名称** | 完全限定的服务器名称 |  |
   | **身份验证** | SQL Server 身份验证 | |
   | **登录** | 服务器管理员帐户 | 此帐户是在创建服务器时指定的帐户。 |
   | **密码** | 服务器管理员帐户的密码 | 此密码是在创建服务器时指定的密码。 |

3. 单击“连接”  。 对象资源管理器窗口在 SSMS 中打开。 

4. 在对象资源管理器中，展开“Integration Services 目录”，然后展开“SSISDB”，查看 SSIS 目录数据库中的对象   。


## <a name="run-the-t-sql-code"></a>运行 T-SQL 代码
运行下面的 TRANSACT-SQL 代码以部署 SSIS 项目。

1.  在 SSMS 中，打开新的查询窗口并粘贴以下代码。

2.  为系统更新 `catalog.deploy_project` 存储过程中的参数值。

3.  请确保 SSISDB 是当前数据库。

4.  运行该脚本。

5. 在对象资源管理器中，如有必要，请刷新 SSISDB 的内容，然后检查已部署的项目。

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary =
    (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>后续步骤
- 考虑部署包的其他方式。
    - [使用 SSMS 部署 SSIS 包](./ssis-quickstart-deploy-ssms.md)
    - [使用 Transact-SQL 部署 SSIS 包 (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
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