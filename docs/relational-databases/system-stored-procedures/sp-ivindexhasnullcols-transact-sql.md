---
description: sp_ivindexhasnullcols (Transact-SQL)
title: sp_ivindexhasnullcols (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_ivindexhasnullcols
- sp_ivindexhasnullcols_TSQL
helpviewer_keywords:
- sp_ivindexhasnullcols
ms.assetid: ed2cde63-37e1-43cf-b6ba-3b6114a0f797
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9fc065d30bc8fac8195a3a389de1b316645bcd57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183404"
---
# <a name="sp_ivindexhasnullcols-transact-sql"></a>sp_ivindexhasnullcols (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  验证索引视图的聚集索引是否唯一，而且当索引视图将要用于创建事务发布时其聚集索引不包含任何可能为 Null 的列。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_ivindexhasnullcols [ @viewname = ] 'view_name'  
        , [ @fhasnullcols= ] field_has_null_columns OUTPUT  
```  
  
## <a name="arguments"></a>参数  
`[ @viewname = ] 'view_name'` 要验证的视图的名称。 *view_name* **sysname**，无默认值。  
  
`[ @fhasnullcols = ] field_has_null_columns OUTPUT` 指示视图索引是否具有允许 NULL 值的列的标志。 *view_name* **sysname**，无默认值。 如果视图索引包含允许 NULL 的列，则返回值 **1** 。 如果视图不包含允许 NULL 的列，则返回值 **0** 。  
  
> [!NOTE]  
>  如果存储过程本身返回的返回代码为 **1**，则表示存储过程执行失败，则此值为 **0** ，应忽略此值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_ivindexhasnullcols** 由事务复制使用。  
  
 默认情况下，发布中的索引视图项目创建为订阅服务器上的表。 但是，当索引列允许 NULL 值时，索引视图创建为订阅服务器上的索引视图而不是表。 通过执行此存储过程，可以警告用户当前索引视图中是否存在此问题。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_ivindexhasnullcols**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
