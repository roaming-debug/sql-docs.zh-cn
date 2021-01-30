---
title: 'sp_MSchange_logreader_agent_properties (T-sql) '
description: 描述用于更改 SQL Server 复制拓扑的日志读取器代理属性的 sp_MSchange_logreader_agent_properties 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_logreader_agent_properties_TSQL
- sp_MSchange_logreader_agent_properties
helpviewer_keywords:
- sp_MSchange_logreader_agent_properties
ms.assetid: 925df9d3-a041-4046-8e17-c47f40edb86d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 84508c1437ae6ef51f1904dfbd414b0e373b32e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195443"
---
# <a name="sp_mschange_logreader_agent_properties-transact-sql"></a>sp_MSchange_logreader_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的分发服务器上运行的日志读取器代理作业的属性。 当发布服务器在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 实例上运行时，可使用此存储过程更改属性。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_MSchange_logreader_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 连接到发布服务器时代理所使用的安全模式。 *publisher_security_mode* 为 **smallint**，无默认值。  
  
 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。  
  
 **1** 指定 Windows 身份验证。  
  
`[ @publisher_login = ] 'publisher_login'` 连接到发布服务器时使用的登录名。 *publisher_login* **sysname**，无默认值。 当 *publisher_security_mode* 为 **0** 时，必须指定 *publisher_login* 。 如果 *publisher_login* 为 NULL 且 *publisher_security_mode* 为 **1**，则在连接到发布服务器时将使用 *job_login* 中指定的 Windows 帐户。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password* **sysname**，无默认值。  
  
`[ @job_login = ] 'job_login'` 用于运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，无默认值。 *不能更改非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*发布者。*  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。  
  
`[ @publisher_type = ] 'publisher_type'` 指定发布服务器未在实例中运行时的发布服务器类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *publisher_type* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。|  
|**联手**|指定标准的 Oracle 发布服务器。|  
|**ORACLE GATEWAY**|指定 Oracle 网关发布服务器。|  
  
 有关 Oracle 发布服务器和 Oracle 网关发布服务器之间的差异的详细信息，请参阅 [Oracle 发布概述](../../relational-databases/replication/non-sql/oracle-publishing-overview.md)。  
  
## <a name="remarks"></a>备注  
 **sp_MSchange_logreader_agent_properties** 用于事务复制。  
  
 执行 **sp_MSchange_logreader_agent_properties** 时必须指定所有参数。 执行 [sp_helplogreader_agent &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) 以返回日志读取器代理作业的当前属性。  
  
 更改代理登录名或密码之后，必须先停止并重新启动代理，然后更改才能生效。  
  
 当发布服务器在或更高版本的实例上运行时 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，应使用 [sp_changelogreader_agent](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md) 更改日志读取器代理的属性。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员才能 **sp_MSchange_logreader_agent_properties** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addlogreader_agent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
  
