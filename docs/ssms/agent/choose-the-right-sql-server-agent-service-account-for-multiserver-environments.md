---
description: 为多服务器环境选择正确的 SQL Server 代理服务帐户
title: 为多服务器环境选择代理服务帐户
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, service accounts
- multiserver environments [SQL Server], SQL Server Agent service account behavior
ms.assetid: a07e2f38-281c-495b-965b-13fad03ba548
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: d5a25c6532207eb709d90ac1acb91c7ff8d031c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472358"
---
# <a name="choose-the-right-sql-server-agent-service-account-for-multiserver-environments"></a>为多服务器环境选择正确的 SQL Server 代理服务帐户

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务选择的 Windows 帐户可影响多服务器环境的行为，如下所示：  
  
-   如果使用非本地 Windows Administrators 组成员的帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务，则将目标服务器登记在主服务器中时可能会失败。 如果出现这种情况，则返回以下错误消息：  
  
    “登记操作失败。”  
  
    重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务以解决此问题。  
  
-   当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务使用本地系统帐户运行时，仅当主服务器和目标服务器位于同一台计算机上时，才支持主服务器 - 目标服务器操作。 如果使用此配置，则在将目标服务器登记到主服务器时返回以下信息：  
  
    “请确保 <target_server_computer_name> 的代理启动帐户拥有以 targetServer 身份登录的权限”。  
  
    您可以忽略此信息性消息。 登记操作将成功完成。  
  
有关如何选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户的信息，请参阅 [选择 SQL Server 代理服务帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)。  
