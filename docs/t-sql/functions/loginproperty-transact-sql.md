---
description: LOGINPROPERTY (Transact-SQL)
title: LOGINPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BadPasswordCount_TSQL
- BadPasswordTime_TSQL
- IsLockedIsMustChange
- PasswordLastSetTime
- LOGINPROPERTY_TSQL
- LockoutTime
- BadPasswordCount
- PasswordHash
- HistoryLengthIsExpired
- LockoutTime_TSQL
- PasswordHash_TSQL
- HistoryLengthIsExpired_TSQL
- PasswordLastSetTime_TSQL
- BadPasswordTime
- IsLockedIsMustChange_TSQL
- LOGINPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- default database
- LOGINPROPERTY function
ms.assetid: b34df777-79b0-49a5-88db-b99998479a5d
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: c31812d98ddb8315adfe86c17f880e757ce25197
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836909"
---
# <a name="loginproperty-transact-sql"></a>LOGINPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  返回有关登录策略设置的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
LOGINPROPERTY ( 'login_name' , 'property_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 login_name  
 将返回登录属性状态的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 propertyname  
 一个表达式，包含要为登录名返回的属性信息。 propertyname 可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**BadPasswordCount**|返回尝试以不正确密码连续登录的次数。|  
|**BadPasswordTime**|返回上次尝试以不正确密码登录的时间。|  
|**DaysUntilExpiration**|返回密码过期前的天数。|  
|**DefaultDatabase**|返回存储在元数据中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的默认数据库；如果未指定数据库，则返回 master 数据库。 为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的用户（例如经过身份验证的 Windows 用户）返回 NULL。|  
|**DefaultLanguage**|返回存储在元数据中的登录默认语言。 为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的用户（例如经过身份验证的 Windows 用户）返回 NULL。|  
|**HistoryLength**|返回使用密码策略强制机制跟踪登录名的密码数。 如果未强制执行密码策略，则为 0。 恢复密码策略实施将从 1 重新开始。|  
|**IsExpired**|指示登录名是否已过期。|  
|**IsLocked**|指示登录名是否已锁定。|  
|**IsMustChange**|指示登录名在下次连接时是否必须更改其密码。|  
|**LockoutTime**|返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名由于超过允许的失败登录尝试次数而被锁定的日期。|  
|**PasswordHash**|返回密码的哈希。|  
|**PasswordLastSetTime**|返回设置当前密码时的日期。|  
|**PasswordHashAlgorithm**|返回用于对密码执行哈希操作的算法。|  
  
## <a name="returns"></a>返回  
 数据类型取决于所请求的值。  
  
 IsLocked、IsExpired 和 IsMustChange 的类型为 int。  
  
-   1（如果登录名处于指定状态）。  
  
-   0（如果登录名不处于指定状态）。  
  
 BadPasswordCount 和 HistoryLength 的类型为 int。  
  
 BadPasswordTime、LockoutTime、PasswordLastSetTime 的类型为 datetime。  
  
 PasswordHash 的类型为 varbinary。  
  
 NULL（如果登录名不是有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名）。  
  
 DaysUntilExpiration 的数据类型为 int。  
  
-   如果登录名已过期或者它将在查询的当日过期，则为 0。  
  
-   如果 Windows 中的本地安全策略使密码永不过期，则为 -1。  
  
-   如果登录名的 CHECK_POLICY 或 CHECK_EXPIRATION 设置为 OFF，或者操作系统不支持该密码策略，则为 NULL。  
  
 PasswordHashAlgorithm 的数据类型为 int。  
  
-   0（如果是 SQL7.0 哈希）  
  
-   1（如果是 SHA-1 哈希）  
  
-   2（如果是 SHA-2 哈希）  
  
-   NULL（如果登录名不是有效的 SQL Server 登录名）  
  
## <a name="remarks"></a>备注  
 此内置函数返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的密码策略设置的信息。 属性名称不区分大小写，因此 BadPasswordCount 和 badpasswordcount 等属性名称是等效的。 PasswordHash、PasswordHashAlgorithm 和 PasswordLastSetTime 属性值在所有支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置中都可用，但仅当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] 上运行并且同时启用了 CHECK_POLICY 和 CHECK_EXPIRATION 时，其他属性才可用。 有关详细信息，请参阅 [Password Policy](../../relational-databases/security/password-policy.md)。  
  
## <a name="permissions"></a>权限  
 需要对登录名具有 VIEW 权限。 请求密码哈希时，还需要 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-checking-whether-a-login-must-change-its-password"></a>A. 检查登录名是否必须更改其密码  
 以下示例检查 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `John3` 在下次连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时是否必须更改其密码。  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsMustChange');  
GO  
```  
  
### <a name="b-checking-whether-a-login-is-locked-out"></a>B. 检查登录名是否已锁定  
 以下示例检查 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `John3` 是否已锁定。  
  
```sql  
SELECT LOGINPROPERTY('John3', 'IsLocked');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
  
