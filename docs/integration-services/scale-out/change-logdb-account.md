---
title: 更改 SSIS Scale Out 日志记录的帐户 | Microsoft Docs
description: 了解如何更改用于 SQL Server Integration Services (SSIS) Scale Out 日志记录的帐户。
ms.custom: performance
ms.date: 06/29/2020
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: b36b01a416a5468def2baa66cd4395b0944a49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354901"
---
# <a name="change-the-account-for-scale-out-logging"></a>更改 Scale Out 日志记录的帐户

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


在 Scale Out 中运行 SSIS 包时，会使用自动创建的名为 ##MS_SSISLogDBWorkerAgentLogin## 的用户帐户将事件消息记录到 SSISDB 数据库中  。 此用户使用 SQL Server 身份验证登录。

要更改用于 Scale Out 日志记录的帐户，请执行以下操作：

> [!NOTE]
> 如果使用 Windows 用户帐户进行日志记录，请使用与运行 Scale Out Worker 服务的帐户相同的帐户。 否则，SQL Server 登录将失败。

## <a name="1-create-a-user-for-ssisdb"></a>1.创建 SSISDB 用户
有关如何创建数据库用户的说明，请参阅[创建数据库用户](../../relational-databases/security/authentication-access/create-a-database-user.md)。

## <a name="2-add-the-user-to-the-database-role-ssis_cluster_worker"></a>2.向数据库角色 ssis_cluster_worker 添加用户

有关如何加入数据库角色的说明，请参阅[加入角色](../../relational-databases/security/authentication-access/join-a-role.md)。

## <a name="3-update-the-logging-information-in-ssisdb"></a>3.在 SSISDB 中更新日志记录信息
使用 SQL Server 名称和连接字符串作为参数调用存储过程 `[catalog].[update_logdb_info]`，如下例所示：

```sql
SET @serverName = CONVERT(sysname, SERVERPROPERTY('servername'))
SET @connectionString = 'Data Source=' + @serverName + ';Initial Catalog=SSISDB;Integrated Security=SSPI;'
EXEC [internal].[update_logdb_info] @serverName, @connectionString
GO
```

## <a name="4-restart-the-scale-out-worker-service"></a>4.重启 Scale Out Worker 服务
重启 Scale Out Worker 服务以使更改生效。

## <a name="next-steps"></a>后续步骤
-   [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md)
