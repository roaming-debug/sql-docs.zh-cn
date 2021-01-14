---
title: SQL Server PowerShell
description: 了解两个 SQL Server PowerShell 模块（SqlServer 和 SQLPS），其中包括 PowerShell 提供程序和 cmdlet。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 06/11/2020
ms.openlocfilehash: fad1ac64dcc1024d5dad091670300b8d39fa0632
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092679"
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

**[安装 SQL Server PowerShell](download-sql-server-ps-module.md)**

SQL Server PowerShell 模块有两种；[SqlServer](/powershell/module/sqlserver) 和 [SQLPS](/powershell/module/sqlps) 。

SqlServer 模块是当前要使用的 PowerShell 模块。

虽然 SQL Server 安装附带了 SQLPS 模块（用于实现后向兼容性），但该模块不再更新。

SqlServer 模块不仅包含 SQLPS 更新版本的 cmdlet，还包含新的 cmdlet 以支持最新的 SQL 功能 。

虽然 [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 随附了以前版本的 SqlServer 模块，但仅限 16.x 版本的 SSMS。

要在 SSMS 17.0 和更高版本中使用 PowerShell，则必须从 [PowerShell 库](https://www.powershellgallery.com/packages/SqlServer)安装 SqlServer 模块。

你也可以将 [PowerShell 和 Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md) 结合使用。

**为什么模块从 SQLPS 更改为 SqlServer？**

要发送 SQL PowerShell 更新，必须更改 SQL PowerShell 模块的标识和名为 SQLPS.exe 的包装器。 由于此更改，现存在两种 SQL PowerShell 模块：SqlServer 模块和 SQLPS 模块 。  

**若导入 SQLPS 模块，请更新 PowerShell 脚本。**

如果具有任何运行 `Import-Module -Name SQLPS` 的 PowerShell 脚本，并希望利用新的提供程序功能和新的 cmdlet，则必须将它们更改为 `Import-Module -Name SqlServer`。 新模块会安装到 `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer` 文件夹。 因此，不需要更新 $env:PSModulePath 变量。 如果脚本使用名为 SqlServer 的第三方或社区版本模块，请使用 Prefix 参数以避免名称冲突。

建议使用 Import-Module SQLServer 来启动脚本，以避免在同一台计算机上安装 SQLPS 模块的情况下出现并行问题。

本部分适用于从 PowerShell 而不是 SQL 代理执行的脚本。 可通过使用 [#NOSQLPS](#sql-server-agent) 将新模块与 SQL 代理作业步骤一起使用。

## <a name="sql-server-powershell-components"></a>SQL Server PowerShell 组件

SqlServer 模块随附：

- [PowerShell 提供程序](/powershell/module/microsoft.powershell.core/about/about_providers)允许使用类似于文件系统路径的简单导航机制。 您可以生成类似于文件系统路径的路径，在该路径中，驱动器与 SQL Server 管理对象模型关联，节点基于对象模型类。 然后，你可以使用熟悉的命令（如 **cd** 和 **dir** ），按照在命令提示符窗口中导航文件夹的类似方式导航路径。 可以使用其他命令（如 **ren** 或 **del**）对路径中的节点执行操作。

- 一组 cmdlet，它支持运行包含 Transact-SQL 或 XQuery 语句的 sqlcmd 脚本等操作。  

- AS 提供程序和 cmdlet，它们之前是单独安装的。

## <a name="sql-server-versions"></a>SQL Server 版本

SQL PowerShell cmdlet 可用于管理 Azure SQL 数据库、Azure Synapse Analytics 和所有[支持的 SQL Server 产品](https://support.microsoft.com/lifecycle/search/1044)的实例。

## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>包含 PowerShell 路径中不支持的字符的 SQL Server 标识符

**Encode-Sqlname** 和 **Decode-Sqlname** cmdlet 帮助你指定包含 PowerShell 路径中不支持的字符的 SQL Server 标识符。 有关详细信息，请参阅 [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md)。

使用 Convert-UrnToPath cmdlet 将数据库引擎对象的统一资源名称转换为 SQL Server PowerShell 提供程序的路径。 有关详细信息，请参阅 [Convert URNs to SQL Server Provider Paths](/powershell/module/sqlserver/Convert-UrnToPath)。
  
## <a name="query-expressions-and-unique-resource-names"></a>查询表达式和唯一资源名称  

查询表达式是使用与 XPath 类似的语法指定一组条件的字符串，用于枚举对象模型层次结构中的一个或多个对象。 唯一资源名称 (URN) 是一种特定类型的查询表达式字符串，用于唯一标识单个对象。 有关详细信息，请参阅 [Query Expressions and Uniform Resource Names](query-expressions-and-uniform-resource-names.md)。

## <a name="sql-server-agent"></a>SQL Server 代理

SQL Server 代理使用的模块不做任何更改。 因此，具有 PowerShell 类型作业步骤的 SQLServer 代理作业使用 SQLPS 模块。 有关详细信息，请参阅[如何使用 SQL Server 代理运行 PowerShell](run-windows-powershell-steps-in-sql-server-agent.md)。 但是，从 SQL Server 2019 开始，可以禁用 SQLPS。 为此，可以在 PowerShell 类型的作业步骤的第一行添加 `#NOSQLPS`，这将阻止 SQL 代理自动加载 SQLPS 模块。 执行此操作时，SQL 代理作业将运行安装在计算机上的 PowerShell 版本，然后你可以使用自己喜欢的任何其他 PowerShell 模块。

如果要在 SQL 代理作业步骤中使用 SqlServer 模块，可以将此代码放在脚本的前两行。

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```

## <a name="cmdlet-reference"></a>Cmdlet 参考

- [SqlServer cmdlet](/powershell/module/sqlserver)
- [SQLPS cmdlet](/powershell/module/sqlps)

## <a name="next-steps"></a>后续步骤

- [下载 SQL Server PowerShell 模块](download-sql-server-ps-module.md)
- [SQL Server PowerShell cmdlet](/powershell/module/sqlserver)
- [将 PowerShell 和 Azure Data Studio 结合使用](../azure-data-studio/extensions/powershell-extension.md)