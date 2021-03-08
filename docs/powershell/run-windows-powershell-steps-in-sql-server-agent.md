---
title: 在 SQL Server 代理中运行 Windows PowerShell 步骤
description: 了解如何在 SQL Server 代理作业中运行 Windows PowerShell 步骤。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.openlocfilehash: 8029d18dee3dd49342c3c029fc78992900394ed4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839395"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>在 SQL Server 代理中运行 Windows PowerShell 步骤

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

使用 SQL Server 代理以在计划时间运行 SQL Server PowerShell 脚本。

[!INCLUDE[sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

[!INCLUDE[sql-server-powershell-no-sqlps](../includes/sql-server-powershell-no-sqlps.md)]

## <a name="to-run-powershell-from-sql-server-agent"></a>从 SQL Server 代理运行 PowerShell

共有多种类型的 SQL Server 代理作业步骤。 每种类型都与用来实现特定环境（如复制代理或命令提示环境）的子系统关联。 可以对 Windows PowerShell 脚本进行编码，然后使用 SQL Server 代理将这些脚本包括在按计划时间运行或者为了响应 SQL Server 事件而运行的作业中。 可以使用命令提示作业步骤或 PowerShell 作业步骤运行 Windows PowerShell 脚本。

- 使用 PowerShell 作业步骤以让 SQL Server 代理子系统运行 sqlps 实用工具，该实用工具将启动 PowerShell 并导入 sqlps 模块。 如果正在运行 SQL Server 2019 或更高版本，建议在 SQL 代理作业步骤中使用 [SqlServer](sql-server-powershell.md#sql-server-agent) 模块。

- 使用命令提示作业步骤以便运行 PowerShell.exe，并且指定导入 **sqlps** 模块的脚本。

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> 关于内存占用的警告

通过 sqlps 模块运行 PowerShell 的每个 SQL Server 代理作业步骤都启动进程，大约占用 20 MB 内存。 同时运行大量的 Windows PowerShell 作业步骤会对性能产生负面影响。

## <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> 创建 PowerShell 作业步骤

### <a name="to-create-a-powershell-job-step"></a>创建 PowerShell 作业步骤

1. 展开“SQL Server 代理”，创建一个新作业或右键单击一个现有作业，再选择“属性”。 有关创建作业的详细信息，请参阅 [创建作业](../ssms/agent/create-jobs.md)。

2. 在“作业属性”对话框中，选择“步骤”页，再选择“新建”。

3. 在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。

4. 在“类型”列表中，选择“PowerShell”。

5. 在 **“运行身份”** 列表中，选择该作业将要使用的代理帐户和凭据。

6. 在 **“命令”** 框中，输入将为该作业步骤执行的 PowerShell 脚本语法。 或者，选择“打开”，选择包含脚本语法的文件。

7. 选择“高级”页设置以下作业步骤选项：当该作业步骤成功或失败时将执行的操作、SQL Server 代理应该尝试执行该作业步骤的次数以及重试的时间间隔。

## <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> 创建命令提示作业步骤

### <a name="to-create-a-cmdexec-job-step"></a>创建 CmdExec 作业步骤

1. 展开“SQL Server 代理”，创建一个新作业或右键单击一个现有作业，再选择“属性”。 有关创建作业的详细信息，请参阅 [创建作业](../ssms/agent/create-jobs.md)。

2. 在“作业属性”对话框中，选择“步骤”页，再选择“新建”。

3. 在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。

4. 在 **“类型”** 列表中，选择 **“操作系统(CmdExec)”** 。

5. 在 **“运行身份”** 列表中，选择具有作业将使用的凭据的代理帐户。 默认情况下，CmdExec 作业步骤在 SQL Server 代理服务帐户的上下文中运行。

6. 在 **“成功命令的进程退出代码”** 框中，输入一个介于 0 到 999999 之间的值。

7. 在 **“命令”** 框中，输入 powershell.exe 以及指定要运行的 PowerShell 脚本的参数。

8. 选择“高级”页设置以下作业步骤选项，例如：当该作业步骤成功或失败时将执行的操作、SQL Server 代理应该尝试执行该作业步骤的次数，以及 SQL Server 代理将作业步骤输出写入的文件。 只有 **sysadmin** 固定服务器角色的成员才可以将作业步骤输出写入到操作系统文件中。

## <a name="see-also"></a>另请参阅

- [SQL Server PowerShell](sql-server-powershell.md)
- [SQL Server 代理 和 PowerShell](sql-server-powershell.md#sql-server-agent)