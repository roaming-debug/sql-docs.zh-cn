---
description: 使用 Transact-SQL 运行来自 SSMS 的 SSIS 包
title: 使用 Transact-SQL (SSMS) 运行 SSIS 包 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b45f89b10e9e5405e7f85c662120f2cf04d7d02
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336326"
---
# <a name="run-an-ssis-package-from-ssms-with-transact-sql"></a>使用 Transact-SQL 运行来自 SSMS 的 SSIS 包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何使用 SQL Server Management Studio (SSMS) 连接到 SSIS 目录数据库，然后使用 Transact-SQL 语句运行存储在 SSIS 目录中的 SSIS 包。

SQL Server Management Studio 是一种集成环境，用于管理从 SQL Server 到 SQL 数据库的任何 SQL 基础结构。 有关 SSMS 的详细信息，请参阅 [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md)。

## <a name="prerequisites"></a>先决条件

开始之前，请确保有最新版本的 SQL Server Management Studio (SSMS)。 要下载 SSMS，请参阅[下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)。

Azure SQL 数据库服务器在端口 1433 上进行侦听。 如果尝试从企业防火墙内连接到 Azure SQL 数据库服务器，必须在企业防火墙中打开该端口，才能成功连接。

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息在以下平台上运行 SSIS 包：

-   Windows 上的 SQL Server。

-   Azure SQL 数据库。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息在 Linux 上运行 SSIS 包。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="for-azure-sql-database-get-the-connection-info"></a>对于 Azure SQL 数据库，请获取连接信息

要在 Azure SQL 数据库上运行包，请获取连接到 SSIS 目录数据库 (SSISDB) 所需的连接信息。 在接下来的步骤中需要完全限定的服务器名称和登录信息。

1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 从左侧的菜单选择“SQL 数据库”，然后选择“SQL 数据库”页中的 SSISDB 数据库。 
3. 在数据库的“概述”页上，查看完全限定的服务器名称。 若想查看“单击以复制”选项，将鼠标悬停在服务器名称上。 
4. 如果忘记了 Azure SQL 数据库服务器登录信息，导航到 SQL 数据库服务器页以查看服务器管理员名称。 如有必要，可重置密码。

## <a name="connect-to-the-ssisdb-database"></a>连接到 SSISDB 数据库

使用 SQL Server Management Studio 来建立到 Azure SQL 数据库服务器上的 SSIS 目录的连接。 

1. 打开 SQL Server Management Studio。

2. 在“连接到服务器”对话框中，输入以下信息：

   | 设置       | 建议的值 | 更多信息 | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器类型** | 数据库引擎 | 此值是必需的。 |
   | **服务器名称** | 完全限定的服务器名称 | 如果要连接到 Azure SQL 数据库服务器，该名称为以下格式：`<server_name>.database.windows.net`。 |
   | **身份验证** | SQL Server 身份验证 | 使用 SQL Server 身份验证，可连接到 SQL Server 或 Azure SQL 数据库。 如果连接到 Azure SQL 数据库服务器，则无法使用 Windows 身份验证。 |
   | **登录** | 服务器管理员帐户 | 此帐户是在创建服务器时指定的帐户。 |
   | **密码** | 服务器管理员帐户的密码 | 此密码是在创建服务器时指定的密码。 |

3.  单击“连接”  。 对象资源管理器窗口在 SSMS 中打开。

4. 在对象资源管理器中，展开“Integration Services 目录”，然后展开“SSISDB”，查看 SSIS 目录数据库中的对象   。

## <a name="run-a-package"></a>运行包
运行以下 Transact-SQL 代码以便运行 SSIS 包。

1.  在 SSMS 中，打开新的查询窗口并粘贴以下代码。 （此代码是由 SSMS “执行包”对话框中的“脚本”选项生成的代码。）

2.  为系统更新 `catalog.create_execution` 存储过程中的参数值。

3.  请确保 SSISDB 是当前数据库。

4.  运行该脚本。

5. 在对象资源管理器中，如有必要，请刷新 SSISDB 的内容，然后检查已部署的项目。

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>后续步骤
- 考虑运行包的其他方式。
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [从命令提示符运行 SSIS 包](./ssis-quickstart-run-cmdline.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md)