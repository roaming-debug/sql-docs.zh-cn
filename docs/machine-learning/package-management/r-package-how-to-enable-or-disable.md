---
title: 启用或禁用远程 R 包管理
description: 在 SQL Server 2016 R Services 或 SQL Server 机器学习服务上启用远程 R 包管理（数据库内）
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017||=sqlallproducts-allversions
ms.openlocfilehash: 1b3c857d5ba86fa1cb10097cf91ee5d591b67f94
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94869810"
---
# <a name="enable-or-disable-remote-package-management-for-sql-server"></a>启用或禁用 SQL Server 的远程包管理
[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

本文介绍如何从客户端工作站或其他 Machine Learning Server 启用 R 包的远程管理。 在 SQL Server 上启用包管理功能后，可以在客户端上使用 RevoScaleR 命令在 SQL Server 上安装包。

默认情况下，将禁用 SQL Server 的外部包管理功能。 若要启用此功能，必须运行一个单独的脚本，如下节所述。

## <a name="overview-of-process-and-tools"></a>过程和工具概述

若要在 SQL Server 上启用或禁用包管理，请使用命令行实用程序 RegisterRExt.exe  ，它包含在 RevoScaleR  包中。

[启用](#bkmk_enable)此功能的过程分为两个步骤，需要数据管理员：在 SQL Server 实例上启用包管理（每个 SQL Server 实例一次），然后在 SQL 数据库上启用包管理（每个 SQL Server 数据库一次）。

[禁用](#bkmk_disable)包管理功能也需要多个步骤：删除数据库级的包和权限（每个数据库一次），然后从服务器删除角色（每个实例一次）。

## <a name="enable-package-management"></a><a name="bkmk_enable"></a> 启用包管理

1. 在 SQL Server 上，打开提升的命令提示符，并导航到包含 RegisterRExt.exe 的文件夹。 默认位置为 `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExe.exe`。

2. 运行以下命令，为环境提供适当的参数：

    `RegisterRExt.exe /install pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    此命令在包管理所需的 SQL Server 计算机上创建实例级的对象。 它还会重新启动实例的启动板。

    如果未指定实例，将使用默认实例。 如果未指定用户，则使用当前安全上下文。 例如，以下命令使用打开命令提示符的用户的凭据，在默认实例上启用包管理：

    `REgisterRExt.exe /install pkgmgmt`

3. 若要为特定数据库添加包管理，从提升的命令提示符处运行以下命令：

    `RegisterRExt.exe /install pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`
   
    此命令将创建一些数据库项目，包括用于控制用户权限的以下数据库角色：`rpkgs-users`、`rpkgs-private` 和 `rpkgs-shared`。

    例如，以下命令对默认实例上的数据库启用包管理。 如果未指定用户，则使用当前安全上下文。

    `RegisterRExt.exe /install pkgmgmt /database:TestDB`

4. 对必须安装包的每个数据库重复此命令。

5. 若要验证是否已成功创建新角色，请在 SQL Server Management Studio 中，单击数据库，展开“安全性”  ，并展开“数据库角色”  。

    还可以在 sys.database_principals 上运行查询，如下所示：

    ```sql
    SELECT pr.principal_id, pr.name, pr.type_desc,   
        pr.authentication_type_desc, pe.state_desc,   
        pe.permission_name, s.name + '.' + o.name AS ObjectName  
    FROM sys.database_principals AS pr  
    JOIN sys.database_permissions AS pe  
        ON pe.grantee_principal_id = pr.principal_id  
    JOIN sys.objects AS o  
        ON pe.major_id = o.object_id  
    JOIN sys.schemas AS s  
        ON o.schema_id = s.schema_id;
    ```

启用此功能后，可以使用 RevoScaleR 函数从远程 R 客户端安装或卸载包。

## <a name="disable-package-management"></a><a name="bkmk_disable"></a> 禁用包管理

1. 在提升的命令提示符下，再次运行 RegisterRExt 实用程序，并禁用数据库级别的包管理：

    `RegisterRExt.exe /uninstall pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`

    此命令从指定数据库中删除与包管理相关的数据库对象。 它还将从 SQL Server 计算机的受保护文件系统位置中，删除安装的所有包。

2. 在使用包管理的每个数据库上重复此命令。

3.  （可选）使用前面的步骤清除所有数据库中的包之后，请在提升的命令提示符下运行以下命令：

    `RegisterRExt.exe /uninstall pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    此命令从实例中删除包管理功能。 可能需要再次手动重新启动启动板服务以查看更改。

## <a name="next-steps"></a>后续步骤

+ [使用 RevoScaleR 安装 R 包](install-r-packages-with-revoscaler.md)
+ [获取 R 包信息](r-package-information.md)
+ [使用 R 包的提示](tips-for-using-r-packages.md)
