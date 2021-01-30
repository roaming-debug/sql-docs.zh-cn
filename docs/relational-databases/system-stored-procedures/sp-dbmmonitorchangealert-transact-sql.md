---
description: sp_dbmmonitorchangealert (Transact-SQL)
title: sp_dbmmonitorchangealert (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorchangealert_TSQL
- sp_dbmmonitorchangealert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorchangealert
- database mirroring [SQL Server], monitoring
ms.assetid: 1b29f82b-9cf8-4539-8d5c-9a1024db8a50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e250cc4fb95c32b677db304f5e4de321c14448a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212412"
---
# <a name="sp_dbmmonitorchangealert-transact-sql"></a>sp_dbmmonitorchangealert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  添加或更改指定镜像性能指标的警告阈值。  

  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbmmonitorchangealert database_name   
    , alert_id   
    , alert_threshold   
    , enabled   
```  
  
## <a name="arguments"></a>参数  
 *database_name*  
 指定将要添加或更改其指定警告阈值的数据库。  
  
 *alert_id*  
 整数值，用于标识要添加或更改的警告。 指定以下值之一：  
  
|值|性能指标|警告阈值|  
|-----------|------------------------|-----------------------|  
|1|最早的未发送事务|指定在主体服务器实例上生成警告之前，发送队列中可以累积的事务的分钟数。 该警告有助于度量数据丢失的可能性（以时间计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|2|未发送日志|指定未发送日志达到多少 KB 后，在主体服务器实例上生成一个警告。 该警告有助于度量数据丢失的可能性（以 KB 计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|3|未还原日志|指定未还原日志达到多少 KB 后，会在镜像服务器实例上生成一个警告。 此警告可以帮助度量故障转移时间。 “故障转移时间 ”主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间。|  
|4|镜像提交开销|指定在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。|  
|5|保留期|用于控制数据库镜像状态表中的行保留多长时间的元数据。|  
  
 有关与警告相对应的事件 Id 的详细信息，请参阅 [对镜像性能指标使用警告阈值和警报 &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)。  
  
 *alert_threshold*  
 警告的阈值。 如果在更新镜像状态时返回大于此阈值的值，则会在 Windows 事件日志中输入项。 此值表示 KB、分钟或毫秒，具体取决于性能指标。  
  
> [!NOTE]  
>  若要查看当前值，请运行 [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) 存储过程。  
  
 *enabled*  
 警告是否已启用？  
  
 0 = 已禁用警告。  
  
 1 = 已启用警告。  
  
> [!NOTE]  
>  始终启用保持期。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的每个性能指标和保持期设置阈值。 下表显示该示例中使用的值。  
  
|*alert_id*|性能指标|警告阈值|警告是否已启用？|  
|-----------------|------------------------|-----------------------|-----------------------------|  
|1|最早的未发送事务|30 分钟|是|  
|2|未发送日志|10000 KB|是|  
|3|未还原日志|10000 KB|是|  
|4|镜像提交开销|1,000 毫秒|否|  
|5|保留期|8 小时|是|  
  
```  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 1, 30, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 2, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 3, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 4, 1000, 0 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 5, 8, 1 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorhelpalert &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitordropalert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
  
