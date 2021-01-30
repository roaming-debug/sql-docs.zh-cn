---
description: sp_adddistpublisher (Transact-SQL)
title: sp_adddistpublisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 3ad2edff83e6600ab77e422d2d18516ecd8a2064
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182821"
---
# <a name="sp_adddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  配置发布服务器以使用指定的分发数据库。 此存储过程在分发服务器上的任何数据库中执行。 请注意，在使用此存储过程之前，必须先运行 [sp_adddistributor &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) 和 [&#40;sp_adddistributiondb ](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) 的存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @storage_connection_string= ] 'storage_connection_string']
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器名称。 *发布服务器* 的 **sysname**，无默认值。  

> [!NOTE]
> 服务器名称可指定为 `<Hostname>,<PortNumber>` 。 如果在 Linux 或 Windows 上使用自定义端口部署 SQL Server，并且禁用了 browser 服务，则可能需要指定连接的端口号。
  
`[ @distribution_db = ] 'distribution_db'` 分发数据库的名称。 *distributor_db* **sysname**，无默认值。 复制代理使用该参数连接到发布服务器。  
  
`[ @security_mode = ] security_mode` 实现的安全模式。 此参数仅供复制代理用于连接到排队更新订阅的发布服务器或非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *security_mode* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|分发服务器中的复制代理使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接到发布服务器。|  
|**1** （默认值）|分发服务器中的复制代理使用 Windows 身份验证连接到发布服务器。|  
  
`[ @login = ] 'login'` 登录名。 如果 *security_mode* 为 **0**，则此参数是必需的。 login 的数据类型为 sysname，默认值为 NULL。 复制代理使用该参数连接到发布服务器。  
  
`[ @password = ] 'password']` 密码。 *password* 的值为 **sysname**，默认值为 NULL。 复制代理使用该参数连接到发布服务器。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。  
  
`[ @working_directory = ] 'working_directory'` 用于存储发布的数据和架构文件的工作目录的名称。 例如， *working_directory* 为 **nvarchar (255)**，默认为 ReplData 文件夹 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData` 。 名称应按 UNC 格式指定。  

 对于 Azure SQL 数据库，请使用 `\\<storage_account>.file.core.windows.net\<share>` 。

`[ @storage_connection_string = ] 'storage_connection_string'` 对于 SQL 数据库是必需的。 使用 Azure 门户中的 "存储 > 设置" 下的访问密钥。

 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]

`[ @trusted = ] 'trusted'` 此参数已弃用，提供此参数只是为了向后兼容。 *trusted* **(5)**，并将其设置为任何值，但 **false** 将导致错误。  
  
`[ @encrypted_password = ] encrypted_password` 不再支持设置 *encrypted_password* 。 尝试将此 **位** 参数设置为 **1** 将导致错误。  
  
`[ @thirdparty_flag = ] thirdparty_flag` 发布服务器是时为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *thirdparty_flag* 是 **bit**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0** （默认值）|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据.|  
|**1**|非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
  
`[ @publisher_type = ] 'publisher_type'` 当发布服务器不是时指定发布服务器类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *publisher_type* sysname，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> （默认值）|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。|  
|**联手**|指定标准的 Oracle 发布服务器。|  
|**ORACLE GATEWAY**|指定 Oracle 网关发布服务器。|  
  
 有关 Oracle 发布服务器与 Oracle 网关发布服务器之间的差异的详细信息，请参阅 [配置 Oracle 发布服务器](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_adddistpublisher** 用于快照复制、事务复制和合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_adddistpublisher** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [配置分发](../../relational-databases/replication/configure-distribution.md)  
  
  
