---
description: CREATE APPLICATION ROLE (Transact-SQL)
title: CREATE APPLICATION ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLICATION_ROLE_TSQL
- CREATE APPLICATION ROLE
- sql13.swb.applicationrole.permissions.f1
- APPLICATION
- APPLICATION ROLE
- CREATE_APPLICATION_ROLE_TSQL
- APPLICATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE APPLICATION ROLE statement
- application roles [SQL Server], creating
ms.assetid: 647386da-ee80-41cf-86c9-dd590f9d66b6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 61ea2134ab91109434aab73072f8befb3d6bd29f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205805"
---
# <a name="create-application-role-transact-sql"></a>CREATE APPLICATION ROLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  向当前数据库中添加应用程序角色。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
CREATE APPLICATION ROLE application_role_name   
    WITH PASSWORD = 'password' [ , DEFAULT_SCHEMA = schema_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 application_role_name  
 指定应用程序角色的名称。 该名称一定不能被用于引用数据库中任何主体。  
  
 PASSWORD ='password'  
 指定数据库用户将用于激活应用程序角色的密码。 应始终使用强密码。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。  
  
 DEFAULT_SCHEMA =_schema\_name_  
 指定服务器在解析该角色的对象名时将搜索的第一个架构。 如果未定义 DEFAULT_SCHEMA，则应用程序角色将使用 DBO 作为其默认架构。 schema_name 可以是数据库中不存在的架构。  
  
## <a name="remarks"></a>注解  
  
> [!IMPORTANT]  
>  当设置应用程序角色密码时，将检查密码复杂性。 调用应用程序角色的应用程序必须存储其密码。 而且应当始终以加密的形式存储应用程序角色密码。  
  
 在 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 目录视图中可以查看应用程序角色。  
  
 有关如何使用应用程序角色的信息，请参阅[应用程序角色](../../relational-databases/security/authentication-access/application-roles.md)。  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY APPLICATION ROLE 权限。  
  
## <a name="examples"></a>示例  
 以下示例创建名为 `weekly_receipts` 的应用程序角色，该角色使用密码 `987Gbv876sPYY5m23` 和 `Sales` 作为其默认架构。  
  
```sql  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987G^bv876sPY)Y5m23'   
    , DEFAULT_SCHEMA = Sales;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [应用程序角色](../../relational-databases/security/authentication-access/application-roles.md)   
 [sp_setapprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [ALTER APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/alter-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [密码策略](../../relational-databases/security/password-policy.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
