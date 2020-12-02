---
description: SETUSER (Transact-SQL)
title: SETUSER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e542bb0ef16017744c7f5f61d7358ec66149e1e8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88478661"
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  可使 sysadmin 固定服务器角色的成员或 db_owner 固定数据库角色的成员模拟另一用户。  
  
> [!IMPORTANT]  
>  包含 SETUSER 只是为了保持向后兼容性。 在以后的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中可能不再支持 SETUSER。 建议改用 [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *username* **'**  
 当前数据库中被模拟的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户名或 Windows 用户名。 如果未指定 username，将重置模拟用户的系统管理员或数据库所有者的原始标识。  
  
 WITH NORESET  
 指定后续 SETUSER 语句（没有指定的 username）不应将用户标识重置为系统管理员或数据库所有者。  
  
## <a name="remarks"></a>备注  
 为测试其他用户的权限，sysadmin 固定服务器角色的成员或数据库所有者可以使用 SETUSER 来使用另外一个用户的标识。 具有 db_owner 固定数据库角色中的成员身份还不够。  
  
 仅对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户使用 SETUSER。 不支持 Windows 用户使用 SETUSER。 如果使用 SETUSER 来模拟其他用户的标识，则进行模拟的用户创建的任何对象均由被模拟的用户所有。 例如，如果数据库所有者模拟了用户 Margaret 的标识并创建了一个名为 orders 的表，则 orders 表将归 Margaret 所有，而不归系统管理员所有。  
  
 SETUSER 一直保持有效，直到发出其他 SETUSER 语句或用 USE 语句更改当前数据库为止。  
  
> [!NOTE]  
>  如果使用了 SETUSER WITH NORESET，数据库所有者或系统管理员必须注销然后重新登录，才能重新建立自己的权限。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色的成员身份或数据库所有者身份。 具有 db_owner 固定数据库角色中的成员身份还不够  
  
## <a name="examples"></a>示例  
 以下示例显示了数据库所有者如何采用其他用户的标识。 用户 `mary` 已创建了一个名为 `computer_types` 的表。 通过使用 SETUSER，数据库所有者可以模拟 `mary` 授予用户 `joe` 访问 `computer_types` 表的权限，然后重置自己的标识。  
  
```sql
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)  
  
  
