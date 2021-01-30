---
description: sp_change_subscription_properties (Transact-SQL)
title: sp_change_subscription_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_subscription_properties_TSQL
- sp_change_subscription_properties
helpviewer_keywords:
- sp_change_subscription_properties
ms.assetid: cf8137f9-f346-4aa1-ae35-91a2d3c16f17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed6615b10487744ae5ac5c0775627bea97e29c66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208301"
---
# <a name="sp_change_subscription_properties-transact-sql"></a>sp_change_subscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更新请求订阅信息。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_change_subscription_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publication_type = ] publication_type ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @property = ] 'property'` 要更改的属性。 *属性* 为 **sysname**。  
  
`[ @value = ] 'value'` 属性的新值。 *值* 为 **nvarchar (1000)**，无默认值。  
  
`[ @publication_type = ] publication_type` 指定发布的复制类型。 *publication_type* 为 **int**，可以是下列值之一。  
  
|值|发布类型|  
|-----------|----------------------|  
|**0**|事务性|  
|**1**|快照|  
|**2**|合并|  
|NULL（默认值）|发布类型是由复制决定的。 因为存储过程必须浏览多个表，因此使用此选项时的执行速度要比提供了精确发布类型时的速度慢。|  
  
 下表说明项目的属性和这些属性的值。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||指定快照的备用文件夹的位置。 如果设置为 NULL，则将从发布服务器指定的默认位置提取快照文件。|  
|**distrib_job_login**||用来运行代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的登录名。|  
|**distrib_job_password**||用来运行代理的 Windows 帐户的密码。|  
|**distributor_login**||分发服务器登录。|  
|**distributor_password**||分发服务器密码。|  
|**distributor_security_mode**|**1**|连接分发服务器时，使用 Windows 身份验证。|  
||**0**|连接分发服务器时，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|dts_package_name||指定 SQL Server 2000 Data Transformation Services (DTS) 包的名称。 仅当发布为事务发布或快照发布时才能指定该值。|  
|dts_package_password||指定包上的密码。 *dts_package_password* 的默认 **值为 NULL** ，默认值为 NULL，指定密码属性保持不变。<br /><br /> 注意： DTS 包必须具有密码。<br /><br /> 仅当发布为事务发布或快照发布时才能指定该值。|  
|dts_package_location||DTS 包的存储位置。 仅当发布为事务发布或快照发布时才能指定该值。|  
|**dynamic_snapshot_location**||指定保存快照文件的文件夹的路径。 仅当发布为合并发布时才能指定该值。|  
|**ftp_address**||仅为保持向后兼容。|  
|**ftp_login**||仅为保持向后兼容。|  
|**ftp_password**||仅为保持向后兼容。|  
|**ftp_port**||仅为保持向后兼容。|  
|**hostname**||连接到发布服务器时使用的主机名称。|  
|**internet_login**||在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录名。|  
|**internet_password**||在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理使用的密码。|  
|**internet_security_mode**|**1**|使用 Windows 集成身份验证进行 Web 同步。 建议您将基本身份验证与 Web 同步结合使用。 有关详细信息，请参阅 [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md)。|  
||**0**|使用基本身份验证进行 Web 同步。<br /><br /> 注意： Web 同步要求与 Web 服务器建立 TLS 连接。|  
|**internet_timeout**||Web 同步请求过期之前的时间长度（秒）。|  
|**internet_url**||表示 Web 同步的复制侦听器位置的 URL。|  
|**merge_job_login**||用来运行代理的 Windows 帐户的登录名。|  
|**merge_job_password**||用来运行代理的 Windows 帐户的密码。|  
|**publisher_login**||发布服务器登录。 只有对合并发布的订阅才支持更改 *publisher_login* 。|  
|**publisher_password**||发布者密码。 只有对合并发布的订阅才支持更改 *publisher_password* 。|  
|**publisher_security_mode**|**1**|连接发布服务器时，使用 Windows 身份验证。 只有对合并发布的订阅才支持更改 *publisher_security_mode* 。|  
||**0**|连接发布服务器时，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**use_ftp**|true|使用 FTP 代替常规协议来检索快照。|  
||**false**|使用常规协议来检索快照。|  
|**use_web_sync**|true|启用 Web 同步。|  
||**false**|禁用 Web 同步。|  
|**working_directory**||使用文件传输协议 (FTP) 传输快照文件时，用于临时存储发布的数据和架构文件的工作目录的名称。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_change_subscription_properties** 在所有类型的复制中使用。  
  
 **sp_change_subscription_properties** 用于请求订阅。  
  
 对于 Oracle 发布服务器，将忽略 *publisher_db* 的值，因为 Oracle 仅允许每个服务器实例有一个数据库。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_change_subscription_properties**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改请求订阅属性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [sp_addmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_addmergepullsubscription_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_addpullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_addpullsubscription_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
