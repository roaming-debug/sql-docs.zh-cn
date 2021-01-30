---
title: 'sp_validate_replica_hosts_as_publishers (T-sql) '
description: 描述允许验证所有辅助副本的 sp_validate_replica_hosts_as_publishers 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validate_replica_hosts_as_publishers_TSQL
- sp_validate_replica_hosts_as_publishers
helpviewer_keywords:
- sp_validate_replica_hosts_as_publishers
ms.assetid: 45001fc9-2dbd-463c-af1d-aa8982d8c813
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f00ca4f972b7fccfa09f9c36c8e5b7fa09420fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205516"
---
# <a name="sp_validate_replica_hosts_as_publishers-transact-sql"></a>sp_validate_replica_hosts_as_publishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **sp_validate_replica_hosts_as_publishers** 是 **sp_validate_redirected_publisher** 的扩展，它允许验证所有辅助副本，而不只是当前的主副本。 **sp_validate_replicat_hosts_as_publisher** 验证整个 Always On 复制拓扑。 必须通过使用远程桌面会话在分发服务器上直接执行 **sp_validate_replica_hosts_as_publishers** ，以避免在 21892)  (的双跃点安全错误。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_validate_replica_hosts_as_publishers   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>参数  
`[ @original_publisher = ] 'original_publisher'` 最初发布数据库的实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *original_publisher* **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 要发布的数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @redirected_publisher = ] 'redirected_publisher'` 在为原始发布服务器/已发布数据库对调用 **sp_redirect_publisher** 时，为重定向的目标。 *redirected_publisher* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 如果发布服务器和发布数据库中不存在任何项， **sp_validate_redirected_publisher** 为输出参数 *\@ redirected_publisher* 返回 null。 否则，在成功和失败的情况下都将返回关联的重定向发布服务器。  
  
 如果验证成功， **sp_validate_redirected_publisher** 将返回成功指示。  
  
 如果验证失败，则会引发相应的错误。  **sp_validate_redirected_publisher** 尽力提高所有问题，而不只是遇到第一次遇到的问题。  
  
> [!NOTE]  
>  在验证不允许读取访问或要求指定读取意图的次要副本主机时，**sp_validate_replica_hosts_as_publishers** 将失败，并显示以下错误。  
>   
>  消息 21899，级别 11，状态 1，过程 **sp_hadr_verify_subscribers_at_publisher**，第 109 行  
>   
>  重定向的发布服务器“MyReplicaHostName”处的查询失败，该查询用于确定是否有原始发布服务器“MyOriginalPublisher”的订阅服务器的 sysserver 条目，出现错误“976”，错误消息“错误 976，级别 14，状态 1，消息：目标数据库‘MyPublishedDB’正参与某个可用性组，查询当前无法访问该数据库。 数据移动被挂起，或者未启用可用性副本以便用于读访问。 若要允许对该可用性组中的这一数据库和其他数据库进行只读访问，请对组中一个或多个辅助可用性副本启用只读访问权限。  有关详细信息，请参阅 **联机丛书中的** ALTER AVAILABILITY GROUP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句。  
>   
>  副本主机“MyReplicaHostName”遇到了一个或多个发布服务器验证错误。  
  
## <a name="permissions"></a>权限  
 调用方必须是 **sysadmin** 固定服务器角色的成员、分发数据库 **db_owner** 固定数据库角色的成员，或者是与发布服务器数据库相关联的已定义发布的发布访问列表的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)  
  
  
