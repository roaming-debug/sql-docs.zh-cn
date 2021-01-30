---
description: sp_unsetapprole (Transact-SQL)
title: sp_unsetapprole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unsetapprole_TSQL
- sp_unsetapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unsetapprole
ms.assetid: 4c4033d3-1a34-4dfb-835d-e3293d1a442d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f1de3a86c7e23956d9c41b13e2f1aef9c933bda3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209653"
---
# <a name="sp_unsetapprole-transact-sql"></a>sp_unsetapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  停用应用程序角色并恢复到前一个安全上下文。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_unsetapprole @cookie   
```  
  
## <a name="arguments"></a>参数  
 **\@票证**  
 指定在激活应用程序角色时创建的 Cookie。 Cookie 是 [sp_setapprole &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)创建的。 **varbinary (8000)**。  
  
> [!NOTE]  
>  **sp_setapprole** 的 cookie **OUTPUT** 参数现记载为 **varbinary(8000)** ，这是正确的最大长度。 但是，目前执行返回 **varbinary(50)** 。 应用程序应继续保留 **varbinary (8000)** 这样，如果 cookie 在将来的版本中返回大小增加，应用程序将继续正常运行。  
  
## <a name="return-code-values"></a>返回代码值  
 0 (成功) 和 1 (失败)   
  
## <a name="remarks"></a>备注  
 使用 **sp_setapprole** 激活应用程序角色后，该角色将保持活动状态，直到用户从服务器断开连接或执行 **sp_unsetapprole**。  
  
 有关应用程序角色的概述，请参阅 [应用程序角色](../../relational-databases/security/authentication-access/application-roles.md)。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 的成员身份以及在激活应用程序角色时保存的 cookie 的知识。  
  
## <a name="examples"></a>示例  
  
### <a name="activating-an-application-role-with-a-cookie-then-reverting-to-the-previous-context"></a>用 Cookie 激活应用程序角色，然后恢复到前一个上下文  
 以下示例使用密码 `Sales11` 激活 `fdsd896#gfdbfdkjgh700mM` 应用程序角色并创建一个 cookie。 该示例返回当前用户的名称，然后通过执行 **sp_unsetapprole** 恢复为原始上下文。  
  
```  
DECLARE @cookie varbinary(8000);  
EXEC sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.   
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_setapprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/drop-application-role-transact-sql.md)  
  
  
