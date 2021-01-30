---
description: sp_vupgrade_mergeobjects (Transact-SQL)
title: sp_vupgrade_mergeobjects (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_vupgrade_mergeobjects
- sp_vupgrade_mergeobjects_TSQL
helpviewer_keywords:
- sp_vupgrade_mergeobjects
ms.assetid: 73257c2e-cc4c-48e7-9d66-7ef045bdd4f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26e7405647b80d26c760bf88caf5e6aa786346ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201836"
---
# <a name="sp_vupgrade_mergeobjects-transact-sql"></a>sp_vupgrade_mergeobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  重新生成特定于项目的触发器、存储过程和视图，用于跟踪和应用合并复制的数据更改。 在以下情况下执行此过程：  
  
-   如果意外删除复制所需的对象。  
  
-   如果应用的更新（如修补程序）需对一个或多个复制对象进行修改。 应用此更新后对每个节点执行此过程。  
  
 执行此存储过程不需要重新初始化订阅。 如果对新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装 service pack 或更新，则不需要此过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_vupgrade_mergeobjects [ [@login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @security_mode = ] security_mode ]  
```  
  
## <a name="arguments"></a>参数  
`[ @login = ] 'login'` 在分发数据库中创建新的系统对象时要使用的系统管理员登录名。 login 的数据类型为 sysname，默认值为 NULL。 如果 *security_mode* 设置为 **1**（这是 Windows 身份验证），则不需要此参数。  
  
`[ @password = ] 'password'` 在分发数据库中创建新的系统对象时要使用的系统管理员密码。 *密码* 为 **sysname**，默认值为 **""** (空字符串) 。 如果 *security_mode* 设置为 **1**（这是 Windows 身份验证），则不需要此参数。  
  
`[ @security_mode = ] 'security_mode'` 在分发数据库中创建新的系统对象时要使用的登录安全模式。 *security_mode* 为 **bit** ，默认值为 **1**。 如果为 **0**， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则使用身份验证。 如果为 **1**，则将使用 Windows 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_vupgrade_mergeobjects** 仅用于合并复制。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [升级复制的数据库](../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
