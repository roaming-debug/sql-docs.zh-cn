---
description: sp_showpendingchanges (Transact-SQL)
title: sp_showpendingchanges (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showpendingchanges
- sp_showpendingchanges_TSQL
helpviewer_keywords:
- sp_showpendingchanges
ms.assetid: 8013a792-639d-4550-b262-e65d30f9d291
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac2c44e836a56c6fa503b6144e99761294ab473a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207304"
---
# <a name="sp_showpendingchanges-transact-sql"></a>sp_showpendingchanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回一个结果集，其中显示了等待复制的更改。 此存储过程在发布服务器上的发布数据库中执行，或者在订阅服务器上的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  此过程提供近似的更改数以及这些更改所涉及到的行。 例如，此过程将从发布服务器或订阅服务器上检索信息，但是不会在这两个服务器上同时进行检索。 存储在其他节点上的信息产生的要同步的更改数可能比该过程估计的更改数小。  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_showpendingchanges [ [ @destination_server = ] 'destination_server' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article']  
    [ , [ @show_rows = ] show_rows]  
```  
  
## <a name="arguments"></a>参数  
`[ @destination_server = ] 'destination_server'` 应用复制更改的服务器的名称。 *destination_server* 的值为 **sysname**，默认值为 NULL。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 NULL。 指定 *发布* 时，结果仅限于指定的发布。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 的值为 **sysname**，默认值为 NULL。 指定 *项目* 后，结果仅限于指定项目。  
  
`[ @show_rows = ] 'show_rows'` 指定结果集是否包含有关挂起的更改的更多特定信息，默认值为 **0**。 如果指定的值为 **1** ，则结果集将包含 is_delete 和 rowguid 的列。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|destination_server|**sysname**|接收更改复制的服务器的名称。|  
|pub_name|**sysname**|发布的名称。|  
|destination_db_name|**sysname**|接收更改复制的数据库的名称。|  
|is_dest_subscriber|**bit**|指示要将更改复制到订阅服务器。 值为 **1** 表示将更改复制到订阅服务器。 **0** 表示将更改复制到发布服务器。|  
|article_name|**sysname**|产生更改的表的项目名称。|  
|pending_deletes|**int**|等待复制的删除数。|  
|pending_ins_and_upd|**int**|等待复制的插入数和更新数。|  
|is_delete|**bit**|指示挂起更改是否为删除操作。 如果值为 **1** ，则表示更改为删除。 需要的值为 **1** @show_rows 。|  
|rowguid|**uniqueidentifier**|标识已更改的行的 GUID。 需要的值为 **1** @show_rows 。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_showpendingchanges 用于合并复制。  
  
 sp_showpendingchanges 在排除合并复制故障时使用。  
  
 sp_showpendingchanges 的结果不包括第 0 代中的行。  
  
 如果为 *项目* 指定的项目不属于为发布指定的发布 *，* 则会为 pending_deletes 和 pending_ins_and_upd 返回计数0。  
  
## <a name="permissions"></a>权限  
 只有 sysadmin 固定服务器角色的成员或 db_owner 固定数据库角色的成员才能执行 sp_showpendingchanges。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
