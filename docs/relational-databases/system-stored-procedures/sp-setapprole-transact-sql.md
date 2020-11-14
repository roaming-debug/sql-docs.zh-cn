---
description: sp_setapprole (Transact-SQL)
title: sp_setapprole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/12/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 0513878f65513e57a6e26bb52d8101ba6c5d672c
ms.sourcegitcommit: 2144a22ad4380182133e87664a907fe6f06b5f95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570954"
---
# <a name="sp_setapprole-transact-sql"></a>sp_setapprole (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  激活与当前数据库中的应用程序角色关联的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  

```sql
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```

## <a name="arguments"></a>自变量

`[ @rolename = ] 'role'` 当前数据库中定义的应用程序角色的名称。 *role* 的值为 **sysname** ，无默认值。 *角色* 必须存在于当前数据库中。  
  
`[ @password = ] { encrypt N'password' }` 激活应用程序角色所需的密码。 *password* 的值为 **sysname** ，无默认值。 可使用 ODBC **加密** 函数对 *密码* 进行模糊处理。 当使用 **加密** 函数时，必须通过将 **N** 置于第一个引号之前，将密码转换为 Unicode 字符串。  
  
 使用 **SqlClient** 的连接不支持加密选项。  
  
> [!IMPORTANT]  
> **ODBC encryption** 函数不提供加密。 您不应当依赖该函数来保护通过网络传输的密码。 如果此信息将通过网络传输，请使用 TLS 或 IPSec。
  
 **@encrypt = "none"**  
 指定不使用任何模糊代码。 密码以明文形式传递到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这是默认设置。  
  
 **@encrypt= "odbc"**  
 指定在将密码发送到之前，ODBC 将使用 ODBC **encrypt** 函数来模糊处理密码 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。 这只能在使用 ODBC 客户端或 OLE DB Provider for SQL Server 时指定。  
  
`[ @fCreateCookie = ] true | false` 指定是否要创建 cookie。 **true** 将隐式转换为1。 **false** 将隐式转换为0。  
  
`[ @cookie = ] @cookie OUTPUT` 指定包含 cookie 的输出参数。 仅当 **\@ fCreateCookie** 的值为 **true** 时，才生成 cookie。 varbinary(8000)  
  
> [!NOTE]  
> **sp_setapprole** 的 cookie **OUTPUT** 参数现记载为 **varbinary(8000)** ，这是正确的最大长度。 但是，目前执行返回 **varbinary(50)** 。 应用程序应继续保留 **varbinary (8000)** 这样，如果 cookie 在将来的版本中返回大小增加，应用程序将继续正常运行。
  
## <a name="return-code-values"></a>返回代码值

 0 (成功) 和 1 (失败)   
  
## <a name="remarks"></a>注解

 使用 **sp_setapprole** 激活应用程序角色后，该角色将保持活动状态，直到用户从服务器断开连接或执行 **sp_unsetapprole** 。 **sp_setapprole** 只能由直接 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在即席级别执行，而不能在另一个存储过程、触发器或用户定义的事务内执行。  
  
 有关应用程序角色的概述，请参阅 [应用程序角色](../../relational-databases/security/authentication-access/application-roles.md)。  
  
> [!IMPORTANT]  
> 若要在通过网络传输应用程序角色密码时对其进行保护，在启用应用程序角色时，应始终使用加密连接。
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] **SqlClient** 不支持 ODBC **encrypt** 选项。 如果必须存储凭据，请使用加密 API 函数对这些凭据进行加密。 参数 *password* 作为单向哈希进行存储。 为了保持与早期版本的兼容性 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，不会 **sp_addapprole** 强制密码复杂性策略。 若要强制实施密码复杂性策略，请使用 [创建应用程序角色](../../t-sql/statements/create-application-role-transact-sql.md)。  
  
## <a name="permissions"></a>权限

要求对角色的密码进行 **公共** 和了解。  
  
## <a name="examples"></a>示例  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>A. 激活应用程序角色但不使用加密选项

 以下示例使用明文密码 `SalesAppRole` 激活名为 `AsDeF00MbXX` 的应用程序角色，该密码是使用特别为当前用户使用的应用程序设计的权限创建的。

```sql
EXEC sys.sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO
```

### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. 使用 cookie 激活应用程序角色并恢复到原始上下文中

 以下示例使用密码 `Sales11` 激活 `fdsd896#gfdbfdkjgh700mM` 应用程序角色并创建一个 cookie。 该示例返回当前用户的名称，然后通过执行 `sp_unsetapprole` 恢复到原始上下文中。  

```sql
DECLARE @cookie varbinary(8000);  
EXEC sys.sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sys.sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.
GO
```

## <a name="see-also"></a>另请参阅

 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)
