---
description: sp_changeqreader_agent (Transact-SQL)
title: sp_changeqreader_agent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changeqreader_agent_TSQL
- sp_changeqreader_agent
helpviewer_keywords:
- sp_changeqreader_agent
ms.assetid: d3fe79c5-31ef-4565-bf38-b476b5fb16f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83be24af54593c4630284e07cf9464899d2b5cfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207056"
---
# <a name="sp_changeqreader_agent-transact-sql"></a>sp_changeqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改队列读取器代理的安全属性。 此存储过程在分发服务器上针对分发数据库执行，或在发布服务器上针对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changeqreader_agent [ [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>参数  
`[ @job_login = ] 'job_login'` 用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 运行代理的 Windows 帐户的登录名。 *job_login* 为 **nvarchar (257)**，默认值为 NULL。  
  
`[ @job_password = ] 'job_password'` 运行代理所用的 Windows 帐户的密码。 *job_password* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @frompublisher = ] frompublisher` 如果正在发布服务器上执行该过程，则为。 *frompublisher* 的值为 bit，默认值为 **0**。 值为 **1** 表示在发布服务器上对发布数据库执行此过程。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changeqreader_agent** 用于事务复制。  
  
 **sp_changeqreader_agent** 用于更改运行队列读取器代理所用的 Windows 帐户。 可以更改现有 Windows 登录名的密码，或提供新的 Windows 登录名和密码。  
  
 更改代理登录名或密码之后，必须先停止并重新启动代理，然后更改才能生效。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_changeqreader_agent** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改复制安全设置](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_addqreader_agent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
  
