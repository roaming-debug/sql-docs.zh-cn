---
title: 使用 RevoScaleR 安装 R 包
description: 了解如何使用 RevoScaleR 函数在附带机器学习服务或 R Services 的 SQL Server 上安装 R 包。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 28dbaf6f3496004ba90731d6bf5eaef693036e19
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471048"
---
# <a name="use-revoscaler-to-install-r-packages"></a>使用 RevoScaleR 安装 R 包

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

本文介绍如何使用 [RevoScaleR](../r/ref-r-revoscaler.md)（9.0.1 版及更高版本）函数在附带机器学习服务或 R Services 的 SQL Server 上安装 R 包。 远程非管理员可以使用 RevoScaleR 函数在 SQL Server 上安装包，而无需直接访问服务器。

::: moniker range="=sql-server-2016"
> [!NOTE]
> SQL Server R Services 客户必须执行[组件升级](../install/upgrade-r-and-python.md)才能获取 RevoScaleR 包管理函数。 有关如何检索包版本和内容的说明，请参阅[获取 R 包信息](../package-management/r-package-information.md)。
::: moniker-end

## <a name="revoscaler-functions-for-package-management"></a>用于包管理的 RevoScaleR 函数

下表描述了用于 R 包安装和管理的函数。

| 函数 | 说明 |
|----------|-------------|
| [rxSqlLibPaths](/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) | 确定远程 SQL Server 上实例库的路径。 |
| [rxFindPackage](/machine-learning-server/r-reference/revoscaler/rxfindpackage) | 获取远程 SQL Server 上的一个或多个包的路径。 |
| [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) | 从远程 R 客户端调用此函数，从指定存储库或通过读取本地保存的压缩包，将包安装到 SQL Server 计算上下文。 此函数检查依赖项并确保任何相关包可安装到 SQL Server，就像本地计算上下文中的 R 包安装一样。 若要使用此选项，必须在服务器和数据库上启用包管理。 客户端和服务器环境必须具有相同版本的 RevoScaleR。 |
| [rxInstalledPackages](/machine-learning-server/r-reference/revoscaler/rxinstalledpackages) | 在指定的计算上下文中获取安装的包的列表。 |
| [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) | 对于指定的计算上下文，在文件系统和数据库之间复制有关包库的信息。 |
| [rxRemovePackages](/machine-learning-server/r-reference/revoscaler/rxremovepackages) | 删除指定计算上下文中的包。 它还会计算依赖项，并确保删除 SQL Server 上其他包不再使用的包以释放资源。 |

## <a name="prerequisites"></a>必备条件

+ 在 SQL Server 上启用远程管理。 有关详细信息，请参阅[在 SQL Server 上启用远程 R 包管理](r-package-how-to-enable-or-disable.md)。

+ 客户端和服务器环境中的 RevoScaleR 版本相同。 有关详细信息，请参阅[获取 R 包信息](../package-management/r-package-information.md)。

+ 你有权连接到服务器和数据库，以及运行 R 命令。 你必须是数据库角色的成员，该角色允许你在指定的实例和数据库上安装包。

  + 共享范围  中的包可以由属于指定数据库中 `rpkgs-shared` 角色的用户进行安装。 此角色中的所有用户都可以卸载共享包。

  + 专用范围  中的包可以由属于数据库中 `rpkgs-private` 角色的任何用户进行安装。 但是，用户只能查看并卸载自己的包。

  + 数据库所有者可以使用共享包或专用包。

## <a name="client-connections"></a>客户端连接

在同一网络上，客户端工作站可以是 [Microsoft R Client](/machine-learning-server/r-client/install-on-windows) 或 [Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install)（数据科学家经常使用免费开发人员版）。

从远程 R 客户端调用包管理函数时，必须先使用 [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) 函数创建计算上下文对象。 此后，针对你使用的每个包管理函数，将计算上下文作为参数传递。

用户标识通常在设置计算上下文时指定。 如果在创建计算上下文时未指定用户名和密码，则将使用运行 R 代码的用户标识。

1. 在 R 命令行中，定义实例和数据库的连接字符串。
2. 利用连接字符串，使用 [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) 构造函数来定义 SQL Server 计算上下文。

    ```R
    sqlcc <- RxInSqlServer(connectionString = myConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```
3. 创建要安装的包列表，并将列表保存在字符串变量中。

    ```R
    packageList <- c("e1071", "mice")
    ```

