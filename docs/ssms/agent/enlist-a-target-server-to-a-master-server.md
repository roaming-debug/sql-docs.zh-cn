---
description: 将目标服务器登记到主服务器
title: 将目标服务器登记到主服务器
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- enlisting target servers [SQL Server]
- SQL Server Agent jobs, target servers
- master servers [SQL Server], enlisting target servers
- SQL Server Agent jobs, master servers
- target servers [SQL Server], enlisting
ms.assetid: 7633adb5-d140-4e58-a8f2-5b4b50c2f95b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 482da129c638daafe7d5a86a1d4e8d33810824ae
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251063"
---
# <a name="enlist-a-target-server-to-a-master-server"></a>将目标服务器登记到主服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何将目标服务器添加到多服务器管理配置中。 从主服务器运行此过程。 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]，或使用 SQL Server 管理对象 (SMO)。  
  
有关用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务的 Windows 帐户如何影响多服务器环境的信息，请参阅 [创建多服务器环境](../../ssms/agent/create-a-multiserver-environment.md)。  
  
默认情况下，将为主服务器和目标服务器之间的连接启用完全传输层安全 (TLS)（以前称为“安全套接字层 (SSL)”）加密和证书验证。 有关详细信息，请参阅 [在目标服务器上设置加密选项](../../ssms/agent/set-encryption-options-on-target-servers.md)  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-enlist-a-target-server"></a>登记目标服务器  
  
1.  在对象资源管理器中，展开配置为主服务器的服务器。   
  
2.  右键单击“SQL Server 代理”  ，指向“多服务器管理”  ，然后单击“添加目标服务器”  。  
  
3.  完成目标服务器向导，它将指导您完成该进程。  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-enlist-a-target-server"></a>登记目标服务器  
  
1.  使用 **sp_msx_enlist** 存储过程。  有关详细信息，请参阅 [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
[企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)  
