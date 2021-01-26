---
title: 更改服务器身份验证模式
description: 了解如何在 SQL Server 中更改服务器身份验证模式。 可使用 SQL Server Management Studio 或 Transact-SQL 执行此任务。
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- authentication [SQL Server], changing modes
- server authentication mode [SQL Server]
- modifying server authentication mode
ms.assetid: 79babcf8-19fd-4495-b8eb-453dc575cac0
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 02/18/2020
ms.openlocfilehash: a4cdc32bd0514be768ac8ed1723fc88840a7e930
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783404"
---
# <a name="change-server-authentication-mode"></a>更改服务器身份验证模式

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中更改服务器身份验证模式。 安装过程中， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 设置为 **“Windows 身份验证模式”** 或 **“SQL Server 和 Windows 身份验证模式”** 。 安装完成后，您可以随时更改身份验证模式。

如果在安装过程中选择了“Windows 身份验证模式”，则 sa 登录名将被禁用，安装程序会分配一个密码。 如果稍后将身份验证模式更改为“SQL Server 和 Windows 身份验证模式”，则 sa 登录名仍处于禁用状态。 若要使用 sa 登录名，请使用 ALTER LOGIN 语句启用 sa 登录名并分配一个新密码。 sa 登录名只能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接到服务器。

## <a name="before-you-begin"></a>开始之前

sa 帐户是一个广为人知的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户，并且经常成为恶意用户的攻击目标。 除非您的应用程序需要使用 sa 帐户，否则请不要启用它。 因此，为 sa 登录名使用强密码非常重要。

## <a name="change-authentication-mode-with-ssms"></a>使用 SSMS 更改身份验证模式

1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中，右键单击服务器，再单击“属性”。

2. 在 **“安全性”** 页上的 **“服务器身份验证”** 下，选择新的服务器身份验证模式，再单击 **“确定”** 。

3. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对话框中，单击 **“确定”** 以确认需要重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。

4. 在“对象资源管理器”中，右键单击服务器，并单击“重新启动”。 如果运行有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，则也必须重新启动该代理。

## <a name="enable-sa-login"></a>启用 sa 登录

可使用 SSMS 或 T-SQL 启用 sa 登录。

### <a name="use-ssms"></a>使用 SSMS

1. 在对象资源管理器中，依次展开“安全性”、“登录名”，右键单击“sa”，再单击“属性”。

2. 在“常规”页上，你可能需要为 sa 登录名创建密码并确认该密码 。

3. 在 **“状态”** 页上的 **“登录”** 部分，单击 **“启用”** ，然后单击 **“确定”** 。

### <a name="using-transact-sql"></a>“使用 Transact-SQL”

下面的示例启用 sa 登录名并设置一个新密码。 在运行之前将 `<enterStrongPasswordHere>` 替换为强密码。

```sql  
ALTER LOGIN sa ENABLE ;  
GO  
ALTER LOGIN sa WITH PASSWORD = '<enterStrongPasswordHere>' ;  
GO  
```

## <a name="change-authentication-mode-t-sql"></a>更改身份验证模式 (T-SQL)

以下示例将服务器身份验证从混合模式 (Windows + SQL) 更改为仅 Windows。

> [!CAUTION]
> 下面的示例使用扩展存储过程来修改服务器注册表。 如果没有正确修改注册表，可能会出现严重问题。 这些问题可能需要你重新安装操作系统。 Microsoft 无法保证可以解决这些问题。 修改注册表的风险自负。

```sql
USE [master]
GO
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', 
     N'Software\Microsoft\MSSQLServer\MSSQLServer',
     N'LoginMode', REG_DWORD, 1
GO
```

> [!Note]
> 更改身份验证模式所需的权限为 [sysadmin](../../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles) 或 [Control Server](../../relational-databases/security/permissions-database-engine.md)

## <a name="see-also"></a>另请参阅

- [强密码](../../relational-databases/security/strong-passwords.md)
- [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [在系统管理员被锁定时连接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)
