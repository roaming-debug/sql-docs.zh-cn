---
description: sp_check_dynamic_filters (Transact-SQL)
title: sp_check_dynamic_filters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- dynamic_filters_TSQL
- sp_check_TSQL
- check
- sp_check_dynamic filter
- check_TSQL
- filters_TSQL
- check_dynamic_filters_TSQL
- dynamic filters
- filters
- check dynamic filters
- sp_check_dynamic filter_TSQL
- sp_check_for_sync_trigger_TSQL
- sp_check
helpviewer_keywords:
- sp_check_dynamic_filters
ms.assetid: dd7760db-a3a5-460f-bd97-b8d436015e19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f458f3dcf629ee6f77b8237edb0a9defa516b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158961"
---
# <a name="sp_check_dynamic_filters-transact-sql"></a>sp_check_dynamic_filters (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  显示有关发布的参数化行筛选器属性的信息，特别是用于为发布生成已筛选数据分区的函数以及关于发布是否有资格使用预计算分区的信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_check_dynamic_filters [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|如果发布符合使用预计算分区的条件，则为;其中， **1** 表示可以使用预计算分区， **0** 表示不能使用。|  
|**has_dynamic_filters**|**bit**|如果在发布中至少定义了一个参数化行筛选器，则为; 否则为。其中， **1** 表示存在一个或多个参数化行筛选器， **0** 表示不存在动态筛选器。|  
|**dynamic_filters_function_list**|**nvarchar (500)**|用于对发布中的项目进行筛选的函数列表，各个函数由分号分隔。|  
|**validate_subscriber_info**|**nvarchar (500)**|用于对发布中的项目进行筛选的函数列表，各个函数由加号 (+) 分隔。|  
|**uses_host_name**|**bit**|如果在参数化行筛选器中使用 [HOST_NAME ( # B1](../../t-sql/functions/host-name-transact-sql.md) 函数，其中 **1** 表示此函数用于动态筛选。|  
|**uses_suser_sname**|**bit**|如果在参数化行筛选器中使用 [SUSER_SNAME ( # B1](../../t-sql/functions/suser-sname-transact-sql.md) 函数，其中 **1** 表示此函数用于动态筛选。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_check_dynamic_filters** 用于合并复制。  
  
 如果已将发布定义为使用预计算分区， **sp_check_dynamic_filters** 将检查是否存在预计算分区限制的任何冲突。 如果发现任何这类情况，则将返回错误。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
 如果已将发布定义为具有参数化行筛选器，但未找到参数化行筛选器，则将返回错误。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_check_dynamic_filters**。  
  
## <a name="see-also"></a>另请参阅  
 [使用参数化筛选器为合并发布管理分区](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [sp_check_join_filter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-check-join-filter-transact-sql.md)   
 [sp_check_subset_filter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-check-subset-filter-transact-sql.md)  
  
  
