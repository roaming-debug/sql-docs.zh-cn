---
title: 在 Azure 中运行 SSIS 包 | Microsoft Docs
description: 概述了用于运行部署到 Azure SQL 数据库的 SSIS 包的可用方法。
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2c7209d43628296169e973012ea459296acc3549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351971"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>运行部署在 Azure 中的 SQL Server Integration Services (SSIS) 包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



可以通过选择本文介绍的某个方法，运行部署到 Azure SQL 数据库服务器上 SSISDB 目录中的 SSIS 包。 可以直接运行包，或将包作为 Azure 数据工厂管道的一部分运行。 有关 Azure 上 SSIS 的概述，请参阅[在 Azure 中部署和运行 SSIS 包](ssis-azure-lift-shift-ssis-packages-overview.md)。

- 直接运行包

  - [使用 SSMS 运行](#ssms)

  - [使用存储过程运行](#sproc)

  - [使用脚本或代码运行](#script)

- 将包作为 Azure 数据工厂管道的一部分运行

  - [使用“执行 SSIS 包”活动运行](#exec_activity)

  - [使用“存储过程”活动运行](#sproc_activity)

> [!NOTE]
> 通过 `dtexec.exe` 运行包的操作尚未经过部署到 Azure 的包的测试。

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a>使用 SSMS 运行包

在 SQL Server Management Studio (SSMS) 中，可以右键单击部署到 SSIS 目录数据库 (SSISDB) 的包，并选择“执行”以打开“执行包”对话框   。 有关详细信息，请参阅[使用 SQL Server Management Studio (SSMS) 运行 SSIS 包](../ssis-quickstart-run-ssms.md)。

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a>使用存储过程运行包

在可连接到 Azure SQL 数据库并运行 Transact-SQL 代码的任何环境中，可以通过调用以下存储过程来运行包：

1. **[catalog].[create_execution]** 。 有关详细信息，请参阅 [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md)。

2. **[catalog].[set_execution_parameter_value]** 。 有关详细信息，请参阅 [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)。

3. **[catalog].[start_execution]** . 有关详细信息，请参阅 [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md)。

有关详细信息，请参阅以下示例：

- [使用 Transact-SQL 从 SSMS 运行 SSIS 包](../ssis-quickstart-run-tsql-ssms.md)

- [使用 Transact-SQL 从 Visual Studio Code 运行 SSIS 包](../ssis-quickstart-run-tsql-vscode.md)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a>使用脚本或代码运行包

在可以调用托管 API 的任何开发环境中，可以通过调用 `Microsoft.SQLServer.Management.IntegrationServices` 命名空间中 `Package` 对象的 `Execute` 方法，运行包。

有关详细信息，请参阅以下示例：

- [使用 PowerShell 运行 SSIS 包](../ssis-quickstart-run-powershell.md)

- [使用 .NET 应用中的 C# 代码运行 SSIS 包](../ssis-quickstart-run-dotnet.md)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a>使用“执行 SSIS 包”活动运行包

有关详细信息，请参阅[在 Azure 数据工厂中使用“执行 SSIS 包”活动运行 SSIS 包](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)。

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a>使用“存储过程”活动运行包

有关详细信息，请参阅[在 Azure 数据工厂中使用存储过程活动运行 SSIS 包](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity)。

## <a name="next-steps"></a>后续步骤

了解用于计划部署到 Azure 的 SSIS 包的选项。 有关详细信息，请参阅[计划 Azure 中的 SSIS 包](ssis-azure-schedule-packages.md)。