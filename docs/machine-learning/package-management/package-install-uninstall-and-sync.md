---
title: 从文件系统同步 R 包
description: 将 SQL Server 上的 R 库更新为文件系统上安装的更新版本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017||=sqlallproducts-allversions
ms.openlocfilehash: 87ac1a75940de892e9796139a4ba0f08263da417
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870061"
---
# <a name="r-package-synchronization-for-sql-server"></a>SQL Server R 包同步
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

SQL Server 2017 附带的 RevoScaleR 版本包含在文件系统与使用包的实例和数据库之间同步 R 包集合的功能。

提供此功能是为了更轻松地备份与 SQL Server 数据库相关联的 R 包集合。 使用此功能，管理员不仅可以还原数据库，还可以还原在该数据库中工作的由数据科学家使用的任何 R 包。

本文介绍包同步功能，以及如何使用 [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) 函数执行以下任务：

+ 同步整个 SQL Server 数据库的包列表

+ 同步单个用户或一组用户使用的包

+ 如果用户移动到不同的 SQL Server，则可以对用户的工作数据库执行备份并将其还原到新服务器，并且用户的包将按照 R 要求安装到新服务器上的文件系统中。

例如，可以在以下场景中使用包同步：

+ DBA 已将 SQL Server 的实例还原到新计算机，要求用户从其 R 客户端进行连接，并运行 `rxSyncPackages` 来刷新和还原包。

+ 你认为文件系统上的 R 包已损坏，因此在 SQL Server 上运行 `rxSyncPackages`。

## <a name="requirements"></a>要求

使用包同步之前，必须具有适当版本的 Microsoft R 或 Machine Learning Server。 此功能在 Microsoft R 版本 9.1.0 或更高版本中提供。 

还必须在服务器上启用[包管理功能](r-package-how-to-enable-or-disable.md)。

### <a name="determine-whether-your-server-supports-package-management"></a>确定服务器是否支持包管理

SQL Server 2017 CTP 2 或更高版本中提供此功能。

可以通过将 SQL Server 2016 实例升级为使用最新版本的 Microsoft R，来将此功能添加到该实例。有关详细信息，请参阅[使用 SqlBindR.exe 升级 SQL Server R Services](../install/upgrade-r-and-python.md)。

### <a name="enable-the-package-management-feature"></a>启用包管理功能

若要使用包同步，需要在 SQL Server 实例和单个数据库上启用新的包管理功能。 有关详细信息，请参阅[启用或禁用 SQL Server 包管理](r-package-how-to-enable-or-disable.md)。

1. 服务器管理员启用 SQL Server 实例的功能。
2. 对于每个数据库，管理员会授予各个用户使用数据库角色安装或共享 R 包的能力。

完成此操作后，可以使用 RevoScaleR 函数（如 [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages)）将包安装到数据库中。  有关用户和可使用的包的信息存储在 SQL Server 实例中。 

每当使用包管理功能添加新包时，SQL Server 和文件系统中的记录都将更新。 此信息可用于还原整个数据库的包信息。

### <a name="permissions"></a>权限

+ 执行包同步功能的人员必须是包含包的 SQL Server 实例和数据库上的安全主体。

+ 函数的调用方必须是下列包管理角色之一的成员：rpkgs-shared  或 rpkgs-private  。

+ 若要同步标记为“共享”  的包，运行函数的人员必须具有 rpkgs-shared-shared  角色的成员身份，并且要移动的包必须已安装到共享范围库。

+ 若要同步标记为“私有”  的包，包的所有者或管理员必须运行该函数，并且包必须是私有的。

+ 若要代表其他用户同步包，所有者必须是 db_owner  数据库角色的成员。

## <a name="how-package-synchronization-works"></a>包同步的工作原理

若要使用包同步，请调用 [rxSyncPackages](/r-server/r-reference/revoscaler/rxsyncpackages)，它是 [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) 中的新函数。 

对于 `rxSyncPackages` 的每个调用，必须指定一个 SQL Server 实例和数据库。 然后，列出要同步的包或指定包范围。

1. 使用 `RxInSqlServer` 函数创建 SQL Server 计算上下文。 如果未指定计算上下文，则使用当前的计算上下文。

2. 在指定的计算上下文中为实例提供数据库名称。 根据数据库来同步包。

3. 使用范围参数指定要同步的包。

    如果使用“私有”  范围，则只同步指定所有者所拥有的包。 如果指定“共享”  范围，将同步数据库中的所有非私有包。 
    
    如果在未指定“私有”  或“共享”  范围的情况下运行函数，将同步所有包。

4. 如果此命令成功，则会将文件系统中的现有包添加到数据库，并具有指定的范围和所有者。

    如果文件系统损坏，则会根据数据库中保留的列表还原包。

    如果包管理功能在目标数据库上不可用，则会引发错误：“未在 SQL Server 上启用包管理功能或版本过旧”

### <a name="example-1-synchronize-all-package-by-database"></a>示例 1。 根据数据库同步所有包

此示例从本地文件系统获取任何新包，并将包安装到数据库 [TestDB] 中。 由于没有特定所有者，因此该列表包含所有为私有和共享范围安装的包。

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>示例 2。 根据范围限制同步包

下面的示例只同步指定范围内的包。

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>示例 3。 根据所有者限制同步包

下面的示例演示如何仅同步为特定用户安装的包。 在此示例中，用户由 SQL 登录名 user1  标识。

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>相关资源

[SQL Server 的 R 包管理](install-additional-r-packages-on-sql-server.md)