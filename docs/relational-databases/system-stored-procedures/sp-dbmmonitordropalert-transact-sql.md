---
description: sp_dbmmonitordropalert (Transact-SQL)
title: sp_dbmmonitordropalert (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitordropalert_TSQL
- sp_dbmmonitordropalert
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], monitoring
- sp_dbmmonitordropalert
ms.assetid: fe4a134b-25bf-464e-a5c4-358de215b65a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9f872e37c0a9ae9d869b9483e5818db3b7d238a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212387"
---
# <a name="sp_dbmmonitordropalert-transact-sql"></a>sp_dbmmonitordropalert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  通过将阈值设置为 NULL 来删除指定性能指标的警告。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbmmonitordropalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>参数  
 *database_name*  
 指定将要删除其指定警告阈值的数据库。  
  
 *alert_id*  
 整数值，用于标识要删除的警告。 如果省略此参数，将删除此数据库的所有警告。 若要删除特定性能指标的警告，请指定下列值之一：  
  
|值|性能指标|警告阈值|  
|-----------|------------------------|-----------------------|  
|1|最早的未发送事务|指定在主体服务器实例上生成警告之前，发送队列中可以累积的事务的分钟数。 该警告有助于度量数据丢失的可能性（以时间计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|2|未发送日志|指定未发送日志达到多少 KB 后，在主体服务器实例上生成一个警告。 该警告有助于度量数据丢失的可能性（以 KB 计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|3|未还原日志|指定未还原日志达到多少 KB 后，会在镜像服务器实例上生成一个警告。 此警告可以帮助度量故障转移时间。 “故障转移时间 ”主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间。|  
|4|镜像提交开销|指定在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。|  
|5|保留期|用于控制数据库镜像状态表中的行保留多长时间的元数据。|  
  
> [!NOTE]  
>  此过程会删除警告阈值，无论它们是使用 **sp_dbmmonitorchangealert** 还是数据库镜像监视器指定。  
  
 有关与警告相对应的事件 Id 的详细信息，请参阅 [对镜像性能指标使用警告阈值和警报 &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将删除 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的保持期设置。  
  
```  
EXEC sp_dbmmonitordropalert AdventureWorks2012, 5;  
```  
  
 以下示例将删除 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的所有警告阈值和保持期。  
  
```  
EXEC sp_dbmmonitordropalert AdventureWorks2012 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
  
