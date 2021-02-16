---
description: 使用 DTExec.exe 从命令提示符运行 SSIS 包
title: 从命令提示符运行 SSIS 包 | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 36743c266004cf628f7462ed8220c6f72914cd45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354276"
---
# <a name="run-an-ssis-package-from-the-command-prompt-with-dtexecexe"></a>使用 DTExec.exe 从命令提示符运行 SSIS 包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本快速入门演示如何通过运行包含相应参数的 `DTExec.exe`，从命令提示符处运行 SSIS 包。

> [!NOTE]
> 本文介绍的方法尚未使用部署到 Azure SQL 数据库服务器的包进行测试。

有关 `DTExec.exe` 的详细信息，请参阅 [dtexec 实用工具](./packages/dtexec-utility.md)。

## <a name="supported-platforms"></a>受支持的平台

可使用此快速入门中的信息在以下平台上运行 SSIS 包：

-   Windows 上的 SQL Server。

本文介绍的方法尚未使用部署到 Azure SQL 数据库服务器的包进行测试。 有关在 Azure 中部署和运行包的详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

无法使用此快速入门中的信息在 Linux 上运行 SSIS 包。 有关在 Linux 上运行包的详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

## <a name="run-a-package-with-dtexec"></a>使用 dtexec 运行包

如果包含 `DTExec.exe` 的文件夹不在 `path` 环境变量中，可能需要使用 `cd` 命令将目录更改为该文件夹的目录。 对于 SQL Server 2017，此文件夹通常位于 `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn`。

使用下面的示例中所用的参数值，程序会运行 SSIS 服务器（即承载 SSIS 目录数据库 (SSISDB) 的服务器）上指定的文件夹路径中的包。 `/Server` 参数提供服务器名称。 程序以当前用户的身份使用 Windows 集成身份验证进行连接。 若要使用 SQL 身份验证，请使用合适的值指定 `/User` 和 `Password` 参数。

1. 打开命令提示符窗口。

2. 如下面的示例中所示，运行 `DTExec.exe` 并至少提供 `ISServer` 和 `Server` 参数的值：

    ```cmd
    dtexec /ISServer "\SSISDB\Project1Folder\Integration Services Project1\Package.dtsx" /Server "localhost"
    ```

## <a name="next-steps"></a>后续步骤
- 考虑运行包的其他方式。
    - [使用 SSMS 运行 SSIS 包](./ssis-quickstart-run-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [使用 Transact-SQL 运行 SSIS 包 (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [使用 PowerShell 运行 SSIS 包](ssis-quickstart-run-powershell.md)
    - [使用 C# 运行 SSIS 包](./ssis-quickstart-run-dotnet.md)