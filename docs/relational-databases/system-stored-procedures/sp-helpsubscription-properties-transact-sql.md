---
description: sp_helpsubscription_properties (Transact-SQL)
title: sp_helpsubscription_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_properties
- sp_helpsubscription_properties_TSQL
helpviewer_keywords:
- sp_helpsubscription_properties
ms.assetid: 7a76a645-97eb-47ac-b3ea-e2d75012cbed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 681a72a604af765311bb10833cc17842819e97f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103117"
---
# <a name="sp_helpsubscription_properties-transact-sql"></a>sp_helpsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  从 [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) 表中检索安全信息。 此存储过程在订阅服务器上执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpsubscription_properties [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db =] 'publisher_db' ]   
    [ , [ @publication =] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的默认值为 **sysname**，默认值为 **%** ，它返回所有发布服务器的信息。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 **%** ，它返回所有发布服务器数据库的信息。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** ，它返回所有发布的信息。  
  
`[ @publication_type = ] publication_type` 发布的类型。*publication_type* 的值为 **int**，默认值为 NULL。 如果提供，则 *publication_type* 必须是下列值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**0**|事务发布|  
|**1**|快照发布|  
|**2**|合并发布|  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布服务器数据库名。|  
|**发布**|**sysname**|发布的名称。|  
|**publication_type**|**int**|发布的类型：<br /><br /> **0** = 事务<br /><br /> **1** = 快照<br /><br /> **2** = 合并|  
|**publisher_login**|**sysname**|在发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**publisher_password**|**nvarchar (524)**|用于在发布服务器上进行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证 (加密) 的密码。|  
|**publisher_security_mode**|**int**|发布服务器上使用的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> **1** = Windows 身份验证|  
|**发行人**|**sysname**|分发服务器的名称。|  
|**distributor_login**|**sysname**|分发服务器登录。|  
|**distributor_password**|**nvarchar (524)**|分发服务器密码（已加密）。|  
|**distributor_security_mode**|**int**|分发服务器上使用的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> **1** = Windows 身份验证|  
|**ftp_address**|**sysname**|仅为保持向后兼容。 服务器 (FTP) 服务的文件传输协议的网络地址。|  
|**ftp_port**|**int**|仅为保持向后兼容。 分发服务器的 FTP 服务端口号。|  
|**ftp_login**|**sysname**|仅为保持向后兼容。 用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**nvarchar (524)**|仅为保持向后兼容。 用于连接到 FTP 服务的用户密码。|  
|**alt_snapshot_folder**|**nvarchar(255)**|指定快照的备用文件夹的位置。|  
|**working_directory**|**nvarchar(255)**|用于存储数据和架构文件的工作目录的名称。|  
|**use_ftp**|**bit**|指定使用 FTP 而非常规协议检索快照。 如果为 **1**，则使用 FTP。|  
|dts_package_name|**sysame**|指定 Data Transformation Services (DTS) 包的名称。|  
|dts_package_password|**nvarchar (524)**|指定用于包的密码（如果有）。|  
|dts_package_location|**int**|DTS 包的存储位置。<br /><br /> **0** = 包位置在分发服务器上。<br /><br /> **1** = 包位置在订阅服务器上。|  
|**offload_agent**|**bit**|指定是否可以远程激活代理。 如果为 **0**，则代理无法远程激活。|  
|**offload_server**|**sysname**|指定用于远程激活的服务器所在的网络的名称。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|指定保存快照文件的文件夹的路径。|  
|**use_web_sync**|**bit**|指定是否可以通过 HTTPS 同步订阅，如果值为 **1** ，则表示已启用此功能。|  
|**internet_url**|**nvarchar(260)**|表示 Web 同步的复制侦听器位置的 URL。|  
|**internet_login**|**nvarchar(128)**|在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录名。|  
|**internet_password**|**nvarchar (524)**|在使用基本身份验证连接到承载 Web 同步的 Web 服务器时，合并代理所使用的登录密码。|  
|**internet_security_mode**|**int**|连接到承载 Web 同步的 Web 服务器时使用的身份验证模式，其中值 **1** 表示 Windows 身份验证，值为 **0** 表示基本身份验证。|  
|**internet_timeout**|**int**|Web 同步请求过期之前的时间长度（秒）。|  
|**hostname**|**nvarchar(128)**|在 WHERE 子句参数化行筛选器中使用 HOST_NAME() 函数时，指定此函数的值。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpsubscription_properties** 用于快照复制、事务复制和合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_helpsubscription_properties**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
