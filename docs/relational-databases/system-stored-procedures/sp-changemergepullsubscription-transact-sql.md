---
description: sp_changemergepullsubscription (Transact-SQL)
title: sp_changemergepullsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergepullsubscription
- sp_changemergepullsubscription_TSQL
helpviewer_keywords:
- sp_changemergepullsubscription
ms.assetid: 5e0d04f2-6175-44a2-ad96-a8e2986ce4c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f8590cd397f41ca4c4db8776f614f47b2b05c921
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159835"
---
# <a name="sp_changemergepullsubscription-transact-sql"></a>sp_changemergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改合并请求订阅的属性。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changemergepullsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @publisher= ] 'publisher' ]  
    [ , [ @publisher_db= ] 'publisher_db' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为%。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname** 为，默认值为%。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的值为 **sysname**，默认值为%。  
  
`[ @property = ] 'property'` 要更改的属性的名称。 *属性* 为 **sysname**，可以是表中的值之一。  
  
`[ @value = ] 'value'` 指定的属性的新值。 *值* 为 **nvarchar (255)**，可以是表中的值之一。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||快照文件夹的存储位置（如果该位置不同于默认位置或是默认位置之外的位置）。|  
|description||对该合并请求订阅的说明。|  
|**发行人**||分发服务器的名称。|  
|**distributor_login**||分发服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**distributor_password**||在分发服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（加密）。|  
|**distributor_security_mode**|**1**|连接分发服务器时，使用 Windows 身份验证。|  
||**0**|连接分发服务器时，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**dynamic_snapshot_location**||保存快照文件的文件夹的路径。|  
|**ftp_address**||仅为向后兼容而提供。 分发服务器的文件传输协议 (FTP) 服务的网络地址。|  
|**ftp_login**||仅为向后兼容而提供。 用于连接到 FTP 服务的用户名。|  
|**ftp_password**||仅为向后兼容而提供。 用于连接到 FTP 服务的用户密码。|  
|**ftp_port**||仅为向后兼容而提供。 分发服务器 FTP 服务的端口号。|  
|**hostname**||在联接筛选器或逻辑记录关系的 WHERE 子句中使用 HOST_NAME() 函数时，指定该函数的值。|  
|**internet_login**||在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录名。|  
|**internet_password**||在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录密码。|  
|**internet_security_mode**|**1**|在连接到承载 Web 同步的 Web 服务器时使用 Windows 身份验证。|  
||**0**|在连接到承载 Web 同步的 Web 服务器时使用基本身份验证。|  
|**internet_timeout**||Web 同步请求过期之前的时间长度（秒）。|  
|**internet_url**||表示 Web 同步的复制侦听器位置的 URL。|  
|**merge_job_login**||用来运行代理的 Windows 帐户的登录名。|  
|**merge_job_password**||用来运行代理的 Windows 帐户的密码。|  
|**priority**||仅可用于向后兼容;改为在发布服务器上运行 [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md) ，以修改订阅的优先级。|  
|**publisher_login**||在发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**publisher_password**||在发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（加密）。|  
|**publisher_security_mode**|**0**|连接发布服务器时，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
||**1**|连接发布服务器时，使用 Windows 身份验证。|  
||**2**|同步触发器使用静态 **sysservers** 项执行远程过程调用 (RPC) ，发布服务器必须在 **sysservers** 表中定义为远程服务器或链接服务器。|  
|**sync_type**|**自动**|已发布表的架构和初始数据将首先传输到订阅服务器。|  
||**无**|订阅服务器已经具有已发布表的架构和初始数据；将始终传输系统表和数据。|  
|**use_ftp**|true|使用 FTP 而不是典型协议来检索快照。|  
||**false**|使用典型协议来检索快照。|  
|**use_web_sync**|true|可以通过 HTTP 同步订阅。|  
||**false**|不能通过 HTTP 同步订阅。|  
|**use_interactive_resolver**|true|在调解过程中使用交互式冲突解决程序。|  
||**false**|不使用交互式冲突解决程序。|  
|**working_directory**||如果指定了使用 FTP 的选项，则是使用 FTP 将快照文件传输到的目录的完全限定路径。|  
|NULL（默认值）||返回 *属性* 的支持值的列表。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changemergepullsubscription** 用于合并复制。  
  
 假定当前服务器和当前数据库分别是订阅服务器和订阅服务器数据库。  
  
 更改代理登录名或密码之后，必须先停止并重新启动代理，然后更改才能生效。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changemergepullsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改请求订阅属性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [sp_addmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
