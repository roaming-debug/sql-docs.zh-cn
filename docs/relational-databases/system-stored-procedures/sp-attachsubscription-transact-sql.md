---
description: sp_attachsubscription (Transact-SQL)
title: sp_attachsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_attachsubscription
- sp_attachsubscription_TSQL
helpviewer_keywords:
- sp_attachsubscription
ms.assetid: b9bbda36-a46a-4327-a01e-9cd632e4791b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a7357d6a37472d9d530dfcf3ecfe3c9bddfffd58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206638"
---
# <a name="sp_attachsubscription-transact-sql"></a>sp_attachsubscription (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  将现有的订阅数据库附加到任何订阅服务器。 此存储过程在新订阅服务器的主数据库中执行。  
  
> [!IMPORTANT]  
>  不推荐使用此功能，该功能将在未来版本中删除。 在新的开发工作中不要使用此功能。 对于使用参数化筛选器分区的合并发布，建议您使用分区快照的新功能，这些功能可简化大量订阅的初始化。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。 对于未分区的发布，可以使用备份来初始化订阅。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_attachsubscription [ @dbname = ] 'dbname'  
        , [ @filename = ] 'filename'  
    [ , [ @subscriber_security_mode = ] 'subscriber_security_mode' ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @db_master_key_password = ] 'db_master_key_password' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 按名称指定目标订阅数据库的字符串。 *dbname* 的值为 **sysname**，无默认值。  
  
`[ @filename = ] 'filename'` 主 MDF (**主** 数据文件) 的名称和物理位置。 *filename* 为 **nvarchar (260)**，无默认值。  
  
`[ @subscriber_security_mode = ] 'subscriber_security_mode'` 同步时连接到订阅服务器时要使用的订阅服务器的安全模式。 *subscriber_security_mode* 的值为 **int**，默认值为 NULL。  
  
> [!NOTE]  
>  必须使用 Windows 身份验证。 如果 *subscriber_security_mode* 不是 **1** (Windows 身份验证) ，则将返回错误。  
  
`[ @subscriber_login = ] 'subscriber_login'` 同步时连接到订阅服务器时使用的订阅服务器登录名。 *subscriber_login* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  已不推荐使用此参数，保留它只是为了让脚本能够向后兼容。 如果 *subscriber_security_mode* 不是 **1** 并且指定了 *subscriber_login* ，将返回错误。  
  
`[ @subscriber_password = ] 'subscriber_password'` 订阅服务器密码。 *subscriber_password* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  已不推荐使用此参数，保留它只是为了让脚本能够向后兼容。 如果 *subscriber_security_mode* 不是 **1** 并且指定了 *subscriber_password* ，将返回错误。  
  
`[ @distributor_security_mode = ] distributor_security_mode` 同步时连接到分发服务器时使用的安全模式。 *distributor_security_mode* 的值为 **int**，默认值为 **0**。 **0** 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 **1** 指定 Windows 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` 同步时连接到分发服务器时使用的分发服务器登录名。 如果 *distributor_security_mode* 设置为 **0**，则 *distributor_login* 是必需的。 *distributor_login* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @distributor_password = ] 'distributor_password'` 分发服务器密码。 如果 *distributor_security_mode* 设置为 **0**，则 *distributor_password* 是必需的。 *distributor_password* 的默认值为 **sysname**，默认值为 NULL。 *Distributor_password* 的值必须少于 120 Unicode 字符。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 同步时连接到发布服务器时使用的安全模式。 *publisher_security_mode* 的值为 **int**，默认值为 **1**。 如果为 **0**，则指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 如果为 **1**，则指定 Windows 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` 同步时连接到发布服务器时使用的登录名。 *publisher_login* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password* 的默认值为 **sysname**，默认值为 NULL。 *Publisher_password* 的值必须少于 120 Unicode 字符。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @job_login = ] 'job_login'` 用于运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，无默认值。 此 Windows 帐户总是用于与分发服务器建立代理连接。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* **sysname**，无默认值。 *Job_password* 的值必须少于 120 Unicode 字符。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @db_master_key_password = ] 'db_master_key_password'` 用户定义数据库主密钥的密码。 *db_master_key_password* 为 **nvarchar (524)**，默认值为 NULL。 如果未指定 *db_master_key_password* ，则会删除并重新创建现有的数据库主密钥。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_attachsubscription** 用于快照复制、事务复制和合并复制。  
  
 如果发布保持期已满，则不能将订阅附加到发布中。 如果指定一个保持期已满的订阅，则附加该订阅或先对其进行同步时将发生错误。 将忽略发布保持期为 **0** (永不过期) 的发布。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_attachsubscription** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
