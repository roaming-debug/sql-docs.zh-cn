---
title: Linux 上的 SQL Server 的 Active Directory 身份验证
titleSuffix: SQL Server
description: 本文概述了 Linux 上的 SQL Server 的 Active Directory 身份验证。
ms.date: 04/01/2019
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: da2de7b66923ce82813fd8863556ac08b422bded
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350953"
---
# <a name="active-directory-authentication-for-sql-server-on-linux"></a>Linux 上的 SQL Server 的 Active Directory 身份验证

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文概述了 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的 Active Directory (AD) 身份验证。 AD 身份验证也称为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 上的集成身份验证。

## <a name="ad-authentication-overview"></a>AD 身份验证概述

通过 AD 身份验证，Windows 或 Linux 上的加入域的客户端能够使用其域凭据和 Kerberos 协议向 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进行身份验证。

与 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证相比，AD 身份验证具有以下优势：

- 用户通过单一登录进行身份验证，不会收到密码输入提示。
- 通过为 AD 组创建登录名，你可以使用 AD 组成员身份管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的访问和权限。  
- 每个用户在你的组织中都有单个标识，因此你无需跟踪哪些 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 登录名对应哪些人员。   
- AD 使你能够在整个组织中强制实施集中式密码策略。

## <a name="configuration-steps"></a>配置步骤

要使用 Active Directory 身份验证，你的网络上必须有一个 AD 域控制器 (Windows)。

有关如何配置 AD 身份验证的详细信息，请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)。 以下列表提供了一个摘要，其中包含指向教程中每个部分的链接：

1. [将 SQL Server 主机加入 Active Directory 域](sql-server-linux-active-directory-join-domain.md)。
1. [为 SQL Server 创建 AD 用户并设置 ServicePrincipalName](sql-server-linux-active-directory-authentication.md#createuser)。
1. [配置 SQL Server 服务 keytab](sql-server-linux-active-directory-authentication.md#configurekeytab)。
1. [保护 keytab 文件](sql-server-linux-active-directory-authentication.md#configurekeytab)。
1. [将 SQL Server 配置为使用 keytab 文件进行 Kerberos 身份验证](sql-server-linux-active-directory-authentication.md#configurekeytab)。
1. [在 Transact-SQL 中创建基于 AD 的 SQL Server 登录名](sql-server-linux-active-directory-authentication.md#createsqllogins)。
1. [使用 AD 身份验证连接到 SQL Server](sql-server-linux-active-directory-authentication.md#connect)。

## <a name="known-issues"></a>已知问题

- 目前，数据库镜像终结点支持的唯一身份验证方法是“证书”。 将来的版本中将启用 WINDOWS 身份验证方法。
- Linux 上的 SQL Server 不支持使用 NTLM 协议进行远程连接。 可能使用 NTLM 进行本地连接。

## <a name="next-steps"></a>后续步骤

有关如何为 Linux 上的 SQL Server 实现 Active Directory 身份验证的详细信息，请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)。
