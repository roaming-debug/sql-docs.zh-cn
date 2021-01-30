---
description: sp_check_join_filter (Transact-SQL)
title: sp_check_join_filter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- filter_TSQL
- sp_check_TSQL
- sp_check_join_filter
- sp_check_join_filter_TSQL
- join
- join_TSQL
- filter
- sp_check
helpviewer_keywords:
- sp_check_join_filter
ms.assetid: e9699d59-c8c9-45f6-a561-f7f95084a540
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 74e4114eca77cbbac1c2e2c2471fb0ee10a55ae1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206554"
---
# <a name="sp_check_join_filter-transact-sql"></a>sp_check_join_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  用于验证两个表之间的联接筛选器以确定联接筛选子句是否有效。 此存储过程还返回提供的联接筛选器的信息，其中包括联接筛选器是否可用于给定表的预计算分区。 此存储过程在发布服务器的发布中执行。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_check_join_filter [ @filtered_table = ] 'filtered_table'  
        , [@join_table = ] 'join_table'  
        , [ @join_filterclause = ] 'join_filterclause'  
```  
  
## <a name="arguments"></a>参数  
`[ @filtered_table = ] 'filtered_table'` 筛选的表的名称。 *filtered_table* 为 **nvarchar (400)**，无默认值。  
  
`[ @join_table = ] 'join_table'` 要联接到 *filtered_table* 的表的名称。 *join_table* 为 **nvarchar (400)**，无默认值。  
  
`[ @join_filterclause = ] 'join_filterclause'` 要测试的联接筛选子句。 *join_filterclause* 为 **nvarchar (1000)**，无默认值。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|如果发布有资格预计算分区，则为;其中， **1** 表示可以使用 precomupted 分区， **0** 表示不能使用。|  
|**has_dynamic_filters**|**bit**|如果提供的筛选子句包含至少一个参数化筛选函数，则为;其中， **1** 表示使用参数化筛选函数， **0** 表示不使用此类函数。|  
|**dynamic_filters_function_list**|**nvarchar (500)**|筛选子句中为项目定义参数化筛选器的函数的列表，其中每个函数用分号分隔。|  
|**uses_host_name**|**bit**|如果在 filter 子句中使用 [HOST_NAME ( # B1](../../t-sql/functions/host-name-transact-sql.md) 函数，其中 **1** 表示存在此函数。|  
|**uses_suser_sname**|**bit**|如果在 filter 子句中使用 [SUSER_SNAME ( # B1](../../t-sql/functions/suser-sname-transact-sql.md) 函数，其中 **1** 表示存在此函数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_check_join_filter** 用于合并复制。  
  
 可以对任何相关表执行 **sp_check_join_filter** ，即使它们未发布也是如此。 在定义两个项目之间的联接筛选器之前，可以使用此存储过程验证联接筛选子句。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_check_join_filter**。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
