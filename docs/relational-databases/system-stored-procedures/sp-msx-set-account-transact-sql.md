---
description: sp_msx_set_account (Transact-SQL)
title: sp_msx_set_account (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_set_account
- sp_msx_set_account_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_set_account
ms.assetid: 314ec720-3a37-48f7-bb6b-8d5b894bf843
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29a92920e81f65beabdc0837b85d19484d2e35f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192009"
---
# <a name="sp_msx_set_account-transact-sql"></a>sp_msx_set_account (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置目标服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理主服务器帐户名和密码。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_msx_set_account [ @credential_name = ] 'credential_name'  | [ @credential_id = ] credential_id  
```  
  
## <a name="arguments"></a>参数  
`[ @credential_name = ] 'credential_name'` 用于登录到主服务器的凭据的名称。 所提供的名称必须是现有凭据的名称。 必须指定 *credential_name* 或 *credential_id* 。  
  
`[ @credential_id = ] credential_id` 用于登录到主服务器的凭据的标识符。 标识符必须是现有凭据的标识符。 必须指定 *credential_name* 或 *credential_id* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用凭据存储目标服务器用于登录到主服务器的用户名和密码信息。 该过程将设置此目标服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用于登录到主服务器的凭据。  
  
 所指定的凭据必须是现有凭据。 有关创建凭据的详细信息，请参阅 [CREATE credential &#40;transact-sql&#41;](../../t-sql/statements/create-credential-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 **Sp_msx_set_account** 默认授予 **sysadmin** 固定服务器角色成员的权限。  
  
## <a name="examples"></a>示例  
 以下是设置该服务器，以使用凭据 `MsxAccount` 登录到主服务器的示例。  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sp_msx_set_account @credential_name = MsxAccount ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_msx_get_account &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-msx-get-account-transact-sql.md)  
  
  
