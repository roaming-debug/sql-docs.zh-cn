---
description: 代理帐户属性 - 新建代理帐户（“常规”页）
title: 代理帐户属性 - 新建代理帐户（“常规”页）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.proxy.general.f1
ms.assetid: 5cd81265-bf59-413b-8397-150ddc70d0c7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: efff382c4b65711f71e619ff33cee26c13210c87
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474398"
---
# <a name="proxy-account-properties---new-proxy-account-general-page"></a>代理帐户属性 - 新建代理帐户（“常规”页）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以查看或更改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户的属性。  
  
## <a name="options"></a>选项  
**代理名称**  
键入代理的名称。  
  
**凭据名称**  
键入代理的凭据名称。  
  
> [!NOTE]  
> 提供的凭据名称必须是一个现有凭据的名称。 有关创建凭据的信息，请参阅[如何：创建代理](../../relational-databases/security/authentication-access/create-a-credential.md)  
  
**...**  
启动“选择凭据”对话框。  
  
**说明**  
键入代理的说明。  
  
**对以下子系统有效**  
选择该代理帐户可以访问的子系统。  
  
**将作业步骤重新分配给**  
选择要将作业步骤重新分配给的代理。 在撤消对代理以前可访问的某个子系统的访问权限后，将启用此列表。  
  
## <a name="see-also"></a>另请参阅  
[创建 SQL Server 代理的代理帐户](../../ssms/agent/create-a-sql-server-agent-proxy.md)  