4. 调用 [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages)，并传递计算上下文和包含包名称的字符串变量。

    ```R
    rxInstallPackages(pkgs = packageList, verbose = TRUE, computeContext = sqlcc)
    ```

    如果需要相关的包，也会安装它们，前提是客户端上有 Internet 连接。
    
    使用建立连接的用户凭据，在该用户的默认范围安装包。

## <a name="call-package-management-functions-in-stored-procedures"></a>在存储过程中调用包管理函数

可以在 `sp_execute_external_script` 中运行包管理函数。 执行此操作时，将使用存储过程调用方的安全上下文执行该函数。

## <a name="examples"></a>示例

本节将提供一些示例，演示在以计算上下文形式连接到 SQL Server 实例或数据库时，如何从远程客户端使用这些函数。

对于所有示例，必须提供连接字符串或需要连接字符串的计算上下文。 此示例提供了一种为 SQL Server 创建计算上下文的方法：

```R
instance_name <- "computer-name/instance-name";
database_name <- "TestDB";
sqlWait= TRUE;
sqlConsoleOutput <- TRUE;
connString <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
sqlcc <- RxInSqlServer(connectionString = connString, wait = sqlWait, consoleOutput = sqlConsoleOutput, numTasks = 4);
```

根据服务器所在的位置和安全模型，你可能需要在连接字符串中提供域和子网规范，或使用 SQL 登录名。 例如：

```R
connStr <- "Driver=SQL Server;Server=myserver.financeweb.contoso.com;Database=Finance;Uid=RUser1;Pwd=RUserPassword"
```

### <a name="get-package-path-on-a-remote-sql-server-compute-context"></a>获取远程 SQL Server 计算上下文中的包路径

此示例获取计算上下文  **上，RevoScaleR**`sqlcc` 包的路径。

```R
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlcc)
print(sqlPackagePaths)
```

**结果**

"C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library/RevoScaleR"

> [!TIP]
> 如果已启用了查看 SQL 控制台输出的选项，则可以从 `print` 语句前面的函数获取状态消息。 测试完代码后，在计算上下文构造函数中将 `consoleOutput` 设置为 FALSE 以消除消息。

### <a name="get-locations-for-multiple-packages"></a>获取多个包的位置

以下示例获取计算上下文  **上，RevoScaleR** **和 lattice**`sqlcc` 包的路径。 获取有关多个包的信息时，传递包含包名称的字符串向量。

```R
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"), computeContext = sqlcc)
print(packagePaths)
```

### <a name="get-package-versions-on-a-remote-compute-context"></a>获取远程计算上下文中的包版本

从 R 控制台运行此命令来获取在计算上下文 sqlServer  中安装的包的生成号和版本号。

```R
sqlPackages <- rxInstalledPackages(fields = c("Package", "Version", "Built"), computeContext = sqlServer)
```

### <a name="install-a-package-on-sql-server"></a>在 SQL Server 上安装包

此示例将 forecast  包及其依赖项安装到计算上下文。

  ```R
  pkgs <- c("forecast")
  rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="remove-a-package-from-sql-server"></a>从 SQL Server 删除包

此示例将 forecast  包及其依赖项从计算上下文删除。

  ```R
  pkgs <- c("forecast")
  rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="synchronize-packages-between-database-and-file-system"></a>在数据库和文件系统之间同步包

下面的示例将检查数据库 TestDB  ，并确定是否在文件系统中安装了所有包。 如果缺少某些包，则它们将安装在文件系统中。

```R
# Instantiate the compute context
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

# Synchronize the packages in the file system for all scopes and users
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

包同步基于每个数据库和每个用户运作。 有关详细信息，请参阅 [SQL Server 的 R 包同步](package-install-uninstall-and-sync.md)。

### <a name="use-a-stored-procedure-to-list-packages-in-sql-server"></a>使用存储过程列出 SQL Server 中的包

从 Management Studio 或另一个支持 T-SQL 的工具运行此命令，以使用存储过程中的 `rxInstalledPackages` 获取当前实例中已安装包的列表。

```sql
EXEC sp_execute_external_script 
  @language=N'R', 
  @script=N'
    myPackages <- rxInstalledPackages();
    OutputDataSet <- as.data.frame(myPackages);
    '
```

`rxSqlLibPaths` 函数可用于确定 SQL Server 机器学习服务使用的活动库。 此脚本只能返回当前服务器的库路径。 

```sql
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
```

## <a name="see-also"></a>另请参阅

+ [启用远程 R 包管理](r-package-how-to-enable-or-disable.md)
+ [同步 R 包](package-install-uninstall-and-sync.md)
+ [使用 R 包的提示](tips-for-using-r-packages.md)
+ [获取 R 包信息](../package-management/r-package-information.md)