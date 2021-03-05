---
title: 常见 R 脚本错误
description: 本文介绍了在 SQL Server 中运行 R 代码时可能会遇到的几个常见脚本错误。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/31/2018
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0f9f086ec09830e65f85ca2df5232e8b401cb9eb
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186409"
---
# <a name="common-r-scripting-errors-in-sql-server"></a>SQL Server 中的常见 R 脚本错误
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍了在 SQL Server 中运行 R 代码时的几个常见脚本错误。 此列表并不全面。 存在许多包，同一包的不同版本可能会有不同的错误。

## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>T-SQL 或存储过程中的有效脚本失败

在将 R 代码包装到存储过程中之前，最好在外部 IDE 或 RTerm 或 RGui 等 R 工具中运行 R 代码。 通过使用这些方法，你可以使用 R 返回的详细错误消息来测试和调试代码。

但是，有时在外部 IDE 或实用程序中运行正常的代码可能无法在存储过程或 SQL Server 计算上下文中运行。 如果发生这种情况，需要先排查各种问题，然后才能推断包在 SQL Server 中不运行。

1. 检查启动板是否正在运行。

2. 检查消息，查看输入数据或输出数据是否包含具有不兼容或不支持的数据类型的列。 例如，对 SQL 数据库的查询通常返回 GUID 或 RowGUID，这两个都是不受支持的。 有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

3. 检查单个 R 函数的帮助页，确定是否支持 SQL Server 计算上下文的所有参数。 有关 ScaleR 帮助，请使用内联的 R 帮助命令，或参阅[包参考](/r-server/r-reference/revoscaler/revoscaler)。

如果 R 运行时正常运行，但脚本返回错误，建议尝试在专用 R 开发环境（如针对 Visual Studio 的 R 工具）中调试脚本。

此外，还建议查看并稍微重写脚本，以更正在 R 和数据库引擎之间移动数据时可能出现的数据类型问题。 有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

另外，还可以使用 sqlrutils 包将 R 脚本打包成一种更易于作为存储过程使用的格式。 有关详细信息，请参阅：
* [sqlrutils 包](../r/ref-r-sqlrutils.md)
* [使用 sqlrutils 创建存储过程](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)

## <a name="script-returns-inconsistent-results"></a>脚本返回不一致的结果

R 脚本可以在 SQL Server 上下文中返回不同的值，原因如下：

- 在 SQL Server 和 R 之间传递数据时，某些数据类型会自动执行隐式类型转换。有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

- 确定位数是否为因素。 例如，对于 32 位和 64 位浮点库，数学运算的结果通常会有所不同。

- 确定是否在任何操作中生成了 NAN。 这可能会使结果无效。

- 当取一个接近零的数的倒数时，则可能会使微小的差异放大。

- 累积的舍入错误可能导致值小于零而不是零。

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>通过 ODBC 远程执行隐式身份验证

如果使用 RevoScaleR 函数连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机，以运行 R 命令，则在使用将数据写入到服务器的 ODBC 调用时，可能会遇到错误。 仅当你使用 Windows 身份验证时，才会发生此错误。

原因是为 R 服务创建的辅助角色帐户没有连接到服务器的权限。 因此，无法代表你执行 ODBC 调用。 使用 SQL 登录不会出现此问题，因为使用 SQL 登录时，凭据会从 R 客户端显式传递到 SQL Server 实例，然后传递到 ODBC。 但是，使用 SQL 登录也不如使用 Windows 身份验证安全。

若要使 Windows 凭据能够安全地从远程启动的脚本进行传递，SQL Server 必须模拟你的凭据。 此过程称为“隐式身份验证”。 要执行此操作，在 SQL Server 计算机上运行 R 或 Python 脚本的辅助角色帐户必须具有正确的权限。

1. 在运行 R 代码的实例上以管理员身份打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。

2. 运行以下脚本。 如果更改了默认值、计算机和实例名称，请务必编辑用户组名称。

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>在 SQL 计算上下文中运行 R 时避免清除工作区

尽管在 R 控制台中工作时经常清除工作区，但在 SQL 计算上下文中，却可能因此造成意外后果。

`revoScriptConnection` 是 R 工作区中的对象，它包含有关从 SQL Server 调用的 R 会话的信息。 但是，如果 R 代码包含清除工作区的命令（例如 `rm(list=ls())`），则将同时清除有关会话以及 R 工作区中其他对象的所有信息。

解决方法之一是在 SQL Server 中运行 R 时，避免随意清除变量和其他对象。 可以使用 remove 函数删除特定变量：

```R
remove('name1', 'name2', ...)
```

若要删除多个变量，建议将临时变量的名称保存到列表中，然后对列表执行定期的垃圾回收。



## <a name="next-steps"></a>后续步骤

[机器学习服务疑难解答和已知问题](machine-learning-troubleshooting-overview.md)

[收集数据进行机器学习故障排除](data-collection-ml-troubleshooting-process.md)

[安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)

[数据库引擎连接疑难解答](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)