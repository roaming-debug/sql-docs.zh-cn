---
description: sp_changereplicationserverpasswords (Transact-SQL)
title: sp_changereplicationserverpasswords (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changereplicationserverpasswords_TSQL
- sp_changereplicationserverpasswords
helpviewer_keywords:
- sp_changereplicationserverpasswords
ms.assetid: 9333da96-3a1c-4adb-9a74-5dac9ce596df
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e301e37f252a81cf4e7c2634f291bc61bc2979c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207046"
---
# <a name="sp_changereplicationserverpasswords-transact-sql"></a>sp_changereplicationserverpasswords (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制代理连接到复制拓扑中的服务器时所用的 Windows 帐户或登录名的存储密码。 一般情况下，必须更改服务器上运行的每个单独代理的密码，即使它们使用相同的登录名或帐户时也是如此。 使用此存储过程，可以更改服务器上运行的所有复制代理所用的给定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 Windows 帐户的所有实例的密码。 此存储过程在复制拓扑中的任意服务器上对主数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changereplicationserverpasswords [ @login_type = ] login_type  
        , [ @login = ] 'login'   
        , [ @password = ] 'password'  
    [ , [ @server = ] 'server' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @login_type = ] login_type` 提供的凭据的身份验证类型。 *login_type* 为 **tinyint**，无默认值。  
  
 **1** = Windows 集成身份验证  
  
 **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证  
  
`[ @login = ] 'login'` 要更改的 Windows 帐户或登录名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *登录名* 为 **nvarchar (257)**，无默认值  
  
`[ @password = ] 'password'` 要为指定的 *登录名* 存储的新密码。 *password* 的值为 **sysname**，无默认值。  
  
> [!NOTE]  
>  更改复制密码后，必须停止并重新启动使用该密码的每个代理，这样代理的更改才能生效。  
  
`[ @server = ] 'server'` 正在为其更改存储密码的服务器连接。 *服务器* **sysname**，可以是以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**发行人**|所有指向分发服务器的代理连接。|  
|**publisher**|所有指向发布服务器的代理连接。|  
|**订阅服务器**|所有指向订阅服务器的代理连接。|  
|**%** (默认值) |指向复制拓扑中所有服务器的代理连接。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changereplicationserverpasswords** 用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_changereplicationserverpasswords** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改复制安全设置](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
