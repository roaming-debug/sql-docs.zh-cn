---
description: sp_addapprole (Transact-SQL)
title: sp_addapprole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addapprole_TSQL
- sp_addapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addapprole
ms.assetid: 24200295-9a54-4cab-9922-fb2e88632721
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5a48cfb80bab8e9a754cbcb8186de8cb2955f096
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182853"
---
# <a name="sp_addapprole-transact-sql"></a>sp_addapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  向当前数据库中添加应用程序角色。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [CREATE APPLICATION ROLE](../../t-sql/statements/create-application-role-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addapprole [ @rolename = ] 'role' , [ @password = ] 'password'  
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 新应用程序角色的名称。 *role* 的值为 **sysname**，无默认值。 *role* 必须是有效的标识符，并且不能已存在于当前数据库中。  
  
 应用程序角色名称可以包含 1 到 128 个字符，包括字母、符号及数字。 角色名称不能包含反斜杠 () 也不能为 NULL，也不能为 \\ 空字符串 ( "" ) 。  
  
`[ @password = ] 'password'` 激活应用程序角色所需的密码。 *password* 的值为 **sysname**，无默认值。 *密码* 不能为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的更早版本中，用户（和角色）与架构并非完全不同。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，架构与角色已完全不同。 此新体系结构反映在 CREATE APPLICATION ROLE 的行为中。 此语句将取代 **sp_addapprole**。  
  
 为了保持与早期版本的向后兼容性 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， **sp_addapprole** 会执行以下操作：  
  
-   如果尚未存在与应用程序角色同名的架构，则创建这样的架构。 新的架构将由应用程序角色拥有，并且它将是该应用程序角色的默认架构。  
  
-   如果与应用程序角色同名的架构已经存在，则该过程将失败。  
  
-   **Sp_addapprole** 未检查密码复杂性。 但 CREATE APPLICATION ROLE 会检查密码复杂性。  
  
 参数 *password* 作为单向哈希进行存储。  
  
 不能在用户定义的事务内执行 **sp_addapprole** 存储过程。  
  
> [!IMPORTANT]  
>  **SqlClient** 不支持 Microsoft ODBC **加密** 选项。 如果您可以，请在运行时提示用户输入应用程序角色凭据。 不要将凭据存储在一个文件中。 如果必须使凭据持久化，请使用 CryptoAPI 函数将它们加密。  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY APPLICATION ROLE 权限。 如果尚未存在具有与新角色相同名称和所有者的架构，则还需要拥有对该数据库的 CREATE SCHEMA 权限。  
  
## <a name="examples"></a>示例  
 下面的示例将新的应用程序角色 `SalesApp` （具有密码）添加 `x97898jLJfcooFUYLKm387gf3` 到当前数据库。  
  
```  
EXEC sp_addapprole 'SalesApp', 'x97898jLJfcooFUYLKm387gf3' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
