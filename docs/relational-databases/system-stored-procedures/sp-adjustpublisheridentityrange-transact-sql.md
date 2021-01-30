---
description: sp_adjustpublisheridentityrange (Transact-SQL)
title: sp_adjustpublisheridentityrange (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adjustpublisheridentityrange_TSQL
- sp_adjustpublisheridentityrange
helpviewer_keywords:
- sp_adjustpublisheridentityrange
ms.assetid: 64f111fd-fb7d-4459-93f7-65f0f8dd7efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5855ffeceff68c10e0eac2089a95d2b0184cd327
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198346"
---
# <a name="sp_adjustpublisheridentityrange-transact-sql"></a>sp_adjustpublisheridentityrange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  调整发布上的标识范围，并根据发布上的阈值重新分配新的范围。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adjustpublisheridentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @table_name = ] 'table_name' ]  
    [ , [ @table_owner= ] 'table_owner' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 在其中重新分配新标识范围的发布的名称。 *发布* 为 **sysname**，默认值为 NULL。  
  
`[ @table_name = ] 'table_name'` 重新分配新标识范围的表的名称。 *table_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @table_owner = ] 'table_owner'` 发布服务器上的表的所有者。 *table_owner* 的默认值为 **sysname**，默认值为 NULL。 如果未指定 *table_owner* ，则使用当前用户的名称。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_adjustpublisheridentityrange** 在所有类型的复制中使用。  
  
 对于已启用自动标识范围的发布，分发代理或合并代理负责基于发布的阈值自动调整发布的标识范围。 但是，如果出于某种原因，分发代理或合并代理尚未运行一段时间，并且标识范围资源已过度消耗到阈值，则用户可以调用 **sp_adjustpublisheridentityrange** 为发布服务器分配新的值范围。  
  
 执行 **sp_adjustpublisheridentityrange** 时，必须指定 *发布* 或 *table_name* 。 如果同时指定了这两个参数或二者都未指定，则将返回错误。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_adjustpublisheridentityrange**。  
  
## <a name="see-also"></a>另请参阅  
 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
