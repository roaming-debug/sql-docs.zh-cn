---
description: sp_helpdistpublisher (Transact-SQL)
title: sp_helpdistpublisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistpublisher_TSQL
- sp_helpdistpublisher
helpviewer_keywords:
- sp_helpdistpublisher
ms.assetid: f207c22d-8fb2-4756-8a9d-6c51d6cd3470
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 461a186656cbef0d1a83eecb2a3e58d63ab896ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209304"
---
# <a name="sp_helpdistpublisher-transact-sql"></a>sp_helpdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回使用分发服务器的发布服务器的属性。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdistpublisher [ [ @publisher=] 'publisher']   
    [ , [ @check_user = ] check_user  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 要为其返回属性的发布服务器。 *发布服务器* 的 **sysname** 为，默认值为 **%** 。  
  
`[ @check_user = ] check_user` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|发布者的名称。|  
|**distribution_db**|**sysname**|指定的发布服务器的分发数据库。|  
|**security_mode**|**int**|复制代理连接到发布服务器进行排队更新订阅时所用的安全模式，或者用于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器的安全模式。<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> **1** = Windows 身份验证|  
|**login**|**sysname**|复制代理连接到发布服务器进行排队更新订阅时的登录名，或者用于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器的登录名。|  
|**password**|**nvarchar (524)**|返回的密码（采用简单加密格式）。 对于除 **sysadmin** 以外的用户，密码为 NULL。|  
|**active**|**bit**|指示远程发布服务器是否将本地服务器用作分发服务器：<br /><br /> 0 = 否<br /><br /> 1 = 是|  
|**working_directory**|**nvarchar(255)**|工作目录的名称。|  
|**受信任**|**bit**|指示发布服务器连接到分发服务器时是否需要密码。 对于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本，这应该始终返回 **0**，这意味着密码是必需的。|  
|**thirdparty_flag**|**bit**|指示发布是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启用还是由第三方应用程序启用：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 、oracle 或 oracle 网关发布服务器。<br /><br /> **1** = 发布服务器已与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用第三方应用程序集成。|  
|**publisher_type**|**sysname**|发布服务器的类型；可以为下列值之一：<br /><br /> **MSSQLSERVER**<br /><br /> **联手**<br /><br /> **ORACLE GATEWAY**|  
|**publisher_data_source**|**nvarchar(4000)**|发布服务器中 OLE DB 数据源的名称。|  
|**storage_connection_string**|**nvarchar(4000)**|适用于 Azure SQL 数据库中的分发服务器或发布服务器的工作目录的存储访问密钥。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpdistpublisher** 在所有类型的复制中使用。  
  
 **sp_helpdistpublisher** 不会在非 **sysadmin** 登录的结果集中显示发布服务器的登录名或密码。  
  
## <a name="permissions"></a>权限  
 **Sysadmin** 固定服务器角色的成员可以对使用本地服务器作为分发服务器的任何发布服务器执行 **sp_helpdistpublisher** 。 **Db_owner** 固定数据库角色的成员或分发数据库中的 **replmonitor** 角色的成员可以对使用该分发数据库的任何发布服务器执行 **sp_helpdistpublisher** 。 指定 *发布服务器* 上发布的发布访问列表中的用户可能会执行 **sp_helpdistpublisher**。 如果未指定 *发布服务器* ，则会为用户有权访问的所有发布服务器返回信息。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
