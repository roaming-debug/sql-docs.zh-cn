---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 8d996c6e48f90cf54b962481cc1e7cd1ed7b088e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596319"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|15401|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|SEC_INVALIDLOGINORGROUP|
|消息正文|找不到 Windows NT 用户或组 '%s'。 请再次检查该名称。|
||

## <a name="explanation"></a>说明

如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法基于 Windows 主体（例如域用户或 Windows 域组）创建登录名，则会发生此错误。 将向用户报告如下错误消息

> 错误 15401：找不到 Windows NT 用户或组 '%s'。 请再次检查该名称。

## <a name="cause"></a>原因

该错误的起因可以是下列原因之一：

- Active Directory 中不存在该登录名。
- 域控制器不可用。
- 添加本地帐户时，你没有使用 BUILTIN 作为域名。
- 名称解析问题。

## <a name="user-action"></a>用户操作

请查看以下部分，了解针对上述每个不同原因可以采取的措施。

### <a name="verify-the-login-you-are-trying-to-add"></a>验证你尝试添加的登录名

1. 验证 Windows 登录名是否仍存在于域中。 由于特定原因，网络管理员可能已删除了 Windows 登录名，因此你可能无法向该登录名授予对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问权限。
1. 验证域和登录名的拼写是否正确，以及是否使用以下格式：`Domain\User`
1. 如果登录名存在并且正确，但仍然收到错误，请继续阅读本文中的以下部分。

### <a name="verify-if-the-domain-controller-is-available"></a>验证域控制器是否可用

如果登录名所在域的域控制器（相同或不同域）由于某种原因而不可用，则可能会收到错误 15401。

如果登录名与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在不同域中，则验证域之间是否存在正确的信任。

使用运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上的 ping 命令验证登录名的域控制器是否可访问。 检查域控制器的 IP 地址和名称。

### <a name="verify-the-domain-name-for-local-accounts"></a>验证本地帐户的域名

本地（非域）帐户需要特殊处理。 如果尝试从运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地计算机添加本地帐户，请确保使用 BUILTIN 作为域名。

### <a name="check-for-name-resolution-issues"></a>检查名称解析问题

如果在解析添加登录名或组所涉及的计算机名称时遇到问题，则可能会收到错误 15401。

验证是否正确配置了名称解析机制（例如，WINS、DNS、HOSTS 或 LMHOSTS）。

## <a name="see-also"></a>另请参阅

- [测试本地计算机与其域之间的通道](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](../system-stored-procedures/sp-change-users-login-transact-sql.md)