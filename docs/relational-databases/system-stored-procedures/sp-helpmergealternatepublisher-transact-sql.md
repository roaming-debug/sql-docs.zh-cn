---
description: sp_helpmergealternatepublisher (Transact-SQL)
title: sp_helpmergealternatepublisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergealternatepublisher_TSQL
- sp_helpmergealternatepublisher
helpviewer_keywords:
- sp_helpmergealternatepublisher
ms.assetid: a96e365f-5967-4580-9d79-5bacf2d12211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7448059958f07ba1aab7e63a1a857e113e21432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183106"
---
# <a name="sp_helpmergealternatepublisher-transact-sql"></a>sp_helpmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回作为合并发布的备用发布服务器启用的所有服务器列表。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergealternatepublisher [ @publisher = ] 'publisher', [ @publisher_db = ] 'publisher_db', [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 备用发布服务器的名称。*发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。*publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 发布的名称。*发布* 为 **sysname**，无默认值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**alternate_publisher**|**sysname**|备用发布服务器的名称。|  
|**alternate_publisher_db**|**sysname**|发布数据库的名称。|  
|**alternate_publication**|**sysname**|发布的名称。|  
|**alternate_distributor**|**sysname**|分发服务器的名称。|  
|**friendly_name**|**nvarchar(255)**|对备用发布服务器的说明。|  
|**enabled**|**bit**|指定服务器是否为备用发布服务器。 **1** 指定将发布服务器启用为备用发布服务器。 **0** 指定不启用。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergealternatepublisher** 用于合并复制。  
  
 在每个合并会话过程中，系统向发布服务器和订阅服务器查询它们各自的备用发布服务器列表。 合并进程将添加或删除备用发布服务器列表项，从而使订阅服务器和发布服务器中的备用发布服务器列表相匹配。  
  
## <a name="permissions"></a>权限  
 只有发布的发布访问列表的成员才能 **sp_helpmergealternatepublisher** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
