---
description: sp_check_subset_filter (Transact-SQL)
title: sp_check_subset_filter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_check_TSQL
- sp_check_subset_filter
- filter
- subset
- subset_TSQL
- sp_check
- sp_check_subset_filter_TSQL
helpviewer_keywords:
- sp_check_subset_filter
ms.assetid: 525cfcfc-f317-478d-ba84-72e62285f160
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6d311670fca8b5514057bd122980e8647566240e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174536"
---
# <a name="sp_check_subset_filter-transact-sql"></a>sp_check_subset_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  用于对任何表检查筛选子句，以确定筛选子句对该表是否有效。 此存储过程返回所提供的筛选器的相关信息，包括筛选器是否适合用于预计算分区。 此存储过程在发布服务器上包含该发布的数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_check_subset_filter [ @filtered_table = ] 'filtered_table'  
        , [ @subset_filterclause = ] 'subset_filterclause'  
    [ , [ @has_dynamic_filters = ] has_dynamic_filters OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @filtered_table = ] 'filtered_table'` 筛选的表的名称。 *filtered_table* 为 **nvarchar (400)**，无默认值。  
  
`[ @subset_filterclause = ] 'subset_filterclause'` 要测试的筛选子句。 *subset_filterclause* 为 **nvarchar (1000)**，无默认值。  
  
`[ @has_dynamic_filters = ] has_dynamic_filters` 如果筛选子句是参数化行筛选器，则为。 *has_dynamic_filters* 为 **bit**，默认值为 NULL，并且是输出参数。 如果筛选子句是参数化行筛选器，则返回值 **1** 。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|如果发布符合使用预计算分区的条件，则为;其中， **1** 表示可以使用预计算分区， **0** 表示不能使用。|  
|**has_dynamic_filters**|**bit**|如果提供的筛选子句包含至少一个参数化行筛选器，则为; 否则为。其中， **1** 表示使用参数化行筛选器， **0** 表示不使用此类函数。|  
|**dynamic_filters_function_list**|**nvarchar (500)**|筛选子句中动态筛选项目的函数的列表，其中，以分号分隔每个函数。|  
|**uses_host_name**|**bit**|如果在 filter 子句中使用 [HOST_NAME ( # B1](../../t-sql/functions/host-name-transact-sql.md) 函数，其中 **1** 表示存在此函数。|  
|**uses_suser_sname**|**bit**|如果在 filter 子句中使用 [SUSER_SNAME ( # B1](../../t-sql/functions/suser-sname-transact-sql.md) 函数，其中 **1** 表示存在此函数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_check_subset_filter** 用于合并复制。  
  
 即使未发布表，也可以对任何表执行 **sp_check_subset_filter** 。 在定义筛选项目之前，此存储过程可以用来验证筛选子句。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_check_subset_filter**。  
  
## <a name="see-also"></a>另请参阅  
 [使用预计算分区优化参数化筛选器的性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)  
  
  
