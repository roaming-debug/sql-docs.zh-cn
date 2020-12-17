---
description: 创建多服务器环境
title: 创建多服务器环境
ms.custom: seo-lt-2019
ms.date: 01/30/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, multiserver environments
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- multiserver environments [SQL Server]
ms.assetid: edc2b60d-15da-40a1-8ba3-f1d473366ee6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 66a3faf1f419bb92aee91ac1621734560fd16a4c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466518"
---
# <a name="create-a-multiserver-environment"></a>创建多服务器环境
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

多服务器管理需要设置一个主服务器 (MSX) 以及一个或多个目标服务器 (TSX)。 首先在主服务器上定义将在所有目标服务器上处理的作业，然后将这些作业下载到目标服务器。  
  
默认情况下，将为主服务器和目标服务器之间的连接启用完全传输层安全 (TLS)（以前称为“安全套接字层 (SSL)”）加密和证书验证。 有关详细信息，请参阅 [在目标服务器上设置加密选项](../../ssms/agent/set-encryption-options-on-target-servers.md)  
  
如果您具有大量目标服务器，应避免通过其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能在具有重大性能要求的生产服务器上定义主服务器，因为目标服务器通信量可能会降低生产服务器的性能。 如果还将事件转发到专用的主服务器，则可以在一台服务器上集中管理。 有关详细信息，请参阅 [管理事件](../../ssms/agent/manage-events.md)。  
  
> [!NOTE]  
> 若要使用多服务器作业处理， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户必须是主服务器上 **msdb** 数据库角色 **TargetServersRole** 的成员。 “主服务器向导”将服务帐户自动添加到此角色，以作为登记过程的一部分。  
  
## <a name="considerations-for-multiserver-environments"></a>多服务器环境的注意事项  
  
创建多服务器环境时，考虑下列问题：  
  
-   使用最新版作为主服务器。 支持当前版本和前两个版本。

-   每台目标服务器只向一台主服务器报告。 必须将目标服务器从一台主服务器上脱离，才能将其登记在其他服务器上。  
  
-   更改目标服务器的名称时，必须在更改名称之前将其脱离并在更改后重新登记。  
  
-   若要取消多服务器配置，必须使所有目标服务器脱离主服务器。  
  
-   SQL Server Integration Services 仅支持版本与主服务器版本相同或更高的目标服务器。  
  
## <a name="related-tasks"></a>Related Tasks  
以下主题介绍创建多服务器环境的常见任务：  
  
|说明|主题|  
|---------------|---------|  
|描述如何创建主服务器。|[设置主服务器](../../ssms/agent/make-a-master-server.md)|  
|描述如何创建目标服务器。|[设置目标服务器](../../ssms/agent/make-a-target-server.md)|  
|描述如何将目标服务器登记到主服务器。|[将目标服务器登记到主服务器](../../ssms/agent/enlist-a-target-server-to-a-master-server.md)|  
|描述如何使目标服务器从主服务器脱离。|[将目标服务器从主服务器脱离](../../ssms/agent/defect-a-target-server-from-a-master-server.md)|  
|描述如何使多台目标服务器脱离主服务器。|[将多台目标服务器从主服务器脱离](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)|  
|描述如何检查目标服务器的状态。|[sp_help_targetserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)<br /><br />[sp_help_targetservergroup (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-targetservergroup-transact-sql.md)|  
  
## <a name="see-also"></a>另请参阅  
[排除使用代理的多服务器作业的故障](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)  
