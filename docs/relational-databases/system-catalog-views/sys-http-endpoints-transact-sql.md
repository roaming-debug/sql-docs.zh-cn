---
description: sys.http_endpoints (Transact-SQL)
title: sys.http_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.http_endpoints
- http_endpoints
- sys.http_endpoints_TSQL
- http_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.http_endpoints catalog view
ms.assetid: 16f59695-ecd9-457e-8874-055af63f8ea7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60b9aa9ad9ef11b88e80a13bfb7469050034cfca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191428"
---
# <a name="syshttp_endpoints-transact-sql"></a>sys.http_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

  对于在使用 HTTP 协议的服务器中创建的每个端点在表中均对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**< 继承列>**||从 sys.databases 中继承列 [&#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)。|  
|**网站**|**nvarchar(128)**|站点的主机名，如 SITE = 选项中指定的那样。|  
|**url_path**|**nvarchar(4000)**|此 HTTP 端点 URL 的仅路径部分，如 PATH= 选项中指定的那样。|  
|**is_clear_port_enabled**|**bit**|1 = 使用 PORT = CLEAR 选项启用清除端口。|  
|**clear_port**|**int**|在 CLEAR PORT = 选项中指定的端口号。<br /><br /> NULL = 未指定。|  
|**is_ssl_port_enabled**|**bit**|1 = 使用 PORT = SSL 选项启用 SSL 端口。|  
|**ssl_port**|**int**|在 SSL PORT = 选项中指定的端口号值。<br /><br /> NULL = 未指定。|  
|**is_anonymous_enabled**|**bit**|1 = 使用 AUTHENTICATION = ANONYMOUS 选项启用匿名访问。|  
|**is_basic_auth_enabled**|**bit**|1 = 使用 AUTHENTICATION = BASIC 选项启用基本身份验证。|  
|**is_digest_auth_enabled**|**bit**|1 = 使用 AUTHENTICATION = DIGEST 选项启用摘要式身份验证。|  
|**is_kerberos_auth_enabled**|**bit**|1 = 使用 AUTHENTICATION = KERBEROS 选项启用集成身份验证。|  
|**is_ntlm_auth_enabled**|**bit**|1 = 使用 AUTHENTICATION = NTLM 选项启用集成身份验证。|  
|**is_integrated_auth_enabled**|**bit**|1 = 使用 AUTHENTICATION = INTEGRATED 选项启用集成身份验证。|  
|**authorization_realm**|**nvarchar(128)**|作为 HTTP DIGEST 身份验证质询的一部分而返回到客户端的提示。 AUTH REALM 选项的值。<br /><br /> 如果未指定或 DIGEST 身份验证未启用，则为 NULL。|  
|**default_logon_domain**|**nvarchar(128)**|启用 BASIC 身份验证时的默认登录域。 DEFAULT LOGON DOMAIN 选项的值。<br /><br /> 如果未指定该选项值或 BASIC 身份验证未启用，则为 NULL。|  
|**is_compression_enabled**|**bit**|1 = 设置了 COMPRESSION = ENABLED 选项。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)  
  
  
