---
description: sp_helpmergepartition (Transact-SQL)
title: sp_helpmergepartition (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepartition
- sp_helpmergepartition_TSQL
helpviewer_keywords:
- sp_helpmergepartition
ms.assetid: 184188cc-f519-445d-97ce-aae38f1eb550
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 06994bb4f0e606f0d67e797603e6f4a23899076b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185733"
---
# <a name="sp_helpmergepartition-transact-sql"></a>sp_helpmergepartition (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回指定合并发布的分区信息。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergepartition [ @publication= ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]  
    [ , [ @host_name = ] 'host_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @suser_sname = ] 'suser_sname'` 用于定义分区的 SUSER_SNAME 值。 *suser_sname* 的值为 **sysname**，默认值为 NULL。 提供此参数是为了将结果集限制在仅将 SUSER_SNAME 解析为提供的值的分区中。  
  
> [!NOTE]  
>  提供 *suser_sname* 时， *HOST_NAME* 必须为 NULL  
  
`[ @host_name = ] 'host_name'` 用于定义分区的 HOST_NAME 值。 *host_name* 的值为 **sysname**，默认值为 NULL。 提供此参数是为了将结果集限制在仅将 HOST_NAME 解析为提供的值的分区中。  
  
> [!NOTE]  
>  提供 *suser_sname* 时， *HOST_NAME* 必须为 NULL  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**partition**|**int**|标识订阅服务器分区。|  
|**host_name**|**sysname**|为订阅创建分区时使用的值，该订阅按订阅服务器上 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值进行筛选。|  
|**suser_sname**|**sysname**|为订阅创建分区时使用的值，该订阅按订阅服务器上 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 函数的值进行筛选。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|订阅服务器分区的已筛选数据快照的位置。|  
|**date_refreshed**|**datetime**|最近运行快照作业为分区生成已筛选数据快照的日期。|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|标识为分区创建已筛选数据快照的作业。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergepartition** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员和 **db_owner** 固定数据库角色的成员才能执行 **sp_helpmergepartition**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addmergepartition &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_dropmergepartition &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)  
  
  
