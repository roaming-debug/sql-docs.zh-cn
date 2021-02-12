---
title: 在 Active Directory 模式下管理大数据群集
description: 管理对大数据群集的访问
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c40a136f748126bd7baa6892d5cdb5317363ad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046327"
---
# <a name="manage-big-data-cluster-access-in-active-directory-mode"></a>在 Active Directory 模式下管理大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍除了在部署期间通过 clusterUsers 配置设置提供的 Active Directory 组之外，如何添加包含 bdcUser 角色的新 Active Directory 组。

>[!IMPORTANT]
>请不要使用此过程添加包含 bdcAdmin 角色的新 Active Directory 组。 HDFS 和 Spark 等 Hadoop 组件仅允许一个 Active Directory 组作为超级用户组，超级用户组等效于 BDC 中的 bdcAdmin 角色。 你必须在部署过程中将其他用户和组添加到已命名的组，部署后才能将大数据群集的 bdcAdmin 权限授予其他 Active Directory 组。 可以按照同一个过程来更新具有 bdcUsers 角色的组成员身份。

## <a name="two-overarching-roles-in-the-big-data-cluster"></a>大数据群集中的两个主要角色

可以在部署配置文件的“安全”部分中提供 Active Directory 组，这是大数据群集中授权的两个主要角色的一部分：

* `clusterAdmins`：此参数采用一个 Active Directory 组。 此组的成员具有 bdcAdmin 角色，这表示这些成员将获取整个群集的管理员权限。 它们具有 SQL Server 中的 sysadmin 权限、Hadoop 分布式文件系统 (HDFS) 和 Spark 中的超级用户权限，以及控制器中的管理员权限。

* `clusterUsers`：这些 Active Directory 组映射到 BDC 的 bdcUsers 角色。 这些用户是在群集中不具有管理员权限的普通用户。 它们有权登录到 SQL Server 主实例，但默认情况下，它们无权访问对象或数据。 这些用户是 HDFS 和 Spark 的普通用户，他们没有超级用户权限。 连接到控制器终结点时，这些用户只能查询终结点（使用 azdata bdc 终结点列表）。

若要在不改变 Active Directory 内的组成员身份的情况下向其他 Active Directory 组授予 bdcUser 权限，请完成后续部分中的过程。

## <a name="grant-bdcuser-permissions-to-additional-active-directory-groups"></a>向其他 Active Directory 组授予 bdcUser 权限

### <a name="create-a-login-for-the-active-directory-user-or-group-in-the-sql-server-master-instance"></a>为 SQL Server 主实例中的 Active Directory 用户或组创建登录名

1. 使用你最喜欢的 SQL 客户端连接到主 SQL 终结点。 使用任何管理员登录名（例如，在部署期间提供的 `AZDATA_USERNAME`）。 或者，它可以是属于安全配置中作为 `clusterAdmins` 提供的 Active Directory 组的任何 Active Directory 帐户。

1. 若要为 Active Directory 用户或组创建登录名，请运行以下 TSQL 命令：

   ```sql
   CREATE LOGIN [<domain>\<principal>] FROM WINDOWS;
   ```

   授予所需的 SQL Server 实例权限：

   ```sql
   ALTER SERVER ROLE <server role> ADD MEMBER [<domain>\<principal>];
   GO
   ```

有关服务器角色的完整列表，请参阅[此处](../relational-databases/security/authentication-access/server-level-roles.md)相应的 SQL Server 安全主题。

### <a name="add-the-active-directory-user-or-group-to-the-roles-table-in-the-controller-database"></a>将 Active Directory 用户或组添加到控制器数据库的角色表中

1. 通过运行以下命令获取控制器 SQL Server 凭据：

   a. 以 Kubernetes 管理员身份运行以下命令：

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. 对密码执行 Base64 解码：

   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. 在单独的命令窗口中，公开控制器数据库服务器端口：

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```

1. 使用前面的连接在“roles”表和“active_directory_principals”表中插入新行。 以大写字母形式键入 REALM 值。

   ```sql
   USE controller;
   GO

   INSERT INTO [controller].[auth].[roles] VALUES (N'<user or group name>@<REALM>', 'bdcUser')
   GO

   INSERT INTO [controller].[auth].[active_directory_principals] VALUES (N'<user or group name>@<REALM>', N'<SID>')
   GO
   ```

   若要查找要添加的用户或组的 SID，可以使用 [Get-ADUser](/powershell/module/addsadministration/get-aduser/) 或 [Get-ADGroup](/powershell/module/addsadministration/get-adgroup/) PowerShell 命令。

2. 登录到控制器终结点或对 SQL Server 主实例进行身份验证，验证添加的组成员是否具有所需的 bdcUser 权限。 例如：

   ```bash
   azdata login
   azdata bdc endpoints list
   ```

## <a name="next-steps"></a>后续步骤

- [SQL Server 2019 大数据群集的安全性概念](concept-security.md)
