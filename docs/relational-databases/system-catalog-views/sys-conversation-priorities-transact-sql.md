---
description: sys.conversation_priorities (Transact-SQL)
title: sys.conversation_priorities (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- conversation_priorities_TSQL
- conversation_priorities
- sys.conversation_priorities_TSQL
- sys.conversation_priorities
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], priorities
- Service Broker, conversations
- sys.conversation_priorities catalog view
ms.assetid: 7cbb9171-3310-4aae-8458-755c882d6462
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5933f5f67b4c05859273f687136b7e38ed70e0fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211362"
---
# <a name="sysconversation_priorities-transact-sql"></a>sys.conversation_priorities (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为当前数据库中创建的每个会话优先级都包含一行，如下表所示： 
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|Priority_id|**int**|一个数字，用于唯一标识会话优先级。 不可为 NULL。|  
|name|**sysname**|会话优先级的名称。 不可为 NULL。|  
|service_contract_id|**int**|为会话优先级指定的约定的标识符。 它可以按 sys.service_contracts 中的 service_contract_id 列进行联接。 可以为 null.|  
|local_service_id|**int**|指定作为会话优先级的本地服务的服务标识符。 该列可以按 sys.services 中的 service_id 列进行联接。 可以为 null.|  
|remote_service_name|**nvarchar(256)**|指定作为会话优先级的远程服务的服务名称。 可以为 null.|  
|priority|**tinyint**|在此会话优先级中指定的优先级。 不可为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 下例通过使用联接来显示约定和本地服务名称，列出了会话优先级。  
  
```  
SELECT scp.name AS priority_name,  
       ssc.name AS contract_name,  
       ssvc.name AS local_service_name,  
       scp.remote_service_name,  
       scp.priority AS priority_level  
FROM sys.conversation_priorities AS scp  
    INNER JOIN sys.service_contracts AS ssc  
       ON scp.service_contract_id = ssc.service_contract_id  
    INNER JOIN sys.services AS ssvc  
       ON scp.local_service_id = ssvc.service_id  
ORDER BY priority_name, contract_name,  
         local_service_name, remote_service_name;  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys.databases &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-services-transact-sql.md)   
 [sys.service_contracts &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-service-contracts-transact-sql.md)  
  
  
