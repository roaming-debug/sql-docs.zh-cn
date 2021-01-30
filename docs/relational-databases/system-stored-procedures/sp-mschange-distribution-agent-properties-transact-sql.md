---
title: 'sp_MSchange_distribution_agent_properties (T-sql) '
description: 描述用于更改 SQL Server 复制拓扑的分发代理属性的 sp_MSchange_distribution_agent_properties 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_distribution_agent_properties
- sp_MSchange_distribution_agent_properties_TSQL
helpviewer_keywords:
- sp_MSchange_distribution_agent_properties
ms.assetid: 7dac5e68-bf84-433a-a531-66921f35126f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 37bb9e7af79fbc3ef99febd84cce78eb8db8a557
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195460"
---
# <a name="sp_mschange_distribution_agent_properties-transact-sql"></a>sp_MSchange_distribution_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的分发服务器上运行的分发代理作业的属性。 当发布服务器在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 实例上运行时，可使用此存储过程更改属性。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_MSchange_distribution_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的 **sysname**，无默认值。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* **sysname**，无默认值。  
  
`[ @property = ] 'property'` 要更改的发布属性。 *属性* 为 **sysname**，无默认值。  
  
`[ @value = ] 'value'` 新属性值。 *值* 为 **nvarchar (524)**，默认值为 NULL。  
  
 下表说明了可以更改的分发服务器代理作业的属性，以及对这些属性值的限制。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||用来运行代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的登录名。|  
|**distrib_job_password**||用来运行代理作业的 Windows 帐户的密码。|  
|**subscriber_catalog**||在与 OLE DB 访问接口建立连接时要使用的目录。 *此属性仅对非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*订阅服务器。*|  
|**subscriber_datasource**||OLE DB 访问接口识别的数据源的名称。 *此属性仅对非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*订阅服务器。*|  
|**subscriber_location**||OLE DB 访问接口识别的数据库的位置。 *此属性仅对非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*订阅服务器。*|  
|**subscriber_login**||在连接到订阅服务器以同步订阅时使用的登录名。|  
|**subscriber_password**||订阅服务器密码。<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_provider**||唯一编程标识符 (PROGID)，用于注册非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源的 OLE DB 访问接口。 *此属性仅对非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*订阅服务器。*|  
|**subscriber_providerstring**||OLE DB 访问接口特定的连接字符串，用于标识数据源。 *此属性仅对非 SQL Server 订阅服务器有效。*|  
|**subscriber_security_mode**|**1**|Windows 身份验证。<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**subscriber_type**|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器|  
||**1**|ODBC 数据源服务器|  
||**3**|OLE DB 访问接口|  
|**subscriptionstreams**||指示每个分发代理允许的连接数，用于将更改批并行应用于订阅服务器。 *不支持非* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*订阅服务器、Oracle 发布服务器或对等订阅。*|  
  
> [!NOTE]  
>  更改代理登录名或密码之后，必须先停止并重新启动代理，然后更改才能生效。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_MSchange_distribution_agent_properties** 用于快照复制和事务复制。  
  
 当发布服务器在或更高版本的实例上运行时 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，您应该使用 [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) 更改同步在分发服务器上运行的推送订阅的合并代理作业的属性。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员才能 **sp_MSchange_distribution_agent_properties** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addpushsubscription_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_addsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)  
  
  
