---
description: sys.linked_logins (Transact-SQL)
title: sys.linked_logins (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.linked_logins
- sys.linked_logins_TSQL
- linked_logins_TSQL
- linked_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.linked_logins catalog view
ms.assetid: af57bf0c-a265-410f-9bab-63b78569b4a6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cd771e7199ce5a120ad0ef6c4c5f0546cfdb426c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191374"
---
# <a name="syslinked_logins-transact-sql"></a>sys.linked_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个链接服务器登录映射返回一行，供 RPC 以及从本地服务器到相应的链接服务器的分布式查询使用。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|**Sys.databases** 中的服务器的 ID。|  
|**local_principal_id**|**int**|对其应用映射的服务器主体。<br /><br /> 0 = 通配符或公共主体。|  
|**uses_self_credential**|**bit**|如果为 1，则映射表示会话应使用它自己的凭据；否则为 0，表示会话使用提供的名称和密码。|  
|**remote_name**|**sysname**|连接时使用的远程用户名。 虽然也存储了密码，但并不显示在目录视图界面中。|  
|modify_date|**datetime**|上次更改该链接登录名的日期。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [链接服务器目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)  
  
  
