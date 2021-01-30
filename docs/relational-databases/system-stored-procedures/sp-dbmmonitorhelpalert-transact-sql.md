---
description: sp_dbmmonitorhelpalert (Transact-SQL)
title: sp_dbmmonitorhelpalert (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorhelpalert_TSQL
- sp_dbmmonitorhelpalert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorhelpalert
- database mirroring [SQL Server], monitoring
ms.assetid: 43911660-b4e4-4934-8c02-35221160aaec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: edcf719e5778ebe7cd2f224c14f53552a2f28325
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212378"
---
# <a name="sp_dbmmonitorhelpalert-transact-sql"></a>sp_dbmmonitorhelpalert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回若干个关键数据库镜像监视器性能指标中的一个或所有指标的警告阈值信息。  
 
  ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbmmonitorhelpalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>参数  
 *database_name*  
 指定数据库。  
  
 [ *alert_id* ]  
 整数值，用于标识要返回的警告。 如果省略此参数，则将返回所有警告，但不包括保持期。  
  
 若要返回特定警告，请指定下列值之一：  
  
|值|性能指标|警告阈值|  
|-----------|------------------------|-----------------------|  
|1|最早的未发送事务|指定在主体服务器实例上生成警告之前，发送队列中可以累积的事务的分钟数。 该警告有助于度量数据丢失的可能性（以时间计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|2|未发送日志|指定未发送日志达到多少 KB 后，在主体服务器实例上生成一个警告。 该警告有助于度量数据丢失的可能性（以 KB 计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|3|未还原日志|指定未还原日志达到多少 KB 后，会在镜像服务器实例上生成一个警告。 此警告可以帮助度量故障转移时间。 “故障转移时间 ”主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间。|  
|4|镜像提交开销|指定在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。|  
|5|保留期|用于控制数据库镜像状态表中的行保留多长时间的元数据。|  
  
 有关与警告相对应的事件 Id 的详细信息，请参阅 [对镜像性能指标使用警告阈值和警报 &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 对于每个返回的警报，都返回包含以下列的行：  
  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|**alert_id**|**int**|下表列出了每个性能指标的 **alert_id** 值，以及 **sp_dbmmonitorresults** 结果集中显示的度量值的度量单位：|  
|**threshold**|**int**|警告的阈值。 如果在更新镜像状态时返回大于此阈值的值，则会在 Windows 事件日志中输入项。 此值表示 KB、分钟或毫秒，具体取决于警告。 如果当前未设置阈值，则该值是 NULL。<br /><br /> **注意：** 若要查看当前值，请运行 [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) 存储过程。|  
|**enabled**|**bit**|0 = 禁用事件。<br /><br /> 1 = 启用事件。<br /><br /> **注意：** 保持期始终处于启用状态。|  
  
|值|性能指标|单位|  
|-----------|------------------------|----------|  
|1|最早的未发送事务|分钟数|  
|2|未发送日志|KB|  
|3|未还原日志|KB|  
|4|镜像提交开销|毫秒|  
|5|保留期|小时|  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将返回一行，该行指示是否为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中最早的未发送事务的性能指标启用警告。  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012, 1 ;  
```  
  
 以下示例对每个性能指标均返回一行，该行指示是否在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中启用该指标。  
  
```  
EXEC sp_dbmmonitorhelpalert AdventureWorks2012;  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
