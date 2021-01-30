---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 570bfade322ef3b3747619cf2a0779a3cf6ec392
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102721"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsubscription_properties** 表包含在订阅服务器上运行复制代理所需的参数信息的行。 对于请求订阅，该表存储在订阅服务器的订阅数据库中；对于推送订阅，该表存储在分发服务器的分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布服务器数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**publication_type**|**int**|发布类型：<br /><br /> **0** = 事务性。<br /><br /> **2** = 合并。|  
|**publisher_login**|**sysname**|发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**publisher_password**|**nvarchar (524)**|发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（已加密）。|  
|**publisher_security_mode**|**int**|在发布服务器上实现的安全模式：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server 身份验证。<br /><br /> **1** 个  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证。<br /><br /> **2** = 同步触发器使用静态 **sysservers** 项执行远程过程调用 (RPC) ， *发布* 服务器必须在 **sysservers** 表中定义为远程服务器或链接服务器。|  
|**发行人**|**sysname**|分发服务器的名称。|  
|**distributor_login**|**sysname**|在分发服务器上用于 SQL Server 身份验证的登录 ID。|  
|**distributor_password**|**nvarchar (524)**|分发服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（加密）。|  
|**distributor_security_mode**|**int**|在分发服务器上实现的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。<br /><br /> **1** = Windows 身份验证。|  
|**ftp_address**|**sysname**|文件传输协议的网络地址 (分发服务器的 FTP) 服务。|  
|**ftp_port**|**int**|分发服务器的 FTP 服务的端口号。|  
|**ftp_login**|**sysname**|用于连接到 FTP 服务的用户名。|  
|**ftp_password**|**nvarchar (524)**|用于连接到 FTP 服务的 u. User 密码。|  
|**alt_snapshot_folder**|**nvarchar(255)**|指定快照的备用文件夹的位置。|  
|**working_directory**|**nvarchar(255)**|用于存储数据和架构文件的工作目录的名称。|  
|**use_ftp**|**bit**|指定使用 FTP 而非常规协议检索快照。 如果为 **1**，则使用 FTP。|  
|dts_package_name|**sysname**|指定 Data Transformation Services (DTS) 包的名称。|  
|dts_package_password|**nvarchar (524)**|指定包上的密码。|  
|dts_package_location|**int**|存储 DTS 包的位置。|  
|**enabled_for_syncmgr**|**bit**|指定能否通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 同步管理器同步订阅。<br /><br /> **0** = 未向同步管理器注册订阅。<br /><br /> **1** = 订阅已向同步管理器注册，可以在不启动的情况下同步 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。|  
|**offload_agent**|**bit**|指定是否可以远程激活代理。 如果为 **0**，则代理无法远程激活。|  
|**offload_server**|**sysname**|指定用于远程激活的服务器所在的网络的名称。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|指定保存快照文件的文件夹的路径。|  
|**use_web_sync**|**bit**|指定能否通过 HTTP 同步订阅。 如果值为 **1** ，则表示已启用此功能。|  
|**internet_url**|**nvarchar(260)**|表示 Web 同步的复制侦听器所在位置的 URL。|  
|**internet_login**|**sysname**|合并代理使用身份验证连接到承载 Web 同步的 Web 服务器时使用的登录名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**internet_password**|**nvarchar (524)**|当使用身份验证连接到承载 Web 同步的 Web 服务器时，合并代理使用的登录名的密码 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**internet_security_mode**|**int**|连接到承载 Web 同步的 Web 服务器时使用的身份验证模式，其中值 **1** 表示 Windows 身份验证，值 **0** 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**internet_timeout**|**int**|表示 Web 同步请求在多长时间之后过期的时间长度（秒）。|  
|**hostname**|**sysname**|指定在联接筛选器或逻辑记录关系的 **WHERE** 子句中使用此函数时 **HOST_NAME** 的值。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
