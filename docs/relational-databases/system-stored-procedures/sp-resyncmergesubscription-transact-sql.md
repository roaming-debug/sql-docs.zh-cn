---
description: sp_resyncmergesubscription (Transact-SQL)
title: sp_resyncmergesubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resyncmergesubscription_TSQL
- sp_resyncmergesubscription
helpviewer_keywords:
- sp_resyncmergesubscription
ms.assetid: e04d464a-60ab-4b39-a710-c066025708e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530c93cbb99db63c7ced6e454ea78b4f6a0f1ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183080"
---
# <a name="sp_resyncmergesubscription-transact-sql"></a>sp_resyncmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将合并订阅重新同步到指定的已知验证状态。 这使您得以将订阅数据库强制集中或同步到特定的时点（如上次成功验证的时间）或指定的日期。 使用此方法重新同步订阅时，不重新应用快照。 此存储过程不用于快照复制订阅或事务复制订阅。 此存储过程在发布服务器的发布数据库中或订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_resyncmergesubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
        , [ @publication = ] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @resync_type = ] resync_type ]  
    [ , [ @resync_date_str = ] resync_date_string ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。 如果存储过程在发布服务器上运行，则 NULL 值有效。 如果在订阅服务器上运行该存储过程，则必须指定发布服务器。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 NULL。 如果存储过程在发布服务器的发布数据库中运行，则 NULL 值有效。 如果在订阅服务器上运行该存储过程，则必须指定发布服务器。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。 如果存储过程在订阅服务器上运行，则 NULL 值有效。 如果在发布服务器上运行该存储过程，则必须指定订阅服务器。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscription_db* 的默认值为 **sysname**，默认值为 NULL。 如果存储过程在订阅服务器的订阅数据库中运行，则 NULL 值有效。 如果在发布服务器上运行该存储过程，则必须指定订阅服务器。  
  
`[ @resync_type = ] resync_type` 定义重新同步的开始时间。 *resync_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|同步从初始快照后开始。 这是占用资源最多的选项，因为自初始快照后的所有更改都重新应用于订阅服务器。|  
|**1**|同步从上次成功验证后开始。 自上次成功验证后发生的所有新的或未完成的生成都重新应用于订阅服务器。|  
|**2**|同步从 *resync_date_str* 中给定的日期开始。 该日期后发生的所有新的或未完成的生成都重新应用于订阅服务器。|  
  
`[ @resync_date_str = ] resync_date_string` 定义重新同步的开始日期。 *resync_date_string* 为 **nvarchar (30)**，默认值为 NULL。 当 *resync_type* 为值 **2** 时，使用此参数。 给定的日期将转换为其等效的日期 **时间** 值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_resyncmergesubscription** 用于合并复制。  
  
 *Resync_type* 参数的值为 **0** ，这将重新应用自初始快照后的所有更改，这可能会占用大量资源，但可能比完全重新初始化要少得多。 例如，如果初始快照在一个月前传递，则该值将使得重新应用上个月的数据。 如果初始快照包含 1 GB 的数据，但是在上个月开始的更改量包含 2 MB 的已更改数据，则重新应用数据与重新应用全部 1 GB 快照相比更有效。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_resyncmergesubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
